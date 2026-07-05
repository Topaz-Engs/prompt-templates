---
framework: CRTSE
model: all
---

# API Design Prompt

## Context
- **Protocol:** [REST / GraphQL / gRPC / tRPC]
- **Language/Framework:** [e.g., FastAPI, Express, Next.js, Spring Boot]
- **Domain:** [e.g., user management, billing, search]
- **Existing patterns:** [link to existing API examples in the codebase]

## Role

Act as a senior API designer. Follow RESTful best practices, prioritize consistency, and design for evolvability.

## Task

Design an API for the described domain. Provide OpenAPI/Swagger-compatible definitions.

## Standards

- **Naming:** Plural nouns for resources (`/users`, not `/getUser`), kebab-case for multi-word
- **Versioning:** URL or header based from day one (`/v1/users`)
- **Status codes:** Use appropriate HTTP semantics (200, 201, 204, 400, 401, 403, 404, 409, 422, 500)
- **Error format:** RFC 9457 Problem Details (`{ "type", "title", "status", "detail", "trace_id" }`)
- **Idempotency:** PUT and DELETE are idempotent; POST is not unless designed with idempotency keys
- **Pagination:** Cursor-based for lists, with `next_cursor` and `has_more` fields
- **Rate limiting:** Return `429 Too Many Requests` with `Retry-After` header

## Requirements

```
[describe the API requirements — resources, actions, relationships, constraints]
```

## Deliverable

- OpenAPI 3.x spec or typed schema definitions
- Key design decisions with rationale
- Request/response examples for each endpoint
