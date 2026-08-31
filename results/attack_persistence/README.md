# Attack Persistence Under Rolling Calibration

## Question

The self-filtered buffer continues updating during deployment, so repeated attack observations may move the rolling threshold. Threshold movement becomes a detection problem only if the threshold catches up with an ongoing attack and suppresses alarms later in the same episode.

This analysis uses only native SWaT attack trajectories. No attacks, telemetry, labels, detector scores, or calibration rules were modified.

## Setup

- SWaT, 10-second sampling
- GRU, L2 anomaly score
- W = 3600
- alpha = 0.01
- lambda = 0.01
- ten trained model seeds
- 32 native attack episodes
- attack labels used only retrospectively for evaluation

For each attack, we compare recall in its first and second halves. If rolling contamination progressively suppresses a previously detected attack, recall should decline as the episode continues.

## Sustained attacks

Sustained attacks are episodes lasting at least 10 minutes.

| Attack | Target | Duration (min) | Max buffer attack % | Threshold max / pre | Recall first half | Recall second half | Change |
|---:|---|---:|---:|---:|---:|---:|---:|
| 0 | MV101 | 15.7 | 2.6 | 1.02 | 0.338 | 0.255 | -0.083 |
| 5 | DPIT301 | 16.2 | 9.3 | 1.44 | 1.000 | 1.000 | +0.000 |
| 6 | FIT401 | 12.0 | 11.3 | 2.32 | 1.000 | 1.000 | +0.000 |
| 9 | MV303 | 11.7 | 3.4 | 1.28 | 0.229 | 1.000 | +0.771 |
| 12 | LIT101 | 11.8 | 7.1 | 1.00 | 0.000 | 0.000 | +0.000 |
| 14 | DPIT301 | 11.5 | 6.9 | 1.04 | 0.991 | 1.000 | +0.009 |
| 16 | LIT401 | 10.0 | 4.4 | 1.00 | 0.000 | 0.000 | +0.000 |
| 17 | P101,LIT301 | 24.0 | 6.5 | 1.21 | 0.986 | 1.000 | +0.014 |
| 19 | P302 | 570.2 | 96.5 | 4.21 | 0.973 | 1.000 | +0.027 |
| 20 | P101,MV201,LIT101 | 19.3 | 54.7 | 1.01 | 0.474 | 0.597 | +0.122 |
| 22 | LIT301 | 10.0 | 1.6 | 1.00 | 0.033 | 0.000 | -0.033 |
| 31 | LIT301 | 27.7 | 8.3 | 1.00 | 0.000 | 0.000 | +0.000 |

Aggregate result:

- 12 attacks last at least 10 minutes.
- None loses more than 0.10 recall from the first half to the second half.
- Mean second-half minus first-half recall across all 32 attacks = +0.013.

This does not prove robustness to arbitrary poisoning.

## Most severe native contamination case

Attack 19 lasts 570.2 minutes. Attack-labeled observations occupy up to 96.5% of the W = 3600 rolling window, and tau_max / tau_pre reaches 4.21. Its first-half recall is 0.973, second-half recall is 1.000, and overall recall is 0.986.

This episode demonstrates that substantial threshold movement can occur, but threshold movement alone is not the failure mechanism. Despite the large movement, detection does not deteriorate as the attack continues.

## Gradual-profile attacks

The repository metadata marks attacks 2, 8, and 31 with profile `line`.

| Attack | Target | Duration (min) | Threshold max / pre | Overall recall |
|---:|---|---:|---:|---:|
| 2 | LIT101 | 6.3 | 1.00 | 0.000 |
| 8 | LIT301 | 4.7 | 1.00 | 0.036 |
| 31 | LIT301 | 27.7 | 1.00 | 0.000 |

Their thresholds remain essentially unchanged during their attack intervals. When these attacks are missed, they are missed from onset rather than after a threshold increase, so these cases do not show the calibrator first detecting an attack and then absorbing it into the baseline.

This pattern is evidence consistent with insufficient attack-score separation, not a formal causal proof of why the upstream model misses them.

## Scope

The native SWaT analysis does not show systematic within-attack detection suppression under the non-adaptive attacks evaluated in the paper. It does not establish robustness to a threshold-aware adversary deliberately designed to manipulate the rolling calibration state.

Representative plots are provided in [`figures/`](figures/).

## Raw results

- [`attack_persistence.csv`](attack_persistence.csv)
- [`sustained_attacks.csv`](sustained_attacks.csv)
