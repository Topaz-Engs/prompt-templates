---
framework: CRTSE
model: all
---

# CI/CD Pipeline Design Prompt

## Context
- **Platform:** [GitHub Actions / GitLab CI / CircleCI / Jenkins / Buildkite]
- **Language/Runtime:** [e.g., Node 22, Python 3.12, Go 1.22, Rust 1.80]
- **Hosting/Deploy target:** [e.g., AWS ECS, Vercel, Railway, Docker, Kubernetes]
- **Monorepo?** [yes/no — if yes, which tool: Turborepo, Nx, Lerna]

## Role

Act as a DevOps engineer designing a production-grade CI/CD pipeline. Prioritize speed, reliability, and developer experience.

## Task

Design a CI/CD pipeline configuration for the described project. Include stages for linting, type-checking, testing, building, and deploying.

## Standards

| Stage | Requirements |
|-------|-------------|
| **Lint** | Run within 30s; fail fast on style/code quality issues |
| **Type-check** | Full type verification; cache node_modules between runs |
| **Test** | Unit tests (parallel), integration tests (sequential), e2e (optional, on demand) |
| **Build** | Cache dependencies and build artifacts; produce SBOM |
| **Deploy** | Only on main/master; require all previous stages to pass; support rollback |
| **Security** | Scan dependencies for CVEs; check for secrets in code; SAST scanning on PRs |

## Deliverable

- Complete CI/CD configuration file (or files)
- Explanation of caching strategy
- Estimated pipeline time breakdown
- Rollback procedure

## Additional Notes

[optional: specific tooling preferences, self-hosted runners, approval gates, etc.]
