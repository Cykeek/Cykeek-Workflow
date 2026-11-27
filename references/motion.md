# Motion

**Index** — Whether · Curves · Choreography · Gestures · Audit · Clone. Read the sections the routing row names, not the file.

## Whether

Read when a task adds, changes or reviews animation. Work in this order — the gate comes before the curve, every time. A page whose timeline is the wheel is `motion-stack.md` § The scroll score, not this file; a reference the user wants copied — a video, a gif, a still, a link — is `§ Clone`, which measures it instead of choosing it. This section still answers first: a clone that should not animate is a clone of a mistake. Old numbering maps here: `motion.md` §1 Should it animate at all and §2 Cheapest tool that works → this section; §3 Properties, §4 Easing, duration, springs and §7 Ship with it → `## Curves`; §5 → `## Choreography`; §6 → `## Gestures`; the audit and clone protocols → `## Audit` and `## Clone`.

| Frequency | Decision |
| --- | --- |
| 100+×/day — keyboard shortcuts, command palettes | **Never.** Stop. |
| Tens/day — hover, list navigation | Near-imperceptible or nothing |
| Occasional — modals, drawers, toasts | Standard animation |
| Rare, first-time — onboarding, success | The delight budget lives here |

Keyboard-initiated actions are disqualified, not a judgment call. If the request fails this gate, say so and offer the static alternative — that is the correct answer, not a dodge. Name the purpose in one word before continuing: **feedback**, **spatial consistency**, **state indication**, **bridging a jarring change**, **explanation** (marketing only), **delight** (rare tier only). Can't name it — don't build it. Data the user is reading or acting on never moves for style.

**Cheapest tool that works.**

| Need | Tool |
| --- | --- |
| Hover, press, colour, class-driven state | CSS transition |
| Mount entrance, no JS state | CSS `@starting-style` |
| Predetermined motion that must stay smooth under load | CSS animation |
| Programmatic control, no library | WAAPI |
| Springs, layout, exits, gesture-driven values | Motion (`motion.dev`) |
| GSAP as the project's library — timelines, ScrollTrigger, React cleanup | `motion-stack.md` § GSAP |
| Full-screen per-pixel effect, real 3D, thousands of elements | `graphics.md` § WebGL |

CSS beats JS under load: `transform`/`opacity` run on the compositor while rAF animation drops frames. Never install a library for a fade. If the task needs a *component* — toast, drawer, dropdown, command menu — route to `platform.md` § Libraries first; hand-rolling those loses focus management.

## Curves

- `transform` and `opacity` only. `clip-path` is the sanctioned third. `height` only for accordions (measured).
- Never `scale(0)` — start at `0.9–0.97` with `opacity: 0`.
- `transform-origin` at the trigger for popovers, menus, tooltips. Modals are exempt, they stay centred.
- Percentages in `translate()` are relative to the element's own size; prefer them to pixels.
- Never drive a child's transform from a CSS variable on the parent — it restyles every descendant.

| Situation | Easing |
| --- | --- |
| Entering or exiting | `ease-out` |
| Moving on screen | `ease-in-out` |
| Hover, colour | `ease` |
| Constant motion | `linear` |

Entrances and exits use `ease-out` or a strong curve; `ease-in` on UI is a block — it delays the moment the user is watching. Built-ins are too weak:

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

Two parameters do the reasoning, and neither is a duration: **damping ratio** — `1.0` critically damped, no overshoot; `~0.8` only when the gesture itself carried momentum — and **response**, how fast it settles (`0.3–0.4` for sheets, drawers and moves). On the web `bounce: 0` plus `duration` is the same idea. Springs instead of curves when the motion is drag with momentum, gesture-interruptible, or should feel alive: `{ type: "spring", duration: 0.5, bounce: 0.2 }`, or `{ mass: 1, stiffness: 100, damping: 10 }`. Bounce 0.1–0.3, and only for drag-to-dismiss and playful interactions.

Reduced motion is **tiered**, not all-or-nothing — gentler rather than absent. Sort every animation into one of three tiers; removing entirely is rarely the right call, because a sudden teleport is as disorienting as unwanted movement.

| Tier | Motion | Reduced |
| --- | --- | --- |
| **1 — Remove** | Parallax, large slides/zooms, 3D spin, scroll-jacked pins, smooth-scroll hijacking | `transform: none`, do not instantiate Lenis |
| **2 — Soften** | Small pop-ins, slide-in panels, scale entrances | Cross-fade ≤ 200ms, drop the displacement |
| **3 — Keep** | Opacity fades, colour transitions, focus rings, loading feedback | Shorten to ≤ 150ms, never `0s` (breaks `transitionend`) |

Gesture tracking (pointerdown → track → pointerup) stays in all tiers — the user is producing that motion. CSS gating covers the cascade; JS gating (`matchMedia` / `gsap.matchMedia()`) covers GSAP, Lenis and canvas. Hover motion is gated behind `(hover: hover) and (pointer: fine)`. Toggling the OS preference fires a `change` event — listen for it, or the page holds a stale state.

| Never | Instead |
| --- | --- |
| `animation: none` / `transition: 0s` globally | `0.01ms` — the event still fires |
| All-or-nothing reduced-motion | tiered remove / soften / keep |
| `transition: all` | name the properties |
| `scale(0)` entrance | `scale(0.95)` + `opacity: 0` |
| `ease-in` on UI | `ease-out` or a strong curve |
| Animation on a 100+/day action | none |
| UI over 300ms with no reason | 150–250ms |
| Animating width, height, margin, top, left | transform, opacity |
| Keyframes on toasts and toggles | transitions |
| Ungated `:hover` motion | `(hover: hover) and (pointer: fine)` |

Feel can't be settled from code: play it at 2–5× duration or in the DevTools animation inspector, test gestures on a real device, and look again the next day.

## Choreography

Two or more things moving. Agents choreograph by adding. Good choreography mostly cuts moving parts and runs the rest in parallel. Sequence only when order carries meaning; otherwise it reads as a queue.

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

## Gestures

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
- Never a keyframe on anything rapid or that a gesture can touch — toasts and toggles included: springs and transitions keep velocity when retargeted, keyframes restart from zero.
- Sheets and drawers: use Vaul. It encodes `cubic-bezier(0.32, 0.72, 0, 1)` over 500ms — override to ~200ms for a small floating drawer.
- `touch-action` on the drag axis, hit targets ≥ 44px, test on real hardware.

## Audit

Read when reviewing motion that exists rather than deciding whether to add it. `## Curves` governs the values; this section is how to judge and fix what is already there.

**The ten standards.** A violation is a finding. Default to flagging; approval is earned. Where a standard restates a hub rule, the rule is stated once in its strictest form at the pointer — the merge is recorded by these rows: 2–8 point at the hub sections, 1 shares `## Whether`'s purpose words.

1. **Justified.** Every animation answers why — the `## Whether` purpose words. "It looks cool" on a frequently seen element is a block.
2. **Frequency-appropriate.** — `## Whether`'s frequency table.
3. **Responsive easing.** — `## Curves`' easing table and the ease-in block.
4. **Sub-300ms.** — `## Curves`' duration table and its exceptions line.
5. **Origin correct.** — `## Curves`' properties list.
6. **Interruptible.** — `## Gestures`' keyframe rule.
7. **Composite only.** — `## Curves`' properties list.
8. **Accessible.** — `## Curves`' reduced-motion tiers and hover gating.
9. **Asymmetric.** The deliberate phase (press, hold) is slower than the system response (release).
10. **Cohesive.** Motion matches the component and the product; a jarring crossfade where a blur would bridge is a finding.

**When it drops frames.** Match the symptom, then diagnose in this order — it is sorted by how often each cause is the real one.

| Symptom | Cause | Fix |
| --- | --- | --- |
| Stutters | layout properties animating | `transform`/`opacity`; `scale` instead of width, `translate` instead of top/left |
| Choppy shadow | shadow repaints every frame | `filter: drop-shadow()` |
| Jank only during navigation or loading | main thread busy | move to CSS or WAAPI |
| Laggier as content grows | animated inherited CSS variable | set `transform` on the element |
| Hand-rolled React animation drops frames | `setState` per frame | write `ref.current.style` |
| Blur transition lags | `blur()` over 20px | keep it 2–5px |
| Shifts 1px at start or end | GPU hand-off | `will-change: transform`, last resort only |

**Diagnosis order:** what animates → is the page busy → is React re-rendering per frame → does it degrade with content → is a CSS variable involved → is blur above 20px → only then `will-change`. The frame budget is 16.7ms at 60Hz and **8ms at 120Hz** — design against 8. "Smooth on my machine" is not evidence; a mid-range phone is the bar, and CPU throttling on a desktop models the wrong thing.

**Compositor-only conversion.** If a finding is layout-thrash (animating `width`/`height`/`top`/`left`/`box-shadow`), replace per the table. A `scaleX` distorts inner text — use **FLIP** (First, Last, Invert, Play) when content must stay crisp:

| Animating (expensive) | Triggers | Replace with |
| --- | --- | --- |
| `width` / `height` | Layout + Paint | `scaleX/Y` (+ FLIP for true size) |
| `top` / `left` / `margin` | Layout + Paint | `translate()` |
| `box-shadow` | Paint | `opacity` of a `::after` pseudo-element |
| `background-position` | Paint | `translate()` on a child |
| `height: auto` | not interpolatable | CSS grid `0fr→1fr` (works today), or `interpolate-size: allow-keywords` |
| Many reads in a loop | Layout thrash | batch all `getBoundingClientRect` reads before the first write |

`will-change: transform` promotes to its own compositor layer just-in-time (on hover or state change) and removes it when idle — never permanently on many elements.

**The fix hierarchy.** Prefer earlier moves; a later one without a stated reason is the wrong fix.

1. Delete the animation — wrong frequency, no purpose, keyboard-triggered.
2. Reduce it — shorter, smaller, fewer properties.
3. Fix the easing.
4. Fix the origin and physicality.
5. Make it interruptible — keyframes to transitions or springs.
6. Move it to the compositor.
7. Asymmetric timing.
8. Polish — blur to bridge a crossfade, stagger a group, spring an "alive" element.
9. Accessibility and cohesion.

**Report format.** A findings table, one row per issue — `Before | After | Why`. Then order by cost: layout before paint, busy-main-thread before idle. Name the file and line. Stop; apply only when asked.

**Surveying a codebase.** List every animation with its driver, properties and when it plays; expand `transition: all` and Motion shorthands into what they actually animate. Report the gaps too: places that should move and don't, where the answer is usually none — motion is proposed only where it clarifies a state change, never to make something "feel more alive". Motion that should go: keyboard actions, high-frequency surfaces, functional data, decoration with no purpose. Removing it also means removing the dead styles, the unused tokens and the now-pointless `will-change`.

**Naming what you see.** "The bouncy thing when a popover opens" is a *pop-in*; the iOS rubber-band scroll is *rubber-banding*; a list arriving in sequence is a *stagger*; content sliding under a fixed bar is *parallax*. Name the effect before prescribing a fix — a request for "make it smoother" is usually a naming problem.

## Clone

When the user hands over a video, a gif, a screen recording, a screenshot or a live URL and says copy it, match it, clone it or "1:1", the work is measurement. One pass ends in a lookalike that fails on the second frame; this protocol ends in a row-by-row proof.

**The fidelity gate.** Three answers, one line each, before the first line of code.

1. **Level.** *Motion-faithful*, the default: same trigger, order, stagger, timing, easing family, direction, and travel relative to the element. *Pixel-faithful* — "exactly", "1:1", "pixel-perfect": add fonts, sizes, colours, spacing, radii and assets. *Mixed* — "clone X and add Y": the additions are named and kept outside the compared region.
2. **Target.** URL and selector, file and time range, or "the toggle in the gif". Crop out browser chrome, the cursor and the recorder overlay; clone the scope given, not the page around it.
3. **Instrument.** What this session can actually do: capture a page, step a recording frame by frame, diff images. Discovered, never assumed. A still image carries no timing — when that is all there is, ask for a recording instead of inventing 300 ms.

**Capture parity.** Reference and clone are captured the same way or the comparison measures the setup: viewport, crop, fps, trigger, scroll steps, wait before, seed. Those numbers go in the spec header. When they differ between the two captures the round is void, and the capture is fixed first — never the code.

**Sources, and what each one gives.**

- **A live page** — the best source. Declared animations, keyframes and computed styles, when the session can read them, beat any estimate made from pixels; use them, then read frames only for what they cannot express.
- **A recording** — video, screen capture, gif: frames and nothing else. A still image is not a recording; it carries no start, duration or easing.
- **Canvas, WebGL, 3D** — frames are the fallback. When a trace of shaders, uniforms and scene values is available, that is the spec; rebuilding shaders from screenshots wastes rounds.
- **Lottie** — the file is the spec: layers, keyframes, bezier handles, its own fps and in and out points. Read it, do not re-derive it from pixels.

**Reading the frames.**

- Probe duration, size and fps. Two frames a second first, to find the window and the crop; then 25–30 fps, or the gif's own rate. Gif timing is quantised to frame delays and often slowed by the exporter, so allow a frame; a screen recording is scaled, so convert with `distance_css = distance_video × viewport / crop_width`.
- First frame it changes is the start, last frame it changes is the end, `duration = (last − first + 1) / fps`.
- Easing from the midpoint: more than half the travel at a third of the duration is an ease-out, less than a quarter an ease-in, symmetric an ease-in-out, constant steps linear, past the end and back a spring or a back curve.
- Stagger is the gap between sibling start frames — constant, growing, or per word and character.
- Read every element that moves: shadow growth, a blur clearing, letter-spacing tightening, a border appearing, an icon rotating. And read what does not move — adding motion the reference lacks fails exactly as hard as missing it.

**The trigger, first.** `t = 0` is the trigger, and it is part of the spec: load; enters the viewport; hover in and hover out, which are different timings and both get measured; click; drag, with the release velocity; a scroll position, where the motion maps to `scrollY` and stops when scrolling stops; a loop, where the period matters and the easing does not.

**The spec, before the code.** One row per element per property, in `.cykeek/verify/motion-<name>.md` — 40 lines: `# | element | property | from → to | start ms | duration ms | easing | stagger | source`. A `?` in a row is unread, not estimated. Trust numbers in this order: declared animation data the page or file exposes → tracked element positions over time → the frames themselves. Add the static facts the motion rests on: box at rest, transform-origin, z-order, overflow clipping, and for pixel work the font family, weight and size.

**Build in the project's own stack** — the `## Whether` tool order: CSS first, then the Web Animations API, then the motion library already present (`motion`/Framer in React, GSAP, Vue or Svelte transitions). A new dependency is a change the user did not ask for. Copy what makes frame zero match: initial state, `forwards` fill, origin, the clipping wrapper, and the order of transforms (individual `translate`/`scale` before a composed `transform`). Wrap non-essential motion in the reduced-motion path whether or not the reference does, and say so.

**Prove it.** Capture the clone with the reference's own command, then judge each row:

| Row | Pass |
| --- | --- |
| start, end, duration | within 80 ms — two frames at 25 fps |
| easing | same family, fitted deviation at most 5 % of travel |
| stagger | order right, gaps within 40 ms |
| distance | within 10 % of the element's size, same direction |
| stillness | nothing moves in the clone that rests in the reference |
| pixels, pixel-faithful only | mean at most 2 %, worst 5 %, at rest 0.5 % |
| activity curve | same shape — front-loaded stays front-loaded, even stays even |

Build side-by-side sheets with timestamps and look at them every round: the numbers say something is off, the sheets say what. Keep each round's folder; the last one is the receipt.

**Failure, and the cause it usually has.**

| Symptom | Cause |
| --- | --- |
| everything shifted by one offset | trigger latency or a missing delay |
| starts on time, ends early | duration too short — fix delay and duration together |
| front-loaded where the reference is even | easing too hard, or duration read off the visible window |
| settles fast, reference overshoots | a bezier standing in for a spring |
| only the last frames differ | fill mode missing, or the end value rounds differently |
| high mismatch at rest | crop, layout, or a fallback font |
| stagger gaps drift | delay is `base + index × step` and the index source changed |
| clone starts early on scroll | entry threshold or root margin differs from the first moving sample |

**When to stop.** Every row passes: say so with the numbers, the sheets and the files. Five rounds with no net gain on a row: stop, report the row, its last numbers and the reason — a font licence, a shader the user may not copy, a third-party embed, physics approximated. A threshold is never lowered silently; if a looser bar is worth it, name it and say why.

**Never.**

- Call one pass a clone.
- Compare two captures taken differently.
- Read only the first and last frame, where the easing is not.
- Invent timing from a still image.
- Wire a spring to a bezier with a different overshoot.
- Judge pixels before the fonts load, on pixel work.
- Report a match without the row numbers.

<!-- source: merged from motion.md, motion-audit.md and motion-clone.md — their sources: animate, choreograph, gestures, emil-design-eng, apple-design, greensock/gsap-skills (MIT), iart-ai/webgl-animation-skills (MIT), iart-ai/web-animation-skills (MIT); animation-performance, review-animations, improve-animations, find-animation-opportunities, remove-animations, animation-vocabulary, iart-ai/web-animation-skills (MIT) — 60fps conversion added; NiazMorshed2007/reanimate (MIT) — SKILL.md, references/capture.md, analysis.md, verify.md, implementation.md -->
