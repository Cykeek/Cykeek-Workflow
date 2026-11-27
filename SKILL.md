---
name: cykeek
description: Set up this project workspace for agents — writes AGENTS.md (the process contract), DESIGN.md (the design system) and HEARTBEAT.md (the learning loop), then keeps them current. Use at the start of a session in a website, dashboard, app or motion project — when there is no AGENTS.md, when it is stale or generic, when a design reference must be cloned 1:1, or when asked to 'set up the workspace', 'lay down the rules', 'onboard the agents', or to capture and apply house rules for design, code and motion. Never edits installed skills or agent config.
compatibility: reasoning-capable and vision-capable model; a browser or screenshot tool for visual projects
---

# Cykeek — set up the workspace

Turn a project into a place where an agent already knows how to think, where to look, when to act, and what done means — before it writes a line of code.

Three files, each with one job:

| File | Job | Read |
| --- | --- | --- |
| `AGENTS.md` | how, where, when to act: routing, order, gates, budgets | every session |
| `DESIGN.md` | the design system: tokens, components, responsive, cloning | only for visual or motion work |
| `HEARTBEAT.md` | the learning loop: capture corrections, promote them | status and pending, every session |

## Start here

Two situations decide the path, and the first call is which one this is. No `AGENTS.md` in the project: run **Part 1**, seven phases, and stop at the end of it. `AGENTS.md` already there: skip Part 1, read `AGENTS.md` then `HEARTBEAT.md` status and pending, and run **Part 2** for the task in hand. Part 1 is a procedure, Part 2 a loop; the sections after them are lookup, read when their subject comes up and not before.

## Preflight — before anything else

Runs on every invocation, in both parts, before a question is asked and before a file is written. Five checks, one pass, in this order. The first hard failure ends the turn.

| # | Check | Evidence | If missing |
| --- | --- | --- | --- |
| 1 | Reasoning | the model id, plus whatever the harness reports about a reasoning or thinking mode | **STOP** |
| 2 | Image input | one bounded probe: load a small image and read it back. The tool existing proves nothing about the model | **STOP** |
| 3 | Render path | catalog scan for preview, navigate or screenshot, then one smoke call. A dev server the tool cannot reach is not a render path | **STOP** for a visual class, ask otherwise |
| 4 | Surface | one catalog pass: subagents (spawn, status, release), research (search, fetch, image), MCP servers, shell | ask once, in one message |
| 5 | Write path | first the line `AGENTS.md` already carries; else the edit tool by shape (`write\|edit\|create\|patch\|str_replace`), else the OS primitive for this shell, proved by one scratch write and a read-back | **STOP** — no file can be written |

The OS costs no call: the shell tool and the shape of the cwd answer it. The scratch probe runs only when the catalog has no edit tool; otherwise the first real write is the probe. A recorded path whose OS no longer matches is stale — re-resolve it and replace the line rather than arguing with it. **The model gates are properties of the model, not the session.** The contract assumes a model that reasons over its own evidence and can see a screenshot; without both, the workspace produced is one nobody can trust. The third stop is plainer: a session that cannot write a file cannot write one.

**STOP** writes nothing and ends the turn:

```
Cykeek will not run in this session: <what is missing>.

- Model: <id, or unknown> — <reasoning not reported | the image probe returned no image>.
- Why it blocks: <the reference and every V case are read at 1440 and 390 | the contract is only as strong as the model holding the goal>.
- Fix: switch to a model whose card lists reasoning and image input, then re-run. Nothing has been changed.
```

Name traits, never brands. A model list goes stale; a capability does not. **Ask** is one message for every gap at once, then wait:

```
Cykeek can run here, with these limits:
- no subagents — the seven states run serially in one agent
- no preview — visual cases are measured by DOM and marked UNVERIFIED
Continue, or change the session first?
```

Silence is not yes. A yes becomes an accepted deviation in `HEARTBEAT.md`. **Pass** is one line — `Preflight: reasoning, vision, preview, subagents, write path — reached.` — then Phase 0.

**Record it twice.** `.cykeek/verify/capability.md`, sixteen lines: model, reasoning, image, render, capture, subagents, research, mcp, shell, os, write, one evidence line per check, and the date — the session's evidence, re-confirmed on every run. The proven write path also goes back into the project's `AGENTS.md` §3, as one line after the first successful write: `Write path: <os> · <chain> · <date>`. Find it with `^Write path:`, replace it in place, never append a second one, never write outside the `cykeek:begin` markers. That line is what stops the next session starting over. If the edit to `AGENTS.md` fails, say so once and continue — the capability file still holds it. A hand-written file with no markers is not touched: the line is proposed in one sentence at close instead.

## Part 1 — Set up a workspace

Seven phases. Run them in order; never skip to writing. This part is one agent's work — each phase consumes what the one before found — so it is never split across workers.

### Phase 0 — Detect, silently

Read the repo before asking a human anything. The filesystem is the first source of truth.

| Look for | Answers |
| --- | --- |
| `package.json`, lockfile | stack, versions, package manager, scripts |
| `tsconfig.json` | TypeScript or not |
| `tailwind.config.*`, `@import 'tailwindcss'` | Tailwind, and which major |
| `components.json` | shadcn present |
| `motion`, `framer-motion`, `gsap`, `react-native-reanimated` | motion library |
| `app/`, `pages/`, `src/`, `routes/` | router and source root |
| `**/components/`, `**/ui/` | where components live |
| `app/globals.css`, `styles/*.css` | where tokens live |
| `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md`, `CLAUDE.md`, `.cursor/rules` | existing contract to merge |
| test runner config | vitest, jest or playwright |
| `git status` | greenfield or live project |
| `app.json`, `expo` dependency | React Native or Expo |
| browser or preview tools in the catalog | which of navigate, observe, interact, capture exist — see Discovery |

Capabilities come from Preflight, which already ran. The gates left for this phase are about the route: Figma MCP tools in the session, and `node_modules/@ui-sdk/*`. Stop after two hops. A missing config is a fact, not a mystery. A repo with no build and no UI — documentation, a skill, a library — is its own class: say so in one line, and it carries through the brief as `class: non-visual` rather than being forced into a website shape.

### Phase 1 — Interview, in two rounds at most

A form that collects one word per line produces a contract that fits every project and therefore none. The contract is only as good as the problem statement behind it, so ask properly — but once.

**Round 1 — five questions, one message.** Open, not multiple choice, each with the detected default attached so a thin answer is still usable.

| # | Question | What the answer changes |
| --- | --- | --- |
| 1 | What problem does this solve, and for whom? | the goal line every session reads first |
| 2 | What does done look like — the one screen or flow that must be right? | the V cases and the gate |
| 3 | What exists today: nothing, a Figma, a screenshot, a half-built repo, a live URL? | the route, and whether the clone protocol arms |
| 4 | What should it feel like — a product you would point at, or three adjectives? | design direction and motion budget |
| 5 | What cannot change — stack, brand, deadline, integrations, people? | constraints, and the rows that get dropped |

Print the detected facts above them in one line, so nothing on the list is already answered, and skip any question the invocation answered itself — `/cykeek a fintech dashboard, screenshot attached` shortens the list to two. Answerable in a sentence each; `defaults` still works, and `unknown` is a valid answer that becomes a stated assumption.

**Round 2 — only the gaps, at most three questions.** Ask only where the answer changes a decision: the riskiest unknown, the most expensive decision, and what would make them reject the result. Never re-ask, never ask what the repo answered, never run a third round. Then stop asking: whatever is still open becomes an assumption in the brief — a wrong assumption is corrected in one line, a questionnaire is not answered at all.

### Phase 2 — The brief, before anything is written

Compress the answers into the thing the contract gets built from. Printed once, corrected in one reply. This is the step that turns a request into a goal an agent can hold for a whole session.

```
Goal     <the problem in one sentence — not the deliverable>
For      <who, in what situation, how often>
Done     <two or three observable outcomes>
Not      <what this is explicitly not>
Unknown  <one assumption per open question>
Fixed    <stack, brand, deadline, people>
Feel     <the product to point at, or the three adjectives>
Shape    1. <workstream> — <the routing row it uses>
         2. <workstream> — <the routing row>
         3. <workstream> — <the routing row>
Risky    <the two decisions that will decide this>
First    <the smallest thing that proves the direction>
```

Three rules. The goal names the problem, not the screen — "contractors cannot tell which invoice is overdue" beats "an invoices dashboard". `Shape` is ordered by dependency, three to five workstreams, one routing row each: it is what makes an agent act on the problem instead of on the request. `First` must be buildable in one pass. A line that cannot be filled says `assumed: ...` rather than staying blank or becoming a question.

### Phase 3 — Tailor, so the contract is smaller than a generic one

| Brief line | Effect |
| --- | --- |
| `Shape` | keep only the routing rows those workstreams can reach; drop the rest entirely |
| `Done` | becomes the first V cases and the gate in `AGENTS.md` §8 |
| `Risky` | becomes the two ordering rules in `AGENTS.md` §6; `First` becomes the first stop |
| `Feel` — a product to point at | arm the clone protocol, pre-seed a `Reference profile` block in `DESIGN.md` |
| `Feel` — quiet or none | keep motion tokens, drop the motion rows from routing |
| `Feel` — expressive | activate choreography, springs and pointer response; scroll rows only when the brief names scroll |
| `Fixed` — prototype delivery | drop release gates and the `react`, `release` and audit rows; keep design and craft |
| `Fixed` — production delivery | full gates: accessibility, performance, diagnostics, release |
| dashboard or app class | density modes, table and chart rules, the chart section of `design.md` |
| `class: non-visual` | drop every visual row; keep craft and release; write `DESIGN.md` only if UI is planned |
| RN/Expo | drop web-only rows; motion follows platform timing, not the web ladder |

A marketing prototype must end up materially shorter than a production dashboard. If it does not, tailoring failed. Tailoring decides which rows survive; it never rewrites a row.

### Phase 4 — Manifest, then write

Print at most ten lines — files, line counts, active route count, reference profile, the `Unknown` lines as assumptions — then write; do not narrate the content. The three file contracts live in `## What AGENTS.md contains`, `## What DESIGN.md contains` and `## HEARTBEAT.md — the learning loop`; read them in that order before writing.

### Phase 5 — Apply to the codebase

One absolute rule: **create freely, modify only inside markers, install nothing.**

| Target | Action |
| --- | --- |
| `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md`, `.cykeek/**` | create, or replace the `cykeek:begin` / `cykeek:end` block if present |
| token stylesheet or `globals.css` | create if absent; else append a marked block, previewed |
| Tailwind theme or config | marked block only, previewed, never rewritten |
| components directory | create if absent |
| `CLAUDE.md`, `.cursor/rules`, `.github/copilot-instructions.md` | add one pointer line to `AGENTS.md`, only if the file already exists |
| dependencies, lockfiles, lint or format config, existing source | never touched |

If a hand-written `AGENTS.md` exists, Phase 5 becomes merge: show the block diff, insert, leave everything outside the markers byte-identical.

### Phase 6 — Hand off in three lines

Workspace ready, N files written, the one route the next task should take, and the single next action. Nothing else.

## Part 2 — Work in a workspace

Per task, eight gates in order. The moves are `references/loop.md`; the gates, the show block and where a gate sends the work back are `references/pipeline.md`; the split is in `references/orchestration.md`. **Before gate 1, the receipt.** Nothing is written, run or dispatched until one line exists: `Rules: AGENTS.md v<n> · <section cited> · owns: <concept> · refuse: <what> · proof: <check>` (`references/loop.md` §0). The card is the top of `AGENTS.md`; the section is cited rather than paraphrased, because a citation you cannot make is a read you did not do. One receipt covers a run of work on one area, refreshed when the area or the goal changes and after any fail, any correction and any returned report. When the file on disk differs from what the receipt cites, the receipt is stale and the read comes first.

1. **Intent** — `AGENTS.md` read, `HEARTBEAT.md` pending, the goal restated in one line; the want and the why in one sentence, or the one question that answers it.
2. **Route** — one reference open, whole, at its routing row. A problem statement classifies through `references/flows.md`; a technology the agent cannot specify goes through `references/research.md` first.
3. **Brief** — five lines: what, why, the shape, what it refuses, what proves it. A one-round change skips it and stops once, with the result.
4. **Contract** — the numbers, states and widths, and the row table when the work splits (`references/orchestration.md`): every row carries the assertion that fails if it is wrong, and a row that says `none` carries its reason. Nothing downstream adds a row.
5. **Slice** — one row built and verified in this round, one owner per row; one consolidated outcome per round, a delta returned as an order and nothing else; the project's tokens and components, nothing outside the markers.
6. **Prove** — the code gate first (`references/craft.md` § The gate), then the assertion re-run, read and quoted at two widths (`## Visual tests`), the simplest and riskiest cases seen yourself.
7. **Show** — the five-line block at `references/pipeline.md` § The show block, one question, then wait. A gate that fails returns to the gate it names, never to the top; the mechanical clean pass runs before the question, never after.
8. **Close** — the diff sweep, the workers released, the ledger closed, one local commit and never a push (`references/git.md`).

A visible artifact that ends without a visual test having run is not finished, and nothing is released to the user as done before they have seen it. The one exception is a capability the session does not have, and that is recorded as UNVERIFIED rather than smoothed over. Every skill runs the same recursive loop inside Prove: **Act → Changes → Review → Loop** (`references/loop.md` §5). Act builds the smallest slice; Changes names one gap in one line — which property, which instant, which width; Review re-renders the same assertion and records PASS or the measured delta; Loop returns to Act with the named fix, or stops after three rounds with no movement and goes back to Decide. No domain file repeats this. WebGL uses it for three `?t=N` frames, GSAP for timeline positions, scroll for three positions per act. A round with no named gap is done — do not improve on taste.

## Re-invocation is a refresh

Diff what is on disk against what is generated, report what went stale — new motion library, deleted shadcn, changed framework major, a reference profile that no longer matches — ask only about the changed facts, update. Running it twice in a row must produce no diff.

## Conflict rules

These are why a 52-skill setup can be worse than none. Encode them in the generated `AGENTS.md` too.

1. **The reference is already loaded; a live skill is the exception.** Load at most one per turn, and only when the reference explicitly hands off to it.
2. **Never load a reviewing skill and a building skill for the same artifact** in one turn.
3. **Canned Initial-Response skills** — `animate`, `animate-expo`, `choreograph`, `apple-design`, `break-ui`, `animation-performance`, `animation-vocabulary`, `prototype`, `pick-ui-library`, `improve-animations`, `find-animation-opportunities`, `emil-design-eng`, `mobile-native`. Each prints a fixed line and stops, but only when invoked *without* a question. Invoke them with the task stated, and never relay the line.
4. **`unslop-ui` and `design-taste-frontend` are never loaded together.** Build with one, clean with the other.
5. **Taste ships in `references/design.md`.** A taste skill, if installed, extends it; it never replaces it, and two taste skills are never loaded at once.
6. **Domain and enterprise skills are scope-gated** to non-UI work. A landing page never summons DDD.
7. **Environment gates are checked before routing**, and an unavailable route is marked skip-and-degrade, never left to dead-end.
8. **Installed skills are read-only.** No edit to `~/.agents/**`, `.claude/**`, `.cursor/**`, `.codex/**`. Project-root files only.

## The references

Twenty-six files ship beside this one. They hold the operative content — numbers, thresholds, decision order, stop conditions — distilled from the design, motion, engineering and architecture practice this skill encodes. They are why a routing row never dies when a machine lacks a skill.

| File | Read it for |
| --- | --- |
| `references/loop.md` | every task, before the domain file — the six moves and the token contract |
| `references/pipeline.md` | the eight gates in order, the show block, where each gate sends the work back |
| `references/flows.md` | a problem statement — which flow runs before a domain file |
| `references/debug.md` | a bug or a wrong result — reproduce, locate, explain, then fix |
| `references/orchestration.md` | a task that splits across agents — the orchestrator's contract |
| `references/research.md` | a library or technique whose current shape you cannot state from memory |
| `references/design.md` | the design gate, then UX and hierarchy, direction, tokens, copy, review |
| `references/motion.md` | whether to animate, and how it should feel |
| `references/gsap.md` | GSAP mechanics — tweens, timelines, ScrollTrigger, React cleanup, utils |
| `references/motion-audit.md` | motion review, frame budget, subtraction |
| `references/scroll.md` | a scroll-driven page — the score, the shapes, the device kit, the timeline, the build, the diagnosis |
| `references/webgl.md` | WebGL gate — shader, Three.js scene, particles, GPU disposal, frozen-frame proof |
| `references/web-animation.md` | specialised web motion — SVG, Lottie, glass, ASCII, Next page transitions, route |
| `references/motion-clone.md` | a shared video, gif, screenshot or link to be matched — fidelity, frames, the spec, the proof |
| `references/ui-engineering.md` | building the interface, accessibility, phones, worst-case data |
| `references/android.md` | an Android app or module — the gate, the official routes, the floors, the fence |
| `references/libraries.md` | component libraries, shadcn, toasts |
| `references/figma.md` | Figma links |
| `references/react.md` | React and Next performance |
| `references/craft.md` | the code gate, the ladder, then five triggered moments — naming, routines, modules, knowledge, changing code — and untangle |
| `references/review.md` | reviewing a change, auditing a repo, the `shortcut:` ledger |
| `references/security.md` | a security audit or question — the gate, the three verdicts, the coverage ledger, verification, severity |
| `references/security-classes.md` | what to look for by surface, and the question that settles each class |
| `references/domain.md` | domain modelling and data systems |
| `references/release.md` | production failure handling, test hygiene |
| `references/git.md` | a commit, an amend, a push — the gate, the message, the attribution scrub |

## The routing table

Rows are keyed by the artifact being produced, not by a skill. Read the file in the second column. A skill name in the third column is depth when it happens to be installed, and never a precondition — if it is absent, the reference is the whole contract and the row still works. **Precedence when guidance conflicts:** project conventions in `AGENTS.md` and `DESIGN.md` first, then a live skill on its own subject, then the reference, and the non-negotiables as a floor nothing crosses without a recorded convention.

| Task | Read | Then, if installed | Never |
| --- | --- | --- | --- |
| Any task, before the rest | `references/loop.md` | — | starting a domain reference instead |
| Any multi-step task, before the first edit | `references/pipeline.md` § The eight gates | — | building before the intent is one sentence |
| "What changed?" or "show me" | `references/pipeline.md` § The show block | — | a summary with no file and no proof |
| A gate failed, or the user says it is wrong | `references/pipeline.md` § When it goes back | — | patching until it looks better |
| "I don't know what it should look like" | `references/flows.md` § Prototype, then `design.md` | `prototype`, `frontend-design` | committing to the first idea |
| "This codebase is a mess" | `references/flows.md` § Untangle | `untangle-codebase` | page-by-page tidying |
| "You keep getting this wrong" | `references/flows.md` § Capture | `learn-taste` | fixing the code instead of the rule |
| Work that splits across agents | `references/orchestration.md`, after the domain row | subagent tools, if present | two workers writing one file |
| Integrate a library or technique you do not know | `references/research.md` before the domain row — the research pass is not the domain read | `find-skills` | writing the API from memory |
| "How do other sites do X?" | `references/research.md` §6 | — | copying the look instead of the structure |
| Any screen, before the first visual decision | `references/design.md` § The gate, then the row for its kind | a design skill that names its own gate | drawing the layout before answering it |
| New marketing page or site | `references/design.md`, then `references/motion.md` | `design-taste-frontend` | `unslop-ui` in the same turn |
| Dashboard or app screen | `references/ui-engineering.md` | `frontend-ui-engineering` | `frontend-design` |
| Closing pass on copy and charts | `references/design.md` | `unslop-ui` | `design-taste-frontend` |
| One animation on the web | `references/motion.md` | `animate` | `animate-expo` |
| GSAP is the project's library | `references/motion.md` §1, `references/gsap.md` §1–3 | `gsap-core`, `gsap-timeline` | a GSAP answer before the gate, or `.delay()` chains |
| Scroll reveals across a set of sections | `references/motion.md` §1, §5, then `references/orchestration.md` | `find-animation-opportunities` | animating every section by default |
| A scroll-driven page or sequence, asked for by name | `references/scroll.md` § The score, then § The shape | — | a section plan before the score |
| GSAP ScrollTrigger on that page | `references/scroll.md` § The score, then `references/gsap.md` §4 | `gsap-scrolltrigger` | a trigger on a timeline child, or `scrub` with `toggleActions` |
| A video, gif, still or link the user wants matched | `references/motion-clone.md` § The fidelity gate | `reanimate` | cloning from a still, or one pass called a match |
| One animation in React Native | outside this skill's scope — say so | `animate-expo` | `animate` |
| GSAP in React or Next | `references/gsap.md` §5 | `gsap-react` | an unscoped selector, or a tween without `ctx.revert()` |
| WebGL, GLSL, Three.js, R3F, GLTF, particles, flow field or constellation | `references/motion.md` §1, `references/webgl.md` §1 | `shader-glsl`, `threejs-animation`, `particle-system` | GPU before the gate, or two skills in one turn |
| GLSL fragment shader or GPU transition | `references/webgl.md` §3 | `shader-glsl` | hand-rolled noise for a full-screen effect |
| Three.js scene, camera, GLTF blend, instancing, R3F | `references/webgl.md` §4 | `threejs-animation` | `scene.remove()` as cleanup |
| Particle, confetti, snow, flow field, connected dots, GPU `Points` | `references/webgl.md` §5 | `particle-system` | O(n²) pair checks past ~300 particles |
| SVG draw-on, path morph, motion-along-path, animated icon | `references/web-animation.md` §3 | `svg-animation` | raw `d` morph with mismatched point counts |
| Lottie or dotLottie integration, control, theming, export | `references/web-animation.md` §4 | `lottie-animation` | re-exporting per brand instead of runtime theme |
| Frosted glass or liquid glass panel | `references/web-animation.md` §5 | `glassmorphism` | backdrop-filter over a flat background |
| ASCII field, image/video-to-ASCII, `AsciiEffect` | `references/web-animation.md` §6 | `ascii-animation` | square-pixel sampling without cell-aspect correction |
| Next App Router page transition, `AnimatePresence` exit | `references/web-animation.md` §7 | `page-transition-animation` | `layout.tsx` for per-route enter animation |
| Micro-interaction, hover/press, toggle, toast, drawer, list reorder | `references/motion.md` §2, §6, `references/web-animation.md` §1 | `micro-interaction` | animating width/height instead of scale |
| Reduced-motion tiering, WCAG 2.3.3 | `references/motion.md` §7 | `accessible-animation` | all-or-nothing kill switch |
| Several things moving at once | `references/motion.md` §5 | `choreograph` | `animate` alone |
| Drag, swipe, sheet, drawer | `references/motion.md` §6 | `gestures`, `apple-design` | both at once |
| Route, page or shared-element transitions | `references/motion.md` | `vercel-react-view-transitions` | hand-rolled `AnimatePresence` routing |
| Motion drops frames | `references/motion-audit.md` | `animation-performance` | `review-animations` |
| Audit motion in one diff | `references/motion-audit.md` | `review-animations` | `animate` |
| Audit motion across a codebase | `references/motion-audit.md` | `improve-animations` | `review-animations` |
| Find where motion should exist | `references/motion-audit.md` | `find-animation-opportunities` | `animate` |
| Too much motion | `references/motion-audit.md` | `remove-animations` | `improve-animations` |
| Name an effect | `references/motion-audit.md` | `animation-vocabulary` | anything else |
| Design review of a screenshot or URL | `references/design.md` | `checklist-design` | `web-design-guidelines` |
| Polish one component interaction | `references/ui-engineering.md` | `emil-design-eng` | `checklist-design` |
| Code-level UI and accessibility audit | `references/ui-engineering.md` | `web-design-guidelines` | `checklist-design` |
| Web app on a phone | `references/ui-engineering.md` | `mobile-native` | `animate-expo` |
| An Android app or module — `settings.gradle`, `AndroidManifest.xml`, `gradlew` present, or the user asks for one | `references/android.md` § The gate | — | Android advice in a web project, or an API recalled from memory |
| Worst-case data and edge cases | `references/ui-engineering.md` | `break-ui` | `checklist-design` |
| shadcn components | `references/libraries.md` | `shadcn` | `pick-ui-library` |
| Choose a UI library | `references/libraries.md` | `pick-ui-library` | `shadcn` |
| Project uses `@ui-sdk/*` | `references/libraries.md` | `ui-sdk` | — |
| Toasts | `references/libraries.md` | `ask-sonner` | — |
| Figma to code | `references/figma.md`, then the Figma section below | `figma-design-to-code` | `figma-use` |
| Write into Figma | outside this skill's scope — say so | `figma-use` | `figma-design-to-code` |
| Any code change, before the first line | `references/craft.md` § The gate | `clean-code`, `code-complete` | narrating the gate instead of answering it |
| A fix growing past its problem, or "this is over-engineered" | `references/craft.md` § The ladder | `ponytail` | planning the bigger version first |
| A smell, a cleanup, a rewrite tempting | `references/craft.md` § Changing code | `refactoring`, `refactoring-guru` | rewriting |
| A bug, a failure, a wrong result | `references/debug.md` | — | a fix before the cause is named |
| Legacy or unfamiliar code | `references/craft.md` § Untangle, `flows.md` § Untangle | `untangle-codebase`, `working-effectively-with-legacy-code` | page-by-page tidying |
| Boundary, layering, module or API shape | `references/craft.md` § Modules | `a-philosophy-of-software-design`, `clean-architecture` | pattern-first |
| Review a diff, a commit or a PR | `references/review.md` § A change, § The report | `ponytail-review` | a finding with no concrete case |
| Answering a code review | `references/review.md` § Receiving | — | agreeing before checking the code |
| Audit a whole codebase, or find what to delete | `references/review.md` § A repo | `ponytail-audit` | file-by-file reading with no ranking |
| Where we deferred something, and the trigger to revisit | `references/review.md` § The ledger | `ponytail-debt` | treating a named limit as a bug |
| A security audit, pen test, "find vulnerabilities", or report files requested | `references/security.md` § The gate, then § The six phases; classes from `security-classes.md` | — | a finding with no principal, control or crossed boundary |
| A security question, a triage, or a change to auth, sessions, permissions, uploads, payments or secrets | `references/security.md` § What a finding needs, then the class row | — | a severity on an unproven claim, or probing a live system |
| Domain rules, services or data flows | `references/domain.md` | the four DDD skills, `patterns-of-enterprise-application-architecture`, `designing-data-intensive-applications` | any UI task |
| Production failure handling, or tests that guard nothing | `references/release.md` | `release-it`, `prune-tests` | prototype delivery |
| React diagnostics before commit | `references/react.md` | `react-doctor` | prototype delivery |
| Any commit, amend or push | `references/git.md` § The gate | — | a commit nobody asked for, a push nobody asked for |
| A skill I do not have | the reference above it still governs | `find-skills` | — |

## Discovery before action

Tool names in this skill are examples of a shape, never a claim of availability. Harnesses differ, some ship no browser at all, and an instruction that names a tool the session does not have fails silently — the agent reports done having verified nothing. So every session resolves its own surface first. This cuts both ways: it applies to the Figma rows and the browser rows equally.

1. **Filter the catalog already in context.** It is free. Look for `navigate|goto|url|open`, `snapshot|screenshot|dom|aria|inspect`, `click|type|press|scroll|hover`, `evaluate|script|console|network`, `playwright|puppeteer|chrome|devtools|browser`.
2. **Read the contract of two or three candidates.** Required arguments, return shape, and any precondition the description states. Do not guess an argument shape.
3. **Make one smoke call** — the cheapest read-only operation the schema allows: a status, a list, a current page. Never open with a click.
4. **Map what exists onto the preflight checks.** Anything unmapped is a gap the user hears about before the work starts.

**Catalog patterns.** Orchestration: `delegate|subagent|task|agent|spawn`, with spawn, status and release all present. Research: `search|fetch|browse` plus `image|screenshot`. Never assume a tool because a skill names one, never fake a roster, and treat a tool missing from the catalog as unproven rather than absent: lazily attached servers get one bounded attempt with the known name. With no search tool, every outside claim is marked as such.

A partial surface is normal and usable: navigate plus capture with no interact still passes every static case and marks interaction unverified. A model without reasoning or vision is the one surface that is not usable — that is the preflight stop.

**Token discipline.** Filter before calling. One smoke call total, not one per candidate. Snapshot text before screenshots — an image costs far more than lines, and only geometry or colour justifies one. Scope a snapshot to a region when the tool allows it. Write the mapping down once per session; re-deriving it every turn is the expensive failure.

## Visual tests

A build that compiles and a suite that passes prove the code runs. They prove nothing about what it looks like. So every visual target is a test case in the same suite as the code tests, and it blocks completion exactly the way a red unit test does.

| ID | Stop | Viewport | Assertion | Status |
| --- | --- | --- | --- | --- |
| V1 | `/pricing` | 1440 | three tiers, 24px gutters, equal card heights | PASS |
| V2 | `/pricing` | 390 | tiers stack, no horizontal scroll | FAIL |
| V3 | hero CTA hover | 1440 | hover fill, press feedback, no layout shift | UNVERIFIED |

On Android the viewport column is the window class and its size, and the render path is an emulator, a device or Robolectric — `android.md` § Prove it.

Three states and no others: PASS, FAIL with the measured delta, UNVERIFIED with the missing capability. A suite holding a single FAIL or a single UNVERIFIED is a failing suite. There is no mostly done.

**The gate.** Three conditions, all required: the build is clean, the code tests pass, and every V case reads PASS or has been approved. Tests satisfy one of the three.

**Two exit conditions, and only two.**

1. **Match** — every V case reads PASS.
2. **Approval** — the user explicitly accepts the remaining deltas, recorded as accepted deviations with the date, the reason and the stop, in `DESIGN.md` under the reference profile and in `HEARTBEAT.md` as a confirmation.

Silence is not approval. A green build is not approval. "Tests pass" is not approval. An agent that has not seen the page has not finished, however clean the diff. **Plateau.** If two consecutive rounds reduce no delta, stop and bring the list to the user — which cases, which viewport, what was tried — as a question rather than a handoff. That is the only way a round ends without one of the two conditions above.

**The loop.**

1. Turn the brief or the attachment into numbered cases: one observable assertion each.
2. Discover the surface, and mark unmappable cases UNVERIFIED.
3. Dive the stops in order — reached the way a user would, by nav, hash or scroll — capturing each separately. A single full-page screenshot is not a dive.
4. Compare and record PASS or the measured delta. Measured, not "looks close".
5. Fix the deltas, and only the deltas.
6. Re-dive from the first case. A fix at one stop routinely breaks another.
7. Repeat until one of the two exit conditions holds.

Never edit a case to match the result. When the reference and the implementation disagree, the reference wins and the deviation is reported. **Across sessions:** the case list and its statuses live in `.cykeek/verify/<task-slug>.md`, so a later session resumes the same cases instead of redefining them — that is what makes the loop recursive rather than merely repeated.

## Non-negotiables

These hold even when no skill is loaded. Keep the list at twenty lines; the long form belongs in `DESIGN.md`. **A session that acted without a receipt is a FAIL, however good the output** — the receipt is the only evidence that the rules were read rather than remembered.

**Design.** A screen is designed for a job before it looks like anything: who it is for, the one action, the order of importance, every state — the five answers at `references/design.md` § The gate, and the design card at the top of `DESIGN.md`. No eyebrows, kickers, `01/02/03` numbering, decorative badges, arrows or edge stripes, and no filler subtitle under a heading. One accent colour, never default violet. At most two typefaces, size-specific tracking, body 16px or larger, measure 60 to 75 characters. Spacing from a 4/8pt token scale; equal optical edges. Hierarchy from size, weight, space and contrast — not from boxes and shadows. Real content and real product UI, never lorem.

**Motion.** `--ease-out: cubic-bezier(0.23,1,0.32,1)`, `--ease-in-out: cubic-bezier(0.77,0,0.175,1)`, `--ease-drawer: cubic-bezier(0.32,0.72,0,1)`. Never `ease-in` on UI, never built-in `ease-out`. Press 100 to 160ms, tooltip 125 to 200, dropdown 150 to 250, modal, drawer, sheet and toast 200 to 500, everything else under 300ms. `transform` and `opacity` only. Never `scale(0)`. Never `transition: all`. Anything seen 100+ times a day, and anything keyboard-triggered, gets no animation. Stagger 30 to 80ms, non-uniform, never on high-frequency UI. Exits mirror entrances and are shorter. Reduced motion is fewer and gentler, not zero. A scroll-driven page answers `references/scroll.md` § The score before its first section — four to seven beats, one peak, one shape, four device families and never the same one twice in a row — and proves itself with frames at scroll positions, never one screenshot.

**Code.** Server components by default, `use client` at the leaf. No state updates inside rAF, pointer or scroll handlers. No `useEffect` for derived state. Stable keys. Explicit media dimensions. Virtualize or paginate past 100 rows. Semantic, labelled, keyboard-reachable, focus-visible, contrast 4.5:1 or better, touch targets 44 by 44. `min-width: 0` on flex text columns, `overflow-wrap: anywhere` on emails and URLs, `tabular-nums` on changing numbers, `Intl` formatting for numbers and plurals. Never `dangerouslySetInnerHTML` with user data. A task is not done until it has been rendered and compared at 1440 and 390 — passing tests is the prerequisite, not the conclusion. Before the first line: the five answers at `references/craft.md` § The gate, and the receipt at `references/loop.md` §0. Commits stay local until the user asks for a push: one per logical change, a follow-up edit amends it, the message says why, and no agent signs it.

## What `AGENTS.md` contains

Fourteen sections, in this order. Cap 240 lines.

0. **Rules card** — twelve lines, first, before anything else: the precedence order; the receipt line and when one is required; the two gates — code before the first line, design before the first visual decision; the never-list; the five events that force a full re-read. It is what an agent reads before it acts, so it survives a long session and a compaction that the rest of the file does not.
1. **Header** — what the file is, precedence of project over user defaults, how to regenerate.
2. **Goal and facts** — the goal sentence, who it is for, what done means, what this is not, then stack and versions, package manager, scripts, directories, UI, styling and motion libraries, delivery mode.
3. **Session start** — read this file, then `HEARTBEAT.md` status and pending; read `DESIGN.md` only when the task touches UI, visual or motion. The preflight verdict lives in `.cykeek/verify/capability.md`; the OS and the proven write path are written at the end of this section and are used before any probe. Restate the goal in one line before starting, so the session is aimed before it is busy.
4. **Working loop** — six moves in order: frame (the artifact and the one reference governing it), decide (should it exist at all), specify (numbers, states, widths, V-cases), build (dispatch independent slices where the harness allows, integrate only accepted reports, else the smallest visible slice), prove (render, compare, iterate on the failing assertion), close (three lines, a ledger, one local commit). One line per move. Each move is a gate with a named destination: a gate that fails returns to the gate `references/pipeline.md` names, and the agent waits at a stop with the five-line show block. Compress for small changes; proof is never the move that gets skipped.
5. **Routing table** — the active subset, three columns: the task, the `references/` file to open, and what never to do here. One reference per task, opened when its row is reached; a matching skill may be opened afterwards for depth, and its absence changes nothing.
6. **Ordering rules** — the four or five sequences that matter for this project class. For a Figma project the binding one is gate, discover, screenshot, traverse, batch, build, compare.
7. **Non-negotiables** — the twenty lines, and in the code and design ones the gates and the receipt, with `references/craft.md` § The gate and `references/design.md` § The gate as the long forms.
8. **Gates and definition of done** — per task class.
9. **Never** — the automatic blocks: an unasked commit or push, an agent trailer, an edit outside the markers, an install, an unverified ship.
10. **Heartbeat** — when to capture, when to promote, which file to edit.
11. **Verification** — the preflight verdict (model, reasoning, image, render, then the discovered names), the V-case schema, the gate, the two exit conditions, the ledger path, the commit read back — subject, why, no trailer, still local — and for a cloned reference the spec rows, the timing numbers and the sheets.
12. **Orchestration** — the contract table, whose rows are the work orders and the tests at once; the seven states from `PLANNED` to `SHIPPED`; the ledger at `.cykeek/verify/<task>.md`; the caps (40-line contract, 12-line assignment, 8-line outcome, 3 rows per batch, 4 rounds); the three stops; the rule that whoever builds never grades; keep-alive until the user approves; only the orchestrator commits, and only locally; and the one-line degradation to serial work with no subagents.
13. **Research** — when a request names a technology the agent cannot state from memory: the card at `.cykeek/research/<slug>.md` with its nine slots, the source ladder, the five-query cap, the spike before integration, and the rule that no dependency enters without a version, a licence and a cleanup path.

Wrap the generated region in `<!-- cykeek:begin -->` and `<!-- cykeek:end -->`.

## What `DESIGN.md` contains

A system, not a mood board. Fifteen sections, cap 300 lines.

| # | Section | Content |
| --- | --- | --- |
| 0 | Design card | twelve lines at the top, the template at `references/design.md` § The card: kind, what is sold, one action, the one thing, palette and type rules, refusals, widths, states, review mode, and the `Design card v1 · <date>` line every return cites |
| 1 | Principles | six named principles, each with the decision it forces |
| 2 | Reference profiles | one block per client reference: source, extracted ratios, allowed deviations, verification stops. Figma profiles also carry file, frame and node count |
| 3 | Colour | neutral ramp, one accent, semantics, contrast pairings, prohibitions |
| 4 | Type | faces, scale with tracking and leading pairs, measure, numerals |
| 5 | Space and grid | 4/8pt scale, container widths, columns per breakpoint, optical edges |
| 6 | Elevation and surfaces | radius scale, border versus shadow, one elevation accent |
| 7 | Motion tokens | three curves, duration ladder, spring defaults, frequency gate, stagger budget |
| 8 | Iconography | stroke, size, optical alignment |
| 9 | Density modes | comfortable and compact, for dashboards |
| 10 | Responsive ladder | the rules below |
| 11 | Component contracts | index of `.cykeek/components/*.md` plus the schema |
| 12 | Composition patterns | page, dashboard shell, auth, empty, error, loading |
| 13 | Content voice | plain language, forbidden filler, heading and CTA conventions |
| 14 | Acceptance checks | emitted as numbered V cases: stop, viewport, assertion, status |

A component spec is written **before** the component is coded, one file each, sixty lines: purpose, anatomy, tokens consumed, states (default, hover, press, focus, disabled, loading, empty, error), responsive behaviour per breakpoint, motion, accessibility, anti-patterns, acceptance checks, open questions.

## Responsive ladder

Lives in `DESIGN.md` section 10, summarised in `AGENTS.md`.

- Breakpoints 360, 390, 768, 1024, 1280, 1440+, with a container maximum and gutter per step.
- Type by `clamp()`, tracking and leading corrected per step, body never below 16px.
- Touch targets 44 by 44. Every hover-only affordance needs a press or always-visible equivalent, and hover motion is gated behind `prefers-hover-capable` media queries: `(hover: hover) and (pointer: fine)`.
- Dashboards: sidebar becomes a bottom bar or sheet; KPI grid 4 then 2 then 1; wide tables become card-per-row or horizontal scroll with a sticky first column, chosen per table and never mixed; legends move below the plot; charts reflow instead of shrinking labels.
- Bind the phone fixes: no sticky hover, no tap-highlight flash, `100dvh` not `100vh`, inputs 16px or larger so iOS does not zoom, no pull-to-refresh hijack, safe-area insets on notch and gesture bar.
- Verify every screen at 360, 768 and 1440 with no horizontal scroll and no clipped control.

## Reference clone protocol

Every dashboard looks different, which is why this is a protocol and not a style. When a reference is shared, this order is binding, and coding may not start before step 4.

1. **Inventory** — regions, grid, components, chart types, density, colour count, type scale, motion cues. Write what is seen, not what is inferred.
2. **Extract ratios, not pixels** — spacing unit, radius ratios, type steps, gutter-to-container ratio, luminance steps. These become a `Reference profile` block in `DESIGN.md` section 2.
3. **Reconcile** — appearance follows the reference; structure, accessibility, motion and responsiveness follow house rules. Where they conflict, say so and record the deviation. Three-accent designs, 12px body text and unmapped focus states get adjusted, then flagged in the handover.
4. **Spec first** — one `.cykeek/components/<name>.md` per new component.
5. **Build in dependency order** — tokens, primitives, components, shell, screen.
6. **Pixel pass** — side by side at the reference native width and at 390; compare spacing, alignment, radii and weights, not just colour; iterate until the remaining differences are listed rather than hidden.
7. **Record** — profile, deviations and the close-enough decisions go into `DESIGN.md` so the next dashboard starts from what was learned.

Several references produce one reconciled profile. Conflicting references get a named decision, never an average.

## Figma: the gate

Read `references/figma.md` before anything else — it carries the traversal, build and asset rules. Then the gate: check the catalog for Figma MCP tools, allowing one bounded attempt for a lazily attached server. If they are genuinely absent, say what to add — the Figma MCP server in the client's configuration — and **end the turn**. No scaffolding, no approximation from the screenshot, no partial work. The only acceptable output is the configuration instruction.

This is stricter than the browser path on purpose: a missing browser limits verification, a missing Figma MCP removes the source. The two do not get the same fallback.

Two surfaces, two discoveries. The MCP reads the design; the browser verifies the render. Separate probings, separate capability maps.


## HEARTBEAT.md — the learning loop

Capture is automatic. Promotion is deliberate. A contract that rewrites itself silently rots within a week.

```
# HEARTBEAT

## Status
session: <date> | last pulse: <date> | pending: <n> | promoted this session: <n>

## Pending
| # | Date | Type | Rule | Target | Evidence |
| 1 | ... | correction | KPI gutters are 8px, not 12 | DESIGN.md section 5 | user said tighter |

## Promotion policy
- Promote when three pending share a target, when the user says heartbeat,
  or when a task closes with explicit approval.
- Never promote without showing the diff. Keep the evidence line.
- A contradicting rule supersedes the old one; it is not appended.
```

**Capture rules.** Corrections are always captured — highest signal, no confirmation needed. Confirmations only when explicit: perfect, ship it, yes that is it — never inferred from silence. Decisions with a stated reason become candidates too. An approved visual delta counts as a confirmation: record the case, the stop and the viewport. Five signals, and the spoken one is the minority: an explicit correction, a silent edit to a value the agent wrote, a repeated nudge (record where it landed, not the steps), a revert or a rejected variant, a confirmed non-obvious choice. Filter before writing: would a fresh agent make this mistake on a different component? If a violation can be matched in code, prefer a check — a lint rule, a banned token, a test — because a check runs every session and a sentence in a long file gets skipped. Never capture transient task state, secrets, or anything about installed skills.

**Promotion** writes only inside the `cykeek:begin` and `cykeek:end` markers in `AGENTS.md` and `DESIGN.md`, plus a dated record in `.cykeek/decisions/`. Everything outside the markers belongs to the user.

**Pruning** keeps the log at twenty entries and archives the overflow into `.cykeek/decisions/`.

## Budgets

| Read | When | Cap |
| --- | --- | --- |
| `AGENTS.md` | every session, first | 240 lines |
| `HEARTBEAT.md` status and pending | every session, second | 40 lines |
| `DESIGN.md` | only for UI, visual or motion work | 300 lines |
| `.cykeek/skills.md` | only when a route is ambiguous | 320 lines |
| `.cykeek/components/<name>.md` | only for the component in hand | 60 lines |
| `.cykeek/verify/<task>.md` | while a verification is open | 40 lines |
| `.cykeek/research/<slug>.md` | while an unknown technology is being integrated | 25 lines |
| `.cykeek/verify/capability.md` | once per session, written by Preflight | 16 lines |
| rules card, top of `AGENTS.md` | before any act that changes state | 12 lines |
| code gate, `references/craft.md` § The gate | before the first line of code | 5 answers |
| lean ladder, `references/craft.md` § The ladder | before a fix that is not one line | the rung it stopped on |
| design gate, `references/design.md` § The gate | before the first visual decision | 5 answers |
| design card, top of `DESIGN.md` | before any visual act | 12 lines + version |
| scroll score, `references/scroll.md` § The score | before the first scroll act | 5 answers |
| fidelity gate, `references/motion-clone.md` § The fidelity gate | before cloning a shared reference | 3 answers, a spec row per property |
| android gate, `references/android.md` § The gate | before any Android act | 3 answers |
| security gate, `references/security.md` § The gate | before an audit or a trust-boundary claim | 5 answers, one ledger unit per surface |
| show block, `references/pipeline.md` § The show block | at every stop | 5 lines, one question |
| git gate, `references/git.md` § The gate | before any git write | 5 answers |
| `references/<domain>.md` | only when a routing row names it | one per task, ≤140 lines |
| installed skills | only when the reference hands off | at most one at once |

If a generated file exceeds its cap, tailoring failed — cut rows, do not cut rules.

## Common sense, written down

- Infer from the repository before asking a human.
- Prefer a stated assumption to a blocking question — except in the Phase 1 interview, where a wrong answer changes the whole contract.
- Never ask the same fact twice across sessions; check `HEARTBEAT.md` first.
- When the repo and the answers conflict, the repo wins and the conflict is reported in one line.
- Never let setup become the task. If a feature was requested, set up the minimum that makes it buildable, say so, and start building.
## Before you stop

Seven questions. A "no" is not a note to make later.

- Every act carried a receipt, or the session says which rule it read before acting.
- Every claim of verified names a viewport and what was actually seen there.
- Every V case reads PASS, or the deviation is recorded and approved with a date.
- Nothing was written outside the `cykeek:begin` / `cykeek:end` markers, and nothing was installed.
- `HEARTBEAT.md` holds this session's corrections, or says there were none.
- Every worker was released at close, or is named as still running with the reason.
- The next session can start from the files on disk, without asking me what happened.

If the workspace already matches what would be written, write nothing and say so.