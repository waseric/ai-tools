# Journal — Context Working-Set Protocol (CWSP)

## Current State
- **Phase:** Design — Draft + validation + grammar-model increment complete, awaiting CP-1 (design approval)
- **Last completed:** Grammar-declaration model folded in (new §5.7; §2/§4/§5.2/§5.5/§5.6/§6/§7/§10/§14 updated; OQ-6 added) (2026-07-07)
- **Next:** CP-1 design review — resolve/defer §13 open questions (now OQ-1..OQ-6); on approval advance status to Approved and author downstream `spec-write` feature spec
- **Open holds:** OQ-1..OQ-6 open (first-class, see §13); no blockers
- **Pending checkpoint:** CP-1 (Design approval), contract in architecture.md §9
- **Archive:** none — all entries live
- **Latest entry:** 2026-07-07 — Grammar-declaration model

## 2026-07-07 — Design session (spec-design)

**Event.** Authored the CWSP design spec via `spec-design`.

**Inputs / discovery.** Three read-only discovery agents summarized (a) prior token-efficiency work in the suite — dominated by [dispatch-execution](../20260705-dispatch-execution/architecture.md); (b) real-world scale on a sibling-repo feature (spec ~21k tokens, journal ~26k, design doc ~13k; ~48k to get centered; measured 2026-07-07); (c) how each spec-* skill reads spec/journal today — whole-file read is *not* universal (`spec-review` selective, `spec-amend` section-scoped, dispatch worker one-task-scoped), the remaining offender being `spec-execute` Phase 1 "read SPEC_PATH in full" re-run every boundary.

**Reframing adopted.** Token cost is a *read-behavior* problem, not a file-layout problem. Quality comes from the *right* slice, not all of it (dispatch CP-2 cold-read passed with a one-task worker). Governing invariant: split freely, never orphan — a scoped slice must always carry a cheap complete map back to the whole.

**Decisions (operator-confirmed).**
- Vehicle: new standalone design spec (this one), orthogonal to dispatch-execution.
- Name: Context Working-Set Protocol (CWSP).
- Spine: stored STATE (non-derivable "where are we") + grep-derived INDEX (zero-drift) + on-demand BODY units.
- Splitting: a reachable tool (Tier 1 enforcement / Tier 2 contention), not the standard (Tier 0 default). Justified by structural enforcement or write-contention, never by token count.

**External claims verified 2026-07-07.** Denning working-set model (CACM 1968); Nielsen/NN-G progressive disclosure (1995) — cited inline in §3/§14.

**Open questions carried to §13.** OQ-1 STATE trust vs. grep-check; OQ-2 working-set sizing; OQ-3 seal trigger; OQ-4 coupling to dispatch OQ-3 (parallel writers); OQ-5 cold-reader guarantee under sealing.

**Next pointer.** CP-1 design review. Additional operator spec histories are available if broader empirical validation of the scale/structure claims is wanted before approval.

## 2026-07-07 — Anchor-convention validation

**Event.** Validated the grep-derived-INDEX assumption against a second, independent corpus — a separate private repo's `specs/` tree (feature specs, findings-pipeline journals, an architecture journal, and constitution-change records).

**Result: holds with caveats.**
- Journal entries (`^## <date> — `) and spec sections (`^## N\. `) grep cleanly and universally, *including* findings-pipeline and constitution-change journals — methodology scales to those journal types unchanged. No standalone constitution journal exists; constitution changes are amendment entries inside spec journals.
- Corrections vs. draft: sections are `## N.` not `## §N` (the `§N` form is reference-only); tasks are usually `### T-NN` but sometimes a heading-less Markdown table, so the §7 task table is the canonical task index.
- Amendments have three coexisting heading forms (dated `##`, undated `## Amendment`, `### Amendment`); enumeration needs a union pattern.

**Spec changes applied.** §3 added a measured "Anchor-convention validation" block; §5.2 grep contracts rewritten to reality (per-type union patterns, §7 table canonical for tasks, drop `§N`); §5.3 execute-task row note for table-only specs; §10 heading-drift risk upgraded Low→High (measured, not hypothetical) with mitigation = union patterns + canonical amendment heading + lint + amendment de-dup.

**Net.** Core STATE/INDEX/BODY architecture unchanged and reinforced. The only substantive learning is that INDEX cannot assume one anchor convention per element type — it unions patterns and leans on the always-present §7 table for tasks. Feeds the grammar-declaration model (next entry) and the amendment-collapse case (§5.5).

## 2026-07-07 — Grammar-declaration model

**Event.** Operator rejected building linting/validation tooling into the skills. Adopted instead a declarative, self-describing grammar model. Folded into the spec (new §5.7 + cross-section updates).

**Decisions (operator-confirmed).**
- No tooling. Anchor discipline is *declared grammar read as data*, never a conformance checker (§2 non-goal).
- Generalized governing invariant: **declared conveniences, re-derivable truth** (§4) — STATE and grammar dialect are cost hints; ground truth is always re-derivable (widening; discovery). Nothing stored/declared is load-bearing for correctness. This is why no lint is needed.
- Two-tier INDEX (§5.2): declared dialect primary (one clean grep), broad-union discovery fallback for undeclared/legacy.
- Grammar model (§5.7): one override, variable scope (constitution / spec / legacy-artifact), over a skill-template default (the reference dialect). Most-specific-already-read wins.
- Resolution is a **free-rider on normal reads**: early writers (spec-design/write) read the constitution during discovery and codify any grammar forward into the spec; late writers (spec-execute/review/amend) consult the spec they already read, else native default. No skill reads a file solely to find grammar; spec-execute never consults the constitution.
- Retrofit = record the local dialect (a `## Grammar` block), non-destructive — body untouched. Opportunistic, never mandated; discovery already reads legacy correctly.
- Bootstrap: a fixed `## Grammar` block adjacent to STATE (and a known constitution/spec section) — the one anchor that can't be dialect-dependent.

**Dissolved.** The earlier conversational OQ about a shared cross-skill grammar file and "third deploy class" — gone: defaults live in each skill (normal deploy-sync), overrides live in the in-repo constitution/spec/artifact (read in place, never deployed).

**Added.** OQ-6 — grammar evolution after codification (snapshot stays fixed for the spec's life; mid-flight change routes through spec-amend). §14 cites finding-* declared pipeline vocabulary as in-house precedent.

**Next pointer.** CP-1 design review. Spec is coherent and self-contained; still uncommitted on disk.
