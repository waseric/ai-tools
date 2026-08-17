# vault-bootstrap — Journal

## Current State
- **Phase:** CP-1 review (design authored, awaiting approval)
- **Last completed:** design pass — third-instance prior art mined and folded in (2026-08-17)
- **Next:** CP-1 — Design Approval (architecture.md §9)
- **Open holds:** OQ-1 through OQ-6 open by design; all carry leanings and owners. No blockers.
- **Pending checkpoint:** CP-1 — Design Approval (architecture.md §9)
- **Archive:** none — all entries live
- **Latest entry:** `## 2026-08-17 — Design pass: third-instance store model folded in`

## Grammar
- **Journal entry:** `## <YYYY-MM-DD> — <event>`
- **Amendment:** `## <YYYY-MM-DD> — Amendment <id>`
- **Spec section:** `## N. <title>`; task blocks `#{3,4} <T-ID> — <title>` (heading depth h3 or h4), plus the §7 table row where one exists.

Codified forward from [specs/tech-stack.md](../tech-stack.md) `## Grammar` — the repo's declared CWSP dialect, read during Discovery. Point-in-time snapshot fixed for this spec's life; a mid-flight dialect change routes through `spec-amend`.

## 2026-08-17 — Design spec authored

**Authored by:** waseric
**Trigger:** [specs/findings/20260817-knowledge-vault-bootstrap-gap/](../findings/20260817-knowledge-vault-bootstrap-gap/finding.md) at `status: routed`, route subtype `spec-write` via a `spec-design` pass first.
**Status:** Draft — Open for Review

**Discovery.** Prior conversation supplied the operator's own distillation of the manual `Finances` spin-up (preserved as the finding's `source-signal.md`), so no Discovery interview was restarted. Discovery instead consisted of a first-hand audit of both reference instances — `sandlotminecraft/admindoc` (~330 files, governed by its own reshape design spec) and the `Finances` vault (~24 files, one day old) — written up as [docs/knowledge-vault-archetype-audit.md](../../docs/knowledge-vault-archetype-audit.md).

**Why the audit exists as a separate committed document.** The operator will continue this work from a context with no access to `admindoc`. Everything load-bearing about the prior art is therefore transcribed into the audit rather than pointed at, and this spec cites the audit as authoritative for all prior-art claims (§3, §14). This was the session's primary durability constraint and it drove the ordering: audit first, then design.

**The audit corrected the originating finding in five places**, and the spec is built on the corrected account:
1. In-vault `memory/` is **not** invariant — `admindoc` has none and routes per-operator facts outside the repo by policy, the exact inverse of `Finances`. It is a decision that follows from sharing posture, and the spec derives it rather than shipping it (§5.2).
2. The `.gitignore` is verbatim only in a six-line core; the two instances **directly conflict** on `.obsidian/plugins/`.
3. The hazard organ is a three-point pattern (canonical doc + agent-contract summary + point-of-use restatement), not one document (§5.3).
4. Five invariants the finding omits, all near-verbatim shippable: `.obsidian/README.md`; the `techniques/` four-part card shape; the README non-breakage section; `knowledge-map.md`'s "frequently re-derived" section; per-area cross-routing sentences.
5. The convergence claim is overstated — `Finances` names `admindoc` as prior art in its own `CLAUDE.md`, so this was a hand transplant, not independent convergence. The spec states this plainly (§3) and substitutes a stronger datum: two independent *designs* of the archetype, 15 months apart, hit the same three open questions (Obsidian posture, memory location, `history/` contents). That recurrence is the argument for shipping those three as explicit interview questions or decisions.

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
