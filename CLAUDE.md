# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Project

Arcade Vault is an online platform for playing games and competing for the highest score.

Stack: Next.js 16.4 (App Router, `app/` only), React 19.3, TypeScript strict, Tailwind v4. Import alias `@/*` maps to the repo root.

## Commands

- `npm run dev`: dev server on http://localhost:3000 (Turbopack)
- `npm run build` / `npm run start`: production build and serve
- `npm run lint`: ESLint flat config (`eslint.config.mjs`, extends `eslint-config-next` core-web-vitals + typescript). Lint one file with `npx eslint app/page.tsx`
- `npx tsc --noEmit`: type-check (there is no script for it)

No test framework is configured yet. If you add one, see `node_modules/next/dist/docs/01-app/02-guides/testing/`.

## Spec Driven Design

Work follows **Spec Driven Design** using the `/spec` and `/spec-impl` skills from [Klerith/fernando-skills](https://github.com/Klerith/fernando-skills) (install them with `npx skills@latest add Klerith/fernando-skills`). Write or update the spec before implementing features.

- Specs live in `specs/NN-slug.md`, numbered sequentially (`01-`, `02-`…), and `specs/.spec-config.yml` holds the skill settings (`AutoCreateBranch`).
- `/spec-impl NN` implements a spec on a branch named `spec-NN-slug`.
- Both skills set `disable-model-invocation`, so the user has to invoke them. Don't try to run them yourself.
- New specs must match the language and wording of the existing ones. The README is in Spanish.

## Non-default configuration

- `next.config.ts` turns on features that change how pages render and cache. Read the bundled docs before writing data-fetching or routing code:
  - `cacheComponents: true`: Cache Components model (`"use cache"`, static shell + streaming). See `01-app/01-getting-started/08-caching.md` and `02-guides/migrating-to-cache-components.md`. Guides without "cache-components" in the name may describe the legacy model.
  - `partialPrefetching: true`: see `02-guides/adopting-partial-prefetching.md`.
  - `experimental.agentFeedback: true`: this is why `next dev` rewrites the block in `AGENTS.md`.
- Tailwind is wired through a **Turbopack loader rule** (`@tailwindcss/turbopack` on `*.css`), not PostCSS, and there is no `postcss.config`. Theme tokens are declared with `@theme inline` in `app/globals.css`.
- Layouts and pages use the globally generated route types (e.g. `LayoutProps<"/">`, `PageProps<...>`), which come from `.next/types`. Run `dev` or `build` once if those types are missing.
