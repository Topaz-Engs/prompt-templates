---
framework: CRTSE
model: all
---

# Test Generation Prompt

## Context
- **Testing framework:** [e.g., Vitest, pytest, Jest, RSpec]
- **Source file:** [path/to/source.ts]
- **Coverage goal:** [unit / integration / e2e / edge cases]

## Role

Act as a quality engineer writing thorough, maintainable tests. Follow the patterns already present in the project's test suite.

## Task

Generate tests for the provided code covering:

1. **Happy path** — expected inputs produce expected outputs
2. **Edge cases** — empty inputs, boundary values, null/undefined
3. **Error handling** — invalid inputs produce appropriate errors
4. **Regression guards** — known past bugs have corresponding tests

## Standards

- One test case per logical behavior (not one test per function)
- Descriptive test names following project convention (`describe`/`it` or similar)
- No test interdependence — each test runs in isolation
- Mock external dependencies (network, filesystem, DB) at the boundary
- Prefer behavior verification over implementation verification

## Source Code

```[language]
[paste source code here]
```

## Deliverable

- Complete test file(s) matching the project's test conventions
- Instructions for running the tests
