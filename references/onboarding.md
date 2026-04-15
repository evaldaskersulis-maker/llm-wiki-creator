## What was created

- **wiki/** — directory with `index.md` and `logs/` for session history
- **.claude/CLAUDE.md** — rules that tell Claude how to manage the wiki automatically
- **wiki/wiki-conventions.md** — reference for page formats, templates, and procedures
- **raw/** — directory for source documents (Claude reads but never modifies these)

## How it works

This wiki is Claude's long-term memory. It stores knowledge across any domain — legal documents, financial plans, IT infrastructure, coding projects, health records, or anything else you work on.

- **Start of session:** Claude reads `wiki/index.md` and relevant domain pages for context
- **During session:** Claude reads wiki pages when they're relevant to your questions or work
- **End of session:** If the session produced knowledge worth remembering, Claude updates the wiki. Trivial sessions (quick questions, small fixes) don't trigger updates

The key test: "Would a future Claude session need this, and can't derive it from existing files?" If no — nothing gets written.

## Dokumentacijos struktūra

Claude valdo tris failų tipus kiekviename projekte:

- **CLAUDE.md** — Tik kritinės taisyklės. Dalykai, kuriuos Claude PRIVALO
  žinoti: draudimai, gotchas, credentials vietos. Trumpas (iki 25 eilučių).
- **README.md** — Žmonėms. Kas tai per projektas ir kaip paleisti.
  Trumpas (iki 30 eilučių).
- **Wiki** (`wiki/`) — Viskas kita. Pilna dokumentacija, organizuota pagal
  domenus (pvz. `legal/`, `finance/`, `IT/`, `projects/`).
- **.env** — Credentials reikšmės. Niekada ne CLAUDE.md, README ar wiki.
  `.env.example` (kintamųjų vardai be reikšmių) saugus git'e.

## Domain folders

Wiki content is organized by life/work domain:

```
wiki/
  index.md
  logs/
  legal/          — contracts, regulations
  finance/        — budgets, taxes, investments
  IT/             — servers, networking, infrastructure
  projects/       — software projects
  wiki-conventions.md
```

Folders are created as needed — don't worry about setting them up in advance. When you start working in a new domain, Claude creates the folder and files it there.

## How to browse

1. Open your project root as an **Obsidian vault** (or any markdown editor)
2. All wiki pages use standard markdown with `[[wikilinks]]`
3. Obsidian's graph view shows the knowledge structure
4. Domain folders give you a natural way to browse by topic
5. Filenames are unique across the wiki — so wikilinks resolve correctly vault-wide

## What you don't need to do

- **No manual wiki maintenance** — Claude handles page creation, updates, and cross-references
- **No commands to remember** — everything runs automatically per the rules in CLAUDE.md
- **No scheduled lint** — Claude fixes issues (dead links, orphan pages) when it notices them while reading
- **No noise** — only meaningful knowledge gets captured, not session ceremony
