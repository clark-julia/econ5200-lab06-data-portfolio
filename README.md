# econ5200-lab06-data-portfolio
# Experiment Design Audit — Power, Selection & Weighting

## Objective

I audited an underpowered A/B test and a biased observational comparison, then corrected the second one with inverse probability weighting.

## Methodology

- Re-ran a product manager's two-proportion z-test on a rollout that gave the new onboarding flow to 2% of traffic (1,000 treatment users vs. 49,000 control), which showed a 0.8pp lift.
- Computed Cohen's h for the 5.0% vs. 5.8% conversion rates and used statsmodels to find the sample size needed for 80% power at a 5% significance level, and the power the experiment actually had.
- Wrote my own `power_check` function from the normal-approximation power formula and compared it with the statsmodels result. I then used it to find the power of an even 25,000 / 25,000 split of the same 50,000 users.
- Simulated a voluntary wellness program where healthier employees were more likely to join, with a known true effect of -$500 on healthcare costs.
- Ran the naive participant vs. non-participant comparison and identified baseline health as the confounder (baseline health → treatment, baseline health → costs).
- Fit a logistic regression propensity model on baseline health and checked overlap.
- Weighted participants by 1/e(x) and non-participants by 1/(1 − e(x)), then compared weighted group means.

## Key Findings

- The A/B test had 19.8% power to detect a 0.8pp lift. Reaching 80% power would have taken 12,514 users per group, and the treatment group had 1,000. The non-significant result says little about whether the effect is real, because the test was too small to detect it.
- My `power_check` function matched statsmodels. The same 50,000 users split evenly would have had 97.7% power.
- The naive wellness-program estimate was -$1,396 against a true effect of -$500. Healthier people both joined more often and cost less, so the comparison overstated the savings.
- Inverse probability weighting brought the estimate to -$522, close to the true -$500.
- Both problems came from design. The test's sample was split too unevenly to have power, and the wellness comparison had self-selected groups. Large samples would not have fixed either one.
