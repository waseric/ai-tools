# ai-tools — Claude Code Context

## Orientation

This is **ai-tools**, the master repository for the operator's AI/SDLC skill family (the `spec-*`, `finding-*`, and `project-constitution` skills). Skills are authored and governed here; they are *used* from consuming projects across the operator's work and hobby contexts. Content in this repo must not name specific consumers or assume any consumer is available.

- Skill masters live at `.agents/skills/<name>/SKILL.md` — `.agents/` is the canonical, harness-agnostic home for skills authored in this repo.
- Common frontmatter, uniform across all skills: `name`, `lastUpdated`, `description` — and nothing else. Do not add harness-specific keys (e.g. Claude Code's `model:` or `allowed-tools:`) to masters; harness-specific behavior belongs in the skill's prose contract (INPUTS, model floors) so the master stays portable.
- Each skill has a governing spec at `specs/YYYYMMDD-<name>-skill/` (`architecture.md` or `feature.md` + `journal.md`).
- `docs/` holds pre-spec research and strategy notes — input material, not authoritative doctrine.

## The deploy-sync rule

Masters live here; the live deploy copies live at `~/.claude/skills/<name>/`. Every change to a skill master must be synced to its deploy copy in the same task closeout — a skill change is not done until both copies match. Never edit a deploy copy directly; if a deploy copy has drifted, the master wins and the drift is a finding.

## Skill-change workflow

Skills are spec-governed. Non-trivial changes to a SKILL.md route through `spec-amend` against that skill's governing spec (or a new spec via `spec-design`/`spec-write` for new skills or cross-cutting changes). Editing a skill without touching its governing spec is silent deviation — the same rule the skills themselves enforce.

Typo-class and formatting-only fixes may land directly with a descriptive commit.

## Design bar

Skills in this family are optimized for three properties, in tension: **token economy** (starve context, not verification), **batch autonomy** (operator-owned stops only), and **rework prevention** (mechanically re-derivable claims). Changes should state which of the three they serve and which they trade against. Consuming repos may build their own operating doctrine on these skills in their own context files; this repo owns the skill mechanics and never points at a specific consumer.

## Conventions

- Markdown links only — no `[[wikilinks]]` in new content.
- Work lands on `main` directly; no feature branches.
- Commit prefixes: `spec:` for spec/journal changes, `docs:` for docs/, `find:` for findings, `<skill-name>:` for skill-master changes (e.g. `spec-execute: add dispatch mode`).
- Skill-master commits state the deploy-sync status (synced in same commit's closeout, or why not).
