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

## The 6-Phase Lifecycle

```
  DEFINE          PLAN           BUILD          VERIFY         REVIEW          SHIP
 ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
 │ Idea │ ───▶ │ Spec │ ───▶ │ Code │ ───▶ │ Test │ ───▶ │  QA  │ ───▶ │  Go  │
 │Refine│      │  PRD │      │ Impl │      │Debug │      │ Gate │      │ Live │
 └──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
  /spec          /plan          /build        /test         /review       /ship
```

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
| **Define** | Task is non-trivial and requirements are unclear | Spec document approved by user |
| **Plan** | Spec exists or requirements are clear | Task list with acceptance criteria approved |
| **Build** | Tasks defined with acceptance criteria | Each slice: implemented, tested, verified |
| **Verify** | Code is written | All tests pass, runtime behavior confirmed |
| **Review** | Code is complete and tested | 5-axis review passed, no blocking issues |
| **Ship** | Review approved | Deployed, monitored, documented |

### Phase Details

Each phase has a dedicated reference file with full instructions:

- **Phase 1 — Define**: Read `references/phase-define.md`
- **Phase 2 — Plan**: Read `references/phase-plan.md`
- **Phase 3 — Build**: Read `references/phase-build.md`
- **Phase 4 — Verify**: Read `references/phase-verify.md`
- **Phase 5 — Review**: Read `references/phase-review.md`
- **Phase 6 — Ship**: Read `references/phase-ship.md`

For task-type routing (bug fix, refactor, hotfix): Read `references/workflow-routes.md`

## Stack Detection & UI Builder Routing

During the Build phase, if the task involves UI/frontend work, auto-detect the project stack and route to the correct Syncfusion UI Builder:

```
Detect project stack:
    │
    ├── package.json + "react"           → syncfusion-react-ui-builder agent
    ├── package.json + "@angular/core"   → syncfusion-angular-ui-builder agent
    ├── *.csproj + "Blazor"              → syncfusion-blazor-ui-builder agent
    ├── *.csproj + "Maui"                → syncfusion-maui-ui-builder agent
    ├── *.csproj + "Wpf"                 → syncfusion-wpf-ui-builder agent
    ├── *.csproj + "WindowsForms"        → syncfusion-winforms-ui-builder agent
    ├── *.csproj + "WinUI"               → syncfusion-winui-ui-builder agent
    │
    └── No Syncfusion stack detected
        → Use incremental-implementation + frontend-ui-engineering skills
```

The UI Builder handles its own 8-stage workflow (Intent → Detect → Map → Theme → Code → Deps → Validate → Insert). Our workflow wraps it with the engineering discipline from agent-skills (spec first, test after, review before merge).

## Skill Activation Matrix

| Phase | Skills Activated | When |
|-------|-----------------|------|
| **Define** | `interview-me` | Requirements are vague or underspecified |
| | `idea-refine` | Concept needs exploration before committing |
| | `spec-driven-development` | Always for non-trivial work |
| **Plan** | `planning-and-task-breakdown` | Always after spec is approved |
| **Build** | `incremental-implementation` | Always — build in vertical slices |
| | `test-driven-development` | Always — red-green-refactor per slice |
| | `source-driven-development` | When using frameworks/libraries |
| | `doubt-driven-development` | High-stakes or unfamiliar code |
| | `context-engineering` | When context quality drops |
| | `frontend-ui-engineering` | UI work (non-Syncfusion or overlay) |
| | `api-and-interface-design` | API or module boundary work |
| | Syncfusion UI Builder | UI work on Syncfusion stack (auto-detected) |
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
| "The UI Builder handles everything" | UI Builder generates frontend. You still need specs, tests, and review. |
| "This is just a prototype" | Prototypes become production. Build it right from the start. |

## Quick Reference

| Command | Phase | What Happens |
|---------|-------|-------------|
| `/spec` | Define | Interview → Idea Refine → Write PRD |
| `/plan` | Plan | Break spec into vertical slices with acceptance criteria |
| `/build` | Build | Implement slice-by-slice with TDD + UI Builder if applicable |
| `/test` | Verify | Run tests, debug failures, verify at runtime |
| `/review` | Review | 5-axis code review + security + performance audit |
| `/ship` | Ship | Git workflow → CI/CD → Deploy → Monitor |
