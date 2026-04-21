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

## Lambda Sensitivity

| Lambda | Calibration | FPR | F1 |
|---:|---|---:|---:|
| pending | pending | pending | pending |

This section will report sensitivity to the damping factor used in self-filtered calibration. It will test whether FPR control and F1 remain stable across conservative and more aggressive update settings. Raw outputs will be stored under `results/lambda_sensitivity/`.

## Buffer Size Extended

| Buffer Size W | Calibration | FPR | F1 |
|---:|---|---:|---:|
| pending | pending | pending | pending |

This section will extend the window-size analysis beyond the main setting. It will quantify the operating tradeoff between faster adaptation and threshold stability. Raw outputs will be stored under `results/buffer_size_extended/`.

## Degraded Feedback

| Feedback Condition | Calibration | FPR | F1 |
|---|---|---:|---:|
| pending | operator_feedback | pending | pending |

This section will measure robustness when operator feedback is sparse, delayed, or noisy. It will show how much performance degrades when feedback quality drops from the idealized setup. Raw outputs will be stored under `results/degraded_feedback/`.
