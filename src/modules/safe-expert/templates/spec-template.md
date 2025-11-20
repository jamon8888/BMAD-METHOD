# Implementation Specification: {{story-title}}

**Story ID**: {{story-id}}
**Created by**: {{author}} ({{agent-role}})
**Date**: {{date}}
**Status**: {{status}}

---

## User Story

**As a** {{user-role}}
**I want** {{functionality}}
**So that** {{business-value}}

---

## Pattern Discovery Results

### Patterns Found
{{#each patterns-found}}
- **{{pattern-name}}** ({{pattern-location}})
  - Applicability: {{applicability}}
  - Modifications needed: {{modifications}}
{{/each}}

### Pattern Decision
**Selected Pattern**: {{selected-pattern}}
**Rationale**: {{pattern-rationale}}

---

## Acceptance Criteria

{{#each acceptance-criteria}}
### AC{{number}}: {{title}}

```
Given {{given}}
When {{when}}
Then {{then}}
```

**Test Type**: {{test-type}}
**Priority**: {{priority}}

---
{{/each}}

## Testing Strategy

### Unit Tests
{{unit-test-strategy}}

### Integration Tests
{{integration-test-strategy}}

### E2E Tests
{{e2e-test-strategy}}

**Expected Coverage**: {{test-coverage-target}}%

---

## Task Breakdown

{{#each tasks}}
### Task {{number}}: {{task-title}}

**Description**: {{description}}
**Assigned to**: {{assigned-agent}}
**Complexity**: {{complexity}}
**Dependencies**: {{dependencies}}

**Acceptance Criteria**:
{{acceptance-criteria}}

---
{{/each}}

## Architectural Guidance

**Architect Review**: {{architect-review-status}}
**Patterns to Use**: {{patterns-to-use}}
**Architectural Concerns**: {{architectural-concerns}}

---

## Metacognitive Tags

### #PATH_DECISION
{{path-decisions}}

### #PLAN_UNCERTAINTY
{{plan-uncertainties}}

### #EXPORT_CRITICAL
{{critical-requirements}}

---

**Next Steps**: {{next-steps}}
**Estimated Effort**: {{estimated-effort}}
