# Best Practices for Working with AI Coding Agents

## Writing Effective Prompts

- **Be specific** — state the task, constraints, and expected output format
- **Provide context** — share relevant files, architecture decisions, and conventions
- **One task at a time** — break complex work into sequential steps
- **Specify boundaries** — what should and shouldn't be changed

## Iteration Loop

1. Review the agent's output before applying
2. Ask for explanations when something is unclear
3. Provide corrective feedback for next attempts
4. Commit often to track progress and roll back if needed

## Project Setup

- Add an `AGENTS.md` file with project conventions, tech stack, and common commands
- Define lint/typecheck/test commands so the agent can verify its work
- Use `.opencode/` or similar for agent-specific configuration
