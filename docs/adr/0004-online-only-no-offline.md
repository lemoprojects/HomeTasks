# ADR-0004: Online-only (no offline mode)

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

The mobile app can operate:
- fully offline with a local database and a synchronization mechanism,
- read-only offline (cache),
- exclusively online (requires a network connection).

Each option has significant consequences for complexity and cost.

## Decision

The app **requires an internet connection**. No local database, no synchronization engine, no conflict resolution mechanisms.

## Considered alternatives

- **Full offline with sync** — rejected. Significant increase in complexity (local storage, CRDT or similar, conflict resolution, operation queueing). Disproportionate to the typical usage scenario (a family at home on Wi-Fi).
- **Read-only offline cache** — rejected. A half-measure that does not eliminate conflicts yet still requires dedicated caching infrastructure in the app.

## Consequences

### Positive
- Drastic simplification of the mobile and backend architecture
- No synchronization problems, no conflicts
- Smaller error surface
- Push and reminders still work — push is an OS-level feature, not an app feature ([ADR-0008](0008-server-driven-push-notifications.md))

### Negative / Trade-offs
- No operations without network — acceptable for a family app, where coverage is typically available
- Poor UX if the user wants to "quickly add a task" in a place without network (e.g., basement, trip)
- The decision is **reversible**, but adding offline later requires a significant refactor of the app's state layer

## Related

- [ADR-0002](0002-mobile-react-native-expo.md) — mobile stack
- [ADR-0008](0008-server-driven-push-notifications.md) — push notifications as the mechanism for "delivering" tasks to the user
