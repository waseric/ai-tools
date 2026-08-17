# Journal — Context Working-Set Protocol (CWSP)

## Current State
- **Phase:** Adopted — CP-1 and CP-2 both passed; design implemented across the spec-* suite via the downstream feature spec. Terminal.
- **Last completed:** CP-2 (pilot validation) — pass with comments (0 blockers), executed and closed via the downstream feature spec (2026-07-08).
- **Next:** none — design adopted and terminal. P4/Tier-1 sealing deliberately not adopted (CP-2 found scoped reads sufficient); revisit only if a future corpus breaches the §6 targets.
- **Open holds:** OQ-2/OQ-3/OQ-6 remain open but dormant (relevant only if P4 opens); OQ-1 resolved in the feature spec (one-grep STATE cross-check); OQ-4 deferred wholesale to dispatch OQ-3; OQ-5 resolved at CP-1. No blockers.
- **Pending checkpoint:** none — CP-1 and CP-2 both closed.
- **Archive:** none — all entries live
- **Latest entry:** 2026-07-08 — Design adopted (terminal): CP-2 passed downstream, implementation complete

## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec (this design):** sections `## N. <title>`; task blocks `### <T-ID> — <title>` plus the §7 task-table row.

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

## 2026-07-07 — Review of CP-1

**Reviewer:** waseric (self-review, inline)
**Outcome:** pass with comments — **CP-1 closed, design Approved**
**Tasks reviewed:** none (design-approval checkpoint; artifact = architecture.md @ 4bb5f02 + review-response edits)
**Blockers:** 0
**Important:** 1 — §5.4/OQ-5 discoverability invariant fully discharged only for Tier 0; Tier 1/2 completeness relied on OQ-5's (then-open) glob convention.
**Advisory:** 6 — §5.1 "only non-derivable" vs. derivable subset; §5.3 Execute-row NFR asymmetry vs. Review row; §5.5 typo "cephony"; §1/§6 orientation-cost figure (48k vs 45k+); journal missing `## Grammar` bootstrap; journal:46 stale "lint" mention (append-only history, left intact).

**Review focus verdicts.** (1) STATE/INDEX split — correct. (2) Discoverability invariant — discharged for Tier 0; Tier 1/2 gap closed by promoting OQ-5 into normative §5.4/§5.5. (3) Working-set table — no unit omitted that isn't cheaply reachable by widening; Execute row made NFR-symmetric with Review row.

**Remediation (operator-directed, dispatched to a general-purpose worker; verified against diff).** §1/§6 unified to ~47k; §5.1 wording reconciled with OQ-1; §5.3 Execute row gained "+ NFR items the task block cross-references"; §5.4/§5.5 promoted OQ-5's atomic-seal + sibling-glob convention into normative design; §5.5 typo fixed; OQ-5 marked RESOLVED (CP-1); `## Grammar` bootstrap block added to this journal head (dogfooding §5.7). No new issues on re-verification.

**Operator decisions.** Approved shape + vocabulary. OQ-5 → promoted into design (resolved). `## Grammar` block → added now. OQ-1–OQ-4, OQ-6 → deferred with assigned owners.

**Spec amendments proposed:** none outstanding (pre-approval draft edits applied directly; spec was not yet baselined).

**Next action.** Author downstream feature spec `specs/YYYYMMDD-cwsp-skill-integration/feature.md` via `spec-write` with this architecture.md as `DESIGN_SPEC_PATH`, sequencing from P1. CP-2 (Pilot validation) triggers when P2 (`spec-execute` adoption) is implemented.

## 2026-07-08 — Design adopted (terminal): CP-2 passed downstream, implementation complete

**Event.** Moved the design spec to terminal state. Status banner: `Approved — CP-1 passed 2026-07-07` → **Adopted — 2026-07-08**. Added a §9 CP-2 status line recording the pilot outcome.

**Basis.** The design's CP-2 (Pilot validation) — the go/no-go gate for broad adoption and the P4 decision — was executed and closed via the downstream feature spec `../20260707-cwsp-skill-integration/feature.md` (now Complete). CP-2 passed with comments: ~8× measured orientation reduction on this corpus (well inside §6 targets), output parity held, cold-read reconstruction total, 0 blockers. The design is implemented across the spec-* suite (spec-write, spec-design, spec-execute, spec-review, spec-amend + spec-worker/spec-reviewer agent defs).

**P4 decision.** Tier-1 sealing (design §5.5, feature P4/T-11) **not adopted** — CP-2 found read-discipline alone sufficient. The seal machinery in §5.4/§5.5 remains specified but dormant; OQ-3 (seal trigger) stays open against a future P4 reopening only.

**Open questions.** OQ-1 resolved downstream (one-grep STATE cross-check embedded in every reader). OQ-2/OQ-3/OQ-6 dormant; OQ-4 remains deferred to dispatch OQ-3; OQ-5 resolved at CP-1.

**Next action.** None — design adopted and terminal.
