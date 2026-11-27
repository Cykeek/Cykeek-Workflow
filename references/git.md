# Git — the commit contract

The repository is the user's, not the agent's. Writing files and publishing history are two acts with two permissions: a commit is local and expected at the close of a task; a push is the user's word, not a conclusion the agent draws.

## The gate

Five answers, one line each, before any git write. A commit without them is a FAIL, however clean the diff.

1. **What changed** — the files, and the one behaviour that moved. Not "several fixes".
2. **One commit or several** — by logical change, never by file count; the split is named here.
3. **New or amend** — has the last local commit already taken this change on?
4. **Local or published** — is the target commit on the remote? Published history is not rewritten.
5. **What proves it** — the diff, the check, and the message read back after the commit.

## The pipeline

Five steps, in this order, every time.

| Step | Act | Stop if |
| --- | --- | --- |
| Status | `git status --short` — every path accounted for | a file changed that no rule names |
| Group | one group per logical change, ordered by dependency | a group exists only to make the diff smaller |
| Stage | `git add <paths>`, explicit, never `-A` blind | a secret, `.env`, build output or dependency folder appears |
| Message | subject, blank line, why | the body needs "also" |
| Commit | `git diff --check`, then commit or amend | the check reports whitespace damage |

Stage the contract files and the evidence that closed the task: `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md`, `.cykeek/**`. Never stage `node_modules`, build output, logs, editor state, or anything the `.gitignore` already refuses — a file forced in with `-f` is a decision, not a convenience.

## The message

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

## Amend

The unit is the logical change, not the file. A follow-up edit to files the last local commit already owns amends that commit: a second request against the same stylesheet is not a second commit.

- `git commit --amend`, then rewrite the message to describe everything the commit now contains. `--no-edit` only while the message is still true.
- Read `git log -1 --format='%an <%ae>%n%n%B'` back before moving on.
- Never amend a published commit, never amend to hide a failing check, and never fold a second reason into a commit that already has one — that is a new commit, and it is allowed to be small.

## Push

Withheld until the user says push, push it, or send it to origin. A green suite, a clean diff and "looks good" are not permission; silence is a no.

Before, with an upstream: `git status -sb` names the branch and its tracking state; `git rev-list --count @{u}..HEAD` counts what would leave the machine; `git fetch` is read-only and allowed, a pull or a rebase is not.
After: the ref, the old and new hashes, and `origin/<branch>` verified at the same hash.

One push per instruction. A push whose purpose is to make CI green is not allowed — run the same checks locally, report what fails.

## Attribution

The author is the project's configured identity. The agent is never the author, the co-author, or a trailer.

Before every commit, read the message back and refuse these:

```bash
git log -1 --format='%B' | grep -nEi 'co-authored-by|generated with|claude|chatgpt|gpt-|copilot|cursor|noreply@anthropic'
```

A hit is removed with `git commit --amend` before the commit is reported — no exception for a harness default, a template, or a hook. `git log -1 --format='%an <%ae>'` names the user's identity; if it names the tool, the commit is rewritten, not explained.

## Prove

| Claim | Command | Expected |
| --- | --- | --- |
| no whitespace damage | `git diff --check` | empty |
| the staged set is the intended set | `git status --short` | one group's paths |
| the message holds | `git log -1 --format=%B` | subject, blank, why, no trailer |
| unpublished | `git rev-list --count @{u}..HEAD` | the number, stated |
| the push landed | `git rev-parse HEAD origin/<branch>` | two identical hashes |

## Recovery

- A local commit that should not exist: `git reset --soft HEAD~1`, only on the user's word, files still staged.
- Already pushed and wrong: a revert commit, never a force-push. Ask before touching history at all.
- Push rejected because the remote moved: report it and wait. A pull or rebase is the user's call, not a reflex.
- A hook refused the commit: fix the cause. `--no-verify` needs the user's explicit word and is recorded.

## Never

- Commit or push because the work "felt done".
- One commit per file edit, or one commit for two unrelated changes.
- Push without the words, or push twice for one instruction.
- `--force`, `--no-verify`, or a rewrite of published history.
- A branch created or switched without being asked.
- An agent in `%an`, `%ae`, or any trailer.
- Secrets, `.env`, build output, dependencies, editor state.
- A message that describes the diff instead of the reason.

<!-- source: kernel commit conventions (Linus Torvalds), Conventional Commits critique, agent-produced git attribution trailers -->