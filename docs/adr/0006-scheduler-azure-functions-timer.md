# ADR-0006: Scheduler — Azure Functions Timer Trigger

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

The app requires reliable delivery of push notifications at specific times:

- Reminders before a task deadline
- Warnings after the deadline is missed
- Recurring notifications

The backend runs serverless (scales to zero, [ADR-0005](0005-backend-azure-functions-consumption.md)), so a scheduler **inside a dormant process** will not work — there is nothing to wake "from within."

## Decision

The scheduler is implemented as an **Azure Functions Timer Trigger** inside the same Function App as the API.

Mechanism: a cron-triggered function periodically (e.g., every minute) queries the database for tasks whose notifications should be sent and publishes push via the Expo Push Service.

## Considered alternatives

- **Hangfire / Quartz.NET in an ASP.NET process** — rejected. Requires an always-on backend (Container Apps with `min replicas=1` or App Service), which eliminates pay-per-use.
- **Cloudflare Workers Cron triggering the Functions endpoint** — works, but introduces an external dependency and complicates the architecture.
- **Azure Logic Apps with a schedule trigger** — overkill, higher cost, worse DX for business logic in C#.
- **Azure Container Apps Jobs (cron jobs)** — rejected along with Container Apps in favor of Functions.
- **Azure Service Bus Scheduled Messages** — elegant, but requires a separate component (Service Bus, ~€10/month Basic) and does not fit a free-tier MVP.

## Consequences

### Positive
- **No additional component** — the timer lives in the same Function App as the API
- Pay-per-execution — a cron every minute = ~43,200 invocations/month, well within the 1M free tier
- Wakes the Function App "from within" — solves the scale-to-zero problem (the instance comes up on its own on the timer trigger)
- Easy schedule changes (application code, deploy)

### Negative / Trade-offs
- Timer Trigger on the Consumption plan can have small delays (on the order of seconds) — acceptable for reminders, would be unacceptable for an app with hard timing SLAs.
- Singleton: only **one instance** of the timer trigger runs at a time (Azure ensures this via storage lease) — for us this is a plus, not a minus.
- The "poll every minute" pattern can be suboptimal at large scale. Scalable alternative: in the future, migrate to **Service Bus Scheduled Messages** or a delayed-queue mechanism.

## Related

- [ADR-0005](0005-backend-azure-functions-consumption.md)
- [ADR-0008](0008-server-driven-push-notifications.md) — what the timer publishes
