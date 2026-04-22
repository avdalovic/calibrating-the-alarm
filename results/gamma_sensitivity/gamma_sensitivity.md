# Gamma Sensitivity (ACI Operator-Feedback Learning Rate)

- Calibration: `operator_feedback`
- Parameter under test: `gamma` (ACI learning rate)
- Fixed settings: `alpha=0.01`, `window=3600`, `damp_alpha=0.01`, `delay=60`, `review_prob=1.0`, `update_mode=damped`
- Score: `score_pred`

| Gamma | FPR | Recall | F1 |
|---:|---:|---:|---:|
| 0.0010 | 0.0204 | 0.7141 | 0.7633 |
| 0.0050 | 0.0273 | 0.7198 | 0.7458 |
| 0.0100 | 0.0298 | 0.7308 | 0.7456 |
| 0.0200 | 0.0310 | 0.7205 | 0.7355 |
| 0.0500 | 0.0357 | 0.7347 | 0.7312 |
| 0.1000 | 0.0375 | 0.7318 | 0.7243 |

Artifacts:
- `results/gamma_sensitivity/gamma_sensitivity.csv`
- `results/gamma_sensitivity/gamma_sensitivity.json`
- `results/gamma_sensitivity/metadata.json`
