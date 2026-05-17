# Test-only signal — synthetic-fixture-naming inconsistency — Finding

> Status: triaged
> Domain: methodology
> Severity: advisory
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

**Triaged by:** waseric; methodologist; persona-frame: triage
**Triage date:** 2026-05-17
**Reproducibility:** reliably
**Repro steps (if reproducible):**
1. List `specs/findings/` (`ls specs/findings/`).
2. Observe that `_template/` and `README.md` sit at the same nesting level as the dated `YYYYMMDD-<short-name>/` directories.
3. Read [specs/20260515-spec-path-convention/architecture.md](../../20260515-spec-path-convention/architecture.md) and confirm it does not call out underscore-prefixed directory names as a reserved sentinel for "not-a-finding."
4. Confirm the layout convention for `_template/` is therefore implicit (load-bearing by convention, undocumented as such).
**Scope:** Newcomers (humans or AI agents) scanning `specs/findings/` for the first time without context about the underscore-prefix convention; scripts or skills that iterate the findings directory and must distinguish dated finding dirs from non-finding entries (`_template/`, `README.md`).
**Domain confirmation:** methodology
**Severity confirmation:** advisory
**Triage notes:** Signal is fabricated for T-02 validation of the finding-triage skill (per [specs/20260517-finding-triage-skill/feature.md §7 T-02](../../20260517-finding-triage-skill/feature.md#t-02--synthetic-validation-exercise)) — not a real-world observation. Severity assessed as `advisory` rather than `important` because the ambiguity is self-correcting for anyone who opens `_template/finding.md` (the leading HTML comment explicitly identifies it as a template), and because no real-world friction has been observed; `important` is reserved for cases where the implicit convention has caused actual confusion or wasted effort. No cause hypothesis recorded — Triage produces hard facts about shape; the route question (whether to amend the spec-path-convention spec to declare the `_` prefix as reserved, or to add a `findings/README.md` paragraph naming the convention) belongs to route, not Triage.

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
