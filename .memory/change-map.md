---
name: Change Map
description: Task-to-file lookup for common Open Higgsfield changes across web, studio, transport, models, and Electron
type: project
---

# Change Map

Start planning in this order when the task touches product behavior:

1. Hosted shell in `components/StandaloneShell.js`
2. Shared studio package in `packages/studio/src/components/`
3. Muapi client in `packages/studio/src/muapi.js`
4. Model registry in `packages/studio/src/models.js`
5. Deploy boundary in `package.json`, `next.config.mjs`, and `vite.config.js`

Task-to-file map:

- Hosted web tabs, API-key modal, top-level balance, or `/studio` page:
  - `components/StandaloneShell.js`
  - `components/ApiKeyModal.js`
  - `app/studio/page.js`
  - `app/page.js`
- Shared React studio behavior used by the hosted web app:
  - `packages/studio/src/components/ImageStudio.jsx`
  - `packages/studio/src/components/VideoStudio.jsx`
  - `packages/studio/src/components/LipSyncStudio.jsx`
  - `packages/studio/src/components/CinemaStudio.jsx`
  - `packages/studio/src/index.js`
- Model catalog, endpoint mapping, and per-model input quirks:
  - `packages/studio/src/models.js`
  - `src/lib/models.js`
- API transport, polling, uploads, result normalization, and balance:
  - `packages/studio/src/muapi.js`
  - `src/lib/muapi.js`
- Standalone Vite and Electron UI:
  - `src/main.js`
  - `src/components/*.js`
  - `src/style.css`
  - `styles/*.css`
- Desktop wrapper and packaging:
  - `electron/main.js`
  - `afterPack.js`
  - `package.json`
- Web build and deployment config:
  - `package.json`
  - `next.config.mjs`
  - `vite.config.js`
  - `packages/studio/package.json`
- Local persistence and resume behavior:
  - `src/lib/pendingJobs.js`
  - `src/lib/uploadHistory.js`
  - `src/components/*Studio.js`
- Styling:
  - Hosted and shared React path: `app/globals.css`, `packages/studio/src/tailwind.css`
  - Standalone path: `src/style.css`, `styles/global.css`, `styles/studio.css`, `styles/variables.css`

When parity matters:

- The hosted React studio and the standalone Vite and Electron app are separate trees.
- If a task changes shared model or Muapi behavior, inspect both `packages/studio/src/` and `src/lib/`.
- If a task is clearly hosted-web-only, start with the root Next.js and shared studio files before touching the Vite and Electron tree.

**Why:** Agents lose time in this repo when they search the whole workspace instead of picking the correct surface first.

**How to apply:** Map the request to one of the groups above, read those files first, and only search wider if the local files do not explain the behavior.
