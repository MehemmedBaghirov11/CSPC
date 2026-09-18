# CSPC - Computer Science for Physics and Chemistry

My coursework repository. Each practical is under `PW<n>/Lab <X>/`.

## Setup
Create the environment for a given lab:
    conda env create -f PW<n>/Lab\ <X>/environment.yml
    conda activate cspc

---

## PW1 - Lab A: Reproducible Foundations

**What I built:**
- A radioactive decay simulator comparing a pure-Python loop implementation against a vectorised NumPy implementation.

**Speed comparison (loop vs NumPy):**
- loop  : 2.773 s
- numpy : 0.000 s
- speed-up: 9598.8x faster

**Tests:** all passing?  yes

**Conclusion:**
- The NumPy vectorised version is dramatically faster than the pure-Python loop because it processes all atoms at once instead of looping through them individually. All three tests pass, confirming the simulation follows the expected exponential decay law within tolerance.



---

## PW1 - Lab B: Data, Plotting, and Automation

**What the data showed:**
- The observed decay data closely follows an exponential decrease over time, consistent with radioactive decay behavior.

**Comparison with the analytical law:**
- The observed data and the analytical curve (N0 * exp(-lambda * t)) show a strong visual match in shape, though the observed data has some noise/scatter around the smooth theoretical curve, as expected from real measurements.

**Snakemake pipeline:**
- The Snakefile automates figure generation: running `snakemake --cores 1 figure.png` only regenerates figure.png when decay_observed.csv or plot.py has changed, avoiding unnecessary recomputation.