# Setting up Office Town

Step-by-step deployment guide. Should take under 15 minutes if Goose is already installed.

## Prerequisites

| Requirement | How to get it | Why |
|---|---|---|
| **Goose** (desktop or CLI) | https://github.com/block/goose/releases | Office Town runs on top of Goose's agent runtime |
| **A working LLM provider** (Anthropic / OpenAI / etc.) | Configure in Goose's settings | Goose talks to the LLM through your provider |
| **A directory to put the town in** | Just a folder; default `~/Documents/office-town/` | Where the buildings live |
| **Git** (optional but recommended) | Usually pre-installed | Versions the template; lets you customise without losing the upstream |

## Step 1 — Get the template

Clone, fork, or download `office-town/` to wherever you want the town to live.

```bash
git clone <office-town-repo-url> ~/Documents/office-town
cd ~/Documents/office-town
```

If you've copied it manually (no git), just place the folder where you want it and skip the git steps below.

## Step 2 — Install the core role files

Goose discovers agent files (the `@-mentionable` roles) from specific paths. Install the four core roles:

```bash
# Option A: copy them (simplest, fully independent)
mkdir -p ~/.agents/agents
cp ~/Documents/office-town/roles/*.md ~/.agents/agents/

# Option B: symlink (template edits flow through; useful while iterating)
mkdir -p ~/.agents/agents
for f in ~/Documents/office-town/roles/*.md; do
  [ "$(basename $f)" = "README.md" ] && continue
  ln -sf "$f" ~/.agents/agents/$(basename "$f")
done
```

After install, restart Goose. The `@-mention` autocomplete in any chat should now show: `@boss`, `@librarian`, `@worker`, `@scout`.

## Step 3 — Verify the briefings load

Open Goose. Use the Open Folder / project picker to navigate to one of the buildings. For example:

```
~/Documents/office-town/buildings/library
```

Start a new chat (task). Type something like:

> *"Who are you, where are you, and who lives next door?"*

If the briefing is loading correctly, the response will identify the building (the Library), the role (librarian), and the adjacent buildings (Office, Workshop, Lookout). If the response is generic ("I'm an AI assistant..."), the `AGENTS.md` file isn't being picked up — see Troubleshooting below.

## Step 4 — Try a real task

A first interaction that exercises the structure:

1. `cd ~/Documents/office-town/buildings/office` (or open in Goose's project picker)
2. Open a new task
3. Type: *"I'd like to set up a knowledge base for our team. Can you delegate the planning to the librarian and the implementation to the worker?"*
4. Watch the boss respond, then use `@librarian` and `@worker` to hand off
5. After the session, look in `buildings/office/journal/<today's date>.md` — boss should have logged what happened

If all that worked, your town is alive.

## Step 5 — Install additional role packs (optional)

If your team needs business / creative / technical roles, install the packs:

```bash
# Once role packs are published:
goose plugin install jezweb/office-town-pack-business
goose plugin install jezweb/office-town-pack-creative
# ... etc
```

Each pack adds new agents to `@-mention` autocomplete. Restart Goose after install.

## Step 6 — Customise (gradual)

The defaults are starting points, not opinions you have to keep.

### Customise the briefings

Each `buildings/<building>/AGENTS.md` can be edited per-deployment:

- Add specifics about your team's tools and conventions
- Add or remove standing orders
- Reference your own wiki schema if you build one

### Customise the role files

Each `roles/<role>.md` (and its installed copy in `~/.agents/agents/`) can be edited:

- Adjust voice / tone for your team
- Add team-specific scope
- Specify a model in frontmatter (e.g., `model: claude-sonnet-4-6`)
- Add or remove "What I do" / "What I don't do" items

If you symlinked the role files (Option B in Step 2), editing the template versions updates Goose's view immediately. If you copied (Option A), you'll need to copy again after edits.

### Add or rename buildings

```bash
mkdir -p ~/Documents/office-town/buildings/studio
mkdir -p ~/Documents/office-town/buildings/studio/{inbox,journal,findings,facts}
touch ~/Documents/office-town/buildings/studio/inbox/.gitkeep
```

Then write the briefing (`buildings/studio/AGENTS.md`) and the role file (`roles/designer.md`). Update other buildings' `AGENTS.md` to mention the new neighbour under "Adjacent buildings".

### Wire in extensions (MCP services)

The buildings reference "services wired in" — these are MCP extensions you configure in Goose's settings:

- Wiki / knowledge service (something to read/write your wiki content)
- Extractors for the librarian (email reader, CRM connector, web scraper, file extractor)
- Web search (for the scout)
- Domain-specific tools per role

Office Town is opinionated about *structure* and not at all about *which services* you pick. Wire what makes sense for your team. The Office Town Cloud backend (separately installable) provides Cloudflare-backed extensions for the common needs.

## Where your data lives

After setup, the per-deployment data accumulates in:

```
buildings/<building>/inbox/        — messages from other roles
buildings/<building>/journal/      — daily logs (YYYY-MM-DD.md)
buildings/<building>/findings/     — working papers
buildings/<building>/facts/        — stable facts held across sessions
buildings/library/wiki/            — the shared knowledge layer
```

These are gitignored in the template (`.gitignore`) so the template stays clean. Your own deployment can either:

- Keep them gitignored (your data isn't versioned)
- Remove them from `.gitignore` (your data IS versioned — useful if you want commit history of the town)

If you're running across multiple machines, consider how to sync this data: shared filesystem, dedicated sync daemon (goannad), or a central Goose server.

## Also using Claude Code on the same town?

`AGENTS.md` is the cross-tool standard — Goose, Cursor, Aider all read it natively. For Claude Code users on the same town, add a one-line `CLAUDE.md` to each building pointing at `AGENTS.md`:

```bash
for b in buildings/*/; do
  cat > "$b/CLAUDE.md" << 'EOF'
# CLAUDE.md
See @AGENTS.md for the actual content.
EOF
done
```

One file per building, one line of content. Both tools see the same context.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| `@librarian` (or other role) doesn't autocomplete | Role files not installed | Re-run Step 2; restart Goose |
| Briefing doesn't load (response is generic) | `AGENTS.md` not detected | Confirm working directory is exactly `buildings/<building>/`; restart Goose |
| Roles don't seem to know each other | Briefings reference roles that aren't installed | Make sure all four core role files are in `~/.agents/agents/` |
| "Provider not configured" | LLM not set up | Goose Settings → Providers; add your API key |

## What to do next

Once the town is running:

1. **Use it for a week.** See whether the structure helps or fights you
2. **Customise from real usage, not speculation.** Edit briefings and role files based on what you actually hit
3. **Add new buildings only when a real need surfaces** — empty buildings on the map are fine, premature ones aren't
4. **Document your variant.** If you've meaningfully diverged from the template, write up *why* in your fork's README

The methodology in `METHODOLOGY.md` doesn't change with use. The contents do.
