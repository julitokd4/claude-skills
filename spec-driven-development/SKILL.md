---
name: spec-driven-development
description: Spec-Driven Development (SDD) - write persisted spec.md, plan.md, and tasks.md artifacts under specs/<slug>/ with human sign-off between stages, then implement against tasks.md. Use when the user asks for SDD, spec-driven development, a spec-kit style workflow, or explicitly wants a feature tracked through spec/plan/tasks documents in the repo instead of an in-conversation plan.
---

# Spec-Driven Development

Four stages, each persisted as a file under `specs/<slug>/`, each gated on explicit
human sign-off before the next begins. The files are the durable record — write
them so someone opening the repo cold, with no memory of this conversation,
understands what was decided and why.

For a well-understood, single-file, low-ambiguity change, this ceremony is
overkill — use the lighter in-conversation `agentic-development` workflow instead.

## 0. Set up the spec directory

1. Derive a short kebab-case slug from the feature name.
2. List `specs/` and find the highest existing numeric prefix (`NNN-`); the new
   directory is `specs/<NNN+1>-<slug>/`, zero-padded to 3 digits. First spec is `001-`.
3. If `specs/<NNN>-<slug>/` already has partial artifacts (an in-progress SDD),
   resume from the latest incomplete stage — read what exists rather than
   restarting it.

## 1. Specify → spec.md

Write the problem in terms of WHAT and WHY, never HOW: no tech stack, no file
names, no implementation detail — that belongs in the plan.

Sections: Overview, User stories, Functional requirements (numbered, each one
independently testable), Non-goals, Open questions.

Ask the user only about ambiguities that would change scope; mark anything
else assumed as an explicit assumption in the doc rather than blocking on it.

**Done when:** every functional requirement is numbered and testable, and no
open question left in the doc would change scope if answered differently.

**Checkpoint:** show the user spec.md. Get explicit approval before writing
plan.md — a lack of objection is not approval; ask directly.

## 2. Plan → plan.md

Read spec.md plus the repo's actual conventions (architecture docs, existing
similar features, stack, test setup) before writing this.

Sections: Technical approach, Affected components/files, Data/API changes,
Testing strategy, Risks and tradeoffs, and a requirement-coverage table mapping
each spec.md requirement to the plan section that satisfies it.

Technical approach follows the Design rules in `agentic-development` §2:
name the patterns used, where domain logic vs. adapters live, and how the
change holds under load (indexes, pagination, queues, timeouts).

Testing strategy names the unit test framework already in the repo and lists
which units get tests, per the `tdd` skill's testing policy. Unit tests only;
no integration or end-to-end tests.

Risks and tradeoffs include the security surface: entry points, callers,
and inputs crossing a trust boundary.

**Done when:** every requirement in spec.md's coverage table maps to a
concrete part of the plan — no requirement left unaddressed.

**Checkpoint:** show the user plan.md. Get explicit approval before writing
tasks.md.

## 3. Tasks → tasks.md

Decompose plan.md into small, ordered, independently verifiable tasks as a
checklist. Each task: id, one-sentence description, files it touches,
dependencies on other task ids, the concrete check that proves it done, and a
checkbox.

For tasks covering logic plan.md marks for testing, the done-condition is a
named unit test written first (file and case, not just "tests pass"). For
the rest (wiring, adapters, config, docs), the done-condition is a passing
type-check/lint/build or an observable behaviour; say which.

Prefer vertical slices over horizontal layers. Mark tasks with no dependency
on each other as parallelizable.

**Done when:** every plan.md section maps to at least one task, and every
task's done-condition is something you can actually run or observe, not a
vague description.

**Checkpoint:** show the user tasks.md. Get explicit approval before
implementing.

## 4. Implement

Work tasks in dependency order, one at a time:

1. Mark the task in-progress.
2. If the task's done-condition is a test, invoke the `tdd` skill and run its
   red-green-refactor loop for that one task, following the repo's existing
   patterns. Otherwise (config, docs, etc.), make the change directly.
3. Run the task's stated check. Never check a box without having actually run it.
4. Check the box in tasks.md.

If reality diverges from plan.md mid-implementation — a needed file the plan
didn't foresee, a different approach — update plan.md to match and note the
deviation there. Do not let the artifact silently drift out of sync with the
code.

## 5. Close out

Once every task is checked, run the review in `agentic-development` §4
(including `owasp-check` where it applies) over the whole change and fix
Critical/High findings. Then summarize what changed against spec.md's
requirements, list anything deferred or descoped, report the verification
actually performed (tests/build/lint run and their results, OWASP result), and leave
spec.md, plan.md, and tasks.md in the repo as the record of the change.
