# ADR-0010: Architectural patterns — DDD + CQRS

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

A domain-rich application: households, users, roles, one-off and recurring tasks, assignments, schedules, completion statistics. These elements have natural aggregates (e.g., `Household`, `Task`, `Assignment`, `Schedule`).

The choice of a NoSQL database ([ADR-0007](0007-database-cosmos-db-free-tier.md)) favors DDD modeling (aggregate ↔ document).

At the same time, the web panel ([ADR-0003](0003-web-admin-only-scope.md)) requires cross-cutting queries (statistics, reports), which in NoSQL are expensive (cross-partition queries) when executed directly against aggregates.

## Decision

A **DDD + CQRS** architecture:

- **DDD (Domain-Driven Design)** — we model the domain with aggregates inside clearly separated bounded contexts. Aggregate = transactional consistency boundary.
- **CQRS (Command Query Responsibility Segregation)** — we separate the write path (commands) from the read path (queries):
  - **Write side**: a command handler operates on an aggregate (1 Cosmos document) and enforces domain rules
  - **Read side**: dedicated **projections / read models** optimized for specific UI queries; updated reactively from the Cosmos Change Feed or domain events

**Event Sourcing**: status **TBD** — see [`open-decisions.md`](../architecture/open-decisions.md). By default **no ES** in MVP. Domain events may appear earlier as a mechanism for propagating to read models, without full ES.

## Considered alternatives

- **Anemic model + CRUD repository** — rejected. Loses the benefit of DDD; domain logic scattered across the service layer.
- **Full Event Sourcing from the start** — rejected for MVP. Overengineering without a specific audit/replay need. Kept as a future option.
- **No CQRS** — rejected. Admin reports would require expensive cross-partition queries in Cosmos.

## Consequences

### Positive
- Natural mapping of the domain to **Cosmos documents** (one collection = one aggregate type)
- Scalable reads via **denormalized read models** (each view has its own document optimized for the query)
- Domain logic **encapsulated in aggregates**, with good readability and testability
- Ability to later evolve to Event Sourcing without a rewrite (if domain events appear)

### Negative / Trade-offs
- **Higher initial complexity** — understanding CQRS requires discipline; it is easy to create a leaky abstraction
- **Eventual consistency** between the write model and read projections (typically seconds — acceptable for statistics/reports, to be considered in UX)
- Need for **two data models** in many areas (write model + read model)
- Cosmos Change Feed as the propagation mechanism requires a separate Function (or process) consuming changes and materializing projections
- Requires **early definition of bounded contexts** — incorrect boundaries are expensive to refactor

## Related

- [ADR-0007](0007-database-cosmos-db-free-tier.md) — NoSQL choice enables natural aggregates
- [ADR-0003](0003-web-admin-only-scope.md) — web requirements drive the need for CQRS read models
- Open decision: Event Sourcing — see [`open-decisions.md`](../architecture/open-decisions.md)
