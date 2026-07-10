# LoneStarOracle — Full Service Catalog

43 live pay-per-call APIs on Base. Pattern: `https://<sub>.lonestaroracle.xyz<endpoint>`. Every call is USDC over x402 (HTTP 402 → pay → retry → JSON). No account, no API key.

## Crypto & DeFi Risk · On-chain Intelligence
- **DeFiRisk** — `defi…/risk?protocol=<name>` — $0.10 — DeFi/RWA protocol risk score
- **StablePulse** — `stable…/pulse` (also `/symbol/{symbol}`, `/risk-summary`) — $0.05 — stablecoin depeg risk & health
- **CascadeWatch** — `cascade…/risk` (also `/cascade`, `/report`) — $0.10 — systemic / contagion risk
- **TokenScope** — `token…/report?address=<contract>` — $0.15 — token safety / risk scan
- **WalletIntel** — `wallet…/score?address=<wallet>` — $0.15 — wallet risk score
- **WalletPnL** — `pnl…/pnl?address=<wallet>` — $0.20 — wallet profit-and-loss
- **ContractCheck** — `contract…/verify?address=<contract>` — $0.05 — smart-contract verification & safety
- **ChainScout** — `chainscout…/report` (also `/whales`, `/trending`, `/tvl`, `/narrative`) — $0.05 — on-chain intel bundle
- **WhaleAlert** — `whale…/whales` — $0.05 — large on-chain transaction alerts
- **BundleScope** — `bundle…/scan?token=<contract>` — $0.10 — token bundle / sniper detection
- **TokenLaunches** — `launches…/scan` — $0.05 — new token launch scanner
- **StakeEdge** — `stake…/report` — $0.05 — staking yields & validator data

## Derivatives & Market Structure
- **FundingRates** — `funding…/rates` (also `/extremes`, `/signal`) — $0.05 — perp funding rates
- **OpenInterest** — `oi…/oi` (also `/extremes`, `/signal`) — $0.05 — open interest
- **Liquidations** — `liq…/liquidations` (also `/biggest`, `/signal`) — $0.10 — liquidation data & signal
- **OptionsFlow** — `options…/flow?symbol=<ticker>` — $0.05 — options flow (crypto + equities)
- **TechAnalysis** — `ta…/scan?symbol=<ticker>` — $0.05 — technical-analysis scan

## Equities & Finance
- **EquityScope** — `equity…/equity?symbol=<ticker>` — $0.05 — equity signal + AI analysis
- **EarningsCalendar** — `earnings…/calendar` — $0.03 — upcoming earnings
- **InsiderFlow** — `insider…/trades` — $0.03 — corporate insider trades
- **PortfolioRisk** — `portfolio…/analyze` — $0.10 — portfolio risk analysis
- **WealthPulse** — `wealth…/analyze` — $0.25 — wealth & macro allocation analysis

## Macro · Commodities · Real Economy
- **MacroPulse** — `macro…/macro` — $0.05 — macro indicators & regime signal
- **IndustrialMetals** — `metals…/report` — $0.05 — industrial metals prices & signals
- **SupplyChainPulse** — `supply…/report` — $0.05 — global supply-chain stress
- **AgriPulse** — `agri…/report` — $0.03 — agricultural commodity prices
- **GridPulse** — `grid…/report` — $0.03 — US electricity grid demand & stress
- **ComputePulse** — `compute…/report` — $0.05 — AI compute / GPU market
- **CrownBlock** — `crownblock…/report` — $1.00 — oil, gas & refined products
- **LatAmPulse** — `latam…/report` — $0.05 — Latin America markets & macro
- **RealEstatePulse** — `realestate…/report` — $0.03 — mortgage & housing data
- **LeaseEdge** — `lease…/report` — $0.15 — commercial lease & property data

## Security Audits (POST)
- **RattlerAI** — `rattler…/audit` (POST) — $2.00 — autonomous smart-contract audit (EVM)
- **CottonmouthAI** — `cottonmouth…/audit` (POST) — $2.00 — autonomous smart-contract audit
- **CopperheadAI** — `copperhead…/audit` (POST) — $2.00 — autonomous smart-contract audit

## News · Intelligence · Utility
- **NewsSentiment** — `news…/news` — $0.05 — crypto & market news sentiment
- **GeoPulse** — `geo…/risk` — $0.07 — geopolitical risk signal
- **GovEdge** — `govedge…/report` (also `/opportunities` — SAM.gov solicitations) — $0.20 — US government contracts
- **AeroCheck** — `aero…/pool` — $0.05 — aviation / airfare pool data
- **WeatherOracle** — `weather…/forecast` — $0.02 — 7-model consensus weather forecast
- **ContentForge** — `content…/repurpose` (POST) — $0.15 — repurpose a URL into posts/threads/newsletter
- **DocEdge** — `doc…/convert` (POST) — $0.05 — document conversion
- **Floyd** — `floyd…/hire` (POST) — $0.50 — hire Floyd, an autonomous AI research & coding agent
