# Review verifies by inspection rather than refutation, and cannot record what it tried — Journal

## 2026-08-17 — Intake: review is structurally sound but epistemically neutral

**Captured by:** waseric; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Surfaced during a repository reconciliation review that compared an abandoned local design line against the current masters; that line has since been discarded. The abandoned line included an adversarial review approach whose *structural* commitments — independent cold-context reviewer, contract read first, tag discipline, drift as a finding, structured verdict written back by the coordinator rather than the reviewer — are all present in today's `spec-review` DISPATCH MODE and `spec-reviewer` agent definition, in several places more thoroughly. What did not carry across was its epistemic half. All claims below were re-verified against the current masters; the finding stands on its own and needs no pointer into the discarded material.

## 2026-08-17 — Triaged: confirmed by inspection; investigation skipped, remedy is prose in existing masters

**Triaged by:** waseric; persona-frame: developer
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably (structural)
**Domain/severity changes from intake:** domain recorded as `security` rather than the more obvious `methodology` — the mechanism is methodological, the exposure is not.
**Skip-investigation decision:** The four items are absences in shipped master text, confirmed by direct read and grep. No runtime behaviour to reproduce and no cause to hypothesise about; investigation would add ceremony without producing evidence.
**Notes:** Suggested routing for the operator's decision — `spec-amend` against existing masters in every case; no new skill is warranted, and the alternative of reinstating a separate adversarial-review skill was considered and rejected on the same reasoning the `dispatch-execution` design used against a parallel proposal: a sibling skill would duplicate the bulk of `spec-review`'s text and create drift risk.

| Item | Target |
|---|---|
| (1) attack-construction guidance for security review | `.agents/skills/spec-review/SKILL.md` Phase 5 NFR walk |
| (2) gate/judgment false-pass check | `.agents/skills/spec-review/SKILL.md` Phase 5 |
| (3) record of attacks attempted and repelled | `.agents/skills/spec-review/SKILL.md` Phase 7 verdict; `.agents/agents/spec-reviewer.md` |
| (4) an outcome expressing reviewer non-confidence | `.agents/skills/spec-review/SKILL.md` Phase 7 outcome set |
| (5) missing exit criteria treated as unverifiable, never auto-pass | `.agents/skills/spec-review/SKILL.md` Phase 4 |
| (6) security tasks require bypass-attempting tests | `.agents/agents/spec-worker.md` |

Sequencing note: (1) and (2) are the substance and should land together — roughly three to four paragraphs folded into the Phase 5 walk. (3) is cheap but interacts with review artifact length; a repelled-attack log is only worth its space if it stays terse. (4) is the item most likely to be contested: adding a fifth outcome touches the checkpoint state machine and the exit-criteria contract, so it may deserve its own amendment cycle rather than riding along with the Phase 5 prose. (5) and (6) are one-liners.

**Deliberately excluded from this finding.** Per-task adversarial verification — running an independent refutation pass against every task rather than at declared checkpoints — was considered and left out. The `dispatch-execution` design consciously traded it for economy under "starve context, not verification", replacing it with the orchestrator's derivation re-check. That trade has a known residual risk: the re-check re-runs only the command the worker itself chose to journal, so a worker that journals a weak command passes its own check ([receipt-schema.md](../../../.agents/skills/spec-execute/receipt-schema.md) makes the re-runnable command a worker-authored field). The right response is guidance in `spec-write` that security-relevant and low-verifiability tasks must be checkpoint-guarded with a top reviewer floor — a doctrine gap, not a mechanism gap — rather than reinstating per-task review. Recorded here so the reasoning is not re-litigated from scratch; not filed as a separate finding because it is a known accepted trade rather than an oversight.
