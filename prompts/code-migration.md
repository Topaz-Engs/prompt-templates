---
framework: CRTSE
model: all
---

# Code Migration Prompt

## Context
- **Source:** [e.g., JavaScript -> TypeScript, Flow -> TypeScript, REST -> GraphQL, Vue 2 -> Vue 3, AngularJS -> React]
- **Scope:** [specific module / entire app / library]
- **Size estimate:** [LOC to migrate / files to touch]
- **Deadline:** [if applicable]

## Role

Act as a senior engineer specializing in safe, incremental code migrations. Minimize risk by migrating in small, verifiable steps.

## Task

Create a migration plan and execute it for the described codebase migration. The plan must be incremental — each step must be independently verifiable and mergeable.

## Standards

1. **Incremental** — never mix migration with feature work or refactoring
2. **Verifiable** — each step must pass existing tests (and new ones if coverage is lacking)
3. **Backward compatible** — the system must remain deployable after each step
4. **No silent changes** — any behavior change must be intentional and documented
5. **Automated where possible** — prefer codemods and automated transforms over manual changes

## Migration Strategy

1. **Audit** — map all files, dependencies, and implicit contracts affected
2. **Prepare** — add types/interfaces alongside existing code (gradual typing)
3. **Strictify** — enable strict mode and fix violations one module at a time
4. **Clean up** — remove old patterns, dead code, and migration scaffolding

## Source

```[language]
[provide source files or paths]
```

## Deliverable

- Step-by-step migration plan with dependencies between steps
- For each step: files to change, expected diff size, verification method
- Migration scripts or codemods where applicable
- Risk assessment: what could go wrong and how to mitigate
