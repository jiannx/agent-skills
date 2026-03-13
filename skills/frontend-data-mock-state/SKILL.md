---
name: frontend-data-mock-state
description: Design frontend data layers, server-state handling, adapters, mock strategies, and state ownership with stable contracts and low integration churn.
---

# Frontend Data Mock State

Use this skill when the task involves API integration, server-state design, module adapters, mock-driven delivery, or deciding where state should live.

## Use This Skill When

- The frontend needs an API client strategy or query/mutation structure.
- The task involves TanStack Query, adapters, normalization, or transport-to-domain mapping.
- The feature should run independently with mocks before real backend integration.
- The team needs clearer boundaries between local state, URL state, server state, and global state.

## Inputs To Confirm Up Front

- API maturity: stable, evolving, mock-first, or backend not ready.
- Required query and mutation flows.
- Which state should be local, URL-based, server-driven, or global.
- Whether optimistic updates or rollback behavior are needed.
- Whether locale affects API payloads, content variants, formatting rules, or translation-resource loading.

## Execute Workflow

1. Define data ownership.
- Separate transport models from domain models.
- Keep adapters close to the consuming module.

2. Define server-state strategy.
- Use TanStack Query for query lifecycle, caching, invalidation, retries, and background refresh.
- Keep request construction and response parsing out of presentational components.

3. Define mock strategy when needed.
- Use fixtures, `msw`, or development-only adapters based on scope.
- Keep the switch between mock and real implementations explicit and localized.

4. Define state boundaries.
- Use local state for ephemeral UI interaction.
- Use URL state for shareable navigation context.
- Use global state only where cross-module ownership is real.

5. Define locale-aware data boundaries when relevant.
- Decide whether locale affects fetched content, display formatting only, or both.
- Keep raw values transport-safe and perform locale-aware presentation formatting outside transport adapters unless the API contract explicitly depends on locale.

## Core Guidance

### Data ownership
- Separate transport models from domain models.
- Keep adapters close to the consuming module.

### State boundaries
- Use local state for ephemeral UI interaction.
- Use URL state for shareable navigation context.
- Use global state only where cross-module ownership is real.

### Internationalization-aware data handling
- Separate raw values from display formatting for dates, numbers, currency, percentages, and relative time.
- If APIs are locale-sensitive, make locale propagation explicit in request construction and cache ownership.
- Do not bury translation-key or locale-selection logic deep inside presentational components when it belongs to module or app-level state.

## Review Checklist

- [ ] Transport models and domain models are separated.
- [ ] Query and mutation ownership is explicit.
- [ ] Mock contracts are close enough to real contracts to avoid churn.
- [ ] State ownership is appropriate across local, URL, server, and global scopes.
- [ ] Locale-sensitive data and formatting boundaries are explicit when i18n is required.

## Output Requirements

- Data-fetching and mutation strategy.
- State management strategy.
- Mock strategy when backend integration is deferred.
- Adapter and contract-boundary notes.
- Internationalization data notes: locale-sensitive APIs, formatting ownership, and cache implications when relevant.
