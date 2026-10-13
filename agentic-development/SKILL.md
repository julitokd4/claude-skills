---
name: agentic-development
description: Plan, design, implement, verify, and security-review software changes. Use for features, bug fixes, refactoring, migrations, and other repository changes.
---

# Agentic Development

Five steps. The bar is code a senior engineer would still stand behind when the
codebase and traffic triple: sound structure, named patterns, safe by default.
Not the shortest output that passes today.

## 1. Understand

1. Restate the outcome and its acceptance criteria.
2. Read the repo's instructions, the code you will touch, its callers, and a
   similar existing feature. Find the lint, type-check, and test commands.
3. Ask only about gaps that would change the implementation. Do not invent
   requirements.

## 2. Design

Before editing a non-trivial change, write a short plan:

- **Structure**: where the logic lives. Domain logic stays pure; HTTP, DB,
  queues, and third-party APIs live in adapters at the edges. Dependencies
  are injected, not imported globals.
- **Patterns**: use one only when it resolves a real force (strategy for
  swappable behaviour, repository for persistence, adapter for an external
  API, factory for complex construction). Name it in the plan.
- **Scalability**: stateless services; indexed queries with no N+1;
  paginated lists; slow or failure-prone work on a queue with idempotency and
  retries; a timeout on every network call; backward-compatible schema
  changes (expand → migrate → contract).
- **Security surface**: entry points, who can call them, inputs crossing a
  trust boundary.
- **What gets unit tests**: per the `tdd` skill's testing policy.
- **Trade-off**: when two approaches are viable, one line on cost now vs.
  cost later; pick the durable one.

Follow the repo's existing conventions. Where they conflict with the above,
flag it rather than mixing styles.

## 3. Implement

- Make the change in vertical slices that each leave the code working.
- Validate input at trust boundaries; return explicit, typed errors.
- Write the unit tests the plan named, test-first via `tdd` where the logic
  is non-trivial. Do not write integration or end-to-end tests.
- Do not hand-edit generated files or refactor unrelated code.

## 4. Verify and review

Run, and read the output yourself:

1. Unit tests for the changed code, then the full unit suite.
2. Type check, lint, format.
3. Build, when the project has one.

Then review the full diff:

- **Correctness**: wrong behaviour, unhandled errors, race conditions,
  non-idempotent operations, breaking changes, unsafe migrations.
- **Design**: logic in the wrong layer, leaked I/O in domain code, missing
  timeout/pagination/index, an untested important rule, needless complexity.
- **Security**: run the `owasp-check` skill on the diff whenever it touches an
  entry point, authn/authz, user input, data access, secrets/config, or
  dependencies. Otherwise record "OWASP check: not applicable" and why.

Fix every Critical/High security finding and every correctness bug before
presenting. For significant changes, ask an independent review agent when
available.

Never claim a check passed unless you ran it.

## 5. Report

1. Outcome
2. Design decisions and patterns used, with trade-offs
3. Files changed
4. Checks run (commands and results), what was unit-tested and why
5. OWASP result; Medium/Low/Plausible findings as remaining risks
6. Stopgaps and follow-ups (`TODO(reason)` in code where relevant)

Do not describe incomplete or unverified work as done.
