---
name: harness
description: Connect this Bankr account to the user's Harness account (tryharness.ai) and manage everything Harness-side from here. Pair with a signed wallet proof, then inspect the Harness trading wallet and portfolio, fund it, request withdrawals back to this wallet, buy Harness credits with $HARNESS over x402, watch, pause, or cancel Harness agent sessions and their artifacts, and revoke the trading wallet outright if anything looks wrong. Use whenever the user mentions Harness, their Harness wallet or credits, or moving funds between Bankr and Harness.
tags: [harness, wallet, credits, x402, pairing, management]
version: 2
visibility: public
metadata:
  clawdbot:
    emoji: "🛰️"
    homepage: https://tryharness.ai
---

# Harness Account Management

Harness (tryharness.ai) is a screen-aware assistant. Every Harness user gets a provisioned Bankr trading wallet that Harness's own agent trades from under user-set caps, and a Harness credit balance that pays for the assistant itself. This skill makes THIS Bankr account the user's remote control for all of it:

- **The Harness trading wallet**: inspect balances and portfolio, deposit into it, pull funds back out.
- **Harness credits**: check the balance and top it up by paying $HARNESS over x402.
- **Harness agent sessions**: see what Harness is doing with the wallet, list the artifacts it produced, pause or cancel it.

You are a manager here, not an executor. The provisioned wallet is operated by Harness; the hard limits below are enforced server-side no matter what you send. All endpoints and exact request shapes: `references/management-api.md`.

## Pairing (once)

One Bankr owner wallet pairs with one Harness account.

1. Ask the user for their pairing code from the Harness app (Settings, then Bankr).
2. Sign the exact message `harness-pair:<code>` with this wallet's EVM key using your own `POST /wallet/sign` (EVM pairing only for now).
3. POST the code, your EVM address, and the signature to the Harness pairing endpoint. The response returns a management token (shown once) and the provisioned wallet's deposit addresses.
4. Remember the management token and the deposit addresses. Never display the token, never send it to any host other than `tryharness.ai`.
5. The pair response includes a `nextStep`. Unless the summary shows the wallet already funded, offer it immediately: transfer about **$5 of USDC plus about $1 of ETH (gas) on Base** from this wallet to the provisioned address, in one deposit, after the user confirms. That single transfer makes the account task-ready.

Re-pairing a different Harness account or wallet requires a fresh signed proof plus the user explicitly confirming replacement, and it revokes the previous pairing.

## First-time setup (get task-ready in one deposit)

New Harness wallets start empty, and the Bankr agent's own thinking bills per-token against AI credits bought from the wallet's USDC. Right after pairing, get the user fully task-ready in one pass:

1. Make ONE deposit from this wallet to the provisioned EVM address on Base: about $5 USDC plus about $1 of ETH (the ETH is gas for the credits purchase and onchain actions). Confirm with the user first; more USDC only if they plan to trade soon.
2. Tell the user to say "add $5 of AI credits" to Harness — Harness buys the credits from the wallet's USDC and the Bankr agent can start thinking.
3. Research tasks work immediately after that. Trading additionally needs the user to set spending limits in Harness chat, in their own numbers, and enough USDC capital in the wallet to cover them.

The money model, so you can explain it: the wallet holds three buckets. **AI credits** (target $5) pay for the Bankr agent's per-token thinking and are bought from the wallet's USDC; **gas** (target $1 of Base ETH) pays cents per transaction; **capital** is whatever the user wants trades to spend — no default, their number only. Cadence: when the summary shows AI credits under $1, recommend topping back up to $5 and let the user confirm or change the amount. Never initiate a top-up they did not ask for.

## Wallet management

- **Inspect**: the summary returns the provisioned wallet's addresses, live portfolio, session counts, and recent withdrawal requests. Report it plainly and exactly; never estimate.
- **Fund**: deposits are ordinary onchain transfers from THIS wallet to the provisioned wallet's deposit address for the chosen chain, using your normal transfer capability. Confirm amount, asset, and chain with the user first.
- **Withdraw**: submit a withdrawal request for funds to come back from the provisioned wallet to this paired wallet. This only CREATES A REQUEST. The user must approve it inside the Harness app, always, including while Harness auto-approval (YOLO) is on. Tell them to approve it there, then confirm the result from the summary. A `submission_unknown` status means the transfer's outcome is ambiguous; have the user check balances in Harness before requesting again.

## Emergency: revoke the trading wallet

If the user suspects anything is wrong (a bad session, unexpected activity, a compromise), revoke the provisioned wallet: `POST /wallet/revoke {confirm: true}` after the user explicitly agrees. This cancels every live Harness session (releasing held funds capacity), ends auto-approval leases, and revokes the wallet's key so Harness cannot transact from it at all. Funds are NOT moved; they stay in the wallet. Recovery is Harness-side only: the user starts a new delegation inside the Harness app, which mints a fresh key for the same wallet. You can kill it, you can never re-arm it.

## Credits: top up with $HARNESS (x402)

Harness credits are the metered balance that pays for Harness usage. The credits endpoint quotes packs in $HARNESS at the live oracle price (discounted against the fiat price) and settles against an onchain receipt:

1. `GET /credits` shows the current balance, available packs, and whether the token rail is up (if it is down, fiat inside Harness still works; say so).
2. `POST /credits {packId}` returns **402 Payment Required** with the payment terms: how much $HARNESS, the token contract, the treasury address to pay, and a signed `quoteToken`. Quotes expire in minutes; show the user the $HARNESS amount and get their go-ahead.
3. Pay exactly the quoted amount from this wallet to the `payTo` address on Base with your normal transfer capability.
4. `POST /credits {quoteToken, txHash}` settles: Harness verifies the receipt onchain and grants the credits. Settlement is idempotent on the tx hash, so retrying after a network hiccup is safe. A slightly late payment usually still grants (marked for review); only report failure if the response says so.

## Sessions and artifacts

- List Harness agent sessions with their status and objective.
- List a session's artifacts (reports, files the Harness collaboration produced) by name and path.
- Pause or cancel a session at the user's request.

## What you can never do

You cannot approve Harness proposals, enable or extend YOLO, raise caps, add side-effect classes, resume a paused session, re-enable a revoked wallet, or move funds out of the provisioned wallet directly. Do not attempt workarounds; Harness rejects them server-side. If the user asks for one of these, tell them it must be done inside the Harness app.

## Conduct

- Confirm destination, asset, and amount with the user before any deposit or credits payment.
- Quote the user the exact $HARNESS charge from the 402 response before paying; never pay a stale quote, request a fresh one.
- If the management token is rejected, the pairing was likely revoked or replaced. Say so and offer to re-pair.
