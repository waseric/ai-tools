# Finding Triage Skill — Journal

## 2026-05-17 — Feature Spec Authored

**Status:** draft — awaiting execution
**Artifact:** specs/20260517-finding-triage-skill/feature.md
**Upstream design spec:** specs/20260517-findings-pipeline/architecture.md (Phase C of §7 Implementation Sequencing)
**Upstream sibling feature specs:**
- Phase A (done): specs/20260517-findings-pipeline-schema/feature.md (RC-2 passed; remediated 2026-05-17)
- Phase B (done): specs/20260517-finding-intake-skill/feature.md (RC-3a passed 2026-05-17)

**Origin:** Phase C execution following Phase B's RC-3a closeout. Phase A delivered the schema; Phase B authored `finding-intake` and produced two real intake artifacts; Phase C now authors `finding-triage` that consumes them.

**Decisions made:**
- Phase C scope is **skill + README integration**, mirroring Phase B's shape. Four tasks: T-01 (SKILL.md), T-02 (synthetic exercise — triage the existing test-only fixture), T-03 (real-signal dogfood — triage the LWC shelves error finding), T-04 (README "Triaging a finding" section). Operator confirmed all four recommendations in Phase 2 Clarify.
- **Skip-investigation surface is in scope.** The skill supports direct transition from `triaged` to `routed` / `closed` when the route is obvious — design spec §5.3 explicit support. Adds ~30 lines to the skill but completes the Phase C interface contract.
- **OQ-4 resolution: optional revalidation, soft default.** The skill suggests checking pointers when intake Summary is sparse, treats as static otherwise. Records the decision per pointer in the journal entry. This is the design-spec leaning, codified in this spec's §5.4 and to be encoded in SKILL.md Phase 2.
- **OQ-3 resolution: descriptive persona-frame.** The skill suggests a frame derived from `Domain` (business analyst / security analyst / QA lead / methodologist) and accepts free-text override. No fixed enum. Codified in this spec's §5.3 and to be encoded in SKILL.md Phase 2.
- **T-03 dogfood target: LWC shelves error finding.** Real signal at `status: intake`, operational domain, pointer-with-snapshot — natural Phase C dogfood target. Avoids the cost of producing a fresh real signal.
- **T-04 README integration.** Add "Triaging a finding" section paralleling "Creating a new finding." Discoverability for `/finding-triage` in the same place `/finding-intake` is documented.
- **Internal review checkpoint named RC-3b** to disambiguate from the design-spec-level RC-3 (joint Phase B + Phase C review). RC-3b closes when Phase C is shippable; the design-spec RC-3 then closes since Phase B's RC-3a already passed.
- **No 60-second NFR.** Triage is hard-facts work; the design constraint is completeness + persona-frame discipline, not speed.
- **Validation: synthetic (T-02) + real-signal dogfood (T-03).** Same shape as Phase B; T-02 catches mechanics bugs without burning a real signal, T-03 verifies real-world fit.
- **Three internal open questions parked in §13** — rich/sparse Summary heuristic prose; persona-frame label format; reproducing-without-opening-code prose — all decidable at T-01 execution time per Phase B precedent.

**Open questions surfaced and parked in §13:**
- OQ-1 (rich-vs-sparse Summary heuristic prose): leaning one-sentence heuristic + two examples. Decided at T-01.
- OQ-2 (persona-frame label format on the artifact): leaning `<name>; <descriptive frame>; persona-frame: triage`. Decided at T-01.
- OQ-3 (reproducing without opening code): leaning yes — running code to reproduce is allowed; "stay out of code" means don't read implementation. Decided at T-01; phrased in OPERATING PRINCIPLES.

**Design-spec OQ resolutions claimed by this spec:**
- **OQ-3** (multi-domain persona naming) — resolved as option (c) descriptive recording. Codified in this spec's §5.3 and to be encoded in SKILL.md. Will be quoted back to the design spec via `/spec-amend` *after* RC-3 closes (out of scope for this feature spec — see §12).
- **OQ-4** (triage-time external-pointer revalidation policy) — resolved as optional, soft default with journaled decision. Codified in this spec's §5.4 and to be encoded in SKILL.md. Same quoting-back plan as OQ-3.

**Tasks defined:** T-01 (skill artifact, M) → T-02 (synthetic exercise, S) → T-03 (real-signal dogfood, S) → T-04 (README integration, S). Four tasks, all S or M, sequenced so each boundary is a safe stopping point: T-01 is the deliverable; T-02 verifies mechanics; T-03 verifies real-world fit; T-04 lands the README change only after dogfood succeeds.

**Conversation grounding:**
- Operator invoked via `/spec-write finding-triage` against the existing findings-pipeline working directory. Confirmed at Discovery time that Phase A and Phase B are closed (RC-2 verdict, RC-3a verdict, Phase A and Phase B status set to Complete in their respective feature specs).
- Operator answered four Phase 2 questions with all-recommended options: skip-investigation surface allowed; OQ-4 optional-with-soft-default; T-03 dogfood targets LWC finding; T-04 adds README section.
- Discovery confirmed two intake-status findings exist (synthetic fixture, LWC shelves error) and one under-investigation finding (tab-display) — the LWC finding is the natural T-03 target.
- Sibling skill patterns: `finding-intake` (153 lines, 12 structural sections) is the closest reference. Target this spec's deliverable at 180–220 lines.

**Next task pointer:** Execute T-01 (`.agents/skills/finding-triage/SKILL.md`) via `/spec-execute`. Dependencies satisfied (Phase A schema artifacts + Phase B intake skill + two real intake-status findings are committed and stable). No `[blocker]` open questions; ready to proceed.
