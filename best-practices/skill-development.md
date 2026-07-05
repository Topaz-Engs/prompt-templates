# Skill Development Guide

## What is an Agent Skill?

A skill is a reusable instruction package that teaches an AI agent how to perform a specific domain of work. Skills encapsulate:

- **Domain knowledge** — what the agent needs to know
- **Workflows** — step-by-step procedures
- **Quality bars** — standards the output must meet
- **Patterns** — examples of good and bad approaches
- **Anti-patterns** — common mistakes to avoid

Skills are typically written as Markdown files (SKILL.md) with optional YAML frontmatter for metadata.

## Anatomy of a Skill

```markdown
---
name: skill-name
description: One-line description of when to use this skill
risk: low | medium | high
---

# Skill Name

[Core instructions for the agent]

## Execution Workflow

[Step-by-step procedure the agent should follow]

## Quality Bar

[Standards the output must meet]

## Patterns

[What to do — examples of good approaches]

## Anti-Patterns

[What to avoid — examples of common mistakes]
```

## When to Create a Skill

Create a skill when:
- You find yourself giving the same instructions repeatedly
- The task requires specialized domain knowledge
- You want to encode team or project conventions
- The work involves a multi-step process with quality gates
- You want to share expertise across the team

## Skill Design Principles

### 1. Narrow Scope
Each skill should cover one domain or workflow. A "full-stack development" skill is too broad; split it into "frontend-react", "api-design", and "database-schema".

### 2. Clear Triggers
The description field should make it obvious when to use the skill. A good trigger description answers: "When should the agent load this?"

### 3. Actionable Instructions
Tell the agent what to DO, not what to know. Instead of "Understanding testing is important", write "Write tests for every new function. Follow the patterns in `src/__tests__/`."

### 4. Examples Over Exhortations
Show, don't tell. Include concrete examples of good output and bad output.

### 5. Quality Gates
Define what "done" looks like. Include a checklist the agent should verify before finishing.

### 6. Progressive Disclosure
Put the most important instructions first. Use sections for detailed reference material the agent can skip if not needed.

## Skill Categories

### Code Quality Skills
- Code review
- Test writing
- Linting and formatting
- Type safety

### Development Workflow Skills
- Refactoring
- Debugging
- Performance optimization
- Migration planning

### Architecture Skills
- API design
- Database schema design
- System architecture
- Technology selection

### Operations Skills
- CI/CD pipeline
- Docker/Kubernetes
- Monitoring and observability
- Incident response

### Safety Skills
- Security review
- Secret scanning
- Prompt injection defense
- Compliance checking

## Skill Metadata

Good metadata helps agents discover and load the right skill:

| Field | Purpose | Example |
|-------|---------|---------|
| `name` | Unique identifier | `code-review` |
| `description` | Trigger description | "Review pull requests for correctness, security, and maintainability" |
| `risk` | Caution level | `low` (read-only), `medium` (modifies code), `high` (dangerous operations) |
| `model` | Compatibility | `all`, `claude`, `gpt` |

## Writing Effective Skill Descriptions

The description is the most important metadata — it determines when the agent loads the skill.

**Good description pattern:**
> "Use when [trigger condition]. Covers [domain scope]. Includes [key capabilities]."

Examples:
- "Use when debugging test failures. Covers reading test output, isolating failures, fixing test code, and verifying the fix."
- "Use when designing REST APIs. Covers endpoint naming, status codes, error formats, pagination, and versioning."

## Testing Skills

Before sharing a skill, validate it:

1. **Does the agent load it?** — Test with a prompt that should trigger the skill
2. **Does the agent follow it?** — Check if the output matches the instructions
3. **Does it improve output?** — Compare with and without the skill
4. **Is it up to date?** — Review for outdated examples or references

## Distributing Skills

Skills can be:
- **Project-local:** `.opencode/skills/skill-name/SKILL.md`
- **User-global:** `~/.opencode/skills/skill-name/SKILL.md`
- **Shared registry:** Published to a skill marketplace or GitHub repository
- **Team repository:** Centralized in a shared repo with version control

## Maintaining Skills

- **Version** — track changes; use semantic versioning if distributed
- **Review quarterly** — update for new tool versions, patterns, or conventions
- **Deprecate gracefully** — mark old skills as deprecated and link to replacements
- **Collect feedback** — ask users what's working and what's missing

## Example: Minimal Skill

```markdown
---
name: string-utilities
description: Helper for string manipulation tasks — formatting, validation, parsing, and transformation patterns.
risk: low
---

# String Utilities

## Always
- Use native string methods before reaching for lodash or similar
- Handle null/undefined inputs gracefully (return empty string or null)
- Consider Unicode and locale

## Never
- Use regex when built-in methods suffice
- Mutate the input string (strings are immutable in JS/TS; return a new one)

## Patterns

| Task | Approach |
|------|----------|
| Case conversion | `String.prototype.toLocaleUpperCase()` |
| Trimming whitespace | `String.prototype.trim()` |
| Checking emptiness | `str.length === 0` (not truthy check) |
| Slug generation | Remove non-alphanumeric, replace spaces with `-`, lowercase |
```
