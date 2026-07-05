# Bad Prompts & How to Fix Them

Real examples of weak prompts, why they fail, and how to fix them using the CRTSE framework.

---

## Example 1: The Vague Request

### Bad

> "Write a login system"

**Why it fails:** Zero context. The agent has to guess:
- Language? Framework? Database? Auth strategy? Session or JWT?
- Password rules? MFA? OAuth? Email verification?
- What UI? What API? What error handling?

Every guess is a chance to be wrong. The output will be generic tutorial code.

### Good

```
Context: Python 3.12, FastAPI, PostgreSQL 16 via SQLAlchemy,
         JWT auth with access + refresh tokens, React frontend
Role: Senior backend engineer
Task: Implement POST /api/auth/login that:
      1. Accepts { email: string, password: string }
      2. Validates credentials against the User table
      3. Returns { accessToken, refreshToken, expiresIn }
      4. Rate-limited to 5 attempts per email per 15 minutes
      5. Logs failed attempts to auth_attempts table
Standards: bcrypt for passwords, Zod-like Pydantic validation,
           proper HTTP status codes (200, 401, 429),
           type-annotated, tested (pytest), OpenAPI docs
```

**Why it works:** The agent knows the stack, the exact endpoint contract, security requirements, and quality bars. No guesswork.

---

## Example 2: No Task Boundary

### Bad

> "Refactor this file and optimize the database queries and also add error handling and write tests"

**Why it fails:** Three unrelated tasks in one prompt. The agent will:
- Do them all at once (hard to review)
- Skip one (ambiguous priority)
- Produce a massive diff mixing structural changes with test additions

**Fixing it means splitting into sequential prompts:**

**Prompt 1:** "Refactor this file...(specific patterns)"
**Prompt 2 (after merge):** "Optimize these DB queries..."
**Prompt 3 (after merge):** "Write tests for the refactored module..."

---

## Example 3: Missing Standards

### Bad

> "Create a REST API for a todo app"

**Why it fails:** REST APIs vary wildly. The agent might use:
- Express (you use Fastify)
- MongoDB (you use PostgreSQL)
- JavaScript (you use TypeScript)
- Snake_case (you use camelCase)
- No pagination (you need it)
- No auth (you need JWT)

### Good

```
Context: TypeScript 5.5, Fastify, Prisma + PostgreSQL 16
Task: Create TODO CRUD endpoints:
      - GET /todos?status=pending&page=1&limit=20
      - POST /todos { title, description, dueDate? }
      - PATCH /todos/:id { title?, description?, status? }
      - DELETE /todos/:id (soft delete)
Standards:
      - JWT auth required on all endpoints
      - Zod schemas for input validation
      - Prisma for all DB queries
      - Paginated responses: { data: [], meta: { total, page, limit } }
      - camelCase JSON keys
      - Proper HTTP status codes (200, 201, 204, 400, 401, 404)
      - Unit tests for each endpoint (Vitest)
```

**Why it works:** No ambiguity. Every dimension is locked down.

---

## Example 4: False Assumption

### Bad

> "Just like any other GraphQL resolver, implement the users query"

**Why it fails:** The agent doesn't know your existing resolver patterns, your data loaders, your auth context, or your error handling conventions. "Just like" is never sufficient context.

### Good

```
Task: Add a GraphQL query `users(search: String, limit: Int = 20): [User!]!`
Standards:
  - Follow the exact pattern in src/graphql/resolvers/orders.ts
  - Use the same DataLoader pattern (see src/graphql/dataloaders/)
  - Auth context from src/graphql/context.ts
  - Error format: { message, code, path } (see src/graphql/errors.ts)
  - Add tests to src/graphql/__tests__/users.test.ts
```

**Why it works:** Instead of hoping the agent knows, you explicitly reference the patterns to follow.

---

## Example 5: Overloading Context

### Bad

> "Here's my entire 50,000-line codebase. Fix the bug."

**Why it fails:** Context window overflow. The agent will either:
- Miss the relevant code entirely
- Forget earlier instructions
- Go into compaction mode and lose detail

### Good

```
Context: Bug in src/payment/checkout.ts line 142-178
Symptom: "Cannot read properties of undefined (reading 'price')"
         when a user applies a discount code that expired
Root cause hypothesis: The discount validator doesn't check
         expiry before applying the percentage
Task: Fix the discount validation to check expiry before
      applying. Return 400 "Discount code expired" if expired.
Reference code:
[Paste only the 36 relevant lines from checkout.ts and
 the Discount model schema]
```

**Why it works:** Focused. The agent gets only what it needs.

---

## Example 6: No Output Format

### Bad

> "Review my code"

**Why it fails:** What kind of review? Security? Style? Architecture? What format? Inline comments? Summary? Categorized?

### Good

```
Role: Senior engineer reviewing for correctness and security
Task: Review src/auth/login.ts
Standards:
  - Categorize issues as blocking / important / nitpick
  - For each issue: file:line, explanation, fix suggestion
  - Blocking = security hole or logic error
  - Important = missing edge case or poor error handling
  - Nitpick = style only
  - End with a 1-2 sentence summary and overall verdict
       (approve / changes requested)
```

**Why it works:** The agent knows what you care about (correctness + security), how to format the output, and what verdict to deliver.

---

## Example 7: Ambiguous Priority

### Bad

> "Optimize the performance if you can, but don't spend too much time on it"

**Why it fails:** "If you can" and "don't spend too much time" are not measurable. The agent can't optimize meaningfully because the bar is undefined.

### Good

```
Task: Reduce the p95 latency of GET /api/products from 800ms to
      under 200ms by adding database indexes and optimizing the
      top 3 queries (identified in the profiling data below).
Constraint: Do not change the API contract or add new
            dependencies. Spend at most 1 hour on this.
```

**Why it works:** Clear target (800ms → 200ms), clear scope (indexes + 3 queries), clear constraint (no new deps, 1 hour).

---

## Example 8: No Negative Constraints

### Bad

> "Update the user model to add a phone number field"

**Why it fails:** The agent might also:
- Reformat the whole model file
- Change the existing validation rules
- Add unrelated indexes
- Remove deprecated fields (that you still need)

### Good

```
Task: Add a `phone` field to the User model:
      - Type: string, nullable, unique
      - Validate: E.164 format on create/update
      - Add index on phone
Constraints:
      - Do NOT change any existing fields or their types
      - Do NOT change existing validation rules
      - Do NOT reformat the file
      - Do NOT remove deprecated fields
```

**Why it works:** Negative constraints are as important as positive ones. Tell the agent what NOT to do.

---

## Example 9: Asking for Architecture Without Context

### Bad

> "What database should I use for my app?"

**Why it fails:** Without context, the agent will list generic pros and cons of SQL vs NoSQL, which isn't actionable.

### Good

```
Context:
  - App: Multi-tenant SaaS analytics dashboard
  - Data: Time-series events (100M/day), user configs, billing
  - Query patterns: Aggregation queries by time range + tenant,
                    OLAP-style, low latency needed (< 1s)
  - Team: 2 backend engineers, experienced with PostgreSQL
  - Budget: $500/mo for database hosting
  - Current: Running on SQLite (hitting limits)
  - Constraints: Must support migrations from current SQLite schema

Task: Recommend a database strategy. Evaluate options, explain
      tradeoffs, and propose a migration path.
```

**Why it works:** With real constraints (team size, budget, data volume, query patterns), the agent can give a specific, actionable recommendation.

---

## Example 10: The Kitchen Sink

### Bad

> "I'm building a social media app. Can you help me with the architecture, database design, API design, frontend components, authentication, notifications, feed algorithm, content moderation, analytics dashboard, mobile app, and also write all the code? Oh and I need it done in 2 weeks."

**Why it fails:** Scope so large the agent either produces a shallow outline or hallucinates. No single prompt can handle this.

### Fixed approach — break into phases:

**Phase 1 prompt:** "Architecture for a social media app with 10k DAU. Focus on the data model and API design. Ignore frontend, mobile, and ML."
**Phase 2 (after review):** "Based on this architecture, implement the auth module..."
**Phase 3+:** Continue feature by feature.

---

## Summary: Common Failure Patterns

| Failure | Symptom | Fix |
|---------|---------|-----|
| **Vague context** | Generic tutorial code | Specify stack, versions, existing patterns |
| **Mixed tasks** | Diff that changes everything | One prompt per concern |
| **No standards** | Code that doesn't match your conventions | List quality requirements explicitly |
| **False assumptions** | Wrong approach because agent guessed | Reference specific files/patterns |
| **Overloaded context** | Agent forgets instructions | Send only relevant code |
| **No output format** | Hard-to-use response | Specify format (categorized, JSON, etc.) |
| **Ambiguous priority** | Wrong tradeoffs | Set measurable targets |
| **Missing negatives** | Unwanted side effects | Say what NOT to do |
| **Too broad** | Shallow outline | Define scope boundaries |
