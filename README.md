# claude-skills

Personal, reusable [Claude Code](https://claude.com/claude-code) skills — installed at `~/.claude/skills/` so they're available across every project, not scoped to one repo.

## Skills

- **[agentic-development](agentic-development/SKILL.md)** — the default flow: understand, design (patterns, scalability, security surface), implement, verify, review; unit tests only for logic that matters; runs `owasp-check` at its review step.
- **[design-doc](design-doc/SKILL.md)** — designs a feature or system and publishes it as a warm, readable HTML artifact: hand-drawn SVG architecture, state and sequence diagrams, data model, API, background jobs, failure modes, and a phased plan. `example.html` is the reference page.
- **[explain](explain/SKILL.md)** — explains code, a system, or a concept as an elegant single-page HTML explainer: Mermaid diagrams for architecture and flows, annotated critical lines of code, and cards for the technologies and libraries.
- **[implement](implement/SKILL.md)** — implements one bounded task end-to-end, then validates it against its acceptance criteria or Definition of Done before calling it complete.
- **[juliopy](juliopy/SKILL.md)** — scaffolds a production-grade Python backend: Docker, Postgres, Django or FastAPI, uv, ruff, mypy, pytest, CI, pre-commit, typed config, health checks. Always resolves current latest-stable versions instead of hardcoding them.
- **[owasp-check](owasp-check/SKILL.md)** — checks a diff or a codebase against the OWASP Top 10:2025 (plus the API Security Top 10:2023 for HTTP APIs): every category ends Finding, Clean, or N/A, each finding is confirmed with `file:line` evidence and an attack path, and the report gives severity and fixes. Run by `agentic-development` at its review step.
- **[ship-pr](ship-pr/SKILL.md)** — ships a change as a PR via GitHub flow: feature branch off the default branch, secrets and `.gitignore` validated, only the relevant files staged, then pushed and opened as a PR.
- **[spec-driven-development](spec-driven-development/SKILL.md)** — spec-driven development: a project constitution (your engineering standards), then spec → clarify → plan → tasks → analyze → implement ⇄ converge under `specs/NNN-name/`, with templates, sign-off between stages, and an OWASP review at the end.
- **[tdd](tdd/SKILL.md)** — red-green-refactor plus the shared testing policy: unit tests for domain, security, and bug-fix logic only, no integration tests.

## Usage

Clone this repo to `~/.claude/skills/` (or symlink individual skill directories into it) to make these skills available in Claude Code globally.
