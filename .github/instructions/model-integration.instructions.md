---
name: "Model Integration Guidelines"
description: "Use when adding or editing Muapi model definitions, endpoint mappings, imageField or videoField quirks, polling behavior, or generation payload logic in the Open Higgsfield model registry and API clients."
applyTo:
  - "packages/studio/src/models.js"
  - "packages/studio/src/muapi.js"
  - "src/lib/models.js"
  - "src/lib/muapi.js"
---
# Model Integration Guidelines

- This repo is currently Muapi-backed. Self-hosting the app does not replace Muapi or lower model API costs.
- `endpoint` is the Muapi API path. Do not assume it matches the model `id`.
- Preserve per-model field quirks such as `image_url`, `images_list`, or model-specific video fields.
- When changing request or response handling, keep `packages/studio/src/muapi.js` and `src/lib/muapi.js` aligned unless the change is intentionally surface-specific.
- Keep result normalization compatible with `result.outputs?.[0] || result.url || result.output?.url`.
- Long-running video and lipsync flows use longer polling windows than image flows. Do not collapse them to image timeouts.
- When adding a model, verify both the registry entry and the matching client payload field.
