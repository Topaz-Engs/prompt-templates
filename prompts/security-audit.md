---
framework: CRTSE
model: claude, gpt
---

# Security Audit Prompt

## Context
- **Scope:** [codebase path / specific components]
- **Threat model:** [external attacker / insider / supply chain]
- **Compliance:** [SOC2, HIPAA, PCI-DSS, GDPR, none]

## Role

Act as an application security engineer performing a focused security review. Assume an attacker with no privileged access unless specified otherwise.

## Task

Audit the provided surface area for security vulnerabilities. For each finding, provide:

1. **Vulnerability class** (e.g., SQL injection, XSS, SSRF, IDOR, path traversal)
2. **Location** — file and line
3. **Impact** — what an attacker could achieve
4. **Likelihood** — low / medium / high
5. **Remediation** — specific code change or pattern to apply

## Standards

Check against the OWASP Top 10 (2021) at minimum:

| Category | What to check |
|----------|--------------|
| **Broken Access Control** | Authorization on every endpoint, IDOR, privilege escalation |
| **Cryptographic Failures** | Hardcoded secrets, weak hashing, HTTP vs HTTPS, missing encryption at rest |
| **Injection** | SQL, NoSQL, OS command, template injection, eval usage |
| **Insecure Design** | Missing rate limits, missing input validation, trust of user-controlled data |
| **Security Misconfiguration** | Debug endpoints, CORS, default credentials, verbose errors |
| **Vulnerable Components** | Outdated dependencies with known CVEs |
| **Auth Failures** | Session management, JWT handling, password policies, MFA |
| **Data Integrity** | Deserialization attacks, unsigned updates, missing CSRF |
| **Logging & Monitoring** | Missing audit trails, log injection |
| **SSRF** | User-controlled URLs fetched server-side |

## Code / Surface Area

```[language]
[paste code or describe endpoints/modules to audit]
```

## Deliverable

- Prioritized list of findings (critical → low)
- For each: location, impact, likelihood, remediation
- Summary assessment: pass / marginal / fail
