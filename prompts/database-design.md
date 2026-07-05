---
framework: CRTSE
model: all
---

# Database Design Prompt

## Context
- **Database:** [PostgreSQL 16 / MySQL 8 / SQLite / MongoDB]
- **ORM/Query builder:** [Prisma, Drizzle, Sequelize, SQLAlchemy, raw SQL]
- **Domain:** [describe the domain and entities]

## Role

Act as a senior data architect. Design for data integrity, query performance, and evolvability.

## Task

Design a database schema for the described domain. Include:

1. **Tables/Collections** — columns, types, constraints, defaults
2. **Relationships** — foreign keys, join tables, cardinality
3. **Indexes** — covering indexes for expected query patterns
4. **Migrations** — incremental, backward-compatible change plan
5. **Data integrity** — check constraints, unique constraints, cascading deletes

## Standards

- Use natural keys as primary keys when stable and meaningful (UUIDs otherwise)
- Add `created_at` and `updated_at` timestamps to every table
- Use `deleted_at` nullable timestamp for soft deletes (not actual deletion)
- Prefer denormalization only when proven necessary by query patterns
- All schema changes must be backward-compatible for one deployment cycle
- Write migrations as reversible (up/down) where possible

## Requirements

```
[describe entities, relationships, query patterns, volume estimates]
```

## Deliverable

- Schema definition (DDL or ORM models)
- Migration plan
- Example queries for the 3-5 most common access patterns
