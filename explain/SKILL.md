---
name: explain
description: Explain code, a system, a concept, or a change as an elegant single-page HTML explainer, with diagrams where they help and annotated critical lines of code, architecture, technologies, libraries, and flows. Use when the user runs /explain, or asks to explain, walk through, break down, or document how something works and wants it visual or as a page.
---

# Explain

Turn "explain X" into one polished HTML page the user can read and share.

## 1. Understand before writing

- Pin the subject: a file, module, repo, PR/diff, concept, or flow. If the
  request is vague ("explain this"), use the code in front of you: the open
  file, the current diff, or the repo root.
- Read the real code. Every claim, line number, and library name on the page
  comes from what you read, never from memory. Quote code verbatim.
- Find the few things that matter: the entry points, the core data structures,
  the 3–8 lines that carry the logic, and the places where it is easy to break.

## 2. Pick the sections

Start from [template.html](template.html). Keep only the sections that earn a
place for this subject; delete the rest (a concept needs no "Critical code",
a one-file script needs no architecture diagram).

| Section | Include when | Content |
|---|---|---|
| TL;DR | always | 2–3 sentences: what it is, what it does, why it exists |
| Architecture | more than one component | Mermaid `flowchart` of components and how they connect, then 1 line per component |
| Flows | a request, event, or job moves through steps | Mermaid `sequenceDiagram` or `flowchart`, then the steps as a numbered list |
| Critical code | there is code | 2–6 snippets, each with `file:line` and callouts explaining *why* each key line matters |
| Technologies & libraries | there are dependencies | cards: name, version if known, role here, why it was chosen or what it replaces |
| Data model | there is persistent or shared state | Mermaid `erDiagram` or `classDiagram` |
| Gotchas | always for code | edge cases, invariants, things that break if changed |
| Next steps | optional | where to read next, as `file:line` references |

## 3. Diagrams

Add a diagram only when it shows something a paragraph can't: structure,
order, or state. Use Mermaid (already wired in the template): put the source
in `<pre class="mermaid">`. Keep each diagram under ~15 nodes, label edges
with what flows across them, and use the real names from the code.
Escape `<`, `>` and `&` inside Mermaid labels.

## 4. Critical lines

For each snippet: the shortest excerpt that shows the idea (trim with `// …`),
the real starting line in `data-start`, and a callout per important line in
the `ol.callouts` list (`<b>L42</b>` then the why, not a restatement of the
code). HTML-escape the code.

## 5. Write and publish

- Write the filled page to the scratchpad (or `./explain-<slug>.html` if no
  scratchpad), keeping the template's style and scripts.
- If the Artifact tool is available, follow its rules (load `artifact-design`
  first) and publish the file with `icon: "book"`; give the user the link.
  Otherwise run `open <file>` on macOS (`xdg-open` on Linux) and give the path.
- In chat, reply with the TL;DR and the link or path only; the page holds the
  explanation.

## Style rules

- Plain, direct language. Define a term the first time it appears.
- Explain *why*, not just *what*.
- Prefer one diagram plus a short list over long prose.
- Never invent APIs, versions, or behaviour you didn't see; say "not checked"
  instead.
