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

Runs on every invocation, in both parts, before a question is asked and before a file is written. Six checks, one pass, in this order. The first hard failure ends the turn.

| # | Check | Evidence | If missing |
| --- | --- | --- | --- |
| 1 | Reasoning | the model id, plus whatever the harness reports about a reasoning or thinking mode | **STOP** |
| 2 | Image input | one bounded probe: load a small image and read it back. The tool existing proves nothing about the model | **STOP** |
| 3 | Render path | catalog scan for preview, navigate or screenshot, then one smoke call. A dev server the tool cannot reach is not a render path | **STOP** for a visual class, ask otherwise |
| 4 | Subagents | one bounded proof: spawn a child with a one-line task, read its status, cancel it. The shape is any tool that does those three; the tool existing proves nothing | **STOP** — this workflow splits work across agents, and a harness that cannot spawn one runs a different, weaker pipeline. Ask for a harness that can, or the explicit sentence `run it serially` |
| 5 | The rest of the surface | research (search, fetch, image), MCP servers, the shell | ask once, in one message |
| 6 | Write path | first the line `AGENTS.md` already carries; else the edit tool by shape (`write\|edit\|create\|patch\|str_replace`), else the OS primitive for this shell, proved by one scratch write and a read-back | **STOP** — no file can be written |

The OS costs no call: the shell tool and the shape of the cwd answer it. The scratch probe runs only when the catalog has no edit tool; otherwise the first real write is the probe. A recorded path whose OS no longer matches is stale — re-resolve it and replace the line rather than arguing with it. **The model gates are properties of the model, not the session.** The contract assumes a model that reasons over its own evidence and can see a screenshot; without both, the workspace produced is one nobody can trust. The third stop is plainer: a session that cannot write a file cannot write one.

**STOP** writes nothing and ends the turn:

```
Cykeek will not run in this session: <what is missing>.

- Model: <id, or unknown> — <reasoning not reported | the image probe returned no image>.
- Why it blocks: <the reference and every V case are read at 1440 and 390 | the contract is only as strong as the model holding the goal>.
- Fix: switch to a model whose card lists reasoning and image input, then re-run. Nothing has been changed.
```

The harness stop is flatter — one sentence, and it is the whole turn: **This session cannot spawn subagents. Switch to a harness that can, or say `run it serially` and the same eight gates run with one agent.** The serial path is a recorded deviation in `.cykeek/verify/capability.md` and `HEARTBEAT.md`, never an inference: silence ends the turn, and no agent may grant it on the user's behalf.

Name traits, never brands. A model list goes stale; a capability does not. **Ask** is one message for every gap at once, then wait:

```
Cykeek can run here, with one limit:
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
| `**/components/`, `**/ui/` | where components live, and the families the index is seeded with |
| `app/globals.css`, `styles/*.css` | where tokens live, and the file:line every extracted value cites |
| `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md`, `CLAUDE.md`, `.cursor/rules` | existing contract to merge |
| test runner config | vitest, jest or playwright |
| `git status` | greenfield or live project |
| `app.json`, `expo` dependency | React Native or Expo |
| browser or preview tools in the catalog | which of navigate, observe, interact, capture exist — see `references/process.md` § Discovery and the write path |

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

Print at most ten lines — files, line counts, active route count, reference profile, the `Unknown` lines as assumptions — then write; do not narrate the content. The three file contracts live in `## What AGENTS.md contains`, `## What DESIGN.md contains` and `## HEARTBEAT.md — the learning loop`; read in that order; the pointer and the manifest beside them are `references/process.md` § Setup.

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

Re-invocation is a refresh, not a rebuild: diff the disk against `.cykeek/manifest.md`, report the rows that moved, ask only about the changed facts, and write only the stale rows. A second run with nothing changed produces no diff.

### Phase 5.5 — Prove the workspace

Run the eight checks at `references/process.md` § Setup, one line each — verdict, measured value — and write the table to `.cykeek/verify/workspace.md`. A red row stops before the handoff, with the row named.

### Phase 6 — Hand off in three lines

Workspace ready, N files written, the check table with its verdicts, the one route the next task should take, and the single next action. Nothing else.

## Part 2 — Work in a workspace

Per task, eight gates in order. The sequence, the receipt, the show block and where a gate sends the work back are `references/process.md`; the split is in `references/agents.md`. **Before gate 1, the receipt.** Nothing is written, run or dispatched until one line exists: `Rules: AGENTS.md v<n> · <section cited> · owns: <concept> · refuse: <what> · proof: <check>` (`references/process.md` § Receipt). The card is the top of `AGENTS.md`; the section is cited rather than paraphrased, because a citation you cannot make is a read you did not do. One receipt covers a run of work on one area, refreshed when the area or the goal changes and after any fail, any correction and any returned report. When the file on disk differs from what the receipt cites, the receipt is stale and the read comes first.

1. **Orient** — `AGENTS.md` read, `HEARTBEAT.md` pending, `.cykeek/state.md` read when it exists, preflight passed, the goal restated in one line: the want and the why, or the one question that answers it. The harness verdict rides with it.
2. **Route** — one hub open at its index, only the named sections read, the mode named in one word. A problem statement classifies through `references/process.md` § Triage; a technology the agent cannot specify goes through `references/agents.md` § Research card first.
3. **Brief** — five lines: what, why, the shape, what it refuses, what proves it. A one-round change skips it and stops once, with the result.
4. **Contract** — the numbers, states and widths, and the row table when the work splits (`references/agents.md` § Contract): every row carries the assertion that fails if it is wrong, and a row that says `none` carries its reason. Nothing downstream adds a row.
5. **Slice** — one row built and verified in this round, one owner per row; one consolidated outcome per round, a delta returned as an order and nothing else; the project's tokens and components, nothing outside the markers.
6. **Prove** — the code gate first (`references/code.md` § Gate), then the four-move loop (`references/process.md` § Inner loops): the assertion re-run, read and quoted at two widths (`## Visual tests`), the simplest and riskiest cases seen yourself.
7. **Show** — the five-line block at `references/process.md` § Stops, one question, then wait. A gate that fails returns to the gate it names, never to the top; the mechanical clean pass runs before the question, never after.
8. **Close** — the diff sweep, the workers released, the ledger closed, one local commit and never a push (`references/agents.md` § Git).

A visible artifact that ends without a visual test having run is not finished, and nothing is released to the user as done before they have seen it. The one exception is a capability the session does not have, and that is recorded as UNVERIFIED rather than smoothed over. Every skill runs the same recursive loop inside Prove: **Act → Changes → Review → Loop** (`references/process.md` § Inner loops). Act builds the smallest slice; Changes names one gap in one line — which property, which instant, which width; Review re-renders the same assertion and records PASS or the measured delta; Loop returns to Act with the named fix, or stops after three rounds with no movement and goes back to Contract. No domain file repeats this. WebGL uses it for three `?t=N` frames, GSAP for timeline positions, scroll for three positions per act. A round with no named gap is done — do not improve on taste.

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

Twelve files ship beside this one, each a hub that opens with an index: read the sections the routing row names, not the file, and a hub past its cap is two hubs. They hold the operative content — numbers, thresholds, decision order, stop conditions — distilled from the design, motion, engineering and architecture practice this skill encodes. They are why a routing row never dies when a machine lacks a skill.

| File | Read it for |
| --- | --- |
| `references/process.md` | every task — the eight gates, the receipt, discovery and the write path, the inner loops, triage, dispatch, stops, resume |
| `references/design.md` | the design gate, then UX and hierarchy, direction, tokens, copy, responsive; the system — its source of truth, one change, components |
| `references/ui-traps.md` | the trap catalogue — 95 behaviour traps in four buckets, each cited by ID |
| `references/motion.md` | whether to animate, curves, choreography, gestures, the motion audit, cloning a reference |
| `references/motion-stack.md` | GSAP mechanics, ScrollTrigger, and a scroll-driven page — the score, the shapes, the devices, the timeline, the diagnosis |
| `references/graphics.md` | WebGL and shaders, SVG and Lottie, glass, ASCII, page transitions |
| `references/code.md` | the code gate, the ladder, the moments, changing code, debugging, React, release |
| `references/review.md` | reviewing a change, a design review against the thirteen criteria, auditing a repo, the `shortcut:` ledger |
| `references/security.md` | a security audit or question — the gate, the verdicts, the coverage ledger, the classes by surface |
| `references/agents.md` | splitting across agents, the research card, git — the gate, the message, the attribution scrub |
| `references/platform.md` | Android, Figma and component libraries |
| `references/domain.md` | domain modelling and data systems |

## The routing table

Rows are keyed by the artifact being produced, not by a skill. Read the file in the second column. A skill name in the third column is depth when it happens to be installed, and never a precondition — if it is absent, the reference is the whole contract and the row still works. **Precedence when guidance conflicts:** project conventions in `AGENTS.md` and `DESIGN.md` first, then a live skill on its own subject, then the reference, and the non-negotiables as a floor nothing crosses without a recorded convention.

| Task | Read | Then, if installed | Never |
| --- | --- | --- | --- |
| Any task, before the rest | `references/process.md` § The eight gates, § Receipt | — | starting a domain file instead |
| "What changed?" or "show me" | `references/process.md` § Stops | — | a summary with no file and no proof |
| A gate failed, or the user says it is wrong | `references/process.md` § Stops, what sends it back | — | patching until it looks better |
| "I don't know what it should look like" | `references/process.md` § Triage, Prototype; then `design.md` | `prototype`, `frontend-design` | committing to the first idea |
| "This codebase is a mess", legacy or unfamiliar code | `references/process.md` § Triage, Untangle | `untangle-codebase`, `working-effectively-with-legacy-code` | page-by-page tidying |
| "You keep getting this wrong" | `references/process.md` § Triage, Capture | `learn-taste` | fixing the code instead of the rule |
| Any screen, before the first visual decision | `references/design.md` § Gate, then the row for its kind | a design skill that names its own gate | drawing the layout before answering it |
| New marketing page or site | `references/design.md`, then `references/motion.md` | `design-taste-frontend` | `unslop-ui` in the same turn |
| Dashboard or app screen | `references/design.md` § UX | `frontend-ui-engineering` | `frontend-design` |
| Closing pass on copy and charts | `references/design.md` § Copy | `unslop-ui` | `design-taste-frontend` |
| Design review of a screenshot or URL | `references/review.md` § Design review | `checklist-design` | `web-design-guidelines` |
| Polish one component interaction, or a code-level UI and accessibility audit | `references/design.md` § UX | `emil-design-eng`, `web-design-guidelines` | `checklist-design` |
| Web app on a phone | `references/design.md` § Responsive | `mobile-native` | `animate-expo` |
| Worst-case data and edge cases | `references/design.md` § UX | `break-ui` | `checklist-design` |
| A UI pattern that must not ship, or a finding that needs its ID | `references/ui-traps.md`, cited from `references/review.md` § Design review | `break-ui` | a trap named without its ID |
| A design system to build, migrate or change | `references/design.md` § System, § One change, § Components | `shadcn` | a starter system the product does not call for |
| One animation on the web | `references/motion.md` § Whether, § Curves | `animate` | `animate-expo` |
| GSAP is the project's library | `references/motion.md` § Curves, `references/motion-stack.md` § GSAP | `gsap-core`, `gsap-timeline` | a GSAP answer before the gate, or `.delay()` chains |
| GSAP in React or Next | `references/motion-stack.md` § GSAP | `gsap-react` | an unscoped selector, or a tween without `ctx.revert()` |
| Scroll reveals across a set of sections | `references/motion.md` § Curves, § Choreography; then `references/agents.md` § Contract | `find-animation-opportunities` | animating every section by default |
| A scroll-driven page or sequence, asked for by name | `references/motion-stack.md` § The scroll score | — | a section plan before the score |
| GSAP ScrollTrigger on that page | `references/motion-stack.md` § ScrollTrigger | `gsap-scrolltrigger` | a trigger on a timeline child, or `scrub` with `toggleActions` |
| A video, gif, still or link the user wants matched | `references/motion.md` § Clone | `reanimate` | cloning from a still, or one pass called a match |
| Several things moving at once | `references/motion.md` § Choreography | `choreograph` | `animate` alone |
| Drag, swipe, sheet, drawer | `references/motion.md` § Gestures | `gestures`, `apple-design` | both at once |
| Micro-interactions, and their reduced-motion tier | `references/motion.md` § Curves, § Gestures | `micro-interaction`, `accessible-animation` | animating width/height instead of scale, or an all-or-nothing kill switch |
| Route, page or shared-element transitions | `references/motion.md` | `vercel-react-view-transitions` | hand-rolled `AnimatePresence` routing |
| One animation in React Native | outside this skill's scope — say so | `animate-expo` | `animate` |
| Motion drops frames | `references/motion.md` § Audit | `animation-performance` | `review-animations` |
| Audit motion in one diff or across a codebase | `references/motion.md` § Audit | `review-animations`, `improve-animations` | `animate` |
| Find where motion should exist, too much motion, or an effect to name | `references/motion.md` § Audit | `find-animation-opportunities`, `remove-animations`, `animation-vocabulary` | `improve-animations` |
| WebGL, GLSL, Three.js, R3F, GLTF, particles, flow field or constellation | `references/motion.md` § Whether, `references/graphics.md` § WebGL | `shader-glsl`, `threejs-animation`, `particle-system` | GPU before the gate, or two skills in one turn |
| GLSL fragment shader or GPU transition | `references/graphics.md` § Shaders | `shader-glsl` | hand-rolled noise for a full-screen effect |
| Three.js scene, camera, GLTF blend, instancing, R3F | `references/graphics.md` § WebGL | `threejs-animation` | `scene.remove()` as cleanup |
| Particle, confetti, snow, flow field, connected dots, GPU `Points` | `references/graphics.md` § WebGL | `particle-system` | O(n²) pair checks past ~300 particles |
| SVG draw-on, path morph, animated icon, Lottie or dotLottie | `references/graphics.md` § SVG and Lottie | `svg-animation`, `lottie-animation` | raw `d` morph with mismatched point counts, or re-exporting per brand |
| Frosted glass, an ASCII field, or a Next page transition | `references/graphics.md` § Glass, § ASCII, § Page transitions | `glassmorphism`, `ascii-animation`, `page-transition-animation` | backdrop-filter over a flat background, square-pixel sampling without cell-aspect correction, or `layout.tsx` for per-route enter |
| Any code change, before the first line | `references/code.md` § Gate | `clean-code`, `code-complete` | narrating the gate instead of answering it |
| A fix growing past its problem, or "this is over-engineered" | `references/code.md` § Ladder | `ponytail` | planning the bigger version first |
| A smell, a cleanup, a rewrite tempting | `references/code.md` § Changing code | `refactoring`, `refactoring-guru` | rewriting |
| A bug, a failure, a wrong result | `references/code.md` § Debug | — | a fix before the cause is named |
| Boundary, layering, module or API shape | `references/code.md` § Moments | `a-philosophy-of-software-design`, `clean-architecture` | pattern-first |
| React diagnostics before commit | `references/code.md` § React | `react-doctor` | prototype delivery |
| Production failure handling, or tests that guard nothing | `references/code.md` § Release | `release-it`, `prune-tests` | prototype delivery |
| Review a diff, a commit or a PR | `references/review.md` § A change | `ponytail-review` | a finding with no concrete case |
| Answering a code review | `references/review.md` § Receiving | — | agreeing before checking the code |
| Audit a whole codebase, or find what to delete | `references/review.md` § A repo | `ponytail-audit` | file-by-file reading with no ranking |
| Where we deferred something, and the trigger to revisit | `references/review.md` § The ledger | `ponytail-debt` | treating a named limit as a bug |
| A security audit, pen test, "find vulnerabilities", or report files requested | `references/security.md` § Gate, then § Phases; classes from § Classes | — | a finding with no principal, control or crossed boundary |
| A security question, a triage, or a change to auth, sessions, permissions, uploads, payments or secrets | `references/security.md` § Verdicts, then § Classes | — | a severity on an unproven claim, or probing a live system |
| An Android app or module — `settings.gradle`, `AndroidManifest.xml`, `gradlew` present, or the user asks for one | `references/platform.md` § Android | — | Android advice in a web project, or an API recalled from memory |
| shadcn components, or choosing a UI library | `references/platform.md` § Libraries | `shadcn`, `pick-ui-library` | loading both |
| Project uses `@ui-sdk/*`, or toasts | `references/platform.md` § Libraries | `ui-sdk`, `ask-sonner` | — |
| Figma to code | `references/platform.md` § Figma | `figma-design-to-code` | `figma-use` |
| Write into Figma | outside this skill's scope — say so | `figma-use` | `figma-design-to-code` |
| Work that splits across agents | `references/agents.md` § Contract, after the domain row | subagent tools, if present | two workers writing one file |
| Integrate a library or technique you do not know | `references/agents.md` § Research card, before the domain row | `find-skills` | writing the API from memory |
| "How do other sites do X?" | `references/agents.md` § Research card | — | copying the look instead of the structure |
| Any commit, amend or push | `references/agents.md` § Git | — | a commit nobody asked for, a push nobody asked for |
| Domain rules, services or data flows | `references/domain.md` | the four DDD skills, `patterns-of-enterprise-application-architecture`, `designing-data-intensive-applications` | any UI task |
| A skill I do not have | the reference above it still governs | `find-skills` | — |

## Visual tests

A build that compiles and a suite that passes prove the code runs. They prove nothing about what it looks like. So every visual target is a test case in the same suite as the code tests, and it blocks completion exactly the way a red unit test does.

| ID | Stop | Viewport | Assertion | Status |
| --- | --- | --- | --- | --- |
| V1 | `/pricing` | 1440 | three tiers, 24px gutters, equal card heights | PASS |
| V2 | `/pricing` | 390 | tiers stack, no horizontal scroll | FAIL |
| V3 | hero CTA hover | 1440 | hover fill, press feedback, no layout shift | UNVERIFIED |

On Android the viewport column is the window class and its size, and the render path is an emulator, a device or Robolectric — `platform.md` § Android.

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

**Design.** A screen is designed for a job before it looks like anything: who it is for, the one action, the order of importance, every state — the five answers at `references/design.md` § Gate, and the design card at the top of `DESIGN.md`. No eyebrows, kickers, `01/02/03` numbering, decorative badges, arrows or edge stripes, and no filler subtitle under a heading. One accent colour, never default violet. At most two typefaces, size-specific tracking, body 16px or larger, measure 60 to 75 characters. Spacing from a 4/8pt token scale; equal optical edges. Hierarchy from size, weight, space and contrast — not from boxes and shadows. Real content and real product UI, never lorem.

**Motion.** `--ease-out: cubic-bezier(0.23,1,0.32,1)`, `--ease-in-out: cubic-bezier(0.77,0,0.175,1)`, `--ease-drawer: cubic-bezier(0.32,0.72,0,1)`. Never `ease-in` on UI, never built-in `ease-out`. Press 100 to 160ms, tooltip 125 to 200, dropdown 150 to 250, modal, drawer, sheet and toast 200 to 500, everything else under 300ms. `transform` and `opacity` only. Never `scale(0)`. Never `transition: all`. Anything seen 100+ times a day, and anything keyboard-triggered, gets no animation. Stagger 30 to 80ms, non-uniform, never on high-frequency UI. Exits mirror entrances and are shorter. Reduced motion is fewer and gentler, not zero. A scroll-driven page answers `references/motion-stack.md` § The scroll score before its first section — four to seven beats, one peak, one shape, four device families and never the same one twice in a row — and proves itself with frames at scroll positions, never one screenshot.

**Code.** Server components by default, `use client` at the leaf. No state updates inside rAF, pointer or scroll handlers. No `useEffect` for derived state. Stable keys. Explicit media dimensions. Virtualize or paginate past 100 rows. Semantic, labelled, keyboard-reachable, focus-visible, contrast 4.5:1 or better, touch targets 44 by 44. `min-width: 0` on flex text columns, `overflow-wrap: anywhere` on emails and URLs, `tabular-nums` on changing numbers, `Intl` formatting for numbers and plurals. Never `dangerouslySetInnerHTML` with user data. A task is not done until it has been rendered and compared at 1440 and 390 — passing tests is the prerequisite, not the conclusion. Before the first line: the five answers at `references/code.md` § Gate, and the receipt at `references/process.md` § Receipt. Commits stay local until the user asks for a push: one per logical change, a follow-up edit amends it, the message says why, and no agent signs it.

## What `AGENTS.md` contains

Fourteen sections, in this order. Cap 240 lines.

0. **Rules card** — twelve lines, first, before anything else: the precedence order; the receipt line and when one is required; the two gates — code before the first line, design before the first visual decision; the never-list; the five events that force a full re-read. It is what an agent reads before it acts, so it survives a long session and a compaction that the rest of the file does not.
1. **Header** — what the file is, precedence of project over user defaults, how to regenerate.
2. **Goal and facts** — the goal sentence, who it is for, what done means, what this is not, then stack and versions, package manager, scripts, directories, UI, styling and motion libraries, delivery mode, the contract version, and the manifest at `.cykeek/manifest.md`.
3. **Session start** — four files, in this order, then act: this file (240), `HEARTBEAT.md` status and pending (40), `.cykeek/state.md` (10), `.cykeek/verify/capability.md` (16). `DESIGN.md` only when the task touches UI, visual or motion. The preflight verdict lives in `.cykeek/verify/capability.md`; the OS and the proven write path are written at the end of this section and are used before any probe. Restate the goal in one line before starting, so the session is aimed before it is busy.
4. **Working loop** — the eight gates in order (`references/process.md` § The eight gates): orient (goal, routing row, harness verdict), route (one hub at its index, the mode), brief (five lines), contract (the row table, every row falsifiable), slice (one row, one owner), prove (the assertion re-run and quoted), show (the five-line block, one question), close (the sweep, the ledger, one local commit). One line per gate; a gate that fails returns to the gate `references/process.md` names, and the agent waits at a stop with the show block. Compress for small changes; proof is never the gate that gets skipped.
5. **Routing table** — the active subset, three columns: the task, the `references/` file to open, and what never to do here. One reference per task, opened when its row is reached; a matching skill may be opened afterwards for depth, and its absence changes nothing.
6. **Ordering rules** — the four or five sequences that matter for this project class. For a Figma project the binding one is gate, discover, screenshot, traverse, batch, build, compare.
7. **Non-negotiables** — the twenty lines, and in the code and design ones the gates and the receipt, with `references/code.md` § Gate and `references/design.md` § Gate as the long forms.
8. **Gates and definition of done** — per task class.
9. **Never** — the automatic blocks: an unasked commit or push, an agent trailer, an edit outside the markers, an install, an unverified ship.
10. **Heartbeat** — when to capture, when to promote, which file to edit.
11. **Verification** — the preflight verdict (model, reasoning, image, render, then the discovered names), the state pointer and the manifest as evidence of what was generated, the V-case schema, the gate, the two exit conditions, the ledger path, the commit read back — subject, why, no trailer, still local — and for a cloned reference the spec rows, the timing numbers and the sheets.
12. **Orchestration** — the contract table, whose rows are the work orders and the tests at once; the seven states from `PLANNED` to `SHIPPED`; the ledger at `.cykeek/verify/<task>.md`; the caps (40-line contract, 12-line assignment, 8-line outcome, 3 rows per batch, 4 rounds); the three stops; the rule that whoever builds never grades; keep-alive until the user approves; only the orchestrator commits, and only locally; and the serial path only on the explicit sentence `run it serially`, recorded as a deviation.
13. **Research** — when a request names a technology the agent cannot state from memory: the card at `.cykeek/research/<slug>.md` with its nine slots, the source ladder, the five-query cap, the spike before integration, and the rule that no dependency enters without a version, a licence and a cleanup path.

Wrap the generated region in `<!-- cykeek:begin -->` and `<!-- cykeek:end -->`.

## What `DESIGN.md` contains

A system, not a mood board. Fifteen sections, cap 300 lines.

| # | Section | Content |
| --- | --- | --- |
| 0 | Design card | twelve lines at the top, the template at `references/design.md` § Card: kind, what is sold, one action, the one thing, palette and type rules, refusals, widths, states, review mode, and the `Design card v1 · <date>` line every return cites |
| 1 | Principles | six named principles, each with the decision it forces |
| 2 | Reference profiles | one block per client reference: source, extracted ratios, allowed deviations, verification stops. Figma profiles also carry file, frame and node count |
| 3 | Colour | neutral ramp, one accent, semantics, contrast pairings, prohibitions — each value cites its `file:line`, or is marked `invented` |
| 4 | Type | faces, scale with tracking and leading pairs, measure, numerals — extracted, or `invented` with the reason |
| 5 | Space and grid | 4/8pt scale, container widths, columns per breakpoint, optical edges |
| 6 | Elevation and surfaces | radius scale, border versus shadow, one elevation accent |
| 7 | Motion tokens | three curves, duration ladder, spring defaults, frequency gate, stagger budget |
| 8 | Iconography | stroke, size, optical alignment |
| 9 | Density modes | comfortable and compact, for dashboards |
| 10 | Responsive ladder | the rules below |
| 11 | Component contracts | the seeded index at `.cykeek/components/index.md` — one canonical per family, with its path — plus the schema |
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

Read `references/platform.md` § Figma before anything else — it carries the traversal, build and asset rules. Then the gate: check the catalog for Figma MCP tools, allowing one bounded attempt for a lazily attached server. If they are genuinely absent, say what to add — the Figma MCP server in the client's configuration — and **end the turn**. No scaffolding, no approximation from the screenshot, no partial work. The only acceptable output is the configuration instruction.

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
| `.cykeek/state.md` | every session, third, and at every gate transition | 10 lines |
| `.cykeek/manifest.md` | at setup, and on every refresh | 12 lines |
| `DESIGN.md` | only for UI, visual or motion work | 300 lines |
| `.cykeek/skills.md` | only when a route is ambiguous | 320 lines |
| `.cykeek/components/<name>.md` | only for the component in hand | 60 lines |
| `.cykeek/verify/<task>.md` | while a verification is open | 40 lines |
| `.cykeek/research/<slug>.md` | while an unknown technology is being integrated | 25 lines |
| `.cykeek/verify/capability.md` | once per session, written by Preflight | 16 lines |
| rules card, top of `AGENTS.md` | before any act that changes state | 12 lines |
| code gate, `references/code.md` § Gate | before the first line of code | 5 answers |
| lean ladder, `references/code.md` § Ladder | before a fix that is not one line | the rung it stopped on |
| design gate, `references/design.md` § Gate | before the first visual decision | 5 answers |
| design card, top of `DESIGN.md` | before any visual act | 12 lines + version |
| scroll score, `references/motion-stack.md` § The scroll score | before the first scroll act | 5 answers |
| fidelity gate, `references/motion.md` § Clone | before cloning a shared reference | 3 answers, a spec row per property |
| android gate, `references/platform.md` § Android | before any Android act | 3 answers |
| security gate, `references/security.md` § Gate | before an audit or a trust-boundary claim | 5 answers, one ledger unit per surface |
| show block, `references/process.md` § Stops | at every stop | 5 lines, one question |
| git gate, `references/agents.md` § Git | before any git write | 5 answers |
| `references/process.md` | every task | 300 lines |
| `references/design.md` | when its routing row names it | 300 lines |
| `references/code.md` | when its routing row names it | 260 lines |
| `references/motion.md` | when its routing row names it | 300 lines |
| `references/motion-stack.md` | when its routing row names it | 230 lines |
| `references/graphics.md` | when its routing row names it | 190 lines |
| `references/review.md` | when its routing row names it | 120 lines |
| `references/security.md` | when its routing row names it | 260 lines |
| `references/agents.md` | when its routing row names it | 300 lines |
| `references/platform.md` | when its routing row names it | 200 lines |
| `references/domain.md` | when its routing row names it | 60 lines |
| `references/ui-traps.md` | when a review or spec cites a trap | 140 lines |
| installed skills | only when the reference hands off | at most one at once |

If a generated file exceeds its cap, tailoring failed — cut rows, do not cut rules. A hub over its cap is two hubs.

## Common sense, written down

- Infer from the repository before asking a human, and never ask the same fact twice across sessions — check `HEARTBEAT.md` first.
- Prefer a stated assumption to a blocking question — except in the Phase 1 interview, where a wrong answer changes the whole contract; when the repo and the answers conflict, the repo wins and the conflict is reported in one line.
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