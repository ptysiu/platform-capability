# CHARTER — Capability Development

**Wersja:** 0.1  
**Data:** 2026-06-30  
**Capability Owner:** Rafał Białowąs  
**Process Owner:** Capability Orchestrator (instancja Claude Code)

---

## Cel (1 zdanie)

Autonomiczne projektowanie, implementowanie i walidowanie nowych zdolności biznesowych — tak żeby każda nowa capability mogła zostać uruchomiona jako działający loop w jednej sesji roboczej.

---

## Metodyka i rytm

- **Metodyka:** iteracyjna — każda capability przechodzi przez fazy Charter → Design → Bootstrap → Validate, z bramkami PO między fazami
- **Rytm:** on-demand — Rafał inicjuje nową capability briefem; reszta jest autonomiczna do następnej bramki
- **Self-development trigger:** capability osiąga Poziom 1 dojrzałości → zaczyna sama siebie rozwijać bez inicjacji PO

---

## MVP — co dostarcza ta capability (v0.1)

1. **CHARTER.md** dla nowej capability — cel, metodyka, MVP, roster agentów
2. **PROCESS_CARD.md** dla każdego procesu w nowej capability
3. **Zwalidowany PoC** — ≥1 pełny cykl procesu z PASS na acceptance criteria

---

## Roster agentów

| Agent | Rola | Status |
|---|---|---|
| Capability Orchestrator | Zarządza flow między agentami, bramkuje PO | v0.1 |
| BA Agent | Elicytacja wymagań, CHARTER draft, PROCESS_CARD requirements | v0.1 (aktywny) |
| Process Architect | Projektuje komponenty procesu (agenci, STATE.md, context.md) | v0.1 |
| Process Implementer | Tworzy pliki i strukturę repo nowej capability | v0.1 |
| Process Validator | Uruchamia PoC, waliduje AC, pisze test report | v0.1 |

---

## Lokalizacja vault

- **Repo:** `ptysiu/platform-capability`
- **Vault wiedzy:** `finance-second-brain/Capabilities/` (każda capability ma własny folder)
- **State loop:** `platform-capability/STATE.md`

---

## Maturity Model

| Poziom | Kryterium | Co się odblokuje |
|---|---|---|
| **L0: Concept** | Brief od PO istnieje | Uruchomienie Orchestratora |
| **L1: Designed** | CHARTER + PROCESS_CARDs zatwierdzone przez PO | Uruchomienie Implementera |
| **L2: Bootstrapped** | Repo structure + agents_registry + context.md pliki istnieją | Uruchomienie Validatora |
| **L3: Validated** | PoC PASS na wszystkich AC | Capability operacyjna |
| **L4: Self-Developing** | ≥3 cykle PASS + loop zautomatyzowany | Capability rozwija się bez inicjacji PO |
