---
name: technical-writing
description: Use when authoring or revising a Markdown documentation file (README, how-to, runbook, tutorial, reference, architecture doc, ADR, CONTRIBUTING, changelog) or a multiline doc comment (JSDoc/TSDoc, Python docstring, Javadoc, C# XML doc, Go doc comment) for web services, cloud infrastructure, APIs, libraries, or developer guidance.
---

# Technical Writing

## Overview

Developer documentation fails in two ways: it states things that aren't true, or it answers a question the reader didn't ask. Style matters less than either. Every sentence must be traceable to a source, and every document must serve one reader doing one kind of task.

**Brevity is the soul of wit.** Say it once, in as few words as carry the meaning, and stop. Where clarity is needed, reach for an example (a command, a request/response, a code snippet, a before/after) before a paragraph of prose.

Default style authority: the **Google developer documentation style guide**, then the **Microsoft Writing Style Guide**. Conventions already present in the repo override both. Full source list with when to consult each: [references/sources.md](references/sources.md).

## Workflow

1. **Establish the brief.** Before drafting, know three things. Ask the user for any you can't determine from context:
   - **Reader** — who opens this, and what do they already know? (new contributor, on-call engineer, API consumer, maintainer)
   - **Doc type** — exactly one from the table below.
   - **House conventions** — look for existing docs, `CONTRIBUTING.md`, `.markdownlint*`, `.vale.ini`, docstring style in neighboring files. Match them.
2. **Gather facts from sources.** Read the code, config, IaC, CLI `--help`, and existing docs. Each claim in the draft traces to one of: code you read, something the user told you, or a cited external doc.
3. **Sort the gaps.** A gap is *blocking* if its answer would change the doc's sections, step order, or architecture (e.g., where a credential lives, which component clients call). If any gap is blocking, send the user your numbered questions now and draft after they answer. If the remaining gaps are details (a name, a default, a link), go straight to step 4.
4. **Draft** using the shape for the doc type. Write only confirmed behavior: a section whose behavior is unconfirmed gets its heading and a single `TODO(verify)`, with no prose or diagram describing a guess.
5. **Mark every unverified detail** inline as `TODO(verify): <question>` at the exact spot, then **list all of them in your reply to the user** as open questions. An assumption the user doesn't see in your reply is an invented fact.
6. **Self-review** against the checklist below.

## Doc types (Diátaxis)

| Type | Reader's need | Shape |
|---|---|---|
| Tutorial | Learning by doing | Guaranteed-to-work path from zero to a result; no choices, no edge cases |
| How-to / runbook | Completing a task | Goal, prerequisites, numbered steps each with one action and its expected result, verification, rollback |
| Reference | Looking something up | Consistent, complete, scannable: tables of params, env vars, endpoints, status codes, errors |
| Explanation | Understanding why | Context, design, trade-offs, alternatives considered (ADRs live here) |
| README | Deciding and starting | What it is, why it exists, quickstart, links out to the other types |

The shapes above, and any template from the repo or [references/sources.md](references/sources.md), list sections a doc *may* have. Include a section only when you have real content the reader needs for it; otherwise omit the section and its heading. (A section the reader needs but whose facts are unconfirmed gets a `TODO(verify)`, per the workflow.)

A document is one type. When content of another type is needed, give it its own section clearly headed as that type or its own file, and link to it. A runbook gets one short "How it works" paragraph, not a design essay.

## Doc comments

A doc comment states the **contract a caller can't see from the signature**:

1. Summary sentence: what it does, in caller terms (verb-first, ends with a period).
2. Behavior that matters to the caller: side effects, idempotency, concurrency, I/O, performance characteristics, units, ranges.
3. Each parameter's meaning and constraints — not its type when the language already declares it.
4. Return value, errors/exceptions thrown and when.
5. *Why* for any non-obvious design choice (one sentence).
6. A short example when usage isn't obvious.

Use the language's standard tag format (TSDoc `@param`/`@returns`/`@throws`, Google- or NumPy-style docstrings, Javadoc, C# `<summary>`, Go comments starting with the identifier name). Implementation detail belongs in a regular comment inside the function body.

## Style essentials

- Second person, present tense, active voice, imperative for steps ("Run…", not "You should run…").
- Sentence-case headings; one H1 per file; don't skip heading levels.
- Code, commands, file paths, env vars, and literal values in `code` formatting. Fenced code blocks always carry a language tag.
- Commands are copy-pasteable: placeholders as `<UPPER_SNAKE>` and explained right after the block; show expected output when it confirms success.
- Lead with the most important information. Supporting material (background, edge cases, full config listings, history) goes in an appendix at the end or in a separate linked document.
- Short sentences; one idea per paragraph.
- To clarify a point, show an example; a block of prose is the fallback when no example fits.
- Descriptive link text ("see the deployment guide", never "click here").
- Every image and diagram has alt text; prefer Mermaid for diagrams in Markdown.

## Self-review checklist

- [ ] Every claim traces to a source; every gap is a `TODO(verify)` listed in the reply.
- [ ] No prose or diagram describes behavior marked `TODO(verify)`.
- [ ] One doc type; other types are linked, not inlined.
- [ ] Reader can act on the first screen (README: quickstart; how-to: goal and prerequisites).
- [ ] Each section says its point once; supporting material sits in an appendix or a linked doc.
- [ ] Explanations lead with an example wherever one fits.
- [ ] Every command and code sample is complete and copy-pasteable.
- [ ] Doc comments describe the contract; signature types are not repeated.
- [ ] Matches house conventions found in step 1.

## Common mistakes

| Mistake | Fix |
|---|---|
| Inventing a plausible name, format, or flag to fill a gap | `TODO(verify)` inline + open question in the reply |
| Assumptions recorded only in an HTML comment in the file | Surface them in the reply to the user |
| Drafting a full doc around a blocking unknown, leaving a dozen TODOs | Ask the blocking questions first; draft after answers |
| Diagramming a guessed architecture, then flagging it | Heading + one `TODO(verify)`; diagram once confirmed |
| Runbook that's half design doc | One "How it works" paragraph; link to the explanation |
| Doc comment that narrates the algorithm | State the contract; move mechanics to body comments |
| Filling a template section with generic or padded content ("Rollback: no rollback needed", boilerplate "Troubleshooting") | Omit the section and its heading |
| Explaining a concept, then re-explaining it in a summary | Say it once; cut the restatement |
| Paragraphs describing a request, config, or output format | Show the actual request, config, or output |
| "Simply", "just", "easy", "obviously" | Delete; they insult readers who are stuck |
| Future tense and passive voice ("will be returned") | Present, active ("returns") |
