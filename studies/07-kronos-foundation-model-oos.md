# Study 7: A time-series foundation model, out of sample

**Date:** 2026-10 | **Verdict:** Failed 2 of 3 pre-registered bars; a follow-up sell-only test also failed

## Question

Kronos is an open-source foundation model for financial candlestick (K-line) data, published by researchers at Tsinghua University. It reports large relative improvements in forecast skill. Does it add value as a filter on a simple long-only crypto trend-following rule?

## Data and design

- **Model:** Kronos-base, run locally on a consumer GPU (GTX 1080 Ti), 20 sampled paths per forecast (about 2.2 s each).
- **Sample:** **5,874 daily walk-forward forecasts**, 7 coins, July 2024 to October 2026. Every forecast falls after the model's June 2024 training cutoff, so no forecast was made on data the model had seen.
- **Pre-registration** (written before running): 7-day forecast cross-sectional information coefficient (IC) must be positive and significant; must beat a plain 28-day momentum baseline; and must improve the baseline trend rule's CAGR without unacceptable drawdown.

## Result

| Measure | Kronos | Baseline |
|---|---|---|
| 7d weekly cross-sectional IC | **-0.074 (t -2.05, wrong sign)** | 28d momentum **+0.095 (t 2.10)** |
| Pooled IC | +0.001 (permutation p 0.48) | |
| Standalone "long when Kronos says up" | CAGR -0.6%, Sharpe 0.09, maxDD -38% | Trend rule: 12.2% / 0.97 / -10% |
| As a filter on the trend rule | CAGR 3.8%, Sharpe 1.11 | 12.2% |

The filter's higher Sharpe came from being **in cash 97% of days**. Its CAGR fell from 12.2% to 3.8%.

**Why:** the forecasts were anti-trend (only 11% of trend-on days got an "up" forecast) and badly miscalibrated. BTC's 7-day forecast standard deviation was 17%, and one small-cap coin's median forecast was +33% per week. The headline improvement in the paper is a RankIC moving from roughly 0.013 to 0.025 on Chinese A-shares, which is a different market, horizon and metric from daily crypto.

## Follow-up: sell-only (pre-registered addendum)

Entries from the trend rule, exit when Kronos forecasts down, locked out until a fresh trend entry.

- **Absolute version** (exit on any negative 7d forecast): exited within days. Exposure fell from 24% to 1% and CAGR from 12.2% to 3.4%. It was no better than random exits at the same rate (91st percentile of placebos).
- **Relative version** (exit when the coin's forecast is in its own bottom 20%): *worse* than random exits, beating only 2% of placebos. After its "sell" signals, coins gained **+2.1%** on average over 7 days vs -0.1% on other trend days. The signal was inverted.

## Why the naive version was wrong

A model's published benchmark isn't evidence for a different asset class, horizon and use. Two checks did the work here: a test period entirely after the training cutoff, and a dumb baseline (28-day momentum) the model had to beat. A Sharpe ratio that improves because the strategy stops trading is not an improvement.

## Verdict

Rejected as a filter and as a sell signal. Closed.

**Limitations:** 7 coins; daily frequency only; base model size only; no fine-tuning on crypto data (fine-tuning would be a different study with its own leakage risks).
