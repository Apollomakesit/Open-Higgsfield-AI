---
name: Build and Deploy
description: Hybrid monorepo build order, app surfaces, and Railway deployment boundary
type: project
---

# Build and Deploy

Open Higgsfield AI has three connected layers:

- Root Next.js app in `app/` and `components/` for the hosted or self-hosted web UI.
- Shared React studio workspace in `packages/studio/src/` consumed by the Next.js app.
- Separate Vite and Electron app in `src/` and `electron/` for the desktop build.

The root Next.js app transpiles the shared `studio` workspace through `next.config.mjs`, which is why `packages/studio` changes affect the hosted web UI.

Build order:

1. `npm install`
2. `npm run build:studio` when `packages/studio` changes
3. `npm run build` for the root Next.js app
4. `npm run vite:build` only for the standalone Vite and Electron path

Non-standard scripts worth using:

- `npm run setup` = install plus `build:studio`
- `npm run vite:dev`
- `npm run vite:build`
- `npm run electron:build`
- `npm run electron:build:win`
- `npm run electron:build:all`

Railway deployment:

- Railway should deploy the root Next.js app only.
- Use `npm install`, `npm run build`, and `npm run start`.
- Do not include the Electron packaging flow in Railway plans.

Verification reality:

- No visible automated test suite or CI workflows were found.
- Verify with targeted manual checks and call out what was not exercised.

**Why:** Agents can easily confuse the root Next.js app, the shared studio workspace, and the standalone Vite and Electron app, which leads to edits in the wrong tree or the wrong build command.

**How to apply:** Decide the target surface before editing. If a task is about hosted web behavior, start in `app/`, `components/`, or `packages/studio/src/`. If it is about desktop packaging or the standalone shell, start in `src/` or `electron/`.
