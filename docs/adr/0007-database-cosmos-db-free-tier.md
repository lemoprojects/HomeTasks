# ADR-0007: Database — Azure Cosmos DB Free Tier (Provisioned)

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

Choice of database for an application with a DDD/NoSQL nature ([ADR-0010](0010-ddd-cqrs-patterns.md)). Requirements:

- Pay-per-use or free for MVP
- EU region (GDPR)
- Natural mapping of DDD aggregate ↔ document
- Support for the CQRS pattern (a propagation mechanism for read models)
- Consistency with the Azure ecosystem

## Decision

**Azure Cosmos DB**, **API for NoSQL**, with **Free Tier (Provisioned 1000 RU/s + 25 GB)** enabled, region **West Europe**.

## Considered alternatives

- **Cosmos DB Serverless (pay-per-RU)** — rejected for MVP. Pay-per-use, but **not strictly zero** (~$0.10–1/month for 1 family). Free Tier Provisioned yields strictly $0. Serverless kept as a migration path after the Free Tier is exhausted.
- **Azure Database for PostgreSQL Flexible Server (B1ms)** — rejected. ~€12–15/month always-on, incompatible with a $0 MVP.
- **Azure SQL Database (Serverless)** — rejected. Auto-pause works, but a relational structure contradicts the NoSQL+DDD choice.
- **MongoDB Atlas (M0 Free Tier)** — an external alternative (free 512 MB). Less consistent with the Azure ecosystem, additional provider to manage.
- **Supabase** — rejected. Introduces an external platform with bundled features (auth/storage), which we want to choose deliberately and separately, not as a package.
- **Cosmos DB API for MongoDB** — left as an open decision in [`open-decisions.md`](../architecture/open-decisions.md). By default we lean toward the native NoSQL API due to better Change Feed support (crucial for CQRS read models).

## Consequences

### Positive
- **Strictly $0** for MVP (1000 RU/s + 25 GB is huge for 1–10 families; 1000 RU/s ≈ 1000 simple operations/s)
- Natural mapping of DDD aggregate ↔ document
- Free Tier available in EU regions
- Migration to larger Provisioned or Serverless **without code changes**
- Change Feed natively supports CQRS projections

### Negative / Trade-offs
- The Free Tier is **one-time per Azure subscription** — requires checking the subscription's status before deployment (see item 7 in [`open-decisions.md`](../architecture/open-decisions.md))
- Cross-cutting queries (admin reports) require projections / read models ([ADR-0010](0010-ddd-cqrs-patterns.md)) or become RU-intensive
- API for NoSQL ≠ SQL — tooling, query language, and mental model differ from a relational database
- Provisioned RU/s is a "reservation" — even without traffic RU/s are allocated (but in the Free Tier the cost is 0, so it has no financial impact)
- Beyond the Free Tier — Provisioned may be more expensive than Serverless for uneven traffic (to be re-evaluated at that point)

## Related

- [ADR-0001](0001-multi-tenant-saas.md) — partitioning per `HouseholdId`
- [ADR-0005](0005-backend-azure-functions-consumption.md) — backend in the same cloud
- [ADR-0010](0010-ddd-cqrs-patterns.md) — pattern that requires projections
- [`cost-estimate-mvp.md`](../architecture/cost-estimate-mvp.md) — detailed calculation
