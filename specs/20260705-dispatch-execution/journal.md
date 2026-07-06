# Dispatch Execution — Journal

## 2026-07-05 — Design Spec Authored

**Status:** draft complete — awaiting CP-1
**Artifacts:** `architecture.md` (this directory); `CLAUDE.md` at repo root authored in the same session (repo baseline — first constitution-adjacent context file for this repo).

**Origin.** Operator-initiated, from a working conversation diagnosing batch-session economics: usage panel (2026-07-05) showed 55% of usage at >150k context and `/spec-execute` at 33% of usage. Root cause identified as the execution model — batch continuity carried in session context rather than in artifacts — not skill size (masters measured 150–231 lines). The conversation was treated as authoritative discovery input per `spec-design` Operating Principle 1.

**Discovery notes.**
- Deploy copies at the harness skills directory verified in sync with masters (mechanical diff, this date).
- No agent definitions exist yet — `.agents/agents/` is greenfield; harness agents directory absent.
- Harness facts (Agent tool `model` override matching floor tiers; agent-definition frontmatter; cold-start subagents; final-message-only return) verified against the running harness this date; recorded as version-sensitive (spec §10 R-3, §13 watch items).
- [session-economy](../20260514-session-economy/architecture.md) surfaced as apparent prior art; operator directed it be treated as historical ideas-capture only, not a design input. Reflected in spec §3.

**Phase 2 decisions (operator + unilateral-by-default).**
- Name: `dispatch-execution`; directory `20260705-dispatch-execution`.
- Audience: skill-family maintainers, broadest reader a cold AI agent session.
- AUTONOMY × EXECUTION orthogonal; dispatch defaults on under `checkpoint` (§5.1).
- Receipt schema referenced from consumers, not copied (OQ-1 leaning).
- Context budget 40%-of-window, pilot-tuned (OQ-2).
- `spec-review` dispatch option in scope (§5.8), flagged for possible trim at CP-1.

**Open questions at authoring:** OQ-1 (schema location, operator @ CP-1), OQ-2 (budget constant, P5), OQ-3 (parallel dispatch, deferred past CP-3), OQ-4 (worker tool surface, P3).

**Next:** CP-1 review (spec §9) — operator approval; then P2 (`spec-execute` amendment set via `spec-amend`).

## 2026-07-05 — Draft Revision (operator feedback, pre-CP-1)

**Status:** draft revised — awaiting CP-1
**Changes** (all in `architecture.md`, same spec per operator direction; spec still Draft, so revised directly rather than amended):

- **OQ-1 resolved:** receipt schema lives as a skill support file (`receipt-schema.md` in the skill directory, deployed alongside `SKILL.md`), referenced by the skill body and the agent definition; consumers copy nothing. Decision moved to §5.4 (Location); OQ-1 stubbed for numbering stability.
- **Consumer references genericized:** this repo serves work and hobby consumers alike; no consuming project is named or assumed anywhere in the spec (§2, §3, §5.2, §7 P5, §11, §12, §14). Repo `CLAUDE.md` updated with the same rule as a standing convention.
- **Defaults flipped to the target posture:** default-autonomous (`AUTONOMY: checkpoint`) + default-dispatch (`EXECUTION: dispatch`), per the operator's guiding principle — escalate only at planned pauses, interrupts, or unexpected scenarios. The prior "checkpoint is never self-granted" rule restated as "the agent may never loosen the stop set"; operator restricts rather than grants (§1, §2, §5.1; CP-1 review focus now includes stop-set sufficiency).
- **Phase 8 budget:** 40%-of-window replaced by fixed **80,000 tokens** — tokens are the pricing fundamental, holding cost exposure constant across models; amend only if it becomes limiting (§5.6, OQ-2).
- **New §5.9 — operator cues:** fixed-format WHAT HAPPENED / YOUR MOVE / HOW blocks at every human-in-the-loop boundary, with pre-filled next invocations; motivated by operator context-switching cost under default autonomy. NFR added (§6 Resumability); P2/P4 scope extended to carry cue emission into both skills.

**Consistency check (re-runnable):** `grep -rn "admindoc\|hungergames\|T-14\|40%" specs/20260705-dispatch-execution/architecture.md CLAUDE.md` returns zero hits (verified this date). Scope is the spec document and repo context file; this journal legitimately retains "40%" in its historical record of the decision change.

**Next:** CP-1 review against the updated §9 focus list.

## 2026-07-05 — Draft Revision 2 (CP-1 review findings, pre-approval)

**Status:** draft revised — awaiting operator approval
**Reviewer:** Claude (AI reviewer), CP-1 review conducted per `/spec-review`, verdict: pass with comments (0 blockers, 2 important, 1 advisory). Full verdict below.

**Changes** (all in `architecture.md`, same spec per operator direction; spec still Draft, so revised directly rather than amended):

- **Doctrine citation fixed (§3, §4, §9):** "verification wins ties" and doctrine rule "derived claims over narrative claims" were presented as inherited constitution phrases but did not appear anywhere in `CLAUDE.md` or any governing spec — grepped and confirmed absent. Replaced with direct citation of `CLAUDE.md`'s actual rework-prevention property ("mechanically re-derivable claims") at all three sites (§3 doctrine list, §4 Derivation re-check vocabulary entry, §9 CP-1 review-focus line).
- **Cue coverage closed (§5.9):** the designed-stop set named in §2 Goals lists six stop types (checkpoint, blocker, amendment trigger, floor conflict, production-touching action, budget breach); §5.9's enumerated cue list covered only five, omitting floor conflict and production-touching-action stops. Added explicit cue treatment for both.

**Deferred (advisory, not fixed here):** pre-existing drift where `AUTONOMY` (`SKILL.md:26`, commit `6bef6f8`, 2026-07-03) is undocumented in the `spec-execute` governing `architecture.md` (2026-05-18, predates that commit). Not caused by this spec; flagged for P2 to backfill alongside the `EXECUTION`-mode amendment rather than fixed here.

**Consistency check (re-runnable):** `grep -rn "admindoc\|hungergames\|T-14\|40%" specs/20260705-dispatch-execution/architecture.md CLAUDE.md` returns zero hits (re-verified this date, post-edit).

**Next:** Operator approval of CP-1 (banner advance to Approved pending explicit confirmation); then P2 (`spec-execute` amendment set via `spec-amend`).

## 2026-07-05 — Review of CP-1

**Reviewer:** Claude (AI reviewer)
**Outcome:** pass with comments
**Tasks reviewed:** N/A — design-spec checkpoint (whole document; no Task Breakdown)
**Blockers:** 0
**Important:** 2 (both fixed in Draft Revision 2 above — doctrine-citation fidelity; §5.9 cue coverage)
**Advisory:** 1 (pre-existing `AUTONOMY` documentation gap in `spec-execute` governing spec — not caused by this spec, deferred to P2)
**Spec amendments proposed:** None — spec was still Draft, so findings were fixed by direct revision rather than routed through `spec-amend`.
**Next action:** Operator approved 2026-07-05. Banner advanced to Approved; CP-1 status line recorded in §9. Proceed to P2 — `spec-execute` amendment set via `spec-amend` (governing spec §4, §5.4–§5.8, master, deploy copy; fold in the `AUTONOMY` backfill noted above).

## 2026-07-05 — P2.1: EXECUTION mode + AUTONOMY backfill (governing-spec amendment + master)

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: dispatch` requested but not yet built, so this session runs **inline** (operator's explicit fallback). P2 bootstraps dispatch inline by necessity.
**Commits:** `630fc5a` (governing-spec amendment 2026-07-05-1 + its journal entry), `87ea98a` (SHA backfill), `d3c64c2` (master EXECUTION input + deploy sync).
**Executed by:** inline
**Files touched:** `specs/20260518-spec-execute-skill/architecture.md` (§4 new "Execution modes and autonomy" subsection), `specs/20260518-spec-execute-skill/journal.md` (amendment 2026-07-05-1 record), `.agents/skills/spec-execute/SKILL.md` (INPUTS: new EXECUTION entry), deploy copy `~/.claude/skills/spec-execute/SKILL.md` (synced).
**DoD verification:**
- Governing-spec change routed through `spec-amend` (diff shown, first-class amendment 2026-07-05-1 journaled): commit `630fc5a`.
- Master edited; master ≡ deploy: `diff -q` clean ("MASTER==DEPLOY OK"), commit `d3c64c2`.
- Vocabulary agreement (dispatch spec ↔ governing spec ↔ master): grep confirms `EXECUTION`/`inline`/`dispatch` present in all three; "never loosen the stop set" present in governing spec + master.
- Closeout journaled here with `Executed by: inline`.
**Models used:** dispatch-execution spec declares no per-task Model floors (design spec, Implementation Sequencing not atomic Task Breakdown); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **P2 decomposed into sub-steps** (operator choice): P2.1 EXECUTION mode + AUTONOMY backfill; P2.2 orchestrator/worker phase split + conduct + worker brief (§5.2–5.3); P2.3 receipt-schema support file + `Executed by` journal field (§5.4–5.5); P2.4 Phase 8 context budget (§5.6); P2.5 operator cues (§5.9); **P2.6 atomic defaults flip** (see below). §5.7 agent definitions = P3; §5.8 spec-review = P4 — not in P2.
- **Defaults flip deferred to a final atomic micro-step (P2.6), not P2.1.** Flipping the master default to `dispatch` before dispatch conduct exists would ship a broken default; flipping only `AUTONOMY`→checkpoint while `EXECUTION` stays inline yields the mega-session combination the design fights (dispatch-execution §5.1). So the master keeps `EXECUTION: inline` / `AUTONOMY: task` defaults until all conduct lands, then flips both atomically. The governing spec already describes the *target* defaults as the design authority; master reconciles by P2 end (dispatch-execution CP-2 verifies).
- **AUTONOMY backfill scope:** the gap was in the *governing spec* only (the master already documented AUTONOMY since `6bef6f8`). Backfill landed in the governing-spec amendment.
- **Amendment model:** dispatch-execution is treated as a sibling design-spec authority for dispatch behavior, mirroring how session-economy is authoritative for Phase 8; the governing spec describes the modes and cites dispatch-execution.
**Spec amendments:** [governing spec amendment 2026-07-05-1](../20260518-spec-execute-skill/journal.md) — §4 "Execution modes and autonomy" subsection added.
**Surprises and learnings:** The `git commit --amend` used to backfill the amendment SHA rewrote the SHA (2f4aca6→630fc5a), so backfill must be a *separate follow-up commit* (`87ea98a`), never `--amend`. Matches the cross-skill-case guidance in spec-amend ("backfill is a small follow-up commit").
**Next task pointer:** P2.2 — orchestrator/worker phase split + conduct rules + worker brief (dispatch-execution §5.2, §5.3), against the [spec-execute governing spec](../20260518-spec-execute-skill/architecture.md) §4/§5 and the master.

## 2026-07-05 — P2.2: orchestrator/worker phase split + conduct rules + worker brief (governing-spec amendment + master)

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: inline` (dispatch not yet built — P2 bootstraps it inline by necessity). Operator approved the concrete P2.2 diffs in-session before apply (spec amendment = designed stop under checkpoint).
**Commits:** `ffe7206` (governing-spec §4/§5.12 application), `54ac7b2` (governing-spec amendment 2026-07-05-2 journal record), `44c61b7` (master DISPATCH MODE + Phase 2/4/5/6 notes + WHAT-NOT-TO-DO bullets, deploy synced). This closeout entry commits separately.
**Executed by:** inline
**Files touched:** `specs/20260518-spec-execute-skill/architecture.md` (§4 index sentence + new §5.12), `specs/20260518-spec-execute-skill/journal.md` (amendment 2026-07-05-2 record), `.agents/skills/spec-execute/SKILL.md` (DISPATCH MODE section + 4 phase notes + 2 WHAT-NOT-TO-DO bullets), deploy copy `~/.claude/skills/spec-execute/SKILL.md` (synced).
**DoD verification:**
- Governing-spec change routed through `spec-amend` (diff shown + operator-approved in-session; first-class amendment 2026-07-05-2 journaled): commits `ffe7206` + `54ac7b2`.
- Master edited; master ≡ deploy: `diff -q` clean ("MASTER==DEPLOY OK") post-commit, commit `44c61b7`.
- Composition rule / orchestrator conduct / worker brief present in master (DISPATCH MODE section) and described in governing §5.12: grep confirms both.
- Vocabulary agreement (dispatch spec ↔ governing spec ↔ master): `orchestrator`/`worker`/`receipt` present in all three (governing 8/7/5, master 16/11/12, dispatch spec 28/40/32).
- Scope boundary held: no `receipt-schema.md` created (that is P2.3); `Executed by` field, context budget (P2.4), cues (P2.5), agent definitions (P3), and the defaults flip (P2.6) untouched.
**Models used:** dispatch-execution declares no per-task Model floors (design spec); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **Governing-spec home = new §5.12** (a peer to §5.11), descriptive of the master's DISPATCH MODE section, rather than expanding each phase subsection. Keeps the eight per-phase subsections intact and groups dispatch as one cohesive block, mirroring how the dispatch spec itself groups §4/§5.2/§5.3.
- **Master home = new top-level `# DISPATCH MODE` section + short per-phase "Dispatch mode." notes** on Phases 2/4/5/6, matching the INPUTS promise ("dispatch-mode notes on Phases 2 and 4–6"). Phase 5/6 notes added too (worker owns 4–6 as a unit) — a minor, faithful superset of the INPUTS wording.
- **Forward references kept, flagged honest:** §5.12 and the master reference the receipt-schema support file (P2.3), `Executed by` (P2.3), context budget (P2.4), cues (P2.5), and the worker agent definition (P3). Dispatch is not *usable* until P2.6 + P3 regardless; the refs mark the amendment set's remaining steps, not drift.
- **Commit shape (no self-referential backfill):** committed the governing-spec application (`ffe7206`) first, then the journal entry referencing that SHA (`54ac7b2`) — avoids the P2.1 `--amend`-rewrites-SHA trap by never having the journal reference its own commit.
**Spec amendments:** [governing spec amendment 2026-07-05-2](../20260518-spec-execute-skill/journal.md) — §4 index sentence + new §5.12 "Dispatch mode".
**Surprises and learnings:** Invoking `spec-amend` and `spec-execute` as in-session Skills reset the Edit tool's "file has been read" tracking; had to re-Read anchor regions before editing. Cheap, but worth knowing: a skill launch is a read-state boundary. The clean 2-commit ordering (spec-edit → journal-referencing-it) is simpler than P2.1's commit-then-backfill and is the recommended pattern for future single-artifact amendments here.
**Next task pointer:** P2.3 — receipt-schema support file (`.agents/skills/spec-execute/receipt-schema.md`, dispatch-execution §5.4) + `Executed by` journal field (§5.5); wire the master's Phase 6 journal template and the governing spec to reference both.

## 2026-07-05 — P2.3: receipt-schema support file + `Executed by` journal field (support file + master + governing-spec amendment)

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: inline` (dispatch not yet built — P2 bootstraps it inline by necessity). Operator approved the concrete P2.3 diffs in-session before apply (amendment = designed stop under checkpoint).
**Commits:** `25c5ffb` (governing-spec §5.12 amendment application), `29939e5` (governing-spec amendment 2026-07-05-3 journal record, refs 25c5ffb), `fb04837` (new `receipt-schema.md` support file + master Phase 6 `Executed by` line + DISPATCH MODE receipt-file references, deploy synced). This closeout entry commits separately.
**Executed by:** inline
**Files touched:** `.agents/skills/spec-execute/receipt-schema.md` (new support file), `.agents/skills/spec-execute/SKILL.md` (Phase 6 template `Executed by` line + 2 DISPATCH MODE receipt-file references), deploy copies `~/.claude/skills/spec-execute/{SKILL.md,receipt-schema.md}` (synced), `specs/20260518-spec-execute-skill/architecture.md` (§5.12 Reversibility + Cross-reference stubs resolved), `specs/20260518-spec-execute-skill/journal.md` (amendment 2026-07-05-3 record).
**Tests added:** n/a (doc/skill artifacts — verification is mechanical grep + diff).
**DoD verification:**
- Receipt-schema support file authored at the OQ-1-resolved path, faithful to dispatch-execution §5.4 (8 fields in order, 25-line cap, commands-as-evidence rule, stop conditions): `grep -n "Hard cap" .agents/skills/spec-execute/receipt-schema.md` → line 14.
- `Executed by` field added to master Phase 6 journal template (dispatch-execution §5.5 shape): `grep -n "Executed by" .agents/skills/spec-execute/SKILL.md` → line 141.
- Master references the now-existing support file (no "added in a later step" language): `grep -n "receipt-schema.md" .agents/skills/spec-execute/SKILL.md` → lines 212, 240.
- Governing-spec §5.12 forward-reference stubs resolved: `grep -n "added in a later step" specs/20260518-spec-execute-skill/architecture.md` → no stale stubs.
- Governing-spec change routed through `spec-amend` (first-class amendment 2026-07-05-3 journaled): commits `25c5ffb` + `29939e5`.
- Master ≡ deploy (both files): `diff -q` clean for `SKILL.md` and `receipt-schema.md` ("MASTER==DEPLOY OK").
**Models used:** dispatch-execution declares no per-task Model floors (design spec); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **Governing §5.6 left untouched.** Its Phase 6 field list (line 237) is explicitly illustrative and defers to SKILL.md for the authoritative format ("matching the format declared in SKILL.md"); it already omits `Models used`. Adding only `Executed by` there would be inconsistent, and the master is the authoritative template — so the field landed in the master template and the §5.12 forward-reference stubs were resolved instead. Surfaced to the operator in Phase 3; approved.
- **Support-file scope = normative schema only.** `receipt-schema.md` carries the schema shape, the 25-line cap, the evidence/stop rules, and the "why capped" rationale — a faithful reproduction of dispatch-execution §5.4, self-contained so both the orchestrator (via the skill) and the worker (via the P3 `spec-worker` agent definition) load one normative copy. No consumer-doctrine copy (OQ-1 resolution).
- **Commit shape mirrors P2.2** (spec-edit → journal-referencing-it → master+deploy → closeout): avoids the P2.1 `--amend`-rewrites-SHA trap by never having a journal reference its own commit.
**Spec amendments:** [governing spec amendment 2026-07-05-3](../20260518-spec-execute-skill/journal.md) — §5.12 Reversibility + Cross-reference forward-reference stubs resolved to the shipped support file and template field.
**Surprises and learnings:** The `Executed by` field this step added is the same field this very journal entry uses — the amendment set became self-hosting at P2.3. Deploy sync now covers *two* files in the skill directory (`SKILL.md` + `receipt-schema.md`); the deploy-sync check must diff both, not just the master, going forward (relevant to P3 when the agent-definitions class adds a third deploy target class).
**Next task pointer:** P2.4 — Phase 8 context budget (fixed 80,000-token trigger; dispatch-execution §5.6): wire the master's Phase 8 and the governing spec's §5.8. Note the still-open remaining P2 steps: P2.5 operator cues (§5.9), P2.6 atomic defaults flip; then P3 agent definitions (`spec-worker`/`spec-reviewer`), P4 spec-review.

## 2026-07-05 — P2.4: Phase 8 context budget (fixed 80,000-token trigger) (governing-spec amendment + master)

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: inline` (dispatch not yet built — P2 bootstraps it inline by necessity). Operator approved the concrete P2.4 diffs in-session before apply (amendment = designed stop under checkpoint).
**Commits:** `f677c90` (governing-spec §5.8/§6/§5.12 amendment application), `4ed745b` (governing-spec amendment 2026-07-05-4 journal record, refs f677c90), `234bd94` (master SKILL.md Phase 8 budget block + rubric row + always-stops override line, deploy synced). This closeout entry commits separately.
**Executed by:** inline
**Files touched:** `.agents/skills/spec-execute/SKILL.md` (Phase 8 "Context budget" block + rubric row + always-stops override sentence), deploy copy `~/.claude/skills/spec-execute/SKILL.md` (synced), `specs/20260518-spec-execute-skill/architecture.md` (§5.8 Context budget paragraph + Override/Pattern updates, §6 Token-economy NFR row, §5.12 Cross-reference stub resolved), `specs/20260518-spec-execute-skill/journal.md` (amendment 2026-07-05-4 record).
**Tests added:** n/a (doc/skill artifacts — verification is mechanical grep + diff).
**DoD verification:**
- 80k budget landed in master Phase 8 (dispatch-execution §5.6 faithful: fixed 80k, inline forced fresh-session, checkpoint-breach stop, dispatch-moot-but-conduct-signal, fixed-tokens rationale): `grep -n "80,000 tokens\|Context budget\|context-budget breach" .agents/skills/spec-execute/SKILL.md` → lines 180, 191, 209.
- Budget wired into governing spec §5.8 (Context budget paragraph + Override + Pattern all cite dispatch-execution §5.6): `grep -n "Context budget" specs/20260518-spec-execute-skill/architecture.md` → line 268.
- §6 Token-economy NFR row updated with the mechanical-trigger clause + dispatch-execution §5.6 source: line 352.
- §5.12 Cross-reference forward stub resolved (no stale "land in their own steps" for the Phase 8 budget): `grep -n "land in their own steps of the P2"` → none.
- Governing-spec change routed through `spec-amend` (first-class amendment 2026-07-05-4 journaled): commits `f677c90` + `4ed745b`.
- Master ≡ deploy (both files): `diff -q` clean for `SKILL.md` and `receipt-schema.md` ("MASTER==DEPLOY OK").
**Models used:** dispatch-execution declares no per-task Model floors (design spec); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **Two consequential edits beyond the bare §5.8 + master minimum, operator-approved in Phase 3:** (a) resolved the §5.12 Cross-reference forward stub — P2.4 is the step it pointed at, mirroring how P2.3 (amendment -3) resolved its own §5.12 stubs; (b) updated the §6 Token-economy NFR row so the NFR table stays faithful to the now-mechanical trigger. Both are within-amendment cross-reference follow-ups under one amendment ID, not separate amendments.
- **Governing §5.8 Override paragraph strengthened** to state a context-budget breach is not subject to the "run the full set without checking in" override — the governing-spec analog of the master's always-stops sentence, keeping the two in parity.
- **Commit shape mirrors P2.2/P2.3** (spec-edit → journal-referencing-it → master+deploy → closeout): avoids the P2.1 `--amend`-rewrites-SHA trap by never having a journal reference its own commit.
**Spec amendments:** [governing spec amendment 2026-07-05-4](../20260518-spec-execute-skill/journal.md) — §5.8 Context budget + §6 NFR + §5.12 Cross-reference stub resolution.
**Surprises and learnings:** None new — the "skill launch resets Edit read-state" boundary (first recorded at P2.2) recurred when `spec-amend` launched mid-task; re-Read the three governing-spec anchor regions before editing, as expected. Remaining P2 forward references now number two (operator cues §5.9 = P2.5; worker agent definition = P3); the defaults flip (P2.6) stays last.
**Next task pointer:** P2.5 — operator cues at human-in-the-loop boundaries (dispatch-execution §5.9): fixed-format WHAT HAPPENED / YOUR MOVE / HOW blocks at every operator-facing stop; wire the master (new cue section + per-boundary emission) and the governing spec (§4 execution-modes note already forward-references cues; add the governing §5.x home). Then P2.6 atomic defaults flip; P3 agent definitions; P4 spec-review.

## 2026-07-05 — P2.5: operator cues at human-in-the-loop boundaries (governing-spec amendment + master)

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: inline` (dispatch not yet built — P2 bootstraps it inline by necessity). Operator approved the concrete P2.5 diffs in-session before apply (amendment = designed stop under checkpoint).
**Commits:** `6de1ffe` (governing-spec §5.13/§6/§4/§5.12 amendment application), `b984f3b` (governing-spec amendment 2026-07-05-5 journal record, refs 6de1ffe), `2b360ca` (master SKILL.md OPERATOR CUES section + Phase 7/Phase 8/AMENDMENT PROTOCOL pointer notes, deploy synced). This closeout entry commits separately.
**Executed by:** inline
**Files touched:** `.agents/skills/spec-execute/SKILL.md` (new `# OPERATOR CUES` section + 3 pointer notes), deploy copy `~/.claude/skills/spec-execute/SKILL.md` (synced), `specs/20260518-spec-execute-skill/architecture.md` (new §5.13, §6 Resumability-human NFR row, §4 §5.13 pointer, §5.12 Cross-reference stub resolved), `specs/20260518-spec-execute-skill/journal.md` (amendment 2026-07-05-5 record).
**Tests added:** n/a (doc/skill artifacts — verification is mechanical grep + diff).
**DoD verification:**
- `# OPERATOR CUES` section shipped in master (fixed-format block + full 7-boundary emission list + "emitted at boundaries, not stored in artifacts" rule, both modes; faithful to dispatch-execution §5.9): `grep -n "^# OPERATOR CUES"` → line 215; pointer notes at Phase 7 (165), Phase 8 (213), AMENDMENT PROTOCOL (306).
- Governing spec §5.13 authored (Purpose/Behavior/Why/Reversibility, cites dispatch-execution §5.9): `grep -n "### 5.13 Operator cues"` → line 341.
- §6 *Resumability (human)* NFR row added: line 364.
- §4 dispatch-mechanics sentence references §5.13: line 155.
- §5.12 Cross-reference operator-cues forward stub resolved (no stale "land in their own step"): `grep -n "land in their own step of the P2"` → none.
- Governing-spec change routed through `spec-amend` (first-class amendment 2026-07-05-5 journaled): commits `6de1ffe` + journal record.
- Master ≡ deploy (both files): `diff -q` clean for `SKILL.md` and `receipt-schema.md` ("MASTER==DEPLOY OK").
**Models used:** dispatch-execution declares no per-task Model floors (design spec); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **Cues are cross-cutting, not dispatch-specific.** Both the master `# OPERATOR CUES` section and the governing §5.13 are standalone (not nested under dispatch mode), because dispatch-execution §5.9 emits cues at every operator-facing boundary in *both* inline and dispatch modes. Surfaced in Phase 3; operator-approved.
- **Master pointer-note placement (surgical).** One-line `Operator cue.` notes at the three phases that most commonly hand control to the operator (Phase 7 checkpoint, Phase 8 pause, AMENDMENT PROTOCOL); the remaining boundaries (blocker, floor conflict, production action, budget breach) are enumerated inside the OPERATOR CUES section rather than each getting a phase note. Mirrors the P2.2 DISPATCH MODE section-plus-per-phase-notes precedent.
- **§5.12 stub resolved in-amendment** (mirrors P2.4): P2.5 is the step the operator-cues forward reference pointed at, so the stub resolves to §5.13 + the shipped section as a cross-reference follow-up under this amendment ID, not a separate amendment.
- **Commit shape mirrors P2.2–P2.4** (spec-edit → journal-referencing-it → master+deploy → closeout).
**Spec amendments:** [governing spec amendment 2026-07-05-5](../20260518-spec-execute-skill/journal.md) — new §5.13 + §6 Resumability-human NFR + §4 pointer + §5.12 Cross-reference stub resolution.
**Surprises and learnings:** With P2.5 landed, the governing spec's §5.12 Cross-reference paragraph now has **no remaining forward stubs** — the only open forward reference across the P2 amendment set is the worker agent definition (P3, §5.7 of the dispatch spec / `spec-worker.md`). Remaining P2 work is P2.6 alone (the atomic defaults flip); all descriptive conduct for dispatch is now in place, so P2.6 is a pure defaults change gated on P3 (flipping `EXECUTION: dispatch` before `spec-worker` exists would ship a default that cannot spawn its worker — see P2.1's deferral rationale).
**Next task pointer:** P2.6 — atomic defaults flip (master INPUTS: `EXECUTION` default → `dispatch`, `AUTONOMY` default → `checkpoint`), dispatch-execution §5.1. **Gating note:** P2.6 should not land until P3 (agent definitions — `spec-worker`/`spec-reviewer`) exists, per the P2.1 decision that flipping the default to `dispatch` before a spawnable worker exists ships a broken default. Recommend sequencing P3 before P2.6, or confirming inline-fallback tolerance with the operator. Then P4 spec-review (§5.8).

## 2026-07-06 — P3: agent definitions (`spec-worker` + `spec-reviewer`) + deploy-sync doctrine

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: inline` (dispatch not yet built — P3 is what builds the workers dispatch will spawn). Direct artifact authoring of a new deliverable class (no governing spec exists for agent definitions; dispatch-execution §5.7 is the authority) plus a `CLAUDE.md` doctrine edit — neither is a `spec-amend` against an existing skill's governing spec, so no amendment stop applied.
**Commits:** `174d908` (agent definitions + deploy-sync doctrine), `929c9f4` (dispatch-spec §13 OQ-4 resolution). This closeout entry commits separately.
**Executed by:** inline
**Files touched:** `.agents/agents/spec-worker.md` (new), `.agents/agents/spec-reviewer.md` (new), deploy copies `~/.claude/agents/{spec-worker,spec-reviewer}.md` (new, synced), `CLAUDE.md` (Orientation + deploy-sync rule + commit-prefix convention extended to the agent-definitions class), `specs/20260705-dispatch-execution/architecture.md` (§13 OQ-4 resolved).
**Tests added:** n/a (agent-definition/doc artifacts — verification is mechanical grep + diff + a deferred cold-read test).
**DoD verification** (§7 "Produces: spawnable worker/reviewer types"; §8 validation):
- `spec-worker.md` authored under `.agents/agents/`, faithful to §5.7: Phases 4–6 contract, orient-from-disk discipline, `receipt-schema.md` reference, `Executed by: worker(spec-worker, <model>)` line, and both prohibitions (no `spec-*` skills; no subagent spawning) — grep-verified (7 checks green).
- `spec-reviewer.md` authored, faithful to §5.8: full diff-reading mandate, no-fix, coordinator-writes-back, spec-review Phase 7 verdict format + outcome vocabulary — grep-verified (4 checks green).
- Frontmatter valid per the harness format (verified against the Claude Code subagents doc this session): `name`/`description` present; `model` deliberately omitted (defaults to `inherit`, per-spawn override sets the floor — §5.7); `spec-worker` → `disallowedTools: Agent` (OQ-4 leaning); `spec-reviewer` → `disallowedTools: Agent, Write, Edit`.
- Deployed to `~/.claude/agents/`; `diff -q` clean for both ("MASTER==DEPLOY OK").
- `CLAUDE.md` deploy-sync rule now spans two deliverable classes (skills + agent definitions), with the agent-definitions master→deploy path, the harness-adapter frontmatter exception, and the `<agent-name>:` commit prefix — grep-verified (4 checks green).
- No wikilinks introduced (repo convention) — grep clean.
**Models used:** dispatch-execution declares no per-task Model floors (design spec); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **P3 kept as one journal entry, not sub-stepped.** Unlike P2 (five governing-spec amendments), P3 is one coherent greenfield deliverable — two definitions + the doctrine that governs their class — landing together as one reviewable unit. Sub-steps would fragment a single logical closeout.
- **Reviewer is read-only; worker mutates.** OQ-4's "full surface minus Agent" is scoped to the *worker* (it implements + commits). The reviewer only reads, analyzes, and returns a verdict — the coordinator writes back (§5.8) — so it gets `disallowedTools: Agent, Write, Edit`, which turns spec-review's "you review, you don't fix" from prose into a tool-level guarantee. Differentiating the two definitions is deliberate, not inconsistency.
- **Model unpinned in frontmatter.** Per §5.7, model is a spawn-time parameter set to the task floor. The harness resolution order (per-invocation `model` param > frontmatter > inherit) means omitting `model` lets the orchestrator pin the floor per spawn without N per-tier definitions.
- **Receipt schema referenced, not embedded.** `spec-worker.md` points to `receipt-schema.md` (the skill support file, single normative copy) rather than restating the 8 fields — same single-source discipline P2.3 established.
- **Journal template referenced, not embedded.** The worker matches spec-execute's Phase 6 template via the existing journal entries it reads at orientation + its brief, rather than the definition carrying a second copy of the template (drift surface). Only the dispatch-specific `Executed by` line is called out explicitly.
- **CLAUDE.md doctrine bundled with the class it governs.** The deploy-sync extension landed in the same commit as the definitions it applies to (§5.7 mandates them together); the harness-adapter frontmatter exception was added so a future contributor does not "fix" the intentional `tools`/`disallowedTools` keys as a portability violation.
- **Harness format verified live, not from memory.** Fetched the Claude Code subagents doc this session to confirm field names (`tools`/`disallowedTools`/`model`/`effort`), the omit-`model`-defaults-to-`inherit` behavior, and the per-invocation override resolution order — these are the §10 R-3 version-sensitive facts, re-confirmed 2026-07-06.
**Spec amendments:** none (design-spec status tracked in this journal, not in §7; §13 OQ-4 updated as a Phase-6 decision-recording per the OQ-1 precedent, not a design amendment).
**Surprises and learnings:**
- The deploy-target directory `~/.claude/agents/` did not exist and had to be created; per the subagents doc, a session that starts before an `agents/` directory exists will not detect newly added definitions until restart — so these two agents become spawnable only in a **fresh** session (relevant when P4/pilot first tries to spawn one).
- OQ-4 resolved in *authoring* but its **cold-read validation** (§8) is a CP-2 exit criterion still pending — CP-2 does not trigger until P2–P4 land, so the cold-read test is correctly deferred, not skipped.
- Deploy-sync now spans two directory classes (`~/.claude/skills/` and `~/.claude/agents/`); the CLAUDE.md rule was updated to say so explicitly, closing the gap the P2.3 "diff both files" learning first opened.
**Next task pointer:** Two P-steps remain, order-flexible now that P3 exists: **P4** — `spec-review` dispatch option (`REVIEW_EXECUTION` input + reviewer-boundary cues, §5.8/§5.9) against the [spec-review governing spec](../20260518-spec-review-skill/architecture.md); and **P2.6** — the atomic defaults flip (master INPUTS `EXECUTION`→`dispatch`, `AUTONOMY`→`checkpoint`, §5.1), now un-gated since a spawnable worker exists. Recommend P4 before P2.6 so the whole dispatch surface (execute + review) is in place before defaults flip; then **CP-2** (amendment-set consistency: master≡deploy sweep, cold-read test, no skill gained harness frontmatter) triggers once P2–P4 have all landed.

## 2026-07-06 — P4: `spec-review` dispatch option (`REVIEW_EXECUTION` + verdict cues) (governing-spec amendment + master)

**Status:** done
**Operating mode (operator-granted this session):** `AUTONOMY: checkpoint`; `EXECUTION: inline` (dispatch not yet built — this whole amendment set bootstraps dispatch inline by necessity). Operator approved the concrete P4 diffs in-session before apply (spec amendment = designed stop under checkpoint); `lastUpdated` bump confirmed via AskUserQuestion.
**Commits:** `93c9ca0` (governing-spec §4/§5.12/§5.13/§6 application), `e872f62` (spec-review journal amendment 2026-07-06-1 record, refs 93c9ca0), `88357b2` (master SKILL.md REVIEW_EXECUTION input + DISPATCH MODE + OPERATOR CUES + WHAT-NOT-TO-DO bullet + lastUpdated bump, deploy synced). This closeout entry commits separately.
**Executed by:** inline
**Files touched:** `specs/20260518-spec-review-skill/architecture.md` (§4 execution-model pointer, new §5.12 Dispatch option + §5.13 Operator cues, two §6 NFR rows), `specs/20260518-spec-review-skill/journal.md` (amendment 2026-07-06-1 record), `.agents/skills/spec-review/SKILL.md` (INPUTS REVIEW_EXECUTION entry, DISPATCH MODE + OPERATOR CUES sections, 1 WHAT-NOT-TO-DO bullet, lastUpdated 2026-05-15→2026-07-06), deploy copy `~/.claude/skills/spec-review/SKILL.md` (synced).
**Tests added:** n/a (doc/skill artifacts — verification is mechanical grep + diff).
**DoD verification** (§7 P4 "per §5.8 and §5.9, against the spec-review governing spec"):
- `REVIEW_EXECUTION: inline | dispatch` input (default inline) added to master INPUTS, faithful to §5.8: `grep -n "REVIEW_EXECUTION:" .agents/skills/spec-review/SKILL.md` → line 25.
- Master `DISPATCH MODE` (coordinator/reviewer phase split, full-diff mandate, reviewer floor, reviewer brief, write-back) + `OPERATOR CUES` (verdict-boundary cue block, per-outcome + amendment + floor-conflict cues) sections shipped: `grep -n "^## DISPATCH MODE\|^## OPERATOR CUES"` → lines 185, 201; WHAT-NOT-TO-DO coordinator-drift bullet present (1 hit).
- Governing spec §5.12 (dispatch option) + §5.13 (operator cues) authored, citing dispatch-execution §5.8/§5.9 as sibling authority; §4 execution-model pointer + two §6 NFR rows added: `grep -n "^### 5.12\|^### 5.13\|Dispatch isolation\|Resumability (human)"` → lines 267, 279, 299, 300.
- Vocabulary agreement (dispatch spec ↔ governing spec ↔ master): `REVIEW_EXECUTION` present in all three (master 4, governing 5, dispatch spec 1); `spec-reviewer` present in all three (master 3, governing 3, dispatch spec 4).
- Governing-spec change routed as first-class amendment 2026-07-06-1 (structured record in spec-review journal, refs 93c9ca0); this dispatch-execution P4 entry is the driving-spec primary record cross-referenced from there.
- Master ≡ deploy: `diff -q` clean ("MASTER==DEPLOY OK").
- No wikilinks introduced; no new `~/.claude` paths in committed prose (the three §5.11/CP-1 hits are the pre-existing portability rule quoting the path as the thing to avoid).
**Models used:** dispatch-execution declares no per-task Model floors (design spec); no floor gating applies. Work done by this session's model (Opus 4.8).
**Decisions made:**
- **P4 as one amendment, not sub-stepped** (mirrors P3, unlike P2's five). The dispatch option and its verdict cues are one coherent deliverable landing together; sub-steps would fragment a single reviewable unit.
- **Two governing-spec subsections (§5.12 dispatch, §5.13 cues)** mirroring how P2 structured the spec-execute governing spec (§5.12/§5.13), for cross-skill parity.
- **Forward-addition framing, not retroactive description.** The spec-review governing spec is a retroactive descriptive spec (Non-goal: no redesign). P4 adds *new* behavior sourced from dispatch-execution, so the amendment record is explicit that dispatch-execution is a sibling design authority for dispatch behavior — cited the way session-economy §5.4 is authoritative for the multi-repo Phase 8 paragraph — rather than describing already-shipped behavior. Banner kept at `Approved — CP-2 closed 2026-05-18`; the living-contract amendment protocol (§11) governs, and CP-1/CP-2 are not reopened (they reviewed the skill as it stood). Surfaced in Phase 1; operator-approved.
- **Default `inline` for REVIEW_EXECUTION** (not `dispatch`), per §5.8 explicit shape — unlike spec-execute's `EXECUTION`, whose default flips to `dispatch` at P2.6. Review dispatch is opt-in; the isolation payoff is real but the inline path is the safe, unchanged default.
- **Reviewer-floor-absent fallback** specified in the master (spawn at coordinator's model, never below) since no current checkpoint declares a reviewer floor — faithful to §5.8's "declared reviewer floor" without inventing a required checkpoint field.
- **`lastUpdated` bumped on spec-review only** (2026-07-06), operator-chosen via AskUserQuestion. spec-execute's stale `lastUpdated: 2026-07-03` (unbumped by P2) left for a separate typo-class fix rather than widening P4 scope — noted as a follow-up below.
- **Commit shape mirrors P2.2–P2.5** (spec-edit → journal-referencing-it → master+deploy → closeout): never has a journal reference its own commit, avoiding P2.1's `--amend`-rewrites-SHA trap.
**Spec amendments:** [spec-review governing spec amendment 2026-07-06-1](../20260518-spec-review-skill/journal.md) — §4 pointer + new §5.12/§5.13 + §6 NFR rows; paired with the master REVIEW_EXECUTION/DISPATCH MODE/OPERATOR CUES additions.
**Surprises and learnings:**
- **Coordinator write-back asymmetry vs. spec-execute dispatch.** In spec-execute dispatch the *worker* writes the journal first-hand (confabulation guard on DoD evidence); in spec-review dispatch the *coordinator* writes the verdict back (§5.8), because the reviewer's returned structured verdict *is* the first-hand deliverable — the coordinator transcribes a structured block, not a narrative. The two skills' dispatch write-back rules are deliberately opposite; the master DISPATCH MODE calls this out so a future reader does not "fix" the asymmetry into false uniformity.
- **CP-2 now un-gated on P4.** With P4 landed, only **P2.6** (defaults flip) remains before dispatch-execution CP-2 (which audits the whole P2–P4 amendment set: master≡deploy sweep, cold-read test of `spec-worker`, no skill gained harness frontmatter) triggers. The `spec-reviewer` cold-read test (§8) is also a CP-2 exit criterion, still pending.
- **Pre-existing follow-up surfaced:** spec-execute master `lastUpdated` is stale (2026-07-03, not bumped by the 2026-07-05 P2 amendments). Typo-class; can land directly per CLAUDE.md's typo-class exception, or fold into P2.6's master edit.
**Next task pointer:** **P2.6** — atomic defaults flip (master `spec-execute` INPUTS: `EXECUTION` default → `dispatch`, `AUTONOMY` default → `checkpoint`; dispatch-execution §5.1), now the last remaining amendment-set step. Consider folding the stale-`lastUpdated` fix into that same master edit. Once P2.6 lands, **CP-2** triggers (amendment-set consistency + cold-read tests).
