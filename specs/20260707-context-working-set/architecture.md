# Context Working-Set Protocol (CWSP) — Architecture and Protocol Specification

> Status: Adopted — 2026-07-08. CP-1 passed 2026-07-07; CP-2 (pilot validation) passed 2026-07-08 via the downstream feature spec `../20260707-cwsp-skill-integration/feature.md` (Complete). Implemented across the spec-* suite; Tier-1 sealing (P4) deliberately not adopted (scoped reads proved sufficient at CP-2).
> Date: 2026-07-07
> Author: waseric
> Audience: skill contributors (human), AI agents executing/reviewing/amending specs, stakeholders evaluating the spec-* suite's cost profile

## 1. Overview

The Context Working-Set Protocol (CWSP) governs *what portion* of a spec's continuity artifacts — the spec file (`feature.md` / `architecture.md`) and the `journal.md` — an agent loads into context for a given operation. Today the spec-* suite treats these as whole-file reads: `spec-execute` reads the spec "in full" and re-runs that read at every task boundary. On a representative multi-task feature (spec ~21k tokens, journal ~26k tokens, upstream design doc ~13k tokens; measured 2026-07-07), an agent pays roughly 47k tokens to "get centered" before opening a line of code — and a batch executor pays a large fraction of that *per turn, per task*. Against high-cost models this produces the multi-turn-at-200k+ sessions that dominate the suite's spend.

CWSP commits to a shape: the spec and journal are **backing store**; each operation loads a small, declared **working set** paged in on demand; **widening is always one cheap targeted read away**; and the journal's *state* ("where are we now") is separated from its *history* ("what happened"). The load-bearing invariant is discoverability — **slices are fine; orphaned context is not.** A scoped read must always carry a cheap, complete map back to everything it was sliced from. This is the same pattern already running in this repo's memory system (`MEMORY.md` index + one-fact shards + `description` relevance cues). CWSP does not change *what* the spec and journal contain, and it does not economize on verification: a reviewer still reads the full diff.

## 2. Goals and Non-goals

**Goals**

- Reduce the cost of *orientation* (getting centered) from O(whole file) to O(working set) for every spec-* operation, on every turn.
- Make per-turn re-anchoring cheap enough that batch and dispatch executors can re-center each boundary without accumulating whole-file reads.
- Guarantee that any agent can *discover* and *reach* the full context from any scoped slice — no silent context loss.
- Preserve the output quality that whole-context reads currently deliver, by loading the *right* slice rather than *all* of it.
- Require no new build tooling: the navigation index is derived on demand from existing anchors (grep), not a maintained artifact that can drift.

**Non-goals**

- Not reducing what the spec/journal *contain*. Richness is preserved; only the default *read* is scoped.
- Not mandating physical file splits. Splitting is a reachable tool justified by enforcement or write-contention, not the standard layout (§4, §5.5).
- Not economizing on verification/review context. The reviewer's full-diff mandate is untouched (extends dispatch-execution's "starve context, not verification").
- Not introducing a generator/compiler step for specs. Specs stay hand-authored markdown.
- **Not building linting or validation tooling.** Anchor discipline is achieved by *declared, self-describing grammar* (§5.7) read as data, never by a tool that checks conformance. Enforcement is soft: writers emit per a declared dialect; readers consult it; content remains re-derivable ground truth.
- Not solving parallel-writer journaling. That is coupled to dispatch-execution OQ-3 and deferred here (§13 OQ-4).

## 3. Background and Constraints

**Prior art in this repo.**
- [dispatch-execution](../20260705-dispatch-execution/architecture.md) attacks context cost via *execution topology*: a thin orchestrator that never opens code, disposable per-task workers scoped to one task, 25-line receipts, derivation re-check, an 80k-token budget trigger. CWSP is orthogonal: dispatch narrows *who holds* context; CWSP narrows *what any reader loads* — including inline-mode `spec-execute` and the dispatch orchestrator itself. Dispatch's worker brief ("scoped to one task") is an early, local instance of a CWSP working set; CWSP generalizes it.
- The earlier `specs/20260514-session-economy/` established the cost-of-context framing.
- The suite's governing doctrine ([CLAUDE.md](../../CLAUDE.md)): three properties in tension — token economy ("starve context, not verification"), batch autonomy, rework prevention. CWSP extends the first to: *starve orientation, not the working set.*

**In-house pattern precedent.** This repo's memory system already runs CWSP's shape: `MEMORY.md` is a small always-loaded index (one hook per memory), each fact is an addressable shard, and `description:` frontmatter is the relevance cue for deciding what to page in. It is proof the pattern works and is idiomatic to the operator.

**Named-pattern prior art (external, inspirational).**
- **Working-set model / demand paging** — Peter J. Denning, "The Working Set Model for Program Behavior," *CACM* 1968: a process actively references only a small locality of its pages within a recent window; pages load on demand. CWSP treats spec/journal as backing store and the operation's needs as its working set. Verified 2026-07-07: [denninginstitute.com/pjd/PUBS/Workingsets.html](https://www.denninginstitute.com/pjd/PUBS/Workingsets.html).
- **Progressive disclosure** — Jakob Nielsen / Nielsen Norman Group (1995): show the essential slice first, reveal complexity on demand. CWSP's STATE-first, widen-on-demand read order is progressive disclosure applied to spec orientation. Verified 2026-07-07: [nngroup.com](https://www.nngroup.com/videos/progressive-disclosure/).

**Constraints.**
- Specs are plain markdown under version control; there is no spec build/compile step, and CWSP must not require one.
- LLM read discipline is imperfect — an instruction to "read a slice" is weaker than a layout that makes over-reading require an explicit extra action. This is why splitting (structural enforcement) has value where discipline alone is insufficient (§5.5), mirroring dispatch's tool-level worker scoping.
- Journals use dated `## <YYYY-MM-DD> — <event>` headings; specs use numbered `## N.` sections and (mostly) `### T-NN` task blocks. These stable anchors make a *derived* index feasible with zero new maintained surface — a first-class constraint CWSP relies on.

**Anchor-convention validation (measured 2026-07-07).** The grep-derived-INDEX bet was tested against a separate private repo's corpus (feature specs, findings-pipeline journals, and constitution-change records). Result — *holds with caveats*:
- **Journal entries and spec sections grep cleanly and universally**, including findings-pipeline and constitution-change journals (same dated `##` shape) — so CWSP scales to those journal types unchanged. There is no separate "project constitution" journal; constitution changes are recorded as amendment entries inside spec journals.
- **Sections are `## N.`, never `## §N`** — the `§N` form appears only in cross-references. INDEX greps `## N.`.
- **Tasks are usually `### T-NN` but sometimes a Markdown table** (no per-task heading). A heading-only grep silently under-counts, so the §7 task table — always present — is the canonical task index.
- **Amendments have three coexisting heading forms** (dated `##`, undated `## Amendment`, `### Amendment`) across feature vs. architecture journals; enumerating them needs a union pattern.
This measured variance is why INDEX resolves in two tiers (§5.2) — a declared dialect where present, broad-union discovery otherwise — and why go-forward consistency comes from a declared grammar (§5.7) rather than a lint. Variance is *tolerated* (declared or discoverable), not policed.

**Spec repo location.** This design spec lives in the same repo as the skills it governs (`ai-tools`). `SPEC_REPO_ROOT` = the `ai-tools` working tree; `SPEC_TARGET_BRANCH` = `main` (work lands on `main`; no feature branches per repo convention). Downstream feature-spec work also lands here.

## 4. Architecture

Both continuity artifacts decompose into three layers:

```
┌─ STATE  — small, always loaded, hand-maintained ────────────────┐
│   "where are we now": phase, last done, next, open holds,        │
│   pending checkpoint, archive pointer, latest-entry anchor        │
├─ INDEX  — derived on demand, never stored, zero-drift ───────────┤
│   grep the stable anchors:                                        │
│     journal:  ^## <date> — <event>                                │
│     spec:     ^### T-NN   (task blocks)  /  ^## <N>\.  (sections) │
├─ BODY   — addressable units, paged in on demand by range ────────┤
│   one journal entry / one task block / one section                │
└──────────────────────────────────────────────────────────────────┘
```

**Read order (progressive disclosure).** Any operation reads STATE first, then the working set for that operation (§5.3), then — only if a cross-reference or ambiguity demands — widens via INDEX to a specific BODY unit by range. Whole-file reads are retired as the default.

**The STATE/INDEX split is the core commitment.**
- **STATE is stored and hand-maintained** — it is the one thing grep cannot derive ("where are we"). Small, hot, updated at each closeout.
- **INDEX is derived by grep, never stored** — so it cannot drift from the BODY. The anchors already exist in every artifact today.

**Splitting is a reachable tool, not the standard.** Token cost is a *read-behavior* problem, not a *file-layout* problem: a range-scoped read of one large file costs the same tokens as reading a small split file. Splitting therefore buys only two things that convention cannot — (1) **structural enforcement** (over-reading requires explicitly opening the archive/appendix, not merely resisting a whole-file read), and (2) **write-contention relief** (parallel writers). The default layout is single-file (Tier 0); splits (Tier 1/2) are adopted only where enforcement or contention actually pays (§5.5).

**Discoverability invariant (load-bearing).** Whatever the layout, from any scoped slice an agent can (a) enumerate every BODY unit that exists (INDEX), and (b) reach any of them cheaply (range read, or open the pointed-to archive named in STATE). No context is ever silently sliced away.

**Governing invariant — declared conveniences, re-derivable truth.** Every *stored or declared* element in CWSP (STATE; the grammar dialect of §5.7) is a cheap **hint** whose ground truth is always re-derivable from the BODY. STATE is re-derivable by widening; the anchor grammar is re-derivable by broad-grep discovery. No stored/declared element is load-bearing for *correctness* — only for *cost*. This is what lets CWSP need no lint or validator: staleness of any hint degrades cost, never correctness, and an agent can always re-derive by inspection.

**Where CWSP plugs in.** Each spec-* skill's orientation phase and per-boundary re-anchor step reads STATE + working set instead of whole files. `spec-execute` (inline and dispatch orchestrator) is the primary adopter; `spec-review`, `spec-amend`, `spec-write`, `spec-design` adopt the STATE/INDEX vocabulary for their reads and writes.

**Vocabulary** (used consistently hereafter): **STATE** (the current-state block), **INDEX** (the grep-derived map), **BODY unit** (one addressable entry/block/section), **working set** (the declared slice for an operation), **widening** (an on-demand targeted read of an additional BODY unit), **sealing** (moving completed BODY units to an archive file, Tier 1), **grammar / dialect** (the declared anchor conventions of an artifact, §5.7), **discovery** (broad-union re-derivation of anchors when no dialect is declared).

## 5. Detailed Design

### 5.1 STATE block

- **Purpose.** Make "where are we now" O(1) to read, replacing the need to page chronological history to re-center.
- **Shape.** A single block maintained at the head of `journal.md`:

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

- **Behavior.** Overwritten in place at every closeout (task, checkpoint, amendment). It is the only in-place-mutated part of the journal; entries below remain append-only. If STATE is stale, an agent reading it plus the latest entry still self-corrects by widening — staleness degrades cost, never correctness (§10).
- **Pattern invoked.** Materialized view / cache of derivable state. Kept small so the drift cost is bounded.
- **Why this design.** STATE is the exact information grep cannot produce; storing that (plus a small derivable convenience subset) and deriving everything else minimizes the maintained, drift-prone surface to a few hundred tokens.
- **Alternatives considered.** A fully stored manifest (J4) carrying gist+status per entry — richer, but a second thing to keep in sync with BODY; rejected as default in favor of derived INDEX. Chosen scope: store non-derivable state plus a minimal derivable convenience subset (last-completed, latest-entry anchor) whose freshness OQ-1 governs.

### 5.2 Derived INDEX

- **Purpose.** Enumerate all BODY units and locate any of them, at zero maintenance cost.
- **Two-tier resolution.** INDEX is derived by grep, but *which* patterns to grep is resolved in two tiers:
  1. **Declared dialect (primary).** If a grammar declaration is in scope (§5.7), use its exact patterns — one clean pattern per element type, cheap and unambiguous.
  2. **Discovery (fallback).** If no dialect is declared (legacy/undeclared artifact) or the declared patterns return implausible results (e.g. zero tasks in a spec that clearly has them), fall back to the broad **union** patterns below and re-derive anchors by inspection. Discovery is the re-derivation that makes the declaration a mere convenience, not a correctness dependency.
- **Union fallback contracts.** Validated 2026-07-07 against a separate private repo's spec/journal corpus (feature, findings, and constitution-change journals):
  - Journal entries: `grep -nE '^## [0-9]{4}-[0-9]{2}-[0-9]{2} — ' journal.md` → chronological list (date, title, line) → range-read any entry. Near-universal: dated `##` headings with an em-dash delimiter hold across feature, findings-pipeline, and constitution-change journals.
  - Spec sections: `grep -nE '^## [0-9]+\. ' <spec>`. Universal. (The `§N` form occurs only in cross-references and prose, never as a heading — do not grep for it as an anchor.)
  - Spec tasks: **the §7 task table is the canonical index** (always present, whether or not per-task headings exist). Where the spec uses per-task blocks, `grep -nE '^### T-[0-9]' <spec>` locates them for range-reading; where tasks are table-encoded (no headings), the table row *is* the unit and there is nothing further to page in.
  - Amendments: union pattern `grep -nE '^#{2,3} .*Amendment' journal.md`. Measured reality: three coexisting forms — dated wrapper `## <date> — Amendment N`, undated detail `## Amendment N — <refs>`, and (in architecture journals) `### Amendment N — <ref>`. Heading level and date-presence vary; a single dated-entry pattern under-counts.
- **Behavior.** Produced on demand when an agent needs to widen or audit; never written to disk. Because it is derived from the same headings/tables the BODY carries, it cannot disagree with the BODY. Where an element type has *measured* convention variance (amendments; task heading-vs-table), the INDEX unions patterns per type and treats the §7 table as canonical for tasks — it does not assume one anchor convention per element.
- **Pattern invoked.** Derived/computed index over a canonical source (no denormalized copy), with a declared-dialect fast path.
- **Why this design.** Eliminates the manifest-drift failure mode entirely. Declared dialect makes the common (go-forward) case one clean grep; union discovery makes the legacy case correct without touching old content. Observed anchor variance (§3) is thus *tolerated* — variance is fine as long as it is either declared or discoverable — rather than requiring a lint or a mass rewrite.
- **Alternatives considered.** A conformance lint (rejected per operator constraint — no tooling; §2). Stored `INDEX.md` — richer per-entry metadata, but reintroduces drift; deferred to Tier 2 only if sharding forces it.

### 5.3 Working-set contracts (per operation)

The declared default slice for each operation. Widening beyond it is always permitted and cheap.

| Operation | Working set (default read) | Explicitly *not* read by default |
|---|---|---|
| Execute task T (inline or worker) | STATE + §7 task-table row + the `### T` block *if the spec uses per-task headings* (table-only specs: the row is the whole unit) + pending checkpoint contract if any + NFR items the task block cross-references | §1–6, other task blocks, journal history |
| Dispatch orchestrator (per task) | STATE + task-table row + receipt(s) | task-block internals, code, journal history |
| Review checkpoint C | checkpoint contract + in-scope task blocks + relevant NFR items + journal entries for tasks under review + **full diff** | out-of-scope tasks, unrelated sections |
| Amend section S | §S block + sections cross-referencing S + prior amendments to S | unrelated sections, unrelated history |
| Cold verify / audit | STATE → INDEX → guaranteed full paging path (§5.4) | — (may page everything, on demand) |

- **Pattern invoked.** Working set sized to the operation's locality (Denning).
- **Why this design.** The quality of spec-* output comes from the *right* slice being present, not all of it — evidenced by dispatch's CP-2 cold-read validation passing with a worker scoped to one task, and by `spec-review`/`spec-amend` already reading selectively. Under-reading risk is bounded by sizing each working set to include the cross-references the operation actually needs, plus guaranteed cheap widening.
- **Alternatives considered.** Per-skill ad-hoc scoping (status quo) — inconsistent and leaves `spec-execute`'s whole-file read in place; rejected in favor of one declared contract table.

### 5.4 Widening protocol and the cold-reader guarantee

- **Purpose.** Make "I need more" safe and cheap, so scoped reading never risks wrong work.
- **Behavior.** When a working-set read leaves a referenced task, section, amendment, or prior decision unresolved: consult INDEX (grep), then range-read the specific BODY unit — never fall back to a whole-file read. A **cold reader** (adversarial verifier, auditor, a fresh session) is guaranteed a complete path. In the default single-file layout (Tier 0) this is trivially total — one file, one grep. Under a sealed layout (Tier 1/2), totality is preserved by two normative rules: seal and STATE-update occur in one atomic closeout, and archives follow a discoverable sibling naming convention (`journal-archive*.md`) so a cold reader enumerates them by glob even if STATE's **Archive** pointer lags. INDEX then spans live + archived files; every unit is reachable by a range read or by opening a discovered archive.
- **Pattern invoked.** Progressive disclosure with a total-coverage index.
- **Why this design.** Directly discharges the discoverability invariant — the operator's stated condition for allowing splits ("a willing agent knows about and can access the broader context").

### 5.5 Splitting tiers (adoption ladder)

- **Tier 0 — Convention (default).** Single `journal.md` (STATE head + append body) and single spec file. Reads are STATE + working set + range-scoped widening. No split. This is the standard.
- **Tier 1 — Enforcement split.** When a checkpoint closes (or a size threshold trips), *seal* completed BODY units into a sibling archive (`journal-archive.md`; optionally a `tasks/` appendix for the spec). STATE's **Archive** pointer names it; INDEX spans both files. Adopt when read-discipline alone proves insufficient and structural enforcement of "history is a separate open" is worth the seal step. Archives use the discoverable sibling naming convention `journal-archive*.md`; sealing and the STATE **Archive**-pointer update happen in one atomic closeout, so a cold reader reaches archived units by glob even under pointer staleness (this discharges the discoverability invariant for sealed layouts — see §5.4).
- **Tier 2 — Contention shard.** One file per BODY unit + a stored manifest, enabling parallel writers. Adopt only if/when parallel dispatch lands (coupled to dispatch OQ-3).
- **Behavior.** Sealing is loss-free and index-preserving: units move verbatim, headings unchanged, STATE updated in the same closeout. Amendment double-recording (a dated pointer entry *plus* a verbatim `### Full record`) collapses to the single amendment form in the reference dialect (§5.7) — keep the full record under one dated `## <date> — Amendment <id>` heading.
- **Why this design.** Ties each structural step to the specific thing it buys, so the suite doesn't pay split ceremony for a token benefit that scoped reading already delivers.
- **Alternatives considered.** Pre-sharding all specs (Tier 2 as default) — maximal optionality but loses single-narrative texture and adds write ceremony for no token gain; rejected per operator preference (splitting reachable, not standard).

### 5.6 Skill integration points

- `spec-execute` Phase 1 ORIENT: replace "read `SPEC_PATH` in full" with the Execute-task working set (§5.3); resolve the Principle-6 self-contradiction ("re-read the relevant section") in favor of scoped reads. Per-boundary re-anchor re-reads STATE + the next task block, not the whole spec.
- `spec-execute` Phase 6 closeout (and `spec-review`/`spec-amend` write phases): update STATE in the same commit as the appended entry.
- `spec-review`, `spec-amend`: already selective; adopt STATE/INDEX vocabulary and the cold-reader guarantee. Reviewer full-diff mandate unchanged.
- `spec-write`, `spec-design`: emit the STATE block when creating `journal.md`; keep the existing token-economy language for codebase reads and extend the same discipline to spec reads.
- **Grammar responsibilities (per §5.7).** `spec-design`/`spec-write` — which already read the constitution during discovery — codify any grammar they observe there into the spec they create. `spec-execute`/`spec-review`/`spec-amend` — which already read the spec — consult the spec's codified grammar and use it, or fall back to their native default. No skill opens a file it would not otherwise read merely to find grammar.

### 5.7 Grammar declaration

- **Purpose.** Let each artifact be *self-describing* about its anchor conventions, so the INDEX (§5.2) resolves exact patterns cheaply — and so *variance is tolerated when declared* rather than forced into uniformity by a tool.
- **Model — one override, variable scope, over a skill default.**
  - **Skill-template default (the go-forward standard).** Each writer skill's prose states the anchor shapes it emits (the reference dialect below). This covers the common case with *zero* declaration: new specs are consistent because the skills emit consistently.
  - **Declared grammar block (the override).** A small block that supersedes the default. It can attach at three scopes; **most-specific-already-read wins**:
    - **Constitution** (repo house style) — read by early writers during discovery.
    - **Spec** (this spec's dialect) — read by every downstream skill.
    - **Legacy artifact itself** (the retrofit record) — the dialect an old file was actually written in.
- **Reference dialect (skill-template default).** The shapes a writer emits absent any override:

  | Element | Canonical anchor |
  |---|---|
  | Journal entry | `## <YYYY-MM-DD> — <event>` |
  | Task closeout | `## <YYYY-MM-DD> — <T-ID>: <title>` |
  | Review | `## <YYYY-MM-DD> — Review of <CP-ID>` |
  | Amendment | `## <YYYY-MM-DD> — Amendment <id>` (single form — retires the 3-variant mess and the double-record) |
  | Spec section | `## N. <title>` |
  | Task block | `### <T-ID> — <title>` **and** the §7 table row |

- **Resolution model (free-rider on normal reads).** Grammar discovery costs *no extra reads* — it rides on files a skill already opens:
  - **Early writers** (`spec-design`, `spec-write`) read the constitution during discovery anyway. If it declares a grammar, they **codify it into the spec** at authoring time. The constitution's dialect is thus snapshotted forward into the spec.
  - **Later skills** (`spec-execute`, `spec-review`, `spec-amend`) read the spec anyway. They use the spec's codified grammar if present, else their native default. They do **not** consult the constitution to see if it disagrees — the spec is authoritative for its own artifacts.
  - **Absent any declaration**, every skill uses its native default. INDEX falls back to discovery (§5.2) for reading undeclared/legacy content.
- **Bootstrap location.** The one anchor that cannot itself be dialect-dependent: a `## Grammar` block adjacent to STATE at the journal head, and a known section in the spec/constitution. A reader looks there first; everything else may vary.
- **Retrofit = record the local dialect (non-destructive).** To make an old artifact cheaply indexable, do not rewrite its body — inspect it once and add a `## Grammar` block declaring the dialect it already uses. The body stays byte-for-byte; the artifact upgrades from discovery-fallback to declared. Opportunistic, never mandated (union discovery already reads legacy correctly).
- **Pattern invoked.** Self-describing data + cascading configuration (most-specific scope wins), analogous to the finding-skills' declared pipeline vocabulary — an in-house precedent for declaring a grammar as data rather than enforcing it with code.
- **Why this design.** Achieves strict go-forward consistency (skill defaults + optional house-style override) and flexible legacy tolerance (per-artifact dialect record + discovery fallback) with **no tooling** — exactly the retrofit-without-over-flexing goal. It also dissolves the "where does a shared grammar file deploy" question: defaults live in each skill (normal deploy-sync); overrides live in the constitution/spec/artifact of the repo being operated on (read in place, never deployed).
- **Alternatives considered.** A single enforced canonical grammar + lint (rejected — tooling, and forces a mass rewrite of legacy). A shared cross-skill grammar support file (rejected — needs a third deploy class; the skill-default + in-repo-override split needs none).

## 6. Non-functional Requirements

- **Adoptability.** Per-skill, incremental; no coordinated cutover. A skill not yet migrated keeps working (reads more). Governed by the skill-change workflow (`spec-amend` against each skill's governing spec).
- **Reversibility.** Revert = restore whole-file reads; STATE becomes a harmless extra block. No data migration.
- **Observability.** Floor/scope compliance stays re-derivable: STATE and INDEX are plain text; a reviewer can reconstruct the index by grep and confirm STATE against the latest entry.
- **Zero-tooling.** INDEX is grep; STATE and grammar are markdown read as data. No generator, no linter, no build step. Enforcement is soft (declared-dialect + discovery), per the governing invariant (§4).
- **Cost.** Target: orientation read for "execute task T" drops from whole-spec+whole-journal (~47k tokens on the measured example) to STATE + one task block + one checkpoint contract (target < ~5k tokens); per-turn re-anchor to STATE-only (target < ~1k tokens).
- **Correctness safety.** No working set may exclude a BODY unit the operation *must* have; widening covers the rest. Under-read degrades to an extra cheap read, never to wrong output.

## 7. Implementation Sequencing (Forward-Looking)

Phases (not atomic tasks). The atomic breakdown belongs to the downstream feature spec named below.

- **P1 — STATE + INDEX + grammar vocabulary.** Define the STATE block shape, two-tier INDEX contracts, and the grammar-declaration model + reference dialect (§5.7) as shared skill vocabulary; wire the `## Grammar` bootstrap location and native defaults into `spec-write`/`spec-design` journal creation, including constitution→spec grammar codification.
- **P2 — `spec-execute` adoption.** Retire the whole-spec read; wire the Execute-task working set, STATE-based re-anchor, and spec-codified-grammar consult (native default otherwise). Biggest single offender (`/spec-execute` ≈ 33% of suite usage).
- **P3 — Cross-skill adoption.** `spec-review`, `spec-amend` adopt vocabulary + cold-reader guarantee + grammar consult; amendment collapses to the single dialect form.
- **P4 — Tier-1 sealing (optional, gated).** Define and dogfood the seal-at-checkpoint step; adopt only if P2/P3 measurement shows read-discipline alone is insufficient.

Each phase produces an artifact the next consumes (vocabulary → skill edits → cross-skill consistency → optional structural layer).

**Downstream feature spec:** `specs/YYYYMMDD-cwsp-skill-integration/feature.md` (to be authored via `spec-write` with this design as `DESIGN_SPEC_PATH`).

## 8. Validation Approach

- **Dogfood on a real spec.** Take a representative multi-task feature (the sibling-repo example measured for §1) and execute one task boundary both ways; measure orientation tokens (whole-file baseline vs. working set) and confirm output parity.
- **Cold-read validation.** A fresh agent, given only STATE + INDEX, must reconstruct current state and reach any historical decision — reusing dispatch-execution's CP-2 cold-read method.
- **Quality regression check.** Compare worker/executor output on the same task under whole-file vs. working-set reads; the design fails if scoped reads degrade output.

## 9. Review Checkpoints

**CP-1 — Design approval.**
- *Trigger.* This spec drafted through §14.
- *Review focus.* Is the STATE/INDEX split correct? Is the discoverability invariant fully discharged by §5.4? Does the working-set table (§5.3) omit any unit an operation genuinely needs?
- *Exit criteria.* Operator approves shape and vocabulary; blockers in §13 resolved or explicitly deferred. Advances status to Approved.
- *Status.* **pass** on 2026-07-07 by waseric (self-review). Verdict: pass with comments — 0 blockers, 1 important, 6 advisory. All remediable findings fixed pre-approval (§5.1 STATE-scope wording, §5.3 NFR symmetry, §5.4/§5.5 OQ-5 promotion, §1/§6 number unification, §5.5 typo; `## Grammar` bootstrap added to journal). Operator approved shape + vocabulary; OQ-1–OQ-4/OQ-6 deferred with owners, OQ-5 resolved into normative design. **CP-1 closed.**

**CP-2 — Pilot validation.**
- *Trigger.* P2 (`spec-execute` adoption) implemented on a branch/deploy pair.
- *Review focus.* Measured orientation-token reduction; output parity vs. baseline; no correctness loss from scoped reads; STATE stays accurate across boundaries.
- *Exit criteria.* Orientation cost meets §6 target; zero quality regression on the dogfood task; cold-read reconstruction succeeds.
- *Status.* **pass with comments** on 2026-07-08 (fable, dispatched spec-reviewer — floor met), executed and recorded in the downstream feature spec's journal (`../20260707-cwsp-skill-integration/journal.md`; feature.md §9 CP-2). 0 blockers, 1 important, 2 advisory. Measured ~8× orientation reduction vs. the whole-file baseline on this corpus (well inside the §6 targets); output parity held; cold-read reconstruction total. **This checkpoint also decided P4:** scoped reads proved sufficient with 2–6× margin, so Tier-1 sealing (P4/T-11) was **not adopted** — gated shut, revisit only if a future corpus breaches the targets. **CP-2 closed.**

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| STATE drifts stale (hand-maintained) | Med | Low | STATE + latest-entry read self-corrects; degrades cost not correctness; update in same closeout commit | executor/worker |
| Under-reading → wrong work | Med | High | Working sets sized to include needed cross-refs; guaranteed cheap widening; reviewer full-diff untouched | P2/CP-2 |
| Grep-INDEX under-counts due to anchor variance (measured, not hypothetical: amendments in 3 heading forms; tasks as heading *or* table) | High | Med | Two-tier INDEX (§5.2): declared dialect (§5.7) where present, broad-union discovery otherwise; §7 table canonical for tasks; go-forward consistency from skill-default dialect; legacy tolerated via discovery or opportunistic dialect record. No lint (soft enforcement, §4 invariant) | P1 |
| Declared grammar drifts from actual content (a writer emits off-dialect) | Low | Low | Declaration is a hint, not truth (§4); INDEX falls back to discovery when declared patterns return implausible results; degrades cost, not correctness | P1 |
| The design is wrong (scoped reads *do* degrade quality) | Low | High | CP-2 quality-regression check on a real task before broad adoption; dispatch CP-2 already evidences the opposite | CP-2 |
| Tier-1 seal loses/garbles an entry | Low | High | Verbatim move, headings unchanged, INDEX-verified post-seal; keep seal opt-in until proven | P4 |
| Adds ceremony without payoff | Low | Med | Tier 0 default (no split); STATE is a few hundred tokens; measure before adopting Tier 1 | CP-2 |

## 11. Adoption Path

A consumer skill adopts CWSP by (1) emitting/maintaining a STATE block in its journal writes, and (2) replacing whole-file orientation reads with the working-set contract for its operation. Adoption is per-skill and independent; unmigrated skills keep functioning (they read more). **Reversibility:** restore whole-file reads; STATE becomes an inert block. **Degradation if partially adopted:** a mix of scoped and whole-file readers is safe — the artifacts are unchanged, so a whole-file reader simply ignores STATE and pays the old cost; a scoped reader that hits an un-updated STATE self-corrects by widening. No lockstep migration, no flag day.

## 12. Out of Scope

- Parallel-writer journaling / Tier-2 sharding (coupled to dispatch OQ-3; §13 OQ-4).
- A stored/generated manifest with per-unit metadata (deferred unless sharding forces it).
- Any change to receipt schema, model floors, or the 80k budget trigger (dispatch-execution owns those).
- Compressing or summarizing BODY content (CWSP scopes *reads*; it does not rewrite the artifacts).
- Applying CWSP to non-spec artifacts beyond noting the memory system as precedent.

## 13. Open Questions

### OQ-1 — STATE maintenance: pure discipline vs. verified-by-check

**Question.** Is the hand-maintained STATE block trusted as written, or is it verified against derivable reality at read time?

**Analysis.** STATE stores non-derivable fields (phase, next, holds) *and* some derivable ones (last completed, latest-entry anchor). The derivable subset could be cross-checked cheaply: `grep '^## '` for the true latest entry, compare to STATE. A mismatch flags staleness. Full trust is cheapest but drift-prone; a check adds one grep per orientation but makes staleness self-announcing.

| Option | Cost | Drift safety |
|---|---|---|
| Trust STATE as written | 0 | relies on closeout discipline |
| Cross-check derivable fields | 1 grep | staleness detected at read time |

**Leaning.** Cross-check the derivable subset (one grep) and treat a mismatch as an automatic widen signal. Cheap insurance that fits the suite's "mechanically re-derivable" doctrine.

**Owner.** P1 (vocabulary definition).

### OQ-2 — Working-set sizing: how much cross-reference to include by default

**Question.** How wide is each default working set (§5.3) before widening is expected — e.g. does "execute task T" include tasks T depends on, or only T?

**Analysis.** Too narrow → frequent widening (cheap but adds round-trips and risks a missed dependency). Too wide → drifts back toward whole-file cost. Dependencies are usually declared in the task block itself, so the block names what to widen to. The measured example suggests one task block + table + checkpoint is sufficient for centered execution.

**Leaning.** Default to the single unit + its declared dependencies-by-reference; let the block's own cross-refs drive widening rather than pre-loading neighbors. Tune at CP-2 from observed widen frequency.

**Owner.** P2 / CP-2.

### OQ-3 — Tier-1 seal trigger: checkpoint close vs. size threshold

**Question.** When does sealing (Tier 1) fire — at checkpoint close, at a token/line threshold, or both?

**Analysis.** Checkpoint close is a natural semantic boundary (completed work becomes history) and aligns with existing review gates. A size threshold catches long checkpoint-free stretches. Both can coexist: seal at checkpoint close OR when the live journal crosses N tokens.

**Leaning.** Checkpoint close as the primary trigger; add a size threshold only if a real spec runs long between checkpoints. Keep Tier 1 opt-in until P2/P3 measurement justifies it.

**Owner.** P4.

### OQ-4 — Coupling to parallel dispatch (dispatch-execution OQ-3)

**Question.** Does CWSP need to pre-commit to a shardable journal layout so parallel workers (if OQ-3 ever lands) can write without contention?

**Analysis.** Tier 2 (one file per BODY unit + stored manifest) is the contention-safe layout. Pre-committing now adds ceremony for a deferred capability; deferring risks a later migration. But Tier 0→2 is a mechanical, index-preserving move, so late adoption is cheap.

**Leaning.** Defer. Tier 2 is reachable and mechanical; do not pay for it until parallel dispatch is real. Note the coupling so whoever revisits OQ-3 sees CWSP Tier 2 as the journaling answer.

**Anti-goals.** Do not split the single-narrative journal *for parallelism* until serial dispatch has proven a full batch at 100% fidelity (mirrors dispatch OQ-3's own anti-goal).

**Owner.** Whoever revisits dispatch OQ-3.

### OQ-5 — Guaranteeing the cold reader without a stored index — RESOLVED (CP-1, 2026-07-07)

**Question.** With INDEX derived (not stored), is a cold reader on a *sealed* (Tier 1/2) spec guaranteed total coverage?

**Resolution.** Promoted into normative design at CP-1. §5.4 and §5.5 now require (a) seal and STATE-update in one atomic closeout, and (b) a discoverable sibling archive naming convention (`journal-archive*.md`) so a cold reader enumerates archives by glob even if STATE's Archive pointer lags. Together these fully discharge the discoverability invariant even under STATE staleness. Any Tier-1 implementation detail remains owned by P4.

### OQ-6 — Grammar evolution after codification

**Question.** A spec snapshots the constitution's grammar at authoring time (§5.7). If the constitution later changes its dialect, an in-flight spec's codified grammar is now stale relative to the repo house style. Is that a problem?

**Analysis.** For the spec's *own* artifacts it is correct behavior — those artifacts were written in the codified dialect, so the spec should remain authoritative for reading them (this is why late skills never re-consult the constitution). The only real question is whether a long-running spec should ever *re-codify* mid-flight. Doing so risks a journal whose early entries use dialect A and later entries dialect B — which discovery handles but which muddies the single declared dialect. A grammar change is rare and, if genuinely needed mid-spec, is a spec change — i.e. `spec-amend` territory, recorded as an amendment, with the mixed-dialect window noted.

**Leaning.** Codified grammar is a point-in-time snapshot and stays fixed for the life of the spec. A mid-flight change routes through `spec-amend` (explicit, journaled) rather than silent re-codification. Discovery covers any resulting mixed-dialect reads.

**Owner.** P3 (cross-skill adoption) / `spec-amend` interaction.

## 14. References

**Authoritative (in-repo, binding).**
- [dispatch-execution architecture](../20260705-dispatch-execution/architecture.md) — execution-topology sibling; source of receipts, model floors, 80k budget, worker scoping.
- [ai-tools CLAUDE.md](../../CLAUDE.md) — suite doctrine (token economy / batch autonomy / rework prevention), deploy-sync rule, skill-change workflow.
- This repo's memory system (`MEMORY.md` index + one-fact shards) — in-house instance of the STATE/INDEX/BODY pattern.
- The finding-* skills' declared pipeline vocabulary (intake → triaged → routed → closed) — in-house precedent for declaring a grammar as data rather than enforcing it with code (§5.7).

**Inspirational (external, non-binding).**
- Denning, P. J., "The Working Set Model for Program Behavior," *Communications of the ACM*, 1968 — working-set / demand-paging model. Verified 2026-07-07: [denninginstitute.com/pjd/PUBS/Workingsets.html](https://www.denninginstitute.com/pjd/PUBS/Workingsets.html).
- Nielsen, J. (Nielsen Norman Group), "Progressive Disclosure," 1995 — show essentials first, reveal on demand. Verified 2026-07-07: [nngroup.com](https://www.nngroup.com/videos/progressive-disclosure/).
