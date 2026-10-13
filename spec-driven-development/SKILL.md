---
name: spec-driven-development
description: Spec-Driven Development (SDD) in the GitHub spec-kit style - constitution, then specify → clarify → plan → tasks → analyze → implement ⇄ converge, with persisted artifacts under specs/<NNN-name>/ and human sign-off between stages. Use when the user asks for SDD, spec-driven development, spec-kit, or a new feature or capability that spans several user stories and deserves a spec, plan, and task list in the repo.
---

# Spec-Driven Development

The spec is the source of truth; code serves it. Intent (WHAT/WHY) lives in
`spec.md`, technical decisions (HOW) in `plan.md`, executable work in
`tasks.md`. When code and spec disagree, fix the spec or the code, never let
them drift.

Templates live next to this file in `templates/`. Copy the matching template,
then replace every placeholder; delete sections that don't apply instead of
writing "N/A".

For a bug fix or a small, low-ambiguity change, use `agentic-development`
instead; this ceremony would be overhead.

## Layout

```text
.specify/memory/constitution.md      # project principles, once per repo
specs/NNN-short-name/
├── spec.md                          # specify, clarify
├── checklists/requirements.md       # spec quality checklist
├── plan.md                          # plan
├── research.md                      # plan, phase 0
├── data-model.md                    # plan, phase 1 (if data is involved)
├── contracts/                       # plan, phase 1 (if external interfaces)
├── quickstart.md                    # plan, phase 1
└── tasks.md                         # tasks, then converge appends
```

These paths match spec-kit, so a repo already using `specify init` works
as-is. If `specs/NNN-*/` partially exists for this feature, resume at the
first incomplete stage.

## 0. Constitution (once per project)

If `.specify/memory/constitution.md` is missing, create it from
`templates/constitution.md`: fill the project name, keep the default
articles, adjust them to the repo's actual stack and conventions, and show it
to the user for approval. Later amendments bump its semantic version
(MAJOR: removed or redefined principle, MINOR: added principle, PATCH:
wording) and update `Last Amended`.

Every later stage checks against the constitution. A conflict is CRITICAL:
change the spec, plan, or tasks, or amend the constitution explicitly. Never
silently ignore a principle.

## 1. Specify → spec.md

1. Name the feature in 2–4 words, kebab-case (`user-auth`,
   `fix-payment-timeout`). Next number = highest `specs/NNN-` + 1, 3 digits.
   If on the default branch, create branch `NNN-short-name`.
2. Fill `templates/spec.md`. Rules:
   - WHAT and WHY only. No languages, frameworks, databases, APIs, or code
     structure.
   - User stories are prioritized (P1, P2, …) and each is **independently
     testable**: shipping only P1 is a viable MVP.
   - Every FR-### is testable. Every SC-### is measurable and
     technology-agnostic ("checkout completes in under 3 minutes", not "API
     under 200 ms").
   - Capture the non-functional requirements the constitution cares about:
     expected scale and growth, who may do what (authorization), sensitive
     data, failure behaviour.
   - Make informed guesses for anything with a reasonable default and record
     them under Assumptions. Use `[NEEDS CLARIFICATION: question]` only when
     the choice changes scope, security/privacy, or UX and no default
     exists. **Maximum 3**, prioritized scope > security/privacy > UX >
     technical.
3. Write `checklists/requirements.md` from
   `templates/requirements-checklist.md`, evaluate every item, fix the
   spec, and re-evaluate (max 3 passes). Leave remaining failures noted in
   the checklist.

## 2. Clarify (before planning)

Scan the spec for gaps across: scope and out-of-scope, roles, entities and
lifecycle, data volume and scale, error/empty states, performance,
reliability, observability, security and privacy, external dependencies and
their failure modes, edge cases and concurrency, ambiguous adjectives
("fast", "secure", "robust").

Ask at most **5** questions, **one at a time** (use `AskUserQuestion` when
available), only where the answer changes architecture, data model, tasks,
tests, or security. Each question leads with your recommended option and why.
After each answer:

- append `- Q: … → A: …` under `## Clarifications` / `### Session YYYY-MM-DD`;
- apply it to the right section (requirement, story, entity, success
  criterion, edge case), replacing any contradicted text;
- save the spec, and re-tick `checklists/requirements.md`.

If nothing material is ambiguous, say so and move on. The user may skip
this stage; warn that rework risk goes up.

**Checkpoint:** show spec.md and the checklist status. Get explicit approval
before planning; silence is not approval.

## 3. Plan → plan.md + design artifacts

Read spec.md, the constitution, and the repo's real architecture and similar
features. Fill `templates/plan.md`:

1. **Technical Context**: language, dependencies, storage, test framework,
   platform, performance goals, constraints, scale. Unknowns are
   `NEEDS CLARIFICATION`.
2. **Constitution Check** (gate): evaluate every article. An unjustified
   violation stops the plan; a justified one goes in Complexity Tracking.
3. **Phase 0 → research.md**: resolve every unknown and each dependency or
   integration choice as *Decision / Rationale / Alternatives considered*.
   Use research sub-agents for independent unknowns. A decision that is hard
   to reverse also gets an ADR in `docs/adr/` when the repo keeps them.
4. **Phase 1 → design**:
   - `data-model.md`: entities, fields, validation rules, relationships,
     state transitions, and the indexes the access patterns need;
   - `contracts/`: only if the feature exposes an interface (HTTP endpoints,
     events, CLI, public library API), including auth requirements and
     error shapes;
   - `quickstart.md`: runnable steps that show each user story working by
     hand, with expected outcomes. It is a run guide, not a test suite.
5. **Architecture and Security sections**: name the patterns used and the
   force each resolves, where domain logic vs. adapters live, how the design
   holds under the stated scale, and the security surface (entry points,
   callers, trust boundaries, sensitive data).
6. **Testing strategy**: the unit-test framework already in the repo and the
   list of units that get tests, per the `tdd` skill's testing policy. No
   integration, contract, or end-to-end tests.
7. Re-run the Constitution Check after design.

**Checkpoint:** show plan.md with the generated artifacts. Get explicit
approval before writing tasks.

## 4. Tasks → tasks.md

Fill `templates/tasks.md`. Every task uses exactly this format:

```text
- [ ] T012 [P] [US1] Create Order entity in src/orders/domain/order.py
```

checkbox, sequential ID, `[P]` only if it touches different files and has no
incomplete dependency, `[USn]` on user-story tasks only, and an exact file
path.

Phases: **Setup** → **Foundational** (blocks all stories) → **one phase per
user story** in priority order, each with its Goal, Independent Test, and
Checkpoint → **Polish** (cross-cutting work and the review in §7).

- Within a story: unit tests (where the plan lists them) → domain → ports
  and adapters → entry points.
- Unit-test tasks name the test file and case and come before the code they
  cover. Other tasks are done when type-check/lint pass and the story's
  Independent Test holds.
- Quote data-model constraints (max length, required, enum values) verbatim
  in the task, so they are not left to implementation-time guesswork.
- A task must be specific enough to complete without other context.

## 5. Analyze (read-only gate)

Before implementing, cross-check spec.md, plan.md, and tasks.md **without
editing them**:

- **Duplication**: near-identical requirements.
- **Ambiguity**: vague adjectives, leftover placeholders.
- **Underspecification**: stories without acceptance criteria, tasks naming
  files the plan doesn't.
- **Constitution**: any conflict is CRITICAL.
- **Coverage**: requirements with no task, tasks with no requirement,
  scale/security success criteria with no task.
- **Inconsistency**: terminology drift, entities in one file but not the
  other, contradictory ordering.

Report a findings table (ID, category, severity, location, recommendation),
a requirement → task coverage table, and coverage %. CRITICAL findings must
be fixed before implementing; propose the edits and apply them only with
the user's approval.

**Checkpoint:** show tasks.md and the analysis. Get explicit approval to
implement.

## 6. Implement ⇄ Converge

**Gate**: if any file in `checklists/` has unchecked items, show the counts
and ask whether to proceed.

**Implement** phase by phase, in dependency order:

- Follow the `agentic-development` Design and Implement rules.
- Run unit-test tasks with the `tdd` skill's red-green-refactor loop.
- Tasks on the same file run sequentially; `[P]` tasks may run in parallel.
- Mark each finished task `[X]` in tasks.md right away. Halt on a failing
  sequential task and report it.
- At each story checkpoint, run the unit suite, type-check, and lint, and
  walk the story's quickstart steps. After US1, offer to stop and ship the MVP.
- If reality diverges from plan.md (an unforeseen file, a better approach),
  update plan.md and note the deviation.

**Converge** after each implement pass: assess the code (not the checkboxes)
against every FR, SC, acceptance scenario, plan decision, and constitution
article. Classify each gap as `missing`, `partial`, `contradicts`, or
`unrequested`. Append them to the end of tasks.md as a new
`## Phase N: Convergence` section, continuing the IDs:

```text
- [ ] T042 Enforce per-tenant filter on order listing per FR-007 (contradicts)
```

Append only: never edit spec.md or plan.md here, never renumber or delete
tasks. If nothing is left, leave tasks.md untouched and report
**Converged**. Repeat implement → converge until it converges.

## 7. Review and close out

1. Run the review in `agentic-development` §4 over the whole feature diff,
   including `owasp-check`. Critical/High findings and correctness bugs
   become a new Convergence phase (source-ref like `OWASP A01`) and go
   through implement again.
2. Report: requirements satisfied (FR/SC → evidence), patterns and
   trade-offs, checks run with results, OWASP result, remaining
   Medium/Low risks, anything deferred.
3. Leave all artifacts in the repo as the record. Ship with `ship-pr`.
