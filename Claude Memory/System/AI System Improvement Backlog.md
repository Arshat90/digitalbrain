---
type: system
created: 2026-05-01
updated: 2026-06-11
tags:
  - claude-memory
  - system
  - backlog
  - ai-workflow
---

# AI System Improvement Backlog

Backlog приоритизированных улучшений к личному AI-стеку Arshat'а — извлечён из критического разбора (сессия `local_71ab0c64-e583-4d43-8751-5f6fcaa4d31e`, 2026-05-01). Это **наблюдения и рекомендации**, не задачи в исполнении. Решение о приоритете и старте — за пользователем.

## Контекст

Текущая позиция системы: **верхние 1–2% пользователей Claude** (архитектор персонального AI-стека). Сильные стороны не трогать: append-driven доктрина, peer-level register, многоязычность как первый класс задачи, разделение Index = карта vs Sync State = журнал, bug-callouts с гипотезами (post-mortem-уровень дисциплины). Слабые места — ниже.

## P0 — высший приоритет

> [!success] AI ROI Ledger — **выполнено 2026-05-02**
> [[AI ROI Ledger]] создан с retro-fill за 25 апр → 1 мая: **9 deliverables, 84.25 ч сэкономлено**. Топ-3: Хасбулатов production (34 ч), Backdrop v2 (17 ч), Хасбулатов pre-prod (10 ч). Q2 Summary (partial) готова. Weekly digest scheduled на понедельники; Q2 close — 30 июня (одностраничная сводка для Акционера в формате [[Annual Report Framework]]).

> [!success] Verification layer — **выполнено 2026-05-02**
> [[Verification Protocol]] создан: триггеры по уровням критичности, Pass A (subagent fact-check с готовым 9-пунктным промпт-шаблоном для копирования), Pass B (24h-delayed human review с 7-пунктным чек-листом, вкл. read-aloud / boss test / numbers re-derive), crisis-mode для deadline <24h, Mistakes Log для пропущенных ошибок. Интегрирован с [[AI ROI Ledger]] (учёт времени Pass A+B) и с playbook'ами (каждый playbook должен включать verification trigger).

## P1 — высокий приоритет

> [!success] Push-капча + autostart — **выполнено 2026-05-02**
> P1 закрыт. Четыре компонента архитектуры:
> - **(a) Self-push доктрина** (сессия `local_f25a5b7e`) — [[Memory Sync Protocol]] §«Push captures» добавлен: формат `^push-YYYY-MM-DD-THHMMZ`, триггеры, workflow, спецификация реконсилиации по `session: <prefix>`, anti-patterns. [[Profile]] §«Работа с vault» дополнен директивой self-push для всех Cowork-сессий с writes в vault.
> - **(b) Autostart через shell:startup** (сессия `local_f25a5b7e`) — `Obsidian.lnk` в `shell:startup` запускает Obsidian + REST API plugin со входом пользователя; даже после Windows reboot REST API доступен к моменту cron.
> - **(c) Пилотный push-блок 2026-05-02** (сессия `local_f25a5b7e`) — `^push-2026-05-02-T0743Z` в [[Sync State]] закрывает дыру дня и служит первым тестом формата.
> - **(d) Реконсилиация в `daily-memory-sync` v3.2** (сессия `local_77524444`) — prompt scheduled-task'а получил шаги 3a (PUSH SCAN: regex по `^push-...session:\s*([0-9a-f]{8})`), 3b (PUSH-CAPTURED → skip CAPTURE), 3c (CAPTURE для не-skipped); шаг 5 RUN HISTORY one-liner расширен note'ом про push-captured prefixes; шаг 6 STATE требует full session_ids push-captured сессий в `sync_captured_recent`. Без этого шага cron создавал бы silent duplicates.
>
> Гипотеза scheduler serialization (Run 4 2026-05-01) больше не блокирует: даже если cron не отработает — push-блок уже зафиксирован самой сессией; а если отработает — реконсилиация v3.2 не задублирует.
>
> Pass A validation (2026-05-02) поймала разрыв между описанием реконсилиации в [[Memory Sync Protocol]] §«Push captures» и реализацией в `daily-memory-sync` prompt; устранено в v3.2. Первая mistake-запись — [[Verification Protocol#^mistake-2026-05-02-recon-gap]].

> [!success] Ежемесячный consolidate-memory — **выполнено 2026-05-01**
> Scheduled task `consolidate-memory` создан (сессия `local_88d86e00`, cron `0 9 1 * *`). Первый автозапуск — 1 июня 2026. Логика: архивировать Run History >90 дней → `Sync Run Archive.md`, проверять 200-line cap в Index, дедупликация wikilinks, обновлять Sync State frontmatter. Рекомендация: прогнать вручную один раз для pre-approve разрешений obsidian-rest MCP.

> [!todo] Извлечь 3 промпта в playbook'и для команды
> Знание сконцентрировано на пользователе. Все 9 активных проектов — персональные пайплайны. Точка следующего скачка: **извлечь 3–5 повторяющихся промптов** в общедоступные playbook'и или n8n-флоу для команды. Кандидаты:
> - 4C-анализ нового рыночного объекта
> - Перевод KZ-маркетинговых материалов RU↔KK с сохранением tone-of-voice
> - Differentiation Instagram vs LinkedIn для одного и того же события
>
> «Не отдавай людям сам Claude — отдавай готовые рабочие места поверх Claude.»

> [!todo] Model routing: Opus для критичных deliverables
> Из efficiency-анализа (сессия `local_f70497a1`, 2026-05-02): 38 сессий идут через Sonnet. Opus 4.6 подключать для 🔴-материалов — письма Президенту/Набсовету, M&A-документы, где цена hallucination высокая. ~~Haiku для daily-memory-sync уже настроен~~
> **UPD 2026-06-11 — Haiku на scheduled-задачах признан недостаточным.** Доказательная база: under-capture 06-09→06-11 (три прогона «zero-capture» при 10 substantive-сессиях в окне) + фантомный отчёт «FULL РЕВИЗИЯ v4.0» 06-09 при фактическом v3.5 на диске. Экономика: ~80% стоимости прогона — input-токены (одинаковы для всех моделей), брак догоняется тяжёлыми catch-up-прогонами. Решение пользователя: `daily-memory-sync` и `weekly-roi-digest` → **Sonnet**; `consolidate-memory` (1×/мес) — Sonnet или Opus. Архитектурные guard'ы против under-capture независимо от модели — daily-memory-sync v3.6 (zero-capture guard, инварианты 14–17), см. [[Memory Sync Protocol]] §Историческая справка «Миграция v3.6».

> [!bug] patch_note(section: heading) — ongoing workaround drain
> Баг зафиксирован с Run 7 (2026-05-01), подтверждён Run 1 и Run 3 (2026-05-02). Каждый вызов требует workaround через `replace_in_note` — это +1–2 RTT на операцию. До починки со стороны obsidian-rest MCP: применять `replace_in_note` сразу, не делать first-attempt через patch. Отслеживать обновления obsidian-rest сервера.
> **UPD 2026-06-07:** frontmatter-половина проблемы закрыта миграцией на `obsidian_manage_frontmatter` (daily-memory-sync v3.5, consolidate-memory v1.2) после schema-validation сбоя sync 2026-06-06. Heading-часть: replace_in_note остаётся default для Run History/Changelog; patch_note(heading) не перепроверялся.

## P2 — средний приоритет

> [!todo] Eval-набор для повторяющихся задач
> Для каждого playbook'а: 5–10 эталонных входов, ожидаемые свойства выхода, скоринг. Запускать раз в квартал. Когда обновляется модель Claude или переписывается промпт — сразу видно, стало лучше или хуже. Без этого «летим без приборов»: Annual Report v1 vs v2, Backdrop v2 vs v2.1 — сравнения нет, кроме субъективного «лучше».

> [!warning] Privacy-классификация
> AMC-переговоры с Ecosign, M&A-контекст, NS-only финансовый слой Backdrop, compliance-биография акционера — всё идёт через cloud Claude. Нет следов классификации: что идёт в облако, что — никогда. Для холдинга такого масштаба это не теоретический риск.
> - Минимум: список «не отдавать в облако никогда» (имена бенефициаров с цифрами, точные суммы M&A, медицинские/юридические детали) — 1 страница
> - Максимум: локальная Llama/Qwen 32B для tier-1-sensitive операций с тем же промптом-стеком

> [!todo] Decision log для negotiation-проектов
> Для [[AMC - Almaty Mountain Cluster]], [[AGS Brand Development]], [[Ecole Ducasse Almaty Studio]] — после каждой ключевой сессии в project-файл добавляется блок: *рассмотренные опции, выбранная, почему, остаточный риск, точка пересмотра*. Превратит память из «что произошло» в «почему мы здесь».

## P3 — отложенные upgrade'ы (Q2)

Все три — $0 затрат, заметный compound-эффект:

- **obsidian-bases пилот** (~1 день) — `Projects.base` как замена ручному списку «Активные проекты» в Index
- **json-canvas пилот** (~1 день) — [[Hoshin Kanri X-matrix]] или [[4C Analysis]] как 2D-canvas
- **obsidian-cli fallback** (~полдня) — native Obsidian CLI как fallback при таймаутах REST API

> [!todo] **obsidian-rest block-section bug — periodic retest**
> `obsidian_get_note(format: "section", section: {type: "block", target: "<block-id>"})` возвращает `Structured content does not match the tool's output schema: data must have required property 'result', data must NOT have additional properties` для любого block-ID (проверено на `^push-2026-05-02-T0909Z` и `^push-2026-05-02-T0852Z` — баг не привязан к конкретному блоку). Heading-секции через `format: "section"` работают нормально — задеты только block-секции. Вероятно баг в cyanheads obsidian-mcp-server wrapper'е либо в Local REST API plugin'е.
>
> Обнаружено 2026-05-02 при дизайне Verify-step для push-капчи (W4); discovery-блок: [[Sync State#^push-2026-05-02-T0909Z]].
>
> **Workaround:** `obsidian_search_notes(mode: "text", query: "<block-id>", pathPrefix: "Claude Memory/System/Sync State")` — block-ID уникален в vault, 1 hit = блок на месте. Прописан в [[Memory Sync Protocol#Push captures]] (Workflow Шаги 2/3, H3 Verify, Anti-pattern bullet) + задокументирован в [[Memory Sync Protocol#API]] как «Известный баг (2026-05-02)».
>
> **Что отслеживать:** при следующем апгрейде obsidian-rest MCP / Local REST API plugin перепроверить `obsidian_get_note(format: "section", section: {type: "block", target: "..."})`. Если починилось — переключить Verify обратно на `get_note(block)` (более прямой метод, не зависит от глобальной уникальности block-ID); workaround оставить как fallback.
>
> Не блокирующий — push-капча работает на workaround стабильно. **Retest 2026-06-07:** баг всё ещё воспроизводится (get_note(block) → schema mismatch), workaround остаётся.

> [!success] **Deferred tool list ↔ obsidian-rest API misalignment — RESOLVED 2026-06-07**
> Endpoint обновился: `list_notes`, `manage_frontmatter`, `manage_tags`, `delete_note` реально доступны (live-верификация 2026-06-07: идемпотентный manage_frontmatter set на Sync State; list_notes — паритет с jsonlogic glob 6/6). Skills мигрированы: daily-memory-sync v3.5, consolidate-memory v1.2; доктрина обновлена ([[Memory Sync Protocol]], [[Obsidian Markdown Conventions]]).
> Исходная запись (2026-05-02): ToolSearch показывал `obsidian_list_notes` и `obsidian_manage_frontmatter` как доступные с полными schemas, но реальный obsidian-rest endpoint их не предоставлял. Misalignment жил на стороне Cowork-инфраструктуры или MCP-протокольного слоя — не локальный баг vault'а или плагина.
>
> Обнаружено 2026-05-02 при работе над S1 (consolidate-memory v1.0 → v1.1) при попытке использовать deferred tools для архивации; discovery-блок: [[Sync State#^push-2026-05-02-T105902Z]].
>
> **Mitigation:** перед использованием любого `obsidian_*` метода, особенно если он не встречается в [[Memory Sync Protocol#API]] или recent push-блоках, сверять с authoritative source — `daily-memory-sync` SKILL.md §«Активный API». Альтернативно — пробный вызов с минимальными аргументами и проверка ответа. Документировано в [[Memory Sync Protocol#API]] (запись от 2026-05-02).
>
> **Что отслеживать:** при следующих апгрейдах Cowork / obsidian-rest MCP — перепроверить, синхронизировался ли deferred catalog с реальным endpoint'ом. Если catalog починился — это сигнал о починке соответствующего слоя инфраструктуры. Cross-link: [[Memory Sync Protocol#API]] (запись от 2026-05-02), [[Sync State#^push-2026-05-02-T105902Z]] (S1 discovery).
>
> Не блокирующий — проявляется только при попытке вызвать устаревший метод; pattern-mismatch отлавливается на этапе Self-Pass-A.

> [!success] **obsidian-rest dataview mode wrapper bug — PATCHED 2026-05-09**
> `obsidian_search_notes(mode: "dataview", query: "TABLE WHERE startswith(file.path, ...)")` возвращает `Structured content does not match the tool's output schema: data must have required property 'result', data must NOT have additional properties` для базовых DQL-запросов листинга папок. Наблюдалось Run 3 2026-05-02 и Run 1 2026-05-06 — повторяемый, не связан с конкретным запросом.
>
> **Workaround применён как основной метод (PATCH APPLIED 2026-05-09):** `mode: "jsonlogic"` с `{"glob": ["Claude Memory/<папка>/**", {"var": "path"}]}` — порядок аргументов: pattern first, var вторым. `daily-memory-sync` SKILL.md обновлён до v3.3 через Desktop Commander (2026-05-09): (1) Шаг 2 — оба dataview-запроса заменены на jsonlogic glob; (2) §«Активный API» таблица «Листинг папки» переписана на jsonlogic; (3) §«Жёсткие инварианты» bullet 9 «Листинг папок — только jsonlogic» добавлен; (4) «Тулы НЕ существуют» дополнен known bug (2026-05-09). `update_scheduled_task` из scheduled-task сессии по-прежнему заблокирован — использован Desktop Commander как workaround.
>
> **Что отслеживать:** при следующем апгрейде obsidian-rest MCP или Local REST API plugin'а — перепроверить прямой DQL-вызов; если починилось — dataview более выразителен для сложных фильтров (WHERE по frontmatter, сортировка), jsonlogic оставить как fallback. Cross-link: [[Memory Sync Protocol#API]] (запись 2026-05-06).
>
> Не блокирующий — jsonlogic покрывает все текущие сценарии листинга.

> [!quote] Stylistic warning
> «Легко перегнуть и начать оптимизировать то, что и так работает.» Не трогать: append-driven доктрину; peer-level register; многоязычность; разделение Index/Sync State; bug-callouts с гипотезами в Run 3.

## Связанное

- [[Memory Sync Protocol]] — append-driven доктрина и workflow
- [[Sync State]] — текущий watermark и аномалии (включая scheduler-serialization гипотезу)
- [[Profile]] — позиция marketing-функции как profit center
