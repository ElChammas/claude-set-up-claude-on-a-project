# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview
A small Express API (users + health endpoints, in-memory state) used as the starter project for the Claude Code course. Later course levels build on this repo.

## Commands
- `npm run dev` — start the API in watch mode at http://localhost:3000 (`PORT` overrides)
- `npm test` — run all tests with the built-in Node test runner (`node --test`)
- `node --test tests/users.test.js` — run one test file
- `node --test --test-name-pattern="POST /users"` — run tests whose name matches
- `npm run lint` — ESLint (`eslint:recommended`, CommonJS/script mode)

CI (`.github/workflows/ci.yml`) runs `npm run lint` then `npm test` on Node 22 for every push and PR; both must pass.

## Conventions
- CommonJS only (`require`/`module.exports`); no ES modules, no TypeScript. ESLint parses files as `sourceType: "script"`, so `import` will fail lint.
- One route module per resource under `routes/`, mounted in `server.js`. Routes never touch data directly — go through the helper functions in `db/store.js`.
- Error responses use the shape `{ error: "<message>" }` with the matching status code (400 validation, 404 not found).
- Tests stay at the HTTP layer: `node:test` + `node:assert` + `supertest` against the exported `app`. No other test frameworks or mocking libraries.
- When endpoints or validation change, update `tests/users.test.js` to match.
- Preserve existing API behavior unless the task asks for a change. Don't add dependencies unless the task requires them.

## Architecture
- `server.js` builds the app and exports it; it only calls `app.listen` when run directly (`require.main === module`). Tests rely on this — keep the export and the guard.
- `db/store.js` holds users in a module-level array seeded with two users (ids 1 and 2); `nextId` starts at 3. State resets on server restart, but within one test process it is shared, so users created in one test are visible to later tests. Route params are strings — convert with `Number()` before lookups, since the store compares ids with `===`.

## Safety
- Don't read or edit `.env`; `.env.example` shows the expected variables. `.claude/settings.local.json` is personal and git-ignored.
