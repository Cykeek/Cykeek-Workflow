# Domain

Read only when the work is genuinely business logic, data systems or service architecture — never for a marketing page or a dashboard whose domain is already obvious.

## Does this apply?

Three conditions. If none holds, close this file and go to `craft.md`; the ceremony here is a real cost, and paying it on a CRUD screen is how DDD gets its reputation.

- identity or invariants that must always hold — not merely fields that travel together;
- more than one meaning of one word, or more than one team, service or context owning it;
- data that outlives the request: stored, replayed, replicated, migrated or shared.

Then classify before modeling: **core** (the reason the product exists — model it properly), **supporting** (needed, not differentiating — keep it simple), **generic** (buy or borrow — configure it, never rebuild it). Modeling effort follows that order and nothing else.

## Start here

- Name the **bounded context** and its local language before interpreting any model, service, repository, event or API.
- Invest real design in the **core domain**; keep supporting, generic and CRUD areas simple. Ceremony around a CRUD screen is not DDD.
- If a term is fuzzy, overloaded or reused across contexts, sharpen or qualify it before writing code. Rename the code to match.
- Business behaviour belongs in the model, not in controllers, jobs, SQL, serializers or framework hooks. If a flag, setter or primitive is carrying a business decision, the domain concept is missing.

## Model

- **Entity** for identity and lifecycle; **value object** for an immutable, validated descriptive value; **domain service** only when no object naturally owns the operation.
- **Aggregate**: a small immediate-consistency boundary with one root, hidden mutable internals, and references to other aggregates by identity only. One aggregate modified per transaction by default; coordinate across aggregates with events or application services.
- **Domain events** are completed, past-tense business facts — not commands and not field-change noise.
- **Repository** per aggregate root; **application service** coordinates the use case and does not decide business rules.
- If a transaction or object graph wants several roots, demand proof of the invariant, then coordinate by identity or eventual consistency.

## Between contexts

- Translate at the boundary; never share domain classes, aggregates, enums or terms whose meaning diverges.
- Choose the relationship deliberately: partnership, customer/supplier, conformist, or anticorruption layer when foreign meaning would otherwise leak.
- Integration contracts are separate from internal models. Do not expose aggregate internals through REST, transport payloads or a client's DTO.

## Enterprise layering

- Choose the business-logic pattern by actual complexity: transaction script for simple independent flows, table module for table-shaped logic, domain model for real invariants and lifecycle.
- A service layer coordinates and orchestrates transactions; it is not the home for all behaviour.
- Match persistence to coupling: repository or data mapper to separate domain from storage, gateway for record access, active record only for simple persistence-coupled domains.
- Make unit of work, identity scope, loading, transaction boundaries and lock semantics explicit before trusting an ORM.
- Remote boundaries are expensive and fail partially: coarse operations, DTO translation, explicit versioning.
- If one class owns UI, workflow, rules, SQL, transactions and external calls, split responsibility before adding patterns.

## Data

- For every important data change, state the source of truth, the consistency expectation, the durability point, the visibility point, the retry semantics and the evolution path.
- Caches, indexes, projections, search copies and materialized views are **derived data**: they need a staleness bound, lag visibility, and a rebuild path.
- Retried, replayed, queued, streamed and event-driven work must be idempotent or transactional. Exactly-once is a claim to prove, not to assume.
- Schemas, encodings, APIs, messages and events are versioned contracts that must survive old code, old data, rolling upgrades and in-flight messages.
- Assume partial failure: crashes, duplicate messages, reordering, stale replicas, clock skew, unknown success. Design the invariant to tolerate it.

<!-- source: domain-driven-design, domain-driven-design-distilled, implementing-domain-driven-design, patterns-of-enterprise-application-architecture, designing-data-intensive-applications -->
