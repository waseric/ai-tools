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

## 2026-05-17 — Review of RC-1

**Reviewer:** waseric (self-review via Claude)
**Outcome:** pass with comments
**Tasks reviewed:** N/A — design spec checkpoint
**Blockers:** 0
**Important:** 1 — dangling internal reference in §5.8 bullet 4 ("the `spec-execute` adoption path below" — no such section in §11). Recommend rewriting the bullet to treat spec-execute integration as a sibling future enhancement, not part of this design's scope.
**Advisory:** 9 — topology diagram missing `reopened` back-transition; monotonicity wording; route/status mapping spread across sections; persona-frame wording drift (§5.6 vs. §6); intake-persona breadth vs. §5.6 role taxonomy; OQ-1 de-facto decided (convert to decision); OQ-5 partly answered (tighten open part to triage-time revalidation policy); `Last transition` field intent undocumented; §14 ITIL/SDLC references uncited (verification pass deferred).
**Spec amendments proposed:** Route §5.8 important fix through `spec-amend` immediately. Batch advisories into RC-2 schema-pass amendment as part of finalizing the template + state machine.
**Next action:** Run `spec-amend` for the §5.8 dangling-reference fix, then proceed to `spec-write` for `findings-pipeline-schema` (Phase A in §7 Implementation Sequencing).

## 2026-05-17 — Amendment 2026-05-17-1

**Section amended:** specs/20260517-findings-pipeline/architecture.md §5.8 (bullet 4) and §10 Risks (row for "Findings accumulate at `status: intake` without triage", mitigation column)
**Trigger:** RC-1 self-review [important] finding — dangling internal reference to "the `spec-execute` adoption path below" with no such section in §11; same orphan dependency inherited by §10 risk row mitigation
**Reason:** Original wording promised cross-section continuity the spec did not deliver. Treating spec-execute integration as a sibling future enhancement keeps this design's scope honest and matches the operator's Clarify-phase scoping.
**Impact summary:** No tasks/checkpoints affected (no downstream specs exist yet); no completed work invalidated; cross-references checked — none require follow-up.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (spec stays at `Draft — Open for Review`)
**Commit:** this commit

### Full record

**Trigger.** RC-1 Design Freeze self-review on 2026-05-17 surfaced an [important] finding: §5.8 bullet 4 references "the spec-execute adoption path below" but §11 (Adoption Path) has no spec-execute section. The §10 risk-table row for stale-intake-findings carries the same orphan dependency in its mitigation column. The RC-1 recommendation was to treat spec-execute integration as a sibling future enhancement, not part of this design's scope.

**Section.** §5.8 Interruption-tolerance (bullet 4); §10 Risks and Mitigations (row for "Findings accumulate at `status: intake` without triage", mitigation column).

**Change.**

§5.8 bullet 4 — Before:
> - Allowing `spec-execute`'s task-boundary pause to surface "any open findings worth raising?" as an optional prompt — captured in the `spec-execute` adoption path below.

§5.8 bullet 4 — After:
> - Keeping the interruption-tolerance property self-contained within the pipeline: the three bullets above (cheap intake, self-contained artifact, no commit-to-route) hold without any change to `spec-execute`. Future enhancement: a separate amendment could teach `spec-execute` to surface "any open findings worth raising?" at task-boundary pauses, but that integration is out of scope for this design and not load-bearing for interruption-tolerance.

§10 row mitigation — Before:
> Triage is the gate; close/defer are valid terminals; periodic review of stale `intake` findings during spec-execute task-boundary checks

§10 row mitigation — After:
> Triage is the gate; close/defer are valid terminals; periodic operator review of stale `intake` findings as a habit, not enforced by tooling

**Reason.** The original §5.8 bullet promised cross-section continuity that the spec did not deliver. The §10 risk row inherited the same dependency in its mitigation, making the mitigation effectively unspecified. Treating spec-execute integration as a sibling future enhancement keeps this design's scope honest. The interruption-tolerance property holds without the spec-execute prompt; that prompt is a nice-to-have, not load-bearing.

**Impact.**
- **Affected tasks:** none (no downstream feature specs have been authored yet).
- **Affected checkpoints:** RC-1 is closed by this amendment. RC-2 through RC-5 unchanged.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — §6 NFR row states the requirement (not the mechanism) and remains valid; §5 line 50 constraint description does not promise an integration.

**Status implication.** Spec stays at `Draft — Open for Review`.

**Approver.** waseric — approved as drafted on 2026-05-17.
