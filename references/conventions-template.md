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

- Use `[[wikilinks]]` for internal references within the same wiki (Obsidian-compatible).
- Filenames: kebab-case.
- Every page must be listed in its wiki's `index.md`.
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

Each wiki's `index.md`:

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
## Legal
- [[some-contract]] — Description

## IT
- [[server-setup]] — Description

## Projects
- [[my-project]] — Description
```

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
4. Update or create related pages with new information.
5. Update the active wiki's `index.md`.
6. Append to the active wiki's daily log.

### Query

When the user asks questions against accumulated knowledge:
1. Read the active wiki's `index.md` + parent wikis' indexes (inheritance).
2. Read relevant pages and synthesize an answer.
3. If the answer is substantial and reusable, offer to file it as a wiki page.

### Naujo projekto kurimas

Kai kuriamas naujas projektas:
1. Sukurti `CLAUDE.md` pagal sabloną:
   ```
   # {Projekto pavadinimas}

   ## Kritines taisykles
   - [imperatyvios instrukcijos]

   ## Credentials
   - `.env` (pagal `.env.example`)

   ## Dokumentacija
   Pilna dokumentacija: žr. `wiki/projects/{projektas}.md`
   ```
2. Sukurti `README.md` pagal sabloną:
   ```
   # {Projekto pavadinimas}

   {Kas tai per projektas — 1-2 sakiniai.}

   ## Paleidimas
   {komandos}

   ## Dokumentacija
   Pilna dokumentacija: žr. `wiki/projects/{projektas}.md`
   ```
3. Sukurti `.env.example` su reikalingais kintamaisiais (be reiksiu).
4. Sukurti wiki puslapi `wiki/projects/{projektas}.md` su pilna dokumentacija.
5. Uzregistruoti `wiki/index.md`.

## Lint Checklist

Claude lintina oportunistiškai — kai pastebi problemų skaitydamas wiki, ne pagal grafiką.

### Checks

- **Orphan pages:** files in wiki not listed in `index.md` -> auto-fix: add to index
- **Dead wikilinks:** `[[links]]` pointing to nonexistent pages -> auto-fix: remove or update
- **Frontmatter gaps:** missing required fields -> auto-fix: add missing fields
- **Filename collisions:** two files with same name across wikis -> auto-fix: add domain prefix
- **Stale pages:** `updated` date older than 90 days -> report to user
- **Contradictions:** conflicting claims across pages -> report to user

After lint, update `last_lint` in the active wiki's `index.md`.

## Domain CLAUDE.md Merge Semantics

Domain `.claude/CLAUDE.md` files are read upward from cwd during session start. Each deeper file can add new H2 sections (`##`) or override specific H2 sections by matching header name. Non-conflicting H2 sections from higher files remain in effect.
