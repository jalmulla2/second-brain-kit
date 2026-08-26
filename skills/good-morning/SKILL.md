---
name: good-morning
description: Morning orientation that recaps recent work and helps decide what to work on. Trigger this skill whenever the user says "good morning", "morning", "let's get to work", "ready to start", "start my day", "what should I work on?", or anything that signals they're beginning a new work session. Use it proactively — if a user opens with a greeting at the start of a session, run this skill before doing anything else.
---

# Good Morning

This skill orients a new session by reading recent logs, recapping what happened, and helping the user decide what to work on.

## Step 1 — Read the workspace

Read these files before saying anything:

1. **CLAUDE.md** — the master context file. This tells you who the user is, what projects are active, and what skills are available. If it doesn't exist yet, the user hasn't run `vault-setup` — tell them and offer to run it now instead of continuing.
2. **Last 3 session logs** — find them in `01 Daily Logs/`, sorted by date, most recent first. These are Claude's previous session notes.
3. **All active project overviews** — for each project listed in CLAUDE.md, read its overview file. These contain the goal, why, and open problems for each project.

If the daily logs or project overviews don't exist yet (the user might be very new), that's fine — just work with what's there.

## Step 2 — Brief recap

Each daily log file may contain **multiple `## [Project Name]` sections** — one per project conversation. Read ALL sections in each log file, not just the first one.

Give the user a short morning briefing organised by project. Keep it tight — this is an orientation, not a report:

- For each project that has a log entry: what was worked on, and what's still open
- Flag anything urgent or time-sensitive at the top

Then immediately give your **recommendation** — one clear sentence on what seems most important to work on based on recency, open problems, and project momentum. Make a real call; don't hedge.

If there are no previous logs (brand new user), skip the recap and go straight to Step 3.

## Step 3 — Ask what they want to do

After the recap, ask:

> "Want to jump into a project, or start something new?"

### If they pick a project:

Show each active project with its open problems as options. Pull from the "Open Problems" section of each project overview. Keep it scannable — one line per problem.

Ask them to pick a project and problem. Once they choose, read whatever additional context is needed and get to work.

### If they want something new:

Tell them to say "new project" and the new-project skill will walk them through it.

## Tone

Keep the morning briefing conversational and brief. The user is starting their day — they don't need a wall of text. Punchy bullets, one clear recommendation, then move into action.
