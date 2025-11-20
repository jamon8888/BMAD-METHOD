# BMAD-METHOD Codebase Overview for Claude

## Project Summary

**BMAD-METHOD** (Build More, Architect Dreams) is an AI-driven agile development framework that provides structured, battle-tested workflows powered by specialized agents. It's a revolutionary collaboration system built on **BMad Core** (Collaboration Optimized Reflection Engine) that enables human-AI partnership across any domain.

### Key Statistics
- **Version:** 6.0.0-alpha.12
- **License:** MIT
- **Node Version:** >=20.0.0
- **Modules:** 4 core modules (BMM, BMB, CIS, BMGD)
- **Specialized Agents:** 19+
- **Workflows:** 50+
- **Lines of Code:** Thousands across agents, workflows, and tooling

### Repository Info
- **Repository:** https://github.com/bmad-code-org/BMAD-METHOD
- **Author:** Brian (BMad) Madison
- **Community:** [Discord](https://discord.gg/gk8jAdXWmj)
- **YouTube:** [BMadCode](https://www.youtube.com/@BMadCode)

---

## Architecture Overview

### Core Philosophy

BMad follows the **C.O.R.E. Framework**:
- **C**ollaboration: Human-AI partnership with unique strengths
- **O**ptimized: Refined collaborative process for maximum effectiveness
- **R**eflection: Guided thinking for better solutions
- **E**ngine: Framework orchestrating specialized agents and workflows

### High-Level Architecture

```
BMAD-METHOD/
├── src/
│   ├── core/                    # BMad Core Framework (universal foundation)
│   │   ├── agents/              # BMad Master orchestrator
│   │   ├── workflows/           # Party mode, brainstorming
│   │   ├── tasks/               # Atomic work units
│   │   ├── tools/               # Core utilities
│   │   ├── resources/           # Shared assets (Excalidraw, etc.)
│   │   └── _module-installer/   # Installation infrastructure
│   │
│   ├── modules/                 # Modular extensions
│   │   ├── bmm/                 # BMad Method (agile development)
│   │   ├── bmb/                 # BMad Builder (create agents/modules)
│   │   ├── cis/                 # Creative Intelligence Suite
│   │   └── bmgd/                # BMad Game Development
│   │
│   └── utility/                 # Shared utilities
│
├── tools/                       # CLI and bundling tools
│   ├── cli/                     # Installation and agent compiler
│   ├── flattener/               # Project documentation tool
│   └── schema/                  # YAML validation schemas
│
├── test/                        # Test suites
├── docs/                        # Documentation
└── .github/                     # CI/CD workflows
```

---

## Core Components

### 1. BMad Core (`src/core/`)

**Purpose:** Universal framework for human-AI collaboration

**Key Components:**
- **BMad Master Agent:** Meta-orchestrator coordinating all agents
- **Party Mode Workflow:** Multi-agent collaboration for complex decisions
- **Brainstorming Workflow:** Facilitated ideation sessions
- **Module Installer:** Infrastructure for installing/managing modules

**Technologies:** YAML-based agent definitions, Markdown templates

### 2. BMM - BMad Method (`src/modules/bmm/`)

**Purpose:** Complete agile AI-driven software development framework

**Key Features:**
- **12 Specialized Agents:** PM, Architect, Scrum Master, Developer, UX Designer, Test Architect, etc.
- **34 Workflows** across 4 phases:
  - Phase 0: Documentation (brownfield)
  - Phase 1: Analysis (optional)
  - Phase 2: Planning (required)
  - Phase 3: Solutioning (architecture)
  - Phase 4: Implementation (story-driven)
- **Scale-Adaptive Intelligence:** Auto-adjusts from bug fixes (Level 0) to enterprise (Level 4)
- **Story-Centric Lifecycle:** `backlog → drafted → ready → in-progress → review → done`

**Directory Structure:**
```
bmm/
├── agents/          # 12 specialized .agent.yaml files
├── workflows/       # Phase-organized workflows
├── teams/           # Pre-configured agent groups
├── testarch/        # Testing infrastructure
├── sub-modules/     # Platform-specific (Claude Code, etc.)
└── docs/            # Comprehensive user documentation
```

### 3. BMB - BMad Builder (`src/modules/bmb/`)

**Purpose:** Tools for creating custom agents, workflows, and modules

**Key Workflows:**
- `create-agent`: Build new BMad agents
- `create-workflow`: Design structured workflows
- `create-module`: Build complete modules
- `edit-agent`, `edit-workflow`, `edit-module`: Modification tools
- `module-brief`: Strategic planning for modules
- `convert-legacy`: v4 to v6 migration

**Agent Types:**
- Full Module Agent (complete persona + commands)
- Hybrid Agent (shared core + extensions)
- Standalone Agent (independent, specialized)

### 4. CIS - Creative Intelligence Suite (`src/modules/cis/`)

**Purpose:** AI-powered creative facilitation

**5 Specialized Agents:**
- Carson (Brainstorming)
- Maya (Design Thinking)
- Dr. Quinn (Problem Solving)
- Victor (Innovation Strategy)
- Sophia (Storytelling)

**150+ Creative Techniques** across 5 workflows

### 5. BMGD - BMad Game Development (`src/modules/bmgd/`)

**Purpose:** Game-specific development workflows

**Agents:** Game Designer, Game Developer, Game Architect, Game Scrum Master

**Workflows:** Game briefs, architecture, production management

---

## Key Technical Concepts

### Agent Architecture

Agents are defined in YAML format (`.agent.yaml`) and compiled to Markdown (`.agent.md`):

```yaml
name: agent-name
role: Brief role description
persona: Multi-line personality and expertise
commands:
  - trigger: "*command-name"
    description: What this command does
    action: What the agent should do
```

**Compilation:** `tools/cli/lib/agent/compiler.js` transforms YAML → MD

### Workflow Structure

Workflows are YAML-configured with:
- Multi-step processes
- Configuration validation
- Web bundle support
- Context integration

### Module System

Each module follows a standard structure:
```
module-name/
├── agents/              # Module-specific agents
├── workflows/           # Module workflows
├── tasks/               # Atomic operations
├── teams/               # Agent collaboration configs
├── _module-installer/   # Installation config
└── docs/                # Module documentation
```

### Installation & Compilation

**CLI Entry Points:**
- `tools/bmad-npx-wrapper.js` - NPX wrapper
- `tools/cli/bmad-cli.js` - Main CLI

**Key Commands:**
- `npx bmad-method@alpha install` - Install BMad
- `npm run bundle` - Build web bundles
- `npm test` - Run all quality checks

### Web Bundles

Built via `tools/cli/bundlers/bundle-web.js`:
- Creates standalone bundles for ChatGPT, Claude Projects, Gemini
- Optimized for token efficiency
- Validation via `npm run validate:bundles`

---

## Development Workflow

### Setting Up for Development

```bash
# Clone repository
git clone https://github.com/bmad-code-org/BMAD-METHOD.git
cd BMAD-METHOD

# Install dependencies (Node >=20 required)
npm install

# Run quality checks
npm test

# Lint and format
npm run lint
npm run format:check
npm run format:fix
```

### Code Quality Standards

**Required Before Commit:**
1. **Schema Validation:** `npm run validate:schemas`
2. **Linting:** ESLint with strict rules (`eslint.config.mjs`)
3. **Formatting:** Prettier with config (`prettier.config.mjs`)
4. **Tests:** All tests must pass (`npm test`)

**Pre-commit Hooks:** Husky + lint-staged configured

**Test Coverage:**
- `test/test-agent-schema.js` - Agent YAML validation
- `test/test-installation-components.js` - Installation validation

### Git Workflow

**Branch Strategy:**
- `main` - Stable production
- Feature branches: `feature/description` or `fix/description`
- Current development: Alpha releases on NPM

**Commit Convention:**
```
feat: New feature
fix: Bug fix
docs: Documentation only
refactor: Code restructuring
test: Test additions
chore: Build/tooling changes
```

**PR Guidelines:**
- Maximum 800 lines (excluding generated files)
- One feature/fix per PR
- Clear description (What/Why/How)
- Reference issue numbers

---

## Key Files & Their Purposes

### Configuration
- `package.json` - Project metadata, scripts, dependencies
- `eslint.config.mjs` - Linting rules
- `prettier.config.mjs` - Code formatting
- `.nvmrc` - Node version specification
- `.npmrc` - NPM configuration

### Documentation
- `README.md` - Main project overview
- `CONTRIBUTING.md` - Contribution guidelines
- `CHANGELOG.md` - Version history
- `docs/` - Comprehensive documentation

### Tools
- `tools/cli/bmad-cli.js` - Main CLI interface
- `tools/cli/lib/agent/compiler.js` - YAML → MD compilation
- `tools/cli/bundlers/bundle-web.js` - Web bundle creation
- `tools/flattener/` - Project documentation generator
- `tools/validate-*.js` - Validation utilities
- `tools/schema/` - YAML schema definitions

### Testing
- `test/test-agent-schema.js` - Agent validation
- `test/test-installation-components.js` - Installation tests
- `test/fixtures/` - Test data

---

## Common Enhancement Areas

### 1. Agent Development

**Opportunities:**
- Create domain-specific agents (legal, medical, finance, education)
- Enhance existing agent personas
- Add new commands to agents
- Improve agent collaboration patterns

**Tools:** Use BMB module (`*create-agent` workflow)

### 2. Workflow Creation

**Opportunities:**
- Add specialized workflows for new domains
- Enhance existing workflow efficiency
- Create compliance/governance workflows
- Build deployment pipeline workflows

**Tools:** Use BMB module (`*create-workflow`)

### 3. Module Extensions

**Opportunities:**
- Build complete industry-vertical modules
- Create technology-stack-specific modules
- Add business process modules
- Develop educational framework modules

**Tools:** Use BMB module (`*create-module`, `*module-brief`)

### 4. Documentation

**Opportunities:**
- Expand user guides with more examples
- Create video tutorial scripts
- Write troubleshooting guides
- Document advanced patterns

**Files:** `docs/`, module `docs/` folders

### 5. Tooling & Infrastructure

**Opportunities:**
- Enhance CLI with new commands
- Improve bundle optimization
- Add CI/CD enhancements
- Create migration tools
- Build analytics/telemetry

**Files:** `tools/cli/`, `.github/workflows/`

### 6. Testing & Quality

**Opportunities:**
- Increase test coverage
- Add integration tests
- Create end-to-end workflow tests
- Build performance benchmarks
- Add security scanning

**Files:** `test/`, add new test files

### 7. IDE Integrations

**Opportunities:**
- Enhance Claude Code support
- Improve Cursor integration
- Add Windsurf optimizations
- Create VS Code extensions

**Files:** `docs/ide-info/`, `src/modules/bmm/sub-modules/`

### 8. Scale-Adaptive Intelligence

**Opportunities:**
- Refine Level 0-4 classification logic
- Add auto-detection improvements
- Create custom scale profiles
- Build complexity metrics

**Files:** `src/modules/bmm/workflows/`, related docs

---

## Dependencies & Tech Stack

### Core Dependencies
- **CLI Tools:** `commander`, `inquirer`, `ora`, `boxen`, `chalk`, `figlet`
- **File Processing:** `fs-extra`, `glob`, `ignore`
- **Data Parsing:** `js-yaml`, `yaml`, `csv-parse`, `xml2js`
- **Utilities:** `semver`, `wrap-ansi`

### Dev Dependencies
- **Linting:** `eslint`, `eslint-plugin-*`
- **Formatting:** `prettier`, `prettier-plugin-packagejson`
- **Testing:** `jest`, `c8` (coverage)
- **Git Hooks:** `husky`, `lint-staged`
- **Schema Validation:** `zod`

### Build Tools
- Node.js >=20.0.0
- NPM package distribution
- Web bundler (custom)

---

## Testing Strategy

### Test Suites

1. **Schema Tests** (`npm run test:schemas`)
   - Validates all `.agent.yaml` files
   - Ensures YAML structure compliance
   - Checks for required fields

2. **Installation Tests** (`npm run test:install`)
   - Validates installation components
   - Checks module structure
   - Verifies dependency resolution

3. **Bundle Validation** (`npm run validate:bundles`)
   - Ensures web bundles are complete
   - Validates bundle structure
   - Checks for missing dependencies

4. **Lint Checks** (`npm run lint`)
   - JavaScript/YAML/Markdown linting
   - Enforces code style
   - Zero warnings tolerance

5. **Format Checks** (`npm run format:check`)
   - Prettier formatting validation
   - Ensures consistent style

### Running Tests

```bash
# All tests
npm test

# Individual suites
npm run test:schemas
npm run test:install
npm run validate:bundles
npm run lint
npm run format:check

# With coverage
npm run test:coverage
```

---

## Build & Release Process

### Development Build

```bash
# Bundle web versions
npm run bundle

# Rebundle (incremental)
npm run rebundle

# Flatten project (documentation)
npm run flatten
```

### Release Workflow

```bash
# Trigger release (requires gh CLI)
npm run release:patch   # 6.0.0 → 6.0.1
npm run release:minor   # 6.0.0 → 6.1.0
npm run release:major   # 6.0.0 → 7.0.0

# Watch release
npm run release:watch
```

**Release Process:**
1. GitHub Actions workflow triggered
2. Version bump in `package.json`
3. CHANGELOG updated
4. Git tag created
5. NPM publish (stable or alpha channel)
6. GitHub release created

---

## Common Patterns & Best Practices

### Agent Development

✅ **Do:**
- Keep dev agents lean (coding context, not docs)
- Use natural language (Markdown)
- Follow YAML schema strictly
- Test with various LLMs
- Document commands clearly

❌ **Don't:**
- Include code in agent definitions
- Make agents too verbose
- Duplicate functionality across agents
- Skip schema validation

### Workflow Design

✅ **Do:**
- Create atomic, focused workflows
- Provide clear step-by-step instructions
- Support context integration
- Enable web bundle export
- Document prerequisites

❌ **Don't:**
- Create monolithic workflows
- Assume user knowledge
- Skip configuration validation
- Forget error handling

### Module Creation

✅ **Do:**
- Follow established structure
- Include comprehensive docs
- Provide installation config
- Create team compositions
- Test integration with BMad Core

❌ **Don't:**
- Fragment BMad Core foundation
- Create tight coupling
- Skip dependency documentation
- Ignore naming conventions

---

## Troubleshooting Common Issues

### Installation Issues
- **Problem:** Installation fails
- **Solution:** Check Node version (>=20), clear npm cache, review install logs

### Agent Not Loading
- **Problem:** Agent YAML errors
- **Solution:** Run `npm run validate:schemas`, check YAML syntax

### Bundle Build Failures
- **Problem:** Web bundle creation fails
- **Solution:** Run `npm run validate:bundles`, check for missing dependencies

### Test Failures
- **Problem:** Tests don't pass
- **Solution:** Review specific test output, ensure all files saved, check for YAML errors

---

## Contributing Guidelines Summary

### Before Contributing

1. **Join Discord:** Get help in #general-dev, #bugs-issues
2. **Check Issues:** Avoid duplicates
3. **Discuss Features:** Post in Discord first
4. **Read CONTRIBUTING.md:** Full guidelines

### PR Requirements

- **Size:** Max 800 lines (excluding generated)
- **Scope:** One feature/fix per PR
- **Description:** Clear What/Why/How (max 200 words)
- **Tests:** All must pass
- **Commits:** Conventional format, atomic changes
- **Branch:** Submit to `main` (critical fixes only)

### Code Standards

- Follow existing patterns
- Validate YAML schemas
- Pass all linting/formatting
- Include clear comments
- Keep natural language (Markdown)

---

## Future Roadmap Opportunities

### Near-Term Enhancements

1. **Documentation**
   - More video tutorials
   - Interactive guides
   - Advanced pattern documentation

2. **Testing**
   - Increase coverage to 80%+
   - Add integration tests
   - Create E2E workflow tests

3. **Tooling**
   - Enhanced CLI features
   - Better error messages
   - Performance optimizations

### Long-Term Vision

1. **Community Marketplace**
   - Share custom modules
   - Agent library
   - Workflow templates

2. **Enterprise Features**
   - Team collaboration tools
   - Governance frameworks
   - Compliance modules

3. **Platform Expansion**
   - More IDE integrations
   - Cloud-based workflows
   - Mobile support

4. **AI Model Support**
   - Multi-model testing
   - Model-specific optimizations
   - Fallback strategies

---

## Resources & Links

### Official
- **Website:** https://bmad-code-org.github.io/bmad-bundles/
- **GitHub:** https://github.com/bmad-code-org/BMAD-METHOD
- **NPM:** https://www.npmjs.com/package/bmad-method
- **Discord:** https://discord.gg/gk8jAdXWmj
- **YouTube:** https://www.youtube.com/@BMadCode

### Documentation
- **Main Docs:** `docs/index.md`
- **BMM Docs:** `src/modules/bmm/docs/README.md`
- **Quick Start:** `src/modules/bmm/docs/quick-start.md`
- **Contributing:** `CONTRIBUTING.md`
- **Changelog:** `CHANGELOG.md`

### Development
- **Issues:** https://github.com/bmad-code-org/BMAD-METHOD/issues
- **Discussions:** https://github.com/bmad-code-org/BMAD-METHOD/discussions
- **v4 Archive:** https://github.com/bmad-code-org/BMAD-METHOD/tree/V4

---

## Quick Reference Commands

### Development
```bash
npm install              # Install dependencies
npm test                 # Run all tests
npm run lint             # Lint code
npm run lint:fix         # Auto-fix lint issues
npm run format:check     # Check formatting
npm run format:fix       # Auto-format code
```

### Building
```bash
npm run bundle           # Build all web bundles
npm run rebundle         # Rebuild modified bundles
npm run flatten          # Generate project docs
```

### Installation
```bash
npm run bmad:install     # Install BMad locally
npm run bmad:status      # Check installation
npm run bmad:agent-install  # Install specific agent
```

### Validation
```bash
npm run validate:schemas # Validate all YAML
npm run validate:bundles # Validate web bundles
npm run test:schemas     # Test agent schemas
npm run test:install     # Test installation
```

### Release
```bash
npm run release:patch    # Patch release
npm run release:minor    # Minor release
npm run release:major    # Major release
npm run release:watch    # Watch release progress
```

---

## Final Notes for Claude

### Working with This Codebase

When enhancing this codebase:

1. **Understand the Philosophy:** Human amplification, not replacement
2. **Follow Conventions:** YAML schemas, Markdown templates, natural language
3. **Think Modular:** Leverage BMad Core, build in modules
4. **Test Thoroughly:** Schemas, linting, formatting, all tests
5. **Document Clearly:** Users and AI need excellent docs
6. **Community First:** Discuss in Discord, align with roadmap

### Common Tasks

**Adding a New Agent:**
1. Use BMB: `*create-agent`
2. Define YAML in appropriate module
3. Validate: `npm run validate:schemas`
4. Test with various LLMs
5. Document in module README

**Creating a Workflow:**
1. Use BMB: `*create-workflow`
2. Create workflow directory with `workflow.yaml`
3. Add supporting files (README, templates)
4. Test end-to-end
5. Update module documentation

**Fixing a Bug:**
1. Create/find issue in GitHub
2. Create fix branch: `fix/description`
3. Make minimal changes
4. Test: `npm test`
5. Submit focused PR

### Key Principles

- **Partnership Over Automation**
- **Guided Discovery Over Direct Answers**
- **Structured Flexibility**
- **Scale-Adaptive Intelligence**
- **Community-Driven Evolution**

---

**Version:** 6.0.0-alpha.12
**Last Updated:** 2025-11-20
**Maintained By:** BMad Code, LLC and Community Contributors

**Questions?** Join [Discord](https://discord.gg/gk8jAdXWmj) or open a [GitHub Issue](https://github.com/bmad-code-org/BMAD-METHOD/issues).
