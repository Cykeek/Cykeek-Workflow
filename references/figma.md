# Figma

Read before touching any Figma link. The file is the source of truth; the screenshot is the target; neither is an asset for the build.

## Gate

Check the catalog for Figma MCP tools. Absence is not proof — lazily attached servers get one bounded attempt with a known name first.

If they are genuinely absent, say what to add — the Figma MCP server in the client's MCP configuration — and end the turn. No scaffolding, no approximation from a screenshot, no partial work. The design source is missing, so everything downstream would be invention.

Then, once per session, enumerate the Figma tools and record four roles: read nodes, render a screenshot, download assets, write nodes. Honour any tool whose description declares a mandatory companion skill. Write the map down; re-deriving it every turn is the token leak.

## Read the design

- **Screenshot first.** Request the render in the initial design call. If none comes back, call the screenshot tool directly before editing. That render, not the node tree, is what the build is compared against.
- **Sparse is not a result.** A response flagged sparse cannot be implemented from. Correlate its child node IDs against the screenshot, then request the visible children in **one parallel batch**.
- **Traverse to the leaves.** Resolve the link to its frame, enumerate children, recurse. Mark every node covered, skipped with a reason, or missing. Done means the ledger accounts for every visible node — "I got the main sections" is a failed traversal.
- **Past the first viewport.** Long frames continue below the fold. Overlapping and scroll-revealed nodes are nodes. Variant states — hover, focus, empty, error — are nodes.
- **Never re-request a node already in the ledger.** Batch independent reads: siblings, assets and screenshots together. Cost here is round trips, not payload.

The ledger is a `## Nodes` block at the top of `.cykeek/verify/<task>.md`:

```
frame: Dashboard 1440x1024
- [x] Header / nav            3 nodes
- [x] KPI row                 4 cards
- [ ] Chart panel             sparse -> 12 children requested
- [~] Footer                  not in this design, skipped
```

## Build

- **Inspect before editing.** Read the likely project paths and the installed design dependencies for matching components, assets and tokens *first*. Reuse or compose a match; write raw markup, inline styles or hardcoded values only when nothing can express the design.
- **Adapt, don't paste.** Returned code is a high-fidelity non-interactive prototype. Translate absolute positioning into the project's layout system unless fixed positioning is the only faithful option, and make the interactive parts interactive.
- **Code Connect** applies precisely at its mapped node: reuse the connected component unless it truly cannot be configured or extended. Styling effort is not an exception.
- **Assets are exact.** Every visible static asset — image or SVG — goes in the exact slot it occupies in the design, substituted only for an exact match already in the codebase. Never omit, redraw, extract paths from, or inline one. Keep API- and data-supplied imagery dynamic.
- **Download every asset** as the design response describes, so no temporary Figma URL survives in the code. Do not reach for other tools to download. Supplied SVGs carry root `width`/`height` that must not be overridden when changing wrappers — no blanket `100% × 100%`. Avoid reading asset bytes unless the task requires it.
- **Build in dependency order**: tokens, primitives, components, shell, screen.

## Verify

- Verify only the requested screen or component. Record — do not alter — pre-existing mismatches outside it.
- Every visible asset is checked four ways: a non-empty local file, the right design slot, the right callsite, and correct rendered geometry. One mismatched or misproportioned asset is a FAIL.
- Compare against the screenshot render, add a row per frame or section to `.cykeek/verify/<task>.md`, and iterate from the first case until all read PASS or the user approves the remainder.

<!-- source: figma-design-to-code, figma-use -->
