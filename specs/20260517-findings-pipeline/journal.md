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

## 2026-05-17 — Review of RC-3

**Reviewer:** Claude (self-review on behalf of waseric)
**Outcome:** pass with comments
**Tasks reviewed:** Phase B (`finding-intake-skill` T-01..T-04 + RC-3a) + Phase C (`finding-triage-skill` T-01..T-04 + RC-3b) — the joint deliverable RC-3 gates per [§9 RC-3](architecture.md#rc-3--intake--triage-skill-review-gates-phase-c--phase-e).
**Diff range:** `b3a0f94~..HEAD` over `.agents/skills/finding-{intake,triage}/` + `specs/findings/` (Phase C feature-spec authorship through RC-3b closure).
**Blockers:** 0
**Important:** 0
**Advisory:** 4 — see one-line summaries below.

**Checkpoint contract quoted verbatim from [§9 RC-3](architecture.md#rc-3--intake--triage-skill-review-gates-phase-c--phase-e):**
- **Trigger:** "Both `finding-intake-skill` and `finding-triage-skill` feature specs are complete and the skills are operational."
- **Review focus:** "Persona-frame guidance is correctly embedded in skill prompts; intake friction meets the 60-second target; triage produces hard facts (not hypotheses about cause)."
- **Exit criteria:** "Both skills exercised against at least one synthetic and one real finding; persona-frame check passes for each."

**Review focus walk (one-line verdicts per RC-3 focus area):**
- **Persona-frame guidance correctly embedded:** **pass with comments** (A-1 administrative). Intake's fixed-label discipline ([.agents/skills/finding-intake/SKILL.md L55, L151](../../.agents/skills/finding-intake/SKILL.md)) and triage's descriptive-with-override discipline ([.agents/skills/finding-triage/SKILL.md L55, L62, L92–L104, L192](../../.agents/skills/finding-triage/SKILL.md)) faithfully encode the design-spec §5.6 amendment sub-change F asymmetry (intake-is-anyone vs. triage/investigation are role-specific). Override path exercised on first real dogfood (T-03 operator override `business analyst` → `Sandlot administrator`) — resolves OQ-3 §10 watch item cleanly.
- **Intake friction meets 60-second target:** **pass**. T-03 dogfood timed at ~30–60s with comfortable headroom; T-02 in structured mode was single-pass. Two further intake findings created mid-Phase-C session ([spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/) commit `d764c08`, [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/) commit `e0b0a32`) are independent under-real-conditions confirmation of the §6 interruption-tolerance NFR.
- **Triage produces hard facts (not hypotheses about cause):** **pass**. Synthetic Triage notes ([finding.md L31](../findings/20260517-test-only-signal-synthetic-fixture/finding.md)) explicit: "No cause hypothesis recorded — Triage produces hard facts about shape." Real Triage notes ([finding.md L40](../findings/20260517-easy-survival-shelves-lwc-error/finding.md)) explicit: intake-time plugin-API hypothesis "deferred to investigation — recorded here as deferred, not confirmed at triage." The harder real-finding test case (Intake Summary already contained a plausible cause hypothesis) correctly preserved the hypothesis as deferred rather than promoting it to a Triage claim.

**Exit criteria status:**
- "Both skills exercised against at least one synthetic and one real finding": **met**. Intake T-02 ([test-only-signal-synthetic-fixture](../findings/20260517-test-only-signal-synthetic-fixture/), commit `e7630c9`) + T-03 ([easy-survival-shelves-lwc-error](../findings/20260517-easy-survival-shelves-lwc-error/), commit `2a6bcc3`); Triage T-02 (same fixture, commit `f628db2`) + T-03 (same real finding, commit `d79a1eb`). The same two artifacts span both skills — natural since triage consumes intake output. Two additional real intake exercises landed independently mid-session.
- "Persona-frame check passes for each": **met**. Intake fixed-label `Captured by: <name>; persona-frame: intake` verified at both produced artifacts. Triage descriptive-with-override `Triaged by: <name>; <descriptive frame>; persona-frame: triage` verified at both (synthetic: `methodologist`; real: `Sandlot administrator` operator-overridden). Persona discipline held in artifact content: neither triage opened code; reproductions are server-running (real) or convention-checking (synthetic), both legitimate per the design-spec OQ-3 D-3 distinction (running code to reproduce is allowed; reading code to hypothesize is not).

**Advisory findings (one-line summaries; full body in the in-conversation verdict report at the time of review):**
- **A-1 — OQ-3 and OQ-4 not yet quoted back to design-spec §13.** Phase C feature spec §12 explicitly defers this to a follow-on `/spec-amend` against this design spec; unblocked by RC-3 closure but should be tracked. Proposed wording included in the spec amendments section below. Not gating.
- **A-2 — Inherited RC-3a + RC-3b advisories remain as recorded.** RC-3a's 5 advisories were either remediated via [amendments 2026-05-17-{1,2,3}](../20260517-finding-intake-skill/journal.md) or accepted; RC-3b's 3 advisories (state-machine guard inspection-verified rather than second-invocation-exercised; skip-investigation surface verified by inspection only across T-01/T-02/T-03 inputs without end-to-end execution; session-side intake findings undeclared in §12) were accepted. None promoted at RC-3.
- **A-3 — Two session-side intake findings flag adjacent methodology gaps.** [spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/) is a recurring commit-hygiene observation; [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/) flags a portability risk — the intake/triage skills carry host-project-relative path references (`../../../specs/findings/_template/`) that would break under global skill installation. The latter is worth examination before any out-of-repo adoption (Phase F or earlier consumer pickup). Not gating RC-3; natural Phase F input.
- **A-4 — RC-5's three-real-findings adoption gate not satisfied yet.** RC-3 only required "at least one synthetic and one real per skill" (met). RC-5's gate is "Three real findings (operational, testing, security) routed end-to-end" per [§9 RC-5](architecture.md#rc-5--adoption-review). Current real-finding inventory is operational/methodology-heavy; a security-domain real finding remains absent. Phase F's runway is partly stocked but not complete.

**Spec amendments proposed:**
- Quote-back amendment for **OQ-3** (multi-domain persona naming): convert §13 OQ-3 from "Decided at RC-3" with leaning to a Decided-on-2026-05-17 entry recording option (c) descriptive recording, with cross-reference to [`.agents/skills/finding-triage/SKILL.md` L92–L104](../../.agents/skills/finding-triage/SKILL.md) and T-03 dogfood evidence (override from suggested `business analyst` → operator-named `Sandlot administrator`).
- Quote-back amendment for **OQ-4** (triage-time revalidation policy): convert §13 OQ-4 from "Decided at RC-3" with leaning to a Decided-on-2026-05-17 entry recording the optional-with-soft-default resolution, with cross-reference to [`.agents/skills/finding-triage/SKILL.md` L106–L114](../../.agents/skills/finding-triage/SKILL.md) and T-03 dogfood evidence (rich-Intake Summary → default `treated-as-static` for the auth-walled forum thread).
- Both amendments route via `/spec-amend` against this design spec in a separate session per Phase C feature spec §12. Neither is gating.

**Verification evidence summary:**
- Phase B SKILL.md frontmatter parseable: [`finding-intake/SKILL.md` L1–L5](../../.agents/skills/finding-intake/SKILL.md).
- Phase C SKILL.md frontmatter parseable: [`finding-triage/SKILL.md` L1–L5](../../.agents/skills/finding-triage/SKILL.md).
- Line counts: intake 152, triage 197, README 190 — all under their respective peer ceilings.
- Severity-axis decoupling NFR demonstrated in real finding: `Severity: advisory` + `Operational urgency: P4` populated together ([easy-survival finding.md L5–L6](../findings/20260517-easy-survival-shelves-lwc-error/finding.md)) — the design-spec §6 prediction that the axes can diverge is empirically confirmed at first dogfood.
- External-pointer durability NFR demonstrated: auth-walled forum source preserved via operator-supplied PDF snapshot with `<!-- fetched 2026-05-17 (via operator-supplied PDF snapshot of auth-walled forum thread) -->` prefix per Phase B amendment 2026-05-17-2.
- Hard-facts discipline demonstrated in artifact content (both findings), not just SKILL.md prose.

**Next action:**
- **RC-3 closed.** Phase B + Phase C deliverable accepted as a joint shippable artifact.
- **Phase E unblocked** per design-spec [§7 Implementation Sequencing](architecture.md#7-implementation-sequencing): authoring `spec-amend-finding-input` and `spec-write-finding-input` feature specs (may be bundled) — minor amendments to `/spec-amend` and `/spec-write` to accept `FINDING_PATH` as a named input. Natural next session via `/spec-write`.
- **OQ-3 / OQ-4 quote-back amendments** are low-cost follow-ons against this design spec; defer to a convenient `/spec-amend` session per Phase C §12.
- **A-3 portability concern** ([intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/)) worth examining before any out-of-repo skill adoption; not gating Phase E.
- **Phase F** remains the post-Phase-E adoption checkpoint; the four real findings now in the pipeline are partial RC-5 runway (security-domain example still missing per A-4).

**Reviewer notes — self-review honesty:**
- Self-review caveat acknowledged: same agent executed Phases B + C and reviewed RC-3a, RC-3b, and now RC-3. Per [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md)'s "be especially honest about advisory findings" guidance, A-3 was deliberately flagged at the RC-3 level (not just inherited from RC-3b) because the [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/) portability concern is *more* visible at the design-spec checkpoint than at the per-skill checkpoint — Phase E is about to extend `/spec-amend` and `/spec-write` to accept `FINDING_PATH`, and host-relative path references in the existing finding-{intake,triage} skills become a portability liability the moment any downstream skill carries the same pattern. The operator should consider whether to address this before Phase E starts or accept it as Phase F adoption-review work.
- A-4 is the place I would most expect a second reviewer to push back on. RC-3's exit criteria ("at least one synthetic and one real per skill") are unambiguously met; surfacing RC-5's still-distant gate as an RC-3 advisory is debatable. I included it because the four real findings currently in the pipeline are domain-skewed (no security example yet), and Phase F's runway looks deceptively full at a glance — flagging it now is cheaper than discovering at RC-5 that the security-domain example was never opportunistically captured. Not promoting beyond advisory; the operator may dismiss if RC-5 owns the gap explicitly enough.

## 2026-05-17 — Amendment 2026-05-17-3

**Section amended:** specs/20260517-findings-pipeline/architecture.md §13 OQ-3 (Multi-domain personas)
**Trigger:** RC-3 review (commit `62aa0af`) explicitly proposed quoting the OQ-3 resolution back to §13; deferred from Phase C feature spec per its §12.
**Reason:** Leaving §13 OQ-3 at "Leaning + Decided at RC-3" after RC-3 had already decided would leave the design spec misleadingly open; the resolution lives in skill prose and Phase C §5.3 but was never quoted back to the design spec's working memory.
**Impact summary:** No tasks/checkpoints affected (Phase C done, Phase B done, RC-3 already closed); no completed work invalidated; no cross-references require follow-up.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (spec stays at `Draft — Open for Review`)
**Commit:** `f6b9390`

### Full record

**Trigger.** RC-3 self-review (Claude on behalf of waseric, 2026-05-17, commit `62aa0af`) verified that OQ-3's resolution was already codified in `.agents/skills/finding-triage/SKILL.md` and in the Phase C feature spec §5.3, and validated by real-signal dogfood at T-03 (operator override `business analyst` → `Sandlot administrator`). The RC-3 verdict explicitly proposed this quote-back amendment ("Quote-back amendment for OQ-3"). Phase C feature spec §12 also deferred the quote-back to a follow-on `/spec-amend` session against the design spec — this is that session.

**Section.** §13 OQ-3 (L429–L439 pre-amendment).

**Change.**

§13 OQ-3 — Before:
> **Leaning.** Option (c) — record the frame descriptively. The pipeline's discipline is "triage stays out of code, regardless of which expert frame fits"; the named frame is orientation, and operators name the frame that fits their finding.
>
> **Owner.** Decided at RC-3.
>
> **Watch items.** If AI agents fail to adopt the right frame because the prompt always says "business analyst," generalize the prompt to "domain expert appropriate to <domain>."

§13 OQ-3 — After:
> **Decided.** 2026-05-17 (RC-3). Option (c) — operator records the frame descriptively. The `finding-triage` skill suggests a frame derived from the `Domain` field (`operational` → business analyst; `security` → security analyst; `testing` → QA lead; `methodology` → methodologist; `other` → operator-named) and accepts free-text override. Encoded in [`.agents/skills/finding-triage/SKILL.md` L92–L104](../../.agents/skills/finding-triage/SKILL.md) (persona-frame derivation table + override path) and in [Phase C feature spec §5.3](../20260517-finding-triage-skill/feature.md). Validated on first real-signal dogfood: T-03 ([easy-survival-shelves-lwc-error](../findings/20260517-easy-survival-shelves-lwc-error/), commit `d79a1eb`) overrode suggested `business analyst` to operator-named `Sandlot administrator`, exercising the override path against real evidence.
>
> **Watch item (resolved in the direction of *used*).** Pre-decision concern was that AI agents would default to the suggested frame and the override surface would atrophy. T-03 dogfood resolved this in the direction of override-is-used: the operator-named frame fits the finding better than the derived frame, and the prompt structure (suggest + accept-override) preserves agent guidance without removing operator authority. If a future signal shows the override path going unused across multiple real findings, revisit by generalizing the suggestion to "domain expert appropriate to <domain>" — but as of RC-3, the path is exercised and the discipline holds.

**Reason.** RC-3 closed `pass with comments` and explicitly proposed this quote-back as a deferrable follow-on. Leaving §13 OQ-3 at "Leaning + Decided at RC-3" after RC-3 has actually decided would leave the design spec misleadingly open — a future reader would not know the question was resolved, where the resolution lives, or that real evidence supports it. The amendment closes the loop in the spec's own working memory.

**Impact.**
- **Affected tasks:** none. Phase C is complete; Phase B is complete; no downstream task reads this OQ as an input.
- **Affected checkpoints:** RC-3 was the deciding checkpoint and is already closed; this amendment ratifies the closure in the spec text. RC-4 and RC-5 unaffected.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — §5.6 Persona model and §9 RC-3 already reference the multi-domain persona framing as established design; neither needs to change. Phase C feature spec §12's note about "quoting OQ-3 and OQ-4 resolutions back to the design spec" is satisfied by this amendment (for OQ-3) and the next (for OQ-4); a separate update to that §12 entry is *not* warranted (the deferral was correctly executed, and the §12 line is a record of the decision-to-defer, not a stale claim).

**Status implication.** Spec stays at `Draft — Open for Review`. Conversion of an OQ-still-flagged-open into an OQ-decided does not change design substance; it ratifies existing prose in linked artifacts.

**Approver.** waseric — approved as drafted on 2026-05-17.

## 2026-05-17 — Amendment 2026-05-17-4

**Section amended:** specs/20260517-findings-pipeline/architecture.md §13 OQ-4 (Triage-time revalidation policy for external pointers)
**Trigger:** RC-3 review (commit `62aa0af`) explicitly proposed quoting the OQ-4 resolution back to §13; deferred from Phase C feature spec per its §12. Companion amendment to 2026-05-17-3 (OQ-3 quote-back).
**Reason:** Same pattern as 2026-05-17-3 — leaving OQ-4 at "Leaning + Decided at RC-3" after RC-3 had already decided would leave the design spec misleadingly open; the resolution lives in skill prose and Phase C §5.4 but was never quoted back to the design spec's working memory.
**Impact summary:** No tasks/checkpoints affected (Phase C done, RC-3 already closed); no completed work invalidated; no cross-references require follow-up. Satisfies Phase C feature spec §12's quoting-back deferral for OQ-4.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (spec stays at `Draft — Open for Review`)
**Commit:** `5c8eb8a`

### Full record

**Trigger.** RC-3 self-review (Claude on behalf of waseric, 2026-05-17, commit `62aa0af`) verified that OQ-4's resolution was already codified in `.agents/skills/finding-triage/SKILL.md` L106–L114 and in the Phase C feature spec §5.4, and validated by real-signal dogfood at T-03 (rich-Intake summary → default `treated-as-static` for the auth-walled forum thread, no prompt felt ceremonial). The RC-3 verdict explicitly proposed this quote-back amendment ("Quote-back amendment for OQ-4"). Same Phase C §12 deferral pattern as OQ-3.

**Section.** §13 OQ-4 (L441–L449 pre-amendment).

**Change.**

§13 OQ-4 — Before:
> **Leaning.** Active revalidation is *optional* in the triage skill prompt: the prompt suggests checking the pointer if the summary is sparse or ambiguous, otherwise treats it as static. Codify the soft default rather than mandate.
>
> **Owner.** Decided at RC-3 as part of the `finding-triage` skill prompt design.

§13 OQ-4 — After:
> **Decided.** 2026-05-17 (RC-3). Optional revalidation with soft default. The `finding-triage` skill prompt asks the triager once whether to check the pointer; the soft default is `treated-as-static` when the Intake Summary is judged rich (≥3 sentences, names components, names reporters, includes verbatim quotes or snapshot references), and `recommend-check` when the Summary is sparse. Outcome recorded per pointer in the Triaged journal entry's `Pointer revalidation` field. Encoded in [`.agents/skills/finding-triage/SKILL.md` L106–L114](../../.agents/skills/finding-triage/SKILL.md) (rich-vs-sparse heuristic + soft-default branch) and in [Phase C feature spec §5.4](../20260517-finding-triage-skill/feature.md). Validated on first real-signal dogfood: T-03 ([easy-survival-shelves-lwc-error](../findings/20260517-easy-survival-shelves-lwc-error/), commit `d79a1eb`) accepted the `treated-as-static` soft default for an auth-walled forum thread (operator-supplied PDF snapshot is durable evidence; the policy "felt minimal, not ceremonial" per the T-03 journal entry). The sparse-Intake branch remains unexercised — if and when a sparse-Intake finding arrives, the `recommend-check` branch gets exercised then.

**Reason.** Same pattern as OQ-3: leaving §13 OQ-4 at "Leaning + Decided at RC-3" after RC-3 has actually decided leaves the design spec misleadingly open. The resolution lives in skill prose and Phase C §5.4 but was never quoted back. Closing the loop in the spec's own working memory.

**Impact.**
- **Affected tasks:** none. Phase C done; no downstream task reads this OQ as an input.
- **Affected checkpoints:** RC-3 already closed; this ratifies the closure in spec text. RC-4 and RC-5 unaffected.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — §5.2 Intake (external pointer capture), §5.3 Triage phase, and §6 NFR (external-pointer durability) already reference the policy as established design. Phase C §12's "Quoting OQ-3 and OQ-4 resolutions back" note is now satisfied (OQ-3 by Amendment 2026-05-17-3; OQ-4 by this one).

**Status implication.** Spec stays at `Draft — Open for Review`. Same ratification-of-existing-prose pattern as Amendment 2026-05-17-3.

**Approver.** waseric — approved as drafted on 2026-05-17.

## 2026-05-17 — Amendment 2026-05-17-5

**Section amended:** specs/20260517-findings-pipeline/architecture.md §6 Non-functional Requirements (appended new `Skill portability` row)
**Trigger:** Direct follow-on from Amendment 2026-05-17-1 to [specs/tech-stack.md](../tech-stack.md) (commit `b515c71`), which committed the Atomic-Skill Portability Principle as a methodology-wide constraint. The design spec needed a back-reference so the principle is visible to readers of the findings-pipeline architecture; the README-as-derived-projection clarification rides along as the specific corollary surfaced by the originating finding's investigation.
**Reason:** Two coupled gaps: (1) the methodology-wide commitment had no back-reference from the design spec; (2) the prior unstated assumption that host's `specs/findings/README.md` was a load-bearing runtime input for the `finding-{intake,triage}` skills needed an explicit replacement — articulating the README as a derived human-readable projection, not a runtime input.
**Impact summary:** No tasks/checkpoints affected (RC-1 and RC-3 already closed); no completed work invalidated; cascading amendments 3/4/5 against `findings-pipeline-schema/feature.md` and the two SKILL.md files bring implementation into conformance.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (spec stays at `Draft — Open for Review`; amendment is additive)
**Commit:** `<pending — backfill in follow-up commit per repo convention>`

### Full record

**Trigger.** Direct follow-on from Amendment 2026-05-17-1 to [specs/tech-stack.md](../tech-stack.md) (commit `b515c71`), which committed the **Atomic-Skill Portability Principle** as a methodology-wide constraint on skill construction. The design spec needs to reference the principle so it is visible to readers of the findings-pipeline architecture; the README-as-derived-projection clarification rides along because it is the specific corollary surfaced by the originating finding's investigation phase ([intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/finding.md), at status `under-investigation`).

**Section.** §6 Non-functional Requirements (L286–L298 pre-amendment), appending one new row after the existing `External-pointer durability` row.

**Change.**

§6 NFR table — Before (last existing row + section transition):
> | **External-pointer durability** | The artifact survives external-system unavailability. Pointer text, summary, and any pasted context travel with the finding. |
>
> ## 7. Implementation Sequencing

§6 NFR table — After:
> | **External-pointer durability** | The artifact survives external-system unavailability. Pointer text, summary, and any pasted context travel with the finding. |
> | **Skill portability** | Findings-pipeline skills follow the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle): they bundle their own operational mirror of the schema and default templates, and adapt to host-context conventions (e.g., a project's `specs/findings/` storage directory, project-supplied `_template/` overrides, sibling skills) only when those conventions are present. The schema's authoritative articulation is this design spec (§5.1); [specs/findings/README.md](../findings/README.md) is the schema's derived human-readable projection — useful to humans browsing the storage location, not a runtime input for skills. |
>
> ## 7. Implementation Sequencing

**Reason.** Two coupled gaps. (1) The methodology-wide commitment in [specs/tech-stack.md](../tech-stack.md) needed a back-reference from the design spec, or readers of the findings-pipeline architecture would not see it. (2) The prior unstated assumption that the host's `specs/findings/README.md` was a load-bearing runtime input for the `finding-{intake,triage}` skills (the originating finding's surface) needed an explicit replacement — articulating the README as a derived human-readable projection of the schema, not a runtime input. The principle was already implicit in §5.1's "the artifact never depends on its originating conversation or any external system being still reachable" — this amendment extends the spirit (artifact-portability) to the skills themselves (skill-portability) by reference.

**Impact.**
- **Affected tasks:** none. This is a design-spec amendment, not a feature-spec task.
- **Affected checkpoints:** none. RC-1 (Design Freeze) and RC-3 (Intake & Triage Skill Review) have already closed; this is additive clarification, not a re-opening.
- **Completed work invalidated:** none. The cascading amendments (3/4/5) bring the schema feature spec and the two SKILL.md files into conformance with the principle now visible in §6.
- **Cross-references requiring follow-up:** Amendment 3 ([findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md)) implements scaffold-marker delimiters + skill-bundled templates with host override; Amendments 4 and 5 bring the two SKILL.md files into conformance. No follow-up edits required within this design spec.

**Status implication.** Spec stays at `Draft — Open for Review`. Amendment is purely additive; no existing NFR row is changed and no design decision is reversed.

**Approver.** waseric — approved as drafted on 2026-05-17.
