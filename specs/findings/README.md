# Findings — Schema and Usage

> Audience: methodology consumers (humans and AI agents) creating, triaging, investigating, or routing findings
> Upstream design spec: [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)

## What this directory holds

This directory is the home for the **Findings Pipeline** — the methodology's channel for observations made *outside* a Review Checkpoint. Each finding is a per-finding subdirectory `YYYYMMDD-<short-name>/` containing `finding.md` (the artifact) and `journal.md` (one entry per status transition). The `_template/` subdirectory holds the canonical templates that a new finding copies from; it is structural support, not a real finding (underscore-prefix follows the `_archive/` precedent from the [spec-path convention](../20260515-spec-path-convention/architecture.md)).

This README describes the **schema**: what fields a finding carries, what the status state machine looks like, and what role-frame orients each phase. The [design spec](../20260517-findings-pipeline/architecture.md) describes the **why**: what gap the pipeline closes, why these phases were chosen, what alternatives were rejected.

## State machine

A finding has exactly one current `status`, drawn from the values below. Status is append-only and forward-progressing under normal flow; reopening creates a new status entry that returns to an earlier phase, preserving prior history in the journal.

```
   ┌─────────┐      ┌──────────┐      ┌────────────────────┐      ┌─────────┐
   │ intake  │ ───► │ triaged  │ ───► │ under-investigation│ ───► │ routed  │
   └─────────┘      └──────────┘      └────────────────────┘      └─────────┘
                          │                                              │
                          │  (investigation may be skipped               │
                          │   when route is already obvious)             │
                          ▼                                              │
                    ┌─────────┐                                          │
                    │ closed  │ ◄────────────────────────────────────────┘
                    └─────────┘   (close is also a valid terminal directly)
                          │
                          ▼
                  ┌──────────────┐
                  │   reopened   │ ──► triaged | under-investigation
                  └──────────────┘
                  (any closed/routed finding may be reopened
                   with a new status entry; prior history kept)
```

**Terminal status mapping.** The four route subtypes map to two terminal status values: `spec-amend` and `spec-write` terminate at `status: routed` (action delegated to a downstream spec); `defer` terminates at `status: routed` with route subtype `defer` (action consciously postponed, watch condition recorded); `close` terminates at `status: closed` (no decision-producing action required). `routed` means "this finding produced a decision and is no longer the pipeline's responsibility"; `closed` means "this finding required no decision-producing action."

## Status semantics

### `intake`

- **Meaning:** Signal has been captured into a finding artifact. Domain, severity, and route are not yet known.
- **Persona-frame:** intake — service desk, manager, end-user, AI agent, or anyone.
- **Exit condition:** A triager picks up the finding and confirms (or refines) its summary, reproducibility, scope, domain, and severity.

### `triaged`

- **Meaning:** Hard facts about the finding's shape are established. Reproducibility, scope, domain, and severity are recorded.
- **Persona-frame:** triage — business-analyst frame (domain-knowledgeable; not yet opening code).
- **Exit condition:** Either investigation begins (status → `under-investigation`), or triage was sufficient to choose a route directly (status → `routed` or `closed` with skip rationale journaled).

### `under-investigation`

- **Meaning:** A developer-frame examination of code, configuration, or runtime state is in progress. Probable cause, touchpoints, and a proposed remedy are being established.
- **Persona-frame:** investigation — developer frame (opens files; cites line numbers).
- **Exit condition:** A route is chosen (status → `routed` or `closed`). May iterate within `under-investigation` if a first pass produces a partial answer.

### `routed`

- **Meaning:** A terminal route is selected: `spec-amend`, `spec-write`, or `defer` (with watch condition). The pipeline has handed off responsibility for the next action.
- **Persona-frame:** N/A — terminal state.
- **Exit condition:** None under normal flow. The finding remains `routed` indefinitely; reopening creates a new status entry.

### `closed`

- **Meaning:** No decision-producing action will be taken. Rationale recorded (cannot reproduce; expected behavior; out of scope; superseded; etc.).
- **Persona-frame:** N/A — terminal state.
- **Exit condition:** None under normal flow. Reopening creates a new status entry.

### `reopened`

- **Meaning:** A previously `routed` or `closed` finding has been re-activated. The new status entry names the phase being returned to (typically `triaged` or `under-investigation`) and the rationale for reopening.
- **Persona-frame:** matches the phase being returned to.
- **Exit condition:** Same as the phase being returned to. Prior status history is preserved in the journal.

## Persona-frame taxonomy

Personas are **orientation, not handoffs**. The discipline structures the work without requiring multi-person teams. A team adopting the methodology later inherits a structure that maps cleanly to multi-person handoffs; a solo adopter benefits from role-framed self-direction in the meantime.

Each phase declares its persona-frame explicitly, and an AI agent performing a phase on the operator's behalf adopts that frame as its prompt orientation.

| Phase | Persona-frame | Orientation |
|---|---|---|
| **intake** | service desk, manager, end-user, AI agent, or **anyone** | Capture-rate over capture-quality. Optimized for a 60-second landing from stray observation to parked artifact. |
| **triage** | business analyst (domain-knowledgeable) — service-desk-light or developer-heavy variants acceptable in solo work | Hard facts about the finding's shape: reproducibility, scope, domain, severity. Not yet opening code. |
| **investigation** | developer | Opens files; cites line numbers; proposes remedy. The first phase that touches the codebase. |

**Asymmetry by design.** Intake's persona-frame is intentionally broader than triage/investigation. The input source is unbounded — a stray observation in a meeting, an automated alert, or an external bug report are all valid signals — and gating intake on persona is incompatible with the 60-second capture-rate target. Triage and investigation, by contrast, do require a specific frame to produce useful output.

## Field reference

The fields below are the schema. `_template/finding.md` instantiates them with placeholders.

| Field | Type | Required by phase | Semantics |
|---|---|---|---|
| **Title** | text | intake | Short descriptive title; used as the heading and the directory short-name source. |
| **Status** | enum: `intake` \| `triaged` \| `under-investigation` \| `routed` \| `closed` \| `reopened` | intake | Current phase; see *State machine* above. |
| **Domain** | enum: `operational` \| `testing` \| `security` \| `methodology` \| `other` | intake (best guess) → confirmed in triage | What kind of finding this is. |
| **Severity** | enum: `blocker` \| `important` \| `advisory` | triage | Methodology axis. Extends `spec-review`'s vocabulary. |
| **Operational urgency** | optional enum: `P1` \| `P2` \| `P3` \| `P4` | triage (operational findings only) | Operational axis, decoupled from severity. Two axes may diverge. |
| **Date opened** | date `YYYY-MM-DD` | intake | When the finding artifact was created. |
| **Last transition** | date `YYYY-MM-DD` | every transition | Scan-aid: most recent status change without traversing the journal. |
| **Reported by** | text | intake | Reporter (may be self, a user, an external system). |
| **Reported via** | text | intake | Signal source: text, URL, or system pointer. |
| **Captured by** | text + persona-frame | intake | Whoever created the artifact, with the `intake` persona-frame label. |
| **Summary** | paragraph | intake | One paragraph of what was noticed; what is known and what is not. |
| **External references** | URLs or pointers (may be empty) | intake | Captured verbatim alongside the summary. Artifact does not assume the external system stays reachable. |
| **Triaged by** | text + persona-frame | triage | Triager + persona-frame label (e.g., `business analyst (solo: Eric)`). |
| **Triage date** | date `YYYY-MM-DD` | triage | When the triage transition was journaled. |
| **Reproducibility** | enum: `reliably` \| `intermittently` \| `not reproduced` \| `not applicable` | triage | Honesty over neatness — record `intermittently` or `not reproduced` rather than guessing. |
| **Repro steps** | ordered list | triage (when reproducible) | Numbered steps; may be empty when not applicable. |
| **Scope** | text | triage | Who or what is affected. |
| **Domain confirmation** | same enum as Domain | triage | Confirms or refines the intake guess. |
| **Severity confirmation** | same enum as Severity | triage | First authoritative severity assignment (intake may leave unknown). |
| **Triage notes** | free text | triage | Rejected hypotheses, clarifications from reporter, anything not captured above. |
| **Investigated by** | text + persona-frame | investigation | Investigator + `developer` persona-frame label. |
| **Investigation date** | date `YYYY-MM-DD` | investigation | When investigation began (or current iteration). |
| **Probable cause** | hypothesis with evidence | investigation | `file:line` references where applicable. |
| **Code/configuration touchpoints** | bulleted file paths | investigation | Files implicated by the cause analysis. |
| **Alternative hypotheses considered** | brief list with reasons rejected | investigation | Keeps reasoning auditable; prevents tunnel vision. |
| **Proposed remedy** | plain-language description | investigation | What change would resolve the finding. |
| **Route decision** | enum: `spec-amend` \| `spec-write` \| `defer` \| `close` | route | The terminal decision. |
| **Decided by** | text + persona-frame + operator | route | Persona-frame appropriate to the deciding phase. |
| **Route date** | date `YYYY-MM-DD` | route | When the route was chosen. |
| **Target spec** | path | route (when `spec-amend` or `spec-write`) | Path to the spec that will absorb the finding (e.g., `specs/<dir>/architecture.md`). |
| **Route rationale** | paragraph | route | One paragraph: why this route over the others. For `defer`, includes watch condition. |

## Creating a new finding

1. Copy the template into a new per-finding directory:

   ```sh
   mkdir -p specs/findings/$(date +%Y%m%d)-<short-name>
   cp specs/findings/_template/finding.md  specs/findings/$(date +%Y%m%d)-<short-name>/finding.md
   cp specs/findings/_template/journal.md  specs/findings/$(date +%Y%m%d)-<short-name>/journal.md
   ```

2. Fill the **Intake** section of `finding.md`. Leave Triage/Investigation/Route as placeholders — later phases append, they do not require pre-filling at intake.
3. Add an "Intake" entry to `journal.md` per the journal template's structure.
4. Set `Status: intake`, `Date opened: <today>`, `Last transition: <today>`.

The downstream `finding-intake` skill (Phase B in the [design spec's implementation sequencing](../20260517-findings-pipeline/architecture.md#7-implementation-sequencing)) will automate steps 1–4 once available. Until then, the manual copy is the supported path.
