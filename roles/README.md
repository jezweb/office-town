# Roles

The five default roles in Office Town. Each `.md` file is a Goose agent — a stateless identity invocable via `@-mention` from any chat.

## The default set

| File | Role | Building |
|---|---|---|
| `boss.md` | The principal user's primary contact; routes work, holds the thread | The Office |
| `librarian.md` | Knowledge curator; tends the shared wiki, graduates findings | The Library |
| `worker.md` | Deep-work specialist; builds, researches, executes | The Workshop |
| `scout.md` | Outward scanner; surfaces patterns the inward roles miss | The Lookout |
| `anthro.md` | Machine steward + comms router; keeps the host healthy | The Post Office |

## Installation — making Goose discover these roles

Goose discovers agent files from specific paths. To make the Office Town roles available:

**Option A: Copy them (simplest)**

```bash
cp ~/Documents/office-town/roles/*.md ~/.agents/agents/
```

After copying, restart Goose. The `@-mention` autocomplete should now show `@boss`, `@librarian`, `@worker`, `@scout`, `@anthro`.

**Option B: Symlink them (keeps them in sync with the template)**

```bash
mkdir -p ~/.agents/agents
for f in ~/Documents/office-town/roles/*.md; do
  [ "$(basename $f)" = "README.md" ] && continue
  ln -sf "$f" ~/.agents/agents/$(basename "$f")
done
```

Editing the template files will immediately update what Goose sees (after restart).

**Option C: Configure Goose's agent search path** (advanced)

If your Goose installation supports it, point its agent discovery path at `~/Documents/office-town/roles/` directly. Check Goose's documentation for the relevant configuration.

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

1. Create a new building folder: `buildings/<building-name>/` with `.goosehints` and subfolders
2. Create the role file: `roles/<role-name>.md` with frontmatter and body
3. Install it (copy or symlink per options above)
4. Update other roles' `.goosehints` to mention the new building under "Adjacent buildings"

Example additions to consider:

| Role | Building | What they do |
|---|---|---|
| `designer` | The Studio | Creative work, mockups, visual design |
| `chef` | The Kitchen | Run playbooks (recipes), batch operations |
| `archivist` | The Archive | Cold storage, superseded material, history |
| `host` | The Town Square | Maintains the shared notice board |

Add what the town actually needs; don't pre-fill with roles you might want later. Empty buildings on the map are fine — they're invitations.

## Why the shape matters

Each role file is small and consistent because:

- **Goose loads it as the system prompt** when the role is `@-mentioned`. Longer files = more tokens per turn.
- **Humans need to read these too.** A consistent shape lets a teammate skim a new role and know it
- **The model performs better with focused identities** than with sprawling persona prompts. Tell it who it is, what it does, what it doesn't do, and how to handle hand-offs — let it do the rest.

If a role file starts pushing past ~120 lines, that's a sign it's trying to do too much. Split into a sub-specialist or move details into a `skill` the role can load on demand.
