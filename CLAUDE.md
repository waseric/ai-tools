# ai-tools — Claude Code Context

## Orientation

This is **ai-tools**, the master repository for the operator's AI/SDLC skill family (the `spec-*`, `finding-*`, and `project-constitution` skills). Skills are authored and governed here; they are *used* from consuming projects across the operator's work and hobby contexts. Content in this repo must not name specific consumers or assume any consumer is available.

- Skill masters live at `.agents/skills/<name>/SKILL.md` — `.agents/` is the canonical, harness-agnostic home for skills authored in this repo.
- Agent-definition masters live at `.agents/agents/<name>.md` — the worker/reviewer subagent types dispatch-mode execution spawns (`spec-worker`, `spec-reviewer`). Unlike skills, agent definitions are **declared harness adapters**: they are the one artifact class in this repo allowed to carry harness-specific frontmatter (`tools`, `disallowedTools`, `effort`, etc.), because the harness resolves subagent types from these files. Model is deliberately *not* pinned in the definition — it is set per-spawn to the task's model floor. See dispatch-execution §5.7.
- Common frontmatter, uniform across all skills: `name`, `lastUpdated`, `description` — and nothing else. Do not add harness-specific keys (e.g. Claude Code's `model:` or `allowed-tools:`) to skill masters; harness-specific behavior belongs in the skill's prose contract (INPUTS, model floors) so the master stays portable. (This prohibition is skill-specific — agent definitions are the deliberate exception above.)
- Each skill has a governing spec at `specs/YYYYMMDD-<name>-skill/` (`architecture.md` or `feature.md` + `journal.md`).
- `docs/` holds pre-spec research and strategy notes — input material, not authoritative doctrine.

## The deploy-sync rule

Masters live here; the live deploy copies live under `~/.claude/`. The rule spans two deliverable classes:

- **Skills:** master `.agents/skills/<name>/` → deploy `~/.claude/skills/<name>/`. Sync *every* file in the skill directory, not just `SKILL.md` — support files (e.g. `receipt-schema.md`) count, so the deploy-sync check must diff all of them.
- **Agent definitions:** master `.agents/agents/<name>.md` → deploy `~/.claude/agents/<name>.md`.

Every change to a master must be synced to its deploy copy in the same task closeout — a change is not done until both copies match. Never edit a deploy copy directly; if a deploy copy has drifted, the master wins and the drift is a finding.

## Skill-change workflow

Skills are spec-governed. Non-trivial changes to a SKILL.md route through `spec-amend` against that skill's governing spec (or a new spec via `spec-design`/`spec-write` for new skills or cross-cutting changes). Editing a skill without touching its governing spec is silent deviation — the same rule the skills themselves enforce.

Typo-class and formatting-only fixes may land directly with a descriptive commit.

## Design bar

Skills in this family are optimized for three properties, in tension: **token economy** (starve context, not verification), **batch autonomy** (operator-owned stops only), and **rework prevention** (mechanically re-derivable claims). Changes should state which of the three they serve and which they trade against. Consuming repos may build their own operating doctrine on these skills in their own context files; this repo owns the skill mechanics and never points at a specific consumer.

## Conventions

- Markdown links only — no `[[wikilinks]]` in new content.
- Work lands on `main` directly; no feature branches.
- Commit prefixes: `spec:` for spec/journal changes, `docs:` for docs/, `find:` for findings, `<skill-name>:` for skill-master changes (e.g. `spec-execute: add dispatch mode`), `<agent-name>:` for agent-definition changes (e.g. `spec-worker: tighten stop conditions`).
- Skill-master and agent-definition commits state the deploy-sync status (synced in same commit's closeout, or why not).
