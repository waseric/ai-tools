# ai-tools

A methodology for AI-assisted software development that spans the full product lifecycle — from greenfield instantiation and brownfield adoption through architecture, build, operation, iteration, transition, and retirement.

Rooted in SDLC, ITSM, and ITIL disciplines, it encodes a single principle: **autonomous development is enabled by governance, not by its absence.** The goal is output a product's owner can be accountable for, not output produced quickly without oversight.

This repository is the master for that methodology. The artifacts here are consumed by AI coding agents working in other projects.

## What's in here

| Path | Contents |
|---|---|
| `.agents/skills/<name>/SKILL.md` | Skill masters — the methodology as executable agent instructions |
| `.agents/agents/<name>.md` | Agent definitions for dispatch-mode execution (`spec-worker`, `spec-reviewer`) |
| `specs/` | Authoritative artifacts: constitution, design specs, feature specs, journals |
| `specs/findings/` | The findings pipeline — observations made outside a review checkpoint |
| `docs/` | Pre-spec research and strategy notes; input material, not doctrine |

## The skill family

The core is a lifecycle of skills that hand off to one another:

```
project-constitution → spec-design → spec-write → spec-execute → spec-review
                                          ↑                          │
                                          └──────── spec-amend ──────┘
```

- **`project-constitution`** — bootstraps a repo with mission, tech-stack, and roadmap or validation criteria
- **`spec-design`** — authors an architecture or protocol design spec, before any feature spec exists
- **`spec-write`** — authors a feature spec with an atomic task breakdown
- **`spec-execute`** — executes against a spec one task at a time, with closeout at each boundary
- **`spec-review`** — reviews work against a declared review checkpoint, producing a structured verdict
- **`spec-amend`** — applies a spec change when execution reveals the spec is wrong, instead of silently deviating

Alongside these, `finding-intake` and `finding-triage` capture and classify observations that don't yet have a home.

## Design principles

**Specs are self-contained.** A spec is written so a different person or agent can pick up the work without re-deriving intent. Journals record every status transition; nothing changes silently.

**Claims are mechanically re-derivable.** A task is not done because an agent says so — it's done because a named, re-runnable command proves it. Verification re-derives claims rather than trusting narrative.

**Skills are portable atomic units.** Each skill self-contains its workflow, schema knowledge, and templates. It adapts to richer host-repo conventions when they're present and degrades cleanly when they're absent, because adopters install the skill, not the repo that authored it.

**The repo eats its own cooking.** Changes to the methodology follow the methodology: design spec → feature spec → execution → review. The specs in `specs/` describing these skills were produced by the skills themselves.

## Using these skills

Skills are authored here and copied to wherever your agent harness loads them from. Nothing in this repo names or assumes any particular consuming project.

The methodology is deliberately tooling-agnostic — it prescribes no specific AI vendor, model, or platform. Where a skill refers to model capability, it does so through per-task *model floors* declared in the spec rather than by naming products.

## Status

Actively developed, single maintainer. The specs and journals are real working artifacts rather than illustrations, so they carry the untidiness of actual use: open questions, deferred decisions, and amendments where earlier thinking turned out to be wrong.

Contributions, adoptions, and outside feedback are welcome — the methodology generalizing beyond its origin is an explicit goal.

## License

MIT — see [LICENSE](LICENSE).
