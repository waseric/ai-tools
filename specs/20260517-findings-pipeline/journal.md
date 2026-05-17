# Findings Pipeline — Journal

## 2026-05-17 — Design Spec Authored

**Status:** draft — awaiting RC-1 review
**Artifact:** specs/20260517-findings-pipeline/architecture.md
**Origin:** Phase 2 entry point per [roadmap.md:28-42](../roadmap.md#L28-L42). Scope narrowed during Clarify to the four items explicitly named by the operator: intake, triage, investigation, integration points to `spec-amend` / `spec-write`. Operational readiness, iteration methodology, and standalone post-incident review remain Phase 2 deliverables out of scope for this design.

**Decisions made:**
- Name: **Findings Pipeline**. Vocabulary extends `spec-review`'s existing "finding" term rather than introducing a parallel "issue" or "observation."
- Three named phases — intake / triage / investigation — distinguished by **persona-frame** rather than only by content. Persona-frame is load-bearing across the methodology, not unique to this spec.
- Persona model is **orientation, not handoffs.** Solo operators play all roles; the discipline structures the work without requiring multi-person teams.
- Investigation launches as a **protocol** (section in the finding artifact), not a separate skill. Graduation to `finding-investigate` skill deferred to OQ-2, decided at RC-5 Adoption Review.
- Finding artifact lives at `specs/findings/YYYYMMDD-<short-name>/finding.md` + `journal.md`, mirroring the feature-spec convention from [spec-path-convention](../20260515-spec-path-convention/architecture.md).
- Status state machine: `intake → triaged → under-investigation? → routed | closed`, with reopen via new status entry. Investigation phase is optional but documented when skipped.
- Severity carries two axes: methodology severity (`blocker / important / advisory`, extended from `spec-review`) and optional operational urgency (`P1–P4`, ITIL-flavored). Final commitment deferred to OQ-1 at RC-2.
- Routing destinations: `spec-amend` / `spec-write` / `defer` / `close`. No new downstream pipelines.
- Integration with `spec-amend` and `spec-write` by **named input** (`FINDING_PATH`), additive and non-breaking. Minor amendments to both skills are sequenced as Phase E.
- Intake design optimized for interruption-tolerance: target under 60 seconds operator effort from stray observation to parked artifact.
- Intake accepts text, external-system pointer, or both. No automated intake from external systems in this design.
- Cross-repo findings re-use the multi-repo discipline already in `spec-execute`; no new machinery introduced.

**Open questions surfaced and parked in §13:**
- OQ-1 (operational urgency overlay): leaning yes, optional. Decided at RC-2.
- OQ-2 (investigation graduation criteria): protocol now; promote when evidence demands. Decided at RC-5.
- OQ-3 (ITIL incident vs. problem distinction): collapsed to "finding" for now; revisit in broader Phase 2 operational readiness design.
- OQ-4 (per-domain persona-frame naming): operator records descriptively; revisit if AI agents misadopt. Decided at RC-3.
- OQ-5 (external-pointer durability policy): verbatim summary at intake is load-bearing; URL is convenience. Decided at RC-2.

**Out of scope confirmed:**
- Operational readiness (run books, alerting, health checks).
- Iteration methodology.
- Standalone post-incident review.
- Automated intake hooks from external systems.
- Issue-tracker substitute features (long-lived queues, SLAs, owner assignments).
- Replacing `spec-review` checkpoint findings.

**Conversation grounding:**
- Initial gap analysis surfaced no general-purpose intake for findings outside a Review Checkpoint, despite Phase 2 naming the deliverable.
- Operator scoped Phase 2 design pass to the four items above.
- Persona model was operator-introduced: "service desk or manager or anyone can do intake; business analyst is sweet spot for triage; developer for investigation." Adopted directly into §5.6.
- Solo-operator framing operator-confirmed: "keeping the personas/roles in mind, along with their use-cases, is important, across all elements of the methodology, even when the primary interlocuter is one person." Lifted into operating principle.
- Broad-scope operational confirmed: "any issue I notice at any time, including while actively working another spec." Drove the interruption-tolerance NFR.

**Next task pointer:** RC-1 Design Freeze review (via `spec-review`). On pass, proceed to `spec-write` for the first downstream feature spec: `findings-pipeline-schema` (Phase A in Implementation Sequencing).
