# Post-Mortem Template

## Incident Overview

**Incident ID**: [INC-YYYY-MM-DD-XXX]
**Incident Date**: [YYYY-MM-DD]
**Severity**: P0 (Critical) / P1 (High) / P2 (Medium) / P3 (Low)
**Duration**: [X hours Y minutes]
**Incident Commander**: [Name]
**Participants**: [Names]

---

## Summary

[2-3 sentence summary of what happened and impact]

**Impact**:
- Users Affected: [X users / X% of users]
- Revenue Impact: [$X]
- Service(s) Affected: [List]
- Geographic Scope: [Global / Regional / Single region]

---

## Timeline

All times in [Timezone]

| Time     | Event                                   | Action Taken                        | Actor        |
| -------- | --------------------------------------- | ----------------------------------- | ------------ |
| 14:00    | [Event description]                     | [What was done]                     | [Who did it] |
| 14:15    | First alert fired                       | On-call engineer paged              | System       |
| 14:20    | Incident declared                       | Incident channel created            | Engineer A   |
| 14:25    | Investigation began                     | Checked logs and metrics            | Engineer A   |
| 14:40    | Root cause identified                   | Database connection pool exhausted  | Engineer B   |
| 15:00    | Mitigation deployed                     | Increased connection pool size      | Engineer B   |
| 15:15    | Service restored                        | Verified healthy metrics            | Engineer A   |
| 15:30    | Incident resolved, monitoring continues | Continued observing for issues      | Team         |

**Total Duration**: 1 hour 30 minutes
**Time to Detect**: 15 minutes
**Time to Mitigate**: 1 hour
**Time to Recover**: 15 minutes

---

## Root Cause

**What Happened**:
[Detailed technical explanation of the root cause]

**Why It Happened**:
[Underlying reasons - technical, process, or human factors]

**Contributing Factors**:
1. [Factor 1]
2. [Factor 2]
3. [Factor 3]

---

## Resolution

**Immediate Mitigation** (What stopped the bleeding):
- [Action 1]
- [Action 2]

**Permanent Fix** (Long-term solution):
- [Action 1]
- [Action 2]

**Verification**:
- [How we confirmed it was fixed]
- [Monitoring put in place]

---

## Impact Analysis

### Customer Impact
**External**:
- [Description of customer-facing impact]
- [Specific features or functionality affected]
- [Customer complaints received: X]

**Internal**:
- [Internal teams affected]
- [Business operations impacted]

### Business Impact
- Revenue Lost: [$X estimated]
- SLA Credits: [$Y]
- Customer Trust: [Assessment]

---

## What Went Well

**Positive aspects of the response**:

1. ✅ **[Positive 1]**
   - [Details]

2. ✅ **[Positive 2]**
   - [Details]

3. ✅ **[Positive 3]**
   - [Details]

---

## What Went Wrong

**Areas for improvement**:

1. ⚠️ **[Issue 1]**
   - [Details]
   - [Why this was problematic]

2. ⚠️ **[Issue 2]**
   - [Details]
   - [Why this was problematic]

3. ⚠️ **[Issue 3]**
   - [Details]
   - [Why this was problematic]

---

## Action Items

| ID  | Action                                        | Owner        | Priority | Due Date   | Status      |
| --- | --------------------------------------------- | ------------ | -------- | ---------- | ----------- |
| 1   | [Specific action to prevent recurrence]      | [Name]       | High     | YYYY-MM-DD | Not Started |
| 2   | [Improve monitoring/alerting]                 | [Name]       | High     | YYYY-MM-DD | Not Started |
| 3   | [Update documentation/runbooks]               | [Name]       | Medium   | YYYY-MM-DD | Not Started |
| 4   | [Process improvement]                         | [Name]       | Medium   | YYYY-MM-DD | Not Started |
| 5   | [Conduct training/knowledge sharing]          | [Name]       | Low      | YYYY-MM-DD | Not Started |

### Action Categories

**Prevent Recurrence** (Stop this specific issue):
- [Action items that prevent this exact problem]

**Improve Detection** (Find issues faster):
- [Better monitoring, alerting, observability]

**Improve Response** (Respond faster and better):
- [Runbooks, training, tools, process]

**Systemic Improvements** (Broader fixes):
- [Architecture, culture, organizational changes]

---

## Lessons Learned

**Technical Lessons**:
1. [Lesson 1]
2. [Lesson 2]
3. [Lesson 3]

**Process Lessons**:
1. [Lesson 1]
2. [Lesson 2]

**Organizational Lessons**:
1. [Lesson 1]
2. [Lesson 2]

---

## Follow-Up

**One Week Follow-Up**:
- Review action item progress
- Check if issue has recurred
- Verify monitoring improvements

**One Month Follow-Up**:
- Validate all action items completed
- Assess effectiveness of changes
- Update runbooks based on learnings

---

## Supporting Information

**Relevant Links**:
- Incident Slack Channel: [Link]
- Status Page Updates: [Link]
- Monitoring Dashboard: [Link]
- Code Changes: [PR links]
- Related Incidents: [Previous incident links]

**Attached Documents**:
- [Screenshots of errors]
- [Graphs/charts of metrics]
- [Customer communications]

---

## Communication

**Internal Communication**:
- All-hands update: [Date/Time]
- Engineering team sync: [Date/Time]
- Executive briefing: [Date/Time]

**External Communication**:
- Status page updates: [Count]
- Customer emails sent: [Count]
- Public post-mortem: [Yes/No - If yes, link]

---

## Reminder: Blameless Culture

This post-mortem focuses on **systems and processes**, not individuals. Everyone involved acted with the best information available at the time. Our goal is to learn and improve, not to assign blame.

**Key Principles**:
- No individual is at fault
- We learn from failures
- We improve our systems
- We share knowledge openly
- We celebrate our incident response teamwork

---

**Document Owner**: [Name]
**Last Updated**: [YYYY-MM-DD]
**Status**: Draft / Under Review / Final
**Distribution**: Engineering Team / All Company / Public
