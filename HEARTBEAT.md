# HEARTBEAT.md

## Status

`SKILL.md` 502 lines · 17 references · `README.md` 261 · `evals/evals.json` 290 lines, 66 cases · install copy synced to `~/.agents/skills/cykeek/`.
Last change: a shared motion reference now has a protocol — `references/motion-clone.md` sets a fidelity level from the user's words, keeps reference and clone on the same capture, reads start, duration, easing and stagger from frames, writes one row per element per property, and passes only on numbers (80 ms timing, 40 ms stagger, 2 % mean pixels when exact), stopping at five rounds with the failing row; `motion.md` hands over, `SKILL.md` routes it, and four evals cover it. Before that: the git contract, then the design gate and card, the scroll pipeline.

## Pending

| Observed | Target | Status |
| --- | --- | --- |
| `.gitignore` still lists `skills/` | `.gitignore` | decide at publish — keep as a guard or drop |

## Promoted

Counts in prose are checked against files before they are written (§7.2). Intake asks before it writes; the brief precedes any file. Both live in `SKILL.md`, not here.
