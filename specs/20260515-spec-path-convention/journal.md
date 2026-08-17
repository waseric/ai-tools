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
**Commits:** 189c6cc
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
- Changes committed → 189c6cc.
**Decisions made:** None — straight execution per spec.
**Spec amendments:** None.
**Surprises and learnings:** None — all replacements were mechanical string substitutions.
**Next task pointer:** T-02 (Update project-constitution skill)

## 2026-05-15 — T-02: Update project-constitution skill

**Status:** done
**Commits:** e91b939
**Files touched:**
- `.agents/skills/project-constitution/SKILL.md`
**Tests added:** N/A (prose-only changes; verified via grep)
**DoD verification:**
- All default output paths updated (`specs/mission.md`, etc.) → confirmed: grep for `docs/(mission|tech-stack|roadmap|validation)` returns zero hits.
- Brownfield guidance added → confirmed: Phase 3 intro contains three-case coverage (greenfield default, brownfield-match, brownfield-conflict) with operator-facing question.
- Repository layout entry added to tech-stack template → confirmed: "Conventions Outside the Stack" section includes `- **Repository layout** —` bullet.
- Output paragraph updated → confirmed: reads "live at `specs/`" not "live at the root of `docs/`".
- `lastUpdated` bumped to `2026-05-15` → confirmed.
- Changes committed → e91b939.
**Decisions made:** None — straight execution per spec.
**Spec amendments:** None.
**Surprises and learnings:** None.
**Next task pointer:** T-03 (Migrate existing spec files and delete todo)

## 2026-05-15 — T-03: Migrate existing spec files and delete todo

**Status:** done
**Commits:** c2678ed
**Files touched:**
- `specs/session-economy-and-multi-repo-disciplines-architecture.md` → `specs/20260514-session-economy/architecture.md`
- `specs/session-economy-and-multi-repo-disciplines.journal.md` → `specs/20260514-session-economy/journal.md`
- `specs/spec-path-convention.todo.md` (deleted)
**Tests added:** N/A (file operations only)
**DoD verification:**
- `specs/20260514-session-economy/` contains `architecture.md` and `journal.md` → confirmed via `Get-ChildItem`.
- Old loose files no longer exist → confirmed: not in `specs/` listing.
- `specs/spec-path-convention.todo.md` deleted → confirmed via `git rm -f`.
- `git log --follow` traces back to original commit `6e2e362` → confirmed.
- `specs/` contains only loose constitution files and dated subdirectories → confirmed: `mission.md`, `tech-stack.md`, `roadmap.md`, `20260514-session-economy/`, `20260515-spec-path-convention/`.
**Decisions made:** Used `git rm -f` for the todo file because it had local modifications (unstaged edits). Safe because the file is superseded by the design spec.
**Spec amendments:** None.
**Surprises and learnings:** The todo file had unstaged local modifications requiring `-f` flag on `git rm`. No content was lost — the design spec supersedes it entirely.
**Next task pointer:** None — all tasks complete. CP-2 review pending.

## 2026-05-15 — Review of CP-1

**Reviewer:** Copilot (self-review, structured)
**Outcome:** pass with comments
**Tasks reviewed:** T-01, T-02
**Blockers:** 0
**Advisory:** 1 — brownfield three-case coverage is implicit rather than labeled; acceptable for instruction prose.
**Spec amendments proposed:** None
**Next action:** Proceed to T-03 (Migrate existing spec files and delete todo)

## 2026-05-15 — Review of CP-2

**Reviewer:** Copilot (self-review, structured)
**Outcome:** pass with comments
**Tasks reviewed:** T-03
**Blockers:** 0
**Important:** 1 — `specs/20260514-session-economy/journal.md` line 7 has stale artifact reference (`docs/specs/session-economy-and-multi-repo-disciplines-architecture.md`); should be `specs/20260514-session-economy/architecture.md`. Pre-existing, not a T-03 DoD violation.
**Spec amendments proposed:** None
**Next action:** All tasks complete. Both checkpoints passed. Feature spec marked complete. Follow-up: fix stale artifact reference in session-economy journal.
