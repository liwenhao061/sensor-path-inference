# Sensor Path Inference

Exact path posterior inference from incomplete fixed-sensor observations.

## Overview

Fixed-location sensors such as automatic vehicle identification and license plate recognition cameras record a vehicle only at some links on its path and may miss it. The complete time-dependent path of each trip must therefore be inferred from an incomplete record.

The Link–Time–Hit (LTH) framework separates three layers that existing methods mix:

1. **Route-choice prior.** Any admissible Markov prior over link transitions in a time-expanded network.
2. **Posterior inference.** A product graph of links, time, and matched detections. One sum recursion gives the observation evidence and the link–time marginals, and one max recursion gives the most probable complete path.
3. **Prior estimation.** The exact posterior yields gradients for estimating the prior from sensor records, complete-path labels, or both.

Because inference is exact for every prior, priors can be compared under the same inference method, and the effect of the inference method can be measured with the prior held fixed.

## Methods

| Method | Description |
|---|---|
| **Uniform** | Uniform choice over admissible actions. |
| **TDSP-Soft** | Softened time-dependent shortest path, temperature selected by validation evidence. |
| **LT-Param / LT-Emp** | Parametric and empirical link-transition priors. |
| **DY-RL** | Dynamic recursive logit estimated by exact maximum likelihood of the sensor records. |
| **Linear-MCEM** | Linear maximum-entropy IRL estimated by Monte Carlo EM. |
| **TDSP-NP** | Neural policy prior trained by imitating time-dependent shortest paths. |
| **NP-EM** | Neural policy estimated by generalized EM of the sensor-record likelihood. |
| **Neural-ME** | Neural maximum-entropy reward estimated by exact maximum likelihood. |
| **NP-Joint** | Neural policy estimated from complete-path labels and sensor records. |
| **TDSP-NP+AIRL** | Posterior-weighted adversarial refinement of TDSP-NP. |
| **AIRL-CP** | Adversarial reference trained on complete paths. |

Every prior is evaluated with **Exact LTH Inference**. Selected priors are also evaluated with **Approximate LTH Inference**, a particle sampler with *K* particles.

## Benchmark

The experiments use a semi-synthetic benchmark on a Shanghai network of 714 directed links. Real taxi routes are replayed through a dynamic travel-time field, and arrivals at detector links are recorded by Bernoulli thinning. Two detector conditions (train-only placement and sparse random placement), 1,500 test trips, and ten seeds per condition are used.

## Code structure

Planned public-release layout, based on the current implementation:

```text
sensor-path-inference/
├── src/lth/
│   ├── network/        # Time-expanded network, travel-time field, reachability
│   ├── benchmark/      # Route replay, detector placement, detection thinning
│   ├── priors/         # Specified, link-transition, linear-reward, and neural priors
│   ├── inference/      # Product graph, feasibility pass, sum and max recursions, particle sampler
│   ├── estimation/     # Imitation, generalized EM, exact ML, Monte Carlo EM, adversarial refinement
│   └── evaluation/     # Path, link, marginal, and flow metrics; paired statistics
├── experiments/
│   ├── prior_by_inference/  # Main comparison of priors under exact inference
│   ├── particle_sweep/      # Sampling versus exact inference and computational cost
│   ├── label_budget/        # Complete-path labels and training-pool size
│   ├── robustness/          # Timestamp tolerance and detection-rate misspecification
│   └── ablations/           # Observation and model ablations
├── configs/             # Benchmark, model, and experiment configurations
├── scripts/             # Figure and table generation
├── tests/               # Unit, integration, and regression tests
├── environment.yml      # Pinned Python environment
└── README.md
```

## Installation and usage

The implementation uses Python 3.10, PyTorch, and NumPy, and runs on CPU. Installation instructions and runnable examples will accompany the source-code release.

## Code and data availability

Source code will be released in this repository after manuscript acceptance. The route data are the released and preprocessed Shanghai taxi route sequences of Zhao and Liang (2023), and their availability follows that release. The benchmark-generation settings will be provided with the code.

## Citation

Publication details and a BibTeX entry will be added when available.

## License

The software license will be specified with the source-code release.
