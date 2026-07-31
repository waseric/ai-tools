# Context budget gates on a token count the agent can't self-measure — Finding

> Status: routed
> Domain: methodology
> Severity: important           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-07-09
> Last transition: 2026-07-09                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self (operator, during a spec-execute dispatch run)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** spec-execute's Phase-8 "context budget" gates the run at "80,000 tokens of session context consumed," read from "the harness's context indicator, or conservatively estimated from turn count and read volume." Both measurement paths fail from the agent's side: the agent has no reliable in-band signal for its own running context size (the ~173K the operator's UI showed this session was never surfaced to the model), and the "estimate from turn count/read volume" fallback asks the agent to do exactly the estimation it is demonstrably poor at — this session ran well past the 80k dispatch stop without the agent perceiving it, until the operator stated the real number. The threshold is sound; its *measurement* is unenforceable by the agent. A related second defect: the dispatch budget model assumes every dispatched unit returns a length-capped (~25-line) receipt (~2k orchestrator growth each), but operator-directed **excursions** — here, two opus investigation spikes returning uncapped multi-thousand-token reports (~100k and ~138k subagent_tokens) — dump their full output into orchestrator context and are what actually drove the session to 173K; the skill has no "excursion" concept. What the agent *can* measure in-band: subagent `<usage>` blocks (subagent_tokens/tool_uses/duration per dispatched agent), its own tool-call/read volume (coarse), and — only inside a Workflow script — `budget.spent()`. Recommended remedies (tooling hook + skill-wording + dispatch-native/excursion accounting) are recorded in journal.md for a future iteration.
**External references:** none

## Triage

**Triaged by:** waseric; methodologist; persona-frame: triage
**Triage date:** 2026-07-09
**Reproducibility:** reliably (structural — the divergence holds on every dispatch run)
**Repro steps (if reproducible):**
1. Run `spec-execute` (or `spec-orchestrate`) in dispatch mode on any multi-task spec.
2. Take an operator-directed excursion — dispatch a subagent whose report is not length-capped (e.g. an investigation spike) so its full output returns into the orchestrator's context.
3. Continue orchestrating; observe the agent does not register a Phase-8 budget breach, because it has no in-band running-context meter to read.
4. Compare the harness UI's context figure (this session: ~173K) against the agent's self-assessment — they diverge, and the 80k gate never fires from the agent's side.
**Scope:** the spec-* skill suite's context-budget mechanism — spec-execute Phase 8, echoed in spec-orchestrate and spec-execute-task. Affects any dispatch/orchestrate run, most acutely those taking operator-directed investigation excursions. Authoring/skill-maintenance concern, not a shipped-app runtime bug.
**Domain confirmation:** methodology
**Severity confirmation:** important
**Triage notes:** Skip-investigation chosen — the remedy is already well-characterized (see the Intake journal entry), so no code archaeology is needed; the "cause" is a known design-level omission (the budget rule assumes a context meter the agent lacks, and assumes every dispatched unit returns a length-capped receipt), recorded here as understood rather than deferred to an investigation pass. The route is a **paired action that must not be collapsed**: (a) spec-amend the skill-wording/budget model, and (b) a separate Claude Code harness-tooling task for the context-size hook (settings.json + script) — the hook is outside spec/skill prose and cannot be delivered by a spec-amend alone. No external pointers to revalidate.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** spec-amend
**Decided by:** methodologist; persona-frame: triage; and operator (Eric Wasgatt)
**Route date:** 2026-07-09
**Target spec:** specs/20260518-spec-execute-skill/architecture.md (primary — Phase 8 context-budget rule). Companions under the same amendment ID: specs/20260705-dispatch-execution/architecture.md (dispatch receipt-budget model + the missing "excursion" concept) and, if it carries the budget language, specs/20260707-context-working-set/architecture.md. Plus a SEPARATE, non-spec tooling task: a Claude Code `UserPromptSubmit` context-size hook (settings.json + script) — tracked alongside, not folded into, the amendment.
**Route rationale:** The defect is a design-level omission with an already-characterized remedy, so investigation (cause archaeology) adds nothing — skip straight to amendment. spec-amend is right over spec-write (this refines existing skill specs, not a new capability) and over defer (the operator explicitly wants it on a future iteration). The amendment is inherently multi-target (the spec-execute skill spec + the dispatch-execution spec, per spec-amend's cross-skill case under one amendment ID), and it carries a hard dependency the route must keep visible: recommendation (1), the context-size hook, is a harness-tooling change outside any spec's prose — so this is a **paired action** (spec-amend the wording + budget model; plus a standalone tooling task for the hook). Do not collapse the two: the wording fix without the hook still leaves the agent without a reliable signal, and the hook without the wording fix leaves the skill telling the agent to self-estimate.
