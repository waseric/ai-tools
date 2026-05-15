# ai-tools — Mission

> Audience: Eric Wasgatt (author); AI coding agents consuming the methodology's artifacts; engineers, hobbyists, and product owners evaluating or adopting lifecycle-aware development practices
> Status: Living document — last updated 2026-05-15

## Purpose

A methodology for AI-assisted software development that spans the full product lifecycle — from greenfield instantiation and brownfield adoption through architecture, build, operation, iteration, transition, and retirement. Rooted in SDLC, ITSM, and ITIL disciplines, it encodes the principle that sustainable decisions made early in a product's life are what make the mid- and end-of-life phases survivable. The methodology is prescriptive by design: autonomous development is enabled by governance, not by its absence — the goal is outputs a product's owner can be accountable for, not outputs produced quickly without oversight.

## Audience

- **Eric Wasgatt** — author, primary user, and practitioner. Draws on 28 years of engineering experience across professional platform engineering and personal hobby development. Both contexts contribute learnings; both consume the methodology.
- **AI coding agents** — the direct consumers of methodology artifacts. Agents execute within the structure the methodology defines: phases, pauses, disciplines, and governance checkpoints.
- **Engineers, hobbyists, and product owners** — anyone evaluating or adopting lifecycle-aware development practices. Contributions and feedback from outside perspectives are welcomed.

## In Scope

- Methodology artifacts covering product lifecycle phases: instantiation, adoption, requirements, architecture, design, build, operation, iteration, transition, and retirement.
- Supporting code within methodology artifacts when the methodology requires it (diagnostic scripts, templates, generators, validation utilities).
- Architecture and design specs that evolve the methodology itself.
- Supporting documentation: recommendations, retrospectives, conversation exports, and research that informs methodology development.
- Cross-cutting disciplines that apply across multiple methodology artifacts (session economy, multi-repo commit discipline, verification protocols).

## Out of Scope

- Standalone tools or applications that exist independently of a methodology artifact — things with their own release lifecycle, user base, or runtime dependencies beyond what a methodology consumer already has.
- Domain-specific methodology variants (ServiceNow, Minecraft, etc.) — those live in their respective project repos and consume the methodology; they do not define it.
- Prescribing specific AI tooling, models, or platforms — the methodology is tooling-agnostic.
- Project management tooling or dashboards.
- Organizational governance (team charters, RACI, escalation paths).
- Defining or enforcing specific ITSM/ITIL/SDLC framework compliance — the methodology is *informed by* these disciplines, not a certification path for them.

## Success

Six months in, the methodology covers enough of the product lifecycle that a new project can be taken from instantiation through active development with the methodology as the primary source of engineering governance — and the artifacts produced during that journey (specs, journals, review verdicts) are durable enough that a different person or agent could pick up the work without re-deriving intent. Contributions or adoptions from outside the author's own projects would be a strong signal that the methodology generalizes beyond its origin.
