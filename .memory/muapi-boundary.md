---
name: Muapi Boundary
description: Current trust, privacy, pricing, and self-hosting limits of the Muapi-backed client architecture
type: project
---

# Muapi Boundary

Current state:

- API keys are stored in browser localStorage by `components/StandaloneShell.js` and `components/ApiKeyModal.js` in the hosted path, and by the standalone app in the matching local storage key.
- The root client in `src/lib/muapi.js` and the shared studio client in `packages/studio/src/muapi.js` send `x-api-key` directly to `https://api.muapi.ai`.
- Muapi endpoints in use include generation endpoints under `/api/v1/{model-endpoint}`, `/api/v1/upload_file`, `/api/v1/predictions/{request_id}/result`, and `/api/v1/account/balance`.
- Generation, file upload, polling, and balance requests all go to Muapi.
- Self-hosting the current repo removes Muapi as the page host only if you deploy your own copy. It does not remove Muapi as the model backend or billing layer.
- Railway hosting adds app-hosting cost but does not lower model API cost for the current code path.

Privacy and trust:

- Keys remain in the browser unless the user enters them on a hosted page they do not control.
- Prompts, uploads, request IDs, polling, and output URLs still flow through Muapi for processing.
- Do not describe the current app as fully local or fully self-contained.
- Repo code reviewed here does not show built-in app telemetry in the Muapi client path, but the public `muapi.ai/open-higgsfield-ai` page currently exposes Google Tag Manager, Google Analytics, and Amplitude tokens in public HTML. Distinguish repo behavior from hosted page behavior.

Future direction:

- A cheaper or more private direct-provider path requires a new backend integration layer.
- Treat “replace Muapi” as a product and architecture project, not a deployment flag.

Common answer to self-hosting questions:

- “Self-hosting the current app gives you control over the UI host and source code, but the app still sends model requests to Muapi and still depends on Muapi pricing.”

**Why:** Security, hosting, and cost questions are easy to answer incorrectly if an agent assumes self-hosting means local model execution.

**How to apply:** Before answering privacy, security, Railway, or pricing questions, check whether the task changes only the UI host or also introduces a new backend. If there is no new backend, the Muapi trust and billing boundary still applies.
