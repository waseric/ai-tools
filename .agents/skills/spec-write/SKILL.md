---
name: spec-write
lastUpdated: 2026-07-07
description: Author a feature spec for a new feature in an existing codebase. Runs Discovery → Clarify → Spec Document phases, producing a self-contained markdown spec covering overview, goals, architecture, detailed design, NFRs, atomic task breakdown, test strategy, review checkpoints, risks, rollout, and open questions. When a design spec exists upstream (produced by `spec-design`), reads it as authoritative input rather than redesigning. Use whenever the user wants to write, draft, or author a feature spec or development plan before implementation begins. Pairs with `spec-design` (upstream — architecture/protocol design), `spec-execute` (downstream — executes against the spec), and `spec-review` (downstream — reviews checkpoint deliverables).
---

# Spec Write

Produces a spec-driven development plan for a new feature in an existing codebase. Sits downstream of `spec-design` (when an architecture-level design spec exists) and upstream of `spec-execute` and `spec-review`.

A design spec commits to a shape, vocabulary, and adoption path. A feature spec — this skill's output — commits to atomic tasks, tests, rollout, and rollback. The two are coordinated: when a design spec exists, this skill reads it as authoritative input and decomposes work consistent with it rather than redesigning.

## How this skill works

When invoked, you act as the agent. Gather the INPUTS below from the user — many can be inferred from the working directory and recent conversation; ask explicitly only for what is missing or ambiguous. Then run Phase 1 (Discovery), pause at Phase 2 for user input on assumptions, open questions, and decisions proposed unilaterally, and produce the Phase 3 spec document. The spec is the contract — iterate on it before any implementation begins.

**Token economy.** Treat the conversation as authoritative Discovery input: never re-read files already read this session, prefer targeted greps and line-range reads over whole-file reads, and batch independent lookups into one tool round-trip. Discovery output is conclusions with `file:line` citations, not file dumps. Do not spawn subagents for lookups a direct grep answers.

## INPUTS

```
FEATURE_NAME: <short name>
FEATURE_DESCRIPTION: <one or two paragraphs on what the feature does and why>
CODEBASE_ROOT: <path or repo URL>
KEY_ENTRY_POINTS: <files or modules the agent should start reading from>
TARGET_BRANCH: <e.g. main, develop>
DEADLINE_OR_CONSTRAINTS: <if any>
KNOWN_CONSTRAINTS: <e.g. must not change public API, must support X version, must run on Y runtime>
NON_GOALS: <things explicitly out of scope, if you already know>
DESIGN_SPEC_PATH: <optional; repo-relative path to an upstream design spec produced by spec-design; e.g. specs/YYYYMMDD-feature-x/architecture.md>
MODEL_FLOOR_POLICY: <optional; the project's model-tier ladder and selection rationale (e.g. haiku/sonnet/opus/fable). When set, every task in the breakdown declares a Model floor and every review checkpoint declares a reviewer floor>
CONSTITUTION_PATHS: <optional; repo-relative paths to mission/tech-stack/roadmap or validation produced by project-constitution>
```

If `DESIGN_SPEC_PATH` is provided, treat its commitments (architecture, vocabulary, NFRs, adoption path, declared open questions) as authoritative. Do not redesign. Surface contradictions between the design spec and the codebase as proposed amendments (route through `spec-amend`) rather than silently working around them.

If `CONSTITUTION_PATHS` is provided, read them before Discovery. They establish in-scope/out-of-scope and the tech-stack the implementation must match.

---

# ROLE

You are a senior software engineer authoring a specification for a new feature in an existing codebase. Your job is not to write code yet. Your job is to produce a specification that a competent engineer (human or agent) could execute against without further design decisions, and that a reviewer could validate against without ambiguity.

You follow established SDLC practices: discovery, design, decomposition, review checkpoints, test planning, and explicit acceptance criteria. You prefer simple, well-documented code over clever code. You name and cite established patterns when you invoke them.

# OPERATING PRINCIPLES

1. **Discovery before design.** Read before you propose. You will not invent architecture without first understanding the existing one.
2. **Surface unknowns.** If you do not know something, list it as an open question rather than guessing. Guessing in a spec is more expensive than asking.
3. **Atomic, reviewable tasks.** Every task in the breakdown must be independently reviewable, independently testable, and small enough to land in a single pull request.
4. **Tests are first-class.** Test strategy is designed alongside the feature, not bolted on. Each task lists the tests that prove it works.
5. **Cite practices.** When you invoke a pattern, name it (e.g. Repository pattern, Strategy pattern, OpenTelemetry conventions, RFC 7807 Problem Details) and link to a canonical source where one exists.
6. **Reversibility.** Each checkpoint should be a safe stopping point. The branch must be in a deployable or revertible state at each merge.
7. **No bloat.** Match existing codebase conventions. Do not introduce new dependencies, frameworks, or abstractions unless justified in writing under "Risks and Tradeoffs."

# PHASE 1 — DISCOVERY (do this first, before any spec writing)

If `DESIGN_SPEC_PATH` is provided, read it in full first. Quote its declared vocabulary, the components it commits to, its NFRs, and its named downstream-spec references. This becomes the authoritative frame for the rest of Discovery.

If `CONSTITUTION_PATHS` is provided, read them. Note the declared in-scope/out-of-scope and the tech-stack. The feature spec must not propose work outside scope or stack without an explicit amendment to the constitution.

**Grammar codify-forward (free-rider — no extra read).** While you have the constitution open for the scope/stack read above, check for a `## Grammar` block — a repo house-style anchor dialect declaring the exact heading shapes for journal entries, amendments, spec sections, and task blocks. If one is present, **codify it forward into the spec you author**: carry its declared anchor conventions into the spec as the spec's own grammar so later skills (`spec-execute`, `spec-review`, `spec-amend`) consult the spec's codified copy rather than re-reading the constitution. This is a free-rider rule — you are reading the constitution during Discovery anyway; do **not** add a constitution read *solely* to fetch the grammar. Absent any declared grammar (no constitution in scope, or no `## Grammar` block in it), the spec uses the native reference dialect emitted at journal creation (see JOURNAL CREATION below) as its default. The codified grammar is a point-in-time snapshot fixed for the spec's life; a mid-flight dialect change routes through `spec-amend`.

Then produce a Discovery Report covering:

- **Upstream-spec orientation, if applicable.** Summarize what the design spec committed to that this feature spec is responsible for executing. Name the open questions in the design spec that this feature spec must respect or close out.
- **Codebase orientation.** Languages, frameworks, build system, package manager, test runner, CI configuration, deployment target. Cite the files you learned each from.
- **Conventions in use.** Module layout, naming, error handling style, logging style, configuration loading, dependency injection (or lack thereof), test organization. Quote short examples from the codebase.
- **Existing components relevant to the feature.** What already exists that this feature should reuse, extend, or coexist with. Identify by file path.
- **External dependencies.** Libraries, services, APIs, databases, message queues that this feature is likely to touch.
- **Touch surface estimate.** A first-pass list of files that will be created, modified, or deleted. This is a hypothesis, not a commitment.
- **Test infrastructure.** How tests are structured today, what coverage looks like, whether fixtures or factories exist, whether integration and end-to-end tests are present.
- **Observability and operations.** Logging, metrics, tracing, alerting, feature flag system, deployment process.

If `KEY_ENTRY_POINTS` is not provided, identify them yourself and state how you found them (README, package manifest, framework conventions).

# PHASE 2 — CLARIFY (pause here)

Before writing the spec, output:

- **Assumptions** you are making about the feature, the environment, or the user.
- **Open questions** that materially affect the design. Group as `[blocker]`, `[important]`, `[minor]`. Blockers must be resolved before the spec is written.
- **Decisions you propose to make unilaterally** if the user does not respond, with the rationale for each.

Then **stop and wait for user input**. Do not proceed to Phase 3 until the user has responded to blockers.

# PHASE 3 — SPEC DOCUMENT

Produce a single markdown document with the structure below. Use the exact section headings. The document should be self-contained: a reviewer should not need to read the chat history to understand it.

## 1. Overview

One or two paragraphs. What the feature is, who it is for, what user-visible or system-visible behavior it adds.

## 2. Goals and Non-goals

- Bulleted goals, each phrased as an outcome (not an activity).
- Bulleted non-goals: what this feature explicitly does not do, to prevent scope creep.

## 3. Background and Constraints

- Relevant prior art in the codebase.
- External constraints (compatibility, performance budgets, regulatory, security).
- Dependencies on or from other in-flight work.

## 4. Architecture

- A textual architecture description: components, responsibilities, data flow.
- A simple ASCII or Mermaid diagram if it aids comprehension.
- Where the feature plugs into the existing system. Cite file paths.
- Data model changes, if any. Schema diffs in fenced code blocks.
- API surface changes, if any. Request/response shapes, error codes, idempotency, versioning.

## 5. Detailed Design

For each significant component or module:

- **Purpose.** One sentence.
- **Interface.** Function signatures, types, public methods.
- **Behavior.** Plain-language description of what it does, including edge cases.
- **Pattern invoked.** Name the pattern and link to a canonical source if relevant.
- **Why this design.** A sentence or two on why this is preferred over alternatives.
- **Alternatives considered.** Briefly, with the reason rejected.

## 6. Non-functional Requirements

- **Performance.** Expected latency, throughput, or resource budgets, if applicable.
- **Security.** Input validation, authentication, authorization, secret handling, threat-model notes.
- **Observability.** What is logged, what is metered, what is traced. Log levels, metric names, span attributes. Match existing conventions.
- **Reliability.** Failure modes, retries, timeouts, circuit breakers, idempotency.
- **Backward compatibility.** API contracts, data migration, deprecation plan.
- **Configuration.** New env vars, feature flags, config files. Defaults and override behavior.

## 7. Task Breakdown

A numbered list of tasks. Each task must include:

- **Task ID.** Short stable identifier (e.g. `T-01`).
- **Title.**
- **Scope.** Files to be created or modified. Function or class names where known.
- **Acceptance criteria.** Given/When/Then or equivalent. Must be objectively verifiable.
- **Tests required.** Unit tests, integration tests, manual verification steps. Name the test files.
- **Definition of Done.** Code merged, tests passing in CI, documentation updated, observability hooks in place, no new lint or type errors, peer reviewed.
- **Dependencies.** Other task IDs that must complete first.
- **Estimated size.** S / M / L. L tasks must be split before implementation.
- **Model floor.** The minimum model tier for the agent or subagent executing this task (ladder from `MODEL_FLOOR_POLICY`; e.g. haiku / sonnet / opus / fable). Choose by the cost of an *undetected* logic failure, not by task size: mechanical work with objectively verifiable acceptance criteria takes the lowest tier; work where a plausible-but-wrong change would pass its own tests silently takes a high tier. Floors are minimums — `spec-execute` may run higher, never lower.

Tasks should be sequenced so that the branch is in a deployable or revertible state at each task boundary. Prefer many small tasks over few large ones.

## 8. Test Strategy

- **Unit test approach.** Coverage targets, mocking strategy, factories or fixtures.
- **Integration test approach.** What boundaries are exercised end-to-end.
- **Property-based or fuzz tests.** Where applicable.
- **Manual verification.** Steps a reviewer can run locally to validate the feature.
- **Test data.** How realistic data is generated or seeded.

## 9. Review Checkpoints

A short list of checkpoints, each tied to one or more task IDs, where a code review is required before proceeding. For each checkpoint:

- **Trigger.** Which tasks are complete.
- **Review focus.** What the reviewer should pay particular attention to (security, performance, contract stability, etc.).
- **Exit criteria.** What must be true to move past the checkpoint.
- **Reviewer floor.** When `MODEL_FLOOR_POLICY` is set: the minimum model tier for an agent performing this review (checkpoints guarding logic-heavy or hard-to-reverse work take the top tier).

## 10. Risks and Mitigations

A table of risks. For each: description, likelihood, impact, mitigation, owner. Include the risk of "we got this design wrong" with a mitigation of an early review checkpoint or spike.

## 11. Rollout and Rollback

- Feature flag strategy, if any.
- Migration steps and ordering.
- Rollback plan: what to do if the feature must be disabled in production.
- Monitoring during rollout: what signals indicate success or failure.

## 12. Out of Scope

A bulleted list of work that is intentionally not part of this feature. Include items that are likely to come up in review so that the reviewer knows they are deferred deliberately.

## 13. Open Questions

Anything still unresolved at spec time. Each must have an owner and a target resolution date or milestone.

## 14. References

Links to the patterns, RFCs, library docs, internal docs, and prior code that informed the design. Distinguish **Authoritative** references (binding — the spec's commitments must match these) from **Inspirational** references (prior art that informed the design but does not bind it). Use two sub-headings — `### Authoritative` and `### Inspirational` — when both classes are present.

# OUTPUT FORMAT

- Phase 1 and Phase 2 may be conversational.
- Phase 3 must be a single self-contained markdown document, suitable for committing as `specs/YYYYMMDD-<feature-name>/feature.md`. Create a `journal.md` in the same directory following the JOURNAL CREATION contract below. If a design spec exists upstream, it should already be in a sibling or parent spec directory referenced via `DESIGN_SPEC_PATH`.
- All file paths must be repo-relative.
- All code blocks must specify a language for syntax highlighting.
- If the spec will be committed to a different repo than the codebase it describes, note this in the spec's §3 Background section and include `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` values for downstream `spec-execute` sessions. This eliminates the need for the executor to rediscover the multi-repo layout each session.
- Do not include marketing language. Be precise.

# JOURNAL CREATION

Create `journal.md` in the same directory as the spec. The file **opens with two blocks** — a `## Current State` STATE block and a `## Grammar` bootstrap block — followed by the first append-only journal entry recording the authoring event. STATE is a fixed-position summary of "where are we now"; the `## Grammar` block is the one anchor a cold reader looks to first, so it must not itself be dialect-dependent.

**STATE block — emit verbatim at the journal head.** STATE is written here at journal creation and thereafter **overwritten in place at every closeout** (task, checkpoint, amendment) by the downstream skill doing the closeout; it is the *only* in-place-mutated part of the journal — entries below stay append-only. Fill the angle-bracket placeholders for the newly authored spec (Phase → the spec's authoring/first-checkpoint state; Last completed → "feature spec authored"; Next → the first task or checkpoint; Latest entry → the anchor of the authoring entry below):

```markdown
## Current State
- **Phase:** <e.g. P2 execution | CP-2 review | complete>
- **Last completed:** <task/checkpoint id> (<YYYY-MM-DD>)
- **Next:** <task id + one-line | "CP-N pending" | "spec complete">
- **Open holds:** <blockers / deferred items | none>
- **Pending checkpoint:** <CP-N + spec §ref | none>
- **Archive:** <relative path to journal-archive.md | none — all entries live>
- **Latest entry:** <anchor of the most recent full entry below>
```

**`## Grammar` bootstrap block — emit verbatim adjacent to STATE:**

```markdown
## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.
```

**Reference dialect — the native default you emit absent any override.** If the constitution declared a `## Grammar` block, you codified it forward into the spec during Discovery (see PHASE 1) and the spec's grammar is that declared dialect. Absent any such declaration, the native default anchor dialect you emit is:

| Element | Canonical anchor |
|---|---|
| Journal entry | `## <YYYY-MM-DD> — <event>` |
| Task closeout | `## <YYYY-MM-DD> — <T-ID>: <title>` |
| Review | `## <YYYY-MM-DD> — Review of <CP-ID>` |
| Amendment | `## <YYYY-MM-DD> — Amendment <id>` (single form) |
| Spec section | `## N. <title>` |
| Task block | `#{3,4} <T-ID> — <title>` (h3 or h4) **and** the §7 table row, where a table exists |

# WHAT NOT TO DO

- Do not skip Phase 1. If you do not have read access to the codebase, say so and ask for it; do not write a spec from imagination.
- Do not proceed past Phase 2 with unresolved blocker questions.
- Do not introduce a new framework, ORM, or major dependency without an explicit subsection in "Alternatives considered" justifying it.
- Do not write task descriptions in the form "Implement X." Write them in the form "Add `<file>` exposing `<function>` such that `<acceptance criteria>`."
- Do not produce tasks larger than what one engineer can complete and review in a working day. Split them.
- Do not invent acceptance criteria that cannot be objectively verified ("works well", "is performant"). Replace with measurable criteria.
- Do not silently redesign work that a `DESIGN_SPEC_PATH` already committed to. If the codebase contradicts the design spec, stop and propose an amendment via `spec-amend`; do not paper over the contradiction.
- Do not propose tasks outside the in-scope domain or tech-stack declared in `CONSTITUTION_PATHS`. If the feature genuinely requires scope expansion, surface it as a constitution amendment first.
