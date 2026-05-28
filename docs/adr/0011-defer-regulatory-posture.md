# ADR-0011: Defer regulatory posture (GDPR and child-data laws) for MVP development

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

Earlier ADRs and the Phase 1 plan treated regulatory compliance as load-bearing:

- [ADR-0001](0001-multi-tenant-saas.md) lists "GDPR compliance required from the start" as a negative consequence of the multi-tenant model.
- [ADR-0005](0005-backend-azure-functions-consumption.md) cites "EU region (GDPR)" in Context and "EU region = GDPR compliance" as a positive consequence of choosing West Europe.
- [ADR-0007](0007-database-cosmos-db-free-tier.md) cites "EU region (GDPR)" as part of the Context for choosing Cosmos DB Free Tier.
- [`plans/phase-1-requirements.md`](../../plans/phase-1-requirements.md) deliverable D7 (Non-functional) included GDPR posture (data residency, retention, right to deletion) and flagged COPPA-like constraints once the Child Member persona was confirmed in scope.

On 2026-05-28 the user issued an explicit directive: regulatory work (GDPR, child-data laws, parental consent, retention policy, right to deletion) is **not** to be addressed during current development. This is a scope decision, not a denial that those obligations exist for a public service.

The decision must be recorded as an ADR because the earlier ADRs are immutable per the ADR discipline (`docs/adr/README.md`), and the change of stance is significant enough that a future reader needs an explicit pointer rather than reconstructing the shift from edits in the plan.

## Decision

We **defer regulatory posture work** for HomeTasks until before any public launch. Concretely:

- No GDPR posture content is authored in Phase 1. D7 (Non-functional requirements) records the deferral as a known debt and a launch gate, and stops there.
- No child-data law analysis (COPPA or equivalents) is performed, even though [Child Member is a confirmed MVP persona](../02-requirements/personas.md#child-member).
- Personas, user stories, use cases, and the permissions matrix are shaped by **product** considerations only — regulatory framing must not drive these documents.
- Open decisions are reweighed without regulatory criteria: in particular, [`open-decisions.md` #1 (Auth)](../architecture/open-decisions.md) no longer evaluates GDPR.
- The choice of **West Europe region** for backend and database ([ADR-0005](0005-backend-azure-functions-consumption.md), [ADR-0007](0007-database-cosmos-db-free-tier.md)) stands on its own merits — primarily latency to expected users and Azure free-tier availability. The regional choice does not require revisiting because of this deferral.

This ADR is the source of truth for the new stance. Earlier ADR text that frames GDPR as required-from-the-start is not corrected in place (ADRs are immutable); readers should consult this ADR for the current position.

## Considered alternatives

- **Address GDPR posture in Phase 1** — rejected by user directive. Rationale: requirements are still moving; investing in posture now is premature and would slow Phase 1 without changing the eventual outcome materially.
- **Drop regulatory consideration permanently** — rejected. Not viable for a multi-tenant SaaS targeting EU users that will process household and possibly child data. This ADR explicitly frames the decision as a deferral, not a cancellation.
- **Edit earlier ADRs in place to remove GDPR references** — rejected. ADRs are immutable after acceptance (`docs/adr/README.md`); editing them destroys the historical record of how decisions were originally justified.

## Consequences

### Positive

- Phase 1 is unblocked from regulatory analysis; D3–D8 can proceed on product grounds.
- No premature posture content that would likely be rewritten once a real legal review happens.
- Open decisions (especially #1 Auth) are simpler to evaluate without a regulatory dimension.

### Negative / Trade-offs

- **Known debt with a hard gate.** Picking this work up is mandatory before any public launch; missing the gate would be a compliance failure, not just a missed feature.
- **Open decision #6 (user identification) carries hidden risk.** Resolving it without a regulatory lens may produce a model (e.g. per-person email for children) that later requires retrofit for parental-consent flows.
- **Child Member persona is in MVP scope without regulatory analysis.** Acceptable for development, but the regulatory rework before launch may require UX or data-model changes that ripple back into earlier deliverables.
- **ADR-0001 consequence "GDPR compliance required from the start" is effectively superseded for the current scope.** ADR-0001 is not edited; this ADR is the corrective record.
- Authoring this deferral as an ADR makes the deferral visible and auditable — but also means a future contributor must check both ADR-0001/0005/0007 and this ADR to understand the current stance.

## Related

- [ADR-0001](0001-multi-tenant-saas.md) — multi-tenant SaaS; GDPR was listed as a required consequence
- [ADR-0005](0005-backend-azure-functions-consumption.md) — backend region; GDPR cited but not load-bearing for region choice
- [ADR-0007](0007-database-cosmos-db-free-tier.md) — database region; same as ADR-0005
- [`plans/phase-1-requirements.md`](../../plans/phase-1-requirements.md) — D7 description, D7 sub-checklist, and §5 risks updated on 2026-05-28 to reflect this deferral
- [`docs/02-requirements/personas.md`](../02-requirements/personas.md) — Child Member MVP implications updated to remove regulatory framing
- [`docs/architecture/open-decisions.md`](../architecture/open-decisions.md) — #1 Auth criteria updated to remove GDPR
