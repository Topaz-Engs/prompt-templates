# Best Practices for Working with AI Coding Agents

## 1. Project Setup & Context

### AGENTS.md / CLAUDE.md
Create a project context file at the repo root. This file is read automatically by most AI agents on every session.

```markdown
# Project Overview
- What does this project do?
- Who is the target audience?

# Tech Stack
- Language: TypeScript 5.x
- Runtime: Node 22
- Framework: Next.js 15 (App Router)
- Database: PostgreSQL 16 + Prisma ORM
- Testing: Vitest + Playwright
- Linting: Biome (no ESLint/Prettier)

# Conventions
- Functional components with hooks (no class components)
- Named exports for utilities, default exports for pages
- PascalCase for types/interfaces, camelCase for variables/functions
- CSS Modules for component styles

# Common Commands
- `pnpm dev` — start dev server
- `pnpm test` — run unit tests
- `pnpm test:e2e` — run e2e tests
- `pnpm lint` — lint and format check
- `pnpm typecheck` — TypeScript check
```

### Key elements to include
- Tech stack with versions
- Coding conventions and patterns
- Testing approach and commands
- Architectural decisions (ADR references)
- Link to contributor guide

## 2. Writing Effective Prompts

### The CRTSE Framework

| Component | What to include |
|-----------|----------------|
| **Context** | Tech stack, relevant files, current state, constraints |
| **Role** | Who the agent should act as (senior engineer, security expert) |
| **Task** | Specific deliverable with acceptance criteria |
| **Standards** | Quality bars, conventions, must-haves vs nice-to-haves |
| **Examples** | Input/output pairs when format matters |

### Do's and Don'ts

| Do | Don't |
|----|-------|
| Be specific about language, framework, versions | "Write a REST API" (no context) |
| Reference specific files with paths | Paste entire files without guidance |
| State what NOT to do | Assume the agent shares your assumptions |
| Provide acceptance criteria | "Fix this bug" (vague) |
| One task per prompt | "Refactor this and add tests and deploy" |

### Prompt templates for common tasks

See the `prompts/` directory for ready-to-use templates:
- `code-review.md` — Systematic PR review with categorized feedback
- `debugging.md` — Root cause analysis workflow
- `test-generation.md` — Thorough test suite generation
- `api-design.md` — REST/GraphQL API design with standards
- `architecture-planning.md` — Structured architecture proposals
- `security-audit.md` — OWASP-focused security review
- `refactoring.md` — Behavior-preserving code improvement

## 3. Review Discipline

### Before accepting agent output
1. **Read the diff** — understand every changed line
2. **Run tests** — `pnpm test` (or equivalent)
3. **Check type safety** — `pnpm typecheck`
4. **Lint** — `pnpm lint`
5. **Review for edge cases** — what happens with empty/null/unexpected input?
6. **Check the non-functional** — performance, security, error handling

### Build a review habit
> After every agent session, look at the diff before you commit. This single practice prevents more bugs than any amount of prompt engineering.

## 4. Context Management

AI agents have finite context windows. Manage them carefully:

- **Start fresh sessions** for unrelated tasks instead of piling into one conversation
- **Reference files by path** instead of pasting their full content when the agent can read them
- **Summarize progress** when starting a new session mid-task
- **Remove irrelevant context** — don't include files that aren't related to the task
- **Set context budgets** — for long-running tasks, break into phases with checkpoints

When a session runs long, start a new one with:
```
Continuing from previous session. Current state:
- Implemented: [list what was done]
- Pending: [list what remains]
- Key decisions: [list decisions made so far]
```

## 5. Spec-Driven Development

Before asking an agent to write code, write a spec:

```markdown
# Feature: User Email Verification

## Goal
Users who register with email should verify ownership before accessing the app.

## Acceptance Criteria
1. On registration, send a verification email with a 1-hour expiry link
2. Clicking the link marks the user as verified
3. Unverified users cannot access `/dashboard` or `/settings`
4. Resend verification is rate-limited to 1 request per 60 seconds

## Technical Constraints
- Use the existing email service (`src/lib/email.ts`)
- Store verification tokens in Redis with TTL (not in PostgreSQL)
- Follow the existing middleware pattern for auth checks

## Out of Scope
- Phone verification
- OAuth provider email verification
```

The more structured your input, the less time you spend fixing the output.

## 6. Running Multiple Agents in Parallel

One of the biggest advantages of agentic development is parallelism:

- **Isolation** — each agent works on a separate branch or independent files
- **Clear interfaces** — define contracts between parallel work streams
- **Orchestration** — a human or parent agent coordinates and merges outputs

Example: While one agent implements a backend API, another builds the frontend, and a third writes tests. This works only if the interfaces (API contract, component props) are defined upfront.

## 7. Security Considerations

### Never trust agent output blindly
- Always review authentication/authorization code
- Run SAST tools on AI-generated code
- Scan for secrets before committing
- Have a security expert review critical paths

### Common security issues in agent-generated code
| Issue | Why it happens | Prevention |
|-------|---------------|------------|
| Injection vulnerabilities | Agent uses string concatenation | Require parameterized queries |
| Hardcoded secrets | Agent doesn't know your secret management | Add to AGENTS.md |
| Missing auth checks | Agent assumes endpoint is internal | Enforce middleware patterns |
| Insecure defaults | Agent optimizes for convenience | Define security baselines |
| Outdated dependencies | Agent doesn't check for known CVEs | Automated dependency scanning |

### Security rules to add to AGENTS.md

```markdown
# Security Rules
- Never hardcode API keys, tokens, or passwords — use environment variables
- All SQL must use parameterized queries (no string interpolation)
- All user input must be validated and sanitized
- Authentication required for every endpoint unless explicitly listed as public
- Use `helmet` middleware in Express / equivalent in other frameworks
- Rate limiting on all auth endpoints
```

## 8. Handling Agent Limitations

| Limitation | Mitigation |
|------------|------------|
| **Context window** | Break work into phases; start fresh sessions |
| **Outdated knowledge** | Reference specific library versions in AGENTS.md |
| **Hallucination** | Always verify; run tests; review the diff |
| **Inconsistency** | Use structured outputs (JSON schemas, types) |
| **Overconfidence** | Ask for confidence levels; question output critically |

### When NOT to use an AI agent
- Security-critical code that needs human audit (crypto, auth core)
- Complex multi-step refactors with implicit dependencies
- Novel algorithms or domains where correctness is hard to verify
- Legal, compliance, or regulated logic

## 9. Iteration Loop

The most effective workflow is tightly iterative:

```
1. Write spec → 2. Ask agent → 3. Review output → 4. Give feedback → 5. Repeat
```

### Giving effective feedback
- Reference specific lines: "The `handleLogin` function on line 42 doesn't validate the email format"
- State what's wrong and what good looks like: "Instead of `any`, use the `User` type from `src/types.ts`"
- Prioritize: tell the agent which feedback is blocking vs nice-to-have

## 10. Team Conventions

### AGENTS.md maintenance
- Schedule monthly reviews
- Update when tech stack changes
- Add rules when seeing repeated mistakes
- Assign ownership to a specific team member

### Handling disagreements
1. Discuss in PR comments
2. Document the decision in AGENTS.md with rationale
3. Link to ADR (Architecture Decision Record) if significant
4. Revisit contentious rules quarterly

### Tool-agnostic rules
Keep the majority of your rules in AGENTS.md (works with any tool). Keep tool-specific config separate (e.g., `opencode.json`, `.cursorrules`).
