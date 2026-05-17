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
**Commits:** to-be-attached at closeout commit
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
