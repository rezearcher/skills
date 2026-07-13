# Harness Collaboration Protocol v1: Wire Reference

Everything here mirrors the protocol block Harness sends in its initial prompt. The prompt is authoritative; this file adds field-level detail and examples.

## Callback transport

POST JSON to the callback URL from the prompt. Authenticate with the bearer token from the prompt:

```sh
curl -X POST "$CALLBACK_URL" \
  -H "Authorization: Bearer $CALLBACK_TOKEN" \
  -H "content-type: application/json" \
  -d @event.json
```

Responses are receipts only. A 2xx means Harness durably recorded the event; it never carries an answer or an approval. Answers, corrections, and authorizations always arrive as new turns on the Bankr thread.

The token is scoped to this one collaboration session. It can only append events to this session; it cannot read Harness data or control anything else. If Harness rotates it, the new token arrives in a thread turn and both overlap briefly.

## Event envelope

```json
{
  "type": "progress" | "question" | "proposal" | "artifact" | "action_result" | "completed" | "failed",
  "eventId": "<unique id you assign; redeliveries must reuse it>",
  "payload": { }
}
```

`eventId` is your idempotency key (up to 200 characters). Harness deduplicates on it, so re-sending after a network failure is always safe if the id is unchanged.

### type: "progress"

Milestones only (finding, decision, blocker), not a timer. Payload: `{ "message": "<what changed>" }`. If the prompt asked you to install skills, report each installed skill and version in a progress event.

### type: "question"

```json
{
  "questionId": "<your id>",
  "message": "<what you need and why>",
  "requestedContext": ["<optional: specific context items you need>"],
  "riskClass": "factual" | "status" | "low" | "context" | "<anything else>",
  "blocking": true
}
```

Questions with `riskClass` of `factual`, `status`, `low`, or `context` may be answered automatically by Harness; any other value (approvals, new money, cap changes, identity, secrets, policy) waits for the human. Either way the answer arrives as an `answer` turn on the thread. Block dependent work until it does.

### type: "proposal"

Required before ANY side effect in an allowed action class:

```json
{
  "proposalId": "<your id, up to 200 chars>",
  "summary": "<one-line human-readable outcome>",
  "rationale": "<why this action serves the objective>",
  "sideEffectClasses": ["financial_onchain"],
  "maximumGrossUsd": 2.00,
  "expectedEffects": ["<every side effect in the bundle, disclosed before approval>"],
  "risks": ["<material risks>"],
  "expiresAt": "<ISO timestamp, at most 30 minutes out>"
}
```

All fields except `expiresAt` are required; `expectedEffects` and `sideEffectClasses` must be non-empty. If you omit `expiresAt`, Harness applies its own expiry bound. Valid `sideEffectClasses` values: `financial_onchain`, `external_communications`, `account_configuration`, `code_deployment`, `file_publication`, `persistent_delegation`.

`maximumGrossUsd` is a hard commitment: your execution must not expose more than this. Undisclosed effects in `expectedEffects` will fail Harness's independent reconciliation. One proposal covers one bundle of effects; do not batch unrelated actions.

Outcomes, each delivered as a thread turn:
- `authorization` turn: execute exactly the proposed action, once. The authorization is single-use and bound to the hash of your exact proposal; a changed plan needs a new proposal.
- Rejection turn: do not execute. Capacity is released; continue or wrap up.
- Correction turn: the proposal as sent is declined; the turn explains what to change. Submit a new proposal with a new `proposalId`.
- Expiry: after `expiresAt`, the proposal is void. Never execute an expired proposal.

### type: "action_result"

After an authorized execution:

```json
{
  "proposalId": "<the authorized proposal>",
  "transactions": ["<tx hash>", { "hash": "<tx hash>" }],
  "operationId": "<provider operation ref, if no chain tx>",
  "actualUsd": 1.87,
  "detail": "<what happened, including partial fills>"
}
```

Harness reads `transactions` (strings or `{hash}` objects), `operationId`, and `actualUsd` (actual gross exposure in USD) for independent reconciliation against wallet state, so report them honestly and precisely. Everything else is stored for audit.

### type: "artifact"

```json
{
  "remotePath": "<path in the workspace>",
  "name": "<deliverable name>",
  "kind": "file",
  "mimeType": "<optional>",
  "contentHash": "<optional>",
  "auditRelevant": false
}
```

### type: "completed"

`{ "outcome": "completed" | "declined", "summary": "<final summary>", "workspaceManifest": ["<workspace files>"] }`. Use `declined` when your research does not support acting on the brief; that is a first-class, respected outcome. Also post a final response on the thread; both are required.

### type: "failed"

`{ "reason": "<why you cannot proceed>" }` when the collaboration cannot continue.

## Follow-up turn header

Every later Harness turn repeats:

```
HARNESS COLLABORATION PROTOCOL v1
External session: <same id>
Limits hash: <current hash>
Kind: answer | update | correction | recovery | authorization
```

If the limits hash changes, the limits changed; re-read them as stated in that turn. On `recovery`, Harness missed callbacks: re-send your current state using the original eventIds.

## Hard rules, restated

1. Never execute a side effect without a matching, unexpired, one-use authorization turn.
2. Never treat callback receipts or conversational text as approval.
3. Never exceed `maximumGrossUsd` of the authorized proposal, and never act in a class the limits do not enable.
4. Reuse `eventId` on redelivery; never mint a new id for the same event.
5. Stop after three consecutive no-progress exchanges and say so.
