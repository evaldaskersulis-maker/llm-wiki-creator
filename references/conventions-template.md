---
title: "Wiki Conventions"
type: reference
tags: [infrastructure, wiki, conventions]
created: {TODAY}
updated: {TODAY}
sources: []
aliases: ["Wiki Formats", "Wiki Reference"]
---

Reference document for wiki page formats, templates, and procedures. Read by Claude when writing wiki content.

## Page Conventions

All wiki pages use YAML frontmatter:

```yaml
---
title: "Page Title"
type: note | project | source | reference | log
tags: [domain, topic]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["[[source-page]]"]
aliases: ["Alternative Name"]
---
```

- Use `[[wikilinks]]` for internal references (Obsidian-compatible).
- Filenames: kebab-case.
- Every page must be listed in `index.md`.
- No H1 (`#`) in the body — Obsidian uses the filename/title as H1.

## Domain Folders

Wiki pages are organized by life/work domain rather than abstract categories. Examples:

- `legal/` — contracts, regulations, procedures
- `finance/` — budgets, tax, investments
- `IT/` — infrastructure, servers, networking
- `projects/` — software projects, each with its own page
- `health/` — medical info, procedures

Create domain folders organically as content arrives. Don't pre-create empty folders.
Pages that span multiple domains go in the one that's most relevant, with wikilinks from the other.

## Index Format

`wiki/index.md`:

```yaml
---
title: "Wiki Index"
type: index
updated: YYYY-MM-DD
last_lint: YYYY-MM-DD
---
```

Sections by domain, entries sorted alphabetically:

```markdown
## Finance
- [[some-finance-page]] — Description

## IT
- [[some-it-page]] — Description

## Legal
- [[some-legal-page]] — Description

## Reference
- [[wiki-conventions]] — Wiki formats, templates, and procedures
```

Domain sections are created as content appears.

## Log Format

Daily files at `wiki/logs/YYYY-MM/YYYY-MM-DD.md`.

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

`raw/` katalogas gali būti bet kuriame poaplankio lygyje (pvz. `Sysadmin/Klviatūra/raw/`, `Teise/Skolininkai/raw/`). Failai ten yra read-only šaltiniai — Claude tik skaito, niekada nekeičia.

Kai naudotojas nurodo `raw/` katalogą arba Claude aptinka jį sesijos pradžioje:
1. Read the source document(s) fully.
2. Discuss key takeaways with the user.
3. Create a source summary page in the wiki under the matching domain folder.
4. Update or create related pages with new information.
5. Update `index.md`.
6. Append to the daily log with verb `ingest`.

### Query

When the user asks questions against accumulated knowledge:
1. Read `index.md` and relevant domain pages.
2. Synthesize an answer.
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

Claude lintina oportunistiškai — kai pastebi problemų skaitydamas wiki, ne pagal grafiką.

### Checks

- **Orphan pages:** files in wiki not listed in `index.md` → auto-fix: add to index
- **Dead wikilinks:** `[[links]]` pointing to nonexistent pages → auto-fix: remove or update if page was moved/renamed
- **Stale pages:** `updated` date older than 90 days → report to user
- **Frontmatter gaps:** missing required fields → auto-fix: add missing fields
- **Filename collisions:** two files with same name in different domain folders → auto-fix: add domain prefix
- **Contradictions:** conflicting claims across pages → report to user (do not auto-resolve)

After lint, update `last_lint` in `index.md`.
