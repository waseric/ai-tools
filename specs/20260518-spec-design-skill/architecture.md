# `spec-design` Skill — Architecture and Protocol Specification

> Status: Draft — Open for Review
> Date: 2026-05-18
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set.

## 1. Overview

The `spec-design` skill authors **design specs**: self-contained architecture or protocol specifications that commit to a *shape* — vocabulary, components, principles, open questions, adoption path — before any feature spec decomposes work into atomic tasks. It sits between `project-constitution` (upstream) and `spec-write` (downstream) in the methodology's spec-driven-development pipeline. Its outputs are read by downstream feature specs as authoritative input rather than redesigned.

This document is a **retroactive design specification**: the skill already ships at [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md), which is authoritative for the skill's behavior. The spec describes what the skill *is* and what it *commits to*, so the methodology can be reviewed, amended, and adopted on the same footing as the artifacts it produces. The spec does not redesign the skill; any divergence the spec exposes between its commitments and the shipping SKILL.md is routed to [/spec-amend](../../.agents/skills/spec-amend/SKILL.md), never silently corrected.

## 2. Goals and Non-goals

### Goals

- Produce a self-contained, descriptive specification of the `spec-design` skill's vocabulary, contract, phases, and operating model.
- Declare review gates (§9) so the skill becomes reviewable via [/spec-review](../../.agents/skills/spec-review/SKILL.md) against named checkpoints.
- Surface the **format-question-prompt gap** as a first-class Open Question (§13 OQ-1) — a recommendation from the skill's predecessor doc that did not land in the shipping SKILL.md, called out as friction in the N=1 journal.
- Hold the skill to the **Atomic-Skill Portability Principle** declared in the methodology's constitution ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)).
- Continue the **paired-artifact pattern** for retroactive skill specs — `architecture.md` + `journal.md` — established at N=1 ([specs/20260517-project-constitution-skill/](../20260517-project-constitution-skill/)). This N=2 instance validates, refines, or rejects each of the N=1 journal's "Pattern for N=2" callouts.

### Non-goals

- **Redesign of the skill.** The shipping SKILL.md is authoritative for behavior. This spec is descriptive.
- **A template for the three sibling legacy-quintet retroactive specs** (`spec-write`, `spec-execute`, `spec-review`, `spec-amend`). Each is authored in its own session per the operator's spec-by-spec cadence; cross-session scaffolding, if any, derives from N=2+ observations recorded in journals, not from declarations in this spec body. The N=2-inflection-point decision on `docs/retroactive-spec-pattern.md` is governed by [docs/retroactive-spec-strategy.md §"N=2 inflection point"](../../docs/retroactive-spec-strategy.md) and resolved at session 2's (`spec-write`) close — not here.
- **Modification of the shipping SKILL.md.** Only the Amendment Protocol via [/spec-amend](../../.agents/skills/spec-amend/SKILL.md) touches SKILL.md.
- **Resolving §13 OQ-1.** Naming the format-question-prompt gap is not resolving it; resolution routes to a future amendment session or a meta-entrypoint design.
- **Specifying tooling, models, or platforms.** The skill produces markdown only and remains tooling-agnostic, consistent with the [mission.md Out of Scope](../mission.md) commitment.

## 3. Background and Constraints

### Prior state

The skill was introduced in commit `49c15f0` (2026-05-14) as part of the trilogy addition (`spec-design` + `project-constitution` + `spec-amend`) that completed the spec-driven-development core. Subsequent evolution:
- `e483466` (2026-05-14) — session-economy and multi-repo disciplines added across the skill family.
- `d9a0002` (2026-05-14) — `lastUpdated` frontmatter added.
- `6d158fb` (2026-05-15) — path convention update: `docs/specs/<feature>.md` → `specs/YYYYMMDD-<name>/`, applied uniformly across the six lifecycle skills via the `spec-path-convention` feature spec.

The closest thing to a design document the skill ever had is [docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md) — a recommendations doc extracted from a session that produced an `ai-frontmatter-distributor-architecture.md` design artifact in an external private repo (`private-design-repo`). The recommendations doc is treated in this spec as **authoritative for the skill's design rationale** but not authoritative for current behavior: the shipping SKILL.md is the latter. Where they diverge, SKILL.md wins and the divergence is a candidate finding for CP-2.

**Recursive use.** This session is the `spec-design` skill authoring its own retroactive design spec — recursive use of the skill on itself. The same pattern occurred at N=1 ([specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md)) and demonstrates that the skill's three-phase shape (Discovery → Clarify → Spec Document) accommodates recursive authorship without special-casing. The recursion is an observation about retroactive-spec authoring, not an architectural property the skill commits to; the spec is silent on recursion as a general use case.

### Constraints (cited)

- **Atomic-Skill Portability Principle** ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)). The skill must be a portable atomic unit: workflow + schema knowledge + section template bundled in its own `SKILL.md`; adapts to richer host-repo embodiments (e.g., an existing constitution, sibling skills) when present; degrades cleanly when absent. A `spec-design` installed against an unrelated host repo with no methodology siblings still produces a conformant design spec.
- **AI context window limits** ([specs/tech-stack.md:44](../tech-stack.md#L44)). Design specs are LLM-consumed. Conciseness is a hard constraint on artifact length, not a style preference. SKILL.md propagates this constraint via the "no marketing language" rule and the recommended Phase-3 self-contained discipline; it does not impose explicit line caps as `project-constitution` does (because design-spec content scales with the artifact under design).
- **Spec-driven-development convention** ([specs/tech-stack.md:51](../tech-stack.md#L51)). The methodology repo "eats its own cooking" — changes to the methodology follow the methodology. This convention is the explicit justification for this retroactive spec: a skill that authors design specs must itself be design-spec'd.
- **Repository layout convention** ([specs/tech-stack.md:48](../tech-stack.md#L48)). `specs/YYYYMMDD-<artifact-name>/architecture.md` + `journal.md` is the canonical output path, set by the `spec-path-convention` propagation in commit `6d158fb`.

### Dependencies

- **Upstream.** [project-constitution](../../.agents/skills/project-constitution/SKILL.md) — design specs cite the constitution in §3 Background when present; [project-constitution architecture.md](../20260517-project-constitution-skill/architecture.md) confirms this handoff from the constitution side.
- **Downstream.**
  - [spec-write](../../.agents/skills/spec-write/SKILL.md) — reads a design spec as authoritative input via `DESIGN_SPEC_PATH`; produces a feature spec that names the design spec by path.
  - [spec-review](../../.agents/skills/spec-review/SKILL.md) — runs against the §9 Review Checkpoints declared in this spec (CP-1, CP-2).
  - [spec-amend](../../.agents/skills/spec-amend/SKILL.md) — applies the Amendment Protocol to this spec (and to SKILL.md when drift is found at CP-2).
- **Lateral.** Sibling lifecycle skills (`spec-execute`, `spec-review`, `spec-amend`, `spec-write`) reference `spec-design` by name and path; they were modified together in commit `6d158fb` when path conventions changed.

## 4. Architecture

The skill's architecture is a **three-phase authorship workflow** that produces a **paired markdown artifact pair** (design spec + journal) conforming to a **bundled 14-section template**, executed as a **portable atomic unit** with no runtime dependency on host-repo files.

### Output topology

```text
specs/YYYYMMDD-<artifact-name>/
├── architecture.md       (Phase 3 output — the design spec)
└── journal.md            (paired artifact, co-created)
```

The paired-artifact pattern is binding. A design spec without a journal is not a complete output; the journal carries the session's decision rationale, friction observations, and downstream mining inputs. The journal is not optional supplementary material.

### Vocabulary (defined here, used consistently below)

- **Design spec** — the output of this skill. A contract for an architectural commitment: shape, vocabulary, principles, open questions, adoption path.
- **Feature spec** — the output of `spec-write` (out of scope for this skill but distinguished). A contract for code that will be written: atomic tasks, tests, rollout, rollback.
- **Discovery / Clarify / Spec Document** — the three phases of the workflow.
- **Discovery Report** — Phase 1 output: landscape orientation, constraint orientation, conversation grounding, prior-art scan, naming candidates.
- **Verification commitment** — Phase 2 decision: *light* (repo-internal citation only) or *heavy* (external claims verified at canonical sources via WebFetch/WebSearch with verification date).
- **Open Question (OQ)** — first-class spec output (§13). Structured as Question / Analysis / Leaning / Owner / Watch items / Anti-goals.
- **Review Checkpoint** — a named gate declared in §9 of a design spec, reviewable via `spec-review`.
- **Authoritative vs Inspirational** — §14 References distinction. Authoritative sources are binding (tool conventions, RFCs, official specs); Inspirational sources are non-binding prior art and patterns.

### Execution model

```text
Phase 1: DISCOVERY                 Phase 2: CLARIFY                   Phase 3: SPEC DOCUMENT
(landscape, constraints,           (naming, audience,                 (single self-contained
 conversation grounding,    ───▶    verification commitment,   ───▶   markdown; 14-section
 prior art, naming                  assumptions, OQ triage;            template;
 candidates)                        pause for operator input)          paired journal)
```

The pause at Phase 2 is load-bearing: it is the operator's primary input channel and the only place where artifact name, audience, and verification commitment are confirmed before Phase 3 commits to them. Skipping the pause biases Phase 3 toward operator-absent defaults.

### Where this design plugs in

`spec-design` is a mid-pipeline node: upstream of `spec-write`, downstream of `project-constitution`. Outputs are referenced by name from downstream feature specs (via the feature spec's `DESIGN_SPEC_PATH`) and from review verdicts (via the review's `SPEC_PATH`). The skill itself is not invoked by other skills; it is invoked by the operator.

## 5. Detailed Design

### 5.1 Phase 1 — Discovery

**Purpose.** Orient the spec author against landscape, constraints, conversation history, and prior art before any commitment to shape is made.

**Inputs.** `ARTIFACT_NAME` (or placeholder), `ARTIFACT_DESCRIPTION`, `LANDSCAPE_ROOT`, `PRIOR_CONVERSATION`, `TARGET_AUDIENCE`, `KNOWN_CONSTRAINTS`, `NON_GOALS`, `DOWNSTREAM_SPECS`.

**Outputs.** Discovery Report covering: **landscape orientation** (systems, repos, conventions, prior art the design intersects with); **constraint orientation** (environmental, organizational, regulatory, tooling — these map into NFRs later); **conversation grounding** (summary of what prior conversation established and left open); **prior-art scan** (similar architectures in org, ecosystem, published references); **naming candidates** (if `ARTIFACT_NAME` is a placeholder).

**Behavior.** Mostly read-only. Conversation is authoritative input — the skill does *not* restart an interview already conducted. The Discovery Report does **not** include test-infrastructure or touch-surface subsections; those belong in a downstream feature spec.

**Pattern invoked.** *Discovery-before-Clarify*, the same Phase 1 shape used by `spec-write` and `project-constitution`. The skill is consistent with sibling skills' shape.

**Why this design.** Architecture work emerges from extended discussion; the skill's job is to capture and structure conclusions already reached, not to re-derive them. Discovery-before-Clarify also resists confabulation: the report names what is *known* before Phase 2 asks what is *open*.

**Alternatives considered.** Skipping Discovery (operator-supplied INPUTS alone). Rejected: the recommendations doc ([§2 Phase 1](../../docs/spec-design-recommendations.md#L33)) explicitly named "landscape orientation," "constraint orientation," and "conversation grounding" as load-bearing; omitting any one of them invites confabulation in Phase 3.

### 5.2 Phase 2 — Clarify (pause point)

**Purpose.** Resolve what Discovery cannot determine and pause for explicit operator input before producing any design spec.

**Inputs.** Discovery Report (from Phase 1); operator's responses to confirmations and open questions.

**Outputs.** Confirmed `ARTIFACT_NAME` (or selection from naming candidates), confirmed `TARGET_AUDIENCE` (broadest reader named), verification commitment (light or heavy), assumptions, triaged open questions, decisions proposed-unilaterally-on-non-response.

**Behavior.** Items presented in declared order: naming confirmation, audience confirmation, verification commitment, assumptions, open questions (triaged `[blocker]` / `[important]` / `[minor]`), proposed-unilateral decisions. The skill **stops and waits**. Blockers must be resolved before Phase 3.

**Pattern invoked.** *Triage-by-severity* — identical to `spec-write` and `project-constitution` Phase 2.

**Why this design.** Naming, audience, and verification commitment shape every section of the Phase 3 spec. Confirming them explicitly (rather than inferring) prevents downstream sections from being authored against incorrect assumptions, and creates a record the operator can challenge before Phase 3 commits.

**Alternatives considered.** Continuous Clarify (skill proceeds to Phase 3 as soon as it has enough). Rejected — the pause-and-wait discipline is the difference between collaborative authorship and skill-driven monologue.

### 5.3 Phase 3 — Spec Document

**Purpose.** Produce the design-spec artifact and its paired journal.

**Inputs.** Discovery Report + Clarify outputs.

**Outputs.** Two files: `specs/YYYYMMDD-<artifact-name>/architecture.md` (the design spec) + `specs/YYYYMMDD-<artifact-name>/journal.md` (the paired journal).

**Behavior.** The design spec conforms to the **14-section template** (§5.8). Sections §1-§6 share structure with feature specs. Sections §7 (Implementation Sequencing, not Task Breakdown), §8 (Validation Approach, not Test Strategy), and §11 (Adoption Path, not Rollout and Rollback) are *design-spec-specific* — they deliberately differ from `spec-write`'s equivalents. The journal records session decisions and is structured for mining by future sessions when patterns recur.

**Pattern invoked.** *Template-driven scaffolding* with operator-supplied content. The skill ships the template inline in its SKILL.md (per the Atomic-Skill Portability Principle: bundled defaults rather than host-repo lookup).

**Why this design.** Inline templates remove host-repo dependency and make the skill's contract auditable from SKILL.md alone. The §7/§8/§11 distinctions are the load-bearing differences between design specs and feature specs: collapsing them produces specs that don't fit either shape.

**Alternatives considered.** Reuse the `spec-write` template wholesale. Rejected (the recommendations doc [§1 "The core distinction"](../../docs/spec-design-recommendations.md#L13) is explicit: a feature-spec template forced onto a design-spec artifact produces contorted §7/§8/§11 sections). Single-file output (no journal). Rejected (the journal carries N+1 mining inputs that cannot be reconstructed from architecture.md alone).

### 5.4 Open Question shape

**Purpose.** Capture unresolved-by-design analysis at first-class detail — the property that distinguishes design specs from premature decisions.

**Interface.**

```markdown
### OQ-N — <Short title>

**Question.** <One or two sentences.>

**Analysis.** <Options analysis. Tables where helpful. Cross-references to other OQs.>

**Leaning.** <Current recommended direction with reasoning, or "no leaning" stated explicitly.>

**Owner.** <Who carries this forward; which downstream phase.>

**Watch items** (optional). <External signals or conditions for revisiting.>

**Anti-goals** (optional). <What NOT to do, with one-line rationale.>
```

**Behavior.** Each element does specific work. Question / Analysis separation lets a skim-reader hit the question; a deep reader gets the analysis. Leaning *with reasoning* (not just a verdict) lets future readers disagree from a position of context. Owner kills the "open forever" failure mode. Watch items capture revisit conditions. Anti-goals capture rejected approaches so the next person tempted by a discarded option finds the reasoning, not just the prohibition.

**Pattern invoked.** Open-question pattern from [docs/spec-design-recommendations.md §3](../../docs/spec-design-recommendations.md#L49) — verified canonical for this skill.

**Why this design.** Premature decisions in design specs become baked-in errors hard to revisit. The OQ shape forces analysis to be *capturable* and *revisitable*, which is what distinguishes a design spec from a snapshot of opinion.

**Alternatives considered.** Simple bullet list of open questions. Rejected — too thin to carry analysis; future readers cannot reconstruct the thinking.

### 5.5 Verification discipline

**Purpose.** Prevent confabulation in design-spec output. LLMs are too deferential to claims made top-of-mind in conversation.

**Behavior.** Operator chooses *light* or *heavy* verification in Phase 2. Light verification cites repo-internal sources at point of claim; no external WebFetch required. Heavy verification adds external-claim discovery + WebFetch/WebSearch verification + inline citation with verification date. Soft hedges (`[needs verification]`, `[unclear]`, `[TBD]`) are **prohibited** in published spec content; the skill either verifies the claim, omits it, or walls it off in a clearly labeled "empirical / undocumented" subsection.

**Pattern invoked.** Anti-confabulation discipline from [docs/spec-design-recommendations.md §5](../../docs/spec-design-recommendations.md#L113).

**Why this design.** LLM readers skip soft-hedge tags; future humans treat them as noise. Walling-off is honest about uncertainty; verify-or-omit is honest about confidence. Soft hedges are the dishonest middle ground.

**Alternatives considered.** Soft hedges (rejected — readers skip them). Heavy verification always (rejected — overkill for repo-internal-only specs).

### 5.6 Voice discipline

**Purpose.** Keep design-spec prose readable and unambiguous across multiple authors and sessions.

**Behavior.**
- Imperative for protocol rules ("the toolset must…").
- First-person plural for design intent ("we chose…").
- Plain declarative for observations.
- No marketing language ("elegant," "robust," "scalable"). Describe the property concretely or omit it.

**Pattern invoked.** Voice rules from [docs/spec-design-recommendations.md §6 "Voice"](../../docs/spec-design-recommendations.md#L166).

**Why this design.** Mixed voice produces ambiguity about whether a sentence is a rule, an intent, or an observation. Marketing language conveys no information and crowds out concrete prose.

### 5.7 Portability rule for links

**Purpose.** Ensure spec text survives repository moves, mirrors, and machine-relative path mishaps.

**Behavior.** Committed prose must not contain absolute filesystem paths or machine-specific paths. Preferred forms, in priority order: (1) published URL, (2) repo-relative path, (3) sibling-relative description, (4) bare name + host description.

**Pattern invoked.** Linking conventions from [docs/spec-design-recommendations.md §6 "Linking conventions"](../../docs/spec-design-recommendations.md#L155).

**Why this design.** Absolute paths and machine-specific paths cannot travel. The N=1 amendment 2026-05-17-1 demonstrated the cost of violating this rule (broken `.claude/skills/...` references in the spec required a five-edit policy amendment).

### 5.8 Section template

**Purpose.** Standardize design-spec output across sessions.

**Interface.** Exactly fourteen sections, exact headings, declared order:

```markdown
# <ARTIFACT_NAME> — Architecture and Protocol Specification

> Status: Draft — Open for Review
> Date: <YYYY-MM-DD>
> Author: <name>
> Audience: <named groups>

## 1. Overview
## 2. Goals and Non-goals
## 3. Background and Constraints
## 4. Architecture
## 5. Detailed Design
## 6. Non-functional Requirements
## 7. Implementation Sequencing
## 8. Validation Approach
## 9. Review Checkpoints
## 10. Risks and Mitigations
## 11. Adoption Path
## 12. Out of Scope
## 13. Open Questions
## 14. References
```

**Behavior.** Sections §1-§6 share structure with feature specs. Sections §7, §8, §11 are design-spec-specific and **deliberately differ** from feature-spec equivalents: §7 sequences phases (artifacts produced, not atomic tasks); §8 validates by review/dogfooding/example exercise (not test strategy); §11 describes adoption (not rollout). The Status banner uses the lifecycle `Draft — Open for Review` → `Approved` → `Superseded`.

**Pattern invoked.** Section template from [docs/spec-design-recommendations.md §4](../../docs/spec-design-recommendations.md#L79) — verified canonical for this skill.

**Why this design.** A feature-spec template forced onto an architectural commitment produces contorted sections (atomic tasks fabricated for code that won't be written; test strategy for prose-only outputs; rollout plans for architectures that adopt). Distinct §7/§8/§11 prevent this.

**Alternatives considered.** Reuse `spec-write`'s template. Rejected (see §5.3 rationale). Fewer sections (e.g., merge §7+§8 or §11+§12). Rejected — each section answers a distinct review-time question; merging trades reviewability for brevity.

## 6. Non-functional Requirements

| NFR | Requirement | Source |
|---|---|---|
| **Portability** | Skill functions when installed at `.agents/skills/spec-design/` and works against unrelated host repos that lack methodology-specific siblings (degrades cleanly). No runtime dependency on host-repo files for the skill itself to function. Section template and workflow bundled in SKILL.md. | [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) |
| **Conciseness** | Design specs are LLM-consumed. The skill's "no marketing language" rule and the self-contained discipline propagate the [AI context window limit](../tech-stack.md#L44) constraint. No explicit line caps — design-spec content scales with the artifact under design — but unnecessary length is prohibited at the per-sentence level (marketing words, vacuous hedges). | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/spec-design/SKILL.md) |
| **Citation discipline** | External claims verified at canonical sources (heavy verification) or limited to repo-internal sources (light verification). Soft hedges (`[needs verification]`, `[unclear]`, `[TBD]`) prohibited in published spec content. | [SKILL.md Notes on the standing disciplines](../../.agents/skills/spec-design/SKILL.md) |
| **Inline citation preference** | Authoritative URLs and source references appear inline at the point of claim. §14 References is reserved for cross-cutting sources too broad to inline; it is not a bibliography catch-all. | [SKILL.md WHAT NOT TO DO + §14 References preamble](../../.agents/skills/spec-design/SKILL.md) |
| **Self-containment** | Each produced design spec opens with Status / Date / Author / Audience banner and reads independently of the originating chat. No "as we discussed"; every named system, role, or pattern defined or linked. | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/spec-design/SKILL.md) |
| **Voice fidelity** | Imperative / first-person-plural / declarative discipline per §5.6. No marketing language. | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/spec-design/SKILL.md) |
| **Format fidelity** | Output conforms to the 14-section template with exact headings and declared order. §7/§8/§11 use design-spec form, not feature-spec form. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-design/SKILL.md) |
| **Markdown hygiene** | All code blocks specify a language. Tables and lists conform to GitHub-flavored markdown rendering conventions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-design/SKILL.md) |
| **Pairing** | Every design spec is accompanied by a journal at the same directory. The pair is the output; architecture.md alone is incomplete. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-design/SKILL.md) |
| **Pause discipline** | Phase 2 stops and waits for operator input on naming, audience, verification commitment, and OQ triage. No Phase-3 work without explicit operator clearance. | [SKILL.md PHASE 2 — CLARIFY](../../.agents/skills/spec-design/SKILL.md) |
| **Multi-repo awareness** | If the design spec lives in a different repo than the codebase it describes, §3 Background notes this and the spec includes `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` for downstream `spec-execute` sessions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-design/SKILL.md) |

## 7. Implementation Sequencing

The skill ships. This spec is descriptive, not implementation-planning. The sequencing for *adopting this spec* is:

1. **Spec authored** (this document and its sibling journal, committed together as a paired commit).
2. **CP-1 review** (§9): retroactive spec faithfully describes the shipping skill. Triggered by a fresh `/spec-review` session against this spec's CP-1 — operator chose the sequenced path (per N=1 precedent), deferring CP-1 to a separate session for cleaner authorship-vs-review separation.
3. **CP-2 drift audit** (§9): batched with the other four legacy-quintet specs per [docs/retroactive-spec-strategy.md §"Drift mitigation"](../../docs/retroactive-spec-strategy.md). Any divergence routes to `/spec-amend` (not silent edit).
4. **Adoption complete**: this spec becomes the reviewable contract for future SKILL.md changes; subsequent SKILL.md edits follow the Amendment Protocol via `/spec-amend`.

There is no Phase B "build the skill" — the skill is already built. There is no downstream feature spec; this spec is terminal for the skill's specification, except for any amendment-driven follow-up.

> Note: This section deliberately differs from a feature spec's Task Breakdown. Design specs do not decompose into atomic dev tasks; retroactive design specs particularly do not, because the implementation predates them.

## 8. Validation Approach

| Approach | What it validates |
|---|---|
| **Stakeholder review** | Eric (operator) reviews this spec for fidelity to intent. CP-1 is the gate. |
| **Drift audit** | Mechanical comparison of SKILL.md commitments to this spec's commitments. CP-2 is the gate. Output is a divergence list (possibly empty). Batched with the four sibling quintet specs. |
| **Dogfooding (recursive case)** | This very spec was authored using the `spec-design` skill being spec'd. Continued usefulness in authoring this and N=1 ([specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md)) is ongoing validation. |
| **Predecessor cross-check** | [docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md) is the skill's design-rationale source. Every spec-side commitment in §5 of this document traces back to a recommendation in that doc; gaps between the two are CP-2 inputs. |
| **Downstream consumption** | Successful design specs authored by the skill — [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md), [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md), [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md), the N=1 retroactive spec — are evidence that the section template and three-phase workflow produce specs that downstream skills can consume. |

> Note: This section deliberately differs from a feature spec's Test Strategy. Design specs are validated by review, audit, dogfooding, predecessor cross-check, and downstream consumption — not by automated test coverage.

## 9. Review Checkpoints

### CP-1 — Retroactive spec faithfully describes the shipping skill

**Status:** pass with comments on 2026-05-18 by Claude (agent reviewer). 0 blockers, 0 important, 4 advisory. See [journal.md "Review of CP-1"](./journal.md) for findings and recommendation. Checkpoint closed.

**Trigger.** This spec and its journal are committed; the operator invokes `/spec-review` against this spec's CP-1 in a fresh session.

**Review focus.**
- Every commitment in §4, §5, and §6 corresponds to behavior actually present in [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md).
- No commitment in this spec contradicts the shipping SKILL.md.
- The **Atomic-Skill Portability Principle** is correctly characterized as a binding constraint (§3, §6) consistent with [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33).
- The **format-question-prompt gap** (§13 OQ-1) is named with full analysis, not silently resolved or omitted.
- The recommendations doc is correctly distinguished as **authoritative for design rationale, not authoritative for current behavior** (§3 Background).
- The spec is self-contained per the Operating Principles in [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md).
- The portability rule for links is honored: no `~/.claude/skills/...` references, no absolute filesystem paths.

**Exit criteria.**
- Reviewer issues a verdict of `pass`, `pass with comments`, or `changes requested` per the structured format declared in [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- All `[blocker]` findings (if any) are resolved or escalated to `/spec-amend`.
- Verdict is written back to this spec's §9 (status line) and to the journal.

### CP-2 — Drift audit complete (batched)

**Status:** pass with comments on 2026-05-18 by Claude (agent reviewer). 0 blockers, 0 important, 5 advisory. See [journal.md "Review of CP-2"](./journal.md) for findings and routing, and [journal.md "CP-2 closeout"](./journal.md) for the closing summary. Checkpoint closed via amendments 2026-05-18-1 (D-1), 2026-05-18-2 (D-2), 2026-05-18-3 (D-3), and 2026-05-18-4 (D-4); D-5 accepted as known minor.

**Trigger.** CP-1 of this spec passes, AND CP-1 of the four sibling quintet specs (`spec-write`, `spec-execute`, `spec-review`, `spec-amend`) passes, AND project-constitution's CP-2 has either run or been folded into the batch per [docs/retroactive-spec-strategy.md OQ-1](../../docs/retroactive-spec-strategy.md).

**Review focus.** A line-by-line audit of [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) against this spec's §4, §5, §6, and §12. The auditor enumerates each divergence: a behavior present in SKILL.md but not committed in the spec, or a commitment in the spec absent from SKILL.md. Cross-skill drift patterns (e.g., four skills citing the Atomic-Skill Portability Principle correctly and one quietly not) are explicitly in scope by virtue of the batch context.

**Exit criteria.**
- Divergence list produced (possibly empty).
- For each divergence, a routing decision: (a) amend the spec to reflect SKILL.md behavior, (b) amend SKILL.md to match the spec, or (c) accept as a known minor discrepancy with rationale recorded in the journal.
- No silent edits to either artifact.
- Outcome recorded in this spec's journal as the closing entry of the retroactive-spec adoption.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Spec drifts from shipping SKILL.md silently after adoption (the methodology-wide failure mode this spec exists to prevent for `spec-design`). | Medium over time | High — the spec stops being trustworthy as a contract | Amendment Protocol via `/spec-amend`; CP-2 establishes the initial baseline; subsequent SKILL.md changes are spec-amended in the same change | Eric / future maintainers |
| Spec is wrong (misdescribes the skill, omits material behavior, asserts commitments the skill does not honor). | Low–Medium | Medium — early review catches it | CP-1 review with explicit "faithfulness to shipping SKILL.md" as the central focus area | Reviewer (CP-1) |
| Format-question-prompt gap (§13 OQ-1) resolves silently outside the methodology — a `spec-design` invocation that should have been `spec-write` (or vice versa) is corrected by operator judgment with no record. | Medium | Low–Medium — friction accumulates; signal of when a meta-entrypoint is justified is lost | OQ-1's Watch items capture revisit conditions; future amendment can promote the gap to action when watch conditions trigger | Eric (per-session); future amendment session |
| The recommendations doc is treated as authoritative for current behavior rather than for design rationale, causing CP-2 false positives. | Low–Medium | Low | §3 Background explicitly distinguishes the two; CP-2 auditor reads §3 before walking divergences | CP-2 auditor |
| N=2 patterns over-codify, foreclosing better N=3+ patterns at the sibling quintet specs. | Low | Medium | §2 Non-goals explicitly disclaims template-establishing intent; this spec's journal records "Pattern for N=3" callouts that are *candidates*, not declarations; the strategy doc's N=2-inflection-point decision is unaffected by this spec | Future retroactive-spec sessions; operator at session 2 close |
| Recursive-authorship of `spec-design` by itself induces self-referential blind spots (the spec validates the skill by exercising the skill — circular). | Low | Low | §3 Background explicitly names the recursion as an observation, not as an architectural property; §8 Validation Approach includes predecessor cross-check (recommendations doc) as a non-recursive validation channel | CP-1 reviewer |

## 11. Adoption Path

The spec is adopted in three steps, matching N=1:

1. **Commit the spec and journal** as a paired commit. The journal is the durable record of this session's decisions and observations, structured to be mined by sessions 2-5 of the legacy quintet.
2. **CP-1 review** in a fresh session (sequenced, per operator's choice): invoke `/spec-review` against this spec's CP-1.
3. **CP-2 drift audit** in the batched quintet-CP-2 session: per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md).

After adoption, the spec is a *living contract*. Edits to SKILL.md follow the Amendment Protocol: stated section being amended, before/after diff, reason and impact, explicit operator approval, journal entry. Per the [N=1 amendment 2026-05-17-1 N=2 mining note](../20260517-project-constitution-skill/journal.md), amendments that touch a *class* of references (paths, citations, vocabulary) must scan the entire spec at Phase 1 Orient, not just the locations called out by the triggering finding.

### Reversibility

The spec can be retired without affecting the skill. The SKILL.md remains the canonical implementation. Retirement would be unusual (it would be the methodology rejecting its own dogfooding convention for `spec-design` specifically) but mechanically clean: delete the spec directory and record the retirement in `roadmap.md`.

### Cross-session knowledge transfer

This spec's [journal.md](./journal.md) records validation/refinement/rejection outcomes for each "Pattern for N=2" callout from [the N=1 journal](../20260517-project-constitution-skill/journal.md). Patterns that recur at N=2 are candidates for promotion to `docs/retroactive-spec-pattern.md` at the N=2 inflection point — the decision is made at session 2's close (`spec-write` retroactive spec), not here.

## 12. Out of Scope

- **Resolving §13 OQ-1 (format-question-prompt gap).** Naming the gap is not resolving it; resolution routes to a future amendment session or to a meta-entrypoint design (see OQ-1 Analysis).
- **Verification-commitment escalation criteria.** When *light* is sufficient and when *heavy* is required is operator judgment. The N=1 pattern noted light suffices for the legacy quintet; future skills citing external systems or RFCs must escalate. The criteria are not formalized; they are not declared here either.
- **Redesign of the `spec-design` skill.** Redesign routes to a new design spec under amendment governance.
- **Modification of the shipping SKILL.md.** Only `/spec-amend` touches SKILL.md.
- **A template for the four sibling legacy-quintet retroactive specs** (`spec-write`, `spec-execute`, `spec-review`, `spec-amend`). The N=2 inflection point and `docs/retroactive-spec-pattern.md` decision are governed by [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md), not by declarations in this spec.
- **Multi-repo `SPEC_REPO_ROOT` mechanics under multi-target conditions** (one design spec spanning multiple downstream code repos; design-spec-repo lifecycle reflected in adopter repos). The single-repo case is the only case exercised in `ai-tools`. Multi-repo mechanics named in SKILL.md OUTPUT FORMAT are not bounded by this spec.
- **External-claim verification beyond repo-internal citation.** Light verification was adopted for this spec; the skill's heavy-verification path exists but is not exercised by this spec's text.
- **Constitution-amendment ceremony.** Inherited from [N=1 OQ-1](../20260517-project-constitution-skill/architecture.md#L274) and tracked at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md). Out of scope here.
- **Cross-skill amendment coordination.** A CP-2 finding that requires coordinated edits across multiple SKILL.md files is named as [docs/retroactive-spec-strategy.md OQ-3](../../docs/retroactive-spec-strategy.md). Out of scope here.

## 13. Open Questions

### OQ-1 — Format-question prompt is undocumented in SKILL.md

**Question.** The skill's predecessor recommendations doc explicitly named a Phase-2 format-question prompt — *"The very first question a design-spec skill should ask is: 'Does the standard template fit the artifact you want to produce, or should sections be reshaped?'"* ([docs/spec-design-recommendations.md §2 Phase 2, line 41](../../docs/spec-design-recommendations.md#L41)). This recommendation landed at the *structural* level via amendment 2026-05-18-2 (SKILL.md Phase 2 now enumerates a `Format confirmation` bullet that routes to `/spec-write` when the artifact is implementation work). The *content* of the prompt — specifically OQ-1 options (a)/(b)/(c)/(d) below — remains open. The N=1 journal's "Friction observed" called it out: *"Format choice was not in the skill's INPUTS. /spec-design took the format as a given (design spec) because the operator invoked it that way. A more general 'retroactive-spec' entrypoint would ask format choice as a Phase 2 question. Not actionable for this skill; flagged for future tooling consideration."* What should the skill do about format selection — and where does that question live?

**Analysis.** Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|---|---|---|
| (a) Add the format-question prompt to `spec-design`'s Phase 2 | New Phase-2 item: "Is the standard 14-section design-spec template the right shape for this artifact, or should sections be reshaped?" Skill admits non-default shapes (e.g., feature-spec-shaped, hybrid). | Closest to the predecessor recommendation. Risk: blurs the spec-design / spec-write boundary that the section template currently enforces. |
| (b) Add a meta-entrypoint skill (e.g., `/spec`) that routes between `spec-design` and `spec-write` | New skill at the front of the pipeline asks the format question; routes to the appropriate downstream skill. `spec-design` and `spec-write` both keep their current shapes. | Cleanest separation of concerns. Cost: a new skill to author, document, and maintain. |
| (c) Accept operator-judgment routing as the intended convention | Format is selected when the operator invokes `/spec-design` vs `/spec-write`. The skill is silent on routing; the friction observed at N=1 is the cost of explicit operator authority. | Zero infrastructure; documents existing practice. Risk: operators who lack the methodology context route by guess; format mismatch surfaces late (in Phase 3). |
| (d) Add a Phase-1 "format check" rather than a Phase-2 prompt | The Discovery Report flags candidate format mismatches based on landscape (e.g., "discovery suggests this is implementation work; consider `/spec-write` instead"). Operator confirms or overrides in Phase 2. | Lighter than (a) — flagging is cheaper than prompting. Risk: false positives erode trust in the flag. |

This OQ has structural dependencies on the four sibling quintet retroactive specs: a comparable analysis may surface for `spec-write` ("does the operator know they want a design spec, not a feature spec?"). If sibling specs introduce equivalent friction, option (b) becomes more attractive.

**Leaning.** No leaning declared at spec time. The four options are genuinely open; option (c) is the *de facto* status quo and is defensible, but the N=1 journal's "future tooling consideration" framing suggests it is not the *preferred* end state. Selection requires triaging (b)'s cost-vs-benefit against the friction (c) imposes. Out of scope for this spec.

**Owner.** A future `/spec-amend` or `/finding-intake` session, triggered by either of the watch items below.

**Watch items.**
- A second instance of format-mismatch friction occurs in a real session (signal: the gap creates recurring friction, urgency increases).
- The session-2 retroactive spec (`spec-write` at session 2 of the legacy quintet) surfaces an equivalent friction from the feature-spec side (signal: meta-entrypoint option (b) becomes more attractive).
- A third-party adopter of the `ai-tools` methodology asks how to pick between `spec-design` and `spec-write` (signal: the gap blocks external adoption).

**Anti-goals.**
- Do not silently resolve this OQ in any sibling quintet spec. Naming the gap is methodology-wide; resolving it must be deliberate.
- Do not pre-empt the N=2-inflection-point decision in [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) by linking this OQ's resolution to the `docs/retroactive-spec-pattern.md` decision. They are independent.

## 14. References

### Authoritative

- [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) — the shipping skill. Authoritative for behavior.
- [specs/tech-stack.md](../tech-stack.md) — methodology constraints, including the Atomic-Skill Portability Principle binding on this skill.
- [specs/mission.md](../mission.md) — `ai-tools` mission; defines audience and in/out of scope for the methodology.
- [specs/roadmap.md](../roadmap.md) — `ai-tools` roadmap; lists `spec-design` as a Phase 1 deliverable ([line 14](../roadmap.md#L14)).

### Inspirational

- [docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md) — the recommendations doc that became this skill. Extracted from a session that produced `ai-frontmatter-distributor-architecture.md` in an external private repo (`private-design-repo`). Authoritative for the skill's *design rationale*; not authoritative for current behavior.
- [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) — N=1 retroactive design spec; structural source for this N=2 instance.
- [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) — strategy doc for the legacy-quintet retroactive specs; orientation material for this session.
- [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md), [specs/20260517-finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) — sibling skills with feature specs (authored before the skills shipped); provided the `<skill-name>-skill` directory-slug convention.
- [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md), [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) — concurrent architectural work cited as negative-signal sources in this spec's journal.
