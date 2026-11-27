# Process — one sequence, eight gates

**Index** — `§ The eight gates` (the spine) · `§ Receipt` (before any act) · `§ Setup` (the pointer, the manifest, the proof) · `§ Discovery and the write path` (what this session can do) · `§ Inner loops` (the plan ladder, the spec, the four-move loop) · `§ Triage` (a problem statement) · `§ Dispatch and build` (inline or split) · `§ Stops` (the show block, the budget, the facts) · `§ Resume and close` (after a break). Read the sections the routing row names, not the file.

This file is the process; the domain files carry the content. Every gate leaves an artifact — no artifact, no next gate. The detail lives where it belongs: the code gate and the ladder in `code.md`, the row table and the states in `agents.md`, findings in `review.md`, git in `agents.md` § Git, a technology not in memory in `agents.md` § Research card, a bug before any fix in `code.md` § Debug. This file never repeats them.

## The eight gates

| # | Gate | Enters with | Artifact | Signed by | Passes when | Fails to | Cap |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Orient | — | one line: goal, routing row, harness verdict | agent | `AGENTS.md` read, `HEARTBEAT.md` pending read, preflight passed | STOP | 3 lines |
| 2 | Route | the goal | one hub, the named sections, the mode | agent | the hub opened at its index, the mode named in one word | Orient | 1 hub |
| 3 | Brief | the route | five lines | agent | what, why, the shape, what it refuses, what proves it | Route | 5 lines |
| 4 | Contract | the brief | the row table | orchestrator | every row has an assertion, or a `none` with its reason | Brief | 40 lines |
| 5 | Slice | the contract | one row built | one writer | the row's assertion can run; nothing outside it touched | Contract | 3 rows |
| 6 | Prove | the slice | the measured result | anyone who did not write it | assertion re-run, two widths, a number quoted, PASS or the delta | Slice | 3 rounds |
| 7 | Show | the proof | the five-line block and one question | the user | the user answered | Prove | 5 lines |
| 8 | Close | the approval | sweep, releases, ledger, one local commit | orchestrator | receipt cited, counts checked, mirror synced | never | 3 lines |

One vocabulary, and only one: the old six moves are these gates — Frame is Orient, Decide is Route, Specify is Contract, Build is Slice — and the two loops in `§ Inner loops` are the only other names in use. A gate that fails goes to the gate its row names, never to the top, and its entry condition is the one next action, so a stuck agent has one move, not a menu. Ask once, then restate the request with the assumption named: a wrong assumption is corrected in one line, a questionnaire is not answered at all.

**The one-stop rule.** A change that fits in one round skips the Brief and stops once, with the result. New behaviour or work that spans rounds stops twice: the brief, then the result. Inside an approved brief the rows run without asking — the agent pauses per batch and per gate failure, not per row. A brief question on a save button costs a turn and changes nothing.

**The mode** (gate 2, one word) is the flow `§ Triage` names, or `build` for a plain artifact request; system work answers use, build, move or audit instead.

Reading order is fixed: metadata → routing row → one hub at its named sections → the regions those name. Never the reverse, never a folder listing to find what you already know the name of. Narrate decisions, not steps; let code and diffs carry the detail. Stop when the gates are done — length is not thoroughness, and extra sections are extra surface to be wrong on.

## Receipt

Not a gate — a condition on starting one. An **act** is the session's first call, any call that changes state (write, delete, move, install, build, run, deploy, dispatch), a new task inside the session, or resuming after compaction, a returned report or a failure. Reading, searching and planning are not acts, so exploring stays free.

Before an act, one line:

`Rules: AGENTS.md v<n> · <section cited> · owns: <concept> · refuse: <what> · proof: <check>`

Cite the section, do not paraphrase it: a citation naming no section is a table of contents, and a receipt naming one that does not exist is a fail at close. One receipt covers a contiguous run on one area — forty edits to one component are one act, not forty. Refresh it when the area, the file or the goal changes, and after every fail, correction or returned report.

Re-read the whole of `AGENTS.md` — the file, not the card — on the first call of a session, after compaction, when a report returns, after any fail, and whenever a rule you are relying on feels unfamiliar. The card survives a long session; the file is the truth and may have changed since it was read. When the file on disk differs from what the receipt cites, the receipt is stale and the act waits for the read.

## Discovery and the write path

Tool names in this skill are examples of a shape, never a claim of availability. Harnesses differ, some ship no browser at all, and an instruction that names a tool the session does not have fails silently — the agent reports done having verified nothing. So every session resolves its own surface first:

1. **Filter the catalog already in context.** It is free. Look for `navigate|goto|url|open`, `snapshot|screenshot|dom|aria|inspect`, `click|type|press|scroll|hover`, `evaluate|script|console|network`, `playwright|puppeteer|chrome|devtools|browser`.
2. **Read the contract of two or three candidates.** Required arguments, return shape, and any precondition the description states. Do not guess an argument shape.
3. **Make one smoke call** — the cheapest read-only operation the schema allows: a status, a list, a current page. Never open with a click.
4. **Map what exists onto the preflight checks.** Anything unmapped is a gap the user hears about before the work starts.

**Catalog patterns.** Orchestration: `delegate|subagent|task|agent|spawn`, with spawn, status and release all present. Research: `search|fetch|browse` plus `image|screenshot`. Never assume a tool because a skill names one, never fake a roster, and treat a tool missing from the catalog as unproven rather than absent: lazily attached servers get one bounded attempt with the known name. With no search tool, every outside claim is marked as such.

A partial surface is normal and usable: navigate plus capture with no interact still passes every static case and marks interaction unverified. A model without reasoning or vision is the one surface that is not usable — that is the preflight stop.

**Token discipline.** Filter before calling. One smoke call total, not one per candidate. Snapshot text before screenshots — an image costs far more than lines, and only geometry or colour justifies one. Scope a snapshot to a region when the tool allows it. Write the mapping down once per session; re-deriving it every turn is the expensive failure.

**Write path.** Preflight records which rung writes here. Use it in order; stop at the first that succeeds.

| OS | Rungs |
| --- | --- |
| Windows | the harness edit tool → `Set-Content -LiteralPath` with a single-quoted here-string → `[IO.File]::WriteAllText` (FullLanguage only; a read-only sandbox runs ConstrainedLanguage and rejects it) → `node -e` or `python -c` if already installed → `git apply` of a patch |
| POSIX | the harness edit tool → `cat > file <<'EOF'` with a quoted delimiter → `printf '%s'` → `sed -i` for one line (GNU `-i`, BSD `-i ''`) → `node -e` or `python3 -` if installed → `git apply` |

One failed call moves to the next rung; diagnosing the method in the transcript is the cost, not the fallback. A denial is not a failure: `access denied`, `EACCES`, `EPERM` or a read-only file means the path is closed to this session — stop the list there, name the path and ask instead of spending attempts on rungs that fail the same way. Read the changed lines back after a write — a tool that reports success and changes nothing is the failure that hurts most. The rung that works is written back to the project's `AGENTS.md` as one `Write path:` line — replaced in place, never appended. Write to `<file>.tmp` and move it into place, because a redirect truncates before it writes. Quote for the shell, not the file: a single-quoted here-string on PowerShell, a quoted heredoc delimiter on POSIX, so `$`, backticks and `!` stay literal. Never install an editor or a runtime to gain a rung, retry an identical denied call, rewrite a file for a one-line change, or use regex where the text is literal.

## Setup

The first run writes a folder and then leaves. Three artifacts make that folder hold without a session to run it: the pointer, the manifest, and the proof. They are written at setup, by the rules below, never improvised later.

**The pointer — `.cykeek/state.md`, ten lines.** One writer, at every gate transition, last write wins:

```
contract: v3 · 2026-11-28
task: checkout-empty-states
gate: 5 Slice · round 2
ledger: .cykeek/verify/checkout-empty-states.md
proof: capture 390 — disabled, 400 ms PASS
next: contract row 4, then prove at 1440
mode: inline
updated: 2026-11-28 14:02
```

The ledger keeps the detail; this keeps the address. After a compaction, a returned report or a fail, read it and re-enter at the gate it names — `§ Resume and close`, without a directory listing. With no task yet it is still written: `task: none`, `gate: —`, and `next` names the route the first task should take.

**The manifest — `.cykeek/manifest.md`, twelve lines.** One row per contract file the setup wrote:

```
| File | Role | Lines | Version | Date | Mode |
| --- | --- | --- | --- | --- | --- |
| AGENTS.md | process contract | 214 | v1 | 2026-11-28 | merged |
| DESIGN.md | design system | 268 | v1 | 2026-11-28 | generated |
| HEARTBEAT.md | learning loop | 24 | — | 2026-11-28 | generated |
| .cykeek/components/index.md | component index | 12 | v1 | 2026-11-28 | generated |
```

`merged` means the block sits inside someone else's file; `generated` means the file is entirely ours. The pointer is not a row — it changes at every gate.

**The version.** The card's `Card v1 · <date>` is the contract's version. A promotion inside the markers bumps it; a refresh that changes nothing does not. The manifest carries the number the file holds at that date, so a receipt citing a lower version is stale and the read comes first.

**Refresh.** Diff the disk against the manifest: a different line count, an older version, or a date older than the last promotion is the stale row. Re-cut that row, report it, ask only about facts that moved, and change nothing else. Twice in a row produces no diff.

**The boot, in order.** `AGENTS.md` (240) → `HEARTBEAT.md` status and pending (40) → `.cykeek/state.md` (10) → `.cykeek/verify/capability.md` (16) → act. Nothing else is opened to be safe; a task opens one hub at its named section.

**The proof — eight checks, after the last file is written and before the handoff.** One line each: verdict, measured value, and the command shape that produced it. A red row stops the run; a workspace is not handed over broken.

| # | Check | Reads | Red means |
| --- | --- | --- | --- |
| 1 | markers | exactly one `cykeek:begin` and one `cykeek:end` per contract file, begin first | the file is corrupt; regenerate it |
| 2 | routes | every row's file exists and its `## ` section matches | a dead route; fix the row |
| 3 | caps | 240 · 300 · 40 · 60 · 16 · 10 · 12 | generation overshot; cut rows, not rules |
| 4 | counts | every numeral in prose equals the tree | a false number; recount |
| 5 | capability | 16 lines, a date, one evidence line per check, one `^Write path:` line | the next session starts over |
| 6 | pointer and manifest | one contract version; the live task has a ledger file | resume is broken |
| 7 | idempotent | the same generation produces no byte difference | the generation is unstable |
| 8 | outside the marks | a merged file's text outside the markers is unchanged | the user's text was touched; restore it |

`class: non-visual` marks the `DESIGN.md` rows `n/a` by name, never by silence. The table is the artifact: `.cykeek/verify/workspace.md`, twenty lines, dated, one verdict per row.

## Inner loops

Two loops, and only two; everything else is a step inside a gate. Verification starts from the preflight verdict in `.cykeek/verify/capability.md`: what the render path can do decides how a case is proven.

**Inside Route — the plan ladder.** Every decision runs the same ladder, and the first rung is refusal:

1. Does this serve the stated job, or the agent's taste?
2. Will it be met often enough to earn its cost?
3. Does this project already have a pattern that does it?
4. What is the smallest version that still works?
5. Only now: which tool, property or pattern — in the hub's preferred order.

Answer in one sentence and move. The ladder is cheap; skipping it is what produces a dropdown that animates on hover, a fourth typeface, or a 900-line module nobody asked for.

**Inside Contract — the spec.** Not a plan document; the smallest set of statements that make the result falsifiable. Numbers, not adjectives: these ms, this curve, this ratio, this breakpoint. States: default, hover/press, focus, loading, empty, error — the ones that apply and what each looks like. Widths: 320, 390, 768, 1024, 1440 — what changes at each, or unchanged. V-cases: ID, stop, viewport, assertion, status; the assertion is what you will look at, not what you hope. If the spec would outgrow the work, cut it to the two checks that would catch a real failure — a spec nobody reads is worse than two lines someone uses.

**Inside Prove — the four-move loop.** Act, Changes, Review, Loop. Recursive: each round's output is the next round's input, and the ledger records the rounds. Named once here so no domain file repeats it.

1. **Act** — build the smallest slice the spec names. One artifact, one assertion.
2. **Changes** — read the result against the assertion and name the gap in one line: which property, which instant, which width. No named gap means done; do not improve on taste.
3. **Review** — render the checks the domain names: three instants for motion (start, mid, end; transitions 0 / 0.5 / 1), three frozen `?t=N` frames for WebGL (`graphics.md` § WebGL), three scroll positions per act (`motion-stack.md` § The scroll score), two widths for screens. Check fidelity and artifacts, record PASS or the measured delta.
4. **Loop** — a real, fixable gap returns to Act with the named fix and the same assertion. Three rounds on one assertion with no movement is the stop: report the delta, return to Contract, and do not restyle.

Bounds: three rounds, one assertion per round, two widths per visual round. Skill-specific checks ride on this loop — GSAP retimes a timeline position, WebGL re-freezes a frame, a scroll act re-captures its beat. A review or audit that builds nothing still runs Changes and Review on the artifact it was given.

**When the work compresses.** A one-line change or an obvious bug: orient, slice, prove. A review or audit: orient, route, brief as the report, close — nothing is built. A research question: orient, route, close with the evidence named. Multi-session work: the ledger is what makes it one task instead of several. Where a domain file's own decision order disagrees with this file, the domain file wins: this is the shape of the work, not its content. Proof is never the gate that gets skipped.

## Triage

Read when the request is a **problem**, not an instruction. "This feels slow", "I don't know what it should look like", "this codebase is a mess", "why do you keep getting this wrong" — each names a different kind of work, and naming the right flow before touching code is most of the answer. Read the problem statement once and classify it in one line; one flow per request, and if the request really spans two, say which runs first and why.

| What they said | The flow | What it ends with |
| --- | --- | --- |
| "I don't know what it should look like" | Prototype | variants behind a picker, their choice |
| "Build X" | Build — gates 4 to 6 | the thing, verified |
| "It feels slow / too much motion" | Subtract — `motion.md` § Audit | a findings list, then removals |
| "Does it hold up?" | Break | what broke, with the fix |
| "Is this safe?" or "audit this" | Security — `security.md` | a verdict per claim, then the coverage statement |
| "The codebase is a mess" | Untangle | a decision log, then one structural pass |
| "Where should motion go?" | Hunt — `motion.md` § Audit | a proposal list, no code |
| "Which library?" | Pick — `platform.md` § Libraries | one dependency and the reason |
| "How do they do X?" | Research — `agents.md` § Research card | a nine-slot card, then an integration plan |
| "You keep getting this wrong" | Capture | a rule, or better, a check |

**Prototype — when the look is the open question.** Divergence, not iteration on one idea: three tints of the same card waste the comparison; each variant must be a direction you could defend shipping alone. The gate is shared before divergence starts — every direction answers `design.md` § The gate (job, one action, hierarchy, states), and variants may differ on the axis, never on those. A page whose timeline is the wheel adds `motion-stack.md` § The scroll score to that frame. Scope to one piece ("the dashboard" is a project; a stat card is a run), say why it is the highest-leverage one, offer the rest as follow-ups. Recon the stack, tokens and personality; variants use the project's tokens and still feel native — sharing tokens is not convergence. Name the directions: default 3, never more than 5, each with a distinct axis (layout, density, personality, motion, interaction model); if two would differ only in accent colour, they are one direction — replace one. Build in isolation: `/prototypes/<slug>`, or one self-contained HTML file with no project; copy `assets/picker.html` verbatim — it is chrome, never restyled with project tokens; one variant at a time, full size, in realistic surroundings (a toast needs a page behind it); switching is instant — it happens a hundred times a session, so it gets no animation. Verify, then stop: every variant renders, every interaction responds, console clean; present the table — Variant, Axis, When it wins, What it costs — and never pre-pick a favourite; if asked which I would choose, answer from the product's personality and how often the piece is seen, not from taste. Promote on choice on the project's conventions, then delete the prototype surface; a riff request keeps the harness and diverges around the chosen direction.

**Break — when the question is durability.** Feed the component data it was not designed for, behind a "Demo data / Worst case" toggle: the longest plausible name, an unbreakable email, a one-letter name, an empty list, five thousand rows, zero, a negative, a long label, non-Latin text, emoji, numbers with many digits. Report every break with its fix; do not fix silently — the list is the deliverable.

**Untangle — when the code is the problem.** One question drives the whole pass: **should these pieces change together?** Everything wrong is one of three shapes, plus special cases. **Hidden links** — must change together, and the code does not say so: copied logic, a module reading a global set elsewhere, one feature split across folders by layer, an order callers must know. **Needless links** — tied, but they change for different reasons: callers reaching into internals, stable code importing volatile code, a general function with a parameter only one caller understands, a shallow module. **Surplus** — no reason of its own: pass-through wrappers, config for things that never vary, dead code. **Special cases** — an `if` for one value, a parameter for one caller, a swallowed exception, a retry or sleep, a disabled test; each is a requirement the design never accounted for, and together they say what the system needs to be. List every problem and change nothing. Then design from first principles and look for the one fix behind several problems — it usually looks nothing like them. Write the decision log: fix above the problems it solves. Fix the list, not the special cases one by one. Comments and tests are opt-in parts of the pass, and tests go last because they are the safety net.

**Capture — when the lesson is about us.** Corrections are the only reliable source of taste. Find them all, not just the spoken ones: an explicit correction, a silent edit to a value I wrote, a repeated nudge (record where it landed, not the steps), a revert or a rejected variant, a confirmed non-obvious choice. When the failure is in the session rather than in a value, read the transcript on disk and quote the line where it went wrong — never reconstruct it from memory, because a guessed cause becomes a wrong rule. Then filter: would a fresh agent make this mistake on a *different* component? If no, skip it. If a rule already covers it, tighten that rule to a number or a never instead of adding a duplicate. If a violation can be found by matching code, propose a check — a lint rule, a banned token, a test — because a check runs every session and a sentence in a long file gets skipped. Never invent a why: mark it *inferred* and ask. Propose the rules, write only what is approved, and write for an agent who was not here — name the component class, the token, the value.

## Dispatch and build

Split when two or more rows have independent assertions and one owner each, and a verifier who did not write the code exists. A capability file still reading `deferred` is proved here, before this decision; a failure now is the stop setup did not take. Run inline when there is one row, when two rows share a file, or when the evidence is already in the ledger. A split that costs more than it saves is inline, and inline is a mode, not a failure. Both modes run the same gates; the mode changes who builds, not the order.

The assignment a worker gets is the contract and its rows — never the session history (`agents.md` § Contract). One consolidated outcome comes back per round: state, counts, deltas, blocker. Accept, or return the delta to the same worker with its retest scope and nothing else. A built row is reviewed twice: against its contract assertion, then with `review.md`. No state is entered without its artifact; the states live in the ledger's rows, inside gates 5 and 6. Keep every worker alive until the user has seen the result; only the orchestrator commits, and only locally.

**While building.** One artifact at a time, in the smallest slice that can be rendered and judged. The project's vocabulary first: its tokens, components, patterns; a new abstraction earns its place at the third use, not the first. Dependencies, lockfiles and config are not touched while building. Leave the project runnable at every stopping point — a half-applied refactor is a broken session. Split only independent slices; two workers on one artifact overwrite each other.

## Stops

Five lines, nothing else, at every stop:

```
State:   built, awaiting your call
Changed: src/settings/SaveBar.tsx:12-40 — save disables while the request is in flight
Proof:   pnpm test settings → 4 passed; capture 390 — disabled for the 400 ms round trip
Next:    focus ring, then the empty state
Ask:     ship this, or change the copy first?
```

One question per stop, never two. `Changed:` names the file and the lines. `Proof:` is a command and its output, never an adjective. Silence is not an answer. These are the same facts as gate 8 Close, addressed to a person instead of the next session.

**What sends it back.** Prove fails → the four-move loop → still fails after three rounds → Contract with the smallest version that works. The user rejects the brief → Orient. The user rejects the result → the gate named in their reason, never the top. A row keeps failing → split it before assigning it again. A fact is missing and the act is safe → assume, record the assumption in `HEARTBEAT.md`, proceed.

**The budget — when to stop.**

| Cap | Count |
| --- | --- |
| loop rounds per assertion | 3 |
| attempts per gate | 2 |
| rounds per dispatched row | 4 |
| questions per stop | 1 |
| research queries | 5 |
| corrections before a rule is captured | 3 |

Hitting a cap is not permission to continue: it is a stop with the blocker named. Two rounds with no measurable delta is that stop, earlier.

**What counts as a fact.** A file you read, a command you ran and quoted, a version in a lockfile. An API, a signature or a platform behaviour from memory is not a fact: read it, card it (`agents.md` § Research card), or mark it `UNVERIFIED`. "Should work" is never a status. When the question is "is it done?", run the command and quote its output — or say UNVERIFIED.

**Red flags.** Four thoughts mean a gate was skipped: "it's simple", "let me look first", "the reference is overkill", "I'll just do this one thing".

## Resume and close

Resume is a gate, not a memory test. After compaction, a returned report, a fail or a correction: read the ledger's state line, re-enter at the gate it names, refresh the receipt. The ledger is the only thing that survives a break; nothing is inferred from the transcript.

Close answers the same question from the other side — what does the next session need? Three lines: what changed, what is verified, what is open with the next route. Persist the verification ledger so a later session resumes the same cases instead of redefining them. On a branch, exactly three options — merge locally, push and open a PR, keep as-is — then wait; discard only when asked. Off a branch: the commit, local and by `agents.md` § Git's gate, the ledger closed, and the next step in one line. A correction worth keeping goes through `§ Triage` Capture before the window closes.

<!-- source: obra/superpowers (MIT) — using-superpowers, brainstorming, writing-plans, subagent-driven-development, executing-plans, test-driven-development, verification-before-completion, finishing-a-development-branch; animate, choreograph, gestures, emil-design-eng, apple-design, iart-ai/webgl-animation-skills (MIT) — the Act/Changes/Review/Loop verify cycle; ask-emil, prototype, PICKER, untangle-codebase, break-ui, learn-taste — the triage flows; discovery and the write path are Cykeek's own, from the SKILL.md preflight -->
