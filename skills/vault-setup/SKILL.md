---
name: vault-setup
description: One-time first-run setup that creates a personal vault workspace — CLAUDE.md, MEMORY.md, and the daily-log/project folders — and asks the user which language they want to work in. Trigger whenever the user installs this plugin for the first time and says things like "set up my vault", "get started", "initial setup", "set this up for me", or when good-morning / end-of-day / new-project can't find a CLAUDE.md and the user needs onboarding first.
---

# Vault Setup

This skill runs once, the first time someone uses this plugin. It interviews the user briefly, then scaffolds the files and folders that `good-morning`, `end-of-day`, and `new-project` all depend on.

If `CLAUDE.md` already exists in the workspace root, tell the user setup already ran and ask if they want to redo it before touching anything — don't silently overwrite an existing vault.

## Step 1 — Interview

Ask these in a single conversational message, not a form:

1. **Name** — what should Claude call them?
2. **Language** — do they want to work in **Arabic** or **English**? This decides the language Claude uses to talk to them and the language of everything Claude writes into daily logs and project overviews going forward. It does **not** change folder names, file names, or frontmatter keys — those stay in English so paths stay predictable and skills keep working the same way regardless of language.
3. **What's this vault for?** — one loose line is enough (e.g. "freelance work and personal projects"). Used only as flavor text in CLAUDE.md; skip if they don't have an answer yet.

Wait for their answers before creating anything.

## Step 2 — Create CLAUDE.md

Create `CLAUDE.md` at the workspace root:

```markdown
# <Name>'s Vault

## About
<their one-line answer, or "Personal workspace for projects and daily work." if they skipped it>

## Language
Talk to <Name> in <Arabic | English>. Write all generated content — daily logs, project overviews — in <Arabic | English> too. Folder names, file names, and frontmatter keys always stay in English, regardless of this setting.

## Active Projects
| Project | Key File |
|---------|----------|

## Skills Available
- **good-morning** — start-of-day orientation: recaps recent logs and active projects, helps decide what to work on
- **end-of-day** — writes a handoff log entry for the current session
- **new-project** — interviews the user and creates a new project folder + overview
```

Keep CLAUDE.md short — this is the file every future session reads first. Don't add sections beyond what's above.

## Step 3 — Create MEMORY.md

Create `MEMORY.md` at the workspace root:

```markdown
# Memory

This file holds **current state**, not a log of events — that's what the daily logs are for. Each active project gets a short block here only if it needs to carry information across sessions that isn't already captured in its own overview file (open questions, decisions pending, things to remember next time). Remove a project's block when the project closes or the state is no longer relevant.

Nothing here yet.
```

## Step 4 — Create folders

Create, relative to the workspace root:

- `01 Daily Logs/` — one file per day of work, written by `end-of-day`
- `02 Projects/` — one subfolder per project, created by `new-project`

A `.gitkeep` or empty placeholder isn't necessary — the folders can just exist empty until the other skills write into them.

## Step 5 — Confirm

Tell the user, briefly:
- CLAUDE.md and MEMORY.md are set up
- Their language preference is saved
- The next step is to say **"good morning"** to start their first session, or **"new project"** to create their first project right away

Keep it short — they're ready to go.
