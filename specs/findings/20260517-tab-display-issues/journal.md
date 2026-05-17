# TAB list display issues across Sandlot servers — Journal

## 2026-05-17 — Intake

**Captured by:** waseric; persona-frame: intake
**Signal source:** Forum thread https://www.sandlotminecraft.com/threads/tab-screen.39849/
**New status:** `intake`
**Notes:**
- Retroactive intake: the original community signal entered on 2026-05-12 via the linked forum thread. Captured as a finding here on 2026-05-17 as the T-05 example-source validation exercise for the findings-pipeline-schema feature spec ([specs/20260517-findings-pipeline-schema/feature.md](../../20260517-findings-pipeline-schema/feature.md)). A real-time intake would have happened on the day of the forum post.
- The thread iterated through partial fixes and re-reports between 2026-05-12 and 2026-05-15. The `intake` status on this artifact is the *artifact's* lifecycle state, not the underlying issue's state — the underlying issue is roughly mid-investigation when captured.
- Other community members who can extend context: trubuhl (initial confirmation), SupersonicE9 (AFK-tag and mod-color reporter), WanderingLlama (screenshot evidence), MangoBreeze (game-mode-list issue), EFret17 (Bedrock-specific scoping report).

## 2026-05-17 — Triaged

**Triaged by:** waseric; persona-frame: business analyst (with admin/operator overlap — solo-operator persona collapse)
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably (multi-user, cross-client confirmations in the thread)
**Domain/severity changes from intake:** None. Domain held at operational; severity held at advisory; operational urgency held at P3.
**Skip-investigation decision (if any):** Did not skip — investigation is entered to document the hypothesis surface for future reference, even though no full root-cause is available in the visible artifact.
**Notes:** Triage was captured in the same session as Intake because the finding is retroactive. In a real-time finding the triage would lag the intake by hours to days. Reproducibility is rated "reliably" based on multi-user, multi-day confirmation in the thread, not on a fresh first-party repro.

## 2026-05-17 — Under investigation

**Investigated by:** waseric; persona-frame: developer (with admin overlap)
**Prior status:** `triaged`
**New status:** `under-investigation`
**Initial hypothesis:** Coupled-display-pipeline hypothesis (see [finding.md Investigation Probable cause](finding.md)) — a single TAB/scoreboard plugin configures multiple visual dimensions, and a fix to one dimension perturbs another.
**Notes:** Investigation is paused pending an operator decision on next route: (a) audit the production server's TAB plugin config as a new spec, (b) close the finding if the latest "looks normal" reports represent stable state, or (c) defer with a watch condition (e.g., reopen on the next forum-thread re-report). Recorded as `under-investigation` for now because the routing decision has not yet been made.
