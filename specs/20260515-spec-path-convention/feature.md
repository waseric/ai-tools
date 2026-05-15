# Spec Path Convention — Feature Spec

> Status: Draft — Open for Review
> Date: 2026-05-15
> Author: waseric + Copilot
> Design Spec: specs/20260515-spec-path-convention/architecture.md

## 1. Overview

Update all six methodology skills to use `specs/YYYYMMDD-<short-name>/` as the default spec path convention instead of `docs/specs/<feature>.md`, and migrate the ai-tools repo's existing spec files to match. This aligns the skills with the convention already practiced in the methodology's own repository and formalized in the design spec.

The work spans three categories: updating path examples in the five spec-lifecycle skills, updating the `project-constitution` skill's output paths and adding brownfield layout guidance, and migrating existing loose spec files into dated subdirectories.

## 2. Goals and Non-goals

**Goals:**

- All `docs/specs/` path references in skill defaults and examples are replaced with `specs/YYYYMMDD-<name>/` equivalents.
- `project-constitution` produces constitution files at `specs/` by default and documents the layout convention in its `tech-stack.md` output template.
- `spec-write` and `spec-design` output format sections reference dated subdirectories and include journal co-creation guidance.
- Existing loose spec files in this repo are migrated to dated subdirectories with git history preserved.
- `specs/spec-path-convention.todo.md` is deleted (superseded by the design spec).

**Non-goals:**

- Changing skill phase structure or behavioral logic.
- Adding tooling to enforce or automate the convention.
- Migrating specs in any repo other than ai-tools.
- Prescribing `docs/` internal organization.
- Adding journal-creation behavior to `spec-write` or `spec-design` (guidance-only).

## 3. Background and Constraints

- **Design spec**: specs/20260515-spec-path-convention/architecture.md — authoritative for vocabulary, convention shape, and brownfield behavior.
- **Constitution**: specs/mission.md, specs/tech-stack.md, specs/roadmap.md — this work is in-scope (Phase 1 deliverable: methodology skills operational and self-consistent).
- **Tech stack**: Markdown only. No build system, test runner, or CI. All validation is manual read-through and invocation testing.
- **Single repo**: Spec and code (skill files) live in the same repo. No multi-repo concerns for this feature.

## 4. Architecture

No new architecture. This feature updates prose content in existing skill files and moves existing files into subdirectories. The architectural decisions are made in the design spec; this feature spec decomposes them into executable tasks.

### Files touched

```
.agents/skills/spec-execute/SKILL.md      ← path example updates
.agents/skills/spec-write/SKILL.md        ← path example updates + journal guidance
.agents/skills/spec-design/SKILL.md       ← path example updates + journal guidance
.agents/skills/spec-amend/SKILL.md        ← path example updates
.agents/skills/spec-review/SKILL.md       ← path example updates
.agents/skills/project-constitution/SKILL.md ← output path + brownfield + layout convention
specs/session-economy-*                   ← migrated to specs/20260514-session-economy/
specs/spec-path-convention.todo.md        ← deleted
```

## 5. Detailed Design

### 5.1 — Spec-lifecycle skill path updates (T-01)

**Purpose.** Replace `docs/specs/` references with `specs/YYYYMMDD-<name>/` convention in five skills.

**Changes per skill:**

**spec-execute:**
- YAML description: `docs/specs/<feature>.md` → `specs/YYYYMMDD-<feature>/feature.md`
- SPEC_PATH example: `docs/specs/feature-x.md` → `specs/YYYYMMDD-feature-x/feature.md`
- JOURNAL_PATH example: `docs/specs/feature-x.journal.md` → `specs/YYYYMMDD-feature-x/journal.md`

**spec-write:**
- DESIGN_SPEC_PATH example: `docs/specs/feature-x-architecture.md` → `specs/YYYYMMDD-feature-x/architecture.md`
- OUTPUT FORMAT: `docs/specs/<feature-name>.md` → `specs/YYYYMMDD-<feature-name>/feature.md`
- Add note after output format path: "Create a `journal.md` in the same directory. If a design spec exists upstream, it should already be in a sibling or parent spec directory referenced via `DESIGN_SPEC_PATH`."

**spec-design:**
- OUTPUT FORMAT: `docs/specs/<artifact-name>-architecture.md` → `specs/YYYYMMDD-<artifact-name>/architecture.md`
- Add note after output format path: "Create a `journal.md` in the same directory."

**spec-amend:**
- SPEC_PATH example: `docs/specs/feature-x.md` → `specs/YYYYMMDD-feature-x/feature.md`
- JOURNAL_PATH example: `docs/specs/feature-x.journal.md` → `specs/YYYYMMDD-feature-x/journal.md`

**spec-review:**
- SPEC_PATH example: `docs/specs/feature-x.md` → `specs/YYYYMMDD-feature-x/feature.md`
- JOURNAL_PATH example: `docs/specs/feature-x.journal.md` → `specs/YYYYMMDD-feature-x/journal.md`

### 5.2 — Project-constitution skill updates (T-02)

**Purpose.** Update default output paths from `docs/` to `specs/` and add brownfield layout guidance.

**Changes:**

1. **Placement instruction** (Phase 3 intro): `docs/mission.md`, `docs/tech-stack.md`, `docs/roadmap.md` or `docs/validation.md` → `specs/mission.md`, `specs/tech-stack.md`, `specs/roadmap.md` or `specs/validation.md`. Update fallback: "If the repo does not use `docs/`" → "If the repo uses a different convention."

2. **Section headings**: `## docs/mission.md` → `## specs/mission.md`, and similarly for tech-stack, roadmap, validation.

3. **Template path references** inside code blocks: `docs/mission.md` → `specs/mission.md` etc.

4. **Brownfield guidance** — add to Phase 3 intro, after the placement instruction:

   > When the scan detects an existing directory structure that differs from the methodology's convention (e.g., `docs/` already contains specs, or a `specifications/` folder exists), surface the layout question to the operator: "The methodology recommends `specs/` for authoritative artifacts and `docs/` for supporting material. This repo has `<detected layout>`. Should I use the methodology's convention, adapt to the existing layout, or ask you to decide per-file?" When the operator chooses a non-default layout, document the exception in the constitution.

5. **Layout convention in tech-stack template** — add a "Repository layout" entry under the "Conventions Outside the Stack" section of the tech-stack template:

   ```markdown
   - **Repository layout** — `specs/` for authoritative artifacts (constitution, design specs, feature specs, journals). `docs/` for supporting material (research, recommendations, retrospectives). Document any divergence from this convention here.
   ```

6. **Output format paragraph**: Update "The output is a set of small markdown documents that live at the root of `docs/`" → "The output is a set of small markdown documents that live at `specs/`".

### 5.3 — Repo migration (T-03)

**Purpose.** Migrate existing loose spec files to dated subdirectories and clean up the superseded todo file.

**Changes:**

1. `git mv specs/session-economy-and-multi-repo-disciplines-architecture.md specs/20260514-session-economy/architecture.md`
2. `git mv specs/session-economy-and-multi-repo-disciplines.journal.md specs/20260514-session-economy/journal.md`
3. `git rm specs/spec-path-convention.todo.md`

The inception date `20260514` comes from the session-economy spec's status banner (`Date: 2026-05-14`). The short name `session-economy` follows the convention of being shorter than the full title while remaining unambiguous.

## 6. Non-functional Requirements

- **Adoptability.** All changes are default updates; `SPEC_PATH` overrides continue to work. No breaking change for existing repos.
- **Conciseness.** Total new prose across skills is under 30 lines. Most changes are example-path string replacements.
- **Reversibility.** A single revert commit undoes all skill changes. Migration can be reversed with `git mv` in the opposite direction.
- **Consistency.** After completion, zero `docs/specs/` references remain in any skill's default examples. Zero `docs/mission.md` (or similar) references remain in `project-constitution`.

## 7. Task Breakdown

### T-01 — Update path references in spec-lifecycle skills

**Scope.** Modify 5 files:
- `.agents/skills/spec-execute/SKILL.md`
- `.agents/skills/spec-write/SKILL.md`
- `.agents/skills/spec-design/SKILL.md`
- `.agents/skills/spec-amend/SKILL.md`
- `.agents/skills/spec-review/SKILL.md`

**Acceptance criteria.**
- Given any skill file, when searching for `docs/specs/`, then zero matches are found.
- Given `spec-execute`'s YAML description, when read, then the example path is `specs/YYYYMMDD-<feature>/feature.md`.
- Given `spec-execute`'s INPUTS block, when read, then SPEC_PATH example is `specs/YYYYMMDD-feature-x/feature.md` and JOURNAL_PATH example is `specs/YYYYMMDD-feature-x/journal.md`.
- Given `spec-write`'s INPUTS block, when read, then DESIGN_SPEC_PATH example is `specs/YYYYMMDD-feature-x/architecture.md`.
- Given `spec-write`'s OUTPUT FORMAT, when read, then the commit path is `specs/YYYYMMDD-<feature-name>/feature.md` and a journal co-creation note follows.
- Given `spec-design`'s OUTPUT FORMAT, when read, then the commit path is `specs/YYYYMMDD-<artifact-name>/architecture.md` and a journal co-creation note follows.
- Given `spec-amend`'s INPUTS block, when read, then SPEC_PATH example is `specs/YYYYMMDD-feature-x/feature.md` and JOURNAL_PATH example is `specs/YYYYMMDD-feature-x/journal.md`.
- Given `spec-review`'s INPUTS block, when read, then SPEC_PATH example is `specs/YYYYMMDD-feature-x/feature.md` and JOURNAL_PATH example is `specs/YYYYMMDD-feature-x/journal.md`.
- Each modified skill's `lastUpdated` YAML field is set to `2026-05-15`.

**Tests required.** Manual: grep all skill files for `docs/specs/`; expect zero hits. Read each modified example in context; confirm it reads naturally.

**Definition of Done.** All 10 `docs/specs/` references replaced. Journal guidance notes added to `spec-write` and `spec-design`. `lastUpdated` bumped. Changes committed. No stale references remain.

**Dependencies.** None.

**Estimated size.** S

---

### T-02 — Update project-constitution skill

**Scope.** Modify 1 file:
- `.agents/skills/project-constitution/SKILL.md`

**Acceptance criteria.**
- Given the Phase 3 placement instruction, when read, then it references `specs/mission.md`, `specs/tech-stack.md`, and `specs/roadmap.md` or `specs/validation.md`.
- Given the section headings, when read, then they are `## specs/mission.md`, `## specs/tech-stack.md`, `## specs/roadmap.md`, `## specs/validation.md`.
- Given the intro paragraph ("The output is…"), when read, then it says `specs/` not `docs/`.
- Given the Phase 3 intro, when read, then brownfield guidance is present describing three cases (greenfield, brownfield-match, brownfield-conflict) and instructs surfacing the layout question to the operator.
- Given the tech-stack template's "Conventions Outside the Stack" section, when read, then it includes a "Repository layout" entry.
- Given any location in the skill file, when searching for `docs/mission`, `docs/tech-stack`, `docs/roadmap`, or `docs/validation` as default paths, then zero matches are found (except in brownfield-detection prose that references `docs/` as an example of a detected existing layout).
- The skill's `lastUpdated` YAML field is set to `2026-05-15`.

**Tests required.** Manual: grep the skill file for `docs/mission`, `docs/tech-stack`, `docs/roadmap`, `docs/validation` in default-path context; expect zero hits. Read brownfield guidance; confirm three cases are covered. Read tech-stack template; confirm layout entry is present.

**Definition of Done.** All default output paths updated. Brownfield guidance added. Layout convention added to tech-stack template. `lastUpdated` bumped. Changes committed.

**Dependencies.** None (independent of T-01).

**Estimated size.** S

---

### T-03 — Migrate existing spec files and delete todo

**Scope.** Move and delete files under `specs/`:
- `specs/session-economy-and-multi-repo-disciplines-architecture.md` → `specs/20260514-session-economy/architecture.md`
- `specs/session-economy-and-multi-repo-disciplines.journal.md` → `specs/20260514-session-economy/journal.md`
- Delete `specs/spec-path-convention.todo.md`

**Acceptance criteria.**
- Given `specs/20260514-session-economy/`, when listed, then it contains `architecture.md` and `journal.md`.
- Given `specs/session-economy-and-multi-repo-disciplines-architecture.md`, when checked, then the file does not exist.
- Given `specs/session-economy-and-multi-repo-disciplines.journal.md`, when checked, then the file does not exist.
- Given `specs/spec-path-convention.todo.md`, when checked, then the file does not exist.
- Given `git log --follow specs/20260514-session-economy/architecture.md`, when run, then history traces back to the original file (git mv preserves history).
- Given the `specs/` directory listing, when read, then it contains only loose constitution files (`mission.md`, `tech-stack.md`, `roadmap.md`) and dated subdirectories.

**Tests required.** Manual: `ls specs/` and verify structure. `git log --follow` on migrated files to confirm history preservation.

**Definition of Done.** Files moved via `git mv`. Todo file deleted via `git rm`. Directory structure matches the design spec's §4 layout diagram. Changes committed.

**Dependencies.** T-01 (so that the migrated paths match the updated skill defaults, per design spec §7).

**Estimated size.** S

## 8. Test Strategy

- **Unit test approach.** N/A — no executable code.
- **Integration test approach.** N/A.
- **Manual verification.**
  1. After T-01: `grep -r "docs/specs/" .agents/skills/` returns zero hits.
  2. After T-02: `grep -r "docs/mission\|docs/tech-stack\|docs/roadmap\|docs/validation" .agents/skills/project-constitution/` returns zero hits in default-path contexts.
  3. After T-03: `ls specs/` shows only loose constitution files and dated subdirectories.
  4. After all tasks: read each skill file end-to-end; verify path examples are consistent and prose reads naturally.
- **Invocation test** (post-completion, not blocking): invoke `spec-design` or `spec-write` in a fresh session and verify the agent proposes `specs/YYYYMMDD-<name>/` paths without manual override.

## 9. Review Checkpoints

### CP-1 — All skill updates complete

- **Trigger.** T-01 and T-02 both done.
- **Review focus.** Consistency of path examples across all six skills. No stale `docs/specs/` or `docs/mission.md` references. Brownfield guidance is clear and covers three cases. Journal guidance notes in `spec-write` and `spec-design` are guidance-only (no behavioral change). Voice and formatting match surrounding prose.
- **Exit criteria.** Zero `docs/specs/` in skill defaults. Zero `docs/mission.md` (etc.) in `project-constitution` defaults. Each skill reads naturally end-to-end.

### CP-2 — Migration complete

- **Trigger.** T-03 done.
- **Review focus.** Git history preserved on migrated files. Directory structure matches design spec §4. No orphaned files. All cross-references within specs resolve (journal pointers, design spec references).
- **Exit criteria.** `specs/` contains only loose constitution files and dated subdirectories. `git log --follow` works on migrated files.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Stale `docs/specs/` reference missed in a skill | Low | Low | Grep verification in T-01 acceptance criteria catches any miss | Executor |
| Brownfield guidance prose is ambiguous | Low | Medium | CP-1 review focuses on clarity of three-case coverage | Reviewer |
| Git history lost during migration | Low | Medium | Use `git mv` exclusively, verify with `git log --follow` | Executor |
| External copies of skills (e.g. user-level `.claude/skills/`) not updated | Medium | Low | Out of scope — those are separate distributions. Note in journal for manual sync later | Author |

## 11. Rollout and Rollback

- **Rollout.** Direct commit to `main`. No feature flag needed — changes are prose updates to skill files.
- **Rollback.** `git revert` the commit(s). Old paths continue to work because `SPEC_PATH` is user-settable. Migration reversal: `git mv` files back to original locations.
- **Success signals.** Next invocation of any spec-lifecycle skill proposes `specs/YYYYMMDD-<name>/` paths by default.

## 12. Out of Scope

- Updating skill copies outside this repo (user-level `.claude/skills/`, other project repos that reference methodology skills).
- Enforcing the convention via linting or CI.
- Migrating specs in other repos.
- Adding journal-creation behavior (code) to `spec-write` or `spec-design` — guidance-only.
- Organizing `docs/` internally.
- Archival convention implementation (`specs/_archive/`) — defined in the design spec as an escape valve, not a default.

## 13. Open Questions

None. All design-level questions were resolved in the design spec.

## 14. References

- Design spec: specs/20260515-spec-path-convention/architecture.md
- Origin analysis: specs/spec-path-convention.todo.md (to be deleted in T-03)
- Constitution: specs/mission.md, specs/tech-stack.md, specs/roadmap.md
