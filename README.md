# hearo-sample-size-calculator

Interactive tool for estimating the required sample size for Hearo's clinical validation study comparing AI-assisted otoscopy to standard otoscopy.

**[Live Calculator →](https://vkouloumentas3.github.io/hearo-sample-size-calculator/)**

---

## Study Design

- **Design:** Paired — each subject's ear is examined by both Doctor A (using Hearo) and Doctor B (using standard otoscopy, ground truth)
- **Outcome:** Binary — diagnosis is either concordant (correct) or discordant (incorrect)
- **Unit of analysis:** Per subject, one ear per patient
- **N from calculator** = number of subjects = number of ears. Each ear gets two exams (one per method), so N = 200 means 200 ears and 400 total exams.

## Parameters You Need to Decide

| Parameter | What it means | How to determine it |
|-----------|--------------|---------------------|
| **p** (expected concordance) | Probability that Hearo's diagnosis matches standard otoscopy | From published tele-otoscopy / digital otoscope literature, or clinician estimate |
| **δ₀** (margin) | Maximum acceptable drop in accuracy for Hearo to still be considered non-inferior or equivalent | Clinical judgment — agreed upon by clinicians and statisticians |
| **α** (Type I error) | Risk of falsely concluding Hearo works when it doesn't | Convention: 0.05 |
| **1 − β** (Power) | Probability of detecting a real difference if one exists | Convention: 0.80 |

## Formulas Used

From [Zhong (2009), *J Thorac Dis*](https://doi.org/10.3978/j.issn.2072-1439.2009.12.01):

**Non-inferiority** (one-sided):

$$N = 2 \times \left(\frac{z_{1-\alpha} + z_{1-\beta}}{\delta_0}\right)^2 \times p(1-p)$$

**Equivalence** (two-sided):

$$N = 2 \times \left(\frac{z_{1-\alpha/2} + z_{1-\beta}}{\delta_0}\right)^2 \times p(1-p)$$

Where *N* = number of subjects (= number of ears in the one-ear-per-subject design).

## Using Both Ears (Design Effect Correction)

The calculator assumes **one ear per subject**. If you want to analyze both ears per patient, the two observations are not independent — they share anatomy, patient cooperation, operator skill, etc.

To account for this, multiply *N* by the **design effect (DE)**:

$$N_{\text{adjusted}} = N \times DE$$

$$DE = 1 + (m - 1) \times \rho$$

Where:
- **m** = number of ears per subject (m = 2)
- **ρ** (rho) = intraclass correlation coefficient (ICC) between ears from the same patient

This simplifies to:

$$DE = 1 + \rho$$

### How to determine ρ

- **Best:** Calculate ICC from pilot data or a preliminary dataset of paired ear diagnoses
- **Literature:** Look for ICC values in otoscopy or tele-otoscopy concordance studies with bilateral exams
- **Conservative default:** If unknown, use ρ = 0.5 as a reasonable starting point for paired anatomical sites

### Example

With ρ = 0.5:

$$DE = 1 + 0.5 = 1.5$$

If the calculator gives N = 200 → with one ear that's 200 ears, 400 exams. Adjusting for both ears: N × DE = 200 × 1.5 = **300 subjects** (600 ears, 1200 exams).

A lower ρ (e.g., 0.2) gives DE = 1.2 → 240 subjects. A higher ρ (e.g., 0.8) gives DE = 1.8 → 360 subjects.

## Reference

Zhong, B. (2009). How to Calculate Sample Size in Randomized Controlled Trial? *Journal of Thoracic Disease*, 1(1), 51–54.
