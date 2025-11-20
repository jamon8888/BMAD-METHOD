# Pattern Library

This directory contains approved, reusable patterns for the Safe Expert methodology.

## Pattern Categories

- **api/** - API patterns (REST, webhooks, validation)
- **database/** - Database patterns (transactions, migrations)
- **ui/** - UI patterns (components, forms, layouts)
- **testing/** - Testing patterns (unit, integration, E2E)
- **data/** - Data patterns (ETL, pipelines)
- **security/** - Security patterns (auth, encryption)

## Pattern Discovery Protocol

Before implementing ANYTHING new:

1. **Check specs directory** for similar requirements
2. **Search codebase** for existing implementations
3. **Review THIS pattern library** for established patterns
4. **Get System Architect approval** if creating new pattern

## Adding New Patterns

New patterns require:
- System Architect approval
- Documentation with examples
- Rationale for why existing patterns don't fit
- ADR if architectural decision

## Using Patterns

Each pattern includes:
- When to use it
- How to implement it
- Common pitfalls
- Example code
- Related patterns
