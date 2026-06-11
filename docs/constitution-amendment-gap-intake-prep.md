# Pre-intake notes — Constitution-amendment workflow gap

> Audience: a future session that will file this finding via `/finding-intake`.
> Status: pre-intake context dump. Read this file, then invoke `/finding-intake` with the INPUTS block at the bottom.

## What this file is

A context dump for a follow-on advisory finding identified during the five-amendment cascade that resolved [finding intake-template-folder-dependency](../specs/findings/20260517-intake-template-folder-dependency/) on 2026-05-17. The cascade committed the **Atomic-Skill Portability Principle** in `specs/tech-stack.md` and threaded it through the findings-pipeline design spec, schema feature spec, and both finding-{intake,triage} skills.

During that cascade, a methodology gap surfaced that did not block the work but should not be lost: there is no defined ceremony for amending constitution documents. This finding captures the gap so it can be triaged separately, in its own session, against the now-updated findings pipeline.

## The finding, in one sentence

`/spec-amend`'s documented scope names design specs and feature specs as its targets, not constitution documents (`mission.md`, `tech-stack.md`, `roadmap.md`). There is no `/constitution-amend` skill, the `project-constitution` skill does not address amendment workflow, and the constitution documents have no journal files. The cascade resolved this pragmatically (informal use of `/spec-amend` against `tech-stack.md`; the amendment journaled inside the originating finding's journal) — but the gap remains.

## Recommended SUMMARY text (for the finding's Intake section)

> During a five-amendment cascade resolving finding intake-template-folder-dependency (2026-05-17), an amendment to `specs/tech-stack.md` was required to commit the Atomic-Skill Portability Principle as a methodology-wide constraint on skill construction. `/spec-amend`'s documented scope (in [.agents/skills/spec-amend/SKILL.md](../.agents/skills/spec-amend/SKILL.md)) explicitly names design specs and feature specs as its targets — not constitution documents (`mission.md`, `tech-stack.md`, `roadmap.md`). The `project-constitution` skill bootstraps these documents but does not address amendment workflow. The constitution documents have no journal files. Pragmatic resolution for the cascade was to use `/spec-amend` informally against `tech-stack.md` and to journal the amendment inside the originating finding's journal (rather than create a new constitution-level journal). The gap: there is no defined ceremony for amending constitution documents, no convention for journaling those amendments, and no skill specifically tasked with that workflow. Worth deciding whether to define a `/constitution-amend` skill, formally extend `/spec-amend`'s scope to include constitution docs, add journal files to the constitution, or accept the informal pattern as the intended convention. Decision is for triage / investigation; this finding only captures the observation.

## Relevant context (for whoever picks this up)

**Where the gap was surfaced.** Session of 2026-05-17, during the cascade that resolved [intake-template-folder-dependency](../specs/findings/20260517-intake-template-folder-dependency/). Specifically: when invoking `/spec-amend` for Amendment 1 of the cascade against `specs/tech-stack.md`, the operator and the agent both recognized that `tech-stack.md` was a constitution doc (produced by `project-constitution`), not a design or feature spec — outside the documented scope of `/spec-amend`.

**Why the cascade did not block on this.** Three options were considered at the time:

- **(a) Use `/spec-amend` informally** against `tech-stack.md`. Defensible because the skill's disciplines (state the section, diff it, capture reason and impact, explicit approval, journal entry) all apply equally well to constitution docs.
- **(b) Edit `tech-stack.md` directly** with a journal-style commit message, treating the constitution as held by convention rather than by review checkpoint contract.
- **(c) File a separate finding** for the gap first, route it, then do the amendment.

Option (a) was chosen for the cascade work — but with explicit recognition that the gap warranted a sibling finding. This file is the prep for that sibling finding.

**The five-amendment cascade for reference.** Each amendment's commit pair (primary + SHA backfill):

| # | Target | Commit | SHA backfill |
|---|---|---|---|
| 1 | `specs/tech-stack.md` | `0018c4c` | `6bc3cc0` |
| 2 | `specs/20260517-findings-pipeline/architecture.md` | `fa1d153` | `7894553` |
| 3 | `specs/20260517-findings-pipeline-schema/feature.md` + templates | `9a1a717` | `ed4b81c` |
| 4 | `specs/20260517-finding-intake-skill/feature.md` + skill + bundled templates | `394133c` | `85816f8` |
| 5 | `specs/20260517-finding-triage-skill/feature.md` + skill + bundled templates | `acde3e1` | `e961448` |

**Routing entry of the originating finding.** Commit `8ba20d4` — see [specs/findings/20260517-intake-template-folder-dependency/finding.md §Route](../specs/findings/20260517-intake-template-folder-dependency/finding.md) and its journal's `Routed` entry, which explicitly names "Constitution-amendment workflow gap" as the second of two follow-on items (the first being the audit of spec-* skills for principle compliance).

## What this finding is NOT

It is not a request to ship a `/constitution-amend` skill. It is not a position on which resolution is correct. It is *capture* — recording an observation so the methodology does not silently accept "no defined ceremony" as the answer by neglect. The triage and investigation phases of this finding's pipeline will sort out the resolution direction. Intake only needs to land the artifact.

## Possible resolution directions (for triage/investigation only — do not pre-populate at intake)

These are candidate framings for the triage and investigation phases to consider. They are listed here purely as context, not as the intake's findings:

- **Define a `/constitution-amend` skill** distinct from `/spec-amend`, with conventions specific to constitution docs (e.g., journal file added to the constitution; a re-approval step if the change is significant; explicit approver list).
- **Formally extend `/spec-amend`'s documented scope** to include constitution docs, treating the constitution as a special case of "spec" with its own discipline notes.
- **Add journal files to the constitution** (`tech-stack-journal.md`, `mission-journal.md`, `roadmap-journal.md`) without a separate skill — the informal `/spec-amend` use remains the workflow.
- **Accept the informal pattern as the intended convention** — i.e., a constitution amendment is journaled inside whatever finding or spec triggered it, and the constitution doc has no journal of its own by design.

Each has a tradeoff. Triage may surface a clear preference; investigation may produce a recommendation; route depends on which option survives examination.

## Domain, severity hints (for intake to record)

- **Domain (best guess at intake):** `methodology`. The gap is in methodology ceremony, not in operational/testing/security domains.
- **Severity:** leave at `<placeholder>` — intake's responsibility ends with the signal capture; severity is triage's first authoritative assignment.
- **Reported via:** `text` (this notes file is the signal source; no URL).
- **External references:** none (internal methodology observation).

## INPUTS block — paste into `/finding-intake` for structured invocation

```
TITLE: Constitution-amendment workflow undefined — /spec-amend documented scope names design/feature specs, not constitution docs
SUMMARY: During a five-amendment cascade resolving finding intake-template-folder-dependency (2026-05-17), an amendment to `specs/tech-stack.md` was required to commit the Atomic-Skill Portability Principle as a methodology-wide constraint on skill construction. `/spec-amend`'s documented scope (in `.agents/skills/spec-amend/SKILL.md`) explicitly names design specs and feature specs as its targets — not constitution documents (`mission.md`, `tech-stack.md`, `roadmap.md`). The `project-constitution` skill bootstraps these documents but does not address amendment workflow. The constitution documents have no journal files. Pragmatic resolution for the cascade was to use `/spec-amend` informally against `tech-stack.md` and to journal the amendment inside the originating finding's journal (rather than create a new constitution-level journal). The gap: there is no defined ceremony for amending constitution documents, no convention for journaling those amendments, and no skill specifically tasked with that workflow. Worth deciding whether to define a `/constitution-amend` skill, formally extend `/spec-amend`'s scope to include constitution docs, add journal files to the constitution, or accept the informal pattern as the intended convention. Decision is for triage / investigation; this finding only captures the observation.
EXTERNAL_POINTER:
REPORTED_BY: self
REPORTED_VIA: text
DOMAIN: methodology
SHORT_NAME: constitution-amendment-workflow-undefined
DATE: <today's date when invoking>
```

Notes on the INPUTS:

- `SHORT_NAME` is pre-derived to skip the round-trip; the skill would otherwise derive it from `TITLE`. The proposed slug is 41 characters; the skill's short-name derivation (in [finding-intake SKILL.md §Phase 2](../.agents/skills/finding-intake/SKILL.md)) targets ≤40 with a word-boundary truncation. If the skill auto-truncates, expect something like `constitution-amendment-workflow-undefined` → `constitution-amendment-workflow` (31 chars) or similar. Either is fine; the intake's job is to land the artifact, not to optimize the slug.
- `DATE` deliberately left as `<today's date when invoking>`. Set to the date of the new session, not 2026-05-17 — the captured-by date reflects when intake was performed, not when the signal was first surfaced. The summary already names 2026-05-17 as the surfacing date.
- `CAPTURED_BY` omitted to let the skill default to `git config user.name`.

## After intake lands

The finding will appear at `specs/findings/<DATE>-constitution-amendment-workflow-undefined/`. A separate session triages it (whenever the operator chooses); the four candidate resolution directions above feed naturally into triage notes. The constitution-amendment workflow gap is then in the pipeline rather than in a notes file — which is the goal.

This notes file can be deleted once the finding artifact exists.
