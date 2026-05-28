# MVP Cost Estimate

- **As of:** 2026-05-28
- **Status:** Planning estimate (to be verified after the first deployment)
- **Baseline assumptions:** 1 test family, ~5 users, ~100 operations/day

## Monthly costs (Azure + Cloudflare + Expo)

| Component | Plan | Free Tier | Expected MVP cost |
|---|---|---|---|
| Azure Functions (API) | Consumption | 1,000,000 invocations/month + 400,000 GB-s | **$0** |
| Azure Functions (Timer) | Consumption | counts toward the same limit | **$0** |
| Azure Cosmos DB | Free Tier (Provisioned) | 1000 RU/s + 25 GB **forever** | **$0** |
| Azure Storage Account (required by Functions) | Standard LRS | none | ~$0.05–0.50 |
| Application Insights (logs/telemetry) | Pay-as-you-go | 5 GB/month | **$0** |
| Cloudflare Pages (web frontend) | Free | unlimited static + 500 builds/month | **$0** |
| Cloudflare DNS / WAF / DDoS | Free | sufficient | **$0** |
| Expo Push Service | Free | unlimited push | **$0** |
| **TOTAL** | | | **~$0.05–0.50 / month** |

## Annual / one-time costs (outside the cloud)

| Item | Cost | Notes |
|---|---|---|
| Apple Developer Program | $99/year | Required to publish on the App Store and for full iOS push |
| Google Play Console | $25 one-time | Required to publish on Google Play |
| Domain (e.g., `.pl`, `.com`) | ~$10–30/year | Optional for MVP, but useful for the web admin |
| **Year 1 total** | **~$135–155** | + $25 one-time |

## Assumptions underlying $0/month on Azure

1. **Cosmos DB Free Tier** is **one-time per Azure subscription**. Requires checking that it is not already used by another resource in the subscription.
2. **1M Functions invocations/month** is ~33,000/day. A per-minute timer = ~43,000/month. API for 5 users × 100 ops/day = ~15,000/month. Combined: ~58,000/month — far from the limit.
3. **1000 RU/s** in Cosmos = ~1000 simple operations/s. For one family we will use fractional RU/s on average.
4. **25 GB** of storage — enough for years of task data for several families.

## When costs will start to grow

| Limit | Approximate threshold | What happens |
|---|---|---|
| Cosmos DB Free Tier (1000 RU/s) | ~50+ active families | Migrate to Provisioned 4000 RU/s (~$24/month) or Serverless (pay-per-RU) |
| Cosmos DB storage (25 GB) | Very long (years) | ~$0.25/GB/month surcharge above the limit |
| Functions Consumption (1M invocations/month) | ~100+ active families | $0.20 per additional 1M invocations — still pennies |
| Cold start becomes a UX blocker | Case by case | Migrate to Functions Premium (~$160/month) or Container Apps with `min replicas=1` (~$30/month) |

## Indicative scaling paths

| Stage | Architecture | Estimated cost / month |
|---|---|---|
| **MVP (1–10 families)** | Free Tier everywhere, Functions Consumption + Cosmos Free Tier | **$0–5** |
| **Growth (10–100 families)** | Cosmos Serverless or Provisioned 4k RU/s, still Functions Consumption | **$10–50** |
| **Scale (100–1000 families)** | Functions Premium, Cosmos Provisioned with autoscale, Application Gateway, dedicated analytics database | **$200–800** |

> **Note:** the values above are INDICATIVE and require verification against the actual traffic pattern, data size, and SLA requirements. Full calculation in the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) once non-functional requirements are clarified.

## Cost risks to monitor

- **Storage Account for Functions** — may grow with high log throughput; monitor via Cost Management.
- **Application Insights** — exceeding 5 GB/month after enabling detailed telemetry; set up sampling.
- **Cosmos cross-partition queries** (admin reports) — RU-intensive; mitigated by CQRS read models (see [ADR-0010](../adr/0010-ddd-cqrs-patterns.md)).
- **Expo Push** — if we ever want to drop Expo, push via our own FCM/APNs requires additional infrastructure, but Expo Push remains free.
