# Findings Pipeline Schema — Journal

## 2026-05-17 — Feature Spec Authored

**Status:** draft — awaiting execution
**Artifact:** specs/20260517-findings-pipeline-schema/feature.md
**Upstream design spec:** specs/20260517-findings-pipeline/architecture.md (Phase A of §7 Implementation Sequencing)
**Origin:** Phase A execution following RC-1 design-freeze pass-with-comments verdict.

**Decisions made:**
- Phase D (investigation protocol) bundled into Phase A — the investigation section is a part of the finding template, not a separate artifact.
- Schema artifacts live at `specs/findings/` (consumer-facing location): `README.md`, `_template/finding.md`, `_template/journal.md`.
- `_template/` follows the `_archive/` underscore-prefix precedent from the spec-path convention.
- Single batched amendment (`2026-05-17-2`) for all 9 RC-1 advisories, per the RC-1 verdict's explicit recommendation. Not nine separate amendments.
- T-01 (the amendment) runs first — build the schema artifacts against the cleaned-up design spec.
- T-05 example-source exercise will use an operator-chosen recent journal note at execution time, not a pre-selected source.
- State-machine documentation: text + ASCII diagram (markdown-portable, no renderer required).
- Schema doc structure: prose narrative followed by field-reference table (readable + agent-consumable).

**Open questions surfaced and parked in §13:**
- OQ-1 (`_template/` exclusion from future validation): leaning yes. Deferred until automated validation is proposed.
- OQ-2 (does T-05 finding count toward Phase F adoption gate): leaning no — T-05 is validation, not a real routed finding. RC-5 reviewer to confirm.

**Tasks defined:** T-01 (amendment) → T-02 (README) → T-03 (finding template) → T-04 (journal template) → T-05 (example-source validation). Five tasks, all S or M, sequenced so each boundary is a safe stopping point.

**Conversation grounding:**
- Operator approved Phase 2 defaults: text + ASCII state machine, prose + field-table schema doc, single batched amendment, T-04 first.
- Discovery phase was light — design spec is comprehensive; constitution is small; conventions are well-established.

**Next task pointer:** Execute T-01 (`/spec-amend` for the batched RC-2 schema-pass amendment) via `/spec-execute`. On T-01 completion, proceed to T-02.

## 2026-05-17 — T-01: Apply batched amendment to design spec for 9 RC-1 advisories

**Status:** done
**Commits:** 8c146ce (amendment applied), d8c3455 (amendment journal entry in design spec)
**Files touched:**
- specs/20260517-findings-pipeline/architecture.md (28 insertions, 31 deletions across §4, §5.1, §5.3, §5.5, §5.6, §6, §13, §14)
- specs/20260517-findings-pipeline/journal.md (148 insertions — full amendment record appended)

**Tests added:** N/A — inspection-based per T-01's "Tests required" (re-read each advisory; verify corresponding change present in amended spec).

**DoD verification:**
- *Amendment applied:* Commit 8c146ce applies all ten edits (sub-changes A–J) covering all nine RC-1 advisories. Inspection confirms each advisory's corresponding section is amended.
- *Journal entry written:* Commit d8c3455 appends the full amendment record to the design spec's journal at specs/20260517-findings-pipeline/journal.md lines 101–247, preserving before/after diffs per advisory.
- *Commit message matches required form:* `spec: amendment 2026-05-17-2 — RC-2 schema-pass advisory batch` — verified on commit 8c146ce.
- *Design spec status unchanged:* `Draft — Open for Review` per amendment's "Status implication: kept" — verified at specs/20260517-findings-pipeline/architecture.md status banner.

**Decisions made:** None new during execution — all nine advisory resolutions were pre-decided at feature-spec authoring time (per the "Per-advisory change summary" in [feature.md §7 T-01](feature.md#L208)). One execution-time refinement: sub-change H absorbed a §6 NFR-table addition (severity-axis decoupling row) as the natural home for OQ-1's converted decision, rather than placing it in §5.3 — the two locations were both proposed at spec time; §6 was selected as more discoverable.

**Spec amendments:** Amendment 2026-05-17-2 — recorded in [specs/20260517-findings-pipeline/journal.md](../20260517-findings-pipeline/journal.md#L101) as a first-class event. Renumbered OQ-2..5 → OQ-1..4 in the design spec; this feature spec's references to "OQ-1" (operational urgency) and "OQ-5" (external-pointer durability) at feature.md lines 159, 215, 216 now point to renumbered design-spec questions but remain semantically correct in context (the feature spec speaks of them as RC-1-era advisories). No follow-up edit required.

**Surprises and learnings:**
- T-01 closeout in this feature spec's journal was missed at amendment time — the amendment was journaled in the *design spec's* journal but not in the *feature spec's* journal. Repaired this session as the Phase 2 pre-flight verify gate before T-02 begins. Lesson: when an execution task is itself an amendment, both journals need an entry — the amendment record lives in the amended spec's journal, but the executing feature spec also needs its task closeout.
- The amendment touched one more section than originally enumerated (§5.3 dangling `See OQ-1` reference — sub-change D) for a total of ten edits across the nine advisories, because advisory 6 split cleanly into two distinct fixes (remove dangling reference + add severity-axis decoupling row). Net design substance unchanged.

**Next task pointer:** T-02 — create `specs/findings/README.md` (schema documentation). Dependency T-01 satisfied; no `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-02: Create `specs/findings/README.md` (schema documentation)

**Status:** done
**Commits:** (this commit)
**Files touched:**
- specs/findings/README.md (new file, 142 lines)
- specs/findings/ (new directory)

**Tests added:** N/A — inspection-based per T-02's "Tests required".

**DoD verification:**
- *File written:* specs/findings/README.md created with all six required sections (What this directory holds, State machine, Status semantics, Persona-frame taxonomy, Field reference, Creating a new finding).
- *Under ~200 lines:* 142 lines (verified via `wc -l`); leaves headroom for any RC-2-driven additions without breaching the NFR.
- *State-machine description matches amended design spec §4:* ASCII diagram includes the `reopened` back-transition (sub-change A); composition rules cite "append-only and forward-progressing" wording (sub-change B); route subtype → terminal status mapping paragraph mirrors sub-change E.
- *Persona-frame taxonomy matches amended §5.6:* uses standardized "orientation, not handoffs" wording (sub-change F first bullet); includes the intake-breadth asymmetry note as its own paragraph (sub-change F second bullet).
- *Field reference covers every template field:* 29-row table covers all fields from design spec §5.1 (architecture.md lines 118–164) — header block (Status, Domain, Severity, Operational urgency, Date opened, Last transition), Intake section (Reported by, Reported via, Captured by, Summary, External references), Triage section (Triaged by, Triage date, Reproducibility, Repro steps, Scope, Domain confirmation, Severity confirmation, Triage notes), Investigation section (Investigated by, Investigation date, Probable cause, Code/configuration touchpoints, Alternative hypotheses considered, Proposed remedy), Route section (Route decision, Decided by, Route date, Target spec, Route rationale). Title added as a 29th row (the H1 of the artifact, schema-load-bearing even though §5.1's template renders it as an implicit `# <Short title>` heading).
- *Cross-references valid:* verified `../20260517-findings-pipeline/architecture.md` and `../20260515-spec-path-convention/architecture.md` exist; verified `#7-implementation-sequencing` anchor resolves to `## 7. Implementation Sequencing` at architecture.md:300.
- *"Create a finding" how-to present:* four-step procedure with literal `cp` shell example; forward-pointer to Phase B `finding-intake` skill noted.

**Decisions made:**
- Field reference rendered as a 4-column table (Field / Type / Required by phase / Semantics) rather than a definition list, to satisfy the §6 NFR "AI-agent consumable: the field reference table in README.md uses a regular structure (column headers, one row per field) that an agent can parse without prose interpretation."
- Status semantics rendered per-status with Meaning / Persona-frame / Exit condition triplets — more readable than a single table, and accommodates the "Persona-frame: N/A" terminal-state cases honestly.
- Title field included as an explicit 29th row in the field reference, even though design spec §5.1 renders the title as an implicit H1 (`# <Short title> — Finding`). The README is a schema document; the title is part of the schema regardless of how the template syntactically expresses it. The T-03 template will need to instantiate the H1 with the title placeholder.

**Spec amendments:** None. Spec was sufficient as written post-T-01.

**Surprises and learnings:**
- The design spec's §5.1 template uses inline placeholder syntax for fields (`<text>`) rather than enumerating them in a table. Translating those into a structured field-reference table required some interpretive judgment about field naming (e.g., the unlabeled "Date:" rows under Triage and Investigation are disambiguated here as "Triage date" and "Investigation date" so the field reference table has unique names per row).
- The state-machine ASCII diagram in the README is rendered differently than the topology diagram in design spec §4 — the design spec's diagram covers the full pipeline (signals in, routes out), while the README's focuses narrowly on status transitions, which is what a status-machine consumer needs. Both diagrams are faithful to the same state machine; the choice was to keep the README's diagram tight and topology-free since the README's scope is the schema, not the full pipeline topology.
- The "Creating a new finding" section deliberately documents the *manual* path (copy + edit) rather than the future `finding-intake` skill path, since the skill does not yet exist (Phase B). When the skill ships, this section is the natural place to update with the skill invocation as the primary path and the manual copy as the fallback.

**Next task pointer:** T-03 — create `specs/findings/_template/finding.md`. Dependencies T-01, T-02 satisfied (T-02 produces the field set that T-03 instantiates). No `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-03: Create `specs/findings/_template/finding.md`

**Status:** done
**Commits:** ab67d3f (deliverable), (this commit, closeout)
**Files touched:**
- specs/findings/_template/finding.md (new file, 65 lines)
- specs/findings/_template/ (new directory)

**Tests added:** N/A — inspection-based per T-03's "Tests required".

**DoD verification:**
- *File written:* specs/findings/_template/finding.md created.
- *Section structure matches design spec §5.1 verbatim:* Header block (Status / Domain / Severity / Operational urgency / Date opened / Last transition) + four phase sections (`## Intake`, `## Triage`, `## Investigation (optional)`, `## Route`) with field lists matching architecture.md:118-164.
- *Under ~200 lines:* 65 lines (verified via `wc -l`).
- *Committed:* ab67d3f.
- *Every README field-reference field present:* All 30 fields enumerated in T-02's field reference table appear in the template (verified field-by-field against specs/findings/README.md).
- *Operational urgency field present per OQ-1 → recorded decision (amendment sub-change H):* Yes, in the header block with `(optional)` marker and clarifying parenthetical "(typically operational findings only)".
- *Top-of-file HTML comment guides intake-first:* Yes — comment block instructs "Fill the Intake section first; later phases append" and cross-references README + design spec.

**Decisions made:**
- Verbatim mirror with one small clarifying gloss: the Operational urgency header line carries the parenthetical "(typically operational findings only)" inline rather than just `(optional)`. This is a minor surface-level addition that helps a first-time user understand when to populate the field; it is consistent with the amendment's severity-axis decoupling NFR row in design spec §6 and does not change the schema.
- HTML comment block includes a "Conventions when filling this template" sub-list (replace placeholders, use "unknown" honestly, leave future-phase sections in placeholder form, journal investigation-skip rationale). Goes slightly beyond the literal §5.1 template content but is supported by the design spec's intake behavior bullets (§5.2: "Unknown fields are recorded as 'unknown' rather than guessed") and §5.3–§5.4 (investigation may be skipped with journaled rationale).
- Did not include placeholder example values (e.g., "John Doe" for Reported by). Placeholders in `<angle brackets>` are clearer for both humans and AI agents than realistic-looking example values, which can be accidentally left in place.

**Spec amendments:** None.

**Surprises and learnings:**
- The §5.1 template's section heading `## Investigation (optional)` carries the `(optional)` marker as part of the heading itself — preserved verbatim. A new finding that skips investigation should still keep the section with placeholder values rather than delete it, so the finding's structure remains recognizable across reads. Recorded this guidance in the HTML comment.
- Followed the two-commit pattern (deliverable + closeout) per operator guidance after T-02's bundled commit. The deliverable commit (ab67d3f) lands the file; this closeout commit lands the journal update. Pattern matches prior feature (spec-path-convention).

**Next task pointer:** T-04 — create `specs/findings/_template/journal.md`. Dependency T-02 satisfied (README documents the journaling pattern, and the structural precedent at specs/20260515-spec-path-convention/journal.md is on hand). No `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-04: Create `specs/findings/_template/journal.md`

**Status:** done
**Commits:** 77136e9 (deliverable), (this commit, closeout)
**Files touched:**
- specs/findings/_template/journal.md (new file, 84 lines)

**Tests added:** N/A — inspection-based per T-04's "Tests required".

**DoD verification:**
- *File written:* specs/findings/_template/journal.md created.
- *Under ~100 lines:* 84 lines (verified via `wc -l`).
- *Committed:* 77136e9.
- *Structure matches feature-spec journal convention:* H1 `# <Short title> — Journal`; per-event `## <YYYY-MM-DD> — <Event>` sections; bolded inline fields. Matches precedent at specs/20260515-spec-path-convention/journal.md.
- *Placeholders clearly marked:* All values are `<angle-bracket>` placeholders, no plausible example values that could be accidentally left in place.
- *Documents one-entry-per-status-transition pattern:* Top HTML comment states the rule explicitly with design spec §6 Observability NFR citation. The skeleton entries below the active Intake entry instantiate the pattern.
- *Includes starter Intake entry with placeholders:* Active section "## <YYYY-MM-DD> — Intake" with Captured by, Signal source, New status, Notes fields aligned to finding.md Intake fields.

**Decisions made:**
- Included commented-out skeleton entries for the five subsequent transitions (Triaged / Under investigation / Investigation iteration / Routed / Closed / Reopened) at the bottom of the template. Goes beyond the literal T-04 scope ("Includes a starter 'Intake' entry with placeholders; documents the one-entry-per-status-transition pattern") but stays inside the spirit: the skeletons *are* how the pattern is documented — operationally, not just declaratively. Without them the operator has to construct each subsequent entry's shape from scratch, which invites drift from the convention.
- Added explicit `**Prior status:**` and `**New status:**` fields to each transition skeleton (other than the Intake starter, which has only `**New status:**` since intake originates the finding). This makes each entry self-documenting about the transition without requiring the reader to cross-reference the prior entry.
- Used HTML comments to hide the skeleton entries rather than including them as visible "examples". Hidden by default keeps the active journal clean for a freshly-copied template; the operator uncomments per transition. This is a small departure from the feature-spec journal precedent, which has no skeleton structure at all — feature-spec journals are written ad-hoc by the executing skill. Finding journals are more amenable to skeletoning because the transition shape is constrained by the state machine.

**Spec amendments:** None.

**Surprises and learnings:**
- The feature-spec journal pattern has no canonical template — feature-spec journals are produced ad-hoc by `spec-write` / `spec-execute` rather than copied from a template file. T-04 introduces the first template-driven journal in the methodology. The skeleton-entries-as-HTML-comments approach is the small innovation here; if it works well, it could backport to feature-spec journals (out of scope for this spec — flag for Phase F adoption review).
- The "Investigation iteration <N>" skeleton entry needed care to stay honest: status remains `under-investigation` across iterations, not "investigating-2" etc. Both `Prior status` and `New status` are `under-investigation` for an iteration in place, with a clarifying inline note "(iteration in place)" so the reader understands the status didn't change. Reflects design spec §5.4: "Investigation may iterate: a first investigation may produce a partial answer and journal 'needs deeper look'; status remains under-investigation until the route is chosen."

**Next task pointer:** T-05 — example-source validation exercise (operator-chosen recent journal note retroactively shaped as a finding under the templates). Dependencies T-02, T-03, T-04 satisfied. No `[blocker]` open questions; ready to proceed. Note: T-05 is a work-shape change — operator picks the source material, then exercises the templates end-to-end. Natural point for the session-continuity check.
