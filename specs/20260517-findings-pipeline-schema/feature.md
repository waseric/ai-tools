# Findings Pipeline Schema — Feature Specification

> Status: Complete
> Date: 2026-05-17
> Author: waseric + Claude
> Audience: Eric Wasgatt (executor); AI coding agents executing this spec; reviewers at RC-2
> Upstream design spec: [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)

## 1. Overview

This feature spec implements **Phase A** of the [Findings Pipeline design spec](../20260517-findings-pipeline/architecture.md): commit the finding artifact template, journal template, status state machine, and persona-frame taxonomy as concrete reference files under `specs/findings/`. The deliverable is a small set of canonical markdown artifacts that downstream skills (`finding-intake`, `finding-triage`) will read and consumers (humans + agents) can reference directly.

Phase D (investigation protocol) is **bundled** into this spec per the design spec's allowance: the investigation section is part of the finding template, not a separate artifact. Additionally, this spec absorbs the nine advisory findings from the RC-1 self-review of the design spec via a single batched amendment.

## 2. Goals and Non-goals

**Goals:**

- Create `specs/findings/README.md` documenting the schema: state machine, status semantics, persona-frame taxonomy, and "how to use this directory."
- Create `specs/findings/_template/finding.md` as the canonical finding artifact template.
- Create `specs/findings/_template/journal.md` as the canonical finding-journal template.
- Apply a single batched amendment to the design spec absorbing all nine RC-1 advisory findings.
- Validate the schema via the example-source exercise: retroactively shape a real journal note as a finding under the template.

**Non-goals:**

- Authoring the `finding-intake` skill (Phase B — separate feature spec).
- Authoring the `finding-triage` skill (Phase C — separate feature spec).
- Amending `spec-amend` or `spec-write` to accept `FINDING_PATH` (Phase E — separate feature spec(s)).
- Routing any real findings through the pipeline (Phase F — adoption review).
- Promoting investigation from a protocol to a separate skill (deferred per OQ-2, decided at RC-5).
- Verifying ITIL/SDLC canonical citations against published sources — addressed lightly in the batched amendment by reframing §14 references as Inspirational without binding citations.

## 3. Background and Constraints

### Spec repo context

This feature spec lives in the same repository as the codebase it modifies — there is no codebase distinct from the methodology repo. `SPEC_REPO_ROOT` and `CODEBASE_ROOT` are the same: `/Users/eric/scm/github/waseric/ai-tools`.

- **SPEC_REPO_ROOT:** `/Users/eric/scm/github/waseric/ai-tools`
- **SPEC_TARGET_BRANCH:** `main`

### Upstream artifacts

- **Design spec:** [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md). All design commitments, vocabulary, and the open questions in §13 are authoritative input. This feature spec executes Phase A of §7 Implementation Sequencing.
- **Constitution:** [mission.md](../mission.md), [tech-stack.md](../tech-stack.md), [roadmap.md](../roadmap.md). Methodology is markdown-only; this spec produces markdown artifacts.
- **Spec-path convention:** [20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md). Establishes per-spec subdirectories with date-prefixed names, artifact-type filenames, and underscore-prefixed special directories (`_archive/` precedent; `_template/` follows the same pattern here).

### RC-1 inheritance

The design spec passed RC-1 on 2026-05-17 with verdict `pass with comments` (zero blockers, one important resolved via Amendment 2026-05-17-1, nine advisories deferred to this RC-2 schema pass). The nine advisories are addressed in T-04 below.

### Constraints

- Markdown-only deliverables. No executable runtime; validation is by inspection and example-source exercise.
- AI context-window economy: schema docs and templates target under ~200 lines each.
- The schema must be usable by a downstream skill (`finding-intake`) that has not yet been authored. Field semantics must be unambiguous on a single read.

## 4. Architecture

### Layout

```
specs/
  findings/
    README.md               ← schema description (this spec produces)
    _template/
      finding.md            ← canonical finding artifact template (this spec produces)
      journal.md            ← canonical finding-journal template (this spec produces)
    [future: YYYYMMDD-<name>/finding.md + journal.md per real finding]
```

`_template/` follows the `_archive/` precedent from the spec-path convention: an underscore-prefixed directory inside `specs/findings/` signals "not a real finding, structural support."

### Schema description shape

`specs/findings/README.md` carries three sections:

1. **What this directory holds.** One paragraph linking to the upstream design spec; defines the boundary between this directory's content and the design spec's content.
2. **State machine.** Text + ASCII diagram of `intake → triaged → under-investigation → routed | closed` with `reopened` back-transition explicit. Per-status semantics.
3. **Persona-frame taxonomy.** The three personas (intake/triage/investigation), their access requirements, and the solo-operator collapse principle. Lifted directly from design spec §5.6.
4. **Field reference table.** Each field in the finding template, with type, required-by-phase, and one-line semantics. Consumed by AI agents.

No new vocabulary is invented here. All terms come from the design spec; this artifact codifies them in a single place.

### Template shape

`specs/findings/_template/finding.md` mirrors design spec §5.1 with placeholder values, plus a top-of-file comment block stating "this is a template — copy to `specs/findings/YYYYMMDD-<short-name>/finding.md` to create a finding." `journal.md` follows the feature-spec journal convention (mirrors [20260515-spec-path-convention/journal.md](../20260515-spec-path-convention/journal.md) structurally).

### Design spec amendment surface (T-04)

A single batched amendment touches these sections of the design spec:

- §4 (topology diagram + monotonicity wording + route/status mapping)
- §5.1 (`Last transition` field documented)
- §5.6 (persona-frame wording consistency + intake-persona breadth)
- §13 (OQ-1 converted to recorded decision; OQ-5 narrowed)
- §14 (ITIL/SDLC references reframed to acknowledge Inspirational-without-binding-citation status)

Detailed before/after diffs are produced at execution time via `/spec-amend`.

## 5. Detailed Design

### 5.1 `specs/findings/README.md`

**Purpose.** Single discoverable entry point for any consumer (human or agent) opening `specs/findings/`. Describes the schema, the state machine, and how to use the directory.

**Structure.**

```markdown
# Findings — Schema and Usage

> Audience: methodology consumers (humans and AI agents) creating, triaging, investigating, or routing findings
> Upstream design spec: [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)

## What this directory holds

[One paragraph: each finding is a directory `YYYYMMDD-<short-name>/` containing `finding.md` and `journal.md`. The `_template/` directory holds the canonical templates. This README describes the schema; the design spec describes the why.]

## State machine

[Text description + ASCII diagram showing status transitions. Includes `reopened` back-transition. Explicitly maps the four route subtypes (amend/write/defer/close) to terminal status values (routed/closed).]

## Status semantics

[Per-status: what it means, who acts during this phase, what completes the transition out.]

## Persona-frame taxonomy

[Lifted from design spec §5.6. Three personas: intake (anyone), triage (business-analyst-frame), investigation (developer-frame). Solo-operator collapse principle. Note that intake intentionally generalizes beyond the three-role frame because the input source is unbounded.]

## Field reference

[Table: field name, type, required-by-phase, one-line semantics. Covers every field in the finding template.]

## Creating a new finding

[One short paragraph + example: copy `_template/finding.md` to `YYYYMMDD-<short-name>/finding.md`, fill the Intake section, journal the transition.]
```

**Pattern invoked.** README-as-front-door, common to convention-bearing directories.

### 5.2 `specs/findings/_template/finding.md`

**Purpose.** Canonical reference template that `finding-intake` (and the operator manually) copy when creating a new finding.

**Structure.** Mirrors design spec §5.1 verbatim, with placeholder values like `<short title>`, `<YYYY-MM-DD>`, `<persona-frame>`. Top-of-file comment block:

```markdown
<!--
This is a template. Copy this file to specs/findings/YYYYMMDD-<short-name>/finding.md
when creating a new finding. Fill the Intake section first; later phases append.

See specs/findings/README.md for the schema and state machine.
See specs/20260517-findings-pipeline/architecture.md for the design spec.
-->
```

The template includes the operational urgency field per OQ-1's leaning (which T-04 converts to a recorded decision).

### 5.3 `specs/findings/_template/journal.md`

**Purpose.** Canonical reference template for a per-finding journal, following the existing feature-spec journal pattern.

**Structure.**

```markdown
# <Short title> — Journal

## <YYYY-MM-DD> — Intake

**Captured by:** <name; persona-frame: intake>
**Signal source:** <text / URL / system pointer>
**Notes:** <anything not captured in finding.md>
```

Subsequent entries follow the same one-event-per-section pattern from existing journals.

### 5.4 Batched design-spec amendment (T-04)

Executed via `/spec-amend`. Single amendment ID `2026-05-17-2` (since `-1` was the §5.8 dangling-reference fix). Scope: nine RC-1 advisories, structured into the amendment's `Change` section as a sub-list per advisory.

**Pattern invoked.** Surgical batched amendment, per spec-amend's principle that "a coherent set of related sections" can be one amendment even when touching multiple sections.

**Why batched rather than nine separate amendments.** All nine are presentation/consistency refinements with no design substance changes; routing each as a separate amendment is ceremony without proportional benefit. The RC-1 verdict explicitly recommended batching.

## 6. Non-functional Requirements

| Property | Requirement |
|---|---|
| **Discoverability** | A consumer opening `specs/findings/` finds README.md as the first orientation point. |
| **Readability** | Each artifact (`README.md`, `finding.md`, `journal.md`) fits within ~200 lines. |
| **Self-contained** | Each artifact references the design spec by path but does not require it loaded into context to be understood. |
| **AI-agent consumable** | The field reference table in README.md uses a regular structure (column headers, one row per field) that an agent can parse without prose interpretation. |
| **Backward compatibility** | Adding `specs/findings/` does not affect any existing spec or skill. The `FINDING_PATH` parameter (Phase E) is additive and not introduced here. |
| **No new dependencies** | All deliverables are markdown. No new tooling, frameworks, or config introduced. |

## 7. Task Breakdown

### T-01 — Apply batched amendment to design spec for 9 RC-1 advisories

**Scope:**
- Run `/spec-amend` against [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md).
- Single amendment ID `2026-05-17-2`.
- Sections touched: §4 (topology, monotonicity, route/status mapping), §5.1 (Last-transition field intent note), §5.6 (persona-frame wording consistency + intake-persona breadth note), §13 (OQ-1 → recorded decision; OQ-5 → narrowed), §14 (Inspirational-without-binding reframing).

**Per-advisory change summary** (for the amendment's `Change` section):

1. §4 topology diagram — add `closed → reopened → triaged|under-investigation` back-transition arrow.
2. §4 composition rule — rewrite "`status` progresses monotonically…" to "`status` is append-only and forward-progressing under normal flow; reopening creates a new status entry that returns to an earlier phase, preserving prior history."
3. §4 (or §5.5) — add an explicit one-paragraph mapping of the four route subtypes (`amend`/`write`/`defer`/`close`) to terminal status values (`routed`/`closed`).
4. §5.6 and §6 — standardize on "orientation, not handoffs" wording; remove "orienting, not gating" variant from the §6 NFR row.
5. §5.6 — add note that intake intentionally generalizes beyond the three-role frame because the input source is unbounded.
6. §13 OQ-1 — convert to a recorded decision in §5.3 (severity discussion) or §6; remove OQ-1 from §13.
7. §13 OQ-5 — narrow framing to "triage-time revalidation policy for external pointers"; remove the parts already answered by §5.2.
8. §5.1 template — add a one-line note documenting `Last transition` field intent (scan-aid for skimming a finding without traversing the journal).
9. §14 — rewrite to acknowledge ITIL/SDLC references are Inspirational without canonical URL verification; note that a verification pass is deferred to external-adopter need.

**Acceptance criteria:**
- **Given** the design spec at `Draft — Open for Review`,
- **When** Amendment 2026-05-17-2 is applied via `/spec-amend`,
- **Then** all nine advisories are reflected in the spec, each before/after diff is recorded in the amendment, the journal entry references all nine, and the design spec remains at `Draft — Open for Review`.

**Tests required:** Inspection — re-read each advisory from the RC-1 journal entry and verify the corresponding change is present in the amended spec. Verify the journal entry contains the full amendment record.

**Definition of Done:** Amendment applied; journal entry written; commit landed with message `spec: amendment 2026-05-17-2 — RC-2 schema-pass advisory batch`; design spec status unchanged.

**Dependencies:** None (this task runs first).

**Estimated size:** M.

### T-02 — Create `specs/findings/README.md` (schema documentation)

**Scope:**
- New file: `specs/findings/README.md`.
- Sections: "What this directory holds", "State machine" (text + ASCII), "Status semantics", "Persona-frame taxonomy", "Field reference" (table), "Creating a new finding".
- All vocabulary lifted from the amended design spec (post-T-01).

**Acceptance criteria:**
- **Given** the amended design spec exists,
- **When** a consumer opens `specs/findings/README.md`,
- **Then** they find: a state-machine description matching the amended design spec, the persona-frame taxonomy matching §5.6 post-amendment, a field reference table covering every field in the finding template, and a "create a finding" how-to.
- **And** every term used is defined either in this README or in the design spec (no implicit vocabulary).

**Tests required:**
- Inspection: every field in the planned `_template/finding.md` (T-03) appears in the field reference table.
- Inspection: state-machine description matches design spec §4 post-amendment (including `reopened` back-transition).
- Inspection: persona-frame description matches design spec §5.6 post-amendment.

**Definition of Done:** File written; cross-references valid (clickable in markdown rendering); under ~200 lines; committed.

**Dependencies:** T-01.

**Estimated size:** M.

### T-03 — Create `specs/findings/_template/finding.md`

**Scope:**
- New file: `specs/findings/_template/finding.md`.
- Mirrors design spec §5.1 verbatim with placeholder values.
- Top-of-file HTML comment with usage instructions (copy-to-instructions + cross-references to README and design spec).
- Includes the operational urgency field per the T-01 conversion of OQ-1 to a recorded decision.

**Acceptance criteria:**
- **Given** the design spec §5.1 (post-T-01 amendment),
- **When** a downstream user copies this template,
- **Then** they receive a fully-shaped finding artifact with placeholders for every field documented in the README field reference table.
- **And** the top-of-file comment guides them to fill the Intake section first.

**Tests required:**
- Inspection: every field in README.md field reference table appears in this template.
- Inspection: section headings match design spec §5.1 verbatim.

**Definition of Done:** File written; section structure matches design spec §5.1; under ~200 lines; committed.

**Dependencies:** T-01, T-02 (must agree on field set).

**Estimated size:** S.

### T-04 — Create `specs/findings/_template/journal.md`

**Scope:**
- New file: `specs/findings/_template/journal.md`.
- Mirrors the feature-spec journal convention from existing journals (e.g., [20260515-spec-path-convention/journal.md](../20260515-spec-path-convention/journal.md)).
- Includes a starter "Intake" entry with placeholders; documents the one-entry-per-status-transition pattern.

**Acceptance criteria:**
- **Given** the finding template exists,
- **When** a downstream user copies this journal template,
- **Then** they have a one-entry-per-transition journal scaffold matching the feature-spec journal convention.

**Tests required:** Inspection — structure matches an existing feature-spec journal; placeholders are clearly marked.

**Definition of Done:** File written; under ~100 lines; committed.

**Dependencies:** T-02 (the README documents the journaling pattern).

**Estimated size:** S.

### T-05 — Example-source validation exercise

**Scope:**
- Operator-chosen at execution time: select a recent ad-hoc journal note from an existing feature-spec journal (e.g., a "Surprises and learnings" entry, or any retrospective observation) that resembles something that would have entered the pipeline as a finding.
- Retroactively shape it as a finding under the template: create `specs/findings/<chosen-date>-<short-name>/finding.md` and `journal.md`, filling out at minimum the Intake section, ideally Triage as well.
- Record the validation outcome in [specs/20260517-findings-pipeline-schema/journal.md](journal.md): did the template absorb the realistic content without distortion? What didn't fit?

**Acceptance criteria:**
- **Given** the schema artifacts from T-02 through T-04,
- **When** the operator selects a real journal note and shapes it as a finding,
- **Then** every section of the template can be populated from the source material (or the field's "unknown" handling is exercised), and any friction is recorded as a finding (meta!) or as a journal observation for RC-2.

**Tests required:**
- The exercise itself is the test. Success is: the template absorbed the content; the state machine accommodated the example's actual phase; no shape mismatch surfaced that would require a schema change.
- Failure is: the example revealed a schema gap. Failure routes back to T-01 amendment (additional design-spec change) or a follow-up RC-2 amendment.

**Definition of Done:** A real finding file exists in `specs/findings/`; the validation outcome is recorded in this feature spec's journal; any schema gaps are surfaced as findings or RC-2 amendments before checkpoint close.

**Dependencies:** T-02, T-03, T-04.

**Estimated size:** S.

## 8. Test Strategy

- **Inspection-based validation.** All deliverables are markdown; tests are inspection (cross-reference checks, vocabulary consistency, field completeness).
- **Example-source exercise (T-05).** The primary integration test — does the template absorb real content?
- **Cross-reference verification.** Every link between the schema artifacts and the design spec must resolve to existing sections.
- **No mocking, no fixtures, no test runner.** The methodology repo's discipline is prose review.

## 9. Review Checkpoints

### RC-2 — Schema Review (inherited from design spec §9 RC-2)

- **Trigger:** All five tasks (T-01 through T-05) complete; commits landed.
- **Review focus per design spec:** "Whether the artifact template is concrete and minimal; whether the state machine is unambiguous; whether the persona-frame fields carry their weight."
- **Additional focus added by this feature spec:** Whether the batched amendment (T-01) cleanly resolved all nine RC-1 advisories without introducing new inconsistencies; whether the example-source exercise (T-05) surfaced any schema gaps.
- **Exit criteria per design spec:** "Template usable for a real finding without further interpretation; state transitions all uniquely defined."
- **Additional exit criteria:** T-05's example-source finding successfully populates the template; the RC-1 advisory list is fully addressed; no new `[blocker]` findings.
- **Status:** pass with comments on 2026-05-17 by waseric (self-review). 0 blockers, 1 important (README ASCII state diagram depicts an unauthorized `routed → closed` transition — prose-form state machine is authoritative and unambiguous; diagram cleanup recommended before Phase B `finding-intake-skill` authoring), 7 advisory (field-name asymmetry README↔template; placeholder-vs-`unknown` convention; missing `triaged → routed` direct-skip arrow in diagram; terminal-state vs Decided-by persona-frame phrasing; multi-symptom bundling guidance; retroactive-intake date semantics; journal template section-header format vs starter entry). Full review record at [journal.md](journal.md#review-of-rc-2).

## 10. Risks and Mitigations

| Description | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Batched amendment in T-01 introduces a new inconsistency the original review missed | Medium | Medium | T-01 acceptance criteria require diff-by-diff inspection per advisory; RC-2 reviews the amendment as part of schema review | Executor; RC-2 reviewer |
| Example-source exercise (T-05) reveals a schema gap that requires re-amending the design spec | Medium | Medium | T-05 explicitly accepts this outcome as valid — a discovered gap routes through `/spec-amend` before RC-2 closes | Executor |
| Field reference table in README.md drifts out of sync with the finding template | Low | Low | T-02 and T-03 dependencies enforce table-template agreement at write time; RC-2 verifies | Executor |
| `_template/` naming convention conflicts with a future spec-path-convention change | Low | Low | Reuses the existing `_archive/` precedent; if the convention changes, all underscore-prefixed dirs move together | Spec-path-convention maintainer |
| Schema design is wrong | Low | High | T-05 is the early-detection mechanism; failure routes back to design-spec amendment before RC-2 close | Executor; RC-2 reviewer |

## 11. Rollout and Rollback

**Rollout:** No production rollout. The deliverables are markdown artifacts in a methodology repo. "Adoption" means downstream skills (Phase B–E) begin referencing the schema artifacts.

**Rollback:**
- Each task is one or more commits; rollback is `git revert` of the affected commits.
- Adding `specs/findings/` is non-breaking — no existing spec or skill references it yet, so removing it has no downstream effect.
- The T-01 amendment to the design spec is rollback-able via `git revert` of the amendment commit; the design spec returns to its pre-amendment state with RC-1 still closed.

**Monitoring during rollout:** N/A — no runtime.

## 12. Out of Scope

- **`finding-intake` skill** — Phase B; separate feature spec to be authored after RC-2.
- **`finding-triage` skill** — Phase C; separate feature spec.
- **`spec-amend` / `spec-write` accepting `FINDING_PATH`** — Phase E; separate feature spec(s).
- **Real findings routed end-to-end** — Phase F; adoption review at RC-5.
- **Automated template enforcement** — no tooling validates that a `finding.md` matches the template. Validation is by human or agent review.
- **`finding-investigate` skill (graduation from protocol)** — deferred per OQ-2, decided at RC-5.
- **Verification of ITIL/SDLC citations against canonical URLs** — T-01 reframes the references rather than verifying them. Verification is a separate exercise if external adopters need it.

## 13. Open Questions

### OQ-1 — Should `_template/` files be excluded from any future linting or schema-validation passes?

**Question.** If the methodology grows automated validation (e.g., a CI check that every `specs/findings/<dir>/finding.md` has all required fields), should `_template/` be excluded from that validation since it contains placeholder values?

**Leaning.** Yes — `_template/` is structural support, not a real finding. Underscore-prefix is the conventional exclusion signal.

**Owner.** Deferred until automated validation is proposed. Not blocking for Phase A.

### OQ-2 — Does T-05's example-source finding "count" toward Phase F's three-real-findings adoption gate?

**Question.** Phase F (per design spec §7) requires three real findings (operational/testing/security) routed end-to-end before adoption is declared. T-05 produces *one* finding retroactively shaped from a journal note. Does it count toward the three?

**Leaning.** No — T-05's finding is a *validation* exercise, not a routed real finding. The three Phase F findings should be genuinely new observations, not retrofits. T-05's finding may still be useful as the first real finding-in-existence, but it doesn't satisfy adoption-gate criteria.

**Owner.** RC-5 reviewer at adoption review.

## 14. References

- **Upstream design spec:** [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)
- **Constitution:** [specs/mission.md](../mission.md), [specs/tech-stack.md](../tech-stack.md), [specs/roadmap.md](../roadmap.md)
- **Spec-path convention:** [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) — establishes the directory and naming conventions this spec follows.
- **Spec-amend skill:** [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) — invoked during T-01 for the batched amendment.
- **Existing journal precedent:** [specs/20260515-spec-path-convention/journal.md](../20260515-spec-path-convention/journal.md) — structural pattern for `_template/journal.md`.
