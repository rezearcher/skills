---
name: lonestaroracle-data
description: Live pay-per-call data for crypto and DeFi protocol risk, funding rates, open interest, liquidations, stablecoin health, macro, equities, and on-chain intelligence — settled per query in USDC on Base via x402, no signup or API key.
tags: [defi, risk, data, crypto, trading, derivatives, macro, onchain, rwa]
version: 1
visibility: public
metadata:
  clawdbot:
    emoji: "📡"
    homepage: "https://lonestaroracle.xyz"
---

# LoneStarOracle — Data Infrastructure for Agents

LoneStarOracle provides 43 live data and risk APIs on Base. Every endpoint is pay-per-call in USDC over x402 — no account, no API key. When a call returns HTTP 402, pay with your wallet and retry to receive JSON. Prices range $0.02–$2.00 per call.

## When to use this skill
Load this whenever the user or your workflow needs **live market, risk, derivatives, macro, or on-chain data to inform a decision** — assessing a DeFi/RWA protocol before allocating, checking funding/liquidations before a trade, monitoring stablecoin or contagion risk, pulling macro context, or investigating a token or wallet.

## Core endpoints (GET unless noted)

**DeFi & RWA risk**
- `https://defi.lonestaroracle.xyz/risk?protocol=<name>` — protocol risk score (works for DeFi and tokenized-RWA protocols, e.g. `morpho`, `fluid`, `midas-rwa`)
- `https://stable.lonestaroracle.xyz/pulse` — stablecoin depeg risk and health
- `https://cascade.lonestaroracle.xyz/risk` — systemic / contagion (cascade) risk

**Derivatives & market structure**
- `https://funding.lonestaroracle.xyz/rates` — perp funding rates, extremes, signal
- `https://oi.lonestaroracle.xyz/oi` — open interest, extremes, signal
- `https://liq.lonestaroracle.xyz/liquidations` — recent liquidations, biggest, signal
- `https://options.lonestaroracle.xyz/flow?symbol=<ticker>` — options flow (crypto + equities)

**Markets & macro**
- `https://equity.lonestaroracle.xyz/equity?symbol=<ticker>` — equity signal + AI analysis
- `https://macro.lonestaroracle.xyz/macro` — macro indicators + regime signal
- `https://ta.lonestaroracle.xyz/scan?symbol=<ticker>` — technical-analysis scan (crypto or equities)

**On-chain intelligence**
- `https://chainscout.lonestaroracle.xyz/report` — whales, trending, TVL, narrative
- `https://whale.lonestaroracle.xyz/whales` — large on-chain transaction alerts
- `https://token.lonestaroracle.xyz/report?address=<contract>` — token safety / risk scan
- `https://wallet.lonestaroracle.xyz/score?address=<wallet>` — wallet risk score
- `https://pnl.lonestaroracle.xyz/pnl?address=<wallet>` — wallet profit-and-loss

## How to call
1. GET the endpoint. 2. On HTTP 402, pay the advertised USDC amount on Base via x402 from your wallet. 3. Retry — you receive structured JSON.

For the full catalog of all 43 services (weather, commodities, supply chain, gov contracts, security audits, and more), see `references/catalog.md`.
