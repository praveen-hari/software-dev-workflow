---
description: >
  Software development workflow rules. Always active. Enforces engineering
  discipline across all tasks: spec before code, test-driven development,
  code review before merge, security hardening, and incremental delivery.
  Use when working on any software development task.
applyTo: "**"
---

# Software Development Workflow Rules

These rules apply to ALL work in this project. They are non-negotiable.

## Task Classification (Always Do First)

Before starting any work, classify the task:

- **New feature / project** → Full lifecycle: `/spec` → `/plan` → `/build` → `/test` → `/review` → `/ship`
- **Feature with clear requirements** → Start at `/plan`
- **Bug fix** → `debugging-and-error-recovery` → TDD → review → ship
- **Refactor** → `code-simplification` → TDD → review → ship
- **Hotfix** → Debug → fix → test → ship (fast track)
- **Trivial change** → Just do it with TDD + commit

## Non-Negotiable Rules

1. **Spec before code** — Non-trivial work without a spec is guessing
2. **Test-driven development** — Write the failing test first, then make it pass
3. **Incremental delivery** — Build in vertical slices, ~100 lines per change
4. **Review before merge** — Every change gets reviewed, no exceptions
5. **Verify at runtime** — "Seems right" is never sufficient
6. **Surface assumptions** — State what you're assuming before proceeding
7. **Stay in scope** — Touch only what you're asked to touch

## UI Work Routing

When the task involves building UI pages, dashboards, or forms:

- Detect the project stack from project files
- Route to the correct Syncfusion UI Builder agent
- After UI generation: write tests, review code, then commit

## Quality Bar

Every change must satisfy the Definition of Done:

- All acceptance criteria met
- Tests written and passing
- No regressions
- No dead code or debug output
- Public interfaces documented
- Security reviewed (if handling user data)
- Human approved
