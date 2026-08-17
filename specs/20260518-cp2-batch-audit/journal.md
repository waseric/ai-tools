# CP-2 Batch Audit — Five-Spec Drift Review

> Status: Closed 2026-05-19 — six-spec CP-2 sweep complete; readiness verdict issued for `docs/retroactive-spec-pattern.md` (see §"Closing summary")
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

### N=3 — 2026-05-18 — spec-write CP-2

**Spec audited:** [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) §4, §5, §6, §12 against [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md).
**Per-spec verdict:** pass with comments. See [specs/20260518-spec-write-skill/journal.md "Review of CP-2"](../20260518-spec-write-skill/journal.md) for the full entry.
**Auditor:** Claude (agent reviewer).
**Audit order position:** session 2 of 5 in the batch; default authoring order followed.

**Divergences and routing (5 advisory, 0 important, 0 blocker):**

| ID | Summary | Routing |
|---|---|---|
| D-1 | OP-7 "No bloat" + WND-3 "no new deps without Alternatives Considered" lacks explicit §5/§6 home | (a) amend spec §6 |
| D-2 | §5.7 + §4 Vocabulary commit Authoritative/Inspirational §14 split; SKILL.md §14 does not require (CP-1 advisory (a) carry-forward) | (b) amend SKILL.md §14 |
| D-3 | SKILL.md OUTPUT FORMAT "All code blocks must specify a language" absent from spec §6 (direct N=2 D-3 parallel) | (a) amend spec §6 |
| D-4 | SKILL.md WND-4 task-phrasing rule has no explicit §5.5 home — operator overrode proposed (c) | (a) amend spec §5.5 |
| D-5 | SKILL.md preamble line 15 omits "decisions proposed-unilaterally" from Phase 2 description; Phase 2 body enumerates it. Internal SKILL.md inconsistency (mirror-class of N=2 D-2) | (b) amend SKILL.md preamble |

**Cross-skill pattern observations (queued for closing summary):**

- **ASPP citation discipline (N=3 confirmation).** spec-write §3 cites [tech-stack.md §21-33](../tech-stack.md#L21-L33) correctly (heading line for ASPP); §6 Portability NFR also cites correctly. N=1 / N=2 baseline pattern (correct citation) holds at N=3.
- **Two-source structure — shape (i) only.** spec-write has shape (i) predecessor cross-check ([docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225); no sibling design spec it cites architecturally. §8 has "Predecessor cross-check" row; no "Sibling design-spec cross-check" row, matching the batch journal's expected shape for non-spec-execute quintet members.
- **Section-heading citation discipline.** spec-write §3 citations (`§21-33`, `§44`, `§48`, `§51`) point at correct content. §21 is the ASPP heading line; §44/§48/§51 are bullet-start lines under sub-section headings (Constraints, Conventions Outside the Stack) — they cite the correct content even when not at top-level heading lines. CP-1 reviewer's "heading/section-start line" claim accurate. N=5 corrective established at N=1 baseline holds.
- **Amendment-ID citation correctness.** §11 cites N=1 amendment 2026-05-17-1 (the `.claude/skills/...` removal) correctly. The N=5 amendment-ID-error class does not surface here.
- **"WHAT NOT TO DO partial home" finding class — three data points.** N=1 D-4 (README-reconciliation), N=2 D-4 (Inline-citation preference), N=3 D-1 (No-bloat / no-new-deps). Three CP-2 audits in a row have surfaced a WND item lacking explicit §5/§6 carrier. Stable cross-skill pattern; closing-summary candidate for cross-cutting observation per [strategy OQ-3](../../docs/retroactive-spec-strategy.md). Future CP-2 audits should walk WHAT NOT TO DO items against §5/§6 carriers as a first-class step.
- **SKILL.md preamble-vs-Phase-body inconsistency class confirmed (D-5).** N=2 D-2 had preamble naming "format" that Phase 2 body did not enumerate. N=3 D-5 is the mirror direction: Phase 2 body includes "decisions proposed-unilaterally" that preamble omits. Two directions of the same class across two consecutive sessions; finding class is real and recurring.
- **CP-1 reviewer-error pattern (N=3 callout iii) did NOT fire at N=3.** spec-write CP-1 explicitly elevated section-heading citation discipline to a review-focus check item; the §3 citations were correct and CP-2 confirms. Pattern: when CP-1 elevates a discipline to explicit check, CP-2 does not re-find it. Suggests CP-1 review-focus item elevation is the right intervention.
- **Phrasing-decision matrix (N=3 callout iv) did NOT fire** — spec §13 reports "none surfaced" with triage table; no open OQs to interact with. D-2 (b) and D-5 (b) routings touch SKILL.md without any OQ interaction. Matrix remains a candidate pattern awaiting first exercise.
- **Status-banner-lifecycle finding class (N=3 callout i) did NOT fire** — spec-write produces feature specs (no Status banner in 14-section template); finding class is design-spec-specific. Future quintet specs (spec-execute, spec-review, spec-amend retroactive specs — all design-spec form) may surface this class.
- **D-4 operator override (proposed (c) → routed (a)).** Reviewer proposed accept-as-known-minor on protocol-detail grounds (mirror of N=2 D-5 disposition); operator chose to amend spec §5.5. Pattern: reviewers should surface protocol-detail findings explicitly for operator choice rather than absorbing them silently into (c). Note this is a deviation from the N=2 D-5 precedent and worth flagging in the closing summary.

**Routing tally so far (N=2 + N=3):** amend-SKILL.md ×4 (N=2's D-1+D-2, N=3's D-2+D-5), amend-spec ×5 (N=2's D-3+D-4, N=3's D-1+D-3+D-4), accept ×1 (N=2's D-5). Running tally accumulates across remaining sessions; the closing summary reads this column to surface cross-cutting amendment opportunities per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) OQ-3.

### N=4 — 2026-05-18 — spec-execute CP-2

**Spec audited:** [specs/20260518-spec-execute-skill/architecture.md](../20260518-spec-execute-skill/architecture.md) §4, §5, §6, §12 against [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md).
**Per-spec verdict:** pass with comments. See [specs/20260518-spec-execute-skill/journal.md "Review of CP-2"](../20260518-spec-execute-skill/journal.md) for the full entry.
**Auditor:** Claude (agent reviewer).
**Audit order position:** session 3 of 5 in the batch; default authoring order followed.
**Notes:** Per [strategy doc](../../docs/retroactive-spec-strategy.md), this session is the **N=3 robustness check** on the most-divergent skill shape (eight-phase iterative execution workflow). The three Pattern-for-N=4 observations from N=3 close ([N=3 callouts above](#L93)) were applied as first-class audit steps: (1) operator (c)→(a) override surfaced explicitly, (2) WND items walked against §5/§6 carriers, (3) SKILL.md preamble walked line-by-line against Phase 2 body in both directions.

**Divergences and routing (5 advisory, 0 important, 0 blocker):**

| ID | Summary | Routing |
|---|---|---|
| D-1 | SKILL.md WND-8 "no speculative code for the next task" lacks explicit §5/§6 carrier — §5.4 cites OP §5 "One task at a time" as Pattern invoked but does not reproduce the prohibition | (a) amend spec §5.4 — observation-1 surfacing |
| D-2 | SKILL.md WND-9 "amendments require diff; surgical not rewrite" only partially carried — §5.3 covers pre-execution amendments; §5.9 Amendment Protocol Behavior does not reproduce the rule for Phase 4/5 trigger sites | (a) amend spec §5.9 — observation-1 surfacing |
| D-3 | SKILL.md `# Spec Execute` preamble (line 9) omits `spec-amend` from pairing list — frontmatter description and AMENDMENT PROTOCOL both reference it | (b) amend SKILL.md preamble |
| D-4 | SKILL.md "How this skill works" preamble (line 13) does not name Phase 8 Session Continuity Check — frontmatter description and Phase 8 body both name the task-boundary pause | (b) amend SKILL.md preamble |
| D-5 | §5.6 "Four updates fire" precedes a five-item list (CP-1 advisory (a) carry-forward) | (a) amend spec §5.6 "Four → Five" — observation-1 surfacing |

**Cross-skill pattern observations (queued for closing summary):**

- **ASPP citation discipline (N=4 confirmation).** spec-execute §3 cites [tech-stack.md §21-33](../tech-stack.md#L21-L33) correctly in both §3 Background and §6 Portability NFR. N=1/N=2/N=3 baseline pattern (correct citation at heading line) holds at N=4.
- **Session-economy commitment propagation — first concrete instance.** Phase 8 token economy (§5.8), Phase 1 multi-repo (§5.1), Phase 4/6 paired-commit (§5.4 / §5.6), and SPEC_REPO_ROOT INPUTS contract all present and consistent. Amendment 2026-05-18-1 (commit `b61cb3f`) concretized the two-shapes framing: shape (i) §5-enumerated (retro §5.1 ↔ session-economy §5.2; retro §5.8 ↔ session-economy §5.1) and shape (ii) narrative-sourced (retro §5.4 / §5.6 cite session-economy §1 + §3 + commit `5ce4024`). Both shapes verified at CP-2. **Closure on shape (i): retro §5.1 ↔ session-economy §5.2; retro §5.8 ↔ session-economy §5.1.** **Closure on shape (ii): session-economy §5 has no subsection for spec-execute Phase 4/6; narrative-source attribution structurally correct.**
- **Two-source structure — first-of-kind confirmed.** §3 distinguishes predecessor doc + sibling design spec; §8 carries both Predecessor-cross-check and Sibling-design-spec-cross-check rows; §14 separates Authoritative (SKILL.md + session-economy + tech-stack + mission + roadmap) from Inspirational (predecessor doc + N=1/N=2/N=3 specs). **Pattern for N=5:** the session-economy spec is the sibling source for both remaining quintet members — spec-review will encounter [session-economy §5.4](../20260514-session-economy/architecture.md#L147); spec-amend will encounter [session-economy §5.3](../20260514-session-economy/architecture.md#L123). The two-shapes framing carries verbatim where applicable.
- **Section-heading citation discipline (N=4 confirmation).** All §3 tech-stack.md citations (§21-33, §44, §48, §51) verified at heading lines; predecessor-doc line citations (235–438, 410, 412, 416, 418, 422) verified at correct anchors. N=2/N=3 corrective holds at N=4. CP-1 reviewer-error pattern from N=2 did **not** fire at N=4 (citations were correct on first walk).
- **Amendment-ID citation correctness (N=4 confirmation).** §11 cites N=1 amendment 2026-05-17-1 correctly (mining-note discipline: scan entire spec at Phase 1 Orient for path/citation/vocabulary-class amendments). The N=5-corrective amendment-ID-error class does not surface at N=4.
- **"WHAT NOT TO DO partial home" finding class — four data points.** N=1 D-4 (README-reconciliation), N=2 D-4 (Inline-citation preference), N=3 D-1 (No-bloat / no-new-deps), N=4 D-1+D-2 (Speculative-code + diff-required). **Pattern confirmed stable across four consecutive sessions.** Observation (2)'s "first-class step" protocol (walking WND items against §5/§6 carriers as a discrete step, not absorbed into "behavioral coverage looks fine") produced two findings at N=4 that would otherwise have been silent. Carry to N=5.
- **SKILL.md preamble-vs-Phase-body mirror class — four data points.** N=2 D-2 (preamble names "format" Phase 2 body omits), N=3 D-5 (Phase 2 body enumerates "decisions proposed-unilaterally" preamble omits), N=4 D-3 (preamble omits `spec-amend` from pairing list; frontmatter+body both name it), N=4 D-4 (preamble omits Phase 8 Session Continuity Check from "How this skill works" enumeration; frontmatter+Phase 8 body both name it). Both directions of the class confirmed; observation (3)'s bidirectional walk produced both findings at N=4. **Class is robust; carry verbatim to N=5.**
- **Operator (c)→(a) override pattern (observation 1) applied at three findings.** D-1, D-2, D-5 all had (c) accept-as-known-minor as the conservative default; reviewer surfaced (a) explicitly per observation (1) precedent (N=3 D-4); operator confirmed (a) on all three via AskUserQuestion at audit close. **Pattern is now load-bearing**: surfacing protocol-detail findings explicitly converts conservative-default routings to active amendments. Intentionally raises the amend-spec count at N=4 (×3) vs prior N audits.
- **Status-banner-lifecycle finding class (N=2 D-1, queued as candidate for N=4) did NOT fire.** spec-execute §1 banner reads `Draft — Open for Review` but no §5.x commits to a lifecycle (Draft → Approved → Superseded or similar); the lifecycle commitment shape was specific to spec-design's §5.8. The candidate finding-class did not surface despite spec-execute being in design-spec form. Pattern observation: the finding class may be specific to design specs that *also* declare a lifecycle commitment, not all design specs. Sessions 4–5 (spec-review, spec-amend) should still walk for it (both are also design-spec form) but a non-fire is the expected default.
- **First /spec-review → /spec-amend → /spec-review cycle in retroactive-spec sequence — closure verified at CP-2.** The cycle (CP-1 verdict at commit `0879935` → amendment 2026-05-18-1 at commit `b61cb3f` → re-review at commit `db2f7b3`) concretized the two-shapes framing now verified at CP-2. **Pattern for N=5:** the cycle is a viable path for any CP-1 "changes requested" verdict whose blockers collapse to a single citation pattern.
- **Robustness check on most-divergent skill shape — passed.** Per [strategy doc](../../docs/retroactive-spec-strategy.md), this session was the robustness check on the spec-design / spec-write CP-2 pattern. The eight-phase iterative shape produces a denser §5 (eleven subsections vs N=2/N=3's eight), a sibling-design-spec source (first-of-kind), and four findings — but the audit shape (review focus walk → divergence list → routing decisions → cross-skill pattern observations) generalized cleanly. **Finding classes (WND partial-home, preamble-vs-body mirror, protocol-detail surfacing) are the same as N=2/N=3.** Test passes.

**Routing tally so far (N=2 + N=3 + N=4):** amend-SKILL.md ×6 (N=2's D-1+D-2, N=3's D-2+D-5, N=4's D-3+D-4), amend-spec ×8 (N=2's D-3+D-4, N=3's D-1+D-3+D-4, N=4's D-1+D-2+D-5), accept ×1 (N=2's D-5). Tally inflection at N=4: operator (c)→(a) override pattern applied at three findings, raising amend-spec from cumulative ×5 to ×8 in one session. The closing summary at end-of-batch should read this skew explicitly.

### N=5 — 2026-05-18 — spec-review CP-2

**Spec audited:** [specs/20260518-spec-review-skill/architecture.md](../20260518-spec-review-skill/architecture.md) §4, §5, §6, §12 against [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md).
**Per-spec verdict:** pass with comments. See [specs/20260518-spec-review-skill/journal.md "Review of CP-2"](../20260518-spec-review-skill/journal.md) for the full entry.
**Auditor:** Claude (agent reviewer).
**Audit order position:** session 4 of 5 in the batch; default authoring order followed.
**Notes:** Per [strategy doc](../../docs/retroactive-spec-strategy.md), this session is the **simplest two-source application in the quintet** — single shape (i) §5-enumerated mapping (retro §5.8 + INPUTS contract ↔ session-economy §5.4); zero shape (ii) claims. The four Pattern-for-N=5 observations from N=4 close (WND walked against §5/§6 carriers; preamble walked line-by-line against Phase 2 body; per-§5-subsection citation audit at authoring time; operator (c)→(a) override surfaced explicitly) were applied as first-class audit steps. All four reproduced findings at N=5 (D-3, D-4, D-1, D-2/D-3 respectively).

**Divergences and routing (5 advisory, 0 important, 0 blocker):**

| ID | Summary | Routing |
|---|---|---|
| D-1 | SKILL.md Design Notes line 209 stale: "Amendment Protocol from `spec-execute`"; Phase 8 body line 181 correctly routes to `spec-amend`. Internal SKILL.md inconsistency. CP-1 A3 carry-forward. | (b) amend SKILL.md Design Notes |
| D-2 | SKILL.md uses `[important]` tag (Phase 3:83, Phase 7:132/138) without defining semantics; spec §4 Vocabulary defines as "middle tag between blocker and advisory: not a spec violation but a quality concern serious enough to warrant attention before the next task." CP-1 A2 carry-forward. | (b) amend SKILL.md — operator (c)→(b) override surfacing |
| D-3 | SKILL.md WND-5 "Do not rubber-stamp based on the journal entry alone. The journal is the implementer's claim; the diff is the evidence" lacks explicit §5/§6 carrier. WND-partial-home class 5th data point. | (a) amend spec §5.1 — operator (c)→(a) override surfacing |
| D-4 | SKILL.md preamble line 9 "Third in the trilogy with `spec-write` and `spec-execute`" omits spec-design + spec-amend from pairing list; frontmatter line 4 names all four. Preamble-vs-body mirror class 5th data point (frontmatter-vs-preamble flavor). | (b) amend SKILL.md preamble |
| D-5 | Spec §5.2/§5.6 Design-spec adaptation sub-blocks describe improvised mechanics; SKILL.md silent on phase mechanics for design-spec artifacts. | (c) accept — already routed via §13 OQ-1 leaning (c) |

**Cross-skill pattern observations (queued for closing summary):**

- **ASPP citation discipline (N=5 confirmation).** spec-review §3 line 51 cites [tech-stack.md §21-33](../tech-stack.md#L21-L33) correctly (heading line for ASPP); §6 line 269 Adoptability NFR cites same. N=1/N=2/N=3/N=4 baseline pattern (correct citation at heading line) holds at N=5. **The discipline does not appear broken across the entire quintet.**
- **Session-economy commitment propagation — simplest application validated.** Single shape (i) §5-enumerated mapping (retro §5.8 + INPUTS contract ↔ session-economy §5.4) verified at CP-2 by reading [session-economy §5.4 lines 147–164](../20260514-session-economy/architecture.md#L147-L164) directly: §5.4 prescribes exactly two SKILL.md additions (INPUTS entry + Phase 8 "Multi-repo case" paragraph), both present in SKILL.md ([line 24](../../.agents/skills/spec-review/SKILL.md#L24) and [line 179](../../.agents/skills/spec-review/SKILL.md#L179)). **Closure on shape (i) at N=5:** retro §5.8 + INPUTS contract ↔ session-economy §5.4. **Zero shape (ii) claims at N=5.** The N=4 prediction that the four-mapping complexity was the outlier (spec-execute as largest beneficiary of session-economy commit) is now confirmed: N=5 has one mapping, the simplest in the quintet.
- **Two-source structure — shape (i) only confirmed.** §3 distinguishes predecessor doc + sibling design spec; §8 carries both Predecessor-cross-check and Sibling-design-spec-cross-check rows; §14 separates Authoritative (SKILL.md + session-economy + tech-stack + mission) from Inspirational (predecessor + N=1/N=2/N=3/N=4 specs). **Pattern for N=6:** spec-amend will be similarly simple — session-economy §5.3 is the sibling-design-spec source; one shape (i) mapping; zero shape (ii). spec-amend has **no predecessor doc** ([N=3 journal](../20260518-spec-write-skill/journal.md) prediction — to be confirmed at N=6), making it the first quintet retroactive spec with zero "Inspirational" predecessor source.
- **Section-heading citation discipline (N=5 confirmation).** All §3 tech-stack.md citations (§21-33, §44, §48, §51) verified at heading lines per CP-1 verification trail and re-verification at CP-2. N=2/N=3/N=4 corrective holds at N=5.
- **Amendment-ID citation correctness post-amendment-2026-05-18-3 confirmed.** §5.11 line 259 now correctly cites `[project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md)`. The pre-amendment error (citing strategy-doc as holder) was caught at the *sibling* spec's CP-1 (spec-amend CP-1 found N=6 §5.9 had the same error, traced upstream to N=5 §5.11), routed through cross-skill amendment 2026-05-18-3 (commits `945f9ab` + `80bb899`), and verified clean at CP-2. **First evidence of upstream-traced cross-skill citation amendment in the retro-spec series.**
- **"WHAT NOT TO DO partial home" finding class — five data points (D-3 at N=5).** N=1 D-4 (README-reconciliation), N=2 D-4 (Inline-citation preference), N=3 D-1 (No-bloat / no-new-deps), N=4 D-1+D-2 (Speculative-code + diff-required), N=5 D-3 (rubber-stamp prohibition). **Pattern confirmed stable across five consecutive sessions.** Each session's "walk WND items against §5/§6 carriers as a discrete step" protocol from N=3 close has produced at least one finding in every subsequent session. Carry verbatim to N=6.
- **SKILL.md preamble-vs-body mirror class — five data points (D-4 at N=5).** N=2 D-2, N=3 D-5, N=4 D-3+D-4, N=5 D-4. **Five consecutive sessions; pattern stable.** N=5 D-4 introduces a frontmatter-vs-preamble flavor of the same class (frontmatter complete, preamble incomplete). The bidirectional walk protocol from N=4 generalizes to include the frontmatter axis. Carry to N=6.
- **New finding class — SKILL.md internal stale-citation (D-1).** First-of-kind for the quintet. SKILL.md Design Notes section was authored pre-trilogy-commit (when Amendment Protocol lived inside spec-execute), and the trilogy commit `80000b1` (which split off `spec-amend`) updated the Phase body but not the Design Notes. This is a class of finding visible only when reading Design Notes against Phase body — neither the preamble walk nor the WND walk would have surfaced it. **Pattern for N=6:** add a first-class CP-2 audit step — walk SKILL.md Design Notes section against Phase body for stale pre-trilogy phrasing. Likely candidates in spec-amend: any mention of "Amendment Protocol" wording that predates the skill being its own skill.
- **Operator (c)→(a/b) override pattern applied at two findings (D-2, D-3).** D-2 had (c) accept-as-known-minor as conservative default; reviewer surfaced (b) explicitly per N=3 D-4 / N=4 D-1/D-2/D-5 precedent; operator confirmed (b) via AskUserQuestion. D-3 had (c) accept as conservative default; reviewer surfaced (a) explicitly; operator confirmed (a). **Pattern continues to be load-bearing across N=3/N=4/N=5 (three consecutive sessions).** The Recommended-option-discipline (Recommended is selected) also held — both Recommended options chosen at N=5.
- **Status-banner-lifecycle finding class (N=2 D-1, queued candidate for N=4 and N=5) did NOT fire at N=5.** spec-review §1 banner reads `Draft — Open for Review` but no §5.x commits to a Draft → Approved → Superseded lifecycle. Class remains spec-design-specific (N=2 only). Two consecutive non-fires (N=4 + N=5); the class is effectively confirmed as N=2-unique. Final session (N=6, spec-amend) should still walk for it but a non-fire is the expected default.
- **Simplest CP-1 review focus — no re-review cycle from CP-1 at N=5.** Predicted in N=5 journal Pattern-for-N=6 #6 ("single-mapping audit case is the simplest CP-1 review focus"). CP-1 produced one [important] (citation error in §5.10), which was amendment-2026-05-18-2'd without triggering re-review. The N=4 re-review cycle precedent (commit `0879935` → `b61cb3f` → `db2f7b3`) was the outlier driven by spec-execute's four-mapping complexity; N=5's single-mapping audit collapses the cycle.
- **First post-trilogy cross-skill amendment cycle (amendment 2026-05-18-3) — closure verified at CP-2.** Amendment 2026-05-18-3 was the first cross-skill citation amendment caught at sibling-spec CP-1, traced upstream to N=5 §5.11, and applied as a single amendment ID spanning both architecture.md files (commit `945f9ab`) + both journals (commit `80bb899`). Mechanics worked; CP-2 verifies both endpoints (this spec's §5.11 + spec-amend N=6 §5.9, assumed verified at N=6 CP-2). **Pattern for N=6 CP-2:** verify the spec-amend endpoint of the same amendment.

**Routing tally so far (N=2 + N=3 + N=4 + N=5):** amend-SKILL.md ×9 (N=2's D-1+D-2, N=3's D-2+D-5, N=4's D-3+D-4, N=5's D-1+D-2+D-4), amend-spec ×9 (N=2's D-3+D-4, N=3's D-1+D-3+D-4, N=4's D-1+D-2+D-5, N=5's D-3), accept ×2 (N=2's D-5, N=5's D-5). N=5 tally adds three SKILL.md amendments + one spec amendment + one accept; the amend-SKILL.md and amend-spec columns now equal at ×9. **N=5 inverts the N=4 amend-spec skew** — N=4 added three amend-specs in one session via the (c)→(a) override; N=5 adds three amend-SKILL.md via two findings driven by SKILL.md internal inconsistency (D-1, D-4) plus one operator-confirmed (b) override (D-2). The cross-cutting amendment opportunities per [strategy OQ-3](../../docs/retroactive-spec-strategy.md) are now visible: **two SKILL.md preamble/frontmatter consistency edits across the quintet (N=4 D-3, N=5 D-4) and two Design-Notes-or-stale-citation edits across SKILL.md files (N=5 D-1 is the first; check N=6 for second instance)**.

### N=6 — 2026-05-18 — spec-amend CP-2

**Spec audited:** [specs/20260518-spec-amend-skill/architecture.md](../20260518-spec-amend-skill/architecture.md) §4, §5, §6, §12 against [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md).
**Per-spec verdict:** pass with comments. See [specs/20260518-spec-amend-skill/journal.md "Review of CP-2"](../20260518-spec-amend-skill/journal.md) for the full entry.
**Auditor:** Claude (agent reviewer).
**Audit order position:** session 5 of 5 in the batch — **final batch step before the closing summary**; default authoring order followed.
**Notes:** Per [strategy doc](../../docs/retroactive-spec-strategy.md), this session is the **simplest two-source application by structure but three additions in scale** (one shape (i) §5-enumerated mapping covering three SKILL.md additions, refining the N=5 prediction of two). The four Pattern-for-N=6 observations from N=5 close (WND walked against §5/§6 carriers; preamble walked line-by-line against frontmatter and HANDOFF NOTES; per-citation walk discipline applied at authoring; Design Notes / load-bearing-notes walked for stale phrasing) were applied as first-class audit steps. Three reproduced findings (D-1 WND-8, D-2 preamble omits spec-write, D-3 OUTPUT FORMAT); one non-fire (Design Notes / load-bearing-notes class) — confirming class is not universal across the quintet.

**Divergences and routing (3 advisory, 0 important, 0 blocker):**

| ID | Summary | Routing |
|---|---|---|
| D-1 | SKILL.md WND-8 "Do not let `spec-execute` or `spec-review` silently apply amendments. Both should propose; this skill applies. The separation is what makes amendments visible." lacks explicit §5/§6 carrier. Carried in §3 Background (trilogy commit framing) and §3 Dependencies (spec-review entry) but no §5 phase rule or §6 NFR. WND-partial-home class 6th data point. | (a) amend spec §6 — operator (c)→(a) override per N=3/N=4/N=5 pattern (Recommended selected) |
| D-2 | SKILL.md preamble line 15 ("How this skill works") "another skill — `spec-execute`, `spec-review`, or an in-flight `spec-design` session" omits `spec-write`; frontmatter line 4 + HANDOFF NOTES line 187 both name it. Preamble-vs-body mirror class 6th data point (preamble-omits-sibling-caller flavor — third flavor of the class). | (b) amend SKILL.md preamble |
| D-3 | SKILL.md OUTPUT FORMAT block (lines 165–170) carries per-phase manifestation rules (Phases 1–3 conversational; Phase 4 Edit + commit; Phase 5 Edit; Phase 6 conversational) absent from spec §4 Output topology. SKILL.md OUTPUT FORMAT-absent-from-spec class 3rd data point (N=2 D-3 / N=3 D-3 / N=6 D-3). | (a) amend spec §4 — operator (c)→(a) override per N=2/N=3 precedent for same class (Recommended selected) |

**Cross-skill pattern observations (queued for closing summary):**

- **ASPP citation discipline (N=6 confirmation; full quintet sweep clean).** spec-amend §3 line 53 cites [tech-stack.md §21-33](../tech-stack.md#L21-L33) correctly (heading line); §6 Adoptability NFR cites same. N=1/N=2/N=3/N=4/N=5/N=6 baseline pattern holds. **The discipline is not broken anywhere in the legacy quintet plus N=1 baseline — six-spec sweep clean.**
- **Session-economy commitment propagation — final closure verified.** spec-amend's single shape (i) §5-enumerated mapping (retro §5.4 + §5.5 + INPUTS contract ↔ session-economy §5.3) verified at CP-2 by reading [session-economy §5.3 lines 123–145](../20260514-session-economy/architecture.md#L123-L145) directly: §5.3 prescribes exactly three SKILL.md additions (INPUTS entry + Phase 4 paragraph + Phase 5 note), all three present in SKILL.md ([line 28](../../.agents/skills/spec-amend/SKILL.md#L28), [line 130](../../.agents/skills/spec-amend/SKILL.md#L130), [line 152](../../.agents/skills/spec-amend/SKILL.md#L152)). **Closure at N=6:** retro §5.4 + §5.5 + INPUTS contract ↔ session-economy §5.3 (three additions). **Zero shape (ii) claims at N=6.** Full session-economy propagation map across the quintet: spec-execute (four mappings, shapes (i)+(ii)); spec-review (one mapping, two additions, shape (i) only); spec-amend (one mapping, three additions, shape (i) only). **Every prescribed addition is present and consistent in its target skill.**
- **Two-source structure — shape (i) only, inline-not-standalone predecessor variant.** §3 distinguishes predecessor doc (inline at [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 391–403 + line 414, extracted into standalone skill at trilogy commit `80000b1`) + sibling design spec (session-economy §5.3); §8 carries both Predecessor-cross-check and Sibling-design-spec-cross-check rows; §14 separates Authoritative (SKILL.md + session-economy + tech-stack + mission + roadmap) from Inspirational (predecessor + N=1–N=5 specs). Pattern terminal at N=6 — no N=7 successor.
- **Section-heading citation discipline (N=6 confirmation).** All §3 tech-stack.md citations verified at heading lines per CP-1 verification trail and re-verification here. N=2/N=3/N=4/N=5 corrective holds at N=6.
- **Amendment-ID citation correctness post-amendment-2026-05-18-3 confirmed at spec-amend endpoint.** §5.9 line 225 now reads `[project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md)` — correct holder. Both endpoints of the first post-trilogy cross-skill amendment cycle (N=5 §5.11 + N=6 §5.9) clean at CP-2. **Cross-skill amendment cycle's mechanics fully verified through both endpoints' CP-2 audits.**
- **"WHAT NOT TO DO partial home" finding class — six consecutive data points (D-1 at N=6).** N=1 D-4 / N=2 D-4 / N=3 D-1 / N=4 D-1+D-2 / N=5 D-3 / N=6 D-1. **Pattern fully stable across the legacy quintet plus N=1.** Each session has produced at least one WND-partial-home finding; six-for-six.
- **SKILL.md preamble-vs-body mirror class — six consecutive data points (D-2 at N=6).** N=2 D-2 / N=3 D-5 / N=4 D-3+D-4 / N=5 D-4 / N=6 D-2. **Pattern fully stable.** N=6 D-2 introduces a third flavor (preamble-omits-sibling-caller while frontmatter + HANDOFF NOTES name it); flavors are now: (i) preamble-omits-Phase-content (N=2 D-2, N=3 D-5); (ii) frontmatter-vs-preamble pairing list (N=4 D-3, N=5 D-4); (iii) preamble-omits-sibling-caller (N=6 D-2).
- **SKILL.md OUTPUT FORMAT items absent from spec — three data points (D-3 at N=6).** N=2 D-3 / N=3 D-3 / N=6 D-3. Recurs after two-session pause (N=4/N=5 non-fire). Lower-frequency than the two preceding classes but real.
- **New SKILL.md internal stale-citation class (N=5 D-1) did NOT fire at N=6.** spec-amend SKILL.md "Notes on what makes this skill load-bearing" §"Visibility is the point" correctly historizes the pre-trilogy state ("The previous design (folding the Amendment Protocol into `spec-execute`) worked, but amendments were buried inside execution sessions."). **Class confirmed not universal across the quintet** — spec-review's Design Notes section was the only instance. Pattern observation for the closing summary: class may be specific to skills whose introductory section was authored pre-trilogy-commit and not refactored at the commit.
- **Operator (c)→(a/b) override pattern applied at two findings (D-1, D-3) — four consecutive sessions load-bearing (N=3/N=4/N=5/N=6).** Both override candidates confirmed (a) by operator via AskUserQuestion; both Recommended options selected. D-2 routed (b) directly (no override needed). **Recommended-discipline holds across all four sessions where the override has been exercised.**
- **Status-banner-lifecycle finding class (N=2 D-1) — three consecutive non-fires (N=4/N=5/N=6).** spec-amend §1 banner reads `Draft — Open for Review` but no §5 commits to a Draft → Approved → Superseded lifecycle. **Class confirmed as N=2-unique (spec-design-specific).**
- **Cross-skill amendment coordination (§13 OQ-4) — anchored with one verified-clean worked example.** Amendment 2026-05-18-3's both endpoints (N=5 §5.11 + N=6 §5.9) clean at CP-2. One cycle anchors the watch item; per [N=6 amendment entry "Cross-skill note — codification candidate for SKILL.md"](../20260518-spec-amend-skill/journal.md), two cycles would justify codifying convention in spec-amend SKILL.md as a §"Cross-skill case" addition. Watch item stays open with first observation anchored.

**Routing tally so far (N=2 + N=3 + N=4 + N=5 + N=6, post-operator-decision):** amend-SKILL.md ×10 (N=2's D-1+D-2, N=3's D-2+D-5, N=4's D-3+D-4, N=5's D-1+D-2+D-4, N=6's D-2), amend-spec ×11 (N=2's D-3+D-4, N=3's D-1+D-3+D-4, N=4's D-1+D-2+D-5, N=5's D-3, N=6's D-1+D-3), accept ×2 (N=2's D-5, N=5's D-5). **N=6 closes the ratio at amend-SKILL.md ×10 vs amend-spec ×11** — nearly balanced over five sessions and 23 total advisory routings. Cross-cutting amendment opportunities (per [strategy OQ-3](../../docs/retroactive-spec-strategy.md)) visible from this final tally:
- **Three SKILL.md preamble/frontmatter consistency edits across the quintet** (N=4 D-3 + N=5 D-4 + N=6 D-2) — the preamble-vs-body mirror class is the cross-cutting amendment opportunity most visible in the routing pattern.
- **Six spec amendments landing WND items into §5/§6 carriers across the quintet** (N=1 D-4, N=2 D-4, N=3 D-1, N=4 D-1+D-2, N=5 D-3, N=6 D-1) — the WND-partial-home class is the largest cross-cutting amendment opportunity, six-for-six sessions.
- **Three spec amendments landing OUTPUT FORMAT items into §4/§5/§6 carriers across the quintet** (N=2 D-3, N=3 D-3, N=6 D-3) — lower-frequency but real.

## Closing summary

> Written 2026-05-19, after N=6 CP-2 re-verification closed (commit `4859929`) and all six per-spec CP-2 entries are complete. Captures the cross-skill layer per [strategy doc "Drift mitigation — CP-2 batch audit"](../../docs/retroactive-spec-strategy.md#drift-mitigation--cp-2-batch-audit), proposes cross-cutting codification candidates per [strategy OQ-3](../../docs/retroactive-spec-strategy.md#oq-3--cross-skill-amendment-coordination), and issues the readiness verdict for `docs/retroactive-spec-pattern.md` per [spec-amend §11 step 4](../20260518-spec-amend-skill/architecture.md#L254).

**Evidence base.** Six CP-2 entries: N=1 in [project-constitution journal §"Review of CP-2"](../20260517-project-constitution-skill/journal.md#L248) (standalone session, baseline); N=2 through N=6 above in this journal. Six verdicts: all `pass with comments`. Aggregate findings: **27 advisory, 0 important, 0 blocker** (4 + 5 + 5 + 5 + 5 + 3 by session). All resulting amendments have landed and CP-2 has closed on each spec.

### Cross-skill divergence summary

**Three stable finding classes** survive at session frequencies ≥3/6:

| Class | Freq. | Session evidence | Class shape |
|---|---|---|---|
| WND-partial-home | 6/6 | N=1 D-4, N=2 D-4, N=3 D-1, N=4 D-1+D-2, N=5 D-3, N=6 D-1 | A SKILL.md WHAT NOT TO DO item lacks an explicit §5 phase carrier or §6 NFR carrier in the spec. "Behavioral coverage is fine" silent-default produces under-coverage; the [N=3-close](#n3--2026-05-18--spec-write-cp-2) protocol ("walk WND items against §5/§6 carriers as a discrete step") fired every session it was applied. |
| Preamble-vs-body mirror | 5/6 (absent N=1) | N=2 D-2, N=3 D-5, N=4 D-3+D-4, N=5 D-4, N=6 D-2 | SKILL.md preamble (frontmatter `description:` line or the `How this skill works` block) enumerates a different set of phase content, sibling pairings, or callers than the Phase body does. Three flavors observed: (i) preamble-omits-Phase-content; (ii) frontmatter-vs-preamble pairing-list mismatch; (iii) preamble-omits-sibling-caller while frontmatter + HANDOFF NOTES name it. |
| OUTPUT FORMAT-absent-from-spec | 3/6 | N=2 D-3, N=3 D-3, N=6 D-3 | SKILL.md OUTPUT FORMAT block carries rules (code-block-language; per-phase manifestation) absent from spec §4/§5/§6. Two-session pause at N=4/N=5; recurs at N=6. Lower frequency than the prior two classes but real. |

**Two finding classes confirmed non-universal**:

- **Status-banner-lifecycle** (N=2 D-1 only). Three consecutive non-fires (N=4/N=5/N=6) confirm the class is spec-design-specific — it surfaces in design specs that *also* declare a Draft → Approved → Superseded lifecycle commitment.
- **SKILL.md internal stale-citation** (N=5 D-1 only). N=6 non-fire confirms non-universal; class is specific to skills whose introductory section was authored pre-trilogy-commit (commit `80000b1`) and not refactored at the commit.

**Six-spec routing tally (post-operator-decision):**

| Routing | N=1 | N=2 | N=3 | N=4 | N=5 | N=6 | Total |
|---|---:|---:|---:|---:|---:|---:|---:|
| (a) amend-spec | 2 | 2 | 3 | 3 | 1 | 2 | **13** |
| (b) amend-SKILL.md | 1 | 2 | 2 | 2 | 3 | 1 | **11** |
| (c) accept | 1 | 1 | 0 | 0 | 1 | 0 | **3** |
| Total | 4 | 5 | 5 | 5 | 5 | 3 | **27** |

Notable skews: N=4 added three amend-spec via the (c)→(a) operator override (the protocol-detail-surfacing pattern, observation 1 from N=3 close); N=5 inverted with three amend-SKILL.md driven by SKILL.md internal inconsistency findings. Across the six sessions, **amend-SKILL.md and amend-spec are nearly balanced (11 vs 13)** — the SKILL.md-is-canonical bias predicted at the [N=1 Pattern-for-N=2 first paragraph](../20260517-project-constitution-skill/journal.md#L280) was countered by the (c)→(a) override pattern surfacing previously-silent protocol-detail findings as active amendments.

### Cross-cutting amendments proposed

Per [strategy OQ-3](../../docs/retroactive-spec-strategy.md#oq-3--cross-skill-amendment-coordination): cross-skill amendments worth proposing as a single coherent change. **All 27 per-finding amendments already landed individually**; the items below are codification candidates that would *prevent* the stable finding classes from recurring at future authoring or audit time, not retrofit the existing specs.

**Codification candidate 1 — Codify the WND / preamble-vs-body / OUTPUT-FORMAT walks as first-class CP-2 audit steps in `spec-review` SKILL.md.** The three stable finding classes share a structural shape: SKILL.md content that the spec's §4/§5/§6 walk does not naturally surface unless audited explicitly. The [N=3-close](#n3--2026-05-18--spec-write-cp-2) protocol fired six-for-six when applied. Codifying the walk-protocol as named CP-2 audit steps in spec-review SKILL.md removes the need for future CP-2 audits to mine prior journals for the discipline. Proposed landing point: [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md) drift-audit review-focus section. **Highest urgency** of the three candidates — the three classes are the most-evidenced output of the entire batch.

**Codification candidate 2 — Add a `§"Cross-skill case"` section to `spec-amend` SKILL.md after a second cycle is observed.** Amendment 2026-05-18-3 (post-trilogy cross-skill amendment cycle, anchored by N=5 §5.11 + N=6 §5.9, both verified clean at CP-2) is the first worked example for [strategy OQ-3](../../docs/retroactive-spec-strategy.md#oq-3--cross-skill-amendment-coordination). Per the [N=6 amendment entry "Cross-skill note — codification candidate for SKILL.md"](../20260518-spec-amend-skill/journal.md), one cycle anchors the watch item but does not yet justify codification. **Status update 2026-05-20: codification applied.** Amendment 2026-05-19-1 provided the second cycle (six-spec methodology-level decision); codification landed at [.agents/skills/spec-amend/SKILL.md §"Cross-skill case"](../../.agents/skills/spec-amend/SKILL.md) under amendment 2026-05-19-2.

**Codification candidate 3 — Add an authoring-time per-citation walk to `spec-write` / `spec-design` SKILL.md.** The amendment-ID citation-error class surfaced at N=5 (cross-skill, traced upstream from N=6 CP-1) was caught at sibling-spec CP-1, not at the originating spec's authoring time. Per the [N=5-close](#n5--2026-05-18--spec-review-cp-2) "first evidence of upstream-traced cross-skill citation amendment," an authoring-time per-citation walk would have caught it earlier. Proposed landing point: spec-write and spec-design SKILL.md Phase 3 final-walk step. **Lower urgency** — one occurrence in six sessions; class is not yet confirmed stable.

### Readiness verdict — `docs/retroactive-spec-pattern.md`

**Verdict: Ready.**

**Evidence sufficiency.** The six-spec CP-2 sweep produces a stable evidence base:

- Six per-spec CP-2 entries, all `pass with comments`, with 27 advisory findings, 0 important, 0 blocker — no substantive drift at the spec ↔ SKILL.md boundary.
- Three stable finding classes confirmed at frequencies 6/6, 5/6, and 3/6 across the six sessions; two finding classes confirmed non-universal by consecutive non-fires.
- One cross-skill amendment cycle (2026-05-18-3) anchored with both endpoints CP-2-clean — mechanics verified, scope-limited.
- Six-spec routing tally near-balanced (amend-spec ×13, amend-SKILL.md ×11, accept ×3); the (c)→(a) override pattern's effect on the SKILL.md-canonical bias is measured and named.
- Constitutional binding ([Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) citation discipline) clean across all six specs — a six-spec sweep with zero broken citations on the constitutional surface.
- Two-source structure (predecessor + sibling design spec) reproduced across all five legacy-quintet specs with shapes (i) and (ii) differentiated; spec-amend (N=6) confirmed the inline-not-standalone predecessor variant.

The evidence base is sufficient to support a pattern doc that codifies *what worked* (session shape, constitutional binding, two-source structure, the (c)→(a) override discipline, the three audit walks) and *what to watch* (cross-skill amendment cycles, authoring-time citation walks).

**Pattern-doc scope leaning.** Recommended that the next session author `docs/retroactive-spec-pattern.md` covering, at minimum:

1. **Session shape** — orient → Discovery → Clarify → Spec Document → CP-1 → CP-2, derived from the N=1 + N=2-N=6 actual sessions, with per-session deviations explicit.
2. **Constitutional binding checklist** — every retroactive spec must cite (in §3 Background and §6 NFRs) the [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33), [AI context window limits](../tech-stack.md#L44), and [spec-driven-development convention](../tech-stack.md#L51); audience reusable verbatim from [N=1](../20260517-project-constitution-skill/architecture.md).
3. **Two-source structure** — predecessor doc + sibling design spec, with shape (i) §5-enumerated mappings and shape (ii) narrative-sourced mappings differentiated. Predecessor doc may be inline (extracted at trilogy commit) per N=6.
4. **The three stable finding classes as authoring-time pre-empt protocols** — WND walk, preamble-vs-body walk, OUTPUT FORMAT walk — surfaced as authoring-time §4/§5/§6 carrier discipline, not just audit-time discovery. (Independent of, but complementary to, codification candidate 1.)
5. **The (c)→(a) operator override pattern** — protocol-detail findings should be surfaced explicitly with Recommended option, not absorbed silently into accept-as-known-minor. Four consecutive sessions (N=3–N=6) confirm the pattern is load-bearing.
6. **Cross-skill amendment mechanics** — anchored by the 2026-05-18-3 cycle; framed as a watch item until a second cycle is observed (per codification candidate 2 above).

The pattern doc is the next-action handoff per [spec-amend §11 step 4](../20260518-spec-amend-skill/architecture.md#L254) and the [N=6 next-action](../20260518-spec-amend-skill/journal.md). This Closing summary closes the batched CP-2 audit; the pattern doc is its own session.

### Status

**Batch CP-2 audit: closed 2026-05-19.** All six per-spec CP-2 entries complete; cross-skill synthesis recorded; readiness verdict issued. No further entries against this journal — successor work is `docs/retroactive-spec-pattern.md` in a fresh session.

> Coda update 2026-05-19: this journal carries one further entry below — amendment 2026-05-19-1, the methodology-level post-CP-2 banner transition that the closing summary anticipated. The "no further entries" line above is preserved as the closing statement at batch-CP-2 closure; the amendment is documented here because the batch journal is its natural anchor (W-1 second cycle, codification candidate 2 satisfied). No per-spec CP-2 entries are added.

## 2026-05-19 — Amendment 2026-05-19-1 (cross-skill — first execution of post-CP-2 banner transition)

**Section amended:** §1 Status banner across six retroactive design specs — [N=1 project-constitution](../20260517-project-constitution-skill/architecture.md#L3), [N=2 spec-design](../20260518-spec-design-skill/architecture.md#L3), [N=3 spec-write](../20260518-spec-write-skill/architecture.md#L3), [N=4 spec-execute](../20260518-spec-execute-skill/architecture.md#L3), [N=5 spec-review](../20260518-spec-review-skill/architecture.md#L3), [N=6 spec-amend](../20260518-spec-amend-skill/architecture.md#L3). All six at `architecture.md:3`.
**Trigger:** First execution of the post-CP-2 banner transition promised in N=5/N=6 "Review of CP-2" entries and deferred at N=4/N=5/N=6 CP-2 re-verification entries for want of a methodology-level decision defining the successor state.
**Reason:** Six consecutive retroactive-spec sessions documented the open question; this amendment is itself the methodology-level decision, defining `Approved — CP-2 closed YYYY-MM-DD` as the post-`Draft — Open for Review` successor state and applying it retroactively across all six specs.
**Impact summary:** No tasks (design specs); no checkpoints re-opened; no completed work invalidated. Two adjacent codification triggers fire (W-1, codification candidate 2 — both "second cycle observed" conditions met) but codification itself is out of scope.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-19
**Status implication:** **forward advancement** (Draft → Approved). First instance of forward status advancement in the methodology — the prior nine /spec-amend invocations (2026-05-17-1 through 2026-05-18-6 across N=1..N=6) all carried `Status implication: kept`. Surfaced explicitly via AskUserQuestion at Phase 2 per [feedback-spec-amend-status-implication memory](file:///Users/eric/.claude/projects/-Users-eric-scm-github-waseric-ai-tools/memory/feedback-spec-amend-status-implication.md) — the journals' "banner amendment to follow" promise (Review-of-CP-2 entries) and "banner stays at Draft, successor state undefined" precedent (subsequent CP-2 re-verification entries) were both shown to the operator alongside the proposed forward advancement. Operator confirmed.
**Commit:** `09b6131` (six architecture.md banner edits); `09ba0e2` (cross-skill anchor + 6 paired companion journal entries).

### Full record

## Amendment 2026-05-19-1 — §1 Status banner across N=1..N=6 retroactive design specs (cross-skill)

**Trigger.** Six retroactive design specs (N=1 project-constitution through N=6 spec-amend) each carry `> Status: Draft — Open for Review` despite all six having closed CP-2 across 2026-05-18 → 2026-05-19 (per [Closing summary](#closing-summary) above). The post-CP-2 transition was named as a "banner amendment to follow" in [N=5 "Review of CP-2":314](../20260518-spec-review-skill/journal.md#L314) and [N=6 "Review of CP-2":343](../20260518-spec-amend-skill/journal.md#L343), but each successive CP-2 re-verification entry (N=4 commit `49d898c`, N=5 commit `baac6d6`, N=6 commit `4859929`) deferred the transition on the grounds that no methodology-level decision had defined the successor state. The user's invocation of /spec-amend on 2026-05-19 defined the successor state — `Approved — CP-2 closed YYYY-MM-DD` — and applied it as a single cross-skill amendment to all six specs.

**Section.** Six edits, one per spec, all at architecture.md line 3 (§1 frontmatter Status line):

| # | Spec | File | Per-spec CP-2 closeout date | CP-2 closeout commit |
|---|---|---|---|---|
| N=1 | project-constitution | [architecture.md:3](../20260517-project-constitution-skill/architecture.md#L3) | 2026-05-18 | `444fec2` |
| N=2 | spec-design | [architecture.md:3](../20260518-spec-design-skill/architecture.md#L3) | 2026-05-18 | `70cc10b` |
| N=3 | spec-write | [architecture.md:3](../20260518-spec-write-skill/architecture.md#L3) | 2026-05-18 | `d4b4bff` |
| N=4 | spec-execute | [architecture.md:3](../20260518-spec-execute-skill/architecture.md#L3) | 2026-05-18 | `49d898c` |
| N=5 | spec-review | [architecture.md:3](../20260518-spec-review-skill/architecture.md#L3) | 2026-05-18 | `baac6d6` |
| N=6 | spec-amend | [architecture.md:3](../20260518-spec-amend-skill/architecture.md#L3) | 2026-05-19 | `4859929` |

**Change.**

Before (byte-identical across all six):

> ```
> > Status: Draft — Open for Review
> ```

After (N=1 through N=5):

> ```
> > Status: Approved — CP-2 closed 2026-05-18
> ```

After (N=6 only):

> ```
> > Status: Approved — CP-2 closed 2026-05-19
> ```

The N=6 date differs because the CP-2 re-verification commit landed on 2026-05-19 (`4859929` 2026-05-19 06:20:59 -0400); the other five closed on 2026-05-18. The journal entry text in N=6 stamps "2026-05-18" but the commit timestamp is the truth-of-closure for the banner; operator confirmed the commit-date interpretation via AskUserQuestion at Phase 2.

**Reason.** Six consecutive retroactive-spec sessions deferred the post-CP-2 banner transition because no methodology-level decision had defined a successor state. The "Review of CP-2" entries promised a banner amendment to follow; the subsequent CP-2 re-verification entries documented the deferral, explicitly framing it as a methodology-level open question (verbatim from [N=6 CP-2 re-verification:526](../20260518-spec-amend-skill/journal.md#L526): *"the post-CP-2 successor state remains undefined across the legacy quintet plus N=1, a six-data-point disposition that the closing summary will codify"*). This amendment closes the question by defining `Approved — CP-2 closed YYYY-MM-DD` as the successor state and applying it retroactively across all six specs in a single coherent change. The successor state name `Approved` matches the lifecycle term [spec-design's §5.8](../20260518-spec-design-skill/architecture.md) commits to (`Draft — Open for Review → Approved → Superseded`), unifying the methodology's lifecycle vocabulary. The `— CP-2 closed YYYY-MM-DD` suffix preserves the per-spec closure record in the banner itself, complementing each spec's §9 CP-2 Status line.

**Impact.**
- **Affected tasks:** none (six design specs, no atomic tasks).
- **Affected checkpoints:** none re-opened. Each spec's CP-2 is already closed; the banner edit reflects the closure that was deferred for want of a defined successor state.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none required for the spec-level transition. Adjacent observations recorded as cross-skill notes below (not blocking):
  - [docs/retroactive-spec-pattern.md §W-1](../../docs/retroactive-spec-pattern.md) — this amendment is the second cross-skill amendment cycle; W-1's "Current state: One cycle observed" line is now stale; W-1's codification trigger is satisfied.
  - [Codification candidate 2](#cross-cutting-amendments-proposed) (above) — "Recommendation: hold codification until a second cycle is observed" condition met.
  - [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) — has no §"Cross-skill case" section yet. Codification is **out of scope for this amendment**; per W-1's "do not codify until N events fire" discipline, the trigger lands here and the codification is the next-action handoff.

**Status implication.** **Forward advancement.** Each affected spec advances from `Draft — Open for Review` to `Approved — CP-2 closed <date>`. This is the **first instance of forward status advancement in the methodology** — the prior nine /spec-amend invocations all carried `Status implication: kept`. Surfaced explicitly per the [feedback-spec-amend-status-implication memory](file:///Users/eric/.claude/projects/-Users-eric-scm-github-waseric-ai-tools/memory/feedback-spec-amend-status-implication.md): the journals' two competing precedents ("banner amendment to follow" promise vs. "banner stays at Draft, successor state undefined") were surfaced to the operator via AskUserQuestion at Phase 2 of this skill session along with two adjacent choices (N=6 date interpretation; cross-skill bundling). Operator confirmed all three: define successor as Approved, use 2026-05-19 for N=6, bundle as single ID 2026-05-19-1.

**Approver.** Eric Wasgatt, 2026-05-19.

### Cross-skill notes (second post-trilogy cycle — W-1 anchor evidence)

This is the **second post-trilogy cross-skill amendment cycle**, mirroring the [2026-05-18-3 cycle](../20260518-spec-amend-skill/journal.md#L199) but with broader scope (six specs vs. two) and a different originating trigger (methodology-level decision vs. CP-1 finding). The two cycles together anchor W-1 with two data points across distinct trigger classes, which is the codification-readiness signal.

| W-1 four-step mechanic | 2026-05-18-3 (first cycle) | 2026-05-19-1 (second cycle, this amendment) |
|---|---|---|
| 1. Surface | N=6 CP-1 found §5.9 citation error | Methodology-level open question documented across N=4/N=5/N=6 re-verifications |
| 2. Trace upstream | N=5 §5.11 carried same error | All six specs share the §1 banner state and the same deferred transition |
| 3. Apply as single amendment ID | `2026-05-18-3` across 2 specs in two-commit shape (architecture + journals) | `2026-05-19-1` across 6 specs in same two-commit shape |
| 4. Verify at both endpoints | N=5 §5.11 + N=6 §5.9 verified clean at CP-2 | Six banners verified post-edit (`grep ^> Status:` after commit `09b6131` confirms all six show `Approved — CP-2 closed <date>`) |

The mechanics generalize cleanly from N=2 to N=6 scope. The two-commit shape (`spec-edit commit` + `journal commit`, both citing the amendment ID) holds. The primary-record + companion-record discipline holds, with the primary record migrating from the originating spec's journal (2026-05-18-3 in spec-amend journal) to the methodology-level journal that anchors the cycle (2026-05-19-1 in this batch journal) — a refinement of the convention, not a contradiction. Refinement worth codifying: the primary record lives at the cycle's natural anchor — the originating CP-1 verdict's spec journal when the trigger is a per-spec finding, the batch journal when the trigger is a methodology-level decision.

### W-1 codification trigger satisfied — next-action handoff

Per [retroactive-spec-pattern.md §W-1](../../docs/retroactive-spec-pattern.md):

> **Codification trigger.** Add a §"Cross-skill case" section to [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) prescribing the four-step mechanics (surface → trace upstream → apply as single amendment ID → verify at both endpoints) once a second cycle anchors the convention as load-bearing.

The second cycle is now observed. The codification is the next-action handoff out of this amendment. It is **deliberately out of scope** for the amendment itself (W-1's "do not codify until N events fire" discipline is about *what triggers codification*, not *whether codification happens in the same session*). The next session — either a fresh /spec-amend invocation against the spec-amend SKILL.md as the target artifact, or a /spec-design session re-opening the spec-amend spec to amend the SKILL.md spec-side first — adds the §"Cross-skill case" section to spec-amend SKILL.md. The codification should reflect the two-cycle refinement of the primary-record location rule (per the table above).

### Methodology-level decision recorded — post-`Draft` successor state defined

The amendment carries an embedded methodology-level decision: `Approved — CP-2 closed YYYY-MM-DD` is now the defined post-`Draft — Open for Review` successor state for retroactive design specs (and, by extension via [spec-design §5.8](../20260518-spec-design-skill/architecture.md)'s lifecycle declaration `Draft — Open for Review → Approved → Superseded`, for design specs in general). The decision is recorded here because:

- The successor-state question was repeatedly framed in the journals as methodology-level (N=4 / N=5 / N=6 re-verification entries).
- No prior methodology artifact (constitution, strategy doc, pattern doc) names the successor state.
- The batch journal is the natural anchor — its closing summary already named "post-CP-2 banner transition" as forward-looking work tied to the readiness verdict.

A future session may elevate the decision to a methodology-level artifact — e.g., adding a "Status lifecycle" subsection to [docs/retroactive-spec-pattern.md](../../docs/retroactive-spec-pattern.md), or strengthening [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) Phase 3's status-banner-template prescription with the successor states. The pattern doc maintenance trigger W-3 names on-demand maintenance for such decisions.

### Companion records

The six per-spec journals carry **companion entries** that reference this primary record by amendment ID without duplicating the structured block. Per the 2026-05-18-3 precedent: cross-reference by amendment ID preserves a single source of truth. The companion entries land in:

- [specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md) (N=1)
- [specs/20260518-spec-design-skill/journal.md](../20260518-spec-design-skill/journal.md) (N=2)
- [specs/20260518-spec-write-skill/journal.md](../20260518-spec-write-skill/journal.md) (N=3)
- [specs/20260518-spec-execute-skill/journal.md](../20260518-spec-execute-skill/journal.md) (N=4)
- [specs/20260518-spec-review-skill/journal.md](../20260518-spec-review-skill/journal.md) (N=5)
- [specs/20260518-spec-amend-skill/journal.md](../20260518-spec-amend-skill/journal.md) (N=6)
