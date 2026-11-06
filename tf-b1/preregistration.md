# TF-B1 — equal-CPU-time benchmark for the Transition Foundry landing page (preregistration)

Frozen before the main run. `bench.py`, `cases.json` and `jobs.json` are hashed into `preregistration.sha256`.

## Question
In the same CPU time, how many more **exact-Hessian-verified first-order saddle points** does Transition Foundry
(`dimer-exploration` 0.81.0, NEP89) return than (a) the ASE dimer method and (b) ASE CI-NEB, on held-out
surface and zeolite reactions?

## Set
The 39 sealed S5 TS-first initial states of GF8 (27 metal surfaces: fcc(221), bcc(211), hcp(10-10), single-atom
alloy (111), vacancy (111); 12 zeolite frameworks). They were never used for tuning. Each has a factory reference TS
and product (`results/kit/prepared-factory5{m,p}/index.json`, `split == "S5"`, `stratum == "tsfirst"`).

## Arms
| arm | what runs | final state given? | seeds |
|---|---|---|---|
| `tf-default` | `dimer-explore` CLI default preset `long-best-auto-80` | no | 455300, 455301 |
| `tf-best-81`, `tf-fullstack-81`, `tf-more-80`, `tf-more-81`, `tf-lowbarrier-80`, `tf-lowbarrier-81` | the other ranked GF8 presets | no | same |
| `ase-dimer-exact` | ASE dimer (`MinModeAtoms`/`MinModeTranslate`, preset `classic`: 10 launches from random directions) **with the same exact-Hessian verifier** (`--exact-hessian` overrides) | no | same |
| `ase-dimer-fd` | ASE dimer as shipped (`classic`, finite-difference certificates) | no | same |
| `ase-cineb` | ASE 3.26 `NEB(images, k=0.1, climb=True, method="aseneb")` (ASE defaults), 7 intermediate images, IDPP interpolation (mic), FIRE to fmax 0.05 eV/Å, ≤ 2000 steps | **yes (reference FS)** | deterministic |
| `ase-cineb-it` | as above but VTST-style: `method="improvedtangent"` (Henkelman & Jónsson 2000), plain NEB to 0.5 eV/Å, then climbing image to 0.05 eV/Å, ≤ 2000 steps in total | **yes (reference FS)** | deterministic |

CI-NEB uses one caching calculator per image (standard ASE usage; ASE's optimizer requests the band gradient
about 3× per step, and a single shared calculator would recompute every image each time; verified on an EMT
toy: 1.01 vs 3.01 evaluations per image-step, identical trajectory). A smoke run of the ASE-default protocol
diverged on a zeolite case (band energy +20 eV after 2000 steps), so the VTST-style protocol was added before
the main run. **The CI-NEB baseline in every headline ratio is whichever of the two protocols has the higher
pooled success rate per CPU-hour** (the stronger competitor); both are reported.

All campaign arms run through the same `run_campaign` path as the GF8 pool (prepared IS certificate, serial session).
Configurations come from `explore.resolve_config` exactly as the CLI builds them with `--workers 1`.

## Timing
One Perlmutter CPU node (premium QoS). Every job is its own process pinned to one physical core with one thread.
CPU time = `time.process_time()` of the job: model load, JAX exact-Hessian kernels (shared warm persistent cache),
search, validation, certification. Python imports are excluded for every arm. Kernel `rusage` is also recorded.
All arms are interleaved in one shuffled queue in the same allocation (same contention for every arm).

## Counting
* Campaign arms: U = channels whose final dense certificate passed (λ1 < −1e-4 eV/Å², λ2 > +1e-4, product a strict
  minimum) and that connect to the IS by two-sided downhill validation; distinct by the package matcher.
* CI-NEB: success = converged within 2000 steps **and** the climbing image has exactly one negative eigenvalue of the
  exact NEP89 Hessian in the same reduced coordinates and tolerance. No connectivity check and a 25× looser force
  threshold than the campaign arms (both lenient to CI-NEB). CPU = IDPP + NEB + one exact Hessian.
* Failed jobs count as zero saddles with their CPU time.

## Endpoints
* **Primary:** pooled rate ratio R = (ΣU_TF / ΣCPU_TF) / (ΣU_X / ΣCPU_X) for X ∈ {ASE dimer (exact), ASE dimer (FD),
  CI-NEB}, with a 95% case-cluster bootstrap CI (10,000 resamples of the 39 cases, seed 20261105).
* Secondary:
  1. Equal-time counts. For each case and seed, U_TF(t) at t = the competitor's CPU time on that case, against the
     competitor's U (CI-NEB: 0/1). Conservative timing: all CPU outside the launches is charged at t = 0.
  2. Reference-TS recovery (package matcher: 0.02 eV, 0.10 Å RMSD, 0.25 Å max; the lenient 0.05 eV / 0.20 Å is
     also reported). TF without the FS vs CI-NEB with the FS.
  3. Median CPU time to the first verified saddle.
  4. Metal vs zeolite breakdown.
  5. The six other presets. Low-barrier presets are compared within the ≤ 1.5 eV window, with the baselines filtered
     identically.

## Reporting rule
The landing page quotes the primary ratios with their CIs and states that CI-NEB answers a different question
(one TS for a product you already know). Every number is reported whatever the outcome; energies are NEP89 only.
