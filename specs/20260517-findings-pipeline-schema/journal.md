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
