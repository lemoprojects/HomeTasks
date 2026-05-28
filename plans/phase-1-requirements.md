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
- [ ] **D7.** `docs/02-requirements/non-functional.md` — Scale, GDPR, accessibility, language, availability, performance, security, observability
- [ ] **D8.** `docs/02-requirements/mvp-scope.md` — MVP scope decision: which user stories / use cases are in vs. out, with rationale; exit gate for Phase 1

### Sub-checklists (granular progress)

**D1 — Vision**
- [ ] Problem statement written (what real pain are we solving, for whom)
- [ ] Target outcomes defined (what changes for the user when this works)
- [ ] Success criteria defined (measurable where possible — e.g. "X% of reminders acted on within Y minutes")
- [ ] Non-goals listed (what we deliberately do not solve)
- [ ] Business model / monetization assumption captured (freemium / paid / per-household; mark as TBD if undecided — must not be silent)
- [ ] User confirmed vision

**D2 — Glossary**
- [ ] Initial term list extracted from ADRs and from D4 user stories (every domain noun is a candidate)
- [ ] Each term has: definition, aliases (if any), examples, related terms
- [ ] Naming convention confirmed (English for code identifiers per `CLAUDE.md`; document any user-facing translations separately)
- [ ] Terms cross-checked against ADR vocabulary for consistency
- [ ] User confirmed glossary is complete and stable enough for Phase 2 wireframes

**D3 — Personas**
- [ ] Personas list identified (candidates: Household Owner, Adult Member, Child Member, SaaS Operator)
- [ ] Each persona has: goals, frustrations, primary device, frequency of use
- [ ] User confirmed list is complete

**D4 — User stories**
- [ ] Stories drafted per persona
- [ ] Stories grouped by capability (e.g. "Manage household", "Plan tasks", "Receive reminders", "Report progress")
- [ ] Each story has acceptance criteria
- [ ] Stories tagged with proposed MVP / post-MVP classification (final scope decision lives in D8)

**D5 — Use cases (target: 4–6 critical flows)**

Each use case must contain: **Trigger, Actors, Preconditions, Postconditions, Main flow, Alternate flows, Error/edge flows.** A use case with only a title and a paragraph does not count as done.

- [ ] Onboard new household (sign up → create household → invite members)
- [ ] Create recurring task and assign to member
- [ ] Receive and act on a reminder (push → open app → mark complete)
- [ ] View family statistics in web admin
- [ ] Handle missed deadline (escalation logic)
- [ ] Add/remove household member
- [ ] All use cases reviewed against the permissions matrix (D6) — operations referenced exist as rows

**D6 — Permissions matrix**
- [ ] Roles enumerated and named
- [ ] Operations enumerated (cross-checked against operations referenced in D5 use cases)
- [ ] Matrix filled
- [ ] Edge cases noted (e.g. can a Child reassign? can a Member remove the Owner?)
- [ ] User reviewed matrix

**D7 — Non-functional**
- [ ] Scale assumptions (target households / users / tasks per household)
- [ ] GDPR posture (data residency confirmed EU, retention policy, right to deletion; note: child-user data may add COPPA-like constraints — flag if Child persona is in scope)
- [ ] Language support — **TBD, candidate open decision** (Polish first vs English first; mechanism: backend-driven copy vs frontend resource files). Surface to user; if it blocks D2/D4, escalate to a new open decision in `open-decisions.md`
- [ ] Accessibility minimum (WCAG level?)
- [ ] Availability target (best-effort vs SLA)
- [ ] **Performance targets** (push delivery latency budget, task-list open time, web-admin report load time)
- [ ] **Security — tenant isolation** (how cross-household data access is prevented; consequence of ADR-0001; what is logged on isolation-boundary checks)
- [ ] **Observability** (what is logged, where, retention; minimum signals for incident triage in MVP)
- [ ] **i18n mechanism** — TBD if D7-language is undecided

**D8 — MVP scope (exit gate)**
- [ ] User stories from D4 split into "MVP" / "post-MVP" with one-line rationale per item
- [ ] Use cases from D5 marked as "MVP critical" / "post-MVP"
- [ ] Cross-checked against D7 (no MVP item depends on a deferred NFR like multi-region, escalation channels beyond push, etc.)
- [ ] Document approved by user as the explicit exit gate for Phase 1

### What Phase 1 does NOT do

- Domain modeling (aggregates, entities) — that's Phase 3
- API design (OpenAPI) — Phase 3
- Wireframes / UX — Phase 2
- Database schema — Phase 3
- Billing / payments / pricing implementation — only the **assumption** is captured in D1, never code
- Any code

---

## 4. Open decisions that block Phase 1

These must be resolved during Phase 1 because they shape the requirements themselves. Tick each box once the corresponding ADR is created and `docs/architecture/open-decisions.md` is updated.

- [ ] **Open decision #6 (`open-decisions.md`)** — User identification within a household: per-person email/account vs shared email + profiles. Blocks: persona definition (D3), User vs Member modeling (D2 glossary), auth flow.
- [ ] **Open decision #8 (`open-decisions.md`)** — Notification format and channels: how many reminder types (before/after/escalation)? Push only or also email/SMS? Blocks: Reminder concept in glossary (D2), reminder use case (D5).
- [ ] **Candidate open decision (language)** — surfaced from D7. Decide whether to formalize as a new open decision (#9) in `open-decisions.md`, or accept "PL first" as default with rationale recorded in D7.

Other open decisions (Auth provider #1, Cosmos API choice #2, .NET version #4, ES yes/no #3) can wait until Phase 3.

---

## 5. Known risks and open questions

Captured so they are not rediscovered mid-phase. Update as new ones surface.

- **Child users raise consent/age constraints.** If Child Member persona stays in scope, GDPR posture in D7 may need to address parental consent and possibly aligned regulations (e.g. COPPA-like). This may also push open decision #6 toward "shared email + profiles".
- **Web admin scope creep.** ADR-0003 fixes web as admin-only, but persona work in D3 will surface requests like "I want to do X from the browser". Risk: silently expanding web scope into daily-use territory. Mitigation: re-cite ADR-0003 in D3 when this comes up.
- **MVP scope (D8) is hard to lock without UX.** Phase 2 (wireframes) may reveal that a "must-have" story is implausible for MVP. Mitigation: treat D8 as the *current* MVP scope, allow one revision after Phase 2 with explicit log entry.
- **Glossary–user-stories cycle.** D2 depends on terms from D4, D4 depends on consistent terms from D2. Mitigation: iterate — draft D2 v1 from ADRs, refine after D4 v1.
- **Open decision #6 may cascade.** Per-person accounts vs shared profiles changes the data model, auth flow, push routing, and permission semantics. Resolve early in Phase 1 to avoid rewriting D3/D4/D6.
- **Open decision #8 may explode in scope.** Adding email/SMS channels adds cost, provider choice, deliverability concerns. Default lean: push-only in MVP unless user pushes back.

---

## 6. Recommended Phase 1 workflow

1. Start with **personas (D3)** — without users, there are no requirements. Likely candidates:
   - Household Owner (adult, organizer, sets up the family)
   - Adult Member (partner — uses mobile, can assign tasks)
   - Child Member (kid — uses mobile, mostly receives and completes tasks; may need restricted UI)
   - SaaS Operator (us — administers across all households)
2. Draft **vision (D1)** in parallel with personas — they inform each other.
3. From personas, derive **user stories (D4)** in "As a … I want … so that …" format.
4. From user stories, extract **glossary (D2)** terms (every noun is a candidate domain concept).
5. Resolve **open decisions #6 and #8** in parallel — they will come up naturally during D3/D4.
6. Build the **permissions matrix (D6)** once personas and operations are stable.
7. Write **use cases (D5)** for 4–6 most critical flows (don't try to cover everything). Apply the structure template (Trigger/Actors/Pre-/Postconditions/Main/Alternates/Errors).
8. Capture **non-functional (D7)** constraints as they surface. Performance and security-isolation targets need explicit user input.
9. Lock **MVP scope (D8)** last — only after D1–D7 are stable. This is the exit gate.

Each deliverable should follow the same discipline as ADRs: write what is decided, mark TBDs explicitly, never invent.

---

## 7. How to resume in a new session

Suggested first prompt for a fresh Claude Code session:

```
Continuing the HomeTasks project. Please read in this order:
  1. CLAUDE.md (project working agreement)
  2. README.md (repository root index of per-area READMEs)
  3. docs/README.md (docs index)
  4. docs/architecture/tech-stack.md and cost-estimate-mvp.md
  5. docs/architecture/open-decisions.md
  6. All ADRs in docs/adr/ (numbered 0001–0010)
  7. plans/phase-1-requirements.md (this file)

Do not rely on prior-session memory — treat the files above as the
single source of truth.

We are starting Phase 1: Functional Requirements. Begin with personas.
Ask me questions about user identification (open decision #6) and
notification types (open decision #8) as they come up — do not assume.
```

---

## 8. Conventions to keep

- **Language**: all project files are in **English**.
- **ADRs are immutable** after acceptance — changes become new ADRs marked `Supersedes ADR-NNNN`.
- **No hallucination** — explicit TBDs over invented details.
- **Plan / ADR before implementation** — per CLAUDE.md and the user's repeated emphasis.
- **Verify before claiming "free"** — Cosmos DB Free Tier is one-per-subscription, Functions Consumption free tier has limits; always state the qualifier.
- **Commits require explicit user permission** per CLAUDE.md — never push without asking.
- **Commit cadence for Phase 1**: one commit per deliverable (D1–D8) once it is reviewed and ticked. Commit message format: `Phase 1: <Dx> — <deliverable name>`. Open-decision resolutions ship in their own ADR commit, separate from the deliverable they unblock.

---

## 9. Phase roadmap (reminder)

| Phase | Output | Status |
|---|---|---|
| 0 — Discovery + ADRs | Tech stack, cost model, 10 ADRs | DONE |
| **1 — Functional Requirements** | **Vision, personas, glossary, user stories, use cases, permissions, NFRs, MVP scope** | **NEXT** |
| 2 — UX / Wireframes | Information architecture, low-fi wireframes for mobile and web | pending |
| 3 — Architecture details | Bounded contexts, aggregates, domain model, OpenAPI, C4 diagrams | pending |
| 4 — Iterative implementation | MVP scope, deployable end-to-end slice | pending |

---

## 10. Session log

Append a row when a working session ends. Keeps a lightweight audit trail of *what changed when* so the next session has continuity. One row per session; cells may use short bullet form. Keep entries terse — the deliverable files themselves are the source of truth, this log is just a pointer.

| Date | Session focus | Deliverables advanced | Decisions resolved | Notes |
|---|---|---|---|---|
| 2026-05-28 | Phase 0 (discovery + ADRs) and Phase 1 hand-off | — | — | 10 ADRs, tech stack, cost estimate, this plan; nothing committed yet |
|  |  |  |  |  |

---

## 11. How to update progress

- **During a session:** tick checkboxes in sections 3 and 4 as work completes.
- **End of session:** append one row to section 10 (Session log) summarizing what moved.
- **When a deliverable is fully done:** tick its top-level box in "Deliverables" *and* its sub-items, then commit per the cadence in §8.
- **When an open decision is resolved:** tick the box in section 4, create the corresponding new ADR under `docs/adr/`, and update `docs/architecture/open-decisions.md`.

### Definition of done for Phase 1

- All D1–D8 boxes ticked (top-level *and* sub-items)
- Both blocking open decisions (#6, #8) resolved with new ADRs; language candidate decision either resolved or explicitly recorded as non-blocking
- Session log up to date
- D8 (MVP scope) approved by the user as the explicit exit gate

### Exit criteria — handoff to Phase 2 (UX / Wireframes)

Phase 2 starts with the following inputs in a stable state. If any is missing, Phase 1 is not done.

- `docs/00-vision.md` — vision and success criteria locked
- `docs/01-glossary.md` — terms stable enough that wireframe labels can reuse them without retranslation
- `docs/02-requirements/personas.md` — personas Phase 2 will design for
- `docs/02-requirements/user-stories.md` — stories with MVP/post-MVP tags consistent with D8
- `docs/02-requirements/use-cases.md` — flows Phase 2 must turn into screens
- `docs/02-requirements/permissions-matrix.md` — drives which UI affordances each role sees
- `docs/02-requirements/non-functional.md` — NFRs that constrain UX (e.g. accessibility level)
- `docs/02-requirements/mvp-scope.md` — explicit scope Phase 2 must cover; nothing else
- New ADRs resolving open decisions #6 and #8
