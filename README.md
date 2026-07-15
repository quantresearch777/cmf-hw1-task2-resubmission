# CMF HFT HW1 — Task 2 Resubmission

This repository contains the strengthened Task 2 submission. It is intentionally separate from the original [`cmf-hw1-task2`](https://github.com/quantresearch777/cmf-hw1-task2) repository so that the initial handoff remains immutable and easy to compare.

## Result

The 30s and 120s rules are unchanged. At 300s, the existing causal liquidation-pressure rule is retained for ETH and BTC is filtered out.

| Horizon | Original Score | Resubmission Score | Delta |
|---:|---:|---:|---:|
| 30s | +34.0292 | +34.0292 | +0.0000 |
| 120s | +29.4222 | +29.4222 | +0.0000 |
| 300s | +19.5994 | **+41.8325** | **+22.2331** |

The 300s delta is positive in all six calendar months and in all four next-month walk-forward evaluations. The exact five-day block-bootstrap interval for the candidate Score is `[+9.80, +73.18]` bps.

## Start here

1. [Common Task2/Task3 research presentation](PRESENTATION.md)
2. [Task 2 resubmission memo](reports/TASK2_RESUBMISSION_MEMO.md)
3. [Frozen submission code](src/task2_solution.py)
4. [Compact exact evidence](evidence)

## Reproduce the code checks

```bash
python -m pip install -r requirements.txt
python -m unittest discover -s tests -v
```

Expected result: 9 tests pass.

## Claim boundary

March–April had already been inspected before this reconstruction. The evidence is strict retrospective walk-forward evidence, not a newly untouched holdout. The full 96-comparison audit gives BH `q = 0.1159`, and the worst active 300s day is `−317.81 bps`; both limitations are reported rather than hidden.

Related repository: [Task 3 resubmission](https://github.com/quantresearch777/cmf-hw1-task3-resubmission).
