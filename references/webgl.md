# WebGL

Read when the request names WebGL, GLSL, a shader, Three.js, R3F, GLTF, instancing, particles, a flow field or a constellation background. `motion.md` §1 still runs first: a 100+/day action or a decorative fade does not become right because it can be drawn on the GPU. This file is the gate and the router; the three skills it points at own the depth.

## 1. Gate — GPU or not

Four answers, one line each, before code.

1. **Job** — what the scene does: background atmosphere, a product in 3D, a generative transition, an emitter, an explanation. "It would look cool" is not a job.
2. **Cheaper path** — a gradient, a parallax layer, a few hundred canvas dots stay in CSS or 2D canvas (`motion.md` §2). GPU is for per-pixel full-screen work, real 3D, or thousands of elements.
3. **Tier** — one HTML file with a CDN importmap for a standalone shader, scene or effect; the project's bundler and stack for an embedded one. Never a bundler for a single file.
4. **Budget** — draw calls, pixel-ratio cap, texture count, particle count, and the disposal owner. A scene without an owner leaks VRAM across mounts.

## 2. Route

| Job | Read | Skill for depth | Never |
| --- | --- | --- | --- |
| Fragment shader, animated gradient, noise/fbm, SDF, domain warp, dissolve/displacement/glitch, post-processing | §3 | `shader-glsl` | hand-rolled noise in JS for a full-screen effect |
| 3D scene, hero, product viewer, camera move, GLTF clip blend, instancing, scroll-linked 3D, R3F, bloom/DOF | §4 | `threejs-animation` | a 3D scene for a flat 2D layout |
| Confetti, snow, smoke, sparks, flow field, curl noise, connected-dot network, GPU `Points` | §5 | `particle-system` | O(n²) pair checks past ~300 particles |

One skill per turn. If two are needed, the second waits until the first has rendered.

## 3. Shader

- Fragment shader = one colour per pixel, in parallel. Normalize `gl_FragCoord.xy / u_resolution`, centre to -1..1, aspect-correct `p.x *= res.x/res.y`.
- Fastest good gradient: cosine palette `a + b*cos(2π*(c*t+d))`. Organic texture: `fbm(uv*scale + u_time)`. Fluid or marble: domain warp, noise fed into noise. Crisp shape: SDF with `smoothstep(fwidth(d), -fwidth(d), d)`.
- Transitions: threshold noise against `u_progress` for dissolve; push UVs by a displacement map before sampling for warp; row-hash shift plus per-channel offset for glitch.
- Loop bounds constant in GLSL ES; fbm ≤5–6 octaves. `mediump` on mobile unless coordinates are large. Avoid branches and dependent texture reads in the hot path.
- Three.js wiring: `ShaderMaterial` with uniforms, `PlaneGeometry(2,2)` and a plain camera for a full-screen pass; pass a `vUv` varying for shaders on real geometry. Keep the shader a pure function of uniforms — `u_time` and `u_progress` are the only clocks.

## 4. Scene

- `setAnimationLoop`, not raw rAF — it pauses on tab blur. `clock.getDelta()` times motion; multiply rates by `dt` so frame rate does not change speed.
- Camera follow: `lerp(goal, 1 - exp(-lambda*dt))`, lambda 3..8. Scroll-linked: map scroll 0→1 to a target and damp; a `CatmullRomCurve3` for a curved path. Never write camera position directly from the wheel.
- GLTF: one `AnimationMixer`, one action per clip, `mixer.update(dt)`. Blend with `from.crossFadeTo(to, dur, false)`; one-shots set `LoopOnce` and `clampWhenFinished`.
- Thousands of objects: `InstancedMesh` + `setMatrixAt`, `DynamicDrawUsage`, `needsUpdate = true`. Reuse geometry and material; merge static geometry. Prefer baked light or a matcap over many shadow casters.
- Cap DPR: `renderer.setPixelRatio(Math.min(devicePixelRatio, 2))`. The largest single mobile win.
- **Disposal is part of the task.** GC does not free GPU buffers: dispose geometry, material and every texture on every map slot, render targets, then on final teardown `renderer.dispose()` + `forceContextLoss()`. `scene.remove()` frees nothing. R3F auto-disposes declarative JSX; imperative `new THREE.*()` leaks — `useMemo` plus a matching `useEffect` cleanup. Verify with `renderer.info.memory`: mount/unmount ten times, geometries and textures return to baseline.

## 5. Particles

- Per-particle integrate: accumulate forces, semi-implicit Euler (velocity first, then position), drag, age, respawn. Clamp `dt` to ≤1/30 so a stalled tab does not explode the sim.
- A force is `[fx, fy]`; compose gravity, drag, attract/repel (`strength / (d² + softening)`), springs. Flow field: noise value as angle; curl noise for divergence-free swirl, evolved with time.
- Burst vs continuous: confetti is rotated flat rectangles + gravity + air drag, not dots; snow is slow fall + sine sway + size-varied depth; smoke rises, grows and fades. Seed the RNG — bare `Math.random()` breaks frame reproducibility.
- Connected dots: a uniform spatial grid, compare only 8 neighbours; O(n) instead of O(n²). Fade line alpha by distance.
- Thousands+: `BufferGeometry` positions + `Points` + `ShaderMaterial`, animate in the vertex shader, `AdditiveBlending`, `depthWrite: false`, `discard` by `gl_PointCoord` for round soft points.

## 6. Deliver and prove

The deliverable for a standalone shader, scene or effect is **one HTML file** that opens directly: importmap pins the library to a CDN, inline script, one render loop, no build step.

- One clock: `u_time`, accumulated clamped `dt`, or `clock.getElapsedTime()`. No `Date.now()` scattered through the code.
- **Freeze harness:** `?t=N` sets the clock to N, renders one frame, stops the loop. Same N gives same pixels — seeded randomness, fixed timestep, absolute `mixer.setTime()`.
- **Three instants:** start, mid, end; transitions use `u_progress` 0 / 0.5 / 1. Screenshot each. Check fidelity to the brief and artifacts: black canvas = compile/init error (read the console), NaN = vanished geometry, 404 = missing asset, banding and white blowout = precision.
- Headless Chromium supplies a WebGL context (swiftshader). A black canvas from a missing context is UNVERIFIED, not a code failure.
- `prefers-reduced-motion`: freeze the clock or slow decorative rotation; keep the still frame and the copy.
- Embedded in an SPA: cancel the loop and dispose before unmount. The four-move loop in `process.md` § Inner loops applies to each round.

## 7. Never

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

<!-- source: iart-ai/webgl-animation-skills (MIT) — shader-glsl, threejs-animation, particle-system; distilled not copied -->
