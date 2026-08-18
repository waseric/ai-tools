# Karpathy's LLM Wiki pattern exposes lifecycle gaps in the vault-bootstrap archetype — Finding

> Status: closed
> Domain: methodology
> Severity: important                                  ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-18
> Last transition: 2026-08-18

## Intake

**Reported by:** self
**Reported via:** text + URL
**Captured by:** waseric; persona-frame: intake
**Summary:** An external pattern document ("LLM Wiki", Karpathy, gist `442a6bf5`) describes a knowledge-base architecture that overlaps the `vault-bootstrap` archetype substantially but partitions the problem differently. Evaluated against the in-progress [vault-bootstrap design spec](../../20260817-vault-bootstrap/architecture.md) (at CP-1, open), the two documents turn out to be adjacent halves rather than competing designs: the gist is a **lifecycle doctrine** (ingest / query / lint, and what compounds over days 1–1000) and explicitly declines to specify structure; the spec is a **bootstrap mechanism** (what exists at commit 1) and specifies no operations at all beyond a structural validator. The comparison surfaced six gaps and two challenges, most consequentially: (a) the spec's [§5.2b](../../20260817-vault-bootstrap/architecture.md) staleness anti-goal is sound for a mechanical checker but silently forecloses the gist's *semantic* lint pass — two linters are needed, not one; (b) the spec has no append-only operations log, though [OQ-2](../../20260817-vault-bootstrap/architecture.md) reasons its way to the edge of one and scopes it down to a bootstrap stub; (c) a synthesis produced during a session has no home in any of the five knowledge stores, so exploration does not compound; (d) there is no raw-sources layer, and this spec's *own* authoring instantiated one ad hoc (`docs/knowledge-vault-archetype-audit.md`) under exactly the pressure the gist predicts. Full analysis with per-item evidence in [analysis.md](analysis.md). No defect in the spec is claimed — the signal is evaluative, captured for future consideration rather than for immediate routing.

**External references:**
- <https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f> — "LLM Wiki", Karpathy. Body fetched successfully 2026-08-18 via `curl` against the `/raw` endpoint (HTTP 200, 11,985 bytes); both `gist.github.com` and `gist.githubusercontent.com` are blocked in this environment's browser pane, and a WebFetch attempt declined to reproduce the body. Snapshot below is a **structural digest, not a verbatim capture** — see the durability note in [analysis.md](analysis.md#snapshot-fidelity).
- <!-- fetched 2026-08-18 --> Digest of the pattern: three layers — *raw sources* (immutable; the LLM reads but never modifies), *the wiki* (LLM-generated markdown, LLM-owned entirely), *the schema* (a `CLAUDE.md`/`AGENTS.md` that encodes conventions and workflows, co-evolved by human and LLM). Three operations — *ingest* (read a new source, discuss takeaways, write a summary page, update the index, sweep relevant entity/concept pages across the wiki, append to the log; "a single source might touch 10-15 wiki pages"), *query* (search pages, synthesize with citations, and **file good answers back into the wiki as new pages** so explorations compound), *lint* (periodic LLM health-check for contradictions between pages, claims superseded by newer sources, orphan pages with no inbound links, concepts mentioned but lacking a page, missing cross-references, data gaps). Two navigation files — `index.md`, content-oriented, every page with a one-line gloss, category-organized, updated on every ingest and read *first* at query time, explicitly positioned as replacing embedding-based RAG at moderate scale (~100 sources / hundreds of pages); and `log.md`, chronological append-only, with a consistent entry prefix so it is greppable with unix tools. Division of labor is emphatic: the human curates sources, directs analysis, and asks questions; the LLM writes and maintains all of the wiki. Rationale is economic — the tedious part of a knowledge base is bookkeeping, not reading or thinking, and humans abandon wikis when maintenance cost outgrows value. Optional surfaces: a local search engine when `index.md` stops scaling (`qmd`, hybrid BM25/vector, CLI + MCP); Obsidian as the reading IDE (web clipper, local image download, graph view, Marp decks, Dataview frontmatter). Closing note states the document is deliberately abstract — structure, page formats, and tooling are left to the reader's agent to instantiate.
- Comment thread on the same gist — **medium confidence, paraphrase only.** Available to this session solely as a WebFetch summary, not as verbatim text. Reported team-scale lessons: concurrent ingests need atomic name reservation to avoid duplicate pages; **separate wikis by audience rather than relying on visibility labels**; full-text search complements index-based navigation at scale; human corrections need "pins" to survive re-compilation; review generated artifacts rather than plans. The audience-separation lesson is the one that challenges spec [§5.2a](../../20260817-vault-bootstrap/architecture.md) directly.

## Triage

**Triaged by:** waseric; persona-frame: business analyst
**Triage date:** 2026-08-18
**Reproducibility:** not applicable
**Repro steps (if reproducible):**
1. Not applicable — the signal is an evaluative comparison between two documents, not a behavior. Both documents are reachable: the spec in-repo, the gist snapshotted above.

**Scope:** The `vault-bootstrap` design spec ([specs/20260817-vault-bootstrap/architecture.md](../../20260817-vault-bootstrap/architecture.md)), currently at CP-1 open with operator approval outstanding, and any sibling archetype skill that later inherits its lifecycle posture. No shipped skill is affected — `vault-bootstrap` has no `SKILL.md` yet, so nothing is deployed and no deploy-sync obligation arises. No produced vault exists to be wrong. Reader scope is the operator plus any future session evaluating the archetype's lifecycle scope.
**Domain confirmation:** methodology
**Severity confirmation:** important
**Triage notes:** Domain and severity both confirmed as captured at intake. Methodology is unambiguous — the subject is a spec's scope boundary, not a runtime behavior. `important` rather than `advisory` because two of the eight items are cheap corrections to reasoning the spec has *already done and scoped down* (the §5.2b anti-goal's over-reach; OQ-2's stub-vs-log conflation), which is the profile of a real miss rather than a preference; and `important` rather than `blocker` because nothing here falsifies a premise the spec depends on, nothing blocks CP-1, and the spec is internally coherent as written. Triage deliberately did **not** open the codebase or re-read the gist — the analysis was complete at intake and re-deriving it is the duplicated work triage exists to avoid.

Three things confirmed at triage without investigation:

1. **Not a defect.** Every gap is scope-boundary, not error. The spec does what it claims; the claim is narrower than the archetype's eventual need. Two items ([§5.2b](../../20260817-vault-bootstrap/architecture.md) and OQ-2) come closest to genuine misses, and even those are *foreclosures by adjacent reasoning* rather than mistakes.
2. **Not dedup-able against the originating finding.** [20260817-knowledge-vault-bootstrap-gap](../20260817-knowledge-vault-bootstrap-gap/finding.md) (`routed`) is about the *absence of a bootstrap mechanism*. This finding is about the *scope of what gets bootstrapped*. Same spec, disjoint questions; the earlier finding's route (design spec) is what produced the artifact this one evaluates.
3. **One item is a live tension worth naming independently of the rest.** The spec's "prose-only, no runtime code" archetype definition ([§1](../../20260817-vault-bootstrap/architecture.md)) is already contradicted by [OQ-1](../../20260817-vault-bootstrap/architecture.md)'s leaning to ship an executable validator into the produced vault. That is internal to the spec and would be true with or without the gist. It is the one item here that could stand as its own finding; it is kept in this bundle because the gist is what made it visible.

## Investigation (optional)

**Investigated by:** <placeholder — skipped; see journal>
**Investigation date:** <placeholder>
**Probable cause:** <placeholder>
**Code/configuration touchpoints:** <placeholder>
**Alternative hypotheses considered:** <placeholder>
**Proposed remedy:** <placeholder>

## Route

**Route decision:** close
**Decided by:** waseric; persona-frame: business analyst, with operator decision
**Route date:** 2026-08-18
**Target spec:** Not applicable — closed without routing. The evaluated spec is [specs/20260817-vault-bootstrap/architecture.md](../../20260817-vault-bootstrap/architecture.md), which is where a future `spec-amend` would land if any item is taken up.
**Route rationale:** Closed at the operator's direction, as **captured-for-consideration** rather than as work. The operator's stated goal was to make the analysis durably reachable, not to open a work item — and the artifact achieves that on its own: [analysis.md](analysis.md) is self-contained, cites the spec by section, and carries the gist digest, so a future session can act on any item without this conversation or a live fetch. Routing to `spec-amend` was considered and rejected on timing rather than merit: the target spec is mid-review at CP-1 with operator approval outstanding, and four of the eight items would widen its scope while it is being reviewed for whether its *current* boundary is drawn correctly. Deferring would have been the closest alternative, but `defer` implies a watch condition the pipeline should track, and there is none — nothing external will change to make these items ripe. The trigger is an operator decision about archetype scope, which is not a condition worth polling. If the operator takes up any item, this finding is reopened or a fresh finding cites it; the close is a filing decision, not a judgment that the items are wrong.
