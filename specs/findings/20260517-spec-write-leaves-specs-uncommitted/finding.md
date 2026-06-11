# spec-write leaves drafted specs uncommitted — Finding

> Status: intake
> Domain: methodology
> Severity: <blocker | important | advisory>           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-05-17
> Last transition: 2026-05-17

## Intake

**Reported by:** self (waseric, via Claude during `/spec-execute` orient phase)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** Two consecutive `/spec-write` runs in this repo (Phase B `finding-intake-skill` authored 2026-05-16; Phase C `finding-triage-skill` authored 2026-05-17) produced `feature.md` + `journal.md` artifacts under `specs/YYYYMMDD-<feature>/` but left the spec directory untracked. Both required an out-of-band `git commit` at the start of the next `/spec-execute` session's orient phase, where the untracked directory was detected as a drift signal. Phase B's remediation commit was `9a6d374 spec: Phase B feature spec — finding-intake-skill`; Phase C's was `c29f23b spec: Phase C feature spec — finding-triage-skill`. The pattern is: `/spec-write` ends with files on disk but no closeout commit, so every downstream `/spec-execute` orient phase has to detect untracked spec directories and prompt the operator to remediate before T-01 can begin. Known: the two occurrences; the remediation commit shape (consistent across both); the fact that `/spec-execute`'s orient phase reliably detects the gap and surfaces it. Unknown: whether `/spec-write` ever included a commit step and lost it, or whether it was intentionally omitted so the operator can review before committing; whether other authoring skills (`/spec-design`, `/project-constitution`) exhibit the same pattern; whether the right remediation is to add a closeout-commit step to `/spec-write`, to add a closeout reminder to its OUTPUT FORMAT, or to leave the `/spec-execute` orient-phase detection as the system's intentional safety net. Classification (severity, route) belongs to triage.
**External references:** none — methodology-internal observation. Pattern evidence is in the repo's git history (`git log --oneline -- specs/20260517-finding-intake-skill/`, same for `specs/20260517-finding-triage-skill/`) and in the orient-phase output of the two `/spec-execute` sessions where the gap was surfaced.

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
