# `spec-design` Skill — Journal

This journal continues the **N=2-mining structure** established at N=1 ([specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md)). Section headings are stable across retroactive-spec journals; future sessions (sessions 3-5 of the legacy quintet — `spec-execute`, `spec-review`, `spec-amend` retroactive specs) find the same slots.

This is the **N=2 instance** in the retroactive-spec sequence and **session 1** of the legacy-quintet sequence per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md).

## 2026-05-18 — Retroactive design spec authored

**Status:** draft — awaiting CP-1 review (deferred to fresh session per N=1 precedent)
**Artifact:** [architecture.md](./architecture.md)
**Companion:** [journal.md](./journal.md) (this file)
**Trigger:** Operator invoked `/spec-design spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 1 ordering. Strategy doc pre-resolved naming, directory slug, audience, verification commitment, and CP-1/CP-2 declaration; this session executed against that strategy.

### N=1 "Pattern for N=2" callouts — validation outcomes

This is the load-bearing addition to the N=2 journal: each callout from N=1 is recorded as validated, refined, or rejected with reasoning. Future sessions (N=3+) read this table first.

| N=1 callout | Outcome at N=2 | Notes |
|---|---|---|
| Source-file selection includes a *negative-signal* row | **Validated** | Applied at Phase 1 with three negative-signal rows: `session-economy/architecture.md`, `spec-path-convention/architecture.md`+`feature.md`, and the external private repo. The pattern caught real risk: `spec-path-convention` modifies spec-design's OUTPUT FORMAT but is *not* a source for the skill's architecture; without the negative-signal discipline, that would have been ambiguous. |
| Retroactive specs for already-shipping skills use `/spec-design`, not `/spec-write` | **Validated** | The shape of this session (no atomic tasks, design-spec §7/§8/§11 form, paired with journal) is correct for descriptive work on a shipped skill. No friction. |
| Directory slug `YYYYMMDD-<skill-name>-skill` | **Validated** | Used `20260518-spec-design-skill/`. **Refinement:** the YYYYMMDD reflects the authoring date, not a date inherited from the strategy doc. The strategy doc was committed 2026-05-17 anticipating session-1 authorship; actual authorship landed 2026-05-18. Future sessions: use the *current* authoring date, not the strategy-doc-anticipated date. |
| Audience reusable verbatim | **Validated** | Used N=1's audience line word-for-word. No friction. |
| Light verification suffices for the legacy quintet | **Validated** | Light verification was sufficient. No external claims in this spec required WebFetch. **Refinement:** the recommendations doc made two externally-verified claims (`.github/skills/SKILL.md`, `AGENTS.md`) but those landed in the *predecessor* artifact, not in the current SKILL.md or this spec. Future skill-specs that *currently make* external claims (e.g., a skill citing GitHub Copilot conventions or RFCs) must escalate to heavy verification. |
| §13 OQ framing for known gaps; name the gap, don't resolve it | **Validated** | OQ-1 (format-question-prompt gap) follows the discipline: full options analysis, no leaning, owner pointing at a future amendment session. The "name vs resolve" separation held. |
| §2 Non-goals explicitly include "redesign of the skill" and "modification of the shipping SKILL.md" | **Validated** | Both present in §2 Non-goals. |
| Both CP-1 (faithfulness) and CP-2 (drift audit) declared as named checkpoints | **Validated** | Both declared. **Refinement:** CP-2 trigger condition was updated to reflect the batch strategy (CP-2 fires when *all five* quintet CP-1s pass, not when this single spec's CP-1 passes). Future quintet specs should declare the same batch trigger. |
| Stable section headings across journals | **Validated** | This journal uses the same section headings as N=1's (Source-file selection, Format choice, Naming pattern, etc.) plus this new "N=1 Pattern for N=2 — validation outcomes" table. The table is the structural addition for N=2. |
| N=1 amendment 2026-05-17-1 pattern: path/citation-class amendments add Phase-1 grep step | **Carried forward, not yet exercised** | No amendments triggered in this session. The Phase-1 grep discipline will be applied to any future class-of-references amendment touching this spec; recording the carry-forward here so the pattern is not lost. |

### Source-file selection (decision + rationale)

Recorded in [architecture.md §"Phase 1 — Discovery Report"](./architecture.md#3-background-and-constraints) implicitly; the explicit table appeared in the session's Phase 1 Discovery Report. Repeated here for journal completeness:

| File | Used? | Rationale |
|---|---|---|
| [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) | Yes — authoritative for behavior | The skill itself. |
| [docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md) | Yes — authoritative for design rationale (NOT for current behavior) | The recommendations doc that became this skill. Cited as Inspirational in §14, *not* Authoritative for behavior. CP-2 will read this distinction. |
| [specs/tech-stack.md](../tech-stack.md), [specs/mission.md](../mission.md), [specs/roadmap.md](../roadmap.md) | Yes — authoritative for constraints, audience, lifecycle position | Constitutional bindings. |
| [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) | Yes — N=1 retroactive-spec source | Structural source. The journal's "Pattern for N=2" callouts are the validation inputs for the table above. |
| [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) | Read — orientation only | Cited in §11 Adoption Path and §12 Out of Scope for the CP-2 batch strategy and the N=2-inflection-point pointer, but not used as a source for §4/§5 architectural commitments. |
| [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) | Negative signal | Mentions `spec-design` only as a future modification target. |
| [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) + [feature.md](../20260515-spec-path-convention/feature.md) | Negative signal | Modifies spec-design's OUTPUT FORMAT (commit `4ebec0c`) but does not architecturally describe the skill. |
| A separate private design repo | Negative signal — out of repo, out of scope | `ai-frontmatter-distributor-architecture.md` lives there. Recommendations doc transmits what is portable. |

### New "Pattern for N=3" callout — distinguish design-rationale source from current-behavior source

A skill may have a **predecessor artifact** (recommendations doc, design brief, scratch notes) that captures the design rationale that became the skill, *separate from* the SKILL.md that captures current behavior. The retroactive spec must distinguish:

- **Authoritative for current behavior** — SKILL.md only. CP-1 reviews against this.
- **Authoritative for design rationale** — predecessor artifact, if it exists. Cited as Inspirational in §14. CP-2 reads divergences between predecessor and SKILL.md as *evolution*, not as drift.

The N=1 retroactive spec did not have a predecessor artifact (project-constitution emerged directly from the trilogy commit `80000b1` without a written design rationale doc) and so the distinction did not surface. This N=2 spec did have one ([docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md)) and the distinction was load-bearing — without it, CP-2 would flag every recommendation that didn't land in SKILL.md as drift, when most of those gaps are *deliberate* evolution.

**Pattern for N=3.** Each future quintet retroactive spec scans for predecessor artifacts in `docs/` early in Phase 1. Candidate predecessors for the remaining skills (operator may have additional context I lack):
- `spec-write` — may have predecessor in `docs/` (operator to confirm at session 2).
- `spec-execute`, `spec-review`, `spec-amend` — likely no predecessor; they were already-shipping siblings extended by the trilogy commit.

If a predecessor is found, distinguish "authoritative for design rationale" from "authoritative for behavior" in §3 Background and §14 References.

### Format choice — design spec vs feature spec

Validated. The shipping skill is `/spec-design`; this is a self-applying use case. No friction.

**Pattern (carried from N=1, validated).** Retroactive specs for already-shipping skills use `/spec-design`.

### Naming pattern — directory slug

`specs/20260518-spec-design-skill/architecture.md`.

**Pattern (carried from N=1, refined).** Use the **authoring date**, not a date inherited from a strategy doc or planning artifact. The strategy doc was committed 2026-05-17; actual session-1 authorship landed 2026-05-18. Sessions 2-5 should resolve the date at session start, not assume continuity with the strategy doc.

### Audience framing

Reused verbatim from N=1: "Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set."

**Pattern (carried from N=1, validated).** Audience is reusable verbatim across the legacy quintet.

### Verification commitment level

**Light verification**, per N=1 precedent and per Phase 2 confirmation. The spec text contains no external claims requiring WebFetch — all citations are repo-internal.

**Pattern (carried from N=1, validated with refinement above).** Light verification is the correct default for the legacy quintet. Escalation triggers must be made explicit when authoring a retroactive spec for a skill that currently cites external systems.

### Open-question framing — handling known gaps

§13 OQ-1 names the format-question-prompt gap. Per the "name the gap, don't resolve it" discipline:
- **Named:** the gap exists; the skill is silent on format selection.
- **Not resolved:** four options surfaced (a/b/c/d); no leaning declared; owner = future amendment or finding session.
- **Watch items:** three concrete signals that would escalate urgency.
- **Anti-goals:** two rejected approaches (silent resolution; pre-empting the N=2-inflection-point decision).

**Decision process:** the operator was asked explicitly (Phase 2 question 1) whether to promote OQ candidate 1 to §13, demote to §12, or skip. Operator chose §13. Same protocol for OQ candidates 2 (verification escalation → §12) and 3 (recursive use → §3 Background).

**Pattern (carried from N=1, validated).** The "name the gap, don't resolve it" discipline held. Triage candidates explicitly to the operator rather than picking placement unilaterally.

### Drift-audit-as-checkpoint (CP-2)

Both CP-1 (faithfulness) and CP-2 (drift audit) declared in §9.

**Refinement at N=2.** CP-2 trigger condition is **batched**: fires when all five quintet CP-1s have passed, not when this single spec's CP-1 passes. This reflects [docs/retroactive-spec-strategy.md §"Drift mitigation"](../../docs/retroactive-spec-strategy.md). Cross-skill drift patterns are only visible at batch time.

**Pattern for N=3+.** Each remaining quintet retroactive spec declares the same batch trigger in §9 CP-2.

### Scope discipline — what was kept out

§2 Non-goals lists five items explicitly. The operator's instruction "I do not want to create drift in the effectiveness of our pre-existing skills" from N=1 carries forward to this session and produced (a) the redesign exclusion, (b) the modify-SKILL.md exclusion, (c) the sibling-template exclusion, (d) the OQ-1-resolution exclusion, (e) the tooling-spec exclusion.

§12 Out of Scope additionally lists eight items, including the inherited constitution-amendment ceremony (from N=1) and cross-skill amendment coordination (from strategy doc OQ-3).

**Pattern (carried from N=1, validated).** Retroactive specs are descriptive, not prescriptive. The list of explicit exclusions grew from N=1's four to N=2's roughly thirteen — most of the additions are *inheritances* from strategy-doc OQs and N=1 itself. Future sessions accumulate inherited exclusions rather than re-derive them.

### Cross-session knowledge transfer

This journal is the canonical N=2 mining input for sessions 3, 4, 5. Specifically:

**What this journal commits to:**
- The "Pattern for N=2 — validation outcomes" table above is the structural addition for N≥2 journals. Future journals (N=3, N=4, N=5) add their own "Pattern for N-1 — validation outcomes" table, mining the prior journal in the same shape.
- The new "Pattern for N=3" callouts (predecessor-artifact distinction, authoring-date refinement, batched CP-2 trigger) are candidates for future-session validation.
- Friction observed at N=2 is recorded honestly below.

**What this journal does NOT commit to:**
- `docs/retroactive-spec-pattern.md` is not authored here. That decision is made at session 2's (`spec-write`) close per the strategy doc.
- A binding template for sessions 3-5. The "Pattern for N=2" outcomes table is the protocol; the spec body shape is the prior-art exemplar; neither is a fillable template.

### Friction observed

Honest record of where this session encountered friction. Useful for sessions 3-5 to anticipate.

- **Recursive case pull.** The skill speccing itself created a subtle pull toward self-referential framings ("this skill is doing what this spec describes right now"). Resisted by treating the session as if the operator had asked for a retroactive spec on *any* skill — keeping the §3 Background "recursion is observation, not architectural property" framing explicit. Future quintet sessions are not recursive in this sense (`/spec-design` will spec a non-self skill); this friction will not recur at sessions 2-5 unless a skill is invoked on itself again. Recording the observation so it is not lost.
- **Date shift between strategy-doc anticipation and actual authorship.** The strategy doc was committed 2026-05-17 anticipating session-1 work that landed 2026-05-18. No material consequence (directory slug uses authoring date), but the refinement is recorded above. Sessions 2-5: resolve the date at session start; do not inherit from strategy doc.
- **Recommendations-doc-as-design-rationale required a deliberate framing.** The predecessor artifact is rich (254 lines) and tempting to treat as authoritative for current behavior. Held the line in §3 Background and §14 References by distinguishing "authoritative for design rationale" from "authoritative for current behavior." Without that distinction, CP-2 would surface every recommendation-that-didn't-land as drift. **Pattern for N=3:** if a predecessor artifact exists, distinguish it explicitly. New callout above.
- **The format-question-prompt gap was hard to keep terse in §13.** The OQ surfaced four real options with non-trivial tradeoffs; the temptation was to declare a leaning. Resisted — N=1's "name the gap, don't resolve it" discipline applies here too. The OQ is honest about being open.

### Conversation grounding

- Operator invoked `/spec-design spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 1 ordering.
- Phase 1 (Discovery) produced source-file table including three negative-signal rows; landscape orientation against the lifecycle skill family; constraint orientation against three constitutional citations; conversation grounding (strategy doc + N=1 spec/journal as inputs); naming candidates not needed (name fixed by skill name).
- Phase 2 (Clarify) surfaced four operator decisions via `AskUserQuestion`: OQ candidate 1 → §13 (chosen), OQ candidate 2 → §12 (chosen), OQ candidate 3 → §3 Background (chosen), CP-1 → defer to fresh session (chosen).
- No `[blocker]` open questions arose. Session proceeded to Phase 3 (spec document + journal authoring).

### Tasks defined

None. Design spec, not feature spec. The "next work" is review (CP-1) and audit (CP-2), declared in §9.

### Next action pointer

Three steps, in order:

1. **Commit** the spec + journal as a paired commit. This is the closing action of session 1.
2. **CP-1 review** in a fresh session: operator invokes `/spec-review` against [architecture.md §9 CP-1](./architecture.md#cp-1--retroactive-spec-faithfully-describes-the-shipping-skill).
3. **Session 2 — `spec-write` retroactive spec.** The N=2 inflection point governs the `docs/retroactive-spec-pattern.md` decision; this journal's "Pattern for N=3" callouts are inputs to that decision.

No `[blocker]` open questions; the spec is ready for CP-1.

## 2026-05-18 — Review of CP-1

**Reviewer:** Claude (agent reviewer)
**Outcome:** pass with comments
**Verdict commit:** `47593ad`
**Diff range:** commit `06e554b` (specs/20260518-spec-design-skill/{architecture.md, journal.md})
**Tasks reviewed:** none — retroactive design spec; CP-1 reviews spec-vs-SKILL.md faithfulness
**Blockers:** 0
**Important:** 0
**Advisory:** 4 — (a) §5.5 introduces light/heavy verification labels not used in SKILL.md itself (accurate description, useful formalization). (b) §5.3 + §4 use framing ("ASPP justifies the inline template", "the pause is load-bearing") that goes slightly beyond SKILL.md's own emphasis — interpretation, not contradiction. (c) §3 citation reads `tech-stack.md §21-33` but the section heading is at line 20; minor off-by-one. (d) §3 Background's recursive-session framing is self-contained for methodology-literate readers but slightly opaque for an outside reader.

**Spec amendments proposed:** none. The four advisories are not material to faithfulness; candidates for folding into a future amendment if/when other §3 or §5 edits are queued, but no Amendment Protocol invocation required by this verdict.

**Findings against CP-1 review focus (all seven items):**
1. Every commitment in §4/§5/§6 corresponds to behavior present in SKILL.md — **pass** (one advisory on the light/heavy labels formalization).
2. No commitment contradicts the shipping SKILL.md — **pass** (one advisory on spec-side interpretive framing).
3. ASPP correctly characterized as binding (§3, §6) consistent with tech-stack.md §21-33 — **pass** (one advisory on off-by-one citation start).
4. Format-question-prompt gap named at first-class detail without silent resolution — **pass** (OQ-1 has Question, Analysis with 4-option table, explicit no-leaning, Owner, Watch items, Anti-goals).
5. Recommendations doc bounded as authoritative-for-design-rationale not authoritative-for-behavior — **pass** (§3 makes the distinction explicit; §14 places it under Inspirational with annotation).
6. Self-contained per Operating Principles — **pass** (one advisory on recursive-session framing clarity for outside readers).
7. Portability rule honored — **pass** (no `~/.claude/skills/...` references; the two `.claude/skills` matches are meta-references to the N=1 amendment and to the review-focus criterion itself).

**Exit criteria status:**
- Structured verdict issued — met.
- Zero blockers — met.
- Verdict written back to §9 status line + journal — met (this entry).

**Pattern observed at N=2 CP-1.** N=1's "Pattern for N=2" callout said retroactive-spec CP-1 reviews are mostly verification of *citations and traceability*, not behavioral judgment, with the natural failure mode being broken or wrong references. **Validated at N=2.** The CP-1 walk surfaced one off-by-one citation start (§3 → tech-stack.md §21-33 should be §20-33) as the only concrete-evidence finding; everything else was interpretive framing. N=3+ retroactive-spec CP-1 reviews should keep citation-walking as the primary discipline.

**Pattern for N=3.** Off-by-one section-heading citations (`§N-M` where N should be the heading line, not the body's first line) recurred at N=2 — minor but worth flagging. Future spec authors: cite the heading line as the section start.

**Next action:** Adoption Path step 2 closed. §11 step 3 — CP-2 (batched drift audit) — remains pending its declared trigger: all five quintet CP-1s plus project-constitution CP-2. The next session is session 2 of the legacy quintet — `spec-write` retroactive spec — per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). The four advisory findings above are non-blocking and may be folded into a future amendment if other §3 or §5 edits are queued.

## 2026-05-18 — Review of CP-2

**Reviewer:** Claude (agent reviewer)
**Outcome:** pass with comments
**Tasks reviewed:** N/A — retroactive design spec; CP-2 audit scope was architecture.md §4, §5, §6, §12 against shipping [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md).
**Blockers:** 0
**Important:** 0
**Advisory:** 5 — D-1 §5.8 Status-banner lifecycle commitment not declared in SKILL.md; D-2 SKILL.md preamble line 15 mentions "format" but Phase 2 enumeration omits it, and §13 OQ-1 phrasing slightly overstates the gap; D-3 SKILL.md OUTPUT FORMAT "All code blocks specify a language" absent from spec §6/§5.8; D-4 SKILL.md WHAT NOT TO DO items have partial home in spec (WND-3 inline-citation rule, WND-4 Risks/OQ distinction, WND-7 rejected-governance rule lack explicit §5/§6 carriers); D-5 "Verification pass as a discrete step" post-draft-walk protocol not captured in spec §5.5.
**Spec amendments proposed:** 4 — D-1 → (b) amend SKILL.md; D-2 → (b) amend SKILL.md; D-3 → (a) amend spec; D-4 → (a) amend spec §6; D-5 → (c) accept as known minor.

**Findings against CP-2 review focus (line-by-line audit of SKILL.md against spec §4, §5, §6, §12):**

| ID | Where | Divergence | Routing |
|---|---|---|---|
| D-1 | [architecture.md §5.8:248](./architecture.md#L248) vs [SKILL.md template line 86](../../.agents/skills/spec-design/SKILL.md#L86) | Spec §5.8 commits "The Status banner uses the lifecycle `Draft — Open for Review` → `Approved` → `Superseded`." SKILL.md template only initializes Status to `Draft — Open for Review`; lifecycle states beyond initial value are not declared in SKILL.md. Spec adds a forward-looking commitment SKILL.md is silent on. | **(b) amend SKILL.md** to declare the lifecycle. Parallels N=1 amendment 2026-05-18-1 (layout-question elevation) — SKILL.md catches up to the spec's tighter contract. |
| D-2 | [SKILL.md preamble line 15](../../.agents/skills/spec-design/SKILL.md#L15) vs [SKILL.md Phase 2 lines 66-77](../../.agents/skills/spec-design/SKILL.md#L66-L77), and [spec §13 OQ-1:374](./architecture.md#L374) | "Format" appears in SKILL.md preamble ("pause at Phase 2 for user input on naming, audience, **format**, and verification commitment") but is **not enumerated** in Phase 2 bullets. Spec §13 OQ-1 says the format-question prompt is "undocumented in SKILL.md" — partly correct (not in Phase 2 enumeration) but partly imprecise (preamble does mention format). Internal SKILL.md inconsistency. | **(b) amend SKILL.md Phase 2** to enumerate `format` as a Phase 2 bullet, parallel to N=1 amendment 2026-05-18-1 elevating `layout`. Partially resolves OQ-1's substance — OQ-1's strict claim ("undocumented") closes, but the four-option resolution (a/b/c/d) for the *content* of the prompt remains open and routes through the OQ's existing watch-items machinery. |
| D-3 | [SKILL.md OUTPUT FORMAT line 183](../../.agents/skills/spec-design/SKILL.md#L183) — no parallel in spec | SKILL.md commits "All code blocks specify a language." Spec §6 NFRs and §5.8 Section template don't carry this. | **(a) amend spec** to add a brief NFR row to §6 or a line to §5.8 Behavior. Generic markdown hygiene; surgical addition. |
| D-4 | [SKILL.md WHAT NOT TO DO lines 187-196](../../.agents/skills/spec-design/SKILL.md#L187-L196) — partial home in spec | WND-1, WND-5, WND-6, WND-8 covered in §5.1/§5.5/§5.3/§5.8. WND-2 (don't pretend implementation-ready) implicit in §5.4. **WND-3** (inline authoritative URLs at point of claim) has no explicit §5/§6 home — only reinforced by §14 References' own preamble. **WND-4** (don't conflate Risks/OQs) and **WND-7** (no rejected governance) lack explicit homes. | **(a) amend spec §6** to add NFR row(s) — at minimum an "Inline citation preference" row covering WND-3; optionally a "Risks vs Open Questions distinction" row covering WND-4. WND-2/7 remain accept-as-minor (implicit / content-specific). |
| D-5 | [SKILL.md standing disciplines lines 224-225](../../.agents/skills/spec-design/SKILL.md#L224-L225) vs [spec §5.5:182-192](./architecture.md#L182-L192) | SKILL.md describes "Verification pass as a discrete step" with specific protocol ("After §1–§14 are drafted, walk the spec and identify every external claim. Verify each against canonical sources. Cite inline."). Spec §5.5 captures the light/heavy formalization (a useful summary beyond SKILL.md's vocabulary) but not the post-draft-walk protocol. | **(c) accept as known minor.** Spec captures the principle; the post-draft-walk procedure is protocol detail, not a load-bearing commitment. |

**Verification performed:**
- Walked SKILL.md INPUTS block (lines 21-30) against spec §5.1 Inputs — all 8 inputs present, ordering matches.
- Walked SKILL.md OPERATING PRINCIPLES (9 items, lines 42-52) against spec §5 + §6 NFR table — items 1-9 each mapped to a §5 subsection or §6 NFR row. OP-9 (link portability) is in §5.7 rather than §6 NFR table; vocabulary collision with §6 "Portability" NFR (which is ASPP/skill-installation portability, not link portability) is structural choice, not drift.
- Walked SKILL.md Phase 1 (lines 54-64) against spec §5.1 — Discovery Report contents match; "no test-infrastructure / no touch-surface" exclusion present in spec.
- Walked SKILL.md Phase 2 (lines 66-77) against spec §5.2 — six enumerated Phase 2 items match; D-2 surfaces the preamble vs body inconsistency on "format."
- Walked SKILL.md Phase 3 (lines 79-176) against spec §5.3 + §5.8 — 14-section template matches verbatim; §1-§6 share-with-feature-spec / §7/§8/§11 design-spec-specific commitment matches SKILL.md's section-content notes.
- Walked SKILL.md OUTPUT FORMAT (lines 178-186) against spec §6 NFRs — Pairing, Multi-repo awareness, Voice fidelity (marketing language), Format fidelity rows present; D-3 surfaces the code-block-language gap.
- Walked SKILL.md WHAT NOT TO DO (lines 187-196) against spec §5/§6 — D-4 details partial coverage.
- Walked SKILL.md HANDOFF NOTES (lines 198-203) against spec §3 Dependencies — upstream/downstream/lateral coverage matches.
- Walked SKILL.md "Notes on the standing disciplines" (lines 207-225) against spec §5 — Anti-confabulation in §5.5; Self-contained rule in §6; Naming-bootstrapper-early in §5.1 placeholder language; D-5 surfaces the verification-pass-as-discrete-step protocol gap.
- Walked spec §12 Out of Scope (9 bullets) — five are spec-level deferrals (no SKILL.md mirror expected); four are inheritances from N=1 / strategy doc OQs and correctly cite their sources.
- Cross-checked CP-1 advisories: (a) light/heavy verification labels — accepted, formalization beyond SKILL.md; not drift. (b) interpretive framing in §4/§5.3 — accepted, interpretation; not drift. (c) off-by-one §3 citation `tech-stack.md §21-33` — **closure by verification**: heading `### Atomic-Skill Portability Principle` is at [tech-stack.md:21](../tech-stack.md#L21), content runs through [line 33](../tech-stack.md#L33); the citation is correct. The CP-1 advisory was the reviewer's error, not the spec's. Spec §6 row 1 also uses `#L21-L33` correctly. (d) §3 recursive framing — outside CP-2 surface (§3 not in audit scope); passes through.
- Verified [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) (Atomic-Skill Portability Principle) source matches §3 and §6 NFR Portability characterizations.
- Verified amendment-ID citations: §5.7 cites N=1 amendment 2026-05-17-1 (the `.claude/skills/...` removal); §11 cites the same amendment's N=2 mining note. Both correct.

**Exit criteria status:**
- Divergence list produced (possibly empty): met — five advisory divergences.
- Each divergence has a routing decision: met — D-1 (b), D-2 (b), D-3 (a), D-4 (a), D-5 (c).
- No silent edits to either artifact: met — this entry and the §9 Status line are the only writes; routing decisions await operator-invoked `/spec-amend`.
- Outcome recorded as closing entry of retroactive-spec adoption: met — this entry.

**Pattern for N=3 — CP-2 audit shape.** The N=1 baseline noted CP-2 finds 2–5 advisory divergences of shapes "enumeration miscounts, missing NFR rows, missing INPUTS catalogs, phase-positioning differences." N=2 confirms the range (5 findings) but extends the shape catalog with two new categories:
- **Status-banner lifecycle commitment** (D-1) — a forward-looking lifecycle the spec declares beyond SKILL.md's template initialization. Watch for analogous lifecycle commitments in §5.x of sibling specs (spec-write, spec-execute, spec-review, spec-amend).
- **SKILL.md preamble vs Phase enumeration inconsistency** (D-2) — the preamble may name items the Phase protocol doesn't enumerate (or vice versa). Walk SKILL.md preambles in sibling specs against their phase-body enumerations as a first-class CP-2 step.

**Pattern for N=3 — CP-1 reviewer error pattern.** N=2 CP-1 produced one off-by-one citation advisory (c) that CP-2 verification has now shown was the *reviewer's* error, not the spec's. Future retroactive-spec CP-1 reviewers should verify section-heading line numbers (and section endings) before flagging citations as off-by-one. CP-2 audits should add explicit citation-position verification when inheriting CP-1 advisories on citations.

**Pattern for N=3 — Two-source structure (shape variant).** spec-design has only shape (i) predecessor cross-check (`docs/spec-design-recommendations.md`) — no sibling design spec. §8 "Predecessor cross-check" row present; no "Sibling design-spec cross-check" row, matching expected shape per batch journal. Future specs that *do* have sibling design-spec dependencies (e.g., spec-execute citing session-economy) carry both rows.

**Next action:**
1. Operator invokes `/spec-amend` for D-1 (SKILL.md lifecycle declaration), D-2 (SKILL.md Phase 2 format-bullet elevation), D-3 (spec markdown-hygiene addition), and D-4 (spec §6 inline-citation NFR row). Amendments may be bundled or sequential per operator preference; note D-1 and D-2 both touch SKILL.md (potential bundle), D-3 and D-4 both touch architecture.md (potential bundle).
2. Cross-skill pattern observations queued for the batch journal closing summary at [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) (entry appended this session, summary written after all five complete).
3. After amendments commit, the spec-design retroactive-spec adoption is **closed for this session's CP-2 routing**. The next per-spec CP-2 (N=3) is `spec-write` per the batch audit's authoring-order default.

## 2026-05-18 — Amendment 2026-05-18-1

**Section amended:** [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) — frontmatter `lastUpdated`, Phase 3 → Section content block (new "Status banner" entry prepended before §1 Overview).
**Trigger:** CP-2 drift audit finding D-1: spec §5.8 commits the Status banner lifecycle `Draft — Open for Review` → `Approved` → `Superseded`; SKILL.md template initialized the value but did not declare the lifecycle. Operator selected routing option (b) — amend SKILL.md.
**Reason:** The Status banner lifecycle is a real commitment in architecture.md §5.8 that SKILL.md did not surface. Adding a "Status banner" Section content entry aligns the shipping skill with the spec's contract.
**Impact summary:** No tasks (design spec). CP-2 D-1 closed. No completed work invalidated. No cross-reference follow-up needed (architecture.md §5.8 and §6 NFR Format fidelity row cite SKILL.md by section name, not line number).
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** SKILL.md has no Status banner; frontmatter `lastUpdated` advanced from 2026-05-15 to 2026-05-18.
**Commit:** 5226f55

### Full record

#### Amendment 2026-05-18-1 — SKILL.md declares Status banner lifecycle

**Trigger.** CP-2 drift audit (2026-05-18, this journal) finding D-1: architecture.md §5.8 commits "The Status banner uses the lifecycle `Draft — Open for Review` → `Approved` → `Superseded`." SKILL.md template (line 86) only initializes Status to `Draft — Open for Review`; lifecycle states beyond initial value are not declared. Operator selected routing (b) — amend SKILL.md.

**Section.** Two coordinated edits in the same SKILL.md file:
- Frontmatter `lastUpdated` field — advanced.
- Phase 3 → Section content block — prepended a new "Status banner" entry before the §1 Overview entry.

**Change.**

*Frontmatter*

Before:
> lastUpdated: 2026-05-15

After:
> lastUpdated: 2026-05-18

*Section content — insert new entry before §1 Overview*

Before:
> ### Section content
>
> **§1 Overview.** Two paragraphs. What this is, who it is for, what architectural commitment it makes. No marketing language.

After:
> ### Section content
>
> **Status banner.** Lifecycle: `Draft — Open for Review` → `Approved` → `Superseded`. Initial value `Draft — Open for Review`; advance to `Approved` after the spec's CP-1 (or equivalent approval gate) closes; advance to `Superseded` when a new spec replaces this one.
>
> **§1 Overview.** Two paragraphs. What this is, who it is for, what architectural commitment it makes. No marketing language.

**Reason.** SKILL.md template initialized Status to `Draft — Open for Review` but did not declare the lifecycle states beyond initial value (`Approved`, `Superseded`). architecture.md §5.8 already commits the lifecycle; CP-2 routing (b) lands the declaration in SKILL.md so spec and shipping skill agree. Parallels N=1 amendment 2026-05-18-1 in project-constitution (SKILL.md catches up to spec via Phase-2 bullet elevation); structural pattern is the same — SKILL.md adopts a commitment already present in the spec.

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-2 D-1 closed.
- Completed work invalidated: none.
- Cross-references requiring follow-up: architecture.md §5.8 cites SKILL.md PHASE 3 — SPEC DOCUMENT by section name; remains valid. §6 NFR Format fidelity row cites the same section name; remains valid. No follow-up edits required.

**Status implication.** SKILL.md has no Status banner. Frontmatter `lastUpdated` advanced from 2026-05-15 to 2026-05-18.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-2

**Section amended:** [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) — Phase 2 bullet list (new `Format confirmation` bullet); [specs/20260518-spec-design-skill/architecture.md](./architecture.md) §13 OQ-1 Question text (one-sentence update).
**Trigger:** CP-2 drift audit finding D-2: "format" appears in SKILL.md preamble line 15 but is not enumerated in Phase 2 bullets. §13 OQ-1 claimed the prompt was "undocumented in SKILL.md" — partially imprecise. Operator selected routing option (b) — amend SKILL.md — and Phrasing X (route-elsewhere flavor, least commitment to OQ-1 content options).
**Reason:** Closes the internal SKILL.md inconsistency by enumerating Format confirmation as a Phase 2 bullet. Phrasing X commits to *where* the format question lives (Phase 2) but stays neutral on *what* the prompt asks beyond a route-elsewhere convention, preserving OQ-1's four-option content question.
**Impact summary:** No tasks (design spec). CP-2 D-2 closed; §13 OQ-1 "strict claim" partially closed; OQ-1 four-option content question remains open. No completed work invalidated. OQ-1 Question text updated in same amendment per cross-reference follow-up rule.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** SKILL.md has no Status banner; frontmatter `lastUpdated` already at 2026-05-18 from amendment 2026-05-18-1 (same-day); no additional advance. architecture.md spec status remains `Draft — Open for Review`.
**Commit:** 5ccf2e7

### Full record

#### Amendment 2026-05-18-2 — SKILL.md Phase 2 enumerates Format confirmation + architecture.md §13 OQ-1 Question update

**Trigger.** CP-2 drift audit (2026-05-18, this journal) finding D-2: SKILL.md preamble line 15 ("pause at Phase 2 for user input on naming, audience, **format**, and verification commitment") commits to Phase 2 asking about format, but Phase 2 itself (lines 68-76) does not enumerate it. architecture.md §13 OQ-1 Question text said "This recommendation did **not** land in the shipping SKILL.md" — imprecise (preamble does mention format). Operator selected routing option (b) — amend SKILL.md Phase 2 to enumerate format. Phrasing X chosen from three alternatives (route-elsewhere; minimal; predecessor-faithful) for least commitment to OQ-1's four-option content question.

**Section.** Two coordinated edits across two files:
- SKILL.md Phase 2 bullet list (lines 68-76) — insert new `Format confirmation` bullet between `Audience confirmation` and `Verification commitment`, matching the preamble's slot order at line 15.
- architecture.md §13 OQ-1 Question text — update the "did not land in the shipping SKILL.md" sentence to reflect the structural-level landing, preserving the four-option content question as still open.

**Change.**

*SKILL.md Phase 2 bullet list — insert new bullet between Audience and Verification*

Before:
> - **Audience confirmation.** Confirm `TARGET_AUDIENCE`. Name the broadest member explicitly — that is who the spec is written for.
> - **Verification commitment.** "What external claims will this spec make? Are you willing to take a verification pass against canonical sources before publishing? (Recommended: yes.)" Capture the answer; in Phase 3, this determines how aggressive the inline citation discipline is.

After:
> - **Audience confirmation.** Confirm `TARGET_AUDIENCE`. Name the broadest member explicitly — that is who the spec is written for.
> - **Format confirmation.** Confirm the design-spec format (14-section template) fits the artifact being produced. If the artifact is implementation work (atomic tasks, tests, rollout), route to `/spec-write` instead. If the artifact is hybrid or non-conformant, surface to the operator before proceeding to Phase 3.
> - **Verification commitment.** "What external claims will this spec make? Are you willing to take a verification pass against canonical sources before publishing? (Recommended: yes.)" Capture the answer; in Phase 3, this determines how aggressive the inline citation discipline is.

*architecture.md §13 OQ-1 Question text — update one sentence*

Before:
> "This recommendation did **not** land in the shipping SKILL.md."

After:
> "This recommendation landed at the *structural* level via amendment 2026-05-18-2 (SKILL.md Phase 2 now enumerates a `Format confirmation` bullet that routes to `/spec-write` when the artifact is implementation work). The *content* of the prompt — specifically OQ-1 options (a)/(b)/(c)/(d) below — remains open."

**Reason.** SKILL.md preamble line 15 promised Phase 2 would ask about format; the Phase 2 enumeration did not include it (an internal inconsistency). architecture.md §13 OQ-1 named the gap but slightly overstated it ("undocumented" — when in fact the preamble does mention format). The amendment closes both by enumerating Format confirmation as a Phase 2 bullet (Phrasing X — route-elsewhere flavor, picked over Phrasings Y (predecessor-faithful, which would commit to non-default shapes within spec-design) and Z (minimal)). Phrasing X commits to *where* the format question lives (Phase 2) but stays neutral on *what* the prompt should ask beyond the route-elsewhere convention, preserving OQ-1's four-option resolution as genuinely open. OQ-1 Question text is updated in the same amendment per the spec-amend cross-reference follow-up rule.

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-2 D-2 closed; §13 OQ-1 "strict claim" (undocumented in SKILL.md) closed; OQ-1 four-option content question remains open and routes through its existing watch-items machinery.
- Completed work invalidated: none.
- Cross-references requiring follow-up: OQ-1 Question text updated in this amendment. §13 OQ-1 Leaning section ("option (c) is the de facto status quo") becomes mildly stale post-amendment but is bounded by the phrase "at spec time" and accepted as historical record. OQ-1 Owner, Watch items, and Anti-goals remain valid.

**Status implication.** SKILL.md has no Status banner; frontmatter `lastUpdated` already at 2026-05-18 from amendment 2026-05-18-1 (same-day); no additional advance. architecture.md spec status remains `Draft — Open for Review` — surgical addition, no commitment in §3, §4, §5, §10, §11, §12 changed.

**Approver.** Eric Wasgatt — approved 2026-05-18 with Phrasing X selected.

## 2026-05-18 — Amendment 2026-05-18-3

**Section amended:** [architecture.md §6 Non-functional Requirements table](./architecture.md#L256-L268) — new "Markdown hygiene" row inserted between "Format fidelity" and "Pairing".
**Trigger:** CP-2 drift audit finding D-3: SKILL.md OUTPUT FORMAT line 183 commits "All code blocks specify a language." Spec §6 NFR table and §5.8 Section template did not surface this. Operator selected routing option (a) — amend spec.
**Reason:** SKILL.md OUTPUT FORMAT carries the rule as a behavioral commitment; spec §6 did not reflect it. New row preserves Format fidelity's scope (template structure) while giving output-rendering hygiene a natural home.
**Impact summary:** No tasks (design spec). CP-2 D-3 closed. No completed work invalidated. No cross-reference follow-up needed.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`Draft — Open for Review`)
**Commit:** f2b7b22

### Full record

#### Amendment 2026-05-18-3 — architecture.md §6 adds Markdown hygiene NFR

**Trigger.** CP-2 drift audit (2026-05-18, this journal) finding D-3: SKILL.md OUTPUT FORMAT line 183 commits "All code blocks specify a language." Spec §6 NFR table and §5.8 Section template did not surface this commitment. Operator selected routing (a) — amend spec.

**Section.** architecture.md §6 NFR table — insert one new row after the existing "Format fidelity" row.

**Change.**

Before (table includes Format fidelity, followed by Pairing):
> | **Format fidelity** | Output conforms to the 14-section template with exact headings and declared order. §7/§8/§11 use design-spec form, not feature-spec form. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-design/SKILL.md) |
> | **Pairing** | Every design spec is accompanied by a journal at the same directory. The pair is the output; architecture.md alone is incomplete. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-design/SKILL.md) |

After (new Markdown hygiene row inserted between Format fidelity and Pairing):
> | **Format fidelity** | Output conforms to the 14-section template with exact headings and declared order. §7/§8/§11 use design-spec form, not feature-spec form. | [SKILL.md PHASE 3 — SPEC DOCUMENT](../../.agents/skills/spec-design/SKILL.md) |
> | **Markdown hygiene** | All code blocks specify a language. Tables and lists conform to GitHub-flavored markdown rendering conventions. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-design/SKILL.md) |
> | **Pairing** | Every design spec is accompanied by a journal at the same directory. The pair is the output; architecture.md alone is incomplete. | [SKILL.md OUTPUT FORMAT](../../.agents/skills/spec-design/SKILL.md) |

**Reason.** SKILL.md OUTPUT FORMAT carries "All code blocks specify a language" as a behavioral commitment; spec §6 did not reflect it. New "Markdown hygiene" NFR row preserves Format fidelity's clean scope (template structure) while giving output-rendering hygiene a natural home. Parallels project-constitution N=1 amendment 2026-05-18-3 (README-reconciliation NFR row — same shape, different finding).

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-2 D-3 closed.
- Completed work invalidated: none.
- Cross-references requiring follow-up: none.

**Status implication.** Kept (`Draft — Open for Review`). Surgical addition to NFR table.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-4

**Section amended:** [architecture.md §6 Non-functional Requirements table](./architecture.md#L256-L270) — new "Inline citation preference" row inserted between "Citation discipline" and "Self-containment".
**Trigger:** CP-2 drift audit finding D-4: SKILL.md WHAT NOT TO DO item 3 ("Do not bury authoritative URLs in §14 when they belong inline at the point of claim") and the reinforcing §14 References preamble had no explicit §5/§6 home in the spec. Operator selected routing option (a) — amend spec §6.
**Reason:** SKILL.md commits the skill to inline-citation-over-bibliography behavior at two locations (WND-3 + §14 preamble); spec §6 did not reflect this. New row pulls the rule into a first-class NFR. WND-4 (Risks/OQ distinction) deferred — already encoded in spec-template §10 vs §13 sectional separation.
**Impact summary:** No tasks (design spec). CP-2 D-4 closed. No completed work invalidated. No cross-reference follow-up needed.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`Draft — Open for Review`)
**Commit:** 78e3012

### Full record

#### Amendment 2026-05-18-4 — architecture.md §6 adds Inline citation preference NFR

**Trigger.** CP-2 drift audit (2026-05-18, this journal) finding D-4: SKILL.md WHAT NOT TO DO item 3 ("Do not bury authoritative URLs in §14 when they belong inline at the point of claim") and the reinforcing §14 References preamble ("Inline citations at the point of claim are preferred over a bibliography-at-the-bottom. §14 is for sources too cross-cutting to inline.") commit the skill to inline-citation-over-bibliography behavior. Spec §6 NFRs and §5 Detailed Design did not surface this commitment. Operator selected routing option (a) — amend spec §6.

**Section.** architecture.md §6 NFR table — insert one new row after the existing "Citation discipline" row (logical grouping: both rows govern citation behavior).

**Change.**

Before (table includes Citation discipline, followed by Self-containment):
> | **Citation discipline** | External claims verified at canonical sources (heavy verification) or limited to repo-internal sources (light verification). Soft hedges (`[needs verification]`, `[unclear]`, `[TBD]`) prohibited in published spec content. | [SKILL.md Notes on the standing disciplines](../../.agents/skills/spec-design/SKILL.md) |
> | **Self-containment** | Each produced design spec opens with Status / Date / Author / Audience banner and reads independently of the originating chat. No "as we discussed"; every named system, role, or pattern defined or linked. | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/spec-design/SKILL.md) |

After (new Inline citation preference row inserted between Citation discipline and Self-containment):
> | **Citation discipline** | External claims verified at canonical sources (heavy verification) or limited to repo-internal sources (light verification). Soft hedges (`[needs verification]`, `[unclear]`, `[TBD]`) prohibited in published spec content. | [SKILL.md Notes on the standing disciplines](../../.agents/skills/spec-design/SKILL.md) |
> | **Inline citation preference** | Authoritative URLs and source references appear inline at the point of claim. §14 References is reserved for cross-cutting sources too broad to inline; it is not a bibliography catch-all. | [SKILL.md WHAT NOT TO DO + §14 References preamble](../../.agents/skills/spec-design/SKILL.md) |
> | **Self-containment** | Each produced design spec opens with Status / Date / Author / Audience banner and reads independently of the originating chat. No "as we discussed"; every named system, role, or pattern defined or linked. | [SKILL.md OPERATING PRINCIPLES](../../.agents/skills/spec-design/SKILL.md) |

**Reason.** SKILL.md commits the skill to inline-citation-over-bibliography behavior at two locations (WHAT NOT TO DO item 3 + §14 References preamble); spec §6 did not reflect this commitment. The new "Inline citation preference" row covers WND-3 directly and pulls the §14 preamble's rule into a first-class NFR. WND-4 (Risks/OQ distinction) was not included in this amendment because the §10 Risks vs §13 Open Questions sectional separation in the spec template already encodes the distinction at structural level — an NFR row for it would be redundant prose.

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-2 D-4 closed.
- Completed work invalidated: none.
- Cross-references requiring follow-up: none.

**Status implication.** Kept (`Draft — Open for Review`). Surgical addition to NFR table.

**Approver.** Eric Wasgatt — approved 2026-05-18.

### CP-2 closeout

With Amendments 2026-05-18-1, 2026-05-18-2, 2026-05-18-3, and 2026-05-18-4 all approved and applied, CP-2 routing is complete:

- D-1 → resolved via 2026-05-18-1 (route b, SKILL.md lifecycle declaration).
- D-2 → resolved via 2026-05-18-2 (route b, SKILL.md Phase 2 Format confirmation bullet; OQ-1 Question text updated in same amendment).
- D-3 → resolved via 2026-05-18-3 (route a, architecture.md §6 Markdown hygiene NFR).
- D-4 → resolved via 2026-05-18-4 (route a, architecture.md §6 Inline citation preference NFR; WND-4 Risks/OQ distinction deferred as redundant with spec-template sectioning).
- D-5 → accepted as known minor discrepancy (rationale in CP-2 verdict above: protocol detail vs principle distinction).

The spec-design retroactive-spec adoption is **closed**. The spec is now the living contract for SKILL.md per the Amendment Protocol. The five-spec batch CP-2 at [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) advances to N=3 — `spec-write` retroactive spec — per the batch's authoring-order default.

**Pattern for N=3.** Four amendments emerged from one CP-2 verdict — one more than N=1's three. The amendments split cleanly by file (D-1, D-2 against SKILL.md; D-3, D-4 against architecture.md). D-2 was the only amendment requiring a cross-reference follow-up (OQ-1 Question text update), applied in the same amendment per spec-amend's coherent-unit rule. The four amendments interacted in two ways:
1. **Sequencing — D-1 then D-2 against SKILL.md.** Both touched SKILL.md; D-1 advanced frontmatter `lastUpdated`, D-2 same-day so no further advance. Same-day amendment sequencing avoids frontmatter churn.
2. **Content-decision interaction — D-2 phrasing partially binds OQ-1.** Phrasing X (route-elsewhere) was chosen to close the structural gap without pre-empting OQ-1's four-option content question. The "Phrasing X/Y/Z + OQ-1 update Yes/No" decision matrix is a pattern for future amendments where a SKILL.md edit interacts with an open OQ; future CP-2 amendments touching OQs should surface the matrix explicitly rather than silently pick a phrasing.

Future CP-2 batches in the quintet should expect 2–5 amendments per spec (N=1 had 3, N=2 had 4); the file-split pattern (SKILL.md amendments vs spec amendments) and the same-day sequencing rule will likely recur.

## 2026-05-19 — Amendment 2026-05-19-1 (cross-skill — post-CP-2 banner advancement)

**Section amended:** [architecture.md:3](./architecture.md#L3) §1 Status banner
**Trigger:** First execution of the post-CP-2 banner transition; methodology-level decision defining `Approved — CP-2 closed YYYY-MM-DD` as the post-`Draft — Open for Review` successor state, applied retroactively across N=1..N=6.
**Reason:** Banner advances from `Draft — Open for Review` to `Approved — CP-2 closed 2026-05-18` per the methodology-level decision recorded in the cross-skill anchor. spec-design's §5.8 lifecycle declaration (`Draft — Open for Review → Approved → Superseded`) is the methodology vocabulary source for the chosen successor state name.
**Impact summary:** No tasks; CP-2 already closed (commit `390f36d` 2026-05-18 12:20:28); no completed work invalidated. §5.8 lifecycle term `Approved` is now load-bearing methodology-wide (six-spec scope) rather than spec-design-internal — observation, not a follow-up edit.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-19
**Status implication:** **forward advancement** — first instance in the methodology. Draft → Approved.
**Commit:** `88eda73` (six architecture.md banner edits); `cf50e2e` (cross-skill anchor + 6 paired companion journal entries).

### Full record

See [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) amendment 2026-05-19-1 for the full structured Phase 2 amendment record. This is the **cross-skill companion entry**; the batch journal holds the primary record because the amendment is methodology-level (defines the post-CP-2 successor state across N=1..N=6). Pasting the structured block here would duplicate the durable record.
