# Study 3: A fundamental screener's 38.5% that wasn't

**Date:** 2026-04 (original), 2026-06 (correction) | **Verdict:** Original headline invalidated

## Question

Does a trailing-fundamentals stock screener (growth, profitability, valuation and balance-sheet filters, built on SEC EDGAR XBRL data) pick stocks that beat QQQ over the next 12 months?

## Original test (April)

- Walk-forward over 10 annual checkpoints (July of each year, 2016-2025), point-in-time fundamentals from EDGAR.
- Universe: **about 30 hand-picked tickers** mixing known winners, losers and defensive names.
- Headline: top-10 picks beat QQQ in **8 of 10** years, average forward 12-month return **38.5%**. It looked like it caught NVDA in 2019 and TSLA before its 2019-2020 run.

## Re-test (June)

The same screener was re-run point-in-time on a realistic **138-ticker** universe that included laggards.

- **NVDA in 2015:** ranked **#15 of 39** names passing filters. It passed, but it was middling, not a top pick. It only reached #3 by 2019, after the ramp was already in trailing fundamentals.
- **TSLA:** failed the filters outright in 2015-16 (unprofitable, ROE -177%) and ranked **dead last, #60 of 65, in 2019**, right before a +344% year.
- **The 2015 top picks** on the fair universe (LLY, YUM, GILD, MA, DE) had mostly negative forward 12-month returns (-8%, -10%, -29%, -2%, -15%), while NVDA (+129%) sat at #15.
- **Parse bug:** some revenue-growth values of +300% were XBRL artifacts. The parser picked up inconsistent revenue line items or segment values across filings, so part of the original ranking was noise.

## Why the naive version was wrong

- **Curated universe.** "NVDA ranked #2 of 28 hand-picked names" became "#15 of 138 realistic names." A universe chosen by someone who already knows the winners puts the winners near the top of any reasonable score.
- **Structural blind spot.** A trailing-fundamentals screen can't buy pre-profit hypergrowth. By the time the numbers show up in filings, much of the move has happened.
- **Unvalidated parser.** Extreme growth values should have triggered a check against the filing before they drove a ranking.

## Corroborating evidence

A separate, unbiased real-money analog, cloning the top holdings of 14 prominent fund managers from 13F filings selected on their 2013 prominence rather than later results, returned about **14.7% vs SPY 14.3% vs QQQ 20.1%** (2015-2026). No manager had a significant positive alpha. Fundamentals-driven selection on a fair universe came out as roughly market beta in both tests.

## Verdict

The 38.5% / "8 of 10 years" result is **not valid**. On a fair universe the screener showed no usable edge at the checkpoints examined. It is still useful as a descriptive lens, alongside a trend/extension lens, but not as a stand-alone selector.

**Process lessons:** always test on a universe you would have had at the time, including the names you would have ignored; sanity-check outlier fundamentals against the source filing.
