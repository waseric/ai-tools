# Finding Triage Skill — Journal

## 2026-05-17 — Feature Spec Authored

**Status:** draft — awaiting execution
**Artifact:** specs/20260517-finding-triage-skill/feature.md
**Upstream design spec:** specs/20260517-findings-pipeline/architecture.md (Phase C of §7 Implementation Sequencing)
**Upstream sibling feature specs:**
- Phase A (done): specs/20260517-findings-pipeline-schema/feature.md (RC-2 passed; remediated 2026-05-17)
- Phase B (done): specs/20260517-finding-intake-skill/feature.md (RC-3a passed 2026-05-17)

**Origin:** Phase C execution following Phase B's RC-3a closeout. Phase A delivered the schema; Phase B authored `finding-intake` and produced two real intake artifacts; Phase C now authors `finding-triage` that consumes them.

**Decisions made:**
- Phase C scope is **skill + README integration**, mirroring Phase B's shape. Four tasks: T-01 (SKILL.md), T-02 (synthetic exercise — triage the existing test-only fixture), T-03 (real-signal dogfood — triage the LWC shelves error finding), T-04 (README "Triaging a finding" section). Operator confirmed all four recommendations in Phase 2 Clarify.
- **Skip-investigation surface is in scope.** The skill supports direct transition from `triaged` to `routed` / `closed` when the route is obvious — design spec §5.3 explicit support. Adds ~30 lines to the skill but completes the Phase C interface contract.
- **OQ-4 resolution: optional revalidation, soft default.** The skill suggests checking pointers when intake Summary is sparse, treats as static otherwise. Records the decision per pointer in the journal entry. This is the design-spec leaning, codified in this spec's §5.4 and to be encoded in SKILL.md Phase 2.
- **OQ-3 resolution: descriptive persona-frame.** The skill suggests a frame derived from `Domain` (business analyst / security analyst / QA lead / methodologist) and accepts free-text override. No fixed enum. Codified in this spec's §5.3 and to be encoded in SKILL.md Phase 2.
- **T-03 dogfood target: LWC shelves error finding.** Real signal at `status: intake`, operational domain, pointer-with-snapshot — natural Phase C dogfood target. Avoids the cost of producing a fresh real signal.
- **T-04 README integration.** Add "Triaging a finding" section paralleling "Creating a new finding." Discoverability for `/finding-triage` in the same place `/finding-intake` is documented.
- **Internal review checkpoint named RC-3b** to disambiguate from the design-spec-level RC-3 (joint Phase B + Phase C review). RC-3b closes when Phase C is shippable; the design-spec RC-3 then closes since Phase B's RC-3a already passed.
- **No 60-second NFR.** Triage is hard-facts work; the design constraint is completeness + persona-frame discipline, not speed.
- **Validation: synthetic (T-02) + real-signal dogfood (T-03).** Same shape as Phase B; T-02 catches mechanics bugs without burning a real signal, T-03 verifies real-world fit.
- **Three internal open questions parked in §13** — rich/sparse Summary heuristic prose; persona-frame label format; reproducing-without-opening-code prose — all decidable at T-01 execution time per Phase B precedent.

**Open questions surfaced and parked in §13:**
- OQ-1 (rich-vs-sparse Summary heuristic prose): leaning one-sentence heuristic + two examples. Decided at T-01.
- OQ-2 (persona-frame label format on the artifact): leaning `<name>; <descriptive frame>; persona-frame: triage`. Decided at T-01.
- OQ-3 (reproducing without opening code): leaning yes — running code to reproduce is allowed; "stay out of code" means don't read implementation. Decided at T-01; phrased in OPERATING PRINCIPLES.

**Design-spec OQ resolutions claimed by this spec:**
- **OQ-3** (multi-domain persona naming) — resolved as option (c) descriptive recording. Codified in this spec's §5.3 and to be encoded in SKILL.md. Will be quoted back to the design spec via `/spec-amend` *after* RC-3 closes (out of scope for this feature spec — see §12).
- **OQ-4** (triage-time external-pointer revalidation policy) — resolved as optional, soft default with journaled decision. Codified in this spec's §5.4 and to be encoded in SKILL.md. Same quoting-back plan as OQ-3.

**Tasks defined:** T-01 (skill artifact, M) → T-02 (synthetic exercise, S) → T-03 (real-signal dogfood, S) → T-04 (README integration, S). Four tasks, all S or M, sequenced so each boundary is a safe stopping point: T-01 is the deliverable; T-02 verifies mechanics; T-03 verifies real-world fit; T-04 lands the README change only after dogfood succeeds.

**Conversation grounding:**
- Operator invoked via `/spec-write finding-triage` against the existing findings-pipeline working directory. Confirmed at Discovery time that Phase A and Phase B are closed (RC-2 verdict, RC-3a verdict, Phase A and Phase B status set to Complete in their respective feature specs).
- Operator answered four Phase 2 questions with all-recommended options: skip-investigation surface allowed; OQ-4 optional-with-soft-default; T-03 dogfood targets LWC finding; T-04 adds README section.
- Discovery confirmed two intake-status findings exist (synthetic fixture, LWC shelves error) and one under-investigation finding (tab-display) — the LWC finding is the natural T-03 target.
- Sibling skill patterns: `finding-intake` (153 lines, 12 structural sections) is the closest reference. Target this spec's deliverable at 180–220 lines.

**Next task pointer:** Execute T-01 (`.agents/skills/finding-triage/SKILL.md`) via `/spec-execute`. Dependencies satisfied (Phase A schema artifacts + Phase B intake skill + two real intake-status findings are committed and stable). No `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-01: Author `.agents/skills/finding-triage/SKILL.md`

**Status:** done
**Commits:** `a8f5674` (SKILL.md authored). Session-shouldered companion commits in the same session: `b3a0f94` (spec authorship belatedly committed at orient time — see "Surprises and learnings" below), `d764c08` (intake finding `spec-write-leaves-specs-uncommitted` captured at operator's direction during orient).
**Files touched:** `.agents/skills/finding-triage/SKILL.md` (new, 197 lines).
**Tests added:** none — methodology repo, markdown-only deliverable. Verification is by inspection (T-01 Tests-required section): line count, twelve §5.1 structural sections, frontmatter parseable with required cross-references, all eighteen §4 INPUTS fields present, all eight WHAT NOT TO DO anti-goals enumerated, state-machine pre-condition guard in Phase 1, two-journal-entry skip-route discipline in Phase 3.
**DoD verification:**
- File written ✓ (`.agents/skills/finding-triage/SKILL.md`, 197 lines).
- ≤220 lines (`wc -l` = 197) ✓.
- Committed ✓ (`a8f5674`).
- Twelve §5.1 structural sections present ✓ — YAML frontmatter; `# Finding Triage` title; three opening paragraphs; `## How this skill works`; `## INPUTS`; `# ROLE`; `# OPERATING PRINCIPLES`; `# PHASE 1 — ORIENT`; `# PHASE 2 — DRAFT`; `# PHASE 3 — APPLY`; `# OUTPUT FORMAT`; `# WHAT NOT TO DO`.
- All 18 §4 INPUTS fields present ✓ — including the conditional skip-investigation set (`SKIP_INVESTIGATION`, `ROUTE_DECISION`, `ROUTE_RATIONALE`, `TARGET_SPEC`, `WATCH_CONDITION`, `CLOSE_REASON`).
- All 8 anti-goals enumerated ✓ — no opening code; no rewriting Intake; no inventing reproducibility; no skipping persona-frame; no silent revalidation; no auto-commit; no re-triage of non-intake; no causal hypotheses in Triage.
- State-machine pre-condition guard ✓ — Phase 1 ORIENT contains an explicit error message and "exit without artifact mutation" wording.
- Two-journal-entry skip-route discipline ✓ — Phase 3 APPLY step 4 plus WHAT NOT TO DO bullet 6 both name "not collapsed; both entries written, in order, `Triaged` first."
- Frontmatter description references `Findings Pipeline`, `[[finding-intake]]`, `[[spec-amend]]`, `[[spec-write]]` ✓.
- OPERATING PRINCIPLES count: 8 (spec range 7–9) ✓.

**Decisions made:**
- **D-1** (was OQ-1, rich-vs-sparse heuristic prose): one-sentence rule + two grounded examples. Phrased in SKILL.md Phase 2. Closing line preserves operator judgment as the load-bearing authority.
- **D-2** (was OQ-2, persona-frame label format): `<name>; <descriptive frame>; persona-frame: triage`. Encoded in Phase 2 derivation and Phase 3 step 1 (`Triaged by` field).
- **D-3** (was OQ-3, reproducing without opening code): running code to reproduce is allowed; reading code to hypothesize is not. Placed in three locations (opening prose, OPERATING PRINCIPLE #2, WHAT NOT TO DO bullet 1) so the discipline is visible regardless of which section the agent loads first.

All three resolutions match the §13 leanings; no leaning was overturned at execution time. Recorded in §13a Decisions.

**Spec amendments:** none. T-01 came together cleanly within the §4 / §5.1 / §7 contracts; no spec edits required beyond the §7 T-01 status line and the §13 → §13a Decisions conversion.

**Surprises and learnings:**
- **Spec authorship was not committed by `/spec-write`** — `specs/20260517-finding-triage-skill/` was found untracked at orient. This is the second consecutive occurrence (Phase B exhibited the same pattern on 2026-05-16). Remediated at `b3a0f94` before opening T-01; pattern captured as intake finding `specs/findings/20260517-spec-write-leaves-specs-uncommitted/` at commit `d764c08`. The intake's triage and route decisions are out of scope for this spec (the finding now lives in the Findings Pipeline and will be triaged via `/finding-triage` itself in a later session — possibly a fitting dogfood for Phase F if not sooner).
- **`finding-intake` SKILL.md template line ranges are stale** — the SKILL cites "template lines 1–22" and "lines 1–18" / "lines 29–84" but the actual comment blocks have shifted in the current templates. Spirit of the rule (strip the leading HTML comment block; strip the closing commented-out skeleton block) is unambiguous and was followed. Noted in the spec-write-leaves-specs-uncommitted intake's journal Notes as a minor methodology observation; not load-bearing for this T-01 closeout, not raised as its own finding.
- **Phase 3 APPLY ordering in SKILL.md** — the spec's §4 implies the status banner update happens early, but in the skill I sequence "populate Triage section first, then update status banner, then append journal entry, then handle skip-route." This is the natural commit order (the Triage section is the load-bearing content; the banner is a scan-aid; the journal is the audit trail) and matches Phase B's analogous APPLY ordering. No spec deviation; just naming for the next executor.

**Next task pointer:** T-02 — Synthetic validation exercise (triage the existing [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/) using the just-authored skill). Dependencies satisfied (T-01 complete; synthetic fixture present at `status: intake`). Estimated size: S.

## 2026-05-17 — T-02: Synthetic validation exercise — finding-triage skill

**Status:** done
**Commits:** `f628db2` (synthetic fixture triaged — finding.md + journal.md edits). Spec closeout commit follows.
**Files touched:** [specs/findings/20260517-test-only-signal-synthetic-fixture/finding.md](../findings/20260517-test-only-signal-synthetic-fixture/finding.md), [specs/findings/20260517-test-only-signal-synthetic-fixture/journal.md](../findings/20260517-test-only-signal-synthetic-fixture/journal.md).
**Tests added:** none — methodology repo, prose-review discipline. Verification by inspection per spec §8 Test Strategy.
**DoD verification:**
- Synthetic fixture triaged ✓ — `Status: triaged`; Triage section populated; Triaged journal entry appended.
- Intake section byte-for-byte preserved ✓ — verified by `git show HEAD:.../finding.md` diff against post-edit content (zero differences in the Intake block).
- All seven Triage fields populated ✓ — Triaged by, Triage date, Reproducibility, Repro steps, Scope, Domain confirmation, Severity confirmation, Triage notes.
- Status banner updated ✓ — `Status: triaged`; `Severity: advisory` (first-population, not delta); `Last transition: 2026-05-17`; operational urgency placeholder retained for methodology domain per SKILL Phase 3 step 2.
- Triaged journal entry has all 8 prescribed fields ✓ — Triaged by, Prior status (`intake`), New status (`triaged`), Reproducibility outcome, Domain/severity changes, Skip-investigation decision (`end at triaged`), Pointer revalidation (`not-applicable`), Notes.
- Persona-frame derivation correct ✓ — Domain `methodology` → suggested `methodologist`; suggestion accepted unchanged in structured-input mode.
- Pointer-revalidation default correct ✓ — `not-applicable` for the fixture (no external pointer).
- Hard-facts discipline held ✓ — Triage section contains observational facts only; the route-amend hypothesis is recorded in the journal entry's Skip-investigation field, not in the finding's Triage section.
- State-machine guard verified by inspection ✓ — SKILL Phase 1 ORIENT enumerates `triaged` among rejected statuses; the fixture is now at `Status: triaged`, so any future `/finding-triage` invocation against this path would hit the Phase 1 check, emit the error, and exit.

**Decisions made:**
- **Fixture retain-vs-delete:** retain. The fixture transitions permanently from `intake` to `triaged`; it continues to serve as the T-02 regression reference (now end-state-Triaged) paralleling [20260517-tab-display-issues/](../findings/20260517-tab-display-issues/) (end-state-Investigation). The spec explicitly allows this outcome (§7 T-02 closing paragraph).
- **Mode:** structured-input. No interactive prompts were issued — all inputs derivable from the fixture's intake-time fields plus session defaults. Chosen because the agent (Claude) is operating on the operator's behalf in the same session that authored the skill; the round-trip would have been ceremony.
- **End at `triaged` (not skip-route):** per §7 T-02 default-expected-outcome framing. The skip-investigation surface is therefore not exercised in T-02 — that exercise is left for T-03 (the operator may choose skip-route in the real-signal dogfood if triage is sufficient to route directly).

**Spec amendments:** none. T-02 closed cleanly within the §7 T-02 acceptance criteria and the SKILL.md as authored at T-01. No surfaced bugs requiring T-01 amendment; no surfaced gaps requiring schema amendment.

**Surprises and learnings:**
- **Skip-route surface remains unexercised after T-02.** The spec's T-03 also allows ending at `triaged` as the default expected outcome. If both T-02 and T-03 end at `triaged`, the skip-route path (two-journal-entry discipline, status `routed`/`closed` transition, Route section population) is verified by inspection only — never executed end-to-end against a real artifact during the spec's task breakdown. RC-3b reviewer may want to flag this; or T-03 may opt-in to skip-route to close the gap. Surfaced for operator awareness at the T-02 → T-03 boundary, not raised as an amendment.
- **Naive `<placeholder>` detection is too coarse.** A by-inspection pass to verify "no remaining placeholders in Triage section" using `re.findall(r'<[a-z]', ...)` initially flagged the *legitimate* `<short-name>` token inside the Repro step prose (which describes the `YYYYMMDD-<short-name>/` directory convention). A future CI-style validator (parked in [findings-pipeline-schema feature.md §13 OQ-1](../20260517-findings-pipeline-schema/feature.md#L373)) will need a structured schema, not regex over angle-bracket patterns. Informational; not raised as its own finding.
- **Parallel work on the branch:** between T-02 content commit (`f628db2`) and this closeout, the operator captured [specs/findings/20260517-intake-template-folder-dependency/](../findings/20260517-intake-template-folder-dependency/) at commit `e0b0a32` — a new intake finding about the `finding-intake` skill's two-level dependency on the host repo's `_template/` folder (path coupling + line-number coupling for the comment-block stripping). This subsumes the "stale line ranges" observation I flagged in the T-01 journal entry's "Surprises and learnings." Partial relevance to the `finding-triage` skill: my SKILL.md has the same `../../../specs/findings/_template/` path coupling (Phase 1 ORIENT step 1), but does *not* have the line-number coupling — `finding-triage` appends entries to existing finding/journal files rather than scaffolding new ones from the templates, so it doesn't strip line ranges. The path-dependency half therefore applies to both skills; the line-range half is intake-only. Both halves are out of scope for this spec — they're properties of the intake skill and the global-install vs. colocated-install question, not of finding-triage. The new finding will get its own triage/route via the pipeline.

**Next task pointer:** T-03 — Real-signal dogfood exercise (triage the existing [specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/) using the just-validated skill). Dependencies satisfied (T-01 complete; T-02 complete; LWC finding present at `status: intake` with external pointer + operator-supplied PDF snapshot). The LWC finding's `operational` domain will exercise the persona-frame override question (suggested `business analyst`; operator may name `Sandlot administrator` or similar) and the pointer-revalidation policy (one pointer present, intake Summary judged rich → default `treated-as-static` is the soft default). Operator may opt-in to skip-route to exercise the unexercised two-journal-entry path. Estimated size: S.
