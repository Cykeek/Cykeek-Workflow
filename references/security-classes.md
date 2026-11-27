# Security classes

Read after `security.md` § The gate: this file is what to look at, chosen per surface found in reconnaissance. A row is a question to settle, not a checklist to tick — the answer is a source path and a test, or the exact fact that is missing. Select by boundary, never because a language, framework or package name appears.

Four moves apply to every row. **Walk the lifecycle** of the object (create, read, update, delete, export, restore) and every door to it (API, job, CLI, admin, webhook, database replica, logs). **Compare the two sides** of every trust decision — parser and consumer, policy and query, gateway and handler, client and server. **Follow the copy** — data stored, cached, indexed, derived, logged or exported stops being governed by the check at the entry point. **Read the sad path** — the branch after the check fails, the retry, the partial write, the fallback identity, the exception handler.

## The ordinary classes — every target

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

## HTTP, identity and sessions

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

## Browser and client

**DOM XSS and clobbering** — `innerHTML`, `insertAdjacentHTML`, `document.write`, `srcdoc`, jQuery `html()`, framework escape hatches, and DOM clobbering where an injected `id`/`name` shadows a variable the script then trusts.

**Prototype pollution** — deep merge, `Object.assign` from parsed input, `__proto__`/`constructor`/`prototype` keys, and a gadget in a library that turns the polluted object into script or a privileged request.

**`postMessage` and cross-window trust** — an origin check that accepts `*`, a handler trusting `event.data` shape, a receiver on the opener, and a token or action passed through a channel the page does not own.

**Cross-site WebSocket and CORS** — a handshake that authenticates only by ambient cookie with no origin check, and credentialed CORS reflecting an arbitrary origin, or trusting a null origin.

**Service workers** — registration scope wider than the app, a cache key that ignores identity, a stale shell serving authenticated content after logout, and an update path a lower-trust page can influence.

**Storage and stale authorization** — tokens and PII in `localStorage`, an authorization decision cached in the client, tab or broadcast-channel state that outlives logout, and a cached page reachable with the back button after sign-out.

**XS-Leaks and navigation** — cross-origin state inferred through cache, frame count, error events, redirects or timing; `window.opener` left powerful; clickjacking without frame-ancestors or a framebusting gap; and a navigation the app rewrites into a trusted destination.

## Data isolation and lifecycle

**Tenant and object isolation** — every query, cache key, search index, job, export and signed URL carries the owner or tenant, and the check is enforced server-side from the session rather than a parameter. Composite keys, namespace collisions and a policy that disagrees with the query are the usual ways one tenant reads another.

**Derived readers** — search indexes, caches, analytics, logs, traces and diagnostics are alternate read paths: do they carry the same ACL, and does a reindex or rebuild preserve it? Aggregate counts and error timing can answer what the UI hides.

**Export, backup, import and restore** — scope at the moment of export, an import that trusts the file's ownership, a restore that reintroduces a deleted tenant or a revoked role, a migration that assigns a default owner, and replication that crosses a boundary the primary enforces.

**Deletion and retention** — soft delete that still serves reads, tombstones that skip the filter, queued work that re-creates deleted data, backups kept beyond the promise, revocation that does not reach cached tokens or derived copies, and retention jobs that fire late or early.

## Supply chain and release

**Dependency and build input** — a name resolved from the wrong registry or namespace, a mutable tag or unpinned action, a lockfile not enforced, codegen whose provenance is unverified, and a build context that includes secrets or developer files.

**CI and automation** — untrusted code from a PR or issue title reaching a privileged workflow, expression injection into `run:`, cache and artifact poisoning across trust levels, write tokens on a fork build, and a bot identity with more repository scope than the job needs.

**Release, signing and updates** — a build substituted between compile and promotion, an artifact promoted without the scanned digest, signature verification skipped on one channel, update metadata that trusts a redirect or an older signed version, rollback to a vulnerable build, and a plugin or extension installed with the app's authority.

## Cloud and deployment

**Workload identity and IAM** — a role assumable across accounts or tenants, a wildcard action or resource, a trust policy that accepts any principal in a provider, application authorization delegated to cloud metadata, and the control-plane path reachable from the workload.

**Ingress, mesh and metadata** — a trusted-proxy header accepted from the internet, mesh identity bypassed by a direct pod address, instance metadata reachable from a request-driven process, and an admission policy applied to one path but not the other.

**Containers and configuration** — a privileged container, mounted socket or host path exposing the control plane; namespace or label used as a security boundary; a security context that silently drops a control; secrets mounted wider than one workload, credentials renewed on a timer with a fail-open outage path, and object or signed-URL policy that trusts the bucket default.

**Events and edge** — an event source whose identity decides authorization, a runtime that differs from the source the repo selects, and an edge rule that runs before the application sees the request.

## AI, agents and MCP

**Indirect injection and context** — retrieved documents, tool output, web pages, issue text or memory inject instructions; cross-session or cross-tenant context bleed; persistent memory poisoned once and reused; role and provenance confusion where user text is treated as system instruction.

**Tools and agency** — a tool argument reaching a downstream sink without validation; excessive agency, where the model can act beyond the requesting user; a confirmation that is not bound to the exact action and parameters; a tool schema that disagrees with the dispatcher; and an action loop with no bound or budget.

**MCP and sub-agents** — trust inherited without re-checking the server's identity, a server name or metadata treated as policy, a sub-agent allowed to widen its own scope, and model output rendered as HTML or used as a command.

**Disclosure** — secrets, system prompt, other users' context or internal tool results extracted through a legitimate feature, and an error path that echoes the whole context.

## Protocols, RPC and messaging

**Framing and schemas** — two components disagree on boundaries or canonical form; union, enum or default values interpreted differently; an envelope identity that does not match the payload; an interceptor and a method path that apply different checks.

**Peer identity and authorization** — a transport identity mistaken for the application principal; per-item and streaming authorization gaps where the stream is checked once; reply correlation a client can spoof; topic, routing-key and subscription scope wider than the subscriber's right.

**Delivery semantics** — duplicate delivery without idempotency, out-of-order or stale messages accepted, acknowledgment before commit, partial multi-consumer transitions, a dead-letter queue that discloses payloads, and an untrusted producer treated as a control plane.

## Resource exhaustion

**Amplification** — superlinear parsing, matching or evaluation on user input; compressed input expanding without a ratio cap; one request fanning out into many queries; unbounded buffers or cardinality.

**Accumulation and scheduling** — descriptor, handle or temporary-file leaks; work that survives cancellation; expensive work before authentication; quota accounting with the wrong scope or reset; worker or priority starvation; a reachable fatal error or deadlock.

**Recovery** — a retry storm, a poison record that blocks a queue head, capacity that rolls back to an unsafe default, and a failure path that fails open.

## Native, binary and kernel

Out of this skill's default scope: a C/C++/Rust-unsafe, kernel, parser or JIT target needs the source repository's `MEMORY-SAFETY-AND-BINARY.md`. The gate, the verdicts and the verification phases still apply. Its classes, named so nothing is missed: out-of-bounds read or write; integer overflow, underflow, truncation and signedness; unit and pointer-depth confusion; uninitialized data; use-after-free, stale view and double free; type confusion and invalid downcast; reference-count and ownership races; shared-state races and TOCTOU; lock-order deadlock and starvation; pointer-length and ownership contract mismatch; layout, alignment and enum disagreement; unwind and thread-affinity violations; library, plugin and executable search-order trust; missing artifact identity; malformed binary metadata and relocations; JIT and generated-code consistency; unload and teardown safety; user-copy bounds; privileged object lifecycle; under-authorized powerful interfaces.

## Desktop, mobile and local IPC

Custom-scheme and deep-link ambiguity; app and account handoff confusion; file-open and intent authority; navigation-origin to bridge confusion; over-broad native bridge capabilities; webview file and universal access; IPC peer authentication; claimed principal versus channel identity; exported service, activity, receiver or provider overreach; IPC lifecycle and correlation; privileged helper as confused deputy; install, update and repair path trust; local file ownership and TOCTOU; credential-store boundary mismatch; account switch, logout and device restore leakage; pending-action and user-presence confusion. The source repository's `DESKTOP-MOBILE-AND-LOCAL-IPC.md` carries the checks; the verdict rules here still decide.

## What settles a verdict

1. The trace starts at a real lower-trust entry and ends at the claimed effect, every line present in current source.
2. The strongest preventing layer was found and named; if it stops the attack, the result is hardening, not a finding.
3. For a framing or cache claim, both components are named with the exact normalized value each sees; for a credential or identity claim, the verification line and every missing binding are cited; for access control, the accepted request is shown.
4. The observed effect is exactly what the evidence shows, and the severity is no higher than the demonstrated impact.

<!-- source: cloudflare/security-audit-skill (MIT) — ATTACK-CLASSES.md, SKILL.md universal moves, and the eleven domain companions -->
