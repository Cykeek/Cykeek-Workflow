# Flows

Read when the request is a **problem**, not an instruction. "This feels slow", "I don't know what it should look like", "this codebase is a mess", "why do you keep getting this wrong" — each names a different kind of work, and naming the right flow before touching code is most of the answer.

Read the problem statement once and classify it in one line. Then open the flow and run it. One flow per request; if the request really spans two, say which runs first and why.

| What they said | The flow | What it ends with |
| --- | --- | --- |
| "I don't know what it should look like" | Prototype | variants behind a picker, their choice |
| "Build X" | Build — `loop.md` moves 2 to 5 | the thing, verified |
| "It feels slow / too much motion" | Subtract — `motion-audit.md` | a findings list, then removals |
| "Does it hold up?" | Break | what broke, with the fix |
| "The codebase is a mess" | Untangle | a decision log, then one structural pass |
| "Where should motion go?" | Hunt — `motion-audit.md` | a proposal list, no code |
| "Which library?" | Pick — `libraries.md` | one dependency and the reason |
| "How do they do X?" | Research — `research.md` | a nine-slot card, then an integration plan |
| "You keep getting this wrong" | Capture | a rule, or better, a check |

## Prototype — when the look is the open question

Divergence, not iteration on one idea. Three tints of the same card waste the comparison; each variant must be a direction you could defend shipping alone. The gate is shared before divergence starts: every direction answers `design.md` § The gate — job, one action, hierarchy, states — and variants may differ on the axis, never on those. A page whose timeline is the wheel adds `scroll.md` § The score to that frame.

1. **Scope to one piece.** "The dashboard" is a project; a stat card is a run. Name the piece, say why it is the highest-leverage one, offer the rest as follow-ups.
2. **Recon.** Stack, tokens, personality (consumer or tool), the context it renders in. Variants use the project's tokens and still feel native — sharing tokens is not convergence.
3. **Name the directions.** Default 3, never more than 5. Each has a name and a distinct axis: layout, density, personality, motion, interaction model. If two would differ only in accent colour, they are one direction — replace one.
4. **Build in isolation.** `/prototypes/<slug>` in a project, or one self-contained HTML file with no project. Nothing imports from the surface. Copy `assets/picker.html` verbatim; it is chrome and is never restyled with project tokens. One variant at a time, full size, in realistic surroundings — a toast needs a page behind it. Switching is instant: it happens a hundred times a session, so the frequency rule gives it no animation.
5. **Verify, then stop.** Every variant renders, every interaction responds, console clean. Present the table and hand the decision over:

| Variant | Axis | When it wins | What it costs |
| --- | --- | --- | --- |

6. **Promote on choice.** Integrate the pick on the project's conventions, then delete the prototype surface. A riff request keeps the harness and diverges around the chosen direction.

Never pre-pick a favourite in the table. If asked which I would choose, answer from the product's personality and how often the piece is seen, not from taste.

## Break — when the question is durability

Feed the component data it was not designed for, behind a "Demo data / Worst case" toggle: the longest plausible name, an unbreakable email, a one-letter name, an empty list, five thousand rows, zero, a negative, a long label, non-Latin text, emoji, numbers with many digits. Report every break with its fix. Do not fix silently — the list is the deliverable.

## Untangle — when the code is the problem

One question drives the whole pass: **should these pieces change together?** Everything wrong is one of three shapes, plus special cases:

- **Hidden links** — must change together, and the code does not say so: copied logic, a module reading a global set elsewhere, one feature split across folders by layer, an order callers must know.
- **Needless links** — tied, but they change for different reasons: callers reaching into internals, stable code importing volatile code, a general function with a parameter only one caller understands, a shallow module.
- **Surplus** — no reason of its own: pass-through wrappers, config for things that never vary, dead code.
- **Special cases** — an `if` for one value, a parameter for one caller, a swallowed exception, a retry or sleep, a disabled test. Each is a requirement the design never accounted for; together they say what the system needs to be.

List every problem and change nothing. Then design from first principles and look for the one fix behind several problems — it usually looks nothing like them. Write the decision log: fix above the problems it solves. Fix the list, not the special cases one by one. Comments and tests are opt-in parts of the pass, and tests go last because they are the safety net.

## Capture — when the lesson is about us

Corrections are the only reliable source of taste. Find them all, not just the spoken ones: an explicit correction, a silent edit to a value I wrote, a repeated nudge (record where it landed, not the steps), a revert or a rejected variant, a confirmed non-obvious choice.

Then filter: would a fresh agent make this mistake on a *different* component? If no, skip it. If a rule already covers it, tighten that rule to a number or a never instead of adding a duplicate. If a violation can be found by matching code, propose a check — a lint rule, a banned token, a test — because a check runs every session and a sentence in a long file gets skipped.

Never invent a why: mark it *inferred* and ask. Propose the rules, write only what is approved, and write for an agent who was not here — name the component class, the token, the value.

<!-- source: ask-emil, prototype, PICKER, untangle-codebase, break-ui, learn-taste -->
