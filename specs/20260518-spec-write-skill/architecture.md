# `spec-write` Skill — Architecture and Protocol Specification

> Status: Approved — CP-2 closed 2026-05-18
> Date: 2026-05-18
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set.

## 1. Overview

The `spec-write` skill authors **feature specs**: self-contained development plans for a new feature in an existing codebase, committing to atomic tasks, tests, review checkpoints, and rollout. It sits downstream of [project-constitution](../../.agents/skills/project-constitution/SKILL.md) (which establishes scope) and [spec-design](../../.agents/skills/spec-design/SKILL.md) (which establishes architectural commitments), and upstream of [spec-execute](../../.agents/skills/spec-execute/SKILL.md) (which executes the tasks) and [spec-review](../../.agents/skills/spec-review/SKILL.md) (which reviews at declared checkpoints). Its output — a `feature.md` paired with a `journal.md` — is the contract under which downstream skills operate.

This document is a **retroactive design specification**: the skill already ships at [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md), which is authoritative for the skill's behavior. The spec describes what the skill *is* and what it *commits to*, so the methodology can be reviewed, amended, and adopted on the same footing as the artifacts it produces. The spec does not redesign the skill; any divergence the spec exposes between its commitments and the shipping SKILL.md is routed to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md), never silently corrected.

## 2. Goals and Non-goals

### Goals

- Produce a self-contained, descriptive specification of the `spec-write` skill's vocabulary, contract, phases, and operating model.
- Declare review gates (§9) so the skill becomes reviewable via [/spec-review](../../.agents/skills/spec-review/SKILL.md) against named checkpoints.
- Hold the skill to the **Atomic-Skill Portability Principle** declared in the methodology's constitution ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)) — including the spec's behavior when `DESIGN_SPEC_PATH` or `CONSTITUTION_PATHS` is absent.
- Continue the **paired-artifact pattern** for retroactive skill specs — `architecture.md` + `journal.md` — established at N=1 ([specs/20260517-project-constitution-skill/](../20260517-project-constitution-skill/)) and refined at N=2 ([specs/20260518-spec-design-skill/](../20260518-spec-design-skill/)). This N=3 instance validates, refines, or rejects each of the N=2 journal's "Pattern for N=3" callouts.
- Distinguish the **predecessor artifact** ([docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225) as authoritative for design rationale, not authoritative for current behavior — per the Pattern-for-N=3 callout raised in the N=2 journal.

### Non-goals

- **Redesign of the skill.** The shipping SKILL.md is authoritative for behavior. This spec is descriptive.
- **A template for the three remaining legacy-quintet retroactive specs** (`spec-execute`, `spec-review`, `spec-amend`). Each is authored in its own session per the operator's spec-by-spec cadence; cross-session scaffolding, if any, derives from journal mining at N≥3, not from declarations in this spec body. The N=2-inflection-point decision on `docs/retroactive-spec-pattern.md` is recorded in this session's [journal.md](./journal.md), not here.
- **Modification of the shipping SKILL.md.** Only the Amendment Protocol via [/spec-amend](../../.agents/skills/spec-amend/SKILL.md) touches SKILL.md.
- **Specifying tooling, models, or platforms.** The skill produces markdown only and remains tooling-agnostic, consistent with the [mission.md Out of Scope](../mission.md) commitment.
- **Specifying when an operator should choose `spec-write` over `spec-design`.** That format-selection question is named at first-class detail in [N=2's §13 OQ-1](../20260518-spec-design-skill/architecture.md#oq-1--format-question-prompt-is-undocumented-in-skillmd) and routes to a future amendment or meta-entrypoint session. It is methodology-wide, not skill-specific to `spec-write`.

## 3. Background and Constraints

### Prior state

The `spec-write` skill predates the spec-driven-development trilogy. Its predecessor — the `spec-writing-prompt.md` artifact at [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225, with companion design notes at lines 207–225 — was authored before the methodology had a constitution, sibling skills, or a path convention. The skill evolved through three later commits:

- `80000b1` (2026-05-14) — trilogy commit added `DESIGN_SPEC_PATH` and `CONSTITUTION_PATHS` INPUTS plus their Phase-1 orientation behavior, integrating `spec-write` with `spec-design` (upstream) and `project-constitution` (upstream).
- `5ce4024` (2026-05-14) — session-economy and multi-repo disciplines added across the skill family (`SPEC_REPO_ROOT`/`SPEC_TARGET_BRANCH` mention in OUTPUT FORMAT).
- `4ebec0c` (2026-05-15) — path convention update: `docs/specs/<feature>.md` → `specs/YYYYMMDD-<feature-name>/feature.md` + `journal.md` companion, applied uniformly across the six lifecycle skills via the [spec-path-convention](../20260515-spec-path-convention/architecture.md) feature spec.

The predecessor doc is treated in this spec as **authoritative for the skill's design rationale** but not authoritative for current behavior: the shipping SKILL.md is the latter. Where they diverge, SKILL.md wins and the divergence is a candidate finding for CP-2. The richer Phase-1 behavior (upstream-spec orientation), the journal pairing, and the multi-repo fields all postdate the predecessor doc and are *evolution*, not drift.

### Constraints (cited)

- **Atomic-Skill Portability Principle** ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)). The skill must be a portable atomic unit: workflow + 14-section template + per-task contract bundled in its own `SKILL.md`; adapts to richer host-repo embodiments (constitution at `CONSTITUTION_PATHS`, design spec at `DESIGN_SPEC_PATH`, sibling skills) when present; degrades cleanly when absent. A `spec-write` installed against an unrelated host repo with no upstream artifacts still produces a conformant feature spec.
- **AI context window limits** ([specs/tech-stack.md §44](../tech-stack.md#L44)). Feature specs are LLM-consumed by `spec-execute`. The skill's "no marketing language" rule and the per-task atomicity discipline propagate this constraint by keeping individual tasks bounded.
- **Spec-driven-development convention** ([specs/tech-stack.md §51](../tech-stack.md#L51)). The methodology repo "eats its own cooking" — changes to the methodology follow the methodology. This convention is the explicit justification for this retroactive spec.
- **Repository layout convention** ([specs/tech-stack.md §48](../tech-stack.md#L48)). `specs/YYYYMMDD-<feature-name>/feature.md` + `journal.md` is the canonical output path, set by the [spec-path-convention](../20260515-spec-path-convention/architecture.md) propagation in commit `4ebec0c`.

### Dependencies

- **Upstream.**
  - [project-constitution](../../.agents/skills/project-constitution/SKILL.md) — read via `CONSTITUTION_PATHS`. Constitution declares in-scope/out-of-scope and the tech-stack; the feature spec must not propose work outside that scope or stack without an explicit constitution amendment.
  - [spec-design](../../.agents/skills/spec-design/SKILL.md) — read via `DESIGN_SPEC_PATH`. Design spec's vocabulary, components, NFRs, adoption path, and declared open questions are authoritative input; `spec-write` does not redesign. Contradictions between the design spec and the codebase route to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md). The [spec-design retroactive spec §3 Dependencies](../20260518-spec-design-skill/architecture.md#3-background-and-constraints) confirms this handoff from the design-spec side.
- **Downstream.**
  - [spec-execute](../../.agents/skills/spec-execute/SKILL.md) — consumes the §7 Task Breakdown, advancing one task at a time with closeout at each task boundary.
  - [spec-review](../../.agents/skills/spec-review/SKILL.md) — runs against the §9 Review Checkpoints declared in each feature spec.
  - [spec-amend](../../.agents/skills/spec-amend/SKILL.md) — applies the Amendment Protocol to feature specs when execution surfaces drift; also amends this spec and SKILL.md per the protocol.
- **Lateral.** Sibling lifecycle skills referenced `spec-write` by name and path; all six were modified together in commit `4ebec0c` when path conventions changed.

## 4. Architecture

The skill's architecture is a **three-phase authorship workflow** that produces a **paired markdown artifact pair** (feature spec + journal) conforming to a **bundled 14-section template**, executed as a **portable atomic unit** that gracefully integrates upstream constitution and design-spec inputs when they exist.

### Output topology

```text
specs/YYYYMMDD-<feature-name>/
├── feature.md           (Phase 3 output — the feature spec)
└── journal.md           (paired artifact, co-created)
```

The paired-artifact pattern is binding. A feature spec without a journal is not a complete output; the journal carries the session's decision rationale, friction observations, and per-task closeout entries during `spec-execute`. The journal is not optional supplementary material.

### Vocabulary (defined here, used consistently below)

- **Feature spec** — the output of this skill. A contract for code that will be written: atomic tasks, tests, review checkpoints, rollout, rollback. Distinct from a design spec.
- **Design spec** — the output of `spec-design`. A contract for an architectural commitment: shape, vocabulary, principles, adoption path. Read by `spec-write` via `DESIGN_SPEC_PATH` as authoritative input when present.
- **Constitution** — the output of `project-constitution`: `mission.md`, `tech-stack.md`, and optionally `roadmap.md` / `validation.md`. Read by `spec-write` via `CONSTITUTION_PATHS` as scope-binding input when present.
- **Discovery / Clarify / Spec Document** — the three phases of the workflow.
- **Discovery Report** — Phase 1 output: upstream-spec orientation (if applicable), codebase orientation, conventions in use, existing components, external dependencies, touch surface, test infrastructure, observability.
- **Task** — an atomic dev unit declared in §7 Task Breakdown. Independently reviewable, independently testable, small enough to land in a single pull request, sized one engineer-day or smaller.
- **Acceptance criteria** — objectively verifiable conditions per task (Given/When/Then or equivalent). "Works well" and "is performant" are explicitly prohibited.
- **Definition of Done** — per-task closeout checklist: code merged, tests passing in CI, documentation updated, observability hooks in place, no new lint or type errors, peer reviewed.
- **Review Checkpoint** — a named gate declared in §9 of a feature spec, reviewable via `spec-review`. Each checkpoint is a deployable / revertible stopping point.
- **Authoritative vs Inspirational** — §14 References distinction inherited from the design-spec form. Authoritative sources are binding (tool conventions, RFCs, design specs read as input); Inspirational sources are non-binding prior art and patterns.

### Execution model

```text
Phase 1: DISCOVERY                Phase 2: CLARIFY                   Phase 3: SPEC DOCUMENT
(upstream-spec orientation        (assumptions, OQ triage            (single self-contained
 if applicable; codebase   ───▶    by [blocker]/[important]/  ───▶   markdown; 14-section
 orientation; conventions;         [minor]; proposed-on-non-          template; paired journal)
 components; tests;                response decisions;
 observability)                    pause for operator input)
```

The pause at Phase 2 is load-bearing: it is the operator's primary input channel and the only place where assumptions and open questions are confirmed before Phase 3 commits to them. Blocker open questions must be resolved before Phase 3 begins; skipping the pause biases Phase 3 toward operator-absent defaults.

### Where this design plugs in

`spec-write` is a mid-pipeline node: downstream of `project-constitution` and `spec-design`, upstream of `spec-execute` and `spec-review`. Outputs are referenced by name from `spec-execute` sessions (via `SPEC_PATH`) and from review verdicts (via `SPEC_PATH`). The skill itself is invoked by the operator, not by other skills.

## 5. Detailed Design

### 5.1 Phase 1 — Discovery

**Purpose.** Orient the spec author against upstream commitments (constitution, design spec, if present), the existing codebase, and the test/observability infrastructure before any commitment to architecture or tasks is made.

**Inputs.** `FEATURE_NAME`, `FEATURE_DESCRIPTION`, `CODEBASE_ROOT`, `KEY_ENTRY_POINTS`, `TARGET_BRANCH`, `DEADLINE_OR_CONSTRAINTS`, `KNOWN_CONSTRAINTS`, `NON_GOALS`, `DESIGN_SPEC_PATH` (optional), `CONSTITUTION_PATHS` (optional).

**Outputs.** Discovery Report covering: **upstream-spec orientation** (if `DESIGN_SPEC_PATH` or `CONSTITUTION_PATHS` is provided — see §5.4); **codebase orientation** (languages, frameworks, build system, package manager, test runner, CI, deployment target, cited per file); **conventions in use** (module layout, naming, error handling, logging, configuration, DI, test organization, with quoted examples); **existing components relevant to the feature** (file paths); **external dependencies** (libraries, services, APIs, databases, queues); **touch surface estimate** (hypothesis-grade list of files to be created/modified/deleted); **test infrastructure** (structure, coverage, fixtures, factories, integration, e2e); **observability and operations** (logging, metrics, tracing, alerting, feature flags, deployment).

**Behavior.** Read-only. If `KEY_ENTRY_POINTS` is not provided, the skill identifies them itself and records how (README, package manifest, framework conventions). The Discovery Report does not commit to architecture or tasks — those are Phase 3 outputs informed by the report.

**Pattern invoked.** *Discovery-before-Design*, the same Phase 1 shape used by `spec-design` and `project-constitution`. The skill is consistent with sibling skills' shape.

**Why this design.** A feature spec authored without reading the codebase ([predecessor doc design note at line 211](../../docs/spec-driven-development-prompts-conversation.md#L211): *"Most spec failures come from agents that skip discovery and produce a plausible-sounding plan that doesn't match the codebase."*) is the single largest failure mode for spec-driven development. The Discovery Report forces the agent to ground every Phase-3 commitment in observation rather than imagination.

**Alternatives considered.** Skipping Discovery (operator-supplied INPUTS alone). Rejected — predecessor design notes are explicit that the forced pause is "the single biggest lever for spec quality."

### 5.2 Phase 2 — Clarify (pause point)

**Purpose.** Resolve what Discovery cannot determine and pause for explicit operator input before producing any feature spec.

**Inputs.** Discovery Report (from Phase 1); operator's responses to assumptions and open questions.

**Outputs.** Assumptions about the feature, environment, or user; triaged open questions (`[blocker]` / `[important]` / `[minor]`); decisions proposed-unilaterally-on-non-response with rationale.

**Behavior.** Items presented in declared order: assumptions, open questions, proposed-unilateral decisions. The skill **stops and waits**. Blockers must be resolved before Phase 3.

**Pattern invoked.** *Triage-by-severity* — identical to `spec-design` and `project-constitution` Phase 2.

**Why this design.** Assumptions and open questions surface what Discovery could not determine. Confirming them with the operator (rather than guessing) prevents Phase 3 from authoring against incorrect assumptions, and creates a record the operator can challenge before the spec body is written.

**Alternatives considered.** Continuous Clarify (skill proceeds to Phase 3 as soon as it has enough). Rejected — the pause-and-wait discipline is the difference between collaborative authorship and skill-driven monologue.

### 5.3 Phase 3 — Spec Document

**Purpose.** Produce the feature-spec artifact and its paired journal.

**Inputs.** Discovery Report + Clarify outputs.

**Outputs.** Two files: `specs/YYYYMMDD-<feature-name>/feature.md` (the feature spec) + `specs/YYYYMMDD-<feature-name>/journal.md` (the paired journal).

**Behavior.** The feature spec conforms to the **14-section template** (§5.8). Sections §1–§6 share structure with design specs. Sections §7 (Task Breakdown, not Implementation Sequencing), §8 (Test Strategy, not Validation Approach), and §11 (Rollout and Rollback, not Adoption Path) are *feature-spec-specific* — they deliberately differ from `spec-design`'s equivalents. The journal records session decisions and is structured to receive `spec-execute` task-closeout entries during execution.

**Pattern invoked.** *Template-driven scaffolding* with operator-supplied content. The skill ships the template inline in its SKILL.md (per the Atomic-Skill Portability Principle: bundled defaults rather than host-repo lookup).

**Why this design.** Inline templates remove host-repo dependency and make the skill's contract auditable from SKILL.md alone. The §7/§8/§11 distinctions are the load-bearing differences between feature specs and design specs: collapsing them produces specs that don't fit either shape.

**Alternatives considered.** Single-file output (no journal). Rejected — the journal carries `spec-execute` closeout entries and N+1 mining inputs that cannot be reconstructed from `feature.md` alone. Reuse the `spec-design` template wholesale. Rejected — atomic tasks, test strategy, and rollout do not fit design-spec sections.

### 5.4 Upstream-spec orientation (`DESIGN_SPEC_PATH` and `CONSTITUTION_PATHS`)

**Purpose.** Integrate `spec-write` with the methodology's upstream artifacts when they exist, while degrading cleanly when they do not.

**Behavior.**
- **`DESIGN_SPEC_PATH` provided.** Phase 1 reads the design spec in full *first*. The skill quotes the design spec's declared vocabulary, the components it commits to, its NFRs, and its named downstream-spec references. These become the authoritative frame for the rest of Discovery. Phase 3 does **not** redesign work the design spec already committed to. Contradictions between the design spec and the codebase route to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md), not silent workaround.
- **`DESIGN_SPEC_PATH` absent.** The skill proceeds without upstream design-spec orientation; Phase 3 makes its own architectural commitments under §4 Architecture and §5 Detailed Design, citing patterns invoked per Operating Principle 5.
- **`CONSTITUTION_PATHS` provided.** Phase 1 reads `mission.md`, `tech-stack.md`, and `roadmap.md` / `validation.md` if present. The feature spec must not propose work outside the declared scope or tech-stack. Scope-expansion routes through a constitution amendment first, not through silent inclusion.
- **`CONSTITUTION_PATHS` absent.** The skill proceeds without constitutional scope binding; the operator becomes the sole authority on in-scope/out-of-scope.

**Pattern invoked.** *Graceful degradation* per [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33). The skill works whether or not the host repo has upstream artifacts; richer host repos get richer orientation.

**Why this design.** A skill that requires upstream artifacts is non-portable; a skill that ignores them when present wastes operator effort. Optional inputs with explicit when-provided behavior preserve both portability and integration.

**Alternatives considered.** Make `DESIGN_SPEC_PATH` and `CONSTITUTION_PATHS` mandatory. Rejected — fails the portability principle. Auto-discover them by scanning `specs/`. Rejected — implicit discovery violates explicit-operator-input discipline; operator must point at the upstream artifact.

### 5.5 Task Breakdown atomicity

**Purpose.** Ensure each task in §7 is independently reviewable, independently testable, and small enough to land in a single pull request.

**Behavior.** Every task in the breakdown declares: Task ID (e.g., `T-01`), Title, Scope (files to create/modify; function or class names), Acceptance criteria (Given/When/Then or equivalent, objectively verifiable), Tests required (named test files; unit / integration / manual), Definition of Done (code merged, tests passing in CI, docs updated, observability in place, no new lint or type errors, peer reviewed), Dependencies (other task IDs), Estimated size (S/M/L; L tasks must be split before implementation). Tasks are sequenced so the branch is in a deployable or revertible state at each task boundary. Task descriptions take the form *"Add `<file>` exposing `<function>` such that `<acceptance criteria>`"* — not the form *"Implement X"*; the latter under-specifies scope and AC.

**Pattern invoked.** *Atomic, reviewable tasks* — Operating Principle 3 in the shipping SKILL.md. *Reversibility* — Operating Principle 6.

**Why this design.** Tasks larger than one engineer-day are spec-failure modes: they merge in batches that can't be cleanly reverted; their tests bolt on at the end rather than alongside; their acceptance criteria drift into "works well." The atomicity discipline catches these at spec authoring time, not at PR review time.

**Alternatives considered.** Allow L tasks if "complex." Rejected — predecessor design notes are explicit ([line 215](../../docs/spec-driven-development-prompts-conversation.md#L215)): *"This stops the 'code is written, just need to add tests later' failure mode."* Tasks must split; the spec author does the splitting at authoring time.

### 5.6 Test Strategy as first-class

**Purpose.** Ensure the test strategy is designed alongside the feature, not bolted on.

**Behavior.** §7 per-task entries declare Tests required (named test files). §8 Test Strategy is a separate section declaring: unit test approach (coverage targets, mocking strategy, factories/fixtures); integration test approach (boundaries exercised end-to-end); property-based or fuzz tests where applicable; manual verification steps a reviewer can run locally; test data approach (realistic data seed/generation).

**Pattern invoked.** *Tests are first-class* — Operating Principle 4 in the shipping SKILL.md.

**Why this design.** Test strategy authored after implementation tends to test what the code does rather than what the spec requires. Authoring tests-per-task alongside the task — plus a unifying §8 strategy section — keeps tests bound to acceptance criteria rather than to incidental code paths.

**Alternatives considered.** Omit §8 if tests are declared per-task. Rejected — per-task tests cover the *what*; §8 covers the *how* (mocking strategy, fixture design, coverage targets) which is repeated infrastructure that benefits from one declaration. Allow informal "works on my machine" verification. Rejected — Operating Principle 4 and the explicit ban in WHAT NOT TO DO ("works well", "is performant").

### 5.7 Citation discipline

**Purpose.** Anchor design commitments to named, canonical patterns rather than to invented vocabulary.

**Behavior.** When the spec invokes a pattern (e.g., Repository, Strategy, OpenTelemetry conventions, RFC 7807 Problem Details), it **names** the pattern and **links** to a canonical source where one exists. §5 Detailed Design subsections include a Pattern invoked field per component. §14 References distinguishes Authoritative (binding) from Inspirational (prior art) — the same distinction used in design specs.

**Pattern invoked.** *Cite practices* — Operating Principle 5 in the shipping SKILL.md.

**Why this design.** Named patterns let reviewers verify that the spec's commitments match the canonical pattern's actual behavior, not the spec author's recollection of it. Invented vocabulary is a confabulation surface.

**Alternatives considered.** Cite patterns informally (no canonical link). Rejected — informal citation is indistinguishable from no citation under review, and links rot less than recollection.

### 5.8 Section template

**Purpose.** Standardize feature-spec output across sessions.

**Interface.** Exactly fourteen sections, exact headings, declared order:

```markdown
## 1. Overview
## 2. Goals and Non-goals
## 3. Background and Constraints
## 4. Architecture
## 5. Detailed Design
## 6. Non-functional Requirements
## 7. Task Breakdown
## 8. Test Strategy
## 9. Review Checkpoints
## 10. Risks and Mitigations
## 11. Rollout and Rollback
## 12. Out of Scope
## 13. Open Questions
## 14. References
```

**Behavior.** Sections §1–§6 share structure with design specs. Sections §7, §8, §11 are feature-spec-specific and **deliberately differ** from design-spec equivalents: §7 enumerates atomic tasks (not phases of work); §8 commits to test approach (not validation by review/dogfooding); §11 plans rollout and rollback (not architectural adoption).

**Pattern invoked.** Section template from [docs/spec-driven-development-prompts-conversation.md lines 90–187](../../docs/spec-driven-development-prompts-conversation.md#L90) — predecessor canonical for this skill.

**Why this design.** A design-spec template forced onto a feature artifact produces contorted sections (vague "phases" instead of atomic tasks; "validation approach" instead of test strategy; "adoption path" instead of rollout). Distinct §7/§8/§11 prevent this.

**Alternatives considered.** Reuse `spec-design`'s template. Rejected (see §5.3 rationale). Fewer sections (e.g., merge §7+§8 or §11+§12). Rejected — each section answers a distinct review-time question; merging trades reviewability for brevity.

## 6. Non-functional Requirements

| NFR | Requirement | Source |
|---|---|---|
| **Portability** | Skill functions when installed at `.agents/skills/spec-write/` and works against unrelated host repos that lack methodology-specific upstream artifacts (degrades cleanly). No runtime dependency on host-repo files for the skill itself to function. 14-section template and workflow bundled in SKILL.md. | [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) |
| **Conciseness** | Feature specs are LLM-consumed by `spec-execute`. The skill's "no marketing language" rule and per-task atomicity discipline propagate the [AI context window limit](../tech-stack.md#L44) constraint. Tasks bounded at one engineer-day or smaller; L tasks must be split before implementation. | [SKILL.md OPERATING PRINCIPLES + WHAT NOT TO DO](../../.agents/skills/spec-write/SKILL.md) |
| **Citation discipline** | Patterns invoked are named with canonical-source links where they exist. No informal "this is like X" without naming X. | [SKILL.md OPERATING PRINCIPLES §5](../../.agents/skills/spec-write/SKILL.md) |
| **Self-containment** | Each produced feature spec reads independently of the originating chat. No "as we discussed"; every named pattern, component, or upstream commitment defined or linked. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-write/SKILL.md) |
| **Atomicity** | Tasks are independently reviewable, independently testable, and small enough to land in a single PR. L tasks split before implementation. Acceptance criteria objectively verifiable. | [SKILL.md OPERATING PRINCIPLES §3 + WHAT NOT TO DO](../../.agents/skills/spec-write/SKILL.md) |
| **Reversibility** | Each Review Checkpoint is a safe stopping point. The branch is in a deployable or revertible state at each merge. | [SKILL.md OPERATING PRINCIPLES §6](../../.agents/skills/spec-write/SKILL.md) |
| **Tests-first** | Test strategy is designed alongside the feature, not bolted on. Each task names the tests that prove it works. | [SKILL.md OPERATING PRINCIPLES §4](../../.agents/skills/spec-write/SKILL.md) |
| **Format fidelity** | Output conforms to the 14-section template with exact headings and declared order. §7/§8/§11 use feature-spec form, not design-spec form. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-write/SKILL.md) |
| **Markdown hygiene** | All code blocks specify a language. Tables and lists conform to GitHub-flavored markdown rendering conventions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |
| **Pairing** | Every feature spec is accompanied by a journal at the same directory. The pair is the output. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |
| **Pause discipline** | Phase 2 stops and waits for operator input on assumptions and open questions. No Phase-3 work with unresolved blocker OQs. | [SKILL.md PHASE 2 — CLARIFY](../../.agents/skills/spec-write/SKILL.md) |
| **Upstream integration** | When `DESIGN_SPEC_PATH` is provided, the design spec is authoritative input. When `CONSTITUTION_PATHS` is provided, declared scope and tech-stack are binding. Both inputs are optional; absence does not block the skill. | [SKILL.md INPUTS + PHASE 1](../../.agents/skills/spec-write/SKILL.md) |
| **Multi-repo awareness** | If the spec lives in a different repo than the codebase it describes, §3 Background notes this and the spec includes `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` for downstream `spec-execute` sessions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |
| **Dependency hygiene** | Specs do not introduce new frameworks, ORMs, or major dependencies without an explicit "Alternatives considered" subsection justifying the addition. Default is to match existing codebase conventions; deviation requires written rationale. | [SKILL.md OPERATING PRINCIPLES §7 + WHAT NOT TO DO](../../.agents/skills/spec-write/SKILL.md) |

## 7. Implementation Sequencing

The skill ships. This spec is descriptive, not implementation-planning. The sequencing for *adopting this spec* is:

1. **Spec authored** (this document and its sibling journal, committed together as a paired commit).
2. **CP-1 review** (§9): retroactive spec faithfully describes the shipping skill. Triggered by a fresh `/spec-review` session against this spec's CP-1 — operator chose the sequenced path (per N=1 and N=2 precedent), deferring CP-1 to a separate session for cleaner authorship-vs-review separation.
3. **CP-2 drift audit** (§9): batched with the other four legacy-quintet specs per [docs/retroactive-spec-strategy.md §"Drift mitigation"](../../docs/retroactive-spec-strategy.md). Any divergence routes to `/spec-amend` (not silent edit).
4. **Adoption complete**: this spec becomes the reviewable contract for future SKILL.md changes; subsequent SKILL.md edits follow the Amendment Protocol via `/spec-amend`.

There is no Phase B "build the skill" — the skill is already built. There is no downstream feature spec for this skill; this spec is terminal for the skill's specification, except for any amendment-driven follow-up.

> Note: This section deliberately differs from a feature spec's Task Breakdown. Design specs do not decompose into atomic dev tasks; retroactive design specs particularly do not, because the implementation predates them.

## 8. Validation Approach

| Approach | What it validates |
|---|---|
| **Stakeholder review** | Eric (operator) reviews this spec for fidelity to intent. CP-1 is the gate. |
| **Drift audit** | Mechanical comparison of SKILL.md commitments to this spec's commitments. CP-2 is the gate. Output is a divergence list (possibly empty). Batched with the four sibling quintet specs. |
| **Predecessor cross-check** | [docs/spec-driven-development-prompts-conversation.md lines 17–225](../../docs/spec-driven-development-prompts-conversation.md) is the skill's design-rationale source. Every spec-side commitment in §5 of this document traces back either to a behavior in SKILL.md (current) or to a recommendation in the predecessor doc (rationale). Gaps between predecessor and SKILL.md are *evolution* (trilogy commit `80000b1`, session-economy commit `5ce4024`, path-convention commit `4ebec0c`), not drift. CP-2 reads this distinction. |
| **Downstream consumption** | Feature specs authored by the skill — [specs/20260515-spec-path-convention/feature.md](../20260515-spec-path-convention/feature.md), [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md), [specs/20260517-finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) — are evidence that the section template and three-phase workflow produce specs that downstream skills can consume. |

> Note: This section deliberately differs from a feature spec's Test Strategy. Design specs are validated by review, audit, predecessor cross-check, and downstream consumption — not by automated test coverage.

## 9. Review Checkpoints

### CP-1 — Retroactive spec faithfully describes the shipping skill

**Status:** pass with comments on 2026-05-18 by Claude (agent reviewer). 0 blockers, 0 important, 2 advisory. See [journal.md "Review of CP-1"](./journal.md) for findings and recommendation. Checkpoint closed.

**Trigger.** This spec and its journal are committed; the operator invokes `/spec-review` against this spec's CP-1 in a fresh session.

**Review focus.**
- Every commitment in §4, §5, and §6 corresponds to behavior actually present in [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md).
- No commitment in this spec contradicts the shipping SKILL.md.
- The **Atomic-Skill Portability Principle** is correctly characterized as a binding constraint (§3, §6) including the optional behavior when `DESIGN_SPEC_PATH` or `CONSTITUTION_PATHS` is absent (§5.4).
- The **predecessor doc** is correctly distinguished as authoritative for design rationale, not authoritative for current behavior (§3 Background; cited Inspirational in §14).
- §13 Open Questions reports "none surfaced" honestly — i.e., the four candidates surfaced in Phase 2 were placed in §12 (three) or dropped (one) by operator triage, not silently omitted.
- The spec is self-contained per the Operating Principles in [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md).
- Section-heading citations point to the heading line, not the body's first line (carried forward from [N=2 CP-1 finding](../20260518-spec-design-skill/journal.md)).

**Exit criteria.**
- Reviewer issues a verdict of `pass`, `pass with comments`, or `changes requested` per the structured format declared in [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- All `[blocker]` findings (if any) are resolved or escalated to `/spec-amend`.
- Verdict is written back to this spec's §9 (status line) and to the journal.

### CP-2 — Drift audit complete (batched)

**Status:** pass with comments on 2026-05-18 by Claude (agent reviewer). 0 blockers, 0 important, 5 advisory. See [journal.md "Review of CP-2"](./journal.md) for findings and routing, and [journal.md "CP-2 closeout"](./journal.md) for the closing summary. Checkpoint closed via amendments 2026-05-18-1 (D-1, spec §6 Dependency hygiene NFR), 2026-05-18-2 (D-2, SKILL.md §14 Authoritative/Inspirational split), 2026-05-18-3 (D-3, spec §6 Markdown hygiene NFR), 2026-05-18-4 (D-4, spec §5.5 task-phrasing anti-pattern), and 2026-05-18-5 (D-5, SKILL.md preamble three-item enumeration). All five advisories resolved by amendment; none accepted.

**Trigger.** CP-1 of this spec passes, AND CP-1 of the three remaining quintet specs (`spec-execute`, `spec-review`, `spec-amend`) passes, AND CP-1 of [spec-design](../20260518-spec-design-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND project-constitution's CP-2 has either run or been folded into the batch per [docs/retroactive-spec-strategy.md OQ-1](../../docs/retroactive-spec-strategy.md).

**Review focus.** A line-by-line audit of [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md) against this spec's §4, §5, §6, and §12. The auditor enumerates each divergence: a behavior present in SKILL.md but not committed in the spec, or a commitment in the spec absent from SKILL.md. Cross-skill drift patterns (e.g., four skills citing the Atomic-Skill Portability Principle correctly and one quietly not) are explicitly in scope by virtue of the batch context.

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
| The predecessor doc is treated as authoritative for current behavior rather than for design rationale, causing CP-2 false positives. | Medium | Low–Medium | §3 Background explicitly distinguishes the two; §8 Validation Approach calls out the three commits (`80000b1`, `5ce4024`, `4ebec0c`) that account for evolution; CP-2 auditor reads §3 before walking divergences | CP-2 auditor |
| §13 reports "no Open Questions" because the operator triaged candidates to §12 / drop, which may strike a future reader as suspicious. | Low | Low | Journal records the four Phase-2 OQ candidates and their triage outcomes; CP-1 review focus explicitly checks that "none surfaced" is honest rather than silent omission | Eric / CP-1 reviewer |
| N=3 patterns over-codify, foreclosing better N=4+ patterns at the sibling quintet specs. | Low | Medium | §2 Non-goals explicitly disclaims template-establishing intent; this spec's journal records "Pattern for N=4" callouts that are *candidates*, not declarations; the N=2-inflection-point decision is recorded in this journal | Future retroactive-spec sessions; operator at session 3 close |
| `DESIGN_SPEC_PATH` consumption behavior (§5.4) commits to amendment-on-contradiction but the mechanics across the `spec-write` → `spec-amend` handoff are unspecified ([strategy-doc OQ-3](../../docs/retroactive-spec-strategy.md)). | Medium | Low — discovered cases route to operator judgment until formalized | Strategy doc names the gap; cross-skill amendment coordination is out of scope for this spec (§12); will surface as a real OQ on `spec-amend`'s retroactive spec | Future `spec-amend` retroactive spec session |

## 11. Adoption Path

The spec is adopted in three steps, matching N=1 and N=2:

1. **Commit the spec and journal** as a paired commit. The journal is the durable record of this session's decisions and observations, structured to be mined by sessions 3–5 of the legacy quintet.
2. **CP-1 review** in a fresh session (sequenced, per operator's choice): invoke `/spec-review` against this spec's CP-1.
3. **CP-2 drift audit** in the batched quintet-CP-2 session: per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md).

After adoption, the spec is a *living contract*. Edits to SKILL.md follow the Amendment Protocol: stated section being amended, before/after diff, reason and impact, explicit operator approval, journal entry. Per the [N=1 amendment 2026-05-17-1 N=2 mining note](../20260517-project-constitution-skill/journal.md), amendments that touch a *class* of references (paths, citations, vocabulary) must scan the entire spec at Phase 1 Orient, not just the locations called out by the triggering finding.

### Reversibility

The spec can be retired without affecting the skill. The SKILL.md remains the canonical implementation. Retirement would be unusual (it would be the methodology rejecting its own dogfooding convention for `spec-write` specifically) but mechanically clean: delete the spec directory and record the retirement in `roadmap.md`.

### Cross-session knowledge transfer

This spec's [journal.md](./journal.md) records validation/refinement/rejection outcomes for each "Pattern for N=3" callout from the [N=2 journal](../20260518-spec-design-skill/journal.md), and the **N=2 inflection-point decision** on `docs/retroactive-spec-pattern.md` is recorded there. Sessions 3–5 read this journal as their primary mining input.

## 12. Out of Scope

- **The N=2 inflection-point decision on `docs/retroactive-spec-pattern.md`.** Per [docs/retroactive-spec-strategy.md §"N=2 inflection point"](../../docs/retroactive-spec-strategy.md), the decision is made at this session's close. It is recorded in this spec's [journal.md](./journal.md), not in this spec body. The spec body is descriptive of the skill, not of the strategy.
- **Cross-spec amendment coordination** when a `DESIGN_SPEC_PATH` and the codebase contradict in a way that requires coordinated edits to the design spec, the feature spec, and possibly SKILL.md across skills. Named at [docs/retroactive-spec-strategy.md OQ-3](../../docs/retroactive-spec-strategy.md). The cross-skill amendment mechanics will surface on the `spec-amend` retroactive spec.
- **Multi-repo `SPEC_REPO_ROOT`/`SPEC_TARGET_BRANCH` mechanics under multi-target conditions** (one feature spec spanning multiple downstream code repos; spec-repo lifecycle reflected in adopter repos). The single-repo case is the only case exercised in `ai-tools`. Multi-repo mechanics named in SKILL.md OUTPUT FORMAT are not bounded by this spec. Mirrors N=2's identical disposition.
- **Resolving the format-question-prompt gap** ([N=2 §13 OQ-1](../20260518-spec-design-skill/architecture.md)). The question of when an operator should invoke `/spec-write` vs `/spec-design` is methodology-wide and named at first-class detail in the N=2 spec; it is not re-raised here.
- **Task-sizing heuristics formalized beyond SKILL.md's commitments.** SKILL.md commits to "L tasks must be split" and "no larger than one engineer-day"; formal heuristics for *how* to size tasks at authoring time are operator judgment per session, not declared here.
- **Redesign of the `spec-write` skill.** Redesign routes to a new design spec under amendment governance.
- **Modification of the shipping SKILL.md.** Only `/spec-amend` touches SKILL.md.
- **A template for the three remaining legacy-quintet retroactive specs** (`spec-execute`, `spec-review`, `spec-amend`). The N=2 inflection point governs whether a `docs/retroactive-spec-pattern.md` is justified; the decision is recorded in the journal, not as a declaration in this spec body.
- **External-claim verification beyond repo-internal citation.** Light verification was adopted for this spec; the heavy-verification path (WebFetch/WebSearch against canonical sources) exists in the methodology but is not exercised by this spec's text.
- **Constitution-amendment ceremony.** Inherited from [N=1 OQ-1](../20260517-project-constitution-skill/architecture.md#L274) and tracked at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md). Out of scope here.

## 13. Open Questions

**None surfaced in this session.**

Four open-question *candidates* were identified in Phase 1 Discovery and triaged with the operator in Phase 2:

| Candidate | Triage outcome |
|---|---|
| N=2 inflection point on `docs/retroactive-spec-pattern.md` | §12 Out of Scope (decision made in this session's journal) |
| `DESIGN_SPEC_PATH`-vs-codebase contradiction mechanics across the `spec-write` → `spec-amend` handoff | §12 Out of Scope (named at strategy-doc OQ-3) |
| Task-size discipline ("working day" / "L must be split") not formalized at authoring time | Dropped (SKILL.md commits to the criteria as written; no spec-level gap) |
| Multi-repo `SPEC_REPO_ROOT` lifecycle under multi-target conditions | §12 Out of Scope (mirrors N=2's disposition; single-repo only) |

"None surfaced" is honest reporting: the candidates exist and were considered, but each was placed where it belongs rather than promoted to §13 by default. The journal records the triage protocol so a future reader can audit the decision.

## 14. References

### Authoritative

- [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md) — the shipping skill. Authoritative for behavior.
- [specs/tech-stack.md](../tech-stack.md) — methodology constraints, including the Atomic-Skill Portability Principle binding on this skill.
- [specs/mission.md](../mission.md) — `ai-tools` mission; defines audience and in/out of scope for the methodology.
- [specs/roadmap.md](../roadmap.md) — `ai-tools` roadmap; lists `spec-write` as a Phase 1 deliverable.

### Inspirational

- [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225 — the `spec-writing-prompt.md` artifact + companion design notes that became this skill. Authoritative for the skill's *design rationale*; not authoritative for current behavior (which has evolved through commits `80000b1`, `5ce4024`, `4ebec0c`).
- [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) — N=1 retroactive design spec; original structural source.
- [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) + [journal.md](../20260518-spec-design-skill/journal.md) — N=2 retroactive design spec; closest-sibling structural source. "Pattern for N=3" callouts validated/refined in this N=3 journal.
- [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) — strategy doc for the legacy-quintet retroactive specs; orientation material for this session.
- [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md), [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) — concurrent architectural work cited as negative-signal sources in this spec's journal.
- [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md), [specs/20260517-finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) — sibling skills with feature specs (authored before the skills shipped); provided the `<skill-name>-skill` directory-slug convention used here.
