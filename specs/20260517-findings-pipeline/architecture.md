# Findings Pipeline — Architecture and Protocol Specification

> Status: Draft — Open for Review
> Date: 2026-05-17
> Author: waseric + Claude
> Audience: Eric Wasgatt (author); AI coding agents consuming the methodology's artifacts; engineers, hobbyists, and product owners evaluating or adopting lifecycle-aware development practices

## 1. Overview

The Findings Pipeline is a methodology artifact-family that captures, refines, and routes observations made **outside** a Review Checkpoint. Where [`spec-review`](../../.agents/skills/spec-review/SKILL.md) surfaces findings inside a checkpoint declared by a parent spec, the Findings Pipeline accepts anything noticed at any time — by anyone, from any signal — and routes it to a terminal outcome through three named, persona-distinguished phases: **intake**, **triage**, **investigation**.

The architectural commitment is fourfold: (1) a Markdown finding artifact at `specs/findings/YYYYMMDD-<short-name>/` mirroring the feature-spec convention; (2) a monotonic status state machine tracking pipeline phase; (3) a persona-aware role frame that orients each phase even when the operator is solo; (4) integration by named input into the existing [`spec-amend`](../../.agents/skills/spec-amend/SKILL.md) and [`spec-write`](../../.agents/skills/spec-write/SKILL.md) skills — no new downstream pipelines beyond the methodology's existing reach.

## 2. Goals and Non-goals

**Goals:**

- Close the gap named in [roadmap.md:38](../roadmap.md#L38): integration points between operational findings and spec amendments — a feedback loop from run back into build.
- Provide a low-friction landing point so an observation can be captured the moment it is noticed, including while the operator is actively working another spec.
- Define three named phases (intake / triage / investigation), each oriented toward a named persona, so the discipline survives a team expansion without rewriting and orients an AI agent's role even in solo work.
- Route every finding to a terminal outcome: amend an existing spec, author a new feature spec, defer with rationale, or close with rationale.
- Preserve methodology coherence: extend `spec-review`'s "finding" vocabulary; route to existing skills; do not introduce parallel pipelines.

**Non-goals:**

- Operational readiness methodology (run books, health checks, alerting conventions) — separate Phase 2 deliverable on the roadmap.
- Iteration methodology (backlog grooming, prioritization, technical debt management) — separate Phase 2 deliverable.
- Post-incident review as a standalone discipline — separate Phase 2 deliverable.
- Automated intake from external observability or ticketing systems (PagerDuty, Sentry, Linear, GitHub Issues). Intake **accepts external-system pointers** but the act of creating the finding artifact is human-initiated.
- Replacing `spec-review`'s in-checkpoint findings. Checkpoint findings continue to be raised under `spec-review`; the Findings Pipeline is the channel for everything else.
- Issue-tracker substitute. Findings Pipeline is a methodology artifact, not a backlog tool. It produces decisions and routes them; it does not host long-lived ticket queues.

## 3. Background and Constraints

### Prior art

- **Internal precedent.** `spec-review` already establishes a `[blocker] / [important] / [advisory]` severity taxonomy and a structured finding shape (file, line range, spec reference). This pipeline extends that vocabulary rather than introducing a parallel one. See [spec-review SKILL.md](../../.agents/skills/spec-review/SKILL.md).
- **Methodology framing.** The constitution declares the methodology "informed by ITSM/ITIL/SDLC" without binding to any single framework ([mission.md](../mission.md), [tech-stack.md](../tech-stack.md)). The Findings Pipeline draws on incident/problem management framing from ITIL and defect-lifecycle framing from SDLC traditions, but commits to neither as a compliance target.
- **Persona-as-orientation.** Software-engineering tradition typically separates roles (service desk, business analyst, developer, security analyst). In a solo-operator context, those roles collapse into one person — but the *role frame* still organizes the work. The pipeline treats personas as orientation, not handoffs.

### Current state

- No general-purpose intake channel exists for findings discovered outside a Review Checkpoint. Three partial mechanisms — `spec-review` findings (scoped to a checkpoint), `spec-amend` (assumes you already know what's wrong), per-feature journals (scoped to one feature) — each cover a narrow slice and none cover ambient operational/testing/security findings.
- The repo follows the spec-path convention established in [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md): authoritative artifacts under `specs/`, per-spec subdirectories with date-prefixed naming, artifact-type filenames within (`architecture.md`, `feature.md`, `journal.md`).

### Constraints

- **Prose-only.** No executable runtime; the pipeline is markdown artifacts and skills. [tech-stack.md](../tech-stack.md).
- **AI context-window economy.** A finding artifact must be short enough that an agent can load several without blowing context. Target: under ~200 lines per finding except where evidence requires more.
- **Interruption-tolerance.** A finding raised during active `spec-execute` must not pull the executor off-task. Intake must be cheap; downstream phases may be deferred without losing the finding.
- **Single `main` branch, no CI/CD.** No automated intake hook; transitions are journaled, not enforced by tooling.

## 4. Architecture

### Topology

```
[Signal sources]                                                  [Terminal outcomes]
  user report                                                       route → spec-amend
  github issue                                                      route → spec-write
  ops alert         ─────────────┐                                  defer (with rationale)
  test failure                   │                                  close (with rationale)
  journal note                   ▼                                          ▲
  intuition              ┌──────────────┐                                   │
  anywhere               │ finding-intake│ (skill)                          │
                         └──────┬───────┘                                   │
                                ▼                                           │
                       ┌────────────────┐                                   │
                       │finding artifact│ status: intake                    │
                       └────────┬───────┘                                   │
                                ▼                                           │
                         ┌──────────────┐                                   │
                         │finding-triage│ (skill)                           │
                         └──────┬───────┘                                   │
                                ▼                                           │
                       status: triaged                                      │
                                │                                           │
                                ▼                                           │
                       ┌─────────────────────────┐                          │
                       │investigation (protocol) │ ── may be skipped ──────►│
                       └──────────┬──────────────┘                          │
                                  ▼                                         │
                       status: under-investigation                          │
                                  │                                         │
                                  ▼                                         │
                              (route) ──────────────────────────────────────┘
                                                                            ▲
                      (any closed/routed finding may be reopened)           │
                      reopened ──→ triaged | under-investigation ───────────┘
```

### Vocabulary

- **Finding** — the canonical artifact. A markdown document recording an observation that may require action. Lives at `specs/findings/YYYYMMDD-<short-name>/finding.md` with a sibling `journal.md`.
- **Signal** — the originating input that motivates a finding. May be a user report, a GitHub issue URL, a journal note, a test failure, an observation in passing. Signals are external to the pipeline; intake transforms a signal into a finding.
- **Intake** — phase that creates the finding artifact from a signal. Captures what is known at the moment of noticing. May be very thin ("moderators report TAB User List is not showing prefixes under some circumstances") or richer if information is available.
- **Triage** — phase that confirms reproducibility, scope, severity, and domain. Produces "hard facts" about the shape of the finding: can it be reproduced, who is affected, what is the urgency, what kind of finding is this.
- **Investigation** — phase that examines code, configuration, or runtime state to identify probable cause and propose remedy. May be skipped when triage already makes the route obvious (e.g., an obvious documentation gap).
- **Route** — the terminal decision: one of `spec-amend`, `spec-write`, `defer`, or `close`. Every finding ends in a route or stays open with an explicit reason.
- **Persona** — the role frame oriented toward each phase. Personas are orientation, not handoffs: a solo operator plays all roles but plays each one *in the right frame* during the phase that calls for it.

### Composition rules

- A finding has exactly one current `status`, drawn from the state machine in §5.1.
- `status` is append-only and forward-progressing under normal flow through `intake → triaged → under-investigation → routed | closed`. Investigation may be skipped (status transitions `triaged → routed | closed` directly), with a journal entry recording the skip rationale. Reopening creates a new status entry that returns to an earlier phase, preserving prior status history in the journal.
- Reopening a closed finding requires a new status entry with rationale (`reopened`, transitioning back to an earlier phase); the prior closure is preserved in journal history.
- Every status transition is journaled with date, persona-frame, and rationale.
- A finding is self-contained: it does not depend on the originating conversation or external system context being still accessible.

## 5. Detailed Design

### 5.1 Finding artifact

**Purpose.** A single markdown document carrying the finding's identity, current state, and the evidence accumulated through each phase.

**Shape.** The artifact has a stable top section that grows as phases complete; later phases append without rewriting earlier ones.

```markdown
# <Short title> — Finding

> Status: <intake | triaged | under-investigation | routed | closed | reopened>
> Domain: <operational | testing | security | methodology | other>
> Severity: <blocker | important | advisory>           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis
> Date opened: <YYYY-MM-DD>
> Last transition: <YYYY-MM-DD>                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** <reporter; may be self, a user, an external system>
**Reported via:** <signal source: text, URL, system pointer>
**Captured by:** <whoever created the artifact; persona-frame: intake>
**Summary:** <one paragraph of what was noticed; what is known and what is not>
**External references:** <URLs or pointers; may be empty>

## Triage

**Triaged by:** <persona-frame: service desk | business analyst | developer>
**Date:** <YYYY-MM-DD>
**Reproducibility:** <reliably | intermittently | not reproduced | not applicable>
**Repro steps (if reproducible):**
1. ...
**Scope:** <who/what is affected>
**Domain confirmation:** <operational | testing | security | methodology | other>
**Severity confirmation:** <blocker | important | advisory>
**Notes:** <anything else surfaced in triage; rejected hypotheses; clarifications from reporter>

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Decision:** <spec-amend | spec-write | defer | close>
**Decided by:** <persona-frame and operator>
**Date:** <YYYY-MM-DD>
**Target spec (if amend or new-spec):** <path to spec; e.g., specs/20260517-findings-pipeline/architecture.md>
**Rationale:** <one paragraph; why this route over the others>
```

**Behavior.**
- The artifact is appended, not overwritten, as phases complete. Earlier sections are preserved; corrections to earlier sections are made via journal entry, not by editing in place.
- The `journal.md` mirrors the feature-spec journal pattern: one entry per status transition.
- The artifact never depends on its originating conversation or any external system being still reachable. Quoted text, pasted summaries, and embedded screenshots travel with the finding.

**Pattern invoked.** Phase-progress-with-immutable-history. Comparable to event-sourced records where current state is a projection over an append-only log.

**Why this design.** A single document per finding keeps the agent's context window small. Append-only sections preserve auditability without git-archaeology. Mirroring the feature-spec layout keeps the methodology's vocabulary and tooling reach consistent.

**Alternatives considered.**
- *One ticket per finding in an external tracker.* Rejected: introduces tooling the methodology declares out of scope, and tracker entries rot when the tool is replaced. Methodology artifacts must travel with the repo.
- *Inline findings into the relevant spec's journal.* Rejected: findings frequently cross spec boundaries or don't yet have a clear parent spec. A separate artifact preserves the "I noticed something" landing-point regardless of where it eventually routes.

### 5.2 Intake phase

**Purpose.** Convert a signal into a finding artifact with the lowest possible ceremony.

**Interface.** A `finding-intake` skill (downstream feature spec) accepts:
- A textual narrative ("moderators report TAB User List is not showing prefixes under some circumstances"), OR
- A pointer to an external system (GitHub issue URL, Slack thread, ticket ID) optionally with commentary, OR
- Both.

The skill produces a finding artifact at `specs/findings/YYYYMMDD-<short-name>/finding.md` plus `journal.md`, populating the Intake section and setting `status: intake`.

**Behavior.**
- Intake is interruption-tolerant. Total ceremony to land a finding from a stray observation during another session is target: under 60 seconds of operator effort.
- Intake does not require the operator to know the domain, severity, or route. Those are triage and investigation outputs.
- Intake captures whatever is known. Unknown fields are recorded as "unknown" rather than guessed.
- External system pointers are captured verbatim alongside any commentary; the artifact does not assume the external system remains reachable.

**Persona orientation.** Service desk, manager, end-user-via-intake, or **anyone**. The lowest barrier to entry. Eric, an AI agent acting on his behalf, or a future external adopter all use the same intake entrypoint.

**Pattern invoked.** Bottom-of-funnel capture, common to incident-management tooling. Optimizing for capture-rate over capture-quality is intentional: an imperfect intake is better than a forgotten observation.

### 5.3 Triage phase

**Purpose.** Shape the finding: confirm reproducibility, scope, severity, and domain. Decide whether investigation is needed before routing.

**Interface.** A `finding-triage` skill (downstream feature spec) takes a finding at `status: intake` and:
- Confirms or refines the summary against the original signal (or reporter, if reachable).
- Establishes repro steps where applicable; records intermittency or non-reproducibility honestly.
- Assigns or confirms domain (operational / testing / security / methodology / other) and severity (`blocker` / `important` / `advisory`).
- Optionally assigns operational urgency (P1–P4) for operational findings where urgency matters separately from methodology severity. The urgency field is decoupled from methodology severity: the two axes can diverge (an `advisory` finding may be P1-urgent; a `blocker` finding may be P4).
- Transitions status to `triaged` and journals the transition.
- Decides: does this require investigation, or is the route already clear?

**Behavior.**
- Triage produces *hard facts*, not hypotheses about cause. "What is the shape of this finding" — not "what is causing it."
- A triage may end with "cannot reproduce; closing as not-actionable" — a valid terminal route directly from triage.
- Triage may also end with "obvious enough to route directly" — skipping investigation, with the skip rationale journaled.

**Persona orientation.** Service desk (light), business analyst familiar with the domain (sweet spot), developer (overkill but acceptable in solo work). When the AI agent performs triage on the operator's behalf, it adopts the business-analyst frame: domain-knowledgeable, asking clarifying questions of the reporter or the system, not yet opening the codebase.

**Pattern invoked.** Reproduction-first defect triage from SDLC tradition.

### 5.4 Investigation phase (protocol)

**Purpose.** Examine code, configuration, or runtime state to identify probable cause and propose a remedy.

**Interface.** Investigation is a **protocol** carried inside the finding artifact, not (yet) a separate skill. The Investigation section of the finding (§5.1) captures: investigator, date, probable cause with evidence, code touchpoints, alternative hypotheses, proposed remedy.

**Behavior.**
- Investigation is performed by a developer-frame persona — the only phase that requires codebase access. Triage may surface hypotheses, but investigation is where hypotheses meet code.
- Investigation may end with: "cause identified, route to spec-amend with proposed remedy"; "cause identified, route to spec-write because remedy is large enough to need a feature spec"; "cause unclear, defer with rationale"; "cause identified but not worth fixing, close with rationale."
- Investigation may iterate: a first investigation may produce a partial answer and journal "needs deeper look"; status remains `under-investigation` until the route is chosen.

**Persona orientation.** Developer. The first phase that opens the codebase.

**Why protocol rather than skill (for now).** Solo-operator findings often have obvious causes; the lightweight form is faster to adopt and lower-overhead. Graduating to a `finding-investigate` skill is reserved for evidence that the protocol is failing (see OQ-1).

**Alternatives considered.**
- *`finding-investigate` skill from day one.* Rejected: speculative weight. The protocol form is reversible — promoting to a skill later requires extracting the protocol's structure into a SKILL.md, which is cheap.

### 5.5 Routing decisions and downstream handoff

**Purpose.** Every finding terminates in one of four routes: `spec-amend`, `spec-write`, `defer`, `close`.

**Interface.**
- **`spec-amend`** — the proposed remedy is a change to an existing spec. The Findings Pipeline produces a finding artifact at a known path; `spec-amend` accepts that path as a named input (`FINDING_PATH`) and uses the finding's investigation section as the input that would otherwise come from conversation. See [§Adoption Path](#11-adoption-path) for the minor amendment required to `spec-amend`.
- **`spec-write`** — the proposed remedy is large enough to warrant a new feature spec. `spec-write` accepts the finding path as a named input (`FINDING_PATH`) and uses it as Discovery-phase input — the architectural context the finding establishes is treated as authoritative rather than re-derived.
- **`defer`** — the finding is real but action is deliberately postponed. Defer requires rationale (why now is not the right time) and a watch condition (what should cause re-evaluation). Status: `routed`, with route subtype `defer`.
- **`close`** — no action will be taken. Close requires rationale (cannot reproduce; expected behavior; out of scope; superseded; etc.). Status: `closed`.

**Behavior.**
- Routing is the only place where the finding leaves the pipeline. A finding cannot be "left in triage forever"; either triage ends in a route or the finding goes to `defer` with a watch condition.
- Multiple findings may route to the same spec (a single amendment may resolve several findings; a new feature spec may incorporate evidence from several findings). The finding artifact records which spec absorbed it; the receiving spec's journal cites which findings it incorporated.

**Route subtype → terminal status mapping.** The four route subtypes map to two terminal status values: `spec-amend` and `spec-write` both terminate at `status: routed` (action delegated to a downstream spec); `defer` terminates at `status: routed` with route subtype `defer` (action consciously deferred, watch condition recorded); `close` terminates at `status: closed` (no action will be taken). The distinction: `routed` means "this finding has produced a decision and is no longer the pipeline's responsibility"; `closed` means "this finding required no decision-producing action." Reopening either terminal state is the `reopened` back-transition documented in §4.

### 5.6 Persona model

**Purpose.** Orient each phase toward the role that best fits it, even when one operator plays all roles.

**Behavior.**
- Each phase declares its **persona-frame** explicitly. The artifact's per-phase `Triaged by` / `Investigated by` field carries the persona-frame label (e.g., `business analyst (solo: Eric)`), not just the operator name.
- An AI agent performing a phase on the operator's behalf adopts that persona-frame as its prompt orientation. The triage skill instructs the agent to operate as a business analyst — domain-knowledgeable, not yet opening code. The investigation protocol instructs the developer frame — opening files, citing line numbers, proposing remedies.
- The persona model is **orientation, not handoffs**. The discipline structures the work without requiring multi-person teams. A team adopting the methodology later inherits a structure that maps cleanly to multi-person handoffs; a solo adopter benefits from role-framed self-direction in the meantime.
- **Intake's persona-frame is intentionally broader than the triage/investigation frames.** Triage maps to a domain-expert frame (typically business analyst) and investigation maps to a developer frame, but intake explicitly admits "service desk, manager, end-user, AI agent, or **anyone**" because the input source is unbounded — a stray observation in a meeting, an automated alert, or an external bug report are all valid signals. The asymmetry is by design: optimizing intake for capture rate (NFR: 60-second target) is incompatible with persona gating.

**Pattern invoked.** Role-based decomposition borrowed from service-management traditions (ITIL service desk / business analyst / engineering separation) without binding to any specific framework's role taxonomy.

### 5.7 Multi-repo findings

A finding may concern code or configuration in a different repository than the one the methodology repo lives in (e.g., an issue in `BattlePlugins/ArenaCTF` surfaces while working in `ai-tools`). The finding artifact:

- Lives in the **methodology-host repo** (or the repo from which the discipline is being driven).
- References the affected consumer repo by URL in the External references / Investigation sections.
- Routes to a `spec-amend` or `spec-write` *targeting the consumer repo*; the multi-repo commit discipline embedded in `spec-execute` handles the cross-repo execution boundary.

This re-uses the multi-repo discipline already established in `spec-execute` — no new multi-repo machinery is introduced here.

### 5.8 Interruption-tolerance

A finding raised during active `spec-execute` must not pull the executor off-task. The pipeline preserves this property by:

- Making intake cheap: a one-paragraph capture is sufficient; triage and investigation can wait.
- Making the finding artifact self-contained: when the operator returns to the finding later, the artifact carries everything needed; the originating conversation is not load-bearing.
- Not requiring the spec-execute session to commit a finding to one route. The finding is parked at `status: intake` until a later session triages it.
- Keeping the interruption-tolerance property self-contained within the pipeline: the three bullets above (cheap intake, self-contained artifact, no commit-to-route) hold without any change to `spec-execute`. Future enhancement: a separate amendment could teach `spec-execute` to surface "any open findings worth raising?" at task-boundary pauses, but that integration is out of scope for this design and not load-bearing for interruption-tolerance.

## 6. Non-functional Requirements

| Property | Requirement |
|---|---|
| **Adoptability (solo)** | A solo operator can run the full pipeline without persona-mismatch overhead. Persona-frame is orientation, not handoffs (see §5.6). |
| **Adoptability (team)** | The same artifacts and phases work without modification when persona-frames map to different humans. |
| **Observability** | Every status transition is journaled with date, persona-frame, and rationale. No transition is silent. |
| **Reversibility** | A closed finding may be reopened by a new status entry with rationale. A routed finding may be re-routed if the receiving spec is rejected (with journal entry). |
| **Context economy** | Finding artifacts target under ~200 lines. Skill artifacts that consume findings load only the relevant section, not the full journal history. |
| **Interruption-tolerance** | Intake cost: under 60 seconds of operator effort from a stray observation to a parked artifact. |
| **Persona durability** | A solo-adopter pipeline must not require restructuring to onboard team members. Role-frames already map to roles. |
| **Severity axis decoupling** | Operational urgency (P1–P4) is an optional axis decoupled from methodology severity (`blocker`/`important`/`advisory`). Operational findings may use both; testing/methodology findings typically use severity alone. The two axes may diverge — recorded decision, RC-2 schema pass. |
| **External-pointer durability** | The artifact survives external-system unavailability. Pointer text, summary, and any pasted context travel with the finding. |
| **Skill portability** | Findings-pipeline skills follow the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle): they bundle their own operational mirror of the schema and default templates, and adapt to host-context conventions (e.g., a project's `specs/findings/` storage directory, project-supplied `_template/` overrides, sibling skills) only when those conventions are present. The schema's authoritative articulation is this design spec (§5.1); [specs/findings/README.md](../findings/README.md) is the schema's derived human-readable projection — useful to humans browsing the storage location, not a runtime input for skills. |

## 7. Implementation Sequencing

Phases of work, not atomic tasks. Each phase produces an artifact the next consumes. Downstream feature specs (named below) carry the atomic task breakdown.

**Phase A — Schema commitment.** Lock the finding artifact template, the status state machine, and the persona-frame taxonomy. Produces: a reference template plus a brief schema description for downstream skill authors. **Downstream feature spec:** `findings-pipeline-schema`.

**Phase B — Intake skill.** Author the `finding-intake` skill. Accepts textual narrative or external-system pointer. Produces a finding artifact at status `intake`. **Downstream feature spec:** `finding-intake-skill`.

**Phase C — Triage skill.** Author the `finding-triage` skill. Consumes a finding at status `intake`; transitions to `triaged`. Persona-frame: business analyst. **Downstream feature spec:** `finding-triage-skill`.

**Phase D — Investigation protocol.** Embed the investigation section as a protocol inside the finding template (no separate skill yet). **Bundled into:** `findings-pipeline-schema` or split as needed during spec-write.

**Phase E — Integration amendments.** Minor amendments to `spec-amend` and `spec-write` to accept `FINDING_PATH` as a named input. **Downstream feature spec(s):** `spec-amend-finding-input`, `spec-write-finding-input`. May be bundled.

**Phase F — Adoption review (after first dogfood).** Route at least three real findings through the pipeline — one operational, one testing, one security — and run an adoption review. Decide whether to graduate investigation to a skill (OQ-1). **No new feature spec required; review checkpoint only.**

## 8. Validation Approach

The design is validated by:

- **Stakeholder review** of this spec at Review Checkpoint RC-1 (Eric, possibly with AI-agent assist).
- **Dogfooding** during Phase F: at least three real findings (one operational, one testing, one security) routed end-to-end. This is the cross-domain validation — the design's claim to span operational/testing/security must survive contact with reality before adoption is declared complete.
- **Example-source exercise**: take a recent ad-hoc journal note from a prior session (such notes have accumulated in feature-spec journals) and retroactively shape it as a finding to test that the format absorbs realistic content without distortion.
- **Persona-frame check**: at adoption review, audit whether each phase's persona-frame guidance was respected (did triage stay out of code? did investigation cite code references? did intake stay cheap?). If a phase routinely violates its persona-frame, the design is wrong and needs amendment.

## 9. Review Checkpoints

### RC-1 — Design Freeze

- **Trigger:** This architecture spec is complete and submitted for review.
- **Review focus:** Coherence of the three-phase model; correctness of the persona-frame claim; completeness of the artifact template; soundness of the integration approach with `spec-amend` and `spec-write`; whether the open questions are correctly scoped (in-scope for follow-on, not blockers).
- **Exit criteria:** Verdict of `pass` or `pass with comments` via `spec-review`. Open questions deferred to §13 are explicitly accepted as deferred. Any `[blocker]` finding triggers `spec-amend` before progressing.
- **Status:** pass with comments on 2026-05-17 by waseric (self-review). 0 blockers, 1 important (dangling §5.8 reference — route through `spec-amend`), 9 advisory (batch into RC-2 schema-pass amendment).

### RC-2 — Schema Review (gates Phase A → Phase B)

- **Trigger:** `findings-pipeline-schema` feature spec is complete and the reference template + state machine are committed.
- **Review focus:** Whether the artifact template is concrete and minimal; whether the state machine is unambiguous; whether the persona-frame fields carry their weight.
- **Exit criteria:** Template usable for a real finding without further interpretation; state transitions all uniquely defined.

### RC-3 — Intake & Triage Skill Review (gates Phase C → Phase E)

- **Trigger:** Both `finding-intake-skill` and `finding-triage-skill` feature specs are complete and the skills are operational.
- **Review focus:** Persona-frame guidance is correctly embedded in skill prompts; intake friction meets the 60-second target; triage produces hard facts (not hypotheses about cause).
- **Exit criteria:** Both skills exercised against at least one synthetic and one real finding; persona-frame check passes for each.
- **Status:** pass with comments on 2026-05-17 by Claude (self-review on behalf of waseric). 0 blockers, 0 important, 4 advisory. Advisories: (A-1) OQ-3 and OQ-4 resolutions not yet quoted back to §13 — Phase C §12 defers via follow-on `/spec-amend`, unblocked but tracked; (A-2) inherited RC-3a + RC-3b advisories remain as recorded, including the inspection-only verification of the triage state-machine guard and the skip-investigation surface; (A-3) two session-side intake findings ([spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/), [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/)) landed mid-Phase-C — the latter flags a portability risk in the intake/triage skills' host-relative path references that should be examined before any out-of-repo adoption; (A-4) RC-5's three-real-findings gate (operational + testing + security) is not satisfied — RC-3 only required one synthetic + one real per skill (met); security-domain example still missing. Exit criteria both met. Checkpoint closed; Phase E (`spec-amend` / `spec-write` accepting `FINDING_PATH`) unblocked per §7 Implementation Sequencing. See [journal entry `2026-05-17 — Review of RC-3`](journal.md#2026-05-17--review-of-rc-3).

### RC-4 — Integration Review (gates Phase E → Phase F)

- **Trigger:** Amendments to `spec-amend` and `spec-write` complete; both skills accept `FINDING_PATH` and use it as named input without re-asking for content the finding already establishes.
- **Review focus:** Whether the integration is non-breaking for existing `spec-amend` / `spec-write` invocations; whether `FINDING_PATH` is treated as authoritative input the way `DESIGN_SPEC_PATH` is by `spec-write`.
- **Exit criteria:** At least one finding successfully routes to each of `spec-amend` and `spec-write`. Existing invocations of both skills continue to work unchanged.

### RC-5 — Adoption Review

- **Trigger:** Three real findings (operational, testing, security) routed end-to-end.
- **Review focus:** Cross-domain claim survives reality; persona-frame discipline held; OQ-1 (investigation graduation) is decidable.
- **Exit criteria:** Recommendation to either declare the pipeline adopted, amend the design, or graduate investigation to a skill.

## 10. Risks and Mitigations

| Description | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| The design is wrong — three-phase model is the wrong cut | Low | High | RC-1 design review; RC-5 adoption review with explicit re-cut option | Eric |
| Pipeline ceremony outweighs benefit for solo work | Medium | High | NFR on interruption-tolerance (60-second intake target); protocol-first investigation; permission to skip investigation when route is obvious | RC-3 reviewer |
| Findings accumulate at `status: intake` without triage | Medium | Medium | Triage is the gate; close/defer are valid terminals; periodic operator review of stale `intake` findings as a habit, not enforced by tooling | Operator discipline; surfaced at RC-5 |
| Persona-frame treated as bureaucratic rather than orienting | Medium | Medium | Explicit guidance: personas are orientation, not handoffs. Persona-frame check at RC-5. Skill prompts model the frame for AI agents. | Skill authors; RC-3 reviewer |
| Vocabulary collision: in-checkpoint findings (spec-review) vs. pipeline findings | Low | Medium | Cross-reference in both directions: `spec-review` notes the pipeline as the home for out-of-checkpoint findings; finding artifacts may reference an originating `spec-review` if applicable | Documentation at RC-2 |
| External-system pointers go stale (GitHub issue moved/closed; Slack thread expired) | Medium | Low | Artifact captures verbatim summary at intake; external references are supplementary, not load-bearing | Intake skill behavior |
| Cross-repo findings produce orphan artifacts | Low | Medium | Multi-repo discipline already exists in `spec-execute`; finding artifact lives in the methodology-host repo; route targets the consumer repo via the existing mechanism | §5.7; revisited at RC-4 |

## 11. Adoption Path

### How an existing consumer adopts

1. **Create `specs/findings/` directory** in the methodology-host repo. No gitignore changes needed.
2. **Install (or sync) the new skills:** `finding-intake`, `finding-triage`. Once Phase F completes the graduation question, optionally `finding-investigate`.
3. **Apply the minor amendments** to `spec-amend` and `spec-write` to accept `FINDING_PATH`. Existing invocations continue to work; `FINDING_PATH` is additive and optional.
4. **Use the pipeline.** First finding establishes the operator's working pattern; subsequent findings follow.

### Reversibility

- A consumer backs out by ceasing to invoke `finding-intake`. Existing finding artifacts remain in `specs/findings/` as a durable record; no migration is required. The `FINDING_PATH` parameter on `spec-amend` / `spec-write` is optional, so leaving it unused has no side effect.
- A consumer may also archive `specs/findings/` to `specs/_archive/` (per the spec-path convention's archival pattern) if rolling back fully.

### Degradation mode if partially adopted

- **Intake-only adoption** (intake skill used, triage/investigation skipped) is useful: parked observations are recoverable later, even if the operator never runs full triage. This is the **minimum viable adoption** — a consumer benefits from intake alone.
- **Intake + triage, no investigation** is also viable: most findings can route without investigation if triage establishes obvious cause-and-remedy.
- **No intake, only ad-hoc routing to `spec-amend` / `spec-write`** — i.e., declining the pipeline entirely — leaves the methodology in its pre-Phase-2 state. Acceptable, but the gap is unaddressed.

### Methodology-host repo adoption

The `ai-tools` repo itself adopts the pipeline as part of Phase F dogfooding. The first three findings routed through the pipeline are recorded in `specs/findings/` here.

## 12. Out of Scope

- **Operational readiness methodology** (run books, health checks, alerting conventions). Separate Phase 2 deliverable on the roadmap.
- **Iteration methodology** (backlog grooming, prioritization, technical debt management). Separate Phase 2 deliverable.
- **Post-incident review as a standalone discipline.** A post-incident review may produce findings that enter this pipeline, but the review framework itself is a separate Phase 2 deliverable.
- **Automated intake from external systems.** No webhooks, no inbound API. Intake is human-initiated; external-system pointers are captured by hand.
- **Replacing `spec-review` checkpoint findings.** Those continue under `spec-review` and are not migrated to the pipeline.
- **Issue-tracker substitute.** The Findings Pipeline produces decisions and routes them. It does not maintain a long-lived ticket queue, owner assignments, or SLA tracking.
- **Cross-organization findings governance.** If outside adoption grows and findings need to flow between separately-governed orgs, that is a separate concern (potentially Phase 3 transition methodology).

## 13. Open Questions

### OQ-1 — Investigation graduation: when does the protocol become a skill?

**Question.** The pipeline launches with investigation as an embedded protocol (section in the artifact), not a separate skill. When should we promote it to a `finding-investigate` skill?

**Analysis.** Promoting prematurely adds weight to a phase that may not need it. Promoting too late means investigators are re-deriving structure for each finding. Promoting is a one-way ratchet only weakly: a skill can be unwound back to a protocol, but the bias against churn is real.

**Leaning.** Promote when *either* (a) three findings in a row produce investigation sections that look like ad-hoc retellings of the same structure, suggesting the structure deserves formalization; or (b) investigation regularly produces low-quality cause analysis that a skill prompt could constrain.

**Owner.** Decided at RC-5 (Adoption Review).

**Anti-goals.** Do not promote investigation to a skill merely for symmetry with intake/triage. Symmetry is not a goal; appropriate weight per phase is.

### OQ-2 — Incident vs. problem distinction (ITIL framing)

**Question.** Should the pipeline distinguish an *incident* (the event) from a *problem* (the underlying cause that may explain multiple incidents)? ITIL maintains this distinction strictly.

**Analysis.** A strict separation would mean a finding can be "an incident" until investigation links it to a "problem" finding that absorbs it. This is rich vocabulary but adds a state-machine branch and a one-to-many relationship the current shape does not have. Without strong evidence of need, it adds complexity.

**Leaning.** Collapse to "finding" for now. Re-expand if Phase 2's broader operational readiness work demonstrates that multi-incident problem tracking is a regular need. Cross-references between findings (one finding noting "subsumes finding-X, finding-Y") can capture the relationship informally without state-machine cost.

**Owner.** Deferred. Re-evaluated as part of the broader Phase 2 operational readiness design.

### OQ-3 — Multi-domain personas: is "business analyst" the right frame for triage of security findings?

**Question.** The persona-frame for triage is named "business analyst" — domain-knowledgeable, not yet opening code. For security findings, a more accurate frame might be "security analyst." For testing findings, perhaps "QA lead."

**Analysis.** Three possible cuts: (a) one persona-frame per phase regardless of domain ("triage is business-analyst-flavored, always"); (b) persona-frame varies by domain ("triage is BA for operational, security analyst for security, QA for testing"); (c) the field is descriptive — operator records whichever frame fits.

**Decided.** 2026-05-17 (RC-3). Option (c) — operator records the frame descriptively. The `finding-triage` skill suggests a frame derived from the `Domain` field (`operational` → business analyst; `security` → security analyst; `testing` → QA lead; `methodology` → methodologist; `other` → operator-named) and accepts free-text override. Encoded in [`.agents/skills/finding-triage/SKILL.md` L92–L104](../../.agents/skills/finding-triage/SKILL.md) (persona-frame derivation table + override path) and in [Phase C feature spec §5.3](../20260517-finding-triage-skill/feature.md). Validated on first real-signal dogfood: T-03 ([easy-survival-shelves-lwc-error](../findings/20260517-easy-survival-shelves-lwc-error/), commit `d79a1eb`) overrode suggested `business analyst` to operator-named `Sandlot administrator`, exercising the override path against real evidence.

**Watch item (resolved in the direction of *used*).** Pre-decision concern was that AI agents would default to the suggested frame and the override surface would atrophy. T-03 dogfood resolved this in the direction of override-is-used: the operator-named frame fits the finding better than the derived frame, and the prompt structure (suggest + accept-override) preserves agent guidance without removing operator authority. If a future signal shows the override path going unused across multiple real findings, revisit by generalizing the suggestion to "domain expert appropriate to <domain>" — but as of RC-3, the path is exercised and the discipline holds.

### OQ-4 — Triage-time revalidation policy for external pointers

**Question.** Intake captures external-system pointers verbatim alongside a summary (§5.2), and the artifact survives external-system unavailability (§6 NFR row). The remaining open question is narrower: should triage actively *revalidate* external pointers (follow the URL, check the linked ticket's current state), or treat the pointer as a static record?

**Analysis.** Active revalidation surfaces stale or contradictory external state at the right moment — when a triager is shaping the finding — but introduces an external dependency in triage that may slow it (network reachability, auth) and may pull the triager into the linked ticket's evolving discussion rather than the finding itself. Static treatment keeps triage focused but risks shaping a finding around a no-longer-accurate external pointer.

**Decided.** 2026-05-17 (RC-3). Optional revalidation with soft default. The `finding-triage` skill prompt asks the triager once whether to check the pointer; the soft default is `treated-as-static` when the Intake Summary is judged rich (≥3 sentences, names components, names reporters, includes verbatim quotes or snapshot references), and `recommend-check` when the Summary is sparse. Outcome recorded per pointer in the Triaged journal entry's `Pointer revalidation` field. Encoded in [`.agents/skills/finding-triage/SKILL.md` L106–L114](../../.agents/skills/finding-triage/SKILL.md) (rich-vs-sparse heuristic + soft-default branch) and in [Phase C feature spec §5.4](../20260517-finding-triage-skill/feature.md). Validated on first real-signal dogfood: T-03 ([easy-survival-shelves-lwc-error](../findings/20260517-easy-survival-shelves-lwc-error/), commit `d79a1eb`) accepted the `treated-as-static` soft default for an auth-walled forum thread (operator-supplied PDF snapshot is durable evidence; the policy "felt minimal, not ceremonial" per the T-03 journal entry). The sparse-Intake branch remains unexercised — if and when a sparse-Intake finding arrives, the `recommend-check` branch gets exercised then.

## 14. References

### Authoritative (in-repo, no verification needed beyond inspection)

- [specs/mission.md](../mission.md) — methodology mission and scope.
- [specs/tech-stack.md](../tech-stack.md) — constraints that shape this design.
- [specs/roadmap.md](../roadmap.md) — Phase 2 context, particularly the "integration points between operational findings and spec amendments" deliverable.
- [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) — spec path convention that this design follows.
- [`.agents/skills/spec-review/SKILL.md`](../../.agents/skills/spec-review/SKILL.md) — source of the `[blocker]/[important]/[advisory]` finding vocabulary extended here.
- [`.agents/skills/spec-amend/SKILL.md`](../../.agents/skills/spec-amend/SKILL.md) — downstream skill for the `spec-amend` route.
- [`.agents/skills/spec-write/SKILL.md`](../../.agents/skills/spec-write/SKILL.md) — downstream skill for the `spec-write` route.
- [`.agents/skills/spec-execute/SKILL.md`](../../.agents/skills/spec-execute/SKILL.md) — multi-repo discipline re-used for cross-repo findings (§5.7).

### Inspirational (frame-agnostic; not binding; no canonical citation verification performed)

These references name traditions that shaped the design's vocabulary and role separation. They are **not** canonical citations: no published source has been verified against the wording or claims attributed below, and the spec is not designed to track any specific framework's version or compliance target. The methodology is "informed by" these traditions per the constitution; precise attribution is deferred to external-adopter need (an adopter who requires citations against ITIL 4 / IEEE / FIRST.org publications can produce a verification pass as a separate exercise).

- **ITIL service-management traditions** — incident/problem management as the source of the role-separation framing (service desk / business analyst / engineering).
- **SDLC defect-lifecycle traditions** — reproduction-first triage; defect state machines. Cited as common engineering practice.
- **Coordinated vulnerability disclosure (CVD)** — security-finding flows from CERT/CC and FIRST.org traditions provide the parallel for security-domain findings.

---

### Spec metadata for downstream `spec-execute`

- **SPEC_REPO_ROOT:** `/Users/eric/scm/github/waseric/ai-tools` (this repo)
- **SPEC_TARGET_BRANCH:** `main`
- **Downstream feature specs (named, not yet authored):** `findings-pipeline-schema`, `finding-intake-skill`, `finding-triage-skill`, `spec-amend-finding-input`, `spec-write-finding-input`. The latter two may be bundled.
