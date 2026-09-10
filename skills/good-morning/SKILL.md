---
name: good-morning
description: Morning orientation that recaps recent work and helps decide what to work on. Trigger this skill whenever the user says "good morning", "morning", "let's get to work", "ready to start", "start my day", "what should I work on?", or anything that signals they're beginning a new work session. Use it proactively — if a user opens with a greeting at the start of a session, run this skill before doing anything else.
---

# Good Morning

This skill orients a new session by reading recent logs, surfacing what is still open, and helping the user decide what to work on.

## Language

Follow the `## Language` block in `CLAUDE.md`. Do not restate or reinterpret the rule here.

## Step 1 — Read the workspace

Read these before saying anything:

1. **CLAUDE.md** — who the user is, the language default, the active projects, the conventions. If it doesn't exist, `vault-setup` hasn't run — say so and offer to run it instead of continuing.
2. **MEMORY.md** — current state carried across sessions. Short, and the most decision-relevant thing in the vault.
3. **The last 3 daily logs** — in `01 Daily Logs/`, by date, newest first. These are Claude's previous session notes.
4. **Every active project's hub** — one for each row in `## Active Projects`. The table links by filename, not path, so find each file by name (`PROJ <Name> Overview.md`) rather than assuming a folder: projects may live under `02 Projects/` or, if the vault has one, `03 Life/Projects/`. Basenames are unique, so the search resolves to exactly one file. Read its `## Open Problems` and `## Key Files`.

If logs or hubs don't exist yet, that's fine — a new vault is allowed to be empty. Work with what's there.

## Step 2 — Brief, open items first

Lead with what is still open. That is the only part of the briefing that needs a decision today; finished work is context, and context goes second.

Each daily log may hold **several `## [Project Name]` sections** — one per conversation that day. Read all of them, not just the first.

**Carried forward.** Walk the logs newest first and collect every `### Still Open` item and every `### Start Here Next Time` line that a later log hasn't already resolved. Fold in anything in `MEMORY.md`. Group by project, one line each. If an item has been carried for several days, say how long — an item open for a week is a different fact from one opened yesterday, and saying so out loud is the point.

**Recently finished.** Then, briefly, what actually got done. Two or three lines for the whole recap, not per project. They were there; they don't need it retold.

**Recommendation.** One sentence on what matters most today, based on what has been carried longest, what is blocking other things, and where the momentum is. Make a real call — don't hedge, and don't offer a menu. That's the next step's job.

Brand-new vault with no logs: skip straight to Step 3.

## Step 3 — Ask what they want to do

After the recap, ask:

> "Want to jump into a project, or start something new?"

### If they pick a project:

Show each active project with its open problems as options. Pull from the `## Open Problems` section of each project hub. Keep it scannable — one line per problem.

Ask them to pick a project and problem. Once they choose, read whatever additional context is needed and get to work.

### If they want something new:

Tell them to say "new project" and the new-project skill will walk them through it.

## Tone

Keep the morning briefing conversational and brief. The user is starting their day — they don't need a wall of text. Open items, a couple of lines of recap, one clear recommendation, then move into action.
