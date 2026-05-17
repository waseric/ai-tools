# finding-intake skill depends on specs/findings/_template/ — breaks when installed globally — Finding

> Status: intake
> Domain: methodology
> Severity: <blocker | important | advisory>           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-05-17
> Last transition: 2026-05-17                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** The `finding-intake` skill at [.agents/skills/finding-intake/SKILL.md](../../../.agents/skills/finding-intake/SKILL.md) instructs the invoking agent to (a) read `specs/findings/_template/finding.md` and `specs/findings/_template/journal.md` during ORIENT, (b) copy those templates verbatim during APPLY (Phase 3 steps 2–3), and (c) strip specific *line ranges by number* from the scaffolding comments inside those templates (finding.md lines 1–22; journal.md lines 1–18 and 29–84). This is a two-level dependency on the host project's `specs/findings/_template/` folder: the path must exist *and* the line numbering inside the templates must match what SKILL.md hardcodes. The dependency works fine while the skill lives co-located with the spec at `.agents/skills/finding-intake/` inside the ai-tools repo. It breaks the moment the skill is installed globally at `~/.agents/skills/finding-intake/` or `~/.claude/skills/finding-intake/` and invoked against a different repo that has no `specs/findings/_template/` directory — the agent has nothing to copy from, and the `../../../specs/findings/_template/...` relative paths in SKILL.md resolve outside the user's workspace. One reasonable interpretation is "adopt the host project's findings folder when present"; the concern is that the skill currently has no fallback when it isn't, and the line-range coupling means even projects that *do* have a `_template/` folder must keep their template line numbering in lockstep with SKILL.md. Options worth considering: inline the template bodies into SKILL.md, replace numeric line-range strips with marker-based delimiters (e.g. `<!--scaffold-start-->` … `<!--scaffold-end-->`), or have the skill ship a bundled `_template/` alongside SKILL.md with the host project's folder as an override.
**External references:**

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
