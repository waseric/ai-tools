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

## 2026-05-17 — Amendment 2026-05-17-1 (specs/tech-stack.md)

**Section amended:** [specs/tech-stack.md](../../tech-stack.md) §Tooling Conventions (added new subsection `Atomic-Skill Portability Principle` after the existing three-bullet body).
**Trigger:** Investigation phase of this finding identified path/line-number coupling in `finding-{intake,triage}` SKILL.md as downstream symptoms of a missing methodology-level commitment that skills are portable atomic units.
**Reason:** The methodology had no written commitment that skills must be portable atomic units. The single-line claim "Each skill is self-contained and independently readable" in the existing first bullet was too thin to prevent the path/line-number coupling pattern from recurring as new skills are authored.
**Impact summary:** Existing `finding-{intake,triage}` SKILL.md are non-compliant; addressed by cascading amendments. All future skill authoring inherits the principle. No tasks or checkpoints affected; no completed work invalidated.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (tech-stack.md remains Living document; addition extends rather than reopens existing commitments).
**Commit:** `b515c71`
**Journal location note:** tech-stack.md has no journal of its own. This amendment record is being kept in the originating finding's journal as the cascade-tracking record; a follow-on advisory finding will capture the constitution-amendment workflow gap.

### Full record

## Amendment 2026-05-17-1 — specs/tech-stack.md §Tooling Conventions

**Trigger.** Investigation phase of [finding intake-template-folder-dependency](finding.md) (commit `0bc9d82`) identified that path-coupling and line-number-coupling between `finding-intake`/`finding-triage` SKILL.md and the host repo's `specs/findings/_template/` folder are downstream symptoms of a missing methodology-level commitment that skills are portable atomic units. The principle was articulated in design-level conversation on 2026-05-17 and generalizes methodology-wide ("each of our skills"), not findings-pipeline-only — placing the primary commitment here in `tech-stack.md` rather than in any one design spec.

**Section.** [specs/tech-stack.md](../../tech-stack.md) §`Tooling Conventions` (L15–L19), adding a new `### Atomic-Skill Portability Principle` subsection at the end of the section.

**Change.** Three-bullet body of `Tooling Conventions` left unchanged; a new `### Atomic-Skill Portability Principle` subsection appended, articulating the three operational decompositions (self-contains; discovers-and-adapts; degrades-cleanly), declaring methodology-wide scope across all `.agents/skills/` entries, stating the rationale, and citing the originating finding. Full before/after diff shown in the Phase 2 draft of this amendment; the After block has been applied verbatim.

**Reason.** The methodology had no written commitment that skills must be portable atomic units. The single-line claim "Each skill is self-contained and independently readable" in the existing first bullet was too thin to prevent the path/line-number coupling pattern from recurring as new skills are authored. Writing the principle down as an explicit commitment makes it binding for new skill authoring and provides a contract for auditing existing skills.

**Impact.**
- **Affected tasks:** None (no task-level work in flight against tech-stack.md).
- **Affected checkpoints:** None (tech-stack.md has no checkpoints).
- **Completed work invalidated:** No. `finding-intake/SKILL.md` and `finding-triage/SKILL.md` were authored before this commitment and are non-compliant with it; their non-compliance is the originating finding's surface and will be addressed in cascading amendments. The skills' completed work is not invalidated — it requires conformance edits.
- **Cross-references requiring follow-up:** [specs/20260517-findings-pipeline/architecture.md](../../20260517-findings-pipeline/architecture.md), [specs/20260517-findings-pipeline-schema/feature.md](../../20260517-findings-pipeline-schema/feature.md), [.agents/skills/finding-intake/SKILL.md](../../../.agents/skills/finding-intake/SKILL.md), [.agents/skills/finding-triage/SKILL.md](../../../.agents/skills/finding-triage/SKILL.md), [specs/findings/_template/finding.md](../_template/finding.md), [specs/findings/_template/journal.md](../_template/journal.md). All land as separate amendments in this cascade.
- **Audit follow-on:** spec-* skills (`project-constitution`, `spec-design`, `spec-write`, `spec-execute`, `spec-review`, `spec-amend`) audited for principle compliance — sibling finding per non-compliant skill (closed immediately if compliant). Not in this cascade; separate work.
- **Methodology gap follow-on:** Constitution-amendment workflow gap (`spec-amend` does not formally target constitution docs; tech-stack.md has no journal) captured as a follow-on advisory finding after the cascade completes.

**Status implication.** Kept as Living document. The principle is additive — no existing commitment is reversed; the change extends `Tooling Conventions` rather than reopening it.

**Approver.** waseric, 2026-05-17.

## 2026-05-17 — Routed: spec-amend cascade across five specs implementing the Atomic-Skill Portability Principle

**Decided by:** waseric; developer; persona-frame: investigation
**Prior status:** `under-investigation`
**New status:** `routed`
**Route subtype:** spec-amend
**Target spec (if amend or new-spec):** [specs/tech-stack.md](../../tech-stack.md) (primary). Cascade also amended [specs/20260517-findings-pipeline/architecture.md](../../20260517-findings-pipeline/architecture.md), [specs/20260517-findings-pipeline-schema/feature.md](../../20260517-findings-pipeline-schema/feature.md), [specs/20260517-finding-intake-skill/feature.md](../../20260517-finding-intake-skill/feature.md), [specs/20260517-finding-triage-skill/feature.md](../../20260517-finding-triage-skill/feature.md).
**Rationale:** The investigation reframed the coupling symptoms (path resolution + line-number stripping) as effects of a missing methodology-level commitment that skills are portable atomic units. The remedy is therefore not narrow file edits but a methodology-wide commitment, with cascading implementation choices threaded through the design spec, schema feature spec, the two SKILL.md files, and the two template files. `spec-amend` is the correct route because every change is surgical against an existing spec or skill; no new feature spec is needed.
**Cascade landed (all in-session, 2026-05-17):**
- Amendment 1 — `tech-stack.md` 2026-05-17-1 — Atomic-Skill Portability Principle committed in §Tooling Conventions. Commit `b515c71`; SHA backfill `55c6df6`.
- Amendment 2 — `findings-pipeline/architecture.md` 2026-05-17-5 — §6 NFR row for Skill portability + README-as-derived-projection clarification. Commit `d8d87d6`; SHA backfill `b5c4f02`.
- Amendment 3 — `findings-pipeline-schema/feature.md` 2026-05-17-2 — schema NFR + §5.2 + §5.3 + cascading edits to `_template/finding.md` and `_template/journal.md` (scaffold-marker delimiters). Commit `38a4afb`; SHA backfill `7faeb8a`.
- Amendment 4 — `finding-intake-skill/feature.md` 2026-05-17-4 — feature spec NFR + §5.1; cascading conformance edits to `.agents/skills/finding-intake/SKILL.md` (ORIENT, APPLY 2-3, WHAT NOT TO DO) + bundled `_template/` in skill directory. Commit `651fbd8`; SHA backfill `83bf60b`.
- Amendment 5 — `finding-triage-skill/feature.md` 2026-05-17-2 — feature spec NFR; cascading conformance edits to `.agents/skills/finding-triage/SKILL.md` (ORIENT step 1) + bundled `_template/` in skill directory. Commit `45a81a9`; SHA backfill `19597d5`.
**Notes:** The formal `under-investigation → routed` transition was journaled here at cascade end (rather than at cascade start) to align "route is final" with "the work the route initiates has landed" — the route's evidence is the cascade itself. Two follow-on items surfaced during execution and remain to be addressed: (1) **Audit of spec-* skills** (`spec-write`, `spec-amend`, `spec-design`, `spec-execute`, `spec-review`, `project-constitution`) for principle compliance — sibling finding per non-compliant skill, closed immediately if compliant; (2) **Constitution-amendment workflow gap** — `/spec-amend`'s documented scope names design and feature specs, not constitution docs; pragmatic resolution for this work was informal use against `tech-stack.md`; advisory finding to be filed next. Neither follow-on blocks the routing of this finding.
