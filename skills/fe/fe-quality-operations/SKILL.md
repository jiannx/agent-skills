---
name: fe-quality-operations
description: Prepare frontend projects for reliable delivery with testing, performance review, observability, release strategy, and production-readiness checks.
---

# Frontend Quality Operations

Use this skill when the task needs quality gates, production-readiness review, release planning, observability, or delivery risk control.

## Use This Skill When

- The feature needs test strategy or coverage planning.
- The task involves release rollout, release verification, rollback planning, or post-release monitoring.
- The project needs observability, performance review, or production-readiness checks.

## Inputs To Confirm Up Front

- Required test types: unit, component, integration, E2E, visual regression.
- Release cadence and deployment path.
- Monitoring stack: Sentry, analytics, Web Vitals, logging.
- High-risk workflows or production-facing changes.

## Execute Workflow

1. Define quality coverage.
- Cover critical user journeys, logic-heavy paths, permission changes, and submission flows.
- Keep screenshot or visual verification in scope when design artifacts matter, but scale the verification effort to the risk of the change.

2. Review performance and observability.
- Check route splitting, heavy dependencies, large lists, and render hotspots.
- Ensure release health is visible through logs, analytics, and error tracking.

3. Define release strategy.
- Decide rollout method, validation path, monitoring plan, and rollback approach.
- Prefer low-risk incremental releases and staged rollout for risky changes.

## Core Guidance

### Testing
- Do not rely on E2E alone for logic-heavy frontends.
- Keep mock-first and real-integration flows aligned so tests survive the handoff.

### Observability
- Track important flows and failures, not every click.
- Use release markers and diagnostics that help isolate regressions.

### Release
- Define rollout, smoke checks, monitoring expectations, and rollback criteria before sign-off for production-facing or high-risk work; use a lighter release note for narrow internal changes.

## Review Checklist

- [ ] Critical user journeys have appropriate coverage.
- [ ] Observability is sufficient to diagnose production regressions.
- [ ] Release path, rollout method, and rollback plan are explicit.
- [ ] High-risk production-facing changes have risk-reduction measures.

## Output Requirements

- Testing strategy and coverage scope.
- Observability and release-risk notes.
- Release strategy: rollout, validation, monitoring, and rollback.
- Performance risk notes where relevant.
