# Knowledge-vault bootstrap has no skill — Finding

> Status: routed
> Domain: methodology
> Severity: important                                  ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
> Date opened: 2026-08-17
> Last transition: 2026-08-17

## Intake

**Reported by:** self
**Reported via:** text + system pointer
**Captured by:** waseric; persona-frame: intake
**Summary:** Spinning up the `Finances` Obsidian vault took one long manual session producing ~18 files, of which only three (`specs/{mission,tech-stack,roadmap}.md`) came from a skill (`project-constitution`). The other fifteen were hand-derived from prior art (`sandlotminecraft/admindoc`) plus an operator interview — and that derivation, which is the reusable asset, exists nowhere. The skill family therefore has no entry point for the **knowledge vault** archetype: a git-first, Obsidian-first, prose-only repository with no runtime code and AI agents as first-class contributors. Two independent instances (`admindoc`, `Finances`) converged on the same skeleton, which is the evidence the archetype is real rather than domain-specific. The signal document (snapshotted alongside this finding) distills: the invariant skeleton a scaffold could ship verbatim (`CLAUDE.md`, `knowledge-map.md`, `README.md`, area dirs each with a `README.md` index, `memory/`, a hard-won `.gitignore`, `specs/` layout, git posture); three load-bearing conventions (per-folder index lines, markdown-links-only, `knowledge-map.md` changes only per *area*); the under-recognized **discipline/hazard doc** for which `project-constitution` has no slot; the five-question two-round interview that worked (scan first, ask second; offer archetypes, not blanks); five frictions worth freezing (`.gitignore` staging a 3 MB plugin bundle, README-inside-ignored-staging-dir, hand-rolled link/index validator, iCloud+git hazard paragraph, memory relocation being a decision rather than a default); and the handoff shape `scaffold → short interview → project-constitution → validate → commit`, with `project-constitution` delegated to rather than absorbed. Known-unknown: five open questions the operator wants resolved in design (static template vs. generated prose; archetype selection via dispatcher vs. sibling skills; whether the scaffold assumes Obsidian; where the link/index validator lives and when it runs; whether the scaffold seeds a first `history/` entry). The operator's stated intent is to start with `spec-design`, not implementation, and the constraint that this must not become the only door — other archetypes will follow.
**External references:** [source-signal.md](source-signal.md) — verbatim snapshot of the originating signal document (`20260817-knowledge-vault-bootstrap-finding.md`, written 2026-08-17 from the Finances vault spin-up session). Copied into this finding directory because the original lived in an ephemeral session scratchpad. Prior art named by the signal, not fetched: `sandlotminecraft/admindoc`; the operator's `Finances` Obsidian vault.

## Triage

**Triaged by:** waseric; methodologist; persona-frame: triage
**Triage date:** 2026-08-17
**Reproducibility:** not applicable
**Repro steps (if reproducible):**
not applicable
**Scope:** The skill family's repo-bootstrap coverage. Affects every future spin-up of a knowledge-vault-shaped repository — git-first, Obsidian-first, prose-only, no runtime code, agents as first-class contributors. Two existing instances (`sandlotminecraft/admindoc`, the operator's `Finances` vault) are prior art rather than affected parties; they are already built. The cost lands entirely on instance N+1, which today re-pays a multi-hour manual derivation. Secondarily affects `project-constitution`, which is in scope as a *delegation target* only — the finding's constraint is that it stay untouched. Not scoped: other repo archetypes (code-bearing, research, service), which the signal explicitly reserves for future siblings.
**Domain confirmation:** methodology
**Severity confirmation:** important
**Triage notes:** Not a defect — a coverage gap in the skill family, so reproducibility is `not applicable` rather than `unknown`: there is no failing behavior to trigger. The observable evidence is a count, not an error: ~18 files produced in one manual session, 3 of them skill-generated. Severity `important` rather than `blocker` because the gap has a working (expensive) manual workaround, and rather than `advisory` because the signal documents two independent convergent instances plus five frictions that each cost real time and would recur verbatim. Design-shaped hypotheses carried in the signal — static skeleton + token substitution over generated prose; sibling skills over a dispatcher; a committed link/index-coverage validator; requiring a hazard-class answer before producing a vault — are recorded as **deferred to design**, not as triage claims; the signal states them as leanings and the operator wants them resolved in the design pass, not before it. No source files in this repo were opened during triage. One observation worth carrying forward: the finding's highest-value claim (every knowledge vault has a hazard class, and `project-constitution` has no slot for it) may itself be a second, separable finding against `project-constitution` if the design pass decides the hazard doc belongs upstream rather than in the new scaffold.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** spec-write
**Decided by:** waseric; methodologist; persona-frame: triage, with operator
**Route date:** 2026-08-17
**Target spec:** new spec — `specs/<YYYYMMDD>-knowledge-vault-scaffold/architecture.md`, to be authored by `spec-design` (design pass first; `spec-write` follows only once the design's open questions are resolved). Path is indicative; the design pass names it.
**Route rationale:** New spec rather than amendment: no existing spec owns repo-archetype bootstrap. `project-constitution`'s governing spec is the nearest neighbour and is explicitly out of bounds — the finding's binding constraint is that the new skill *delegate to* `project-constitution` rather than absorb it, so amending that spec would break the boundary the finding exists to preserve. Not `defer`: the cost is fully re-paid by the next vault spin-up, and the reusable asset (the invariant skeleton, the interview shape, the five frozen frictions) currently exists only in the snapshot attached to this finding, where it decays. Not `close`: nothing in the skill family covers this. Investigation skipped — see journal; the signal is already a self-contained design brief, and the remaining unknowns are design decisions, not diagnosis. The design pass owns the signal's five open questions (static template vs. generated prose; dispatcher vs. sibling skills; whether the scaffold assumes Obsidian; where the link/index validator lives and when it runs; whether the scaffold seeds a first `history/` entry), plus the anti-monopoly constraint that this archetype must not become the only door.
