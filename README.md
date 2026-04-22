# Calibrating the Alarm

Public supporting artifact for the paper "Calibrating the Alarm: Why Deep Learning ICS Anomaly Detectors Fail at Deployment".

Repository URL
https://github.com/avdalovic/calibrating-the-alarm

## Scope
This repository contains robustness artifacts that supplement Section 6 of the paper.

## Repository layout
```
calibrating-the-alarm/
├── README.md
├── ROBUSTNESS.md
├── results/
│   ├── sampling_rate/
│   ├── lambda_sensitivity/
│   ├── layers_robustness/
│   └── degraded_feedback/
└── ...
```

## Main artifact
See `ROBUSTNESS.md` for compact robustness tables and short interpretation notes.

## Current populated robustness results
1. `results/sampling_rate/`
2. `results/lambda_sensitivity/`
3. `results/layers_robustness/`
4. `results/degraded_feedback/`

The lambda sweep confirms smooth FPR control and a best observed F1 at `lambda = 0.02`, while the paper default remains `lambda = 0.01` as a conservative pre-fixed setting.
The layer-count robustness sweep (1 to 3 layers, hidden size 64) preserves the same conclusion: `self_filtered` keeps much lower FPR than `static` with stable F1.
The degraded-feedback grid shows robust false alarm control under delayed and noisy operator feedback.
