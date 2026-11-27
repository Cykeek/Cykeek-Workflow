# HEARTBEAT.md

## Status

`SKILL.md` 496 lines · 12 references · `README.md` 280 · `evals/evals.json` 106 cases · install copy **not synced** — 18 files sit at `~/.agents/skills/cykeek/` still carrying the old staging rule.
Last change: the four setup artifacts are local by design — one marked `.gitignore` block names `AGENTS.md`, `DESIGN.md`, `HEARTBEAT.md` and `.cykeek/`, the git gate refuses to stage them, and `README.md` stays outside the decision. The proof is nine checks, the ninth being locality.
Before that: the setup proof (nine checks, `.cykeek/verify/workspace.md`), the state pointer and the manifest, and DESIGN provenance with a seeded component index — each in `git log`.

## Pending

| Observed | Target | Status |
| --- | --- | --- |
| `.gitignore` lists `skills/` | `.gitignore` | closed — kept as a guard |
| install copy carries the old staging rule | `~/.agents/skills/cykeek/` | open — the copy needs a write the sandbox denies; one approved run closes it |

## Promoted

Counts in prose are checked against files before they are written (§7.2). Intake asks before it writes; the brief precedes any file. Both live in `SKILL.md`, not here.
