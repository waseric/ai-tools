# LWC "Missing API" error on shelves in Easy Survival — Finding

> Status: triaged
> Domain: operational
> Severity: advisory
> Operational urgency (optional): P4
> Date opened: 2026-05-17
> Last transition: 2026-05-17

## Intake

**Reported by:** GargoyleAnt (initial reporter, 2026-05-10); confirmation from NeonLights10927 (replied in-thread 2026-05-16) — both Sandlot community members posting in the "Easy Survival - Error when placing items in shelves" Bug Reports thread.
**Reported via:** Forum thread "Easy Survival - Error when placing items in shelves" in Sandlot Bug Reports — https://www.sandlotminecraft.com/threads/error-when-placing-items-in-shelves.39846/ — started 2026-05-10; PDF snapshot supplied by operator at session intake (forum is auth-walled, no live re-fetch).
**Captured by:** waseric (Sandlot Administrator and operator); persona-frame: intake
**Summary:** Two players report that the LWC protection plugin throws an internal error on the Easy Survival Sandlot Minecraft server when interacting with shelf items. GargoyleAnt (2026-05-10) encountered the error while placing and removing items in a shelf; NeonLights10927 reproduced it by right-clicking a shelf. Error text verbatim: `[LWC] Internal error. Notify an admin immediately. :Missing API.` GargoyleAnt confirms the same interaction on the Normal world (the sibling Sandlot survival server) does not trigger the error — the failure is Easy-Survival-specific. The `:Missing API` suffix typically indicates LWC is looking for an API surface another plugin should expose (likely the shelf-providing furniture/decoration plugin) that is absent or version-mismatched on Easy Survival but present on Normal. Root cause, remedy, and whether the issue is server-config drift versus a plugin update gap are unknown at intake; classification belongs to triage.
**External references:**
- Forum thread: https://www.sandlotminecraft.com/threads/error-when-placing-items-in-shelves.39846/
- <!-- fetched 2026-05-17 (via operator-supplied PDF snapshot of auth-walled forum thread) -->
  Quoted from thread (verbatim):
  > "Was placing and removing items in a shelf in Easy and received the following error: [LWC] Internal error. Notify an admin immediately. :Missing API. Tested Normal and it was fine. Thanks." — GargoyleAnt, 2026-05-10
  > "I right clicked a shelf and got this error as well" — NeonLights10927, 2026-05-16 (quoting the same error text from GargoyleAnt's post)
  > Thread tags: error, missing api

## Triage

**Triaged by:** waseric; Sandlot administrator; persona-frame: triage
**Triage date:** 2026-05-17
**Reproducibility:** reliably
**Repro steps (if reproducible):**
1. Join the Easy Survival Sandlot Minecraft server.
2. Right-click a shelf, *or* attempt to place or remove an item in a shelf.
3. Observe the verbatim error returned to the player: `[LWC] Internal error. Notify an admin immediately. :Missing API.`
4. (Control) Repeat the same interaction on the Normal Survival sibling server; no error occurs.

**Scope:** Easy Survival server only; all observed shelf interactions across two distinct interaction modes (place/remove and right-click); ≥2 distinct players affected per in-thread reports (GargoyleAnt 2026-05-10, NeonLights10927 2026-05-16); Normal Survival is unaffected per in-thread cross-world control test.
**Domain confirmation:** operational
**Severity confirmation:** advisory
**Triage notes:**
- The cross-world control test (Easy fails, Normal succeeds) is the most useful single fact carried forward to investigation: it isolates the failure to Easy Survival's plugin/configuration state, not to LWC or shelves in general.
- The intake Summary's hypothesis ("the `:Missing API` suffix typically indicates LWC is looking for an API surface another plugin should expose, likely the shelf-providing furniture/decoration plugin, that is absent or version-mismatched on Easy Survival but present on Normal") is *deferred to investigation* — recorded here as deferred, not confirmed at triage.
- No live re-fetch of the forum thread was attempted at triage; the operator-supplied PDF snapshot captured at intake is the durable evidence and the intake Summary is judged rich. Pointer revalidation: `treated-as-static` (journaled).
- No further reporters surfaced between intake (2026-05-17) and triage (same day). Reporter list is stable for now.

## Investigation (optional)

**Investigated by:** <persona-frame: developer>
**Investigation date:** <YYYY-MM-DD>
**Probable cause:** <hypothesis with evidence; file:line references where applicable>
**Code/configuration touchpoints:** <bulleted file paths>
**Alternative hypotheses considered:** <briefly, with reason rejected>
**Proposed remedy:** <plain-language description>

## Route

**Route decision:** <spec-amend | spec-write | defer | close>
**Decided by:** <persona-frame of the deciding phase, and operator>
**Route date:** <YYYY-MM-DD>
**Target spec:** <path to spec, when route is `spec-amend` or `spec-write`; e.g., specs/20260517-findings-pipeline/architecture.md>
**Route rationale:** <one paragraph; why this route over the others. For `defer`, include watch condition: what would cause re-evaluation.>
