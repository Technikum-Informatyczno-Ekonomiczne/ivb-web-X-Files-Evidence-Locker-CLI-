# 🛸 Projekt Zespołowy: X-Files Evidence Locker (CLI)

> *"Prawda jest gdzieś tam... a dokładniej w lokalnym systemie plików. Ale uważajcie: nawet najlepszy informator czasem halucynuje."*

**Zespół śledczy (uzupełnijcie przed pierwszym commitem!):**
- **Agent A (Architekt Danych & Bezpieczeństwa Systemu Plików):** [Wpisz Imię i Nazwisko oraz @login_github]
- **Agent B (Architekt Interfejsu CLI & Walidacji Granic):** [Wpisz Imię i Nazwisko oraz @login_github]

---

## 📋 Opis Misji

Papierowe archiwa w piwnicy FBI pękają w szwach, a Syndykat regularnie wykrada nasze dokumenty. Dyrektor Skinner zlecił Waszemu dwuosobowemu zespołowi stworzenie cyfrowego, konsolowego systemu archiwizacji dowodów (**`x-locker`**). 

Waszym zadaniem jest zaprojektowanie i zbudowanie asynchronicznej aplikacji CLI w **Node.js i TypeScript**, która pozwala katalogować sprawy paranormalne, serializować dane do bazy JSON oraz generować tajne raporty w formacie Markdown.

Projekt realizujecie w **pełnym cyklu SDLC (Software Development Life Cycle)** i zgodnie z zasadami **nowoczesnej inżynierii w erze AI**. Oceniany jest nie tylko działający kod, ale przede wszystkim: zrozumienie architektury asynchronicznej, zdefiniowanie warunków brzegowych, ocena ryzyka operacji na plikach oraz rygorystyczne wzajemne Code Review!

---

## 🧠 Nowy Paradygmat: Rola Agenta-Inżyniera w Świecie AI

W dzisiejszym świecie samo „klepanie kodu” to za mało – kod potrafi wygenerować maszyna na podstawie rachunku prawdopodobieństwa. Wasza rola w tym projekcie wchodzi na **wyższy poziom abstrakcji**. Nie jesteście tylko koderami; jesteście **Architektami Systemu i Audytorami Ryzyka**:

1. **Architektura i powiązania komponentów:** Zanim powstanie pierwsza linijka kodu, musicie rozumieć, jak moduł wejścia (`index.ts`) rozmawia z logiką (`commands.ts`), a ta z asynchroniczną warstwą dyskową (`storage.ts`).
2. **Kontrakt i poziom abstrakcji:** Zarządzacie całościowym konceptem systemu poprzez ścisłe typy (`types.ts`) oraz obietnice (`Promise<T>`). Kompilator TypeScript to Wasza pierwsza linia obrony przed chaosem.
3. **Definiowanie granic (Edge Cases):** Musicie jasno określić, jakich danych i scenariuszy aplikacja **nie** akceptuje (np. uszkodzony plik JSON, brak pliku przy pierwszym uruchomieniu, duplikaty ID, puste argumenty).
4. **Świadomość konsekwencji i ryzyka:** Wasz program wykonuje operacje na prawdziwym dysku twardym (`writeFile`, usuwanie rekordów). Błędnie skonstruowana ścieżka, brak `await` przed zapisem lub brak walidacji może uszkodzić bazę danych albo wyprowadzić pliki poza folder projektu!
5. **Obsługa braku determinizmu (Weryfikacja):** Jeśli korzystacie z podpowiedzi narzędzi AI, traktujcie je jak niepewnego informatora FBI. AI generuje tekst statystycznie i nie ponosi odpowiedzialności za błędy. **To Wy odpowiadacie za każdą linijkę kodu w Pull Requeście i musicie potrafić wyjaśnić, jak ona działa.**

---

## 🛠️ Wymagany Stos Technologiczny

**Fundamenty (Musicie rozumieć każdy z tych elementów):**
- Podział projektu na moduły (`import` / `export`) – separacja odpowiedzialności (Separation of Concerns).
- Moduł `node:path` (`join`, `resolve`, `__dirname`) – pełna kompatybilność między Windows i Linux/macOS oraz ochrona przed wyjściem poza katalog roboczy.
- Asynchroniczny moduł `node:fs/promises` (`mkdir`, `readFile`, `writeFile`, `access`) oraz składnia `async / await` i obsługa błędów `try / catch`. **Zakaz używania metod synchronicznych (`*Sync`) blokujących wątek główny!**
- Serializacja i deserializacja danych (`JSON.stringify` / `JSON.parse`).
- Obiekt `process` (`process.argv.slice(2)`, `process.env`, `process.exit()`).

**Lekkie wykroczenie poza materiał (Do samodzielnego zbadania – "Stretch Goals"):**
- Generowanie ładnie sformatowanego raportu tekstowego `.md` za pomocą asynchronicznego `writeFile`.
- Użycie obiektu `new Date().toISOString()` do automatycznego zapisywania daty utworzenia akt.
- *(Opcjonalnie)* Pokolorowanie komunikatów w konsoli za pomocą wbudowanego w Node.js `util.styleText` (np. czerwony dla błędów, zielony dla sukcesu).

---

## 🚀 Krok 0: Inicjalizacja Projektu (Zanim zaczniecie!)

Ponieważ repozytorium startowe zawiera tylko ten opis, musicie wspólnie zainicjować projekt Node.js i ustalić fundamenty architektoniczne.

**Agent A lub Agent B (tylko jedna osoba!) wykonuje na gałęzi `main`:**
1. Sklonujcie repozytorium na swój komputer (`git clone ...`).
2. Upewnijcie się, że w głównym folderze jest plik `.gitignore`, który zawiera wpisy `node_modules/`, `dist/` oraz `.env` (jeśli go nie ma – utwórzcie go!).
3. Otwórzcie terminal w folderze projektu i wpiszcie:
   ```bash
   npm init -y
   ```
4. Zainstalujcie TypeScript oraz nowoczesne narzędzie uruchomieniowe `tsx`:
   ```bash
   npm install --save-dev typescript @types/node tsx
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
8. Zróbcie pierwszego commita z konfiguracją (`chore: initial project setup`) i wypchnijcie go na GitHuba (`git push`), aby druga osoba mogła go pobrać (`git pull`).

Dopiero od tego momentu przejdźcie do tworzenia zadań na tablicy Kanban i rozdzielenia pracy na osobne gałęzie!

---

## 🔄 Faza 1: Proces SDLC, Git & Audyt Kodu (Zasady Pracy w Parach)

**UWAGA:** Commity wrzucane bezpośrednio na gałąź `main` (poza Krokiem 0 i aktualizacją imion w tym pliku) są **zabronione protokołem FBI** i obniżają ocenę!

### Krok 1: Tablica Kanban i Issues
1. W zakładce **Projects** Waszego repozytorium utwórzcie tablicę **Kanban** z kolumnami: `To Do`, `In Progress`, `Code Review`, `Done`.
2. W zakładce **Issues** utwórzcie minimum **6 zadań** (np. *Asynchroniczna inicjalizacja bazy JSON i obsługa błędów*, *Obsługa autoryzacji .env*, *Komenda add + walidacja warunków brzegowych*, *Komenda list*, *Komenda status/delete*, *Bezpieczny eksport raportu do .md*).
3. Przypiszcie każde zadanie (pole *Assignees*) do Agenta A lub Agenta B i podepnijcie je pod tablicę Kanban.

### Krok 2: Praca z gałęziami (Git Flow) i Audyt w Pull Requestach
1. Każde zadanie wykonujcie na osobnej gałęzi, np. `feat/storage-system` lub `feat/cli-add-command`.
2. Przed wypchnięciem kodu upewnijcie się, że Wasz kod nie ma błędów typowania (ponieważ `tsx` uruchamia kod bez weryfikacji typów, sprawdźcie projekt komendą `npx tsc --noEmit`).
3. Wypchnijcie gałąź na GitHuba i utwórzcie **Pull Request (PR)** do gałęzi `main`. Przesuńcie kartę na tablicy Kanban do kolumny `Code Review`.
4. **Zasada Czystych Rąk i Weryfikacji Determinizmu:** Nie wolno robić merge'a własnego Pull Requesta! Druga osoba z pary występuje w roli **Audytora Bezpieczeństwa**. Musi przejrzeć kod, sprawdzić czy nie ma w nim „halucynacji” (np. brakujących słów kluczowych `await`), przetestować warunki brzegowe, zostawić merytoryczny komentarz i dopiero kliknąć **Approve** oraz **Merge**.

---

## 🏗️ Faza 2: Architektura Systemu i Kontrakt Danych

Aby uniknąć konfliktów w Gicie i zachować czysty podział odpowiedzialności, podzielcie pracę zgodnie z poniższą architekturą:

```text
x-files-locker/
├── data/
│   └── cases.json          # Baza danych JSON (tworzona automatycznie przez program!)
├── reports/                # Folder na wygenerowane raporty .md (tworzony automatycznie!)
├── src/
│   ├── types.ts            # [WSPÓLNE]: Kontrakt danych – interfejsy i typy
│   ├── storage.ts          # [AGENT A]: Asynchroniczna warstwa dyskowa (fs/promises, path, serializacja)
│   ├── commands.ts         # [AGENT B]: Asynchroniczna warstwa logiki biznesowej i walidacji granic
│   └── index.ts            # [AGENT B]: Warstwa wejścia (CLI), autoryzacja process.env, routing process.argv
├── .env.example            # Szablon zmiennych środowiskowych (bez prawdziwych haseł!)
├── .gitignore              # Chroni przed wyciekiem .env oraz folderu node_modules/
├── package.json
├── tsconfig.json
└── README.md
```

### Przepływ danych w systemie (Powiązania komponentów):
`Terminal Użytkownika` ➔ `index.ts` (Autoryzacja i funkcja `async main()`) ➔ `await commands.ts` (Walidacja danych i logika) ➔ `await storage.ts` (Bezpieczny odczyt/zapis przez `fs/promises` i `path`) ➔ `Dysk Twardy (cases.json / reports/*.md)`

> **Zasada Architektoniczna:** Plik `commands.ts` **nie może** bezpośrednio importować modułu `fs/promises`! Wszelkie operacje na plikach muszą przechodzić przez asynchroniczne funkcje wyeksportowane z `storage.ts`.

### Kontrakt Danych (`src/types.ts`)
Zanim zaczniecie kodować osobno na własnych branchach, upewnijcie się, że w `src/types.ts` macie ten sam, nienaruszalny kontrakt:

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

## 🕵️‍♂️ Faza 3: Podział Zadań, Granice i Zarządzanie Ryzykiem

### 👤 Zadania Agenta A: Architekt Danych i Systemu Plików (`src/storage.ts`)
Odpowiadasz za asynchroniczną, nieblokującą komunikację z dyskiem twardym oraz ochronę danych przed uszkodzeniem. Korzystaj wyłącznie z modułu `node:fs/promises` oraz `node:path`.

1. **Bezpieczne ścieżki (`path`):** Użyj `path.join()` lub `path.resolve()`, aby zdefiniować ścieżki do pliku `data/cases.json` oraz katalogu `reports/`. **Zero ukośników wpisanych "na sztywno"!**
2. **Automatyczna inicjalizacja (`ensureStorage()`):** Napisz funkcję `async`, która upewnia się, że folder `data/` oraz plik `cases.json` istnieją:
   * Użyj `await mkdir(dataDir, { recursive: true })` (opcja `recursive: true` nie rzuci błędu, jeśli folder już istnieje!).
   * Sprawdź dostępność pliku `cases.json` (np. przez `await access(casesFilePath)` w bloku `try / catch`). Jeśli plik nie istnieje – utwórz go za pomocą `await writeFile(casesFilePath, "[]", "utf-8")`.
3. **Deserializacja i odporność na błędy (`loadCases(): Promise<CaseFile[]>`):** Odczytaj plik `cases.json` (`await readFile(...)`) i zamień tekst JSON na tablicę obiektów (`JSON.parse`).
   * 🛡️ **Warunek brzegowy / Ryzyko:** Co się stanie, jeśli ktoś ręcznie wyczyści plik `cases.json` (będzie pustym stringiem `""`) lub wpisze tam błędny JSON? `JSON.parse` rzuci wyjątek! Zabezpiecz odczyt blokiem `try / catch` – w razie uszkodzenia pliku wypisz ostrzeżenie w konsoli i zwróć pustą tablicę `[]` (lub zakończ program kontrolowanym komunikatem).
4. **Serializacja (`saveCases(cases: CaseFile[]): Promise<void>`):** Zamień tablicę obiektów na sformatowany tekst (`JSON.stringify(cases, null, 2)`) i zapisz asynchronicznie w `cases.json` (`await writeFile(...)`).
5. **Bezpieczny zapis raportu (`saveMarkdownReport(id: string, content: string): Promise<string>`):** Upewnij się, że folder `reports/` istnieje (`await mkdir(..., { recursive: true })`), zapisz raport pod nazwą `[id]-raport.md` i zwróć pełną ścieżkę absolutną do tego pliku.
   * 🛡️ **Świadomość konsekwencji (Path Traversal):** Co jeśli złośliwy użytkownik poda jako ID sprawy ciąg `"../../Windows/system32"` albo `"../package"`? Zanim utworzysz plik, oczyść `id` ze znaków specjalnych (np. ukośników `/` i `\`), aby raport **zawsze** lądował wyłącznie wewnątrz folderu `reports/`!

### 👤 Zadania Agenta B: Architekt CLI i Walidacji Granic (`src/index.ts` & `src/commands.ts`)
Odpowiadasz za przechwytywanie rozkazów z terminala, autoryzację, obsługę asynchronicznego przepływu sterowania i pilnowanie, by do bazy danych nie trafiły śmieci.

1. **Autoryzacja dostępu (`process.env`):**
   * Program przy starcie ładuje zmienne środowiskowe (`import "dotenv/config"`) i sprawdza `process.env.AGENT_CODE`.
   * Jeśli zmienna nie istnieje lub jest inna niż `"TRUST_NO_ONE"`, wypisz błąd odmowy dostępu i natychmiast zamknij program z kodem błędu: `process.exit(1)`.
2. **Asynchroniczny punkt wejścia (`src/index.ts`):**
   * Odczytaj komendę oraz pozostałe parametry z `process.argv.slice(2)`.
   * Stwórz główną funkcję `async function main()`, w której wywołasz (z użyciem `await`!) odpowiednią funkcję z pliku `commands.ts`. Owiń wywołanie w globalny blok `try / catch`, aby żaden nieobsłużony błąd (`UnhandledPromiseRejection`) nie wywalił brzydkiego śladu stosu na ekran użytkownika.
3. **Definiowanie granic i warunków brzegowych (`src/commands.ts`):**
   * Każda funkcja obsługująca komendę (np. `addCase`, `listCases`) musi być oznaczona jako `async` i zwracać `Promise<void>`, ponieważ wewnątrz czeka (`await`) na dane z warstwy `storage.ts`.
   * 🛡️ **Granica typów:** Użytkownik w terminalu może wpisać cokolwiek (np. poziom zagrożenia `"SUPER_GROŹNE"` zamiast `"EXTREME"`, albo status `"ZAMKNIĘTE"` zamiast `"CLOSED"`). TypeScript sprawdza typy tylko podczas kompilacji – w czasie działania programu (`runtime`) musisz samodzielnie sprawdzić, czy podany tekst należy do dozwolonych wartości `ThreatLevel` / `CaseStatus`!
   * 🛡️ **Brakujące dane:** Jeśli użytkownik poda nieznaną komendę lub zapomni podać wymaganych argumentów (np. wywoła `add` bez lokalizacji lub z pustym stringiem `""`), zablokuj operację i wyświetl czytelną instrukcję obsługi.

---

## 💻 Wymagane Komendy CLI (Specyfikacja Zachowania Systemu)

### 1. `add` — Dodanie nowej sprawy
Asynchronicznie pobiera bazę, tworzy nowy obiekt `CaseFile` z domyślnym statusem `"OPEN"` oraz aktualną datą (`new Date().toISOString()`) i zapisuje zaktualizowaną tablicę na dysku.
* **Warunek brzegowy:** Waliduj, czy obiekt o takim `id` już nie istnieje oraz czy poziom zagrożenia to dokładnie `"LOW"`, `"MEDIUM"` lub `"EXTREME"`.

```bash
$ npx tsx src/index.ts add "X-404" "Diabeł z Jersey" "New Jersey" "MEDIUM"
✅ [SUKCES] Sprawa X-404 (Diabeł z Jersey) została pomyślnie dodana do archiwum.
```

### 2. `list` — Wyświetlenie akt (z opcjonalnym filtrowaniem!)
Asynchronicznie wczytuje i wyświetla wszystkie sprawy z bazy. Jeśli użytkownik poda dodatkowy argument (np. `"EXTREME"` lub `"OPEN"`), wyświetl tylko te sprawy, których poziom zagrożenia lub status pasuje do filtra.
* **Warunek brzegowy:** Jeśli baza jest pusta lub żadna sprawa nie pasuje do filtra, wyświetl czytelny komunikat (np. `📭 Brak akt spełniających kryteria.`), zamiast pustego ekranu.

```bash
$ npx tsx src/index.ts list
📂 ARCHIWUM X-FILES (Znaleziono: 2):
- [X-404] Diabeł z Jersey | Lokalizacja: New Jersey | Zagrożenie: MEDIUM | Status: OPEN
- [X-999] Incydent w Roswell | Lokalizacja: Nowy Meksyk | Zagrożenie: EXTREME | Status: REDACTED

$ npx tsx src/index.ts list EXTREME
📂 ARCHIWUM X-FILES (Filtr: EXTREME | Znaleziono: 1):
- [X-999] Incydent w Roswell | Lokalizacja: Nowy Meksyk | Zagrożenie: EXTREME | Status: REDACTED
```

### 3. `status` — Zmiana statusu sprawy
Wyszukuje sprawę po `id`, zmienia jej status na `OPEN`, `CLOSED` lub `REDACTED` i czeka na zapis zmian w pliku.
* **Warunek brzegowy:** Obsłuż sytuację, gdy sprawa o podanym `id` nie istnieje lub nowy status jest spoza dozwolonej listy.

```bash
$ npx tsx src/index.ts status "X-404" "CLOSED"
🔒 Status sprawy X-404 został zmieniony na: CLOSED
```

### 4. `delete` — Zniszczenie dowodów (Usunięcie sprawy)
Usuwa sprawę o podanym `id` i asynchronicznie nadpisuje plik `cases.json`.
* **Świadomość ryzyka:** Upewnij się, że funkcja usuwa wyłącznie wskazany rekord, a nie nadpisuje całej bazy pustą tablicą! Jeśli sprawa o podanym `id` nie istnieje – poinformuj o tym użytkownika i nie wykonuj zbędnego zapisu na dysk.

```bash
$ npx tsx src/index.ts delete "X-404"
🔥 Dowody w sprawie X-404 zostały trwale usunięte z bazy.
```

### 5. `export` — Generowanie oficjalnego raportu `.md` *(Stretch Goal)*
Wyszukuje sprawę po `id`, formatuje jej dane jako czytelny dokument Markdown i za pomocą asynchronicznej funkcji od Agenta A zapisuje go w folderze `reports/`. Na koniec wypisuje ścieżkę absolutną do wygenerowanego pliku.

```bash
$ npx tsx src/index.ts export "X-999"
📄 Wygenerowano tajny raport: /home/mulder/x-files-locker/reports/X-999-raport.md
```

**Przykładowa zawartość wygenerowanego pliku `reports/X-999-raport.md`:**

```markdown
# 🕵️‍♂️ TAJNY RAPORT FBI: AKTA X-999
---
- **Kryptonim sprawy:** Incydent w Roswell
- **Lokalizacja:** Nowy Meksyk
- **Poziom zagrożenia:** EXTREME
- **Aktualny status:** REDACTED
- **Data utajnienia w systemie:** 2026-10-26T19:30:00.000Z

> Dokument wygenerowany automatycznie przez system X-Locker. Nie kopiować.
```

---

## 📊 Kryteria Oceny (Checklista Architekta przed oddaniem projektu)

Przed zgłoszeniem gotowości sprawdźcie swój projekt według poniższej listy (możecie odhaczać kolejne punkty podczas pracy):

- [ ] **Tablica Kanban i Issues (15%):** Wszystkie zadania na GitHub Projects przeszły drogę do kolumny `Done`, każde ma przypisanego autora.
- [ ] **Git, Code Review i Weryfikacja (20%):** Praca na osobnych branchach, minimum 4 zmergowane Pull Requesty. W każdym PR widoczny komentarz sprawdzający warunki brzegowe i akceptacja (Approve) od drugiej osoby z pary. Projekt przechodzi test `npx tsc --noEmit` bez błędów. **Każdy członek zespołu potrafi wyjaśnić przy tablicy/komputerze działanie dowolnej linijki swojego kodu.**
- [ ] **Bezpieczeństwo, Świadomość Ryzyka i `.env` (15%):** Plik `.env` jest w `.gitignore`, w repozytorium jest `.env.example`. Brak poprawnego hasła blokuje program (`process.exit(1)`). Operacje zapisu plików są zabezpieczone przed uszkodzeniem bazy i wyjściem poza folder docelowy (*Path Traversal*).
- [ ] **Architektura Asynchroniczna, Modułowość i `path`/`fs/promises` (25%):** Ścisła separacja warstw (`commands.ts` nie wywołuje bezpośrednio `fs/promises`), pełna asynchroniczność (`async/await`, zero metod `*Sync`!), pełne typowanie TypeScript (zero typu `any`!), wszystkie ścieżki budowane przez moduł `path`, program sam tworzy brakujące foldery `data/` oraz `reports/`.
- [ ] **Warunki Brzegowe i Działanie CLI (25%):** Wszystkie 5 komend (`add`, `list`, `status`, `delete`, `export`) działa bezbłędnie przez `npx tsx` i elegancko obsługuje błędne dane wejściowe (duplikaty ID, nieznane poziomy zagrożenia, uszkodzony plik JSON, brak sprawy o podanym ID).
