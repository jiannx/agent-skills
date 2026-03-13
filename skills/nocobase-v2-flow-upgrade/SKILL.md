---
name: nocobase-v2-flow-upgrade
description: Upgrade NocoBase v1 plugins to v2 FlowModel or field-model architecture with behavior parity. Covers migration planning, implementation, parity validation, and common pitfalls around request payloads, settings ownership, i18n namespaces, roles persistence, flow variables, and editor integrations.
---

# NocoBase V2 Flow Upgrade

Use this skill to upgrade existing NocoBase plugins from v1 frontend patterns to v2 models-based flow architecture.

This skill is for **existing plugin upgrades**, not greenfield plugin design. The default goal is parity with the current plugin behavior unless the user explicitly asks for product changes.

## Use This Skill When

- The repo is a NocoBase monorepo or contains the target plugin source.
- The plugin currently uses v1 frontend patterns under `src/client/` and needs a v2 models-based migration.
- The migration may target either a flow action plugin or a custom field plugin.
- Backend APIs and persisted data should remain stable unless the user explicitly approves a contract change.

## Inputs To Confirm Up Front

- Plugin name and root path.
- Whether the target is an action plugin, field plugin, or mixed plugin.
- Whether core flow editor files may be changed.
- Whether parity is strict or allows intentional product changes.
- Whether backend payload shape and persisted settings must remain byte-for-byte compatible.

## Execute Workflow

1. Confirm migration scope.
- Identify plugin root and branch.
- Confirm the task is a frontend-first migration; keep backend endpoints and data schema unchanged unless a hard gap is found.
- Record old code paths in `src/client/` and target paths in `src/client/models/`.
- Explicitly confirm if core flow editor files are allowed to change (for example `packages/core/client/src/flow/components/DynamicFlowsIcon.tsx`).

2. Build v1 feature inventory.
- Collect v1 user-visible capabilities and configuration points from `src/client/`.
- Fill `references/feature-inventory-template.md` completely.
- Mark each item as `must-have`, `optional`, or `drop` with reason.

3. Define v2 support list and mapping.
- Convert inventory to v2 implementation targets using `references/v1-to-v2-flow-mapping.md`.
- For plugin upgrades, move behavior into models-based flow actions and settings-oriented hooks.
- Map each v1 capability to one v2 owner: `action`, `model`, `uiSchema`, `resource call`, `locale`, or `runtime component`.

4. Lock parity contract **before coding**.
- For every request call used in v1, capture the exact payload shape and required fields.
- Mark each field as `required`, `conditional`, or `legacy-noise`.
- Do not “improve” payload shape unless explicitly requested; default to old shape.

5. Implement v2 frontend.
- Prefer adding/rewriting code in `src/client/models/`.
- Register new flow actions in plugin client entry (`src/client/index.tsx`) via flow engine registration.
- Put parameter editing into action `uiSchema` and `beforeParamsSave/afterParamsSave` when needed.
- Keep API contracts stable so backend code remains unchanged whenever possible.
- Preserve existing user-facing labels, defaults, and persistence semantics unless the parity contract says otherwise.

6. Distinguish plugin type before coding.
- Do not assume every plugin migration target is an action plugin.
- If the plugin is a custom field plugin, migrate it to v2 field-model patterns instead of action patterns.
- For custom field plugins, prefer `FieldModel` / `DisplayItemModel` subclasses under `src/client/models/`, bind them to the field interface, and register them with `flowEngine.registerModels(...)` in the plugin client entry.
- Preserve feature parity of the original field component before introducing any flow-specific enhancement.

7. Validate parity before finishing.
- Run through `references/verification-checklist.md`.
- Verify create/edit/run paths and role/permission behavior.
- Verify locale keys for all new labels and error messages.
- Verify request counts (no duplicate GET/POST caused by remount/effect timing).

8. Report migration outcome.
- Summarize what was migrated, what stayed unchanged, and any intentional deviations.
- Call out unresolved parity gaps explicitly, with owner files and next action.
- Do not finish with a vague “mostly works” summary.

## Migration Rules (Hard Requirements)

### 1) Request payload parity first
- If v1 uses `resource('x').updateOrCreate({ filterKeys, values })`, v2 must use the same shape.
- Avoid replacing with raw `ctx.request({ url: '/x:updateOrCreate', data: ... })` unless v1 also did this.
- Preserve legacy option fields such as `collectionName`, `dataSourceKey`, `responseType`, etc.

### 2) i18n namespace consistency
- In plugin flow uiSchema, do not rely on generic flow translations for plugin-specific copy.
- Use plugin namespace helpers (for example template generators) and ensure **both** en-US and zh-CN keys exist.
- Audit keys for case-sensitive mismatches (`Custom Request` vs `Custom request`).
- In flow model titles and step titles, prefer `tExpr('Key', { ns: NAMESPACE })` instead of bare `tExpr('Key')`.
- In helper files and custom dropdown renderers, do not pass raw `ctx.t` through unchanged; wrap it so plugin strings always resolve with `ns: NAMESPACE`.

### 3) Field-plugin migration rules
- If the source plugin is a field plugin, inventory all user-visible field behaviors first: editor mode, read-only mode, component props, supported database types, filterability, and settings.
- Register editable/display field models explicitly and bind them to the interface with the correct default model mapping.
- If the field had configurable behavior in v1 schema settings, move the canonical configuration path to model-level `registerFlow({ steps })` for v2.
- Avoid leaving both legacy 1.0 `SchemaSettings` and new 2.0 model settings active unless dual-path compatibility is an explicit requirement. Two write paths commonly cause state drift (`x-component-props` vs `model.props`).
- For quick settings similar to table column width, prefer `uiMode(ctx) => ({ type: 'select', key, props: { options, dropdownRender } })` over a generic `uiSchema` dialog.
- For independent quick settings items, put defaults on each step's `defaultParams`, not only at the parent flow level. Otherwise the default value may save correctly but fail to display in the UI.

### 4) Flow variable UI compatibility
- Some variable components that work in schema settings may not work in flow settings runtime directly.
- If missing in flow component registry, either:
	- register the needed component in flow settings, or
	- build a flow-native equivalent (recommended when variable systems differ).

### 5) Rich editor / CodeMirror integration pitfalls
- If a field plugin embeds CodeMirror and hits errors such as `Unrecognized extension value in extension set`, suspect duplicate CodeMirror package instances first.
- In that case, prefer a direct `EditorView` / `EditorState` integration over wrapper components that may pull incompatible extension instances.
- Keep the editor instance stable after mount. Do not recreate the editor on each `value`, `height`, `language`, or `indentUnit` prop change; use compartments and reconfigure effects instead.
- When adding indent configuration, update both `indentUnit` and `tabSize` semantics. Validate actual typing behavior, not only static config state.
- Preserve normal editor behavior for selections. A custom Tab handler must not overwrite multi-line selections with plain spaces.
- If height is configurable, keep stored values stable (`auto`, `300px`, `50%`) and translate only display labels, not persisted values.

## Case Notes (From specific migrations)

These are **not universal requirements**. Apply only when the plugin/context matches.

- Save timing issue: some flows did not trigger `beforeParamsSave` as expected in save path.
- Access-control persistence issue: request-level roles may require explicit persistence strategy.
- Duplicate preload request issue: effect/remount patterns can cause repeated `:get` calls.
- Body storage semantics issue: text vs JSON-object persistence may diverge from legacy behavior.
- i18n namespace/key drift issue: plugin keys may silently fall back when missing.
- Dual settings source issue: keeping both 1.0 schema settings and 2.0 model settings active may cause one UI to update while the runtime reads from another source.
- Step default visibility issue: quick settings may appear empty until `defaultParams` is defined on the step itself.
- Custom dropdown translation issue: helper-rendered placeholders may stay English if they rely on raw `ctx.t` without plugin namespace.
- Editor remount issue: editor recreation on every keystroke can break IME/composition and cursor stability.
- Indentation behavior issue: displayed indent settings may appear ineffective if only `indentUnit` is updated but actual key handling is not validated.

When these symptoms appear, treat them as targeted diagnostics, not default migration tasks.

## Review Checklist (Must pass)

### A. API/contract parity
- [ ] For each migrated request, payload key set matches v1.
- [ ] Optional legacy fields (`collectionName`, `dataSourceKey`, etc.) preserved where v1 used them.
- [ ] Response handling parity verified (`json`/`stream`, download filename behavior if applicable).

### B. Persistence lifecycle
- [ ] Persistence timing matches product expectation (create/edit/save lifecycle).

### C. Access control
- [ ] ACL behavior matches product requirements and v1 behavior.

### D. i18n
- [ ] All newly introduced labels exist in en-US and zh-CN.
- [ ] Plugin UI strings use plugin namespace.
- [ ] Case-sensitive key mismatches audited.
- [ ] Flow step titles use `tExpr(..., { ns: NAMESPACE })` when plugin-specific.
- [ ] Helper-rendered dropdown/placeholders do not rely on unscoped `ctx.t`.

### E. Runtime behavior
- [ ] Create/edit/run all pass.
- [ ] Flow result object shape is stable for downstream steps.
- [ ] For editor-like fields, height/indent settings visibly affect the runtime component.
- [ ] Editor input does not remount on each keystroke.
- [ ] Tab and Shift-Tab behavior are correct for single cursor and multi-line selection.

## Use Core References

Read these paths as the primary implementation references for FlowModel patterns:
- `packages/core/client/src/flow`
- `packages/core/flow-engine`

Follow local patterns before introducing a new abstraction.

## Working Style

- Read the existing plugin code before proposing structure changes.
- Prefer the smallest migration that preserves product behavior.
- Avoid keeping two competing settings sources unless dual-path support is explicitly required.
- If the migration target is ambiguous, decide based on the current plugin behavior and state the decision.
- When a parity gap is discovered, document it before changing behavior.

## Fast Discovery Commands

Use these commands from repo root to collect migration context:

```bash
rg --files packages/plugins/@nocobase/plugin-<name>/src/client
rg "defineAction|beforeParamsSave|afterParamsSave|uiSchema|flowEngine.registerActions" packages/plugins/@nocobase/plugin-<name>/src/client -n
rg "customize:|x-action|x-settings|SchemaSettings" packages/plugins/@nocobase/plugin-<name>/src/client -n
rg "updateOrCreate|:send|:get|listByCurrentRole|roles" packages/plugins/@nocobase/plugin-<name>/src -n
rg "FieldModel|DisplayItemModel|bindModelToInterface|registerModels" packages/plugins/@nocobase/plugin-<name>/src/client -n
rg "CodeMirror|EditorView|EditorState|Compartment|indentUnit|tabSize" packages/plugins/@nocobase/plugin-<name>/src/client -n
```

Use these commands to inspect Flow reference implementations:

```bash
rg "defineAction\(|ActionScene|FlowRuntimeContext|setupRuntimeContextSteps" packages/core/client/src/flow packages/core/flow-engine -n
rg "registerFlow\(|uiMode\(|defaultParams\(|dropdownRender" packages/core/client/src/flow packages/core/flow-engine -n
rg "Column width|CustomWidth|bindModelToInterface" packages/core/client/src/flow -n
```

Use these commands for parity diff review:

```bash
git --no-pager diff -- packages/plugins/@nocobase/plugin-<name>/src/client packages/plugins/@nocobase/plugin-<name>/src/server | cat
rg "\$nForm|collectionName|dataSourceKey|responseType|content-disposition" packages/plugins/@nocobase/plugin-<name>/src -n
rg "tExpr\(|lang\(|useTranslation\(|ctx\.t\(" packages/plugins/@nocobase/plugin-<name>/src/client -n
```

## Output Requirements

Produce these artifacts in every migration task:
- Completed v1 inventory table.
- v2 support scope (must-have/optional/drop).
- Capability mapping table (v1 source -> v2 model/action).
- Implemented code changes with parity notes.
- Validation summary and known gaps.

Additionally include:
- API payload parity table (v1 vs v2 per request).
- Risk list with severity (`high|medium|low`) and owner files.
- Explicit list of intentional behavior changes (if any).

For field-plugin migrations additionally include:
- Settings source map (`legacy schema settings` vs `v2 model settings`) and which one is canonical.
- Runtime prop map (`model.props` -> rendered component props).
- Editor behavior checklist if the field wraps a code/text editor.

## Completion Standard

The task is complete only when:

- Code changes are implemented or a concrete blocker is identified.
- Parity risks are listed explicitly instead of implied.
- Validation status is reported for create, edit, run, ACL, i18n, and persistence.
- Any intentional behavior changes are called out as intentional, not accidental drift.
