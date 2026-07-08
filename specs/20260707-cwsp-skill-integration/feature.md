# CWSP Skill Integration — Feature Specification

> Status: Draft — awaiting review (CP-P1 pending)
> Date: 2026-07-07
> Author: waseric
> Upstream design: [Context Working-Set Protocol (CWSP) architecture](../20260707-context-working-set/architecture.md) (Approved, CP-1 passed 2026-07-07)
> Audience: skill contributors (human), AI agents executing this spec, reviewers at CWSP checkpoints

## 1. Overview

This feature spec executes the [Context Working-Set Protocol design](../20260707-context-working-set/architecture.md) against the `spec-*` skill family. CWSP commits to a shape — the spec and journal are **backing store**; each operation loads a small declared **working set** paged in on demand; **widening is always one cheap grep + range read away**; the journal's **STATE** ("where are we now") is split from its history. This spec turns that committed shape into atomic edits to the skill masters and agent definitions, sequenced so the branch is deployable at every task boundary.

The user-visible behavior added is a **cost reduction**: every `spec-*` orientation and per-boundary re-anchor reads STATE + a scoped working set instead of whole files, dropping the "get centered" cost on the measured example from ~47k tokens toward the design's targets (§6 of the design; < ~5k for execute-orientation, < ~1k for re-anchor) with no quality regression. The change is prose-only: it edits *how skills read and write* their continuity artifacts; it does not change *what* those artifacts contain, and it does not economize on review context.

This is a **meta-repo change**: the "codebase" is instructional markdown — skill masters at `.agents/skills/<name>/SKILL.md` and agent-definition masters at `.agents/agents/<name>.md`, each with a live deploy copy under `~/.claude/`. Per the [repo deploy-sync rule](../../CLAUDE.md), no task is done until master and deploy copy match.

## 2. Goals and Non-goals

**Goals**

- `spec-execute` (inline and dispatch orchestrator) orients and re-anchors on STATE + a task-scoped working set, never a whole-file spec read — closing the suite's single biggest orientation-cost offender.
- `spec-write` and `spec-design` emit a STATE block and a `## Grammar` bootstrap block (native reference dialect) when they create a `journal.md`, and codify any constitution-declared grammar forward into the spec they author.
- `spec-review` and `spec-amend` adopt the STATE/INDEX vocabulary, the cold-reader guarantee, and grammar consult; the amendment heading collapses to the single reference-dialect form.
- Agent definitions (`spec-worker`, `spec-reviewer`) consult STATE on orientation and (for the worker) update STATE on their closeout commit.
- Every reader cross-checks STATE's derivable subset against one grep and treats a mismatch as an automatic widen signal (OQ-1 resolved).
- The whole change is reversible per-skill and requires no new tooling, no build step, and no shared deploy artifact.

**Non-goals**

- Not changing what any spec or journal *contains* (design §2). Only the default *read* is scoped.
- Not building any lint, validator, generator, or shared vocabulary file. Vocabulary lives as bounded prose in each skill; cross-skill drift is a review finding, not a tooling target (design §5.7; §2 of the design).
- Not mandating physical file splits. Tier-1 sealing (P4) is gated behind CP-2 measurement and adopted only if read-discipline alone proves insufficient.
- Not touching the standalone deploy-only dispatch skills (`spec-orchestrate`, `spec-execute-task`, `spec-review-adversarial`) — they have no master in this repo and are governed by no spec here (§12).
- Not economizing on review context: the reviewer's full-diff mandate is untouched.
- Not resolving OQ-2 (working-set sizing), OQ-3 (seal trigger), or OQ-4 (parallel writers) here — deferred to CP-2 / P4 / dispatch OQ-3 per the design.

## 3. Background and Constraints

**Prior art in this repo.** The design spec's §3 is authoritative. Two in-repo instances already run CWSP's shape and are the pattern proof: the **memory system** (`MEMORY.md` index + one-fact shards + `description:` relevance cues) and the **dispatch worker** ([spec-worker.md:25-31](../../.agents/agents/spec-worker.md#L25)), which already orients on one task + the latest journal entry — an early, local working set this spec generalizes.

**Anchor-convention reality (measured, design §3).** INDEX cannot assume one anchor convention per element type: journal entries and spec sections grep cleanly and universally, but tasks appear as `### T-NN` headings *or* a heading-less §7 table (the table is canonical), and amendments have three coexisting heading forms. Go-forward consistency comes from the declared reference dialect (§5.2 here); legacy tolerance comes from broad-union discovery. Neither is policed by a tool.

**Repo house-style grammar is now declared (FQ-1 resolved 2026-07-07).** Per operator direction, the ai-tools constitution declares the reference dialect as repo house style in [tech-stack.md](../tech-stack.md) "Grammar", to manage go-forward anchor drift. This is a **live fixture** for T-01/T-02: spec-write/spec-design read the constitution during discovery and must codify this declared grammar forward into every spec they author. The constitution grammar and this spec's §5.2 reference dialect must stay identical (a CP-P1 consistency check).

**Constraints.**
- Skill masters are portable markdown with uniform frontmatter (`name`, `lastUpdated`, `description` only). Harness-specific keys are prohibited in skill masters; agent definitions are the declared exception (they carry harness frontmatter). CWSP behavior therefore lands in **prose contracts**, never in frontmatter.
- No spec build/compile step exists and CWSP must not introduce one (design §3).
- LLM read discipline is imperfect: a prose "read a slice" instruction is weaker than structural enforcement. This spec relies on prose discipline for Tiers 0; structural enforcement (P4 sealing) is reserved for where discipline measurably fails.
- The suite has no test runner. Verification is **grep-checkable prose assertions**, byte-identical master↔deploy diffs, and **dogfooding** (running a skill against a real spec and measuring orientation tokens).

**Spec repo location.** This feature spec lives in the same repo as the skills it edits (`ai-tools`). For downstream `spec-execute` sessions:
- `SPEC_REPO_ROOT` = the `ai-tools` working tree (spec and skill masters are co-located).
- `SPEC_TARGET_BRANCH` = `main` (work lands on `main`; no feature branches, per repo convention).
- Each task's code edits (skill/agent master) and its spec/journal updates land in the **same repo** — a single commit per task suffices (no cross-repo pairing), but the master→deploy sync is a second file-tree touch under `~/.claude/` that every task's Definition of Done must include.

## 4. Architecture

The design's §4 three-layer decomposition (STATE / INDEX / BODY) is the authoritative architecture; it is not restated here. This spec's architectural contribution is **where each layer's behavior is authored** given the no-shared-file constraint:

```
Canonical vocabulary source (this spec §5)
        │  copied-from (not linked; skills are portable)
        ▼
┌───────────────────────────────────────────────────────────────┐
│ WRITERS  spec-write / spec-design                               │
│   emit  →  STATE block + ## Grammar bootstrap + reference       │
│            dialect  into every new journal.md;                  │
│            codify constitution grammar forward into the spec    │
├───────────────────────────────────────────────────────────────┤
│ READERS  spec-execute / spec-review / spec-amend + agent defs   │
│   consult → STATE first (cross-check derivable subset, 1 grep); │
│            working-set contract for the operation;              │
│            INDEX (grep) → range-read to widen;                  │
│            grammar declared in the spec, else native default    │
├───────────────────────────────────────────────────────────────┤
│ CONSTITUTION  project-constitution                              │
│   may declare → repo house-style ## Grammar (read by writers)   │
└───────────────────────────────────────────────────────────────┘
```

**No shared deploy artifact.** The vocabulary definitions live authoritatively in this feature spec's §5 and are *copied* (as prose) into each skill's own text — each skill carries only the slice it uses. This accepts bounded duplication (STATE shape appears in writers and is consulted by readers) as the price of skill portability, per the confirmed design decision. Cross-skill inconsistency is a defect caught by grep at review, consistent with the design's no-tooling constraint.

**Where it plugs in (exact locations, verified 2026-07-07):**

| Skill / agent | Master path | Integration site |
|---|---|---|
| spec-execute | [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) | Phase 1 ORIENT (`SPEC_PATH in full`, :57); Principle 6 (:49); boundary re-anchor (:177, :209, :277); Phase 6 closeout; DISPATCH MODE orchestrator working set |
| spec-write | [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md) | journal creation (:192) |
| spec-design | [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) | journal creation (:184) |
| spec-review | [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md) | Phase 1 ORIENT (:45); Phase 8 write-back |
| spec-amend | [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) | Phase 1 ORIENT (:53); journal append (:134); amendment heading form |
| project-constitution | [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) | authoritative-artifacts / conventions section (:131) |
| spec-worker | [.agents/agents/spec-worker.md](../../.agents/agents/spec-worker.md) | Orient (:25-31); journal commit (:58-59) |
| spec-reviewer | [.agents/agents/spec-reviewer.md](../../.agents/agents/spec-reviewer.md) | orientation read |

## 5. Detailed Design

§5 is the **canonical vocabulary source**. The blocks below are the copy-ready text each task embeds into its target skill. They restate the design's §5.1/§5.2/§5.3/§5.7 in implementation-ready form; where this spec and the design ever disagree, the design is authoritative and the gap is a `spec-amend` trigger.

### 5.1 STATE block (emitted by writers; consulted by readers)

**Copy-ready shape** (writers emit this at the head of every new `journal.md`; the design's own [journal.md](../20260707-context-working-set/journal.md) is the live exemplar):

```markdown
## Current State
- **Phase:** <e.g. P2 execution | CP-2 review | complete>
- **Last completed:** <task/checkpoint id> (<YYYY-MM-DD>)
- **Next:** <task id + one-line | "CP-N pending" | "spec complete">
- **Open holds:** <blockers / deferred items | none>
- **Pending checkpoint:** <CP-N + spec §ref | none>
- **Archive:** <relative path to journal-archive.md | none — all entries live>
- **Latest entry:** <anchor of the most recent full entry below>
```

**Writer contract.** STATE is written at journal creation and **overwritten in place at every closeout** (task, checkpoint, amendment). It is the *only* in-place-mutated part of the journal; entries below stay append-only. The closeout commit that appends the entry also updates STATE — one commit, not two.

**Reader contract (STATE consult, OQ-1 resolved — cross-check the derivable subset).** A reader reads STATE first, then:
1. Cross-checks STATE's **derivable** fields — *Last completed* and *Latest entry* anchor — against **one grep** of the true latest entry (`grep -nE '^## [0-9]{4}-[0-9]{2}-[0-9]{2} — ' journal.md | tail -1`).
2. If they match, trust STATE and proceed to the working set. If they **mismatch**, STATE is stale: treat it as an automatic **widen signal** — range-read the true latest entry before acting. Staleness thus degrades cost (one extra read), never correctness.

This one-grep cross-check is the resolution of design OQ-1 and is embedded in every reader (spec-execute, spec-review, spec-amend, spec-worker).

### 5.2 Grammar bootstrap block + reference dialect + INDEX contracts

**Copy-ready `## Grammar` bootstrap block** (writers emit adjacent to STATE at the journal head — the one anchor that cannot be dialect-dependent):

```markdown
## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.
```

**Reference dialect (the native default a writer emits absent any override):**

| Element | Canonical anchor |
|---|---|
| Journal entry | `## <YYYY-MM-DD> — <event>` |
| Task closeout | `## <YYYY-MM-DD> — <T-ID>: <title>` |
| Review | `## <YYYY-MM-DD> — Review of <CP-ID>` |
| Amendment | `## <YYYY-MM-DD> — Amendment <id>` (single form) |
| Spec section | `## N. <title>` |
| Task block | `#{3,4} <T-ID> — <title>` (h3 or h4) **and** the §7 table row, where a table exists |

**INDEX resolution (two-tier, derived on demand, never stored).**
1. **Declared dialect (primary).** If a `## Grammar` block is in scope (journal head, or a spec/constitution grammar section), grep its exact patterns — one clean pattern per element type.
2. **Discovery (fallback).** If no dialect is declared, or declared patterns return implausible results, fall back to broad-union patterns and re-derive by inspection:
   - Journal entries: `grep -nE '^## [0-9]{4}-[0-9]{2}-[0-9]{2} — ' journal.md`
   - Spec sections: `grep -nE '^## [0-9]+\. ' <spec>` (never grep `§N` — that form is cross-reference-only)
   - Spec tasks: heading-block discovery is primary — `grep -nE '^#{3,4} T-[0-9]' <spec>` locates per-task headings at either depth; the §7 table row, where one exists, is a secondary index, not assumed canonical
   - Amendments: `grep -nE '^#{2,3} .*Amendment' journal.md` (unions the three legacy heading forms)

**Grammar consult, free-rider rule (design §5.7).** Grammar discovery costs *no extra read*: early writers (spec-write, spec-design) read the constitution during discovery anyway and **codify any declared grammar forward into the spec they author**; later skills (spec-execute, spec-review, spec-amend) read the spec anyway and use its codified grammar if present, else their native default — they do **not** re-consult the constitution. Absent any declaration, every skill uses its native default and INDEX uses discovery.

### 5.3 Working-set contracts (per operation)

The declared default slice per operation (design §5.3). Widening beyond it is always permitted and cheap (§5.4). This table is the exact text each reader task embeds.

| Operation | Working set (default read) | Explicitly *not* read by default |
|---|---|---|
| Execute task T (inline or worker) | STATE + §7 task-table row (if present) + the `#{3,4} T` heading block *if the spec uses per-task headings* (table-only specs: the row is the whole unit; heading-only specs with no §7 table: the heading block is the whole unit) + pending checkpoint contract if any + NFR items the task block cross-references | §1–6, other task blocks, journal history |
| Dispatch orchestrator (per task) | STATE + task-table row + receipt(s) | task-block internals, code, journal history |
| Review checkpoint C | checkpoint contract + in-scope task blocks + relevant NFR items + journal entries for tasks under review + **full diff** | out-of-scope tasks, unrelated sections |
| Amend section S | §S block + sections cross-referencing S + prior amendments to S | unrelated sections, unrelated history |
| Cold verify / audit | STATE → INDEX → guaranteed full paging path (§5.4) | — (may page everything, on demand) |

### 5.4 Widening + cold-reader guarantee (reader contract)

When a working-set read leaves a referenced task, section, amendment, or decision unresolved: consult INDEX (grep), then **range-read** the specific BODY unit — never fall back to a whole-file read. In the default single-file layout (Tier 0) this is trivially total: one file, one grep. A cold reader (fresh session, adversarial verifier, auditor) is guaranteed a complete path. Sealed layouts (Tier 1/2) preserve totality via the two normative rules in design §5.4/§5.5 (atomic seal + STATE-update; discoverable `journal-archive*.md` sibling glob) — relevant only if P4 is adopted.

### 5.5 Deploy-sync + verification method (applies to every task)

- **Deploy-sync.** After editing a master, copy the changed file(s) to the deploy path (`.agents/skills/<name>/` → `~/.claude/skills/<name>/`; `.agents/agents/<name>.md` → `~/.claude/agents/<name>.md`). The task's DoD includes a byte-identical `diff` of master vs. deploy copy returning empty. Never edit a deploy copy directly.
- **Verification without a test runner.** Each task's acceptance criteria are **grep-checkable assertions** over the edited prose (a required phrase is present; a retired phrase — e.g. `read \`SPEC_PATH\` in full` — is absent) plus the master↔deploy diff. Behavioral verification (does orientation actually cost less, with parity) is deferred to the CP-2 dogfood (§8), which is the pilot's substance.
- **`lastUpdated` frontmatter.** Bump the edited master's `lastUpdated` to the task's date.

## 6. Non-functional Requirements

- **Adoptability.** Per-skill, incremental, no coordinated cutover (design §11). A not-yet-migrated skill keeps working (reads more). Each task leaves the branch deployable.
- **Reversibility.** Revert = restore the whole-file-read prose; the emitted STATE/Grammar blocks become harmless extra text in journals already created. No data migration, no flag day.
- **Zero-tooling.** No linter, generator, or build step introduced (design §6). INDEX is grep; STATE and grammar are markdown read as data.
- **Cost (the CP-2 target).** Orientation for "execute task T" drops from whole-spec+whole-journal (~47k tokens on the measured example) to STATE + one task block + one checkpoint contract (target < ~5k tokens); per-turn re-anchor to STATE-only + one cross-check grep (target < ~1k tokens). These are the numbers CP-2 measures.
- **Correctness safety.** No working set may exclude a BODY unit the operation *must* have; widening + the STATE cross-check cover the rest. Under-read degrades to one cheap extra read, never to wrong output (design §4 governing invariant).
- **Consistency across skills (the duplication cost).** Because vocabulary is copied prose, the STATE shape and reference dialect must read identically across skills. This is not tool-enforced; it is a review focus at CP-P1 and CP-P3 and a standing grep check (`grep -A8 '## Current State' across skills`).

## 7. Task Breakdown

Global DoD addenda for **every** task (not repeated per task): master edited; deploy copy synced (byte-identical `diff`); `lastUpdated` bumped; grep assertions pass; commit uses the correct prefix (`<skill-name>:` / `<agent-name>:`); journal entry appended and STATE updated in the same commit; no harness-specific frontmatter added to a skill master.

### P1 — STATE + INDEX + grammar vocabulary

#### T-01 — spec-write emits STATE + Grammar + reference dialect on journal creation
- **Title.** Wire the STATE block, `## Grammar` bootstrap, and reference dialect into `spec-write` journal creation, including constitution→spec grammar codification.
- **Scope.** [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md) — the "Create a `journal.md`" instruction (:192) and Phase 1 Discovery (where the constitution is read). Add: (a) emit §5.1 STATE block + §5.2 `## Grammar` block at journal head; (b) Phase 1 — if `CONSTITUTION_PATHS` declares a grammar, codify it forward into the spec's own grammar section; (c) the reference dialect table as the native default.
- **Acceptance criteria.** *Given* `spec-write` is invoked to author a new feature spec, *when* it creates `journal.md`, *then* the file opens with a `## Current State` block matching §5.1 and a `## Grammar` block matching §5.2. *Given* a constitution with a declared grammar, *when* spec-write authors the spec, *then* the spec carries a codified grammar section. Grep: `## Current State` and `## Grammar` strings present in the skill's journal-creation prose; reference-dialect table present.
- **Tests required.** Grep assertions above. Manual: dry-run the skill's journal-creation prose against the §5.1/§5.2 blocks for verbatim match.
- **Dependencies.** none (this spec §5 is the copy source).
- **Estimated size.** M.
- **Model floor.** opus — defines the emitted shape every downstream skill depends on; a wrong STATE/dialect definition silently propagates to every future spec.

#### T-02 — spec-design emits STATE + Grammar + reference dialect on journal creation
- **Title.** Mirror T-01 into `spec-design` journal creation.
- **Scope.** [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) — the "Create a `journal.md`" instruction (:184) and Discovery (constitution read). Same three additions as T-01, adapted to `architecture.md` authoring.
- **Acceptance criteria.** As T-01, for spec-design. The emitted STATE and `## Grammar` blocks are byte-identical to T-01's (consistency NFR §6). Grep-confirm both blocks present; `diff` the emitted-block prose against T-01's.
- **Tests required.** Grep assertions; cross-skill diff of the STATE/Grammar prose vs. T-01.
- **Dependencies.** T-01 (canonical copy established there).
- **Estimated size.** S.
- **Model floor.** sonnet — mechanical mirror of an established shape; failure is a visible cross-skill diff, caught at CP-P1.

#### T-03 — project-constitution may declare repo house-style grammar
- **Title.** Add an optional house-style `## Grammar` declaration to `project-constitution` and note that early writers read it during discovery.
- **Scope.** [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — conventions/authoritative-artifacts section (:131). Add: constitutions *may* declare a `## Grammar` block (reference dialect shape) as repo house style; spec-write/spec-design will codify it forward. Optional, never mandated.
- **Acceptance criteria.** *Given* an operator wants a repo-wide dialect, *when* project-constitution runs, *then* its prose documents the optional `## Grammar` declaration and its downstream free-rider consumption. Grep: `## Grammar` mention + "house style" + reference to writer codification present.
- **Tests required.** Grep assertions.
- **Dependencies.** T-01 (defines the dialect being referenced).
- **Estimated size.** S.
- **Model floor.** sonnet — additive, optional, low blast radius.

*→ CP-P1 triggers here (T-01–T-03 complete).*

### P2 — spec-execute adoption

#### T-04 — spec-execute retires whole-file reads for STATE + working set
- **Status.** done (2026-07-08; this task's closeout commit — see journal / receipt).
- **Title.** Convert all of `spec-execute`'s read-discipline prose to STATE-first + Execute-task working set + INDEX widening + grammar consult, as one coherent edit.
- **Scope.** [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md): Phase 1 ORIENT (:57, replace "`SPEC_PATH` in full" with the §5.3 Execute-task working set + §5.1 STATE consult incl. the OQ-1 cross-check + §5.2 grammar consult); Operating Principle 6 (:49, resolve the self-contradiction — re-anchor = STATE + next task block, not "re-read the relevant section" ambiguously); boundary re-anchor prose (:177, :209, :277 — "re-read the spec at every boundary" → re-read STATE + the next task block via INDEX); DISPATCH MODE orchestrator per-task read (align to the §5.3 "Dispatch orchestrator" row). Must be reviewed as a unit — a partial edit would leave Phase 1 scoped while boundary prose still says "re-read the whole spec," an internal contradiction.
- **Acceptance criteria.** *Given* spec-execute Phase 1, *when* orienting, *then* the prose directs STATE + working-set + widen-via-INDEX and the string "`SPEC_PATH` in full" is **absent**. *Given* a task boundary, *when* re-anchoring, *then* the prose directs STATE + next-task-block, not a whole-spec re-read. *Given* the dispatch orchestrator, *then* its per-task read matches the §5.3 orchestrator row. Grep: "`SPEC_PATH` in full" absent; "Current State"/"working set"/"widen" present in Phase 1 and boundary prose; no residual "re-read the spec from scratch"/"re-read it at every task boundary" whole-file phrasing.
- **Tests required.** Grep assertions (presence + absence). Manual: read Phase 1 + Principle 6 + Phase 8 + DISPATCH MODE together and confirm no whole-file-read instruction survives and no two sections contradict.
- **Dependencies.** T-01 (STATE/dialect canonical); T-02.
- **Estimated size.** M.
- **Model floor.** opus — highest-traffic skill (~33% of suite usage); the edit spans coupled sites and a plausible-but-wrong scoping would pass grep yet silently under-read at execution. High undetected-failure cost.

#### T-05 — spec-execute Phase 6 closeout updates STATE
- **Status.** done (2026-07-08; this task's closeout commit — see journal / receipt).
- **Title.** Make `spec-execute` Phase 6 closeout overwrite STATE in the same commit as the appended journal entry.
- **Scope.** [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) Phase 6 closeout prose. Add: on task/checkpoint/amendment closeout, overwrite the `## Current State` block per §5.1 in the same commit that appends the entry; entries below stay append-only.
- **Acceptance criteria.** *Given* a task closes out, *when* Phase 6 writes the journal, *then* the prose requires a STATE overwrite in the same commit. Grep: "Current State" + "same commit" present in Phase 6 prose.
- **Tests required.** Grep assertions.
- **Dependencies.** T-04.
- **Estimated size.** S.
- **Model floor.** sonnet — additive write-side rule, objectively grep-verifiable.

#### T-06 — spec-worker consults + updates STATE
- **Status.** done (2026-07-08; this task's closeout commit — see journal / receipt).
- **Title.** Add STATE consult (with OQ-1 cross-check) to the worker's orient, and STATE update to the worker's journal commit.
- **Scope.** [.agents/agents/spec-worker.md](../../.agents/agents/spec-worker.md): Orient (:25-31 — read STATE first + one-grep cross-check before the task + latest-entry read it already does); journal commit (:58-59 — overwrite STATE per §5.1 in the paired spec/journal commit).
- **Acceptance criteria.** *Given* a worker spawns cold, *when* it orients, *then* it reads STATE + cross-checks + the scoped task/latest-entry (unchanged) — no whole-file read introduced. *Given* the worker closes out, *then* it overwrites STATE in its journal commit. Grep: "Current State" present in Orient and journal-commit prose; cross-check grep phrasing present.
- **Tests required.** Grep assertions.
- **Dependencies.** T-04 (reader contract established); T-05 (STATE-write pattern).
- **Estimated size.** S.
- **Model floor.** opus — the worker writes artifacts autonomously in dispatch runs; a wrong STATE-write rule corrupts continuity across an unattended batch, undetected until a later reader mis-orients.

*→ CP-2 (Pilot validation) triggers here (P2 complete). This is the design's CP-2 (architecture.md §9).*

### P3 — Cross-skill adoption

#### T-07 — spec-review adopts STATE/INDEX vocabulary + cold-reader guarantee + STATE write-back
- **Status.** done (2026-07-08; commit e1bd8a8 — see journal).
- **Title.** Wire STATE/INDEX vocabulary, the cold-reader guarantee, grammar consult, and Phase 8 STATE write-back into `spec-review`.
- **Scope.** [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md): Phase 1 ORIENT (:45 — STATE consult + cross-check + grammar consult; the Review-checkpoint working set from §5.3; **full-diff mandate explicitly preserved**); Phase 8 write-back — overwrite STATE in the same commit as the verdict entry.
- **Acceptance criteria.** *Given* spec-review Phase 1, *then* prose directs STATE + checkpoint-contract-scoped reads while retaining the full-diff mandate (grep: "full diff" mandate still present — not weakened). *Given* Phase 8, *then* STATE is overwritten in the verdict commit. Grep: "Current State" present in Phase 1 + Phase 8; full-diff language intact.
- **Tests required.** Grep assertions (presence + full-diff-retention).
- **Dependencies.** T-04 (reader contract).
- **Estimated size.** M.
- **Model floor.** sonnet — spec-review is already selective; edit is additive vocabulary + a write-back, verifiable by grep. Guarded by CP-P3.

#### T-08 — spec-reviewer consults STATE on orientation
- **Status.** done (2026-07-08; commit 4f3a6e8 — see journal).
- **Title.** Add STATE consult (with cross-check) to the `spec-reviewer` agent definition's orientation read.
- **Scope.** [.agents/agents/spec-reviewer.md](../../.agents/agents/spec-reviewer.md) — orientation read. Add STATE-first + cross-check; full-diff mandate unchanged.
- **Acceptance criteria.** *Given* a reviewer subagent spawns cold, *then* it reads STATE + cross-checks before the checkpoint-scoped reads; full-diff mandate intact. Grep: "Current State" present; full-diff language intact.
- **Tests required.** Grep assertions.
- **Dependencies.** T-07.
- **Estimated size.** S.
- **Model floor.** sonnet — additive, grep-verifiable.

#### T-09 — spec-amend adopts STATE/INDEX vocabulary + STATE write-back
- **Title.** Wire STATE/INDEX vocabulary, grammar consult, and STATE write-back into `spec-amend`.
- **Scope.** [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md): Phase 1 ORIENT (:53 — STATE consult + cross-check + grammar consult; the Amend-section working set from §5.3); journal append (:134 — overwrite STATE in the amendment commit).
- **Acceptance criteria.** *Given* spec-amend Phase 1, *then* prose directs STATE + section-scoped reads. *Given* the amendment journal append, *then* STATE is overwritten in the same commit. Grep: "Current State" present in Phase 1 + journal-append prose.
- **Tests required.** Grep assertions.
- **Dependencies.** T-04 (reader contract).
- **Estimated size.** S.
- **Model floor.** sonnet — additive vocabulary + write-back, grep-verifiable.

#### T-10 — spec-amend collapses to the single amendment dialect form
- **Title.** Retire the three-variant amendment heading and the double-record; emit the single reference-dialect form `## <YYYY-MM-DD> — Amendment <id>`.
- **Scope.** [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) — the amendment heading template (:75) and journal-entry template (:134-139), and any prose prescribing a separate verbatim "Full record". Emit one dated `## <YYYY-MM-DD> — Amendment <id>` heading holding the full record; drop the double-record. Note that INDEX's union pattern (§5.2) still reads *legacy* three-form amendments — this change is go-forward only, non-destructive to existing journals.
- **Acceptance criteria.** *Given* spec-amend emits an amendment, *then* it writes exactly one dated `## ... — Amendment <id>` heading and no separate `### Full record` block. Grep: single-form heading template present; "Full record" double-record prose absent; a note that legacy forms remain readable via discovery present.
- **Tests required.** Grep assertions (presence + absence). Manual: confirm the emitted form matches the §5.2 reference-dialect amendment row.
- **Dependencies.** T-09.
- **Estimated size.** M.
- **Model floor.** opus — this changes the *emitted grammar*; getting it wrong desynchronizes new amendments from the INDEX contract and the reference dialect, and interacts with legacy-discovery guarantees. Judgment-bearing.

*→ CP-P3 triggers here (P3 complete).*

### P4 — Tier-1 sealing (gated)

#### T-11 — Define and dogfood seal-at-checkpoint (GATED on CP-2 outcome)
- **Title.** Define the Tier-1 seal-at-checkpoint step (atomic seal + STATE-update; `journal-archive*.md` sibling convention; INDEX spans live+archived) and dogfood it on this spec's own journal.
- **Scope.** spec-execute / spec-review closeout prose (or a shared seal note referenced by both); resolves design OQ-3 (seal trigger). **Do not start** unless CP-2's exit criteria conclude that read-discipline alone is insufficient (design §5.5, §7 P4, §10 "Adds ceremony without payoff").
- **Acceptance criteria.** *Given* CP-2 justified sealing, *when* a checkpoint closes, *then* completed BODY units move verbatim to `journal-archive*.md` in one atomic closeout with STATE's **Archive** pointer updated; INDEX (grep) spans both files; a cold reader reaches archived units by glob even under pointer staleness. Loss-free: post-seal INDEX enumerates the same unit count as pre-seal.
- **Tests required.** Dogfood on this journal; pre/post-seal INDEX unit-count equality; cold-read reconstruction.
- **Dependencies.** CP-2 exit (gate); T-04–T-10.
- **Estimated size.** L — **must be split before implementation** if the gate opens (seal-step definition; spec-execute wiring; spec-review wiring; dogfood + cold-read validation).
- **Model floor.** opus — irreversible-ish data movement (sealing) where a garbled move loses history; guarded but high-impact.

## 8. Test Strategy

- **Unit approach.** No code units exist. "Unit" verification is **grep-checkable prose assertions** per task (required phrase present; retired phrase absent) plus the master↔deploy byte-identical `diff`. Each task's acceptance criteria name the exact strings.
- **Integration approach.** The cross-skill consistency check: `grep` the emitted STATE block and reference dialect across all writer/reader skills and confirm byte-identical shape (§6 consistency NFR). Run at CP-P1 and CP-P3.
- **Behavioral / dogfood (CP-2 substance).** Take the representative multi-task feature measured for the design's §1 (spec ~21k / journal ~26k / design ~13k). Execute one task boundary **both ways** — whole-file baseline vs. CWSP working set — and measure orientation tokens against the §6 targets (< ~5k orient, < ~1k re-anchor). Confirm **output parity** (same task, same result) — the design fails if scoped reads degrade output.
- **Cold-read validation.** A fresh agent given only STATE + INDEX must reconstruct current state and reach any historical decision, reusing dispatch-execution's CP-2 cold-read method (design §8).
- **Test data.** The real in-repo specs are the corpus — no synthetic data. This spec's own journal is the primary dogfood target (it already carries STATE + Grammar).

## 9. Review Checkpoints

**CP-P1 — Vocabulary consistency (feature-local).**
- *Trigger.* T-01–T-03 complete.
- *Review focus.* Do the emitted STATE block and reference dialect read **byte-identically** across spec-write and spec-design? Does the `## Grammar` bootstrap match §5.2? Does the reference dialect emitted by the skills match the **constitution's declared grammar** ([tech-stack.md](../tech-stack.md) "Grammar") — the two must be identical? Is the constitution→spec codification free-rider correct (no skill reads a file solely for grammar)? Is the OQ-1 cross-check phrased as a reader contract, not baked into writers?
- *Exit criteria.* Cross-skill STATE/dialect diff is empty; skill dialect matches the constitution grammar; grep assertions for T-01–T-03 pass; no whole-file-read regression introduced.
- *Reviewer floor.* opus — guards the vocabulary every later phase copies; an inconsistency here propagates.
- *Status.* **changes requested** on 2026-07-07 by waseric (opus, inline) — 1 blocker: skill dialect ≠ constitution grammar (constitution [tech-stack.md](../tech-stack.md) says "§7 **task-table** row"; §5.2 + both skills say "§7 **table** row"). Cross-skill (writer↔writer) diff empty; STATE/bootstrap faithful to §5.1/§5.2; free-rider correct; OQ-1 not baked into writers; deploy-sync byte-identical. Checkpoint stays open pending reconciliation. See journal verdict entry.
  **Re-review: pass** on 2026-07-08 by waseric (opus, inline) — blocker resolved: constitution [tech-stack.md](../tech-stack.md) Task-block anchor reconciled to "§7 table row", now byte-identical to §5.2 and both skills; important (CRLF) resolved by normalizing tech-stack.md to LF. All exit criteria met; 0 blockers. Checkpoint **CLOSED**. Fix lives in the working-tree tech-stack.md edit — must land as the paired spec-side commit alongside this verdict. Next: P2 (T-04).

**CP-2 — Pilot validation (the design's CP-2; architecture.md §9).**
- *Trigger.* P2 (T-04–T-06) implemented.
- *Review focus.* Measured orientation-token reduction vs. §6 targets; output parity vs. baseline; no correctness loss from scoped reads; STATE stays accurate across boundaries; the "`SPEC_PATH` in full" retirement introduced no under-read path.
- *Exit criteria.* Orientation cost meets §6 target; zero quality regression on the dogfood task; cold-read reconstruction succeeds. **This checkpoint's outcome also decides whether P4 (T-11) is adopted** — sealing proceeds only if scoped reads alone prove insufficient.
- *Reviewer floor.* fable — the go/no-go gate guarding all downstream adoption and the "the design is wrong" risk (design §10); the most consequential review in this spec.
- *Status.* **pass with comments** on 2026-07-08 by waseric (fable, dispatched spec-reviewer — floor met). 0 blockers, 1 important, 2 advisory. Measured orientation for the T-07 boundary: ~0.7k tokens core / ~2.4k with the expected vocabulary-copy widen, vs. the < ~5k target (~8× reduction from the ~19k whole-file baseline on this corpus); re-anchor ~0.17k vs. the < ~1k target. Output parity held — T-04–T-06 were themselves executed under the migrated scoped discipline with the STATE cross-check passing at each boundary; deploy-sync byte-identical; STATE accurate across all three boundaries; cold-read reconstruction total (Tier-0 path guaranteed). All three exit criteria met. **P4 (T-11) NOT adopted** — scoped reads proved sufficient with 2–6× margin and low widen frequency; T-11 stays gated shut, revisit only if a future corpus breaches the targets. One important finding: the declared task-anchor grammar (§5.2 `### <T-ID>` + "§7 task table is canonical") misses this spec's own `#### T-NN` / no-§7-table shape — correctness-safe (miss → cheap widen) but live anchor drift; routes to `spec-amend` on §5.2, non-blocking, land before/with CP-P3. Checkpoint **CLOSED**. Next: P3 / T-07. See journal verdict entry.

**CP-P3 — Cross-skill consistency (feature-local).**
- *Trigger.* P3 (T-07–T-10) complete.
- *Review focus.* Vocabulary reads identically across spec-review, spec-amend, and the agent defs; the full-diff mandate survived T-07/T-08 intact; the amendment single-form (T-10) matches the reference dialect and legacy discovery still reads old forms; STATE write-backs are present in every write phase.
- *Exit criteria.* Cross-skill grep consistency holds; full-diff mandate demonstrably intact; amendment-form grep assertions pass.
- *Reviewer floor.* opus — cross-skill consistency + a grammar-form change with legacy-compat implications.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Scoped reads degrade output (the design is wrong) | Low | High | CP-2 quality-parity check on a real task before P3/P4 broad adoption; dispatch CP-2 already evidences the opposite | CP-2 |
| T-04 partial edit leaves spec-execute internally contradictory (Phase 1 scoped, boundary prose still whole-file) | Med | High | T-04 reviewed as one unit; grep asserts *absence* of every whole-file phrasing, not just presence of the new one | T-04 / CP-2 |
| Copied vocabulary drifts across skills (no lint) | Med | Med | Byte-identical cross-skill diff at CP-P1 and CP-P3; standing grep check (§6); this spec §5 is the single copy source | CP-P1 / CP-P3 |
| STATE drifts stale (hand-maintained) | Med | Low | OQ-1 cross-check (one grep) self-announces staleness → automatic widen; STATE updated in same closeout commit | readers |
| Under-reading → wrong work | Med | High | Working sets sized to include declared cross-refs; guaranteed cheap widening; reviewer full-diff untouched | CP-2 |
| Amendment single-form (T-10) breaks reading of legacy 3-form journals | Low | Med | Change is go-forward only; INDEX union pattern (§5.2) still reads legacy forms; non-destructive to existing journals | T-10 / CP-P3 |
| Deploy copy drifts from master (edited copy, or missed sync) | Med | Med | Every task DoD requires empty master↔deploy `diff`; never edit deploy copies | every task |
| P4 sealing garbles/loses an entry | Low | High | Gated behind CP-2; verbatim move, headings unchanged, pre/post INDEX count equality, cold-read check | T-11 |

## 11. Rollout and Rollback

- **No feature flag** — skill prose has no runtime flag. Rollout *is* the per-skill merge sequence; each task is independently deployable and independently revertible.
- **Ordering.** P1 (writers emit vocabulary) → P2 (spec-execute consumes it) → CP-2 gate → P3 (cross-skill) → optional P4. A skill not yet migrated keeps functioning (reads more); a scoped reader that hits an un-migrated (STATE-less) journal falls back to discovery + whole-file read — safe degradation, no lockstep cutover (design §11).
- **Rollback.** Revert the offending skill's commit; the master→deploy sync of the revert restores prior behavior. Emitted STATE/Grammar blocks in already-created journals become inert extra text — no cleanup required, no data migration.
- **Monitoring during rollout.** The success signal is the CP-2 measurement (orientation tokens down, parity held). The failure signal is any dogfood output divergence or a cold-read reconstruction failure — either halts adoption at CP-2 before P3.

## 12. Out of Scope

- The standalone deploy-only dispatch skills `spec-orchestrate`, `spec-execute-task`, `spec-review-adversarial` — no master exists for them in this repo and no spec here governs them; their orientation reads may benefit from CWSP but adopting them is a separate effort. (The dispatch *orchestrator* covered here is spec-execute's `EXECUTION: dispatch` mode, which is mastered in this repo.)
- Any change to receipt schema, model floors, or the 80k budget trigger — dispatch-execution owns those (design §12).
- A stored/generated manifest or shared vocabulary deploy file (design §5.7 rejects the third deploy class; confirmed in Clarify).
- Compressing or summarizing BODY content — CWSP scopes *reads*, not rewrites (design §12).
- Parallel-writer journaling / Tier-2 sharding (design OQ-4; coupled to dispatch OQ-3).
- Resolving OQ-2 (working-set sizing) at authoring time — tuned at CP-2 from observed widen frequency.

## 13. Open Questions

| ID | Question | Analysis / leaning | Owner | Target |
|---|---|---|---|---|
| ~~FQ-1~~ **RESOLVED (2026-07-07)** | Should a house-style grammar be declared for *this* repo now, or left as an available-but-unused capability? | **Resolved: declared now.** Operator directed declaring a repo house-style grammar to manage go-forward anchor drift across the growing spec/journal corpus. A `## Grammar` section was added to the constitution ([tech-stack.md](../tech-stack.md) "Grammar"), carrying the reference dialect. This becomes a live input to T-01/T-02 — spec-write/spec-design will codify it forward into every spec they author. | operator | ✔ done |
| OQ-2 (design) | How wide is each default working set before widening is expected? | Design leaning: single unit + declared dependencies-by-reference; let block cross-refs drive widening. Tune from observed widen frequency. | CP-2 | at CP-2 |
| OQ-3 (design) | Tier-1 seal trigger — checkpoint close vs. size threshold? | Design leaning: checkpoint close primary; size threshold only if a spec runs long between checkpoints. Only relevant if P4 gate opens. | T-11 / P4 | if CP-2 opens P4 |
| OQ-6 (design) | Grammar evolution after codification. | Design leaning (adopted): codified grammar is a point-in-time snapshot fixed for the spec's life; mid-flight change routes through `spec-amend`. No action unless a live spec needs a mid-flight dialect change. | P3 / spec-amend | as encountered |

Design OQ-4 (parallel writers) is deferred wholesale to dispatch OQ-3 and is not this spec's to resolve.

## 14. References

### Authoritative (binding — this spec's commitments must match these)
- [Context Working-Set Protocol architecture](../20260707-context-working-set/architecture.md) — the design this spec executes; source of the STATE/INDEX/BODY shape, working-set contracts (§5.3), grammar model (§5.7), splitting tiers (§5.5), NFR targets (§6), CP-2 contract (§9), and all open questions.
- [ai-tools CLAUDE.md](../../CLAUDE.md) — deploy-sync rule, skill-change workflow, commit-prefix and no-feature-branch conventions, the design-bar three properties.
- [dispatch-execution architecture](../20260705-dispatch-execution/architecture.md) — model-floor ladder (`haiku/sonnet/opus/fable`), worker-scoping precedent (the dispatch worker's existing one-task working set), CP-2 cold-read method reused in §8.
- The `spec-*` and agent-definition masters listed in §4 — the exact artifacts this spec edits.

### Inspirational (informed the design; non-binding here)
- Denning, P. J., "The Working Set Model for Program Behavior," *CACM* 1968 — working-set / demand-paging model.
- Nielsen, J. (Nielsen Norman Group), "Progressive Disclosure," 1995 — show essentials first, reveal on demand.
