# PROCESS_CARD — Capability Orchestrator

**Wersja:** 0.1  
**Trigger:** PO podaje brief nowej capability (1-3 zdania)

---

## Cel

Przeprowadzić nową capability przez wszystkie fazy Capability Development workflow — od briefu do zwalidowanego PoC — koordynując subagentów i bramkując PO w kluczowych momentach.

---

## Wejście

```
CAPABILITY_BRIEF = brief od PO (nazwa, cel, 1-3 zdania)
TARGET_REPO      = repo gdzie capability zostanie zbudowana
                   (np. ptysiu/trading-capability, ptysiu/quant-knowledge-mgmt)
```

---

## Workflow

```
0. LOAD STATE
   └── Read STATE.md
       → czy ta capability już istnieje? (sprawdź sekcję "Capabilities w budowie")
       → jakie lessons learned dotyczą podobnych capabilities?
       → jeśli istnieje i jest IN PROGRESS: wznów od ostatniej fazy

═══ FAZA 1: CHARTER ═══════════════════════════════════════════════════════════

1. SPAWN BA_AGENT
   └── Przekaż: CAPABILITY_BRIEF, TARGET_REPO
       BA Agent prowadzi sesję elicytacyjną z PO (max 5 pytań)
       BA Agent produkuje: CHARTER.md draft

   ┌─ PO GATE #1 ─────────────────────────────────────────────────────────────
   │  Pokaż PO: CHARTER.md draft
   │  Pytanie: "Czy CHARTER poprawnie opisuje cel i MVP? Zatwierdzasz?"
   │  → TAK: kontynuuj do Fazy 2
   │  → NIE (z komentarzem): wróć do BA Agent z feedback → popraw CHARTER
   └──────────────────────────────────────────────────────────────────────────

═══ FAZA 2: DESIGN ════════════════════════════════════════════════════════════

2. SPAWN PROCESS_ARCHITECT
   └── Przekaż: zatwierdzone CHARTER.md
       Architekt projektuje:
       - PROCESS_CARD.md dla każdego procesu w capability
       - agents_registry.yml
       - STATE.md template
       - context.md template dla każdego agenta

   ┌─ PO GATE #2 ─────────────────────────────────────────────────────────────
   │  Pokaż PO: listę PROCESS_CARDs i agents_registry
   │  Pytanie: "Czy ten design capture'uje to co potrzebujesz? Zatwierdzasz?"
   │  → TAK: kontynuuj do Fazy 3
   │  → NIE: wróć do Architekta z feedback
   └──────────────────────────────────────────────────────────────────────────

═══ FAZA 3: BOOTSTRAP ═════════════════════════════════════════════════════════

3. SPAWN PROCESS_IMPLEMENTER
   └── Przekaż: artefakty z Fazy 2, TARGET_REPO
       Implementer tworzy strukturę repo i wszystkie pliki
       Wynik: repo gotowe do uruchomienia PoC

═══ FAZA 4: VALIDATE ══════════════════════════════════════════════════════════

4. SPAWN PROCESS_VALIDATOR
   └── Przekaż: zbootstrapowane repo, CHARTER.md (AC lista)
       Validator uruchamia PoC i waliduje każde AC
       Wynik: test report PASS/FAIL per AC

   ┌─ PO GATE #3 ─────────────────────────────────────────────────────────────
   │  Pokaż PO: test report
   │  → WSZYSTKIE AC PASS: capability osiąga L3 (Validated) → idź do kroku 5
   │  → CZĘŚĆ AC FAIL: przedstaw PO failure summary
   │     Pytanie: "Poprawiamy design (→ Faza 2) czy akceptujemy z ograniczeniami?"
   └──────────────────────────────────────────────────────────────────────────

═══ FAZA 5: GRADUATE ══════════════════════════════════════════════════════════

5. UPDATE STATE
   └── Zaktualizuj STATE.md:
       - przesuń capability z "W budowie" do "Operacyjne"
       - zapisz datę graduation, wersję, link do test report
       - dodaj lessons learned z tego cyklu
       - sprawdź: czy ≥3 cykle PASS → zaproponuj PO automatyzację loopa (L4)

6. SUMMARY
   └── Raport dla PO:
       - Nowa capability: [nazwa] — status L3 Validated
       - CHARTER: [link]
       - Repo: [link]
       - Test report: [link do pass/fail]
       - Maturity level: L3 (next: L4 wymaga ≥3 cyklów PASS + auto trigger)
```

---

## Zasady orkiestracji

- **Każdy subagent ma swój PROCESS_CARD** — nie dawaj mu instrukcji ad-hoc; powołaj go i wskaż PROCESS_CARD
- **Przekaż artefakty explicite** — każdy subagent dostaje konkretne pliki jako input, nie abstrakcyjny "kontekst poprzedniej fazy"
- **Gate = stop i czekaj na PO** — nie przechodzi dalej bez zatwierdzonego artefaktu. Użyj AskUserQuestion
- **STATE.md jest źródłem prawdy** — zawsze zacznij od READ STATE, zawsze zakończ UPDATE STATE
