---
name: fe-product-development
description: Build product-grade frontend projects with implementation guidance optimized for React ecosystems. Covers bootstrap, architecture, design implementation, data flow, testing, observability, release strategy, and long-term maintainability.
---

# Frontend Product Development

Use this skill to design, implement, refactor, or scale a frontend project that is expected to survive real product evolution, not just deliver a one-off page.

This is a master frontend delivery skill. It is optimized for React, Next.js, Vite, and React-adjacent workflows. It also covers project bootstrap, framework selection, and existing-project inspection when the actual repository is Vue-based or framework choice is still open.

This skill is for product-grade frontend work: reusable business modules, maintainable architecture, design fidelity, engineering rigor, and production readiness.

It treats frontend development as product implementation, not only visual implementation. The UI must correctly carry business rules, workflow constraints, permissions, states, and user feedback.

It applies to:
- Greenfield React applications.
- Large feature additions in existing React codebases.
- Frontend architecture refactors.
- Design system and shared component work.
- SaaS and admin platforms with complex configuration and permissions.
- Project bootstrap and project initialization work for new or existing frontend repositories.

It is not optimized for:
- Throwaway prototypes with no long-term ownership.
- Single isolated component tasks where broader architecture does not matter.
- Backend-only tasks.

## Use This Skill When

- The user wants to build or improve a frontend project, especially in a React, Next.js, or Vite ecosystem.
- The task starts from project bootstrap, framework selection, or repository initialization.
- The task involves turning business requirements into reusable frontend product capabilities.
- The work requires design implementation from Figma, Sketch, screenshots, or an existing product.
- The codebase needs structure for scale: modules, routing, state, data fetching, permissions, testing, and observability.
- The system will be maintained by a team over time and must support extension instead of repeated rewrites.

Use this master skill when the task crosses multiple frontend domains or when the right implementation path is still ambiguous.

For narrower tasks, use only the relevant subsection of this document instead of applying the whole skill mechanically.

## How This Skill Is Organized

The document is structured in four layers so implementation decisions stay coherent:

- Discovery and bootstrap: project initialization, architecture, routing, permissions, and feature/module boundaries.
- Product and UI delivery: productization, design implementation, component system, and interaction consistency.
- Application behavior: data layer, mock strategy, state strategy, environment handling, and React implementation practices.
- Quality and operations: engineering, performance, testing, observability, release strategy, and completion standards.

When the repo already exists, read and align with local conventions first. When the repo does not exist yet, choose the smallest sound setup that still matches product constraints.

## Suggested Child Skill Map

This file acts as the master skill. The sections below have been split into smaller child skills for finer-grained invocation.

- `fe-bootstrap-architecture`
  Covers project initialization, framework selection, architecture, and environment baseline.
- `fe-routing-permission`
  Covers routing strategy, permission system, guards, access states, and URL-state modeling.
- `fe-design-implementation`
  Covers design implementation, interaction consistency, styling strategy, design verification, and responsive adaptation.
- `fe-component-feature-system`
  Covers component system, feature development pattern, shared abstractions, and module boundaries.
- `fe-data-mock-state`
  Covers data layer strategy, state strategy, mock strategy, adapters, and API integration.
- `fe-quality-operations`
  Covers testing, performance review, observability, release strategy, and production-readiness checks.

Prefer one or more child skills for narrow tasks. Use this master skill when the task spans multiple domains or when the correct implementation path is still unclear.

## Inputs To Confirm Up Front

- Whether the task is broad enough to justify the master skill instead of a single child skill.
- Whether the repo already exists and should be inspected before any structural recommendation.
- Whether the task primarily concerns bootstrap, routing and permissions, design implementation, component and feature structure, data and mock strategy, or quality and operations.
- Whether multiple domains are tightly coupled in the current task.
- Whether internationalization is already in scope or likely to become a near-term requirement.

If the task is narrow, route to one or more child skills instead of expanding the whole master skill.

## Orchestration Workflow

1. Identify scope first.
- Determine whether the task is narrow or cross-domain.
- If the task is narrow, use the most relevant child skill directly.
- If the task is cross-domain, keep this master skill active and invoke only the required domains.

2. Inspect the current project context.
- If a local repo exists, read local structure, framework, conventions, and constraints before choosing a path.
- Do not recommend framework or architecture changes without confirming they are actually needed.

3. Route the task to the right child skills.
- Use `fe-bootstrap-architecture` for bootstrap, framework choice, architecture, and environment baseline.
- Use `fe-routing-permission` for navigation, URL state, auth boundaries, and permission systems.
- Use `fe-design-implementation` for UI implementation, responsive behavior, styling strategy, and design verification.
- Use `fe-component-feature-system` for shared UI, component layering, and feature/module boundaries.
- Use `fe-data-mock-state` for API integration, state ownership, adapters, and mock-driven delivery.
- Use `fe-quality-operations` for testing, performance review, observability, release strategy, and production-readiness checks.

4. Recombine outputs into one coherent delivery plan.
- Resolve conflicts between child-skill recommendations.
- Keep one consistent architecture, state model, permission model, release plan, and testing scope.

## Master Rules

- Do not apply every frontend concern mechanically to every task.
- Prefer the smallest sound scope that still covers real product risk.
- Keep module boundaries explicit and avoid hidden cross-module dependencies.
- Prefer stable libraries over custom infrastructure for solved problems.
- Always implement the necessary user-visible states: loading, error, empty, success, and permission-restricted states when relevant.
- If design artifacts exist, include design verification before sign-off.
- If backend readiness is deferred, use a deliberate mock strategy rather than ad hoc fake data.
- Treat environment, observability, and release as required concerns for production-facing work, but not as mandatory deep-design exercises for trivial UI-only tasks.
- If internationalization is required, treat locale strategy, copy extraction, formatting, and layout resilience as first-class concerns rather than a late-stage patch.

## Cross-Cutting Requirements

These apply regardless of which child skill is activated:

- Accessibility and semantic UI must not be ignored.
- Security-sensitive behavior must stay aligned with backend enforcement.
- Similar interactions should remain consistent across the product.
- Styling should prefer theme, tokens, and shared primitives over repeated local overrides.
- Important assumptions and deviations must be documented explicitly.
- Internationalization should cover copy extraction, locale-aware formatting, and resilience for longer translated text or RTL layout when relevant.

## Fast Routing Guide

Use this guide to choose the smallest useful skill set:

- New repo, framework choice, or structural reset:
  `fe-bootstrap-architecture`
- Route guards, auth redirects, access control, or tenant-aware navigation:
  `fe-routing-permission`
- Figma-to-code, responsive UI, tokenized styling, or screenshot comparison:
  `fe-design-implementation`
- Shared UI, feature modules, design-system extraction, or refactoring repeated page structures:
  `fe-component-feature-system`
- Query and mutation design, adapters, mock-first delivery, or state ownership:
  `fe-data-mock-state`
- Locale-aware formatting, translation ownership, or i18n-ready UI delivery:
  combine `fe-bootstrap-architecture`, `fe-design-implementation`, and `fe-data-mock-state`
- Testing, observability, performance review, rollout, rollback, or release checks:
  `fe-quality-operations`
- Broad product delivery touching several of the above at once:
  keep `fe-product-development` as the controller and combine the needed child skills

## Output Requirements

The master skill should produce a consolidated plan or delivery summary rather than repeating every child-skill detail verbatim.

Include:

- Which child skills were activated and why.
- The final implementation path across architecture, UI, data, and operations.
- Cross-domain tradeoffs or conflicts that were resolved.
- Explicit assumptions, risks, and deferred work.

When relevant, also include:

- Design verification summary.
- Mock-to-real integration plan.
- Permission and routing boundary summary.
- Environment and release summary.
- Internationalization summary: locale scope, formatting strategy, translation ownership, and known gaps.

## Completion Standard

The master skill is complete only when:

- The task has been routed to the correct child skills.
- Cross-domain decisions are coherent rather than contradictory.
- The final plan is scoped correctly for the actual task size.
- Risks, assumptions, and unresolved gaps are explicit.