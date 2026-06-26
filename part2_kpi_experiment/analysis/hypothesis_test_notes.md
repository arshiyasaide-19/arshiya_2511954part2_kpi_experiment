# Hypothesis Test Notes

## Business Decision Context
Leadership needs to decide whether to roll out the new onboarding and activation campaign (Treatment) to all users, replacing the existing experience (Control).

---

## Test 1: Paid Conversion Rate (North Star Metric)

**Metric tested**: Paid conversion rate (converted_to_paid)

**Null Hypothesis (H0)**: The paid conversion rate of the Treatment group is equal to that of the Control group.
> H0: p_treatment = p_control

**Alternate Hypothesis (H1)**: The paid conversion rate of the Treatment group is greater than that of the Control group.
> H1: p_treatment > p_control

**Test Type**: One-tailed Chi-Square test of proportions (two binary groups, binary outcome)

**Significance Level**: α = 0.05

**Why this test**: Paid conversion is a binary outcome (0 or 1). Chi-square is appropriate for comparing proportions between two independent groups.

**Why one-tailed**: We only care whether Treatment improves conversion, not whether it is simply different.

### Test Inputs
| Input | Value |
|---|---|
| Control — Not Converted | 671 |
| Control — Converted | 22 |
| Treatment — Not Converted | 665 |
| Treatment — Converted | 50 |
| Total users | 1408 |

### Test Output
| Statistic | Value |
|---|---|
| Chi-Square Statistic | 9.8024 |
| P-Value | 0.001743 |
| Degrees of Freedom | 1 |
| Control Conversion Rate | 3.17% |
| Treatment Conversion Rate | 6.99% |
| Absolute Lift | +3.82 percentage points |
| Relative Lift | +120.5% |

### Decision Rule
- If p-value < 0.05 → Reject H0 (Treatment is significantly better)
- If p-value ≥ 0.05 → Fail to reject H0

### Result
**p-value = 0.0017 < 0.05 → Reject H0**

### Business Interpretation
The Treatment group shows a statistically significant improvement in paid conversion rate. The campaign more than doubles conversion (3.17% → 6.99%). This is a meaningful result. However, conversion alone is insufficient for a launch decision — guardrail metrics must also be evaluated.

---

## Test 2: Engagement Score (Supporting Metric)

**H0**: Mean engagement score is equal across Control and Treatment.
**H1**: Treatment group has a higher mean engagement score.
**Test**: One-tailed Welch T-Test (unequal variance, continuous metric)
**Significance Level**: α = 0.05

| Statistic | Value |
|---|---|
| Control Mean Engagement | 57.03 |
| Treatment Mean Engagement | 62.93 |
| t-statistic | -7.9454 |
| P-Value | ~0.000000 |

**Result**: Reject H0. Treatment significantly improves engagement. ✓

---

## Test 3: Support Ticket Rate (Guardrail Metric)

**H0**: Support ticket rate is equal across Control and Treatment.
**H1**: Support ticket rate differs between groups.
**Test**: Two-tailed Welch T-Test (guardrail — we care about increase OR decrease)
**Significance Level**: α = 0.05

| Statistic | Value |
|---|---|
| Control Support Ticket Rate | 21.93% |
| Treatment Support Ticket Rate | 37.20% |
| t-statistic | -4.2287 |
| P-Value | 0.000025 |

**Result**: Reject H0. Treatment significantly increases support tickets. ⚠ HIGH RISK.

**Business Interpretation**: A 15.27 percentage point increase in support tickets means the new onboarding experience is causing significant user confusion or friction. This is a major guardrail concern that must be resolved before full launch.

---

## Test 4: Avg Revenue Per Converted User (Guardrail Metric)

**H0**: Mean revenue per converted user is equal across Control and Treatment.
**H1**: Mean revenue per converted user differs between groups.
**Test**: Two-tailed Welch T-Test
**Significance Level**: α = 0.05

| Statistic | Value |
|---|---|
| Control Avg Revenue (converted) | $1,630.10 |
| Treatment Avg Revenue (converted) | $770.41 |
| t-statistic | 1.8397 |
| P-Value | 0.0791 |

**Result**: Fail to reject H0 (p = 0.079 > 0.05). Not statistically significant at α = 0.05.

**Business Interpretation**: Although Treatment converts more users, those users generate substantially less revenue on average (-$859). This difference is not statistically significant at 5% but is practically significant given the magnitude. This warrants close monitoring post-launch.

---

## Summary of All Tests

| Test | Metric | P-Value | Result |
|---|---|---|---|
| Chi-Square | Paid Conversion Rate | 0.0017 | Significant |
| Welch T-Test | Engagement Score | ~0.000 | Significant  |
| Welch T-Test | Support Ticket Rate | 0.000025 | Significant RISK |
| Welch T-Test | Revenue Per Converted User | 0.0791 | Not Significant (watch) |
