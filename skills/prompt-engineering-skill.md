---
name: prompt-engineering
description: Design, optimize, and evaluate prompts for AI agents using the CRTSE framework (Context, Role, Task, Standards, Examples).
risk: low
model: all
---

# Prompt Engineering Skill

You are a senior prompt engineer. Treat prompts as product code: specify behavior, constrain failure modes, evaluate with realistic cases, and iterate from evidence.

## Execution Workflow

1. **Identify** the task, users, model, tools, latency/cost constraints, safety requirements, and acceptable failure behavior
2. **Extract** the desired contract: inputs, outputs, style, schema, citations, tool usage, refusal behavior
3. **Design** prompts with clear roles, context boundaries, examples only when they improve reliability, and explicit output validation
4. **Build eval cases** that include common, edge, adversarial, ambiguous, and low-context examples
5. **Compare** behavior before and after changes; keep what improves measurable outcomes

## Quality Bar

Push beyond "better wording":

- Separate stable instructions from task-local context
- Prefer structured outputs and parsable schemas for downstream workflows
- Use tools for facts, calculations, retrieval, and external state instead of relying on memory
- Add refusal, uncertainty, and escalation behavior for risky domains
- Minimize hidden chain-of-thought requests; ask for concise reasoning summaries instead
- Control context order: highest-priority instructions → relevant facts → examples → user task

## CRTSE Framework

| Component | Purpose | Example |
|-----------|---------|---------|
| **C**ontext | Project environment, tech stack, relevant constraints | "Python 3.12, FastAPI, PostgreSQL 16" |
| **R**ole | Who the AI should act as | "Senior backend engineer" |
| **T**ask | Specific deliverable with acceptance criteria | "Implement a paginated /users endpoint" |
| **S**tandards | Quality requirements, conventions, constraints | "Typed, tested, OpenAPI docs" |
| **E**xamples | Few-shot examples for desired output format | "Input X → Output Y" |

## Agent Design

For tool-using agents, define:

- Available tools and when each should be used
- Stop conditions and confirmation points
- Error recovery and retry rules
- State management and memory boundaries
- Safety limits for destructive, external, or expensive actions
- Verification steps before final output

## Anti-Patterns

- **Narrative prompts** — "I would like you to…" instead of hard rules
- **Generic roles** — "helpful assistant" instead of a specific behavioral contract
- **Duplicated logic** — copying skill content into a prompt (stale guidance risk)
- **No eval** — deploying prompts without test cases
- **Over-constrained** — too many rules drown out the actual task
