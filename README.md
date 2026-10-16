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

---

## PW3: Data, Distributions, Testing, and Causation

### Dataset 1: Heart Disease (Cleveland)

Source: Kaggle / UCI Machine Learning Repository. 303 patients, 14 columns, no missing values in the four continuous columns I used (age, chol, trestbps, thalach).

#### Session 1 (Tasks 1-3, code: `session1.py`)

**Task 1, loading the data:**
- Loaded `heart.csv` with pandas. Shape is (303, 14). The four continuous columns are age (mean 54.4), chol (mean 246.7), trestbps (mean 131.7) and thalach (mean 149.6).

**Task 2, shape of each variable (see `distributions.png`):**
- `age`: spread mostly between 40 and 70, with a rough, uneven top around 55-60.
- `chol`: one peak near 200-250 and a long tail to the right, with one extreme value near 560.
- `trestbps`: peak around 120-130 and a tail to the right up to 200.
- `thalach`: peak around 150-170 and a tail to the left down to 70.

**Task 3, normality verdicts:**

I used two checks: the Shapiro-Wilk test (numerical) and Q-Q plots (graphical, see `qqplots.png`). With 303 patients Shapiro-Wilk is very sensitive, and it rejected normality for all four variables (age p = 0.006, chol p = 5.9e-9, trestbps p = 1.8e-6, thalach p = 7.0e-5). So I also used the Q-Q plots to judge how far each variable is from normal.
- `age`: approximately normal. The points follow the line closely, with only small deviations at the extreme ends.
- `thalach`: approximately normal. The points follow the line except for a small deviation in the lower tail (a slight skew to the left).
- `trestbps`: not normal. The upper tail rises above the line, so it is skewed to the right.
- `chol`: not normal. It is clearly skewed to the right, with one extreme value (564) far from the rest.

#### Session 2 (Tasks 4-5, code: `session2.py`)

**Task 4, is the effect real? (see `thalach_groups.png`):**
- I split `thalach` by `target`: 139 patients with heart disease and 164 healthy patients. Mean thalach is 139.3 for the disease group and 158.4 for the healthy group.
- In Session 1, `thalach` was approximately normal on the Q-Q plot but Shapiro-Wilk rejected normality (p = 7.0e-5), so it is a borderline case. Because of this I ran both a t-test (assumes normality) and a Mann-Whitney U test (does not). Both agree: t-test p = 3.5e-14 and Mann-Whitney p = 1.9e-13. The groups genuinely differ: patients with heart disease reach a maximum heart rate about 19 beats per minute lower than healthy patients.
- Uncertainty: I used the standard error of the mean (SEM): 1.92 for the disease group and 1.50 for the healthy group. The bar plot shows each mean with a 95% confidence interval (1.96 x SEM). The two error bars do not overlap (about 135.5-143.0 vs 155.4-161.3), so the picture agrees with the tests.

**Task 5, do age and thalach move together? (see `age_thalach.png`):**
- I computed both Pearson and Spearman correlation, since `thalach` was only borderline normal. They agree: Pearson r = -0.39 (p = 1.1e-12) and Spearman r = -0.39 (p = 1.5e-12).
- Interpretation: age and maximum heart rate move in opposite directions. Older patients tend to reach a lower maximum heart rate. The relationship is moderate (r about -0.4), not strong: the scatter plot shows a clear downward trend but with a wide spread around the line. It is very unlikely to be due to chance.

### Dataset 2: A chemical-exposure mystery (synthetic), Session 2 (Tasks 6-7, code: `chemicals.py`)

**Task 6, the naive analysis:**
- (to be filled in after running `chemicals.py`)

**Task 7, look again (confounding):**
- (to be filled in after the confounder analysis)