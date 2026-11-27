# Pipeline — the eight gates

Read with `loop.md` on every task. The loop names the moves; this names their order, what sends the work back, where the agent stops, and when to stop trying. Every gate leaves an artifact — no artifact, no next gate.

## The eight gates

| # | Gate | It passes when | If not | Stops |
| --- | --- | --- | --- | --- |
| 1 | Intent | the want and the why fit in one sentence, from the request or one question | ask once, then restate the request and name the assumption | — |
| 2 | Route | one reference, or a research card when the technology is not in memory | classify through `flows.md`, then route | no route at all |
| 3 | Brief | five lines: what, why, the shape, what it refuses, what proves it | back to Intent | new behaviour, always |
| 4 | Contract | every row carries the assertion that fails if it is wrong, or its reason | back to Contract; nothing may be assigned | — |
| 5 | Slice | one row that can be built and verified in one round | split the row before assigning | — |
| 6 | Prove | the assertion re-run, read and quoted | the four-move loop, three rounds, then Decide | — |
| 7 | Show | the five-line block below, with one question | apply the delta, return to the named gate | always: wait |
| 8 | Close | ledger, one local commit, three lines | — | on a branch: the three options |

The detail lives where it already belongs: the code gate and the ladder in `craft.md`, the row table and the states in `orchestration.md`, findings in `review.md`, git in `git.md`. This file never repeats them.

## The one-stop rule

A change that fits in one round skips the Brief and stops once, with the result. New behaviour or work that spans rounds stops twice: the brief, then the result. Inside an approved brief the rows run without asking — the agent pauses per batch and per gate failure, not per row. A brief question on a save button costs a turn and changes nothing.

## The show block

Five lines, nothing else, at every stop:

```
State:   built, awaiting your call
Changed: src/settings/SaveBar.tsx:12-40 — save disables while the request is in flight
Proof:   pnpm test settings → 4 passed; capture 390 — disabled for the 400 ms round trip
Next:    focus ring, then the empty state
Ask:     ship this, or change the copy first?
```

One question per stop, never two. `Changed:` names the file and the lines. `Proof:` is a command and its output, never an adjective. Silence is not an answer. These are the same facts as `loop.md` §6 Close, addressed to a person instead of the next session.

## When it goes back

- Prove fails → the four-move loop → still fails after three rounds → Decide with the smallest version that works.
- The user rejects the brief → Intent. The user rejects the result → the gate named in their reason, never the top.
- A row keeps failing → split it before assigning it again.
- A fact is missing and the act is safe → assume, record the assumption in `HEARTBEAT.md`, proceed.

## Two modes

- **Inline** — one context, one build, one review at the end. The normal mode for work that fits a session.
- **Dispatched** — fresh context per row (`orchestration.md`); the row is reviewed twice, against its contract assertion and then with `review.md`; what travels to a worker is the contract, never the session history.

Both run the same gates; the mode changes who builds, not the order. Falling back to inline is a mode change, not a failure.

## The budget — when to stop

| Cap | Count |
| --- | --- |
| loop rounds per assertion | 3 |
| attempts per gate | 2 |
| rounds per dispatched row | 4 |
| questions per stop | 1 |
| research queries | 5 |
| corrections before a rule is captured | 3 |

Hitting a cap is not permission to continue: it is a stop with the blocker named. Two rounds with no measurable delta is that stop, earlier.

## What counts as a fact

A file you read, a command you ran and quoted, a version in a lockfile. An API, a signature or a platform behaviour from memory is not a fact: read it, card it (`research.md`), or mark it `UNVERIFIED`. "Should work" is never a status. When the question is "is it done?", run the command and quote its output — or say UNVERIFIED.

## Close

On a branch, exactly three options — merge locally, push and open a PR, keep as-is — then wait; discard only when asked. Off a branch: the commit, local and by `git.md`'s gate, the ledger closed, and the next step in one line.

## Red flags

Four thoughts mean a gate was skipped: "it's simple", "let me look first", "the reference is overkill", "I'll just do this one thing".

## Where the rest lives

- A bug or a wrong result, before any fix: `debug.md`.
- Findings and the report: `review.md`; answering a review: `review.md` § Receiving.
- Work that splits across agents: `orchestration.md`; branches and isolation: `git.md`.
- A correction worth keeping: `flows.md` § Capture.

<!-- source: obra/superpowers (MIT) — using-superpowers, brainstorming, writing-plans, subagent-driven-development, executing-plans, test-driven-development, verification-before-completion, finishing-a-development-branch -->
