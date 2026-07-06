# `spec-execute` Skill — Journal

This journal continues the **N≥2 mining structure** established at N=1 ([specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md)), refined at N=2 ([specs/20260518-spec-design-skill/journal.md](../20260518-spec-design-skill/journal.md)), and stabilized at N=3 ([specs/20260518-spec-write-skill/journal.md](../20260518-spec-write-skill/journal.md)). Section headings are stable across retroactive-spec journals; future sessions (sessions 4–5 of the legacy quintet — `spec-review`, `spec-amend` retroactive specs) find the same slots.

This is the **N=4 instance** in the retroactive-spec sequence and **session 3** of the legacy-quintet sequence per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). The strategy doc identifies this slot as the **N=3 robustness check** — the most divergent skill shape in the family.

## 2026-05-18 — Retroactive design spec authored

**Status:** draft — awaiting CP-1 review (deferred to fresh session per N=1 / N=2 / N=3 precedent)
**Artifact:** [architecture.md](./architecture.md)
**Companion:** [journal.md](./journal.md) (this file)
**Trigger:** Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 3 ordering. Strategy doc pre-resolved audience, verification commitment, batched CP-2, and the N=2 inflection-point deferral; this session executed against that strategy plus the N=3-journal corrective on predecessor-richness prediction.

### N=3 "Pattern for N=4" callouts — validation outcomes

This is the load-bearing addition to the N=4 journal: each callout from the [N=3 journal](../20260518-spec-write-skill/journal.md) is recorded as validated, refined, or rejected with reasoning. Future sessions (N=5) read this table first.

| N=3 callout | Outcome at N=4 | Notes |
|---|---|---|
| **"None surfaced" §13 reporting with triage table** (carry-forward candidate) | **Not exercised — but pattern preserved** | This session surfaced two real §13 OQs (Phase 7 ↔ Phase 8 ordering; multi-repo lifecycle mismatch). Two of four Phase-2 OQ candidates were placed in §13; one in §12; one dropped. The N=3 "none surfaced" report shape was not needed here, so the pattern remains untested at N=4. N=5 may exercise it again if all candidates triage to §12 / drop. Carry-forward to N=5. |
| **Spec-design's CP-1 already-passed adjusts CP-2 trigger narrative** (carry-forward, validated) | **Validated and extended** | This spec's §9 CP-2 trigger now names BOTH N=2's already-passed CP-1 AND N=3's already-passed CP-1, narrowing the remaining trigger condition to "two sibling quintet CP-1s + project-constitution CP-2." The narrowing pattern compounds at each session. |
| **Off-by-one section-heading citation discipline elevated to CP-1 review focus** (carry-forward, validated) | **Validated, applied** | All §3 citations to `tech-stack.md` use the heading line: `§21-33` (ASPP), `§44` (context-window limits), `§48` (repo layout), `§51` (spec-driven-development). Section-heading discipline explicitly carried forward as a §9 CP-1 review-focus item. |
| **Predecessor-richness varies; spec accommodates by adjusting §8 Validation Approach** (carry-forward candidate) | **Refined — N=3 prediction openly disconfirmed** | N=3 journal predicted "the remaining three skills (`spec-execute`, `spec-review`, `spec-amend`) likely have *thin or no* predecessor — they were already-shipping siblings extended by the trilogy commit." **This is incorrect for `spec-execute`.** The skill has a rich predecessor in the same shared doc: [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438 — `spec-execution-prompt.md` artifact + companion design notes. The §8 Validation Approach was *not* simplified by dropping the predecessor row; instead, it was *expanded* by adding a new row: **sibling design-spec cross-check** (the session-economy spec is authoritative for current behavior added after the predecessor). The corrected prediction for `spec-review` (session 4): also has a rich predecessor in the same shared doc, lines 446+. For `spec-amend` (session 5): no predecessor in that doc (spec-amend was added at trilogy commit `80000b1` without a predecessor artifact). |

### Two-source structure — new at N=4

This session surfaces a structural novelty that did not exist at N=1/N=2/N=3: **two authoritative upstream sources for the skill's commitments**:

1. **Predecessor doc** ([docs/spec-driven-development-prompts-conversation.md lines 235–438](../../docs/spec-driven-development-prompts-conversation.md)) — authoritative for **design rationale**, not current behavior. Same role N=2 and N=3 assigned to their predecessors.
2. **Sibling design spec** ([specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md)) — authoritative for **current behavior added after the predecessor** (Phase 8, token economy, Phase 1 multi-repo detection, Phase 4/6 paired commits). This is **the first time a quintet retroactive spec has a sibling design spec that authoritatively commits to behavior in the current SKILL.md.** It is cited Authoritative in §14, alongside the SKILL.md itself.

**Why this matters for CP-2.** Without the distinction, CP-2 would read every Phase-8 behavior, every multi-repo detection commitment, every paired-commit rule as "behavior in SKILL.md not present in the predecessor doc — drift." The two-source structure prevents this: the session-economy spec is cited *as the architectural source* for those behaviors, so CP-2 reads them as authoritative-and-already-spec'd, not as drift.

**Pattern for N=5.** Each remaining quintet retroactive spec scans for *both*:
- predecessor artifact in `docs/` (or absent), AND
- sibling design spec(s) that authoritatively committed to behavior in the current SKILL.md (or absent).

For `spec-review` (session 4): the session-economy spec adds Phase 8 (Update Artifacts) multi-repo handling — same sibling-design-spec source. For `spec-amend` (session 5): the session-economy spec adds `SPEC_REPO_ROOT` to INPUTS and a Phase 8 multi-repo note — same sibling-design-spec source. The two-source structure recurs across the remaining quintet members.

### Source-file selection (decision + rationale)

The explicit table appeared in the session's Phase 1 Discovery Report. Repeated here for journal completeness:

| File | Used? | Rationale |
|---|---|---|
| [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) | Yes — authoritative for current behavior | The skill itself (227 lines, 8 phases). |
| [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438 | Yes — authoritative for design rationale (NOT for current behavior) | Predecessor of this skill: `spec-execution-prompt.md` artifact (lines 235–404) + "Design notes on the execution prompt" (lines 406–438). Cited Inspirational in §14. **N=3 prediction "thin or no predecessor" disconfirmed.** |
| [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) | Yes — authoritative for current behavior added after the predecessor | **First-of-its-kind source** for a quintet retroactive spec. Authoritative for Phase 8 (token-economy factor), Phase 1 multi-repo detection, Phase 4/6 paired commits. Cited Authoritative in §14. |
| [specs/tech-stack.md](../tech-stack.md), [specs/mission.md](../mission.md), [specs/roadmap.md](../roadmap.md) | Yes — authoritative for constraints, audience, lifecycle position | Constitutional bindings. |
| [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) + [journal.md](../20260518-spec-write-skill/journal.md) | Yes — N=3 retroactive-spec source | Closest-sibling structural source. "Pattern for N=4" callouts validated/refined/rejected above. |
| [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) + [journal.md](../20260518-spec-design-skill/journal.md) | Yes — N=2 retroactive-spec source | Original "Pattern for N=3" source; predecessor-distinction discipline originates here. |
| [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) | Yes — N=1 retroactive-spec source | Original structural source. |
| [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) | Read — orientation only | Cited in §3, §9, §11, §12. Not used as a source for §4/§5 architectural commitments. |
| [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) | Negative signal | Modifies spec-execute's `SPEC_PATH` example (commit `4ebec0c`) but does not architecturally describe the skill. |
| [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md), [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md), [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) | Negative signal — pipeline neighbors, not architectural sources | Referenced for handoffs (Phase 7 → spec-review, Amendment Protocol → spec-amend) but their internal architecture is out of scope for this skill's spec. |

### New "Pattern for N=5" callouts

Candidates for future-session validation. Recorded here, not declared as binding.

1. **Two-source structure for trilogy-extended skills.** When a quintet skill has both a predecessor artifact (design rationale) AND a sibling design spec authoritatively committing to current behavior, the spec must distinguish both. The session-economy spec is the recurring sibling source for `spec-execute`, `spec-review`, and `spec-amend` (all three were modified by the session-economy commit `5ce4024`). **First exercised at N=4.** Carry to N=5 for validation; expected to recur for both remaining sessions.

2. **CP-2 trigger narrative compounds, not just narrows.** N=3 observed the trigger condition narrows as CP-1s pass. N=4 extends this: the trigger condition now names BOTH already-passed CP-1s (N=2 and N=3). N=5 will name three or four already-passed CP-1s as a list. **Pattern for N=5:** keep enumerating already-passed CP-1s rather than collapsing them into "the prior quintet CP-1s" — explicit attribution survives time and is easier to audit.

3. **Most-divergent-shape skill validates the family pattern by exception.** The strategy doc named this session as the robustness check on the spec-design / spec-write pattern. The actual result: the eight-phase iterative shape DOES fit the 14-section design-spec template, with §5 expanded to eight subsections (one per phase) plus an Amendment Protocol subsection. The template generalizes; the test passes. **Pattern for N=5:** sessions 4 and 5 inherit the validated template; novel divergences (if any) surface as new §5 subsections, not as template overhauls.

4. **§13 OQ count varies by skill divergence.** N=1 surfaced one OQ. N=2 surfaced one OQ. N=3 surfaced zero ("none surfaced" reporting). N=4 surfaced two. The OQ count is a function of how many unspecified phase interactions and edge cases the skill has, not a function of journal age or session number. **Pattern for N=5:** OQ count is not a quality signal; faithful surfacing of real gaps is.

5. **Phase 8's source-attribution model is exportable.** Phase 8 is the first SKILL.md content whose architectural commitment lives in a *sibling design spec* (session-economy §5.1), not a predecessor doc or organic SKILL.md authorship. The §5 subsection for it cites the session-economy spec as its pattern-source. **Pattern for N=5:** when a phase or commitment is sourced from a sibling design spec, name that spec as the pattern-source in §5 (not just in §3 / §14). Sessions 4 and 5 will need this for their session-economy-sourced behaviors.

### Format choice — design spec vs feature spec

Validated. The shipping skill is `/spec-design`; the operator invoked it for a retroactive design spec on `spec-execute`. No friction.

**Pattern (carried from N=1, validated at N=2/N=3/N=4).** Retroactive specs for already-shipping skills use `/spec-design`.

### Naming pattern — directory slug

`specs/20260518-spec-execute-skill/architecture.md`. Today is 2026-05-18.

**Pattern (carried from N=2/N=3, validated).** Authoring date, not strategy-doc-anticipated date. **Three consecutive same-date sessions** (N=2, N=3, N=4 all dated 2026-05-18) produce three sibling directories with the same date prefix; differentiation by skill-name slug is sufficient.

### Audience framing

Reused verbatim from N=1/N=2/N=3: "Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set."

**Pattern (carried from N=1/N=2/N=3, validated).** Audience is reusable verbatim across the legacy quintet.

### Verification commitment level

**Light verification**, per N=1/N=2/N=3 precedent and per Phase 2 unilateral confirmation. The spec text contains no external claims requiring WebFetch — all citations are repo-internal.

**Pattern (carried from N=1/N=2/N=3, validated).** Light verification is the correct default for the legacy quintet.

### Open-question framing — handling known gaps

§13 reports **two first-class OQs** (Phase 7 ↔ Phase 8 ordering; multi-repo lifecycle mismatch), one moved to §12 (cross-skill amendment coordination — inherited from strategy-doc OQ-3 and N=3 §12), and one dropped (Phase 8 unilateral-continuation override — already covered by SKILL.md).

**Decision process:** the operator was asked explicitly (Phase 2 `AskUserQuestion` covering four candidates) where each belonged. Operator chose §13, §13, drop, §12 in order. The Recommended option was selected in all four cases.

**Pattern (carried from N=2/N=3, validated).** Triage candidates explicitly to the operator rather than picking placement unilaterally. The Recommended option was the choice in all four candidates — recommendations were tightly scoped and held.

### Drift-audit-as-checkpoint (CP-2)

Both CP-1 (faithfulness) and CP-2 (drift audit) declared in §9.

**Refinement at N=4.** CP-2 trigger narrative compounds: names both N=2's already-passed CP-1 AND N=3's already-passed CP-1, narrowing the remaining trigger to "two sibling quintet CP-1s + project-constitution CP-2." Also adds the **session-economy-spec cross-consistency check** as a CP-2 review-focus line item — a new check unique to N=4.

**Pattern for N=5.** Each remaining quintet retroactive spec declares the compounded batched CP-2 trigger. Session 5 will name three already-passed CP-1s.

### Scope discipline — what was kept out

§2 Non-goals lists six items explicitly. §12 Out of Scope lists eleven items, most inherited from N=3 / strategy-doc / N=2 / N=1, and one new for N=4:
- (new) Resolving §13 OQ-2 (multi-repo lifecycle mismatch) — explicitly named as requiring real multi-repo execution data.

The format-question gap from N=2 §13 OQ-1 is named in §12 (inherited from N=3's disposition). The `docs/retroactive-spec-pattern.md` decision is named in §12 (deferred at N=3 close, revisited at N=5).

**Pattern (carried from N=1/N=2/N=3, validated).** Retroactive specs are descriptive, not prescriptive. The §12 list grew from N=1's four → N=2's thirteen → N=3's fifteen → N=4's eleven (smaller because two N=3 items were collapsed into a single multi-repo-related §12 item).

### Cross-session knowledge transfer

This journal is the canonical N=4 mining input for sessions 4 and 5. Specifically:

**What this journal commits to:**
- The "Pattern for N=4 — validation outcomes" table above is the structural pattern for N≥3 journals; N=4 extends it with the **two-source-structure** observation.
- The N=3 prediction "trilogy-extended skills likely have thin or no predecessor" is openly disconfirmed for `spec-execute`. Corrective predictions for `spec-review` (has a predecessor in the shared doc) and `spec-amend` (no predecessor) are recorded above.
- The five new "Pattern for N=5" callouts are candidates for future-session validation.

**What this journal does NOT commit to:**
- A `docs/retroactive-spec-pattern.md`. Decision deferred to N=5 close per N=3.
- A binding template for sessions 4–5. The journal-mining protocol is the pattern; the two-source-structure observation is the structural addition; neither is a fillable template.
- Resolution of the format-question-prompt gap, the cross-skill amendment coordination gap, or the constitution-amendment ceremony — all out-of-scope items inherited unchanged.

### Friction observed

Honest record of where this session encountered friction. Useful for sessions 4–5 to anticipate.

- **N=3 prediction disconfirmation required a deliberate corrective.** The N=3 journal's "Pattern for N=4 #4" stated trilogy-extended skills "likely have *thin or no* predecessor." Discovery in this session showed `spec-execute` has a *rich* predecessor (lines 235–438 of the shared doc, ~204 lines of prompt + design notes). Disconfirming a prior journal's prediction risks self-doubt ("did I miss something?") but the correction is honest. The §3 Background and the corrected predictions for sessions 4–5 (`spec-review` predecessor present; `spec-amend` predecessor absent) are recorded above so sessions 4–5 can sanity-check at Phase 1 Discovery.

- **Two-source attribution required new §3 Background framing.** N=2 and N=3 each had one upstream source (the predecessor doc). N=4 has two: predecessor (rationale) AND sibling design spec (current behavior). The temptation was to fold the session-economy spec into "another predecessor" in §14 Inspirational; that would be wrong — the session-economy spec authoritatively commits to *current* behavior, not rationale that evolved into current behavior. Held the line by citing it Authoritative in §14 and naming the two-source structure explicitly in §3 Background. Pattern-for-N=5 #1 records this for sessions 4 and 5.

- **Eight-phase shape did not break the 14-section template.** The strategy-doc framed this as the robustness check; the worry was that an iterative, multi-task, multi-pause skill might not fit the template designed for single-shot authoring skills. Result: §5 expanded to eight phase-subsections + an Amendment Protocol subsection + voice/portability subsections, but the 14-section count and the §1–§14 structure held. **No template overhaul needed.** Pattern-for-N=5 #3 records this.

- **Two §13 OQs is more than the prior siblings.** N=1 had one OQ (constitution-amendment ceremony); N=2 had one (format-question gap); N=3 had zero. N=4 has two. The OQ-count tension named in the friction section is honest reporting: spec-execute has more unspecified phase-interaction edges (Phase 7 ↔ Phase 8; paired-commit recovery) than the simpler authoring skills, so its OQ count is legitimately higher. The session resisted the urge to drop one to match prior-session norms.

### Conversation grounding

- Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 3 ordering. No prior in-thread conversation; the strategy doc + N=1/N=2/N=3 specs and journals function as the "extended conversation."
- Phase 1 (Discovery) produced source-file table including three negative-signal rows; landscape orientation against the lifecycle skill family; constraint orientation against four constitutional citations + one sibling-design-spec citation (session-economy); conversation grounding (strategy doc + N=1/N=2/N=3 specs and journals as inputs); naming candidates not needed (name fixed by skill name); **predecessor confirmed and scoped** to [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 235–438; **sibling design spec confirmed** at [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md).
- Phase 2 (Clarify) surfaced four operator decisions via `AskUserQuestion`: four OQ candidate placements (2 × §13, 1 × drop, 1 × §12). The Recommended option was chosen in all four. The unilateral decisions (defer CP-1 to fresh session; narrowed CP-2 trigger; session-economy spec as Authoritative companion source; sibling-design-spec cross-check row in §8) were not objected to.
- No `[blocker]` open questions arose. Session proceeded to Phase 3 (spec document + journal authoring).

### Tasks defined

None. Design spec, not feature spec. The "next work" is review (CP-1) and audit (CP-2), declared in §9.

### Next action pointer

Three steps, in order:

1. **Commit** the spec + journal as a paired commit. This is the closing action of session 3.
2. **CP-1 review** in a fresh session: operator invokes `/spec-review` against [architecture.md §9 CP-1](./architecture.md#cp-1--retroactive-spec-faithfully-describes-the-shipping-skill).
3. **Session 4 — `spec-review` retroactive spec.** N=5 in retroactive-spec sequence. The five "Pattern for N=5" callouts above are inputs. Pre-confirmed predecessor: [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446+ (`spec-review-prompt.md` artifact). Pre-confirmed sibling design spec: [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) (Phase 8 Update Artifacts multi-repo handling). Two-source structure expected to recur.

No `[blocker]` open questions; the spec is ready for CP-1.

## 2026-05-18 — Review of CP-1

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** changes requested
**Verdict commit:** `57be7fa`
**Diff range:** `1c95460` (paired commit introducing [architecture.md](./architecture.md) and this journal)
**Tasks reviewed:** none (retroactive design spec — no atomic tasks)
**Blockers:** 4 — one underlying citation pattern (session-economy §5.3 / §5.5 cited as architectural source for spec-execute Phase 4/6 paired-commit discipline) surfaced at four sites: [§5.4](./architecture.md), [§5.6](./architecture.md), [§6 Multi-repo discipline NFR row](./architecture.md), and the [§9 CP-1 / CP-2 review-focus assertions](./architecture.md). Verified against [session-economy/architecture.md §5](../20260514-session-economy/architecture.md): §5.3 covers `spec-amend`, §5.5 covers `spec-write`/`spec-design` — neither is about spec-execute Phase 4/6. Session-economy spec has no §5 subsection that architecturally commits to spec-execute Phase 4/6 paired-commit prose; its §3 Background acknowledges the prose pre-existed, and commit `5ce4024` added new multi-repo paragraphs to Phases 4 and 6 without a corresponding session-economy §5 specification. The CP-1 review-focus assertion ("retro §5.1/§5.4/§5.6/§5.8 ↔ session-economy §5.1/§5.2/§5.3/§5.5") fails on two of four mappings — the spec's CP-1 contract literally fails its own check.
**Important:** 0
**Advisory:** 3 — (a) [§5.6](./architecture.md) opens "Four updates fire" then lists five numbered items; should be "Five." (b) [§5.10 Voice discipline](./architecture.md) and [§5.11 Portability rule for links](./architecture.md) describe disciplines applied to this spec's prose rather than commitments of SKILL.md — useful formalization, mirrors [N=2 advisory (a)](../20260518-spec-design-skill/journal.md) and [N=3 advisory (a)](../20260518-spec-write-skill/journal.md). (c) [§3 Background](./architecture.md) and [§8 Validation Approach](./architecture.md) lean on dense cross-references to N=1/N=2/N=3 journals + predecessor line numbers — coherent for named audience, dense for outside readers. Carry-forward of [N=2 advisory (d)](../20260518-spec-design-skill/journal.md) / [N=3 advisory (b)](../20260518-spec-write-skill/journal.md).
**Spec amendments proposed:** one batched amendment to fix the citation pattern at five sites. Replace session-economy §5.3 / §5.5 with one of: (a) session-economy §1 Overview + §3 Background (acknowledges Phase 4/6 paired-commit prose pre-existed and was strengthened); (b) commit `5ce4024` as the implementation source; or (c) the shipping SKILL.md itself as the architectural commitment. Route through /spec-amend.

### Review focus walk — itemized outcomes

1. Every commitment in §4/§5/§6 corresponds to behavior in SKILL.md — **pass with comments** (two advisories: "Four updates fire" mis-count; Voice/Portability subsections describe spec-prose discipline, not SKILL.md behavior).
2. No commitment contradicts the shipping SKILL.md — **pass**. Behavioral descriptions are accurate; citation correctness is a separate item (#5 below).
3. ASPP correctly characterized as binding (§3, §6) including absent-input degradation — **pass**. [tech-stack.md §21-33](../tech-stack.md#L21-L33) citation on heading line. N=2's off-by-one fully resolved at N=4.
4. Predecessor doc distinguished as authoritative-for-rationale-not-current-behavior — **pass**. Line citations (lines 410, 412, 416, 418, 422) verified against the design-notes block at [docs/spec-driven-development-prompts-conversation.md lines 406–438](../../docs/spec-driven-development-prompts-conversation.md). Three evolution-explaining commits (`80000b1`, `5ce4024`, `4ebec0c`) verified to have touched [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md).
5. Session-economy spec distinguished as sibling-authoritative and §5 cross-mapping holds — **fail**. §14 categorization correct (Authoritative). [§5.1 Phase 1 multi-repo](./architecture.md) → [session-economy §5.2](../20260514-session-economy/architecture.md) ✓; [§5.8 Phase 8 token economy](./architecture.md) → [session-economy §5.1](../20260514-session-economy/architecture.md) ✓; [§5.4 Phase 4 paired commits](./architecture.md) → session-economy §5.3 (which is `spec-amend`) ✗; [§5.6 Phase 6 paired commits](./architecture.md) → session-economy §5.5 (which is `spec-write`/`spec-design`) ✗. Four blocker sites enumerated above.
6. Eight phases (§5.1–§5.8) and Amendment Protocol (§5.9) match SKILL.md structure — **pass** (folded into #1).
7. §13 OQ-1 (Phase 7 ↔ Phase 8 ordering) and OQ-2 (multi-repo lifecycle mismatch) named with full structure — **pass**. Both have Question / Analysis / Leaning / Owner / Watch items / Anti-goals. Both identify real ambiguities in SKILL.md (Phase 7 silent on post-checkpoint Phase 8 firing; Phase 6 silent on half-committed paired-commit recovery).
8. Self-contained per SKILL.md OPs — **pass with comments** (advisory on cross-reference density).
9. Section-heading citation discipline — **pass**. `tech-stack.md §21-33`, `§44`, `§48`, `§51` all verified at correct heading lines.
10. Portability rule for links honored — **pass**. No `~/.claude/skills/...` references, no absolute filesystem paths.

### Exit criteria status

- Reviewer verdict in structured format: **met** (this entry).
- All blocker findings resolved or escalated to /spec-amend: **pending** — escalation invoked next.
- Verdict written back to spec §9 status line and journal: **met** ([§9 CP-1 Status](./architecture.md#cp-1--retroactive-spec-faithfully-describes-the-shipping-skill) updated in same change; this entry).

### Pattern observation at N=4 CP-1

First "changes requested" verdict in the retroactive-spec sequence (N=1 / N=2 / N=3 all passed with comments). Driven by the **cross-spec consistency check** — new at N=4 because N=4 is the first quintet retroactive spec with a sibling design spec as an authoritative source. The check has a higher bar than prior CP-1 review-focuses: it requires that retro §5 subsection citations match real session-economy §5 subsection content. The bar is appropriate; the spec's own CP-1 review focus declares it. The author's mental model treated session-economy's "multi-repo discipline" as a single concept spread across §5.3 + §5.5, but those subsections architecturally commit to *other skills'* multi-repo behavior — spec-execute's Phase 4/6 paired-commit prose has no session-economy §5 subsection sourcing it. **Pattern for N=5:** the two-source structure (predecessor for rationale; sibling design spec for current-behavior-added-after-predecessor) requires per-§5-subsection attribution audits at authoring time — not just at CP-1 review time. Sessions 4 and 5 will encounter the same session-economy spec; the audit pattern carries.

### Pattern observation: N=3 Pattern-for-N=4 callouts re-validated at CP-1

- **#1 ("None surfaced" §13 reporting with triage table)** — not exercised at N=4 (two real OQs surfaced); pattern remains carry-forward candidate for N=5.
- **#2 (CP-2 trigger narrative narrows as CP-1s pass)** — validated and extended at N=4; the trigger now names both N=2 and N=3 already-passed CP-1s.
- **#3 (Section-heading citation discipline elevated to CP-1 review focus)** — validated at N=4; all four tech-stack.md citations point to heading lines.
- **#4 (Predecessor-richness varies; §8 Validation Approach adapts)** — validated and refined at N=4. The N=3 prediction "thin or no predecessor" was openly disconfirmed; §8 was *expanded* by a new sibling-design-spec cross-check row, not simplified. The refinement is itself the load-bearing N=4 addition.

### Pattern observation: New Pattern-for-N=5 callouts encounter their own first review test

The five new Pattern-for-N=5 callouts in [the authoring journal entry above](./journal.md) include #1 (two-source structure for trilogy-extended skills) and #5 (Phase 8's source-attribution model). This CP-1 review surfaces a refinement: the two-source structure isn't just an authoring discipline — it requires audit at every §5 subsection that cites the sibling design spec. The blocker findings at §5.4 / §5.6 are exactly the failure mode that audit would catch. **Refined Pattern-for-N=5 #1:** when authoring a retro spec with a sibling-design-spec source, walk every §5 subsection's session-economy citation against the sibling spec's actual §5 content before declaring authoring complete.

### Next action

[§11 Adoption Path step 2](./architecture.md) is **not yet closed** — CP-1 is open pending the amendment. Sequenced next action: invoke /spec-amend in this session (per operator direction) with the citation-correction batched amendment described above. After /spec-amend lands the fix, re-run /spec-review against CP-1 to capture the "pass with comments" verdict closing the checkpoint. Then resume the strategy-doc ordering toward session 4 (`spec-review` retroactive spec; N=5 in retroactive-spec sequence).

## 2026-05-18 — Amendment 2026-05-18-1

**Section amended:** [specs/20260518-spec-execute-skill/architecture.md](./architecture.md) §§5.4, 5.6, 6, 8, 9 (CP-1 and CP-2 review focuses)
**Trigger:** [CP-1 review verdict](./journal.md) (changes requested) at commit `57be7fa` — four blocker findings collapsed to one citation pattern.
**Reason:** Session-economy spec §5.3 / §5.5 cited as architectural source for spec-execute Phase 4/6 paired-commit discipline, but those subsections are about `spec-amend` and `spec-write`/`spec-design` respectively. Phase 4/6 strengthening is acknowledged in session-economy §1 / §3 but not §5-enumerated; commit `5ce4024` is the implementation event.
**Impact summary:** No atomic tasks. CP-1 stays open (re-review needed); CP-2 review focus updated. No completed work invalidated; behavior descriptions in §5.4 / §5.6 unchanged.
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at Draft — CP-1 still open with "changes requested" verdict; will be re-reviewed after this amendment.
**Commit:** `d11c405`

### Full record

**Trigger.** CP-1 review verdict (changes requested) at commit `57be7fa`. Four blocker findings collapse to one underlying citation pattern. Verified against [specs/20260514-session-economy/architecture.md §5](../20260514-session-economy/architecture.md): §5.3 covers `spec-amend`, §5.4 covers `spec-review`, §5.5 covers `spec-write`/`spec-design`. The session-economy spec has no §5 subsection that architecturally commits to spec-execute Phase 4/6 paired-commit discipline — the prose pre-existed in spec-execute and was strengthened by commit `5ce4024` without a paired §5 specification. Session-economy [§1 Overview](../20260514-session-economy/architecture.md) and [§3 Background](../20260514-session-economy/architecture.md) acknowledge this.

**Section.** Six sites total in [architecture.md](./architecture.md): §5.4 (Phase 4 multi-repo case, line 202); §5.6 (Phase 6 multi-repo case, line 233); §6 (NFR table Multi-repo discipline row, line 324); §8 (Validation Approach sibling-design-spec cross-check row, line 351); §9 (CP-1 review focus bullet, line 369); §9 (CP-2 review focus paragraph, line 385).

**Change.** Replaced "session-economy spec §5.3 / §5.5" citation pattern (and the corresponding §9 mapping assertions) with a two-attribution-shapes framing:
- **Shape (i) — §5-enumerated:** retro §5.1 ↔ session-economy §5.2 (Phase 1 multi-repo detection); retro §5.8 ↔ session-economy §5.1 (Phase 8 token economy).
- **Shape (ii) — narrative-sourced:** retro §5.4 and §5.6 cite session-economy §1 Overview + §3 Background plus commit `5ce4024` (Phase 4/6 paired-commit strengthening is not enumerated in session-economy §5).

Full before/after text for all six sites recorded in the amendment commit (`d11c405`) diff.

**Reason.** The original citations claimed session-economy §5.3 and §5.5 as the architectural source for spec-execute Phase 4/6 paired-commit discipline, but those subsections are about sibling skills. Phase 4/6 paired-commit prose was added to spec-execute SKILL.md by commit `5ce4024` (the session-economy commit) but the session-economy spec's §5 does not enumerate the change — it is acknowledged only in §1 Overview and §3 Background. The corrected citations preserve the sibling-design-spec attribution model (matching the spec's existing pattern) while honoring the actual structure: some commitments are §5-enumerated, others are narrative-sourced plus implementation commit.

**Impact.**
- **Affected tasks:** none (no atomic tasks in this design spec).
- **Affected checkpoints:** CP-1 (still open after this amendment lands; will be re-reviewed). CP-2 (review focus updated; not yet triggered).
- **Completed work invalidated:** none. The behavioral descriptions in §5.4 and §5.6 are unchanged; only the architectural attribution citations and §8 / §9 review-focus assertions change.
- **Cross-references requiring follow-up:** none. §3 Background line 41 already describes commit `5ce4024` as having "Strengthened Phase 4 / Phase 6 paired-commit prose" — accurate, no change. §10 Risks row about "Phase 8 token-economy factor is the only quintet commitment with a *sibling design spec* as its authoritative source" — still accurate (Phase 4/6 strengthening is now framed as narrative-sourced, distinct from §5.1 / §5.2 §5-enumerated commitments). §14 References — session-economy spec under Authoritative — still correct.

**Status implication.** Kept at Draft. The spec is currently at `Draft — Open for Review` per §1 banner; CP-1 is open with "changes requested" verdict. This amendment closes the citation-correction part of CP-1's blocker findings but does not advance the spec out of Draft — CP-1 must be re-run to reach a "pass with comments" verdict before the spec leaves Draft.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

### Pattern observation at amendment time

The amendment is the first instance of an amendment to a retroactive design spec (N=1 / N=2 / N=3 retroactive specs had zero amendments; their CP-1 verdicts were "pass with comments" with advisories logged but not amended). This amendment is also the first instance of a /spec-review → /spec-amend hand-off in the retroactive-spec sequence — exercising the documented hand-off pattern from `spec-amend` SKILL.md "HANDOFF NOTES → From `spec-review`". **Pattern observation:** the hand-off works as documented; the spec-review verdict's "Spec amendments proposed" section transferred cleanly to spec-amend's TRIGGER and PROPOSED_CHANGE inputs. **Pattern for N=5:** if sessions 4–5 (`spec-review` and `spec-amend` retroactive specs) surface similar cross-spec citation issues, the same /spec-review → /spec-amend hand-off applies; precedent established at N=4.

### Refined Pattern-for-N=5 #1 — concrete shape now visible

The CP-1 verdict refined Pattern-for-N=5 #1 (audit every §5 subsection's sibling-design-spec citation at authoring time). The amendment makes the audit's *output shape* concrete: when a sibling-design-spec source applies to a retro §5 subsection, the retro spec must declare whether the citation is **§5-enumerated** (the sibling spec has a §5 subsection committing to the behavior) or **narrative-sourced** (the sibling spec acknowledges the behavior in §1 / §3 / similar but does not enumerate it). The two-shapes framing in retro §8 / §9 carries forward to sessions 4–5 verbatim if they encounter the same session-economy spec attribution pattern.

## 2026-05-18 — Re-review of CP-1 (post-amendment)

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Verdict commit:** `061117e`
**Diff range:** `d11c405` (amendment edits to [architecture.md](./architecture.md) §5.4, §5.6, §6, §8, §9) + `a5c6c58` (this journal's amendment entry) layered on `1c95460` (original paired commit)
**Tasks reviewed:** none (retroactive design spec — no atomic tasks)
**Blockers:** 0 — the four blocker findings from the [original CP-1 verdict](./journal.md) (citation pattern at four sites) were escalated to [amendment 2026-05-18-1](./journal.md) (commit `d11c405`), which corrected six sites total. Verified against [session-economy/architecture.md §5](../20260514-session-economy/architecture.md): only §5.1 (Phase 8 token economy) and §5.2 (Phase 1 multi-repo detection) architecturally commit to `spec-execute` behavior; §5.3 / §5.4 / §5.5 commit to sibling skills; no §5 subsection enumerates Phase 4/6 paired-commit strengthening. The amendment's two-attribution-shapes framing is consistent with this structure.
**Important:** 0
**Advisory:** 3 (all carried forward unchanged from the original CP-1 verdict; none introduced or resolved by the amendment, which was scoped to blockers only).
**Spec amendments proposed:** none — the citation-pattern amendment (2026-05-18-1) is already applied; this re-review confirms it is correct.

### Review focus walk — itemized outcomes (re-review)

1. Every commitment in §4/§5/§6 corresponds to behavior in SKILL.md — **pass with comments** (advisory (a) carried forward).
2. No commitment contradicts the shipping SKILL.md — **pass**. Amendment was citation-only; behavioral descriptions unchanged.
3. ASPP correctly characterized as binding — **pass** (unchanged from prior verdict).
4. Predecessor doc distinguished as authoritative-for-rationale-not-current-behavior — **pass** (unchanged).
5. Session-economy spec distinguished as sibling-authoritative; two attribution shapes hold — **pass**. Shape (i) §5-enumerated: retro [§5.1](./architecture.md) → session-economy [§5.2](../20260514-session-economy/architecture.md) ✓; retro [§5.8](./architecture.md) → session-economy [§5.1](../20260514-session-economy/architecture.md) ✓. Shape (ii) narrative-sourced: session-economy [§1 line 10](../20260514-session-economy/architecture.md) + [§3 lines 37–39](../20260514-session-economy/architecture.md) establish pre-existence and strengthening-intent; commit `5ce4024` is the implementation event. The CP-1 review focus bullet at [§9](./architecture.md) now declares its own check correctly — the spec's own check no longer fails on itself.
6. Eight phases + Amendment Protocol match SKILL.md — **pass** (folded into #1).
7. §13 OQ-1 and OQ-2 named with full structure — **pass** (unchanged).
8. Self-contained — **pass with comments** (advisory (c) carried forward).
9. Section-heading citation discipline — **pass** (unchanged).
10. Portability rule for links honored — **pass** (unchanged).

### Advisory carry-forward

- (a) [§5.6](./architecture.md) Behavior opens "Four updates fire" then lists five numbered items. Not addressed by the amendment (which was blocker-scoped); trivially correctable in a future amendment or accepted as inconsequential.
- (b) [§5.10](./architecture.md) and [§5.11](./architecture.md) describe disciplines applied to this spec's own prose rather than commitments of the shipping SKILL.md. Carried forward; mirrors [N=2 advisory (a)](../20260518-spec-design-skill/journal.md) and [N=3 advisory (a)](../20260518-spec-write-skill/journal.md).
- (c) [§3 Background](./architecture.md) and [§8 Validation Approach](./architecture.md) cross-reference density. Carried forward from [N=2 advisory (d)](../20260518-spec-design-skill/journal.md) / [N=3 advisory (b)](../20260518-spec-write-skill/journal.md).

### Exit criteria status

- Reviewer verdict in structured format: **met** (this entry).
- All blocker findings resolved or escalated to /spec-amend: **met** (amendment 2026-05-18-1, commit `d11c405`).
- Verdict written back to spec §9 status line and journal: **met** ([§9 CP-1 Status](./architecture.md) updated in same change; this entry).

### Pattern observation at N=4 re-review

This is the first /spec-review → /spec-amend → /spec-review (re-review) cycle in the retroactive-spec sequence — N=1 / N=2 / N=3 all reached "pass with comments" on first review with no amendment. The cycle works as documented: the re-review verifies the amendment's scope (six sites touched, all correctly attributed) and confirms the amendment's two-shapes framing now stands up to the CP-1 review focus bullet that originally fell through. **Pattern for N=5:** the re-review cycle is a viable path for any future CP-1 "changes requested" verdict whose blockers collapse to a single citation pattern fixable by /spec-amend without disturbing behavioral descriptions. The amendment-then-re-review pattern adds one commit pair (amendment + journal) and one verdict; the cost is bounded and the trail is fully visible.

### Pattern observation: the refined Pattern-for-N=5 #1 audit shape held

The [refined Pattern-for-N=5 #1](./journal.md) (audit every §5 subsection's sibling-design-spec citation at authoring time, with §5-enumerated vs narrative-sourced as the two output shapes) was tested by this re-review and held — the amendment's six-site framing is internally consistent, externally verified against session-economy §5, and symmetrical across §5.4 / §5.6 / §6 NFR / §8 / §9 CP-1 / §9 CP-2. Sessions 4 and 5 inherit this audit shape if they encounter the same session-economy spec attribution pattern (`spec-review` will; `spec-amend` will).

### Status implication

Spec advances out of "changes requested" into "pass with comments" — CP-1 is now **closed**. The spec's [§1 banner](./architecture.md) still reads `Draft — Open for Review`; per N=1 / N=2 / N=3 precedent, the Draft banner remains until CP-2 (drift audit, batched) runs. CP-2's trigger ([§9](./architecture.md)) now narrows further: the remaining condition is "two sibling quintet CP-1s (`spec-review`, `spec-amend`) + project-constitution CP-2" — `spec-execute`'s CP-1 has joined N=2 and N=3 as a passed predecessor.

### Next action

Per the [strategy-doc session-4 ordering](../../docs/retroactive-spec-strategy.md) and the [next-action pointer in the authoring journal entry](./journal.md), the next session is the `spec-review` retroactive spec (session 4 of the legacy quintet; N=5 in the retroactive-spec sequence). Pre-confirmed predecessor: [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446+. Pre-confirmed sibling design spec: [session-economy §5.4](../20260514-session-economy/architecture.md). The five "Pattern for N=5" callouts (in the authoring entry above) plus the refined Pattern-for-N=5 #1 (audit shape concretized in amendment 2026-05-18-1) and the new re-review-cycle observation above are the N=5 inputs.

## 2026-05-18 — Review of CP-2

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Diff range:** [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md) (lastUpdated 2026-05-15) vs [architecture.md](./architecture.md) §4 / §5 / §6 / §12, post-amendment 2026-05-18-1 (commit `d11c405`) and CP-1 re-review (commit `061117e`).
**Tasks reviewed:** none (retroactive design spec — no atomic tasks).
**Blockers:** 0
**Important:** 0
**Advisory:** 5 — D-1 through D-5 enumerated below. Three are WND-partial-home or preamble-vs-Phase-body class findings surfaced explicitly per [batch journal N=3 Pattern-for-N=4 observations (1)/(2)/(3)](../20260518-cp2-batch-audit/journal.md#L93). Two carry forward CP-1 advisories (none promoted to blocker).
**Spec amendments proposed:** three (a)-route findings (§5.4 / §5.6 / §5.9) plus two (b)-route findings (SKILL.md preamble line 9 + line 13). Routings confirmed by operator via AskUserQuestion at audit close; amendments routed via /spec-amend in a subsequent step.

### Audit context

N=4 of the five-spec batched CP-2 audit driven by [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) "Drift mitigation"; see [batch journal](../20260518-cp2-batch-audit/journal.md) for routing tally and cross-skill pattern context. This session is the **N=3 robustness check** on the most-divergent skill shape (eight-phase iterative execution workflow vs the single-shot authoring shape of N=2/N=3). Per [batch journal N=3 Pattern-for-N=4 observations](../20260518-cp2-batch-audit/journal.md#L93): observation (1) operator (c)→(a) override; observation (2) WND partial-home as first-class step; observation (3) preamble-vs-Phase-body mirror walk in both directions.

### Divergence list with operator-confirmed routings

| ID | Summary | Class | Routing |
|---|---|---|---|
| D-1 | WND-8 ("no speculative code for the next task") lacks explicit §5/§6 carrier — §5.4 cites OP §5 "One task at a time" as Pattern invoked but does not reproduce the speculative-code prohibition | WND partial-home (observation 2) | **(a) amend spec §5.4** — confirmed |
| D-2 | WND-9 ("amendments require diff; surgical not rewrite") only partially carried — §5.3 covers pre-execution amendments; §5.9 Amendment Protocol Behavior does not reproduce the rule for Phase 4/5 trigger sites | WND partial-home (observation 2) | **(a) amend spec §5.9** — confirmed |
| D-3 | [SKILL.md `# Spec Execute` preamble (line 9)](../../.agents/skills/spec-execute/SKILL.md#L9) omits `spec-amend` from the pairing list — frontmatter description and AMENDMENT PROTOCOL both reference it | Preamble-vs-Phase-body mirror (observation 3) | **(b) amend SKILL.md preamble** — confirmed |
| D-4 | [SKILL.md "How this skill works" preamble (line 13)](../../.agents/skills/spec-execute/SKILL.md#L13) does not name Phase 8 Session Continuity Check — frontmatter description and Phase 8 body both name the task-boundary pause | Preamble-vs-Phase-body mirror (observation 3) | **(b) amend SKILL.md preamble** — confirmed |
| D-5 | [§5.6](./architecture.md) opens "**Four updates fire** before the task is declared complete" and lists **five** numbered items — CP-1 advisory (a) carry-forward | Protocol detail (carry-forward; operator (c)→(a) override candidate per observation 1) | **(a) amend spec §5.6 "Four → Five"** — confirmed |

**Routing tally for N=4:** amend-spec ×3 (D-1, D-2, D-5); amend-SKILL.md ×2 (D-3, D-4); accept ×0.

### Cross-spec consistency check (per [CP-2 review focus](./architecture.md#L385))

- **Shape (i) §5-enumerated.** Retro [§5.1](./architecture.md) (Phase 1 multi-repo detection) ↔ [session-economy §5.2](../20260514-session-economy/architecture.md#L107) ✓. Retro [§5.8](./architecture.md) (Phase 8 token economy) ↔ [session-economy §5.1](../20260514-session-economy/architecture.md#L91) ✓.
- **Shape (ii) narrative-sourced.** Retro [§5.4](./architecture.md) and [§5.6](./architecture.md) multi-repo paragraphs cite [session-economy §1 Overview](../20260514-session-economy/architecture.md#L8) + [§3 Background](../20260514-session-economy/architecture.md#L31) + commit `5ce4024`. Verified: session-economy §5 has no subsection committing to spec-execute Phase 4/6 paired-commit strengthening — session-economy §5.3 covers spec-amend, §5.4 covers spec-review, §5.5 covers spec-write+spec-design. The narrative-sourced attribution is structurally correct. §6 Multi-repo discipline NFR and §8 Validation Approach Sibling-design-spec cross-check row both carry the two-shapes framing. Amendment 2026-05-18-1 closure verified at CP-2.

### Review focus walk — itemized outcomes

1. Every commitment in §4/§5/§6 corresponds to behavior in SKILL.md — **pass with comments** (D-1, D-2, D-5 advisories).
2. No commitment contradicts SKILL.md — **pass**.
3. WND items walked against §5/§6 carriers (first-class step per observation 2) — **pass with comments** (D-1, D-2).
4. SKILL.md preamble walked line-by-line against Phase 2 body in both directions (observation 3) — **pass with comments** (D-3, D-4).
5. Cross-spec consistency shape (i) and shape (ii) — **pass** (amendment 2026-05-18-1 closure verified).
6. Cross-skill drift patterns — **pass** (ASPP citation, session-economy commitment propagation, two-source structure, section-heading citation discipline, amendment-ID citation correctness all verified).
7. §12 Out of Scope walk — **pass** (scope exclusions; no SKILL.md-behavior verification applies).

### Exit criteria status

- Divergence list produced: **met** (five advisory findings).
- Routing decision per divergence: **met** (operator confirmed all five via AskUserQuestion at audit close).
- No silent edits: **met** (this entry; spec §9 CP-2 Status line; batch journal N=4 entry).
- Outcome recorded in this spec's journal as closing entry of retroactive-spec adoption: **met** (this entry).

### Pattern observations at N=4 CP-2

- **WHAT NOT TO DO partial-home class — four-data-point confirmation.** N=1 D-4, N=2 D-4, N=3 D-1, N=4 D-1+D-2. The pattern is stable across four consecutive CP-2 audits. Observation (2)'s "first-class step" protocol confirmed valuable — walking WND items against §5/§6 carriers as a discrete audit step (not absorbed into "behavioral coverage looks fine") surfaced two findings (D-1, D-2) that would otherwise have been silent.
- **Preamble-vs-Phase-body mirror class — both directions confirmed at four data points total.** N=2 D-2 (preamble names item body omits), N=3 D-5 (body enumerates item preamble omits), N=4 D-3+D-4 (preamble omits items body and frontmatter both enumerate — same direction as N=3 D-5). Bidirectional walk per observation (3) produced both findings at N=4.
- **Operator (c)→(a) override pattern (observation 1) applied at three findings.** D-1, D-2, D-5 all had (c) accept-as-known-minor as the conservative default; reviewer surfaced (a) explicitly per observation (1) precedent (N=3 D-4); operator confirmed (a) on all three. The pattern is now load-bearing: surfacing protocol-detail findings explicitly converts conservative-default routings to active amendments. This intentionally raises the amend-spec count at N=4 vs prior N audits.
- **Status-banner-lifecycle finding class (N=2 D-1) did NOT fire at N=4.** spec-execute §1 banner reads `Draft — Open for Review` but no §5.x commits to a lifecycle (Draft → Approved → Superseded or similar); the lifecycle commitment shape was specific to spec-design's §5.8. The candidate finding-class for N=4 did not surface as predicted. Banner stays at Draft pending closure of routed amendments and re-verification.
- **Robustness check on most-divergent skill shape — passed.** Per [strategy doc](../../docs/retroactive-spec-strategy.md), this session was the robustness check on the spec-design / spec-write CP-2 pattern. The eight-phase shape produces a denser §5 (eleven subsections vs N=2/N=3's eight), a sibling-design-spec source (first-of-kind), and four findings — but the audit shape (review focus walk → divergence list → routing decisions → cross-skill pattern observations) generalized cleanly. Finding classes (WND partial-home; preamble-vs-body mirror; protocol-detail surfacing) are the same as N=2/N=3.
- **CP-2 closure compounds.** The CP-2 batch's narrowed trigger condition now reads "one sibling quintet CP-1 (`spec-review`, `spec-amend`) + project-constitution CP-2" once CP-2 closure here is committed — N=2, N=3, and N=4 CP-2s all having closed.
- **Pattern for N=5 (spec-review at session 4).** spec-review will encounter the same session-economy spec as a sibling design spec ([session-economy §5.4](../20260514-session-economy/architecture.md#L147)); the two-shapes attribution framing from amendment 2026-05-18-1 carries verbatim where applicable. spec-review's WND items + preamble should be walked the same way (observations 2 and 3). The CP-1 re-review cycle precedent established here is the available path if N=5 CP-1 surfaces blockers collapsing to a single citation pattern.

### Next action

Three steps, in order:

1. **Route amendments via /spec-amend** — the three (a)-route findings (D-1, D-2, D-5) and two (b)-route findings (D-3, D-4) per operator-confirmed routings. spec amendments collapse into a single /spec-amend session for the spec; SKILL.md amendments into a separate /spec-amend session for the SKILL.md preamble. Each amendment carries the trigger (CP-2 audit finding ID), proposed change, impact, and approver.
2. **Re-verify CP-2 closure** — after amendments land, this spec's CP-2 may need a re-verification entry (parallel to the CP-1 re-review pattern) if amendments materially change the §5/§6 audit surface. For (b)-route amendments to SKILL.md preamble, no spec-side re-verification is required (preamble change does not alter Phase-body content). For (a)-route amendments to §5.4/§5.6/§5.9, a brief re-audit confirms the divergences are closed; can be a short journal entry rather than a full /spec-review session.
3. **Resume batch audit** — proceed to N=5 (`spec-review` retroactive spec CP-2) per [strategy doc](../../docs/retroactive-spec-strategy.md) and [batch journal](../20260518-cp2-batch-audit/journal.md) ordering.

CP-2 closure for spec-execute is **conditional on amendment landing**: the §9 Status line records "pass with comments" with five advisories; full closure (banner out of Draft) waits on amendments + re-verification per step 2 above.

## 2026-05-18 — Amendment 2026-05-18-2

**Section amended:** [architecture.md](./architecture.md) §5.4 Phase 4 — Execute
**Trigger:** CP-2 audit finding D-1 — WND-8 (no speculative next-task code) lacked §5/§6 carrier
**Reason:** WND-8 is behaviorally distinct from OP §5 "one task at a time"; the spec's Phase 4 commitments must reproduce the pre-staging prohibition the shipping SKILL.md states
**Impact summary:** Affects no atomic tasks (retroactive design spec); closes CP-2 D-1; no completed work invalidated; no cross-reference follow-up
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at Draft (banner unchanged; CP-2 closure now one step closer)
**Commit:** `5cfcfae`

### Full record

**Trigger.** N=4 CP-2 audit (this journal "Review of CP-2") finding D-1: WND-8 ("Do not write speculative code for 'the next task' while finishing the current one") lacks an explicit §5/§6 carrier. §5.4 cites OP §5 "One task at a time" as Pattern invoked but does not reproduce the speculative-code prohibition. Pattern class: "WND partial-home" (N=4 observation 2 — four-data-point confirmation across N=1 D-4, N=2 D-4, N=3 D-1, N=4 D-1+D-2).

**Section.** §5.4 Phase 4 — Execute, **Behavior** paragraph (line 200) and **Pattern invoked** line (line 204).

**Change.**

Before (Behavior):
> **Behavior.** Match existing codebase conventions identified in the feature spec's §3 Background. Do not introduce new patterns, dependencies, or abstractions unless the task explicitly calls for them. Write tests required by the task alongside production code (tests and code land together). Keep changes scoped to the task's declared file list — touching a file outside that list requires stopping and proposing a spec amendment first. Commit at logical points within the task with messages that reference the task ID (e.g. `T-04: add validator for Foo input`). If the task as specified cannot be completed correctly, stop and propose a spec amendment — do not work around the spec.

After (Behavior):
> **Behavior.** Match existing codebase conventions identified in the feature spec's §3 Background. Do not introduce new patterns, dependencies, or abstractions unless the task explicitly calls for them. Write tests required by the task alongside production code (tests and code land together). Keep changes scoped to the task's declared file list — touching a file outside that list requires stopping and proposing a spec amendment first. Do not write speculative code for "the next task" while finishing the current one — pre-staging the next task's work blurs the boundary that closeout depends on. Commit at logical points within the task with messages that reference the task ID (e.g. `T-04: add validator for Foo input`). If the task as specified cannot be completed correctly, stop and propose a spec amendment — do not work around the spec.

Before (Pattern invoked):
> **Pattern invoked.** *No silent deviation* — OP §3 in the shipping SKILL.md. *One task at a time* — OP §5.

After (Pattern invoked):
> **Pattern invoked.** *No silent deviation* — OP §3 in the shipping SKILL.md. *One task at a time* — OP §5. *No speculative code for the next task* — [SKILL.md WHAT NOT TO DO](../../.agents/skills/spec-execute/SKILL.md).

**Reason.** WND-8 is a behaviorally distinct rule from "one task at a time": OP §5 forbids *interleaving* tasks, while WND-8 forbids *pre-staging* the next task's code while the current is still in flight. The retroactive spec's §5.4 carried scope-expansion (file-list discipline) and workaround prohibition but was silent on pre-staging. The shipping SKILL.md is explicit at WND-8.

**Impact.**
- **Affected tasks:** none (retroactive design spec — no atomic tasks).
- **Affected checkpoints:** CP-2 already at "pass with comments"; D-1 now closed.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none. §6 NFR table cites OP-class principles, not WND items; no edit needed.

**Status implication.** Kept at Draft. Spec advances toward unconditional CP-2 closure once all three (a)-route amendments + the (b)-route SKILL.md amendments land and the brief re-audit confirms closure per the audit's "Next action" step 2.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-3

**Section amended:** [architecture.md](./architecture.md) §5.9 Amendment Protocol — proposing vs applying
**Trigger:** CP-2 audit finding D-2 — WND-9 (diff required; surgical not rewrite) only partially carried; §5.3 covers pre-execution amendments but §5.9 omits the rule for Phase 4/5 trigger sites
**Reason:** The proposing skill carries an upstream obligation distinct from the applying skill's structural enforcement; without it at §5.9, rewrites get smuggled through as amendments
**Impact summary:** Affects no atomic tasks; closes CP-2 D-2; no completed work invalidated; N=5 `spec-amend` retroactive spec may cite this section back
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at Draft
**Commit:** `a948061`

### Full record

**Trigger.** N=4 CP-2 audit finding D-2: WND-9 ("Do not produce a spec amendment without showing the diff against the existing section. Amendments are surgical, not rewrites") is only partially carried — §5.3 covers pre-execution amendments (Phase 3); §5.9 Amendment Protocol Behavior does not reproduce the diff-required + surgical-not-rewrite discipline for Phase 4/5 trigger sites. Pattern class: WND partial-home (observation 2).

**Section.** §5.9 Amendment Protocol — proposing vs applying, **Behavior (the `spec-execute` side)** step 5 and **Pattern invoked** line.

**Change.**

Before (Behavior step 5):
> 5. Hand off to `spec-amend`, passing `SECTION`, `TRIGGER`, and any `PROPOSED_CHANGE` text.

After (Behavior step 5):
> 5. Hand off to `spec-amend`, passing `SECTION`, `TRIGGER`, and any `PROPOSED_CHANGE` text. Any `PROPOSED_CHANGE` carried forward must be expressible as a diff against the existing section — surgical, not a rewrite. If the change cannot be expressed surgically, route as a rewrite candidate (`spec-write` re-decomposition) rather than as an amendment.

Before (Pattern invoked):
> **Pattern invoked.** *Separation of proposing from applying* — added at trilogy commit `80000b1`, distinct from the predecessor's inline Amendment Protocol (predecessor lines 391–403 applied amendments within the execution session). The current routing is the methodology's commitment that amendments are first-class events.

After (Pattern invoked):
> **Pattern invoked.** *Separation of proposing from applying* — added at trilogy commit `80000b1`, distinct from the predecessor's inline Amendment Protocol (predecessor lines 391–403 applied amendments within the execution session). The current routing is the methodology's commitment that amendments are first-class events. *Diff required; surgical not rewrite* — [SKILL.md WHAT NOT TO DO](../../.agents/skills/spec-execute/SKILL.md); the proposing side carries the obligation by ensuring `PROPOSED_CHANGE` is expressible as a diff against the existing section.

**Reason.** WND-9 governs the *form* of the amendment artifact at the trigger site. The `spec-amend` skill enforces diff-required structurally (its Phase 2 template requires Before/After). But the *proposing* skill carries the upstream obligation: not all triggered changes are surgical, and `spec-execute` must distinguish "the spec needs a one-paragraph amendment" from "the spec needs to be re-decomposed by `spec-write`." Without that distinction at the proposing side, rewrites get smuggled through as amendments. The retroactive spec was silent on this; the shipping SKILL.md is explicit at WND-9.

**Impact.**
- **Affected tasks:** none.
- **Affected checkpoints:** closes CP-2 finding D-2.
- **Completed work invalidated:** none. The `spec-amend` retroactive spec at N=5 may cite this amendment as the upstream-side commitment paired with `spec-amend`'s downstream-side rewrite-reclassification step.
- **Cross-references requiring follow-up:** none in this spec.

**Status implication.** Kept at Draft.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-4

**Section amended:** [architecture.md](./architecture.md) §5.6 Phase 6 — Update artifacts
**Trigger:** CP-2 audit finding D-5 (also CP-1 advisory (a) carry-forward); Behavior opener "Four updates fire" mismatched a five-item list
**Reason:** Trivial count mismatch; CP-1 advisory (c)→(a) override per N=4 observation 1 converted the conservative default into an active amendment
**Impact summary:** Affects no atomic tasks; closes CP-1 advisory (a) carry-forward and CP-2 D-5; no completed work invalidated; no cross-reference follow-up
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at Draft
**Commit:** `63c0e12`

### Full record

**Trigger.** N=4 CP-2 audit finding D-5 (also CP-1 advisory (a) carry-forward, escalated (c)→(a) per N=4 observation 1): §5.6 Behavior opened "**Four updates fire** before the task is declared complete" and listed five numbered items.

**Section.** §5.6 Phase 6 — Update artifacts, opener of the **Behavior** block (line 226).

**Change.**

Before:
> **Behavior.** Four updates fire before the task is declared complete:

After:
> **Behavior.** Five updates fire before the task is declared complete:

(List items 1–5 unchanged; only the count word changed.)

**Reason.** Trivial count mismatch surfaced at CP-1 (advisory (a) carry-forward) and re-surfaced at CP-2. The (c)→(a) override per N=4 observation 1 converted the conservative default ("accept as known minor") into an active amendment, consistent with the pattern applied to D-1 and D-2.

**Impact.**
- **Affected tasks:** none.
- **Affected checkpoints:** closes CP-1 advisory (a) carry-forward and CP-2 finding D-5.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none. §5.4's "Multi-repo case" mentions "the artifact-update commit" in singular form (item 5 in §5.6's list); still correct.

**Status implication.** Kept at Draft.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

### Pattern observation at three-amendment close

This /spec-amend session bundled three amendments into one invocation — the first such instance in the retroactive-spec sequence. Prior /spec-amend sessions handled one amendment per invocation (2026-05-18-1 CP-1 citation correction at N=4; amendments 1–5 in N=3 spec-write spec each in their own session). The bundling was operator-confirmed at audit close ("spec amendments collapse into a single /spec-amend session for the spec") and worked cleanly: shared TRIGGER (CP-2 audit), shared Orientation Report, three distinct Phase 2 drafts presented together, single batch approval at Phase 3, three sequential Phase 4 commits, three sequential Phase 5 journal entries. **Pattern for N=5 and N=6:** when a CP-2 audit routes multiple (a)-route findings to the same spec, batching them in one /spec-amend session is the path of least friction. The skill's "one coherent change per amendment" rule still holds at the *amendment* level (each is surgical, each gets its own commit + journal entry + ID); it does not require one *invocation* per amendment.

## 2026-05-18 — Amendment 2026-05-18-5

**Section amended:** [.agents/skills/spec-execute/SKILL.md preamble — paragraph following "# Spec Execute" (line 9)](../../.agents/skills/spec-execute/SKILL.md#L9)
**Trigger:** CP-2 audit finding D-3 — pairing list omits `spec-amend` despite frontmatter description, AMENDMENT PROTOCOL phase, and spec §4 all committing to the triad
**Reason:** Aligns SKILL.md preamble pairing list to its own frontmatter description, AMENDMENT PROTOCOL body, and spec §4 commitments
**Impact summary:** No tasks affected; CP-2 D-3 resolved; no completed work invalidated; no cross-reference follow-up
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at Draft (CP-2 already at `pass with comments`; SKILL.md preamble edit only)
**Commit:** `988818d`

### Full record

**Trigger.** N=4 CP-2 audit finding D-3 ([batch journal](../20260518-cp2-batch-audit/journal.md#L116), [spec journal "Review of CP-2"](#L334)). Preamble line 9 pairing list reads *"Pairs with `spec-write` (authors the spec) and `spec-review` (reviews checkpoint deliverables)"* — omits `spec-amend`, despite the frontmatter description naming it as the third peer and the AMENDMENT PROTOCOL phase in the body routing to it. Preamble-vs-Phase-body mirror class (N=4 observation 3); same direction as N=3 D-5.

**Section.** [.agents/skills/spec-execute/SKILL.md preamble — paragraph following "# Spec Execute" (line 9)](../../.agents/skills/spec-execute/SKILL.md#L9). Single-paragraph section; the change replaces the pairing-list clause.

**Change.**

Before:
> In-session execution against an existing spec. Pairs with `spec-write` (authors the spec) and `spec-review` (reviews checkpoint deliverables). Keeps the spec as the source of truth, prevents silent deviation, and survives end-of-session context decay by doing frequent small closeouts at task boundaries instead of one large one at session end.

After:
> In-session execution against an existing spec. Pairs with `spec-write` (authors the spec), `spec-review` (reviews checkpoint deliverables), and `spec-amend` (applies spec changes when execution reveals drift). Keeps the spec as the source of truth, prevents silent deviation, and survives end-of-session context decay by doing frequent small closeouts at task boundaries instead of one large one at session end.

**Reason.** The preamble is a contract summary; readers (operators and review skills) rely on it to know who this skill talks to. Omitting `spec-amend` understates the AMENDMENT PROTOCOL relationship that the body, the frontmatter description, and the spec §4 all commit to. The amendment aligns the preamble with the rest of SKILL.md and with the spec.

**Impact.**
- **Affected tasks:** none (retroactive design spec; no §7 Task Breakdown).
- **Affected checkpoints:** [CP-2](./architecture.md) — closing entry will reference this amendment.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none. Frontmatter description (line 4) already names spec-amend as the third peer; AMENDMENT PROTOCOL phase already references it; spec §4 already commits to the triad.

**Status implication.** Kept at Draft. Spec is currently at `Draft — Open for Review` per §1 banner; CP-2 §9 Status line records `pass with comments` from the N=4 audit. This amendment touches SKILL.md preamble only; spec commitments unchanged. No revert to Draft.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-6

**Section amended:** [.agents/skills/spec-execute/SKILL.md preamble — "How this skill works" paragraph (line 13)](../../.agents/skills/spec-execute/SKILL.md#L13)
**Trigger:** CP-2 audit finding D-4 — preamble enumerates task-cycle moments but does not name the Phase 8 session-continuity check that frontmatter description and Phase 8 body both commit to
**Reason:** Aligns SKILL.md preamble per-task cycle to its own frontmatter description and Phase 8 body
**Impact summary:** No tasks affected; CP-2 D-4 resolved; no completed work invalidated; no cross-reference follow-up
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at Draft (CP-2 already at `pass with comments`; SKILL.md preamble edit only)
**Commit:** `91e30d1`

### Full record

**Trigger.** N=4 CP-2 audit finding D-4 ([batch journal](../20260518-cp2-batch-audit/journal.md#L117), [spec journal "Review of CP-2"](#L335)). Preamble line 13 enumerates the per-task cycle (orient → propose → approve → update spec/journal) but does not name the Phase 8 Session Continuity Check, despite the frontmatter description committing to it (*"At every task boundary, pauses for a session-continuity check (continue in this session vs. pick up fresh)"*) and Phase 8 body enumerating the pause as a first-class step. Preamble-vs-Phase-body mirror class (N=4 observation 3); same direction as D-3 and N=3 D-5.

**Section.** [.agents/skills/spec-execute/SKILL.md preamble — "How this skill works" paragraph (line 13)](../../.agents/skills/spec-execute/SKILL.md#L13). Single-paragraph section; the change adds one clause near the end of the per-task cycle description.

**Change.**

Before:
> When invoked, you act as the agent. Gather the INPUTS below from the user — infer what you can from the working directory and recent conversation, ask explicitly only for what is missing or ambiguous. Then orient against the spec, verify the last task's Definition of Done, and propose the next task. Wait for approval before any new work begins. At every task completion, update the spec and the session journal. If the session ends abruptly, the last completed task is already clean.

After:
> When invoked, you act as the agent. Gather the INPUTS below from the user — infer what you can from the working directory and recent conversation, ask explicitly only for what is missing or ambiguous. Then orient against the spec, verify the last task's Definition of Done, and propose the next task. Wait for approval before any new work begins. At every task completion, update the spec and the session journal, then pause at the task boundary for a session-continuity check (continue in this session vs. pick up fresh). If the session ends abruptly, the last completed task is already clean.

**Reason.** The preamble describes the per-task cycle as a contract summary; readers rely on it to know what fires at each task boundary. Omitting the Phase 8 session-continuity pause understates a behavior the frontmatter description and Phase 8 body both commit to, and matters operationally (operators reading only the preamble would miss the deliberate stop). The added clause is the minimum naming required to align the preamble with the frontmatter description and the Phase 8 body.

**Impact.**
- **Affected tasks:** none (retroactive design spec; no §7 Task Breakdown).
- **Affected checkpoints:** [CP-2](./architecture.md) — closing entry will reference this amendment.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none. Frontmatter description (line 4) already names the pause; Phase 8 body enumerates it; spec §5 carries Phase 8.

**Status implication.** Kept at Draft. Spec is currently at `Draft — Open for Review` per §1 banner; CP-2 §9 Status line records `pass with comments` from the N=4 audit. Preamble-only change; spec commitments unchanged. No revert to Draft.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

### Pattern observation at (b)-route session close

This /spec-amend session was the SKILL.md-side counterpart to the (a)-route session that bundled amendments 2026-05-18-2 / -3 / -4 — first time the retroactive-spec sequence has run two /spec-amend invocations from a single CP-2 verdict, split by routing target. The split followed the audit's "Next action" step-1 phrasing verbatim (*"spec amendments collapse into a single /spec-amend session for the spec; SKILL.md amendments into a separate /spec-amend session for the SKILL.md preamble"*) and worked cleanly: two independent surgical edits to the same preamble, both in the same finding class (preamble-vs-Phase-body mirror, observation 3, same direction as N=3 D-5), each with its own ID + commit + journal entry. **Pattern for N=5 and N=6:** when a CP-2 audit routes findings to two different targets (spec body + SKILL.md), splitting into two /spec-amend invocations keeps each invocation's TRIGGER coherent (one target file, one Orientation Report) while preserving the per-amendment ID/commit/journal discipline. Confirms the rule from the prior session — *"one coherent change per amendment"* applies at the amendment level, not the invocation level; the invocation-split discriminator is *target artifact*, not *amendment count*.

### CP-2 closeout (post-amendment)

With amendments 2026-05-18-2, 2026-05-18-3, 2026-05-18-4, 2026-05-18-5, and 2026-05-18-6 all approved and applied, CP-2 routing is complete:

- D-1 → resolved via [2026-05-18-2](#L382) (route a, architecture.md §5.4 WND-8 speculative-code prohibition).
- D-2 → resolved via [2026-05-18-3](#L425) (route a, architecture.md §5.9 WND-9 diff-required + surgical-not-rewrite at Phase 4/5 trigger sites).
- D-3 → resolved via 2026-05-18-5 (route b, SKILL.md preamble pairing-list adds `spec-amend`).
- D-4 → resolved via 2026-05-18-6 (route b, SKILL.md preamble names Phase 8 session-continuity check).
- D-5 → resolved via [2026-05-18-4](#L468) (route a, architecture.md §5.6 Four→Five count correction; CP-1 advisory (a) carry-forward + CP-2 D-5 closed; **operator override** of reviewer-proposed (c) accept-as-minor per N=4 observation 1).

The N=4 audit's "Next action" step 2 conditional ("a brief re-audit confirms divergences are closed; can be a short journal entry rather than a full /spec-review session"; "for (b)-route amendments to SKILL.md preamble, no spec-side re-verification is required") is now actionable: the three (a)-route spec edits warrant a short re-verification journal entry; the two (b)-route SKILL.md edits do not. CP-2 closure for spec-execute is **conditional on the brief re-audit landing** per step 2.

**Pattern for N=5.** Per [batch journal N=4 Pattern-for-N=5](../20260518-cp2-batch-audit/journal.md#L370), spec-review CP-2 will encounter the same session-economy sibling-design-spec source ([session-economy §5.4](../20260514-session-economy/architecture.md#L147)). The amendment-count tally for spec-execute (5 total) matches spec-write's tally (5 total); the file-split pattern (3 spec + 2 SKILL.md) is also identical to spec-write's pattern. Both candidates for cross-cutting observation in the batch closing summary.

## 2026-05-18 — CP-2 re-verification (post-amendment)

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass — CP-2 closed unconditionally
**Scope:** Brief re-verification of [architecture.md](./architecture.md) §5.4, §5.6, §5.9 against the post-amendment state, per the [N=4 CP-2 audit "Next action" step 2 conditional](#L373) — short journal entry, not a full `/spec-review` session. The two (b)-route SKILL.md preamble amendments (D-3 via 2026-05-18-5, D-4 via 2026-05-18-6) require no spec-side re-verification per that same step-2 carve-out and are confirmed closed by the [CP-2 closeout (post-amendment)](#L589) block alone.

### Re-verification walk

| Finding | Amendment | Section | Closure evidence |
|---|---|---|---|
| D-1 — WND-8 speculative-code prohibition partial-home | [2026-05-18-2](#L382) (commit `5cfcfae`) | [§5.4 Phase 4 — Execute](./architecture.md#L196) | **Behavior** paragraph now carries *"Do not write speculative code for 'the next task' while finishing the current one — pre-staging the next task's work blurs the boundary that closeout depends on."* **Pattern invoked** line now cites *"No speculative code for the next task — [SKILL.md WHAT NOT TO DO]"*. Matches SKILL.md WND-8 obligation; cite-back is correct. **Closed.** |
| D-2 — WND-9 diff-required + surgical-not-rewrite partial-home | [2026-05-18-3](#L425) (commit `a948061`) | [§5.9 Amendment Protocol — proposing vs applying](./architecture.md#L267) | Behavior step 5 now carries *"Any `PROPOSED_CHANGE` carried forward must be expressible as a diff against the existing section — surgical, not a rewrite. If the change cannot be expressed surgically, route as a rewrite candidate (`spec-write` re-decomposition) rather than as an amendment."* **Pattern invoked** line now cites *"Diff required; surgical not rewrite — [SKILL.md WHAT NOT TO DO]; the proposing side carries the obligation by ensuring `PROPOSED_CHANGE` is expressible as a diff against the existing section."* Phase 4/5 trigger sites now covered alongside §5.3's pre-execution coverage. **Closed.** |
| D-5 — Four→Five count mismatch (CP-1 advisory (a) carry-forward + CP-2 D-5) | [2026-05-18-4](#L468) (commit `63c0e12`) | [§5.6 Phase 6 — Update artifacts](./architecture.md#L222) | **Behavior** opener now reads *"Five updates fire before the task is declared complete:"*. List items 1–5 unchanged. Both the CP-1 advisory carry-forward and the CP-2 D-5 finding are **Closed.** |

### Cross-spec consistency — no regressions

Spot-check: §5.4 / §5.6 Multi-repo paragraphs still cite [session-economy §1 Overview](../20260514-session-economy/architecture.md#L8) + [§3 Background](../20260514-session-economy/architecture.md#L31) + commit `5ce4024` per the shape-(ii) narrative-sourced framing from [amendment 2026-05-18-1](#L217). None of the three (a)-route amendments touched the multi-repo paragraphs; framing intact.

### Exit criteria status (CP-2 — final)

- Divergence list produced: **met** (five advisories at original N=4 audit).
- Routing decision per divergence: **met** (operator-confirmed at audit close; all five routings executed via amendments 2026-05-18-2 through 2026-05-18-6).
- No silent edits: **met** (five amendments + this re-verification all recorded).
- Outcome recorded in journal as closing entry of retroactive-spec adoption: **met** (the [CP-2 closeout (post-amendment)](#L589) block + this re-verification entry together close it).

### Outcome

**CP-2 closed unconditionally.** All three (a)-route spec amendments and both (b)-route SKILL.md amendments have landed; re-verification confirms divergences D-1 / D-2 / D-5 closed in the spec body. The "conditional on brief re-audit landing" qualifier from the [post-amendment CP-2 closeout block](#L589) is now satisfied. §9 CP-2 Status line updated to record post-amendment closure.

### Status implication

§1 banner stays at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to (matches N=1 / N=2 / N=3 precedent: [project-constitution](../20260517-project-constitution-skill/architecture.md#L3), [spec-design](../20260518-spec-design-skill/architecture.md#L3), [spec-write](../20260518-spec-write-skill/architecture.md#L3) all sit at `Draft — Open for Review` post-CP-2 closure). The §9 CP-2 Status line + this re-verification entry carry the closure record. A defined post-Draft state would be a methodology-level decision, not a per-spec one; surfacing the gap here matches the [status-banner-lifecycle finding class non-fire observation](#L367) at CP-2.

### Pattern observation at re-verification close

This entry is the artifact form of the audit's step-2 conditional ("a brief re-audit confirms divergences are closed; can be a short journal entry rather than a full `/spec-review` session"). Shape: table-form walk per finding with section reference + after-state quote + closure verdict, no Phase 1–8 walkthrough. The discriminator from a full re-review (which N=4 used at CP-1 → `/spec-review` after blocker-class amendments — [CP-1 re-review entry](#L260)) is severity class: **blocker-class divergences warrant a full re-review; advisory-class divergences with surgical amendments warrant the short form.** Both shapes now have precedent at N=4. **Pattern for N=5 and N=6:** when a CP-2 audit routes multiple (a)-route spec amendments and the divergences are surgical (not structural), the short-form re-verification (table walk + closure outcome) is sufficient and avoids the overhead of a full `/spec-review` session.

### Next action

Banner advancement deferred (no successor state defined; matches precedent). Resume batch audit at N=5 (`spec-review` retroactive spec CP-2) per [strategy doc](../../docs/retroactive-spec-strategy.md) and [batch journal](../20260518-cp2-batch-audit/journal.md) ordering.

> Update 2026-05-19: the banner advancement deferred above is **resolved** by amendment 2026-05-19-1 (this journal's next entry); the successor state was defined methodology-wide at that amendment.

## 2026-05-19 — Amendment 2026-05-19-1 (cross-skill — post-CP-2 banner advancement)

**Section amended:** [architecture.md:3](./architecture.md#L3) §1 Status banner
**Trigger:** First execution of the post-CP-2 banner transition; methodology-level decision defining `Approved — CP-2 closed YYYY-MM-DD` as the post-`Draft — Open for Review` successor state, applied retroactively across N=1..N=6. Directly resolves the "Banner advancement deferred (no successor state defined)" line at the end of the [CP-2 re-verification entry above](#L640).
**Reason:** Banner advances from `Draft — Open for Review` to `Approved — CP-2 closed 2026-05-18` per the methodology-level decision recorded in the cross-skill anchor. The N=4 re-verification framing (*"§1 banner stays at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to … A defined post-Draft state would be a methodology-level decision, not a per-spec one"*) anticipated exactly this resolution path: a methodology-level decision lands the successor state, then the banner advances. That happens here.
**Impact summary:** No tasks; CP-2 already closed (commit `c116b46` 2026-05-18 20:17:38); no completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-19
**Status implication:** **forward advancement** — first instance in the methodology. Draft → Approved.
**Commit:** `88eda73` (six architecture.md banner edits); `cf50e2e` (cross-skill anchor + 6 paired companion journal entries).

### Full record

See [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) amendment 2026-05-19-1 for the full structured Phase 2 amendment record. This is the **cross-skill companion entry**; the batch journal holds the primary record because the amendment is methodology-level (defines the post-CP-2 successor state across N=1..N=6). Pasting the structured block here would duplicate the durable record.

## 2026-07-05 — Amendment 2026-07-05-1

**Section amended:** [architecture.md](./architecture.md) §4 Architecture → *Execution model* (new subsection "Execution modes and autonomy")
**Trigger:** dispatch-execution design spec ([specs/20260705-dispatch-execution/architecture.md](../20260705-dispatch-execution/architecture.md), Approved, CP-1 closed 2026-07-05) P2.1; folds in the CP-1 advisory to backfill the `AUTONOMY` input undocumented here since it shipped in `6bef6f8` (2026-07-03).
**Reason:** This 2026-05-18 governing spec described only single-lane inline execution with per-task pauses; it predates both the `AUTONOMY` input and the dispatch execution topology. It must describe the two orthogonal execution-mode/autonomy axes and their flipped defaults to stay a faithful contract.
**Impact summary:** Affected tasks: dispatch-execution P2.1 (paired master INPUTS edit follows in same closeout); affected checkpoints: dispatch-execution CP-2 (amendment-set consistency); no completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-07-05
**Status implication:** kept — §1 banner stays `Approved — CP-2 closed 2026-05-18`. Additive amendment backed by the separately-approved dispatch-execution spec; governing-spec ↔ master consistency is gated by dispatch-execution CP-2, not by re-opening this spec's checkpoints.
**Commit:** `630fc5a`

### Full record

**Change.** Inserted a new `### Execution modes and autonomy` subsection into §4 between the "Four explicit pause points" paragraph and `### Where this design plugs in`. The subsection documents: (1) the `AUTONOMY` input (`task` | `checkpoint`) with its designed-stop set and operator-set-only discipline — the backfill; (2) the `EXECUTION` input (`inline` | `dispatch`) as an orthogonal axis governing who implements; (3) the flipped defaults (`EXECUTION: dispatch`, `AUTONOMY: checkpoint`) with the "agent may never loosen the stop set" governing rule, citing the dispatch-execution spec as the sibling design-spec authority for dispatch behavior (as session-economy is for Phase 8).

**Before:** §4 *Execution model* ended at the "Four explicit pause points" paragraph, then proceeded directly to `### Where this design plugs in`. No mention of `AUTONOMY` or `EXECUTION` anywhere in the spec.

**After:** As quoted in the P2.1 amendment presentation (dispatch-execution session, 2026-07-05); see [architecture.md](./architecture.md) §4 "Execution modes and autonomy".

This is a **single-artifact amendment** (the cross-skill four-step mechanics do not apply): the paired master + deploy-copy edits are cross-reference follow-ups under this amendment ID, not parallel amendment records. The dispatch-execution session journal carries the P2.1 task closeout separately.
