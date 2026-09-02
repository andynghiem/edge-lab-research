# EDGE LAB Research

**Open, falsification-first research on crypto trading edges.**

[![Research Status](https://img.shields.io/badge/status-open%20research-2ea44f)](#)
[![Trading](https://img.shields.io/badge/live%20trading-none-lightgrey)](#)
[![Method](https://img.shields.io/badge/method-falsification--first-blue)](#)

EDGE LAB is an independent research project for testing whether a trading hypothesis still has an edge **after realistic costs, timing constraints, and out-of-sample checks**.

The project is deliberately skeptical. A strategy is not considered useful because a chart looks good, a backtest is profitable, or an AI model likes it. We try to break a hypothesis before we promote it.

> **Research success and deployment success are different things.**
> A strategy can be statistically real and still be rejected because the return on total deployed capital is too small.

## What we test

EDGE LAB focuses on mechanisms that can be stated clearly enough to falsify:

- Who is paying for the edge?
- Why should that behavior persist?
- What information was actually available at decision time?
- Does the edge survive fees, slippage and latency?
- Does it survive different market regimes?
- Is the return large enough on **total capital actually required**?

The working protocol is documented in [`RESEARCH_PROTOCOL.md`](RESEARCH_PROTOCOL.md).

## Published case study: WBETH staking-enhanced carry

The first result published here is a structural Binance carry hypothesis:

**Long WBETH spot + short ETHUSDT perpetual**, with a fixed ETH hedge quantity determined from the contemporaneous WBETH/ETH ratio and no rebalancing during each 90-day evaluation window.

The economic idea is simple: WBETH embeds ETH staking accrual, while a short ETH perpetual can receive funding when longs pay shorts. The short leg also removes most directional ETH exposure.

### Historical result

| Metric | Result |
|---|---:|
| Common history | 2023-10-11 → 2026-09-02 |
| Rolling 90-day windows | **968** |
| Median net return after 35 bps | **+1.7567%** of one-leg notional |
| Approx. fully-collateralized equivalent | **+0.878% / 90 days** |
| Windows positive after 35 bps | **97.31%** |
| Windows positive after 50 bps | **95.45%** |
| Windows positive after 75 bps | **88.64%** |
| Median net return after 50 bps | **+1.6067%** of one-leg notional |
| Bootstrap 95% CI for mean net35 | **+1.901% to +3.466%** |
| Research classification | `WBETH_STAKING_CARRY_CANDIDATE` |
| Holdout unseals | **0** |

**Important:** the 968 windows overlap heavily. `97.31% positive windows` is **not** the same thing as 968 independent trades or a conventional win rate.

### Why we did not promote it to live trading

The hypothesis survived the historical research gates, but the return on fully collateralized capital was too small for the deployment objective of the project. Roughly half of the one-leg return is the relevant capital return when both the spot leg and equal futures collateral are funded.

That makes this a useful example of an EDGE LAB principle:

> **A real edge can still be a bad allocation of capital.**

The case study, preregistration summary, result artifact and limitations are in [`case-studies/0030-wbeth-staking-carry/`](case-studies/0030-wbeth-staking-carry/).

## Research integrity

Published studies should be reproducible from public data wherever practical. EDGE LAB uses the following rules:

1. **Pre-register the mechanism and primary evaluation before looking at the result.**
2. **No lookahead.** Only data available at the simulated decision time may be used.
3. **Model realistic costs.** A gross edge that disappears after costs is not an edge.
4. **Stress the result.** Costs, timing and regime dependence must be examined.
5. **Keep holdout discipline.** A sealed holdout is not repeatedly reopened for tuning.
6. **Publish failures and limitations.** Negative results are useful research output.
7. **Add a capital-efficiency gate.** Statistical positivity alone is insufficient.

## Community research

We welcome useful clues from traders, market makers, researchers, developers and careful observers.

A clue can be small: a sentence in a podcast, an X/Twitter post, a YouTube comment, a market-structure observation, a paper, a postmortem, or something repeatedly visible in live markets.

What matters is whether it can be converted into a falsifiable hypothesis.

Before proposing a strategy, please read [`CONTRIBUTING.md`](CONTRIBUTING.md). Good submissions answer five questions:

1. Who pays for the edge?
2. Why might it persist?
3. What exact public data could test it?
4. What would kill the hypothesis quickly?
5. If the hypothesis is true, is the optimistic return large enough to justify the capital and engineering effort?

## Support the research

EDGE LAB is independently funded. Donations help cover data access, compute, API usage, reproducibility work and research tooling.

**PayPal:** `huyai386145@gmail.com`

Donations are support for independent research only. They are **not** investments, do not represent equity or profit sharing, and do not create any entitlement to trading returns.

More details: [`SUPPORT.md`](SUPPORT.md).

## Licensing

This repository uses a split license so research can be shared openly without making third-party material ambiguous:

- **Source code:** MIT License — see [`LICENSE-CODE`](LICENSE-CODE).
- **Original research, documentation and written case-study material:** Creative Commons Attribution 4.0 International (**CC BY 4.0**) — see [`LICENSE-DOCS`](LICENSE-DOCS).
- **Third-party data, trademarks, quoted material and linked sources:** remain subject to their respective owners' terms and are not relicensed by this repository.

The top-level [`LICENSE`](LICENSE) file summarizes which license applies to which material.

## Repository map

```text
edge-lab-research/
├── README.md
├── RESEARCH_PROTOCOL.md
├── CONTRIBUTING.md
├── DISCLAIMER.md
├── SUPPORT.md
├── LICENSE
├── LICENSE-CODE
├── LICENSE-DOCS
└── case-studies/
    └── 0030-wbeth-staking-carry/
        ├── README.md
        ├── PRE_REG.md
        ├── DATA_MANIFEST.json
        └── RESULTS.json
```

## Safety and scope

This repository contains research, not a trading service. It does not provide private keys, exchange credentials, live order execution, custody, pooled funds, investment management or promises of profit.

Please read [`DISCLAIMER.md`](DISCLAIMER.md) before using any material in this repository.

---

**EDGE LAB Research** — test the mechanism, count the costs, publish what survives.