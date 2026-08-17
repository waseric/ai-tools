# Agent-tool model alias silently downgrades to session default, defeating declared model floors — Finding

> Status: closed
> Domain: methodology
> Severity: important           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-17
> Last transition: 2026-08-17                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self (operator; observed 2026-07-28 during a `spec-execute` dispatch run, captured retroactively during a repository reconciliation)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** Dispatch execution's central guarantee is that "model floors survive delegation" — [dispatch-execution/architecture.md:21](../../20260705-dispatch-execution/architecture.md) states "the worker's model is pinned at spawn to the task's declared floor." The mechanism specified to deliver it does not hold when the requested tier is not entitled to the account. The Agent tool's `model` parameter accepts **only** the aliases `sonnet | opus | haiku | fable` and rejects dated build IDs outright; the aliases resolve to the *latest* build in each tier. When a dispatch requests an alias the account is not entitled to, the harness does **not** error — it **silently falls back to the session's default model**. A worker dispatched with `model: "opus"` therefore ran on Sonnet, silently, with the declared floor unmet and nothing in the receipt or journal recording the substitution. This bit a `spec-execute` dispatch run **twice** before being caught, and was caught only because the worker happened to self-report its own model identity string — not by any check the dispatch contract requires. The workaround built at the time exploited a second, independent resolution path: an agent definition's `model:` frontmatter **does** accept a dated build ID (`model: claude-opus-4-8`) and **overrides parent inheritance**, so per-floor worker variants (`spec-worker-opus.md`, `spec-worker-sonnet.md`) were authored and dispatched with the tool's `model` param **omitted** so the frontmatter pin would take effect. Observed resolution order: tool `opts.model` (alias enum) → agent-def `model:` frontmatter (dated IDs accepted) → parent session model. Caveat found alongside it: the registry loads agent definitions at **session start** and hot-reload is unreliable, so adding or renaming a variant requires a fresh session. This finding is captured as **precedent only** — see Route.
**External references:** none

## Triage

**Triaged by:** waseric; persona-frame: developer
**Triage date:** 2026-08-17
**Reproducibility:** reliably at time of observation (2026-07-28, twice in one run); **not currently reproducible** — the precondition (requesting a non-entitled alias) no longer holds for this account
**Repro steps (if reproducible):**
1. On an account **not** entitled to the tier an alias resolves to, dispatch a subagent via the Agent tool with that alias, e.g. `model: "opus"` where `opus` resolves to a non-entitled build.
2. Instruct the worker to read and report its own environment model line as its first action.
3. Observe: no error, no warning — the worker reports the *session default* model, not the requested tier. The declared floor is silently unmet.
4. Contrast: set `model: claude-opus-4-8` in an agent definition's frontmatter and dispatch with the tool `model` param omitted. The worker reports the pinned dated build, confirming the two resolution paths differ.
**Scope:** Every dispatch-mode execution that relies on per-spawn model floors — `spec-execute` under `EXECUTION: dispatch` (worker floors) and `spec-review` under `REVIEW_EXECUTION: dispatch` (reviewer floors). Impact is highest exactly where floors matter most: the high-floor, low-verifiability tasks and the most consequential checkpoints, since those are the ones whose floors are set above the orchestrator's own tier and therefore the ones most likely to request a tier the session does not already hold.
**Domain confirmation:** methodology
**Severity confirmation:** important
**Triage notes:** Recorded as `methodology` rather than `other` because the exposure is a spec guarantee that silently does not hold, not a tooling inconvenience. Worth naming the specific tension for whoever reopens this: [architecture.md:202](../../20260705-dispatch-execution/architecture.md) (§5.7) states "Model is deliberately **not** pinned in the definition — it is set per-spawn to the task's floor," and the [OQ-4 resolution at line 350](../../20260705-dispatch-execution/architecture.md) reaffirms it — "Model is left unpinned in the frontmatter (defaults to `inherit`) so the per-spawn `model` override sets the floor." Both commitments are correct *given an entitled alias* and silently wrong otherwise; the spec has no failure mode for "the requested floor was not honored." The durable mitigation, independent of entitlement, is a **STOP-AND-VERIFY preamble** in the worker brief for any floored task: instruct the worker to read its own environment model line first and return `blocked` if below floor, so a residual downgrade self-halts rather than completing under-floor. That mitigation is cheap, works regardless of which resolution path is in play, and is the piece worth carrying forward even now that the workaround is retired. Investigation skipped: the mechanism was already fully diagnosed at the time of observation (both resolution paths identified and verified empirically — a worker pinned to `claude-opus-4-5` from a `claude-opus-4-8` session reported `claude-opus-4-5`), so there is no open hypothesis for an investigation phase to test. Likely a harness defect worth reporting upstream: a non-entitled alias should error, not silently downgrade.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** close
**Decided by:** waseric; persona-frame: developer, and operator
**Route date:** 2026-08-17
**Target spec:** n/a — closed without amendment
**Route rationale:** Closed because the precondition was removed rather than the defect fixed: the corporate account has since been granted entitlement to the head Opus model, so no dispatch this repo issues currently requests a non-entitled alias, and the per-spawn override specified in §5.7 / OQ-4 now works as written. The two variant agent definitions (`spec-worker-opus.md`, `spec-worker-sonnet.md`) existed only as deploy copies at `~/.claude/agents/` — they were never mastered in this repo, in deliberate deference to §5.7 — and were deleted on 2026-08-17 as part of the go-forward deploy sync. Closing rather than amending §5.7 is the operator's call: amending the spec to bless frontmatter pinning would encode a workaround for a condition that no longer holds, and the capability is cheap to rebuild from this record (the variants differed from `spec-worker.md` only in `name`, `description`, and one added `model:` line — bodies were byte-identical). **This finding is retained as precedent, not as an open item.** Reopen if any of the following surface: entitlement to a tier a spec declares as a floor is withdrawn or changes; a worker or reviewer is observed running below its declared floor; or the STOP-AND-VERIFY preamble described in Triage notes is adopted as a standing brief requirement, in which case §5.7 should gain an explicit "floor not honored" failure mode and this finding becomes its supporting evidence.
