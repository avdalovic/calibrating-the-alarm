# Calibrating the Alarm

This repository is the public supporting artifact for the paper
"Calibrating the Alarm: Why Deep Learning ICS Anomaly Detectors Fail at Deployment".

Repository URL
https://github.com/avdalovic/calibrating-the-alarm

## What is included
This repo currently focuses on the sampling rate ablation artifact used in the paper discussion.
It reports only the deployment metrics used for this claim

1. FPR (lower is better)
2. F1 (higher is better)

## Sampling rate ablation

| Sampling | Calibration | FPR | F1 |
|---|---|---:|---:|
| 10s | static | 0.7406 | 0.2514 |
| 10s | self_filtered | 0.1473 | 0.5574 |
| 10s | operator_feedback | 0.0298 | 0.7456 |
| 1s | static | 0.7284 | 0.2518 |
| 1s | self_filtered | 0.1462 | 0.3640 |
| 1s | operator_feedback | 0.0519 | 0.6688 |

The trend is consistent across sampling rates.
operator_feedback remains best, self_filtered improves over static, and only absolute values shift from 10s to 1s.

## Files
1. `Ablation.md`
2. `results/ablation_sampling_rate/ablation_sampling_rate_fpr_f1_table.md`
3. `results/ablation_sampling_rate/ablation_sampling_rate_fpr_f1.csv`
4. `results/ablation_sampling_rate/ablation_sampling_rate_from_existing_results.csv`

## Note
These numbers are recovered from existing SWaT 10s and 1s artifacts and are provided as a clean reference table for the manuscript.
