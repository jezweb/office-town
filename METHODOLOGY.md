# Methodology

Office Town's vocabulary and conventions, condensed. For deeper background, see the upstream reference at `~/Documents/.jez/knowledge/office-town.md`.

## The four primitives

| Word | Question it answers | Goose primitive | Lives in |
|---|---|---|---|
| **Town** | *In which world?* | (the goose installation + this folder) | Just a name |
| **Place** | *Where am I working?* | Project (working directory + `AGENTS.md`) | Folder on disk |
| **Role** | *Who am I delegating to?* | Agent (`.md` file invokable via `@`) | File on disk |
| **Task** | *What's being done?* | Session / chat | Goose's session store |

Three nouns are content layer; the fourth (Town) is just the name we put on the whole thing. Sessions don't need their own metaphor noun — they're just "doing a task at the Library" or "a sit-down with the librarian".

## The complete vocabulary

| Office Town word | Goose primitive | Notes |
|---|---|---|
| Town | (whole setup) | Brand-level — "Office Town" |
| Place | Project | A building. Each has its own `AGENTS.md`. |
| Role | Agent | Stateless identity defined in a `.md` file. |
| Task | Session / chat | One conversation. Shows in the Goose sidebar. |
| **Playbook** | Recipe | A preset bundle: instructions + tools + role assignment. |
| **Skill** | Skill | No rename — universal word. |
| **Service** | Extension (MCP) | A wired-in external capability (Gmail, search, etc.). |
| **App** | App | No rename — universal word. |
| **Routine** | Schedule | A cron'd / recurring task. |
| **Trigger** | Hook | Event-driven automatic action. |
| **Briefing** | `AGENTS.md` | Per-place context that auto-loads. |
| **Standing orders** | Persistent instructions (MOIM) | Reminders applied every turn. |
| **Tools** | Tools | No rename. |
| **Notes / journal / findings** | Memory | Use the building's existing subfolder taxonomy. |
| **Delegate to** *(verb)* | `@-mention` subagent dispatch | "Delegate the research to the worker (`@worker`)" |

## On file naming

Office Town standardises on **`AGENTS.md`** — the emerging cross-tool standard (see https://agents.md/). Goose loads it natively. Cursor, Aider and other tools also read it. The legacy `.goosehints` still works but is documented as such; new content uses `AGENTS.md`.

If you also use Claude Code on the same town, drop a one-line `CLAUDE.md` that points at `AGENTS.md`:

```markdown
# CLAUDE.md
See @AGENTS.md for the actual content.
```

## The bracket convention

Use Office Town words by default. When a reader might need to find the underlying Goose primitive, bracket it on first mention in a doc, then drop the bracket.

> *"Open a new **task** (chat) in **the Library** and **delegate** the catalogue update **to the librarian** (`@librarian`)."*

| Context | Use which words | Bracket? |
|---|---|---|
| Goose UI (sidebar, settings) | Goose's words (we can't change these) | N/A |
| `AGENTS.md` / briefings | Office Town | First mention only |
| Internal team docs | Office Town | First mention only |
| Client-facing docs | Office Town | Drop entirely |
| Code comments / technical docs | Goose's words | Don't overlay |

## The buildings — core four

The default Office Town ships with four buildings. Each is a Goose project (a working directory with `AGENTS.md`). Each houses a specific role; the natural pairing is below but the relationship is independent — you can work *at* any place *with* any role.

| Building | Role housed there | Speciality |
|---|---|---|
| **The Office** (or Town Hall) | `boss` | Dispatch, conversation, the user's primary contact |
| **The Library** | `librarian` | Extracts from external systems + curates the wiki — the growth engine |
| **The Workshop** | `worker` | Deep work — research, building, executing |
| **The Lookout** | `scout` | Outward scanning — environment, industry, tools |

You can add more buildings as your team grows. Common additions from packs (separately installable):

- **The Studio** — creative work (designer, copywriter)
- **The Planning Room** — product/project management
- **The Sales Floor** — quoting, proposals, estimating
- **The Help Desk** — public-facing intake (separate from the boss)
- **The Town Square** — shared coordination notice board

## Role packs

Office Town ships as composable plugins. Install only what you need:

| Pack | Roles | Plugin |
|---|---|---|
| **Core** (mandatory) | boss, librarian, worker, scout | `office-town-core` |
| **Business** | estimator, project-manager, product-manager, marketer, writer | `office-town-pack-business` |
| **Creative** | designer, copywriter, video-editor, web-designer | `office-town-pack-creative` |
| **Technical** | wordpress-specialist, hostmaster, devops, code-reviewer | `office-town-pack-technical` |
| **Comms** | helpdesk, social-poster, newsletter-editor | `office-town-pack-comms` |

Individual roles available outside packs as `office-town-role-<name>` plugins.

## The librarian is extractive AND curative

The librarian doesn't just tend what's there. She actively reaches into external systems — email, CRM, websites, file drops, chat archives — extracts what belongs in the wiki, and files it. Other roles drop findings into her inbox; she promotes the ones that earn their place. She's the wiki's *growth engine*.

This is a deliberate departure from many "librarian" mental models (passive curator). For Office Town, extraction is half the job.

## The sentence grammar

Every operation in the town can be described in one sentence using the nouns + verbs:

```
A TASK [chat]
opened at a PLACE [project]
delegated to a ROLE [agent]
following a PLAYBOOK [recipe]
using SERVICES [extensions]
guided by the BRIEFING [AGENTS.md]
fired by a ROUTINE [schedule] or a TRIGGER [hook]
in our TOWN [Office Town]
```

Example:

> *"At 9am every Monday, a **routine** opens a new **task** in **the Lookout**, **delegated to** the **scout**, following the 'AI news sweep' **playbook**, using the **web-search service** and the **wiki service**. The Lookout's **briefing** tells the scout where to file findings."*

Every noun and verb maps to a real Goose primitive. A non-technical reader can follow exactly what's happening.

## What we deliberately don't name

Some Goose concepts are infrastructure plumbing. They don't earn an Office Town word.

| Goose concept | Why no metaphor name |
|---|---|
| Provider (Anthropic, OpenAI, etc.) | Config, not concept |
| Model (specific LLM variant) | Same |
| API key / token | Plumbing |
| Plugin format vs extension | Same thing wrapped differently |
| Goose internal protocols (MCP, ACP) | Engineer-level only |

**The earned-place test:** would removing the metaphor name leave a future agent unable to act correctly? If no, leave it as its technical name.

## Place ≠ Role (this is important)

Place and Role are independent in Goose. You can:

- Stand in any place
- Delegate to any role
- Mix and match

The natural pairing (library → librarian) is convention, not constraint. Delegating to a different role doesn't change where the files are — it changes who's working on them.

Example: at the Workshop, the worker is researching how to build something. Mid-task, the worker delegates a lookup to the librarian (`@librarian`). The librarian responds for that turn, working on the files at the Workshop. Then the worker continues.

## On-disk shape (default)

```
office-town/
├── METHODOLOGY.md      ← this file
├── README.md           ← deployment overview
├── AGENTS.md           ← town-level guide (loaded at town root)
├── buildings/
│   ├── office/
│   │   ├── AGENTS.md             ← briefing, auto-loaded by Goose
│   │   ├── inbox/                ← incoming requests from other roles
│   │   ├── journal/              ← daily log (YYYY-MM-DD.md)
│   │   ├── findings/             ← working papers
│   │   └── facts/                ← stable facts held across sessions
│   ├── library/
│   │   ├── AGENTS.md
│   │   ├── wiki/                 ← THE shared knowledge layer
│   │   └── ...
│   ├── workshop/
│   └── lookout/
├── roles/                ← the .md files for each role's identity
├── playbooks/            ← (TBD) recipes for common workflows
└── skills/               ← (TBD) loadable skills
```

The role files (`.md` per role) get installed to `~/.agents/agents/` per Goose's default discovery path.

## Customisation per deployment

Things you'll want to set per team / per business:

| Item | Where | Why |
|---|---|---|
| Voice / tone in personas | Each role's `.md` file | Match the team's communication style |
| Wiki schema | `buildings/library/wiki/AGENTS.md` | Each business's knowledge has its own shape |
| Building set | Add new folders under `buildings/` | New roles, new spaces |
| Extension list | Goose config + per-building briefings | Different teams need different services |
| Extractors wired into the librarian | Librarian's services section | Email reader, CRM connector, scraper — per deployment |

## Sentinel test for good adoption

You'll know Office Town is landing when you (or your team) say things like:

- "Open a task at the Library with the librarian"
- "Delegate the SEO sweep to the scout"
- "What's in the Workshop's inbox today?"
- "Add a building for the designer — call it the Studio"

If people are still saying "start a chat with the agent that does library stuff", the vocabulary hasn't earned its keep yet. The mark of success is that the metaphor disappears into normal speech.
