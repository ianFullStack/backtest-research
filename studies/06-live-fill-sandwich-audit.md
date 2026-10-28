# Study 6: Was it MEV? A live-fill sandwich audit

**Date:** 2026-09 | **Verdict:** Not MEV. The gap was fixed costs the simulator ignored, plus post-entry drift on one strategy.

## Question

During one live trading period, small-order memecoin strategies on Solana did noticeably worse live than in paper simulation. The obvious suspect was MEV: bots sandwiching our swaps (buying just before, selling just after). Was it?

## Data: three independent primary-source instruments

- **Block ordering:** for every fill, fetch the full block and check whether the same signer traded the same token immediately before and after our transaction in the same slot (the strict sandwich signature).
- **Tick tape:** every trade on the token +/-120 seconds around each fill, from a market-data API.
- **Quotes:** the aggregator quote logged at decision time, compared with the realized fill.

Sample: **n=747 live fills (344 round trips).**

## Result

- **Sandwiches: 0 of 747** under the strict same-block, same-signer test.
- A looser size-aware tape test flagged 2.7% of fills, and those trips actually did *better* (+3.6% vs -2.2%). The flag marked hot tokens, not victims.
- **Fill vs last pre-trade print:** median +0.20% on buys, -0.04% on sells.
- **Fill vs aggregator quote:** median **0.00%** on buys, **+0.10%** on sells. The aggregator (private submission) delivered its quote.
- **The real gap was fixed cost the paper simulator charged as zero.** At small order size (~0.1 SOL), a buy cost **3.28%** over the pool price: about 2.0% one-time token-account rent, 1% pool fee, 0.1% other fees. Sells cost 1.15%. That comes to **about 4.4 pp per round trip** before any price movement. The simulator returned the raw quote with no fees or rent.
- One strategy (buying tokens right after they graduate from a launch curve) showed **-1.37% drift in the 30 s after buying (t=-2.14)**. That is real adverse selection in *what* it bought, not *how* it was filled. A comparable strategy showed none.
- The worst trips (-48% to -99%) had about 0.15% execution premium and no sandwich flags. They were rugs, not execution failures.
- A side finding: hundreds of empty token accounts were still holding reclaimable rent.

## Why the naive version was wrong

"Live is worse than paper, so someone is front-running us" is a story that fits any gap. The test that can actually say no is block-level ordering, and it said no. The paper simulator was optimistic in a boring, measurable way: it left out per-trade fixed costs, and those dominate at small size.

## Verdict

Not MEV. For small-order backtests, charge account rent and pool fees per side and model the aggregator's fill-vs-quote as roughly zero. Never attribute a live-vs-paper gap to MEV without the same-block test. The block-ordering and tape tooling is reusable for any future fill-quality question.

**Limitations:** one venue/aggregator and one trading period; the tape API's coverage of very thin pools is imperfect, so the loose flag is approximate (the strict block test is not).
