---
description: "Plan how to build it. Starts the Plan phase: breaks the spec into small, verifiable tasks with acceptance criteria and dependency ordering. Use when you have a spec and need implementable units."
argument-hint: "Reference the spec or describe the feature to plan"
---

# /plan — Plan How to Build It

Execute **Phase 2 (Plan)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-plan.md`
2. Activate the `planning-and-task-breakdown` skill
3. Enter read-only mode — read the spec and relevant codebase sections
4. Map the dependency graph (what depends on what)
5. Slice vertically — each task delivers working, testable functionality
6. Write tasks with:
   - Description
   - Acceptance criteria (specific, verifiable)
   - Dependencies
   - Estimated scope
7. Identify the critical path
8. Present the plan for user review — do NOT proceed until approved

## Output

- `tasks/plan.md` — Dependency graph and architecture notes
- `tasks/todo.md` — Ordered task list with acceptance criteria
