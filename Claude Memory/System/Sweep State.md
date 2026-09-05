---
type: system
created: 2026-09-04
updated: 2026-09-05
tags:
  - claude-memory
  - system
  - sync-state
  - auto-managed
sweep_last_run_at: 2026-09-05T14:22:17Z
sweep_last_run_status: success
sweep_unmatched:
  - APS-AED-MIN №24 от 31.08.2026 года.docx
  - Alienkind_razbor_v1.docx
  - Kamshat Bekturgan_CV.pdf
  - 970506451012-20260810150813235.pdf
---

# Sweep State

Авто-управляется задачей `deliverables-sweep` (SKILL.md — `C:\Users\LENOVO\Documents\Claude\Scheduled\deliverables-sweep\`). Не редактировать вручную.

## Зачем существует

`daily-memory-sync` обходит **сессии** через `mcp__session_info__list_sessions` — а туда попадают только локальные сессии Claude на этой машине. Облачные чаты claude.ai в этот список не входят: их транскрипты остаются на сервере. Всё, что сделано в облачном чате, для основной синхронизации невидимо.

Инцидент, из-за которого задача создана: переговоры [[ФК Кайрат — 1xBET]] (27.07–19.08.2026, 26 документов, задолженность 422,5 млн ₸) не оставили в vault ни одной записи. Файлы при этом лежали в `Downloads` всё время.

`deliverables-sweep` закрывает этот разрыв с другой стороны: обходит **файлы**, а не сессии.

## State (frontmatter)

- `sweep_last_run_at` — UTC ISO, watermark по времени изменения файлов
- `sweep_last_run_status` — `success` | `partial` | `failed`
- `sweep_unmatched` — пути, не отнесённые ни к одному проекту (FIFO, до 20)

## Область обхода

- `C:\Users\LENOVO\Downloads`
- `C:\Users\LENOVO\Documents\Claude\Projects`
- `C:\Users\LENOVO\Desktop`

Исключаются: `image-gen-package`, `fal_gen_*`, `_tmp_*`, `~$*`, служебные каталоги. Trading-файлы ведёт `trading-evening-review`.

## Run History

### 2026-09-04

- **Init** ^sweep-init-2026-09-04 — задача создана, `sweep.ps1` протестирован вручную: за окно 01.09–04.09 отдал 25 файлов после фильтрации шума (155 до фильтров). Watermark выставлен на 2026-09-01T00:00:00Z, чтобы первый прогон подобрал последние дни.

## Связанное

- [[Memory Sync Protocol]] — протокол основной синхронизации
- [[Sync State]] — состояние и история `daily-memory-sync`
- [[ФК Кайрат — 1xBET]] — кейс, из-за которого задача появилась
