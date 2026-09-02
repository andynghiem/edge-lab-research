# EDGE LAB Research Protocol

EDGE LAB exists to answer a narrow question:

> **Does a clearly specified market mechanism retain an economically meaningful edge after realistic implementation constraints?**

This document describes the public research standard. Individual studies may add stricter rules, but should not silently weaken these rules after results are visible.

## 1. Start with a mechanism, not a chart

A hypothesis should identify an economic payer or structural reason for the effect.

Examples of useful explanations include inventory pressure, forced rebalancing, funding transfer, liquidity segmentation, stale quoting, constrained arbitrage, settlement mechanics or persistent participant behavior.

A correlation without a plausible payer is allowed as an exploratory clue, but it should not be promoted as a structural edge without additional evidence.

## 2. Practitioner clue → falsifiable hypothesis

EDGE LAB gives special attention to concrete clues from practitioners: market makers, quant traders, exchange developers, protocol teams, bot operators, postmortems, podcasts, social posts, papers and public trading infrastructure.

A practitioner statement is **not proof**. It is a hypothesis generator.

Before implementation, write down:

- the claimed mechanism;
- the predicted direction;
- the market and instruments;
- the required data;
- the expected holding or reaction horizon;
- the primary cost assumptions;
- the exact condition that would reject the hypothesis.

## 3. Data-availability gate before engineering

Do not build a large system before verifying that the required data exists with usable timestamp semantics.

A study should fail closed if the required historical evidence cannot distinguish:

- information available before the decision from information known afterward;
- executable price from a convenient candle close or mid;
- real event time from ingestion time;
- actual liquidity from inferred liquidity when the distinction matters.

If the required data is unavailable, publish `BLOCKED_DATA` rather than inventing a proxy that changes the hypothesis.

## 4. No lookahead

At simulated time `t`, the strategy may use only information with `available_time <= t`.

Typical lookahead errors include:

- using the close of a candle at its opening timestamp;
- filling at a later best price;
- using a post-trade DEX state as a pre-trade quote;
- aligning funding or event data to a future market observation;
- selecting the best threshold, venue, direction or horizon after seeing the result.

If an as-of join is used, its direction and maximum tolerance must be frozen before evaluation.

## 5. Pre-register before the primary result

A primary study should freeze at least:

- hypothesis and direction;
- universe or instrument;
- entry rule;
- exit or evaluation horizon;
- execution convention;
- fee/slippage model;
- latency assumption when relevant;
- primary metric;
- pass/fail gates;
- robustness checks;
- any holdout boundary.

Changing these after observing the primary result creates a new hypothesis and should be recorded as such.

## 6. Costs are part of the strategy

Report gross and net results separately.

Depending on the mechanism, costs may include:

- maker/taker fees;
- bid/ask spread;
- slippage and market impact;
- gas and priority fees;
- funding payments;
- borrow cost;
- bridge or withdrawal cost;
- inventory-rebalance cost;
- latency decay;
- failed or partial execution.

Stress assumptions should be conservative enough that a strategy is not promoted only because a single optimistic fee tier was chosen.

## 7. Capital-efficiency kill gate

A statistically positive result is not automatically useful.

Before deep engineering, estimate an optimistic upper bound for return on **total capital required to run the strategy**.

Total capital includes capital stranded across multiple venues, collateral, hedging inventory and mandatory reserves when those are required by the mechanism.

If even the optimistic upper bound is too small for the project objective, stop early. This rule was added after the WBETH carry study: the mechanism was historically robust, but the fully collateralized return was not attractive enough for deployment.

## 8. Robustness before promotion

Useful checks depend on the strategy, but may include:

- multiple cost levels;
- latency stress;
- first-half / second-half stability;
- calendar week, month, quarter or year breakdowns;
- concentration of profit in a small number of periods or trades;
- block bootstrap or another dependence-aware uncertainty estimate;
- multiple economically justified trade sizes;
- adverse mark-to-market or drawdown;
- inventory and capacity limits.

Do not confuse overlapping observations with independent samples.

## 9. Holdout discipline

When a study uses a sealed holdout, it should not be repeatedly reopened for tuning.

A holdout result is evidence, not a new optimization surface.

Public reports should state how many times the holdout was unsealed whenever that concept applies.

## 10. Classification vocabulary

Recommended result labels:

- `NO_SIGNAL` — the preregistered effect is not supported.
- `COST_KILLED` — gross signal exists but is not economically positive after realistic costs.
- `BLOCKED_DATA` — required evidence is unavailable or insufficient.
- `MECHANISM_MISMATCH` — available implementation does not test the stated mechanism.
- `CANDIDATE` — historical evidence survives the frozen gates and warrants prospective validation.
- `PROSPECTIVE_PASS` — a preregistered forward test survived its gates.
- `DEPLOYMENT_REJECTED` — research may be valid, but economics, capacity, risk or operational constraints make deployment unattractive.

`CANDIDATE` is never equivalent to “safe”, “guaranteed”, or “ready for live money”.

## 11. Publication standard

A published study should include, where practical:

- the preregistration;
- data sources and time range;
- data manifest or checksums;
- exact cost assumptions;
- result artifact;
- known limitations;
- enough code or pseudocode to understand the calculation;
- a clear statement of what would invalidate the conclusion.

Failures are publishable results.

## 12. AI policy

AI systems may help search, implement, critique, test and audit research. They are not allowed to substitute confidence for evidence.

No result is accepted because an AI model calls it promising. The evidence and deterministic calculations must support the claim.

When multiple AI systems contribute, disagreements should be resolved by data, reproducible logic or an explicit unresolved limitation rather than by majority vote.

---

This protocol is intentionally conservative. The objective is not to maximize the number of profitable-looking backtests; it is to minimize the number of false edges that survive the research process.