# HEARTBEAT.md

## Status

`SKILL.md` 496 lines · 12 references · `README.md` 280 · `evals/evals.json` 107 cases · install copy synced to `~/.agents/skills/cykeek/`.
Last change: an agent in an already-set-up workspace no longer needs to invoke the skill. The generated `AGENTS.md` §1 carries a literal `Reference library:` line naming the installed folder, so every routing row's `references/...` path opens from the project; capability.md records `library: <absolute path>` beside the write path. Eval 107 asserts that a normal task reads `AGENTS.md` alone, that the path exists, and that a dead reference is a FAIL. AGENTS.md §11 names the check. Before that: the four setup artifacts are local by design — one marked `.gitignore` block names `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md` and `.cykeek/`, the git gate refuses to stage them, and `README.md` stays outside the decision. The proof is nine checks, the ninth being locality.
Before that: the setup proof (nine checks, `.cykeek/verify/workspace.md`), the state pointer and the manifest, and DESIGN provenance with a seeded component index — each in `git log`.

## Pending

| Observed | Target | Status |
| --- | --- | --- |
| `.gitignore` lists `skills/` | `.gitignore` | closed — kept as a guard |
| install copy carries the old staging rule | `~/.agents/skills/cykeek/` | open — the copy needs a write the sandbox denies; one approved run closes it |

## Promoted

Counts in prose are checked against files before they are written (§7.2). Intake asks before it writes; the brief precedes any file. Both live in `SKILL.md`, not here.
