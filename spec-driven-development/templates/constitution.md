# [PROJECT_NAME] Constitution

## Core Principles

### I. Durable Design

Build for the system as it will be when the codebase and traffic triple.
When an option is cheap now but costly later, choose the durable one and
record the trade-off. Durable means structurally sound, not gold-plated:
YAGNI applies to features, never to structure.

### II. Ports and Adapters

Domain logic MUST be pure and free of I/O. HTTP, databases, queues, and
third-party APIs live in adapters at the edges. Dependencies point inward
and are injected through constructors or parameters.

### III. Deep Modules and Named Patterns

Modules expose small interfaces over substantial behaviour, with decisions
likely to change behind a seam. A pattern (strategy, repository, adapter,
factory, …) is used only when it resolves a real force, and the plan names
it. Prefer composition and immutable data; make illegal states
unrepresentable with precise types validated at the boundary.

### IV. Scalability

Services MUST be stateless. Every query is checked for indexes and N+1
access; lists are paginated (cursor-based for large sets). Slow or
failure-prone work runs on a queue with idempotency keys and retries with
backoff. Every network call has a timeout. Schema changes are backward
compatible (expand → migrate → contract).

### V. Security by Default

Input is validated at every trust boundary. Every entry point declares who
may call it and enforces authorization server-side. Secrets come from the
environment, never the code. Every feature passes an OWASP check before it
ships; Critical and High findings block release.

### VI. Focused Unit Testing

Unit tests cover domain rules, security decisions, parsers and
calculations, and every bug fix (as a regression test), written before the
code. Framework wiring, configuration, thin adapters, and plain data
classes are not tested. No integration or end-to-end tests.

### VII. Observability and Errors

Structured logs carry correlation IDs. Errors are explicit and typed,
handled at the layer that can act on them, and logged with context.
Configuration is typed and validated at startup.

## Technology Constraints

[STACK_AND_PLATFORM: languages, frameworks, storage, hosting this project
commits to; remove this line if none are fixed]

## Quality Gates

A feature is done when its unit tests, type check, lint, and formatting
pass; analyze reports no CRITICAL findings; converge reports Converged; and
the OWASP check has no open Critical or High findings. Trade-offs and
stopgaps are written in plan.md, an ADR, or a `TODO(reason)`.

## Governance

This constitution supersedes conflicting practices. Every plan runs the
Constitution Check; a violation needs a justification in Complexity
Tracking. Amendments state their rationale and bump the version (MAJOR:
principle removed or redefined, MINOR: principle added or expanded,
PATCH: wording).

**Version**: 1.0.0 | **Ratified**: [YYYY-MM-DD] | **Last Amended**: [YYYY-MM-DD]
