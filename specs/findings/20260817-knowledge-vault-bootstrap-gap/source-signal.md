# Finding: knowledge-vault bootstrap has no skill, and the gap is the expensive part

> Written 2026-08-17 from the Finances vault spin-up session, for `waseric/ai-tools`.
> Intended as intake for a future design spec. Self-contained: a reader who was
> not in that session should be able to act on this without it.

## Signal

Spinning up the `Finances` Obsidian vault took one long session of analysis that
produced ~18 files. Exactly three of them — `specs/{mission,tech-stack,roadmap}.md`
— came from a skill (`project-constitution`). The other fifteen were derived by
hand from prior art (`sandlotminecraft/admindoc`) plus an operator interview.

That derivation is the reusable asset, and it currently exists nowhere. The next
vault of this shape re-pays the whole cost: re-reading admindoc, re-deriving the
index convention, re-deciding the `.gitignore`, re-inventing the memory layout.

**The goal is a spin-up as fast as creating a OneNote notebook.** Today it is a
multi-hour session.

## What the archetype actually is

Call it a **knowledge vault**: a git-first, Obsidian-first repository whose
content is prose, whose contributors include AI agents, and which holds no
runtime code. Two instances exist — `admindoc` (Minecraft platform ops) and
`Finances` (personal financial tooling). They converged on the same shape
independently, which is the evidence that the shape is real and not
domain-specific.

It is explicitly **one archetype among future others**. A code-bearing repo, a
research repo, or a service repo would want a different skeleton. Whatever is
built here must not become the only door.

## The invariant skeleton (what a scaffold can ship verbatim)

These carried over from admindoc to Finances with only nouns changed. This is
the copyable ~80%:

| Artifact | Role | Variability |
| --- | --- | --- |
| `CLAUDE.md` | Agent working rules: orientation, knowledge capture, session discipline, conventions, git posture | Structure fixed; ~4 domain paragraphs |
| `knowledge-map.md` | One line per area; loaded every session; says *that* a thing exists and *where*, never what | Table shape fixed; rows are the area list |
| `README.md` | Human entry point; boundary rule; where-to-read-next; how to work in Obsidian | Structure fixed |
| Area dirs + `README.md` index each | `architecture/ techniques/ operations/ research/ history/` | Set is near-invariant; one or two domain-specific dirs added |
| `<discipline>.md` | The hazard doc — see below | **Always present, always domain-specific** |
| `memory/MEMORY.md` + one file per fact | In-project agent memory | Verbatim |
| `.gitignore` | Obsidian + Claudian + iCloud + staging exclusions | Verbatim, and hard-won — see Frictions |
| `specs/` layout + `constitution.journal.md` | Feeds the spec-* family | Verbatim |
| Git: `init -b main`, local, agent-owned commits | | Verbatim |

### The three conventions that carry the most weight

1. **Every folder has a `README.md` index, and adding a document means adding its
   index line.** This is what makes a vault navigable by a cold agent. It is also
   the rule most likely to silently rot — see Frictions.
2. **Markdown links only; no `[[wikilinks]]`.** They do not render outside
   Obsidian. Memory files are the deliberate exception.
3. **`knowledge-map.md` changes only when a whole new *area* appears**, never
   per document. Without this rule the map becomes a second, worse index.

## The under-recognized piece: the discipline doc

The single highest-value document produced for Finances was
`architecture/data-discipline.md` — what may be written where, what must never
be written down, and where the boundaries on agent action sit.

`project-constitution` has no slot for this. It was written because the operator
answered "yes, real financial data will land here." admindoc has the same organ
under different names (its `Secrets` section, its privilege-workaround rule).

**Generalization: every knowledge vault has a hazard class**, and naming it up
front is worth more than any other single document. Candidates by domain:
credentials and PII (finance), secrets and privileged escalation (infra),
embargo and attribution (research), licensing and provenance (anything
ingesting third-party material).

The scaffold should *require* an answer to "what is the hazard class here?" and
refuse to produce a vault without the corresponding doc. In this session it
emerged from a question I nearly did not ask.

## The interview that worked

Five questions, in two rounds, before any file was written:

**Round 1 — shape:**
1. What is this vault actually for? (offered concrete archetypes, not open prose)
2. What do the pre-existing empty directories hold? (surfaced a real open
   question; correct answer was "undecided", which became a logged deferral)
3. Git posture — init, remote, or none?

**Round 2 — domain, from round 1's answers:**
4. The specific factual thing only the operator knows (here: migration
   direction, incumbent vs. predecessor vs. target)
5. **The hazard question** ("will real financial data land here?")

Two properties made this cheap and worth preserving:

- **Scan first, ask second.** The prior-art read and the empty-vault scan
  happened before any question, so the questions were only about what could not
  be observed.
- **Offer archetypes, not blanks.** Every question was multiple-choice with a
  recommended option. The operator's most valuable answer was a *correction to
  the framing* of a choice ("primarily a migration project, but favor structural
  elements foundational to the later uses"), which an open-ended prompt would
  have been less likely to elicit.

The operator answer that most changed the output was the instruction to keep
foundational structure for anticipated-but-not-current uses. Worth making that a
standing question: **"what will this vault probably become, and what should be
kept for that even though today's project does not need it?"**

## Frictions worth freezing into the scaffold

Each of these cost real time and would recur verbatim:

- **The `.gitignore` is not obvious and is high-stakes.** First commit staged a
  3 MB minified Obsidian plugin bundle (`realclaudian/main.js`) and generated
  Claudian metadata carrying absolute paths. Both had to be un-cached before the
  commit landed. A knowledge-vault `.gitignore` should ship known-good:
  `.obsidian/plugins/`, `.obsidian/workspace*.json`, `.claudian/`,
  `.claude/settings.local.json`, `data/raw/**` with a `!README.md` exception.
- **The gitignored staging directory needs a committed README inside it.** An
  empty ignored directory does not survive git and offers no guidance at the
  moment someone is about to drop a sensitive file into it.
- **Link validation was hand-rolled.** A throwaway Python walker checked every
  relative markdown link and flagged wikilinks. It found only false positives
  (the convention examples in prose that *say* `[text](path)` and `[[wikilinks]]`)
  — but it should be a committed script in ai-tools, with those two false-positive
  classes handled, plus an **index-coverage check**: every `.md` in an area
  directory must be linked from that directory's `README.md`. That check is the
  enforcement the most-likely-to-rot convention currently lacks.
- **iCloud + git deserves a stock paragraph.** The vault lives in iCloud Drive;
  `.git` in a sync-on-write directory can corrupt under concurrent access, and
  with no remote it is the only copy. Stated in three places in Finances; should
  be a scaffold constant, not re-reasoned each time.
- **Memory location is a decision, not a default.** The harness default is
  per-user `~/.claude/…`; this archetype relocates memory *into* the vault so it
  is self-contained and shareable. That inversion needs to be stated explicitly
  in `CLAUDE.md` or an agent will follow its default.

## Handoff shape

The clean sequence, and where the seam is:

```
scaffold  →  short interview  →  project-constitution  →  validate  →  commit
(static)     (3–5 questions)     (existing skill)         (script)    (agent)
```

`project-constitution` should stay untouched and be *delegated to*, not
absorbed. It already handles lifecycle → roadmap-vs-validation correctly, and in
this session it consumed pre-gathered discovery cleanly when told not to
re-interview. Preserving that boundary keeps the constitution skill reusable by
archetypes that are not knowledge vaults.

Note the ordering: **scaffold before constitution**. The constitution's
`tech-stack.md` needs to describe the layout, and the layout is easier to
describe once it exists.

## Open questions for the spec

- **Static template vs. generated prose.** A committed skeleton directory with
  placeholder tokens is fast, diffable, and drift-resistant; fully generated
  prose is flexible but re-derives (and re-drifts) every time. *Leaning: static
  skeleton + token substitution, with the discipline doc and the constitution as
  the only genuinely authored artifacts.*
- **How archetypes are selected.** One dispatcher skill with an archetype
  argument, versus sibling skills discovered by description. *Leaning: siblings
  sharing a common `assets/` directory — no dispatcher to maintain, and skill
  descriptions already do the routing.*
- **Does the scaffold assume Obsidian?** Both instances are Obsidian vaults, but
  little of the skeleton actually requires it beyond `.obsidian/` handling and
  the wikilink prohibition. Possibly two variants, possibly one with an Obsidian
  toggle.
- **Where does the link/index validator live**, and does it run as a hook, a
  committed script invoked at session close, or only on demand?
- **Does the scaffold create the first `history/` entry?** Finances has a
  "known episodes, not yet written up" stub that captured real context the
  operator had verbally. Cheap, and it caught something that would have been lost.

## Suggested opening prompt for the future session

> In `~/scm/gh/waseric/ai-tools`, I want a skill that spins up a **knowledge
> vault** repository — the archetype used by `sandlotminecraft/admindoc` and by
> my `Finances` Obsidian vault: git-first, Obsidian-first, prose-only, agents as
> first-class contributors, no runtime code. The goal is that spinning one up is
> as fast as creating a OneNote notebook used to be.
>
> Read the attached finding first — it distills a full manual spin-up, including
> the invariant skeleton, the interview that worked, and the frictions worth
> freezing. Then read both instances as prior art before designing anything.
>
> Constraints: it must delegate to the existing `project-constitution` skill
> rather than absorbing it; it must not become the only way to spin up a repo,
> since other archetypes will follow; and it should ship a static skeleton plus
> a validator rather than regenerating prose each time.
>
> Start with `spec-design`, not implementation — the open questions at the end of
> the finding are the ones I want resolved in the design.
