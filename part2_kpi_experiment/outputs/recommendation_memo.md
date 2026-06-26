# Recommendation Memo
**To**: Leadership Team  
**From**: Business Analyst  
**Subject**: Campaign Experiment — Launch Decision Recommendation  
**Date**: June 2025

---

## Executive Summary

The new onboarding and activation campaign (Treatment) shows a statistically significant improvement in paid conversion rate — the North Star metric — more than doubling it from 3.17% to 6.99% (p = 0.0017). Engagement scores also improved significantly. However, two critical guardrail metrics have failed: support ticket rates increased by +15.27 percentage points, and average revenue per converted user dropped by $859.69. Based on the full evidence, the recommendation is a **conditional partial launch to selected segments** with mandatory guardrail resolution before full rollout.

---

## Business Problem Statement

**Decision to be made**: Whether to replace the existing onboarding experience with the new campaign experience for all users.

**Who it impacts**: All new users signing up for the product. Poor decisions could affect revenue, support costs, and long-term retention.

**Metric that should improve**: Paid conversion rate (North Star). The business needs more users to convert from free/trial to paid plans.

**Risks to monitor**: Support load increase, revenue per user decline, refund rate increase, and segment-level performance variation.

**Evidence required**: Statistically significant improvement in the North Star metric AND no significant deterioration in guardrail metrics before a full launch recommendation can be made.

---

## North Star Metric

**Selected**: Paid Conversion Rate

**Why this is the North Star**:
- Directly measures the campaign's core objective: converting users to paid plans
- Connects directly to revenue and business growth
- All funnel metrics (landing page visits, trial starts, onboarding completion) are sub-drivers that only matter if they ultimately lead to conversion
- Other metrics like engagement score are supportive but not the final business outcome

**Why other metrics are supporting metrics**:
- Landing page visit rate only measures reach, not value
- Trial start rate is a funnel step, not the end goal
- Engagement score is a leading indicator of conversion, not conversion itself
- Revenue per user could be optimised blindly by acquiring high-paying users without improving the campaign

**Risk of blind optimisation**: Optimising only for conversion rate could drive low-quality conversions — users who pay once and immediately request refunds. Revenue per converted user must be monitored in parallel.

---

## KPI Tree Summary

The KPI tree breaks Paid Conversion Rate into three primary drivers:

1. **Funnel Progression**: Landing page visit rate → Trial start rate → Onboarding completion rate → Paid conversion
2. **User Engagement**: Engagement score, days to convert, session depth
3. **Revenue Quality**: Avg revenue per user, avg revenue per converted user, plan type mix

Guardrail metrics (support ticket rate, refund rate, segment-level decline) run alongside the tree to catch unintended negative consequences.

All drivers are further segmented by Region, Device Type, Traffic Source, and Plan Type.

---

## Experiment Result Summary

| Metric | Control | Treatment | Change | Significant? |
|---|---|---|---|---|
| Users | 693 | 715 | — | — |
| Landing Page Visit Rate | 63.64% | 72.59% | +8.95pp | — |
| Trial Start Rate | 25.11% | 29.09% | +3.98pp | — |
| Onboarding Completion Rate | 15.58% | 21.26% | +5.68pp | — |
| **Paid Conversion Rate ★** | **3.17%** | **6.99%** | **+3.82pp (+120%)** | **Yes ✓** |
| Avg Revenue Per User | $51.75 | $53.88 | +$2.13 | No |
| Avg Revenue Per Converted User | $1,630.10 | $770.41 | -$859.69 | No (p=0.079) |
| Refund Rate | 0.00% | 0.42% | +0.42pp | — |
| Support Ticket Rate | 21.93% | 37.20% | +15.27pp | Yes ⚠ |
| Avg Engagement Score | 57.03 | 62.93 | +5.90 | Yes ✓ |
| Avg Days to Convert | 8.86 | 6.40 | -2.46 days | — |

---

## Hypothesis Test Interpretation

**Primary test**: Chi-Square test on paid conversion rate.
- χ² = 9.80, p-value = 0.0017
- At α = 0.05, we **reject H0** — the conversion rate difference is statistically significant
- The campaign more than doubles conversion rate

**Guardrail test**: Welch T-Test on support ticket rate.
- t = -4.23, p-value = 0.000025
- The support ticket rate increase is also statistically significant — this is a real problem, not random variation

**Engagement test**: Welch T-Test on engagement score.
- t = -7.95, p-value ≈ 0
- Engagement improvement is highly significant and positive

---

## Guardrail Analysis

| Guardrail | Finding | Risk Level |
|---|---|---|
| Support Ticket Rate | Increased from 21.93% to 37.20% (+15.27pp) — statistically significant | HIGH ⚠ |
| Avg Revenue Per Converted User | Dropped from $1,630 to $770 (-$859) — not significant but practically large | MEDIUM ⚠ |
| Refund Rate | Increased from 0.00% to 0.42% — small but non-zero | LOW |
| Days to Convert | Decreased from 8.86 to 6.40 — positive signal | NONE |
| Engagement Score | Increased significantly — positive signal | NONE |

The support ticket rate increase is a critical concern. If launched at full scale, support costs would increase significantly. The revenue quality drop suggests the campaign may be attracting lower-intent or lower-value users.

---

## Segment-Level Insights

- **By Region**: Treatment outperforms Control in conversion across all regions. North region shows the strongest absolute lift.
- **By Device**: Mobile users (largest group) show conversion lift in Treatment. Desktop users also improve.
- **By Traffic Source**: Organic and Paid Search traffic sources show the largest conversion improvements.
- **By Plan Type**: Free plan users show the largest conversion lift, suggesting the campaign is effective at moving free users toward paid plans.

---

## Final Recommendation

### ⚠ Do NOT launch to all users immediately.

### Recommended Action: **Launch to selected segments with guardrail monitoring**

**Rationale**:
1. Conversion rate improvement is statistically significant and practically meaningful (+120%)
2. However, support ticket rate increase (+15.27pp, statistically significant) is a critical operational risk
3. Revenue per converted user has dropped substantially (-$859) — the quality of conversions needs investigation
4. A phased rollout to lower-risk segments (e.g., Organic traffic, Free plan users) while resolving support issues is the responsible path

**Suggested next steps**:
1. Investigate why support tickets increased — is it UI confusion, unclear pricing, or technical bugs in the new flow?
2. Analyse plan types of converted users in Treatment vs Control — are Treatment converts selecting lower-value plans?
3. Run a follow-up experiment with a revised campaign that addresses the support ticket driver
4. If support ticket cause is identified and resolved, proceed with full launch

---

## Risks and Limitations

1. Experiment ran for a defined period — seasonal effects or external campaigns may have influenced results
2. 8 duplicate user IDs and 24 missing traffic source values may slightly affect segment analysis
3. Revenue is measured at 30 days — longer-term retention and LTV are unknown
4. days_to_convert is missing for 1,336 of 1,408 users (only available for converted users)
5. Causation cannot be confirmed from this experiment alone — correlation between campaign and conversion does not rule out confounders
