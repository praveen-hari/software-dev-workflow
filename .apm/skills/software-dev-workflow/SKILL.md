---
name: software-dev-workflow
description: >
  Production-grade software development workflow for any tech stack.
  Orchestrates a 6-phase lifecycle (Define → Plan → Build → Verify → Review → Ship)
  using agent-skills for engineering discipline and Syncfusion UI Builders for
  frontend generation. Use when starting a new project, building features, fixing
  bugs, refactoring code, or shipping to production. Activates automatically based
  on task type.
---

# Software Development Workflow

## Overview

This skill orchestrates the complete software development lifecycle by routing tasks to the right agent-skills at the right time. It does not duplicate existing skills — it sequences them, enforces phase gates, and ensures nothing is skipped.

Every task follows a subset of the 6-phase lifecycle. The skill determines which phases apply based on task classification.

## The 7-Phase Lifecycle

```
  DEFINE        PLAN          DESIGN         BUILD          VERIFY        REVIEW         SHIP
 ┌────────┐   ┌────────┐   ┌────────┐   ┌──────┐   ┌──────┐   ┌──────┐   ┌──────┐
 │ Explore│─▶│  Tasks │─▶│ Colors │─▶│ Code │─▶│ Test │─▶│  QA  │─▶│  Go  │
 │Propose │   │  Plan  │   │ Tokens │   │ Impl │   │Debug │   │ Gate │   │ Live │
 └────────┘   └────────┘   └────────┘   └──────┘   └──────┘   └──────┘   └──────┘
  /spec        /plan        /design        /build       /test       /review      /ship
```

> **Define + Plan** use [OpenSpec](https://openspec.dev/) for structured artifact management.
> **Design** finalizes the visual system (colors, typography, spacing) before any UI code.
> **Build → Ship** use agent-skills for engineering discipline (TDD, review, security, CI/CD).
> All artifacts live in `openspec/changes/<feature-name>/` until archived.

## Task Classification & Route Selection

Before starting any work, classify the task and select the appropriate route:

```
Task arrives
    │
    ├── New project or major feature?
    │   → FULL LIFECYCLE: Define → Plan → Build → Verify → Review → Ship
    │   → Read: references/phase-define.md (start here)
    │
    ├── Feature with clear requirements?
    │   → PLAN-FORWARD: Plan → Build → Verify → Review → Ship
    │   → Read: references/phase-plan.md (start here)
    │
    ├── Bug fix?
    │   → BUG PATH: debugging-and-error-recovery → TDD → Review → Ship
    │   → Read: references/workflow-routes.md#bug-fix
    │
    ├── Refactor / simplification?
    │   → REFACTOR PATH: code-simplification → TDD → Review → Ship
    │   → Read: references/workflow-routes.md#refactor
    │
    ├── Hotfix (production emergency)?
    │   → HOTFIX PATH: Debug → Fix → Test → Ship (fast track)
    │   → Read: references/workflow-routes.md#hotfix
    │
    ├── UI page/dashboard (Syncfusion stack)?
    │   → UI BUILD PATH: Define → Plan → UI Builder → Verify → Review → Ship
    │   → Read: references/phase-build.md#ui-builder-routing
    │
    └── Single file / trivial change?
        → DIRECT: Just do it with TDD + Review
        → No workflow overhead needed
```

## Phase Execution Rules

### Entry & Exit Gates

Every phase has gates. Do not advance until the exit criteria are met.

| Phase | Entry Criteria | Exit Criteria |
|-------|---------------|---------------|
| **Define** | Task is non-trivial and requirements are unclear | OpenSpec change folder created, proposal + specs approved by user |
| **Plan** | OpenSpec change exists or requirements are clear | `tasks.md` refined with acceptance criteria, approved |
| **Design** | Tasks include UI work and no `DESIGN.md` exists | `DESIGN.md` + `tokens.css` created and approved by user |
| **Build** | Tasks defined with acceptance criteria; `DESIGN.md` approved (if UI work) | Each slice: implemented, tested, verified |
| **Verify** | Code is written | All tests pass, runtime behavior confirmed |
| **Review** | Code is complete and tested | 5-axis review passed, no blocking issues |
| **Ship** | Review approved | Deployed, monitored, documented |

### Phase Details

Each phase has a dedicated reference file with full instructions:

- **Phase 1 — Define**: Read `references/phase-define.md`
- **Phase 2 — Plan**: Read `references/phase-plan.md`
- **Phase 2.5 — Design**: Read `references/phase-design.md`
- **Phase 3 — Build**: Read `references/phase-build.md`
- **Phase 4 — Verify**: Read `references/phase-verify.md`
- **Phase 5 — Review**: Read `references/phase-review.md`
- **Phase 6 — Ship**: Read `references/phase-ship.md`

For task-type routing (bug fix, refactor, hotfix): Read `references/workflow-routes.md`

## Stack Detection & UI Builder Routing

> **Context budget note:** UI Builders are installed per-project, not bundled with
> this workflow. Each builder pulls ~60–70 component skills. Installing all 7
> frameworks would add ~500+ skills to the context — most of which are irrelevant
> to any single project. Install only the one you need.

During the Build phase, if the task involves UI/frontend work:

### Step 1 — Detect the project stack

| Signal | Framework | UI Builder Package |
|--------|-----------|--------------------|
| `package.json` contains `"react"` | React | `syncfusion/react-ui-builder` |
| `package.json` contains `"@angular/core"` | Angular | `syncfusion/angular-ui-builder` |
| `*.csproj` contains `Blazor` SDK | Blazor | `syncfusion/blazor-ui-builder` |
| `*.csproj` contains `Maui` SDK | .NET MAUI | `syncfusion/maui-ui-builder` |
| `*.csproj` contains `Wpf` references | WPF | `syncfusion/wpf-ui-builder` |
| `*.csproj` contains `WindowsForms` | WinForms | `syncfusion/winforms-ui-builder` |
| `*.csproj` contains `WinUI` SDK | WinUI | `syncfusion/winui-ui-builder` |
| None of the above | — | Not applicable |

### Step 2 — Check if the UI Builder is installed

Look for the builder's agent file (e.g., `syncfusion-react-ui-builder.agent.md`)
or skill folder in the installed skills directory.

```
UI Builder installed?
    │
    ├── YES → MUST activate the Syncfusion UI Builder agent
    │         (it runs its own 8-stage workflow: Intent → Detect → Map →
    │          Theme → Code → Deps → Validate → Insert)
    │         The UI Builder consumes tokens.css from the design system.
    │
    ├── NO  → Tell the user:
    │         "Detected [Framework] project. To enable Syncfusion UI generation, run:
    │          apm install syncfusion/[framework]-ui-builder -t <target>"
    │         → Fall back to incremental-implementation + frontend-ui-engineering
    │
    └── No Syncfusion stack detected
        → Use incremental-implementation + frontend-ui-engineering skills
```

After UI Builder generation, the workflow still enforces engineering discipline:
write tests, review code, check security, then commit.

## Skill Activation Matrix

| Phase | Skills / Tools Activated | When |
|-------|--------------------------|------|
| **Define** | OpenSpec `/opsx:explore` | Requirements are vague or underspecified |
| | OpenSpec `/opsx:propose` | Always for non-trivial work — creates change folder with proposal, specs, design, tasks |
| **Plan** | `planning-and-task-breakdown` | To refine `openspec/changes/<name>/tasks.md` after proposal is approved |
| **Design** | `design-system` (MUST activate) | When tasks include UI work and no `DESIGN.md` exists — MUST run the `design-system` skill to create color palette, typography, spacing tokens. Never create DESIGN.md without this skill. |
| **Build** | `incremental-implementation` | Always — build in vertical slices |
| | `test-driven-development` | Always — red-green-refactor per slice |
| | `source-driven-development` | When using frameworks/libraries |
| | `doubt-driven-development` | High-stakes or unfamiliar code |
| | `context-engineering` | When context quality drops |
| | `frontend-ui-engineering` | UI work (non-Syncfusion or overlay) |
| | `api-and-interface-design` | API or module boundary work |
| | Syncfusion UI Builder (MUST activate when installed) | UI work on Syncfusion stack — MUST activate the UI Builder agent when installed. If not installed, prompt user to install. Fall back to agent-skills only if user explicitly declines. |
| **Verify** | `debugging-and-error-recovery` | Something broke or behaves unexpectedly |
| | `browser-testing-with-devtools` | Web frontend verification |
| **Review** | `code-review-and-quality` | Always before merge |
| | `code-simplification` | Code is complex or hard to read |
| | `security-and-hardening` | Handles user input, auth, or data |
| | `performance-optimization` | Performance requirements or regressions |
| **Ship** | `git-workflow-and-versioning` | Always — atomic commits |
| | `ci-cd-and-automation` | Pipeline setup or modification |
| | `documentation-and-adrs` | Architectural decisions made |
| | `observability-and-instrumentation` | Production code paths |
| | `shipping-and-launch` | Deploying to production |
| | `deprecation-and-migration` | Retiring old systems |

## Core Operating Behaviors

These apply at all times, inherited from agent-skills:

1. **Surface assumptions** — State what you're assuming before proceeding. Don't silently fill in ambiguous requirements.
2. **Manage confusion** — Stop when you encounter inconsistencies. Name the confusion. Wait for resolution.
3. **Push back when warranted** — Point out problems directly. Propose alternatives. Accept overrides with full information.
4. **Enforce simplicity** — Prefer the boring, obvious solution. If 100 lines suffice, don't write 1000.
5. **Maintain scope discipline** — Touch only what you're asked to touch. No unsolicited renovation.
6. **Verify, don't assume** — Every task needs evidence: tests passing, build output, runtime data.

## Definition of Done

Every change must clear this standing bar (from `references/definition-of-done.md` in agent-skills):

- [ ] All acceptance criteria met
- [ ] Code runs and behaves as intended, verified at runtime
- [ ] New behavior covered by tests
- [ ] Existing tests still pass
- [ ] No dead code, debug output, or commented-out blocks
- [ ] Public interfaces documented
- [ ] Security implications reviewed
- [ ] Human has reviewed and approved

## Anti-Rationalization

Common excuses the agent must reject:

| Excuse | Rebuttal |
|--------|---------|
| "The requirements are obvious, skip the spec" | Non-trivial work without a spec is guessing. Write the spec. |
| "I'll add tests later" | Tests are not optional. Red-green-refactor per slice. |
| "It's a small change, skip review" | Small changes cause big outages. Review everything. |
| "Security isn't relevant here" | If it handles user input or data, security is relevant. |
| "Let me refactor this adjacent code too" | Stay in scope. File a separate task for refactoring. |
| "The UI Builder handles everything" | UI Builder generates frontend. You still need design tokens, specs, tests, and review. |
| "I'll write the UI manually" | If the Syncfusion UI Builder is installed, you MUST activate it for page/dashboard/form generation. Manual UI code is only for non-Syncfusion stacks or when the user explicitly declines installation. |
| "This is just a prototype" | Prototypes become production. Build it right from the start. |
| "I'll pick colors as I go" | No. Activate the `design-system` skill and finalize DESIGN.md first. All UI code uses tokens.css. |
| "The design system can come later" | Design before UI code. Run `/design` to activate the `design-system` skill before `/build`. |
| "I'll just use Tailwind defaults" | The project has its own design tokens in DESIGN.md. Use `tokens.css` variables, not framework defaults. |

## Quick Reference

| Command | Phase | What Happens |
|---------|-------|-------------|
| `/spec` | Define | OpenSpec: `/opsx:explore` → `/opsx:propose` → creates change folder |
| `/plan` | Plan | Refine `openspec/changes/<name>/tasks.md` with vertical slices |
| `/design` | Design | **MUST activate `design-system` skill** → creates `DESIGN.md` + `tokens.css` at project root |
| `/build` | Build | Reads tasks, implements slice-by-slice with TDD. UI code consumes `tokens.css`. |
| `/test` | Verify | Run tests, debug failures, verify at runtime |
| `/review` | Review | 6-axis code review (correctness, readability, architecture, design compliance, security, performance) |
| `/ship` | Ship | Git workflow → CI/CD → Deploy → Monitor → `/opsx:archive` |
