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
