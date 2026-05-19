---
name: spec-review
lastUpdated: 2026-05-15
description: Review a body of work against a specific Review Checkpoint declared in a feature spec or design spec. Reads the checkpoint contract first (review focus + exit criteria), then walks the diff (or the artifact, for design-spec checkpoints) producing structured findings tagged blocker/important/advisory, distinguishing spec-compliance failures from preferences. Produces a fixed-format verdict (pass / pass with comments / changes requested / blocked) and writes the outcome back to the spec and journal. Use whenever a spec Review Checkpoint has been triggered, or the user wants a structured review against an existing spec. Pairs with `spec-design` and `spec-write` (which declare checkpoints), `spec-execute` (which produces the work being reviewed), and `spec-amend` (which applies spec changes when the review surfaces them).
---

# Spec Review

Use when a Review Checkpoint defined in a spec has been triggered. Pairs with `spec-design` and `spec-write` (which declare checkpoints), `spec-execute` (which produces the work being reviewed), and `spec-amend` (which applies spec changes when the review surfaces them). Walks the diff against the checkpoint's declared review focus and exit criteria, produces a structured verdict, and records the outcome back into the spec and journal.

## How this skill works

When invoked, you act as the reviewer. Gather the INPUTS below from the user — infer what you can from the working directory and recent conversation, ask explicitly only for what is missing or ambiguous. Then read the spec checkpoint first, then the journal, then the diff. Produce findings in the structured form below. Write the verdict back to the spec and journal so the next session knows whether to proceed.

## INPUTS

```
SPEC_PATH: <repo-relative path, e.g. specs/YYYYMMDD-feature-x/feature.md>
JOURNAL_PATH: <repo-relative path, e.g. specs/YYYYMMDD-feature-x/journal.md>
CHECKPOINT_ID: <e.g. CP-1, or the name from the spec's Review Checkpoints section>
DIFF_RANGE: <e.g. main..feature/x, or a list of commit SHAs, or a PR URL>
TASK_IDS_IN_SCOPE: <task IDs covered by this checkpoint, from the spec>
REVIEWER: <name or role>
SPEC_REPO_ROOT: <optional; path to the repo where SPEC_PATH and JOURNAL_PATH live, when different from the codebase under review>
```

---

## ROLE

You are a code reviewer evaluating whether a body of work satisfies a specific Review Checkpoint declared in a specification. The spec is the contract. Your job is to verify that the implementation matches what the spec said it would be, that the checkpoint's exit criteria are met, and that the review focus areas declared in the spec have been addressed with evidence.

You distinguish between **spec compliance findings** (objective; block merge until resolved) and **advisory findings** (subjective or out of spec scope; surfaced as comments, not blockers). You do not invent new requirements that the spec did not declare. If you believe the spec is wrong, you propose an amendment under the Amendment Protocol rather than blocking on something the spec did not require.

## OPERATING PRINCIPLES

1. **Spec first, code second.** Read the checkpoint definition before reading any code. The spec tells you what to look for.
2. **Evidence per finding.** Every finding cites a file path, line range, spec section, or test name. No vague verdicts.
3. **Compliance vs. advisory.** A finding is `[blocker]` only if it violates the spec, the Definition of Done, or the checkpoint's exit criteria. Style preferences, refactoring ideas, and "I would have done it differently" are `[advisory]`. The middle tag `[important]` denotes a quality concern that is not a spec violation but is serious enough to warrant attention before the next task — surfaced as a comment, does not block the verdict, and exists to distinguish "you should fix this before moving on" from "I'd have done it differently."
4. **Drift is a first-class finding.** If the implementation does not match the spec, that is a finding regardless of whether the implementation is otherwise good. Either the code is wrong, or the spec needs an amendment. Both must be resolved.
5. **No new requirements.** If you want something the spec did not require, propose a spec amendment for the next iteration. Do not block this checkpoint on it.
6. **The verdict is recorded.** The review outcome is written back to the spec and the journal. Future sessions need to know whether the checkpoint passed.

## PHASE 1 — ORIENT

Read, in order:

1. The **Review Checkpoints** section of `SPEC_PATH`. Find the entry matching `CHECKPOINT_ID`. Note its **trigger**, **review focus**, and **exit criteria** verbatim. These are the contract for this review.
2. The **Task Breakdown** entries for each task in `TASK_IDS_IN_SCOPE`. Note each task's scope, acceptance criteria, and Definition of Done.
3. The **Non-functional Requirements** section. Note the items relevant to the tasks under review (security, observability, performance, backward compatibility, configuration).
4. The journal entries in `JOURNAL_PATH` for the tasks under review. Note any spec amendments, decisions made, surprises, or partial-completion flags.
5. The diff in `DIFF_RANGE`. Skim for shape and scope before reading in detail.

Then output an **Orientation Report**:

- **Checkpoint contract.** Quote the review focus and exit criteria from the spec. Quoting is required so subsequent phases can be checked against the same text.
- **Tasks in scope.** List with status from the journal.
- **Diff shape.** Files added / modified / deleted, line counts, test files present.
- **Journal flags.** Any partial-completion, spec amendments, or surprises noted by the implementer.
- **Initial drift signals.** Any obvious mismatches between the diff and the declared task scope.

## PHASE 2 — SCOPE VERIFICATION

Verify that the diff stayed within what the spec authorized:

- For each task in scope, list the files declared in the task's Scope field. Compare against the files actually touched in the diff.
    - Files touched and declared: `[ok]`.
    - Files declared but not touched: `[review]` — was this task fully implemented?
    - Files touched but not declared: `[blocker unless amended]` — either out of scope, or the spec was amended in the journal. Cite the amendment if present.
- Verify that no new dependencies, frameworks, or top-level abstractions were introduced unless the spec explicitly authorized them.
- Verify that the tasks' declared tests are present in the diff. Missing test files are `[blocker]`.

Findings in this phase are usually `[blocker]` because they indicate the implementation went somewhere the spec did not sanction. The remedy is either to revert the out-of-scope work or to retroactively amend the spec under the Amendment Protocol with explicit user approval.

## PHASE 3 — REVIEW FOCUS WALK

For each item in the checkpoint's **review focus** (quoted from Phase 1), walk the diff and produce findings.

For each focus item:

- **Focus item.** Quoted from the spec.
- **What was reviewed.** Files and line ranges examined.
- **Findings.** Itemized. Each finding is tagged `[blocker]`, `[important]`, `[advisory]`, or `[ok]`. Each cites a file path and line range, and references the spec section or principle it relates to.
- **Verdict on this focus item.** `pass`, `pass with comments`, or `fail`.

The review focus is the most important section of the review. It is what the spec author identified as worth deep attention for this checkpoint. Do not shortcut it.

## PHASE 4 — EXIT CRITERIA VERIFICATION

Walk the checkpoint's **exit criteria** one item at a time. For each:

- **Criterion.** Quoted from the spec.
- **Evidence.** File path, test output, CI status, doc location, log line, metric name. Whatever proves the criterion is met.
- **Verdict.** `met` or `not met`.

If any criterion is `not met`, the checkpoint fails regardless of other findings. Exit criteria are non-negotiable; that is what makes them exit criteria.

## PHASE 5 — GENERIC QUALITY PASS

A lighter-weight pass over items that the spec's Non-functional Requirements section already declared. For each NFR category relevant to the tasks in scope, verify:

- **Security.** Input validation present where required, secrets not logged, authorization checks at declared boundaries.
- **Observability.** Logs, metrics, traces match the names and conventions declared in the spec. Log levels are appropriate. Sensitive fields are not logged.
- **Performance.** No obvious regressions against declared budgets. Loops over unbounded inputs flagged. N+1 queries flagged.
- **Reliability.** Timeouts, retries, idempotency keys present where the spec required them. Error handling is consistent with codebase conventions.
- **Backward compatibility.** Public APIs, schemas, and config defaults unchanged unless the spec authorized changes.
- **Configuration.** New env vars or feature flags documented and defaulted as the spec specified.

Findings here are typically `[important]` or `[advisory]`. Promote to `[blocker]` only when an NFR was declared in the spec and is clearly violated.

## PHASE 6 — TEST AND DOCUMENTATION REVIEW

- **Test coverage.** Verify that every acceptance criterion from each task in scope has at least one test. Cite the test by name. Missing coverage on a declared acceptance criterion is `[blocker]`.
- **Test quality.** Tests assert behavior, not implementation. Tests are independent and deterministic. Fixtures match the spec's declared test data approach.
- **Documentation.** Inline doc comments are present on new public interfaces. README, API docs, runbooks, or operator-facing docs are updated as the spec required. Missing docs declared in the spec are `[blocker]`; nice-to-have docs are `[advisory]`.

## PHASE 7 — VERDICT

Produce a structured verdict in this exact format:

```
## Checkpoint <CHECKPOINT_ID> Review Verdict

**Reviewer:** <name>
**Date:** <YYYY-MM-DD>
**Diff range:** <as input>
**Tasks reviewed:** <task IDs>

**Outcome:** pass | pass with comments | changes requested | blocked

**Blockers:** <count>
**Important:** <count>
**Advisory:** <count>

### Blocker findings
- <one line per finding, with file:line and spec reference>

### Important findings
- <one line per finding, with file:line and spec reference>

### Advisory findings
- <one line per finding>

### Spec amendments proposed
- <if any, with link to diff and rationale>

### Exit criteria status
- <criterion>: met | not met (evidence)

### Recommendation
<one paragraph: what must happen for this checkpoint to pass, or confirmation that it has passed and work may proceed to the next task>
```

Outcome rubric:

- **pass.** Zero blockers, all exit criteria met. Work may proceed.
- **pass with comments.** Zero blockers, all exit criteria met, but advisory findings exist. Work may proceed; advisories logged for future sessions or follow-up tasks.
- **changes requested.** One or more blockers, but they are addressable within the current task scope. Implementer fixes and the checkpoint is re-reviewed.
- **blocked.** A blocker requires a spec amendment, design rework, or external decision before work can continue. Stops further task pickup until resolved.

## PHASE 8 — UPDATE ARTIFACTS

Regardless of outcome, record the review:

- **Update the spec.** In the Review Checkpoints section, add a `Status` line under the relevant checkpoint entry: `Status: <outcome> on <date> by <reviewer>`. If `pass` or `pass with comments`, the checkpoint is closed. If `changes requested` or `blocked`, the checkpoint stays open.
- **Append a journal entry.** Format:

```
  ## <YYYY-MM-DD> — Review of <CHECKPOINT_ID>

  **Reviewer:** <name>
  **Outcome:** <outcome>
  **Tasks reviewed:** <task IDs>
  **Blockers:** <count, with one-line summaries>
  **Spec amendments proposed:** <if any>
  **Next action:** <what the implementer or user should do next>
```

**Multi-repo case.** When `SPEC_REPO_ROOT` is set, spec and journal updates are committed in `SPEC_REPO_ROOT`, not in the codebase repo. The commit message references the checkpoint ID. This is the paired commit to whatever code-side work was reviewed — do not let one ship without the other.

- **If spec amendments were proposed,** route them through the `spec-amend` skill. Amendments are not applied silently as part of a review.
- **If the outcome is `pass` or `pass with comments`,** state the next task ID per the dependency graph so the next session has a clear pickup point.

## WHAT NOT TO DO

- Do not begin reviewing the diff before reading the checkpoint definition. The spec tells you what to focus on; reading code first biases the review toward whatever happens to be salient in the diff.
- Do not approve without citing evidence per exit criterion. "Looks good" is not a verdict.
- Do not invent new requirements. If something is missing that the spec did not require, raise it as an advisory or propose a spec amendment for next iteration; do not block on it.
- Do not conflate `[blocker]` with `[advisory]`. Blockers are spec or DoD violations. Advisories are preferences. Mixing them creates review fatigue and trains implementers to ignore comments.
- Do not rubber-stamp based on the journal entry alone. The journal is the implementer's claim; the diff is the evidence. Verify against the diff.
- Do not silently accept out-of-scope file changes. Either confirm a spec amendment is recorded or block until one is.
- Do not skip the exit criteria walk. A checkpoint with unmet exit criteria fails regardless of how good the rest of the work looks.
- Do not produce a verdict without the structured format. Future sessions and reviewers depend on the predictable shape.

## REVIEWER NOTES

- For human reviewers: the prompt structure is a checklist. You can fill it in conversationally in a PR description or review tool, as long as every section is addressed.
- For agent reviewers: produce the report as a single markdown document and append it to the journal. Do not skip phases even when a section appears trivially satisfied; the absence of findings in a phase is itself a recorded outcome.
- For self-review (reviewer is also the implementer): run the full prompt anyway. Self-review with a structured checklist catches more than self-review without one. Be especially honest about advisory findings; they are easier to dismiss when reviewing your own work.

---

Design notes on what makes this different from a generic code review checklist:

**Spec is read before code.** Phase 1 forces the reviewer to quote the checkpoint contract verbatim before looking at the diff. This stops the most common review failure: reviewing whatever happens to be salient in the diff rather than what the spec said to focus on. Especially important for self-review, where you already know what you wrote and will defend it instead of test it.

**Blocker vs. advisory is a hard distinction.** A finding is a blocker only if it violates the spec, the DoD, or an exit criterion. Everything else is advisory. This stops review fatigue and prevents the spec from being silently expanded mid-implementation through reviewer preferences. It also matches how good engineering teams operate: the contract is the contract; opinions are opinions.

**Drift is a first-class finding.** If the code and spec disagree, the review records that as a finding regardless of which side is "right." Resolution goes through the `spec-amend` skill, not through silent acceptance. This is what keeps the spec accurate over time.

**No new requirements during review.** If the reviewer wants something the spec didn't require, the channel is "propose a spec amendment for next iteration," not "block this checkpoint." This is the discipline that lets specs survive contact with multiple reviewers without becoming unbounded.

**Verdict is structured.** The Phase 7 format is fixed because future sessions and agents need to find the outcome programmatically. "Pass with comments" and "changes requested" are deliberately distinct: the first lets work proceed, the second stops it.

**Self-review case is called out.** You'll often be the only reviewer (Sandlot work, side projects, contracting setups where there's no peer reviewer). Phase 8's structure plus the explicit "be especially honest about advisory findings" note in Reviewer Notes addresses the natural bias to wave through your own work.

**How the loop closes.** Author → execute → review → next task. Every artifact (spec, journal, review verdict) is durable and re-readable, so a session can drop in at any phase without losing context. This is what makes the trilogy actually spec-driven rather than chat-driven: the chat is ephemeral; the spec, journal, and verdicts are the working memory of the project.
