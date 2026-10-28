# Study 1: Whale copy-trading and the control cohort

**Date:** 2026-08 | **Verdict:** Refuted on all three pre-registered criteria

## Question

A widely shared social-media method for Solana memecoins goes like this: take a token that already went up 100x, find its ~20 earliest buyers, filter to active non-bot wallets, and copy whatever they buy next. The useful reframing is that this is a claim about **wallets**, not about the past winning coin. Do wallets that were early to past winners keep being early to future winners?

## Data

- Prospectively collected trending-token snapshots (our own collector, so no look-back selection of the token list).
- On-chain transaction history from an RPC provider, rate-limited and run serially (7,754 calls, zero rate-limit errors).
- **Formation window:** winners (>=5x, liquidity >= $10K) launched June 1 to July 15. Cohort frozen point-in-time on 2026-07-15.
- **Test window:** tokens born July 16 to August 31. 92 test winners, 97 test non-winners.

## Design

- **Skilled cohort:** early buyers of formation-window winners.
- **Control cohort:** wallets that were *equally early* to formation-window tokens that went nowhere. Same selection procedure, losing outcome.
- **C1 (skill):** skilled-cohort enrichment for test winners must exceed the control cohort's.
- **C2 (tradeable):** a copier polling at a realistic interval must be able to follow entries and exits.
- **C3 (robust):** the result must survive leave-one-wallet-out and leave-one-token-out.

## Result

| | Odds ratio (presence in test winners) | p |
|---|---|---|
| Skilled cohort | **1.94** (CI 1.34-2.81) | 3.4e-4 |
| Control cohort (early to duds) | **1.55** | 0.024 |
| Skilled vs control | z = 0.83 | **0.41** |
| Skilled, token-clustered | 1.74 | 0.091 |

- **Mechanism:** the median cohort wallet bought **1,227 distinct mints** (max 2,002). They are early to everything, so they are early to the winners too.
- **The post's own "repeat winner" filter made it worse.** Early to one formation winner: OR 2.12. Early to two: OR 1.25 (p=0.68). Early to three or more: zero wallets. The dud cohort actually recurred more often than the winner cohort.
- **Untradeable.** Median hold for these early buyers was **1 second** (p90 53 s). **17 of 17** observed exits happened before a 15-minute copy poll could fire. Even an oracle arm that exited at their actual sell lost **-8.5%**.
- **Leave-one-out.** LOO-wallet +37.3% -> +11.2% (t 0.47). LOO-token -> +16.6% (t 0.60).
- **Base-rate correction.** The test arm was 49% winners by construction against about 7.9% in reality. Inverse-probability reweighted, copying the cohort returned **-62.7% / -72.4% / -85.7%** at 1h / 24h / 7d. That is worse than blindly buying every token the screener discovered (-13.6% at 1h, -46.5% at 24h, n=1,574).

## Why the naive version was wrong

Without the control cohort, this was a clean false positive at p=0.0003, the kind of number that ends up in a thread with a chart. The enrichment is real. It just doesn't come from skill. Hyperactive wallets show up early on every token, so they also show up early on the winners.

## Reusable engineering findings

- **74% of transactions at token birth were failed snipes** with empty transfers. Filter on success before spending parse calls.
- **Page the bonding-curve account, not the mint**, to reach a token's birth. The curve stops transacting at graduation, so it reaches the start in 2-6 pages instead of 110+ (about 20x cheaper).

## Verdict and standing rule

Refuted. The study produced a standing rule for the research loop: **an enrichment or skill claim needs a cohort selected the same way on losing outcomes. If the losers enrich too, you measured activity.**

**Limitations:** one six-week test window; a single chain and launch venue; the cohort definition ("early") uses one cutoff.
