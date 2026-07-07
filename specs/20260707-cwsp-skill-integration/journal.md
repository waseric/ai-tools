# Journal — CWSP Skill Integration

## Current State
- **Phase:** P1 execution
- **Last completed:** T-01 (2026-07-07)
- **Next:** T-02 — spec-design emits STATE + Grammar + reference dialect on journal creation
- **Open holds:** OQ-2/OQ-3/OQ-6 carried from design (deferred to CP-2 / P4 / spec-amend). FQ-1 resolved 2026-07-07.
- **Pending checkpoint:** CP-P1 (vocabulary consistency) — triggers when T-01–T-03 complete; contract in feature.md §9
- **Archive:** none — all entries live
- **Latest entry:** 2026-07-07 — T-01: spec-write emits STATE + Grammar + reference dialect on journal creation

## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec section:** `## N. <title>`; task blocks `### <T-ID> — <title>` plus the §7 table row.

## 2026-07-07 — Feature spec authored

**Event.** Authored the downstream feature spec `feature.md` via `spec-write`, with the [CWSP architecture](../20260707-context-working-set/architecture.md) (Approved, CP-1 passed) as `DESIGN_SPEC_PATH`. Sequences the design's implementation phases P1–P4 into 11 atomic tasks.

**Inputs / discovery.** Read the design spec + its journal STATE block in full (authoritative frame). Located exact integration sites by grep across the five skill masters + two agent-def masters (§4 table). Confirmed the "codebase" is instructional markdown in a meta-repo: masters at `.agents/skills|agents/`, deploy copies under `~/.claude/`, deploy-sync per task. Model-floor ladder `haiku/sonnet/opus/fable` taken from dispatch-execution.

**Clarify decisions (operator-confirmed).**
- **Phase coverage:** P1–P4, with P4 (Tier-1 sealing) gated behind CP-2's outcome.
- **Vocabulary location:** bounded per-skill prose — no shared file / third deploy class (upholds design §5.7). This feature spec's §5 is the single canonical copy source; cross-skill drift is a review finding, not a lint target.
- **OQ-1 (STATE maintenance):** resolved into P1 as the design's leaning — readers cross-check STATE's derivable subset against one grep; a mismatch is an automatic widen signal. Embedded in every reader (spec-execute, spec-review, spec-amend, spec-worker).

**Structure.** P1 (T-01–T-03: writers emit STATE/Grammar/dialect + constitution codification) → CP-P1 → P2 (T-04–T-06: spec-execute retires whole-file reads + worker STATE) → **CP-2 Pilot validation** (the design's CP-2; also the P4 gate) → P3 (T-07–T-10: spec-review/amend adopt vocabulary; amendment collapses to single dialect form) → CP-P3 → P4 (T-11, gated). Reviewer floors: CP-P1 opus, CP-2 fable, CP-P3 opus.

**Scope calls.** Standalone deploy-only dispatch skills (`spec-orchestrate`, `spec-execute-task`, `spec-review-adversarial`) ruled out of scope — no master here, governed by no spec here (§12). The in-scope "dispatch orchestrator" is spec-execute's `EXECUTION: dispatch` mode.

**Verification model.** No test runner; acceptance = grep-checkable prose assertions (phrase present / retired phrase absent) + byte-identical master↔deploy diff per task. Behavioral proof (token reduction + output parity) is CP-2's dogfood on this repo's own journals (§8).

**Next pointer.** CP-P1 gates P1. First implementation task is T-01. Feature spec is on disk (Draft); uncommitted until operator reviews.

## 2026-07-07 — FQ-1 resolved: repo house-style grammar declared

**Event.** Operator resolved FQ-1 in favor of declaring a repo house-style grammar now, to manage go-forward anchor drift across the repo's growing spec/journal corpus (reversing the spec's original "leaning no").

**Changes applied (pre-baseline, spec still Draft — edited directly, not via `spec-amend`).**
- **Constitution:** added a `## Grammar` section to [tech-stack.md](../tech-stack.md) declaring the CWSP reference dialect (journal entry, task closeout, review, amendment single-form, spec section, task block) as repo house style, with the read-as-data / discovery-fallback / no-tooling framing and a pointer to CWSP architecture §5.2/§5.7. Bumped its last-updated to 2026-07-07.
- **feature.md §13:** FQ-1 marked RESOLVED (declared now); recorded the constitution as a live fixture for T-01/T-02.
- **feature.md §3:** added a Background note that the house-style grammar is declared and is the codify-forward source for P1.
- **feature.md §9 CP-P1:** added a review-focus + exit-criterion check that the skill-emitted dialect matches the constitution's declared grammar byte-for-byte.

**Impact on task plan.** No task added or removed. T-01/T-02 (spec-write/spec-design codify constitution grammar forward) now have a real fixture — the repo's own constitution — rather than a hypothetical one. T-03 (project-constitution *capability* to declare a grammar) is unchanged; the manual declaration now does not depend on T-03 landing.

**Note.** This is a documentation/house-style declaration, not skill behavior — no skill yet reads it (P1 wires that). It manages drift immediately for human and agent authors regardless, and is forward-compatible with P1.

**Next pointer.** Unchanged — CP-P1 gates P1; first task is T-01. Committing the spec + constitution edit now.

## 2026-07-07 — T-01: spec-write emits STATE + Grammar + reference dialect on journal creation

**Status.** Done. First P1 task; establishes the canonical STATE/Grammar/reference-dialect copy source that T-02 mirrors and every downstream reader consults.

**Event.** Wired the three P1 vocabulary emissions into `spec-write` (SKILL.md master + deploy copy) per feature.md §7 T-01, copying the shapes verbatim from feature.md §5.1/§5.2.

**Files touched.**
- `.agents/skills/spec-write/SKILL.md` (master): `lastUpdated` 2026-07-03 → 2026-07-07; Phase 1 Discovery grammar codify-forward (free-rider) paragraph added; OUTPUT FORMAT journal.md bullet now points to a new `# JOURNAL CREATION` section carrying the verbatim `## Current State` STATE block, the `## Grammar` bootstrap block, and the reference-dialect table as the native default.
- `~/.claude/skills/spec-write/SKILL.md` (deploy copy): synced byte-identical.

**Tests added (grep assertions / manual checks).**
- Grep (presence): `## Current State`, `## Grammar`, and the `Canonical anchor` reference-dialect table all present in the master.
- Manual/verbatim: `diff` of the emitted STATE / Grammar / dialect blocks against feature.md §5.1/§5.2 copy-ready text — all three empty (verbatim match).

**DoD verification.**
- AC1 (journal opens with §5.1 STATE + §5.2 Grammar): PASS — `# JOURNAL CREATION` section emits both blocks verbatim; `grep -nE '^(## Current State|## Grammar)$'` on the master.
- AC2 (constitution-declared grammar codified forward into the spec): PASS — Phase 1 Discovery free-rider paragraph; `grep -n 'codify it forward' .agents/skills/spec-write/SKILL.md`.
- AC3 (reference-dialect table present): PASS — `grep -n 'Canonical anchor' .agents/skills/spec-write/SKILL.md`.
- Global: deploy-sync empty diff; `lastUpdated` 2026-07-07; frontmatter still only name/lastUpdated/description; commit prefix `spec-write:` referencing T-01; STATE overwritten in this same commit.

**Models used.** opus (floor: opus — met exactly).

**Executed by:** worker(spec-worker, opus).

**Decisions made.** Placed the emission contract in a dedicated `# JOURNAL CREATION` section (rather than inline in the OUTPUT FORMAT bullet) so the verbatim blocks read as copy templates in fenced `markdown` code blocks; the OUTPUT FORMAT bullet now cross-references it. Grammar codify-forward lives in Phase 1 Discovery adjacent to the existing constitution read, making the free-rider rule (no read added solely for grammar) explicit.

**Spec amendments.** None. Spec §5 copy source and design were consistent; no trigger.

**Surprises and learnings.** None. STATE cross-check (OQ-1) passed on orientation — STATE's Latest-entry anchor matched the true latest entry grep.

**Next task pointer.** T-02 — spec-design emits STATE + Grammar + reference dialect on journal creation (mirror of this task, adapted to `architecture.md` authoring; STATE/Grammar blocks must be byte-identical to T-01's).
