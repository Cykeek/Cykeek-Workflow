# AGENTS.md

This file governs **this repository** — the Cykeek skill itself — not the projects the skill sets up. It is written by `SKILL.md` Part 1 and edited only inside its markers.

<!-- cykeek:begin -->

## 0. Rules card

1. Order: the user's instruction > this file > `SKILL.md` > the reference > your preference.
2. Before any act that changes state — write, delete, build, run, install, dispatch — produce one line:
   `Rules: AGENTS.md v<n> · <section cited> · owns: <concept> · refuse: <what> · proof: <check>`
3. Before the first line of code: the five answers at `references/craft.md` § The gate.
4. Never: install a dependency, edit outside the markers, touch a file no rule names, ship unverified.
5. Re-read this whole file when: session start, compaction, a report returns, any fail, any correction.

Card v1 · 2026-11-27

## 1. What this is

Cykeek is one agent skill that makes a coding agent run a project the way a careful engineer would: set it up once, then work task by task under a written contract. This repo *is* the skill — clone it into a skills folder and it works, with nothing installed.

Precedence: this file, then the user's instruction, then `SKILL.md`. A rule here yields only to an explicit, recorded decision in `HEARTBEAT.md`.

## 2. Goal and facts

**Goal.** Keep Cykeek a single skill that turns a project into one an agent can work in without re-deriving the rules every session — and keep it cheap to read.

**For.** Agents that load it (Claude Code, Codex, any client that reads Agent Skills) and the person who maintains it.

**Done means.** A change lands when the skill's own rules survive contact: budgets hold, every claim matches a file, evals cover the behaviour, and an agent reading only `SKILL.md` could reproduce the intent.

**This is not** a website, an app, a framework, or a bundle of the 52 skills it folds in. It is Markdown, one HTML demo, and a JSON test suite.

| Fact | Value |
| --- | --- |
| Runtime | none — no `package.json`, no build, no test runner |
| Dependencies | none, by design |
| Shipped files | `SKILL.md`, `README.md`, 18 files in `references/`, `assets/picker.html`, `evals/evals.json` |
| Sizes | `SKILL.md` ~503 lines · each reference ≤140 · `README.md` ~266 |
| Evals | 70 cases in `evals/evals.json`, assertions in plain English |
| Install | the same files, this one included, at `~/.agents/skills/cykeek/` — only `.git*` stays behind |

## 3. Session start

Read this file, then `HEARTBEAT.md` for status and pending. State the session's preflight verdict — reasoning, image, render path, subagents — in one line before the first edit; a model without the first two does not work here. There is no `DESIGN.md`: this is a `class: non-visual` repo, and a design system for it would be a file that never earns its lines. Restate the goal in one line before starting.

Write path: windows · edit tool → `Set-Content -LiteralPath` with a single-quoted here-string → `git apply` · 2026-11-27

## 4. Working loop

Six moves, one line each: **frame** (what changes, which file owns it, which reference governs) · **decide** (should this rule exist at all) · **specify** (the exact lines, counts and files) · **build** (the smallest edit that carries the change) · **prove** (grep, count, parse — see §11) · **close** (what the next session needs).

Compress for a one-line fix. Proof is never the move that gets skipped.

## 5. Routing

| Task | Read | Never |
| --- | --- | --- |
| Change the process, phases or contracts | `SKILL.md` | editing `README.md` first |
| Change what an agent is told to do in a domain | `references/loop.md`, then that one file | reading the other sixteen to be safe |
| Add or retune a route | `SKILL.md` routing table + the named reference | a row naming a file that does not exist |
| Change the prototype picker | `references/design.md`, `assets/picker.html` | redesigning it; it is a verbatim copy |
| Add a case | `evals/evals.json` | an assertion that cannot be checked |
| Change the coding gate or the card | `references/craft.md` § The gate, `references/loop.md` §0 | a reminder in prose where a receipt would bind |
| Change the design gate or the design card | `references/design.md` § The gate, § The card, `SKILL.md` § What `DESIGN.md` contains | a reminder in prose where a card would bind |
| Change the scroll pipeline | `references/scroll.md`, `SKILL.md` routing row | a device list without the score |
| Change the motion clone protocol | `references/motion-clone.md`, `references/motion.md` | a clone verdict without row numbers |
| Change the Android hub | `references/android.md`, `SKILL.md` routing row | an Android API recalled from memory |
| Commit, amend or push | `references/git.md` § The gate | a commit nobody asked for, a push nobody asked for |

## 6. Ordering rules

1. `SKILL.md` first, then the reference it points at, then `README.md`, then the evals. Never the reverse — the README describes what exists.
2. Counts before prose. Any number written down is checked against the files in the same turn.
3. One posture at a time: a change that edits files does not also grade them.
4. After any change, sync the install copy — hash both trees, not just the file you touched.

## 7. Non-negotiables

1. Every line must change a decision an agent would otherwise get wrong. If it does not, delete it.
2. A number in prose is a fact about files. Grep before writing it.
3. No skill is assumed installed. Routes say "if installed" and degrade without it.
4. The skill works with zero dependencies and zero setup. No build step, no script that must run first.
5. Folder name and frontmatter `name` stay `cykeek`, lowercase.
6. `references/<domain>.md` is depth, never a precondition. `SKILL.md` alone must be usable.
7. Budgets hold: reference ≤140 lines, `HEARTBEAT.md` ≤40, `SKILL.md` grows only by replacing text.
8. `README.md` describes behaviour that exists. No forward claims, no aspirational features.
9. Every reference ends with `<!-- source: ... -->` naming where its guidance came from.
10. The install copy mirrors the shipped files after every change.
11. Commits stay local until the user says push; one per logical change, a follow-up edit amends, no agent in the author or a trailer.

## 8. Gates and definition of done

A change is done when: `SKILL.md` and `README.md` agree; every count matches; `evals/evals.json` parses; each touched reference is within budget; the install copy is synced; and a fresh reader could act on the change without asking a question the files should have answered.

## 9. Never

- Add a reference without a routing row, a budget entry and a source line.
- Renumber, rename or reorder without grepping `SKILL.md`, `README.md` and `evals/` for the old shape.
- Let the two copies drift. The install folder and this repo hold the same bytes, `.git*` excepted.
- Introduce a dependency, a lockfile or a generated artifact.
- Leave a stale count in prose — "twelve files" after the thirteenth lands is a bug.
- Commit or push without being asked, or push twice for one instruction.
- Stack a second commit on a follow-up edit, or let an agent sign the message.

## 10. Heartbeat

Capture a correction when the user edits a file you wrote, repeats an instruction, or rejects a shape you chose. Three corrections on one target, an explicit "remember this", or an approved close promotes it here, inside the markers. Prune at twenty entries.

## 11. Verification

This repo ships no visual surface except `assets/picker.html`, so verification is mostly mechanical:

| Claim | Check |
| --- | --- |
| line counts | `(Get-Content file).Count` |
| a count in prose | grep the numeral across `SKILL.md`, `README.md`, `evals/` |
| evals valid | parse the JSON |
| install in sync | hash both trees and diff the file lists, not the line counts |
| a claim about the session | the preflight verdict: model traits, render path, subagents, write path |
| a file written | the recorded rung, then the changed lines read back |
| an act | the receipt, and the section it cites exists |
| a design review | the gate answered, and the render looked at at five widths |
| a scroll sequence | the score answered, frames at three positions per act, one phone statement |
| picker still works | open it, capture at 390 and 1440, check the three variants and the keys |
| a commit | `git log -1 --format=%B` read back: one idea, why in the body, no trailer, still unpublished |

Only three states: PASS, FAIL with the measured difference, UNVERIFIED when the capability is absent. "Looks right" is not one of them.

## 12. Orchestration

This repo is small enough that most changes are one agent's work. Split only when a change spans `SKILL.md`, several references, `README.md` and the evals at once — and then the contract is a five-row table in `.cykeek/verify/<task>.md`.

The states are fixed: `PLANNED → ASSIGNED → BUILT → CHECKED → ACCEPTED → APPROVED → SHIPPED`. Caps: 40-line contract, 12-line assignment, 8-line outcome, 3 rows per batch, 4 rounds. Stops: two rounds with no net delta, four rounds in total, a third attempt at one row. One writer per file; whoever builds never grades; no worker is released before `APPROVED`.

With no subagents the states run serially, by hand, in the same ledger — and the session says so in one line.

## 13. Research

A request naming a technology you cannot state from memory starts with a card, not a contract: check the repository and `node_modules` first, frame one claim, climb from versioned docs to real implementations, and write `.cykeek/research/<slug>.md` — nine slots, 25 lines, each answered or named `unknown`. Five queries maximum. No dependency enters without a version, a licence and a cleanup path; a twenty-line spike proves the API before the app is touched. Search results never enter the transcript.

<!-- cykeek:end -->
