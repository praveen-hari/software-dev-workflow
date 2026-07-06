---
description: "Review before merge. Starts the Review phase: performs 5-axis code review (correctness, readability, architecture, security, performance). Delegates to specialist agents for security audit and test coverage analysis. Use before merging any change."
argument-hint: "Specify files or changes to review, or leave blank for full review"
---

# /review — Review Before Merge

Execute **Phase 5 (Review)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-review.md`
2. Activate `code-review-and-quality` — perform 5-axis review:
   - **Correctness**: Does it match the spec? Edge cases handled?
   - **Readability**: Understandable in 5 minutes?
   - **Architecture**: Fits the system design?
   - **Security**: Input validation? No hardcoded secrets?
   - **Performance**: No N+1 queries? No unnecessary re-renders?
3. If code is complex, activate `code-simplification`
4. If handling user data, delegate to `security-auditor` agent
5. If performance matters, activate `performance-optimization`
6. Delegate to `test-engineer` agent for coverage analysis
7. Use severity labels: `Nit` / `Optional` / `FYI` / `Required`
8. All `Required` issues must be resolved before approval

## Standard

"Would a staff engineer approve this?"

## Output

Review report with findings, severity labels, and recommendations.
