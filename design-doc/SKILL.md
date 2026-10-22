---
name: design-doc
description: Design a feature or system and publish the design as a warm, readable HTML artifact with hand-drawn SVG diagrams (architecture, state machine, sequences), data model, API, background jobs, failure modes, and a phased plan. Use when the user asks to design a solution, feature, or system and show it as an artifact or page, or asks for a design doc or design proposal.
---

# Design doc

Turn "design X" into one published artifact page that a teammate can read cold and build from. The house look comes from [example.html](example.html), the waitlist design: warm cream and terracotta, Atkinson Hyperlegible body, Source Serif 4 headings, JetBrains Mono for code.

## 1. Ground the design

- Read the code the feature touches: modules, models, error types, the repo's layering, existing ADRs. Every file path, class and convention named on the page comes from what you read.
- Settle the decisions that change the architecture (job queue, scope, sync vs. async, where state lives). Ask with `AskUserQuestion` only for those, at most 3 questions, recommended option first. Everything else gets a sensible default that you state on the page.
- Done when you can name the system's source of truth, every side effect, and every place the request path crosses a network.

## 2. Choose the sections

Keep a section only when it earns its place for this subject. Order as listed.

| Section | Include when | Content |
|---|---|---|
| Header | always | eyebrow, name, 2–3 sentence lede, meta row (scope, key decisions, date), pill nav |
| Key decisions | always | 3–5 decisions, each a heading + 1–2 sentences on what it buys |
| Screens | there is UI | example screens with sample data, marked as examples (`.phone`, `.board`) |
| Architecture | more than two components | SVG flow diagram + 2–3 bullets on why the non-obvious edges exist |
| Lifecycle | an entity has statuses | SVG state machine; final states double-bordered; where the rules live |
| Flows | a request or job crosses components | SVG sequence diagrams, one per flow that matters |
| Data model | new tables | `.schema` cards with columns and indexes, each index saying which query it serves |
| API | new endpoints | table: endpoint, caller, behaviour, error codes; boundary validation |
| Background jobs | async work | table: task, queue, trigger, why it's safe to run twice |
| Failure modes | always | table: failure, what users see, what the system does |
| Code layout | new modules | `pre` tree following the repo's structure; `.callout` for any convention change |
| Plan | always | numbered phases, shortcuts with their upgrade trigger, open questions, tests |

## 3. Build the page

- Copy `example.html`'s `<title>`, meta, font links and the whole `<style>` block verbatim. Write a new `<main>`, reusing its classes. Keep the tokens: they're what the user asked for.
- **Diagrams are hand-drawn inline SVG** using the stylesheet's classes:
  - Shapes: `.node`, `.node-acc`, `.inner`, and `.st-waiting`, `.st-notified`, `.st-seated`, `.st-noshow`, `.st-cancel` for states.
  - Lines: `.edge`, `.edge-acc`, `.edge-ret`, and `.life` for lifelines.
  - Text: `.t-title`, `.t-sub`, `.t-lbl`, `.t-mono`, `.t-acc`.
  - Rename the state classes to fit the subject's statuses, and keep the same five semantic tokens.
- Each SVG defines its own markers with ids unique on the page, as in the example. Wrap each in `.fig-scroll`, give it a `min-width`, `role="img"`, and an `aria-label` that states the claim, then a `figcaption`.
- Label every arrow with what moves (`POST /entries`, `NOTIFY on commit`). Use the accent only for the one mechanism the design hinges on. Dashed lines are responses.
- Lay out on a grid. Fix column x positions and box widths first, leave about 96px gaps for arrow labels, and step sequence messages every 32–34px. Start a sequence label 8px right of the arrow's left end.
- **Copy:** plain words, active voice, real names from the code. Every trade-off says what it costs. A shortcut says its limit and when to upgrade. Errors name their stable `code`.
- Done when every included section is filled, every arrow is labelled, and every open question from step 1 appears under "Open questions".

## 4. Publish and report

- Follow the Artifact tool's contract (quickstart for a plain page). Write the page to the scratchpad as `<subject>-design.html`. Publish it with a one-word `icon` and a one-sentence `description`. Republish to the same path for changes.
- Reply with the link, the 3–5 core decisions in plain language, any convention change, and the open questions. End with what you didn't check, such as no preview at phone width or in dark mode.
