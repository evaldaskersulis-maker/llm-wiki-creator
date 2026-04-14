## What was created

- **wiki/** — directory with `index.md` and subdirectories for concepts, entities, projects, logs, topics, comparisons, and synthesis
- **.claude/CLAUDE.md** — rules that tell Claude how to manage the wiki automatically
- **wiki/concepts/wiki-conventions.md** — reference for page formats, templates, and procedures
- **raw/** — directory for source documents (Claude reads but never modifies these)

## How sessions work

- **Start:** Claude finds the nearest `wiki/` from your working directory and loads its index
- **During:** Claude reads wiki pages for context when answering questions or doing work
- **End:** Claude updates wiki pages with new knowledge, appends to the daily log, and runs lint if it's due

You don't need to ask Claude to update the wiki — it happens automatically at the end of every session.

## Dokumentacijos struktūra

Claude valdo tris failų tipus kiekviename projekte:

- **CLAUDE.md** — Tik kritinės taisyklės. Dalykai, kuriuos Claude PRIVALO
  žinoti: draudimai, gotchas, credentials vietos. Skaitomas automatiškai
  kiekvieną sesiją. Trumpas (iki 25 eilučių).
- **README.md** — Žmonėms. Kas tai per projektas ir kaip paleisti.
  Trumpas (iki 30 eilučių).
- **Wiki** (`wiki/projects/`) — Viskas kita. Pilna dokumentacija:
  tech stack, architektūra, deployment, API, ryšiai su kitais projektais.
  Atnaujinama automatiškai kiekvienos sesijos pabaigoje.
- **.env** — Credentials reikšmės. Niekada ne CLAUDE.md, README ar wiki.
  `.env.example` (kintamųjų vardai be reikšmių) saugus git'e.

Claude pats nusprendžia, ką kur rašyti. Tau nereikia apie tai galvoti.

## How wikis branch

Each directory can have its own `wiki/` for **context isolation** — so unrelated projects don't pollute each other's context.

New `wiki/` directories are created automatically when:
- A directory contains `raw/` with at least one file
- Claude creates 2+ wiki pages related to a directory in a single session

Wikis **inherit context** from parent wikis: Claude reads the active wiki plus all parent wikis up to the root. Child wiki content takes priority over parent.

## How to browse

1. Open your project root as an **Obsidian vault** (or any markdown editor)
2. All wiki pages use standard markdown with `[[wikilinks]]`
3. Obsidian's graph view shows the knowledge structure
4. Filenames are unique across all wikis — so wikilinks resolve correctly vault-wide

## What you don't need to do

- **No manual wiki maintenance** — Claude handles all page creation, updates, and cross-references
- **No commands to remember** — everything runs automatically per the session rules in CLAUDE.md
- **No lint scheduling** — lint runs automatically every 7 days at session end, fixing dead links, orphan pages, and frontmatter gaps
- **No file organization** — Claude decides where pages belong based on content relevance (active wiki, parent wiki, or root wiki)

## Customization

- **Lint intervals:** Edit `.claude/CLAUDE.md`, change "7 days" (domain) or "30 days" (global) in the Lint section
- **Domain-specific rules:** Create `.claude/CLAUDE.md` in any subdirectory with additional H2 sections
- **Obsidian vault root:** By default, open your project root as the vault. All `wiki/` directories will be visible
