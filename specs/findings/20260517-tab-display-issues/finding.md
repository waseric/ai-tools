# TAB list display issues across Sandlot servers — Finding

> Status: under-investigation
> Domain: operational
> Severity: advisory
> Operational urgency (optional): P3
> Date opened: 2026-05-17
> Last transition: 2026-05-17

## Intake

**Reported by:** NeonLights10927 (initial reporter); confirmations and additional symptoms from trubuhl, EFret17, SupersonicE9, WanderingLlama, MangoBreeze (Sandlot community members posting in the same thread)
**Reported via:** Forum thread "All Servers - Tab Screen" in Sandlot Bug Reports — https://www.sandlotminecraft.com/threads/tab-screen.39849/ — started Tuesday 2026-05-12
**Captured by:** waseric (Sandlot Administrator and operator); persona-frame: intake
**Summary:** Multiple TAB player-list display issues surfaced over the week of 2026-05-11 through 2026-05-15 across Sandlot Minecraft servers. Symptoms reported on both Java and Bedrock clients in multiple game-mode contexts (lobby, parkour, creative, skywars). The issues cluster around four coupled visual dimensions: (1) rank icons not displayed, notably the second/Senior Helper pink star; (2) Moderator name colors — default lime-green not rendering while custom colors do; (3) AFK tag visibility intermittently broken; (4) Bedrock TAB scoping showing all server players rather than current-game-mode players. Intermediate fixes were applied in-band via the forum thread (admin announced "should be working now" on 2026-05-13), each followed by reports of new or returning symptoms. As of the most recent post (Friday 2026-05-15), TAB appears mostly normal with lingering AFK-tag glitches and one report of all ranks missing for `@sprainedd` when viewed from Parkour.
**External references:**
- Forum thread: https://www.sandlotminecraft.com/threads/tab-screen.39849/
- Screenshot evidence in-thread (post #10 by trubuhl, Wed 2026-05-13 6:39 PM): annotated lobby TAB view confirming missing Senior Helper second star on Sprainedd and TwistyYarn

## Triage

**Triaged by:** waseric; persona-frame: business analyst (with admin/operator overlap — solo-operator persona collapse per design spec §5.6)
**Date:** 2026-05-17 (artifact-side triage; in-band admin response in the forum thread first occurred 2026-05-13 Wed 7:50 AM)
**Reproducibility:** reliably — multi-user, cross-client, cross-game-mode, persisted across multiple posts and dates in the symptomatic windows
**Repro steps (if reproducible):**
1. Join any Sandlot Minecraft server on either a Java or Bedrock client.
2. Press TAB to open the player list.
3. Observe rank icons next to player names — expected: full rank star set including Senior Helper second pink star where applicable.
4. Observe Moderator name colors — expected: default lime-green renders for default-color moderator accounts.
5. Observe AFK tag for any AFK player — expected: AFK indicator visible.
6. On Bedrock: observe whether the list is filtered to the current game mode or shows all server players — expected: filtered.
**Scope:** All players opening TAB during symptomatic windows are affected. Cross-client (Java + Bedrock). Cross-game-mode (lobby, Parkour, Creative, Skywars all referenced). Cosmetic — does not affect gameplay, data integrity, or security. Does affect player ability to identify staff and AFK state, with mild moderation-workflow impact.
**Domain confirmation:** operational
**Severity confirmation:** advisory (methodology axis). Operational urgency: P3 (user-visible, multi-user, cosmetic, not blocking gameplay).
**Notes:**
- The intake symptoms are related but distinct. They may share an upstream cause (TAB rendering, scoreboard team config, or display plugin) but reporters experienced different combinations. Bundling them into one finding is a judgment call: it preserves the iteration history coherently but risks blurring distinct root causes if investigation reveals them.
- Iteration pattern observed in the thread: fix → new-symptom-spawned. The 2026-05-13 fix to AFK-tag visibility was followed within hours by reports of missing mod colors and Senior Helper stars. Suggests coupled display dimensions sharing a single config surface.
- Severity is mixed: visual on its face, but moderation-relevant (staff identification) and player-experience-relevant. Held at advisory + P3 — the operational urgency axis carries the load here, per amendment 2026-05-17-2 sub-change H (severity-axis decoupling).
- The template's single `Reported by` field was used for the initial reporter; subsequent confirmers are listed inline. Recorded as a friction observation in this finding's journal.

## Investigation (optional)

**Investigated by:** waseric; persona-frame: developer (with admin overlap)
**Date:** 2026-05-17 (artifact-side; in-band investigation across the forum thread began 2026-05-13)
**Probable cause:** unknown — root-cause documentation does not appear in the visible artifact (fixes were applied without a root-cause narrative in the thread). Working hypothesis based on symptom clustering: a single TAB/scoreboard plugin configures multiple visual dimensions (rank prefix, name color, AFK indicator, Bedrock game-mode partitioning) and a config change to fix one dimension appears to perturb another.
**Code/configuration touchpoints:** unknown at this artifact level — would require operator access to the production server config. Likely candidates by symptom shape:
- TAB/scoreboard plugin configuration governing rank-prefix display (Senior Helper second star)
- Permission-group → color mapping (default lime-green Moderator color)
- AFK-state visibility configuration
- Bedrock-specific TAB scoping (game-mode filter vs. server-wide)
**Alternative hypotheses considered:**
- Client-side rendering difference (e.g., resource pack). Rejected — confirmed across multiple clients and platforms, and admin-side fixes had visible effect.
- Single-user permission misconfiguration. Rejected — too many distinct affected players (Sprainedd, TwistyYarn, multiple staff accounts).
**Proposed remedy:** unknown at this artifact level. In-band remediation has been ad-hoc. Documenting this finding may itself be the first step toward a structured remedy — route to `spec-write` for a TAB-display config audit, or close as already-remediated if the latest "looks normal" reports represent stable state.

## Route

**Decision:** unknown
**Decided by:** unknown
**Date:** unknown
**Target spec (if amend or new-spec):** unknown
**Rationale:** Route deferred. This finding's primary value at intake is documenting that the in-band fix iteration happened, surfacing the lack of root-cause documentation, and providing a structured artifact future TAB display issues can build on. A real routing decision (audit-spec write, close as remediated, or defer pending recurrence) is appropriate at the next triage pass — likely after observing whether the lingering AFK-tag glitches and rank-missing edge cases reported on 2026-05-15 recur or fade.
