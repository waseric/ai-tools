# Finding Intake Skill — Feature Specification

> Status: Draft — Open for Review
> Date: 2026-05-17
> Author: waseric + Claude
> Audience: Eric Wasgatt (executor); AI coding agents executing this spec; reviewers at the Phase B internal checkpoint and at the design-spec RC-3 (joint with Phase C)
> Upstream design spec: [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)
> Sibling upstream feature spec (Phase A — done): [specs/20260517-findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md)

## 1. Overview

This feature spec implements **Phase B** of the [Findings Pipeline design spec](../20260517-findings-pipeline/architecture.md): author the `finding-intake` skill that converts a signal into a finding artifact. The skill consumes the Phase A schema (template + journal template + README field reference) and produces a per-finding directory at `specs/findings/YYYYMMDD-<short-name>/` populated through the Intake phase, with `status: intake` set and the starter journal entry written.

The skill is the lowest-ceremony entry point to the Findings Pipeline. The 60-second-operator-effort NFR from the design spec is the central design constraint — every interaction the skill imposes on the operator must justify itself against that target.

## 2. Goals and Non-goals

**Goals:**

- Create `.agents/skills/finding-intake/SKILL.md` following the YAML-frontmatter + ROLE + OPERATING PRINCIPLES + INPUTS + phased-workflow convention used by the six existing peer skills.
- Produce a finding artifact at `specs/findings/YYYYMMDD-<short-name>/finding.md` + `journal.md` from a signal (textual narrative, external pointer, or both), populating the Intake section per the [findings schema README](../findings/README.md) field reference and the [_template/finding.md](../findings/_template/finding.md) shape.
- Default to the 60-second-operator-effort target: defer every optional field, accept "unknown" honestly, minimize prompts.
- Surface external-pointer fetch failures rather than silently accepting them — when the operator provides a URL and a fetch is attempted, a fetch failure must be visible to the operator before the finding is created.
- Validate the skill via one synthetic exercise and one real-signal dogfood exercise.

**Non-goals:**

- Authoring the `finding-triage` skill (Phase C — separate feature spec).
- Authoring or graduating `finding-investigate` (Phase F decision per design spec OQ-1).
- Amending `spec-amend` or `spec-write` to accept `FINDING_PATH` (Phase E — separate feature spec(s)).
- Routing the dogfood finding end-to-end (Phase F adoption review).
- Flipping the "Creating a new finding" section of [specs/findings/README.md](../findings/README.md) to make `/finding-intake` primary and the manual `cp` path the fallback — bundled into Phase B but executed as a small follow-on task (T-04 below), kept scope-honest as a README update separate from the skill itself.
- Automated deduplication against existing findings — adds ceremony that defeats the 60-second target. Deduplication is triage's responsibility.
- Fetching external URLs *for the operator* — the agent's existing URL-fetching capability (Claude Code WebFetch or equivalent) is delegated to; the skill prescribes the policy, not the mechanism.

## 3. Background and Constraints

### Spec repo context

This feature spec lives in the same repository as the artifact it modifies — there is no codebase distinct from the methodology repo. `SPEC_REPO_ROOT` and `CODEBASE_ROOT` are the same.

- **SPEC_REPO_ROOT:** `/Users/eric/scm/github/waseric/ai-tools`
- **SPEC_TARGET_BRANCH:** `main`

### Upstream artifacts (authoritative input)

- **Design spec:** [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md). §5.2 (Intake phase) is the interface contract; §6 (NFRs) sets the 60-second target and external-pointer durability requirement; §5.6 (Persona model) sets the intake persona-frame breadth.
- **Schema artifacts (Phase A — RC-2 passed, remediated 2026-05-17):**
  - [specs/findings/README.md](../findings/README.md) — 30-field reference table, state machine, status semantics, persona-frame taxonomy, "Creating a new finding" manual path.
  - [specs/findings/_template/finding.md](../findings/_template/finding.md) — canonical template; placeholder-vs-unknown convention codified in HTML comment.
  - [specs/findings/_template/journal.md](../findings/_template/journal.md) — canonical journal; starter Intake entry plus commented skeleton entries; section header format `## <YYYY-MM-DD> — <New status>: <one-line summary>`.
  - [specs/findings/20260517-tab-display-issues/](../findings/20260517-tab-display-issues/) — T-05 living example. Reference for what an end-state Intake-phase artifact looks like.
- **Sibling skill patterns:** [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md), [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md), [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md). Each is 200–225 lines; structured as YAML frontmatter → ROLE → OPERATING PRINCIPLES → INPUTS → Phase 1 → Phase 2 → Phase 3.
- **Constitution:** [mission.md](../mission.md), [tech-stack.md](../tech-stack.md), [roadmap.md](../roadmap.md). Methodology is markdown-only; this spec produces a markdown skill artifact.

### Open questions inherited from design spec

Post-amendment numbering (per [design-spec amendment 2026-05-17-2](../20260517-findings-pipeline/journal.md#L101)):

- **OQ-1** (investigation graduation): decided at RC-5; Phase B is unaffected.
- **OQ-2** (incident vs. problem distinction): collapsed to "finding"; Phase B is unaffected.
- **OQ-3** (multi-domain persona naming): descriptive recording is the chosen policy. The intake skill **must not** hardcode a domain-specific persona-frame; intake's persona-frame is "anyone."
- **OQ-4** (triage-time revalidation of external pointers): decided at RC-3 as part of triage skill design. Phase B intake captures verbatim and snapshots if fetchable; revalidation is triage's concern.

### Constraints

- **Markdown-only deliverable.** No new runtime, build step, or test runner ([tech-stack.md:11-13](../tech-stack.md#L11)).
- **AI context-window economy.** SKILL.md target: ~200 lines, matching peer skills.
- **Corporate-TLS interception** ([tech-stack.md:28](../tech-stack.md#L28)). The skill cannot mandate URL fetching as a precondition. The agent running the skill may attempt a fetch; the skill prescribes the failure-handling policy.
- **No automated enforcement.** The skill is consumed by an AI agent (Claude Code, GitHub Copilot) or used by a human reading it as a checklist; there is no validator that checks operator-produced artifacts against the skill's prescriptions.

## 4. Architecture

### Deliverable layout

```
.agents/
  skills/
    finding-intake/
      SKILL.md            ← this spec produces (T-01)

specs/
  findings/
    20260517-<dogfood-short-name>/    ← T-03 produces (a real finding)
      finding.md
      journal.md
    20260517-<synthetic-short-name>/  ← T-02 produces, then is cleaned up or
      finding.md                        retained per decision in T-02
      journal.md
```

The SKILL.md is the primary deliverable. The two finding directories are validation artifacts; the synthetic one may be retained as a regression-reference or deleted at T-02 closeout (decided during execution).

### Skill invocation shape

```
/finding-intake [optional structured args]
```

In **interactive mode** (typical human use), the skill prompts for required fields with sensible defaults. In **structured-input mode** (typical AI-agent use), the skill accepts the INPUTS block populated by the calling agent and skips the prompts.

### Interface contract

Inputs (named, all optional except `TITLE` + at least one of `SUMMARY` / `EXTERNAL_POINTER`):

| Input | Type | Default | Required |
|---|---|---|---|
| `TITLE` | text | none | always |
| `SUMMARY` | paragraph | none | unless EXTERNAL_POINTER also empty |
| `EXTERNAL_POINTER` | URL or pointer | none | unless SUMMARY also empty |
| `REPORTED_BY` | text | "self" | no |
| `REPORTED_VIA` | text | "text" if no pointer; "URL" if pointer; both if both | no |
| `CAPTURED_BY` | text | git `user.name` (with persona-frame label) | no |
| `DOMAIN` | enum from schema | "unknown" (intake's best guess; refined at triage) | no |
| `SHORT_NAME` | kebab-case slug | AI-derived from `TITLE`, operator confirms | no |
| `DATE` | YYYY-MM-DD | operator's local date | no |

Outputs:

- `specs/findings/<DATE>-<SHORT_NAME>/finding.md` — Intake section populated; later sections in `<placeholder>` form.
- `specs/findings/<DATE>-<SHORT_NAME>/journal.md` — Starter Intake entry populated; later skeletons left as HTML-commented blocks.
- A path string returned to the caller for downstream chaining.

### Pointer-fetch policy

When `EXTERNAL_POINTER` is supplied:

1. The skill **requires** an operator-supplied `SUMMARY`. A pointer alone is insufficient — the summary is the load-bearing capture; the URL is durability convenience (per design spec §6 External-pointer durability).
2. If the agent invoking the skill has URL-fetching capability, the skill **attempts** a fetch and snapshots the result as part of the "External references" field of the Intake section, prefixed `<!-- fetched <YYYY-MM-DD> -->`.
3. If the fetch fails, the skill **must not** silently proceed. It surfaces the failure (reason, status code) to the operator and presents three choices: retry, proceed without snapshot (acknowledging reduced durability), or cancel. Journal entry records the outcome regardless.
4. If the agent has no fetch capability (pure-human path), the skill instructs the operator to either paste content from the URL manually or journal the lack of snapshot.

### Persona-frame handling

The Intake `Captured by` field carries the persona-frame label `intake`. The skill does **not** prompt the operator to pick a persona-frame at intake — the field is fixed to `intake`. Per design spec §5.6 amendment sub-change F, intake's persona-frame is "anyone," and re-asking the operator to confirm that is a 60-second-target violation.

## 5. Detailed Design

### 5.1 `.agents/skills/finding-intake/SKILL.md` shape

**Purpose.** A self-contained skill artifact that an AI agent (or a human reading it) can execute end-to-end without further design input.

**Required sections** (matching peer-skill convention):

1. **YAML frontmatter:** `name: finding-intake`; `lastUpdated: <YYYY-MM-DD>`; `description:` one or two sentences summarizing the skill's purpose, pairing references to [[spec-amend]] and [[spec-write]] (Phase E downstream consumers) and to the Findings Pipeline design spec.
2. **Title heading:** `# Finding Intake`.
3. **Opening paragraphs:** 2–3 short paragraphs framing the skill's purpose, its position in the Findings Pipeline (Phase B per [design spec §7](../20260517-findings-pipeline/architecture.md#7-implementation-sequencing)), and its 60-second-target operating constraint.
4. **"How this skill works"** subsection: brief operator-facing instructions on interactive vs. structured-input invocation.
5. **INPUTS block:** the table from §4 above, transcribed into the skill's input contract.
6. **ROLE:** short statement that the skill is the lowest-ceremony entry to the pipeline and that the operator/agent is acting in the *intake* persona-frame regardless of who they are.
7. **OPERATING PRINCIPLES:** 5–7 numbered principles (capture-rate over capture-quality; "unknown" is first-class; never silently accept a fetch failure; never prompt for triage-phase fields; the artifact is self-contained — never references the originating conversation; placeholder vs. unknown distinction).
8. **Phase 1 — ORIENT:** read the schema artifacts ([README](../findings/README.md), [template](../findings/_template/finding.md)) if not loaded; verify the operator's intent (signal source, working title) in one round-trip if interactive.
9. **Phase 2 — DRAFT:** propose `SHORT_NAME` from `TITLE`; show the operator the proposed directory path and the Intake section to be written; pause for confirmation.
10. **Phase 3 — APPLY:** create the directory; copy the templates; populate the Intake section and starter Intake journal entry; on URL pointer, attempt fetch per §4 pointer-fetch policy; commit if the operator's session is in a commit-friendly state, otherwise leave as a working-tree change.
11. **OUTPUT FORMAT:** the path string returned to the caller; the journal entry summary.
12. **WHAT NOT TO DO:** explicit anti-goals (no triage-phase prompts; no fetch failure silently swallowed; no deduplication scan; no rewriting the template).

**Pattern invoked.** Skill-as-self-contained-prompt — the format used by all six existing skills in `.agents/skills/`. Loadable by Claude Code's slash-command resolver and by GitHub Copilot.

**Why this design.** The skill replicates a structure operators and agents already know from `spec-write`, `spec-amend`, `spec-execute`. New methodology surface area is minimized; the cognitive load is "another phased skill," not "a new shape."

**Alternatives considered.**

- *Embedded script generating the artifact.* Rejected: violates tech-stack.md's "no executable runtime"; AI agents already have file-writing tools, so a script adds dependency without capability.
- *Two skills — `finding-intake-quick` for the 60-second path and `finding-intake-rich` for richer signals.* Rejected: bifurcation forces a choice the operator shouldn't have to make at the noticing moment. One skill with sensible defaults handles both extremes.
- *Skill that also runs triage in the same invocation.* Rejected: collapses Phase B and Phase C; loses interruption-tolerance (the property that an intake can be parked at `status: intake` for a later triage session).

### 5.2 Short-name derivation

**Purpose.** Produce a filesystem-safe, human-readable directory slug from the operator's title.

**Behavior.**

- Lowercase the title.
- Strip punctuation except hyphens.
- Replace whitespace with hyphens.
- Drop common stop words (`the`, `a`, `an`, `of`, `is`, `are`, `and`, `or`, `to`, `for`, `in`, `on`, `with`) if the resulting slug is still ≥3 words.
- Truncate to a target length: 3–5 word-pieces, ≤40 characters total.
- Present the result to the operator in interactive mode for one-step accept/edit. In structured-input mode, the slug is used as-supplied if `SHORT_NAME` is given, else derived as above with no confirmation.

**Pattern invoked.** Kebab-case slug-derivation, common in content-management systems. No external library; the skill prose specifies the rules and the agent applies them.

**Why this design.** AI-derive + operator-confirm preserves the 60-second target (one keystroke to accept) while giving the operator the final word on naming.

**Alternatives considered.**

- *Title-as-directory-name verbatim.* Rejected: titles may contain colons, slashes, capital letters, and other characters that produce ugly paths.
- *Hash-based slug (`f3a2c1/`).* Rejected: directories should be human-scannable.

### 5.3 Date handling

**Purpose.** Produce `Date opened` and `Last transition` values matching the operator's calendar day.

**Behavior.**

- Default to the operator's local-timezone date in `YYYY-MM-DD` format.
- `Date opened` and `Last transition` are the same value at intake (the finding was just created; its most recent transition is the initial intake).
- If `DATE` is supplied via structured input, use it verbatim (this enables retroactive intake — see [the T-05 example finding's "Retroactive intake date" observation in the schema spec's journal](../20260517-findings-pipeline-schema/journal.md#L182) for prior art).

**Why this design.** Local-timezone matches every existing journal-entry date in the repo. No timezone bikeshedding; no UTC translation surprises.

### 5.4 README integration (T-04 — small follow-on)

The Phase A README already forecasts the skill at [specs/findings/README.md:158](../findings/README.md#L158): "The downstream `finding-intake` skill (Phase B in the [design spec's implementation sequencing](...)) will automate steps 1–4 once available. Until then, the manual copy is the supported path."

T-04 (below) flips the section so the `/finding-intake` invocation is the primary path and the manual `cp` recipe is the fallback. One-paragraph rewrite; no other content shift.

## 6. Non-functional Requirements

| Property | Requirement |
|---|---|
| **60-second operator effort** | From "I should capture this" to "finding is at `status: intake`," operator effort ≤60 seconds for a typical signal (one paragraph of narrative; one URL; no exotic fields). Measured during T-03 dogfood. |
| **Template fidelity** | Produced `finding.md` matches `_template/finding.md` field-by-field per the README field-reference table; placeholder-vs-unknown convention is honored (Intake section filled, later sections in `<placeholder>` form). |
| **Pointer-fetch transparency** | When a URL pointer is supplied and a fetch is attempted, the outcome is visible to the operator. Failures are never silently swallowed. Successes are journaled with the snapshot date. |
| **Self-contained artifact** | Produced `finding.md` does not require the originating conversation or any external system to be still reachable. Quoted summary and any successful snapshot travel with the artifact (design spec §5.1, §6 External-pointer durability). |
| **Persona-frame honesty** | `Captured by` field is labeled `intake` regardless of who the operator is or which agent invoked the skill. Triage-phase fields are not pre-filled at intake; they are left in `<placeholder>` form. |
| **No new dependencies** | Pure markdown deliverable. No new runtime, library, or build step. |
| **Backward compatibility** | The manual-copy path documented in the Phase A README remains valid as a fallback. Operators or contexts where the skill isn't available continue to work unchanged. |
| **Context economy** | SKILL.md ≤220 lines (peer-skill ceiling). The skill loads only what it needs from `specs/findings/` (`README.md`, `_template/finding.md`, `_template/journal.md`) — not the full pipeline. |

## 7. Task Breakdown

### T-01 — Author `.agents/skills/finding-intake/SKILL.md`

**Status:** done — 2026-05-17 — commit 1e640c7 — see journal entry. SKILL.md written at 149 lines (below §7 soft target of 180–220, within §6 NFR ceiling of ≤220); all 12 §5.1 structural sections present; all 9 INPUTS fields covered; all 4 anti-goals enumerated in WHAT NOT TO DO. The three internal OQs from §13 (auto-commit, captured-by, multi-pointer) were decided at execution time per their leanings — see §13 for the decisions.

**Scope:**

- New file: `.agents/skills/finding-intake/SKILL.md`.
- Sections per [§5.1](#51-agentsskillsfinding-intakeskillmd-shape) above (YAML frontmatter through WHAT NOT TO DO).
- Target length 180–220 lines. Match the prose style of [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) (a structurally similar "transform input into per-spec artifact" skill).
- INPUTS block per [§4](#interface-contract) above.
- OPERATING PRINCIPLES enumerate at minimum: capture-rate over quality; "unknown" is first-class but not the default for unstarted phases; never silently swallow a fetch failure; never prompt triage fields; placeholder-vs-unknown distinction honored; artifact is self-contained.
- Phase 3 APPLY phase explicitly: creates the directory, copies the templates verbatim, fills the Intake section + starter Intake journal entry, leaves later sections in `<placeholder>` form, returns the artifact path.

**Acceptance criteria:**

- **Given** the schema artifacts at [specs/findings/README.md](../findings/README.md), [specs/findings/_template/finding.md](../findings/_template/finding.md), and [specs/findings/_template/journal.md](../findings/_template/journal.md),
- **When** the skill is invoked with a `TITLE` and either a `SUMMARY` or an `EXTERNAL_POINTER`,
- **Then** the skill produces `specs/findings/<DATE>-<SHORT_NAME>/finding.md` and `journal.md` with the Intake section + starter Intake journal entry populated, and all later sections in `<placeholder>` form.
- **And** the file structure, field names, and section headings match the canonical templates byte-for-byte at every position not occupied by the operator's input.
- **And** the YAML frontmatter contains `name: finding-intake`, a `lastUpdated:` date, and a `description:` paragraph that names the Findings Pipeline and references downstream `spec-amend` / `spec-write` integration.

**Tests required:**

- Inspection: SKILL.md exists, is parseable as YAML+Markdown, opens with the frontmatter block, and contains the twelve required structural sections from §5.1.
- Inspection: every INPUT field documented in §4 appears in the skill's INPUTS block.
- Inspection: the WHAT NOT TO DO section explicitly forbids the four anti-goals (no triage prompts; no silent fetch failures; no dedup scan; no template rewrite).

**Definition of Done:** File written; under 220 lines (`wc -l`); committed.

**Dependencies:** None.

**Estimated size:** M.

### T-02 — Synthetic validation exercise

**Status:** done — 2026-05-17 — commit e7630c9 — see journal entry. Skill invoked in structured-input mode against a fabricated methodology-domain signal. Produced [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/) matching template structurally; Triage/Investigation/Route sections byte-for-byte identical to template; all six behavioral properties verified (60-second-target plausibility, no triage-phase prompts, persona-frame label correct, placeholder-vs-unknown convention honored, Date-opened == Last-transition, self-contained artifact). **Retain-vs-delete: retain** — see journal entry. Two advisory observations surfaced (template-scaffolding ambiguity in SKILL.md, stop-word list cosmetics); both deferred to post-RC-3a `spec-amend`, neither blocking T-03.

**Scope:**

- Invoke the skill (interactively or via structured input — operator's choice at execution time) with a *fabricated* signal: a one-paragraph narrative describing a plausible but non-real finding (e.g., "test-only signal: theme-flicker on dark-mode toggle"). No external pointer.
- Verify the produced `finding.md` and `journal.md` match the templates field-by-field.
- Verify the skill's user-facing behavior follows the design (60-second target plausibility, no triage-phase prompts, persona-frame label correct, placeholder vs. unknown convention honored).
- Record validation outcomes (what worked, what surprised, any friction) in [journal.md](journal.md).
- Decide at closeout: retain the synthetic finding as `specs/findings/<DATE>-<synthetic>/` (as a regression reference), or delete it. Document the choice with rationale.

**Acceptance criteria:**

- **Given** the skill from T-01,
- **When** the operator invokes it against a fabricated signal,
- **Then** the artifact produced matches the template structurally and the validation outcomes (including the retain/delete decision for the synthetic artifact) are recorded in this feature spec's journal.

**Tests required:**

- The exercise itself is the test. Success criterion: artifact produced matches the template; no skill bug surfaced that would block T-03 dogfood.
- Failure criterion: a template-mismatch or skill bug surfaces. Failure routes back to T-01 amendment via `/spec-amend`.

**Definition of Done:** Synthetic artifact created (or created-and-deleted with rationale); validation outcomes journaled; any surfaced bugs either resolved via T-01 amendment or escalated as `[blocker]` open questions.

**Dependencies:** T-01.

**Estimated size:** S.

### T-03 — Real-signal dogfood exercise

**Status:** done — 2026-05-17 — commits 2a6bcc3 (finding artifact) + this closeout commit — see journal entry. Skill invoked in interactive mode against an operator-supplied real signal: Sandlot Bug Reports forum thread "Easy Survival - Error when placing items in shelves" (LWC plugin "Missing API" error on shelf interaction; Easy world affected, Normal world clean; two reporters). Produced [specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/) at `status: intake`; Triage/Investigation/Route sections byte-for-byte identical to template; 60-second NFR met with comfortable headroom (~30–60s of operator interaction: PDF attach + two confirm round-trips). Pointer-fetch policy refinement surfaced: the existing policy ("attempt fetch if capable") did not anticipate the case "operator pre-supplied a snapshot at session intake." Documented in journal as advisory observation; deferred to post-RC-3a `spec-amend` against SKILL.md Phase 3 step 4.

**Scope:**

- Operator selects a real new signal at execution time — any observation that genuinely warrants a finding, sourced from the operator's recent work, an external thread, or an in-flight session. The signal must not be the T-02 fabricated case and must not be the T-05 schema-spec example finding.
- Invoke the skill against the real signal. Time the operator effort from "I should capture this" to "artifact is at `status: intake`" — verify the 60-second NFR plausibly held (allow a wider envelope if the signal is unusually complex, but record the duration honestly).
- Verify the artifact:
  - Matches the template structurally.
  - Captures the signal's substance accurately in the Summary field.
  - Handles any external pointer per [§4 pointer-fetch policy](#pointer-fetch-policy): summary supplied; fetch attempted if applicable; fetch outcome surfaced and recorded.
  - Leaves Triage/Investigation/Route in `<placeholder>` form.
- Record dogfood outcomes (timing, friction, any schema gaps surfaced) in [journal.md](journal.md).
- The real finding remains in `specs/findings/` as the second living example (joining the T-05 `tab-display-issues` finding) and as candidate evidence for design-spec Phase F adoption review (subject to Phase F adoption-gate criteria — see [findings-pipeline-schema feature.md §13 OQ-2](../20260517-findings-pipeline-schema/feature.md#L383)).

**Acceptance criteria:**

- **Given** the skill from T-01 and a real new signal selected at execution time,
- **When** the operator invokes the skill,
- **Then** a real finding artifact exists in `specs/findings/`, the operator-effort timing is journaled, and any pointer-fetch outcome is recorded.
- **And** if the 60-second NFR was missed by more than 50% (≥90 seconds) the journal records *why* and proposes either a skill amendment or an acknowledgement that the NFR needs revisiting at RC-3.

**Tests required:**

- The exercise itself is the test. Success criterion: artifact produced, signal accurately captured, timing recorded, fetch outcome (if any) handled per policy.
- Failure criterion: a substantive skill or schema gap surfaces. Failure routes via `/spec-amend` against this feature spec or the design spec, as appropriate.

**Definition of Done:** Real finding artifact committed in `specs/findings/`; dogfood outcomes journaled; any surfaced gaps either resolved or escalated.

**Dependencies:** T-01, T-02.

**Estimated size:** S.

### T-04 — Update `specs/findings/README.md` "Creating a new finding" to make `/finding-intake` primary

**Scope:**

- Edit [specs/findings/README.md](../findings/README.md), specifically the "Creating a new finding" section (currently lines 142–158).
- Make the `/finding-intake` invocation the **primary** path: a short paragraph + the slash-command syntax.
- Keep the manual `cp` recipe as the **fallback** path, under a heading like "Manual fallback (if the skill is not available)."
- Update the forward-pointer paragraph (currently line 158: "The downstream `finding-intake` skill ... will automate steps 1–4 once available") to reflect that the skill is now available, citing this feature spec.
- Keep the existing "One finding or several?" bundle-vs-split paragraph intact — it is orthogonal.

**Acceptance criteria:**

- **Given** the skill from T-01 is operational and the dogfood from T-03 has produced at least one real finding,
- **When** a consumer reads the README's "Creating a new finding" section,
- **Then** they find the `/finding-intake` invocation as the recommended primary path with the manual `cp` recipe still present as a fallback, and the cross-references resolve.

**Tests required:**

- Inspection: README updated; `/finding-intake` is the first option presented; manual recipe is preserved verbatim or with only header-level changes; line count remains ≤200.
- Inspection: cross-references to the skill ([.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md)) and this feature spec resolve.

**Definition of Done:** README.md updated; committed; cross-references valid.

**Dependencies:** T-01, T-03 (do not flip the primary path until the skill has been dogfooded successfully).

**Estimated size:** S.

## 8. Test Strategy

- **Inspection-based validation.** SKILL.md is markdown; the only "tests" are inspection passes (structure, frontmatter, INPUTS coverage, anti-goals stated).
- **Synthetic exercise (T-02).** Catches skill-mechanics bugs without exposing a real signal to a buggy skill.
- **Dogfood exercise (T-03).** The integration test: real signal, real operator timing, real artifact. The 60-second NFR is verified here.
- **No mocking, no fixtures, no test runner.** Methodology repo discipline is prose review.
- **No automated artifact validation.** A future spec may add a CI-style validator that checks `specs/findings/<dir>/finding.md` matches the schema; not in scope here ([findings-pipeline-schema feature.md §13 OQ-1](../20260517-findings-pipeline-schema/feature.md#L373) parked this).

## 9. Review Checkpoints

### RC-3a — Phase B Skill Review (this feature spec's internal checkpoint)

This is a feature-spec-level checkpoint that gates Phase B alone. The design-spec-level RC-3 (joint Phase B + Phase C review) still requires Phase C to complete; this checkpoint exists to declare Phase B shippable before authoring Phase C against it.

- **Trigger:** T-01, T-02, T-03, T-04 all complete; commits landed.
- **Review focus:** Whether the skill produces artifacts faithful to the schema; whether the 60-second NFR was plausibly met in T-03 dogfood; whether the pointer-fetch policy was honored if exercised; whether the skill prose follows peer-skill conventions; whether the README flip preserved the manual-fallback path.
- **Exit criteria:**
  - SKILL.md exists, ≤220 lines, frontmatter parseable.
  - T-02 synthetic artifact matches template byte-for-byte at non-input positions.
  - T-03 dogfood produced a real finding; effort timing recorded; pointer-fetch outcome (if any) recorded.
  - T-04 README update preserves the manual fallback and updates the forward-pointer.
  - No `[blocker]` findings; `[important]` findings either resolved or escalated to amendments.

### RC-3 — Intake & Triage Skill Review (design-spec checkpoint, not closed by this spec alone)

- **Inherited from design spec §9 RC-3.** Triggers when both Phase B (`finding-intake`) and Phase C (`finding-triage`) are operational.
- **This feature spec's contribution:** the `finding-intake` skill and its dogfood evidence. The triage skill is contributed by the Phase C feature spec.
- **Exit criteria per design spec:** "Both skills exercised against at least one synthetic and one real finding; persona-frame check passes for each."
- **Phase B's piece of the persona-frame check:** intake's `Captured by` field is labeled `intake`; no triage-phase fields are pre-filled by intake; the artifact is acquired without opening code (intake stays out of the codebase, regardless of who the operator is).

## 10. Risks and Mitigations

| Description | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| 60-second NFR is unachievable for typical signals | Medium | High | T-03 explicitly times the dogfood; if missed by >50% the journal proposes either a skill amendment or an NFR revisit | Executor; RC-3a reviewer |
| Skill prompts the operator for triage-phase fields, violating Phase B / Phase C separation | Medium | Medium | OPERATING PRINCIPLES explicitly forbids this; WHAT NOT TO DO repeats it; T-02 synthetic exercise verifies | Executor |
| URL-pointer fetch failures silently accepted, violating the user's explicit Phase 2 directive | Medium | Medium | OPERATING PRINCIPLES enumerates "never silently swallow a fetch failure"; pointer-fetch policy in §4 prescribes surface-retry-proceed-cancel; T-03 verifies if a URL signal is chosen | Executor; RC-3a reviewer |
| Short-name derivation produces ugly slugs ("a-the-of-and-bug") | Low | Low | Stop-word filtering + 3-word minimum + operator one-step confirm. Cosmetic only — recoverable via rename | Executor |
| Synthetic exercise (T-02) misses bugs that surface only against real signals | Medium | Medium | T-03 dogfood is the second-line defense; any T-03 surfaced bug routes via `/spec-amend` against T-01 | Executor; RC-3a reviewer |
| README flip (T-04) lands before the skill is dogfooded, advertising an untested path | Low | Medium | T-04 explicitly depends on T-03; cannot proceed until dogfood succeeds | Task dependency declaration |
| Skill is too long (exceeds peer-skill ceiling of ~220 lines), bloating the methodology context | Low | Low | Length budget declared in §6 and T-01 DoD; trim in revision if exceeded | Executor |
| The skill is wrong (the chosen interaction model is the wrong cut for intake) | Low | High | T-03 dogfood is the early-detection mechanism; failure routes via `/spec-amend` (skill prose) or upstream `/spec-amend` (design spec) before RC-3a closes | Executor; RC-3a reviewer |

## 11. Rollout and Rollback

**Rollout:** No production rollout. The skill is a markdown artifact in `.agents/skills/`. "Adoption" means the operator invokes `/finding-intake` instead of (or before) the manual `cp` recipe.

**Rollback:**

- Each task is one or more commits; rollback is `git revert` of the affected commits.
- Removing `.agents/skills/finding-intake/SKILL.md` is non-breaking — the manual `cp` recipe in the Phase A README continues to work.
- T-04's README update is rollback-able via `git revert` of that commit; the README returns to its Phase-A-final state ("manual is primary; skill is forecast").

**Monitoring during rollout:** N/A — no runtime. Subsequent dogfood (Phase F adoption review) provides the only post-rollout signal.

## 12. Out of Scope

- **`finding-triage` skill** — Phase C; separate feature spec.
- **`finding-investigate` skill** — Phase F decision per design spec OQ-1; not authored here.
- **`spec-amend` / `spec-write` accepting `FINDING_PATH`** — Phase E; separate feature spec(s).
- **Three-real-findings adoption gate** — Phase F; this spec produces one real finding via T-03 dogfood, which may or may not count toward the gate per [findings-pipeline-schema feature.md §13 OQ-2](../20260517-findings-pipeline-schema/feature.md#L383).
- **Automated artifact validation** — no CI check that a finding matches the schema. Parked at [findings-pipeline-schema feature.md §13 OQ-1](../20260517-findings-pipeline-schema/feature.md#L373).
- **Deduplication scan** — explicitly out per §2 Non-goals.
- **Cross-repo finding intake automation** — the design spec's §5.7 multi-repo handling is in scope conceptually (the operator may name a consumer repo path in the finding) but no special skill behavior is added for it in Phase B.
- **External-system push (Slack/Linear/GitHub integration)** — the design spec's §12 declares this out of scope for the methodology; Phase B inherits the exclusion.

## 13. Open Questions

All three internal OQs (OQ-1 auto-commit policy; OQ-2 captured-by field; OQ-3 multi-pointer signals) were decided at T-01 execution time per their leanings; see [§13 Decisions](#13-decisions) below for the resolved record. No new OQs surfaced during T-01.

## 13a. Decisions

### D-1 (was OQ-1) — Skill leaves the artifact as a working-tree change; does not auto-commit.

The skill produces `finding.md` and `journal.md` and returns a suggested one-line commit message to the operator. The operator commits when their session is in a commit-friendly state. Preserves interruption-tolerance — an operator capturing a finding during active `spec-execute` does not end up with a finding commit interleaved with their feature work. Recorded in SKILL.md Operating Principle #7 and Phase 3 step 5. Decided: 2026-05-17 (T-01 execution).

### D-2 (was OQ-2) — `Captured by` defaults to git `user.name` only; no email.

Matches existing journal-entry convention in the repo, avoids accidental email exposure in a public-ish methodology repo. Recorded in SKILL.md Phase 2 ("Captured by" derivation). If `user.name` is unset, the skill falls back to `unknown; persona-frame: intake` and journals the absence. Decided: 2026-05-17 (T-01 execution).

### D-3 (was OQ-3) — `EXTERNAL_POINTER` accepts comma- or newline-separated list of pointers.

Each pointer is fetched per the §4 pointer-fetch policy; each outcome (success / failure with chosen disposition) is journaled separately. No special UI for adding-one-by-one. Recorded in SKILL.md INPUTS block and Phase 3 step 4. Decided: 2026-05-17 (T-01 execution).

## 14. References

- **Upstream design spec:** [specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md) — §5.2 Intake phase (interface contract); §5.6 Persona model (intake-breadth asymmetry); §6 NFRs (60-second target, external-pointer durability); §7 Implementation Sequencing (Phase B definition).
- **Upstream schema feature spec (Phase A — done):** [specs/20260517-findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md) — the schema artifacts this skill consumes.
- **Schema artifacts (Phase A deliverables):**
  - [specs/findings/README.md](../findings/README.md) — field reference, state machine, status semantics.
  - [specs/findings/_template/finding.md](../findings/_template/finding.md) — canonical artifact template.
  - [specs/findings/_template/journal.md](../findings/_template/journal.md) — canonical journal template.
- **Living example (T-05 of Phase A):** [specs/findings/20260517-tab-display-issues/](../findings/20260517-tab-display-issues/) — reference for end-state Intake-phase artifact.
- **Constitution:** [specs/mission.md](../mission.md), [specs/tech-stack.md](../tech-stack.md), [specs/roadmap.md](../roadmap.md).
- **Spec-path convention:** [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) — directory layout this spec follows.
- **Peer skill artifacts (pattern references):**
  - [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) — structurally similar "transform input into per-spec artifact" skill.
  - [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md) — the skill that produced this feature spec.
  - [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — peer skill that also creates files in the operator's working tree.
