# Findings pipeline missing investigate/route skills — Finding

> Status: intake
> Domain: methodology
> Severity: <blocker | important | advisory>
> Date opened: 2026-05-20
> Last transition: 2026-05-20

## Intake

**Reported by:** self
**Reported via:** text
**Captured by:** Eric Wasgatt; persona-frame: intake
**Summary:** The findings pipeline has skills for Phase B (finding-intake) and Phase C (finding-triage), and downstream consumers for routed output (spec-amend, spec-write, spec-execute). But Phases D (Investigation) and E (Route) have no skill support. When an operator reaches the post-triage boundary with a non-obvious finding, no skill exists to guide investigation against existing specs/architecture or to produce a structured route decision. The operator reaches for `spec-design` as the closest available tool, which collapses investigation + routing into "amend the spec + make the code fix directly" — bypassing spec-write and spec-execute rigor. The pattern has been observed across multiple findings in the private-design-repo repo (2026-05-19 through 2026-05-20). The gap is structural: the pipeline design spec (20260517-findings-pipeline/architecture.md) defines these phases but no corresponding skills were authored.
**External references:** —

## Triage

**Triaged by:** <persona-frame: service desk | business analyst | developer>
**Triage date:** <YYYY-MM-DD>
**Reproducibility:** <reliably | intermittently | not reproduced | not applicable>
**Repro steps (if reproducible):**
1. ...
**Scope:** <who/what is affected>
**Domain confirmation:** <operational | testing | security | methodology | other>
**Severity confirmation:** <blocker | important | advisory>
**Triage notes:** <anything else surfaced in triage; rejected hypotheses; clarifications from reporter>

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
