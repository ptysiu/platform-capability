# platform-capability

Meta-procesy platformy quant trading — obsługują inne capability repos.

## Architektura

```
platform-capability/
├── ba_agent_process/          ← Business Analyst Agent
│   ├── PROCESS_CARD.md        ← jak agent działa
│   ├── TEST_CARD.md           ← jak testujemy
│   └── context.md             ← persona BA (BABOK + corporate architect)
├── process_designer_process/  ← (przyszłość) projektuje PROCESS_CARDs
└── dev_lead_process/          ← (przyszłość) code review, arch decisions
```

## Zależności

```
platform-capability → (obsługuje) → trading-capability
platform-capability → (obsługuje) → quant-knowledge-mgmt
platform-capability → (obsługuje) → [przyszłe capability repos]
```

`platform-capability` nie zależy od żadnego repo domenowego.

## BA Agent — szybki start

BA Agent przekształca brief w GitHub Issues gotowe do implementacji.

**Trigger:** ręczny — zacznij sesję Claude Code w tym repo i uruchom PROCESS_CARD.

**Input:** brief od użytkownika (1-3 zdania) + sesja elicytacyjna (max 5 pytań)
**Output:** GitHub Issues z pełną specyfikacją (Business Value, AC, Technical Notes, Dependencies)
