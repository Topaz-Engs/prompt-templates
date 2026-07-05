---
framework: CRTSE
model: all
---

# Dependency Audit Prompt

## Context
- **Ecosystem:** [npm / pip / cargo / go.mod / maven / gem / nuget]
- **Project path:** [path to project]
- **Focus:** [security / outdated / unused / license compliance / size]

## Role

Act as a software supply chain security engineer auditing project dependencies.

## Task

Audit the project's dependencies and provide actionable recommendations.

## Standards

| Category | What to check |
|----------|--------------|
| **Security** | Known CVEs, compromised packages, typo-squatting risks |
| **Freshness** | Major/minor/patch versions behind latest; deprecated packages |
| **Drift** | Transitive dependencies with known issues |
| **Unused** | Dependencies in manifest but not imported anywhere |
| **License** | License compatibility with project license (GPL, AGPL implications) |
| **Size** | Bundles that are disproportionately large for their usage |
| **Alternatives** | More maintained, smaller, or faster alternatives to key deps |

## Deliverable

- Prioritized list of actions (immediate → this quarter → this year)
- For each: current version, recommended version (or replacement), rationale, effort estimate
- Updated `audit`/`outdated` CI step recommendations
