# QA-Extension

A React 18 and Vite 4 scaffold for a QA portal: file-based routing, a protected layout and a production asset pipeline. The QA screens themselves are placeholders.

## The problem

A QA dashboard usually needs routing, layouts and build tooling before any test-related UI can exist. This repo is that foundation. It is meant to be the starting point for the portal, not a working test runner.

## What it does

Today the app renders one placeholder screen. The parts that are real:

- **File-based routing**: `src/router/Router.tsx` builds routes from `src/screens` with `import.meta.glob`. Filename suffixes select lazy-loaded (`.lazy`) and protected (`.protected`) route variants.
- **Protected layout**: `src/layouts/protected.jsx` renders its child routes only when a user is set, and redirects to `/` otherwise. The user is currently hard-coded to `null`, so nothing gets through yet.
- **Route loaders and actions**: `src/router/loader.tsx` and `src/router/action.tsx` resolve a screen module's exported `loader` or `action` lazily, so data-loading functions live next to the screen they serve.
- **Per-route metadata**: `src/router/Head.tsx` sets page titles and meta tags with `react-helmet`.
- **Barrel exports**: `export.config.ts` uses `vite-plugin-hot-export` to generate index files for `src/components` and `src/assets`.
- **Build-time asset optimization**: `vite.config.ts` adds chunk splitting, Brotli compression, image minification and a webfont download plugin.

## How it's built

- **Stack**: React 18, React Router 6.30, Vite 4, TypeScript, SCSS, and Prettier.
- **Path aliases**: `components`, `assets` and `router` are aliased in `vite.config.ts`, so imports read as package names. A `global` alias also exists, but its folder is not in the repo yet.
- **Decision: routes are discovered from file names.** A new screen is a new file, and no route table has to be edited. The cost is that the naming convention is the real API, and a misnamed file silently becomes a route or gets skipped.

## Running it locally

Requires Node.js 18 or later.

```bash
pnpm install       # or: npm install
pnpm dev           # Vite dev server at http://localhost:5173
pnpm build         # production build
pnpm preview       # serve the production build locally
pnpm formate       # Prettier
```


## What I learned and what I'd do next

- **The dashboard is not built yet.** The current screen says only "index". The test-run console and reporting views would be the real work.
- **No auth source.** The protected layout has nothing to check against. A session or token source needs to set the user.
- **A dead script.** `npm start` points at `build/server.js`, which is not in the repo.
- **No tests.** Route discovery and the protected redirect are the first candidates for tests.
- **Dependencies to audit.** Several Vite plugins are used only in the build config. Check which ones are still needed as the app grows.

## Author

Created by [Mehfooz-ur-Rehman](https://github.com/MehfoozurRehman).
