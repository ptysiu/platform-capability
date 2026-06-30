# PROCESS_CARD — Process Validator

**Wersja:** 0.1  
**Trigger:** Orchestrator przekazuje zbootstrapowane repo + listę AC z CHARTER

---

## Cel

Uruchomić PoC nowej capability i zwalidować każde acceptance criteria — tak żeby Orchestrator mógł przedstawić PO jednoznaczny wynik PASS/FAIL.

---

## Wejście

```
REPO_PATH    = ścieżka do zbootstrapowanego repo
CHARTER_PATH = CHARTER.md z listą AC per proces
```

---

## Kroki

### 1. PRZECZYTAJ AC

```
Przeczytaj CHARTER.md — sekcja MVP.
Wypisz każde AC jako:

| ID | Proces | AC | Metoda walidacji |
|----|---------|----|-----------------|
| AC1 | [proces] | [treść] | [jak sprawdzić PASS/FAIL] |
```

Jeśli AC jest niejednoznaczne (nie ma obiektywnej metody walidacji) → zgłoś do Orchestratora PRZED uruchomieniem PoC.

### 2. PRZYGOTUJ ŚRODOWISKO

```
Sprawdź pre-conditions:
[ ] Repo ma PROCESS_CARD.md dla pierwszego procesu
[ ] STATE.md istnieje i jest zainicjowany
[ ] agents_registry.yml ma ≥1 enabled: true
[ ] context.md istnieje dla każdego enabled agenta
[ ] Wymagane MCP tools są dostępne (Notion, GitHub, WebFetch...)
[ ] Wymagane dane/queue są dostępne

Brakujące pre-conditions → zgłoś do Orchestratora
```

### 3. URUCHOM POC

```
Uruchom pierwszy proces z PROCESS_CARD.md.
Śledź każdy krok — notuj:
- Czy krok zakończył się sukcesem?
- Jeśli nie: co dokładnie zawiodło (message, tool error, brak danych)?
- Czy STATE.md został zaktualizowany po runie?
```

### 4. WALIDUJ AC

Dla każdego AC z kroku 1:

```
AC[N]: [treść]
Metoda: [jak sprawdzono]
Wynik: PASS ✅ / FAIL ❌
Dowód: [plik/output/screenshot który to potwierdza]
Uwagi: [jeśli FAIL — co konkretnie zawiodło]
```

### 5. NAPISZ TEST REPORT

```markdown
# Test Report — [Capability Name] PoC Run #[N]
**Data:** YYYY-MM-DD
**Validator:** Process Validator

## Pre-conditions
| P | Warunek | Status |
|---|---------|--------|
| P1 | ... | ✅/❌ |

## Acceptance Criteria
| AC | Treść | Wynik | Dowód |
|----|-------|-------|-------|
| AC1 | ... | ✅ PASS | ... |
| AC2 | ... | ❌ FAIL | ... |

## Wynik ogólny
**PASS** / **FAIL** (PASS = wszystkie AC ✅)

## Wnioski i rekomendacje
[Co zadziałało, co nie, co wymaga poprawki w PROCESS_CARD lub design]

## Akcje następcze
- [ ] [jeśli FAIL: co konkretnie naprawić]
```

### 6. OUTPUT

```
Przekaż do Orchestratora:
- test_reports/[YYYY-MM-DD]_poc_run[N].md
- Wynik: PASS / FAIL
- Jeśli FAIL: lista AC które nie przeszły z rekomendacją (PROCESS_CARD? Design? Bootstrap?)
```
