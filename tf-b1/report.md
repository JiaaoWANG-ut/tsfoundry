# TF-B1 results (equal-CPU-time benchmark, NEP89)

Cases 39 (27 metal, 12 zeolite) · jobs 780/780 with results · CI-NEB baseline: `ase-cineb-it` (higher success per CPU-h) · 95% CIs: case-cluster bootstrap (10000 resamples).

## Arms

| arm | sessions | failed | verified saddles U | CPU-h | U per CPU-h | median first saddle (CPU-s) | reference TS (strict) |
|---|---|---|---|---|---|---|---|
| tf-default | 78 | 0 | 492 | 3.99 | 123.5 | 10 | 38/78 |
| tf-best-81 | 78 | 0 | 498 | 3.82 | 130.3 | 10 | 42/78 |
| tf-fullstack-81 | 78 | 0 | 497 | 5.19 | 95.9 | 10 | 42/78 |
| tf-more-80 | 78 | 0 | 605 | 5.31 | 114.0 | 12 | 42/78 |
| tf-more-81 | 78 | 0 | 624 | 5.31 | 117.5 | 12 | 46/78 |
| tf-lowbarrier-80 | 78 | 0 | 297 | 3.48 | 85.3 | 12 | 40/78 |
| tf-lowbarrier-81 | 78 | 0 | 300 | 3.49 | 85.9 | 12 | 40/78 |
| ase-dimer-exact | 78 | 0 | 101 | 4.44 | 22.7 | 92 | 15/78 |
| ase-dimer-fd | 78 | 0 | 87 | 6.60 | 13.2 | 104 | 14/78 |
| ase-cineb | 39 | 0 | 25 converged first-order (35 converged) | 0.28 | 88.6 | 7 (successes) | 21/39 converged on it, 20 also first order (FS given) |
| ase-cineb-it | 39 | 0 | 26 converged first-order (37 converged) | 0.26 | 99.9 | 7 (successes) | 21/39 converged on it, 19 also first order (FS given) |

## Rate ratios (verified saddles per CPU-hour; low-barrier presets: saddles ≤ 1.5 eV vs baselines filtered identically)

| TF preset | vs ASE dimer (exact cert.) | vs ASE dimer (FD) | vs CI-NEB (ase-cineb-it) | vs CI-NEB (other) | vs TF default |
|---|---|---|---|---|---|
| tf-default | 5.43× [4.33, 7.32] | 9.37× [7.75, 11.84] | 1.24× [0.61, 2.33] | 1.39× [0.66, 2.66] | n/a |
| tf-best-81 | 5.73× [4.59, 7.69] | 9.89× [8.19, 12.45] | 1.30× [0.64, 2.46] | 1.47× [0.70, 2.76] | 1.06× [1.02, 1.09] |
| tf-fullstack-81 | 4.22× [3.28, 5.98] | 7.27× [5.94, 9.51] | 0.96× [0.46, 1.84] | 1.08× [0.50, 2.16] | 0.78× [0.70, 0.88] |
| tf-more-80 | 5.01× [3.98, 6.93] | 8.65× [7.13, 11.14] | 1.14× [0.56, 2.13] | 1.29× [0.61, 2.51] | 0.92× [0.85, 1.04] |
| tf-more-81 | 5.17× [4.03, 7.23] | 8.92× [7.23, 11.62] | 1.18× [0.58, 2.22] | 1.33× [0.63, 2.53] | 0.95× [0.86, 1.08] |
| tf-lowbarrier-80 | 6.11× [4.56, 8.65] | 9.88× [7.42, 13.64] | 0.85× [0.42, 1.58] | 0.96× [0.44, 1.93] | 1.14× [0.88, 1.45] |
| tf-lowbarrier-81 | 6.15× [4.66, 8.61] | 9.95× [7.52, 13.63] | 0.86× [0.42, 1.62] | 0.97× [0.44, 1.97] | 1.15× [0.91, 1.42] |

## Equal CPU time

| TF preset | saddles by CI-NEB's time (TF, no FS) | sessions with ≥ 1 | reference by then | CI-NEB saddles (FS given, ×2 seeds) | saddles by ASE-dimer's time | ASE dimer saddles |
|---|---|---|---|---|---|---|
| tf-default | 68 | 23/78 | 8 (CI-NEB 42) | 52 | 445 | 101 |
| tf-best-81 | 73 | 25/78 | 8 (CI-NEB 42) | 52 | 458 | 101 |
| tf-fullstack-81 | 71 | 23/78 | 8 (CI-NEB 42) | 52 | 415 | 101 |
| tf-more-80 | 59 | 18/78 | 6 (CI-NEB 42) | 52 | 492 | 101 |
| tf-more-81 | 62 | 18/78 | 4 (CI-NEB 42) | 52 | 509 | 101 |
| tf-lowbarrier-80 | 55 | 23/78 | 8 (CI-NEB 42) | 52 | 295 | 62 |
| tf-lowbarrier-81 | 57 | 24/78 | 8 (CI-NEB 42) | 52 | 299 | 62 |

## Metal vs zeolite

| TF preset | metal vs ASE dimer | zeolite vs ASE dimer | metal vs CI-NEB | zeolite vs CI-NEB |
|---|---|---|---|---|
| tf-default | 12.65× [9.90, 16.40] | 4.16× [3.35, 5.18] | 2.44× [1.41, 4.10] | 2.08× [0.51, 7.80] |
| tf-best-81 | 12.59× [9.88, 16.34] | 4.49× [3.59, 5.69] | 2.43× [1.39, 4.08] | 2.24× [0.54, 8.68] |
| tf-fullstack-81 | 12.74× [9.98, 16.56] | 3.16× [2.32, 4.27] | 2.46× [1.41, 4.13] | 1.58× [0.37, 6.22] |
| tf-more-80 | 10.88× [8.46, 14.24] | 3.85× [3.11, 4.87] | 2.10× [1.21, 3.52] | 1.92× [0.48, 7.34] |
| tf-more-81 | 10.90× [8.48, 14.28] | 4.09× [3.17, 5.50] | 2.10× [1.20, 3.55] | 2.04× [0.49, 8.03] |
| tf-lowbarrier-80 | 12.24× [8.99, 17.15] | 5.56× [3.69, 8.28] | 2.09× [1.16, 3.55] | 0.91× [0.22, 3.44] |
| tf-lowbarrier-81 | 12.52× [9.25, 17.52] | 5.72× [3.89, 8.50] | 2.13× [1.19, 3.62] | 0.93× [0.23, 3.62] |
