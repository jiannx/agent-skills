# Verification Checklist

Run this list before declaring migration complete.

## Scope and Parity

- Every `must-have` capability from inventory is implemented in v2.
- Every dropped capability has explicit rationale.
- No silent behavior drift in default settings.

## Runtime Behavior

- Action runs successfully in intended flow scene.
- Handler output is consumable by downstream steps.
- Error paths surface clear user-facing messages.

## Settings and Persistence

- Settings form fields map to persisted params correctly.
- `beforeParamsSave` and `afterParamsSave` behaviors are validated.
- Existing saved configurations can be loaded without crash.

## Integration

- Plugin registers v2 actions/models on client load.
- Existing backend endpoints remain compatible.
- Permission behavior matches v1 expectations.

## Quality

- Locale keys are complete for changed UI text.
- No dead v1-only frontend code remains in active path.
- Manual smoke checks pass for add/edit/run flows.
