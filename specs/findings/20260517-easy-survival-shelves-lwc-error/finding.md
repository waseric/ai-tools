# LWC "Missing API" error on shelves in Easy Survival — Finding

> Status: intake
> Domain: operational
> Severity: <blocker | important | advisory>           ← methodology axis
> Operational urgency (optional): <P1 | P2 | P3 | P4>  ← operational axis (typically operational findings only)
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

**Triaged by:** <persona-frame: service desk | business analyst | developer>
**Triage date:** <YYYY-MM-DD>
**Reproducibility:** <reliably | intermittently | not reproduced | not applicable>
**Repro steps (if reproducible):**
1. ...
**Scope:** <who/what is affected>
**Domain confirmation:** <operational | testing | security | methodology | other>
**Severity confirmation:** <blocker | important | advisory>
**Triage notes:** <anything else surfaced in triage; rejected hypotheses; clarifications from reporter>

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
