# Agents

**Index** — Contract · Research card · Git. Read the sections the routing row names, not the file.

## Contract

Read when the work splits across agents. The orchestrator's job is not the work: it states the contract, reads one outcome per round, and refuses a result with no number behind it. Plan expensively, execute cheaply. A contract rejected before any code costs one line; the same mistake found after the build costs three rounds. Every token spent during execution must be an order or a measurement.
### States

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
### The contract

The plan is a table, not prose. Each row is the work order, the spec and the test at once.

```
Row | Trigger | From → to | Duration / curve | Stagger | Reduced motion | Assertion
```
- One row per item — **including a row that says `none`, with its reason.** Refusal is part of the plan, not an omission.
- 40 lines, no rationale. Values, never adjectives: a row the applier must interpret is a row that will be wrong.
- The applier and the reviewer receive this table. Nobody receives a reference the table did not name.
- After code exists, rows are amended additively. Rewriting a row after the build is a new task.
### The order

After round one the unit of work is an order, not a rebuild:

```
#4 | S3 | 390: reveal fires at 40% viewport, copy clipped | applier | re-check S3@390 only
```
- Three fields carry everything: what is wrong with evidence, who owns it, what to re-run.
- Nothing outside the retest scope is touched or re-rendered.
- A second failure on a row must name a different mechanism. A retry is a gamble, not an order.
### Dispatch

- **Whoever builds never grades.** A built row is reviewed twice: against its contract assertion, then with `review.md`. Both name a delta; the row passes only on both.
- The dispatcher hands a worker its contract and rows, never the session history. What the worker needs is in its twelve lines.
- Do not dispatch: one row alone, rows coupled with no independent assertion, or evidence already in the ledger. Those run inline — inline is a mode, not a failure. With workers available that is a choice; with none it is the recorded serial deviation, one line, and the gates do not change.
### The ledger

`.cykeek/verify/<task>.md`, 40 lines, read before any work and written by everyone:

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
### Receipts

- Every assignment pastes the project's rules card inside its twelve lines. A worker often does not inherit the parent's context, and the rules are the one thing it cannot do without.
- A report opens with its receipt. One without a cited card version is returned as `no receipt` — not read, not partially accepted.
- A receipt citing an older card than the orchestrator's means the worker acted on rules that had already changed: reissue its rows. The PASS may be real; the reasoning behind it was not current.
- The ledger carries the round's receipt beside its rows, so a round that skipped one is visible after the session, not only during it.
### Four lenses

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
### Caps

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
### Stops

Three, and they are not judgement calls: two rounds with no net delta, four rounds in any direction, a third attempt at one row. Each one stops the loop and brings the user the list, the evidence, and the two fixes already tried. A loop that cannot fail cannot finish.
### Never

- Spawn a worker to answer what the ledger already says.
- Let two workers write one file. One writer, many readers.
- Accept a PASS without a viewport and a number.
- Ask the user without a capture.
- Re-run a suite for one row.
- Respawn an idle worker. A send-back reuses it.
- Hand a worker the routing table, or any reference the contract did not name.
- Let the applier grade, the reviewer edit, or the planner build.
- Let a worker run git. The orchestrator commits, once, locally.
- Release anyone before `APPROVED`.
### No subagents

The states do not change; only the cast does. One agent advances the ledger by hand: write the row status, then the state line, before starting the next. Say in one line that the work is serial, so the user knows why nothing is parallel.
## Research card

Read when the request names a library, platform API or technique whose current shape you cannot state from memory — or when the user says "make it like that site". No research tools? Then the repository is the source: imports, lockfile, `node_modules/<pkg>` types. Mark every outside claim UNVERIFIED and name what would settle it. Never fill a gap with a plausible API.
### The rule

One card per topic. Search results never enter the transcript — extract while reading, close the tab, keep 25 lines. A research round that changes no row of the plan was a wasted round.
### 0. The repository answers first

- Grep the package name; read `package.json` and the lockfile for the exact version.
- If installed, `node_modules/<pkg>/README.md` and its `.d.ts` are the version you actually have — better than any doc site.
- Find one existing usage before writing a second: the project's own idiom beats the docs' first example.
- Already used and working? You are reading, not researching.
### 1. Frame — five lines before the first query

1. What it is: library, platform API, pattern or effect. Each has a different authority.
2. The version in play. The wrong major is the single largest source of wrong answers.
3. The claim to settle: "how do I X in Y, with <framework>, on <version>". One question, not a topic.
4. What you need back: a call signature, an integration point, a number or a pattern.
5. What would make you wrong here: SSR, RSC, StrictMode double-mount, cleanup, reduced-motion, mobile.
### 2. Source ladder

Climb only as far as the question requires. Each rung is cheaper to trust than the one below it.

1. **Versioned docs** — the API reference page, not the landing page.
2. **Changelog or migration guide** — the breaking change every tutorial predates.
3. **Official examples** — the project's own demo, compiled and current.
4. **Real code** — GitHub code search over recent, starred projects; issues and discussions for what breaks in production.
5. **Community** — blogs, videos, StackOverflow. Edges only. Never an API signature.

Stop when two independent sources agree on the same call. If rung 4 contradicts rung 1, trust the code, then check the version again: docs describe intent, code describes reality.
### 3. Queries

- Quote the API: `"gsap.context" cleanup react`.
- Pin version and framework: `gsap 3.12 ScrollTrigger next.js app router`.
- Paste error text verbatim, with the library name.
- Ask for the rejection too: `gsap vs motion one performance` — the alternative names the trade-off.

Five queries maximum. Three that add nothing end the web pass; read the installed types instead.
### 4. Images

An image settles three things: what the effect looks like at start, middle and end; the layout it lives in; the spacing and easing it implies. Capture the reference at rest, mid-motion and settled, then write numbers — translate distance, duration feel, stagger — never adjectives. Not for: colour values you can sample from the real site, or "inspiration". No image tool: describe the motion from text, mark the visual slot UNVERIFIED, ship the contract anyway.
### 5. What to extract — the nine slots

1. **Identity** — name, version, licence, weight min+gzip.
2. **One line** — what it does that the platform does not.
3. **Three calls** — the only API you will actually use.
4. **Integration point** — where it hooks: ref, effect, plugin registration, CSS import, provider. `file:line` in this codebase.
5. **Minimal example** — ten lines, from the docs, adapted to this stack.
6. **Lifecycle** — cleanup, teardown, SSR and RSC guard, StrictMode double-invoke.
7. **Pitfalls** — the two failures users report most, in their words.
8. **Cost and access** — main-thread work, layout thrash, bundle budget, reduced-motion path, keyboard.
9. **Exit** — how it is removed later, and what you would use instead.

Each slot is answered or `unknown — what it would take`. A silent blank is the failure this file exists to prevent.
### 6. Real implementations

Read three, not thirty: a shipped site using it, a repo using it in your framework, and its issue tracker filtered by your framework and version.

- Take: composition, config, cleanup, where it mounts, what it is not allowed to touch.
- Reject: tutorials older than the last major, snippets with no teardown, timings copied between projects, anything fighting the framework's own rendering.

Copy the structure, never the pixels — the look belongs to the reference clone protocol, which has its own rules.
### 7. The card

`.cykeek/research/<slug>.md`, 25 lines: the nine slots, then the sources with dates. The next worker, the applier and the orchestrator read the card, not the web. That is the whole token argument — 25 lines bought once, read four times.
### 8. Card to contract

1. Dependency line: exact version, licence, weight, and "already present?" answered.
2. Files touched, in dependency order, one line each.
3. The riskiest unknown, named. If it is the whole feature, spike it first.
4. Rollback: what you delete to undo.
5. Contract rows added (§ Contract) — one per behaviour, assertions included.
6. **Spike before integration** — twenty lines in the real framework, mounted, at 390 and 1440. If the spike fails, the plan is wrong, not the app.

Then build the smallest visible slice.
### 9. Gates

- No dependency without a version pin, a licence and a bundle number.
- No snippet without its teardown.
- No "done" from research alone — the card is a plan, the spike is proof.
- Two rounds, then decide with the unknowns named. More searching is procrastination in a lab coat.
## Git

The repository is the user's, not the agent's. Writing files and publishing history are two acts with two permissions: a commit is local and expected at the close of a task; a push is the user's word, not a conclusion the agent draws.
### The gate

Five answers, one line each, before any git write. A commit without them is a FAIL, however clean the diff.

1. **What changed** — the files, and the one behaviour that moved. Not "several fixes".
2. **One commit or several** — by logical change, never by file count; the split is named here.
3. **New or amend** — has the last local commit already taken this change on?
4. **Local or published** — is the target commit on the remote? Published history is not rewritten.
5. **What proves it** — the diff, the check, and the message read back after the commit.

Who answers the five is settled in § Contract — "whoever builds never grades" — and the gate is reported as a receipt in the shape of `process.md` § Receipt.
### The pipeline

Five steps, in this order, every time.

| Step | Act | Stop if |
| --- | --- | --- |
| Status | `git status --short` — every path accounted for | a file changed that no rule names |
| Group | one group per logical change, ordered by dependency | a group exists only to make the diff smaller |
| Stage | `git add <paths>`, explicit, never `-A` blind | a secret, `.env`, build output or dependency folder appears |
| Message | subject, blank line, why | the body needs "also" |
| Commit | `git diff --check`, then commit or amend | the check reports whitespace damage |

Stage the contract files and the evidence that closed the task: `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md`, `.cykeek/**`. Never stage `node_modules`, build output, logs, editor state, or anything the `.gitignore` already refuses — a file forced in with `-f` is a decision, not a convenience.
### The message

The subject says what the tree does after the commit; the body says why it had to.

- Subject imperative, fifty characters, seventy-two the hard ceiling, no trailing period, no `fix:`, no emoji. A `scope: change` prefix only when the scope is a real subsystem — `auth: reject expired session tokens`, not `feat: update stuff`.
- Blank line, body wrapped at seventy-two: the problem, what was wrong with it, why this shape rather than the obvious one, and what is deliberately not included. No "this commit", no "I".
- One idea per commit. A change that needs "also" in the body is two commits; a change that cannot be described without the diff was not understood yet.

```
auth: reject expired session tokens

The session lookup trusted the expiry claim without decoding the
signature, so a stale token outlived its window whenever the client
kept the tab open past it.

Verify the token before reading the expiry, and treat a malformed
one as absent rather than as an error the login screen has to dress
up.
```
### Isolation

- Detect what exists before creating anything: `git rev-parse --show-toplevel`, `git worktree list`, `git status -sb`. A worktree or branch the harness already made is used as-is.
- Never create a branch or a worktree unasked. Isolation is requested, not assumed.
- Start from a clean baseline: a dirty tree before the first build is a fact to report, not a state to build on.
- Closing a branch offers exactly the three options of `process.md` § Resume and close — merge locally, push and open a PR, keep as-is — and waits for the choice.
### Amend

The unit is the logical change, not the file. A follow-up edit to files the last local commit already owns amends that commit: a second request against the same stylesheet is not a second commit.

- `git commit --amend`, then rewrite the message to describe everything the commit now contains. `--no-edit` only while the message is still true.
- Read `git log -1 --format='%an <%ae>%n%n%B'` back before moving on.
- Never amend a published commit, never amend to hide a failing check, and never fold a second reason into a commit that already has one — that is a new commit, and it is allowed to be small.
### Push

Withheld until the user says push, push it, or send it to origin. A green suite, a clean diff and "looks good" are not permission; silence is a no.

Before, with an upstream: `git status -sb` names the branch and its tracking state; `git rev-list --count @{u}..HEAD` counts what would leave the machine; `git fetch` is read-only and allowed, a pull or a rebase is not.
After: the ref, the old and new hashes, and `origin/<branch>` verified at the same hash.

One push per instruction. A push whose purpose is to make CI green is not allowed — run the same checks locally, report what fails.
### Attribution

The author is the project's configured identity. The agent is never the author, the co-author, or a trailer.

Before every commit, read the message back and refuse these:

```bash
git log -1 --format='%B' | grep -nEi 'co-authored-by|generated with|claude|chatgpt|gpt-|copilot|cursor|noreply@anthropic'
```
A hit is removed with `git commit --amend` before the commit is reported — no exception for a harness default, a template, or a hook. `git log -1 --format='%an <%ae>'` names the user's identity; if it names the tool, the commit is rewritten, not explained.
### Prove

| Claim | Command | Expected |
| --- | --- | --- |
| no whitespace damage | `git diff --check` | empty |
| the staged set is the intended set | `git status --short` | one group's paths |
| the message holds | `git log -1 --format=%B` | subject, blank, why, no trailer |
| unpublished | `git rev-list --count @{u}..HEAD` | the number, stated |
| the push landed | `git rev-parse HEAD origin/<branch>` | two identical hashes |
### Recovery

- A local commit that should not exist: `git reset --soft HEAD~1`, only on the user's word, files still staged.
- Already pushed and wrong: a revert commit, never a force-push. Ask before touching history at all.
- Push rejected because the remote moved: report it and wait. A pull or rebase is the user's call, not a reflex.
- A hook refused the commit: fix the cause. `--no-verify` needs the user's explicit word and is recorded.
### Never

- Commit or push because the work "felt done".
- One commit per file edit, or one commit for two unrelated changes.
- Push without the words, or push twice for one instruction.
- `--force`, `--no-verify`, or a rewrite of published history.
- A branch created or switched without being asked.
- An agent in `%an`, `%ae`, or any trailer.
- Secrets, `.env`, build output, dependencies, editor state.
- A message that describes the diff instead of the reason.

<!-- source: merged from orchestration.md, research.md and git.md — their sources: delegate_task, task_status, task_cancel, prototype-review discipline · vendor documentation, GitHub code search, the session's search and image tool contracts · kernel commit conventions (Linus Torvalds), Conventional Commits critique, agent-produced git attribution trailers -->
