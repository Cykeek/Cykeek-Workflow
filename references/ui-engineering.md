# UI engineering

Read when building or changing an interface: layout, components, state, accessibility, phones, and the data that breaks them.

## Structure

- Colocate what belongs together: component, tests, hook, types in one folder.
- Prefer composition over configuration. A card with six variant props is a card that should have been children.
- Keep a component to one job; split past ~200 lines.
- Separate data fetching from presentation: a container owns loading, error, empty and success; the leaf renders.
- Choose the smallest state tool that works: local state → lifted (2–3 siblings) → context (theme, auth, locale) → URL (filters, pagination) → server cache → global store. Never drill props deeper than three levels.

## The project's system, not yours

- Use the project's tokens for spacing, colour and type. No arbitrary `13px` or `2.3rem`; no raw hex where a semantic token exists.
- One `h1` per page, no skipped heading levels, no heading styles on non-headings.
- Contrast 4.5:1 for body, 3:1 for large text. Never colour as the only signal — pair it with text, icon or shape.

**The generated look**, avoid all of it: default violet/indigo, gradients as decoration, every corner maximally rounded, uniform card grids, generous padding everywhere, layered shadows, lorem ipsum. Real content reveals the layout problems placeholder text hides.

## Accessibility

- Every interactive element is a real `<button>`, `<a>` or `<input>`. A `div` with `onClick` is not a control.
- Every control has an accessible name: visible label, else `aria-label`. Icon-only buttons always carry one.
- Keyboard: logical tab order, visible focus, Escape closes overlays, focus moves into a dialog and returns to the trigger on close, and focus is trapped while it is open.
- Menus, dialogs, tabs, listboxes and comboboxes follow the WAI-ARIA APG pattern exactly — do not invent the semantics.
- Loading regions get `aria-busy`; status changes get `role="status"`.

## Layout and responsiveness

- Mobile first, then expand. Verify at 320, 390, 768, 1024 and 1440.
- `min-width: 0` on flex and grid text columns, `minmax(0, 1fr)` in grids, `overflow-wrap: anywhere` on emails and URLs, `flex-shrink: 0` on avatars and icons, `tabular-nums` on changing numbers.
- Long words, one-letter names, missing fields, counts at exactly 1, empty lists and 1,000+ rows are the states that ship broken. Render at least one of each before calling the layout done.
- 200% zoom is a layout test: text grows, boxes do not.

## On a phone

- `100vh` is wrong on mobile browsers — use `100dvh` or `svh`.
- Sticky hover: wrap hover styles in `@media (hover: hover) and (pointer: fine)`, or a tap leaves the state stuck.
- Kill the tap flash with `-webkit-tap-highlight-color: transparent`; set `touch-action` on drag axes.
- Inputs must be 16px or larger, or iOS zooms the page on focus.
- Respect the notch: `env(safe-area-inset-*)` on fixed bars.
- Prevent long-press selection on controls: `-webkit-user-select: none` where it is not text.

## States

Every async surface has five, and each is designed before the happy path is called finished.

| State | Shows | Implementation |
| --- | --- | --- |
| Loading | a skeleton of the real layout, not a spinner over nothing | `aria-busy`, `role="status"` |
| Empty | what belongs here, why it is empty, the action that fills it | never error styling |
| Error | what failed, whether the data is safe, the retry | `role="alert"`, input kept |
| No permission | who can grant it, and how | no dead control, no silent failure |
| Success | the result, then the next step, in the verb the button used | an optimistic update ships with its rollback |

Test with the worst realistic data, not the demo row.

## Stress it before shipping

Take the component's values and their real limits from the schema, the migration or the API type. Build one worst-case fixture beside the demo one, enter it at the same data boundary (never by editing markup), and put a dev-only **Demo / Worst case** toggle where it can be flipped. Then check the failure signatures:

| What you see | Cause | Fix |
| --- | --- | --- |
| Avatar squashed to an oval | flex child shrinking | `flex-shrink: 0` |
| Text overflows instead of wrapping | `min-width: auto` | `min-width: 0` |
| Email runs past the edge | no break opportunity | `overflow-wrap: anywhere` |
| Trailing action pushed off-screen | middle column too greedy | constrain the middle, keep actions fixed |
| Row height jumps | line clamp missing | clamp to a fixed number of lines |
| "1 members" | no plural handling | `Intl.PluralRules` |
| Layout collapses at 200% zoom | fixed px containers | size in rem and let boxes grow |

Report the breaks before fixing them — several are design decisions, not bugs.

## Before you say done

Run the project's own commands — lint, type-check, test, build — in the order its `package.json` or `Makefile` gives. Install nothing to verify: a toolchain added for one check is a dependency every later session pays for. Where none exists, review against this list and name what you could not run.

- [ ] renders with no console error
- [ ] every control reached by Tab, in order, with a visible focus ring
- [ ] screen reader conveys the structure — headings, landmarks, labels
- [ ] correct at 320, 390, 768, 1024 and 1440
- [ ] loading, empty, error and permission states exist where they apply
- [ ] the project's tokens, not invented values
- [ ] the render looked at, not just built — and the deltas named
- [ ] Android work: not this file's phone rules — window classes, dp and insets are `android.md`; 44px targets, sticky hover and `100vh` do not transfer

<!-- source: frontend-ui-engineering, web-design-guidelines, mobile-native, break-ui, emil-design-eng -->
