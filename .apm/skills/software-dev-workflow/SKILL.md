---
name: software-dev-workflow
description: >
  Lightweight development workflow that uses OpenSpec as the orchestrator.
  OpenSpec handles the lifecycle (explore → propose → apply → sync → archive).
  This skill layers engineering discipline on top during implementation.
  Use when starting a new project, building features, or shipping to production.
---

# Software Development Workflow

OpenSpec is the orchestrator. This skill adds engineering discipline during implementation.

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

## Engineering Skills During Apply

When `openspec-apply-change` is running and implementing tasks, layer these skills based on what the task touches:

### Enforce These

- `incremental-implementation` — build in vertical slices, not horizontal layers
- `test-driven-development` — red-green-refactor per slice
- `code-review-and-quality` — before merge, always
- `git-workflow-and-versioning` — atomic commits on ship

## UI Builder Routing

During apply, if a task involves UI/frontend work:

1. **Detect stack** from `package.json` or `*.csproj`
2. **Check if Syncfusion UI Builder is installed** (look for the agent file or skill folder)
3. **If installed** → activate the UI Builder agent for page/dashboard/form generation
4. **If not installed** → suggest: `apm install syncfusion/<framework>-ui-builder -t <target>`
5. **If no Syncfusion stack** → use `frontend-ui-engineering` skill

| Signal | Framework | Package |
|--------|-----------|---------|
| `"react"` in package.json | React | `syncfusion/react-ui-builder` |
| `"@angular/core"` in package.json | Angular | `syncfusion/angular-ui-builder` |
| `Blazor` in *.csproj | Blazor | `syncfusion/blazor-ui-builder` |
| `Maui` in *.csproj | .NET MAUI | `syncfusion/maui-ui-builder` |

## Design System

If tasks include UI work and no `DESIGN.md` exists at the project root, activate the `design-system` skill to create color palette, typography, and spacing tokens **before** writing UI code. All UI code should consume `tokens.css`.

## Behaviors

- Surface assumptions before proceeding
- Stop on inconsistencies — name the confusion, wait for resolution
- Prefer the boring, obvious solution
- Stay in scope — no unsolicited renovation
- Verify at runtime, don't assume
