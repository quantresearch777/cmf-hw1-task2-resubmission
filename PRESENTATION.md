# CMF HFT HW1 — Task 2 and Task 3 Resubmission

This is the common reader-first presentation for both resubmission repositories.

- [Task 2 repository](https://github.com/quantresearch777/cmf-hw1-task2-resubmission)
- [Task 3 repository](https://github.com/quantresearch777/cmf-hw1-task3-resubmission)
- [Polished HTML version](docs/index.html)

## Executive summary

### Task 2: submit the new candidate

The 30s and 120s rules remain unchanged. At 300s, the existing causal liquidation-pressure rule is retained only for ETH, while BTC is filtered out.

- exact Score: `+34.0292 / +29.4222 / +41.8325 bps` at 30s/120s/300s;
- 300s improvement versus the initial submission: `+22.2331 bps`;
- positive 300s delta in `6/6` calendar months;
- positive next-month delta in `4/4` walk-forward folds;
- no runtime ML or model artifact.

### Task 3: report both frozen variants

The previous horizon-specific implementation remains the numerical leader. A deterministic train-only challenger is reported alongside it to separate performance from selection methodology.

| Horizon | Previous high-score | Train-only challenger |
|---:|---:|---:|
| 30s | +0.036674 | +0.029095 |
| 120s | +0.112193 | +0.086263 |
| 300s | +0.204462 | +0.078606 |

The final choice must be made on a hidden or genuinely fresh period rather than by inspecting February again.

## Objective and Score

Both tasks filter trades from a market-making strategy after causally observable market or liquidation events. The official metric is turnover-weighted:

```text
Score = PnL_kept / Turnover_kept − PnL_all / Turnover_all
```

The exact pipeline aggregates numerators and turnover before taking the ratio; it does not average daily Score values.

## Research scope

The final code is simple, but the research screen was broad:

- public course repositories, compared by results, validation design, reproducibility, and implementation choices;
- liquidation pressure, maker/taker interpretation, symbol gates, q90/q95/q99 thresholds, 30/60s windows, size bands, delayed reactions, and rule combinations;
- the synthetic-sampling study as a diagnostic benchmark;
- 31 CPU classical-ML configurations or architectures, including linear, tree, boosting-style, calibration, clustering, and feature-increment branches;
- exact-stream, monthly, walk-forward, bootstrap, multiple-testing, capacity, and tail-risk checks.

The learned increment after the Task2 ETH gate was small and unstable, so it was not added to runtime. Task3 combinations, symbol gates, size bands, and delayed-reaction windows also failed their held-out checks.

## Anti-overfitting protocol

1. Define the selection rule before examining its evaluation result.
2. Select only on past months and evaluate on the next month.
3. Recompute finalists on the complete exact stream after screening.
4. Disclose the full candidate family and corrected uncertainty diagnostics.
5. Freeze code, run deterministic tests, execute notebooks, and hash artifacts.

March–April for Task2 and February for Task3 had been inspected during earlier work. The reconstruction is strict but retrospective; these periods are not relabeled as untouched holdouts.

## Task 2 evidence

| Month | Initial 300s | Candidate 300s | Delta |
|---|---:|---:|---:|
| 2025-11 | +9.3693 | +20.6433 | +11.2740 |
| 2025-12 | −12.1990 | −2.0970 | +10.1020 |
| 2026-01 | +38.8956 | +69.3658 | +30.4702 |
| 2026-02 | +20.2312 | +47.9906 | +27.7594 |
| 2026-03 | −5.5461 | +17.3233 | +22.8694 |
| 2026-04 | +13.3549 | +14.2245 | +0.8696 |

All four calibration months independently select ETH-only, and all four subsequent evaluation deltas are positive. The exact one-sided 4/4 sign test is `p = 0.0625`: supportive, but based on a small number of regimes.

The 300s five-day block-bootstrap interval is `[+9.80, +73.18]` bps. The rule is active on 77/179 days with turnover of approximately `$9.19m/day`. Its worst active day is `−317.81 bps`, which remains a material limitation.

## Task 3 evidence

The train-only selector evaluates q90/q95/q99 × 30/60s maker-side rules. It requires positive Score and turnover above `$500k/day` in both December and January, then maximizes the worst monthly train Score.

| Horizon | Frozen train-only rule | December | January | February |
|---:|---|---:|---:|---:|
| 30s | `q95 / 30s` | +0.041871 | +0.049936 | +0.029095 |
| 120s | `q99 / 60s` | +0.042993 | +0.246614 | +0.086263 |
| 300s | `q99 / 30s` | +0.017482 | +0.277382 | +0.078606 |

All three February values are positive, but their block intervals include zero and their corrected sign-flip tests are not significant. The challenger strengthens the methodology rather than proving a new edge.

## Final claim boundary

Supported:

- Task2 improves the 300s exact aggregate by `+22.23 bps`;
- the Task2 delta is positive in 6/6 months and 4/4 next-month folds;
- both Task3 implementations obey causal event availability;
- all published code passes deterministic and original course-interface tests.

Not supported:

- calling previously inspected periods untouched holdouts;
- claiming 5% FDR significance for Task2 (`BH q = 0.1159` across 96 comparisons);
- claiming corrected Task3 significance from 28 February days;
- selecting Task3 again after looking at February;
- promising production performance without a hidden or fresh test.

## Recommended handoff

- **Task2:** submit the ETH-only 300s implementation with unchanged 30s/120s rules.
- **Task3:** show the numerical high-score reference and the train-only challenger, and let a hidden period decide.
- **Runtime:** do not add ML; retain the ML screen as a transparent negative result.
