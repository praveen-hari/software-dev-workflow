# Phase 5 — Review

## Purpose

Quality gates before merge. Review the change across multiple dimensions to catch issues that testing alone cannot find: design problems, security vulnerabilities, performance anti-patterns, and unnecessary complexity.

## When to Enter

- Code is complete, tested, and verified (from Phase 4)
- Ready to merge or deploy

## Skills Activated

### `code-review-and-quality` (Always)

Perform a 5-axis review:

| Axis | What to Check |
|------|--------------|
| **Correctness** | Does it do what the spec says? Are edge cases handled? |
| **Readability** | Can another engineer understand this in 5 minutes? |
| **Architecture** | Does it fit the system's design? Any Hyrum's Law risks? |
| **Security** | Input validation? Auth checks? No hardcoded secrets? |
| **Performance** | Any N+1 queries? Unnecessary re-renders? Bundle size impact? |

Review standards:
- Change size: ~100 lines (split larger changes)
- Severity labels: `Nit` / `Optional` / `FYI` / `Required`
- Standard: "Would a staff engineer approve this?"

### `code-simplification` (When Complexity Is High)

Activate when code is harder to read or maintain than it should be:

- Apply Chesterton's Fence — understand why code exists before changing it
- Rule of 500 — functions under 500 lines, files under 500 lines
- Reduce complexity while preserving exact behavior
- Remove dead code, unused imports, commented-out blocks

### `security-and-hardening` (When Handling User Data)

Activate when the change handles user input, authentication, data storage, or external integrations:

- OWASP Top 10 prevention
- Input validation and sanitization
- Authentication and authorization patterns
- Secrets management (no hardcoded credentials)
- Dependency auditing (known vulnerabilities)
- Three-tier boundary system (trust / verify / reject)

### `performance-optimization` (When Performance Matters)

Activate when performance requirements exist or regressions are suspected:

- Measure-first approach — profile before optimizing
- Core Web Vitals targets (LCP, FID, CLS)
- Bundle analysis and tree-shaking verification
- Database query optimization
- Caching strategy review

## Agent Personas for Review

For thorough reviews, delegate to specialist agents:

| Agent | Role | When to Use |
|-------|------|------------|
| `code-reviewer` | Senior Staff Engineer | Every review — 5-axis quality gate |
| `security-auditor` | Security Engineer | Changes involving auth, user input, data |
| `test-engineer` | QA Specialist | Verify test coverage and strategy |

## Review Checklist

- [ ] 5-axis review completed (correctness, readability, architecture, security, performance)
- [ ] No `Required` severity issues remain unresolved
- [ ] Security review passed for changes handling user data
- [ ] No unnecessary complexity — code is as simple as it can be
- [ ] Change is scoped — no unrelated modifications snuck in
- [ ] Documentation updated for public interfaces

## Exit Criteria

- [ ] All review findings addressed
- [ ] No blocking issues remain
- [ ] Human has reviewed and approved

## Transition

→ Proceed to **Phase 6 — Ship** (read `phase-ship.md`)
