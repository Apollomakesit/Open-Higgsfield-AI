# Project Guidelines

These instructions are automatically applied to all Copilot interactions in this repository.

## Architecture

- This repo has two app surfaces. The hosted or self-hosted web UI is the root Next.js app under `app/` and `components/`. The separate standalone desktop path is the Vite and Electron app under `src/` and `electron/`.
- The shared React studio implementation lives in `packages/studio/src/` and is mounted by `components/StandaloneShell.js` at `/studio`.
- Decide which surface a change targets before editing. Do not mix the root Next.js web shell with the standalone Vite and Electron app.
- Model and transport logic is duplicated across `packages/studio/src/{models,muapi}.js` and `src/lib/{models,muapi}.js`. Keep both sides aligned unless the change is intentionally surface-specific.

## Build And Verify

- Non-standard commands in this repo:
  - `npm run build:studio` builds the shared `packages/studio` workspace.
  - `npm run setup` installs dependencies and then runs `build:studio`.
  - `npm run vite:dev` and `npm run vite:build` are for the standalone Vite app, not the root Next.js app.
  - `npm run electron:build`, `npm run electron:build:win`, and `npm run electron:build:all` package the Vite build for Electron.
- Railway deployment targets the root Next.js app only: `npm install`, `npm run build`, and `npm run start`.
- There is no visible automated test suite or CI workflow in this repo. Validate with the lightest relevant manual check and state what you could not verify.

## Muapi Boundary

- The current architecture is BYOK and client-side. API keys are stored in browser localStorage and requests are sent directly to `https://api.muapi.ai`.
- Self-hosting this repo changes where the UI runs, not who processes generations or bills model usage. Do not claim that Railway deployment lowers model API costs or removes Muapi from the trust boundary.
- Do not repeat README privacy language as fully local. Keys stay in the browser, but prompts, uploads, polling, and generated outputs still flow through Muapi.
- Treat direct-provider integrations as a future backend project, not a configuration tweak.

## Navigation

- Read `MEMORY.md` before broad exploration.
- Use `/.memory/change-map.md` to pick the first files to inspect for a task.
- Use `/.memory/muapi-boundary.md` before answering security, privacy, hosting, or cost questions.
