# `project-constitution` Skill — Journal

This journal is structured for **N=2 mining**. The four sibling legacy-trilogy skills (`spec-design`, `spec-write`, `spec-execute`, `spec-review`, `spec-amend`) are in the same retroactive-spec position; future sessions authoring those specs read this journal as prior art rather than re-deriving the choices. Section headings are stable across retroactive-spec journals on purpose: "Source-file selection," "Format choice," "Open-question framing," etc., are the slots a future N=2 session looks in.

## 2026-05-17 — Retroactive design spec authored

**Status:** draft — awaiting CP-1 review
**Artifact:** [architecture.md](./architecture.md)
**Companion:** [journal.md](./journal.md) (this file)
**Trigger:** `/spec-review the project-constitution skill` was invoked; the reviewer stopped at Phase 1 orientation because no spec or Review Checkpoint existed. Operator chose retroactive-spec authoring as the path forward and selected `/spec-design` over `/spec-write`.

### Source-file selection (decision + rationale)

| File | Used? | Rationale |
|---|---|---|
| [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) (== `.claude/skills/...` mirror, byte-identical) | Yes — authoritative for behavior | The skill itself. Every commitment in the spec must trace back to behavior in this file. |
| [specs/tech-stack.md](../tech-stack.md) | Yes — authoritative for constraints | The Atomic-Skill Portability Principle lives here and binds the skill. User confirmed content was "largely absorbed into tech stack" from an architecture doc that does not exist. |
| [specs/mission.md](../mission.md) | Yes — authoritative for audience and scope context | Anchors the spec's framing of who the skill serves. |
| [specs/roadmap.md](../roadmap.md) | Yes — authoritative for lifecycle position | Confirms `project-constitution` as a Phase 1 deliverable; positions the skill in the methodology's evolution. |
| [docs/constitution-amendment-gap-intake-prep.md](../../docs/constitution-amendment-gap-intake-prep.md) | Yes — known-gap signal only | Source of §13 OQ-1 content. The gap is named in the spec; the intake action is *out of session* per operator. |
| [specs/20260514-session-economy/architecture.md](../20260514-session-economy/architecture.md) | Negative signal — explicitly NOT a source | Lists `project-constitution` as out-of-scope twice ([line 27](../20260514-session-economy/architecture.md#L27), [line 220](../20260514-session-economy/architecture.md#L220)). Confirmed the absence of prior architectural specification. |
| External repo (`private-design-repo`) | No — operator confirmed stay within ai-tools | Surfaced as a candidate during orientation; ruled out before /spec-design invocation. |

**Pattern for N=2.** Source-file selection for retroactive specs should explicitly include a *negative-signal* row — what was checked and ruled out. Future skills will have their own out-of-scope mentions in concurrent architecture docs that should be cited as "confirmed not the source" rather than silently ignored.

### Format choice — design spec vs. feature spec

The two existing skill-as-spec examples ([finding-intake-skill](../20260517-finding-intake-skill/feature.md), [finding-triage-skill](../20260517-finding-triage-skill/feature.md)) used `/spec-write` and produced feature specs with atomic task breakdowns (T-01, T-02, …).

This spec used `/spec-design` and produced a design spec instead. Operator-driven choice ("Let's start with /spec-design"). The deviation is principled:

- The finding-* skills were spec'd **before** they were built. Feature spec + task breakdown was the right shape because the work was implementation.
- `project-constitution` is being spec'd **after** it ships. The work is *describing what is committed*, not planning implementation. Design spec is the right shape because the artifact is a commitment to vocabulary, contract, and principles — not a development plan.

**Pattern for N=2.** Retroactive specs for already-shipping skills use `/spec-design`, not `/spec-write`. If the skill ships and works, the spec's job is to declare commitments, not decompose tasks. A retroactive feature spec would force fabrication of T-01-style tasks for code that already exists; nobody benefits.

### Naming pattern — directory slug

| Candidate | Chosen? | Rationale |
|---|---|---|
| `specs/20260517-project-constitution/architecture.md` | No | Matches the skill name exactly but doesn't disambiguate the skill from the actual constitution docs (`mission.md`, `tech-stack.md`, `roadmap.md`) that live at `specs/`. |
| `specs/20260517-project-constitution-skill/architecture.md` | **Yes** | Matches sibling pattern (`finding-intake-skill`, `finding-triage-skill`). The `-skill` suffix is the disambiguator: this spec is about *the skill*, not about *a project's constitution*. |

**Pattern for N=2.** Retroactive specs for skills use the directory slug `YYYYMMDD-<skill-name>-skill`. Matches existing convention; no surprise to future readers.

### Audience framing

User-supplied verbatim: "Maintainers of the ai-tools methodology skills (Eric + future contributors + future AI agents picking up the skill set)." Adopted without modification.

**Pattern for N=2.** Retroactive specs share this audience. It can be reused verbatim; the audience does not vary skill-to-skill because the methodology's reader set does not vary skill-to-skill.

### Verification commitment level

Discussed in Phase 2: this skill makes no external claims requiring WebFetch (standard manifest filenames are common-knowledge; the Atomic-Skill Portability Principle is internal-only). Adopted **light verification**: inline-cite repo-internal sources at point of claim; no external verification needed.

**Pattern for N=2.** The other four legacy-trilogy skills also make minimal external claims (they describe workflow conventions, not external systems). Light verification will likely be appropriate for all of them. If a future retroactive-spec session targets a skill that *does* invoke external tools or cite RFCs, the verification commitment must escalate explicitly.

### Open-question framing — handling known gaps

**Decision:** the constitution-amendment workflow gap surfaces as §13 OQ-1 with full analysis, not §12 Out of Scope or silent omission. Operator selected the §13 framing.

**Rationale:** the gap is a real property of the skill (the skill does not address amendment workflow). A spec silent on the gap would be dishonest about the skill's coverage. §12 Out of Scope would be lighter but loses the four-options analysis that the pre-intake notes already produced. §13 captures the analysis at first-class detail, with the owner pointing at the *pending* finding intake — making clear that resolution lives elsewhere, not in this spec.

**Subtle point:** the operator separately ruled the finding-intake action *out of session*. The spec's §13 names the gap; this session does not file the intake. The two decisions are independent. A spec can describe a known gap without triggering its resolution.

**Pattern for N=2.** Retroactive specs for the other four skills should scan for known gaps before authoring. Gaps from existing finding-prep docs, parked OQs in sibling specs, or operator memory get a §13 entry with owner pointing at the appropriate downstream artifact. The "name the gap, don't resolve it" discipline is what makes retroactive specs honest descriptions rather than aspirational ones.

### Drift-audit-as-checkpoint

**Decision:** declared CP-2 in §9 — "Drift audit complete" — as a first-class checkpoint, not just a step in §11 Adoption Path.

**Rationale:** the retroactive spec's central risk is silent divergence from the shipping SKILL.md. Making the drift audit a named gate (a) ensures it cannot be skipped, (b) gives `/spec-review` something concrete to anchor against at the second invocation, and (c) sets expectation that *every* SKILL.md edit going forward routes through `/spec-amend`. CP-1 + CP-2 collapse the adoption work into two clearly-scoped reviews.

**Pattern for N=2.** Every retroactive spec should declare both CP-1 (faithfulness review) and CP-2 (drift audit) in §9. The two checkpoints serve different purposes and require different review focus; one combined checkpoint blurs both.

### Scope discipline — what was kept out

The operator's instruction "I do not want to create drift in the effectiveness of our pre-existing skills" produced several explicit non-goals captured in §2:

- The spec does **not** declare itself a template for the four sibling retroactive specs. Templates emerge from N=2 observation; declaring one from N=1 risks foreclosing better patterns.
- The spec does **not** resolve the constitution-amendment gap. Naming ≠ resolving.
- The spec does **not** redesign the skill. Redesign would route to a new design spec.
- The spec does **not** modify the shipping SKILL.md. The CP-2 audit identifies divergences; `/spec-amend` is the only mechanism that touches SKILL.md.

**Pattern for N=2.** Retroactive specs are descriptive, not prescriptive. The §2 Non-goals list should explicitly include "redesign of the skill" and "modification of the shipping SKILL.md" — the no-drift discipline must be made textual to be enforceable.

### Cross-session knowledge transfer — explicit deferral of docs/ scaffolding

The operator offered to allow `docs/` scaffolding this session for future-session ease. Recommended deferral to N=2 was accepted, *conditional on this journal being mineable*. That conditional is the load-bearing constraint on this journal's structure.

**What this journal commits to:**

- Stable section headings across retroactive-spec journals (so an N=2 author finds the same slot in this one).
- Explicit "Pattern for N=2" callouts under each major decision, so the next session does not have to infer what is generalizable.
- Source-file selection rationale, not just the source-file list.
- Friction points and surprise moments recorded honestly (see "Friction observed" below).

**What this journal does NOT commit to:**

- Being a fillable template. That would over-commit from N=1.
- Naming the next skill to receive a retroactive spec. Operator decides per session.
- Pre-resolving the constitution-amendment gap or any other deferred concern.

**Pattern for N=2.** When the second retroactive spec is authored, read this journal first. The "Pattern for N=2" callouts above are the candidate generalizations; the N=2 session validates or rejects each. After N=2, if two or three callouts have proven correct, a `docs/retroactive-spec-pattern.md` is justified. Before N=2, it is premature.

### Friction observed

Honest record of where this session encountered friction. Useful for N=2 to anticipate.

- **Source-file ambiguity.** Operator's memory of "an architecture file made around the time we retroactively created the project-constitution skill" did not match git history — no such file ever existed. The session paused to walk the operator through the actual candidates (docs/spec-design-recommendations.md was the closest fit but wrong; the architecture content was absorbed into tech-stack.md). Cost: one round-trip. **N=2 mitigation:** start with the assumption that no standalone architecture doc exists for legacy skills; verify by `git log --diff-filter=A` before consulting operator memory.
- **Format choice was not in the skill's INPUTS.** `/spec-design` took the format as a given (design spec) because the operator invoked it that way. A more general "retroactive-spec" entrypoint would ask format choice as a Phase 2 question. Not actionable for this skill; flagged for future tooling consideration.
- **Open-question framing took a clarification round.** "Move the constitution amendment gap intake out of scope for this session" was ambiguous between "don't file the finding" and "don't mention the gap in the spec." The session asked explicitly; operator confirmed the former. **N=2 mitigation:** scope-deferral instructions from the operator should be parsed for action-scope vs. content-scope distinction; ask explicitly rather than assume.

### Conversation grounding

- Operator invoked via `/spec-review the project-constitution skill`; the reviewer's Phase 1 orientation produced "no spec exists" as the finding, surfaced four resolution options, and the operator selected "Author retroactive spec first" → "/spec-design."
- The `/spec-design` skill's Phase 1 (Discovery) read the four source files plus the negative-signal source; Phase 2 (Clarify) surfaced three open questions ([blocker] scope, [important] gap framing, [minor] CP-2 declaration); operator resolved all three before Phase 3 commenced.
- No `[blocker]` open questions remain. The session proceeded to Phase 3 (spec document authoring) and concluded with paired spec + journal commit (this entry).

### Tasks defined

None. This is a design spec, not a feature spec. The "next work" is review (CP-1) and audit (CP-2), declared in §9, not atomic dev tasks.

### Next action pointer

Two paths, operator choice:

1. **Immediate:** invoke `/spec-review` against this spec's CP-1 — closes the original `/spec-review the project-constitution skill` request that triggered this entire thread.
2. **Sequenced:** commit the spec and journal first; trigger CP-1 in a separate session with a fresh reviewer (cleaner separation between authorship and review).

No `[blocker]` open questions; the spec is ready for CP-1 either way.

## 2026-05-17 — Review of CP-1

**Reviewer:** Claude (agent reviewer)
**Outcome:** pass with comments
**Tasks reviewed:** N/A — design spec, no atomic tasks; reviewable scope was the architecture.md + journal.md artifact pair against the shipping [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md).
**Blockers:** 0
**Important:** 1 — Broken in-repo link target at [architecture.md §1](./architecture.md#L12) and [§14](./architecture.md#L305): `[.claude/skills/project-constitution/SKILL.md](../../.claude/skills/project-constitution/SKILL.md)` points at a path that does not exist in the repo (`.claude/` contains only `settings.local.json`). The byte-identical mirror referenced by the body is real, but lives at the user-global location `~/.claude/skills/project-constitution/SKILL.md`, which §6 NFR Portability correctly cites. Substance is correct; rendered link is broken.
**Advisory:** 3 — (a) §5.2 "Six topics" merges assumptions + open questions; SKILL.md enumerates seven bullets. (b) §5.3 captures only the journal half of layout-deviation handling; SKILL.md prescribes both journal-notation and constitution-doc documentation (split across §5.3 and §5.4 in the spec). (c) §9 CP-1 trigger phrasing ("are committed") doesn't match the journal's "Next action pointer" path 1 which permits CP-1 pre-commit.
**Spec amendments proposed:** 1 — revise §1 and §14 to either drop the repo-relative `.claude/skills/...` link or reword to clearly indicate the mirror is at the user-global location and not reachable via a repo-relative link. Route through `/spec-amend` per the spec's own Amendment Protocol.

**Findings against CP-1 review focus (all five items):**
- *Every commitment corresponds to behavior in SKILL.md.* pass with comments — two advisory descriptive choices (§5.2 count, §5.3 partial coverage), no substantive drift.
- *No commitment contradicts SKILL.md.* pass — none found.
- *Atomic-Skill Portability Principle correctly characterized in §3 and §6 against tech-stack.md §21-33.* pass — three operational halves correctly summarized; line citation verified.
- *Constitution-amendment gap (§13 OQ-1) named, not silently resolved.* pass — gap explicitly named with question, four-option analysis, no premature leaning, owner pointer to future `/finding-intake`.
- *Spec self-contained per spec-design Operating Principles.* pass with comments — banner, audience, vocabulary, structure all present; one important finding on the broken link target.

**Verification performed:**
- Byte-identical mirror claim: verified `diff -q` of `.agents/skills/project-constitution/SKILL.md` against `~/.claude/skills/project-constitution/SKILL.md`.
- Commit SHAs in §3: `49c15f0` (2026-05-14) and `a11119d` (2026-05-15) both verified via `git log`.
- Cross-references: `docs/constitution-amendment-gap-intake-prep.md`, `specs/mission.md`, `specs/tech-stack.md`, `specs/roadmap.md` (line 13 verified), `specs/20260514-session-economy/architecture.md` (lines 27 + 220 verified), `specs/20260517-finding-intake-skill/feature.md`, `specs/20260517-finding-triage-skill/feature.md`, `specs/20260517-findings-pipeline/architecture.md` — all exist.
- `specs/tech-stack.md` §21-33 confirmed to contain the Atomic-Skill Portability Principle.

**Pattern for N=2.** A retroactive-spec CP-1 review is mostly verification of *citations* and *traceability*, not behavioral judgment — the behavior is already shipping. The natural failure mode is broken or wrong references (as found here at §1/§14), not substantive misalignment. Future retroactive-spec CP-1 reviews should weight citation-walking heavily and reserve substance-checking for any place where the spec adds rationale or interpretation absent from the source SKILL.md.

**Next action:**
1. Operator invokes `/spec-amend` against §1 and §14 to fix the broken `.claude/skills/...` link target.
2. Commit the spec + journal + this CP-1 verdict + the amendment as a paired commit set, closing §11 Adoption Path step 1.
3. Trigger CP-2 (drift audit) per §9. CP-1 closure unblocks CP-2 per its declared trigger.

## 2026-05-17 — Amendment 2026-05-17-1

**Section amended:** specs/20260517-project-constitution-skill/architecture.md §1, §6, §9 (CP-1 review focus, fifth bullet), §14 (Authoritative + Inspirational subsections)
**Trigger:** CP-1 review (2026-05-17) flagged broken `.claude/skills/project-constitution/...` link target in §1 and §14 as `[important]`; operator established broader policy that the spec must contain no `~/.claude/skills/...` references at all.
**Reason:** Project's authoritative version of skills exists only under `.agents/skills/`. Spec must cite only `.agents/skills/...` locations; global-install paths managed by host tooling are not the spec's concern.
**Impact summary:** No tasks, no checkpoints invalidated. CP-1's `[important]` finding resolved (proposed-amendment route closed). CP-2 unaffected as a checkpoint; will see the amended text. No completed work affected.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-17
**Status implication:** kept (Draft — Open for Review)
**Commit:** 9d5263f

### Full record

#### Amendment 2026-05-17-1 — architecture.md §1, §6, §9, §14

**Trigger.** CP-1 review of this spec (2026-05-17) identified a broken in-repo link target in §1 and §14 (`.claude/skills/project-constitution/SKILL.md` does not exist in the repo). The operator subsequently established a broader policy: the spec must contain **no** references to `~/.claude/skills/...` paths (whether absolute, repo-relative, or as parenthetical examples). The project's authoritative version of skills exists only under `.agents/skills/`. Phase 4 verification grep surfaced two additional violations referencing `.claude/skills/spec-design/SKILL.md`; the operator approved extending the amendment scope (option a) to cover them as the same coherent change.

**Section.** Five coherent edits across the same policy:
- §1 Overview ([architecture.md:12](./architecture.md#L12)) — removed byte-identical-mirror clause.
- §6 NFR Portability row ([architecture.md:171](./architecture.md#L171)) — rephrased to omit `~/.claude/skills/...` global-install example.
- §9 CP-1 review focus, fifth bullet ([architecture.md:216](./architecture.md#L216)) — swapped `.claude/skills/spec-design/...` → `.agents/skills/spec-design/...`.
- §14 Authoritative, second bullet ([architecture.md:304-305](./architecture.md#L304-L305)) — removed `.claude/skills/project-constitution/...` mirror entry.
- §14 Inspirational, first bullet ([architecture.md:313](./architecture.md#L313)) — swapped `.claude/skills/spec-design/...` → `.agents/skills/spec-design/...`.

**Change.**

*§1 Overview*

Before:
> This document is a **retroactive design specification**: the skill already ships at [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) and is mirrored byte-identical to [.claude/skills/project-constitution/SKILL.md](../../.claude/skills/project-constitution/SKILL.md). The spec describes what the skill *is* and what it *commits to*…

After:
> This document is a **retroactive design specification**: the skill already ships at [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md), which is authoritative for the skill's behavior. The spec describes what the skill *is* and what it *commits to*…

*§6 NFR Portability row*

Before:
> | **Portability** | Skill functions installed globally (e.g. `~/.claude/skills/project-constitution/`) and in-repo (`.agents/skills/`). No runtime dependency on host-repo files for the skill itself to function. Schema and templates are bundled in SKILL.md. | [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) |

After:
> | **Portability** | Skill functions when installed at `.agents/skills/project-constitution/` and works against unrelated host repos that lack methodology-specific siblings (degrades cleanly). No runtime dependency on host-repo files for the skill itself to function. Schema and templates are bundled in SKILL.md. | [Atomic-Skill Portability Principle](../tech-stack.md#L21-L33) |

*§9 CP-1 review focus, fifth bullet*

Before:
> - The spec is self-contained per the Operating Principles in [.claude/skills/spec-design/SKILL.md](../../.claude/skills/spec-design/SKILL.md).

After:
> - The spec is self-contained per the Operating Principles in [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md).

*§14 Authoritative, second bullet*

Before:
> - [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — the shipping skill. Authoritative for behavior.
> - [.claude/skills/project-constitution/SKILL.md](../../.claude/skills/project-constitution/SKILL.md) — global-install mirror; byte-identical to the in-repo copy.

After:
> - [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — the shipping skill. Authoritative for behavior.

*§14 Inspirational, first bullet*

Before:
> - [.claude/skills/spec-design/SKILL.md](../../.claude/skills/spec-design/SKILL.md) — the skill that authored this spec; its OUTPUT FORMAT and OPERATING PRINCIPLES are the structural source.

After:
> - [.agents/skills/spec-design/SKILL.md](../../.agents/skills/spec-design/SKILL.md) — the skill that authored this spec; its OUTPUT FORMAT and OPERATING PRINCIPLES are the structural source.

**Reason.** The original §1 and §14 Authoritative text introduced two problems: (a) a broken in-repo link target (`.claude/skills/project-constitution/` does not exist in this repo — only `.agents/skills/project-constitution/` does), and (b) confusion of methodology-canonical paths with deployment artifacts. The §6 NFR named a specific global-install path that conflicts with operator policy. The §9 and §14 Inspirational spec-design references were broken in the same way and required swap-to-canonical paths. Per operator policy, the spec references only the project-canonical `.agents/skills/...` location; global-install paths managed by host tooling are not the spec's concern. The portability NFR is preserved by stating the functional commitment (works against unrelated host repos, degrades cleanly) without naming a specific install path.

**Impact.**
- **Affected tasks:** none (design spec, no atomic task breakdown).
- **Affected checkpoints:**
  - **CP-1.** The `[important]` finding that drove this amendment is resolved. CP-1's `pass with comments` verdict stands; the proposed-amendment route is now closed.
  - **CP-2.** Unaffected as a checkpoint — drift audit's scope is unchanged. Audit will see the amended text; this is the intended state.
- **Completed work invalidated:** none.
- **Cross-references requiring follow-up:** none in this spec. **[separately tracked, not part of this amendment]** [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) (cited by this spec) still names `~/.claude/skills/<name>/` as an AS-PP example at [tech-stack.md:27](../tech-stack.md#L27); applying the same policy to tech-stack.md would be a separate amendment against a different artifact.

**Status implication.** Spec remains `Draft — Open for Review`. The amendment is surgical (five localized text replacements addressing one policy), does not modify any commitment in §3, §4, §5, §10, §11, §12, or §13, and does not change the spec's substance — only its choice of cited paths. No revert to Draft is needed.

**Approver.** Eric Wasgatt — approved initial three-section scope (§1, §6, §14 Authoritative) as drafted 2026-05-17; approved scope extension (option a) covering §9 and §14 Inspirational locations 2026-05-17.

### N=2 mining note

**Pattern for N=2.** When a retroactive-spec amendment removes a class of references (here: `.claude/skills/...`), Phase 1 Orient must scan the *entire spec* for all instances of the class, not just the locations called out by the triggering finding. The CP-1 finding here named §1 and §14, but two more instances (§9, §14 Inspirational) referenced a different skill via the same path pattern and were missed in Orient. Caught at Phase 4 verification grep. Future retroactive-spec amendments touching path/citation classes should add a pre-draft `grep` step to Phase 1 to surface all instances before scope is locked.

## 2026-05-18 — Review of CP-2

**Reviewer:** Claude (agent reviewer)
**Outcome:** pass with comments
**Tasks reviewed:** N/A — design spec, no atomic tasks; CP-2 audit scope was architecture.md §4, §5, §6, §12 against shipping [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md).
**Blockers:** 0
**Important:** 0
**Advisory:** 4 — D-1 §5.2 "Six topics" miscounts SKILL.md Phase 2's seven bullets; D-2 §6 layout-question Phase-2 placement vs SKILL.md Phase-3 prose positioning; D-3 spec lacks an INPUTS catalog mirroring SKILL.md's six-field INPUTS block; D-4 SKILL.md "do not duplicate or contradict existing READMEs" commitment absent from spec §6 / §5.3.
**Spec amendments proposed:** 2 routed to spec, 1 optionally routed to SKILL.md, 1 accept-as-minor — see below.

**Findings against CP-2 review focus (line-by-line audit of SKILL.md against spec §4, §5, §6, §12):**

| ID | Where | Divergence | Routing |
|---|---|---|---|
| D-1 | [architecture.md §5.2:122](./architecture.md#L122) vs [SKILL.md Phase 2:66-72](../../.agents/skills/project-constitution/SKILL.md#L66-L72) | Spec says "Six topics"; SKILL.md enumerates seven (Assumptions and Open questions are separate). Spec merges them. Carried forward from CP-1 advisory (a). | **(a) amend spec.** Substantive content unchanged; behavioral coverage already present in spec prose. Surgical text edit. |
| D-2 | [architecture.md §6 NFR Layout neutrality:177](./architecture.md#L177) vs [SKILL.md:78](../../.agents/skills/project-constitution/SKILL.md#L78) | Spec commits layout choice is surfaced "as a Phase 2 question"; SKILL.md positions the layout question inside Phase 3 prose, triggered by Phase 1 detection. | **(b) amend SKILL.md** to elevate layout to a Phase 2 bullet (spec's commitment is the cleaner contract), **or (c) accept as minor** since an executor would raise the question at the Phase 2/3 boundary either way. Operator discretion. |
| D-3 | [architecture.md §5.1:102](./architecture.md#L102) vs [SKILL.md INPUTS:21-28](../../.agents/skills/project-constitution/SKILL.md#L21-L28) | SKILL.md declares six INPUTS (REPO_ROOT, REPO_PURPOSE, TARGET_AUDIENCE, LIFECYCLE_STAGE, STACK_HINTS, SCOPE_HINTS); spec §5.1 cites only REPO_ROOT and narrates the remainder through §5.2 Phase 2 topics without cataloging them as the skill's INPUTS contract. | **(c) accept as known minor discrepancy.** Spec narrates equivalent ground; cataloging adds duplication for a descriptive retroactive spec. Rationale: the §5.2 Phase 2 walk *is* the spec's treatment of the Phase-2-supplied inputs; the SKILL.md INPUTS block is interface metadata more than behavioral commitment. |
| D-4 | [SKILL.md:196](../../.agents/skills/project-constitution/SKILL.md#L196) — no parallel in spec §6 or §5.3 | SKILL.md commits "Do not duplicate or contradict existing READMEs without addressing them. If a README is stale, either update it as part of this work or note that the constitution supersedes it." Spec has no parallel NFR or Phase 3 behavior. | **(a) amend spec** to add a README-reconciliation NFR row in §6 or a Phase 3 behavior line in §5.3. This is a real SKILL.md commitment the spec does not reflect. |

**Verification performed:**
- Walked all five spec §5 subsections against SKILL.md Phase 1 / Phase 2 / Phase 3 sections, Operating Principles, and WHAT NOT TO DO.
- Walked all seven §6 NFR rows against SKILL.md Operating Principles + WHAT NOT TO DO + Phase 3 preface.
- Walked all six §12 Out of Scope bullets — five are spec-level deferrals (no SKILL.md mirror expected); one (governance docs) matches SKILL.md WHAT NOT TO DO first bullet.
- Verified [specs/tech-stack.md §21-33](../tech-stack.md#L21-L33) (Atomic-Skill Portability Principle) source matches §3 and §6 NFR Portability characterizations.
- Cross-checked CP-1 advisories: (a) §5.2 count → reinstated here as D-1 with explicit routing; (b) §5.3 layout-deviation split → closed (§5.3 + §5.4 together cover both journal-notation and constitution-doc documentation, matching SKILL.md:76-78); (c) §9 CP-1 trigger phrasing → CP-1 internal, not CP-2 surface.

**Exit criteria status:**
- Divergence list produced (possibly empty): met — four advisory divergences.
- Each divergence has a routing decision: met — D-1 (a), D-2 (b)-or-(c), D-3 (c), D-4 (a).
- No silent edits to either artifact: met — this entry and the §9 Status line are the only writes; routing decisions await operator-invoked `/spec-amend`.
- Outcome recorded as closing entry of retroactive-spec adoption: met — this entry.

**Pattern for N=2.** A retroactive-spec CP-2 drift audit is structurally different from CP-1's faithfulness review. CP-1 verifies that the spec describes the skill; CP-2 enumerates the *gaps* where the description doesn't reach (commitments in SKILL.md absent from spec, or vice versa). The natural findings tier is **advisory** — substantive drift would have been caught at CP-1; CP-2 typically surfaces enumeration miscounts, missing NFR rows, missing INPUTS catalogs, and phase-positioning differences. Future CP-2 audits in the quintet batch should expect 2–5 advisory findings each, mostly of these shapes, with routing strongly biased toward (a) amend-spec (the SKILL.md is the canonical authority).

**Pattern for N=2.** CP-2 audits inherit CP-1 advisories that intersect the CP-2 surface (§4, §5, §6, §12). The CP-1 advisory may not have been acted on at CP-1 (advisories don't block); CP-2 is where the routing decision is forced. The N=5 batch should walk each prior CP-1 advisory list for items still relevant at CP-2 — they need explicit routing, not silent drop.

**Pattern for N=2.** When SKILL.md ships an INPUTS block but the spec narrates inputs through Phase 2 prose, the (c) accept-as-minor route is generally appropriate for retroactive specs — duplicating the contract adds maintenance burden without behavioral gain. Forward-authored specs (where the spec precedes the SKILL.md) should instead carry the INPUTS contract in spec form.

**Next action:**
1. Operator decides routing for D-2 (option b amend SKILL.md, or option c accept-as-minor) and confirms D-1 + D-4 amendments + D-3 acceptance.
2. Invoke `/spec-amend` for D-1 and D-4 (and D-2 if option b chosen). Each amendment is small and surgical; could be bundled or sequential per operator preference.
3. After amendments commit, the project-constitution retroactive-spec adoption (§11) is **closed**. The spec becomes the living contract for SKILL.md per the Amendment Protocol.
4. The five-spec batch CP-2 in [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) may now proceed using this audit as the N=1 baseline pattern.

## 2026-05-18 — Amendment 2026-05-18-1

**Section amended:** [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — frontmatter `lastUpdated`, Phase 2 bullet list, Phase 3 preface paragraph
**Trigger:** CP-2 drift audit finding D-2: spec §6 NFR Layout neutrality commits "Phase 2 question" but SKILL.md positioned the layout question in Phase 3 prose. Operator selected routing option (b) — amend SKILL.md.
**Reason:** The layout question requires operator input before Phase 3 file placement; it is structurally a Phase 2 activity. Elevating it to a Phase 2 bullet aligns SKILL.md with the spec's contract and tightens the workflow ordering.
**Impact summary:** No tasks (design spec). CP-2 D-2 closed. No completed work invalidated. SKILL.md cross-citation at [architecture.md §5.4:146](./architecture.md#L146) points to "SKILL.md:78" — content at that locus still exists (the constitution-documentation commitment) but the exact line number may have drifted; line-number citations into SKILL.md are inherently fragile and accepted as such here.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** SKILL.md has no Status banner; frontmatter `lastUpdated` advanced from 2026-05-15 to 2026-05-18.
**Commit:** TBD

### Full record

#### Amendment 2026-05-18-1 — SKILL.md Phase 2 elevates layout to an explicit bullet

**Trigger.** CP-2 drift audit (2026-05-18, this journal) enumerated four divergences between architecture.md and SKILL.md. Finding D-2: architecture.md §6 NFR Layout neutrality commits "Layout choice is surfaced as a Phase 2 question, not assumed" ([architecture.md:177](./architecture.md#L177)), but SKILL.md positioned the layout question in Phase 3 prose ([SKILL.md pre-amendment line 78](../../.agents/skills/project-constitution/SKILL.md)), not as a Phase 2 bullet. Per the spec's §11 Adoption Path, SKILL.md edits go through the Amendment Protocol; per the unresolved OQ-1 fallback, the journal entry lands in this spec's journal.

**Section.** Three coordinated edits in the same SKILL.md file:
- Frontmatter `lastUpdated` field — advanced.
- Phase 2 bullet list — new "Layout confirmation" bullet inserted between "In-scope / out-of-scope" and "Stack confirmation."
- Phase 3 preface paragraph — trimmed to remove the duplicated question text (which now lives in Phase 2), preserving only the constitution-document-the-exception half.

**Change.**

*Frontmatter*

Before:
> lastUpdated: 2026-05-15

After:
> lastUpdated: 2026-05-18

*Phase 2 bullet list — insert new bullet*

Before:
> - **In-scope / out-of-scope.** The non-goals list is doing real work — it stops the repo from accreting unrelated work. Capture explicit non-goals.
> - **Stack confirmation.** From Scan, list what was found. Ask the user to confirm or correct (occasionally a manifest is present but the actual stack is different — e.g., a `package.json` left from a prior life).

After:
> - **In-scope / out-of-scope.** The non-goals list is doing real work — it stops the repo from accreting unrelated work. Capture explicit non-goals.
> - **Layout confirmation.** When Phase 1 detected a non-default authoritative-artifacts directory (e.g., `docs/` already contains specs, or a `specifications/` folder exists), surface the choice: "The methodology recommends `specs/` for authoritative artifacts and `docs/` for supporting material. This repo has `<detected layout>`. Should I use the methodology's convention, adapt to the existing layout, or ask you to decide per-file?" Skip when scan detected the default `specs/` shape.
> - **Stack confirmation.** From Scan, list what was found. Ask the user to confirm or correct (occasionally a manifest is present but the actual stack is different — e.g., a `package.json` left from a prior life).

*Phase 3 preface paragraph*

Before:
> When the scan detects an existing directory structure that differs from the methodology's convention (e.g., `docs/` already contains specs, or a `specifications/` folder exists), surface the layout question to the operator: "The methodology recommends `specs/` for authoritative artifacts and `docs/` for supporting material. This repo has `<detected layout>`. Should I use the methodology's convention, adapt to the existing layout, or ask you to decide per-file?" When the operator chooses a non-default layout, document the exception in the constitution.

After:
> When the operator chose a non-default authoritative-artifacts layout in Phase 2, document the exception in the constitution (in `tech-stack.md` under "Conventions Outside the Stack — Repository layout") in addition to the journal entry of the originating session.

**Reason.** The layout question is a Phase 2 activity — it requires operator input before Phase 3 file placement. The pre-amendment positioning in Phase 3 prose worked but made the spec's "Phase 2 question" commitment unmappable to a specific bullet. Elevating it tightens SKILL.md's contract and aligns SKILL.md structure with the spec.

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-2 D-2 closed.
- Completed work invalidated: none.
- Cross-references requiring follow-up: architecture.md §5.4 cites "SKILL.md:78" — the cited content (constitution-document the exception) still exists post-amendment but the exact line number may have shifted slightly. Line-number citations into SKILL.md are inherently fragile; accepted as a known minor.

**Status implication.** SKILL.md has no Status banner. Frontmatter `lastUpdated` advanced from 2026-05-15 to 2026-05-18 as part of this amendment.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-2

**Section amended:** [architecture.md §5.2 Phase 2 — Clarify, Behavior paragraph](./architecture.md#L122)
**Trigger:** CP-2 drift audit finding D-1 (carried from CP-1 advisory (a)): spec said "Six topics" but SKILL.md Phase 2 enumerated discrete bullets. Post-Amendment 2026-05-18-1, SKILL.md Phase 2 has eight bullets; spec count needed to match.
**Reason:** Spec's enumeration must match SKILL.md Phase 2 (post-2026-05-18-1: eight bullets — lifecycle / purpose / audience / scope / layout / stack / assumptions / open-questions). Separating assumptions and open questions matches SKILL.md's structural distinction; including layout matches the 2026-05-18-1 elevation.
**Impact summary:** No tasks (design spec). CP-1 advisory (a) closed; CP-2 D-1 closed. No completed work invalidated. No cross-reference follow-up needed.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (Draft — Open for Review)
**Commit:** TBD

### Full record

#### Amendment 2026-05-18-2 — architecture.md §5.2 "Six topics" → "Eight topics"

**Trigger.** CP-2 drift audit (2026-05-18, this journal) finding D-1, carried from CP-1 advisory (a) which CP-2 routed to amendment. Spec's "Six topics" wording merged Assumptions and Open Questions into one bullet, undercounting SKILL.md's discrete enumeration. Amendment 2026-05-18-1 then added a "Layout confirmation" bullet to SKILL.md Phase 2, bringing the SKILL.md count to eight; the spec's count needs to match.

**Section.** [architecture.md §5.2 Phase 2 — Clarify, "Behavior" paragraph at line 122](./architecture.md#L122).

**Change.**

Before:
> **Behavior.** Six topics presented in order: lifecycle stage, purpose, audience, scope, stack confirmation, assumptions + open questions. The skill **stops and waits** for operator input. Open questions are triaged `[blocker]` / `[important]` / `[minor]`; blockers must be resolved before Phase 3.

After:
> **Behavior.** Eight topics presented in order: lifecycle stage, purpose, audience, scope, layout, stack confirmation, assumptions, open questions. The skill **stops and waits** for operator input. Open questions are triaged `[blocker]` / `[important]` / `[minor]`; blockers must be resolved before Phase 3.

**Reason.** Spec enumeration must match SKILL.md Phase 2 bullet count and order. The original "Six topics" wording merged assumptions with open questions (a count mismatch) and predated the layout bullet (now present after 2026-05-18-1). Behavior is unchanged in the spec — both assumptions and open questions were already in scope; layout is a separate amendment that the spec already commits to in §5.4 and §6.

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-1 advisory (a) closed (was open since 2026-05-17 CP-1 verdict); CP-2 D-1 closed.
- Completed work invalidated: none.
- Cross-references requiring follow-up: none.

**Status implication.** Kept (Draft — Open for Review). Surgical text fix.

**Approver.** Eric Wasgatt — approved 2026-05-18.

## 2026-05-18 — Amendment 2026-05-18-3

**Section amended:** [architecture.md §6 Non-functional Requirements](./architecture.md#L167-L179) — new "README reconciliation" row appended to the NFR table.
**Trigger:** CP-2 drift audit finding D-4: SKILL.md WHAT NOT TO DO commits "Do not duplicate or contradict existing READMEs without addressing them. If a README is stale, either update it as part of this work or note that the constitution supersedes it." Spec §6 NFRs and §5.3 had no parallel commitment.
**Reason:** The README-reconciliation rule is a real SKILL.md behavioral commitment that the spec failed to reflect. Adding the row makes §6 faithful to SKILL.md's contract and matches the table's pattern for SKILL.md-derived behavioral constraints (e.g., "No proliferation," "Forward orientation").
**Impact summary:** No tasks (design spec). CP-2 D-4 closed. No completed work invalidated. No cross-reference follow-up needed.
**Approver:** Eric Wasgatt
**Approved on:** 2026-05-18
**Status implication:** kept (Draft — Open for Review)
**Commit:** TBD

### Full record

#### Amendment 2026-05-18-3 — architecture.md §6 adds README-reconciliation NFR

**Trigger.** CP-2 drift audit (2026-05-18, this journal) finding D-4: SKILL.md WHAT NOT TO DO line at [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) commits "Do not duplicate or contradict existing READMEs without addressing them. If a README is stale, either update it as part of this work or note that the constitution supersedes it." Spec §6 NFRs and §5.3 Phase 3 behavior did not surface this commitment.

**Section.** [architecture.md §6 NFR table](./architecture.md#L169-L179) — append one new row after the "Layout neutrality" row.

**Change.**

Before (table ended with):
> | **Layout neutrality** | The skill detects and respects non-default authoritative-artifacts directories. Layout choice is surfaced as a Phase 2 question, not assumed.                                                                                                | [SKILL.md Phase 3 preface](../../.agents/skills/project-constitution/SKILL.md#L78) |

After (Layout neutrality row preserved; new README reconciliation row appended):
> | **Layout neutrality** | The skill detects and respects non-default authoritative-artifacts directories. Layout choice is surfaced as a Phase 2 question, not assumed.                                                                                                | [SKILL.md Phase 3 preface](../../.agents/skills/project-constitution/SKILL.md#L78) |
> | **README reconciliation** | The skill does not duplicate or contradict existing READMEs. When a relevant README is present, the operator is asked whether to update it as part of the constitution work or to note that the constitution supersedes it. Silent duplication is prohibited. | [SKILL.md WHAT NOT TO DO](../../.agents/skills/project-constitution/SKILL.md) |

**Reason.** The README-reconciliation rule is a real SKILL.md behavioral commitment (WHAT NOT TO DO line plus Phase 1 "existing docs" scan target) that the spec did not reflect in §6 NFRs or §5.3 Phase 3 behavior. Adding the row makes the spec faithful to SKILL.md and matches the table's existing pattern for SKILL.md-derived behavioral constraints (e.g., "No proliferation" cites WHAT NOT TO DO; "Forward orientation" cites Operating Principle 7).

**Impact.**
- Affected tasks: none.
- Affected checkpoints: CP-2 D-4 closed.
- Completed work invalidated: none.
- Cross-references requiring follow-up: none.

**Status implication.** Kept (Draft — Open for Review). Surgical addition.

**Approver.** Eric Wasgatt — approved 2026-05-18.

### CP-2 closeout

With Amendments 2026-05-18-1, 2026-05-18-2, and 2026-05-18-3 all approved and applied, CP-2 routing is complete:

- D-1 → resolved via 2026-05-18-2.
- D-2 → resolved via 2026-05-18-1 (option b).
- D-3 → accepted as known minor discrepancy (rationale in the CP-2 verdict above).
- D-4 → resolved via 2026-05-18-3.

The project-constitution retroactive-spec adoption (§11) is **closed**. The spec is now the living contract for SKILL.md per the Amendment Protocol. The five-spec batch CP-2 in [specs/20260518-cp2-batch-audit/journal.md](../20260518-cp2-batch-audit/journal.md) may proceed using this audit as the N=1 baseline pattern.

**Pattern for N=2.** Three surgical amendments emerged from one CP-2 verdict, sequenced to avoid count-churn (D-2-b first elevated SKILL.md to 8 Phase 2 bullets, then D-1 updated spec to "Eight topics" matching). Future CP-2 batches should screen for amendments that interact (one's "after" text depends on another's "after" state) and sequence accordingly. Three separate commits with three separate amendment IDs is appropriate when the findings share only a triggering verdict, not a policy theme (contrast 2026-05-17-1, which bundled five edits sharing the `.claude/skills/...` removal policy).
