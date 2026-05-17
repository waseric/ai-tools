# finding-intake skill depends on specs/findings/_template/ — breaks when installed globally — Journal

## 2026-05-17 — Intake: skill hardcodes path + line numbers to host project's `_template/`, breaks on global install

**Captured by:** waseric; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Surfaced during a verification check — operator asked whether the finding-intake skill depended on `specs/findings/_template/`, after I (the agent) had noted earlier in the same session that the skill links to `../../../specs/findings/_template/finding.md` and `journal.md` and references specific scaffolding line ranges (finding.md 1–22; journal.md 1–18 and 29–84) in Phase 3 steps 2–3 and in the WHAT NOT TO DO section. Two coupling layers: (1) the relative path itself, and (2) the line-number references inside the templates. Related question worth raising in triage: should the skill bundle its own `_template/` alongside SKILL.md so it can run standalone, or should it explicitly require a host-project `specs/findings/_template/` and refuse to run otherwise? Captured here so the question doesn't get lost; deciding the right shape is a triage/investigation question, not an intake question.
