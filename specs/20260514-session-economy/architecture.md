# Session Economy and Multi-Repo Disciplines — Architecture and Protocol Specification

> Status: Approved — CP-1 closed 2026-05-20
> Date: 2026-05-14
> Author: waseric + Copilot
> Audience: AI agents executing the spec-driven development skills, human users invoking and reviewing them

## 1. Overview

Two cross-cutting concerns have emerged from repeated use of the spec-driven development skill family: **session/token economy** (deciding when to continue vs. start fresh at task boundaries) and **multi-repo commit discipline** (ensuring spec/journal updates are committed in the spec repo alongside code commits in the codebase repo). Both are already partially addressed in `spec-execute` but require strengthening there and propagation to sibling skills.

This spec defines where each discipline belongs, what each skill must do, and the specific edits required. The changes are surgical — each skill remains self-contained; no shared-disciplines document or cross-reference dependency is introduced.

## 2. Goals and Non-goals

**Goals:**

- Eliminate the need for the user to manually append session-economy and multi-repo-commit instructions to every `/spec-execute` invocation.
- Add "token economy" as a concrete, named factor in `spec-execute`'s session continuity check, alongside the existing heuristic factors.
- Propagate multi-repo commit discipline to `spec-amend` and `spec-review`, which currently have no multi-repo awareness.
- Add multi-repo awareness notes to `spec-write` and `spec-design` output format sections so downstream execution sessions inherit the configuration.
- Keep each skill self-contained — no external discipline file that skills must cross-reference.

**Non-goals:**

- Introducing a shared "disciplines" document that all skills import. Each skill inlines what it needs.
- Adding token-economy checks to skills that lack task boundaries (`spec-write`, `spec-design`, `project-constitution`).
- Automating multi-repo detection via tooling (e.g., git remote introspection). The skill adds guidance for the agent to detect and confirm; actual tooling is out of scope.
- Changing the structure or phase numbering of any skill. All changes are additive within existing phases and sections.

## 3. Background and Constraints

### Prior state

`spec-execute` Phase 8 (Session Continuity Check) already provides a detailed session-continuity rubric. The user's recurring instruction adds one dimension Phase 8 does not name: **token economy** — the literal resource cost of continuing (tokens consumed, context window saturation, billing).

`spec-execute` Phases 4 and 6 already support a multi-repo workflow via `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` inputs and "paired commit" prose. But:

- The multi-repo discipline is woven into paragraphs rather than called out as a standalone rule.
- The user must remember to set `SPEC_REPO_ROOT` each session; no detection guidance exists.
- `spec-amend` and `spec-review` update spec/journal artifacts but have zero multi-repo awareness — commits land in whichever repo the agent happens to be in.

### Constraints

- Skills are consumed by LLM agents with finite context windows. Additions must be concise.
- Skills must remain independently readable. A reader of `spec-amend` should not need to read `spec-execute` to understand the multi-repo discipline.
- The skill family is versioned in the `ai-tools` repo. Changes land as edits to existing SKILL.md files.

## 4. Architecture

No new components. The architecture is the existing skill family with two disciplines propagated to the skills where they apply:

```
                    ┌──────────────────┐
                    │ project-const.   │  (no changes)
                    └──────────────────┘
                             │
              ┌──────────────┴──────────────┐
              ▼                              ▼
   ┌──────────────────┐          ┌──────────────────┐
   │   spec-design    │          │   spec-write      │
   │  + output note   │          │  + output note    │
   └──────────────────┘          └──────────────────┘
              │                              │
              └──────────────┬───────────────┘
                             ▼
                  ┌──────────────────┐
                  │   spec-execute   │
                  │  + token economy │
                  │  + multi-repo    │
                  │    detection     │
                  └──────────────────┘
                     │            │
              ┌──────┘            └──────┐
              ▼                          ▼
   ┌──────────────────┐       ┌──────────────────┐
   │   spec-amend     │       │   spec-review    │
   │  + multi-repo    │       │  + multi-repo    │
   │    INPUTS/commit │       │    INPUTS/commit │
   └──────────────────┘       └──────────────────┘
```

**Vocabulary:**

- **Token economy**: The practice of weighing tokens consumed, context window remaining, and billing cost when deciding whether to continue a session or start fresh.
- **Multi-repo paired commit**: When spec/journal files live in a different repo than the code, every task closeout (or amendment, or review verdict) produces commits in *both* repos referencing the same task/amendment ID. Neither commit is complete without the other.
- **Multi-repo detection**: Agent guidance in Phase 1 (Orient) to notice when `SPEC_PATH` resolves to a different repo than the working directory, and to confirm/set `SPEC_REPO_ROOT` proactively.

## 5. Detailed Design

### 5.1 — `spec-execute` Phase 8: Add token-economy factor

**Purpose.** Ensure the agent explicitly reasons about token/context-window consumption when recommending continue-vs-pause.

**Change.** Add "Token economy" as a factor in the "Factors to weigh" list, and add a row to the rubric table.

**New factor text:**

> - **Token economy.** How much of the model's context window has this session consumed? Long sessions with extensive code reads, error traces, or exploration burn context that crowds out the next task's working space. When the session has consumed a large fraction of available context, a fresh session gives the next task full headroom. Also consider billing: if the model charges per token, a fresh session with a clean prompt-cache hit on the spec is often cheaper than continuing with a bloated context.

**New rubric row:**

> | Session has consumed significant context (long reads, traces, false starts) | fresh session |

**Why this design.** The existing Phase 8 factors are cognitive/drift heuristics. Token economy is a resource constraint that operates independently — a session can be cognitively fresh but context-window-full. Naming it explicitly ensures the agent surfaces it rather than treating it as implicit.

### 5.2 — `spec-execute` Phase 1: Add multi-repo detection

**Purpose.** Eliminate the "I forgot to set `SPEC_REPO_ROOT`" failure mode by having the agent detect the multi-repo case during orientation.

**Change.** Add a step to Phase 1's read-order list and a bullet to the Orientation Report.

**New read-order step (after existing step 3):**

> 4. **Multi-repo detection.** If `SPEC_REPO_ROOT` is not set, check whether `SPEC_PATH` resolves within `CODEBASE_ROOT`. If it does not — e.g., the spec is at a path in a different working tree — surface this and confirm with the user: "The spec appears to live in a different repo than the codebase. Should I treat `<detected-path>` as `SPEC_REPO_ROOT`?" Set it upon confirmation. If `SPEC_REPO_ROOT` is already set, confirm it is still correct (repos move).

**New Orientation Report bullet:**

> - **Multi-repo state.** Whether `SPEC_REPO_ROOT` is set (explicitly or detected). If set, confirm both repos are on the expected branches and have no uncommitted changes that would block paired commits.

**Why this design.** Detection-then-confirm is safer than silent inference. The user gets a one-time confirmation per session instead of restating `SPEC_REPO_ROOT` every invocation.

### 5.3 — `spec-amend`: Add multi-repo awareness

**Purpose.** When an amendment's spec/journal live in a different repo than the code that triggered the amendment, the commit discipline must match `spec-execute`'s paired-commit pattern.

**Changes:**

1. Add `SPEC_REPO_ROOT` to the INPUTS block with a note that it is optional and inherited from the triggering `spec-execute` session when present.
2. In Phase 4 (Apply), add a paragraph about multi-repo commits.
3. In Phase 5 (Journal), note that the journal commit lands in `SPEC_REPO_ROOT` when set.

**New INPUTS entry:**

> ```
> SPEC_REPO_ROOT: <optional; path to the repo where SPEC_PATH lives, when different from the codebase being amended; inherited from the triggering spec-execute session if set>
> ```

**New Phase 4 paragraph (after the existing commit message block):**

> **Multi-repo case.** When `SPEC_REPO_ROOT` is set, the spec and journal edits are committed in `SPEC_REPO_ROOT`, not in the codebase repo. The amendment commit message references the same amendment ID as any related code-side changes. Do not let the amendment commit ship without verifying the codebase-side state is consistent — if the amendment changes task scope, the implementer's next code commit must reflect it.

**New Phase 5 note (after the journal entry format block):**

> When `SPEC_REPO_ROOT` is set, the journal lives in `SPEC_REPO_ROOT`. Stage and commit the journal entry in that repo. The commit references the amendment ID and is paired with any code-side commit that prompted the amendment.

### 5.4 — `spec-review`: Add multi-repo awareness

**Purpose.** When review verdicts update spec/journal artifacts that live in a different repo, the commit must land in the correct repo.

**Changes:**

1. Add `SPEC_REPO_ROOT` to the INPUTS block.
2. In Phase 8 (Update Artifacts), add a multi-repo note.

**New INPUTS entry:**

> ```
> SPEC_REPO_ROOT: <optional; path to the repo where SPEC_PATH and JOURNAL_PATH live, when different from the codebase under review>
> ```

**New Phase 8 paragraph (after the journal entry format block):**

> **Multi-repo case.** When `SPEC_REPO_ROOT` is set, spec and journal updates are committed in `SPEC_REPO_ROOT`, not in the codebase repo. The commit message references the checkpoint ID. This is the paired commit to whatever code-side work was reviewed — do not let one ship without the other.

### 5.5 — `spec-write` and `spec-design`: Output format note

**Purpose.** When the spec author knows the spec will live in a different repo than the code, downstream `spec-execute` sessions need `SPEC_REPO_ROOT` set. Surfacing this at authoring time prevents the "forgot to set it" failure in execution.

**Change.** Add a note to the OUTPUT FORMAT section of both skills.

**New note (identical in both skills):**

> - If the spec will be committed to a different repo than the codebase it describes, note this in the spec's §3 Background section and include `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` values for downstream `spec-execute` sessions. This eliminates the need for the executor to rediscover the multi-repo layout each session.

## 6. Non-functional Requirements

- **Adoptability.** Changes are additive. No existing behavior is removed or contradicted. An agent running the old skills produces correct (if incomplete) behavior; an agent running the updated skills gets the two disciplines automatically.
- **Conciseness.** Each addition is 1–3 paragraphs. Total addition across all five skills is under 40 lines of new prose.
- **Self-containment.** Each skill remains independently readable. The multi-repo discipline is stated in each skill that needs it, not cross-referenced from a shared file.

## 7. Implementation Sequencing

**Phase 1 — `spec-execute` edits (token economy + multi-repo detection).** The primary skill. All other changes are secondary.

**Phase 2 — `spec-amend` and `spec-review` edits (multi-repo awareness).** These can land independently of each other and of Phase 1, but Phase 1 establishes the vocabulary.

**Phase 3 — `spec-write` and `spec-design` edits (output format note).** Lightest touch. Can land any time after Phase 1.

No downstream feature spec is needed — the changes are direct edits to existing skill files, well-specified in §5 above.

## 8. Validation Approach

- **Self-test.** After applying edits, re-read each modified skill end-to-end and verify it reads coherently — no contradictions, no dangling references, no duplicated prose.
- **Invocation test.** Invoke `/spec-execute` on a multi-repo project without manually appending the two instructions. Verify the agent: (a) reasons about token economy in Phase 8, and (b) detects or confirms `SPEC_REPO_ROOT` in Phase 1.

## 9. Review Checkpoints

### CP-1 — All edits applied

- **Trigger.** All five skills edited.
- **Review focus.** Coherence within each skill; no contradictions with existing prose; no excessive length; vocabulary consistency ("paired commit," "token economy," "multi-repo" used consistently).
- **Exit criteria.** Each skill reads as if the discipline was always part of it — no seam visible.
- **Status.** pass with comments on 2026-05-20 by Claude (delayed formal closeout). One advisory recorded (Phase 8 rubric near-overlap at .agents/skills/spec-execute/SKILL.md:172,174 — distinct enough to keep). See journal entry of 2026-05-20.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Additions make skills too long, causing agents to lose focus on the new content | Low | Medium | Keep additions under 3 paragraphs each; match existing voice | Author |
| Multi-repo prose in spec-amend/spec-review is confusing without spec-execute context | Low | Low | Each skill's addition is self-contained; no cross-reference needed | Author |

## 11. Adoption Path

Immediate. Updated skill files are picked up on next invocation. No migration, no versioning, no rollback needed — if the additions prove unhelpful, they are removed with a revert.

## 12. Out of Scope

- Automated multi-repo detection tooling (git remote parsing, workspace folder introspection). The skills add agent guidance, not tool calls.
- Token-counting instrumentation. The agent estimates context consumption; no API call to check exact token count.
- Changes to `project-constitution`. No task boundaries, no multi-repo commit pattern.
- Session-economy checks in `spec-write`, `spec-design`, or `spec-amend`. These skills don't have iterative task boundaries where a continue-vs-pause decision applies.

## 13. Open Questions

### OQ-1 — Should `spec-execute` Phase 8 auto-detect multi-repo state?

**Question.** Phase 1 detects multi-repo state. Should Phase 8's session continuity check also factor in multi-repo state (e.g., "the paired commit in the spec repo hasn't been pushed yet — don't pause until it is")?

**Analysis.** Phase 6 already requires both commits before declaring a task done. Phase 8 runs after Phase 6. If Phase 6 is followed, the multi-repo state is clean by the time Phase 8 runs. Adding it to Phase 8 would be redundant.

**Leaning.** No — Phase 6 is sufficient. Phase 8 stays focused on session economy.

**Owner.** Resolved here; no downstream action.

## 14. References

- `spec-execute` SKILL.md — Phase 8 (Session Continuity Check), Phase 4 (Execute, multi-repo case), Phase 6 (Update Artifacts, multi-repo case)
- `spec-amend` SKILL.md — Phase 4 (Apply), Phase 5 (Journal)
- `spec-review` SKILL.md — Phase 8 (Update Artifacts)
- `spec-write` SKILL.md — Output Format section
- `spec-design` SKILL.md — Output Format section
