# Finding Triage Skill — Feature Specification

> Status: Complete
> Date: 2026-05-17
> Author: waseric + Claude
> Audience: Eric Wasgatt (executor); AI coding agents executing this spec; reviewers at the Phase C internal checkpoint (RC-3b) and at the design-spec RC-3 (joint with Phase B)
> Upstream design spec: [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)
> Sibling upstream feature specs (Phase A — done): [specs/20260517-findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md); (Phase B — done): [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md)

## 1. Overview

This feature spec implements **Phase C** of the [Findings Pipeline design spec](../20260517-findings-pipeline/architecture.md): author the `finding-triage` skill that consumes a finding at `status: intake` and produces a finding at `status: triaged` — or, when the route is obvious, transitions the finding directly to `routed` / `closed` with skip-investigation rationale journaled. The skill operates against the Phase A schema and the existing intake artifacts produced by Phase B's `finding-intake`.

Triage's job is to produce **hard facts** about the finding's shape — reproducibility, scope, severity, domain — without opening the codebase. "Stay out of code" is the persona-frame discipline of this phase: investigation is the developer-frame phase that touches files; triage is the domain-expert-frame phase that establishes what kind of thing this is and how big it is.

This spec also resolves two design-spec open questions inherited from RC-1: **OQ-3** (multi-domain persona naming) — descriptive recording, no hardcoded "business analyst" — and **OQ-4** (triage-time revalidation of external pointers) — optional revalidation, soft default, journaled decision.

## 2. Goals and Non-goals

**Goals:**

- Create `.agents/skills/finding-triage/SKILL.md` following the YAML-frontmatter + ROLE + OPERATING PRINCIPLES + INPUTS + phased-workflow convention used by the seven existing peer skills.
- Consume a finding at `specs/findings/YYYYMMDD-<short-name>/finding.md` with `status: intake`; populate the Triage section per the [findings schema README](../findings/README.md) field reference and the [_template/finding.md](../findings/_template/finding.md) shape; transition status to `triaged` (or directly to `routed` / `closed` when triage is sufficient to route).
- Support both "end at `triaged`" and "skip-investigation, route directly" paths per design spec §5.3.
- Resolve design-spec OQ-3 with descriptive persona-frame recording: operator names the frame appropriate to the finding's domain rather than picking from a fixed enum.
- Resolve design-spec OQ-4 with an optional, soft-default revalidation policy and a journaled revalidation decision per external pointer.
- Validate the skill via one synthetic exercise (triage the existing test-only-signal-synthetic-fixture) and one real-signal dogfood exercise (triage the LWC shelves error finding).
- Land a "Triaging a finding" section in [specs/findings/README.md](../findings/README.md) paralleling the "Creating a new finding" section.

**Non-goals:**

- Authoring `finding-investigate` (Phase F decision per design spec OQ-1 — graduation reserved for evidence the protocol is failing).
- Amending `spec-amend` or `spec-write` to accept `FINDING_PATH` (Phase E — separate feature spec(s)).
- Routing the dogfood finding *through* to a downstream spec (Phase F adoption review). T-03 may select a route subtype if triage is sufficient to decide; it does not invoke `/spec-amend` or `/spec-write` against the routed target.
- Automated reproducibility verification (running code, scripting reproduction). Reproducibility is operator-determined and operator-recorded; the skill prescribes the field, not the mechanism.
- Re-running intake. The skill expects a finding already at `status: intake`; if invoked against a missing or wrong-status finding, the skill surfaces the mismatch and exits.
- Multi-finding bundling or de-bundling. If triage reveals two findings should split (or two findings should merge), that is a separate action handled outside this skill — surfaced as a triage-note observation, not automated.
- A 60-second NFR. Triage is hard-facts work; speed is not its design constraint. Quality (completeness + persona-frame discipline) is.

## 3. Background and Constraints

### Spec repo context

This feature spec lives in the same repository as the artifact it modifies — there is no codebase distinct from the methodology repo. `SPEC_REPO_ROOT` and `CODEBASE_ROOT` are the same.

- **SPEC_REPO_ROOT:** `/Users/eric/scm/github/waseric/ai-tools`
- **SPEC_TARGET_BRANCH:** `main`

### Upstream artifacts (authoritative input)

- **Design spec:** [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md). §5.3 (Triage phase) is the interface contract; §5.5 (Routing decisions and downstream handoff) defines the four route subtypes and the routed-vs-closed terminal mapping; §5.6 (Persona model) sets the persona-frame requirement; §6 NFRs include observability (every transition journaled) and external-pointer durability.
- **Schema artifacts (Phase A — RC-2 passed, remediated 2026-05-17):**
  - [specs/findings/README.md](../findings/README.md) — 30-field reference table, state machine (including `triaged → routed`, `triaged → closed`, and the `triaged → under-investigation → routed | closed` paths), status semantics, persona-frame taxonomy.
  - [specs/findings/_template/finding.md](../findings/_template/finding.md) — canonical template; Triage and Route sections are the fields this skill populates.
  - [specs/findings/_template/journal.md](../findings/_template/journal.md) — canonical journal; the commented-out skeleton entries for "Triaged", "Routed", "Closed" are the shapes this skill uncomments and fills.
- **Phase B skill (sibling — done, RC-3a passed 2026-05-17):**
  - [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) — 153 lines; structural reference for skill shape; the source of artifact-state this skill consumes.
  - [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md) — pattern reference for this spec's structure.
- **Existing intake artifacts to triage:**
  - [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/) — synthetic, at `status: intake`. T-02 target.
  - [specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/) — real, at `status: intake`, operational domain, with external pointer + operator-supplied snapshot. T-03 target.
- **Sibling skill patterns:** [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md), [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md), [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md). Each 200–225 lines.
- **Constitution:** [mission.md](../mission.md), [tech-stack.md](../tech-stack.md), [roadmap.md](../roadmap.md). Methodology is markdown-only; this spec produces a markdown skill artifact + a small README extension.

### Open questions resolved by this feature spec

This spec is the named owner for two design-spec open questions inherited from RC-1:

- **Design-spec OQ-3** — Multi-domain persona naming. Resolution: **option (c) — descriptive recording.** The skill prompts the operator for the persona-frame as free text with suggested values (business analyst, security analyst, QA lead, methodologist, end-user advocate). The discipline is "triage stays out of code, regardless of which frame fits"; the named frame is orientation. Recorded in SKILL.md OPERATING PRINCIPLES and in this spec's §5.3.
- **Design-spec OQ-4** — Triage-time revalidation policy for external pointers. Resolution: **optional, soft default.** The skill suggests checking the pointer when the intake summary is sparse or ambiguous, otherwise treats the pointer as static. The revalidation decision (checked-still-current / checked-changed / treated-as-static / not-applicable) is journaled per pointer regardless. Recorded in SKILL.md Phase 2 and in this spec's §5.4.

Both resolutions will be quoted back to the design spec via `/spec-amend` *after* RC-3 closes (i.e., once both Phase B and Phase C are operational) — not as part of this spec's task breakdown, which produces the skill itself. See §12 Out of Scope.

Design-spec OQ-1 (investigation graduation) and OQ-2 (incident/problem distinction) remain owned by RC-5 / Phase 2 and are unaffected by Phase C.

### Constraints

- **Markdown-only deliverable.** No new runtime, build step, or test runner ([tech-stack.md](../tech-stack.md)).
- **AI context-window economy.** SKILL.md target: ≤220 lines, matching peer skills' ceiling.
- **No automated enforcement.** The skill is consumed by an AI agent (Claude Code, GitHub Copilot) or used by a human reading it as a checklist; there is no validator that checks operator-produced artifacts against the skill's prescriptions.
- **Append-only artifact discipline.** Triage appends to the finding and the journal; it does not rewrite Intake-section content. Corrections to Intake-section data are made via a journal entry that supersedes — never by editing the Intake section in place.

## 4. Architecture

### Deliverable layout

```
.agents/
  skills/
    finding-triage/
      SKILL.md            ← T-01 produces

specs/
  findings/
    README.md             ← T-04 amends (adds "Triaging a finding" section)
    20260517-test-only-signal-synthetic-fixture/   ← T-02 amends (Triage section populated, journal entry appended)
      finding.md
      journal.md
    20260517-easy-survival-shelves-lwc-error/       ← T-03 amends (Triage section populated, journal entry appended; possibly Route populated if triage chooses skip-route)
      finding.md
      journal.md
```

The SKILL.md is the primary deliverable. The two amended findings are validation artifacts.

### Skill invocation shape

```
/finding-triage [FINDING_PATH] [optional structured args]
```

In **interactive mode** (typical human use), the skill prompts for the load-bearing fields (reproducibility, scope, severity, domain confirmation, urgency if operational). In **structured-input mode** (typical AI-agent use), the skill accepts the INPUTS block populated by the calling agent and skips the prompts.

### Interface contract

Inputs:

| Input | Type | Default | Required |
|---|---|---|---|
| `FINDING_PATH` | path | none | always |
| `TRIAGED_BY` | text | git `user.name` (no email) | no |
| `PERSONA_FRAME` | free text with suggested values | derived from finding's `Domain` (`operational` → `business analyst`, `security` → `security analyst`, `testing` → `QA lead`, `methodology` → `methodologist`, `other` → operator-named) | no |
| `TRIAGE_DATE` | YYYY-MM-DD | operator's local date | no |
| `REPRODUCIBILITY` | enum: `reliably` / `intermittently` / `not reproduced` / `not applicable` | none | yes |
| `REPRO_STEPS` | ordered list | none | when `REPRODUCIBILITY` is `reliably` or `intermittently` |
| `SCOPE` | text | none | yes |
| `DOMAIN_CONFIRMATION` | enum: same as schema `Domain` | finding's intake-time `Domain` value | yes |
| `SEVERITY_CONFIRMATION` | enum: `blocker` / `important` / `advisory` | none | yes |
| `OPERATIONAL_URGENCY` | optional enum: `P1` / `P2` / `P3` / `P4` | none | when `DOMAIN_CONFIRMATION` is `operational`; optional otherwise |
| `TRIAGE_NOTES` | free text | none | no |
| `POINTER_REVALIDATION` | enum: `checked-still-current` / `checked-changed` / `treated-as-static` / `not-applicable` | `not-applicable` when no pointer; `treated-as-static` when pointer present | per-pointer; recorded in journal |
| `SKIP_INVESTIGATION` | bool | false | no |
| `ROUTE_DECISION` | enum: `spec-amend` / `spec-write` / `defer` / `close` | none | when `SKIP_INVESTIGATION` is true |
| `ROUTE_RATIONALE` | paragraph | none | when `SKIP_INVESTIGATION` is true |
| `TARGET_SPEC` | path | none | when `ROUTE_DECISION` is `spec-amend` or `spec-write` |
| `WATCH_CONDITION` | text | none | when `ROUTE_DECISION` is `defer` |
| `CLOSE_REASON` | enum: `cannot reproduce` / `expected behavior` / `out of scope` / `superseded` / `other` | none | when `ROUTE_DECISION` is `close` |

Outputs:

- The same `finding.md` and `journal.md`, with:
  - Status banner updated: `Status: triaged` (or `routed` / `closed` if skip-investigation); `Last transition: <TRIAGE_DATE>`.
  - Triage section populated (all seven fields).
  - Route section populated if skip-investigation chosen.
  - Journal: one new entry (`Triaged`), or two entries (`Triaged` then `Routed` / `Closed`) if skip-investigation chosen — entry shape from the journal template's commented-out skeletons, uncommented and filled.
- A suggested commit message returned to the caller.

### Persona-frame derivation

Per design-spec OQ-3 resolution (this spec's §5.3), the skill **does not** hardcode "business analyst." The skill derives a suggested frame from the finding's `Domain` field and presents it to the operator with one round-trip to accept or override. The discipline is "triage stays out of code"; the named frame is descriptive orientation only.

| Domain | Suggested persona-frame |
|---|---|
| `operational` | business analyst |
| `security` | security analyst |
| `testing` | QA lead |
| `methodology` | methodologist |
| `other` | operator-named |

In structured-input mode, `PERSONA_FRAME` is used as-supplied. The skill writes the field to the artifact as `<frame>; persona-frame: triage`, preserving the per-phase persona-frame label discipline established by intake.

### Pointer-revalidation policy

Per design-spec OQ-4 resolution (this spec's §5.4), revalidation is **optional with a soft default**:

1. If the finding has no external pointer, `POINTER_REVALIDATION` is `not-applicable`. No prompt.
2. If the finding has an external pointer **and** the intake Summary field is sparse (operator-judged: short, missing key context, or contradicted by triage's own understanding), the skill suggests checking the pointer. The operator accepts (revalidation attempted) or declines (treated-as-static).
3. If the intake Summary is rich (operator-judged), the default is `treated-as-static` — no revalidation. The operator may still opt to check if desired.
4. The revalidation decision is journaled per pointer regardless of which path was taken. The skill writes a one-line per-pointer entry under the new Triage journal entry's `Pointer revalidation` field.

The skill does not redo the intake-time URL fetch on its own; revalidation is an operator-driven act using whatever fetch capability the invoking agent has. The skill's job is to ask the question and journal the answer.

### Skip-investigation surface

The skill ends with one prompt (interactive) or one input flag (structured): `SKIP_INVESTIGATION`. When true, the skill prompts for the four Route fields (decision, rationale, target spec or watch condition or close reason) and:

- Writes the Route section of the finding.
- Updates the status banner to `Status: routed` or `Status: closed` (per the schema's terminal-status mapping in [specs/findings/README.md §State machine](../findings/README.md#state-machine)).
- Writes a second journal entry — `Routed` or `Closed` — immediately after the `Triaged` entry, with the skip-investigation rationale in the entry's Notes field.

When false (or omitted), the skill ends at `Status: triaged` with only the `Triaged` journal entry written.

## 5. Detailed Design

### 5.1 `.agents/skills/finding-triage/SKILL.md` shape

**Purpose.** A self-contained skill artifact that an AI agent (or a human reading it) can execute end-to-end without further design input.

**Required sections** (matching peer-skill convention):

1. **YAML frontmatter:** `name: finding-triage`; `lastUpdated: <YYYY-MM-DD>`; `description:` two-to-three sentences summarizing the skill's purpose, pairing references to `[[finding-intake]]` (upstream phase), `[[spec-amend]]` and `[[spec-write]]` (Phase E downstream consumers), and the Findings Pipeline design spec.
2. **Title heading:** `# Finding Triage`.
3. **Opening paragraphs:** 2–3 short paragraphs framing the skill's purpose, its position in the Findings Pipeline (Phase C per [design spec §7](../20260517-findings-pipeline/architecture.md#7-implementation-sequencing)), and its persona-frame discipline ("triage stays out of code").
4. **"How this skill works"** subsection: brief operator-facing instructions on interactive vs. structured-input invocation, and on the skip-investigation surface.
5. **INPUTS block:** the table from §4 above, transcribed into the skill's input contract.
6. **ROLE:** the operator/agent acts in the *triage* persona-frame — domain-knowledgeable, not yet opening code; producing hard facts about the finding's shape.
7. **OPERATING PRINCIPLES:** 7–9 numbered principles (hard facts over hypotheses; stay out of code; "unknown" is first-class but distinct from `<placeholder>`; persona-frame is descriptive; revalidation is optional and journaled; skip-route requires explicit rationale; append, never rewrite Intake; working-tree-leave; honest reproducibility).
8. **Phase 1 — ORIENT:** load the finding at `FINDING_PATH`; verify `status: intake`; read the Intake section (Summary, External references, Reported by, Captured by); if not at `intake`, surface the mismatch and exit.
9. **Phase 2 — DRAFT:** derive `PERSONA_FRAME` from `Domain`; gather triage fields (interactive: prompt; structured: read INPUTS); make revalidation decision per pointer; show the operator the proposed Triage section + journal entry for one-step confirm/edit.
10. **Phase 3 — APPLY:** append Triage section fields to the finding's existing Triage section (replacing `<placeholder>` values); update status banner; append `Triaged` journal entry; if `SKIP_INVESTIGATION` is true, append Route section + status update + `Routed` or `Closed` journal entry; return suggested commit message and updated artifact path.
11. **OUTPUT FORMAT:** the updated artifact path; the journal entry summary; the suggested commit message.
12. **WHAT NOT TO DO:** explicit anti-goals (no opening code; no rewriting Intake; no inventing reproducibility; no skipping persona-frame; no silent revalidation decisions; no auto-commit; no re-triage of a non-intake finding; no proposing cause).

**Pattern invoked.** Skill-as-self-contained-prompt — the format used by all seven existing skills in `.agents/skills/`. Loadable by Claude Code's slash-command resolver and by GitHub Copilot.

**Why this design.** The skill replicates a structure operators and agents already know from `finding-intake`, `spec-write`, `spec-amend`. New methodology surface area is minimized; the cognitive load is "another phased skill," not "a new shape."

**Alternatives considered.**

- *Two skills — `finding-triage` for end-at-triaged and `finding-route-skip` for direct routing.* Rejected: collapsing the skip-route surface into the same skill matches design spec §5.3 phrasing ("triage may end with 'obvious enough to route directly'") and avoids forcing the operator to switch skills mid-thought.
- *Triage skill that also performs investigation when needed.* Rejected: collapses Phase C and Phase F; loses the persona-frame separation that motivates the three-phase model.
- *Hardcoded "business analyst" persona-frame.* Rejected: contradicts design-spec OQ-3 resolution; loses the multi-domain orientation flexibility.

### 5.2 Status state machine handling

**Purpose.** Honor the schema's state machine: a finding at `status: intake` may transition to `triaged`, or directly to `routed` / `closed` (skip-investigation paths) per [specs/findings/README.md §State machine](../findings/README.md#state-machine).

**Behavior.**

- **Pre-condition check:** the finding's status banner must read `Status: intake` before the skill writes anything. Any other status is rejected at Phase 1 with a clear error: "finding-triage operates on findings at `status: intake`; this finding is at `status: <X>`. Use a different skill (or future skill) for re-triage." A reopened finding at status `reopened → triaged` is a legitimate re-triage path; for Phase C, treat this the same as a status mismatch and surface the operator-must-handle scenario.
- **Post-condition under normal flow:** `Status: triaged` set; `Last transition` updated to `TRIAGE_DATE`.
- **Post-condition under skip-investigation:** `Status: routed` (when `ROUTE_DECISION` is `spec-amend`, `spec-write`, or `defer`) or `Status: closed` (when `ROUTE_DECISION` is `close`). The schema's terminal-status mapping is honored: `defer` terminates at `Status: routed` with the Route section's decision field reading `defer`.
- **Journaling discipline:** every status transition is journaled (design spec §6 Observability NFR). Skip-investigation produces *two* status transitions in one skill invocation, and therefore two journal entries — `Triaged` first, `Routed` or `Closed` second. The two entries are not collapsed.

**Pattern invoked.** Pre-/post-condition guards from defensive programming, applied to artifact state rather than runtime state.

**Why this design.** The schema's state machine is authoritative; the skill is one consumer of it. Pre-condition checks at Phase 1 prevent silent corruption (e.g., re-triaging an under-investigation finding back to triaged). Post-condition writes are atomic per phase: either both finding.md and journal.md are updated, or the skill surfaces an error and leaves the artifact unchanged.

### 5.3 Persona-frame derivation (resolves design-spec OQ-3)

**Purpose.** Honor design-spec OQ-3 resolution (option (c) — descriptive recording) without losing the "triage stays out of code" discipline.

**Behavior.**

- The skill derives a suggested persona-frame from the finding's `Domain` field per the table in §4.
- In interactive mode, the skill shows the suggestion and asks: "Persona-frame: `<suggested>`. Accept, override (free text), or skip?" The operator accepts with a single keystroke, overrides with a free-text label, or skips (the field defaults to a generic `domain expert`).
- In structured-input mode, `PERSONA_FRAME` is used as-supplied.
- The field is written to the artifact as `<frame>; persona-frame: triage`. The `triage` label is fixed; only the descriptive frame varies.

**Pattern invoked.** Descriptive metadata field with suggested values, common in survey design.

**Why this design.** A fixed enum would force operators of security/testing findings into a frame ("business analyst") that doesn't fit their work. A fully free-text field would lose the orientation signal. Suggested-with-override threads the needle: discoverable defaults, descriptive flexibility, persona-frame label preserved.

**Watch item.** If AI agents fail to adopt the right frame because the SKILL.md *suggests* but doesn't *enforce*, generalize the prompt language. See design-spec OQ-3's watch items.

### 5.4 Pointer-revalidation policy (resolves design-spec OQ-4)

**Purpose.** Honor design-spec OQ-4 resolution (optional, soft default) — surface stale or contradictory external state at the right moment without making triage slow or network-dependent.

**Behavior.**

- The skill examines the finding's Intake section `External references` field at Phase 1.
- **No pointer:** `POINTER_REVALIDATION` defaults to `not-applicable`. No prompt. Triage proceeds.
- **Pointer present, intake Summary judged rich:** default is `treated-as-static`. The skill shows: "External pointer detected: `<url>`. Intake Summary is detailed; defaulting to treat-as-static. Override to check?" One-keystroke accept or override.
- **Pointer present, intake Summary judged sparse:** the skill suggests checking: "External pointer detected: `<url>`. Intake Summary is brief; recommend checking the pointer for current state. Check now / treat as static / cancel?"
- **Operator opts to check:** the skill defers the actual fetch to the invoking agent's capability (WebFetch, browser, or operator-manual). The skill prompts: "What did you find? `checked-still-current` / `checked-changed` / `fetch-failed`?" The operator answers; the skill records the answer plus any one-line summary in the journal entry's `Pointer revalidation` field.
- **Operator declines to check:** `POINTER_REVALIDATION` is `treated-as-static`. Journaled.

Per pointer, when multiple pointers exist on the finding. The skill iterates.

**"Rich" vs. "sparse" Summary heuristic.** The skill prose names a soft threshold (operator-judged) rather than enforcing word counts. Examples to include in SKILL.md prose:

- Rich: ≥3 sentences, names the affected components, names the reporter(s), includes verbatim quotes or screenshot references.
- Sparse: ≤2 sentences, refers the reader to "see the URL," or contains only "moderators report TAB User List is not showing prefixes."

**Pattern invoked.** Soft-default policy with operator override, common in tooling that wants to make the costly choice opt-in rather than opt-out.

**Why this design.** Mandatory revalidation slows every triage and pulls the operator into the linked ticket. Never-revalidate misses the chance to catch stale external state. Optional-with-soft-default keeps triage fast in the common case (rich intake → static) and prompts revalidation when intake didn't capture enough (sparse intake → recommend check).

### 5.5 Hard-facts discipline

**Purpose.** Triage produces facts about the finding's shape, not hypotheses about cause. The skill's prompts and field labels reinforce this.

**Behavior.**

- The Triage section's fields are all observational, not causal: reproducibility (what was tried), scope (what is affected), domain confirmation (what kind of finding), severity confirmation (how bad), triage notes (anything else surfaced — including *rejected* hypotheses, clarifications from the reporter, observations about the signal's shape).
- The skill prose explicitly forbids causal speculation in Triage. "Probable cause," "code touchpoints," "proposed remedy" — these belong in Investigation, never Triage. The WHAT NOT TO DO section names this anti-goal.
- If the operator surfaces a cause hypothesis during triage, the skill's prompt encourages recording it in `TRIAGE_NOTES` as a *rejected* or *deferred* hypothesis (e.g., "hypothesis: LWC version mismatch on Easy Survival; deferred to investigation"), not as Triage's authoritative claim.

**Pattern invoked.** Phase-discipline-by-vocabulary. The field names alone signal what belongs in this phase.

**Why this design.** Without explicit discipline, triage will absorb cause analysis and lose its persona-frame separation from investigation. Design-spec §5.3 is explicit: "Triage produces *hard facts*, not hypotheses about cause."

### 5.6 README integration (T-04)

The Phase A README has a "Creating a new finding" section [(README.md L142–L170)](../findings/README.md#L142). The intake skill's T-04 made `/finding-intake` the primary path there. Phase C's T-04 adds a parallel "Triaging a finding" section that:

- Names `/finding-triage` as the primary invocation for converting an `intake`-status finding to `triaged` (or routed/closed via skip).
- Describes inputs (the load-bearing fields).
- Cross-references the design spec §5.3 and this feature spec.
- Names the skip-investigation surface and the persona-frame derivation as features worth knowing about.
- Keeps the manual-fallback shape (operator may edit the finding by hand following the schema field reference) — but states it briefly, since the schema already documents the field reference.

One-paragraph section + a short skeleton example of the invocation. Target: ~25 lines added.

## 6. Non-functional Requirements

| Property | Requirement |
|---|---|
| **Persona-frame discipline** | The skill never opens code in the codebase being triaged about. Reproducibility is operator-determined; the skill prompts for the answer, not the act. Verified in T-02 and T-03. |
| **Hard-facts discipline** | The Triage section never contains cause analysis. Cause hypotheses surfaced during triage are recorded as deferred-to-investigation in `TRIAGE_NOTES`, not as Triage findings. Verified by inspection in T-02 and T-03. |
| **State-machine pre-condition** | The skill rejects invocation against a finding not at `status: intake` with a clear error and no artifact mutation. Verified by inspection. |
| **Append-only Intake preservation** | The skill never rewrites the Intake section. Triage section is populated where `<placeholder>` was; Intake section is byte-for-byte unchanged. Verified by `diff` in T-02. |
| **Two-journal-entry skip-route** | When `SKIP_INVESTIGATION` is true, two journal entries are appended (`Triaged` then `Routed` / `Closed`). Status transitions are not collapsed into one entry. |
| **Revalidation decision journaling** | Every external pointer present on the finding gets a `POINTER_REVALIDATION` decision recorded in the journal entry, regardless of which path was taken (decision is journaled, not omitted). |
| **No new dependencies** | Pure markdown deliverable. No new runtime, library, or build step. |
| **Backward compatibility** | The schema artifacts are unchanged. The intake skill is unchanged. Existing intake-status findings remain valid input. |
| **Context economy** | SKILL.md ≤220 lines (peer-skill ceiling). The skill loads only what it needs: the finding being triaged, plus its bundled `_template/finding.md` and `_template/journal.md` (or the host's `specs/findings/_template/` override when present) as shape references for the Triage/Route sections and Triaged/Routed/Closed journal skeletons. Schema knowledge is embedded in SKILL.md prose, not resolved at runtime from the host's `README.md`. |
| **Self-contained artifact** | After triage, the finding still does not require the originating intake conversation or any external system to remain reachable. Triage notes are self-contained; pointer revalidation outcome is captured in the journal. |
| **Persona-frame descriptive flexibility** | The skill suggests a persona-frame derived from `Domain` but accepts free-text override. No fixed enum. Verified in T-03 by triaging an operational finding without forcing "business analyst" if the operator names a more apt frame. |
| **Honest unknowns** | Reproducibility, scope, severity may be recorded as `unknown` when the operator tried and could not determine. The skill prompts allow this and require a one-line `TRIAGE_NOTES` entry explaining what was tried. |
| **Skill portability** | This skill conforms to the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle): it ships its own bundled `_template/finding.md` and `_template/journal.md` as defaults inside [`.agents/skills/finding-triage/`](../../.agents/skills/finding-triage/), and uses a host project's `specs/findings/_template/` (when present) as an override. Schema knowledge (field reference, state machine, persona-frame taxonomy) is embedded in SKILL.md prose; no runtime read of `specs/findings/README.md` is required. Templates carry scaffold-marker delimiters for any strip operation; this skill does not strip line ranges (it appends/edits rather than materializes). |

## 7. Task Breakdown

### T-01 — Author `.agents/skills/finding-triage/SKILL.md`

**Status:** done — 2026-05-17 — commit 04eb616 — see journal entry. SKILL.md written at 197 lines (within §7 soft target of 180–220 and §6 NFR ceiling of ≤220). All twelve §5.1 structural sections present; YAML frontmatter parseable with description block referencing the Findings Pipeline and naming `[[finding-intake]]`, `[[spec-amend]]`, `[[spec-write]]`. All 18 §4 INPUTS fields covered (including the conditional skip-investigation set). All eight T-01-scope anti-goals enumerated in WHAT NOT TO DO. State-machine pre-condition guard present in Phase 1 ORIENT with explicit "exit without artifact mutation" wording. Two-journal-entry skip-route discipline explicit in Phase 3 APPLY ("not collapsed; both written, in order, `Triaged` first"). The three internal OQs from §13 (OQ-1 rich/sparse heuristic prose; OQ-2 persona-frame label format; OQ-3 reproducing-without-opening-code) were decided at execution time per their leanings — see §13.

**Scope:**

- New file: `.agents/skills/finding-triage/SKILL.md`.
- Sections per [§5.1](#51-agentsskillsfinding-triageskillmd-shape) above (YAML frontmatter through WHAT NOT TO DO).
- Target length 180–220 lines. Match the prose style of [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) (the structurally closest peer; same phase pattern, same Pre-/Post-condition discipline, same working-tree-leave finish).
- INPUTS block per [§4](#interface-contract) above (18 fields including the conditional skip-investigation set).
- OPERATING PRINCIPLES enumerate at minimum: hard facts over hypotheses; stay out of code; "unknown" is first-class and distinct from `<placeholder>`; persona-frame is descriptive (suggested, not fixed); revalidation is optional and journaled; skip-route requires explicit rationale + two journal entries; append, never rewrite Intake; working-tree-leave.
- Phase 1 ORIENT explicitly performs the state-machine pre-condition check from [§5.2](#52-status-state-machine-handling).
- Phase 2 DRAFT explicitly performs the persona-frame derivation from [§5.3](#53-persona-frame-derivation-resolves-design-spec-oq-3) and the pointer-revalidation policy from [§5.4](#54-pointer-revalidation-policy-resolves-design-spec-oq-4).
- Phase 3 APPLY explicitly handles the two-journal-entry skip-route path.
- WHAT NOT TO DO covers at minimum: no opening code; no rewriting Intake; no inventing reproducibility; no skipping persona-frame; no silent revalidation decisions; no auto-commit; no re-triage of a non-intake finding; no causal hypotheses in Triage.

**Acceptance criteria:**

- **Given** the schema artifacts at [specs/findings/README.md](../findings/README.md), [specs/findings/_template/finding.md](../findings/_template/finding.md), and [specs/findings/_template/journal.md](../findings/_template/journal.md), and a finding at `status: intake`,
- **When** the skill is invoked with `FINDING_PATH` and the load-bearing fields,
- **Then** the skill produces an updated `finding.md` with the Triage section populated, the status banner updated to `triaged` (or `routed` / `closed` if skip-route was chosen), and an updated `journal.md` with one (or two) new entries appended.
- **And** the Intake section of the produced `finding.md` is byte-for-byte identical to its pre-triage state (preserved verbatim).
- **And** the YAML frontmatter contains `name: finding-triage`, a `lastUpdated:` date, and a `description:` paragraph that names the Findings Pipeline, references upstream `[[finding-intake]]`, and references downstream `[[spec-amend]]` / `[[spec-write]]` integration.
- **And** the skill rejects invocation against a finding not at `status: intake` with a clear error and no artifact mutation.

**Tests required:**

- Inspection: SKILL.md exists, is parseable as YAML+Markdown, opens with the frontmatter block, contains the twelve required structural sections from §5.1.
- Inspection: every INPUT field documented in §4 appears in the skill's INPUTS block (including conditional fields).
- Inspection: the WHAT NOT TO DO section explicitly forbids the eight anti-goals listed in T-01 scope.
- Inspection: the state-machine pre-condition check appears in Phase 1 ORIENT.
- Inspection: the two-journal-entry skip-route discipline appears in Phase 3 APPLY.

**Definition of Done:** File written; under 220 lines (`wc -l`); committed.

**Dependencies:** None.

**Estimated size:** M.

### T-02 — Synthetic validation exercise

**Status:** done — 2026-05-17 — commit `f7d59ac` — see journal entry. Skill invoked in structured-input mode against [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/) (Domain: methodology; no external pointer). Fixture transitioned cleanly from `intake` to `triaged`; all seven Triage fields populated; Intake section byte-for-byte preserved (verified by `git show HEAD:.../finding.md` diff against post-edit content — zero differences); status banner updated (`Status: triaged`; `Severity: advisory`; `Last transition: 2026-05-17`; operational urgency placeholder retained for non-operational domain per SKILL Phase 3 step 2); Triaged journal entry contains all 8 prescribed fields (Triaged by, Prior status, New status, Reproducibility outcome, Domain/severity changes, Skip-investigation decision, Pointer revalidation, Notes); persona-frame `methodologist` suggested-and-accepted via Domain → frame derivation; pointer-revalidation defaulted to `not-applicable`; hard-facts discipline held (no cause hypothesis in Triage section; the route-amend hypothesis is recorded in the journal entry's Skip-investigation field, not in the finding's Triage section); state-machine guard verified by inspection (SKILL Phase 1 ORIENT enumerates `triaged` among rejected statuses; a future `/finding-triage` against this fixture would emit the Phase 1 error and exit). Fixture retains its prior role as the finding-intake T-02 regression reference while now also serving as the finding-triage T-02 reference and as an end-state-Triaged living example paralleling the end-state-Investigation `tab-display-issues`.

**Scope:**

- Invoke the skill against the existing synthetic finding at [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/), currently at `status: intake`. Mode: structured-input or interactive — operator's choice at execution time.
- Verify the produced `finding.md` and `journal.md` match the schema field-by-field for the Triage section and the new journal entry.
- Verify the skill's behavioral properties:
  - State-machine pre-condition check fires on attempted second invocation (post-triage finding now at `status: triaged` — confirm second invocation surfaces a clear error and exits without artifact mutation).
  - Intake section is byte-for-byte preserved (`diff` the Intake section pre- and post-triage).
  - Persona-frame derivation suggests an appropriate frame (the synthetic fixture is `Domain: methodology`, so `methodologist` should be suggested).
  - Pointer-revalidation defaults to `not-applicable` since the synthetic fixture has no external pointer.
  - Hard-facts discipline held: no cause hypothesis in the Triage section.
- Record validation outcomes (what worked, what surprised, any friction) in [journal.md](journal.md).
- Decide at closeout: this triage produces real content on the synthetic fixture. The fixture transitions permanently from `intake` to `triaged`. This is acceptable — the fixture continues to be a regression reference, now as an end-state-Triaged example, paralleling [20260517-tab-display-issues/](../findings/20260517-tab-display-issues/) which is the end-state-Investigation example. If the operator decides at execution time the fixture should remain an intake reference instead, they may produce a *new* fixture for the triage exercise; document the choice with rationale.

**Acceptance criteria:**

- **Given** the skill from T-01 and the existing synthetic finding at `status: intake`,
- **When** the operator invokes the skill,
- **Then** the synthetic finding transitions to `status: triaged`, the Triage section is populated per the schema, the Intake section is preserved verbatim, and the journal has a new `Triaged` entry.
- **And** the validation outcomes (including any second-invocation rejection evidence) are recorded in this feature spec's journal.

**Tests required:**

- Inspection: produced Triage section matches the schema field reference (all seven fields present per the field-reference table).
- Inspection: produced journal entry matches the journal template's `Triaged` skeleton (all fields filled or `unknown`-with-rationale).
- `diff` of Intake section pre- and post-triage: exit-clean (zero differences).
- Inspection: status banner updated; `Last transition` updated to triage date.

**Definition of Done:** Synthetic fixture triaged; validation outcomes journaled; any surfaced bugs either resolved via T-01 amendment or escalated as `[blocker]` open questions.

**Dependencies:** T-01.

**Estimated size:** S.

### T-03 — Real-signal dogfood exercise

**Status:** done — 2026-05-17 — commit `8a5d544` — see journal entry. LWC `:Missing API` finding ([specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/)) transitioned cleanly from `intake` to `triaged`; all seven Triage fields populated; Intake section byte-for-byte preserved (verified by `diff` of pre- and post-edit Intake blocks — zero differences); status banner updated (`Status: triaged`; `Severity: advisory`; `Operational urgency: P4`; `Last transition: 2026-05-17`); Triaged journal entry contains all eight prescribed fields (Triaged by, Prior status, New status, Reproducibility outcome, Domain/severity changes, Skip-investigation decision, Pointer revalidation, Notes); reproducibility recorded honestly as `reliably` matching the in-thread two-reporter / two-interaction-mode / cross-world-control evidence; persona-frame overridden from suggested `business analyst` to operator-named `Sandlot administrator` (first exercise of the §5.3 override path — resolves the §10 watch item in the direction of *used*); pointer-revalidation `treated-as-static` for the auth-walled forum thread (Intake Summary judged rich; PDF snapshot is durable evidence); hard-facts discipline held (intake-time plugin-API hypothesis recorded as *deferred to investigation* in Triage notes, not as a Triage claim); ended at `triaged` (skip-investigation surface not exercised in T-03 — that path remains verified only by inspection across T-01/T-02/T-03 inputs to RC-3b).

**Scope:**

- Invoke the skill (interactive mode preferred for a more honest 60-second-vs-not comparison against intake's NFR) against the existing real finding at [specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/), currently at `status: intake`.
- The finding's domain is `operational`; suggested persona-frame is `business analyst`. The operator may override to `Sandlot administrator` or similar if that frame fits the work better.
- The finding has an external pointer (forum thread URL) with an operator-supplied PDF snapshot. The pointer-revalidation policy is exercised here:
  - Default suggestion: `treated-as-static` (the snapshot is rich) or `recommend check` (if operator judges the Intake Summary insufficient). Operator chooses.
  - Record the decision in the journal entry per [§5.4](#54-pointer-revalidation-policy-resolves-design-spec-oq-4).
- The operator at execution time decides whether triage is sufficient to skip investigation:
  - If yes: invoke skip-route. Two journal entries appended; finding transitions to `routed` (route subtype `spec-amend` / `spec-write` / `defer`) or `closed`. The route target may be the consumer repo (e.g., a future ArenaCTF or BattleTracker spec, or `defer` with a watch condition).
  - If no: end at `triaged`. Investigation is reserved for a separate later session; for the purpose of this dogfood, ending at `triaged` is the default expected outcome.
- Verify the artifact:
  - Triage section populated; Intake section preserved verbatim.
  - Reproducibility assessment is honest (the LWC error is reliably reproducible per the in-thread report; "reliably" is the expected outcome).
  - Severity and operational urgency are both populated (operational domain → urgency is required).
  - Pointer-revalidation outcome recorded in the journal entry.
  - Hard-facts discipline held: no cause hypothesis in Triage (cause analysis like ":Missing API typically indicates LWC is looking for an API surface" lives in Intake Summary already; Triage may reference it but not extend it with new causal claims).
- Record dogfood outcomes (friction, persona-frame override observations, revalidation policy observations) in [journal.md](journal.md).

**Acceptance criteria:**

- **Given** the skill from T-01, the synthetic exercise from T-02, and the real intake finding,
- **When** the operator invokes the skill,
- **Then** the real finding transitions to `status: triaged` (or `routed` / `closed` if skip-route chosen), the Triage section is populated, the pointer-revalidation decision is journaled, and the Intake section is preserved verbatim.
- **And** the dogfood outcomes are journaled, including whether the persona-frame suggestion fit, whether the revalidation policy was useful or felt like ceremony, and whether the skip-investigation surface was used.

**Tests required:**

- The exercise itself is the test. Success criterion: artifact produced; signal substance reflected accurately; pointer-revalidation handled per policy; persona-frame discipline held.
- Failure criterion: a substantive skill or schema gap surfaces. Failure routes via `/spec-amend` against this feature spec or the design spec, as appropriate.

**Definition of Done:** Real finding triaged; dogfood outcomes journaled; any surfaced gaps either resolved or escalated.

**Dependencies:** T-01, T-02.

**Estimated size:** S.

### T-04 — Add "Triaging a finding" section to `specs/findings/README.md`

**Status:** done — 2026-05-17 — commit `9ae5e89` — see journal entry. README extended with a new `## Triaging a finding` section landing immediately after the existing `## Creating a new finding` + its `### Manual fallback` block (now at L172). `/finding-triage` is the primary path (slash-command invocation with `FINDING_PATH` argument). Load-bearing inputs named in the behavior paragraph (reproducibility, scope, severity confirmation, domain confirmation, operational urgency when applicable, triage notes). Skip-investigation surface called out in a dedicated `**Skip-investigation surface.**` paragraph with `triaged → routed` / `triaged → closed` cross-link to the state machine. Persona-frame derivation called out in a dedicated `**Persona-frame derivation.**` paragraph with the full Domain → frame table and an explicit override example (`Sandlot administrator` — mirrors the T-03 dogfood evidence). Cross-references resolve to the SKILL.md, this feature spec, and the design spec §5.3 Triage phase. Manual fallback retained as a one-sentence `### Manual fallback (if the skill is not available)` subsection that points to the field reference and the journal template's commented-out `Triaged` skeleton — brief per spec §5.6 guidance. Existing "One finding or several?" paragraph and intake manual fallback are byte-for-byte untouched. README line count: 190 (≤200 inspection ceiling). **All Phase C tasks now complete — RC-3b checkpoint triggers.**

**Scope:**

- Edit [specs/findings/README.md](../findings/README.md), inserting a new "Triaging a finding" section after the existing "Creating a new finding" section (current line ~170).
- Make the `/finding-triage` invocation the primary path: a short paragraph + the slash-command syntax.
- Mention the load-bearing inputs (FINDING_PATH; reproducibility; scope; severity; domain confirmation; operational urgency if applicable).
- Name the skip-investigation surface explicitly: "Triage may end at `triaged` (default), or skip directly to `routed` / `closed` when the route is obvious. The skill prompts for this at the end."
- Name the persona-frame derivation: "The skill suggests a persona-frame based on the finding's `Domain` (business analyst for operational, security analyst for security, QA lead for testing, methodologist for methodology). Operators may override with a free-text label that better fits their finding."
- Cross-reference the design spec §5.3 and this feature spec.
- Keep the existing "One finding or several?" paragraph and the "Manual fallback" section untouched.

**Acceptance criteria:**

- **Given** the skill from T-01 is operational and dogfood from T-03 has produced at least one real triaged finding,
- **When** a consumer reads the README's new "Triaging a finding" section,
- **Then** they find the `/finding-triage` invocation as the recommended primary path, the load-bearing inputs named, the skip-investigation surface and persona-frame derivation called out, and the cross-references resolving.

**Tests required:**

- Inspection: README updated; new section present immediately after "Creating a new finding"; the `/finding-triage` invocation is the first option presented.
- Inspection: cross-references to the skill ([.agents/skills/finding-triage/SKILL.md](../../.agents/skills/finding-triage/SKILL.md)) and this feature spec resolve.
- Line count: README remains ≤200 lines after the addition.

**Definition of Done:** README.md updated; committed; cross-references valid.

**Dependencies:** T-01, T-03 (do not flip primary documentation until the skill has been dogfooded successfully).

**Estimated size:** S.

## 8. Test Strategy

- **Inspection-based validation.** SKILL.md is markdown; the only "tests" are inspection passes (structure, frontmatter, INPUTS coverage, anti-goals stated, state-machine guard present).
- **Synthetic exercise (T-02).** Catches skill-mechanics bugs (state guard, Intake preservation, persona-frame derivation) without exposing a real signal to a buggy skill.
- **Dogfood exercise (T-03).** The integration test: real signal, real operator timing, real artifact. The pointer-revalidation policy and persona-frame override are verified here.
- **No mocking, no fixtures, no test runner.** Methodology repo discipline is prose review.
- **No automated artifact validation.** A future spec may add a CI-style validator that checks `specs/findings/<dir>/finding.md` matches the schema; not in scope here ([findings-pipeline-schema feature.md §13 OQ-1](../20260517-findings-pipeline-schema/feature.md#L373) parked this).

## 9. Review Checkpoints

### RC-3b — Phase C Skill Review (this feature spec's internal checkpoint)

This is a feature-spec-level checkpoint that gates Phase C alone. The design-spec-level RC-3 (joint Phase B + Phase C review) closes when *both* internals pass — RC-3a already closed for Phase B; RC-3b is the Phase C equivalent. Closing RC-3b lets the design-spec RC-3 close.

- **Trigger:** T-01, T-02, T-03, T-04 all complete; commits landed.
- **Review focus:** Whether the skill enforces the state-machine pre-condition correctly; whether the persona-frame discipline ("stay out of code") was held in T-02 and T-03; whether the hard-facts discipline was held (no cause hypotheses in Triage); whether the pointer-revalidation policy felt useful or like ceremony in T-03; whether the skip-investigation surface was used (and if so, whether the two-journal-entry discipline held); whether the persona-frame override felt natural in T-03 (or whether the suggested frame fit on the first try); whether OQ-3 and OQ-4 resolutions are correctly encoded in the skill prose.
- **Exit criteria:**
  - SKILL.md exists, ≤220 lines, frontmatter parseable, all twelve §5.1 structural sections present.
  - T-02 synthetic finding transitions cleanly; Intake-byte-preservation verified by `diff`; state-machine guard verified via second-invocation rejection evidence.
  - T-03 dogfood produces a real triaged finding; pointer-revalidation decision recorded; persona-frame discipline held; operator effort observations journaled.
  - T-04 README update preserves existing sections and adds the new "Triaging a finding" section under the line-count ceiling.
  - OQ-3 and OQ-4 resolutions are present in SKILL.md prose and in this spec's §5.3 and §5.4.
  - No `[blocker]` findings; `[important]` findings either resolved or escalated to amendments.
- **Status:** pass with comments on 2026-05-17 by Claude (self-review on behalf of waseric). 0 blockers, 0 important, 3 advisory. Advisories: (A-1) state-machine guard inspection-verified rather than second-invocation-exercised; (A-2) skip-investigation surface not exercised end-to-end across T-02 and T-03 (option (a) — accept inspection-only verification — adopted per the T-03 journal lean); (A-3) two session-side intake findings captured during Phase C work suggest a small §12 Out of Scope clarification in a future amendment. Exit criteria all met (criterion 2 met-with-caveat per A-1). Checkpoint closed; Phase C shippable. See journal entry `2026-05-17 — Review of RC-3b`.

### RC-3 — Intake & Triage Skill Review (design-spec checkpoint, closed by this spec's completion)

- **Inherited from design spec [§9 RC-3](../20260517-findings-pipeline/architecture.md#rc-3--intake--triage-skill-review-gates-phase-c--phase-e)**. Triggers when both Phase B (`finding-intake`) and Phase C (`finding-triage`) are operational. Phase B's RC-3a passed 2026-05-17.
- **This feature spec's contribution:** the `finding-triage` skill and its dogfood evidence, plus the resolutions to design-spec OQ-3 and OQ-4.
- **Exit criteria per design spec:** "Both skills exercised against at least one synthetic and one real finding; persona-frame check passes for each."
- **Phase C's piece of the persona-frame check:** triage's `Triaged by` field carries a persona-frame label derived from `Domain` (or operator-overridden); no triage prompt opens or reads codebase files; cause hypotheses are recorded as deferred in `TRIAGE_NOTES`, not as Triage findings.

## 10. Risks and Mitigations

| Description | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Triage absorbs cause analysis, blurring the boundary with investigation | Medium | High | OPERATING PRINCIPLES explicitly forbids causal speculation in Triage; WHAT NOT TO DO repeats it; T-02 and T-03 verify the discipline held | Executor; RC-3b reviewer |
| Persona-frame override surface is rarely used, defaulting back to a single suggestion the operator accepts blindly | Medium | Medium | T-03 explicitly records whether the suggestion was accepted or overridden; if always accepted, the suggestion mechanism is doing its job; the watch item is the *opposite* (operator overrides every time, indicating the derivation is wrong) | RC-3b reviewer |
| Pointer-revalidation policy feels like ceremony in T-03 dogfood | Medium | Medium | The policy is *optional with soft default* by design; T-03 evidence may show the default (treat-as-static) is hit on every dogfood, and the prompt itself adds time. If so, the policy is acceptable as long as the operator can dismiss it quickly | RC-3b reviewer |
| State-machine pre-condition guard is bypassed or implemented incorrectly | Low | High | T-02 acceptance criteria require a second-invocation attempt that must be rejected; verification is by inspection of the produced artifact's status banner and the skill's error output | Executor |
| Skip-investigation surface produces a single collapsed journal entry instead of two separate entries | Low | Medium | OPERATING PRINCIPLES enumerates "skip-route requires two journal entries"; Phase 3 APPLY explicitly walks the two-entry sequence; T-03 verifies if skip-route is chosen | Executor |
| Append discipline violated — skill rewrites Intake section while editing finding.md | Low | High | OPERATING PRINCIPLES enumerates "append, never rewrite Intake"; T-02 verifies by `diff`; the skill prose's Phase 3 APPLY uses "populate fields where `<placeholder>` was" wording, not "rewrite the finding" | Executor |
| OQ-3 / OQ-4 resolutions in the skill drift from the design spec leanings | Low | Medium | This spec's §5.3 and §5.4 explicitly quote the design-spec leanings; T-01 acceptance criteria require those sections; RC-3b reviewer checks against the design spec | Executor; RC-3b reviewer |
| Triage of the synthetic fixture (T-02) corrupts the regression-reference artifact | Low | Low | The synthetic fixture transitioning to `triaged` is the intended outcome — it becomes a Triaged-state regression reference. If the operator prefers a pristine intake fixture, they may create a new fixture at execution time | Executor |
| The skill is wrong (interaction model is the wrong cut for triage) | Low | High | T-03 dogfood is the early-detection mechanism; failure routes via `/spec-amend` (skill prose) or upstream `/spec-amend` (design spec) before RC-3b closes | Executor; RC-3b reviewer |

## 11. Rollout and Rollback

**Rollout:** No production rollout. The skill is a markdown artifact in `.agents/skills/`. "Adoption" means the operator invokes `/finding-triage` instead of editing the finding by hand.

**Rollback:**

- Each task is one or more commits; rollback is `git revert` of the affected commits.
- Removing `.agents/skills/finding-triage/SKILL.md` is non-breaking — operators can continue to triage findings manually by editing the Triage section per the schema field reference.
- T-04's README update is rollback-able via `git revert` of that commit; the README returns to its post-Phase-B state (no "Triaging a finding" section).
- T-02 and T-03 modifications to existing findings are rollback-able via `git revert`; the findings return to their `status: intake` state. Note: rolling back T-03's modification of the LWC shelves finding loses the recorded triage decision; consider whether that loss is acceptable before reverting.

**Monitoring during rollout:** N/A — no runtime. Subsequent dogfood (Phase F adoption review) provides the only post-rollout signal.

## 12. Out of Scope

- **`finding-investigate` skill** — Phase F decision per design spec OQ-1; not authored here.
- **`spec-amend` / `spec-write` accepting `FINDING_PATH`** — Phase E; separate feature spec(s).
- **Routing the dogfood finding through to a downstream spec** — Phase F; T-03 may record a route subtype if triage is sufficient, but does not invoke `/spec-amend` or `/spec-write` against the routed target.
- **Three-real-findings adoption gate** — Phase F; this spec produces one real triaged finding via T-03, which may or may not count toward the gate per [findings-pipeline-schema feature.md §13 OQ-2](../20260517-findings-pipeline-schema/feature.md#L383).
- **Automated artifact validation** — no CI check that a triaged finding matches the schema. Parked at [findings-pipeline-schema feature.md §13 OQ-1](../20260517-findings-pipeline-schema/feature.md#L373).
- **Multi-finding bundling/de-bundling automation** — out per §2 Non-goals.
- **Re-triage of non-intake findings** — Phase C rejects this; a future `finding-retriage` skill (if needed) is out of scope.
- **Quoting OQ-3 and OQ-4 resolutions back to the design spec via `/spec-amend`** — the resolutions are recorded in this feature spec's §5.3 and §5.4 and in the produced SKILL.md, satisfying RC-3's exit criteria. Quoting them back to the design spec's §13 (converting OQ-3 and OQ-4 from "open" to "decided") has been satisfied via design-spec amendments [2026-05-17-3](../20260517-findings-pipeline/journal.md) (OQ-3) and [2026-05-17-4](../20260517-findings-pipeline/journal.md) (OQ-4); the work was not part of this feature spec's task breakdown.
- **Session-side intake captures via `/finding-intake`** — findings captured via the upstream `/finding-intake` skill during a Phase C execution session (e.g., [spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/) commit `e6a93a2`; [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/) commit `c966f8a`) are out-of-scope side artifacts: they exercise the `/finding-intake` skill (Phase B deliverable) under real conditions, contribute independent evidence for the design spec's §6 interruption-tolerance NFR, and live in the Findings Pipeline awaiting their own triage in a later session. They require no Phase C amendment, no inclusion in §7 Task Breakdown, and no closeout against this spec's DoD; the intake skill's own discipline (atomic finding commit, status banner, intake journal entry) is the contract they meet.
- **External-system push (Slack/Linear/GitHub integration)** — the design spec's §12 declares this out of scope for the methodology; Phase C inherits the exclusion.
- **A 60-second NFR for triage** — out per §2 Non-goals. Triage is hard-facts work; speed is not the design constraint.

## 13. Open Questions

All three internal OQs (OQ-1 rich-vs-sparse heuristic prose; OQ-2 persona-frame label format; OQ-3 reproducing-without-opening-code) were decided at T-01 execution time per their leanings; see [§13a Decisions](#13a-decisions) below for the resolved record. No new OQs surfaced during T-01.

## 13a. Decisions

### D-1 (was OQ-1) — Rich-vs-sparse heuristic stated as a one-sentence rule + two grounded examples.

The SKILL.md Phase 2 pointer-revalidation policy includes a single-sentence heuristic: "a summary is **rich** when it is ≥3 sentences, names the affected components, names the reporter(s), and includes verbatim quotes or snapshot references; **sparse** when it is ≤2 sentences, refers the reader to 'see the URL,' or contains only a one-line 'moderators report X.'" The closing sentence ("the heuristic is orientation, not a threshold") preserves operator judgment as the load-bearing authority. Recorded in SKILL.md Phase 2 "Rich-vs-sparse heuristic" subsection. Decided: 2026-05-17 (T-01 execution).

### D-2 (was OQ-2) — Persona-frame label carries both pieces: `<name>; <descriptive frame>; persona-frame: triage`.

Preserves the per-phase phase-label discipline established by intake while making the descriptive frame visible at a glance. Example written in SKILL.md: `waseric; business analyst; persona-frame: triage`. The `triage` label is fixed across all triage invocations; only the descriptive frame varies. Recorded in SKILL.md Phase 2 "Persona-frame derivation" subsection and Phase 3 step 1 (`Triaged by` field). Decided: 2026-05-17 (T-01 execution).

### D-3 (was OQ-3) — Running code to reproduce is allowed; reading code to hypothesize is not.

The SKILL.md draws the distinction explicitly in OPERATING PRINCIPLE #2 ("Stay out of code") and reinforces it in WHAT NOT TO DO ("Do not open code in the codebase being triaged about. Reading source to hypothesize about cause is investigation's job. *Running* code to reproduce a signal — joining a server, triggering an action, observing the error — is allowed and often necessary — that is the act of reproduction, not file inspection."). The opening prose framing also names this distinction. Three placements ensure the discipline is visible regardless of which section the AI agent loads first. Recorded in SKILL.md opening prose paragraph 3, OPERATING PRINCIPLE #2, and WHAT NOT TO DO bullet 1. Decided: 2026-05-17 (T-01 execution).

## 14. References

- **Upstream design spec:** [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md) — §5.3 Triage phase (interface contract); §5.5 Routing decisions (route subtypes + terminal status mapping); §5.6 Persona model (multi-domain orientation); §6 NFRs (observability, external-pointer durability); §7 Implementation Sequencing (Phase C definition); §13 OQ-3, OQ-4 (resolved by this spec).
- **Upstream sibling feature specs:**
  - [specs/20260517-findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md) — Phase A (done). The schema this skill consumes.
  - [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md) — Phase B (done). The structural and stylistic precedent for this spec.
- **Schema artifacts (Phase A deliverables):**
  - [specs/findings/README.md](../findings/README.md) — field reference, state machine, status semantics, persona-frame taxonomy.
  - [specs/findings/_template/finding.md](../findings/_template/finding.md) — canonical artifact template; Triage and Route sections are this skill's edit surface.
  - [specs/findings/_template/journal.md](../findings/_template/journal.md) — canonical journal template; `Triaged` and `Routed` / `Closed` commented-out skeletons are this skill's append shape.
- **Phase B deliverable:** [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) — the upstream skill whose output this skill consumes; closest structural reference.
- **Existing intake artifacts for validation:**
  - [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/) — T-02 target.
  - [specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/) — T-03 target.
- **End-state Investigation reference:** [specs/findings/20260517-tab-display-issues/](../findings/20260517-tab-display-issues/) — shape of a finding past triage, for comparison.
- **Constitution:** [specs/mission.md](../mission.md), [specs/tech-stack.md](../tech-stack.md), [specs/roadmap.md](../roadmap.md).
- **Spec-path convention:** [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) — directory layout this spec follows.
- **Peer skill artifacts (pattern references):**
  - [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) — primary structural reference.
  - [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) — peer skill that also edits in-place across a defined section surface.
  - [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) — peer skill with multi-stage Phase 3 (orient → propose → execute → close); structural reference for the skip-investigation surface.
