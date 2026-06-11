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
**Commits:** `9845b42` (SKILL.md authored). Session-shouldered companion commits in the same session: `c29f23b` (spec authorship belatedly committed at orient time — see "Surprises and learnings" below), `21d5765` (intake finding `spec-write-leaves-specs-uncommitted` captured at operator's direction during orient).
**Files touched:** `.agents/skills/finding-triage/SKILL.md` (new, 197 lines).
**Tests added:** none — methodology repo, markdown-only deliverable. Verification is by inspection (T-01 Tests-required section): line count, twelve §5.1 structural sections, frontmatter parseable with required cross-references, all eighteen §4 INPUTS fields present, all eight WHAT NOT TO DO anti-goals enumerated, state-machine pre-condition guard in Phase 1, two-journal-entry skip-route discipline in Phase 3.
**DoD verification:**
- File written ✓ (`.agents/skills/finding-triage/SKILL.md`, 197 lines).
- ≤220 lines (`wc -l` = 197) ✓.
- Committed ✓ (`9845b42`).
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
- **Spec authorship was not committed by `/spec-write`** — `specs/20260517-finding-triage-skill/` was found untracked at orient. This is the second consecutive occurrence (Phase B exhibited the same pattern on 2026-05-16). Remediated at `c29f23b` before opening T-01; pattern captured as intake finding `specs/findings/20260517-spec-write-leaves-specs-uncommitted/` at commit `21d5765`. The intake's triage and route decisions are out of scope for this spec (the finding now lives in the Findings Pipeline and will be triaged via `/finding-triage` itself in a later session — possibly a fitting dogfood for Phase F if not sooner).
- **`finding-intake` SKILL.md template line ranges are stale** — the SKILL cites "template lines 1–22" and "lines 1–18" / "lines 29–84" but the actual comment blocks have shifted in the current templates. Spirit of the rule (strip the leading HTML comment block; strip the closing commented-out skeleton block) is unambiguous and was followed. Noted in the spec-write-leaves-specs-uncommitted intake's journal Notes as a minor methodology observation; not load-bearing for this T-01 closeout, not raised as its own finding.
- **Phase 3 APPLY ordering in SKILL.md** — the spec's §4 implies the status banner update happens early, but in the skill I sequence "populate Triage section first, then update status banner, then append journal entry, then handle skip-route." This is the natural commit order (the Triage section is the load-bearing content; the banner is a scan-aid; the journal is the audit trail) and matches Phase B's analogous APPLY ordering. No spec deviation; just naming for the next executor.

**Next task pointer:** T-02 — Synthetic validation exercise (triage the existing [specs/findings/20260517-test-only-signal-synthetic-fixture/](../findings/20260517-test-only-signal-synthetic-fixture/) using the just-authored skill). Dependencies satisfied (T-01 complete; synthetic fixture present at `status: intake`). Estimated size: S.

## 2026-05-17 — T-02: Synthetic validation exercise — finding-triage skill

**Status:** done
**Commits:** `dc87ba1` (synthetic fixture triaged — finding.md + journal.md edits). Spec closeout commit follows.
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
- **Parallel work on the branch:** between T-02 content commit (`dc87ba1`) and this closeout, the operator captured [specs/findings/20260517-intake-template-folder-dependency/](../findings/20260517-intake-template-folder-dependency/) at commit `7c61bca` — a new intake finding about the `finding-intake` skill's two-level dependency on the host repo's `_template/` folder (path coupling + line-number coupling for the comment-block stripping). This subsumes the "stale line ranges" observation I flagged in the T-01 journal entry's "Surprises and learnings." Partial relevance to the `finding-triage` skill: my SKILL.md has the same `../../../specs/findings/_template/` path coupling (Phase 1 ORIENT step 1), but does *not* have the line-number coupling — `finding-triage` appends entries to existing finding/journal files rather than scaffolding new ones from the templates, so it doesn't strip line ranges. The path-dependency half therefore applies to both skills; the line-range half is intake-only. Both halves are out of scope for this spec — they're properties of the intake skill and the global-install vs. colocated-install question, not of finding-triage. The new finding will get its own triage/route via the pipeline.

**Next task pointer:** T-03 — Real-signal dogfood exercise (triage the existing [specs/findings/20260517-easy-survival-shelves-lwc-error/](../findings/20260517-easy-survival-shelves-lwc-error/) using the just-validated skill). Dependencies satisfied (T-01 complete; T-02 complete; LWC finding present at `status: intake` with external pointer + operator-supplied PDF snapshot). The LWC finding's `operational` domain will exercise the persona-frame override question (suggested `business analyst`; operator may name `Sandlot administrator` or similar) and the pointer-revalidation policy (one pointer present, intake Summary judged rich → default `treated-as-static` is the soft default). Operator may opt-in to skip-route to exercise the unexercised two-journal-entry path. Estimated size: S.

## 2026-05-17 — T-03: Real-signal dogfood — LWC shelves error triaged

**Status:** done
**Commits:** `0e04235` (LWC finding triaged — finding.md status banner + Triage section + journal.md Triaged entry). Spec closeout commit follows.
**Files touched:** [specs/findings/20260517-easy-survival-shelves-lwc-error/finding.md](../findings/20260517-easy-survival-shelves-lwc-error/finding.md), [specs/findings/20260517-easy-survival-shelves-lwc-error/journal.md](../findings/20260517-easy-survival-shelves-lwc-error/journal.md).
**Tests added:** none — methodology repo, prose-review discipline. Verification by inspection per spec §8 Test Strategy; the dogfood exercise itself is the test (spec §7 T-03 Tests-required section).
**DoD verification:**
- LWC finding triaged ✓ — `Status: triaged`; all seven Triage fields populated; Triaged journal entry appended.
- Intake section byte-for-byte preserved ✓ — verified by `diff` of pre-edit (`git show HEAD:.../finding.md`) and post-edit Intake blocks — zero differences.
- Status banner updated ✓ — `Status: triaged`; `Severity: advisory`; `Operational urgency (optional): P4`; `Last transition: 2026-05-17`. Operational urgency populated per spec §7 T-03 requirement (operational domain → urgency required); follows T-02 convention of dropping the trailing axis-comment when populating a placeholder.
- Triaged journal entry has all 8 prescribed fields ✓ — Triaged by, Prior status (`intake`), New status (`triaged`), Reproducibility outcome (`reliably` with two-reporter / cross-world-control rationale), Domain/severity changes (Domain unchanged; Severity + urgency first-population), Skip-investigation decision (`end at triaged` with explicit RC-3b note about the still-unexercised skip-route path), Pointer revalidation (`treated-as-static` for the forum URL, soft-default per rich-Summary), Notes (persona-frame override exercised, hard-facts discipline held, mode observation, reporter-list stable).
- Reproducibility honest ✓ — `reliably` matches in-thread evidence (two reporters, two interaction modes — place/remove and right-click — plus negative cross-world control on Normal Survival). Spec §7 T-03 expected outcome was `reliably`; matched.
- Severity + operational urgency both populated ✓ — `advisory + P4` (operational domain → urgency required per spec §7 T-03).
- Pointer-revalidation outcome recorded ✓ — `treated-as-static` for the auth-walled forum URL, with rationale (rich Intake Summary + operator-supplied PDF snapshot is durable evidence; no live re-fetch attempted because forum is auth-walled per intake journal note). Soft-default accepted without override.
- Hard-facts discipline held ✓ — Triage section contains observational facts only. The intake-time hypothesis (":Missing API → plugin API surface missing/version-mismatched") is recorded in the Triage notes field as *deferred to investigation*, not as a Triage claim. WHAT NOT TO DO bullet on causal hypotheses honored.
- Persona-frame override exercised ✓ — suggested `business analyst` overridden to operator-named `Sandlot administrator`. First exercise of the §5.3 OQ-3-resolution override path on a real-signal dogfood. Resolves the §10 watch item ("Persona-frame override surface is rarely used, defaulting back to a single suggestion the operator accepts blindly") in the direction of *used*; RC-3b reviewer has direct evidence the override surface is reachable and useful.
- Dogfood outcomes journaled ✓ — Notes field of the LWC journal entry captures the persona-frame override observation, the pointer-revalidation policy observation (`treated-as-static` felt right for a rich Summary with PDF snapshot — not ceremony), the skip-investigation observation (not exercised — left for a future session), and the mode observation (structured-input via `/spec-execute` rather than per-field interactive prompts).

**Decisions made:**
- **End at `triaged` (skip-route surface not exercised).** Operator decision at execution-time prompt. Per spec §7 T-03 framing, ending at `triaged` is the default expected outcome; the operator chose it. **Consequence:** the two-journal-entry skip-route discipline (Triaged then Routed/Closed) remains verified only by inspection across T-01 (skill prose), T-02 (no skip), and T-03 (no skip) — never executed end-to-end against a real artifact during this spec's task breakdown. This was flagged at T-02 closeout as a possible RC-3b concern; T-03 confirms the gap rather than closing it. RC-3b reviewer may choose to accept inspection-only verification given the discipline is explicit in the skill prose at three placement points (OPERATING PRINCIPLE #6, Phase 3 step 4, WHAT NOT TO DO bullet 6), or may request a follow-on synthetic exercise that does exercise skip-route. The decision is theirs.
- **Persona-frame override.** Operator chose `Sandlot administrator` over the suggested `business analyst` — the operator IS the Sandlot admin and that frame fits the work better. This is exactly the use case the §5.3 OQ-3-resolution was designed for; the override path is now exercised on a real signal, not just specified.
- **Severity + urgency: `advisory + P4`.** Operator-judged: shelves are not core to gameplay; player workaround exists (avoid shelf interactions on Easy Survival until investigated); signal is real and worth investigating but is not blocking active play. Lower than my own pre-prompt lean of `important + P3`; operator's domain knowledge is authoritative here (this is exactly what the persona-frame discipline is for).
- **Pointer revalidation `treated-as-static`.** Soft-default for a rich Intake Summary plus operator-supplied PDF snapshot. No prompt felt like ceremony; the policy did its job (asked the question once, accepted the default, journaled the decision).
- **Mode: structured-input via single up-front `AskUserQuestion` round-trip.** Claude operated on the operator's behalf via `/spec-execute`. The skill's per-field interactive prompts were *not* used directly — instead, the four operator-judgment fields (persona-frame, severity+urgency, skip-investigation, pointer-revalidation) were collected in one batch. Reproducibility, repro steps, scope, domain confirmation, and triage notes were derived from the Intake artifact without further prompting. The 60-second-vs-not honest comparison against the intake NFR (per spec §7 T-03 mode framing) is therefore not directly observed in this T-03; the spec accepts this trade — interactive prompts are an option the skill supports, not a discipline the skill enforces. RC-3b reviewer should be aware that the operator-effort observation here is a *batch* observation, not a sequential-prompt observation.

**Spec amendments:** none. T-03 closed cleanly within the §7 T-03 acceptance criteria and the SKILL.md as authored at T-01. No surfaced bugs requiring T-01 amendment; no surfaced gaps requiring schema amendment.

**Surprises and learnings:**
- **Operator's severity assessment came in lower than my pre-prompt lean.** I framed `important + P3` as the likely default in the operator question; the operator chose `advisory + P4`. This is *exactly* what the persona-frame discipline is meant to surface — domain expertise lives with the Sandlot administrator, not with the agent estimating from the artifact. The skill's design (operator-supplied severity, not agent-derived) held. Worth recording for RC-3b: when the operator's severity differs from the agent's pre-prompt lean, the operator's is authoritative and the agent's lean was a useful framing scaffolding, not a recommendation in disguise.
- **The skip-investigation surface remains unexercised after T-03.** Both validation exercises (T-02 synthetic, T-03 real) ended at `triaged`. The skip-route path (two-journal-entry discipline, Route section population, terminal-status transition) is verified only by inspection of the SKILL.md prose. RC-3b reviewer has three options: (a) accept inspection-only verification given the three placement points in SKILL.md make the discipline visible; (b) request a follow-on micro-exercise that explicitly exercises skip-route against a synthetic finding; (c) defer the question until a real signal naturally invites skip-route (potentially at Phase F adoption). My own lean is (a) — the discipline is explicit and the inspection trail is solid — but the call is RC-3b's.
- **Pointer-revalidation policy felt minimal, not ceremonial.** The single-prompt soft-default path for a rich Intake Summary was the right cut: one ask, accept-default, journaled. If every dogfood from now on hits the same path (rich Intake → static), the policy is doing exactly what spec §5.4 designed it to do — making the costly choice opt-in rather than opt-out. The sparse-Intake recommend-check path is not exercised by T-03 (LWC Intake is rich) and may not be exercised soon if intake quality stays high; if and when a sparse-Intake finding arrives, that path gets exercised then.
- **Persona-frame override path is now real.** §10's watch item about override-surface-rarely-used resolves in the direction of *used* on the first real-signal dogfood. The frame derivation table (operational → business analyst) is useful as orientation; the override path is useful as accuracy. Both are doing their jobs.
- **T-02 closeout flagged spec-write-leaves-specs-uncommitted (`21d5765`) and intake-template-folder-dependency (`7c61bca`) as parallel-work findings.** Neither moved during T-03. Both remain at `status: intake`, awaiting their own triage in a later session. Not raised against this spec.

**Next task pointer:** T-04 — Add "Triaging a finding" section to [specs/findings/README.md](../findings/README.md). Dependencies satisfied (T-01 complete; T-03 dogfood produced a real triaged finding per the dependency requirement: "do not flip primary documentation until the skill has been dogfooded successfully"). Estimated size: S. T-04 is the trigger for RC-3b per §9 — once T-04 lands, the checkpoint gate fires and the next action is the reviewer handoff, not another task.

## 2026-05-17 — T-04: README "Triaging a finding" section — flip /finding-triage primary

**Status:** done
**Commits:** `23837e5` (README extended). Spec closeout commit follows.
**Files touched:** [specs/findings/README.md](../findings/README.md) (+20 lines: new `## Triaging a finding` section + `### Manual fallback` subsection).
**Tests added:** none — methodology repo, prose-review discipline. Verification by inspection per spec §7 T-04 Tests-required section.
**DoD verification:**
- README.md updated ✓ — new `## Triaging a finding` section appended after `## Creating a new finding` + its `### Manual fallback` block (now at L172).
- Committed ✓ (`23837e5`).
- Cross-references valid ✓ — [.agents/skills/finding-triage/SKILL.md](../../.agents/skills/finding-triage/SKILL.md) exists; [specs/20260517-finding-triage-skill/feature.md](feature.md) exists (this spec); [specs/20260517-findings-pipeline/architecture.md §5.3 Triage phase](../20260517-findings-pipeline/architecture.md) exists (heading verified at L200 of that file).
- New section present immediately after "Creating a new finding" ✓ — section ordering: L142 `## Creating a new finding` → L156 `### Manual fallback` → L172 `## Triaging a finding` → L188 `### Manual fallback`.
- `/finding-triage` invocation is the first option presented ✓ — opening paragraph + code-fenced slash-command invocation with `FINDING_PATH` argument.
- Load-bearing inputs named ✓ — `FINDING_PATH` in the invocation; reproducibility, scope, severity confirmation, domain confirmation, operational urgency when applicable, triage notes all named in the behavior paragraph.
- Skip-investigation surface called out ✓ — dedicated `**Skip-investigation surface.**` paragraph naming the `triaged → routed` / `triaged → closed` skip path, the route-section + second-journal-entry behavior, and the state-machine cross-link.
- Persona-frame derivation called out ✓ — dedicated `**Persona-frame derivation.**` paragraph with the full Domain → frame table (`operational` → business analyst; `security` → security analyst; `testing` → QA lead; `methodology` → methodologist; `other` → operator-named) and an explicit override example (`Sandlot administrator`) that mirrors the T-03 dogfood evidence.
- Line count ≤200 ✓ — `wc -l` = 190 (20-line addition from 170; under the 200-line inspection ceiling per §7 T-04 Tests-required).
- Existing "One finding or several?" paragraph and intake "Manual fallback" section untouched ✓ — verified by section-header listing (L154 and L156 unchanged from pre-T-04 state).

**Decisions made:**
- **Anchor format for design-spec §5.3 cross-link:** chose `#53-triage-phase` (GFM convention: period dropped, no hyphen replacement). Matches the precedent in the existing intake-section README link `#7-implementation-sequencing` (single-digit, no period to test the choice with). Some markdown renderers use `#5-3-triage-phase` (period → hyphen); if anchors don't resolve in some viewer, that is a known cross-renderer markdown inconsistency, not a content issue. Not raised as its own finding.
- **Triage manual-fallback wording held to a single sentence** per spec §5.6 (`"states it briefly, since the schema already documents the field reference"`). The intake "Manual fallback" subsection by contrast walks through a `cp` recipe — that asymmetry is intentional: intake's manual path includes file *creation* (which benefits from the explicit shell snippet), triage's manual path is *editing existing fields* (which the field reference already documents).
- **Override example chosen as `Sandlot administrator`** (mirrors T-03 dogfood). Two purposes: (1) demonstrates the override path concretely for a reader scanning for "what does override look like in practice?", and (2) preserves continuity with the T-03 journal evidence so future RC-3b readers can connect the README copy to a real exercise of the path.

**Spec amendments:** none. T-04 closed within the §7 T-04 acceptance criteria + §5.6 design guidance. No surfaced bugs requiring T-01 amendment; no surfaced gaps requiring schema or design-spec amendment.

**Surprises and learnings:**
- **README line budget held with headroom.** 190 of 200 lines used. The dedicated paragraphs for skip-investigation and persona-frame derivation (mandated by §7 T-04 scope) plus the manual-fallback subsection landed in 20 lines total — close to the §5.6 ~25-line target but slightly under. If a future amendment needs to extend any of the three paragraphs, the budget supports it without crowding.
- **The intake `Manual fallback` shell snippet pattern was *not* copied to the triage section.** Per §5.6 ("states it briefly"), the triage manual fallback is a single sentence pointing back to the field reference. This is a deliberate asymmetry, recorded above in Decisions. A future reader who expects symmetric subsections may find this jarring; if so, that is a candidate methodology observation for a follow-on finding, not a T-04 issue.
- **All four Phase C tasks landed in one calendar day (2026-05-17)** across two sessions: prior session closed T-01 (SKILL.md) and T-02 (synthetic exercise); this session closed T-03 (real-signal dogfood) and T-04 (README integration). Phase B's cadence was similar (T-01/T-02/T-03/T-04 in one day, 2026-05-16). The Phase B/C pattern of "one day per skill" is the second consecutive observation; may inform Phase F sequencing expectations.
- **No skill or schema amendments surfaced across T-01 → T-04.** The SKILL.md as authored at T-01 carried unchanged through both validation exercises (T-02 synthetic, T-03 real-signal) and the README integration (T-04). This is the second skill (Phase B's `finding-intake` was the first) to complete its full validation cycle without requiring its own spec-amend pass — a positive signal about the design-spec → feature-spec → execution pipeline.

**Spec status:** **All Phase C tasks now complete.** §9 RC-3b checkpoint trigger condition satisfied ("T-01, T-02, T-03, T-04 all complete; commits landed"). The next action per the spec-execute Phase 7 protocol is the RC-3b reviewer handoff, not another task. RC-3b's closure in turn satisfies the design-spec-level RC-3 exit condition (RC-3a already closed for Phase B), unblocking the OQ-3 / OQ-4 quote-back amendment to the design spec (out of scope per §12) and Phase F (adoption review).

**Next task pointer:** **RC-3b — Phase C Skill Review.** Triggered by T-04 completion. Reviewer handoff is the next action; see this spec's §9 RC-3b for review focus and exit criteria. No further tasks remain in this spec's §7 Task Breakdown.

## 2026-05-17 — Review of RC-3b

**Reviewer:** Claude (self-review on behalf of waseric)
**Outcome:** pass with comments
**Tasks reviewed:** T-01, T-02, T-03, T-04
**Diff range:** `c29f23b..HEAD` (Phase C spec authorship through T-04 closeout — 12 files, +542/−39)
**Blockers:** 0
**Important:** 0
**Advisory:** 3 — see one-line summaries below.

**Advisory findings (full body in the review report; one-line summaries here):**
- **A-1 — State-machine guard inspection-verified, not second-invocation-exercised.** SKILL.md L78–L80 encodes the rejection wording with explicit "exit without artifact mutation" guarantee; T-02 journal documents "verified by inspection" rather than running an actual second invocation against the now-`triaged` fixture. For a prompt-style skill, prose-encoding is meaningful evidence; the spec's exit-criterion wording ("second-invocation rejection evidence") reads stricter than what was performed. Recommended follow-on: 30-second second-invocation exercise to close the gap formally. Not blocking — accepted as **met-with-caveat**.
- **A-2 — Skip-investigation surface not exercised end-to-end.** Both T-02 and T-03 ended at `Status: triaged`. The two-journal-entry discipline is verified by inspection across three SKILL.md placements (OP #6, Phase 3 step 4, WHAT NOT TO DO bullet 6). Spec §7 framed end-at-triaged as the default expected outcome and the RC-3b review-focus phrasing "(and if so, whether...)" anticipates non-use. T-03 journal listed options (a)/(b)/(c); option (a) — accept inspection-only verification — adopted. Phase F adoption may surface a natural skip-route signal.
- **A-3 — Two session-side intake findings created during Phase C work but not declared in §7 or §12.** [specs/findings/20260517-spec-write-leaves-specs-uncommitted/](../findings/20260517-spec-write-leaves-specs-uncommitted/) (commit `21d5765`) and [specs/findings/20260517-intake-template-folder-dependency/](../findings/20260517-intake-template-folder-dependency/) (commit `7c61bca`) captured via `/finding-intake` mid-session; both transparently journaled as parallel work; neither modifies Phase C deliverables. Recommended follow-on: a small §12 Out of Scope clarification ("session-side intake captures via the upstream `/finding-intake` skill are out-of-scope side artifacts and require no Phase C amendment"), deferrable until convenient. Not blocking.

**Exit criteria status:**
- SKILL.md ≤220 lines, twelve §5.1 sections present: **met** (197 lines).
- T-02 transitions cleanly; Intake-byte-preserved; state-machine guard verified: **met-with-caveat** (guard inspection-verified per A-1).
- T-03 produces real triaged finding; pointer-revalidation recorded; persona-frame held; operator effort journaled: **met**.
- T-04 README update preserves existing + adds new section under line-count ceiling: **met** (190 of 200 lines).
- OQ-3 and OQ-4 resolutions in SKILL.md and feature spec §5.3 + §5.4: **met**.
- No `[blocker]` / `[important]` findings: **met** (0 / 0).

**Review focus walk (one-line verdicts per RC-3b §9 focus area):**
- State-machine pre-condition enforcement: **pass with comments** (A-1 advisory).
- Persona-frame discipline ("stay out of code") in T-02 and T-03: **pass** (no codebase opening; reproduction descriptions are server-running, not source-reading).
- Hard-facts discipline (no cause hypotheses in Triage): **pass** (T-02 explicit "no cause hypothesis recorded"; T-03 intake-time hypothesis "deferred to investigation," not asserted at triage).
- Pointer-revalidation policy usefulness in T-03: **pass with comments** (T-03 journal: "felt minimal, not ceremonial"; sparse-Intake branch unexercised, advisory only).
- Skip-investigation surface usage and discipline: **pass with comments** (A-2 advisory; not used; option (a) adopted).
- Persona-frame override naturalness in T-03: **pass** (operator override from `business analyst` → `Sandlot administrator` on first dogfood; §10 watch item resolves in direction of *used*).
- OQ-3 and OQ-4 encoding in SKILL.md prose: **pass** (multi-placement coverage verified).

**Spec amendments proposed:** None required for RC-3b closure. A-3 above suggests a small §12 clarification deferrable to a future session; not gated on this checkpoint.

**Verification evidence summary:**
- Intake byte-preservation verified by `git show dc87ba1 -- ...test-only-signal.../finding.md` and `git show 0e04235 -- ...lwc-error/finding.md` — both diffs show only status-banner and Triage-section changes; Intake blocks unchanged.
- SKILL.md frontmatter parseable: L1–L5; description references `Findings Pipeline`, `[[finding-intake]]`, `[[spec-amend]]`, `[[spec-write]]`.
- Twelve §5.1 structural sections: confirmed by T-01 journal entry's DoD verification, independently verified at review time.
- Design-spec §5.3 cross-reference target exists at [specs/20260517-findings-pipeline/architecture.md:200](../20260517-findings-pipeline/architecture.md#L200) (`### 5.3 Triage phase`); GFM anchor `#53-triage-phase` follows the same convention used elsewhere in the repo (e.g., intake README's `#7-implementation-sequencing`).

**Next action:**
- **RC-3b closed.** No further work required in this feature spec's task breakdown.
- **RC-3 (design-spec joint checkpoint) becomes eligible to close.** RC-3a passed for Phase B (2026-05-17); RC-3b now passes for Phase C. The design spec's RC-3 entry at [specs/20260517-findings-pipeline/architecture.md §9 RC-3](../20260517-findings-pipeline/architecture.md) should be updated with a status line via a future `/spec-review` or `/spec-amend` pass against that spec; out of scope for this entry per this spec's §9 framing ("closed by this spec's completion").
- **OQ-3 / OQ-4 quote-back amendments to the design spec §13** are now unblocked. Out of scope for this spec per §12; executed via `/spec-amend` against the design spec in a separate session.
- **Phase F (adoption review)** becomes the natural next phase per the design-spec implementation sequencing. Three real findings are already in the pipeline at `status: intake` ([20260517-spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/), [20260517-intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/), and any future captures) plus two end-state references ([20260517-test-only-signal-synthetic-fixture](../findings/20260517-test-only-signal-synthetic-fixture/) end-state-Triaged, [20260517-tab-display-issues](../findings/20260517-tab-display-issues/) end-state-Investigation) — natural Phase F input.
- **Optional follow-on (low cost, deferrable):** the A-1 second-invocation exercise (30 seconds against the now-`triaged` synthetic fixture, capturing the rejection output as direct evidence). Not gating; recorded here as a confidence-check the operator may run when convenient.

**Reviewer notes — self-review honesty:**
- Self-review caveat acknowledged: Claude executed T-01 through T-04 on the operator's behalf via `/spec-execute`, then performed this review. Per the skill's "be especially honest about advisory findings" guidance, A-1 (state-machine guard inspection-only) was deliberately not dismissed even though my pre-review instinct was to accept inspection as sufficient. The advisory is recorded so the operator (waseric) can decide whether to require the second-invocation exercise before closing RC-3b in their own judgment, or accept the verdict as written.
- The skip-route inspection-only verdict (A-2) is the area I would most expect a second reviewer to push back on. Option (a) reflects the journal's documented lean and the spec's §7 default-expected-outcome framing; option (b) (synthetic skip-route micro-exercise) is low-cost and would resolve A-2 cleanly. Recommending (a) without pretending the gap is invisible.

## 2026-05-17 — Amendment 2026-05-17-1

**Section amended:** specs/20260517-finding-triage-skill/feature.md §12 (Out of Scope) — two surgical changes
**Trigger:** RC-3b review (commit `e3ae8af`) advisory A-3 + RC-3 review (commit `e92d28e`) advisory A-3 — recommended a small §12 clarification declaring session-side intake captures as out-of-scope side artifacts. Bundled with a consequential tense-tweak to the existing OQ-3/OQ-4 quoting-back bullet now that design-spec amendments 2026-05-17-3 and -4 have satisfied that deferral.
**Reason:** Future readers would have no in-spec record that mid-execution intake captures via the upstream `/finding-intake` skill are an expected pattern rather than scope drift; and the existing OQ-3/OQ-4 bullet's future-tense framing is now stale after this session's preceding amendments.
**Impact summary:** No tasks affected (Phase C T-01–T-04 complete and untouched); no checkpoints re-opened (RC-3b and RC-3 stay closed); no completed work invalidated; no cross-references require follow-up.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept (status banner remains `Draft — Awaiting Execution`; banner-staleness observed but out of scope for this amendment — flagged separately at Phase 6 handoff)
**Commit:** `3ace275`

### Full record

**Trigger.** RC-3b self-review (commit `e3ae8af`) advisory A-3 surfaced that two intake findings were captured via `/finding-intake` during Phase C execution ([spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/) commit `21d5765`; [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/) commit `7c61bca`), but §12 Out of Scope never declared session-side captures as out-of-scope side artifacts. RC-3 review (commit `e92d28e`) advisory A-3 reinforced the framing. Recommended remedy was "a small §12 Out of Scope clarification, deferrable until convenient." Bundled in this amendment: a surgical tense-tweak to the existing OQ-3/OQ-4 quoting-back bullet, now that this session's preceding amendments (2026-05-17-3 and -4 against the design spec) have satisfied that deferral.

**Section.** §12 Out of Scope (L522–L533 pre-amendment).

**Change.** Two surgical edits to §12 — a tense-tweak to one existing bullet, and a new bullet inserted between two existing bullets.

§12 — Before (relevant adjacency):
> - **Quoting OQ-3 and OQ-4 resolutions back to the design spec via `/spec-amend`** — the resolutions are recorded in this feature spec's §5.3 and §5.4 and in the produced SKILL.md, satisfying RC-3's exit criteria. Quoting them back to the design spec's §13 (converting OQ-3 and OQ-4 from "open" to "decided") is a small follow-on amendment, executed after RC-3 closes, against the design spec — *not* part of this feature spec's task breakdown.
> - **External-system push (Slack/Linear/GitHub integration)** — the design spec's §12 declares this out of scope for the methodology; Phase C inherits the exclusion.

§12 — After (relevant adjacency):
> - **Quoting OQ-3 and OQ-4 resolutions back to the design spec via `/spec-amend`** — the resolutions are recorded in this feature spec's §5.3 and §5.4 and in the produced SKILL.md, satisfying RC-3's exit criteria. Quoting them back to the design spec's §13 (converting OQ-3 and OQ-4 from "open" to "decided") has been satisfied via design-spec amendments [2026-05-17-3](../20260517-findings-pipeline/journal.md) (OQ-3) and [2026-05-17-4](../20260517-findings-pipeline/journal.md) (OQ-4); the work was not part of this feature spec's task breakdown.
> - **Session-side intake captures via `/finding-intake`** — findings captured via the upstream `/finding-intake` skill during a Phase C execution session (e.g., [spec-write-leaves-specs-uncommitted](../findings/20260517-spec-write-leaves-specs-uncommitted/) commit `21d5765`; [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/) commit `7c61bca`) are out-of-scope side artifacts: they exercise the `/finding-intake` skill (Phase B deliverable) under real conditions, contribute independent evidence for the design spec's §6 interruption-tolerance NFR, and live in the Findings Pipeline awaiting their own triage in a later session. They require no Phase C amendment, no inclusion in §7 Task Breakdown, and no closeout against this spec's DoD; the intake skill's own discipline (atomic finding commit, status banner, intake journal entry) is the contract they meet.
> - **External-system push (Slack/Linear/GitHub integration)** — the design spec's §12 declares this out of scope for the methodology; Phase C inherits the exclusion.

**Reason.** RC-3b A-3 and RC-3 A-3 surfaced the same gap from two checkpoint perspectives: the spec's §12 was silent on the legitimate, transparent, and journal-documented practice of capturing parallel findings via the upstream skill during execution. Future readers (or future self) re-orienting to this spec would have no in-spec record that session-side captures are an expected pattern rather than scope drift. The clarification makes the discipline self-documenting. Bundled tense-tweak: the existing OQ-3/OQ-4 bullet's "executed after RC-3 closes" framing became stale the moment this session's amendments 2026-05-17-3 and -4 landed; refreshing the language in the same amendment keeps §12 internally coherent.

**Impact.**
- **Affected tasks:** none. Phase C's T-01 through T-04 are complete; this amendment does not change task scope, it only declares that the two session-side captures were never task-scoped to begin with, and ratifies that the OQ-3/OQ-4 quote-back is now done.
- **Affected checkpoints:** RC-3b is closed (`pass with comments` per `e3ae8af`); this amendment ratifies A-3's recommendation in spec text. RC-3 is closed (`pass with comments` per `e92d28e`); same. No re-review triggered.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — §7 Task Breakdown's silence on session-side captures is now self-consistent with §12's explicit declaration. The two findings themselves are unaffected (they live in the Findings Pipeline regardless). The design-spec OQ-3/OQ-4 entries are already current as of amendments 2026-05-17-3 and -4.

**Status implication.** Spec status banner remains `Draft — Awaiting Execution`. The banner-staleness is observed (Phase C is in fact done, RC-3b closed) but is out of scope for this amendment — flagged separately at Phase 6 handoff so the operator can decide whether to address it as a future amendment.

**Approver.** waseric — approved (with bundled OQ-3/OQ-4 staleness tweak per Phase 3 question) on 2026-05-17.

## 2026-05-17 — Amendment 2026-05-17-2

**Section amended:** specs/20260517-finding-triage-skill/feature.md §6 NFRs (appended new `Skill portability` row; replaced `Context economy` row). Cascading file changes: new bundled template files at [.agents/skills/finding-triage/_template/finding.md](../../.agents/skills/finding-triage/_template/finding.md) and [.agents/skills/finding-triage/_template/journal.md](../../.agents/skills/finding-triage/_template/journal.md); SKILL.md Phase 1 ORIENT step 1 (L73–L75) rewritten to use bundled-with-host-override + embedded schema knowledge.
**Trigger:** Cascading from Amendment 2026-05-17-4 to finding-intake-skill (commit `c71311f`), which brought `finding-intake` into conformance with the Atomic-Skill Portability Principle. The triage skill carries the same host-relative-path + runtime README read pattern at SKILL.md L73–L75; this amendment closes the cascade. Originating finding: [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/finding.md).
**Reason:** Same coupling pattern as finding-intake — the triage skill's ORIENT phase reads `../../../specs/findings/_template/...` and `../../../specs/findings/README.md` as runtime inputs. Both are addressed by bundling templates inside the skill directory with host-override semantics and embedding schema knowledge in SKILL.md prose. The triage skill's coupling is narrower than intake's (no line-range strip mechanism to convert) because triage edits an existing finding artifact rather than materializing a new one — so the conformance amendment is correspondingly smaller.
**Impact summary:** No tasks re-opened (T-01 through T-04 stay closed); no checkpoints re-opened (RC-3 and RC-3b stay closed). RC-3 advisory A-3 (portability liability flag) is now satisfied for both skills (Amendments 4 and 5 together). No further cross-references in this cascade.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept at `Complete`. The amendment reverses no design decision, re-opens no §7 task. Explicit operator confirmation captured during the amendment session.
**Commit:** `73ea104`

### Full record

**Trigger.** Cascading from Amendment 2026-05-17-4 to [specs/20260517-finding-intake-skill/feature.md](../20260517-finding-intake-skill/feature.md) (commit `c71311f`), which brought the `finding-intake` skill into conformance with the Atomic-Skill Portability Principle. The triage skill carries the same coupling pattern at SKILL.md L73–L75 (host-relative `../../../specs/findings/_template/...` paths + runtime README read); this amendment closes the cascade.

**Sections and files.** Feature spec §6 NFR (append `Skill portability` row + replace `Context economy` row); plus new files `.agents/skills/finding-triage/_template/finding.md` and `.agents/skills/finding-triage/_template/journal.md`; plus edits to `.agents/skills/finding-triage/SKILL.md` Phase 1 ORIENT step 1. Five changes bundled. Full before/after diffs are in the Phase 2 draft of this amendment in the calling session; the After blocks have been applied verbatim.

**Change summary.**
1. feature.md §6 NFR — appended `Skill portability` row referencing the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle).
2. feature.md §6 NFR — replaced `Context economy` row to remove the "loads from `specs/findings/`" wording (replaced with bundled-template/host-override wording).
3. `.agents/skills/finding-triage/_template/finding.md` (new) — verbatim copy of [specs/findings/_template/finding.md](../findings/_template/finding.md) in its post-amendment-3 scaffold-marker form.
4. `.agents/skills/finding-triage/_template/journal.md` (new) — verbatim copy of [specs/findings/_template/journal.md](../findings/_template/journal.md) in its post-amendment-3 scaffold-marker form.
5. `.agents/skills/finding-triage/SKILL.md` — Phase 1 ORIENT step 1 rewritten to use bundled-with-host-override + embedded schema; schema-knowledge paragraph relocated to follow item 2 of the ORIENT list to preserve numbered-list continuity.

**Reason.** Same as Amendment 4: path-coupling and README-runtime-read pattern from the originating finding. Bundling templates inside the skill directory with host-override semantics and embedding schema knowledge in SKILL.md prose makes the skill self-contained. The triage skill's coupling is narrower than intake's (no line-range strip mechanism to convert, because triage edits an existing finding artifact rather than materializing a new one) — but the structural fix is the same.

**Impact.**
- **Affected tasks:** T-01 (authored SKILL.md) already done. Retroactive conformance update; stays done.
- **Affected checkpoints:** RC-3 (Intake & Triage Skill Review) and RC-3b (Phase C SKILL.md self-review) both already closed. RC-3 advisory A-3 (portability liability flag) is now fully satisfied across both skills (Amendments 4 and 5 together).
- **Completed work invalidated:** No. Operational behavior is equivalent at canonical state.
- **Cross-references requiring follow-up:** None — this closes the cascade. Post-cascade: route the originating finding (`under-investigation → routed`) and file the follow-on advisory finding for the constitution-amendment workflow gap.

**Status implication.** Spec stays at `Status: Complete`. Same logic as Amendments 3 and 4 — no design decision reversed, no §7 task re-opened.

**Approver.** waseric — approved as drafted on 2026-05-17, with explicit confirmation to keep the spec at `Complete`.
