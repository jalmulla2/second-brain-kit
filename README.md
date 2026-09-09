# second-brain-kit

A small Claude Code plugin that sets up a personal "vault" workspace: a `CLAUDE.md` and `MEMORY.md` that give every session context, plus skills for running your day.

Claude Code forgets everything between conversations. This plugin fixes that with plain markdown files in a folder you own — a context file Claude reads at the start of every session, a daily log it writes at the end of one, and a project folder structure both of those point at. No database, no sync service, no lock-in.

## What's inside

| Skill | What it does |
|---|---|
| `vault-setup` | One-time setup. Interviews you (name, language, what the vault is for) and creates `CLAUDE.md`, `MEMORY.md`, and the `01 Daily Logs/` / `02 Projects/` folders. |
| `good-morning` | Start-of-day orientation. Reads recent logs and active projects, recaps what happened, and recommends what to work on next. |
| `end-of-day` | Writes a short handoff log for the current session so the next session can pick up where you left off. |
| `new-project` | Interviews you about a new project and creates its folder, overview file, and an entry in `CLAUDE.md`. |

You can work in **Arabic or English** — pick during `vault-setup`. That choice controls the language Claude uses to talk to you and the language of everything it writes into your logs and project files. File names and folder names always stay in English so paths keep working the same way either way.

---

## Install

Everything below happens in the terminal, in Claude Code.

### 0. Prerequisite — Claude Code

If you don't have it yet:

```bash
npm install -g @anthropic-ai/claude-code
```

Check it's there:

```bash
claude --version
```

### 1. Pick the folder that will be your vault

The plugin writes into whatever directory Claude Code was started in, so decide this first. Make a folder anywhere you like — an existing Obsidian vault works well, since everything here is plain markdown:

```bash
mkdir -p ~/Vault
```

### 2. Start Claude Code in that folder

```bash
cd ~/Vault && claude
```

This matters more than it looks: `cd` into the vault every time. A session started somewhere else will read and write the wrong `CLAUDE.md`.

### 3. Add the marketplace

The simplest way is to copy this repo's link and paste it into the plugin menu.

At the Claude Code prompt, run:

```
/plugin
```

Choose the option to add a marketplace, then paste:

```
https://github.com/jalmulla2/second-brain-kit
```

That's the same URL as the address bar on this page — copy it from there if it's easier.

If you'd rather type one line than click through the menu, this does exactly the same thing:

```
/plugin marketplace add jalmulla2/second-brain-kit
```

Either way, Claude Code now knows where to find the plugin — it reads `.claude-plugin/marketplace.json` from this repo. Nothing is installed yet.

### 4. Install the plugin

Still in `/plugin`, pick **second-brain-kit** from the marketplace you just added and install it.

Or by command:

```
/plugin install second-brain-kit@second-brain-kit
```

The first `second-brain-kit` is the plugin, the second is the marketplace it came from — they happen to share a name.

### 5. Restart Claude Code

Skills are loaded at startup. Exit and start again from the same folder:

```
/exit
```

```bash
cd ~/Vault && claude
```

### 6. Verify

Run `/plugin` and confirm `second-brain-kit` shows as installed, or just say **"set up my vault"** — if the setup interview starts, you're good.

---

## Manual install (no marketplace fetch)

If you'd rather grab the files yourself — offline, behind a firewall, or you just prefer having the folder on disk — download the repo instead of adding it by name.

On GitHub, hit **Code → Download ZIP**, then unzip it. GitHub appends the branch name, so you get `second-brain-kit-main`:

```bash
unzip ~/Downloads/second-brain-kit-main.zip -d ~/
```

Or clone it, if you'd rather be able to `git pull` later:

```bash
git clone https://github.com/jalmulla2/second-brain-kit.git ~/second-brain-kit
```

Then start Claude Code in your vault and add the **folder** as the marketplace:

```
/plugin marketplace add ~/second-brain-kit-main
```

```
/plugin install second-brain-kit@second-brain-kit
```

`/exit`, relaunch with `cd ~/Vault && claude`, and you're in the same place as the normal install.

Three things to know:

- **The path must be the unzipped folder, not the `.zip`** — it needs to contain `.claude-plugin/marketplace.json`. If Finder unzipped it for you, that's `~/Downloads/second-brain-kit-main`.
- **Don't delete or move the folder afterwards.** The marketplace points at that directory; it isn't a copy. Put it somewhere permanent *before* you add it.
- **A ZIP install can't update itself.** `/plugin marketplace update` has nothing to pull from. To upgrade, download the ZIP again, replace the folder, and restart Claude Code. Clone instead if you want `git pull` to work.

### Or skip plugins entirely

The skills are just folders with a `SKILL.md` in them. Copying them into your personal skills directory works too:

```bash
cp -r ~/second-brain-kit-main/skills/* ~/.claude/skills/
```

Restart Claude Code and all four are available. You lose plugin-managed updates and `/plugin uninstall` — you'd add and remove these folders by hand.

---

## Getting started

Once installed, say:

```
set up my vault
```

That runs `vault-setup`. It asks three things — your name, Arabic or English, and one loose line about what the vault is for — then creates:

```
your-vault/
├── CLAUDE.md          ← read at the start of every session
├── MEMORY.md          ← current state that has to survive between sessions
├── 01 Daily Logs/     ← one file per day of work
└── 02 Projects/       ← one folder per project
```

Then say **"good morning"** to start your first real session, or **"new project"** to create your first project right away.

---

## Using it day to day

The whole thing is three moments in a day. You never call a skill by name — you just talk, and the phrasing triggers the right one.

### Morning — "good morning"

Say **"good morning"** (or "morning", "let's get to work", "what should I work on?") right after `cd ~/Vault && claude`.

Claude Code reads `CLAUDE.md`, your last three daily logs, and every active project overview, then gives you a short briefing grouped by project — what was worked on, what's still open — followed by one clear recommendation for what to do next. It then asks whether you want to jump into an existing project or start something new.

Do this *before* asking Claude for anything else. It's what loads the context.

### Whenever you start something new — "new project"

Say **"new project"** (or "start a project", "I want to work on something new").

Claude interviews you: name, goal, why it matters, what "done" looks like, and any problems you already know you'll hit. Then it creates:

```
02 Projects/<Project Name>/PROJ <Project Name> Overview.md
```

and adds a row to the `## Active Projects` table in `CLAUDE.md` — which is what makes `good-morning` aware of it tomorrow.

One check it will push back on: if you can't say what "done" looks like, it's an ongoing area of responsibility, not a project, and Claude will say so rather than create a folder that can never close.

### End of a session — usually automatic

`end-of-day` writes the handoff note. Most of the time you don't have to ask: **any session that actually changed something in the vault gets logged automatically** when the conversation closes. Sessions that only read or discussed things write nothing, and that's correct — a missing day means there was no work to hand off.

To write one explicitly, say **"end of day"** ("wrap up", "we're done", "done for the day") before you `/exit`.

Logs land in `01 Daily Logs/YYYY-MM-DD.md`, one `## [Project Name]` section per project you touched that day, each with what was worked on, what changed, what's still open, and where to pick up next time. Work past midnight and it still writes to the day the session started — anything before 06:00 goes to the previous day's file.

### A typical week

```bash
cd ~/Vault && claude
```

```
Monday    "good morning"        → recap + recommendation
          "new project"         → Client Onboarding created
          ...work...            → log written automatically on close

Tuesday   "good morning"        → picks up Client Onboarding from Monday's log
          ...work...
          "end of day"          → explicit log before you close the laptop

Wednesday "good morning"        → sees both days, recommends what's most urgent
```

---

## The files you own

| File | What it's for | Edit it yourself? |
|---|---|---|
| `CLAUDE.md` | Who you are, your language, the active-projects table, skills available. Read first in every session. | Yes — keep it short. It's read every time. |
| `MEMORY.md` | Current state only, not a history — open questions, pending decisions. Delete blocks when they stop being true. | Yes. |
| `01 Daily Logs/*.md` | Session handoff notes, written by `end-of-day`. | Rarely — let the skill own these. |
| `02 Projects/*/PROJ * Overview.md` | Goal, why, outcomes, open problems, and an optional `## Tasks` section. | Yes — this is the file to keep current. |

Tasks live inside each project's own overview. There is no central task list, by design.

Since it's all markdown in a folder, you can put the vault in git, open it in Obsidian, or sync it with anything that moves files.

---

## Updating and removing

Pull the latest version of the plugin:

```
/plugin marketplace update second-brain-kit
```

Then `/exit` and start Claude Code again. Your vault files are never touched by an update — the plugin only ships skills.

To remove it:

```
/plugin uninstall second-brain-kit@second-brain-kit
```

Your `CLAUDE.md`, logs, and projects stay exactly where they are.

---

## Troubleshooting

**"Nothing happens when I say good morning."**
The skills didn't load. `/exit`, start Claude Code again, then check `/plugin` shows `second-brain-kit` as installed.

**"Claude Code says it can't find CLAUDE.md."**
The session wasn't started in your vault folder, or setup never ran. Run `pwd` — if it isn't your vault, `/exit` and start again with `cd ~/Vault && claude`. If the path is right, say "set up my vault".

**"It created the files in the wrong place."**
Skills write relative to the directory Claude Code was started in. Move the files to your real vault folder and always launch with `cd ~/Vault && claude`.

**"good-morning doesn't see my project."**
It reads the `## Active Projects` table in `CLAUDE.md`. If the project folder exists but the row doesn't, add the row.

**"Setup ran twice."**
`vault-setup` checks for an existing `CLAUDE.md` and asks before overwriting. If you said yes by mistake, your logs and project folders are untouched — only `CLAUDE.md` and `MEMORY.md` were rewritten.

---

## License

MIT
