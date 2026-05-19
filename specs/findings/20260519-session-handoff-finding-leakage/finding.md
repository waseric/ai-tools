# session-handoff finding leakage — Finding

> Status: triaged
> Domain: methodology
> Severity: important
> Date opened: 2026-05-19
> Last transition: 2026-05-19

## Intake

**Reported by:** self
**Reported via:** text
**Captured by:** Eric Wasgatt; persona-frame: intake
**Summary:** Findings mentioned in spec-execute journals (e.g., "finding to be filed against X") are not formally filed before session boundaries. The journal captures the signal correctly, but without formal intake the findings risk being lost — they exist only as prose in a journal entry, not as trackable pipeline artifacts. In the poc-reference-integration spec, 5 findings were mentioned across journal entries (T-03, T-05, OQ-1, Amendment 2026-05-19-3, T-10 blocked) but none were filed until a spec-review integrity pass caught them. The spec-execute skill's Phase 6 (task closeout) and Phase 8 (session boundary) do not include a step to scan the journal for unfiled finding references and batch-intake them. This is an orchestration gap: the skill trusts the operator to file findings mentioned in the journal, but the operator is context-switching between tasks and sessions, and the findings fall through.
**External references:** —

## Triage

**Triaged by:** Eric Wasgatt; methodologist; persona-frame: triage
**Triage date:** 2026-05-19
**Reproducibility:** reliably
**Repro steps (if reproducible):**
1. Run /spec-execute against a spec with multiple tasks
2. During task execution, encounter an issue that warrants a finding
3. Journal the finding reference in the task closeout
4. End the session
5. Observe: the finding exists only as journal prose, not as a pipeline artifact
**Scope:** Observed on /spec-execute so far. Currently reliant on /spec-review to detect unfiled findings. Broader gap: any pipeline phase that encounters an out-of-scope problem should be creating a finding to offload the concept without distraction.
**Domain confirmation:** methodology
**Severity confirmation:** important
**Triage notes:** none

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
**Target spec:** <path to spec>
**Route rationale:** <one paragraph>
