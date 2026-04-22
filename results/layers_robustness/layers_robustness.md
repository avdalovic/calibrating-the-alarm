## Number of Layers Robustness (Hidden = 64)

| Layers | Hidden | Calibration | FPR | Recall | F1 |
|---:|---:|---|---:|---:|---:|
| 1 | 64 | static | 0.7300 | 0.9438 | 0.2493 |
| 1 | 64 | self_filtered | 0.1395 | 0.7721 | 0.5423 |
| 2 | 64 | static | 0.7191 | 0.9486 | 0.2533 |
| 2 | 64 | self_filtered | 0.1422 | 0.7641 | 0.5344 |
| 3 | 64 | static | 0.7418 | 0.9610 | 0.2503 |
| 3 | 64 | self_filtered | 0.1539 | 0.7961 | 0.5339 |

`self_filtered` remains clearly better than `static` across 1-3 layers, with stable F1 and much lower FPR.
