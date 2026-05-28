# ADR-0001: Multi-tenant SaaS

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

HomeTasks is ultimately intended to serve many independent families (households). A decision was needed on the deployment model: one shared backend serving all families vs. separate instances per household (self-hosted).

## Decision

A **multi-tenant SaaS** architecture:

- One backend, one database
- Data isolation at the `HouseholdId` level (partition key in Cosmos DB)
- One global operator (SaaS administrator) + per-family administrators

## Considered alternatives

- **Self-hosted per family** — rejected. High operational cost for end users, no economies of scale, low monetization potential.
- **Hybrid (self-hosted first, SaaS later)** — rejected because it requires making the architectural decision twice and creates pressure for a refactor.

## Consequences

### Positive
- Economies of scale — all users share the cost of the infrastructure
- Central handling of updates, security, and monitoring
- Future monetization potential (subscriptions)
- A single backend to maintain

### Negative / Trade-offs
- Data isolation must be designed from day one (every Cosmos DB collection partitioned per `HouseholdId`, every query must include a tenant filter)
- Backups and operations must be tenant-aware
- A backend outage = an outage for all users (requires SLA and monitoring)
- GDPR compliance required from the start (processing data of many people)

## Related

- [ADR-0005](0005-backend-azure-functions-consumption.md) — backend hosting
- [ADR-0007](0007-database-cosmos-db-free-tier.md) — database that requires per-tenant partitioning
