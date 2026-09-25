# AGENTS.md — axia

Greenfield repo: no app source committed yet (only `opencode.json` + `.opencode/agents/` scaffolding). Scaffold per spec/architecture; don't assume an existing `src/`, `docs/`, or `tests/` layout — create it as designed.

## Execution model

- Orchestrator is the only primary agent and **never edits source code** (`edit: deny`). It owns decomposition, specialist selection, delegation, synthesis, and delivery.
- **Do not use a fixed pipeline.** The orchestrator chooses the minimum set of specialists needed for each goal and may run independent work in parallel.
- Requirements/design should be sufficiently settled before implementation. For larger or consequential implementation work, the orchestrator presents the resulting plan and gets explicit user go/no-go before @coder starts.
- Review is an independent quality gate. Cap coder/reviewer fix cycles at 3; stop and report unresolved issues after that.
- Carry goal, constraints, relevant artifacts, and decisions explicitly in every delegation; subagents do not automatically share conversation context.
- Do not add agents, technologies, abstractions, or workflow stages merely because they are available. Prefer the simplest solution that satisfies the user's goal.

## Available capabilities

See `docs/CAPABILITIES.md`. The capability list is a toolbox, not an implementation prescription. The orchestrator decides whether a capability is useful for a given task.

## Permissions — `opencode.json` is truth

- `opencode.json` overrides model/permission frontmatter in `.opencode/agents/*.md` when they conflict.
- `@requirements` / `@architect` / `@ux`: `docs/**` only.
- `@ui`: `docs/**` + `src/**` on ask.
- `@coder`: full edit + bash.
- `@reviewer`: no edits; read-only review with appropriate git inspection.
- `@tester`: `tests/**` only + bash.
- Secrets live in untracked `.env`; never commit or paste their contents.

## Artifact locations

- Specs → `docs/specs/<task>.md` (concise problem, scope, testable acceptance criteria, open questions; no implementation solution).
- Architecture → `docs/architecture/<task>.md` (approach, key decisions/state ownership, risks, what NOT to build).
- UX flows → `docs/ux/<task>.md`.
- UI definitions → `docs/ui/<task>.md`.
- Tests → `tests/**`.

## Current project intent

The initial product goal is a scheduled job-finding system: maintain a list of target companies and a professional profile; periodically retrieve available jobs from those companies; identify roles that match the profile; store the results; and expose new and historical matches through a UI.

This is the **business goal, not a fixed technical design**. The orchestrator and specialists must determine the appropriate architecture, integrations, matching approach, scheduler, persistence, UI, and use of available capabilities.

## UI / UX quality

The product should have a **polished, professional, intuitive UI/UX suitable for regular daily use**. UX quality is a first-class product requirement, not something to optimize only after functionality is complete.

The UI/UX should:
- Make the primary user workflows immediately clear.
- Present information with strong visual hierarchy and sensible information density.
- Be easy to scan and navigate, especially when reviewing many job matches.
- Provide clear states for loading, empty results, errors, new results, and previously seen results.
- Be responsive and usable across relevant screen sizes.
- Feel cohesive and production-quality rather than like an internal prototype.
- Minimize unnecessary interaction and cognitive load.
- Use appropriate interaction patterns, accessibility practices, and useful user feedback.
- Preserve consistency across screens and components.

These are **quality goals, not implementation instructions**. Do not prescribe a framework, component library, visual style, navigation pattern, color palette, or page structure unless the requirements or design work establish a reason for one.

The UX/UI specialists and architect should determine the appropriate design patterns, visual language, component structure, and implementation technology based on the product requirements. The orchestrator should ensure that UX quality is considered before implementation and that the final UI is evaluated against these goals.

## Engineering biases

- Prefer simple, solo-maintainable architecture.
- Prefer official/reliable company/ATS APIs where available.
- Mock external ATS calls in tests; never hit real endpoints from automated tests.
- Cover scheduler behavior and important external failure paths such as API outages, malformed responses, rate limits, and authentication expiry where relevant.
- Review acceptance criteria, architecture adherence, security/secrets handling, external-integration failure modes, and injection risks where applicable.
