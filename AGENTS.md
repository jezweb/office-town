# Office Town — town-level guide

This file is auto-loaded by your agent host when working at the town root. Each building has its own `AGENTS.md` with role-specific context.

## What this is

A structured AI agent fleet running on Goose. Four core roles in four buildings:

- **The Office** (`@boss`) — your primary contact; routes work
- **The Library** (`@librarian`) — extracts from external systems + curates the wiki
- **The Workshop** (`@worker`) — deep work, execution
- **The Lookout** (`@scout`) — outward scanning

Plus the wiki (`buildin../../wiki/`) — the team's shared knowledge layer.

## The four primitives

| Word | What it is |
|---|---|
| **Town** | The whole environment — this folder + Goose installation |
| **Place** | A building. Each has its own `AGENTS.md` and folders for inbox/journal/findings/facts |
| **Role** | An identity — defined in `roles/<name>.md`, invokable via `@-mention` |
| **Task** | A piece of work — a Goose chat / session |

## Default behaviours expected of all roles

- Read your building's `AGENTS.md` and your role's `.md` file at session start
- Read recent entries in `inbox/`, `journal/`, `findings/`, `facts/` to pick up the trail
- Update today's `journal/<YYYY-MM-DD>.md` as you work
- Drop pattern-shaped output into `findings/` for the librarian to graduate
- Delegate to siblings via `@-mention` when work falls outside your room

## On vocabulary

We use "Office Town" words by convention (place, role, task, building, librarian, etc.). When a reader might need the underlying Goose primitive, bracket it on first mention: "the **task** (chat)", "the **briefing** (`AGENTS.md`)". See `METHODOLOGY.md` for the full vocabulary.

## What's important

The town runs on markdown files. Source of truth is on disk. Indexes and views may be derived (by workers, by the Office Town Cloud backend) but the files are primary. Edit them, version them, sync them across machines — they're your knowledge.

For deployment, customisation, and additional roles, see `README.md` and `SETUP.md`.
