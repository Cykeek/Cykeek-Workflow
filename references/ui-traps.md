# UI traps

**Index** — Actions · Choice and overlays · Navigation and data · Motion. Read the section the routing row names, not the file.

Behaviour traps that hold in any app. Each has a stable ID that specs, reviews and checks cite, so a finding, a spec line and a failing check name one thing. A trap outranks the app's majority: when the app does it everywhere, the fix still wins and the conflict becomes a gate whose default is the fix. Add a trap only after it cost a real run. Every ID appears once.

A person describing a symptom rather than a component:

| They say | Look at |
| --- | --- |
| "laggy", "slow to respond" | `trap/press-delayed`, `trap/motion-input-lag`, `trap/motion-blocks-input` |
| "it jumps" | `trap/loading-layout-shift`, `trap/numbers-shift`, `trap/weight-shift` |
| "fine on my laptop, not my phone" | `trap/touch-hover-flash`, `trap/hover-unguarded`, `trap/touch-input-zoom`, `trap/viewport-height` |
| "I can't click it" | `trap/decor-pointer`, `trap/list-dead-gap`, `trap/exit-eats-input` |
| "the focus ring comes and goes" | `trap/hover-beats-focus`, `trap/open-trigger-unfocused`, `trap/disabled-drops-focus` |

## Actions

- `trap/button-div`: onClick on a div or span → render `button`, or `a` when it navigates.
- `trap/role-button`: `role="button"` on anything but `<button>` → same; Enter, Space and disabled must not be rebuilt by hand.
- `trap/native-control`: `<button>`, `<input>`, `<select>`, `<textarea>`, `<dialog>` outside the ui folder when the system has the component → use the system component.
- `trap/link-as-button`: an `<a>` with a background and padding, or a Button variant on a link → one element: a Button that renders as the link.
- `trap/link-wraps-button`: a link around a button, or the reverse → one element; two tab stops for one action is the bug.
- `trap/button-clone`: another element carrying the Button's classes → render the Button.
- `trap/button-icon-name`: an icon-only button with no accessible name → name it in every variant.
- `trap/button-type`: a button in a form with no `type` submits twice → default `type="button"`.
- `trap/button-label-wrap`: a label wrapping to two lines at 390px → `white-space: nowrap`, a shorter label, or a full-width button.
- `trap/control-height`: button, input and select side by side at different heights → one height token per size; measure, never read source (more than 1px apart fails).
- `trap/submit-repeat`: a pending submit accepts a second click or Enter → `aria-disabled` and an early return, focus kept.
- `trap/loading-label-swap`: `{saving ? "Saving..." : "Save"}` → keep the idle label, spinner inside the same box, `aria-busy` set.
- `trap/loading-layout-shift`: the box changes size between idle and pending → measure idle and pending; any change fails.
- `trap/disabled-drops-focus`: a focused control turns `disabled` while it runs and focus falls to `body` → `aria-disabled`, not `disabled`, while pending.
- `trap/disabled-still-hovers`: a disabled control still changes colour on hover → hover styles skip `[disabled]` and `[aria-disabled]`.
- `trap/hover-beats-focus`: hover removes a focus ring or border → state order is focus and invalid over hover.
- `trap/weight-shift`: font weight changes on hover or selection → show the state with colour, fill or an underline.
- `trap/focus-ring-shape`: the ring ignores the radius or is clipped by a parent's `overflow` → draw it with an outline that follows the radius.
- `trap/field-label`: a placeholder is the only name → a visible label bound to the control.
- `trap/label-unbound`: `<label>` with no `htmlFor` and no control inside → bind it; clicking it must focus the control.
- `trap/field-input-type`: a field with no `type`, `autocomplete` or `inputmode` → set all three per field kind.
- `trap/field-affix-focus`: an icon or prefix beside the input, not inside its box → put it inside the hit area, or make it its own named button.
- `trap/field-form-enter`: fields outside a `form`, so Enter does nothing → one `form`, one submit, native constraints passed through.
- `trap/field-error-link`: error text near the field but not tied to it → `aria-invalid` plus `aria-describedby`.
- `trap/field-error-timing`: errors on the first keystroke → first show on blur or submit, then live.
- `trap/date-local-midnight`: a date-only value passed through a midnight `Date` and shown or sent in another zone → keep it a `YYYY-MM-DD` string, or read and write it in UTC; test west of UTC.
- `trap/field-keeps-input`: a failed request clears what was typed → keep every entered value.
- `trap/required-everywhere`: a required marker on every field → mark the rarer of required and optional.
- `trap/touch-input-zoom`: input text under 16px on a phone → 16px at touch widths; never disable pinch zoom.
- `trap/zoom-disabled`: `user-scalable=no` or `maximum-scale=1` → remove it.
- `trap/touch-autofocus`: `autoFocus` on a page field → drop it on touch unless typing is the only job.

## Choice and overlays

- `trap/select-value-lost`: a select drops its value when the form errors or options reload → the value outlives errors and refetches.
- `trap/select-empty-value`: no way back to "none" in an optional field → a clear control or an explicit empty option.
- `trap/select-async`: "No results" flashes while the search runs → loading wins over empty.
- `trap/toggle-switch-submit`: a switch that only applies on Save → a switch acts at once; otherwise it is a checkbox.
- `trap/toggle-indeterminate`: "Select all" with no mixed state → three states, each defined.
- `trap/overlay-title`: a dialog with no title → a title part, visible or hidden.
- `trap/overlay-focus-return`: focus lands on `body` after close → return to the opener; if it is gone, to a named fallback.
- `trap/overlay-conditional-render`: `{open && <Dialog>}` → keep it mounted and pass `open`.
- `trap/overlay-destructive`: a destructive confirm that closes on an outside click → only Cancel and Escape dismiss it.
- `trap/overlay-pending-dismiss`: Cancel or Escape closes a dialog mid-submit → block dismissal while pending.
- `trap/overlay-no-max-height`: a dialog taller than a 320px screen with no scroll → `max-height: calc(100dvh - 2rem)` and `overflow-y: auto`.
- `trap/exit-eats-input`: a press during the exit animation reopens or is lost → the exiting layer takes no pointer input.
- `trap/escape-nested`: Escape closes the dialog behind an open menu → Escape closes the top layer only.
- `trap/open-trigger-unfocused`: an open menu's trigger looks like it does at rest → show the focus look while open.
- `trap/menu-navigation`: navigating items built as buttons → links, so new-tab works.
- `trap/menu-only-path`: an action reachable only by right-click or hover → a visible second path.
- `trap/menu-diagonal`: a submenu closes on a diagonal pointer path → a short delay or a safe area.
- `trap/list-dead-gap`: rows separated by margin, so the gaps do nothing → padding, so hit areas touch.
- `trap/tooltip-essential`: information only a tooltip holds → put the text on the page; a tooltip repeats it.
- `trap/tooltip-disabled-trigger`: a tooltip explaining a disabled button → visible text, or an `aria-disabled` button that stays focusable.
- `trap/tooltip-interactive`: a link inside a hover tooltip → text only; anything clickable is a popover.
- `trap/toast-errors`: an error that needs action shown only in a timed toast → persist it until closed or acted on.
- `trap/toast-timer`: the timer keeps running under the pointer or in a hidden tab → pause on hover, focus and hidden.
- `trap/live-region-double`: two layers announce one message → one announcing layer.

## Navigation and data

- `trap/tabs-routes`: tabs that change the URL built as a tablist → links with `aria-current`, or panels, not both.
- `trap/current-unmarked`: the current item looks different and is not marked → `aria-current` or `aria-selected`.
- `trap/narrow-hidden-nav`: at 390px nav links or columns sit past a clipped edge with no cue → a menu button, a wrap, or a fade at the edge; a gate lists each hidden item.
- `trap/link-cue`: a link in body copy with no underline and the text's colour → a resting cue; changing it is a gate listing every surface.
- `trap/table-divs`: a table made of divs → a table, or the grid role with its keyboard model.
- `trap/chosen-text-truncated`: a name or other text the user chose to identify something, cut with an ellipsis → wrap it; it never truncates.
- `trap/empty-value`: a blank cell, a typed dash here, "N/A" there → one primitive for empty values, announced as "None".
- `trap/numbers-shift`: changing numbers in proportional figures → `font-variant-numeric: tabular-nums`.
- `trap/color-only-status`: status told by colour alone → text or an icon carries it.
- `trap/text-measure`: reading text over 75ch or under 30ch per line → `max-width` in `ch` on reading columns.
- `trap/overflow-scrollbar`: `overflow: auto` on a box 1 or 2px short of its content → leave room, or clip.
- `trap/viewport-height`: `100vh` or `h-screen` on a shell or sheet → `dvh`, or `svh` when it must not change while scrolling.
- `trap/safe-area`: controls under the notch or home indicator → `env(safe-area-inset-*)` with `viewport-fit=cover`.
- `trap/scroll-chain`: scrolling a panel to its end scrolls the page → `overscroll-behavior: contain`, and lock page scroll under a modal.
- `trap/decor-pointer`: a glow or gradient layer above content takes clicks → `pointer-events: none` on decoration.
- `trap/touch-hover-flash`: hover styles apply on tap and stick → put them in `@media (hover: hover)`.
- `trap/hover-unguarded`: a `:hover` rule outside that media query that reveals content → guard it and give touch another path.
- `trap/touch-tap-highlight`: the tap highlight removed and no pressed style → a pressed state from pointer-down.
- `trap/press-delayed`: press feedback waits for release or an animation → feedback within 100ms of pointer-down.
- `trap/surface-double-edge`: a border and a shadow on one surface, undecided → border on resting surfaces, shadow on raised ones.
- `trap/weight-synthesized`: a weight token the loaded font files lack → tokens only name weights that exist; `font-synthesis: none` shows the gap.
- `trap/illustration-announced`: an HTML illustration read out as fragments → `role="img"` with a label, or `aria-hidden`.
- `trap/loader-small-dots`: a dot loader under 20px → a thin ring.
- `trap/gesture-drag`: a drag that drops when the pointer leaves or jumps on a second touch → pointer capture, ignore extra touches, decide release on speed.
- `trap/gesture-axes`: the element and the page both move on one swipe → set `touch-action` to the axis the element owns.

## Motion

- `trap/motion-transition-all`: `transition: all`, `transition-all` → name the properties, usually `transform` and `opacity`.
- `trap/motion-ease-in-enter`: an entrance with `ease-in` → enters ease out.
- `trap/motion-exit-slower`: an exit longer than its enter → exit shorter than enter.
- `trap/motion-overshoot`: a `cubic-bezier` with a value outside 0 to 1 on a routine change → keep inside 0 to 1.
- `trap/motion-linear`: linear easing on anything but progress and loops → ease out for enters, symmetric between two points.
- `trap/motion-frequent`: entrance motion on nav, list rows or primary actions → none, or first open only.
- `trap/motion-input-lag`: an element that follows the pointer through a curve → track the input each frame, carry release velocity.
- `trap/motion-blocks-input`: an entrance that ignores clicks until it ends → take input at once.
- `trap/motion-origin`: a menu grows from its own centre → grow from the trigger; exit reverses entry; dialogs stay centred.
- `trap/motion-restart`: a retrigger restarts from the start value → continue from the current value.
- `trap/motion-layout-property`: animating width, height, top, left or margin → `transform` and `opacity` only.
- `trap/reduced-motion-ignored`: animation under `prefers-reduced-motion: reduce` → drop movement, keep the fade that explains the state.
- `trap/loop-offscreen`: a loop that runs off screen or in a hidden tab → pause out of view.
- `trap/theme-transition`: a theme switch fires every hover transition → disable transitions for that frame.
- `trap/demo-on-timers`: a showcase demo stepping on `setTimeout` → wait on `getAnimations()` and `finished`.

How the file is used: a spec cites the IDs it must honour; a review cites the ID it saw; a check reuses the ID when it enforces one; a trap outranks the app's majority.

<!-- source: arla6ka/design.how (MIT) — build-design-system/references/traps.md -->
