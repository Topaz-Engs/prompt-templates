---
framework: CRTSE
model: all
---

# Performance Optimization Prompt

## Context
- **System type:** [API server / frontend SPA / CLI tool / data pipeline / mobile app]
- **Measured bottleneck:** [CPU / memory / I/O / network / DB queries / rendering]
- **Current metrics:** [p50/p99 latency, throughput, memory usage, bundle size, FCP, LCP]
- **Target metrics:** [desired values after optimization]

## Role

Act as a performance engineer. Profile first, then optimize. Measure before and after every change.

## Task

Identify and resolve performance bottlenecks in the provided code or system. For each optimization, provide:

1. **Baseline measurement** — current metric value
2. **Root cause** — why the bottleneck exists
3. **Proposed change** — specific code or configuration change
4. **Expected improvement** — estimated impact
5. **Verification** — how to measure the improvement

## Standards

- Profile before optimizing — never guess where the bottleneck is
- One optimization at a time — measure each change independently
- Favor algorithmic improvements over micro-optimizations
- Document any tradeoffs (readability, memory usage, complexity)
- Keep a performance budget and enforce it in CI

## Common Areas to Investigate

| Area | Tools/Techniques |
|------|-----------------|
| **Database** | `EXPLAIN ANALYZE`, missing indexes, N+1 queries, connection pooling |
| **API** | Response pagination, compression, caching headers, CDN |
| **Frontend** | Bundle analysis, code splitting, lazy loading, memoization |
| **Compute** | Algorithm complexity, caching, memoization, worker threads |
| **I/O** | Batch operations, streaming, buffering, async concurrency |

## Code / Profile Data

```[language]
[paste relevant code or profiling output]
```

## Deliverable

- Prioritized list of optimizations (highest impact first)
- Before/after measurements for each change
- Updated performance budget
