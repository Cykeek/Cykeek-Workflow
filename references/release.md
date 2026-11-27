# Release

Read when the work will face real traffic, real failure, or when a test suite has grown beyond what it guards.

## Design for failure

- Assume every dependency fails slowly, partially, and for longer than expected: queues, caches, callers, timeouts, bad data.
- Bound every outbound call, wait, retry, queue, pool, cache, log, result set and payload. Unbounded is the default if you do not decide it.
- Retry only when it is safe, bounded, backed off and jittered. Never retry a permanent failure, and never stack retries into a storm.
- Isolate with circuit breakers, bulkheads, fast failure, separate pools and degraded modes. Keep a deterministic cleanup path.
- Validate external input and responses before trusting them; bad data poisons caches, queues and downstream state.
- Make deployment, startup, health, observability, configuration, rollback and restart part of the design.
- When traffic can concentrate through routing, retries or fan-out, add back pressure, shedding or pacing **before** the expensive work.

When you add a remote call, wait, queue, pool, cache, job or large result path, define four things on the spot: its bound, its failure behaviour, its validation, and its saturation signal.

## Prune tests that guard nothing

Keep a test only if it changes when the intended behaviour changes, and fails when that behaviour regresses. If another test already guards the same behaviour, delete the copy.

Delete tests that depend on:

- **The past** — asserting that removed code stays gone. Restoring it would be a decision, not a regression.
- **The shape** — file layout, internal names, call order, private helpers. They fail on refactors that change nothing observable.
- **The framework** — that the ORM maps, the router routes, the library works.
- **Trivia** — getters, constants, config echoes, constructors.

A test that guards no behaviour is not free: it costs runtime, and it teaches the next person to edit assertions until the build turns green.

<!-- source: release-it, prune-tests -->
