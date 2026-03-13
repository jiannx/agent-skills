---
name: fe-component-feature-system
description: Build coherent component systems and feature-module structures with clear ownership boundaries, reusable abstractions, and maintainable shared UI patterns.
---

# Frontend Component Feature System

Use this skill when the task involves shared UI architecture, component layering, feature module boundaries, or extracting repeatable patterns from product workflows.

## Use This Skill When

- The codebase needs a component system instead of isolated widgets.
- Feature code is scattered and needs better module ownership.
- The task requires deciding what belongs in primitives, composite components, domain components, or feature modules.

## Inputs To Confirm Up Front

- Existing shared UI or design system structure.
- Repeated product patterns across pages.
- Stable versus volatile abstractions.
- Desired module ownership boundaries.

## Execute Workflow

1. Define component layers.
- Separate primitives, composite components, and domain components.
- Keep primitives free of business data and module-specific logic.

2. Define feature boundaries.
- Co-locate UI, data access, types, validation, and feature hooks inside each feature module.
- Keep feature entry points obvious.

3. Extract only proven abstractions.
- Prefer composition, variants, and slots over giant prop surfaces.
- Move code to shared layers only when the concept is stable across features.

## Core Guidance

### Component system
- Primitives encode tokens, accessibility, and interaction states.
- Composite components encode shared product patterns such as `PageHeader`, `DataTable`, or `FilterBar`.
- Domain components encode business semantics and stay close to their module unless truly cross-domain.

### Feature system
- Feature modules should contain UI, data access, domain mapping, validation, and types together.
- Avoid scattering one feature across unrelated app-level directories.

## Review Checklist

- [ ] Primitive, composite, and domain component boundaries are clear.
- [ ] Shared components encode stable patterns instead of generic wrappers.
- [ ] Feature code is co-located by module.
- [ ] Shared extraction is based on real repetition rather than speculation.

## Output Requirements

- Component system strategy.
- Feature/module boundary strategy.
- Shared abstraction decisions and rationale.
