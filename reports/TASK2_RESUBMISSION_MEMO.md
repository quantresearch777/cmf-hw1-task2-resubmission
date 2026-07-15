# Task 2 resubmission: exact ETH-only 300s gate

## Decision

Keep the submitted 30s and 120s rules unchanged. At 300s, keep the existing causal liquidation-pressure rule for ETH and filter BTC. No ML runtime or model artifact is required.

## Full-stream exact result

| Horizon | Previous Score | Resubmission Score | Delta | Turnover/day | Kept turnover share | Active days |
|---:|---:|---:|---:|---:|---:|---:|
| 30s | +34.0292 | +34.0292 | +0.0000 | $7.70m | 0.0326% | 43/179 |
| 120s | +29.4222 | +29.4222 | +0.0000 | $20.04m | 0.0850% | 77/179 |
| 300s | +19.5994 | +41.8325 | +22.2331 | $9.19m | 0.0389% | 77/179 |

The accepted 300s exact Score is **+41.8325 bps**, versus +19.5994 previously. The 5-day circular block-bootstrap CI for the candidate Score is [+9.80, +73.18] bps.

## Month-by-month 300s result

| Month | Previous | Candidate | Delta | Turnover/day |
|---|---:|---:|---:|---:|
| 2025-11 | +9.3693 | +20.6433 | +11.2740 | $11.90m |
| 2025-12 | -12.1990 | -2.0970 | +10.1020 | $3.97m |
| 2026-01 | +38.8956 | +69.3658 | +30.4702 | $23.45m |
| 2026-02 | +20.2312 | +47.9906 | +27.7594 | $4.81m |
| 2026-03 | -5.5461 | +17.3233 | +22.8694 | $4.10m |
| 2026-04 | +13.3549 | +14.2245 | +0.8696 | $6.29m |

The delta is positive in all six calendar months. Leave-one-month-out and block-length sensitivity checks are stored in the exact Phase 2 evidence bundle.

## Causal selection

The symbol choice was frozen on each calibration month and evaluated on the next month:

| Fold | Calibration | Evaluation | Gate | Evaluation Score | Previous | Delta | Turnover/day |
|---:|---|---|---|---:|---:|---:|---:|
| fold1 | 2025-12 | 2026-01 | ETH only | +69.3658 | +38.8956 | +30.4702 | $23.45m |
| fold2 | 2026-01 | 2026-02 | ETH only | +47.9906 | +20.2312 | +27.7594 | $4.81m |
| fold3 | 2026-02 | 2026-03 | ETH only | +17.3233 | -5.5461 | +22.8694 | $4.10m |
| fold4 | 2026-03 | 2026-04 | ETH only | +14.2245 | +13.3549 | +0.8696 | $6.29m |

All four calibration months selected the same ETH-only gate and all four future evaluation deltas were positive. The exact one-sided four-fold sign-test is p=0.0625; it is supportive but not, by itself, a large independent-regime sample.

## Selection-bias and multiple-testing audit

- The symbol gate and its day-matched pooled null were written into the Phase 2 protocol before model results were available.
- The complete family contained 32 candidates × 3 horizons = 96 comparisons. For the 300s symbol gate, the day-block bootstrap probability of a positive delta is 0.9583; BH q across the full family is 0.1159. It does **not** cross a 5% FDR threshold and is reported rather than hidden.
- The preregistered day-matched random-row control gives +19.13 bps, with its empirical 95% interval [+18.41, +19.83] over 300 draws.
- The learned candidates add only a small and unstable increment after the symbol gate; therefore none is included in the submission.

## Capacity and tail risk

The 300s rule is active on 77/179 days, with a median active-day Score of +14.54 bps. Its worst active day is -317.81 bps on 2026-03-23; the 5% expected shortfall is -116.33 bps and the worst five-day aggregate ends on 2026-03-23 at -319.46 bps.

This tail is a material limitation. A separate `phase_safe` shadow challenger removes the March 23 loss but reduces aggregate edge and is not promoted without fresh data.

## Reproducibility

- `task2_solution.py`: standalone submission code;
- `test_task2_solution.py`: deterministic unit tests;
- `actual_data_parity.json`: real-stream parity evidence;
- CSV files in this directory: exact aggregate, monthly, causal-fold, capacity and tail evidence.

March-April had already been inspected before Phase 2. This is strict retrospective walk-forward evidence, not a claim of a genuinely untouched production holdout.
