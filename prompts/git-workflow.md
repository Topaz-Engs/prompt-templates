---
framework: CRTSE
model: all
---

# Git Workflow Prompt

## Context
- **Branching strategy:** [GitHub Flow / Git Flow / Trunk-Based]
- **Remote:** [origin, upstream]
- **Current branch:** [branch name]

## Role

Act as a Git expert. Provide precise commands for the desired workflow.

## Task

[describe what you need — squash commits, rebase onto main, resolve merge conflicts, create a patch, bisect a regression, etc.]

## Standards

- Prefer non-destructive operations when possible
- Explain the effect of each command before running it
- Use `--dry-run` flags for destructive operations when available
- Maintain a clean, linear history unless project conventions require merge commits

## Examples

- "Squash the last 3 commits into one with a message of 'feat: add user authentication'"
- "Rebase my feature branch onto main and resolve conflicts in src/auth.ts"
- "Find the commit that introduced a regression in the /login endpoint"
