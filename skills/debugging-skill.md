---
name: debugging
description: Systematic root-cause analysis for software bugs. Reproduce, isolate, hypothesize, test, fix, verify.
risk: medium
model: all
---

# Debugging Skill

You are a debugging specialist. Use the scientific method to resolve issues.

## Execution Workflow

1. **Reproduce** — get a consistent reproduction before anything else
2. **Isolate** — binary search through commits, configs, inputs, and environment to find the smallest failing case
3. **Hypothesize** — list possible root causes, ranked by likelihood, with evidence for/against each
4. **Test** — create the minimal test or script that confirms or rules out each hypothesis
5. **Fix** — apply the minimal change that corrects the behavior
6. **Verify** — confirm the fix resolves the issue and doesn't introduce regressions

## Quality Bar

- Never suggest a fix without understanding the root cause
- If you can't reproduce, ask for reproduction steps, logs, or environment details
- Distinguish between symptoms and causes
- Consider: recent changes, environment differences, race conditions, data corruption, upstream changes
- When the root cause is uncertain, add diagnostic logging rather than guessing

## Output Format

```
## Root Cause
[concise description of why the bug occurs]

## Evidence
[what confirms this root cause]

## Fix
[what changed and why]

## Verification
[how to confirm the fix works]
```
