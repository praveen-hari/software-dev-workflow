---
description: "Plan how to build it. If OpenSpec change exists, refines its tasks. Otherwise creates a plan from clear requirements. Use when you have a spec/proposal and need implementable units."
argument-hint: "Reference the feature name or describe what to plan"
---

# /plan — Plan How to Build It

Execute **Phase 2 (Plan)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-plan.md`
2. Check if an OpenSpec change folder exists for this feature:
   - If `openspec/changes/<feature-name>/` exists → read `proposal.md`, `specs/`, and `design.md`
   - If not → the user may have clear requirements without running `/spec` first
3. If the OpenSpec change already has `tasks.md`, review and refine it:
   - Ensure tasks are vertically sliced (not horizontal layers)
   - Ensure each task has acceptance criteria
   - Ensure dependency ordering is correct
4. If no tasks exist yet, use `/opsx:apply` or manually create them:
   - Activate the `planning-and-task-breakdown` skill
   - Enter read-only mode — read the proposal/spec and relevant codebase sections
   - Map the dependency graph
   - Slice vertically — each task delivers working, testable functionality
   - Write tasks to `openspec/changes/<feature-name>/tasks.md`
5. Identify the critical path
6. Present the plan for user review — do NOT proceed until approved

## Output

Tasks written to `openspec/changes/<feature-name>/tasks.md` with:
- Grouped tasks under headings
- Hierarchical numbering (1.1, 1.2, etc.)
- Acceptance criteria per task
- Dependency ordering
- Checkboxes for tracking progress

> **Note:** The old `tasks/plan.md` and `tasks/todo.md` paths are deprecated.
> All planning artifacts now live inside the OpenSpec change folder.
