# PROCESS_CARD — BA Agent
**Wersja:** v0.1  
**Repo:** platform-capability  
**Trigger:** ręczny — użytkownik inicjuje sesję z briefem lub tematem

---

## Cel procesu

Przekształcić brief (1-3 zdania od użytkownika) w kompletny zestaw GitHub Issues z pełną specyfikacją — gotowych do implementacji bez dodatkowych pytań.

Output quality standard: *"Czy junior developer może wziąć ten issue i zaimplementować go bez zadawania pytań?"* Jeśli nie → issue nie jest gotowe.

---

## Kontekst wejściowy

```
REPO_TARGET    = repo dla którego piszemy issues (np. ptysiu/trading-capability)
BRIEF          = opis od użytkownika (może być 1 zdanie, może być konwersacja)
```

---

## Kroki procesu

### 0. LOAD CONTEXT — zrozum landscape

```
Przeczytaj:
  - PROCESS_CARD*.md w $REPO_TARGET (jakie procesy już istnieją?)
  - gh issue list --repo $REPO_TARGET --state open (co jest już w backlogu?)
  - README.md / CLAUDE.md (architektura, konwencje)
  - Otwarte issues powiązane z briefem (unikaj duplikatów)

Zidentyfikuj:
  - Capability landscape: jakie zdolności biznesowe już istnieją?
  - Gap: co brakuje?
  - Ryzyko kolizji z istniejącymi issues
```

### 1. ELICITATION — sesja z użytkownikiem

**BABOK: Elicitation & Collaboration Knowledge Area**

Wejdź w tryb rozmowy. Zadaj pytania elicytacyjne — maksymalnie jedna runda, maksymalnie 5 pytań. Wybierz tylko te które są niezbędne do zdefiniowania scope:

```
PYTANIA ELICYTACYJNE (wybierz relevantne):

1. [Cel biznesowy]
   "Jaki problem rozwiązujemy? Co się dzieje dzisiaj i co powinno 
   dziać się po wdrożeniu?"

2. [Stakeholder]
   "Kto używa tego systemu/procesu? Agent, użytkownik, zewnętrzny 
   system?"

3. [Scope boundary]
   "Co jest definitywnie out of scope? Czego NIE budujemy w tej 
   iteracji?"

4. [Constraints]
   "Czy są ograniczenia: technologia, czas, zależności od innych 
   procesów?"

5. [Definition of Done]
   "Skąd będziemy wiedzieć że to działa? Jak wygląda sukces?"
```

Zanim zadasz pytania — sprawdź czy brief już na nie odpowiada. Jeśli tak, pomiń pytanie.

Zapisz odpowiedzi jako:
```
BUSINESS NEED: [co i dlaczego]
STAKEHOLDERS: [kto]
SOLUTION SCOPE IN: [lista]
SOLUTION SCOPE OUT: [lista]
SUCCESS METRICS: [jak mierzymy sukces]
CONSTRAINTS: [ograniczenia]
```

### 2. STRATEGY ANALYSIS

**BABOK: Strategy Analysis Knowledge Area**

```
Current State:  co istnieje dziś (procesy, dane, narzędzia)?
Future State:   co chcemy osiągnąć?
Gap:            co musi powstać żeby przejść z current → future?

Capability Map: 
  - Które business capabilities realizuje to rozwiązanie?
  - Czy nowa capability, czy rozszerzenie istniejącej?
  - Jakie value stream obsługuje?
```

### 3. REQUIREMENTS DECOMPOSITION

Podziel na cztery typy wymagań (BABOK):

```
BUSINESS REQUIREMENTS
  Cel organizacyjny — dlaczego to robimy?
  Przykład: "Umożliwić agentowi autonomiczne tworzenie issues bez 
  przerywania użytkownika"

STAKEHOLDER REQUIREMENTS  
  Czego potrzebuje każdy stakeholder?
  Format: "Jako [rola] chcę [capability] żeby [business outcome]"

SOLUTION REQUIREMENTS — FUNCTIONAL
  Jak system ma się zachować? Co musi robić?
  Lista konkretnych zachowań, nie implementacji.

SOLUTION REQUIREMENTS — NON-FUNCTIONAL
  Jak dobrze? Performance, security, maintainability, reliability.
  Tylko te które są istotne — nie wymuszaj NFRs na siłę.

TRANSITION REQUIREMENTS
  Co potrzeba żeby wdrożyć? Migracje danych, konfiguracja, szkolenie?
```

Każde wymaganie → kandydat na issue lub część issue.

### 4. WRITE ISSUES

Dla każdego issue użyj poniższego formatu. Twórz issues przez GitHub API (`gh issue create` lub curl).

```markdown
## Business Value

Jako [rola] chcę [capability] żeby [business outcome].

## Scope

**In scope:**
- [konkretna funkcjonalność 1]
- [konkretna funkcjonalność 2]

**Out of scope:**
- [co explicite nie wchodzi w tę iterację]

## Acceptance Criteria

- [ ] Given [kontekst/stan początkowy]
      When [akcja/trigger]
      Then [oczekiwany rezultat — konkretny, mierzalny]
- [ ] Given ...
      When ...
      Then ...

## Technical Notes

[Zależności techniczne, ograniczenia implementacyjne, edge cases,
 sugestie architektoniczne — nie prescribe implementation, ale daj 
 context który pomoże developerowi podjąć właściwe decyzje]

## Dependencies

- #[numer] — [dlaczego musi być zrobione najpierw]

## NFRs

[Tylko jeśli istotne: performance, security, observability]

## Open Questions

[Pytania które pozostały nierozstrzygnięte — do wyjaśnienia przed implementacją]
```

**Konwencja tytułu:** `feat/fix/chore/refactor: [co robimy] ([komponent])`

**Labele:** `feature` | `bug` | `chore` | `backlog` | `blocked` | `needs-refinement`

### 5. QUALITY GATE

Dla każdego napisanego issue wykonaj checklist:

```
[ ] Czy developer może zaimplementować bez zadawania pytań?
[ ] Czy każde AC ma obiektywną odpowiedź PASS/FAIL?
[ ] Czy scope IN i OUT jest explicite?
[ ] Czy business value jest jasne (po co to robimy)?
[ ] Czy dependencies są zidentyfikowane?
[ ] Czy issue jest niezależny lub ma jasne prereqs?
[ ] Czy NFRs są konkretne (jeśli są)?
[ ] Czy open questions są zapisane (nie ukryte)?
```

Jeśli którykolwiek = NIE → popraw issue przed przejściem dalej.

### 6. DEPENDENCY ORDERING

```
Ułóż issues w DAG (directed acyclic graph):
  - Które issues są foundational (muszą być pierwsze)?
  - Które można robić równolegle?
  - Co jest critical path do MVP?

Format:
  Faza 1 (foundation):    #X, #Y
  Faza 2 (równolegle):    #A || #B || #C  
  Faza 3 (integracja):    #Z (wymaga #A + #B)
```

### 7. SUMMARY

```
Podsumowanie sesji:

BUSINESS NEED: [1 zdanie]
ISSUES CREATED: [lista z #numerami i tytułami]
SCOPE ESTIMATE:
  S (< 4h):  [lista]
  M (4-16h): [lista]
  L (> 16h): [lista — rozważ dalszy podział]

SUGGESTED SPRINT ORDER: [kolejność implementacji]
OPEN RISKS: [co może pójść nie tak]
OPEN QUESTIONS: [co wymaga decyzji przed/w trakcie implementacji]
```

---

## Zasady jakości

- **Jeden issue = jedna decyzja implementacyjna.** Jeśli developer musi podjąć >1 istotną decyzję architektoniczną — podziel.
- **Nie prescribe implementation.** Issue definiuje *co* i *dlaczego*, nie *jak*. Technical Notes dają context, nie instrukcję.
- **Ambiguity jest błędem.** Każde niejednoznaczne wymaganie to potencjalny rework.
- **Duplikaty są błędem.** Sprawdź istniejące issues przed stworzeniem nowego.

---

## Historia uruchomień

| Run | Data | Repo target | Brief | Issues created | Status |
|-----|------|-------------|-------|----------------|--------|
| — | — | — | — | — | — |
