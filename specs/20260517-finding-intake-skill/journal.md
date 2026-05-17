# Finding Intake Skill — Journal

## 2026-05-17 — Feature Spec Authored

**Status:** draft — awaiting execution
**Artifact:** specs/20260517-finding-intake-skill/feature.md
**Upstream design spec:** specs/20260517-findings-pipeline/architecture.md (Phase B of §7 Implementation Sequencing)
**Upstream sibling feature spec (Phase A — done):** specs/20260517-findings-pipeline-schema/feature.md (RC-2 passed; remediated 2026-05-17)
**Origin:** Phase B execution following the RC-2 closeout of the Phase A schema spec. Phase A delivered the schema artifacts (README, finding template, journal template); Phase B builds the skill that consumes them.

**Decisions made:**
- Phase B scope is **skill only** (not skill + adoption gate; not skill-defers-README). Operator confirmed in Phase 2 Clarify. README flip is bundled as a small follow-on task (T-04) within the same spec, executed only after T-03 dogfood succeeds.
- Short-name derivation: **AI proposes from title; operator confirms**. Stop-word filtering + 3–5 word target + ≤40 character limit + one-step accept/edit. Cosmetic choice; recoverable via rename.
- External-pointer fetch policy: operator **must** supply a summary when providing a URL (summary is load-bearing per design spec §6); the skill **attempts** a fetch if the invoking agent has the capability; fetch failures **must not be silently accepted** — operator sees the failure and chooses retry / proceed without snapshot / cancel. This refines the operator's Phase 2 directive ("failure to retrieve url should not be silently accepted").
- Validation: **synthetic (T-02) + real-signal dogfood (T-03)**. T-02 catches mechanics bugs without burning a real signal on a buggy skill; T-03 verifies the 60-second NFR and produces a real artifact.
- Three internal open questions parked (OQ-1 auto-commit policy; OQ-2 captured-by email; OQ-3 multi-pointer signals), all decidable at T-01 execution time by the skill author rather than requiring upstream amendment.
- Internal review checkpoint named **RC-3a** to disambiguate from the design-spec-level RC-3 (joint Phase B + Phase C review). RC-3a closes when Phase B is shippable; RC-3 still requires Phase C.

**Open questions surfaced and parked in §13:**
- OQ-1 (auto-commit vs. working-tree-leave): leaning working-tree-leave for interruption-tolerance. Decided at T-01 execution time.
- OQ-2 (`Captured by` field with or without email): leaning name-only matching journal convention. Decided at T-01.
- OQ-3 (multiple pointers per signal): leaning accept list with per-pointer fetch attempts. Decided at T-01.

**Tasks defined:** T-01 (skill artifact, M) → T-02 (synthetic validation, S) → T-03 (real-signal dogfood, S) → T-04 (README flip, S). Four tasks, all S or M, sequenced so each boundary is a safe stopping point: T-01 is the deliverable; T-02 verifies mechanics; T-03 verifies real-world fit; T-04 lands the README change only after dogfood succeeds.

**Conversation grounding:**
- Operator invoked via `/spec-write phase b` against the existing findings-pipeline working directory. Confirmed at Discovery time that Phase A is closed (RC-2 verdict committed at ccce4ce; remediation at 71480e9).
- Operator's Phase 2 directive on URL pointer failures ("if operator provides url, failure to retrieve url should not be silently accepted") refined the spec's policy beyond my recommended option — incorporated as a fetch-with-surfaced-failure model rather than the no-fetch model I'd originally proposed. The 60-second NFR remains intact in the typical no-pointer / fetch-succeeds case; pointer-with-fetch-failure is an explicit deviation case the operator chooses to handle.
- Discovery confirmed sibling skill patterns (six existing skills at 200–225 lines each, frontmatter + ROLE + OPERATING PRINCIPLES + INPUTS + phased workflow) — Phase B SKILL.md targets ≤220 lines for consistency.

**Next task pointer:** Execute T-01 (`.agents/skills/finding-intake/SKILL.md`) via `/spec-execute`. Dependencies satisfied (Phase A schema artifacts are committed and stable). No `[blocker]` open questions; ready to proceed.
