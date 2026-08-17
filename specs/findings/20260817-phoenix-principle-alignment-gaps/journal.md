# Phoenix Principle alignment gaps — Journal

## 2026-08-17 — Intake: external manifesto assessed against the skill family; Tests-as-Truth gap identified as the mineable divergence

**Captured by:** waseric; persona-frame: intake
**Signal source:** text + URL
**New status:** `intake`
**Notes:** Signal originated from the operator asking for an evaluation of Bergel's Phoenix Principle manifesto against this repo's methodology, explicitly without making changes, and asking that the assessment be captured so it can be mined later. The finding is therefore a *durable record of an evaluation*, not a defect report — triage should expect `Reproducibility: not applicable`.

The external pointer was fetched live during the originating session on 2026-08-17; the fetch succeeded and the load-bearing content is snapshotted verbatim-in-substance in `finding.md` under `External references`. No fetch failure to report. Medium articles are paywall-variable and the snapshot is what makes this artifact self-contained — do not assume the URL will re-fetch cleanly later.

Two of the three identified divergences (Deletion Test / Phoenix Architecture regenerability; specs-as-delta rather than specs-as-complete-behavior) were assessed as **deliberate consequences of the lifecycle mission** in [mission.md](../../mission.md) and roadmap Phases 2–3, not as gaps. They are recorded for completeness and to prevent a future re-derivation of the same analysis, but the operator should expect them to route to `close` or `defer` rather than to a spec change.

The third — **Tests as Truth** — is the one flagged as worth evaluating properly. The specific claim: `spec-write` §7 makes tests an *evidence* item under Definition of Done and §8 Test Strategy describes approach rather than contract, so a feature spec's correctness claims exist only as prose Given/When/Then. Nothing in the family produces an executable, implementation-independent correctness artifact. This bears on the CLAUDE.md design bar's *rework prevention* property ("mechanically re-derivable claims") and trades against *token economy*. Relevant touchpoints if this is later investigated: `.agents/skills/spec-write/SKILL.md` §7 Task Breakdown / §8 Test Strategy, and `.agents/skills/spec-execute/SKILL.md` Phase 5 DoD verification.

Adjacent-but-not-part-of-this-finding: the assessment also noted that `journal.md` + the `Current State` block is a durability mechanism with **no** Phoenix analogue — the manifesto's five files capture a snapshot of intent, whereas the journal captures the derivation of intent over time. Recorded here as a positive-alignment observation in case a future reader mistakes the journal for redundant ceremony.

Parallel unrelated work was in flight in this repository at capture time; per intake policy nothing was staged or committed by the skill.

## 2026-08-17 — Triaged: advisory methodology gap; scoped to correctness-contract executability in spec-write §7–8

**Triaged by:** waseric; methodologist; persona-frame: triage
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** not applicable
**Domain/severity changes from intake:** none — intake's `methodology` domain confirmed; `Severity: advisory` set for the first time (it was in placeholder form at intake).
**Skip-investigation decision (if any):** skip chosen — triage established the affected surface, the tradeoff axis, and enough to decide the route. The questions that remain are design questions for a future session, not diagnostic ones, so opening code to hypothesize about cause would produce nothing the route decision needs. Routed entry follows.
**Pointer revalidation:** `https://medium.com/@bergel/the-phoenix-principle-a-manifesto-for-programmers-in-the-ai-age-ca63317c5ebc` — `treated-as-static`. The intake Summary is rich (names the affected components, quotes the manifesto's six principles verbatim-in-substance, carries a same-day snapshot), so the default applies and no re-fetch was attempted. The pointer was fetched successfully earlier the same day; a manifesto is a static document by nature, and the snapshot is what makes the artifact self-contained if the URL later paywalls.
**Notes:** Triage deliberately narrowed the finding. Intake recorded three divergences; only one — Tests as Truth / correctness-contract executability — is carried forward as the actionable scope. The other two (Deletion Test and the Phoenix Architecture regenerability model; specs-as-delta vs. specs-as-complete-behavior) are scoped **out** at triage: both follow necessarily from the lifecycle mission in [mission.md](../../mission.md) and roadmap Phases 2–3, so treating them as gaps would be a category error. They stay in the artifact so a future reader does not re-derive the same analysis and reach the same dead end.

Two cause-shaped hypotheses were surfaced and are recorded as **deferred**, not as triage claims: that the missing executable-contract artifact is a deliberate token-economy tradeoff rather than an oversight; and that a `tests.yaml`-equivalent may conflict with the tooling-agnostic stance in `mission.md` Out of Scope. Neither was tested — per the stay-out-of-code discipline, no implementation source was read to evaluate them.

No deduplication match found against the twelve existing findings in `specs/findings/`; none touch correctness-contract executability.

## 2026-08-17 — Routed: defer, with a five-part watch condition

**Decided by:** waseric (operator); methodologist; persona-frame: triage
**Prior status:** `triaged`
**New status:** `routed`
**Route subtype:** defer
**Target spec (if amend or new-spec):** not applicable. Likely future target if re-evaluated: `specs/20260518-spec-write-skill/` via `spec-amend` against §7–8, coupled with `specs/20260518-spec-execute-skill/`.
**Watch condition (if defer):** (a) a rework incident traced to a feature spec's correctness claims being un-re-verifiable after its originating session ended; (b) a `spec-review` checkpoint unable to mechanically confirm a prior task's Definition of Done, forced to re-derive intent from prose; (c) dispatch-mode execution producing a plausible-but-wrong change that passes its own tests, where an implementation-independent contract would have caught it; (d) an outside adopter asking for executable acceptance criteria; (e) roadmap Phase 2 feedback-loop work independently surfacing a need for machine-checkable spec assertions.
**Rationale:** See the finding's Route section for the full rationale. In short: the operator asked for capture, not change; nothing is broken; and amending `spec-write` would impose a new required artifact class on every feature spec, spending *token economy* against a *rework prevention* benefit that has not yet been observed to bite. Close was rejected because the observation is substantive and bears on a stated design-bar property.

Watch condition (c) is worth flagging to a future reader as the one most likely to fire first — it overlaps with the concerns already captured in [20260817-review-verifies-by-inspection-not-refutation](../20260817-review-verifies-by-inspection-not-refutation/) and [20260817-dispatch-autonomy-lacks-preconditions-and-bounds](../20260817-dispatch-autonomy-lacks-preconditions-and-bounds/). If those two are ever worked together, this finding should be pulled into the same session rather than re-evaluated alone; the three share a root question about what makes an agent's self-verification trustworthy.
