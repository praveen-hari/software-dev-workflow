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
2. **Read before write** — on brownfield projects, read existing code patterns, folder structure, naming conventions, and component library before writing anything. Match what exists.
3. **Test-driven** — failing test first, then make it pass
4. **Incremental slices** — vertical slices, not horizontal layers
5. **Review before merge** — no exceptions
6. **Verify at runtime** — "seems right" is never sufficient
7. **Stay in scope** — touch only what you're asked to touch

## UI Work (Non-Negotiable)

Before ANY UI implementation, complete these checks in order:

1. **Design system** — if `DESIGN.md` doesn't exist → STOP. On brownfield projects, extract existing design into `DESIGN.md` first. On greenfield, create from scratch via `design-system` skill.
2. **Component library** — if project uses Syncfusion, check if component skills are installed. If not → suggest `npx skills add syncfusion/<framework>-ui-components-skills -y`. If project uses a different library (Material UI, Ant Design, etc.) → follow that library's patterns.
3. **Tokens only** — all UI code uses `tokens.css` variables. No hardcoded colors, fonts, or spacing.
