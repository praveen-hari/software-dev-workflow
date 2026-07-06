---
description: "Prove it works. Starts the Verify phase: runs tests, debugs failures, and verifies behavior at runtime. Use when code is written and needs verification, or when tests fail."
argument-hint: "Describe what to verify, or leave blank for full verification"
---

# /test — Prove It Works

Execute **Phase 4 (Verify)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-verify.md`
2. Run the complete test suite
3. If tests fail, activate `debugging-and-error-recovery`:
   - Reproduce → Localize → Reduce → Fix → Guard
4. For web frontends, activate `browser-testing-with-devtools`:
   - DOM renders correctly
   - Console has no errors
   - Responsive layout works
   - Accessibility: keyboard navigation works
5. Verify against acceptance criteria from the task list
6. Confirm runtime behavior with evidence (not just "seems right")

## Verification Checklist

- [ ] All tests pass (unit, integration, e2e)
- [ ] Runtime behavior verified
- [ ] No console errors or build warnings
- [ ] Edge cases and error paths confirmed
- [ ] No regressions in existing functionality

## Output

Verification report with evidence of passing tests and runtime confirmation.
