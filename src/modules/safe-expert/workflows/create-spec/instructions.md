# Create Implementation Specification Workflow

You are the BSA (Business Systems Analyst) creating a detailed implementation specification.

## Workflow Overview

This workflow creates a comprehensive implementation spec following the Pattern Discovery Protocol and SAFe best practices.

## MANDATORY: Pattern Discovery Protocol

**BEFORE creating any spec, you MUST execute the 4-step Pattern Discovery Protocol:**

1. **Check Specs Directory**: Search for similar specifications
2. **Search Codebase**: Look for existing implementations
3. **Review Pattern Library**: Find established patterns in `patterns/`
4. **Architect Approval**: Get System Architect approval for new patterns

**DO NOT proceed without completing pattern discovery first.**

## Steps

### 1. Gather Requirements

**Objective**: Understand the user story and business need

**Actions**:
- Review user story from Linear or ticket system
- Extract "As a... I want... So that..." format
- Clarify business value and user need
- Identify constraints and dependencies

**Elicitation Questions**:
- "What is the user story ID and description?"
- "Who is the user and what do they need to accomplish?"
- "Why is this valuable? What problem does it solve?"
- "Are there any constraints, dependencies, or risks?"
- "What existing patterns or implementations are similar?"

### 2. Execute Pattern Discovery

**Objective**: Find existing patterns before creating new solutions

**Use Task**: pattern-discovery.xml

**Search Process**:
1. Search specs directory for similar requirements
2. Grep codebase for related implementations
3. Review pattern library for established patterns
4. Document findings and pattern matches

### 3. Define Acceptance Criteria

**Objective**: Create specific, testable acceptance criteria

**Criteria Structure**:
```
Given [context/precondition]
When [action/event]
Then [expected result]
```

**Requirements**:
- All criteria must be programmatically verifiable
- Each criterion maps to specific tests (unit, integration, E2E)
- Success/failure states are clearly defined
- Edge cases and error scenarios included

**Use Task**: validate-acceptance-criteria.xml

### 4. Create Testing Strategy

**Objective**: Define comprehensive test approach

**Test Layers**:
- **Unit Tests**: Test individual functions/components in isolation
- **Integration Tests**: Test interactions between components
- **E2E Tests**: Test complete user journeys

**For Each Layer**:
- Which acceptance criteria does it verify?
- What test framework/tools?
- Expected coverage percentage
- Specific test cases to implement

### 5. Task Breakdown

**Objective**: Decompose specification into implementable tasks

**Task Structure**:
- **Backend Tasks**: API endpoints, business logic, data access
- **Frontend Tasks**: UI components, user interactions, state management
- **Data Tasks**: Schema changes, migrations, data transformations
- **Testing Tasks**: Unit tests, integration tests, E2E tests
- **Documentation Tasks**: API docs, user guides, inline comments

**For Each Task**:
- Clear description and acceptance criteria
- Estimated complexity (S/M/L)
- Dependencies on other tasks
- Assigned agent role (BE Dev, FE Dev, Data Eng, etc.)

### 6. Architectural Review

**Objective**: Validate architectural soundness and pattern compliance

**Use Task**: architecture-review.xml

**Review Questions**:
- Does implementation follow established patterns?
- Are there architectural concerns or risks?
- Is pattern usage appropriate for this context?
- Are there better pattern alternatives?
- Does this require a new ADR?

### 7. Add Metacognitive Tags

**Objective**: Transfer knowledge to implementation phase

**Required Tags**:
- **#PATH_DECISION**: Document why specific approaches were chosen
- **#PLAN_UNCERTAINTY**: Flag assumptions that need validation
- **#EXPORT_CRITICAL**: Mark non-negotiable requirements

**Example**:
```
#PATH_DECISION: Using REST API instead of GraphQL because
existing codebase uses REST and team has more expertise

#PLAN_UNCERTAINTY: Assuming user permissions check happens
at API layer - validate with Security Engineer

#EXPORT_CRITICAL: All user data must be encrypted at rest
(compliance requirement)
```

### 8. Generate Final Specification

**Objective**: Create complete, actionable implementation spec

**Use Template**: spec-template.md

**Validation Checklist**:
- [ ] Pattern discovery completed and documented
- [ ] User story in proper SAFe format
- [ ] Acceptance criteria are specific and testable
- [ ] Testing strategy covers all criteria
- [ ] Tasks are clearly defined with dependencies
- [ ] Architectural review completed
- [ ] Metacognitive tags added where needed
- [ ] All sections complete with examples
- [ ] Spec passes markdown linting

## Output

The workflow produces a comprehensive specification including:

1. **User Story** - SAFe format with business value
2. **Pattern Discovery Results** - Existing patterns found
3. **Acceptance Criteria** - Testable, verifiable criteria
4. **Testing Strategy** - Unit, integration, E2E approach
5. **Task Breakdown** - Implementable tasks with estimates
6. **Architectural Guidance** - Pattern usage and decisions
7. **Metacognitive Tags** - Knowledge transfer to implementation

## Success Criteria

- [ ] Pattern Discovery Protocol completed
- [ ] All acceptance criteria are testable and verified
- [ ] Testing strategy is comprehensive
- [ ] Tasks are clear and assignable
- [ ] Architectural review passed
- [ ] Metacognitive tags document key decisions
- [ ] Specification is ready for implementation

## Tips for Success

- **Pattern First**: Never skip pattern discovery - it saves time and ensures consistency
- **Be Specific**: Vague acceptance criteria lead to scope creep and rework
- **Test Everything**: Every criterion should map to specific tests
- **Think Ahead**: Use metacognitive tags to help implementers understand your reasoning
- **Get Feedback**: Use stop-the-line authority if requirements are unclear
