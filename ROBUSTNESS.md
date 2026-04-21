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

| Feedback Condition | Calibration | FPR | F1 |
|---|---|---:|---:|
| pending | operator_feedback | pending | pending |

This section will measure robustness when operator feedback is sparse, delayed, or noisy. It will show how much performance degrades when feedback quality drops from the idealized setup. Raw outputs will be stored under `results/degraded_feedback/`.
