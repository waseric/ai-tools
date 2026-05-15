# ai-tools — Roadmap

> Audience: Eric Wasgatt (author); AI coding agents consuming the methodology's artifacts; engineers evaluating or adopting the methodology
> Status: Forward-looking — last updated 2026-05-15

## Phase 1 — Spec-Driven Development Core (current)

**Outcome:** A complete, self-consistent methodology for taking a product from instantiation through architecture, specification, implementation, review, and amendment — with governance at every boundary.

**Timing:** In progress. Constitution established 2026-05-15; core skill family operational.

**Key deliverables:**
- Project constitution skill (`project-constitution`)
- Architecture/design spec skill (`spec-design`)
- Feature spec authoring skill (`spec-write`)
- Spec execution skill (`spec-execute`)
- Spec review skill (`spec-review`)
- Spec amendment skill (`spec-amend`)
- Session economy and multi-repo commit disciplines propagated across the family
- Repository constitution (mission, tech-stack, roadmap) — this document

**Exit criteria:**
- All six skills operational and producing quality artifacts with minimal oversight
- Cross-cutting disciplines (session economy, multi-repo) embedded, not appended
- The methodology repo has its own constitution and follows its own conventions
- At least one external project has been taken from constitution through active spec-execute using the methodology

## Phase 2 — Operational Lifecycle

**Outcome:** The methodology extends beyond build into the phases that consume what build produced — operation, monitoring, incident response, and iterative improvement.

**Timing:** After Phase 1 exit criteria are met.

**Key deliverables:**
- Operational readiness methodology (run books, health checks, alerting conventions)
- Incident and problem management disciplines (triage, root cause, post-incident review)
- Iteration methodology (backlog grooming, prioritization, technical debt management)
- Integration points between operational findings and spec amendments (feedback loop from run back into build)

**Exit criteria:**
- A product managed under the methodology can transition from active development to steady-state operation using methodology artifacts — not tribal knowledge
- Operational incidents produce structured artifacts that feed back into the methodology's spec pipeline

## Phase 3 — Transition and End-of-Life

**Outcome:** The methodology covers the full remaining lifecycle — handoff to new owners, technology migration, deprecation, and retirement — closing the loop on the lifecycle ambition stated in the mission.

**Timing:** After Phase 2 disciplines are proven in at least one real project.

**Key deliverables:**
- Transition methodology (knowledge transfer, ownership handoff, documentation audit)
- Migration/modernization disciplines (technology refresh, platform migration)
- Deprecation and retirement methodology (sunset criteria, data disposition, stakeholder communication)
- Validation artifacts (`validation.md` as a constitution doc for mature/inherited repos)

**Exit criteria:**
- A product can be transitioned from one owner to another using methodology artifacts as the primary orientation material
- The methodology's own `roadmap.md` can graduate to `validation.md` — the lifecycle is complete

## Out of Roadmap

- **Domain-specific methodology variants** — ServiceNow, Minecraft, etc. remain in their respective repos. The methodology provides the framework; domains provide the context. If a pattern emerges for domain specialization, it may become a Phase 2+ deliverable.
- **AI model or platform lock-in** — the methodology will not specialize for any single AI provider. Mechanism evolution (skills → something else) is expected and welcomed.
- **Certification or compliance mapping** — ITIL/ITSM/SDLC frameworks inform the methodology but the methodology does not aim to be a compliance vehicle for any of them.
- **Community governance** — if outside adoption grows, governance structures (contributing guide, code of conduct, maintainer rotation) will be addressed then, not preemptively.
