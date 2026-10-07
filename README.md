# FOMY website

A static site: no server, no build step. CoinGecko is the only data source, used without a key.
Deploy the same way as before: put these files at the top level of your GitHub repo and import it into Vercel with Framework Preset "Other".

Everything you edit is in the `SITE` block at the top of the `<script>` in `index.html`.

## Launch day
Set `ca: "YourSolanaMint"`. This switches on the header coin button, the copy buttons, the hero price and the live "$FOMY, live" card.

## Callouts
Every hour the page scans Pump.fun launches from CoinGecko's onchain data, using four lists: trending Solana pools, the busiest pump.fun pools, the busiest PumpSwap pools, and the newest pools.
- **Filter:** each token has to be 20 minutes to 24 hours old, hold at least $8k liquidity, and have 25+ buyers in the last hour.
- **Score:** the survivors are ranked by volume, unique buyers and buy/sell balance. Vertical 5-minute candles and dumping tokens are penalised.
- **Write-up:** FOMY writes a read for the top 3: what's working, red flags, and what has to be true. He labels each "Worth watching", "Early, unproven" or "Handle with tongs". He never says buy.

Tune the thresholds in `SITE.callouts`. Add words or mint addresses to `blockWords` / `blockMints` to keep tokens off the page, for example offensive names.

Each visitor's browser does the scan. Two visitors in the same hour may see slightly different picks if the market moved between their visits. A single shared set of callouts plus a history page needs a small scheduled job, such as a GitHub Action, to write them to the repo.

## Replies
Paste links to FOMY's best reply posts into `posts: [...]`. If the list is empty, the section shows the @FomyBot timeline instead (X sometimes only shows that to logged-in visitors).

## Wallet judge
Visitors paste a Solana address. The page then:
1. Reads the wallet's last 100 transactions from a public Solana RPC node. This needs no key. CoinGecko doesn't offer wallet history, so this is the one non-CoinGecko source.
2. Finds the swaps in those transactions and works out habits: win rate, realized PnL, hold times, overtrading, bags never sold, late-night trading, fees, and buying bigger right after a loss.
3. Writes FOMY's verdict with a grade and an archetype, plus a "Share on X" button.

Everything runs in the visitor's browser and nothing is stored. The numbers are approximate. The judge only sees the last 100 transactions, and it skips multi-hop routes, transfers and anything it can't read as a simple swap.

Public RPC nodes are rate-limited. If the judge starts failing under traffic, get a free RPC URL from Helius or QuickNode and put it first in `solanaRpcs`. If the provider offers it, restrict that URL to your domain.
