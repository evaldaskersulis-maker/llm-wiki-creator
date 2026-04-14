## Dokumentacijos struktūra

  Visa projekto dokumentacija saugoma wiki (`wiki/`).
  Projektų aplankuose lieka tik minimalūs failai:

  **CLAUDE.md** (projekto aplanke) — automatiškai nuskaitomas sesijos pradžioje.
  Turi TIK: (1) imperatyvias taisykles — draudimus ir privalomus veiksmus,
  (2) credentials nuorodas — .env kintamųjų vardai (NIEKADA reikšmės),
  (3) nuorodą į wiki puslapį.
  Čia NERAŠOMA: tech stack, failų struktūra, architektūra, deployment,
  API aprašymai — visa tai eina į wiki.
  CLAUDE.md turi likti trumpas — iki 25 eilučių (be wiki sistemos taisyklių).

  **Wiki** (`wiki/projects/`) — pilna dokumentacija: tech stack, architektūra,
  ryšiai (wikilinks), deployment, API, gotchas, credentials vardai, statusas.
  Cross-cutting temos — `concepts/`, `topics/`.

  **README.md** — žmonėms: kas tai (1-2 sakiniai), kaip paleisti, nuoroda į wiki.
  Iki 30 eilučių.

  **.env** — visos credentials reikšmės TIK čia. Wiki/CLAUDE.md turi tik vardus.
  **.env.example** — kintamųjų vardai be reikšmių, saugus git'e.

  **Prieštaravimai:** jei CLAUDE.md ir wiki nesutampa — CLAUDE.md teisus
  (imperatyvios taisyklės turi pirmenybę). Atnaujinti wiki.

  Naujo projekto kūrimo procedūra — žr. [[wiki-conventions]].

# Claude Wiki Sistema

Claude palaiko decentralizuotą wiki sistemą po `{PROJECT_ROOT}`. Kiekviena direktorija gali turėti savo `wiki/` — wiki šakojasi žemyn pagal projekto struktūrą. Claude yra vienintelis wiki turinio autorius ir redaktorius.

## A. Aktyvus Wiki

Algoritmas rasti aktyvų wiki nuo cwd:
1. Patikrinti ar `cwd/wiki/` egzistuoja — jei taip, tai yra aktyvus wiki
2. Pereiti į tėvinę direktoriją ir patikrinti ar `wiki/` egzistuoja
3. Kartoti 2 žingsnį, kylant aukštyn kol randamas `wiki/`
4. **Fallback:** `{PROJECT_ROOT}/wiki/` (globalus) visada egzistuoja

Pavyzdžiai: iš `{PROJECT_ROOT}/SubProject/Feature/` → tikrina `Feature/wiki/`, tada `SubProject/wiki/`, tada `{PROJECT_ROOT}/wiki/`.

## B. Sesijos pradžia

Jei cwd yra po `{PROJECT_ROOT}`:
1. Rasti aktyvų wiki (taisyklė A). Perskaityti jo `index.md`.
2. Jei cwd neturi `wiki/`, užregistruoti cwd kaip projektą aktyvaus wiki `index.md`.
3. Užtikrinti, kad `raw/` egzistuoja cwd.
4. Skaityti `.claude/CLAUDE.md` failus aukštyn nuo cwd (gilesnis perrašo aukštesnį; detalės `wiki-conventions.md`).
5. Jei cwd atitinka registruotą projektą aktyvaus wiki `index.md` — perskaityti to projekto wiki puslapį pilnam kontekstui. Jei projektas neregistruotas, bet egzistuoja `wiki/projects/` puslapis pagal aplanko pavadinimą — irgi perskaityti.

## C. Automatinis Wiki kūrimas

Sukurti `wiki/` automatiškai kai:
1. Direktorija turi `raw/` su ≥1 failu
2. Sesija sukuria 2+ puslapius šiai direktorijai (laikina vieta: tėvinis wiki; perkelti žemyn po)

Naujas wiki gauna: `index.md` (su `last_lint: šiandiena`), subdirektorijas (`concepts/`, `entities/`, `projects/`, `logs/`, `topics/`, `comparisons/`, `synthesis/`). Užregistruoti tėvinio wiki `index.md`.

## D. Turinio vieta

- Numatyta: rašyti į aktyvų wiki
- Platesnė reikšmė → tėvinis wiki; tarp-domeninis → globalus wiki
- **Promotion (lint metu):** puslapis, į kurį nurodoma iš 2+ wiki šakų → perkelti aukštyn, palikti nuorodą
- **Konfliktas:** vaikinio wiki versija yra autoritetingesnė už tėvinio

## E. Sesijos pabaiga

0. **Perskaityti `wiki/concepts/wiki-conventions.md`** formatams ir taisyklėms.
1. Sukurti arba atnaujinti wiki puslapius aktyviam wiki.
1b. **CLAUDE.md patikra:** Ar sesijoje atsirado naujų kritinių taisyklių?
    Triggeriai: komanda kuri sulaužo sistemą, naujas privalomas env kintamasis,
    eiliškumo priklausomybė, porto konfliktas, failas kurio negalima keisti.
    Jei taip — pridėti TIK taisyklę į projekto CLAUDE.md (ne tech stack, ne struktūrą).
1c. **README.md patikra:** Ar pasikeitė paleidimo/instaliacijos komandos?
    Triggeriai: pakeitimai Dockerfile, docker-compose.yml, package.json scripts,
    Makefile, requirements.txt, pyproject.toml, arba naujas .env kintamasis.
    Jei taip — atnaujinti README.md paleidimo sekciją.
1d. **.env.example patikra:** Ar atsirado naujų env kintamųjų arba panaikinti seni?
    Jei taip — atnaujinti .env.example (tik vardai, be reikšmių).
2. Atnaujinti `index.md` jei pridėti/pašalinti puslapiai.
3. Pridėti sesijos santrauką į `{aktyvus-wiki}/logs/YYYY-MM/YYYY-MM-DD.md`. Veiksmažodžiai: `ingest`, `create`, `update`, `build`, `fix`, `lint`, `query`, `register`, `delete`.
4. Neliesti globalaus wiki automatiškai. Išimtys: wiki registracija (C), promotion (D).
5. Paleisti lint jei laikas (taisyklė G).
6. Pasakyti naudotojui kas atnaujinta.

## F. Konteksto skaitymas

- Skaityti aktyvų wiki + visus tėvinius wiki iki globalaus (paveldėjimas)
- Neskaityti seserų wiki; tarp-domeninis tik naudotojui paprašius
- Prioritetas: vaikinis > tėvinis (konkretesnis laimi)

## G. Lint (sesijos pabaiga, automatinis)

0. **Perskaityti `wiki/concepts/wiki-conventions.md`** lint procedūroms.
1. **Domeninis lint:** kas 7 dienas (tik aktyvus wiki). **Globalus lint:** kas 30 dienų (visi wiki).
2. Triggerina `last_lint` data aktyvaus wiki `index.md`.
3. **Optimizacija:** skaityti turinį tik failams su `updated >= last_lint`. Naudoti `find`/`grep` struktūriniams patikrinimams (orphans, kolizijos, seni puslapiai).
4. **Auto-fix:** mirę linkai, orphan registracija, frontmatter spragos, failų vardų kolizijos. **Raportuoti:** senus puslapius, prieštaravimus, promotion kandidatus.

## H. Obsidian suderinamumas

**PRIVALOMA:** failų vardai turi būti unikalūs per visus wiki (pridėti domeno prefiksą jei generinis). Claude rašo wikilinks tik to paties wiki viduje. Tarp-wiki nuorodos naudoja paprastą tekstą.

<!-- Lint intervalai konfigūruojami: keiskite "7 dienas" / "30 dienų" aukščiau pagal pageidavimą -->

## Saugos taisyklės

- Niekada nekeisti `raw/` failų. Niekada netrinti wiki puslapių be naudotojo patvirtinimo.
- Logai yra append-only per dieną. Visada atnaujinti `updated` frontmatter redaguojant.
- Prieš kuriant puslapį, patikrinti ar toks jau egzistuoja.
