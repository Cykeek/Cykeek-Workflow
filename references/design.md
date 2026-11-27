# Design

**Index** — Gate · Card · UX · Direction · Style · Copy · Responsive · System. Read the sections the routing row names, not the file.

Read when building or reshaping a screen, choosing its direction, writing the copy, or reviewing a render. Motion decisions live in `motion.md`; the design-review protocol is `review.md` § Design review.

## Gate

Five answers, one line each, under the receipt, before the first layout decision. Answer 2 is never a colour or a font; answer 4 never ends in "clean and modern". The answers are written into the card below, not into the chat.

1. **Job** — what is sold, who buys it, the one action (Book a room, Start a free trial; a place with no transaction: Get directions). A silent brief gets a proposal and a confirmation, not a guess shipped.
2. **Hierarchy** — what the screen makes obvious first, then the next three, in order. Two things competing for first means there is no first.
3. **Flow and states** — where the user enters, the steps, where they leave; and loading, empty, error, permission, denied, success. A state left undesigned is one the user meets first; § UX owns the state rule.
4. **Origin and refusal** — the subject's world the look comes from: its trade, materials, vernacular. Name the three defaults refused from § Style, and what replaces them.
5. **Proof** — rendered at 320, 390, 768, 1024 and 1440; keyboard pass; contrast; every state; and the capture looked at, not the diff.

## Card

Twelve lines at the top of `DESIGN.md`. The gate is answered into it, and every return cites its version line the way the receipt cites the rules card. Replace it in place when the system changes; never append a second one.

```
Design card v1 · 2026-12-01
Kind        marketing page · dashboard · app · store · editorial
Sold        the one thing, in the buyer's words
Action      one label, used everywhere on the page
First       what the screen makes obvious first, then the next three
The one     the single memorable element, and where it sits
Palette     one accent, the neutral ramp, what is never used
Type        faces, the scale rule, what is never used
Refused     the three defaults refused, and what replaces them
Widths      320 390 768 1024 1440 — what changes at each; Android uses window classes, `platform.md` § Prove it
States      loading · empty · error · denied · success
Review      audit or critique; the reviewer is never the builder
```

A card that cannot fill a line names it `assumed: ...` rather than leaving it blank or reopening the gate mid-build.

## UX

A screen that is beautiful and unusable is a failed screen, and no type choice repairs it.

- **A job, not a page.** State the user's job in one sentence — "find last month's invoice" — and delete what does not serve it.
- **One main action a screen.** Secondary actions are quieter and visually subordinate. Two primary buttons means none.
- **Hierarchy by order, then weight, then space, then colour.** A box, a shadow or an accent stripe is the last resort, never the first move.
- **Group by meaning, not symmetry.** Proximity is the strongest grouping tool; a border is the weakest and costs the most.
- **Feedback on the press, not the release.** Anything clickable answers immediately; anything slow shows progress instead of freezing.
- **Undo over confirm.** Reserve confirmation for the irreversible, and use it rarely — a dialog people dismiss reflexively protects nothing.
- **Wayfinding.** Every screen answers where am I, where can I go, how do I get out. A dead end is a design failure, not a copy problem.
- **Names say what they do**, in the user's words: they manage notifications, not webhook config. Specific beats safe — "Progress", not "Home".
- **Real content is the test**: the longest name, the empty list, the thousandth row. Placeholder text hides the layout that breaks; § Responsive owns the edge rule and the worst-case fixture.

**Spend boldness in one place, then remove one accessory.** The memorable element is usually the subject itself, not a treatment applied to it.

- Open with the most characteristic thing in the subject's world — a photograph, the product, a live demo, the work, a headline.
- Structural devices — rules, numbering, borders, eyebrows — encode information or they go. Numbering only when the content is a sequence.
- One orchestrated motion moment beats scattered entrances (`motion.md` § Choreography). A page where every section fades up reads as a template.
- Type is design, not delivery: a display face doing one loud thing, or one family set with real weight and scale discipline.

**Structure — the screen.**

- **One section per screen.** The hero alone on the first screen; each section one or two screens tall at 1440×900; no second heading peeking in.
- **Equal left and right content edges.**
- **Every visual renders** — never an empty ground — at 1440, 1024, 768 and 390.
- **No caption or description under any image, render or visual.** Licence credits are one footer line.
- Photographs lead on product, service and place pages; software leads with its own UI in a real frame, never a generic dashboard.
- A stand-in photograph or icon is declared as one; never passed off as the client's own, and never carrying another brand's mark.
- Responsive is a design state, not a shrink: at 390 one column, buttons full width, a real menu, nothing clipped, 44px targets.

**Structure — the code.**

- Colocate what belongs together: component, tests, hook, types in one folder.
- Prefer composition over configuration. A card with six variant props is a card that should have been children.
- Keep a component to one job; split past ~200 lines.
- Separate data fetching from presentation: a container owns loading, error, empty and success; the leaf renders.
- Choose the smallest state tool that works: local state → lifted (2–3 siblings) → context (theme, auth, locale) → URL (filters, pagination) → server cache → global store. Never drill props deeper than three levels.

**Accessibility.**

- Every interactive element is a real `<button>`, `<a>` or `<input>`. A `div` with `onClick` is not a control.
- Every control has an accessible name: visible label, else `aria-label`. Icon-only buttons always carry one.
- Keyboard: logical tab order, visible focus, Escape closes overlays, focus moves into a dialog and returns to the trigger on close, and focus is trapped while it is open.
- Menus, dialogs, tabs, listboxes and comboboxes follow the WAI-ARIA APG pattern exactly — do not invent the semantics.
- Loading regions get `aria-busy`; status changes get `role="status"`.

**States.** Every async surface has five, and each is designed before the happy path is called finished — this section owns the state rule.

| State | Shows | Implementation |
| --- | --- | --- |
| Loading | a skeleton of the real layout, not a spinner over nothing | `aria-busy`, `role="status"` |
| Empty | what belongs here, why it is empty, the action that fills it | never error styling |
| Error | what failed, whether the data is safe, the retry | `role="alert"`, input kept |
| No permission | who can grant it, and how | no dead control, no silent failure |
| Success | the result, then the next step, in the verb the button used | an optimistic update ships with its rollback |

## Direction

- Fix the **kind** first, because it decides the rest: **software** (SaaS, AI, dev tool, fintech), **gaming**, **hardware**, **product** (any other physical thing), **service** (people are hired: clinic, firm, studio), **place** (people come to it: cafe, bakery, shop, gym).
- Distinctive choices come from the subject's industry, materials and vernacular — not from a style you like this week.
- **Plan in six lines**: kind; 4–6 named hex values with one sentence on why they fit the trade; typefaces and roles; layout concept and alignment; three principles that make this page this client's; the one thing.
- **Read the plan against the brief** before writing code. Anything that would come out the same for any similar page is a default, not a choice: revise it and say what changed.
- A reference is evidence, not a template: study two or three screens, record the structural decisions, rebuild them in this client's tokens and content. Never copy code, copy, imagery or a signature effect.
- Type: one family, or two clearly distinct. Tracking tightens as size grows (`-0.02em` at display, `0` at body); leading is inverse to size; measure under 80 characters; body 16px minimum, sized in `rem` so it scales with the user's setting.
- Colour: one accent, a neutral ramp that does the work, semantic names. Contrast and colour-alone rules are § System's.

## Style

§ System owns the token rule: the project's tokens are the source, never invented values. Only when the project has no system **and the user asks** for one: derive it once and write it into `DESIGN.md` — a screen that invents `13px` will invent the next one.

| Token | Derived from |
| --- | --- |
| Space | one unit (4 or 8px); every margin and padding a multiple |
| Type | a ratio between steps (1.125–1.333), 6–8 sizes, tracking and leading per step |
| Radius | one scale with hierarchy — controls, cards, containers — not one value everywhere |
| Colour | neutral ramp + one accent + semantic aliases; no raw hex in a component |
| Elevation | border first; one shadow for what truly floats; nothing else |

**The generated-looking defaults.** These read as AI whether or not they serve the brief. Each is legitimate somewhere; none is a choice when it appears regardless of subject.

- Cream `#F4F1EA` + high-contrast serif + terracotta accent; near-black + one acid-green accent; broadsheet hairline columns.
- The SaaS-card kit: every block an identical rounded card, one radius regardless of hierarchy, the same grey shadow, gradient washes as decoration; default violet/indigo; every corner maximally rounded; uniform card grids; generous padding everywhere; layered shadows.
- Template chrome: a letter-spaced ALL-CAPS eyebrow above every heading; meta strings joined with `·`; `WORD — fragment` labels; tinted near-black instead of black; monospace for small labels; `→` appended to every link.
- Typographic tells: one word of a headline in italic or a second colour; all-caps labels; a label above content that adds no meaning.
- Lorem ipsum and placeholder content where real data belongs.

## Copy

Words exist to make the screen usable. Bring the same restraint to them as to spacing.

- Active voice; a CTA states exactly what happens — "Save changes", not "Submit". One action keeps one name through the flow: Publish, then Published.
- Errors say what happened and what to do, in the interface's voice. No apology, no "Oops!", no exclamation mark. An empty screen is an invitation.
- Sentence case, plain verbs, no filler, one job per element. Cut rather than rewrite: a rewrite may only state what the screen already shows.
- Never invent a number, speed, customer or benefit you cannot point to in the code.

| Cut | Keep when |
| --- | --- |
| Eyebrow labels, pills, chips above headings | never |
| Decorative numbering, `01 / 02` | the list is a real sequence — steps, install, orders |
| A subtitle under every heading; reassurance footnotes | it states a concrete fact the screen does not |
| Helper text narrating what is visible | a constraint the user cannot guess |
| Badges, status dots, pulsing dots | bound to real data or real state |
| Emojis and icons in headings, KPIs, feature lists | it is the control itself, or carries meaning the text cannot |
| Edge accent stripes on cards | it encodes real state — alert, error, warning |
| `→` on links and buttons | it leaves the site, or is icon-only |
| "Click here", "Learn more", "Read more" | — name the destination or the result |
| A legend where a direct label fits | more than four series, or marks too close to label |

Charts: the title names the metric, not the chart type; one encoding per quantity — a bar, its printed value, a gridline and a tick saying the same thing is three too many; scaffold never competes with data. Headings and card titles inside an app stay at five words or fewer.

## Responsive

Mobile first, then expand. Verify at 320, 390, 768, 1024 and 1440. This section owns the edge rule: worst-case data renders at every width before the layout is done.

- `min-width: 0` on flex and grid text columns, `minmax(0, 1fr)` in grids, `overflow-wrap: anywhere` on emails and URLs, `flex-shrink: 0` on avatars and icons, `tabular-nums` on changing numbers.
- Long words, one-letter names, missing fields, counts at exactly 1, empty lists and 1,000+ rows are the states that ship broken. Render at least one of each before calling the layout done.
- 200% zoom is a layout test: text grows, boxes do not.

**On a phone.**

- `100vh` is wrong on mobile browsers — use `100dvh` or `svh`.
- Sticky hover: wrap hover styles in `@media (hover: hover) and (pointer: fine)`, or a tap leaves the state stuck.
- Kill the tap flash with `-webkit-tap-highlight-color: transparent`; set `touch-action` on drag axes.
- Inputs must be 16px or larger, or iOS zooms the page on focus.
- Respect the notch: `env(safe-area-inset-*)` on fixed bars.
- Prevent long-press selection on controls: `-webkit-user-select: none` where it is not text.

**Stress it before shipping.** Take the component's values and their real limits from the schema, the migration or the API type. Build one worst-case fixture beside the demo one, enter it at the same data boundary (never by editing markup), and put a dev-only **Demo / Worst case** toggle where it can be flipped. Then check the failure signatures:

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

**Before you say done.** Run the project's own commands — lint, type-check, test, build — in the order its `package.json` or `Makefile` gives. Install nothing to verify: a toolchain added for one check is a dependency every later session pays for. Where none exists, review against this list and name what you could not run.

- [ ] renders with no console error
- [ ] every control reached by Tab, in order, with a visible focus ring
- [ ] screen reader conveys the structure — headings, landmarks, labels
- [ ] correct at 320, 390, 768, 1024 and 1440
- [ ] loading, empty, error and permission states exist where they apply
- [ ] the project's tokens, not invented values
- [ ] the render looked at, not just built — and the deltas named
- [ ] Android work: not this file's phone rules — window classes, dp and insets are `platform.md` § Android; 44px targets, sticky hover and `100vh` do not transfer

## System

The project's tokens are the source — this section owns that rule (§ Style derives a system only where none exists and the user asks). Use the project's tokens for spacing, colour and type. No arbitrary `13px` or `2.3rem`; no raw hex where a semantic token exists.

- One `h1` per page, no skipped heading levels, no heading styles on non-headings.
- Contrast 4.5:1 for body, 3:1 for large text. Never colour as the only signal — pair it with text, icon or shape.

<!-- source: merged from design.md and ui-engineering.md — their sources: frontend-design, design-taste-frontend, unslop-ui, web-design-guidelines, checklist-design, frontend-ui-engineering, emil-design-eng, apple-design, mobile-native, break-ui -->
