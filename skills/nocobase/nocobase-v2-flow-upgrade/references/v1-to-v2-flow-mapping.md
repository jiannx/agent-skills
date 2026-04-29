# v1 to v2 Flow Mapping Guide

## Goal

Implement v1 plugin frontend behavior in v2 FlowModel architecture with minimal backend changes.

## Mapping Rules

1. Map each v1 capability to one primary v2 owner.
- `Flow action` for executable behavior.
- `Model` for state orchestration and composition.
- `uiSchema` for settings form and parameter editing.
- `hooks/resource` for API access and data shaping.

2. Prefer models-based plugin structure.
- Old version typically centers on `src/client/` components and schema settings.
- New version should place migration logic under `src/client/models/`.

3. Keep runtime and settings hooks explicit.
- Put pre-save normalization in `beforeParamsSave`.
- Put side effects after persist in `afterParamsSave`.
- Keep handler return shape stable for downstream steps.

4. Preserve API contracts.
- Reuse existing server endpoints and payload shape whenever feasible.
- If payload shape must change, document and gate change explicitly.

## Mapping Table Template

| Capability ID | v1 implementation (file/symbol) | v2 target (file/symbol) | Type (`action`/`model`/`uiSchema`/`hook`) | Status | Risk |
| --- | --- | --- | --- | --- | --- |
| F-01 |  |  |  | todo |  |

## Flow-Centric Implementation Checklist

- Define action with `defineAction` and explicit `scene`.
- Provide `uiSchema` for all user-configurable params.
- Normalize params in `beforeParamsSave` if persistence format differs from UI values.
- Return deterministic result shape from `handler`.
- Register action in plugin client bootstrap.
- Add locale keys for every new label/message.
