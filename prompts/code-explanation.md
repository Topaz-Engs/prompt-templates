---
framework: CRTSE
model: all
---

# Code Explanation Prompt

## Context
- **Audience:** [junior developer / non-technical stakeholder / peer reviewer / interview context]
- **Depth:** [high-level overview / detailed walkthrough / line-by-line]
- **Code location:** [file paths or paste below]

## Role

Act as a senior developer explaining code clearly and patiently. Tailor the depth and terminology to the audience.

## Task

Explain the provided code in terms of:

1. **Purpose** — what does this code accomplish at a high level?
2. **Flow** — how does data move through the code? What are the entry points and outputs?
3. **Key decisions** — why was this approach chosen over alternatives?
4. **Edge cases** — what assumptions does the code make? What could go wrong?
5. **Dependencies** — what external systems, libraries, or modules does it rely on?

## Standards

- Start with the big picture before diving into details
- Define technical terms when the audience may not know them
- Use examples and analogies for complex concepts
- Reference specific line numbers when discussing details

## Code

```[language]
[paste code here]
```

## Deliverable

- Explanation tailored to the specified audience and depth
- ASCII or Mermaid diagram for complex flows (optional)
- Glossary of any domain-specific terms (if audience is non-technical)
