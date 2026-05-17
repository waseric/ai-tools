<!--
This is a template. Copy this file to specs/findings/YYYYMMDD-<short-name>/journal.md
when creating a new finding. The Intake entry below is the starter; append one
new entry per status transition as the finding moves through the pipeline.

See specs/findings/README.md for the schema, state machine, and status semantics.
See specs/20260517-findings-pipeline/architecture.md for the design spec.

Journaling convention:
- One entry per status transition. Every transition is journaled — no transition
  is silent (design spec §6 Observability NFR).
- Section header format: `## <YYYY-MM-DD> — <New status>: <one-line summary>`.
- Entries are append-only. Corrections to earlier entries are made by appending
  a new entry that supersedes the old, not by rewriting in place.
- The starter Intake entry below uses placeholders matching the finding template.
- Skeleton entries for later transitions are shown commented-out at the bottom;
  uncomment and fill as the finding progresses.

This leading HTML comment block AND the closing commented-out skeleton block
at the end of this file are template scaffolding — strip both from produced
journal.md artifacts. The skeleton entries are re-added (uncommented and filled)
by downstream skills at the moment of each status transition. See
.agents/skills/finding-intake/SKILL.md Phase 3 step 3.
-->

# <Short title> — Journal

## <YYYY-MM-DD> — Intake: <one-line summary of the captured signal>

**Captured by:** <name; persona-frame: intake>
**Signal source:** <text / URL / system pointer; same value as finding.md "Reported via">
**New status:** `intake`
**Notes:** <anything relevant to the act of capture that did not fit in finding.md — context, related observations, who else might know about this signal>

<!--
Subsequent entries follow the one-event-per-section pattern. Uncomment and fill
each as the corresponding transition happens. Suggested skeletons:

## <YYYY-MM-DD> — Triaged: <one-line summary of triage outcome>

**Triaged by:** <name; persona-frame: business analyst | service desk | developer>
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** <reliably | intermittently | not reproduced | not applicable>
**Domain/severity changes from intake:** <if intake's best guess was refined>
**Skip-investigation decision (if any):** <if triage chose to route directly, the rationale>
**Notes:** <triage observations not captured in finding.md Triage section>

## <YYYY-MM-DD> — Under investigation: <one-line summary of investigation scope>

**Investigated by:** <name; persona-frame: developer>
**Prior status:** `triaged`
**New status:** `under-investigation`
**Initial hypothesis:** <starting hypothesis for this investigation pass>
**Notes:** <scope of investigation; expected duration; what would close vs. defer>

## <YYYY-MM-DD> — Investigation iteration <N>: <one-line summary of what this iteration tested>

**Investigated by:** <name; persona-frame: developer>
**Prior status:** `under-investigation`
**New status:** `under-investigation`  (iteration in place)
**What was learned:** <new evidence; refined or rejected hypotheses>
**Next step:** <what closes this investigation; or "needs deeper look" with what>

## <YYYY-MM-DD> — Routed: <one-line summary of routing decision>

**Decided by:** <name; persona-frame and operator>
**Prior status:** `triaged` or `under-investigation`
**New status:** `routed`
**Route subtype:** <spec-amend | spec-write | defer>
**Target spec (if amend or new-spec):** <path>
**Watch condition (if defer):** <what should cause re-evaluation>
**Rationale:** <one paragraph; why this route over the others — same as finding.md Route Rationale, may cross-reference>

## <YYYY-MM-DD> — Closed: <one-line summary of close rationale>

**Decided by:** <name; persona-frame and operator>
**Prior status:** `triaged` or `under-investigation`
**New status:** `closed`
**Close reason:** <cannot reproduce | expected behavior | out of scope | superseded | other>
**Rationale:** <one paragraph; why close rather than route>

## <YYYY-MM-DD> — Reopened: <one-line summary of what re-activated this finding>

**Reopened by:** <name; persona-frame matches the phase being returned to>
**Prior status:** `routed` or `closed`
**New status:** `reopened` → returning to <triaged | under-investigation>
**Trigger:** <what surfaced that warrants reopening: new signal, watch-condition met, route rejected by receiving spec>
**Notes:** <what changed; what the next phase needs to attend to>
-->
