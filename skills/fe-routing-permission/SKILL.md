---
name: fe-routing-permission
description: Design frontend navigation, route ownership, URL state, auth boundaries, and permission systems with explicit access rules and user-state handling.
---

# Frontend Routing Permission

Use this skill when the task involves route design, navigation flows, auth redirects, permission modeling, or access control across routes and components.

## Use This Skill When

- The user needs route hierarchy, layout strategy, deep links, or URL-state design.
- The task involves auth guards, permission boundaries, or role and capability modeling.
- The feature includes public/private routes, forbidden states, or tenant-scoped access.

## Inputs To Confirm Up Front

- Public versus authenticated areas.
- Required route hierarchy and shared layouts.
- Which state should live in the URL.
- Permission model: roles, capabilities, policies, entitlements, tenant scope.
- Unauthorized, forbidden, expired-session, and downgraded-access behavior.

## Execute Workflow

1. Model routes around product flows.
- Design routes around user journeys and domain concepts, not only file names.
- Keep URLs readable, stable, and shareable.

2. Define route ownership and layout boundaries.
- Separate app shell routes, section layouts, feature routes, and route-level error states.
- Use nested layouts deliberately.

3. Define access rules explicitly.
- Put major access boundaries at route level.
- Use component-level guards for fine-grained actions.
- Distinguish hidden, disabled, read-only, and forbidden states intentionally.

4. Align frontend and backend authorization.
- Frontend checks should reflect product behavior, not replace backend security.
- Model permissions as capabilities or policies rather than raw role-name checks.

## Core Guidance

### Routing
- Use URL state for filters, tabs, pagination, and shareable view state.
- Avoid encoding ephemeral-only UI state in the URL.
- Handle loading, unauthorized, forbidden, not-found, and redirect flows explicitly.

### Permissions
- Prefer domain-level permission names such as `canEditUser` or `canApproveInvoice`.
- Keep permission data typed and traceable.
- Do not scatter checks like `role === 'admin'` across leaf components.

## Review Checklist

- [ ] Route hierarchy reflects product flows and layout ownership.
- [ ] URL state is used intentionally and only where shareability matters.
- [ ] Route-level loading, unauthorized, forbidden, and not-found states are defined.
- [ ] Permission rules are modeled explicitly instead of scattered as raw role checks.
- [ ] Frontend permission behavior is aligned with backend enforcement.

## Output Requirements

- Routing strategy: hierarchy, layouts, guards, and URL-state decisions.
- Permission strategy: capability model, UI access states, and backend alignment.
- Open questions about auth, tenancy, or access rules.
