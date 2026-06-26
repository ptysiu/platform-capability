# TEST_CARD — BA Agent
**Wersja:** v0.1

---

## Scenariusz 1: Greenfield — nowa aplikacja od zera

**Setup:**
- Repo target: ptysiu/trading-capability (lub testowe)
- Brief: 1-2 zdania opisujące nową aplikację
- Brak istniejących issues powiązanych z briefem

**Kroki (A1–A10):**

| # | Krok | Oczekiwany rezultat |
|---|------|---------------------|
| A1 | LOAD CONTEXT wykonany | Agent wie co już istnieje w repo |
| A2 | Agent zadaje max 5 pytań elicytacyjnych | Pytania dotyczą celu, stakeholdera, scope, constraints, DoD |
| A3 | User odpowiada | Agent parafrazuje i potwierdza zrozumienie |
| A4 | BUSINESS NEED zapisany explicite | Widoczny w sesji przed pisaniem issues |
| A5 | SOLUTION SCOPE IN/OUT zdefiniowany | Minimum 2 pozycje w każdej kategorii |
| A6 | Minimum 3 issues napisane | Format zgodny z PROCESS_CARD sekcja 4 |
| A7 | Każdy issue przeszedł QUALITY GATE | Checklist 8 punktów bez NIE |
| A8 | Każdy issue ma ≥2 testowalne AC (Given/When/Then) | Brak AC w stylu "system powinien być dobry" |
| A9 | DEPENDENCY ORDERING podany | Przynajmniej faza 1 i 2 określone |
| A10 | SUMMARY z szacunkiem S/M/L | Wszystkie issues sklasyfikowane |

**Wynik oczekiwany:** PASS — wszystkie A1–A10 zaliczone

---

## Scenariusz 2: Refinement — rozbicie dużego issue

**Setup:**
- Istnieje issue oznaczony `needs-refinement` lub `L` (duży)
- Brief: "rozwiń issue #X"

**Kroki (B1–B6):**

| # | Krok | Oczekiwany rezultat |
|---|------|---------------------|
| B1 | Agent czyta istniejący issue | Identyfikuje co jest niejasne lub zbyt duże |
| B2 | Zadaje max 3 pytania clarification | Tylko o tym co jest ambiguous w issue |
| B3 | Oryginalny issue zostaje zamknięty lub zaktualizowany | Nie duplikuje treści |
| B4 | Powstają 2–5 mniejszych issues | Każdy ≤ M scope |
| B5 | Nowe issues mają dependency na siebie (jeśli sequential) | DAG jest poprawny |
| B6 | QUALITY GATE passed dla wszystkich nowych issues | |

---

## Scenariusz 3: Duplicate detection

**Setup:**
- W backlogu istnieje issue "feat: dodaj FMP client"
- Brief od użytkownika: "potrzebujemy klienta do FMP"

**Oczekiwane zachowanie:**
- Agent wykrywa istniejący issue
- Informuje użytkownika: "Issue #X już pokrywa ten temat — czy chcesz go rozbudować zamiast tworzyć nowy?"
- Nie tworzy duplikatu

---

## Historia uruchomień

| Run | Data | Scenariusz | Brief | Wynik | Obserwacje |
|-----|------|------------|-------|-------|------------|
| — | — | — | — | — | — |
