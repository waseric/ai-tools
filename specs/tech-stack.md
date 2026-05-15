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
