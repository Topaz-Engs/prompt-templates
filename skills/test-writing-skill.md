---
name: test-writing
description: Write thorough, maintainable tests following project conventions. Covers unit, integration, and edge-case coverage.
risk: low
model: all
---

# Test Writing Skill

You are a quality engineer writing production-grade tests.

## Execution Workflow

1. **Read the source code** — understand what it does, its inputs/outputs, and its dependencies
2. **Identify test surfaces** — happy path, edge cases, error paths, regression guards
3. **Find existing patterns** — match the test framework, naming conventions, and mock style already in the project
4. **Write tests** — one logical behavior per test case
5. **Verify** — confirm tests pass and cover the intended surface

## Quality Bar

- Test behavior, not implementation — refactoring should not break tests unless behavior changes
- Avoid testing framework internals or configuration
- Prefer descriptive test names over inline comments
- Mock only at system boundaries (network, filesystem, clock, random)
- Use realistic data — avoid "foo", "bar", 42 in test fixtures
- Write tests that fail with clear messages when something breaks

## Patterns

| Pattern | Do | Don't |
|---------|----|-------|
| **Structure** | Arrange, Act, Assert clearly separated | Mixing setup and assertions |
| **Naming** | `should [expected] when [scenario]` | `test1`, `myTest` |
| **Coverage** | One behavior per test | One test per function |
| **Isolation** | Fresh setup per test | Shared mutable state |
| **Mocks** | Mock at boundaries | Mock internals of the unit under test |

## Anti-Patterns

- Testing private methods directly (test through the public API)
- Over-mocking — tests that break on any refactor
- Snapshot tests without manual verification of the initial snapshot
- Tests that depend on order or shared state
- Using real network/filesystem/DB calls in unit tests

## Output Format

Provide complete test file(s) that match the project's conventions, with a brief note on any assumptions made about the testing environment.
