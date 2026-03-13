---
name: fe-design-implementation
description: Implement product-grade frontend UI from design artifacts with consistent interaction patterns, theme-driven styling, responsive behavior, and design verification.
---

# Frontend Design Implementation

Use this skill when the task centers on translating design artifacts into code with strong visual fidelity, interaction consistency, and maintainable styling.

## Use This Skill When

- The user provides Figma, Sketch, screenshots, or an existing product to match.
- The task involves layout implementation, responsive adaptation, or interaction consistency.
- The UI should be built through tokens, theme definitions, component variants, and shared primitives.

## Inputs To Confirm Up Front

- Design source and target screens or states.
- Required desktop and mobile breakpoints.
- Whether mobile adaptation is in scope.
- Existing theme, design system, or component library.
- Whether screenshot comparison or visual verification is required.
- Locale scope, translated copy source, and whether RTL or long-text expansion must be supported.

## Execute Workflow

1. Read the design critically.
- Infer component structure, reusable patterns, spacing, hierarchy, and interaction intent.

2. Implement using shared styling systems.
- Prefer tokens, semantic color roles, typography scales, shared primitives, and variants over page-local overrides.

3. Normalize inconsistency intentionally.
- If the design is inconsistent, normalize spacing, typography, states, and interaction behavior instead of copying accidental differences.

4. Validate internationalization readiness when relevant.
- Check text expansion, truncation risk, empty-state wording, validation copy, and locale-aware formatting in the UI.
- Confirm that layouts can survive translated content without breaking hierarchy or interaction clarity.

5. Verify before completion.
- Compare key screens and states against the design.
- Generate screenshots and summarize material gaps in a concise comparison report.

## Core Guidance

### Styling strategy
- Prefer theme-level and component-level styling over local one-off overrides.
- Keep business components focused on behavior and composition.
- Push reusable visual rules into the design-system or shared UI layer.

### Interaction consistency
- Similar workflows should use the same interaction language for validation, loading, success, failure, and destructive actions.
- Responsive adaptation should reconsider density and navigation, not only width changes.

### Internationalization in UI
- Do not hardcode user-facing copy in a way that prevents extraction into translation resources.
- Design layouts to survive longer translated strings, plural forms, and locale-specific labels.
- Use locale-aware formatting for dates, numbers, currency, and relative time instead of fixed string templates.
- If RTL support is required, validate spacing, alignment, icons, and navigation patterns under mirrored layout conditions.

### Design verification
- High fidelity is expected, but exact pixel-perfect parity is not mandatory if deviations are minor and documented.

## Review Checklist

- [ ] Styling is primarily implemented through theme, tokens, or shared components.
- [ ] Similar interactions behave consistently across screens.
- [ ] Responsive behavior works for required breakpoints.
- [ ] Design verification has been done with screenshots when design artifacts exist.
- [ ] Internationalization readiness has been checked for copy length, formatting, and RTL behavior when relevant.

## Output Requirements

- Design-token and component strategy.
- Styling strategy and local-exception notes.
- Interaction consistency notes.
- Internationalization UI notes: copy extraction, formatting behavior, text expansion, and RTL risks when relevant.
- Screenshot comparison set and design verification summary when applicable.
