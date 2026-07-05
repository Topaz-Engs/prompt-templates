# Schema Reference

Formal schemas for prompts, skills, and configuration files in this repository.

---

## 1. Prompt Template Schema

Each prompt in `prompts/` follows the CRTSE framework with YAML frontmatter.

### Frontmatter

```yaml
---
# REQUIRED fields
framework: CRTSE                    # Always "CRTSE"
model: all                          # Compatible models: all, claude, gpt, gemini

# OPTIONAL fields
version: 1.0.0                      # Semantic version
author: team                        # Author or maintainer
tags: [code-review, security]       # Categorization tags
---
```

### Body Sections

```
# Title (H1)

## Context
- **Key:** value pairs describing the project environment
- Covers: language, framework, database, build tools, team size, constraints
- Be specific: versions matter ("Node 22" not "Node")

## Role
Who the agent should act as. Single paragraph.

- Drives tone, depth, and priorities
- Examples: "senior backend engineer", "security auditor", "DevOps engineer"

## Task
What the agent should do. Bullet list or numbered steps.

- Each item is specific and testable
- One task per prompt (multiple tasks = multiple prompts)
- Use acceptance criteria language ("returns X when Y")

## Standards
Quality requirements the output must meet. Table or bullet list.

| Dimension | Requirement |
|-----------|-------------|
| Performance | p95 < 200ms |
| Testing | 90% coverage minimum |
| Style | Follow existing patterns in src/ |

## Examples (optional but recommended)
Input/output pairs showing desired behavior.

- Use code blocks
- Show edge cases and error states too
- Format: "Input X → Expected output Y"

## Code / Diff (as needed)
```
--- prompt/code/belongs/here
```
```

---

## 2. Skill Definition Schema (SKILL.md)

Each skill in `skills/` follows the SKILL.md open standard.

### Frontmatter

The frontmatter is YAML between `---` delimiters.

```yaml
---
# REQUIRED
name: skill-name                    # lowercase-alphanumeric-with-hyphens
description: >                      # 1-1024 chars
  One-line description of when to use this skill.
  The agent reads this to decide if the skill matches the task.

# OPTIONAL
risk: low                           # low | medium | high
model: all                          # all | claude | gpt | gemini
license: MIT                        # SPDX identifier
compatibility: opencode             # opencode | claude-code | codex | all
metadata:                           # String-to-string map
  audience: developers
  category: code-quality
---
```

#### Name rules
- 1-64 characters
- Lowercase alphanumeric with single hyphen separators
- Must not start or end with `-`
- Must not contain consecutive `--`
- Regex: `^[a-z0-9]+(-[a-z0-9]+)*$`

### Body Sections

```
# Skill Name (H1)

[One paragraph explaining the skill's purpose and when to invoke it.]

## Execution Workflow
[Step-by-step procedure. Numbered list preferred.]

## Quality Bar
[Standards the output must meet. Tables or bullet lists.]

## Patterns
[What to do. Examples of good approaches. Include code blocks.]

## Anti-Patterns
[What to avoid. Examples of common mistakes.]

## Output Format (optional)
[Expected output structure if non-obvious.]
```

### Risk Levels

| Risk | Meaning | Example |
|------|---------|---------|
| `low` | Read-only analysis, no side effects | Code review, documentation |
| `medium` | Modifies code or config, but safe rollback | Refactoring, test writing |
| `high` | Destructive operations, security-sensitive | Security audit, migration, DB schema |

---

## 3. Plugin Manifest Schema (plugin.json)

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What this plugin does",
  "author": "your-name",
  "license": "MIT",

  "skills": ["skill-one", "skill-two"],
  "hooks": {
    "preToolUse": "hooks/pre-tool-use.json",
    "postToolUse": "hooks/post-tool-use.json"
  },
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@scope/server-package", "."],
      "env": {
        "API_KEY": "${API_KEY}"
      }
    }
  },
  "agents": ["agents/my-agent.agent.md"],
  "commands": ["commands/my-command.md"],

  "minAppVersion": "2.1.0",
  "compatibility": ["claude-code", "opencode", "codex"]
}
```

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | Unique plugin name (lowercase, hyphens) |
| `version` | string | yes | SemVer |
| `description` | string | yes | Short description |
| `skills` | string[] | no | List of skill directory names |
| `hooks` | object | no | Pre/post tool use hook configs |
| `mcpServers` | object | no | MCP server definitions |
| `agents` | string[] | no | Custom agent files |
| `commands` | string[] | no | Custom slash command files |
| `minAppVersion` | string | no | Minimum agent version required |
| `compatibility` | string[] | no | Compatible agent platforms |

---

## 4. Hook Schema

### Pre-Tool-Use Hook

```json
{
  "name": "security-check",
  "description": "Validate tool calls before execution",
  "hookPoint": "preToolUse",
  "condition": {
    "toolName": ["bash", "write", "edit"],
    "toolNamePattern": "rm|del|drop"
  },
  "action": "validate",
  "rules": [
    {
      "pattern": "rm -rf",
      "response": "block",
      "message": "rm -rf is blocked via agent"
    },
    {
      "pattern": "DROP TABLE",
      "response": "confirm",
      "message": "Are you sure you want to DROP TABLE?"
    }
  ]
}
```

### Post-Tool-Use Hook

```json
{
  "name": "log-results",
  "description": "Log tool execution results",
  "hookPoint": "postToolUse",
  "selector": {
    "toolName": ["*"]
  },
  "action": "log",
  "fields": ["toolName", "duration", "success", "error"]
}
```

---

## 5. MCP Server Config Schema

### For Claude Desktop (`claude_desktop_config.json`)

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@scope/server-package", "arg1", "arg2"],
      "env": {
        "API_KEY": "${API_KEY_VAR}"
      }
    }
  }
}
```

### For OpenCode (`opencode.json`)

```json
{
  "mcp": {
    "servers": {
      "server-name": {
        "command": "npx",
        "args": ["-y", "@scope/server-package"],
        "env": {
          "TOKEN": "${TOKEN}"
        }
      }
    }
  }
}
```

---

## 6. AGENTS.md / CLAUDE.md Schema

```markdown
# Project Overview
- What does this project do?
- Who is the target audience?

# Tech Stack
- Language: [Language] [version]
- Runtime: [Runtime] [version]
- Framework: [Framework] [version]
- Database: [Database] [version] + [ORM]
- Testing: [Test framework]
- Linting: [Linter name]
- CI/CD: [Platform]

# Conventions
- [Coding conventions, patterns, naming]
- [Component/folder structure]
- [Error handling pattern]
- [Testing conventions]

# Common Commands
- `command` — what it does
- `command` — what it does
- `command` — what it does

# Architecture Decisions
- [Reference ADR documents or link to docs/adr/]

# Security Rules
- [Hardcoded secrets forbidden — use env vars]
- [All SQL must be parameterized]
- [Auth required on all endpoints unless listed in PUBLIC_ROUTES]

# Agent Instructions
- [Behavioral rules for the AI agent]
- [What to avoid, what to always do]
```

---

## 7. Example: Complete Prompt File

File: `prompts/example-prompt.md`

```markdown
---
framework: CRTSE
model: all
tags: [example, template]
---

# Example Prompt Title

## Context
- **Language:** TypeScript 5.5
- **Framework:** React 19 with Next.js 15 (App Router)
- **Database:** PostgreSQL 16 via Prisma
- **Project:** E-commerce platform

## Role

Act as a senior frontend engineer specializing in React and Next.js.

## Task

Implement a product search page at `/search?q=query` that:
1. Displays results in a responsive grid (4 cols desktop, 2 tablet, 1 mobile)
2. Shows loading skeleton while fetching
3. Handles empty results, network errors, and invalid queries
4. Debounces input by 300ms before searching

## Standards

- Server Components for initial data fetch, Client Components for interactivity
- Tailwind CSS for styling (no CSS modules)
- Loading states with `<Suspense>` and fallback skeletons
- Error states with `error.tsx` boundary
- Responsive design matching Figma breakpoints (sm: 640, md: 768, lg: 1024, xl: 1280)

## Examples

- `/search?q=shoes` → grid of product cards with image, name, price
- `/search?q=¨` → empty state "No results found for ..."
- `/search` (no query) → redirect to `/products`

## Related Files

- `src/app/search/page.tsx` — main page to create/modify
- `src/components/product/ProductCard.tsx` — card component
- `src/lib/api/products.ts` — API helper for searching
```
