---
name: nocobase-v2-flow-upgrade
description: Upgrade existing NocoBase v1 plugins to v2 FlowModel or field-model architecture with behavior parity. Use for migration planning, implementation, parity validation, and pitfalls around request payloads, settings ownership, i18n namespaces, roles persistence, flow variables, and editor integrations.
---

# NocoBase V2 Flow Upgrade

Use this skill for existing NocoBase plugin migrations from v1 frontend patterns under `src/client/` to v2 model-based flow architecture. Default to behavior parity; do not introduce product changes unless the user asks.

## Start Here

Confirm:
- Plugin name and root path.
- Plugin type: action, field, or mixed.
- Whether core flow editor files may be changed.
- Whether backend payloads and persisted settings must remain compatible.

Load references only when needed:
- `references/feature-inventory-template.md` for v1 capability inventory.
- `references/v1-to-v2-flow-mapping.md` for owner mapping.
- `references/verification-checklist.md` before closing.

## Workflow

1. Inventory v1 behavior from `src/client/`: capabilities, settings, APIs, roles, locale keys, runtime components.
2. Classify each capability as `must-have`, `optional`, or `drop` with reason.
3. Map each v1 capability to one v2 owner: `action`, `model`, `uiSchema`, `hook/resource`, `locale`, or `runtime component`.
4. Lock API and persistence parity before coding. Preserve request shape and stored values unless explicitly changed.
5. Implement in `src/client/models/` when possible, then register actions/models in the plugin client entry.
6. Validate add/edit/run paths, ACL, i18n, persistence, and request counts.

## Implementation Rules

- Prefer the smallest migration that preserves current product behavior.
- Keep backend endpoints and data schema unchanged unless there is a hard gap.
- Do not keep both v1 `SchemaSettings` and v2 model settings active unless dual-path compatibility is explicitly required.
- Put persisted configuration in model flows and steps (`registerFlow`, `steps`, `defaultParams`, `uiMode`, `uiSchema`).
- Use `beforeParamsSave` / `afterParamsSave` for save-time normalization or side effects.
- Keep handler output shape stable for downstream flow steps.
- Use plugin namespace translations for plugin UI text, especially `tExpr(..., { ns })` in FlowModel titles and step labels.

## Plugin Type Rules

For action plugins:
- Define flow actions with explicit scene, params UI, lifecycle hooks, handler, and registration.
- Preserve v1 request helpers and payload shape, such as `resource('x').updateOrCreate({ filterKeys, values })`.

For field plugins:
- Prefer `FieldModel` / `DisplayItemModel` subclasses under `src/client/models/`.
- Bind models to the field interface and register them with `flowEngine.registerModels(...)`.
- Move canonical field settings to model-level flows.
- Preserve editor mode, read-only mode, props, database types, filterability, and settings.

## Common Pitfalls

- Payload drift: preserve legacy fields such as `collectionName`, `dataSourceKey`, `responseType`, roles, and response handling.
- i18n drift: audit en-US and zh-CN keys, namespace usage, and case-sensitive key mismatches.
- Flow variables: schema-setting variable components may need flow-setting registration or a flow-native equivalent.
- Rich editors: if CodeMirror reports duplicate extension errors, prefer direct `EditorView` / `EditorState` integration and keep the editor instance stable.
- Quick settings: put defaults on each step's `defaultParams` when the UI must display saved defaults.

## Discovery Commands

From repo root, adapt `<plugin>` as needed:

```bash
rg --files packages/plugins/@nocobase/<plugin>/src/client
rg "defineAction|beforeParamsSave|afterParamsSave|uiSchema|registerActions|registerModels|bindModelToInterface" packages/plugins/@nocobase/<plugin>/src/client -n
rg "SchemaSettings|x-settings|updateOrCreate|roles|collectionName|dataSourceKey|responseType" packages/plugins/@nocobase/<plugin>/src -n
rg "tExpr|lang\\(|useTranslation|ctx\\.t|CodeMirror|EditorView|EditorState" packages/plugins/@nocobase/<plugin>/src/client -n
```

Reference core patterns:

```bash
rg "defineAction\\(|registerFlow\\(|uiMode\\(|defaultParams|bindModelToInterface" packages/core/client/src/flow packages/core/flow-engine -n
```

## Output

End with:
- v1 inventory summary and v2 support scope.
- Capability mapping: v1 source -> v2 owner.
- API payload parity notes.
- Implemented changes and intentional behavior changes, if any.
- Validation summary for create/edit/run, ACL, i18n, persistence, and known gaps.
