# Review verifies by inspection rather than refutation, and cannot record what it tried — Finding

> Status: triaged
> Domain: security
> Severity: important           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-17
> Last transition: 2026-08-17                        ← scan-aid: most recent status change without traversing journal

## Intake

**Reported by:** self (operator, during a repository reconciliation review)
**Reported via:** text
**Captured by:** waseric; persona-frame: intake
**Summary:** `spec-review` establishes an independent, floor-sized, cold-context reviewer that reads the contract first and re-derives claims rather than trusting narrative. What it does not establish is an *adversarial* posture, and four gaps follow. (1) **The security checklist is inspection-shaped, not attack-shaped.** [spec-review/SKILL.md:106](../../../.agents/skills/spec-review/SKILL.md#L106) reads in full: "**Security.** Input validation present where required, secrets not logged, authorization checks at declared boundaries." Each clause asks the reviewer to confirm a control is *present*; none asks the reviewer to construct the input that defeats it. The concrete attack classes that distinguish a real security review — prefix and namespace bypasses via quotes, `;`, newlines, or `../`; secrets reaching `argv`; whether a declared injection corpus is actually exhaustive rather than illustrative; crypto reviewed against published test vectors with no custom math — appear nowhere in the masters. (2) **Gate/judgment false-pass has no named check.** A call that "didn't throw" being read as success, while the artifact it was supposed to produce silently went nowhere, is a distinct and recurring defect class with no coverage in the NFR walk. (3) **A clean review is not auditable.** The reviewer emits findings but has nowhere to record attacks attempted and repelled, so a thorough pass and a shallow pass produce identical artifacts — the absence of findings is unfalsifiable. (4) **There is no way to decline.** The four review outcomes each assert something about the work; `blocked` means the work needs an external decision, not that the reviewer's confidence was insufficient at its tier. Under `REVIEW_EXECUTION: dispatch` a cold reviewer is more likely to hit exactly that state, not less. Related, smaller: nothing tells a reviewer what to do when a checkpoint declares no exit criteria — the natural reading is a vacuous pass, where the correct outcome is a finding that the claim is unverifiable. Related, in the worker: [spec-worker.md:40](../../../.agents/agents/spec-worker.md#L40) says "Write the tests the task requires", with no instruction that security-relevant tasks require tests that attempt the bypass rather than confirm the happy path.
**External references:** none

## Triage

**Triaged by:** waseric; persona-frame: developer
**Triage date:** 2026-08-17
**Reproducibility:** reliably (structural — absences in the masters, verifiable by inspection)
**Repro steps (if reproducible):**
1. Read `.agents/skills/spec-review/SKILL.md:106` — the entire security NFR bullet. Note every clause is a presence check.
2. `grep -rin 'adversarial\|refute\|attack\|bypass\|escape' .agents/skills/spec-review/SKILL.md .agents/agents/spec-reviewer.md` — no attack-construction guidance.
3. Read the four outcome values in `spec-review` Phase 7 — none expresses reviewer non-confidence.
4. Read `.agents/agents/spec-worker.md:40` — no security-test distinction.
**Scope:** Every Review Checkpoint, in both interactive and dispatch review modes. Highest impact on checkpoints guarding security-relevant or low-verifiability work, which is precisely where `spec-write` is expected to set the top reviewer floor.
**Domain confirmation:** security
**Severity confirmation:** important
**Triage notes:** Domain recorded as `security` rather than `methodology` because the failure mode is a security review that passes work it should have failed; the *mechanism* is methodological but the exposure is not. Worth stating plainly what is **not** wrong here: the structural half of review is sound and in places stronger than any alternative considered — independence from the producing session, contract-before-code ordering, the blocker/important/advisory line, drift as a first-class finding, and "never trust a narrative claim when you can re-derive it" are all present and enforced. The gap is epistemic: the reviewer verifies claims rather than attempting to break them, and has no place to record the attempt. Item (1) is the highest-value remedy and the one with the strongest supporting evidence — the attack classes named above are not speculative, they are drawn from clauses that caught real defects in prior calibration work, including a gate false-pass of exactly the shape described in item (2). Investigation skipped: these are absences in shipped text, and the remedy is prose in existing masters rather than a new mechanism or a new skill.

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
