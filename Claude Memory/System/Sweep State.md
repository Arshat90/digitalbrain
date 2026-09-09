---
type: system
created: 2026-09-04
updated: 2026-09-09
tags:
  - claude-memory
  - system
  - sync-state
  - auto-managed
sweep_last_run_at: 2026-09-09T04:20:27Z
sweep_last_run_status: success
sweep_unmatched: []
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

## Sweep Runs

- 2026-09-07T04:13:42Z — run OK; -Since param anomaly (2026-09-04 used instead of 2026-09-05T14:22:17Z); 9 files returned, all pre-watermark; effective new=0; unmatched=0
- 2026-09-08T04:27:36Z — run OK; -Since 2026-09-07T04:13:42Z; 16 files; 4 clusters: (A) ФК Кайрат 1xBET — ДС №6 PDF подтверждён; (B) ФК Кайрат Halyk — план переговоров v1; (C) Маркетинг ЦКП 2026 — v5 pptx; (D) Medeu Park Hotel — выбор ивент-агентства Fashion Bureau vs Insider; unmatched=0 (narod_attachment_links.html + Рисунок1.png отфильтрованы)

- 2026-09-09T04:20:27Z — run OK; -Since 2026-09-08T04:27:36Z; 20 files; 3 clusters: (A) Маркетинг ЦКП 2026 — v6 pptx + именная версия + Методика показателей v1 docx; (B) Ritz-Carlton Almaty — Протокол переговоров GAP v1 + Итоги первого раунда v1; (C) Медео Парк Отель — grand opening PDF 08.09 + 22.09 (date shift); unmatched=0

## Связанное

- [[Memory Sync Protocol]] — протокол основной синхронизации
- [[Sync State]] — состояние и история `daily-memory-sync`
- [[ФК Кайрат — 1xBET]] — кейс, из-за которого задача появилась
