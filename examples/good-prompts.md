# Good Prompt Examples

Concrete, real-world prompts with the CRTSE framework fully filled in. Use these as references when writing your own.

---

## Example 1: Adding a Feature (REST Endpoint)

```
## Context
- Project: e-commerce backend
- Language: TypeScript 5.5, Node 22
- Framework: Express with Zod validation
- Database: PostgreSQL 16 via Prisma ORM
- Auth: JWT tokens in Authorization header
- Existing pattern: `src/routes/users.ts` for reference

## Role
Act as a senior backend engineer on this project. Follow the
existing patterns in `src/routes/` for error handling, response
formatting, and middleware usage.

## Task
Add a `GET /api/products/search` endpoint that:
1. Accepts `?q=searchterm` query parameter (required, min 2 chars)
2. Searches the `Product` table by `name` and `description` (case-insensitive)
3. Returns `{ results: Product[], total: number, page: number }`
4. Paginates with `?page=1&limit=20` (default page=1, limit=20, max limit=100)
5. Requires a valid JWT token in the Authorization header

## Standards
- Zod schema for input validation (see `src/schemas/`)
- Prisma raw query or `contains` mode: insensitive for search
- Return 400 if query is missing or under 2 chars
- Return 401 if token is invalid
- Match the JSON response format from `src/routes/users.ts`
- Add unit tests in `src/__tests__/routes/products.test.ts`
- Type-check, lint, and pass all tests before finishing

## Examples
- `GET /api/products/search?q=phone&page=1&limit=10`
  Response: `{ results: [...], total: 42, page: 1 }`
- `GET /api/products/search?q=ab`
  Response: 400 `{ error: "Query must be at least 2 characters" }`
```

**Why this works:** Every dimension is specified. The agent knows the stack, the exact endpoint shape, validation rules, auth requirements, error codes, and even which file to reference for style. No guessing.

---

## Example 2: Debugging a Performance Issue

```
## Context
- Application: Node.js 22 API server on AWS ECS (4 CPU, 16GB)
- Endpoint: POST /api/orders/create (handles ~500 req/min)
- Symptom: p95 latency jumped from 200ms to 4.2s after last deploy
- Deploy: commit a1b2c3d — "feat: add inventory reservation"
- Monitoring: Datadog traces available

## Role
Act as a senior performance engineer. Profile first, then fix.
Do not guess — use data.

## Task
Debug the latency regression on POST /api/orders/create.
Walk through:
1. What data would you look at first? (traces, logs, metrics)
2. What tools would you use to narrow the scope?
3. What hypotheses would you form?
4. How would you confirm or rule out each one?

## Standards
- One hypothesis at a time — do not batch changes
- Before/after measurements for any proposed fix
- Consider: DB queries (N+1?), missing index, sync I/O, serialization
- Check the diff of a1b2c3d for the likely culprit first

## Additional Context
- The `inventory.reserve()` function was added in a1b2c3d
- It calls an external inventory service with no timeout set
- The DB has 2M+ orders, no new index was added
- No new database indexes were added in the deploy
```

**Why this works:** Gives the agent real metrics, a specific commit to investigate, and contextual hints (no timeout on external call, no index). The agent can form targeted hypotheses instead of guessing.

---

## Example 3: TypeScript Migration (Incremental)

```
## Context
- Source: JavaScript (ES2022), 15k LOC across 120 files
- Target: TypeScript 5.5 with strict mode
- Build: Vite + tsc for type-checking only
- Test: Vitest, 80% coverage
- Deadline: 6 weeks, 2 engineers

## Role
Act as a migration specialist. Use the strangler fig pattern —
never break the build. Each PR must be independently mergeable.

## Task
Create an incremental migration plan from JS to TypeScript.
The plan must:
1. Start with tsconfig allowing JS (`allowJs: true`, `checkJs: false`)
2. Rename files .js -> .ts in batches of 5-10
3. Add types incrementally, using `any` as escape hatch
4. Enable `strict: true` only as a final phase
5. Keep the build green after every PR

## Standards
- No more than 10 files per PR
- Every PR must pass: lint, type-check (allowJs mode), full test suite
- Use `// @ts-expect-error` for unfixable type issues, tracked in a spreadsheet
- Library type packages must be installed (`@types/*`)
- Target `strict: true` with < 50 `@ts-expect-error` remaining
- Do NOT refactor logic while migrating types — separate PRs

## Examples
- Step 1 (Day 1-2): Setup tsconfig, install @types, rename 5 utility files
- Step 2 (Day 3-5): Migrate 10 model files with full interfaces
- Step 10 (Week 4): Enable `strict: true`, fix remaining 47 errors

## Reference
- See `best-practices/migration-plan-2026.md` for the full migration strategy
- See `src/utils/helpers.js` as the "easy first file" candidate
```

**Why this works:** Realistic scope, explicit constraints (file batch size, strict mode target, escape hatch tracking), and a sequenced plan. The agent can produce a concrete week-by-week schedule.

---

## Example 4: Database Schema Design

```
## Context
- Database: PostgreSQL 16
- ORM: Prisma
- Domain: Multi-tenant SaaS for project management
- Scale: 10k tenants, 500k users, 5M tasks
- Existing: `Tenant`, `User`, `Project` tables already designed

## Role
Act as a senior data architect. Design for query performance,
data integrity, and tenant isolation.

## Task
Design the schema for a task management system:

Requirements:
- Tasks belong to a Project
- Tasks have: title, description, status (enum), priority (enum),
  assignee (User), due date, tags (string array), attachments
- Tasks can have multiple comments (User + text + timestamp)
- Tasks can have subtasks (self-referential)
- Tasks have an activity log (who did what, when)
- Soft-delete for tasks and projects
- Query pattern: "show me all tasks assigned to me in this project,
  ordered by due date, filtered by status"

## Standards
- UUIDs for all primary keys (auto-generated)
- Tenant ID on every table (not just via join)
- `created_at` and `updated_at` timestamps on every table
- `deleted_at` nullable timestamp for soft deletes
- Indexes on: (tenant_id, status), (assignee_id, due_date), (project_id)
- No JSON/JSONB columns — use proper relations
- Migration must be backward-compatible for one deploy cycle
- Include the Prisma schema, not raw SQL

## Schema to design
[Tables: Task, Comment, TaskAssignee, ActivityLog, TaskTag]
```

**Why this works:** Specifies scale, existing tables, exact requirements, indexing strategy, and even the output format (Prisma schema). The agent won't waste cycles deciding between UUID and auto-increment.

---

## Example 5: Refactoring with Test Preservation

```
## Context
- Language: Python 3.12
- Module: `src/payment/calculator.py` (450 lines)
- Problem: Single 200-line function `calculate_fee()` with nested
  conditionals, 7 branches, and mixed concerns (validation, tax,
  discount, currency conversion)
- Tests: `tests/test_calculator.py` has 35 tests, 94% coverage

## Role
Act as a senior Python developer. Preserve every existing test
and all behavior. Do not change the public API.

## Task
Refactor `calculate_fee()` into smaller, single-responsibility
functions. Extract these concerns:
1. Input validation
2. Tax calculation
3. Discount application
4. Currency conversion
5. Fee computation

## Standards
- All 35 existing tests must pass with zero modifications
- Add new tests for any extracted function that lacks coverage
- Public function signature must remain identical
- Each extracted function gets a docstring describing inputs/outputs
- Keep extracted functions in the same module unless they exceed 5
- One extracted function per commit, with passing tests after each

## Code
[The 450-line calculate_fee function — not shown here for brevity]

## Deliverable
- Series of small refactoring commits, each preserving tests
- Brief rationale for each extraction
- Final diff summary
```

**Why this works:** Gives a hard constraint (35 tests, zero changes), specifies the extraction targets, and demands incremental commits. The agent knows exactly what "done" looks like.

---

## Example 6: Security Audit Request

```
## Context
- Project: FinTech mobile API (Node.js, Express, MongoDB)
- Scope: Authentication module only (`src/auth/`)
- Compliance: SOC 2 Type II, GDPR
- Auth methods: JWT + OAuth2 (Google, Apple)
- Recent pentest: 3 medium findings, all fixed

## Role
Act as an application security engineer. Assume an external
attacker with no privileged access. Check every OWASP Top 10
category against the auth module.

## Task
Audit `src/auth/` for security vulnerabilities.
For each finding, provide:
1. Vulnerability class (OWASP category)
2. File and line number
3. Exploit scenario (how an attacker would use this)
4. Severity (Critical/High/Medium/Low)
5. Specific remediation with code example

## Standards
- Check password policies (min length, hashing algorithm)
- Check JWT implementation (algorithm confusion, expiry, secret strength)
- Check OAuth2 flow (state parameter, redirect URI validation, CSRF)
- Check rate limiting on login/register/reset-password
- Check for hardcoded secrets, tokens, or API keys
- Check error messages (no stack traces, no internal info leaking)
- Check session management (token rotation, revocation)

## Files to audit
- src/auth/login.ts
- src/auth/register.ts
- src/auth/oauth.ts
- src/auth/jwt.ts
- src/auth/middleware.ts

## Deliverable
- Prioritized finding list with CVSS-style scores
- Remediation code for each finding
- Summary: pass / conditional pass / fail
```

**Why this works:** Narrow scope (one module), specific compliance requirements, files listed explicitly, and a clear deliverable format. The agent doesn't need to audit the whole codebase.

---

## Example 7: CI/CD Pipeline for a Monorepo

```
## Context
- Platform: GitHub Actions
- Repo: Monorepo with apps/web, apps/api, packages/shared
- Tools: pnpm workspaces, Turborepo, Docker
- Deploy: web → Vercel, api → Railway (Docker)
- Node 22, TypeScript 5.5

## Role
Act as a DevOps engineer. Prioritize speed: the entire CI
pipeline should complete in under 5 minutes.

## Task
Design a CI/CD pipeline with these stages:
1. Lint (all packages in parallel — 30s target)
2. Type-check (all packages in parallel — 1m target)
3. Test (unit tests per package, integration tests for api — 2m target)
4. Build (web + api with Turborepo caching — 1.5m target)
5. Deploy (main branch only, web→Vercel, api→Railway — 30s target)

## Standards
- Cache pnpm store and Turborepo outputs between runs
- Run affected packages only (Turborepo filtering)
- Fail fast: lint → type-check → test → build
- Security scan dependencies on every PR
- Docker layer caching for the api build
- Deploy requires approval for production
- Test matrix: Node 20 and 22
- Pipeline must not exceed 5 minutes (greenfield baseline)

## Additional Context
- 3 engineers, ~20 PRs/week
- Existing Dockerfile in apps/api/
- Vercel CLI is authenticated via OIDC
- Railway deploy via GitHub Action
```

**Why this works:** Concrete time targets, platform details, caching strategy, and team context. The agent can produce a real `.github/workflows/ci.yml` that meets measurable goals.
