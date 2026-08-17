# vault-bootstrap — Journal

## Current State
- **Phase:** CP-1 review (design authored, awaiting approval)
- **Last completed:** design spec authored (2026-08-17)
- **Next:** CP-1 — Design Approval (architecture.md §9)
- **Open holds:** OQ-1 through OQ-6 open by design; all carry leanings and owners. No blockers.
- **Pending checkpoint:** CP-1 — Design Approval (architecture.md §9)
- **Archive:** none — all entries live
- **Latest entry:** `## 2026-08-17 — Design spec authored`

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
