---
description: >
  Core development rules. Always active. Enforces engineering discipline:
  OpenSpec for planning, TDD, code review, incremental delivery.
applyTo: "**"
---

# Development Rules

## Workflow

OpenSpec is the orchestrator. Use it for non-trivial work:

- **Think** → `openspec-explore`
- **Plan** → `openspec-propose` (creates proposal + design + specs + tasks)
- **Build** → `openspec-apply-change` (implements tasks with engineering discipline)
- **Finish** → `openspec-sync-specs` then `openspec-archive-change`
- **Trivial changes** → skip the ceremony, just do it

## Non-Negotiable

1. **Spec before code** — non-trivial work without a spec is guessing
2. **Test-driven** — failing test first, then make it pass
3. **Incremental slices** — vertical slices, not horizontal layers
4. **Review before merge** — no exceptions
5. **Verify at runtime** — "seems right" is never sufficient
6. **Stay in scope** — touch only what you're asked to touch

## UI Work

- If `DESIGN.md` doesn't exist, activate `design-system` skill before writing UI code
- All UI code uses `tokens.css` variables — no hardcoded colors, fonts, or spacing
- If a Syncfusion UI Builder is installed for the project's stack, use it for page/form generation
