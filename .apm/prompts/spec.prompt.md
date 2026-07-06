---
description: "Define what to build. Uses OpenSpec to explore the idea, draft a proposal with specs, design, and tasks — all in one self-contained change folder. Use when starting a new project, feature, or significant change."
argument-hint: "Describe what you want to build"
---

# /spec — Define What to Build

Execute **Phase 1 (Define)** of the software development workflow using [OpenSpec](https://openspec.dev/).

## Prerequisites

OpenSpec must be initialized in the project. If `openspec/` directory does not exist, tell the user:
```
openspec init
```

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-define.md`
2. If the request is vague, use `/opsx:explore` to think it through with the AI first
   - This is a no-stakes thinking partner that reads the codebase, weighs options, and shapes a plan
   - Skip if the user already has clear, detailed requirements
3. Run `/opsx:propose <feature-name>` to create a change folder with all artifacts:
   - `openspec/changes/<feature-name>/proposal.md` — Why, scope, approach
   - `openspec/changes/<feature-name>/specs/` — Delta specs (what's changing in system behavior)
   - `openspec/changes/<feature-name>/design.md` — Technical approach, architecture decisions
   - `openspec/changes/<feature-name>/tasks.md` — Implementation checklist with checkboxes
4. Surface all assumptions explicitly
5. If the project involves UI work, detect the framework early and recommend installing the matching Syncfusion UI Builder if not already present:
   - `apm install syncfusion/<framework>-ui-builder -t <target>`
6. Present the proposal and specs for user review — do NOT proceed until approved

## Output

A self-contained change folder at `openspec/changes/<feature-name>/` containing:
- `proposal.md` — Intent, scope, and approach
- `specs/` — Delta specs describing behavior changes
- `design.md` — Technical approach and architecture decisions
- `tasks.md` — Ordered task list with checkboxes

> **Note:** This replaces the old `specs/<feature-name>.md` and `tasks/` flat files.
> All artifacts for a feature are now co-located in one folder via OpenSpec.
