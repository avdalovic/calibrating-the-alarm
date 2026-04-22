# Robustness Supplement (Section 6 Extension)

This document reports supplementary robustness evaluations for **"Calibrating the Alarm"**.

## What This Supplement Tests
Primary question:
Does the deployment conclusion from Section 6 remain valid when we perturb practical choices around sampling, calibration hyperparameters, model depth, and operator-feedback quality?

Primary answer:
Yes. Across these sweeps, adaptive calibration remains substantially more deployment-usable than static calibration in low-FPR operation.

## Shared Evaluation Context
Unless explicitly stated otherwise:
- Testbed context: SWaT setting used in the paper's main deployment analysis.
- Upstream detector: fixed pretrained GRU artifact (no retraining in these sweeps).
- Score stream: `score_pred` (L2 residual aggregate).
- Primary deployment metric: false positive rate (FPR) on benign test periods.
- Adaptive targets: `alpha = 0.01`, buffer `W = 3600`.

Scientific limits:
- Results are point estimates from fixed split/run artifacts.
- This supplement does not report multi-seed uncertainty intervals.
- Interpretation is constrained to robustness trends, not universal guarantees.

## Sampling Rate Robustness
Question:
Does the method ordering persist at native 1-second sampling?

| Sampling | Calibration | FPR | F1 |
|---|---|---:|---:|
| 10s | static | 0.7406 | 0.2514 |
| 10s | self_filtered | 0.1473 | 0.5574 |
| 10s | operator_feedback | 0.0298 | 0.7456 |
| 1s | static | 0.7284 | 0.2518 |
| 1s | self_filtered | 0.1462 | 0.3640 |
| 1s | operator_feedback | 0.0519 | 0.6688 |

Interpretation:
The ranking is unchanged across sampling intervals: `operator_feedback` remains best, `self_filtered` remains second, and `static` remains worst on FPR/F1 tradeoff.

Raw files:
- `results/sampling_rate/ablation_sampling_rate_fpr_f1.csv`
- `results/sampling_rate/ablation_sampling_rate_fpr_f1_table.md`

## Lambda Sensitivity (Self-Filtered Calibration)
Question:
How sensitive is self-filtered calibration to damping factor `lambda`?

| Lambda | FPR | Recall | F1 |
|---:|---:|---:|---:|
| 0.0050 | 0.2627 | 0.8413 | 0.4352 |
| 0.0100 | 0.1473 | 0.8252 | 0.5574 |
| 0.0200 | 0.0521 | 0.7409 | 0.6916 |
| 0.0500 | 0.0504 | 0.6122 | 0.6120 |
| 0.1000 | 0.0481 | 0.4310 | 0.4785 |

Interpretation:
FPR decreases as `lambda` increases, while recall decreases after moderate damping; the best observed F1 is at `lambda = 0.02`. The paper default `lambda = 0.01` remains a conservative, stable operating point.

Raw files:
- `results/lambda_sensitivity/lambda_sensitivity.csv`
- `results/lambda_sensitivity/lambda_sensitivity.json`
- `results/lambda_sensitivity/metadata.json`

## Gamma Sensitivity (Operator-Feedback ACI Learning Rate)
Question:
How sensitive is operator-feedback ACI to step size `gamma`?

| Gamma | FPR | Recall | F1 |
|---:|---:|---:|---:|
| 0.0010 | 0.0204 | 0.7141 | 0.7633 |
| 0.0050 | 0.0273 | 0.7198 | 0.7458 |
| 0.0100 | 0.0298 | 0.7308 | 0.7456 |
| 0.0200 | 0.0310 | 0.7205 | 0.7355 |
| 0.0500 | 0.0357 | 0.7347 | 0.7312 |
| 0.1000 | 0.0375 | 0.7318 | 0.7243 |

Interpretation:
Across the tested range, recall stays comparatively stable while larger `gamma` increases FPR and gradually lowers F1. Best observed F1 occurs at `gamma = 0.001`; the paper default `gamma = 0.01` remains close to the practical tradeoff region.

Raw files:
- `results/gamma_sensitivity/gamma_sensitivity.csv`
- `results/gamma_sensitivity/gamma_sensitivity.json`
- `results/gamma_sensitivity/metadata.json`

## Number of Layers Robustness
Question:
Do conclusions persist when model depth changes from 1 to 3 layers (hidden size fixed)?

| Layers | Hidden | Calibration | FPR | Recall | F1 |
|---:|---:|---|---:|---:|---:|
| 1 | 64 | static | 0.7300 | 0.9438 | 0.2493 |
| 1 | 64 | self_filtered | 0.1395 | 0.7721 | 0.5423 |
| 2 | 64 | static | 0.7191 | 0.9486 | 0.2533 |
| 2 | 64 | self_filtered | 0.1422 | 0.7641 | 0.5344 |
| 3 | 64 | static | 0.7418 | 0.9610 | 0.2503 |
| 3 | 64 | self_filtered | 0.1539 | 0.7961 | 0.5339 |

Interpretation:
Across all tested depths, `self_filtered` reduces FPR by roughly 0.57 to 0.59 absolute versus `static`, while keeping F1 near 0.53 to 0.54. This supports robustness of the calibration conclusion to moderate model-depth variation.

Raw files:
- `results/layers_robustness/layers_robustness.csv`
- `results/layers_robustness/layers_robustness.md`

## Degraded Operator Feedback
Question:
How robust is operator-feedback ACI when feedback is delayed and label quality degrades?

Parameters matched to paper setup:
- target false alarm rate `alpha = 0.01`
- buffer size `W = 3600`
- ACI step size `gamma = 0.01`
- score stream `score_pred` (L2)

Stress axes:
- delay `d in {60, 180, 600}`
- feedback label flip probability `misclassify_rate in {0.0, 0.1, 0.2, 0.5}`

Static reference: `FPR = 0.7406`, `F1 = 0.2514`.

| Misclassify Rate | Delay 60 (FPR/F1) | Delay 180 (FPR/F1) | Delay 600 (FPR/F1) |
|---:|---:|---:|---:|
| 0.0% | 0.0298 / 0.7456 | 0.0220 / 0.7558 | 0.0251 / 0.5558 |
| 10.0% | 0.0234 / 0.5102 | 0.0186 / 0.5212 | 0.0192 / 0.4505 |
| 20.0% | 0.0205 / 0.4719 | 0.0195 / 0.4773 | 0.0220 / 0.4298 |
| 50.0% | 0.0310 / 0.4013 | 0.0221 / 0.4209 | 0.0228 / 0.3843 |

Interpretation:
FPR remains low across the grid (0.019 to 0.031), while F1 degrades gradually with increasing feedback corruption. At 20% feedback misclassification, F1 remains above static across delays, indicating graceful degradation rather than collapse.

Raw files:
- `results/degraded_feedback/degraded_feedback_grid.csv`

## Overall Robustness Takeaway
These supplementary results support the same deployment claim as the main paper:
static calibration is brittle under realistic shift, while adaptive calibration provides materially better false-alarm control with usable detection performance across practical perturbations.
