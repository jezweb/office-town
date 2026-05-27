# Setting up Office Town — manual fallback

> **Most people should use [INSTALL.md](./INSTALL.md) instead** — paste two prompts into your AI agent, it does everything. This SETUP.md is the deterministic manual path for users without a capable agent, or who want to see every step explicitly.

If you do have an agent (Claude Code, Goose, Aider, Cline, Cursor, etc.), close this file and use [INSTALL.md](./INSTALL.md). It does this same flow in ~25 minutes with zero clicks beyond pasting the prompts.

---

## What you're installing

| Component | Where | Why |
|---|---|---|
| **Office Town Cloud** (5 Cloudflare Workers) | Your Cloudflare account | Wiki + files + publish + cron + MCPs |
| **Office Town template** (this folder cloned) | `~/Documents/my-town` or wherever | The 4 buildings + roles + AGENTS.md |
| **Office Town plugin** (agents/skills/recipes) | `~/.claude/` or `~/.config/goose/` | Your agent host needs these |
| **Knowledge starter pack** (17 portable concepts) | `<town>/wiki/knowledge/` | Seed the wiki with hard-won learnings |
| **4 MCP server wirings** | Your agent host config | Connects host → deployed workers |

End state: your agent host can `@-mention` the boss / librarian / worker / scout, who can read+write the wiki, and the wiki is persisted in your Cloudflare account.

## Total time

~25-40 minutes if everything goes smoothly. Mostly waiting for Cloudflare deploys.

## Cost

~$2-5 USD/month on Cloudflare at typical SMB volume. Most workers stay in the free tier; Vectorize queries + Workers AI embeddings are the variables.

---

## Prerequisites

### Tools

| Required | Version | Install if missing |
|---|---|---|
| git | any | usually pre-installed; `xcode-select --install` on macOS |
| Node | 20+ | `brew install node` (macOS) / nvm / fnm / volta |
| pnpm | 10+ | `corepack enable pnpm` (after Node is in place) |
| wrangler | 3+ | `npm install -g wrangler@latest` |
| an agent host | — | Claude Code or Goose; pick one |

### Cloudflare

- Account ID — dash.cloudflare.com → any account → right sidebar
- API token — dash.cloudflare.com → My Profile → API Tokens → Create Token. Permissions: Workers Scripts (Edit), Workers Routes (Edit), D1 (Edit), R2 (Edit Object + Bucket), Vectorize (Edit), Queues (Edit), Workers AI (Read), Account Settings (Read).

Stash in env vars for this session:

```bash
export CLOUDFLARE_ACCOUNT_ID=...
export CLOUDFLARE_API_TOKEN=...
```

Verify: `wrangler whoami`

---

## Step 1 — Deploy Office Town Cloud

```bash
git clone https://github.com/jezweb/office-town-cloud ~/src/office-town-cloud
cd ~/src/office-town-cloud
pnpm install  # workspace root is the repo root
```

Generate a 32-byte hex MCP bearer token — you'll need it several more times:

```bash
export MCP_BEARER_TOKEN=$(openssl rand -hex 32)
```

Create the D1 database, then patch the returned `database_id` into `packages/core/wrangler.jsonc`:

```bash
wrangler d1 create office-town-d1
# edit packages/core/wrangler.jsonc — set d1_databases[0].database_id

cd packages/core
wrangler d1 migrations apply office-town-d1 --remote
cd ../..
```

Create the R2 buckets:

```bash
wrangler r2 bucket create office-town-wiki
wrangler r2 bucket create office-town-wiki-preview
wrangler r2 bucket create office-town-files
wrangler r2 bucket create office-town-files-preview
```

Create the Vectorize index, then the metadata indexes BEFORE deploying any worker (Vectorize won't retroactively index existing vectors):

```bash
wrangler vectorize create office-town-vec --dimensions=768 --metric=cosine
wrangler vectorize create-metadata-index office-town-vec --property-name=collection --type=string
wrangler vectorize create-metadata-index office-town-vec --property-name=slug         --type=string
wrangler vectorize create-metadata-index office-town-vec --property-name=entry_id     --type=string
```

Create the queue:

```bash
wrangler queues create office-town-index
```

Set the bearer token as a secret on each worker, then deploy in order — core first, then the 4 MCPs (they service-bind to core):

```bash
for pkg in packages/core packages/mcp-wiki packages/mcp-browser packages/mcp-devops packages/mcp-email; do
  (cd "$pkg" && echo -n "$MCP_BEARER_TOKEN" | wrangler secret put MCP_BEARER_TOKEN && wrangler deploy)
done
```

Each deploy prints the `.workers.dev` URL. Record all 5.

Verify health:

```bash
for u in <core-url> <wiki-url> <browser-url> <devops-url> <email-url>; do
  curl -sS "$u/health" && echo
done
```

Each returns `{"status":"ok",...}`.

Verify the wiki API:

```bash
curl -H "Authorization: Bearer $MCP_BEARER_TOKEN" <core-url>/api/wiki/collections
```

Expect 11 collections.

## Step 2 — Set up the town folder

```bash
git clone https://github.com/jezweb/office-town ~/Documents/my-town
cd ~/Documents/my-town
ls buildings/   # should show: office library workshop lookout
```

## Step 3 — Install the plugin into your agent host

```bash
git clone https://github.com/jezweb/office-town-plugin ~/src/office-town-plugin
```

### Claude Code

```bash
mkdir -p ~/.claude/{agents,skills,commands,rules}
cp -r ~/src/office-town-plugin/agents/*   ~/.claude/agents/
cp -r ~/src/office-town-plugin/skills/*   ~/.claude/skills/
cp -r ~/src/office-town-plugin/commands/* ~/.claude/commands/
cp    ~/src/office-town-plugin/rules/*    ~/.claude/rules/

claude mcp add office-town-wiki    --transport http <core-url>/mcp/wiki \
  --header "Authorization: Bearer $MCP_BEARER_TOKEN"
claude mcp add office-town-browser --transport http <browser-url>/mcp \
  --header "Authorization: Bearer $MCP_BEARER_TOKEN"
claude mcp add office-town-devops  --transport http <devops-url>/mcp \
  --header "Authorization: Bearer $MCP_BEARER_TOKEN"
claude mcp add office-town-email   --transport http <email-url>/mcp \
  --header "Authorization: Bearer $MCP_BEARER_TOKEN"
```

### Goose

```bash
goose plugin install jezweb/office-town-plugin
goose plugin install jezweb/office-town-pack-knowledge

# Add the 4 MCP servers via `goose mcp add` or by editing ~/.config/goose/config.yaml.
```

## Step 4 — Seed the wiki with starter knowledge

```bash
git clone https://github.com/jezweb/office-town-pack-knowledge ~/src/office-town-pack-knowledge
mkdir -p ~/Documents/my-town/wiki/knowledge
cp -r ~/src/office-town-pack-knowledge/concepts/* ~/Documents/my-town/wiki/knowledge/
```

## Step 5 — Smoke test

From your agent host, call `wiki.create` with:

```json
{
  "collection": "contacts",
  "slug": "test-smoke",
  "frontmatter": { "name": "Test Person", "kind": "contact" },
  "body": "Created during initial Office Town install smoke test."
}
```

Then `wiki.search "Test Person"` should return the entry.

Open a chat and `@boss`. Ask "who's on this town?". The boss should respond coherently.

Optional cleanup: `wiki.delete contacts/test-smoke`.

## What you have now

```
~/Documents/my-town/
├── buildings/           ← 4 building folders with AGENTS.md each
│   ├── office/          ← @boss
│   ├── library/         ← @librarian
│   ├── workshop/        ← @worker
│   └── lookout/         ← @scout
├── wiki/                ← shared substrate (11 default collections)
│   ├── knowledge/       ← seeded with 17 portable concepts
│   └── contacts/, orgs/, projects/, decisions/, business/, owner/, team/, research/, feedback/, tasks/
└── AGENTS.md            ← town-level standing orders
```

Wiki entries are persisted in your Cloudflare account (R2 + D1 + Vectorize), accessed by your agents via the wiki MCP. Buildings hold local state (journal, findings, inbox).

## What's next

- Run the `office-town-setup` recipe (in `office-town-plugin/commands/`) — captures business identity, voice, team via a conversation with the boss
- Install a domain-specific role pack: pack-startup, pack-design, pack-hosting, pack-wordpress, pack-business, pack-comms, pack-cloudflare
- (Optional) register a `.town` domain (~$30/yr at Cloudflare) and wire `app.<yourbusiness>.town` to your core worker

## Help

File an issue at https://github.com/jezweb/office-town/issues with the step that failed + the full error.

For the agent-assisted version (recommended), see [INSTALL.md](./INSTALL.md).
