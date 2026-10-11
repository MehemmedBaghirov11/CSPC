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

---

## PW2 - Lab A: Motion from Tracking Data

**Mean acceleration measured:**
- -8.58 m/s^2 (standard deviation: 28.72), compared to the expected -9.81 m/s^2.

**Why the acceleration was noisy:**
- Differentiation amplifies measurement noise: small errors in the smooth position data get magnified once when computing velocity, and magnified again when computing acceleration from velocity. That's why the standard deviation (28.72) is much larger than the mean itself.

**What integrating back showed:**
- Integrating the noisy acceleration back up to velocity and then position recovered a curve very close to the original: the maximum difference was about 0.78 meters. This confirms that integration suppresses noise (the opposite effect of differentiation), since random errors partly cancel out when summed.

---

## PW2 - Lab B: Optimization in Chemistry

**Three optimisation methods (Part 2):**
- On the easy convex function f(x) = (x-3)^2 + 1, gradient descent, Newton and SLSQP all reached x = 3 from x0 = 0, so the methods agree.
- On the harder function g(x) = x^4 - 3x^2 + x + 5 (two minima and one maximum), the methods did not agree. From x0 = 0, gradient descent and SLSQP reached the global minimum at x = -1.30, but Newton landed on x = 0.17, which is a maximum (g'' = -5.65 < 0), because Newton only finds a point where g'(x) = 0. From x0 = 2, gradient descent and Newton found the local minimum at x = 1.13 (g'' = 9.35 > 0), while SLSQP found the global minimum at x = -1.30.
- Lesson: on a simple problem the methods agree, but on a complicated landscape the starting point and the algorithm matter.

**Reaction rate constant (Part 3):**
- Fitting C(t) = C0 * exp(-k*t) to the noisy data gave k = 0.262, close to the expected 0.25, and the fitted curve passes through the data points.

**Chemical equilibrium (Part 4):**
- With K = 15.6, Newton (root-finding) gave x = 0.6638 and SLSQP (minimisation) gave x = 0.6638, so both methods agree.
- Equilibrium composition: H2 = 0.336 mol, I2 = 0.336 mol, HI = 1.328 mol. The reactants fall and the product rises as x increases.

**Titration equivalence point (Part 5, bonus):**
- The slope of the pH curve is largest at about 50.0 mL, so the equivalence point is at about 50 mL.