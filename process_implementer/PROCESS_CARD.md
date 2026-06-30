# PROCESS_CARD — Process Implementer

**Wersja:** 0.1  
**Trigger:** Orchestrator przekazuje artefakty z Process Architect po zatwierdzeniu PO

---

## Cel

Stworzyć strukturę repo nowej capability — wszystkie pliki i katalogi gotowe do uruchomienia PoC.

---

## Wejście

```
ARCHITECT_ARTIFACTS = katalog z artefaktami Process Architect:
  - PROCESS_CARD_[nazwa].md  (jeden per proces)
  - agents_registry.yml
  - STATE.md template
  - context.md templates (jeden per agent)
TARGET_REPO = repo gdzie to trafia (lokalny clone lub przez GitHub API)
CHARTER_PATH = ścieżka do CHARTER.md
```

---

## Kroki

### 1. WERYFIKACJA KOMPLETNOŚCI

```
Sprawdź czy masz wszystkie artefakty:
[ ] PROCESS_CARD.md dla każdego procesu z CHARTER
[ ] agents_registry.yml
[ ] STATE.md template
[ ] context.md template dla każdego agenta z enabled: false

Brakujące artefakty → zgłoś do Orchestratora (nie improwizuj)
```

### 2. UTWÓRZ STRUKTURĘ KATALOGÓW

```
[TARGET_REPO]/
├── CHARTER.md                    ← skopiuj z input
├── agents_registry.yml           ← z artefaktów Architekta
├── STATE.md                      ← z template Architekta
├── [proces_1]/
│   ├── PROCESS_CARD.md           ← z artefaktów Architekta
│   └── (context.md jeśli agent ma enabled: true)
├── [proces_2]/
│   └── PROCESS_CARD.md
└── Investors/                    ← lub odpowiednik, jeśli w CHARTER
    └── [agent_id]/
        └── context.md            ← z template Architekta
```

Nazwy katalogów: `snake_case`, zgodne z `id` w agents_registry.yml.

### 3. WYPEŁNIJ STATE.md

Zainicjuj STATE.md dla tej capability:

```markdown
## Agents Status
[Tabela z agents_registry.yml — wszystkie enabled: false, last run: —]

## Last Run
Timestamp: — (nie uruchomiono jeszcze)

## Lessons Learned
(puste — wypełni się po pierwszym PoC)
```

### 4. WERYFIKACJA STRUKTURY

```
Dla każdego pliku który stworzyłeś:
[ ] Czy ścieżka zgadza się z agents_registry.yml (context_path)?
[ ] Czy PROCESS_CARD ma placeholder STATE.md path?
[ ] Czy STATE.md ma sekcję "Agents Status" z wszystkimi agentami?
[ ] Czy CHARTER.md jest w katalogu głównym repo?
```

### 5. COMMIT

```bash
git add .
git commit -m "feat([capability-name]): bootstrap capability structure

CHARTER, agents_registry, STATE.md, PROCESS_CARDs, context.md templates.
Ready for PoC run by Process Validator.

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
```

### 6. OUTPUT SUMMARY

```
Bootstrap complete:
- Repo: [TARGET_REPO]
- Commit: [hash]
- Pliki: [lista]
- Gotowe do PoC: TAK

Instrukcja dla Process Validator:
- CHARTER.md: [ścieżka]  
- Pierwszy proces do uruchomienia: [nazwa procesu z CHARTER]
- Warunek sukcesu PoC: [AC z PROCESS_CARD]
```
