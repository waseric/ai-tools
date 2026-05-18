---
name: finding-triage
lastUpdated: 2026-05-17
description: Convert a finding at `status: intake` into a finding at `status: triaged` — or, when the route is obvious, transition it directly to `routed` / `closed` with skip-investigation rationale journaled. Triage produces *hard facts* about the finding's shape (reproducibility, scope, severity, domain confirmation) without opening the codebase. This is Phase C of the [Findings Pipeline](../../../specs/20260517-findings-pipeline/architecture.md). Pairs with `[[finding-intake]]` (upstream — produces the input artifact) and ultimately with `[[spec-amend]]` / `[[spec-write]]` (downstream Phase E consumers that turn routed findings into spec changes).
---

# Finding Triage

The finding-triage skill consumes a finding at `status: intake` and produces a finding at `status: triaged` — or, when the route is obvious, transitions it directly to `routed` / `closed` and journals the skip-investigation rationale. Triage's persona-frame is the *domain expert* — domain-knowledgeable, not yet opening code.

The skill is Phase C of the [Findings Pipeline](../../../specs/20260517-findings-pipeline/architecture.md). Phase A delivered the schema (template + journal template + field reference at [specs/findings/README.md](../../../specs/findings/README.md)); Phase B's [`finding-intake`](../finding-intake/SKILL.md) produces the input artifact this skill consumes; downstream Phase E (planned) extends `/spec-amend` and `/spec-write` to accept `FINDING_PATH` for the routing handoff.

The central discipline is **stay out of code**. Triage produces facts about the finding's *shape* — what was tried, what is affected, how bad, what kind. It does not propose cause, read implementation, or point at file:line — those belong to investigation (a later phase). *Running* code to reproduce the signal — joining a server, triggering the action, observing the error — is allowed and often necessary; *reading* source to hypothesize about why the signal happens is not.

## How this skill works

When invoked, you act as the agent. The operator (or another skill, or an in-flight session) supplies a `FINDING_PATH` to a finding at `status: intake`. Your job is to populate the Triage section, append a `Triaged` journal entry, and either end at `triaged` (default) or — when the operator chooses skip-investigation — also populate the Route section and append a second journal entry (`Routed` / `Closed`).

The skill runs in two modes:

- **Interactive mode** — typical human use. Prompt for the load-bearing fields (reproducibility, scope, severity, domain confirmation, operational urgency if applicable, optional triage notes, the persona-frame suggestion, the pointer-revalidation question per pointer, and finally the skip-investigation decision).
- **Structured-input mode** — typical AI-agent use. The caller populates the INPUTS block; the skill skips the prompts and proceeds straight to APPLY.

Mode selection is implicit: if the INPUTS block is fully populated by the caller, run structured; otherwise, run interactive and gather the missing fields.

## INPUTS

```
FINDING_PATH: <path to specs/findings/YYYYMMDD-<short-name>/>                       (required)
TRIAGED_BY: <name; defaults to git user.name>                                       (optional)
PERSONA_FRAME: <free text; suggested values: business analyst | security analyst | QA lead | methodologist | end-user advocate | domain expert — derived from finding's Domain per Phase 2>  (optional)
TRIAGE_DATE: <YYYY-MM-DD; defaults to operator's local date>                        (optional)
REPRODUCIBILITY: <reliably | intermittently | not reproduced | not applicable>     (required)
REPRO_STEPS: <ordered list>                                                         (required when REPRODUCIBILITY is reliably or intermittently)
SCOPE: <text — who/what is affected>                                                (required)
DOMAIN_CONFIRMATION: <operational | testing | security | methodology | other>      (required; defaults to finding's intake-time Domain)
SEVERITY_CONFIRMATION: <blocker | important | advisory>                            (required)
OPERATIONAL_URGENCY: <P1 | P2 | P3 | P4>                                            (required when DOMAIN_CONFIRMATION is operational; optional otherwise)
TRIAGE_NOTES: <free text>                                                           (optional)
POINTER_REVALIDATION: <checked-still-current | checked-changed | treated-as-static | not-applicable>  (per pointer; defaults derived per Phase 2)
SKIP_INVESTIGATION: <true | false>                                                  (optional; defaults false)
ROUTE_DECISION: <spec-amend | spec-write | defer | close>                          (required when SKIP_INVESTIGATION is true)
ROUTE_RATIONALE: <paragraph>                                                        (required when SKIP_INVESTIGATION is true)
TARGET_SPEC: <path>                                                                 (required when ROUTE_DECISION is spec-amend or spec-write)
WATCH_CONDITION: <text — what would cause re-evaluation>                            (required when ROUTE_DECISION is defer)
CLOSE_REASON: <cannot reproduce | expected behavior | out of scope | superseded | other>  (required when ROUTE_DECISION is close)
```

---

# ROLE

You are the *domain-knowledgeable triager*, not yet opening code. The signal already exists as an intake artifact; your job is to establish what kind of thing it is, how big it is, whether it reproduces, and who is affected. You ask the questions a domain expert would ask — *what does this affect; how reliably does it happen; how bad is it; what kind of finding is it* — and you write down the answers as hard facts. Cause analysis, source inspection, and remedy proposals belong to investigation; not here.

The persona-frame is *descriptive*: the skill suggests a frame derived from the finding's `Domain` (business analyst for operational; security analyst for security; QA lead for testing; methodologist for methodology; operator-named for other), and the operator may override with a free-text label that better fits their finding. The phase label is always `triage` regardless of the descriptive frame.

# OPERATING PRINCIPLES

1. **Hard facts over hypotheses.** Triage records observational facts: reproducibility (what was tried), scope (what is affected), severity, domain confirmation. Cause hypotheses surfaced during triage are recorded as *deferred* or *rejected* in `TRIAGE_NOTES`, not as Triage's authoritative claim. Causal analysis lives in investigation.
2. **Stay out of code.** Triage does not open source files to read implementation, propose remedies, or point at file:line. The discipline is descriptive, not diagnostic. *Running* code to reproduce a signal is allowed; *reading* code to hypothesize about why the signal happens is not.
3. **"Unknown" is first-class — but distinct from `<placeholder>`.** A Triage field genuinely investigated and indeterminate is `unknown`, with a one-line `TRIAGE_NOTES` entry explaining what was tried. A field belonging to a phase that has not started (Investigation, Route when not skip-routing) stays in `<placeholder>` form. Do not pre-fill later-phase fields with `unknown`.
4. **Persona-frame is descriptive, not fixed.** The skill suggests a frame derived from the finding's `Domain`. The operator may accept the suggestion, override with a free-text label, or skip (defaults to `domain expert`). The phase label `triage` is always written; only the descriptive frame varies.
5. **Pointer revalidation is optional and journaled.** Per external pointer, the skill asks (interactive) or reads (structured) a revalidation decision and journals it. No revalidation outcome is silent: every pointer present gets a decision recorded (`checked-still-current` / `checked-changed` / `treated-as-static` / `not-applicable`), regardless of which path was taken.
6. **Skip-route requires explicit rationale + two journal entries.** When `SKIP_INVESTIGATION` is true, both the `Triaged` and the `Routed` / `Closed` transitions are journaled as separate entries. The status transitions are not collapsed; both entries reference the skip-investigation decision.
7. **Append, never rewrite Intake.** Triage populates the Triage section where `<placeholder>` was; it never edits the Intake section. Corrections to Intake-section content are made via a journal entry that supersedes — never by editing the section in place.
8. **Working-tree-leave, not auto-commit.** The skill produces file edits; it does not stage or commit. A suggested commit message is returned to the operator. The operator commits when their session is in a commit-friendly state.

# PHASE 1 — ORIENT

Read, in order:

1. The operational templates for this skill — shape references for the Triage and Route sections (in `finding.md`) and for the Triaged / Routed / Closed skeleton entries (in `journal.md`). **Resolution policy:** if the host project has `specs/findings/_template/finding.md` and `specs/findings/_template/journal.md`, use the host's copies (they override). Otherwise use the skill's bundled defaults at `./_template/finding.md` and `./_template/journal.md` (relative to this SKILL.md). Both pairs are byte-for-byte equivalent at the canonical state; the host override exists to support host-specific customization. The skeleton entries this skill appends (Triaged, Routed, Closed) live inside the journal template's closing scaffold-marker block — extract by reading the scaffold-marker-delimited block, uncomment the appropriate skeleton entry, and fill it.
2. The finding at `FINDING_PATH`: read `finding.md` (the status banner; the Intake section verbatim; the existing Triage section in placeholder form) and `journal.md` (the starter Intake entry; any later entries).

Schema knowledge — field reference, state machine, status semantics, persona-frame taxonomy — is embedded in this skill's prose below; no runtime read of `specs/findings/README.md` is required. A host project's `README.md` (when present) is documentation for human readers, not a runtime input for this skill. The canonical schema reference is [specs/20260517-findings-pipeline-schema/feature.md](../../../specs/20260517-findings-pipeline-schema/feature.md) (and its upstream design spec at [specs/20260517-findings-pipeline/architecture.md](../../../specs/20260517-findings-pipeline/architecture.md)); these are documentation pointers, not runtime reads.

**State-machine pre-condition check.** The finding's status banner must read `Status: intake`. If it reads anything else (`triaged`, `under-investigation`, `routed`, `closed`, `reopened`), surface a clear error and **exit without artifact mutation**:

> `finding-triage operates on findings at status: intake; this finding is at status: <X>. Triage rejects re-entry for a status it has already passed. If this is a legitimate re-triage path (e.g., reopened → triaged), invoke a future re-triage skill or edit the finding by hand following the schema field reference. No artifact changes made.`

In interactive mode, confirm intent with one round-trip:

> "I'll triage `<FINDING_PATH>`. Current intake summary: `<one-line summary from Intake section>`. Suggested persona-frame: `<derived from Domain>`. Anything missing before I draft?"

In structured-input mode, skip the round-trip; proceed directly to Phase 2.

# PHASE 2 — DRAFT

Derive the missing fields and produce the artifact-shape preview.

**Persona-frame derivation.** Map the finding's `Domain` to a suggested frame:

| Domain | Suggested persona-frame |
|---|---|
| `operational` | business analyst |
| `security` | security analyst |
| `testing` | QA lead |
| `methodology` | methodologist |
| `other` | operator-named (default: `domain expert`) |

In interactive mode, present the suggestion: "Persona-frame: `<suggested>`. Accept, override (free text), or skip?" One-keystroke accept or override; skip defaults to `domain expert`. In structured-input mode, `PERSONA_FRAME` is used as-supplied (default: suggestion-from-Domain).

The artifact field is written as `<name>; <descriptive frame>; persona-frame: triage`. Example: `waseric; business analyst; persona-frame: triage`. The `triage` label is fixed across all triage invocations; only the descriptive frame varies.

**Pointer-revalidation policy.** Examine the finding's Intake section `External references` field:

1. **No pointer:** `POINTER_REVALIDATION` is `not-applicable`. No prompt. Triage proceeds.
2. **Pointer present, intake Summary judged rich:** default is `treated-as-static`. The skill shows: "External pointer detected: `<url>`. Intake Summary is detailed; defaulting to treat-as-static. Override to check?" One-keystroke accept or override.
3. **Pointer present, intake Summary judged sparse:** the skill recommends checking: "External pointer detected: `<url>`. Intake Summary is brief; recommend checking the pointer for current state. Check now / treat as static / cancel?"
4. **Operator opts to check:** the skill defers the actual fetch to the invoking agent's capability (WebFetch, browser, or operator-manual). The skill then prompts: "What did you find? `checked-still-current` / `checked-changed` / `fetch-failed`?" The operator answers; the skill records the answer plus any one-line summary in the journal entry's `Pointer revalidation` field.
5. **Operator declines to check:** `POINTER_REVALIDATION` is `treated-as-static`. Journaled.

Iterate per pointer when multiple pointers exist.

**Rich-vs-sparse heuristic.** Operator-judged, not enforced. As a soft rule: a summary is **rich** when it is ≥3 sentences, names the affected components, names the reporter(s), and includes verbatim quotes or snapshot references; **sparse** when it is ≤2 sentences, refers the reader to "see the URL," or contains only a one-line "moderators report X." The heuristic is orientation, not a threshold.

**Reproducibility.** The operator (or invoking agent) supplies `REPRODUCIBILITY`. When `reliably` or `intermittently`, also supply ordered `REPRO_STEPS`. Reproducibility may legitimately be `unknown` — the operator tried and could not determine — with a one-line `TRIAGE_NOTES` explaining what was tried. The skill does not invent reproducibility data; it prompts for the answer.

**Hard-facts discipline.** When gathering `TRIAGE_NOTES`, the skill explicitly encourages recording cause hypotheses as *deferred* or *rejected* (e.g., "hypothesis: LWC version mismatch; deferred to investigation"), not as Triage's authoritative claim. The skill rejects any input to the Triage section that reads as causal analysis ("the cause is X," "the fix is Y"); it surfaces the input as a candidate for deferred-hypothesis recording in `TRIAGE_NOTES` instead.

Show the operator (interactive mode only):

- The proposed Triage section, fully populated.
- The proposed `Triaged` journal entry.
- If `SKIP_INVESTIGATION` is true: the proposed Route section + the proposed `Routed` / `Closed` journal entry.

Pause for one-step confirm/edit. In structured-input mode, proceed without confirmation.

# PHASE 3 — APPLY

Edit the artifact:

1. **Populate the Triage section** of `finding.md`, replacing `<placeholder>` values in the existing Triage section with the gathered fields. Preserve the Intake section byte-for-byte unchanged. All seven Triage fields are written:
   - `Triaged by`: `<TRIAGED_BY>; <PERSONA_FRAME>; persona-frame: triage`
   - `Triage date`: `<TRIAGE_DATE>`
   - `Reproducibility`: `<REPRODUCIBILITY>`
   - `Repro steps (if reproducible)`: ordered list (when reproducibility is `reliably` or `intermittently`); otherwise replace the placeholder ordered-list shell with `not applicable` on a single line.
   - `Scope`: `<SCOPE>`
   - `Domain confirmation`: `<DOMAIN_CONFIRMATION>`
   - `Severity confirmation`: `<SEVERITY_CONFIRMATION>`
   - `Triage notes`: `<TRIAGE_NOTES>` (or `none` if omitted)
2. **Update the status banner** of `finding.md`:
   - `Status: triaged` (default; skip-route may override below).
   - `Severity: <SEVERITY_CONFIRMATION>` — the banner's Severity field is set here for the first time (it was in `<placeholder>` form at intake).
   - `Operational urgency: <OPERATIONAL_URGENCY>` when applicable; otherwise leave the placeholder in place.
   - `Last transition: <TRIAGE_DATE>`.
3. **Append a `Triaged` journal entry** by uncommenting and filling the journal template's `Triaged` skeleton:
   - Header: `## <TRIAGE_DATE> — Triaged: <one-line summary of triage outcome>`.
   - `Triaged by`: same shape as the finding's `Triaged by` field.
   - `Prior status`: `intake`.
   - `New status`: `triaged`.
   - `Reproducibility outcome`: `<REPRODUCIBILITY>`.
   - `Domain/severity changes from intake`: state any deltas, or `none — intake values confirmed`.
   - `Skip-investigation decision (if any)`: state explicitly (`end at triaged` when skip not chosen — named here for journaling honesty).
   - `Pointer revalidation`: one line per external pointer, recording the chosen `POINTER_REVALIDATION` value plus any one-line context.
   - `Notes`: anything else surfaced in triage not captured in the finding's Triage section.
4. **If `SKIP_INVESTIGATION` is true**, also:
   - **Populate the Route section** of `finding.md`, replacing `<placeholder>` values with `ROUTE_DECISION`, `TRIAGED_BY` (with descriptive frame), `TRIAGE_DATE`, `TARGET_SPEC` (when applicable) / `WATCH_CONDITION` (when defer) / `CLOSE_REASON` (when close), and `ROUTE_RATIONALE`.
   - **Update the status banner**: set `Status: routed` when `ROUTE_DECISION` is `spec-amend`, `spec-write`, or `defer`; set `Status: closed` when `ROUTE_DECISION` is `close`. Update `Last transition` to `TRIAGE_DATE`.
   - **Append a second journal entry** — `Routed` or `Closed` — by uncommenting and filling the corresponding journal template skeleton. The two entries are *not* collapsed; both are written, in order, `Triaged` first.
5. **Suggested commit message.** Return to the operator: `find: <short-name> triaged — <one-line summary>` (or `... triaged + routed/closed — ...` when skip-investigation was chosen). The skill does not run `git add` or `git commit`.

# OUTPUT FORMAT

Closing message states, explicitly:

```
Updated: specs/findings/<YYYYMMDD-short-name>/
  - finding.md (status: triaged | routed | closed)
  - journal.md (+1 entry: Triaged | +2 entries: Triaged, Routed | +2 entries: Triaged, Closed)

Triage outcome:
  Reproducibility: <value>
  Scope: <one-line>
  Severity: <value> (operational urgency: <value or n/a>)
  Persona-frame: <descriptive frame> (suggested: <suggestion>; <accepted | overridden>)
  Pointer revalidation: <per-pointer one-liner, or "not applicable">

Skip-investigation: <yes — route subtype <X> | no>

Suggested commit:
  git add specs/findings/<YYYYMMDD-short-name>/
  git commit -m "find: <short-name> triaged[+routed|+closed] — <one-line summary>"
```

# WHAT NOT TO DO

- **Do not open code in the codebase being triaged about.** Reading source to hypothesize about cause is investigation's job. *Running* code to reproduce a signal (joining a server, triggering an action, observing the error) is allowed and often necessary — that is the act of reproduction, not file inspection.
- **Do not rewrite the Intake section.** Triage appends; it does not edit. Corrections to Intake-section content are journaled as superseding entries.
- **Do not invent reproducibility.** If the operator did not try to reproduce, `REPRODUCIBILITY` is `unknown` with a one-line `TRIAGE_NOTES` explaining why. Triage does not fabricate evidence.
- **Do not skip the persona-frame.** Even an operator-named or `domain expert` default frame is recorded. The persona-frame is *descriptive orientation*, not enforcement; omitting it loses the multi-domain signal the schema is designed to preserve.
- **Do not record revalidation decisions silently.** Every external pointer present on the finding gets a `POINTER_REVALIDATION` value journaled — including the default `treated-as-static` and `not-applicable`. The decision is journaled, not omitted.
- **Do not collapse the skip-route transitions into one journal entry.** When `SKIP_INVESTIGATION` is true, the `Triaged` and `Routed` / `Closed` entries are both written, in order. The two status transitions are observably separate per design spec §6 Observability NFR.
- **Do not auto-commit.** The skill produces file edits; the operator decides when to commit. The skill's responsibility ends at returning a suggested commit message.
- **Do not re-triage a non-intake finding.** The state-machine pre-condition guard in Phase 1 rejects this. Use a future re-triage skill (when authored) or edit the finding by hand following the schema field reference.
- **Do not record cause hypotheses as Triage findings.** Cause analysis lives in investigation. Hypotheses surfaced during triage are recorded as *deferred* or *rejected* in `TRIAGE_NOTES`, not as Triage's authoritative claim.
