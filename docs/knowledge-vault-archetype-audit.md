# Knowledge-vault archetype — prior-art audit

> Pre-spec research. Input material for the knowledge-vault scaffold design spec, not
> authoritative doctrine. Audited 2026-08-17.
>
> **Why this document exists at all.** The two instances audited here are the only
> evidence the archetype exists, and one of them (`admindoc`) is
> reachable only from the operator's personal machine. The scaffold design will be
> continued from a context with **no access to admindoc**. Everything load-bearing
> is therefore transcribed or quoted here rather than pointed at. Treat this file,
> not the repos, as the design's source of prior art.
>
> Companion finding: [specs/findings/20260817-knowledge-vault-bootstrap-gap/](../specs/findings/20260817-knowledge-vault-bootstrap-gap/).
> That finding's `source-signal.md` is the operator's own distillation, written from
> the newer instance's spin-up. This audit checks that distillation against both instances and
> **corrects it in five places** — see §7.
>
> **Naming.** `admindoc` is named; the second instance is *the newer instance*, and its subject
> matter is deliberately withheld. Only postures, sizes, and structural choices are load-bearing
> here, so the domains are not needed. Where a domain detail carries a point (§4.4's naming trap,
> §6's area sets), it is described generically. The companion finding's `source-signal.md` is a
> verbatim operator snapshot and is not retro-scrubbed.

## 1. The two instances

| | `admindoc` | newer instance |
| --- | --- | --- |
| Domain | platform operations | a private personal domain |
| Age / maturity | ~2023 origin, deliberately reshaped 2026-05-25 | spun up 2026-08-16/17 |
| Size at audit | ~330 tracked files, ~61k lines of markdown | ~24 tracked files |
| Git posture | remote on GitHub, `main` only, hand-curated commits | **local only, no remote**, `main` only, agent commits |
| Sync layer | plain filesystem | **iCloud Drive** |
| Obsidian | primary authoring surface, `.obsidian/` committed | primary owner interface, `.obsidian/` committed |
| Code in repo | none (spokes hold code); a few tracked tool scripts under `specs/*/tools/` | none |
| Agent harness | Claude Code | Claude Code **and** Claudian |
| Governing spec for its own shape | `specs/2026-05-25-admindoc-reshape/architecture.md` (design) + `specs/2026-05-25-admindoc-bootstrap/feature.md` (Phase 1) | none — hand-derived from admindoc |

The convergence claim holds, but it is worth being precise about *why*: the newer instance did
not converge independently. Its `CLAUDE.md` ends with an explicit **Prior art** section
naming admindoc as the reference. So the evidence is not "two independent designs agreed"
— it is "one design was successfully transplanted across a domain boundary by hand, and
the transplant is what cost the session." That is a weaker convergence claim but a
*stronger* scaffold argument: the manual transplant is precisely the step a scaffold
removes.

## 2. Root-level artifacts

### 2.1 `CLAUDE.md` — the agent contract

Present in both, and the largest single structural agreement. Section-by-section:

| Section | admindoc | newer instance | Verdict |
| --- | --- | --- | --- |
| Orientation | ✅ "This is **admindoc**, the one-ring knowledge repository and design hub…" | ✅ "This is the **<name>** vault: the knowledge and design hub…" (name elided) | **invariant**; 1–2 domain paragraphs |
| Pointer to constitution | ✅ "read specs/mission.md first, check specs/roadmap.md, see specs/tech-stack.md" | ✅ same three, near-verbatim | **invariant, near-verbatim** |
| `@knowledge-map.md` transclusion | ✅ | ✅ | **invariant, verbatim** |
| Hazard block | ✅ as `## Secrets` + `## Privilege workarounds — containers` | ✅ as `## Read this before touching data` → pointer to `architecture/data-discipline.md` | **invariant organ, divergent placement** — see §4 |
| Knowledge capture | ✅ | ✅ | **invariant**, but with an inverted memory clause — see §5 |
| Session discipline | ❌ (implied by spec-session doctrine) | ✅ "A session leaves the vault in a coherent state, ready to be picked up cold." | **candidate invariant**; the newer instance's wording is the better one |
| Conventions | ✅ | ✅ | **invariant**; see §3 |
| Git posture | ✅ inside Conventions | ✅ own `## Git` section incl. iCloud caveat | **invariant**, promote to own section |
| Spec workflow | ✅ | ✅ | **invariant, near-verbatim** |
| Spec-session doctrine | ✅ 5 numbered rules incl. model floors | ❌ | admindoc-only; see §6 |
| Boundary rule | ✅ own section, repeated in README | ✅ implied (self-contained vault) | **invariant question, divergent answer** |
| Cross-repo / prior art | ✅ `## Cross-repo work`, `## Code spokes` | ✅ `## Prior art` | domain-specific |

Verbatim-quotable invariant, present in both with only the noun changed:

> **Durable knowledge is committed to this repo, not to per-operator memory.** Anything
> a teammate or a future session would need — access routes, techniques, platform facts,
> conventions, gotchas — goes in the repo, where it is shared, reviewable, and reachable.

and the index rule, which the newer instance states best:

> **Default:** write it into the area it belongs to and **add its line to that folder's
> index**. A document no index points at is a document the next session won't find.

and, in both:

> Update `knowledge-map.md` only when a whole new *area* appears — not per document.

> **Skills reference techniques by path; they never copy them.**

### 2.2 `knowledge-map.md`

Present in both. admindoc's opening is the canonical statement of the contract and is
worth shipping near-verbatim:

> Where durable knowledge lives in this repo. One line per topic area — follow the link
> for the index, then the index for the document. This file is loaded into every session,
> so it stays short by design: it says *that* a thing exists and *where*, never what the
> thing is.

Shape: a two-column table (`Area` | `What lives there`), one row per area, each linking
to that area's `README.md` (not to the directory). admindoc adds a second section,
`## Frequently needed, easy to re-derive by accident` — a short bulleted list of the
handful of facts sessions keep re-deriving. That section is **not** in the newer instance and is a
strong scaffold candidate: it is the map's highest-value half, and it is the one part
that cannot be generated (it accrues from experience). Ship it as an empty section with
its heading and a one-line explanation of what earns a bullet.

### 2.3 `README.md`

Present in both. Human entry point. Invariant sub-structure:

1. One-line identity + what the repo is.
2. **The boundary rule** — what lives here vs. what lives elsewhere. admindoc: "Knowledge
   lives here. Code lives in the code spoke." Plus a tiebreak rule worth stealing verbatim:
   *"when the two repos disagree, the spec is the source of truth for intent; the code is
   the source of truth for current behavior."*
3. **Where to read next** — a 3–4 item list pointing at `specs/mission.md`,
   `specs/roadmap.md`, `specs/tech-stack.md`, and the repo's own governing design spec.
4. **Working with this repo in Obsidian** — vault config posture, the authoring
   conventions restated for a human, the stage/commit/push procedure as a copyable block.
5. **Working with this repo in other editors** — the *non-breakage* constraint: edits from
   non-Obsidian editors must not break Obsidian's parsing of links, frontmatter, or vault
   config.
6. License.

Item 5 is admindoc-only and is a genuine invariant the operator's finding does not mention.

### 2.4 `.gitignore`

Both instances, common lines (the true invariant core):

```
.DS_Store
.trash/
.obsidian/workspace*.json
.obsidian/hotkeys.json
.claude/settings.local.json
.claude/scheduled_tasks.lock
```

Divergent lines, each with a *recorded reason* — these are the interesting ones:

| Line | admindoc | newer instance | Note |
| --- | --- | --- | --- |
| `.obsidian/plugins/` | **not ignored, deliberately** | **ignored** | Direct conflict — see §4.2 |
| `.claudian/` | n/a (no Claudian) | ignored | harness-conditional |
| `data/raw/**` + `!data/raw/README.md` | n/a | ignored | the staging-dir pattern, generalizable |
| `_site/ .sass-cache/ .jekyll-cache/ .jekyll-metadata` | present | absent | vestigial in admindoc; **do not** ship |
| canvas files | commented-out non-ignore with rationale | absent | see below |

admindoc carries a comment where a rule *isn't*, which is a technique worth copying:

> `# Obsidian canvas files are not used as authoritative documentation`
> `# (deliberately not gitignored — if a canvas is ever committed,`
> `#  it must be a conscious act, not an accident.)`

The finding's claim that the `.gitignore` is "Verbatim, and hard-won" is half right: the
six-line core is verbatim, and every line beyond it is a *decision* with a rationale
comment attached. The scaffold should ship the core plus commented, switchable blocks —
not one merged file.

### 2.5 `.obsidian/README.md`

**admindoc only, and the finding misses it entirely.** It documents which vault-config
files are tracked vs. ignored, why the community-plugin set ships empty, and why a plugin
was *removed from VCS* rather than merely disabled ("Disabling would leave its bundled jar,
config, and askpass shim in the repo — inert but tracked"). This is the doc that prevents
the exact 3 MB-bundle friction the finding reports from the newer instance's side. High-value,
near-verbatim shippable, ~1 domain sentence.

### 2.6 Other root files

- `LICENSE` — admindoc: MIT. Newer instance: none (private, no remote). Conditional on git posture.
- admindoc has one stray root file (a dated root-level document with spaces in its filename) — unindexed, space-
  bearing name, pre-reshape residue. Evidence that the index convention rots exactly as the
  finding predicts, in the mature instance, under an operator who wrote the convention.
  **This is the single strongest argument for the index-coverage validator.**

## 3. The conventions block

Both instances, in a `## Conventions` section:

- Markdown links only — `[text](path)`. **No** `[[wikilinks]]`. The newer instance adds the exception
  clause: "(memory files are the one exception, per their own format)"; admindoc has no
  memory dir so needs no exception. **The exception is memory-conditional, not absolute.**
- No Obsidian canvas files as authoritative documentation.
- No absolute filesystem paths in committed prose — repo-/vault-relative only.
- No feature branches; work lands on `main` directly.
- Hand-curated commits; spec work uses `spec: YYYY-MM-DD-<feature> — <short>`.
- Newer instance only: "Every folder carries a `README.md` index." (admindoc states this in the
  Knowledge-capture section instead.)
- Newer instance only: "Obsidian is the owner's primary interface. Agent edits must not break its
  parsing…"

## 4. The hazard organ — the finding's best insight, and where it is incomplete

The finding is right that this is the highest-value document and that
`project-constitution` has no slot for it. The audit adds three things it gets wrong or misses.

### 4.1 Placement diverges, and admindoc's answer is the more robust one

- **Newer instance:** a dedicated `architecture/data-discipline.md` (~70 lines), pointed at from
  `CLAUDE.md` via a `## Read this before touching data` section that also restates the two
  rules in short form.
- **admindoc:** **no dedicated doc.** The organ lives inline in `CLAUDE.md` as `## Secrets`
  (9 lines of rules) and `## Privilege workarounds — containers`.

But the important structural fact is that admindoc **restates the hazard rule at the point
of use**: `techniques/README.md` carries its own `## Secrets` section repeating "Name the
route, never the value" with a pointer back to `CLAUDE.md`. The rule is replicated exactly
where a contributor is about to violate it. The newer instance's `CLAUDE.md` does the same thing at one
level (summary + pointer to the full doc).

**Design consequence:** the hazard organ is not one document, it is a *three-point pattern* —
canonical doc, summary-and-pointer in the agent contract, and restatement at each point of
use where the hazard is live. A scaffold that ships only the canonical doc ships one third
of what both instances actually converged on.

### 4.2 Both instances name the same four sub-rules

Independently phrased, near-identical content — this is the most transplantable prose in
either repo:

1. **Name the route, never the value.** Say where a credential lives; never what it is.
2. **Never inline a secret in a shell command**; read from source (`--password-file`, env
   already set, keychain, docker secret).
3. **Never echo or `cat` a secret to inspect it.** Verify by using it; check shape only if needed.
4. **Redact to `<redacted>` before quoting** any config/connection string/env dump — "a
   correction after the fact does not un-persist it."

Plus, in both, the same causal observation:

> Leaks here have come from *exploration*, not routine work — inlining a credential "just to
> test" while figuring out an unfamiliar connection path. Apply the rule hardest when the
> technique is new.

And in both: **if a secret leaks, say so immediately so it can be rotated** (the newer instance adds:
in a git repo, a history rewrite may be needed as well as a rotation).

### 4.3 The newer instance adds two organs admindoc lacks

- **A data-grade table** (Raw / Derived / Illustrative → where each may live), with a
  redaction bar and the sharp guidance *"prefer inventing the example over sanitizing a real
  one, since sanitizing is where mistakes happen."* Plus a named deliberate exception
  (reconciliation figures are real numbers, and that is the point).
- **Boundaries on action** — hard prohibitions on what an agent may *do*, not just write:
  no executing financial transactions, no investment/tax advice, confirm scope before any
  write to a live book.

admindoc's analogue of "boundaries on action" is the privilege-workaround rule, which is
structurally the same thing: a named escalation that must be surfaced in-session before use
and never happen silently as an implementation detail.

**Generalized hazard-doc shape (four parts, both instances support all four):**
1. The one-to-three headline rules, stated as absolutes.
2. Credential handling (the four sub-rules above — near-verbatim shippable).
3. A grading table: what class of content may live where.
4. Boundaries on agent *action* — what the agent must never do, and what escalations must
   be surfaced before use rather than performed silently.

### 4.4 A naming trap

admindoc has `architecture/discipline.md` — which is **not** the hazard doc; it is a domain
doc about something else entirely. The newer instance has `architecture/data-discipline.md`, which **is** the
hazard doc. The scaffold must not claim the bare token `discipline`; prefer a name derived
from the hazard class itself (`data-discipline`, `secrets-discipline`, `provenance-discipline`).

## 5. Memory — the finding contradicts itself, and admindoc settles it

The finding's invariant table lists `memory/MEMORY.md` + one file per fact as **"Verbatim."**
That is wrong. **admindoc has no `memory/` directory at all**, and says so as policy:

> **The one exception:** genuinely operator-specific facts — personal preferences, individual
> tooling habits, who the operator is — belong in per-user memory under `~/.claude`. If it
> would still be true for a different admin, it is not this.

The newer instance inverts this deliberately:

> **The exception:** genuinely operator-specific facts … go in `memory/`, indexed in
> `memory/MEMORY.md`. Still inside the vault, never in `~/.claude`.

These are opposite answers to the same question, and both are correct *for their instance*:
admindoc is **multi-operator with a remote** (per-user facts must not be shared), the newer instance is
**single-operator, self-contained, no remote** (nothing should live outside the vault). The
finding's own Frictions list gets this right — "Memory location is a decision, not a default"
— so the error is confined to the invariant table.

**Design consequence:** memory location is an *interview question* whose answer follows from
the sharing posture, not a shipped default. It also determines whether the wikilink prohibition
needs its memory-file exception clause, so the two decisions are coupled.

> **Superseded in part — see Appendix A.** Both quotes above concern **user-oriented** facts.
> Neither instance names a **repo-oriented** memory category, and the newer instance can commit
> personal facts safely only because it is single-operator. A third instance keeps both stores at
> once and states the boundary, which reframes the "location" question as a *kind* question. The
> spec's §5.2a supersedes this section's design consequence; the observations above stand.

The newer instance's `memory/` holds 2 facts + index after one session (`agent-drives-desktop-tools`,
`migration-priorities`), which suggests the in-vault variant does get used when it exists.

## 6. Area directories

Observed sets:

- **admindoc:** `architecture/ techniques/ operations/ research/ history/` + `permissions/`
  `plugins/` + one domain-specific area + `specs/`
- **Newer instance:** `architecture/ techniques/ operations/ research/ history/` + `data/`
  `repositories/` `memory/` `specs/`

The five-name core is confirmed exactly as the finding states. Every area dir in both repos
carries a `README.md`. Notable per-area findings:

- **`techniques/` has a contract, not just an index.** admindoc's `techniques/README.md`
  defines a four-part **card shape** — (1) what it reaches/decides, one line, so the index row
  is sufficient to triage; (2) route; (3) **gotchas** — "the things that cost someone an hour.
  This is the most valuable section and usually the reason the card exists"; (4) verify,
  including "how to tell 'no signal' from 'not reachable.'" Plus a landing rule: *"A technique
  lands here the moment it is discovered — not after it has been used twice, and not inside the
  skill that happened to learn it."* Both repos reference "that README's card shape" from
  `CLAUDE.md`, so the card shape is load-bearing across instances. The finding does not mention
  it. **Ship it near-verbatim.**
- **Index format varies by area and that is fine.** `techniques/` uses grouped two-column
  tables (`Card` | `Reaches`/`Decides`/`Covers`); `history/` uses a `Document` | `What it covers`
  table; `operations/` uses a bulleted list with trailing em-dash glosses. The invariant is
  "one row/line per document, with a one-line gloss," not a fixed table.
- **`history/` — prior art answers the finding's open question.** admindoc's reshape spec
  OQ-6 resolved to: ship the directory **empty at Phase 1, populate on first natural addition**,
  with the README stating the inclusion rule and its two exclusions ("not for changelog-style
  entries (use `git log`), not for runbook content"). The newer instance instead seeded a "known episodes,
  not yet written up" stub. Both are defensible; the admindoc answer is the governed one.
- **`operations/` carries a dated `changelog/` subdirectory** with its own README index
  (admindoc: ~40 dated entries). The newer instance has no changelog. Candidate optional area.
- **Cross-area routing sentences.** Each area README ends by naming what belongs *elsewhere*
  ("For *how to reach* a data surface … see techniques/"; "design rationale belongs in
  architecture/"). This is what keeps areas from bleeding. Cheap to template, easy to miss.

## 7. Corrections to the finding

| # | Finding claim | Audit result |
| --- | --- | --- |
| 1 | `memory/MEMORY.md` + one file per fact — "Verbatim" invariant | **Wrong.** admindoc has no `memory/` and routes per-operator facts to `~/.claude` by policy. It is a decision driven by sharing posture (§5) — as the finding's own Frictions list says. |
| 2 | `.gitignore` ships "Verbatim" | **Half right.** A six-line core is verbatim; everything else is a per-instance decision with a rationale comment, and the two instances *directly conflict* on `.obsidian/plugins/` (§2.4, §4.2). |
| 3 | admindoc's hazard organ = "its Secrets section, its privilege-workaround rule" | **Right, and incomplete.** The organ is a three-point pattern — canonical statement, summary in the agent contract, restatement at each point of use (§4.1). Also: four credential sub-rules are near-verbatim across both instances (§4.2). |
| 4 | Invariant list omits: `.obsidian/README.md`; the `techniques/` card shape; the README "other editors / non-breakage" section; `knowledge-map.md`'s "frequently re-derived" section; per-area cross-routing sentences | **Five additions**, all present in admindoc and all near-verbatim shippable (§2.5, §6, §2.3). |
| 5 | "Two instances converged independently, which is the evidence the shape is real" | **Overstated.** the newer instance names admindoc as prior art in its own `CLAUDE.md`. This was a hand transplant, not independent convergence (§1). The scaffold argument survives — arguably strengthens — but the evidence should be described honestly in the spec. |

Not a correction, but the sharpest new datum: admindoc, the mature instance whose operator
*wrote* the index convention, still has one unindexed root-level document with spaces in its
filename. The convention rots. §2.6.

## 8. Design input the audit adds

1. **The archetype has a governed precedent for its own bootstrap.** admindoc's
   `specs/2026-05-25-admindoc-reshape/architecture.md` is a design spec for reshaping a repo
   into this archetype, with 7 phases, 4 review checkpoints, and 6 open questions. Its section
   set is a usable outline for the new spec, and three of its OQs are the *same questions* the
   new finding raises independently — OQ-1 (Obsidian git-plugin posture), OQ-5 (memory
   directory naming/location once the vault is primary), OQ-6 (what goes in `history/`).
   **Two designs of this archetype hit the same three open questions.** That is the real
   convergence evidence, and it says those three must be interview questions or shipped
   decisions, never left implicit.
2. **A vault can host its own skills.** admindoc tracks `.claude/skills/` (two vault-local
   skills). The scaffold should decide whether to create that directory.
3. **`specs/docs/` is a real pattern** — admindoc parks non-spec doctrine documents there
   (community rules, moderation doctrine, a BBCode ground truth). Distinct from top-level areas.
4. **`specs/constitution.journal.md` has a defined contract:** it journals amendments to the
   three constitution files, which carry no status banner and no per-file journal of their own.
   Both instances have the file. Ship it with that explanatory header.
5. **Both instances already run the `spec-*` family** and admindoc encodes a five-rule
   spec-session doctrine in `CLAUDE.md` (mechanical re-derivability, batch-by-default with
   operator-owned stops, model floors, starve-context-not-verification, verification-wins
   tiebreak). This is the block that makes a vault interoperate with this repo's skills; per
   `ai-tools`' own rule that content must not name specific consumers, the scaffold ships the
   doctrine's *shape* with the model-floor ladder as a token, not admindoc's specific text.
6. **iCloud + git**, verbatim-shippable as a conditional block: let sync settle before
   committing; avoid concurrent edits from two machines; with no remote this is the only copy.
7. **`git add .` is named as the specific hazard** in the newer instance's git section — "the most likely
   way something from `data/raw/` or an untracked scratch file ends up in history." Pairs with
   the staging-directory pattern.

## 9. Validator requirements (from observed rot, not speculation)

The finding proposes a link/index validator. The audit supplies its concrete checks:

1. **Relative-link resolution** — every `[text](path)` target exists.
2. **Wikilink prohibition** — flag `[[...]]`, with two known false-positive classes: prose
   that *documents* the convention, and memory files (when the in-vault memory variant is chosen).
3. **Index coverage** — every `.md` in an area directory is linked from that directory's
   `README.md`. Confirmed to catch a real miss in the mature instance (§2.6).
4. **Area coverage** — every area directory appears as a row in `knowledge-map.md`, and every
   `knowledge-map.md` row resolves to an existing `README.md`.
5. **No absolute filesystem paths in committed prose** — an explicit convention in both
   instances with no enforcement in either.

Checks 3–5 are the ones neither instance can currently enforce, and 3 has already failed in
production.

---

## Appendix A — third instance: the knowledge-store model

Transcribed 2026-08-17 from an internal capability repository, commits `51a5076..c177707`. Not
reachable outside the operator's environment, hence transcription rather than citation — the same
durability constraint that produced this audit (spec §3).

**What this instance is, and why it is not a third vault.** A specialized multi-modal knowledge
repository with its own distribution management: a published content tree with a manifest, version
bumps, sync to an open-ended and unseeable consumer set, and a dual-environment constraint. Its
functional requirements exceed a knowledge vault's. It satisfies the archetype's *observable*
properties — git-first, prose-only, no runtime code, agents as first-class contributors, area
directories with index READMEs, a root agent contract — which is worth noting as a limit of those
properties as a discriminator, but the operator's judgment is that this repo's needs are a superset
and that meeting them would be over-delivery for the vault archetype. Treated as prior art for the
*store model only*.

### A.1 The five stores

Its agent contract §3, "Where knowledge goes" — "Four stores, four jobs. Putting a fact in the wrong
one either bloats every session or hides it from the agent that needs it." Table, transcribed:

| Store | Loads | In git | Use for |
|---|---|---|---|
| `CLAUDE.md` | Always, in full | Yes | Facts needed in every session. Keep under 200 lines. |
| `.claude/rules/*.md` | Always, or path-scoped via `paths:` frontmatter | Yes | Topic instructions; scope them so they cost nothing until relevant |
| `.claude/skills/*/SKILL.md` | Name + description always; body on demand | Yes | Procedures |
| Auto memory | `MEMORY.md` index only; topic files on recall | Optional — machine-local pointer | Learnings discovered in session |

Closing rule: "Prefer the cheapest store that reaches the right audience. If a section of this file
grows into a procedure, move it to a skill."

The fifth store is named in its §4: operator-specific and machine-specific material goes in
`CLAUDE.local.md` (gitignored) or per-user memory — "never in a committed rule or skill."

### A.2 The two mechanical constraints

Stated in the same section as "two consequences worth holding":

1. **Auto memory does not reach subagents.** A dispatched worker or reviewer "sees `CLAUDE.md` and
   `.claude/rules/`, and nothing else from this list. Anything a worker must know belongs in-repo."
   This is the stated reason the instance relocated memory into the repository at all.
2. **`@imports` in `CLAUDE.md` do not save context** — imported files load at launch too. "Use a
   path-scoped rule or a skill instead."

### A.3 The uncommittable-configuration organ

A `workspace-setup` skill (~113 lines), advertised by description and loaded on demand, whose
premise is that some configuration cannot be committed and a fresh clone is therefore
under-configured until someone runs it. Load-bearing content:

- The memory-directory pointer must be an absolute or `~/`-prefixed path, so any committed value
  "would be wrong on every other machine." The pointer lives in the gitignored local settings file;
  **only the content is committed**.
- Establish the repo root from `git rev-parse --show-toplevel`; never hardcode a home directory.
- **Report state before changing anything.** If already pointed at the in-repo directory, "report
  that and stop — do not rewrite the file." Idempotent by construction.
- Two consequences it requires stating to the user explicitly: memories become **team-visible**, so
  operator-specific facts belong elsewhere; and **changing the setting does not migrate existing
  memories** — "offer it, do not perform it silently."
- The setting "takes effect only after the workspace-trust dialog is accepted for this folder. If
  memory writes are still landing elsewhere, that dialog is the first thing to check."
- Verification is confirming the agent contract appears as a loaded memory file: "If it does not,
  the file was not loaded and nothing else in this setup is trustworthy."
- Prune stale local permission entries; "removing a permission entry is safe: the worst outcome is
  one additional prompt."
- Authored to be liftable: "makes no assumption about which repository it is in beyond reading
  `CLAUDE.md`."

### A.4 The committed-memory index header

Its `MEMORY.md` opens with an HTML comment, not prose: the index is "committed and team-visible";
"only the first 200 lines / 25KB load at session start — keep one line per entry and put detail in
the topic file"; and operator-specific, machine-specific, or credential-adjacent facts "do NOT
belong here." Point-of-use restatement applied to the memory store — the same three-point hazard
pattern this audit documents in §4.

Its entries are one line each, `- [Title](file.md) — hook`, and each points at the artifact it
concerns rather than restating it.

### A.5 Path-scoped rules in practice

Two rules, both with `paths:` frontmatter. One scopes spec-pipeline doctrine — model floors,
dispatch, context budget — to `specs/**/*.md`; the other scopes a single in-flight design spec's
working context to that spec's directory and its related content paths, and states plainly that the
spec "is authoritative. This rule carries only what an agent needs *before* reading it."

Two transferable observations: the spec-doctrine block is the block most likely to go stale (this
audit's §2.1 found the same in the mature vault instance, carrying an obsolete model tier), and it
is needed only when a spec file is open — so it belongs out of the always-loaded contract. And a
rule scoped to one spec is *temporary by design*, deleted when the spec closes.

### A.6 Durability conventions

- References carry `> Verified as of: YYYY-MM-DD` at line 1.
- "Validated knowledge. Claims come from real build cycles. Mark provenance where a claim is
  measured-but-not-shipped, or inferred."
- External sources are cited by immutable version-pinned permalink, "never by local path or branch
  tip"; a local checkout "is a convenience for reading; it is never the citation."
- No repository outside the current one is ever modified from it — content reaches other
  repositories through the declared sync path, never by direct edit.
- The list of sibling repositories is explicitly "the evidence set for work currently in flight,
  not the consumer set and not a fixed list. Add and remove rows as specs come and go."

### A.7 Cross-session handoff via memory

One commit writes two memory entries specifically so a queued spec amendment can be picked up in a
fresh session: a `project` entry carrying the queued changes, their timing constraint, and the
judgment calls that must survive; and a `feedback` entry carrying the reusable lesson the episode
taught. Both "point at the finding rather than restating it." This is the discipline that makes an
interrupted spec resumable, and it is a session-discipline convention rather than an artifact.
