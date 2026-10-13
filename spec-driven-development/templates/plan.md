# Implementation Plan: [FEATURE]

**Feature Directory**: `specs/[NNN-short-name]` | **Date**: [YYYY-MM-DD] | **Spec**: [spec.md](spec.md)

## Summary

[Primary requirement + chosen technical approach, 2–4 sentences]

## Technical Context

**Language/Version**: [or NEEDS CLARIFICATION]
**Primary Dependencies**: [...]
**Storage**: [... or N/A]
**Unit Test Framework**: [already in the repo]
**Target Platform**: [...]
**Performance Goals**: [from SC-###, e.g., p95 < 300 ms at 500 req/s]
**Constraints**: [...]
**Scale**: [data volume, users, growth over two years]

## Constitution Check

*Gate: must pass before Phase 0; re-check after Phase 1.*

| Article | Status | Evidence / Justification |
|---------|--------|--------------------------|
| I. Durable Design | Pass/Fail | |
| II. Ports and Adapters | | |
| III. Deep Modules and Named Patterns | | |
| IV. Scalability | | |
| V. Security by Default | | |
| VI. Focused Unit Testing | | |
| VII. Observability and Errors | | |

## Architecture

**Patterns**:

| Pattern | Where | Force it resolves |
|---------|-------|-------------------|
| [e.g., Repository] | [module] | [swap storage, keep domain pure] |

**Domain vs. adapters**: [which modules are pure, which ports they define, which adapters implement them]

**Scalability**: [statelessness, indexes and access patterns, pagination, async work and idempotency, timeouts and fallbacks, schema migration steps]

**Trade-offs**: [option chosen vs. rejected, cost now vs. cost later]

## Security Design

| Entry point | Caller | AuthN / AuthZ | Untrusted input | Sensitive data |
|-------------|--------|---------------|-----------------|----------------|
| [POST /orders] | [signed-in user] | [session; owns account] | [body] | [address] |

## Testing Strategy

Units that get tests (per the `tdd` testing policy) and why:

- [domain rule / authorization decision / parser] → [test file]

Not tested: [wiring, adapters, config], verified by type-check and quickstart.md.

## Project Structure

```text
specs/[NNN-short-name]/
├── plan.md  research.md  data-model.md  quickstart.md
├── contracts/
└── tasks.md
```

```text
[real source paths this feature creates or edits]
```

## Requirement Coverage

| Requirement | Plan section |
|-------------|--------------|
| FR-001 | |
| SC-001 | |

## Complexity Tracking

*Only for Constitution Check violations that are justified.*

| Violation | Why needed | Simpler alternative rejected because |
|-----------|------------|--------------------------------------|
