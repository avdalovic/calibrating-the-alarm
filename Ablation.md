# Sampling-Rate Ablation (10s vs 1s)

This note summarizes a focused ablation for paper discussion: do the calibration trends observed at 10s sampling remain when evaluated at the original 1s sampling?

## Datasets and Evaluation Environments
We evaluate on the Secure Water Treatment (SWaT) and Water Distribution (WADI) testbeds, with 51 and 120 process variables (sensor readings and actuator states), respectively. Following prior work, our main setting uses 10-second subsampling.

In our temporal splits, SWaT has 44,490 training, 4,890 validation, and 44,932 test observations (39,772 benign, 5,160 attack). WADI has 108,805 training, 12,036 validation, and 17,221 test observations (16,227 benign, 994 attack).

We report three evaluation environments: `static`, `self_filtered`, and `operator_feedback`.

The goal of this ablation is not to replace the 10s main result, but to verify that the relative calibration trend is preserved at 1s.

## Metrics Used Here
To keep this appendix-style artifact concise, we report only:
- `FPR` (false positive rate, lower is better)
- `F1` (higher is better)

## Results (Recovered From Existing Artifacts)

| Sampling | Calibration | FPR (lower is better) | F1 (higher is better) |
|---|---|---:|---:|
| 10s | static | 0.7406 | 0.2514 |
| 10s | self_filtered | 0.1473 | 0.5574 |
| 10s | operator_feedback | 0.0298 | 0.7456 |
| 1s | static | 0.7284 | 0.2518 |
| 1s | self_filtered | 0.1462 | 0.3640 |
| 1s | operator_feedback | 0.0519 | 0.6688 |

## Takeaway for the Paper
The same ordering is retained across sampling rates:
- `operator_feedback` remains best overall.
- `self_filtered` consistently improves over `static`.
- moving from 10s to 1s changes absolute values, but the calibration trend stays intact.

This supports the paper claim that adaptive calibration behavior is robust to sampling-rate changes, even when evaluated at the native 1s setting.

## Files
- `results/ablation_sampling_rate/ablation_sampling_rate_fpr_f1_table.md`
- `results/ablation_sampling_rate/ablation_sampling_rate_fpr_f1.csv`
- `results/ablation_sampling_rate/ablation_sampling_rate_from_existing_results.csv`

## Provenance
Numbers in this document are recovered from existing SWaT 10s/1s three-calibration artifacts, not newly retrained runs.
