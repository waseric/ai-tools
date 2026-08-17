# Knowledge-vault bootstrap has no skill — Journal

## 2026-08-17 — Intake: no skill covers knowledge-vault repo bootstrap; the manual derivation is the reusable asset

**Captured by:** waseric; persona-frame: intake
**Signal source:** text + system pointer (operator-supplied signal document in a session scratchpad)
**New status:** `intake`
**Notes:** The signal arrived as a self-contained narrative document written from the `Finances` vault spin-up session and explicitly addressed to this repo as intake for a future design spec. Because the original path was an ephemeral session scratchpad (`/private/tmp/claude-501/.../scratchpad/20260817-knowledge-vault-bootstrap-finding.md`), the document was copied verbatim into this finding directory as `source-signal.md` rather than referenced in place — no live fetch was attempted or needed, since the operator supplied the full content inline. Domain recorded as `methodology` at intake (a gap in the skill family's coverage, not a defect in a running system); severity left for triage. The signal names two prior-art instances (`sandlotminecraft/admindoc` and the operator's `Finances` vault) that a downstream design pass would need to read; neither was opened during intake. The signal document also carries its own suggested opening prompt for the future design session and five open questions the operator wants resolved during design — both preserved in the snapshot.

## 2026-08-17 — Triaged: coverage gap, not a defect; important; scoped to the next knowledge-vault spin-up

**Triaged by:** waseric; methodologist; persona-frame: triage
**Prior status:** `intake`
**New status:** `triaged`
**Reproducibility outcome:** not applicable
**Domain/severity changes from intake:** none — intake's `methodology` domain confirmed; severity set for the first time at triage (`important`). No operational urgency: not an operational finding.
**Skip-investigation decision (if any):** skip chosen — route directly to `spec-write` (via a `spec-design` pass first). The signal document is already a self-contained design brief: it names the archetype, enumerates the invariant skeleton, records the interview that worked, freezes five recurring frictions, and states its own five open questions. Investigation's product — a cause hypothesis with file:line touchpoints and a proposed remedy — has no purchase on a coverage gap: there is no implementation to diagnose, only a design to author. Everything an investigation pass could add is a design decision the operator has explicitly reserved for `spec-design`.
**Pointer revalidation:** `treated-as-static` — the single external reference is `source-signal.md`, a verbatim snapshot committed inside this finding directory; it cannot drift. The two prior-art repos named in the signal (`sandlotminecraft/admindoc`, the operator's `Finances` vault) were deliberately not opened: reading them is the design pass's first task, and triage stays out of code.
**Notes:** Triage opened no source files in this repo. Severity reasoning and the deferred design-shaped hypotheses are recorded in the finding's Triage notes. One flag for the design pass: the hazard/discipline-doc claim may be separable into its own finding against `project-constitution` if the design decides that organ belongs upstream rather than in the new scaffold.

## 2026-08-17 — Routed: new design spec for a knowledge-vault scaffold skill, authored via spec-design

**Decided by:** waseric; methodologist; persona-frame: triage, with operator
**Prior status:** `triaged`
**New status:** `routed`
**Route subtype:** spec-write
**Target spec (if amend or new-spec):** new spec — `specs/<YYYYMMDD>-knowledge-vault-scaffold/architecture.md`, authored by `spec-design`; path indicative, named by the design pass
**Watch condition (if defer):** not applicable
**Rationale (see also the 2026-08-17 route-consumed entry below, which resolves the indicative target path):** As recorded in the finding's Route rationale: no existing spec owns repo-archetype bootstrap, and the nearest neighbour (`project-constitution`'s governing spec) is out of bounds by the finding's own binding constraint — the new skill must delegate to it, not absorb it. Deferring would leave the reusable asset living only in this finding's snapshot while the next vault spin-up re-pays the full manual cost. The operator's stated entry point is `spec-design`, not implementation; the design pass inherits the signal's five open questions and the anti-monopoly constraint that this archetype must not become the only door for repo bootstrap.

## 2026-08-17 — Route consumed: design spec authored as `vault-bootstrap`; prior art audited before it became unreachable

**Authored by:** waseric
**Prior status:** `routed`
**New status:** `routed` (unchanged — this entry records route consumption, not a transition)
**Target spec resolved:** [specs/20260817-vault-bootstrap/architecture.md](../../20260817-vault-bootstrap/architecture.md), `Draft — Open for Review`, awaiting CP-1. The finding's Route section carried an indicative path; it is updated in place to the actual path now that the design pass named the artifact.
**Urgency driver:** The operator will continue this work from a context with **no access to `sandlotminecraft/admindoc`**, one of the two reference instances. The audit of both instances was therefore pulled forward to this session and committed as [docs/knowledge-vault-archetype-audit.md](../../../docs/knowledge-vault-archetype-audit.md), which transcribes everything load-bearing rather than pointing at repos that will be unreachable. The design spec cites the audit, not the repos.
**Corrections to this finding's own signal:** the audit contradicts `source-signal.md` in five places — most consequentially, in-vault `memory/` is **not** an invariant (the mature instance has none and routes per-operator facts outside the repo by policy, the exact inverse), and the "two instances converged independently" claim is overstated, since the newer instance names the older as prior art in its own agent contract. Per the append-only convention, `source-signal.md` and the Intake section are left unedited; the audit's §7 carries the corrections and the design spec is built on the corrected account.
**Notes:** Triage's flag — that the hazard/discipline-doc claim may be separable into its own finding against `project-constitution` — is carried into the design spec as an explicit out-of-scope item with a named revisit path (§12), not silently dropped. The design also closed one question triage did not know existed: `project-constitution` has no INPUT for pre-gathered discovery, which the operator ruled a presumed gap rather than a real one, keeping that skill untouched.
