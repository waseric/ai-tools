# Retroactive-Spec Pattern — Legacy Quintet Codification

> Status: Planning record — operator-approved 2026-05-19
> Audience: The operator (Eric) and any future session (human or AI) authoring or auditing a retroactive design spec against a shipping skill in this repository.
> Companion artifacts: [retroactive-spec-strategy.md](retroactive-spec-strategy.md), [specs/20260518-cp2-batch-audit/journal.md](../specs/20260518-cp2-batch-audit/journal.md)

## Purpose

This doc codifies what worked across the six retroactive design specs authored against the legacy quintet plus the `project-constitution` baseline (N=1 through N=6, 2026-05-17 to 2026-05-19). It is a planning record, not a spec, and not a binding template — each new retroactive spec author still adapts the patterns to the skill's specific shape. The six patterns are descriptive: they were observable in the evidence, not designed up-front.

The pattern doc is the next-action handoff promised in the [batch CP-2 audit closing summary's readiness verdict](../specs/20260518-cp2-batch-audit/journal.md). The decision to author it at all was the [N=2 inflection point](retroactive-spec-strategy.md) in the strategy doc, deferred at session 2's close and resolved at the batch CP-2 audit's close.

## Scope

### When to apply this pattern doc

- Authoring a retroactive design spec for a skill that already ships in [.agents/skills/](../.agents/skills/) and lacks a spec.
- Auditing such a spec at CP-1 (faithfulness) or CP-2 (drift) against its shipping SKILL.md.
- Reviewing the methodology's spec-coverage state to identify which skills still lack specs.

### When this pattern doc does *not* apply

- Authoring a *fresh* design spec for a skill that does not yet ship — that is `spec-design` against a new skill, not retroactive coverage. Use [.agents/skills/spec-design/SKILL.md](../.agents/skills/spec-design/SKILL.md) directly.
- Authoring a feature spec — use [.agents/skills/spec-write/SKILL.md](../.agents/skills/spec-write/SKILL.md).
- Specs for non-skill artifacts (workflows, conventions, repository-wide policies) — the constitutional binding checklist is partially reusable but the session shape and finding-class protocols are skill-spec-specific.

## Evidence base

Six retroactive specs, six journals, one batch journal, twenty-seven advisory CP-2 findings across six sessions, zero blockers. All six per-spec CP-2 verdicts: `pass with comments`. Cross-skill synthesis in [specs/20260518-cp2-batch-audit/journal.md §"Closing summary"](../specs/20260518-cp2-batch-audit/journal.md). The patterns below are the outputs of that synthesis; this doc is the codification, not the synthesis.

## Session shape

The shape is six steps from session start to CP-2 close. Each step had per-session deviations worth surfacing.

1. **Orient.** Read [retroactive-spec-strategy.md](retroactive-spec-strategy.md), the prior session's journal (N>1), the shipping SKILL.md being spec'd, and the constitution ([specs/mission.md](../specs/mission.md), [specs/tech-stack.md](../specs/tech-stack.md), [specs/roadmap.md](../specs/roadmap.md)).
2. **Phase 1 — Discovery.** Source-file selection (include negative-signal rows: artifacts deliberately not consulted, with rationale); landscape orientation; prior-art scan within the repo.
3. **Phase 2 — Clarify.** Confirm artifact name + directory slug (`<skill-name>-skill` per N=1); confirm audience (reusable verbatim from N=1); confirm verification commitment (light verification for all six sessions — claims cite internal artifacts, not external sources). Triage open questions.
4. **Phase 3 — Spec document.** Author `architecture.md` + `journal.md` paired. Journal records the prior session's Pattern-for-N callouts as validated / refined / rejected, then writes Pattern-for-N+1 callouts of its own.
5. **CP-1 — Faithfulness review.** Per-session, via [/spec-review](../.agents/skills/spec-review/SKILL.md). Verdicts at N=1–N=6: all `pass with comments`. Three sessions produced single-amendment cycles (N=3, N=4, N=5); one produced no amendment (N=6 CP-1 had `important` only via cross-skill upstream tracing); the rest's advisories were absorbed into journal pattern observations without amendment.
6. **CP-2 — Drift audit.** Batched per the [strategy doc's "Drift mitigation"](retroactive-spec-strategy.md). Driver skill: [/spec-review](../.agents/skills/spec-review/SKILL.md) per spec, sequential, with cross-spec patterns captured in a dedicated batch journal at [specs/20260518-cp2-batch-audit/journal.md](../specs/20260518-cp2-batch-audit/journal.md).

### Per-session deviations

| Session | Skill | Deviation worth surfacing |
|---|---|---|
| N=1 | `project-constitution` | Standalone first session — no prior journal to mine. Authored the Pattern-for-N=2 callouts that subsequent sessions inherited. CP-2 ran as its own standalone session ([OQ-1 resolution (a)](retroactive-spec-strategy.md#decisions-recorded-2026-05-18)), establishing the drift-audit baseline before the batch. |
| N=2 | `spec-design` | First N>1 session. First-of-kind status-banner-lifecycle finding class (D-1) surfaced; reconfirmed only at design-spec authoring skills, not at any sibling. Format-question OQ left open and tightened (not closed) via amendment routing. |
| N=3 | `spec-write` | Non-design-spec subject (the skill produces feature specs, not design specs). Predicted status-banner-lifecycle non-fire confirmed. First operator (c)→(a) override observed at D-4. |
| N=4 | `spec-execute` | Most divergent skill shape (eight-phase iterative execution). Two-shape session-economy mapping framing introduced via [amendment 2026-05-18-1](../specs/20260518-spec-execute-skill/journal.md). First re-review cycle in the retroactive series (CP-1 → amendment → re-CP-1). Three (c)→(a) overrides in one session — tally inflection. |
| N=5 | `spec-review` | Simplest two-source application (single shape-(i) mapping). First SKILL.md internal stale-citation finding (D-1) and first cross-skill amendment cycle ([2026-05-18-3](../specs/20260518-spec-review-skill/journal.md), traced upstream from sibling CP-1). |
| N=6 | `spec-amend` | Inline-not-standalone predecessor variant: predecessor doc was a section inside [docs/spec-driven-development-prompts-conversation.md](spec-driven-development-prompts-conversation.md), not a standalone doc. Stale-citation class non-fire confirmed non-universal. Final batch step before closing summary. |

## Constitutional binding checklist

Every retroactive spec cites three constitutional commitments. Verified six-for-six across the quintet plus baseline at CP-2; the discipline is not broken anywhere.

| Commitment | Source | Cited in | Verification scope |
|---|---|---|---|
| Atomic-Skill Portability Principle | [specs/tech-stack.md §21-33](../specs/tech-stack.md#L21-L33) | §3 Background, §6 NFRs (Portability or Adoptability) | Heading-line citation form (`§21-33`, not a sub-bullet line); verified per-spec at CP-2. |
| AI context window limits | [specs/tech-stack.md §44](../specs/tech-stack.md#L44) | §3 Background; propagates into output-sizing commitments where applicable | Heading-bullet citation form. |
| Spec-driven-development convention | [specs/tech-stack.md §51](../specs/tech-stack.md#L51) | §3 Background; explicit rationale for the retroactive spec existing | Heading-bullet citation form. |
| Audience | [specs/mission.md](../specs/mission.md) | §1 frontmatter `Audience:` line | Reusable verbatim from N=1; six-for-six identical. |

The N=5 corrective on section-heading vs. line-number citation form (established at N=1 baseline) holds across all six specs. Authoring discipline: cite the heading line, not the body line inside the section.

## Two-source structure

Each retroactive spec carries up to two structural sources. Both are made explicit in §3 Background and validated in §8 Validation Approach via dedicated cross-check rows.

| Source | Role | Citation example |
|---|---|---|
| **Predecessor doc** | The artifact the skill was originally designed against — frequently a conversation export or a recommendations doc. May be inline (a section inside a larger doc, extracted at the trilogy commit `49c15f0`) per N=6. | N=3 [docs/spec-driven-development-prompts-conversation.md](spec-driven-development-prompts-conversation.md) (whole doc); N=6 same doc lines 391–403 + 414 (inline). |
| **Sibling design spec** | A separate design spec that the retroactive spec inherits commitments from — most often [specs/20260514-session-economy/architecture.md](../specs/20260514-session-economy/architecture.md). | N=4–N=6 all cite session-economy as the sibling source. |

### Two mapping shapes

When a retroactive spec inherits commitments from the sibling design spec, the mapping takes one of two shapes:

- **Shape (i) — §5-enumerated mapping.** The sibling design spec has a numbered §5.x subsection that prescribes specific SKILL.md additions. The retroactive spec cites the §5.x subsection by number; the SKILL.md additions are verifiable at the line level. Example: [N=5 retro §5.8 + INPUTS contract ↔ session-economy §5.4](../specs/20260518-spec-review-skill/architecture.md), prescribing two SKILL.md additions both present.
- **Shape (ii) — narrative-sourced mapping.** The sibling design spec has no numbered subsection for the retroactive concern; the commitment is sourced from §1 / §3 narrative content plus a commit reference. Example: N=4 retro §5.4 / §5.6 cite [session-economy §1 + §3 + commit `e483466`](../specs/20260514-session-economy/architecture.md).

Shape (i) is the simpler and preferred case where the sibling spec affords it. Shape (ii) is acceptable when §5.x coverage doesn't exist; the citation must include the commit reference to anchor the narrative source. Only N=4 (`spec-execute`) exhibited shape (ii); N=5 and N=6 confirmed that the four-mapping complexity at N=4 was the outlier driven by spec-execute's eight-phase iterative shape.

## Authoring-time pre-empt protocols — the three stable finding classes

Three finding classes recurred across the six CP-2 audits at frequencies ≥3/6. Each is a class of drift between SKILL.md and the spec's §4/§5/§6 — and each is surfaceable at authoring time, not just at audit time, by walking the SKILL.md against the spec's carriers as a discrete step.

| Class | Freq. | Authoring-time walk |
|---|---|---|
| WND-partial-home | 6/6 | Walk each SKILL.md `WHAT NOT TO DO` item against §5 Phase content and §6 NFRs. Each WND item should have an explicit §5 or §6 carrier. "Behavioral coverage is fine" is the silent default that produces this class. |
| Preamble-vs-body mirror | 5/6 (absent N=1) | Walk SKILL.md `description:` frontmatter, the `# Skill Name` preamble (`How this skill works` block), and HANDOFF NOTES against the Phase body — bidirectionally. Three flavors observed: (i) preamble omits Phase content the body enumerates; (ii) frontmatter and preamble disagree on pairing list; (iii) preamble omits a sibling caller the frontmatter + HANDOFF NOTES name. |
| OUTPUT FORMAT-absent-from-spec | 3/6 | Walk each rule in the SKILL.md `OUTPUT FORMAT` block (code-block language rules; per-phase manifestation rules; commit-pairing rules) against §4 Output topology / §5 Phase content / §6 NFRs. Lower frequency than the two preceding classes but real; surfaced at N=2 / N=3 / N=6. |

Two finding classes were confirmed *non-universal* and do not generalize as authoring-time pre-empts:

- **Status-banner-lifecycle** (N=2 D-1 only). Three consecutive non-fires (N=4 / N=5 / N=6) confirmed the class is specific to design specs that *also* declare a Draft → Approved → Superseded lifecycle commitment. Watch for it only when authoring a spec that proposes a lifecycle commitment in §5.x.
- **SKILL.md internal stale-citation** (N=5 D-1 only). N=6 non-fire confirmed non-universal. Class is specific to skills whose introductory sections (e.g., "Design Notes," "Notes on what makes this skill load-bearing") were authored pre-trilogy-commit (`49c15f0`, 2026-05-14) and not refactored at the commit. Walk such sections against current Phase body if the skill predates the trilogy split.

## Operator (c)→(a/b) override pattern

The default routing for protocol-detail findings (a SKILL.md commitment partially carried in the spec, with no behavioral failure observable in the running skill) is **(c) accept as known minor**. The pattern that emerged across N=3 through N=6 — load-bearing across four consecutive sessions — is to **surface protocol-detail findings explicitly** rather than absorb them into the (c) silent default.

### How the override works

1. The reviewer identifies a protocol-detail finding whose conservative default is (c).
2. The reviewer surfaces it explicitly via `AskUserQuestion`, with `(a) amend spec` or `(b) amend SKILL.md` as the **Recommended** option, and `(c) accept as known minor` as a non-Recommended alternative.
3. The operator selects the Recommended option in eight-for-eight overrides across N=3–N=6.

The pattern is a discipline, not a policy: the reviewer is not required to override every (c), but is required to *consider* whether the finding warrants surfacing. The Recommended-option discipline held across all four sessions where the override has been exercised.

### Effect on routing tally

The override pattern's load-bearing nature is visible in the six-spec routing tally: the predicted SKILL.md-canonical bias (i.e., most amendments should land in SKILL.md because SKILL.md is authoritative) was countered by the override surfacing previously-silent protocol-detail findings as active spec amendments. Final tally: amend-spec ×13, amend-SKILL.md ×11, accept ×3 — nearly balanced over twenty-seven findings. Without the override, the amend-SKILL.md column would dominate and ×7+ findings would have collapsed silently into accept.

## Cross-skill amendment mechanics

When a CP-2 audit (or a sibling-spec CP-1 audit) surfaces a finding that affects more than one retroactive spec — e.g., the same citation-form error in two specs' §5.x — the amendment can be applied as a **single coherent change across both artifacts**, not two independent amendments. The mechanics were first exercised in [amendment 2026-05-18-3](../specs/20260518-spec-review-skill/journal.md):

| Step | Description |
|---|---|
| Surface | N=6 (`spec-amend`) CP-1 found §5.9 cited the strategy doc as holder of Amendment 2026-05-17-1; correct holder is project-constitution-skill. |
| Trace upstream | Citation error traced to N=5 (`spec-review`) §5.11, which carried the same error. Both specs needed correction. |
| Apply as single amendment ID | Amendment 2026-05-18-3 spanned both `architecture.md` files (commit `7a33abe`) + both journals (commit `c01488a`), with a single amendment ID. |
| Verify at both endpoints | N=5 §5.11 verified clean at N=5 CP-2 (session 4 of the batch); N=6 §5.9 verified clean at N=6 CP-2 (session 5 of the batch). |

The mechanics worked, but the convention is not yet codified anywhere in [.agents/skills/spec-amend/SKILL.md](../.agents/skills/spec-amend/SKILL.md). The single observed cycle anchors a watch item (W-1 below) rather than triggering immediate codification.

## Watch items

These are forward-looking signals named in the [batch CP-2 audit closing summary §"Cross-cutting amendments proposed"](../specs/20260518-cp2-batch-audit/journal.md) and reproduced here as the pattern doc's own watch items. Each is a "do not codify until N events fire" threshold; the codification trigger is recorded so a future session knows when the watch item has matured.

### W-1 — Cross-skill amendment mechanics codification

**Signal.** A second cross-skill amendment cycle (analogous to [2026-05-18-3](../specs/20260518-spec-review-skill/journal.md)) is observed across the methodology.

**Codification trigger.** Add a §"Cross-skill case" section to [.agents/skills/spec-amend/SKILL.md](../.agents/skills/spec-amend/SKILL.md) prescribing the four-step mechanics (surface → trace upstream → apply as single amendment ID → verify at both endpoints) once a second cycle anchors the convention as load-bearing.

**Current state.** One cycle observed; held in [spec-amend §13 OQ-4](../specs/20260518-spec-amend-skill/architecture.md) as the first observation.

### W-2 — Authoring-time per-citation walk codification

**Signal.** A second instance of an upstream-traced citation error (analogous to N=5 §5.11 ↔ N=6 §5.9) surfaces during a future retroactive spec session.

**Codification trigger.** Add a per-citation walk to [.agents/skills/spec-write/SKILL.md](../.agents/skills/spec-write/SKILL.md) and [.agents/skills/spec-design/SKILL.md](../.agents/skills/spec-design/SKILL.md) Phase 3 final-walk steps, prescribing authoring-time verification of every citation's anchor before commit.

**Current state.** One instance observed (the N=5 ↔ N=6 cross-skill amendment). Lower urgency than W-1; class is not yet confirmed stable.

### W-3 — Pattern doc maintenance trigger

**Signal.** A new retroactive spec is authored against a skill currently outside the legacy quintet (e.g., a future skill that ships without a spec and is later retrofitted).

**Codification trigger.** Validate or refine the patterns in this doc against the new session's evidence. If a pattern proves quintet-specific, narrow its scope or add a "does not generalize to" note. If a new stable finding class surfaces, add it to §"Authoring-time pre-empt protocols."

**Current state.** No new retroactive-spec sessions are planned at time of writing. The pattern doc captures a closed evidence base (N=1 through N=6); maintenance is on-demand.

## Limits of this pattern doc

- **Quintet-specific evidence.** Six sessions across `project-constitution` + the five legacy quintet skills. Patterns may not generalize to retroactive specs for non-skill artifacts (workflows, conventions, repository-wide policies).
- **Snapshot in time.** Captured 2026-05-19, post-batch-CP-2. Amendments to the methodology after this date may invalidate specific procedures (e.g., a future SKILL.md schema change would invalidate the three authoring-time walks). W-3 names the maintenance trigger.
- **Not a template.** This doc does not replace [.agents/skills/spec-design/SKILL.md](../.agents/skills/spec-design/SKILL.md) for authoring a retroactive spec. It orients; the skill drives.

## References

### Strategy and synthesis

- [retroactive-spec-strategy.md](retroactive-spec-strategy.md) — strategy doc; this pattern doc's predecessor in the planning-record lineage.
- [specs/20260518-cp2-batch-audit/journal.md](../specs/20260518-cp2-batch-audit/journal.md) — batch CP-2 audit journal; source of the six-spec evidence base, the three stable finding classes, the routing tally, and the readiness verdict that triggered this doc.

### Specs (N=1 through N=6)

- [specs/20260517-project-constitution-skill/](../specs/20260517-project-constitution-skill/) — N=1 baseline; architecture.md + journal.md.
- [specs/20260518-spec-design-skill/](../specs/20260518-spec-design-skill/) — N=2.
- [specs/20260518-spec-write-skill/](../specs/20260518-spec-write-skill/) — N=3.
- [specs/20260518-spec-execute-skill/](../specs/20260518-spec-execute-skill/) — N=4.
- [specs/20260518-spec-review-skill/](../specs/20260518-spec-review-skill/) — N=5.
- [specs/20260518-spec-amend-skill/](../specs/20260518-spec-amend-skill/) — N=6.

### Constitution

- [specs/mission.md](../specs/mission.md), [specs/tech-stack.md](../specs/tech-stack.md), [specs/roadmap.md](../specs/roadmap.md) — constitutional commitments cited by every retroactive spec.

### Skills

- [.agents/skills/spec-design/SKILL.md](../.agents/skills/spec-design/SKILL.md) — the skill that authors retroactive specs (this doc was produced via this skill).
- [.agents/skills/spec-review/SKILL.md](../.agents/skills/spec-review/SKILL.md) — drives CP-1 and CP-2 audits.
- [.agents/skills/spec-amend/SKILL.md](../.agents/skills/spec-amend/SKILL.md) — applies amendments surfaced by review; cross-skill mechanics watch item (W-1) above.

### Methodology-shaping companion specs

- [specs/20260514-session-economy/architecture.md](../specs/20260514-session-economy/architecture.md) — sibling design spec cited by N=4, N=5, N=6; source of shape-(i) and shape-(ii) mapping framing.
