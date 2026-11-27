# Graphics — WebGL and specialised web motion

**Index** — WebGL · Shaders · SVG and Lottie · Glass · ASCII · Page transitions. Read the sections the routing row names, not the file.

Read when the request names WebGL, GLSL, a shader, Three.js, R3F, GLTF, instancing, particles, a flow field, a constellation background, SVG, Lottie, glass, ASCII, page transitions or micro-interactions. `motion.md` § Whether still runs first (merged from both sources, stated once): a 100+/day action or a decorative fade does not become right because it can be drawn on the GPU, and a decorative SVG, a Lottie loop on a first visit, or a frosted glass panel over a flat background is wrong before it is specialized. This file is the gate and the router; the skills the rows name own the depth, and this file is always read first and is the contract when they are absent. It gates five specialised surfaces and routes four core-motion jobs out.

**Gate — GPU or not** (runs for § WebGL, § Shaders). Four answers, one line each, before code. **1 Job** — what the scene does: background atmosphere, a product in 3D, a generative transition, an emitter, an explanation. "It would look cool" is not a job. **2 Cheaper path** — a gradient, a parallax layer, a few hundred canvas dots stay in CSS or 2D canvas (`motion.md` § Curves). GPU is for per-pixel full-screen work, real 3D, or thousands of elements. **3 Tier** — one HTML file with a CDN importmap for a standalone shader, scene or effect; the project's bundler and stack for an embedded one. Never a bundler for a single file. **4 Budget** — draw calls, pixel-ratio cap, texture count, particle count, and the disposal owner. A scene without an owner leaks VRAM across mounts.

**Gate — specialised surface or core motion** (runs for § SVG and Lottie, § Glass, § ASCII, § Page transitions). Four answers, one line each, before opening any skill. **1 Job** — data marks, logo draw-on, glass card, Lottie integration, ASCII generator, or a page transition that the router can already do. **2 Core motion first** — would `motion.md` § Curves do it? A micro-interaction, a tooltip, a loading indicator, a list reorder, a toggle are all `motion.md` + `micro-interaction`. If yes, the special surface is a layer, not the job. **3 New surface?** — SVG stroke draw-on, Lottie control/theming, frosted glass, ASCII render, or Next App Router exit animations are not in `motion.md`; those open § SVG and Lottie, § Glass, § ASCII, § Page transitions. **4 With or without a skill** — the iart skill IDs in the route tables are depth. This file is the contract when they are absent; the file is always read first.

**Core-motion routes** — when gate answer 2 sends the job out (the four route rows, kept whole): GSAP scroll, hero timeline, SplitText, Flip, Lenis sync → `references/motion.md` § Whether, `references/motion-stack.md` § GSAP, skill `gsap-web`; never `.delay()` chains, or ScrollTrigger without `gsap.ticker`. 60fps: stop layout thrash, convert width/left to transform → `references/motion.md` § Audit, skill `60fps-animation`; never animating layout props per frame. Micro-interaction, hover/press feedback, toggle → `references/motion.md` § Curves, § Gestures, skill `micro-interaction`; never animating width/height instead of scale. Reduced-motion tiering (vestibular / soften / keep) → `references/motion.md` § Curves, skill `accessible-animation`; never an all-or-nothing kill switch.

## WebGL

Read when the request names WebGL, Three.js, R3F, GLTF, instancing, particles, a flow field or a constellation background. The GPU gate above runs first. Depth (if installed): `threejs-animation`, `particle-system`, `shader-glsl`; without them this section is the contract.

| Job | Read | Skill for depth | Never |
| --- | --- | --- | --- |
| Fragment shader, animated gradient, noise/fbm, SDF, domain warp, dissolve/displacement/glitch, post-processing | § Shaders | `shader-glsl` | hand-rolled noise in JS for a full-screen effect |
| 3D scene, hero, product viewer, camera move, GLTF clip blend, instancing, scroll-linked 3D, R3F, bloom/DOF | this section, **Scene** | `threejs-animation` | a 3D scene for a flat 2D layout |
| Confetti, snow, smoke, sparks, flow field, curl noise, connected-dot network, GPU `Points` | this section, **Particles** | `particle-system` | O(n²) pair checks past ~300 particles |

One skill per turn. If two are needed, the second waits until the first has rendered.

**Scene**

- `setAnimationLoop`, not raw rAF — it pauses on tab blur. `clock.getDelta()` times motion; multiply rates by `dt` so frame rate does not change speed.
- Camera follow: `lerp(goal, 1 - exp(-lambda*dt))`, lambda 3..8. Scroll-linked: map scroll 0→1 to a target and damp; a `CatmullRomCurve3` for a curved path. Never write camera position directly from the wheel.
- GLTF: one `AnimationMixer`, one action per clip, `mixer.update(dt)`. Blend with `from.crossFadeTo(to, dur, false)`; one-shots set `LoopOnce` and `clampWhenFinished`.
- Thousands of objects: `InstancedMesh` + `setMatrixAt`, `DynamicDrawUsage`, `needsUpdate = true`. Reuse geometry and material; merge static geometry. Prefer baked light or a matcap over many shadow casters.
- Cap DPR: `renderer.setPixelRatio(Math.min(devicePixelRatio, 2))`. The largest single mobile win.
- **Disposal is part of the task.** GC does not free GPU buffers: dispose geometry, material and every texture on every map slot, render targets, then on final teardown `renderer.dispose()` + `forceContextLoss()`. `scene.remove()` frees nothing. R3F auto-disposes declarative JSX; imperative `new THREE.*()` leaks — `useMemo` plus a matching `useEffect` cleanup. Verify with `renderer.info.memory`: mount/unmount ten times, geometries and textures return to baseline.

**Particles**

- Per-particle integrate: accumulate forces, semi-implicit Euler (velocity first, then position), drag, age, respawn. Clamp `dt` to ≤1/30 so a stalled tab does not explode the sim.
- A force is `[fx, fy]`; compose gravity, drag, attract/repel (`strength / (d² + softening)`), springs. Flow field: noise value as angle; curl noise for divergence-free swirl, evolved with time.
- Burst vs continuous: confetti is rotated flat rectangles + gravity + air drag, not dots; snow is slow fall + sine sway + size-varied depth; smoke rises, grows and fades. Seed the RNG — bare `Math.random()` breaks frame reproducibility.
- Connected dots: a uniform spatial grid, compare only 8 neighbours; O(n) instead of O(n²). Fade line alpha by distance.
- Thousands+: `BufferGeometry` positions + `Points` + `ShaderMaterial`, animate in the vertex shader, `AdditiveBlending`, `depthWrite: false`, `discard` by `gl_PointCoord` for round soft points.

**Deliver and prove** — shared by all six sections; merged from webgl §6 and web-animation §8, stated once here.

- The deliverable is **one HTML file** that opens directly: inline script, one render loop, no build step. A standalone shader, scene or effect pins the library to a CDN importmap; a specialised surface ships the SVG inline, the dotLottie from CDN or a relative path, the glass panel over a real backdrop, the ASCII field with a fixed-seed `?t=N` harness, the transition as two view stubs toggled in one module.
- One clock: `u_time`, accumulated clamped `dt`, or `clock.getElapsedTime()`. No `Date.now()` scattered through the code.
- **Freeze harness:** `?t=N` sets the clock to N, renders one frame, stops the loop. Same N gives same pixels — seeded randomness, fixed timestep, absolute `mixer.setTime()`. Time-driven surfaces (SVG draw-on, ASCII field, Lottie loop) freeze by `?t=N` injecting elapsed time or `setFrame(N)`; state-driven surfaces (micro-interaction, glass hover, page transition) freeze by `?state=<name>` setting the controlled prop/class.
- **Three instants:** start, mid, end — transitions use `u_progress` 0 / 0.5 / 1, a toggle takes its three states. Screenshot each, both normal and `prefers-reduced-motion` paths. Check fidelity to the brief and artifacts: black canvas = compile/init error (read the console), NaN = vanished geometry, 404 = missing asset, banding and white blowout = precision.
- Headless Chromium supplies a WebGL context (swiftshader). A black canvas from a missing context is UNVERIFIED, not a code failure.
- Reduced motion (merged): freeze the clock or slow decorative rotation; keep the still frame and the copy. Tiering is vestibular / soften / keep at `references/motion.md` §7 — never an all-or-nothing kill switch, never global `animation: none`. Glass adds `prefers-reduced-transparency` → solid panel, verified with an opaque fallback screenshot.
- Embedded in an SPA: cancel the loop and dispose before unmount. The four-move loop in `process.md` § Inner loops applies to each round.

**Never** — GPU domain; each specialised section carries its own.

| Never | Instead |
| --- | --- |
| WebGL for a fade or a hover | CSS (`motion.md` §2) |
| GPU before `motion.md` §1 | the gate first |
| Two skills in one turn | finish the first, render, then the second |
| One HTML file with a bundler | CDN importmap |
| Raw rAF, wall-clock per object | `setAnimationLoop`, one clock |
| Uncapped pixel ratio | `min(dpr, 2)` |
| `scene.remove()` as cleanup | explicit `dispose()` of GPU resources |
| O(n²) connections | spatial grid |
| One screenshot as proof | three frozen instants |
| Missing reduced-motion branch | freeze or slow |

## Shaders

Read when the request names a fragment shader, an animated gradient, noise/fbm, SDF, domain warp, dissolve/displacement/glitch or post-processing. Depth (if installed): `shader-glsl`; without it this section is the contract.

- Fragment shader = one colour per pixel, in parallel. Normalize `gl_FragCoord.xy / u_resolution`, centre to -1..1, aspect-correct `p.x *= res.x/res.y`.
- Fastest good gradient: cosine palette `a + b*cos(2π*(c*t+d))`. Organic texture: `fbm(uv*scale + u_time)`. Fluid or marble: domain warp, noise fed into noise. Crisp shape: SDF with `smoothstep(fwidth(d), -fwidth(d), d)`.
- Transitions: threshold noise against `u_progress` for dissolve; push UVs by a displacement map before sampling for warp; row-hash shift plus per-channel offset for glitch.
- Loop bounds constant in GLSL ES; fbm ≤5–6 octaves. `mediump` on mobile unless coordinates are large. Avoid branches and dependent texture reads in the hot path.
- Three.js wiring: `ShaderMaterial` with uniforms, `PlaneGeometry(2,2)` and a plain camera for a full-screen pass; pass a `vUv` varying for shaders on real geometry. Keep the shader a pure function of uniforms — `u_time` and `u_progress` are the only clocks.

## SVG and Lottie

Read when the request names SVG stroke draw-on, path morph, motion-along-path, Lottie/dotLottie, scroll-driving a Lottie or runtime theming. Depth (if installed): `svg-animation`, `lottie-animation`; without them this section is the contract.

- **Draw-on:** `pathLength="1"` on the `<path>`, then `stroke-dasharray: 1; stroke-dashoffset: 1;` animate to `0`. No JS measurement needed. Reverse by negating the sign. Get length in JS with `path.getTotalLength()` as fallback.
- **Morph:** GSAP MorphSVG handles mismatched point counts; Flubber for non-GSAP stacks. Hand-authored morphs need identical command structure in both `d` attributes.
- **Motion path:** GSAP MotionPath (`autoRotate`, `alignOrigin`) for scrub and rotation; SMIL (`<animateMotion>` + `<mpath>`) for self-contained icons; CSS `offset-path` for declarative.
- **Filters:** animate `feDisplacementMap scale` for gooey, `feGaussianBlur stdDeviation` for focus pulls. Paint-heavy — animate sparingly.
- **Lottie:** prefer **dotLottie** (`@lottiefiles/dotlottie-web`) over raw `lottie-web` for new work — smaller, WASM, multiple themes. `autoplay: false` when scroll-driving; `setFrame(p * totalFrames)` on scroll, after `load` event. Theme via `dotLottie.setTheme("dark")` at runtime; a manual color walk of `layers[].shapes[]` is the fallback. State-machine style: `setSegment(start, end)`, then a `complete` listener switches back to idle.
- **Never:** animating raw `d` with mismatched path counts → GSAP MorphSVG or matching commands; `autoplay` Lottie when scroll-driving → `autoplay: false` + `setFrame`; re-exporting per brand → runtime `setTheme`/recolor. (Route-Never and never-table rows merged.)

## Glass

Read when the request names frosted glass or Apple liquid glass. Depth (if installed): `glassmorphism`; without it this section is the contract.

- Believable glass needs four properties together: frost (`backdrop-filter: blur(16px) saturate(160%)`), translucency (alpha ~0.08–0.18 bg), edge of glass (brighter border + lit `::before` ring), and depth (layered `box-shadow` drop + inset highlight/shade).
- Liquid glass: `feDisplacementMap` + animated `feTurbulence` via SVG `filter` layered over `backdrop-filter`. Slow churn (`dur: 12–20s`), subtle scale (8–14). One element only — each glass layer re-blurs everything behind it.
- Limits: cap the blur count, animate `transform`/`background`/`box-shadow` on hover (cheap) rather than the radius (expensive). Always `prefers-reduced-transparency` → solid panel, `prefers-reduced-motion` → stop the sweep.
- **Never:** backdrop-filter over a flat background → a real busy background; large blur radius animation → animate `transform` instead.

## ASCII

Read when the request names an ASCII generative field, video-to-ASCII or Three.js AsciiEffect. Depth (if installed): `ascii-animation`; without it this section is the contract.

- Brightness ramp (dark→light), then map luminance per cell. Cell aspect correction: `rows = cols * imgAspect * 0.5` — character cells are taller than wide; skipping this produces vertically stretched output.
- Render: `<pre>` + `textContent` up to ~120×60 at 30fps; canvas `fillText` for per-char color or 60fps. Three.js: `AsciiEffect.render(scene, camera)` through the effect, not the raw renderer. Terminal: ANSI `ESC + 'H'` home + throttle 12–24fps.
- **Never:** square-pixel sampling without cell-aspect correction → `rows = cols * aspect * 0.5`.

## Page transitions

Read when the request names Next.js App Router page exit/entrance or Framer AnimatePresence. Depth (if installed): `page-transition-animation`; without it this section is the contract.

- The exit problem: App Router unmounts the old route before the new one finishes mounting, so `AnimatePresence` never sees a removal. Fix with `template.tsx` (remounts per route) + `mode="wait"` keyed on `pathname`, and **FrozenRouter** (`LayoutRouterContext` snapshot) so the outgoing tree keeps rendering its own content during exit.

```tsx
// app/template.tsx
<AnimatePresence mode="wait" initial={false}>
  <PageTransition key={pathname}>
    <FrozenRouter>{children}</FrozenRouter>
  </PageTransition>
</AnimatePresence>
```

- `FrozenRouter` snapshots `useContext(LayoutRouterContext)` and re-provides it so the exiting subtree does not re-render the next route's data. `next-view-transitions` is the alternative for simple crossfade/morph: `document.startViewTransition(() => router.push(href))`.
- **Never:** `layout.tsx` for per-route enter animation → `template.tsx`; `AnimatePresence` without a stable changing key → `key={pathname}`.

<!-- source: iart-ai/webgl-animation-skills (MIT) — shader-glsl, threejs-animation, particle-system — and iart-ai/web-animation-skills (MIT) — gsap-web, 60fps-animation, page-transition-animation, accessible-animation, micro-interaction, glassmorphism, svg-animation, lottie-animation, ascii-animation; both distilled not copied; merged from references/webgl.md + references/web-animation.md, repeated rules stated once -->
