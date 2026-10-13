# Tasks: [FEATURE NAME]

**Input**: `specs/[NNN-short-name]/` — plan.md, spec.md, data-model.md, contracts/

**Format**: `- [ ] T### [P] [US#] Description with exact file path`
- `[P]`: different files, no dependency on an incomplete task
- `[US#]`: user-story phases only

<!-- Replace every sample below with real tasks. -->

## Phase 1: Setup

- [ ] T001 [Project or module scaffolding per plan.md in path/]

## Phase 2: Foundational (blocks all stories)

- [ ] T002 [Shared port, migration, auth guard, or config in path/]

**Checkpoint**: foundation ready; stories can start.

## Phase 3: User Story 1 - [Title] (P1) MVP

**Goal**: [what this story delivers]
**Independent Test**: [from spec.md]

- [ ] T003 [P] [US1] Unit test: [rule] in tests/unit/test_[name].py — must fail first
- [ ] T004 [US1] [Domain entity/service] in src/[...] ("name: required, max 120 chars")
- [ ] T005 [US1] [Port + adapter] in src/[...]
- [ ] T006 [US1] [Entry point with input validation and authz] in src/[...]

**Checkpoint**: unit suite, type-check, lint green; US1 quickstart steps pass.

## Phase 4: User Story 2 - [Title] (P2)

[...]

## Phase N: Polish

- [ ] T0XX Review per agentic-development §4, including owasp-check
- [ ] T0XX Walk quickstart.md end to end

## Dependencies

- Setup → Foundational → stories (in priority order, or in parallel) → Polish
- [Story-to-story dependencies, if any]

## Parallel Opportunities

- [T003, T007 can run together: different files]
