# Calibrating the Alarm

Public supplementary artifact for the paper **"Calibrating the Alarm: Why Deep Learning ICS Anomaly Detectors Fail at Deployment"**.

Repository URL
https://github.com/avdalovic/calibrating-the-alarm

## Purpose
This repository reports additional robustness evaluations that extend Section 6 of the paper.

Single takeaway:
Adaptive calibration (self-filtered and operator-feedback ACI) remains materially more deployment-usable than static calibration under changes in sampling interval, calibration hyperparameters, model depth, and degraded operator feedback.

## Scope and Scientific Framing
- These are supplementary robustness checks, not a new benchmark.
- Metrics are reported on fixed temporal splits used in the paper protocol.
- Results are point estimates from fixed run artifacts (no multi-seed confidence intervals in this repository).
- Unless noted, experiments reuse the same upstream detector and vary calibration settings only.

## Repository Layout
```
calibrating-the-alarm/
├── README.md
├── ROBUSTNESS.md
├── results/
│   ├── sampling_rate/
│   ├── lambda_sensitivity/
│   ├── gamma_sensitivity/
│   ├── layers_robustness/
│   └── degraded_feedback/
└── ...
```

## Robustness Results Map
| Result Folder | Robustness Question | Main Observation |
|---|---|---|
| `results/sampling_rate/` | Does the Section 6 ranking hold at 1-second sampling? | Yes. Method ranking is unchanged (`operator_feedback` best FPR/F1, then `self_filtered`, then `static`). |
| `results/lambda_sensitivity/` | How sensitive is self-filtered calibration to damping `lambda`? | Smooth tradeoff; best observed F1 at `lambda = 0.02`; conservative paper default `lambda = 0.01` remains strong. |
| `results/gamma_sensitivity/` | How sensitive is operator-feedback ACI to learning rate `gamma`? | Recall is stable, FPR rises with larger `gamma`; default `gamma = 0.01` stays near the practical tradeoff region. |
| `results/layers_robustness/` | Do conclusions hold when model depth changes (1-3 layers)? | Yes. `self_filtered` consistently keeps far lower FPR than `static` across all tested depths. |
| `results/degraded_feedback/` | Does operator-feedback ACI remain useful under delayed/noisy feedback? | Yes. FPR remains low across delay and misclassification stress conditions, while F1 degrades gradually. |

## Main Document
See `ROBUSTNESS.md` for compact tables, assumptions, and interpretation text that is ready to cite in the paper supplement narrative.
