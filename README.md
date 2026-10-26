# 🛸 Projekt Zespołowy: X-Files Evidence Locker (CLI)

> *"Prawda jest gdzieś tam... a dokładniej w lokalnym systemie plików."*

**Zespół śledczy (uzupełnijcie przed pierwszym commitem!):**
* **Agent A (Architekt Danych & Systemu Plików):** [Wpisz Imię i Nazwisko oraz @login_github]
* **Agent B (Operatorka/Operator CLI & Logiki):** [Wpisz Imię i Nazwisko oraz @login_github]

---

## 📋 Opis Misji

Papierowe archiwa w piwnicy FBI pękają w szwach, a Syndykat regularnie wykrada nasze dokumenty. Dyrektor Skinner zlecił Waszemu dwuosobowemu zespołowi stworzenie cyfrowego, konsolowego systemu archiwizacji dowodów (**`x-locker`**). 

Waszym zadaniem jest zbudowanie aplikacji CLI w **Node.js i TypeScript**, która pozwala katalogować sprawy paranormalne, serializować dane do bazy JSON oraz generować tajne raporty w formacie Markdown.

Projekt realizujecie w **pełnym cyklu SDLC (Software Development Life Cycle)** – oceniany jest nie tylko działający kod, ale również współpraca na GitHubie, tablica Kanban, gałęzie (branches) oraz wzajemne Code Review!

---

## 🛠️ Wymagany Stos Technologiczny

**To, co już znacie (Podstawa):**
* Podział projektu na moduły (`import` / `export`).
* Moduł `path` (`join`, `resolve`, `__dirname`) – pełna kompatybilność między Windows i Linux/macOS.
* Moduł `fs` (`existsSync`, `mkdirSync`, `readFileSync`, `writeFileSync`).
* Serializacja i deserializacja danych (`JSON.stringify` / `JSON.parse`).
* Obiekt `process` (`process.argv.slice(2)`, `process.env`, `process.exit()`).

**Lekkie wykroczenie poza materiał (Do samodzielnego zbadania – "Stretch Goals"):**
* Generowanie ładnie sformatowanego raportu tekstowego `.md` za pomocą `fs.writeFileSync`.
* Użycie obiektu `new Date().toISOString()` do automatycznego zapisywania daty utworzenia akt.
* *(Opcjonalnie)* Pokolorowanie komunikatów w konsoli za pomocą wbudowanego w Node.js `util.styleText` (np. czerwony dla błędów, zielony dla sukcesu).

---

## 🚀 Krok 0: Inicjalizacja Projektu (Zanim zaczniecie!)

Ponieważ repozytorium startowe zawiera tylko ten opis, musicie wspólnie zainicjować projekt Node.js. 

**Agent A lub Agent B (tylko jedna osoba!) wykonuje na gałęzi `main`:**
1. Sklonujcie repozytorium na swój komputer (`git clone ...`).
2. Upewnijcie się, że w głównym folderze jest plik `.gitignore`, który zawiera wpisy `node_modules/`, `dist/` oraz `.env` (jeśli go nie ma – utwórzcie go!).
3. Otwórzcie terminal w folderze projektu i wpiszcie:
   ```bash
   npm init -y
   ```
4. Zainstalujcie TypeScript i narzędzia uruchomieniowe:
   ```bash
   npm install --save-dev typescript @types/node ts-node
   ```
5. Zainstalujcie paczkę do obsługi zmiennych środowiskowych:
   ```bash
   npm install dotenv
   ```
6. Zainicjujcie konfigurację TypeScriptu:
   ```bash
   npx tsc --init
   ```
7. Utwórzcie folder `src/`, plik `src/types.ts` (ze wspólnymi typami – patrz Faza 2) oraz plik `.env.example` (z wpisem `AGENT_CODE=tutaj_wpisz_kod`).
8. Utwórz plik `.gitignore`.
9. Zróbcie pierwszego commita z konfiguracją (`chore: initial project setup`) i wypchnijcie go na GitHuba (`git push`), aby druga osoba mogła go pobrać (`git pull`).

Dopiero od tego momentu przejdźcie do tworzenia zadań na tablicy Kanban i rozdzielenia pracy na osobne gałęzie!

---

## 🔄 Faza 1: Proces SDLC, Git & Kanban (Zasady Pracy w Parach)

**UWAGA:** Commity wrzucane bezpośrednio na gałąź `main` (poza Krokiem 0 i aktualizacją imion w tym pliku) są **zabraniające protokołem FBI** i obniżają ocenę!

### Krok 1: Tablica Kanban i Issues
1. W zakładce **Projects** Waszego repozytorium utwórzcie tablicę **Kanban** z kolumnami: `To Do`, `In Progress`, `Code Review`, `Done`.
2. W zakładce **Issues** utwórzcie minimum **6 zadań** (np. *Inicjalizacja bazy JSON*, *Obsługa autoryzacji .env*, *Komenda add*, *Komenda list*, *Komenda status/delete*, *Eksport raportu do .md*).
3. Przypiszcie każde zadanie (pole *Assignees*) do Agenta A lub Agenta B i podepnijcie je pod tablicę Kanban.

### Krok 2: Praca z gałęziami (Git Flow) i Pull Requesty
1. Każde zadanie wykonujcie na osobnej gałęzi, np. `feat/storage-system` lub `feat/cli-add-command`.
2. Po skończeniu zadania wypchnijcie gałąź na GitHuba i utwórzcie **Pull Request (PR)** do gałęzi `main`. Przesuńcie kartę na tablicy Kanban do kolumny `Code Review`.
3. **Zasada Czystych Rąk:** Nie wolno robić merge'a własnego Pull Requesta! Druga osoba z pary musi wejść w PR, przejrzeć kod, zostawić chociaż jeden komentarz i kliknąć **Approve**, a następnie **Merge**.

---

## 🏗️ Faza 2: Architektura Projektu (Podział na Moduły)

Aby uniknąć konfliktów w Gicie, podzielcie pracę zgodnie z poniższą strukturą plików:

```text
x-files-locker/
├── data/
│   └── cases.json          # Baza danych JSON (tworzona automatycznie przez program!)
├── reports/                # Folder na wygenerowane raporty .md (tworzony automatycznie!)
├── src/
│   ├── types.ts            # Wspólne interfejsy i typy (piszecie razem w Kroku 0)
│   ├── storage.ts          # [AGENT A]: moduły fs, path, serializacja i deserializacja
│   ├── commands.ts         # [AGENT B]: logika komend (add, list, status, delete, export)
│   └── index.ts            # [AGENT B]: punkt wejścia, process.env, routing process.argv
├── .env.example            # Szablon zmiennych środowiskowych (bez prawdziwych haseł!)
├── .gitignore              # Musi zawierać node_modules/ oraz .env!
├── package.json
├── tsconfig.json
└── README.md
```

### Kontrakt Danych (`src/types.ts`)
Zanim zaczniecie kodować osobno na własnych branchach, upewnijcie się, że w `src/types.ts` macie ten sam interfejs:

```typescript
export type ThreatLevel = "LOW" | "MEDIUM" | "EXTREME";
export type CaseStatus = "OPEN" | "CLOSED" | "REDACTED";

export interface CaseFile {
  id: string;             // np. "X-1013"
  title: string;          // np. "Potwór z Jersey"
  location: string;       // np. "Atlantic City"
  threat: ThreatLevel;
  status: CaseStatus;
  createdAt: string;      // Data wygenerowana przez new Date().toISOString()
}
```

---
## 🕵️‍♂️ Faza 3: Podział Zadań

### 👤 Zadania Agenta A: Archiwista Danych (`src/storage.ts`)
Odpowiadasz za bezpieczną komunikację z dyskiem twardym (niezależnie czy program działa na Windowsie, czy na Macu/Linuksie).

1. **Bezpieczne ścieżki (`path`):** Użyj `path.join()` lub `path.resolve()`, aby zdefiniować ścieżki do pliku `data/cases.json` oraz katalogu `reports/`. **Zero ukośników wpisanych "na sztywno"!**
2. **Automatyczna inicjalizacja (`fs`):** Napisz funkcję, która sprawdza (`fs.existsSync`), czy folder `data/` oraz plik `cases.json` istnieją. Jeśli nie (np. świeżo po pobraniu repozytorium) – program ma je automatycznie utworzyć (`fs.mkdirSync`, `fs.writeFileSync` z pustą tablicą `"[]"`).
3. **Deserializacja (`loadCases(): CaseFile[]`):** Odczytaj plik `cases.json` i zamień tekst JSON na tablicę obiektów (`JSON.parse`).
4. **Serializacja (`saveCases(cases: CaseFile[]): void`):** Zamień tablicę obiektów na sformatowany tekst (`JSON.stringify(cases, null, 2)`) i zapisz w `cases.json`.
5. **Zapis raportu (`saveMarkdownReport(id: string, content: string): string`):** Zapisz tekst raportu w folderze `reports/` pod nazwą `[id]-raport.md` (upewnij się wcześniej, że folder `reports/` istnieje!) i zwróć pełną ścieżkę absolutną do tego pliku.

### 👤 Zadania Agenta B: Operator Polowy (`src/index.ts` & `src/commands.ts`)
Odpowiadasz za przechwytywanie rozkazów z terminala, autoryzację i logikę biznesową.

1. **Autoryzacja dostępu (`process.env`):**
   * Program przy starcie sprawdza zmienną `process.env.AGENT_CODE`.
   * Jeśli zmienna nie istnieje lub jest inna niż `"TRUST_NO_ONE"`, wypisz błąd odmowy dostępu i natychmiast zamknij program z kodem błędu: `process.exit(1)`.
2. **Obsługa argumentów (`process.argv.slice(2)`):** Odczytaj komendę (pierwszy argument użytkownika) oraz pozostałe parametry i wywołaj odpowiednią funkcję z pliku `commands.ts`.
3. **Walidacja:** Jeśli użytkownik poda nieznaną komendę lub zapomni podać wymaganych argumentów (np. wywoła `add` bez tytułu sprawy), wyświetl czytelną instrukcję obsługi w konsoli.

---

## 💻 Wymagane Komendy CLI (Jak ma działać program)

### 1. `add` — Dodanie nowej sprawy
Tworzy nowy obiekt `CaseFile` z domyślnym statusem `"OPEN"` oraz aktualną datą (`new Date().toISOString()`) i dopisuje go do bazy. Waliduj, czy obiekt o takim `id` już nie istnieje!

```bash
$ npx ts-node src/index.ts add "X-404" "Diabeł z Jersey" "New Jersey" "MEDIUM"
✅ [SUKCES] Sprawa X-404 (Diabeł z Jersey) została pomyślnie dodana do archiwum.
```

### 2. `list` — Wyświetlenie akt (z opcjonalnym filtrowaniem!)
Wyświetla wszystkie sprawy z bazy. Jeśli użytkownik poda dodatkowy argument (np. `"EXTREME"` lub `"OPEN"`), wyświetl tylko te sprawy, których poziom zagrożenia lub status pasuje do filtra.

```bash
$ npx ts-node src/index.ts list
📂 ARCHIWUM X-FILES (Znaleziono: 2):
- [X-404] Diabeł z Jersey | Lokalizacja: New Jersey | Zagrożenie: MEDIUM | Status: OPEN
- [X-999] Incydent w Roswell | Lokalizacja: Nowy Meksyk | Zagrożenie: EXTREME | Status: REDACTED

$ npx ts-node src/index.ts list EXTREME
📂 ARCHIWUM X-FILES (Filtr: EXTREME | Znaleziono: 1):
- [X-999] Incydent w Roswell | Lokalizacja: Nowy Meksyk | Zagrożenie: EXTREME | Status: REDACTED
```

### 3. `status` — Zmiana statusu sprawy
Wyszukuje sprawę po `id` i zmienia jej status na `OPEN`, `CLOSED` lub `REDACTED`.

```bash
$ npx ts-node src/index.ts status "X-404" "CLOSED"
🔒 Status sprawy X-404 został zmieniony na: CLOSED
```

### 4. `delete` — Zniszczenie dowodów (Usunięcie sprawy)
Usuwa sprawę o podanym `id` z pliku `cases.json`.

```bash
$ npx ts-node src/index.ts delete "X-404"
🔥 Dowody w sprawie X-404 zostały trwale usunięte z bazy.
```

### 5. `export` — Generowanie oficjalnego raportu `.md` *(Stretch Goal)*
Wyszukuje sprawę po `id`, formatuje jej dane jako czytelny dokument Markdown i za pomocą funkcji od Agenta A zapisuje go w folderze `reports/`. Na koniec wypisuje ścieżkę absolutną do wygenerowanego pliku.

```bash
$ npx ts-node src/index.ts export "X-999"
📄 Wygenerowano tajny raport: /home/mulder/x-files-locker/reports/X-999-raport.md
```

**Przykładowa zawartość wygenerowanego pliku `reports/X-999-raport.md`:**

```markdown
# 🕵️‍♂️ TAJNY RAPORT FBI: AKTA X-999
---
* **Kryptonim sprawy:** Incydent w Roswell
* **Lokalizacja:** Nowy Meksyk
* **Poziom zagrożenia:** EXTREME
* **Aktualny status:** REDACTED
* **Data utajnienia w systemie:** 2026-10-26T19:30:00.000Z

> Dokument wygenerowany automatycznie przez system X-Locker. Nie kopiować.
```

---

## 📊 Kryteria Oceny (Checklista przed oddaniem projektu)

Przed zgłoszeniem gotowości sprawdźcie swój projekt według poniższej listy (możecie odhaczać kolejne punkty podczas pracy):

- [ ] **Tablica Kanban i Issues (20%):** Wszystkie zadania na GitHub Projects przeszły drogę do kolumny `Done`, każde ma przypisanego autora.
- [ ] **Git & Code Review (20%):** Praca na osobnych branchach, minimum 4 zmergowane Pull Requesty, w każdym PR widoczny komentarz i akceptacja (Approve) od drugiej osoby z pary. Czysta historia commitów.
- [ ] **Bezpieczeństwo i `.env` (10%):** Plik `.env` jest dodany do `.gitignore` (nie ma go w repozytorium!), a w projekcie znajduje się plik `.env.example` pokazujący, jakiej zmiennej program oczekuje. Brak poprawnego hasła blokuje program (`process.exit(1)`).
- [ ] **Modułowość i `path`/`fs` (25%):** Kod podzielony na moduły, pełne typowanie TypeScript (brak `any`), wszystkie ścieżki budowane przez moduł `path`, program sam tworzy brakujące foldery `data/` oraz `reports/`.
- [ ] **Działanie CLI i Eksport `.md` (25%):** Wszystkie 5 komend (`add`, `list`, `status`, `delete`, `export`) działa bezbłędnie i obsługuje błędne dane wejściowe (np. brak sprawy o podanym ID).




