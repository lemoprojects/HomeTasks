# Personas

- **Status:** Draft v1 (Phase 1, Deliverable D3)
- **Date:** 2026-05-28
- **Owner:** Piotr Glowacki

## Purpose

This document identifies the people who use HomeTasks and what they care about. Personas drive every subsequent Phase 1 deliverable: user stories (D4), glossary terms (D2), use cases (D5), and the permissions matrix (D6).

Personas are derived from:

- [ADR-0001](../adr/0001-multi-tenant-saas.md) — multi-tenant SaaS with one global operator and per-household administrators
- [ADR-0003](../adr/0003-web-admin-only-scope.md) — web is admin-only; mobile is the operational client
- [ADR-0008](../adr/0008-server-driven-push-notifications.md) — push notifications are delivered to mobile only
- [Phase 1 plan](../../plans/phase-1-requirements.md) — candidate personas listed in §6

## Persona summary

| Persona | Adult/child | Primary device | Frequency of use | In MVP |
|---|---|---|---|---|
| [Household Owner](#household-owner) | Adult | Mobile (daily) + Web (occasional) | Daily | Yes |
| [Adult Member](#adult-member) | Adult | Mobile | Daily | Yes |
| [Child Member](#child-member) | Child | Mobile | Daily | Yes (confirmed 2026-05-28) |
| [SaaS Operator](#saas-operator) | Adult (internal) | Web | On demand | Yes (minimal scope) |

---

## Household Owner

The adult who signs up, creates the household, and is accountable for keeping it organized. Typically a parent in a family setting. There is exactly one Owner per household at any time (ownership transfer is post-MVP — see open question OQ-3).

### Goals

- Set up the household once and onboard the rest of the family with minimal friction
- See, at a glance, what is happening across all members today and this week
- Make sure recurring chores (cleaning, school prep, bills, medication) actually happen without having to remind people in person
- Get insight into who does how much — for fairness, for coaching kids, and for catching things falling through the cracks
- Manage household membership (invite, remove, change roles) as life changes

### Frustrations

- Tasks fall through the cracks because nobody owns them
- Having to verbally nag family members about repeating chores
- No shared, single source of truth for "what needs doing"
- Hard to tell if a child actually did the task or just clicked the button

### Primary device

- **Mobile** for daily operations: creating tasks, checking status, receiving and responding to reminders
- **Web** for administrative work: configuring the household, managing members and roles, reading statistics and reports (per [ADR-0003](../adr/0003-web-admin-only-scope.md))

### Frequency of use

- Mobile: multiple times per day
- Web: weekly or less; longer sessions when they happen

### Distinguishing capabilities (informs D6 permissions matrix)

- Full read/write across the household
- Manage members and roles
- Access statistics and reports
- The only persona who can delete the household (subject to confirmation in D6)

---

## Adult Member

A second adult in the household — typically a partner or co-parent. Not the household creator, but operates with similar autonomy day-to-day. There can be zero or more Adult Members per household.

### Goals

- Know what is expected of them today without having to ask the Owner
- Add and assign tasks (e.g. "I'll handle groceries, you handle pickup") without needing the Owner's permission
- Complete tasks quickly on mobile, including from outside the home
- Not be reminded about things that are not theirs

### Frustrations

- Being treated as a subordinate rather than an equal in the household
- Receiving reminders for tasks they did not agree to
- Lack of visibility into what the Owner has set up

### Primary device

- **Mobile only** (web access scope for Adult Member is TBD — see OQ-4)

### Frequency of use

- Multiple times per day

### Distinguishing capabilities (informs D6)

- Create, assign, and complete tasks
- See full household task list
- Likely cannot manage membership or delete the household (to be confirmed in D6)

---

## Child Member

A child in the household. Confirmed as a **full MVP persona** on 2026-05-28. The exact age range and identity model are still open (see OQ-1, OQ-2). The product assumes the child is old enough to operate a mobile device and understand a task list; toddlers are out of scope.

### Goals

- See clearly what they need to do today
- Get credit for completing tasks (positive reinforcement)
- Use an interface that does not require reading dense text or making complex decisions

### Frustrations

- Reminders at inconvenient times (school, sleep)
- Being asked to do things outside their normal scope
- Being unable to mark a task done because the UI is too complicated

### Primary device

- **Mobile only**

### Frequency of use

- Daily, but in shorter bursts than adults — typically opening the app in response to a reminder

### Distinguishing capabilities (informs D6)

- Read assigned tasks
- Mark assigned tasks as complete
- Likely **cannot**: create tasks for others, manage members, see statistics across the household (to be confirmed in D6)
- Likely **cannot**: reassign tasks (subject to D6)

### MVP implications

Having Child as a full MVP persona creates obligations in later deliverables:

- **D2 (Glossary):** distinguish *User* (identity) from *Member* (household role), so a Child can be modelled either way once OQ-1 is resolved
- **D6 (Permissions matrix):** Child needs a restricted row distinct from Adult Member
- **D7 (Non-functional):** age-appropriate UX constraints (reading level, iconography, simplified interactions) — note: regulatory posture (GDPR, child-data laws) is **deferred for now** by explicit decision on 2026-05-28; revisit before any public launch
- **Open decision #6** (user identification): the choice of per-person account vs. shared email + profiles is heavily influenced by this persona — flag explicitly when resolving it

---

## SaaS Operator

The person (initially the project owner) who administers the service across **all** households. This is a single global role, not per-household, and corresponds to the "one global operator" stated in [ADR-0001](../adr/0001-multi-tenant-saas.md).

### Goals

- Keep the service healthy: detect incidents, triage errors, respond to support requests
- Understand usage at the tenant level: how many active households, how many active users, push delivery health
- Enforce policy: respond to abuse reports, handle deletion requests, manage subscriptions when monetization exists
- Operate the service without ever needing to look at individual household task content unless explicitly authorized (privacy boundary — see D7)

### Frustrations

- Lack of cross-tenant visibility (the multi-tenant model requires deliberate operator tooling)
- Having to choose between "see nothing" and "see everything", with no middle ground
- Troubleshooting blind when a user reports an issue

### Primary device

- **Web** (admin panel) only — there is no mobile experience for the Operator

### Frequency of use

- On demand: occasional in a healthy state, intense during incidents

### Distinguishing capabilities (informs D6)

- Cross-tenant administrative views (counts, health metrics)
- **Not** automatic access to household content; access to a specific household's content requires explicit authorization (mechanism TBD in D6 / D7)
- Manage the service itself: maintenance windows, feature flags, support actions
- MVP scope is **minimal**: enough to operate the service safely, not a full back-office product (see OQ-5)

---

## Open questions

Tracked here so they are not lost. Each must be resolved before D3 can be considered final.

- **OQ-1 — Child identity model.** Does a Child Member have their own account/email, or are they a profile under a parent's account? This is [open decision #6](../architecture/open-decisions.md) and must be resolved before D2/D6 stabilize.
- **OQ-2 — Child age range.** What minimum and maximum age does HomeTasks design for? Affects UI complexity (reading level, iconography). Regulatory implications (child-data laws) are deferred — see decision recorded on 2026-05-28.
- **OQ-3 — Household ownership transfer.** Is there exactly one Owner per household? Can ownership be transferred? Assumed "one Owner, no transfer in MVP" — to be confirmed.
- **OQ-4 — Adult Member web access.** Does an Adult Member have any web access (e.g. view statistics), or is web strictly Owner + Operator? [ADR-0003](../adr/0003-web-admin-only-scope.md) defines web as admin-only but does not say *which* admins.
- **OQ-5 — Operator scope in MVP.** What is the minimum Operator tooling required to ship MVP safely? Listing tenants? Reading logs? Suspending an abusive tenant? To be scoped against D7 (observability) and D8 (MVP scope).
- **OQ-6 — Is the persona list complete?** Candidates considered but not adopted: *Invited-but-not-onboarded Member* (treated as a state, not a persona), *Guest/Babysitter* (out of MVP), *Extended family* (out of MVP). To be confirmed by the user.

## Definition of done for D3

Per the [Phase 1 plan](../../plans/phase-1-requirements.md) §3:

- [x] Personas list identified (4 personas above)
- [x] Each persona has: goals, frustrations, primary device, frequency of use
- [ ] User confirmed list is complete