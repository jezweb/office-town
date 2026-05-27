# Office Town

**AI agents that work like a team.** Four buildings, four roles, a wiki-backed substrate. Your agents stop guessing — they know the work.

Office Town is a markdown methodology for AI agent fleets running on [Goose](https://github.com/block/goose) (or any Open Plugin Spec host) with a [Cloudflare Workers backend](https://github.com/jezweb/office-town-cloud).

```
your-town/
├── buildings/
│   ├── office/        # @boss — routes work, holds the thread
│   ├── library/       # @librarian — extracts + curates the wiki
│   ├── workshop/      # @worker — deep work
│   └── lookout/       # @scout — outward scanning
└── wiki/              # the shared substrate (11 default collections)
```

---

## Get started

**You already have a capable AI agent — Claude Code, Goose, OpenAI Codex, Aider, Cline, etc. Don't read setup docs. Tell your agent to install it.**

👉 [Open INSTALL.md](./INSTALL.md) — three prompts to paste into your agent:

1. **Full install** — deploys Cloudflare backend + sets up plugin + first town
2. **Cloud-only** — just deploy the backend, you wire up the rest
3. **Connect-existing** — wire a new machine to an existing town

Or visit [officetown.au](https://officetown.au) for the same prompt in a copy-button-shaped form.

If you don't have a capable agent (or want a deterministic manual path), see [SETUP.md](./SETUP.md).

## Or download Office Town Desktop

A pre-configured macOS .app — signed + notarised, no warnings:

```
https://download.officetown.au/mac
```

Then run the same agent-install prompt from inside it. The .app is rebranded Goose with our icon + bundle ID, so any agent on macOS understands what it's looking at.

## What you get after install

- 4 addressable role agents: `@boss`, `@librarian`, `@worker`, `@scout`
- A wiki MCP backed by FTS5 + Vectorize hybrid search
- Files MCP, publish MCP, cron routines, dashboard
- A pluggable role-pack catalogue: `pack-startup`, `pack-design`, `pack-hosting`, `pack-wordpress`, `pack-business`, `pack-cloudflare`, `pack-comms`, `pack-knowledge`
- Cost: ~$2-5/month on Cloudflare at typical SMB volume

## How it actually works

| Primitive | Job |
|---|---|
| **Town** | Your installation — a folder that defines who, what, where |
| **Place** (Building) | A workplace — Office, Library, Workshop, Lookout |
| **Role** | A specialist agent — addressable by name |
| **Task** | A discrete unit of work — routed from boss to a specialist |

Each role is a markdown file with an `AGENTS.md` that Goose loads on session start. The wiki is the shared memory — entries have a universal sextet of frontmatter fields (`slug`, `kind`, `created`, `last_updated`, `last_edited_by`, `last_change_summary`) plus collection-specific fields.

Deep dive: [METHODOLOGY.md](./METHODOLOGY.md).

## Repo layout

```
office-town/
├── README.md           ← you are here
├── INSTALL.md          ← agent-paste install prompts (primary path)
├── SETUP.md            ← manual fallback (deterministic, no agent needed)
├── METHODOLOGY.md      ← the why and how
├── AGENTS.md           ← town-level standing orders (Goose auto-loads)
├── buildings/          ← 4 building folders, each with its own AGENTS.md
├── roles/              ← core role definitions
└── LICENSE             ← MIT
```

## Related repos

| Repo | What |
|---|---|
| [office-town-cloud](https://github.com/jezweb/office-town-cloud) | Cloudflare Workers backend (wiki/files/publish/cron MCPs) |
| [office-town-plugin](https://github.com/jezweb/office-town-plugin) | Open Plugin Spec plugin (roles + skills + recipes + hooks) |
| [office-town-desktop](https://github.com/jezweb/office-town-desktop) | Custom Distribution of Goose Desktop (signed Mac .app) |
| [office-town-pack-*](https://github.com/jezweb?tab=repositories&q=office-town-pack) | Role packs: startup, design, hosting, wordpress, business, cloudflare, comms, knowledge |

## Licence

MIT. Use, modify, redistribute. © 2026 Jezweb Pty Ltd.
