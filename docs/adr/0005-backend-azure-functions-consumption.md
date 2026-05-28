# ADR-0005: Backend — Azure Functions Consumption (West Europe)

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

Choice of hosting for the .NET backend. Requirements:

- Multi-tenant SaaS ([ADR-0001](0001-multi-tenant-saas.md))
- **Pay-per-use** — cost grows with usage, no fixed charges for idleness
- EU region (GDPR)
- Low cost barrier for MVP (1 test family)
- Native support for .NET / ASP.NET

**Cloudflare**, considered earlier, was dropped — Workers/Pages don't host the CLR runtime for .NET.
A **self-hosted NAS + Cloudflare Tunnel** option, considered earlier, was dropped — inconsistent with the multi-tenant SaaS model (SLA, availability, scaling).

## Decision

**ASP.NET (.NET 8/9 Isolated Worker)** on the **Azure Functions Consumption plan**, region **West Europe**.

HTTP-trigger functions expose a REST API for the mobile and web apps.

## Considered alternatives

- **Cloudflare Workers** — rejected. The runtime does not support CLR / .NET.
- **Self-hosted NAS + Cloudflare Tunnel** — rejected. Incompatible with SaaS (no SLA, home connectivity/power outages would affect all tenants).
- **AWS Lambda (.NET)** — comparable, but the Microsoft ecosystem is the native home for .NET; lower learning curve.
- **Azure Container Apps consumption** — rejected in favor of Functions. Functions has a 1M invocations/month free tier and Timer Trigger inside the same app ([ADR-0006](0006-scheduler-azure-functions-timer.md)) — eliminating the need for a separate scheduler component.
- **Azure App Service** — rejected. Requires an always-on plan (B1 ~€12–15/month, F1 has 60 min CPU/day). Incompatible with pay-per-use.
- **Functions Premium plan** — rejected for MVP (~€160/month). Kept as a migration path once cold start becomes a blocker.

## Consequences

### Positive
- ~$0/month for MVP (1M Functions Consumption invocations in the free tier)
- Native .NET — best DX, integration with Visual Studio / Rider
- EU region = GDPR compliance
- Pay-per-use — cost scales with usage
- Easy migration path to the Premium plan without code changes

### Negative / Trade-offs
- **Cold start ~1–3 seconds** for .NET on the Consumption plan. Acceptable for MVP, but to be monitored for mobile UX impact.
- A **Storage Account** is required by Functions (a few cents/month) — the only fixed charge.
- The **Isolated Worker** programming model differs from classic ASP.NET (small differences, but awareness is required).
- Maximum length of a single invocation: 5 minutes (Consumption). Sufficient for an API, not for batch jobs.

## Related

- [ADR-0001](0001-multi-tenant-saas.md)
- [ADR-0006](0006-scheduler-azure-functions-timer.md) — scheduler in the same Function App
- [ADR-0007](0007-database-cosmos-db-free-tier.md) — database in the same cloud
- [`cost-estimate-mvp.md`](../architecture/cost-estimate-mvp.md) — detailed calculation
