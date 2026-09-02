# Case Study 0030 — WBETH Staking-Enhanced Carry

**Research verdict:** `WBETH_STAKING_CARRY_CANDIDATE`  
**Deployment verdict:** `DEPLOYMENT_REJECTED_CAPITAL_EFFICIENCY`

This case study tests a structural, approximately delta-neutral Binance carry mechanism over historical public data.

## Hypothesis

At the start of each evaluation window:

1. Buy WBETH spot.
2. Read the contemporaneous WBETH/ETH spot ratio.
3. Short a fixed quantity of ETHUSDT perpetual equal to that entry ratio.
4. Hold both quantities for exactly 90 calendar days.
5. Do not rebalance inside the window.

The proposed sources of carry are:

- staking accrual embedded in WBETH;
- ETH perpetual funding received when the funding rate is positive for shorts;
- with most directional ETH price risk offset by the short leg.

This is not a momentum strategy and the direction was frozen before evaluation.

## Data

Public, unauthenticated Binance market data only:

- WBETHUSDT spot 1h klines;
- WBETHETH spot 1h klines;
- ETHUSDT spot 1h klines for diagnostics;
- ETHUSDT USD-M perpetual mark-price 1h klines;
- ETHUSDT USD-M funding history.

Common historical coverage used by the accepted result:

`2023-10-11 00:00 UTC` → `2026-09-02 04:00 UTC`

See [`DATA_MANIFEST.json`](DATA_MANIFEST.json).

## Frozen costs

Total two-leg round-trip cost assumptions:

- **35 bps** primary;
- **50 bps** stress;
- **75 bps** severe stress.

These were frozen before the result and were not lowered to rescue the strategy.

## Accepted historical result

| Metric | Result |
|---|---:|
| Exact rolling 90-day windows | **968** |
| Median net35 | **+1.756683%** one-leg notional |
| Positive net35 windows | **97.314%** |
| Median net50 | **+1.606683%** one-leg notional |
| Positive net50 windows | **95.455%** |
| Positive net75 windows | **88.636%** |
| Bootstrap 95% CI, mean net35 | **+1.9013% to +3.4663%** |
| First-half median net35 | **+3.5668%** |
| Second-half median net35 | **+0.9879%** |
| Max quarter positive contribution | **23.85%** |
| Holdout unseals | **0** |

The fresh partial diagnostic from 2026-08-01 to 2026-09-02 had gross combined PnL of **+0.8438%** of one-leg notional. It was a partial direction/regime diagnostic, not a completed 90-day result.

Full machine-readable values are in [`RESULTS.json`](RESULTS.json).

## Critical interpretation

The 968 observations are rolling daily entries with 90-day holding periods, so adjacent observations overlap heavily. The `97.314% positive` statistic must **not** be interpreted as a conventional independent-trade win rate.

The historical research result was strong enough to satisfy the preregistered candidate gates. That does not mean the strategy was attractive for deployment.

### Capital efficiency

The published net35 return is normalized to the initial WBETH spot notional. A conservative fully collateralized implementation funds both:

- the WBETH spot leg; and
- equal initial futures collateral.

Therefore the median fully collateralized equivalent is approximately:

`1.7567% / 2 ≈ 0.878% per 90 days`

That return was considered too small relative to the project's deployment objective and the additional crypto-specific risks. The project therefore stopped further deployment work on this mechanism.

This is an intentional example of the distinction between:

- **mechanism validity** — evidence that a structural effect existed historically; and
- **investment attractiveness** — whether the effect is worth the required capital, risk and engineering effort.

## Risks and limitations

Material risks include:

- Binance counterparty and operational risk;
- WBETH liquidity, redemption and basis risk;
- funding inversion;
- perp margin/liquidation risk if implemented with insufficient collateral;
- execution mismatch between spot and perpetual legs;
- residual delta drift from fixed-quantity hedging;
- changing fees, market structure and regional availability.

Known research limitations include overlapping rolling windows and the fact that historical market-data replay cannot perfectly reproduce future executable liquidity or operational conditions.

## Files

- [`PRE_REG.md`](PRE_REG.md) — frozen research design summary.
- [`DATA_MANIFEST.json`](DATA_MANIFEST.json) — source/coverage counts.
- [`RESULTS.json`](RESULTS.json) — accepted machine-readable result.

## Reproduction status

The public repository currently publishes the accepted research artifacts and methodology. The original private laboratory contains additional implementation/audit machinery that is intentionally not mirrored here.

A sanitized standalone reproduction runner should be added only after it is independently checked for public release. Until then, treat the artifacts here as a transparent research publication rather than a turnkey trading implementation.

---

This case study is research only. See [`../../DISCLAIMER.md`](../../DISCLAIMER.md).