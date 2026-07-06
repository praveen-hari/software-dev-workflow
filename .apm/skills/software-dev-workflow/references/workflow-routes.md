# Workflow Routes

Shortcut paths for common task types that don't need the full 6-phase lifecycle.

## Route: Bug Fix

**Trigger**: User reports a bug, test failure, or unexpected behavior.

**Path**: Debug → Fix → Test → Review → Ship

```
1. debugging-and-error-recovery
   → Reproduce the bug consistently
   → Localize to the specific component/function
   → Reduce to minimal reproduction case

2. test-driven-development
   → Write a failing test that captures the bug
   → Fix the code to make the test pass
   → Verify no regressions

3. code-review-and-quality
   → Review the fix (correctness + no side effects)
   → Verify the test actually guards against recurrence

4. git-workflow-and-versioning
   → Atomic commit: "fix: [description of what was broken]"
   → Reference issue/ticket number if applicable
```

**Skip**: Define phase, Plan phase (bug is already defined by its symptoms).

---

## Route: Refactor

**Trigger**: Code works but is harder to read, maintain, or extend than it should be.

**Path**: Simplify → Test → Review → Ship

```
1. code-simplification
   → Apply Chesterton's Fence — understand WHY before changing
   → Reduce complexity while preserving EXACT behavior
   → Remove dead code, unused imports, commented-out blocks
   → No behavior changes — refactor only

2. test-driven-development
   → Ensure existing tests still pass (100% regression check)
   → Add tests for any untested code paths discovered during refactor
   → Run full test suite before and after

3. code-review-and-quality
   → Verify behavior is preserved (diff should show structure changes, not logic changes)
   → Confirm complexity is actually reduced (not just moved)

4. git-workflow-and-versioning
   → Atomic commit: "refactor: [description of simplification]"
   → Keep refactor commits separate from feature commits
```

**Skip**: Define phase, Plan phase, Build phase (no new functionality).

---

## Route: Hotfix (Production Emergency)

**Trigger**: Production is broken. Users are affected. Time is critical.

**Path**: Debug → Fix → Test → Ship (fast track)

```
1. debugging-and-error-recovery
   → Reproduce in production-like environment
   → Identify root cause (not just symptoms)
   → Apply the MINIMAL fix — no refactoring, no improvements

2. test-driven-development
   → Write a test that captures the production failure
   → Verify the fix resolves it
   → Run critical path tests only (speed over coverage)

3. shipping-and-launch (fast track)
   → Deploy immediately after test verification
   → Monitor closely for 30 minutes post-deploy
   → Rollback plan ready before deploying

4. Follow-up (after production is stable)
   → File a task for proper fix if the hotfix was a band-aid
   → Add comprehensive tests
   → Conduct post-incident review
   → Update monitoring/alerting to catch this class of issue
```

**Skip**: Define, Plan, full Review (speed is critical). Do a post-incident review later.

---

## Route: Documentation Only

**Trigger**: Adding or updating documentation, ADRs, or README files.

**Path**: Write → Review → Ship

```
1. documentation-and-adrs
   → Write in timeless language (describe current state, not change history)
   → Document the "why" not just the "what"
   → For architectural decisions: use ADR format (Context → Decision → Consequences)

2. code-review-and-quality
   → Review for accuracy, completeness, and clarity
   → Verify code examples are correct and runnable

3. git-workflow-and-versioning
   → Atomic commit: "docs: [description]"
```

**Skip**: Define, Plan, Build, Verify phases.

---

## Route: CI/CD Pipeline Change

**Trigger**: Setting up or modifying build, test, or deployment pipelines.

**Path**: Plan → Build → Verify → Ship

```
1. planning-and-task-breakdown
   → Define what the pipeline should do
   → Identify quality gates needed

2. ci-cd-and-automation
   → Shift Left — run checks as early as possible
   → Configure quality gate pipeline
   → Set up failure feedback loops

3. Verify
   → Test the pipeline with a dry run
   → Verify all quality gates trigger correctly
   → Confirm failure notifications work

4. documentation-and-adrs
   → Document pipeline configuration
   → Record decisions about quality gates
```

---

## Route: Dependency Update

**Trigger**: Updating packages, frameworks, or libraries.

**Path**: Plan → Build → Verify → Review → Ship

```
1. planning-and-task-breakdown
   → List all dependencies to update
   → Check changelogs for breaking changes
   → Identify migration steps needed

2. deprecation-and-migration
   → Follow migration guides from official docs
   → Update deprecated API usage

3. source-driven-development
   → Verify changes against official documentation
   → Don't rely on memory for new API patterns

4. test-driven-development
   → Run full test suite after each update
   → Fix any breaking changes

5. security-and-hardening
   → Audit updated dependencies for known vulnerabilities
   → Verify no new security issues introduced
```

---

## Route Selection Decision Tree

```
Is production broken RIGHT NOW?
├── YES → Hotfix route
└── NO
    ├── Is this a bug report?
    │   └── YES → Bug fix route
    ├── Is this a refactor (no new behavior)?
    │   └── YES → Refactor route
    ├── Is this documentation only?
    │   └── YES → Documentation route
    ├── Is this a CI/CD change?
    │   └── YES → CI/CD route
    ├── Is this a dependency update?
    │   └── YES → Dependency update route
    ├── Is this a trivial change (< 10 lines, obvious)?
    │   └── YES → Direct (TDD + commit)
    └── Is this a feature or project?
        ├── Requirements clear? → Plan-forward (Phase 2 start)
        └── Requirements unclear? → Full lifecycle (Phase 1 start)
```
