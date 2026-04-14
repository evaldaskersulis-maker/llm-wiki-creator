---
title: "Wiki Conventions"
type: concept
tags: [infrastructure, wiki, conventions, reference]
created: {TODAY}
updated: {TODAY}
sources: []
aliases: ["Wiki Formats", "Wiki Reference"]
---

Reference document for wiki page formats, templates, and procedures. Read by Claude before writing wiki content (triggered by CLAUDE.md rules E and G).

## Page Conventions

All wiki pages use YAML frontmatter:

```yaml
---
title: "Page Title"
type: project | entity | concept | topic | comparison | synthesis | source
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["[[source-page]]"]
aliases: ["Alternative Name"]
project: "path/relative/to/root"   # only for project pages
---
```

- Use `[[wikilinks]]` for internal references within the same wiki (Obsidian-compatible).
- Filenames: kebab-case.
- Every page must be listed in its wiki's `index.md`.
- Every page must have at least one wikilink to another page in the same wiki (no orphans).
- No H1 (`#`) in the body — Obsidian uses the filename/title as H1.

## Index Format

Each wiki's `index.md`:

```yaml
---
title: "Wiki Index"
type: index
updated: YYYY-MM-DD
last_lint: YYYY-MM-DD
---
```

Sections by type, entries sorted alphabetically:

```markdown
## Projects
- [[my-project]] — Description

## Entities
- [[some-entity]] — Description

## Concepts
- [[some-concept]] — Description
```

For the **root wiki**, add a `## Domains` section listing domain directories and their wiki paths.

## Log Format

Daily files at `{active-wiki}/logs/YYYY-MM/YYYY-MM-DD.md`.

```yaml
---
title: "YYYY-MM-DD"
date: YYYY-MM-DD
type: log
---
```

Entries:

```markdown
## HH:MM verb | Title

- What was done
- What was affected
```

Append-only within a day. New day = new file. Create month directories as needed.

## Writing Style

- Factual, concise, neutral tone.
- Cite sources with wikilinks: "According to [[source-page]], ..."
- Prefer bullet points and short paragraphs over long prose.
- Use `##` and `###` headers to structure pages.
- Mark uncertain claims with `[unverified]` or `[needs source]`.

## Operations

### Manual Ingest

When the user points to a source in `raw/`:
1. Read the source document fully.
2. Discuss key takeaways with the user.
3. Create a source summary page in the active wiki.
4. Update or create entity/concept/topic pages with new information.
5. Update the active wiki's `index.md`.
6. Append to the active wiki's daily log.

### Query

When the user asks questions against accumulated knowledge:
1. Read the active wiki's `index.md` + parent wikis' indexes (inheritance).
2. Read relevant pages and synthesize an answer.
3. If the answer is substantial and reusable, offer to file it as a wiki page.

### Naujo projekto kūrimas

Kai kuriamas naujas projektas:
1. Sukurti `CLAUDE.md` pagal šabloną:
   ```
   # {Projekto pavadinimas}

   ## Kritinės taisyklės
   - [imperatyvios instrukcijos]

   ## Credentials
   - `.env` (pagal `.env.example`)

   ## Dokumentacija
   Pilna dokumentacija: žr. `wiki/projects/{projektas}.md`
   ```
2. Sukurti `README.md` pagal šabloną:
   ```
   # {Projekto pavadinimas}

   {Kas tai per projektas — 1-2 sakiniai.}

   ## Paleidimas
   {komandos}

   ## Dokumentacija
   Pilna dokumentacija: žr. `wiki/projects/{projektas}.md`
   ```
3. Sukurti `.env.example` su reikalingais kintamaisiais (be reikšmių).
4. Sukurti wiki puslapį `wiki/projects/{projektas}.md` su pilna dokumentacija.
5. Užregistruoti `wiki/index.md`.

## Lint Checklist

### Checks

- **Orphan pages:** files in wiki not listed in `index.md` → auto-fix: add to index
- **Dead wikilinks:** `[[links]]` pointing to nonexistent pages → auto-fix: remove or update if page was moved/renamed
- **Stale pages:** `updated` date older than 90 days → report to user
- **Frontmatter gaps:** missing required fields → auto-fix: add missing fields
- **Unregistered projects:** directories without a wiki page → report to user
- **Promotion candidates:** pages referenced from 2+ wiki branches → report to user, execute promotion
- **Filename collisions:** two files with same name across wikis → auto-fix: add domain prefix
- **Contradictions:** conflicting claims across pages → report to user (do not auto-resolve)

### Optimization

- Read full content only for files with `updated >= last_lint`.
- Use `find`/`grep` for structural checks (orphans, collisions, stale pages) without reading full content.
- Stale pages: `grep 'updated:' *.md` and compare dates.
- Dead links from deletions: compare `index.md` entries against filesystem.

After lint, update `last_lint` in the active wiki's `index.md`.

## Domain CLAUDE.md Merge Semantics

Domain `.claude/CLAUDE.md` files are read upward from cwd during session start. Each deeper file can add new H2 sections (`##`) or override specific H2 sections by matching header name. Non-conflicting H2 sections from higher files remain in effect.
