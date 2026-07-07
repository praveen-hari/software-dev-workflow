---
name: software-dev-workflow
description: >
  Lightweight development workflow that uses OpenSpec as the orchestrator.
  OpenSpec handles the lifecycle (explore → propose → apply → sync → archive).
  This skill layers engineering discipline on top during implementation.
  Use when building features, fixing bugs, or starting a new project.
  Use when you need to route a task to the right OpenSpec skill or enforce
  TDD, code review, design system, or UI Builder checks.
---

# Software Development Workflow

OpenSpec is the orchestrator. This skill adds engineering discipline during implementation.

## Prerequisites

- **OpenSpec CLI** must be installed (`npm install -g @fission-ai/openspec@latest`)
- **OpenSpec must be initialized** in the project (`openspec init --tools github-copilot`) — this creates the `openspec/` directory and installs the OpenSpec skills (`openspec-explore`, `openspec-propose`, `openspec-apply-change`, `openspec-sync-specs`, `openspec-archive-change`)
- Those skills contain the detailed CLI instructions. This skill just routes to them.

## How It Works

```
  THINK              PLAN               BUILD + SHIP
 ┌──────────┐      ┌──────────┐      ┌──────────────────────┐
 │ /explore │─────▶│ /propose │─────▶│ /apply               │
 │          │      │          │      │  + engineering skills │
 └──────────┘      └──────────┘      └──────┬───────────────┘
                                            │
                                     ┌──────▼───────┐
                                     │ /sync + /archive │
                                     └──────────────┘
```

OpenSpec skills handle the what and when. This skill handles the how during `/apply`.

## Routing

When a task arrives, route based on what exists:

| Situation | Action |
|-----------|--------|
| Vague idea, unclear requirements | → `openspec-explore` |
| Ready to plan a change | → `openspec-propose` (creates proposal + design + specs + tasks) |
| Change exists, ready to build | → `openspec-apply-change` + engineering skills (this is where this workflow adds value) |
| Implementation done | → `openspec-sync-specs` then `openspec-archive-change` |
| Trivial change (single file, obvious fix) | → Just do it. No ceremony needed. |

## Gates (Run in Order Before Implementation)

Before writing any code, run through these gates top-to-bottom. Each gate must pass before moving to the next. Do not skip any.

### Gate 1 — Spec exists (non-trivial work only)

- Does an OpenSpec change exist for this work? (`openspec list --json`)
- If NO and the work is non-trivial → STOP. Run `openspec-propose` first.
- If YES or work is trivial → proceed.

### Gate 2 — Design system (UI work only)

- Does the task involve UI/frontend code?
- If YES → does `DESIGN.md` exist at the project root?
  - If NO → STOP. Activate the `design-system` skill. Do not write UI code without it.
  - If YES → `tokens.css` must also exist. All UI code MUST use its CSS variables. No hardcoded colors, fonts, spacing, or radii.
- If NO UI work → skip this gate.

### Gate 3 — UI Builder (UI work only)

- Does the task involve building pages, dashboards, or forms?
- If YES → detect stack from `package.json` or `*.csproj`:

  | Signal | Framework | Package |
  |--------|-----------|---------|
  | `"react"` in package.json | React | `syncfusion/react-ui-builder` |
  | `"@angular/core"` in package.json | Angular | `syncfusion/angular-ui-builder` |
  | `Blazor` in *.csproj | Blazor | `syncfusion/blazor-ui-builder` |
  | `Maui` in *.csproj | .NET MAUI | `syncfusion/maui-ui-builder` |

  - **If Syncfusion UI Builder is installed** → MUST activate it. Do not write UI manually.
  - **If not installed** → STOP. Tell the user: `apm install syncfusion/<framework>-ui-builder -t <target>`. Only proceed manually if the user explicitly declines.
  - **If no Syncfusion stack** → build UI manually with standard frontend practices.
- If NO UI work → skip this gate.

### Gate 4 — TDD (always)

- Write a failing test first, then implement to make it pass. No exceptions.
- Red → Green → Refactor per slice.

### Gate 5 — Incremental slices (always)

- Build in vertical slices, not horizontal layers.
- Each slice is a working, testable unit. Keep changes small (~100 lines).

### Gate 6 — Code review (before merge)

- Every change gets reviewed before merge. No exceptions.
- Check: correctness, readability, security, performance.

### Gate 7 — Atomic commits (on ship)

- Clean, atomic commits with descriptive messages.
- One logical change per commit. No mixing unrelated changes.

## Behaviors

- Surface assumptions before proceeding
- Stop on inconsistencies — name the confusion, wait for resolution
- Prefer the boring, obvious solution
- Stay in scope — no unsolicited renovation
- Verify at runtime — "seems right" is never sufficient
