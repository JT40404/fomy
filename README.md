# FOMY website

A static site with no server and no build step. It does three things:

1. **Token tracker:** every hour it scans new Pump.fun launches from CoinGecko and posts FOMY's read on up to three of them.
2. **Wallet judge:** visitors paste a Solana address and FOMY grades their trading habits.
3. **How FOMY replies:** explains that people post their thesis on Fomo and FOMY answers there.

The tracker and judge run on their own, in each visitor's browser. There's nothing to keep running and no API keys to set.

Deploy the same way as before: put these files at the top level of your GitHub repo and push. Vercel redeploys automatically.

## Settings
Everything you edit is in the `SITE` block at the top of the `<script>` in `index.html`.

- `fomoUrl`: link to FOMY's profile on Fomo, already set to https://fomo.family/profile/FomyBot. Once set, the "FOMY on Fomo" header button, the "Open FOMY on Fomo" button and the footer link all point there.
- `ca`: the $FOMY Solana mint address, added on launch day. This switches on the header coin button, the copy buttons, the hero price and the "$FOMY, live" card.
- `callouts`: thresholds for the token tracker (age, liquidity, buyers). Add words or mint addresses to `blockWords` / `blockMints` to keep tokens off the page.
- `solanaRpcs`: the RPC nodes the wallet judge reads from. They're public and keyless by default. If the judge fails under heavy traffic, put a free Helius or QuickNode RPC URL first.
- `coingeckoDemoKey`: optional free CoinGecko key, if you hit rate limits.

## How the tracker picks tokens
It pulls four lists from CoinGecko's onchain data: trending Solana pools, the busiest pump.fun pools, the busiest PumpSwap pools, and the newest pools. Each token has to be 20 minutes to 24 hours old, hold at least $8k liquidity, and have 25+ buyers in the last hour. The survivors are ranked by volume, unique buyers and buy/sell balance. Vertical candles and dumping tokens are penalised. FOMY writes a read for the top three, with what's working, red flags, what has to be true, and a label. He never says buy.

Each visitor's browser runs its own scan, so two people in the same hour can see slightly different picks.

## How the wallet judge works
It reads the wallet's last 100 transactions from a public Solana RPC node, finds the swaps, and works out:
- win rate, realized PnL and hold times
- overtrading, bags never sold and late-night trading
- fees, and buying bigger right after a loss

FOMY then gives a grade, an archetype and a verdict, plus a "Copy my verdict" button so people can share it on Fomo. It's read-only: only the public address is used and nothing is stored.

## Replies on Fomo
Fomo has no public API, so the site can't read or post Fomo replies. Write FOMY's replies with the FOMY Reply Desk in your Claude account and post them on Fomo yourself.
