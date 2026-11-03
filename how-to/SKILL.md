---
name: how-to
description: Teach how to implement something as a beginner-friendly, step-by-step HTML guide published as an Artifact, with diagrams where they help, complete copyable code for every step, and plain-language explanations of each concept and library. Use when the user asks "how do I implement / build / add / set up X", wants a tutorial, walkthrough or step-by-step guide for building something, or runs /how-to, even if they don't mention a page or artifact. Not for explaining code that already exists (use explain) or for making the change in the repo (use implement).
---

# How-to

Turn "how do I build X?" into one page a newcomer can follow from top to bottom and end up
with working code. The reader should come away knowing what to type and also why each piece
exists, so they can adapt it rather than copy it blindly.

## 1. Pin the goal and the reader's starting point

- Restate the goal as an end result: "a FastAPI app where `/login` returns a JWT and
  `/menu-items` rejects requests without one", not "JWT auth".
- **Inside a project?** Read the files the feature will touch (entry point, config,
  dependency file, an existing module to copy the style from). Then write the guide for
  *this* codebase: its folders, naming, test setup and existing helpers. A guide that fits the
  user's repo is worth far more than a generic tutorial.
- **No project context?** Pick the most common, current stack for the job and state the
  assumption in the "You'll need" line.
- Ask the user only when the choice changes the whole guide (e.g. "your own login or Google
  sign-in?"). Otherwise pick a sensible default and name the alternative in "Where to go next".

## 2. Check facts before teaching them

Beginners copy what they read, so a wrong version or a deprecated library becomes their bug.

- **Pick the version to teach, in this order:**
  1. The version the user names ("using Pydantic v1", "Django 4.2").
  2. The version the project already uses: read it from the lockfile (`uv.lock`,
     `poetry.lock`, `package-lock.json`, `pnpm-lock.yaml`, …) or the installed package, not
     only the manifest's range.
  3. Otherwise, the latest stable release. Look it up (PyPI JSON API, `npm view`, crates.io,
     …); don't rely on memory.

  Write the version and the check date on the page.
- **Teach the current approach for that version.** Libraries change their recommended style
  between majors, and tutorials online lag behind. Use the idiom the library's own docs
  recommend for the chosen version, never an older pattern that still happens to work. For
  example: SQLAlchemy 2.x `Mapped[...]`/`mapped_column`, not the legacy `Column`; Pydantic v2
  `model_config`, not `class Config`; FastAPI `lifespan`, not `@app.on_event`. When in doubt,
  read that version's docs or the installed package source.
- **Don't mix versions.** Every snippet must work on the chosen version. Don't use an API that
  only exists in a newer release, and don't fall back to one the chosen version deprecates.
- **When a pinned version is old**, still write code for it, since that's what will actually
  run. Add a short note saying it's behind, what the newer approach looks like, and how to
  upgrade. Don't silently teach the latest API against an older pin; the reader's code would
  break.
- **Check that the library itself is still the recommended choice.** Ecosystems move (e.g.
  FastAPI's docs moved from python-jose/passlib to PyJWT/pwdlib). Prefer what the framework's
  own docs use today. If the project already uses a deprecated library, say so on the page
  and name the replacement, but don't swap it out unless the user asks.
- For any API you're not sure about, read the docs or the installed package source. If you
  can't check something, say "not checked" on the page rather than guessing.
- **Inside a project, dry-run the guide**: copy the repo to the scratchpad (skip `.venv`,
  `.git`, `node_modules`), follow your own steps there, and paste the *real* outputs into the
  page as checkpoints and "expected output" blocks. Run any Docker Compose stack under its own
  project name (`docker compose -p <name>`) and tear it down with `-v` afterwards. Never run the
  steps in the user's working tree. If the copy shows work the user has already started (stub
  files, new settings, new dependencies), build the guide from that state rather than from
  scratch.

## 3. Plan the steps

Break the build into 3–8 steps. Each step:

- adds **one** thing (a setting, a model, a function, a route, a test), in the order a person
  would actually type it, so the project still runs after every step;
- names the exact file and whether it's new or an edit to an existing file;
- ends with a **checkpoint**: a command to run and what they should see. Checkpoints are what
  turn a wall of code into something a beginner can debug: when step 4's checkpoint fails,
  they know the mistake is in step 4.

If more than 8 steps seem necessary, the goal is really two guides. Cover the core one and put
the rest under "Where to go next".

## 4. Write the page

Start from [template.html](template.html) and keep its styles and scripts. Keep a section only
when it helps with this goal, and delete the rest.

| Section | Contents |
|---|---|
| Header | The end result in one or two sentences, what they need first, table of contents |
| Big picture | One Mermaid `flowchart` of the moving parts and what passes between them, before any code |
| Concepts | One card per idea the reader must understand (e.g. "access token", "hashing"). Each card gets a 1–2 sentence definition plus an everyday analogy. Only concepts the steps use. |
| Libraries | One card per dependency: the version taught (and whether it comes from the user, the lockfile, or latest stable), the job it does here, why it beats the obvious alternative. Then the install command, pinned to that version. |
| Steps | The plan from §3: title, why it comes now, code, callouts, checkpoint |
| How it runs | A Mermaid `sequenceDiagram` of one real request/event moving through the finished code, then one line per arrow naming the function that handles it |
| Try it | Copyable commands that exercise the finished feature, including one that should fail (e.g. no token gives 401) |
| Mistakes to avoid | The 2–5 errors people actually make, especially security ones: what goes wrong, and the fix |
| Where to go next | Follow-ups and the alternatives you didn't pick, each with when you'd want it |

### Writing for a beginner

- Define every term the first time it appears, in plain words. If a sentence needs a term
  from a later section, reorder the sections.
- Explain the *why* in prose and callouts. The code already shows the *what*.
- Short paragraphs, active voice, "you". No "simply", "just" or "obviously": if it were
  obvious, they wouldn't be reading.

### Code

- **Complete and runnable**: the full file for new files. For edits to an existing file, show
  the changed function or block, with a sentence saying where it goes. No `...` hiding code
  the reader needs, and no placeholders except secrets, which come from env/config.
- **Consistent across steps**: the same names, imports and paths from step 1 to the end, so
  each step's snippet works with the previous ones.
- **Callouts**: 1–4 per snippet, on the lines that carry the idea or would surprise a
  beginner. `data-hot` and `<b>L…</b>` use line numbers within the snippet, starting at 1.
- HTML-escape code (`<` → `&lt;`, `>` → `&gt;`, `&` → `&amp;`). Use `class="plain"` on
  terminal commands (no line numbers). For command output, use a `figure class="code output"`
  with `class="plain nohl"` on the `code` element and no Copy button.
- When the guide targets the user's repo, follow its conventions (layering, typing, test
  style) so they could paste the code in as-is.

### Diagrams

Use a diagram only when it shows structure or order better than a paragraph would. Usually
that means the big picture and the runtime flow, plus a `stateDiagram-v2` or `erDiagram` when
the feature has states or tables. Keep each under ~12 nodes, use the same names as the code,
label every arrow with what crosses it, and escape `<`, `>` and `&` inside labels.

## 5. Publish

1. Follow the Artifact tool's rules for a new page (its quickstart and/or the
   `artifact-design` skill come before writing). The template already follows them: there's no
   `<html>`/`<head>`/`<body>` (the publisher adds those), Mermaid renders natively without a
   library, and syntax colours are inline tokens because external stylesheets other than
   Google Fonts are blocked. Keep it that way.
2. Write the page to the scratchpad directory as `how-to-<slug>.html` and publish it with
   `icon: "book"` and a one-sentence `description`.
3. If the Artifact tool isn't available, save the file and `open` it (`xdg-open` on Linux).
4. In chat, reply with 2–3 sentences (what the guide builds and the key choice you made)
   plus the link. The page holds the explanation, so don't repeat it in chat.
5. If you're inside a project, offer to implement it there for real.

## Before publishing, check

- Every step leaves the project runnable and has a checkpoint.
- Every library has a checked version, chosen in order: the user's version, then the lockfile, then latest stable. Every snippet uses that version's current idiom, and nothing deprecated is recommended.
- Every concept the code relies on is explained before the step that uses it.
- Names and imports match across all snippets.
- The page reads correctly in light and dark mode and at phone width (code scrolls inside its
  box, not the page).
