# Prompt Engineering Best Practices

## Overview

Prompt engineering is the practice of crafting instructions that guide AI models to generate accurate, reliable, and useful outputs. It is not about "magic words" — it is about structured thinking and pattern steering.

## The CRTSE Framework

The most effective prompts follow a consistent structure:

```
Context — Role — Task — Standards — Examples
```

### Context
Tell the AI about its environment:
- Tech stack (language, framework, libraries, versions)
- Project type (API, frontend app, CLI tool, microservice)
- Current architecture patterns
- Relevant files or directories

### Role
Define who the AI should act as:
- "Act as a senior TypeScript developer"
- "You are a DevOps engineer specializing in Kubernetes"
- "You are a security engineer performing a code audit"

This is not role-play — it genuinely shifts the model toward higher-quality output patterns.

### Task
State exactly what you need:
- Specific deliverable with acceptance criteria
- One task per prompt for best results
- Boundaries: what is in scope and what is out of scope

### Standards
Define quality requirements:
- Type safety expectations
- Error handling patterns
- Code style and conventions
- Testing requirements
- Performance constraints

### Examples
Provide input/output pairs when the output format matters:
```
Input:  "user@example.com"
Output: { "valid": true, "provider": "example.com" }

Input:  "not-an-email"
Output: { "valid": false, "reason": "missing @" }
```

## Prompt Design Principles

### 1. Be Explicit, Not Ambiguous

| Instead of | Write |
|------------|-------|
| "Fix this bug" | "Refactor `parseUser` to handle empty strings gracefully and return `null` instead of throwing" |
| "Optimize this" | "Reduce the time complexity of `findDuplicates` from O(n^2) to O(n)" |
| "Add error handling" | "Wrap the database call in a try/catch, log the error with `logger.error`, and return a 503 response" |

### 2. Provide Sufficient Context

The more relevant context the agent has, the better its output will match your intent:

- Paste relevant code snippets
- Reference specific file names and line numbers
- State the programming language and framework
- Mention architectural patterns already in use
- Link to similar existing implementations

### 3. Control the Output Format

When you need structured output, specify it explicitly:

- **JSON:** "Return a JSON object with `id`, `name`, and `createdAt` fields"
- **Markdown:** "Format as a Markdown table with columns: Parameter, Type, Required, Default"
- **Code blocks:** "Show the TypeScript implementation in a code block with type annotations"

### 4. Set Boundaries

Tell the AI what NOT to do as clearly as what to do:

- "Do not modify any files outside `src/features/auth/`"
- "Do not add new dependencies without approval"
- "Do not change the public API of existing functions"
- "Do not use `any` — all types must be explicit"

### 5. Iterate and Refine

Treat prompting like pair programming:

1. First attempt → review output
2. Identify gaps or misalignments
3. Refine the prompt with more specific instructions
4. Repeat until the output meets quality standards

## Advanced Techniques

### Chain-of-Thought Prompting

For complex reasoning tasks, ask the agent to show its work:

> "Think through this step by step before giving the final answer. For each step, state what you're checking and why."

### Few-Shot Prompting

Provide examples of desired input/output pairs:

> "Here are examples of the format I want:
> Example 1: ...
> Example 2: ...
> Now handle this input:"

### Persona Prompting

Assign a specific role to anchor the model's behavior:

> "You are a staff engineer at a company that deploys 100+ times a day. You value simplicity, testability, and incremental delivery over perfect design."

### Negative Prompting

State what to avoid:

> "Do not suggest:
> - Microservices for small projects
> - Adding dependencies for trivial tasks
> - Rewriting existing code that isn't broken"

## Common Pitfalls

### Vagueness
"Optimize this" or "Improve quality" are too vague. Be specific about what dimension to improve and by what measure.

### Information Overload
Too much context can be as harmful as too little. Include only what's relevant to the task. Irrelevant details dilute the signal.

### Assuming Shared Knowledge
Don't assume the AI knows your project conventions, team preferences, or past decisions. Explicitly state them.

### One-Shot Expectations
The first result is rarely perfect. Plan for 2-3 iterations per task. Each iteration sharpens the output.

### Ignoring Model Limitations
- AI models may not correctly handle very large codebases
- Novel libraries or frameworks may have limited training data
- Math and counting are unreliable without tools (calculator, code execution)

## Evaluation

Before deploying prompts in production, evaluate them:

| Criterion | What to check |
|-----------|---------------|
| **Accuracy** | Does the output match the expected behavior? |
| **Consistency** | Does the same prompt produce similar outputs each time? |
| **Robustness** | Does it handle edge cases and unexpected inputs? |
| **Efficiency** | Is the prompt concise enough to fit in the context window? |
| **Safety** | Does it refuse harmful instructions and avoid dangerous outputs? |

### Building eval cases

Cover these categories:
- **Happy path** — typical, expected inputs
- **Edge cases** — boundary values, empty inputs, nulls
- **Adversarial** — prompt injection attempts, out-of-scope requests
- **Ambiguous** — underspecified inputs that should trigger clarification
- **Low-context** — requests with minimal information

## Checking Pass/Fail

Set clear thresholds:
- **Pass:** All happy path and edge case tests pass
- **Marginal:** Happy path passes, some edge cases fail
- **Fail:** Happy path fails or safety checks are breached
