# Phase 6 — Ship

## Purpose

Deploy with confidence. Clean commit history, documentation, observability, and a rollback plan before going live.

## When to Enter

- Code is reviewed and approved (from Phase 5)
- Ready to merge and/or deploy to production

## Skills Activated

### `git-workflow-and-versioning` (Always)

- Atomic commits with descriptive messages
- Clean commit history (squash fixups, rebase if needed)
- Trunk-based development — merge to main frequently
- Change sizing: ~100 lines per commit
- Tag releases with semantic versioning

### `documentation-and-adrs` (When Decisions Were Made)

- Record Architecture Decision Records for significant choices
- Update API documentation for changed interfaces
- Document the "why" not just the "what"
- Use timeless language — describe current state, not change history

### `observability-and-instrumentation` (Production Code)

- Structured logging for new code paths
- RED metrics (Rate, Errors, Duration) for services
- Distributed tracing for cross-service calls
- Symptom-based alerting (alert on user impact, not internal metrics)
- Health checks and readiness probes

### `ci-cd-and-automation` (Pipeline Changes)

- Shift Left — run quality checks as early as possible
- Feature flags for risky changes
- Automated quality gate pipeline
- Failure feedback loops — fast notification on breaks

### `shipping-and-launch` (Production Deployment)

Pre-launch checklist:
- [ ] Feature flags configured (if applicable)
- [ ] Rollback procedure documented and tested
- [ ] Monitoring dashboards ready
- [ ] Alerting configured for new code paths
- [ ] Staged rollout plan (canary → percentage → full)
- [ ] On-call team notified

### `deprecation-and-migration` (When Retiring Old Code)

- Code-as-liability mindset — less code is better
- Compulsory vs advisory deprecation strategy
- Migration path documented for consumers
- Zombie code removal — delete what's no longer used

## Ship Checklist

### Pre-Merge
- [ ] All tests pass in CI
- [ ] Code review approved
- [ ] Documentation updated
- [ ] Commit history is clean and descriptive

### Pre-Deploy
- [ ] Feature flags set to off (if using staged rollout)
- [ ] Rollback procedure documented
- [ ] Monitoring and alerting configured
- [ ] Database migrations tested (if applicable)

### Post-Deploy
- [ ] Smoke tests pass in production
- [ ] Monitoring confirms healthy metrics
- [ ] No error rate increase
- [ ] Feature flag gradually enabled (if applicable)

## Exit Criteria

- [ ] Code merged to main branch
- [ ] Deployed to production (or ready for deployment)
- [ ] Monitoring confirms healthy state
- [ ] Documentation is current
- [ ] ADRs recorded for significant decisions

## Completion

The feature is **done** when:
1. All acceptance criteria are met
2. The standing Definition of Done is satisfied
3. Code is in production and monitored
4. Documentation reflects the current state
