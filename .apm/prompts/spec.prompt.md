---
description: "Define what to build. Starts the Define phase: interviews for requirements, refines ideas, and produces a specification document. Use when starting a new project, feature, or significant change."
argument-hint: "Describe what you want to build"
---

# /spec — Define What to Build

Execute **Phase 1 (Define)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-define.md`
2. If the request is vague, activate the `interview-me` skill — ask one question at a time until ~95% confidence
3. If the concept needs exploration, activate the `idea-refine` skill — generate approaches, evaluate tradeoffs
4. Activate the `spec-driven-development` skill — write a specification covering:
   - Objective (what, why, who, success criteria)
   - Commands (build, test, lint, dev)
   - Project structure
   - Code style (with example)
   - Testing strategy
   - Boundaries (always do / ask first / never do)
5. Surface all assumptions explicitly
6. Present the spec for user review — do NOT proceed until approved

## Output

A specification document saved to `specs/<feature-name>.md`
