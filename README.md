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