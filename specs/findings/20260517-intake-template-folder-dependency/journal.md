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

## 2026-05-17 — Under investigation: coupling symptoms reframed as effects of a missing methodology-level Atomic-Skill Portability Principle

**Investigated by:** waseric; developer; persona-frame: investigation
**Prior status:** `triaged`
**New status:** `under-investigation`
**Initial hypothesis:** Symptoms (path coupling; line-number coupling) are downstream of a missing methodology-level commitment that skills are portable atomic units. The load-bearing remedy is to commit the principle in [specs/tech-stack.md](../../tech-stack.md) and cascade implementation choices through the findings-pipeline design spec, the schema feature spec, the two existing SKILL.md files, and the two template files.
**Notes:** Investigation conducted through a design-level conversation with the operator (this session, 2026-05-17), grounded in the file-level evidence already established at triage (the relative paths and line ranges in `finding-intake` and `finding-triage` SKILL.md). The conversation surfaced three reframings: (a) Branch C accepted over Branch A (narrow fix in two SKILL.md files) and Branch B (pluggable storage backends — Branch B re-opens design-spec §5.1 / §12 decisions and was rejected); (b) host's `specs/findings/README.md` reframed as documentation — a derived projection of the schema for human readers — not a required runtime distributable; the schema's conceptual source of truth is the design spec, and the skill carries its own operational mirror; (c) the principle generalizes methodology-wide ("each of our skills need to be an atomic unit that happens to know how to work with the other embodiments of the methodology when deployed outside the context of our project"), so the primary amend target is [specs/tech-stack.md](../../tech-stack.md) rather than the findings-pipeline design spec alone. The operator noted that if a constitution-level `architecture.md` existed it would be the natural home; `tech-stack.md` is the right current location and the principle can migrate later if/when an `architecture.md` doc lands. Constitution-amendment workflow is currently undefined — `/spec-amend`'s documented scope names design and feature specs, not constitution docs; pragmatic resolution for this work is to use `/spec-amend` informally against `tech-stack.md` and file a follow-on advisory finding to capture the gap. Next step: route via `/spec-amend` against [specs/tech-stack.md](../../tech-stack.md) (primary), with cascading amendments to the findings-pipeline design spec, the schema feature spec, the two SKILL.md files, and the two template files. Audit of spec-* skills for principle compliance follows as separate sibling findings (closed immediately if compliant).
