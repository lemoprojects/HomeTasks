# ADR-0003: Web as administration panel only

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

We need to decide the scope of responsibility of the web app. The mobile app is user-centric and operational. The web could duplicate its features (adding tasks from a computer) or focus exclusively on administration.

## Decision

The web app handles **only**:

- Household configuration (family members, roles, relationships)
- Task completion statistics and reports
- Global administration (for the SaaS operator)

The web app **does not handle**:

- Day-to-day adding or editing of tasks (mobile does that)
- Reminder reception (push is delivered only to mobile)

## Considered alternatives

- **Full-featured web (admin + task management)** — rejected. UI duplication, scope growth, pressure to maintain consistency in two places.
- **Decide after a UX prototype** — rejected because the user expressed a clear preference at the requirements stage.

## Consequences

### Positive
- Clear separation of responsibilities: mobile = operations, web = management
- Significantly smaller web UI scope to build
- No UX logic duplication between platforms

### Negative / Trade-offs
- A parent who wants to add a task for a child from a computer must reach for the phone (to be considered in UX requirements — may turn out to be acceptable or may require a response)
- The decision is **reversible** in the future — the backend API exposes the same endpoints for both clients, so adding task features in the web requires only UI

## Related

- [ADR-0002](0002-mobile-react-native-expo.md) — mobile as the main operational client
- Open decision in [`open-decisions.md`](../architecture/open-decisions.md) — possible revision after MVP
