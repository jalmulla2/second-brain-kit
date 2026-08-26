---
name: end-of-day
description: Writes the daily log entry for a work session so the next Claude session can pick up where things left off. Trigger whenever the user says "end of day", "wrap up", "we're done", "that's it for today", "log today", "done for the day", "goodnight", "shutdown", or anything that signals they're finishing a work session. Also triggers automatically at the end of any session that changed something in the vault, and when a conversation is closing — silently write or update the daily log without asking the user. Sessions run past midnight, so the log date is not always today's date.
---

# End of Day

This skill writes a handoff note from the current session — something Claude can read at the start of the next session to quickly understand what happened and where to pick up.

## Why this matters

Without this log, the next Claude session starts with no memory of what you did today. This note is what bridges sessions together and keeps momentum going.

## The log is a byproduct, not a ritual

Do not wait for the end of the day. **Any session that changed something in the vault gets logged** — files created or edited, a decision taken, a plan advanced. Write the 2–4 lines when the work is done, not when the user remembers to ask.

A session that only read, searched or discussed writes nothing. Gaps in the log are correct: a log exists for days with vault work, and a missing day means there was none. Nothing to catch up on.

## Language

Write the log content — the bullet points under each section — in whichever language `CLAUDE.md` specifies under `## Language`. Keep the frontmatter keys, section headings, the `01 Daily Logs/` folder name, and the `YYYY-MM-DD.md` filename format in English regardless of that setting.

## Conversation-Close Behaviour (automatic, silent)

When this skill is triggered by a conversation closing (not an explicit user request), run silently:

1. Scan the conversation for meaningful work — if the conversation was purely exploratory chat with no real output or decisions, skip logging entirely.
2. Work out which day's log to write to (see "Which day" below), and check whether it exists.
3. If it **does not exist** — create it (full format below).
4. If it **does exist** — read the existing content first. If this conversation's project/topic is already covered, check whether there is new content to add (new tasks done, new files built, new open items). If so, append to the existing section or add a new section. If nothing new, skip.
5. Do NOT ask the user questions. Do NOT confirm with the user. Just write and close.

## Which day — the after-midnight rule

Work sessions sometimes cross midnight. Logging strictly by wall-clock date would split one continuous session across two files.

**If the current time is between 00:00 and 05:59, write to the *previous* calendar day's log.** From 06:00 onwards, use today's date.

```bash
# The log date, accounting for after-midnight sessions.
# Tries BSD date (macOS) first, falls back to GNU date (Linux).
if [ "$(date +%H)" -lt 06 ]; then
  date -v-1d +%F 2>/dev/null || date -d yesterday +%F
else
  date +%F
fi
```

Applies to both the filename and the `date:` frontmatter.

## Step 1 — Review the session

Look back through the conversation and pull out what matters:

- **What we worked on** — which tasks were tackled in this session
- **What was built or changed** — specific files created or edited, decisions made
- **Still open** — anything mid-flight, unresolved, or left for later
- **Start here next time** — 1–2 sentences on the best place to pick up next session

Write it like a note to a colleague taking over the shift — enough to orient them fast, not so much they have to read an essay. If nothing is clearly "still open," skip that section. Don't pad.

Note: Tasks live inside each project's own overview file under a `## Tasks` section, if it has one. There is no central task list — don't create or look for one.

## Step 2 — Save the log

Save to:

```
01 Daily Logs/YYYY-MM-DD.md
```

where `YYYY-MM-DD` is the log date from the after-midnight rule above — **not** blindly today's date.

Note: `01 Daily Logs/` is at the **root** of the workspace.

**Important:** the user may work on different projects in separate conversations throughout the day. The same day's log file may already exist. Always check first:

- **If the file does NOT exist** — create it with the full header and the project section.
- **If the file ALREADY exists** — open it and read it. Then:
  - If this project/topic **already has a section**, check for new content and append within that section if needed.
  - If this project/topic **does not have a section yet**, append a new `## [Project Name]` section to the bottom.
  - Never recreate the frontmatter or top-level heading.

Each project session gets its own clearly labeled `## [Project Name]` section so good-morning can scan all projects from a single file.

### Format for a NEW file:

```markdown
---
author: claude
type: log
date: YYYY-MM-DD
---

# Session Log — [Weekday, Month DD YYYY]

## [Project Name]

### What We Worked On
- [task — what was done]
- [task — what was done]

### What Was Built or Changed
- [specific file or decision]

### Still Open
- [thing that's mid-flight or unresolved]

### Start Here Next Time
[1–2 sentences on the best place to pick up next session]
```

### Format when APPENDING to an existing file:

Add only the project section — no new frontmatter or top-level heading:

```markdown

## [Project Name]

### What We Worked On
- [task — what was done]

### What Was Built or Changed
- [specific file or decision]

### Still Open
- [thing that's mid-flight or unresolved]

### Start Here Next Time
[1–2 sentences on the best place to pick up next session]
```

Only include "Still Open" if there's actually something unresolved. Don't invent open items.

## Step 3 — Confirm (explicit user request only)

If triggered by the user explicitly, tell them where the file was saved and give them the "Start Here Next Time" line so they know the handoff is solid. Keep it to one or two lines — they're done for the day.

If triggered automatically on conversation close, skip this step entirely.
