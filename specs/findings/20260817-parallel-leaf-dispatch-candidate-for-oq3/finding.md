# Candidate answer to dispatch OQ-3: same-tier leaf fan-out with serialized closeout — Finding

> Status: triaged
> Domain: methodology
> Severity: advisory           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-17
> Last transition: 2026-08-17                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self (operator, during a repository reconciliation review)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** [dispatch-execution §13 OQ-3](../../20260705-dispatch-execution/architecture.md) — "When may an orchestrator run multiple workers concurrently?" — remains open, owned by "future amendment; earliest after CP-3". Its analysis names two blockers: concurrent workers writing paired commits and journal entries contend on the spec repo, and receipt-acceptance ordering becomes a merge problem. It also fixes an anti-goal: "do not parallelize by giving workers disjoint journal files — splitting the journal breaks the single-handoff-artifact property that everything else relies on." A concrete recipe exists that satisfies both blockers without violating the anti-goal: **fan out only across dependency-independent leaf tasks sharing the same declared model floor, run each worker in an isolated worktree, and have the orchestrator serialize all closeout writes** — receipts are accepted one at a time in a deterministic order, and only the orchestrator appends to the journal. The journal stays single; contention moves from the journal to the orchestrator's accept loop, which is already serial by construction. Same-floor grouping keeps a batch from mixing tiers in one fan-out, which would otherwise make the accept ordering carry a correctness meaning it does not have today. This finding records the recipe as an input for whoever resolves OQ-3; it does not argue for opening the gate early.
**External references:** none

## Triage

**Triaged by:** waseric; persona-frame: developer
**Triage date:** 2026-08-17
**Reproducibility:** not applicable (this is a design input against a declared open question, not a defect)
**Repro steps (if reproducible):**
1. Not applicable. Read `specs/20260705-dispatch-execution/architecture.md` §13 OQ-3 for the question this addresses.
**Scope:** Future resolution of OQ-3 only. Nothing in the current serial dispatch path changes, and nothing here is proposed for adoption before the gate the design already set.
**Domain confirmation:** methodology
**Severity confirmation:** advisory
**Triage notes:** Deliberately filed at `advisory`, and it should not be read as pressure to parallelize. OQ-3's leaning is "defer until serial receipts prove ≥ one full batch at 100% fidelity", and that gating condition is about *receipt fidelity*, which this recipe does not address and cannot substitute for — parallel workers producing unreliable receipts is strictly worse than serial workers producing unreliable receipts. The recipe answers "how, once the gate opens", not "whether the gate should open". Two known weaknesses to carry into any future amendment: isolated worktrees interact with the multi-repo paired-commit discipline in ways not worked through here (a paired spec-repo commit from a worktree-isolated worker needs a defined landing path), and "dependency-independent" currently relies on consuming specs' `Deps:` fields being complete, which nothing validates. Investigation skipped — there is no defect to investigate, and the material is an input to a question already owned by a future amendment.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** <spec-amend | spec-write | defer | close>
**Decided by:** <persona-frame of the deciding phase, and operator>
**Route date:** <YYYY-MM-DD>
**Target spec:** <path to spec, when route is `spec-amend` or `spec-write`; e.g., specs/20260517-findings-pipeline/architecture.md>
**Route rationale:** <one paragraph; why this route over the others. For `defer`, include watch condition: what would cause re-evaluation.>
