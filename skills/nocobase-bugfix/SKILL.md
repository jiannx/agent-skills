---
name: nocobase-bugfix
description: Diagnose and fix NocoBase bugs with emphasis on v2 flow, client-v2, and plugin client models. Use when working on NocoBase issue reproduction, root-cause analysis, targeted code fixes, regression validation, or task-driven bug repair in `packages/core/client/src/flow`, `packages/core/flow-engine/src`, `packages/core/client-v2`, `packages/plugins`, or `packages/pro-plugins/@nocobase`. Default to fixing v2 only unless the user explicitly asks for v1 changes; use v1 schema implementations only as reference when v2 lacks equivalent support.
---

# Nocobase Bugfix

## Overview

Use this skill to handle NocoBase bugfix work in a disciplined way: confirm the problem shape, locate the root cause, make the smallest safe v2 change, then run focused regression checks. Treat v1 schema code as reference material unless the user explicitly asks to modify v1. Do not open a browser for reproduction or validation unless the user explicitly asks for it or the task clearly requires browser-based inspection.

## Scope

- Default to v2-only fixes.
- Do not modify v1 schema code unless the user explicitly asks for it.
- Use v1 implementations as reference when a feature exists in v1 but is missing or incomplete in v2.
- Do not open a browser for reproduction or validation unless the user explicitly asks for it or browser inspection is necessary to complete the task safely.
- Prioritize these code areas:
  - `packages/core/client/src/flow`
  - `packages/core/client/src/flow/models`
  - `packages/core/flow-engine/src`
  - `packages/core/client-v2`
  - `packages/plugins`
  - `packages/pro-plugins/@nocobase`
- For plugin v2 code, inspect `src/client/models` and `src/client-v2` first.

## Workflow

1. Confirm the problem shape.
- Read the user report carefully.
- If the user provides a URL, treat it as context for locating the scene or code path. Open it in a browser only when the user explicitly requests browser-based reproduction or the task cannot be completed safely without it. Do not open the page inside VS Code.
- If the user provides a `taskid`, fetch the task detail before changing code when that helps clarify the requirement.

2. Reproduce before editing.
- Identify the exact scene, component, block, field type, action path, and data context.
- Prefer code-path analysis, local reproduction without a browser, existing tests, or existing fixtures before writing new code.
- If reproduction is blocked, gather the smallest missing fact and ask only a brief targeted question.

3. Locate the root cause.
- Search with `rg` first.
- Trace from user-visible entry point to model, flow, renderer, schema initializer, plugin model, or engine helper.
- Separate symptom from cause before patching.
- Prefer understanding registration, scene filtering, model inheritance, flow step wiring, and context propagation before changing behavior.

4. Implement the smallest safe fix.
- Change the narrowest v2 location that fixes the actual cause.
- Avoid broad refactors during bugfix work unless they are necessary to remove the defect safely.
- Preserve surrounding behavior and existing extension points.
- If v2 lacks an implementation and v1 has it, port only the necessary idea, not the whole v1 structure.

5. Validate regressions.
- Run the narrowest relevant tests first.
- Add or update tests when the bug can be covered reliably.
- If automated coverage is unavailable, perform the smallest meaningful manual verification and state what was not verified.

## Investigation Guide

- For flow UI issues, start from the visible block, field, action, or popup, then trace into:
  - `packages/core/client/src/flow/models`
  - `packages/core/client/src/flow/actions`
  - `packages/core/client/src/flow/flows`
  - `packages/core/flow-engine/src/components`
  - `packages/core/flow-engine/src/models`
- For client-v2 issues, inspect feature entry, route, state wiring, adapters, and rendering boundaries before changing shared infrastructure.
- For plugin issues, confirm whether the behavior is contributed by core flow models or plugin-side model registration.
- Check whether scene-specific filtering, model inheritance, or dynamic menu/item generation is involved before assuming a rendering bug.
- If behavior differs between v1 and v2, compare only the relevant implementation slice and extract the minimal missing behavior.

## URL And Task Handling

### If the user provides a URL

- Do not open a browser by default.
- Use the URL to identify the relevant page, route, scene, or plugin entry first.
- Open a browser only when the user explicitly asks for browser-based reproduction or verification, or when browser inspection is necessary to complete the task safely.
- Do not open the webpage inside VS Code.

### If the user provides a `taskid`

Use the project root `.env` to obtain `TEST_API_TOKEN`, then fetch task details with:

```bash
curl 'https://test_management.v2.test.nocobase.com/nocobase/api/tasks:get?filterByTk={taskid}' \
  -H 'authorization: Bearer {TEST_API_TOKEN}'
```

- Ignore unrelated media or video-processing details unless they are directly needed to understand the bug.
- If the fetched task description is ambiguous, confirm only the critical missing point.

## Validation Standard

- Prefer regression tests close to the changed code.
- If the bug is UI-only, prefer focused automated checks first; do manual browser verification only when the user explicitly asks for it or it is necessary to complete the task safely.
- Verify that the fix does not reopen nearby scene-specific behavior, especially across:
  - block types
  - popup modes
  - field interfaces
  - plugin-provided models
  - v1/v2 coexistence boundaries
- State residual risks explicitly if full verification is not possible.

## Output Format

When closing the task, include a bilingual change log in Chinese and English. Keep it concise and practical.

Use this shape:

```md
问题原因：
- ...

变更日志：
- fix: ...
- fix: 修复 ... 场景下的 ...

Cause:
- ...

Changelog:
- fix: ...
- fix: resolve ... in the ... scenario
```

## Working Rules

- Diagnose before patching.
- Prefer minimal, local changes over broad rewrites.
- Protect existing behavior.
- Keep the user informed of assumptions when they affect scope.
- If a test, command, or repro step cannot be completed, say so directly.
