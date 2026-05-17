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

## 2026-05-17 — Amendment 2026-05-17-2

**Section amended:** specs/20260517-findings-pipeline/architecture.md — §4 (topology diagram + composition rules), §5.1 (template `Last transition` field), §5.3 (dangling `See OQ-1` removed), §5.5 (route → terminal status mapping added), §5.6 (persona-model wording + intake-breadth note), §6 (NFR row wording + new severity-axis row), §13 (OQ-1 removed, OQ-2..5 renumbered to OQ-1..4, new OQ-4 narrowed to triage-time revalidation), §14 (Inspirational reframed)
**Trigger:** RC-1 design-freeze verdict deferred 9 advisory findings to RC-2 schema pass; feature spec specs/20260517-findings-pipeline-schema/feature.md §7 T-01 authorizes the batched amendment.
**Reason:** Presentation, consistency, and disambiguation cleanup; no design substance change. Batched per the RC-1 verdict's explicit recommendation.
**Impact summary:** Affects T-01 of findings-pipeline-schema (this satisfies it). RC-2 reads the amendment as part of schema-pass review. No prior task invalidated; no completed work invalidated.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (spec stays at `Draft — Open for Review`)
**Commit:** 8c146ce

### Full record

**Trigger.** RC-1 Design Freeze self-review on 2026-05-17 closed `pass with comments` with 9 advisory findings deferred to the RC-2 schema pass per the reviewer's explicit recommendation to batch. Feature spec [specs/20260517-findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md) §7 T-01 authorizes this single batched amendment as the first execution task of Phase A.

**Section.** §4 (topology, composition rules), §5.1 (Finding artifact template), §5.3 (cross-reference to OQ-1), §5.5 (Routing decisions), §5.6 (Persona model), §6 (NFR row wording + new row), §13 (Open questions: OQ-1 removed, OQ-5 narrowed, OQ-2..5 renumbered to OQ-1..4), §14 (Inspirational references reframed).

**Change.** Ten edits across nine advisories, in spec-line order:

**Sub-change A (advisory 1) — §4 Topology diagram: add `reopened` back-transition.**

Before (closing line of the topology diagram):
> ```
>                               (route) ──────────────────────────────────────┘
> ```

After:
> ```
>                               (route) ──────────────────────────────────────┘
>                                                                             ▲
>                       (any closed/routed finding may be reopened)           │
>                       reopened ──→ triaged | under-investigation ───────────┘
> ```

**Sub-change B (advisory 2) — §4 Composition rules, bullet 2: monotonicity wording.**

Before:
> - `status` progresses monotonically through `intake → triaged → under-investigation → routed | closed`. Investigation may be skipped (status transitions `triaged → routed | closed` directly), with a journal entry recording the skip rationale.

After:
> - `status` is append-only and forward-progressing under normal flow through `intake → triaged → under-investigation → routed | closed`. Investigation may be skipped (status transitions `triaged → routed | closed` directly), with a journal entry recording the skip rationale. Reopening creates a new status entry that returns to an earlier phase, preserving prior status history in the journal.

**Sub-change C (advisory 8) — §5.1 template: document `Last transition` field intent.**

Before:
> > Last transition: <YYYY-MM-DD>

After:
> > Last transition: <YYYY-MM-DD>                        ← scan-aid: most recent status change without traversing journal

**Sub-change D (advisory 6 part 1) — §5.3 Triage phase: remove dangling `See OQ-1` reference.**

Before:
> - Optionally assigns operational urgency (P1–P4) for operational findings where urgency matters separately from methodology severity. See OQ-1.

After:
> - Optionally assigns operational urgency (P1–P4) for operational findings where urgency matters separately from methodology severity. The urgency field is decoupled from methodology severity: the two axes can diverge (an `advisory` finding may be P1-urgent; a `blocker` finding may be P4).

**Sub-change E (advisory 3) — §5.5 Routing decisions: explicit route subtype → terminal status mapping paragraph.**

Added a new paragraph at the end of §5.5's **Behavior.** block:
> **Route subtype → terminal status mapping.** The four route subtypes map to two terminal status values: `spec-amend` and `spec-write` both terminate at `status: routed` (action delegated to a downstream spec); `defer` terminates at `status: routed` with route subtype `defer` (action consciously deferred, watch condition recorded); `close` terminates at `status: closed` (no action will be taken). The distinction: `routed` means "this finding has produced a decision and is no longer the pipeline's responsibility"; `closed` means "this finding required no decision-producing action." Reopening either terminal state is the `reopened` back-transition documented in §4.

**Sub-change F (advisory 4 part 1, advisory 5) — §5.6 Persona model: wording standardization + intake-breadth note.**

Before (third bullet of §5.6):
> - The persona model is **orientation, not handoff**. The discipline structures the work without requiring multi-person teams. A team adopting the methodology later inherits a structure that maps cleanly to multi-person handoff; a solo adopter benefits from role-framed self-direction in the meantime.

After (third bullet rewritten + fourth bullet added):
> - The persona model is **orientation, not handoffs**. The discipline structures the work without requiring multi-person teams. A team adopting the methodology later inherits a structure that maps cleanly to multi-person handoffs; a solo adopter benefits from role-framed self-direction in the meantime.
> - **Intake's persona-frame is intentionally broader than the triage/investigation frames.** Triage maps to a domain-expert frame (typically business analyst) and investigation maps to a developer frame, but intake explicitly admits "service desk, manager, end-user, AI agent, or **anyone**" because the input source is unbounded — a stray observation in a meeting, an automated alert, or an external bug report are all valid signals. The asymmetry is by design: optimizing intake for capture rate (NFR: 60-second target) is incompatible with persona gating.

**Sub-change G (advisory 4 part 2) — §6 "Adoptability (solo)" NFR row: wording standardization.**

Before:
> | **Adoptability (solo)** | A solo operator can run the full pipeline without persona-mismatch overhead. Persona-frame is orienting, not gating. |

After:
> | **Adoptability (solo)** | A solo operator can run the full pipeline without persona-mismatch overhead. Persona-frame is orientation, not handoffs (see §5.6). |

**Sub-change H (advisory 6 part 2) — §6 NFR table: new severity-axis decoupling row; §13 OQ-1 removed; OQ-2..5 renumbered; cross-references updated.**

§6: New NFR row inserted after "Persona durability":
> | **Severity axis decoupling** | Operational urgency (P1–P4) is an optional axis decoupled from methodology severity (`blocker`/`important`/`advisory`). Operational findings may use both; testing/methodology findings typically use severity alone. The two axes may diverge — recorded decision, RC-2 schema pass. |

§13: Old OQ-1 (Operational urgency overlay) removed (decision now recorded in the new §6 NFR row above). Old OQ-2 → OQ-1, OQ-3 → OQ-2, OQ-4 → OQ-3, OQ-5 → OQ-4 (with sub-change I content).

Cross-reference updates:
- §5.4 `(see OQ-2)` → `(see OQ-1)` (line 234)
- §7 Phase F `(OQ-2)` → `(OQ-1)` (line 314)
- §9 RC-5 review focus: `OQ-2 (investigation graduation)` → `OQ-1 (investigation graduation)` (line 355)

Note on historical record: the prior journal entries (Design Spec Authored, Review of RC-1) reference the original OQ-1..OQ-5 numbering. Journal entries are not retroactively edited; future readers should consult this amendment for the renumbering map.

**Sub-change I (advisory 7) — §13 OQ-5 narrowed to triage-time revalidation; renumbered to OQ-4.**

Before (§13 OQ-5):
> ### OQ-5 — External-system pointer durability and refresh
>
> **Question.** When an intake includes a pointer to an external system (GitHub issue, Slack thread, Sentry alert), what guarantees does the methodology provide about that pointer remaining accessible? What happens when the pointer goes stale?
>
> **Analysis.** Three policies: (a) capture verbatim at intake and don't re-check; (b) capture verbatim at intake plus revalidate during triage; (c) require an offline-readable snapshot (paste content into the artifact) at intake. Option (c) is heaviest but most durable; option (a) is lightest but loses fidelity over time.
>
> **Leaning.** Hybrid: intake captures verbatim + summary; if a triager finds the external pointer is unreachable, that fact is journaled but does not block triage (the summary is load-bearing, the URL is convenience).
>
> **Owner.** Decided at RC-2 as part of intake/triage skill prompts.

After (renumbered to OQ-4, content narrowed):
> ### OQ-4 — Triage-time revalidation policy for external pointers
>
> **Question.** Intake captures external-system pointers verbatim alongside a summary (§5.2), and the artifact survives external-system unavailability (§6 NFR row). The remaining open question is narrower: should triage actively *revalidate* external pointers (follow the URL, check the linked ticket's current state), or treat the pointer as a static record?
>
> **Analysis.** Active revalidation surfaces stale or contradictory external state at the right moment — when a triager is shaping the finding — but introduces an external dependency in triage that may slow it (network reachability, auth) and may pull the triager into the linked ticket's evolving discussion rather than the finding itself. Static treatment keeps triage focused but risks shaping a finding around a no-longer-accurate external pointer.
>
> **Leaning.** Active revalidation is *optional* in the triage skill prompt: the prompt suggests checking the pointer if the summary is sparse or ambiguous, otherwise treats it as static. Codify the soft default rather than mandate.
>
> **Owner.** Decided at RC-3 as part of the `finding-triage` skill prompt design.

**Sub-change J (advisory 9) — §14 Inspirational subsection reframed.**

Before (§14 Inspirational subsection header + preamble + bullets):
> ### Inspirational (frame-agnostic; not binding)
>
> - **ITIL service-management traditions** — incident/problem management as the source of the role-separation framing (service desk / business analyst / engineering). The methodology is "informed by" ITIL per the constitution; this spec does not bind to a specific ITIL version or compliance target.
> - **SDLC defect-lifecycle traditions** — reproduction-first triage; defect state machines. Cited as common engineering practice rather than against any specific standard.
> - **Coordinated vulnerability disclosure (CVD)** — security-finding flows from CERT/CC and FIRST.org traditions provide the parallel for security-domain findings.

After:
> ### Inspirational (frame-agnostic; not binding; no canonical citation verification performed)
>
> These references name traditions that shaped the design's vocabulary and role separation. They are **not** canonical citations: no published source has been verified against the wording or claims attributed below, and the spec is not designed to track any specific framework's version or compliance target. The methodology is "informed by" these traditions per the constitution; precise attribution is deferred to external-adopter need (an adopter who requires citations against ITIL 4 / IEEE / FIRST.org publications can produce a verification pass as a separate exercise).
>
> - **ITIL service-management traditions** — incident/problem management as the source of the role-separation framing (service desk / business analyst / engineering).
> - **SDLC defect-lifecycle traditions** — reproduction-first triage; defect state machines. Cited as common engineering practice.
> - **Coordinated vulnerability disclosure (CVD)** — security-finding flows from CERT/CC and FIRST.org traditions provide the parallel for security-domain findings.

**Reason.** All nine RC-1 advisories were presentation/consistency/disambiguation refinements with no design-substance change. Batching produces a coherent, clean baseline for the schema artifacts (T-02 through T-05 of the schema feature spec). The RC-1 verdict explicitly recommended batching rather than nine separate amendments.

**Impact.**
- **Affected tasks:** T-01 of `findings-pipeline-schema` is satisfied by this amendment. T-02–T-05 of that feature spec read the *amended* spec when authoring schema artifacts — that is the intended sequencing.
- **Affected checkpoints:** RC-1 already closed. RC-2 (Schema Review) reads this amendment as part of the schema-pass review focus. RC-3 through RC-5 unchanged.
- **Completed work invalidated:** None.
- **Cross-references requiring follow-up:** All handled within this amendment — §5.3's `See OQ-1` inlined; §5.4, §7, §9 OQ-2 references renumbered to OQ-1; §6 NFR cross-references §5.6 explicitly.

**Status implication.** Spec stays at `Draft — Open for Review`. No design substance changed.

**Approver.** waseric — approved as drafted on 2026-05-17.
