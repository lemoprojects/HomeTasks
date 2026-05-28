# ADR-0002: Mobile — React Native + Expo

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

The mobile app is the main channel for the end user: it displays scheduled tasks, receives push notifications (before the deadline and after it is missed), and lets users easily add tasks to other family members once the household has been configured in the web panel.

Push notifications are a **key** product feature — the reliability of their delivery is critical.

We don't need code sharing with the web app — the web is a dedicated admin panel with completely different UX and scope ([ADR-0003](0003-web-admin-only-scope.md)).

## Decision

**React Native + Expo**, deployed via **EAS** (Expo Application Services) to the App Store and Google Play.

Push is handled via the **Expo Push Service** (an FCM/APNs proxy) — see [ADR-0008](0008-server-driven-push-notifications.md).

## Considered alternatives

- **PWA (Progressive Web App)** — rejected. Push on iOS is only available from 16.4 and works reliably only from an app added to the home screen. Too risky for a key product feature.
- **Native (Kotlin + Swift)** — rejected. Two codebases, double the maintenance cost, disproportionate for a family-oriented app.
- **.NET MAUI** — rejected. Language consistency with the backend (C#) is tempting, but the MAUI mobile ecosystem is less mature than RN, with a smaller community and weaker push libraries.

## Consequences

### Positive
- Single codebase for Android + iOS
- Full push notifications via FCM/APNs out of the box (through Expo)
- EAS simplifies builds, signing, and submission to the stores
- Large community, mature stack
- Hot reload speeds up development

### Negative / Trade-offs
- Codebase independent from the web (a deliberate choice — the two clients have different scope)
- Dependency on the Expo SDK (mitigation: Expo allows ejecting to a bare workflow)
- TypeScript instead of C# in the mobile stack (requires a separate skill set)
- Requires an Apple Developer Program account ($99/year) and Google Play Console ($25 one-time) for publishing

## Related

- [ADR-0003](0003-web-admin-only-scope.md) — web scope explains why we don't share code
- [ADR-0008](0008-server-driven-push-notifications.md) — push delivery mechanism
