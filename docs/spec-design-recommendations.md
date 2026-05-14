# Recommendations for `spec-design`

Observations and recommendations from a session in which `spec-write` was adapted to produce a system-architecture / protocol-design document rather than a feature-implementation spec. The session designed an AI agent content distribution protocol (`ai-frontmatter-distributor`) and produced [`ai-frontmatter-distributor-architecture.md`]((internal URL removed) as the deliverable. These notes name what worked, what required reshape, and what a dedicated `spec-design` skill should formalize.

These are observations, not specifications. Take what fits the direction you're heading.

---

## 1. The core distinction the skill should embody

A **feature spec** is a contract for code that will be written: tasks, tests, rollout, rollback. A **design spec** is a contract for an architectural commitment: shape, vocabulary, principles, open questions, adoption path. Both are valuable; they share roughly half their structure and differ on the other half.

`spec-write`'s template is correctly shaped for the first. When pushed to do the second, three sections need substantive reshaping:

| `spec-write` section | What a design spec wants instead | Why |
|---|---|---|
| §7 Task Breakdown | **Implementation Sequencing (Forward-Looking)** | Design specs don't decompose into atomic dev tasks. They describe phases of work, where each phase produces an artifact the next phase consumes. The actual atomic task breakdown belongs in a downstream implementation spec named explicitly. |
| §8 Test Strategy | **Validation Approach** | Design specs are validated by stakeholder review, dogfooding, and example-source exercise — not by automated test coverage. The implementation spec carries the test strategy for code. |
| §11 Rollout and Rollback | **Adoption Path** | Architectures adopt; they don't roll out. Reversibility is a property of the design (sources can be removed, the protocol degrades gracefully) rather than a deployment plan. |

In our session this was the `B1` blocker question that surfaced in Phase 2: the skill's template assumed an implementation spec, and the artifact wanted a different shape. A dedicated `spec-design` skill removes that friction.

## 2. Phases the skill should run

The three-phase structure (Discovery → Clarify → Spec Document) translates well, with adjustments:

### Phase 1 — Discovery

For a design spec, "Discovery" is broader than "read the codebase":

- **Landscape orientation** instead of (or in addition to) codebase orientation. What systems, repos, conventions, and prior art does this design intersect with? Cite each.
- **Constraint orientation.** What environmental constraints (TLS, OS, security model, organizational topology) shape the design space? These map into NFRs later.
- **Conversation grounding.** Most architecture work emerges from extended discussion before the spec is opened. The skill should explicitly *welcome* conversation as authoritative input, and lean on it rather than restarting the interview. In our session, my discovery was minimal because we had hours of prior conversation; the skill should treat that as a feature, not a gap.
- **No "test infrastructure" subsection** in Discovery. Design specs don't have code-coverage discovery.

### Phase 2 — Clarify (this phase is more important for design specs than for feature specs)

The Clarify phase carried a lot of weight in our session because the artifact's shape itself was negotiable. Recommendations:

- Keep the `[blocker]` / `[important]` / `[minor]` triage. It worked.
- **Add a "format question" prompt.** The very first question a design-spec skill should ask is: "Does the standard template fit the artifact you want to produce, or should sections be reshaped?" Surfacing this explicitly avoids the friction we hit.
- **Confirm naming early.** When designing a system or protocol, the central artifact's name matters and surfaces in every section. The skill should prompt for it explicitly in Clarify, with the option "I haven't named it yet — pick one for me / propose a few."
- **Confirm the spec's audience.** Design specs are read by more people than feature specs (contributors, stakeholders, evaluators, AI agents). The skill should require the audience to be named — and write for the broadest one.

### Phase 3 — Spec Document

Generate the spec. See §4 below for the recommended section template.

## 3. The open-question pattern that worked

Our session evolved a shape for open questions that proved load-bearing. Recommend canonicalizing it in `spec-design`'s template:

```markdown
### OQ-N — <Short title>

**Question.** <One or two sentences. What is unresolved.>

**Analysis.** <Full options analysis. Tables where helpful. Cite cross-references to existing parts of the spec or to other open questions.>

**Leaning.** <The current recommended direction with reasoning. If no leaning, say so explicitly.>

**Owner.** <Who carries this forward; which downstream phase.>

**Watch items** (optional). <External signals, dependencies, things to revisit when conditions change.>

**Anti-goals** (optional). <What NOT to do, with one-line rationale each.>
```

Why each element matters:

- **Question and Analysis separated** lets a skim-reader hit the question and move on; a deep reader gets the full analysis. Don't conflate.
- **Leaning** (with reasoning, not just a verdict) is the load-bearing addition over typical "open questions" sections. Future readers see the thinking, not just the answer. They can disagree from a position of context.
- **Owner + downstream phase** kills the "open forever" failure mode. Every open question has a place where it gets resolved.
- **Watch items** captures conditions for revisiting. Especially valuable when the resolution depends on external developments (a tool's roadmap, a spec evolving elsewhere).
- **Anti-goals** captures rejected approaches with rationale. The next person tempted by a discarded option finds the reasoning, not just the prohibition.

This pattern also produces good cross-references: open questions can name other open questions in their analysis or watch items, making dependencies explicit. In our session this caught a real issue — OQ-11 (prioritization) required additions to OQ-1 (manifest schema), and the cross-link made the dependency visible to the Phase 1 implementer.

## 4. Proposed section template for `spec-design` output

The structure that emerged from our session, with rationale:

```markdown
# <Artifact name> — Architecture and Protocol Specification

> Status: <Draft — Open for Review | Approved | Superseded>
> Date: <YYYY-MM-DD>
> Author: <name>
> Audience: <named groups>

## 1. Overview                          # what this is, two paragraphs
## 2. Goals and Non-goals                # outcomes (not activities); explicit non-goals
## 3. Background and Constraints         # prior art, environmental constraints, current state
## 4. Architecture                       # topology, model, content shape, composition rules
## 5. Detailed Design                    # per-component: purpose / interface / behavior / pattern / why
## 6. Non-functional Requirements        # adoptability, observability, security, etc.
## 7. Implementation Sequencing          # phases, not tasks; points to downstream specs
## 8. Validation Approach                # stakeholder review, dogfooding, example exercise
## 9. Review Checkpoints                 # named gates with focus + exit criteria
## 10. Risks and Mitigations             # likelihood / impact / mitigation / owner
## 11. Adoption Path                     # how an existing consumer adopts; reversibility
## 12. Out of Scope                      # deliberately deferred
## 13. Open Questions                    # OQ pattern, full analysis + leaning
## 14. References                        # repos, external standards, pattern sources
```

Notes:

- **§1–6 are mostly identical to feature-spec sections.** The reshape concentrates in §7/§8/§11.
- **§13 is heavier in a design spec than in a feature spec.** Plan for it.
- **§14 References should distinguish authoritative from inspirational.** In our session we ended up with separate subsections for "Tool-convention sources" (authoritative URLs verified at a date) and "Pattern references (non-tool-specific)" (sources of inspiration like overlay composition patterns). Useful split.

## 5. Verification discipline (specific to design specs)

This came up sharply in our session and deserves to be a first-class discipline in `spec-design`:

**Soft hedges like `[needs verification]` are too weak a signal.** LLM readers skip them. Future humans treat them as noise. Don't use them.

The pattern that worked:

1. Identify every external claim the spec makes (tool conventions, file paths, behavior assertions, version dependencies).
2. Verify each at its canonical source. WebFetch / WebSearch the docs. Don't trust in-conversation claims, including your own. The skill should explicitly state: "Treat all claims you've made in conversation as unverified."
3. For each verified claim, cite the source URL inline and note the verification date.
4. For each claim you cannot verify, **omit it or wall it off in a clearly distinct section** (e.g., "Empirical patterns without authoritative source"). Do not soft-hedge.

In our session this produced corrections to two claims I'd made confidently:

- `.github/skills/SKILL.md` is officially documented by GitHub Copilot (I had been skeptical).
- `AGENTS.md` has an authoritative spec stewarded by the Linux Foundation (I had called it "de facto, no single source").

Both were corrected because we verified instead of soft-hedging. A skill that bakes in this discipline will produce better artifacts.

**Recommended Clarify-phase prompt:** "What external claims will this spec make? Are you willing to take a verification pass against canonical sources before publishing? (Recommended: yes.)"

## 6. Cross-cutting disciplines worth baking in

These came up enough times in our session that they should probably be standing requirements in `spec-design`'s template:

### Status + Date + Author at the top

Mandatory. Without a date, design specs become timeless and undated — dangerous for fast-evolving conventions. Status lifecycle: `Draft — Open for Review` → `Approved` → `Superseded`.

### Audience named explicitly

The Audience line at the top (in our session: "Future contributors; specialization authors; stakeholders evaluating adoption; AI agents") shapes everything else. The skill should prompt for it and write the spec for the broadest member.

### "Self-contained" rule

Write so a reader who was not present in the originating conversation can fully understand the artifact. This is the difference between a spec and a chat-log transcript. Concrete tests:

- No "as we discussed."
- No implicit shared vocabulary.
- Every named system, role, or pattern is defined or linked to its definition.

### Linking conventions (portability rule)

Committed prose must not contain absolute filesystem paths or machine-specific paths. The preference order, derived from our session:

1. Published URL (with deep-link form for files inside a repo when supported).
2. Repo-relative path for references within the document's own repo.
3. Sibling-relative description (named, without an encoded `../` path) for local-only repos.
4. Bare name + host description for well-known artifacts without paths.

If none fits, the artifact probably needs a stable address before it's cited.

### Voice

- Imperative for protocol rules ("the toolset must…").
- First-person plural for design intent ("we chose…").
- Plain declarative for observations.
- No marketing language ("elegant," "robust," "scalable" — describe the property concretely or omit).

### Anti-confabulation

The skill should explicitly remind itself (and the operator) that "LLMs are too deferential to facts discussed top-of-mind in conversation." Claims made in conversation should be treated as unverified until cited. This deserves to be a standing principle in the skill, not a per-session reminder.

## 7. How `spec-design` relates to the other skills

The four-skill suite makes sense as a pipeline, but the handoffs need to be named:

```
project-constitution → spec-design → spec-write → spec-execute → spec-review
   (proposed)                                                    (existing trilogy)
```

| Skill | Produces | Reads |
|---|---|---|
| `project-constitution` (proposed) | Repo mission, scope, tech-stack, conventions | New / empty repo |
| `spec-design` (proposed) | Architecture / design spec | Conversation + landscape; usually after constitution exists |
| `spec-write` (existing) | Feature implementation spec | Design spec (if present) + codebase |
| `spec-execute` (existing) | Implementation work, one task at a time | Feature spec + journal |
| `spec-review` (existing) | Verdict against a review checkpoint | Implementation work + spec |

Notes on the handoffs:

- **`spec-design`'s output references downstream feature specs by name.** Our `ai-frontmatter-distributor-architecture.md` named `ai-frontmatter-distributor-mvp` as the implementation spec that hasn't been written yet. `spec-write` should treat a design spec, when present, as authoritative input.
- **`spec-execute` doesn't run against design specs directly.** Design specs are reviewed and adopted; they don't produce code. `spec-execute` runs against the implementation specs downstream.
- **`spec-review` operates the same way against design specs as against feature specs**, but the "Review Checkpoints" in a design spec are stakeholder reviews and adoption gates, not code-merge gates.

## 8. Companion skills worth building

In addition to `spec-design`, two companion needs surfaced:

### `project-constitution`

Bootstraps a new repo with the three-part shape that's appeared in every repo we touched (mission, tech-stack, roadmap or mission, tech-stack, validation). Each repo (`legacy-agents-repo`, `standards-repo`, `reference-repo`) hand-rolled this and they all converged on similar structure. A skill that does it consistently would shorten every new repo's setup and would make `spec-design` more grounded (architecture specs are stronger when the host repo has a constitution).

Likely structure:
- Prompt for: repo purpose, audience, what's in scope / out of scope.
- Scan for: `package.json`, any existing READMEs, framework markers, CI config.
- Produce: `mission.md` (the why), `tech-stack.md` (the how), and optionally `roadmap.md` (planned phases) or `validation.md` (done criteria), depending on which lifecycle the repo is in.
- The old `init-project-specs.md` and `new-feature-spec.md` skills in `legacy-agents-repo` are an earlier iteration of this idea and could inform the new shape.

### `spec-amend` (lower priority)

A small skill that supports the Amendment Protocol the existing `spec-execute` references. When implementation reveals that the design spec needs revision, there should be a clear path for proposing and recording amendments rather than silently deviating. Currently this is folded into `spec-execute`; pulling it out makes amendments more visible as a first-class action.

## 9. Things to NOT do (informed by session experience)

These are anti-patterns to bake into the skill's anti-goals:

- **Don't require a full Discovery interview when conversation already covered it.** Force the skill to acknowledge prior conversation as input rather than restarting.
- **Don't pretend a design spec is implementation-ready.** Open questions and decisions deferred are first-class outputs, not failures. The skill should explicitly accept and document them rather than pushing for premature commitment.
- **Don't bury authoritative URLs in §14 References when they belong inline at the point of claim.** Verified-at-date tables (like the "Path conventions" subsection in our spec) work better than a single bibliography at the bottom.
- **Don't conflate Risks with Open Questions.** Risks are known things that might go wrong (probability × impact × mitigation). Open questions are things not yet decided (analysis + leaning + owner). Different shapes, different sections.
- **Don't allow soft hedge tags (`[needs verification]`, `[unclear]`, `[TBD]`) in published spec content.** Either verify, or wall off in a labeled "empirical / undocumented" section. Hedges get skipped by readers — LLM and human alike.
- **Don't try to standardize the architecture's tag vocabulary across teams via the skill.** Spec authors will reach for "standardize the tags I use" as a recommendation; resist. Tags are free strings; collision is the architecture's signal of competition. (This may be specific to the `ai-frontmatter-distributor` design but the meta-pattern — don't add governance the architecture explicitly rejects — generalizes.)
- **Don't assume the skill's section order is canonical.** Let the spec author reshape if the artifact demands it; require the reshape to be called out in a "Format note" at the top of the spec.

## 10. Small things that mattered more than they sounded like they would

A few process notes from the session that aren't quite recommendations but are worth recording:

- **Naming the bootstrapper-of-the-design-conversation early was load-bearing.** "The thing we're designing" worked for ten messages, then collapsed under its own weight. `ai-frontmatter-distributor` only emerged after several iterations of options. Future-me with a skill: "name the thing in Clarify, even with a placeholder."
- **The "format note at top of spec" pattern is valuable.** When a section deviates from the template, naming the deviation up front is honest and helps reviewers understand the artifact's shape.
- **Verifying with a separate research agent worked well.** Three parallel research agents (one per tool ecosystem) caught two real errors in claims I had made confidently. The pattern: brief the agent to be skeptical, list the specific claims to verify, demand canonical sources, accept "not documented" as an answer. The skill might want a built-in "verification pass" sub-routine.
- **The session's main artifact got better through small focused edits.** "TLS interception" wording, the path-conventions section, OQ-11 — each edit was a small turn that improved one thing without churning the document. The pattern of small focused edits late in the process kept the artifact from getting stale during review.
- **A short Status: Draft banner at the top of a spec is doing real work** — readers calibrate their expectations and feedback accordingly. Don't omit it.

---

## Appendix: The ai-frontmatter-distributor spec as a worked example

[`ai-frontmatter-distributor-architecture.md`]((internal URL removed) was produced by running `spec-write` against an extended design conversation and adapting the output. It demonstrates:

- The reshape of §7/§8/§11 for a design spec
- The Open Question pattern with full analysis + leaning + watch items + anti-goals
- The "verified at date" pattern for external authoritative sources
- Cross-references between open questions (OQ-1 ↔ OQ-11)
- The "Format note" at the top declaring the reshape
- Sibling-relative references for repos without remotes; URL-based for those with remotes
- The Topic Index (a managed-region composition the toolset emits) as a way to surface design-time signals to read-time agents

The spec is a draft and almost certainly contains things that won't survive review. Take it as a worked example of the shape, not a model of the content.
