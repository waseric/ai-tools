# CP-2 Batch Audit — Five-Spec Drift Review

> Status: Scaffolded 2026-05-18 — awaiting project-constitution CP-2 closeout
> Driver skill: [/spec-review](../../.agents/skills/spec-review/SKILL.md) (per-spec, sequential)
> Scope: CP-2 drift audits for the five new retroactive specs in the legacy quintet
> Out of scope: project-constitution's CP-2 (runs as a separate session per [strategy OQ-1 resolution as (a)](../../docs/retroactive-spec-strategy.md#decisions-recorded-2026-05-18); journal entry lives in [specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md))

## Purpose

This journal records the cross-skill findings of the batched CP-2 drift audit conducted via `/spec-review` against five retroactive design specs in sequence. Per-spec verdicts are written to each spec's §9 Status line; per-spec journal entries land in each spec's own journal. This file captures the *cross-skill* layer — drift patterns that are only visible when audited together, per [strategy "Drift mitigation"](../../docs/retroactive-spec-strategy.md).

## Roster

| # | Spec | Path | CP-1 state |
|---|------|------|------------|
| 1 | spec-design | [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) | pass with comments 2026-05-18 |
| 2 | spec-write | [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) | pass with comments 2026-05-18 |
| 3 | spec-execute | [specs/20260518-spec-execute-skill/architecture.md](../20260518-spec-execute-skill/architecture.md) | pass with comments 2026-05-18 + amendment 2026-05-18-1 |
| 4 | spec-review | [specs/20260518-spec-review-skill/architecture.md](../20260518-spec-review-skill/architecture.md) | pass with comments 2026-05-18 + amendment 2026-05-18-2 |
| 5 | spec-amend | [specs/20260518-spec-amend-skill/architecture.md](../20260518-spec-amend-skill/architecture.md) | pass with comments 2026-05-18 + amendment 2026-05-18-3 |

Audit order is the reviewer's call at session start. A reasonable default mirrors authoring order (the table above), since each spec was authored citing the previous; auditing in the same direction lets cross-references be walked forward as encountered.

Project-constitution CP-2 (N=1 baseline) runs separately first per the strategy resolution. Its outcome is the precondition for this batch session beginning; if its drift audit surfaces patterns that change how the quintet should be audited, the reviewer reads its journal entry before opening this batch.

## Cross-skill drift patterns to watch

Each spec's §9 CP-2 review focus calls these out explicitly; the batch session enumerates them at the cross-spec layer:

- **Atomic-Skill Portability Principle citation discipline.** Four quintet specs may cite [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) in §3 / §6 correctly and one quietly not. Walk each spec for the bound positions.
- **Session-economy commitment propagation.** Phase 8 token-economy factor (spec-execute §5.8), Phase 1 multi-repo detection (spec-execute §5.1), Phase 4/6 paired-commit strengthening (spec-execute §5.4 / §5.6), `SPEC_REPO_ROOT` INPUTS contract (spec-execute, spec-review, spec-amend). Confirm each appears where committed and is consistent with [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md).
- **Two-source structure (predecessor + sibling design spec).** Each spec's §3 Background distinguishes its predecessor doc from the session-economy sibling design spec (where applicable). Confirm the §8 Validation Approach "Predecessor cross-check" + "Sibling design-spec cross-check" rows are present and structurally consistent. spec-execute introduces shapes (i) and (ii); spec-review / spec-amend use only shape (i).
- **Section-heading citation discipline.** Carried from N=2 / N=3 / N=4 / N=5 CP-1 advisories. Walk each citation in each spec's prose for §-heading vs. line-number form. Per-citation walk discipline was the N=5 corrective; CP-2 verifies it landed.
- **Amendment-ID citation correctness.** Inherited from N=5 — amendment 2026-05-18-3 fixed a citation that pointed to the wrong holder of Amendment 2026-05-17-1. Walk for analogous errors elsewhere in the quintet.

## Entries

Entries appended per-spec audit, in audit order. Each entry includes: spec audited, divergences found, routing decision per divergence (amend spec / amend SKILL.md / accept as known minor), cross-skill pattern observations.

### N=2 — 2026-05-18 — spec-design CP-2

**Spec audited:** [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) §4, §5, §6, §12 against [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md).
**Per-spec verdict:** pass with comments. See [specs/20260518-spec-design-skill/journal.md "Review of CP-2"](../20260518-spec-design-skill/journal.md) for the full entry.
**Auditor:** Claude (agent reviewer).
**Audit order position:** session 1 of 5 in the batch; default authoring order followed.

**Divergences and routing (5 advisory, 0 important, 0 blocker):**

| ID | Summary | Routing |
|---|---|---|
| D-1 | §5.8 Status-banner lifecycle (`Draft → Approved → Superseded`) commitment not declared in SKILL.md | (b) amend SKILL.md |
| D-2 | "Format" in SKILL.md preamble but not Phase 2 enumeration; §13 OQ-1 phrasing slightly overstates the gap | (b) amend SKILL.md Phase 2 (parallel to N=1 amendment 2026-05-18-1's layout elevation) |
| D-3 | SKILL.md OUTPUT FORMAT "All code blocks specify a language" absent from spec | (a) amend spec |
| D-4 | SKILL.md WHAT NOT TO DO items partial home in spec; WND-3 inline-citation rule lacks explicit §5/§6 carrier | (a) amend spec §6 |
| D-5 | "Verification pass as a discrete step" post-draft-walk protocol not in §5.5 | (c) accept as known minor |

**Cross-skill pattern observations (queued for closing summary):**

- **ASPP citation discipline (N=2 confirmation).** spec-design cites `tech-stack.md §21-33` correctly in both §3 Background and §6 NFR Portability. N=1 baseline pattern (correct citation) holds. The discipline does not appear broken here.
- **Two-source structure — shape variant.** spec-design carries shape (i) predecessor cross-check (recommendations doc) but no shape (ii) sibling design spec. §8 has "Predecessor cross-check" row; no "Sibling design-spec cross-check" row, matching the batch journal's expected shape (shape ii is introduced at spec-execute).
- **Section-heading citation discipline.** spec-design §6 NFR Source column uses section-name citations without line numbers (e.g., `SKILL.md OPERATING PRINCIPLES`, `SKILL.md PHASE 3 — SPEC DOCUMENT`). Consistent and follows the N=5 corrective established in N=1 baseline.
- **Amendment-ID citation correctness.** §5.7 cites N=1 amendment 2026-05-17-1 correctly; §11 cites the same amendment's N=2 mining note correctly. The N=5 amendment-ID-error class does not surface here.
- **New finding class: Status-banner lifecycle (D-1).** A forward-looking lifecycle commitment that goes beyond SKILL.md's template-initialization-only stance. Watch for analogous lifecycle commitments in §5.x of the remaining quintet specs.
- **New finding class: SKILL.md preamble vs Phase-body enumeration inconsistency (D-2).** SKILL.md preambles may name items the Phase protocol does not enumerate (or vice versa). Add a first-class CP-2 step: walk preamble vs phase-body for each sibling SKILL.md.
- **CP-1 reviewer-error pattern.** spec-design CP-1 produced one off-by-one citation advisory (c) that CP-2 verification has shown was the reviewer's error, not the spec's. Future retroactive-spec CP-1 reviewers should verify section-heading line numbers before flagging citations as off-by-one; CP-2 audits should add explicit citation-position verification when inheriting CP-1 advisories on citations.
- **D-2 partial-resolution interaction with §13 OQ-1.** Operator chose routing (b) amend SKILL.md — partially resolves OQ-1's strict claim ("undocumented") but the four-option (a/b/c/d) resolution for the *content* of the format-question prompt remains open and routes through OQ-1's existing watch-items machinery. The D-2 amendment does not close OQ-1; it tightens its scope. Sibling specs should watch for the same closure-vs-tightening distinction in their own OQs.

**Routing tally so far (N=2):** amend-SKILL.md ×2, amend-spec ×2, accept ×1. Tally accumulates across the five sessions; the closing summary reads this column to surface cross-cutting amendment opportunities per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) OQ-3.

## Closing summary

Written after all five audits complete. Captures the cross-skill divergence list, any cross-cutting amendments proposed (per [strategy OQ-3](../../docs/retroactive-spec-strategy.md)), and the readiness verdict for `docs/retroactive-spec-pattern.md` per [spec-amend §11 step 4](../20260518-spec-amend-skill/architecture.md).
