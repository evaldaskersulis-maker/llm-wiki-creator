# llm-wiki-creator — Single Wiki Architecture

## What This Is

A Claude Code skill that initializes a wiki system — an AI assistant's persistent memory organized by domain. The skill creates the wiki directory structure, CLAUDE.md with wiki rules, conventions, and documentation layout. Currently it supports spawning child wikis in subdirectories; this project removes that capability in favor of a single flat wiki with domain subfolders.

## Core Value

One wiki per project root — all knowledge organized through domain subfolders, never through child wiki instances in subdirectories.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Remove child wiki spawning logic from claude-md-template.md (sections A, B, C, D, F, H)
- [ ] Update SKILL.md to remove any child wiki creation steps
- [ ] Update wiki-conventions.md template to reflect single-wiki structure
- [ ] Update active deployed CLAUDE.md at /home/evaldas/projects/.claude/CLAUDE.md
- [ ] Growth handled via domain subfolders (wiki/IT/, wiki/legal/, wiki/projects/MyApp/) with deeper nesting as needed

### Out of Scope

- Child/baby wiki spawning — replaced by domain subfolders
- Multi-wiki inheritance hierarchy — single wiki means no parent/child relationships
- Cross-wiki references — only one wiki exists, all wikilinks are direct

## Context

The llm-wiki-creator skill lives at `~/.claude/skills/llm-wiki-creator/`. It has:
- `SKILL.md` — skill definition with initialization steps
- `references/claude-md-template.md` — template injected into project CLAUDE.md
- `references/conventions-template.md` — wiki conventions documentation
- `references/onboarding.md` — user-facing onboarding guide

The active deployed wiki rules live at `/home/evaldas/projects/.claude/CLAUDE.md` and must be updated to match the template changes.

Key sections that reference child wikis:
- **A. Aktyvus Wiki** — walk-up algorithm to find nearest wiki/
- **B. Sesijos pradžia** — references "aktyvų wiki" discovery
- **C. Automatinis Wiki kūrimas** — explicitly creates child wikis at 20+ pages
- **D. Turinio vieta** — parent/child wiki content placement
- **F. Konteksto skaitymas** — parent/child inheritance chain
- **H. Obsidian suderinamumas** — cross-wiki reference rules

## Constraints

- **Skill location**: `~/.claude/skills/llm-wiki-creator/` (git-tracked)
- **Active CLAUDE.md**: `/home/evaldas/projects/.claude/CLAUDE.md` (not git-tracked, must be written directly)
- **Language**: All wiki rules are in Lithuanian
- **Backwards compatibility**: Existing wiki content structure should not break

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Single wiki, no children | Simpler mental model, all content in one place | — Pending |
| Domain subfolders for growth | Natural organization without wiki proliferation | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-15 after initialization*
