# Technology Stack

- **As of:** 2026-05-28
- **Status:** Approved for MVP
- **Scope:** Stack decisions for the first iteration (MVP — test deployment for 1 family)

## Short version

| Layer | Technology | Hosting | Location |
|---|---|---|---|
| Mobile | React Native + Expo | App Store / Google Play (deploy: EAS) | — |
| Web (admin) | React | Cloudflare Pages | edge |
| Backend API | ASP.NET (.NET 8/9 Isolated Worker) | Azure Functions Consumption | West Europe |
| Scheduler | Azure Functions Timer Trigger | same Function App | West Europe |
| Database | Azure Cosmos DB (Free Tier Provisioned, API for NoSQL) | Azure | West Europe |
| Push notifications | Expo Push (FCM/APNs proxy), server-driven | — | — |
| Auth | Local (email + password) + Social login | TBD provider | — |
| DNS / CDN / WAF | Cloudflare | edge | — |
| Architectural patterns | DDD + CQRS | — | — |

## Application character

- **Mobile** = thin client, user-centric. Displays scheduled tasks, receives push, allows easy task creation for other family members once the household is configured.
- **Web** = administration only: managing users, families, roles, statistics, reports. **Does not** handle day-to-day task creation or editing.
- **Backend** = a single API serving both clients, serverless pay-per-use, multi-tenant SaaS.
- **Scheduler** = built into the backend (Functions timer trigger) — sends push before and after a deadline.

## Stack decisions (references to ADRs)

| Area | ADR |
|---|---|
| Multi-tenancy | [ADR-0001](../adr/0001-multi-tenant-saas.md) |
| Mobile technology choice | [ADR-0002](../adr/0002-mobile-react-native-expo.md) |
| Web app scope | [ADR-0003](../adr/0003-web-admin-only-scope.md) |
| No offline mode | [ADR-0004](../adr/0004-online-only-no-offline.md) |
| Backend hosting | [ADR-0005](../adr/0005-backend-azure-functions-consumption.md) |
| Scheduler | [ADR-0006](../adr/0006-scheduler-azure-functions-timer.md) |
| Database | [ADR-0007](../adr/0007-database-cosmos-db-free-tier.md) |
| Push mechanism | [ADR-0008](../adr/0008-server-driven-push-notifications.md) |
| Frontend hosting | [ADR-0009](../adr/0009-frontend-hosting-cloudflare-pages.md) |
| Architectural patterns | [ADR-0010](../adr/0010-ddd-cqrs-patterns.md) |

## Open stack decisions

See [`open-decisions.md`](open-decisions.md). Most important:

- Social login provider (in-house ASP.NET Identity vs. Auth0 / Clerk / Supabase Auth)
- Cosmos DB API: for NoSQL vs. for MongoDB
- Event Sourcing — yes or no
- Specific .NET version (8 vs. 9) — to be confirmed after checking Functions Isolated Worker support
