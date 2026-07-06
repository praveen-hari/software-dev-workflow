---
description: "Build incrementally. Starts the Build phase: implements tasks slice-by-slice with TDD. Auto-detects project stack and routes UI work to the correct Syncfusion UI Builder (React, Angular, Blazor, MAUI, WPF, WinForms, WinUI). Use when tasks are defined and ready for implementation."
argument-hint: "Specify which task to build, or 'auto' for sequential execution"
---

# /build — Build Incrementally

Execute **Phase 3 (Build)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-build.md`
2. Load the task list from `tasks/todo.md`
3. For each task (in dependency order):
   a. **Implement** the smallest complete piece of functionality
   b. **Test** — activate `test-driven-development` (write failing test first, then make it pass)
   c. **Verify** — confirm tests pass and build succeeds
   d. **Commit** — atomic commit with descriptive message
4. If the task involves UI work, detect the project stack and route to the correct Syncfusion UI Builder
5. Activate companion skills as needed:
   - `source-driven-development` when using frameworks
   - `doubt-driven-development` for high-stakes decisions
   - `api-and-interface-design` for API work
   - `observability-and-instrumentation` for production code paths

## Auto Mode

If invoked with `auto`, execute all tasks sequentially. Each task is still test-driven and committed individually. Pause on failures or risky steps.

## Output

Working, tested code committed slice-by-slice.
