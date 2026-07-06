# Phase 1 — Define (via OpenSpec)

## Purpose

Get 100% clarity on WHAT to build before writing any code. This phase uses [OpenSpec](https://openspec.dev/) to produce a self-contained change folder with proposal, specs, design, and tasks — all co-located.

## Prerequisites

OpenSpec must be initialized in the project. If `openspec/` directory does not exist:

```bash
npm install -g @fission-ai/openspec@latest
openspec init
```

## When to Enter

- Starting a new project or feature
- Requirements are ambiguous, incomplete, or only exist as a vague idea
- The change touches multiple files or modules
- The task would take more than 30 minutes to implement

## When to Skip

- Single-line fixes, typo corrections, or self-contained changes
- Requirements are already documented and unambiguous
- Bug fixes (use the bug-fix route instead)

## Workflow

### Step 1: Explore (optional) — `/opsx:explore`

If the request is underspecified or the approach is unclear, use OpenSpec's explore command:

- `/opsx:explore` is a no-stakes thinking partner that reads the codebase, weighs options, and shapes a plan
- It replaces the `interview-me` and `idea-refine` skills for the Define phase
- Output: a concrete direction agreed upon before any artifacts are created

**Skip if:** The user already has clear, detailed requirements.

### Step 2: Propose — `/opsx:propose <feature-name>`

Create a change folder with all planning artifacts in one step:

```
/opsx:propose add-dark-mode
```

OpenSpec creates:
```
openspec/changes/add-dark-mode/
├── proposal.md     ← Why we're doing this, scope, approach
├── specs/          ← Delta specs (ADDED/MODIFIED/REMOVED requirements)
├── design.md       ← Technical approach, architecture decisions
└── tasks.md        ← Implementation checklist with checkboxes
```

The proposal covers:
1. **Intent** — What are we building and why?
2. **Scope** — What's in scope and what's explicitly out of scope
3. **Approach** — High-level technical direction

The delta specs cover:
- **ADDED requirements** — New behavior
- **MODIFIED requirements** — Changed behavior (with "Previously:" note)
- **REMOVED requirements** — Deprecated behavior

### User Gate

Present the proposal and specs for user review. Do NOT proceed to Phase 2 until the user approves.

```
ASSUMPTIONS I'M MAKING:
1. [assumption about requirements]
2. [assumption about architecture]
3. [assumption about scope]
→ Correct me now or I'll proceed with these.
```

## Exit Criteria

- [ ] OpenSpec change folder created at `openspec/changes/<feature-name>/`
- [ ] `proposal.md` covers intent, scope, and approach
- [ ] `specs/` contains delta specs with ADDED/MODIFIED/REMOVED sections
- [ ] Assumptions surfaced and confirmed
- [ ] User has reviewed and approved
- [ ] Ambiguities resolved — no open questions remain

## Output Artifacts

```
openspec/changes/<feature-name>/
├── proposal.md
├── specs/
├── design.md
└── tasks.md
```

> **Migration note:** This replaces the old `specs/<feature-name>.md` flat file.
> All artifacts for a feature are now co-located in one OpenSpec change folder.

## Transition

→ Proceed to **Phase 2 — Plan** (read `phase-plan.md`) to refine `tasks.md`
