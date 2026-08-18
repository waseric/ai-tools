# Karpathy's LLM Wiki pattern exposes lifecycle gaps in the vault-bootstrap archetype — Journal

## 2026-08-18 — Intake: external pattern doc evaluated against the vault-bootstrap spec surfaces six lifecycle gaps and two challenges

**Captured by:** waseric; persona-frame: intake
**Signal source:** text + URL
**New status:** `intake`
**Notes:** Signal originated as an operator request to evaluate an external pointer against the in-progress `vault-bootstrap` design spec, so the analysis was complete before the finding existed — intake captured it rather than eliciting it. Two things about the act of capture are worth recording:

*Fetch outcome.* The gist body was fetched successfully via `curl` against the `/raw` endpoint (HTTP 200, 11,985 bytes). Two other paths failed first and the failures are worth knowing about, since they will recur: both `gist.github.com` and `gist.githubusercontent.com` are **blocked by policy in this environment's browser pane**, and a `WebFetch` against the raw endpoint **declined to reproduce the body**, returning an explanation that a verbatim-reproduction request conflicted with its own quoting limits. `curl` was the only path that worked.

*Snapshot is a digest, not a verbatim capture.* Per OP #3 this is journaled as an explicit decision rather than left implicit. The skill's guidance is to quote the load-bearing portion verbatim; that was deliberately not done, to avoid wholesale reproduction of a third-party document. What landed instead is a structural digest naming every layer, operation, file, and rationale in the source, with short quoted fragments only. The durability cost is accepted and stated in [analysis.md](analysis.md#snapshot-fidelity): if the gist goes away, the pattern survives but the author's phrasing does not. No claim in the analysis depends on re-fetching.

*One reference is weaker than the rest.* The gist's **comment thread** was never obtained as text — only as a WebFetch summary. It is labelled medium-confidence in both `finding.md` and `analysis.md`, and analysis item 8 (audience separation) rests on it alone. Anyone acting on item 8 should re-read the comments first.

*Sibling file.* The analysis itself lives in `analysis.md` rather than in the Summary field, following the precedent set by [20260817-knowledge-vault-bootstrap-gap](../20260817-knowledge-vault-bootstrap-gap/finding.md)'s `source-signal.md`. The Summary carries the load-bearing capture and names the four most consequential items, so the finding stands alone even if `analysis.md` is never opened.

## 2026-08-18 — Triaged: confirmed methodology/important, not a defect, not a duplicate; investigation skipped

**Triaged by:** waseric; persona-frame: business analyst
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** not applicable
**Domain/severity changes from intake:** None. `methodology` was unambiguous at intake and is confirmed. Severity was not set at intake (correctly — it is not an intake field) and is set at triage to `important`: two of the eight items are cheap corrections to reasoning the spec has already done and then scoped down (§5.2b's staleness anti-goal over-reaching from the mechanical checker onto semantic lint; OQ-2 conflating a one-shot bootstrap stub with a standing operations log), which is the profile of a real miss rather than a preference. Not `blocker` — nothing here falsifies a premise the spec depends on, nothing blocks CP-1, and the spec is internally coherent as written.
**Skip-investigation decision (if any):** Investigation skipped. The route was obvious at triage and investigation has nothing to add: there is no cause to diagnose (the signal is a scope-boundary comparison between two documents, not a behavior), no code touchpoints (`vault-bootstrap` has no `SKILL.md` and nothing is deployed), and the remedy shape for each item is already enumerated in `analysis.md`'s closing section. Opening the codebase would have re-derived the analysis rather than extending it. The Investigation section is left in `<placeholder>` form rather than deleted, per schema convention.
**Notes:** Triage confirmed three things without investigating:

1. **Not a defect.** Every item is scope-boundary rather than error. The spec does what it claims; the claim is narrower than the archetype's eventual need. The framing that carried the most weight — and the most useful output of the whole comparison — is that the two documents are *adjacent halves*: the gist is lifecycle doctrine that explicitly declines to specify structure, the spec is a bootstrap mechanism that specifies no operations. That reframing is what makes the gaps tractable instead of adversarial.
2. **Not a duplicate.** [20260817-knowledge-vault-bootstrap-gap](../20260817-knowledge-vault-bootstrap-gap/finding.md) (`routed`) concerns the *absence of a bootstrap mechanism*; this finding concerns the *scope of what gets bootstrapped*. Same target spec, disjoint questions — and the earlier finding's route is what produced the artifact this one evaluates.
3. **One item could stand alone.** Analysis item 7 — the archetype's "prose-only, no runtime code" definition in §1 already contradicted by OQ-1's leaning to ship an executable validator into the produced vault — is internal to the spec and true with or without the gist. It is kept in this bundle because the gist is what made it visible, but a future session may reasonably split it out.

## 2026-08-18 — Closed: filed as captured-for-consideration at operator direction; no watch condition to defer against

**Decided by:** waseric; persona-frame: business analyst, with operator decision
**Prior status:** `triaged`
**New status:** `closed`
**Close reason:** other — captured for future consideration; analysis durably filed, no work opened
**Rationale:** Closed at the operator's explicit direction, whose stated goal was to make the analysis reachable rather than to open a work item. The artifact meets that goal on its own: `analysis.md` cites the spec by section, carries the gist digest, and sizes each item, so a future session can act without this conversation or a live fetch. Routing to `spec-amend` was considered and rejected on **timing rather than merit** — the target spec sits mid-review at CP-1 with operator approval outstanding, and four of the eight items would widen its scope while it is being reviewed for whether its *current* boundary is drawn correctly. `defer` was the closest alternative and was rejected because it implies a watch condition the pipeline should track, and there is none: nothing external will change to make these items ripe. The trigger is an operator decision about archetype scope, which is not a condition worth polling. Reopen path if that decision is made: reopen this finding, or author a fresh one citing it.

One deliberate non-action worth recording, since it is the kind of thing a later reader will wonder about: **no change was made to the `vault-bootstrap` spec, its CP-1 status, or its open-question set.** CP-1 remains open and awaiting re-review exactly as it was. Analysis item 8 is checkpoint input that argues against a decision CP-1 is already reviewing, and leaving it filed-but-unrouted means the checkpoint will not see it unless the operator raises it. That is the accepted cost of closing rather than routing, and it is stated here rather than left to be discovered.
