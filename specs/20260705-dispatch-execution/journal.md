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
