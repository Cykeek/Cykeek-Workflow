# Motion stack — GSAP and scroll

**Index** — GSAP · ScrollTrigger · The scroll score. Read the sections the routing row names, not the file.

## GSAP

When the request names GSAP or the project already imports `gsap`. This is mechanics, not taste: `motion.md` still decides whether to animate, the curve and the choreography. GSAP is the mechanism, not the journey — a GSAP answer that skips § The score is a library answer to a design question.

### Setup

Register each plugin once, at module scope — `gsap.registerPlugin(ScrollTrigger)` — importing from `gsap` and `gsap/<Plugin>`, never a private registry. Every plugin is free since the Webflow acquisition: no Club GSAP, token, licence key or `@gsap/*` package — treat that advice as stale. GSAP is client-only: no tween, trigger or `window` reference during SSR, create inside the mounted effect. `gsap.config({ nullTargetWarn: false })` only when an absent target is intentional — never to hide a bad selector.

### Tweens

| Call | Use |
| --- | --- |
| `gsap.to(target, vars)` | from current state to `vars` — the default |
| `gsap.from(target, vars)` | from `vars` to current; `immediateRender: true` by default, set it false for a later-played timeline |
| `gsap.fromTo(target, from, to)` | both ends explicit when the resting state matters |
| `gsap.set(target, vars)` | instant state, no animation — use inside a timeline for a hold |

Durations are seconds; declare time once, do not convert to ms at the call site. Animate transform aliases — `x`, `y`, `xPercent`, `yPercent`, `scale`, `rotation`, `skewX` — not raw `transform` strings: they compose, tween independently and stay on the compositor. `autoAlpha` over `opacity` when the element must also stop receiving clicks; it writes `visibility: hidden` at zero. One ease per motion family: `power2.out` entering, `power2.inOut` moving, `none` for scrubs and gesture tracking — `back` and `elastic` are the delight budget, not a default. `stagger` is a tween property: `stagger: { each: 0.04, from: "start", amount: 0.3 }`, with `each` and `amount` mutually exclusive; non-uniform stagger comes from `stagger: { grid: "auto", from: "center" }` or a function — never equal delays across a list. `overwrite: "auto"` on tweens that can retarget the same property, so the last intent wins instead of two tweens fighting. `invalidateOnRefresh: true` when a from-value is recalculated on resize.

### SplitText and Flip

**SplitText (text reveal):** split after fonts load (`document.fonts.ready`), `autoSplit: true` (GSAP 3.13+) for re-split on resize, `revert()` before re-splitting or on unmount. Wrap line-mask reveals with `overflow: hidden` on the parent so `yPercent: 100` hides cleanly. **Flip (layout transitions):** `Flip.getState(els)` captures before the DOM mutates, `Flip.from(state, { absolute: true, duration: 0.6 })` animates the delta; `absolute: true` takes items out of flow during the move — without it, reflows between old and new positions produce jitter. For many FLIP elements batch all `getBoundingClientRect` reads before the mutation.

### Lenis smooth scroll sync

Drive Lenis from `gsap.ticker` and update ScrollTrigger on every Lenis scroll, or they run on separate loops and the pin drifts:

```js
lenis.on("scroll", ScrollTrigger.update);
gsap.ticker.add((t) => lenis.raf(t * 1000)); // seconds → ms
gsap.ticker.lagSmoothing(0);
```

`gsap.ticker` passes seconds; `lenis.raf()` wants milliseconds — multiply by 1000. Remove any standalone `requestAnimationFrame` loop driving Lenis.

### Timelines

A timeline over chained `.to(..., { delay })` — delays do not compose when a tween is retimed, a timeline's positions do. Position parameter: `0`, `"-=0.1"`, `">"` (end of previous), `"<"` (start of previous), label; overlap with `"-="`, never with a hand-calculated delay. `tl.defaults({ duration: 0.3, ease: "power2.out" })` sets the family once; children override only where they differ. Labels for beats — `tl.addLabel("peak")` — so a trigger can seek a named moment instead of a magic time. `repeat`, `repeatDelay`, `yoyo` for loops; loops are rare and never on high-frequency UI. `tl.timeScale()` to retune, `tl.progress()` to drive from a variable, `tl.pause()` to hold; `kill()` the timeline on unmount — a paused timeline still owns its targets. Sequence only where order carries meaning (`motion.md` §5); otherwise positions overlap.

### React

`useGSAP(() => { ... }, { scope: ref, dependencies: [...] })` from `@gsap/react`, registering plugins inside the callback or at module scope. `scope` is mandatory for selector text: `gsap.to(".card", ...)` must resolve inside the component, not the document — unscoped selectors are the commonest React/GSAP bug. Without the hook: `const ctx = gsap.context(() => {...}, ref)` and `ctx.revert()` in the cleanup — `revert()` belongs in the same effect's cleanup, not a later one; it restores inline styles as well as killing tweens, `kill()` alone leaves them; Strict Mode double-invokes effects, so a missing revert doubles every tween. No GSAP on the server: effects, not render; a dependency array that re-runs on every render is a loop, not reactivity. `useGSAP` returns `contextSafe` for event handlers — wrap callbacks that create tweens so their animations join the scope and revert.

### Other frameworks

Vue: `onMounted` → `gsap.context(...)`, `onBeforeUnmount` → `ctx.revert()`. Svelte: `onMount` returning the revert. Same fence as React: mount-time creation, scoped selectors, one revert per context — a tween created outside its context is an orphan.

### Performance and helpers

`gsap.quickTo(target, "x", { duration: 0.3, ease: "power3" })` for pointer-driven values; `gsap.quickSetter` for direct writes — both avoid a tween allocation per event. `gsap.utils.toArray(selector, scope)` before batch work; `mapRange`, `clamp`, `wrap`, `snap`, `interpolate`, `pipe`, `random`, `normalize`, `distribute` replace hand-rolled arithmetic. `gsap.ticker` for one rAF shared by many animations; never a second rAF loop beside it. `ScrollTrigger.batch()` for reveals across many elements — one trigger per row is a scroll handler per row. `force3D: "auto"` (default) keeps transforms on the compositor; do not set `will-change` on every element permanently. Animate only `transform`, `opacity` and `clip-path` — never `transition: all` and never `width`, `height`, `top`, `left`, which force restyle; `motion.md` § Audit owns the frame-budget review.

### Never

| Never | Instead |
| --- | --- |
| GSAP for a fade or hover | CSS (`motion.md` §2) |
| Club plugin, token, private registry | official `gsap` package — all plugins free |
| `gsap.to(".el")` in React without scope | `useGSAP` with `scope` |
| `.delay()` chains | a timeline with position parameters |
| `kill()` without `revert()` on unmount | `ctx.revert()` |
| Tween during SSR | mount-time effect |
| A GSAP answer before `motion.md` §1 | the gate first |

## ScrollTrigger

Attach ScrollTrigger to the **timeline or top-level tween**, never to its children — child triggers with the same start drift apart. `scrub` and `toggleActions` are mutually exclusive: `scrub: true` maps progress to the wheel, use it for a playhead, not a discrete reveal; `toggleActions: "play none none reverse"` for a state change. `start` and `end` are pairs such as `"top bottom"` and `"bottom top"`; a pinned span's floor is set in § The timeline, and its length is made explicit with `end: "+=120%"` or more. `pin: true` needs a reserved final height in CSS and a static stage — content height changing inside it is why the page jumps when the pin ends. `ease: "none"` on every tween inside a scrub or a `containerAnimation`; any other ease reads as the wheel fighting the animation. `ScrollTrigger.refresh()` after fonts, images, accordions or any layout change that moves a trigger — once on `document.fonts.ready`, then on the resize that actually changed height. `gsap.matchMedia()` owns breakpoints and reduced motion: one `mm.add()` per condition, `mm.revert()` on cleanup — a media query inside a tween is too late. `markers: true` in development only; strip before ship. With GSAP this section owns the trigger wiring and § The score owns the journey.

| Never | Instead |
| --- | --- |
| ScrollTrigger on a timeline's children | one trigger on the timeline |
| `scrub` with `toggleActions` | pick one |
| Ease inside a scrub | `ease: "none"` |

## The scroll score

When the request names scroll as the mechanism — scroll-driven, scrollytelling, pinned, cinematic hero, a site where scrolling plays a video — or when a scroll sequence already ships and is wrong. An ordinary entrance or reveal is `motion.md`; this is a page whose timeline is the wheel. Scroll is the one input every visitor already has: treat it as a timeline with real text on top, not a flythrough with copy pasted over it, and five sections that behave identically are one section shown five times. It is still a screen: `design.md` § The gate is answered first — job, one action, hierarchy, every state — and the score below adds time to it. A timeline with no hierarchy is a video with a scrollbar. The five answers below are written in the ledger and get one yes before the first section or trigger exists; the editor does not open before that.

### The score

Five answers, one line each, under the receipt, before the first section exists. A scroll page has no single state, so the plan is where the cost is saved.

1. **Journey** — four to seven beats, each a change in what the visitor knows or feels: recognition, tension, turn, substance, range, commitment. A section serving no beat is cut, however good the shot.
2. **The peak** — the one moment, as the sentence a visitor would tell a friend, and which beat it lives in. One peak: it gets the largest visual change, the most scroll room, and the silence before it. Three competing peaks are none.
3. **Devices per beat** — name the device each beat uses, from the kit below. At least four families, never the same one twice in a row, at most two scrubs.
4. **The frame at rest** — what each act looks like at 390, with reduced motion, and when its clip fails to load. Every act carries its beat as a still or the page is a video with no fallback.
5. **Proof** — which scroll positions are captured, at which widths, and the one thing that would make the sequence fail. One V case per act per width, its start, middle and end in one assertion, because the ledger is capped at 40 lines. One full-page screenshot proves nothing.

If the brief is one line, propose the journey in six sentences and get a yes before building. Silence is not a yes.

### The shape

Devices vary how a page looks. The shape is what its sections actually do, and it is a separate axis — two sites with the same order of sections are the same site, however different the skin. Pick one, and say why the others lost:

- **One continuous shot** — one camera move through the whole page. The most expensive and fragile shape; only when the brief is literally one journey.
- **Chapters** — hard cuts between worlds, each a full change of ground. The default when the subject has range.
- **Lateral journey** — the pan is the spine, not one section of it.
- **Longread** — figures pin while the argument scrolls; the copy is the timeline.
- **Product demo** — the interface is the scrubber and the copy annotates it. Never a fake dashboard.
- **Collection** — an index that indexes, then a detail that lingers.
- **Single scene** — one held image, copy on top, an ending that resolves.

One bespoke signature move per site, invented for this subject — not a retuned tilt or a recoloured spotlight, which are the moves every generated page already has.

### The devices

Pick per beat, never per page. Each is a different answer to what the visitor's hand actually does.

| Device | The hand | Use for | Cost |
| --- | --- | --- | --- |
| scrub | wheel scrubs a pre-rendered clip, one frame per notch | the open, a transformation, a place | heaviest: video, encoding, poster, mobile crop |
| pin | the stage holds while copy crosses | an argument, three states in one frame | the cheapest premium move; span 1.2 or more |
| pan | vertical scroll, lateral travel | a range, a lineup, a timeline | never a hierarchy — the first item reads as first |
| reveal | a `clip-path` wipe | something becoming something else | near free |
| kinetic | lines, words or characters assemble | one short headline, a punch line | lines by default; per-character is a cameo |
| parallax | layers at different rates | depth in a static composition | 200px of total travel or it reads as a bug |
| count | numbers land | real numbers only | no number, no counter |
| flow | ordinary sections, well set | rest, contrast, the close | free, and what makes the pinned acts land |
| pointer | things answer the cursor | presence, not scroll | never on a touch-only surface |

Two more are not acts: **drift** — the ground shifts colour as the page travels, which is what makes it one place instead of slides — and **composing**, where a pinned stage holds a scrubbing clip, a parallax layer and a kinetic headline at once. The limit is legibility, not generosity.

### The timeline

These are the contract; the mechanism follows the library the project already has, or one rAF if it has none.

- **Span** is the act's scroll length in viewport heights. Hero 2.2 to 3.0; a pinned act 1.2 or more (ScrollTrigger `end: "+=120%"` or more), because below that its travel is a pixel and every cue snaps; a lateral rail 2 to 3.
- **Clip time is not cue time.** A pinned stage is visible for a viewport before and after its travel, so a clip mapped to pinned progress sits frozen through both slides and the page scrolls a still photograph past the reader. Map the clip across the stage's whole visible life; keep cues on pinned progress.
- **The playhead is lerped.** Scroll writes a target, a rAF walks the video toward it at about 0.18 per frame, with an 8ms deadband on desktop and 20ms on mobile, and no new seek while the decoder is still resolving the last. Scroll never writes `currentTime`: wheel events arrive unevenly and a 1:1 write reproduces the gaps as a stutter.
- **Poster until a real frame paints.** A seeked, never-played muted video stays blank on iOS; hiding the poster at metadata flashes an empty stage.
- **Cues overlap** by roughly 15%, so the outgoing line is still fading as the next arrives and no gap reads as a loading failure. The last cue takes one value and holds, so the act ends on a statement.
- **Depth is separation, not blur.** Background, subject, type and foreground each move at a different rate, and the type sits between the planes with genuine occlusion. A single image with a slow zoom is not depth.
- **Contrast is measured on the brightest frame** the text sits over. A scrim covers the text, not the frame; a full-frame dark overlay to fix legibility is a refusal.
- **Every act publishes its progress** as a variable from 0 to 1 on its own element. Anything the kit does not cover is driven from that variable in CSS — reach for it before inventing a device.
- **Reduced motion** skips the clip fetch entirely, puts cues at their plateau and shows the still. Never a shorter animation of the same thing.

### Build, from scratch

1. **Score.** Answer the five above, write them in the ledger, get one yes.
2. **Sections.** One per beat, each with its device, its span and its copy. That table is the work order, and the tests.
3. **Basis.** Tokens from `DESIGN.md`, curves from `motion.md` §4, the grid and type checked at 390 as well as 1440.
4. **Skeleton first.** One driver, one rAF, one progress variable per act, a still per act, the reduced-motion branch — before any content is polished. The page must survive with every clip missing.
5. **Assets last.** Poster, a portrait crop, a clip with no audio track, encoded for seeking. Budget the page's weight before generating anything: two clips is a small spend, five is a product.
6. **Proof.** The act captured at its start, middle and end at 1440 and 390, in reduced motion, with the network throttled. Read the frames; a diff says nothing about composition. Two failures a still hides: **dead scroll**, where an act moves but nothing in it changes, and a cue that never reaches full opacity before the act ends.
7. **Close honestly.** Say what a green headless run does not cover: a real phone's decoder, autoplay policy, Low Power Mode and touch scrolling.

### Improve an existing sequence

Diagnose before rewriting. The fix is usually one device, not a new page.

| Symptom | Cause | Fix |
| --- | --- | --- |
| Stutter under the wheel | scroll writing `currentTime`, no lerp, seeks piling up | target plus rAF, deadband, coalescing |
| Clip freezes entering and leaving | clip mapped to pinned travel | map it across the whole visible life |
| Snap at the start of a pin | span at or below 1.0 | raise it to 1.2 or more, or make the act a flow |
| The page jumps when a pin ends | content height changes inside the stage | reserve the final height, keep the stage static |
| Phone overflows or crops the stage | desktop composition squeezed | a portrait composition per act, `100svh` not `100vh` |
| Everything feels the same | one device repeated | re-score: four families, no repeat in a row |
| Jank only on a busy page | several clips decoding at once | at most two scrubs, load by act, drop one |
| Reduced-motion users get a video | the branch was skipped | cues at plateau, still frame, full copy |

Change the failing row and stop. A broken scroll page is not a reason to rebuild the site.

### Never

Never a scroll cue, arrow or mouse icon — they are looking at the hero; they know. Never `01 / 06` counters, an eyebrow over every section, or centred copy in every act. Never text baked into an image: real markup, always. Never an invented statistic in a counter. Never autoplaying audio, or any audio on a scrub clip. Never an ending that fades into a footer — the close resolves and holds. Never fewer than four device families, the same one twice in a row, or a third scrub act. Never ship without frames at scroll positions and one phone check. (No `transition: all` or layout-property animation either — the merged rule is stated once in § Performance and helpers.)

<!-- source: greensock/gsap-skills (MIT), iart-ai/web-animation-skills (MIT) — SplitText, Flip, Lenis sync added · scroll-craft, greensock/gsap-skills (MIT) · merged from references/gsap.md and references/scroll.md -->
