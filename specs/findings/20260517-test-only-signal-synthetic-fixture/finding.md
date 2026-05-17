# Test-only signal — synthetic-fixture-naming inconsistency — Finding

> Status: intake
> Domain: methodology
> Severity: <blocker | important | advisory>           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-05-17
> Last transition: 2026-05-17

## Intake

**Reported by:** self (T-02 synthetic exercise — this finding is fabricated for validation of the finding-intake skill; it is not a real signal)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** While reviewing the methodology repo's `specs/findings/` layout, I noticed (fabricated for T-02 validation — not a real signal) that the underscore-prefixed `_template/` directory sits alongside dated finding directories at the same nesting level, but the spec-path convention spec doesn't explicitly call out the underscore-prefix as a reserved sentinel for "not-a-finding directory." A newcomer scanning `specs/findings/` might either ignore `_template/` as detritus or treat it as a finding from an unspecified date. The convention is implicit; whether it should be explicit is a methodology-domain question.
**External references:** (none)

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
