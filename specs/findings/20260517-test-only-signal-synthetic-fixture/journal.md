# Test-only signal — synthetic-fixture-naming inconsistency — Journal

## 2026-05-17 — Intake: synthetic-fixture-naming inconsistency captured for T-02 skill validation

**Captured by:** waseric; persona-frame: intake
**Signal source:** text (T-02 synthetic exercise — fabricated, not a real signal)
**New status:** `intake`
**Notes:**
- This finding is the T-02 synthetic validation artifact for the finding-intake skill (spec: [specs/20260517-finding-intake-skill/feature.md](../../20260517-finding-intake-skill/feature.md)). It is fabricated and does not represent a real observation. Retain-vs-delete decision is made at T-02 closeout and recorded in this feature spec's journal.
- The fabricated narrative is methodology-domain to avoid colliding with the operational-domain T-05 living example (`tab-display-issues`). The signal is plausible-shaped but should not be acted on as if real.

## 2026-05-17 — Triaged: synthetic-fixture-naming inconsistency triaged as advisory methodology finding, reliably reproducible

**Triaged by:** waseric; methodologist; persona-frame: triage
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably (anyone listing `specs/findings/` and reading the spec-path-convention spec will observe the same gap)
**Domain/severity changes from intake:** Domain unchanged (methodology confirmed). Severity set to `advisory` — was `<placeholder>` at intake (severity is a triage-time field per the schema), so this is a first-population, not a delta from a prior triage value.
**Skip-investigation decision (if any):** end at `triaged` — investigation not invoked at this transition. The natural route (when triaged onward) is likely `spec-amend` against [specs/20260515-spec-path-convention/architecture.md](../../20260515-spec-path-convention/architecture.md), but the route decision belongs to a later transition, not to this Triaged entry.
**Pointer revalidation:** not-applicable — no external pointer present in the Intake `External references` field.
**Notes:**
- This finding now serves as the T-02 validation artifact for the finding-triage skill (spec: [specs/20260517-finding-triage-skill/feature.md §7 T-02](../../20260517-finding-triage-skill/feature.md#t-02--synthetic-validation-exercise)) — in addition to its prior role as T-02 validation artifact for the finding-intake skill. The fixture has transitioned permanently from `intake` to `triaged`; it continues to be a regression reference, now as an end-state-Triaged example paralleling [20260517-tab-display-issues/](../20260517-tab-display-issues/) (end-state-Investigation).
- Triage produced in structured-input mode by Claude on the operator's behalf via the just-authored `.agents/skills/finding-triage/SKILL.md`. No interactive prompts were issued; persona-frame `methodologist` was used as supplied (the Domain → frame derivation suggested the same value, so accept-vs-override was a no-op).
- Hard-facts discipline held: no cause hypothesis appears in the Triage section. The route hypothesis ("amend spec-path-convention") is noted in the Skip-investigation field of *this journal entry*, not in the finding's Triage section, because routing is a triage-output question, not a triage-content question.
