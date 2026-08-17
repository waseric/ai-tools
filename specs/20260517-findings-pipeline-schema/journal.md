# Findings Pipeline Schema — Journal

## 2026-05-17 — Feature Spec Authored

**Status:** draft — awaiting execution
**Artifact:** specs/20260517-findings-pipeline-schema/feature.md
**Upstream design spec:** specs/20260517-findings-pipeline/architecture.md (Phase A of §7 Implementation Sequencing)
**Origin:** Phase A execution following RC-1 design-freeze pass-with-comments verdict.

**Decisions made:**
- Phase D (investigation protocol) bundled into Phase A — the investigation section is a part of the finding template, not a separate artifact.
- Schema artifacts live at `specs/findings/` (consumer-facing location): `README.md`, `_template/finding.md`, `_template/journal.md`.
- `_template/` follows the `_archive/` underscore-prefix precedent from the spec-path convention.
- Single batched amendment (`2026-05-17-2`) for all 9 RC-1 advisories, per the RC-1 verdict's explicit recommendation. Not nine separate amendments.
- T-01 (the amendment) runs first — build the schema artifacts against the cleaned-up design spec.
- T-05 example-source exercise will use an operator-chosen recent journal note at execution time, not a pre-selected source.
- State-machine documentation: text + ASCII diagram (markdown-portable, no renderer required).
- Schema doc structure: prose narrative followed by field-reference table (readable + agent-consumable).

**Open questions surfaced and parked in §13:**
- OQ-1 (`_template/` exclusion from future validation): leaning yes. Deferred until automated validation is proposed.
- OQ-2 (does T-05 finding count toward Phase F adoption gate): leaning no — T-05 is validation, not a real routed finding. RC-5 reviewer to confirm.

**Tasks defined:** T-01 (amendment) → T-02 (README) → T-03 (finding template) → T-04 (journal template) → T-05 (example-source validation). Five tasks, all S or M, sequenced so each boundary is a safe stopping point.

**Conversation grounding:**
- Operator approved Phase 2 defaults: text + ASCII state machine, prose + field-table schema doc, single batched amendment, T-04 first.
- Discovery phase was light — design spec is comprehensive; constitution is small; conventions are well-established.

**Next task pointer:** Execute T-01 (`/spec-amend` for the batched RC-2 schema-pass amendment) via `/spec-execute`. On T-01 completion, proceed to T-02.

## 2026-05-17 — T-01: Apply batched amendment to design spec for 9 RC-1 advisories

**Status:** done
**Commits:** b974889 (amendment applied), bc46c07 (amendment journal entry in design spec)
**Files touched:**
- specs/20260517-findings-pipeline/architecture.md (28 insertions, 31 deletions across §4, §5.1, §5.3, §5.5, §5.6, §6, §13, §14)
- specs/20260517-findings-pipeline/journal.md (148 insertions — full amendment record appended)

**Tests added:** N/A — inspection-based per T-01's "Tests required" (re-read each advisory; verify corresponding change present in amended spec).

**DoD verification:**
- *Amendment applied:* Commit b974889 applies all ten edits (sub-changes A–J) covering all nine RC-1 advisories. Inspection confirms each advisory's corresponding section is amended.
- *Journal entry written:* Commit bc46c07 appends the full amendment record to the design spec's journal at specs/20260517-findings-pipeline/journal.md lines 101–247, preserving before/after diffs per advisory.
- *Commit message matches required form:* `spec: amendment 2026-05-17-2 — RC-2 schema-pass advisory batch` — verified on commit b974889.
- *Design spec status unchanged:* `Draft — Open for Review` per amendment's "Status implication: kept" — verified at specs/20260517-findings-pipeline/architecture.md status banner.

**Decisions made:** None new during execution — all nine advisory resolutions were pre-decided at feature-spec authoring time (per the "Per-advisory change summary" in [feature.md §7 T-01](feature.md#L208)). One execution-time refinement: sub-change H absorbed a §6 NFR-table addition (severity-axis decoupling row) as the natural home for OQ-1's converted decision, rather than placing it in §5.3 — the two locations were both proposed at spec time; §6 was selected as more discoverable.

**Spec amendments:** Amendment 2026-05-17-2 — recorded in [specs/20260517-findings-pipeline/journal.md](../20260517-findings-pipeline/journal.md#L101) as a first-class event. Renumbered OQ-2..5 → OQ-1..4 in the design spec; this feature spec's references to "OQ-1" (operational urgency) and "OQ-5" (external-pointer durability) at feature.md lines 159, 215, 216 now point to renumbered design-spec questions but remain semantically correct in context (the feature spec speaks of them as RC-1-era advisories). No follow-up edit required.

**Surprises and learnings:**
- T-01 closeout in this feature spec's journal was missed at amendment time — the amendment was journaled in the *design spec's* journal but not in the *feature spec's* journal. Repaired this session as the Phase 2 pre-flight verify gate before T-02 begins. Lesson: when an execution task is itself an amendment, both journals need an entry — the amendment record lives in the amended spec's journal, but the executing feature spec also needs its task closeout.
- The amendment touched one more section than originally enumerated (§5.3 dangling `See OQ-1` reference — sub-change D) for a total of ten edits across the nine advisories, because advisory 6 split cleanly into two distinct fixes (remove dangling reference + add severity-axis decoupling row). Net design substance unchanged.

**Next task pointer:** T-02 — create `specs/findings/README.md` (schema documentation). Dependency T-01 satisfied; no `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-02: Create `specs/findings/README.md` (schema documentation)

**Status:** done
**Commits:** (this commit)
**Files touched:**
- specs/findings/README.md (new file, 142 lines)
- specs/findings/ (new directory)

**Tests added:** N/A — inspection-based per T-02's "Tests required".

**DoD verification:**
- *File written:* specs/findings/README.md created with all six required sections (What this directory holds, State machine, Status semantics, Persona-frame taxonomy, Field reference, Creating a new finding).
- *Under ~200 lines:* 142 lines (verified via `wc -l`); leaves headroom for any RC-2-driven additions without breaching the NFR.
- *State-machine description matches amended design spec §4:* ASCII diagram includes the `reopened` back-transition (sub-change A); composition rules cite "append-only and forward-progressing" wording (sub-change B); route subtype → terminal status mapping paragraph mirrors sub-change E.
- *Persona-frame taxonomy matches amended §5.6:* uses standardized "orientation, not handoffs" wording (sub-change F first bullet); includes the intake-breadth asymmetry note as its own paragraph (sub-change F second bullet).
- *Field reference covers every template field:* 29-row table covers all fields from design spec §5.1 (architecture.md lines 118–164) — header block (Status, Domain, Severity, Operational urgency, Date opened, Last transition), Intake section (Reported by, Reported via, Captured by, Summary, External references), Triage section (Triaged by, Triage date, Reproducibility, Repro steps, Scope, Domain confirmation, Severity confirmation, Triage notes), Investigation section (Investigated by, Investigation date, Probable cause, Code/configuration touchpoints, Alternative hypotheses considered, Proposed remedy), Route section (Route decision, Decided by, Route date, Target spec, Route rationale). Title added as a 29th row (the H1 of the artifact, schema-load-bearing even though §5.1's template renders it as an implicit `# <Short title>` heading).
- *Cross-references valid:* verified `../20260517-findings-pipeline/architecture.md` and `../20260515-spec-path-convention/architecture.md` exist; verified `#7-implementation-sequencing` anchor resolves to `## 7. Implementation Sequencing` at architecture.md:300.
- *"Create a finding" how-to present:* four-step procedure with literal `cp` shell example; forward-pointer to Phase B `finding-intake` skill noted.

**Decisions made:**
- Field reference rendered as a 4-column table (Field / Type / Required by phase / Semantics) rather than a definition list, to satisfy the §6 NFR "AI-agent consumable: the field reference table in README.md uses a regular structure (column headers, one row per field) that an agent can parse without prose interpretation."
- Status semantics rendered per-status with Meaning / Persona-frame / Exit condition triplets — more readable than a single table, and accommodates the "Persona-frame: N/A" terminal-state cases honestly.
- Title field included as an explicit 29th row in the field reference, even though design spec §5.1 renders the title as an implicit H1 (`# <Short title> — Finding`). The README is a schema document; the title is part of the schema regardless of how the template syntactically expresses it. The T-03 template will need to instantiate the H1 with the title placeholder.

**Spec amendments:** None. Spec was sufficient as written post-T-01.

**Surprises and learnings:**
- The design spec's §5.1 template uses inline placeholder syntax for fields (`<text>`) rather than enumerating them in a table. Translating those into a structured field-reference table required some interpretive judgment about field naming (e.g., the unlabeled "Date:" rows under Triage and Investigation are disambiguated here as "Triage date" and "Investigation date" so the field reference table has unique names per row).
- The state-machine ASCII diagram in the README is rendered differently than the topology diagram in design spec §4 — the design spec's diagram covers the full pipeline (signals in, routes out), while the README's focuses narrowly on status transitions, which is what a status-machine consumer needs. Both diagrams are faithful to the same state machine; the choice was to keep the README's diagram tight and topology-free since the README's scope is the schema, not the full pipeline topology.
- The "Creating a new finding" section deliberately documents the *manual* path (copy + edit) rather than the future `finding-intake` skill path, since the skill does not yet exist (Phase B). When the skill ships, this section is the natural place to update with the skill invocation as the primary path and the manual copy as the fallback.

**Next task pointer:** T-03 — create `specs/findings/_template/finding.md`. Dependencies T-01, T-02 satisfied (T-02 produces the field set that T-03 instantiates). No `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-03: Create `specs/findings/_template/finding.md`

**Status:** done
**Commits:** 5872278 (deliverable), (this commit, closeout)
**Files touched:**
- specs/findings/_template/finding.md (new file, 65 lines)
- specs/findings/_template/ (new directory)

**Tests added:** N/A — inspection-based per T-03's "Tests required".

**DoD verification:**
- *File written:* specs/findings/_template/finding.md created.
- *Section structure matches design spec §5.1 verbatim:* Header block (Status / Domain / Severity / Operational urgency / Date opened / Last transition) + four phase sections (`## Intake`, `## Triage`, `## Investigation (optional)`, `## Route`) with field lists matching architecture.md:118-164.
- *Under ~200 lines:* 65 lines (verified via `wc -l`).
- *Committed:* 5872278.
- *Every README field-reference field present:* All 30 fields enumerated in T-02's field reference table appear in the template (verified field-by-field against specs/findings/README.md).
- *Operational urgency field present per OQ-1 → recorded decision (amendment sub-change H):* Yes, in the header block with `(optional)` marker and clarifying parenthetical "(typically operational findings only)".
- *Top-of-file HTML comment guides intake-first:* Yes — comment block instructs "Fill the Intake section first; later phases append" and cross-references README + design spec.

**Decisions made:**
- Verbatim mirror with one small clarifying gloss: the Operational urgency header line carries the parenthetical "(typically operational findings only)" inline rather than just `(optional)`. This is a minor surface-level addition that helps a first-time user understand when to populate the field; it is consistent with the amendment's severity-axis decoupling NFR row in design spec §6 and does not change the schema.
- HTML comment block includes a "Conventions when filling this template" sub-list (replace placeholders, use "unknown" honestly, leave future-phase sections in placeholder form, journal investigation-skip rationale). Goes slightly beyond the literal §5.1 template content but is supported by the design spec's intake behavior bullets (§5.2: "Unknown fields are recorded as 'unknown' rather than guessed") and §5.3–§5.4 (investigation may be skipped with journaled rationale).
- Did not include placeholder example values (e.g., "John Doe" for Reported by). Placeholders in `<angle brackets>` are clearer for both humans and AI agents than realistic-looking example values, which can be accidentally left in place.

**Spec amendments:** None.

**Surprises and learnings:**
- The §5.1 template's section heading `## Investigation (optional)` carries the `(optional)` marker as part of the heading itself — preserved verbatim. A new finding that skips investigation should still keep the section with placeholder values rather than delete it, so the finding's structure remains recognizable across reads. Recorded this guidance in the HTML comment.
- Followed the two-commit pattern (deliverable + closeout) per operator guidance after T-02's bundled commit. The deliverable commit (5872278) lands the file; this closeout commit lands the journal update. Pattern matches prior feature (spec-path-convention).

**Next task pointer:** T-04 — create `specs/findings/_template/journal.md`. Dependency T-02 satisfied (README documents the journaling pattern, and the structural precedent at specs/20260515-spec-path-convention/journal.md is on hand). No `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-04: Create `specs/findings/_template/journal.md`

**Status:** done
**Commits:** 08df0fd (deliverable), (this commit, closeout)
**Files touched:**
- specs/findings/_template/journal.md (new file, 84 lines)

**Tests added:** N/A — inspection-based per T-04's "Tests required".

**DoD verification:**
- *File written:* specs/findings/_template/journal.md created.
- *Under ~100 lines:* 84 lines (verified via `wc -l`).
- *Committed:* 08df0fd.
- *Structure matches feature-spec journal convention:* H1 `# <Short title> — Journal`; per-event `## <YYYY-MM-DD> — <Event>` sections; bolded inline fields. Matches precedent at specs/20260515-spec-path-convention/journal.md.
- *Placeholders clearly marked:* All values are `<angle-bracket>` placeholders, no plausible example values that could be accidentally left in place.
- *Documents one-entry-per-status-transition pattern:* Top HTML comment states the rule explicitly with design spec §6 Observability NFR citation. The skeleton entries below the active Intake entry instantiate the pattern.
- *Includes starter Intake entry with placeholders:* Active section "## <YYYY-MM-DD> — Intake" with Captured by, Signal source, New status, Notes fields aligned to finding.md Intake fields.

**Decisions made:**
- Included commented-out skeleton entries for the five subsequent transitions (Triaged / Under investigation / Investigation iteration / Routed / Closed / Reopened) at the bottom of the template. Goes beyond the literal T-04 scope ("Includes a starter 'Intake' entry with placeholders; documents the one-entry-per-status-transition pattern") but stays inside the spirit: the skeletons *are* how the pattern is documented — operationally, not just declaratively. Without them the operator has to construct each subsequent entry's shape from scratch, which invites drift from the convention.
- Added explicit `**Prior status:**` and `**New status:**` fields to each transition skeleton (other than the Intake starter, which has only `**New status:**` since intake originates the finding). This makes each entry self-documenting about the transition without requiring the reader to cross-reference the prior entry.
- Used HTML comments to hide the skeleton entries rather than including them as visible "examples". Hidden by default keeps the active journal clean for a freshly-copied template; the operator uncomments per transition. This is a small departure from the feature-spec journal precedent, which has no skeleton structure at all — feature-spec journals are written ad-hoc by the executing skill. Finding journals are more amenable to skeletoning because the transition shape is constrained by the state machine.

**Spec amendments:** None.

**Surprises and learnings:**
- The feature-spec journal pattern has no canonical template — feature-spec journals are produced ad-hoc by `spec-write` / `spec-execute` rather than copied from a template file. T-04 introduces the first template-driven journal in the methodology. The skeleton-entries-as-HTML-comments approach is the small innovation here; if it works well, it could backport to feature-spec journals (out of scope for this spec — flag for Phase F adoption review).
- The "Investigation iteration <N>" skeleton entry needed care to stay honest: status remains `under-investigation` across iterations, not "investigating-2" etc. Both `Prior status` and `New status` are `under-investigation` for an iteration in place, with a clarifying inline note "(iteration in place)" so the reader understands the status didn't change. Reflects design spec §5.4: "Investigation may iterate: a first investigation may produce a partial answer and journal 'needs deeper look'; status remains under-investigation until the route is chosen."

**Next task pointer:** T-05 — example-source validation exercise (operator-chosen recent journal note retroactively shaped as a finding under the templates). Dependencies T-02, T-03, T-04 satisfied. No `[blocker]` open questions; ready to proceed. Note: T-05 is a work-shape change — operator picks the source material, then exercises the templates end-to-end. Natural point for the session-continuity check.

## 2026-05-17 — T-05: Example-source validation exercise

**Status:** done
**Commits:** cd6ed9e (deliverable), (this commit, closeout)
**Files touched:**
- specs/findings/20260517-tab-display-issues/finding.md (new file, 62 lines)
- specs/findings/20260517-tab-display-issues/journal.md (new file, 29 lines)

**Tests added:** N/A — the exercise itself is the test, per T-05's "Tests required". Success criteria: template absorbed the content; state machine accommodated the example; no shape mismatch requiring a schema change.

**Source material:** Operator-supplied real-world signal — the Sandlot Minecraft community forum thread "All Servers - Tab Screen" at https://www.sandlotminecraft.com/threads/tab-screen.39849/, started Tuesday 2026-05-12 by NeonLights10927, with confirmations and additional symptoms from five other community members across the week. The thread captures a multi-faceted TAB-display bug with iterative in-band fixes — the kind of operational-domain signal the pipeline is designed to absorb. Stronger fit than the alternative candidates from internal feature-spec journals because it is genuinely external, multi-reporter, and mid-investigation.

**DoD verification:**
- *Real finding file exists in `specs/findings/`:* specs/findings/20260517-tab-display-issues/finding.md (62 lines) and journal.md (29 lines), both committed at cd6ed9e.
- *Validation outcome recorded in this feature spec's journal:* this entry. See "Validation observations" below.
- *Any schema gaps surfaced as findings or RC-2 amendments before checkpoint close:* No `[blocker]` schema gaps surfaced. Minor friction observations recorded below as journal observations for RC-2 reviewer attention, per T-05's "or as a journal observation for RC-2" allowance.
- *Acceptance criteria walked:* every section of finding.md populated, with honest `unknown` markers in fields where information was genuinely unknown (most of Investigation; all of Route). State machine accommodated intake → triaged → under-investigation in one session without distortion. No schema-change-requiring shape mismatch.

**Validation observations (what the exercise revealed):**

1. **Multi-reporter handling — friction, no schema change.** The template has a single `Reported by` field. The source had ~6 distinct reporters over a week. Workaround used: list initial reporter, list confirmers inline. The friction is real but mild; a schema change would over-specify (a "confirmers" field would be empty for most findings). Adequate as-is; consider documenting the convention in the README's "Creating a new finding" section if the pattern recurs.

2. **Multiple coupled symptoms in one finding — judgment call surfaced, no schema change.** The source documents four coupled visual bugs (rank icons, mod colors, AFK tags, Bedrock scoping). Whether to bundle them or split them into separate findings is a judgment the template does not guide. Bundling preserves iteration coherence but risks blurring distinct causes if investigation reveals them. Suggestion for RC-2 reviewer: a single-paragraph "when to bundle vs. split" guideline in the README would lower the cognitive load for the next intake.

3. **In-band iteration vs. linear pipeline — known property, validated.** The source thread shows fix-attempt → new-symptom → fix-attempt in real time within the same forum thread, not the clean intake → triage → investigation linear flow. The template and state machine handled this fine because the design spec's "orientation, not handoffs" framing (§5.6) and the `reopened` back-transition (amendment 2026-05-17-2 sub-change A) explicitly cover non-linear progression. Validates the design decision.

4. **Investigation-without-root-cause — `unknown` convention worked.** The source thread shows fixes applied without root-cause documentation. The finding's Investigation section landed most fields as `unknown` or hypothesis-only. The template absorbed this cleanly — `unknown` is a first-class value per design spec §5.2 and the recorded decision survives template instantiation. No schema change needed.

5. **Retroactive intake date — convention worked, mildly improvable.** The artifact's "Date opened" is the capture date (today, 2026-05-17), not the signal origination date (2026-05-12). The distinction was recorded in the journal Intake Notes. Mild friction: a future reader might assume "Date opened" = signal date. Possible schema enhancement: add a "Signal date" field separate from "Date opened" — defer until pattern recurs.

6. **Severity decomposition (severity vs. operational urgency) — validated.** The finding's `advisory + P3` combination accurately captures "low methodology severity, real but manageable operational urgency." Validates amendment 2026-05-17-2 sub-change H (severity-axis decoupling NFR row in design spec §6).

7. **State machine accommodated multi-transition single-session capture.** The journal records `intake → triaged → under-investigation` in three separate entries on the same date — the state machine permits this and the journal convention handles it via one-entry-per-transition. No friction.

**Spec amendments:** None. The schema is sufficient as-is for this example.

**Surprises and learnings:**
- The most informative validation outcome is *what didn't need to change*. The template absorbed a multi-reporter, multi-symptom, mid-investigation, externally-sourced operational signal end-to-end without requiring a schema change. This is stronger evidence of fitness than a polished example would have been — the friction points (#1, #2, #5 above) are exactly the friction points the design spec's RC-1 review anticipated, and they remain mild enough not to block adoption.
- The "Retroactive intake" pattern (capture-after-the-fact for signals that originated days or weeks ago) is itself a real intake mode not separately called out in the design spec. The template handled it by leaning on the journal Notes to record provenance. Worth noting for RC-2 reviewer: should this pattern get a one-line acknowledgment in §5.2 (Intake behavior)?
- All 30 README field-reference fields appeared in the finished finding artifact. Cross-check on T-02/T-03 agreement is confirmed end-to-end.

**RC-2 checkpoint triggered.** T-05 was the last task in the spec. Per [feature.md §9](feature.md#L329), RC-2 ("Schema Review") triggers when all five tasks complete and commits land. The checkpoint gate is now open. Review focus per design spec §9 RC-2: "Whether the artifact template is concrete and minimal; whether the state machine is unambiguous; whether the persona-frame fields carry their weight." Additional focus per this feature spec: whether the batched amendment (T-01) cleanly resolved all nine RC-1 advisories; whether the example-source exercise (T-05) surfaced any schema gaps. Exit criteria: T-05's example-source finding successfully populates the template (✅ confirmed above); the RC-1 advisory list is fully addressed (✅ amendment 2026-05-17-2 applied); no new `[blocker]` findings (✅ none surfaced).

**Next task pointer:** No further tasks in this feature spec. Awaiting RC-2 reviewer verdict via `/spec-review`. On RC-2 pass, this feature spec transitions to complete; Phase B (`finding-intake` skill) is the next phase per the design spec's §7 Implementation Sequencing.

## 2026-05-17 — Review of RC-2

**Reviewer:** waseric (self-review, with Claude as agent reviewer)
**Outcome:** pass with comments
**Tasks reviewed:** T-01, T-02, T-03, T-04, T-05
**Diff range:** `b362ffd..HEAD` (10 commits)
**Blockers:** 0
**Important:** 1 — README ASCII state diagram ([specs/findings/README.md](../findings/README.md) lines 17-26) depicts an unauthorized `routed → closed` transition that is not in design spec §5.5 (`routed` and `closed` are sibling terminal states, not sequential). Prose-form state machine at [README:36-74](../findings/README.md) is unambiguous and authoritative; the diagram needs cleanup before Phase B `finding-intake-skill` authoring so downstream consumers don't read an internally-inconsistent README.
**Spec amendments proposed:** None. All findings addressable within existing spec scope.
**Next action:** Clean up the README state diagram (redraw so `routed` and `closed` are visibly parallel terminal states; show both skip-investigation arrows `triaged → routed` and `triaged → closed`; keep the `reopened` loop). The seven advisory findings are good candidates for a small consolidation pass alongside the diagram fix. Once the cleanup lands, work may proceed to Phase B (author `finding-intake-skill` feature spec via `spec-write`, per [design spec §7 Phase B](../20260517-findings-pipeline/architecture.md#L306)).

### Full review record

#### Checkpoint contract

> Review focus per design spec: "Whether the artifact template is concrete and minimal; whether the state machine is unambiguous; whether the persona-frame fields carry their weight."
> Additional focus per feature spec: Whether the batched amendment (T-01) cleanly resolved all nine RC-1 advisories without introducing new inconsistencies; whether the example-source exercise (T-05) surfaced any schema gaps.
> Exit criteria per design spec: "Template usable for a real finding without further interpretation; state transitions all uniquely defined."
> Additional exit criteria: T-05's example-source finding successfully populates the template; the RC-1 advisory list is fully addressed; no new `[blocker]` findings.

#### Scope verification

All five tasks satisfied scope. T-01 touched §5.3, §5.5, §6 of the design spec in addition to the explicitly enumerated sections (§4, §5.1, §5.6, §13, §14) — the additions were journaled as legitimate execution-time expansions in the T-01 closeout (sub-change D removed a dangling `See OQ-1`; sub-change E added the route-subtype mapping paragraph in §5.5; sub-change H placed OQ-1's converted decision in a new §6 NFR row). No out-of-scope file changes; no new dependencies; all declared inspection-based tests performed.

#### Review focus walk

**Focus 1 — Template concrete and minimal:** **pass with comments**. Section structure mirrors design spec §5.1 verbatim; under-budget at 65 lines; top-of-file HTML comment guides intake-first. Two advisories: (a) field-name asymmetry between README field-reference (`Triage date`, `Triage notes`, `Route decision`, `Route rationale`) and template (bare `Date`, `Notes`, `Decision`, `Rationale`) — resolvable in context; (b) template HTML comment says "leave future-phase sections as placeholders" but the T-05 example fills Route with `unknown` while status is `under-investigation` — convention for unfilled-vs-unknown phases not authoritatively answered.

**Focus 2 — State machine unambiguous:** **pass with comments**. Terminal-status mapping paragraph at [README:36](../findings/README.md), per-status semantics block at [README:38-74](../findings/README.md), `reopened` back-transition, append-only forward-progressing wording, and six-status enum are all faithful to amended design spec §4-§5.5. One `[important]`: the ASCII state diagram depicts a `routed → closed` transition not in the design spec. One advisory: diagram is also missing the `triaged → routed` direct-skip path. Prose form is authoritative and unambiguous; diagram is the cleanup target.

**Focus 3 — Persona-frame fields carry their weight:** **pass**. README persona-frame taxonomy table preserves asymmetry-by-design wording from amendment sub-change F. T-05 example uses persona-frame explicitly in solo-operator-collapse case (`business analyst (with admin/operator overlap)` for triage; `developer (with admin overlap)` for investigation) — direct evidence that the field forces deliberate framing rather than degenerating into a label slot. Severity-axis decoupling (sub-change H NFR row) exercised: `advisory + P3` accurately captures "low methodology severity, real but manageable operational urgency." One advisory: template's `Triaged by` line lists persona-frame options without an explicit operator slot — the T-05 example correctly shows `operator; persona-frame: <...>` and the template could prompt that form.

**Additional focus — batched amendment cleanliness:** **pass**. All 9 RC-1 advisories accounted for in 10 sub-changes (advisory 6 split cleanly into D + H). Cross-reference updates handled (OQ-2..5 → OQ-1..4 renumbering propagated). Spec status correctly kept at `Draft — Open for Review`. RC-1's `[important]` (§5.8 dangling reference) was resolved by prior amendment 2026-05-17-1 (commit ecfba35).

**Additional focus — T-05 schema-gap detection:** **pass with comments**. Seven validation observations recorded; five are `[ok]` (multi-reporter handling; in-band iteration validated by reopened back-transition + "orientation not handoffs"; `unknown` first-class value worked; severity-axis decoupling exercised; multi-transition single-session capture). Two are `[advisory]`: (a) bundling-vs-splitting guidance is a real operator judgment with no template-side prompt — one paragraph in README "Creating a new finding" could fill this; (b) retroactive-intake date semantics — possible future "Signal date" field, deferred until pattern recurs.

#### Exit criteria walk

| Criterion | Evidence | Verdict |
|---|---|---|
| Template usable for a real finding without further interpretation | T-05 produced [specs/findings/20260517-tab-display-issues/finding.md](../findings/20260517-tab-display-issues/finding.md) end-to-end from an external real signal | met |
| State transitions all uniquely defined | [README:36 terminal-status mapping](../findings/README.md) + [README:38-74 per-status semantics](../findings/README.md) — prose form unambiguous; diagram has `[important]` caveat | met (via prose) |
| T-05 example-source finding successfully populates the template | finding.md (62 lines) + journal.md (29 lines) committed at cd6ed9e | met |
| RC-1 advisory list is fully addressed | Amendment 2026-05-17-2 record at [design-spec journal:101-247](../20260517-findings-pipeline/journal.md#L101) | met |
| No new `[blocker]` findings | Zero blockers in this review | met |

#### NFR pass

All §6 NFRs (discoverability, readability, self-contained, AI-agent consumable, backward compatibility, no new dependencies) verified clean. The field-name asymmetry advisory above is the only friction against AI-agent-consumable; not a strict NFR violation.

#### Test and documentation review

Inspection-based testing per task spec; all task closeout journal entries walk the inspection checks. T-05's "the exercise itself is the test" is the strongest validation. One documentation advisory: journal template HTML comment ([_template/journal.md:12](../findings/_template/journal.md)) declares section-header format `## <YYYY-MM-DD> — <New status>: <one-line summary>` but the starter Intake entry omits the colon and summary — convention/example inconsistency.

#### Summary of all findings

**Important (1):**
- README ASCII state diagram depicts unauthorized `routed → closed` transition ([specs/findings/README.md](../findings/README.md) lines 17-26).

**Advisory (7):**
- Field-name asymmetry README field-reference ↔ template (`Triage date`/`Triage notes`/`Route decision`/`Route rationale` vs bare `Date`/`Notes`/`Decision`/`Rationale`).
- Template HTML guidance "leave placeholders" vs T-05 example using `unknown` for unstarted Route phase — unfilled-vs-unknown convention not authoritatively answered.
- ASCII state diagram missing `triaged → routed` direct-skip path.
- README terminal-state `Persona-frame: N/A` vs Decided-by field requiring a persona-frame — context-resolvable but subtle.
- Multi-symptom bundling-vs-splitting guidance absent from README "Creating a new finding."
- Retroactive-intake date semantics — possible future "Signal date" field, deferred.
- Journal template section-header format (HTML comment) vs starter Intake entry — convention/example inconsistency.

## 2026-05-17 — RC-2 remediation closeout

**Status:** done
**Remediation commit:** 32d102e
**RC-2 verdict commit:** 8408a54
**Files touched:**
- specs/findings/README.md (state diagram redrawn; terminal-state persona-frame clarified; bundle-vs-split paragraph added)
- specs/findings/_template/finding.md (field names aligned with README; placeholder-vs-`unknown` convention formalized in HTML comment)
- specs/findings/_template/journal.md (starter Intake header and all skeleton entries aligned with the declared `## <date> — <status>: <one-line summary>` convention)
- specs/findings/20260517-tab-display-issues/finding.md (T-05 example brought into alignment: field names updated; Route section returned to placeholders since the phase has not started)
- specs/findings/20260517-tab-display-issues/journal.md (header format aligned)

**Per-finding disposition:**

| RC-2 finding | Class | Disposition | Where in 32d102e |
|---|---|---|---|
| Unauthorized `routed → closed` transition in ASCII state diagram | important | fixed — diagram redrawn with `routed`/`closed` as visibly parallel terminals; authoritative valid-transitions list added below the diagram | [README.md:16-48](../findings/README.md#L16) |
| Field-name asymmetry README ↔ template | advisory | fixed — template now uses `Triage date`/`Triage notes`/`Investigation date`/`Route decision`/`Route date`/`Route rationale` matching the README field-reference | [_template/finding.md:38-65](../findings/_template/finding.md) |
| Placeholder-vs-`unknown` convention | advisory | fixed — HTML comment formalizes the distinction: `<placeholder>` = phase not yet started; "unknown" = active-phase field investigated but indeterminate | [_template/finding.md:10-19](../findings/_template/finding.md) |
| Diagram missing `triaged → routed` skip path | advisory | folded into the diagram fix — explicit `triaged → routed` and `triaged → closed` edges in the valid-transitions list | [README.md:39-40](../findings/README.md#L39) |
| Terminal-state `Persona-frame: N/A` vs `Decided by` | advisory | fixed — per-status semantics for `routed` and `closed` now clarify that N/A applies to the terminal state itself; `Decided by` carries the deciding phase's persona-frame | [README.md:60-68](../findings/README.md#L60) |
| Bundle-vs-split guidance absent | advisory | fixed — "Creating a new finding" gains a paragraph on the bundle-vs-split judgment | [README.md:142-144](../findings/README.md#L142) |
| Retroactive-intake "Signal date" field | advisory | deferred per verdict — pattern has not recurred; reconsider if a second retroactive-intake finding surfaces | n/a |
| Journal template header format vs starter Intake | advisory | fixed — starter Intake header and all skeleton-entry exemplars now follow `## <date> — <status>: <one-line summary>`; T-05 example finding's journal aligned to match | [_template/journal.md:22-83](../findings/_template/journal.md#L22), [20260517-tab-display-issues/journal.md](../findings/20260517-tab-display-issues/journal.md) |

**Verification:**
- Re-reading the verdict's Important finding: the ASCII diagram no longer shows a `routed → closed` arrow; both terminals are drawn side-by-side; the prose-form state machine and the explicit valid-transitions list are aligned. Reviewer's stated remediation criterion ("redraw so `routed` and `closed` are visibly parallel terminal states; show both skip-investigation arrows `triaged → routed` and `triaged → closed`; keep the `reopened` loop") is satisfied — the skip arrows are surfaced via the authoritative valid-transitions list rather than as additional diagram strokes, which keeps the visual topology readable.
- Each advisory's disposition was verified against the verdict's `Summary of all findings` checklist; six addressed, one deferred per the verdict's own deferral note.

**Decisions made:**
- Adopted "diagram-topology + authoritative-transitions-list" pattern over fork-arrows-in-ASCII. The list is declared authoritative; the diagram intentionally simplifies. Trade-off: a reader scanning only the diagram still won't see `triaged → closed` or `under-investigation → routed` as edges, but the explicit list directly below catches anyone who needs the precise edge set. The earlier RC-2 verdict noted the prose form was already unambiguous; this remediation preserves that property by making the list (rather than the diagram) the authoritative reference.
- Updated the T-05 example finding's Route section from `unknown` to `<placeholder>` form. Strictly, the T-05 example was a faithful demonstration of the pre-remediation convention; post-remediation it must instead demonstrate the new convention. Treating the example as a living demonstration of the current schema (rather than as a frozen artifact) keeps it useful as a reference for future finding-intake skill development.
- Did not propose any further amendments to the design spec or feature spec. All RC-2 findings were schema-artifact-level cleanups within the scope of the deliverables produced under T-02/T-03/T-04 — no design substance changed.

**Surprises and learnings:**
- The "diagram shows topology; list is authoritative" pattern is a small but useful methodology contribution. ASCII state diagrams hit readability limits when transitions branch; deferring to an explicit transition list under the diagram preserves both readability and precision. Worth remembering for any future spec that needs to depict a non-linear state machine.
- The placeholder-vs-`unknown` distinction (Adv-2) is more load-bearing than it looked at first — it changes what a reader concludes from a finding's incomplete sections. With placeholders, "the work hasn't been done yet"; with `unknown`, "the work was done and produced no answer." Conflating them would have made findings harder to triage at scale. Worth promoting from convention-in-a-comment to a documented norm in the README's "Status semantics" if the methodology grows.
- The T-05 example's role straddles two purposes: validation-of-the-templates (its original purpose), and living-reference-for-future-consumers (an emergent purpose). RC-2 surfaced this tension implicitly: a one-shot validation artifact wouldn't need updating, but a living reference does. The methodology's existing "feature spec journals are append-only" discipline doesn't transfer cleanly to schema-deliverable example artifacts; this case sets a small precedent that example artifacts may be revised when the schema they exemplify is revised, with the revision journaled.

**Feature spec status:** complete. All five tasks (T-01 through T-05) done; RC-2 verdict pass-with-comments committed at 8408a54; remediation committed at 32d102e; all findings dispositioned. No further work in this feature spec.

**Next:** Phase B — author the `finding-intake-skill` feature spec via `/spec-write`, per [design spec §7 Implementation Sequencing](../20260517-findings-pipeline/architecture.md#7-implementation-sequencing). Recommended fresh session: Phase B is a clean scope break from this feature spec, and a fresh session benefits from re-reading the now-stable schema artifacts against a clean prompt cache rather than carrying this session's context forward.

## 2026-05-17 — Amendment 2026-05-17-1

**Section amended:** [specs/findings/_template/finding.md](../findings/_template/finding.md) leading HTML comment block (lines 1–22) + [specs/findings/_template/journal.md](../findings/_template/journal.md) leading HTML comment block (lines 1–18). Both files are Phase A deliverables of this schema feature spec.
**Trigger:** RC-3a review of the finding-intake skill (commit 20333e2) advisory A5 — templates don't self-describe their strip-from-artifact behavior, making correct intake handling depend on reading SKILL.md or relying on living-example precedent.
**Reason:** Three intake exercises (T-05 of this spec; T-02 and T-03 of the Phase B finding-intake-skill spec) all empirically stripped the leading HTML comments as scaffolding. SKILL.md just codified the distinction (Phase B amendment 2026-05-17-1, commit 1920e43). Adding a one-paragraph self-describing note inside each leading comment closes the loop: future operators or agents reading the templates directly will reach the right conclusion without needing to read SKILL.md.
**Impact summary:** No tasks affected (T-03 and T-04 of this spec created the templates as they were; this amendment refines a deliverable post-hoc); no checkpoints reopened (RC-2 closed; this doesn't change exit-criteria assessment); no completed work invalidated (T-05 of this spec and T-02/T-03 of Phase B are ratified, not invalidated); no cross-references to other sections need follow-up edits (feature.md §2/§5.2/§5.3 describe the templates purposively, not verbatim).
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** Schema feature spec stays at "Status: Draft — Open for Review" (no change). Templates have no Status banner.
**Commit:** this amendment closeout commit

### Full record

**Trigger.** RC-3a review of the finding-intake skill (commit 20333e2 on [specs/20260517-finding-intake-skill/journal.md](../20260517-finding-intake-skill/journal.md)) advisory A5. Three intake exercises (T-05 of this spec; T-02 and T-03 of the Phase B finding-intake-skill spec) all stripped the templates' leading HTML comment blocks on the empirical reading that those comments are operator-facing scaffolding (instructions for filling the template), not artifact content. SKILL.md just landed amendment 2026-05-17-1 (commit 1920e43) codifying this distinction in the finding-intake skill prose. Making the templates self-describe closes the loop: future operators or agents reading the templates directly will reach the right conclusion without needing to read SKILL.md or rely on living-example precedent.

**Section.**
- [specs/findings/_template/finding.md](../findings/_template/finding.md) lines 1–22 (leading HTML comment block).
- [specs/findings/_template/journal.md](../findings/_template/journal.md) lines 1–18 (leading HTML comment block).

Both files touched by the same coherent change ("templates self-describe their strip-from-artifact behavior"); per spec-amend's "one coherent change per amendment" principle, this is one amendment, not two.

**Change.**

`_template/finding.md` — appended a final paragraph immediately before the closing `-->` of the leading HTML comment block:

> ```
> This entire HTML comment block is template scaffolding — strip it from
> produced finding.md artifacts. See .agents/skills/finding-intake/SKILL.md
> Phase 3 step 2.
> ```

`_template/journal.md` — appended a final paragraph immediately before the closing `-->` of the leading HTML comment block:

> ```
> This leading HTML comment block AND the closing commented-out skeleton block
> at the end of this file are template scaffolding — strip both from produced
> journal.md artifacts. The skeleton entries are re-added (uncommented and filled)
> by downstream skills at the moment of each status transition. See
> .agents/skills/finding-intake/SKILL.md Phase 3 step 3.
> ```

The journal.md note is longer than the finding.md note because journal.md has *two* scaffolding regions (the leading comment block AND the closing commented-out skeleton block), while finding.md has only one (the leading comment block). The asymmetry preserves accuracy.

**Reason.** Three living examples converged on stripping the leading comment blocks as scaffolding, but a future operator or agent encountering the templates for the first time has no way to discover this without reading either SKILL.md or one of the existing examples. The amendment makes the templates self-describing: anyone reading the leading comment block now sees, at the bottom of that very block, "strip this when you produce the artifact." Reduces drift risk and decouples the templates from needing to be read alongside SKILL.md.

**Impact.**
- **Affected tasks:** None. T-03 (created _template/finding.md) and T-04 (created _template/journal.md) of this spec are closed; this amendment is a post-RC-2 refinement, not a re-execution. T-05 of this spec and T-02/T-03 of the Phase B spec used the templates and stripped the scaffolding — their behavior is ratified, not invalidated.
- **Affected checkpoints:** RC-2 (closed; this amendment does not change RC-2's exit-criteria assessment — the templates still match design spec §5.1 byte-for-byte at every artifact-bearing position; the new scaffolding paragraph lives inside the existing scaffolding region). RC-3 (joint Phase B + Phase C — still open; this amendment strengthens the schema's contribution to Phase C's eventual inheritance).
- **Completed work invalidated:** None.
- **Cross-references requiring follow-up:** None. Feature.md §2 Background (line 88), §5.2 (line 143), and §5.3 (line 161) all describe the templates' leading comments purposively rather than verbatim. The amendment grows the comment by one paragraph but does not contradict any of those descriptions. [specs/findings/README.md](../findings/README.md) "Manual fallback" section (lines 163–176) directs operators to copy the templates; after the amendment, a copy-following operator sees the new self-describing line as guidance, an upgrade not a contradiction.

**Status implication.** Schema feature spec stays at `Status: Draft — Open for Review` (no change). The templates themselves have no Status banner. The amendment is purely additive in the deliverables; no revert to Draft warranted.

**Approver.** waseric (2026-05-17).

## 2026-05-17 — Spec closeout — Phase A complete

**Status:** done
**Commits:** this closeout commit
**Files touched:**
- Edited: `specs/20260517-findings-pipeline-schema/feature.md` — status banner flipped from `Draft — Open for Review` → `Complete`.
- Edited: `specs/20260517-findings-pipeline-schema/journal.md` — this entry.

**Tests added:** None (housekeeping; inspection-based).

**DoD verification:**
- Status banner reflects post-RC-2 + post-amendment-1 state: ✓ — [feature.md:3](feature.md#L3) now `> Status: Complete`.
- All §7 tasks closed: ✓ — T-01 through T-05 all `done` in §7 with commit references; verified at RC-2 remediation closeout.
- No deferred follow-ups outstanding: ✓ — amendment 2026-05-17-1 explicitly noted "Cross-references requiring follow-up: None" (this journal:359). RC-2 remediation closeout dispositioned six advisories and deferred one ("Signal date" field per advisory's own deferral note — reconsider if a second retroactive-intake finding surfaces; not a banner-blocking follow-up).

**Decisions made:**
- **Status value: `Complete`.** Matches the precedent set by the Phase B closeout ([finding-intake-skill journal:405-431](../20260517-finding-intake-skill/journal.md#L405)) and the spec-path-convention feature spec.
- **No bundled content change.** The Phase B closeout (commit 45f693c) bundled a §5.2 stop-word cross-reference (the deferred follow-up from amendment 2026-05-17-3). This spec's amendment 2026-05-17-1 explicitly identified no follow-ups, so this closeout is banner-only — no §X edits piggybacked.

**Spec amendments:** None. This is closeout housekeeping; the spec is functionally unchanged.

**Surprises and learnings:**
- The schema spec was already operationally complete at the RC-2 remediation closeout (line 306: "Feature spec status: complete. ... No further work in this feature spec.") but the banner was not flipped at that moment because Phase B's amendments against this spec (the A5 advisory from RC-3a, applied as amendment 2026-05-17-1) were still pending. Flipping the banner after *both* RC-2 remediation *and* the post-RC-3a cross-side amendment landed avoids re-flipping it twice. This is the same pattern Phase B used at 45f693c.
- Two specs in the findings-pipeline tree are now `Complete`: this one (Phase A schema) and the Phase B finding-intake-skill. The upstream design spec ([specs/20260517-findings-pipeline/architecture.md](../20260517-findings-pipeline/architecture.md)) remains `Draft — Open for Review`; its RC-3 (joint Phase B + Phase C review) is still open and will close only after Phase C's `finding-triage` skill spec exists and passes review.

**Next pointer:** **Outside this spec.** The natural next step is `/spec-write` for Phase C — the `finding-triage` skill — against the upstream [findings-pipeline design spec](../20260517-findings-pipeline/architecture.md). Phase A's contribution to the design-spec RC-3 is now sealed alongside Phase B's.

## 2026-05-17 — Amendment 2026-05-17-2

**Section amended:** specs/20260517-findings-pipeline-schema/feature.md §6 NFRs (appended new `Skill portability` row); §5.2 finding-template subsection (replaced top-of-file-comment example with scaffold-marker example; added bundled-template / host-override clarification); §5.3 journal-template subsection (same treatment). Cascading file edits to specs/findings/_template/finding.md and specs/findings/_template/journal.md (wrapped scaffolding blocks with `<!-- scaffold-start -->` / `<!-- scaffold-end -->` markers; updated self-describing text to reference the marker mechanism).
**Trigger:** Cascading from Amendment 2026-05-17-5 to the design spec (commit `c1418b0`), which committed a `Skill portability` NFR referencing the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle) in tech-stack.md. This amendment implements the principle's operational choices at the schema level.
**Reason:** The existing scaffolding strip mechanism (line-numbers hardcoded into `finding-{intake,triage}/SKILL.md`) is the load-bearing surface of the originating finding [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/finding.md). Marker delimiters fix both coupling axes (templates carry their own boundaries; skills strip by marker, not by line number). Templates are also explicitly committed as skill-bundled-with-host-override so that a globally installed skill ships its own defaults rather than expecting the host repo to provide them.
**Impact summary:** No tasks re-opened (T-01 through T-05 stay closed); no checkpoints re-opened (RC-2 stays closed). Cascading amendments 4 and 5 bring the two SKILL.md files into conformance with the new marker mechanism. Until those land, the line-number-based strip instructions in the SKILL.md files mis-match the new marker-wrapped templates — handled in-session as part of this cascade.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept at `Complete`. The amendment reverses no design decision, re-opens no §7 task, and is cascading conformance work flowing from a higher-level methodology principle just committed in tech-stack.md. Explicit operator confirmation captured during the amendment session: "concur. Keep at complete."
**Commit:** `0923dda`

### Full record

**Trigger.** Cascading from Amendment 2026-05-17-5 to the design spec (commit `c1418b0`), which committed the **Skill portability** NFR row referencing the Atomic-Skill Portability Principle. This amendment implements the principle's operational choices at the schema level: scaffold-marker delimiters replacing the line-number strip mechanism currently hardcoded in `finding-{intake,triage}/SKILL.md`; templates committed as skill-bundled defaults with the host project's `_template/` (when present) taking precedence as override; README.md role explicit in the schema's own NFR table.

**Section(s).** [specs/20260517-findings-pipeline-schema/feature.md](feature.md) §6 NFRs (L187–L196 pre-amendment, appended new row); §5.2 (L143–L159 pre-amendment, replaced); §5.3 (L161–L177 pre-amendment, replaced). Plus cascading file edits to [specs/findings/_template/finding.md](../findings/_template/finding.md) and [specs/findings/_template/journal.md](../findings/_template/journal.md).

**Change.** Five changes bundled as one amendment:

1. §6 NFR table — appended new `Skill portability` row pointing at the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle) and committing: (a) schema artifacts are canonical reference but not load-bearing runtime inputs; (b) skills bundle their own copies as defaults; (c) host project's `_template/` (when present) takes precedence as override; (d) scaffolding inside templates is delimited by `<!-- scaffold-start -->` / `<!-- scaffold-end -->` markers; (e) README.md is the schema's derived human-readable projection, not a runtime input.
2. §5.2 prose — replaced top-of-file-comment example with scaffold-marker example; added bundled-template / host-override clarification at the Purpose level; added explicit "skills strip everything from scaffold-start through scaffold-end inclusive" rule.
3. §5.3 prose — same treatment for the journal template; explicit recognition that the closing skeleton block is also a scaffold-marker-delimited block.
4. [specs/findings/_template/finding.md](../findings/_template/finding.md) — wrapped the leading HTML comment block with `<!-- scaffold-start -->` and `<!-- scaffold-end -->`; rewrote the self-describing scaffolding text to reference the marker mechanism rather than SKILL.md line-number references.
5. [specs/findings/_template/journal.md](../findings/_template/journal.md) — wrapped both the leading HTML comment block AND the closing commented-out skeleton block with the same scaffold markers; rewrote self-describing scaffolding text similarly.

Full before/after diffs are in the Phase 2 draft of this amendment in the calling session; the After blocks have been applied verbatim.

**Reason.** The existing scaffolding mechanism (templates with HTML-comment blocks at known line ranges, stripped by SKILL.md using hardcoded line numbers like "finding.md 1–22; journal.md 1–18 and 29–84") is the load-bearing surface of the originating finding. Two coupled problems: (a) the line numbers couple SKILL.md to the *current* template line layout — any template author edit shifts the numbers and breaks the skills silently; (b) the SKILL.md references to specific line ranges encode template-shape knowledge into the workflow layer, against the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle). Marker delimiters fix both: the templates carry their own scaffolding boundaries; skills strip by marker without knowing line numbers. Templates are also explicitly committed as skill-bundled-with-host-override so that a globally installed skill ships its own defaults rather than expecting the host repo to provide them.

**Impact.**
- **Affected tasks:** T-03 and T-04 in this spec describe creating the template files. Those tasks are already done (templates exist; spec at `Status: Complete`). This amendment retroactively updates the template-file content and the spec's description of the template structure to use scaffold markers. The tasks themselves remain marked done; no re-execution required.
- **Affected checkpoints:** RC-2 (Schema Review) already closed. This is additive clarification + a strip-mechanism conversion that does not change the schema's field set or state machine; no re-review needed.
- **Completed work invalidated:** No. The `_template/` files are modified in place; the schema's information content is unchanged. The `finding-{intake,triage}` SKILL.md files are non-compliant with the new marker mechanism and will be brought into conformance by Amendments 4 and 5 — the line-range-based strip instructions are replaced with marker-based strip instructions.
- **Cross-references requiring follow-up:** Amendments 4 ([finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md) + [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md)) and 5 ([finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) + [.agents/skills/finding-triage/SKILL.md](../../.agents/skills/finding-triage/SKILL.md)). Both must be landed before the new marker mechanism is fully operational — until they land, the SKILL.md files still reference line numbers and would mis-strip if invoked.

**Status implication.** Spec stays at `Status: Complete`. The amendment is operational (changes a mechanism + adds an NFR row); no design decision is reversed and no task is re-opened. Pattern matches the amendment-against-terminal-status discipline: an amendment that does not reverse a design decision and does not re-open any task does not require reverting to Draft. Explicit operator confirmation captured during the amendment session.

**Approver.** waseric — approved as drafted on 2026-05-17, with explicit confirmation to keep the spec at `Complete` after surfacing the status discrepancy between the Phase 2 draft (which incorrectly said "stays at Draft") and the actual current status.
