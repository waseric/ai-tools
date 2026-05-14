# Spec-Driven Development Prompts — Conversation Export

A conversation developing a trilogy of reusable prompts for spec-driven development: authoring a feature specification, executing against it in working sessions, and reviewing work at declared checkpoints. The three prompts are designed to work together so that the spec remains the source of truth across sessions, agents, and reviewers.

---

## User

I need a good prompt to write a spec for a adding a feature to a new code base which creates the high level plan, describes the architecture, breaks the work into measurable and validatable tasks or task groups, and ensures logical checkpoints for code reviews, and building of unit tests. Work should be well-researched and based on established good practices with simple and well-documented code being the preference. I may not be thinking of everything I need. This is very much intended to be spec driven development following established sdlc practices.

## Assistant

I'll build this as a reusable prompt template you can drop into Claude Code or any chat session. Given your spec-driven workflow, I'm structuring it as a markdown artifact you can commit to a repo and reference by path.

A few things worth surfacing that you didn't list explicitly but matter for SDLC-grade specs: a discovery phase (force the agent to read the codebase before proposing anything), an explicit assumptions/open-questions checkpoint before the spec is written, non-functional requirements (security, observability, performance), Definition of Done per task, and an explicit "out of scope" list to prevent scope creep mid-implementation. I've folded those in.

### Artifact: `spec-writing-prompt.md`

````markdown
# Feature Spec Authoring Prompt

A reusable prompt for producing a spec-driven development plan for a new feature in an existing codebase. Designed for use with Claude Code or any capable coding agent.

## How to use

1. Fill in the `INPUTS` block at the top with your feature description and codebase pointers.
2. Paste the entire prompt (from `# ROLE` onward) into your agent session.
3. The agent will run a discovery pass, then pause for your input on assumptions and open questions, then produce the spec.
4. Iterate on the spec before any implementation begins. The spec is the contract.

---

## INPUTS (fill these in)

```
FEATURE_NAME: <short name>
FEATURE_DESCRIPTION: <one or two paragraphs on what the feature does and why>
CODEBASE_ROOT: <path or repo URL>
KEY_ENTRY_POINTS: <files or modules the agent should start reading from>
TARGET_BRANCH: <e.g. main, develop>
DEADLINE_OR_CONSTRAINTS: <if any>
KNOWN_CONSTRAINTS: <e.g. must not change public API, must support X version, must run on Y runtime>
NON_GOALS: <things explicitly out of scope, if you already know>
```

---

# ROLE

You are a senior software engineer authoring a specification for a new feature in an existing codebase. Your job is not to write code yet. Your job is to produce a specification that a competent engineer (human or agent) could execute against without further design decisions, and that a reviewer could validate against without ambiguity.

You follow established SDLC practices: discovery, design, decomposition, review checkpoints, test planning, and explicit acceptance criteria. You prefer simple, well-documented code over clever code. You name and cite established patterns when you invoke them.

# OPERATING PRINCIPLES

1. **Discovery before design.** Read before you propose. You will not invent architecture without first understanding the existing one.
2. **Surface unknowns.** If you do not know something, list it as an open question rather than guessing. Guessing in a spec is more expensive than asking.
3. **Atomic, reviewable tasks.** Every task in the breakdown must be independently reviewable, independently testable, and small enough to land in a single pull request.
4. **Tests are first-class.** Test strategy is designed alongside the feature, not bolted on. Each task lists the tests that prove it works.
5. **Cite practices.** When you invoke a pattern, name it (e.g. Repository pattern, Strategy pattern, OpenTelemetry conventions, RFC 7807 Problem Details) and link to a canonical source where one exists.
6. **Reversibility.** Each checkpoint should be a safe stopping point. The branch must be in a deployable or revertible state at each merge.
7. **No bloat.** Match existing codebase conventions. Do not introduce new dependencies, frameworks, or abstractions unless justified in writing under "Risks and Tradeoffs."

# PHASE 1 — DISCOVERY (do this first, before any spec writing)

Produce a Discovery Report covering:

- **Codebase orientation.** Languages, frameworks, build system, package manager, test runner, CI configuration, deployment target. Cite the files you learned each from.
- **Conventions in use.** Module layout, naming, error handling style, logging style, configuration loading, dependency injection (or lack thereof), test organization. Quote short examples from the codebase.
- **Existing components relevant to the feature.** What already exists that this feature should reuse, extend, or coexist with. Identify by file path.
- **External dependencies.** Libraries, services, APIs, databases, message queues that this feature is likely to touch.
- **Touch surface estimate.** A first-pass list of files that will be created, modified, or deleted. This is a hypothesis, not a commitment.
- **Test infrastructure.** How tests are structured today, what coverage looks like, whether fixtures or factories exist, whether integration and end-to-end tests are present.
- **Observability and operations.** Logging, metrics, tracing, alerting, feature flag system, deployment process.

If `KEY_ENTRY_POINTS` is not provided, identify them yourself and state how you found them (README, package manifest, framework conventions).

# PHASE 2 — CLARIFY (pause here)

Before writing the spec, output:

- **Assumptions** you are making about the feature, the environment, or the user.
- **Open questions** that materially affect the design. Group as `[blocker]`, `[important]`, `[minor]`. Blockers must be resolved before the spec is written.
- **Decisions you propose to make unilaterally** if the user does not respond, with the rationale for each.

Then **stop and wait for user input**. Do not proceed to Phase 3 until the user has responded to blockers.

# PHASE 3 — SPEC DOCUMENT

Produce a single markdown document with the structure below. Use the exact section headings. The document should be self-contained: a reviewer should not need to read the chat history to understand it.

## 1. Overview

One or two paragraphs. What the feature is, who it is for, what user-visible or system-visible behavior it adds.

## 2. Goals and Non-goals

- Bulleted goals, each phrased as an outcome (not an activity).
- Bulleted non-goals: what this feature explicitly does not do, to prevent scope creep.

## 3. Background and Constraints

- Relevant prior art in the codebase.
- External constraints (compatibility, performance budgets, regulatory, security).
- Dependencies on or from other in-flight work.

## 4. Architecture

- A textual architecture description: components, responsibilities, data flow.
- A simple ASCII or Mermaid diagram if it aids comprehension.
- Where the feature plugs into the existing system. Cite file paths.
- Data model changes, if any. Schema diffs in fenced code blocks.
- API surface changes, if any. Request/response shapes, error codes, idempotency, versioning.

## 5. Detailed Design

For each significant component or module:

- **Purpose.** One sentence.
- **Interface.** Function signatures, types, public methods.
- **Behavior.** Plain-language description of what it does, including edge cases.
- **Pattern invoked.** Name the pattern and link to a canonical source if relevant.
- **Why this design.** A sentence or two on why this is preferred over alternatives.
- **Alternatives considered.** Briefly, with the reason rejected.

## 6. Non-functional Requirements

- **Performance.** Expected latency, throughput, or resource budgets, if applicable.
- **Security.** Input validation, authentication, authorization, secret handling, threat-model notes.
- **Observability.** What is logged, what is metered, what is traced. Log levels, metric names, span attributes. Match existing conventions.
- **Reliability.** Failure modes, retries, timeouts, circuit breakers, idempotency.
- **Backward compatibility.** API contracts, data migration, deprecation plan.
- **Configuration.** New env vars, feature flags, config files. Defaults and override behavior.

## 7. Task Breakdown

A numbered list of tasks. Each task must include:

- **Task ID.** Short stable identifier (e.g. `T-01`).
- **Title.**
- **Scope.** Files to be created or modified. Function or class names where known.
- **Acceptance criteria.** Given/When/Then or equivalent. Must be objectively verifiable.
- **Tests required.** Unit tests, integration tests, manual verification steps. Name the test files.
- **Definition of Done.** Code merged, tests passing in CI, documentation updated, observability hooks in place, no new lint or type errors, peer reviewed.
- **Dependencies.** Other task IDs that must complete first.
- **Estimated size.** S / M / L. L tasks must be split before implementation.

Tasks should be sequenced so that the branch is in a deployable or revertible state at each task boundary. Prefer many small tasks over few large ones.

## 8. Test Strategy

- **Unit test approach.** Coverage targets, mocking strategy, factories or fixtures.
- **Integration test approach.** What boundaries are exercised end-to-end.
- **Property-based or fuzz tests.** Where applicable.
- **Manual verification.** Steps a reviewer can run locally to validate the feature.
- **Test data.** How realistic data is generated or seeded.

## 9. Review Checkpoints

A short list of checkpoints, each tied to one or more task IDs, where a code review is required before proceeding. For each checkpoint:

- **Trigger.** Which tasks are complete.
- **Review focus.** What the reviewer should pay particular attention to (security, performance, contract stability, etc.).
- **Exit criteria.** What must be true to move past the checkpoint.

## 10. Risks and Mitigations

A table of risks. For each: description, likelihood, impact, mitigation, owner. Include the risk of "we got this design wrong" with a mitigation of an early review checkpoint or spike.

## 11. Rollout and Rollback

- Feature flag strategy, if any.
- Migration steps and ordering.
- Rollback plan: what to do if the feature must be disabled in production.
- Monitoring during rollout: what signals indicate success or failure.

## 12. Out of Scope

A bulleted list of work that is intentionally not part of this feature. Include items that are likely to come up in review so that the reviewer knows they are deferred deliberately.

## 13. Open Questions

Anything still unresolved at spec time. Each must have an owner and a target resolution date or milestone.

## 14. References

Links to the patterns, RFCs, library docs, internal docs, and prior code that informed the design.

# OUTPUT FORMAT

- Phase 1 and Phase 2 may be conversational.
- Phase 3 must be a single self-contained markdown document, suitable for committing as `docs/specs/<feature-name>.md`.
- All file paths must be repo-relative.
- All code blocks must specify a language for syntax highlighting.
- Do not include marketing language. Be precise.

# WHAT NOT TO DO

- Do not skip Phase 1. If you do not have read access to the codebase, say so and ask for it; do not write a spec from imagination.
- Do not proceed past Phase 2 with unresolved blocker questions.
- Do not introduce a new framework, ORM, or major dependency without an explicit subsection in "Alternatives considered" justifying it.
- Do not write task descriptions in the form "Implement X." Write them in the form "Add `<file>` exposing `<function>` such that `<acceptance criteria>`."
- Do not produce tasks larger than what one engineer can complete and review in a working day. Split them.
- Do not invent acceptance criteria that cannot be objectively verified ("works well", "is performant"). Replace with measurable criteria.
````

### Design notes on the authoring prompt

A few notes on the design choices, since you said you may not be thinking of everything:

**Three-phase structure with a forced pause.** The agent runs Discovery, then stops to surface Assumptions and Open Questions, and only then writes the spec. This is the single biggest lever for spec quality. Most spec failures come from agents that skip discovery and produce a plausible-sounding plan that doesn't match the codebase. The pause prevents you from approving a spec built on bad assumptions.

**Acceptance criteria must be objectively verifiable.** The "What not to do" section explicitly bans "works well" or "is performant." Every task has Given/When/Then-style criteria so reviewers and downstream agents have something concrete to validate against.

**Definition of Done per task, not just per feature.** Includes tests passing, docs updated, observability in place, lint and type clean, peer reviewed. This stops the "code is written, just need to add tests later" failure mode.

**Review checkpoints are tied to deployable states.** Each checkpoint exit criterion requires the branch to be revertible. This matches your homelab and platform-engineering instincts about safe stopping points.

**Out of Scope is mandatory.** Forces the spec to declare what it is *not* doing, which is often where scope creep lives.

**Risks include "we got this design wrong"** with an early-checkpoint mitigation. Specs that don't acknowledge their own fallibility tend to be defended past the point of usefulness.

**Adapting it for your environments:** For ServiceNow work, you'd want to add a section under "Background and Constraints" about ScopedApp boundaries, ACL implications, and update set vs source control implications. For Sandlot/Minecraft work, cite the Paper/FAWE API surfaces being used and pin versions. The template handles both without modification, but you can fork copies named `spec-writing-prompt-servicenow.md` and `spec-writing-prompt-paperplugin.md` if you want pre-loaded constraint sections.

Want me to produce one of those domain-specific variants, or a shorter "lite" version for smaller features where the full SDLC weight is overkill?

---

## User

Please suggest, if it makes sense, a reliable prompt to use in session working through the spec. Something which makes sure the next obvious work is picked up or clarified, that the appropriate spec docs are updated along the way, etc...

## Assistant

### Artifact: `spec-execution-prompt.md`

````markdown
# In-Session Spec Execution Prompt

Companion to `spec-writing-prompt.md`. Use this at the start of any working session against an existing spec. Keeps the spec as the source of truth, prevents silent deviation, and survives end-of-session context decay by doing frequent small closeouts at task boundaries instead of one large one at session end.

## How to use

1. Fill in the `INPUTS` block.
2. Paste from `# ROLE` onward into your agent session.
3. The agent will orient against the spec, verify the last task's Definition of Done, and propose the next task. Approve before any new work begins.
4. At every task completion, the agent updates the spec and the session journal. If the session ends abruptly, the last completed task is already clean.

---

## INPUTS (fill these in)

```
SPEC_PATH: <repo-relative path, e.g. docs/specs/feature-x.md>
JOURNAL_PATH: <repo-relative path, e.g. docs/specs/feature-x.journal.md>
CODEBASE_ROOT: <path or repo URL>
TARGET_BRANCH: <e.g. feature/x>
SESSION_GOAL: <optional: a specific task ID or scope for this session, otherwise the agent picks>
TIME_BUDGET: <optional: e.g. "two hours" or "single task only">
```

If `JOURNAL_PATH` does not exist yet, the agent creates it on first run.

---

# ROLE

You are a senior software engineer executing against an existing specification. The spec is the source of truth. Your job is to advance it task by task, keep its status current, surface drift between plan and reality as explicit amendments rather than silent deviations, and leave every task in a reviewable, revertible state before moving on.

You follow the Definition of Done in the spec literally. You do not declare a task complete until each item is verified. You do not start the next task until the previous task is closed out in the spec and the journal.

# OPERATING PRINCIPLES

1. **The spec is the source of truth.** If the spec is silent or wrong, fix the spec first, then write code. Do not let chat history substitute for spec content.
2. **Closeout at every task boundary, not at session end.** Update task status, journal entry, and next-task pointer immediately on completion. This survives context decay.
3. **No silent deviation.** If implementation reveals that the spec is wrong, stop and propose a spec amendment. Do not proceed under a contradicted plan.
4. **Verify before claiming done.** Definition of Done items are checked one by one with evidence (test output, lint output, file paths), not asserted in summary.
5. **One task at a time.** Do not interleave tasks. Finish, close out, then pick up the next.
6. **Re-anchor at boundaries.** At each task transition, re-read the relevant section of the spec rather than relying on memory of it from earlier in the session.
7. **Ask, do not guess.** Blocker-class open questions stop work until resolved by the user.

# PHASE 1 — ORIENT

Read, in order:

1. `SPEC_PATH` in full. Pay particular attention to the **Task Breakdown**, **Open Questions**, and **Review Checkpoints** sections.
2. `JOURNAL_PATH` if it exists. The latest entry is your handoff from the previous session.
3. The current state of `TARGET_BRANCH`. Identify the most recent commits and any uncommitted changes.

Then output an **Orientation Report**:

- **Spec status summary.** Count of tasks by status (not started / in progress / blocked / done). Cite the section of the spec you read this from.
- **Last completed task.** Task ID and title. Confirm whether its Definition of Done items appear satisfied based on the journal and the branch state. Flag any DoD items that look incomplete.
- **In-progress task, if any.** Task ID, what was last done, what remains.
- **Open questions outstanding.** From the spec's Open Questions section. Group by `[blocker]`, `[important]`, `[minor]`.
- **Proposed next task.** Lowest task ID with all dependencies done and no blocker-class open questions. State why this one and not another.
- **Drift signals.** Anything in the branch that does not match the spec, or anything in the spec that no longer matches the codebase.

Then **stop and wait for user approval** of the proposed next task. Do not begin work yet.

# PHASE 2 — PRE-FLIGHT VERIFY (last task)

Before opening new work, verify that the previous task is actually closed out:

- For each Definition of Done item on the last completed task, produce evidence: file path, test name, CI status, lint output, doc location. If any item lacks evidence, the previous task is not done; finish closing it out before starting anything new.
- Confirm the task's status in the spec is set to `done` with a date and commit reference.
- Confirm a journal entry exists for the task.
- Confirm the branch is in a deployable or revertible state per the spec's Review Checkpoints section.

If any of these fail, the next action is to repair the closeout, not to start new work.

# PHASE 3 — CLARIFY (only if needed)

For the proposed next task, list:

- **Assumptions** the task description leaves implicit, that you intend to act on.
- **Open questions** that materially affect the implementation. Group `[blocker]`, `[important]`, `[minor]`.
- **Spec amendments proposed** before starting, if your reading of the codebase suggests the task as written is wrong, ambiguous, or stale. State the amendment as a diff against the relevant spec section.

Stop on blockers. Do not proceed until the user resolves them. For non-blockers, state how you will proceed and what you will note in the journal.

# PHASE 4 — EXECUTE

Implement the task. Constraints:

- Match existing codebase conventions identified in the spec's Background and Constraints section. Do not introduce new patterns, dependencies, or abstractions unless the task explicitly calls for them.
- Write the tests required by the task, not just the production code. Tests and code land together.
- Keep changes scoped to the task's declared file list. If you must touch a file outside that list, stop and propose a spec amendment first.
- Commit at logical points within the task with messages that reference the task ID (e.g. `T-04: add validator for Foo input`).
- If you discover that the task as specified cannot be completed correctly, stop and propose a spec amendment. Do not work around the spec.

# PHASE 5 — VERIFY (current task)

Walk the Definition of Done item by item. For each item, produce evidence:

- Tests: paste the runner output showing the new tests passing and the existing suite still green.
- Lint and types: paste the runner output showing zero new violations.
- Docs: name the files updated and quote the new sections.
- Observability: name the log lines, metrics, or spans added and where they appear.
- Acceptance criteria: walk each Given/When/Then and state how it is satisfied.

If any item is not satisfied, the task is not done. Either finish it or split out the unfinished portion as a follow-up task in the spec.

# PHASE 6 — UPDATE ARTIFACTS

Do all of the following before claiming the task complete:

- **Update the spec.** In the Task Breakdown section, mark the task `done`, add the date and the commit SHA range. If any open questions were resolved during the task, move them out of the Open Questions section into the relevant design section as decisions, with rationale.
- **Append a journal entry.** Format:

  ```
  ## <YYYY-MM-DD> — <Task ID>: <Title>

  **Status:** done | partial | blocked
  **Commits:** <SHA range or list>
  **Files touched:** <list>
  **Tests added:** <list of test names or files>
  **DoD verification:** <one line per DoD item with evidence pointer>
  **Decisions made:** <any in-flight design decisions, with rationale>
  **Spec amendments:** <links to any sections amended, with one-line summary>
  **Surprises and learnings:** <anything that future sessions or reviewers should know>
  **Next task pointer:** <Task ID, or "awaiting user input on <question>">
  ```

- **Update the next-task pointer.** Identify the next task per the dependency graph and state it explicitly in the journal entry. This is the handoff for the next session.
- **Surface new risks or open questions.** If the work revealed any, add them to the spec's Risks or Open Questions sections.

# PHASE 7 — CHECKPOINT GATE

Check the spec's Review Checkpoints section. If the just-completed task is the trigger for a checkpoint:

- Stop. Do not proceed to the next task.
- Summarize what the reviewer should focus on, citing the checkpoint's review focus.
- Provide a diff summary or PR-ready description.
- Wait for explicit user confirmation that the checkpoint has passed before continuing.

If no checkpoint is triggered, return to Phase 1 for the next task, re-reading the spec rather than relying on memory of it.

# WHAT NOT TO DO

- Do not skip Phase 1 on the assumption that you remember the spec from earlier in the session. Re-read it at every task boundary.
- Do not declare Definition of Done satisfied with a summary like "tests pass and docs updated." Cite specific evidence per item.
- Do not silently deviate from the spec when reality bites. Propose an amendment, get approval, then proceed.
- Do not start a new task while the previous task's closeout is incomplete.
- Do not batch closeouts to session end. Close out at every task boundary.
- Do not edit files outside the current task's declared scope without proposing a spec amendment first.
- Do not add new dependencies, frameworks, or abstractions mid-task. If they are needed, stop and amend the spec.
- Do not write speculative code for "the next task" while finishing the current one.
- Do not produce a spec amendment without showing the diff against the existing section. Amendments are surgical, not rewrites.

# AMENDMENT PROTOCOL

When the spec must change mid-execution:

1. Stop work.
2. State the section of the spec being amended.
3. Show the proposed change as a diff (before / after) or as a clearly marked addition.
4. State the reason: what was discovered, why the original is wrong or stale.
5. State the impact: which task IDs are affected, whether any completed work is invalidated, whether any review checkpoints need to be re-run.
6. Wait for user approval.
7. On approval, apply the amendment to the spec, log the amendment in the journal under the current task's entry, then resume.

Amendments are first-class events. They are how the spec stays accurate over time without becoming fiction.
````

### Design notes on the execution prompt

Here are the design decisions worth flagging, since they directly address pain points you've worked through before:

**Closeout at every task boundary, not at session end.** This is the single biggest reliability lever, and it's a direct response to the context-decay problem you've hit where session closeout instructions get missed by the time the session ends. Phase 6 fires after every task, so even if the session dies abruptly, the last completed task is already clean in the spec and the journal. The "next task pointer" written into the journal entry becomes the handoff for the next session, regardless of which agent or environment picks it up.

**Re-anchor at task transitions.** Phase 1 explicitly tells the agent to re-read the spec at every task boundary rather than trusting earlier-in-session memory of it. This matches the periodic re-anchoring mitigation you've already explored in your multi-agent framework.

**Spec is the source of truth, with an explicit amendment protocol.** The Amendment Protocol section turns "implementation revealed the plan was wrong" from a silent failure mode into a first-class event that requires diff, rationale, impact analysis, and approval. This stops the common drift where the code and the spec slowly diverge until the spec is fiction.

**Pre-flight verification of the previous task.** Phase 2 forces the agent to prove the last task actually closed out cleanly before starting new work. This catches the "claimed done but tests weren't actually re-run" failure mode.

**Definition of Done with evidence per item.** Phase 5 explicitly bans summary statements like "tests pass and docs updated." Each DoD item must be backed by output, file paths, or quoted sections. This pairs with the spec authoring prompt's requirement that DoD items be objectively verifiable.

**Journal format matches your existing development-journal pattern.** The journal entry template has explicit fields for Decisions made, Spec amendments, Surprises and learnings, and Next task pointer, which is the same shape you've been using to feed your process improvement agent.

**Checkpoint gate is a hard stop.** Phase 7 will not allow the agent to continue past a review checkpoint without explicit user confirmation. This is the safety valve that keeps the branch deployable at every checkpoint per the spec.

**How the two prompts fit together:** the authoring prompt produces a spec where every task has a DoD, every checkpoint has exit criteria, and the structure is stable. The execution prompt assumes that structure and exploits it. If you use the execution prompt against a spec that wasn't authored with the first prompt, it will still work, but the DoD verification phase will be looser because the spec won't have the same enforced rigor.

**Suggested file layout in a repo:**

```
docs/
  prompts/
    spec-writing-prompt.md
    spec-execution-prompt.md
  specs/
    <feature-name>.md           # the spec itself
    <feature-name>.journal.md   # appended at every task boundary
```

Want me to add a third prompt for the review checkpoint itself, where the reviewer (you, or another agent acting as reviewer) walks the diff against the spec's review focus and exit criteria? That would close the loop on the SDLC: author → execute → review → next task.

---

## User

Yes, please do.

## Assistant

### Artifact: `spec-review-prompt.md`

````markdown
# Review Checkpoint Prompt

Third in the trilogy with `spec-writing-prompt.md` and `spec-execution-prompt.md`. Use this when a Review Checkpoint defined in the spec has been triggered. The reviewer (a teammate, your future self, or another agent) walks the diff against the checkpoint's declared review focus and exit criteria, produces a structured verdict, and records the outcome back into the spec and journal.

## How to use

1. Fill in the `INPUTS` block with the checkpoint identifier and the diff range.
2. Paste from `# ROLE` onward into the agent or hand the whole document to a human reviewer.
3. The reviewer reads the spec checkpoint first, then the journal, then the diff. Findings are produced in a structured form. The verdict is written back to the spec and journal so the next session knows whether to proceed.

---

## INPUTS (fill these in)

```
SPEC_PATH: <repo-relative path, e.g. docs/specs/feature-x.md>
JOURNAL_PATH: <repo-relative path, e.g. docs/specs/feature-x.journal.md>
CHECKPOINT_ID: <e.g. CP-1, or the name from the spec's Review Checkpoints section>
DIFF_RANGE: <e.g. main..feature/x, or a list of commit SHAs, or a PR URL>
TASK_IDS_IN_SCOPE: <task IDs covered by this checkpoint, from the spec>
REVIEWER: <name or role>
```

---

# ROLE

You are a code reviewer evaluating whether a body of work satisfies a specific Review Checkpoint declared in a specification. The spec is the contract. Your job is to verify that the implementation matches what the spec said it would be, that the checkpoint's exit criteria are met, and that the review focus areas declared in the spec have been addressed with evidence.

You distinguish between **spec compliance findings** (objective; block merge until resolved) and **advisory findings** (subjective or out of spec scope; surfaced as comments, not blockers). You do not invent new requirements that the spec did not declare. If you believe the spec is wrong, you propose an amendment under the Amendment Protocol rather than blocking on something the spec did not require.

# OPERATING PRINCIPLES

1. **Spec first, code second.** Read the checkpoint definition before reading any code. The spec tells you what to look for.
2. **Evidence per finding.** Every finding cites a file path, line range, spec section, or test name. No vague verdicts.
3. **Compliance vs. advisory.** A finding is `[blocker]` only if it violates the spec, the Definition of Done, or the checkpoint's exit criteria. Style preferences, refactoring ideas, and "I would have done it differently" are `[advisory]`.
4. **Drift is a first-class finding.** If the implementation does not match the spec, that is a finding regardless of whether the implementation is otherwise good. Either the code is wrong, or the spec needs an amendment. Both must be resolved.
5. **No new requirements.** If you want something the spec did not require, propose a spec amendment for the next iteration. Do not block this checkpoint on it.
6. **The verdict is recorded.** The review outcome is written back to the spec and the journal. Future sessions need to know whether the checkpoint passed.

# PHASE 1 — ORIENT

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

# PHASE 2 — SCOPE VERIFICATION

Verify that the diff stayed within what the spec authorized:

- For each task in scope, list the files declared in the task's Scope field. Compare against the files actually touched in the diff.
  - Files touched and declared: `[ok]`.
  - Files declared but not touched: `[review]` — was this task fully implemented?
  - Files touched but not declared: `[blocker unless amended]` — either out of scope, or the spec was amended in the journal. Cite the amendment if present.
- Verify that no new dependencies, frameworks, or top-level abstractions were introduced unless the spec explicitly authorized them.
- Verify that the tasks' declared tests are present in the diff. Missing test files are `[blocker]`.

Findings in this phase are usually `[blocker]` because they indicate the implementation went somewhere the spec did not sanction. The remedy is either to revert the out-of-scope work or to retroactively amend the spec under the Amendment Protocol with explicit user approval.

# PHASE 3 — REVIEW FOCUS WALK

For each item in the checkpoint's **review focus** (quoted from Phase 1), walk the diff and produce findings.

For each focus item:

- **Focus item.** Quoted from the spec.
- **What was reviewed.** Files and line ranges examined.
- **Findings.** Itemized. Each finding is tagged `[blocker]`, `[important]`, `[advisory]`, or `[ok]`. Each cites a file path and line range, and references the spec section or principle it relates to.
- **Verdict on this focus item.** `pass`, `pass with comments`, or `fail`.

The review focus is the most important section of the review. It is what the spec author identified as worth deep attention for this checkpoint. Do not shortcut it.

# PHASE 4 — EXIT CRITERIA VERIFICATION

Walk the checkpoint's **exit criteria** one item at a time. For each:

- **Criterion.** Quoted from the spec.
- **Evidence.** File path, test output, CI status, doc location, log line, metric name. Whatever proves the criterion is met.
- **Verdict.** `met` or `not met`.

If any criterion is `not met`, the checkpoint fails regardless of other findings. Exit criteria are non-negotiable; that is what makes them exit criteria.

# PHASE 5 — GENERIC QUALITY PASS

A lighter-weight pass over items that the spec's Non-functional Requirements section already declared. For each NFR category relevant to the tasks in scope, verify:

- **Security.** Input validation present where required, secrets not logged, authorization checks at declared boundaries.
- **Observability.** Logs, metrics, traces match the names and conventions declared in the spec. Log levels are appropriate. Sensitive fields are not logged.
- **Performance.** No obvious regressions against declared budgets. Loops over unbounded inputs flagged. N+1 queries flagged.
- **Reliability.** Timeouts, retries, idempotency keys present where the spec required them. Error handling is consistent with codebase conventions.
- **Backward compatibility.** Public APIs, schemas, and config defaults unchanged unless the spec authorized changes.
- **Configuration.** New env vars or feature flags documented and defaulted as the spec specified.

Findings here are typically `[important]` or `[advisory]`. Promote to `[blocker]` only when an NFR was declared in the spec and is clearly violated.

# PHASE 6 — TEST AND DOCUMENTATION REVIEW

- **Test coverage.** Verify that every acceptance criterion from each task in scope has at least one test. Cite the test by name. Missing coverage on a declared acceptance criterion is `[blocker]`.
- **Test quality.** Tests assert behavior, not implementation. Tests are independent and deterministic. Fixtures match the spec's declared test data approach.
- **Documentation.** Inline doc comments are present on new public interfaces. README, API docs, runbooks, or operator-facing docs are updated as the spec required. Missing docs declared in the spec are `[blocker]`; nice-to-have docs are `[advisory]`.

# PHASE 7 — VERDICT

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

# PHASE 8 — UPDATE ARTIFACTS

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

- **If spec amendments were proposed,** route them through the Amendment Protocol from the execution prompt. Amendments are not applied silently as part of a review.
- **If the outcome is `pass` or `pass with comments`,** state the next task ID per the dependency graph so the next session has a clear pickup point.

# WHAT NOT TO DO

- Do not begin reviewing the diff before reading the checkpoint definition. The spec tells you what to focus on; reading code first biases the review toward whatever happens to be salient in the diff.
- Do not approve without citing evidence per exit criterion. "Looks good" is not a verdict.
- Do not invent new requirements. If something is missing that the spec did not require, raise it as an advisory or propose a spec amendment for next iteration; do not block on it.
- Do not conflate `[blocker]` with `[advisory]`. Blockers are spec or DoD violations. Advisories are preferences. Mixing them creates review fatigue and trains implementers to ignore comments.
- Do not rubber-stamp based on the journal entry alone. The journal is the implementer's claim; the diff is the evidence. Verify against the diff.
- Do not silently accept out-of-scope file changes. Either confirm a spec amendment is recorded or block until one is.
- Do not skip the exit criteria walk. A checkpoint with unmet exit criteria fails regardless of how good the rest of the work looks.
- Do not produce a verdict without the structured format. Future sessions and reviewers depend on the predictable shape.

# REVIEWER NOTES

- For human reviewers: the prompt structure is a checklist. You can fill it in conversationally in a PR description or review tool, as long as every section is addressed.
- For agent reviewers: produce the report as a single markdown document and append it to the journal. Do not skip phases even when a section appears trivially satisfied; the absence of findings in a phase is itself a recorded outcome.
- For self-review (reviewer is also the implementer): run the full prompt anyway. Self-review with a structured checklist catches more than self-review without one. Be especially honest about advisory findings; they are easier to dismiss when reviewing your own work.
````

### Design notes on the review prompt

Design notes on what makes this one different from a generic code review checklist:

**Spec is read before code.** Phase 1 forces the reviewer to quote the checkpoint contract verbatim before looking at the diff. This stops the most common review failure: reviewing whatever happens to be salient in the diff rather than what the spec said to focus on. Especially important for self-review, where you already know what you wrote and will defend it instead of test it.

**Blocker vs. advisory is a hard distinction.** A finding is a blocker only if it violates the spec, the DoD, or an exit criterion. Everything else is advisory. This stops review fatigue and prevents the spec from being silently expanded mid-implementation through reviewer preferences. It also matches how good engineering teams operate: the contract is the contract; opinions are opinions.

**Drift is a first-class finding.** If the code and spec disagree, the review records that as a finding regardless of which side is "right." Resolution goes through the Amendment Protocol from the execution prompt, not through silent acceptance. This is what keeps the spec accurate over time.

**No new requirements during review.** If the reviewer wants something the spec didn't require, the channel is "propose a spec amendment for next iteration," not "block this checkpoint." This is the discipline that lets specs survive contact with multiple reviewers without becoming unbounded.

**Verdict is structured.** The Phase 7 format is fixed because future sessions and agents need to find the outcome programmatically. "Pass with comments" and "changes requested" are deliberately distinct: the first lets work proceed, the second stops it.

**Self-review case is called out.** You'll often be the only reviewer (Sandlot work, side projects, contracting setups where there's no peer reviewer). Phase 8's structure plus the explicit "be especially honest about advisory findings" note in Reviewer Notes addresses the natural bias to wave through your own work.

**How the loop closes.** Author → execute → review → next task. Every artifact (spec, journal, review verdict) is durable and re-readable, so a session can drop in at any phase without losing context. This is what makes the trilogy actually spec-driven rather than chat-driven: the chat is ephemeral; the spec, journal, and verdicts are the working memory of the project.

---

## Suggested repo layout

```
docs/
  prompts/
    spec-writing-prompt.md
    spec-execution-prompt.md
    spec-review-prompt.md
  specs/
    <feature-name>.md           # the spec itself
    <feature-name>.journal.md   # appended at every task boundary, with review verdicts
```

## How the trilogy fits together

1. **Author** — Run `spec-writing-prompt.md` against the feature description and codebase. Produces `<feature-name>.md` after a forced discovery → clarify → spec pause.
2. **Execute** — Run `spec-execution-prompt.md` at the start of each working session. Orients against the spec, verifies prior task closeout, proposes the next task, executes one task to Definition of Done, and updates the spec and journal at the task boundary. Repeats until a Review Checkpoint is hit.
3. **Review** — Run `spec-review-prompt.md` when a checkpoint is triggered. Produces a structured verdict that gets written back to the spec and journal. Outcome determines whether execution resumes, work is revised, or a spec amendment is needed.
4. **Repeat** — Execution and review alternate until the spec's task list is complete.

Every step has durable artifacts (spec, journal, review verdicts) so a session can drop in at any phase without losing context. The chat is ephemeral; the artifacts are the working memory.
