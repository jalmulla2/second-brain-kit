---
name: new-project
description: Creates a new project in the workspace with a pre-filled project hub file that links the project's files together. Trigger this skill whenever the user says "new project", "start a project", "create a project", "I want to work on something new", "add a project", or anything that signals they want to kick off a new project. Always interview the user before creating any files.
---

# New Project

This skill interviews the user about a new project, creates its folder and hub file, and registers the project in `CLAUDE.md` so Claude knows about it in future sessions.

## Language

Follow the `## Language` block in `CLAUDE.md`. Do not restate or reinterpret the rule here.

## Step 1 — Interview the user

**Ask one question per message.** Send the question, stop, and wait for the answer before sending the next one. Do not batch the questions and do not list them all up front. If the user answers several at once, confirm what you captured and resume from the first one they haven't answered.

Before you start, check whether `03 Life/` exists at the workspace root. If it does, question 6 applies; if it doesn't, skip it.

1. **Name** — what's the project called?
2. **Goal** — what is this trying to accomplish? One sentence is fine.
3. **Why** — why does this matter to you? The real reason.
4. **Done** — what does "done" look like? What will exist when this has succeeded?
5. **Open problems** — do you already know the main problems you'll have to solve? Fine if not.
6. **Work or personal?** — asked only if `03 Life/` exists. Work goes in `02 Projects/`, personal in `03 Life/Projects/`.

**Is it actually a project?** Ask this of yourself after question 4, not out loud. A project is time-bounded and has an end state — something that will be *finished*. If there is no answer to "what does done look like", it is an ongoing area of responsibility, not a project. Say so instead of creating a folder that can never close.

**Name collision.** Before creating anything, check that no `PROJ <Project Name> Overview.md` already exists anywhere in the vault. Two notes with the same basename make every `[[...]]` to them ambiguous. If it collides, tell the user and ask for a different name.

## Step 2 — Create the project folder and hub

Where it goes:

- work, or `03 Life/` doesn't exist → `02 Projects/<Project Name>/`
- personal → `03 Life/Projects/<Project Name>/` (create `03 Life/Projects/` if it isn't there yet)

The folder starts with one file:

```
<root>/<Project Name>/
└── PROJ <Project Name> Overview.md
```

This is the project's **hub**. It is what Claude reads first whenever the project comes up, and it is what ties the project's files together. The `PROJ` prefix makes it identifiable; never rename it to start with `[`.

Write the prose — goal, why, outcomes, problems — in the conversation's current language. Keep the frontmatter keys, section headings, folder name and the `PROJ ... Overview.md` filename pattern in English regardless.

Use this exact template:

```markdown
---
title: <Project Name> Overview
description: <one sentence: what this project is and what finishing it looks like>
author: claude
type: overview
date: YYYY-MM-DD
update date: YYYY-MM-DD
status: active
tags: []
---

## Goal
<their goal answer>

## Why
<their why answer>

## Tangible Outcomes
- <outcome 1>
- <outcome 2>

## Open Problems
1. <problem>
<or, if they had none: 1. (to be defined — we'll work these out as we go)>

## Key Files
| File | Purpose |
|------|---------|

_No files yet — a row is added here every time a file is created in this project folder._

## Links Out
_Nothing yet._
```

Two things about that template:

- **No `project` key.** Every *other* file in the folder points at this hub with `project: "[[PROJ <Project Name> Overview]]"`, which is how the hub collects them as backlinks. The hub does not point at itself.
- **`## Key Files` is created empty on purpose, and does not stay empty.** It fills as the project is worked on. The italic placeholder line is deleted when the first row lands.

### Keeping `## Key Files` current

Empty at birth is correct. Empty at week three means the mechanism failed. Two triggers keep it honest:

1. **At creation time — the real one.** The rule in `## Project Files` of `CLAUDE.md` applies to *any* file created in a project folder, by any skill or by ordinary work with no skill involved: set the `project` frontmatter and add the row in the same action that creates the file. Row format:

   `| [[Filename Without Extension]] | <short phrase: what this file is for> |`

   The Purpose column is one phrase, not a sentence, and says why the file exists rather than restating its name.

2. **At end of session — the safety net.** `end-of-day` walks the files it is about to list under "What Was Built or Changed" and adds any row or `project` key that was missed. It never rewrites rows that already exist.

### `## Links Out`

A labelled list of deliberate relationships to notes *outside* this project folder — a related project's hub, a reference note, a standing document. Label every entry with what the relationship is; a bare link nobody can interpret isn't worth the line.

```markdown
## Links Out
- Depends on: [[PROJ Other Project Overview]]
- Background: [[Some Reference Note]]
- Vendor documentation — https://example.com/docs
```

Wikilinks here are filename-only, like everywhere else. External URLs are fine — they aren't wikilinks. Add an entry when a real relationship exists; never invent one to fill the section.

## Step 3 — Update CLAUDE.md

This step is critical — if you don't do it, `good-morning` and future sessions won't know this project exists.

Open `CLAUDE.md` in the workspace root. Find the `## Active Projects` section. It uses a pipe table format:

```markdown
| Project | Key File |
|---------|----------|
| <Project Name> | [[PROJ <Project Name> Overview]] |
```

Add the new project as a new row. The row is identical whether the project sits in `02 Projects/` or `03 Life/Projects/` — the link is filename-only, so the table survives a project being moved between roots. That is the point.

Do not change the table structure or add new columns, and don't add a folder listing elsewhere in `CLAUDE.md` — a stale map is worse than no map, and Claude can read the file system directly when it needs to.

## Step 4 — Confirm and offer to dive in

Tell the user:
- The project folder and hub file are created, and where
- `CLAUDE.md` has been updated so Claude will remember this project
- Ask: "Want to dive into one of the open problems right now, or save it for later?"

Keep it short — they're ready to work.
