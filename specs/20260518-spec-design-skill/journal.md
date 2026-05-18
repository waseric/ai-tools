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
| [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) + [feature.md](../20260515-spec-path-convention/feature.md) | Negative signal | Modifies spec-design's OUTPUT FORMAT (commit `6d158fb`) but does not architecturally describe the skill. |
| External private repo (`private-design-repo`) | Negative signal — out of repo, out of scope | `ai-frontmatter-distributor-architecture.md` lives there. Recommendations doc transmits what is portable. |

### New "Pattern for N=3" callout — distinguish design-rationale source from current-behavior source

A skill may have a **predecessor artifact** (recommendations doc, design brief, scratch notes) that captures the design rationale that became the skill, *separate from* the SKILL.md that captures current behavior. The retroactive spec must distinguish:

- **Authoritative for current behavior** — SKILL.md only. CP-1 reviews against this.
- **Authoritative for design rationale** — predecessor artifact, if it exists. Cited as Inspirational in §14. CP-2 reads divergences between predecessor and SKILL.md as *evolution*, not as drift.

The N=1 retroactive spec did not have a predecessor artifact (project-constitution emerged directly from the trilogy commit `49c15f0` without a written design rationale doc) and so the distinction did not surface. This N=2 spec did have one ([docs/spec-design-recommendations.md](../../docs/spec-design-recommendations.md)) and the distinction was load-bearing — without it, CP-2 would flag every recommendation that didn't land in SKILL.md as drift, when most of those gaps are *deliberate* evolution.

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
**Diff range:** commit `989fdf3` (specs/20260518-spec-design-skill/{architecture.md, journal.md})
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
