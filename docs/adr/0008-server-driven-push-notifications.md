# ADR-0008: Push notifications sent from the server (Expo Push)

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

Push notifications are a **key product feature** — reminders before a deadline and warnings after it is missed. A decision was needed on the delivery mechanism:

1. Server-driven — the server decides when to send and pushes to mobile via FCM/APNs
2. Client-driven — the mobile app runs a scheduler in the background and publishes local notifications itself

## Decision

**Server-driven push** via the **Expo Push Service** (a proxy to FCM for Android and APNs for iOS).

The mobile app is a **thin client**:
- Registers its push token with the backend after sign-in
- Receives push via the OS (works even when the app is closed)
- Displays a system notification

All scheduling is on the server side ([ADR-0006](0006-scheduler-azure-functions-timer.md)).

## Considered alternatives

- **Client-driven (the app maintains a background scheduler itself)** — rejected:
  - Requires an always-on background process — high battery consumption
  - Conflicts with store policies (Google/Apple restrict background tasks)
  - Doesn't work when the app is closed (killed by the system)
  - Significantly complicates the mobile code
- **Direct FCM/APNs from the backend (without Expo Push)** — possible, but requires separate handling of Android and iOS tokens, integration with two SDKs, and managing APNs certificates. Expo Push simplifies this. Kept as a future migration path should we ever drop Expo.

## Consequences

### Positive
- The mobile app stays **thin and simple** — minimal local logic
- Push works **even when the app is closed** (OS-level delivery via FCM/APNs)
- Changing reminder logic = backend deploy, **without an app update in the stores**
- Expo Push handles Android and iOS with a single API, including token exposure
- Free service, unlimited push

### Negative / Trade-offs
- Requires an **Apple Developer Program** account ($99/year) for production iOS push
- Requires a **Google Play Console** account ($25 one-time) for Android
- Dependency on the **Expo Push Service** (third-party, free) — if it becomes unavailable, the fallback requires implementing FCM/APNs directly
- The backend must store and manage user push tokens (rotation, deactivation on sign-out)
- No delivery guarantee (push is best-effort by nature — there is never an SLA on arrival)

## Related

- [ADR-0002](0002-mobile-react-native-expo.md) — mobile stack
- [ADR-0006](0006-scheduler-azure-functions-timer.md) — what triggers the push send
- [ADR-0004](0004-online-only-no-offline.md) — online-only does not block push (push is delivered by the OS)
