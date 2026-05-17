<!--
This is a template. Copy this file to specs/findings/YYYYMMDD-<short-name>/finding.md
when creating a new finding. Fill the Intake section first; later phases (Triage,
Investigation, Route) append as the finding progresses through the pipeline — they
do not need to be filled at intake time.

See specs/findings/README.md for the schema, state machine, and field reference.
See specs/20260517-findings-pipeline/architecture.md for the design spec.

Conventions when filling this template:
- Replace every <placeholder> with the corresponding value, or with "unknown" if
  not yet known at the current phase.
- Sections for future phases (Triage / Investigation / Route) may be left as-is
  with placeholders until that phase begins; the placeholder structure makes it
  clear what to fill when the phase arrives.
- The Investigation section is optional and may be skipped entirely when triage
  produces an obvious route. Journal the skip rationale in journal.md rather
  than deleting the section.
-->

# <Short title> — Finding

> Status: <intake | triaged | under-investigation | routed | closed | reopened>
> Domain: <operational | testing | security | methodology | other>
> Severity: <blocker | important | advisory>           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
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
**Rationale:** <one paragraph; why this route over the others. For `defer`, include watch condition: what would cause re-evaluation.>
