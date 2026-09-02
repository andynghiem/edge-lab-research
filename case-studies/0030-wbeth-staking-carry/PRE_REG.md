# Preregistration — WBETH Staking-Enhanced Carry

This document records the frozen design used for Case Study 0030.

## Goal

Test whether a long-lived structural carry mechanism exists on Binance when holding WBETH spot and hedging ETH price exposure with a short ETHUSDT USD-M perpetual position.

The proposed carry sources are:

1. staking accrual embedded in WBETH;
2. perpetual funding received by the short leg when funding is positive.

The study is not a momentum strategy and does not select bull-market windows after the fact.

## Frozen strategy

Venue: Binance only.

- Long leg: WBETH spot.
- Hedge leg: short ETHUSDT USD-M perpetual.
- Direction fixed ex ante.
- At each historical evaluation entry, determine ETH hedge quantity from the contemporaneous WBETH/ETH spot ratio.
- Keep the ETH short quantity fixed for the entire evaluation window.
- No rebalancing during the 90-day hold.

The fixed-quantity design intentionally leaves small residual delta drift caused by staking accrual. That drift is part of the realized PnL rather than being removed post hoc.

## Data

Public unauthenticated Binance market data only.

Required series:

- WBETHETH spot;
- WBETHUSDT spot;
- ETHUSDT spot for diagnostic cross-checks;
- ETHUSDT USD-M perpetual mark-price data;
- ETHUSDT USD-M perpetual funding history.

No account data, private reward endpoints, API keys, orders or wallets are used.

## Historical boundary

Use the maximum common data history, but not before:

`2023-10-11 00:00:00 UTC`

Fresh diagnostic boundary:

`2026-08-01 00:00:00 UTC` → latest complete common observation available at the time of the study.

## Timing rules

- UTC throughout.
- Never use future information.
- Exact synchronization or a frozen backward-asof tolerance of at most 1 hour.
- Positive Binance perpetual funding means longs pay shorts, so the short funding cashflow is positive when `fundingRate > 0`.
- Funding cashflow scales with the fixed ETH short quantity and a contemporaneous valid mark price.
- Spot and perpetual price PnL are calculated separately before combination.

## Primary evaluation

Rolling exact 90-calendar-day holds stepped daily across the common history.

For each window, normalized by initial WBETH spot notional, compute:

1. WBETH spot PnL;
2. ETH perpetual short price PnL;
3. funding PnL;
4. gross combined PnL;
5. net PnL after frozen costs;
6. fully collateralized capital return assuming capital equals spot notional plus equal initial futures collateral;
7. adverse combined mark-to-market when supported by the retained path data.

No leverage in the primary gate.

## Frozen costs

Total two-leg round-trip assumptions:

- 35 bps primary;
- 50 bps stress;
- 75 bps severe stress.

These assumptions must not be lowered after observing the result.

## Robustness

- calendar-quarter and calendar-year summaries;
- first-half versus second-half history;
- positive-window share at 35/50/75 bps;
- UTC calendar-month block bootstrap;
- bootstrap seed `300030`;
- 2,000 resamples;
- fresh 2026-08-01 → latest fixed-position diagnostic;
- anti-concentration check for dependence on a small number of quarters.

## Frozen candidate gates

Classify `WBETH_STAKING_CARRY_CANDIDATE` only if all are true:

1. At least 30 months common history and data-quality checks pass.
2. Median rolling-90d net35 > 0.
3. At least 70% of rolling-90d windows have net35 > 0.
4. Median rolling-90d net50 > 0.
5. Bootstrap 95% CI lower bound for mean 90d net35 > 0.
6. At least 75% of complete calendar quarters have positive median net35.
7. Every complete calendar year except at most one has positive median 90d net35.
8. First-half and second-half historical median net35 are both positive.
9. Fresh 2026-08-01 → latest combined gross direction is positive.
10. No single calendar quarter contributes more than 35% of total positive historical net35 aggregate.
11. Holdout unseals remain zero and there is no material mechanism mismatch.

Otherwise classify `WBETH_STAKING_CARRY_FAIL`.

## Interpretation frozen before result

A PASS means only that the historical mechanism survived the specified gates and deserves prospective consideration. It does not authorize live trading.

A separate deployment decision must consider capital efficiency, execution, liquidity, operational risk and current market conditions.