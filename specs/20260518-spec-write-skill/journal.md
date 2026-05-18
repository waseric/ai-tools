# `spec-write` Skill — Journal

This journal continues the **N≥2 mining structure** established at N=1 ([specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md)) and refined at N=2 ([specs/20260518-spec-design-skill/journal.md](../20260518-spec-design-skill/journal.md)). Section headings are stable across retroactive-spec journals; future sessions (sessions 4–5 of the legacy quintet — `spec-execute`, `spec-review`, `spec-amend` retroactive specs) find the same slots.

This is the **N=3 instance** in the retroactive-spec sequence and **session 2** of the legacy-quintet sequence per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). This session also resolves the strategy doc's **N=2 inflection-point** decision on `docs/retroactive-spec-pattern.md`.

## 2026-05-18 — Retroactive design spec authored

**Status:** draft — awaiting CP-1 review (deferred to fresh session per N=1 / N=2 precedent)
**Artifact:** [architecture.md](./architecture.md)
**Companion:** [journal.md](./journal.md) (this file)
**Trigger:** Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 2 ordering. Strategy doc pre-resolved audience, verification commitment, batched CP-2, and the N=2 inflection-point question framing; this session executed against that strategy.

### N=2 "Pattern for N=3" callouts — validation outcomes

This is the load-bearing addition to the N=3 journal: each callout from the [N=2 journal](../20260518-spec-design-skill/journal.md) is recorded as validated, refined, or rejected with reasoning. Future sessions (N=4, N=5) read this table first.

| N=2 callout | Outcome at N=3 | Notes |
|---|---|---|
| Predecessor-artifact distinction (authoritative for design rationale vs authoritative for current behavior) | **Validated, load-bearing** | This skill's predecessor ([docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225) is the **richest predecessor in the quintet** — full prompt artifact + design-notes block (lines 207–225) where the assistant explained design choices (three-phase pause, objectively-verifiable AC, per-task DoD, deployable-state checkpoints, mandatory Out-of-Scope, "we got this design wrong" risk). §3 Background and §14 References explicitly distinguish authoritative-for-design-rationale from authoritative-for-current-behavior. §8 Validation Approach calls out the three commits (`49c15f0`, `e483466`, `6d158fb`) that account for evolution between predecessor and shipping SKILL.md, so CP-2 reads divergences as evolution rather than drift. The N=2 prediction held exactly: without this distinction, CP-2 would have flagged every recommendation-not-in-SKILL.md as drift. |
| Authoring date, not strategy-doc-anticipated date, for directory slug | **Validated** | Today is 2026-05-18 (same date as N=2's spec). Used `20260518-spec-write-skill/`. Strategy doc was committed 2026-05-17; date refinement honored. |
| Batched CP-2 trigger (fires when *all five* quintet CP-1s plus project-constitution CP-2 condition holds, not when this single spec's CP-1 passes) | **Validated** | §9 CP-2 declares the batched trigger, with the additional refinement that N=2's CP-1 is now noted as already-passed (pass with comments on 2026-05-18), narrowing the remaining trigger condition to "three sibling quintet CP-1s + project-constitution CP-2." |
| Off-by-one section-heading citations (from N=2 CP-1 review): cite the heading line, not the body's first line | **Validated, applied** | All §3 citations to `tech-stack.md` use the heading line as the section start: `§21-33` (ASPP heading at line 21), `§44` (constraint heading line), `§48` (conventions heading line), `§51` (spec-driven-development convention heading line). §9 CP-1 review focus explicitly carries this discipline forward as a check item. |
| Source-file selection includes a *negative-signal* row | **Validated** | Applied at Phase 1 with three negative-signal rows: `spec-path-convention/architecture.md`+`feature.md`, `session-economy/architecture.md`, and `docs/spec-design-recommendations.md` (sibling-skill predecessor, not this skill's predecessor). The pattern caught real risk: `spec-design-recommendations.md` is *tempting* to treat as relevant to a spec-writing-related skill, but it is `spec-design`'s predecessor, not `spec-write`'s. The negative-signal row prevented confabulation. |
| Retroactive specs for already-shipping skills use `/spec-design`, not `/spec-write` | **Validated** | The shape of this session (no atomic tasks, design-spec §7/§8/§11 form, paired with journal) is correct for descriptive work on a shipped skill. No friction. |
| Audience reusable verbatim | **Validated** | Used N=1/N=2's audience line word-for-word. No friction. |
| Light verification suffices for the legacy quintet | **Validated** | Light verification was sufficient. No external claims in this spec required WebFetch. All citations repo-internal. |
| §13 OQ framing for known gaps; name the gap, don't resolve it | **Validated, with refinement** | Phase 2 surfaced four OQ candidates. Operator triage: three placed in §12, one dropped (no spec-level gap). §13 reports "none surfaced" honestly with a triage-outcomes table that records what was considered. **Refinement at N=3:** the "name don't resolve" discipline extends to "name the *triage*, even when no OQ survives" — silence about whether OQs exist is dishonest; explicit "none surfaced" with a triage record is honest. CP-1 review focus explicitly checks that the "none surfaced" report is faithful. |
| §2 Non-goals explicitly include "redesign of the skill", "modification of the shipping SKILL.md", "template for sibling specs", "tooling-spec" | **Validated** | All present in §2 Non-goals, plus N=3 addition: "Specifying when an operator should choose `spec-write` over `spec-design`" (the format-question gap from N=2 §13 OQ-1, named as out-of-scope rather than re-raised). |
| Both CP-1 (faithfulness) and CP-2 (drift audit) declared as named checkpoints | **Validated** | Both declared. CP-2 trigger updated to reflect spec-design's CP-1 being already-passed. |
| Stable section headings across journals | **Validated** | This journal uses the N=2 section headings (Source-file selection, Format choice, Naming pattern, etc.) plus the inherited "Pattern for N-1 — validation outcomes" table. The protocol stabilizes at N=3. |
| Predecessor scan in Phase 1: candidates for `spec-write` — *may have predecessor in `docs/` (operator to confirm at session 2)* | **Validated and exercised** | Predecessor found and confirmed: [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225. N=2's call-ahead held — this skill *does* have a predecessor and the distinction was needed. **Pattern for N=4 (carry-forward):** candidates for remaining sessions per [N=2 journal](../20260518-spec-design-skill/journal.md): `spec-execute`, `spec-review`, `spec-amend` — likely no predecessor (already-shipping siblings extended by the trilogy commit); session 3 (`spec-execute`) confirms or refutes. |
| N=1 amendment 2026-05-17-1 pattern: path/citation-class amendments add Phase-1 grep step | **Carried forward, not exercised** | No amendments triggered in this session. |

### Source-file selection (decision + rationale)

The explicit table appeared in the session's Phase 1 Discovery Report. Repeated here for journal completeness:

| File | Used? | Rationale |
|---|---|---|
| [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md) | Yes — authoritative for current behavior | The skill itself. |
| [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225 | Yes — authoritative for design rationale (NOT for current behavior) | Predecessor of this skill. Cited Inspirational in §14. **Pattern for N=3 (from spec-design at N=2) validated by this entry.** |
| [specs/tech-stack.md](../tech-stack.md), [specs/mission.md](../mission.md), [specs/roadmap.md](../roadmap.md) | Yes — authoritative for constraints, audience, lifecycle position | Constitutional bindings. |
| [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) + [journal.md](../20260518-spec-design-skill/journal.md) | Yes — N=2 retroactive-spec source | Closest-sibling structural source. Journal's "Pattern for N=3" callouts validated above. |
| [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) | Yes — N=1 retroactive-spec source | Original structural source. |
| [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) | Read — orientation only | Cited in §3, §11, §12. Not used as a source for §4/§5 architectural commitments. |
| [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) + [feature.md](../20260515-spec-path-convention/feature.md) | Negative signal | Modifies spec-write's OUTPUT FORMAT (commit `6d158fb`) but does not architecturally describe the skill. |
| [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) | Negative signal | Mentions spec-write only as a propagation target for session-economy disciplines. |
| [docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md) | Negative signal — sibling-skill predecessor, not this skill's | Authoritative for `spec-design`'s rationale, not `spec-write`'s. Tempting to confuse; flagged as negative signal to prevent confabulation. |

### N=2 inflection-point decision — `docs/retroactive-spec-pattern.md`

Per [docs/retroactive-spec-strategy.md §"N=2 inflection point"](../../docs/retroactive-spec-strategy.md), this session's close is where the decision is made. Two options on the table:

- **(a) Author `docs/retroactive-spec-pattern.md`** capturing the validated patterns; sessions 3–5 cite the pattern doc instead of re-deriving from prior journals.
- **(b) Defer further or abandon**; sessions 3–5 continue mining prior journals individually.

**Analysis.** After authoring this N=3 spec:

| Consideration | Finding |
|---|---|
| Did I need a pattern doc to write this spec? | No. Mining the N=2 journal directly was sufficient. The N=2 "Pattern for N=2 — validation outcomes" table was exactly the right structure to consume. |
| How many N=2 patterns are retroactive-quintet-specific vs general spec-authoring? | Roughly half / half. Quintet-specific: audience verbatim, light verification, batched CP-2, redesign-non-goal, modify-SKILL.md-non-goal, predecessor-distinction. General: negative-signal rows, directory slug, "name OQ don't resolve", stable section headings, grep-for-class-of-references discipline. |
| Will the quintet-specific patterns recur unchanged at N=4 and N=5? | Likely yes — they are essentially predictable inheritance. |
| Cost of authoring `docs/retroactive-spec-pattern.md` now | Medium. Real doc to write; non-trivial maintenance burden to keep in sync with journals. |
| Risk of authoring now | The N=2 callout about predecessor-artifact distinction is a perfect counter-example: it did not exist at N=1, was *added* at N=2, validated at N=3. If we had codified at N=2, we would have prematurely committed before the pattern was tested. Codifying at N=3 risks the same against N=4/N=5 inventions. |
| Risk of deferring | Sessions 4 and 5 mine the N=3 (this) journal as primary input; that mining is *cheap* relative to authoring a pattern doc. If patterns stabilize cleanly across N=3, N=4, N=5, the N=5-close decision has three corroborating data points. |

**Leaning.** **Defer** (option b). The journal-mining protocol IS the pattern; codifying a meta-pattern now risks freezing it before N=4 and N=5 have a chance to refine it. Sessions 4 and 5 continue mining prior journals; the decision is revisited at N=5 close with three data points of corroboration rather than two.

**Operator decision.** *Confirmed by operator at session 2 close: defer. Sessions 4 and 5 mine prior journals individually; the `docs/retroactive-spec-pattern.md` decision is revisited at N=5 close.*

### New "Pattern for N=4" callouts

Candidates for future-session validation. Recorded here, not declared as binding.

1. **"None surfaced" §13 reporting with triage table.** When all OQ candidates triage to §12 or drop, §13 reports "none surfaced" with the triage outcomes recorded. Silence about whether OQs were considered is dishonest; explicit triage-record is honest. CP-1 reviews check that "none surfaced" is faithful, not silent omission. **First exercise at N=3.** Carry to N=4 for validation.

2. **Spec-design's CP-1 already-passed adjusts CP-2 trigger narrative.** This spec's §9 CP-2 trigger names the remaining CP-1 requirements ("three sibling quintet CP-1s + project-constitution CP-2") rather than the original five — because N=2's CP-1 has already passed. As more quintet CP-1s pass, each subsequent spec's CP-2 trigger declaration narrows. **Pattern for N=4:** declare the remaining CP-1 requirements at session time, not the full original set.

3. **Off-by-one section-heading citation discipline elevated to CP-1 review focus.** N=2 CP-1 flagged one off-by-one section-heading citation. This session deliberately cites heading lines (not body-first lines) and elevates the check to CP-1 review focus explicitly. **Pattern for N=4:** all section-heading citations cite the heading line; CP-1 review focus carries this as an explicit item.

4. **Predecessor-richness varies; spec accommodates by adjusting §8 Validation Approach.** This skill's predecessor is rich (~210 lines of prompt + design notes). `spec-design`'s predecessor was also rich (254 lines). The remaining three skills (`spec-execute`, `spec-review`, `spec-amend`) likely have *thin or no* predecessor — they were already-shipping siblings extended by the trilogy commit. **Pattern for N=4:** §8 Validation Approach's "Predecessor cross-check" row is conditional on predecessor existing. If no predecessor, drop the row and replace with another validation channel (sibling-skill cross-check, perhaps).

### Format choice — design spec vs feature spec

Validated. The shipping skill is `/spec-design`; the operator invoked it for a retroactive design spec on `spec-write`. No friction.

**Pattern (carried from N=1, validated at N=2 and N=3).** Retroactive specs for already-shipping skills use `/spec-design`.

### Naming pattern — directory slug

`specs/20260518-spec-write-skill/architecture.md`. Today is 2026-05-18.

**Pattern (carried from N=2, validated).** Authoring date, not strategy-doc-anticipated date.

### Audience framing

Reused verbatim from N=1/N=2: "Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set."

**Pattern (carried from N=1/N=2, validated).** Audience is reusable verbatim across the legacy quintet.

### Verification commitment level

**Light verification**, per N=1/N=2 precedent and per Phase 2 confirmation. The spec text contains no external claims requiring WebFetch — all citations are repo-internal.

**Pattern (carried from N=1/N=2, validated).** Light verification is the correct default for the legacy quintet. Escalation triggers must be made explicit when authoring a retroactive spec for a skill that currently cites external systems.

### Open-question framing — handling known gaps

§13 reports **no Open Questions** with a triage-outcomes table. Per the refined "name the gap, don't resolve it" discipline:

- **Named:** all four Phase-2 candidate OQs are recorded in the §13 triage table with their disposition.
- **Not resolved:** three deferred to §12 (out of scope) with framing; one dropped (no spec-level gap).
- **Honest reporting:** "none surfaced" is the honest summary of triage outcomes, not silent omission.

**Decision process:** the operator was asked explicitly (Phase 2 `AskUserQuestion` covering four candidates) where each belonged. Operator chose §12, §12, drop, §12 in order. The triage table in §13 records the protocol.

**Pattern (carried from N=2, refined at N=3).** When all candidates triage to §12/drop, §13 reports "none surfaced" with a triage table. Silence is dishonest; explicit triage-record is honest.

### Drift-audit-as-checkpoint (CP-2)

Both CP-1 (faithfulness) and CP-2 (drift audit) declared in §9.

**Refinement at N=3.** CP-2 trigger narrative narrows: "three remaining quintet CP-1s pass, AND CP-1 of [spec-design](../20260518-spec-design-skill/architecture.md) has passed (already done — pass with comments on 2026-05-18), AND project-constitution's CP-2 has either run or been folded into the batch." Each subsequent quintet retroactive spec's CP-2 trigger declaration narrows further as CP-1s land.

**Pattern for N=4+.** Each remaining quintet retroactive spec declares the same batched CP-2 trigger, narrowing as CP-1s pass.

### Scope discipline — what was kept out

§2 Non-goals lists five items explicitly. §12 Out of Scope lists ten items, eight inherited from N=2 / strategy-doc / N=1 and two new for N=3:
- (new) The N=2 inflection-point decision on `docs/retroactive-spec-pattern.md` — explicitly relegated to journal, not spec body.
- (new) Resolving the format-question-prompt gap — explicitly named as N=2's OQ-1, not re-raised here.

**Pattern (carried from N=1/N=2, validated).** Retroactive specs are descriptive, not prescriptive. The list of explicit exclusions grew from N=1's four to N=2's thirteen to N=3's fifteen — most additions are *inheritances*.

### Cross-session knowledge transfer

This journal is the canonical N=3 mining input for sessions 4 and 5. Specifically:

**What this journal commits to:**
- The "Pattern for N=3 — validation outcomes" table above is the structural pattern for N≥3 journals.
- The N=2 inflection-point decision (defer; revisit at N=5 close) is recorded above; sessions 4 and 5 do not re-raise it.
- The four new "Pattern for N=4" callouts are candidates for future-session validation.

**What this journal does NOT commit to:**
- A `docs/retroactive-spec-pattern.md`. Decision deferred to N=5 close (see above).
- A binding template for sessions 4–5. The mining protocol is the pattern; the journal-mining table is the structural addition; neither is a fillable template.
- Predicting predecessor existence at sessions 4 and 5. Each session scans for predecessors as part of Phase 1 source-file selection; the spec-design and spec-write journals predict "likely no predecessor for the trilogy-extended skills" but this is hypothesis, not commitment.

### Friction observed

Honest record of where this session encountered friction. Useful for sessions 4–5 to anticipate.

- **Predecessor-doc scope ambiguity.** [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) covers *three* skills (spec-write, spec-execute, spec-review) in one 687-line conversation export. Identifying the spec-write-specific lines (17–225 for the prompt artifact + design notes) required care; the rest of the doc covers spec-execute and spec-review (their predecessors). **Pattern for N=4:** at sessions 3 (`spec-execute`) and 4 (`spec-review`), the same predecessor doc is re-read — different line ranges scoped to those skills. The doc is a shared predecessor for three skills, not three separate predecessors.
- **§13 "none surfaced" tension.** All four Phase-2 OQ candidates triaged to §12 or drop. Reporting "no OQs" felt suspicious initially — every retroactive spec so far had at least one OQ. Held the line by writing the explicit triage-outcomes table; honest reporting beats forcing an OQ that does not exist. CP-1 review focus carries this as an explicit check item. **Pattern for N=4:** "none surfaced" is acceptable; the triage record is what matters.
- **N=2 inflection-point decision required journal-level synthesis.** The decision IS the journal entry (not spec body), but the analysis required reading the strategy doc, mining the N=2 journal, and judging the cost/risk of authoring a meta-pattern doc against the cost of mining journals individually. The leaning section above captures the synthesis; the operator decision is what closes it.
- **Authoring date matches N=2 (both 2026-05-18).** Directory slug differentiates by skill name (`spec-design-skill` vs `spec-write-skill`), not by date — no friction, just noting that two consecutive sessions on the same calendar day produce sibling directories with the same date prefix.

### Conversation grounding

- Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 2 ordering. Confirmed scope explicitly via `AskUserQuestion` before Phase 1.
- Phase 1 (Discovery) produced source-file table including three negative-signal rows; landscape orientation against the lifecycle skill family; constraint orientation against four constitutional citations; conversation grounding (strategy doc + N=1/N=2 specs and journals as inputs); naming candidates not needed (name fixed by skill name); **predecessor confirmed and scoped** to [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 17–225.
- Phase 2 (Clarify) surfaced five operator decisions via `AskUserQuestion`: four OQ candidate placements (3 × §12, 1 × drop), and CP-1 timing (defer to fresh session). The N=2 inflection-point decision was surfaced as a journal-level decision (not a Phase 2 candidate) and is recorded above.
- No `[blocker]` open questions arose. Session proceeded to Phase 3 (spec document + journal authoring).

### Tasks defined

None. Design spec, not feature spec. The "next work" is review (CP-1) and audit (CP-2), declared in §9.

### Next action pointer

Three steps, in order:

1. **Commit** the spec + journal as a paired commit. This is the closing action of session 2.
2. **CP-1 review** in a fresh session: operator invokes `/spec-review` against [architecture.md §9 CP-1](./architecture.md#cp-1--retroactive-spec-faithfully-describes-the-shipping-skill).
3. **Session 3 — `spec-execute` retroactive spec.** N=4 in retroactive-spec sequence; **N=3 robustness check** per [strategy doc ordering](../../docs/retroactive-spec-strategy.md) — most divergent shape (iterative, multi-task, branch-state-aware, paired-commit-aware vs. single-shot authoring). The four "Pattern for N=4" callouts above are inputs.

No `[blocker]` open questions; the spec is ready for CP-1.

## 2026-05-18 — Review of CP-1

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Verdict commit:** `e72ec35`
**Diff range:** `a753259` (paired commit introducing [architecture.md](./architecture.md) and this journal)
**Tasks reviewed:** none (retroactive design spec — no atomic tasks)
**Blockers:** 0
**Important:** 0
**Advisory:** 2 — (a) [§5.7](./architecture.md) commits "§14 References distinguishes Authoritative (binding) from Inspirational (prior art)" but [spec-write SKILL.md PHASE 3 §14](../../.agents/skills/spec-write/SKILL.md) does not require the split; useful formalization imported from `spec-design`, not a contradiction. Mirrors [N=2 advisory (a)](../20260518-spec-design-skill/journal.md). (b) [§3 Background](./architecture.md) and [§8 Validation Approach](./architecture.md) lean on chains of references to N=1 / N=2 journal callouts; coherent for the named audience, dense for an outside adopter. Carry-forward of [N=2 advisory (d)](../20260518-spec-design-skill/journal.md).
**Spec amendments proposed:** none

### Review focus walk — itemized outcomes

1. Every commitment in §4/§5/§6 corresponds to behavior present in SKILL.md — **pass with comments** (one advisory on §5.7's authoritative/inspirational claim).
2. No commitment contradicts the shipping SKILL.md — **pass**.
3. ASPP correctly characterized as binding (§3, §6) including absent-input behavior (§5.4) — **pass**. Citation to [tech-stack.md §21-33](../tech-stack.md#L21-L33) is on the heading line; N=2 off-by-one carry-forward fully resolved.
4. Predecessor doc distinguished as authoritative-for-design-rationale-not-current-behavior (§3, §14) — **pass**. The three evolution-explaining commits (`49c15f0`, `e483466`, `6d158fb`) all verified to have touched [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md).
5. §13 "none surfaced" is honest reporting backed by triage table — **pass**. First exercise of N=3 candidate Pattern-for-N=4 #1; recommend carrying forward to sessions 3 and 4.
6. Self-contained per Operating Principles — **pass with comments** (advisory on recursive-session framing density for outside readers; carry-forward from N=2).
7. Section-heading citation discipline — **pass**. All four §3 citations to tech-stack.md (`§21-33`, `§44`, `§48`, `§51`) point to the heading/section-start line.

### Exit criteria status

- Reviewer verdict issued in structured Phase 7 format: **met**.
- All blocker findings resolved or escalated to `/spec-amend`: **met** (zero blockers).
- Verdict written back to spec §9 status line and journal: **met** (this entry; spec [§9 CP-1 Status](./architecture.md#cp-1--retroactive-spec-faithfully-describes-the-shipping-skill) updated in same change).

### Pattern observation at N=3 CP-1

[N=2's Pattern-for-N=3 callout on CP-1 reviews](../20260518-spec-design-skill/journal.md) said retroactive-spec CP-1 reviews are primarily verification of citations and traceability, with the natural failure mode being broken or wrong references. **Validated again at N=3.** The CP-1 walk surfaced zero concrete-evidence findings; the two advisories are interpretive (over-commitment that imports a sibling-spec convention; density of recursive-session framing). Pattern stable across N=1, N=2, N=3: keep citation-walking as the primary discipline.

### Pattern observation: Pattern-for-N=4 #1 first-exercise

Pattern-for-N=4 #1 from the [N=3 journal](./journal.md) (*"None surfaced" §13 reporting with triage table*) was the load-bearing N=3 addition under review. CP-1 verified it passes the honesty check: §13's "none surfaced" claim is backed by an explicit triage row per candidate. **Carry-forward to N=4 confirmed at review time, not just at authoring time** — the pattern is now corroborated by two data points (authored at N=3, reviewed-as-honest at N=3 CP-1). N=4 and N=5 sessions may invoke this pattern with reduced friction.

### Next action

[§11 Adoption Path step 2](./architecture.md) is now closed. Step 3 (batched CP-2 drift audit) remains pending its declared trigger: three remaining quintet CP-1s pass + project-constitution CP-2 disposition. Per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session ordering, the next session is session 3 — `spec-execute` retroactive spec (N=4 in retroactive-spec sequence; the robustness check on the most-divergent skill shape).

The two advisory findings are non-blocking. They may be folded into a future amendment if other §3 or §5.7 edits are queued; otherwise they are simply recorded here.

## 2026-05-18 — Review of CP-2

**Reviewer:** Claude (agent reviewer)
**Outcome:** pass with comments
**Tasks reviewed:** N/A — retroactive design spec; CP-2 audit scope was [architecture.md §4, §5, §6, §12](./architecture.md) against shipping [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md).
**Blockers:** 0
**Important:** 0
**Advisory:** 5 — D-1 OP-7 "No bloat" + WND-3 "no new deps without Alternatives Considered" lacks explicit §5/§6 home; D-2 §5.7 + §4 Vocabulary commit to Authoritative/Inspirational §14 split that SKILL.md §14 does not require (CP-1 advisory (a) carry-forward); D-3 SKILL.md OUTPUT FORMAT "All code blocks must specify a language" absent from spec §6; D-4 SKILL.md WND-4 task-phrasing rule has no explicit §5.5 home; D-5 SKILL.md preamble line 15 undersells Phase 2 by omitting "decisions proposed-unilaterally" — internal SKILL.md inconsistency.
**Spec amendments proposed:** 5 — D-1 → (a) amend spec §6; D-2 → (b) amend SKILL.md §14; D-3 → (a) amend spec §6; D-4 → (a) amend spec §5.5; D-5 → (b) amend SKILL.md preamble.

### Findings against CP-2 review focus (line-by-line audit of SKILL.md against spec §4, §5, §6, §12)

| ID | Where | Divergence | Routing |
|---|---|---|---|
| D-1 | [SKILL.md OP-7:52](../../.agents/skills/spec-write/SKILL.md#L52) + [WND-3:197](../../.agents/skills/spec-write/SKILL.md#L197) vs spec §5 / §6 | OP-7 ("No bloat. Match existing codebase conventions. Do not introduce new dependencies, frameworks, or abstractions unless justified in writing under 'Risks and Tradeoffs.'") and WND-3 ("Do not introduce a new framework, ORM, or major dependency without an explicit subsection in 'Alternatives considered' justifying it.") both commit to the same no-new-deps-without-justification rule. Spec §5 has no Detailed Design subsection; spec §6 has no NFR row. Same finding class as N=1 D-4 (README-reconciliation) and N=2 D-4 (Inline citation preference): "WHAT NOT TO DO commitment lacks explicit §5/§6 carrier." | **(a) amend spec** — add §6 NFR row covering OP-7 + WND-3. |
| D-2 | [architecture.md §5.7:200](./architecture.md#L200) + [§4 Vocabulary:87](./architecture.md#L87) vs [SKILL.md §14:180-182](../../.agents/skills/spec-write/SKILL.md#L180-L182) | Spec commits "§14 References distinguishes Authoritative (binding) from Inspirational (prior art)." SKILL.md §14 has single bucket: "Links to the patterns, RFCs, library docs, internal docs, and prior code that informed the design." Spec commits beyond SKILL.md's silence — same shape as N=2 D-1 (Status banner lifecycle). Carry-forward from CP-1 advisory (a). | **(b) amend SKILL.md** — SKILL.md §14 catches up to spec's tighter contract. Parallels N=2 amendment 2026-05-18-1. |
| D-3 | [SKILL.md OUTPUT FORMAT line 189](../../.agents/skills/spec-write/SKILL.md#L189) — no parallel in spec | SKILL.md commits "All code blocks must specify a language for syntax highlighting." Spec §6 NFRs and §5.8 Section template don't carry this. Direct N=2 D-3 (Markdown hygiene) parallel. | **(a) amend spec** — add Markdown hygiene NFR row to §6 (same row text as N=2 amendment 2026-05-18-3 appropriate). |
| D-4 | [SKILL.md WND-4:198](../../.agents/skills/spec-write/SKILL.md#L198) — no explicit §5.5 home | SKILL.md WND-4: "Do not write task descriptions in the form 'Implement X.' Write them in the form 'Add `<file>` exposing `<function>` such that `<acceptance criteria>`.'" Spec §5.5 lists Scope (files, function/class names) and AC (objectively verifiable) implying the positive form, but neither surfaces the anti-pattern explicitly. | **(a) amend spec §5.5** — operator override of proposed (c) accept-as-minor. Add an explicit line to §5.5 Behavior surfacing the task-description anti-pattern. |
| D-5 | [SKILL.md preamble line 15](../../.agents/skills/spec-write/SKILL.md#L15) vs [SKILL.md PHASE 2 body lines 75-79](../../.agents/skills/spec-write/SKILL.md#L75-L79) | Preamble: "pause at Phase 2 for user input on **assumptions and open questions**." Phase 2 body enumerates three items: Assumptions, Open questions, **Decisions you propose to make unilaterally**. Preamble omits the third item. Internal SKILL.md inconsistency; mirror-class of N=2 D-2 (preamble names "format" that Phase 2 body does not enumerate — same class, opposite direction). Spec §5.2 has the complete list correctly. | **(b) amend SKILL.md preamble** — minor text edit aligning preamble with Phase 2 body. No open-OQ interaction. |

### Verification performed

- Walked SKILL.md INPUTS (lines 17-30) against [spec §5.1 Inputs:112](./architecture.md#L112) — all 10 inputs present, ordering matches (FEATURE_NAME … CONSTITUTION_PATHS, with DESIGN_SPEC_PATH / CONSTITUTION_PATHS optional).
- Walked SKILL.md OPERATING PRINCIPLES (7 items, lines 44-52) against spec §5 + §6 — OP-1 (§5.1), OP-2 (§5.2 implicit via OQ triage), OP-3 (§5.5 + §6 Atomicity), OP-4 (§5.6 + §6 Tests-first), OP-5 (§5.7 + §6 Citation discipline), OP-6 (§5.5 + §6 Reversibility) all mapped. **OP-7 ("No bloat") — D-1 surfaces the gap.**
- Walked SKILL.md PHASE 1 (lines 54-71) against [spec §5.1:108-122](./architecture.md#L108-L122) — Discovery Report contents match (upstream-spec orientation, codebase, conventions, components, deps, touch surface, tests, observability). KEY_ENTRY_POINTS auto-discovery noted.
- Walked SKILL.md PHASE 2 (lines 73-81) against [spec §5.2:124-138](./architecture.md#L124-L138) — assumptions, OQ triage, decisions-proposed-unilaterally all present; pause-and-wait + blockers-before-Phase-3 present. **D-5 surfaces preamble-vs-body inconsistency.**
- Walked SKILL.md PHASE 3 (lines 83-182) against [spec §5.3:140-154](./architecture.md#L140-L154) + [§5.8:208-237](./architecture.md#L208-L237) — 14-section template matches; §1-§6 share structure with design specs; §7/§8/§11 feature-spec-specific commitment matches SKILL.md's section-content notes. Per-section sub-field catalog gap accepted as known minor (same precedent as N=1 D-3 INPUTS catalog).
- Walked SKILL.md OUTPUT FORMAT (lines 184-191) against [spec §6 NFRs:240-254](./architecture.md#L240-L254) — Pairing, Multi-repo awareness, Conciseness (no-marketing-language) present. **D-3 surfaces code-block-language gap.** Items 1 ("Phase 1/2 may be conversational") and 3 ("repo-relative paths") accepted as known minor (implicit / convention-followed).
- Walked SKILL.md WHAT NOT TO DO (lines 193-202) against spec §5 / §6 — WND-1 (§5.1), WND-2 (§5.2 + §6 Pause discipline), WND-5 (§5.5 task sizing), WND-6 (§4 vocabulary + §5.5/§5.6), WND-7 (§5.4 DESIGN_SPEC_PATH contradiction), WND-8 (§5.4 CONSTITUTION_PATHS scope) all mapped. **WND-3 — D-1.** **WND-4 — D-4.**
- Walked spec §12 Out of Scope (10 bullets) — five are spec-level deferrals (no SKILL.md mirror expected); three are inheritances from N=1 / N=2 / strategy doc OQs and correctly cite their sources; two are non-goal mirrors of §2 (redesign, modify-SKILL.md). No drift surfaced in §12.
- Cross-checked CP-1 advisories: (a) Authoritative/Inspirational §14 split — **reinstated here as D-2 with explicit routing**; (b) §3 / §8 recursive-references density — outside CP-2 surface (§3 not in audit scope), passes through.
- Verified section-heading citation discipline: [spec §3:44-48](./architecture.md#L44-L48) cites tech-stack.md §21-33 (ASPP heading at line 21 — correct), §44 (AI context window limits bullet — correct content), §48 (Repository layout bullet — correct content), §51 (Spec-driven-development convention bullet — correct content). Bullet-start citations under sub-section headings, not top-level §-headings; CP-1's "heading/section-start line" claim accurate. **N=3 callout (iii) CP-1 reviewer-error pattern does NOT fire here** — CP-1 elevated citation discipline to a review-focus item and the citations were correct.
- Verified amendment-ID citation: [spec §11:333](./architecture.md#L333) cites N=1 amendment 2026-05-17-1 (the `.claude/skills/...` removal). Correct.
- Verified ASPP citation: [spec §3:46](./architecture.md#L46) and [§6 Portability NFR:243](./architecture.md#L243) both cite [tech-stack.md §21-33](../tech-stack.md#L21-L33) — heading line for ASPP, correct.

### Exit criteria status

- Divergence list produced (possibly empty): met — five advisory divergences.
- Each divergence has a routing decision: met — D-1 (a), D-2 (b), D-3 (a), D-4 (a), D-5 (b).
- No silent edits to either artifact: met — this entry and the §9 Status line are the only writes; amendments await operator-invoked `/spec-amend`.
- Outcome recorded as closing entry of retroactive-spec adoption: met — this entry; final closeout follows once amendments apply.

### Pattern observations for N=4

- **CP-2 audit shape stable at N=3.** N=1 found 4 advisory divergences, N=2 found 5, N=3 found 5. Range remains 2–5 per N=1 baseline. Finding shapes recurring: missing NFR row (D-3 mirrors N=2 D-3 verbatim; D-1 mirrors N=1 D-4 / N=2 D-4 in *shape*), spec-commits-beyond-SKILL.md (D-2 mirrors N=2 D-1 in shape), SKILL.md preamble-vs-Phase-body inconsistency (D-5 mirrors N=2 D-2 in shape, opposite direction).
- **"WHAT NOT TO DO partial home" finding class** confirmed across N=1 (D-4 README-reconciliation), N=2 (D-4 inline-citation), N=3 (D-1 no-bloat). Three data points: stable cross-skill pattern. Future CP-2 audits should walk WHAT NOT TO DO items against §5 / §6 carriers as a first-class step.
- **CP-1 reviewer-error pattern (N=3 callout iii) did NOT fire.** CP-1 explicitly elevated section-heading citation discipline to a review-focus check item, and the citations were correct. Pattern: when CP-1 elevates a discipline to explicit check, CP-2 does not re-find it. Suggests CP-1 review-focus item elevation is the right intervention.
- **Phrasing-decision matrix (N=3 callout iv) did NOT fire** — no open OQs in spec §13. The D-2 (b) and D-5 (b) routings touching SKILL.md do not interact with any OQ. Matrix remains a candidate pattern; first-exercise awaits a future session where SKILL.md edit interacts with an open OQ.
- **Status-banner-lifecycle finding class (N=3 callout i) did NOT fire** — feature-spec template doesn't include a Status banner; finding class is design-spec-specific. Pattern: callouts may be format-specific (design-spec vs feature-spec); future quintet specs (spec-execute, spec-review, spec-amend retroactive specs) are design-spec form and may surface this class.
- **D-4 operator override (proposed (c) → routed (a)).** Reviewer proposed accept-as-known-minor on protocol-detail grounds; operator chose to amend spec §5.5. Pattern: protocol-detail accept-as-minor is the reviewer's default heuristic; operator may prefer explicit surfacing where the protocol detail is a real anti-pattern (not just a procedure). Future CP-2 reviewers should surface protocol-detail findings explicitly for operator choice rather than absorbing them silently.

### Next action

1. Operator invokes `/spec-amend` for each of the five amendments. Amendments may be bundled or sequential per operator preference; natural bundles: (a) D-1 + D-3 + D-4 all touch architecture.md (one paired bundle or three surgical commits), (b) D-2 + D-5 both touch SKILL.md (potential bundle).
2. Cross-skill pattern observations queued for the [batch journal closing summary](../20260518-cp2-batch-audit/journal.md); the N=3 per-spec entry is appended to the batch journal in the same shape as N=2's entry.
3. After amendments commit, the spec-write retroactive-spec adoption is **closed for this session's CP-2 routing**. The next per-spec CP-2 (N=4) is `spec-execute` per the batch audit's authoring-order default.

## 2026-05-18 — Amendment 2026-05-18-1

**Section amended:** [architecture.md §6 Non-functional Requirements](./architecture.md#L239)
**Trigger:** CP-2 D-1 — SKILL.md OP-7 "No bloat" + WND-3 no-new-dependency rule have no §5/§6 carrier.
**Reason:** Adds explicit Dependency hygiene NFR row so the §6 catalog is faithful to SKILL.md's binding commitments.
**Impact summary:** No tasks affected (retroactive design spec); CP-2 closing entry references this amendment; no completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`pass with comments` on CP-2 holds; missing-NFR-carrier fill, same shape as N=1 D-4 and N=2 D-4 precedents).
**Commit:** 2c416fc

### Full record

## Amendment 2026-05-18-1 — specs/20260518-spec-write-skill/architecture.md §6

**Trigger.** During the N=3 CP-2 drift audit of `spec-write` (2026-05-18, Claude as agent reviewer), a line-by-line walk of [SKILL.md OPERATING PRINCIPLES OP-7 (line 52)](../../.agents/skills/spec-write/SKILL.md#L52) and [SKILL.md WHAT NOT TO DO WND-3 (line 197)](../../.agents/skills/spec-write/SKILL.md#L197) against the spec's §5 and §6 showed both commitments — the no-bloat principle and the no-new-dependency-without-Alternatives-Considered prohibition — name the same rule, yet the spec has no §5 subsection and no §6 NFR row carrying it. Same finding class as N=1 D-4 (README-reconciliation NFR) and N=2 D-4 (Inline-citation preference NFR): a WHAT NOT TO DO commitment without an explicit §5 / §6 carrier.

**Section.** [specs/20260518-spec-write-skill/architecture.md §6 Non-functional Requirements](./architecture.md#L239), table rows after the Multi-repo awareness row (line 254). The table grows from twelve rows to thirteen.

**Change.**

Before:
> | **Multi-repo awareness** | If the spec lives in a different repo than the codebase it describes, §3 Background notes this and the spec includes `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` for downstream `spec-execute` sessions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |
>
> (end of table)

After:
> | **Multi-repo awareness** | If the spec lives in a different repo than the codebase it describes, §3 Background notes this and the spec includes `SPEC_REPO_ROOT` / `SPEC_TARGET_BRANCH` for downstream `spec-execute` sessions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |
> | **Dependency hygiene** | Specs do not introduce new frameworks, ORMs, or major dependencies without an explicit "Alternatives considered" subsection justifying the addition. Default is to match existing codebase conventions; deviation requires written rationale. | [SKILL.md OPERATING PRINCIPLES §7 + WHAT NOT TO DO](../../.agents/skills/spec-write/SKILL.md) |

**Reason.** SKILL.md commits OP-7 ("No bloat") and WND-3 (the no-new-dependency-without-Alternatives-Considered prohibition) — both binding rules on every feature spec the skill produces. The shipping behavior is correct; the spec's §6 catalog is incomplete. Without an explicit NFR row, CP-2 future audits will keep re-discovering the same gap, and downstream `spec-review` runs of feature specs the skill produces have no §6-anchor for verifying the rule was applied. Adding the row makes the spec's §6 catalog faithful to SKILL.md's binding commitments.

**Impact.**
- **Affected tasks:** none (spec-write-skill is a retroactive design spec with no §7 Task Breakdown — see [§7 Implementation Sequencing](./architecture.md#L256)).
- **Affected checkpoints:** [CP-2](./architecture.md#L302) — closing entry of CP-2 references this amendment.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none (the new row is self-contained; §5 makes no claim about a dependency-hygiene subsection that would need updating).

**Status implication.** Spec remains at `pass with comments` for CP-2. The amendment is a missing-NFR-carrier fill; identical in shape to N=1 D-4 and N=2 D-4 NFR additions, both of which were ratified without spec-status regression. No revert to Draft.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-2

**Section amended:** [.agents/skills/spec-write/SKILL.md §14 References](../../.agents/skills/spec-write/SKILL.md#L180)
**Trigger:** CP-2 D-2 — spec §4 + §5.7 commit Authoritative/Inspirational §14 split; SKILL.md §14 had single undifferentiated bucket.
**Reason:** Brings SKILL.md into alignment with the spec; retires CP-1 advisory (a).
**Impact summary:** No tasks affected; CP-2 closing entry references this amendment; existing feature specs not back-amended (rule applies forward).
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`pass with comments` on CP-2 holds; SKILL.md edit not spec edit).
**Commit:** 85907c1

### Full record

## Amendment 2026-05-18-2 — .agents/skills/spec-write/SKILL.md §14

**Trigger.** During the N=3 CP-2 drift audit of `spec-write` (2026-05-18, Claude as agent reviewer), a walk of [architecture.md §5.7 Citation discipline (line 200)](./architecture.md#L200) and [§4 Vocabulary (line 87)](./architecture.md#L87) showed both commit to a §14 Authoritative/Inspirational split that SKILL.md's §14 description does not require. Same shape as the N=2 spec-design amendment 2026-05-18-1 (SKILL.md catches up to spec by adding a discipline the spec already commits to). Carry-forward from CP-1 advisory (a).

**Section.** [.agents/skills/spec-write/SKILL.md §14 References (line 180)](../../.agents/skills/spec-write/SKILL.md#L180). Single-paragraph section; the change appends discipline to it.

**Change.**

Before:
> ## 14. References
>
> Links to the patterns, RFCs, library docs, internal docs, and prior code that informed the design.

After:
> ## 14. References
>
> Links to the patterns, RFCs, library docs, internal docs, and prior code that informed the design. Distinguish **Authoritative** references (binding — the spec's commitments must match these) from **Inspirational** references (prior art that informed the design but does not bind it). Use two sub-headings — `### Authoritative` and `### Inspirational` — when both classes are present.

**Reason.** Feature specs the skill produces should make the distinction visible: a reviewer reading §14 needs to know which references are binding contracts (e.g., RFC 7807 if the spec invokes it as a pattern) versus which are merely informative (e.g., "we looked at how library X does this"). Without the split, every §14 entry reads as equally weighted. The spec-write retroactive spec already commits to the discipline; this brings the shipping SKILL.md into alignment with the spec.

**Impact.**
- **Affected tasks:** none (spec-write-skill is a retroactive design spec with no §7 Task Breakdown).
- **Affected checkpoints:** [CP-2](./architecture.md#L302) — closing entry references this amendment. CP-1 advisory (a) explicitly retired by this amendment.
- **Completed work invalidated:** none. Existing feature specs produced before this amendment may have undifferentiated §14 sections; they are not required to be back-amended (the rule applies to specs authored after this commit).
- **Cross-references requiring follow-up:** none. [architecture.md §4 Vocabulary](./architecture.md#L87) and [§5.7](./architecture.md#L200) continue to commit the discipline — they describe the rule that SKILL.md now enforces.

**Status implication.** Spec remains at `pass with comments` for CP-2. This amendment touches SKILL.md, not the spec — the spec's §14 commitment is unchanged. No revert to Draft.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-3

**Section amended:** [architecture.md §6 Non-functional Requirements](./architecture.md#L239)
**Trigger:** CP-2 D-3 — SKILL.md OUTPUT FORMAT "All code blocks must specify a language" had no §6 carrier.
**Reason:** Adds Markdown hygiene NFR row; direct parallel of N=2 D-3 with identical row text.
**Impact summary:** No tasks affected; CP-2 closing entry references this amendment; no completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`pass with comments` on CP-2 holds; NFR-row addition matching N=2 D-3 precedent).
**Commit:** cafaca8

### Full record

## Amendment 2026-05-18-3 — specs/20260518-spec-write-skill/architecture.md §6

**Trigger.** During the N=3 CP-2 drift audit of `spec-write` (2026-05-18, Claude as agent reviewer), the auditor surfaced [SKILL.md OUTPUT FORMAT line 189](../../.agents/skills/spec-write/SKILL.md#L189) — "All code blocks must specify a language for syntax highlighting" — as a binding output-format commitment with no §6 NFR carrier. Direct parallel of N=2 D-3 (`spec-design` Markdown hygiene NFR addition, commit `4b0b9c8`). Same finding shape, same routing.

**Section.** [specs/20260518-spec-write-skill/architecture.md §6 Non-functional Requirements](./architecture.md#L239), inserted **between** the Format fidelity row (line 250) and the Pairing row (line 251). The table grows from thirteen rows (after D-1) to fourteen.

**Change.**

Before:
> | **Format fidelity** | Output conforms to the 14-section template with exact headings and declared order. §7/§8/§11 use feature-spec form, not design-spec form. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-write/SKILL.md) |
> | **Pairing** | Every feature spec is accompanied by a journal at the same directory. The pair is the output. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |

After:
> | **Format fidelity** | Output conforms to the 14-section template with exact headings and declared order. §7/§8/§11 use feature-spec form, not design-spec form. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-write/SKILL.md) |
> | **Markdown hygiene** | All code blocks specify a language. Tables and lists conform to GitHub-flavored markdown rendering conventions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |
> | **Pairing** | Every feature spec is accompanied by a journal at the same directory. The pair is the output. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-write/SKILL.md) |

**Reason.** SKILL.md OUTPUT FORMAT binds the rule; the spec's §6 catalog should reflect every binding output-format commitment so downstream `spec-review` can verify per-NFR. Without an explicit row, downstream reviewers have no anchor for the rule. Matches N=2 D-3 precedent verbatim, keeping the quintet aligned.

**Impact.**
- **Affected tasks:** none (retroactive design spec; no §7 Task Breakdown).
- **Affected checkpoints:** [CP-2](./architecture.md#L302) — closing entry references this amendment.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none.

**Status implication.** Spec remains at `pass with comments` for CP-2. NFR-row addition; identical shape to N=2 D-3 which was ratified without spec-status regression. No revert to Draft.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-4

**Section amended:** [architecture.md §5.5 Task Breakdown atomicity](./architecture.md#L172) — Behavior paragraph
**Trigger:** CP-2 D-4 — SKILL.md WND-4 "Implement X" anti-pattern had no explicit §5.5 home. Operator override of reviewer-proposed (c) accept-as-minor.
**Reason:** §5.5 Behavior names the wrong shape, not just the right shape; closes the protocol-detail authorship hazard.
**Impact summary:** No tasks affected; CP-2 closing entry references this amendment; existing feature specs not back-amended (rule applies forward).
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`pass with comments` on CP-2 holds; single-clause Behavior addition).
**Commit:** 0cd3518

### Full record

## Amendment 2026-05-18-4 — specs/20260518-spec-write-skill/architecture.md §5.5

**Trigger.** During the N=3 CP-2 drift audit of `spec-write` (2026-05-18, Claude as agent reviewer), the auditor surfaced [SKILL.md WHAT NOT TO DO WND-4 (line 198)](../../.agents/skills/spec-write/SKILL.md#L198): *"Do not write task descriptions in the form 'Implement X.' Write them in the form 'Add `<file>` exposing `<function>` such that `<acceptance criteria>`.'"* Spec §5.5 declares the positive form (Scope, AC) but does not name the anti-pattern. Reviewer proposed (c) accept-as-known-minor on protocol-detail grounds (mirror of N=2 D-5 precedent); operator overrode to (a) amend spec — task phrasing is a real anti-pattern worth surfacing in the spec's Behavior contract, not absorbed silently.

**Section.** [specs/20260518-spec-write-skill/architecture.md §5.5 Task Breakdown atomicity](./architecture.md#L172), **Behavior** paragraph (line 176). The change appends one sentence to the existing paragraph.

**Change.**

Before:
> **Behavior.** Every task in the breakdown declares: Task ID (e.g., `T-01`), Title, Scope (files to create/modify; function or class names), Acceptance criteria (Given/When/Then or equivalent, objectively verifiable), Tests required (named test files; unit / integration / manual), Definition of Done (code merged, tests passing in CI, docs updated, observability in place, no new lint or type errors, peer reviewed), Dependencies (other task IDs), Estimated size (S/M/L; L tasks must be split before implementation). Tasks are sequenced so the branch is in a deployable or revertible state at each task boundary.

After:
> **Behavior.** Every task in the breakdown declares: Task ID (e.g., `T-01`), Title, Scope (files to create/modify; function or class names), Acceptance criteria (Given/When/Then or equivalent, objectively verifiable), Tests required (named test files; unit / integration / manual), Definition of Done (code merged, tests passing in CI, docs updated, observability in place, no new lint or type errors, peer reviewed), Dependencies (other task IDs), Estimated size (S/M/L; L tasks must be split before implementation). Tasks are sequenced so the branch is in a deployable or revertible state at each task boundary. Task descriptions take the form *"Add `<file>` exposing `<function>` such that `<acceptance criteria>`"* — not the form *"Implement X"*; the latter under-specifies scope and AC.

**Reason.** SKILL.md WND-4 names a real anti-pattern: tasks phrased as "Implement X" omit the scope (which file? which function?) and the acceptance criteria (what does done look like?) that make a task atomically reviewable. The spec's positive-form declaration (Scope, AC) implies the right shape but does not name the wrong shape. CP-2 future audits will keep re-finding the gap unless the §5.5 Behavior contract surfaces the anti-pattern directly. The operator override of the reviewer's accept-as-minor heuristic is intentional: this is a protocol detail that is also a real authorship hazard, not a procedural footnote.

**Impact.**
- **Affected tasks:** none (retroactive design spec; no §7 Task Breakdown).
- **Affected checkpoints:** [CP-2](./architecture.md#L302) — closing entry references this amendment.
- **Completed work invalidated:** none. Existing feature specs produced before this amendment may have under-specified task descriptions; not required to be back-amended.
- **Cross-references requiring follow-up:** none. §5.5 Why-this-design (line 180) cites the "code is written, just need to add tests later" anti-pattern; the new clause is adjacent but distinct. §6 Atomicity NFR (line 247) already cites WND-4 by reference and needs no change.

**Status implication.** Spec remains at `pass with comments` for CP-2. Single-clause addition to existing Behavior paragraph; does not change the positive contract preceding it. No revert to Draft.

**Approver.** Eric Wasgatt — approved 2026-05-18.
