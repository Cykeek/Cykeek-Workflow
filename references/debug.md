# Debug

Read when something is broken and the cause is not yet named: a failing test, a crash, a wrong result, a UI that misbehaves. The law is one line — **no fix before the cause is named** — because a symptom fix is a second bug waiting for its own report.

## Phase 1 — Reproduce

Find the smallest command, input or URL that fails, with its exact output, and run it twice. A failure that cannot be reproduced is not yet a bug: say so and ask for the case that fails, rather than reasoning about code that has not failed.

## Phase 2 — Locate

Read the failing code before its neighbours. Bisect one variable per run: by input (half the data), by commit (`git bisect` when the failure predates this change), by layer (stub the boundary and see which side lies). Never change two things in one run — the result proves nothing about either.

## Phase 3 — Explain

One sentence naming the mechanism, with a file and a line: "the listener registers twice because StrictMode mounts the effect and nothing cleans it up (`useFeed.ts:42`)". If the sentence contains "somehow" or "probably", the phase is not done — go back to Locate.

## Phase 4 — Fix and prove

The smallest change at the cause. Then the failing case passes **and** its neighbours still pass — the same command, quoted. When the cause sits in shared code, grep every caller first. Delete the code that existed only to work around the symptom.

## When no cause is found

After the budget — three rounds — stop. Report what was ruled out, what remains untested, and the one observation that would decide it, then ask. Do not harden the symptom, do not add a retry to silence it, and do not ship a guess wrapped in a confident sentence.

## Four rules that hold in every phase

- **Condition-based waiting.** Never sleep and hope: wait for the condition, with a timeout that names what never arrived.
- **Defence in depth is not a fix.** A guard at the boundary is worth adding beside the cause fix, never instead of it.
- **Trace, do not guess.** One log line at the boundary beats three hypotheses — and it comes back out before the commit.
- **Symptoms name the phase.** A crash is usually Locate work; a wrong number is usually Explain work. Match the phase to the symptom before spending a round.

<!-- source: obra/superpowers (MIT) — systematic-debugging, root-cause-tracing, defense-in-depth, condition-based-waiting -->
