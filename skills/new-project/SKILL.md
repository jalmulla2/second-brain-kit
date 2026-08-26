---
name: new-project
description: Creates a new project in the workspace with a pre-filled project overview. Trigger this skill whenever the user says "new project", "start a project", "create a project", "I want to work on something new", "add a project", or anything that signals they want to kick off a new project. Always interview the user before creating any files.
---

# New Project

This skill interviews the user about a new project, creates a folder and project overview, and registers the project in CLAUDE.md so Claude knows about it in future sessions.

## Step 1 — Interview the user

Before creating anything, ask these questions in a single conversational message. Don't make it feel like a form — keep it natural:

1. **Name** — What's the project called?
2. **Goal** — What is this project trying to accomplish? (one sentence is fine)
3. **Why** — Why does this project matter to you? What's the real reason you're doing it?
4. **Tangible Outcomes** — What does "done" look like? What will exist when this project is successful?
5. **Open Problems** — Do you already know the main problems you'll need to solve? (totally fine if they don't know yet)

Wait for their answers before doing anything else.

**Is it actually a project?** A project is time-bounded and has an end state — something that will be *finished*. If there is no answer to "what does done look like", it is an ongoing area of responsibility, not a project. Say so instead of creating a project that can never close.

## Step 2 — Create the project folder and overview

Every project lives under one flat folder, relative to the workspace root:

```
02 Projects/<Project Name>/
└── PROJ <Project Name> Overview.md
```

The overview file is the most important file in any project — it's what Claude reads first every time the project comes up. Prefix it with `PROJ` so it's easy to identify.

Write the overview content — the goal, why, outcomes, and open problems — in whichever language `CLAUDE.md` specifies under `## Language`. Keep the frontmatter keys, the folder name, and the `PROJ ... Overview.md` filename pattern in English regardless.

Use this exact template:

```markdown
---
title: <Project Name> Overview
description: <one sentence summary of the project goal>
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
- <outcome 3>

## Open Problems
<if they gave problems, list them numbered: 1. Problem one>
<if they didn't, write: 1. (to be defined — we'll figure these out as we go)>
```

## Step 3 — Update CLAUDE.md

This step is critical — if you don't do this, the good-morning skill and future sessions won't know this project exists.

Open CLAUDE.md in the workspace root. Find the `## Active Projects` section. It uses a pipe table format:

```markdown
| Project | Key File |
|---------|----------|
| <Project Name> | [[PROJ <Project Name> Overview]] |
```

Add the new project as a new row in that table. Do not change the table structure or add new columns, and don't add a folder listing elsewhere in CLAUDE.md — a stale map is worse than no map, and Claude can read the file system directly when it needs to.

## Step 4 — Confirm and offer to dive in

Tell the user:
- The project folder and overview are created
- CLAUDE.md has been updated so Claude will remember this project
- Ask: "Want to dive into one of the open problems right now, or save it for later?"

Keep it short — they're ready to work.
