# Architecture Decision Records (ADR)

Each ADR records a single significant architectural decision, its context, and its consequences. ADRs are **immutable after acceptance** — changing a decision = a new ADR with status `Supersedes ADR-NNNN`.

## List

| # | Title | Status | Date |
|---|---|---|---|
| [0001](0001-multi-tenant-saas.md) | Multi-tenant SaaS | Accepted | 2026-05-28 |
| [0002](0002-mobile-react-native-expo.md) | Mobile: React Native + Expo | Accepted | 2026-05-28 |
| [0003](0003-web-admin-only-scope.md) | Web as administration panel only | Accepted | 2026-05-28 |
| [0004](0004-online-only-no-offline.md) | Online-only (no offline mode) | Accepted | 2026-05-28 |
| [0005](0005-backend-azure-functions-consumption.md) | Backend: Azure Functions Consumption (West Europe) | Accepted | 2026-05-28 |
| [0006](0006-scheduler-azure-functions-timer.md) | Scheduler: Azure Functions Timer Trigger | Accepted | 2026-05-28 |
| [0007](0007-database-cosmos-db-free-tier.md) | Database: Azure Cosmos DB Free Tier (Provisioned) | Accepted | 2026-05-28 |
| [0008](0008-server-driven-push-notifications.md) | Push notifications sent from the server (Expo Push) | Accepted | 2026-05-28 |
| [0009](0009-frontend-hosting-cloudflare-pages.md) | Frontend (web): Cloudflare Pages | Accepted | 2026-05-28 |
| [0010](0010-ddd-cqrs-patterns.md) | Patterns: DDD + CQRS | Accepted | 2026-05-28 |

## Status — glossary

- **Proposed** — proposed, not accepted
- **Accepted** — approved, in force
- **Deprecated** — withdrawn without replacement
- **Superseded by ADR-NNNN** — replaced by a newer decision

## Template

```markdown
# ADR-NNNN: <Short decision title>

- **Status:** Proposed | Accepted | Deprecated | Superseded by ADR-XXXX
- **Date:** YYYY-MM-DD
- **Deciders:** <names>

## Context

What problem are we solving. What constraints and forces are at play.

## Decision

What we specifically decide.

## Considered alternatives

- **A** — rejected because...
- **B** — rejected because...

## Consequences

### Positive
- ...

### Negative / Trade-offs
- ...

## Related

- ADR-XXXX
- External documents / links
```
