---
name: implement
description: Implement one bounded, given task end-to-end, then validate the result against its acceptance criteria or Definition of Done before calling it complete. Use when the user runs /implement, asks to implement a specific task, ticket, or tasks.md item, or wants one piece of work built and explicitly checked off against criteria rather than eyeballed.
---

# Implement

Implements one bounded task. The task is not done until the result has been
checked against explicit acceptance criteria — not "it compiles" or "the
happy path works," but a checklist that was verified, item by item.

The task comes from the argument passed to this skill: a task ID (e.g. from
a `tasks.md`), a ticket reference, or a plain description.

## 1. Establish the task and its acceptance criteria

Before writing any code, pin down two things:

1. **The task.** If it references a `tasks.md` entry, read that task's own
   line (`- [ ] T012 [P] [US1] Description in path`), its phase's Goal,
   Independent Test, and Checkpoint, and the plan.md and data-model.md parts
   it relies on. If it's a ticket or plain description, restate it in one or
   two sentences to confirm scope.
2. **Acceptance criteria / Definition of Done** — the concrete, checkable
   conditions that make this "done." Look for them in order of preference:
   - explicit criteria already attached to the task (constraints quoted in
     the `tasks.md` line, the story's acceptance scenarios and FR-### in
     `spec.md`, a ticket's AC section);
   - a repo-level Definition of Done (`CONTRIBUTING.md`, `CLAUDE.md`, an
     ADR, a house style doc);
   - if neither exists, derive 3–6 criteria yourself from the task
     description and confirm them with the user before implementing. Don't
     invent scope, but don't start without something concrete to check
     against.

Write the criteria down as a short checklist before moving on. Each
criterion must be independently checkable — a test that passes, a command
that succeeds, a behavior you can observe — never "works well," "is
robust," or "handles edge cases."

## 2. Understand before changing

Read the affected code, the repo's existing conventions, and any tests
already covering this area. Match what's there rather than guessing at
style or introducing new patterns.

## 3. Implement

Make the change following the Design rules in `agentic-development` §2
(pure domain logic, adapters at the edges, named patterns, scalability
checks). Where a criterion covers logic the `tdd` skill's testing policy says
to test, write that unit test first with its red-green-refactor loop. Do not
write integration or end-to-end tests.

Avoid scope creep: don't fix unrelated issues, refactor untouched code, or
add functionality the criteria don't call for. Note anything adjacent you
notice but don't fix, instead of silently expanding the task.

## 4. Validate against acceptance criteria

A distinct, explicit phase — never folded into "the implementation looked
right." For every criterion from step 1:

1. State the criterion.
2. Run or observe the specific check that proves it (a test, a command, a
   traced-through code path). Never mark a criterion satisfied without
   having actually performed the check.
3. Record the result: pass, fail, or not applicable (with a reason).

If any criterion fails, fix it and re-run the whole checklist — a fix for
one criterion can break another; don't assume the rest still hold. Always
run the repo's unit suite, lint, and type-check too, even when they aren't
listed as criteria.

Then run the review in `agentic-development` §4, including `owasp-check`
when the task touches an entry point, authn/authz, user input, data access,
secrets/config, or dependencies. Fix Critical/High findings before reporting.

## 5. Report

Present:

- the task and the acceptance criteria agreed in step 1;
- the checklist with each criterion's result and how it was verified;
- files changed;
- OWASP result, or "not applicable" and why;
- anything descoped, deferred, or found-but-not-fixed;
- any criterion that couldn't be verified, and why.

For a `tasks.md` task, mark it `[X]` only when every criterion passed.

Do not report the task as complete if any criterion is unmet or unverified.
