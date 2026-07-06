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

- **New feature / project** → Full lifecycle: `/spec` → `/plan` → `/design` → `/build` → `/test` → `/review` → `/ship`
  - `/spec` uses OpenSpec (`/opsx:explore` → `/opsx:propose`) to create a change folder
  - `/plan` refines `openspec/changes/<name>/tasks.md`
  - `/design` finalizes DESIGN.md with color system, typography, spacing tokens (required before UI code)
  - `/build` reads tasks from the OpenSpec change folder, consumes tokens.css for all UI work
- **Feature with clear requirements** → Start at `/plan` (or `/opsx:propose` directly)
- **UI-only change** → `/design` → `/build` → `/test` → `/review` → `/ship`
- **Bug fix** → `debugging-and-error-recovery` → TDD → review → ship
- **Refactor** → `code-simplification` → TDD → review → ship
- **Hotfix** → Debug → fix → test → ship (fast track)
- **Trivial change** → Just do it with TDD + commit

## Artifact Storage (OpenSpec)

All specs, proposals, designs, and tasks are stored in OpenSpec change folders:

```
openspec/changes/<feature-name>/
├── proposal.md     ← Why + scope (from /spec)
├── specs/          ← Delta specs (behavior changes)
├── design.md       ← Technical approach
└── tasks.md        ← Implementation checklist (from /plan)
```

When a feature ships, run `/opsx:archive` to merge delta specs into `openspec/specs/`
and move the change folder to `openspec/changes/archive/`.

## Design System Rules (UI Work)

When the project involves frontend UI:

- **MUST activate `design-system` skill** — Run `/design` which activates the `design-system` skill. Never create `DESIGN.md` manually or skip this skill.
- **Design before UI code** — `DESIGN.md` + `tokens.css` must exist at project root and be approved before any UI implementation
- **No hardcoded values** — All colors, fonts, spacing, and radii must use CSS variables from `tokens.css`
- **Accent = interactive only** — The accent color is reserved for buttons, links, focus rings, and toggles
- **WCAG contrast required** — Text on background ≥ 4.5:1, interactive elements ≥ 3:1

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
- Check if the matching Syncfusion UI Builder is installed (it is a per-project dependency, not bundled)
- If installed: **MUST activate the Syncfusion UI Builder agent** for all UI page/dashboard/form generation
- If NOT installed: inform the user with the install command, then fall back to `incremental-implementation` + `frontend-ui-engineering`
- After UI generation: write tests, review code, then commit

UI Builder install commands:
- `apm install syncfusion/react-ui-builder -t <target>`
- `apm install syncfusion/angular-ui-builder -t <target>`
- `apm install syncfusion/blazor-ui-builder -t <target>`
- `apm install syncfusion/maui-ui-builder -t <target>`
- `apm install syncfusion/wpf-ui-builder -t <target>`
- `apm install syncfusion/winforms-ui-builder -t <target>`
- `apm install syncfusion/winui-ui-builder -t <target>`

## Quality Bar

Every change must satisfy the Definition of Done:

- All acceptance criteria met
- Tests written and passing
- No regressions
- No dead code or debug output
- Public interfaces documented
- Security reviewed (if handling user data)
- Human approved
