# session-handoff finding leakage — Journal

## 2026-05-19 — Intake: findings mentioned in spec-execute journals not formally filed at session boundaries

**Captured by:** Eric Wasgatt; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Observed on the poc-reference-integration spec (private-design-repo repo). Five findings were mentioned across journal entries during a single spec-execute session but none were filed until a post-session spec-review caught them. The spec-execute skill's session boundary check (Phase 8) focuses on context load and task progress but does not scan for unfiled finding references. A lightweight addition to Phase 6 (task closeout) or Phase 8 (session boundary) — scanning the current session's journal entries for "finding to be filed" / "file a finding" patterns and prompting batch intake — would close this gap. Routes to spec-amend against the spec-execute skill spec or directly against the skill's SKILL.md.

## 2026-05-19 — Triaged: spec-execute skill lacks finding-filing step at task closeout and session boundaries

**Triaged by:** Eric Wasgatt; methodologist; persona-frame: triage
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably
**Domain/severity changes from intake:** none — intake values confirmed (methodology / important)
**Skip-investigation decision (if any):** end at triaged
**Pointer revalidation:** not applicable — no external pointers
**Notes:** Scope is broader than spec-execute alone: any pipeline phase encountering an out-of-scope problem should be able to create a finding to offload the concept without distraction. Currently only /spec-review catches unfiled findings after the fact.
