# Phase 3 — Build

## Purpose

Implement the plan slice-by-slice. Each slice is implemented, tested, verified, and committed before moving to the next. The system stays in a working state at all times.

## When to Enter

- Task list exists with acceptance criteria (from Phase 2)
- Dependencies are mapped and implementation order is clear

## Core Skill: `incremental-implementation`

Always active during Build. Follow the increment cycle:

```
┌──────────────────────────────────────┐
│   Implement ──→ Test ──→ Verify ──┐  │
│       ▲                           │  │
│       └───── Commit ◄─────────────┘  │
│              │                       │
│              ▼                       │
│          Next slice                  │
└──────────────────────────────────────┘
```

For each slice:
1. **Implement** the smallest complete piece of functionality
2. **Test** — write tests first (TDD), then make them pass
3. **Verify** — confirm the slice works (tests pass, build succeeds)
4. **Commit** — atomic commit with descriptive message
5. **Next slice** — carry forward, don't restart

## Companion Skills (Activate as Needed)

| Skill | Activate When |
|-------|--------------|
| `test-driven-development` | Always — write failing test first, then implement |
| `source-driven-development` | Using any framework or library — verify against official docs |
| `doubt-driven-development` | High-stakes decisions, unfamiliar code, security-sensitive logic |
| `context-engineering` | Output quality drops or context window is cluttered |
| `frontend-ui-engineering` | Building UI (non-Syncfusion or as quality overlay) |
| `api-and-interface-design` | Designing APIs, module boundaries, or public interfaces |
| `observability-and-instrumentation` | Adding production code paths — instrument as you build |

## UI Builder Routing

If the current task involves building UI pages, dashboards, or forms, detect the project stack and route to the appropriate Syncfusion UI Builder:

### Auto-Detection Rules

| Signal | UI Builder Agent |
|--------|-----------------|
| `package.json` contains `"react"` | `syncfusion-react-ui-builder` |
| `package.json` contains `"@angular/core"` | `syncfusion-angular-ui-builder` |
| `*.csproj` contains `Blazor` SDK | `syncfusion-blazor-ui-builder` |
| `*.csproj` contains `Maui` SDK | `syncfusion-maui-ui-builder` |
| `*.csproj` contains `Wpf` references | `syncfusion-wpf-ui-builder` |
| `*.csproj` contains `WindowsForms` | `syncfusion-winforms-ui-builder` |
| `*.csproj` contains `WinUI` SDK | `syncfusion-winui-ui-builder` |
| None of the above | Use `incremental-implementation` + `frontend-ui-engineering` |

### UI Builder Integration Rules

When routing to a Syncfusion UI Builder:

1. **Before UI Builder**: Ensure the spec and task breakdown exist. The UI Builder generates code, but it needs clear requirements.
2. **During UI Builder**: Let the UI Builder run its 8-stage workflow (Intent → Detect → Map → Theme → Code → Deps → Validate → Insert).
3. **After UI Builder**: Apply engineering discipline:
   - Run `test-driven-development` — write component tests for generated UI
   - Run `code-review-and-quality` — review generated code quality
   - Run `security-and-hardening` — validate no XSS, no hardcoded secrets
   - Commit with `git-workflow-and-versioning`

### Single Component vs Full UI

- **Single component** (e.g., "add a DataGrid"): Use the component skill directly. Skip the UI Builder.
- **Full page/dashboard** (3+ components): Use the UI Builder agent.

## Build Rules

1. **One slice at a time** — Do not implement multiple slices in parallel
2. **Test before expand** — Each slice must have passing tests before starting the next
3. **~100 lines per change** — Keep changes small and reviewable
4. **Working state always** — The system must build and pass tests after every commit
5. **No TODO debt** — Don't leave TODOs for "later." Implement or file a task.

## Exit Criteria (Per Slice)

- [ ] Acceptance criteria for the task are met
- [ ] Tests written and passing (unit + integration as appropriate)
- [ ] Code builds without errors or warnings
- [ ] Atomic commit with descriptive message
- [ ] No regressions in existing tests

## Exit Criteria (Phase Complete)

- [ ] All tasks from the plan are implemented
- [ ] All tests pass
- [ ] System is in a working, deployable state

## Transition

→ Proceed to **Phase 4 — Verify** (read `phase-verify.md`)
