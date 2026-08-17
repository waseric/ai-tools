# Dispatch autonomy lacks entry preconditions, retry bounds, and floor-confidence signals — Journal

## 2026-08-17 — Intake: four guardrail absences in the now-default autonomous dispatch path

**Captured by:** waseric; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Surfaced during a repository reconciliation review that compared an abandoned local design line against the current masters. That design line proposed an orchestration approach which the `dispatch-execution` design spec superseded on the record — "No new skill. Dispatch is a mode of `spec-execute`" ([architecture.md §2](../../20260705-dispatch-execution/architecture.md)) — and the abandoned line has since been discarded. The supersession was correct and is not in question; what this finding captures is a short tail of hardening details that the abandoned line had specified and that the superseding design did not carry across. Every claim here was re-verified against the current masters rather than taken from the abandoned material, and the finding is written to stand on its own: no pointer into the discarded line is required to act on it.

## 2026-08-17 — Triaged: confirmed by inspection; investigation skipped, remedy is wording in existing masters

**Triaged by:** waseric; persona-frame: developer
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably (structural)
**Domain/severity changes from intake:** none — methodology / important held.
**Skip-investigation decision:** All four items are absences in shipped master text, confirmed by direct grep and read (see finding.md Repro steps). There is no cause to hypothesise about and no runtime behaviour to reproduce, so the Investigation phase would add ceremony without evidence. Routing directly.
**Notes:** Suggested routing for the operator's decision — all five items are `spec-amend` against existing masters, none needs a new spec:

| Item | Target |
|---|---|
| (1) spec-approval precondition | `.agents/skills/spec-execute/SKILL.md` Phase 1 |
| (2) re-dispatch attempt bound | `.agents/skills/spec-execute/SKILL.md` (the three re-dispatch sites) |
| (3) receipt confidence field | `.agents/skills/spec-execute/receipt-schema.md` Shape |
| (4) worker floor self-report framing | `.agents/agents/spec-worker.md` |
| (5) floor-ladder calibration method | `.agents/skills/spec-write/SKILL.md` `MODEL_FLOOR_POLICY` |

Sequencing note: (1) is independently valuable and by far the cheapest — it can land alone without waiting on the others. (3) interacts with the receipt's 25-line cap; adding a field consumes one line of a deliberately scarce budget, so it should be weighed against the cap's rationale ("Why capped") rather than added reflexively. (5) is the weakest of the five: the calibration probe named in triage is drawn from prose-review work and may not generalise to code-heavy consuming projects — it may deserve `defer` with a watch condition even if (1)–(4) are amended.
