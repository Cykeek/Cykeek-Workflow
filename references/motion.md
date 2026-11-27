# Motion

Read when a task adds, changes or reviews animation. Work in this order — the gate comes before the curve, every time. A page whose timeline is the wheel is `scroll.md`, not this file; a reference the user wants copied — a video, a gif, a still, a link — is `motion-clone.md`, which measures it instead of choosing it. This file still answers first: a clone that should not animate is a clone of a mistake.

## 1. Should it animate at all

| Frequency | Decision |
| --- | --- |
| 100+×/day — keyboard shortcuts, command palettes | **Never.** Stop. |
| Tens/day — hover, list navigation | Near-imperceptible or nothing |
| Occasional — modals, drawers, toasts | Standard animation |
| Rare, first-time — onboarding, success | The delight budget lives here |

Keyboard-initiated actions are disqualified, not a judgment call. If the request fails this gate, say so and offer the static alternative — that is the correct answer, not a dodge.

Name the purpose in one word before continuing: **feedback**, **spatial consistency**, **state indication**, **bridging a jarring change**, **explanation** (marketing only), **delight** (rare tier only). Can't name it — don't build it. Data the user is reading or acting on never moves for style.

## 2. Cheapest tool that works

| Need | Tool |
| --- | --- |
| Hover, press, colour, class-driven state | CSS transition |
| Mount entrance, no JS state | CSS `@starting-style` |
| Predetermined motion that must stay smooth under load | CSS animation |
| Programmatic control, no library | WAAPI |
| Springs, layout, exits, gesture-driven values | Motion (`motion.dev`) |

CSS beats JS under load: `transform`/`opacity` run on the compositor while rAF animation drops frames. Never install a library for a fade. If the task needs a *component* — toast, drawer, dropdown, command menu — route to `references/libraries.md` first; hand-rolling those loses focus management.

## 3. Properties

- `transform` and `opacity` only. `clip-path` is the sanctioned third. `height` only for accordions.
- Never `scale(0)` — start at `0.9–0.97` with `opacity: 0`.
- `transform-origin` at the trigger for popovers, menus, tooltips. Modals are exempt, they stay centred.
- Percentages in `translate()` are relative to the element's own size; prefer them to pixels.
- Never drive a child's transform from a CSS variable on the parent — it restyles every descendant.

## 4. Easing, duration, springs

| Situation | Easing |
| --- | --- |
| Entering or exiting | `ease-out` |
| Moving on screen | `ease-in-out` |
| Hover, colour | `ease` |
| Constant motion | `linear` |

Never `ease-in` on UI — it delays the moment the user is watching. Built-ins are too weak:

```css
--ease-out: cubic-bezier(0.23, 1, 0.32, 1);
--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);
--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);
```

| Element | Duration |
| --- | --- |
| Button press | 100–160ms |
| Tooltip, small popover | 125–200ms |
| Dropdown, select | 150–250ms |
| Modal, drawer | 200–500ms |

UI stays under 300ms; modals, drawers, sheets and toasts are the documented exceptions.

Two parameters do the reasoning, and neither is a duration: **damping ratio** — `1.0` critically damped, no overshoot; `~0.8` only when the gesture itself carried momentum — and **response**, how fast it settles (`0.3–0.4` for sheets, drawers and moves). On the web `bounce: 0` plus `duration` is the same idea.

Springs instead of curves when the motion is drag with momentum, gesture-interruptible, or should feel alive: `{ type: "spring", duration: 0.5, bounce: 0.2 }`, or `{ mass: 1, stiffness: 100, damping: 10 }`. Bounce 0.1–0.3, and only for drag-to-dismiss and playful interactions.

## 5. Choreography — two or more things moving

Agents choreograph by adding. Good choreography mostly cuts moving parts and runs the rest in parallel. Sequence only when order carries meaning; otherwise it reads as a queue.

- **One entrance per container.** If the panel slides in, its contents are already there. Never animate a parent's entrance *and* stagger its children.
- **No stagger on high-frequency or keyboard-triggered UI.** Dropdown rows, menu items, command palettes appear together or not at all.
- **Uniform stagger is forbidden.** Equal delays make a group move like a metronome.
- **Choreography never blocks interaction.** Visible means clickable.

Cast every element before writing timing: **lead** (moves first, furthest, longest), **support** (shorter, smaller), **decoration** (fade only), **static** (doesn't move). Two leads means a hierarchy problem motion can't fix.

For each pair, pick one relationship: **together** (same object or same moment — the default), **overlap** (stagger), **sequence** (only when simultaneous would look broken — two rich views, one element passing through another, cause then effect), or **carry** (animate the parent, let children come along).

- Stagger 30–80ms, total under ~300ms. Long lists: stagger the first few, release the rest together.
- Marketing and first-run can run 100–150ms steps on 0.5–0.6s entrances.
- Order follows importance, not DOM order. Vary treatment as well as delay.
- Exits reverse the entrance direction.
- An element leaving must stop occupying space immediately — `AnimatePresence mode="popLayout"`, or `position: absolute` before animating out.
- Sub-animations of one component share a timing family, or the component reads as two objects.

## 6. Gestures

Two phases, and mixing them is the main cause of gestures that feel wrong:

- **Pointer down — track.** Position maps 1:1. No easing, no duration, no spring; the element *is* the finger. Write `transform` directly from a motion value, never React state per move (a re-render per pointermove drops frames), never a CSS variable on the parent.
- **Pointer up — release.** Physics: a spring seeded with release velocity. Never a keyframe.
- **Project where it was going**, not where it stopped: `v/1000 · d/(1−d)` with `d ≈ 0.998`, snap to the point nearest that endpoint, then hand the release velocity to the spring.
- **Interrupt from the presentation value.** Read the live transform and start there; starting from the target jumps, and a hard reversal reads as a brick wall.

- Rubber-band past boundaries (`dragElastic` ≈ 0.2); a hard stop feels like a wall.
- Dismiss on distance **or** velocity — ~40–50% of travel, or a fast flick. Distance alone makes flicks feel ignored.
- Bounce only at the end of a drag; the same element closed by a button gets zero bounce.
- Snap-back is fast.
- Derive linked values from drag progress (`useTransform`), never a parallel animation — two sources drift the moment the gesture is interrupted.
- Never a keyframe on anything a gesture can touch: springs and transitions keep velocity when retargeted, keyframes restart from zero.
- Sheets and drawers: use Vaul. It encodes `cubic-bezier(0.32, 0.72, 0, 1)` over 500ms — override to ~200ms for a small floating drawer.
- `touch-action` on the drag axis, hit targets ≥ 44px, test on real hardware.

## 7. Ship with it

Reduced motion means fewer and gentler, not zero: keep opacity and comprehension, drop movement and position changes. Gesture tracking stays — the user is producing that motion. Translucency and contrast have their own signals: `prefers-reduced-transparency: reduce` goes solid, `prefers-contrast: more` gets a defined border. Gate hover motion behind `@media (hover: hover) and (pointer: fine)`.

| Never | Instead |
| --- | --- |
| `transition: all` | name the properties |
| `scale(0)` entrance | `scale(0.95)` + `opacity: 0` |
| `ease-in` on UI | `ease-out` or a strong curve |
| Animation on a 100+/day action | none |
| UI over 300ms with no reason | 150–250ms |
| Animating width, height, margin, top, left | transform, opacity |
| Keyframes on toasts and toggles | transitions |
| Ungated `:hover` motion | hover media query |
| Missing reduced-motion variant | gentler, not zero |
| Everything entering at once | 30–80ms stagger |
| `transform-origin: center` on a popover | the trigger's origin — modals stay centred |

Feel can't be settled from code: play it at 2–5× duration or in the DevTools animation inspector, test gestures on a real device, and look again the next day.

<!-- source: animate, choreograph, gestures, emil-design-eng, apple-design -->
