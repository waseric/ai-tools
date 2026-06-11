# `spec-review` Skill — Journal

This journal continues the **N≥2 mining structure** established at N=1 ([specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md)), refined at N=2 ([specs/20260518-spec-design-skill/journal.md](../20260518-spec-design-skill/journal.md)), stabilized at N=3 ([specs/20260518-spec-write-skill/journal.md](../20260518-spec-write-skill/journal.md)), and extended at N=4 ([specs/20260518-spec-execute-skill/journal.md](../20260518-spec-execute-skill/journal.md)) with the two-source structure and the per-§5-subsection citation audit. Section headings are stable across retroactive-spec journals; the final remaining session (session 5 of the legacy quintet — `spec-amend` retroactive spec, N=6 in the retroactive-spec sequence) finds the same slots.

This is the **N=5 instance** in the retroactive-spec sequence and **session 4** of the legacy-quintet sequence per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). The strategy doc identifies this slot as the "authoring-time pair to `spec-execute`" — review gates produce verdicts; execute consumes them. Pre-confirmed in the [N=4 journal](../20260518-spec-execute-skill/journal.md) Next action pointer: predecessor at [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446+; sibling design spec at [session-economy §5.4](../20260514-session-economy/architecture.md). Both confirmed in Phase 1 Discovery of this session.

## 2026-05-18 — Retroactive design spec authored

**Status:** draft — awaiting CP-1 review (deferred to fresh session per N=1 / N=2 / N=3 / N=4 precedent)
**Artifact:** [architecture.md](./architecture.md)
**Companion:** [journal.md](./journal.md) (this file)
**Trigger:** Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 4 ordering. Strategy doc pre-resolved audience, verification commitment, batched CP-2, and the N=2 inflection-point deferral; the [N=4 journal](../20260518-spec-execute-skill/journal.md) pre-confirmed the predecessor and sibling-design-spec sources. This session executed against that strategy plus the N=4 two-source structure plus the N=4-amendment-refined audit shape.

### N=4 "Pattern for N=5" callouts — validation outcomes

This is the load-bearing addition to the N=5 journal: each callout from the [N=4 journal](../20260518-spec-execute-skill/journal.md) is recorded as validated, refined, or rejected with reasoning. Future session N=6 reads this table first.

| N=4 callout | Outcome at N=5 | Notes |
|---|---|---|
| **#1 Two-source structure for trilogy-extended skills** (predecessor for rationale + sibling design spec for current behavior) | **Validated; simplest application yet** | `spec-review` has both sources. Predecessor: [shared doc lines 446–663](../../docs/spec-driven-development-prompts-conversation.md) (~218 lines of prompt + design notes — comparable to spec-execute's ~204-line predecessor). Sibling design spec: [session-economy §5.4](../20260514-session-economy/architecture.md), contributing exactly two SKILL.md additions (`SPEC_REPO_ROOT` INPUTS entry + Phase 8 "Multi-repo case" paragraph). Only **shape (i) §5-enumerated** attribution applies; no shape (ii) narrative-sourced mappings exercised. Spec-execute had four mappings (two §5-enumerated + two narrative-sourced); spec-review has **one** (§5-enumerated). The pattern generalizes; this session is the cleanest application. |
| **#2 CP-2 trigger narrative compounds, not just narrows** (enumerate already-passed CP-1s explicitly) | **Validated and extended** | §9 CP-2 trigger now names BOTH N=2's already-passed CP-1, N=3's already-passed CP-1, AND N=4's already-passed CP-1 (after amendment + re-review), narrowing the remaining trigger to "one sibling quintet CP-1 (`spec-amend`) + project-constitution CP-2." Three CP-1s explicitly enumerated. **Pattern for N=6:** four already-passed CP-1s enumerated, remaining condition reduces to "project-constitution CP-2 only" (assuming this spec's CP-1 also passes before N=6). |
| **#3 Most-divergent-shape skill validates family pattern by exception** | **Does not apply at N=5; family template generalizes without strain** | The N=4 prediction noted that the iterative multi-task spec-execute was the divergent shape. spec-review is the **inverse**: single-shot one-checkpoint-one-verdict workflow, the simplest shape in the quintet. The 14-section design-spec template + 11-subsection §5 organization (8 phases + reviewer-shapes + voice + portability) holds without strain. **Pattern for N=6:** `spec-amend` is another single-shot skill (one amendment per session); template expected to generalize the same way. |
| **#4 §13 OQ count varies by skill divergence** (not a quality signal) | **Validated** | N=5 surfaced two real §13 OQs (design-spec checkpoint mechanics gap; amendment-then-re-review cycle artifact tracking). N=4 had two, N=3 had zero, N=2 had one, N=1 had one. OQ count tracks unspecified-phase-interaction count, not session age. Resisted urge to drop one to match prior-session norms. **Pattern for N=6:** OQ count will be whatever it is; faithful surfacing is the signal. |
| **#5 Phase 8's source-attribution model exportable** (name the sibling design spec as pattern-source in §5) | **Validated; pattern stable** | retro §5.8 names session-economy §5.4 as the architectural source for both the Phase 8 multi-repo paragraph and the `SPEC_REPO_ROOT` INPUTS entry. The §5 subsection bears the citation directly; §8 Validation Approach and §14 References repeat it. **Pattern for N=6:** `spec-amend` also has session-economy contributions ([session-economy §5.3](../20260514-session-economy/architecture.md) prescribes `SPEC_REPO_ROOT` + Phase 8 paragraph for spec-amend). The same shape (i) §5-enumerated attribution applies. |
| **Refined #1 — per-§5-subsection audit at authoring time** (§5-enumerated vs narrative-sourced as the two output shapes, concretized in N=4 amendment 2026-05-18-1) | **Validated; only shape (i) needed** | At authoring time, the audit walked retro §5.8 against session-economy §5.4. The §5-enumerated mapping holds cleanly: session-economy §5.4 prescribes two additions (INPUTS entry + Phase 8 paragraph), both present in retro §5.8 + INPUTS contract. No narrative-sourced shape (ii) mappings claimed. The N=4 failure mode (citing session-economy §5.3 / §5.5 as architectural source for spec-execute Phase 4/6, when those subsections are about *other* skills) is structurally impossible here: session-economy §5.4 IS the spec-review subsection. **Pattern for N=6:** spec-amend will face the same simple shape — session-economy §5.3 IS the spec-amend subsection — and the audit shape carries. |
| **/spec-review → /spec-amend → /spec-review re-review cycle** (N=4 amendment observation: viable path for blockers collapsing to a single citation pattern) | **Not exercised at N=5 authoring; precedent stands** | The cycle is not invoked during authoring; it may fire at CP-1 of this spec if blockers surface. Until then, the N=4 precedent (commit `d11c405` + commit `061117e`) stands as the documented pattern. **Pattern for N=6:** carry forward as established. |

### Structural simplification — N=5 is the cleanest two-source application

The N=4 journal predicted spec-review would have a "rich predecessor" (confirmed: 218 lines) and a session-economy contribution. The actual session-economy contribution to spec-review is **two SKILL.md additions in a single session-economy §5 subsection** (§5.4). This produces a structurally simpler retroactive spec than spec-execute's:

- **spec-execute had four cross-spec mappings:** retro §5.1 ↔ session-economy §5.2 (Phase 1 multi-repo detection) **(§5-enumerated)**; retro §5.8 ↔ session-economy §5.1 (Phase 8 token economy) **(§5-enumerated)**; retro §5.4 ↔ session-economy §1 + §3 + commit `5ce4024` (Phase 4 paired-commit strengthening) **(narrative-sourced)**; retro §5.6 ↔ session-economy §1 + §3 + commit `5ce4024` (Phase 6 paired-commit strengthening) **(narrative-sourced)**.
- **spec-review has one cross-spec mapping:** retro §5.8 + INPUTS contract ↔ session-economy §5.4 **(§5-enumerated)**.

This means CP-1 review focus has one cross-spec citation to verify, not four. CP-2 audit has one mapping to walk under shape (i), zero under shape (ii). The N=4 failure mode (citing a session-economy §5 subsection that is actually about a different skill) is structurally impossible at N=5 because session-economy §5.4 IS the spec-review subsection.

**Pattern for N=6.** spec-amend's session-economy contribution is also a single subsection — [session-economy §5.3](../20260514-session-economy/architecture.md) — prescribing two additions (`SPEC_REPO_ROOT` INPUTS entry + Phase 8 multi-repo paragraph). The N=6 retroactive spec will have **one** cross-spec mapping under **shape (i) only**, matching N=5's simplicity. The complex four-mapping case at N=4 was driven by spec-execute being the largest beneficiary of the session-economy commit (Phase 8 token economy + Phase 1 multi-repo detection + Phase 4/6 strengthening = three distinct behavioral additions across the same SKILL.md). spec-review and spec-amend each get one additive cluster from the session-economy commit, not three.

### Source-file selection (decision + rationale)

The explicit table appeared in the session's Phase 1 Discovery Report. Repeated here for journal completeness:

| File | Used? | Rationale |
|---|---|---|
| [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md) | Yes — authoritative for current behavior | The skill itself. 217 lines, 8 phases + REVIEWER NOTES + Design Notes. |
| [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446–663 | Yes — authoritative for design rationale (NOT for current behavior) | Predecessor: `spec-review-prompt.md` artifact (lines 446–645) + "Design notes on the review prompt" (lines 647–663). Cited Inspirational in §14. **N=4 prediction "rich predecessor for spec-review" confirmed** (~218 lines, comparable to spec-execute's ~204). |
| [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) §5.4 | Yes — authoritative for current behavior added after the predecessor | Sibling design spec. Contributes exactly two items to spec-review (INPUTS entry + Phase 8 paragraph). Cited Authoritative in §14. **Simplest two-source application yet** — one §5-enumerated mapping only. |
| [specs/tech-stack.md](../tech-stack.md), [specs/mission.md](../mission.md), [specs/roadmap.md](../roadmap.md) | Yes — authoritative for constraints, audience, lifecycle position | Constitutional bindings. |
| [specs/20260518-spec-execute-skill/architecture.md](../20260518-spec-execute-skill/architecture.md) + [journal.md](../20260518-spec-execute-skill/journal.md) | Yes — N=4 retroactive-spec source | Closest-sibling structural source. "Pattern for N=5" callouts validated/refined/rejected above. |
| [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) + [journal.md](../20260518-spec-write-skill/journal.md) | Yes — N=3 retroactive-spec source | "Pattern for N=4" lineage. N=3 folded its session-economy contribution into evolution-commits framing (different from N=4's two-source structure); spec-write's session-economy contribution was a single OUTPUT FORMAT note, smaller than spec-review's. |
| [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) + [journal.md](../20260518-spec-design-skill/journal.md) | Yes — N=2 retroactive-spec source | Original predecessor-distinction discipline. |
| [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) | Yes — N=1 retroactive-spec source | Original structural source. |
| [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) | Read — orientation only | Cited in §3, §9, §11, §12. Not used as a source for §4/§5 architectural commitments. |
| [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) | Negative signal | Modifies spec-review's `SPEC_PATH` example via commit `4ebec0c` but does not architecturally describe the skill. |
| [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md), [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md), [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md), [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) | Negative signal — pipeline neighbors, not architectural sources | Referenced for handoffs (Phase 7 of spec-execute → here; Phase 8 → spec-amend) but their internal architecture is out of scope for this skill's spec. |

### New "Pattern for N=6" callouts

Candidates for the final-session (N=6, spec-amend) validation. Recorded here, not declared as binding.

1. **Simplest two-source application recurs.** spec-amend's session-economy contribution is exactly one §5 subsection ([session-economy §5.3](../20260514-session-economy/architecture.md)) prescribing two SKILL.md additions (`SPEC_REPO_ROOT` INPUTS entry + Phase 8 multi-repo paragraph). The N=6 retroactive spec will have **one** cross-spec mapping under **shape (i) only**, structurally identical to N=5. **Pattern for N=6:** the four-mapping complexity of N=4 was the outlier, not the norm. spec-review (N=5) and spec-amend (N=6) bracket it on the simpler side; the N=4 prediction that the two-source structure recurs is validated; the **scale** of that recurrence is bounded.

2. **CP-2 trigger narrative compounds further.** N=4 enumerated two already-passed CP-1s (N=2, N=3); N=5 enumerated three (N=2, N=3, N=4). N=6 will enumerate four (N=2, N=3, N=4, N=5), narrowing the remaining trigger to "project-constitution CP-2 only." **Pattern for N=6:** the explicit enumeration continues; collapsing into "all sibling quintet CP-1s" would lose audit clarity.

3. **Family template generalizes to single-shot skills.** N=4 (spec-execute) was the iterative-multi-task outlier; N=5 (spec-review) and N=6 (spec-amend) are single-shot skills. The 14-section design-spec template + 11-subsection §5 organization holds at N=5 without strain. **Pattern for N=6:** spec-amend has 5 phases (per its SKILL.md), fewer than spec-review's 8; expect 8 §5 subsections (5 phases + voice + portability + something amendment-specific like the Amendment Lifecycle if it exists), not 11. Template still holds; phase count differs.

4. **Audit shape stabilizes at "verify one §5-enumerated mapping."** The refined Pattern-for-N=5 #1 (per-§5-subsection audit at authoring time) reduces to a single concrete check in this session: walk retro §5.8 + INPUTS contract against session-economy §5.4. The audit pattern is bounded; the N=4 four-site amendment was an outlier driven by spec-execute's four mappings. **Pattern for N=6:** spec-amend's audit is the same single-check shape; the audit is bounded in size from N=5 onward.

5. **OQ-1 (design-spec checkpoint mechanics) becomes a methodology-wide candidate.** This spec's §13 OQ-1 names a gap that affects every retroactive design spec's CP-1 (six instances of improvisation across N=1/N=2/N=3/N=4/N=4-re-review + project-constitution). **Pattern for N=6:** if spec-amend's CP-1 also encounters the gap (highly likely — it is a design-spec checkpoint), the case for codifying the improvised conventions in SKILL.md strengthens. N=6 may surface a refined OQ or trigger an amendment.

6. **Single-mapping audit case is the simplest CP-1 review focus.** N=4 CP-1's review focus item #5 (session-economy cross-mapping) failed on two of four mappings, producing four blocker findings and an amendment cycle. N=5 CP-1's review focus item for the analogous check has one mapping to verify — the failure mode is bounded to a single citation. **Pattern for N=6:** spec-amend's CP-1 inherits the simpler audit; the re-review cycle is less likely to fire.

### Format choice — design spec vs feature spec

Validated. The shipping skill is `/spec-design`; the operator invoked it for a retroactive design spec on `spec-review`. No friction.

**Pattern (carried from N=1/N=2/N=3/N=4, validated).** Retroactive specs for already-shipping skills use `/spec-design`.

### Naming pattern — directory slug

`specs/20260518-spec-review-skill/architecture.md`. Today is 2026-05-18.

**Pattern (carried from N=2/N=3/N=4, validated).** Authoring date, not strategy-doc-anticipated date. **Four consecutive same-date sessions** (N=2, N=3, N=4, N=5 all dated 2026-05-18) produce four sibling directories with the same date prefix; differentiation by skill-name slug is sufficient.

### Audience framing

Reused verbatim from N=1/N=2/N=3/N=4: "Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set."

**Pattern (carried from N=1/N=2/N=3/N=4, validated).** Audience is reusable verbatim across the legacy quintet.

### Verification commitment level

**Light verification**, per N=1/N=2/N=3/N=4 precedent. The spec text contains no external claims requiring WebFetch — all citations are repo-internal. Per-citation verifications walked at authoring time: predecessor line range (446–663) verified by reading the shared doc; session-economy §5.4 verified by reading the section; tech-stack.md §21-33 / §44 / §48 / §51 verified by reading on heading lines; commit SHAs (`80000b1`, `5ce4024`, `c63e3ba`, `4ebec0c`) verified against `git log`.

**Pattern (carried from N=1/N=2/N=3/N=4, validated).** Light verification is the correct default for the legacy quintet.

### Open-question framing — handling known gaps

§13 reports **two first-class OQs** (design-spec checkpoint mechanics gap; amendment-then-re-review cycle artifact tracking), two moved to §12 (reviewer attribution under AI assistance; cross-spec citation review as Phase 3/Phase 5 check), and none dropped.

**Decision process:** the operator was asked explicitly (Phase 2 `AskUserQuestion` covering four candidates) where each belonged. Operator chose §13, §13, §12, defer-pattern-doc-again in order. The Recommended option was selected in all four cases.

**Pattern (carried from N=2/N=3/N=4, validated).** Triage candidates explicitly to the operator rather than picking placement unilaterally. The Recommended option was the choice in all four candidates — recommendations were tightly scoped and held. **Five consecutive sessions** (N=1's single OQ, N=2's four-candidate triage, N=3's four-candidate triage, N=4's four-candidate triage, N=5's four-candidate triage) have produced the same pattern: Recommended is selected. **Pattern for N=6:** Recommended-option-discipline is a stable convention.

### Drift-audit-as-checkpoint (CP-2)

Both CP-1 (faithfulness) and CP-2 (drift audit) declared in §9.

**Refinement at N=5.** CP-2 trigger narrative compounds further: names N=2's, N=3's, AND N=4's already-passed CP-1s, narrowing the remaining trigger to "one sibling quintet CP-1 (`spec-amend`) + project-constitution CP-2." Also adds the **session-economy-spec cross-consistency check under shape (i) only** as a CP-2 review-focus line item — narrower than N=4's two-shapes check.

**Pattern for N=6.** spec-amend's retro spec will enumerate four already-passed CP-1s (N=2, N=3, N=4, N=5), narrowing the remaining trigger to "project-constitution CP-2 only." The CP-2 review focus's session-economy check will also be shape-(i)-only.

### Scope discipline — what was kept out

§2 Non-goals lists six items explicitly. §12 Out of Scope lists thirteen items, most inherited from N=4 / N=3 / strategy-doc / N=2 / N=1, and two new for N=5:
- (new) Reviewer attribution convention under AI assistance — Candidate C from Phase 2 triage.
- (new) Cross-spec citation review as a Phase 3 / Phase 5 explicit check — Candidate D from Phase 1 discovery; documented as a gap that is patched per-spec, not actionable from inside SKILL.md.

The format-question gap from N=2 §13 OQ-1 is named in §12 (inherited from N=3 / N=4 disposition). The `docs/retroactive-spec-pattern.md` decision is named in §12 — **deferred again at this session's Phase 2** (operator-confirmed), revisited at N=6 close.

**Pattern (carried from N=1/N=2/N=3/N=4, validated).** Retroactive specs are descriptive, not prescriptive. The §12 list grew from N=1's four → N=2's thirteen → N=3's fifteen → N=4's eleven → N=5's thirteen. The size is a function of items inherited from prior sessions plus new items surfaced; not a quality signal.

### Cross-session knowledge transfer

This journal is the canonical N=5 mining input for session 5 (`spec-amend`, N=6). Specifically:

**What this journal commits to:**
- The "Pattern for N=5 — validation outcomes" table above is the structural pattern for N≥3 journals; N=5 confirms the pattern from N=3/N=4 and adds the simplification observation.
- The N=4 prediction "spec-review has a rich predecessor and a session-economy contribution" is fully confirmed.
- The **structural simplification** observation: spec-review is the cleanest two-source application yet; spec-amend will be similarly clean. The N=4 four-mapping complexity was the outlier driven by spec-execute being the largest beneficiary of the session-economy commit.
- The six new "Pattern for N=6" callouts are candidates for the final-session validation.

**What this journal does NOT commit to:**
- A `docs/retroactive-spec-pattern.md`. Decision deferred again at N=5 close per operator confirmation; revisited at N=6 close.
- A binding template for session 5. The journal-mining protocol is the pattern; the two-source structure + structural-simplification observation is the structural addition; neither is a fillable template.
- Resolution of the format-question-prompt gap, the cross-skill amendment coordination gap, or the constitution-amendment ceremony — all out-of-scope items inherited unchanged.

### Friction observed

Honest record of where this session encountered friction. Useful for session 5 to anticipate.

- **N=4's two-source structure narrative dominated the orientation read.** Reading the N=4 journal first (per the strategy-doc session-shape protocol) set the expectation that N=5 would face a similar four-mapping complexity. Discovering at Phase 1 that session-economy §5.4 contains the entire spec-review contribution in one subsection (one INPUTS entry + one Phase 8 paragraph) required adjusting the mental model from "complex two-source structure to validate" to "simplest two-source application in the quintet." The adjustment is recorded explicitly in §2 Goals and §3 Background so future readers don't replay the same expectation.

- **Pattern-for-N=5 #3 (most-divergent-shape skill validates by exception) had to be marked Does-not-apply.** N=4 framed itself as the robustness check on the family template; N=5 (spec-review) is the **inverse** — the simplest shape. The N=4 prediction's framing required reading carefully: "validates by exception" means the divergent case stressed the template; the simple case doesn't stress it back. Recording the outcome as "does not apply" rather than "validates" or "refines" is the honest classification.

- **§13 OQ-1 (design-spec checkpoint mechanics gap) has six instances of improvisation without a documented convention.** The gap has been silently navigated at every retroactive CP-1 to date (project-constitution + N=2 + N=3 + N=4 + N=4 re-review = five; plus this spec's eventual CP-1 will be the sixth or seventh depending on counting). The temptation to either codify the improvisation in SKILL.md (Option a) or to treat it as a known stable convention (Option c) is real; the OQ resists premature commitment and leans toward (c) with (a) as the formalization path. The friction is the **age of the gap** — five-plus improvisations without a documented convention is a long backlog. Recording the lean explicitly as "stable improvisation" rather than "unresolved" is honest reporting.

- **Operator-AI reviewer attribution convention** ("Claude (AI assistant) on behalf of Eric Wasgatt") has appeared at every retroactive CP-1 since N=2, but is not in SKILL.md's `REVIEWER: <name or role>` description. Routing to §12 rather than §13 because it is methodology-wide (every skill's invocation will eventually have the same question), not unique to spec-review. The disposition matches N=2's format-question-prompt OQ. Same risk: a methodology-wide convention question without a clear owner ossifies.

### Conversation grounding

- Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 4 ordering. No prior in-thread conversation; the strategy doc + N=1/N=2/N=3/N=4 specs and journals function as the "extended conversation."
- Phase 1 (Discovery) produced source-file table including three negative-signal rows; landscape orientation against the lifecycle skill family; constraint orientation against four constitutional citations + one sibling-design-spec citation (session-economy §5.4); conversation grounding (strategy doc + N=1/N=2/N=3/N=4 specs and journals as inputs); naming candidates not needed (name fixed by skill name); **predecessor confirmed and scoped** to [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 446–663; **sibling design spec confirmed** at [specs/20260514-session-economy/architecture.md §5.4](../20260514-session-economy/architecture.md).
- Phase 2 (Clarify) surfaced four operator decisions via `AskUserQuestion`: three OQ candidate placements (2 × §13, 1 × §12) + pattern-doc deferral. The Recommended option was chosen in all four. The unilateral decisions (defer CP-1 to fresh session; compounded CP-2 trigger; session-economy spec as Authoritative companion source; §5.8 + INPUTS contract as the single shape-(i) §5-enumerated mapping) were not objected to.
- No `[blocker]` open questions arose. Session proceeded to Phase 3 (spec document + journal authoring).

### Tasks defined

None. Design spec, not feature spec. The "next work" is review (CP-1) and audit (CP-2), declared in §9.

### Next action pointer

Three steps, in order:

1. **Commit** the spec + journal as a paired commit. This is the closing action of session 4.
2. **CP-1 review** in a fresh session: operator invokes `/spec-review` against [architecture.md §9 CP-1](./architecture.md).
3. **Session 5 — `spec-amend` retroactive spec.** N=6 in retroactive-spec sequence; final session of the legacy quintet. The six "Pattern for N=6" callouts above are inputs. Pre-confirmed predecessor scope: the [shared doc](../../docs/spec-driven-development-prompts-conversation.md) has **no** `spec-amend-prompt.md` artifact (spec-amend was added at trilogy commit `80000b1` without a predecessor artifact per the [N=3 journal](../20260518-spec-write-skill/journal.md) prediction). Pre-confirmed sibling design spec: [session-economy §5.3](../20260514-session-economy/architecture.md) (analogous to spec-review's §5.4 — `SPEC_REPO_ROOT` INPUTS entry + Phase 8 multi-repo paragraph). **Structural simplification:** spec-amend will have **one** session-economy §5-enumerated mapping (same as N=5) and **zero** predecessor-rationale source (different from N=2/N=3/N=4/N=5 — N=6 is the first quintet retroactive spec with no predecessor doc to cite Inspirational). The §14 Inspirational categorization will adapt; §3 Background will note the absence explicitly.

No `[blocker]` open questions; the spec is ready for CP-1.

## 2026-05-18 — Review of CP-1

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Tasks reviewed:** N/A (design spec; no atomic Task Breakdown — design-spec adaptation per §5.2/§5.6 applied)
**Blockers:** 0
**Important:** 1 — §5.10 line 247 cites "[N=3 §5.X](../20260518-spec-write-skill/architecture.md)" as a Voice-discipline source; N=3 (spec-write) has no Voice-discipline subsection. `§5.X` is an unresolved placeholder for a section that does not exist. Voice discipline appeared at N=2 §5.6 and N=4 §5.10; N=3 omitted it. The "carried forward verbatim" lineage claim is overstated.
**Advisory:** 3 — (A1) `§5.8 + INPUTS contract` mapping label is consistent though the INPUTS entry itself lives in §4/§6 rather than §5.8 (informational only); (A2) §4 Vocabulary defines `[important]` tag semantics that SKILL.md leaves implicit (faithful gloss, future-SKILL.md-amendment candidate); (A3) SKILL.md Design Notes line 209 says "Amendment Protocol from `spec-execute`" while body line 181 routes to `spec-amend` — internal SKILL.md inconsistency surfaced for the batched quintet CP-2.
**Spec amendments proposed:** Amend §5.10 to either drop the [N=3 §5.X] citation entirely (revising lineage to "N=2 §5.6 and N=4 §5.10; N=3 omitted") or — preferred — to make the gap explicit. Route through `/spec-amend`. Severity proportionally lower than the N=4 amendment cycle (lineage citation, not architectural source citation).
**Next action:** Operator decides whether to apply the §5.10 amendment now (`/spec-amend`) or defer. Either way, CP-1 is closed (`pass with comments`) and the spec exits "Draft — Open for Review". Session 5 (spec-amend retroactive spec, N=6) may proceed per the journal's Next action pointer. CP-2 batched audit awaits one remaining sibling quintet CP-1 (spec-amend) + project-constitution CP-2 per the spec's §9 narrowed trigger.

### Verification trail

Per-claim verification walked at review time:

- SKILL.md structure: 217 lines, 8 numbered phases (lines 44/62/75/88/98/111/117/161), Reviewer Notes (line 195), Design Notes (line 203). Verified.
- Session-economy §5.4 (lines 147–165): prescribes exactly two `spec-review` SKILL.md additions — `SPEC_REPO_ROOT` INPUTS entry and Phase 8 "Multi-repo case" paragraph. Both present in SKILL.md (INPUTS line 24; Phase 8 paragraph line 179). Verified.
- Predecessor doc line range: `## Assistant` at line 446; `### Artifact: spec-review-prompt.md` at line 448; opening 4-backticks at line 450; closing 4-backticks at line 645; `### Design notes on the review prompt` at line 647; last paragraph ends line 663. Split accurate.
- Commit SHAs: `80000b1` (trilogy), `5ce4024` (session-economy), `c63e3ba` (lastUpdated), `4ebec0c` (path convention) — all verified against `git log`.
- Tech-stack.md citations: `#L21-L33` → "Atomic-Skill Portability Principle" heading; `#L44` → "AI context window limits"; `#L48` → "Repository layout"; `#L51` → "Spec-driven development". All four point to heading lines (section-heading citation discipline honored).
- Portability rule: three `~/.claude/skills/...` occurrences in architecture.md (lines 257, 259, 316) are all meta-references inside backticks describing the prohibited pattern. No occurrences as actual link targets.
- §13 OQ-1 and OQ-2: both include all six headed sub-blocks (Question / Analysis-table / Leaning / Owner / Watch items / Anti-goals). Verified.
- Sibling-spec voice-discipline lineage: N=2 §5.6 = "Voice discipline" ✓; N=4 §5.10 = "Voice discipline" ✓; N=3 §5 subsections enumerated (5.1 Discovery, 5.2 Clarify, 5.3 Spec Document, 5.4 Upstream-spec orientation, 5.5 Task Breakdown atomicity, 5.6 Test Strategy as first-class, 5.7 Citation discipline, 5.8 Section template) — **no Voice-discipline subsection**. This is the basis of the [important] finding.

### Pattern observation for N=6

The [important] finding I1 (citation to nonexistent N=3 §5.X) shares structural shape with the N=4 amendment cycle (commits `57be7fa` → `d11c405` → `061117e`): a citation error caught at CP-1, routed to `/spec-amend`, re-reviewed if material. Two retro-spec CP-1s in a row have surfaced citation-discipline findings on **lineage/architectural-source attribution**. Pattern for N=6: the spec-amend retroactive spec should perform an explicit lineage-citation audit at authoring time, walking each "Pattern invoked" reference against the actual cited subsection. The shape of the N=4 amendment plus the present finding suggests this is a recurring failure mode of the cross-spec lineage narrative under the journal-mining pattern; codifying the authoring-time check would head it off at N=6 rather than catching it at CP-1.

## 2026-05-18 — Amendment 2026-05-18-2

**Section amended:** specs/20260518-spec-review-skill/architecture.md §5.10 (Voice discipline, "Pattern invoked" sub-block, line 247)
**Trigger:** CP-1 review verdict (commit `fcb5094`) raised [important] finding I1 — `§5.X` placeholder unresolved AND cited pattern does not exist at N=3.
**Reason:** "Carried forward verbatim" lineage was inaccurate on two counts: unresolved authoring placeholder, and the cited subsection does not exist at N=3 at all. Amended language matches verified facts and surfaces the N=3 gap explicitly.
**Impact summary:** No task or checkpoint scope changed; no completed work invalidated; no cross-references require follow-up.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** No banner change (banner amends at post-CP-2 transition per N=1/N=2/N=3/N=4 convention); §9 CP-1 Status line preserved as the historical record of what CP-1 found. Honors §13 OQ-2 leaning (d) — do not codify re-review/Status-mutation mechanics on one cycle's evidence.
**Commit:** `6efaf51`

### Full record

```
## Amendment 2026-05-18-2 — specs/20260518-spec-review-skill/architecture.md §5.10

**Trigger.** CP-1 review (verdict commit fcb5094, SHA backfill b8a188a)
raised [important] finding I1: the "Pattern invoked" sub-block of §5.10
cites "[N=3 §5.X](../20260518-spec-write-skill/architecture.md)" as a
Voice-discipline source, but verification against N=3's architecture.md
confirms it has no Voice-discipline subsection (N=3 §5 covers Discovery,
Clarify, Spec Document, Upstream-spec orientation, Task Breakdown
atomicity, Test Strategy, Citation discipline, Section template — eight
subsections total, none Voice-discipline). The §5.X placeholder is
unresolved AND the cited pattern does not exist at N=3. Voice discipline
appeared at N=2 §5.6 and N=4 §5.10; N=3 omitted it.

**Section.** §5.10 Voice discipline, "Pattern invoked" sub-block, line 247.

**Change.**

Before:
> **Pattern invoked.** Same voice discipline declared at [N=2 §5.6](../20260518-spec-design-skill/architecture.md), [N=3 §5.X](../20260518-spec-write-skill/architecture.md), and [N=4 §5.10](../20260518-spec-execute-skill/architecture.md). Carried forward verbatim as a spec-side discipline.

After:
> **Pattern invoked.** Voice discipline was declared at [N=2 §5.6](../20260518-spec-design-skill/architecture.md), omitted at [N=3](../20260518-spec-write-skill/architecture.md) (whose §5 covered phases + upstream-spec orientation + atomicity + test strategy + citation discipline + section template, with no Voice-discipline subsection), and reintroduced at [N=4 §5.10](../20260518-spec-execute-skill/architecture.md). Carried forward at N=5 as a spec-side discipline. The gap at N=3 is surfaced explicitly rather than papered over as continuous lineage.

**Reason.** The original "carried forward verbatim" lineage was inaccurate
on two counts: (1) the §5.X placeholder for N=3 was an unresolved authoring
artifact, and (2) the cited pattern does not exist at N=3 at all. The
amended language matches the verified facts and makes the N=3 gap a
recorded observation rather than an implicit (false) continuity claim.
Pairs with the citation-discipline failure mode N=4 amendment 2026-05-18-1
addressed (architectural-source citations) — both are evidence that
cross-spec lineage narration is a recurring failure mode of the
journal-mining authoring pattern.

**Impact.**
- **Affected tasks:** none (design spec; no task breakdown).
- **Affected checkpoints:** CP-1 outcome unchanged (was "pass with
  comments"; remains "pass with comments" — the amendment fixes the one
  [important] finding but the verdict was already in the work-may-proceed
  band). CP-2 unaffected (audits the spec body's faithfulness to SKILL.md;
  this amendment does not change any SKILL.md-mapping commitment).
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none. The CP-1 journal entry
  (already committed) accurately describes the finding; no edit needed
  there. The §9 CP-1 Status line documents the [important] finding by
  reference; rather than rewrite it, the post-amendment state is captured
  in this journal entry and the §9 Status line stands as the contemporary
  record of what CP-1 found.

**Status implication.** No banner change. Spec banner remains
"Draft — Open for Review" (the banner amends at post-CP-2 transition per
N=1/N=2/N=3/N=4 convention; CP-2 is still pending). §9 CP-1 Status line
remains "pass with comments on 2026-05-18" — accurate at the time of the
verdict; the amendment that addressed the [important] is recorded in the
journal entry below, not by mutating the historical Status line. This
honors §13 OQ-2's leaning (d): do not codify re-review/Status-mutation
mechanics on one cycle's evidence; preserve both the original verdict and
the amendment record as separate durable artifacts.

**Approver.** Eric Wasgatt — approved 2026-05-18.
```

### Mining note for N=6

This amendment is the **second consecutive citation-error amendment in the retro-spec series** (N=4 amendment 2026-05-18-1 was the first). Both targeted "Pattern invoked" / architectural-source lineage citations, not SKILL.md mapping commitments. Two cycles is now the evidence base. **Pattern for N=6:** the journal-mining authoring pattern reliably introduces citation errors in cross-spec lineage prose; spec-amend's retroactive spec should perform an authoring-time per-citation walk against actual cited subsections (heading-text and content match, not just heading-line target). If a third consecutive CP-1 surfaces the same failure mode at N=6, codifying the check as a SKILL.md-level discipline (in spec-design or spec-write) becomes warranted; until then, per-spec mitigation in each retro spec's CP-1 review focus suffices.

## 2026-05-18 — Amendment 2026-05-18-3 (cross-skill — paired with spec-amend)

**Section amended:** [specs/20260518-spec-review-skill/architecture.md](./architecture.md) §5.11 (Pattern invoked sub-block)
**Trigger:** Inherited from spec-amend CP-1 verdict (this session) — [important] finding I1 traced N=6 §5.9's citation error to its upstream source, N=5 §5.11 (same incorrect form: "[Strategy-doc Amendment 2026-05-17-1](../../docs/retroactive-spec-strategy.md)"). Operator paired the upstream fix into the same amendment cycle (Q2 of spec-amend amendment 2026-05-18-3).
**Reason:** §5.11 cited `docs/retroactive-spec-strategy.md` as the holder of Amendment 2026-05-17-1; the strategy doc contains no such record. The actual amendment lives in [specs/20260517-project-constitution-skill/journal.md:163](../20260517-project-constitution-skill/journal.md#L163).
**Impact summary:** No tasks; spec-review CP-1 verdict remains "pass with comments" (post-CP-1 citation correction, same shape as the prior amendment 2026-05-18-2 to §5.10). No completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (Draft — Open for Review post-CP-1); §9 Status line "pass with comments on 2026-05-18 by Claude (AI assistant) on behalf of Eric Wasgatt — one [important] citation error in §5.10 (Voice-discipline lineage cites nonexistent N=3 §5.X) proposed for amendment via `/spec-amend`; see [journal](./journal.md) entry of same date. Verdict commit: `fcb5094`." remains as the contemporary record; this amendment is appended below per the N=5 amendment 2026-05-18-2 convention (do not mutate historical verdict text; append new amendment as a separate durable artifact).
**Commit:** `5d1e503` (architecture edit); `c6ba48a` (journal entries)

### Full record

See [specs/20260518-spec-amend-skill/journal.md](../20260518-spec-amend-skill/journal.md) amendment 2026-05-18-3 for the full structured Phase 2 amendment record. This is the **cross-skill companion entry**; the spec-amend journal holds the primary record because spec-amend is where the originating CP-1 verdict was issued. Pasting the structured block here would duplicate the durable record; cross-reference by amendment ID preserves a single source of truth.

### Cross-skill note (first post-trilogy cycle — §13 OQ-4 evidence)

This is the **first post-trilogy-commit cross-skill amendment cycle**. The two pre-trilogy cross-skill changes (`5ce4024`, `4ebec0c`) used a different model (single commit touching multiple skills directly) that the trilogy commit was designed to deprecate. Mechanics applied — recorded in full in the spec-amend journal entry; summarized here so a reader of N=5's journal can navigate without round-tripping:

- Single amendment ID (`2026-05-18-3`) shared across both specs.
- One spec-edit commit (`5d1e503`) touching both `architecture.md` files.
- One journal commit touching both `journal.md` files.
- Primary record in spec-amend journal; companion record (this entry) in spec-review journal.

The pattern doc (post-CP-2) will consume the two amendment entries together as the first worked example of post-trilogy cross-skill amendment mechanics, anchoring §13 OQ-4 to evidence.

## 2026-05-18 — Review of CP-2

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Tasks reviewed:** N/A (design spec; CP-2 is a drift audit of the spec body against the shipping SKILL.md — see [architecture.md §9 CP-2](./architecture.md) review focus.)
**Blockers:** 0
**Important:** 0
**Advisory:** 5 — D-1 SKILL.md Design Notes line 209 stale "Amendment Protocol from `spec-execute`" phrasing (Phase 8 body line 181 correctly routes to `spec-amend`); D-2 SKILL.md uses `[important]` tag (Phase 3:83, Phase 7:132/138) without defining semantics that spec §4 Vocabulary defines; D-3 SKILL.md WND-5 "Do not rubber-stamp based on the journal entry alone" lacks explicit §5/§6 carrier in spec; D-4 SKILL.md preamble line 9 "Third in the trilogy with `spec-write` and `spec-execute`" omits spec-design + spec-amend from pairing list (frontmatter line 4 names all four); D-5 design-spec adaptation gap (already routed via §13 OQ-1 leaning (c)).
**Spec amendments proposed:** One — D-3 amend spec §5.1 Behavior to name the WND-5 rubber-stamp prohibition as an explicit discipline (a single sentence carrying the rule). Route through `/spec-amend`. Severity proportional to a discipline-articulation patch, not an architectural-source citation correction.
**SKILL.md amendments proposed (post-CP-2 batched):** Three — D-1 amend SKILL.md Design Notes line 209 stale citation to point to `spec-amend`; D-2 amend SKILL.md to define `[important]` semantics inline (Phase 3 or ROLE); D-4 amend SKILL.md preamble line 9 to match frontmatter line 4 pairing list (all four siblings).
**Next action:** Operator decides whether to apply the four amendments (one spec, three SKILL.md) now or batch with `spec-amend`'s eventual CP-2 outcome. Either way, CP-2 is closed (`pass with comments`). The retroactive-spec adoption arc for `spec-review` is complete pending operator action on amendments; the spec exits "Draft — Open for Review" at post-CP-2 transition per N=1/N=2/N=3/N=4 convention (banner amendment to follow). Cross-spec layer recorded in [batch journal N=5 entry](../20260518-cp2-batch-audit/journal.md).

### Routing summary

| ID | Summary | Routing |
|---|---|---|
| D-1 | SKILL.md Design Notes line 209 stale: "Amendment Protocol from `spec-execute`" (Phase 8 body line 181 correctly routes to `spec-amend`); internal SKILL.md inconsistency. CP-1 A3 carry-forward. | (b) amend SKILL.md Design Notes |
| D-2 | SKILL.md uses `[important]` tag without defining semantics; spec §4 Vocabulary defines as "middle tag between blocker and advisory: not a spec violation but a quality concern serious enough to warrant attention before the next task." CP-1 A2 carry-forward. | (b) amend SKILL.md — operator (c)→(b) override per N=3/N=4 pattern |
| D-3 | SKILL.md WND-5 "Do not rubber-stamp based on the journal entry alone. The journal is the implementer's claim; the diff is the evidence" lacks explicit §5/§6 carrier; WND-partial-home class 5th data point. | (a) amend spec §5.1 — operator (c)→(a) override per N=3/N=4 pattern |
| D-4 | SKILL.md preamble line 9 "Third in the trilogy with `spec-write` and `spec-execute`" omits spec-design + spec-amend from pairing list; frontmatter line 4 names all four. Internal SKILL.md preamble-vs-frontmatter inconsistency; preamble-vs-body mirror class 5th data point. | (b) amend SKILL.md preamble |
| D-5 | Spec §5.2/§5.6 Design-spec adaptation sub-blocks describe improvised mechanics; SKILL.md silent on phase mechanics for design-spec artifacts. | (c) accept — already routed via §13 OQ-1 leaning (c) |

### Verification trail

Per-claim verification walked at audit time:

- **Session-economy §5.4 shape (i) mapping verified.** Read [specs/20260514-session-economy/architecture.md lines 147–164](../20260514-session-economy/architecture.md#L147-L164) directly. §5.4 prescribes exactly two SKILL.md additions: (1) `SPEC_REPO_ROOT` INPUTS entry; (2) Phase 8 "Multi-repo case" paragraph. Both present in SKILL.md ([line 24](../../.agents/skills/spec-review/SKILL.md#L24) and [line 179](../../.agents/skills/spec-review/SKILL.md#L179) respectively). Wording matches verbatim. Zero shape (ii) claims to verify; zero shape (ii) drift possible.
- **ASPP citation discipline confirmed.** Spec §3 line 51 cites `tech-stack.md §21-33` correctly (heading line for Atomic-Skill Portability Principle). Spec §6 line 269 (Adoptability NFR) cites same. N=1/N=2/N=3/N=4 baseline pattern (correct citation at heading line) holds at N=5.
- **Section-heading citation discipline confirmed.** All §3 tech-stack.md citations (§21-33, §44, §48, §51) point to heading lines per CP-1 verification trail and re-verification at this audit. N=2/N=3/N=4 corrective holds at N=5.
- **Amendment-ID citation correctness post-amendment-2026-05-18-3 confirmed.** §5.11 line 259 now reads `[project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md)` — correct holder. The pre-amendment citation form (strategy-doc holder) has been removed by amendment 2026-05-18-3 commit `5d1e503`. CP-2 verifies no remaining instances of the error class.
- **Phase 8 protocol verified.** SKILL.md Phase 8 (lines 161–182) carries: (1) spec status line update; (2) journal entry with format template; (3) Multi-repo case paragraph; (4) conditional "if amendments proposed" routing through `spec-amend`; (5) conditional "if pass or pass with comments" next-task statement. Spec §5.8 carries identical "two unconditional + two conditional" structure with same content. Match.
- **OPs and WND walked.** All six SKILL.md OPs (lines 35–42) have explicit §5/§6 carriers in spec (verified one-by-one). All eight WND items walked; WND-5 alone lacks explicit §5/§6 carrier (D-3).
- **SKILL.md preamble walked line-by-line against frontmatter description.** Line 4 frontmatter names spec-design, spec-write, spec-execute, spec-amend (four pairings). Line 9 preamble names spec-write, spec-execute (two; omits spec-design and spec-amend; uses stale "trilogy" framing). D-4.
- **SKILL.md Design Notes walked against Phase body.** Line 209 cites "Amendment Protocol from `spec-execute`" — stale pre-trilogy phrasing. Phase 8 body line 181 cites `spec-amend` skill — current correct routing. D-1.
- **`[important]` tag usage walked.** Appears in Phase 3 line 83 (tagging vocabulary), Phase 7 line 132 (count slot), Phase 7 line 138 (findings section). No definition in ROLE, OP, or any phase body. D-2.

### Cross-skill pattern observations (queued for closing summary)

Summarized at audit close; full detail in the batch journal N=5 entry.

- **ASPP citation discipline (N=5 confirmation).** Holds.
- **Session-economy commitment propagation — simplest application validated.** Single shape (i) mapping; zero shape (ii). N=5 is the cleanest two-source application in the quintet so far.
- **Two-source structure — shape (i) only.** Predecessor (lines 446–663) + sibling design spec (session-economy §5.4); §8 carries both cross-check rows. Pattern for N=6 (spec-amend) is the same simple shape with session-economy §5.3.
- **Section-heading citation discipline (N=5 confirmation).** Holds.
- **Amendment-ID citation correctness (N=5 confirmation post-amendment-2026-05-18-3).** Holds — the error introduced at §5.11 was caught at sibling-spec CP-1 (spec-amend), corrected via cross-skill amendment 2026-05-18-3, and verified at CP-2.
- **"WHAT NOT TO DO partial home" finding class — five data points (D-3).** N=1/N=2/N=3/N=4/N=5 consecutive sessions; pattern stable.
- **SKILL.md preamble-vs-body mirror class — five data points (D-4).** N=2/N=3/N=4 (×2)/N=5 consecutive sessions; pattern stable. N=5 D-4 introduces a frontmatter-vs-preamble flavor (similar shape).
- **New finding class: SKILL.md internal stale-citation (D-1).** Design Notes carry pre-trilogy-commit phrasing while Phase body has been updated. First-of-kind for the quintet; worth watching at N=6 — scan spec-amend SKILL.md Design Notes for analogous stale phrasings.
- **Operator (c)→(a/b) override pattern applied at two findings (D-2, D-3).** Both confirmed via AskUserQuestion; both Recommended (b for D-2, a for D-3) selected. N=3/N=4/N=5 — three consecutive sessions where the pattern is load-bearing.
- **Status-banner-lifecycle finding class (N=2 D-1) did NOT fire at N=5.** Consistent with N=4 non-fire. Class remains spec-design-specific.
- **Simplest CP-1 review focus — no re-review cycle at N=5 from CP-1.** Predicted in N=5 journal Pattern-for-N=6 #6 ("single-mapping audit case is the simplest CP-1 review focus"). One [important] (citation error in §5.10) was amendment-2026-05-18-2'd without triggering re-review.

### Pattern observation for N=6 (carried forward to batch journal closing summary)

- Spec-amend's session-economy contribution is also a single subsection ([session-economy §5.3](../20260514-session-economy/architecture.md)) — expect the same simple shape (i) mapping at N=6.
- Spec-amend has no predecessor doc — N=6 will be the first quintet retroactive spec with zero "Inspirational" predecessor source; §3 Background will note the absence explicitly.
- The new D-1 finding class (Design Notes stale-citation) is worth elevating to a first-class CP-2 audit step at N=6: walk Design Notes section against Phase body for stale pre-trilogy phrasing.

## 2026-05-18 — Amendment 2026-05-18-4

**Section amended:** [specs/20260518-spec-review-skill/architecture.md §5.1 Phase 1 — Orient, "Behavior" sub-block (line 113)](./architecture.md#L113)
**Trigger:** N=5 CP-2 audit finding D-3 ([batch journal N=5 entry](../20260518-cp2-batch-audit/journal.md), [spec journal "Review of CP-2"](#L304)) — SKILL.md WND-5 "Do not rubber-stamp based on the journal entry alone. The journal is the implementer's claim; the diff is the evidence" lacks an explicit §5/§6 carrier in the spec body. WND-partial-home class, 5th data point (N=1/N=2/N=3/N=4/N=5).
**Reason:** Carries WND-5's rubber-stamp prohibition into Phase 1's Behavior sub-block as an explicit discipline, closing the partial-home gap that has accumulated five data points across the retro-spec series.
**Impact summary:** No tasks affected; CP-2 D-3 resolved; no completed work invalidated; no cross-reference follow-up.
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at `Draft — Open for Review` (matches N=1/N=2/N=3/N=4 precedent — no defined successor state; closure recorded via §9 CP-2 Status line, not via banner advancement). Explicitly operator-confirmed via AskUserQuestion at Phase 1 of this /spec-amend session, surfacing the divergence between the original invocation's premise ("banner exits Draft") and the N=4 journal's documented precedent ("banner stays at Draft").
**Commit:** `6d41b3b`

### Full record

**Trigger.** N=5 CP-2 audit (commit `5c762a0`) finding D-3 — SKILL.md WND-5 (lines 190–191: "Do not rubber-stamp based on the journal entry alone. The journal is the implementer's claim; the diff is the evidence. Verify against the diff.") was walked against §5/§6 of this spec and found to have no explicit carrier. CP-2 audit logged this as the 5th data point in the WND-partial-home class (N=1/N=2/N=3/N=4/N=5 consecutive sessions). Operator chose route (a) — amend spec to carry the discipline — over reviewer-default (c) — accept as known minor — at audit close per AskUserQuestion.

**Section.** [architecture.md §5.1 Phase 1 — Orient, "Behavior" sub-block (line 113)](./architecture.md#L113). The Behavior paragraph already enumerates the read-order; adding the rubber-stamp prohibition immediately before the "Then the reviewer emits an Orientation Report" closing sentence places the discipline in the cycle where it matters — between consuming the journal (step 4) and constructing the Orientation Report.

**Change.**

Before:
> **Behavior.** The reviewer reads, in order: (1) the spec's §9 Review Checkpoints entry matching `CHECKPOINT_ID`, noting `trigger` / `review focus` / `exit criteria` verbatim; (2) the spec's §7 Task Breakdown entries for each task in `TASK_IDS_IN_SCOPE`, noting scope / acceptance criteria / DoD; (3) the spec's §6 Non-functional Requirements section, noting items relevant to the tasks; (4) the journal entries for those tasks, noting amendments / decisions / surprises / partial-completion flags; (5) the diff in `DIFF_RANGE`, skimmed for shape and scope only. Then the reviewer emits an Orientation Report with quoted checkpoint contract, tasks-in-scope with status, diff shape, journal flags, and initial drift signals.

After:
> **Behavior.** The reviewer reads, in order: (1) the spec's §9 Review Checkpoints entry matching `CHECKPOINT_ID`, noting `trigger` / `review focus` / `exit criteria` verbatim; (2) the spec's §7 Task Breakdown entries for each task in `TASK_IDS_IN_SCOPE`, noting scope / acceptance criteria / DoD; (3) the spec's §6 Non-functional Requirements section, noting items relevant to the tasks; (4) the journal entries for those tasks, noting amendments / decisions / surprises / partial-completion flags; (5) the diff in `DIFF_RANGE`, skimmed for shape and scope only. **The journal is read as the implementer's claim, not as verified evidence; verification is against the diff itself, never the journal narrative alone.** Then the reviewer emits an Orientation Report with quoted checkpoint contract, tasks-in-scope with status, diff shape, journal flags, and initial drift signals.

**Reason.** SKILL.md WND-5 has carried this discipline since the trilogy commit (`80000b1`); the spec body has been silent on it through five retroactive sessions. The CP-2 audit treats this absence as a partial-home gap rather than a faithfulness failure (the SKILL.md commitment is still authoritative; the spec just doesn't echo it), but five data points is enough evidence to close the gap. The carrier sentence is placed at the natural site — Phase 1, where the journal is consumed — and uses the same verbal frame as SKILL.md WND-5 ("the implementer's claim" / "the diff is the evidence") so future drift between the two sites is mechanically detectable.

**Impact.**
- **Affected tasks:** none (retroactive design spec; no §7 Task Breakdown).
- **Affected checkpoints:** [CP-2](./architecture.md#L325) — closing entry references this amendment; D-3 resolved.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none. §5.2 / §5.6 design-spec adaptation paragraphs do not interact with WND-5 (they address Scope and Test/Doc adaptation, not journal-vs-diff evidence weight). §13 OQ-2 (amendment-then-re-review cycle) is structurally orthogonal.

**Status implication.** Kept at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to (matches N=1 / N=2 / N=3 / N=4 precedent — see [N=4 spec-execute journal CP-2 re-verification](../20260518-spec-execute-skill/journal.md): *"§1 banner stays at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to … A defined post-Draft state would be a methodology-level decision, not a per-spec one"*). The §9 CP-2 Status line plus a brief re-verification entry below carry the closure record. Operator-confirmed via AskUserQuestion at Phase 1 of this /spec-amend session — the original invocation premise ("banner exits Draft per N=1–N=4 convention") was the inverse of the actual N=1–N=4 precedent; the divergence was surfaced, the precedent was reconciled, and "keep at Draft" was chosen explicitly.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-5

**Section amended:** [.agents/skills/spec-review/SKILL.md Design Notes — "Drift is a first-class finding" sub-block (line 209)](../../.agents/skills/spec-review/SKILL.md#L209)
**Trigger:** N=5 CP-2 audit finding D-1 ([spec journal "Review of CP-2"](#L304)) — Design Notes line 209 carries pre-trilogy-commit phrasing "Amendment Protocol from `spec-execute`"; Phase 8 body line 181 correctly routes to `spec-amend`. Internal SKILL.md inconsistency between Design Notes (rationale prose) and Phase body (procedural prose). First instance of the new **"SKILL.md internal stale-citation"** finding class (CP-1 advisory A3 → CP-2 D-1 elevation path).
**Reason:** Aligns Design Notes rationale prose with Phase 8 body procedural prose; closes the pre-trilogy phrasing that survived commit `80000b1`'s Phase-body update without a parallel Design Notes refactor.
**Impact summary:** No tasks affected; CP-2 D-1 resolved; no completed work invalidated; no cross-reference follow-up.
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at `Draft — Open for Review` (SKILL.md-only edit; spec commitments unchanged)
**Commit:** `67e0fe0`

### Full record

**Trigger.** N=5 CP-2 audit finding D-1 — Design Notes "Drift is a first-class finding" sub-block (line 209) reads: *"Resolution goes through the Amendment Protocol from `spec-execute`, not through silent acceptance."* The phrasing predates trilogy commit `80000b1` (2026-05-14), which extracted the Amendment Protocol from spec-execute into the new `spec-amend` skill. Phase 8 body line 181 was correctly updated at that commit (*"route them through the `spec-amend` skill"*), but the Design Notes prose was not refactored — a documentation drift from the spec-driven-development-prompts predecessor's pre-trilogy phrasing. The CP-1 audit (commit `fcb5094`) surfaced this as advisory A3; the CP-2 audit (commit `5c762a0`) elevated it to D-1 with route (b) — amend SKILL.md. First-of-kind for the quintet retro-spec series; CP-2 audit logged the new finding class.

**Section.** [.agents/skills/spec-review/SKILL.md Design Notes "Drift is a first-class finding" sub-block (line 209)](../../.agents/skills/spec-review/SKILL.md#L209). Surgical single-sentence edit; surrounding sub-block prose unchanged.

**Change.**

Before:
> **Drift is a first-class finding.** If the code and spec disagree, the review records that as a finding regardless of which side is "right." Resolution goes through the Amendment Protocol from `spec-execute`, not through silent acceptance. This is what keeps the spec accurate over time.

After:
> **Drift is a first-class finding.** If the code and spec disagree, the review records that as a finding regardless of which side is "right." Resolution goes through the `spec-amend` skill, not through silent acceptance. This is what keeps the spec accurate over time.

**Reason.** Phase 8 body line 181 already routes drift resolution to `spec-amend`; frontmatter line 4 names spec-amend as a sibling skill; spec §4 Vocabulary commits to spec-amend routing. Design Notes line 209 was the lone holdout carrying the pre-trilogy phrasing. The amendment removes the inconsistency without rewriting the sub-block's design rationale.

**Impact.**
- **Affected tasks:** none.
- **Affected checkpoints:** CP-2 D-1 resolved.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — Phase 8 body line 181 already routes correctly; frontmatter line 4 already pairs with spec-amend; spec §4 Vocabulary already commits to spec-amend resolution.

**Status implication.** Kept at `Draft — Open for Review`. SKILL.md-only edit; spec commitments unchanged.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-6

**Section amended:** [.agents/skills/spec-review/SKILL.md OPERATING PRINCIPLES §3 (line 39)](../../.agents/skills/spec-review/SKILL.md#L39)
**Trigger:** N=5 CP-2 audit finding D-2 ([spec journal "Review of CP-2"](#L304)) — SKILL.md uses `[important]` tag at Phase 3 line 83, Phase 7 line 132, and Phase 7 line 138, but defines no semantics for it (only `[blocker]` and `[advisory]` are characterized in OP §3). Spec §4 Vocabulary defines `[important]` as "middle tag between blocker and advisory: not a spec violation but a quality concern serious enough to warrant attention before the next task." CP-1 advisory A2 carry-forward. Operator chose route (b) — amend SKILL.md — over reviewer-default (c) at audit close per AskUserQuestion (N=3/N=4/N=5 operator override pattern).
**Reason:** Defines `[important]` semantics in the principle that already governs the blocker/advisory distinction, so all three tag levels live in one place.
**Impact summary:** No tasks affected; CP-2 D-2 resolved; no completed work invalidated; no cross-reference follow-up.
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at `Draft — Open for Review` (SKILL.md-only edit; spec commitments unchanged)
**Commit:** `05e4697`

### Full record

**Trigger.** N=5 CP-2 audit finding D-2 — SKILL.md OP §3 (line 39) characterizes `[blocker]` and `[advisory]` but is silent on `[important]`, even though `[important]` appears in Phase 3 line 83 (as one of four allowable tags), Phase 7 line 132 (as a count slot in the verdict template), and Phase 7 line 138 (as a findings section in the verdict template). Spec §4 Vocabulary defines `[important]` as: *"a middle tag between blocker and advisory: not a spec violation but a quality concern serious enough to warrant attention before the next task. Tagged `[important]`. Surfaced as comments; does not block the verdict."* CP-1 surfaced this as advisory A2 (*"§4 Vocabulary defines `[important]` tag semantics that SKILL.md leaves implicit (faithful gloss, future-SKILL.md-amendment candidate)"*); CP-2 elevated it to D-2 with the reviewer's default routing of (c) — accept as known minor — but the operator overrode to (b) — amend SKILL.md — per the N=3/N=4/N=5 (c)→(a/b) override pattern.

**Section.** [.agents/skills/spec-review/SKILL.md OPERATING PRINCIPLES §3 (line 39)](../../.agents/skills/spec-review/SKILL.md#L39). Placement chosen at the natural site where the blocker/advisory distinction is already defined, so the three-tag vocabulary lives in one place rather than scattered across Phase 3 (where the tag is first used) and Phase 7 (where it appears as a count slot). Phase 3 / Phase 7 placement would have produced a second source-of-truth for the definition; OP §3 placement keeps the principle and the vocabulary co-located.

**Change.**

Before:
> 3. **Compliance vs. advisory.** A finding is `[blocker]` only if it violates the spec, the Definition of Done, or the checkpoint's exit criteria. Style preferences, refactoring ideas, and "I would have done it differently" are `[advisory]`.

After:
> 3. **Compliance vs. advisory.** A finding is `[blocker]` only if it violates the spec, the Definition of Done, or the checkpoint's exit criteria. Style preferences, refactoring ideas, and "I would have done it differently" are `[advisory]`. The middle tag `[important]` denotes a quality concern that is not a spec violation but is serious enough to warrant attention before the next task — surfaced as a comment, does not block the verdict, and exists to distinguish "you should fix this before moving on" from "I'd have done it differently."

**Reason.** The added sentence tracks spec §4 Vocabulary's load-bearing clause ("middle tag … not a spec violation but a quality concern serious enough to warrant attention before the next task") verbatim, with a phrasing extension that names the operational distinction ("you should fix this before moving on" vs. "I'd have done it differently"). Aligning SKILL.md OP §3 with spec §4 Vocabulary closes the implicit-definition gap; future readers of SKILL.md no longer need to cross-reference the spec to know what `[important]` means.

**Impact.**
- **Affected tasks:** none.
- **Affected checkpoints:** CP-2 D-2 resolved.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — Phase 3 line 83 / Phase 7 lines 132, 138 already use the tag; spec §4 Vocabulary already defines it identically.

**Status implication.** Kept at `Draft — Open for Review`. SKILL.md-only edit; spec commitments unchanged.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-7

**Section amended:** [.agents/skills/spec-review/SKILL.md preamble — paragraph following "# Spec Review" (line 9)](../../.agents/skills/spec-review/SKILL.md#L9)
**Trigger:** N=5 CP-2 audit finding D-4 ([spec journal "Review of CP-2"](#L304)) — preamble line 9 reads "Third in the trilogy with `spec-write` and `spec-execute`", omitting spec-design + spec-amend; frontmatter description line 4 names all four siblings. Preamble-vs-body mirror class, 5th data point (N=2/N=3/N=4 ×2/N=5).
**Reason:** Replaces stale pre-`spec-design`-lift, pre-`spec-amend`-extraction "trilogy" framing with the four-sibling pairing list that matches frontmatter line 4 verbatim.
**Impact summary:** No tasks affected; CP-2 D-4 resolved; no completed work invalidated; no cross-reference follow-up.
**Approver:** Eric Wasgatt (operator)
**Approved on:** 2026-05-18
**Status implication:** kept at `Draft — Open for Review` (SKILL.md-only edit; spec commitments unchanged)
**Commit:** `e60a17b`

### Full record

**Trigger.** N=5 CP-2 audit finding D-4 — SKILL.md preamble line 9 carries pre-trilogy-commit framing (*"Third in the trilogy with `spec-write` and `spec-execute`"*), omitting both `spec-design` (lifted to first-class in commit `80000b1`, 2026-05-14) and `spec-amend` (extracted from spec-execute in the same commit). The frontmatter description (line 4) names all four siblings explicitly: *"Pairs with `spec-design` and `spec-write` (which declare checkpoints), `spec-execute` (which produces the work being reviewed), and `spec-amend` (which applies spec changes when the review surfaces them)."* Preamble-vs-body mirror class, 5th consecutive data point (N=2 D-1 / N=3 D-5 / N=4 D-3 / N=4 D-4 / N=5 D-4).

**Section.** [.agents/skills/spec-review/SKILL.md preamble — paragraph following "# Spec Review" (line 9)](../../.agents/skills/spec-review/SKILL.md#L9). Same shape as spec-execute amendment [2026-05-18-5](../20260518-spec-execute-skill/journal.md) (commit `988818d`): replace trilogy-framing clause with the four-sibling pairing list reusing frontmatter line 4 phrasing verbatim.

**Change.**

Before:
> Third in the trilogy with `spec-write` and `spec-execute`. Use when a Review Checkpoint defined in a spec has been triggered. Walks the diff against the checkpoint's declared review focus and exit criteria, produces a structured verdict, and records the outcome back into the spec and journal.

After:
> Use when a Review Checkpoint defined in a spec has been triggered. Pairs with `spec-design` and `spec-write` (which declare checkpoints), `spec-execute` (which produces the work being reviewed), and `spec-amend` (which applies spec changes when the review surfaces them). Walks the diff against the checkpoint's declared review focus and exit criteria, produces a structured verdict, and records the outcome back into the spec and journal.

**Reason.** Reuses frontmatter line 4 phrasing verbatim so the two sites cannot drift independently. Closes the preamble-vs-frontmatter inconsistency that has accumulated five data points across the retro-spec series — the load-bearing observation is that consecutive sessions surface the same finding class, so codifying the four-sibling pairing as the canonical preamble phrasing (rather than the pre-trilogy "Nth in the trilogy" framing) keeps the convention stable through N=6.

**Impact.**
- **Affected tasks:** none.
- **Affected checkpoints:** CP-2 D-4 resolved.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none — frontmatter line 4 already pairs with all four siblings; spec §3 Dependencies already commits to the quintet pairing.

**Status implication.** Kept at `Draft — Open for Review`. SKILL.md-only edit; spec commitments unchanged.

**Approver.** Eric Wasgatt (operator), 2026-05-18.

### Pattern observation at four-amendment close

This /spec-amend session bundled four amendments (one spec edit, three SKILL.md edits) into a single invocation, paired against the N=5 CP-2 audit (commit `5c762a0`). Differs from N=4's two-invocation split (one (a)-route session, one (b)-route session): N=5's smaller (a)-route count (one amendment vs. N=4's three) and the operator's framing of the four CP-2 amendments as "post-CP-2 closeout artifacts" supported the bundle. The invocation-shape discriminator from [N=4 pattern observation](../20260518-spec-execute-skill/journal.md) — *"the invocation-split discriminator is target artifact, not amendment count"* — generalizes: when the (a)-route count is small (one amendment) the split is unnecessary because there is no Orientation-Report coherence cost to bundling target artifacts within one invocation. **Pattern for N=6:** if spec-amend's CP-2 produces a mix of (a) and (b) routes with small (a) count, bundling is acceptable; if (a) count is ≥2 (matching N=4), splitting preserves Orientation coherence.

### Pattern observation — divergent-framing surfacing at Phase 1

The operator's invocation framed the four amendments as preceding a banner exit from `Draft — Open for Review` ("per N=1/N=2/N=3/N=4 convention"). N=4's CP-2 re-verification journal explicitly recorded the inverse precedent (*"§1 banner stays at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to"*). The /spec-amend skill's discipline of surfacing status implications explicitly (per [feedback-spec-amend-status-implication memory](file:///Users/eric/.claude/projects/-Users-eric-scm-github-waseric-ai-tools/memory/feedback-spec-amend-status-implication.md), recorded 2026-05-17) caught the divergence at Phase 1; the operator confirmed via AskUserQuestion that the banner stays at Draft. **Pattern for N=6:** the feedback-memory-driven Phase 1 surfacing discipline is load-bearing — without it, the original framing would have produced a silent banner advancement contrary to N=1–N=4 precedent. Worth carrying forward to spec-amend's eventual amendment cycle.

## 2026-05-18 — CP-2 re-verification (post-amendment)

Brief post-amendment re-audit of the (a)-route §5.1 carrier sentence and the three (b)-route SKILL.md sites, per the N=4 CP-2 audit step-2 conditional adapted to N=5 ([batch journal](../20260518-cp2-batch-audit/journal.md) cross-spec norm: short journal entry, not full /spec-review). Confirms all four routed amendments closed. CP-2 §9 Status line updated to "Checkpoint closed." Banner held at `Draft — Open for Review` per N=1/N=2/N=3/N=4 precedent (no defined successor state; matches the [N=4 CP-2 re-verification framing](../20260518-spec-execute-skill/journal.md): *"§1 banner stays at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to … A defined post-Draft state would be a methodology-level decision, not a per-spec one"*).

### Re-verification walk

| Divergence | Routing | Verification | Status |
|---|---|---|---|
| D-1 (SKILL.md Design Notes stale "Amendment Protocol from `spec-execute`") | (b) amend SKILL.md | [SKILL.md line 209](../../.agents/skills/spec-review/SKILL.md#L209) now reads "Resolution goes through the `spec-amend` skill, not through silent acceptance." Phase 8 body line 181 unchanged. Frontmatter line 4 unchanged. Internal consistency restored. | Closed via [amendment 2026-05-18-5](#L406) commit `67e0fe0`. |
| D-2 (SKILL.md uses `[important]` tag without defining semantics) | (b) amend SKILL.md | [SKILL.md OP §3 (line 39)](../../.agents/skills/spec-review/SKILL.md#L39) now defines `[important]` as middle tag with the spec §4 Vocabulary's load-bearing clause carried verbatim. Phase 3 line 83 / Phase 7 lines 132, 138 usages unchanged — they already reference the tag; the definition now lives in OP §3. | Closed via [amendment 2026-05-18-6](#L438) commit `05e4697`. |
| D-3 (SKILL.md WND-5 rubber-stamp prohibition lacks §5/§6 carrier) | (a) amend spec §5.1 | [architecture.md §5.1 Behavior sub-block (line 113)](./architecture.md#L113) now carries: *"The journal is read as the implementer's claim, not as verified evidence; verification is against the diff itself, never the journal narrative alone."* SKILL.md WND-5 unchanged. Carrier discipline restored. | Closed via [amendment 2026-05-18-4](#L376) commit `6d41b3b`. |
| D-4 (SKILL.md preamble "Third in the trilogy" omits spec-design + spec-amend) | (b) amend SKILL.md | [SKILL.md preamble line 9](../../.agents/skills/spec-review/SKILL.md#L9) now reads: *"Pairs with `spec-design` and `spec-write` (which declare checkpoints), `spec-execute` (which produces the work being reviewed), and `spec-amend` (which applies spec changes when the review surfaces them)."* Matches frontmatter line 4 verbatim. Preamble-vs-frontmatter consistency restored. | Closed via [amendment 2026-05-18-7](#L475) commit `e60a17b`. |
| D-5 (design-spec adaptation gap) | (c) accept | Already routed via §13 OQ-1 leaning (c) — design-spec adaptation noted in §5.2 / §5.6; SKILL.md silent; watch items capture revisit conditions. No artifact change required. | Closed (accepted) — no amendment. |

### Cross-spec consistency — no regressions

Verified that the four amendments produced no new drift against the [session-economy spec §5.4](../20260514-session-economy/architecture.md) (shape (i) §5-enumerated mapping for SPEC_REPO_ROOT + Phase 8 multi-repo paragraph). None of the four amendments touched §5.8 Phase 8 Update Artifacts or the SPEC_REPO_ROOT INPUTS contract; the cross-spec mapping holds unchanged.

Verified that the spec §4 Vocabulary entry for `[important]` and SKILL.md OP §3's new definition agree on the load-bearing clause ("middle tag between blocker and advisory: not a spec violation but a quality concern serious enough to warrant attention before the next task"). The SKILL.md addition extends with an operational phrasing distinction ("you should fix this before moving on" vs. "I'd have done it differently") that does not commit beyond the spec; if a future audit treats the extension as additional commitment, route to a fresh amendment.

### Exit criteria status (CP-2 — final)

- **Divergence list produced:** met (five divergences D-1 through D-5 in [CP-2 audit journal entry](#L304)).
- **Routing decisions recorded:** met (one (a), three (b), one (c); operator-confirmed via AskUserQuestion at audit close).
- **No silent edits:** met (all four amendments routed through /spec-amend with structured Phase 2 records + operator approval; D-5 routed (c) with rationale).
- **Outcome recorded as closing entry of retroactive-spec adoption:** met (this re-verification entry + the §9 Status line update + the four amendment entries above).

### Outcome

**Checkpoint closed.** All four routed amendments verified at re-audit. D-5 closed (accepted) without artifact change. The retroactive-spec adoption arc for `spec-review` is complete. N=6 (spec-amend CP-2) is the only remaining batch step before the [batch journal](../20260518-cp2-batch-audit/journal.md) closing summary.

### Status implication

§1 banner stays at `Draft — Open for Review`. The spec lifecycle has no defined successor state to advance to (matches [N=1](../20260517-project-constitution-skill/architecture.md#L3), [N=2](../20260518-spec-design-skill/architecture.md#L3), [N=3](../20260518-spec-write-skill/architecture.md#L3), and [N=4](../20260518-spec-execute-skill/architecture.md#L3) precedent — all sit at `Draft — Open for Review` post-CP-2 closure). The §9 CP-2 Status line plus this re-verification entry carry the closure record. A defined post-Draft state remains a methodology-level open question, not a per-spec decision; the gap is surfaced here for cross-session continuity but not resolved.

### Pattern observation at re-verification close

The CP-2 closeout shape matches N=4 (commit `c116b46`) exactly: §9 Status line updated to list all routed amendment IDs + "Checkpoint closed", brief re-verification journal entry (this entry) with per-divergence Closed status, banner held at Draft with explicit precedent citation. **Pattern for N=6:** the closeout shape is stable from N=4 onward. Spec-amend's CP-2 closeout should reuse the same template with its own amendment-ID list. The banner-stays-at-Draft framing is now N=1/N=2/N=3/N=4/N=5 consecutive precedent — five data points are sufficient to treat the "no defined successor state" disposition as the methodology default until a future explicit decision changes it.

### Pattern observation for N=6 (carried forward to batch journal closing summary)

- Spec-amend's CP-2 closeout reuses this template: per-divergence Closed table, cross-spec no-regressions check (session-economy §5.3 for spec-amend), exit-criteria-status, "Checkpoint closed", banner-held-at-Draft framing.
- N=5 single-bundle /spec-amend invocation precedent (one (a)-route + three (b)-route amendments) generalizes to "bundling acceptable when (a) count is small"; N=4 two-invocation split precedent applies when (a) count is ≥2.
- The Phase 1 status-implication surfacing discipline ([feedback-spec-amend-status-implication](file:///Users/eric/.claude/projects/-Users-eric-scm-github-waseric-ai-tools/memory/feedback-spec-amend-status-implication.md)) is load-bearing and should be re-applied at N=6 even when the answer feels obvious — the surfacing is the point.

### Next action

Resume batch audit at N=6 (`spec-amend` retroactive spec CP-2) per the [batch journal](../20260518-cp2-batch-audit/journal.md) ordering. Closing summary follows N=6 CP-2 closure.

## 2026-05-19 — Amendment 2026-05-19-1 (cross-skill — post-CP-2 banner advancement)

**Section amended:** [architecture.md:3](./architecture.md#L3) §1 Status banner
**Trigger:** First execution of the post-CP-2 banner transition; methodology-level decision defining `Approved — CP-2 closed YYYY-MM-DD` as the post-`Draft — Open for Review` successor state, applied retroactively across N=1..N=6. The feedback-memory-driven Phase 1 surfacing discipline noted in N=5 amendment 2026-05-18-4 ([line 516](#L516)) was re-applied at this amendment's Phase 2 — the operator was shown both competing precedents (banner-amendment-to-follow promise vs. banner-stays-at-Draft re-verifications) along with the proposed forward advancement, and explicitly confirmed the methodology-level decision.
**Reason:** Banner advances from `Draft — Open for Review` to `Approved — CP-2 closed 2026-05-18` per the methodology-level decision recorded in the cross-skill anchor.
**Impact summary:** No tasks; CP-2 already closed (commit `741fd96` 2026-05-18 20:48:27); no completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-19
**Status implication:** **forward advancement** — first instance in the methodology. Draft → Approved.
**Commit:** `88eda73` (six architecture.md banner edits); `cf50e2e` (cross-skill anchor + 6 paired companion journal entries).

### Full record

See [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) amendment 2026-05-19-1 for the full structured Phase 2 amendment record. This is the **cross-skill companion entry**; the batch journal holds the primary record because the amendment is methodology-level (defines the post-CP-2 successor state across N=1..N=6). Pasting the structured block here would duplicate the durable record.

