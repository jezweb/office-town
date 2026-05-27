# Office Town

**Office Town adds team-shaped capabilities to [Goose](https://block.github.io/goose/)** — four addressable roles, a Cloudflare-backed wiki, and a set of role packs for different kinds of work. Your Goose stops being a single mega-agent and starts acting like a team that knows the work.

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

You need Goose installed first — grab it from https://block.github.io/goose/ (Desktop or CLI both work). Once you have Goose:

👉 **[Open INSTALL.md](./INSTALL.md)** — paste one prompt into any capable AI agent (Goose itself, Claude Code, Aider, etc.). The agent checks your toolchain, asks before installing anything missing, deploys the Cloudflare backend, wires the plugin into Goose, sets up your first town. ~20-30 minutes total.

Plus a **Connect-second-machine** helper for laptop+desktop setups.

Or [officetown.au](https://officetown.au) shows the same prompt in copy-friendly form.

For a manual step-by-step path (no AI agent assisting the install), see [SETUP.md](./SETUP.md).

## What this is

A **methodology layer over Goose**. The methodology shape:

| Primitive | Job |
|---|---|
| **Town** | Your installation — a folder that defines who, what, where |
| **Place** (Building) | A workplace — Office, Library, Workshop, Lookout |
| **Role** | A specialist Goose agent — addressable by name |
| **Task** | A discrete unit of work — routed from boss to a specialist |

The **wiki MCP** runs on Cloudflare Workers. It's the team's shared memory — entries have a universal sextet of frontmatter fields (`slug`, `kind`, `created`, `last_updated`, `last_edited_by`, `last_change_summary`) plus collection-specific fields. Hybrid search (FTS5 + Vectorize) means agents find things by keyword AND by meaning.

Deep dive: [METHODOLOGY.md](./METHODOLOGY.md).

## What you get after install

- 4 addressable Goose roles: `@boss`, `@librarian`, `@worker`, `@scout`
- Wiki MCP (FTS5 + Vectorize hybrid search, 11 default collections)
- Files MCP (R2-backed, signed share links)
- Publish MCP (markdown → public page)
- Cron routines (Goose Desktop poll model — runs while you're at your desk)
- Web dashboard with town map + kanban + recently-updated
- Pluggable role-pack catalogue: startup, design, hosting, wordpress, business, comms, cloudflare, knowledge
- Cost: ~$2-5/month on Cloudflare at typical SMB volume

## Repo layout

```
office-town/
├── README.md           ← you are here
├── INSTALL.md          ← agent-paste install prompts (primary path)
├── SETUP.md            ← manual fallback (deterministic, no agent)
├── METHODOLOGY.md      ← the why and how
├── AGENTS.md           ← town-level standing orders (Goose auto-loads)
├── buildings/          ← 4 building folders with AGENTS.md each
├── roles/              ← core role definitions
└── LICENSE             ← MIT
```

## Related repos

| Repo | What |
|---|---|
| [office-town-cloud](https://github.com/jezweb/office-town-cloud) | Cloudflare Workers backend (wiki/files/publish/cron MCPs) |
| [office-town-plugin](https://github.com/jezweb/office-town-plugin) | Goose plugin (roles + skills + recipes + hooks) |
| [office-town-pack-*](https://github.com/jezweb?tab=repositories&q=office-town-pack) | Role packs: startup, design, hosting, wordpress, business, cloudflare, comms, knowledge |

## Why Goose

[Goose](https://block.github.io/goose/) is the agent runtime we built this on. It's open-source (Apache 2.0), runs locally with whichever LLM provider you have keys for, ships with MCP support out of the box, and has an active community improving it. Office Town adds a methodology layer + cloud substrate; Goose does the actual agent work.

If a different open-source agent runtime catches up with the right primitives (project-scoped agents, MCP, plugin systems), Office Town's plugin content is largely Open Plugin Spec compliant and could port. For v1.0 we ship for Goose.

## Licence

MIT. Use, modify, redistribute. © 2026 Jezweb Pty Ltd.
