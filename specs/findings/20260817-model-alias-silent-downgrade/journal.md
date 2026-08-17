# Agent-tool model alias silently downgrades to session default, defeating declared model floors — Journal

## 2026-08-17 — Intake: Agent-tool `model` alias silently resolves to session default when the tier is not entitled, so declared model floors go unmet without error

**Captured by:** waseric; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Captured retroactively. The behavior was observed on 2026-07-28 during a `spec-execute` dispatch run; this artifact was created on 2026-08-17 during a repository reconciliation, when the two workaround agent definitions were found deployed at `~/.claude/agents/` with no master in any ref of this repo. The reconciliation is what prompted the capture — the workaround was about to be deleted, and deleting it would have taken the diagnosis with it. Everything load-bearing is in the finding's Summary and Triage notes; the originating session is not required.

## 2026-08-17 — Triaged: mechanism fully diagnosed at observation time; precondition since removed by an entitlement grant

**Triaged by:** waseric; persona-frame: developer
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably at time of observation (twice in one run); not currently reproducible — requesting a non-entitled alias is no longer possible on this account
**Domain/severity changes from intake:** none — `methodology` / `important` confirmed as captured. Domain considered and rejected: `other`. The exposure is a spec guarantee that silently does not hold ([architecture.md:21](../../20260705-dispatch-execution/architecture.md) — "model floors survive delegation"), which is methodological, not merely a tooling quirk.
**Skip-investigation decision (if any):** Investigation skipped. Both resolution paths were identified and verified empirically at the time of observation — the tool-param alias enum (rejects dated IDs, silently downgrades on non-entitlement) and the agent-def `model:` frontmatter path (accepts dated IDs, overrides parent inheritance, confirmed by a worker pinned to `claude-opus-4-5` from a `claude-opus-4-8` session reporting `claude-opus-4-5`). No open hypothesis remains for an investigation phase to test.
**Notes:** Two things surfaced in triage that are not defects in this repo but belong in the record. First, the §5.7 / OQ-4 tension: both commit to leaving model unpinned so the per-spawn override sets the floor, and both are correct given an entitled alias and silently wrong otherwise — the spec has no "floor not honored" failure mode. Second, the entitlement-independent mitigation: a STOP-AND-VERIFY preamble in the worker brief for floored tasks (worker reads its own model line first, returns `blocked` if below floor). That mitigation survives this finding's closure and is the piece worth carrying forward.

## 2026-08-17 — Closed: precondition removed by entitlement grant; retained as precedent, workaround retired

**Decided by:** waseric; persona-frame: developer, and operator
**Prior status:** `triaged`
**New status:** `closed`
**Close reason:** other — the precondition was removed rather than the defect fixed
**Rationale:** The corporate account has since been granted entitlement to the head Opus model, so no dispatch this repo issues currently requests a non-entitled alias and the per-spawn override specified in §5.7 / OQ-4 now works as written. The operator's decision was to close rather than amend: amending §5.7 to bless frontmatter pinning would encode a workaround for a condition that no longer holds, and the capability is cheap to rebuild from this record — the two variants differed from `spec-worker.md` only in `name`, `description`, and one added `model:` line, with byte-identical bodies. The variants were deleted from `~/.claude/agents/` on 2026-08-17; they never had masters in this repo, in deliberate deference to §5.7. Reopen triggers are enumerated in the finding's Route rationale: entitlement to a declared floor tier changing, an observed below-floor worker or reviewer, or adoption of the STOP-AND-VERIFY preamble as a standing brief requirement (in which case §5.7 should gain an explicit "floor not honored" failure mode and this finding becomes its supporting evidence).
