# GSAP

Read when GSAP is the library — the request names it, or the project already imports `gsap`. This file is mechanics, not taste: `motion.md` still decides whether to animate, the curve and the choreography; `scroll.md` still scores a scroll page before a single trigger is written. A GSAP answer that skips either gate is a library answer to a design question.

## 1. Setup

- Register each plugin once, at module scope: `gsap.registerPlugin(ScrollTrigger)`. Import from `gsap` and `gsap/<Plugin>`; never from a private registry.
- Every plugin is free since the Webflow acquisition. No Club GSAP, token, licence key or `@gsap/*` package — treat that advice as stale.
- GSAP is client-only. No tween, trigger or `window` reference during SSR; create inside the mounted effect.
- `gsap.config({ nullTargetWarn: false })` only when an absent target is intentional — never to hide a bad selector.

## 2. Tweens

| Call | Use |
| --- | --- |
| `gsap.to(target, vars)` | from current state to `vars` — the default |
| `gsap.from(target, vars)` | from `vars` to current; `immediateRender: true` by default, set it false for a later-played timeline |
| `gsap.fromTo(target, from, to)` | both ends explicit when the resting state matters |
| `gsap.set(target, vars)` | instant state, no animation — use inside a timeline for a hold |

- Durations are seconds. Declare time once; do not convert to ms at the call site.
- Animate transform aliases — `x`, `y`, `xPercent`, `yPercent`, `scale`, `rotation`, `skewX` — not raw `transform` strings. They compose, tween independently and stay on the compositor.
- `autoAlpha` over `opacity` when the element must also stop receiving clicks; it writes `visibility: hidden` at zero.
- One ease per motion family: `power2.out` entering, `power2.inOut` moving, `none` for scrubs and gesture tracking. `back` and `elastic` are the delight budget, not a default.
- `stagger` is a tween property: `stagger: { each: 0.04, from: "start", amount: 0.3 }`. `each` and `amount` are mutually exclusive. Non-uniform stagger comes from `stagger: { grid: "auto", from: "center" }` or a function — never equal delays across a list.
- `overwrite: "auto"` on tweens that can retarget the same property; the last intent wins instead of two tweens fighting.
- `invalidateOnRefresh: true` when a from-value is recalculated on resize.

## 3. Timelines

- A timeline over chained `.to(..., { delay })`. Delays do not compose when a tween is retimed; a timeline's positions do.
- Position parameter: `0`, `"-=0.1"`, `">"` (end of previous), `"<"` (start of previous), label. Overlap with `"-="`, never with a hand-calculated delay.
- `tl.defaults({ duration: 0.3, ease: "power2.out" })` sets the family once; children override only where they differ.
- Labels for beats — `tl.addLabel("peak")` — so a trigger can seek a named moment instead of a magic time.
- `repeat`, `repeatDelay`, `yoyo` for loops; loops are rare and never on high-frequency UI.
- `tl.timeScale()` to retune, `tl.progress()` to drive from a variable, `tl.pause()` to hold. `kill()` the timeline on unmount; a paused timeline still owns its targets.
- Sequence only where order carries meaning (`motion.md` §5); otherwise positions overlap.

## 4. ScrollTrigger

- Attach ScrollTrigger to the **timeline or top-level tween**, never to its children. Child triggers with the same start drift apart.
- `scrub` and `toggleActions` are mutually exclusive. `scrub: true` maps progress to the wheel — use it for a playhead, not a discrete reveal; `toggleActions: "play none none reverse"` for a state change.
- `start` and `end` are pairs such as `"top bottom"` and `"bottom top"`; pinned spans use `end: "+=120%"` so the act's length is explicit.
- `pin: true` needs a reserved final height in CSS or the page jumps when the pin ends (`scroll.md` § The timeline).
- `ease: "none"` on every tween inside a scrub or a `containerAnimation`; any other ease reads as the wheel fighting the animation.
- `ScrollTrigger.refresh()` after fonts, images, accordions or any layout change that moves a trigger — once on `document.fonts.ready`, then on the resize that actually changed height.
- `gsap.matchMedia()` owns breakpoints and reduced motion: one `mm.add()` per condition, `mm.revert()` on cleanup. A media query inside a tween is too late.
- `markers: true` in development only; strip before ship.
- A ScrollTrigger page still answers `scroll.md` § The score first. GSAP is the mechanism, not the journey.

## 5. React

- `useGSAP(() => { ... }, { scope: ref, dependencies: [...] })` from `@gsap/react`. Register plugins inside the callback or at module scope.
- `scope` is mandatory for selector text: `gsap.to(".card", ...)` must resolve inside the component, not the document. Unscoped selectors are the commonest React/GSAP bug.
- Without the hook: `const ctx = gsap.context(() => {...}, ref)` and `ctx.revert()` in the cleanup. `ctx.revert()` restores inline styles as well as killing tweens; `kill()` alone leaves them.
- `revert()` belongs in the same effect's cleanup, not a later one. Strict Mode double-invokes effects; a missing revert doubles every tween.
- No GSAP on the server. Effects, not render. A dependency array that re-runs on every render is a loop, not reactivity.
- `useGSAP` returns `contextSafe` for event handlers; wrap callbacks that create tweens so their animations join the scope and revert.

## 6. Other frameworks

- Vue: `onMounted` → `gsap.context(...)`, `onBeforeUnmount` → `ctx.revert()`. Svelte: `onMount` returning the revert.
- Same fence as React: mount-time creation, scoped selectors, one revert per context. A tween created outside its context is an orphan.

## 7. Performance and helpers

- `gsap.quickTo(target, "x", { duration: 0.3, ease: "power3" })` for pointer-driven values; `gsap.quickSetter` for direct writes. Both avoid a tween allocation per event.
- `gsap.utils.toArray(selector, scope)` before batch work; `mapRange`, `clamp`, `wrap`, `snap`, `interpolate`, `pipe`, `random`, `normalize`, `distribute` replace hand-rolled arithmetic.
- `gsap.ticker` for one rAF shared by many animations; never a second rAF loop beside it.
- `ScrollTrigger.batch()` for reveals across many elements — one trigger per row is a scroll handler per row.
- `force3D: "auto"` (default) keeps transforms on the compositor; do not set `will-change` on every element permanently.
- Animate `transform` and `opacity`. Layout properties (`width`, `height`, `top`, `left`) force restyle; `motion-audit.md` owns the frame-budget review.

## 8. Never

| Never | Instead |
| --- | --- |
| GSAP for a fade or hover | CSS (`motion.md` §2) |
| Club plugin, token, private registry | official `gsap` package — all plugins free |
| `gsap.to(".el")` in React without scope | `useGSAP` with `scope` |
| ScrollTrigger on a timeline's children | one trigger on the timeline |
| `scrub` with `toggleActions` | pick one |
| Ease inside a scrub | `ease: "none"` |
| `.delay()` chains | a timeline with position parameters |
| `kill()` without `revert()` on unmount | `ctx.revert()` |
| Tween during SSR | mount-time effect |
| A GSAP answer before `motion.md` §1 | the gate first |

<!-- source: greensock/gsap-skills (MIT) — eight SKILL.md files, distilled not copied -->
