# finding-intake skill depends on specs/findings/_template/ — breaks when installed globally — Finding

> Status: triaged
> Domain: methodology
> Severity: blocker                                    ← methodology axis
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

**Triaged by:** waseric; methodologist; persona-frame: triage
**Triage date:** 2026-05-17
**Reproducibility:** reliably
**Repro steps (if reproducible):**
1. Open `.agents/skills/finding-intake/SKILL.md`.
2. Confirm Phase 3 step 2 references stripping `finding.md` lines 1–22, and Phase 3 step 3 references stripping `journal.md` lines 1–18 and 29–84.
3. Confirm the relative path `../../../specs/findings/_template/finding.md` (and the journal sibling) appears in ORIENT and APPLY — this path resolves outside the user's workspace when SKILL.md lives at `~/.claude/skills/finding-intake/SKILL.md`.
4. Simulate (or perform) installation at `~/.claude/skills/finding-intake/` and invocation against a repo with no `specs/findings/_template/` directory — the agent has nothing to copy from, and the path strings in SKILL.md point outside the user's workspace.
**Scope:** The `finding-intake` skill today, plus any future findings-pipeline skill that inherits the same scaffolding-copy pattern. Phase C's `finding-triage` skill just landed and is a strong candidate for parallel coupling (investigation should audit); Phase D/E skills (investigation, route) have not yet been authored, so the coupling pattern can still be corrected before it spreads.
**Domain confirmation:** methodology
**Severity confirmation:** blocker
**Triage notes:** Three candidate remedies surfaced in the intake summary — (a) inline the template bodies into SKILL.md, (b) replace numeric line-range strips with marker-based delimiters (e.g. `<!--scaffold-start-->` … `<!--scaffold-end-->`), (c) ship a bundled `_template/` alongside SKILL.md with the host project's folder as an override. These are *deferred to investigation*: they require file-level evidence (template-author ergonomics; whether the two coupling layers — path resolution and line numbering — should be removed together or separately; host-project override semantics). Hypothesis worth recording, not asserting: the line-number coupling looks easier to remove (marker-based delimiters are mechanical) than the path-resolution coupling (which forces a design choice between "skill ships its own scaffolding" vs. "skill requires host-project scaffolding"). Deferred to investigation.

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
