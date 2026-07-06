# Dispatch Execution — Architecture and Protocol Specification

> Status: Approved — CP-1 closed 2026-07-05
> Date: 2026-07-05
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agent sessions executing or amending the skills. Written for the broadest reader: a cold AI agent session with no memory of the originating conversation.

## 1. Overview

Dispatch execution is an execution topology for the `spec-*` skill family. Under it, a spec-execute session may run as a **thin orchestrator** that delegates each task's implementation to a **disposable worker subagent** spawned at the task's declared model floor. The orchestrator never opens code files; workers die at task closeout; all continuity between tasks lives in the spec and journal, never in a session's context window.

The architectural commitment is twofold. **Session context is not the carrier of batch state** — the artifacts already are (existing doctrine: every closeout must be fresh-session-resumable); dispatch execution makes that structural rather than advisory. And the default operating posture is **autonomous, dispatched**: runs proceed unattended, escalating to the operator only at planned pauses (checkpoints and other designed stops), on interruption, or to resolve unexpected scenarios. The two defaults reinforce each other — dispatch keeps autonomous runs cheap and flat; autonomy is what makes dispatch pay. The motivating evidence is operational: batches run inline accumulate context roughly linearly per task while paying re-read cost per *turn*, producing sessions above 150k tokens whose later tasks each cost more than the entire batch should. Measured on 2026-07-05: 55% of the operator's usage ran at >150k context, with `/spec-execute` alone accounting for 33% of usage.

## 2. Goals and Non-goals

### Goals

- Batch cost scales approximately linearly with task count: an orchestrator's context grows by a capped receipt (~1–2k tokens) per task, not by the task's working set.
- Default-autonomous, default-dispatch operation: a run proceeds unattended until a *designed* stop (review checkpoint, blocker, amendment trigger, floor conflict, production-touching action, budget breach), an operator interrupt, or an unexpected scenario the contract does not cover. Tight supervision (`AUTONOMY: task`, `EXECUTION: inline`) remains available as an operator-selected restriction.
- Verification discipline survives delegation: every done-claim in a receipt is mechanically re-derivable, and the orchestrator re-derives it before accepting the receipt.
- Model floors survive delegation: the worker's model is pinned at spawn to the task's declared floor, and the journal records who executed what, at which model, re-derivably.
- Delegation is a named, versioned artifact (an **agent definition**), not per-batch ad-hoc prompt engineering.
- Skill masters remain harness-agnostic; all harness-specific mechanics live in the new agent-definitions deliverable class.
- Inline execution remains fully supported as an operator-selected restriction for tightly supervised work.

### Non-goals

- **No new skill.** Dispatch is a mode of `spec-execute` (and an option within `spec-review`); the skill count does not change.
- **No headless/CLI driver.** A fresh-session-per-task driver loop (`claude -p` or equivalent) is a compatible future tier, deliberately out of scope (§12).
- **No parallel fan-out.** Workers run serially in this iteration (§13 OQ-3).
- **No change to spec formats, floor vocabulary, or checkpoint semantics.** The *stop set* stays operator-owned and may never be loosened by the agent; what changes is the default between stops (autonomous rather than per-task approval — §5.1).
- **No cloud or scheduled execution.** Consuming projects commonly touch live local state that remote environments cannot reach; execution stays on the operator's host.

## 3. Background and Constraints

### Prior state

`spec-execute` (governed by [the spec-execute architecture spec](../20260518-spec-execute-skill/architecture.md), Approved 2026-05-18) executes single-lane: the invoking session performs Phases 1–8 itself. Phase 4 permits delegating *parts* of a task to subagents at-or-above floor; Phase 8 weighs session continuity qualitatively at each boundary. Under `AUTONOMY: checkpoint` (batch mode, landed in commit `6bef6f8`), tasks run back-to-back in one session and context accumulates without bound until a checkpoint stops the run.

An earlier spec, [session-economy](../20260514-session-economy/architecture.md) (2026-05-14), captured first-iteration ideas on session economy and added Phase 8's token-economy factor. It is historical context only; this design derives from current harness capabilities and current operational evidence, not from that spec's framing.

The repo constitution ([mission](../mission.md), [tech-stack](../tech-stack.md), [roadmap](../roadmap.md)) establishes this repo as the master home of the skill family. The family serves multiple consuming projects across the operator's work and hobby contexts; this spec names none of them and assumes none is available. Consuming repos may carry their own operating doctrine built on the same principles the skills encode, per `CLAUDE.md`'s design bar: mechanically re-derivable claims (rework prevention); batch-by-default with operator-owned stops (batch autonomy); starve context, not verification (token economy).

### Constraints (cited)

- **Frontmatter portability** (`CLAUDE.md`, this repo): skill masters carry only `name`/`lastUpdated`/`description`. Harness-specific keys are forbidden in masters. Consequence: dispatch mechanics must be expressed in the skill's prose contract, and worker configuration must live in a harness-facing artifact class.
- **Harness capabilities** (verified against the running Claude Code harness, 2026-07-05; see also the Claude Code subagents documentation, <https://code.claude.com/docs/en/sub-agents>):
  - The Agent tool accepts a per-spawn `model` override with values `haiku` / `sonnet` / `opus` / `fable` — a 1:1 match for the model-floor tiers used in consuming specs.
  - Agent types are defined in `.claude/agents/*.md` files whose frontmatter sets model, reasoning effort, and tool access.
  - Subagents start cold (no conversation inheritance) and return only their final message to the parent.
  - These are harness-version-dependent facts (§13 watch items; §10 R-3).
- **Cost model** (Anthropic prompt-caching reference, verified 2026-07-05 against the claude-api reference cached 2026-06-24): prompt-cache reads are ~0.1× input price with a 5-minute default TTL; every assistant turn re-reads the full conversation. A per-task worker context that dies at closeout is therefore strictly cheaper than the same work appended to an accumulating session.
- **Floor policy** (`spec-execute` Operating Principle 8): work executes at or above the task's declared floor; floors are never laundered through subagents; only the user sets `AUTONOMY`.

### Dependencies

- Downstream amendments target the governing specs and masters of `spec-execute` and `spec-review`, plus deploy copies under `~/.claude/skills/` (described here as: the harness's user-level skills directory; the deploy-sync rule in `CLAUDE.md` governs).
- The pilot (§8) depends on a live consuming spec with declared per-task floors, selected operator-side at pilot time and recorded in this spec's journal.

## 4. Architecture

### Topology

```text
                 ┌────────────────────────────────────────┐
                 │  ORCHESTRATOR SESSION (thin)           │
                 │  model ≥ decision tier for the batch   │
                 │  Phases 1–3, 7, 8 + receipt acceptance │
                 │  never opens code files                │
                 └───────┬───────────────────▲────────────┘
        worker brief     │                   │   receipt (capped)
        (per task)       ▼                   │
                 ┌────────────────────────────────────────┐
                 │  WORKER (disposable, per task)         │
                 │  model = task's declared floor         │
                 │  Phases 4–6: implement, verify, close  │
                 │  commits code + spec/journal updates   │
                 └───────┬───────────────────▲────────────┘
                         │ writes            │ orients from
                         ▼                   │
                 ┌────────────────────────────────────────┐
                 │  ARTIFACTS (source of truth)           │
                 │  spec + journal + branch state         │
                 │  carry ALL inter-task continuity       │
                 └────────────────────────────────────────┘
```

### Vocabulary (defined here, used consistently below)

- **EXECUTION mode** — a `spec-execute` input: `inline` (the session implements, current behavior) or `dispatch` (the session orchestrates).
- **Orchestrator session** — a `spec-execute` session running in dispatch mode. Owns orientation, pre-flight verification, clarification, checkpoint gates, and continuity decisions (Phases 1–3, 7, 8). Prohibited from implementation work and from reading code files.
- **Worker** — a subagent spawned for exactly one task, at a model meeting the task's floor, from a named agent definition. Owns Phases 4–6 for its task, including the paired commits and the journal entry. Dies at closeout.
- **Worker brief** — the prompt the orchestrator passes at spawn: the task's full spec text, referenced spec sections, file list, DoD, conventions pointer, and artifact paths. The worker orients from artifacts, never from the orchestrator's transcript.
- **Receipt** — the worker's final message, in a fixed schema with a hard length cap (§5.4). The only thing that enters the orchestrator's context per task.
- **Derivation re-check** — the orchestrator re-running the receipt's journaled verification command(s) itself before accepting the receipt. The delegation-era form of the rework-prevention property (`CLAUDE.md`: "mechanically re-derivable claims").
- **Context budget** — a numeric threshold on session context (§5.6) that converts Phase 8's qualitative continuity check into a mechanical trigger.
- **Agent definition** — a versioned artifact under `.agents/agents/` defining a worker or reviewer agent type: its role, tool surface, prompt discipline, and receipt contract. Deployed to the harness's user-level agents directory.
- **Operator cue** — a fixed-format "what happened / your move / how" block emitted at every boundary that hands control to the operator (§5.9).

### Composition rule

The eight phases of `spec-execute` split by mode. Inline: the session runs all eight. Dispatch: the orchestrator runs 1–3, 7, 8; the worker runs 4–6. Phase 2 (pre-flight verify) is always orchestrator-side and consists of derivation re-checks — cheap shell commands, not code reading. The continuity invariant is unchanged from existing doctrine: at every task boundary the artifacts alone must suffice to resume; dispatch merely enforces it by construction, because the worker that held the working set no longer exists.

## 5. Detailed Design

### 5.1 `EXECUTION` input on `spec-execute`

**Purpose.** Select the execution topology per session.

**Shape.** New INPUTS entry:

```text
EXECUTION: <optional; "inline" | "dispatch". Default: "dispatch".
"inline" is an operator-selected restriction for tightly supervised work.>
```

**Behavior.** Orthogonal to `AUTONOMY`: `AUTONOMY` governs *where the run stops*; `EXECUTION` governs *who implements*. All four combinations are legal. Both defaults flip under this design: `EXECUTION` defaults to `dispatch`, and `AUTONOMY` defaults to `checkpoint` (autonomous between designed stops), inverting the prior `task` default. The prior rule "checkpoint is never self-granted" is restated for the new posture: **the agent may never loosen the stop set** — it may not skip a designed pause, downgrade a blocker, or continue past a budget breach; the operator may always *restrict* (`AUTONOMY: task` and/or `EXECUTION: inline`) for supervised work. The agent may *recommend* an `EXECUTION` switch mid-session, but the switch is surfaced, never silent.

**Pattern invoked.** Orchestrator–worker delegation with model-tiered subagents — Anthropic's documented agent-design pattern ("spawn a subagent with the cheaper model for the sub-task; keep the main loop on one model"; Claude API agent-design guidance, verified 2026-07-05).

**Why this design.** A mode, not a new skill: the contract (spec as source of truth, closeout discipline, amendment protocol) is identical in both modes; only the executor changes. A ninth skill would duplicate 80% of spec-execute's text and create drift risk between two contracts. The flipped defaults are the point of the design: autonomy without dispatch rebuilds mega-sessions; dispatch without autonomy makes the operator the loop's bottleneck. Defaulting both, against a rich designed-stop set, is what reinforces efficient practice by default rather than by per-session opt-in.

**Alternatives considered.** (a) Separate `spec-dispatch` skill — rejected: contract duplication, drift risk. (b) Making dispatch the only mode — rejected: single fully-specified tasks with tight coupling to a prior task are cheaper inline; inline also remains the fallback when the harness's Agent tool is unavailable.

### 5.2 Orchestrator conduct rules

**Purpose.** Keep the orchestrator flat so batch cost stays linear.

**Behavior.** In dispatch mode the session: (1) reads only spec, journal, and receipts — never code files, diffs, or build output; (2) runs shell commands only for derivation re-checks and git state inspection; (3) performs Phase 2 by re-running each receipt's journaled verification command(s) and comparing against the receipt's claim — a receipt whose re-check fails is rejected and the task is not accepted as done; (4) treats an amendment trigger in any receipt as a batch stop (existing `AUTONOMY: checkpoint` stop semantics, unchanged); (5) never "just quickly fixes" anything — a failed or partial task is re-dispatched with an updated brief, or stopped to the operator.

**Why this design.** Rule (3) is the load-bearing one: it preserves doctrine's "a reviewer re-runs the check, never trusts the narrative" across the delegation boundary at near-zero context cost (command output, not file contents). Rule (5) exists because orchestrator drift into implementation silently rebuilds the mega-session this design removes.

**Alternatives considered.** Trusting receipts outright — rejected: one masked accounting failure costs more remediation sessions than every re-check in a spec's lifetime combined (observed in practice in a consuming project, 2026-07; recorded here without naming the consumer). Having the orchestrator re-read the diff — rejected: reimports the working set; the re-check gives equivalent assurance for claims that were journaled as commands, which the receipt schema requires.

### 5.3 Worker brief contract

**Purpose.** Give a cold subagent everything it needs without transcript inheritance.

**Shape.** The brief contains, at minimum: task ID and full task text quoted from the spec; repo-relative paths to `SPEC_PATH` and `JOURNAL_PATH` (and `SPEC_REPO_ROOT` values when multi-repo); the referenced spec sections (quoted or path+anchor); the task's declared file list and DoD; the model floor being satisfied; the conventions pointer (repo `CLAUDE.md`s); and the receipt schema with its cap.

**Behavior.** The worker orients by reading the spec task and the latest journal entry from disk — the same Phase 1 discipline, scoped to one task. Workers do **not** invoke `spec-*` skills: the Phases 4–6 rules they must follow are embedded in the agent definition (§5.7), so loading the full skill (and its orientation phases, which the orchestrator already ran) would re-import exactly the cost this design removes. Workers implement, verify DoD with evidence, write the journal entry, and make the paired commits (code and spec/journal), all per the existing Phase 4–6 contracts.

**Why this design.** The spec already *is* the self-contained task contract — the brief mostly quotes it. Journaling and committing from the worker (not the orchestrator) keeps the evidence chain first-hand: the entity that ran the tests writes the entry.

**Alternatives considered.** Worker invokes `/spec-execute` with `SESSION_GOAL: <task>` — rejected: reloads the full contract per task and re-runs orchestration phases already done. Orchestrator writes the journal from the receipt — rejected: second-hand evidence; a transcription layer where confabulation can enter.

### 5.4 Receipt schema

**Purpose.** Fixed-shape, capped task summary — the only per-task addition to orchestrator context.

**Shape.** Hard cap 25 lines. Fields, in order:

```text
TASK: <ID> — <title>
STATUS: done | partial | blocked
COMMITS: <SHA list; both repos when paired>
DOD: <one line per DoD item: PASS/FAIL + evidence pointer + the re-runnable command, verbatim>
FILES: <count>; <list, truncated at 10 with "+N more">
AMENDMENT-TRIGGER: none | <one-line trigger + affected spec section>
SURPRISES: none | <≤2 lines>
NEXT: <next-task pointer per dependency graph>
```

**Behavior.** File contents, diffs, test output bodies, and build logs are forbidden in receipts — evidence enters as *pointers plus re-runnable commands*. At least one `DOD` line per task must carry a command the orchestrator can re-run (the derivation re-check hook). `AMENDMENT-TRIGGER ≠ none` or `STATUS ≠ done` stops the batch in every autonomy mode.

**Why this design.** The cap is what makes orchestrator growth ~1–2k tokens/task. Commands-as-evidence is what lets the cap coexist with the verification doctrine.

**Location (resolves OQ-1).** The schema's normative copy lives as a **support file inside the skill directory** — `.agents/skills/spec-execute/receipt-schema.md`, deployed alongside `SKILL.md` — referenced from the skill body and from the `spec-worker` agent definition rather than embedded in either. Skills carry their own supporting content; consumers copy nothing and need not reference it in their doctrine. This keeps a single normative copy on the load path of every party that needs it (orchestrator via the skill, worker via the agent definition) while keeping `SKILL.md` itself lean.

**Alternatives considered.** Free-form summaries — rejected: unbounded and unverifiable. Structured JSON — rejected: no consumer needs machine parsing yet; prose lines are cheaper to read and write (revisit if a tier-2 driver arrives, §12). Copying the schema into consuming-repo doctrine — rejected: N synchronized copies across independent consumers (this repo serves work and hobby projects alike); the enforcement point is the skill, not the consumer.

### 5.5 Journal `Executed by` field

**Purpose.** Make floor compliance re-derivable when work is delegated.

**Shape.** New journal-entry line, adjacent to the existing `Models used`:

```text
**Executed by:** inline | worker(<agent-definition-name>, <model>)
```

**Behavior.** Written by whoever writes the entry (the worker, in dispatch mode). A reviewer cross-checks `worker(…, <model>)` against the task's declared floor mechanically.

**Alternatives considered.** Folding into `Models used` — rejected: that field records models per work unit; this records the topology. Both are cheap; conflating them loses the distinction between "opus was consulted" and "an opus worker owned the task."

### 5.6 Phase 8 context budget

**Purpose.** Convert the continuity check's token-economy factor from advice into a trigger.

**Shape.** A fixed token threshold: **80,000 tokens of session context consumed** (assessed from the harness's context indicator, or conservatively estimated from turn count and read volume when no indicator is available).

**Behavior.** Below budget: Phase 8 operates as today. At or above budget: the recommendation *must* be "fresh session" (inline mode) or is moot (dispatch mode — the orchestrator, growing ~2k/task, should not approach it; hitting the budget in dispatch mode is itself a signal that conduct rules were violated, and the run stops for the operator). Under `AUTONOMY: checkpoint` + inline, a budget breach stops the run at the boundary rather than continuing — cleanly, with the journal's next-task pointer set.

**Why this design.** Fixed tokens rather than a window fraction: tokens are the fundamental unit of pricing, so a token threshold means the same cost exposure on every model, whereas a fractional threshold silently raises the spend ceiling whenever a larger-window model is selected. 80k leaves a full task's working set of headroom before the quality-degradation and cost effects observed in practice (well established by ~150k) dominate. If fixed tokens prove too limiting as capabilities evolve, amend when it becomes necessary — not preemptively (§13 OQ-2; pilot measures it).

**Alternatives considered.** Fraction-of-window — rejected: couples the operator's cost exposure to the selected model's window size, which pricing does not.

### 5.7 Agent definitions — new deliverable class

**Purpose.** Version the worker/reviewer contracts as artifacts instead of re-improvising them per batch.

**Shape.** Masters at `.agents/agents/<name>.md`, mirroring the skills layout. Initial set: `spec-worker.md` (Phases 4–6 contract, receipt schema, orientation discipline, prohibition on invoking `spec-*` skills or spawning further subagents) and `spec-reviewer.md` (§5.8). Model is deliberately **not** pinned in the definition — it is set per-spawn to the task's floor; the definition pins role, tool surface, and conduct.

**Behavior.** Deployed to the harness's user-level agents directory (`~/.claude/agents/` today — described in committed prose as "the harness agents directory"; the deploy-sync rule in `CLAUDE.md` §deploy-sync extends to cover this class). The frontmatter-portability rule applies to *skills*; agent definitions are declared harness adapters — the one artifact class in this repo that is allowed to be harness-facing, and the place harness-specific keys belong.

**Why this design.** The harness natively resolves agent types from these definitions (verified 2026-07-05), so the worker contract rides the same mechanism as the built-in agent types rather than being pasted into every spawn prompt.

**Alternatives considered.** Embedding the full worker contract in `spec-execute`'s prose and pasting it per spawn — rejected: bloats the skill every session loads, and unversionable per-batch drift. One agent definition per floor tier (`spec-worker-sonnet.md`, …) — rejected: model is a spawn-time parameter; N definitions differing only in a model line is drift surface.

### 5.8 `spec-review` dispatch option

**Purpose.** Give checkpoint reviews the same economics: a fresh reviewer context at the declared reviewer floor, coordinated by a thin session.

**Shape.** A `REVIEW_EXECUTION: inline | dispatch` input on `spec-review`, defaulting to `inline`. In dispatch, the session spawns a `spec-reviewer` agent at the checkpoint's reviewer floor; the reviewer reads the checkpoint contract, journal, and diff, and returns structured findings plus a verdict in `spec-review`'s existing fixed format; the coordinating session writes the outcome back to spec and journal.

**Behavior.** The reviewer subagent gets the *full* diff-reading mandate — review is the one place this design does not starve context, per doctrine ("starve context, not verification"). What dispatch buys here is isolation (a fresh, floor-correct context untainted by the execution session) and a thin coordinator, not a thinner review.

**Alternatives considered.** Leaving `spec-review` untouched — viable, and the fallback if CP-1 trims scope; included because fable-floor checkpoint reviews are among the most expensive sessions in current practice and benefit most from fresh-context isolation.

### 5.9 Operator cues at human-in-the-loop boundaries

**Purpose.** Reduce the operator's re-orientation cost. Human-in-the-loop steps typically land after a context switch — hours or days later, mid-other-work — and the operator should not have to remember the framework to act on a stop.

**Shape.** Every boundary that hands control to the operator ends with a fixed-format cue block:

```text
WHAT HAPPENED: <one line — e.g. "CP-2 triggered after T-12; batch stopped">
YOUR MOVE:     <one line — the decision or action only the operator can take>
HOW:           <the exact next invocation(s): skill name + INPUTS, pre-filled
                from current state; plus where to look first
                (journal entry / spec section / receipt)>
```

**Behavior.** Emitted at every designed stop and pause — the full set named in §2 Goals: Phase 7 checkpoint stops (cue: the `spec-review` invocation with inputs pre-filled from the checkpoint contract), Phase 8 pauses (cue: how to resume `spec-execute`, carrying the journal's next-task pointer), blocker escalations (cue: the question to answer and where the answer lands), amendment triggers (cue: the `spec-amend` invocation with `SECTION`/`TRIGGER` pre-filled), floor conflicts (cue: the floor that could not be met, the task it blocks, and the choice — supply a compliant model or amend the floor), production-touching-action stops (cue: the specific action requiring explicit authorization and how to grant it), and budget stops (cue: how to relaunch fresh). The journal's next-task pointer remains the durable handoff; the cue is its ephemeral, human-facing rendering — cues are emitted at boundaries, not stored in artifacts.

**Pattern invoked.** Pre-filled handoff prompts — the journal's next-task-pointer principle, extended to the human side of the loop.

**Why this design.** Under default autonomy the operator's touches become rarer and therefore colder; the framework's stops are only as good as the operator's ability to act on them cheaply. A stop without a cue converts autonomy's savings into operator re-orientation cost.

**Alternatives considered.** Documentation-only ("see the skill docs") — rejected: the operator's context switch is exactly when documentation lookup is most expensive. Storing full cues in the journal — rejected: journals are durable artifacts and stay terse; the pointer lives there, the how-to renders at the boundary.

## 6. Non-functional Requirements

- **Economy.** Orchestrator context growth ≤ ~2k tokens per accepted receipt; a 6-task batch ends below ~40k total orchestrator context (pilot-measured, §8).
- **Auditability.** Every done-claim traceable to a re-runnable command; floor compliance re-derivable from `Executed by` + the spec's floor table; receipts quoted in no artifact (the journal, written first-hand by the worker, is the record).
- **Reversibility.** No artifact-format changes beyond one additive journal field. A dispatch-produced journal is indistinguishable from an inline one except `Executed by`. Setting `EXECUTION: inline` fully restores prior behavior.
- **Resumability (human).** Every operator-facing stop emits a §5.9 cue; an operator returning cold can act on a stopped run from the cue plus the journal's latest entry alone.
- **Security/safety.** Workers inherit the session's permission mode; production-touching actions stop the run in every autonomy mode (unchanged); workers may not spawn further subagents (single delegation level, enforced by the agent definition).
- **Portability.** Skill masters remain harness-agnostic; all harness coupling is confined to `.agents/agents/` and flagged as version-sensitive (§10 R-3).

## 7. Implementation Sequencing (Forward-Looking)

Amendment-class work throughout — no downstream feature spec is anticipated; each phase lands via `spec-amend` against the named governing spec (paired with the master + deploy-copy edits) or as direct artifact authoring where no governing spec exists yet.

1. **P1 — Approval.** This spec through CP-1.
2. **P2 — `spec-execute` amendment set.** Amend the [spec-execute governing spec](../20260518-spec-execute-skill/architecture.md) (§4 Execution model, §5.4–5.8) and the master per §5.1–5.6 and §5.9 (defaults flip, budget, cues); author the `receipt-schema.md` support file in the skill directory; sync deploy copies. Produces: the amended skill.
3. **P3 — Agent definitions.** Author `spec-worker.md` (and `spec-reviewer.md`) under `.agents/agents/`; deploy; extend `CLAUDE.md`'s deploy-sync rule to the new class. Produces: spawnable worker/reviewer types.
4. **P4 — `spec-review` amendment.** Per §5.8 and §5.9 (reviewer-boundary cues), against the [spec-review governing spec](../20260518-spec-review-skill/architecture.md).
5. **P5 — Pilot and calibration.** One real batch in a consuming repo (selected operator-side at pilot time; recorded in this spec's journal) in dispatch mode; measure §6 economy targets and §8 criteria; file findings via `finding-intake`; tune the §5.6 constant. Produces: CP-3 evidence.

## 8. Validation Approach

- **Dogfood pilot (primary).** Run P5's batch and measure: ending orchestrator context vs. the ~150k inline baseline; receipt fidelity (fraction of derivation re-checks that pass on first acceptance — target 100%); floor compliance (journal `Executed by` vs. spec floor table — target 100%); operator interrupt count vs. an equivalent inline batch.
- **Cold-read test.** Before P5, hand the `spec-worker` definition plus a sample brief to a fresh session with no other context; it must correctly state its task, its DoD, its receipt obligations, and what it is forbidden to do. Failures are contract gaps, fixed before pilot.
- **Consistency sweep.** After P2–P4: masters ≡ deploy copies (mechanical diff); governing specs, skill text, and this spec agree on vocabulary (grep for the §4 terms).

## 9. Review Checkpoints

### CP-1 — Design approval

**Status:** pass with comments on 2026-07-05 by Claude (AI reviewer) — see [CP-1 review journal entry](./journal.md). 2 important findings (doctrine-citation fidelity §3/§4/§9; §5.9 cue coverage for floor-conflict and production-touching-action stops) fixed in Draft Revision 2 before approval; 1 advisory (pre-existing `AUTONOMY` documentation gap in the `spec-execute` governing spec) deferred to P2. Operator approved 2026-07-05. **Checkpoint closed.**

- **Trigger.** This document complete, Draft status.
- **Review focus.** Doctrine compatibility (does the derivation re-check genuinely preserve the rework-prevention property, `CLAUDE.md`'s "mechanically re-derivable claims"?); receipt schema sufficiency (can a reviewer reconstruct floor compliance and DoD state from artifacts alone?); the flipped defaults (§5.1 — is the designed-stop set rich enough to make autonomous-by-default safe?); cue coverage (§5.9 — is any operator-facing boundary missing one?); harness-fact verification (§3 constraints).
- **Exit criteria.** Operator approves; open questions have owners; banner advances to Approved.

### CP-2 — Amendment-set consistency

- **Trigger.** P2–P4 landed.
- **Review focus.** Master/deploy equality (mechanical); governing-spec ↔ skill-text agreement; cold-read test passed; no skill gained harness-specific frontmatter.
- **Exit criteria.** Consistency sweep clean; cold-read pass journaled.

### CP-3 — Pilot acceptance

- **Trigger.** P5 batch complete.
- **Review focus.** §6 economy targets against measured numbers; receipt-fidelity and floor-compliance targets; whether the §5.6 constant held up.
- **Exit criteria.** Operator accepts the measured result, or findings are filed and the spec amended.

## 10. Risks and Mitigations

| # | Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| R-1 | Receipt confabulation — a worker reports PASS on a DoD item that fails | Medium | High (masked rework, the exact failure doctrine exists to prevent) | Mandatory derivation re-check before acceptance (§5.2); commands-as-evidence required in schema (§5.4); pilot measures fidelity | Orchestrator conduct rules / CP-3 |
| R-2 | Worker scope creep beyond the declared file list | Medium | Medium | Brief carries the file list; receipt `FILES` cross-checked against `git diff --stat` in the re-check; overruns stop the batch as amendment triggers | §5.2 / §5.4 |
| R-3 | Harness changes to the Agent tool or agent-definition format | Medium | Medium (dispatch unavailable; inline unaffected) | Harness coupling confined to `.agents/agents/`; `EXECUTION: inline` is a complete fallback; §13 watch items | Operator |
| R-4 | Orchestrator drift into implementation ("quick fix" temptation) | Medium | High (silently rebuilds the mega-session) | Conduct rule §5.2(5) + WHAT-NOT-TO-DO entries in the amended skill; budget breach in dispatch mode is itself a stop signal (§5.6) | P2 amendment |
| R-5 | Cold workers missing tacit context an inline session would have had | Medium | Medium (partial/blocked receipts, re-dispatch cost) | Briefs quote the spec rather than summarize it; `SURPRISES` field feeds the journal so the gap becomes artifact-borne; re-dispatch with an updated brief is the designed recovery | §5.3 |
| R-6 | Dispatch overhead makes small tasks net-costlier | Low | Low | Inline remains available as an operator restriction; Phase 8 rubric already leans "either is fine" for small, fully-specified tasks | §5.1 |
| R-7 | The design is wrong | Low | High | CP-1 review before any amendment lands; pilot before doctrine references it | CP-1 |

## 11. Adoption Path

Adoption is per-session and per-consumer: a consuming repo adopts by invoking `spec-execute` with `EXECUTION: dispatch` (or by running `AUTONOMY: checkpoint` batches and taking the default). No consuming-repo artifacts change; consuming specs already declare floors, which is the only input dispatch needs from them. A consuming repo's doctrine may add a reference to this spec but does not have to for dispatch to work.

**Back-out.** Set `EXECUTION: inline` (or nothing, under task autonomy). Because artifacts are format-identical, a batch can mix modes freely and a consumer can abandon dispatch mid-spec with no cleanup.

**Partial-adoption degradation.** Fully graceful: any task boundary is a valid mode switch point, because the journal — not the session — is the carrier either way.

## 12. Out of Scope

Deliberately deferred, expected to come up in review:

- **Tier-2 headless driver** (fresh session per task via `claude -p` or scheduled runs): compatible with everything here — receipts and briefs are its natural interface — but a separate commitment with its own operational surface.
- **Parallel worker fan-out.** Requires dependency-graph reasoning about task independence; serial dispatch must prove receipt fidelity first (§13 OQ-3).
- **Cloud/scheduled execution** (`/schedule`-class): consuming projects commonly touch live local state remote environments cannot reach; revisit per-consumer if that changes.
- **Skill-frontmatter model pinning** (per the harness usage panel's suggestion): rejected for this family — floors are per-task and spec-declared, not per-skill; masters stay harness-agnostic.
- **`finding-*` and authoring skills** (`spec-write`, `spec-design`, `spec-amend`, `project-constitution`): interactive by nature; no execution topology to change.
- **Rewriting consuming-repo doctrine.** Consumers already state the principles this design mechanizes; at most a consumer adds a cross-reference.

## 13. Open Questions

### OQ-1 — Where does the receipt schema live for consumers?

**Resolved 2026-07-05 (draft revision, operator decision).** The schema lives as a support file inside the skill directory (`receipt-schema.md`, deployed alongside `SKILL.md`), referenced by the skill and the agent definition; consumers copy nothing. Decision and rationale recorded in §5.4 (Location). Retained here as a stub to keep OQ numbering stable.

### OQ-2 — The context-budget constant

**Question.** Is 80,000 tokens the right Phase 8 trigger?

**Analysis.** Too low: churny fresh-session stops on healthy inline sessions. Too high: the budget never fires before quality/cost damage is done. The 2026-07-05 usage evidence puts established damage at ~150k; 80k leaves a full task's working set of headroom below that. Fixed tokens rather than a window fraction is an operator decision (draft revision): tokens are the pricing fundamental, so the threshold holds cost exposure constant across models. No measured curve yet.

**Leaning.** Ship 80k, measure at P5, tune by amendment — including revisiting fixed-vs-fractional only if fixed tokens prove limiting as capabilities evolve. **Watch items.** Pilot's per-task context deltas; any harness change to context-indicator visibility; capability changes that make 80k materially restrictive.

**Owner.** P5 / CP-3.

### OQ-3 — Parallel dispatch

**Question.** When may an orchestrator run multiple workers concurrently?

**Analysis.** The harness supports background subagents, and consuming specs encode dependencies (`Deps:` fields), so the information exists. But concurrent workers writing paired commits and journal entries contend on the spec repo; and receipt-acceptance ordering becomes a merge problem. Serial dispatch has none of this.

**Leaning.** Defer until serial receipts prove ≥ one full batch at 100% fidelity. **Anti-goal:** do not parallelize by giving workers disjoint journal files — splitting the journal breaks the single-handoff-artifact property that everything else relies on.

**Owner.** Future amendment; earliest after CP-3.

### OQ-4 — Worker tool surface

**Question.** Exactly which tools does the `spec-worker` definition grant?

**Analysis.** Workers need file edit/read, shell, and git (implement, verify, commit). They must not spawn subagents (single delegation level, floor-laundering guard). Web access is task-dependent (some consuming tasks fetch upstream docs); granting it broadly is simpler, denying it is safer-by-default with per-batch exceptions in the brief.

**Leaning.** Full tool surface minus the Agent tool; note web access as a brief-level instruction rather than a definition-level denial, to keep one definition.

**Owner.** P3 authoring; validated by the cold-read test.

## 14. References

**Authoritative.**

- Claude Code subagents and agent definitions — <https://code.claude.com/docs/en/sub-agents>; behavioral facts (per-spawn `model` override with floor-matching tiers, cold start, final-message-only return, `.claude/agents/*.md` frontmatter) verified against the running harness, 2026-07-05.
- Anthropic prompt-caching reference — <https://platform.claude.com/docs/en/build-with-claude/prompt-caching> (TTL, read/write pricing); verified 2026-07-05 via the claude-api skill reference (cached 2026-06-24).
- [spec-execute governing spec](../20260518-spec-execute-skill/architecture.md); [spec-review governing spec](../20260518-spec-review-skill/architecture.md) — amendment targets.
- Repo constitution: [mission](../mission.md), [tech-stack](../tech-stack.md), [roadmap](../roadmap.md); repo `CLAUDE.md` (frontmatter portability, deploy-sync rule).
- Operational evidence: operator usage panel, 2026-07-05 (55% of usage at >150k context; `/spec-execute` 33% of usage) — recorded in this spec's journal.

**Inspirational.**

- Anthropic agent-design guidance (orchestrator/worker with model-tiered subagents; context-editing vs. compaction vs. memory taxonomy).
- [session-economy spec](../20260514-session-economy/architecture.md) — first-iteration ideas capture; historical context only.
- Consuming-repo operating doctrine — the principles this design mechanizes; each consumer carries its own, and none is named or assumed by this spec.
