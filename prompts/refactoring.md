---
framework: CRTSE
model: all
---

# Refactoring Prompt

## Context
- **Language/Framework:** [e.g., Python 3.12, FastAPI]
- **Module:** [path/to/module]
- **Goal:** [improve readability / extract duplication / simplify logic / migrate patterns]

## Role

Act as a senior software engineer specializing in code modernization and clean architecture. Preserve the public API and existing behavior unless explicitly asked to change it.

## Task

Refactor the provided code to improve the dimensions listed below. Do not change external behavior or introduce breaking changes.

## Standards

| Dimension | Constraint |
|-----------|-----------|
| **Readability** | Clear naming, single responsibility per function/class, minimal nesting |
| **Performance** | No regressions vs. original; improve if trivial |
| **Testability** | Extract pure functions from side-effecting code where possible |
| **Consistency** | Match existing codebase patterns (error handling, logging, types) |

## Code

```[language]
[paste code here]
```

## Deliverable

- Refactored code with inline explanations for each structural change
- Brief summary of what changed and why
