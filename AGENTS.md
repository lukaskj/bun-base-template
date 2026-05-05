# AGENTS.md

## Repo Shape
- Single-package Bun + TypeScript repo. Main source entrypoint: `src/index.ts`.
- Do not edit `dist/`; build output is generated and gitignored.
- Tests live under `test/`. Bun test root is pinned there by `bunfig.toml` and always preloads `test/test-setup.ts`.

## Commands
- Use Bun, not npm. Engines in `package.json`: Node `>=23`, Bun `>=1.2.22`.
- `bun run dev` runs `src/index.ts` directly.
- `bun run test` loads `.env.test`: `bun test --env-file .env.test`.
- Single test file: `bun test --env-file .env.test test/<file>.spec.ts`.
- `bun run lint` is not read-only: it runs `tsc -p . --noEmit && biome lint --write` and can modify files.
- `bun run format` runs `biome format --write`.
- Prefer `bun run build:all` for publishable output. It deletes `dist/`, emits declarations, then builds JS bundles.
- `bun run build` only rebuilds JS bundles; it does not clean `dist/` or refresh `.d.ts` files.

## Build / Package Wiring
- Package outputs expected by `package.json`: `dist/index.mjs`, `dist/index.cjs`, `dist/index.d.ts`.
- `scripts/build.ts` builds `src/index.ts` twice with Bun (`esm` and `cjs`) and renames emitted `.js` entrypoints to `.mjs` / `.cjs`.
- Type declarations come from `tsconfig.build.json`, not Bun build.

## Hooks / CI Gotchas
- Pre-commit hook runs `bunx lint-staged` and then full `bun run test`.
- `lint-staged` for `*.{ts,tsx}` runs `bun format` then `bun lint`; both may rewrite files before commit.
- Publish workflow is tag-driven only: `.github/workflows/publish.yml` runs on `v*.*.*` tags.
- Publish workflow uses `bun ci --ignore-scripts`, `bun run build:all`, `bun audit signatures`, then plain `bun test`, and finally `bun publish`.
- Local test script uses `.env.test`; publish workflow uses plain `bun test`. If new tests need env vars, update workflow or keep them runnable without `--env-file`.

## Verified Scope
- Biome only includes `src/**/*.ts`, `test/**/*.ts`, and `scripts/**` from repo config. Changes outside those paths will not be covered by `bun run lint` / `bun run format`.
