# Open Decisions

List of decisions not yet resolved. Each will be closed by a new ADR when it becomes required for implementation.

- **As of:** 2026-05-28

## Open

### 1. Auth: in-house or external provider

- **Options:** ASP.NET Core Identity (in-house) / Auth0 / Clerk / Supabase Auth
- **Criteria:** free tier for MVP, social login integration (Google/Apple/Microsoft), ease of migration
- **Note:** regulatory criteria (GDPR, data residency) are deferred on 2026-05-28 and not weighed here for now; revisit before public launch
- **Decision expected:** before implementing the Auth module
- **Status:** TBD — in-house ASP.NET Core Identity seems safest long-term, but requires more code

### 2. Cosmos DB API: for NoSQL vs. for MongoDB

- **Options:** native Cosmos DB API (`API for NoSQL`) vs. MongoDB-compatible API
- **Criteria:**
  - .NET ecosystem (both have official SDKs; the NoSQL API is native)
  - Possibility of migrating to/from MongoDB Atlas in the future (MongoDB API makes this easier)
  - Transactional batch and Change Feed (better support in the NoSQL API)
- **Decision expected:** before the first persistence implementation
- **Status:** By default we lean toward **API for NoSQL** — native to Cosmos, best Change Feed support (crucial for CQRS projections)

### 3. Event Sourcing — yes or no

- **Options:** full ES / domain events only without ES / no events
- **Criteria:** whether we have a real use case for change audit / replay; cost of complexity
- **Decision expected:** after bounded contexts are defined (Phase 1/3)
- **Status:** By default **no Event Sourcing** in MVP, but the design allows evolution; domain events may appear as a mechanism for propagating to read models

### 4. .NET version (8 vs. 9)

- **Criteria:** support in Azure Functions Isolated Worker as of June 2026, LTS stability
- **Decision expected:** before the first backend scaffolding
- **Status:** By default **the latest stable version supported by Functions Isolated Worker** — to be confirmed against Microsoft documentation at implementation start

### 5. Multi-region disaster recovery

- **Question:** does MVP require DR beyond West Europe?
- **Default:** NO in MVP. Cosmos DB Free Tier does not support multi-region.
- **Decision expected:** when leaving MVP

### 6. User identification within a family

- **Question:** Does each family member have their own email/account, or is a shared email used with profiles?
- **Impact:** data model (User vs. Profile), auth flow, push delivery
- **Decision expected:** in Phase 1 (functional requirements)

### 7. Whether we have an unused Cosmos DB Free Tier in the target Azure subscription

- **Operational question:** does the Azure subscription where we'll deploy MVP still have an unused entitlement to create a Free Tier Cosmos resource
- **Impact:** if used → we switch to Serverless (~$0.10–1/month) or look for another subscription
- **Decision expected:** before creating the Azure resources

### 8. Push notification format and content

- **Question:** What types of notifications (before deadline? how long before? after deadline? escalation?), what channels besides push (email? SMS?)
- **Decision expected:** in functional requirements (Phase 1)

## Closed (moved to ADRs)

> List of closures updated as new ADRs are created.

- Multi-tenant SaaS → [ADR-0001](../adr/0001-multi-tenant-saas.md)
- Mobile React Native + Expo → [ADR-0002](../adr/0002-mobile-react-native-expo.md)
- Web admin-only → [ADR-0003](../adr/0003-web-admin-only-scope.md)
- Online-only → [ADR-0004](../adr/0004-online-only-no-offline.md)
- Backend Azure Functions Consumption → [ADR-0005](../adr/0005-backend-azure-functions-consumption.md)
- Scheduler = Functions Timer Trigger → [ADR-0006](../adr/0006-scheduler-azure-functions-timer.md)
- Database Cosmos DB Free Tier → [ADR-0007](../adr/0007-database-cosmos-db-free-tier.md)
- Server-driven push → [ADR-0008](../adr/0008-server-driven-push-notifications.md)
- Frontend hosting Cloudflare Pages → [ADR-0009](../adr/0009-frontend-hosting-cloudflare-pages.md)
- DDD + CQRS → [ADR-0010](../adr/0010-ddd-cqrs-patterns.md)
- Defer regulatory posture (GDPR, child-data laws) → [ADR-0011](../adr/0011-defer-regulatory-posture.md)
