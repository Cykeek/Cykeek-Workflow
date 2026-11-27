# Security

Read when the question is "is this safe?" rather than "make it work": an audit, a pen test, a review of auth, sessions, payments, uploads, secrets or dependencies, a vulnerability report, or a change that touches a trust boundary. `security-classes.md` holds what to look at by surface; this file is the protocol both of them run. An audit describes the fix and never writes it — a fix asked for afterwards is its own task, under the normal pipeline.

One rule reduces the file: a finding is a crossed trust boundary with a lower-trust principal and a concrete result, proven from source or a bounded local check. Everything below applies that rule without guessing, without looping and without noise.

## The gate

Five answers, one line each, before the first file is read for hunting. A one-line security question gets one line.

| Ask | Answer by naming |
| --- | --- |
| What is the target? | repository root, source ref, and whether the worktree is dirty |
| Which mode? | `guidance` for a question, a focused review or a triage; `full audit` only when the user asks to audit, pen-test, review end to end, or wants report files |
| Which profile? | `quick`, `standard` or `deep` — standard by default, quick for a re-run or a first look, deep for high stakes |
| Which surfaces? | the paths, subsystems or diff in scope; everything else is `out_of_scope`, never silently dropped |
| What proves it? | a verifier that did not hunt, and the file:line or command that settles each claim |

Ambiguous between the two modes: ask one question before creating a file. Loading this reference never authorizes a full audit.

## Two modes

- **Guidance** — answer in the conversation: the exact line, the condition that would make it real, the fix. No directory, no artifacts, no six phases. Focused workers may run and return to this task.
- **Full audit** — the six phases and the files they leave, entered only on the explicit ask. An unspecified output directory is `~/security-audit-skill/<repo>/run-<N>`, outside the target; inside it only when the user names it and version control ignores the whole directory.

## Three verdicts

| Verdict | Carries | Never |
| --- | --- | --- |
| `confirmed` | complete source trace, conditions, a bounded observed result, severity, confidence, the smallest fix | without a reproduced result, or with a visible preventing layer |
| `needs_validation` | source trace, the exact unresolved blocker, a local or owner-observed plan | a severity, the word vulnerability, or a guess at deployment |
| `rejected` | the trace and the disproof, kept so a later run does not repeat it | a second report of the same claim without changed evidence |

These map onto the three states: `confirmed` is a PASS, `needs_validation` a UNVERIFIED, `rejected` a closed FAIL. One unverified claim is reported as such, never averaged into a clean sweep.

## What a finding needs

Six parts, one sentence each: the lower-trust principal and its starting capability; the accepted value, action or selector; the control that should have rejected or bound it; the exact source path after that decision; the affected principal or resource; the observed or owner-observable result.

Three exclusions, and they are most of the discipline. A missing best practice with no affected principal is a hardening note. A defence-in-depth gap another layer already stops is a hardening note. Self-impact and intended authority are not cross-boundary results. A crash is not code execution, a hang is not shared availability, and a cookie flag is not a finding until a less-trusted origin can use it.

## Severity

| Level | Anchor |
| --- | --- |
| critical | an unauthenticated actor gets code execution, the whole data store, or any account takeover |
| high | an explicit control fully defeated with real consequences: auth bypass, cross-tenant read or write, stored script for other users, authenticated code execution, a remote stop of a shared service |
| medium | a real boundary violation with a narrow blast radius or uncommon preconditions |
| low | non-secret internals disclosed, or sustained effort for minimal gain |
| informational | confirmed, minimal impact, useful as a prerequisite inside a larger finding |

Severity is likelihood × impact and never exceeds demonstrated impact. The high/medium discriminator is whether the result fully defeats the control or only weakens it; if the concrete damage cannot be stated, the level is lower than it feels. Only `confirmed` records carry a severity.

## The six phases

1. **Map** — `architecture.md`, about 1,000 words: product and principals, protected resources, stack and deployment paths, entry surfaces with their sink paths, trust boundaries with the strongest source-visible control on each, starting paths, prior gaps, and the companion classes selected by boundary — never because a language or package name appears. A comparable product calibrates effort and never dismisses a finding. Read prior ledgers first: same-source confirmations are re-verified, and changed source, `needs_validation`, `deferred` and `blocked` are current work, not coverage.
2. **Coverage** — `coverage-ledger.json`: one unit per surface × boundary × subsystem × attack class, plus lifecycle when it matters. IDs are canonical — percent-encoded refs joined by `::`, sorted, no duplicates, never a line, wave, agent or verdict. States: `planned`, `in_progress`, `covered`, `candidate`, `blocked`, `deferred`, `not_applicable`, `out_of_scope`. A unit closes only with an owner, reviewed paths and checks; a source check has no artifact, a local check needs a promoted regular file. The ledger is the coverage claim — "auth reviewed" is not.
3. **Hunt** — one fresh owner per unit set. The prompt carries the role, the architecture verbatim, unit IDs and starting paths, the exact class text, exclusions with reasons, the method, the safety rules, prior same-source exclusions, and one JSON result contract. Depth bound: trace only until the invariant is settled, then stop with `covered`, `candidate`, `blocked` or `uncovered`. Sad paths only where the interface accepts them — absent, empty, zero, negative, maximum, duplicate, stale, revoked, reordered, concurrent, partially migrated, failed dependency. One root cause, one fingerprint; a different boundary comes back as `uncovered` for a new unit.
4. **Validate** — every unique candidate to a verifier that did not find it and does not see another verifier's conclusion. It tries to refute from source, re-reads every cited line, independently reproduces the decisive check, and returns `confirmed`, `needs_validation` or `rejected` with the same fingerprint. A candidate is never upgraded on the finder's word.
5. **Verify** — one fresh verifier per retained record, checking the record rather than the write-up: paths and lines, conditions, the strongest preventing layer, the observed result, impact against severity, and whether the fix enforces the invariant instead of moving trust. A replacement that strengthens a verdict or changes root cause, trace, input, result, impact or severity gets another fresh verifier; with no independent verifier left the record leaves the set, its unit stays `candidate`, and the run is incomplete.
6. **Report** — derived from verified records and the ledger only; prose never changes a verdict. Confirmed table (severity, title, boundary, one-line result), per-finding detail (principal, bounded reproduction, conditions, impact, smallest fix), a needs-validation table with no severity, hardening notes and positive patterns, then the coverage counts and the final critic's result. The three files are `REPORT.md`, `FINDINGS-DETAIL.md` and `NEEDS-VALIDATION.md`. Zero confirmed is a normal result; invented low findings are not.

## Coverage, critics and stopping

- After each wave, one fresh critic audits coverage rather than findings: missing units, units to reassign, prior leads resolved, and whether it can stop. A reopened unit archives its old evidence and gets a new owner and wave; old evidence is never copied forward.
- `standard` and `deep` run another wave only while accepted gaps exist, and claim complete coverage only after a clean post-wave critic and a distinct final-clean critic. `quick` is one wave and one critic, with the critic's remaining work recorded `deferred` — a pre-declared partial pass, not a clean one.
- A run ends in exactly two ways: every artifact written with the ledger's counts closed and every retained record verified, or `run_status: "incomplete"` with an exact reason and the gap stated in the report. Never mid-phase, never a silent cap presented as coverage.

## Priority and budget

- Order the work: unauthenticated and lowest-trust surfaces first, then the highest-value resources — credentials, cross-tenant data, code execution, release authority — then prior gaps and changed source, then classes with a history on this target type; lexicographic tie-break so two runs agree.
- A budget is a maximum invocation count. Reserve the mandatory critics and the verifiers before hunters; if the budget cannot fund reconnaissance, the critics and one verifier, launch nothing and say why. Never exceed a user budget silently: stop hunting, validate in fingerprint order, and record the unvalidated candidates with their units.
- The pipeline caps still hold: three rounds on one assertion, two attempts per unit, one question per stop. A fourth attempt is not a retry, it is a new task with a named blocker.

## Evidence and safety

- Source first. Target-controlled code — builds, tests, parsers, browsers, emulators, fuzzers, fixtures — runs only inside an OS sandbox with no external network, an empty allowlisted environment, a read-only target and toolchain, writes only to scratch, and explicit low CPU, memory, process, size and time limits. Missing any control means no execution.
- Dummy principals, fixtures and secrets; loopback only for a local fixture. Never a deployed endpoint, real account, shared queue, registry, signing service, paid API, publication, or any stress of availability. Stop at the minimum boundary result: no persistence, no denial, no post-exploitation.
- A deployment, proxy, identity-provider, browser, broker or certificate fact outside source is neither present nor absent: `needs_validation` with the exact missing observation and a safe owner-observed check. No installs and no dependency fetching during an audit.
- Evidence states what ran, the exact input, the limits, and the minimum output. Never capture the ambient environment, credentials, or unrelated host paths.

## The fix

Name the invariant the code must enforce, the narrowest change at the last trusted decision point, and the regression test that fails without it. The audit writes neither. A finding that cannot name its fix has not understood the bug.

## Anti-patterns

- A checklist deviation with no affected principal.
- A defence-in-depth gap presented as a vulnerability.
- Probing a live or shared system for proof a source trace already gives.
- Guessing a provider, proxy, browser, identity or deployment fact.
- Self-impact or intended authority reported as a boundary crossing.
- A crash, a hang or a wrong value strengthened into takeover claims.
- Prose findings that cannot be deduplicated or verified.
- A severity on a `needs_validation` lead, or a report written before verification.
- Re-reporting a carried same-source record as a new find.

## Where the rest lives

`security-classes.md` is the class inventory selected per surface. The source repository carries the long form — `RECONNAISSANCE.md`, `HUNTING.md` and `VALIDATION-AND-REPORTING.md` for the exact prompts and result contracts, `ATTACK-CLASSES.md` for the ordinary classes, eleven domain companions for native, AI, HTTP/identity, client, supply chain, cloud, protocol, availability, data lifecycle and desktop classes, plus `report-schema.json` and two zero-dependency Node validators that check the ledger's counts and the findings' shape. Nothing here needs them installed; they are where to look when a class needs more than the row in `security-classes.md`.

<!-- source: cloudflare/security-audit-skill (MIT) — SKILL.md, RECONNAISSANCE.md, HUNTING.md, ATTACK-CLASSES.md, VALIDATION-AND-REPORTING.md -->
