---
name: nocobase-bugfix
description: Diagnose and fix NocoBase v2 bugs, especially flow, FlowModel, client-v2, and plugin client issues. Use for issue reproduction, root-cause analysis, targeted fixes, regression checks, or narrow v2 feature completion based on nearby v1 behavior. Default to v2-only changes unless the user explicitly asks for v1.
---

# NocoBase Bugfix

## Defaults

- Fix v2 only. Treat v1 schema code as reference unless the user asks to modify v1.
- Prefer code-path analysis, tests, and local reproduction before browser use. Open a browser only when explicitly requested or necessary.
- Make the smallest safe change that fixes the cause, not just the symptom.
- Search with `rg` first.

## Where To Look

- `packages/core/client/src/flow`
- `packages/core/flow-engine/src`
- `packages/core/client-v2`
- Plugin packages: `packages/plugins`, `packages/pro-plugins/@nocobase`

For plugin v2 bugs, search the plugin package first, then inspect `src/client-v2` and `src/client/models`.

## Workflow

1. Identify the exact scene, component, block, field type, action path, and data context.
2. Decide whether the issue is broken v2 behavior or missing v2 capability that should mirror v1.
3. Trace from the user-visible entry point to the owning model, flow, renderer, schema initializer, plugin model, or engine helper.
4. For flow bugs, identify the correct owner before editing: `FlowModel`, block model, `FieldModel`, `DisplayItemModel`, action, or plugin registration.
5. Implement the narrowest v2 fix. If borrowing from v1, port only the behavior contract into the v2 architecture.
6. Run focused tests or the smallest meaningful manual verification. State anything not verified.

## FlowModel Rules

- Keep persistent settings in the model/flow path; do not add a second settings source or revive v1 `SchemaSettings` unless explicitly required.
- Put configurable behavior into flows and steps (`registerFlow`, `steps`, `defaultParams`, `uiMode`, `uiSchema`) when it must persist or participate in the editor.
- Use lifecycle hooks such as `beforeParamsSave` / `afterParamsSave` for save-time normalization or side effects.
- Preserve registration and context contracts: check `scene`, model inheritance, `bindModelToInterface`, `registerModels`, `registerActions`, and plugin-side model registration before changing shared renderers.
- Use namespace-aware text such as `tExpr(..., { ns })` for plugin FlowModel UI copy.

## Task IDs

If the user provides `taskid` or a plain numeric string, treat it as a task ID and fetch details when useful. Resolve `NOCOBASE_TEST_API_TOKEN` from project `.env`, environment variables, then zsh environment.

```bash
curl 'https://test_management.v2.test.nocobase.com/nocobase/api/tasks:get?filterByTk={taskid}' \
  -H 'authorization: Bearer {NOCOBASE_TEST_API_TOKEN}'
```

## Closing Format

End with a concise bilingual summary:

```md
问题原因：
- ...

变更日志：
- fix: ...

Cause:
- ...

Changelog:
- fix: ...
```
