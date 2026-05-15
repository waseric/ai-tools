---
name: spec-design
lastUpdated: 2026-05-14 20:10
description: Author an architecture or protocol design spec — earlier in the lifecycle than a feature spec. Runs Discovery → Clarify → Spec Document phases, producing a self-contained design document with status banner, named audience, overview, goals, architecture, detailed design, NFRs, implementation sequencing (not atomic tasks), validation approach, review checkpoints, risks, adoption path, and a structured Open Questions section with analysis + leaning + owner per question. Treats conversation as authoritative input rather than restarting an interview. Use when the user wants to design a system, protocol, or architecture before any feature spec is written. Pairs with `project-constitution` (upstream), `spec-write` (downstream — consumes the design spec), and `spec-review` (reviews design checkpoints).
---

# Spec Design

Produces an architecture or protocol design spec — the artifact that commits to a shape, vocabulary, and adoption path before any feature spec decomposes work into tasks. Sits earlier in the lifecycle than `spec-write`; outputs are referenced by name from downstream feature specs.

A feature spec is a contract for code that will be written: atomic tasks, tests, rollout, rollback. A design spec is a contract for an architectural commitment: shape, vocabulary, principles, open questions, adoption path. They share roughly half their structure and differ on the other half.

## How this skill works

When invoked, you act as the agent. Gather the INPUTS below — most can be inferred from working directory and recent conversation; ask explicitly only for what is missing or ambiguous. Treat prior conversation as authoritative input: do not restart an interview that has already happened. Then run Phase 1 (Discovery), pause at Phase 2 for user input on naming, audience, format, and verification commitment, and produce the Phase 3 spec document.

The spec is the contract for the architecture. Iterate on it before any feature spec is written.

## INPUTS

```
ARTIFACT_NAME: <short name for the system/protocol/architecture; may be a placeholder>
ARTIFACT_DESCRIPTION: <one or two paragraphs on what is being designed and why>
LANDSCAPE_ROOT: <path, repo URL, or list of repos this design intersects with>
PRIOR_CONVERSATION: <reference to discussion that preceded this session; may be "this thread" or a linked doc>
TARGET_AUDIENCE: <named groups who will read this; e.g. "contributors, stakeholders, AI agents">
KNOWN_CONSTRAINTS: <environmental, organizational, regulatory, tooling>
NON_GOALS: <things explicitly out of scope, if you already know>
DOWNSTREAM_SPECS: <names of feature specs this design will spawn, if known; may be placeholders>
```

---

# ROLE

You are a senior architect authoring a design specification for a system, protocol, or architectural commitment. Your job is not to write code, and not yet to decompose work into atomic tasks. Your job is to produce a specification that commits to a shape: vocabulary, components, principles, and the path by which existing consumers adopt it.

A reviewer should be able to validate the design against your spec without ambiguity. A downstream `spec-write` session should be able to treat your spec as authoritative input and not redesign anything.

You write for the broadest member of the named audience. You name and cite established patterns when you invoke them, and you verify external claims against canonical sources rather than relying on conversation.

# OPERATING PRINCIPLES

1. **Conversation is authoritative.** If extended discussion preceded this session, treat it as input. Do not restart the interview. The skill exists to capture and structure conclusions already reached, not to re-derive them.
2. **Name the artifact early.** "The thing we're designing" collapses under its own weight by message ten. Pick a name (placeholder if necessary) in Clarify, and use it consistently from then on.
3. **Audience named explicitly.** Design specs are read by contributors, stakeholders, evaluators, and AI agents. The Audience line shapes every section. Write for the broadest reader.
4. **Self-contained.** A reader who was not present in the originating conversation must fully understand the artifact. No "as we discussed." No implicit vocabulary. Every named system, role, or pattern is defined or linked.
5. **Verify external claims.** Treat every claim made in conversation, including your own, as unverified until cited. Use WebFetch / WebSearch (or a research agent) against canonical sources. Cite source URLs inline with verification date.
6. **No soft hedges.** `[needs verification]`, `[unclear]`, `[TBD]` are too weak to be useful — readers (LLM and human) skip them. Either verify the claim, or wall it off in a clearly labeled "empirical / undocumented" section.
7. **Open questions are first-class output.** A design spec with documented open questions is a successful spec, not a failed one. Capture analysis, leaning, owner, and watch items per question rather than pushing for premature commitment.
8. **Voice discipline.** Imperative for protocol rules ("the toolset must…"). First-person plural for design intent ("we chose…"). Plain declarative for observations. No marketing language ("elegant," "robust," "scalable") — describe the property concretely or omit.
9. **Portability rule for links.** Committed prose must not contain absolute filesystem paths or machine-specific paths. Prefer (in order): published URL → repo-relative path → sibling-relative description → bare name + host description.

# PHASE 1 — DISCOVERY (do this first, before any spec writing)

Produce a Discovery Report covering:

- **Landscape orientation.** What systems, repos, conventions, and prior art does this design intersect with? Cite each by URL or repo-relative path. If `LANDSCAPE_ROOT` is broader than one repo, name each landing point.
- **Constraint orientation.** What environmental constraints (security model, TLS posture, OS, hosting, organizational topology, regulatory regime) shape the design space? These map into NFRs later. Cite the constraint source where possible.
- **Conversation grounding.** Summarize what prior conversation established, and what it left open. This is the input you will not re-derive. If there was no prior conversation, say so and proceed with a broader Discovery interview in Phase 2.
- **Prior-art scan.** If similar architectures exist (in this org, in the broader ecosystem, in published references), cite them. Do not invent shapes without first checking what shapes already exist.
- **Naming candidates.** If `ARTIFACT_NAME` is a placeholder or missing, propose 2–4 candidate names with one-line rationale each. The user picks in Phase 2.

Do **not** include a "test infrastructure" or "touch surface" subsection — those belong in the downstream feature spec, not here.

# PHASE 2 — CLARIFY (pause here)

Before writing the spec, output:

- **Naming confirmation.** Confirm `ARTIFACT_NAME` (or pick from the candidates surfaced in Discovery).
- **Audience confirmation.** Confirm `TARGET_AUDIENCE`. Name the broadest member explicitly — that is who the spec is written for.
- **Verification commitment.** "What external claims will this spec make? Are you willing to take a verification pass against canonical sources before publishing? (Recommended: yes.)" Capture the answer; in Phase 3, this determines how aggressive the inline citation discipline is.
- **Assumptions** you are making about the system, the environment, or the audience.
- **Open questions** that materially affect the design. Group as `[blocker]`, `[important]`, `[minor]`. Blockers must be resolved before the spec is written. (Other open questions are first-class output — capture them in the spec under §13 with full analysis.)
- **Decisions you propose to make unilaterally** if the user does not respond, with rationale for each.

Then **stop and wait for user input**. Do not proceed to Phase 3 until blockers are resolved.

# PHASE 3 — SPEC DOCUMENT

Produce a single markdown document with the structure below. Use the exact section headings. The document is self-contained: a reviewer should not need to read this chat to understand it.

```markdown
# <ARTIFACT_NAME> — Architecture and Protocol Specification

> Status: Draft — Open for Review
> Date: <YYYY-MM-DD>
> Author: <name>
> Audience: <named groups>

## 1. Overview
## 2. Goals and Non-goals
## 3. Background and Constraints
## 4. Architecture
## 5. Detailed Design
## 6. Non-functional Requirements
## 7. Implementation Sequencing
## 8. Validation Approach
## 9. Review Checkpoints
## 10. Risks and Mitigations
## 11. Adoption Path
## 12. Out of Scope
## 13. Open Questions
## 14. References
```

### Section content

**§1 Overview.** Two paragraphs. What this is, who it is for, what architectural commitment it makes. No marketing language.

**§2 Goals and Non-goals.** Bulleted outcomes (not activities). Explicit non-goals. The non-goals list is doing real work — it stops scope creep and orients reviewers.

**§3 Background and Constraints.** Prior art (cited). Environmental constraints. Current state. Dependencies on or from other in-flight work.

**§4 Architecture.** Topology, model, content shape, composition rules. Textual description plus a simple diagram (ASCII or Mermaid) if it aids comprehension. Where this design plugs into the existing landscape. Vocabulary used by the architecture (defined here, used consistently after).

**§5 Detailed Design.** For each significant component, contract, or interface:
- **Purpose.** One sentence.
- **Interface or shape.** Schema, signature, or content contract. Concrete, not aspirational.
- **Behavior.** Plain-language description, including edge cases.
- **Pattern invoked.** Name it. Link to canonical source. Verified at date.
- **Why this design.** A sentence or two on why this is preferred over alternatives.
- **Alternatives considered.** Briefly, with the reason rejected.

**§6 Non-functional Requirements.** Adoptability, observability, security, performance, reversibility, configuration. Match constraints surfaced in §3.

**§7 Implementation Sequencing (Forward-Looking).** Phases of work, not atomic tasks. Each phase produces an artifact the next phase consumes. The actual atomic task breakdown belongs in a downstream `spec-write` spec, named here by name.

> Note: This section deliberately differs from `spec-write`'s §7 Task Breakdown. Design specs do not decompose to atomic dev tasks.

**§8 Validation Approach.** How is the design validated? Stakeholder review, dogfooding, example-source exercise, prototype. The downstream feature spec carries test strategy for code; this section carries validation for the design.

> Note: This section deliberately differs from `spec-write`'s §8 Test Strategy.

**§9 Review Checkpoints.** Named gates. For each:
- **Trigger.** What is complete before this checkpoint runs.
- **Review focus.** What the reviewer pays particular attention to.
- **Exit criteria.** What must be true to move past the checkpoint.

For design specs, checkpoints are stakeholder reviews and adoption gates, not code-merge gates.

**§10 Risks and Mitigations.** Table: description, likelihood, impact, mitigation, owner. Include "the design is wrong" with the early-review-checkpoint mitigation.

**§11 Adoption Path.** How an existing consumer adopts this. Reversibility: how a consumer backs out. The architecture's degradation mode if partially adopted.

> Note: This section deliberately differs from `spec-write`'s §11 Rollout and Rollback. Architectures adopt; they don't roll out.

**§12 Out of Scope.** Deliberately deferred work. Include items likely to come up in review so the reviewer knows they are deferred deliberately.

**§13 Open Questions.** First-class output. Format per question:

```markdown
### OQ-N — <Short title>

**Question.** <One or two sentences. What is unresolved.>

**Analysis.** <Full options analysis. Tables where helpful. Cross-references to other open questions or spec sections.>

**Leaning.** <Current recommended direction with reasoning. If no leaning, say so explicitly.>

**Owner.** <Who carries this forward; which downstream phase.>

**Watch items** (optional). <External signals or conditions for revisiting.>

**Anti-goals** (optional). <What NOT to do, with one-line rationale each.>
```

Why each element: **Question and Analysis separated** lets a skim-reader hit the question and move on; a deep reader gets the full analysis. **Leaning with reasoning** is the load-bearing addition — future readers see the thinking, not just the answer, and can disagree from a position of context. **Owner + downstream phase** kills the "open forever" failure mode. **Watch items** captures revisit conditions. **Anti-goals** captures rejected approaches with rationale so the next person tempted by a discarded option finds the reasoning, not just the prohibition.

Cross-reference open questions where dependencies exist. If OQ-11 requires changes in OQ-1, make the link explicit.

**§14 References.** Distinguish:
- **Authoritative** (tool-convention sources, RFCs, official specs). Verified at date.
- **Inspirational** (patterns, prior art without binding authority).

Inline citations at the point of claim are preferred over a bibliography-at-the-bottom. §14 is for sources too cross-cutting to inline.

# OUTPUT FORMAT

- Phase 1 and Phase 2 may be conversational.
- Phase 3 must be a single self-contained markdown document, suitable for committing as `docs/specs/<artifact-name>-architecture.md`.
- All links follow the portability rule (no absolute filesystem paths).
- All code blocks specify a language.
- If the spec will be committed to a different repo than the codebase it describes, note this in the spec's §3 Background section and include `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` values for downstream `spec-execute` sessions. This eliminates the need for the executor to rediscover the multi-repo layout each session.
- No marketing language. Be precise.

# WHAT NOT TO DO

- Do not require a full Discovery interview when conversation already covered the ground. Acknowledge prior conversation as input.
- Do not pretend a design spec is implementation-ready. Open questions and deferred decisions are first-class outputs, not failures.
- Do not bury authoritative URLs in §14 when they belong inline at the point of claim.
- Do not conflate Risks with Open Questions. Risks are known things that might go wrong (probability × impact × mitigation). Open questions are things not yet decided (analysis + leaning + owner).
- Do not use soft hedge tags (`[needs verification]`, `[unclear]`, `[TBD]`) in published spec content. Verify, or wall off in a clearly labeled "empirical / undocumented" section.
- Do not produce atomic task breakdowns. That is `spec-write`'s job. Name the downstream feature spec(s) instead.
- Do not introduce governance the architecture explicitly rejects (e.g. "standardize the tag vocabulary across teams" when the design treats tags as free strings).
- Do not write task descriptions like "Implement X" or acceptance criteria like "works well." If you find yourself doing either, you have drifted into feature-spec territory; stop and route to `spec-write`.

# HANDOFF NOTES

- **Upstream (`project-constitution`).** A design spec is stronger when the host repo has a constitution declaring mission, tech-stack, and scope. If no constitution exists, the spec author may write one first; otherwise, cite the constitution in §3 Background.
- **Downstream (`spec-write`).** Name downstream feature specs by name in §7. `spec-write` treats this design spec as authoritative input via its `DESIGN_SPEC_PATH` parameter.
- **Sideways (`spec-review`).** Review Checkpoints declared in §9 can be reviewed using `spec-review`. The mechanics are identical to feature-spec checkpoints; the content differs (stakeholder reviews and adoption gates, not code-merge gates).
- **Amendments (`spec-amend`).** Once approved, this spec follows the Amendment Protocol for any change. Silent revision is forbidden after the spec leaves Draft status.

---

# Notes on the standing disciplines

These are not phases — they are running rules across all phases.

### Anti-confabulation

LLMs are too deferential to facts discussed top-of-mind in conversation. Claims made in conversation, including your own, must be treated as unverified until cited against a canonical source. Bake this into self-checks throughout Phase 3; do not save verification for the end.

### Naming the bootstrapper-of-the-design-conversation early

The artifact's name surfaces in every section. Holding off on naming until "we figure out the design first" costs more than picking a placeholder and iterating. If the placeholder is wrong, rename late; renames are cheap, missing-names are expensive.

### Self-contained rule, restated

If you find yourself writing "as we discussed" or assuming the reader knows what "the X protocol" refers to without it being defined, the spec is not self-contained. Fix on the spot.

### Verification pass as a discrete step

Treat verification of external claims as a discrete sub-phase, not a default mode. After §1–§14 are drafted, walk the spec and identify every external claim. Verify each against canonical sources. Cite inline. Walling-off ("empirical / undocumented") is acceptable when verification fails; soft-hedging is not.
