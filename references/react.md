# React

Read when writing, reviewing or refactoring React or Next.js for performance. Ordered by impact — waterfalls and bundle size beat micro-optimisations every time.

## Waterfalls (biggest win)

- Independent awaits go in `Promise.all`. Three sequential fetches are three round trips.
- Defer `await` into the branch that needs it. Check a cheap synchronous condition before an async flag.
- Start independent work immediately and await later — especially in route handlers and server actions.
- Server components render sequentially: parallelise by composing, not by awaiting siblings in the parent.
- Split slow data behind a `<Suspense>` boundary so the shell paints immediately.

## Bundle

- Import from source, not barrel files. Icon and component libraries can cost 200–800ms per cold start through re-exports. In Next.js 13.5+, `optimizePackageImports` handles it — elsewhere, deep imports.
- Lazy-load heavy components (`next/dynamic`, `ssr: false` for client-only) and defer analytics, logging and error tracking until after hydration.
- Keep import paths statically analysable; a path built from a variable forces the bundler to include everything.
- Preload on intent — hover or focus — when the bundle is expensive.

## Server

- Authenticate and authorise **inside** every server action; they are public endpoints. Never rely on middleware or layout guards alone.
- Pass only the fields the client uses across the RSC boundary; serialisation is page weight. Derive in the client rather than sending both raw and transformed copies.
- No mutable module-scope state for request data — server renders run concurrently and it leaks between requests.
- `React.cache()` deduplicates per request; an LRU cache deduplicates across requests.
- Hoist static I/O — fonts, templates, config — to module scope so it loads once, not per request.
- `after()` for logging, analytics and cleanup, so the response is not blocked.

## Client and re-renders

- Derive state during render; never mirror props into state or set state in an effect for a derived value.
- Never define a component inside a component — it remounts on every render and destroys focus and state.
- Functional `setState` updates avoid stale closures and unstable callbacks.
- Lazy-init expensive state with a function.
- Narrow effect dependencies to primitives; subscribe to a derived boolean, not a continuously changing number.
- Put interaction logic in the event handler, not in state plus effect.
- Split a hook or effect that combines independent concerns so each recomputes alone.
- `useTransition` for non-urgent updates; `useDeferredValue` for expensive derived renders.
- Do not wrap a one-line primitive computation in `useMemo` — the hook costs more than the expression.
- Passive listeners for touch and wheel; one shared listener per event, not one per component.

## Rendering

- Animate the SVG **wrapper**, not the `<svg>` itself.
- `content-visibility: auto` for long lists; virtualise past ~100 rows.
- Hoist static JSX out of the render path.
- Stable keys, explicit image dimensions, `defer`/`async` on scripts.

## JavaScript

- Avoid layout thrashing: batch reads, then writes.
- `Set`/`Map` for repeated lookups; index maps instead of repeated `find`; one pass instead of chained iterations; `toSorted()` over mutating `sort()`; hoist `RegExp` construction.

## Before committing

Lint, accessibility, bundle size and architecture, in that order — then a regression check on what changed. If the project has its own check script, run it; never install a toolchain just to verify.

<!-- source: vercel-react-best-practices, react-doctor -->
