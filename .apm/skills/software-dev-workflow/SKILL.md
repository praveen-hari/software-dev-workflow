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

## Engineering Skills During Apply

When `openspec-apply-change` is running and implementing tasks, layer these skills based on what the task touches:

### Enforce These

- `incremental-implementation` — build in vertical slices, not horizontal layers
- `test-driven-development` — red-green-refactor per slice
- `code-review-and-quality` — before merge, always
- `git-workflow-and-versioning` — atomic commits on ship

## UI Builder Routing (Required Gate)

Before implementing ANY UI task, run this check. Do not skip it.

1. **Detect stack** from `package.json` or `*.csproj`
2. **Check if Syncfusion UI Builder is installed** (look for the agent file or skill folder)
3. **If installed** → MUST activate the UI Builder agent for page/dashboard/form generation. Do not write UI manually.
4. **If not installed** → STOP. Tell the user: `apm install syncfusion/<framework>-ui-builder -t <target>`. Only proceed manually if the user explicitly declines.
5. **If no Syncfusion stack** → use `frontend-ui-engineering` skill

| Signal | Framework | Package |
|--------|-----------|---------|
| `"react"` in package.json | React | `syncfusion/react-ui-builder` |
| `"@angular/core"` in package.json | Angular | `syncfusion/angular-ui-builder` |
| `Blazor` in *.csproj | Blazor | `syncfusion/blazor-ui-builder` |
| `Maui` in *.csproj | .NET MAUI | `syncfusion/maui-ui-builder` |

## Design System (Required for UI Work)

Before writing **any** UI code, check for `DESIGN.md` at the project root.

- **If `DESIGN.md` does not exist** → STOP. Activate the `design-system` skill first. Do not write UI code without it.
- **If `DESIGN.md` exists** → all UI code MUST use CSS variables from `tokens.css`. No hardcoded colors, fonts, spacing, or radii.

The `design-system` skill creates both `DESIGN.md` (token definitions) and `tokens.css` (CSS custom properties). Never create these files manually — always use the skill.

## Behaviors

- Surface assumptions before proceeding
- Stop on inconsistencies — name the confusion, wait for resolution
- Prefer the boring, obvious solution
- Stay in scope — no unsolicited renovation
- Verify at runtime — "seems right" is never sufficient
- Verify at runtime, don't assume
