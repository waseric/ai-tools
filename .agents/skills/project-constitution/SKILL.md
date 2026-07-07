---
name: project-constitution
lastUpdated: 2026-07-07
description: Bootstrap a new (or recently created) repo with a constitution — the three-part shape that orients contributors, AI agents, and stakeholders before any feature or design spec is written. Produces `mission.md` (the why), `tech-stack.md` (the how), and conditionally `roadmap.md` (planned phases) or `validation.md` (done criteria), depending on the repo's lifecycle stage. Scans for existing signals (package manifests, READMEs, framework markers, CI config) before prompting for what the scan cannot determine. Use when starting a new repo, adopting an inherited repo, or formalizing a repo whose intent has drifted from its docs. Pairs with `spec-design` (downstream — references the constitution).
---

# Project Constitution

Bootstraps a repo with three coordinated documents that establish *what this repo is for*, *what it's built with*, and *where it's going* (or *how we'll know it's done*). The constitution is the upstream input to every design and feature spec that follows.

A repo without a constitution forces every contributor (human and agent) to re-derive scope, stack, and intent from scattered signals. A repo with a constitution converges contributors on shared vocabulary and orientation before any work begins.

## How this skill works

When invoked, you act as the agent. Scan the repo for existing signals first; ask the user only for what the scan cannot determine. Then run Phase 1 (Scan), pause at Phase 2 for user input on purpose, audience, and lifecycle stage, and produce the Phase 3 documents.

The output is a set of small markdown documents that live at `specs/` (or the repo root, if the repo uses a different convention).

## INPUTS

```
REPO_ROOT: <path or repo URL>
REPO_PURPOSE: <one or two sentences on what this repo is for; may be empty>
TARGET_AUDIENCE: <named groups who will read these docs; e.g. "contributors, AI agents, stakeholders">
LIFECYCLE_STAGE: <new | growing | mature | inherited; affects whether roadmap or validation is produced>
STACK_HINTS: <known stack choices; may be empty — scan will surface most>
SCOPE_HINTS: <what is in / out of scope, if you already know>
```

---

# ROLE

You are an architect bootstrapping a repo with its founding documents. Your job is not to design features, not to write code, and not to plan releases. Your job is to produce a small, durable set of documents that any future contributor — human or agent — can read in five minutes and understand what this repo is, what it's built with, and where it's heading.

The constitution is short. It is read often. It rots if neglected. Keep it minimal so it stays accurate.

# OPERATING PRINCIPLES

1. **Scan before asking.** Most stack and structure signals are visible in the repo. Ask only for what the scan cannot determine (purpose, audience, lifecycle stage, scope).
2. **Three documents, not thirty.** Resist the urge to produce architecture notes, contributing guides, code-of-conduct, security policies, etc. Those belong in their own files when needed. The constitution is the three core docs only.
3. **Lifecycle determines the third doc.** `new` and `growing` repos get a roadmap (forward-looking phases). `mature` and `inherited` repos get a validation doc (done criteria, or "what good looks like" for the existing system). Pick one, not both.
4. **Cite, don't assume.** Stack claims must be backed by a file in the repo (`package.json`, `pyproject.toml`, `Cargo.toml`, framework config). Do not assert a stack the repo doesn't actually use.
5. **Audience-named.** Every doc opens with the audience it's written for. Different audiences want different specificity; declaring the audience prevents downstream noise.
6. **Short.** Each doc fits comfortably on one screen when reasonable. Mission ≤ 30 lines, tech-stack ≤ 60 lines, roadmap/validation ≤ 80 lines. If you exceed these, ask whether the content belongs in a different artifact.
7. **No fictional history.** Do not narrate the repo's origin, evolution, or design rationale unless the user supplied it. The constitution is forward-orienting, not biographical.

# PHASE 1 — SCAN (do this first, before any prompting)

Walk the repo and produce a Scan Report covering:

- **Manifest files.** `package.json`, `pyproject.toml`, `requirements.txt`, `Gemfile`, `Cargo.toml`, `go.mod`, `pom.xml`, `*.csproj`, `composer.json`, etc. List languages and major dependencies.
- **Framework markers.** `next.config.js`, `vite.config.*`, `webpack.config.*`, `tsconfig.json`, `tailwind.config.*`, `tox.ini`, `nx.json`, etc.
- **Repository structure.** Top-level directories. Identify obvious shapes (monorepo, packages/, apps/, services/, docs/, scripts/, .github/workflows/).
- **CI configuration.** `.github/workflows/`, `.gitlab-ci.yml`, `azure-pipelines.yml`, `Jenkinsfile`. Note test commands, deployment targets.
- **Existing docs.** Any `README*`, `CONTRIBUTING*`, `ARCHITECTURE*`, `ROADMAP*`, `docs/` content. The constitution should not duplicate or contradict these — read them first.
- **Git signals.** Recent commits, branches, contributors (high-level: solo vs. team-sized). Use `git log --oneline -20` and `git branch -a` rather than reading every commit.
- **`.agents/skills/` or similar.** If skills are present, the repo likely has prior AI-agent collaboration conventions worth referencing.

State which scan results you are confident about and which need user confirmation in Phase 2.

# PHASE 2 — CLARIFY (pause here)

Output the following, then **stop and wait for user input**:

- **Lifecycle stage confirmation.** `new` (just created, no shipping product), `growing` (active development, intent in flux), `mature` (stable, well-understood), `inherited` (existing repo whose intent is being formalized for the first time). This determines whether the third doc is `roadmap.md` or `validation.md`.
- **Purpose statement.** Confirm or refine `REPO_PURPOSE`. Ideal length: one or two sentences answering "what is this repo for?"
- **Audience confirmation.** Who will read these docs? Name the broadest member explicitly.
- **In-scope / out-of-scope.** The non-goals list is doing real work — it stops the repo from accreting unrelated work. Capture explicit non-goals.
- **Layout confirmation.** When Phase 1 detected a non-default authoritative-artifacts directory (e.g., `docs/` already contains specs, or a `specifications/` folder exists), surface the choice: "The methodology recommends `specs/` for authoritative artifacts and `docs/` for supporting material. This repo has `<detected layout>`. Should I use the methodology's convention, adapt to the existing layout, or ask you to decide per-file?" Skip when scan detected the default `specs/` shape.
- **Stack confirmation.** From Scan, list what was found. Ask the user to confirm or correct (occasionally a manifest is present but the actual stack is different — e.g., a `package.json` left from a prior life).
- **Assumptions.** Anything you intend to infer if the user doesn't respond.
- **Open questions** with `[blocker]` / `[important]` / `[minor]` triage. Blockers must be resolved before Phase 3.

# PHASE 3 — DOCUMENTS

Produce two or three small markdown documents. Use the exact headings below. Place them at `specs/mission.md`, `specs/tech-stack.md`, and either `specs/roadmap.md` or `specs/validation.md`. If the repo uses a different convention, place at the appropriate location and note in the journal.

When the operator chose a non-default authoritative-artifacts layout in Phase 2, document the exception in the constitution (in `tech-stack.md` under "Conventions Outside the Stack — Repository layout") in addition to the journal entry of the originating session.

## specs/mission.md

```markdown
# <Repo name> — Mission

> Audience: <named groups>
> Status: Living document — last updated <YYYY-MM-DD>

## Purpose
<One or two sentences. What this repo exists to do.>

## Audience
<Who this repo serves, and what they get from it. Be specific — "developers" is too broad; "backend engineers maintaining the payments API" is right-sized.>

## In Scope
<Bulleted. What kinds of work belong here.>

## Out of Scope
<Bulleted. What kinds of work do NOT belong here, even if related. This list is doing real work; resist the urge to leave it short.>

## Success
<One paragraph. How would we know, six or twelve months in, that this repo is working?>
```

## specs/tech-stack.md

```markdown
# <Repo name> — Tech Stack

> Audience: <named groups>
> Status: Living document — last updated <YYYY-MM-DD>

## Languages and Runtimes
<Bulleted. Cite manifest files. Note version pins where they matter.>

## Frameworks and Major Libraries
<Bulleted. Group by area (UI / data / testing / build / deploy). Cite the config or manifest file for each.>

## Tooling Conventions
<Bulleted. Linter, formatter, type-checker, test runner, package manager, monorepo tooling.>

## Hosting and Deployment
<Where this runs in production. CI/CD targets. Environments. Cite workflow or pipeline files.>

## Constraints
<Environmental constraints that shape engineering choices. Examples: corporate TLS interception, no-admin Windows, regulatory regimes, on-prem requirements. Pull from user-global context if relevant.>

## Conventions Outside the Stack
<Anything tooling-adjacent: branch naming, commit-message format, PR template, secret handling, env-var prefixes. Keep this short — link to a CONTRIBUTING.md if more detail is needed.>

- **Repository layout** — `specs/` for authoritative artifacts (constitution, design specs, feature specs, journals). `docs/` for supporting material (research, recommendations, retrospectives). Document any divergence from this convention here.
- **Grammar (optional).** A constitution *may* declare a `## Grammar` block here — a repo-wide house-style anchor dialect for spec and journal headings (journal entry, task closeout, review, amendment, spec section, task block). This is never mandated: absent a declared block, every skill falls back to its own native default dialect. When declared, early writers (`spec-write`, `spec-design`) read it during Discovery and codify it forward into each spec they author — a free rider on the constitution read they already perform, not an extra read — so later skills consult the spec's copy rather than re-reading the constitution.
```

## specs/roadmap.md (for `new` and `growing` repos)

```markdown
# <Repo name> — Roadmap

> Audience: <named groups>
> Status: Forward-looking — last updated <YYYY-MM-DD>

## Phase 1 — <Short name>
**Outcome:** <What is true at the end of this phase>
**Timing:** <Rough target, calendar or sprint>
**Key deliverables:** <Bulleted, named>
**Exit criteria:** <Bulleted, objective>

## Phase 2 — <Short name>
<Same shape as Phase 1>

## Phase N — <Short name>
<Same shape>

## Out of Roadmap
<Items that have been raised but explicitly deferred or rejected. One line each with rationale.>
```

Roadmaps describe phases, not features. Each phase produces an outcome the next phase consumes. Resist the urge to enumerate every backlog item.

## specs/validation.md (for `mature` and `inherited` repos)

```markdown
# <Repo name> — Validation

> Audience: <named groups>
> Status: Living document — last updated <YYYY-MM-DD>

## What Good Looks Like
<One paragraph. The healthy steady state for this repo: build green, tests passing, deploys clean, no chronic issues. This is the bar the repo holds itself to.>

## Acceptance Signals
<Bulleted. Concrete signals that the repo is doing its job. E.g., "alert notifications dispatched within 60 seconds of source event," "CI green on `main` for 95% of weekdays in the trailing 30 days.">

## Known Gaps
<Bulleted. Honest list of where the repo currently falls short of the bar above. Each item names an owner or routes to a downstream spec.>

## Done Criteria for In-Flight Work
<If specific in-flight work has a "done" definition that lives at the repo level, capture it here. Otherwise omit this subsection.>
```

# OUTPUT FORMAT

- Phase 1 (Scan) and Phase 2 (Clarify) are conversational.
- Phase 3 produces two or three small markdown files. Each is self-contained: a reader who has not read the others can still get value from one.
- All links follow the portability rule (no absolute filesystem paths).
- No marketing language. Plain declarative prose.

# WHAT NOT TO DO

- Do not produce a CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md, or governance docs. Those are separate artifacts; the constitution is the three core docs only.
- Do not narrate the repo's history unless the user supplied it. Forward-orient.
- Do not enumerate every backlog item in the roadmap. Phases, not features.
- Do not assert a stack the manifest files don't support. If `package.json` exists but the project is actually Python, ask before writing tech-stack.md.
- Do not produce both `roadmap.md` and `validation.md`. Pick one based on lifecycle stage. A repo can graduate from roadmap to validation later; that is an amendment, not a constitution rewrite.
- Do not exceed the soft line limits unless the repo genuinely demands it. Constitution rot starts with sprawling docs.
- Do not write the constitution as a chat-log transcript. No "as we discussed." No implicit shared vocabulary. Self-contained, every time.
- Do not duplicate or contradict existing READMEs without addressing them. If a README is stale, either update it as part of this work or note that the constitution supersedes it.

# HANDOFF NOTES

- **Downstream (`spec-design`).** Design specs reference the constitution in their §3 Background. The constitution provides vocabulary and constraint context the design spec assumes.
- **Downstream (`spec-write`).** Feature specs reference the constitution to scope work to in-scope domains and to identify the stack the implementation must match.
- **Updates.** The constitution is a living document. Routine drift (new framework, scope expansion, new phase) is updated in place. Major direction changes (mission rewrite, audience pivot) should go through `spec-amend` for visibility.
