# Motion audit

Read when reviewing motion that exists rather than deciding whether to add it. `motion.md` still governs the values; this file is how to judge and fix what is already there.

## The ten standards

A violation is a finding. Default to flagging; approval is earned.

1. **Justified.** Every animation answers why: feedback, spatial consistency, state indication, explanation, or bridging a jarring change. "It looks cool" on a frequently seen element is a block.
2. **Frequency-appropriate.** 100+/day and keyboard-triggered get none. Tens/day gets near-imperceptible.
3. **Responsive easing.** Entrances and exits use `ease-out` or a strong curve. `ease-in` on UI is a block.
4. **Sub-300ms.** Modals, drawers, sheets and toasts are the documented exceptions.
5. **Origin correct.** Popovers, menus and tooltips scale from their trigger, never centre. Never `scale(0)`. Modals stay centred.
6. **Interruptible.** Anything rapid or gesture-driven uses transitions or springs, never keyframes.
7. **Composite only.** `transform` and `opacity`; `clip-path` and measured accordion height are the exceptions.
8. **Accessible.** `prefers-reduced-motion` present and gentler rather than absent; hover motion gated behind `(hover: hover) and (pointer: fine)`.
9. **Asymmetric.** The deliberate phase (press, hold) is slower than the system response (release).
10. **Cohesive.** Motion matches the component and the product; a jarring crossfade where a blur would bridge is a finding.

## When it drops frames

Match the symptom, then diagnose in this order — it is sorted by how often each cause is the real one.

| Symptom | Cause | Fix |
| --- | --- | --- |
| Stutters | layout properties animating | `transform`/`opacity`; `scale` instead of width, `translate` instead of top/left |
| Choppy shadow | shadow repaints every frame | `filter: drop-shadow()` |
| Jank only during navigation or loading | main thread busy | move to CSS or WAAPI |
| Laggier as content grows | animated inherited CSS variable | set `transform` on the element |
| Hand-rolled React animation drops frames | `setState` per frame | write `ref.current.style` |
| Blur transition lags | `blur()` over 20px | keep it 2–5px |
| Shifts 1px at start or end | GPU hand-off | `will-change: transform`, last resort only |

**Diagnosis order:** what animates → is the page busy → is React re-rendering per frame → does it degrade with content → is a CSS variable involved → is blur above 20px → only then `will-change`.

The frame budget is 16.7ms at 60Hz and **8ms at 120Hz** — design against 8. "Smooth on my machine" is not evidence; a mid-range phone is the bar, and CPU throttling on a desktop models the wrong thing.

## Compositor-only conversion

If a finding is layout-thrash (animating `width`/`height`/`top`/`left`/`box-shadow`), replace per the table. A `scaleX` distorts inner text — use **FLIP** (First, Last, Invert, Play) when content must stay crisp:

| Animating (expensive) | Triggers | Replace with |
| --- | --- | --- |
| `width` / `height` | Layout + Paint | `scaleX/Y` (+ FLIP for true size) |
| `top` / `left` / `margin` | Layout + Paint | `translate()` |
| `box-shadow` | Paint | `opacity` of a `::after` pseudo-element |
| `background-position` | Paint | `translate()` on a child |
| `height: auto` | not interpolatable | CSS grid `0fr→1fr` (works today), or `interpolate-size: allow-keywords` |
| Many reads in a loop | Layout thrash | batch all `getBoundingClientRect` reads before the first write |

`will-change: transform` promotes to its own compositor layer just-in-time (on hover or state change) and removes it when idle — never permanently on many elements.

## The fix hierarchy

Prefer earlier moves; a later one without a stated reason is the wrong fix.

1. Delete the animation — wrong frequency, no purpose, keyboard-triggered.
2. Reduce it — shorter, smaller, fewer properties.
3. Fix the easing.
4. Fix the origin and physicality.
5. Make it interruptible — keyframes to transitions or springs.
6. Move it to the compositor.
7. Asymmetric timing.
8. Polish — blur to bridge a crossfade, stagger a group, spring an "alive" element.
9. Accessibility and cohesion.

## Report format

A findings table, one row per issue — `Before | After | Why`. Then order by cost: layout before paint, busy-main-thread before idle. Name the file and line. Stop; apply only when asked.

## Surveying a codebase

- List every animation with its driver, properties and when it plays. Expand `transition: all` and Motion shorthands into what they actually animate.
- Report the gaps too: places that should move and don't, where the answer is usually none — motion is proposed only where it clarifies a state change, never to make something "feel more alive".
- Motion that should go: keyboard actions, high-frequency surfaces, functional data, decoration with no purpose. Removing it also means removing the dead styles, the unused tokens and the now-pointless `will-change`.

## Naming what you see

"The bouncy thing when a popover opens" is a *pop-in*; the iOS rubber-band scroll is *rubber-banding*; a list arriving in sequence is a *stagger*; content sliding under a fixed bar is *parallax*. Name the effect before prescribing a fix — a request for "make it smoother" is usually a naming problem.

<!-- source: animation-performance, review-animations, improve-animations, find-animation-opportunities, remove-animations, animation-vocabulary, iart-ai/web-animation-skills (MIT) — 60fps conversion added -->
