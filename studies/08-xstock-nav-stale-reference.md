# Study 8: Tokenized-stock NAV discounts and a stale reference

**Date:** 2026-07 | **Verdict:** Headline edge two-thirds artifact; follow-up execution overlay rejected by its own artifact check

## Question

Tokenized stocks on Solana ("xStocks") trade 24/7 in on-chain pools, while the underlying stock trades only during US market hours. A pool sometimes trades at a premium or discount to the stock. One example was a +4-5% premium over a closed-market weekend that converged by Monday. Is a discount a harvestable mean-reversion edge for a long-only trader?

## Data and design

- **Pools:** 15-minute OHLCV for the most liquid pool of 7 tokenized stocks (QQQ, NVDA, GOOGL, COIN, MSTR, CRCL, TSLA), January to July 2026.
- **Reference:** hourly underlying stock prices, forward-filled through nights and weekends (the last regular-session price is what arbitrage converges to at the next open).
- **Rule:** buy when the pool trades at least 1.5% below the reference (bars with volume only), exit when within 0.25%, or after about 5 days. Costs 30 bps per side.

## Result

| Run | Trades | Win rate | Avg net / trade |
|---|---|---|---|
| Headline (all hours) | 327 | **75%** | **+0.96%** |
| Artifact check: entries only while the stock market is closed | 319 | **61%** | **+0.30%** |

**Two-thirds of the headline edge was a stale-reference mirage.** During market hours the reference was an hourly price, while the pool traded every 15 minutes. A fast intraday move made the pool look "discounted" against a reference that hadn't updated yet. Restricting entries to closed-market hours, when the reference really is frozen, removes that.

**What was left:** overnight and weekend discounts of 1.5% or more converged 100% of the time (average about 9 hours) with **+1.84%** premium capture. But the underlying stock drifted **-1.3% to -2.0%** while the position was held. Discounts show up during selloffs, so the pool was partly right. The per-name spread confirms it: GOOGL +1.13% (best), MSTR -0.14% (worst). In high-beta names, the discount was information rather than mispricing.

## Follow-up: premium-aware execution overlay

The next idea was to use the premium only to time execution of existing trades: wait for a discount to buy, never buy at more than a +1% premium, never sell into a discount below -1%.

- Replayed on all 92 eligible historical fills, with a fixed-delay placebo as control.
- **Fill at the trigger bar's close:** +25.3 bps per fill. It passed all four pre-registered criteria and beat the placebo by about 34 bps.
- **Pre-registered artifact check, fill at the next bar:** **+9.3 bps per fill**, below the proposal's own 20 bps shipping bar. Leave-one-out minimum +3.1 bps (one fill carried most of the residual).
- Historical fills were already premium-neutral (only 3 of 47 buys at more than +1% premium), so there was little to save.

## Why the naive version was wrong

Both runs fell to the same family of artifact: **measuring a fast price against a slow reference, or filling on the same bar whose print triggered the signal.** Either one selects favorable prints that a real order wouldn't get. Running the artifact check as a pre-registered step meant the rejection was decided by the plan, not argued after the fact.

## Verdict

The premium effect is real but small (single-digit bps once the artifact is removed), fragile, and partly explained by the underlying stock drifting during the hold. Rejected as a strategy and as an execution overlay.

**Limitations:** about 6 months of free-tier pool history; early-2026 pools were thin and carried larger premiums that later normalized; the hourly reference resolution is itself a limitation.
