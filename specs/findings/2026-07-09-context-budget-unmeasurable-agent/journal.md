# Context budget gates on a token count the agent can't self-measure — Journal

## 2026-07-09 — Intake: context-budget threshold is unmeasurable by the agent; excursions bypass the receipt-cap assumption

**Captured by:** waseric; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Surfaced during the RSQP-1546 spec-execute dispatch run (a different repo — the ServiceNow probe work). The operator observed a ~173K session-context figure while the agent had no matching self-measured number and had continued orchestrating well past the 80k dispatch stop. Recommendations to preserve for a future iteration:

1. **Tooling (precise, per-turn cadence).** Add a Claude Code `UserPromptSubmit` hook that reads `transcript_path`, extracts the latest request's usage (`input_tokens + cache_read_input_tokens + cache_creation_input_tokens` ≈ current context size = the operator's UI number), and injects it as `additionalContext` (e.g. `CONTEXT_TOKENS≈173000`). Optionally a `PreToolUse`/`Stop` hook that hard-gates at the threshold. Caveat: a hook refreshes at *turn* cadence, so it misses mid-turn growth inside a single marathon turn (exactly this session's shape — four spikes + an amendment with no intervening user prompt).

2. **Skill wording.** In spec-execute Phase 8 (and the parallel budget notes in spec-orchestrate / spec-execute-task), replace "estimate from turn count and read volume" with "read the hook-injected `CONTEXT_TOKENS` value; if absent, fall back to dispatch-native accounting" — and delete the self-estimation clause, which invites the observed failure.

3. **Dispatch-native + excursion accounting (in-turn, agent-summable).** Redefine the dispatch budget as `Σ subagent_tokens across accepted receipts + a flat per-receipt orchestrator overhead`, plus a first-class **excursion** concept: operator-directed uncapped subagent reports (spikes) are budget events that should end the batch soon after, because their full output enters context uncapped. This catches the mid-turn growth the turn-cadence hook misses.

**Who else might know / related surfaces:** the budget mechanism lives in spec-execute Phase 8 and is echoed in spec-orchestrate and spec-execute-task; the ScheduleWakeup/Workflow tooling already exposes a real `budget.spent()` meter (precedent that the right signal exists, just not in the main loop).

## 2026-07-09 — Triaged: methodology/important; skip-investigation → route to spec-amend (paired with a harness-hook task)

**Triaged by:** waseric; methodologist; persona-frame: triage
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** reliably — structural divergence between the harness context size and the agent's self-assessment on every dispatch run; demonstrated this session at ~173K vs an 80k gate the agent never registered.
**Domain/severity changes from intake:** none — intake domain (methodology) confirmed; severity set to `important` (first confirmation; intake left it in placeholder form).
**Skip-investigation decision (if any):** skip investigation — the remedy was already characterized at intake and the "cause" is a known design omission, not a bug needing archaeology. Proceed directly to route.
**Pointer revalidation:** not-applicable — no external pointers on this finding.
**Notes:** Severity rationale — the gate silently fails to fire (a real correctness gap in the skill's asserted guarantee) but degrades gracefully (operator still sees the number and can intervene; no data loss) and has a clean workaround → `important`, not `blocker` or `advisory`.

## 2026-07-09 — Routed: spec-amend the spec-* budget model (+ paired harness-hook task)

**Decided by:** waseric; methodologist; persona-frame: triage; and operator (Eric Wasgatt)
**Prior status:** `triaged`
**New status:** `routed`
**Route subtype:** spec-amend
**Target spec (if amend or new-spec):** specs/20260518-spec-execute-skill/architecture.md (primary); companions specs/20260705-dispatch-execution/architecture.md and (if it carries the budget language) specs/20260707-context-working-set/architecture.md, under one amendment ID.
**Watch condition (if defer):** n/a
**Rationale:** See finding.md Route rationale. Paired, non-collapsible action: (a) spec-amend the Phase-8 wording (drop "estimate from turn count / read volume"; consume a hook-injected `CONTEXT_TOKENS`) + the dispatch budget model (add the "excursion" concept + Σ-subagent_tokens accounting); (b) a separate Claude Code `UserPromptSubmit` context-size hook (settings.json + script) — harness tooling, outside spec prose, tracked as its own task. The hook and the wording fix are mutually dependent: delivering one without the other leaves the budget unenforceable.
