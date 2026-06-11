# Findings pipeline missing investigate/route skills — Journal

## 2026-05-20 — Intake: No skill coverage for Phases D (Investigation) and E (Route) of the findings pipeline

**Captured by:** Eric Wasgatt; persona-frame: intake
**Signal source:** text
**New status:** `intake`
**Notes:** Surfaced during a session in a separate private repo where the operator attempted to use `spec-design` to investigate and route the generate-readme stale-paths finding (20260519-generate-readme-stale-agents-paths). The operator recognized that the architect persona kept collapsing investigation + routing into "amend + direct fix," bypassing execution rigor (spec-write → spec-execute). The same pattern had occurred across multiple findings on 2026-05-19. Root observation: the pipeline design defines five phases (A: schema, B: intake, C: triage, D: investigation, E: route) but only B and C have skill implementations. The gap forces operators to repurpose spec-design for a job it was not designed to do, producing architectural analysis when the need is diagnostic investigation and routing.
