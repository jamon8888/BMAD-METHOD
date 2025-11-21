# Risk Register Template

## Risk Register Overview

**Organization**: [Company Name]
**Period**: [YYYY-QX]
**Owner**: [CTO/Risk Manager]
**Last Updated**: [YYYY-MM-DD]

---

## Risk Summary

| Category     | Critical (15-25) | High (10-14) | Medium (6-9) | Low (1-5) | Total   |
| ------------ | ---------------- | ------------ | ------------ | --------- | ------- |
| Security     | [X]              | [X]          | [X]          | [X]       | [X]     |
| Availability | [X]              | [X]          | [X]          | [X]       | [X]     |
| Compliance   | [X]              | [X]          | [X]          | [X]       | [X]     |
| Operational  | [X]              | [X]          | [X]          | [X]       | [X]     |
| Business     | [X]              | [X]          | [X]          | [X]       | [X]     |
| **Total**    | **[X]**          | **[X]**      | **[X]**      | **[X]**   | **[X]** |

---

## Critical Risks (Score 15-25)

### Risk R-001: [Risk Name]

**Category**: Security / Availability / Compliance / Operational / Business
**Identified By**: [Name]
**Date Identified**: [YYYY-MM-DD]
**Status**: 🔴 Active / 🟡 Mitigating / 🟢 Mitigated

**Description**:
[Detailed description of the risk]

**Probability**: 5 (High) / 4 (Medium-High) / 3 (Medium)
**Impact**: 5 (Critical) / 4 (High) / 3 (Medium)
**Risk Score**: [Probability × Impact = XX]

**Existing Controls**:

- [Control 1]
- [Control 2]

**Control Effectiveness**: Effective / Partially Effective / Ineffective / None

**Mitigation Strategy**:

1. [Action 1] - [Owner] - [Deadline]
2. [Action 2] - [Owner] - [Deadline]

**Estimated Cost**: [$X] or [Y engineer-weeks]

**Risk After Mitigation**:

- Probability: [New score]
- Impact: [New score]
- Residual Risk Score: [New total]

**Progress**:

- [X%] Complete
- Last Update: [Date]
- Next Review: [Date]

---

## High Risks (Score 10-14)

[Same format as Critical Risks section]

---

## Medium Risks (Score 6-9)

[Same format - can be summarized more briefly]

---

## Low Risks (Score 1-5)

[Brief list format acceptable]

---

## Risk Matrix Visualization

```
IMPACT →
   5  |  R-003 |       |  R-001 | R-002 |
   4  |  R-007 |  R-004|  R-005 |       |
   3  |  R-009 |  R-008|  R-006 |       |
   2  |  R-012 |  R-010|  R-011 |       |
   1  |  R-015 |  R-014|  R-013 |       |
   ─────────────────────────────────────
   1      2       3       4       5
            ← PROBABILITY
```

---

## Closed/Mitigated Risks This Period

| Risk ID | Risk Name | Original Score | Final Score | Date Closed | Impact         |
| ------- | --------- | -------------- | ----------- | ----------- | -------------- |
| R-XXX   | [Name]    | 20             | 3           | YYYY-MM-DD  | [Brief impact] |

---

## New Risks Identified This Period

| Risk ID | Risk Name | Score | Category   | Owner  | Status   |
| ------- | --------- | ----- | ---------- | ------ | -------- |
| R-XXX   | [Name]    | [XX]  | [Category] | [Name] | [Status] |

---

## Trend Analysis

**Quarter-over-Quarter**:

- Total Risks: [Previous QTR] → [Current QTR] (↑/↓/→)
- Critical Risks: [Previous QTR] → [Current QTR] (↑/↓/→)
- Mitigated Risks: [Count]
- New Risks: [Count]

**Overall Risk Posture**: Improving / Stable / Worsening

**Key Trends**:

1. [Trend observation]
2. [Trend observation]

---

## Action Items Due This Month

| Risk ID | Action            | Owner  | Due Date   | Status   |
| ------- | ----------------- | ------ | ---------- | -------- |
| R-XXX   | [Specific action] | [Name] | YYYY-MM-DD | [Status] |

---

## Budget & Resources

**Mitigation Budget Allocated**: [$X]
**Mitigation Budget Spent**: [$Y]
**Remaining**: [$Z]

**FTE Allocation**:

- Security: [X FTE]
- Platform/Infrastructure: [X FTE]
- Compliance: [X FTE]

---

## Executive Summary for Leadership

**Risk Status**: 🟢 Low / 🟡 Medium / 🔴 High

**Top 3 Risks**:

1. [Risk Name] (Score: XX) - [Status]
2. [Risk Name] (Score: XX) - [Status]
3. [Risk Name] (Score: XX) - [Status]

**Key Actions This Quarter**:

- [Action 1]
- [Action 2]
- [Action 3]

**Resource Needs**:

- [Need 1]
- [Need 2]

---

## Appendix: Risk Assessment Criteria

### Probability Scale

- **5 - High**: Very likely, could happen anytime (monthly or more frequent)
- **4 - Medium-High**: Likely to occur (quarterly)
- **3 - Medium**: Possible but not certain (annually)
- **2 - Medium-Low**: Unlikely but possible (every few years)
- **1 - Low**: Very unlikely (once in 5+ years)

### Impact Scale

- **5 - Critical**: Catastrophic business impact (>$1M loss, existential threat)
- **4 - High**: Severe impact ($100K-$1M loss, major disruption)
- **3 - Medium**: Moderate impact ($10K-$100K loss, minor disruption)
- **2 - Medium-Low**: Minor impact (<$10K loss, brief disruption)
- **1 - Low**: Minimal impact (no revenue loss, internal inconvenience)

### Risk Priority

- **Critical (15-25)**: Address immediately (within 30 days)
- **High (10-14)**: Plan mitigation (within 90 days)
- **Medium (6-9)**: Plan mitigation (within 6 months)
- **Low (1-5)**: Monitor, address opportunistically

---

**Next Review Date**: [YYYY-MM-DD]
**Distribution**: Executive Team / Board / Audit Committee
