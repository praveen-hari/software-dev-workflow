---
description: "Build incrementally. Starts the Build phase: implements tasks slice-by-slice with TDD. Auto-detects project stack and routes UI work to the correct Syncfusion UI Builder (React, Angular, Blazor, MAUI, WPF, WinForms, WinUI). Use when tasks are defined and ready for implementation."
argument-hint: "Specify which task to build, or 'auto' for sequential execution"
---

# /build — Build Incrementally

Execute **Phase 3 (Build)** of the software development workflow.

## Prerequisites

**If the task list includes ANY UI work:**
- `DESIGN.md` MUST exist at the project root (created by the `design-system` skill via `/design`)
- `tokens.css` MUST exist at the project root alongside it
- Both MUST be approved by the user
- If missing → STOP and tell the user: "Run `/design` first. The `design-system` skill must finalize the design system before any UI code is written."
- Backend-only tasks can proceed without `DESIGN.md`

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-build.md`
2. Load the task list:
   - **Primary:** `openspec/changes/<feature-name>/tasks.md` (OpenSpec change folder)
   - **Fallback:** `tasks/todo.md` (legacy path, if OpenSpec is not initialized)
3. For each task (in dependency order):
   a. **Implement** the smallest complete piece of functionality
   b. **Test** — activate `test-driven-development` (write failing test first, then make it pass)
   c. **Verify** — confirm tests pass and build succeeds
   d. **Commit** — atomic commit with descriptive message
4. If the task involves UI work:
   a. **Verify `DESIGN.md` + `tokens.css` exist at project root** — if not, STOP and run `/design` first
   b. All UI code MUST use CSS variables from `tokens.css` — no hardcoded colors, fonts, or spacing
   c. Detect the project stack from project files
   d. Check if the matching Syncfusion UI Builder is installed (per-project dependency)
   e. If installed → route to the UI Builder agent (it consumes `tokens.css`)
   f. If NOT installed → inform user with install command (`apm install syncfusion/<framework>-ui-builder -t <target>`), fall back to `incremental-implementation` + `frontend-ui-engineering`
5. Activate companion skills as needed:
   - `source-driven-development` when using frameworks
   - `doubt-driven-development` for high-stakes decisions
   - `api-and-interface-design` for API work
   - `observability-and-instrumentation` for production code paths

## Auto Mode

If invoked with `auto`, execute all tasks sequentially. Each task is still test-driven and committed individually. Pause on failures or risky steps.

## Output

Working, tested code committed slice-by-slice.
