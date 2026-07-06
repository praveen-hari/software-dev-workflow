# Phase 2 — Plan (Refine OpenSpec Tasks)

## Purpose

Refine the task list in the OpenSpec change folder into small, verifiable vertical slices with explicit acceptance criteria. Good task breakdown is the difference between reliable delivery and a tangled mess.

## When to Enter

- OpenSpec change folder exists at `openspec/changes/<feature-name>/`
- Proposal and specs are approved (from Phase 1)
- The implementation touches multiple files or components

## Workflow

### Step 1: Read the OpenSpec Change

Before writing any code, operate in read-only mode:

- Read `openspec/changes/<feature-name>/proposal.md` for intent and scope
- Read `openspec/changes/<feature-name>/specs/` for behavior requirements
- Read `openspec/changes/<feature-name>/design.md` for technical approach
- Read relevant codebase sections
- Identify existing patterns and conventions
- Note risks and unknowns

**Do NOT write code during planning.**

### Step 2: Refine `tasks.md`

OpenSpec's `/opsx:propose` already generates an initial `tasks.md`. Refine it:

1. **Map the dependency graph** — identify what depends on what
2. **Slice vertically** — each task delivers working, testable functionality
3. **Add acceptance criteria** — each task must have specific, verifiable criteria
4. **Order by dependency** — foundation first, then services, then presentation
5. **Identify the critical path** — mark blocking vs. parallelizable tasks

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

### Step 3: Write Refined Tasks

Update `openspec/changes/<feature-name>/tasks.md` with this format:

```markdown
## 1. [Group Name]
- [ ] 1.1 [Short description]
  - **Acceptance Criteria**: [specific, verifiable]
  - **Dependencies**: [task IDs]
  - **Scope**: [Small / Medium / Large]
- [ ] 1.2 [Short description]
  - ...

## 2. [Group Name]
- [ ] 2.1 [Short description]
  - ...
```

### Step 4: Update Design if Needed

If planning reveals architectural decisions, update `openspec/changes/<feature-name>/design.md`.

## User Gate

Present the refined task list to the user for review. Confirm:
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

Refined `openspec/changes/<feature-name>/tasks.md`

> **Migration note:** The old `tasks/plan.md` and `tasks/todo.md` paths are deprecated.
> All planning artifacts now live inside the OpenSpec change folder.

## Transition

→ Proceed to **Phase 3 — Build** (read `phase-build.md`)
