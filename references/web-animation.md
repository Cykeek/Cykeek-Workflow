# Web Animation

Read when the request names SVG, Lottie, glass, ASCII, page transitions, or micro-interactions. `motion.md` §1 still runs first: a decorative SVG, a Lottie loop on a first visit, or a frosted glass panel over a flat background is wrong before it is specialized. This file gates the four surfaces the existing motion references do not cover and routes the other five to the skill depth that owns them.

## 1. Gate — specialised surface or core motion

Four answers, one line each, before opening any skill.

1. **What is the job** — data marks, logo draw-on, glass card, Lottie integration, ASCII generator, or a page transition that the router can already do.
2. **Core motion first** — would `motion.md` §2 do it? A micro-interaction, a tooltip, a loading indicator, a list reorder, a toggle are all `motion.md` + `micro-interaction`. If yes, the special surface is a layer, not the job.
3. **New surface?** — SVG stroke draw-on, Lottie control/theming, frosted glass, ASCII render, or Next App Router exit animations are not in `motion.md`. Those open §3–§7.
4. **With or without a skill** — the iart skill IDs in the route table are depth. This file is the contract when they are absent; the file is always read first.

## 2. Route

| Job | Read | Skill for depth | Never |
| --- | --- | --- | --- |
| GSAP scroll, hero timeline, SplitText, Flip, Lenis sync | `references/motion.md` §1, `references/gsap.md` §3–4 | `gsap-web` | `.delay()` chains, or ScrollTrigger without `gsap.ticker` |
| 60fps: stop layout thrash, convert width/left to transform | `references/motion-audit.md` | `60fps-animation` | animating layout props per frame |
| SVG stroke draw-on, path morph, motion-along-path | §3 | `svg-animation` | animating raw `d` with mismatched point counts |
| Lottie dotLottie, scroll-drive, runtime theming | §4 | `lottie-animation` | re-exporting per brand instead of runtime recolor |
| Frosted glass / Apple liquid glass | §5 | `glassmorphism` | backdrop-filter over a flat background |
| ASCII generative field, video-to-ASCII, Three.js AsciiEffect | §6 | `ascii-animation` | square-pixel sampling without cell-aspect correction |
| Next App Router page exit/entrance, Framer AnimatePresence | §7 | `page-transition-animation` | layout.tsx for per-route animations |
| Micro-interaction, hover/press feedback, toggle | `references/motion.md` §2, §6 | `micro-interaction` | animating width/height instead of scale |
| Reduced-motion tiering (vestibular / soften / keep) | `references/motion.md` §7 | `accessible-animation` | all-or-nothing kill switch |

## 3. SVG

- **Draw-on:** `pathLength="1"` on the `<path>`, then `stroke-dasharray: 1; stroke-dashoffset: 1;` animate to `0`. No JS measurement needed. Reverse by negating the sign. Get length in JS with `path.getTotalLength()` as fallback.
- **Morph:** GSAP MorphSVG handles mismatched point counts; Flubber for non-GSAP stacks. Hand-authored morphs need identical command structure in both `d` attributes.
- **Motion path:** GSAP MotionPath (`autoRotate`, `alignOrigin`) for scrub and rotation; SMIL (`<animateMotion>` + `<mpath>`) for self-contained icons; CSS `offset-path` for declarative.
- **Filters:** animate `feDisplacementMap scale` for gooey, `feGaussianBlur stdDeviation` for focus pulls. Paint-heavy — animate sparingly.

## 4. Lottie

- Prefer **dotLottie** (`@lottiefiles/dotlottie-web`) over raw `lottie-web` for new work — smaller, WASM, multiple themes.
- `autoplay: false` when scroll-driving; `setFrame(p * totalFrames)` on scroll, after `load` event.
- Theme via `dotLottie.setTheme("dark")` at runtime; manual color walk of `layers[].shapes[]` as the fallback.
- State-machine style: `setSegment(start, end)` then `complete` listener switches back to idle.

## 5. Glass

Believable glass needs four properties together: frost (`backdrop-filter: blur(16px) saturate(160%)`), translucency (alpha ~0.08–0.18 bg), edge of glass (brighter border + lit `::before` ring), and depth (layered `box-shadow` drop + inset highlight/shade).

Liquid glass: `feDisplacementMap` + animated `feTurbulence` via SVG `filter` layered over `backdrop-filter`. Slow churn (`dur: 12–20s`), subtle scale (8–14). One element only — each glass layer re-blurs everything behind it.

Limits: cap the blur count, animate `transform`/`background`/`box-shadow` on hover (cheap) rather than the radius (expensive). Always `prefers-reduced-transparency` → solid panel, `prefers-reduced-motion` → stop the sweep.

## 6. ASCII

Brightness ramp (dark→light), then map luminance per cell. Cell aspect correction: `rows = cols * imgAspect * 0.5` — character cells are taller than wide; skipping this produces vertically stretched output.

Render: `<pre>` + `textContent` up to ~120×60 at 30fps; canvas `fillText` for per-char color or 60fps. Three.js: `AsciiEffect.render(scene, camera)` through the effect, not the raw renderer. Terminal: ANSI `ESC + 'H'` home + throttle 12–24fps.

## 7. Page transitions (Next.js App Router)

The exit problem: App Router unmounts the old route before the new one finishes mounting, so `AnimatePresence` never sees a removal. Fix with `template.tsx` (remounts per route) + `mode="wait"` keyed on `pathname`, and **FrozenRouter** (`LayoutRouterContext` snapshot) so the outgoing tree keeps rendering its own content during exit.

```tsx
// app/template.tsx
<AnimatePresence mode="wait" initial={false}>
  <PageTransition key={pathname}>
    <FrozenRouter>{children}</FrozenRouter>
  </PageTransition>
</AnimatePresence>
```

`FrozenRouter` snapshots `useContext(LayoutRouterContext)` and re-provides it so the exiting subtree does not re-render the next route's data. `next-view-transitions` is the alternative for simple crossfade/morph: `document.startViewTransition(() => router.push(href))`.

## 8. Deliver and prove

The deliverable for a specialized surface is **one HTML file** that opens directly: the SVG inline, the dotLottie from CDN or a relative path, the glass panel over a real backdrop, the ASCII field with a fixed-seed `?t=N` harness, the transition as two view stubs toggled in one module.

- **State-driven** surfaces (micro-interaction, glass hover, page transition): freeze by `?state=<name>` setting the controlled prop/class.
- **Time-driven** surfaces (SVG draw-on, ASCII field, Lottie loop): freeze by `?t=N` injecting elapsed time or `setFrame(N)`.
- **Three instants:** start, mid, end (or the three states for a toggle). Screenshot both normal and `prefers-reduced-motion` paths.
- `prefers-reduced-transparency` verified for glass: opaque fallback screenshot.

## 9. Never

| Never | Instead |
| --- | --- |
| Flat-backdrop glass | a real busy background |
| Square-pixel ASCII sampling | `rows = cols * aspect * 0.5` |
| `layout.tsx` for per-route enter animation | `template.tsx` |
| Animate raw `d` with mismatched path counts | GSAP MorphSVG or matching commands |
| `autoplay` Lottie when scroll-driving | `autoplay: false` + `setFrame` |
| Large blur radius animation | animate transform instead |
| Global `animation: none` on reduced motion | tiered remove/soften/keep |
| `AnimatePresence` without a stable changing key | `key={pathname}` |
| Re-export Lottie per brand | runtime `setTheme` |

<!-- source: iart-ai/web-animation-skills (MIT) — gsap-web, 60fps-animation, page-transition-animation, accessible-animation, micro-interaction, glassmorphism, svg-animation, lottie-animation, ascii-animation; distilled not copied -->
