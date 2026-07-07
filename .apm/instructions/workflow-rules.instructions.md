---
description: >
  Core development rules. Always active. Enforces engineering discipline:
  OpenSpec for planning, TDD, code review, incremental delivery.
  CRITICAL: Before writing ANY code, output the pre-flight checklist.
applyTo: "**"
---

# Development Rules

## ⚠️ MANDATORY PRE-FLIGHT — Output This Before Writing Any Code

Before writing ANY code (not just UI), output this checklist and confirm each item. Do not skip this step. Do not start coding without it.

```
PRE-FLIGHT CHECK:
□ Task type: [feature / bug / refactor / trivial]
□ OpenSpec change exists: [yes / no / not needed (trivial)]
□ Brownfield project: [yes / no] → if yes, read existing patterns first
□ UI work involved: [yes / no]
  □ DESIGN.md exists: [yes / no / extracting from existing]
  □ tokens.css exists: [yes / no]
  □ Component library: [Syncfusion / Material UI / Ant Design / custom / none]
  □ Component skills installed: [yes / no / not applicable]
→ All checks passed. Proceeding.
```

If any check fails, STOP and resolve it before writing code.

## Workflow

OpenSpec is the orchestrator. Use it for non-trivial work:

- **Think** → `openspec-explore`
- **Plan** → `openspec-propose` (creates proposal + design + specs + tasks)
- **Build** → `openspec-apply-change` (implements tasks with engineering discipline)
- **Finish** → `openspec-sync-specs` then `openspec-archive-change`
- **Trivial changes** → skip the ceremony, just do it

## Non-Negotiable

1. **Pre-flight checklist** — output the checklist above before ANY code. No exceptions.
2. **Spec before code** — non-trivial work without a spec is guessing
3. **Read before write** — on brownfield projects, read existing code patterns, folder structure, naming conventions, and component library before writing anything. Match what exists.
4. **Test-driven** — failing test first, then make it pass
5. **Incremental slices** — vertical slices, not horizontal layers
6. **Review before merge** — no exceptions
7. **Verify at runtime** — "seems right" is never sufficient
8. **Stay in scope** — touch only what you're asked to touch

## UI Work (Non-Negotiable)

Before ANY UI implementation, complete these checks in order:

1. **Design system** — if `DESIGN.md` doesn't exist → ❌ STOP. On brownfield projects, extract existing design into `DESIGN.md` first. On greenfield, create from scratch via `design-system` skill.
2. **Component library** — if project uses Syncfusion, check if component skills are installed. If not → ❌ STOP and run `npx skills add syncfusion/<framework>-ui-components-skills -y`. If project uses a different library (Material UI, Ant Design, etc.) → follow that library's patterns.
3. **Tokens only** — all UI code uses `tokens.css` variables. No hardcoded colors, fonts, or spacing.
