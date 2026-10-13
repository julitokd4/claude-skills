---
name: agentic-development
description: Plan, design, implement, verify, and security-review software changes. Use for features, bug fixes, refactoring, migrations, and other repository changes.
---

# Agentic Development

For a new feature or capability with several user stories, or when the user
asks for a spec, use `spec-driven-development` instead; it reuses the Design,
Implement, and Review rules below. This flow fits bug fixes, refactors, and
single-story changes.

Five steps. The bar is code a senior engineer would still stand behind when the
codebase and traffic triple: sound structure, named patterns, safe by default.
Not the shortest output that passes today.

**Least new code, never less structure.** Write only what the task needs, and
reuse before you write. That is a rule about features and code volume; it never
removes a layer, a seam, validation, or a test the design calls for.

## 1. Understand

1. Restate the outcome and its acceptance criteria.
2. Read the repo's instructions, the code you will touch, and a similar
   existing feature. Find the lint, type-check, and test commands.
3. **Map the reach.** List every place the change must land: callers of each
   function you change, tests, fixtures, config, exports, docs. Note what it
   could break for users: data it could destroy or expose, callers that stop
   working. That list is the scope; extra features are not.
4. **Find the expected load** (README, deploy config, existing infra): one
   user running a script, or many users and workers at once. Design and
   review against that, and say which load you assumed.
5. Ask only about gaps that would change the implementation. Do not invent
   requirements.

## 2. Design

Before editing a non-trivial change, write a short plan:

- **Reuse first.** For each piece, stop at the first option that fully works:
  1. Does it need to exist? Name in one line the features, options, and
     flexibility nobody asked for, and skip them.
  2. Already in this codebase (helper, component, service, pattern)? Use it
     the way the surrounding code does.
  3. Standard library or a platform feature? Use it, unless the project has
     its own.
  4. An installed dependency? Use it. Never add a dependency for a few lines.
  5. Otherwise, the minimum new code, inside the structure below.
- **Structure**: where the logic lives. Domain logic stays pure; HTTP, DB,
  queues, and third-party APIs live in adapters at the edges. Dependencies
  are injected, not imported globals. Keep the layers, interfaces, and
  conventions the codebase already has.
- **Patterns**: use one only when it resolves a real force (strategy for
  swappable behaviour, repository for persistence, adapter for an external
  API, factory for complex construction). Name it in the plan. An
  abstraction with one implementation and no real force is speculation.
- **Scalability**: stateless services; indexed queries with no N+1;
  paginated lists; slow or failure-prone work on a queue with idempotency and
  retries; a timeout on every network call; backward-compatible schema
  changes (expand → migrate → contract).
- **Security surface**: entry points, who can call them, inputs crossing a
  trust boundary.
- **What gets unit tests**: per the `tdd` skill's testing policy.
- **Trade-off**: when two approaches are viable, one line on cost now vs.
  cost later; pick the durable one. Between options of equal size, pick the
  one that is correct on edge cases.

Follow the repo's existing conventions. Where they conflict with the above,
flag it rather than mixing styles.

## 3. Implement

- Make the change in vertical slices that each leave the code working.
- Finish everything on the reach list: callers, tests, and fixtures the
  change breaks are part of the change.
- Validate input at trust boundaries; return explicit, typed errors.
- **Bug fix**: grep every caller of the function you touch, then fix the root
  cause once in the shared code, with a regression test.
- Code you move or merge keeps its validation and error handling.
- No options, config, wrappers, or "for later" code nobody asked for.
  Deleting code beats adding it.
- A deliberate shortcut with a known limit gets a one-line comment naming the
  limit and the trigger to revisit:
  `TODO(<limit>; upgrade when <trigger>)`.
- Comments explain only the *why* the code cannot show.
- Write the unit tests the plan named, test-first via `tdd` where the logic
  is non-trivial. Do not write integration or end-to-end tests.
- Do not hand-edit generated files or refactor unrelated code.

**Never cut**: validation at trust boundaries, error handling that prevents
data loss, security, accessibility, or anything the user asked for.

## 4. Verify and review

Run, and read the output yourself:

1. Unit tests for the changed code, then the full unit suite.
2. Type check, lint, format.
3. Build, when the project has one.

Then review the full diff **and the code it touches** (callers, callees,
tests), in this order of importance:

1. **Correct**: wrong result, missed edge case (empty, zero, last item,
   rounding, time zones), a caller broken by a changed signature or
   behaviour, a fix applied in one caller while the shared code stays broken.
2. **Safe**: run the `owasp-check` skill on the diff whenever it touches an
   entry point, authn/authz, user input, data access, secrets/config, or
   dependencies; otherwise record "OWASP check: not applicable" and why. Also
   data loss: swallowed errors, writes in the wrong order, no transaction.
3. **Holds under the expected load**: check-then-write races, per-process
   state that must be shared, a query per item, lists or memory that only
   grow, O(n²) on large input, missing timeout/pagination/index.
4. **Tested**: risky logic (branch, parser, money, security, data writes, a
   bug fix) has one test that fails when it breaks.
5. **Fast**: real slowdowns only.
6. **Lean**: dead code, unused options, a helper the repo already has, a
   dependency doing a few lines' work, near-copies that must change together,
   one function doing unrelated jobs. Also logic in the wrong layer or I/O
   leaked into domain code.

Every finding needs a concrete case: "this input leads to this wrong
result". No case, no finding. A marked `TODO(<limit>; …)` shortcut is a
decision, not a finding, unless the expected load already crosses it. Prefer
the smallest fix; never add layers the problem does not need.

Fix every correctness bug and every Critical/High security finding before
presenting. For significant changes, ask an independent review agent when
available.

Never claim a check passed unless you ran it.

## 5. Report

1. Outcome
2. Design decisions and patterns used, with trade-offs; what was reused, and
   what was deliberately not built
3. Files changed
4. Checks run (commands and results), what was unit-tested and why
5. OWASP result; Medium/Low/Plausible findings as remaining risks
6. Stopgaps and follow-ups (`TODO(<limit>; upgrade when <trigger>)` in code)
7. **Not checked**: one line on what mattered but could not be verified, and
   any risk the user must know

Do not describe incomplete or unverified work as done.
