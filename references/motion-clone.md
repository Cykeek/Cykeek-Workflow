# Motion clone — a reference is a recording, not an impression

When the user hands over a video, a gif, a screen recording, a screenshot or a live URL and says copy it, match it, clone it or "1:1", the work is measurement. One pass ends in a lookalike that fails on the second frame; this protocol ends in a row-by-row proof.

## The fidelity gate

Three answers, one line each, before the first line of code.

1. **Level.** *Motion-faithful*, the default: same trigger, order, stagger, timing, easing family, direction, and travel relative to the element. *Pixel-faithful* — "exactly", "1:1", "pixel-perfect": add fonts, sizes, colours, spacing, radii and assets. *Mixed* — "clone X and add Y": the additions are named and kept outside the compared region.
2. **Target.** URL and selector, file and time range, or "the toggle in the gif". Crop out browser chrome, the cursor and the recorder overlay; clone the scope given, not the page around it.
3. **Instrument.** What this session can actually do: capture a page, step a recording frame by frame, diff images. Discovered, never assumed. A still image carries no timing — when that is all there is, ask for a recording instead of inventing 300 ms.

## Capture parity

Reference and clone are captured the same way or the comparison measures the setup: viewport, crop, fps, trigger, scroll steps, wait before, seed. Those numbers go in the spec header. When they differ between the two captures the round is void, and the capture is fixed first — never the code.

## Sources, and what each one gives

- **A live page** — the best source. Declared animations, keyframes and computed styles, when the session can read them, beat any estimate made from pixels; use them, then read frames only for what they cannot express.
- **A recording** — video, screen capture, gif: frames and nothing else. A still image is not a recording; it carries no start, duration or easing.
- **Canvas, WebGL, 3D** — frames are the fallback. When a trace of shaders, uniforms and scene values is available, that is the spec; rebuilding shaders from screenshots wastes rounds.
- **Lottie** — the file is the spec: layers, keyframes, bezier handles, its own fps and in and out points. Read it, do not re-derive it from pixels.

## Reading the frames

- Probe duration, size and fps. Two frames a second first, to find the window and the crop; then 25–30 fps, or the gif's own rate. Gif timing is quantised to frame delays and often slowed by the exporter, so allow a frame; a screen recording is scaled, so convert with `distance_css = distance_video × viewport / crop_width`.
- First frame it changes is the start, last frame it changes is the end, `duration = (last − first + 1) / fps`.
- Easing from the midpoint: more than half the travel at a third of the duration is an ease-out, less than a quarter an ease-in, symmetric an ease-in-out, constant steps linear, past the end and back a spring or a back curve.
- Stagger is the gap between sibling start frames — constant, growing, or per word and character.
- Read every element that moves: shadow growth, a blur clearing, letter-spacing tightening, a border appearing, an icon rotating. And read what does not move — adding motion the reference lacks fails exactly as hard as missing it.

## The trigger, first

`t = 0` is the trigger, and it is part of the spec: load; enters the viewport; hover in and hover out, which are different timings and both get measured; click; drag, with the release velocity; a scroll position, where the motion maps to `scrollY` and stops when scrolling stops; a loop, where the period matters and the easing does not.

## The spec, before the code

One row per element per property, in `.cykeek/verify/motion-<name>.md` — 40 lines:

`# | element | property | from → to | start ms | duration ms | easing | stagger | source`

A `?` in a row is unread, not estimated. Trust numbers in this order: declared animation data the page or file exposes → tracked element positions over time → the frames themselves. Add the static facts the motion rests on: box at rest, transform-origin, z-order, overflow clipping, and for pixel work the font family, weight and size.

## Build in the project's own stack

CSS first, then the Web Animations API, then the motion library already present — `motion`/Framer in React, GSAP, Vue or Svelte transitions. A new dependency is a change the user did not ask for. Copy what makes frame zero match: initial state, `forwards` fill, origin, the clipping wrapper, and the order of transforms (individual `translate`/`scale` before a composed `transform`). Wrap non-essential motion in the reduced-motion path whether or not the reference does, and say so.

## Prove it

Capture the clone with the reference's own command, then judge each row:

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

## Failure, and the cause it usually has

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

## When to stop

Every row passes: say so with the numbers, the sheets and the files. Five rounds with no net gain on a row: stop, report the row, its last numbers and the reason — a font licence, a shader the user may not copy, a third-party embed, physics approximated. A threshold is never lowered silently; if a looser bar is worth it, name it and say why.

## Never

- Call one pass a clone.
- Compare two captures taken differently.
- Read only the first and last frame, where the easing is not.
- Invent timing from a still image.
- Wire a spring to a bezier with a different overshoot.
- Judge pixels before the fonts load, on pixel work.
- Report a match without the row numbers.

<!-- source: NiazMorshed2007/reanimate (MIT) — SKILL.md, references/capture.md, analysis.md, verify.md, implementation.md -->