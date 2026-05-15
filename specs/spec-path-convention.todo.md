# Spec Path Convention — Future Feature Spec

> Status: Queued — not yet authored
> Origin: Project constitution session, 2026-05-15
> Type: Feature spec (use `spec-write`)

## Context

During the ai-tools project constitution, the repository layout was established as:

- `specs/` — authoritative artifacts (constitution, design specs, feature specs, journals)
- `docs/` — supporting material (research, recommendations, retrospectives)

The current skill family (`spec-write`, `spec-execute`, `spec-review`, `spec-amend`, `spec-design`) defaults to `docs/specs/<feature>.md` as the conventional spec path. This default should be updated to `specs/<feature>.md` to match the convention established in the constitution.

## Scope

- Update default path references in all six skills from `docs/specs/` to `specs/`
- Ensure the change is a default, not a hard constraint — `SPEC_PATH` is user-settable and any repo can override
- Update `project-constitution` to prescribe `specs/` as the default layout when producing constitution documents
- Consider whether `docs/` vs `specs/` distinction should be documented as a methodology convention (in the constitution itself) or left as a per-repo decision

## Decision Needed

Whether this is a universal methodology convention (all repos using the methodology should use `specs/`) or a per-repo layout choice that the methodology suggests but doesn't require. The constitution session leaned toward convention — `specs/` for authoritative, `docs/` for supporting — but this should be confirmed during spec authoring.

## Dependencies

- None. The skills accept any `SPEC_PATH`; this is a default-update, not a behavioral change.
