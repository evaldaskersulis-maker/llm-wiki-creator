---
name: llm-wiki-creator
description: >
  Inicializuoja wiki sistemą — asmeninio AI asistento atmintį.
  Sukuria wiki/ struktūrą su domeniniais aplankais, CLAUDE.md su wiki taisyklėmis,
  konvencijas, ir dokumentacijos pasiskirstymą. Priima argumentą: projekto
  šakninis kelias (numatytasis — cwd).
---

## Wiki System Initialization

Set up a wiki-based memory system for an AI personal assistant. Covers any domain — legal, finance, IT, coding, health, or anything else.

### Steps

1. **Resolve project root:** Use `$ARGUMENTS` if provided, otherwise use cwd. Store as `{PROJECT_ROOT}` (absolute path).

2. **Guard check:** Read `.claude/CLAUDE.md` if it exists. If it already contains a section titled "## A. Aktyvus Wiki" — warn the user that wiki rules already exist and ask whether to overwrite or skip. If it does not exist or has no wiki rules — proceed.

3. **Read CLAUDE.md template:** Read `${CLAUDE_SKILL_DIR}/references/claude-md-template.md`. Replace every occurrence of `{PROJECT_ROOT}` with the resolved absolute path from step 1. This substitution must be literal — do not paraphrase or rewrite the template content.

4. **Write CLAUDE.md:**
   - If `.claude/CLAUDE.md` does not exist → create it with the substituted template content.
   - If it exists but has no wiki rules → append the substituted template content after existing content, separated by a blank line.
   - If it exists with wiki rules and user chose overwrite → replace wiki sections only.

5. **Create wiki/ structure:** All paths relative to `{PROJECT_ROOT}`:
   ```
   mkdir -p {PROJECT_ROOT}/wiki/logs/
   ```
   Note: `raw/` directories are created organically in project subdirectories as needed (e.g., `Teise/Skolininkai/raw/`), not at the project root during initialization.

   Ask the user which domain folders to create. Suggest based on context (e.g., a coding project might want `projects/`, a personal root might want `legal/`, `finance/`, `IT/`). Create whatever the user picks. If unsure, start with no domain folders — they'll be created organically as content arrives.

   Create `{PROJECT_ROOT}/wiki/index.md` with this content:
   ```yaml
   ---
   title: "Wiki Index"
   type: index
   updated: {TODAY}
   last_lint: {TODAY}
   ---
   ```
   Replace `{TODAY}` with today's date (YYYY-MM-DD).

6. **Create conventions:** Read `${CLAUDE_SKILL_DIR}/references/conventions-template.md`. Replace `{TODAY}` with today's date. Write to `{PROJECT_ROOT}/wiki/wiki-conventions.md`. Update `wiki/index.md` to include `- [[wiki-conventions]] — Wiki formats, templates, and procedures`.

7. **Initial log entry:** Create `wiki/logs/{YYYY-MM}/{YYYY-MM-DD}.md` with today's date and:
   ```markdown
   ---
   title: "{YYYY-MM-DD}"
   date: {YYYY-MM-DD}
   type: log
   ---

   ## {HH:MM} register | wiki-init

   - Initialized wiki system via /llm-wiki-creator skill
   - Created: wiki/ structure, .claude/CLAUDE.md, wiki-conventions.md
   ```

8. **Ask about .gitignore:** Ask the user: "Should wiki/ be tracked in git, or added to .gitignore?" Act on their answer. If they're unsure, recommend tracking (wiki is valuable knowledge).

9. **Onboarding:** Read `${CLAUDE_SKILL_DIR}/references/onboarding.md` and present its content to the user. This explains how the wiki system works.
