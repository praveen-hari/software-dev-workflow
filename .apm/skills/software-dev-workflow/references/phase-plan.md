# Phase 2 — Plan

## Purpose

Break the spec into small, verifiable tasks with explicit acceptance criteria. Good task breakdown is the difference between reliable delivery and a tangled mess.

## When to Enter

- Spec exists and is approved (from Phase 1)
- Requirements are clear and the scope is defined
- The implementation touches multiple files or components

## Skill Activated

### `planning-and-task-breakdown`

Activate the `planning-and-task-breakdown` skill. Follow these steps:

### Step 1: Enter Read-Only Mode

Before writing any code, operate in read-only mode:

- Read the spec and relevant codebase sections
- Identify existing patterns and conventions
- Map dependencies between components
- Note risks and unknowns

**Do NOT write code during planning.**

### Step 2: Map the Dependency Graph

Identify what depends on what:

```
Foundation layer (schema, types, config)
    │
    ├── Service layer (business logic, API)
    │       │
    │       └── Presentation layer (UI, CLI)
    │
    └── Infrastructure (migrations, seeds)
```

Implementation order follows the dependency graph bottom-up.

### Step 3: Slice Vertically

Build one complete feature path at a time, not horizontal layers:

**Bad (horizontal):**
```
Task 1: Build entire database schema
Task 2: Build all API endpoints
Task 3: Build all UI components
```

**Good (vertical):**
```
Task 1: User can create an account (schema + API + UI)
Task 2: User can log in (auth + API + UI)
Task 3: User can create a task (schema + API + UI)
```

Each vertical slice delivers working, testable functionality.

### Step 4: Write Tasks

Each task must include:

```markdown
### Task [ID]: [Short description]
- **Description**: What to implement
- **Acceptance Criteria**:
  - [ ] [Specific, verifiable criterion]
  - [ ] [Another criterion]
- **Dependencies**: [Task IDs this depends on]
- **Estimated Scope**: [Small / Medium / Large]
```

### Step 5: Identify the Critical Path

Mark which tasks are on the critical path (blocking other work) and which can be parallelized.

## User Gate

Present the task list to the user for review. Confirm:
- Task order makes sense
- Acceptance criteria are clear
- Scope estimates are reasonable
- Nothing is missing

## Exit Criteria

- [ ] All tasks have acceptance criteria
- [ ] Tasks are ordered by dependency
- [ ] Each task is small enough for a single focused session
- [ ] Vertical slices deliver working functionality
- [ ] User has reviewed and approved the plan

## Output Artifacts

- `tasks/plan.md` — Dependency graph and architecture notes
- `tasks/todo.md` — Ordered task list with acceptance criteria

## Transition

→ Proceed to **Phase 3 — Build** (read `phase-build.md`)
