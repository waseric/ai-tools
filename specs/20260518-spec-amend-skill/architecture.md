# `spec-amend` Skill — Architecture and Protocol Specification

> Status: Draft — Open for Review
> Date: 2026-05-18
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set.

## 1. Overview

The `spec-amend` skill is the methodology's **amendment skill**: it structures, drives approval of, applies, and journals a surgical change to an existing spec — design spec or feature spec — after that spec has been committed or shared. It is the only quintet skill whose primary input is a *proposed change* to an artifact authored by another skill, and whose primary output is a *visible, durable revision record* in the spec and its journal. It is the loop-closer for the lifecycle: every other quintet skill ([spec-execute](../../.agents/skills/spec-execute/SKILL.md) Phase 3/4, [spec-review](../../.agents/skills/spec-review/SKILL.md) Phase 7, [spec-design](../../.agents/skills/spec-design/SKILL.md) / [spec-write](../../.agents/skills/spec-write/SKILL.md) once a Draft has been shared) routes here when drift, contradiction, or new information reveals the spec must change.

This document is a **retroactive design specification**: the skill already ships at [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md), which is authoritative for behavior. The spec describes what the skill *is* and what it *commits to*. It does not redesign the skill; any divergence the spec exposes between its commitments and the shipping SKILL.md is routed to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md) itself — never silently corrected. (This skill is self-referential at the amendment-application step; the journal records the meta-observation for the final-session reader.)

## 2. Goals and Non-goals

### Goals

- Produce a self-contained, descriptive specification of the `spec-amend` skill's vocabulary, contract, phases, change-classification scheme, and journal-entry shape — sufficient that a reviewer can validate the design against the shipping SKILL.md without ambiguity.
- Declare review gates (§9) so the skill becomes reviewable via [/spec-review](../../.agents/skills/spec-review/SKILL.md) against named checkpoints.
- Hold the skill to the **Atomic-Skill Portability Principle** declared in the methodology's constitution ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)) — including degradation when `SPEC_REPO_ROOT` is absent and when the amended artifact is a design spec rather than a feature spec.
- Continue the **paired-artifact pattern** for retroactive skill specs — `architecture.md` + `journal.md` — established at N=1 ([specs/20260517-project-constitution-skill/](../20260517-project-constitution-skill/)), refined at N=2 ([specs/20260518-spec-design-skill/](../20260518-spec-design-skill/)) and N=3 ([specs/20260518-spec-write-skill/](../20260518-spec-write-skill/)), extended at N=4 ([specs/20260518-spec-execute-skill/](../20260518-spec-execute-skill/)) with the two-source structure, and refined at N=5 ([specs/20260518-spec-review-skill/](../20260518-spec-review-skill/)) as the simplest two-source application. This N=6 instance validates, refines, or rejects each of the N=5 journal's six "Pattern for N=6" callouts plus the post-CP-1 authoring-time-citation-walk observation.
- Apply the **two-source structure with an inline predecessor**: the predecessor for `spec-amend` is not a standalone artifact but the AMENDMENT PROTOCOL block embedded *inside* the [predecessor `spec-execute-prompt.md` artifact](../../docs/spec-driven-development-prompts-conversation.md) (lines 391–403) plus the design-note paragraph at line 414. This is structurally distinct from N=2–N=5, all of which had standalone predecessor artifacts. The asymmetry is recorded in §3, §14, and the journal so future readers do not replay the search for a missing standalone artifact.
- Apply the **single shape (i) §5-enumerated sibling-design-spec mapping** in its **three-addition variant** — [session-economy §5.3](../20260514-session-economy/architecture.md) prescribes three SKILL.md additions for `spec-amend` (INPUTS entry + Phase 4 paragraph + Phase 5 note), not two as the N=5 journal's "Pattern for N=6 #1" predicted. The refinement is recorded explicitly.
- Carry forward the **authoring-time per-citation walk** discipline named in [N=5 journal §"Pattern observation for N=6"](../20260518-spec-review-skill/journal.md). Every Pattern-invoked citation in §5 of this document was walked against the actual cited subsection at authoring time (heading text + content match, not just heading line target).

### Non-goals

- **Redesign of the skill.** The shipping SKILL.md is authoritative for behavior. This spec is descriptive.
- **A template for any further retroactive specs.** No legacy-quintet retroactive spec remains after this one. The `docs/retroactive-spec-pattern.md` decision is governed by [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md); the operator confirmed at this session's Phase 2 that the pattern doc will be authored as the closing artifact of the retroactive-spec series, *after* this spec, *after* CP-1, and ideally *after* the batched CP-2 audit produces its evidence. This spec does not pre-author it.
- **Modification of the shipping SKILL.md.** Only the Amendment Protocol via [/spec-amend](../../.agents/skills/spec-amend/SKILL.md) touches SKILL.md.
- **Specifying tooling, models, or platforms.** The skill produces spec-edit + journal-entry + commit only, consistent with the [mission.md Out of Scope](../mission.md) commitment.
- **Resolving the open mechanics gaps surfaced in §13.** The four §13 OQs (in-flight/amendment boundary; re-approval path after status revert; multi-finding batching; cross-skill amendment coordination) are first-class outputs; resolution belongs to future amendment sessions triggered by their watch items.
- **Specifying when an operator should invoke `/spec-amend` vs. work directly without the skill.** Same shape as [N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md) (format-selection question); methodology-wide, not skill-specific.

## 3. Background and Constraints

### Prior state

The `spec-amend` skill was created at trilogy commit `49c15f0` (2026-05-14) by **extracting the inline AMENDMENT PROTOCOL block from inside the predecessor `spec-execute-prompt.md` artifact** ([docs/spec-driven-development-prompts-conversation.md lines 391–403](../../docs/spec-driven-development-prompts-conversation.md)) into its own standalone skill. This is structurally distinct from N=2 / N=3 / N=4 / N=5, all of which had standalone predecessor artifacts — `spec-writing-prompt.md`, `spec-execution-prompt.md`, `spec-review-prompt.md`. The trilogy commit kept the **AMENDMENT PROTOCOL header** in [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) (currently line 209) but **re-pointed its body** to route to `spec-amend` rather than apply the change inline. The separation between *proposing* (spec-execute) and *applying* (spec-amend) is the trilogy commit's load-bearing architectural change.

The skill evolved through three later commits:

- `e483466` (2026-05-14) — **session-economy commit**. Added `SPEC_REPO_ROOT` to the INPUTS block; added the Phase 4 "Multi-repo case" paragraph; added the Phase 5 journal-commit note. The architectural source for all three is [specs/20260514-session-economy/architecture.md §5.3](../20260514-session-economy/architecture.md) — a *sibling design spec*, not a predecessor doc. The same commit touched four sibling skills with analogous multi-repo additions; the `spec-amend` contribution is the **only one in the quintet to prescribe three SKILL.md additions** (every other skill received two or fewer).
- `d9a0002` (2026-05-14) — `lastUpdated` frontmatter added across the skill family.
- `6d158fb` (2026-05-15) — path convention update: `docs/specs/<feature>.md` → `specs/YYYYMMDD-<feature-name>/feature.md`, propagated through `SPEC_PATH` / `JOURNAL_PATH` examples in the six lifecycle skills via the [spec-path-convention](../20260515-spec-path-convention/architecture.md) feature spec.

The predecessor block is treated in this spec as **authoritative for the skill's design rationale** but not authoritative for current behavior: the shipping SKILL.md is the latter. Where they diverge, SKILL.md wins and the divergence is read by CP-2 as *evolution*, not drift. The extraction-into-standalone-skill (`49c15f0`), the session-economy `SPEC_REPO_ROOT` / Phase 4 / Phase 5 additions (`e483466`), and the `SPEC_PATH` update (`6d158fb`) all postdate the predecessor block and are *evolution* with explicit commits and, where applicable, an authoritative sibling design spec.

The [session-economy design spec](../20260514-session-economy/architecture.md) occupies a distinct role: it is a *sibling design spec* that authoritatively commits to behavior present in the current SKILL.md. Its §5.3 enumerates exactly three SKILL.md additions for `spec-amend` — the `SPEC_REPO_ROOT` INPUTS entry, the Phase 4 "Multi-repo case" paragraph, and the Phase 5 journal-commit note. It is cited Authoritative in §14 alongside the SKILL.md. This is the **three-addition variant** of the simplest application of the two-source structure introduced at N=4: only **shape (i) §5-enumerated** attribution applies (retro §5.4 + §5.5 + INPUTS contract ↔ session-economy §5.3 covers all three additions); no **shape (ii) narrative-sourced** mappings are exercised. The N=5 prediction ("the N=6 retroactive spec will have one cross-spec mapping under shape (i) only, matching N=5's simplicity") is **validated structurally** (shape (i) only; no narrative-sourced) but **refined in scale** (three additions, not two — N=5 had two; spec-amend is one larger).

### Constraints (cited)

- **Atomic-Skill Portability Principle** ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)). The skill must be a portable atomic unit: workflow + amendment-template + journal-entry template + change-classification scheme bundled in its own `SKILL.md`; adapts to richer host-repo embodiments (`SPEC_REPO_ROOT` set, design spec being amended instead of feature spec, AI-assisted approver) when present; degrades cleanly when absent. A `spec-amend` installed against an unrelated host repo with a single-repo feature-spec amendment still produces a conformant amendment record + journal entry.
- **AI context window limits** ([specs/tech-stack.md §44](../tech-stack.md#L44)). Amendment journal entries are LLM-consumed across sessions. The fixed Phase 2 structured-amendment format and Phase 5 journal-entry format propagate this constraint — amendments are scannable in any future session by structural recognition, not by re-reading prior chat or commit history.
- **Spec-driven-development convention** ([specs/tech-stack.md §51](../tech-stack.md#L51)). The methodology repo "eats its own cooking." This convention is the explicit justification for this retroactive spec. The self-referential observation that this skill applies amendments to specs *including its own* is recorded in §11 Adoption Path.
- **Repository layout convention** ([specs/tech-stack.md §48](../tech-stack.md#L48)). `specs/YYYYMMDD-<feature-name>/feature.md` + `journal.md` is the canonical `SPEC_PATH` / `JOURNAL_PATH` shape, set by the [spec-path-convention](../20260515-spec-path-convention/architecture.md) propagation in commit `6d158fb`.

### Dependencies

- **Upstream.**
  - [spec-design](../../.agents/skills/spec-design/SKILL.md) and [spec-write](../../.agents/skills/spec-write/SKILL.md) — author the design specs and feature specs that this skill amends. The shape of the amendment depends on the section being amended (feature-spec task scope is one shape; design-spec §5 detailed-design contract is another); SKILL.md is intentionally agnostic.
  - [project-constitution](../../.agents/skills/project-constitution/SKILL.md) — indirect upstream. Constitutional bindings travel through to the specs `spec-amend` revises; an amendment to a section that cites a constitutional binding inherits that binding's revision discipline (the constitution amendment ceremony, named in §12, is out of scope here).
  - [session-economy architecture spec](../20260514-session-economy/architecture.md) — *companion* design spec authoritative for the `SPEC_REPO_ROOT` INPUTS entry, the Phase 4 "Multi-repo case" paragraph, and the Phase 5 journal-commit note. Not a *predecessor*: it postdates the predecessor block, and its committed behavior is what currently ships.
- **Downstream / sideways.**
  - [spec-execute](../../.agents/skills/spec-execute/SKILL.md) — routes here from its Phase 3 (Clarify) or Phase 4 (Execute) when the implementer halts on a spec contradiction. The implementer's evidence (file path, test output, contradiction) becomes the amendment's Trigger and Reason. After the amendment is applied, `spec-execute` re-orients via its Phase 1 against the amended spec.
  - [spec-review](../../.agents/skills/spec-review/SKILL.md) — routes here from its Phase 7 verdict's "Spec amendments proposed" section. The reviewer *proposes* amendments; this skill *applies* them. The separation is what makes amendments visible.
- **Lateral.** Sibling lifecycle skills reference `spec-amend` by name and path; all six skills were modified together in commits `e483466` (session-economy / multi-repo) and `6d158fb` (path convention).

## 4. Architecture

The skill's architecture is a **six-phase sequential workflow** that takes a proposed change to a committed spec, structures it as a diff, drives explicit approval, applies the change with paired commit, journals the transition as a first-class event, and hands off downstream consumers (implementer, reviewer, next session). It produces a **structured amendment record** in a fixed Phase 2 format with a corresponding **journal entry** in a fixed Phase 5 format, and (when `SPEC_REPO_ROOT` is set) a **paired commit in the spec repo**. It is executed as a **portable atomic unit** that integrates with the broader spec-driven-development pipeline.

### Output topology

A single amendment session produces three durable artifacts:

1. **Structured amendment record** — a markdown block in the fixed Phase 2 format (Trigger / Section / Change before+after / Reason / Impact / Status implication / Approver). Pasted into the journal entry's "Full record" sub-block.
2. **Spec edit + commit** — the section identified in `SECTION` is edited to match the `After:` text, plus any cross-reference follow-ups that are part of the same coherent change, plus (if status changed) the Status banner and date. Committed in the spec's repo (which is `SPEC_REPO_ROOT` when set) with a message referencing the amendment ID.
3. **Journal entry** — short form (Section amended / Trigger / Reason / Impact summary / Approver / Approved on / Status implication / Commit) with the structured amendment record embedded under "Full record." Committed in the spec's repo.

When `SPEC_REPO_ROOT` is set, all three artifacts land in the spec repo, paired with whatever code-side state prompted the amendment.

### Vocabulary (defined here, used consistently below)

- **Amendment** — a change to a Draft-or-later spec, *after* it has been committed or shared. The unit this skill produces and records. Goes through all six phases.
- **In-flight edit** — a change to a Draft spec that has *not yet* been committed or shared, while the original [spec-write](../../.agents/skills/spec-write/SKILL.md) or [spec-design](../../.agents/skills/spec-design/SKILL.md) session is still running. Does not go through this skill; the author just edits. The boundary between in-flight edit and amendment is surfaced as §13 OQ-1.
- **Rewrite** — the spec is so wrong that patching it section-by-section is dishonest. Stops this skill and routes to a new [spec-write](../../.agents/skills/spec-write/SKILL.md) or [spec-design](../../.agents/skills/spec-design/SKILL.md) session. A rewrite masquerading as a sprawling amendment is the failure mode this distinction prevents.
- **Surgical** — an amendment targets one section, or a coherent set of related sections. "Coherent" is the load-bearing word; its precise scope is §13 OQ-3.
- **Diff** — the before/after representation of the change. Blockquoted in the Phase 2 format so the visual distinction between "current text" and "proposed text" survives copy-paste into chat or PR descriptions.
- **Status implication** — whether the amendment keeps the spec at its current Status banner (e.g., `Approved` stays `Approved`) or reverts it (e.g., `Approved` returns to `Draft`). The default is "kept"; reversion requires explicit justification.
- **Approval** — the approver's explicit "yes" before Phase 4 fires. Silence is not approval. The approver is usually the user but may be a named reviewer in governance-heavier contexts.
- **Class classification** — the Phase 1 decision: `amendment` / `in-flight edit` / `rewrite`. Determines whether the skill proceeds, exits to an editor, or routes to a redesign session.

### Execution model

An amendment session enters with:
- A `SPEC_PATH` and `JOURNAL_PATH` (in the spec repo, which may be `SPEC_REPO_ROOT` if set).
- A `SECTION` identifying the part of the spec being amended (section number, task ID, checkpoint ID).
- A `TRIGGER` — what surfaced the need (an `spec-execute` halt, a `spec-review` finding, an operator observation).
- A `PROPOSED_CHANGE` if the user already drafted it; otherwise the skill drafts from conversation.
- An `APPROVER` — usually the user.

The skill reads the section verbatim and its cross-references (Phase 1), drafts the structured amendment (Phase 2), stops for explicit approval (Phase 3), applies the edit with paired commit (Phase 4), appends the journal entry (Phase 5), and states downstream actions (Phase 6). The structured amendment + journal entry are the deliverables; the phases are the means.

### Where this design plugs in

`spec-amend` is invoked when a committed or shared spec must change. Upstream invokers include [spec-execute](../../.agents/skills/spec-execute/SKILL.md) (Phase 3/4 halt), [spec-review](../../.agents/skills/spec-review/SKILL.md) (Phase 7 verdict's "Spec amendments proposed" section), or a direct operator invocation. Downstream consumers — the implementer resuming execution, the reviewer closing a checkpoint, the next session orienting via the journal — see the amendment as a first-class event and adjust their work to the amended spec. The skill is the **only spec-driven-core skill whose primary output is a revision record on another skill's artifact, not a new authored artifact**.

## 5. Detailed Design

### 5.1 Phase 1 — Orient

**Purpose.** Read the section being amended verbatim before proposing any change, plus the surrounding sections, the trigger context, and prior amendments to the same section. Produce an Orientation Report.

**Behavior.** The agent reads, in order: (1) the full SECTION of `SPEC_PATH` being amended, quoting verbatim for the eventual `Before:` block; (2) surrounding sections that reference or depend on this one, noting cross-references; (3) the TRIGGER context (the relevant `spec-execute` task and journal entry if execution surfaced it; the `spec-review` verdict if review surfaced it; the operator's statement if the operator surfaced it); (4) the journal at `JOURNAL_PATH` for prior amendments to the same section (an amendment contradicting a recent prior one is a stability signal worth surfacing). The agent emits an Orientation Report with: Section in scope (quoted text + line range), Trigger summary, Cross-references list, Prior-amendments-to-this-section list, and **Class classification** (`amendment` / `in-flight edit` / `rewrite`) with justification. If the classification is `rewrite`, the skill stops and routes to a new `spec-write` or `spec-design` session.

**Pattern invoked.** "Quote the contract before changing it" — the same disciplinary shape as [spec-review §5.1 Phase 1 Orient](../20260518-spec-review-skill/architecture.md) and [spec-execute §5.1 Phase 1 Orient](../20260518-spec-execute-skill/architecture.md) (read-before-act). Verified against the [shipping SKILL.md Phase 1](../../.agents/skills/spec-amend/SKILL.md) at the date of this spec.

**Why this design.** Without verbatim quoting at Phase 1, the Phase 2 `Before:` block becomes a paraphrase, and paraphrase silently introduces drift — the amendment then changes more than the surfaced trigger demanded. Reading prior amendments to the same section catches the "we already fixed this; now we are undoing the fix" failure mode at the only point it is cheap to catch.

**Alternatives considered.** Skipping the prior-amendments scan — rejected; the redundancy with Phase 1's section quote is small, and the cost of missing a contradicting-prior-amendment is large. Folding class classification into Phase 2 — rejected; classification belongs at orientation because a `rewrite` classification stops the skill before any drafting work.

### 5.2 Phase 2 — Draft the amendment

**Purpose.** Produce the structured amendment record in the fixed format so the approver, future sessions, and downstream consumers all read the same shape.

**Behavior.** If the user supplied `PROPOSED_CHANGE`, the agent structures it. If not, the agent drafts from conversation and asks the user to confirm. The output is a markdown block with the exact headings declared in [SKILL.md Phase 2](../../.agents/skills/spec-amend/SKILL.md): Trigger (one paragraph on what was discovered, by whom, in what context); Section (name + line range); Change (Before / After, both blockquoted verbatim); Reason (two or three sentences on why the original is wrong, stale, or under-specified); Impact (Affected tasks / Affected checkpoints / Completed work invalidated / Cross-references requiring follow-up); Status implication (does the spec stay at current status or revert to Draft; justify if non-default); Approver (filled in after Phase 3).

**Pattern invoked.** "Structured artifact for parseable history" — the same shape applied at [spec-review §5.7 Phase 7 Verdict](../20260518-spec-review-skill/architecture.md): a fixed format with variable content so future sessions and agents scan by structural recognition. Verified against [SKILL.md Phase 2 lines 74–100](../../.agents/skills/spec-amend/SKILL.md) at the date of this spec.

**Why this design.** The "Before:" / "After:" blockquote pattern is load-bearing: it survives copy-paste into PR descriptions, Slack threads, and chat tools that strip code-fences. Plain-text diffs in a fenced code block lose blockquote rendering; embedded HTML loses portability. Blockquoted prose preserves the visual distinction in every reader's default rendering.

**Alternatives considered.** A unified-diff format (`@@ ... @@` plus `-` / `+` lines) — rejected; readable to engineers but illegible to non-engineer approvers, and brittle under copy-paste. A "describe the change in prose" approach — rejected; the entire point of OP #2 ("Diff is required") is that prose descriptions of changes silently expand scope when applied.

### 5.3 Phase 3 — Approval

**Purpose.** Stop. Present the structured amendment to the approver. Apply only on explicit approval; iterate or end on rejection.

**Behavior.** The agent presents the Phase 2 record and waits for one of four responses: **Approved as drafted** (proceed to Phase 4); **Approved with revisions** (capture revisions, update draft, re-present, iterate until approved); **Rejected** (capture rejection rationale in the journal as a *non-amendment*, so the discussion is recoverable next time the same issue surfaces; do not modify the spec; end); **Reclassified as rewrite** (stop; route to a new `spec-write` or `spec-design` session; end). The agent does not proceed to Phase 4 without explicit approval; silence is not approval.

**Pattern invoked.** "Explicit approval is a hard stop." Same discipline as [spec-execute §5.7 Phase 7 Checkpoint gate](../20260518-spec-execute-skill/architecture.md) — the skill structurally enforces the pause so the operator cannot accidentally implicitly approve by inaction. Verified against [SKILL.md Phase 3](../../.agents/skills/spec-amend/SKILL.md) at the date of this spec.

**Why this design.** Approval-by-silence is the failure mode that corrodes the spec contract over time. Each "amendment that nobody objected to" individually looks fine; the aggregate is a spec that drifted without any single visible decision. The phase boundary stops drift at the moment of decision rather than three months later when the drift becomes load-bearing. The four explicit outcome paths (including "rejected as non-amendment journal entry") prevent the "we discussed this and decided not to" pattern from being lost.

**Alternatives considered.** A "default-approve after silence-window" approach — rejected; defeats the whole purpose. A "single approval channel" (no Reclassified-as-rewrite path) — rejected; without an explicit reclassification path, the skill would silently apply sprawling amendments that should have been rewrites.

### 5.4 Phase 4 — Apply

**Purpose.** Apply the approved change to the spec, including any cross-reference follow-ups that are part of the same coherent amendment, and commit with a message referencing the amendment ID.

**Behavior.** The agent edits the SECTION in `SPEC_PATH` to match the `After:` text. If status implication is "revert to Draft," the Status banner and date are updated. If cross-references in other sections need follow-up edits, the agent applies them now (each follow-up is part of the same amendment, not a separate amendment — keeping the unit coherent). If the spec has a "Format note" at the top declaring deviations from a template and this amendment changes the deviation, the agent updates the Format note. Then the agent commits with the message:

```
spec: amendment <YYYY-MM-DD-N> — <one-line summary>

See <JOURNAL_PATH> for full amendment record.
```

**Multi-repo case.** When `SPEC_REPO_ROOT` is set, the spec edits are committed in `SPEC_REPO_ROOT`, not in the codebase repo. The amendment commit message references the same amendment ID as any related code-side changes. The agent does not let the amendment commit ship without verifying the codebase-side state is consistent — if the amendment changes task scope, the implementer's next code commit must reflect it.

**Pattern invoked.** "Spec and journal are the durable working memory; paired commit is the durable transition record." Architectural source: [session-economy/architecture.md §5.3](../20260514-session-economy/architecture.md) — the *sibling design spec* authoritatively committing to both the `SPEC_REPO_ROOT` INPUTS entry and the Phase 4 "Multi-repo case" paragraph. Shape (i) §5-enumerated mapping: retro §5.4 + INPUTS contract ↔ session-economy §5.3 (additions 1 and 2 of the three). Verified at the date of this spec by reading session-economy §5.3 lines 123–145 directly.

**Why this design.** The amendment commit is the only mechanical signal future sessions have that the spec changed. Without it, `git log` shows no spec mutation; the journal entry is the only trace, and the only way to find it is to know it exists. Pairing the commit message with the amendment ID lets a future session that finds the journal entry navigate forward to the commit (and vice versa). The multi-repo discipline preserves this navigability when spec and code live apart.

**Alternatives considered.** Auto-applying without commit (let the operator commit later) — rejected; the commit-as-part-of-Phase-4 step is what makes the amendment atomic. Bundling multiple amendments into one commit — rejected; per-amendment commits let bisects pinpoint which amendment introduced a regression.

### 5.5 Phase 5 — Journal

**Purpose.** Append a structured journal entry recording the transition (what was, what is, why), so future sessions read the spec's history without re-deriving it.

**Behavior.** The agent appends to `JOURNAL_PATH` an entry with the fixed shape declared in [SKILL.md Phase 5 lines 136–149](../../.agents/skills/spec-amend/SKILL.md): a header (`## YYYY-MM-DD — Amendment YYYY-MM-DD-N`), short fields (Section amended / Trigger / Reason / Impact summary / Approver / Approved on / Status implication / Commit), and a `### Full record` sub-block with the structured Phase 2 amendment record pasted verbatim, Approver field filled in.

When `SPEC_REPO_ROOT` is set, the journal lives in `SPEC_REPO_ROOT`. The journal entry is staged and committed in that repo. The commit references the amendment ID and is paired with any code-side commit that prompted the amendment.

**Pattern invoked.** "The spec shows the new state; the journal preserves the transition." Architectural source for the multi-repo journal-commit-routing: [session-economy/architecture.md §5.3](../20260514-session-economy/architecture.md) — the *sibling design spec* authoritatively committing to the Phase 5 journal-commit note. Shape (i) §5-enumerated mapping: retro §5.5 ↔ session-economy §5.3 (addition 3 of the three). Verified at the date of this spec by reading session-economy §5.3 lines 143–145 directly.

**Why this design.** A spec section after an amendment looks the same as a spec section that was always that way. Without the journal entry, three-months-from-now-you cannot tell what changed, when, or why. The journal entry is the only durable evidence of the transition — the spec hides it by design (a clean revision is hard to read with strikethrough markup), and `git blame` answers "who changed it" but not "why was it changed" without reading the commit body.

**Alternatives considered.** Embedding the amendment record in the spec itself (e.g., a Status block under the amended section) — rejected; bloats the spec with revision history that should be elsewhere. Skipping the journal entry when the spec edit is one line — rejected; the one-line edits are exactly the ones that future sessions cannot tell are amendments without a journal trace.

### 5.6 Phase 6 — Downstream handoff

**Purpose.** State next actions explicitly so the implementer, reviewer, and next session each know what changes.

**Behavior.** The agent declares each of the following that applies: (a) **If a task was in progress when the amendment was triggered**, state whether work resumes against the amended spec, restarts from scratch, or is paused — the implementer needs to know; (b) **If a checkpoint was open**, state whether the checkpoint is closed, re-opened, or re-scoped — the reviewer needs to know; (c) **If the amendment invalidated completed work**, name the work and route to the skill that handles the redo (usually `spec-execute` re-running a task; sometimes `spec-write` re-decomposing); (d) **If the spec status reverted to Draft**, state who must re-approve and by when. The handoff is conversational, with explicit names for downstream skills and actors.

**Pattern invoked.** "Hand off explicitly; do not leave next-state implicit." Same disciplinary shape as [spec-execute §5.6 Phase 6 Update artifacts](../20260518-spec-execute-skill/architecture.md) (whose Behavior point 3 is the Next-task pointer) and [spec-review §5.8 Phase 8 Update artifacts](../20260518-spec-review-skill/architecture.md) (whose conditional update 4 is the next-task-ID statement) — both end-of-flow phases that name the next actor explicitly. Verified against [SKILL.md Phase 6 lines 156–164](../../.agents/skills/spec-amend/SKILL.md) at the date of this spec.

**Why this design.** Without Phase 6, the amendment is applied but downstream consumers do not know to consult it. The implementer may resume against pre-amendment memory; the reviewer may close a checkpoint that should re-open; the next session may pick up a task whose scope just changed. Naming the downstream actors explicitly closes those gaps.

**Alternatives considered.** Folding Phase 6 into Phase 5's journal entry — rejected; the journal is read on demand, not pushed to downstream actors. A conversational message after Phase 5 reaches the operator immediately and is what the operator uses to brief the next session.

### 5.7 Change classification (amendment / in-flight edit / rewrite)

**Purpose.** Distinguish the three change classes so the skill only fires on the case it is designed for.

**Behavior.** The agent classifies the proposed change at Phase 1's Orientation Report. **Amendment**: change to a Draft-or-later spec that has been committed or shared. Proceeds through all six phases. **In-flight edit**: change to a Draft spec that has not been committed or shared, while the original `spec-write` or `spec-design` session is still running. Does not go through this skill; the author just edits. **Rewrite**: the spec is so wrong that patching is dishonest. Stops this skill and routes to a new `spec-write` or `spec-design` session.

The boundary between *in-flight edit* and *amendment* is the spec having been **committed or shared** — the exact moment of "shared" is unspecified and surfaced as §13 OQ-1. The boundary between *amendment* and *rewrite* is *coherence* of the change: an amendment targets one section or a coherent set; if the diff touches half the spec, it is a rewrite. "Coherent" is unspecified and surfaced as §13 OQ-3.

**Pattern invoked.** "Three classes, not two" — the canonical [SKILL.md ROLE block lines 37–41](../../.agents/skills/spec-amend/SKILL.md) declares the trichotomy explicitly. Verified at the date of this spec.

**Why this design.** A two-class scheme (amendment / not-amendment) collapses in-flight edits and rewrites into a single residual category. Without the in-flight-edit class, the skill is invoked too eagerly during initial authoring and adds ceremonial overhead where none is needed. Without the rewrite class, the skill is invoked too tolerantly on sprawling changes that should restart authoring; the spec then accumulates "amendments" that are actually rewrites, and the journal becomes unreadable.

**Alternatives considered.** A binary "amendment / not-amendment" classification — rejected for the reasons above. A four-class scheme (e.g., adding "correction" for typo-only changes) — rejected; the cost of additional classes is more decision overhead at Phase 1, and typo-only changes are an in-flight-edit case in practice (they happen pre-share or are batched into the next genuine amendment).

### 5.8 Voice discipline

**Purpose.** Match the prose voice to the kind of statement being made.

**Behavior.** Imperative for amendment rules ("the agent does not proceed to Phase 4 without explicit approval", "amendments are not applied silently"). First-person plural for design intent ("we chose…"). Plain declarative for observations. No marketing language ("elegant," "robust," "scalable").

**Pattern invoked.** Voice discipline was declared at [N=2 §5.6](../20260518-spec-design-skill/architecture.md), omitted at [N=3](../20260518-spec-write-skill/architecture.md) (whose §5 covered phases + upstream-spec orientation + atomicity + test strategy + citation discipline + section template, with no Voice-discipline subsection), reintroduced at [N=4 §5.10](../20260518-spec-execute-skill/architecture.md), and carried forward at [N=5 §5.10](../20260518-spec-review-skill/architecture.md) (with the §5.10 Voice-discipline lineage corrected via amendment 2026-05-18-2 in commit `85821ca`). Carried forward at N=6 with the corrected lineage citation pattern: each cited subsection's heading text was verified against the actual cited file before commit.

**Why this design.** The voice signals what kind of statement the reader is parsing — rule, intent, or observation. Mixed voice produces ambiguous prose that LLM readers misinterpret. The N=5 amendment 2026-05-18-2 (commit `85821ca`) exemplifies the lineage-citation failure mode this subsection's "Pattern invoked" sub-block must avoid: citing a section that does not exist at the cited file. The authoring-time per-citation walk discipline named in [N=5 journal §"Pattern observation for N=6"](../20260518-spec-review-skill/journal.md) heads off this failure mode at authoring time.

**Alternatives considered.** Permitting any voice — rejected; introduces ambiguity. Single-voice everywhere — rejected; loses the rule/intent/observation distinction.

### 5.9 Portability rule for links

**Purpose.** Keep committed prose interpretable on any clone of the repo, regardless of the author's filesystem layout.

**Behavior.** Committed prose contains no absolute filesystem paths and no machine-specific paths. Order of preference for links: published URL → repo-relative path → sibling-relative description → bare name + host description. No `~/.claude/skills/...` references; the canonical path is [.agents/skills/...](../../.agents/skills/spec-amend/SKILL.md).

**Pattern invoked.** [project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md) (drop `~/.claude/skills/` references). Carried forward at every N≥2; carried forward at N=6.

**Why this design.** A spec is read on machines other than the author's. Absolute paths break for every other reader. The `.agents/skills/` path is the authoritative location in this repo; cloned/forked copies inherit it.

**Alternatives considered.** Permitting absolute paths in author-private prose — rejected. Spec is committed; nothing is "author-private."

## 6. Non-functional Requirements

| NFR | Requirement | Source |
|---|---|---|
| **Adoptability (ASPP)** | The skill must run portably against any host repo whose specs follow the methodology's §-numbered + journal shape. Degrades to a feature-spec amendment without `SPEC_REPO_ROOT` (single-repo case) and without sibling skills (Phase 6 handoff becomes a manual operator note rather than a skill invocation). | [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) |
| **Context economy** | Amendment record and journal entry formats are fixed and bounded; future sessions scan journal headers (`## YYYY-MM-DD — Amendment YYYY-MM-DD-N`) and field labels without re-reading the structured "Full record" sub-block. | [specs/tech-stack.md §44](../tech-stack.md#L44) |
| **Approval discipline** | Approval is explicit; silence is not approval. Phase 3 is structurally a stop. Operators may not bypass Phase 3 by directly invoking Phase 4 actions. | [SKILL.md OP §5 + Phase 3](../../.agents/skills/spec-amend/SKILL.md) |
| **Diff discipline** | Every amendment shows the change as before/after (or as a clearly marked addition). No "rewrite this section" without showing what the rewrite is. | [SKILL.md OP §2](../../.agents/skills/spec-amend/SKILL.md) |
| **Surgical-not-sprawling discipline** | An amendment targets one section or a coherent set of related sections. A diff touching half the spec is a rewrite, not an amendment, and routes accordingly. | [SKILL.md OP §1](../../.agents/skills/spec-amend/SKILL.md) |
| **Multi-repo discipline** | When `SPEC_REPO_ROOT` is set, spec edits + journal entries are committed in `SPEC_REPO_ROOT`, paired with the code-side commit that prompted the amendment. Architectural source: [session-economy §5.3](../20260514-session-economy/architecture.md). Shape (i) §5-enumerated attribution: retro §5.4 + §5.5 + INPUTS contract ↔ session-economy §5.3. | [SKILL.md INPUTS + Phase 4 Multi-repo + Phase 5 note](../../.agents/skills/spec-amend/SKILL.md); [session-economy §5.3](../20260514-session-economy/architecture.md) |
| **Visibility** | Amendments are first-class events with their own journal-entry shape, distinguishable from task closeouts or review verdicts. The journal entry is the durable record; the spec shows only the new state. | [SKILL.md Phase 5 + "Notes on what makes this skill load-bearing"](../../.agents/skills/spec-amend/SKILL.md) |
| **Class-classification discipline** | The Phase 1 Orientation Report explicitly classifies the change as `amendment` / `in-flight edit` / `rewrite`. `rewrite` stops the skill and routes to a new authoring session. | [SKILL.md ROLE block + Phase 1](../../.agents/skills/spec-amend/SKILL.md) |

## 7. Implementation Sequencing

The skill is already implemented. This spec retroactively documents what ships. Sequencing for the spec itself:

1. **Author this `architecture.md` + paired `journal.md`** (this session). Closes the authoring phase.
2. **CP-1 review** in a fresh session. Closes the faithfulness phase.
3. **CP-2 drift audit** in the batched quintet-CP-2 session per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). Closes the drift-baseline phase.
4. **(Out of this spec's scope but recorded in journal Next-action)** Author `docs/retroactive-spec-pattern.md` as the closing artifact of the retroactive-spec series, after CP-2 batch audit produces its evidence base.

After step 3, this spec moves out of `Draft — Open for Review` to its post-CP-2 status (per N=1 / N=2 / N=3 / N=4 / N=5 convention, the banner is amended at that point).

## 8. Validation Approach

| Approach | What it validates |
|---|---|
| **Stakeholder review** | Eric (operator) reviews this spec for fidelity to intent. CP-1 is the gate. |
| **Drift audit** | Mechanical comparison of SKILL.md commitments to this spec's commitments. CP-2 is the gate. Output is a divergence list (possibly empty). Batched with the four sibling quintet specs and project-constitution's pending CP-2. |
| **Predecessor cross-check** | [docs/spec-driven-development-prompts-conversation.md lines 391–403](../../docs/spec-driven-development-prompts-conversation.md) (AMENDMENT PROTOCOL block embedded in the `spec-execute-prompt.md` artifact) plus the design-note paragraph at line 414 is the skill's design-rationale source. Every spec-side commitment in §5 of this document traces to either a behavior in SKILL.md (current) or to a recommendation in the predecessor block (rationale). Gaps between predecessor and SKILL.md are *evolution* (trilogy commit `49c15f0` extraction, session-economy commit `e483466`, path-convention commit `6d158fb`), not drift. CP-2 reads this distinction with awareness that the predecessor is **inline, not standalone**, distinct from N=2–N=5. |
| **Sibling design-spec cross-check** | [specs/20260514-session-economy/architecture.md §5.3](../20260514-session-economy/architecture.md) is the *sibling* design spec authoritative for current behavior added after the predecessor. Only **shape (i) §5-enumerated** attribution applies (no narrative-sourced shape (ii) is exercised — matches N=5's simplicity). Mapping: retro §5.4 (Phase 4 Apply, multi-repo paragraph) and retro §5.5 (Phase 5 Journal, multi-repo note) and the `SPEC_REPO_ROOT` INPUTS contract all trace to session-economy §5.3, which prescribes **three** additions to spec-amend SKILL.md. CP-2 reads cross-spec consistency under shape (i). |
| **Downstream consumption** | The skill has been invoked twice in the retroactive-spec series to date: amendment 2026-05-18-1 against [spec-execute architecture.md §5.4 + §5.6](../20260518-spec-execute-skill/architecture.md) (commit `7fee46f`) — citation-error correction post-CP-1; amendment 2026-05-18-2 against [spec-review architecture.md §5.10](../20260518-spec-review-skill/architecture.md) (commit `85821ca`) — Voice-discipline lineage citation correction post-CP-1. Plus the spec-review's [Status backfill (commit `0ccc644`)](../20260518-spec-review-skill/architecture.md) — a CP-1 Status line backfill. Each invocation produced a structured amendment record, paired commit, and journal entry in the expected format. Two amendment cycles + one Status backfill = three invocations is the evidence base that the workflow produces durable, reviewable, future-readable revision records. The shared structural shape across both amendment cycles — citation-error in cross-spec lineage prose — is the evidence basis for the authoring-time per-citation walk discipline this spec adopts at §2 Goals. |

> Note: This section deliberately differs from a feature spec's Test Strategy. Design specs are validated by review, audit, predecessor cross-check, sibling-spec cross-check, and downstream consumption — not by automated test coverage.

## 9. Review Checkpoints

### CP-1 — Retroactive spec faithfully describes the shipping skill

**Trigger.** This spec and its journal are committed; the operator invokes `/spec-review` against this spec's CP-1 in a fresh session.

**Review focus.**
- Every commitment in §4, §5, and §6 corresponds to behavior actually present in [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md).
- No commitment in this spec contradicts the shipping SKILL.md.
- The **Atomic-Skill Portability Principle** is correctly characterized as a binding constraint (§3, §6) including degradation behavior when `SPEC_REPO_ROOT` is absent and when the amended artifact is a design spec rather than a feature spec.
- The **predecessor block** is correctly distinguished as inline-not-standalone, authoritative-for-design-rationale-not-current-behavior (§3 Background, §14 Inspirational). The inline-extraction story from the `spec-execute-prompt.md` artifact (lines 391–403 + line 414) at trilogy commit `49c15f0` is recorded faithfully.
- The **session-economy spec** is correctly distinguished as a *sibling design spec* authoritative for current behavior added after the predecessor — cited Authoritative in §14. Only **shape (i) §5-enumerated** attribution applies: retro §5.4 + §5.5 + `SPEC_REPO_ROOT` INPUTS entry map to session-economy §5.3, which prescribes **three** additions (the spec records this as a refinement of the N=5 journal's "two additions" prediction).
- The six phases in §5 (§5.1–§5.6) match the shipping SKILL.md's phase structure. §5.7 Change classification (amendment / in-flight edit / rewrite) accurately reflects the [SKILL.md ROLE block](../../.agents/skills/spec-amend/SKILL.md).
- §13 OQ-1 (in-flight/amendment boundary), OQ-2 (re-approval path after status revert), OQ-3 (multi-finding batching scope), and OQ-4 (cross-skill amendment coordination) are each named with full Question / Analysis / Leaning / Owner / Watch items / Anti-goals structure.
- The spec is self-contained per the Operating Principles in [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md).
- Section-heading citations point to the heading line, not the body's first line (Pattern-for-N=5 #3 carried forward from N=5).
- **Authoring-time per-citation walk discipline** (named in [N=5 journal §"Pattern observation for N=6"](../20260518-spec-review-skill/journal.md)) — every "Pattern invoked" citation in §5 was walked against its actual cited subsection (heading text + content match, not just heading line target) before commit. CP-1 reviewer verifies a sample.
- The portability rule for links is honored: no `~/.claude/skills/...` references, no absolute filesystem paths.

**Exit criteria.**
- Reviewer issues a verdict of `pass`, `pass with comments`, or `changes requested` per the structured format declared in [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- All `[blocker]` findings (if any) are resolved or escalated to `/spec-amend`.
- Verdict is written back to this spec's §9 (status line) and to the journal.

**Status:** pass with comments on 2026-05-18 by Claude (AI assistant) on behalf of Eric Wasgatt — one [important] citation error in §5.9 (Portability rule "Pattern invoked" cited `docs/retroactive-spec-strategy.md` as holder of Amendment 2026-05-17-1; actual location is `specs/20260517-project-constitution-skill/journal.md`) resolved via amendment 2026-05-18-3 (commits `7a33abe` + `c01488a`); two advisories (A1 §5.3 "Phase 7 — Checkpoint Gate" → "gate" capitalization bundled into the same amendment per operator scope decision Q1; A2 per-citation walk scope refinement carried forward to the pattern doc post-CP-2). See [journal](./journal.md) entry of same date. Verdict commit: SHA backfilled below.

### CP-2 — Drift audit complete (batched)

**Trigger.** CP-1 of this spec passes, AND CP-1 of [spec-design](../20260518-spec-design-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND CP-1 of [spec-write](../20260518-spec-write-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND CP-1 of [spec-execute](../20260518-spec-execute-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18, after amendment 2026-05-18-1 commit `7fee46f`), AND CP-1 of [spec-review](../20260518-spec-review-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18, after amendment 2026-05-18-2 commit `85821ca`), AND project-constitution's CP-2 has either run or been folded into the batch per [docs/retroactive-spec-strategy.md OQ-1](../../docs/retroactive-spec-strategy.md). With this spec being the final quintet retroactive spec, the **remaining condition reduces to "project-constitution CP-2 only"** (assuming this spec's CP-1 passes).

**Review focus.** A line-by-line audit of [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) against this spec's §4, §5, §6, and §12. The auditor enumerates each divergence: a behavior present in SKILL.md but not committed in the spec, or a commitment in the spec absent from SKILL.md. Cross-skill drift patterns (e.g., four quintet specs citing the Atomic-Skill Portability Principle correctly and one quietly not; or session-economy commitments inconsistently propagated across the quintet) are explicitly in scope by virtue of the batch context — this being the final quintet retroactive spec, the batch context is now complete. The cross-spec consistency check between this spec and the [session-economy spec](../20260514-session-economy/architecture.md) is a CP-2 line item under **shape (i) §5-enumerated** only: retro §5.4 + §5.5 + `SPEC_REPO_ROOT` INPUTS contract map to session-economy §5.3. CP-2 verifies this mapping; no shape (ii) check is needed because no shape (ii) mappings are claimed.

**Exit criteria.**
- Divergence list produced (possibly empty).
- For each divergence, a routing decision: (a) amend the spec to reflect SKILL.md behavior, (b) amend SKILL.md to match the spec, or (c) accept as a known minor discrepancy with rationale recorded in the journal.
- No silent edits to either artifact.
- Outcome recorded in this spec's journal as the closing entry of the retroactive-spec adoption.
- The cross-skill drift patterns are explicitly audited as a CP-2-only check (single-spec CP-2s cannot see them); the final retroactive-spec batch is the only moment this audit is performed.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Spec drifts from shipping SKILL.md silently after adoption. | Medium over time | High — the spec stops being trustworthy as a contract | Amendment Protocol via `/spec-amend` (self-referentially); CP-2 establishes the initial baseline; subsequent SKILL.md changes are spec-amended in the same change | Eric / future maintainers |
| Spec is wrong (misdescribes the skill, omits material behavior, asserts commitments the skill does not honor). | Low–Medium | Medium — early review catches it | CP-1 review with explicit "faithfulness to shipping SKILL.md" as the central focus area; **authoring-time per-citation walk discipline** ([N=5 journal pattern observation](../20260518-spec-review-skill/journal.md)) heads off the cross-spec lineage-citation failure mode at authoring time rather than CP-1 | Reviewer (CP-1); author (per-citation walk) |
| The two-source structure causes CP-2 false positives — auditor reads behavior present in SKILL.md but absent from the predecessor as drift, missing that the session-economy spec is the authoritative source for `SPEC_REPO_ROOT`, the Phase 4 multi-repo paragraph, and the Phase 5 journal-commit note. | Medium | Medium | §3 Background and §14 References explicitly distinguish the two sources; §8 Validation Approach declares both cross-check rows; CP-1 review focus includes the session-economy-spec consistency check; CP-2 reviewer reads §3 / §8 / §14 before walking divergences. Same mitigation pattern as N=4 / N=5. | CP-2 auditor |
| The predecessor's **inline-not-standalone** shape (unique to N=6) causes a CP-1 or CP-2 reviewer to look for a missing `spec-amend-prompt.md` artifact and conclude the predecessor citation is wrong. | Medium (one-time, this spec only) | Low–Medium | §3 Background, §14 Inspirational, and §2 Goals all call out the inline-not-standalone shape explicitly. The journal records the structural distinction as a friction note for the CP-1 reviewer. | CP-1 / CP-2 reviewer |
| §13 OQs (in-flight/amendment boundary; re-approval after status revert; multi-finding batching scope; cross-skill amendment coordination) resolve silently as improvisation in each session — operator and approver do the right thing without recording the pattern, and the SKILL.md gaps stay unwritten. | Medium per OQ | Low–Medium | Each OQ's Watch items capture revisit conditions; the Amendment Protocol creates a route when each gap surfaces as friction. CP-2 reviewer may flag if an OQ is still open after >N more amendment sessions. | Eric (per-session); future amendment session |
| **Self-referential amendment paradox** — an amendment to *this* spec must itself go through `spec-amend`, and an amendment to `spec-amend` SKILL.md must also go through the very skill being amended. Risk: the skill is unable to amend its own SKILL.md cleanly if the amendment changes the skill's amendment workflow. | Low (gap has not surfaced in practice) | Medium when it surfaces | Recorded as a meta-observation in §11 Adoption Path and in the journal. Mitigation: amendments to spec-amend SKILL.md operate under the skill's *current* (pre-amendment) workflow, applied to the proposed *next* (post-amendment) state — i.e., always one version behind. Same shape as compilers self-compiling: bootstrap from the prior version. | Eric; future amendment session that touches spec-amend SKILL.md |
| N=6 patterns over-codify, since this is the final retroactive spec — any "Pattern for N=7" callouts have no consumer. | Low | Low | This spec's journal explicitly does *not* declare "Pattern for N=7" callouts. The journal closes the pattern-mining loop and hands off to `docs/retroactive-spec-pattern.md` (to be authored post-CP-2). | Operator; pattern-doc author |
| Self-review bias — Eric, as both author and reviewer of this spec's CP-1, waves through advisory findings that warrant attention. | Medium | Low | The shipping spec-review SKILL.md's REVIEWER NOTES section explicitly counsels self-review honesty about advisories; CP-1 reviewer (even if Eric or an AI assistant on his behalf) runs the full prompt and emits the structured verdict format, which surfaces advisories independently of any handwaved global verdict. | CP-1 reviewer |

## 11. Adoption Path

The spec is adopted in three steps, matching N=1 / N=2 / N=3 / N=4 / N=5:

1. **Commit the spec and journal** as a paired commit. The journal is the durable record of this session's decisions and observations.
2. **CP-1 review** in a fresh session (sequenced, per operator's choice): invoke `/spec-review` against this spec's CP-1.
3. **CP-2 drift audit** in the batched quintet-CP-2 session: per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). With this spec being the final quintet retroactive spec, the batch context is now complete.

After adoption, the spec is a *living contract*. Edits to SKILL.md follow the Amendment Protocol — including, self-referentially, edits to the **spec-amend SKILL.md itself**. The self-referential case is the meta-observation this spec makes: amendments to the amendment skill operate under the skill's *current* (pre-amendment) workflow, applied to the proposed *next* (post-amendment) state. Per the [N=1 amendment 2026-05-17-1 N=2 mining note](../20260517-project-constitution-skill/journal.md), amendments that touch a *class* of references (paths, citations, vocabulary) must scan the entire spec at Phase 1 Orient, not just the locations called out by the triggering finding.

### Reversibility

The spec can be retired without affecting the skill. The SKILL.md remains the canonical implementation. Retirement would be unusual (it would be the methodology rejecting its own dogfooding convention for `spec-amend` specifically) but mechanically clean: delete the spec directory and record the retirement in `roadmap.md`.

### Cross-session knowledge transfer

This is the **final** quintet retroactive spec. Its [journal.md](./journal.md) records validation/refinement/rejection outcomes for each "Pattern for N=6" callout from the [N=5 journal](../20260518-spec-review-skill/journal.md) (including the post-CP-1 authoring-time-citation-walk observation), and explicitly declines to declare "Pattern for N=7" callouts because there is no N=7 consumer. The pattern-mining loop closes here. Next action: author `docs/retroactive-spec-pattern.md` as the closing artifact of the retroactive-spec series, drawing on the consolidated evidence base of N=1 through N=6 journals plus the CP-2 batch audit findings.

## 12. Out of Scope

- **Resolving §13 OQ-1 (in-flight/amendment boundary).** The OQ is named; resolution routes to a future amendment session when watch conditions trigger.
- **Resolving §13 OQ-2 (re-approval path after status revert).** The OQ is named; resolution requires the first observed `Approved → Draft → Approved` cycle in practice.
- **Resolving §13 OQ-3 (multi-finding batching scope).** The OQ is named; resolution requires the first observed multi-finding amendment that forces a coherence judgment.
- **Resolving §13 OQ-4 (cross-skill amendment coordination).** The OQ is named; the strategy doc's [OQ-3](../../docs/retroactive-spec-strategy.md) named this venue as the resolution point, but the operator chose at this session's Phase 2 to surface as §13 OQ rather than resolve unilaterally without an instance to anchor on.
- **Multi-repo `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` mechanics under multi-target conditions** (one feature spec spanning multiple downstream code repos; spec-repo lifecycle reflected in adopter repos). The single-repo case is the common case in `ai-tools`; the multi-repo two-repo case is the architecturally-committed configuration. Multi-*target* (>2 repos) mechanics are not bounded by this spec. Mirrors N=2 / N=3 / N=4 / N=5's identical disposition.
- **Resolving the format-question-prompt gap** ([N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md)). Methodology-wide; not re-raised here.
- **Reviewer-attribution convention under AI assistance** (e.g., "Claude (AI assistant) on behalf of Eric Wasgatt"). Methodology-wide convention, surfaced at [N=5 §12](../20260518-spec-review-skill/architecture.md); inherited here as the same disposition — out of scope for this spec, candidate for a future methodology-wide amendment session if friction surfaces.
- **Cross-spec citation review as a Phase 3 / Phase 5 explicit check in spec-review.** Inherited from [N=5 §12](../20260518-spec-review-skill/architecture.md); gap is patched per-spec via authoring-time per-citation walks (adopted at this spec's §2 Goals), not from inside spec-review SKILL.md.
- **The `docs/retroactive-spec-pattern.md` decision.** Deferred at N=3 close, deferred again at N=5 close, **now resolved at this session's Phase 2** as "author as closing artifact post-CP-2." The pattern doc itself is out of scope for this spec; the journal records it as the next-action handoff.
- **Constitution-amendment ceremony.** Inherited from [N=1 OQ-1](../20260517-project-constitution-skill/architecture.md) and tracked at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md). Out of scope here.
- **Redesign of the `spec-amend` skill.** Redesign routes to a new design spec under amendment governance — which, self-referentially, would operate through `spec-amend` itself (see §11).
- **Modification of the shipping SKILL.md.** Only `/spec-amend` touches SKILL.md.
- **Mechanism for committing an amendment to spec-amend SKILL.md atomically with the spec change describing the amendment.** Self-referential edge case; the staged-bootstrap mitigation in §10 ("always one version behind") is the operational answer. Documented but not actionable from inside SKILL.md.
- **External-claim verification beyond repo-internal citation.** Light verification was adopted for this spec; the heavy-verification path exists in the methodology but is not exercised by this spec's text.

## 13. Open Questions

### OQ-1 — In-flight edit vs. amendment boundary: when is a Draft "shared"?

**Question.** SKILL.md's class classification ([ROLE block lines 37–41](../../.agents/skills/spec-amend/SKILL.md)) distinguishes "amendment" (change to a spec that has been committed or shared) from "in-flight edit" (change to a Draft spec that has not been committed or shared). The phrase "committed or shared" admits at least three interpretations: (a) the first `git commit` of the spec; (b) the first push or PR opening; (c) any chat-channel link sharing prior to a commit. Reviewers and operators have improvised the boundary case-by-case; the cost of the wrong call is either invoking the amendment skill prematurely (in-flight edit treated as amendment, ceremonial overhead) or treating an amendment as in-flight edit (silent revision of a shared contract). Which interpretation governs?

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) **Commit is the boundary.** Any spec with a `git log` entry is amendment-class; any draft without one is in-flight. | Mechanical, observable in `git log`. SKILL.md could add one line. | Sharpest line. Cost: a spec drafted in chat and shared in chat (no commit yet) is then "in-flight" — but it has been shared, and silent revision is exactly the failure mode this skill prevents. |
| (b) **Sharing is the boundary, defined as "another human has read the draft."** | Conservative: any non-author reader makes the draft amendment-class. | Hardest to mechanize but matches the operating principle's intent. Cost: not observable from artifacts; depends on chat history that may not survive. |
| (c) **Status banner is the boundary.** Any spec whose banner is not literally `Draft (in-flight)` is amendment-class. | Adds a banner level: `Draft (in-flight)` → `Draft — Open for Review` → `Approved`. Mechanically observable. | New banner state to maintain; risk of stale banners. SKILL.md (and spec-write / spec-design) would need amendments. |
| (d) **Hybrid: commit OR explicit "Draft — Open for Review" banner.** Either trigger marks the spec amendment-class. | Most expressive; covers commit-without-review and review-without-commit cases. | Two conditions to remember; minor decision overhead at Phase 1. |

This OQ has a weak structural relationship to §13 OQ-3 (multi-finding batching scope): both surface as "SKILL.md leaves a coherence/boundary judgment to the operator." Cross-reference noted; mitigation patterns may overlap.

**Leaning.** **(d) is the most operationally honest position.** Operators in practice have used both signals — committed-and-shared is amendment; uncommitted-but-shared (e.g., a chat link to a draft) has also been treated as amendment. The hybrid trigger captures both. **Recommended direction: (d), with the SKILL.md amendment to add one paragraph to the ROLE block specifying the dual trigger.** No formal commitment until the first observed disagreement between author and reviewer about which class applies.

**Owner.** A future `/spec-amend` session triggered by the watch item below.

**Watch items.**
- An operator and reviewer disagree about whether a specific change is in-flight edit or amendment, in a context where the cost of the wrong call is non-trivial. Signal: the disagreement surfaces in chat or a verdict.
- A spec is silently revised without a commit during what was thought to be its in-flight period, and the silent revision is later discovered to have changed a shared interpretation. Signal: post-hoc archaeology in chat history.

**Anti-goals.**
- **Do not** add a new banner state (`Draft (in-flight)`) without first observing whether operators actually maintain banners reliably. New banner states ossify into stale labels if they are not load-bearing.
- **Do not** specify the boundary as "intent of the author" — intent is unverifiable post-hoc. Mechanical or banner-state triggers are reviewable; intent-based triggers are not.

### OQ-2 — Re-approval path when an amendment reverts `Approved → Draft`

**Question.** SKILL.md OP §7 ("Status, if present, advances") and Phase 2's "Status implication" field allow an amendment to revert a spec from `Approved` to `Draft`. The mechanics of returning the spec to `Approved` are unspecified: (a) does the re-approval require a full new CP-1 review, or a focused review of only the amended sections; (b) who must re-approve — the original approver, any approver, or a named role; (c) is there a new Status banner state (e.g., `Draft (post-amendment, awaiting re-approval)`) or does the spec sit in plain `Draft` until re-approval; (d) what happens to in-flight `spec-execute` work against the reverted spec — pause, continue, or halt? The N=5 amendment 2026-05-18-2 explicitly cited the absence of evidence ("§13 OQ-2 leaning (d): do not codify re-review/Status-mutation mechanics on one cycle's evidence") — the gap is named there too but in a different context.

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) **Focused re-review of amended sections only.** A new `/spec-review` invocation targeting the amended sections + any cross-references; verdict closes a new "CP-A" (amendment-review) checkpoint declared at amendment time. | Proportional to the change. Cost: requires a new checkpoint type at amendment time, which has no precedent in `spec-design` / `spec-write` SKILL.md. |
| (b) **Full new CP-1 review.** Treat the amended spec as a new artifact; re-run CP-1 verbatim. | Maximally conservative; safe. Cost: high friction for small amendments; discourages amendments that would actually be net-positive. |
| (c) **Same approver as the original CP-1 verdict re-approves via lightweight ack** ("I read the amendment record, no new concerns"); no formal verdict required. | Lightweight. Cost: no structured record of re-approval; depends on the same approver being available. |
| (d) **Defer until first observed Approved→Draft→Approved cycle; codify based on evidence.** | Honest about evidence; no premature formalization. Cost: gap stays unwritten longer. |

This OQ has a moderate structural relationship to §13 OQ-1 (in-flight/amendment boundary): both touch the spec-lifecycle status machine; resolution patterns may need to be consistent.

**Leaning.** **(d) is the most honest current position.** No `Approved → Draft → Approved` cycle has occurred in the retroactive-spec series (all CP-1 verdicts were `pass with comments`, not `changes requested` blocking; status banners moved `Draft → Draft post-CP-1` to `(post-CP-2)`, never reverting). When the first such cycle occurs, the operational pattern can be observed and codified at (a) or (c) depending on the scale of the amendment. **Recommended direction: (d) until the watch condition triggers; codification when evidence exists.**

**Owner.** A future `/spec-amend` session triggered by the watch item below.

**Watch items.**
- A spec at `Approved` status receives an amendment whose Status implication is "revert to Draft." Signal: first `Approved → Draft` transition in any spec.
- A reverted-to-Draft spec needs to advance back to `Approved`. Signal: first re-approval cycle.

**Anti-goals.**
- **Do not** codify re-approval mechanics on hypothetical scenarios. The N=5 amendment's leaning (d) is the same disciplinary commitment in a different context.
- **Do not** introduce a new banner state (`Draft (post-amendment, awaiting re-approval)`) unless the first observed cycle actually needs to disambiguate. Banner proliferation increases maintenance cost faster than it adds expressiveness.

### OQ-3 — Multi-finding batching scope: when do multiple findings batch into one amendment vs. split into many?

**Question.** SKILL.md OP §1 ("Surgical, not sprawling — targets one section, or a coherent set of related sections") and OP "Do not bundle unrelated changes into one amendment" together leave the word "coherent" undefined. When a CP-1 review surfaces, say, three [important] findings — one citation error in §5.2, one missing cross-reference in §5.4, and one missing NFR row in §6 — should they batch into one amendment (because they all flow from the same review), or split into three (because they touch three sections)? Or split into two (citation cluster vs. NFR addition)?

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) **One amendment per finding.** Mechanical: every finding is its own amendment with its own ID, commit, journal entry. | Sharpest separation; easiest bisect; simplest mental model. Cost: amendment-overhead per finding; journal noise if reviews routinely produce >3 findings. |
| (b) **One amendment per CP-1 review verdict.** Batch all findings from the same review into one amendment with multiple sub-changes. | Matches the unit of decision (the verdict); reduces journal noise. Cost: blurs the "surgical" principle when findings touch unrelated sections. |
| (c) **Operator judgment, anchored to "coherent set of related sections."** No mechanical rule; operator decides per amendment whether to batch or split. | Matches current practice (N=4 amendment 2026-05-18-1 batched two §5 subsections' citation errors; N=5 amendment 2026-05-18-2 was a single subsection's citation correction). Cost: depends on operator judgment quality. |
| (d) **Two amendments: one for spec-side citation errors, one for SKILL.md gap noting** if the review surfaces both kinds. Otherwise (c). | Adds one explicit rule on top of (c). Cost: only addresses one specific batching question. |

This OQ has a weak structural relationship to §13 OQ-1 (in-flight/amendment boundary): both surface as "SKILL.md leaves a coherence/boundary judgment to the operator." Cross-reference noted; mitigation patterns may overlap. Cross-references also OQ-4 (cross-skill amendment coordination) — that OQ asks the analogous batching question across *skills*, this asks within a single spec.

**Leaning.** **(c) is the current de-facto resolution**, with the existing two-amendment evidence base ([N=4 amendment 2026-05-18-1](../20260518-spec-execute-skill/journal.md) — two §5 subsections batched; [N=5 amendment 2026-05-18-2](../20260518-spec-review-skill/journal.md) — single subsection) consistent with "one coherent set per amendment." **Recommended direction: (c) as the convention, with (b) as a formalization path when CP-1 reviews routinely produce findings touching >3 unrelated sections.** No formal commitment until the operator faces a CP-1 verdict with findings that genuinely span unrelated sections.

**Owner.** A future `/spec-amend` session triggered by the watch item below.

**Watch items.**
- A CP-1 verdict surfaces findings touching unrelated sections (no plausible single "coherent" frame) and the operator finds the (c) judgment ambiguous. Signal: explicit Slack/chat question about how to batch.
- Three or more amendment cycles in a single spec's CP-1 stack produce inconsistent batching choices. Signal: the journal-mining reader observes drift in batching convention.

**Anti-goals.**
- **Do not** mechanize batching prematurely. The (c) convention has worked for the only two amendment cycles in the retroactive-spec series; mechanizing on this evidence base would over-fit.
- **Do not** introduce a "review-verdict-amendment" type distinct from regular amendments. The structured-amendment format is the unit; differentiating types adds taxonomy without benefit on current evidence.

### OQ-4 — Cross-skill amendment coordination

**Question.** When a change must propagate across multiple SKILL.md files — a vocabulary change, a path-convention update, a shared INPUTS field — the current model is N independent `/spec-amend` sessions, one per skill, with no mechanical coordination beyond the operator's memory and the commits' message references. The [strategy doc OQ-3](../../docs/retroactive-spec-strategy.md) named this venue (the `spec-amend` retroactive spec at session 5) as the resolution point, but the operator chose at this session's Phase 2 to surface as §13 OQ rather than resolve unilaterally without an instance to anchor on. The actual cross-skill amendments to date — `e483466` (session-economy commit, touched four sibling skills) and `6d158fb` (path-convention update, touched six skills) — predate the trilogy commit's separation between propose and apply and so did not actually fire `/spec-amend` per-skill. The first instance of a true cross-skill amendment cycle after the propose/apply separation has not yet occurred. What coordination mechanism, if any, should exist?

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) **N independent amendments with a shared amendment-ID prefix.** Each `/spec-amend` session uses an ID like `2026-MM-DD-N-shared-X` where X is the cross-skill change. Operator commits each in its own skill's spec directory; the shared X ties them together. | Lightweight; no new skill. Cost: depends on operator discipline; nothing mechanical enforces atomicity. |
| (b) **A new `cross-skill-amend` skill that orchestrates N `/spec-amend` invocations.** Single entry point, fans out to each touched skill's spec directory, ensures all N apply or none. | Strongest atomicity. Cost: significant new skill; only valuable when cross-skill amendments are frequent. |
| (c) **Document the convention in a §-of-spec-amend addition: "Cross-skill case."** A paragraph in spec-amend SKILL.md naming the convention, similar to the "Multi-repo case" paragraphs. | Lightweight; SKILL.md grows by one paragraph. Cost: convention without mechanical enforcement. |
| (d) **Defer until the first true post-propose/apply-separation cross-skill amendment cycle occurs.** No infrastructure now. | Honest about evidence. Cost: gap stays unwritten until a cycle forces the question. |

This OQ has a moderate structural relationship to §13 OQ-3 (multi-finding batching scope): both ask "what is the unit of an amendment?" — OQ-3 asks within a spec, OQ-4 asks across specs.

**Leaning.** **(d) is the most honest current position, with (c) as the formalization path once evidence exists.** The strategy doc named this venue but did not name evidence — no post-trilogy-commit cross-skill amendment has fired yet. The two pre-trilogy cross-skill changes (`e483466`, `6d158fb`) used a different model (single commit touching multiple skills directly) that the trilogy commit was specifically designed to deprecate. **Recommended direction: (d) until the watch condition triggers; (c) as the codification path.**

**Owner.** A future `/spec-amend` session triggered by the watch item below.

**Watch items.**
- A vocabulary change, schema update, or shared INPUTS field amendment surfaces and must propagate across two or more skills' SKILL.md files. Signal: first post-trilogy cross-skill amendment cycle.
- The CP-2 batch audit surfaces a divergence pattern that would have been caught by a single cross-skill amendment but was missed by N independent ones. Signal: CP-2 evidence motivates (c).

**Anti-goals.**
- **Do not** author the `cross-skill-amend` skill (option b) on zero post-trilogy evidence. Skill proliferation without evidence is over-engineering.
- **Do not** retro-fit the pre-trilogy cross-skill changes (`e483466`, `6d158fb`) as "what cross-skill amendments should look like." Those changes operated under the inline-amendment-in-spec-execute model the trilogy commit deprecated; they are not evidence for the post-trilogy design space.

## 14. References

### Authoritative

- [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) — the shipping skill. Authoritative for behavior. Verified at the date of this spec.
- [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) — Atomic-Skill Portability Principle. Authoritative for ASPP. Verified at the date of this spec.
- [specs/tech-stack.md §44](../tech-stack.md#L44) — AI context window limits. Authoritative for context-economy constraints.
- [specs/tech-stack.md §48](../tech-stack.md#L48) — Repository layout convention. Authoritative for `SPEC_PATH` / `JOURNAL_PATH` shape.
- [specs/tech-stack.md §51](../tech-stack.md#L51) — Spec-driven-development convention. Authoritative for the methodology-eats-its-own-cooking justification.
- [specs/mission.md](../mission.md) — repo mission, including the Out of Scope items (tooling-agnostic; no domain variants in core).
- [specs/20260514-session-economy/architecture.md §5.3](../20260514-session-economy/architecture.md) — sibling design spec authoritative for current behavior added after the predecessor (the `SPEC_REPO_ROOT` INPUTS entry, the Phase 4 "Multi-repo case" paragraph, and the Phase 5 journal-commit note). **Three** SKILL.md additions, not two. Shape (i) §5-enumerated attribution: retro §5.4 + §5.5 + INPUTS contract ↔ session-economy §5.3. Verified at the date of this spec.

### Inspirational

- [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 391–403 + line 414 — predecessor of this skill. **Inline, not standalone**: the AMENDMENT PROTOCOL block was embedded inside the `spec-execute-prompt.md` artifact (lines 391–403) plus the design-note paragraph at line 414. Extracted into the standalone `spec-amend` skill at trilogy commit `49c15f0` (2026-05-14). Structurally distinct from N=2 / N=3 / N=4 / N=5 predecessors, each of which had a standalone artifact. Authoritative for design rationale; not authoritative for current behavior.
- [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) — operating principles for the retroactive-spec series. Read as orientation; not used as a source for §4/§5 architectural commitments.
- [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) and [journal.md](../20260517-project-constitution-skill/journal.md) — N=1 retroactive-spec source.
- [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) and [journal.md](../20260518-spec-design-skill/journal.md) — N=2 retroactive-spec source.
- [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) and [journal.md](../20260518-spec-write-skill/journal.md) — N=3 retroactive-spec source.
- [specs/20260518-spec-execute-skill/architecture.md](../20260518-spec-execute-skill/architecture.md) and [journal.md](../20260518-spec-execute-skill/journal.md) — N=4 retroactive-spec source; origin of the two-source structure.
- [specs/20260518-spec-review-skill/architecture.md](../20260518-spec-review-skill/architecture.md) and [journal.md](../20260518-spec-review-skill/journal.md) — N=5 retroactive-spec source; closest-sibling structural source; origin of the authoring-time per-citation walk discipline.
