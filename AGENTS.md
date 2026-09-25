# AGENTS.md — axia

Greenfield repo: no app source committed yet (only `opencode.json` + `.opencode/agents/` scaffolding). Scaffold per spec/architecture; don't assume existing `src/`, `docs/`, or `tests/` layout — create it as designed.

## Execution model (differs from defaults — follow this)

- Orchestrator is the only primary agent and **never edits code** (`edit: deny` in `opencode.json`). Break work down and delegate via subagents; do small typo/one-line fixes directly and say you're skipping the pipeline.
- Fixed pipeline per `.opencode/agents/orchestrator.md`: `@requirements` → `@architect` (+ `@ux` → `@ui` only if user-facing) → **STOP, get explicit user go/no-go** → `@coder` → `@reviewer` → `@tester` → summarize. Never start `@coder` before approval.
- Review loop cap: max 3 `@coder`↔`@reviewer` cycles, then stop and report to the user.
- Carry spec + architecture decisions explicitly in every delegation — subagents don't share context automatically.

## Permissions — `opencode.json` is truth

- `opencode.json` overrides model/permission frontmatter in `.opencode/agents/*.md` when they conflict (they currently do — trust `opencode.json`).
- `@requirements` / `@architect` / `@ux`: `docs/**` only. `@ui`: `docs/**` + `src/**` on ask. `@coder`: full edit + bash. `@reviewer`: no edits (read-only; `git diff/log/grep` allowed). `@tester`: `tests/**` only + bash. Ignore `opencode.json.bak` / `.env.bak` — stale duplicates.

## Where things go

- Specs → `docs/specs/<task>.md` (≤1 page: problem, scope in/out, testable acceptance criteria, open questions; no solutions).
- Designs → `docs/architecture/<task>.md` (approach, key decisions/state ownership, risks, what NOT to build).
- UX flows → `docs/ux/<task>.md`; screens → `docs/ui/<task>.md`. Keep minimal — personal tool, no consumer polish.
- Tests → `tests/**` (only place `@tester` can write).

## Project biases (from agent briefs — still the plan until architecture says otherwise)

- What: scheduled backend agent polling ATS APIs (Greenhouse / Workday / SmartRecruiters) for target companies — ranking, storage, notification — not an always-on service. Hosting bias: Railway + GitHub Actions for scheduling; favor simple, solo-maintainable patterns.
- Tests: **mock all external ATS calls — never hit real endpoints**. Cover cron/schedule triggers and failure paths (API down, malformed response, 429/rate-limit).
- Review gate: check acceptance criteria + architecture adherence, plus ATS error handling (failures, rate limits, auth expiry), secrets handling, and injection risks in query/filter logic. Concrete line-level findings, `pass` or `changes requested`.
- Secrets live in untracked `.env` — never commit it or paste its contents into docs/specs.
