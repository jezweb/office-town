# Office Town

A deployable template for running an AI agent fleet using **Goose** with a structured, business-shaped methodology.

Office Town gives a small business (or any team) a vocabulary, a folder structure, and a set of role briefings that let them run a coordinated team of AI agents the way they'd run a small office — a Library, a Workshop, a Lookout, and so on. Each building is a workplace; each role is a specialist; the user delegates to roles and the roles do the work.

## Who this is for

- Small and medium businesses adopting AI agent fleets and wanting structure instead of a single mega-agent
- Teams using Goose (https://github.com/block/goose) who want a clear methodology layer on top
- Anyone who's tired of "an agent" and wants a *team* of agents that know where they live, who they are, and when to hand work off

## What's in this folder

```
office-town/
├── README.md           — this file
├── SETUP.md            — step-by-step deployment guide
├── METHODOLOGY.md      — the four primitives, vocabulary, conventions
├── LICENSE             — MIT
├── buildings/          — one folder per building (each is a Goose project)
│   ├── office/         — The Office — the boss, dispatch, user interface
│   ├── library/        — The Library — the librarian, knowledge curation
│   ├── workshop/       — The Workshop — the worker, execution
│   ├── lookout/        — The Lookout — the scout, outward scanning
│   └── post-office/    — The Post Office — comms + machine ops
├── roles/              — role definition files (Goose agents); copied to ~/.agents/agents/
├── playbooks/          — (TBD) recipes for common workflows
└── skills/             — (TBD) markdown skills agents can load
```

Each building folder has its own `.goosehints` (auto-loaded by Goose when working in that directory) plus the standard subfolders a role uses: `inbox/`, `journal/`, `findings/`, `facts/`.

## The four primitives

| Word | What it is | Goose primitive |
|---|---|---|
| **Town** | The whole environment | The Office Town installation |
| **Place** | A building, a workplace | A Goose project (working directory) |
| **Role** | An identity, a job description | A Goose agent (`.md` file) |
| **Task** | A piece of work being done | A Goose session / chat |

You **work at** a Place. You **delegate to** a Role. You **open** a Task. See `METHODOLOGY.md` for the full vocabulary.

## How to deploy this for a new team

See **[SETUP.md](SETUP.md)** for the step-by-step walkthrough. Should take under 15 minutes if Goose is already installed.

The short version:

1. Clone or copy this folder where you want the town to live
2. Install the role files (`cp roles/*.md ~/.agents/agents/`)
3. Open Goose at one of the buildings (`cd buildings/library && goose`)
4. Delegate to roles with `@-mention`

Customisation, troubleshooting, extension wiring, and multi-machine setup all in SETUP.md.

## Status

**v1 — usable.** Methodology, briefings, role files, and deployment guide are in place. The maintainer (Jezweb) is dogfooding before broader release. Playbooks and skills are TBD. Multi-machine / hosted-backend setup is documented but not yet automated. Expect refinements; the core structure is settled.

## Related

- Goose: https://github.com/block/goose
- Goosetown (Block's lightweight coordination layer): https://github.com/aaif-goose/goosetown
- Gas Town (Steve Yegge's full coordination framework): https://github.com/gastownhall/gastown

Office Town is a smaller, business-shaped sibling — same convergent shape (orchestrator, delegates, shared workspace), different vocabulary (business words instead of frontier-town themes).

## License

MIT — see [LICENSE](LICENSE). Copyright (c) 2026 Jezweb Pty Ltd. Permissive open license; use, modify, deploy as you see fit.
