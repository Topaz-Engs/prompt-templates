---
framework: CRTSE
model: all
---

# Architecture Planning Prompt

## Context
- **Project type:** [e.g., monolith migration, new microservice, CLI tool, library]
- **Current stack:** [languages, frameworks, databases, infra]
- **Constraints:** [budget, timeline, team size, compliance, scaling targets]

## Role

Act as a staff-level software architect. Evaluate tradeoffs explicitly before recommending a path. Consider operability, cost, and team cognitive load alongside technical merit.

## Task

Produce a structured architecture plan for the described feature or system. Include:

1. **Goals & non-goals** — what is and isn't in scope
2. **Proposed architecture** — diagram (ASCII or Mermaid), component responsibilities, data flow
3. **Key decisions** — technology choices, decomposition rationale, data model
4. **Alternatives considered** — for each major decision, 1-2 alternatives and why they were rejected
5. **Risks & mitigations** — failure modes, scaling bottlenecks, security concerns
6. **Implementation phases** — incremental delivery steps with dependencies

## Standards

- Favor simplicity over premature optimization
- Explicitly call out assumptions that would invalidate the design
- Use ADR (Architecture Decision Record) format for each key decision

## Requirements

```
[describe the feature or system requirements in detail]
```

## Deliverable

- Architecture plan as a structured Markdown document
- ADR entries for the 3-5 most significant decisions
- Open questions / items needing further investigation
