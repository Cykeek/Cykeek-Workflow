# Research

Read when the request names a library, platform API or technique whose current shape you cannot state from memory — or when the user says "make it like that site".

No research tools? Then the repository is the source: imports, lockfile, `node_modules/<pkg>` types. Mark every outside claim UNVERIFIED and name what would settle it. Never fill a gap with a plausible API.

## The rule

One card per topic. Search results never enter the transcript — extract while reading, close the tab, keep 25 lines. A research round that changes no row of the plan was a wasted round.

## 0. The repository answers first

- Grep the package name; read `package.json` and the lockfile for the exact version.
- If installed, `node_modules/<pkg>/README.md` and its `.d.ts` are the version you actually have — better than any doc site.
- Find one existing usage before writing a second: the project's own idiom beats the docs' first example.
- Already used and working? You are reading, not researching.

## 1. Frame — five lines before the first query

1. What it is: library, platform API, pattern or effect. Each has a different authority.
2. The version in play. The wrong major is the single largest source of wrong answers.
3. The claim to settle: "how do I X in Y, with <framework>, on <version>". One question, not a topic.
4. What you need back: a call signature, an integration point, a number or a pattern.
5. What would make you wrong here: SSR, RSC, StrictMode double-mount, cleanup, reduced-motion, mobile.

## 2. Source ladder

Climb only as far as the question requires. Each rung is cheaper to trust than the one below it.

1. **Versioned docs** — the API reference page, not the landing page.
2. **Changelog or migration guide** — the breaking change every tutorial predates.
3. **Official examples** — the project's own demo, compiled and current.
4. **Real code** — GitHub code search over recent, starred projects; issues and discussions for what breaks in production.
5. **Community** — blogs, videos, StackOverflow. Edges only. Never an API signature.

Stop when two independent sources agree on the same call. If rung 4 contradicts rung 1, trust the code, then check the version again: docs describe intent, code describes reality.

## 3. Queries

- Quote the API: `"gsap.context" cleanup react`.
- Pin version and framework: `gsap 3.12 ScrollTrigger next.js app router`.
- Paste error text verbatim, with the library name.
- Ask for the rejection too: `gsap vs motion one performance` — the alternative names the trade-off.

Five queries maximum. Three that add nothing end the web pass; read the installed types instead.

## 4. Images

An image settles three things: what the effect looks like at start, middle and end; the layout it lives in; the spacing and easing it implies. Capture the reference at rest, mid-motion and settled, then write numbers — translate distance, duration feel, stagger — never adjectives.

Not for: colour values you can sample from the real site, or "inspiration".

No image tool: describe the motion from text, mark the visual slot UNVERIFIED, ship the contract anyway.

## 5. What to extract — the nine slots

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

## 6. Real implementations

Read three, not thirty: a shipped site using it, a repo using it in your framework, and its issue tracker filtered by your framework and version.

- Take: composition, config, cleanup, where it mounts, what it is not allowed to touch.
- Reject: tutorials older than the last major, snippets with no teardown, timings copied between projects, anything fighting the framework's own rendering.

Copy the structure, never the pixels — the look belongs to the reference clone protocol, which has its own rules.

## 7. The card

`.cykeek/research/<slug>.md`, 25 lines: the nine slots, then the sources with dates. The next worker, the applier and the orchestrator read the card, not the web. That is the whole token argument — 25 lines bought once, read four times.

## 8. Card to contract

1. Dependency line: exact version, licence, weight, and "already present?" answered.
2. Files touched, in dependency order, one line each.
3. The riskiest unknown, named. If it is the whole feature, spike it first.
4. Rollback: what you delete to undo.
5. Contract rows added (`orchestration.md`) — one per behaviour, assertions included.
6. **Spike before integration** — twenty lines in the real framework, mounted, at 390 and 1440. If the spike fails, the plan is wrong, not the app.

Then build the smallest visible slice.

## 9. Gates

- No dependency without a version pin, a licence and a bundle number.
- No snippet without its teardown.
- No "done" from research alone — the card is a plan, the spike is proof.
- Two rounds, then decide with the unknowns named. More searching is procrastination in a lab coat.

<!-- source: vendor documentation, GitHub code search, the session's search and image tool contracts -->
