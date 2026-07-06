# Phase 1 — Define

## Purpose

Get 100% clarity on WHAT to build before writing any code. This phase produces a specification document that becomes the shared source of truth.

## When to Enter

- Starting a new project or feature
- Requirements are ambiguous, incomplete, or only exist as a vague idea
- The change touches multiple files or modules
- The task would take more than 30 minutes to implement

## When to Skip

- Single-line fixes, typo corrections, or self-contained changes
- Requirements are already documented and unambiguous
- Bug fixes (use the bug-fix route instead)

## Skill Sequence

### Step 1: Extract Intent — `interview-me`

If the request is underspecified, activate the `interview-me` skill:

- Ask one question at a time until ~95% confidence about the underlying intent
- Surface what the user actually wants, not what they think they should want
- Do NOT proceed until requirements are concrete

**Skip if:** The user has already provided detailed, unambiguous requirements.

### Step 2: Refine Ideas — `idea-refine`

If the concept needs exploration, activate the `idea-refine` skill:

- Structured divergent thinking: generate 3-5 approaches
- Convergent thinking: evaluate tradeoffs, select the best approach
- Output: a concrete proposal with clear rationale

**Skip if:** The approach is already decided and agreed upon.

### Step 3: Write the Spec — `spec-driven-development`

Activate the `spec-driven-development` skill. Write a specification covering:

1. **Objective** — What are we building and why? Who is the user? What does success look like?
2. **Commands** — Full executable commands (build, test, lint, dev)
3. **Project Structure** — Where source code, tests, and docs live
4. **Code Style** — One real code snippet showing the style
5. **Testing Strategy** — Framework, location, coverage expectations
6. **Boundaries** — Always do / Ask first / Never do

### User Gate

Present the spec to the user for review. Do NOT proceed to Phase 2 until the user approves.

```
ASSUMPTIONS I'M MAKING:
1. [assumption about requirements]
2. [assumption about architecture]
3. [assumption about scope]
→ Correct me now or I'll proceed with these.
```

## Exit Criteria

- [ ] Spec document written and covers all 6 areas
- [ ] Assumptions surfaced and confirmed
- [ ] User has reviewed and approved the spec
- [ ] Ambiguities resolved — no open questions remain

## Output Artifacts

- `specs/<feature-name>.md` — The specification document

## Transition

→ Proceed to **Phase 2 — Plan** (read `phase-plan.md`)
