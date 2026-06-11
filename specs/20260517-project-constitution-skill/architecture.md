# `project-constitution` Skill — Architecture and Protocol Specification

> Status: Approved — CP-2 closed 2026-05-18
> Date: 2026-05-17
> Author: Eric Wasgatt (with AI assistance)
> Audience: Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set.

## 1. Overview

The `project-constitution` skill bootstraps a repository with a small, durable set of foundation documents — a *constitution* — that orients every contributor (human or AI) on what the repo is for, what it is built with, and where it is heading. It is the upstream input to every design and feature spec written under the `ai-tools` methodology.

This document is a **retroactive design specification**: the skill already ships at [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md), which is authoritative for the skill's behavior. The spec describes what the skill *is* and what it *commits to*, so the methodology can be reviewed, amended, and adopted on the same footing as the artifacts it produces. The spec does not redesign the skill; any divergence the spec exposes between its commitments and the shipping SKILL.md is routed to `/spec-amend`, not silently corrected.

## 2. Goals and Non-goals

### Goals

- Produce a self-contained, descriptive specification of the `project-constitution` skill's vocabulary, contract, and operating model.
- Declare review gates (§9) so the skill becomes reviewable via `/spec-review` against named checkpoints.
- Surface the **constitution-amendment workflow gap** as a first-class Open Question (§13 OQ-1) so the methodology does not silently accept "no ceremony" as the answer.
- Hold the skill to the **Atomic-Skill Portability Principle** declared in the constitution it bootstraps ([specs/tech-stack.md §21-33](../tech-stack.md#L21-L33)).
- Establish a paired-artifact pattern for retroactive skill specs — `architecture.md` + `journal.md` — that subsequent retroactive specs in the legacy trilogy may follow if patterns recur (decision deferred to N=2).

### Non-goals

- **Redesign of the skill.** The shipping SKILL.md is authoritative for behavior. This spec is descriptive.
- **A template for the four sibling legacy-trilogy retroactive specs** (`spec-design`, `spec-write`, `spec-execute`, `spec-review`, `spec-amend`). Each is authored in its own session per the operator's spec-by-spec cadence; cross-session scaffolding, if any, derives from N=2 observations recorded in this spec's journal, not from declarations in this spec body.
- **Defining the constitution-amendment ceremony.** That gap is named in §13 OQ-1 and tracked via the pending finding intake at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md). Resolving it is outside this spec's scope.
- **Producing governance documents** (CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, RACI). The skill explicitly excludes these from its outputs ([SKILL.md WHAT NOT TO DO](../../.agents/skills/project-constitution/SKILL.md)); the spec preserves that exclusion.
- **Specifying tooling, models, or platforms.** The skill produces markdown only and remains tooling-agnostic, consistent with the [mission.md Out of Scope](../mission.md) commitment.

## 3. Background and Constraints

### Prior state

Before the skill existed, repository orientation was reconstructed each time a contributor (human or AI) arrived at a repo. Stack, scope, and intent were inferred from scattered signals — READMEs of varying freshness, ambient knowledge in chat, commit archaeology. The skill was introduced in commit `80000b1` (2026-05-14) as part of the broader `spec-design` / `project-constitution` / `spec-amend` addition to the methodology. The constitution it produces for the `ai-tools` repo itself was added in commit `76e6bb6` (2026-05-15).

No standalone architecture document for this skill was ever authored. The architectural commitments — particularly the **Atomic-Skill Portability Principle** — were absorbed into [specs/tech-stack.md](../tech-stack.md) during the 2026-05-17 amendment cascade that resolved the `intake-template-folder-dependency` finding. This spec consolidates the commitments back into a dedicated, reviewable artifact.

### Constraints (cited)

- **Atomic-Skill Portability Principle** ([specs/tech-stack.md:21-33](../tech-stack.md#L21-L33)). Every skill, including this one, must be a portable atomic unit: self-contained workflow + schema knowledge + default templates; adapts to richer host-repo embodiments when present; degrades cleanly when they are absent. A skill installed globally must work against an unrelated repo.
- **AI context window limits** ([specs/tech-stack.md:44](../tech-stack.md#L44)). Methodology artifacts are LLM-consumed. Conciseness is a hard constraint on artifact length, not a style preference. This constraint propagates through the skill into its output sizing (mission ≤30 lines, tech-stack ≤60 lines, roadmap/validation ≤80 lines).
- **Spec-driven development convention** ([specs/tech-stack.md:51](../tech-stack.md#L51)). The methodology repo "eats its own cooking" — changes to the methodology follow the methodology. This convention is the explicit justification for this retroactive spec: a skill that bootstraps the spec-driven discipline must itself be spec-driven.
- **Repository layout convention** ([specs/tech-stack.md:48](../tech-stack.md#L48)). `specs/` for authoritative artifacts, `docs/` for supporting material. The skill enforces this convention on the host repos it runs against and surfaces deviations to the operator.

### Dependencies

- **Upstream**: none. The skill is the upstream-most artifact in the methodology; nothing precedes it.
- **Downstream** (per [SKILL.md HANDOFF NOTES](../../.agents/skills/project-constitution/SKILL.md)): `spec-design` references the constitution in its §3 Background; `spec-write` references the constitution to scope work to in-scope domains and to identify the stack the implementation must match.

## 4. Architecture

The skill's architecture is the **three-document constitution** with a **lifecycle-conditional third document**, executed via a **scan-before-prompt** three-phase workflow.

### Output topology

```text
specs/                          (or repo-specific authoritative-artifacts directory)
├── mission.md                  (always; "the why")
├── tech-stack.md               (always; "the how")
└── roadmap.md  | validation.md (exactly one; lifecycle-determined)
```

- `mission.md` — purpose, audience, in/out of scope, success criteria.
- `tech-stack.md` — languages, frameworks, tooling, hosting, constraints, conventions outside the stack.
- `roadmap.md` — for `new` or `growing` repos; forward-looking phases.
- `validation.md` — for `mature` or `inherited` repos; done criteria, acceptance signals, known gaps.

The third-document choice is binding for the lifecycle stage at constitution creation. A repo may graduate from roadmap to validation over time; that transition is an *amendment*, not a constitution rewrite. (See §13 OQ-1: the ceremony for that amendment is undefined.)

### Vocabulary (defined here, used consistently below)

- **Constitution** — the set of two or three coordinated markdown documents produced by the skill. Not a single file; the *set* is the constitution.
- **Lifecycle stage** — one of `new`, `growing`, `mature`, `inherited`. Selected by the operator in Phase 2; determines which third document is produced.
- **Scan** — the Phase 1 read-only inspection of the host repo. Produces a Scan Report consumed by Phase 2.
- **In-scope signal** — a host-repo file or convention the scan recognizes (manifest, framework marker, CI config, existing docs, git activity, `.agents/skills/` presence) and reports against.
- **Audience-named** — the convention that every produced document opens with an explicit Audience line naming the broadest reader.

### Execution model

```text
Phase 1: SCAN                Phase 2: CLARIFY              Phase 3: DOCUMENTS
(repo inspection,            (operator confirms             (produce 2 or 3
 no prompts)         ───▶     lifecycle, purpose,   ───▶    markdown files
                              audience, scope,               at specs/)
                              stack)
```

The scan-before-prompt ordering is load-bearing: most stack and structure signals are visible in the repo. Asking the operator for what the scan can determine wastes their attention and biases the constitution toward what the operator remembers rather than what the repo actually contains.

### Where this design plugs in

The skill is the upstream-most node in the methodology graph. Outputs are consumed by every downstream `spec-design` (§3 Background) and `spec-write` (scope + stack confirmation) session. The constitution is not regenerated per spec — it is read by downstream skills as ambient context.

## 5. Detailed Design

### 5.1 Phase 1 — Scan

**Purpose.** Inspect the host repo for orientation signals before any operator prompt.

**Inputs.** `REPO_ROOT` (path or repo URL).

**Outputs.** Scan Report covering: manifest files, framework markers, repository structure, CI configuration, existing docs, git signals (recent commits, branches, contributor density), and presence of `.agents/skills/` or similar AI-agent collaboration markers.

**Behavior.** Read-only. Categorical: each scan finding is tagged as confident (skill is sure) or needs-confirmation (skill flags for Phase 2). No operator prompts in this phase.

**Pattern invoked.** *Discovery-before-clarify*, the same Phase 1 pattern used by `spec-design` and `spec-write`. The skill is consistent with the sibling skills' shape.

**Why this design.** Operators repeatedly confirm what the repo already contains; pre-scanning collapses that loop. Operators also misremember stack details, especially in repos that have been inherited or have drifted from their original purpose; the scan grounds the conversation in repository ground truth.

**Alternatives considered.** A scan-on-demand model (skill asks first, then scans only when the operator's answers are ambiguous) was implicitly considered and rejected by the existing skill's design. The chosen always-scan model is cheaper for the operator and produces a better Phase 2 conversation.

### 5.2 Phase 2 — Clarify

**Purpose.** Resolve what the scan cannot determine and pause for operator input before producing any documents.

**Inputs.** Scan Report (from Phase 1); operator's responses to confirmations and open questions.

**Outputs.** Confirmed lifecycle stage, purpose statement, audience, in/out of scope, stack confirmations, captured assumptions, triaged open questions.

**Behavior.** Eight topics presented in order: lifecycle stage, purpose, audience, scope, layout, stack confirmation, assumptions, open questions. The skill **stops and waits** for operator input. Open questions are triaged `[blocker]` / `[important]` / `[minor]`; blockers must be resolved before Phase 3.

**Pattern invoked.** *Triage-by-severity*, identical to `spec-design` and `spec-write` Phase 2.

**Why this design.** The three documents are short; their content is determined more by operator intent than by repository signal. Phase 2 is the operator's primary input channel and must be explicit about what is being committed to.

### 5.3 Phase 3 — Documents

**Purpose.** Produce the two or three markdown documents constituting the constitution.

**Inputs.** Scan Report + Clarify outputs.

**Outputs.** Files at `specs/mission.md`, `specs/tech-stack.md`, and either `specs/roadmap.md` or `specs/validation.md`. If the host repo uses a non-default authoritative-artifacts directory, files are placed at the appropriate location and the exception is noted in the journal of the originating session.

**Behavior.** Each document follows the section template declared in [SKILL.md Phase 3](../../.agents/skills/project-constitution/SKILL.md) with the documented line caps. Documents are self-contained — a reader who has read only one still derives value. No marketing language; plain declarative prose; portability rule applies to all links (no absolute filesystem paths).

**Pattern invoked.** *Template-driven scaffolding* with operator-supplied content. The skill ships the templates inline in its SKILL.md (per the Atomic-Skill Portability Principle: bundled defaults rather than host-repo lookup).

**Why this design.** Inline templates remove host-repo dependency and make the skill's contract auditable from the SKILL.md alone. The line caps are concrete numerics that future drift can be measured against.

### 5.4 Layout-deviation handling

**Purpose.** Respect host repos that use a different authoritative-artifacts directory than `specs/`.

**Behavior.** When Phase 1 detects an existing directory structure that differs from the methodology's convention (e.g., `docs/` already contains specs, or a `specifications/` folder exists), the skill surfaces the layout question to the operator before Phase 3 file placement: "The methodology recommends `specs/` for authoritative artifacts and `docs/` for supporting material. This repo has `<detected layout>`. Should I use the methodology's convention, adapt to the existing layout, or ask you to decide per-file?" When the operator chooses a non-default layout, the exception is documented in the constitution itself (per [SKILL.md:78](../../.agents/skills/project-constitution/SKILL.md#L78)).

**Why this design.** Forcing methodology layout onto a repo with existing conventions creates immediate adoption friction. Surfacing the choice as an explicit question preserves operator authority and produces a self-explaining constitution.

### 5.5 Lifecycle-stage selection

**Purpose.** Determine whether the third document is `roadmap.md` (forward-looking phases) or `validation.md` (done criteria for the existing system).

**Behavior.** Four declared stages:

| Stage       | Third document  | Typical context                                                |
|-------------|-----------------|----------------------------------------------------------------|
| `new`       | `roadmap.md`    | Just created; no shipping product.                             |
| `growing`   | `roadmap.md`    | Active development; intent in flux.                            |
| `mature`    | `validation.md` | Stable, well-understood; "what good looks like" is the focus. |
| `inherited` | `validation.md` | Existing repo whose intent is being formalized for the first time. |

Exactly one third document is produced. Producing both is explicitly prohibited in [SKILL.md WHAT NOT TO DO](../../.agents/skills/project-constitution/SKILL.md).

**Why this design.** Forward-looking and assessment-of-state are different framings; mixing them in a single repo produces incoherence. The lifecycle-conditional choice forces the operator to declare the repo's current orientation.

## 6. Non-functional Requirements

| NFR                  | Requirement                                                                                                          | Source                                                                                  |
|----------------------|----------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| **Portability**      | Skill functions when installed at `.agents/skills/project-constitution/` and works against unrelated host repos that lack methodology-specific siblings (degrades cleanly). No runtime dependency on host-repo files for the skill itself to function. Schema and templates are bundled in SKILL.md. | [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) |
| **Conciseness**      | Produced documents respect line caps: mission ≤30, tech-stack ≤60, roadmap/validation ≤80. Soft caps; operator may override with rationale.                                            | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/project-constitution/SKILL.md) |
| **Citation discipline** | Stack claims are backed by an actual file in the repo. The skill must not assert a stack a manifest does not support. If `package.json` exists but the project is Python, the skill asks before writing `tech-stack.md`.                      | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/project-constitution/SKILL.md) |
| **Self-containment** | Each produced document opens with an Audience line and reads independently. No "as we discussed"; no implicit shared vocabulary.                                                                                                              | [SKILL.md OUTPUT FORMAT](../../.agents/skills/project-constitution/SKILL.md) |
| **No proliferation** | Three documents only. The skill explicitly excludes CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, governance docs.                                                                                                                                  | [SKILL.md WHAT NOT TO DO](../../.agents/skills/project-constitution/SKILL.md) |
| **Forward orientation** | The skill does not narrate the repo's history unless the operator supplies it. The constitution is forward-orienting, not biographical.                                                                                                     | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/project-constitution/SKILL.md) |
| **Layout neutrality** | The skill detects and respects non-default authoritative-artifacts directories. Layout choice is surfaced as a Phase 2 question, not assumed.                                                                                                | [SKILL.md Phase 3 preface](../../.agents/skills/project-constitution/SKILL.md#L78) |
| **README reconciliation** | The skill does not duplicate or contradict existing READMEs. When a relevant README is present, the operator is asked whether to update it as part of the constitution work or to note that the constitution supersedes it. Silent duplication is prohibited. | [SKILL.md WHAT NOT TO DO](../../.agents/skills/project-constitution/SKILL.md) |

## 7. Implementation Sequencing

The skill ships. This spec is descriptive, not implementation-planning. The sequencing for *adopting this spec* is:

1. **Spec authored** (this document and its sibling journal, committed together).
2. **CP-1 review** (§9): retroactive spec faithfully describes the shipping skill. Triggered by the original `/spec-review` request that began this work.
3. **CP-2 drift audit** (§9): line-by-line comparison of the shipping SKILL.md against this spec's commitments. Any divergence routes to `/spec-amend` (not silent edit).
4. **Adoption complete**: this spec becomes the reviewable contract for future SKILL.md changes; subsequent edits to SKILL.md follow the Amendment Protocol via `/spec-amend`.

There is no Phase B "build the skill" — the skill is already built. There is no downstream feature spec; this spec is terminal for the skill's specification, except for any amendment-driven follow-up.

> Note: This section deliberately differs from a feature spec's Task Breakdown. Design specs do not decompose into atomic dev tasks; retroactive design specs particularly do not, because the implementation predates them.

## 8. Validation Approach

| Approach                | What it validates                                                                                                                  |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| **Stakeholder review**  | Eric (operator) reviews this spec for fidelity to intent. CP-1 is the gate.                                                       |
| **Drift audit**         | Mechanical comparison of SKILL.md commitments to this spec's commitments. CP-2 is the gate. Output is a divergence list (possibly empty). |
| **Dogfooding**          | The `ai-tools` repo's own constitution ([specs/mission.md](../mission.md), [specs/tech-stack.md](../tech-stack.md), [specs/roadmap.md](../roadmap.md)) was produced by this skill; its continued usefulness is ongoing validation. |
| **Downstream consumption** | The `spec-design` and `spec-write` skills cite the constitution in produced specs. Successful downstream specs (e.g. [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md), [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)) are evidence that the constitution provides workable upstream input. |

> Note: This section deliberately differs from a feature spec's Test Strategy. Design specs are validated by review, audit, and dogfooding, not by automated test coverage.

## 9. Review Checkpoints

### CP-1 — Retroactive spec faithfully describes the shipping skill

**Status:** pass with comments on 2026-05-17 by Claude (agent reviewer). 0 blockers, 1 important (broken §1/§14 link target — proposed amendment), 3 advisory. Recommendation: proceed to CP-2 after /spec-amend addresses the link target. Full verdict in [journal.md](./journal.md#2026-05-17--review-of-cp-1).

**Trigger.** This spec and its journal are committed; the original `/spec-review` request that initiated this work resumes.

**Review focus.**
- Every commitment in this spec corresponds to behavior actually present in [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md).
- No commitment in this spec contradicts the shipping SKILL.md.
- The Atomic-Skill Portability Principle is correctly characterized as a binding constraint (§3, §6) consistent with [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33).
- The constitution-amendment gap (§13 OQ-1) is named, not silently resolved or omitted.
- The spec is self-contained per the Operating Principles in [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md).

**Exit criteria.**
- Reviewer issues a verdict of `pass` or `pass with comments` per the structured format declared in the `spec-review` skill.
- All `[blocker]` findings (if any) are resolved or escalated to `/spec-amend`.
- Verdict is written back to this spec's §9 (status line) and to the journal.

### CP-2 — Drift audit complete

**Status:** pass with comments on 2026-05-18 by Claude (agent reviewer); routing closed 2026-05-18. 0 blockers, 0 important, 4 advisory — D-1 resolved via amendment 2026-05-18-2 (commit 974b882); D-2 resolved via amendment 2026-05-18-1 (commit 6626756, option b — SKILL.md amended); D-3 accepted as known minor (rationale in journal); D-4 resolved via amendment 2026-05-18-3 (commit 7eda915). Retroactive-spec adoption (§11) **closed**. Full verdict in [journal.md](./journal.md#2026-05-18--review-of-cp-2).

**Trigger.** CP-1 passes.

**Review focus.** A line-by-line audit of [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) against this spec's §4, §5, §6, and §12. The auditor enumerates each divergence: a behavior present in SKILL.md but not committed in the spec, or a commitment in the spec absent from SKILL.md.

**Exit criteria.**
- Divergence list produced (possibly empty).
- For each divergence, a routing decision: (a) amend the spec via `/spec-amend` to reflect the SKILL.md behavior, or (b) amend the SKILL.md via `/spec-amend` to match the spec, or (c) accept as a known minor discrepancy with rationale recorded in the journal.
- No silent edits to either artifact.
- Outcome recorded in the journal as the closing entry of the retroactive-spec adoption.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|------|-----------|--------|------------|-------|
| Spec drifts from shipping SKILL.md silently after adoption (the methodology-wide failure mode this spec exists to prevent for project-constitution). | Medium over time | High — the spec stops being trustworthy as a contract | Amendment Protocol via `/spec-amend`; CP-2 establishes the initial baseline; subsequent SKILL.md changes are spec-amended in the same change | Eric / future maintainers |
| The spec is wrong (misdescribes the skill, omits material behavior, asserts commitments the skill does not honor). | Low–Medium | Medium — early review catches it | CP-1 review with explicit "faithfulness to shipping SKILL.md" as the central focus area | Reviewer (CP-1) |
| Constitution-amendment ceremony (§13 OQ-1) remains undefined indefinitely; informal pattern silently becomes the norm. | Medium | Medium — methodology gap accumulates work | OQ-1 names the gap as a tracked finding-prep; intake routes to triage in a future session | Future `/finding-intake` session |
| Retroactive-spec investment does not pay off because the four sibling skills are never spec'd (cost of this work amortized over only one skill). | Low | Low — this spec stands on its own value | Operator's stated cadence is spec-by-spec across sessions; journal of this spec is structured for N=2 mining (see §11 Adoption Path) | Operator (per-session decision) |
| The pattern declared here unintentionally forecloses better patterns for the four sibling specs by becoming a default template before patterns are validated. | Low | Medium | §2 explicitly disclaims template-establishing intent; patterns surface from N=2 observation, not declaration | Future retroactive-spec sessions |

## 11. Adoption Path

The spec is adopted in three steps:

1. **Commit the spec and journal** as a paired commit. The journal is the durable record of this session's decisions and observations, structured to be mined by future retroactive-spec sessions (N=2 onward) — see §11 cross-reference below.
2. **CP-1 review**: invoke `/spec-review` against this spec's CP-1. The original `/spec-review the project-constitution skill` request that initiated this work is the trigger.
3. **CP-2 drift audit**: after CP-1 passes, conduct the line-by-line audit declared in §9 CP-2. Any divergence routes to `/spec-amend`.

After adoption, the spec is a *living contract*. Edits to the SKILL.md follow the Amendment Protocol: stated section being amended, before/after diff, reason and impact, explicit operator approval, journal entry under [SKILL.md](../../.agents/skills/project-constitution/SKILL.md) (or, pending OQ-1 resolution, this spec's journal as a fallback).

### Reversibility

The spec can be retired without affecting the skill. The SKILL.md remains the canonical implementation. Retirement would be unusual (it would be the methodology rejecting its own dogfooding convention) but mechanically clean: delete the spec directory, record the retirement in `roadmap.md` or wherever methodology decisions are journaled.

### Cross-session knowledge transfer

This spec's [journal.md](./journal.md) is structured for mining by future retroactive-spec sessions. Specifically, the journal records: source-file selection rationale, naming-pattern choice, format choice (design spec vs. feature spec), open-question framing, drift-audit-as-checkpoint decision, and friction points encountered. A subsequent retroactive-spec session for `spec-design`, `spec-write`, `spec-execute`, `spec-review`, or `spec-amend` reads this journal as prior art rather than re-deriving the choices. Patterns that recur at N=2 may be promoted to a `docs/` notes file at that point — explicitly deferred from this session.

## 12. Out of Scope

- **Constitution-amendment workflow definition.** Deferred — tracked as §13 OQ-1 and via the pending finding-intake at [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md).
- **Redesign of the `project-constitution` skill.** This spec is descriptive; redesign would route to a new design spec under amendment governance.
- **Templates or scaffolding for the four sibling legacy-trilogy retroactive specs.** Deferred to N=2 observation, per operator instruction; emerging patterns will be journaled, not pre-declared in this spec.
- **Definition of governance documents** (CONTRIBUTING, CODE_OF_CONDUCT, SECURITY). The skill explicitly excludes these; the spec preserves the exclusion.
- **Multi-repo skill-spec layout** (e.g. spec lives in a different repo than the skill). Not applicable: this spec and the skill both live in `waseric/ai-tools`. `SPEC_REPO_ROOT` is unset.
- **External-claim verification beyond repo-internal citation.** The skill makes no claims about external systems, RFCs, or third-party tools that require WebFetch verification.

## 13. Open Questions

### OQ-1 — Constitution-amendment ceremony is undefined

**Question.** The `project-constitution` skill bootstraps constitution documents but does not address their amendment workflow. `/spec-amend`'s documented scope names design specs and feature specs as its targets, not constitution documents (`mission.md`, `tech-stack.md`, `roadmap.md`, `validation.md`). The constitution documents have no journal files. There is no `/constitution-amend` skill. What ceremony amends a constitution document, and where are those amendments journaled?

**Analysis.** The gap surfaced during the 2026-05-17 five-amendment cascade resolving the `intake-template-folder-dependency` finding, when an amendment to `specs/tech-stack.md` (committing the Atomic-Skill Portability Principle) was required. The cascade resolved the gap pragmatically: `/spec-amend` was used informally against `tech-stack.md`, and the amendment was journaled inside the originating finding's journal rather than at a constitution-level location. Four candidate resolutions, none yet selected:

| Option | Mechanism | Tradeoff |
|--------|-----------|----------|
| (a) Define a `/constitution-amend` skill | New skill, distinct from `/spec-amend`, with constitution-specific conventions (e.g., journal added to constitution; re-approval step for significant changes; explicit approver list). | Highest fidelity; highest implementation cost. |
| (b) Formally extend `/spec-amend`'s scope to include constitution docs | Update `/spec-amend` SKILL.md and any sibling references; treat constitution as a special case of "spec" with its own discipline notes. | Reuses existing skill; risks blurring the distinction between specs and constitution docs. |
| (c) Add journal files to the constitution | `specs/tech-stack-journal.md`, `specs/mission-journal.md`, `specs/roadmap-journal.md`; informal `/spec-amend` use remains the workflow. | Lightest infrastructure; preserves "the constitution is short" principle by keeping rationale in journals. |
| (d) Accept the informal pattern as the intended convention | Constitution amendments are journaled inside whatever finding or spec triggered them; the constitution doc has no journal by design. | Zero infrastructure; documents existing practice; risks invisibility of amendment intent over time. |

Full context, including the cascade history and pre-intake notes, is captured in [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md).

**Leaning.** No leaning declared at spec time. The four options are genuinely open; resolution requires the triage and investigation phases of the pending finding's pipeline. This spec's role is to *name* the gap formally so it cannot be silently accepted; selecting an option is a downstream concern.

**Owner.** A future session invoking `/finding-intake` against [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md), then `/finding-triage` once the finding lands. Triage and investigation phases select among the four options.

**Watch items.**
- A second informal amendment of a constitution doc occurs before the gap is triaged (signal: the gap is creating recurring friction, urgency increases).
- A third-party adopter of the `ai-tools` methodology asks how to amend their own constitution (signal: the gap blocks external adoption, urgency increases).
- The pending finding-intake is filed and routed (signal: gap is moving through the pipeline; this OQ can be amended to reference the finding's path).

**Anti-goals.**
- Do not silently merge constitution amendments without journaling them. Even option (d) (informal pattern as convention) requires a journal entry somewhere — never zero.
- Do not pre-empt the triage by resolving this OQ in this spec. The operator explicitly deferred resolution to the finding's pipeline.

## 14. References

### Authoritative

- [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — the shipping skill. Authoritative for behavior.
- [specs/tech-stack.md](../tech-stack.md) — methodology constraints, including the Atomic-Skill Portability Principle binding on this skill.
- [specs/mission.md](../mission.md) — `ai-tools` mission; defines audience and in/out of scope for the methodology.
- [specs/roadmap.md](../roadmap.md) — `ai-tools` roadmap; lists `project-constitution` as a Phase 1 deliverable ([line 13](../roadmap.md#L13)).

### Inspirational

- [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) — the skill that authored this spec; its OUTPUT FORMAT and OPERATING PRINCIPLES are the structural source.
- [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md) and [specs/20260517-finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) — sibling skills with their own specs; provided naming-pattern prior art (`<skill-name>-skill` directory suffix) and journal-structure prior art.
- [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md) — the pre-intake notes for the OQ-1 finding.
- [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) — concurrent architectural work that explicitly listed this skill as out-of-scope, confirming the absence of prior architectural specification.
