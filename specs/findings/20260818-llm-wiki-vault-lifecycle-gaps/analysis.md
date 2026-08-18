# LLM Wiki vs. vault-bootstrap — gap analysis

> Companion to [finding.md](finding.md). Written 2026-08-18 at intake; the load-bearing content of the
> comparison, preserved so that acting on any item later requires neither the originating conversation
> nor a live fetch of the external source.
>
> Subject spec: [specs/20260817-vault-bootstrap/architecture.md](../../20260817-vault-bootstrap/architecture.md)
> as of commit `56995ec` (CP-1 open, revised 2026-08-18, awaiting re-review).
> External source: "LLM Wiki", Karpathy — gist `442a6bf555914893e9891c11519de94f`.

## Snapshot fidelity

The gist body was fetched successfully on 2026-08-18 (`curl` against the `/raw` endpoint, HTTP 200,
11,985 bytes). It is **not** reproduced verbatim here or in `finding.md`: the snapshot is a structural
digest that names every layer, operation, file, and rationale in the source, with only short quoted
fragments. This is a deliberate reduction in durability, taken to avoid wholesale reproduction of a
third-party document. The consequence to accept: if the gist becomes unreachable, the *pattern* survives
in the digest but the author's phrasing does not. Every claim attributed to the gist below is traceable
to the digest in `finding.md`, so no item in this analysis depends on re-fetching.

The **comment thread** is weaker still — available to this session only as a WebFetch summary, never as
text. Item 8 below rests on it and is marked accordingly. Anyone acting on item 8 should re-read the
comments first.

## The framing that makes the comparison useful

The two documents are not competing designs of the same thing. They are adjacent halves that neither
one owns:

- The gist is a **lifecycle doctrine** — what the knowledge base *does* on days 1–1000. It explicitly
  declines to specify structure, closing with a note that the reader's own agent should instantiate the
  specifics.
- `vault-bootstrap` is a **bootstrap mechanism** — what exists at commit 1, and how to get there in one
  session without re-deriving it by hand.

So the gaps run almost entirely in one direction. The spec produces a vault with a strong skeleton, a
hazard gate, a portability invariant, and a falsification loop — **and no verbs**. The gist is
nine-tenths verbs.

This framing is itself the most useful output of the comparison, and it is what makes the gaps
tractable: none of them means the spec is wrong. They mean its boundary sits earlier in the lifecycle
than the archetype's eventual need.

---

## Gaps, ranked by value-per-unit-of-change

### 1. "Lint" means two different things, and the spec ruled out the wrong one

Spec §5.4 defines a mechanical, deterministic, report-never-repair validator: link resolution, wikilink
prohibition, index coverage, area coverage, no absolute paths. §5.2b then declares an anti-goal — do not
add a staleness check, because "verified 14 months ago" is not a violation, and a checker that says it is
trains contributors to touch the date rather than re-verify the claim.

That reasoning is correct **for a checker**, and it silently forecloses the gist's `lint`, which is not a
checker. The gist's lint is an *LLM judgement pass*: contradictions between pages, claims superseded by
newer sources, orphan pages with no inbound links, concepts mentioned but lacking a page, missing
cross-references, data gaps a web search could fill — plus suggesting new questions to investigate.

Note the pairwise mismatch on orphans: validator check 3 tests **outbound** coverage (every file linked
from its area `README.md`). Orphan detection is **inbound** (nothing links here). Different failures; the
spec catches only one.

Highest leverage of the eight items, because it costs a section rather than a phase: the anti-goal
survives verbatim, scoped explicitly to the mechanical validator, and a semantic lint pass is added
alongside it as a vault operation. The spec currently has one linter where the archetype needs two.

### 2. `log.md` — the spec reasoned to its edge and stopped

OQ-2 is the tell. It wrestles with whether `history/` seeds a stub, records that the newer reference
instance's stub captured real context the operator had only said aloud and would otherwise have lost,
and reconciles the two instances by declaring the stub *a capture buffer for context that has no other
home yet — closer in kind to a finding at `status: intake` than to a history document*.

That is `log.md`, derived independently and then scoped down to a one-shot bootstrap artifact. The gist's
version is a standing append-only operational record with a deliberately greppable entry prefix
(`## [YYYY-MM-DD] ingest | Title`, so `grep "^## \[" log.md | tail -5` yields recent activity). Cheap,
mechanical, and it answers "what has this vault done lately" — which nothing in the produced vault
currently answers. `history/` is finished narrative; `specs/*/journal.md` covers spec work only;
`memory/` holds facts *about the vault*.

OQ-2's leaning ("empty by default, seeded by evidence") becomes strictly better if the capture buffer is
permanent rather than a bootstrap-time stub.

### 3. Query answers have no home — the compounding loop is missing

The gist's sharpest operational point is that good answers should be filed back into the wiki as new
pages, so that explorations compound the way ingested sources do rather than disappearing into chat
history.

Trace a synthesis through the spec's five stores (§5.2a) and it has nowhere to land. It is not an area
doc — nobody deliberately authored it. It is not `memory/`, which is scoped to durable facts *about the
vault*, not conclusions about the domain. It is not `history/` (not an episode). It is not a technique
card unless it happens to be procedural. So it goes to chat history and dies — the exact failure the
gist names.

The store-split table is otherwise the most carefully reasoned part of the spec, which makes the
omission legible: the split was derived from a **governance** question (who may read this fact) rather
than a **knowledge-accumulation** question (where does a new conclusion go). Both are needed, and the
second is absent.

### 4. No raw-sources layer — and the spec's own authoring proves the need

The gist's layer 1 is immutable raw sources: the LLM reads from them and never modifies them; they are
the source of truth. The spec's vault has `architecture/`, `operations/`, `research/`, `techniques/`,
`history/` — all layer 2, all authored conclusions. There is no corpus, no immutability rule, and no line
between what was read and what was concluded.

§5.2b substitutes conventions for the layer — `> Verified as of:` and version-pinned citation — which
pins the *pointer* but does not keep the *artifact*.

The strongest evidence that this is insufficient is in the spec's own history. §10 rates "prior art
becomes unreachable mid-design" at likelihood **certain**, and the mitigation was to transcribe
everything load-bearing into `docs/knowledge-vault-archetype-audit.md` and cite the audit rather than the
repos. That transcription *is* an immutable raw-sources layer, invented ad hoc, under precisely the
pressure the gist predicts. A vault that cites external sources will hit the same wall without having
thought about it in advance. (This very finding hit it a third time: see *Snapshot fidelity* above.)

Cheap version: one `raw/` area, a `README.md` carrying an immutability rule, tracked or gitignored per
the hazard answer. It also generalizes the conditional staging-directory asset in §5.1 — a clipped
article is exactly the artifact that lands there and exactly the one that may carry credentials, so it
wires into §5.3's hazard model rather than fighting it.

### 5. No retrieval or scale story anywhere in the NFRs

The gist bounds its own claim explicitly: index-first navigation works well at moderate scale (~100
sources, hundreds of pages) and avoids embedding-based RAG infrastructure. It names the escape hatch too
— `qmd`, hybrid BM25/vector with LLM re-ranking, available as both CLI and MCP server.

The spec's two-level scheme (`knowledge-map.md` → area `README.md` → doc) is deliberately churn-resistant:
the map changes only per *area*, and validator check 4 enforces it. That is a real advantage the gist's
single root `index.md` does not have. But it means no single artifact lists every page with a gloss,
cross-area synthesis requires reading N area READMEs, and finding a document requires already knowing
its area.

`admindoc` is ~330 tracked files — at or past the boundary the gist names. §6 has no retrieval NFR at
all, and §8's cold-reader test exercises *writing* (place a file, add the index line, leave the map
alone) but never *finding*.

### 6. The division of labor is never stated — and OQ-1 pays for it

The gist is unambiguous: the human curates sources, explores, and asks the questions; the LLM writes and
maintains all of the wiki.

The spec says agents are "first-class participants" and OQ-1 calls them "the archetype's primary
contributors" — but never states the split. OQ-1 then absorbs the cost: its leaning accepts that
human-only edits are covered only at the next agent session, "a real gap and better than none," because
the mature instance's unindexed document was added by a human-authored edit rather than an agent session.

Under the gist's model that edit does not occur. Worth flagging not because the gist's position is
obviously right for `admindoc`, but because the spec is paying for an *unstated* position with a
permanent enforcement gap. Stating the division either closes the gap by convention or makes keeping it
an explicit choice.

---

## Two challenges rather than gaps

### 7. "Prose-only, no runtime code" is already breached

§1 defines the archetype as prose-only with no runtime code. OQ-1's leaning ships an **executable
validator into the produced vault**. One artifact, deliberately — but the invariant as written does not
survive it.

Restating it as "no *application* code" costs nothing, and it matters because the current phrasing closes
a question by accident: whether a search tool or a render step may live in the vault (items 5 and the
gist's Marp / matplotlib / Dataview surface). This item is internal to the spec and would hold with or
without the gist.

### 8. Audience separation: separate vaults, or one vault with labelled stores?

*Medium confidence — comment-thread paraphrase; re-read the comments before acting.*

The team-scale lesson reported in the gist's comments is to **separate wikis by audience rather than rely
on visibility labels**. That challenges §5.2a's architecture directly: it solves audience separation
*inside* one repo, via two-kind labelling plus posture-derived location. It also challenges OQ-7, which
defers the migration and concedes that the single-operator → multi-operator transition "is exactly the
kind of change nobody schedules."

If the reported field experience is that in-repo labelling is the approach that failed at team scale,
OQ-7 is not merely an open question — it is the seam that evidence says will tear. CP-1's review focus
already asks whether the portability invariant is the right organizing axis for the split, so this
belongs in front of that checkpoint if it is taken up at all.

---

## What the spec has that the gist does not

Stated because the comparison is genuinely two-directional, and three of these are places the spec is
ahead of the published pattern:

- **The hazard doc and the hazard gate** (§5.3). The gist has nothing on content that must never be
  written down. For the gist's own first example — personal psychology and health — that is the
  highest-consequence organ in the archetype, and it is absent.
- **Mechanical validation with a falsifying test case.** The gist's lint is judgement only. The spec's
  check 3 has a *confirmed live miss* in a production instance, and §8 declares that a validator
  reporting both reference instances clean is a broken validator.
- **Absorbing being wrong.** The gist's schema is a document the human and LLM co-evolve — with no record
  of why it changed and no mechanism for a falsified premise. §5.7's amendment journal and drift-routing
  rule, plus §8's premise-falsification test ranked above the cold-reader test, have no counterpart.
- **The portability invariant.** The gist notes that the wiki is just a git repo and version history is
  free, then stops — never noticing that the agent's memory lives outside the repo. §5.2a's two organs
  and §8's fresh-clone test address a silent failure the gist does not see.
- **Bootstrap automation itself**, which the gist explicitly punts to the reader's agent.

---

## If this is ever taken up

Sizing, recorded so a future session does not re-derive it:

- **Items 1, 2, 3, 7** are section-level changes to an existing spec — `spec-amend` against
  [specs/20260817-vault-bootstrap/architecture.md](../../20260817-vault-bootstrap/architecture.md). Item 7
  is nearly a typo-class clarification; items 1–3 each add a convention plus a sentence of rationale.
- **Items 4, 5, 6** are NFR and contract additions of similar size, but item 4 also touches §5.1's asset
  table and §5.3's hazard model, so it is the largest of the amendment-shaped set.
- **Item 8** is checkpoint input, not an amendment — it argues against a decision CP-1 is already
  reviewing.
- **The lifecycle question underneath all of them** is bigger than an amendment: whether `vault-bootstrap`
  ships a vault that merely *is* well-shaped, or one that arrives knowing how to ingest, query, and lint
  itself. That is either a new §5.x with its own implementation phase, or a sibling spec that the
  skeleton makes room for. A design call, not a drafting one — which is part of why this finding closed
  rather than routed.
