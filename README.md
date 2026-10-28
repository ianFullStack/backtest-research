# Backtest Research Notes

Case studies from an autonomous research loop that proposes, backtests and adjudicates trading ideas. Most of the ideas failed. These notes record how each one was tested, what the numbers were, and why the first, flattering version of the result was wrong.

> **Not financial advice.** These are research notes about test design. Nothing here recommends buying, selling or holding any asset. Results are historical, sample-limited and may contain errors. Each study states its own limitations.

## Who and what

I'm Ian Sandoval, a full-stack and AI automation engineer. I built a research workflow on top of Claude Code where AI agents do most of the legwork, and the rules below keep that legwork honest:

- A **daily iteration agent** picks the highest-priority open question, writes a pre-registered test plan (pass/fail criteria fixed before the data is run), builds the backtest, runs it, and records the verdict.
- A **weekly "appeals court" agent** re-examines rejected and accepted results. It can reopen a verdict only with new evidence or a new test design.
- A **scoreboard** labels every claim VALIDATED or UNVALIDATED. Nothing goes into the production system without a passing backtest first.
- I review and push back. Several of the studies below changed because a question was reframed partway through (see the MSTR study).

This repository holds the write-ups only. It does not include the trading system, its live configuration, or any account information.

## Methodology principles

These rules came out of mistakes. Each one is here because a study looked significant until it was applied.

1. **Backtest before you build.** No idea ships on a story, a chart, or someone else's track record. If it can't be tested, it doesn't ship.
2. **Pre-register the criteria.** Pass/fail bars (for example "beats the equal-weight universe in at least 3 of 4 eras" or "IC t-stat > 2 with the correct sign") are written down before the run, and a failed bar is reported as failed.
3. **Control cohort selected the same way on losing outcomes.** A skill or enrichment claim ("wallets that were early to winners keep finding winners") needs a control group picked by the same procedure from losers. If the losers' group shows the same enrichment, you measured activity, not skill. ([Study 1](studies/01-whale-copy-trading-control-cohort.md))
4. **Matched random-day baseline.** "Positive N out of N times" means nothing until you know how often a random day in the same window is positive. Every signal gets compared against random dates on the same tickers.
5. **Artifact checks, run before believing anything:**
   - **Clock / timezone artifacts.** Two series closing at different times of day create a fake lead-lag. ([Study 4](studies/04-mstr-btc-lead-lag-clock-artifact.md))
   - **Stale-quote artifacts.** Comparing a fast price against a slow reference makes the fast one look mispriced. ([Study 8](studies/08-xstock-nav-stale-reference.md))
   - **Bar-close print selection.** Filling at the same bar that triggered the signal picks the lucky low print. Re-run with the fill on the next bar.
   - **Curated universe / survivorship.** A hand-picked or still-listed-today universe flatters everything, including the benchmark. ([Studies 2, 3](studies/02-top-0-3-percent-concentration-rule.md))
   - **Parse and unit bugs.** XBRL line-item mismatches, pre-split share counts, wrong-symbol price series. Reproduce a known number (an all-time high to the cent, a published share count) before trusting the pipeline.
6. **Walk-forward and out-of-sample halves.** Results are split by era or by half. A result that lives in one era is reported as one era.
7. **t-stats, leave-one-out, and placebos.** Report the t-stat, drop the single best name or trade and re-run, and compare against a placebo (random exits at the same rate, fixed-delay fills).
8. **State the frame.** Daily and weekly closes gave materially different answers in the MSTR study. Every result names its sampling frame.
9. **When a bug is found, check what consumed the bad records.** Re-running the headline number is not enough. Downstream summaries and reviews built on corrupted data need re-checking too.

## Studies

| # | Study | One-line finding |
|---|---|---|
| 1 | [Whale copy-trading: the control cohort](studies/01-whale-copy-trading-control-cohort.md) | OR 1.94 at p=0.0003 looked like skill. Wallets equally early to duds scored 1.55, statistically the same. It was activity. |
| 2 | ["Only own the top 0.3%"](studies/02-top-0-3-percent-concentration-rule.md) | The skew is real (one name created 24% of all wealth in the panel), but picking it ex ante failed. Top-1 selection returned 7.37% CAGR vs 19.21% for owning everything equally. |
| 3 | [A screener's 38.5% that wasn't](studies/03-fundamental-screener-invalidated.md) | A 10-year walk-forward "beat QQQ 8/10 years". On a realistic 138-name universe, with a parse bug found, the edge was roughly market beta. |
| 4 | [MSTR leads BTC? A three-hour clock artifact](studies/04-mstr-btc-lead-lag-clock-artifact.md) | "BTC leads MSTR" at p=0.0018 became p=0.228 once both series were sampled at 16:00 ET. Weekly cycle tops do show a lead, but it couldn't be harvested. |
| 5 | [Limit orders on a mean-reversion strategy](studies/05-limit-orders-adverse-selection.md) | Every resting-limit variant lost 8 to 14 times more per trip than market orders. Classic adverse selection. |
| 6 | [Was it MEV? A live-fill sandwich audit](studies/06-live-fill-sandwich-audit.md) | 0 of 747 fills were sandwiched and fills matched quotes. The live-vs-paper gap was fixed costs the simulator ignored (about 4.4pp per round trip at small size). |
| 7 | [A time-series foundation model, out of sample](studies/07-kronos-foundation-model-oos.md) | On 5,874 post-training-cutoff forecasts, cross-sectional IC was -0.074 (wrong sign). Plain 28-day momentum beat it. |
| 8 | [Tokenized-stock NAV discounts and a stale reference](studies/08-xstock-nav-stale-reference.md) | A 75% win rate fell to 61% once a stale hourly reference was excluded. The follow-up overlay lost two thirds of its edge to bar-close selection. |

## Common thread

Most of these studies have the same failure: **the first test design could not produce a negative result.** A cohort with no control, a universe chosen after the fact, a reference price that lags, a fill on the triggering bar. Each one makes a null effect look like an edge. Most of the work is building the version of the test that can say no.

## License

Text is licensed under [CC BY 4.0](LICENSE) (c) Ian Sandoval.
