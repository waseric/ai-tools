# Journal — CWSP Skill Integration

## Current State
- **Phase:** CP-2 (Pilot validation) PASSED with comments 2026-07-08 — P3 open.
- **Last completed:** Out-of-band re-sync of T-01/T-02 SKILL.md masters' reference-dialect table to Amendment 2026-07-08-1's text (mechanical copy-fix, not a §7 task).
- **Next:** T-07 — spec-review adopts STATE/INDEX vocabulary + cold-reader guarantee + STATE write-back.
- **Open holds:** OQ-2/OQ-3/OQ-6 carried from design. FQ-1 resolved 2026-07-07. CP-P1 CLOSED 2026-07-08. P4/T-11 NOT adopted (CP-2 decided scoped reads sufficient).
- **Pending checkpoint:** CP-P3 — cross-skill consistency (triggers after T-07–T-10).
- **Archive:** none — all entries live
- **Latest entry:** 2026-07-08 — Re-sync T-01/T-02 masters to Amendment 2026-07-08-1

## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.

## 2026-07-08 — Re-sync T-01/T-02 masters to Amendment 2026-07-08-1

**Event.** Out-of-band mechanical copy-fix flagged by Amendment 2026-07-08-1's Phase 6 handoff: T-01 (spec-write) and T-02 (spec-design) SKILL.md masters still embedded the pre-amendment reference-dialect table text (`### <T-ID> — <title>` / table-always-present), diverging from feature.md §5.2's amended canonical text. Not a §7 task — a narrow re-sync to keep the CP-P1 byte-identity invariant alive ahead of CP-P3.

**Change.** Two lines per file, four files: `.agents/skills/spec-write/SKILL.md`, `~/.claude/skills/spec-write/SKILL.md`, `.agents/skills/spec-design/SKILL.md`, `~/.claude/skills/spec-design/SKILL.md`. Replaced the Grammar bullet and reference-dialect table row with the amended text (`#{3,4} <T-ID> — <title>`, h3 or h4, §7 table row "where one exists" / "where a table exists"). No other text touched.

**Verification.** `diff` master vs. deploy — empty for both skills. `grep -n '### <T-ID>' .agents/skills/spec-write/SKILL.md .agents/skills/spec-design/SKILL.md` — no matches (old form fully retired). New text confirmed byte-identical to feature.md:131,142-143 and tech-stack.md:64.

**Next task pointer.** T-07 — spec-review adopts STATE/INDEX vocabulary + cold-reader guarantee + STATE write-back.

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

## 2026-07-07 — T-02: spec-design emits STATE + Grammar + reference dialect on journal creation

**Status.** Done. Mirrors T-01 into `spec-design`'s journal creation, adapted to `architecture.md` authoring.

**Event.** Wired the three P1 vocabulary emissions into `spec-design` (SKILL.md master + deploy copy) per feature.md §7 T-02, copying the STATE/Grammar/dialect shapes verbatim from feature.md §5.1/§5.2 (byte-identical to T-01's spec-write emission).

**Files touched.**
- `.agents/skills/spec-design/SKILL.md` (master): `lastUpdated` 2026-05-18 → 2026-07-07; Phase 1 Discovery grammar codify-forward (free-rider) paragraph added, adapted to spec-design's landscape/constraint-orientation Discovery (no `CONSTITUTION_PATHS` input parameter exists here, so the paragraph hooks off the constitution read implied by Constraint orientation / HANDOFF NOTES §upstream); OUTPUT FORMAT journal.md bullet now points to a new `# JOURNAL CREATION` section carrying the verbatim `## Current State` STATE block, the `## Grammar` bootstrap block, and the reference-dialect table as the native default.
- `~/.claude/skills/spec-design/SKILL.md` (deploy copy): synced byte-identical.

**Tests added (grep assertions / manual checks).**
- Grep (presence): `grep -n "## Current State\|## Grammar$\|Canonical anchor" .agents/skills/spec-design/SKILL.md` — all three present.
- Grep (free-rider paragraph): `grep -n "codify it forward" .agents/skills/spec-design/SKILL.md` — present.
- Byte-identical cross-check vs T-01 (consistency NFR §6): `diff <(sed -n '/^## Current State$/,/^## Grammar$/p' .agents/skills/spec-write/SKILL.md) <(sed -n '/^## Current State$/,/^## Grammar$/p' .agents/skills/spec-design/SKILL.md)` — empty. `diff <(sed -n '/^| Element | Canonical anchor |$/,/^| Task block/p' .agents/skills/spec-write/SKILL.md) <(sed -n '/^| Element | Canonical anchor |$/,/^| Task block/p' .agents/skills/spec-design/SKILL.md)` — empty.

**DoD verification.**
- AC1 (journal opens with §5.1 STATE + §5.2 Grammar, byte-identical to T-01): PASS — `# JOURNAL CREATION` section emits both blocks verbatim; both cross-check diffs above return empty.
- AC2 (Discovery constitution-grammar codify-forward, adapted to spec-design's landscape orientation): PASS — `grep -n "codify it forward" .agents/skills/spec-design/SKILL.md`.
- AC3 (reference-dialect table present, byte-identical to T-01's): PASS — `grep -n "Canonical anchor" .agents/skills/spec-design/SKILL.md`; empty diff shown above.
- Global: deploy-sync `diff .agents/skills/spec-design/SKILL.md ~/.claude/skills/spec-design/SKILL.md` — empty; `lastUpdated` 2026-07-07; frontmatter still only `name`/`lastUpdated`/`description`; commit prefix `spec-design:` referencing T-02; STATE overwritten in this same commit.

**Models used.** sonnet (floor: sonnet — met exactly).

**Executed by:** worker(spec-worker, sonnet).

**Decisions made.** Placed the emission contract in a dedicated `# JOURNAL CREATION` section mirroring T-01's placement exactly (same heading, same block order) so the two skills read identically at the anchor a cold reader consults first. Because spec-design has no `CONSTITUTION_PATHS` input parameter (unlike spec-write), the free-rider paragraph is worded to hook off the existing Constraint-orientation Discovery step and the HANDOFF NOTES' constitution-citation convention, rather than inventing a new input.

**Spec amendments.** None. §5 copy source and T-01's precedent were consistent and reconcilable; no trigger.

**Surprises and learnings.** spec-design lacks an explicit `CONSTITUTION_PATHS`-style input that spec-write has — the constitution read is implicit (Constraint orientation + HANDOFF NOTES upstream note). Adapted wording accordingly rather than adding a new INPUTS field, since T-02's scope is limited to the journal-creation instruction and Discovery paragraph, not INPUTS restructuring. Flagging for awareness only, not as an amendment trigger — the free-rider rule still holds (no extra read added).

**Next task pointer.** T-03 — project-constitution may declare repo house-style grammar.

## 2026-07-07 — T-03: project-constitution may declare repo house-style grammar

**Status.** Done.

**Commits.** (this task's commit — see receipt).

**Files touched.**
- `.agents/skills/project-constitution/SKILL.md` (master): `lastUpdated` 2026-05-18 → 2026-07-07; added a "Grammar (optional)" bullet to the `tech-stack.md` "Conventions Outside the Stack" template, adjacent to the existing "Repository layout" bullet, documenting that a constitution *may* declare a `## Grammar` block as repo house style, that it is never mandated (native-default fallback otherwise), and that `spec-write`/`spec-design` read and codify it forward during Discovery as a free rider on the constitution read they already perform.
- `~/.claude/skills/project-constitution/SKILL.md` (deploy copy): synced byte-identical.

**Tests added (grep checks).**
- `grep -n '## Grammar' .agents/skills/project-constitution/SKILL.md` — present.
- `grep -n 'house-style' .agents/skills/project-constitution/SKILL.md` — present.
- `grep -n 'spec-write.*spec-design\|spec-design.*spec-write' .agents/skills/project-constitution/SKILL.md` — present (writer codification reference).

**DoD verification.**
- AC (prose documents optional `## Grammar` declaration + downstream free-rider consumption): PASS — new bullet at `.agents/skills/project-constitution/SKILL.md:132`; `grep -n 'Grammar (optional)' .agents/skills/project-constitution/SKILL.md`.
- No contradiction with the already-declared `tech-stack.md` "## Grammar" fixture: PASS — new prose describes the *capability* generically (never mandates, native-default fallback), consistent with the fixture's own "Declared as data, never enforced by tooling" framing; manual read of both side by side.
- Global: deploy-sync empty diff — `diff .agents/skills/project-constitution/SKILL.md ~/.claude/skills/project-constitution/SKILL.md` (empty). `lastUpdated` bumped to 2026-07-07, no other frontmatter keys added — `sed -n '1,5p' .agents/skills/project-constitution/SKILL.md`. Commit prefix `project-constitution:` referencing T-03. STATE overwritten in this same commit.

**Models used.** sonnet (floor: sonnet — met exactly).

**Executed by:** worker(spec-worker, sonnet).

**Decisions made.** Placed the new bullet inside the existing `tech-stack.md` "Conventions Outside the Stack" template (adjacent to "Repository layout"), matching the task's declared scope location (`:131`) rather than adding a new top-level SKILL.md section — the grammar declaration is itself a tech-stack convention, consistent with where the fixture actually lives in this repo's own `tech-stack.md`.

**Spec amendments.** None. Scope, acceptance criteria, and the already-declared fixture were all consistent; no trigger.

**Surprises and learnings.** None of note. Read location matched the brief's `:131` pointer once "conventions/authoritative-artifacts section" was resolved to the "Conventions Outside the Stack" template block (project-constitution's own section headings don't use that literal phrase; the Phase 2/3 prose does, at `:70` and `:79`).

**Next task pointer.** CP-P1 — vocabulary consistency review (feature.md §9). Triggered: T-01–T-03 complete.

## 2026-07-07 — Review of CP-P1

**Reviewer:** waseric (opus, inline)
**Outcome:** changes requested
**Tasks reviewed:** T-01, T-02, T-03
**Diff range:** c6fa82c^..6baea95
**Blockers:** 1 — skill-emitted reference dialect ≠ constitution's declared grammar. Constitution [tech-stack.md](../tech-stack.md#L64) Task-block row reads `**and** the §7 **task-table** row`; feature.md §5.2 and both skills (spec-write:235, spec-design) read `**and** the §7 **table** row`. Fails the CP-P1 exit criterion "skill dialect matches the constitution grammar" (feature.md §3 requires the two identical). All other CP-P1 focus items pass.
**Important:** 1 — `specs/tech-stack.md` is entirely CRLF (66/66 lines) while skill masters + specs are LF, so a literal byte-for-byte identity diff of the constitution grammar against the skills can never be empty. Pre-existing (not introduced by T-01–T-03); does not affect grep-derived INDEX function. Normalize to LF alongside the blocker fix.
**Advisory:** 0
**Spec amendments proposed:** none required if the constitution is reconciled to §5.2 (the canonical vocabulary source, feature.md §5). If instead §5.2 is changed to "task-table row" (more precise wording), that routes through `spec-amend` on feature.md §5.2 and re-emission of T-01/T-02.
**Passed checks:** writer↔writer STATE/Grammar/dialect diffs empty; STATE block + Grammar bootstrap faithful to §5.1/§5.2; free-rider rule present in both writers (no grammar-only constitution read); OQ-1 reader cross-check correctly NOT baked into writers (reserved for P2 readers); no whole-file-read regression; deploy-sync byte-identical for all three masters; T-03 grep assertions pass.
**Next action:** Operator reconciles the one-word dialect drift (recommend editing `specs/tech-stack.md:64` "§7 task-table row" → "§7 table row" and normalizing that file to LF — cheapest path, keeps §5.2 canonical and avoids spec-amend), then re-invoke `/spec-review` against CP-P1. No skill re-edit required under the recommended path.

## 2026-07-08 — Review of CP-P1 (re-review)

**Reviewer:** waseric (opus, inline)
**Outcome:** pass
**Tasks reviewed:** T-01, T-02, T-03
**Diff range:** c6fa82c^..6baea95 (skill masters, unchanged since verdict 777c308) + working-tree `specs/tech-stack.md` (the reconciliation fix)
**Blockers:** 0 — the sole prior blocker is resolved. Constitution [tech-stack.md:64](../tech-stack.md#L64) Task-block anchor now reads "`### <T-ID> — <title>` **and** the §7 table row" — byte-identical to feature.md §5.2 and both skills' emitted dialect tables (`diff` empty, both directions).
**Important:** 0 — the prior CRLF finding is resolved. `specs/tech-stack.md` normalized to LF (0 CR bytes across 66 lines); working-tree diff is a clean 66-line CRLF→LF re-write with the one intended content edit at line 64.
**Advisory:** 0.
**Spec amendments proposed:** none. Recommended path (reconcile constitution to §5.2) was taken; §5.2 stays canonical, no `spec-amend` needed.
**Exit criteria (all met):**
- Cross-skill STATE/dialect diff empty — STATE block and dialect table byte-identical spec-write↔spec-design; Grammar-bootstrap intro's sole variance ("the spec" vs "the design spec") is the intended per-skill contextual difference, not a dialect drift.
- Skill dialect matches the constitution grammar — constitution↔skill dialect table `diff` empty.
- Grep assertions T-01–T-03 pass — unchanged since verdict; masters untouched (`git diff 777c308..HEAD` empty).
- No whole-file-read regression — the only "in full" instruction is spec-write reading its upstream `DESIGN_SPEC_PATH` (an input), not a CWSP scoped-read reader regression (that surface is P2).
- Deploy-sync byte-identical — spec-write / spec-design / project-constitution masters `diff` clean against `~/.claude/` copies.
**Reviewer floor:** opus — met (reviewer ran on opus).
**Next action:** Commit the working-tree `specs/tech-stack.md` fix as the paired spec-side commit alongside this verdict (spec + journal + constitution in one). CP-P1 is CLOSED. Resume `spec-execute` at P2 / T-04.

## 2026-07-08 — T-04: spec-execute retires whole-file reads for STATE + working set

**Status.** done. First P2 task. Converts all of `spec-execute`'s read-discipline prose from whole-file spec reads to STATE-first + Execute-task working set + INDEX widening + grammar consult, as one coherent unit-edit spanning the coupled sites.

**Commits.** This task's closeout commit (single-repo; skill master + deploy copy + spec/journal in one commit — SHA in the receipt).

**Files touched.**
- `.agents/skills/spec-execute/SKILL.md` (master): `lastUpdated` 2026-07-06 → 2026-07-08; six coupled edits — (1) Operating Principle 6 re-anchor redefined to STATE + next-task working set via INDEX, not "re-read the relevant section"; (2) Phase 1 ORIENT read list restructured to STATE-consult-first with the OQ-1 one-grep cross-check (§5.1), then the Execute-task working set + grammar consult + INDEX widening (§5.2/§5.3/§5.4), retiring "`SPEC_PATH` in full"; (3) Phase 8 re-anchoring-cost bullet ("re-reads the spec from scratch" → scoped STATE + working-set read); (4) Phase 8 continue/pause prose ("re-reading the spec" → STATE + next task block via INDEX; handoff = the `## Current State` block); (5) WHAT NOT TO DO boundary bullet ("Re-read it at every task boundary" → STATE + next task block via INDEX, never a whole-spec reload); (6) DISPATCH MODE orchestrator conduct item 1 aligned to the §5.3 "Dispatch orchestrator" row (STATE + §7 task-table row + receipt(s); not task-block internals/code/journal history).
- `~/.claude/skills/spec-execute/SKILL.md` (deploy copy): synced byte-identical.
- `specs/20260707-cwsp-skill-integration/feature.md`: §7 T-04 marked done (2026-07-08).

**Tests added (grep assertions / manual checks).**
- Absence: `grep -c 'SPEC_PATH\` in full' .agents/skills/spec-execute/SKILL.md` → 0.
- Absence (whole-file phrasings): `grep -nE 're-read the spec from scratch|re-reads the spec from scratch|re-read it at every task boundary' .agents/skills/spec-execute/SKILL.md` → none.
- Presence (Phase 1 + boundary prose): `grep -cE 'Current State|working set|widen' .agents/skills/spec-execute/SKILL.md` present in Phase 1 read list and Operating Principle 6.
- Manual: read Phase 1 + Principle 6 + Phase 8 + DISPATCH MODE together — no whole-file-read instruction survives; Phase 1 (scoped) and boundary prose (scoped) no longer contradict.

**DoD verification.**
- AC1 (Phase 1 directs STATE + working-set + widen-via-INDEX; "`SPEC_PATH` in full" absent): PASS — Phase 1 items 1–2; `grep -c 'SPEC_PATH\` in full' .agents/skills/spec-execute/SKILL.md` → 0.
- AC2 (task-boundary re-anchor = STATE + next-task-block, not whole-spec re-read): PASS — Principle 6 + Phase 8 + WHAT NOT TO DO; whole-file-phrasing absence grep above → none.
- AC3 (dispatch orchestrator per-task read matches §5.3 orchestrator row): PASS — DISPATCH MODE orchestrator conduct item 1 now reads "STATE + the §7 task-table row + the receipt(s)".
- Global: deploy-sync `diff .agents/skills/spec-execute/SKILL.md ~/.claude/skills/spec-execute/SKILL.md` — empty; `lastUpdated` 2026-07-08; frontmatter still only `name`/`lastUpdated`/`description`; commit prefix `spec-execute:` referencing T-04; STATE overwritten in this same commit.

**Models used.** opus (floor: opus — met).

**Executed by:** worker(spec-worker, opus).

**Decisions made.** Restructured the Phase 1 numbered read list so STATE (journal) is item 1 and the spec working set is item 2 — reversing the prior spec-first order — because STATE is now the cheapest orient anchor and the working-set read for the spec depends on which task STATE names as Next. Kept branch-state and multi-repo-detection as items 3/4 unchanged. Folded the OQ-1 one-grep cross-check verbatim into Phase 1 item 1 rather than a separate principle, matching §5.1's "reader contract" framing.

**Spec amendments.** None. §5.1–5.4 copy source and the design were consistent with the task's scope; no trigger. STATE cross-check passed on orientation (Latest-entry anchor matched the true latest journal entry grep).

**Surprises and learnings.** Prior P1 tasks (T-01–T-03) recorded done-status only in the journal (STATE + entries), not in §7 task blocks; T-04's brief explicitly required a §7 done-mark, so a `- **Status.** done` line was added to the T-04 block — a small convention divergence from earlier tasks, flagged for awareness (not an amendment trigger).

**Next task pointer.** T-05 — spec-execute Phase 6 closeout updates STATE in the same commit as the appended journal entry.

## 2026-07-08 — T-05: spec-execute Phase 6 closeout updates STATE

**Status.** done. Additive write-side rule pairing with T-04's read-side conversion — Phase 6 closeout now overwrites STATE in place in the same commit as the appended journal entry.

**Commits.** This task's closeout commit (single-repo; skill master + deploy copy + spec/journal in one commit — SHA in the receipt).

**Files touched.**
- `.agents/skills/spec-execute/SKILL.md` (master): Phase 6 — added a new bullet ("Overwrite the journal's `## Current State` block") between "Update the spec" and "Append a journal entry," embedding §5.1's Writer contract (overwrite in place at every closeout — task, checkpoint, amendment; STATE is the only in-place-mutated part of the journal, entries below stay append-only; STATE overwrite and the appended entry land in the same commit). `lastUpdated` left at 2026-07-08 (already current from T-04).
- `~/.claude/skills/spec-execute/SKILL.md` (deploy copy): synced byte-identical.
- `specs/20260707-cwsp-skill-integration/feature.md`: §7 T-05 marked done (2026-07-08).

**Tests added (grep assertions).**
- Presence: `grep -c 'Current State' .agents/skills/spec-execute/SKILL.md` → 4 (Principle 6, Phase 1, new Phase 6 bullet, Phase 8 pause prose).
- Presence: `grep -c 'same commit' .agents/skills/spec-execute/SKILL.md` → 1 (new Phase 6 bullet).
- Manual: read the new Phase 6 bullet against §5.1's Writer contract — verbatim-equivalent (overwritten in place at every closeout; only in-place-mutated part; entries below append-only; same commit as the appended entry).

**DoD verification.**
- AC (Phase 6 requires a STATE overwrite in the same commit as the journal entry; grep "Current State" + "same commit" present in Phase 6 prose): PASS — new bullet at Phase 6 (line ~130); `grep -nE 'Current State|same commit' .agents/skills/spec-execute/SKILL.md` shows both in the Phase 6 section.
- Global: deploy-sync `diff .agents/skills/spec-execute/SKILL.md ~/.claude/skills/spec-execute/SKILL.md` — empty; `lastUpdated` 2026-07-08; frontmatter still only `name`/`lastUpdated`/`description`; commit prefix `spec-execute:` referencing T-05; STATE overwritten in this same commit (dogfooded).

**Models used.** sonnet (floor: sonnet — met).

**Executed by:** worker(spec-worker, sonnet).

**Decisions made.** Placed the new STATE-overwrite bullet immediately before "Append a journal entry" (rather than after) so the closeout checklist reads in the order the rule implies — STATE gets overwritten as part of preparing the same commit that appends the entry — and integrated T-04's language ("same commit," "one commit, not two") rather than introducing new phrasing, to avoid contradicting T-04's edit.

**Spec amendments.** None. §5.1's Writer contract mapped cleanly onto Phase 6 with no drift found.

**Surprises and learnings.** None beyond T-04's flagged §7 `**Status.**` convention, which this task continued.

**Next task pointer.** T-06 — spec-worker consults + updates STATE.

## 2026-07-08 — T-06: spec-worker consults + updates STATE

**Status.** done. Agent-definition edit mirroring the reader/writer contracts T-04/T-05 added to spec-execute: the worker's Orient now consults STATE first with the OQ-1 one-grep cross-check, and Phase 6 close-out overwrites STATE in the same commit as the appended journal entry.

**Commits.** This task's closeout commit (single-repo; agent-def master + deploy copy + spec/journal in one commit — SHA in the receipt).

**Files touched.**
- `.agents/agents/spec-worker.md` (master): (a) Orient — prepended a new item 1 (`## Current State` consult first + derivable-subset cross-check via the one grep `grep -nE '^## [0-9]{4}-[0-9]{2}-[0-9]{2} — ' journal.md | tail -1`, mismatch → range-read true latest entry; explicitly "a scoped consult, never a whole-file read"), renumbering the existing task/section/latest-entry/CLAUDE.md reads (2–5, unchanged). (b) Phase 6 — added a new "Overwrite the journal's `## Current State` block" bullet before "Append a journal entry" (§5.1 Writer contract: overwrite in place, only in-place-mutated part, entries append-only, same commit — one commit not two), and folded "overwritten STATE, appended journal entry" into the paired-commit bullet. No `lastUpdated` field exists in this agent-def frontmatter (name/description/disallowedTools only) — per the harness-adapter rule, none added.
- `~/.claude/agents/spec-worker.md` (deploy copy): synced byte-identical.
- `specs/20260707-cwsp-skill-integration/feature.md`: §7 T-06 marked done (2026-07-08).

**Tests added (grep assertions).**
- Presence (Orient + Phase 6): `grep -c 'Current State' .agents/agents/spec-worker.md` → 2 (Orient item 1; Phase 6 bullet).
- Cross-check phrasing present: `grep -c "grep -nE '\^## \[0-9\]{4}" .agents/agents/spec-worker.md` → 1 (Orient item 1).
- Phase 6 same-commit rule: `grep -c 'same commit' .agents/agents/spec-worker.md` → 1.
- No whole-file-read instruction in Orient: the only "whole-file" match is the negation "never a whole-file read"; the numbered reads remain scoped (STATE, task text, referenced sections, latest entry, CLAUDE.md).

**DoD verification.**
- AC (worker orients → reads STATE + cross-check + scoped task/latest-entry, no whole-file read): PASS — Orient item 1 at line ~29; existing scoped reads unchanged (renumbered 2–5).
- AC (worker closes out → overwrites STATE in its journal commit): PASS — Phase 6 STATE-overwrite bullet at line ~59; paired-commit bullet updated.
- Grep ("Current State" in Orient + journal-commit prose; cross-check grep phrasing present): PASS — see grep counts above.
- Global: deploy-sync `diff .agents/agents/spec-worker.md ~/.claude/agents/spec-worker.md` — empty; frontmatter unchanged (no `lastUpdated` to bump — agent-def harness adapter, no new key added); commit prefix `spec-worker:` referencing T-06; STATE overwritten in this same commit (dogfooded).

**Models used.** opus (floor: opus — met).

**Executed by:** worker(spec-worker, opus).

**Decisions made.** Mirrored spec-execute's exact vocabulary (T-04 Phase 1 item 1 cross-check language; T-05 Phase 6 Writer-contract language "same commit / one commit, not two") rather than introducing new phrasing, so worker and orchestrator read consistently. Added the STATE consult as a new item 1 (STATE-first) and renumbered rather than appending, matching spec-execute Phase 1's read order. `lastUpdated` case: field absent from agent-def frontmatter — not added, per the repo rule that agent defs are declared harness adapters and must not gain new frontmatter keys.

**Spec amendments.** None. §5.1's reader + writer contracts mapped cleanly onto the worker's Orient and Phase 6.

**Surprises and learnings.** None. spec-worker.md carries no `lastUpdated` (unlike skill masters), so the global "bump lastUpdated" DoD addendum is N/A for agent-def tasks — worth noting for the sibling T-08 (spec-reviewer).

**Next task pointer.** CP-2 — Pilot validation (P2 complete; reviewer floor fable). P3 (T-07) is gated behind CP-2.

## 2026-07-08 — Review of CP-2

**Reviewer:** fable (dispatched spec-reviewer; floor fable — met)
**Outcome:** pass with comments
**Tasks reviewed:** T-04, T-05, T-06
**Diff range:** e5c2d55^..HEAD
**Execution:** dispatch — opus coordinator session spawned a `spec-reviewer` at model=fable (checkpoint floor could not be met inline by the opus session; operator chose dispatch). Reviewer ran Phases 1–7 and returned the verdict; this session (coordinator) performed Phase 8 write-back unchanged, recording the reviewer's verdict first-hand.
**Blockers:** 0.
**Important:** 1 — declared task-anchor grammar misses this spec's own task blocks. journal.md:15 + feature.md:131/150 declare `### <T-ID> — <title>` and assert "the §7 task table is canonical (always present)," but feature.md has **no** §7 table and its task blocks are `#### T-NN` (h4, e.g. feature.md:257); both the declared-dialect grep and the §5.2 discovery fallback (`^### T-[0-9]`) return zero on the very spec under test. Correctness-safe (a miss forces a cheap widen via broad task-ID grep, never wrong output — the §6 correctness-safety invariant held), but it is live anchor drift in the exact place the grammar was declared to prevent it. Pre-existing at spec authoring (2026-07-07), not introduced by T-04–T-06.
**Advisory:** 2 — (1) spec-execute Phase 1 item 2 (SKILL.md:58) and Principle 6 (SKILL.md:49) lead with "§7 task-table row" and handle the table-only case parenthetically but not the heading-only/no-table case this repo's specs exhibit; readers recover trivially. (2) Inline spec-execute Phase 1 no longer reads the latest journal entry when STATE matches, whereas spec-worker Orient deliberately keeps it (spec-worker.md:32); no correctness gap (Phase 2 prior-DoD verify forces the widen; Phase 6 carries the entry template) but the asymmetry is worth knowing.

**Measured results (the CP-2 substance).**
- Baseline orientation (whole-file, the retired discipline): ~19.1k tokens on this corpus (feature.md 44,987 + journal.md 31,362 chars; method chars/4). The design's ~47k figure is its larger measured example (spec ~21k + journal ~26k tokens, architecture.md:10) — order-consistent once ~2× harness read overhead is included; not contradicted.
- Scoped orientation (CWSP working set for the T-07 boundary): ~0.7k tokens core (STATE+Grammar + OQ-1 grep + T-07 block + CP-P3 contract) / ~2.4k including the expected widen to the §5.1–§5.4 vocabulary copy-source that T-07 embeds verbatim (an explicit cross-ref widen, not an under-read). vs. < ~5k target — **met**, ~8× reduction vs. baseline (28× core-only).
- Re-anchor: ~0.17k tokens (STATE + the one cross-check grep) vs. < ~1k target — **met** (~6× margin).
- Output parity / under-read: T-05 and T-06 were themselves executed by workers orienting under the migrated scoped discipline (STATE cross-check passed at each); all declared DoD greps re-run clean; the one under-read-shaped path (task-anchor miss, above) degrades to a cheap widen, never wrong output.
- Cold-read reconstruction: **succeeded** — from STATE + the one-grep INDEX (10 entries enumerated) reconstructed current phase/next-task and reached two historical decisions by range-read (CP-P1 verdict journal.md:149-179; FQ-1 resolution journal.md:36-50); `Archive: none` keeps the Tier-0 path guaranteed-total.

**Verification cross-checks (passed).** "`SPEC_PATH` in full" count 0; no whole-file re-read phrasing survives at Phase 1 / Principle 6 / Phase 8 / WHAT NOT TO DO / DISPATCH sites (read together — no internal contradiction, the §10 top risk did not materialize); deploy-sync byte-identical for spec-execute and spec-worker masters vs. `~/.claude/` copies; STATE derivable fields match the true latest-entry grep; frontmatter clean; commit prefixes conform.

**Exit criteria — all met.** Orientation cost meets §6 target (measured above); zero quality regression on the dogfood task (T-04–T-06 executed under scoped discipline, greps + deploy-sync + STATE accuracy all clean); cold-read reconstruction succeeds (above).

**P4 gate decision.** **Do not adopt P4 (T-11).** The exit criterion admits sealing only if scoped reads alone prove insufficient; they proved sufficient with 2–6× margin and low widen frequency (one expected cross-ref widen per vocabulary-copy task, zero unexpected widens across three boundaries), and the journal is far from size pressure (~31k chars, all-live). T-11 stays gated shut; revisit only if a future corpus breaches the targets under read-discipline alone.

**Spec amendments proposed:** one — feature.md §5.2 (and the coupled emitted Grammar bootstrap + constitution dialect, per CP-P1): broaden the task-block discovery pattern to tolerate heading depth (`^#{3,4} T-[0-9]` or equivalent) or state that the §7 table is optional and heading-blocks may sit at h3/h4, and reconcile this spec's own `####`/absent-table shape with whatever is declared. Non-blocking for CP-2; route through `spec-amend` before or alongside P3 since it touches the same vocabulary CP-P3 re-checks. Not applied here — reviews propose amendments; `spec-amend` applies them.

**Next action:** CP-2 is CLOSED (pass with comments); P4 remains gated shut. Resume `spec-execute` at P3 / T-07. Recommended: land the §5.2 task-anchor `spec-amend` before or alongside T-07 so CP-P3's byte-identity checks stay meaningful.

## 2026-07-08 — Amendment 2026-07-08-1

**Section amended:** feature.md §5.2 (Grammar bootstrap block + reference dialect + INDEX contracts), plus coupled §5.3 (Execute-task working-set row), `specs/tech-stack.md:64` (constitution grammar), and this journal's own `## Grammar` block.
**Trigger:** CP-2 review (2026-07-08, fable) — declared task-anchor grammar (`### <T-ID>` + "§7 table is canonical, always present") missed this spec's own `#### T-NN` / no-§7-table task blocks; both the declared-dialect grep and the discovery fallback returned zero on this spec.
**Reason:** Broaden the grammar to tolerate heading depth (h3 or h4) and make the §7 table an optional secondary index rather than an assumed-present primary one, so the common case hits on the first grep instead of always falling through to widen.
**Impact summary:** No completed task's output is invalidated; T-01/T-02's SKILL.md masters embed the now-superseded table text and need a follow-up re-sync task before CP-P3 closes (tracked in STATE's Open holds). CP-P3's exit criteria should read against the amended §5.2.
**Approver:** waseric
**Approved on:** 2026-07-08
**Status implication:** kept (Draft — awaiting review; non-blocking clarification, does not advance or revert status)
**Commit:** 3578fbe

### Full record

**Trigger.** CP-2 review (2026-07-08, fable, dispatched spec-reviewer) found that the declared task-anchor grammar (`### <T-ID>` heading + "the §7 task table is canonical, always present") does not match this spec's own task blocks, which are `#### T-NN` (h4) with no §7 table. Both the declared-dialect grep and the discovery fallback return zero hits on this spec. Non-blocking (correctness-safe — the miss degrades to a cheap widen), but flagged as live anchor drift that should be reconciled before CP-P3 re-checks cross-skill dialect consistency.

**Section.** `feature.md` §5.2 (lines 123–153), plus the coupled §5.3 working-set row, the coupled constitution grammar (`specs/tech-stack.md:64`), and this spec's own already-emitted journal Grammar block.

**Change.**

Before (feature.md §5.2, bootstrap bullet):
> `- **Spec section:** `## N. <title>`; task blocks `### <T-ID> — <title>` plus the §7 table row.`

After:
> `- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.`

Before (feature.md §5.2, reference-dialect table, Task block row):
> `| Task block | `### <T-ID> — <title>` **and** the §7 table row |`

After:
> `| Task block | `#{3,4} <T-ID> — <title>` (h3 or h4) **and** the §7 table row, where a table exists |`

Before (feature.md §5.2, discovery-fallback bullet):
> `- Spec tasks: **the §7 task table is canonical** (always present); where per-task headings exist, `grep -nE '^### T-[0-9]' <spec>` locates them for range-reading`

After:
> `- Spec tasks: heading-block discovery is primary — `grep -nE '^#{3,4} T-[0-9]' <spec>` locates per-task headings at either depth; the §7 table row, where one exists, is a secondary index, not assumed canonical`

Before (feature.md §5.3, Execute-task working-set row):
> `| Execute task T (inline or worker) | STATE + §7 task-table row + the `### T` block *if the spec uses per-task headings* (table-only specs: the row is the whole unit) + pending checkpoint contract if any + NFR items the task block cross-references | §1–6, other task blocks, journal history |`

After:
> `| Execute task T (inline or worker) | STATE + §7 task-table row (if present) + the `#{3,4} T` heading block *if the spec uses per-task headings* (table-only specs: the row is the whole unit; heading-only specs with no §7 table: the heading block is the whole unit) + pending checkpoint contract if any + NFR items the task block cross-references | §1–6, other task blocks, journal history |`

Before (`specs/tech-stack.md:64`):
> `| Task block | `### <T-ID> — <title>` **and** the §7 table row |`

After (kept byte-identical to feature.md §5.2's new row, per CP-P1's binding exit criterion):
> `| Task block | `#{3,4} <T-ID> — <title>` (h3 or h4) **and** the §7 table row, where a table exists |`

Before (this journal's own emitted `## Grammar` bootstrap):
> `- **Spec section:** `## N. <title>`; task blocks `### <T-ID> — <title>` plus the §7 table row.`

After (matching feature.md's new bootstrap bullet — dogfood correction):
> `- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.`

**Reason.** The original grammar assumed every spec's task blocks are h3-with-mandatory-table. This spec itself is h4-with-no-table, so the declared dialect and its own discovery fallback both silently fail to grep this spec's tasks. Broadening the pattern to tolerate heading depth and making the table optional closes the gap without weakening correctness (widening was already the safety net; this just makes the common case hit on the first grep instead of falling through to widen every time).

**Impact.**
- **Affected tasks:** None re-opened. T-04–T-06 (done) already executed correctly under the old dialect via widen-fallback, so no rework. **Follow-up task needed** (new, to be scoped by `spec-execute`): re-sync the reference-dialect table and bootstrap bullet embedded verbatim in the already-done T-01 (`spec-write`) and T-02 (`spec-design`) SKILL.md masters (+ deploy copies), so future spec-write/spec-design journal creation emits the corrected grammar. Recommend landing this before CP-P3, since CP-P3 re-checks cross-skill dialect byte-identity.
- **Affected checkpoints:** CP-P3 (not yet triggered) — its exit criteria should be read against the amended §5.2, not the original.
- **Completed work invalidated:** No completed task's output is wrong; the completed T-01/T-02 masters now embed a stale copy of the canonical text and need the follow-up re-sync above before CP-P3 closes.
- **Cross-references requiring follow-up:** `specs/tech-stack.md:64` (constitution grammar, edited in this amendment per the CP-P1 byte-identity binding) and this journal's own `## Grammar` block (edited in this amendment as a dogfood correction).

**Status implication.** Spec stays at its current status (Draft — awaiting review). This is a non-blocking clarification, pre-flagged as non-blocking by CP-2; it doesn't revert or advance status.

**Approver.** waseric, approved 2026-07-08.
