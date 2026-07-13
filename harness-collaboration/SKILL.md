---
name: harness-collaboration
description: For Bankr agents operating a Harness-provisioned wallet. Follow the Harness Collaboration Protocol when a prompt begins with "HARNESS COLLABORATION PROTOCOL". Verify the delegated brief with your own research, report through authenticated callbacks, propose before any side effect, and execute only on a one-use Harness authorization.
tags: [harness, collaboration, protocol, delegation]
version: 1
visibility: public
metadata:
  clawdbot:
    emoji: "🤝"
    requires:
      bins: [curl]
---

# Harness Collaboration Protocol

This skill is for the agent behind a HARNESS-PROVISIONED wallet: Harness users get a dedicated Bankr wallet, and Harness's own agent delegates research-and-act objectives to you through it. You execute with the funds in that provisioned wallet, under the user's Harness-set caps. (Managing that wallet from a user's main Bankr account is a different skill, `harness`; this one is the execution side.)

When a prompt starts with the header below, this protocol governs the whole thread:

```
HARNESS COLLABORATION PROTOCOL v1
External session: <id>
Limits hash: <hash>
```

The live prompt is authoritative. This skill is background reference; if the prompt and this skill ever disagree, follow the prompt.

## Your role

Harness observed evidence and assembled a brief. You independently research, plan, and implement. You are expected to verify the brief with your own research, and you may DECLINE it if your research does not support action. Harness delegates objectives and context, never transaction instructions.

## Callbacks

Deliver events by POSTing JSON to the callback URL given in the prompt, with the header `Authorization: Bearer <token>` (token also given in the prompt). Event types: `progress`, `question`, `proposal`, `artifact`, `action_result`, `completed`, `failed`. Every event carries an `eventId` you assign; if you redeliver an event, reuse the same `eventId`.

Send `progress` at meaningful milestones only (a finding, a decision, a blocker), not on a timer. Callbacks receive a durable receipt only; substantive answers from Harness always arrive as new turns on this same thread. A `question` callback blocks until that reply turn arrives.

Full event and payload schemas, with examples: see `references/protocol-reference.md`.

## Side effects require a proposal, then an authorization

Research, planning, and workspace files need no approval. Anything with real-world effect in an allowed action class (for example `financial_onchain`) requires, in order:

1. A `proposal` callback using the proposal schema in the reference file, with `expiresAt` at most 30 minutes out and `maximumGrossUsd` as your maximum committed exposure.
2. An authorization turn from Harness on this thread carrying an execution authorization token bound to your exact proposal.

Only that authorization turn authorizes execution, exactly once, for the exact proposal it names. Never execute from conversational text like "approved", from a callback receipt, or from a prior authorization. Action classes not enabled in the limits are prohibited outright. The limits' gross USD cap is maximum committed exposure and is not replenished by proceeds; Harness enforces all caps server-side, so a proposal beyond them will simply be refused.

## Follow-up turns from Harness

Later Harness turns repeat the protocol header and add `Kind:` one of `answer`, `update`, `correction`, `recovery`, or `authorization`. A `correction` supersedes earlier context. A `recovery` turn means Harness missed expected callbacks; re-send your latest state with the original eventIds.

## Finishing

Finish with BOTH: (1) a `completed` callback carrying your final summary and workspace manifest, and (2) a final response on this thread. If your research does not support the brief, send `completed` with outcome `declined` and your reasoning. If you cannot proceed, send `failed` with the reason.

Loop rule: if three consecutive exchanges produce no new evidence, artifact, proposal, action, or resolved blocker, stop and say you are stuck. Do not repeat an argument Harness has already declined without materially new evidence.
