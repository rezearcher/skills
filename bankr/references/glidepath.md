# Glidepath Reference

Glidepath is an AI-paced, gradual exit for token builders. Instead of dumping creator-fee tokens into the pool in one trade — which craters the chart and wrecks holder trust — you commit a slice of your own tokens to be sold back into the pool in tiny, liquidity-sized increments over time. The chart sees ordinary, absorbable sell pressure instead of a cliff.

Glidepath is managed from your token page at [bankr.bot](https://bankr.bot). It is available for tokens you launched through Bankr (or that you are a fee beneficiary on).

## What it is — and isn't

- **It does not pull liquidity.** Glidepath never touches your pool's LP position. Only the tokens you explicitly commit are sold.
- **You stay in control.** You decide if, when, and how much to commit. Nobody forces a sell.
- **It is public and capped.** Once you commit, the rules are fixed and visible to holders: committed / sold / remaining are shown as a live exit envelope on your token page.

## How it works

1. **Preview.** From your token page, enter how much of your token you'd consider gliding out. You'll see what that stack is worth today and what it would be worth at higher market caps — a what-if value preview, not a sell target or prediction.
2. **Commit.** When ready, confirm. Committed tokens are locked to a vesting wallet.
3. **Heads-up window.** Selling does not start immediately. After a short window (about 48 hours), the glide begins selling from the **current** market cap — not from the higher caps shown in the preview.
4. **AI-paced slices.** The AI reads the pool's live liquidity and trading volume to size each sale and its cadence. Every slice is capped to a fraction of real liquidity (the AI can size under the cap, never over), so each sell barely moves the chart. Slice timing is fuzzed so it can't be front-run.
5. **Transparent envelope.** Your token page shows committed, sold, and remaining amounts so holders always see the full picture.

## Cancelling

You can cancel at any time; sells stop immediately when you do.

| When you cancel | What happens |
|-----------------|--------------|
| Within ~30 minutes of committing (before selling starts) | Instant undo — committed tokens return to your wallet right away |
| After selling has started | Sells stop immediately; any **unsold** tokens return to your wallet after a 7-day cooldown |

The 7-day cooldown is public — holders see the cancellation before you regain custody. Tokens already sold during the glide can't be reclaimed; those proceeds are yours.

## Why selling your own fee token routes through Glidepath

Fee beneficiaries can't sell their own fee token through Bankr's ordinary swap / limit / stop / DCA / TWAP tools — those sells are intentionally blocked on the sell side. To take profit on a token you earn fees in, use a Glidepath instead. This keeps builder exits gradual, capped, and transparent by design.

## Bottom line

- **Creators** take profit on their own terms, chart and reputation intact.
- **Holders** see the builder's exit as a known, capped, visible number instead of a hidden overhang and a surprise dump.

## Related

- [Token Deployment](token-deployment.md) — launch a token and earn creator fees
- Public docs: https://docs.bankr.bot/token-launching/glidepath
