# Phase 1 — Functional Requirements (Continuation Plan)

- **Created:** 2026-05-28
- **Owner:** Piotr Glowacki
- **Purpose:** Hand-off document so work on HomeTasks can resume in a fresh Claude Code session without losing context.

---

## 1. What this project is

HomeTasks is a **multi-tenant SaaS** that lets households schedule tasks for their members, deliver mobile push reminders before and after deadlines, and track completion statistics. Two clients consume one backend:

- **Mobile app** (React Native + Expo) — user-facing, push-driven. Used by every household member daily.
- **Web app** (React on Cloudflare Pages) — administration only: household setup, users, roles, statistics, reports. **Not** used for daily task creation or reminders.

Backend is ASP.NET on Azure Functions Consumption (West Europe), with Cosmos DB Free Tier as the data store. DDD + CQRS as architectural style.

---

## 2. Where we are (Phase 0: Discovery + ADRs — DONE)

Completed in session of 2026-05-28:

- 10 Architecture Decision Records in [`docs/adr/`](../docs/adr/) covering: multi-tenancy, mobile stack, web scope, online-only, backend hosting, scheduler, database, push delivery, frontend hosting, DDD+CQRS
- Tech stack documented in [`docs/architecture/tech-stack.md`](../docs/architecture/tech-stack.md)
- MVP cost estimate (~$0.05–0.50/mc) in [`docs/architecture/cost-estimate-mvp.md`](../docs/architecture/cost-estimate-mvp.md)
- 8 open decisions tracked in [`docs/architecture/open-decisions.md`](../docs/architecture/open-decisions.md)
- Memory snapshot saved (stack, language convention, cost discipline, ADR-first workflow)

**Status of working tree:** uncommitted. Documentation needs `git add` + commit before next phase. User's permission required per CLAUDE.md.

---

## 3. What Phase 1 must produce

Phase 1 turns the *idea* into *something concrete enough to design and build*. It is **content**, not code.

### Deliverables (top-level progress)

Tick the box when the file is **drafted and reviewed by the user**. A merely-scaffolded file does not count as done.

- [ ] **D1.** `docs/00-vision.md` — Product vision, problem statement, target outcomes, success criteria
- [ ] **D2.** `docs/01-glossary.md` — Ubiquitous language (Household, Member, Task, Reminder, Assignment, Schedule, Recurrence, Role, Completion, …)
- [ ] **D3.** `docs/02-requirements/personas.md` — Personas: who uses the system and what they care about
- [ ] **D4.** `docs/02-requirements/user-stories.md` — User stories per persona, grouped by capability
- [ ] **D5.** `docs/02-requirements/use-cases.md` — Detailed flows for critical paths
- [ ] **D6.** `docs/02-requirements/permissions-matrix.md` — Roles × operations matrix
- [ ] **D7.** `docs/02-requirements/non-functional.md` — Scale, GDPR, accessibility, language, availability

### Sub-checklists (granular progress)

**D3 — Personas**
- [ ] Personas list identified (candidates: Household Owner, Adult Member, Child Member, SaaS Operator)
- [ ] Each persona has: goals, frustrations, primary device, frequency of use
- [ ] User confirmed list is complete

**D4 — User stories**
- [ ] Stories drafted per persona
- [ ] Stories grouped by capability (e.g. "Manage household", "Plan tasks", "Receive reminders", "Report progress")
- [ ] Each story has acceptance criteria
- [ ] MVP scope marked (which stories are in / out for first release)

**D5 — Use cases (target: 4–6 critical flows)**
- [ ] Onboard new household (sign up → create household → invite members)
- [ ] Create recurring task and assign to member
- [ ] Receive and act on a reminder (push → open app → mark complete)
- [ ] View family statistics in web admin
- [ ] Handle missed deadline (escalation logic)
- [ ] Add/remove household member

**D6 — Permissions matrix**
- [ ] Roles enumerated and named
- [ ] Operations enumerated
- [ ] Matrix filled
- [ ] Edge cases noted (e.g. can a Child reassign? can a Member remove the Owner?)

**D7 — Non-functional**
- [ ] Scale assumptions (target households / users / tasks per household)
- [ ] GDPR posture (data residency confirmed EU, retention policy, right to deletion)
- [ ] Language support (Polish first; English next?)
- [ ] Accessibility minimum (WCAG level?)
- [ ] Availability target (best-effort vs SLA)

### Open decisions to resolve during Phase 1

- [ ] **Decision #6** — User identification within a household (per-person email/account vs shared email + profiles). Resolution → new ADR + update `open-decisions.md`.
- [ ] **Decision #8** — Notification format & channels (reminder types, push-only vs also email/SMS). Resolution → new ADR + update `open-decisions.md`.

### What Phase 1 does NOT do

- Domain modeling (aggregates, entities) — that's Phase 3
- API design (OpenAPI) — Phase 3
- Wireframes / UX — Phase 2
- Database schema — Phase 3
- Any code

---

## 4. Open decisions that block Phase 1

These must be resolved during Phase 1 because they shape the requirements themselves:

- **Open decision #6 (`open-decisions.md`)** — User identification within a household: per-person email/account vs shared email + profiles. Blocks: persona definition, User vs Member modeling, auth flow.
- **Open decision #8 (`open-decisions.md`)** — Notification format and channels: how many reminder types (before/after/escalation)? Push only or also email/SMS? Blocks: Reminder concept in glossary.

Other open decisions (Auth provider, Cosmos API choice, .NET version, ES yes/no) can wait until Phase 3.

---

## 5. Recommended Phase 1 workflow

1. Start with **personas** — without users, there are no requirements. Likely candidates:
   - Household Owner (adult, organizer, sets up the family)
   - Adult Member (partner — uses mobile, can assign tasks)
   - Child Member (kid — uses mobile, mostly receives and completes tasks; may need restricted UI)
   - SaaS Operator (us — administers across all households)
2. From personas, derive **user stories** in "As a … I want … so that …" format.
3. From user stories, extract **glossary terms** (every noun is a candidate domain concept).
4. Resolve **open decisions #6 and #8** in parallel — they will come up naturally.
5. Build the **permissions matrix** once personas and operations are stable.
6. Write **use cases** for 4–6 most critical flows (don't try to cover everything).
7. Capture **non-functional** constraints as they surface (GDPR is already known; scale targets need user input).

Each deliverable should follow the same discipline as ADRs: write what is decided, mark TBDs explicitly, never invent.

---

## 6. How to resume in a new session

Suggested first prompt for a fresh Claude Code session:

```
Continuing the HomeTasks project. Please read in this order:
  1. CLAUDE.md (project working agreement)
  2. docs/README.md (docs index)
  3. docs/architecture/tech-stack.md and cost-estimate-mvp.md
  4. docs/architecture/open-decisions.md
  5. All ADRs in docs/adr/ (numbered 0001–0010)
  6. plans/phase-1-requirements.md (this file)

Memory (MEMORY.md) already has the key context: project stack, language
convention (Polish for conversation/docs, English for code and this plans/
folder), cost discipline, and the "plan-before-implementation" workflow.

We are starting Phase 1: Functional Requirements. Begin with personas.
Ask me questions about user identification (open decision #6) and
notification types (open decision #8) as they come up — do not assume.
```

---

## 7. Conventions to keep

- **Language**: project docs in `docs/` are in **Polish**; this `plans/` folder is in **English** (per user request); code, identifiers, commit messages in English.
- **ADRs are immutable** after acceptance — changes become new ADRs marked `Supersedes ADR-NNNN`.
- **No hallucination** — explicit TBDs over invented details.
- **Plan / ADR before implementation** — per CLAUDE.md and the user's repeated emphasis.
- **Verify before claiming "free"** — Cosmos DB Free Tier is one-per-subscription, Functions Consumption free tier has limits; always state the qualifier.
- **Commits require explicit user permission** per CLAUDE.md — never push without asking.

---

## 8. Phase roadmap (reminder)

| Phase | Output | Status |
|---|---|---|
| 0 — Discovery + ADRs | Tech stack, cost model, 10 ADRs | DONE |
| **1 — Functional Requirements** | **Vision, personas, glossary, user stories, use cases, permissions, NFRs** | **NEXT** |
| 2 — UX / Wireframes | Information architecture, low-fi wireframes for mobile and web | pending |
| 3 — Architecture details | Bounded contexts, aggregates, domain model, OpenAPI, C4 diagrams | pending |
| 4 — Iterative implementation | MVP scope, deployable end-to-end slice | pending |

---

## 9. Session log

Append a row when a working session ends. Keeps a lightweight audit trail of *what changed when* so the next session has continuity.

| Date | Session focus | Deliverables advanced | Decisions resolved | Notes |
|---|---|---|---|---|
| 2026-05-28 | Phase 0 (discovery + ADRs) and Phase 1 hand-off | — | — | 10 ADRs, tech stack, cost estimate, this plan; nothing committed yet |
|  |  |  |  |  |

## 10. How to update progress

- **During a session:** tick checkboxes in sections 3 and 4 as work completes.
- **End of session:** append one row to section 9 (Session log) summarizing what moved.
- **When a deliverable is fully done:** tick its top-level box in "Deliverables" *and* its sub-items.
- **When an open decision is resolved:** tick the box in section 3 ("Open decisions to resolve"), create the corresponding new ADR under `docs/adr/`, and update `docs/architecture/open-decisions.md`.
- **Definition of done for Phase 1:** all D1–D7 boxes ticked, both Phase-1 open decisions resolved (with ADRs), session log up to date.
