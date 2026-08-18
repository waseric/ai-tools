# vault-bootstrap — Architecture and Protocol Specification

> Status: Draft — Open for Review (CP-1 changes requested 2026-08-17; blockers remediated 2026-08-18, awaiting re-review)
> Date: 2026-08-17
> Author: waseric
> Audience: the operator; contributors to this skill family; AI agents executing `vault-bootstrap` or a sibling archetype skill; adopters who install the skill without this repo

## 1. Overview

`vault-bootstrap` is a skill that spins up a **knowledge vault** repository: a git-first, prose-only repository with no runtime code, whose contributors include AI agents as first-class participants, and (optionally) whose primary human authoring surface is Obsidian. It produces the repository skeleton, conducts a short interview for the facts a scan cannot observe, authors the one genuinely domain-specific document (the hazard doc), delegates the constitution to the existing `project-constitution` skill, validates the result, and commits.

The architectural commitment is threefold. First, that **repository bootstrap is archetype-shaped** — there is no universal repo skeleton, so `vault-bootstrap` is deliberately one of a planned family of sibling archetype skills and must not become the only door. Second, that the ~80% of a knowledge vault that is invariant across instances ships as **static assets with token substitution**, not as prose regenerated per invocation, because regenerated prose re-derives and re-drifts every time. Third, that the conventions a knowledge vault depends on most — every folder carries an index; markdown links only; the knowledge map changes only per *area* — are **mechanically validated**, because the audit found the most load-bearing of them already rotted in the mature reference instance.

The target is that spinning up a knowledge vault costs a short interview and a commit, rather than the multi-hour manual derivation session that produced the two existing instances.

## 2. Goals and Non-goals

### Goals

- A new knowledge vault reaches a committed, coherent, cold-readable state in one session, with operator effort dominated by a 3–5 question interview rather than by authoring.
- The invariant skeleton is captured once as versioned assets, so instance N+1 does not re-derive it from instance N by hand.
- The hazard class of the new vault is named before the vault is produced; a vault without its hazard doc is not a valid output.
- The three rot-prone conventions (index coverage, markdown-links-only, area-level knowledge-map changes) are enforced by a validator rather than by discipline.
- `project-constitution` remains untouched and is delegated to, preserving its reusability for archetypes that are not knowledge vaults.
- The skill remains a portable atomic unit per the [Atomic-Skill Portability Principle](../tech-stack.md), functioning when installed standalone against an unrelated repo.
- The design leaves room for sibling archetype skills without requiring a dispatcher or a shared runtime dependency between them.

### Non-goals

- **Not** a general-purpose repo initializer. Code-bearing repos, research repos, and service repos are different archetypes with different skeletons and are out of scope (§12).
- **Not** a replacement for or wrapper around `project-constitution`. The constitution stays that skill's product.
- **Not** an Obsidian plugin, Obsidian-specific tooling, or a vault-migration tool. Existing vaults are prior art, not adoption targets (§11 covers what partial adoption by an existing vault does and does not offer).
- **Not** a content generator. The skill produces structure, conventions, and one authored hazard doc; it does not write the vault's domain knowledge.
- **Not** a git remote provisioner. Remote creation, hosting choice, and access control stay with the operator.

## 3. Background and Constraints

### Originating finding

This design is routed from [specs/findings/20260817-knowledge-vault-bootstrap-gap/](../findings/20260817-knowledge-vault-bootstrap-gap/finding.md) (`status: routed`, domain `methodology`, severity `important`). That finding's `source-signal.md` is the operator's own distillation, written immediately after the newer instance's spin-up.

### Prior art, and the constraint that it is going away

Two instances of the archetype exist:

- **`admindoc`** — platform operations. ~330 tracked files, deliberately reshaped in 2026-05 under its own governing design spec (`specs/2026-05-25-admindoc-reshape/architecture.md`, 7 phases, 4 checkpoints, 6 open questions). Multi-operator, GitHub remote.
- **The newer instance** — a private personal domain. Spun up 2026-08-16/17, ~24 tracked files. Single-operator, local-only, no remote, iCloud-synced.

**Naming.** `admindoc` is referred to by name; the second instance is *the newer instance* throughout, and its subject matter is deliberately not stated. The instances' domains are not load-bearing for any claim in this spec — only their postures, sizes, and structural choices are — so withholding them costs the design nothing. Where a domain detail is genuinely needed to make a point (as in §5.3's naming trap), it is described generically.

**Both are reachable only from the operator's personal machine, and this design will be continued from a context that cannot reach `admindoc`.** Every load-bearing fact about the prior art has therefore been transcribed into [docs/knowledge-vault-archetype-audit.md](../../docs/knowledge-vault-archetype-audit.md), which is this spec's authoritative prior-art source. Downstream sessions cite the audit, not the repos. The audit corrects the originating finding in five places; §4 and §5 below are built on the audit's corrected account, not on the finding's table.

The convergence evidence is weaker than the finding claims and the audit says so plainly: the newer instance names `admindoc` as prior art in its own `CLAUDE.md`, so this was a successful hand transplant across a domain boundary, not independent convergence. That weakens the "the shape is objectively real" claim and strengthens the scaffold claim — the hand transplant is precisely the step being automated.

The stronger convergence datum is different: **two independent design efforts against this archetype hit the same three open questions.** `admindoc`'s reshape spec asked about Obsidian git-plugin posture (its OQ-1), memory directory location once the vault is primary (OQ-5), and what belongs in `history/` (OQ-6). The originating finding, written 15 months later by the same operator against a different domain and without consulting those OQs, raised all three again. Questions that recur across independent designs of the same archetype must be shipped as explicit interview questions or shipped as decisions — never left implicit. This is the single most load-bearing input to §5.2.

### Host-repo constraints

- **Skills are authored in `.agents/skills/<name>/` and deployed to `~/.claude/skills/<name>/`.** Both copies must match at task closeout ([CLAUDE.md](../../CLAUDE.md), the deploy-sync rule). A skill that ships asset files makes the deploy-sync check a whole-directory diff, not a `SKILL.md` diff.
- **Skill masters carry uniform frontmatter only** — `name`, `lastUpdated`, `description`. No harness-specific keys. Model floors and behavioral contracts live in the skill's prose.
- **The Atomic-Skill Portability Principle** ([specs/tech-stack.md](../tech-stack.md)) requires that a skill self-contain its workflow, schema knowledge, and default templates, with no runtime dependency on host-repo files; discover and adapt to richer host embodiments when present; and degrade cleanly when absent. This principle is the binding constraint on §5.5 and it **contradicts the originating finding's leaning** on archetype selection.
- **Content must not name specific consumers.** The skill cannot reference `admindoc` or the newer instance in its shipped output; the audit exists in `docs/` precisely so the domain-specific evidence lives outside the distributable surface.
- **This repo declares a `## Grammar` block** in [specs/tech-stack.md](../tech-stack.md); it is codified forward into this spec's journal per §5.7.
- Single branch `main`, no CI, no build step, no deployment target. A validator therefore cannot be a CI check in this repo; §5.4 and OQ-1 address what it can be.

### Spec and code location

Spec and skill master live in the same working tree (`waseric/ai-tools`), so no multi-repo split applies to authoring this skill:

- `SPEC_REPO_ROOT` = `waseric/ai-tools`
- `SPEC_TARGET_BRANCH` = `main`

The *produced* vault is always a third repository, distinct from both. `vault-bootstrap` runs against a target directory that is not this repo and not the skill's install location.

## 4. Architecture

### Topology

```
                        ┌──────────────────────────────────────┐
   operator invokes ───▶│  vault-bootstrap  (this skill)       │
                        ├──────────────────────────────────────┤
                        │ P1 SCAN     target dir + git state   │
                        │ P2 INTERVIEW 3–5 archetype questions │
                        │ P3 MATERIALIZE skeleton + tokens     │
                        │ P4 AUTHOR   hazard doc               │
                        │ P5 DELEGATE ──────┐                  │
                        │ P6 VALIDATE       │                  │
                        │ P7 COMMIT         │                  │
                        └───────────────────┼──────────────────┘
                                            │ session context
                                            ▼
                              ┌───────────────────────────┐
                              │   project-constitution    │
                              │   (existing, untouched)   │
                              │  → specs/mission.md       │
                              │    specs/tech-stack.md    │
                              │    roadmap.md|validation.md│
                              └───────────────────────────┘

   assets shipped inside the skill directory:
     _skeleton/     token-bearing invariant files (§5.1)
     _hazard/       hazard-doc frame + credential-rules block (§5.3)
     validate       the convention validator (§5.4)
```

The ordering is load-bearing and inverted from intuition: **skeleton before constitution.** `project-constitution`'s `tech-stack.md` describes the vault's layout and authoring conventions, and a layout is easier to describe accurately once it exists on disk than to predict. The originating finding identifies this and the audit confirms it against both instances.

### Vocabulary

| Term | Definition |
| --- | --- |
| **Knowledge vault** | A git-first, prose-only repository with no runtime code, agents as first-class contributors, organized as area directories each carrying a `README.md` index, with a root agent contract and knowledge map. Optionally Obsidian-first. |
| **Archetype** | A repository shape with its own invariant skeleton. `vault-bootstrap` implements exactly one. Siblings implement others. |
| **Invariant skeleton** | The set of files that carry over between instances of one archetype with only tokens changed. |
| **Token** | A named substitution point in a skeleton asset (`{{VAULT_NAME}}`, `{{HAZARD_DOC}}`, …). Resolved from scan + interview. |
| **Hazard class** | The category of content that must never be written down, or must be written only under constraint, in this particular vault: credentials and PII, secrets and privilege escalation, embargo and attribution, licensing and provenance. |
| **Hazard doc** | The vault's canonical statement of its hazard class and the boundaries on agent action. Always present, always domain-specific, never shipped verbatim. |
| **Area directory** | A top-level content directory carrying a `README.md` index and appearing as one row in `knowledge-map.md`. |
| **Agent contract** | The vault's root `CLAUDE.md` — orientation, knowledge capture, session discipline, conventions, git posture. |
| **Sharing posture** | Whether the vault is single- or multi-operator, and whether it has a remote. **Shared is the expected default** (§5.2). Determines the license, the hazard doc's sharing assumptions, and which memory store a given fact belongs in. |
| **Repo-oriented memory** | A durable fact about *the vault* — its founding intent, a settled decision, a correction that must survive the session that found it. Belongs to the repository, is committed, and is team-visible (§5.2a). |
| **User-oriented memory** | A fact about *an operator* — personal preference, working habit, machine path, credential-adjacent state. Never committed; stays in the harness-default memory location or an ignored local file (§5.2a). |
| **Knowledge store** | One of the places a fact can be put, each with its own load cost and audience: the agent contract, a path-scoped rule, a skill, repo-oriented memory, user-oriented memory (§5.2a). |

### Composition rules

1. **One archetype per skill.** `vault-bootstrap` never branches on archetype. A second archetype is a second skill.
2. **No shared runtime surface between sibling skills.** Each archetype skill self-contains its own assets, even where two archetypes would ship a near-identical file. Duplication across skills is accepted deliberately; see §5.5.
3. **The scaffold owns structure; the constitution owns intent.** No file produced by `vault-bootstrap` restates what `mission.md` says, and `vault-bootstrap` never writes into `specs/mission.md`, `specs/tech-stack.md`, `specs/roadmap.md`, or `specs/validation.md`.
4. **Every produced file is either a token-substituted asset or an authored artifact, and the skill states which.** There is no third category. The authored set is exactly: the hazard doc, and (via delegation) the constitution.
5. **The hazard doc gates output.** No hazard class named → no vault produced.

## 5. Detailed Design

### 5.1 The skeleton asset set

**Purpose.** Ship the invariant ~80% of a knowledge vault as versioned files rather than re-deriving it per invocation.

**Shape.** A `_skeleton/` directory inside the skill, mirroring the target layout. Files carry `{{TOKEN}}` substitution points. Derived from the audit's confirmed-invariant set:

| Asset | Substance | Token load |
| --- | --- | --- |
| `CLAUDE.md` | Agent contract. Sections, in order: Orientation; the boundary that costs most if missed; `@knowledge-map.md`; hazard summary + pointer; **Where knowledge goes** (the store-routing table, §5.2a); Knowledge capture; Session discipline; Conventions; Git; Spec workflow | 1–2 domain paragraphs + `{{BOUNDARY_RULE}}`, `{{MEMORY_CLAUSE}}`, `{{GIT_POSTURE}}`, `{{HAZARD_SUMMARY}}` |
| `knowledge-map.md` | Header prose verbatim (audit §2.2); area table; empty `## Frequently needed, easy to re-derive by accident` section with its inclusion rule | area rows |
| `README.md` | Human entry: identity; boundary rule + intent-vs-behavior tiebreak; Where to read next; Working in Obsidian *(conditional)*; Working in other editors — the non-breakage constraint; License *(conditional)* | `{{VAULT_NAME}}`, `{{BOUNDARY_RULE}}` |
| `<area>/README.md` × 3 | Index scaffold for `architecture/`, `operations/`, `research/`, each ending with its cross-area routing sentence. The other two core areas — `techniques/` and `history/` — carry their own rows below, with different substance | area gloss |
| `techniques/README.md` | The four-part **card shape** contract — what it reaches/decides · route · gotchas · verify — plus the landing rule and a point-of-use hazard restatement. Near-verbatim from audit §6 | `{{HAZARD_POINTER}}` |
| `history/README.md` | Inclusion rule plus the two exclusions (not changelog — use `git log`; not runbook) and the populate-on-first-addition rule | — |
| `.gitignore` | A **seven-line** core, then commented switchable blocks: the audit's six verbatim lines (which already include the local settings file) plus one addition, `CLAUDE.local.md`, as the named home for operator-specific facts (§5.2a) | posture switches |
| `.obsidian/` + `.obsidian/README.md` | Vault config and the tracked-vs-ignored doc *(conditional on Obsidian)* | — |
| `specs/constitution.journal.md` | Amendment journal with its explanatory header | — |
| `memory/MEMORY.md` | Index stub, plus a header carrying its own load limit (index-only, first ~200 lines) and the point-of-use restatement that this file is committed and team-visible, so operator-specific facts do not belong in it | — |
| `.claude/rules/<topic>.md` | Path-scoped rule with `paths:` frontmatter. Shipped only where the design actually places one (§5.2a): the point-of-use hazard restatement, and the spec-session block scoped to `specs/**` | `{{HAZARD_POINTER}}` |
| `.claude/skills/workspace-setup/SKILL.md` | The fresh-clone procedure for configuration that cannot be committed — chiefly the memory-directory pointer, which needs a machine-absolute path. Idempotent; reports and stops when already configured. Unconditional — §5.2a makes the repo-oriented store part of every vault, so this asset has no off-state | `{{MEMORY_DIR}}` |
| `CLAUDE.md` § Git — sync-layer block | The iCloud-plus-git conditional block, verbatim-shippable per audit §8.6: let sync settle before committing; avoid concurrent edits from two machines; with no remote this is the only copy. Emitted only when the git-posture answer reports a sync-layer-backed working tree *(conditional)* | `{{GIT_POSTURE}}` |
| staging dir + `README.md` | Gitignored-dir-with-committed-README pattern *(conditional on a staging need)* | `{{STAGING_DIR}}` |

**Behavior.** The skill copies assets, substitutes tokens, and omits conditional assets whose gate is off. It never partially writes: a failed substitution (unresolved token) aborts before any file lands.

**One correction to the contract's token economy.** The `@knowledge-map.md` import in `CLAUDE.md` is there so the map is *always* in context, not to defer its cost: an `@import` loads at session launch exactly as inlined text would. The import buys single-sourcing, not laziness. Anything genuinely wanted *on demand* has to be a path-scoped rule or a skill instead (§5.2a) — which is why those two stores appear in the asset table at all.

**Pattern invoked.** Template-with-substitution, the same mechanism `finding-intake` and `finding-triage` already use for `_template/finding.md` and `_template/journal.md`, including their host-override resolution policy (host copy wins when present, bundled default otherwise). Reusing the family's existing idiom rather than inventing one.

**Why this design.** The finding's leaning, confirmed by the audit: static assets are fast, diffable, and drift-resistant, and the audit found that ~80% of both instances' non-constitution content is identical modulo nouns. Generated prose would re-derive that 80% and re-drift on each generation.

**Alternatives considered.** Fully generated prose — rejected: no diffable baseline, and drift between instances is exactly the cost being eliminated. A cookiecutter-style external template tool — rejected: adds a runtime dependency, and this repo has no build step or package manager.

### 5.2 The interview

**Purpose.** Ask only what a scan cannot observe, and force an answer to the hazard question.

**Shape.** Two rounds, and within them **two tiers**. Round 2's content depends on Round 1's answers. Every question is multiple-choice with a recommended option; none is open prose.

The tiers are the answer to §6's adoptability target. **Four questions are required** — purpose, posture, Obsidian, hazard — and answering only those four produces a valid, coherent, committed vault. The remaining three are an explicitly **skippable depth round**: they improve the output materially, and the design says so, but none of them gates it. The skill states which tier it is in when it asks, so an operator in a hurry knows exactly where the floor is and an operator with ten minutes knows what the extra six minutes buy.

**Round 1 — shape.**
1. **[required] What is this vault for?** Offer concrete archetype-fit options, not a blank.
2. **[depth] What do the pre-existing directories hold?** (When the scan finds any.) "Undecided" is a valid answer and becomes a logged deferral rather than a stall.
3. **[required] Git posture** — init / existing repo / remote or none — and **sharing posture**: single- or multi-operator. **Shared is the recommended default**, because a knowledge vault that is worth keeping tends to acquire a second reader, and the conventions that make sharing safe are expensive to retrofit. A single-operator answer is a deliberate narrowing, not the baseline.
4. **[required] Obsidian** — is this an Obsidian vault? Gates the `.obsidian/` assets, the wikilink prohibition's exception clause, and the canvas rule.

**Round 2 — domain, derived from Round 1.**
5. **[depth] The domain fact only the operator knows.** Question text is generated from Round 1's archetype answer.
6. **[required] The hazard question** — "what class of content must never be written down here?" — with candidates offered by domain: credentials and PII; secrets and privilege escalation; embargo and attribution; licensing and provenance.
7. **[depth] The anticipated-use question** — *"what will this vault probably become, and what should be kept for that even though today's project does not need it?"* The originating finding records that the operator answer which most changed the manual output was exactly this, volunteered rather than asked. It becomes a standing question.

**Behavior — the answers the operator never gives.** Two things the interview does not ask: one because the archetype simply decides it, one because it follows from Round 1.

- **Memory splits by *kind*, not by posture.** Repo-oriented memory goes in-vault and committed; user-oriented memory stays outside it. Sharing posture does not move that boundary — it only changes how much it costs to get wrong. See §5.2a, which replaces what earlier drafts derived here.
- **`history/` ships empty**, with its README stating the populate-on-first-addition rule (OQ-2 revisits the seeded-stub variant).

**The conditional prompts.** Two questions exist outside both tiers because their *gates* are derived rather than asked. The **license** gate: remote-bearing → ask, local-only → omit entirely. The **citation-durability** gate (§5.2b): asked only when the vault will cite sources outside itself. When a gate opens, the choice itself is a real question and is counted as one against the interview budget rather than hidden inside a derivation — but neither can fire on the minimum path unless the posture answer opens it.

**Skipping the depth round.** A skipped depth question is recorded as a logged deferral in the produced vault, not silently dropped, so a later session can see what was not asked. The anticipated-use question in particular is the one the originating finding names as having most changed the manual output; the skill therefore recommends the depth round rather than treating the four-question path as the expected case.

**Pattern invoked.** Scan-first-ask-second, as already implemented by `project-constitution`'s Phase 1 SCAN → Phase 2 CLARIFY sequence and `finding-intake`'s defer-every-optional-field discipline.

**Why this design.** The finding names the two properties that made the manual interview cheap and worth preserving: scan before asking, and offer archetypes rather than blanks. The audit adds the empirical argument for questions 4 and 6 and for the memory derivation: those three questions recurred across two independent designs of this archetype 15 months apart, which is evidence they are archetype-intrinsic rather than instance-specific.

**Alternatives considered.** A single-round interview — rejected: the domain and hazard questions cannot be phrased well without Round 1's archetype answer. Open-prose questions — rejected: the finding records that the operator's most valuable answer was a *correction to the framing of a choice*, which an open prompt is less likely to elicit.

### 5.2a Where knowledge goes — the store split

**Purpose.** Give every fact exactly one right home, and make the two *kinds* of memory distinguishable, so a shared vault does not leak one operator's habits into the team's knowledge or lose a durable decision to a personal store.

**The distinction that has to be first-class.** A knowledge vault accumulates two categories of fact that look alike at the moment of capture and diverge completely in where they belong:

| | **Repo-oriented** | **User-oriented** |
| --- | --- | --- |
| About | The vault: founding intent, settled decisions, corrections that must outlive the session | An operator: preferences, working habits, machine paths, credential-adjacent state |
| Home | `memory/` inside the vault | Harness-default memory location, or `CLAUDE.local.md` |
| In git | **Yes** — committed, team-visible | **Never** |
| Survives | A machine change, and reaches every other reader | Only that operator, only that machine |

Because a shared vault is the default (§5.2), this is not an optional refinement. The failure is asymmetric and both directions are silent: a personal habit committed into `memory/` becomes an instruction to everyone, and a durable decision written to a per-user store is invisible to the next reader who needs it.

**The five stores, and the routing rule.** The agent contract ships this table, adapted from the store model proven in the third prior-art instance (§14):

| Store | Loads | In git | Use for |
| --- | --- | --- | --- |
| `CLAUDE.md` | Always, in full | Yes | Facts needed in *every* session. Keep it short — it is paid for on every turn. |
| `.claude/rules/*.md` | Path-scoped via `paths:` frontmatter | Yes | Topic instructions that cost nothing until a matching file is opened |
| `.claude/skills/*/SKILL.md` | Name + description always; body on demand | Yes | Procedures |
| `memory/` (repo-oriented) | `MEMORY.md` index at start; topic files on recall | Yes | Durable facts about the vault, discovered in session |
| Harness default / `CLAUDE.local.md` (user-oriented) | Per that operator | No | Everything personal or machine-specific |

The rule: **prefer the cheapest store that reaches the right audience**, and when a contract section grows into a procedure, move it to a skill.

**Two mechanical facts that constrain the split.**

1. **Repo-oriented memory is not reachable by a dispatched subagent.** A worker or reviewer spawned into a fresh context sees the agent contract and path-scoped rules; it does not see recalled memory. So the derivation has a second axis beyond sharing posture: anything a *dispatched* agent must know belongs in the contract or a rule, not in memory — and since §5.7 wires the produced vault to the `spec-*` family (dispatch included) from its first commit, this applies to every vault, not to an exotic subset.
2. **The pointer to an in-vault memory directory cannot be committed.** It needs a machine-absolute path and lives in an ignored local settings file. The *content* is committed; only the pointer is local. The consequence is that a fresh clone is silently under-configured — memory keeps working, just in the wrong place — which is why the conditional `workspace-setup` skill (§5.1) is a shipped asset rather than a sentence in the contract. Three details it must carry, each learned the expensive way: the setting takes effect only after the workspace-trust prompt is accepted; changing it does **not** migrate memories already written elsewhere, so migration is offered and never done silently; and verification is confirming the agent contract actually appears as a loaded memory file, because if it did not load, nothing else in the setup is trustworthy.

**Calibration — what this deliberately does not become.** The prior-art instance this model comes from also carries a release surface, a manifest, versioned distribution to an unseeable consumer set, and a dual-environment constraint. None of that is in scope. The target of this archetype is closer to an AI-moderated notebook than to a distribution system: the vault ships **the routing table, the two-kind memory split, and the ignore lines that enforce it** — five stores and one rule, not a governance regime. What the skeleton *does* ship under `.claude/` is bounded and named: two path-scoped rules the design already places (§5.3's point-of-use hazard restatement, §5.7's `specs/**` block) and one skill, `workspace-setup`, which exists because the memory pointer cannot be committed. Nothing is created speculatively — no empty `.claude/rules/` directory, no skill without a procedure to carry — but the restraint claimed here is *bounded need*, not absence: the archetype does create `.claude/` subdirectories when a named asset lands in one. That is the resolution OQ-4 records.

**Why this design.** The audit (§5) reads the two reference instances as having made *opposite* memory choices, each right for its posture: the mature instance routes operator-specific facts outward by policy and keeps no in-vault `memory/`; the newer one routes the same category *inward*, explicitly "never in `~/.claude`." Both quotes are about **user-oriented** facts — and the newer instance can safely commit them only because it is single-operator, where the distinction collapses for want of a second reader. Neither instance names the repo-oriented category at all. Against a third instance that keeps both stores at once and states the boundary, the better reading is that a shared vault needs *both* answers simultaneously: the mature instance's outward routing for user-oriented facts, and an in-vault committed store for repo-oriented ones. Naming the two kinds is cheaper than deriving a location, and it removes a switch from the interview instead of adding one.

**Alternatives considered.** Keeping memory location as a single posture-derived toggle — rejected: it forces a shared vault to choose between committing personal facts and losing durable ones. Putting everything in the agent contract because that is the one store a subagent sees — rejected: it is the always-loaded store, and the archetype's whole token economy depends on it staying short.

### 5.2b Durability conventions

**Purpose.** Keep a vault's claims and its outward references from rotting quietly.

**Design.** Two lightweight conventions in the contract's Conventions section, both shipped as *rules for prose* rather than as tooling:

- **Provenance and staleness.** A document whose value depends on a state of the world outside the vault carries a `> Verified as of: YYYY-MM-DD` line at the top, and a claim that is inferred rather than confirmed says so at the point of the claim. This is the minimum that lets a cold reader tell a checked fact from a plausible one.
- **Citation durability** *(conditional — shipped when the interview says this vault cites sources outside itself)*. Cite an external source by an immutable, version-pinned reference, never by a local filesystem path or a branch tip. A local checkout is a convenience for reading; it is never the citation. §10 already rates "prior art becomes unreachable" as **certain** for this very spec — a vault that cites its own sources by mutable pointer inherits that risk permanently.

**Why this design.** Both are one-line conventions with no runtime cost, and each addresses a rot mode the validator cannot detect: a stale-but-well-formed document and a link that resolves today to different content than it did when cited. Neither becomes a validator check — see the anti-goal below.

**Anti-goals.** Do not add a staleness check to the validator: "verified 14 months ago" is not a violation, and a checker that says it is trains contributors to touch the date rather than re-verify the claim.

### 5.3 The hazard doc

**Purpose.** Name the vault's hazard class and the boundaries on agent action, before the vault exists.

**Shape — a three-point pattern, not one document.** The audit's correction to the finding: both reference instances place the hazard rule at three points, and a scaffold shipping only the canonical doc ships one third of what they converged on.

1. **Canonical doc** — `architecture/<hazard>-discipline.md`. Four parts: the one-to-three headline rules as absolutes; credential handling; a grading table (what class of content may live where); boundaries on agent *action*.
2. **Summary + pointer in the agent contract** — a short `CLAUDE.md` section restating the headline rules and linking the canonical doc.
3. **Point-of-use restatement** — the same rule repeated where a contributor is about to violate it, notably in `techniques/README.md`, whose cards routinely describe credentialed access, and in `memory/MEMORY.md`, which is committed and team-visible (§5.1). Where the point of use is a *path* rather than a document — an area whose files routinely brush the hazard — the restatement ships as a path-scoped rule under `.claude/rules/` instead of prose, so it loads exactly when a file in that area is opened and reaches a dispatched agent that would never recall it from memory (§5.2a).

**Behavior.** Parts 1's credential-handling block ships near-verbatim; the audit found four sub-rules phrased near-identically across both instances, plus a shared causal observation that leaks come from *exploration* rather than routine work, so the rule applies hardest when a route is new. The grading table and the boundaries-on-action section are authored per-hazard-class from a frame. **The skill refuses to produce a vault when no hazard class is named** — the finding records that in the manual session this organ emerged from a question the agent nearly did not ask, which is precisely why it is a gate rather than a prompt.

**Naming.** The bare token `discipline` is not used: the audit found that one instance's `architecture/discipline.md` is a *domain* doc about something else entirely while the other's `architecture/data-discipline.md` is the hazard doc. The name derives from the hazard class (`data-discipline`, `secrets-discipline`, `provenance-discipline`).

**Pattern invoked.** Defense in depth via restatement at point of use — the same reasoning behind `finding-intake`'s and `finding-triage`'s repeated "never silently swallow" rules appearing both in OPERATING PRINCIPLES and in WHAT NOT TO DO.

**Why this design.** The finding identifies this as the single highest-value document produced in the manual session and notes `project-constitution` has no slot for it. It stays with `vault-bootstrap` rather than being pushed upstream, because the hazard class is archetype- and domain-specific while `project-constitution` is archetype-neutral — see §12.

**Alternatives considered.** Amending `project-constitution` to own the hazard doc — rejected: it would couple an archetype-neutral skill to an archetype-specific organ, and the finding's binding constraint is to leave that skill's role intact. (If the design pass or a later review concludes otherwise, the audit flags it as a separable finding.) Shipping a single verbatim hazard doc — rejected: the four hazard classes have materially different grading tables and action boundaries.

### 5.4 The validator

**Purpose.** Enforce the conventions most likely to rot, mechanically.

**Shape.** An executable shipped inside the skill directory, run against a vault root, exiting non-zero with a per-violation report. Checks, all derived from observed rot rather than speculation (audit §9):

1. **Relative-link resolution** — every `[text](path)` target exists.
2. **Wikilink prohibition** — flag `[[…]]`, with two known false-positive classes handled: prose that *documents* the convention, and the repo-oriented `memory/` files, which are exempt by construction (§5.2a).
3. **Index coverage** — every `.md` in an area directory is linked from that directory's `README.md`.
4. **Area coverage** — every area directory appears as a row in `knowledge-map.md`, and every row resolves to an existing `README.md`.
5. **No absolute filesystem paths in committed prose.**

**Behavior.** Read-only; reports, never repairs. Runs as the last step before commit in the bootstrap sequence, and is left in the produced vault for re-use.

**Why this design, and why check 3 is the point.** The manual session's throwaway link walker found only false positives, which reads as "the conventions hold." The audit found otherwise: the *mature* reference instance — whose operator wrote the index convention — still carries an unindexed root-level document with spaces in its filename. Check 3 catches a real, live miss in the reference instance. Checks 3–5 are precisely the ones neither instance can currently enforce, and check 3 has already failed in production. That asymmetry is the argument for shipping a validator rather than a convention.

**Alternatives considered.** A git hook installed into the produced vault — deferred to OQ-1: hooks are not committed by git and so do not survive a clone, which contradicts the archetype's self-contained property. CI enforcement — rejected for the vault (a local-only, remote-less vault has no CI) and unavailable in this repo (no CI, per §3).

### 5.5 Archetype selection — and why the finding's leaning does not survive

**Purpose.** Decide how `vault-bootstrap` and future archetype siblings are discovered and routed to.

**Design.** **Sibling skills discovered by description, each self-contained, with no shared asset directory.** No dispatcher.

The finding's leaning was "siblings sharing a common `assets/` directory — no dispatcher to maintain, and skill descriptions already do the routing." The first half stands; **the second half is incompatible with this repo's [Atomic-Skill Portability Principle](../tech-stack.md)**, which requires each skill to self-contain its templates with *no runtime dependency on host-repo files*, and to work when installed standalone (e.g. into `~/.claude/skills/<name>/`) against an unrelated repo. A `_skeleton/` shared *between* skill directories is exactly such a dependency: install one sibling without the other, or without the parent repo, and its assets are gone. The principle's own originating finding ([20260517-intake-template-folder-dependency](../findings/20260517-intake-template-folder-dependency/finding.md)) is this same failure mode.

So sibling archetype skills **duplicate** whatever they share. Duplication is the accepted cost of atomicity, and the family already pays it: `finding-intake` and `finding-triage` each ship their own byte-for-byte-equivalent `_template/` copies for exactly this reason.

**Behavior.** Routing is by skill description. `vault-bootstrap`'s description names the archetype's observable properties — git-first, prose-only, no runtime code, agents as contributors, optionally Obsidian — so an agent can tell whether a given repo is in scope, and defers rather than forcing a fit when it is not.

**Why this design.** No dispatcher to maintain or keep in sync with the sibling set; descriptions already perform routing across this family; and atomicity is preserved for standalone adopters, who are the reason the principle exists.

**Alternatives considered.** A dispatcher skill taking an archetype argument — rejected: one more artifact to maintain, it centralizes what description-routing already does, and it becomes the "only door" the finding explicitly warns against. Shared `assets/` between siblings — rejected on portability, as above.

### 5.6 The `project-constitution` delegation seam

**Purpose.** Produce the constitution without absorbing or modifying the skill that owns it.

**Design.** `vault-bootstrap` invokes `project-constitution` after the skeleton exists, passing what it already gathered through that skill's existing INPUTS (`REPO_ROOT`, `REPO_PURPOSE`, `TARGET_AUDIENCE`, `LIFECYCLE_STAGE`, `STACK_HINTS`, `SCOPE_HINTS`) and relying on session context for the rest. **No change to `project-constitution`.**

This was examined and closed rather than deferred. `project-constitution`'s INPUTS block has no explicit parameter for "discovery is already gathered, do not re-interview," which suggested formalizing one via `spec-amend`. The operator's judgment, from repeated use, is that the skill is already adept at working from session context — which routinely points at prior art — so the absence is a **presumed gap, not a real one**. Formalizing an input against a gap that does not manifest would touch a deliberately-untouched skill for no observed benefit. The manual session is the supporting evidence: `project-constitution` consumed pre-gathered discovery cleanly when told not to re-interview, using nothing but session context.

**Behavior.** `vault-bootstrap` never writes to `specs/mission.md`, `specs/tech-stack.md`, `specs/roadmap.md`, or `specs/validation.md`. It creates `specs/constitution.journal.md` (structure, not intent — the amendment journal for files that carry no status banner of their own) and stops there. It does not re-run the constitution's lifecycle logic; `roadmap`-vs-`validation` selection stays that skill's decision, which the finding records as already correct.

**Why this design.** Preserving the boundary keeps `project-constitution` reusable by archetypes that are not knowledge vaults — the finding's binding constraint. If the seam does regress in practice, that is a new finding against a real symptom rather than a speculative amendment.

**Alternatives considered.** Adding `PREGATHERED_DISCOVERY` / `SKIP_INTERVIEW` to `project-constitution` — rejected per the operator's judgment above. Absorbing constitution authoring into `vault-bootstrap` — rejected: duplicates a working skill and breaks the finding's constraint.

### 5.7 Spec-family interoperation, and the grammar carried forward

**Purpose.** Let a produced vault work with the `spec-*` family from its first commit.

**Design.** The produced `CLAUDE.md` carries a short Spec workflow section — spec layout, the constitution's location, the amendment journal, the skill family, and the route-drift-via-`spec-amend` rule — and nothing more. The spec-session doctrine block (model floors, dispatch conventions, context budget) ships **as a path-scoped rule scoped to `specs/**`**, not as a contract section: it is needed only when a spec file is open, it must reach a dispatched worker, and it is the block most likely to go stale. It is shipped **as shape with tokens** — not as any consumer's specific text, per this repo's rule that content must not name specific consumers. The model-floor ladder is a token, since floors change: the audit found the reference instance carrying a stale `fable` tier it had to annotate around in prose, and the third instance (§14) had already moved the same block out of its always-loaded contract for these reasons.

**Grammar.** This repo declares a `## Grammar` block in [specs/tech-stack.md](../tech-stack.md); it is codified forward into this spec's [journal.md](journal.md) per the `spec-design` contract, so downstream skills consult the spec's copy rather than re-reading the constitution. It is a point-in-time snapshot fixed for this spec's life; a mid-flight dialect change routes through `spec-amend`.

The produced vault's own grammar is a separate question from this spec's: the skeleton's `specs/` layout ships the same anchor dialect as its default, which a vault may later override in its own constitution.

## 6. Non-functional Requirements

- **Adoptability.** A vault reaches its first commit in one session. Operator effort is dominated by the interview, which is two-tiered (§5.2): **4 required questions, up to 7 total**, plus up to two conditional follow-ups whose gates are derived from the posture answer rather than asked. The four required questions alone produce a valid vault; the depth round is recommended but skippable, and a skipped question is logged as a deferral rather than dropped. Every question carries a recommended default. This supersedes the earlier 3–5 target, which was contradicted by §5.2's own enumeration — resolved at CP-1 in favor of moving the target and declaring the floor, rather than merging questions the design argues are individually load-bearing.
- **Portability.** The skill functions installed standalone against an unrelated directory, with no dependency on this repo, on sibling skills, or on any host file. Host-provided overrides are used when present and absent-tolerated when not (§5.1).
- **Observability.** Every interview answer that becomes a structural decision is recorded in the produced vault — deferrals as logged deferrals, the store split stated explicitly in the agent contract, the hazard class named in its own doc. A cold reader can tell what was decided and why without the originating session.
- **Reversibility.** The skill's output is a set of files in an otherwise-untouched target directory, and the first commit is a single commit. Backing out is discarding that commit or deleting the produced files (§11).
- **Idempotence-adjacent safety.** Run against a non-empty directory, the skill reports what already exists and refuses to overwrite; it never silently replaces an existing `CLAUDE.md`, `README.md`, or `.gitignore`.
- **Security.** The hazard doc gates output. The skill never writes a credential, and the staging-directory pattern (gitignored dir with a committed README) exists so that the guidance is present at the moment someone is about to drop a sensitive file into it. The committed memory store is treated as a publication surface: its ignore lines and its own header carry the user-oriented/repo-oriented rule at the point of writing (§5.2a).
- **Configuration.** Two switches from the interview — Obsidian on/off, and whether the vault cites external sources (§5.2b) — plus the derived license and staging-directory gates. The memory split is *not* a switch; both stores exist in every vault. No configuration file.

## 7. Implementation Sequencing (Forward-Looking)

Phases, not atomic tasks. The downstream feature spec is named `specs/YYYYMMDD-vault-bootstrap-skill/feature.md` and owns the task breakdown.

### Phase 1 — Skeleton assets and token vocabulary
Materialize `_skeleton/` from the audit's confirmed-invariant set (§5.1); fix the token vocabulary and the conditional gates. Produces the asset tree the later phases substitute into. Consumes the audit, not the reference repos.

### Phase 2 — Interview contract and derivations
Author the two-round interview, the per-domain hazard candidates, and the derived-answer rules (license from remote posture; the store split from §5.2a, which is asserted rather than asked). Produces the `SKILL.md` prose contract for Phases P1–P2 of the skill's own runtime sequence.

### Phase 3 — Hazard-doc frame
The `_hazard/` frame, the near-verbatim credential-rules block, and the four per-hazard-class grading-table and action-boundary variants. Produces the authored-artifact generator and the output gate.

### Phase 4 — Validator
The five checks (§5.4), false-positive handling for checks 1–2, and the report format. Independently useful: it can be run against the two existing instances as its own first test, which is also how the design's central rot claim gets re-verified.

### Phase 5 — Delegation, closeout, and deploy-sync
Wire the `project-constitution` hand-off (§5.6), the validate-then-commit closeout, and the whole-directory deploy-sync of master → `~/.claude/skills/vault-bootstrap/`.

### Phase 6 — Dogfood against a third vault
Spin up a genuinely new vault, of a *different* hazard class than either reference instance, measuring operator effort and counting authored-vs-substituted files. This is the design's validation gate (§8), not a smoke test.

**Sequencing constraints.** P1 precedes P2 (the interview's tokens are the assets' tokens). P3 depends on P2 (hazard candidates come from the domain question). P4 is independent of P1–P3 and may run in parallel. P5 requires P1–P3. P6 requires all.

## 8. Validation Approach

- **Skeleton fidelity, both directions.** Materialize a skeleton and diff it against the audit's invariant table. Then run the validator against both reference instances: it must find the known unindexed root document in the mature instance (§5.4). A validator that reports both instances clean is a broken validator, and this is the check that says so.
- **Reconstruction test.** Materialize with tokens filled for the newer instance's domain and diff against the real vault. The gap is exactly the authored surface (hazard doc + constitution) plus genuine domain content. Any *structural* difference is either a skeleton bug or an invariant the audit missed.
- **Store-split test.** In a produced vault, give a fresh session one durable fact about the vault and one personal working preference, and ask it to remember both. The first must land in the committed `memory/` index; the second must not appear anywhere in `git status`. This is the behavioral check on §5.2a, and it is the one that fails silently in the field.
- **Cold-reader test.** Hand a produced vault to a fresh agent session with no access to the bootstrap conversation, and ask it to add a document to an area. It must place the file, add the index line, and leave `knowledge-map.md` alone. This validates the three load-bearing conventions as *behavioral* rather than documented.
- **Interview-effort measure.** In the Phase 6 dogfood, run the interview twice: once on the **required-only** path and once with the depth round, counting questions asked and operator time for each, against §6's `4 required / 7 total` NFR. The required-only run is also the test that the four-question floor genuinely produces a valid vault — if it does not, a depth question is misclassified and belongs in the required tier.
- **Hazard gate test.** Decline to name a hazard class and confirm the skill refuses to produce a vault.
- **Portability test.** Install the skill alone into `~/.claude/skills/`, run it against an unrelated directory with this repo absent, and confirm it completes (§5.5).

## 9. Review Checkpoints

### CP-1 — Design Approval
**Trigger.** This spec is complete and committed.
**Review focus.** Whether the archetype boundary is drawn correctly; whether the invariant/authored split matches the audit; whether §5.5's rejection of the finding's shared-assets leaning is sound; whether the five audit corrections are reflected rather than merely cited; and whether §5.2a's store split is calibrated to this archetype rather than carried over intact from a repo whose distribution requirements exceed it. The open questions are reviewed for whether they are the *right* deferrals, not for resolution.
**Exit criteria.** Operator approves the shape and the OQ set; spec advances to `Approved`; downstream feature spec is cleared to be authored.
**Status.** changes requested on 2026-08-17 by Claude Opus 5; remediated 2026-08-18; re-reviewed 2026-08-18 by Claude Opus 5 (inline `spec-review`, agent reviewer) — **changes requested**, 1 residual blocker: §2 and §4 still carry the superseded 3–5 question target that §6 replaced with `4 required / up to 7 total`. The three original blockers are otherwise closed. Checkpoint stays open. Operator approval of the shape and OQ set is still outstanding and is not something an agent reviewer can supply.

**Prior status.** changes requested on 2026-08-17 by Claude Opus 5 (inline `spec-review`, agent reviewer) — 3 blockers: the ungated `*(conditional on repo-oriented memory in-vault)*` gate (§5.1 vs §6); OQ-4's leaning contradicted by §5.1's `workspace-setup` asset; the §6 Adoptability 3–5 question target contradicted by §5.2/§5.2b. Checkpoint stays open. Operator approval of the shape and OQ set is still outstanding and is not something an agent reviewer can supply.

### CP-2 — Skeleton and Interview (post-Phase 3)
**Trigger.** `_skeleton/`, the interview contract, and the hazard frame exist.
**Review focus.** Token coverage with no unresolved substitution; that conditional gates actually omit rather than emit empty scaffolding; that the hazard gate blocks; that no shipped asset names a specific consumer.
**Exit criteria.** A materialized skeleton diffs clean against the audit's invariant table; the reconstruction test's residue is only the authored surface.

### CP-3 — Validator (post-Phase 4)
**Trigger.** The validator runs.
**Review focus.** That check 3 finds the known real miss in the mature reference instance; that checks 1–2's two false-positive classes are handled; that the validator reports and never repairs.
**Exit criteria.** Non-zero exit with an accurate report on the mature instance; clean exit on a freshly produced vault.

### CP-4 — Adoption (post-Phase 6)
**Trigger.** A third vault has been bootstrapped.
**Review focus.** Operator effort against the NFR; the authored-vs-substituted file count; whether anything had to be hand-fixed after the skill finished. Anything hand-fixed is either a missing invariant or a missed interview question.
**Exit criteria.** The third vault is coherent and committed without manual repair; deploy-sync verified across the whole skill directory.

## 10. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| **The design is wrong** — the archetype is really two archetypes, or the invariant set is over-fitted to two instances of one operator | Medium | High | CP-1 reviews the boundary before any implementation; the Phase 6 dogfood deliberately uses a different hazard class; the audit already documents where the two instances diverge rather than smoothing it over | operator |
| Skeleton over-fits the reference instances; a third vault needs substantial hand-repair | Medium | Medium | CP-4's hand-fix count is the explicit measure; conditional gates keep posture-dependent assets out of the invariant core | CP-4 |
| Deploy-sync drift — a shipped asset changes in the master but not the deploy copy | Medium | Medium | Whole-directory diff, not `SKILL.md` diff, in the deploy-sync check (§3); Phase 5 wires it | Phase 5 |
| Validator false positives train the operator to ignore it | Medium | Medium | The two known false-positive classes are handled as first-class requirements in Phase 4, not as follow-ups; the manual session's throwaway walker found *only* false positives, so this is the observed failure mode | Phase 4 |
| Sibling archetypes duplicate the skeleton and drift apart | High | Low | Accepted deliberately (§5.5); the family already pays this cost for `finding-*` templates; equivalence is asserted at the canonical state and drift is a finding, not a silent divergence | operator |
| The hazard gate is experienced as friction and gets bypassed | Low | High | It is a gate rather than a prompt precisely because the finding records the question was nearly not asked; per-domain candidates make answering cheap | Phase 3 |
| `project-constitution`'s informal seam regresses | Low | Medium | Closed as a presumed gap (§5.6); a regression is a real-symptom finding rather than a speculative amendment | operator |
| Prior art becomes unreachable mid-design | **Certain** | High | Already mitigated: the audit transcribes everything load-bearing, and this spec cites the audit rather than the repos | done |

## 11. Adoption Path

**A new vault** invokes the skill in an empty or near-empty directory and answers the interview. That is the whole adoption path; there is no migration step.

**An existing vault** is not an adoption target. The skill's value for one is diagnostic rather than generative: run the validator (§5.4) against it, and diff its files against a materialized skeleton to surface drift. Retrofitting an existing vault to the skeleton is out of scope (§12) and would be its own spec — the reference instance's own reshape took 7 phases.

**Reversibility.** The output is files in a target directory plus one commit. Backing out is discarding that commit, or deleting the produced files if the commit has not happened. Nothing is installed, no hook is registered, no external state is touched. The produced vault has no runtime dependency on the skill: it is plain markdown and git after the skill exits.

**Degradation mode (partial adoption).** A vault that takes the skeleton but skips the constitution has structure without intent — navigable but unable to answer "what is this for," and `spec-*` sessions lose their orientation read. A vault that takes the skeleton and skips the hazard doc is the one combination the skill refuses to produce; if it is assembled by hand anyway, the failure mode is silent, since nothing signals a missing hazard doc except the absence of a rule at the moment it is needed. A vault that takes the skeleton but never runs the validator degrades exactly as the reference instances did: slowly, at the index layer, and invisibly.

## 12. Out of Scope

- **Other archetypes.** Code-bearing, research, and service repos. Each is a sibling skill (§5.5).
- **Retrofitting existing vaults** to the skeleton (§11).
- **Changes to `project-constitution`** — deliberately, per §5.6 and the finding's binding constraint.
- **Pushing the hazard doc upstream** into `project-constitution`. Deferred with a named revisit path: the audit flags it as a separable finding if the design or a later review concludes the organ is archetype-neutral after all.
- **Remote provisioning, hosting, and access control.** Operator's.
- **Obsidian plugin curation** beyond shipping an empty community-plugin set with the tracked-vs-ignored doc.
- **Domain content.** The skill produces structure and one hazard doc; it does not write the vault's knowledge.
- **Migrating the two reference instances** onto whatever the skill ships. They are prior art; reconciling them is optional future work with no dependency in either direction.

## 13. Open Questions

### OQ-1 — Where does the validator live, and what invokes it?

**Question.** The validator ships inside the skill directory (§5.4), but what runs it — and does a copy also land in the produced vault, where it is needed long after bootstrap?

**Analysis.**

| Option | Survives clone | Runs unprompted | Cost |
| --- | --- | --- | --- |
| Skill-only, run at bootstrap | n/a | no | Vault has no ongoing enforcement — the rot the design exists to stop happens *after* bootstrap |
| Copy into the vault, run on demand | yes | no | Requires someone to remember; but "someone" is an agent reading `CLAUDE.md`, which is more reliable than a human remembering |
| Copy into the vault + git `pre-commit` hook | **no** — hooks are not tracked by git | yes, locally | Contradicts the archetype's self-contained property; a fresh clone silently loses enforcement |
| Copy into the vault + session-close instruction in `CLAUDE.md` | yes | yes, for agent sessions | Only covers agent sessions, not human-only edits — but agents are the archetype's primary contributors |

The rot evidence cuts specifically here: the mature instance's unindexed document was added by a human-authored edit, not by an agent session, so agent-session enforcement alone would not have caught it.

**Leaning.** Copy the validator into the produced vault *and* reference it from the agent contract's session-discipline section, treating the run as part of "a session leaves the vault in a coherent state." Skip the git hook — untracked hooks contradict self-containment. Accept that human-only edits are covered only at the next agent session, which is a real gap and better than none.

**Owner.** Phase 4, ratified at CP-3.

**Watch items.** If the archetype ever grows a CI-bearing instance, revisit — CI would close the human-edit gap the leaning leaves open.

**Anti-goals.** Do not make the validator auto-repair: an index line it writes is a gloss nobody chose, and the gloss is the valuable half. Do not gate the bootstrap commit on a clean validator run in a *non-empty* target directory — pre-existing violations are not this skill's to fix.

### OQ-2 — Does the scaffold seed a first `history/` entry?

**Question.** Ship `history/` empty with its inclusion rule, or seed a stub capturing context the operator gave verbally during the interview?

**Analysis.** The two instances answered differently, and one of the answers was *governed*: the mature instance's reshape spec resolved its own OQ-6 to ship the directory empty and populate on first natural addition. The newer instance instead seeded a "known episodes, not yet written up" stub — and the finding records that the stub captured real context the operator had only said aloud, which would otherwise have been lost. Both outcomes are real: the empty-directory rule keeps the vault honest about what has actually been written up, and the stub catches ephemeral context at the one moment it is available.

These are reconcilable, because they answer different questions. The inclusion rule governs what a *finished* history document is. The stub is a capture buffer for context that has no other home yet — closer in kind to a finding at `status: intake` than to a history document.

**Leaning.** Ship `history/` with its README and inclusion rule always; seed a stub **only when the interview actually surfaced un-written-up context**, and mark it explicitly as a capture buffer rather than a history entry, so it cannot be mistaken for a finished document. Empty by default, seeded by evidence.

**Owner.** Phase 2 (the interview decides), ratified at CP-2.

**Anti-goals.** Do not seed a placeholder stub with no real content — an empty stub is worse than an empty directory, because it looks like the convention is in use when it is not.

### OQ-3 — How much of the agent contract is token-substituted versus authored?

**Question.** §5.1 ships `CLAUDE.md` as a token-bearing asset, and the finding's leaning was that only the hazard doc and the constitution are genuinely authored. Does the agent contract's Orientation section survive that split?

**Analysis.** Most of `CLAUDE.md` is confirmed near-verbatim across both instances (audit §2.1) — the knowledge-capture block, the index rule, the conventions list, the spec-workflow section. But Orientation is 1–2 paragraphs of genuinely domain-specific prose, in both instances, and it is the first thing every session reads. Token substitution into a fixed sentence frame would produce something serviceable and slightly wrong: the mature instance's orientation carries a "one-ring hub, code lives in a spoke" topology claim that no frame would have predicted, and the newer one carries a three-system migration narrative.

The honest split may therefore be three categories rather than two — substituted, framed-and-authored, fully authored — which would amend §4's composition rule 4.

**Leaning.** Treat Orientation as framed-and-authored: ship a frame with its required elements enumerated (what this is, what the boundary is, what the founding project is) and author the prose from interview answers, rather than substituting tokens into a fixed sentence. Keep every other `CLAUDE.md` section substituted. If this holds, composition rule 4 gains a third category at CP-2.

**Owner.** Phase 1/Phase 2 boundary, ratified at CP-2.

**Watch items.** If the Phase 6 dogfood's Orientation needs hand-repair, the frame is too loose and should enumerate more required elements.

### OQ-4 — Does `vault-bootstrap` create `.claude/skills/` in the produced vault? — **Resolved during design**

**Question.** The mature reference instance hosts two vault-local skills under a tracked `.claude/skills/`. Should the skeleton create that directory?

**Analysis.** The question was posed at `3bab5bd`, when the answer was "vault-local skills arrive with a domain need, not at bootstrap" — an empty `.claude/skills/` being an empty directory git will not track anyway. The third-instance pass then introduced a *named* asset that lands there: `workspace-setup` (§5.1, §5.2a), which exists because the in-vault memory pointer needs a machine-absolute path and therefore cannot be committed. That is a bootstrap-time need, not a domain need, and it is unconditional — so the premise the deferral rested on no longer holds.

**Resolution.** The skeleton does **not** create an empty `.claude/skills/`; it creates exactly the directories its named assets occupy, which as of this design is `.claude/skills/workspace-setup/` and the two path-scoped rules under `.claude/rules/`. Structure still follows need — the need is now present and named. Also shipped, unchanged from the original leaning: the `.gitignore` discipline (already in the core set — track skills and shared settings, ignore per-operator and machine state) and one sentence in the agent contract's conventions noting that vault-local skills belong under `.claude/skills/` and are tracked.

**Why resolved rather than re-deferred.** A deferral whose leaning the detailed design already contradicts is drift, not a deferral. §5.1 and §5.2a answer this question in the body of the spec; leaving it open would mean the spec deferred a question it had already decided.

**Owner.** Phase 1 implements; CP-2 confirms the created set matches the named-asset set exactly (no speculative directories).

### OQ-5 — Is `specs/docs/` part of the skeleton?

**Question.** The mature instance parks non-spec doctrine documents — house rules, an internal doctrine document, a format ground-truth reference — under `specs/docs/`. Is that an invariant or an instance artifact?

**Analysis.** Present in one instance, absent in the other, which by the audit's standard makes it not invariant. But its existence answers a real question the skeleton otherwise leaves open: where does a document go that is authoritative doctrine but is neither a spec nor area knowledge? Without a named home, such documents land in whichever area directory is least wrong, and the areas start to bleed — the exact failure the cross-area routing sentences (§5.1) exist to prevent. The newer instance may simply not have hit the case yet, being one day old.

**Leaning.** Do not ship the directory; **do** ship the routing rule — one line in the `specs/` orientation naming where cross-cutting doctrine goes when it appears. That gives the question an answer without creating an empty directory, consistent with OQ-4's reasoning.

**Owner.** Phase 1, ratified at CP-2.

**Watch items.** If the Phase 6 dogfood produces a doctrine document in its first session, promote to a shipped directory.

### OQ-6 — What does the skill do when the target directory is already a populated vault?

**Question.** §6 requires refusing to overwrite. Is refusal the whole behavior, or does the skill offer a diagnostic mode?

**Analysis.** §11 already establishes that the skill's value for an existing vault is diagnostic — run the validator, diff against a materialized skeleton. That is a genuinely useful mode and it is nearly free, since both halves exist for other reasons. The risk is scope: a diagnostic mode invites "and now fix it," which is the retrofit that §12 puts out of scope and which the reference instance needed 7 phases to do. There is also a real ambiguity in the middle — a directory with a `README.md` and nothing else is neither empty nor a vault, and the skill must not refuse to help there.

**Leaning.** Three states, explicitly distinguished: **empty-or-near-empty** → normal bootstrap, reporting what it found and preserving it; **populated non-vault** → bootstrap the missing skeleton, never overwriting an existing file, listing every file it declined to touch; **already a vault** (has an agent contract *and* a knowledge map) → refuse to bootstrap, offer the diagnostic report, and stop there. Report-only in all three; no repair.

**Owner.** Phase 5, ratified at CP-4.

**Anti-goals.** Do not add a `--force` overwrite. The skill's blast radius is someone's knowledge repository, and the safe failure is doing nothing.

## 14. References

### Authoritative

- [docs/knowledge-vault-archetype-audit.md](../../docs/knowledge-vault-archetype-audit.md) — the prior-art audit of the two reference instances, transcribed 2026-08-17, plus **Appendix A**, the third instance's store model transcribed 2026-08-17. **This spec's authoritative source for every prior-art claim**, because the instances themselves become unreachable (§3).
- [specs/findings/20260817-knowledge-vault-bootstrap-gap/](../findings/20260817-knowledge-vault-bootstrap-gap/finding.md) — the originating finding, with the operator's own distillation preserved as `source-signal.md`.
- [specs/tech-stack.md](../tech-stack.md) — the Atomic-Skill Portability Principle (binding on §5.5) and the declared `## Grammar` block (codified forward per §5.7).
- [CLAUDE.md](../../CLAUDE.md) — the deploy-sync rule, skill-frontmatter uniformity, and the no-named-consumers rule.
- [.agents/skills/project-constitution/SKILL.md](../../.agents/skills/project-constitution/SKILL.md) — the delegation target's INPUTS and phase structure (§5.6).
- [specs/findings/20260517-intake-template-folder-dependency/](../findings/20260517-intake-template-folder-dependency/finding.md) — the portability principle's originating finding; the same failure mode §5.5 rejects.

### Inspirational

- `admindoc`, `specs/2026-05-25-admindoc-reshape/architecture.md` — a governed design spec for reshaping a repo into this archetype; its section set informed §7 and its OQ-1/OQ-5/OQ-6 are the recurrence evidence in §3. Not reachable outside the operator's personal machine; content transcribed in the audit.
- A third prior-art instance — an internal capability repository, commits `51a5076..c177707` — source of §5.2a's five-store model, the two mechanical constraints (memory not reaching dispatched agents; the uncommittable memory pointer), the `workspace-setup` organ, and §5.2b's two durability conventions. It is **not** an instance of this archetype: it is a specialized multi-modal knowledge repository with its own distribution management, whose functional requirements exceed a knowledge vault's, so its release, manifest, versioning, and dual-environment apparatus is deliberately not carried forward (§5.2a *Calibration*). Not reachable outside the operator's environment; the load-bearing content is transcribed in the audit's Appendix A.
- The `finding-intake` / `finding-triage` `_template/` pair — the template-with-host-override idiom reused in §5.1, and the deliberate-duplication-for-atomicity precedent cited in §5.5.
