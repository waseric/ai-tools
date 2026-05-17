# ai-tools — Tech Stack

> Audience: Eric Wasgatt (author); AI coding agents consuming the methodology's artifacts; engineers evaluating or adopting the methodology
> Status: Living document — last updated 2026-05-15

## Languages and Runtimes

- **Markdown** — all methodology artifacts, specs, journals, and supporting documentation. No executable runtime required.
- **YAML** — frontmatter in `.agents/skills/` SKILL.md files (skill metadata: name, description, lastUpdated). Parsed by VS Code / Copilot skill loading.

## Frameworks and Major Libraries

None. The repo contains no executable code at this time. Methodology artifacts may include supporting scripts in the future; this section will be updated when they do.

## Tooling Conventions

- **AI agent skill format** — `.agents/skills/<skill-name>/SKILL.md` with YAML frontmatter. Consumed by VS Code GitHub Copilot and Claude Code. Each skill is self-contained and independently readable.
- **Claude Code configuration** — `.claude/settings.local.json` for repo-scoped agent permissions.
- **No linter, formatter, or test runner** — prose artifacts are reviewed manually or via agent-assisted review (`spec-review` skill).

### Atomic-Skill Portability Principle

Each skill in `.agents/skills/<skill-name>/` is a **portable atomic unit**. Operationally:

1. **Self-contains its workflow, schema knowledge, and default templates.** The skill's behavior is determined by what ships in its own directory (`SKILL.md`, any bundled assets such as `_template/`, embedded schema understanding in prose) plus the conventions the skill explicitly declares. No runtime dependency on host-repo files for the skill to function.
2. **Discovers and adapts to richer methodology embodiments when present in the host context.** Host-repo conventions the skill recognizes — a project's `specs/findings/` storage directory, a project-supplied `_template/` override, sibling skills, design-spec references — augment the skill's behavior when present. They are inputs the skill *uses*, not preconditions it *requires*.
3. **Degrades cleanly when those embodiments are absent.** A skill installed globally (e.g. `~/.claude/skills/<name>/`) and invoked against an unrelated repo must work — producing output that conforms to the skill's bundled schema and templates, not silently failing because host-relative paths do not resolve.

The principle is methodology-wide: it applies to every skill in `.agents/skills/`, including `project-constitution`, `spec-design`, `spec-write`, `spec-execute`, `spec-review`, `spec-amend`, `finding-intake`, `finding-triage`, and any future siblings. New skills are authored against this principle from the start; existing skills are audited for compliance and brought into conformance via [spec-amend](../.agents/skills/spec-amend/SKILL.md) when gaps are found.

**Why.** Skills are the methodology's distributable surface. Outside adopters install the skill, not the repo that authored it; their host project may or may not have any of the methodology's reference docs, templates, or sibling skills present. A skill that silently requires the host to mirror the authoring repo's layout is not portable, regardless of how well it works in the authoring context.

**Originating finding.** [specs/findings/20260517-intake-template-folder-dependency/](findings/20260517-intake-template-folder-dependency/finding.md).

## Hosting and Deployment

- **GitHub** — `waseric/ai-tools`. Single branch (`main`), no CI/CD pipeline.
- **No deployment target** — the methodology is consumed by cloning, forking, or referencing skill files. No build step, no release process.

## Constraints

- **Corporate TLS interception** — the author's primary development environment (Windows 11 Enterprise, corporate-managed) terminates TLS at the perimeter. Tools using bundled CA stores require system-CA configuration. This constraint applies when methodology artifacts include scripts that fetch external resources.
- **No local admin** — the author's corporate machine lacks administrator rights. Tooling must be user-scope installable. This shapes recommendations in methodology artifacts that reference tooling setup.
- **AI context window limits** — methodology artifacts are consumed by LLM agents with finite context windows. Conciseness is a hard constraint on artifact length, not just a style preference.

## Conventions Outside the Stack

- **Repository layout** — `specs/` for authoritative artifacts (constitution, design specs, feature specs, journals). `docs/` for supporting material (research, recommendations, retrospectives, conversation exports). `.agents/skills/` for methodology artifacts in skill format.
- **Commit messages** — descriptive, present-tense summaries. No enforced conventional-commits format at this time.
- **Branch strategy** — single `main` branch. Feature branches when warranted by scope; not currently in use.
- **Spec-driven development** — changes to the methodology itself follow the methodology: design spec → feature spec → execution → review. The repo eats its own cooking.
