Calibrating the Alarm: Why Deep Learning ICS Anomaly Detectors Fail at Deployment. These robustness experiments supplement Section 6.

## Sampling Rate Robustness

| Sampling | Calibration | FPR | F1 |
|---|---|---:|---:|
| 10s | static | 0.7406 | 0.2514 |
| 10s | self_filtered | 0.1473 | 0.5574 |
| 10s | operator_feedback | 0.0298 | 0.7456 |
| 1s | static | 0.7284 | 0.2518 |
| 1s | self_filtered | 0.1462 | 0.3640 |
| 1s | operator_feedback | 0.0519 | 0.6688 |

This table checks whether the Section 6 trend survives at native 1s sampling. The ranking is unchanged across both sampling intervals, with `operator_feedback` best and `self_filtered` better than `static`. Absolute values shift, but the deployment conclusion is stable.

## Lambda Sensitivity for self-filtered method

| Lambda | FPR | Recall | F1 |
|---:|---:|---:|---:|
| 0.0050 | 0.2627 | 0.8413 | 0.4352 |
| 0.0100 | 0.1473 | 0.8252 | 0.5574 |
| 0.0200 | 0.0521 | 0.7409 | 0.6916 |
| 0.0500 | 0.0504 | 0.6122 | 0.6120 |
| 0.1000 | 0.0481 | 0.4310 | 0.4785 |

The FPR tradeoff is smooth and monotonic as lambda increases. F1 peaks at `lambda = 0.02` and then declines as larger damping allows more attack signal into the buffer and recall falls. The main paper keeps `lambda = 0.01` as a conservative default fixed before this sweep, while this supplement shows the method is robust to this choice.

## Buffer Size Extended

| Buffer Size W | Calibration | FPR | F1 |
|---:|---|---:|---:|
| pending | pending | pending | pending |

This section will extend the window-size analysis beyond the main setting. It will quantify the operating tradeoff between faster adaptation and threshold stability. Raw outputs will be stored under `results/buffer_size_extended/`.

## Degraded Feedback

This section is a robustness extension of the paper setup in Section 6.2 for `operator_feedback` ACI. It uses the same calibration operator and updates, then relaxes the idealized operator assumption by injecting feedback errors and longer delays.

Parameters matched to Section 6.2:
- target false alarm rate `alpha = 0.01`
- buffer size `W = 3600` observations
- ACI step size `gamma = 0.01`
- score stream `score_pred` (L2) on the SWaT GRU run artifacts

Robustness axes varied here:
- feedback delay `d` in `{60, 180, 600}` steps
- feedback label flip probability `misclassify_rate` in `{0.0, 0.1, 0.2, 0.5}`

Static reference is `FPR = 0.7406`, `F1 = 0.2514`.

| Misclassify Rate | Delay 60 (FPR/F1) | Delay 180 (FPR/F1) | Delay 600 (FPR/F1) |
|---:|---:|---:|---:|
| 0.0% | 0.0298 / 0.7456 | 0.0220 / 0.7558 | 0.0251 / 0.5558 |
| 10.0% | 0.0234 / 0.5102 | 0.0186 / 0.5212 | 0.0192 / 0.4505 |
| 20.0% | 0.0205 / 0.4719 | 0.0195 / 0.4773 | 0.0220 / 0.4298 |
| 50.0% | 0.0310 / 0.4013 | 0.0221 / 0.4209 | 0.0228 / 0.3843 |

FPR is stable across the entire grid, staying between `0.019` and `0.031` even when half of operator feedback labels are incorrect. F1 degrades gracefully with misclassification rate and is less sensitive to delay than to feedback quality. This converts the idealized-operator limitation into a quantified boundary, and even at 20% misclassification the method remains above static with `F1 = 0.47` versus `0.25` in low FPR regime.
