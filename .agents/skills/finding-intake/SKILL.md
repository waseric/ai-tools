---
name: finding-intake
lastUpdated: 2026-05-17
description: Convert a signal — a textual narrative, an external pointer, or both — into a finding artifact at `specs/findings/YYYYMMDD-<short-name>/` with `status: intake` set and the starter Intake journal entry written. This is the lowest-ceremony entry to the Findings Pipeline (Phase B per the [findings-pipeline architecture spec](../../../specs/20260517-findings-pipeline/architecture.md)): capture the signal at the moment of noticing, with operator effort ≤60 seconds for a typical case, then hand off to downstream skills. Pairs with `finding-triage` (next phase; classifies the signal), and ultimately with `spec-amend` / `spec-write` (downstream consumers that turn routed findings into spec changes).
---

# Finding Intake

The finding-intake skill is the door into the Findings Pipeline. Its job is to capture a signal — something the operator noticed that does not yet have a home — as a structured, self-contained artifact, fast enough that the act of capture is not its own deterrent.

The skill is Phase B of the [Findings Pipeline design](../../../specs/20260517-findings-pipeline/architecture.md). Phase A delivered the schema (template + journal template + field reference at [specs/findings/README.md](../../../specs/findings/README.md)); this skill is what the operator or agent invokes to produce a new finding from that schema. Triage, investigation, and routing happen in later phases against later skills.

The central design constraint is the **60-second operator effort target** from the design spec's NFRs. Every prompt the skill might impose has to justify itself against that target. The skill defers every optional field, accepts "unknown" honestly, and never asks the operator to do triage-phase work at intake.

## How this skill works

When invoked, you act as the agent. The operator (or another skill, or an in-flight session) wants to convert a signal into a finding. Your job is to populate the Intake section of a new finding artifact from the signal, write the starter Intake journal entry, and surface the produced path to the caller.

The skill runs in two modes:

- **Interactive mode** — typical human use. Prompt for required fields; offer sensible defaults; confirm a derived short-name with the operator in one round-trip.
- **Structured-input mode** — typical AI-agent use. The caller populates the INPUTS block; the skill skips the prompts and proceeds straight to APPLY.

Mode selection is implicit: if the INPUTS block is fully populated by the caller, run structured; otherwise, run interactive and gather the missing fields.

## INPUTS

```
TITLE: <one-line human-readable title of the signal>                        (required)
SUMMARY: <one paragraph of what was noticed>                                (required unless EXTERNAL_POINTER alone is supplied AND a fetch will be attempted with operator-supplied prose follow-up — but see §4 pointer-fetch policy: a summary is always strongly preferred)
EXTERNAL_POINTER: <URL, or comma- or newline-separated list of URLs/pointers>  (optional; required if SUMMARY empty)
REPORTED_BY: <reporter; defaults to "self">                                 (optional)
REPORTED_VIA: <signal source; defaults derived from inputs>                 (optional)
CAPTURED_BY: <name; persona-frame "intake" added by the skill>              (optional; defaults to git user.name)
DOMAIN: <operational | testing | security | methodology | other | unknown> (optional; defaults to "unknown")
SHORT_NAME: <kebab-case slug; derived from TITLE if omitted>                (optional)
DATE: <YYYY-MM-DD; defaults to operator's local date>                       (optional)
```

---

# ROLE

You are the operator's first hand at the moment of noticing. The signal might be sharp or fuzzy, complete or one-line, sourced from the operator's own observation or from an external thread someone else opened. Your job is to get it into the pipeline, not to interpret it.

Regardless of who is invoking the skill — a developer mid-feature, a service-desk operator triaging tickets, the user reading email — the persona-frame for this phase is **intake**. Intake's job is to capture, not to classify. Domain, severity, reproducibility, scope, route — those are triage's questions. Intake records what was seen and gets out of the way.

# OPERATING PRINCIPLES

1. **Capture rate beats capture quality.** A rough finding in the pipeline is more useful than a polished one that the operator never wrote. Prefer "unknown" or "see external reference" over a third prompt.
2. **"Unknown" is first-class — but only for fields actually attempted.** A field genuinely investigated and indeterminate is `unknown`. A field belonging to a phase that has not started (Triage, Investigation, Route) stays in `<placeholder>` form. Do not pre-fill later-phase fields with `unknown`.
3. **Never silently swallow a fetch failure.** When a URL pointer is supplied and a fetch is attempted, the outcome — success with snapshot, failure with reason — is visible to the operator before the artifact is finalized.
4. **Never prompt for triage-phase fields at intake.** No reproducibility questions, no scope, no severity confirmation, no triage notes. Those fields stay in `<placeholder>` form in the produced artifact.
5. **The artifact is self-contained.** It must not require the originating conversation, the operator's session memory, or any external system to remain reachable. Quoted summary and any successful URL snapshot travel inside the artifact.
6. **Persona-frame is fixed.** `Captured by` is always labeled `intake`. Do not ask the operator which persona they are speaking from — that is a 60-second-target violation.
7. **Working-tree-leave, not auto-commit.** The skill produces files; it does not stage or commit. A suggested commit message is returned to the operator. The operator commits when their session is in a commit-friendly state.

# PHASE 1 — ORIENT

Read, in order:

1. The schema artifacts, if not already loaded in this session:
   - [specs/findings/README.md](../../../specs/findings/README.md) — field reference, state machine, status semantics, persona-frame taxonomy.
   - [specs/findings/_template/finding.md](../../../specs/findings/_template/finding.md) — canonical artifact template.
   - [specs/findings/_template/journal.md](../../../specs/findings/_template/journal.md) — canonical journal template with starter Intake entry.
2. The operator's signal: whatever was supplied in INPUTS, or the conversation context in interactive mode.

In interactive mode, confirm intent with one round-trip:

> "I'll capture this as a new finding. Working title: `<TITLE-as-supplied-or-derived>`. Signal source: `<text only | URL only | text + URL>`. Anything missing before I draft?"

In structured-input mode, skip the round-trip; proceed directly to Phase 2.

# PHASE 2 — DRAFT

Derive the missing fields and produce the artifact-shape preview.

**Short-name derivation.** From `TITLE` (when `SHORT_NAME` not supplied):
- Lowercase.
- Strip punctuation except hyphens.
- Replace whitespace with hyphens.
- Drop common stop words (`the`, `a`, `an`, `of`, `is`, `are`, `and`, `or`, `to`, `for`, `in`, `on`, `with`) when the resulting slug still has ≥3 word-pieces.
- Truncate to ≤40 characters, ending on a word boundary.
- Resulting slug shape: 3–5 word-pieces.

**Date handling.** Default `DATE` to the operator's local date. `Date opened` and `Last transition` in the finding both take this value at intake.

**Captured by.** Read `git config user.name`; emit as `<name>; persona-frame: intake`. If `user.name` is unset, fall back to "unknown; persona-frame: intake" and journal the absence.

**Reported via derivation.** If `SUMMARY` only → `text`. If `EXTERNAL_POINTER` only → `URL`. If both → `text + URL`.

Show the operator (interactive mode only):

- The target directory path: `specs/findings/<DATE>-<SHORT_NAME>/`.
- The proposed `finding.md` Intake section, fully populated.
- The proposed starter journal entry.

Pause for one-step confirm/edit. In structured-input mode, proceed without confirmation.

# PHASE 3 — APPLY

Create the artifact:

1. **Make the directory** `specs/findings/<DATE>-<SHORT_NAME>/`. If a directory at that path already exists, surface the collision to the operator and offer either (a) a disambiguating suffix (`-2`, `-alt`) or (b) cancellation.
2. **Write `finding.md`** by copying [`_template/finding.md`](../../../specs/findings/_template/finding.md) verbatim, then editing only:
   - The status banner: `Status: intake`; `Domain: <DOMAIN or "unknown">`; `Date opened` / `Last transition` to `DATE`.
   - The Intake section (all five fields).
   - **Leave Triage / Investigation / Route sections in `<placeholder>` form unchanged.**
3. **Write `journal.md`** by copying [`_template/journal.md`](../../../specs/findings/_template/journal.md) verbatim, then editing only:
   - The title heading.
   - The starter Intake entry (date, summary line, all four fields).
   - **Leave the commented skeletons for later transitions in place.**
4. **External-pointer handling.** When `EXTERNAL_POINTER` is supplied (one or more pointers):
   - Require `SUMMARY` to be supplied as well. If the operator provided a pointer but no summary, prompt once for a one-paragraph summary before proceeding — the summary is the load-bearing capture; the URL is durability convenience.
   - If the invoking agent has URL-fetching capability, **attempt** a fetch on each pointer in turn.
   - On success: snapshot the relevant content into the `External references` field of the Intake section, prefixed `<!-- fetched <DATE> -->`. Keep snapshots reasonable in size — quote the load-bearing portion, not the whole page.
   - On failure: **do not silently proceed.** Surface the failure (URL, status code or error) to the operator and present three choices: **retry**, **proceed without snapshot** (acknowledging reduced durability), **cancel**. Record the chosen outcome in the journal entry regardless of which is chosen.
   - If the invoking agent has no fetch capability (pure-human path), instruct the operator to either paste relevant content from the URL into the Summary field manually or journal the absence of snapshot.
5. **Suggested commit message.** Return to the operator: `find: <SHORT_NAME> intake — <one-line summary>`. The skill does not run `git add` or `git commit`. The operator commits when convenient.
6. **Return the artifact path** to the caller in the closing message: `specs/findings/<DATE>-<SHORT_NAME>/`.

# OUTPUT FORMAT

Closing message states, explicitly:

```
Created: specs/findings/<DATE>-<SHORT_NAME>/
  - finding.md (status: intake)
  - journal.md (starter Intake entry)

Suggested commit:
  git add specs/findings/<DATE>-<SHORT_NAME>/
  git commit -m "find: <SHORT_NAME> intake — <one-line summary>"

Fetch outcomes (if any): <per-pointer one-liner: succeeded / failed-with-reason / not-attempted>
```

If multiple external pointers were attempted, one line per pointer.

# WHAT NOT TO DO

- **Do not prompt for triage-phase fields.** Reproducibility, scope, severity confirmation, triage notes — all left in `<placeholder>` form. Asking at intake is a 60-second-target violation and bleeds Phase C into Phase B.
- **Do not silently swallow a URL-fetch failure.** When the operator supplied a pointer and a fetch was attempted, the outcome must be visible to the operator before the artifact is finalized. "Proceed without snapshot" is an acceptable choice; *silently* proceeding without snapshot is not.
- **Do not run a deduplication scan against existing findings.** Adds ceremony that defeats the 60-second target. Deduplication is triage's responsibility.
- **Do not rewrite or paraphrase the templates.** The produced `finding.md` and `journal.md` match the canonical templates byte-for-byte at every position not occupied by operator input. Field names, section headings, ordering, and the commented skeletons are preserved verbatim.
- **Do not pre-fill later-phase fields with `unknown`.** `<placeholder>` means "this phase has not started"; `unknown` means "we looked, we couldn't tell." Conflating them corrupts the schema's information content.
- **Do not auto-commit.** The skill produces files; the operator decides when to commit. The skill's responsibility ends at returning a suggested commit message.
- **Do not ask the operator which persona-frame they're in.** Intake's persona-frame is fixed to `intake` regardless of who the operator is.
- **Do not embed the originating conversation or session memory in the artifact.** The Summary field carries the load-bearing capture; the artifact stands alone after intake closes.
