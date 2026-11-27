# Code

**Index** — Gate · Moments · Ladder · Changing code · Debug · React · Release. Read the sections the routing row names, not the file.

## Gate

Five answers, one line each, before the first line of code. A one-line fix gets one line. A gate that runs longer than five lines is a design problem, not diligence.

| Ask | Answer by naming |
| --- | --- |
| What owns this? | the one module, concept and place a future change lands |
| Step or leap? | the smallest move that keeps behaviour intact |
| What did I hide? | what the caller must not need to know: order, storage, transport, representation |
| What am I refusing? | the special case, flag, wrapper or abstraction you will not add |
| What proves it? | the check that fails if the answer is wrong |

An answer you cannot fill means the design is not ready, and the move is to read the code, not to write it. The gate is also the receipt's body (`process.md` § Receipt): the same five answers, cited, before any write. Business logic and data flows live in `domain.md`, behind a gate that keeps them shut on a landing page.

## Moments

Five moments of writing code, then the untangle goal. **Naming and shape** — *when a name needs a comment, a comment explains the flow, or a reader must scroll to follow one function.* One term per concept, everywhere; a name that reveals mechanism instead of meaning is a defect even when it compiles — rename before deeper work. Write for local reasoning: a reader understands the function without loading the file. A comment earns its place by carrying what the code cannot — rationale, a contract, an invariant, an external constraint; a comment that restates the code changes only when the code does, so it has no reason of its own to exist. When the comment grows to justify an awkward interface, fix the interface. Match the surrounding conventions: consistency beats preference, and surprise is complexity even when short.

**Routines** — *when a routine mixes setup, validation, computation and side effects, or one function both answers and mutates.* Small is not the metric; one level of abstraction and a small interface is — a tidy body behind a leaky signature is still shallow. Few, meaningful parameters: a clump of primitives carrying one concept becomes a type, a boolean flag is two functions wearing one name. Guard clauses flatten nesting; name the predicate instead of explaining it in a comment. Separate commands from queries — a hidden side effect in a function that "just returns" is the expensive bug. Keep the happy path readable; assert programmer assumptions and make invalid states unrepresentable rather than defended at every call site. Handle an error once, at the abstraction that can act on it, with its diagnostic context intact; retries, sleeps and swallowed exceptions are requirements the design missed — say which, then fix the design.

**Modules** — *when adding a module, layer, helper, wrapper, facade, option, argument or pattern — each one has to earn its name.* Prefer deep modules: a small semantic interface over real internal complexity; an interface that changes whenever its internals do is a pass-through, and the caller pays for it. Design the interface around what callers must know, never around how it works — setup order, modes, flags and exposed containers are complexity pushed outward. Pull complexity downward: a harder implementation behind a simpler contract is usually the better trade. Hide volatile decisions — storage shape, protocol, file format, vendor, performance trick — inside the module that owns the knowledge. Prove the addition hides more than it adds; if you cannot, delete it. Keep special behaviour in one special place, not threaded as branches through the common path. Dependencies point inward: domain and use cases never import frameworks, database, transport, UI or vendor types; adapters translate at the edge. Organise by use case or capability — `utils`, `common` and `core` are where coupling hides.

**Knowledge** — *when the same fact lives in two places, one change crosses several files, or the caller must remember something.* One authoritative source per piece of knowledge — validation, status meanings, calculations, mappings, schemas, config meaning — everything else derives from it; DRY is about knowledge, not about text that looks alike. Orthogonality: components change for their own reasons or they are one component; overlapping responsibilities are why a small change needs three edits. A typical change lands in one place; when it does not, the layout is wrong, not the task. Rule of Three: tolerate uncertain duplication twice; remove it on the third, when the real shape is known. Keep volatile decisions reversible until evidence justifies committing to a vendor, platform, schema or environment. No speculative generality: extension points and config for things that never vary have no reason to change, so they rot instead of helping. Deleting dead code has a positive return; keeping it means the next reader reads it as intent.

**Judgment** — *when the request sounds like an implementation detail, or the fix would multiply future cost.* Own the outcome: surface the tradeoff and the risk instead of blaming the tool, the framework default or the deadline. Restate the durable need before building the proposed solution. A quick fix that raises future maintenance cost is a bad bargain — leave untouched code alone and touched code better. Automate the repeated, the error-prone and the easily forgotten, and version the script. Measure before optimising. Understand a tool, a generated file or a formal method before relying on it.

**Untangle** (this section) — *when "the codebase is a mess."* A goal, not a task — run it to the end. 1 **Ask** what else is in scope besides structure — comments that restate code, tests that guard nothing — and do only what was chosen. 2 **List every problem, change nothing.** For each piece ask: should these change together? There are three failures. A **hidden link**: a copy, a global set by another module, a call order callers must know. A **needless link**: a caller reaching into internals, stable code importing volatile code, a shallow module. **Surplus**: a wrapper that only forwards, config for something that never varies, dead code. Add every **special case** — an `if` for one value, a parameter for one caller, a swallowed exception, a retry or sleep, a disabled test. 3 **Redesign from first principles**, as if today's requirements were known from the start; look for the one fix behind several problems — it rarely resembles them. 4 **Fix the list.** A structural fix removes special cases on the way. Do not go bug-hunting separately. 5 **Re-check with subagents** until nothing worth fixing is left. Keep a decision log — problems first, each fix placed above the problems it solves — because compaction loses reasons, not code. Script a skeleton with the bodies removed and a dependency graph, so the whole system fits in one view. Done means: the fewest special cases remain, a typical change lands in one place, nothing imports something more volatile than itself, and intended behaviour is unchanged.

## Ladder

**When:** a change is being written, not read — the fix, the feature, the script, or the dependency choice. Read the code the change touches first. Then take the first rung that fully works:

1. **Does it need to exist?** Drop features, options and flexibility nobody asked for, and name them in one line. A vague request gets the smallest version that does the core job.
2. **Already here?** A helper, component, service or pattern in this codebase, used the way the surrounding code uses it.
3. **Standard library or platform feature?** Use it, unless the project has its own — a house component beats a native widget.
4. **An installed dependency?** Use it. Never add one for a few lines.
5. **One line a reader gets at a glance?** Then one line.
6. **Otherwise:** the minimum code that works.

Scope is every place the change must reach — callers, tests, fixtures, config, exports — and what it can break for a user: data it destroys or exposes, callers that stop working. Extra features are not scope; an unfinished caller is not lazy either, so the smallest complete change still finishes what it touches.

- The shortest working diff wins once you know everything it touches. A one-liner that needs decoding is not short.
- Deletion beats addition, and the structure already there stays: its layers, interfaces and conventions.
- Moved or merged code keeps its validation and error handling. Between two options of equal size, take the one that is correct on the edges.
- Bug fix: fix the root cause once in the shared code — the caller scan and the workaround deletion live in § Debug.
- Logic with a branch, a loop, a parser, money or security gets its check written first, watched failing, then the smallest code that passes; code written before its test is deleted. Trivial changes need none.
- A shortcut with a known limit is marked `shortcut: <the limit>, <when to upgrade>`, so "later" has a trigger.

Never cut to be lazy: validation at trust boundaries (assert programmer assumptions there), error handling that prevents data loss, security, accessibility, what the hardware needs calibrated, anything the user asked for.

Every reply ends with one or two lines: what you skipped or did not check, and the risk that leaves.

## Changing code

**When:** cleanup starts, a smell is named, or a rewrite is tempting. Refactoring preserves observable behaviour; feature work, migration and cleanup are separate steps — a patch that mixes them cannot be reviewed or reverted. Small, buildable, reviewable steps: a step that cannot be reasoned about locally is too big. Get a safety net first — a test, a characterization, a capture — or write the verification gap down before starting. Name the smell, apply the smallest treatment, verify, stop; neighbouring smells are not this change. Preferred moves in order: rename, extract, inline, move, split phases, encapsulate mutation, decompose conditionals, remove duplication — a pattern comes after the simple move fails. Stop when the named smell is gone and further cleanup would be speculative; write the remaining smell down instead of hunting it.

| Smell | Move |
| --- | --- |
| long routine, large class, long parameter list, primitive obsession | extract, split phase, parameter object, value object |
| switch on type, repeated type codes | isolate the decision, then polymorphism or a table |
| one change edits many files | move behaviour and data to the owner of the change |
| dead code, a wrapper that only forwards, an extension point nothing varies | delete or inline |
| train-wreck navigation, private knowledge, a middle man | hide the delegate, move behaviour, remove the middle man |

## Debug

**When:** something is broken and the cause is not yet named — a failing test, a crash, a wrong result, a UI that misbehaves. The law is one line — **no fix before the cause is named** — because a symptom fix is a second bug waiting for its own report. The order is reproduce, locate, explain, fix, verify.

1. **Reproduce.** Find the smallest command, input or URL that fails, with its exact output, and run it twice. A failure that cannot be reproduced is not yet a bug: say so and ask for the case that fails, rather than reasoning about code that has not failed.
2. **Locate.** Read the failing code before its neighbours. Bisect one variable per run: by input (half the data), by commit (`git bisect` when the failure predates this change), by layer (stub the boundary and see which side lies). Never change two things in one run — the result proves nothing about either.
3. **Explain.** One sentence naming the mechanism, with a file and a line: "the listener registers twice because StrictMode mounts the effect and nothing cleans it up (`useFeed.ts:42`)". If the sentence contains "somehow" or "probably", the phase is not done — go back to Locate.
4. **Fix and prove.** The smallest change at the cause. Then the failing case passes **and** its neighbours still pass — the same command, quoted. When the cause sits in shared code, grep every caller first, fix the root cause once in the shared code, and delete the code that existed only to work around the symptom.

**When no cause is found.** After the budget — three rounds — stop. Report what was ruled out, what remains untested, and the one observation that would decide it, then ask. Do not harden the symptom, do not add a retry to silence it, and do not ship a guess wrapped in a confident sentence.

Four rules that hold in every phase: **condition-based waiting** — never sleep and hope: wait for the condition, with a timeout that names what never arrived. **Defence in depth is not a fix** — a guard at the boundary is worth adding beside the cause fix, never instead of it. **Trace, do not guess** — one log line at the boundary beats three hypotheses, and it comes back out before the commit. **Symptoms name the phase** — a crash is usually Locate work; a wrong number is usually Explain work; match the phase to the symptom before spending a round.

## React

**When:** writing, reviewing or refactoring React or Next.js for performance. Ordered by impact — waterfalls and bundle size beat micro-optimisations every time.

**Waterfalls (biggest win).** Independent awaits go in `Promise.all` — three sequential fetches are three round trips. Defer `await` into the branch that needs it; check a cheap synchronous condition before an async flag. Start independent work immediately and await later, especially in route handlers and server actions. Server components render sequentially: parallelise by composing, not by awaiting siblings in the parent. Split slow data behind a `<Suspense>` boundary so the shell paints immediately.

**Bundle.** Import from source, not barrel files: icon and component libraries can cost 200–800ms per cold start through re-exports; in Next.js 13.5+ `optimizePackageImports` handles it — elsewhere, deep imports. Lazy-load heavy components (`next/dynamic`, `ssr: false` for client-only) and defer analytics, logging and error tracking until after hydration. Keep import paths statically analysable — a path built from a variable forces the bundler to include everything. Preload on intent — hover or focus — when the bundle is expensive.

**Server.** Authenticate and authorise **inside** every server action; they are public endpoints — never rely on middleware or layout guards alone. Pass only the fields the client uses across the RSC boundary; serialisation is page weight; derive in the client rather than sending both raw and transformed copies. No mutable module-scope state for request data — server renders run concurrently and it leaks between requests. `React.cache()` deduplicates per request; an LRU cache deduplicates across requests. Hoist static I/O — fonts, templates, config — to module scope so it loads once, not per request. `after()` for logging, analytics and cleanup, so the response is not blocked.

**Client and re-renders.** Derive state during render; never mirror props into state or set state in an effect for a derived value. Never define a component inside a component — it remounts on every render and destroys focus and state. Functional `setState` updates avoid stale closures and unstable callbacks. Lazy-init expensive state with a function. Narrow effect dependencies to primitives; subscribe to a derived boolean, not a continuously changing number. Put interaction logic in the event handler, not in state plus effect. Split a hook or effect that combines independent concerns so each recomputes alone. `useTransition` for non-urgent updates; `useDeferredValue` for expensive derived renders. Do not wrap a one-line primitive computation in `useMemo` — the hook costs more than the expression. Passive listeners for touch and wheel; one shared listener per event, not one per component.

**Rendering.** Animate the SVG **wrapper**, not the `<svg>` itself. `content-visibility: auto` for long lists; virtualise past ~100 rows. Hoist static JSX out of the render path. Stable keys, explicit image dimensions, `defer`/`async` on scripts.

**JavaScript.** Avoid layout thrashing: batch reads, then writes. `Set`/`Map` for repeated lookups; index maps instead of repeated `find`; one pass instead of chained iterations; `toSorted()` over mutating `sort()`; hoist `RegExp` construction.

**Before committing.** Lint, accessibility, bundle size and architecture, in that order — then a regression check on what changed. If the project has its own check script, run it; never install a toolchain just to verify.

## Release

**When:** the work will face real traffic, real failure, or when a test suite has grown beyond what it guards.

**Design for failure.** Assume every dependency fails slowly, partially, and for longer than expected: queues, caches, callers, timeouts, bad data. Bound every outbound call, wait, retry, queue, pool, cache, log, result set and payload — unbounded is the default if you do not decide it. Retry only when it is safe, bounded, backed off and jittered; never retry a permanent failure, and never stack retries into a storm. Isolate with circuit breakers, bulkheads, fast failure, separate pools and degraded modes, and keep a deterministic cleanup path. Validate external input and responses before trusting them; bad data poisons caches, queues and downstream state. Make deployment, startup, health, observability, configuration, rollback and restart part of the design. When traffic can concentrate through routing, retries or fan-out, add back pressure, shedding or pacing **before** the expensive work. When you add a remote call, wait, queue, pool, cache, job or large result path, define four things on the spot: its bound, its failure behaviour, its validation, and its saturation signal.

**Prune tests that guard nothing.** Keep a test only if it changes when the intended behaviour changes, and fails when that behaviour regresses; if another test already guards the same behaviour, delete the copy. Delete tests that depend on **the past** — asserting that removed code stays gone; restoring it would be a decision, not a regression. **The shape** — file layout, internal names, call order, private helpers; they fail on refactors that change nothing observable. **The framework** — that the ORM maps, the router routes, the library works. **Trivia** — getters, constants, config echoes, constructors. A test that guards no behaviour is not free: it costs runtime, and it teaches the next person to edit assertions until the build turns green.

<!-- source: untangle-codebase, the-pragmatic-programmer, refactoring, refactoring-guru, clean-code, code-complete, a-philosophy-of-software-design, clean-architecture, working-effectively-with-legacy-code; obra/superpowers (MIT) — systematic-debugging, root-cause-tracing, defense-in-depth, condition-based-waiting; vercel-react-best-practices, react-doctor; release-it, prune-tests -->
