# Phase 4 — Verify

## Purpose

Prove the implementation works with evidence. "Seems right" is never sufficient — there must be passing tests, successful builds, and runtime confirmation.

## When to Enter

- Code is written and committed (from Phase 3)
- Individual slice tests pass but end-to-end verification is needed
- Integration between slices needs confirmation

## Skills Activated

### `test-driven-development` (Verification Pass)

Run the complete test suite and verify:

- **Unit tests**: All pass, coverage meets project standards
- **Integration tests**: Components work together correctly
- **Edge cases**: Error paths, boundary conditions, empty states handled
- **Regression**: No existing tests broken by new changes

### `debugging-and-error-recovery` (If Issues Found)

If any test fails or behavior is unexpected, activate this skill:

1. **Reproduce** — Confirm the failure is consistent
2. **Localize** — Narrow down to the specific component/function
3. **Reduce** — Create the minimal reproduction case
4. **Fix** — Apply the targeted fix
5. **Guard** — Add a test that catches this specific failure

### `browser-testing-with-devtools` (Web Frontend)

For web applications, verify in the browser:

- DOM renders correctly
- Console has no errors or warnings
- Network requests succeed
- Performance is acceptable
- Responsive layout works at all breakpoints
- Accessibility: keyboard navigation, screen reader compatibility

## Verification Checklist

### Correctness
- [ ] All acceptance criteria met (check against task definitions)
- [ ] Code runs and behaves as intended at runtime
- [ ] New behavior covered by tests that fail without the change
- [ ] Existing tests still pass — no regressions
- [ ] Edge cases and error paths handled

### Integration
- [ ] Change works with the rest of the system, not just in isolation
- [ ] Database migrations, config changes, feature flags accounted for
- [ ] API contracts honored (request/response shapes match)

### Runtime Evidence
- [ ] Application starts without errors
- [ ] Key user flows work end-to-end
- [ ] Build output is clean (no warnings)
- [ ] Test output shows all green

## Exit Criteria

- [ ] All tests pass (unit, integration, e2e as applicable)
- [ ] Runtime behavior verified with evidence
- [ ] No console errors, build warnings, or test failures
- [ ] Edge cases and error paths confirmed

## Transition

→ Proceed to **Phase 5 — Review** (read `phase-review.md`)
