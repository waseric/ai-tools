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
