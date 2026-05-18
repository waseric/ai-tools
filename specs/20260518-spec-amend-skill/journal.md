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
| §5.3 Phase 3 Approval | spec-execute §5.7 ("Phase 7 — Checkpoint Gate"); SKILL.md Phase 3 | spec-execute §5.7 verified as "Phase 7 — Checkpoint Gate" (verified to exist; explicit-approval discipline matches). SKILL.md Phase 3 lines 102–111 verified. |
| §5.4 Phase 4 Apply | session-economy §5.3 (Phase 4 paragraph); SKILL.md Phase 4 + Multi-repo block | session-economy §5.3 verified at lines 123–145; Phase 4 paragraph at line 141 verified verbatim. SKILL.md Phase 4 at lines 113–130; Multi-repo block at line 130 verified. |
| §5.5 Phase 5 Journal | session-economy §5.3 (Phase 5 note); SKILL.md Phase 5 lines 136–149 + multi-repo at 152 | session-economy §5.3 Phase 5 note verified at lines 143–145 verbatim. SKILL.md Phase 5 + multi-repo verified. |
| §5.6 Phase 6 Downstream Handoff | spec-execute §5.6; spec-review §5.8; SKILL.md Phase 6 lines 156–164 | **Citation error caught at walk:** original draft cited spec-execute §5.6 as "Phase 6 — Closeout" — verification against the actual heading showed it is **"Phase 6 — Update artifacts"** (closeout is the *purpose*; "Update artifacts" is the heading). Same correction for spec-review §5.8: actual heading is "Phase 8 — Update artifacts" (not "Update Artifacts"). Functional characterization (end-of-flow phase naming next actor) verified: spec-execute §5.6 Behavior point 3 = "Next-task pointer"; spec-review §5.8 conditional update 4 = "state the next task ID per the dependency graph." Spec body corrected before commit. **This is the per-citation walk discipline working as designed — catching the error at authoring time rather than at CP-1.** SKILL.md Phase 6 lines 156–164 verified. |
| §5.7 Change Classification | SKILL.md ROLE block lines 37–41 | Verified: lines 37–41 contain the trichotomy (amendment / in-flight edit / rewrite). |
| §5.8 Voice discipline | N=2 §5.6; N=3 (omitted); N=4 §5.10; N=5 §5.10 + amendment 2026-05-18-2 commit `85821ca` | All verified at authoring time. N=2 §5.6 = "Voice discipline" ✓; N=3 §5 has no Voice-discipline subsection ✓; N=4 §5.10 = "Voice discipline" ✓; N=5 §5.10 = "Voice discipline" (post-amendment) ✓; commit `85821ca` is the amendment commit ✓. |
| §5.9 Portability rule for links | Strategy-doc Amendment 2026-05-17-1; .agents/skills/spec-amend/SKILL.md path | Strategy-doc amendment verified. SKILL.md path verified. |

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
