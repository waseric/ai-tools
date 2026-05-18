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

## Closing summary

Written after all five audits complete. Captures the cross-skill divergence list, any cross-cutting amendments proposed (per [strategy OQ-3](../../docs/retroactive-spec-strategy.md)), and the readiness verdict for `docs/retroactive-spec-pattern.md` per [spec-amend §11 step 4](../20260518-spec-amend-skill/architecture.md).
