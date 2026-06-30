# PROCESS_CARD — Process Architect

**Wersja:** 0.1  
**Trigger:** Orchestrator przekazuje zatwierdzony CHARTER.md

---

## Cel

Zaprojektować kompletny zestaw komponentów procesu dla nowej capability — tak żeby Process Implementer mógł je stworzyć bez dodatkowych pytań, a Process Validator mógł uruchomić PoC.

---

## Wejście

```
CHARTER_PATH = ścieżka do zatwierdzonego CHARTER.md
TARGET_REPO  = repo gdzie capability zostanie zbudowana
```

---

## Kroki

### 1. ANALIZA CHARTER

```
Przeczytaj CHARTER.md. Zidentyfikuj:
- Ile procesów ma capability? (każdy proces = osobna sekcja w CHARTER MVP)
- Ile agentów per proces?
- Jakie są inputs/outputs każdego procesu?
- Jakie MCP tools są potrzebne? (Notion, GitHub, WebFetch, Bash...)
- Czy capability ma materiały źródłowe (queue, vault, database)?
```

### 2. ZAPROJEKTUJ PROCESS_CARD dla każdego procesu

Dla każdego procesu zidentyfikowanego w CHARTER napisz PROCESS_CARD.md:

```markdown
# PROCESS_CARD — [Nazwa Procesu]

## Cel
[1 zdanie]

## Trigger
[Co uruchamia ten proces]

## Input
| Wejście | Źródło | Opis |
|---|---|---|

## Output
[Co powstaje, gdzie trafia]

## E2E Workflow
[Kroki numerowane z krokami 0. LOAD STATE i N. UPDATE STATE]

## Skille agenta
| Skill | Tool | Uwagi |
|---|---|---|

## Acceptance Criteria (PoC)
[Lista mierzalnych kryteriów — każde PASS/FAIL bez interpretacji]
```

### 3. ZAPROJEKTUJ agents_registry.yml

```yaml
# Jeden wpis per agent. Format:
- id: [snake_case]
  name: [czytelna nazwa]
  context_path: [ścieżka do context.md]
  enabled: true/false
  status: active/pending_context
  role: [1-2 zdania]
```

### 4. ZAPROJEKTUJ context.md template dla każdego agenta

Dla każdego agenta w registry (z enabled: false i status: pending_context) dostarcz template:

```markdown
# [Imię/Nazwa] — Agent Context

## Kim jestem
[Persona, filozofia, styl myślenia]

## Co cenię w materiałach / zadaniach
[Co jest relevantne przez pryzmat tego agenta]

## Czego nie uznaję
[Co odrzucam, co jest poza scope]

## Moje kluczowe narzędzia myślowe
[Mental models, frameworki, heurystyki]

## Pytania które zadaję materiałom / zadaniom
[Przez jakie pytania filtruję input]
```

### 5. ZAPROJEKTUJ STATE.md template

Bazuj na wzorcu z quant-knowledge-mgmt/STATE.md. Sekcje:
- Process Info (statyczne)
- Last Run (dynamiczne)
- Agents Status (dynamiczne)
- Failed / Escalated (persistent)
- Lessons Learned (persistent, rośnie)
- Next Run Checklist (reset per run)
- Run History (archiwum)

### 6. QUALITY GATE

Przed przekazaniem do Orchestratora sprawdź:

```
[ ] Każdy proces z CHARTER ma swój PROCESS_CARD?
[ ] Każdy PROCESS_CARD ma kroki 0. LOAD STATE i N. UPDATE STATE?
[ ] Każde AC w PROCESS_CARD jest obiektywnie PASS/FAIL?
[ ] agents_registry.yml ma wszystkich agentów z CHARTER?
[ ] STATE.md template ma sekcję Lessons Learned?
[ ] context.md template dla każdego agenta z enabled: false?
```

### 7. OUTPUT SUMMARY

```
Artefakty gotowe do Bootstrap:
- PROCESS_CARD.md: [lista procesów]
- agents_registry.yml: [N agentów]
- STATE.md template: [ścieżka]
- context.md templates: [lista agentów]

Do decyzji PO (jeśli są):
- [ewentualne open questions architektoniczne]
```
