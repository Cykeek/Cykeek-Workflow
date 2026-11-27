# Security

**Index** — Gate · Verdicts · Phases · Coverage · Classes. Read the sections the routing row names, not the file.

Read when the question is "is this safe?" rather than "make it work": an audit, a pen test, a review of auth, sessions, payments, uploads, secrets or dependencies, a vulnerability report, or a change that touches a trust boundary. An audit describes the fix and never writes it — a fix asked for afterwards is its own task, under the normal pipeline. This file is the protocol; `## Classes` is what to look at, chosen per surface found in reconnaissance.

One rule reduces the file: a finding is a crossed trust boundary with a lower-trust principal and a concrete result, proven from source or a bounded local check. Everything below applies that rule without guessing, without looping and without noise.

## Gate

Hunting before the answers is the wrong approach: a grep for flags before the five answers produces checklist deviations with no affected principal and guessed facts — noise a verifier must undo before anything is real.

Five answers, one line each, before the first file is read for hunting. A one-line security question gets one line.

| Ask | Answer by naming |
| --- | --- |
| What is the target? | repository root, source ref, and whether the worktree is dirty |
| Which mode? | `guidance` for a question, a focused review or a triage; `full audit` only when the user asks to audit, pen-test, review end to end, or wants report files |
| Which profile? | `quick`, `standard` or `deep` — standard by default, quick for a re-run or a first look, deep for high stakes |
| Which surfaces? | the paths, subsystems or diff in scope; everything else is `out_of_scope`, never silently dropped |
| What proves it? | a verifier that did not hunt, and the file:line or command that settles each claim |

**Guidance** answers in the conversation — the exact line, the condition that would make it real, the fix — with no directory, no artifacts, no six phases; focused workers may run and return to this task. **Full audit** runs the six phases and the files they leave, entered only on the explicit ask; an unspecified output directory is `~/security-audit-skill/<repo>/run-<N>`, outside the target, inside it only when the user names it and version control ignores the whole directory. Ambiguous between the two: ask one question before creating a file. Loading this reference never authorizes a full audit.

A class is admitted only as a question to settle, never a checklist to tick: the answer must be a source path and a test, or the exact fact that is missing. Classes are selected by boundary — never because a language, framework or package name appears; a comparable product calibrates effort and never dismisses a finding. The taxonomy stays honest through the exclusions in § Verdicts and the one-fingerprint rule in § Phases — a class row is never a verdict by itself.

Freshness: read prior ledgers first — same-source confirmations are re-verified, and changed source, `needs_validation`, `deferred` and `blocked` are current work, not coverage.

Evidence rules: evidence states what ran, the exact input, the limits, and the minimum output — a link, a guess, or the ambient environment is not evidence; credentials and unrelated host paths are never captured. A verdict names its unit; a conclusion names the phase and unit it settles. An unverified code path is `needs_validation` with the exact missing verification named: a deployment, proxy, identity-provider, browser, broker or certificate fact outside source is neither present nor absent, and needs the exact missing observation and a safe owner-observed check. No installs and no dependency fetching during an audit.

## Verdicts

Six parts, one sentence each, before anything is a finding: the lower-trust principal and its starting capability; the accepted value, action or selector; the control that should have rejected or bound it; the exact source path after that decision; the affected principal or resource; the observed or owner-observable result.

| Verdict | Carries | Never |
| --- | --- | --- |
| `confirmed` | complete source trace, conditions, a bounded observed result, severity, confidence, the smallest fix | without a reproduced result, or with a visible preventing layer |
| `needs_validation` | source trace, the exact unresolved blocker, a local or owner-observed plan | a severity, the word vulnerability, or a guess at deployment |
| `rejected` | the trace and the disproof, kept so a later run does not repeat it | a second report of the same claim without changed evidence |

These map onto the three states: `confirmed` is a PASS, `needs_validation` a UNVERIFIED, `rejected` a closed FAIL. One unverified claim is reported as such, never averaged into a clean sweep.

| Level | Anchor |
| --- | --- |
| critical | an unauthenticated actor gets code execution, the whole data store, or any account takeover |
| high | an explicit control fully defeated with real consequences: auth bypass, cross-tenant read or write, stored script for other users, authenticated code execution, a remote stop of a shared service |
| medium | a real boundary violation with a narrow blast radius or uncommon preconditions |
| low | non-secret internals disclosed, or sustained effort for minimal gain |
| informational | confirmed, minimal impact, useful as a prerequisite inside a larger finding |

Severity is likelihood × impact and never exceeds demonstrated impact — it is recorded only where it gates the next action. The high/medium discriminator is whether the result fully defeats the control or only weakens it; if the concrete damage cannot be stated, the level is lower than it feels. Only `confirmed` records carry a severity.

A finding ends one way only: `rejected` keeps the trace and the disproof; `needs_validation` names the exact verification still missing; `confirmed` is resolved at the matching layer with the smallest fix. A carried same-source record is never re-reported as a new find.

Never a finding: a checklist deviation or missing best practice with no affected principal — a hardening note; a defence-in-depth gap another layer already stops — a hardening note; self-impact or intended authority as a cross-boundary result; a crash sold as code execution, a hang as shared availability, a wrong value strengthened into takeover claims, or a cookie flag before a less-trusted origin can use it; probing a live or shared system for proof a source trace already gives; a guessed provider, proxy, browser, identity or deployment fact; prose findings that cannot be deduplicated or verified; a severity on a `needs_validation` lead, or a report written before verification.

## Phases

1. **Map (scope)** — `architecture.md`, about 1,000 words: product and principals, protected resources, stack and deployment paths, entry surfaces with their sink paths, trust boundaries with the strongest source-visible control on each, starting paths, prior gaps, and the companion classes selected per boundary (§ Gate). A comparable product calibrates effort and never dismisses a finding.
2. **Inventory (the unit plan)** — `coverage-ledger.json`, the one-unit-per-combination rule in § Coverage: every surface a Web App, API, Desktop, Provider, Pipeline or Identity system exposes. IDs are canonical — percent-encoded refs joined by `::`, sorted, no duplicates, never a line, wave, agent or verdict. States: `planned`, `in_progress`, `covered`, `candidate`, `blocked`, `deferred`, `not_applicable`, `out_of_scope`. A unit closes only with an owner, reviewed paths and checks; a source check has no artifact, a local check needs a promoted regular file. The ledger is the coverage claim — "auth reviewed" is not.
3. **Check (hunt, then validate)** — one fresh owner per unit set, carrying the role, the architecture verbatim, unit IDs and starting paths, the exact class text, exclusions with reasons, the method, the safety rules, prior same-source exclusions and one JSON result contract. Depth bound: trace only until the invariant is settled, then stop with `covered`, `candidate`, `blocked` or `uncovered`. Sad paths only where the interface accepts them — absent, empty, zero, negative, maximum, duplicate, stale, revoked, reordered, concurrent, partially migrated, failed dependency. One root cause, one fingerprint; a finding is scope + class + proof. Every unique candidate goes to a verifier that did not find it and does not see another verifier's conclusion: it refutes from source, re-reads every cited line, independently reproduces the decisive check, and returns `confirmed`, `needs_validation` or `rejected` with the same fingerprint. A candidate is never upgraded on the finder's word.
4. **Verify** — one fresh verifier per retained record, checking the record rather than the write-up. It settles only when: the trace runs from a real lower-trust entry to the claimed effect with every line in current source, under the conditions stated; the strongest preventing layer is found and named — if it stops the attack the result is hardening, not a finding; a framing or cache claim names both components with the exact normalized value each sees, a credential or identity claim cites the verification line and every missing binding, and an access-control claim shows the accepted request; the observed effect is exactly what the evidence shows and severity is no higher than the demonstrated impact; and the fix enforces the invariant instead of moving trust. A replacement that strengthens a verdict or changes root cause, trace, input, result, impact or severity gets another fresh verifier; with no independent verifier left the record leaves the set, its unit stays `candidate`, and the run is incomplete.
5. **Fix (the matching layer)** — the invariant the code must enforce, the narrowest change at the last trusted decision point, and the regression test that fails without it. The audit writes neither. A finding that cannot name its fix has not understood the bug.
6. **Report** — derived from verified records and the ledger only; prose never changes a verdict. Order: confirmed table (severity, title, boundary, one-line result), per-finding detail (principal, bounded reproduction, conditions, impact, smallest fix), a needs-validation table with no severity, hardening notes and positive patterns, then the coverage counts and the final critic's result. The three files are `REPORT.md`, `FINDINGS-DETAIL.md` and `NEEDS-VALIDATION.md`. Zero confirmed is a normal result; invented low findings are not. Evidence names the file and line. Limits: target-controlled code — builds, tests, parsers, browsers, emulators, fuzzers, fixtures — runs only inside an OS sandbox with no external network, an empty allowlisted environment, a read-only target and toolchain, writes only to scratch, and explicit low CPU, memory, process, size and time limits; missing any control means no execution. Dummy principals, fixtures and secrets; loopback only for a local fixture. Never a deployed endpoint, real account, shared queue, registry, signing service, paid API, publication, or any stress of availability. Stop at the minimum boundary result: no persistence, no denial, no post-exploitation.

## Coverage

The ledger rule: one unit per surface × boundary × subsystem × attack class, plus lifecycle when it matters — 2 is not 1, and `not_applicable` and `out_of_scope` are values, never blanks. In scope is what the gate named. What every unit reviews — the four moves: walk the lifecycle of the object (create, read, update, delete, export, restore) and every door to it (API, job, CLI, admin, webhook, database replica, logs); compare the two sides of every trust decision (parser and consumer, policy and query, gateway and handler, client and server); follow the copy — data stored, cached, indexed, derived, logged or exported stops being governed by the check at the entry point; read the sad path — the branch after the check fails, the retry, the partial write, the fallback identity, the exception handler.

Workflow: a new execution phase, agent surface or source is a new unit. A reopened unit archives its old evidence and gets a new owner and wave; old evidence is never copied forward. After each wave one fresh critic audits coverage rather than findings: missing units, units to reassign, prior leads resolved, and whether it can stop. `standard` and `deep` run another wave only while accepted gaps exist, and claim complete coverage only after a clean post-wave critic and a distinct final-clean critic; `quick` is one wave and one critic, with the critic's remaining work recorded `deferred` — a pre-declared partial pass, not a clean one. A run ends in exactly two ways: every artifact written with the ledger's counts closed and every retained record verified, or `run_status: "incomplete"` with an exact reason and the gap stated in the report. Never mid-phase, never a silent cap presented as coverage.

Order and budget: unauthenticated and lowest-trust surfaces first, then the highest-value resources — credentials, cross-tenant data, code execution, release authority — then prior gaps and changed source, then classes with a history on this target type; lexicographic tie-break so two runs agree. A budget is a maximum invocation count: reserve the mandatory critics and the verifiers before hunters; if the budget cannot fund reconnaissance, the critics and one verifier, launch nothing and say why. Never exceed a user budget silently — stop hunting, validate in fingerprint order, and record the unvalidated candidates with their units. The pipeline caps still hold: three rounds on one assertion, two attempts per unit, one question per stop. A fourth attempt is not a retry, it is a new task with a named blocker.

## Classes

Read every row with the four moves in § Coverage; a row settles only under § Gate's admission rule.

**The ordinary classes — every target**

**Injection** — untrusted input reaches SQL, HTML, a shell, a template, a path, a redirect, a log line, a header or a deserializer. Name the sink, the encoding applied, and every path to the same sink: stored then used, second-order through a file or queue, keys and field names as well as values, and metadata the framework parses.

**Broken access control** — for each lower-trust principal, list the actions it has by design; then check that every route, handler, job, export and bulk read enforces the right permission over the right resource through the right mechanism. Look for parallel implementations of one check, client-supplied ownership, body or query overriding identity, masked-field leaks, verbs the router forgets, and an authn check mistaken for an authz one.

**Resource and file handling** — traversal and symlinks, absolute-path injection, archive extraction escapes, temp-file races, unbounded reads and writes, uploads whose type is checked by extension only, and files served back with an inferred content type.

**Server-side request forgery** — a user-supplied URL or host reaches a fetch: scheme allowlist, post-redirect host check, DNS and parsing differentials, metadata endpoints, credential forwarding, and any provider SDK that follows redirects by default.

**Deserialization and parser abuse** — native JSON/codecs, XML entities, YAML tags, template compilation, image and archive parsers, and anything that instantiates types or executes callbacks from data.

**Cryptography and secrets** — predictable randomness for tokens, hardcoded or committed keys, secrets in logs and errors, password hashing that is fast or unsalted, HMAC compared non-constant-time, reused or static IV and nonce, ECB and home-made modes, JWT verification that trusts a header algorithm or skips a claim, and any fail-open branch.

**Business logic** — state machines that skip or replay transitions, check-then-act races (double spend, duplicate claim, coupon reuse), numeric and currency manipulation, trust in client-computed totals, negative quantities, refunds after cancellation, time boundaries and timezones, and the fallback path when a provider fails.

**Feature abuse and leaked data** — export and import wider than the acting scope; search and sort that answer what a page reveals; enumeration through error text, timing, counts or ids; preview, draft and staging routes without the production check; webhook receivers that trust the payload rather than the signature; invoice and receipt files reachable by URL.

**Chained findings** — a combination that violates a guarantee neither component breaks alone; a fix that moves trust rather than removing it; scope that grows one step per request; ordering and timing across two processes; and a rollback that restores state the forward path never allowed.

**The obvious things** — grep for committed secrets and `.env`, debug and test flags reachable in production, seeded or default credentials, `TODO`/`FIXME` around auth, `eval`/`exec` on data, `Access-Control-Allow-Origin: *` with credentials, cookies without `HttpOnly`/`Secure`/`SameSite`, redirect parameters (`redirect`, `return`, `next`, `url`, `goto`, `continue`), missing TLS, stack traces in responses, unpinned dependencies with known advisories, and admin, GraphQL-introspection, metrics or debug endpoints reachable without a check. A flag still gets traced to an impact before it is a finding.

**HTTP, identity and sessions**

**Request framing and desynchronization** — front end and back end disagree on length or normalization: several `Content-Length` values, `Transfer-Encoding`, HTTP/2 downgrade, header-name folding, CR/LF conversion. Name both components and the exact bytes each assigns to which request.

**Cache poisoning and deception** — a request value changes the cached body or a security header but is absent from the cache key (forwarded host and scheme, selected cookies, query normalization, language); or a private dynamic route is cacheable because the edge sees a static suffix. Require a victim-visible change, not just an unkeyed header.

**Host and forwarded-header trust** — `Host`, `Forwarded`, `X-Forwarded-*`, `Origin` and `Referer` decide absolute URLs, tenant routing, reset links, callbacks or client identity. Check who can supply each and whether trusted ingress strips the client's copy.

**Response-header injection** — untrusted data in `Location`, `Set-Cookie` or CSP with control characters; confirm the framework's rejection before reporting.

**CSRF** — an ambient credential, a state-changing route, an accepted cross-site shape (form, JSON-like, multipart, method override), the cookie's `SameSite`, and the absent effective token or origin check. A bearer-token API or a read-only route does not qualify.

**Session and cookie lifecycle** — fixation at login, rotation on privilege change, invalidation on logout and password reset, server-side revocation actually consulted, cookie scope and transport, and remember-me tokens that never expire.

**JWT** — signature verified with a fixed algorithm, then every binding the role needs: issuer, audience, expiry, not-before, subject, session, tenant, and one-time state. Check algorithm confusion, `alg: none`, key confusion between HMAC and public keys, and claims trusted from an unsigned or nested token.

**OAuth, OIDC and SAML** — `redirect_uri` matched exactly, `state` bound to the session, PKCE where the client is public, authorization-code replay, token audience and issuer checks, implicit flows, IdP-initiated assertions, signed-object wrapping and reference resolution, and a relay state that decides the landing identity.

**MFA, passkeys and account transitions** — enrolment without re-authentication, step-up bound to the action rather than the session, a fallback that skips the second factor, recovery codes that never rotate, WebAuthn origin and challenge binding, passkey registration replacing an existing credential without proof, email or phone change that transfers the account, and linking that merges two identities on an unverified claim.

**API keys and mTLS** — scope limited to the owning principal and action, keys not readable from the client or a log, rotation and revocation enforced, and certificate identity bound to the principal rather than asserted in a header the caller writes. Check the renewal path and what happens when the identity provider is unreachable.

**Browser and client**

**DOM XSS and clobbering** — `innerHTML`, `insertAdjacentHTML`, `document.write`, `srcdoc`, jQuery `html()`, framework escape hatches, and DOM clobbering where an injected `id`/`name` shadows a variable the script then trusts.

**Prototype pollution** — deep merge, `Object.assign` from parsed input, `__proto__`/`constructor`/`prototype` keys, and a gadget in a library that turns the polluted object into script or a privileged request.

**`postMessage` and cross-window trust** — an origin check that accepts `*`, a handler trusting `event.data` shape, a receiver on the opener, and a token or action passed through a channel the page does not own.

**Cross-site WebSocket and CORS** — a handshake that authenticates only by ambient cookie with no origin check, and credentialed CORS reflecting an arbitrary origin, or trusting a null origin.

**Service workers** — registration scope wider than the app, a cache key that ignores identity, a stale shell serving authenticated content after logout, and an update path a lower-trust page can influence.

**Storage and stale authorization** — tokens and PII in `localStorage`, an authorization decision cached in the client, tab or broadcast-channel state that outlives logout, and a cached page reachable with the back button after sign-out.

**XS-Leaks and navigation** — cross-origin state inferred through cache, frame count, error events, redirects or timing; `window.opener` left powerful; clickjacking without frame-ancestors or a framebusting gap; and a navigation the app rewrites into a trusted destination.

**Data isolation and lifecycle**

**Tenant and object isolation** — every query, cache key, search index, job, export and signed URL carries the owner or tenant, and the check is enforced server-side from the session rather than a parameter. Composite keys, namespace collisions and a policy that disagrees with the query are the usual ways one tenant reads another.

**Derived readers** — search indexes, caches, analytics, logs, traces and diagnostics are alternate read paths: do they carry the same ACL, and does a reindex or rebuild preserve it? Aggregate counts and error timing can answer what the UI hides.

**Export, backup, import and restore** — scope at the moment of export, an import that trusts the file's ownership, a restore that reintroduces a deleted tenant or a revoked role, a migration that assigns a default owner, and replication that crosses a boundary the primary enforces.

**Deletion and retention** — soft delete that still serves reads, tombstones that skip the filter, queued work that re-creates deleted data, backups kept beyond the promise, revocation that does not reach cached tokens or derived copies, and retention jobs that fire late or early.

**Supply chain and release**

**Dependency and build input** — a name resolved from the wrong registry or namespace, a mutable tag or unpinned action, a lockfile not enforced, codegen whose provenance is unverified, and a build context that includes secrets or developer files.

**CI and automation** — untrusted code from a PR or issue title reaching a privileged workflow, expression injection into `run:`, cache and artifact poisoning across trust levels, write tokens on a fork build, and a bot identity with more repository scope than the job needs.

**Release, signing and updates** — a build substituted between compile and promotion, an artifact promoted without the scanned digest, signature verification skipped on one channel, update metadata that trusts a redirect or an older signed version, rollback to a vulnerable build, and a plugin or extension installed with the app's authority.

**Cloud and deployment**

**Workload identity and IAM** — a role assumable across accounts or tenants, a wildcard action or resource, a trust policy that accepts any principal in a provider, application authorization delegated to cloud metadata, and the control-plane path reachable from the workload.

**Ingress, mesh and metadata** — a trusted-proxy header accepted from the internet, mesh identity bypassed by a direct pod address, instance metadata reachable from a request-driven process, and an admission policy applied to one path but not the other.

**Containers and configuration** — a privileged container, mounted socket or host path exposing the control plane; namespace or label used as a security boundary; a security context that silently drops a control; secrets mounted wider than one workload, credentials renewed on a timer with a fail-open outage path, and object or signed-URL policy that trusts the bucket default.

**Events and edge** — an event source whose identity decides authorization, a runtime that differs from the source the repo selects, and an edge rule that runs before the application sees the request.

**AI, agents and MCP**

**Indirect injection and context** — retrieved documents, tool output, web pages, issue text or memory inject instructions; cross-session or cross-tenant context bleed; persistent memory poisoned once and reused; role and provenance confusion where user text is treated as system instruction.

**Tools and agency** — a tool argument reaching a downstream sink without validation; excessive agency, where the model can act beyond the requesting user; a confirmation that is not bound to the exact action and parameters; a tool schema that disagrees with the dispatcher; and an action loop with no bound or budget.

**MCP and sub-agents** — trust inherited without re-checking the server's identity, a server name or metadata treated as policy, a sub-agent allowed to widen its own scope, and model output rendered as HTML or used as a command.

**Disclosure** — secrets, system prompt, other users' context or internal tool results extracted through a legitimate feature, and an error path that echoes the whole context.

**Protocols, RPC and messaging**

**Framing and schemas** — two components disagree on boundaries or canonical form; union, enum or default values interpreted differently; an envelope identity that does not match the payload; an interceptor and a method path that apply different checks.

**Peer identity and authorization** — a transport identity mistaken for the application principal; per-item and streaming authorization gaps where the stream is checked once; reply correlation a client can spoof; topic, routing-key and subscription scope wider than the subscriber's right.

**Delivery semantics** — duplicate delivery without idempotency, out-of-order or stale messages accepted, acknowledgment before commit, partial multi-consumer transitions, a dead-letter queue that discloses payloads, and an untrusted producer treated as a control plane.

**Resource exhaustion**

**Amplification** — superlinear parsing, matching or evaluation on user input; compressed input expanding without a ratio cap; one request fanning out into many queries; unbounded buffers or cardinality.

**Accumulation and scheduling** — descriptor, handle or temporary-file leaks; work that survives cancellation; expensive work before authentication; quota accounting with the wrong scope or reset; worker or priority starvation; a reachable fatal error or deadlock.

**Recovery** — a retry storm, a poison record that blocks a queue head, capacity that rolls back to an unsafe default, and a failure path that fails open.

**Native, binary and kernel** — out of this skill's default scope: a C/C++/Rust-unsafe, kernel, parser or JIT target needs the source repository's `MEMORY-SAFETY-AND-BINARY.md`. The gate, the verdicts and the verification phases still apply. Its classes, named so nothing is missed: out-of-bounds read or write; integer overflow, underflow, truncation and signedness; unit and pointer-depth confusion; uninitialized data; use-after-free, stale view and double free; type confusion and invalid downcast; reference-count and ownership races; shared-state races and TOCTOU; lock-order deadlock and starvation; pointer-length and ownership contract mismatch; layout, alignment and enum disagreement; unwind and thread-affinity violations; library, plugin and executable search-order trust; missing artifact identity; malformed binary metadata and relocations; JIT and generated-code consistency; unload and teardown safety; user-copy bounds; privileged object lifecycle; under-authorized powerful interfaces.

**Desktop, mobile and local IPC** — Custom-scheme and deep-link ambiguity; app and account handoff confusion; file-open and intent authority; navigation-origin to bridge confusion; over-broad native bridge capabilities; webview file and universal access; IPC peer authentication; claimed principal versus channel identity; exported service, activity, receiver or provider overreach; IPC lifecycle and correlation; privileged helper as confused deputy; install, update and repair path trust; local file ownership and TOCTOU; credential-store boundary mismatch; account switch, logout and device restore leakage; pending-action and user-presence confusion. The source repository's `DESKTOP-MOBILE-AND-LOCAL-IPC.md` carries the checks; the verdict rules here still decide.

The source repository carries the long form — `RECONNAISSANCE.md`, `HUNTING.md` and `VALIDATION-AND-REPORTING.md` for the exact prompts and result contracts, `ATTACK-CLASSES.md` for the ordinary classes, eleven domain companions for native, AI, HTTP/identity, client, supply chain, cloud, protocol, availability, data lifecycle and desktop classes, plus `report-schema.json` and two zero-dependency Node validators that check the ledger's counts and the findings' shape. Nothing here needs them installed; they are where to look when a class needs more than its line above.

<!-- source: cloudflare/security-audit-skill (MIT) — merged from security.md (SKILL.md, RECONNAISSANCE.md, HUNTING.md, ATTACK-CLASSES.md, VALIDATION-AND-REPORTING.md) and security-classes.md (ATTACK-CLASSES.md, SKILL.md universal moves, eleven domain companions) -->
