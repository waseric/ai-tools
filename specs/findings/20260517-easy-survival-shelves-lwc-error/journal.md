# LWC "Missing API" error on shelves in Easy Survival — Journal

## 2026-05-17 — Intake: LWC plugin "Missing API" error on shelf interaction in Easy Survival; absent on Normal

**Captured by:** waseric; persona-frame: intake
**Signal source:** text + URL (forum thread URL plus operator-supplied PDF snapshot of the thread page)
**New status:** `intake`
**Notes:**
- Source thread is on the Sandlot Minecraft forum, auth-walled: https://www.sandlotminecraft.com/threads/error-when-placing-items-in-shelves.39846/. Operator pre-fetched the page as a PDF and supplied it at session intake, so the load-bearing content (the error text, the cross-world test "Tested Normal and it was fine," and the second reporter's confirmation) is captured verbatim in `finding.md`'s `External references` field rather than relying on the live URL remaining reachable for unauthenticated parties.
- No live URL re-fetch was attempted by the agent. Rationale per SKILL.md OP #3 (never silently swallow a fetch outcome — choosing not to attempt is still an outcome to surface): the forum is auth-walled and would have returned a login redirect, and the operator-supplied PDF already provides the durable snapshot the policy is meant to secure. This deviation from "attempt a fetch if capable" is itself the right call when the snapshot already exists; recorded here so reviewers can revisit at RC-3a if they disagree.
- The two-reporter pattern (GargoyleAnt initial + NeonLights10927 confirmation) is recorded in the `Reported by` field per the T-05 inline-list convention. The single-field schema constraint on `Reported by` was already surfaced as a friction observation in the T-05 finding's journal; not re-raising here.
- This finding is the T-03 real-signal dogfood for the finding-intake skill (spec: [specs/20260517-finding-intake-skill/feature.md](../../20260517-finding-intake-skill/feature.md)). It joins the T-05 `tab-display-issues` finding as the second operational-domain living example in `specs/findings/`, alongside the T-02 synthetic `test-only-signal-synthetic-fixture` retained as a methodology-domain regression reference.
