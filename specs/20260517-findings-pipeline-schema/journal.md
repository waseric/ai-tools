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
