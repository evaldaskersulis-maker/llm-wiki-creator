## Dokumentacijos struktūra

  Visa dokumentacija saugoma wiki (`wiki/`).
  Projektų aplankuose lieka tik minimalūs failai:

  **CLAUDE.md** (projekto aplanke) — automatiškai nuskaitomas sesijos pradžioje.
  Turi TIK: (1) imperatyvias taisykles — draudimus ir privalomus veiksmus,
  (2) credentials nuorodas — .env kintamųjų vardai (NIEKADA reikšmės),
  (3) nuorodą į wiki puslapį.
  Čia NERAŠOMA: tech stack, failų struktūra, architektūra, deployment,
  API aprašymai — visa tai eina į wiki.
  CLAUDE.md turi likti trumpas — iki 25 eilučių (be wiki sistemos taisyklių).

  **Wiki** (`wiki/`) — pilna dokumentacija: tech stack, architektūra,
  ryšiai (wikilinks), deployment, API, gotchas, credentials vardai, statusas.
  Organizuota pagal domenus (pvz. `legal/`, `finance/`, `IT/`, `projects/`).

  **README.md** — žmonėms: kas tai (1-2 sakiniai), kaip paleisti, nuoroda į wiki.
  Iki 30 eilučių.

  **.env** — visos credentials reikšmės TIK čia. Wiki/CLAUDE.md turi tik vardus.
  **.env.example** — kintamųjų vardai be reikšmių, saugus git'e.

  **Prieštaravimai:** jei CLAUDE.md ir wiki nesutampa — CLAUDE.md teisus
  (imperatyvios taisyklės turi pirmenybę). Atnaujinti wiki.

  Naujo projekto kūrimo procedūra — žr. [[wiki-conventions]].

# Claude Wiki Sistema

Claude palaiko wiki sistemą po `{PROJECT_ROOT}`. Wiki tarnauja kaip AI asistento atmintis — bet koks domenas (teisė, finansai, IT, kodas, sveikata ir t.t.). Claude yra vienintelis wiki turinio autorius ir redaktorius.

## A. Aktyvus Wiki

Vienas wiki: `{PROJECT_ROOT}/wiki/`. Struktūra organizuojama domeniniais aplankais (pvz. `wiki/IT/`, `wiki/legal/`, `wiki/projects/AppName/`) ir gilesniais poaplankiais pagal poreikį.

## B. Sesijos pradžia

Jei cwd yra po `{PROJECT_ROOT}`:
1. Perskaityti `{PROJECT_ROOT}/wiki/index.md`.
2. Skaityti `.claude/CLAUDE.md` failus aukštyn nuo cwd (gilesnis perrašo aukštesnį).
3. Jei cwd atitinka registruotą projektą wiki `index.md` — perskaityti to projekto wiki puslapį pilnam kontekstui.

## C. Wiki struktūros augimas

Kai domenas auga — kurti gilesnius poaplankius (pvz. `wiki/IT/docker/`, `wiki/projects/MyApp/`). Domeniniai aplankai kuriami pagal poreikį, kai atsiranda turinys.

## D. Turinio vieta

- Visas turinys rašomas į `{PROJECT_ROOT}/wiki/`
- Organizuoti pagal domeninius aplankus ir poaplankius

## E. Sesijos pabaiga

**Testas prieš rašant:** "Ar būsima Claude sesija tai turėtų žinoti, ir negali to išvesti iš esamų failų?" Jei ne — nerašyti.

Jei sesija sukūrė vertingų žinių:
1. Sukurti arba atnaujinti wiki puslapius.
2. Atnaujinti `index.md` jei pridėti/pašalinti puslapiai.
3. Pridėti sesijos santrauką į `wiki/logs/YYYY-MM/YYYY-MM-DD.md`. Veiksmažodžiai: `ingest`, `create`, `update`, `build`, `fix`, `lint`, `query`, `register`, `delete`.

**CLAUDE.md patikra:** Ar sesijoje atsirado naujų kritinių taisyklių?
Triggeriai: komanda kuri sulaužo sistemą, naujas privalomas env kintamasis,
eiliškumo priklausomybė, porto konfliktas, failas kurio negalima keisti.
Jei taip — pridėti TIK taisyklę į projekto CLAUDE.md (ne tech stack, ne struktūrą).

**README.md patikra:** Ar pasikeitė paleidimo/instaliacijos komandos?
Jei taip — atnaujinti README.md paleidimo sekciją.

**.env.example patikra:** Ar atsirado naujų env kintamųjų arba panaikinti seni?
Jei taip — atnaujinti .env.example (tik vardai, be reikšmių).

Trivialios sesijos (klausimai, maži pataisymai) — nieko nerašyti.

## F. Konteksto skaitymas

- Skaityti wiki `index.md` ir atitinkamus domeninius puslapius

## G. Lint (oportunistinis)

Nelinti pagal grafiką. Lint tik kai Claude pastebi problemų skaitydamas wiki:
- Mirę linkai → auto-fix
- Orphan puslapiai (nėra `index.md`) → auto-fix: pridėti
- Frontmatter spragos → auto-fix
- Failų vardų kolizijos → auto-fix: pridėti domeno prefiksą
- Seni puslapiai, prieštaravimai → pranešti naudotojui

Atnaujinti `last_lint` datą `index.md` po lint.

## H. Obsidian suderinamumas

**PRIVALOMA:** failų vardai turi būti unikalūs per visą wiki (pridėti domeno prefiksą jei generinis). Wikilinks veikia tiesiogiai visame wiki.

## Saugos taisyklės

- Niekada nekeisti `raw/` failų. Niekada netrinti wiki puslapių be naudotojo patvirtinimo.
- Logai yra append-only per dieną. Visada atnaujinti `updated` frontmatter redaguojant.
- Prieš kuriant puslapį, patikrinti ar toks jau egzistuoja.
