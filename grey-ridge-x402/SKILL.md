---
name: grey-ridge-x402
description: Base-native x402 data for AI agents — pay-per-call on Base USDC, no account or API key. Use when an agent needs Base-chain RPC reads (block number, gas price, ETH/ERC-20 balances, tx, receipt, wallet bundle — all enriched with USD values), crypto prices, cross-venue funding-rate arbitrage, DeFi yields, live Polymarket markets, or an MCP-server security audit. Base-native (most x402 data sellers are Ethereum-only) and USD-enriched. Triggers on Base RPC, gas price, wallet balance, token balance, on-chain reads, funding rate, DeFi yield, Polymarket, prediction markets, crypto prices, or MCP security scan.
---

# Grey Ridge Signals — Base-native x402 Data

Agent-native, pay-per-call data on **Base** (USDC via x402 — no account, no API key). Most x402 data sellers are Ethereum-only; this is **Base-first**, with **USD values** on chain reads.

## What it returns
- **Base RPC** ($0.001/call): block number, gas price (EIP-1559 base/priority + `gas_price_usd`), ETH & ERC-20 balances (+ `balance_usd`), tx, receipt (+ Base L1-fee breakdown + `l1_fee_usd`), wallet bundle, contract / EIP-7702 detection.
- **Crypto signals**: spot prices (+ 24h change), cross-venue funding-rate arbitrage (Hyperliquid / OKX / dYdX, annualized + signal), DeFi yields (+ risk-adjusted apy/sigma).
- **Prediction markets**: live Polymarket markets (bestBid/ask/spread, 24h volume, tags).
- **Security**: on-demand MCP-server audit (tool-poisoning / prompt-injection; verdict clear|review|block).

## Access
- **x402 (no account)**: pay $0.001-$0.10 USDC per call on Base — the agent pays inline, no API key.
- **Free preview** on the hot reads: `curl -s https://x402-data-api.sigrunner.workers.dev/chain/gas-price/preview`

## MCP install (20 tools)
```json
{"mcpServers":{"grey-ridge-x402":{"type":"streamable-http","url":"https://x402-data-api.sigrunner.workers.dev/mcp"}}}
```

## Research -> Execute
Grey Ridge is the **Base data layer** (RPC + USD + market signals): read chain state, price wallets in USD, find funding-rate / yield / prediction-market opportunities — then hand off to Bankr for **execution** (swaps, orders).

Docs: https://x402-data-api.sigrunner.workers.dev/llms.txt - OpenAPI: `/openapi.json` - MCP Registry: `io.github.rezearcher/tech-risk`
