# Review

Read when the question is "is this change good?" rather than "make it work": a diff, a commit, a PR, a whole repo, or the shortcuts left behind. The order of importance is fixed — correct, safe, holds under load, tested, fast, lean — and lean comes last because a fast wrong answer is still wrong. This is a report the user asked for: give it in full, and change nothing.

Three targets, one protocol. A change → § A change. A codebase → § A repo. What was deferred → § The ledger.

## A change

**1. Understand first.**

- Review what the user names — uncommitted, staged, a branch, a PR, files. Nothing named means the uncommitted changes, else the last commit.
- Read the diff, then the code it touches: callers of every changed function, what those functions call, the tests, the README.
- Trace the flow: where data enters, what is stored, what leaves. When a signature, return value or behaviour changes, grep every caller — a change breaks code it does not touch.
- Name the load the repo expects, from the README or deploy config: one person running a script, or many users and processes at once. Judge scale against it, and say which you assumed.

**2. The six lenses.**

| Lens | Look for |
| --- | --- |
| Bug | wrong result, a crash, an edge missed: empty, zero, the last item, rounding, time zones; a caller the change breaks; a fix in one caller while the shared function stays broken |
| Risk | injection, weak randomness, a secret in code, unchecked input; errors swallowed, writes in the wrong order, no transaction |
| Scale | a check-then-write race, the same work in every process, memory that only grows, a query per item, O(n²) on real input, per-process state that should be shared |
| Missing test | risky logic with no test that fails when it breaks — a branch, a parser, money, security, a data write, a bug fix. One good test, not coverage |
| Speed | a real slowdown is a problem; work repeated in a hot loop is a suggestion, and some software counts every millisecond |
| Lean | code that should not exist: dead code, a speculative option, a helper the repo already has, a dependency for a few lines, an abstraction with one implementation, near-copies that must change together, one function doing unrelated jobs |

**3. Before it is a finding.**

- It needs a concrete case: this input, that situation, this wrong result. No case, no finding.
- Re-read and confirm: the caller exists, the value can be empty, the code is really unused.
- A `shortcut:` comment that names its limit is a decision, not a finding, unless the load already crosses it.
- The smallest fix that works, preferring one that deletes. No style taste, no "consider", no vague worry.

**4. The report.** Short sentences, everyday words, a technical term explained once — the reader may never have seen this code. Open with `What this change does:` in two or three sentences. Then the findings in **Must fix** (bug, security, data loss, breaks at the expected load), **Should fix** (risky code without a test, real slowness, duplication, mixed jobs, code that should not exist) and **Nice to have** (small speed-ups, shorter forms), skipping an empty group and numbering across all of them so the user can say "fix 2 and 5".

Every finding has four parts, one or two sentences each:

2. **Orders land on the wrong day** (`billing/close_day.py:L40-52`)
   - **What this is:** the midnight job bills every order of the day.
   - **Problem:** it takes the day from the server clock, which is UTC, so a 00:30 order in Berlin bills on the day before.
   - **Fix:** compute the day in the shop's zone, `datetime.now(ZoneInfo("Europe/Berlin")).date()`. One line.
   - **If we skip it:** late orders show the wrong date, and accounting fixes them by hand.

Close with `Verdict: Ship.` or `Verdict: fix 1 and 3 first.`, `Lean: -<N> lines possible.` when a lean finding exists, and `Not checked:` one line for what mattered and could not be checked. Nothing found: `What this change does:`, then `Looks good. Ship.` and one line on what you checked.

## A repo

Map before reading deeply: the README, the build and deploy config, the dependency list, the entry points — main, routes, handlers, jobs, CLI — and the tests; the load it expects; the main flows end to end, through user input, money, auth, data writes, background jobs and anything shared between processes. In a big repo go where a mistake costs the most, not file by file, and say which parts you did not read.

Same six lenses, same four-part finding, same evidence rule. Ranked most important first, capped at twenty — if smaller ones are left out, say how many. Before calling code unused, grep the whole tree, including tests, fixtures, config and string references. Open with `What this repo does:` and the load assumed; close with `Verdict:`, `Lean: -<N> lines, -<M> dependencies possible.` and `Not checked:`. Healthy: `What this repo does:`, then `Healthy. Nothing to fix.` and one line on what you checked.

## The ledger

Grep the tree for deferred shortcuts, skipping `.git` and what `.gitignore` covers:

`git grep --no-index --exclude-standard -nE '(#|//|/[*]) ?(shortcut|ponytail):'`

One row per marker, grouped by file: `<file>:<line>, <what was simplified>. ceiling: <the limit named>. upgrade: <the trigger to revisit>`. The convention is `shortcut: <the limit>, <when to upgrade>`, so both come from the comment; a user may name their own marker word, and then grep that. A marker naming no trigger gets a `no-trigger` tag — those are the ones that rot silently. End with `<N> markers, <M> with no trigger.`; clean: `No shortcut debt. Clean ledger.` Write it to a file only when asked.

<!-- source: DietrichGebert/ponytail (MIT) — SKILL.md, AGENTS.md, skills/ponytail-review, ponytail-audit, ponytail-debt -->