# Study 4: Does MSTR lead Bitcoin? A three-hour clock artifact

**Date:** 2026-09 | **Verdict:** No return lead (the commonly cited one is a sampling artifact); a lead at weekly cycle tops is real but couldn't be harvested with the rules tested

## Question

A common claim says MicroStrategy (MSTR) leads Bitcoin, so MSTR's moves can be used to time BTC, or MSTR can be timed around BTC's cycle.

## Data

- MSTR daily and weekly closes; BTC from exchange hourly data.
- SEC EDGAR filings for MSTR's bitcoin holdings (145 observations, cross-checked against XBRL, max difference 0.24%).
- **A0 sanity check:** reproduced both all-time highs to the cent and verified the 10:1 split on 2024-08-08.

## Test 1: daily return lead-lag

**Design:** sample both series at **16:00 ET** on each NYSE session (BTC from hourly data, DST-resolved; 1,677 of 1,677 sessions matched). Then compute cross-correlations at lags -10 to +10 and run Granger tests by era.

**Result:**
- Contemporaneous correlation **0.76-0.82**. Every lagged correlation sits inside the +/-0.051 noise band. The contemporaneous term is **16x** the largest lagged one.
- MSTR -> BTC Granger: insignificant at every lag in every era (1 isolated hit out of 20, about the expected false-positive rate).

**The methodological finding:** using the free data source's *daily* BTC close, which is stamped at a different time of day than the NYSE close, "BTC leads MSTR" is significant at **p=0.0018**. Align the clocks and it drops to **p=0.228**. The widely repeated lead-lag result, in either direction, is a **three-hour sampling artifact**.

## Weekly bars: no return lead either

320 weekly bars, lags to +/-16 weeks. Lag 0 = 0.697. Lag +8 weeks = 0.065, inside the +/-0.112 naive band. With a Bonferroni band for 33 lags (+/-0.177), nothing outside lag 0 clears it.

## Turning points: where a lead does exist

Asked differently ("does MSTR turn before BTC at cycle tops?"), the answer changed. With a weekly zigzag (MSTR 40%, BTC 30% thresholds), each BTC turn was paired with the nearest MSTR turn of the same kind:

- Median lead **+28 days** across 7 turns, 4 of 7 positive.
- **Tops only:** +56, 0, +84 days. All non-negative, median **+56 days**.
- Bottoms were mixed (one lagged by 42 days).

**Frame matters:** on *daily* closes MSTR's top appeared to come 320 days before BTC's. That top was an intra-week spike. On *weekly closes* the MSTR top moves to a later week, and the lead over BTC is 84 days.

## Can the lead be harvested?

Fourteen top-detection rules were tested on weekly MSTR over 2020-08 to 2026-08 (buy-and-hold 10.32x, maxDD -86.3%):

- **Only one of 14** (a 40-week SMA) beat buy-and-hold: 26.55x, maxDD -61.1%. Its edge rests on two decisions, and a one-week execution shift scrambles the rankings across rules.
- Ten of 14 rules cut drawdown by less than 5 pp, and six made it worse.
- **The decisive arm:** applying each rule to MSTR's signal instead of BTC's own signal won in only **5 of 13** cases, with a median **12% worse** outcome. On the best rule it was a dead heat (10.47x vs 10.52x).
- Detection lag (1-5 months) is about the same size as the lead (2-3 months), so a confirmed top arrives around or after BTC has already turned.

## Why the naive version was wrong

Two daily series closing at different times of day create a lead-lag that isn't there. A second trap: the answer depended on the frame (daily vs weekly, returns vs turning points), and the frame wasn't stated at first.

## Verdict

- No return lead at daily or weekly frequency, once the clocks are aligned.
- A lead at weekly cycle tops exists, but n=3 tops and the zigzag thresholds were researcher-chosen.
- It adds nothing over watching BTC itself with the rules tested. The 40-week SMA held up as a **risk overlay** (drawdown reduction), not as a return source.

**Limitations:** an intraday lead is untested; very small n at turning points; multiple rules on one window (forking paths).
