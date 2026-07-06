---
description: "Ship to production. Starts the Ship phase: clean commit history, documentation, observability setup, and deployment with rollback plan. Use when code is reviewed and ready to deploy."
argument-hint: "Specify deployment target or leave blank for standard ship process"
---

# /ship — Ship to Production

Execute **Phase 6 (Ship)** of the software development workflow.

## Steps

1. Read the `software-dev-workflow` skill, then read `references/phase-ship.md`
2. Activate `git-workflow-and-versioning`:
   - Clean commit history (squash fixups if needed)
   - Semantic version tag
   - Descriptive merge commit
3. Activate `documentation-and-adrs`:
   - Record Architecture Decision Records for significant choices
   - Update API docs for changed interfaces
4. Activate `observability-and-instrumentation`:
   - Structured logging for new code paths
   - Metrics and alerting configured
5. Activate `shipping-and-launch`:
   - Pre-launch checklist verified
   - Rollback procedure documented
   - Staged rollout plan (if applicable)
   - Monitoring dashboards ready
6. If retiring old code, activate `deprecation-and-migration`
7. After successful deployment, archive the OpenSpec change:
   - Run `/opsx:archive` to merge delta specs into `openspec/specs/` (source of truth)
   - The change folder moves to `openspec/changes/archive/` with a date prefix
   - This keeps `openspec/changes/` clean — only active work shows

## Pre-Deploy Checklist

- [ ] All tests pass in CI
- [ ] Code review approved
- [ ] Documentation updated
- [ ] Rollback procedure documented
- [ ] Monitoring configured

## Output

Deployment confirmation with monitoring status and rollback instructions.
