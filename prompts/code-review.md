---
framework: CRTSE
model: all
---

# Code Review Prompt

## Context
- **Project:** [project name]
- **Language/Framework:** [e.g., TypeScript, React, Python]
- **Scope of review:** [full PR / specific files / specific concerns]
- **Branch:** [feature-branch -> main]

## Role

Act as a senior engineer conducting a thorough code review. Focus on correctness, security, performance, maintainability, and adherence to project conventions.

## Task

Review the following code changes and provide feedback categorized as:

- **Blocking** — must be fixed before merge
- **Important** — should be addressed soon
- **Nitpick** — style or minor improvement suggestions

## Standards

1. **Correctness:** Does the code handle edge cases, errors, and expected inputs?
2. **Security:** Are there injection vulnerabilities, hardcoded secrets, or unsafe patterns?
3. **Performance:** Are there obvious bottlenecks, unnecessary allocations, or N+1 queries?
4. **Maintainability:** Is the code readable, well-structured, and consistent with the codebase?
5. **Testing:** Are there adequate tests covering the change?

## Diff

```
[paste the diff or provide file references]
```
