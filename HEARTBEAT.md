# HEARTBEAT.md

## Status

`SKILL.md` 513 lines · 20 references · `README.md` 266 · `evals/evals.json` 305 lines, 70 cases · install copy synced to `~/.agents/skills/cykeek/`.
Last change: WebGL now has a gated domain hub — `references/webgl.md` opens only after `motion.md` §1, routes shader / scene / particle work to the three iart-ai skills, and fixes the GPU contract: one HTML file with a CDN importmap, one clock, capped DPR, explicit disposal, three frozen `?t=N` frames. The recursive four-move loop — Act → Changes → Review → Loop — now lives in `references/loop.md` §5 and is named in `AGENTS.md` §4 and `SKILL.md`, so every skill corrects its own result instead of building once; three rounds per assertion, then the delta is recorded. Counts, routing rows and verification rows land with it; no new eval. Before that: GSAP now has a canonical hub — `references/gsap.md` holds mechanics (register once, all plugins free, transform aliases and `autoAlpha`, timelines over `.delay()`, one trigger on the timeline, `scrub` XOR `toggleActions`, `ease: "none"` in scrubs, `gsap.matchMedia()`, `useGSAP` with `scope` and `ctx.revert()`, `quickTo` and utils), while `motion.md` keeps the gate and `scroll.md` keeps the score. Three routing rows, one AGENTS row, source lines and updated counts land with it; no new eval — case 26 already covers GSAP in React. Before that: Android work now has a gated hub — `references/android.md` opens only on a Gradle or manifest marker or an explicit ask, routes each job to Google's official skill, fixes eight floors (Kotlin and Compose, Material 3, edge-to-edge, window classes, one nav and DI, state in the ViewModel, catalog dependencies, no silent toolchain bump), and fences verification: nothing from memory, an unread API stays UNVERIFIED, proof is a build, a test run and a render on device, emulator or Robolectric. Design widths and the phone checklist now point at it; four evals cover the gate, the web false positive, Play Billing and an unverified API. Before that: the motion clone protocol, the git contract, the scroll pipeline.

## Pending

| Observed | Target | Status |
| --- | --- | --- |
| `.gitignore` still lists `skills/` | `.gitignore` | decide at publish — keep as a guard or drop |

## Promoted

Counts in prose are checked against files before they are written (§7.2). Intake asks before it writes; the brief precedes any file. Both live in `SKILL.md`, not here.
