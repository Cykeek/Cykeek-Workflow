# The loop

Preflight runs before move 1: reasoning and image input are the two model traits the whole loop rests on, and a session without them stops before this file matters.

Read this before the domain reference. Every other file here assumes these moves; naming them once means the agent does not rediscover the process per domain, and does not spend tokens narrating it.

## Why the shape is the same everywhere

Read forty skills closely and the same skeleton appears under the content: refuse first, count in numbers, write the check before the thing, build the smallest version, look at the result, stop. Motion, design, refactoring, Figma-to-code, accessibility review — different vocabulary, same six moves. That is not a coincidence; it is what doing careful work looks like from the outside.

| Move | The question | The artifact it leaves | Budget |
| --- | --- | --- | --- |
| 1 Frame | What is being made, and what does done mean? | one line plus a routing row | 2 |
| 2 Decide | Should this exist at all? | a decision and its reason | 3 |
| 3 Specify | What must be true at the end, in numbers? | the spec and its V-cases | 10 |
| 4 Build | What is the smallest slice that can be seen? | the code | proportional |
| 5 Prove | What evidence closes this? | what was rendered, compared, and failed | 5 |
| 6 Close | What does the next session need? | three lines and a ledger | 3 |

## 0 — Receipt

Not a seventh move — a condition on starting one. An **act** is the session's first call, any call that changes state (write, delete, move, install, build, run, deploy, dispatch), a new task inside the session, or resuming after compaction, a returned report or a failure. Reading, searching and planning are not acts, so exploring stays free.

Before an act, one line:

`Rules: AGENTS.md v<n> · <section cited> · owns: <concept> · refuse: <what> · proof: <check>`

Cite the section, do not paraphrase it: a citation naming no section is a table of contents, and a receipt naming one that does not exist is a fail at close. One receipt covers a contiguous run on one area — forty edits to one component are one act, not forty. Refresh it when the area, the file or the goal changes, and after every fail, correction or returned report.

Re-read the whole of `AGENTS.md` — the file, not the card — on the first call of a session, after compaction, when a report returns, after any fail, and whenever a rule you are relying on feels unfamiliar. The card survives a long session; the file is the truth and may have changed since it was read. When the file on disk differs from what the receipt cites, the receipt is stale and the act waits for the read.

## 1 — Frame

Name the artifact — a page, a component, a dashboard screen, a transition, a refactor — the job it does, and the reference that governs it. If the artifact cannot be named in one sentence, ask one question and name it yourself; two questions means the request was not read.

Then open exactly one reference, whole. Not a search across the folder, not two files to be safe.

## 2 — Decide

Every reference in this skill runs its own version of the same ladder, and the first rung is always refusal:

1. Does this serve the stated job, or the agent's taste?
2. Will it be met often enough to earn its cost?
3. Does this project already have a pattern that does it?
4. What is the smallest version that still works?
5. Only now: which tool, property or pattern — in the reference's preferred order.

Answer in one sentence and move. The ladder is cheap; skipping it is what produces a dropdown that animates on hover, a fourth typeface, or a 900-line module nobody asked for.

## 3 — Specify

The step most often skipped, and the reason results cannot be checked later. A spec is not a plan document; it is the smallest set of statements that make the result falsifiable.

- **Numbers, not adjectives.** These ms, this curve, this ratio, this breakpoint.
- **States.** Default, hover/press, focus, loading, empty, error — name the ones that apply and what each looks like.
- **Widths.** 320, 390, 768, 1024, 1440. Say what changes at each, or say unchanged.
- **V-cases.** ID, stop, viewport, assertion, status. The assertion is what you will look at, not what you hope.

If the spec would outgrow the work, cut it to the two checks that would catch a real failure. A spec nobody reads is worse than two lines someone uses.

## 4 — Build

- **Split only independent slices.** If the harness can spawn agents, dispatch each one with its `Done` assertion and integrate only the reports you accepted (`references/orchestration.md`); if it cannot, run them in order and write each report first. Two workers on one artifact overwrite each other.
- One artifact at a time, in the smallest slice that can be rendered and judged.
- The project's vocabulary first: its tokens, components, patterns. A new abstraction earns its place at the third use, not the first.
- Dependencies, lockfiles and config are not touched while building.
- Leave the project runnable at every stopping point. A half-applied refactor is a broken session.

**Write path.** Preflight records which rung writes here. Use it in order; stop at the first that succeeds.

| OS | Rungs |
| --- | --- |
| Windows | the harness edit tool → `Set-Content -LiteralPath` with a single-quoted here-string → `[IO.File]::WriteAllText` (FullLanguage only; a read-only sandbox runs ConstrainedLanguage and rejects it) → `node -e` or `python -c` if already installed → `git apply` of a patch |
| POSIX | the harness edit tool → `cat > file <<'EOF'` with a quoted delimiter → `printf '%s'` → `sed -i` for one line (GNU `-i`, BSD `-i ''`) → `node -e` or `python3 -` if installed → `git apply` |

One failed call moves to the next rung; diagnosing the method in the transcript is the cost, not the fallback. WSL follows the POSIX column, and a `/mnt/c/` path is a cross-filesystem write that can drop permissions. A denial is not a failure: `access denied`, `EACCES`, `EPERM` or a read-only file means the path is closed to this session, so stop the list there, name the path and ask instead of spending four more attempts on rungs that fail the same way. Read the changed lines back after a write — a tool that reports success and changes nothing is the failure that hurts most. The rung that works is used for the rest of the session, written back to the project's `AGENTS.md` as one `Write path:` line — replaced in place, never appended — and never re-derived. Write to `<file>.tmp` and move it into place, because a redirect truncates before it writes. Quote for the shell, not the file: a single-quoted here-string on PowerShell, a quoted heredoc delimiter on POSIX, so `$`, backticks and `!` stay literal. Never install an editor or a runtime to gain a rung, retry an identical denied call, rewrite a file for a one-line change, or use `-replace`/`sed` regex where the text is literal.

## 5 — Prove

Verification starts from the preflight verdict in `.cykeek/verify/capability.md`: what the render path can do decides how a case is proven. Tool names differ per harness; the checks do not.

A write is proved by the changed lines, read back, not by the tool's success message.

Then render at two widths and compare against the spec or the reference — never against memory. Report what was seen and where. "It should work" is not a status, and a passing test suite is the prerequisite, not the conclusion.

Iterate on the assertion that failed, not on the whole screen. Three passes on the same assertion with no movement means the cause is upstream, so go back to Decide rather than restyling.

### The four-move loop — Act, Changes, Review, Loop

Every skill execution runs this inside Prove. It is recursive: each round's output is the next round's input, and the ledger records the rounds. It is named once here so no domain file repeats it.

1. **Act** — build the smallest slice the spec names. One artifact, one assertion.
2. **Changes** — read the result against the assertion and name the gap in one line: which property, which instant, which width. No named gap means done; do not improve on taste.
3. **Review** — render the checks the domain names: three instants for motion (start, mid, end; transitions 0 / 0.5 / 1), three frozen `?t=N` frames for WebGL (`webgl.md` §6), three scroll positions per act (`scroll.md` § The score), two widths for screens. Check fidelity and artifacts, record PASS or the measured delta.
4. **Loop** — a real, fixable gap returns to Act with the named fix and the same assertion. Three rounds on one assertion with no movement is the stop: report the delta, return to Decide, and do not restyle.

Bounds: three rounds, one assertion per round, two widths per visual round. Skill-specific checks ride on this loop — GSAP retimes a timeline position, WebGL re-freezes a frame, a scroll act re-captures its beat. A review or audit that builds nothing still runs Changes and Review on the artifact it was given.

## 6 — Close

Three lines: what changed, what is verified, what is open with the next route. Persist the verification ledger so a later session resumes the same cases instead of redefining them — that is what makes the loop recursive rather than merely repeated.

## Worked through

> Add a hero to the landing page.

**Frame.** Hero on `/`; done means it reads at 1440 and 390, uses `DESIGN.md` tokens, and has one entrance. Governed by `design.md`, with §2 of `motion.md` for the entrance.

**Decide.** Motion earned: the hero is the page's one dealt moment, seen once per visit, not a hundred times a day. Entrance only — no scroll parallax on a section that is already on screen.

**Specify.** H1 48/1.1, subhead 18, CTA 44px target. Entrance: `opacity` 0→1 with `y` 12→0, 500ms, `--ease-out`, 60ms stagger across three elements. Reduced motion drops the `y`. V1 1440, V2 390, V3 reduced motion.

**Build.** Existing tokens, no new dependency, one component. Runnable at the end of the move.

**Prove.** Both widths screenshotted and compared to the spec; reduced motion emulated, not assumed; the failing assertion fixed and re-shot.

**Close.** Hero written and verified at two widths; reduced-motion pass open. Next route: `motion.md` §5 for the stat row beneath it.
## The token contract

Quality comes from reading the right 100 lines, not from reading 1,000. The order is fixed:

metadata → routing row → one reference, whole → the specific regions of the files it names.

Never the reverse. Never a folder listing to find what you already know the name of.

- Search before reading, read the region rather than the file, and never re-read what this session already read.
- Narrate decisions, not steps. One line per move beats a paragraph per file.
- Let code and diffs carry the detail; prose earns its place only where the code is silent.
- Stop when the six moves are done. Length is not thoroughness, and extra sections are extra surface to be wrong on.

## When the loop compresses

- **One-line change, obvious bug, typo:** frame, build, prove. Moves may be skipped; proof may not.
- **Review or audit:** frame, decide, specify as the report, close. Nothing is built.
- **Research question:** frame, decide, close — with the answer's evidence named.
- **Multi-session work:** the ledger is what makes it one task instead of several.

Where a reference's own decision order disagrees with this table, the reference wins. This is the shape of the work, not its content.

<!-- source: animate, choreograph, gestures, emil-design-eng, apple-design, iart-ai/webgl-animation-skills (MIT) — the Act/Changes/Review/Loop verify cycle -->
