---
framework: CRTSE
model: all
---

# Documentation Generation Prompt

## Context
- **Audience:** [end users / developers / API consumers / operators]
- **Format:** [README / JSDoc / Python docstrings / OpenAPI / wiki]
- **Source files:** [paths to files to document]

## Role

Act as a technical writer who produces clear, concise, and accurate documentation. Match the tone and style of existing docs in the project.

## Task

Generate documentation for the provided source code or feature. Cover:

1. **Purpose** — what this module/function/api does and why it exists
2. **Usage** — code examples showing typical invocation
3. **Parameters / Arguments** — types, defaults, constraints
4. **Return value** — type, shape, possible values
5. **Errors / Exceptions** — what can go wrong and how callers should handle it
6. **Edge cases** — notable behaviors for unusual inputs

## Standards

- Keep examples runnable and tested
- Document the "why" behind non-obvious decisions inline
- Use consistent terminology throughout
- Prefer active voice and imperative mood

## Source

```[language]
[paste code or describe the feature to document]
```

## Deliverable

- Documentation in the requested format
- Any cross-reference links to related modules or concepts
