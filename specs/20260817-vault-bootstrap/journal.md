# vault-bootstrap — Journal

## Current State
- **Phase:** CP-1 open. Revised 2026-08-18 on originating-session feedback adjudicated by the operator — the store split reframed portability-first, the validator unbundled to Phase 0, a premise-falsification test added. Spec stays `Draft`.
- **Last completed:** feedback revision — 4 of 5 feedback points accepted, 1 accepted in part and its inference corrected
- **Next:** re-invoke `/spec-review` against CP-1, or hand the shape to the operator. Operator approval remains the standing exit criterion and is not something an agent reviewer can supply. Phase 0 (the validator) is now clear to start without waiting on CP-1.
- **Open holds:** OQ-1/2/3/5/6/7 open by design. **OQ-4 resolved during design.** OQ-7 is new — the single-to-multi-operator memory re-split, deliberately unowned until a transition is observed.
- **Pending checkpoint:** CP-1 — Design Approval (architecture.md §9), open
- **Archive:** none — all entries live
- **Latest entry:** `## 2026-08-18 — Revision on originating-session feedback: portability-first store split`

## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.

Codified forward from [specs/tech-stack.md](../tech-stack.md) `## Grammar` — the repo's declared CWSP dialect, read during Discovery. Point-in-time snapshot fixed for this spec's life; a mid-flight dialect change routes through `spec-amend`.

## 2026-08-17 — Design spec authored

**Authored by:** waseric
**Trigger:** [specs/findings/20260817-knowledge-vault-bootstrap-gap/](../findings/20260817-knowledge-vault-bootstrap-gap/finding.md) at `status: routed`, route subtype `spec-write` via a `spec-design` pass first.
**Status:** Draft — Open for Review

**Discovery.** Prior conversation supplied the operator's own distillation of the newer instance's manual spin-up (preserved as the finding's `source-signal.md`), so no Discovery interview was restarted. Discovery instead consisted of a first-hand audit of both reference instances — `admindoc` (~330 files, governed by its own reshape design spec) and the newer instance (~24 files, one day old) — written up as [docs/knowledge-vault-archetype-audit.md](../../docs/knowledge-vault-archetype-audit.md).

**Why the audit exists as a separate committed document.** The operator will continue this work from a context with no access to `admindoc`. Everything load-bearing about the prior art is therefore transcribed into the audit rather than pointed at, and this spec cites the audit as authoritative for all prior-art claims (§3, §14). This was the session's primary durability constraint and it drove the ordering: audit first, then design.

**The audit corrected the originating finding in five places**, and the spec is built on the corrected account:
1. In-vault `memory/` is **not** invariant — `admindoc` has none and routes per-operator facts outside the repo by policy, the exact inverse of the newer instance. It is a decision that follows from sharing posture, and the spec derives it rather than shipping it (§5.2).
2. The `.gitignore` is verbatim only in a six-line core; the two instances **directly conflict** on `.obsidian/plugins/`.
3. The hazard organ is a three-point pattern (canonical doc + agent-contract summary + point-of-use restatement), not one document (§5.3).
4. Five invariants the finding omits, all near-verbatim shippable: `.obsidian/README.md`; the `techniques/` four-part card shape; the README non-breakage section; `knowledge-map.md`'s "frequently re-derived" section; per-area cross-routing sentences.
5. The convergence claim is overstated — the newer instance names `admindoc` as prior art in its own `CLAUDE.md`, so this was a hand transplant, not independent convergence. The spec states this plainly (§3) and substitutes a stronger datum: two independent *designs* of the archetype, 15 months apart, hit the same three open questions (Obsidian posture, memory location, `history/` contents). That recurrence is the argument for shipping those three as explicit interview questions or decisions.

**Operator decisions taken at Clarify.**
- **Name:** `vault-bootstrap` — verb-shaped like the `spec-*` family, leaving room for a `*-bootstrap` sibling family.
- **Obsidian:** one skill with an Obsidian toggle, gating `.obsidian/` assets, the wikilink exception clause, and the canvas rule. The audit supports it — the Obsidian-specific surface is small and cleanly separable.
- **`project-constitution` seam:** treat the missing "discovery already gathered" INPUT as a **presumed gap, not a real one**. The operator's judgment from repeated use is that the skill works adeptly from session context, which routinely points at prior art. Closed as a decision (§5.6) rather than deferred to an OQ; a regression would be a new finding against a real symptom rather than a speculative amendment. `project-constitution` stays untouched, per the finding's binding constraint.

**The finding's leaning that did not survive.** The finding leaned toward sibling archetype skills **sharing a common `assets/` directory**. The first half (siblings, no dispatcher) is adopted; the shared-assets half is rejected as incompatible with this repo's [Atomic-Skill Portability Principle](../tech-stack.md), which forbids a runtime dependency on host-repo files and requires standalone installation to work. A `_skeleton/` shared *between* skill directories is exactly that dependency, and the principle's own originating finding ([20260517-intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/finding.md)) is this same failure mode. Sibling archetypes therefore duplicate what they share — the cost the `finding-intake` / `finding-triage` template pair already pays deliberately (§5.5).

**Sharpest new datum.** The mature reference instance — whose operator *wrote* the index convention — still carries an unindexed root-level document with spaces in its filename. The convention rots under the person who authored it. That single observation converted the validator from a nice-to-have into §5.4's index-coverage check and into CP-3's exit criterion: a validator that reports both reference instances clean is a broken validator.

**Structure.** Full 14-section design-spec format. 6 phases (§7), 4 checkpoints (§9), 6 open questions (§13) each with analysis, leaning, and owner. Downstream feature spec named: `specs/YYYYMMDD-vault-bootstrap-skill/feature.md`.

**Not verified against external sources.** This spec makes no external-ecosystem claims — every factual claim is about the operator's own repos (audited first-hand and transcribed) or about this repo's own constitution and skills (read directly). No `WebFetch`/`WebSearch` pass was warranted.

**Next.** CP-1 — Design Approval. The checkpoint reviews whether the archetype boundary is drawn correctly, whether the invariant/authored split matches the audit, whether §5.5's rejection of the shared-assets leaning is sound, and whether the OQ set contains the right deferrals.

## 2026-08-17 — Design pass: third-instance store model folded in

Still in `spec-design`, pre-CP-1, so these land as direct draft revisions rather than amendments —
`spec-amend` governs approved specs.

**Why a third instance.** The operator directed a mining pass over an internal capability
repository (commits `51a5076..c177707`), on the standing premise that this work is conceptually
iterative and the same problems get re-solved in each context. The commit range is squarely
on-topic: it establishes a layered agent working-context store for a prose-first repo with agents
as first-class contributors. Load-bearing content is transcribed into the audit's new **Appendix A**
under the same durability constraint that produced the audit — the repo is not reachable from every
context this design will be continued from.

**The archetype-boundary question, raised and declined.** The mining pass opened with an
observation that the third instance satisfies §4's *knowledge vault* definition verbatim while
being emphatically not one (it has a release surface, a manifest, versioned distribution to an
unseeable consumer set), which would make the vocabulary under-discriminating and would bear
directly on CP-1's first review focus. **The operator declined to weight it.** The stated reason:
that repo is a specialized multi-modal knowledge repository with its own distribution management,
whose needs greatly exceed a normal knowledge repository's, and treating it as a boundary case would
invite building toward its requirements. Recorded rather than dropped, with the standing calibration
that the target archetype is closer to an AI-moderated notebook than to a distribution system. The
§4 vocabulary is unchanged; Appendix A notes the discriminator limit as an observation only.

**The substantive correction — memory is two kinds, not one location.** The audit (§5) read the two
reference instances as having made opposite memory choices, each right for its posture, and §5.2
derived memory *location* from sharing posture. The operator's framing supersedes that: knowledge
projects will likely be **shared**, and what has to be first-class is the distinction between
**user-oriented** and **repo-oriented** memories. Re-reading the audit's own quotes confirms it —
both concern *operator-specific* facts, and the single-operator instance can commit them only
because the distinction collapses for want of a second reader. Neither instance names the
repo-oriented category at all. A shared vault needs both stores at once, so the split is asserted in
every vault and the interview loses a switch instead of gaining one. This is now **§5.2a**, and the
audit's §5 design consequence carries a superseded-in-part note pointing at it.

**Two mechanical facts that were absent and change the design.**
1. Repo-oriented memory is invisible to a dispatched subagent — it sees the agent contract and
   path-scoped rules only. Since §5.7 wires every produced vault to the `spec-*` family including
   dispatch, this is a second derivation axis, not an exotic case.
2. `@import` in an agent contract does not defer cost; imported files load at launch. §5.1 ships
   `@knowledge-map.md`, so the note now says plainly that the import buys single-sourcing, not
   laziness — which is *why* path-scoped rules and skills had to enter the asset set.

**Consequent additions.** A five-store routing table with the cheapest-store-that-reaches-the-right-
audience rule (§5.2a); `.claude/rules/*.md` as a shipped store, used for the point-of-use hazard
restatement (§5.3) and the spec-session doctrine block moved out of the always-loaded contract and
scoped to `specs/**` (§5.7); a conditional `workspace-setup` skill, because the pointer to an
in-vault memory directory cannot be committed and a fresh clone is therefore *silently*
misconfigured — with the three details learned expensively (trust-prompt gating, no automatic
migration, verify the contract actually loaded); a `MEMORY.md` header carrying its own load limit
and team-visibility warning, which is the §5.3 restatement pattern applied to the memory store; the
ignore lines that enforce the split; and **§5.2b**, two durability conventions (verified-as-of
provenance, immutable citation) with an explicit anti-goal against making staleness a validator
check.

**What was deliberately not carried forward.** The third instance's release/manifest/versioning/
dual-environment apparatus, its evidence-repo table as an apparatus, and any governance regime
around the stores. §5.2a carries an explicit *Calibration* paragraph so a later reader can see the
over-delivery line was drawn on purpose, and CP-1's review focus now asks whether it was drawn in
the right place.

**Also updated.** §6 NFRs (configuration switches — the memory split is no longer one; security now
treats committed memory as a publication surface); §7 Phase 2's derived-answer list; §8 gains a
**store-split test**, the behavioral check for the failure mode that is silent in the field; §14
references the third instance with its non-carry-forward rationale.

**Numbering.** New subsections are `5.2a` / `5.2b` rather than renumbering `5.3`–`5.7` and every
cross-reference to them. The declared Grammar governs `## N.` and journal anchors, so the dialect is
intact; a renumber is a legitimate CP-1 nit if the operator wants the sequence clean.

**Next.** CP-1 — Design Approval, unchanged, with one review focus added.

**One pre-existing tension surfaced, not resolved.** §5.2's interview already enumerated seven
questions against a 3–5 NFR, and §5.2b's citation gate would make eight. Rather than quietly
worsening it, §6's Adoptability NFR now states the tension, offers the defensible reading (several
are one-breath decisions rather than prompts), and flags it for CP-1: either the target moves or the
questions merge. Phase 6's effort measure is what settles it.

## 2026-08-17 — Generalize prior-art references

**Operator directive.** Remove references to the reference instances' subject matter from the spec
and the audit going forward. `admindoc` may be named — the name is generic enough to disclose
nothing — and pointing at unreachable specs by path is fine, since an agent may happen to find one
locally. Git history is deliberately **not** rewritten; this is a going-forward change only.

**Applied.** The second instance is now *the newer instance* throughout `architecture.md`,
`journal.md`, and the audit; its domain reads as "a private personal domain," and the first
instance's as "platform operations." A code-spoke repo name, an area-directory name, a domain
policy document's subject, and a stray root filename were generalized in place; one verbatim
`CLAUDE.md` quote has its vault name elided rather than reworded, so it stays marked as a quote.
Both documents gained a short **Naming** paragraph so a cold reader knows the label is deliberate
and not a gap: the instances' *domains* were never load-bearing — only their postures, sizes, and
structural choices are — so withholding them costs the design nothing.

**Not scrubbed, and why.** The originating finding's `source-signal.md` is a verbatim operator
snapshot; editing it would falsify what it claims to be. Unrelated older findings in
`specs/findings/` are domain artifacts of their own dogfood exercises and out of this directive's
scope. Both are noted in the audit's Naming paragraph so the inconsistency reads as a decision.

**No design content changed.** Labels and domain descriptors only; every claim, checkpoint, and open
question is untouched.

## 2026-08-17 — Review of CP-1

**Reviewer:** Claude Opus 5 — inline `spec-review` (agent reviewer)
**Outcome:** changes requested
**Tasks reviewed:** none — design-spec checkpoint. Artifact under review is `architecture.md` plus
[docs/knowledge-vault-archetype-audit.md](../../docs/knowledge-vault-archetype-audit.md), over
`3bab5bd..01bd088`.
**Blockers:** 3
1. **The `*(conditional on repo-oriented memory in-vault)*` gate has no off-state.** §5.1 marks
   `memory/MEMORY.md` and `.claude/skills/workspace-setup/SKILL.md` conditional on it, but §5.2a
   makes repo-oriented memory unconditional and §6 *Configuration* states outright that "the memory
   split is **not** a switch; both stores exist in every vault." Phase 1 cannot implement an ungated
   gate, and CP-2's review focus is precisely "that conditional gates actually omit rather than emit
   empty scaffolding."
2. **OQ-4's leaning is contradicted by §5.1.** OQ-4 leans "do not create the directory," yet §5.1
   ships `.claude/skills/workspace-setup/SKILL.md` — in every vault, per blocker 1. OQ-4 was authored
   at `3bab5bd` and the `3d3c1a9` third-instance pass introduced `workspace-setup` without revisiting
   it. As written the spec answers its own open question the opposite way from the deferral, which is
   drift rather than a deferral.
3. **§6's Adoptability NFR is contradicted by its own detailed design.** The 3–5 question target
   versus 7 unconditional prompts (purpose, pre-existing dirs, git posture, sharing posture, Obsidian,
   domain fact, hazard, anticipated use) plus the §5.2b citation gate and the §5.2/§6 license prompt.
   The spec flagged this for CP-1 itself; the review's ruling is that the **target must move** — only
   git-and-sharing-posture is genuinely mergeable. §8's interview-effort measure is unmeasurable until
   this is settled, so it gates the Phase 6 validation approach too.

**Important:** 6 — license is listed under "the answers the operator never gives" yet says *prompt*
(§5.2); three off-by-one OQ cross-references (lines 64, 189, 284, all wrong since `3bab5bd` — the OQ
set was never renumbered); the `.gitignore` "six-line core *gains* the local settings file" arithmetic
(already in the audit's six, so the core is seven); the `<area>/README.md` × 5 row double-counting
`techniques/` and `history/`, which have their own rows with different substance; audit §8.6's
iCloud+git conditional block dropped with no asset row and no interview input to resolve
`{{GIT_POSTURE}}`; and CP-1 review-focus item 5 (calibration) only partly satisfied — the Calibration
paragraph claims more restraint than §5.1 exercises.

**Review focus verdicts.** Archetype boundary: **pass** — Appendix A's repo that satisfies all five
observable properties while sitting *outside* the archetype is the sharpest boundary evidence in the
document, and §14 carries it. §5.5's rejection of the shared-assets leaning: **pass** — verified
against the principle's text (item 1's no-runtime-dependency, item 3's standalone install) and
against the live precedent (both `finding-*` skills do ship their own `_template/`, same two files).
Five audit corrections: **pass** — #2/#3/#4/#5 reflected in §5.1/§5.3/§3, and #1 is consciously
*superseded* by §5.2a with the audit carrying a matching "Superseded in part" note at its §5, which is
a stronger outcome than reflection. Invariant/authored split against the audit: **pass with comments**.
§5.2a calibration and the OQ set as deferrals: **fail**, per the blockers.

**Spec amendments proposed:** none as amendments. The spec is `Draft — Open for Review` and pre-CP-1,
so all three blockers land as **direct draft revisions**, consistent with the precedent this journal
set at the third-instance pass: `spec-amend` governs approved specs.

**Next action:** operator decides blocker 3's question count (the only blocker needing a decision
rather than an edit); blockers 1 and 2 are mechanical reconciliations. Then re-invoke `/spec-review`
against CP-1. CP-1's exit criterion of operator approval remains outstanding regardless — an agent
reviewer cannot supply it.

## 2026-08-18 — CP-1 remediation

**Trigger.** Operator directed remediation of the CP-1 review's findings. The spec is
`Draft — Open for Review` and pre-approval, so every change lands as a **direct draft revision**,
not an amendment — the precedent this journal set at the third-instance pass and which the review
itself restated.

**Blockers — all three closed.**

1. **The ungated gate.** `memory/MEMORY.md` and `.claude/skills/workspace-setup/SKILL.md` are now
   unconditional §5.1 rows; the `*(conditional on repo-oriented memory in-vault)*` marker is gone
   from both, and the `workspace-setup` row states outright that §5.2a leaves it no off-state. The
   same correction propagates to §5.4's check-2 false-positive class, which had described the
   `memory/` exemption as conditional on a variant that no longer exists.
2. **OQ-4.** Resolved during design rather than re-deferred, and rewritten to say why: the deferral
   was authored at `3bab5bd` on the premise that vault-local skills arrive with a *domain* need, and
   the third-instance pass then introduced a **bootstrap-time** one. The skeleton creates exactly the
   `.claude/` subdirectories its named assets occupy — `workspace-setup` and the two path-scoped
   rules — and nothing speculative. The original leaning's shipped artifacts (the `.gitignore`
   discipline, the contract sentence) survive unchanged. CP-2 now confirms created-set equals
   named-asset-set.
3. **The interview target — operator decision.** Offered three reconciliations; the operator chose
   **two-tier: 4 required + optional depth**. §5.2 now tags every question `[required]` or `[depth]`
   and states that the four required questions (purpose, posture, Obsidian, hazard) alone produce a
   valid vault. §6's Adoptability NFR reads `4 required, up to 7 total` and explicitly supersedes the
   3–5 target. A skipped depth question becomes a logged deferral rather than a silent drop, and the
   skill recommends the depth round rather than treating the floor as the expected path — the
   anticipated-use question is the one the originating finding names as having most changed the
   manual output, so it must not be cheap to lose by default. §8's effort measure now runs the
   interview **twice**, and the required-only run doubles as the test that the four-question floor
   genuinely produces a valid vault; if it does not, a depth question is misclassified.

**Important — all six closed.**
- **License reclassified.** It was listed under "the answers the operator never gives" while saying
  *prompt*. Now stated correctly: the *gate* is derived from remote posture, the *choice* is a
  question, and it is counted against the interview budget. §5.2b's citation-durability question is
  given the same shape, so both conditionals are described by one rule.
- **Three off-by-one OQ cross-references** fixed (§3's validator-and-CI line → OQ-1; §5.2's
  `history/` line → OQ-2; §5.4's git-hook alternative → OQ-1). All three were wrong since `3bab5bd`.
- **`.gitignore` arithmetic.** The core is **seven** lines, not "six plus two": the audit's six
  already contain the local settings file, so the only addition is `CLAUDE.local.md`.
- **The `<area>/README.md` row** is now `× 3` (`architecture/ operations/ research/`) and says
  explicitly that `techniques/` and `history/` carry their own rows with different substance.
- **Audit §8.6's iCloud-plus-git block** regains an asset row — a conditional `CLAUDE.md` § Git
  sub-block emitted when the git-posture answer reports a sync-layer-backed working tree, which is
  also the interview input that resolves `{{GIT_POSTURE}}`.
- **§5.2a's Calibration paragraph** rewritten to claim only the restraint it actually exercises. It
  now names what *is* shipped under `.claude/` (two rules, one skill), and states that the restraint
  is **bounded need, not absence** — the archetype does create `.claude/` subdirectories when a named
  asset lands in one.

**Not addressed.** The review's 7 advisory findings. They were not enumerated in the review entry's
body, so they are not actionable from the journal alone; they need to come from the review session or
be re-derived at re-review.

**Status banner.** §1 now records the CP-1 outcome and the remediation date inline, so a cold reader
opening `architecture.md` alone sees that the document is post-review-and-revised rather than
untouched draft.

**Next.** Re-invoke `/spec-review` against CP-1. CP-1's exit criterion of operator approval remains
outstanding regardless — an agent reviewer cannot supply it.

## 2026-08-18 — Re-review of CP-1

**Reviewer:** Claude Opus 5 — inline `spec-review` (agent reviewer)
**Outcome:** changes requested
**Tasks reviewed:** none — design-spec checkpoint. Artifact under review is `architecture.md` plus
[docs/knowledge-vault-archetype-audit.md](../../docs/knowledge-vault-archetype-audit.md), over
`3bab5bd..95b5a3e`, with the remediation commit `95b5a3e` read in full against the prior verdict.

**Prior blockers — 2 closed, 1 partially closed.**
1. **The ungated gate — closed.** Both §5.1 rows are unconditional and the `workspace-setup` row
   states why it has no off-state; §5.4's check-2 false-positive class no longer references the
   dropped variant. One residual word survives in §5.2a (see important findings).
2. **OQ-4 — closed.** Resolved during design, with the premise change (domain need → bootstrap-time
   need) stated explicitly and CP-2 given the confirming check. This is the stronger outcome: it
   records *why* the deferral stopped being one rather than silently deleting it.
3. **The interview target — partially closed.** The two-tier design is coherent and internally
   consistent: 4 required (purpose, posture, Obsidian, hazard) + 3 depth = 7, §6 restated as
   `4 required / up to 7 total`, the two conditional prompts described by one rule, §8's effort
   measure now running the interview twice with the required-only run doubling as the floor test.
   But the superseded 3–5 number still stands in **§2 Goals** and in the **§4 topology diagram** —
   the two places a reader meets it first. §6 announces the supersession; the document does not
   carry it through. That is the same self-contradiction the original blocker named, now smaller.

**Blockers:** 1
1. **The superseded 3–5 question target survives in §2 and §4** (`architecture.md:20`, `:84`). §6
   supersedes it explicitly, so the spec now asserts both numbers. Mechanical fix.

**Important:** 2
- **`architecture.md:233`** still calls it "the conditional `workspace-setup` skill" — residual of
  blocker 1, contradicting §5.1's now-explicit "no off-state."
- **§9's CP-1 Status line** recorded only the original verdict, with the remediation visible solely
  in §1's banner and the journal. Fixed in this pass: the checkpoint entry now carries the
  re-review outcome, with the original preserved as **Prior status**.

**Advisory:** 4
- §5.2's "the extra six minutes" quantifies the depth round with a number nothing in the spec
  supports; the Phase 6 dogfood is what produces it.
- §13 is titled *Open Questions* and contains a resolved one. A short "Resolved during design"
  grouping would keep the section's contract honest as more OQs close.
- §6's "up to 7 total" hides that depth question 2 fires only when the scan finds pre-existing
  directories, so 7 is reachable only on a non-empty target.
- §5.1's new iCloud row is a *sub-block of another row's file* rather than a file, mixing
  granularity in an otherwise file-per-row table.

**Review focus verdicts.** Archetype boundary: **pass** (unchanged). §5.5's rejection of the
shared-assets leaning: **pass** (unchanged). Five audit corrections: **pass** (unchanged; the audit's
§5 "Superseded in part" note and Appendix A verified present). Invariant/authored split: **pass with
comments**. §5.2a calibration: **pass** — the rewritten paragraph now claims *bounded need* rather
than absence and names the three shipped `.claude/` assets, which is what the prior fail asked for.
The OQ set as deferrals: **pass** — with OQ-4 removed from the open set, the remaining five are
genuinely undecided and each carries an owner and a ratifying checkpoint.

**Exit criteria.** Not met. One blocker outstanding, and independently: operator approval of the
shape and OQ set has not been given, which no agent reviewer can supply.

**Spec amendments proposed:** none. The spec remains `Draft — Open for Review` and pre-approval, so
the blocker and the important finding land as direct draft revisions, per this journal's standing
precedent.

**Next action:** strike 3–5 from §2 and §4, drop "conditional" at §5.2a's mechanical fact 2, then
re-invoke `/spec-review` against CP-1 — or hand the shape to the operator for the approval that is
the checkpoint's real gate.

## 2026-08-18 — CP-1 re-review remediation

All seven findings from the 2026-08-18 re-review closed as direct draft revisions (spec is
pre-approval, so no `spec-amend` route — this journal's standing precedent).

**Blocker — closed.** The superseded 3–5 question target struck from both places it survived: §2
Goals now reads "four required questions, up to seven (§6)", and the §4 topology diagram's P2 row
reads `4 required + 3 depth`. §6 remains the single authority on the number.

**Important — both closed.** §5.2a's mechanical fact 2 no longer calls `workspace-setup`
"conditional", matching §5.1's "no off-state". §9's CP-1 Status line was already fixed in the prior
pass and now also carries this remediation, with the original verdict preserved as *Prior status*.

**Advisory — all four closed.**
- §5.2's "the extra six minutes" replaced with "more time" / "what the depth round buys" — the
  number was unsupported and the Phase 6 dogfood is what produces it.
- §13 gained a **Resolved during design** grouping at its end; OQ-4 moved there as an `h4` with its
  analysis and resolution intact, so the section title stays honest as more OQs close.
- §6's "up to 7 total" now states that 7 is reachable only when the scan finds pre-existing
  directories, which is what depth question 2 asks about.
- §5.1's standalone sync-layer row folded into the `CLAUDE.md` row as a named conditional sub-block,
  restoring file-per-row granularity. **Operator correction taken in the same edit:** the row named
  iCloud, which is one storage technology among several and has no business in a spec that must not
  name specific consumers or stacks. The rule itself — let sync settle before committing, avoid
  concurrent edits from two machines, with no remote this is the only copy — is genuine
  contributor-facing git posture and stays in `CLAUDE.md` § Git, now gated on "a working tree backed
  by a file-sync layer" rather than on a product name. §3's prior-art description of the newer
  instance still says iCloud-synced; that is a transcribed fact about a reference instance, not
  shipped content, and stays.

**Next action:** re-invoke `/spec-review` against CP-1, or take the shape to the operator for the
approval that is the checkpoint's real gate.

## 2026-08-18 — Revision on originating-session feedback: portability-first store split

**Trigger.** Structured CP-1 feedback from the session that authored this spec's original content and then ran the newer instance's spin-up. Five points: §5.2a over-weighted; "shared is the recommended default" contested; the output has a setup ceremony the interview does not; unbundle the validator; and §8 never tests whether a produced vault can absorb being wrong. Adjudicated by the operator, who supplied the constraint that settles the second point and forced the correction to it.

**The operator's constraint, and why it reverses the feedback's inference.** Stated unconditionally: critical data must not be stranded in a local machine's `~/.claude`, because that destroys portability. Attached question — is it acceptable for the repository to hold memories about the operator's own preferences, likely yes for a personal notebook.

The feedback's chain was: default to single-operator → §5.2a mostly collapses → `workspace-setup` gets an off-state → the produced vault sheds its post-clone ritual. The first link holds and the rest do not. The chain assumes that a single-operator vault may leave user-oriented facts in a per-machine store; the operator's constraint forbids exactly that. So the default flips and the apparatus stays.

**What actually changed: the organizing axis, not the weight.** §5.2a previously argued the split exists because a shared vault would leak personal facts, and asserted that "sharing posture does not move that boundary." Both are now wrong. The section is rebuilt on the invariant **nothing durable about the vault lives outside the vault**, which holds at every posture:

- Repo-oriented memory: in-vault and committed, **unconditionally**. Not a posture question.
- User-oriented memory: kind is fixed by the archetype; **location** derives from the Q3 posture answer. Single-operator commits it in-vault beside the repo-oriented store; multi-operator routes it out.
- The two kinds stay labelled at every posture even where they share a location — that is what makes a later re-split mechanical rather than a re-reading of every entry (new OQ-7).

This reads the two reference instances better than the previous text did. The audit (§5) has them making *opposite* choices; on the portability reading they make the *same* choice evaluated at different postures — keep durable knowledge reachable, and do not let one operator's habits become everyone's instructions. `admindoc` is multi-operator, so it routes personal facts out; the newer instance is single-operator, so it keeps them in. Neither names the repo-oriented category, and that gap is identical at both postures, which is why filling it is unconditional.

**The question the operator flagged, resolved as a derivation.** Committing user-preference memories into a personal notebook is now the single-operator default, stated in the produced agent contract rather than asked as a fifth interview question — the posture answer already determines it, and the interview floor stays at 4 required / 7 total. The `MEMORY.md` header becomes posture-dependent: on a shared vault it warns personal facts out; on a personal vault it says they belong there and names what changes when a second reader arrives.

**The ceremony is declared, not wished away.** `workspace-setup` keeps no off-state, because the uncommittable memory pointer is the invariant's irreducible cost. What was fixable was the *silence*: a one-line first-session check now ships in the always-loaded agent contract, so a fresh clone announces that memory is not resolving into the vault and offers the setup, unprompted. §6 caps post-clone manual steps at one and requires it to be self-announcing; §8 gains a fresh-clone test as the gate.

**Validator unbundled.** Promoted from Phase 4 to **Phase 0**, explicitly not gated by CP-1. Nothing in the five checks depends on the archetype boundary or the store split — all five derive from conventions the reference instances already declare — and check 3 has a confirmed live miss in the mature instance to fix now. Authored at its final home in the skill directory, which stays inert until Phase 2 writes `SKILL.md`. Old Phase 5 → 4, old Phase 6 → 5; CP-3 retargets to post-Phase 0, CP-4 to post-Phase 5.

**Premise-falsification test added, and it is the best point in the feedback.** §8 tested navigation and nothing else. It now leads with a premise-falsification test, ranked above the cold-reader test: hand a produced vault a fact contradicting a premise in its own constitution, and confirm the session routes it to a dated amendment rather than an in-place edit or silent continuation. The evidence is live — the newer instance's constitution was amended twice inside 24 hours when a first-phase survey falsified a premise written at bootstrap, and the loop ran correctly. §5.7 now states that `specs/constitution.journal.md` and the route-drift-via-`spec-amend` rule are shipped for this reason rather than for convenience, which is a stronger justification than the one it carried.

**Sections touched.** §1 (fourth architectural commitment), §2 (portability goal), §4 (three vocabulary rows), §5.1 (three asset rows), §5.2 (Q3 default; the derivation bullet), §5.2a (rewritten), §5.4 (ships first), §5.7 (why it is load-bearing), §6 (skill-vs-vault portability split, security, configuration), §7 (Phase 0 + renumber), §8 (two new tests, store-split test made posture-aware), §9 (CP-1 status with a disposition table; CP-3/CP-4 triggers), §10 (two new risks), §13 (OQ-7 added).

**Not changed, deliberately.** The four-question interview floor. The `project-constitution` seam. §5.5's rejection of shared assets between sibling skills. The five-store table's shape — the feedback's charge that it is a governance apparatus was partly accepted by locating the weight (`workspace-setup`) rather than by trimming rows that cost nothing.

**Status.** Spec stays `Draft`. CP-1 remains open; operator approval of the shape and OQ set is still the standing exit criterion.
