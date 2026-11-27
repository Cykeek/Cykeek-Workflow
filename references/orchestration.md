# Orchestration

Read when the work splits across agents. The orchestrator's job is not the work: it states the contract, reads one outcome per round, and refuses a result with no number behind it.

Plan expensively, execute cheaply. A contract rejected before any code costs one line; the same mistake found after the build costs three rounds. Every token spent during execution must be an order or a measurement.

## States

| State | Enter when | Artifact | Leaves to |
| --- | --- | --- | --- |
| `PLANNED` | the contract exists and every row has an assertion | ledger | `ASSIGNED` |
| `ASSIGNED` | one order batch, at most three rows, each with a retest scope | ledger | `BUILT` |
| `BUILT` | every ordered row's code exists, nothing outside them touched | diff | `CHECKED` |
| `CHECKED` | each row measured at its viewport, plus reduced-motion | ledger | `ACCEPTED` or `BUILT` |
| `ACCEPTED` | all rows PASS and the orchestrator viewed two cases itself | ledger | `APPROVED` |
| `APPROVED` | the user said yes after seeing one capture | thread | `SHIPPED` |
| `SHIPPED` | clean sweep done, workers released, ledger closed | ledger | — |

Failure edges are explicit: `CHECKED → BUILT` returns a delta to the same applier, `ACCEPTED → PLANNED` re-scopes one row, `APPROVED → CHECKED` when the user names something. No state is entered without its artifact on disk, and none is skipped.

## The contract

The plan is a table, not prose. Each row is the work order, the spec and the test at once.

```
Row | Trigger | From → to | Duration / curve | Stagger | Reduced motion | Assertion
```

- One row per item — **including a row that says `none`, with its reason.** Refusal is part of the plan, not an omission.
- 40 lines, no rationale. Values, never adjectives: a row the applier must interpret is a row that will be wrong.
- The applier and the reviewer receive this table. Nobody receives a reference the table did not name.
- After code exists, rows are amended additively. Rewriting a row after the build is a new task.

## The order

After round one the unit of work is an order, not a rebuild:

```
#4 | S3 | 390: reveal fires at 40% viewport, copy clipped | applier | re-check S3@390 only
```

- Three fields carry everything: what is wrong with evidence, who owns it, what to re-run.
- Nothing outside the retest scope is touched or re-rendered.
- A second failure on a row must name a different mechanism. A retry is a gamble, not an order.

## The ledger

`.cykeek/verify/<task>.md`, under 60 lines, read before any work and written by everyone:

```
state: CHECKED  round: 2  goal: "scroll reveals that explain the page, not decorate it"
rows: 9 planned · 6 pass · 2 delta · 1 blocked
S1 hero     fade-up 24px   400ms ease-out  none  PASS 1440/390
S3 features stagger 60ms   360ms ease-out  fade  FAIL(Δt 200ms) 390
S4 pricing  none — a static table, motion would fight scanning
open: S3 → applier, order #4
```

That status line is what the orchestrator reads. Not three reports to reconcile.
- **Facts travel sideways, decisions travel up.** A worker may ask another for a token, a viewport result, or an interface it changed; it may not ask another to choose, approve, or restate the plan.
- Where the harness messages peers directly, that is the same rule and this file is still the record.
- A restarted worker resumes from this file. That is where the savings compound: nothing has to be explained twice.

## Receipts

- Every assignment pastes the project's rules card inside its twelve lines. A worker often does not inherit the parent's context, and the rules are the one thing it cannot do without.
- A report opens with its receipt. One without a cited card version is returned as `no receipt` — not read, not partially accepted.
- A receipt citing an older card than the orchestrator's means the worker acted on rules that had already changed: reissue its rows. The PASS may be real; the reasoning behind it was not current.
- The ledger carries the round's receipt beside its rows, so a round that skipped one is visible after the session, not only during it.

## Four lenses

A UI task is one piece of work seen from four angles. When workers exist they are agents; when they do not, one session runs the lenses in order and says so in one line.

| Lens | Owns | Returns | Never |
| --- | --- | --- | --- |
| UX | the job, the one action, the screen inventory, hierarchy order, every state | eight lines, states included | a colour, a font or a layout |
| Direction | the references studied, the competitor read, palette and type, tokens | the design card | another site's layout, copy or skin |
| Build | one section per order, from the card and the system | the diff | deciding the look |
| Review | the render at five widths, keyboard, contrast, states | the deltas, in a Before/After table | grading its own build |

- At most three workers, one Build and one Review, never the same agent. UX rides with Direction on a new screen; alone when only the look changes.
- Every return opens with the design card version. A lens that returns no artifact did not run.
- All four sit behind `design.md` § The gate: five answers, one line each, under the receipt.

## Caps

| Artifact | Cap |
| --- | --- |
| Contract | 40 lines |
| Assignment to one worker | 12 lines — goal, rows, files, retest scope, report format, the rules card |
| Consolidated outcome | 8 lines — state, counts, deltas, blocker |
| One order | 1 line |
| Orchestrator reads per round | 60 lines — status, open rows, outcome |
| Rounds before the user sees it | 1 — the first visible build |
| Orchestrator's own visual checks | 2 — the simplest row and the riskiest |
| Workers | 3 — build, direction, review, one each, reused; UX rides with direction |

## Stops

Three, and they are not judgement calls: two rounds with no net delta, four rounds in any direction, a third attempt at one row. Each one stops the loop and brings the user the list, the evidence, and the two fixes already tried. A loop that cannot fail cannot finish.

## Never

- Spawn a worker to answer what the ledger already says.
- Let two workers write one file. One writer, many readers.
- Accept a PASS without a viewport and a number.
- Ask the user without a capture.
- Re-run a suite for one row.
- Respawn an idle worker. A send-back reuses it.
- Hand a worker the routing table, or any reference the contract did not name.
- Let the applier grade, the reviewer edit, or the planner build.
- Release anyone before `APPROVED`.

## No subagents

The states do not change; only the cast does. One agent advances the ledger by hand: write the row status, then the state line, before starting the next. Say in one line that the work is serial, so the user knows why nothing is parallel.

<!-- source: delegate_task, task_status, task_cancel, prototype-review discipline -->
