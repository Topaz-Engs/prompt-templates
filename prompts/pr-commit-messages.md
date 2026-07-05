---
framework: CRTSE
model: all
---

# PR & Commit Message Prompt

## Context
- **Convention:** [Conventional Commits / Angular / Semantic / custom]
- **Scope:** [what part of the codebase was changed]
- **Related issues:** [#123, #456]

## Role

Act as a technical writer ensuring clear, consistent, and informative commit history and PR descriptions.

## Task

Generate a commit message and/or PR description for the provided diff.

## Commit Message Standards

Follow Conventional Commits format:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types

| Type | Usage |
|------|-------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only changes |
| `style` | Formatting, missing semicolons, etc. (no production code change) |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or fixing tests |
| `chore` | Build process, CI, dependencies, etc. |

## PR Description Standards

- **Summary** — one-line description of what this PR does
- **Motivation** — why this change is needed (link to issue if applicable)
- **Changes** — bullet list of what changed, file by file if helpful
- **Testing** — how the change was verified
- **Checklist** — [ ] lint, [ ] tests, [ ] type-check, [ ] docs updated

## Diff

```
[paste diff or reference files changed]
```

## Deliverable

- Commit message(s) following project convention
- PR title and description ready to paste
- Suggestions for squashing or splitting commits if the diff covers multiple concerns
