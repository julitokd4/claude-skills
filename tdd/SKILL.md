---
name: tdd
description: Test-driven development (red-green-refactor) with unit tests for the logic that matters. Use when building a feature or fixing a bug test-first, when the user says "TDD" or "red-green-refactor", or when another skill needs the testing policy or a test-first loop for one unit of work.
---

# Test-Driven Development

Unit tests only, and only for the parts that matter. Every test must earn its
maintenance cost.

## Testing policy

**Test** (unit level, through the public interface):

- domain rules and business logic: pricing, permissions, state transitions,
  validation at trust boundaries;
- parsers, calculations, and non-trivial branching;
- security-relevant logic: authorization decisions, input sanitisation;
- a regression test for every bug fix.

**Do not test**:

- integration with real databases, queues, or HTTP (no integration or
  end-to-end tests);
- framework wiring, configuration, getters/setters, plain data classes;
- thin adapters that only translate calls to a library;
- code with no branching that the type checker already covers.

Keep domain logic pure (see `agentic-development` §2) so it is unit-testable
without mocks. Mock only at a true boundary port (repository, gateway), via a
hand-written fake of that port, never a library internal.

## The loop

1. **Red.** Write one failing test for the next piece of behaviour. Confirm
   it fails for the expected reason, not a typo or broken setup.
2. **Green.** Write the least code that makes it pass.
3. **Refactor.** Clean up what you touched, re-running the test after each
   change. Skip if nothing is worth cleaning.

One test at a time. Never write a batch of tests before any implementation:
it locks in imagined behaviour.

## What a good test looks like

- Reads like a spec ("rejects a refund larger than the charge") and survives
  an internal refactor.
- Asserts on return values or emitted results, not on mock call counts or
  private state.
- Expected values come from an independent source (a hand-worked example, the
  spec), never computed the way the code computes them.

## Applying this to one task

1. Write the named test. Run it; confirm the expected failure.
2. Write the minimal code to pass.
3. Refactor if warranted, keeping it green.
4. Report the passing run as the check. Never mark a task done without having
   run it.

Bugs follow the same loop: a test that reproduces the bug (red), then the fix.
