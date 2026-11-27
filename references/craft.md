# Craft

Read when the work is code rather than interface: writing it, changing it, untangling it, or deciding where a boundary belongs. Nothing here is a style preference — each rule exists to make the next change cheap. Nine books agree on this file; the disagreements are ceremony, and ceremony is what the `When:` lines refuse. The four that need business logic or data flows to say anything — the DDD pair, the enterprise patterns, the data-intensive one — are in `domain.md`, behind a gate that keeps them shut on a landing page.

## The gate

Five answers, one line each, before the first line of code. A one-line fix gets one line. A gate that runs longer than five lines is a design problem, not diligence.

| Ask | Answer by naming |
| --- | --- |
| What owns this? | the one module, concept and place a future change lands |
| Step or leap? | the smallest move that keeps behaviour intact |
| What did I hide? | what the caller must not need to know: order, storage, transport, representation |
| What am I refusing? | the special case, flag, wrapper or abstraction you will not add |
| What proves it? | the check that fails if the answer is wrong |

An answer you cannot fill means the design is not ready, and the move is to read the code, not to write it. The gate is also the receipt's body (`loop.md` §0): the same four answers, cited, before any write.

## The ladder

**When:** a change is being written, not read — the fix, the feature, the script, or the dependency choice.

Read the code the change touches first. Then take the first rung that fully works:

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
- Bug fix: grep every caller first, then fix the root cause once in the shared code.
- Logic with a branch, a loop, a parser, money or security leaves one small test or an assert-based self-check. Trivial changes need none.
- A shortcut with a known limit is marked `shortcut: <the limit>, <when to upgrade>`, so "later" has a trigger.

Never cut to be lazy: validation at trust boundaries, error handling that prevents data loss, security, accessibility, what the hardware needs calibrated, anything the user asked for.

Every reply ends with one or two lines: what you skipped or did not check, and the risk that leaves.
## Naming and shape

**When:** a name needs a comment, a comment explains the flow, or a reader must scroll to follow one function.

- One term per concept, everywhere. A name that reveals mechanism instead of meaning is a defect even when it compiles — rename before deeper work.
- Write for local reasoning: a reader understands the function without loading the file.
- A comment earns its place by carrying what the code cannot — rationale, a contract, an invariant, an external constraint. A comment that restates the code changes only when the code does, so it has no reason of its own to exist.
- When the comment grows to justify an awkward interface, fix the interface.
- Match the surrounding conventions. Consistency beats preference, and surprise is complexity even when short.

## Routines

**When:** a routine mixes setup, validation, computation and side effects, or one function both answers and mutates.

- Small is not the metric; one level of abstraction and a small interface is. A tidy body behind a leaky signature is still shallow.
- Few, meaningful parameters. A clump of primitives carrying one concept becomes a type; a boolean flag is two functions wearing one name.
- Guard clauses flatten nesting; name the predicate instead of explaining it in a comment.
- Separate commands from queries. A hidden side effect in a function that “just returns” is the expensive bug.
- Keep the happy path readable. Validate at trust boundaries, assert programmer assumptions, and make invalid states unrepresentable rather than defended at every call site.
- Handle an error once, at the abstraction that can act on it, with its diagnostic context intact. Retries, sleeps and swallowed exceptions are requirements the design missed — say which, then fix the design.

## Modules

**When:** adding a module, layer, helper, wrapper, facade, option, argument or pattern — each one has to earn its name.

- Prefer deep modules: a small semantic interface over real internal complexity. An interface that changes whenever its internals do is a pass-through, and the caller pays for it.
- Design the interface around what callers must know, never around how it works. Setup order, modes, flags and exposed containers are complexity pushed outward.
- Pull complexity downward: a harder implementation behind a simpler contract is usually the better trade.
- Hide volatile decisions — storage shape, protocol, file format, vendor, performance trick — inside the module that owns the knowledge.
- Prove the addition hides more than it adds. If you cannot, delete it.
- Keep special behaviour in one special place, not threaded as branches through the common path.
- Dependencies point inward: domain and use cases never import frameworks, database, transport, UI or vendor types; adapters translate at the edge.
- Organise by use case or capability. `utils`, `common` and `core` are where coupling hides.

## Knowledge

**When:** the same fact lives in two places, one change crosses several files, or the caller must remember something.

- One authoritative source per piece of knowledge: validation, status meanings, calculations, mappings, schemas, config meaning. Everything else derives from it. DRY is about knowledge, not about text that looks alike.
- Orthogonality: components change for their own reasons or they are one component. Overlapping responsibilities are why a small change needs three edits.
- A typical change lands in one place. When it does not, the layout is wrong, not the task.
- Rule of Three: tolerate uncertain duplication twice; remove it on the third, when the real shape is known.
- Keep volatile decisions reversible until evidence justifies committing to a vendor, platform, schema or environment.
- No speculative generality. Extension points and config for things that never vary have no reason to change, so they rot instead of helping.
- Deleting dead code has a positive return; keeping it means the next reader reads it as intent.

## Changing code

**When:** cleanup starts, a smell is named, or a rewrite is tempting.

- Refactoring preserves observable behaviour. Feature work, migration and cleanup are separate steps — a patch that mixes them cannot be reviewed or reverted.
- Small, buildable, reviewable steps. A step that cannot be reasoned about locally is too big.
- Get a safety net first — a test, a characterization, a capture — or write the verification gap down before starting.
- Name the smell, apply the smallest treatment, verify, stop. Neighbouring smells are not this change.
- Preferred moves in order: rename, extract, inline, move, split phases, encapsulate mutation, decompose conditionals, remove duplication. A pattern comes after the simple move fails.
- Stop when the named smell is gone and further cleanup would be speculative. Write the remaining smell down instead of hunting it.

| Smell | Move |
| --- | --- |
| long routine, large class, long parameter list, primitive obsession | extract, split phase, parameter object, value object |
| switch on type, repeated type codes | isolate the decision, then polymorphism or a table |
| one change edits many files | move behaviour and data to the owner of the change |
| dead code, a wrapper that only forwards, an extension point nothing varies | delete or inline |
| train-wreck navigation, private knowledge, a middle man | hide the delegate, move behaviour, remove the middle man |

## Untangle

**When:** “the codebase is a mess.” A goal, not a task — run it to the end.

1. **Ask** what else is in scope besides structure — comments that restate code, tests that guard nothing — and do only what was chosen.
2. **List every problem, change nothing.** For each piece ask: should these change together? There are three failures. A **hidden link**: a copy, a global set by another module, a call order callers must know. A **needless link**: a caller reaching into internals, stable code importing volatile code, a shallow module. **Surplus**: a wrapper that only forwards, config for something that never varies, dead code. Add every **special case** — an `if` for one value, a parameter for one caller, a swallowed exception, a retry or sleep, a disabled test.
3. **Redesign from first principles**, as if today's requirements were known from the start. Look for the one fix behind several problems; it rarely resembles them.
4. **Fix the list.** A structural fix removes special cases on the way. Do not go bug-hunting separately.
5. **Re-check with subagents** until nothing worth fixing is left.

Keep a decision log — problems first, each fix placed above the problems it solves — because compaction loses reasons, not code. Script a skeleton with the bodies removed and a dependency graph, so the whole system fits in one view. Done means the fewest special cases remain, a typical change lands in one place, nothing imports something more volatile than itself, and intended behaviour is unchanged.

## Judgment

**When:** the request sounds like an implementation detail, or the fix would multiply future cost.

- Own the outcome. Surface the tradeoff and the risk instead of blaming the tool, the framework default or the deadline.
- Restate the durable need before building the proposed solution.
- A quick fix that raises future maintenance cost is a bad bargain. Leave untouched code alone and touched code better.
- Automate the repeated, the error-prone and the easily forgotten, and version the script.
- Debug from reproduced facts: observe, isolate, explain, fix, verify. Measure before optimising.
- Understand a tool, a generated file or a formal method before relying on it.

<!-- source: untangle-codebase, the-pragmatic-programmer, refactoring, refactoring-guru, clean-code, code-complete, a-philosophy-of-software-design, clean-architecture, working-effectively-with-legacy-code -->