# `spec-review` Skill — Architecture and Protocol Specification

> Status: Approved — CP-2 closed 2026-05-18
> Date: 2026-05-18
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set.

## 1. Overview

The `spec-review` skill is the methodology's **checkpoint review skill**: it walks a body of work against a specific Review Checkpoint declared in a spec, produces a structured verdict (`pass` / `pass with comments` / `changes requested` / `blocked`), and writes the outcome back to the spec and journal so subsequent sessions know whether execution may proceed. It is the only quintet member whose primary input is *not* a spec being authored or executed — it consumes a spec already authored and a body of work already produced, and emits a verdict that gates what happens next. It sits laterally to [spec-execute](../../.agents/skills/spec-execute/SKILL.md) (Phase 7 hands off here when a checkpoint triggers), routes findings to [spec-amend](../../.agents/skills/spec-amend/SKILL.md) when drift surfaces, and inherits architectural commitments from the constitution ([specs/tech-stack.md](../tech-stack.md)) and from a sibling design spec ([specs/20260514-session-economy/architecture.md §5.4](../20260514-session-economy/architecture.md)) that postdates the skill's predecessor and authoritatively shaped the INPUTS block and Phase 8.

This document is a **retroactive design specification**: the skill already ships at [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md), which is authoritative for the skill's behavior. The spec describes what the skill *is* and what it *commits to*. It does not redesign the skill; any divergence the spec exposes between its commitments and the shipping SKILL.md is routed to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md), never silently corrected.

## 2. Goals and Non-goals

### Goals

- Produce a self-contained, descriptive specification of the `spec-review` skill's vocabulary, contract, phases, verdict format, and reviewer-shapes coverage (human / agent / self-review).
- Declare review gates (§9) so the skill becomes reviewable via [/spec-review](../../.agents/skills/spec-review/SKILL.md) against named checkpoints.
- Hold the skill to the **Atomic-Skill Portability Principle** declared in the methodology's constitution ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)) — including degradation when optional inputs (`SPEC_REPO_ROOT`) are absent and when the reviewed artifact is a design spec rather than a feature spec.
- Continue the **paired-artifact pattern** for retroactive skill specs — `architecture.md` + `journal.md` — established at N=1 ([specs/20260517-project-constitution-skill/](../20260517-project-constitution-skill/)), refined at N=2 ([specs/20260518-spec-design-skill/](../20260518-spec-design-skill/)) and N=3 ([specs/20260518-spec-write-skill/](../20260518-spec-write-skill/)), and extended at N=4 ([specs/20260518-spec-execute-skill/](../20260518-spec-execute-skill/)) with the two-source structure. This N=5 instance validates, refines, or rejects each of the N=4 journal's "Pattern for N=5" callouts.
- Apply the **two-source structure** (predecessor for design rationale; sibling design spec for current behavior added after the predecessor) in its **simplest shape encountered so far**: predecessor is rich (lines 446–663 of the shared doc); sibling-design-spec contribution is small ([session-economy §5.4](../20260514-session-economy/architecture.md) prescribes exactly two SKILL.md additions — INPUTS entry + Phase 8 paragraph). Only **shape (i) §5-enumerated** attribution applies; no narrative-sourced shape (ii) mappings are exercised.
- Carry forward the **section-heading citation discipline** ([N=2 CP-1 advisory c](../20260518-spec-design-skill/journal.md), reinforced at [N=3 CP-1 §7](../20260518-spec-write-skill/journal.md), elevated at [N=4 CP-1](../20260518-spec-execute-skill/journal.md)) and the **per-§5-subsection citation audit at authoring time** (refined at [N=4 amendment 2026-05-18-1](../20260518-spec-execute-skill/journal.md)).

### Non-goals

- **Redesign of the skill.** The shipping SKILL.md is authoritative for behavior. This spec is descriptive.
- **A template for the remaining legacy-quintet retroactive spec** (`spec-amend`). The session is authored at the operator's spec-by-spec cadence; cross-session scaffolding, if any, derives from journal mining, not from declarations in this spec body. The `docs/retroactive-spec-pattern.md` decision is governed by [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) and was *deferred again* at N=5 close (operator-confirmed); to be revisited at N=6 close, not here.
- **Modification of the shipping SKILL.md.** Only the Amendment Protocol via [/spec-amend](../../.agents/skills/spec-amend/SKILL.md) touches SKILL.md.
- **Specifying tooling, models, or platforms.** The skill produces verdict markdown + paired commit only, consistent with the [mission.md Out of Scope](../mission.md) commitment.
- **Resolving the cross-skill amendment coordination question** named at [docs/retroactive-spec-strategy.md OQ-3](../../docs/retroactive-spec-strategy.md) and at [N=3 §12](../20260518-spec-write-skill/architecture.md) / [N=4 §12](../20260518-spec-execute-skill/architecture.md). Resolution belongs to the `spec-amend` retroactive spec at session 5.
- **Specifying when an operator should invoke `/spec-review` vs. work directly without the skill.** Same shape as [N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md) (format-selection question); methodology-wide, not skill-specific.

## 3. Background and Constraints

### Prior state

The `spec-review` skill predates the spec-driven-development trilogy. Its predecessor — the `spec-review-prompt.md` artifact at [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446–645, with companion design notes at lines 647–663 — was authored before the methodology had a constitution, sibling skills, or the session-economy discipline. The skill evolved through four later commits:

- `80000b1` (2026-05-14) — trilogy commit. Lifted the predecessor artifact into [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md). Re-routed Phase 8's amendment-handling: the predecessor's "route them through the Amendment Protocol from the execution prompt" became "route them through the `spec-amend` skill" — the Amendment Protocol moved from inline-in-spec-execute to its own skill. Wired the skill laterally to [spec-amend](../../.agents/skills/spec-amend/SKILL.md), [spec-execute](../../.agents/skills/spec-execute/SKILL.md), [spec-write](../../.agents/skills/spec-write/SKILL.md), and [spec-design](../../.agents/skills/spec-design/SKILL.md).
- `5ce4024` (2026-05-14) — **session-economy commit**. Added `SPEC_REPO_ROOT` to the INPUTS block; added the Phase 8 "Multi-repo case" paragraph. The architectural source for both is [specs/20260514-session-economy/architecture.md §5.4](../20260514-session-economy/architecture.md) — a *sibling design spec*, not a predecessor doc. The same commit touched four sibling skills with the analogous multi-repo additions.
- `c63e3ba` (2026-05-14) — `lastUpdated` frontmatter added across the skill family.
- `4ebec0c` (2026-05-15) — path convention update: `docs/specs/<feature>.md` → `specs/YYYYMMDD-<feature-name>/feature.md`, propagated through `SPEC_PATH` / `JOURNAL_PATH` examples in the six lifecycle skills via the [spec-path-convention](../20260515-spec-path-convention/architecture.md) feature spec.

The predecessor doc is treated in this spec as **authoritative for the skill's design rationale** but not authoritative for current behavior: the shipping SKILL.md is the latter. Where they diverge, SKILL.md wins and the divergence is read by CP-2 as *evolution*, not drift. The Amendment-Protocol routing change (`80000b1`), the session-economy `SPEC_REPO_ROOT` / Phase 8 paragraph additions (`5ce4024`), and the `SPEC_PATH` update (`4ebec0c`) all postdate the predecessor doc and are *evolution* with explicit commits and, where applicable, an authoritative sibling design spec.

The [session-economy design spec](../20260514-session-economy/architecture.md) occupies a distinct role: it is a *sibling design spec* that authoritatively commits to behavior present in the current SKILL.md. Its §5.4 enumerates exactly two SKILL.md additions for `spec-review` — the `SPEC_REPO_ROOT` INPUTS entry and the Phase 8 "Multi-repo case" paragraph. It is cited Authoritative in §14, alongside the SKILL.md itself. This is the **simplest application** of the two-source structure introduced at N=4: only **shape (i) §5-enumerated** attribution applies (retro §5.8 ↔ session-economy §5.4 covers both additions); no **shape (ii) narrative-sourced** mappings are exercised. The N=4 prediction that `spec-review` has a "rich predecessor" ([N=4 journal](../20260518-spec-execute-skill/journal.md)) is confirmed — lines 446–663 of the shared doc are ~218 lines of prompt + design notes, comparable to or slightly larger than spec-execute's ~204-line predecessor. The session-economy contribution is *smaller* than spec-execute's, making this the cleanest two-source application in the quintet so far.

### Constraints (cited)

- **Atomic-Skill Portability Principle** ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)). The skill must be a portable atomic unit: workflow + verdict-format template + reviewer-shapes coverage bundled in its own `SKILL.md`; adapts to richer host-repo embodiments (`SPEC_REPO_ROOT` set, design spec being reviewed instead of feature spec, AI-assisted reviewer) when present; degrades cleanly when absent. A `spec-review` installed against an unrelated host repo with a single-repo feature-spec checkpoint still produces a conformant verdict.
- **AI context window limits** ([specs/tech-stack.md §44](../tech-stack.md#L44)). Verdicts are LLM-consumed across sessions. The fixed Phase 7 verdict format propagates this constraint — verdicts are scannable in any future session by structural recognition, not by re-reading the full review.
- **Spec-driven-development convention** ([specs/tech-stack.md §51](../tech-stack.md#L51)). The methodology repo "eats its own cooking." This convention is the explicit justification for this retroactive spec.
- **Repository layout convention** ([specs/tech-stack.md §48](../tech-stack.md#L48)). `specs/YYYYMMDD-<feature-name>/feature.md` + `journal.md` is the canonical `SPEC_PATH` / `JOURNAL_PATH` shape, set by the [spec-path-convention](../20260515-spec-path-convention/architecture.md) propagation in commit `4ebec0c`.

### Dependencies

- **Upstream.**
  - [spec-write](../../.agents/skills/spec-write/SKILL.md) — authors the feature spec whose Review Checkpoints define the contract this skill reviews against. The checkpoint's `Trigger`, `Review focus`, and `Exit criteria` (per [spec-write SKILL.md](../../.agents/skills/spec-write/SKILL.md) §9 template) are the verbatim inputs Phase 1 reads.
  - [spec-design](../../.agents/skills/spec-design/SKILL.md) — also authors Review Checkpoints (for design specs). The checkpoint shape is identical to feature-spec checkpoints, but the artifact under review is the design spec itself, not a code diff. This adapter case is exercised by every retroactive spec in this series (project-constitution CP-1, N=2 CP-1, N=3 CP-1, N=4 CP-1, N=4 re-review CP-1) and surfaces as §13 OQ-1 below.
  - [project-constitution](../../.agents/skills/project-constitution/SKILL.md) — indirect upstream. Constitutional bindings travel through to the specs `spec-review` reviews.
  - [session-economy architecture spec](../20260514-session-economy/architecture.md) — *companion* design spec authoritative for the `SPEC_REPO_ROOT` INPUTS entry and the Phase 8 "Multi-repo case" paragraph. Not a *predecessor*: it postdates the predecessor doc, and its committed behavior is what currently ships.
- **Downstream.**
  - [spec-execute](../../.agents/skills/spec-execute/SKILL.md) — consumes verdicts emitted by this skill. A `pass` or `pass with comments` verdict lets `spec-execute` resume; `changes requested` stops further task pickup until addressed; `blocked` stops further task pickup until the underlying issue is resolved (typically via amendment).
  - [spec-amend](../../.agents/skills/spec-amend/SKILL.md) — receives the routing when a verdict's "Spec amendments proposed" section has entries. Amendments are *proposed* in the review, *applied* by `spec-amend`.
- **Lateral.** Sibling lifecycle skills reference `spec-review` by name and path; all six skills were modified together in commits `5ce4024` (session-economy / multi-repo) and `4ebec0c` (path convention).

## 4. Architecture

The skill's architecture is an **eight-phase sequential review workflow** that walks a body of work against a specific Review Checkpoint contract, producing a **structured verdict** in a fixed Phase 7 format, with **artifact updates** (spec §9 status line + journal entry) at Phase 8 and, when `SPEC_REPO_ROOT` is set, **a paired commit in the spec repo**, executed as a **portable atomic unit** that integrates with the broader spec-driven-development pipeline.

### Output topology

A single review session produces three durable artifacts:

1. **Verdict** — a markdown block in the fixed Phase 7 format, appended to the journal.
2. **Spec §9 Status line update** — `Status: <outcome> on <date> by <reviewer>` under the relevant checkpoint entry.
3. **Journal entry** — short form (`## YYYY-MM-DD — Review of <CHECKPOINT_ID>` with Reviewer / Outcome / Tasks / Blockers / Amendments / Next action). The Phase 7 verdict block may be embedded in or alongside this entry; either is acceptable per current practice.

When `SPEC_REPO_ROOT` is set, all three artifacts land in the spec repo, paired with the code-side work that was reviewed.

### Vocabulary (defined here, used consistently below)

- **Review Checkpoint** — a named gate declared in the spec's §9 Review Checkpoints section, with `Trigger`, `Review focus`, and `Exit criteria` fields. The checkpoint is the contract; the review verifies the body of work against the contract.
- **Spec compliance finding** — a finding that violates the spec, the Definition of Done, or the checkpoint's exit criteria. Tagged `[blocker]`. Objectively block merge until resolved.
- **Advisory finding** — a finding that does not violate the spec but is worth noting (style preference, refactoring idea, "I would have done it differently"). Tagged `[advisory]`. Surfaced as comments, not blockers.
- **Important finding** — a middle tag between blocker and advisory: not a spec violation but a quality concern serious enough to warrant attention before the next task. Tagged `[important]`. Surfaced as comments; does not block the verdict.
- **Drift finding** — a finding where the implementation does not match the spec, regardless of which side is "right." Tagged `[blocker]` until resolved. Resolution goes through the Amendment Protocol via [/spec-amend](../../.agents/skills/spec-amend/SKILL.md), not silent acceptance.
- **Outcome** — the verdict's overall classification: `pass`, `pass with comments`, `changes requested`, or `blocked`. Stored at the top of the verdict block and in the spec's §9 status line.
- **Reviewer shape** — the operating mode of the reviewer: human, agent, or self-review. Each shape has slightly different ergonomics; the skill is portable across all three.

### Execution model

A reviewer enters the skill with:
- A `SPEC_PATH` and `JOURNAL_PATH` (in the spec repo, which may be `SPEC_REPO_ROOT` if set).
- A `CHECKPOINT_ID` matching an entry in the spec's §9.
- A `DIFF_RANGE` (for code reviews) or the artifact path (for design-spec reviews).
- A list of `TASK_IDS_IN_SCOPE` (from the checkpoint's Trigger).
- A `REVIEWER` name or role.

The reviewer reads the checkpoint contract first (Phase 1), then walks Phases 2–6 producing findings, emits the verdict in Phase 7, and updates artifacts in Phase 8. The verdict is the deliverable; the phases are the means.

### Where this design plugs in

`spec-review` is invoked when a Review Checkpoint declared in a spec is triggered — usually by `spec-execute`'s Phase 7 Checkpoint Gate, sometimes directly by an operator who completed a body of work outside `spec-execute`. The skill is the **only spec-driven-core skill whose primary output is a verdict, not a markdown artifact authored from scratch**. Its outputs feed back to `spec-execute` (whether to proceed) and to `spec-amend` (when amendments are proposed).

## 5. Detailed Design

### 5.1 Phase 1 — Orient

**Purpose.** Read the checkpoint contract verbatim before reading any code. The spec tells the reviewer what to focus on; reading code first biases the review toward whatever happens to be salient in the diff.

**Behavior.** The reviewer reads, in order: (1) the spec's §9 Review Checkpoints entry matching `CHECKPOINT_ID`, noting `trigger` / `review focus` / `exit criteria` verbatim; (2) the spec's §7 Task Breakdown entries for each task in `TASK_IDS_IN_SCOPE`, noting scope / acceptance criteria / DoD; (3) the spec's §6 Non-functional Requirements section, noting items relevant to the tasks; (4) the journal entries for those tasks, noting amendments / decisions / surprises / partial-completion flags; (5) the diff in `DIFF_RANGE`, skimmed for shape and scope only. **The journal is read as the implementer's claim, not as verified evidence; verification is against the diff itself, never the journal narrative alone.** Then the reviewer emits an Orientation Report with quoted checkpoint contract, tasks-in-scope with status, diff shape, journal flags, and initial drift signals.

**Pattern invoked.** "Read the contract before reading the code" — applies the **Spec first, code second** Operating Principle ([SKILL.md OP §1](../../.agents/skills/spec-review/SKILL.md)). Verified against the predecessor [docs/spec-driven-development-prompts-conversation.md line 491](../../docs/spec-driven-development-prompts-conversation.md) ("Read, in order: …") at the date of this spec.

**Why this design.** Self-review is the dominant case in `ai-tools` (the operator is usually the only reviewer). Self-review's natural failure mode is to defend whatever was written rather than test it. Quoting the contract first prevents the reviewer's mental model from drifting into "what the diff is" rather than "what the spec said the diff would be."

**Alternatives considered.** Reading the diff first and the spec second — rejected because it biases the review toward diff-salient items. Reading checkpoint contract + diff simultaneously — rejected because it muddles the contract's role as the prior.

### 5.2 Phase 2 — Scope verification

**Purpose.** Verify the diff stayed within what the spec authorized.

**Behavior.** For each task in scope, the reviewer lists the files declared in the task's Scope field and compares against files actually touched in the diff. Files touched and declared: `[ok]`. Files declared but not touched: `[review]`. Files touched but not declared: `[blocker unless amended]` (either out-of-scope or amended in the journal; the journal amendment is the redemption). Also verifies no new dependencies / frameworks / top-level abstractions are introduced without spec authorization, and that declared test files exist.

**Pattern invoked.** "Scope is the spec's authorization envelope." Aligns with the **No new requirements** Operating Principle ([SKILL.md OP §5](../../.agents/skills/spec-review/SKILL.md)).

**Why this design.** Out-of-scope work is the single most common drift mode in spec-driven development. Catching it here, before semantic review, prevents the spec from being silently expanded.

**Alternatives considered.** Reviewing scope only at the end as a sanity check — rejected because reviewers fatigue and miss scope creep at end-of-pass.

**Design-spec adaptation.** When the artifact under review is a design spec rather than a code diff, "files declared" and "tests present" do not apply directly. Current practice (exercised at every retro-spec CP-1 to date) is to substitute the spec document itself as the artifact and verify that no architectural commitments outside the design spec's §2 Goals were introduced. The mechanics are improvised; the gap is surfaced as §13 OQ-1.

### 5.3 Phase 3 — Review focus walk

**Purpose.** Walk the diff against each item in the checkpoint's `review focus` (quoted from Phase 1). Produce itemized findings.

**Behavior.** For each focus item: the reviewer quotes the item from the spec, names files and line ranges examined, lists findings tagged `[blocker]` / `[important]` / `[advisory]` / `[ok]` (each citing file:line and spec section), and emits a per-focus-item verdict (`pass`, `pass with comments`, or `fail`).

**Pattern invoked.** "The spec's review-focus list is the deep-attention contract." Applies the **Spec first, code second** and **Evidence per finding** Operating Principles ([SKILL.md OP §1-2](../../.agents/skills/spec-review/SKILL.md)).

**Why this design.** Without the focus list, every review becomes a generic code-quality pass. The focus list lets the spec author concentrate reviewer attention on the actual risks of the change. Phase 3 is what makes the review specifically a spec-anchored review.

**Alternatives considered.** Treating review-focus items as optional or aggregated — rejected. The spec author chose each focus item deliberately; aggregation discards that intent.

### 5.4 Phase 4 — Exit criteria verification

**Purpose.** Walk the checkpoint's `exit criteria` one item at a time, producing per-criterion `met` / `not met` verdicts with evidence.

**Behavior.** For each criterion: quote from the spec, name evidence (file path, test output, CI status, doc location, log line, metric name), declare `met` or `not met`. If any criterion is `not met`, the checkpoint fails regardless of other findings.

**Pattern invoked.** "Exit criteria are non-negotiable — that is what makes them exit criteria." Hard-coded gating discipline that prevents review handwaving.

**Why this design.** The exit-criteria check is what gives the checkpoint its name. Without per-criterion `met`/`not met` with evidence, a checkpoint becomes a soft milestone rather than a gate.

**Alternatives considered.** Aggregating exit criteria into a single "checkpoint met" boolean — rejected. Per-criterion verdicts produce the durable evidence trail that future sessions read.

### 5.5 Phase 5 — Generic quality pass

**Purpose.** A lighter-weight pass over items the spec's Non-functional Requirements section declared.

**Behavior.** For each NFR category relevant to tasks in scope, the reviewer verifies: security (input validation, secrets not logged, auth checks), observability (log / metric / trace names match spec conventions), performance (no regressions against declared budgets, unbounded-loop flags, N+1 query flags), reliability (timeouts / retries / idempotency keys where spec required), backward compatibility (public APIs / schemas / config defaults unchanged unless authorized), configuration (new env vars or feature flags documented and defaulted per spec).

**Pattern invoked.** "Generic quality is the spec's NFR list, not the reviewer's preferences." Promotes findings to `[blocker]` only when an NFR was declared and is clearly violated; otherwise `[important]` or `[advisory]`.

**Why this design.** Without anchoring quality findings to the spec's NFR list, every review becomes negotiation about what counts as a quality concern. Anchoring to the spec settles the question.

**Alternatives considered.** A free-form quality pass — rejected because it generates `[blocker]` findings the spec never authorized, which trains implementers to ignore reviewer comments.

### 5.6 Phase 6 — Test and documentation review

**Purpose.** Verify test coverage of declared acceptance criteria, test quality, and required documentation.

**Behavior.** Test coverage: every acceptance criterion from each in-scope task has at least one test, cited by name; missing coverage on a declared criterion is `[blocker]`. Test quality: tests assert behavior not implementation; tests are independent and deterministic; fixtures match the spec's test-data approach. Documentation: inline doc comments on new public interfaces; README / API docs / runbooks / operator docs updated where the spec required (missing required docs are `[blocker]`; nice-to-have docs are `[advisory]`).

**Pattern invoked.** "Tests and docs are spec deliverables, not afterthoughts." Same anchor-to-spec discipline as Phase 5: findings are `[blocker]` only when the spec required the item.

**Why this design.** Tests and documentation are the most common review-fatigue drop-off points. Anchoring to the spec preserves the rigor when the reviewer is tired or rushed.

**Alternatives considered.** Folding tests and documentation into Phase 5 (Generic Quality Pass) — rejected. Test coverage of acceptance criteria is a contract-level check; collapsing it into generic quality dilutes the contract-anchored framing.

**Design-spec adaptation.** When the artifact under review is a design spec, "acceptance criteria → tests" does not apply directly. Current practice (exercised at every retro-spec CP-1 to date) is to verify that the design spec's commitments are traceable to source artifacts (the shipping SKILL.md, the predecessor doc, the sibling design spec) — the "test" of a design spec is its cross-source consistency. Mechanics are improvised; surfaced as §13 OQ-1.

### 5.7 Phase 7 — Verdict

**Purpose.** Emit the structured verdict in a fixed format so future sessions and agents can find the outcome programmatically.

**Behavior.** The reviewer emits a markdown block titled `## Checkpoint <CHECKPOINT_ID> Review Verdict` containing: reviewer / date / diff range / tasks reviewed / outcome (one of `pass` / `pass with comments` / `changes requested` / `blocked`) / blocker count / important count / advisory count / itemized blocker findings (one line each with file:line and spec reference) / important findings / advisory findings / spec amendments proposed (if any) / per-criterion exit-criteria status / one-paragraph recommendation. The format is fixed; the content is variable.

**Outcome rubric** ([SKILL.md Phase 7](../../.agents/skills/spec-review/SKILL.md)):
- **pass** — zero blockers, all exit criteria met.
- **pass with comments** — zero blockers, all exit criteria met, advisories exist.
- **changes requested** — one or more blockers addressable in current scope. Re-review after fixes.
- **blocked** — blocker requires spec amendment, design rework, or external decision. Stops task pickup until resolved.

**Pattern invoked.** "Structured verdict is the durable artifact." The structured shape lets agents parse outcomes mechanically and lets humans skim review history without re-reading every word.

**Why this design.** The distinction between `pass with comments` (let work proceed) and `changes requested` (stop and fix) is the load-bearing semantic distinction in the rubric. Without it, "pass with comments" inflates and reviewer comments lose weight.

**Alternatives considered.** A three-value rubric (pass / fail / blocked) — rejected. The four-value rubric distinguishes "advisory-only pass" from "real fixes needed" from "wait for amendment," and that distinction is the gating signal the next session needs.

### 5.8 Phase 8 — Update artifacts

**Purpose.** Write the verdict back to the spec and journal so subsequent sessions know whether the checkpoint passed. When the spec lives in a different repo, the updates land in `SPEC_REPO_ROOT`, paired with the code-side commit.

**Behavior.** Two updates fire regardless of outcome:

1. **Spec §9 Status line.** Under the relevant checkpoint entry, add `Status: <outcome> on <date> by <reviewer>`. If `pass` or `pass with comments`, the checkpoint is closed. If `changes requested` or `blocked`, the checkpoint stays open.
2. **Journal entry.** Format: `## YYYY-MM-DD — Review of <CHECKPOINT_ID>` with Reviewer / Outcome / Tasks reviewed / Blockers (count + one-line summaries) / Spec amendments proposed (if any) / Next action.

Two conditional updates:

3. **If spec amendments were proposed**, route them through [/spec-amend](../../.agents/skills/spec-amend/SKILL.md). Amendments are not applied silently as part of a review.
4. **If outcome is `pass` or `pass with comments`**, state the next task ID per the dependency graph so the next session has a clear pickup point.

**Multi-repo case.** When `SPEC_REPO_ROOT` is set, both the spec §9 update and the journal entry are committed in `SPEC_REPO_ROOT`, not in the codebase repo. The commit message references the checkpoint ID. This is the paired commit to whatever code-side work was reviewed — do not let one ship without the other.

**Pattern invoked.** "Spec and journal are the durable working memory." Architectural source: [session-economy/architecture.md §5.4](../20260514-session-economy/architecture.md) — the *sibling design spec* that authoritatively commits to both the `SPEC_REPO_ROOT` INPUTS entry and the Phase 8 "Multi-repo case" paragraph. Shape (i) §5-enumerated mapping: retro §5.8 ↔ session-economy §5.4 (the full session-economy contribution to this skill is enumerated in that single §5 subsection). Verified at the date of this spec by reading session-economy §5.4 directly.

**Why this design.** Without writing the verdict back, the review is ephemeral chat. Writing back to spec + journal makes the outcome part of the spec's history; future sessions read the §9 status to know whether to proceed.

**Alternatives considered.** A single combined artifact (the verdict block embedded in the journal entry) — current practice often does this, and the SKILL.md is silent on whether to embed or duplicate. Either is acceptable. The journal-entry format and the verdict-block format overlap intentionally — they are both records of the same review.

### 5.9 Reviewer shapes — human, agent, self-review

**Purpose.** Make the skill portable across three reviewer operating modes.

**Behavior.** The Reviewer Notes section of [SKILL.md](../../.agents/skills/spec-review/SKILL.md) declares three shapes:

- **Human reviewer.** Treats the prompt as a checklist. Fills it in conversationally in a PR description or review tool, as long as every section is addressed.
- **Agent reviewer.** Produces the report as a single markdown document and appends it to the journal. Does not skip phases even when a section appears trivially satisfied; the absence of findings in a phase is itself a recorded outcome.
- **Self-review (reviewer is also implementer).** Runs the full prompt anyway. Self-review with a structured checklist catches more than self-review without one. Reviewer is explicitly counseled to be honest about advisory findings — easier to dismiss when reviewing one's own work.

**Pattern invoked.** "Portability includes reviewer-mode portability." The same eight-phase structure applies to all three shapes; only ergonomics differ.

**Why this design.** Self-review is the dominant case in `ai-tools` (single operator, no peer reviewer). Without explicit self-review counseling, the natural bias is to wave through one's own work. The Phase 1 "quote the contract first" and the Reviewer Notes "be especially honest about advisory findings" together address this.

**Alternatives considered.** A separate `self-review` skill — rejected. The phases are identical; only the bias is different, and bias is addressed in counseling, not in a separate skill.

### 5.10 Voice discipline

**Purpose.** Match the prose voice to the kind of statement being made.

**Behavior.** Imperative for review rules ("the reviewer must…", "do not begin reviewing the diff before reading the checkpoint definition"). First-person plural for design intent ("we chose…"). Plain declarative for observations. No marketing language ("elegant," "robust," "scalable").

**Pattern invoked.** Voice discipline was declared at [N=2 §5.6](../20260518-spec-design-skill/architecture.md), omitted at [N=3](../20260518-spec-write-skill/architecture.md) (whose §5 covered phases + upstream-spec orientation + atomicity + test strategy + citation discipline + section template, with no Voice-discipline subsection), and reintroduced at [N=4 §5.10](../20260518-spec-execute-skill/architecture.md). Carried forward at N=5 as a spec-side discipline. The gap at N=3 is surfaced explicitly rather than papered over as continuous lineage.

**Why this design.** The voice signals what kind of statement the reader is parsing — rule, intent, or observation. Mixed voice produces ambiguous prose that LLM readers misinterpret.

**Alternatives considered.** Permitting any voice — rejected; introduces ambiguity. Single-voice everywhere — rejected; loses the rule/intent/observation distinction.

### 5.11 Portability rule for links

**Purpose.** Keep committed prose interpretable on any clone of the repo, regardless of the author's filesystem layout.

**Behavior.** Committed prose contains no absolute filesystem paths and no machine-specific paths. Order of preference for links: published URL → repo-relative path → sibling-relative description → bare name + host description. No `~/.claude/skills/...` references; the canonical path is [.agents/skills/...](../../.agents/skills/spec-review/SKILL.md).

**Pattern invoked.** [project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md) (drop `~/.claude/skills/` references). Carried forward at every N≥2.

**Why this design.** A spec is read on machines other than the author's. Absolute paths break for every other reader. The `.agents/skills/` path is the authoritative location in this repo; cloned/forked copies inherit it.

**Alternatives considered.** Permitting absolute paths in author-private prose — rejected. Spec is committed; nothing is "author-private."

## 6. Non-functional Requirements

| NFR | Requirement | Source |
|---|---|---|
| **Adoptability (ASPP)** | The skill must run portably against any host repo whose specs follow the methodology's §9 Review Checkpoint shape. Degrades to a feature-spec review without `SPEC_REPO_ROOT` (single-repo case) and without sibling skills (verdict still emits; spec-amend routing becomes a manual operator note rather than a skill invocation). | [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) |
| **Context economy** | Verdict format is fixed and bounded; future sessions scan §9 status lines and journal "Outcome:" fields without re-reading the full review. | [specs/tech-stack.md §44](../tech-stack.md#L44) |
| **Reviewer-shape portability** | The skill must work for human reviewers (checklist), agent reviewers (full structured report), and self-reviewers (same with bias counseling). Reviewer Notes section addresses all three. | [SKILL.md REVIEWER NOTES](../../.agents/skills/spec-review/SKILL.md) |
| **Drift surfacing** | Drift findings are first-class — when the implementation does not match the spec, the review records that regardless of which side is "right." Resolution routes to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md). | [SKILL.md OP §4](../../.agents/skills/spec-review/SKILL.md) |
| **Blocker vs. advisory discipline** | A finding is `[blocker]` only if it violates the spec, the DoD, or an exit criterion. Style preferences, refactoring ideas, "I would have done it differently" are `[advisory]`. The distinction is hard, not soft. | [SKILL.md OP §3](../../.agents/skills/spec-review/SKILL.md) |
| **Multi-repo discipline** | When `SPEC_REPO_ROOT` is set, spec §9 status updates and journal entries are committed in `SPEC_REPO_ROOT`, paired with the code-side commit being reviewed. Architectural source: [session-economy §5.4](../20260514-session-economy/architecture.md). Shape (i) §5-enumerated attribution: retro §5.8 ↔ session-economy §5.4. | [SKILL.md INPUTS + Phase 8](../../.agents/skills/spec-review/SKILL.md); [session-economy §5.4](../20260514-session-economy/architecture.md) |
| **Verdict observability** | Outcomes are recorded with predictable structural markers (`Status:` line in spec §9; `**Outcome:**` line in journal verdict block). Future sessions and agents parse without ambiguity. | [SKILL.md Phase 8](../../.agents/skills/spec-review/SKILL.md) |
| **No new requirements during review** | If the reviewer wants something the spec did not require, the channel is "propose a spec amendment for next iteration," not "block this checkpoint." This is what lets specs survive multiple reviewers without becoming unbounded. | [SKILL.md OP §5](../../.agents/skills/spec-review/SKILL.md) |

## 7. Implementation Sequencing

The skill is already implemented. This spec retroactively documents what ships. Sequencing for the spec itself:

1. **Author this `architecture.md` + paired `journal.md`** (this session). Closes the authoring phase.
2. **CP-1 review** in a fresh session. Closes the faithfulness phase.
3. **CP-2 drift audit** in the batched quintet-CP-2 session per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). Closes the drift-baseline phase.

After step 3, the spec moves out of `Draft — Open for Review` to its post-CP-2 status (per N=1 / N=2 / N=3 / N=4 convention, the banner is amended at that point).

## 8. Validation Approach

| Approach | What it validates |
|---|---|
| **Stakeholder review** | Eric (operator) reviews this spec for fidelity to intent. CP-1 is the gate. |
| **Drift audit** | Mechanical comparison of SKILL.md commitments to this spec's commitments. CP-2 is the gate. Output is a divergence list (possibly empty). Batched with the four sibling quintet specs. |
| **Predecessor cross-check** | [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446–663 is the skill's design-rationale source (prompt artifact + design notes block). Every spec-side commitment in §5 of this document traces to either a behavior in SKILL.md (current) or to a recommendation in the predecessor doc (rationale). Gaps between predecessor and SKILL.md are *evolution* (trilogy commit `80000b1`, session-economy commit `5ce4024`, path-convention commit `4ebec0c`), not drift. CP-2 reads this distinction. |
| **Sibling design-spec cross-check** | [specs/20260514-session-economy/architecture.md §5.4](../20260514-session-economy/architecture.md) is the *sibling* design spec authoritative for current behavior added after the predecessor. Only **shape (i) §5-enumerated** attribution applies (no narrative-sourced shape (ii) is exercised in this skill — the simplest application of the two-source structure encountered in the quintet so far). Mapping: retro §5.8 (Phase 8 Update Artifacts, including the multi-repo case paragraph) and the `SPEC_REPO_ROOT` INPUTS contract both trace to session-economy §5.4. CP-2 reads cross-spec consistency under shape (i). |
| **Downstream consumption** | The skill has been used to review the retroactive design specs at [specs/20260517-project-constitution-skill/](../20260517-project-constitution-skill/) (CP-1 pass with comments), [specs/20260518-spec-design-skill/](../20260518-spec-design-skill/) (CP-1 pass with comments), [specs/20260518-spec-write-skill/](../20260518-spec-write-skill/) (CP-1 pass with comments), and [specs/20260518-spec-execute-skill/](../20260518-spec-execute-skill/) (CP-1 changes-requested → amended → re-review pass with comments). Five reviews against the eight-phase workflow; verdicts in the expected structured format; spec §9 status lines updated; journal entries appended. The five reviews are evidence the workflow produces durable, reviewable, future-readable outcomes. |

> Note: This section deliberately differs from a feature spec's Test Strategy. Design specs are validated by review, audit, predecessor cross-check, sibling-spec cross-check, and downstream consumption — not by automated test coverage.

## 9. Review Checkpoints

### CP-1 — Retroactive spec faithfully describes the shipping skill

**Status:** pass with comments on 2026-05-18 by Claude (AI assistant) on behalf of Eric Wasgatt — one [important] citation error in §5.10 (Voice-discipline lineage cites nonexistent N=3 §5.X) proposed for amendment via `/spec-amend`; see [journal](./journal.md) entry of same date. Verdict commit: `fcb5094`.

**Trigger.** This spec and its journal are committed; the operator invokes `/spec-review` against this spec's CP-1 in a fresh session.

**Review focus.**
- Every commitment in §4, §5, and §6 corresponds to behavior actually present in [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- No commitment in this spec contradicts the shipping SKILL.md.
- The **Atomic-Skill Portability Principle** is correctly characterized as a binding constraint (§3, §6) including degradation behavior when optional inputs are absent and when the artifact under review is a design spec rather than a feature spec.
- The **predecessor doc** is correctly distinguished as authoritative-for-design-rationale-not-current-behavior (§3 Background, §14 Inspirational). Line range (lines 446–663) and the artifact+design-notes split are accurate.
- The **session-economy spec** is correctly distinguished as a *sibling design spec* authoritative for current behavior added after the predecessor — cited Authoritative in §14. Only **shape (i) §5-enumerated** attribution applies: retro §5.8 (Phase 8 Update Artifacts plus the `SPEC_REPO_ROOT` INPUTS entry) maps to session-economy §5.4. No shape (ii) narrative-sourced mappings are exercised in this skill; the spec does not claim any.
- The eight phases in §5 (§5.1–§5.8) match the shipping SKILL.md's phase structure. §5.9 Reviewer shapes accurately reflects the three modes named in [SKILL.md REVIEWER NOTES](../../.agents/skills/spec-review/SKILL.md).
- §13 OQ-1 (design-spec checkpoint mechanics gap) and §13 OQ-2 (amendment-then-re-review cycle artifact tracking) are named with full Question / Analysis / Leaning / Owner / Watch items / Anti-goals structure.
- The spec is self-contained per the Operating Principles in [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- Section-heading citations point to the heading line, not the body's first line (Pattern-for-N=5 #3 carried forward from N=4).
- The portability rule for links is honored: no `~/.claude/skills/...` references, no absolute filesystem paths.

**Exit criteria.**
- Reviewer issues a verdict of `pass`, `pass with comments`, or `changes requested` per the structured format declared in [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- All `[blocker]` findings (if any) are resolved or escalated to `/spec-amend`.
- Verdict is written back to this spec's §9 (status line) and to the journal.

### CP-2 — Drift audit complete (batched)

**Status:** pass with comments on 2026-05-18 by Claude (AI assistant) on behalf of Eric Wasgatt — five divergences (D-1 through D-5) all advisory: D-1 (b) amend SKILL.md Design Notes stale-citation; D-2 (b) amend SKILL.md to define `[important]` tag; D-3 (a) amend spec §5.1 to home WND-5 rubber-stamp prohibition; D-4 (b) amend SKILL.md preamble pairing list; D-5 (c) accept as known minor (already routed via §13 OQ-1). See [journal](./journal.md) "Review of CP-2" entry and [batch journal N=5 entry](../20260518-cp2-batch-audit/journal.md) of same date. All four routed amendments landed via [2026-05-18-4](./journal.md) (D-3 → §5.1), [2026-05-18-5](./journal.md) (D-1 → SKILL.md Design Notes), [2026-05-18-6](./journal.md) (D-2 → SKILL.md OP §3), and [2026-05-18-7](./journal.md) (D-4 → SKILL.md preamble); D-5 accepted unchanged per route (c). Brief re-verification on 2026-05-18 confirms the (a)-route divergence closed in spec body and the three (b)-route divergences closed in SKILL.md — see [CP-2 re-verification journal entry](./journal.md). Banner held at `Draft — Open for Review` per N=1/N=2/N=3/N=4 precedent (no defined successor state). **Checkpoint closed.**

**Trigger.** CP-1 of this spec passes, AND CP-1 of the remaining quintet spec (`spec-amend`) passes, AND CP-1 of [spec-design](../20260518-spec-design-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND CP-1 of [spec-write](../20260518-spec-write-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND CP-1 of [spec-execute](../20260518-spec-execute-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18, after amendment 2026-05-18-1 commit `d11c405`), AND project-constitution's CP-2 has either run or been folded into the batch per [docs/retroactive-spec-strategy.md OQ-1](../../docs/retroactive-spec-strategy.md). The narrowed remaining condition is "one sibling quintet CP-1 (`spec-amend`) + project-constitution CP-2."

**Review focus.** A line-by-line audit of [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md) against this spec's §4, §5, §6, and §12. The auditor enumerates each divergence: a behavior present in SKILL.md but not committed in the spec, or a commitment in the spec absent from SKILL.md. Cross-skill drift patterns (e.g., four quintet specs citing the Atomic-Skill Portability Principle correctly and one quietly not; or session-economy commitments inconsistently propagated across the quintet) are explicitly in scope by virtue of the batch context. The cross-spec consistency check between this spec and the [session-economy spec](../20260514-session-economy/architecture.md) is a CP-2 line item under **shape (i) §5-enumerated** only: retro §5.8 plus the `SPEC_REPO_ROOT` INPUTS contract map to session-economy §5.4. CP-2 verifies this mapping; no shape (ii) check is needed because no shape (ii) mappings are claimed.

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
| The two-source structure causes CP-2 false positives — auditor reads behavior present in SKILL.md but absent from the predecessor as drift, missing that the session-economy spec is the authoritative source for `SPEC_REPO_ROOT` and the Phase 8 multi-repo paragraph. | Medium | Medium | §3 Background and §14 References explicitly distinguish the two sources; §8 Validation Approach declares both cross-check rows; CP-1 review focus includes the session-economy-spec consistency check; CP-2 reviewer reads §3 / §8 / §14 before walking divergences. Same mitigation pattern as N=4. | CP-2 auditor |
| §13 OQ-1 (design-spec checkpoint mechanics) resolves silently as improvisation in each review — operator and reviewer do the right thing without recording the pattern, and the SKILL.md gap stays unwritten forever. | High (already six instances of improvisation) | Low–Medium — improvisation has worked but is undocumented | OQ-1's Watch items capture revisit conditions; the Amendment Protocol creates a route when the gap surfaces as friction; CP-2 reviewer may flag if the OQ is still open after >N more retro-spec reviews. | Eric (per-session); future amendment session |
| §13 OQ-2 (amendment-then-re-review cycle artifact tracking) ossifies as theoretical because future re-review cycles inherit the N=4 improvised precedent without questioning it. | Medium | Low–Medium | OQ-2's Watch items name the next re-review cycle (whether at CP-1 of this spec, CP-1 of `spec-amend`, or elsewhere) as the revisit trigger; until then, the OQ is documented but inactive. | Eric; first /spec-review→/spec-amend→/spec-review session after this spec |
| N=5 patterns over-codify, foreclosing better N=6 patterns at the sibling `spec-amend` spec. | Low | Medium | §2 Non-goals explicitly disclaims template-establishing intent; this spec's journal records "Pattern for N=6" callouts that are *candidates*, not declarations; the `docs/retroactive-spec-pattern.md` decision is deferred again to N=6 close. | Future retroactive-spec session; operator at N=6 close |
| The predecessor doc is treated as authoritative for current behavior, causing CP-2 to flag every recommendation-that-evolved as drift. | Medium | Low–Medium | §3 Background distinguishes the two; §8 Validation Approach names the three evolution-explaining commits (`80000b1`, `5ce4024`, `4ebec0c`); CP-2 auditor reads §3 before walking divergences. Same mitigation pattern as N=3 / N=4. | CP-2 auditor |
| Self-review bias — Eric, as both author and reviewer of this spec's CP-1, waves through advisory findings that warrant attention. | Medium | Low | The shipping SKILL.md's REVIEWER NOTES section explicitly counsels self-review honesty about advisories; CP-1 reviewer (even if Eric or an AI assistant on his behalf) runs the full prompt and emits the structured verdict format, which surfaces advisories independently of any handwaved global verdict. | CP-1 reviewer |

## 11. Adoption Path

The spec is adopted in three steps, matching N=1 / N=2 / N=3 / N=4:

1. **Commit the spec and journal** as a paired commit. The journal is the durable record of this session's decisions and observations, structured to be mined by session 5 (`spec-amend`) of the legacy quintet.
2. **CP-1 review** in a fresh session (sequenced, per operator's choice): invoke `/spec-review` against this spec's CP-1.
3. **CP-2 drift audit** in the batched quintet-CP-2 session: per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md).

After adoption, the spec is a *living contract*. Edits to SKILL.md follow the Amendment Protocol: stated section being amended, before/after diff, reason and impact, explicit operator approval, journal entry. Per the [N=1 amendment 2026-05-17-1 N=2 mining note](../20260517-project-constitution-skill/journal.md), amendments that touch a *class* of references (paths, citations, vocabulary) must scan the entire spec at Phase 1 Orient, not just the locations called out by the triggering finding.

### Reversibility

The spec can be retired without affecting the skill. The SKILL.md remains the canonical implementation. Retirement would be unusual (it would be the methodology rejecting its own dogfooding convention for `spec-review` specifically) but mechanically clean: delete the spec directory and record the retirement in `roadmap.md`.

### Cross-session knowledge transfer

This spec's [journal.md](./journal.md) records validation/refinement/rejection outcomes for each "Pattern for N=5" callout from the [N=4 journal](../20260518-spec-execute-skill/journal.md) (including the refined #1 audit shape concretized in amendment 2026-05-18-1 and the new re-review-cycle observation), plus new "Pattern for N=6" callouts for session 5. The session 5 mining input also includes this session's confirmation that the N=4 prediction "spec-review has a rich predecessor" holds and that the sibling-design-spec contribution to spec-review is **smaller** than spec-execute's (one §5-enumerated mapping only). Session 5 mines this journal as primary input.

## 12. Out of Scope

- **Resolving §13 OQ-1 (design-spec checkpoint mechanics gap).** The OQ is named; resolution routes to a future amendment session when watch conditions trigger.
- **Resolving §13 OQ-2 (amendment-then-re-review cycle artifact tracking).** The OQ is named; resolution requires the next re-review cycle (this spec's CP-1 may be that next cycle if it produces blockers).
- **Cross-skill amendment coordination** when a review surfaces drift that requires coordinated edits across multiple skills' SKILL.md files. Named at [docs/retroactive-spec-strategy.md OQ-3](../../docs/retroactive-spec-strategy.md), at [N=3 §12](../20260518-spec-write-skill/architecture.md), and at [N=4 §12](../20260518-spec-execute-skill/architecture.md). Resolution lives in the `spec-amend` retroactive spec at session 5.
- **Multi-repo `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` mechanics under multi-target conditions** (one feature spec spanning multiple downstream code repos; spec-repo lifecycle reflected in adopter repos). The single-repo case is the common case in `ai-tools`; the multi-repo two-repo case is the architecturally-committed configuration. Multi-*target* (>2 repos) mechanics are not bounded by this spec. Mirrors N=2 / N=3 / N=4's identical disposition.
- **Resolving the format-question-prompt gap** ([N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md)). Methodology-wide; not re-raised here.
- **The `docs/retroactive-spec-pattern.md` decision.** Deferred at N=3 close, considered again at N=5 close (this session), **deferred again to N=6 close** (operator-confirmed at Phase 2 of this session). The N=4 introduction of the two-source structure plus the N=5 application of it in its simplest shape are evidence inputs to the eventual N=6 decision; this spec's journal records the inputs but does not pre-empt the decision.
- **Redesign of the `spec-review` skill.** Redesign routes to a new design spec under amendment governance.
- **Modification of the shipping SKILL.md.** Only `/spec-amend` touches SKILL.md.
- **A template for the remaining legacy-quintet retroactive spec** (`spec-amend`). Cross-session scaffolding, if any, derives from journal mining; this spec body does not declare a template.
- **External-claim verification beyond repo-internal citation.** Light verification was adopted for this spec; the heavy-verification path exists in the methodology but is not exercised by this spec's text.
- **Constitution-amendment ceremony.** Inherited from [N=1 OQ-1](../20260517-project-constitution-skill/architecture.md) and tracked at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md). Out of scope here.
- **Reviewer attribution convention under AI assistance** (e.g., "Claude (AI assistant) on behalf of Eric Wasgatt"). A convention has emerged organically across the retro-spec CP-1s but is not declared in SKILL.md's `REVIEWER: <name or role>` input description. Methodology-wide convention question, not unique to spec-review; deferred to a future amendment session if it surfaces friction. Disposition matches N=2's format-question-prompt item.
- **Cross-spec citation review as a Phase 3 / Phase 5 explicit check.** The N=4 CP-1 review focus declared per-§5-subsection citation verification against the session-economy spec, but SKILL.md's Phase 3 and Phase 5 do not name "cross-spec citation review" as a focus item. The retro specs themselves now declare the check in their CP-1 review focus (this spec included), so the gap is patched per-spec. SKILL.md is silent; the gap is documented but not actionable from inside SKILL.md.

## 13. Open Questions

### OQ-1 — Design-spec checkpoint mechanics gap

**Question.** When the artifact under review is a design spec rather than a code diff, several phases do not apply directly: Phase 2 (Scope Verification) talks about "files declared in the task's Scope field" — design specs have no atomic tasks; Phase 6 (Test and Documentation Review) talks about "every acceptance criterion from each task" — same issue. The SKILL.md description names design-spec checkpoints ("the diff (or the artifact, for design-spec checkpoints)") but the phase mechanics inside assume feature-spec shape. Reviewers improvise the design-spec adaptation each time. Is the improvisation acceptable as a stable convention, or should the phases name design-spec-specific behavior explicitly?

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) Document the improvised conventions in SKILL.md as alternative phase behaviors when the artifact is a design spec | Phase 2 alternative: "When reviewing a design spec, substitute architectural-commitment scope (§2 Goals + §4 Architecture vs. introduced commitments) for file-level scope." Phase 6 alternative: "When reviewing a design spec, substitute cross-source consistency (predecessor, sibling design spec, SKILL.md) for acceptance-criteria coverage." | Closes the gap; codifies what has been improvised six times. Cost: lengthens SKILL.md; mechanics may not generalize cleanly. |
| (b) Document the conventions in a separate skill or sub-skill (`spec-review-design`) | A sibling skill for design-spec checkpoints with the same Phase 7 verdict format but adapted Phases 2/6. | Cleanest separation. Cost: two skills to maintain; risk of divergence; violates the Atomic-Skill Portability Principle's "self-contained workflow" if reviewers must choose. |
| (c) Accept the improvisation as stable convention; surface only in spec body where the gap appears | Each retro-spec's §5 Phase-2/Phase-6 subsections call out the design-spec adaptation explicitly (as this spec does in §5.2 and §5.6). SKILL.md silent. | Zero infrastructure. Cost: convention exists only across specs that mention it; new reviewers may not realize the adaptation exists. Mirrors current behavior. |
| (d) Mark the SKILL.md description's "(or the artifact, for design-spec checkpoints)" as undocumented and route to amendment when the gap creates friction | A targeted SKILL.md note inviting future amendment when phase-mechanics friction is observed. | Honest about the gap. Cost: doesn't help current reviewers; delays formalization. |

This OQ has no structural relationship to §13 OQ-2 (amendment-then-re-review cycle). Cross-reference noted but not load-bearing.

**Leaning.** **(c) is the current de-facto resolution**, with each retro spec's §5 making the adaptation explicit at the affected phase subsections (§5.2 and §5.6 in this spec). The improvisation has worked six times (N=1 / N=2 / N=3 / N=4 / N=4 re-review CP-1s + project-constitution CP-1). **Recommended direction: (c) as the convention, with (a) as the formalization path when a >2-line SKILL.md adaptation is warranted.** No formal commitment until either watch condition triggers.

**Owner.** A future `/spec-amend` session triggered by either of the watch items below.

**Watch items.**
- A future retro-spec CP-1 reviewer encounters friction with the design-spec adaptation — improvises in a way inconsistent with prior reviews. Signal: divergent §5.2 / §5.6 adaptations across the retro-spec series.
- A non-retroactive design-spec CP-1 is run (e.g., a fresh design spec authored by `/spec-design` and reviewed before any feature spec exists). Signal: the improvised convention faces its first non-retroactive test, and either holds or does not.

**Anti-goals.**
- **Do not** add a design-spec-specific phase to SKILL.md without first checking whether the existing phases adapt cleanly. Premature phase-addition bloats SKILL.md.
- **Do not** require Phase 2 / Phase 6 to behave identically for design specs and feature specs. The artifacts genuinely differ; uniform mechanics would force false equivalence.

### OQ-2 — Amendment-then-re-review cycle artifact tracking

**Question.** When a `/spec-review` produces a `changes requested` verdict whose blockers are addressed via `/spec-amend` and the checkpoint is re-reviewed, several mechanics are unspecified in SKILL.md: (a) does the re-review reuse the original `CHECKPOINT_ID` or get a new one (e.g., `CP-1-retry`); (b) is the re-review verdict appended as a separate journal entry or folded into the original; (c) what does the spec's §9 Status line say during the "amendment pending" interregnum between original verdict and re-review; (d) does the re-review's `DIFF_RANGE` include only the amendment commits or the original diff plus the amendment? N=4 improvised reasonable answers but the pattern is not documented.

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) Document the N=4-improvised pattern as SKILL.md convention | Re-review reuses `CHECKPOINT_ID`; separate journal entry titled `## YYYY-MM-DD — Re-review of <CHECKPOINT_ID>`; spec §9 Status line shows the latest verdict (overwriting prior "changes requested" with "pass with comments"); re-review `DIFF_RANGE` covers the amendment commits layered on the original commits. | Closes the gap; codifies a working pattern. Cost: requires SKILL.md amendment; N=4's improvisation may not generalize to other amendment shapes. |
| (b) Add explicit re-review mechanics as a new Phase 9 or sub-phase 8.5 | An "Amendment-then-re-review" phase declaring the artifact-tracking conventions. | Most structured. Cost: lengthens SKILL.md noticeably; risk of over-engineering a path that fires rarely. |
| (c) Accept the improvisation as stable convention; document in journal notes only | Each /spec-review session that needs re-review consults prior re-review journals for convention. SKILL.md silent. | Zero infrastructure. Cost: convention is journal-mined, not declared; new sessions may diverge. Mirrors current behavior. |
| (d) Leave the gap unresolved; revisit when a second re-review cycle occurs | First-instance evidence is N=4 only. Two cycles produce a pattern; one is an anecdote. | Honest about evidence; no premature formalization. Cost: the gap stays unwritten longer. |

This OQ has a weak structural relationship to §13 OQ-1 (design-spec checkpoint mechanics): both surface as "SKILL.md is silent on a case that has worked through improvisation." Cross-reference noted; mitigation patterns may overlap.

**Leaning.** **(d) is the most honest current position.** One re-review cycle is insufficient evidence to codify mechanics. If a second cycle occurs (likely at CP-1 of this spec or at CP-1 of `spec-amend` if either generates blockers), the pattern can be observed and codified at (a). **Recommended direction: (d) until the watch condition triggers, then (a).**

**Owner.** A future `/spec-amend` session triggered by the watch item below.

**Watch items.**
- A second re-review cycle occurs in the retro-spec series (CP-1 of this spec produces blockers, or CP-1 of `spec-amend` does). Signal: two cycles' evidence lets (a) be codified.
- A re-review cycle occurs in a non-retroactive context (e.g., a feature-spec CP-2). Signal: the pattern faces its first non-retroactive test.

**Anti-goals.**
- **Do not** introduce a new `CHECKPOINT_ID` shape (e.g., `CP-1-retry`) without first observing whether reuse causes ambiguity in practice. N=4's reuse worked; the convention may be sufficient.
- **Do not** require re-review verdicts to overwrite original verdicts in the journal. The original verdict is the durable evidence that the amendment was necessary; both should remain visible.

## 14. References

### Authoritative

- [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md) — the shipping skill. Authoritative for behavior. Verified at the date of this spec.
- [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) — Atomic-Skill Portability Principle. Authoritative for ASPP. Verified at the date of this spec.
- [specs/tech-stack.md §44](../tech-stack.md#L44) — AI context window limits. Authoritative for context-economy constraints.
- [specs/tech-stack.md §48](../tech-stack.md#L48) — Repository layout convention. Authoritative for `SPEC_PATH` / `JOURNAL_PATH` shape.
- [specs/tech-stack.md §51](../tech-stack.md#L51) — Spec-driven-development convention. Authoritative for the methodology-eats-its-own-cooking justification.
- [specs/mission.md](../mission.md) — repo mission, including the Out of Scope items (tooling-agnostic; no domain variants in core).
- [specs/20260514-session-economy/architecture.md §5.4](../20260514-session-economy/architecture.md) — sibling design spec authoritative for current behavior added after the predecessor (the `SPEC_REPO_ROOT` INPUTS entry and the Phase 8 "Multi-repo case" paragraph). Shape (i) §5-enumerated attribution: retro §5.8 + INPUTS contract ↔ session-economy §5.4. Verified at the date of this spec.

### Inspirational

- [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446–663 — predecessor of this skill: `spec-review-prompt.md` artifact (lines 446–645) + "Design notes on the review prompt" (lines 647–663). Authoritative for design rationale; not authoritative for current behavior.
- [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) — operating principles for the retroactive-spec series. Read as orientation; not used as a source for §4/§5 architectural commitments.
- [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) and [journal.md](../20260517-project-constitution-skill/journal.md) — N=1 retroactive-spec source.
- [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) and [journal.md](../20260518-spec-design-skill/journal.md) — N=2 retroactive-spec source.
- [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) and [journal.md](../20260518-spec-write-skill/journal.md) — N=3 retroactive-spec source.
- [specs/20260518-spec-execute-skill/architecture.md](../20260518-spec-execute-skill/architecture.md) and [journal.md](../20260518-spec-execute-skill/journal.md) — N=4 retroactive-spec source; closest-sibling structural source; origin of the two-source structure applied here.
