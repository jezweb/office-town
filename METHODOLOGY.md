# Methodology

Office Town's vocabulary and conventions, condensed. For deeper background, see the upstream reference at `~/Documents/.jez/knowledge/office-town.md`.

## The four primitives

| Word | Question it answers | Goose primitive | Lives in |
|---|---|---|---|
| **Town** | *In which world?* | (the goose installation + this folder) | Just a name |
| **Place** | *Where am I working?* | Project (working directory + `.goosehints`) | Folder on disk |
| **Role** | *Who am I delegating to?* | Agent (`.md` file invokable via `@`) | File on disk |
| **Task** | *What's being done?* | Session / chat | Goose's session store |

Three nouns are content layer; the fourth (Town) is just the name we put on the whole thing. Sessions don't need their own metaphor noun — they're just "doing a task at the Library" or "a sit-down with the librarian".

## The complete vocabulary

| Office Town word | Goose primitive | Notes |
|---|---|---|
| Town | (whole setup) | Brand-level — "Office Town" |
| Place | Project | A building. Each has its own `.goosehints`. |
| Role | Agent | Stateless identity defined in a `.md` file. |
| Task | Session / chat | One conversation. Shows in the Goose sidebar. |
| **Playbook** | Recipe | A preset bundle: instructions + tools + role assignment. |
| **Skill** | Skill | No rename — universal word. |
| **Service** | Extension (MCP) | A wired-in external capability (Gmail, search, etc.). |
| **App** | App | No rename — universal word. |
| **Routine** | Schedule | A cron'd / recurring task. |
| **Trigger** | Hook | Event-driven automatic action. |
| **Briefing** | `.goosehints` | Per-place context that auto-loads. |
| **Standing orders** | Persistent instructions | Reminders applied every turn. |
| **Tools** | Tools | No rename. |
| **Notes / journal / findings** | Memory | Use the building's existing subfolder taxonomy. |
| **Delegate to** *(verb)* | `@-mention` subagent dispatch | "Delegate the research to the researcher (`@researcher`)" |

## The bracket convention

Use Office Town words by default. When a reader might need to find the underlying Goose primitive, bracket it on first mention in a doc, then drop the bracket.

> *"Open a new **task** (chat) in **the Library** and **delegate** the catalogue update **to the librarian** (`@librarian`)."*

| Context | Use which words | Bracket? |
|---|---|---|
| Goose UI (sidebar, settings) | Goose's words (we can't change these) | N/A |
| `.goosehints` / briefings | Office Town | First mention only |
| Internal team docs | Office Town | First mention only |
| Client-facing docs | Office Town | Drop entirely |
| Code comments / technical docs | Goose's words | Don't overlay |

## The buildings

Default set of five buildings. Each is a Goose project (a working directory with `.goosehints`). Each houses a specific role; the natural pairing is below but the relationship is independent — you can work *at* any place *with* any role.

| Building | Role housed there | Speciality |
|---|---|---|
| **The Office** (or Town Hall) | `boss` | Dispatch, conversation, the user's primary contact |
| **The Library** | `librarian` | Curates the wiki, indexes knowledge, files decisions |
| **The Workshop** | `worker` | Deep work — research, building, executing |
| **The Lookout** | `scout` | Outward scanning — environment, industry, tools |
| **The Post Office** | `anthro` | Comms routing + machine ops |

You can add more buildings as your team grows:

- **The Studio** — creative work
- **The Kitchen** — recipe (playbook) execution
- **The Archive** — superseded material, cold storage
- **The Town Square** — shared coordination notice board

## The sentence grammar

Every operation in the town can be described in one sentence using the nouns + verbs:

```
A TASK [chat]
opened at a PLACE [project]
delegated to a ROLE [agent]
following a PLAYBOOK [recipe]
using SERVICES [extensions]
guided by the BRIEFING [hints]
fired by a ROUTINE [schedule] or a TRIGGER [hook]
in our TOWN [Office Town]
```

Example:

> *"At 9am every Monday, a **routine** opens a new **task** in **the Lookout**, **delegated to** the **scout**, following the 'AI news sweep' **playbook**, using the **web-search service** and the **library service**. The Lookout's **briefing** tells the scout where to file findings."*

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

The natural pairing (study → researcher, library → librarian) is convention, not constraint. Delegating to a different role doesn't change where the files are — it changes who's working on them.

Example: at the Workshop, the worker is researching how to build something. Mid-task, the worker delegates a lookup to the librarian (`@librarian`). The librarian responds for that turn, working on the files at the Workshop. Then the worker continues.

## On-disk shape (default)

```
office-town/
├── METHODOLOGY.md      ← this file
├── README.md           ← deployment overview
├── buildings/
│   ├── office/
│   │   ├── .goosehints           ← briefing, auto-loaded by Goose
│   │   ├── inbox/                ← incoming requests from other roles
│   │   ├── journal/              ← daily log (YYYY-MM-DD.md)
│   │   ├── findings/             ← working papers
│   │   └── facts/                ← stable facts held across sessions
│   ├── library/
│   │   ├── .goosehints
│   │   ├── inbox/                ← + wiki/, the shared knowledge layer
│   │   └── ...
│   └── ...
├── roles/                ← (TBD) the .md files for each role's identity
├── playbooks/            ← (TBD) recipes for common workflows
└── skills/               ← (TBD) loadable skills
```

The role files (`.md` per role) currently live in `~/.agents/agents/` per Goose's default. They could optionally live in `roles/` here if the deployment wants everything in one tree.

## Customisation per deployment

Things you'll want to set per team / per business:

| Item | Where | Why |
|---|---|---|
| Voice / tone in personas | Each role's `.md` file | Match the team's communication style |
| Comms channels (in post-office briefing) | `buildings/post-office/.goosehints` | iMessage / Slack / email — pick what's wired in |
| Wiki schema (in library) | `buildings/library/wiki/` | Each business's knowledge has its own shape |
| Building set | Add new folders under `buildings/` | New roles, new spaces |
| Extension list | Goose config + per-building briefings | Different teams need different services |

## Sentinel test for good adoption

You'll know Office Town is landing when you (or your team) say things like:

- "Open a task at the Library with the librarian"
- "Delegate the SEO sweep to the scout"
- "What's in the Workshop's inbox today?"
- "Add a building for the designer — call it the Studio"

If people are still saying "start a chat with the agent that does library stuff", the vocabulary hasn't earned its keep yet. The mark of success is that the metaphor disappears into normal speech.
