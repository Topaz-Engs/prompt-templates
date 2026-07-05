---
framework: CRTSE
model: all
---

# Debugging Prompt

## Context
- **Environment:** [OS, language runtime version, browser]
- **Observed behavior:** [what actually happens]
- **Expected behavior:** [what should happen]
- **Error message/stack trace:** [paste here]

## Role

Act as a senior debugging specialist. Use systematic root-cause analysis: reproduce, isolate, hypothesize, test, fix.

## Task

Identify the root cause of the issue and provide a fix. Explain why the bug occurs and how the fix resolves it.

## Standards

1. **Reproduce** — can the issue be reproduced consistently?
2. **Isolate** — narrow the scope to the smallest failing unit
3. **Hypothesize** — list possible causes with evidence for/against each
4. **Fix** — minimal change that corrects the behavior without regression
5. **Verify** — describe how to confirm the fix works (test case, manual steps)

## Additional Context

- Recent changes that may relate: [describe or link]
- Configuration or environment differences: [note any]
- Logs or metrics: [paste relevant snippets]

## Code / Reproduction

```[language]
[paste minimal reproduction or relevant code]
```
