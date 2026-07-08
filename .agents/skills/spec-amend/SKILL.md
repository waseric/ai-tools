---
name: spec-amend
lastUpdated: 2026-07-08
description: Propose, structure, and apply an amendment to an existing spec (design spec or feature spec) when implementation, review, or new information reveals the spec must change. Walks the user through stating the section being amended, showing the change as a diff (before/after), capturing reason and impact, getting explicit approval, applying the change, and logging the amendment in the journal as a first-class event. Use when work has revealed that the spec is wrong, stale, or under-specified — instead of silently deviating. Pairs with `spec-write` and `spec-design` (the spec being amended), `spec-execute` (which surfaces drift), and `spec-review` (which may propose amendments rather than block).
---

# Spec Amend

When the spec must change after it leaves Draft status, the change goes through this skill. Silent deviation is the alternative, and silent deviation is how specs become fiction.

This skill exists because amendments are easy to neglect when work is in flight: the implementer is mid-task, the reviewer wants to ship, the user wants progress. Amendments are first-class events with their own discipline; folding them into the execution or review skill makes them invisible. Pulling them out keeps them visible.

## How this skill works

When invoked, you act as the agent. The user (or another skill — `spec-execute`, `spec-review`, or an in-flight `spec-design` or `spec-write` session) has identified that a spec needs to change. Your job is to structure the proposal, drive approval, apply the change, and record the amendment in the journal so it survives context decay.

Amendments are surgical, not rewrites. If a spec needs a rewrite, that is a new spec, not an amendment.

## INPUTS

```
SPEC_PATH: <repo-relative path to the spec being amended; e.g. specs/YYYYMMDD-feature-x/feature.md>
JOURNAL_PATH: <repo-relative path; e.g. specs/YYYYMMDD-feature-x/journal.md>
SECTION: <name or number of the section being amended; e.g. "§5 Detailed Design — Validator", "Task T-04 Scope">
TRIGGER: <what surfaced the need; e.g. "spec-execute T-04 hit a contradiction", "spec-review CP-2 raised an out-of-spec finding", "user noticed during planning">
PROPOSED_CHANGE: <the change itself, if the user already drafted it; otherwise the skill drafts from conversation>
APPROVER: <who must approve; usually the user; may be a named reviewer for governance-heavier contexts>
SPEC_REPO_ROOT: <optional; path to the repo where SPEC_PATH lives, when different from the codebase being amended; inherited from the triggering spec-execute session if set>
```

---

# ROLE

You are the steward of an amendment. The spec is the contract; you are documenting the contract's revision. Your job is not to design the change (that is the spec author's job) but to structure it, drive approval, apply it cleanly, and make it visible in the project's working memory.

You distinguish between three change classes:

- **Amendment** — a change to a Draft-or-later spec, after it has been committed or shared. Goes through this skill.
- **In-flight edit** — a change to a Draft spec that has not been shared, while the original `spec-write` or `spec-design` session is still running. Does not go through this skill; just edit.
- **Rewrite** — the spec is so wrong that patching it is dishonest. Stop and propose a new spec; do not pretend this is an amendment.

# OPERATING PRINCIPLES

1. **Surgical, not sprawling.** An amendment targets one section, or a coherent set of related sections. If the diff touches half the spec, it is a rewrite — route accordingly.
2. **Diff is required.** Every amendment shows the change as before/after (or as a clearly marked addition). No "rewrite this section" without showing what the rewrite is.
3. **Reason is required.** Why did the spec need to change? What was discovered? Without a reason, the amendment looks arbitrary three months later.
4. **Impact is required.** Which task IDs are affected? Which checkpoints need to be re-run? Is any completed work invalidated? An amendment without impact is half a thought.
5. **Approval is explicit.** Amendments are not applied silently as part of execution or review. The approver says yes, in writing, before the spec changes.
6. **Journal entry is required.** Every amendment leaves a journal entry. Future sessions need to know the spec changed and why.
7. **Status, if present, advances.** If the spec has a Status banner (`Draft — Open for Review` / `Approved` / `Superseded`), amendments to an `Approved` spec require explicit confirmation that the new state is still `Approved` (sometimes an amendment is significant enough to revert to Draft).

# PHASE 1 — ORIENT

Read, in order:

1. **`JOURNAL_PATH`'s `## Current State` block first** (if the journal exists). STATE — Phase, Last completed, Next, Open holds, Pending checkpoint, Archive, Latest entry — is your handoff from prior sessions. Then cross-check STATE's **derivable** fields — *Last completed* and the *Latest entry* anchor — against **one grep** of the true latest entry (`grep -nE '^## [0-9]{4}-[0-9]{2}-[0-9]{2} — ' journal.md | tail -1`). If they match, trust STATE and proceed. If they mismatch, STATE is stale: range-read the true latest entry before acting (staleness degrades cost by one read, never correctness).
2. **The Amend-section working set from `SPEC_PATH`**: the full **SECTION** being amended (quote the current text verbatim — you will need it for the diff) + sections cross-referencing it + prior amendments to it. Consult the grammar first: if a `## Grammar` block is declared at the journal head (or a spec grammar section), grep its exact anchor patterns to locate the section and prior amendments; else use the native reference dialect and broad-union discovery patterns. spec-amend reads the spec anyway, so it uses the spec's codified grammar if present, else its native default — it does **not** re-consult the constitution.
3. The **TRIGGER** context: if `spec-execute` surfaced this, read the relevant task and journal entry; if `spec-review` surfaced this, read the review verdict; if the user surfaced this, capture their statement.
4. The journal at `JOURNAL_PATH` for prior amendments to the same section (an amendment that contradicts a recent prior amendment is a signal that something deeper is unstable).

Then output an **Orientation Report**:

- **Section in scope.** Quote the current text. Cite the spec line range.
- **Trigger.** One paragraph on what surfaced the need.
- **Cross-references.** Other sections, tasks, or checkpoints that reference this section. Each will be evaluated for impact in Phase 3.
- **Prior amendments to this section, if any.** Cite by date and rationale. Note if this amendment contradicts a recent one — that is a stability signal worth surfacing.
- **Class classification.** `amendment` / `in-flight edit` / `rewrite`. Justify the choice. If `rewrite`, stop and route to a new `spec-write` or `spec-design` session.

# PHASE 2 — DRAFT THE AMENDMENT

Produce the amendment in the structured form below. If the user supplied `PROPOSED_CHANGE`, structure it. If not, draft from conversation, then ask the user to confirm.

```markdown
## Amendment <YYYY-MM-DD-N> — <SPEC_PATH §SECTION>

**Trigger.** <One paragraph. What was discovered, by whom, in what context.>

**Section.** <Section name and line range in the spec.>

**Change.**

Before:
> <Quoted current text, verbatim. Use blockquote so the diff is visually distinct.>

After:
> <Quoted proposed text, verbatim. Use blockquote.>

**Reason.** <Why the original is wrong, stale, or under-specified. Two or three sentences.>

**Impact.**
- **Affected tasks:** <Task IDs, or "none">
- **Affected checkpoints:** <Checkpoint IDs, or "none">
- **Completed work invalidated:** <Yes/no; if yes, name the work>
- **Cross-references requiring follow-up:** <Section names; or "none">

**Status implication.** <Does this amendment keep the spec at its current status, or does it revert to Draft? Justify if non-default.>

**Approver.** <Name and date when approved. Filled in after Phase 3.>
```

# PHASE 3 — APPROVAL

Stop. Present the structured amendment to the approver (usually the user). Wait for one of:

- **Approved as drafted.** Proceed to Phase 4.
- **Approved with revisions.** Capture the revisions, update the draft, re-present. Iterate until approved.
- **Rejected.** Capture the rejection rationale in the journal as a *non-amendment* (so the discussion is recoverable next time the same issue surfaces). Do not modify the spec. End.
- **Reclassified as rewrite.** Stop. Route to a new `spec-write` or `spec-design` session, depending on the spec class. End this skill.

Do not proceed to Phase 4 without explicit approval. Approval is not implied by silence.

# PHASE 4 — APPLY

Apply the change to the spec:

- Edit the SECTION in `SPEC_PATH` to match the `After:` text.
- If status implication is "revert to Draft," update the Status banner and the date.
- If cross-references in other sections need follow-up edits, apply them now (each follow-up is part of the same amendment, not a separate amendment — keep the unit coherent).
- If the spec has a "Format note" at the top declaring deviations from a template, and this amendment changes the deviation, update the Format note.

Then commit the change with a message that references the amendment ID:

```
spec: amendment <YYYY-MM-DD-N> — <one-line summary>

See <JOURNAL_PATH> for full amendment record.
```

**Multi-repo case.** When `SPEC_REPO_ROOT` is set, the spec and journal edits are committed in `SPEC_REPO_ROOT`, not in the codebase repo. The amendment commit message references the same amendment ID as any related code-side changes. Do not let the amendment commit ship without verifying the codebase-side state is consistent — if the amendment changes task scope, the implementer's next code commit must reflect it.

# PHASE 5 — JOURNAL

**Overwrite the journal's `## Current State` block** in the same commit as the entry below: overwrite STATE in place (Phase, Last completed, Next, Open holds, Pending checkpoint, Archive, Latest entry) to reflect the amendment just applied. STATE is the *only* in-place-mutated part of the journal; the entries below it stay append-only.

Append a journal entry to `JOURNAL_PATH`:

```markdown
## <YYYY-MM-DD> — Amendment <YYYY-MM-DD-N>

**Section amended:** <SPEC_PATH §SECTION>
**Trigger:** <One line>
**Reason:** <One line>
**Impact summary:** <Affected tasks/checkpoints/completed-work in one line>
**Approver:** <Name>
**Approved on:** <YYYY-MM-DD>
**Status implication:** <kept | reverted to Draft | other>
**Commit:** <SHA>

### Full record
<Paste the structured amendment from Phase 2 here, with the Approver field filled in.>
```

When `SPEC_REPO_ROOT` is set, the journal lives in `SPEC_REPO_ROOT`. Stage and commit the journal entry in that repo. The commit references the amendment ID and is paired with any code-side commit that prompted the amendment.

The journal entry is the durable record. The spec itself shows only the new state; the journal preserves the *transition* — what was, what is, and why.

# PHASE 6 — DOWNSTREAM HANDOFF

State next actions explicitly:

- **If a task was in progress when this amendment was triggered:** state whether work resumes against the amended spec, restarts from scratch, or is paused. The implementer needs to know.
- **If a checkpoint was open:** state whether the checkpoint is closed, re-opened, or re-scoped. The reviewer needs to know.
- **If the amendment invalidated completed work:** name the work and route to whichever skill handles the redo (usually `spec-execute` re-running a task; sometimes `spec-write` re-decomposing).
- **If the spec status reverted to Draft:** state who must re-approve and by when.

# CROSS-SKILL CASE

When a single finding affects more than one spec — a citation that recurs across siblings, a methodology-level decision that touches every spec in a related set, a lineage error inherited from an upstream source — the amendment applies as a single coherent change across all affected artifacts under a single amendment ID. The case is a specialization of the six-phase workflow, not a replacement: Phases 1–3 run normally on the trigger artifact; Phases 4–5 apply the four-step mechanics below across every affected artifact.

## Four-step mechanics

1. **Surface.** Name where the finding originated — per-spec CP-1 verdict, batch synthesis, methodology-level open question, sibling-spec drift audit — in the structured Phase 2 Trigger field. The origin determines the primary record location (step 3).

2. **Trace upstream.** Identify every artifact affected by the same finding before drafting. Sibling specs sharing a citation, journals carrying the same stale claim, design specs sharing a lifecycle commitment — the traced set defines the scope of the amendment. Do not split into per-artifact amendments; do not narrow to the trigger artifact alone.

3. **Apply as single amendment ID.** One amendment ID (`YYYY-MM-DD-N`) spans every affected artifact. The shape is two functional commits: one spec-edit commit covering all `SPEC_PATH`-side edits across the affected set; one journal commit covering all journal entries (primary + companions). Both commits cite the amendment ID. The journal-commit SHA is backfilled into each amendment entry's `**Commit:**` field after the journal commit lands, per the standard backfill convention; the backfill is a small follow-up commit, not a third functional change.

4. **Verify at every endpoint.** Each affected artifact's verification record is cited explicitly in the Impact block — grep post-edit confirming the change applied, an audit closure verifying drift is closed at that endpoint, or equivalent check. Verification is per-endpoint; one passing endpoint does not stand in for the others.

## Primary record vs. companion records

The structured Phase 2 amendment block lives at the cycle's natural anchor:

- **Per-spec finding trigger** (CP-1, CP-2, in-flight authoring discovery) → the originating spec's journal holds the primary. Sibling specs' journals carry companion entries that reference the primary by amendment ID.
- **Methodology-level decision trigger** (batch synthesis, cross-spec lifecycle decision) → the methodology-level journal (e.g., a batch audit journal) holds the primary. Each affected spec's journal carries a companion entry that references the primary by amendment ID.

Companion records summarize impact for their own spec and link to the primary; they do not duplicate the structured Phase 2 block. This preserves a single source of truth — readers reach the full record through one canonical entry rather than reconciling N parallel records.

## When the case does not apply

- **Single-artifact amendments**, including those touching multiple sections within the same spec. Phase 4's "each follow-up is part of the same amendment" rule already covers within-spec coherence; the cross-skill case adds nothing.
- **Cross-reference maintenance follow-ups** — edits to other files that simply update stale lines pointing at the amended artifact. These ride under the same amendment ID per Phase 4, but the four-step mechanics do not apply (no "trace upstream," no "verify at every endpoint"); the follow-ups are bookkeeping, not parallel amendments.
- **Self-referential amendments to this skill.** Self-referential amendments hold their primary record in the affected skill's own retroactive-spec journal regardless of trigger origin; the skill's retroactive-spec adoption-path documents the staged-bootstrap pattern that governs the rest.

# OUTPUT FORMAT

- Phases 1–3 are conversational.
- Phase 4 produces an Edit to the spec and a commit.
- Phase 5 produces an Edit to the journal.
- Phase 6 is conversational, with explicit names for downstream actions.

# WHAT NOT TO DO

- Do not apply an amendment without explicit approval. Approval-by-silence corrodes the spec contract.
- Do not produce an amendment without a diff. "Update §5 to reflect the new approach" is not an amendment; it's a TODO.
- Do not bundle unrelated changes into one amendment. Each amendment targets one coherent change. If you find yourself editing §5 and §11 for unrelated reasons, that's two amendments.
- Do not treat an amendment as a chance to rewrite the section. Surgical, not sprawling. If the section needs a rewrite, the spec needs a rewrite — route accordingly.
- Do not skip the journal entry. The spec shows the new state; the journal preserves the transition. Without the journal, three-months-from-now-you cannot tell what changed or why.
- Do not amend an `Approved` spec without addressing whether the status holds. Sometimes the amendment is significant enough that the spec returns to Draft for re-approval; the skill must surface this question, not paper over it.
- Do not invoke this skill for in-flight edits to a Draft spec that has not been shared. Just edit. This skill is for amendments to specs that have already become a contract.
- Do not let `spec-execute` or `spec-review` silently apply amendments. Both should propose; this skill applies. The separation is what makes amendments visible.

# HANDOFF NOTES

- **From `spec-execute`.** When `spec-execute`'s Phase 3 (Clarify) or Phase 4 (Execute) reveals the spec is wrong, the implementer halts and routes here. The amendment carries the implementer's evidence (file path, test output, contradiction) into the Trigger and Reason fields.
- **From `spec-review`.** When `spec-review`'s Phase 3 (Review Focus Walk) or Phase 5 (Generic Quality Pass) raises a finding that the reviewer believes should be a spec change rather than a code change, the reviewer proposes an amendment here. The Verdict's "Spec amendments proposed" section is the handoff.
- **From `spec-design` or `spec-write`.** Once the design or feature spec leaves Draft and is committed, further changes go through this skill. While still in Draft (no commit, no share), the author edits directly.
- **To `spec-execute` (resumption).** After Phase 6, work resumes against the amended spec. The implementer re-orients via `spec-execute`'s Phase 1 (Orient), which will read the amended spec and the new journal entry.

---

# Notes on what makes this skill load-bearing

**Amendments are how specs survive contact with reality.** Without an explicit channel, every contradiction between spec and implementation is resolved silently — usually in favor of the code, because the code is what runs. The spec drifts, then becomes fiction, then becomes ignored. The amendment channel is the discipline that keeps the spec honest.

**Visibility is the point.** The previous design (folding the Amendment Protocol into `spec-execute`) worked, but amendments were buried inside execution sessions. Pulling them into a named skill — invoked explicitly, recorded in journals as their own event type — makes them visible to reviewers, future sessions, and the user scrolling back through history.

**Surgical-not-sprawling.** The discipline of small, focused amendments keeps the spec readable. A spec with thirty surgical amendments over a year is a healthy spec. A spec with three sprawling rewrites masquerading as amendments is a spec that should have been replaced.

**Approval-is-explicit.** Silence is not approval. This rule is harder to hold in fast-moving sessions than it sounds, which is why the skill is structured to *stop* at Phase 3 and wait. If the user can read the structured amendment and not respond, the work that surfaced the amendment can wait too.
