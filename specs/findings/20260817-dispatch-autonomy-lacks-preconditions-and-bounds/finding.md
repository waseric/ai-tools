# Dispatch autonomy lacks entry preconditions, retry bounds, and floor-confidence signals — Finding

> Status: triaged
> Domain: methodology
> Severity: important           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-17
> Last transition: 2026-08-17                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self (operator, during a repository reconciliation review)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** `spec-execute` now defaults to `AUTONOMY: checkpoint` and `EXECUTION: dispatch` — an unattended batch that runs between operator stops. Four guardrails that autonomy makes load-bearing are absent from the current masters. (1) **No entry precondition on spec approval.** Phase 1 orients on task status, drift, and open questions but never reads the spec's status banner; `spec-execute/SKILL.md` contains no occurrence of "banner", "Approved", or "Draft". An unattended batch can therefore execute a full task breakdown against a spec still in Draft, which is the highest-leverage waste the mode can produce. (2) **Re-dispatch is unbounded.** Three sites instruct "re-dispatch it with an updated brief, or stop to the operator" ([spec-execute/SKILL.md:85](../../../.agents/skills/spec-execute/SKILL.md#L85), [:260](../../../.agents/skills/spec-execute/SKILL.md#L260), [:290](../../../.agents/skills/spec-execute/SKILL.md#L290)) with no attempt counter, so a task that fails its derivation re-check the same way each time can cycle indefinitely under default autonomy. (3) **The receipt carries no confidence signal.** The 8-field, 25-line receipt ([receipt-schema.md](../../../.agents/skills/spec-execute/receipt-schema.md)) expresses `STATUS: done | partial | blocked` but has no way for a worker to report that it completed the task while doubting its own result — the orchestrator sees a binary it cannot weight. (4) **Worker floor self-reporting is discouraged by framing.** [spec-worker.md:78](../../../.agents/agents/spec-worker.md#L78) provides the `blocked` path for a model-floor conflict, but [:13](../../../.agents/agents/spec-worker.md#L13) opens with "Do not second-guess or try to change it", which cuts against a worker volunteering that a task feels above its tier. A model below its floor is precisely the model least able to detect that it is. Related: [MODEL_FLOOR_POLICY](../../../.agents/skills/spec-write/SKILL.md#L31) asks the project for a "model-tier ladder and selection rationale" but supplies no method for establishing one, so the ladder each project declares is unfalsifiable.
**External references:** none

## Triage

**Triaged by:** waseric; persona-frame: developer
**Triage date:** 2026-08-17
**Reproducibility:** reliably (structural — all four are absences in the masters, verifiable by inspection)
**Repro steps (if reproducible):**
1. `grep -in 'banner\|Approved\|Draft' .agents/skills/spec-execute/SKILL.md` → no matches. Phase 1 has no approval precondition.
2. `grep -in 're-dispatch\|retry\|attempt' .agents/skills/spec-execute/SKILL.md` → three re-dispatch instructions, no attempt bound.
3. Read `.agents/skills/spec-execute/receipt-schema.md` "Shape" → 8 fields, none expressing confidence.
4. Read `.agents/agents/spec-worker.md:13` against `:78` → the stop path exists; the framing discourages using it.
**Scope:** Every dispatch-mode run of `spec-execute`, i.e. the default execution path. Items (1) and (2) affect any autonomous batch; (3) and (4) affect the orchestrator's ability to weight receipts and the worker's willingness to self-report floor mismatch.
**Domain confirmation:** methodology
**Severity confirmation:** important
**Triage notes:** These are four distinct absences sharing one root: the dispatch design's default flipped from per-task human approval to autonomous-between-stops, and the guardrails that a human-in-the-loop was implicitly providing were not all re-established mechanically. Item (1) is the cheapest and highest-value — a single sentence in Phase 1. Item (2) is a one-clause bound. Item (3) is one additional receipt field within the existing 25-line cap. Item (4) is a framing adjustment, not a new mechanism. The `MODEL_FLOOR_POLICY` calibration gap is grouped here rather than filed separately because an uncalibrated ladder makes every floor declaration — and therefore items (3) and (4) — unverifiable. A candidate calibration method: hand a candidate model a known-hard case with a planted defect (e.g. "does this spec section fairly represent its cited source?") and promote the floor above that tier if the model does not independently catch it. Not investigated further; the remedy for all five is wording in existing masters, so investigation was skipped in favour of routing.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** <spec-amend | spec-write | defer | close>
**Decided by:** <persona-frame of the deciding phase, and operator>
**Route date:** <YYYY-MM-DD>
**Target spec:** <path to spec, when route is `spec-amend` or `spec-write`; e.g., specs/20260517-findings-pipeline/architecture.md>
**Route rationale:** <one paragraph; why this route over the others. For `defer`, include watch condition: what would cause re-evaluation.>
