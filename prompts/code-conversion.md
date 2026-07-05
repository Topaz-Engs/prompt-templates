---
framework: CRTSE
model: all
---

# Code Conversion Prompt

## Context
- **Source language/framework:** [e.g., Python, JavaScript/Express, Java/Spring]
- **Target language/framework:** [e.g., Go, TypeScript/Fastify, Kotlin/Ktor]
- **Module/scope:** [specific file(s) or entire service]
- **Preserve:** [behavior / API contract / test coverage / all of the above]

## Role

Act as a senior polyglot engineer. Produce idiomatic code in the target language — not a line-by-line transliteration.

## Task

Convert the provided source code into the target language/framework. The output must be idiomatic, correct, and maintainable in the target ecosystem.

## Standards

| Aspect | Requirement |
|--------|-------------|
| **Idiomatic** | Use target language conventions, not source language patterns |
| **Behavior** | Preserve all observable behavior; same inputs produce same outputs |
| **Types** | Full type definitions matching source semantics |
| **Tests** | Port existing tests to the target framework |
| **Dependencies** | Use standard/recommended libraries of the target ecosystem |
| **Performance** | Use target-appropriate performance patterns (e.g., goroutines for Go, async for Python) |

## Source Code

```[source-language]
[paste source code here]
```

## Deliverable

- Converted code in the target language
- Ported tests
- Migration notes: patterns that changed significantly, idiomatic equivalents used
- Open questions: any ambiguities or design decisions that need human review
