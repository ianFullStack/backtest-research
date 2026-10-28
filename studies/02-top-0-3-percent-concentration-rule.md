# Study 2: "Only own the top 0.3%"

**Date:** 2026-09 | **Verdict:** Descriptive claim true; portfolio rule fails all three pre-registered criteria

## Question

A popular rule of thumb says to invest only in the top 0.3% of assets, because a tiny fraction of stocks creates most of the wealth. That sentence mixes two claims:

- **A (descriptive):** wealth creation is extremely concentrated.
- **B (actionable):** you can pick that concentrated set in advance and come out ahead.

The study was built to separate them.

## Data and design

- 147-name US equity panel, 1996-12-31 to 2025-12-31 (29 years). One name was dropped for a data defect (see below).
- Annual formation, 15 bps per side costs.
- Arms: TOP1, TOP5, TOP10 (ranked on prior performance), TOPCAP1/TOPCAP5 (largest caps), QUALITY5, **EWALL** (equal-weight every name, no selection), SPY, QQQ.
- Criteria: beat EWALL on CAGR, beat it on risk-adjusted terms, and beat it in a majority of four eras.

## Result

**A: true, and understated.** Of the total wealth created per $1 invested in each name:

- Top 0.3% (one stock, NVDA) = **24.0%**
- Top 1% (two stocks) = 34.9%
- Top 5% (8 names) = **61.2%**
- Top 25% = 89.3%
- Only 38 of 147 (25.9%) beat SPY. 12 lost money outright.
- Mean total return 16,633% vs median 2,929%. That 5.7x gap *is* the phenomenon.

**B: fails.** 29-year results:

| Arm | CAGR | Sharpe | Max DD | Terminal $1 |
|---|---|---|---|---|
| TOP1 | **7.37%** | 0.39 | **-97.98%** | $7.87 |
| TOP5 | 14.20% | 0.48 | -80.8% | $47 |
| TOP10 | 15.05% | | | |
| TOPCAP5 | 16.76% | | | |
| QUALITY5 | 14.49% | | | |
| **EWALL (no selection)** | **19.21%** | **0.83** | **-51.4%** | **$163.32** |
| SPY | 9.85% | | | |
| QQQ | 8.30% | | | |

No concentrated arm beat EWALL in more than 2 of 4 eras. TOP1's whole lifetime result comes from one era (-12.0% / -6.9% / +42.6% / -5.0%).

## The subtle part: it finds tails, both of them

TOP1 landed in the ex-post top decile in **8 of 29 years (27.6% vs a 10% baseline, binomial p=0.006)**. So the ranking is not a dart throw. It also landed in the bottom decile in 5 of 29 years (17.2%), and its median percentile rank was the **41st**, below average. In a one-name portfolio that is a variance amplifier: TOP1's arithmetic mean annual return was **+38.78%**, the best in the table, while its geometric CAGR was **7.37%**, the worst. Seven of 29 years lost more than 40%.

## Why the naive version was wrong: two survivorship biases

1. **Dead companies are missing.** 28 of 34 delisted tickers were unavailable from the free data source. Injecting them at -100% leaves the wealth shares robust (the top 5% rises from 61.2% to 73.7%), but "share of names beating T-bills" collapses from **91.8% to 45.8%**. The 91.8% should never be quoted.
2. **The benchmark itself is contaminated.** EWALL, with no selection at all, beat SPY by **9.36 pp/yr in 93.1% of years**. That is the fingerprint of a universe assembled in 2026 from still-listed names. So **arm-vs-SPY is not a valid alpha comparison in this panel.** Only arm-vs-EWALL is internally valid, and every concentrated arm loses it. The bias pushes ex-ante selection results upward, so the true TOP1 CAGR is likely below 7.37%.

## Data defect found along the way

One ticker symbol returned a different security's price series (starting in 2021 at a price level around 101,500 and ending with a spurious -100%). It was dropped here, and a prior study that used the same name list was flagged for re-checking.

## Verdict

"Only invest in the top 0.3%" describes an outcome distribution. It isn't a portfolio rule. When returns are that skewed, the response that held up here was to own enough of the distribution that you can't miss the winner, which is the opposite of what the rule says.

**Limitations:** small panel assembled with hindsight (see bias 2); annual formation only; ranking signals tested are simple.
