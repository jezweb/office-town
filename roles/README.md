# Roles

The four core roles in Office Town. Each `.md` file is a Goose agent — a stateless identity invocable via `@-mention` from any chat.

## The core set (this template)

| File | Role | Building |
|---|---|---|
| `boss.md` | The principal user's primary contact; routes work, holds the thread | The Office |
| `librarian.md` | Extracts from external systems + curates the wiki — the growth engine | The Library |
| `worker.md` | Deep-work specialist; builds, researches, executes | The Workshop |
| `scout.md` | Outward scanner; surfaces patterns the inward roles miss | The Lookout |

## Additional roles (separately installable packs)

Office Town's role library is distributed as separate plugins. Install what your deployment needs:

| Pack plugin | Roles | Use case |
|---|---|---|
| `office-town-pack-business` | estimator, project-manager, product-manager, marketer, writer | Service businesses, consultancies |
| `office-town-pack-creative` | designer, copywriter, video-editor, web-designer | Agencies, creative shops |
| `office-town-pack-technical` | wordpress-specialist, hostmaster, devops, code-reviewer | Tech shops, dev consultancies |
| `office-town-pack-comms` | helpdesk, social-poster, newsletter-editor | High-comms-volume teams |

Plus individual roles as `office-town-role-<name>` plugins (e.g., `office-town-role-mentor` for fleet quality review, `office-town-role-archivist` for deployments with serious archival needs).

## Installation — making Goose discover these roles

Goose discovers agent files from specific paths. To make the Office Town core roles available:

**Option A: Copy them (simplest)**

```bash
mkdir -p ~/.agents/agents
cp ~/Documents/office-town/roles/*.md ~/.agents/agents/
```

After copying, restart Goose. The `@-mention` autocomplete should now show `@boss`, `@librarian`, `@worker`, `@scout`.

**Option B: Symlink them (keeps in sync with the template)**

```bash
mkdir -p ~/.agents/agents
for f in ~/Documents/office-town/roles/*.md; do
  [ "$(basename $f)" = "README.md" ] && continue
  ln -sf "$f" ~/.agents/agents/$(basename "$f")
done
```

Editing the template files will immediately update what Goose sees (after restart).

**Option C: Plugin install** (when role packs are published)

```bash
goose plugin install jezweb/office-town-pack-business
```

## Customising for your deployment

Each role's `.md` file is a system-prompt-shaped identity. Customise per deployment:

| Customisation | Where in the role file |
|---|---|
| Voice / tone | The "Vibe" line in Identity; voice rules section |
| Local language variant | Add to vibe (e.g., "Australian English, no filler") |
| Additional standing orders | "Standing orders" section |
| Team-specific scope | "What I do" / "What I don't do" sections |
| Different model | Add `model: claude-sonnet-4-6` (or other) to the frontmatter |

Keep the core shape (frontmatter → identity → wake up → what I do / don't do → voice → standing orders → end of session). It's predictable for both humans and the model.

## Adding new roles

To add a new role to your town:

1. Create a new building folder: `buildings/<building-name>/` with `AGENTS.md` and subfolders
2. Create the role file: `roles/<role-name>.md` with frontmatter and body
3. Install it (copy or symlink per options above)
4. Update other roles' building `AGENTS.md` to mention the new neighbour under "Adjacent buildings"

## Why the shape matters

Each role file is small and consistent because:

- **Goose loads it as the system prompt** when the role is `@-mentioned`. Longer files = more tokens per turn.
- **Humans need to read these too.** A consistent shape lets a teammate skim a new role and know it
- **The model performs better with focused identities** than with sprawling persona prompts. Tell it who it is, what it does, what it doesn't do, and how to handle hand-offs — let it do the rest.

If a role file starts pushing past ~120 lines, that's a sign it's trying to do too much. Split into a sub-specialist or move details into a skill the role can load on demand.
