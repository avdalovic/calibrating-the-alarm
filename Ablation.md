# Sampling-Rate Ablation (SWaT, 10s vs 1s)

| Sampling | Calibration | FPR | F1 |
|---|---|---:|---:|
| 10s | static | 0.7406 | 0.2514 |
| 10s | self_filtered | 0.1473 | 0.5574 |
| 10s | operator_feedback | 0.0298 | 0.7456 |
| 1s | static | 0.7284 | 0.2518 |
| 1s | self_filtered | 0.1462 | 0.3640 |
| 1s | operator_feedback | 0.0519 | 0.6688 |

Trend is consistent across sampling rates: `operator_feedback` remains best, `self_filtered` improves over `static`, and only absolute values shift from 10s to 1s.
