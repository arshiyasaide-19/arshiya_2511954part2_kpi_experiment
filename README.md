# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## Business Context
A subscription-based digital product company ran an A/B experiment to test a new onboarding and activation campaign. Users were split into Control (existing experience, n=693) and Treatment (new campaign, n=715). Leadership needs a data-driven recommendation on whether to launch the new campaign to all users.

---

## Dataset Description
- **File**: `data/campaign_experiment_data.xlsx`
- **Rows**: 1,408 users
- **Columns**: 16 fields
- **Key fields**: user_id, experiment_group, region, device_type, traffic_source, plan_type, visited_landing_page, started_trial, completed_onboarding, converted_to_paid, revenue_30d, support_tickets_30d, refund_requested, days_to_convert, engagement_score

### Data Quality Issues Found
| Issue | Count | Action |
|---|---|---|
| Missing device_type | 18 | Excluded from device-level segment analysis |
| Missing traffic_source | 24 | Excluded from traffic-level segment analysis |
| Duplicate user_ids | 8 | Flagged; included in analysis (no conflicting data) |
| Missing days_to_convert | 1,336 | Expected — only populated for converted users |

---

## North Star Metric

**Selected**: Paid Conversion Rate

Paid conversion rate is the single metric that most directly measures the campaign's success: converting users from free/trial to paying customers. All other funnel and engagement metrics are sub-drivers that support this outcome. Optimising conversion without monitoring revenue quality and support load is a risk, which is why guardrail metrics are tracked in parallel.

---

## KPI Tree Summary

The KPI tree (`outputs/kpi_tree.png`) breaks Paid Conversion Rate into:

**Primary KPI Drivers**:
1. Funnel Progression → Landing page visits, trial starts, onboarding completions
2. User Engagement → Engagement score, days to convert, session depth
3. Revenue Quality → Avg revenue per user, avg revenue per converted user, plan mix

**Guardrail Metrics**: Support ticket rate, refund rate, segment-level decline

All drivers are segmented by Region, Device Type, Traffic Source, and Plan Type.

---

## Experiment Analysis Approach

1. Computed 11 metrics for both Control and Treatment groups
2. Conducted segment-level analysis across Region, Device Type, Traffic Source, and Plan Type
3. Used Chi-Square test for the binary North Star metric (paid conversion)
4. Used Welch T-Tests for continuous guardrail metrics (engagement, support tickets, revenue)
5. Evaluated all guardrail metrics before making a recommendation

---

## Hypothesis Test Summary

| Test | Metric | P-Value | Result |
|---|---|---|---|
| Chi-Square | Paid Conversion Rate | 0.0017 | Significant ✓ |
| Welch T-Test | Engagement Score | ~0.000 | Significant ✓ |
| Welch T-Test | Support Ticket Rate | 0.000025 | Significant ⚠ RISK |
| Welch T-Test | Revenue Per Converted User | 0.0791 | Not Significant (monitor) |

Significance level: α = 0.05

---

## Guardrail Metrics Considered

| Guardrail | Control | Treatment | Risk |
|---|---|---|---|
| Support Ticket Rate | 21.93% | 37.20% | HIGH ⚠ |
| Avg Revenue Per Converted User | $1,630.10 | $770.41 | MEDIUM ⚠ |
| Refund Rate | 0.00% | 0.42% | LOW |
| Days to Convert | 8.86 days | 6.40 days | Positive |

---

## Final Recommendation

**Do NOT launch to all users immediately.**

Recommended action: **Phased launch to selected segments** (Organic traffic, Free plan users) while investigating the support ticket rate increase and revenue quality drop. Full launch should proceed only after the support issue root cause is identified and resolved.

See `outputs/recommendation_memo.md` for the full decision rationale.

---

## Assumptions and Limitations
1. Experiment duration and randomisation method are assumed to be valid
2. 30-day revenue window may not reflect true LTV
3. Missing device_type and traffic_source values excluded from segment analysis
4. days_to_convert only available for converted users (72 of 1408)
5. Causal inference is limited — confounders cannot be fully ruled out

---

## Screenshots Included

| File | Shows |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment overall metrics table |
| `screenshots/hypothesis_test_output.png` | Chi-square and T-test results |
| `screenshots/kpi_tree_preview.png` | KPI tree diagram |
