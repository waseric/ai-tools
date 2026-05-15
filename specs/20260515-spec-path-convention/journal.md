# Spec Path Convention — Journal

## 2026-05-15 — Design Spec Authored

**Status:** done
**Artifact:** specs/20260515-spec-path-convention/architecture.md
**Origin:** specs/spec-path-convention.todo.md (analysis and recommendation document)
**Decisions made:**
- Two-folder convention (`specs/` authoritative, `docs/` supporting) confirmed as methodology-wide default.
- Per-spec subdirectory layout with date-prefixed naming (`YYYYMMDD-<short-name>/`).
- File-vs-directory boundary: constitution files are loose `.md` at `specs/` root; work specs are subdirectories.
- Artifact-type filenames within subdirectories (`architecture.md`, `feature.md`, `journal.md`).
- Constitution files stay at `specs/` root per operator confirmation — agents reliably discover them there.
- `project-constitution` exposes the layout question to the operator on brownfield repos; uses methodology default on greenfield without prompting.
- Layout exceptions documented in the constitution itself, not rediscovered per-session.
- Archival: in-place by default (status banner signals completion); physical `specs/_archive/` available as escape valve.
**Open questions resolved in spec:**
- OQ-1 (inception date): file-creation date, not discussion-start date.
- OQ-2 (same-day collisions): no special handling; short-name disambiguates.
- OQ-3 (journal filename per spec type): universal `journal.md`; directory context disambiguates.
**Surprises and learnings:** None at this phase — the todo document had already done thorough analysis.
**Next task pointer:** Implementation via downstream feature spec(s) per §7 Implementation Sequencing. Three phases: skill path-default updates, constitution skill updates, repo migration. May collapse into a single feature spec at author's discretion.

## 2026-05-15 — T-01: Update path references in spec-lifecycle skills

**Status:** done
**Commits:** 6d158fb
**Files touched:**
- `.agents/skills/spec-execute/SKILL.md`
- `.agents/skills/spec-write/SKILL.md`
- `.agents/skills/spec-design/SKILL.md`
- `.agents/skills/spec-amend/SKILL.md`
- `.agents/skills/spec-review/SKILL.md`
**Tests added:** N/A (prose-only changes; verified via grep)
**DoD verification:**
- 10 `docs/specs/` references replaced → confirmed: `grep docs/specs/ .agents/skills/**` returns zero hits.
- Journal guidance note added to `spec-write` OUTPUT FORMAT → confirmed: line reads "Create a `journal.md` in the same directory…"
- Journal guidance note added to `spec-design` OUTPUT FORMAT → confirmed: line reads "Create a `journal.md` in the same directory."
- `lastUpdated` bumped to `2026-05-15` on all 5 files → confirmed via read.
- Changes committed → 6d158fb.
**Decisions made:** None — straight execution per spec.
**Spec amendments:** None.
**Surprises and learnings:** None — all replacements were mechanical string substitutions.
**Next task pointer:** T-02 (Update project-constitution skill)

## 2026-05-15 — T-02: Update project-constitution skill

**Status:** done
**Commits:** dc7021f
**Files touched:**
- `.agents/skills/project-constitution/SKILL.md`
**Tests added:** N/A (prose-only changes; verified via grep)
**DoD verification:**
- All default output paths updated (`specs/mission.md`, etc.) → confirmed: grep for `docs/(mission|tech-stack|roadmap|validation)` returns zero hits.
- Brownfield guidance added → confirmed: Phase 3 intro contains three-case coverage (greenfield default, brownfield-match, brownfield-conflict) with operator-facing question.
- Repository layout entry added to tech-stack template → confirmed: "Conventions Outside the Stack" section includes `- **Repository layout** —` bullet.
- Output paragraph updated → confirmed: reads "live at `specs/`" not "live at the root of `docs/`".
- `lastUpdated` bumped to `2026-05-15` → confirmed.
- Changes committed → dc7021f.
**Decisions made:** None — straight execution per spec.
**Spec amendments:** None.
**Surprises and learnings:** None.
**Next task pointer:** T-03 (Migrate existing spec files and delete todo)
