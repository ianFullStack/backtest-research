# Study 5: Limit orders on a mean-reversion strategy

**Date:** 2026-09 | **Verdict:** Rejected. Every limit-order variant was worse than market orders.

## Question

A pair strategy that trades convergence between correlated liquid tokens was losing a small amount per round trip. Would resting limit orders (enter and exit "at our price") and tighter slippage tolerance fix the cost problem?

## Design

The existing pair-exit backtest harness was extended with three variants, all measured against the market-order baseline on the same signals:

- `limitEntry`: rest a limit at the signal price for entries.
- `limitExit`: rest a limit at the convergence target for exits. If it doesn't fill, fall back to a market exit later.
- `limitBoth`: both.

Cost evidence was taken from real fills. Fill-vs-quote slippage was measured directly (see [Study 6](06-live-fill-sandwich-audit.md)), and logged retry failures were attributed to their actual causes.

## Result

- **Every resting-limit arm lost 8-14x more per trip** than the market-order baseline: **-0.55% to -0.90% per trip, t from -3.3 to -6.2**, against **-0.066%** for market orders.
- Entry limits filled only **19-48%** of signals, and the trades they did fill were the ones that kept falling.
- Exit limits left **55-60%** of convergence exits unfilled, and those later closed at worse market prices.
- **Slippage was never the cost.** Real fill-vs-quote median was **0.00%**. All 165 logged "slippage" retries traced to a transaction-signing bug, not to price movement. The real live-vs-simulated gap came from fixed per-trade costs (account rent and pool fees), and limit orders don't remove those.

## Why the naive version was wrong

It's classic **adverse selection**. A resting limit on a mean-reversion signal fills when price moves against you and misses when the trade works. Winners run away unfilled; losers fill. "Wait for our price" is a market-maker strategy, and it only pays with a maker edge, meaning a reason to expect the price to come back to you.

The second error was diagnosing the cost before measuring it. Fill-vs-quote data already showed slippage was zero, so lowering slippage tolerance couldn't help.

## Verdict

Rejected. Keep limit orders only where the fill itself is the target, for example tiered take-profit orders. Never tighten slippage tolerance to fix a cost that fill-vs-quote data already shows is zero.

**Limitations:** one strategy on a small set of liquid token pairs; limit fills are simulated in the backtest harness, not observed live.
