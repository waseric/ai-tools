# Phoenix Principle alignment gaps — Finding

> Status: routed
> Domain: methodology
> Severity: advisory           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-17
> Last transition: 2026-08-17                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self
**Reported via:** text + URL
**Captured by:** waseric; persona-frame: intake
**Summary:** An external manifesto — Alexandre Bergel's "The Phoenix Principle: A Manifesto for Programmers in the AI Age" — was assessed against this repo's `spec-*` skill family to gauge methodological alignment. The assessment found roughly 70% convergence, with the divergences principled rather than accidental: both the manifesto and this methodology conclude that meaning must live outside the code and that AI-assisted work without spec lineage is debt, but they arrive there from opposite premises (Bergel from epistemology, concluding code should be *regenerable*; this methodology from SDLC/ITSM governance, concluding code should be *accountable*). Strong alignment was found on Specification-as-Source (this repo arguably exceeds the manifesto, since `spec-amend` enforces against spec decay, which the manifesto does not address), on the anti-"vibe-coding" stance (`spec-execute` gives every unit of work spec lineage by construction), and on the Symbiont Partnership (the `spec-worker`/`spec-reviewer` dispatch model). Three divergences were identified. Two are deliberate and follow from the lifecycle mission — the **Deletion Test** and **Phoenix Architecture** regenerability model is incompatible with incremental, revertible-at-each-boundary task sequencing and with roadmap Phases 2–3 (operation, incident response, transition, retirement); and feature specs are scoped to *deltas* against an existing codebase (touch surface, migration, rollback, backward compat) rather than to complete system behavior. The third divergence is the one worth mining: **Tests as Truth**. This methodology treats tests as *evidence* satisfying a task's Definition of Done (`spec-write` §7–8), not as a language-agnostic definition of correctness. There is no `tests.yaml`-equivalent artifact — no executable, implementation-independent contract a regenerated or refactored implementation could be validated against. Acceptance criteria are prose Given/When/Then inside a markdown spec: verifiable by a human or agent *reading* it, but not mechanically executable. That gap bears directly on the repo's stated third design-bar property, *rework prevention* ("mechanically re-derivable claims"), and a per-feature-spec executable contract artifact would serve it at some cost to *token economy*. Captured for later mining; no change to the methodology is proposed or implied at intake.
**External references:**
<!-- fetched 2026-08-17 -->
https://medium.com/@bergel/the-phoenix-principle-a-manifesto-for-programmers-in-the-ai-age-ca63317c5ebc

Snapshot of the load-bearing content (summarized from a successful live fetch on 2026-08-17):

> Core thesis: "Code is ephemeral. Specification is eternal. Tests are truth." The shift is from "programming IN languages" to "programming WITH language" — humans supply precise specification, AI supplies implementation, code becomes regenerable and deletable.
>
> Six named principles:
> 1. **The Deletion Test** — if you delete your entire codebase, does meaningful documentation survive? If not, your meaning lives in the wrong location.
> 2. **Specification as Source** — `SPEC.md` is the actual source code: precise behavioral definitions, formal logic where applicable, edge cases, security requirements, performance constraints. Implementation in any language is merely a translation.
> 3. **Tests as Truth** — `tests.yaml` defines correctness via input/output pairs and edge cases, as language-agnostic contracts. Any implementation passing all tests is valid; failure means the implementation is wrong, not the tests.
> 4. **The Phoenix Architecture** — five essential files: `AXIOMS.md`, `SPEC.md`, `tests.yaml`, `ARCHITECTURE.md`, `INSTALL.md`. All implementation directories remain deletable.
> 5. **The New Literacy** — programming skill becomes precise specification writing, test-first thinking, architectural vision, language precision. Syntax mastery becomes secondary.
> 6. **The Symbiont Partnership** — humans contribute meaning, architecture, specification; AI contributes implementation, optimization, translation. Neither succeeds alone.
>
> Named anti-pattern — **"vibe coding"**: conversational AI usage without specification, producing "orphaned code" with no specification lineage, no reproducibility, and maximum technical debt.
>
> Prescriptions: write specifications first with mathematical precision where possible; define tests comprehensively (happy paths, edge cases, errors); generate implementations in multiple languages from the same spec; delete and regenerate regularly to verify the specification is complete; apply the Phoenix Test — can it be deleted, does specification exist, will regeneration work.
>
> Historical grounding: Euclidean geometry (axioms/theorems/proofs), scientific methodology (theory/predictions/experiments), clinical trials (protocol/endpoints/data collection) — positioned as recovering ancient epistemological patterns applied to software.

## Triage

**Triaged by:** waseric; methodologist; persona-frame: triage
**Triage date:** 2026-08-17
**Reproducibility:** not applicable
**Repro steps (if reproducible):**
not applicable
**Scope:** The `spec-*` skill family's treatment of correctness contracts — specifically `spec-write` §7 Task Breakdown (`Tests required`, `Definition of Done`) and §8 Test Strategy, and `spec-execute` Phase 5 DoD verification. Affects any consuming project whose feature specs need their correctness claims re-verified after the originating session ends — i.e. the *rework prevention* leg of the CLAUDE.md design bar. Does not affect any currently-shipping artifact's correctness; nothing is broken today. The two other divergences recorded at intake (Deletion Test / regenerability; specs-as-delta) are scoped out: they were assessed as deliberate consequences of the lifecycle mission, not as gaps.
**Domain confirmation:** methodology
**Severity confirmation:** advisory
**Triage notes:** This finding is a durable record of an *evaluation*, not a defect report — hence `not applicable` reproducibility. The underlying observation is directly re-checkable by reading `spec-write` §7–8, but nothing "reproduces" in the defect sense. Severity is `advisory` because the gap is a missed opportunity rather than a failure: specs produced today are internally consistent and verifiable by a reader; they simply carry no *mechanically executable* correctness contract. No blocker or important consequence was identified. Cause-shaped observations surfaced during the originating assessment are **deferred to investigation, not asserted here** — notably the hypothesis that the absence of an executable contract artifact is a deliberate token-economy tradeoff rather than an oversight, and the hypothesis that a `tests.yaml`-equivalent would conflict with the family's tooling-agnostic stance (`mission.md` Out of Scope). Both would need investigation to confirm. Deduplication check: no existing finding in `specs/findings/` covers correctness-contract executability. Adjacent positive-alignment observation about `journal.md` having no analogue in the source manifesto is recorded in the journal, not here — it is context, not a triage fact.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** defer
**Decided by:** waseric (operator); methodologist; persona-frame: triage
**Route date:** 2026-08-17
**Target spec:** not applicable (defer). If re-evaluated and accepted, the likely target is `specs/20260518-spec-write-skill/` via `spec-amend` against §7–8 of the skill, with `specs/20260518-spec-execute-skill/` as a coupled secondary target.
**Route rationale:** Defer rather than amend. The operator explicitly asked for capture-for-later-mining and did not ask for a methodology change, and nothing here is broken: the gap is an unclaimed property, not a defect. Amending `spec-write` now would add a required artifact class to every feature spec on the strength of a single external manifesto's framing, trading measurable *token economy* against a *rework prevention* benefit that has not yet been observed to bite in practice. Close was rejected because the observation is substantive and touches a stated design-bar property — closing would discard it. Investigation was skipped because triage established everything needed to decide the route: the affected surface is named, the tradeoff axis is named, and the remaining questions are design questions for a future `spec-design`/`spec-amend` session rather than diagnostic ones. **Watch condition — re-evaluate if any of the following occurs:** (a) a real rework incident is traced to a feature spec's correctness claims being un-re-verifiable after its originating session ended; (b) a `spec-review` checkpoint is unable to mechanically confirm a prior task's Definition of Done and has to re-derive intent from prose; (c) dispatch-mode execution (`spec-worker`) produces a plausible-but-wrong change that passes its own tests, where an implementation-independent contract would have caught it; (d) the methodology is adopted by an outside project that asks for executable acceptance criteria; or (e) roadmap Phase 2 work on the operate→build feedback loop independently surfaces a need for machine-checkable spec assertions.
