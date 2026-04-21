# Lambda Sensitivity (Self-Filtered Calibration)

- Dataset: `SWaT`
- Model: `GRU` (pretrained run artifact)
- Calibration: `self_filtered` only
- Score: `score_pred` (L2 residual stream)
- W: `3600`
- alpha: `0.01`

| Lambda | FPR | Recall | F1 |
|---:|---:|---:|---:|
| 0.0050 | 0.2627 | 0.8413 | 0.4352 |
| 0.0100 | 0.1473 | 0.8252 | 0.5574 |
| 0.0200 | 0.0521 | 0.7409 | 0.6916 |
| 0.0500 | 0.0504 | 0.6122 | 0.6120 |
| 0.1000 | 0.0481 | 0.4310 | 0.4785 |

Artifacts:
- `results/lambda_sensitivity/lambda_sensitivity.csv`
- `results/lambda_sensitivity/lambda_sensitivity.json`
- `results/lambda_sensitivity/metadata.json`
