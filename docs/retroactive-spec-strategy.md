# Retroactive-Spec Strategy — Legacy Quintet

> Status: Planning record — operator-approved 2026-05-17
> Audience: The operator (Eric) and any future session (human or AI) authoring one of the legacy-quintet retroactive design specs
> Companion artifacts: [specs/20260517-project-constitution-skill/architecture.md](../specs/20260517-project-constitution-skill/architecture.md), [specs/20260517-project-constitution-skill/journal.md](../specs/20260517-project-constitution-skill/journal.md)

## Purpose

This doc captures the strategy for retrofitting design specs against the five spec-driven-core skills that currently ship without specs. It is a planning record, not a spec. Each retroactive spec is authored in its own session against this strategy as orientation material.

The decision to commit this as a planning doc (rather than start authoring the first spec immediately) was made to preserve session economy: each retroactive spec requires a full Phase 1/2/3 cycle, and orienting a fresh session against this doc is cheaper than orienting against five separate journals + the constitution from scratch.

## Scope

### In scope — five retroactive design specs

| # | Skill | Path |
|---|-------|------|
| 1 | `spec-design` | [.agents/skills/spec-design/SKILL.md](../.agents/skills/spec-design/SKILL.md) |
| 2 | `spec-write` | [.agents/skills/spec-write/SKILL.md](../.agents/skills/spec-write/SKILL.md) |
| 3 | `spec-execute` | [.agents/skills/spec-execute/SKILL.md](../.agents/skills/spec-execute/SKILL.md) |
| 4 | `spec-review` | [.agents/skills/spec-review/SKILL.md](../.agents/skills/spec-review/SKILL.md) |
| 5 | `spec-amend` | [.agents/skills/spec-amend/SKILL.md](../.agents/skills/spec-amend/SKILL.md) |

Output for each: `specs/YYYYMMDD-<skill-name>-skill/architecture.md` + `specs/YYYYMMDD-<skill-name>-skill/journal.md`, following the directory-slug pattern established at [specs/20260517-project-constitution-skill/](../specs/20260517-project-constitution-skill/) ("N=1 pattern" in the source journal).

### Out of scope — already covered

- `project-constitution` — retroactive spec landed 2026-05-17; CP-1 passed-with-comments; one amendment applied; **CP-2 drift audit still pending** (see Open questions below).
- `finding-intake`, `finding-triage` — both have feature specs ([feature.md](../specs/20260517-finding-intake-skill/feature.md), [feature.md](../specs/20260517-finding-triage-skill/feature.md)) authored before the skills were built. Design-level coverage exists via the shared [findings-pipeline architecture.md](../specs/20260517-findings-pipeline/architecture.md). No retroactive design spec is needed.

## Operating principles for each retroactive spec

These are non-negotiable. Apply in every session.

1. **Descriptive, not prescriptive.** The shipping SKILL.md is authoritative for behavior. The spec describes commitments; it does not redesign. Any divergence the spec exposes routes to [/spec-amend](../.agents/skills/spec-amend/SKILL.md) — never silent edit.
2. **Bind to the constitution explicitly.** Each spec must cite, in §3 Background and §6 NFRs, the constitutional commitments it inherits:
   - **Atomic-Skill Portability Principle** ([specs/tech-stack.md §21-33](../specs/tech-stack.md#L21-L33)) — every skill is a portable atomic unit; the spec must show how this skill honors it.
   - **AI context window limits** ([specs/tech-stack.md:44](../specs/tech-stack.md#L44)) — propagates into output-sizing commitments where applicable.
   - **Spec-driven-development convention** ([specs/tech-stack.md:51](../specs/tech-stack.md#L51)) — the methodology eats its own cooking; this is the rationale for retroactive spec authoring at all.
   - **Audience and scope** ([specs/mission.md](../specs/mission.md)) — same audience as project-constitution; reusable verbatim.
3. **Mine the prior session's journal first.** Each session begins by reading the prior session's journal and walking its "Pattern for N=2" callouts. Validate, reject, or refine each pattern; record the outcome in the new journal.
4. **No SKILL.md edits mid-session.** Drift findings are recorded in the journal and CP-2 list; resolution is a post-CP-2 amendment.
5. **Self-contained.** The spec must read independently of this strategy doc and of prior-session journals. Cross-link to prior sessions where helpful, but do not assume the reader has read them.

## Order and rationale

| # | Skill | Why this slot |
|---|-------|---------------|
| 1 | `spec-design` | Self-describing: the skill that authored both the project-constitution spec and (recursively) its own spec. Spec'ing it first means sessions 2-5 can cite the design spec, not just the SKILL.md. |
| 2 | `spec-write` | **N=2 inflection point.** Most similar shape to project-constitution (authoring skill, three-phase, operator-facing). Validates the "Pattern for N=2" callouts in the project-constitution journal. **Decision gate: justify `docs/retroactive-spec-pattern.md` or defer further.** |
| 3 | `spec-execute` | **N=3 robustness check.** Most divergent shape (iterative, multi-task, branch-state-aware, paired-commit-aware vs. single-shot authoring). Surfaces what the spec-design / spec-write pattern does *not* generalize to. |
| 4 | `spec-review` | Authoring-time pair to `spec-execute` — review gates produce verdicts; execute consumes them. Spec'ing both before `spec-amend` ensures the amendment skill's spec can cite both. |
| 5 | `spec-amend` | Closes the loop. Smallest cognitive lift since amendments are the simplest workflow; benefits most from accumulated journal mining. |

The order is a recommendation, not a contract. The operator may resequence at session boundaries if context shifts.

## N=2 inflection point — `docs/retroactive-spec-pattern.md` decision

The project-constitution journal explicitly deferred to N=2 the question of whether a documented retroactive-spec pattern is justified. After session 2 (`spec-write`), the operator decides:

- **Yes, pattern is justified** → author `docs/retroactive-spec-pattern.md` capturing the validated patterns; sessions 3-5 cite the pattern doc instead of re-deriving.
- **No, patterns are too skill-specific** → defer again or abandon the idea; sessions 3-5 continue to mine prior journals individually.

This is not a blocker for session 2; the decision is made at session 2's close, in its journal. Either outcome is acceptable.

## Drift mitigation — CP-2 batch audit

Each retroactive spec declares two checkpoints (per the project-constitution precedent):
- **CP-1** — Faithfulness review: every commitment in the spec corresponds to behavior in SKILL.md; no commitment contradicts SKILL.md.
- **CP-2** — Drift audit: line-by-line comparison of SKILL.md against the spec; divergence list (possibly empty) produced.

**Strategy for CP-2: batch.** After all five specs are authored and their CP-1 reviews pass, run a single dedicated session that conducts CP-2 drift audits for all five (or six, if project-constitution's CP-2 is still pending — see Open questions). Rationale: cross-skill drift patterns are only visible when audited together. If four skills cite the Atomic-Skill Portability Principle correctly and one quietly doesn't, that gap is only catchable at batch time.

CP-1 reviews remain per-session — a faithfulness review is naturally scoped to a single spec.

## Decisions recorded 2026-05-18

The CP-2 batch session was scoped during a `/spec-execute retroactive-spec-strategy CP-2` invocation that surfaced a skill-shape mismatch — this strategy doc is a planning record, not a feature spec with task breakdown, so `/spec-execute`'s Phase 1 cannot orient against it. The operator resolved scope and recorded the following decisions:

- **CP-2 driver skill: `/spec-review` per spec, sequential.** Each retroactive spec's CP-2 is the canonical drift-audit shape, which fits [/spec-review](../.agents/skills/spec-review/SKILL.md) natively. `/spec-execute` is not used to orchestrate the batch — its task-breakdown / DoD discipline does not map onto review activities.
- **OQ-1 resolved as (a).** Project-constitution's CP-2 runs first as its own standalone session, establishing the N=1 drift-audit baseline. The five new specs' batch CP-2 then inherits the pattern. Lean (a) was the strategy's stated preference; this records the formal resolution.
- **Batch journal location: [specs/20260518-cp2-batch-audit/journal.md](../specs/20260518-cp2-batch-audit/journal.md).** A standalone journal for the five-spec batch session, scaffolded in the same session that recorded these decisions. Each spec's own journal still gets its CP-2 closeout entry; the batch journal captures the *cross-spec* layer (drift patterns visible only when audited together). Project-constitution's CP-2 journal entry lives in [its own existing journal](../specs/20260517-project-constitution-skill/journal.md) per the standalone-session shape.

## Session shape (per retroactive spec)

Each session follows this rough shape. It is not a binding template — adapt as conditions warrant.

1. **Orient.** Read this strategy doc; read the prior session's journal; read the shipping SKILL.md being spec'd; read the constitution ([mission.md](../specs/mission.md), [tech-stack.md](../specs/tech-stack.md), [roadmap.md](../specs/roadmap.md)).
2. **Phase 1 — Discovery** (per [spec-design SKILL.md](../.agents/skills/spec-design/SKILL.md)). Source-file selection (include negative-signal rows per N=1 pattern); landscape orientation; prior-art scan.
3. **Phase 2 — Clarify.** Confirm artifact name + directory slug (use `<skill-name>-skill` per N=1); confirm audience (reusable verbatim from N=1); confirm verification commitment (light verification suffices for all five, per N=1); triage open questions.
4. **Phase 3 — Spec document.** Author `architecture.md` + `journal.md` paired. Journal records "Pattern for N=2" callouts validated/refined/added (or "Pattern for N=3" at N=2 inflection and beyond).
5. **Commit.** Paired commit of architecture.md + journal.md. CP-1 may be triggered in the same session or deferred to a fresh session (operator's choice per N=1 precedent).

## Open questions for fresh sessions to consider

These are not blockers. Each is named so a future session is not surprised when it surfaces.

### OQ-1 — Project-constitution CP-2 batch timing

Project-constitution's CP-2 drift audit is still pending ([journal.md](../specs/20260517-project-constitution-skill/journal.md) "Next action" item 3). Two reasonable resolutions:
- (a) Run project-constitution's CP-2 first as its own session, establish N=1 baseline for what a drift audit looks like, *then* the five new specs' batch CP-2 inherits the pattern.
- (b) Fold project-constitution into the six-spec batch CP-2 audit at the end.

Leaning: (a) — gives the batch audit a worked example to model against. **Resolved 2026-05-18 as (a)** — see ["Decisions recorded 2026-05-18"](#decisions-recorded-2026-05-18) section above.

### OQ-2 — Skill-spec ordering if downstream specs need upstream spec citations

The proposed order assumes session 2+ can cite session 1's spec by path. If session 2 (spec-write) needs to cite session 1's spec for a structural choice (e.g., "matches the §9 Review Checkpoints shape declared in `specs/.../spec-design-skill/architecture.md`"), and session 1 hasn't yet been committed, the citation has to wait. Likely-fine in practice (operator works one session at a time and commits before moving on), but worth flagging.

### OQ-3 — Cross-skill amendment coordination

If a CP-2 drift audit reveals that two skills both diverge from an inherited convention (e.g., both miss a citation of the Atomic-Skill Portability Principle), the amendment may be a single coherent change across both SKILL.md files, not two independent amendments. The amendment-protocol mechanics for cross-artifact changes are not declared anywhere; [spec-amend SKILL.md](../.agents/skills/spec-amend/SKILL.md) currently scopes to single-spec amendments. A cross-cutting amendment may surface as an open question on the spec-amend retroactive spec itself.

### OQ-4 — Constitution-amendment ceremony (inherited)

The pending finding-intake at [docs/constitution-amendment-gap-intake-prep.md](./constitution-amendment-gap-intake-prep.md) (also named as OQ-1 in the project-constitution spec) is still unresolved. If any retroactive spec's drift audit triggers an amendment to the constitution itself (e.g., a tech-stack.md edit), the operator faces the same gap. Carry forward to each session as a known risk; do not block on resolution.

## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Pattern over-codification at N=2 forecloses better patterns for N=3+ | Medium | Medium | The N=2 inflection point makes the `docs/retroactive-spec-pattern.md` decision deliberate; if leaning is "not yet," defer further. |
| Context-rot across five sessions — operator forgets prior decisions | Medium | Low–Medium | Each journal commits to mineable structure; each session begins by reading the prior journal. |
| Five sessions reveal drift that requires SKILL.md amendments, accumulating amendment debt | Medium | Medium | Batch CP-2 audit collapses amendment planning into one coherent session, not five scattered ones. |
| Spec-design (session 1) recursion confusion — the skill spec'ing itself | Low | Low | The project-constitution journal demonstrated this works in practice; spec-design's spec is descriptive, not a redesign, so recursion is cognitive, not technical. |
| The "legacy trilogy" misnaming in the project-constitution journal causes future readers to look for three skills, not five | Low | Low | This strategy doc names "legacy quintet" explicitly; future retroactive-spec journals reference this doc and inherit the corrected count. |

## References

- [specs/20260517-project-constitution-skill/architecture.md](../specs/20260517-project-constitution-skill/architecture.md) — N=1 retroactive design spec; structural source for sessions 1-5.
- [specs/20260517-project-constitution-skill/journal.md](../specs/20260517-project-constitution-skill/journal.md) — N=1 journal with "Pattern for N=2" callouts; mineable input for session 1.
- [specs/mission.md](../specs/mission.md), [specs/tech-stack.md](../specs/tech-stack.md), [specs/roadmap.md](../specs/roadmap.md) — constitution; constitutional bindings each spec must honor.
- [.agents/skills/spec-design/SKILL.md](../.agents/skills/spec-design/SKILL.md) — the skill authoring each retroactive spec.
- [docs/constitution-amendment-gap-intake-prep.md](./constitution-amendment-gap-intake-prep.md) — inherited unresolved gap referenced by OQ-4.
