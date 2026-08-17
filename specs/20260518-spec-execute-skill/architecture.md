# `spec-execute` Skill — Architecture and Protocol Specification

> Status: Approved — CP-2 closed 2026-05-18
> Date: 2026-05-18
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set.

## 1. Overview

The `spec-execute` skill is the methodology's **in-session execution skill**: it advances against an existing feature spec one task at a time, with full closeout at every task boundary, and surfaces drift between plan and reality as explicit amendments rather than silent deviations. It is the only quintet member whose primary output is not markdown — its principal artifacts are code commits, accompanying spec/journal edits, and, when present, paired commits across two repositories. It sits downstream of [spec-write](../../.agents/skills/spec-write/SKILL.md), interacts laterally with [spec-review](../../.agents/skills/spec-review/SKILL.md) (Phase 7 handoff at checkpoint triggers) and [spec-amend](../../.agents/skills/spec-amend/SKILL.md) (the Amendment Protocol routes drift findings here), and inherits architectural commitments from the constitution ([specs/tech-stack.md](../tech-stack.md)) and from a sibling design spec ([specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md)) that postdates the skill's predecessor and authoritatively shaped Phase 1, Phase 4, Phase 6, and Phase 8.

This document is a **retroactive design specification**: the skill already ships at [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md), which is authoritative for the skill's behavior. The spec describes what the skill *is* and what it *commits to*, so the methodology can be reviewed, amended, and adopted on the same footing as the artifacts it produces. The spec does not redesign the skill; any divergence the spec exposes between its commitments and the shipping SKILL.md is routed to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md), never silently corrected.

## 2. Goals and Non-goals

### Goals

- Produce a self-contained, descriptive specification of the `spec-execute` skill's vocabulary, contract, phases, operating model, and Amendment Protocol.
- Declare review gates (§9) so the skill becomes reviewable via [/spec-review](../../.agents/skills/spec-review/SKILL.md) against named checkpoints.
- Hold the skill to the **Atomic-Skill Portability Principle** declared in the methodology's constitution ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)) — including degradation when optional inputs (`SPEC_REPO_ROOT`, `SPEC_TARGET_BRANCH`, `SESSION_GOAL`, `TIME_BUDGET`) are absent.
- Continue the **paired-artifact pattern** for retroactive skill specs — `architecture.md` + `journal.md` — established at N=1 ([specs/20260517-project-constitution-skill/](../20260517-project-constitution-skill/)), refined at N=2 ([specs/20260518-spec-design-skill/](../20260518-spec-design-skill/)) and N=3 ([specs/20260518-spec-write-skill/](../20260518-spec-write-skill/)). This N=4 instance validates, refines, or rejects each of the N=3 journal's "Pattern for N=4" callouts.
- Distinguish two upstream sources for the skill's commitments: a **predecessor artifact** ([docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438) authoritative for *design rationale*, and a **sibling design spec** ([specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md)) authoritative for *current behavior added after the predecessor* (Phase 8, token economy, multi-repo detection). This two-source structure is new at N=4.
- Carry forward the **section-heading citation discipline** ([N=2 CP-1 advisory c](../20260518-spec-design-skill/journal.md), reinforced at [N=3 CP-1 §7](../20260518-spec-write-skill/journal.md)) and elevate it in §9 CP-1 review focus.

### Non-goals

- **Redesign of the skill.** The shipping SKILL.md is authoritative for behavior. This spec is descriptive.
- **A template for the two remaining legacy-quintet retroactive specs** (`spec-review`, `spec-amend`). Each is authored in its own session per the operator's spec-by-spec cadence; cross-session scaffolding, if any, derives from journal mining at N≥4, not from declarations in this spec body. The `docs/retroactive-spec-pattern.md` decision is governed by [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) and was *deferred* at N=3 close (operator-confirmed); to be revisited at N=5 close, not here.
- **Modification of the shipping SKILL.md.** Only the Amendment Protocol via [/spec-amend](../../.agents/skills/spec-amend/SKILL.md) touches SKILL.md.
- **Specifying tooling, models, or platforms.** The skill produces commits + markdown only and remains tooling-agnostic, consistent with the [mission.md Out of Scope](../mission.md) commitment.
- **Resolving the cross-skill amendment coordination question** named at [docs/retroactive-spec-strategy.md OQ-3](../../docs/retroactive-spec-strategy.md) and at [N=3 §12](../20260518-spec-write-skill/architecture.md). It surfaces in spec-execute's domain (Phase 4 / Phase 6 amendment-trigger sites) but its resolution belongs to the `spec-amend` retroactive spec at session 5.
- **Specifying when an operator should invoke `/spec-execute` vs. work directly without the skill.** Same shape as [N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md) (the spec-design vs spec-write format-selection question), methodology-wide, not skill-specific to `spec-execute`.

## 3. Background and Constraints

### Prior state

The `spec-execute` skill predates the spec-driven-development trilogy. Its predecessor — the `spec-execution-prompt.md` artifact at [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438, with companion design notes at lines 406–438 — was authored before the methodology had a constitution, sibling skills, or the session-economy discipline. The skill evolved through four later commits:

- `80000b1` (2026-05-14) — trilogy commit. Amendment Protocol changed from inline-applied (predecessor lines 391–403) to routed to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md). Wired the skill into the broader pipeline (upstream `spec-design` / `project-constitution`; lateral `spec-amend`).
- `5ce4024` (2026-05-14) — **session-economy commit**, the largest behavioral expansion in this family. Added Phase 8 (Session Continuity Check) with the token-economy factor, the heuristic rubric, and the output format. Added Phase 1 multi-repo detection. Strengthened Phase 4 / Phase 6 paired-commit prose. Phase 7's predecessor flow ("if no checkpoint, return to Phase 1") changed to "proceed to Phase 8". The architectural source for these changes is [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) — a *sibling design spec*, not a predecessor doc.
- `c63e3ba` (2026-05-14) — `lastUpdated` frontmatter added across the skill family.
- `189c6cc` (2026-05-15) — path convention update: `docs/specs/<feature>.md` → `specs/YYYYMMDD-<feature-name>/feature.md`, propagated through `SPEC_PATH` examples in the six lifecycle skills via the [spec-path-convention](../20260515-spec-path-convention/architecture.md) feature spec.

The predecessor doc is treated in this spec as **authoritative for the skill's design rationale** but not authoritative for current behavior: the shipping SKILL.md is the latter. Where they diverge, SKILL.md wins and the divergence is read by CP-2 as *evolution*, not drift. The Amendment-Protocol routing change (`80000b1`), the session-economy Phase 8 addition (`5ce4024`), the multi-repo strengthening (`5ce4024`), and the `SPEC_PATH` update (`189c6cc`) all postdate the predecessor doc and are *evolution* with explicit commits and, where applicable, an authoritative sibling design spec.

The [session-economy design spec](../20260514-session-economy/architecture.md) occupies a distinct role: it is a *sibling design spec* that authoritatively commits to behavior present in the current SKILL.md (its §5.1 specifies the Phase 8 token-economy factor exactly as it ships; its §5.2 specifies Phase 1 multi-repo detection exactly as it ships). It is cited Authoritative in §14, alongside the SKILL.md itself. This two-source split (predecessor for rationale; sibling design spec for current behavior added after the predecessor) is the structural novelty at N=4. The N=3 prediction that the trilogy-extended skills "likely have thin or no predecessor" ([N=3 journal "Pattern for N=4 #4"](../20260518-spec-write-skill/journal.md)) is openly disconfirmed by this session — `spec-execute` has a *rich* predecessor (~204 lines of prompt + design notes); the corrective is recorded in [this spec's journal](./journal.md).

### Constraints (cited)

- **Atomic-Skill Portability Principle** ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)). The skill must be a portable atomic unit: workflow + Orientation Report shape + journal-entry template bundled in its own `SKILL.md`; adapts to richer host-repo embodiments (`SPEC_REPO_ROOT` set, `SESSION_GOAL` provided, multi-repo layout, sibling skills) when present; degrades cleanly when absent. A `spec-execute` installed against an unrelated host repo with a single-repo feature spec still produces a conformant Orientation Report and per-task closeout.
- **AI context window limits** ([specs/tech-stack.md §44](../tech-stack.md#L44)). Feature specs and journals are LLM-consumed across multiple sessions. The closeout-at-every-task-boundary discipline (OP §2) and the Phase 8 token-economy factor both propagate this constraint by keeping each session's working context bounded and by surfacing context saturation as a session-continuity signal.
- **Spec-driven-development convention** ([specs/tech-stack.md §51](../tech-stack.md#L51)). The methodology repo "eats its own cooking" — changes to the methodology follow the methodology. This convention is the explicit justification for this retroactive spec.
- **Repository layout convention** ([specs/tech-stack.md §48](../tech-stack.md#L48)). `specs/YYYYMMDD-<feature-name>/feature.md` + `journal.md` is the canonical `SPEC_PATH` / `JOURNAL_PATH` shape, set by the [spec-path-convention](../20260515-spec-path-convention/architecture.md) propagation in commit `189c6cc`.

### Dependencies

- **Upstream.**
  - [spec-write](../../.agents/skills/spec-write/SKILL.md) — produces the feature spec at `SPEC_PATH` (with `feature.md` + `journal.md` pair) that `spec-execute` consumes. The §7 Task Breakdown, §9 Review Checkpoints, and Definition-of-Done entries authored by `spec-write` are the contract `spec-execute` advances against. The [spec-write retroactive spec §3 Dependencies](../20260518-spec-write-skill/architecture.md#3-background-and-constraints) confirms this handoff from the spec-write side.
  - [spec-design](../../.agents/skills/spec-design/SKILL.md) and [project-constitution](../../.agents/skills/project-constitution/SKILL.md) — indirect upstream (via the feature spec). The feature spec carries forward design-spec and constitutional bindings; `spec-execute` honors them by following the feature spec's Task Breakdown and Background sections rather than re-deriving them.
  - [session-economy architecture spec](../20260514-session-economy/architecture.md) — *companion* design spec authoritative for Phase 8 (Session Continuity Check), the token-economy factor, Phase 1 multi-repo detection, and the strengthened Phase 4/6 paired-commit discipline. Not a *predecessor*: this design spec postdates the predecessor doc, and its committed behavior is what currently ships.
- **Downstream.**
  - [spec-review](../../.agents/skills/spec-review/SKILL.md) — Phase 7 (Checkpoint Gate) hands off to `spec-review` when the just-completed task triggers a Review Checkpoint. `spec-execute` does not run the review itself; it stops, summarizes, and waits.
  - [spec-amend](../../.agents/skills/spec-amend/SKILL.md) — the Amendment Protocol routes drift findings from `spec-execute` to `spec-amend`. The separation between *proposing* the amendment (in `spec-execute`) and *applying* it (in `spec-amend`) is what keeps amendments visible.
- **Lateral.** Sibling lifecycle skills reference `spec-execute` by name and path; all six skills were modified together in commits `5ce4024` (session-economy / multi-repo) and `189c6cc` (path convention).

## 4. Architecture

The skill's architecture is an **eight-phase iterative execution workflow** that advances against an existing feature spec one task at a time, producing **code commits + paired spec/journal edits** (and, when `SPEC_REPO_ROOT` is set, **paired commits across two repos**) at every task boundary, executed as a **portable atomic unit** that integrates with the broader spec-driven-development pipeline.

### Output topology

```text
Per task boundary:
  CODEBASE_ROOT:                                SPEC_REPO_ROOT (if set; else same as CODEBASE_ROOT):
    code commits (with T-NN: prefixes)            spec edits  ── Task status set to `done` with date + commit SHAs
                                                  journal entry ── one entry per task (Status / Commits / Files /
                                                                    Tests / DoD verification / Decisions / Amendments /
                                                                    Surprises / Next-task pointer)
                                                  closeout commit (paired with code commit; same task ID)
```

The paired-commit discipline is binding when `SPEC_REPO_ROOT` is set: neither commit ships without the other. Single-repo execution is the common case; the multi-repo case is a strictly-richer configuration that does not break the single-repo path.

### Vocabulary (defined here, used consistently below)

- **Feature spec** — the input artifact at `SPEC_PATH`, produced by `spec-write`. Authoritative for the work to be executed. Distinct from a design spec.
- **Journal** — the paired artifact at `JOURNAL_PATH`. Created by `spec-write` (or by `spec-execute` on first run if absent). Per-task closeout entries are appended at every task boundary.
- **Definition of Done (DoD)** — per-task closeout checklist authored by `spec-write` in §7. Verified item-by-item with evidence (not summary) in Phase 5.
- **Task boundary** — the transition between a just-completed task's closeout and the proposal of the next task. The skill's most load-bearing transition: closeout fires here, Phase 7 checkpoint gate fires here, Phase 8 session-continuity check fires here.
- **Orientation Report** — Phase 1 output: spec status summary, last completed task, in-progress task, open questions, proposed next task, drift signals, multi-repo state.
- **Closeout** — the Phase 6 update sequence: spec edits, journal entry, next-task pointer, surfaced risks/OQs, and the artifact-update commit. A task is not complete until closeout is complete.
- **Drift signal** — anything in the branch that does not match the spec, or anything in the spec that no longer matches the codebase. Surfaced in Phase 1 Orientation Report and routed through Amendment Protocol when material.
- **Amendment Protocol** — the mechanism by which `spec-execute` *proposes* a spec change to `spec-amend` (which *applies* it). The separation of proposing from applying is what keeps amendments visible.
- **Review Checkpoint** — a named gate declared in the feature spec's §9, with a trigger task. Phase 7 detects trigger and hands off to `spec-review`.
- **Session continuity check** — Phase 8's pause-and-recommend at every task boundary, weighing context spent, coupling, re-anchoring cost, drift risk, token economy, and user signal. The recommendation is for the operator; the skill does not unilaterally continue.
- **Paired commit** — when `SPEC_REPO_ROOT` is set, the pair of commits (one in `CODEBASE_ROOT` for code, one in `SPEC_REPO_ROOT` for spec/journal updates) that together close out a single task. Both reference the same task ID.

### Execution model

```text
                ┌─────────────────────────────────────────────────────────────────┐
                │                                                                 │
                ▼                                                                 │
  Phase 1: ORIENT  ──▶  Phase 2: PRE-FLIGHT  ──▶  Phase 3: CLARIFY  ──▶  Phase 4: │ EXECUTE
  (spec / journal /     (verify last task          (assumptions / OQs /            │
   branch / multi-      DoD with evidence)         amendments before               │
   repo state;                                     starting; stop on               │
   propose next task;                              blockers)                       │
   STOP for approval)                                                              │
                                                                                   │
                                                                                   ▼
                                                                          Phase 5: VERIFY
                                                                          (DoD with
                                                                           per-item evidence
                                                                           for current task)
                                                                                   │
                                                                                   ▼
                                                                          Phase 6: UPDATE
                                                                          (spec edits +
                                                                           journal entry +
                                                                           next-task pointer
                                                                           + closeout commit;
                                                                           paired if multi-repo)
                                                                                   │
                                                                                   ▼
                                                                          Phase 7: CHECKPOINT
                                                                          GATE
                                                                          (if triggered, STOP
                                                                           and hand off to
                                                                           /spec-review;
                                                                           else proceed)
                                                                                   │
                                                                                   ▼
                                                                          Phase 8: SESSION
                                                                          CONTINUITY CHECK
                                                                          (pause; recommend
                                                                           continue vs fresh;
                                                                           STOP for user call)
                                                                                   │
                              ◀──────────────────────────────────────────────────────
                            (user says continue → return to Phase 1 for next task;
                             user says pause → session ends with journal as handoff)
```

Four explicit pause points: Phase 1 (await approval of proposed next task), Phase 3 (stop on blocker OQs), Phase 7 (hard stop on checkpoint trigger, await /spec-review verdict), Phase 8 (pause regardless of checkpoint state, await user call on session continuity). The pause discipline at Phase 8 is the largest behavioral difference from the predecessor doc: the predecessor's Phase 7 said "if no checkpoint is triggered, return to Phase 1"; the current skill says "if no checkpoint is triggered, proceed to Phase 8" — never auto-continues.

### Execution modes and autonomy

Two orthogonal operator-set inputs govern how a session runs, independent of the eight-phase workflow above:

- **`AUTONOMY`** governs *where the run stops*. `task` pauses for operator approval at every task boundary (Phase 8). `checkpoint` runs tasks back-to-back and stops only at the designed-stop set: review checkpoints, blocker-class open questions, proposed spec amendments, model-floor conflicts, and any production-touching action. `AUTONOMY` is operator-set only — never self-granted, never self-escalated — and is restated in the journal so later sessions know the operating mode. This input shipped in commit `44bd7f1` (2026-07-03), postdating this spec's 2026-05-18 authoring; it is documented here to close that gap.
- **`EXECUTION`** governs *who implements*. `inline` — the session runs every phase itself, the behavior described throughout §5. `dispatch` — the session runs as a thin orchestrator that delegates each task's Phases 4–6 to a disposable worker subagent spawned at the task's model floor. The two inputs are orthogonal; all four combinations are legal.

**Defaults.** Under the [dispatch-execution design spec](../20260705-dispatch-execution/architecture.md) (Approved, CP-1 closed 2026-07-05), the defaults are `EXECUTION: dispatch` and `AUTONOMY: checkpoint` — autonomous operation between designed stops, dispatched to workers. `inline` and `task` remain available as operator-selected *restrictions* for tightly supervised work. The governing rule across both axes: **the agent may never loosen the stop set** — it may not skip a designed pause, downgrade a blocker, or continue past a designed stop; only the operator may restrict, and the agent never grants itself more autonomy. Dispatch mechanics — orchestrator/worker topology, the worker brief and receipt contract, the Phase 8 context budget, and operator cues at boundaries — are specified in the dispatch-execution spec (§4, §5.2–5.9) and described against the affected phases across the P2 amendment set. That spec is the sibling design-spec authority for dispatch behavior, standing to this spec as the [session-economy spec](../20260514-session-economy/architecture.md) stands for Phase 8. The orchestrator/worker phase split, orchestrator conduct rules, and worker brief contract are described in §5.12; operator cues at operator-facing boundaries (emitted in both modes) are described in §5.13.

### Where this design plugs in

`spec-execute` is downstream of `spec-write` (consumes the feature spec) and laterally connected to both `spec-review` (Phase 7 handoff) and `spec-amend` (Amendment Protocol handoff). Outputs are commits and journal entries at the paths declared by the feature spec; downstream review and amendment sessions read these as their input. The skill is invoked by the operator, not by other skills.

## 5. Detailed Design

### 5.1 Phase 1 — Orient (pause point)

**Purpose.** Re-anchor against the spec, the journal, the branch state, and any multi-repo configuration before proposing the next task. Resists context decay by re-reading rather than relying on memory of earlier-in-session reads.

**Inputs.** `SPEC_PATH`, `JOURNAL_PATH`, `CODEBASE_ROOT`, `TARGET_BRANCH`; optionally `SPEC_REPO_ROOT`, `SPEC_TARGET_BRANCH`, `SESSION_GOAL`, `TIME_BUDGET`. If `JOURNAL_PATH` does not exist yet, create it on first run.

**Outputs.** Orientation Report: spec status summary (counts by status, citing the section read from), last completed task (with DoD-satisfaction sanity check), in-progress task (if any), open questions outstanding (triaged), proposed next task (with rationale), drift signals (branch vs spec), and multi-repo state (whether `SPEC_REPO_ROOT` is set; both repos on expected branches; no blocking uncommitted changes).

**Behavior.** Read-only. Phase 1 ends with **stop and wait for user approval** of the proposed next task. No work begins.

**Multi-repo detection.** If `SPEC_REPO_ROOT` is not set, the skill checks whether `SPEC_PATH` resolves within `CODEBASE_ROOT`. If it does not, the skill surfaces the discrepancy and asks the operator to confirm: *"The spec appears to live in a different repo than the codebase. Should I treat `<detected-path>` as `SPEC_REPO_ROOT`?"* If `SPEC_REPO_ROOT` is already set, it is reconfirmed (repos move). This detection is the architectural commitment from [session-economy spec §5.2](../20260514-session-economy/architecture.md).

**Pattern invoked.** *Re-anchor at boundaries* — OP §6 in the shipping SKILL.md, rationalized in the predecessor design notes ([line 412](../../docs/spec-driven-development-prompts-conversation.md#L412)): *"matches the periodic re-anchoring mitigation you've already explored in your multi-agent framework."*

**Why this design.** Sessions that rely on remembered spec content drift; re-reading at every task boundary is cheap relative to the drift cost. The pause for approval is the operator's primary input channel before any new work begins.

**Alternatives considered.** Read once at session start and rely on memory thereafter (rejected — context decay; predecessor design notes are explicit on this lever). Auto-propose and auto-start the next task (rejected — collapses the operator's primary control point).

### 5.2 Phase 2 — Pre-flight verify (last task)

**Purpose.** Confirm that the previously completed task is actually closed out before opening any new work. Catches the "claimed done but tests weren't actually re-run" failure mode named in the predecessor design notes ([line 416](../../docs/spec-driven-development-prompts-conversation.md#L416)).

**Behavior.** Walk each DoD item on the last completed task and produce evidence (file path, test name, CI status, lint output, doc location). Confirm task status in spec is `done` with date + commit reference. Confirm journal entry exists. Confirm branch is in a deployable/revertible state per the §9 Review Checkpoints section of the feature spec. If any check fails, the next action is to repair the closeout, not to start new work.

**Pattern invoked.** *Pre-flight verification* — predecessor design notes line 416.

**Why this design.** A clean handoff to the next task requires the last one to be actually clean, not nominally clean. The pre-flight check is the place where "the previous session said done" is verified against branch and spec state.

**Alternatives considered.** Trust the prior session's journal claim of done (rejected — the journal is one input; the branch state is the load-bearing one).

### 5.3 Phase 3 — Clarify (only if needed)

**Purpose.** Resolve task-specific ambiguity before execution, when the proposed task as written has implicit assumptions, blocker-class open questions, or is materially wrong/stale relative to the codebase.

**Behavior.** For the proposed next task, list: assumptions intended to be acted on, open questions (triaged `[blocker]` / `[important]` / `[minor]`), spec amendments proposed before starting (with diff against the relevant spec section). Stop on blockers. For non-blockers, state how the agent will proceed and what will be noted in the journal.

**Pattern invoked.** *Triage-by-severity* from sibling authoring skills (`spec-design`, `spec-write`, `project-constitution`), applied here at task granularity rather than spec granularity.

**Why this design.** Task-level clarification is the place to surface a stale or wrong task description *before* it produces code that has to be undone. Pre-execution amendments are cheaper than mid-execution amendments.

**Alternatives considered.** Skip Phase 3 if the task description appears self-evident (rejected — the silent-deviation failure mode names exactly this: tasks that "appear" clear often mask assumptions, and the cost of stating them is small).

### 5.4 Phase 4 — Execute

**Purpose.** Implement the proposed task strictly within its declared scope.

**Behavior.** Match existing codebase conventions identified in the feature spec's §3 Background. Do not introduce new patterns, dependencies, or abstractions unless the task explicitly calls for them. Write tests required by the task alongside production code (tests and code land together). Keep changes scoped to the task's declared file list — touching a file outside that list requires stopping and proposing a spec amendment first. Do not write speculative code for "the next task" while finishing the current one — pre-staging the next task's work blurs the boundary that closeout depends on. Commit at logical points within the task with messages that reference the task ID (e.g. `T-04: add validator for Foo input`). If the task as specified cannot be completed correctly, stop and propose a spec amendment — do not work around the spec.

**Multi-repo case.** When `SPEC_REPO_ROOT` is set, code commits land in `CODEBASE_ROOT`; spec/journal updates land in `SPEC_REPO_ROOT`. Each task produces a *pair* of commits — one per repo — both referencing the same task ID. A code commit shipped without its paired spec/journal commit is a failure mode: the spec falls out of sync the moment that happens. The paired-commit discipline is the architectural commitment from the [session-economy spec §1 Overview](../20260514-session-economy/architecture.md) and [§3 Background](../20260514-session-economy/architecture.md), which acknowledge that spec-execute Phase 4/6 paired-commit prose pre-existed and was strengthened by commit `5ce4024`. The strengthening is not enumerated in session-economy §5 (which covers Phase 8 token economy and Phase 1 multi-repo detection only); the commit is the implementation event.

**Pattern invoked.** *No silent deviation* — OP §3 in the shipping SKILL.md. *One task at a time* — OP §5. *No speculative code for the next task* — [SKILL.md WHAT NOT TO DO](../../.agents/skills/spec-execute/SKILL.md).

**Why this design.** The two failure modes Phase 4 addresses are silent scope expansion (touching files outside the task's declared list) and silent workaround (continuing under a spec that contradicts implementation). Both are addressed by routing back to the spec rather than around it.

**Alternatives considered.** Allow inline scope expansion when "small" (rejected — "small" is a self-justifying threshold; the spec-amend route forces the question into the open).

### 5.5 Phase 5 — Verify (current task)

**Purpose.** Confirm the current task's DoD is satisfied with evidence per item, not summary.

**Behavior.** Walk each DoD item and produce evidence: test runner output for added and existing tests, lint/types runner output, doc files updated (with quoted new sections), observability hooks (log lines, metrics, spans, with locations), acceptance-criteria walks (each Given/When/Then with how it is satisfied). If any item is not satisfied, the task is not done — either finish it or split the unfinished portion out as a follow-up task in the spec.

**Pattern invoked.** *Verify before claiming done* — OP §4 in the shipping SKILL.md. *DoD with evidence per item* — predecessor design notes line 418: *"pairs with the spec authoring prompt's requirement that DoD items be objectively verifiable."*

**Why this design.** Summary statements ("tests pass and docs updated") permit nominal completion without verification. Item-by-item evidence forces the verification to be produced and recorded.

**Alternatives considered.** Single-summary DoD check (rejected — predecessor design notes are explicit on this lever).

### 5.6 Phase 6 — Update artifacts

**Purpose.** Close out the just-completed task before any next task is started. Survives end-of-session context decay by ensuring the last completed task is always clean.

**Behavior.** Five updates fire before the task is declared complete:
1. **Spec.** In the Task Breakdown section, mark the task `done`, add date and commit SHA range. Move any resolved open questions out of the OQ section into the relevant design section as decisions, with rationale.
2. **Journal entry.** Append an entry matching the format declared in SKILL.md (Status / Commits / Files touched / Tests added / DoD verification / Decisions made / Spec amendments / Surprises and learnings / Next task pointer).
3. **Next-task pointer.** Identified per the dependency graph; stated explicitly in the journal entry. This is the next-session handoff.
4. **Surface new risks/OQs.** Anything the work revealed lands in the spec's Risks or OQ sections.
5. **Commit the artifact updates.** Stage and commit the spec and journal changes with a message that references the task ID (e.g. `spec: T-04 closeout — mark done, journal entry, next-task pointer`).

**Multi-repo case.** When `SPEC_REPO_ROOT` is set, the artifact-update commit lands in `SPEC_REPO_ROOT`, *not* in `CODEBASE_ROOT`. It is the paired commit to whatever code commits closed out the task. The task is not declared complete until **both** commits exist. The discipline is the same architectural commitment described in §5.4 above — session-economy [§1 Overview](../20260514-session-economy/architecture.md) and [§3 Background](../20260514-session-economy/architecture.md) acknowledge Phase 4/6 paired-commit pre-existence and strengthening; commit `5ce4024` is the implementation event.

**Pattern invoked.** *Closeout at every task boundary, not at session end* — OP §2 in the shipping SKILL.md, rationalized in the predecessor design notes ([line 410](../../docs/spec-driven-development-prompts-conversation.md#L410)): *"the single biggest reliability lever, and it's a direct response to the context-decay problem you've hit where session closeout instructions get missed by the time the session ends."*

**Why this design.** Closeout-at-boundary makes the spec and journal always accurate-as-of-the-last-completed-task, even when the session dies abruptly. Closeout-at-session-end is the failure mode this design exists to prevent.

**Alternatives considered.** Batch closeouts at session end (rejected — explicitly the failure mode). Closeout in code repo only and leave spec/journal updates for later (rejected — the spec falls out of sync the moment that happens).

### 5.7 Phase 7 — Checkpoint gate

**Purpose.** Stop the iteration loop when the just-completed task triggers a Review Checkpoint declared in the feature spec's §9.

**Behavior.** Check the feature spec's Review Checkpoints section. If the completed task is the trigger for a checkpoint: stop, do not proceed to the next task; summarize what the reviewer should focus on (citing the checkpoint's review focus); provide a diff summary or PR-ready description; wait for explicit user confirmation that the checkpoint has passed before continuing. If no checkpoint is triggered, **do not** jump back to Phase 1 — proceed to Phase 8.

**Pattern invoked.** *Checkpoint gate as hard stop* — predecessor design notes line 422: *"This is the safety valve that keeps the branch deployable at every checkpoint per the spec."*

**Why this design.** Review checkpoints are the methodology's coarse-grained gates; allowing the executor to step past one silently turns them into nominal markers. The hard stop is what keeps the spec's §9 commitments binding.

**Alternatives considered.** Soft-pause that asks "shall I continue?" (rejected — the gate is a hard stop in spec-write's contract; spec-execute honors it).

### 5.8 Phase 8 — Session continuity check

**Purpose.** Pause at every task boundary — even when no checkpoint is triggered — to weigh whether the *same conversation* should pick up the next task or whether a fresh session would serve the work better. Surface reasoning and recommendation to the operator; never unilaterally continue.

**Behavior.** The skill considers six factors: context already spent (long sessions accumulate context the next task does not need), coupling to what was just done (next task on same files/vocabulary favors continuing), re-anchoring cost (small relative to drift risk), drift risk on long sessions (long sessions confabulate), **token economy** (literal tokens consumed and context-window saturation; billing — fresh session with clean prompt-cache hit is often cheaper than continuing a bloated context), and user signal ("stay in this session" overrides heuristic). A reasonable default rubric is provided (next task on same surface → continue; clean break in scope → fresh session; long session / chewed through context → fresh session; small fully-specified task → either; open question needing offline decision → pause regardless). The recommendation is output in a declared format (Task closed-out / one-or-two-sentence reasoning / **Recommendation:** continue|pause / next-task pointer). Then **stop and wait** for the user's call. If the user says continue, return to Phase 1 (re-reading the spec rather than relying on memory). If the user pauses, the session ends cleanly with the journal as the handoff.

**Context budget.** The token-economy factor carries a fixed mechanical trigger: **80,000 tokens of session context consumed** (harness context indicator, or conservatively estimated when none is available). Below budget, Phase 8 weighs qualitatively as above; at or above budget the recommendation is forced — "fresh session" in inline mode, and under `AUTONOMY: checkpoint` a breach stops the run at the task boundary with the journal's next-task pointer set. In dispatch mode the budget is moot: a thin orchestrator should never approach 80k, so reaching it signals an orchestrator-conduct violation and stops the run for the operator. Fixed tokens rather than a fraction of the window holds the operator's cost exposure constant across models. Authoritative design source: [dispatch-execution spec §5.6](../20260705-dispatch-execution/architecture.md).

**Override.** Phase 8 is skipped only when the operator has explicitly said *"run the full set without checking in"* — and even then, a brief end-of-task note is surfaced so the operator can interrupt. A context-budget breach is not subject to the override: it stops the run in every mode.

**Pattern invoked.** *Session economy* — the architectural commitment from [session-economy spec §5.1](../20260514-session-economy/architecture.md), which added the token-economy factor explicitly to the otherwise heuristic-only Phase 8 inherited at session-economy time; the fixed context budget is the [dispatch-execution spec §5.6](../20260705-dispatch-execution/architecture.md) commitment that converts the factor from advice into a mechanical trigger.

**Why this design.** Cognitive freshness and token availability are independent constraints — a session can be cognitively fresh but context-window-full, or fresh in context but cognitively stale from a long debug detour. Naming token economy as a discrete factor (alongside cognitive ones) prevents it from being treated as implicit. The no-unilateral-continuation discipline is what keeps the recommendation a *recommendation*; the operator decides.

**Alternatives considered.** Skip Phase 8 when no checkpoint is triggered (the predecessor doc's behavior at line 377; explicitly rejected at session-economy commit `5ce4024`). Auto-continue when the recommendation is "continue" (rejected — collapses operator control). Make Phase 8 mandatory even under the explicit-override clause (rejected — operator authority to opt out is preserved by the override note).

### 5.9 Amendment Protocol — proposing vs applying

**Purpose.** Route spec amendments through the named `spec-amend` skill rather than applying them inline. The separation between *proposing* (in `spec-execute`) and *applying* (in `spec-amend`) is what keeps amendments visible to reviewers, future sessions, and the operator scrolling back through history.

**Trigger.** Any place in Phase 3 (pre-execution), Phase 4 (mid-execution), or Phase 5 (DoD-verification surfaces a mismatch) where implementation contradicts the spec.

**Behavior (the `spec-execute` side).**
1. Stop work.
2. Capture the trigger: file path, test output, contradiction, or other evidence from the current task.
3. State which section of the spec needs amending and a one-line description of the proposed change.
4. State the impact on the current task: blocked entirely, partially blocked, or proceedable-with-a-note.
5. Hand off to `spec-amend`, passing `SECTION`, `TRIGGER`, and any `PROPOSED_CHANGE` text. Any `PROPOSED_CHANGE` carried forward must be expressible as a diff against the existing section — surgical, not a rewrite. If the change cannot be expressed surgically, route as a rewrite candidate (`spec-write` re-decomposition) rather than as an amendment.
6. Wait for the amendment to be applied (or rejected) before resuming.

**On amendment applied.** Re-orient via Phase 1 against the *amended* spec — do not rely on memory of the pre-amendment text.

**On amendment rejected.** Reconsider whether the task can complete as written. If not, the task may need re-decomposition via `spec-write`, or pulled back to the design level via `spec-design`.

**Pattern invoked.** *Separation of proposing from applying* — added at trilogy commit `80000b1`, distinct from the predecessor's inline Amendment Protocol (predecessor lines 391–403 applied amendments within the execution session). The current routing is the methodology's commitment that amendments are first-class events. *Diff required; surgical not rewrite* — [SKILL.md WHAT NOT TO DO](../../.agents/skills/spec-execute/SKILL.md); the proposing side carries the obligation by ensuring `PROPOSED_CHANGE` is expressible as a diff against the existing section.

**Why this design.** Inline amendments collapse the visibility of "what changed in the spec, when, and why." Routed amendments produce a discoverable trail (commit, journal entry, spec edit) that survives session boundaries.

**Alternatives considered.** Keep amendments inline (rejected — the predecessor's failure mode is exactly the silent-amendment problem the trilogy commit `80000b1` set out to fix). Make `spec-amend` automatic when contradiction is detected (rejected — amendments require operator approval; automation would defeat the visibility goal).

### 5.10 Voice discipline

**Purpose.** Keep execution-phase prose unambiguous across multiple sessions and authors.

**Behavior.**
- Imperative for protocol rules ("the agent must…", "do not declare done until…").
- Plain declarative for orientation reports and journal entries.
- No marketing language ("clean," "robust," "ready"). Describe the property concretely or omit.

**Pattern invoked.** Voice rules inherited from sibling design specs ([N=2 §5.6](../20260518-spec-design-skill/architecture.md), [N=3 implicit](../20260518-spec-write-skill/architecture.md)).

**Why this design.** A session's journal entries are read by future sessions as authoritative; ambiguity here propagates into execution decisions.

### 5.11 Portability rule for links

**Purpose.** Ensure spec text survives repository moves, mirrors, and machine-relative path mishaps.

**Behavior.** Committed prose must not contain absolute filesystem paths or machine-specific paths. Preferred forms, in priority order: (1) published URL, (2) repo-relative path, (3) sibling-relative description, (4) bare name + host description.

**Pattern invoked.** Linking conventions inherited from N=1 ([N=1 amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md)) and the four sibling retroactive specs.

**Why this design.** Absolute paths and machine-specific paths cannot travel. The carry-forward discipline is now N=4-validated.

### 5.12 Dispatch mode — orchestrator/worker split, conduct, and worker brief

**Purpose.** Describe the behavior the shipping SKILL.md's DISPATCH MODE section commits to under `EXECUTION: dispatch`: a thin orchestrator delegates each task's implementation to a disposable worker, so batch cost stays flat and all inter-task continuity lives in the artifacts. Authoritative design source: the [dispatch-execution spec](../20260705-dispatch-execution/architecture.md) §4 (composition rule), §5.2 (orchestrator conduct), §5.3 (worker brief) — a sibling design spec for dispatch behavior, standing to this spec as the [session-economy spec](../20260514-session-economy/architecture.md) stands for Phase 8.

**Composition rule.** The eight phases split by mode. Inline: the session runs all eight (§5.1–§5.8). Dispatch: the orchestrator runs Phases 1–3, 7, 8 plus receipt acceptance; the worker runs Phases 4–6. Phase 2 is always orchestrator-side and consists of derivation re-checks — re-running the verification commands the prior task's receipt journaled and comparing against the claim, cheap shell output rather than code reading. The continuity invariant is unchanged from existing doctrine (closeout is fresh-session-resumable at every boundary); dispatch enforces it by construction, because the worker that held the working set no longer exists.

**Orchestrator conduct.** The orchestrator reads only spec, journal, and receipts; runs shell only for derivation re-checks and git inspection; rejects any receipt whose re-check fails; treats a receipt-borne amendment trigger as a batch stop; and never "quick-fixes" — a failed or partial task is re-dispatched or stopped, never silently completed by the orchestrator (which would rebuild the accumulating session dispatch removes).

**Worker brief.** The worker starts cold and inherits nothing from the orchestrator's transcript; the brief carries the task ID and quoted task text, the artifact paths (including `SPEC_REPO_ROOT` when multi-repo), the referenced spec sections, the file list and DoD, the model floor, the conventions pointer, and the receipt schema with its cap. The worker orients from disk (Phase 1 discipline scoped to one task), does not invoke `spec-*` skills (its Phase 4–6 rules ride the worker agent definition, not a re-loaded skill), writes the journal entry first-hand, and makes the paired commits. Single delegation level: workers may not spawn further subagents.

**Why this design.** A mode, not a new skill: the contract is identical in both modes; only the executor changes. Derivation re-check (orchestrator conduct rule 3) is load-bearing — it preserves the rework-prevention property (`CLAUDE.md`'s "mechanically re-derivable claims") across the delegation boundary at near-zero context cost. First-hand journaling and committing from the worker keep the evidence chain unbroken: the entity that ran the tests writes the record.

**Reversibility.** No format change: a dispatch-produced journal is indistinguishable from an inline one except the `Executed by` field (`inline | worker(<agent-definition-name>, <model>)`), now in the SKILL.md Phase 6 journal template. Setting `EXECUTION: inline` fully restores the §5.1–§5.8 single-session behavior.

**Cross-reference.** The receipt schema (referenced above) is specified in the receipt-schema support file `receipt-schema.md` in the skill directory, deployed alongside SKILL.md; the `Executed by` journal field is in the SKILL.md Phase 6 template (dispatch-execution §5.4–5.5). The Phase 8 context budget (dispatch-execution §5.6) is now wired into §5.8 and the SKILL.md Phase 8 section; operator cues (dispatch-execution §5.9) are described in §5.13 and shipped as the SKILL.md OPERATOR CUES section.

### 5.13 Operator cues at human-in-the-loop boundaries

**Purpose.** Describe the master's OPERATOR CUES section: every boundary that hands control to the operator ends with a fixed-format WHAT HAPPENED / YOUR MOVE / HOW cue block whose HOW line is a pre-filled next invocation. Authoritative design source: [dispatch-execution spec §5.9](../20260705-dispatch-execution/architecture.md).

**Behavior.** Emitted at every designed stop and pause across both execution modes — Phase 7 checkpoint stops, Phase 8 pauses, blocker escalations, amendment triggers, model-floor conflicts, production-touching-action stops, and context-budget breaches. Each cue names the decision only the operator can take and pre-fills the exact next skill invocation plus where to look first (journal entry / spec section / receipt).

**Why this design.** Under default autonomy (dispatch-execution's posture) the operator's touches become rarer and colder; a stop without a cue converts autonomy's savings into operator re-orientation cost. The cue is the journal's next-task-pointer principle extended to the human side of the loop.

**Reversibility.** Additive: cues are emitted at boundaries, never stored in artifacts, so a cue-bearing session produces artifacts identical to one without. The journal's next-task pointer stays the durable handoff.

## 6. Non-functional Requirements

| NFR | Requirement | Source |
|---|---|---|
| **Portability** | Skill functions when installed at `.agents/skills/spec-execute/` and works against unrelated host repos that lack methodology-specific upstream artifacts (degrades cleanly). No runtime dependency on host-repo files for the skill itself to function. Workflow + Orientation Report shape + journal-entry template bundled in SKILL.md. Optional inputs (`SPEC_REPO_ROOT`, `SPEC_TARGET_BRANCH`, `SESSION_GOAL`, `TIME_BUDGET`) degrade cleanly when absent. | [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) |
| **Closeout discipline** | Each task closes out completely (spec + journal + next-task pointer + commit) before any next task is opened. No batched session-end closeouts. | [SKILL.md OPERATING PRINCIPLES §2](../../.agents/skills/spec-execute/SKILL.md) |
| **Re-anchoring** | Phase 1 re-reads the spec rather than relying on memory of earlier-in-session reads. | [SKILL.md OPERATING PRINCIPLES §6](../../.agents/skills/spec-execute/SKILL.md) |
| **No silent deviation** | Spec contradictions route through Amendment Protocol; no in-place workaround. | [SKILL.md OPERATING PRINCIPLES §3](../../.agents/skills/spec-execute/SKILL.md) |
| **Evidence per DoD item** | Phase 5 verification produces evidence (test output, lint output, file paths, log lines) per item, not summary. | [SKILL.md OPERATING PRINCIPLES §4](../../.agents/skills/spec-execute/SKILL.md) |
| **Pause discipline** | Four explicit pause points (Phase 1 approval, Phase 3 blockers, Phase 7 checkpoint, Phase 8 continuity). No unilateral continuation past Phase 8. | [SKILL.md PHASE 1/3/7/8](../../.agents/skills/spec-execute/SKILL.md) |
| **Multi-repo discipline** | When `SPEC_REPO_ROOT` is set, paired commits are mandatory at each task boundary. Code commit + spec/journal commit reference the same task ID; neither ships without the other. Detected in Phase 1; enforced in Phase 4/6. | [session-economy spec §5.2](../20260514-session-economy/architecture.md) (Phase 1 multi-repo detection — §5-enumerated); [session-economy spec §1 + §3](../20260514-session-economy/architecture.md) and commit `5ce4024` (Phase 4/6 paired-commit strengthening — narrative-sourced, not enumerated in session-economy §5); [SKILL.md PHASE 1/4/6](../../.agents/skills/spec-execute/SKILL.md) |
| **Token economy** | Phase 8 names token economy as a first-class factor alongside cognitive freshness. Long sessions / large prompt-cache misses are surfaced explicitly; a fixed 80,000-token context budget converts the factor into a mechanical trigger (fresh-session in inline mode / stop for operator in dispatch mode). | [session-economy spec §5.1](../20260514-session-economy/architecture.md); [dispatch-execution spec §5.6](../20260705-dispatch-execution/architecture.md); [SKILL.md PHASE 8](../../.agents/skills/spec-execute/SKILL.md) |
| **Amendment routing** | Drift findings route to `spec-amend` (proposing in `spec-execute`, applying in `spec-amend`). No inline amendments. | [SKILL.md AMENDMENT PROTOCOL](../../.agents/skills/spec-execute/SKILL.md) |
| **Resumability (human)** | Every operator-facing stop emits a fixed-format cue; an operator returning cold can act on a stopped run from the cue plus the journal's latest entry alone. | [dispatch-execution spec §5.9](../20260705-dispatch-execution/architecture.md); [SKILL.md OPERATOR CUES](../../.agents/skills/spec-execute/SKILL.md) |
| **Voice fidelity** | Imperative for protocol rules, plain declarative for reports and journal entries, no marketing language. | Inherited from N=2/N=3 |
| **Citation discipline** | Repo-internal citations at point of claim. Section-heading citations point to the heading line, not the body's first line (carried from N=2 / N=3 CP-1 advisories). | [N=3 journal "Pattern for N=4 #3"](../20260518-spec-write-skill/journal.md) |
| **Self-containment** | Each Orientation Report and journal entry reads independently of the chat that produced it. No "as we discussed"; every named task, file, or pattern defined or linked. | [SKILL.md OPERATING PRINCIPLES §6](../../.agents/skills/spec-execute/SKILL.md) (re-anchor at boundaries implies self-contained handoffs) |

## 7. Implementation Sequencing

The skill ships. This spec is descriptive, not implementation-planning. The sequencing for *adopting this spec* is:

1. **Spec authored** (this document and its sibling journal, committed together as a paired commit).
2. **CP-1 review** (§9): retroactive spec faithfully describes the shipping skill. Triggered by a fresh `/spec-review` session against this spec's CP-1 — operator chose the sequenced path (per N=1 / N=2 / N=3 precedent), deferring CP-1 to a separate session for cleaner authorship-vs-review separation.
3. **CP-2 drift audit** (§9): batched with the four sibling quintet specs per [docs/retroactive-spec-strategy.md §"Drift mitigation"](../../docs/retroactive-spec-strategy.md). Any divergence routes to `/spec-amend` (not silent edit).
4. **Adoption complete**: this spec becomes the reviewable contract for future SKILL.md changes; subsequent SKILL.md edits follow the Amendment Protocol via `/spec-amend`.

There is no Phase B "build the skill" — the skill is already built. There is no downstream feature spec for this skill; this spec is terminal for the skill's specification, except for any amendment-driven follow-up.

> Note: This section deliberately differs from a feature spec's Task Breakdown. Design specs do not decompose into atomic dev tasks; retroactive design specs particularly do not, because the implementation predates them.

## 8. Validation Approach

| Approach | What it validates |
|---|---|
| **Stakeholder review** | Eric (operator) reviews this spec for fidelity to intent. CP-1 is the gate. |
| **Drift audit** | Mechanical comparison of SKILL.md commitments to this spec's commitments. CP-2 is the gate. Output is a divergence list (possibly empty). Batched with the four sibling quintet specs. |
| **Predecessor cross-check** | [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438 is the skill's design-rationale source (prompt artifact + design notes block). Every spec-side commitment in §5 of this document traces to either a behavior in SKILL.md (current) or to a recommendation in the predecessor doc (rationale). Gaps between predecessor and SKILL.md are *evolution* (trilogy commit `80000b1`, session-economy commit `5ce4024`, path-convention commit `189c6cc`), not drift. CP-2 reads this distinction. |
| **Sibling design-spec cross-check** *(new at N=4)* | [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) is the *sibling* design spec authoritative for current behavior added after the predecessor. Two attribution shapes apply. **Shape (i) — §5-enumerated commitments:** retro §5.1 (Phase 1 multi-repo detection) → session-economy §5.2; retro §5.8 (Phase 8 token economy) → session-economy §5.1. Both must match. **Shape (ii) — narrative-sourced commitments:** retro §5.4 and §5.6 (Phase 4/6 paired-commit strengthening) → session-economy §1 Overview + §3 Background (which acknowledge pre-existence and strengthening) + commit `5ce4024` as the implementation event. Phase 4/6 strengthening is not enumerated in session-economy §5. CP-2 reads cross-spec consistency under both shapes, not just SKILL.md-vs-this-spec consistency. |
| **Downstream consumption** | The skill has been used to execute the feature specs at [specs/20260515-spec-path-convention/feature.md](../20260515-spec-path-convention/feature.md), [specs/20260514-session-economy/feature.md](../20260514-session-economy/feature.md), and the findings-pipeline cluster. The journals of those specs are evidence the eight-phase workflow and per-task closeout discipline produce reviewable, revertible task boundaries. |

> Note: This section deliberately differs from a feature spec's Test Strategy. Design specs are validated by review, audit, predecessor cross-check, sibling-spec cross-check, and downstream consumption — not by automated test coverage.

## 9. Review Checkpoints

### CP-1 — Retroactive spec faithfully describes the shipping skill

**Status:** pass with comments on 2026-05-18 by Claude (AI assistant) on behalf of Eric Wasgatt — see [re-review journal entry](./journal.md). Initial verdict (2026-05-18, "changes requested") routed four blockers to amendment 2026-05-18-1 (commit `b61cb3f`); this re-review confirms the citation pattern is now correctly attributed under the two-shapes framing. Three advisories carry forward unaddressed (out of amendment scope).

**Trigger.** This spec and its journal are committed; the operator invokes `/spec-review` against this spec's CP-1 in a fresh session.

**Review focus.**
- Every commitment in §4, §5, and §6 corresponds to behavior actually present in [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md).
- No commitment in this spec contradicts the shipping SKILL.md.
- The **Atomic-Skill Portability Principle** is correctly characterized as a binding constraint (§3, §6) including degradation behavior when optional inputs are absent.
- The **predecessor doc** is correctly distinguished as authoritative-for-design-rationale-not-current-behavior (§3 Background, §14 Inspirational).
- The **session-economy spec** is correctly distinguished as a *sibling design spec* authoritative for current behavior added after the predecessor — cited Authoritative in §14. Two attribution shapes apply (per §8 Validation Approach): **(i) §5-enumerated** — retro §5.1 (Phase 1 multi-repo detection) maps to session-economy §5.2; retro §5.8 (Phase 8 token economy) maps to session-economy §5.1; both mappings hold. **(ii) Narrative-sourced** — retro §5.4 and §5.6 (Phase 4/6 paired commits) cite session-economy §1 Overview + §3 Background plus commit `5ce4024`, because the Phase 4/6 strengthening is not enumerated in session-economy §5.
- The eight phases in §5 (§5.1–§5.8) and the Amendment Protocol (§5.9) match the shipping SKILL.md's phase structure and Amendment Protocol section.
- §13 OQ-1 (Phase 7 ↔ Phase 8 ordering) and §13 OQ-2 (multi-repo lifecycle mismatch) are named with full Question / Analysis / Leaning / Owner / Watch items / Anti-goals structure.
- The spec is self-contained per the Operating Principles in [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md).
- Section-heading citations point to the heading line, not the body's first line (Pattern-for-N=4 #3 carried forward from N=3).
- The portability rule for links is honored: no `~/.claude/skills/...` references, no absolute filesystem paths.

**Exit criteria.**
- Reviewer issues a verdict of `pass`, `pass with comments`, or `changes requested` per the structured format declared in [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- All `[blocker]` findings (if any) are resolved or escalated to `/spec-amend`.
- Verdict is written back to this spec's §9 (status line) and to the journal.

### CP-2 — Drift audit complete (batched)

**Status:** pass with comments on 2026-05-18 by Claude (AI assistant) on behalf of Eric Wasgatt — see [CP-2 review journal entry](./journal.md) and [batch journal N=4 entry](../20260518-cp2-batch-audit/journal.md). Five advisory findings, zero blockers; routing tally amend-spec ×3 (D-1, D-2, D-5), amend-SKILL.md ×2 (D-3, D-4), accept ×0. All five amendments landed via [2026-05-18-2](./journal.md) (D-1 → §5.4), [2026-05-18-3](./journal.md) (D-2 → §5.9), [2026-05-18-4](./journal.md) (D-5 → §5.6), [2026-05-18-5](./journal.md) (D-3 → SKILL.md preamble), [2026-05-18-6](./journal.md) (D-4 → SKILL.md preamble). Brief re-verification on 2026-05-18 confirms (a)-route divergences closed in spec body — see [CP-2 re-verification journal entry](./journal.md). **Checkpoint closed.**

**Trigger.** CP-1 of this spec passes, AND CP-1 of the two remaining quintet specs (`spec-review`, `spec-amend`) passes, AND CP-1 of [spec-design](../20260518-spec-design-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND CP-1 of [spec-write](../20260518-spec-write-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND project-constitution's CP-2 has either run or been folded into the batch per [docs/retroactive-spec-strategy.md OQ-1](../../docs/retroactive-spec-strategy.md). The narrowed remaining condition is "two sibling quintet CP-1s + project-constitution CP-2."

**Review focus.** A line-by-line audit of [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) against this spec's §4, §5, §6, and §12. The auditor enumerates each divergence: a behavior present in SKILL.md but not committed in the spec, or a commitment in the spec absent from SKILL.md. Cross-skill drift patterns (e.g., four skills citing the Atomic-Skill Portability Principle correctly and one quietly not; or session-economy commitments inconsistently propagated across the quintet) are explicitly in scope by virtue of the batch context. The cross-spec consistency check between this spec and the [session-economy spec](../20260514-session-economy/architecture.md) is a CP-2 line item, under two shapes (per §8 Validation Approach): **(i) §5-enumerated mappings** — retro §5.1 ↔ session-economy §5.2; retro §5.8 ↔ session-economy §5.1. **(ii) Narrative-sourced attributions** — retro §5.4 and §5.6 cite session-economy §1 + §3 plus commit `5ce4024`, because Phase 4/6 paired-commit strengthening is not enumerated in session-economy §5. CP-2 verifies both shapes.

**Exit criteria.**
- Divergence list produced (possibly empty).
- For each divergence, a routing decision: (a) amend the spec to reflect SKILL.md behavior, (b) amend SKILL.md to match the spec, or (c) accept as a known minor discrepancy with rationale recorded in the journal.
- No silent edits to either artifact.
- Outcome recorded in this spec's journal as the closing entry of the retroactive-spec adoption.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Spec drifts from shipping SKILL.md silently after adoption. | Medium over time | High — the spec stops being trustworthy as a contract | Amendment Protocol via `/spec-amend`; CP-2 establishes the initial baseline; subsequent SKILL.md changes are spec-amended in the same change | Eric / future maintainers |
| Spec is wrong (misdescribes the skill, omits material behavior, asserts commitments the skill does not honor). | Low–Medium | Medium — early review catches it | CP-1 review with explicit "faithfulness to shipping SKILL.md" as the central focus area | Reviewer (CP-1) |
| The two-source structure (predecessor + sibling design spec) causes CP-2 false positives — auditor reads behavior present in SKILL.md but absent from the predecessor as drift, missing that the session-economy spec is the authoritative source. | Medium | Medium | §3 Background and §14 References explicitly distinguish the two sources; §8 Validation Approach declares both cross-check rows; CP-1 review focus includes the session-economy-spec consistency check; CP-2 reviewer reads §3 / §8 / §14 before walking divergences. | CP-2 auditor |
| §13 OQ-1 (Phase 7 ↔ Phase 8 ordering) resolves silently in a real session — operator does the right thing without recording the decision, and the spec stays open forever. | Medium | Low–Medium | OQ-1's Watch items capture revisit conditions; the Amendment Protocol creates a route when the gap surfaces as friction; CP-2 reviewer flags if the OQ is still open after >N sessions and ramp toward resolution is warranted. | Eric (per-session); future amendment session |
| §13 OQ-2 (multi-repo lifecycle mismatch) is never exercised because `ai-tools` is single-repo; the OQ ossifies as theoretical. | Medium | Low | OQ-2's Watch items name the first real multi-repo project as the trigger condition; until then, the OQ is documented but inactive — which is honest reporting, not a failure. | Eric; first multi-repo `spec-execute` session |
| N=4 patterns over-codify, foreclosing better N=5+ patterns at the sibling `spec-review` / `spec-amend` specs. | Low | Medium | §2 Non-goals explicitly disclaims template-establishing intent; this spec's journal records "Pattern for N=5" callouts that are *candidates*, not declarations; the `docs/retroactive-spec-pattern.md` decision is deferred to N=5 close per N=3 journal. | Future retroactive-spec sessions; operator at N=5 close |
| The predecessor doc is treated as authoritative for current behavior, causing CP-2 to flag every recommendation-that-evolved as drift. | Medium | Low–Medium | §3 Background distinguishes the two; §8 Validation Approach names the three evolution-explaining commits (`80000b1`, `5ce4024`, `189c6cc`); CP-2 auditor reads §3 before walking divergences. Same mitigation pattern as N=3. | CP-2 auditor |
| Phase 8 token-economy factor is the only quintet commitment with a *sibling design spec* as its authoritative source — CP-2 doesn't know how to read this. | Medium | Low | §3 Background and §8 Validation Approach explicitly name the session-economy spec as Authoritative-for-current-behavior; §6 NFR row "Token economy" cites the session-economy spec directly; CP-2 review focus includes the session-economy-spec cross-check explicitly. | CP-2 auditor |

## 11. Adoption Path

The spec is adopted in three steps, matching N=1 / N=2 / N=3:

1. **Commit the spec and journal** as a paired commit. The journal is the durable record of this session's decisions and observations, structured to be mined by sessions 4–5 of the legacy quintet.
2. **CP-1 review** in a fresh session (sequenced, per operator's choice): invoke `/spec-review` against this spec's CP-1.
3. **CP-2 drift audit** in the batched quintet-CP-2 session: per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md).

After adoption, the spec is a *living contract*. Edits to SKILL.md follow the Amendment Protocol: stated section being amended, before/after diff, reason and impact, explicit operator approval, journal entry. Per the [N=1 amendment 2026-05-17-1 N=2 mining note](../20260517-project-constitution-skill/journal.md), amendments that touch a *class* of references (paths, citations, vocabulary) must scan the entire spec at Phase 1 Orient, not just the locations called out by the triggering finding.

### Reversibility

The spec can be retired without affecting the skill. The SKILL.md remains the canonical implementation. Retirement would be unusual (it would be the methodology rejecting its own dogfooding convention for `spec-execute` specifically) but mechanically clean: delete the spec directory and record the retirement in `roadmap.md`.

### Cross-session knowledge transfer

This spec's [journal.md](./journal.md) records validation/refinement/rejection outcomes for each "Pattern for N=4" callout from the [N=3 journal](../20260518-spec-write-skill/journal.md), plus new "Pattern for N=5" callouts for sessions 4–5. The corrective on N=3's prediction that "trilogy-extended skills likely have thin or no predecessor" is recorded — `spec-execute` has a rich predecessor in the shared doc, *and* a sibling design spec (session-economy) authoritative for current behavior. Sessions 4–5 mine this journal as primary input.

## 12. Out of Scope

- **Resolving §13 OQ-1 (Phase 7 ↔ Phase 8 ordering).** The OQ is named; resolution routes to a future amendment session when watch conditions trigger.
- **Resolving §13 OQ-2 (multi-repo lifecycle mismatch).** The OQ is named; resolution requires real multi-repo execution data before formalization.
- **Cross-skill amendment coordination** when Phase 4 / Phase 6 surfaces drift that requires coordinated edits across the feature spec, its upstream design spec, and possibly SKILL.md across skills. Named at [docs/retroactive-spec-strategy.md OQ-3](../../docs/retroactive-spec-strategy.md) and at [N=3 §12](../20260518-spec-write-skill/architecture.md). Resolution lives in the `spec-amend` retroactive spec at session 5.
- **Multi-repo `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` mechanics under multi-target conditions** (one feature spec spanning multiple downstream code repos; spec-repo lifecycle reflected in adopter repos). The single-repo case is the common case in `ai-tools`; the multi-repo two-repo case is the architecturally-committed configuration. Multi-*target* (>2 repos) mechanics are not bounded by this spec. Mirrors N=2 / N=3's identical disposition.
- **Resolving the format-question-prompt gap** ([N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md)). Methodology-wide; not re-raised here.
- **The `docs/retroactive-spec-pattern.md` decision.** Deferred at N=3 close (operator-confirmed); revisited at N=5 close, not here. This spec's journal records its inputs to the eventual decision but does not pre-empt it.
- **Redesign of the `spec-execute` skill.** Redesign routes to a new design spec under amendment governance.
- **Modification of the shipping SKILL.md.** Only `/spec-amend` touches SKILL.md.
- **A template for the two remaining legacy-quintet retroactive specs** (`spec-review`, `spec-amend`). Cross-session scaffolding, if any, derives from journal mining; this spec body does not declare a template.
- **External-claim verification beyond repo-internal citation.** Light verification was adopted for this spec; the heavy-verification path exists in the methodology but is not exercised by this spec's text.
- **Constitution-amendment ceremony.** Inherited from [N=1 OQ-1](../20260517-project-constitution-skill/architecture.md#L274) and tracked at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md). Out of scope here.

## 13. Open Questions

### OQ-1 — Phase 7 ↔ Phase 8 ordering when both fire at the same task boundary

**Question.** Current SKILL.md treats Phase 7 (Checkpoint Gate) as a hard stop that hands off to `/spec-review` and waits for a verdict; Phase 8 (Session Continuity Check) only runs when no checkpoint is triggered. But the operator may legitimately want to pause-and-defer-to-fresh-session *after* a checkpoint passes (checkpoint resolved → session is long → next task opens different scope). The interaction between the two pause points is unspecified.

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) Run Phase 8 *after* a Phase 7 checkpoint passes — always, regardless of which fired first | Phase 7 passes → automatically transition into Phase 8 → continuity check fires with checkpoint-passed state as additional input. | Covers the legitimate "checkpoint passed, but pause anyway" case. Cost: Phase 8 becomes the universal task-boundary check; the "skip Phase 8" rule from current SKILL.md still applies under explicit override. |
| (b) Make Phase 8 conditional on a configurable post-checkpoint switch | New input `RUN_PHASE_8_AFTER_CHECKPOINT: <yes|no>` (default `yes`) controls whether Phase 8 fires after a passed checkpoint. | Maximum flexibility. Cost: another input to remember; complexity in the operator interface. |
| (c) Accept the silent-resolve-by-operator-judgment as current convention | Operator who wants to defer after a checkpoint says "let's pick this up tomorrow" between Phase 7 and the next Phase 1 invocation. The skill is silent on the transition. | Zero infrastructure. Cost: the deferral is invisible — no journal entry, no recommendation rationale captured. Mirrors current behavior. |
| (d) Treat Phase 7 verdict-confirmation as the trigger for Phase 8 | When the operator confirms checkpoint passage, the skill emits a Phase 8 recommendation in the same response. | Compact. Cost: blurs the boundary between checkpoint and continuity; reviewers reading the journal see a mixed entry. |

This OQ has a structural relationship to §13 OQ-2 (multi-repo lifecycle mismatch) only weakly: the multi-repo paired-commit state intersects with checkpoint gating in that a passed checkpoint with an incomplete paired commit is a different state than a passed checkpoint with a complete paired commit. Cross-reference noted but not load-bearing.

**Leaning.** **(a) is the strongest candidate.** Rationale: Phase 8's discipline (token economy, drift risk, coupling) is independent of whether a checkpoint just passed; the legitimate deferral case is real; and the existing override mechanism (operator's "run the full set without checking in") still applies under (a). (b) adds operator-interface complexity for marginal benefit. (c) preserves the gap. (d) trades clarity for compactness. **Recommended direction: (a)**, but no formal commitment until either watch condition triggers.

**Owner.** A future `/spec-amend` session triggered by either of the watch items below.

**Watch items.**
- A real session encounters this state and the operator's resolution feels improvised (signal: the gap creates friction in journal authoring or downstream review).
- A future quintet retroactive spec (`spec-review` at session 4 or `spec-amend` at session 5) raises a parallel question about phase interaction between sibling skills (signal: phase-interaction is a recurring methodology issue, not just a `spec-execute` issue).

**Anti-goals.**
- Do not auto-resolve by amending SKILL.md silently before CP-2; the gap is methodology-visible and routes through governance.
- Do not treat (c) as the de-facto resolution by writing journal-entry conventions that bake it in.

### OQ-2 — Multi-repo lifecycle mismatch when paired commits half-fail

**Question.** SKILL.md's Phase 6 commits to paired commits in the multi-repo case ("do not declare task complete until both commits exist"), but is silent on the mid-state when the spec-repo commit succeeds and the codebase-repo commit fails (or vice versa). What is the recovery protocol? Who owns the rollback decision?

**Analysis.** The failure mode is real: network failures, pre-commit-hook differences between repos, divergent permissions, or a working tree that becomes dirty between the two commits all produce the half-committed state. Three candidate resolutions:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) Add a recovery sub-procedure to Phase 6 | New §"Recovery from half-failed paired commits" prose: detection (Phase 1's multi-repo state check now reads commit pairing as well as branch state), rollback (revert the successful commit; record in journal), re-attempt (after fix). | Concrete and discoverable. Cost: phase complexity increases; recovery may not generalize cleanly across git hosting providers. |
| (b) Treat half-committed state as a Phase 3 amendment trigger | A half-committed state surfaced in Phase 1 is treated as drift; the operator is routed to `/spec-amend` to record the half-state and decide rollback-vs-forward. | Uses existing machinery. Cost: Amendment Protocol is for spec-vs-code contradictions, not for commit-state recovery — semantic stretch. |
| (c) Accept operator judgment with explicit "you fix this" framing in SKILL.md | Add a one-line note in Phase 6 (multi-repo case): "If one commit fails after the other succeeds, do not proceed to Phase 7; the operator owns the rollback decision." | Minimal infrastructure. Cost: the mid-state recovery procedure stays implicit; new operators may not know how to recover. |

This OQ has weak structural dependency on OQ-1 (the half-committed state interacts with checkpoint gating only at the boundary case). No structural dependency on the format-question gap from N=2.

**Leaning.** **(a) is the strongest candidate when multi-repo is exercised regularly**; **(c) is the strongest candidate while `ai-tools` remains single-repo and the failure mode is theoretical**. Until real multi-repo execution data surfaces, the OQ remains open with (c) as the de-facto convention and (a) as the documented escalation path.

**Owner.** A future `/spec-amend` session triggered by the first watch item below.

**Watch items.**
- A first real multi-repo project executes against the methodology (signal: the failure mode becomes empirically observable; resolution direction can be chosen with data).
- A first half-committed state actually occurs in any project using `spec-execute` (signal: the failure mode is no longer theoretical).
- A sibling skill (`spec-amend`) at session 5 surfaces cross-skill amendment coordination as a related concern (signal: paired-commit recovery may share mechanism with cross-spec amendment).

**Anti-goals.**
- Do not pre-emptively author a recovery sub-procedure (option a) in this spec; commitment without data risks freezing the wrong shape.
- Do not collapse this OQ into [strategy-doc OQ-3 (cross-skill amendment coordination)](../../docs/retroactive-spec-strategy.md); the failure modes overlap but are not the same — half-committed state is a transient git condition, cross-skill amendment is a structural multi-spec condition.

## 14. References

### Authoritative

- [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) — the shipping skill. Authoritative for behavior.
- [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) — sibling design spec authoritative for current behavior added after the predecessor: Phase 8 (Session Continuity Check, token-economy factor), Phase 1 multi-repo detection, Phase 4/6 paired-commit discipline.
- [specs/tech-stack.md](../tech-stack.md) — methodology constraints, including the Atomic-Skill Portability Principle binding on this skill.
- [specs/mission.md](../mission.md) — `ai-tools` mission; defines audience and in/out of scope for the methodology.
- [specs/roadmap.md](../roadmap.md) — `ai-tools` roadmap; lists `spec-execute` as a Phase 1 deliverable.

### Inspirational

- [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438 — the `spec-execution-prompt.md` artifact + companion design notes that became this skill. Authoritative for the skill's *design rationale*; not authoritative for current behavior (which has evolved through commits `80000b1`, `5ce4024`, `189c6cc`). Same shared predecessor doc that N=3 (`spec-write`) cited at lines 17–225; the doc covers three skills.
- [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) — N=1 retroactive design spec; original structural source.
- [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) + [journal.md](../20260518-spec-design-skill/journal.md) — N=2 retroactive design spec; predecessor-distinction discipline originates here.
- [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) + [journal.md](../20260518-spec-write-skill/journal.md) — N=3 retroactive design spec; closest-sibling structural source. "Pattern for N=4" callouts validated/refined/rejected in this N=4 journal.
- [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) — strategy doc for the legacy-quintet retroactive specs; orientation material for this session. Identifies this session as the N=3 robustness check.
- [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) — negative-signal source: modifies `SPEC_PATH` examples (commit `189c6cc`) but does not architecturally describe this skill.
- [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md), [specs/20260517-finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) — sibling skills with feature specs (authored before the skills shipped); provided the `<skill-name>-skill` directory-slug convention used here.
