# second-brain-kit

A small Claude Code / Claude Cowork plugin that sets up a personal "vault" workspace: a `CLAUDE.md` and `MEMORY.md` that give every session context, plus three skills for running your day.

## What's inside

| Skill | What it does |
|---|---|
| `vault-setup` | One-time setup. Interviews you (name, language, what the vault is for) and creates `CLAUDE.md`, `MEMORY.md`, and the `01 Daily Logs/` / `02 Projects/` folders. |
| `good-morning` | Start-of-day orientation. Reads recent logs and active projects, recaps what happened, and recommends what to work on next. |
| `end-of-day` | Writes a short handoff log for the current session so the next session can pick up where you left off. |
| `new-project` | Interviews you about a new project and creates its folder, overview file, and an entry in `CLAUDE.md`. |

You can work in **Arabic or English** — pick during `vault-setup`. That choice controls the language Claude uses to talk to you and the language of everything it writes into your logs and project files. File names and folder names always stay in English so paths keep working the same way either way.

## Install

```
/plugin marketplace add jalmulla2/second-brain-kit
/plugin install second-brain-kit
```

## Getting started

Once installed, just say:

```
set up my vault
```

That runs `vault-setup`. After it finishes, say **"good morning"** to start your first real session, or **"new project"** to create your first project right away.

## License

MIT
