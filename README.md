# Wasserstein median: Gaussian contamination example

This repository reproduces Figure 1 of the manuscript titled **Uniqueness of Wasserstein medians under compatible transport**. It features the following Jupyter notebook:

- [`gaussian-contamination.ipynb`](gaussian-contamination.ipynb): a self-contained, executed notebook with the simulation, solvers, checks, and figure.

The notebook generates all inputs from a fixed random seed and recomputes every curve point. It does not read external data, access the network, or depend on the manuscript source. The figure is embedded in the notebook, so no separate image or data files are needed.

## Run

Use Python 3.11 and install the checked package versions in your Python environment:

```sh
python -m pip install numpy==2.3.5 matplotlib==3.10.8 threadpoolctl==3.6.0 jupyterlab==4.6.1
python -m jupyterlab gaussian-contamination.ipynb
```

Choose **Restart Kernel and Run All Cells**. The notebook prints the package versions, computes all 50 contamination settings, checks the results, and displays Figure 1. The numerical sweep normally takes seconds on a recent computer; plotting with LaTeX takes additional time. Running all cells creates no additional files in the repository. Optional PDF and PNG export instructions are included at the end of the notebook.

## What the figure shows

Six signal Gaussians use the sample means and unbiased covariances of six independent samples of 100 observations from the two-dimensional standard normal distribution. Four noise Gaussians have displaced first-quadrant means and rotated, stretched covariances. The seed is **20260925**; the notebook specifies all noise parameters.

- **(a)** The ten input laws, represented by unfilled 95% probability ellipses at opacity 0.2.
- **(b)** Their equally weighted Wasserstein barycenter and numerical median, with the standard normal target. Equal weights correspond to **40% total noise weight**.
- **(c)** Wasserstein errors to the target at total noise weights from **0% to 49%**, in one-percentage-point increments.

The ten laws are held fixed. Each signal receives weight `(1 - epsilon) / 6` and each noise law receives `epsilon / 4`; the curve changes weights rather than input counts. The ellipses describe probability content under each Gaussian, not uncertainty in an estimated mean. This is a single simulated realization, not a repeated-sampling average.

## Computation and checks

The code uses the Gaussian quadratic Wasserstein formula, a Bures fixed-point iteration for the barycenter covariance, and inverse-distance majorization-minimization for the median. It selects the lowest objective among three starts and the active input laws. There is no entropic regularization or support discretization. The target is used for generation, plotting, and error evaluation, never for choosing a median candidate.

All 150 MM runs are checked for convergence, together with covariance definiteness, barycenter residuals, objective descent, and agreement across starts. The notebook also checks these reference errors (small platform-dependent rounding differences are allowed):

| Total noise weight | Barycenter error | Median error |
|---|---:|---:|
| 0% | 0.046016 | 0.067419 |
| 40% | 3.530443 | 0.187069 |
| 49% | 4.317351 | 0.457239 |

The displayed median is a numerical candidate; the checks do not certify global optimality or uniqueness. The example does not impose compatible transport geometry on its inputs.

## Figure typography

The notebook preserves the original panel dimensions, colors, limits, equal spatial scales, and font size. `RENDER_MODE = "auto"` chooses between:

- **LaTeX:** the manuscript's PGF renderer and CAS font selection, with 10-point type on a 164.6 mm-wide canvas. This needs a TeX installation providing `pdflatex`, `kpsewhich`, `pgf`, and `stix`, plus Poppler's `pdftocairo`, with the executables on `PATH`. The checked manuscript uses STIX; the same CAS font-selection rule uses Charis text if `charis.sty` is installed.
- **Matplotlib:** a portable preview using bundled STIX fonts, with the same calculations and panel geometry. Text layout can differ slightly from the LaTeX version.

Set `RENDER_MODE = "tex"` to require the manuscript renderer, or `"matplotlib"` to avoid the external typesetting dependencies. The saved notebook output uses the LaTeX renderer. Both modes produce an inline PNG and keep a vector PDF in memory for optional export.

## References

- Álvarez-Esteban et al. (2016), *A fixed-point approach to barycenters in Wasserstein space*. [DOI](https://doi.org/10.1016/j.jmaa.2016.04.045).
- You, Shung, and Giuffrè (2025), *On the Wasserstein median of probability measures*. [DOI](https://doi.org/10.1080/10618600.2024.2374580).

This is a new shifted Gaussian illustration inspired by the latter paper's Gaussian example. OpenAI Codex assisted with code, numerical checks, and notebook preparation. The figures are generated from the simulation rather than supplied as static inputs.
