---
name: fe-bootstrap-architecture
description: Initialize or restructure frontend projects with sound framework selection, project bootstrap, architecture, routing foundations, environment setup, and maintainable module boundaries.
---

# Frontend Bootstrap Architecture

Use this skill when the task starts from an empty repo, a new frontend app, a major structural refactor, or an unclear existing codebase that needs architectural direction.

This skill is optimized for React, Next.js, Vite, and React-adjacent repositories, but it can also be used to inspect an existing Vue or other frontend project before making architecture decisions.

## Use This Skill When

- The task starts from project initialization or framework selection.
- The repository already exists and needs structural review before implementation.
- The user needs architecture, directory strategy, environment setup, or baseline engineering conventions.
- The task affects routing foundations, app shell design, module boundaries, or shared layers.

## Inputs To Confirm Up Front

- Whether the task is greenfield or based on an existing local repo.
- Preferred framework or whether framework choice is still open.
- Rendering model: SPA, SSR, SSG, ISR, or hybrid.
- Package manager, workspace needs, and deployment constraints.
- Environment requirements: local, development, staging, production.
- Mobile support, browser support, and internationalization requirements.

## Execute Workflow

1. Inspect the current starting point.
- If the repo exists, read the local structure, scripts, routing approach, styling approach, test setup, and environment conventions first.
- If the repo does not exist, identify the smallest viable stack that fits the product constraints.

2. Choose the project foundation deliberately.
- Select React, Next.js, Vite, Vue, or another framework based on product requirements rather than habit.
- Keep initialization choices aligned with team familiarity, deployment model, and ecosystem stability.

3. Define architecture before feature growth.
- Establish app shell boundaries, module strategy, shared layers, and dependency direction.
- Keep domain modules explicit instead of relying on pages alone as architecture.

4. Establish configuration and engineering baseline.
- Add linting, formatting, type checking when applicable, environment config, and test scaffolding.
- Make environment ownership explicit through a centralized config layer.
- If internationalization is required, establish translation infrastructure, locale loading strategy, and formatter ownership early.

## Core Guidance

### Framework selection
- Use Next.js when SSR, SEO, edge rendering, or route-level data loading materially matter.
- Use Vite for SPA-heavy products, internal tools, and fast iteration when SSR is not the core requirement.
- Do not replace an existing healthy stack without a concrete architectural reason.

### Architecture
- Prefer domain-oriented structure for larger products.
- Keep shared layers small and stable.
- Separate local state, server state, URL state, and global state.
- Prefer mature libraries over custom infrastructure for solved problems.

### Environment baseline
- Support required environments explicitly: local, development, staging, and production.
- Centralize environment access in config files such as `src/config/env.ts` and `src/config/runtimeConfig.ts`.
- Validate required environment variables early.

### Internationalization baseline
- Decide early whether the project needs single-locale delivery, multilingual support, or near-term i18n readiness.
- Choose a translation strategy deliberately: static dictionaries, namespace-based resources, CMS-backed content, or hybrid loading.
- Define ownership for text resources, date and time formatting, number formatting, currency formatting, and pluralization.
- Keep the baseline architecture capable of locale switching, lazy-loaded message bundles, and RTL support when relevant.

## Review Checklist

- [ ] It is clear whether the task starts from a greenfield repo or an existing project.
- [ ] Framework choice is justified by product and deployment requirements.
- [ ] Module boundaries and shared layers are understandable.
- [ ] Environment access is centralized and validated.
- [ ] Baseline engineering setup exists for linting, formatting, and test scaffolding.
- [ ] Internationalization baseline is explicit when multilingual support or i18n-readiness is required.

## Output Requirements

- Bootstrap or initialization strategy.
- Framework and rendering decision with tradeoffs.
- Module or directory strategy.
- Environment strategy and config ownership.
- Internationalization baseline and locale-loading strategy when relevant.
- Explicit assumptions and constraints.
