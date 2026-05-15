# Spec Path Convention — Architecture and Protocol Specification

> Status: Draft — Open for Review
> Date: 2026-05-15
> Author: waseric + Copilot
> Audience: AI agents executing the methodology's skills; human contributors authoring or reviewing specs; engineers evaluating or adopting the methodology

## 1. Overview

The spec-driven development skill family (`spec-write`, `spec-execute`, `spec-review`, `spec-amend`, `spec-design`, `project-constitution`) currently defaults to `docs/specs/<feature>.md` as the conventional spec path. The ai-tools repo itself already diverges from this, placing authoritative artifacts under `specs/` and supporting material under `docs/`. This spec formalizes the path convention, adopts a per-spec subdirectory layout with date-prefixed naming, and propagates the convention across all six skills.

The architectural commitment is a two-folder convention (`specs/` for authoritative, `docs/` for supporting), a file-vs-directory boundary within `specs/` (constitution files are loose, work specs are subdirectories), and date-prefixed subdirectory naming for chronological sorting and age visibility.

## 2. Goals and Non-goals

**Goals:**

- Establish a methodology-wide default path convention: `specs/` for authoritative artifacts, `docs/` for supporting material.
- Adopt per-spec subdirectories with date-prefixed naming (`specs/YYYYMMDD-<short-name>/`) for work specs, keeping constitution files as loose `.md` files at `specs/` root.
- Update all six skills to reference the new convention as the default.
- Ensure the convention is a recommended default, not a hard constraint — repos with different layouts document their convention in the constitution, and the skills adapt.
- Define the artifact naming convention within subdirectories (artifact-type filenames: `architecture.md`, `feature.md`, `journal.md`).

**Non-goals:**

- Requiring all repos to use this layout. The methodology recommends it; repos may diverge if documented.
- Migrating existing specs in this or other repos as part of this design. Migration is a downstream activity.
- Changing the skill family's phase structure or behavior. All changes are default-path updates and guidance additions.
- Automating directory creation or layout enforcement via tooling.
- Prescribing how `docs/` is organized internally. Only the `specs/` convention is in scope.

## 3. Background and Constraints

### Current state

The ai-tools repo already practices the two-folder convention:

- `specs/` contains `mission.md`, `tech-stack.md`, `roadmap.md` (constitution), plus work spec files.
- `docs/` contains supporting material: recommendations, research, conversation exports, feedback.

But the skill files that define the methodology still reference `docs/specs/` as the default. This creates a mismatch: the methodology's own repo doesn't follow the methodology's stated defaults.

Work spec files currently live as loose files at `specs/` root (e.g., `session-economy-and-multi-repo-disciplines-architecture.md` alongside its journal). As the number of specs grows, this flat layout will become noisy — each spec produces 2–5 related files (spec, journal, review verdicts, amendments), and they interleave with constitution files.

### Prior art

ADR (Architecture Decision Record) conventions use flat directories with numbered files. RFCs and PEPs follow similar patterns. These work because each proposal is a single file. This methodology produces richer per-spec artifact families, which justifies subdirectories.

The date-prefix pattern (`YYYYMMDD-`) is used by ADR tooling (e.g., `adr-tools` uses `NNNN-` sequential numbering; date-prefixed variants exist in practice). The date provides chronological sorting and age-at-a-glance without requiring sequential number management.

### Constraints

- **AI context windows.** Shallower paths cost fewer tokens. `specs/20260515-session-economy/architecture.md` is comparable in length to `docs/specs/session-economy-and-multi-repo-disciplines-architecture.md` but carries more information (date, clear hierarchy).
- **Self-containment.** Each skill remains independently readable. The path convention is stated within each skill, not cross-referenced from a shared file.
- **Backward compatibility.** `SPEC_PATH` is user-settable in every skill. Changing the default doesn't break existing repos.

### Dependencies

This repo's constitution was established in the same session (2026-05-15). The constitution's `tech-stack.md` already references the `specs/` vs `docs/` layout convention. This design spec formalizes and extends it.

## 4. Architecture

### Two-folder convention

```
<repo-root>/
  specs/                          ← authoritative artifacts
    mission.md                    ← constitution (loose file)
    tech-stack.md                 ← constitution (loose file)
    roadmap.md                    ← constitution (loose file)
    20260514-session-economy/     ← work spec (dated subdirectory)
      architecture.md
      journal.md
    20260515-spec-path-convention/ ← work spec (dated subdirectory)
      architecture.md
      journal.md
  docs/                           ← supporting material
    feedback/
    research/
    recommendations/
```

### Structural boundary: file vs. directory

The boundary between constitution artifacts and work specs is **file vs. directory**:

- **Constitution artifacts** are loose `.md` files at `specs/` root. They are few (3–4), change slowly, and have no companion files.
- **Work specs** are date-prefixed subdirectories. Each contains the spec document, journal, and any review verdicts or amendment logs.

This boundary is immediately visible in any file browser or `ls` output. No intermediate grouping folder is needed (avoiding the naming problems documented in the rejected alternatives analysis).

### Date-prefixed subdirectory naming

Format: `YYYYMMDD-<short-name>/`

- **YYYYMMDD** is the spec's inception date (when the spec effort was first created, not when it was completed).
- **`<short-name>`** is a human-friendly identifier. Shorter than the spec's full title. Used for directory navigation and conversation reference.

Examples:
- `specs/20260514-session-economy/`
- `specs/20260515-spec-path-convention/`
- `specs/20260520-operational-readiness/`

Benefits: chronological sorting by default; age is visible at a glance ("that spec has been open for three months"); no sequential-number management across contributors.

### Artifact-type filenames within subdirectories

Since the subdirectory provides the spec's identity, filenames inside use artifact type only:

| Artifact | Filename |
|---|---|
| Design spec | `architecture.md` |
| Feature spec | `feature.md` |
| Journal | `journal.md` |
| Review verdict | `review-<checkpoint-id>.md` |
| Amendment log | `amendment-<id>.md` |

This avoids redundant naming (no `session-economy-architecture.md` inside a `session-economy/` directory).

### Vocabulary

- **Constitution artifacts**: `mission.md`, `tech-stack.md`, `roadmap.md` (or `validation.md`). Loose files at `specs/` root. Few, slow-changing, foundational.
- **Work spec**: A bounded effort with its own lifecycle (design spec, feature spec, defect spec). Lives in a dated subdirectory under `specs/`.
- **Spec family**: The set of related files produced by a work spec (spec document, journal, review verdicts, amendments). Co-located in the spec's subdirectory.
- **Date prefix**: `YYYYMMDD-` at the start of a work spec subdirectory name. Inception date of the spec effort.

## 5. Detailed Design

### 5.1 — Skill path defaults: update from `docs/specs/` to `specs/`

**Purpose.** Align the skills' default path examples with the methodology's convention.

**Interface.** Each skill's INPUTS block has `SPEC_PATH` and/or `JOURNAL_PATH` with example values. The output format sections reference default commit paths.

**Changes by skill:**

**`spec-execute`:**
- Description: `docs/specs/<feature>.md` → `specs/<feature>/feature.md`
- SPEC_PATH example: `docs/specs/feature-x.md` → `specs/YYYYMMDD-feature-x/feature.md`
- JOURNAL_PATH example: `docs/specs/feature-x.journal.md` → `specs/YYYYMMDD-feature-x/journal.md`

**`spec-write`:**
- DESIGN_SPEC_PATH example: `docs/specs/feature-x-architecture.md` → `specs/YYYYMMDD-feature-x/architecture.md`
- Output format: `docs/specs/<feature-name>.md` → `specs/YYYYMMDD-<feature-name>/feature.md`

**`spec-design`:**
- Output format: `docs/specs/<artifact-name>-architecture.md` → `specs/YYYYMMDD-<artifact-name>/architecture.md`

**`spec-amend`:**
- SPEC_PATH example: `docs/specs/feature-x.md` → `specs/YYYYMMDD-feature-x/feature.md`
- JOURNAL_PATH example: `docs/specs/feature-x.journal.md` → `specs/YYYYMMDD-feature-x/journal.md`

**`spec-review`:**
- SPEC_PATH example: `docs/specs/feature-x.md` → `specs/YYYYMMDD-feature-x/feature.md`
- JOURNAL_PATH example: `docs/specs/feature-x.journal.md` → `specs/YYYYMMDD-feature-x/journal.md`

**Why this design.** The examples are the primary teaching mechanism — agents and humans learn the convention from the examples more than from prose. Updating the examples propagates the convention naturally.

**Alternatives considered.** Adding a prose section explaining the convention while keeping old examples — rejected because examples override prose in practice.

### 5.2 — `project-constitution`: layout convention and placement

**Purpose.** The constitution skill produces the repo's foundational docs. It should place them at `specs/` by default and expose the layout decision to the operator when existing infrastructure exists.

**Changes:**

1. **Default output path**: `docs/mission.md` → `specs/mission.md` (and similarly for `tech-stack.md`, `roadmap.md`/`validation.md`).
2. **Phase 2 addition**: When the scan detects an existing directory structure that differs from the methodology's convention (e.g., `docs/` already contains specs, or a `specifications/` folder exists), surface the layout question to the operator: "The methodology recommends `specs/` for authoritative artifacts and `docs/` for supporting material. This repo has `<detected layout>`. Should I use the methodology's convention, adapt to the existing layout, or ask you to decide per-file?"
3. **Phase 3 addition**: When the skill creates the constitution, it adds a "Repository layout" subsection to `tech-stack.md` under "Conventions Outside the Stack" documenting the `specs/` vs `docs/` convention. When the layout differs from the methodology default, the divergence is documented here so downstream skills and agents can discover it without re-scanning.
4. **Brownfield guidance**: When existing infrastructure is present and the operator chooses a non-default layout, the constitution documents the exception. The skill does not silently rearrange files.

**Behavior:**
- **Greenfield (no existing structure)**: Use `specs/` + `docs/` convention without prompting. Inform the operator of the layout chosen.
- **Brownfield (existing structure matches convention)**: Use `specs/` + `docs/`. Note alignment in the output.
- **Brownfield (existing structure conflicts)**: Surface the question to the operator. Do not assume.

**Why this design.** The methodology should be opinionated but not rigid. Documenting exceptions in the constitution (rather than requiring agents to rediscover them) is cheaper than enforcing uniformity.

### 5.3 — Subdirectory layout convention in spec-producing skills

**Purpose.** When `spec-write` or `spec-design` produces a spec, the output should target a dated subdirectory, and the journal should be co-located.

**Changes to `spec-write`:**
- Output format: "suitable for committing as `specs/YYYYMMDD-<feature-name>/feature.md`"
- Add note: "Create a `journal.md` in the same directory. If a design spec exists upstream, it should already be in a sibling or parent spec directory referenced via `DESIGN_SPEC_PATH`."

**Changes to `spec-design`:**
- Output format: "suitable for committing as `specs/YYYYMMDD-<artifact-name>/architecture.md`"
- Add note: "Create a `journal.md` in the same directory."

**Changes to `spec-execute`:**
- The skill already accepts `SPEC_PATH` and `JOURNAL_PATH`. No structural change needed; the updated examples (§5.1) teach the convention.

**Changes to `spec-amend` and `spec-review`:**
- Updated examples (§5.1) are sufficient. These skills consume paths, they don't create directory structure.

### 5.4 — Archival convention

**Purpose.** Define what happens to completed specs.

**Convention:**
- **Default (in-place)**: Completed specs stay in their subdirectory. The status banner (`> Status: Complete`) is the signal. This is the cheapest option, preserves git history at the original path, and keeps links stable.
- **Physical separation (optional)**: If the `specs/` directory accumulates more than ~20 completed spec directories alongside active ones and becomes genuinely noisy, a `specs/_archive/` directory may be used. The underscore sorts it to the top as infrastructure, visually distinct from spec directories. Move completed spec subdirectories there wholesale.
- **No automatic archival.** Archival is a human decision, not a skill behavior.

**Why this design.** Premature archival breaks links and forces agents to search two locations. In-place with status banners is the simplest approach that works. Physical archival is an escape valve, not a default.

## 6. Non-functional Requirements

- **Adoptability.** The convention is a default change, not a behavioral change. Repos using `docs/specs/` continue to work — `SPEC_PATH` overrides apply. Migration is optional.
- **Discoverability.** Agents encountering a repo for the first time find the convention documented in `tech-stack.md` under "Conventions Outside the Stack." No scanning heuristics needed.
- **Conciseness.** Total new prose across all skills is under 30 lines. Most changes are example-path updates (a few characters each).
- **Reversibility.** If the convention proves wrong, reverting the skill defaults is a single commit. No data migration needed.

## 7. Implementation Sequencing

**Phase 1 — Skill path-default updates.** Update the 10 `docs/specs/` references across the five skills (`spec-execute`, `spec-write`, `spec-design`, `spec-amend`, `spec-review`) to the new convention. This is the core change.

> Downstream feature spec: *Spec Path Convention — Skill Updates*

**Phase 2 — `project-constitution` updates.** Change default output paths from `docs/` to `specs/`. Add brownfield detection and layout-question logic. Add "Repository layout" convention to `tech-stack.md` output template.

> Downstream feature spec: *Spec Path Convention — Constitution Skill Updates*

**Phase 3 — This repo migration.** Migrate existing spec files in `specs/` to the subdirectory convention. This is a concrete exercise of the convention and validates it.

> Downstream feature spec: *Spec Path Convention — Repo Migration*

Phases 1 and 2 are independent of each other. Phase 3 depends on Phase 1 (so the migrated paths match the updated defaults). All three phases are small enough that they may collapse into a single feature spec at the author's discretion.

## 8. Validation Approach

- **Self-test.** After applying skill edits, read each modified skill end-to-end. Verify path examples are consistent, no `docs/specs/` references remain in defaults, and prose reads naturally.
- **Invocation test.** Invoke `spec-design` or `spec-write` in a fresh session. Verify the agent proposes creating files under `specs/YYYYMMDD-<name>/` without manual path override.
- **Constitution test.** Invoke `project-constitution` on a new repo. Verify constitution files land at `specs/` and `tech-stack.md` includes the layout convention.
- **Brownfield test.** Invoke `project-constitution` on a repo with an existing `docs/specs/` layout. Verify the skill surfaces the layout question rather than silently overwriting.
- **Migration test.** After Phase 3, verify that all spec references within the repo (cross-references between specs, journal pointers) resolve correctly.

## 9. Review Checkpoints

### CP-1 — Skill path defaults updated

- **Trigger.** All five skills updated with new path examples and output format references.
- **Review focus.** Consistency of examples across skills. No stale `docs/specs/` references. Voice and formatting match surrounding prose.
- **Exit criteria.** Each skill's path examples and output format section reference the `specs/YYYYMMDD-<name>/` convention. No `docs/specs/` remains in default examples.

### CP-2 — Constitution skill updated

- **Trigger.** `project-constitution` skill updated with new default paths and brownfield logic.
- **Review focus.** Brownfield detection is exposed to the operator (not silently decided). Layout convention appears in `tech-stack.md` output. Greenfield path is smooth (no unnecessary prompting).
- **Exit criteria.** Constitution skill produces files at `specs/` by default. Brownfield divergence is documented in the constitution, not left implicit.

### CP-3 — Repo migration complete

- **Trigger.** Existing spec files in this repo migrated to subdirectory convention.
- **Review focus.** All internal cross-references resolve. Git history is preserved (git mv, not delete+create). Journal entries reference correct paths.
- **Exit criteria.** `specs/` directory contains only loose constitution files and dated subdirectories. No orphaned files.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Date prefix adds friction to directory creation | Low | Low | Agents and humans both know today's date; the format is unambiguous. Skills can auto-suggest the date. | Author |
| Existing repos with `docs/specs/` don't migrate | Medium | Low | The convention is a default, not a requirement. Old paths continue to work via `SPEC_PATH` override. | Per-repo owner |
| Subdirectory-per-spec is overkill for repos with few specs | Low | Low | The convention degrades gracefully — a repo with one spec has one subdirectory. No overhead beyond the directory itself. | N/A |
| Agents unfamiliar with the convention create files at wrong paths | Medium | Medium | The skill examples are the primary teaching mechanism; updated examples are sufficient for agents that read the skill. For agents without skill access, the constitution's layout section provides the convention. | Author |
| The design is wrong | Low | Medium | CP-1 review catches issues before propagation. The change is fully reversible (single commit revert). | Author |

## 11. Adoption Path

### New repos

Repos bootstrapped with `project-constitution` after this change automatically get the `specs/` convention. No action needed.

### Existing repos using the methodology

Two options:

1. **Migrate.** Rename `docs/specs/` contents to `specs/YYYYMMDD-<name>/` subdirectories. Update any hardcoded path references. This is a one-time effort proportional to the number of existing specs.
2. **Don't migrate.** Continue using `docs/specs/`. Set `SPEC_PATH` explicitly when invoking skills. The methodology recommends migration but does not require it.

### Reversibility

If the convention proves wrong:
- Revert the skill default changes (one commit).
- Repos that migrated can stay at `specs/` (the paths are valid regardless of what the skills default to) or migrate back.
- No data loss in either direction.

### Partial adoption

A repo could adopt `specs/` for new work while leaving existing specs at `docs/specs/`. The only cost is inconsistency, which is visible and documentable in the constitution.

## 12. Out of Scope

- **Internal organization of `docs/`.** Only the `specs/` convention is specified here.
- **Enforcing the convention via linting or CI.** The convention is advisory, taught by example and documented in the constitution.
- **Sequential numbering as an alternative to date prefixes.** Evaluated and rejected: sequential numbers require coordination across contributors and provide less information than dates.
- **Spec templates or boilerplate generators.** The skills themselves are the generators; no separate template system is needed.
- **Cross-repo spec referencing conventions.** How one repo's spec references another repo's spec is a separate concern (partially addressed by `SPEC_REPO_ROOT` in the multi-repo disciplines design).

## 13. Open Questions

### OQ-1 — Inception date vs. authoring date

**Question.** The date prefix uses the spec's inception date. For specs that emerge from extended discussion before formal authoring, should the date be when the discussion started, or when the spec file was first created?

**Analysis.** The inception date's purpose is sorting and age visibility. Discussion-start dates may be hard to pin down (scattered across chat sessions). File-creation date is unambiguous and automatable.

**Leaning.** File-creation date — the date the spec directory is first created. Simple, unambiguous, automatable. If a spec sat in discussion for weeks before formalization, that context belongs in the journal, not the directory name.

**Owner.** Resolved here; convention documented in §4.

### OQ-2 — What if two specs are created on the same date with similar names?

**Question.** The `YYYYMMDD-<short-name>` pattern could theoretically collide if two specs with similar short names start on the same day.

**Analysis.** In practice, spec inception is infrequent enough (days to weeks apart) that date collisions are rare. When they occur, distinct short names resolve the ambiguity. True collisions (same date, same short name) would require the author to differentiate the short name.

**Leaning.** No special handling. The short-name portion provides sufficient disambiguation. If a collision occurs, the author picks a more specific short name. This is a human judgment, not a system rule.

**Owner.** Resolved here; no downstream action.

### OQ-3 — Should the journal filename vary by spec type?

**Question.** Currently all spec types use `journal.md`. Should design specs use `architecture-journal.md` to distinguish from feature spec journals in the same directory?

**Analysis.** A spec subdirectory typically contains one spec document (either `architecture.md` or `feature.md`, rarely both). When a design spec spawns a feature spec, the feature spec typically gets its own subdirectory. Co-location of both in one directory is uncommon.

**Leaning.** Keep `journal.md` universally. The directory context disambiguates. If both a design spec and feature spec share a directory (unusual), the journal covers the full effort — it doesn't need splitting by spec type.

**Owner.** Resolved here; no downstream action.

## 14. References

- **Authoritative:**
  - `specs/mission.md` — ai-tools repo mission, establishing the methodology's scope
  - `specs/tech-stack.md` — ai-tools repo tech stack, documenting the `specs/` vs `docs/` convention
  - `.agents/skills/spec-execute/SKILL.md` — primary skill with path defaults
  - `.agents/skills/spec-write/SKILL.md` — spec authoring skill with output format
  - `.agents/skills/spec-design/SKILL.md` — design spec authoring skill with output format
  - `.agents/skills/spec-amend/SKILL.md` — amendment skill with path examples
  - `.agents/skills/spec-review/SKILL.md` — review skill with path examples
  - `.agents/skills/project-constitution/SKILL.md` — constitution skill with output paths
- **Inspirational:**
  - ADR (Architecture Decision Record) conventions — flat-directory, numbered-file pattern for single-file decisions
  - `specs/spec-path-convention.todo.md` — the analysis document that preceded this spec, containing rejected alternatives and the subdirectory recommendation
