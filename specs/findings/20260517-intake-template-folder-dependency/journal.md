# finding-intake skill depends on specs/findings/_template/ — breaks when installed globally — Journal

## 2026-05-17 — Intake: skill hardcodes path + line numbers to host project's `_template/`, breaks on global install

**Captured by:** waseric; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Surfaced during a verification check — operator asked whether the finding-intake skill depended on `specs/findings/_template/`, after I (the agent) had noted earlier in the same session that the skill links to `../../../specs/findings/_template/finding.md` and `journal.md` and references specific scaffolding line ranges (finding.md 1–22; journal.md 1–18 and 29–84) in Phase 3 steps 2–3 and in the WHAT NOT TO DO section. Two coupling layers: (1) the relative path itself, and (2) the line-number references inside the templates. Related question worth raising in triage: should the skill bundle its own `_template/` alongside SKILL.md so it can run standalone, or should it explicitly require a host-project `specs/findings/_template/` and refuse to run otherwise? Captured here so the question doesn't get lost; deciding the right shape is a triage/investigation question, not an intake question.

## 2026-05-17 — Triaged: methodology blocker; two-level coupling (path + line numbers) confirmed by inspection; route deferred to investigation

**Triaged by:** waseric; methodologist; persona-frame: triage
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably (by inspection of SKILL.md — relative paths and line-range strips are observably present in Phase 3 steps 2–3)
**Domain/severity changes from intake:** Domain confirmed `methodology` — no change. Severity set to `blocker` (intake left `<placeholder>` per schema; first authoritative severity assignment).
**Skip-investigation decision (if any):** end at triaged — three candidate remedies (inline templates / marker delimiters / bundled `_template/` with host override) are non-trivial design choices that warrant file-level investigation evidence before routing.
**Pointer revalidation:** not applicable — Intake's External references field is empty.
**Notes:** Investigation should audit `.agents/skills/finding-triage/SKILL.md` (Phase C skill, just landed) for parallel coupling to the same `_template/` scaffolding before authoring a remedy, since the remedy may need to cover both skills at once. The line-number coupling and the path-resolution coupling are distinct concerns and may warrant separate decisions; investigation should treat them as two coupled-but-separable axes, not one.
