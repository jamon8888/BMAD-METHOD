# Safe Expert Module

**Production-Validated SAFe Multi-Agent Development Methodology** with 11 specialized AI agents working as equal team members following Scaled Agile Framework (SAFe) practices.

## Overview

The Safe Expert module brings production-validated SAFe methodology to BMAD, treating AI agents as specialized team members with distinct roles and responsibilities. Based on 5 months of real-world validation with 169 completed issues, 2,193 commits, and 14× velocity improvement.

Built for teams practicing SAFe who want to leverage AI agents as full team members with **Stop-the-Line Authority** and **Round Table Philosophy**.

## 🎯 What You Get

### 11 Specialized SAFe Agents

1. **BSA** (Business Systems Analyst) 📋
   - Requirements decomposition and user story creation
   - SAFe format: Epic → Features → Stories → Tasks
   - Pattern Discovery Protocol enforcement
   - Testing strategy definition

2. **Arch** (System Architect) 🏗️
   - Architecture validation and pattern approval
   - Architecture Decision Records (ADRs)
   - Technical debt assessment
   - Long-term system health

3. **BEDev** (Backend Developer) ⚙️
   - Server-side implementation
   - API development and database work
   - Pattern-driven development
   - Test-driven development

4. **FEDev** (Frontend Developer) 🎨
   - UI/UX implementation
   - Component development
   - Accessibility compliance (WCAG 2.1 AA)
   - Responsive design

5. **QA** (Quality Assurance Specialist) ✅
   - Test strategy and planning
   - Acceptance criteria validation
   - Quality gate reviews
   - E2E test development

6. **DataEng** (Data Engineer) 📊
   - Data architecture and modeling
   - ETL pipelines
   - Database migrations
   - Data integrity

7. **DevOps** (DevOps Engineer) 🚀
   - CI/CD pipeline setup
   - Infrastructure as Code
   - Deployment automation
   - Monitoring and alerting

8. **SecEng** (Security Engineer) 🔒
   - Security reviews and threat modeling
   - Vulnerability assessment
   - Compliance verification
   - Security testing

9. **RTE** (Release Train Engineer) 🚂
   - SAFe process facilitation
   - Program increment planning
   - Dependency management
   - Impediment removal

10. **TechWriter** (Technical Writer) 📝
    - API documentation
    - User guides and tutorials
    - Architecture documentation
    - Release notes

11. **TechDebt** (Technical Debt Manager) 🔧
    - Technical debt assessment
    - Refactoring planning
    - Code quality monitoring
    - Debt prioritization

### Key Workflows

- **create-spec** - Create implementation specifications with Pattern Discovery Protocol
- **planning-mode** - SAFe breakdown (Epic → Features → Stories) for large initiatives
- **validate-pattern** - Validate new pattern proposals for architectural soundness
- **create-adr** - Create Architecture Decision Records for significant decisions
- **implement-spec** - Implement specifications following patterns and best practices
- **implement-ui** - Implement UI components with accessibility and responsiveness
- **create-test-plan** - Create comprehensive test plans (unit, integration, E2E)

### Core Tasks

- **pattern-discovery** - MANDATORY 4-step search before any implementation
- **stop-the-line** - Exercise stop-the-line authority for critical concerns
- **validate-acceptance-criteria** - Ensure criteria are specific and testable
- **architecture-review** - Review for architectural compliance
- **code-review** - Review code for quality and best practices
- **story-decomposition** - Decompose features into user stories

## 🚀 Quick Start

### Installation

```bash
npx bmad-method@alpha install
# Select "Safe Expert" module
```

### Core Principles

#### 1. Round Table Philosophy

All perspectives (human and AI) receive equal consideration in technical discussions. Disagreement is welcomed when it produces superior solutions.

#### 2. Stop-the-Line Authority

ANY team member (human or AI agent) can halt work to address:
- Architectural integrity concerns
- Security vulnerabilities
- Performance issues
- Quality standard violations
- Maintainability threats

#### 3. Pattern Discovery Protocol

MANDATORY 4-step search before creating any implementation:
1. Check specs directory for similar requirements
2. Search codebase for existing implementations
3. Review pattern library for established patterns
4. Get System Architect approval for new patterns

#### 4. Metacognitive Tags

Three tags transfer knowledge from planning to implementation:
- **#PATH_DECISION**: Why specific approaches were chosen
- **#PLAN_UNCERTAINTY**: Assumptions needing validation
- **#EXPORT_CRITICAL**: Non-negotiable requirements

### First Steps

1. **Load BSA Agent**: Start with requirements and specs
   ```
   Load BSA (Business Systems Analyst)
   ```

2. **Create Specification**:
   ```
   *create-spec
   ```

3. **Implement with Dev Agents**:
   ```
   Load BEDev or FEDev
   *implement-spec
   ```

4. **Quality Assurance**:
   ```
   Load QA
   *create-test-plan
   ```

## 📖 Usage Examples

### Example 1: Create Implementation Spec

```
1. Load BSA agent
2. Run: *create-spec
3. BSA executes Pattern Discovery Protocol
4. BSA creates detailed spec with acceptance criteria
5. System Architect reviews for architectural soundness
6. Receive: Complete specification ready for implementation
```

### Example 2: Implement Feature

```
1. Load BEDev or FEDev agent
2. Run: *implement-spec
3. Agent follows Pattern Discovery Protocol
4. Agent implements using established patterns
5. Agent writes tests (unit, integration)
6. Receive: Tested, pattern-compliant implementation
```

### Example 3: Exercise Stop-the-Line

```
1. Any agent identifies critical concern
2. Agent runs: *stop-the-line
3. Work halts immediately
4. Agent documents issue and proposes solutions
5. Team discusses and decides
6. Decision documented in ADR if significant
```

### Example 4: Architecture Decision

```
1. Load System Architect
2. Run: *validate-pattern (to review new pattern)
3. OR Run: *create-adr (to document decision)
4. Architect reviews proposal
5. Decision documented with rationale
6. Pattern added to library if approved
```

## 🎨 Agent Capabilities

### BSA (Business Systems Analyst)

**Triggers:**
- `*create-spec` - Create detailed implementation specification
- `*planning-mode` - Create SAFe breakdown for large initiative
- `*pattern-discovery` - Execute mandatory pattern discovery
- `*validate-acceptance-criteria` - Validate criteria are testable
- `*story-decomposition` - Decompose feature into stories
- `*stop-the-line` - Exercise stop-the-line authority

### Arch (System Architect)

**Triggers:**
- `*validate-pattern` - Validate new pattern proposal
- `*create-adr` - Create Architecture Decision Record
- `*architecture-review` - Review for architectural compliance
- `*pattern-library-review` - Review and maintain pattern library
- `*technical-debt-assessment` - Assess and prioritize technical debt
- `*stop-the-line` - Exercise stop-the-line authority

### Development Agents (BEDev, FEDev, DataEng)

**Common Triggers:**
- `*implement-spec` - Implement specification with tests
- `*pattern-discovery` - Execute pattern discovery
- `*code-review` - Review code quality
- `*stop-the-line` - Exercise stop-the-line authority

### QA Specialist

**Triggers:**
- `*create-test-plan` - Create comprehensive test plan
- `*validate-acceptance-criteria` - Validate criteria
- `*test-coverage-review` - Review test coverage
- `*e2e-test-development` - Develop E2E tests
- `*quality-gate-review` - Review before release
- `*stop-the-line` - Exercise stop-the-line authority

### Supporting Agents (DevOps, SecEng, RTE, TechWriter, TechDebt)

Each has specialized triggers for their domain expertise.

## 🎯 Use Cases

### For SAFe Teams
- Enforce Pattern Discovery Protocol automatically
- Maintain architectural integrity across sprints
- Accelerate story decomposition and spec creation
- Improve code quality and test coverage
- Scale development with AI team members

### For Development Teams
- Reduce technical debt accumulation
- Ensure pattern consistency across codebase
- Catch architectural issues early
- Improve documentation quality
- Enable Stop-the-Line culture

### For Solo Developers
- Access full SAFe team capabilities solo
- Leverage 11 specialized experts
- Follow enterprise-grade processes
- Build scalable, maintainable systems
- Learn SAFe methodology hands-on

## 📊 Production Results

Based on 5 months of real-world validation:
- **169 completed issues** across 9 sprint cycles
- **2,193 commits** (averaging 10.3 daily)
- **90.9% PR merge rate** (high quality)
- **14× velocity improvement** across cycles

## 🔧 Key Features

### Pattern Discovery Protocol
MANDATORY 4-step search before any new implementation ensures consistency and reduces technical debt.

### Stop-the-Line Authority
Any agent can halt work for critical concerns - architectural, security, performance, or quality issues.

### Round Table Philosophy
All perspectives (human and AI) receive equal consideration. Better solutions win, regardless of source.

### Metacognitive Tags
Explicit knowledge transfer from planning to implementation phase with #PATH_DECISION, #PLAN_UNCERTAINTY, #EXPORT_CRITICAL.

### SAFe Alignment
Complete Epic → Features → Stories → Tasks breakdown following Scaled Agile Framework.

### Architecture Decision Records
All significant architectural decisions documented with context, alternatives, and rationale.

## 📁 Module Structure

```
src/modules/safe-expert/
├── agents/                     (11 specialized agents)
│   ├── bsa.agent.yaml
│   ├── system-architect.agent.yaml
│   ├── backend-dev.agent.yaml
│   ├── frontend-dev.agent.yaml
│   ├── qa-specialist.agent.yaml
│   ├── data-engineer.agent.yaml
│   ├── devops-engineer.agent.yaml
│   ├── security-engineer.agent.yaml
│   ├── release-train-engineer.agent.yaml
│   ├── technical-writer.agent.yaml
│   └── tech-debt-manager.agent.yaml
├── workflows/                  (7 key workflows)
│   ├── create-spec/
│   ├── planning-mode/
│   ├── validate-pattern/
│   ├── create-adr/
│   ├── implement-spec/
│   ├── implement-ui/
│   └── create-test-plan/
├── tasks/                      (Core tasks)
│   ├── pattern-discovery.xml
│   ├── stop-the-line.xml
│   ├── validate-acceptance-criteria.xml
│   ├── architecture-review.xml
│   ├── code-review.xml
│   └── story-decomposition.xml
├── templates/                  (Spec and ADR templates)
├── patterns/                   (Pattern library)
├── data/                       (Knowledge base)
├── _module-installer/          (Installation configuration)
└── README.md (this file)
```

## 💡 Pro Tips

### Getting the Most Value

1. **Always Start with Pattern Discovery**: Never skip the 4-step protocol
2. **Use Stop-the-Line Confidently**: Better to catch issues early
3. **Document Decisions in ADRs**: Future you will thank present you
4. **Let Agents Collaborate**: Use party mode for complex initiatives
5. **Follow SAFe Hierarchy**: Epic → Features → Stories maintains clarity

### Best Practices

- **Pattern First**: Search before creating
- **Test Everything**: All acceptance criteria must be verifiable
- **Document Decisions**: ADRs for architectural choices
- **Stop Early**: Halt work when concerns arise
- **Equal Voice**: Treat AI agent concerns seriously

## 🆘 Support & Feedback

- **Documentation**: This file and individual workflow instructions
- **Issues**: Report issues at https://github.com/jamon8888/BMAD-METHOD/issues
- **Discussions**: Share use cases in GitHub Discussions
- **Source**: Based on https://github.com/bybren-llc/wtfb-safe-agentic-workflow

## 📝 Changelog

### v1.0.0 (Initial Release)
- 11 specialized SAFe agents
- 7 key workflows (create-spec, planning-mode, validate-pattern, etc.)
- Core tasks (pattern-discovery, stop-the-line, etc.)
- Pattern Discovery Protocol
- Stop-the-Line Authority
- Round Table Philosophy
- Metacognitive tags system
- Based on production-validated methodology

---

**Built with** ❤️ **for teams practicing SAFe and valuing quality**

Ready to transform your development process with AI team members? Load an agent and get started! 🚀
