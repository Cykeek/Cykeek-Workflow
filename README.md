# Cykeek

A skill that prepares a project so an agent arrives already knowing how to work in it.

## The problem

Every new session starts blind. The agent does not know your stack, does not know your design system, and of the forty-odd skills installed it picks the wrong ones — or loads four at once and follows none of them properly.

So you explain it again. The tokens are spent, the rules drift between sessions, and the same corrections get made in the same order every time.

Cykeek writes those rules down once, in the project, in the shape an agent actually reads: what to do, where to look, when to act, and what done means.

## What it does

Invoking `/cykeek` writes three files into the project.

| File | Job | Read |
| --- | --- | --- |
| `AGENTS.md` | how, where and when to act: routing, order, gates, budgets | every session |
| `DESIGN.md` | the design system: tokens, components, responsive rules, reference cloning | only when the task touches UI or motion |
| `HEARTBEAT.md` | the learning loop: what to capture, when to promote it | status and pending, every session |

Nothing is installed and nothing is overwritten. Existing files are edited only inside `cykeek:begin` and `cykeek:end` markers.

## How it works

Seven phases, in order, with a proof pass before the handoff.

1. **Detect** — reads `package.json`, `tsconfig.json`, Tailwind config, `components.json`, the motion library, the router, the components directory, the token stylesheet, existing contract files and git state. It also checks which optional routes are actually available in this session — Figma MCP tools, `@ui-sdk/*`, which of the four browser capabilities — navigate, observe, interact, capture — the session actually has, and whether it can spawn agents at all — probed before the first dispatch, since setup itself never splits.
2. **Ask properly** — five open questions in one message: what problem it solves and for whom, what done looks like, what exists today, what it should feel like, and what cannot change. The detected facts print above them so nothing is asked twice, and anything the invocation already answered is skipped. A second round covers at most three gaps, and then it stops asking.
3. **Brief** — the answers compress into twelve lines: goal, audience, done, not, unknown, fixed, feel, shape, risky, first. `Shape` names the workstreams in dependency order with the reference row each one uses, which is what makes an agent act on the problem rather than on the request.
4. **Tailor** — the brief selects a subset, not a variant of everything. A prototype marketing page drops the release gates, `react-doctor` and `prune-tests` rows entirely; a production dashboard keeps them and adds density modes, table rules and the chart cleanup pass. Tailored output is materially shorter than generic output.
5. **Write** — creates its own files freely, appends marked blocks to a token stylesheet or Tailwind theme after showing the diff, and never touches dependencies, lockfiles, lint config or existing source, and keeps its own four artifacts local with one marked `.gitignore` block.
6. **Hand off** — the nine checks, then three lines: what was written, which route the next task should take, and the one next action.

After setup, every task runs through eight gates — orient, route, brief, contract, slice, prove, show, close — and the first act opens with one receipt line naming the card version and the section of `AGENTS.md` being followed. A gate that fails returns to the gate the process names, never to the top, and a change that fits in one round skips the brief and stops once, with the result. **Slice** is where the work splits: with agents available the session becomes an orchestrator that writes a contract whose rows are simultaneously the work orders and the tests, takes back one eight-line outcome per round, answers it with one-line orders instead of rebuilds, and holds every worker alive until you have seen the result. The states are fixed, `PLANNED` to `SHIPPED`, and all of it lives in `.cykeek/verify/<task>.md` — which is why a restarted worker resumes instead of being re-briefed. Without subagents the same states run serially, by hand, in the same file. A UI task splits by lens instead of by row: **UX** (the job, the one action, hierarchy order, every state), **Direction** (the references studied, the competitor read, palette and type), **Build** (one section, from the design card and the system) and **Review** (the render at five widths, keyboard, contrast, the deltas named) — three workers at most, and the one who builds never grades.

Re-running it is a refresh, not a rebuild. It diffs the disk against the manifest the first run wrote, reports the rows that went stale, asks only about the changed facts, and produces no diff at all if nothing changed.

## What lands in your project

### `AGENTS.md`

The process contract, capped at 240 lines. It opens with a twelve-line **rules card** — precedence, the receipt, the two gates, the never-list, and the five events that force a full re-read — then thirteen more sections: goal and facts, session-start order, the working loop, the active routing table, ordering rules, twenty non-negotiables, gates and definition of done, an automatic-never list, the heartbeat protocol, the verification rules, how the work splits across agents, and the research protocol.

The non-negotiables are the rules that hold even when no skill is loaded — the ones that decide whether the output looks like a considered interface or like everything else an agent produces on a first attempt.

### `DESIGN.md`

The design system, capped at 300 lines, fifteen sections: a twelve-line design card (kind, what is sold, the one action, the one thing, palette and type rules, refused tells, widths, review mode), then principles, reference profiles, colour, type, space and grid, elevation, motion tokens, iconography, density modes, the responsive ladder, component contracts, composition patterns, content voice, and the acceptance checks run before done.

Components are specified before they are built. Each one gets a sixty-line spec in `.cykeek/components/`: purpose, anatomy, tokens consumed, every state including empty and error, behaviour per breakpoint, motion, accessibility, anti-patterns and acceptance checks. The index at `.cykeek/components/index.md` is seeded at setup from what the repo already ships — one canonical per family, with its path — and every value written into `DESIGN.md` names the file and line it came from, or is marked invented.

The responsive ladder is binding, not advisory: breakpoints 360 through 1440, `clamp()` type, 44 by 44 touch targets, hover motion gated behind `(hover: hover) and (pointer: fine)`, dashboards that collapse their sidebar, regrid KPIs 4 to 2 to 1, and decide per table between card-per-row and horizontal scroll with a sticky first column. It also carries the phone-specific fixes — no sticky hover, `100dvh` instead of `100vh`, inputs at 16px so iOS does not zoom the page.

### `HEARTBEAT.md`

The learning loop, covered below.

### `.cykeek/`

The depth, loaded only when needed: the state pointer a resumed session reads first, the manifest that makes a refresh a diff instead of a guess, component specs, dated decision records, and the expanded per-skill contracts. The folder is local: one marked block in `.gitignore` names it and the three contract files, so none of them enters a commit, and `README.md` plus the project's own docs are never touched.

## The routing table

The core of the skill is a table keyed by the artifact being produced, not by a skill. Ask for a stagger and it reads `references/motion.md`; ask for a design review and it reads `references/review.md` § Design review; ask why an animation stutters and it reads `references/motion.md` § Audit. No round of "what do you mean by a stagger" first.

It also encodes the four ways a large skill library makes an agent worse:

- **The reference is already loaded.** A live skill is the exception, one per turn, and only when the reference hands off to it.
- **Never a reviewer and a builder for the same artifact** in one turn.
- **No cold-loading a skill that prints a canned line and stops.** Several installed skills open with a fixed greeting and wait for a question. Loaded at the wrong moment they stall the session, so they are always invoked with the task stated.
- **Never two overlapping taste skills together.** `unslop-ui` and `design-taste-frontend` do the same job in different vocabularies; picking both produces contradictory instructions.

Domain and enterprise skills — DDD, Fowler, Clean Architecture — are scope-gated to non-UI work, so a landing page never summons them.

## The heartbeat

Capture is automatic, promotion is deliberate. A contract that silently rewrites itself rots within a week, so the two are separated.

- **Corrections are always captured.** You say "tighter" and the rule lands in `HEARTBEAT.md` with the quote as evidence, immediately.
- **Confirmations are captured only when explicit** — "perfect", "ship it", "that is it". Never inferred from silence.
- **Nothing reaches `AGENTS.md` or `DESIGN.md`** until three pending rules share a target, you say "heartbeat", or a task closes with clear approval — and then only after the diff is shown.
- **A contradicting rule supersedes the old one** rather than being appended next to it.

The point is that you explain something once. The next session starts already knowing it.

## Nothing is done until it has been seen

A build that compiles and a suite that passes prove the code runs. They prove nothing about what it looks like — and "tests pass" is the most common way an agent declares a screen finished without ever loading it.

So visual checks are test cases. Each is a stop, a viewport, an assertion and a status, and a suite holding a single FAIL or UNVERIFIED is a failing suite:

| ID | Stop | Viewport | Assertion | Status |
| --- | --- | --- | --- | --- |
| V1 | `/pricing` | 1440 | three tiers, 24px gutters, equal heights | PASS |
| V2 | `/pricing` | 390 | tiers stack, no horizontal scroll | FAIL |
| V3 | hero CTA hover | 1440 | hover fill, press feedback, no shift | UNVERIFIED |

The run continues until one of two things happens: every case reads PASS, or you explicitly accept the remaining deltas, which are then recorded with the date and the reason. Silence is not approval, and a green build is not approval.

If two rounds reduce nothing, it stops and brings you the list — which case, which viewport, what it tried — instead of handing back something it has not looked at.

### When you hand it a video

A gif, a screen recording, a still or a link is an instruction to match, not to riff on. `references/motion.md` § Clone sets the fidelity level from your words — moves the same by default, pixel-perfect only when you ask — reads the frames for start, duration, easing and stagger, writes one row per element per property, rebuilds in the stack the project already uses, then captures the copy the same way and compares: 80 ms on timing, 40 ms on stagger, 2 % mean pixels when you asked for exact. Five rounds without a gain and it stops with the failing row, its numbers and the reason. A still image carries no timing, so from a still alone it asks for the recording.

### When the project is Android

Android work is gated, not assumed: the hub opens on a `settings.gradle`, an `AndroidManifest.xml`, a `gradlew` or an explicit request, and stays shut on a web project that merely mentions the platform. `references/platform.md` § Android names the official Google skill that owns each job — Compose, adaptive, navigation, testing, R8, edge-to-edge, Play — with the decision it forces, the eight floors that hold everywhere, and the fence: no API, version or Gradle flag from memory, and an unread one stays UNVERIFIED. The design widths become window classes, and the proof is a build, a test run and a render on an emulator, a device or Robolectric.

### The smallest complete change

Before a fix is written, `references/code.md` § Ladder asks for the first rung that fully works: does it need to exist, is it already in this codebase, does the standard library or the platform do it, is there an installed dependency, can it be one line, and only then the minimum that works. Reading comes first, and scope is every caller, test and fixture the change reaches — lazy about the solution, never about the change. Validation, error handling, security, accessibility and whatever was asked for are never cut for size. Logic with a branch, a loop, a parser, money or security leaves one small test behind, and a deliberate shortcut is marked `shortcut: <limit>, <when to upgrade>` so it has a trigger. Those markers are the ledger `references/review.md` harvests — the same file that turns a diff or a whole repo into ranked findings, each with a concrete case, a four-part explanation, a verdict and a `Not checked:` line.

### The gates, in order

Every task runs eight gates — orient, route, brief, contract, slice, prove, show, close — and each has a named destination when it fails, so work goes back to the step that owns the problem instead of to the top. A change that fits in one round skips the brief and stops once, with the result. At every stop the agent shows exactly five lines — state, what changed with the file and its lines, the proof as a command and its output, what comes next, and one question — then waits for your answer before touching the next row. Budgets keep the loop finite: three rounds per assertion, two attempts per gate, one question per stop, and a cap is a stop with the blocker named, never a fourth attempt.

### When the question is security

Security is gated too: a one-line question is answered in one line, and a full audit starts only when you ask for an audit, a pen test or report files. `references/security.md` is the protocol — five gate answers, three verdicts (`confirmed`, `needs_validation`, `rejected`), a severity only where impact was demonstrated, a coverage ledger with one unit per surface and class, and a verifier who did not hunt. `references/security.md` § Classes says what to look at by surface, from injection and access control through HTTP framing, browser context, data isolation, supply chain, cloud, agents and MCP. Probing a live system is never the proof: a deployment fact outside the source becomes `needs_validation` with the exact missing observation, and the report states its coverage instead of implying it.
### It does not assume which tools exist

Harnesses differ, and some sessions have no browser at all. Before anything else, it runs a preflight: the model must reason and accept images, or the session is stopped with the fix; then it filters its own tool catalog, reads the contract of the candidates, makes one cheap read-only call, and maps what it finds onto the capabilities: navigate, observe, interact, capture.

The write path is the OS's own primitives, tried in order until one lands — the harness's edit tool, then the shell's, then an interpreter that is already installed — with no install to gain a rung, no retry of a denied path, and the changed lines read back. The rung that works is written back into the project's `AGENTS.md` as one line, so the next session starts there instead of probing again.

A missing capability degrades that dimension and is stated rather than faked — navigate and capture with no click still passes every static case, and marks the interaction cases unverified. The verdict and the mapping are written to `.cykeek/verify/capability.md` and re-confirmed each session, never trusted blindly, because the same project opened in a different harness has a different surface.

### Figma

Figma is the one source that cannot be approximated, so it gets a hard gate: if the Figma MCP is not configured in the session, the skill says what to add and ends the turn. No approximation from the screenshot, no scaffolding, no partial work — the source is missing, so everything downstream would be invention.

Once connected it renders the frame before touching code, walks the node tree recursively until every visible node is covered or explicitly skipped, batches every independent read into a single round, and then compares the build against that render until it *is* the design rather than something that looks close.

## The rules ship with it

`references/` holds twelve hubs, each opening with an index: read the sections the routing row names, not the file, and a hub past its cap is two hubs. `process.md` is the first of them: one sequence, eight gates — orient, route, brief, contract, slice, prove, show, close — each with the artifact it leaves, who signs it, its cap and the gate a failure returns to. It also carries the receipt, the discovery and write-path rungs, the two inner loops (the plan ladder and the four-move verify loop), the triage that reads the problem statement rather than the request — prototype the look, break the durability, untangle the codebase, capture the correction — the inline-or-split decision, the five-line show block with its budget, and the resume rule that makes a break survivable. The other ten carry the operative content per domain: `design.md` the design gate and the look — UX and hierarchy, direction, tokens, copy, the responsive ladder — and the design system itself: the system is the source of truth, one change sweeps every consumer, a ban ships with its check, a component ships with its contract; `code.md` the code gate, the ladder that takes the first rung that works, the triggered moments, changing code, debugging (reproduce, locate, explain, then fix at the cause — a symptom patch is a second bug), React and release; `motion.md` whether to animate and how it should feel — curves, choreography, gestures — plus the ten audit standards and the frame-by-frame clone protocol with its row-by-row proof; `motion-stack.md` GSAP mechanics, ScrollTrigger, and the scroll score with its shapes, devices, timeline contract and diagnosis table; `graphics.md` WebGL and shaders, SVG and Lottie, glass, ASCII and page transitions; `review.md` reviewing a change, a design review, answering a review, auditing a repo and the `shortcut:` ledger; `security.md` the gated security protocol — five gate answers, three verdicts, severity only on demonstrated impact, the coverage ledger — and its class inventory by surface; `agents.md` splitting work across agents, the research card, and git — the gate, the message, the attribution scrub; `platform.md` Android, Figma and component libraries; `domain.md` domain modelling and data systems; `ui-traps.md` the 95 behaviour traps, cited by bucket and ID from any review. `assets/picker.html` is the prototype surface, copied verbatim rather than redesigned.

They are why it never depends on what a machine has installed. A routing row names a reference, not a skill. If the matching skill happens to be present it is loaded for depth; if it is absent, nothing breaks, because the reference was the contract all along.

One hub per task, only the sections the routing row names, at the moment the row is reached. The non-negotiables in `AGENTS.md` cover ordinary work, so most tasks read none.

`agents.md` § Git is the exception to "read when the row is reached": it is opened before any git write. Commits are local, one per logical change; a follow-up edit to the same files amends the last commit and rewrites its message to cover all of it; a push waits for the user's word. The author is the project's identity — an agent never signs its own work, in the author field or in a trailer.

## Install

The repository *is* the skill folder. Clone it into place and it is ready — no installer, no copy step, no build.

All projects:

```bash
git clone https://github.com/Cykeek/Cykeek-Workflow.git ~/.agents/skills/cykeek
```

```powershell
git clone https://github.com/Cykeek/Cykeek-Workflow.git "$HOME\.agents\skills\cykeek"
```

One project only:

```bash
git clone https://github.com/Cykeek/Cykeek-Workflow.git .agents/skills/cykeek
```

Update:

```bash
git -C ~/.agents/skills/cykeek pull
```

Restart the session afterwards. `cykeek` then appears in the skill list and responds to `/cykeek`.

The folder must be named `cykeek`, because the specification requires the directory name to match the `name` field in the frontmatter. Cloning into a differently named folder is the one way to break this.

## Use

```
/cykeek
```

With nothing else, it interviews you:

```
Detected: Next.js 15 · React 19 · TS · Tailwind 4 · shadcn · motion · pnpm · no AGENTS.md

1. What problem does this solve, and for whom?
2. What does done look like — the one screen or flow that must be right?
3. What exists today: nothing, a Figma, a screenshot, a half-built repo, a live URL?
4. What should it feel like — a product you would point at, or three adjectives?
5. What cannot change — stack, brand, deadline, integrations, people?
```

Then it compresses the answers into the brief it will build from:

```
Goal     freelancers cannot tell which invoices are overdue
For      solo freelancers, weekly, between client work
Done     the list sorts by lateness; paid and unpaid filter in one click
Not      a full accounting suite, an export pipeline, a mobile app
Unknown  assumed: auth is out of scope for this pass
Fixed    Next 15, shadcn, two weeks
Feel     linear.app — quiet, dense, one accent
Shape    1. tokens and shell — references/design.md
         2. invoice list and filters — references/design.md § UX
         3. row motion and empty states — references/motion.md
Risky    table density at 1440; the empty and overdue states
First    the list, with twelve seeded invoices
```

Answer in a sentence each, or write `defaults` to take the detected values. Pass detail inline and it skips the questions it already answered:

```
/cykeek a fintech dashboard, screenshot attached
```

Then it works from the generated files for the rest of the project's life. You do not invoke it again for a normal task — the agent reads `AGENTS.md` and `DESIGN.md` and routes itself.

Two mechanisms keep that true as a session gets long. The **rules card** — twelve lines at the top of `AGENTS.md` — is read before any act that changes state, and the act opens with a receipt naming the card version and the section it followed; an act without one is a failure, however good the output. The **code gate** is five answers before the first line of any code change — what owns this, step or leap, what is hidden, what is refused, what proves it — which is what the twelve coding books reduce to once their ceremony is removed. The **design gate** is its twin for visuals, and it opens with behaviour rather than style: the job and the one action, the order of importance, the flow and every state, where the look comes from and which three defaults it refuses, and what proves it — then the design card at the top of `DESIGN.md` holds the answers — twelve lines, its own version — so a later session builds from them instead of re-deriving taste.

## A real run

```
/cykeek a marketing site for a scheduling tool, expressive motion, production
```

It detects an empty directory, so it asks only what it cannot infer. You answer the two it asks. It prints:

```
AGENTS.md      214 lines    18 routes active    delivery: production
DESIGN.md      287 lines    motion: expressive   density: comfortable
HEARTBEAT.md     34 lines    pending: 0
.cykeek/         2 files    skills.md, components/

Assumptions: no reference supplied, so the design system starts neutral.

Next route: references/design.md, then references/motion.md
Start with the hero, then the pricing section.
```

## Guardrails

- Never edits installed skills or agent config: no writes to `~/.agents`, `.claude`, `.cursor` or `.codex`.
- Never touches dependencies, lockfiles, lint or format config, or existing source.
- Never rewrites hand-written content outside its own markers.
- Never invents stack facts it did not detect; a missing config is a fact, not a mystery to solve.
- Never asks the same fact twice across sessions — `HEARTBEAT.md` is checked first.
- Never acts on remembered rules: the card is read, the section is cited, and a receipt that names a section which does not exist is caught at close.

## Requirements

Any client that discovers Agent Skills from `.agents/skills/`. No Node, no build step, no dependencies. The skill is Markdown, twelve reference files, one HTML asset, and a JSON test suite.

Two model traits are not negotiable, because the contract rests on them: reasoning over its own evidence, and image input. Both are checked before a question is asked, and a session missing either is stopped with the fix — switch the model — rather than run degraded.

The rest of the session is confirmed once and written to `.cykeek/verify/capability.md`: a browser or screenshot path for visual work, subagent tools — without them the session stops and asks for a harness that can spawn, or the exact sentence `run it serially`, recorded as a deviation and never inferred — search and image tools for research (without them the repository is the source), MCP servers where a project needs one, and the write path. Every limit is named with its consequence and either accepted or declined before the work starts — silence is not acceptance.

## Layout

```
SKILL.md            the skill itself
references/         twelve hubs, each opened at its named sections:
                    process, design, code, motion, motion-stack, graphics, review, security, agents, platform, domain, ui-traps
assets/picker.html  the prototype picker chrome, copied verbatim
evals/evals.json    ninety-four cases: cold start, tailoring, merge safety, idempotency, preflight,
                    routing, browser probing, Figma, plateau, skill-less machines, write path,
                    the two gates, the design lenses, the card version, the scroll score, the receipt,
                    commit shape, amend, push permission, attribution, frame-by-frame cloning,
                    the Android gate and its official routes, the lean ladder, the review report,
                    the eight gates and the show block, root-cause debugging,
                    the security gate, the three verdicts, the coverage ledger,
                    the harness gate and the serial sentence, resume from the ledger,
                    hub sections and hub caps, the design-system rules and the trap IDs
README.md           this file
```

## Editing

This repository is the source of truth. Edit `SKILL.md`, commit, and pull on each machine. There is no generated copy; the installed folder at `~/.agents/skills/cykeek/` is a byte-for-byte mirror, refreshed after every change.