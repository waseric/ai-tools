# Finding Intake Skill — Journal

## 2026-05-17 — Feature Spec Authored

**Status:** draft — awaiting execution
**Artifact:** specs/20260517-finding-intake-skill/feature.md
**Upstream design spec:** specs/20260517-findings-pipeline/architecture.md (Phase B of §7 Implementation Sequencing)
**Upstream sibling feature spec (Phase A — done):** specs/20260517-findings-pipeline-schema/feature.md (RC-2 passed; remediated 2026-05-17)
**Origin:** Phase B execution following the RC-2 closeout of the Phase A schema spec. Phase A delivered the schema artifacts (README, finding template, journal template); Phase B builds the skill that consumes them.

**Decisions made:**
- Phase B scope is **skill only** (not skill + adoption gate; not skill-defers-README). Operator confirmed in Phase 2 Clarify. README flip is bundled as a small follow-on task (T-04) within the same spec, executed only after T-03 dogfood succeeds.
- Short-name derivation: **AI proposes from title; operator confirms**. Stop-word filtering + 3–5 word target + ≤40 character limit + one-step accept/edit. Cosmetic choice; recoverable via rename.
- External-pointer fetch policy: operator **must** supply a summary when providing a URL (summary is load-bearing per design spec §6); the skill **attempts** a fetch if the invoking agent has the capability; fetch failures **must not be silently accepted** — operator sees the failure and chooses retry / proceed without snapshot / cancel. This refines the operator's Phase 2 directive ("failure to retrieve url should not be silently accepted").
- Validation: **synthetic (T-02) + real-signal dogfood (T-03)**. T-02 catches mechanics bugs without burning a real signal on a buggy skill; T-03 verifies the 60-second NFR and produces a real artifact.
- Three internal open questions parked (OQ-1 auto-commit policy; OQ-2 captured-by email; OQ-3 multi-pointer signals), all decidable at T-01 execution time by the skill author rather than requiring upstream amendment.
- Internal review checkpoint named **RC-3a** to disambiguate from the design-spec-level RC-3 (joint Phase B + Phase C review). RC-3a closes when Phase B is shippable; RC-3 still requires Phase C.

**Open questions surfaced and parked in §13:**
- OQ-1 (auto-commit vs. working-tree-leave): leaning working-tree-leave for interruption-tolerance. Decided at T-01 execution time.
- OQ-2 (`Captured by` field with or without email): leaning name-only matching journal convention. Decided at T-01.
- OQ-3 (multiple pointers per signal): leaning accept list with per-pointer fetch attempts. Decided at T-01.

**Tasks defined:** T-01 (skill artifact, M) → T-02 (synthetic validation, S) → T-03 (real-signal dogfood, S) → T-04 (README flip, S). Four tasks, all S or M, sequenced so each boundary is a safe stopping point: T-01 is the deliverable; T-02 verifies mechanics; T-03 verifies real-world fit; T-04 lands the README change only after dogfood succeeds.

**Conversation grounding:**
- Operator invoked via `/spec-write phase b` against the existing findings-pipeline working directory. Confirmed at Discovery time that Phase A is closed (RC-2 verdict committed at ccce4ce; remediation at 71480e9).
- Operator's Phase 2 directive on URL pointer failures ("if operator provides url, failure to retrieve url should not be silently accepted") refined the spec's policy beyond my recommended option — incorporated as a fetch-with-surfaced-failure model rather than the no-fetch model I'd originally proposed. The 60-second NFR remains intact in the typical no-pointer / fetch-succeeds case; pointer-with-fetch-failure is an explicit deviation case the operator chooses to handle.
- Discovery confirmed sibling skill patterns (six existing skills at 200–225 lines each, frontmatter + ROLE + OPERATING PRINCIPLES + INPUTS + phased workflow) — Phase B SKILL.md targets ≤220 lines for consistency.

**Next task pointer:** Execute T-01 (`.agents/skills/finding-intake/SKILL.md`) via `/spec-execute`. Dependencies satisfied (Phase A schema artifacts are committed and stable). No `[blocker]` open questions; ready to proceed.

## 2026-05-17 — T-01: Author `.agents/skills/finding-intake/SKILL.md`

**Status:** done
**Commits:** 1e640c7
**Files touched:**
- New: `.agents/skills/finding-intake/SKILL.md` (149 lines)
- Edited: `specs/20260517-finding-intake-skill/feature.md` — T-01 marked done; §13 OQs collapsed to decisions D-1/D-2/D-3 in new §13a
- Edited: `specs/20260517-finding-intake-skill/journal.md` — this entry

**Tests added:** None (inspection-based per §8 Test Strategy). Inspection evidence:
- `wc -l`: 149 (under 220 ceiling; below 180 soft floor — flagged as deviation, see below).
- Twelve §5.1 structural sections present and in declared order (frontmatter, title, opening paragraphs, How this skill works, INPUTS, ROLE, OPERATING PRINCIPLES, Phase 1, Phase 2, Phase 3, OUTPUT FORMAT, WHAT NOT TO DO).
- INPUTS coverage: all 9 fields from §4 present (TITLE, SUMMARY, EXTERNAL_POINTER, REPORTED_BY, REPORTED_VIA, CAPTURED_BY, DOMAIN, SHORT_NAME, DATE).
- WHAT NOT TO DO covers all 4 anti-goals: no triage-phase prompts; no silent fetch failures; no dedup scan; no template rewrite.
- Cross-reference paths (`../../../specs/...`) verified to resolve from `.agents/skills/finding-intake/`.

**DoD verification:**
- File written: `.agents/skills/finding-intake/SKILL.md` exists.
- Under 220 lines: 149 ✓.
- Committed: at this closeout commit (paired with feature.md + journal.md updates).

**Decisions made:**
- **D-1 (was OQ-1):** Working-tree-leave, not auto-commit. Skill returns suggested commit message; operator commits when ready. Recorded in SKILL.md OP #7 and Phase 3 step 5.
- **D-2 (was OQ-2):** `Captured by` defaults to git `user.name` only (no email). Fallback "unknown" when `user.name` is unset. Recorded in SKILL.md Phase 2 "Captured by" derivation.
- **D-3 (was OQ-3):** `EXTERNAL_POINTER` accepts comma- or newline-separated list; each fetched per policy; each outcome journaled separately. Recorded in SKILL.md INPUTS and Phase 3 step 4.

All three were marked decidable-at-T-01 in the spec; all three resolved per their leanings. None required upstream amendment.

**Spec amendments:** None. Feature spec §7 task statement was followed; one minor deviation noted below.

**Surprises and learnings:**
- **Line-count deviation.** Spec §7 sets "target length 180–220 lines"; §6 NFR ceiling is "≤220 lines"; T-01 DoD requires "under 220 lines." Final SKILL.md is 149 — below the soft floor, well within the ceiling. The skill is structurally complete (all 12 sections present, all anti-goals stated, all INPUTS covered) and the deficit reflects this skill being inherently simpler than peer skills (no multi-class diagnostic logic à la `spec-amend`, no governance phases à la `spec-execute`, no discovery/clarify phases à la `spec-write`). Treating the 180 floor as advisory and the 220 ceiling as the hard rule. Reviewer at RC-3a can revisit if the skill feels under-specified during T-02 / T-03 exercises.
- **Pointer-fetch policy emerged as the load-bearing complexity.** Most of Phase 3's specificity goes to the URL-fetch surface (success → snapshot; failure → surface to operator with 3 choices; no fetch capability → fall back to manual paste). This was the operator's Phase 2 directive in spec-write; the SKILL.md gives it explicit step-by-step in Phase 3 step 4 plus a dedicated WHAT NOT TO DO clause.
- **Persona-frame fixity.** SKILL.md OP #6 explicitly forbids asking the operator to pick a persona-frame at intake. This is the codification of design-spec §5.6 amendment sub-change F (intake's persona-frame is "anyone"). One sentence in the spec, one principle in the skill.

**Next task pointer:** T-02 — Synthetic validation exercise. Dependencies (T-01) satisfied. No new blockers surfaced.

## 2026-05-17 — T-02: Synthetic validation exercise

**Status:** done
**Commits:** e7630c9
**Files touched:**
- New: `specs/findings/20260517-test-only-signal-synthetic-fixture/finding.md`
- New: `specs/findings/20260517-test-only-signal-synthetic-fixture/journal.md`
- Edited: `specs/20260517-finding-intake-skill/feature.md` — T-02 marked done
- Edited: `specs/20260517-finding-intake-skill/journal.md` — this entry

**Tests added:** None (inspection-based per §8 Test Strategy; the exercise itself is the test). Inspection evidence:

*Template-fidelity verification:*
- Title heading shape matches template: `# <Short title> — Finding` → produced `# Test-only signal — synthetic-fixture-naming inconsistency — Finding`.
- Status banner: all 6 lines present in declared order. Intake-set fields (Status, Domain, Date opened, Last transition) populated; triage-phase fields (Severity, Operational urgency) left in `<…|…>` enum-placeholder form.
- Intake section: all 5 fields populated (Reported by, Reported via, Captured by, Summary, External references) in declared order.
- Triage / Investigation / Route sections: **byte-for-byte identical to template** via `diff` (exit-clean).
- Journal Intake entry: all 4 fields populated (Captured by, Signal source, New status, Notes). Commented-out skeletons for later transitions stripped per T-05 living-example precedent.

*Behavioral verification:*
- 60-second-target plausibility: structured-input mode skipped all prompts; artifact produced in a single APPLY pass. Interactive mode would add only Phase 1 round-trip + Phase 2 slug-confirm, both single-keystroke.
- No triage-phase prompts: skill solicited none of Reproducibility, Scope, Severity confirmation, Triage notes.
- Persona-frame label correct: `Captured by: waseric; persona-frame: intake` regardless of who the operator is.
- Placeholder-vs-unknown convention honored: Intake fields populated with concrete values; later-phase fields in `<placeholder>` form (not "unknown").
- `Date opened` == `Last transition` at intake: both `2026-05-17`.
- Self-contained: Summary field carries the load-bearing capture in full; no external context required.

**DoD verification:**
- Synthetic artifact created (or created-and-deleted with rationale): **created and retained** — rationale below.
- Validation outcomes journaled: this entry.
- Any surfaced bugs resolved via T-01 amendment or escalated as `[blocker]`: no blockers surfaced; two advisory observations recorded below for post-RC-3a treatment.

**Decisions made:**

- **Retain-vs-delete (synthetic artifact): retain.** The directory name (`test-only-signal-synthetic-fixture`), the Reported-by field (`self (T-02 synthetic exercise — this finding is fabricated...)`), the Summary field (parenthetical "fabricated for T-02 validation"), and the journal Notes (two more "fabricated, not a real signal" disclaimers) all flag the artifact as obviously-not-real at four independent locations. Retaining gives RC-3a reviewers a paired example next to the operational-domain T-05 (`tab-display-issues`) for methodology-domain at status: intake — a structural regression reference. A future operator browsing `specs/findings/` cannot reasonably confuse it for a real finding.
- **Mode for the exercise: structured-input.** When the skill is invoked by an AI agent (as it is here), structured-input is the honest match — interactive mode would require simulating prompts I'd answer myself. Interactive mode is still in the skill contract; T-03 may exercise it if the real signal arrives via human operator.

**Spec amendments:** None. Two advisory observations parked for post-RC-3a treatment (see below).

**Surprises and learnings:**

1. **SKILL.md "copy verbatim" is ambiguous on template scaffolding.** `_template/finding.md` lines 1–22 are an HTML comment block telling the operator how to fill the template; `_template/journal.md` has both a top HTML comment block AND closing commented-out skeleton entries ("uncomment and fill as the corresponding transition happens"). These are *template scaffolding for the operator filling them in*, not artifact content. The T-05 living example strips both; this T-02 synthetic does the same. SKILL.md Phase 3 step 2/3 says "copy verbatim, then editing only…" — the resolution is "copy the artifact-bearing content verbatim, strip the template scaffolding." **Park as advisory** for post-RC-3a `spec-amend` against SKILL.md; not blocking T-03 because the existing T-05 precedent + this T-02 outcome give the right answer empirically.

2. **Short-name slug stop-word list could grow.** SKILL.md Phase 2 (and feature.md §5.2) lists 12 stop words; "signal" and "test-only" aren't included, so the derived slug for the synthetic was longer than ideal (`test-only-signal-synthetic-fixture` at 33 chars — within the ≤40 budget, but the leading "test-only-signal-" could compress). Cosmetic; recoverable via rename. **Park as advisory.**

3. **Template scaffolding observation applies upstream to the schema spec, not just SKILL.md.** If the SKILL.md's "copy verbatim" clarification lands as an amendment, the schema spec's `_template/*.md` files could *also* benefit from a short comment in their leading scaffolding saying "this comment is for the operator filling the template; strip it from the produced artifact." That's an `[advisory]` upstream finding the T-02 exercise surfaced. Routes via `spec-amend` against the schema spec (not this spec) post-RC-3a.

**Next task pointer:** T-03 — Real-signal dogfood exercise. Dependencies (T-01, T-02) satisfied. T-03 needs a real new signal selected at execution time by the operator (must not be the T-02 fabricated case; must not be the T-05 schema-spec example).

## 2026-05-17 — T-03: Real-signal dogfood exercise

**Status:** done
**Commits:** 2a6bcc3 (finding artifact, `find:` prefix per skill suggestion) + this closeout commit (spec/journal updates)
**Files touched:**
- New: `specs/findings/20260517-easy-survival-shelves-lwc-error/finding.md` (51 lines)
- New: `specs/findings/20260517-easy-survival-shelves-lwc-error/journal.md` (12 lines)
- Edited: `specs/20260517-finding-intake-skill/feature.md` — T-03 marked done; T-02 status line backfilled with commit SHA `e7630c9` per drift signal noted at this session's orientation (T-02 closeout commit 206df55 backfilled the SHA into the journal but missed the §7 T-02 status line — fixed in passing during this closeout, scope-honest since it is a one-character-class consistency fix observed by spec-execute's pre-flight verify).
- Edited: `specs/20260517-finding-intake-skill/journal.md` — this entry.

**Tests added:** None (inspection-based per §8 Test Strategy; the exercise itself is the test). Inspection evidence:

*Template-fidelity verification:*
- Title heading shape matches template: `# <Short title> — Finding` → produced `# LWC "Missing API" error on shelves in Easy Survival — Finding`.
- Status banner: all 6 lines present in declared order. Intake-set fields (Status, Domain, Date opened, Last transition) populated; triage-phase fields (Severity, Operational urgency) left in `<…|…>` enum-placeholder form.
- Intake section: all 5 fields populated (Reported by, Reported via, Captured by, Summary, External references) in declared order.
- Triage / Investigation / Route sections: **byte-for-byte identical to template** via `diff` (exit-clean). Verified with `diff /tmp/tpl_phases.txt /tmp/finding_phases.txt` on Template lines 41–69 vs. Finding lines 24–52 — empty diff output.
- Journal Intake entry: all 4 fields populated (Captured by, Signal source, New status, Notes). Commented-out skeletons for later transitions stripped per the T-02 / T-05 living-example precedent.

*Behavioral verification:*
- 60-second-target plausibility: operator effort timed at ~30–60 seconds of human interaction (PDF attach + AskUserQuestion answer on signal source + AskUserQuestion answer on apply-confirmation). Agent latency was its own thing — the NFR is operator-effort-bound, not wall-clock-bound. NFR met with comfortable headroom.
- No triage-phase prompts: skill solicited none of Reproducibility, Scope, Severity confirmation, Triage notes. Interactive mode preview showed only Intake-section content for confirmation.
- Persona-frame label correct: `Captured by: waseric (Sandlot Administrator and operator); persona-frame: intake`. The operator's real-world role (Sandlot Administrator) is recorded for context but the persona-frame is fixed to `intake` per SKILL.md OP #6.
- Placeholder-vs-unknown convention honored: Intake fields populated with concrete values; later-phase fields in `<placeholder>` form. The `Severity` and `Operational urgency` placeholders are intake-time-unknown by schema design (triage-phase fields).
- `Date opened` == `Last transition` at intake: both `2026-05-17`.
- Self-contained: Summary field carries the load-bearing capture in full; External references field carries the verbatim error text and reporter quotes from the PDF snapshot. The artifact does not require the forum URL to remain reachable or the originating PDF to be re-attached.
- Pointer-fetch policy: outcome surfaced to operator (preview text spelled out the no-live-fetch decision and rationale) and recorded in the finding's journal Notes. Not silently swallowed.

**DoD verification:** (per [feature.md §7 T-03 DoD](../20260517-finding-intake-skill/feature.md#L311))
- Real finding artifact committed in `specs/findings/`: ✓ — commit 2a6bcc3.
- Dogfood outcomes journaled: ✓ — this entry, plus the per-finding journal at [specs/findings/20260517-easy-survival-shelves-lwc-error/journal.md](../findings/20260517-easy-survival-shelves-lwc-error/journal.md).
- Any surfaced gaps resolved or escalated: ✓ — one advisory observation surfaced (pointer-fetch policy operator-supplied-snapshot gap); deferred to post-RC-3a `spec-amend`. See Surprises below.

**Decisions made:**

- **Mode for the exercise: interactive.** T-02 already exercised structured-input mode; T-03 exercises interactive mode for orthogonal coverage. The interactive round-trip was a single confirmation question (AskUserQuestion) — measurable as one keystroke from the operator's side.
- **Pointer-fetch: no live fetch attempt.** The forum is auth-walled (Sandlot's Bug Reports section requires member login; the PDF page header confirms operator is logged in as `waseric`). The operator pre-fetched and supplied a PDF snapshot at session intake. Per SKILL.md OP #3, *every* fetch decision must be surfaced — including the decision *not* to fetch. The interactive preview explicitly stated "live URL not attempted" with rationale; the finding's journal records it; this entry records it. This is the deviation the policy is designed to permit: the snapshot exists in a durable form, so a redundant live fetch would only burn time. Reviewers at RC-3a can challenge.
- **Backfill T-02 SHA in §7.** Noted as a drift signal in orientation; fixed in-line during closeout rather than a separate housekeeping commit. Scope is a one-token addition (`commit e7630c9`); it brings T-02's status line into the same shape as T-01's status line; not a content change.

**Spec amendments:** None. Two observations parked for post-RC-3a treatment (see below).

**Surprises and learnings:**

1. **Pointer-fetch policy gap: operator-supplied snapshot vs. attempt-live-fetch.** SKILL.md Phase 3 step 4 prescribes "if the invoking agent has URL-fetching capability, **attempt** a fetch on each pointer in turn" → on success snapshot, on failure surface and choose. The policy does not anticipate the case where the operator has already pre-fetched the snapshot and supplied it at session intake — common when the source is auth-walled, behind a CAPTCHA, or otherwise not agent-reachable. In this case, attempting a live fetch is redundant at best, fails at worst. The right interpretation (taken in this exercise): treat the operator-supplied snapshot as the snapshot, journal the no-live-fetch decision with rationale, do not attempt a redundant fetch. **Park as advisory** for post-RC-3a `spec-amend` against SKILL.md — proposed change: add a fourth bullet to Phase 3 step 4 explicitly handling "operator-supplied snapshot pre-exists" as a first-class branch. Not blocking T-04 (T-04 is README primary-path flip).

2. **Two-reporter pattern continues to surface the `Reported by` single-field constraint.** Same observation as the T-05 example finding's journal: when a forum thread has an initial reporter + subsequent confirmers, the schema's single `Reported by` field is overloaded. T-05 documented this as a friction observation; this T-03 reuses the same inline-list convention. Two findings now exhibit the pattern — strengthens the case for revisiting the schema's `Reported by` field shape, but not in scope for Phase B. Routes via `spec-amend` against the schema spec when accumulated evidence warrants.

3. **PDF-as-snapshot is a meaningful auxiliary path.** The 60-second target was easily met *partly because* the operator supplied a PDF instead of a URL alone. The PDF reduced agent latency (no fetch wait), reduced fetch-failure surface (no auth wall to navigate), and produced a richer verbatim snapshot than a typical headless-browser fetch would have. This is not a SKILL.md change — it's an empirical observation about operator workflow. Worth surfacing at the design-spec level if the pipeline ever formalizes "snapshot-supplied-at-intake" as a first-class signal type.

4. **Interactive mode preview was load-bearing.** In structured-input mode (T-02), I went straight to APPLY. In interactive mode (T-03), the preview gave the operator a chance to see the Intake section + pointer-fetch decision + suggested directory path before any file was written. The operator's "Apply as drafted" confirmation took ~1 keystroke; if they had wanted edits, the preview was the cheapest moment to surface them. The preview pattern justifies its place in the skill.

**Next task pointer:** T-04 — Update [specs/findings/README.md](../findings/README.md) "Creating a new finding" section to make `/finding-intake` the primary path and the manual `cp` recipe the fallback. Dependencies (T-01, T-03) satisfied. After T-04, the RC-3a checkpoint triggers (all four tasks complete) — Phase 8 session-continuity check applies at that boundary.

## 2026-05-17 — T-04: README "Creating a new finding" — flip `/finding-intake` to primary

**Status:** done
**Commits:** this closeout commit (single combined commit — README content edit + spec status + journal entry, no SHA-backfill needed)
**Files touched:**
- Edited: `specs/findings/README.md` — "Creating a new finding" section restructured per T-04 scope.
- Edited: `specs/20260517-finding-intake-skill/feature.md` — T-04 marked done; RC-3a trigger noted in status line.
- Edited: `specs/20260517-finding-intake-skill/journal.md` — this entry.

**Tests added:** None (inspection-based per §8 Test Strategy). Inspection evidence:

- **README line count:** 170 (≤200 ceiling per T-04 inspection criteria). Up from 159 pre-edit; the net addition is the primary-path paragraph + forward-pointer paragraph + Manual fallback subheading + intro sentence, offset by removal of the prior single-line forward-pointer.
- **`/finding-intake` is the first option presented:** the section opens with "Invoke the `/finding-intake` skill" + fenced slash-command code block, before any reference to the manual path.
- **Manual recipe preserved verbatim:** the 4-step list at [specs/findings/README.md:165-176](../findings/README.md#L165-L176) is byte-identical to the pre-edit content. Only header-level changes (now under `### Manual fallback (if the skill is not available)` subheading + one intro sentence "If `/finding-intake` is not available in your environment...").
- **Bundle-vs-split paragraph preserved verbatim:** the "**One finding or several?**" paragraph at [specs/findings/README.md:158](../findings/README.md#L158) is byte-identical to its pre-edit form. Placement moved from "after the manual recipe" to "before the Manual fallback subheading" — this prevents H3 scoping pulling the paragraph into the fallback subsection.
- **Cross-references resolve:** verified via `ls`:
  - `../../.agents/skills/finding-intake/SKILL.md` → `ai-tools/.agents/skills/finding-intake/SKILL.md` ✓
  - `../20260517-finding-intake-skill/feature.md` → `ai-tools/specs/20260517-finding-intake-skill/feature.md` ✓
  - `../20260517-findings-pipeline/architecture.md` → `ai-tools/specs/20260517-findings-pipeline/architecture.md` ✓
- **Forward-pointer updated:** old line ("will automate steps 1–4 once available") replaced with sentence pointing to this feature spec + the design-spec implementation sequencing.

**DoD verification:** (per [feature.md §7 T-04 DoD](../20260517-finding-intake-skill/feature.md#L338))
- README.md updated: ✓
- Committed: ✓ — this commit.
- Cross-references valid: ✓ — verified above.

**Decisions made:**

- **Bundle-vs-split paragraph placement:** moved from "after the manual recipe" to "between the primary-path description and the Manual fallback subheading." Reason: it is orthogonal (per T-04 scope: "applies regardless of which intake path"); placing it under `### Manual fallback` would scope it visually to the fallback subsection. Placement at parent `## Creating a new finding` level preserves the orthogonality. Paragraph text itself untouched per T-04's "Keep ... intact" requirement.
- **Forward-pointer placement:** moved from "end of section" to "right after the primary-path description." Reason: the pointer is now a "see also for the primary path's authoring spec," not a "see also for the future skill that doesn't exist yet" — its semantic relationship has changed, so its placement should follow. The pointer sits inline with the description of what it points to.
- **Single combined commit (no work-commit + closeout-commit split):** T-04's "work" is a README content edit, not a skill-artifact emission. No SKILL.md-suggested commit format to honor (unlike T-03's `find:` prefix). One commit titled `T-04: README ...` containing both the content change and the spec/journal closeout is cleaner than two commits with a backfill.

**Spec amendments:** None.

**Surprises and learnings:**

- **Forward-pointer semantic shift was the substantive change, not the recipe demotion.** T-04 reads as "demote the manual recipe; promote the skill." In practice, the recipe was already correct and stays verbatim under a new subheading — that part is mechanical. The substantive change is the forward-pointer paragraph going from "the skill will exist" (forecast, line 158 in the old README) to "the skill exists; here are the specs that describe it" (current state, cross-reference). Readers arriving at the README after T-04 land on the skill as the assumed default, not on a forecast.
- **README line count moved only modestly (159 → 170, +11 lines).** Most of the new content offsets removed content: the primary-path paragraph + intro sentence + subheading replace the single-line forecast paragraph. The README's overall character stays unchanged in surface area, which is the right outcome — Phase B was not supposed to bloat the schema docs.
- **RC-3a trigger arrives in this same commit.** This is the first task in Phase B whose closeout *also* triggers the spec's only feature-spec-level review checkpoint. The journal's "next task pointer" is no longer another T-XX task; it is the RC-3a checkpoint review. Phase 7 of spec-execute governs from here.

**Next pointer:** **RC-3a — Phase B Skill Review** triggers ([feature.md §9 RC-3a](../20260517-finding-intake-skill/feature.md#L354)). All four Phase B tasks complete:
- T-01: SKILL.md authored (commit 1e640c7)
- T-02: synthetic validation exercise (commit e7630c9)
- T-03: real-signal dogfood exercise (commits 2a6bcc3 + 88d9b0a)
- T-04: README primary-path flip (this commit)

Reviewer focus per [feature.md §9 RC-3a Exit criteria](../20260517-finding-intake-skill/feature.md#L360-L365):
- SKILL.md exists, ≤220 lines, frontmatter parseable. (149 lines per T-01 closeout — well under ceiling; frontmatter validated structurally.)
- T-02 synthetic artifact matches template byte-for-byte at non-input positions. (Verified at T-02 closeout via `diff`.)
- T-03 dogfood produced a real finding; effort timing recorded; pointer-fetch outcome recorded. (Verified at T-03 closeout — ~30–60s operator effort, live-fetch-not-attempted with rationale.)
- T-04 README update preserves the manual fallback and updates the forward-pointer. (Verified above.)
- No `[blocker]` findings; `[important]` findings either resolved or escalated. (Two `[advisory]` observations from T-02 + one from T-03, all deferred to post-RC-3a `spec-amend`; no `[blocker]` or `[important]`.)

Three advisory observations queued for post-RC-3a `spec-amend` against SKILL.md (and one against the schema spec):
1. **SKILL.md (from T-02):** "copy verbatim" ambiguity on template scaffolding — the top HTML comment in `_template/finding.md` and the bottom commented-out skeleton block in `_template/journal.md` are template scaffolding (instructions to the operator filling them), not artifact content. T-05, T-02, and T-03 all stripped them empirically; SKILL.md prose could be more explicit.
2. **SKILL.md (from T-02):** short-name slug stop-word list could grow ("test-only", "signal" both surfaced as candidates from T-02; T-03's "easy", "survival" did not need expansion). Cosmetic.
3. **SKILL.md (from T-03):** pointer-fetch policy gap — the policy ("attempt fetch if capable") does not anticipate the case "operator pre-supplied a snapshot at session intake." Proposed change: add explicit branch handling "operator-supplied snapshot pre-exists" to Phase 3 step 4.
4. **Schema spec (from T-02):** the template scaffolding observation applies upstream too — `_template/*.md` could carry a short note in their leading comments saying "this comment is for the operator filling the template; strip it from the produced artifact."

Per [spec-execute Phase 7](../../.agents/skills/spec-execute/SKILL.md) — stop here, do not proceed past RC-3a without explicit user confirmation.

## 2026-05-17 — Review of RC-3a

**Reviewer:** Claude (AI agent, reviewing on behalf of waseric)
**Outcome:** pass with comments
**Tasks reviewed:** T-01, T-02, T-03, T-04
**Diff range:** `1e640c7^..HEAD` (7 commits)

**Blockers:** 0
**Important:** 0
**Advisory:** 5

### Phase 1 — Orient

Checkpoint contract quoted verbatim from [feature.md §9 RC-3a](feature.md#L356-L369). Review focus: schema fidelity, 60-second NFR, pointer-fetch policy, peer-skill conventions, README flip. Exit criteria: 5 items spanning SKILL.md shape, T-02 byte-for-byte match, T-03 real finding + timing + fetch outcome, T-04 manual-fallback preservation, no blockers/importants.

Tasks-in-scope status (per [§7 task statuses](feature.md#L221-L347)): all four done; commits 1e640c7 (T-01), e7630c9+fe923e8 (T-02 + SHA backfill), 2a6bcc3+88d9b0a (T-03 work + closeout), 9f7a6d3 (T-04 combined).

Diff shape: 8 files; +499 / −22. One new SKILL.md (149 lines); two new finding directories (synthetic + dogfood); README delta 159→170 lines; spec + journal updates.

Initial drift signals: (1) SKILL.md Phase 3 step 3 + WHAT NOT TO DO both prescribe "leave/preserve commented skeletons" — produced artifacts strip them per T-05 precedent. (2) T-03 closeout commit (88d9b0a) bundled a T-02 SHA backfill; acknowledged in journal as a one-token consistency fix.

### Phase 2 — Scope verification

All four tasks stayed within declared scope. T-01 produced exactly the declared file. T-02 and T-03 produced their declared finding directories (paired `finding.md` + `journal.md`). T-04 edited only the "Creating a new finding" section of the schema README. No undeclared files touched; no new dependencies, frameworks, or top-level abstractions introduced.

Scope observation `[advisory]`: T-03 closeout (88d9b0a) bundled T-02 status-line SHA backfill alongside T-03 closeout work. Acknowledged in journal as scope-honest, but cleaner pattern is a dedicated housekeeping commit. Logged as advisory A4.

### Phase 3 — Review focus walk

1. **Schema fidelity:** Triage/Investigation/Route sections of both produced findings byte-for-byte identical to template (verified via `diff`). Status banner shape, field ordering, persona-frame labels, placeholder-vs-unknown convention honored. **Drift surfaced:** SKILL.md prose ("leave commented skeletons in place" / "skeletons preserved verbatim") contradicts produced artifacts (skeletons stripped per T-05 precedent). Logged as advisory A1. Verdict on focus item: pass with comments.

2. **60-second NFR plausibility in T-03 dogfood:** ~30–60s operator effort timed. NFR honored with comfortable headroom. Journal correctly distinguishes operator effort (NFR-bound) from agent latency (not NFR-bound). Verdict on focus item: pass.

3. **Pointer-fetch policy:** Spirit honored — no-live-fetch decision surfaced in interactive preview and journaled per OP #3. Snapshot durability achieved via operator-supplied PDF. **Letter mismatch:** SKILL.md Phase 3 step 4 ("attempt a fetch on each pointer in turn") does not contemplate the "operator-supplied snapshot pre-exists" branch T-03 took. Logged as advisory A2. Verdict on focus item: pass with comments.

4. **Peer-skill conventions:** All 12 required structural sections present in declared order. All 9 INPUTS fields covered. All 4 declared anti-goals enumerated in WHAT NOT TO DO (plus 4 spec-consistent additions). Line count 149 — under the 220-line ceiling; below the 180 soft floor by implementer's rationale (skill is inherently simpler than peer skills — no diagnostic logic, no governance phases, no Discovery/Clarify phases). Cross-references resolve. Verdict on focus item: pass.

5. **README flip:** `/finding-intake` is the first option presented. Manual cp recipe preserved byte-identical under `### Manual fallback (if the skill is not available)`. Bundle-vs-split paragraph preserved verbatim and re-placed at parent-section level. Forward-pointer updated from forecast to present-tense cross-reference. All three new cross-references resolve. README line count 170 (≤200 inspection ceiling). Verdict on focus item: pass.

### Phase 4 — Exit criteria

All five met. The "byte-for-byte at non-input positions" criterion is met under the "scaffolding-is-not-artifact-content" reading the implementer adopted; the prose/artifact contradiction at scaffolding positions is logged as advisory A1.

### Phase 5 — Generic quality pass

Markdown-only methodology repo. Security, observability, performance, reliability, backward compatibility, configuration: all `[ok]` or N/A. Manual cp recipe preservation satisfies the backward-compatibility NFR.

### Phase 6 — Tests and documentation

Per [§8 Test Strategy](feature.md#L348-L354), validation is inspection-based + exercise-based. Each task's acceptance criteria has citeable inspection evidence in the corresponding closeout journal entry. SKILL.md is itself the operator-facing documentation; the schema README's "Creating a new finding" section is the consumer entry point. Both present and updated.

### Advisory findings (5)

- **A1 (SKILL.md):** Template-scaffolding "copy verbatim" prose drift. SKILL.md Phase 3 step 3 + WHAT NOT TO DO both prescribe leaving/preserving commented skeletons; both produced journals strip them per T-05 precedent. Resolution: `/spec-amend` against SKILL.md to align prose with established precedent. **Recommend resolving before Phase C authoring begins** so Phase C inherits the corrected pattern.
- **A2 (SKILL.md):** Pointer-fetch policy lacks "operator-supplied snapshot pre-exists" branch in Phase 3 step 4. Resolution: `/spec-amend` adding the fourth branch explicitly.
- **A3 (SKILL.md):** Short-name stop-word list could grow ("test-only", "signal" surfaced from T-02). Cosmetic. Resolution: `/spec-amend`, low priority.
- **A4 (commit hygiene):** T-03 closeout commit (88d9b0a) bundled T-02 SHA backfill. Acknowledged in journal as scope-honest. Forward guidance: dedicated housekeeping commits for future drift-fix work. No re-run required for this checkpoint.
- **A5 (schema spec — upstream):** `_template/*.md` leading scaffolding comments could carry a self-describing "strip-from-artifact" note. Routes via `/spec-amend` against the [findings-pipeline-schema feature spec](../20260517-findings-pipeline-schema/feature.md), not this spec.

### Spec amendments proposed

- Three SKILL.md amendments (A1, A2, A3) — recommend bundling into one `/spec-amend` cycle, with A1 prioritized before Phase C authoring.
- One schema-spec amendment (A5) — routes against the upstream Phase A spec.

### Next action

RC-3a closes. Work may proceed past Phase B.

**Recommended sequence:**
1. **`/spec-amend` against [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md)** bundling advisories A1 + A2 + A3. A1 is the load-bearing one — resolve before Phase C authoring so the new `finding-triage` skill adopts the corrected template-handling pattern from the start rather than replicating the contradiction.
2. **`/spec-amend` against [the findings-pipeline-schema feature spec](../20260517-findings-pipeline-schema/feature.md)** for A5 — can be deferred without Phase C friction; batch with A1+A2+A3 only if convenient.
3. **`/spec-write` for Phase C — `finding-triage` skill** against the upstream design spec. RC-3 (design-spec joint checkpoint Phase B + Phase C) remains open and triggers when Phase C completes; this RC-3a closeout contributes Phase B's evidence to that future review.

## 2026-05-17 — Amendment 2026-05-17-1

**Section amended:** [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) Phase 3 steps 2 & 3 + WHAT NOT TO DO "Do not rewrite or paraphrase the templates" bullet
**Trigger:** RC-3a review (commit 7ba74ba) advisory A1 — template-scaffolding "copy verbatim" prose drift; SKILL.md prose contradicted three living examples that all strip template scaffolding (T-05, T-02, T-03).
**Reason:** Empirical pattern across three findings (strip scaffolding) was correct; SKILL.md prose (preserve scaffolding) was wrong. Pre-Phase-C is the right moment to land the correction so the downstream `finding-triage` skill inherits the right pattern.
**Impact summary:** No tasks affected (T-02/T-03 ratified, not invalidated); no checkpoints reopened; no completed work invalidated; A5 (schema-spec amendment, separate /spec-amend cycle) is the cross-side companion.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** SKILL.md has no Status banner (skill artifact, not spec); no change. Feature spec stays at "Draft — Open for Review."
**Commit:** this amendment closeout commit

### Full record

**Trigger.** RC-3a review (commit 7ba74ba) advisory A1, surfaced first at T-02 closeout (journal.md:111) and reaffirmed by T-03's identical handling. SKILL.md prose prescribed preserving template scaffolding ("Leave the commented skeletons for later transitions in place"; produced artifacts match canonical templates "byte-for-byte at every position not occupied by operator input" including "the commented skeletons preserved verbatim"). Three living examples (T-05 `tab-display-issues`, T-02 synthetic, T-03 dogfood) all strip the leading HTML comment in `_template/finding.md` (lines 1–22) and the closing commented-out skeleton block in `_template/journal.md` (lines 29–84) on the empirical reading that those are *operator-facing scaffolding* (instructions for filling the template), not artifact content.

**Section.** SKILL.md Phase 3 step 2 ("Write `finding.md`"); Phase 3 step 3 ("Write `journal.md`"); WHAT NOT TO DO "Do not rewrite or paraphrase the templates" bullet.

**Change.**

Phase 3 step 2 — added bullet: `**Strip the leading HTML comment block** (template lines 1–22) — it is operator-facing scaffolding for filling the template, not artifact content.`

Phase 3 step 3 — added bullet for the leading comment (`Strip the leading HTML comment block` for template lines 1–18); replaced "Leave the commented skeletons for later transitions in place" with `**Strip the closing commented-out skeleton block** (template lines 29–84). These are operator-facing scaffolds for *future* status transitions — they will be added back, uncommented and filled, by downstream skills (triage, investigation, route) at the moment of each transition. Including them at intake conflates "this phase has not started" with "here is a pre-filled template of what this phase will look like."`

WHAT NOT TO DO templates bullet refined: "byte-for-byte at every position not occupied by operator input" → "byte-for-byte at every *artifact-bearing* position not occupied by operator input"; added explicit enumeration of which template ranges are scaffolding vs. content.

**Reason.** The prose contradicted three living examples (T-05, T-02, T-03) which had all converged on the same reading: template scaffolding is operator-facing instruction, not artifact content, and gets stripped. The empirical pattern is correct (artifacts shouldn't carry "fill this in" comments meant for the template-user); the prose should match. Pre-Phase-C is the right moment to land this so the new `finding-triage` skill inherits the corrected pattern from the start.

**Impact.**
- **Affected tasks:** None. T-02 and T-03 already executed under the empirical interpretation.
- **Affected checkpoints:** RC-3a (closed by this remediation); RC-3 (still open; amendment improves Phase B's contribution).
- **Completed work invalidated:** None.
- **Cross-references requiring follow-up:** A5 (schema-spec amendment, separate /spec-amend cycle) is the cross-side companion.

**Status implication.** No status change.

**Approver.** waseric (2026-05-17).

## 2026-05-17 — Amendment 2026-05-17-2

**Section amended:** [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) Phase 3 step 4 (External-pointer handling)
**Trigger:** RC-3a review advisory A2 — Phase 3 step 4 enumerates fetch-success / fetch-failure / no-fetch-capability branches but does not contemplate "operator pre-supplied a snapshot at session intake."
**Reason:** T-03 surfaced the case fresh (operator attached an auth-walled forum PDF). T-03 made the right call (treat the snapshot as the snapshot; do not redundantly fetch) but the contract gave no explicit guidance. Adding the branch converts good-judgment behavior into contracted behavior.
**Impact summary:** No tasks affected; no checkpoints reopened; no completed work invalidated. T-03's pointer-fetch handling is ratified, not changed.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** No change.
**Commit:** this amendment closeout commit

### Full record

**Trigger.** RC-3a review (commit 7ba74ba) advisory A2, surfaced fresh at T-03 dogfood. T-03 operator pre-supplied a PDF snapshot of an auth-walled forum thread at session intake. Current Phase 3 step 4 enumerated three branches (fetch-success / fetch-failure / no-fetch-capability) but none contemplated the "operator pre-supplied a snapshot" case. T-03 took the correct judgement call (treat the supplied snapshot as the snapshot; do not attempt redundant live fetch; journal the no-live-fetch rationale per OP #3) but the policy text gave no explicit guidance.

**Section.** SKILL.md Phase 3 step 4 (External-pointer handling).

**Change.** Added a new branch (the second sub-bullet, after the "Require SUMMARY" bullet and before the "If the invoking agent has URL-fetching capability" bullet): "**If the operator pre-supplied a snapshot at session intake** (a PDF, screenshot, copy-pasted content, or other inline capture of the URL's content): treat the operator-supplied snapshot as the snapshot for the `External references` field. Prefix it `<!-- fetched <DATE> (via operator-supplied snapshot) -->` and quote the load-bearing portion verbatim. Do **not** attempt a redundant live fetch unless the operator explicitly requests one — but per OP #3, journal the no-live-fetch decision with rationale (the policy applies to *every* fetch decision, including the decision not to fetch)." Reordered the subsequent fetch-capability bullet to say "if the invoking agent has URL-fetching capability **and no operator-supplied snapshot exists**, attempt a fetch on each pointer in turn" — making the precedence explicit.

**Reason.** Operator-pre-supplied snapshots are a meaningful path the original policy didn't anticipate. T-03's case (auth-walled forum, operator already logged in, PDF on desktop) is not edge — it's a common operational shape. Adding the branch explicitly converts good-judgment behavior into contracted behavior, makes the right answer obvious to future operators and agents, and avoids the antipattern of attempting a redundant live fetch that will probably fail (the URL was auth-walled precisely so the agent couldn't reach it).

**Impact.**
- **Affected tasks:** None. T-03 already executed under the correct interpretation.
- **Affected checkpoints:** RC-3a (closed); RC-3 (still open).
- **Completed work invalidated:** None.
- **Cross-references requiring follow-up:** None. WHAT NOT TO DO bullet on silent fetch failures (SKILL.md:143) already covers "journal the decision" in its general form; the amendment makes the specific operator-supplied case explicit without contradicting it.

**Status implication.** No status change.

**Approver.** waseric (2026-05-17).

## 2026-05-17 — Amendment 2026-05-17-3

**Section amended:** [.agents/skills/finding-intake/SKILL.md](../../.agents/skills/finding-intake/SKILL.md) Phase 2 (Short-name derivation — stop-words bullet)
**Trigger:** RC-3a review advisory A3 — stop-word list could grow with validation/test-context boilerplate ("test", "test-only", "signal", "fixture", "sample", "example") surfaced empirically at T-02.
**Reason:** T-02's synthetic slug `test-only-signal-synthetic-fixture` (33 chars) carried three boilerplate tokens that the operator would naturally have dropped given a one-step accept/edit. Codifying them lets the agent produce the right slug on first proposal. Cosmetic improvement.
**Impact summary:** No tasks affected; no checkpoints reopened; T-02's existing slug not retroactively renamed (retain-vs-rename decided at T-02 closeout in favor of retain).
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** No change.
**Commit:** this amendment closeout commit

### Full record

**Trigger.** RC-3a review advisory A3, surfaced at T-02 closeout. T-02 produced the slug `test-only-signal-synthetic-fixture` (33 characters — within the ≤40 budget but compressible). The leading `test-only-signal-` is validation/test-context boilerplate that adds no distinguishing meaning to a slug. T-03's signal-bearing slug `easy-survival-shelves-lwc-error` did not surface the same need. Cosmetic — recoverable via rename — but accumulating these in the stop-word list is cheaper than fixing slugs case-by-case.

**Section.** SKILL.md Phase 2 short-name derivation, stop-words bullet.

**Change.** Extended the bullet's parenthetical list. Before: only common English stop words (`the`, `a`, `an`, etc.). After: appended sentence "Also drop validation/test-context boilerplate (`test`, `test-only`, `signal`, `fixture`, `sample`, `example`) when the slug retains ≥3 signal-bearing word-pieces — these add length without distinguishing meaning."

**Reason.** Empirical: T-02's synthetic slug carried three boilerplate tokens that the operator would naturally have dropped given a one-step accept/edit prompt. Codifying them as stop words lets the agent produce the right slug on first proposal. The "≥3 signal-bearing word-pieces" guard prevents pathological over-compression on findings that legitimately have only test-related content.

**Impact.**
- **Affected tasks:** None. T-02's existing slug is not retroactively renamed.
- **Affected checkpoints:** None.
- **Completed work invalidated:** None.
- **Cross-references requiring follow-up:** [feature.md §5.2 "Short-name derivation"](feature.md#L168) mirrors the stop-word list in its design-rationale text. **Flagging as a downstream follow-up** — the feature spec section is the design rationale and SKILL.md is the operative artifact, so they can drift slightly, but a one-line "see SKILL.md for the canonical stop-word list" or matching update in §5.2 would close the loop. Not bundled into this amendment per spec-amend's surgical-not-sprawling principle.

**Status implication.** No status change.

**Approver.** waseric (2026-05-17).

## 2026-05-17 — Spec closeout — Phase B complete

**Status:** done
**Commits:** this closeout commit
**Files touched:**
- Edited: `specs/20260517-finding-intake-skill/feature.md` — status banner flipped from `Draft — Open for Review` → `Complete`; §5.2 stop-word list now cross-references SKILL.md Phase 2 as the canonical list (closes amendment 2026-05-17-3's deferred follow-up).
- Edited: `specs/20260517-finding-intake-skill/journal.md` — this entry.

**Tests added:** None (housekeeping; inspection-based).

**DoD verification:**
- Status banner reflects post-RC-3a state: ✓ — feature.md:3 now `> Status: Complete`.
- Amendment 2026-05-17-3 deferred follow-up resolved: ✓ — §5.2 stop-word bullet now points at SKILL.md as the canonical list rather than carrying a stale partial enumeration. Cross-reference resolves (`../../.agents/skills/finding-intake/SKILL.md` from this spec dir).
- No other §7 tasks outstanding; all four (T-01 through T-04) closed.

**Decisions made:**
- **Status value: `Complete`** (not `Reviewed — Phase B Complete` or other variants). Matches existing repo precedent — one other spec uses `> Status: Complete`; the rest use `> Status: Draft — Open for Review`. Single-word state is the terser convention.
- **§5.2 update shape: cross-reference, not mirror.** Amendment 2026-05-17-3's deferred-follow-up note offered two options ("see SKILL.md" pointer *or* matching update). Chose the pointer — duplicates less, decays less. SKILL.md is the operative artifact; feature.md §5.2 is the design rationale and should defer to the operative source for the canonical list.

**Spec amendments:** None. This is closeout housekeeping; the spec is functionally unchanged.

**Surprises and learnings:**
- The spec's executable surface area is exhausted at this commit. No T-XX task in §7 remained open; the only outstanding work was (1) the status banner (routine post-checkpoint update) and (2) the §5.2 stop-word cross-reference (the one deferred follow-up explicitly identified in amendment 2026-05-17-3's "Cross-references requiring follow-up"). Both are sub-task scope.
- Closing out the spec with a dedicated journal entry (rather than rolling it into the last amendment closeout) preserves the convention that *spec state changes* — including reaching the terminal `Complete` state — are first-class journal events.

**Next pointer:** **Outside this spec.** Per [RC-3a review's recommended sequence](#next-action), the natural next step is `/spec-write` for Phase C — the `finding-triage` skill — against the upstream [findings-pipeline design spec](../20260517-findings-pipeline/architecture.md). Phase B's contribution to the design-spec RC-3 (joint Phase B + Phase C review) is now sealed.

## 2026-05-17 — Amendment 2026-05-17-4

**Section amended:** specs/20260517-finding-intake-skill/feature.md §6 NFRs (appended new `Skill portability` row; replaced `Context economy` row); §5.1 SKILL.md shape L152 (rewrote Phase 1 ORIENT description). Cascading file changes: new bundled template files at [.agents/skills/finding-intake/_template/finding.md](../../.agents/skills/finding-intake/_template/finding.md) and [.agents/skills/finding-intake/_template/journal.md](../../.agents/skills/finding-intake/_template/journal.md); SKILL.md updates to Phase 1 ORIENT (template/README reads), Phase 3 APPLY steps 2–3 (line-range strips → marker strips; host-or-bundled source), and WHAT NOT TO DO section (strip mechanism references).
**Trigger:** Cascading from Amendment 2026-05-17-2 to schema spec (commit `38a4afb`), which committed scaffold-marker delimiters as the strip mechanism and templates as skill-bundled-with-host-override. This amendment brings `finding-intake` into conformance. Originating finding: [intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/finding.md).
**Reason:** The path-coupling and line-number-coupling identified in the originating finding both manifest in `finding-intake/SKILL.md` (host-relative `../../../specs/findings/_template/...` paths break under global skill install; hardcoded line ranges couple the skill to template line layout). Both are fixed by bundling templates inside the skill directory with host-override semantics and using scaffold markers carried by the templates themselves. The schema is no longer resolved from a host README at runtime — the skill's own prose carries the operational schema knowledge it needs.
**Impact summary:** No tasks re-opened (T-01 through T-04 stay closed); no checkpoints re-opened (RC-3 and RC-3a stay closed). RC-3 advisory A-3 (portability liability flag) is satisfied by this amendment. Cross-reference: Amendment 5 ([finding-triage-skill](../20260517-finding-triage-skill/feature.md) + [.agents/skills/finding-triage/SKILL.md](../../.agents/skills/finding-triage/SKILL.md)) applies the same conformance pattern to the triage skill.
**Approver:** waseric
**Approved on:** 2026-05-17
**Status implication:** kept at `Complete`. The amendment reverses no design decision, re-opens no §7 task. Explicit operator confirmation captured during the amendment session.
**Commit:** `651fbd8`

### Full record

**Trigger.** Cascading from Amendment 2026-05-17-2 to [specs/20260517-findings-pipeline-schema/feature.md](../20260517-findings-pipeline-schema/feature.md) (commit `38a4afb`), which committed scaffold-marker delimiters as the strip mechanism and templates as skill-bundled-with-host-override at the schema level. The `finding-intake` skill is the principal consumer of those templates; bringing it into conformance is the next cascade step.

**Section(s) and files.** Feature spec §6 NFR (append) + §6 `Context economy` row (replace) + §5.1 SKILL.md shape L152 (replace); plus new files `.agents/skills/finding-intake/_template/finding.md` and `.agents/skills/finding-intake/_template/journal.md`; plus edits to `.agents/skills/finding-intake/SKILL.md` Phase 1 ORIENT, Phase 3 APPLY steps 2–3, and WHAT NOT TO DO section. Six changes bundled. Full before/after diffs are in the Phase 2 draft of this amendment in the calling session; the After blocks have been applied verbatim.

**Change summary.**
1. feature.md §6 NFR — appended `Skill portability` row referencing the [Atomic-Skill Portability Principle](../tech-stack.md#atomic-skill-portability-principle).
2. feature.md §6 NFR — replaced `Context economy` row to remove the "loads from `specs/findings/`" wording (replaced with bundled-template/host-override wording).
3. feature.md §5.1 L152 — rewrote Phase 1 ORIENT description to reference bundled-template + host-override mechanism and embedded schema knowledge.
4. `.agents/skills/finding-intake/_template/finding.md` (new) — verbatim copy of [specs/findings/_template/finding.md](../findings/_template/finding.md) in its post-amendment-3 scaffold-marker form.
5. `.agents/skills/finding-intake/_template/journal.md` (new) — verbatim copy of [specs/findings/_template/journal.md](../findings/_template/journal.md) in its post-amendment-3 scaffold-marker form.
6. `.agents/skills/finding-intake/SKILL.md` — Phase 1 ORIENT rewritten to use bundled-with-host-override + embedded schema; Phase 3 APPLY step 2 rewritten to strip by scaffold markers from resolved-template source; Phase 3 APPLY step 3 rewritten to strip both scaffold-marker blocks (leading + closing skeletons); WHAT NOT TO DO L148 rewritten to reference marker mechanism.

**Reason.** The originating finding's path-coupling and line-number-coupling both manifest in `finding-intake/SKILL.md`: host-relative `../../../specs/findings/_template/...` paths break under global skill install; hardcoded line ranges (`finding.md` 1–22; `journal.md` 1–18 and 29–84) couple the skill to template line layout. Both are fixed by this conformance amendment: templates are bundled in the skill directory (with host-override semantics for host customization); strips use scaffold markers carried by the templates themselves. The schema is no longer resolved from a host README at runtime — the skill's own prose carries the operational schema knowledge it needs.

**Impact.**
- **Affected tasks:** T-01 in §7 (authored SKILL.md) is already done. This amendment retroactively brings SKILL.md into conformance with a methodology-level principle just committed; T-01 stays marked done.
- **Affected checkpoints:** RC-3 (Intake & Triage Skill Review) and RC-3a (Phase B SKILL.md self-review) both already closed. The conformance amendment was explicitly flagged in RC-3 advisory A-3 as a portability liability to address before out-of-repo adoption; this amendment satisfies that advisory.
- **Completed work invalidated:** No. The skill's behavior is operationally equivalent at the canonical state (same templates, same strip outcome); the change is in *where* the templates come from and *how* the strip is identified.
- **Cross-references requiring follow-up:** Amendment 5 ([finding-triage-skill/feature.md](../20260517-finding-triage-skill/feature.md) + [.agents/skills/finding-triage/SKILL.md](../../.agents/skills/finding-triage/SKILL.md)) — the same conformance pattern applied to the triage skill.

**Status implication.** Spec stays at `Status: Complete`. The amendment reverses no design decision, re-opens no §7 task, and is cascading conformance work flowing from the principle committed in tech-stack.md and cascaded through the design spec and schema spec.

**Approver.** waseric — approved as drafted on 2026-05-17, with explicit confirmation to keep the spec at `Complete`.
