# Libraries

Read before hand-rolling something a library already does well, or when the project already uses one.

## Choose before you build

A toast, drawer, dropdown, dialog, combobox or command menu is weeks of edge cases — focus trapping, escape handling, scroll lock, collision-aware positioning, interruptible motion. Use the library. Hand-rolling is how a `<div>` dropdown with no focus management ships.

- Default primitives: Radix or Base UI. Default styling layer: shadcn/ui. Default sheet: Vaul.
- Choose one system per project and stay in it. Adding a second component library doubles the tokens, the bundle and the inconsistency.
- The project's existing components win over any of this. Check before installing anything.

## shadcn/ui

- It is a **registry**, not a dependency: components are copied into your repo and then belong to you. Edit them freely; do not expect upstream updates.
- `components.json` defines aliases, style and tokens. Read it before adding anything.
- Add with the CLI so paths, aliases and dependencies stay consistent. Do not hand-copy from docs.
- Style through CSS variables and Tailwind tokens, not by patching every component. If you must patch, patch the one component that needs it.
- A design system or `DESIGN.md` maps onto the token layer first — colour, radius, spacing — and only then onto components.

## Toasts (Sonner)

- Mount `<Toaster />` once, at the root. Two toasters means duplicated toasts.
- `toast()` for a message; `toast.promise()` for async flow; `toast.loading()` then update by id rather than stacking messages.
- Give every toast a stable `id` when it can fire repeatedly, and `dismiss()` it explicitly.
- Toast styling comes from the Toaster's props and tokens — portal and z-index problems are why a toast hides behind a modal.
- Keep toast copy short and past tense; it reports, it does not explain.

## Vaul (sheets and drawers)

- Use it for any bottom sheet or drawer gesture. It encodes velocity-aware dismissal, overlay fading tied to drag progress, and the iOS timing curve.
- Mark interactive children `data-vaul-no-drag`.
- A small floating drawer overrides the transition to ~200ms; the 500ms iOS timing only reads right full-width and edge-attached.

## ui-sdk

When a project depends on `@ui-sdk/*`, compose its components rather than recreating behaviour it ships; when it lacks one, build the behaviour on `@ui-sdk/core` and keep accessibility, keyboard, focus and RTL intact. Check its documented patterns before inventing a local one.

<!-- source: shadcn, ask-sonner, pick-ui-library, ui-sdk -->
