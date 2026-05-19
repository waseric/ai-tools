# `spec-amend` Skill — Journal

This journal continues the **N≥2 mining structure** established at N=1 ([specs/20260517-project-constitution-skill/journal.md](../20260517-project-constitution-skill/journal.md)), refined at N=2 ([specs/20260518-spec-design-skill/journal.md](../20260518-spec-design-skill/journal.md)), stabilized at N=3 ([specs/20260518-spec-write-skill/journal.md](../20260518-spec-write-skill/journal.md)), extended at N=4 ([specs/20260518-spec-execute-skill/journal.md](../20260518-spec-execute-skill/journal.md)) with the two-source structure and the per-§5-subsection citation audit, and refined at N=5 ([specs/20260518-spec-review-skill/journal.md](../20260518-spec-review-skill/journal.md)) as the simplest two-source application plus the authoring-time per-citation walk observation. Section headings are stable across retroactive-spec journals.

This is the **N=6 instance** in the retroactive-spec sequence and the **final (session 5) of the legacy-quintet sequence** per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). The strategy doc identifies this slot as "Closes the loop. Smallest cognitive lift since amendments are the simplest workflow; benefits most from accumulated journal mining." Pre-confirmed in the [N=5 journal](../20260518-spec-review-skill/journal.md) Next action pointer: predecessor scope — the [shared doc](../../docs/spec-driven-development-prompts-conversation.md) has **no standalone `spec-amend-prompt.md` artifact** (spec-amend was added at trilogy commit `49c15f0` by extracting the inline AMENDMENT PROTOCOL from spec-execute-prompt.md); sibling design spec at [session-economy §5.3](../20260514-session-economy/architecture.md). The N=5 prediction "**zero** predecessor-rationale source" was **refined** during this session's Phase 1 Discovery: the predecessor exists but is **inline rather than standalone** — the AMENDMENT PROTOCOL block at lines 391–403 plus the design-note paragraph at line 414, both embedded inside the `spec-execute-prompt.md` artifact.

## 2026-05-18 — Retroactive design spec authored

**Status:** draft — awaiting CP-1 review (deferred to fresh session per N=1 / N=2 / N=3 / N=4 / N=5 precedent)
**Artifact:** [architecture.md](./architecture.md)
**Companion:** [journal.md](./journal.md) (this file)
**Trigger:** Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 5 ordering (final session of the legacy quintet). Strategy doc pre-resolved audience, verification commitment, batched CP-2, and the N=2 inflection-point deferral; the [N=5 journal](../20260518-spec-review-skill/journal.md) pre-confirmed the sibling-design-spec source (session-economy §5.3) and predicted zero predecessor. This session executed against that strategy plus the N=4 two-source structure plus the N=5 authoring-time per-citation walk discipline, refining the "zero predecessor" prediction to "inline predecessor."

### N=5 "Pattern for N=6" callouts — validation outcomes

This is the load-bearing addition to the N=6 journal: each callout from the [N=5 journal](../20260518-spec-review-skill/journal.md) is recorded as validated, refined, or rejected with reasoning. **This is the final retroactive spec in the quintet; no "Pattern for N=7" callouts are declared.** The pattern-mining loop closes here and hands off to `docs/retroactive-spec-pattern.md` (to be authored post-CP-2 batch audit).

| N=5 callout | Outcome at N=6 | Notes |
|---|---|---|
| **#1 Simplest two-source application recurs** (one §5-enumerated mapping, two SKILL.md additions) | **Validated structurally; refined in scale** | Shape (i) §5-enumerated only — validated. The N=5 prediction was specifically "**two** SKILL.md additions"; verification against [session-economy §5.3](../20260514-session-economy/architecture.md) at authoring time showed **three** additions (INPUTS entry + Phase 4 paragraph + **Phase 5 journal-commit note**). The third addition is small (one paragraph) but real. spec-amend is **one larger than N=5** but still smaller than N=4's four mappings. The "structural simplicity" holds; the "scale prediction" needed refinement. Spec records the refinement at §2 Goals and §3 Background. |
| **#2 CP-2 trigger narrative compounds further** (enumerate 4 already-passed CP-1s) | **Validated** | §9 CP-2 trigger now names N=2's, N=3's, N=4's, AND N=5's already-passed CP-1s, narrowing the remaining trigger to "project-constitution CP-2 only." Four CP-1s explicitly enumerated. The compounding narrative arrived at its terminal form at N=6 — there is no N=7 to extend it. |
| **#3 Family template generalizes to single-shot skills** (8 §5 subsections expected, not 11) | **Validated with adjustment** | spec-amend has 6 phases (per SKILL.md). §5 has 9 subsections (6 phase subsections + change-classification + voice + portability). The N=5 prediction was "8 (5 phases + voice + portability + something amendment-specific like the Amendment Lifecycle if it exists)" — close. The "something amendment-specific" turned out to be Change Classification (§5.7), which the N=5 prediction noted as a possibility. Phase count: 6, not 5 as N=5 noted. Template still holds; phase count differs. |
| **#4 Audit shape stabilizes at single-§5-enumerated mapping** | **Validated** | One cross-spec mapping to verify under shape (i); zero under shape (ii). N=5 had one mapping; N=6 has one mapping (covering three additions). The audit is bounded in size from N=5 onward; N=6 confirms the bound holds. |
| **#5 OQ-1 (design-spec checkpoint mechanics) strengthens as methodology-wide candidate** | **Not exercised at N=6 authoring; will surface (or not) at CP-1** | This spec's CP-1 will be the next design-spec checkpoint. If the design-spec adaptation improvisation surfaces again, the case strengthens. Until CP-1 fires, the callout is recorded as "carried, awaiting CP-1 evidence." |
| **#6 Single-mapping audit is the simplest CP-1 review focus** | **Provisionally validated** | This spec's CP-1 review focus has one cross-spec mapping to verify (under shape (i) only, covering three additions). Re-review cycle less likely to fire than at N=4 (which had four mappings, two of which failed). N=5's CP-1 surfaced one [important] citation finding (Voice-discipline lineage) → amendment 2026-05-18-2; N=6's CP-1 may or may not surface findings of comparable severity. |
| **Post-CP-1 pattern observation: authoring-time per-citation walk** | **Adopted at N=6 authoring** | Every "Pattern invoked" citation in §5 was walked against the actual cited subsection at authoring time (heading text + content match, not just heading line target). Two prior CP-1 cycles surfaced citation errors as [important] findings (N=4 amendment 2026-05-18-1, N=5 amendment 2026-05-18-2); the discipline aims to head off the third instance at authoring time. Per-citation audit results recorded in the next sub-section of this journal. |

### Pre-commit per-citation walk — N=5 pattern observation applied

Per [N=5 journal §"Pattern observation for N=6"](../20260518-spec-review-skill/journal.md), every "Pattern invoked" citation and every cross-§ link in §5 was walked at authoring time. Results:

| §5 subsection | Citation walked | Outcome |
|---|---|---|
| §5.1 Phase 1 Orient | spec-review §5.1 + spec-execute §5.1 (both "Phase 1 Orient"); SKILL.md Phase 1 | Both sibling §5.1 subsections exist and are titled "Phase 1 — Orient" (verified). SKILL.md Phase 1 at line 53–68 verified. |
| §5.2 Phase 2 Draft | spec-review §5.7 ("Phase 7 — Verdict") for structured-artifact pattern; SKILL.md Phase 2 lines 74–100 | spec-review §5.7 verified as "Phase 7 — Verdict" with structured-format discussion. SKILL.md Phase 2 verified at lines 74–100. |
| §5.3 Phase 3 Approval | spec-execute §5.7 ("Phase 7 — Checkpoint gate"); SKILL.md Phase 3 | spec-execute §5.7 verified as "Phase 7 — Checkpoint gate" (verified to exist; explicit-approval discipline matches). SKILL.md Phase 3 lines 102–111 verified. Capitalization corrected post-CP-1 via amendment 2026-05-18-3. |
| §5.4 Phase 4 Apply | session-economy §5.3 (Phase 4 paragraph); SKILL.md Phase 4 + Multi-repo block | session-economy §5.3 verified at lines 123–145; Phase 4 paragraph at line 141 verified verbatim. SKILL.md Phase 4 at lines 113–130; Multi-repo block at line 130 verified. |
| §5.5 Phase 5 Journal | session-economy §5.3 (Phase 5 note); SKILL.md Phase 5 lines 136–149 + multi-repo at 152 | session-economy §5.3 Phase 5 note verified at lines 143–145 verbatim. SKILL.md Phase 5 + multi-repo verified. |
| §5.6 Phase 6 Downstream Handoff | spec-execute §5.6; spec-review §5.8; SKILL.md Phase 6 lines 156–164 | **Citation error caught at walk:** original draft cited spec-execute §5.6 as "Phase 6 — Closeout" — verification against the actual heading showed it is **"Phase 6 — Update artifacts"** (closeout is the *purpose*; "Update artifacts" is the heading). Same correction for spec-review §5.8: actual heading is "Phase 8 — Update artifacts" (not "Update Artifacts"). Functional characterization (end-of-flow phase naming next actor) verified: spec-execute §5.6 Behavior point 3 = "Next-task pointer"; spec-review §5.8 conditional update 4 = "state the next task ID per the dependency graph." Spec body corrected before commit. **This is the per-citation walk discipline working as designed — catching the error at authoring time rather than at CP-1.** SKILL.md Phase 6 lines 156–164 verified. |
| §5.7 Change Classification | SKILL.md ROLE block lines 37–41 | Verified: lines 37–41 contain the trichotomy (amendment / in-flight edit / rewrite). |
| §5.8 Voice discipline | N=2 §5.6; N=3 (omitted); N=4 §5.10; N=5 §5.10 + amendment 2026-05-18-2 commit `85821ca` | All verified at authoring time. N=2 §5.6 = "Voice discipline" ✓; N=3 §5 has no Voice-discipline subsection ✓; N=4 §5.10 = "Voice discipline" ✓; N=5 §5.10 = "Voice discipline" (post-amendment) ✓; commit `85821ca` is the amendment commit ✓. |
| §5.9 Portability rule for links | project-constitution-skill Amendment 2026-05-17-1; .agents/skills/spec-amend/SKILL.md path | Citation file corrected post-CP-1 via amendment 2026-05-18-3 (original draft cited the strategy doc; actual amendment lives in project-constitution-skill journal). SKILL.md path verified at authoring time. |

**Outcome.** **One citation error caught and fixed at authoring time** (the §5.6 "Phase 6 — Closeout" → "Phase 6 — Update artifacts" correction, plus the sibling spec-review §5.8 capitalization). Spec body corrected before commit; both the spec and this journal table now reflect the verified-correct citation. The discipline applied at N=6 worked as designed — the same failure mode that surfaced at N=4 CP-1 (amendment 2026-05-18-1) and N=5 CP-1 (amendment 2026-05-18-2) was caught one inflection point earlier, at authoring time, **before** commit. If CP-1 surfaces additional citation findings regardless, the discipline needs refinement; if CP-1 finds zero or only-non-citation findings, the discipline is validated as effective at the authoring-time inflection point. Either outcome is data for the pattern doc.

### Structural simplification — N=6 mirrors N=5, with refinement to scale

The N=5 journal predicted spec-amend would have a "**zero** predecessor-rationale source" and a "single §5-enumerated mapping with two SKILL.md additions." The actual session-economy contribution to spec-amend is **three SKILL.md additions in a single session-economy §5 subsection** (§5.3), and the predecessor exists **inline** (not standalone) at lines 391–403 + line 414 of the shared doc.

- **Predecessor refinement:** zero → inline. The N=5 prediction missed that the trilogy commit `49c15f0` *extracted* the AMENDMENT PROTOCOL from inside spec-execute-prompt.md into a standalone skill; the predecessor block exists but is structurally distinct from N=2/N=3/N=4/N=5 (each of which had a standalone `*-prompt.md` artifact). Cite Inspirational with explicit inline-extraction story per Phase 2 confirmation.
- **SKILL.md additions refinement:** two → three. session-economy §5.3 prescribes INPUTS entry, Phase 4 paragraph, and **Phase 5 note about the journal commit landing in SPEC_REPO_ROOT**. The N=5 prediction (mirroring N=5's two additions for spec-review) missed the Phase 5 note. spec-amend is the **only quintet skill receiving three additions** from the session-economy commit — every other skill received two (spec-review, spec-execute Phase 1 + Phase 8) or fewer (spec-write / spec-design: one OUTPUT FORMAT note each).

These refinements are first-class outputs of N=6; the spec records both at §2 Goals and §3 Background with explicit prediction-vs-reality framing.

### Source-file selection (decision + rationale)

The explicit table appeared in the session's Phase 1 Discovery Report. Repeated here for journal completeness:

| File | Used? | Rationale |
|---|---|---|
| [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) | Yes — authoritative for current behavior | The skill itself. 200 lines, 6 phases + Operating Principles + class-classification + "Notes on what makes this skill load-bearing." |
| [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 391–403 + line 414 | Yes — authoritative for design rationale (NOT for current behavior) | **Inline predecessor.** AMENDMENT PROTOCOL block embedded in the `spec-execute-prompt.md` artifact (lines 391–403) plus the design-note paragraph at line 414. Extracted into standalone skill at trilogy commit `49c15f0`. Cited Inspirational in §14 with explicit inline-extraction story. **N=5 prediction "zero predecessor" refined to "inline predecessor."** |
| [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) §5.3 | Yes — authoritative for current behavior added after the predecessor | Sibling design spec. Contributes **three** items to spec-amend (INPUTS entry + Phase 4 paragraph + Phase 5 note). Cited Authoritative in §14. **N=5 prediction "two SKILL.md additions" refined to "three SKILL.md additions"; spec-amend is the only quintet skill with three.** |
| [specs/tech-stack.md](../tech-stack.md), [specs/mission.md](../mission.md), [specs/roadmap.md](../roadmap.md) | Yes — authoritative for constraints, audience, lifecycle position | Constitutional bindings. |
| [specs/20260518-spec-review-skill/architecture.md](../20260518-spec-review-skill/architecture.md) + [journal.md](../20260518-spec-review-skill/journal.md) | Yes — N=5 retroactive-spec source | Closest-sibling structural source. "Pattern for N=6" callouts validated/refined above. Origin of the authoring-time per-citation walk discipline applied at this session. |
| [specs/20260518-spec-execute-skill/architecture.md](../20260518-spec-execute-skill/architecture.md) + [journal.md](../20260518-spec-execute-skill/journal.md) | Yes — N=4 retroactive-spec source | Origin of the two-source structure; closest large-skill structural source. spec-execute's amendment 2026-05-18-1 + spec-review's amendment 2026-05-18-2 are the two-cycle evidence base for the per-citation walk discipline. |
| [specs/20260518-spec-write-skill/architecture.md](../20260518-spec-write-skill/architecture.md) + [journal.md](../20260518-spec-write-skill/journal.md) | Yes — N=3 retroactive-spec source | "Pattern for N=4" lineage. N=3 omitted Voice-discipline §5 subsection — relevant input to the §5.8 Voice-discipline lineage citation. |
| [specs/20260518-spec-design-skill/architecture.md](../20260518-spec-design-skill/architecture.md) + [journal.md](../20260518-spec-design-skill/journal.md) | Yes — N=2 retroactive-spec source | Original predecessor-distinction discipline; Voice-discipline §5.6 origin. |
| [specs/20260517-project-constitution-skill/architecture.md](../20260517-project-constitution-skill/architecture.md) + [journal.md](../20260517-project-constitution-skill/journal.md) | Yes — N=1 retroactive-spec source | Original structural source; §11 cross-reference for class-of-references-amendment-scanning discipline. |
| [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) | Read — orientation only | Cited in §3, §9, §11, §12. Strategy doc's OQ-3 named this venue as cross-skill-coordination resolution point; surfaced as §13 OQ-4 rather than resolved unilaterally per Phase 2 operator decision. |
| [specs/20260515-spec-path-convention/architecture.md](../20260515-spec-path-convention/architecture.md) | Negative signal | Modifies spec-amend's `SPEC_PATH` example via commit `6d158fb` but does not architecturally describe the skill. |
| [.agents/skills/spec-execute/SKILL.md](../../.agents/skills/spec-execute/SKILL.md), [.agents/skills/spec-review/SKILL.md](../../.agents/skills/spec-review/SKILL.md), [.agents/skills/spec-write/SKILL.md](../../.agents/skills/spec-write/SKILL.md), [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) | Negative signal — pipeline neighbors, not architectural sources | Referenced for handoffs (spec-execute Phase 3/4 → here; spec-review Phase 7 → here) but their internal architecture is out of scope for this skill's spec. spec-execute SKILL.md line 209 verified as still containing the AMENDMENT PROTOCOL header that routes to spec-amend (post-trilogy state). |

### "Pattern for N=7" callouts — explicitly not declared

**This is the final retroactive spec.** N=6 declines to declare "Pattern for N=7" callouts because:

1. **No N=7 consumer.** The legacy quintet is complete. No further retroactive specs are planned.
2. **Pattern-doc handoff.** The accumulated patterns across N=1–N=6 journals are the input to `docs/retroactive-spec-pattern.md` (to be authored post-CP-2 batch audit). The pattern doc consumes the journals; the journals do not predict for the pattern doc.
3. **Honest closure.** Declaring patterns for a session that will never run is over-codification — the failure mode N=5 risk row warned about.

Instead, this journal ends with a **Next-action handoff** to the pattern-doc authoring session, listing the journal-mining inputs that session will consume.

### Format choice — design spec vs feature spec

Validated. The shipping skill is `/spec-design`; the operator invoked it for a retroactive design spec on `spec-amend`. No friction.

**Pattern (carried from N=1/N=2/N=3/N=4/N=5, validated).** Retroactive specs for already-shipping skills use `/spec-design`. **Six consecutive validations** is the terminal state of this pattern in the legacy quintet; the pattern doc will codify.

### Naming pattern — directory slug

`specs/20260518-spec-amend-skill/architecture.md`. Today is 2026-05-18.

**Pattern (carried from N=2/N=3/N=4/N=5, validated).** Authoring date, not strategy-doc-anticipated date. **Five consecutive same-date sessions** (N=2, N=3, N=4, N=5, N=6 all dated 2026-05-18) produce five sibling directories with the same date prefix; differentiation by skill-name slug is sufficient. Five is the terminal count for the legacy quintet.

### Audience framing

Reused verbatim from N=1/N=2/N=3/N=4/N=5: "Maintainers of the `ai-tools` methodology skills — Eric Wasgatt, future contributors, and future AI agents picking up the skill set."

**Pattern (carried from N=1/N=2/N=3/N=4/N=5, validated).** Audience is reusable verbatim across the legacy quintet. Six consecutive validations; pattern doc will codify.

### Verification commitment level

**Light verification**, per N=1/N=2/N=3/N=4/N=5 precedent. The spec text contains no external claims requiring WebFetch — all citations are repo-internal. Per-citation verifications walked at authoring time (recorded in dedicated sub-section above): predecessor line range (391–403 + 414) verified by reading the shared doc; session-economy §5.3 verified by reading the section line-by-line; tech-stack.md §21-33 / §44 / §48 / §51 verified by reading on heading lines; commit SHAs (`49c15f0`, `e483466`, `d9a0002`, `6d158fb`, `7fee46f`, `85821ca`, `0ccc644`) verified against `git log`.

**Pattern (carried from N=1/N=2/N=3/N=4/N=5, validated).** Light verification is the correct default for the legacy quintet. Six consecutive validations.

### Open-question framing — handling known gaps

§13 reports **four first-class OQs** (in-flight/amendment boundary; re-approval path after status revert; multi-finding batching scope; cross-skill amendment coordination), two moved to §12 (reviewer attribution under AI assistance — inherited from N=5; cross-spec citation review as Phase 3/Phase 5 check — inherited from N=5), and the pattern-doc decision resolved (no longer §12; resolved at Phase 2 as Next-action).

**Decision process:** the operator was asked explicitly (Phase 2 `AskUserQuestion` covering four candidates) where each belonged. Operator chose:
- Predecessor framing: Recommended (Inspirational with inline-extraction story).
- Cross-skill coordination: Recommended (§13 OQ with full structure).
- Additional OQs: Recommended × 2 (in-flight/amendment boundary; re-approval path) + non-Recommended × 1 (multi-finding batching; user opted to surface). Reviewer-attribution declined as OQ (kept as §12 inheritance per Recommended).
- Pattern doc: Recommended (author as closing artifact post-CP-2).

**Pattern (carried from N=2/N=3/N=4/N=5, validated and extended).** Triage candidates explicitly to the operator rather than picking placement unilaterally. **Six consecutive sessions** have produced the same pattern: explicit candidate triage works. **Refinement at N=6:** the operator selected three of three Recommended options plus added one non-Recommended (multi-finding batching), making four total §13 OQs — more than any prior session. The pattern allows for operator additions beyond the Recommended set, which N=5 (where all four Recommended were taken) did not exercise. Recommended-discipline holds; operator-addition-capacity demonstrated.

### Drift-audit-as-checkpoint (CP-2)

Both CP-1 (faithfulness) and CP-2 (drift audit) declared in §9.

**Refinement at N=6.** CP-2 trigger narrative compounds to its terminal form: names N=2's, N=3's, N=4's, AND N=5's already-passed CP-1s, narrowing the remaining trigger to "project-constitution CP-2 only." With no N=7 retroactive spec, the compounding narrative has reached its endpoint. The CP-2 review focus also adds **cross-skill drift patterns as a CP-2-only check** explicitly — single-spec CP-2s cannot see them; the final retroactive-spec batch is the only moment this audit is performed. CP-2 cross-spec session-economy check is shape-(i)-only (matches N=5).

**Pattern terminal state.** The CP-2 trigger narrative has now compounded across N=2→N=3→N=4→N=5→N=6, enumerating each prior CP-1 verdict. The pattern doc will codify the discipline as "each retroactive spec's CP-2 trigger enumerates all prior already-passed CP-1s and narrows the remaining condition."

### Scope discipline — what was kept out

§2 Non-goals lists six items explicitly. §12 Out of Scope lists fourteen items, most inherited from N=5 / N=4 / N=3 / N=2 / N=1 / strategy-doc, plus two new:
- (new) Mechanism for committing an amendment to spec-amend SKILL.md atomically with the spec change — self-referential edge case named at §10 and §11; documented but not actionable from inside SKILL.md.
- (refined) Pattern-doc decision **resolved at Phase 2 as Next-action handoff** rather than deferred. Resolved, not deferred; the §12 item describes the resolution.

The format-question gap from N=2 §13 OQ-1 is named in §12 (inherited from N=3 / N=4 / N=5 disposition). The reviewer-attribution convention is named in §12 (inherited from N=5).

**Pattern (carried from N=1/N=2/N=3/N=4/N=5, validated).** Retroactive specs are descriptive, not prescriptive. The §12 list grew from N=1's four → N=2's thirteen → N=3's fifteen → N=4's eleven → N=5's thirteen → N=6's fourteen. The size is a function of items inherited from prior sessions plus new items surfaced; not a quality signal. Pattern doc will codify the discipline.

### Cross-session knowledge transfer

This is the **final** legacy-quintet retroactive spec. The journal-mining handoff loop closes here.

**What this journal commits to:**
- The "Pattern for N=6 — validation outcomes" table above is the terminal structural pattern for N≥3 journals. The pattern doc will codify the table-of-validation-outcomes structure as a recurring journal section.
- The N=5 prediction "spec-amend has zero predecessor and two SKILL.md additions" is refined to **"inline predecessor (one structural distinction from N=2-N=5) and three SKILL.md additions (one larger than N=5)."** Both refinements are recorded explicitly in spec body.
- The **authoring-time per-citation walk discipline** (named in [N=5 journal §"Pattern observation for N=6"](../20260518-spec-review-skill/journal.md)) was **adopted at N=6 authoring** and **caught one citation error before commit** (§5.6 "Phase 6 — Closeout" → "Phase 6 — Update artifacts" plus a sibling capitalization). The discipline worked as designed: the same failure mode that drove the N=4 and N=5 amendment cycles was caught one inflection point earlier, at authoring time, **before** the commit and **before** CP-1. CP-1 evidence will further validate or refine the discipline.
- **No "Pattern for N=7" callouts are declared.** The pattern-mining loop closes.

**What this journal does NOT commit to:**
- A `docs/retroactive-spec-pattern.md`. **Resolved at Phase 2: to be authored as a separate closing artifact post-CP-2 batch audit, drawing on the consolidated journal-mining evidence from N=1-N=6 plus CP-2 findings.** This journal records the input list but does not pre-author the pattern doc.
- A binding template for any future retroactive spec. The journal-mining protocol is the pattern across N=1-N=6; the two-source structure + authoring-time per-citation walk are the structural additions; the pattern doc will codify all of these.
- Resolution of any of the four §13 OQs, the format-question-prompt gap, the cross-skill amendment coordination question (now §13 OQ-4 rather than out-of-scope inheritance), the constitution-amendment ceremony, or the reviewer-attribution convention. All carried forward as open work.

### Friction observed

Honest record of where this session encountered friction. Useful for the pattern-doc authoring session to anticipate.

- **The N=5 prediction of "zero predecessor" required correction at Phase 1.** Reading the N=5 journal first set the expectation that spec-amend would have no Inspirational predecessor citation. Discovery at Phase 1 found the AMENDMENT PROTOCOL block at lines 391–403 of the shared doc, plus the design-note paragraph at line 414, embedded inside `spec-execute-prompt.md`. The N=5 prediction missed that the trilogy commit `49c15f0` *extracted* this block into a standalone skill — the predecessor exists, but as an inline-not-standalone source. Recording the refinement explicitly at §2 Goals and §3 Background prevents the next reader from replaying the search.

- **The N=5 prediction of "two SKILL.md additions" required correction at Phase 1.** Reading session-economy §5.3 directly showed three additions, not two: INPUTS entry, Phase 4 paragraph, and Phase 5 note. The N=5 prediction mirrored N=5's own two-addition shape (for spec-review). spec-amend's third addition (Phase 5) is small but real. Recording the refinement explicitly at §2 Goals, §3 Background, and §14 prevents the CP-2 audit from missing the third mapping.

- **The strategy doc's pre-resolution of cross-skill amendment coordination at "the spec-amend retroactive spec at session 5" did not provide evidence to resolve.** Reading the strategy doc OQ-3 + N=3/N=4/N=5 §12 disposition created the expectation that N=6 would resolve the question. Discovery surfaced that no post-trilogy cross-skill amendment cycle has actually fired — the two evidence points (`e483466`, `6d158fb`) predate the trilogy commit's propose/apply separation and so do not exemplify the post-trilogy design space. Resolving without evidence would have been premature codification (the failure mode the methodology guards against). Phase 2 operator decision: surface as §13 OQ-4 rather than resolve unilaterally. Honest position; gap stays open with explicit watch items.

- **The self-referential amendment paradox surfaced as a §10 risk row.** The skill amends specs, including its own. An amendment to spec-amend SKILL.md goes through the very skill being amended. The risk row's mitigation ("staged bootstrap — apply current workflow to produce next workflow") is the standard answer (compiler self-compilation), but the question only surfaces in a spec for *this* skill, not for siblings. Recording the meta-observation at §11 prevents future readers from being surprised when they first amend SKILL.md.

- **Six consecutive "Recommended option selected" choices have not yet generated a counter-instance.** N=6 was the first session where the operator added a non-Recommended option (multi-finding batching, surfaced as the third candidate among "additional OQs"). The Recommended-discipline still held for the other three of four. The pattern doc may want to codify "Recommended is the default; operator additions are expected" rather than "Recommended is always selected."

- **The authoring-time per-citation walk caught its first error on its first application.** Drafting §5.6 Phase 6's "Pattern invoked" sub-block, the author wrote "Phase 6 — Closeout" as the sibling-spec citation — a paraphrase of the *purpose* of spec-execute §5.6 rather than its actual heading. The discipline forced re-reading the heading verbatim before commit; the error was caught and the citation corrected to "Phase 6 — Update artifacts." This is direct evidence that the failure mode underlying the N=4 amendment 2026-05-18-1 and N=5 amendment 2026-05-18-2 (paraphrasing a section heading instead of quoting it) recurs at authoring time without the discipline — and is preventable with it. The pattern doc should treat this as core evidence: the discipline is **load-bearing**, not ceremonial.

### Conversation grounding

- Operator invoked `/spec-design` per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md) session 5 ordering. No prior in-thread conversation; the strategy doc + N=1/N=2/N=3/N=4/N=5 specs and journals function as the "extended conversation."
- Phase 1 (Discovery) produced source-file table including three negative-signal rows; landscape orientation against the lifecycle skill family; constraint orientation against four constitutional citations + one sibling-design-spec citation (session-economy §5.3); conversation grounding (strategy doc + N=1/N=2/N=3/N=4/N=5 specs and journals as inputs); naming candidates not needed (name fixed by skill name); **predecessor confirmed and scoped** to [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md) lines 391–403 + line 414 (inline, not standalone — refinement of N=5 prediction); **sibling design spec confirmed** at [session-economy §5.3](../20260514-session-economy/architecture.md) with **three SKILL.md additions** (refinement of N=5 prediction).
- Phase 2 (Clarify) surfaced four operator decisions via `AskUserQuestion`: predecessor framing (Recommended), cross-skill coordination placement (Recommended), additional OQs (three of three Recommended plus one operator-added: multi-finding batching), pattern doc disposition (Recommended). All Recommended options chosen; one operator-added non-Recommended OQ surfaced. The unilateral decisions (defer CP-1 to fresh session; compounded CP-2 trigger to terminal four-CP-1 form; session-economy spec as Authoritative companion source; §5.4 + §5.5 + INPUTS contract as the single shape-(i) §5-enumerated mapping covering three additions; authoring-time per-citation walk adopted) were not objected to.
- No `[blocker]` open questions arose. Session proceeded to Phase 3 (spec document + journal authoring).

### Tasks defined

None. Design spec, not feature spec. The "next work" is review (CP-1) and audit (CP-2), declared in §9, plus the pattern-doc authoring as a separate downstream artifact named in the Next action pointer below.

### Next action pointer

Three steps for this spec, plus one closing-of-the-series step:

1. **Commit** the spec + journal as a paired commit. This is the closing action of session 5 (the final legacy-quintet retroactive-spec authoring session).
2. **CP-1 review** in a fresh session: operator invokes `/spec-review` against [architecture.md §9 CP-1](./architecture.md).
3. **CP-2 batched drift audit** per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). With N=6's CP-1 close, the remaining condition reduces to "project-constitution CP-2 only" — that may be folded into the batch per [strategy doc OQ-1](../../docs/retroactive-spec-strategy.md). Batched CP-2 produces the cross-skill drift evidence base.
4. **Pattern-doc authoring** (separate session, not this spec): operator invokes `/spec-design` or `/spec-write` against a new spec/doc `docs/retroactive-spec-pattern.md`. **Journal-mining inputs for that session:**
   - [N=1 journal](../20260517-project-constitution-skill/journal.md) — original structural source; "Pattern for N=2" callouts.
   - [N=2 journal](../20260518-spec-design-skill/journal.md) — N=2 inflection point; first "Pattern for N=3" callouts; first explicit candidate-triage Phase 2.
   - [N=3 journal](../20260518-spec-write-skill/journal.md) — voice-discipline gap; first deferral of pattern-doc decision.
   - [N=4 journal](../20260518-spec-execute-skill/journal.md) — two-source structure introduced; per-§5-subsection citation audit refined via amendment 2026-05-18-1.
   - [N=5 journal](../20260518-spec-review-skill/journal.md) — simplest two-source application; structural simplification observation; authoring-time per-citation walk pattern observation (post-CP-1).
   - **This journal** — final journal; six predictions validated/refined/rejected with two N=5 predictions refined (inline predecessor, three additions); per-citation walk discipline adopted at authoring time with zero-error outcome.
   - **CP-2 batch audit findings** (when available) — cross-skill drift patterns visible only at batch.
   - **N=4 amendment 2026-05-18-1 + N=5 amendment 2026-05-18-2** — the two amendment cycles in the retroactive-spec series; structural evidence base for the citation-error failure mode the per-citation walk discipline mitigates.

No `[blocker]` open questions; the spec is ready for CP-1.

## 2026-05-18 — Amendment 2026-05-18-3

**Section amended:** [specs/20260518-spec-amend-skill/architecture.md](./architecture.md) §5.3 (Pattern invoked sub-block), §5.9 (Pattern invoked sub-block); [specs/20260518-spec-amend-skill/journal.md](./journal.md) "Pre-commit per-citation walk" rows §5.3 and §5.9; [specs/20260518-spec-review-skill/architecture.md](../20260518-spec-review-skill/architecture.md) §5.11 (Pattern invoked sub-block — upstream source of inherited error)
**Trigger:** CP-1 review verdict (this session, pre-commit) raised [important] finding I1 — §5.9 cites `docs/retroactive-spec-strategy.md` as holder of Amendment 2026-05-17-1; actual amendment lives in [project-constitution-skill journal.md:163](../20260517-project-constitution-skill/journal.md#L163). Sampling §5.3 surfaced advisory A1 (Phase 7 — Checkpoint gate capitalization).
**Reason:** Two citation errors point to files that do not contain the cited records. The per-citation walk discipline applied at authoring time scoped to §-heading paraphrases against sibling specs and did not extend to amendment-ID citations against their cited records, nor catch the single-letter capitalization difference. Fix the inherited form at its upstream source forward.
**Impact summary:** No tasks; spec-amend CP-1 verdict closes "pass with comments" on application; spec-review CP-1 remains "pass with comments" (post-CP-1 citation correction, same shape as N=5 amendment 2026-05-18-2). No completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept for both specs (Draft — Open for Review post-CP-1)
**Commit:** `7a33abe` (architecture edits); `c01488a` (journal entries)

### Full record

## Amendment 2026-05-18-3 — spec-amend §5.3 + §5.9 + journal rows 36, 42; spec-review §5.11 (cross-skill)

**Trigger.** CP-1 review of spec-amend-skill (this session, pre-commit) raised [important] finding I1: §5.9 "Pattern invoked" cites `docs/retroactive-spec-strategy.md` for "Amendment 2026-05-17-1"; the cited file contains no such record. The actual Amendment 2026-05-17-1 lives at `specs/20260517-project-constitution-skill/journal.md` line 163 and amends the project-constitution-skill spec. The N=6 §5.9 citation was inherited verbatim from N=5 spec-review-skill `architecture.md:259`, which holds the same incorrect form. Sampling the §5.3 citation under the per-citation walk surfaced advisory A1: §5.3 cites "Phase 7 — Checkpoint Gate" (capital G) where the actual heading is "Phase 7 — Checkpoint gate" (lowercase). Operator chose to bundle A1 + I1 as a coherent §5 citation-accuracy fix (Q1 scope) and to pair with the upstream N=5 source (Q2 scope) — making this the first post-trilogy cross-skill amendment cycle, directly relevant evidence for §13 OQ-4.

**Section.** Five edits across two specs and two journals — same coherent change (§5 citation accuracy, including upstream source of inherited error):

- spec-amend `architecture.md:141` (§5.3 Phase 3 Approval, "Pattern invoked" sub-block)
- spec-amend `architecture.md:225` (§5.9 Portability rule for links, "Pattern invoked" sub-block)
- spec-amend `journal.md:36` (§5.3 row in "Pre-commit per-citation walk" table)
- spec-amend `journal.md:42` (§5.9 row in "Pre-commit per-citation walk" table)
- spec-review `architecture.md:259` (§5.11 Portability rule for links, "Pattern invoked" sub-block — same incorrect citation)

**Change.**

*Edit 1 — spec-amend architecture.md §5.3 line 141:*

Before:
> **Pattern invoked.** "Explicit approval is a hard stop." Same discipline as [spec-execute §5.7 Phase 7 Checkpoint Gate](../20260518-spec-execute-skill/architecture.md) — the skill structurally enforces the pause so the operator cannot accidentally implicitly approve by inaction. Verified against [SKILL.md Phase 3](../../.agents/skills/spec-amend/SKILL.md) at the date of this spec.

After:
> **Pattern invoked.** "Explicit approval is a hard stop." Same discipline as [spec-execute §5.7 Phase 7 Checkpoint gate](../20260518-spec-execute-skill/architecture.md) — the skill structurally enforces the pause so the operator cannot accidentally implicitly approve by inaction. Verified against [SKILL.md Phase 3](../../.agents/skills/spec-amend/SKILL.md) at the date of this spec.

*Edit 2 — spec-amend architecture.md §5.9 line 225:*

Before:
> **Pattern invoked.** [Strategy-doc Amendment 2026-05-17-1](../../docs/retroactive-spec-strategy.md) (drop `~/.claude/skills/` references). Carried forward at every N≥2; carried forward at N=6.

After:
> **Pattern invoked.** [project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md) (drop `~/.claude/skills/` references). Carried forward at every N≥2; carried forward at N=6.

*Edit 3 — spec-amend journal.md line 36 (§5.3 row in walk table):*

Before:
> | §5.3 Phase 3 Approval | spec-execute §5.7 ("Phase 7 — Checkpoint Gate"); SKILL.md Phase 3 | spec-execute §5.7 verified as "Phase 7 — Checkpoint Gate" (verified to exist; explicit-approval discipline matches). SKILL.md Phase 3 lines 102–111 verified. |

After:
> | §5.3 Phase 3 Approval | spec-execute §5.7 ("Phase 7 — Checkpoint gate"); SKILL.md Phase 3 | spec-execute §5.7 verified as "Phase 7 — Checkpoint gate" (verified to exist; explicit-approval discipline matches). SKILL.md Phase 3 lines 102–111 verified. Capitalization corrected post-CP-1 via amendment 2026-05-18-3. |

*Edit 4 — spec-amend journal.md line 42 (§5.9 row in walk table):*

Before:
> | §5.9 Portability rule for links | Strategy-doc Amendment 2026-05-17-1; .agents/skills/spec-amend/SKILL.md path | Strategy-doc amendment verified. SKILL.md path verified. |

After:
> | §5.9 Portability rule for links | project-constitution-skill Amendment 2026-05-17-1; .agents/skills/spec-amend/SKILL.md path | Citation file corrected post-CP-1 via amendment 2026-05-18-3 (original draft cited the strategy doc; actual amendment lives in project-constitution-skill journal). SKILL.md path verified at authoring time. |

*Edit 5 — spec-review architecture.md §5.11 line 259 (upstream source of inherited error):*

Before:
> **Pattern invoked.** [Strategy-doc Amendment 2026-05-17-1](../../docs/retroactive-spec-strategy.md) (drop `~/.claude/skills/` references). Carried forward at every N≥2.

After:
> **Pattern invoked.** [project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md) (drop `~/.claude/skills/` references). Carried forward at every N≥2.

**Reason.** Both citation errors point readers to files that do not contain the cited records: §5.9 (and N=5 §5.11) names the strategy doc as holder of Amendment 2026-05-17-1, but the amendment lives in the project-constitution-skill journal; §5.3 capitalizes "Gate" where the actual heading uses lowercase "gate". The per-citation walk discipline applied at authoring time caught the §5.6 paraphrase but did not extend its scope to amendment-ID citations against their cited records, nor catch the single-letter capitalization difference. Fixing both errors at once across both specs makes the inherited form correct from its upstream source forward and produces the first post-trilogy cross-skill amendment evidence point — directly relevant to §13 OQ-4.

**Impact.**
- **Affected tasks:** none (design specs, no atomic tasks).
- **Affected checkpoints:** spec-amend CP-1 (this session's verdict closes "pass with comments" upon application); spec-review CP-1 (already closed at "pass with comments" on 2026-05-18 via verdict commit `e8193a8`; amendment to §5.11 is post-CP-1, same shape as N=5 amendment 2026-05-18-2 which itself was post-CP-1 to §5.10).
- **Completed work invalidated:** No. spec-review's CP-1 verdict remains "pass with comments"; the §5.11 citation was not among the [important] findings at the time of that verdict (it was inherited from a prior N≥2 spec and propagated without challenge).
- **Cross-references requiring follow-up:** None. §11 of spec-amend already cites the amendment correctly; no other §5.X "Pattern invoked" rows reference this same amendment.

**Status implication.** **kept** for both specs. spec-amend: `Draft — Open for Review` post-CP-1, pending the §9 Status line backfill that will be applied as part of the verdict-application sequence. spec-review: `Draft — Open for Review` post-CP-1 (status already at this state per its §9 Status line on 2026-05-18, verdict commit `e8193a8`). Surfacing per the auto-memory directive on status-implication explicitness: amendment is a citation correction, not a structural change to the skill design; neither spec needs to revert to pre-Draft for re-approval. N=5 amendment 2026-05-18-2 (commit `85821ca`) is the precedent for kept-status post-CP-1 citation corrections; this amendment follows the same shape across two specs simultaneously.

**Approver.** Eric Wasgatt, 2026-05-18.

### Cross-skill note (first post-trilogy cycle — §13 OQ-4 evidence)

This is the **first post-trilogy-commit cross-skill amendment cycle**. The two pre-trilogy cross-skill changes (`e483466`, `6d158fb`) used a different model (single commit touching multiple skills directly) that the trilogy commit was designed to deprecate. Mechanics applied at this cycle, recorded as worked-example evidence for §13 OQ-4:

- **Single amendment ID** (`2026-05-18-3`) shared across two specs (N=5 spec-review + N=6 spec-amend). No "shared prefix" needed — the ID itself is shared because the change is the same.
- **One spec-edit commit** (`7a33abe`) touching both specs' `architecture.md` files (three edits total).
- **One journal commit** touching both specs' `journal.md` files (two existing-row corrections in spec-amend's per-citation walk table; one new amendment entry in each spec's journal).
- **Primary record lives in spec-amend journal** (this entry) with the structured Phase 2 amendment block. **Companion record in spec-review journal** references the primary by amendment ID without duplicating the structured block.
- **Status implication kept for both specs.** No re-approval cycle triggered.

This worked example anchors §13 OQ-4 to evidence rather than hypothesis. The honest leaning "(d) defer until first observed post-trilogy cycle" → now (d) is satisfied; the future amendment session that takes up OQ-4 has this entry as its anchor for codifying option (a) ("N independent amendments with a shared amendment-ID prefix") — with the simplification that no prefix is needed when there is only one cross-skill change at a time. The pattern doc (post-CP-2) will consume this entry alongside the OQ-4 watch-item description.

### Cross-skill note — codification candidate for SKILL.md

Option (c) from §13 OQ-4 ("Document the convention in a §-of-spec-amend addition: 'Cross-skill case.'") is a candidate for a future amendment to spec-amend SKILL.md once this evidence base accumulates a second cycle. One worked example is enough to anchor the watch item; codifying convention on one cycle would over-fit. The watch item for OQ-4 stays open with this entry as its first observation.

## 2026-05-18 — Review of CP-1

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Tasks reviewed:** none (design spec, no atomic tasks); artifacts under review were [architecture.md](./architecture.md) and [journal.md](./journal.md) at commit `3390688`
**Blockers:** 0
**Important:** 1 — I1: §5.9 "Pattern invoked" cited `docs/retroactive-spec-strategy.md` as holder of Amendment 2026-05-17-1; actual amendment lives at [specs/20260517-project-constitution-skill/journal.md:163](../20260517-project-constitution-skill/journal.md#L163). Form inherited verbatim from N=5 [spec-review architecture.md:259](../20260518-spec-review-skill/architecture.md#L259). Structurally identical to the citation-error failure mode that drove N=4 amendment 2026-05-18-1 and N=5 amendment 2026-05-18-2.
**Advisory:** 2 — A1: §5.3 cites "Phase 7 — Checkpoint Gate" (capital G); actual heading "Phase 7 — Checkpoint gate" (lowercase). A2: per-citation walk discipline scope at N=6 was §-heading paraphrases against sibling-spec §5 subsections; did not cover amendment-ID citations against their cited records. Journal "zero-error outcome" claim is incorrect under the broader scope. Refinement signal for the pattern doc; not a spec defect.
**Spec amendments proposed:** Amendment to §5.9 "Pattern invoked" retargeting citation from `docs/retroactive-spec-strategy.md` to `specs/20260517-project-constitution-skill/journal.md`; companion correction to journal table row at [journal.md:42](./journal.md#L42); paired upstream fix to N=5 [spec-review architecture.md:259](../20260518-spec-review-skill/architecture.md#L259) per operator scope decision Q2. Applied as amendment 2026-05-18-3 (commits `7a33abe` + `c01488a`).
**Next action:** Verdict written back to §9 Status line (this commit) and recorded in this journal entry. CP-1 closed at "pass with comments." Next checkpoint is CP-2 (batched drift audit per §9); remaining trigger condition reduces to "project-constitution CP-2 only" with this spec's CP-1 close. CP-2 happens in a separate session per [docs/retroactive-spec-strategy.md](../../docs/retroactive-spec-strategy.md). Also queued (separate session): authoring `docs/retroactive-spec-pattern.md` post-CP-2, per §11 closing-of-the-series step.

### Review focus walk — per-item findings

| Focus item (from §9 CP-1) | Verdict | Notes |
|---|---|---|
| Every commitment in §4/§5/§6 corresponds to behavior in shipping SKILL.md | pass | All six phases + change classification + voice + portability verified against SKILL.md. §6 NFR rows each map to an OP or Phase commitment. |
| No commitment contradicts shipping SKILL.md | pass | No contradictions surfaced. |
| ASPP correctly characterized in §3, §6 | pass | Degradation paths for `SPEC_REPO_ROOT` absent AND design-spec target both named. |
| Predecessor distinguished as inline-not-standalone | pass | AMENDMENT PROTOCOL block at predecessor lines 391–403 + line 414 design-note paragraph verified verbatim against [docs/spec-driven-development-prompts-conversation.md](../../docs/spec-driven-development-prompts-conversation.md). Trilogy commit `49c15f0` extraction story recorded faithfully. Three independent citations across §2/§3/§14. |
| Session-economy as sibling design spec; shape (i) §5-enumerated, three additions | pass | Mapping retro §5.4 + §5.5 + INPUTS contract ↔ session-economy §5.3 verified. Three SKILL.md additions confirmed at session-economy §5.3 lines 123–145. N=5-prediction refinement (two → three additions) recorded explicitly. |
| Six phases in §5 match shipping SKILL.md; §5.7 change classification matches ROLE block | pass | §5.1–§5.6 match SKILL.md Phase 1–6. §5.7 trichotomy matches SKILL.md ROLE block lines 37–41. |
| §13 OQs structurally complete | pass | OQ-1, OQ-2, OQ-3, OQ-4 each with Question / Analysis / Leaning / Owner / Watch items / Anti-goals. |
| Spec self-contained per OP | pass | Vocabulary block (§4) defines load-bearing terms inline; each §5 subsection independently readable. |
| Section-heading citations point to heading line | pass | Sampled. |
| Authoring-time per-citation walk discipline (verify a sample) | pass with comments | §5.6 (the celebrated pre-commit catch) verified; §5.8 Voice-discipline lineage verified; §5.3 surfaced A1 (capitalization); §5.9 surfaced I1 (wrong file). Discipline worked for §-heading citations; missed amendment-ID citations and single-letter capitalization. |
| Portability rule honored | pass | No `~/.claude/skills/...` references; no absolute paths. |

### Exit criteria status

- **Reviewer issues a verdict per the structured format declared in spec-review SKILL.md.** met — verdict block above.
- **All [blocker] findings resolved or escalated to /spec-amend.** met — zero blockers; the [important] finding routed to /spec-amend and applied as amendment 2026-05-18-3.
- **Verdict written back to §9 (status line) and to the journal.** met — this commit closes CP-1.

### Recommendation

Pass with comments. The retro spec faithfully describes the shipping SKILL.md across all phases, OPs, NFRs, and the change-classification trichotomy. The two N=5-prediction refinements (inline predecessor; three SKILL.md additions) are recorded with explicit prediction-vs-reality framing across §2, §3, §8, and §14. The four §13 OQs are well-structured and honestly framed (especially OQ-4's refusal to codify on pre-trilogy-commit evidence — a position which this same session then unblocked by triggering the first post-trilogy cross-skill amendment cycle). The single [important] finding is a §5.9 cross-spec lineage-citation error inherited verbatim from N=5 — exactly the failure mode the authoring-time per-citation walk discipline was designed to head off; the discipline caught one such error (§5.6 Phase 6) but missed one (§5.9) because its declared scope was §-heading citations, not amendment-ID citations. Resolution: amendment 2026-05-18-3 (applied this session, commits `7a33abe` + `c01488a`) fixed the inherited form at its upstream source forward and established the first post-trilogy cross-skill amendment cycle's mechanics as worked-example evidence for §13 OQ-4. Work may proceed past CP-1; CP-2 batch audit remains open per §9 trigger ("project-constitution CP-2 only" condition).

**Verdict commit:** `61b2a28` (backfilled into §9 Status line in a follow-up commit per the established N=4/N=5 pattern).

## 2026-05-18 — Review of CP-2

**Reviewer:** Claude (AI assistant) on behalf of Eric Wasgatt
**Outcome:** pass with comments
**Tasks reviewed:** N/A (design spec; CP-2 is a drift audit of the spec body against the shipping SKILL.md — see [architecture.md §9 CP-2](./architecture.md) review focus.)
**Blockers:** 0
**Important:** 0
**Advisory:** 3 — D-1 SKILL.md WND-8 (propose/apply separation between `spec-execute`/`spec-review` and `spec-amend`) lacks explicit §5/§6 carrier; carried in §3 Background + §3 Dependencies but not first-class in §5 phase rules or §6 NFRs. D-2 SKILL.md preamble line 15 ("How this skill works") omits `spec-write` from the invocation list; frontmatter line 4 and HANDOFF NOTES line 187 both name it. D-3 SKILL.md OUTPUT FORMAT block (lines 165–170) carries per-phase manifestation rules (Phases 1–3 conversational; Phase 4 Edit + commit; Phase 5 Edit; Phase 6 conversational) absent from spec §4 Output topology.
**Spec amendments proposed:** Two — D-1 amend spec §6 to add a "Propose/apply separation discipline" NFR row landing WND-8 as first-class commitment; D-3 amend spec §4 Output topology to add a phase-manifestation enumeration paragraph. Both routed via `/spec-amend` this session per operator's (c)→(a) override decision.
**SKILL.md amendments proposed (in-session per operator decision):** One — D-2 amend SKILL.md preamble line 15 to name all four siblings (matching frontmatter + HANDOFF NOTES).
**Next action:** Apply three amendments (2026-05-18-4 D-1; 2026-05-18-5 D-3; 2026-05-18-6 D-2) in this session per N=5 batch precedent. Brief re-verification commit closes CP-2 once amendments land. Retroactive-spec adoption arc for `spec-amend` is complete after that point; spec exits "Draft — Open for Review" at post-CP-2 transition per N=1/N=2/N=3/N=4/N=5 convention. Cross-spec layer recorded in [batch journal N=6 entry](../20260518-cp2-batch-audit/journal.md). After CP-2 closes, the only remaining batch-level step is the closing summary in the batch journal, followed by authoring `docs/retroactive-spec-pattern.md` per §11 step 4.

### Routing summary

| ID | Summary | Routing |
|---|---|---|
| D-1 | SKILL.md WND-8 "Do not let `spec-execute` or `spec-review` silently apply amendments. Both should propose; this skill applies. The separation is what makes amendments visible." lacks explicit §5/§6 carrier. Carried in §3 Background (trilogy commit framing) + §3 Dependencies (spec-review entry) but no §5 phase rule or §6 NFR row carries the propose/apply discipline as a first-class commitment. WND-partial-home class 6th consecutive data point (N=1/N=2/N=3/N=4/N=5/N=6). | (a) amend spec §6 — operator (c)→(a) override per N=3/N=4/N=5 pattern (Recommended selected) |
| D-2 | SKILL.md preamble line 15 ("How this skill works") "another skill — `spec-execute`, `spec-review`, or an in-flight `spec-design` session" omits `spec-write`. Frontmatter line 4 names all four siblings; HANDOFF NOTES line 187 names spec-write explicitly. Internal SKILL.md preamble-vs-body inconsistency. Preamble-vs-body mirror class 6th consecutive data point (N=2/N=3/N=4 ×2/N=5/N=6). New flavor: preamble-omits-sibling-caller (third flavor of the class). | (b) amend SKILL.md preamble |
| D-3 | SKILL.md OUTPUT FORMAT block (lines 165–170) carries per-phase manifestation rules absent from spec §4 Output topology. Spec §4 captures the 3 durable artifacts; SKILL.md additionally characterizes Phases 1–3 / Phase 4 / Phase 5 / Phase 6 by output form (conversational vs Edit). 3rd data point of the N=2 D-3 / N=3 D-3 class (SKILL.md OUTPUT FORMAT items absent from spec). | (a) amend spec §4 — operator (c)→(a) override per N=2/N=3 routing precedent for the same class (Recommended selected) |

### Verification trail

Per-claim verification walked at audit time:

- **Session-economy §5.3 shape (i) mapping verified.** Read [specs/20260514-session-economy/architecture.md §5.3](../20260514-session-economy/architecture.md) directly. §5.3 prescribes exactly three SKILL.md additions: (1) `SPEC_REPO_ROOT` INPUTS entry; (2) Phase 4 "Multi-repo case" paragraph; (3) Phase 5 journal-commit note. All three present in SKILL.md ([line 28](../../.agents/skills/spec-amend/SKILL.md#L28), [line 130](../../.agents/skills/spec-amend/SKILL.md#L130), [line 152](../../.agents/skills/spec-amend/SKILL.md#L152)). Wording matches verbatim. Zero shape (ii) claims to verify; zero shape (ii) drift possible. Mapping is the simplest in the quintet structurally and three additions in scale (one larger than N=5's two).
- **ASPP citation discipline confirmed.** Spec §3 line 53 cites `tech-stack.md §21-33` correctly (heading line for Atomic-Skill Portability Principle). Spec §6 Adoptability NFR cites same. N=1/N=2/N=3/N=4/N=5 baseline pattern (correct citation at heading line) holds at N=6 — six-spec sweep clean across the entire legacy quintet plus N=1 baseline.
- **Section-heading citation discipline confirmed.** All §3 tech-stack.md citations (§21-33, §44, §48, §51) point to heading lines per CP-1 verification trail and re-verification at this audit. N=2/N=3/N=4/N=5 corrective holds at N=6.
- **Amendment-ID citation correctness post-amendment-2026-05-18-3 confirmed at spec-amend endpoint.** §5.9 line 225 now reads `[project-constitution-skill Amendment 2026-05-17-1](../20260517-project-constitution-skill/journal.md)` — correct holder. Both endpoints of the first post-trilogy cross-skill amendment cycle (N=5 §5.11 + N=6 §5.9) are clean at CP-2.
- **Six SKILL.md phases verified.** Phase 1 (lines 53–68), Phase 2 (lines 70–100), Phase 3 (lines 102–111), Phase 4 (lines 113–130), Phase 5 (lines 132–154), Phase 6 (lines 156–164) all match spec §5.1–§5.6 commitments verbatim or in substance. Multi-repo paragraphs at Phase 4 (line 130) and Phase 5 (line 152) verified verbatim against session-economy §5.3.
- **All 7 OPs and 8 WND items walked against §5/§6 carriers.** OP-1/2/3/4/5/6/7 all have explicit §5/§6 carriers in spec (verified one-by-one). WND-1/2/3/4/5/6/7 all have explicit §5/§6 carriers. WND-8 alone lacks explicit §5/§6 carrier (D-1).
- **SKILL.md preamble walked line-by-line against frontmatter description and HANDOFF NOTES.** Line 4 frontmatter names spec-write, spec-design, spec-execute, spec-review (four pairings). Line 15 preamble names spec-execute, spec-review, spec-design (three; omits spec-write). Line 187 HANDOFF NOTES names spec-design + spec-write. Internal SKILL.md preamble-vs-body inconsistency on the spec-write axis (D-2).
- **SKILL.md "Notes on what makes this skill load-bearing" walked against Phase body (new N=5 class check).** "Visibility is the point" paragraph (lines 196–197) correctly historizes the pre-trilogy state ("The previous design (folding the Amendment Protocol into `spec-execute`) worked, but amendments were buried inside execution sessions. Pulling them into a named skill...") — no stale phrasing. **Class non-fires at N=6** — consistent with the N=5 prediction that the class may be spec-review-specific.
- **SKILL.md OUTPUT FORMAT walked against spec §4 Output topology.** SKILL.md lines 165–170 enumerate per-phase manifestation; spec §4 lists 3 durable artifacts (structured amendment record / spec edit + commit / journal entry) but does not characterize phase-by-phase form. Adjacent to N=2 D-3 / N=3 D-3 (D-3).
- **§12 Out of Scope walked against SKILL.md.** All 14 §12 items either name §13 OQs (carried open) or methodology-wide gaps. SKILL.md makes no claim on any §12 item. Clean.

### Cross-skill pattern observations (queued for closing summary)

Summarized at audit close; full detail in the [batch journal N=6 entry](../20260518-cp2-batch-audit/journal.md).

- **ASPP citation discipline (N=6 confirmation; six-spec sweep clean).** Holds across the entire legacy quintet plus N=1 baseline. Discipline is not broken anywhere.
- **Session-economy commitment propagation — final closure verified.** spec-amend's single shape (i) §5-enumerated mapping (three additions) verified. Full session-economy propagation across the quintet now mapped: spec-execute (four mappings, shapes (i)+(ii)); spec-review (one mapping, two additions, shape (i) only); spec-amend (one mapping, three additions, shape (i) only). **Every session-economy §5 subsection's prescribed SKILL.md addition is present and consistent in its target skill.**
- **Two-source structure — six-spec sweep.** Predecessor + sibling-design-spec structure introduced at N=4; spec-amend extends with the inline-not-standalone predecessor variant. Pattern terminal at N=6.
- **Section-heading citation discipline (N=6 confirmation).** Holds.
- **Amendment-ID citation correctness post-amendment-2026-05-18-3 confirmed at spec-amend endpoint.** Both endpoints of the first post-trilogy cross-skill amendment cycle (N=5 §5.11 + N=6 §5.9) clean at CP-2.
- **"WHAT NOT TO DO partial home" finding class — six consecutive data points (D-1 at N=6).** N=1 D-4 / N=2 D-4 / N=3 D-1 / N=4 D-1+D-2 / N=5 D-3 / N=6 D-1. **Fully stable across the quintet.** Discipline-articulation patch is the standard remedy.
- **SKILL.md preamble-vs-body mirror class — six consecutive data points (D-2 at N=6).** N=2 D-2 / N=3 D-5 / N=4 D-3+D-4 / N=5 D-4 / N=6 D-2. **Fully stable.** N=6 D-2 introduces a third flavor (preamble-omits-sibling-caller while frontmatter + HANDOFF NOTES name it).
- **SKILL.md OUTPUT FORMAT items absent from spec — three data points (D-3 at N=6).** N=2 D-3 / N=3 D-3 / N=6 D-3. Pattern recurs after two-session pause (N=4/N=5 non-fire). Lower frequency than WND-partial-home / preamble-vs-body but real.
- **New SKILL.md internal stale-citation class (N=5 D-1) did NOT fire at N=6.** "Notes on what makes this skill load-bearing" §"Visibility is the point" correctly historizes pre-trilogy. Class confirmed not universal across the quintet.
- **Operator (c)→(a/b) override pattern — load-bearing at N=3/N=4/N=5/N=6 (four consecutive sessions).** Both override candidates (D-1, D-3) confirmed (a) by operator via AskUserQuestion; D-2 routed (b) directly. **Recommended-discipline holds.**
- **Status-banner-lifecycle finding class (N=2 D-1) — three consecutive non-fires (N=4/N=5/N=6).** Class confirmed as spec-design-specific.
- **Cross-skill amendment coordination (§13 OQ-4) — first post-trilogy cycle's CP-2 endpoint cleanly verified.** Amendment 2026-05-18-3's worked-example evidence is anchored with verified-clean endpoints; one cycle anchors the watch item, two cycles would justify codifying convention (per [N=6 amendment 2026-05-18-3 entry](#L290) "Cross-skill note — codification candidate for SKILL.md").

### Closing-summary handoff

This is the final retroactive-spec CP-2. With N=6 close, the batch journal moves into its closing-summary phase per [batch journal "Closing summary" section](../20260518-cp2-batch-audit/journal.md). The closing summary consumes:
- Per-session entries (N=2 / N=3 / N=4 / N=5 / N=6) — all five complete.
- Cross-cutting amendment opportunities (per [strategy OQ-3](../../docs/retroactive-spec-strategy.md)) — two preamble/frontmatter-consistency edits across the quintet (N=4 D-3 + N=5 D-4 + N=6 D-2 = three SKILL.md preamble amendments across the quintet); design-notes-or-stale-citation pattern at N=5 D-1 only.
- Final routing tally — pending application of D-1, D-2, D-3 amendments this session.
- Readiness verdict for `docs/retroactive-spec-pattern.md` per [§11 step 4](./architecture.md).

## 2026-05-18 — Amendment 2026-05-18-4

**Section amended:** [specs/20260518-spec-amend-skill/architecture.md](./architecture.md) §6 (Non-functional Requirements table — new "Propose/apply separation discipline" row inserted between "Multi-repo discipline" and "Visibility").
**Trigger:** N=6 CP-2 audit finding D-1 ([spec journal "Review of CP-2"](#L333), [batch journal N=6 entry](../20260518-cp2-batch-audit/journal.md)) — SKILL.md WND-8 "Do not let `spec-execute` or `spec-review` silently apply amendments. Both should propose; this skill applies. The separation is what makes amendments visible." lacks explicit §5/§6 carrier. WND-partial-home class 6th consecutive data point (N=1/N=2/N=3/N=4/N=5/N=6 — six-for-six). Operator chose route (a) amend spec §6 over reviewer-default (c) at audit close per N=3/N=4/N=5 (c)→(a) override pattern (Recommended selected).
**Reason:** The propose/apply separation between upstream callers and this skill is the trilogy commit's load-bearing architectural change. It is carried in §3 Background (trilogy framing) and §3 Dependencies (the spec-review entry repeats it) but is not first-class in §5 phase rules or §6 NFRs. Landing it as a §6 NFR row matches the discipline-articulation patch shape used at N=1 D-4 / N=2 D-4 / N=3 D-1 / N=4 D-1+D-2 / N=5 D-3 — five prior data points all routed (a) amend spec §5 or §6.
**Impact summary:** No tasks; spec-amend CP-2 verdict closes "pass with comments" on application; one of three batched amendments this session. No completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`Draft — Open for Review` post-CP-1 → post-CP-2). New §6 NFR row is a discipline-articulation patch landing an existing SKILL.md WND item; not an architectural change to the skill design. Per the auto-memory directive on status-implication explicitness: this amendment does not require re-approval; same shape as N=5 amendment 2026-05-18-4 (which also landed a WND item into spec §5.1).
**Commit:** `40527df`

### Full record

## Amendment 2026-05-18-4 — spec-amend §6 (new NFR row: Propose/apply separation discipline)

**Trigger.** N=6 CP-2 drift audit (this session) found that SKILL.md WND-8 "Do not let `spec-execute` or `spec-review` silently apply amendments. Both should propose; this skill applies. The separation is what makes amendments visible." has no explicit §5 phase rule or §6 NFR carrier in the spec body, despite being carried in §3 Background ("The separation between *proposing* (spec-execute) and *applying* (spec-amend) is the trilogy commit's load-bearing architectural change") and §3 Dependencies (the spec-review entry: "The reviewer *proposes* amendments; this skill *applies* them. The separation is what makes amendments visible."). The WND-partial-home class is the most stable cross-skill pattern in the batch — six consecutive sessions, six findings; each previously routed (a) amend spec.

**Section.** §6 Non-functional Requirements, new table row inserted between "Multi-repo discipline" and "Visibility" (approximately line 241).

**Change.**

Before:
> *(no row existed; the table jumped from "Multi-repo discipline" directly to "Visibility")*

After:
> | **Propose/apply separation discipline** | Amendments are *proposed* by upstream callers ([spec-execute](../../.agents/skills/spec-execute/SKILL.md) Phase 3/4 halt; [spec-review](../../.agents/skills/spec-review/SKILL.md) Phase 7 verdict's "Spec amendments proposed" section; operator observation) and *applied* only by this skill. Sibling skills do not silently apply spec changes; the separation between *proposing* and *applying* is the trilogy commit's load-bearing architectural change (§3 Background) and is what makes amendments visible as first-class events. | [SKILL.md WND-8 + Phase 4](../../.agents/skills/spec-amend/SKILL.md); [§3 Background and Dependencies](#3-background-and-constraints) |

**Reason.** WND-8 is the only WND item in spec-amend SKILL.md that lacked a §5/§6 carrier at CP-2 audit time. Landing it as a §6 NFR row matches the discipline-articulation patch shape used five times before in the batch (N=1 D-4 / N=2 D-4 / N=3 D-1 / N=4 D-1+D-2 / N=5 D-3). The row names the three upstream callers (spec-execute halt, spec-review verdict, operator observation), cites WND-8 and Phase 4 as primary SKILL.md sources, and back-references §3 for the architectural rationale.

**Impact.**
- **Affected tasks:** none (design spec, no atomic tasks).
- **Affected checkpoints:** CP-2 (closes "pass with comments" upon application of all three batched amendments).
- **Completed work invalidated:** No.
- **Cross-references requiring follow-up:** None. §3 already carries the rationale; the new NFR row cross-references §3 explicitly.

**Status implication.** **kept** (`Draft — Open for Review`). Discipline-articulation patch; not an architectural change. Same shape as N=5 amendment 2026-05-18-4 (WND-5 landed at §5.1) and N=4 amendments 2026-05-18-3/-4 (WND-9/§5.6 count fix).

**Approver.** Eric Wasgatt, 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-5

**Section amended:** [specs/20260518-spec-amend-skill/architecture.md](./architecture.md) §4 Output topology — paragraph added after the three-artifact list mapping artifacts to phases by output form.
**Trigger:** N=6 CP-2 audit finding D-3 ([spec journal "Review of CP-2"](#L333), [batch journal N=6 entry](../20260518-cp2-batch-audit/journal.md)) — SKILL.md OUTPUT FORMAT block (lines 165–170) carries per-phase manifestation rules (Phases 1–3 conversational; Phase 4 Edit + commit; Phase 5 Edit; Phase 6 conversational) absent from spec §4 Output topology. SKILL.md OUTPUT FORMAT-absent-from-spec class 3rd data point (N=2 D-3 / N=3 D-3 / N=6 D-3). Operator chose route (a) amend spec §4 over reviewer-default (c) at audit close per N=2/N=3 routing precedent for the same class (Recommended selected).
**Reason:** Spec §4 captured the three durable artifacts but did not characterize phases-by-output-form. SKILL.md OUTPUT FORMAT block adds this characterization; the spec should match.
**Impact summary:** No tasks; one of three batched amendments at this CP-2 close. No completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (`Draft — Open for Review`). Output-topology articulation patch; not an architectural change.
**Commit:** `3ba4d77`

### Full record

## Amendment 2026-05-18-5 — spec-amend §4 (Output topology: per-phase manifestation paragraph)

**Trigger.** N=6 CP-2 drift audit (this session) found that SKILL.md OUTPUT FORMAT block (lines 165–170) carries per-phase manifestation rules absent from spec §4 Output topology. Spec §4 enumerates 3 durable artifacts but does not characterize phase-by-phase output form. The class is the same one routed (a) at N=2 D-3 and N=3 D-3 — the third data point of the SKILL.md OUTPUT FORMAT-absent-from-spec class.

**Section.** §4 Architecture, "Output topology" sub-section, paragraph added after the existing "When `SPEC_REPO_ROOT` is set..." sentence (approximately line 82).

**Change.**

Before:
> When `SPEC_REPO_ROOT` is set, all three artifacts land in the spec repo, paired with whatever code-side state prompted the amendment.

After:
> When `SPEC_REPO_ROOT` is set, all three artifacts land in the spec repo, paired with whatever code-side state prompted the amendment.
>
> The three artifacts map onto the six phases by output form: **Phases 1–3 are conversational** (Orient produces the Orientation Report; Draft produces the structured amendment record presented to the approver; Approval produces an explicit yes/revisions/no/reclassify response). **Phase 4 produces an Edit to the spec and a commit** (artifact 2 above lands here). **Phase 5 produces an Edit to the journal** (artifact 3 above, with the structured amendment record from Phase 2 pasted into the "Full record" sub-block — artifact 1 lands here). **Phase 6 is conversational, with explicit names for downstream actors** (implementer / reviewer / next session / re-approver). The mapping makes the per-phase work shape predictable for both the agent executing the skill and the operator reviewing its outputs.

**Reason.** SKILL.md OUTPUT FORMAT block is the procedural meta-commitment characterizing phase manifestation. Spec §4 needs to carry it so the spec and SKILL.md tell the same story about how phases produce outputs. The new paragraph also ties the three durable artifacts to specific phases, making the artifact-to-phase mapping unambiguous.

**Impact.**
- **Affected tasks:** none (design spec, no atomic tasks).
- **Affected checkpoints:** CP-2 (closes "pass with comments" upon application of all three batched amendments).
- **Completed work invalidated:** No.
- **Cross-references requiring follow-up:** None. §5 phase subsections already describe per-phase behavior; the new §4 paragraph references them without requiring §5 edits.

**Status implication.** **kept** (`Draft — Open for Review`). Output-topology articulation patch; not an architectural change. Same shape as N=2 amendment that resolved D-3 (Markdown hygiene NFR) and N=3 amendment that resolved D-3 (§6 amendment).

**Approver.** Eric Wasgatt, 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-6

**Section amended:** [.agents/skills/spec-amend/SKILL.md](../../.agents/skills/spec-amend/SKILL.md) preamble line 15 ("How this skill works").
**Trigger:** N=6 CP-2 audit finding D-2 ([spec journal "Review of CP-2"](#L333), [batch journal N=6 entry](../20260518-cp2-batch-audit/journal.md)) — SKILL.md preamble line 15 "another skill — `spec-execute`, `spec-review`, or an in-flight `spec-design` session" omits `spec-write` from the in-flight invocation list, while frontmatter line 4 names all four siblings and HANDOFF NOTES line 187 names spec-write explicitly. Preamble-vs-body mirror class 6th consecutive data point (N=2/N=3/N=4 ×2/N=5/N=6) — new flavor (preamble-omits-sibling-caller, third flavor of the class). Operator confirmed (b) amend SKILL.md preamble at audit close.
**Reason:** Internal SKILL.md inconsistency. The preamble's invocation list should match the frontmatter pairing and HANDOFF NOTES caller list. Adding `spec-write` to the in-flight session list closes the gap.
**Impact summary:** No tasks; SKILL.md preamble correction; one of three batched amendments at this CP-2 close. No completed work invalidated.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (SKILL.md has no Status banner; preamble correction is internal-consistency patch). The amended skill applies the amendment to itself (self-referential per §11) under the staged-bootstrap pattern: the current (pre-amendment) workflow applied to produce the next (post-amendment) state. No re-approval cycle needed.
**Commit:** `b6fb958`

### Full record

## Amendment 2026-05-18-6 — spec-amend SKILL.md preamble (line 15 invocation list)

**Trigger.** N=6 CP-2 drift audit (this session) found that SKILL.md preamble line 15 ("How this skill works") omits `spec-write` from the in-flight invocation list. Frontmatter description (line 4) names all four siblings: "Pairs with `spec-write` and `spec-design` (the spec being amended), `spec-execute` (which surfaces drift), and `spec-review` (which may propose amendments rather than block)." HANDOFF NOTES (line 187) names spec-write explicitly: "From `spec-design` or `spec-write`. Once the design or feature spec leaves Draft and is committed, further changes go through this skill." Preamble was the lone outlier; preamble-vs-body mirror class 6th consecutive data point with a new flavor (preamble-omits-sibling-caller).

**Section.** SKILL.md "How this skill works" sub-section, line 15.

**Change.**

Before:
> When invoked, you act as the agent. The user (or another skill — `spec-execute`, `spec-review`, or an in-flight `spec-design` session) has identified that a spec needs to change. Your job is to structure the proposal, drive approval, apply the change, and record the amendment in the journal so it survives context decay.

After:
> When invoked, you act as the agent. The user (or another skill — `spec-execute`, `spec-review`, or an in-flight `spec-design` or `spec-write` session) has identified that a spec needs to change. Your job is to structure the proposal, drive approval, apply the change, and record the amendment in the journal so it survives context decay.

**Reason.** The preamble's invocation list is part of the skill's "how to read this file" framing — it tells the reader who calls this skill. The frontmatter and HANDOFF NOTES already name spec-write among the callers; the preamble should match. The fix is a single-clause addition (`or spec-write`) preserving the existing rhythm of the sentence.

**Impact.**
- **Affected tasks:** none.
- **Affected checkpoints:** N=6 CP-2 (closes "pass with comments" upon application of all three batched amendments).
- **Completed work invalidated:** No.
- **Cross-references requiring follow-up:** None. Frontmatter and HANDOFF NOTES already name spec-write; the preamble is the only place where the gap existed.

**Status implication.** **kept** (SKILL.md has no Status banner). Self-referential amendment (the amendment skill amending its own SKILL.md) under the staged-bootstrap pattern named at §11 — the current (pre-amendment) workflow applies the amendment to produce the next (post-amendment) state. The change is small (single clause) and the workflow does not change; no re-approval cycle needed.

**Approver.** Eric Wasgatt, 2026-05-18.

### Cross-skill note — self-referential amendment to spec-amend SKILL.md (§11 staged-bootstrap)

This amendment is the **first observed amendment to spec-amend SKILL.md itself** post-trilogy. Per §11 Adoption Path, "amendments to the amendment skill operate under the skill's *current* (pre-amendment) workflow, applied to the proposed *next* (post-amendment) state." This session realized that pattern in practice: the SKILL.md preamble correction was drafted, approved, applied, and journaled under the same six-phase workflow defined by SKILL.md (Phase 1 Orient via CP-2 audit; Phase 2 Draft via structured-amendment-record block above; Phase 3 Approval via operator's AskUserQuestion response at audit close; Phase 4 Apply via single-line edit + commit `b6fb958`; Phase 5 Journal via this entry; Phase 6 Downstream Handoff = none — the change is purely textual, no implementer / reviewer / next session impact beyond reading the corrected text). **No staged-bootstrap friction observed.** §10 risk row "Self-referential amendment paradox" anchored to first worked example.
