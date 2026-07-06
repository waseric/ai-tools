# Receipt schema (dispatch mode)

Normative schema for the **receipt** a dispatch-mode worker returns as its final
message. The receipt is the *only* thing that enters the orchestrator's context
per task, so it is fixed-shape and length-capped. Authoritative design source:
[dispatch-execution §5.4](../../../specs/20260705-dispatch-execution/architecture.md).

Read by two parties: the **orchestrator** (via `spec-execute`'s DISPATCH MODE
section) to accept or reject a task, and the **worker** (via the `spec-worker`
agent definition) as the shape it must emit.

## Shape

Hard cap: **25 lines**. Fields, in order:

```text
TASK: <ID> — <title>
STATUS: done | partial | blocked
COMMITS: <SHA list; both repos when paired>
DOD: <one line per DoD item: PASS/FAIL + evidence pointer + the re-runnable command, verbatim>
FILES: <count>; <list, truncated at 10 with "+N more">
AMENDMENT-TRIGGER: none | <one-line trigger + affected spec section>
SURPRISES: none | <≤2 lines>
NEXT: <next-task pointer per dependency graph>
```

## Rules

- **Evidence is pointers plus re-runnable commands — never payloads.** File
  contents, diffs, test-output bodies, and build logs are forbidden in a
  receipt. A DoD result names *where* the evidence is and *the command that
  re-derives it*, not the output itself.
- **At least one `DOD` line per task must carry a command the orchestrator can
  re-run.** This is the derivation re-check hook: Phase 2 re-runs it and
  compares against the receipt's claim before the task is accepted.
- **`FILES` is cross-checked** against `git diff --stat` in the re-check; a file
  list exceeding the task's declared scope is an amendment trigger (scope creep).
- **`STATUS ≠ done` or `AMENDMENT-TRIGGER ≠ none` stops the batch** in every
  autonomy mode. A partial/blocked task is re-dispatched with an updated brief
  or stopped to the operator — never silently completed by the orchestrator.

## Why capped

The cap is what keeps orchestrator context growth to ~1–2k tokens per accepted
receipt, so batch cost scales with task count rather than task working-set.
Commands-as-evidence is what lets the cap coexist with the verification
doctrine (`CLAUDE.md`'s "mechanically re-derivable claims"): the orchestrator
re-derives every done-claim instead of trusting the narrative.
