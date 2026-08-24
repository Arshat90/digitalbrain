---
type: system
created: 2026-04-23
updated: 2026-06-11
tags:
  - claude-memory
  - system
  - protocol
---

# Memory Sync Protocol

Протокол синхронизации памяти Claude с Obsidian-vault `brain`. **Append-driven цифровой мозг** — vault обогащается, информация не удаляется бессознательно, история сохраняется в естественных местах (Changelog проектов, Run History в Sync State).

С 2026-04-26 vault работает через **`obsidian-mcp-server` (cyanheads) + плагин Local REST API**. С 2026-05-01 (v3) дата-онли файлы упразднены; Run-meta живёт в [[Sync State]] `## Run History`.

## Цель

`Claude Memory/` — единый источник правды по профилю, проектам, фреймворкам, истории и аудит-следу прогонов. Каждая сессия Claude:

1. Читает `00 - Index.md` → карта контекста
2. Подгружает релевантные `Projects/*` по сигналам
3. По итогу расширяет карточки (Changelog) или создаёт новые; run-meta фиксирует в [[Sync State]] `## Run History`

## API

Стек: `mcp__obsidian-rest__*`. **Canonical API reference — в prompt `daily-memory-sync` §«Активный API»** — там полная таблица тулов, паттерны, примеры кода.

Специфика, которой нет в prompt:
- `obsidian_delete_note` существует с обновления сервера (верифицировано 2026-06-07), но политика прежняя: НЕ вызывать — удаления только по явному запросу пользователя / в Obsidian UI
- Известный баг (2026-05-01): `patch_note(section: {type: "heading"})` возвращает malformed response → используй `replace_in_note` для body-правок по заголовкам. **UPD 2026-06-07:** frontmatter-записи мигрированы с `patch_note(frontmatter)` на `obsidian_manage_frontmatter` (после schema-validation сбоя sync 2026-06-06); `patch_note(frontmatter)` больше не использовать.
- Известный баг (2026-05-02): `obsidian_get_note(format: "section", section: {type: "block", target: "..."})` возвращает tool-output schema mismatch (`Structured content does not match the tool's output schema`) для block-секций — вероятно баг в cyanheads MCP wrapper'е либо Local REST API plugin'е. Verify push-блоков делать через `obsidian_search_notes(mode: "text", query: "<block-id>", pathPrefix: "Claude Memory/System/Sync State")` (block-ID уникален в vault, hit = блок на месте). Heading-секции через `format: "section"` работают корректно.
- Известный баг (2026-05-06): `obsidian_search_notes(mode: "dataview", query: "TABLE WHERE startswith(file.path, ...)")` — wrapper schema-mismatch (`Structured content does not match the tool's output schema`) для DQL-запросов листинга папок (наблюдалось в Run 3 2026-05-02 и Run 1 2026-05-06). Drop-in замена — `mode: "jsonlogic"` с `{"glob": ["Claude Memory/<папка>/**", {"var": "path"}]}` (порядок: pattern первым, var вторым — обратный порядок выдаёт hits []). **PATCH APPLIED 2026-05-09:** `daily-memory-sync` SKILL.md v3.2 → v3.3 обновлён через Desktop Commander (write_file) из user-driven сессии: Шаг 2 — оба dataview-запроса заменены на jsonlogic glob; таблица «Листинг папки» переписана; инвариант 9 добавлен. Backlog callout переведён в [!success]. Детали — [[AI System Improvement Backlog#P3 — отложенные upgrade'ы (Q2)|Backlog §P3]].
- Известный баг (2026-05-02, deferred tool list misalignment) — **RESOLVED 2026-06-07**: endpoint обновился, `obsidian_list_notes`, `obsidian_manage_frontmatter`, `obsidian_manage_tags`, `obsidian_delete_note` реально доступны (live-верификация: manage_frontmatter — идемпотентный set на Sync State; list_notes — паритет с jsonlogic glob 6/6). Skills мигрированы: daily-memory-sync v3.5, consolidate-memory v1.2. Исходная запись: ToolSearch показывал эти тулы как доступные, но endpoint их не предоставлял. Authoritative source о реальном API — `daily-memory-sync` SKILL.md §«Активный API». Перед использованием любого `obsidian_*` метода, особенно если он не встречается в Memory Sync Protocol или recent push-блоках, **сверять с активным API** (или пробный вызов с минимальными аргументами и проверка ответа). Misalignment живёт на стороне Cowork-инфраструктуры — не локальный баг.

## Append-driven доктрина

### 1. Знание расширяется в карточках, не в дата-файлах

Любое новое знание идёт В существующую карточку проекта (Changelog), либо в новый файл проекта. **Дата-онли файлы (`YYYY-MM-DD.md`) запрещены** — они не несут доменного смысла, замусоривают граф и плодят wikilink'и без семантики.

Run-meta (timestamps, anomalies, какие сессии захвачены) — единственное исключение: оно живёт в [[Sync State]] `## Run History` (один файл, append-only записи под date headings).

### 2. Project files — внутренний Changelog внизу

Каждый `Projects/*.md` — фиксированная верхняя часть (current state) + секция `## Changelog` внизу. Новый entry — сверху секции (обратная хронология), с block-ID `^cl-YYYY-MM-DD`:

```markdown
## Changelog

### YYYY-MM-DD — <короткий заголовок> ^cl-YYYY-MM-DD

- ...
```

Добавление: `patch_note(section: {type: "heading", target: "Changelog"}, operation: "prepend")` — или fallback через `replace_in_note(search: "## Changelog\n\n", replace: ...)` (workaround известного бага).

Frontmatter `updated` — через `obsidian_manage_frontmatter(operation: "set", key: "updated", value: "YYYY-MM-DD")` (с 2026-06-07; ранее — patch_note(frontmatter)).

### 3. Index — стабильная карта

`00 - Index.md` трогается только при **структурных событиях**: новый/завершённый/переименованный проект, новый System-файл, изменение one-liner описания.

Метод: `replace_in_note` для строки + `obsidian_manage_frontmatter` для `last_reviewed` / `updated`. **Лимит: < 200 строк** — детали в topic-файлах.

### 4. Completed Deliverables — append-only

Каждое завершение деливерабла — новая секция снизу через `obsidian_append_to_note`.

### 5. Profile, Frameworks — статика

`Profile.md` — меняется только по явному сигналу пользователя.
`Frameworks/*` — новый кейс → дописать в «Применения»; новая методология → новый файл.

### 6. Sync State — auto-managed

[[Sync State]] обновляется ТОЛЬКО задачей `daily-memory-sync`. Frontmatter — через `obsidian_manage_frontmatter` (с 2026-06-07). Тело (`## Run History`) — через `replace_in_note` (append строки под заголовком даты). Архивация Run History → `consolidate-memory` (1-го числа каждого месяца).

### 7. Snapshots — дисконтинуированы

`_Snapshots/` отменена 2026-04-26 после миграции на cyanheads. История — в `.obsidian/trash/` + File Recovery плагин.

## Структура vault (v3)

```
Claude Memory/
├── 00 - Index.md                   ← карта (не журнал)
├── Profile.md                      ← роль, стиль, форматы, ожидания от Claude
├── System/
│   ├── Memory Sync Protocol.md     ← этот файл
│   ├── Obsidian Markdown Conventions.md
│   └── Sync State.md               ← watermark + Run History (auto-managed)
├── Projects/                       ← активные проекты (project/active)
├── Frameworks/                     ← методологии
└── History/
    ├── Completed Deliverables.md   ← завершённые артефакты, append-only
    └── Sync Run Archive.md         ← archived Run History (auto, consolidate-memory)
```

## Watermark и pending captures

Watermark в [[Sync State]] frontmatter решает три проблемы: двойные захваты, потерянные running-сессии, неизвестный Run N.

### Поля Sync State frontmatter

- `sync_last_run_at` — UTC ISO timestamp
- `sync_last_run_status` — `success` | `partial` | `failed`
- `sync_runs_today` / `sync_runs_today_date` — счётчик в текущий день
- `sync_captured_recent` — последние ~100 captured session IDs (FIFO truncate; v3.6 с 2026-06-11, до этого ~50)
- `sync_pending_capture` — running сессии, ожидают завершения
- `last_anomalies` — bash unavailable / search_replace 0-match / REST timeouts / gap-дни / scheduler serialization

### Классификация сессий

| Статус | В captured_recent? | В pending_capture? | Действие |
|--------|---------------------|---------------------|----------|
| idle | да | — | SKIP |
| idle | нет | да | CAPTURE → перенести в captured_recent |
| idle | нет | нет | CAPTURE → добавить в captured_recent |
| running | — | да | DEFER (оставить в pending) |
| running | — | нет | DEFER (добавить в pending) |
| description = «Daily memory sync» | — | — | SKIP |

### Self-discovery System/

`obsidian_list_notes(path: "Claude Memory/System", depth: 1, extension: "md")` (с 2026-06-07; fallback — jsonlogic glob) → список System-файлов → `get_note` каждый. Новые System-файлы подхватываются автоматически без правки промпта.

### Резилиентность

- **Bash unavailable** — retry раз, fallback на `list_scheduled_tasks().lastRunAt`; аномалия
- **REST timeout** — retry раз; `replace_in_note` идемпотентен; перед повтором `append_to_note` — проверить через `get_note(format: "section")`
- **`replace_in_note` → `totalReplacements: 0`** — аномалия, фиксировать в `last_anomalies`
- **Gap-дни** — не создавать ретро-записи; дельты периода в текущий Run, gap → anomalies
- **Scheduler serialization** (гипотеза 2026-05-01) — Cowork scheduler возможно не запускает scheduled-task пока есть active user-сессия

### Инварианты v3

- Дата-онли файлы (`YYYY-MM-DD.md`) — не создавать
- Sync State `## Run History` — append-only; прошлые даты не редактировать
- `Profile.md` — никогда без явного сигнала пользователя
- Strikethroughs `~~[[...]]~~` вне Историческая справка — anomaly
- Перед записью в vault — следовать [[Obsidian Markdown Conventions]]

## Push captures

Самозахват сессии в [[Sync State]] не дожидаясь утреннего `daily-memory-sync`. Закрывает root-cause гэп-дней (см. [[AI System Improvement Backlog]] §P1): scheduled task требует одновременно ноут + Obsidian + REST API plugin — push-капча требует их только **в момент записи**, а не в фиксированное время cron'а.

### Когда пушить

Cowork-сессия классифицирует себя по 4 bright-line триггерам. Если сработал хотя бы один → **перед финальным сообщением пользователю** дописывает push-блок в [[Sync State#Run History]] под сегодняшним заголовком даты. Один push на сессию, даже если применимы несколько триггеров — все они просто перечисляются в `Deliverables` / `Anomalies` push-блока.

**Триггер 1 — Vault write.** Сессия выполнила хотя бы один `obsidian_write_note` / `obsidian_replace_in_note` / `obsidian_append_to_note` / `obsidian_patch_note` в vault `brain`.

- Пример: правка [[Memory Sync Protocol]], добавление push-блока, создание новой заметки в `Projects/`, апдейт `## Changelog` в проектном файле.
- Не считается: чтение через `obsidian_get_note` / `obsidian_search_notes` / `obsidian_list_notes`.

**Триггер 2 — Deliverable saved.** Сессия создала или существенно отредактировала файл-артефакт, который пользователь будет открывать вне Cowork (документы, изображения, скрипты, презентации, видео-материалы).

- Пример: годовой отчёт `.docx`, презентация `.pptx`, готовый AI-портрет `.png`, n8n-flow JSON, экспортированный CSV. (Push-блок в [[Sync State]] — это Триггер 1, а не 2.)
- Не считается: временные файлы в `outputs/` для собственных нужд сессии (промежуточный draft, который не дошёл до пользователя).
- **Граница «существенно»:** ≥30% изменений в файле или ≥50 строк нового контента; меньше — fixup, не deliverable.

**Триггер 3 — External system action.** Сессия выполнила действие во внешней системе с long-lasting эффектом.

- Пример: создан scheduled task через `mcp__scheduled-tasks__create_scheduled_task`, отправлено сообщение в Slack/Gmail, изменён календарь (`create_event` / `update_event` / `delete_event`), выполнен commit/PR в репозиторий, нажат «Send» в форме через computer-use.
- Не считается: read-only вызовы внешних MCP (`search_threads`, `get_event`, `list_calendars` и пр.).

**Триггер 4 — Architectural/process decision committed.** Сессия и пользователь вместе приняли решение, которое меняет, как будут работать будущие сессии — даже если в этот момент vault не правился.

- Пример: «решили использовать `host.docker.internal` вместо `127.0.0.1` для curl-проверки REST», «решили переходить на obsidian-bases в Q3», «договорились, что AGS-проект ведём в отдельном space'е».
- **Если триггер 4 сработал, но vault не правился — обязанность сессии записать само решение в vault (Profile / Backlog / соответствующий decision-лог проекта) перед push-блоком.** Голый push-блок без следов решения в vault'е — anti-pattern (см. §Anti-patterns).
- Граница: пустые «обсуждения без решения» **не** считаются. Решение = есть конкретный action item или новая инвариант, которую следующая сессия должна знать.

**Read-only сессии (не пушат) — определены через отрицание:** ни один из триггеров 1-4 не сработал. Примеры:

- Вопрос/ответ без записи и без принятого решения.
- Поиск в vault'е без правок.
- Проверка статуса задачи без действий.
- Help / explain-сессии.
- Сама `daily-memory-sync` (scheduled пишет [[Sync State]] своими `^run-*` блоками — это её собственная работа, а не push поверх неё).

**Серая зона.** Если сомнение «считать ли substantive» — принцип **«когда сомневаешься, пуши»**. Дешевле один лишний push-блок, чем silent gap: `daily-memory-sync` дедуплицирует через `session:` prefix, `consolidate-memory` архивирует — overhead минимальный. Принцип применим **только** к серой зоне между триггером 4 и read-only; для очевидных read-only (вопрос/ответ без действий) push не нужен.

**Decision flow (5 секунд на классификацию).** Сессия пробегает чек-лист сверху вниз, останавливается на первом сработавшем пункте:

1. Я сделал хотя бы один vault write? → push.
2. Я сохранил deliverable пользователю? → push.
3. Я выполнил действие во внешней системе с long-lasting эффектом? → push.
4. Мы зафиксировали архитектурное/процессное решение?
    1. Сначала записать решение в vault (Profile / Backlog / decision-лог проекта).
    2. Потом push.
5. Ничего из выше? → не пушить, сессия read-only.

### Формат блока

```markdown
- **Push** ^push-YYYY-MM-DD-THHMMSSZ (session: <8-char-prefix>, "<session-title>") — <one-liner>. **Deliverables:** <list>. **Anomalies:** <none|list>.
```

Опциональные поля `retro: true, original_session: <8-char-prefix>` — только для recovery-push'а за упавшую сессию (см. §«Fail-mode: REST endpoint недоступен»):

```markdown
- **Push** ^push-YYYY-MM-DD-THHMMSSZ (session: <new-prefix>, "<title>", retro: true, original_session: <failed-prefix>) — <one-liner>. **Deliverables:** <list>. **Anomalies:** retro-push for failed push at <UTC ISO original moment>.
```

- `^push-YYYY-MM-DD-THHMMSSZ` — block-ID, UTC, секундное разрешение, без `:` (Obsidian-friendly). Префикс `push-` отличает от `run-`. Для retro-push'а timestamp = UTC момент записи retro, **не** оригинального падения (тот идёт в `Anomalies`). С 2026-05-02 — секундное разрешение (закрытие WARN W6, см. §«Concurrency и known limitations»); до 2026-05-02 использовался минутный формат `THHMMZ` — старые блоки в архиве остаются валидными, `daily-memory-sync` regex `T\d{4,6}Z` ловит оба.
- `<8-char-prefix>` — первые 8 символов после `local_`, для cross-ref с `sync_captured_recent`.
- `retro: true` + `original_session:` ходят парой — оба или ни одного. Одно без другого = anti-pattern (§Anti-patterns).
- Frontmatter Sync State **не трогать** — auto-managed `daily-memory-sync` / `consolidate-memory`.

### Workflow

1. **Проверить наличие секции сегодняшней даты:** `get_note(format: "section", section: {type: "heading", target: "Run History::YYYY-MM-DD"})`. Если возвращает 200 с непустым `valueText` — Шаг 2; если 404 / heading-not-found — Шаг 3.

2. **Секция ЕСТЬ** → `replace_in_note` вставляет push-строку первой под заголовком даты (literal mode, `useRegex: false`):

   - `search`: `### {TODAY}\n\n- **`
   - `replace`: `### {TODAY}\n\n{push-block-line}\n- **`

   Где `{TODAY}` — сегодняшняя дата `YYYY-MM-DD`, `{push-block-line}` — push-блок одной строкой ровно по §«Формат блока» (начинается с `- **Push** ^push-...`, обязателен `session: <8-char-prefix>` для реконсилиации с `daily-memory-sync`). Якорь `- **` ставит новый bullet прямо перед существующим первым, без лишних blank-line'ов. **После записи обязательно:** проверить `response.totalReplacements ≥ 1` (если 0 → §«Fail-mode: pattern-mismatch (`totalReplacements: 0`)» ниже), затем Verify через `obsidian_search_notes(mode: "text", query: "push-YYYY-MM-DD-THHMMSSZ", pathPrefix: "Claude Memory/System/Sync State")` — должен быть ровно 1 hit с этим block-ID в Sync State (иначе → R4 того же fail-mode). [Первичный метод `get_note(section: {type: "block", ...})` на 2026-05-02 баганый — см. §API.]

3. **Секции НЕТ** → сначала прочитать самую свежую дату, уже присутствующую под `## Run History`: `get_note(format: "section", section: {type: "heading", target: "Run History"})` → первый `### YYYY-MM-DD` сверху списка дат. Затем `replace_in_note` создаёт новую секцию **над** ней (literal mode):

   - `search`: `### {LAST_EXISTING_DATE}`
   - `replace`: `### {TODAY}\n\n{push-block-line}\n\n### {LAST_EXISTING_DATE}`

   `{LAST_EXISTING_DATE}` — прочитанная свежая дата, `{TODAY}` и `{push-block-line}` — как в Шаге 2. Якорь `### {LAST_EXISTING_DATE}` уникален в Run History (даты не повторяются); intro-параграф между `## Run History` и первой датой остаётся нетронутым. **После записи обязательно:** проверить `response.totalReplacements ≥ 1` (если 0 → §«Fail-mode: pattern-mismatch (`totalReplacements: 0`)» ниже), затем Verify через `obsidian_search_notes(mode: "text", query: "push-YYYY-MM-DD-THHMMSSZ", pathPrefix: "Claude Memory/System/Sync State")` — должен быть ровно 1 hit с этим block-ID в Sync State (иначе → R4 того же fail-mode). [Первичный метод `get_note(section: {type: "block", ...})` на 2026-05-02 баганый — см. §API.]

4. **Сетевой fail-mode (REST endpoint недоступен — timeout, ECONNREFUSED, 5xx)** → см. §«Fail-mode: REST endpoint недоступен» ниже (1 retry → surface anomaly, deliverables не откатываются).

5. **Pattern-mismatch fail-mode (REST вернул 200 OK, но `totalReplacements: 0`)** → не сетевой fail-mode; recovery — §«Fail-mode: pattern-mismatch (`totalReplacements: 0`)» ниже: R1 (re-read структуру `## Run History` → updated anchor → retry того же Шага один раз) → R2 (alt-pattern: если был Шаг 2 → попробовать Шаг 3, и наоборот) → R3 (failsafe `append_to_note` в конец `## Run History` с `Anomalies: pattern-mismatch fallback append`) → R4 (in-memory anomaly + выделенный `[!warning]` блок в финальное сообщение пользователю; deliverables не откатывать).

6. `patch_note(section: {type: "heading"})` пока не использовать — баг описан в §API.

### Fail-mode: REST endpoint недоступен

Закрывает WARN W3 валидации P1 push-капчи (2026-05-02): без явного fail-mode сессия молча проглатывала REST timeout перед финальным сообщением, и пользователь считал push зафиксированным, хотя его не было.

**Что считается этим fail-mode (только сетевой класс):**
- timeout запроса к `http://127.0.0.1:27123` (нет ответа в разумный срок),
- `ECONNREFUSED` (Obsidian закрыт, REST plugin выключен, ноут просыпается),
- HTTP 5xx от REST endpoint'а.

`200 OK` с `totalReplacements: 0` — **не** этот fail-mode (это pattern-mismatch, см. Workflow Шаг 5 / §W4 в [[AI System Improvement Backlog]]). Категории не путать: сетевой fail = REST не дошёл; pattern-mismatch = REST дошёл, но якорь search'а не совпал.

**Retry policy.** 1 retry с задержкой ~2 секунды. Применяется в обоих ветках Workflow (Шаг 2 — секция дня уже есть; Шаг 3 — нужно создать). `replace_in_note` идемпотентен по якорю (`### {TODAY}\n\n- **` либо `### {LAST_EXISTING_DATE}`) — повторный вызов после успеха не задвоит. Повторять **только** при сетевом fail; при `totalReplacements: 0` retry не делать (это §W4 территория).

**Что делать с anomaly.** Retry упал → НЕ бросать exception, не откатывать deliverables. Записать anomaly локально (in-memory, до конца сессии) с полями: тип ошибки (`timeout` / `ECONNREFUSED` / `HTTP <code>`), UTC timestamp попытки, session_id (8-char prefix), перечень deliverables (для возможного retro-push'а). В финальное сообщение пользователю добавить выделенный блок (шаблон ниже) **отдельной секцией после обычных deliverables**, не переплетая с ними — мульти-deliverable сессии должны видеть push-fail как самостоятельный warning, не размывая основной deliverable-блок.

**Шаблон сообщения пользователю** (копируется в финальное сообщение):

```markdown
> [!warning] Push-блок не записан в Sync State
> REST endpoint недоступен: <тип ошибки: timeout / ECONNREFUSED / HTTP 5xx>.
> Что нужно сделать: проверить, что Obsidian запущен и Local REST API plugin активен (см. [[Memory Sync Protocol#Post-reboot verification]]).
> Восстановление: следующая substantive Cowork-сессия может ретро-запушить за эту — передай ей session prefix `<8-char>` и перечень deliverables (ниже), сессия запишет push-блок с `retro: true, original_session: <8-char>`.
> Deliverables этой сессии (для retro-push'а): <список>.
```

**Recovery flow (retro-push).**

1. Следующая substantive Cowork-сессия видит просьбу пользователя сделать retro-push (или сама замечает по watermark gap'у — будущая итерация).
2. Записывает push-блок по обычному Workflow (Шаг 2 либо 3 — зависит от того, есть ли сегодня секция даты), но в формате с retro-полями (см. §«Формат блока»).
3. Block-ID timestamp = UTC момент записи retro-push'а, **не** оригинального падения. Оригинальный момент идёт в `Anomalies`: `retro-push for failed push at <UTC ISO original>`.
4. `daily-memory-sync` reconciliation дедуплицирует и `session:` (новой сессии), и `original_session:` (упавшей) — обе попадают в `sync_captured_recent`, ни Run-блок, ни push-блок не дублируется.

**НЕ откатывать deliverables.** Push-капча — best-effort layer поверх обычной работы сессии. Vault-writes, проектные deliverables, ответ пользователю **уже сделаны** к моменту попытки push'а. REST-недоступность фиксации push'а **не должна** влиять ни на их существование, ни на финальный ответ. Сессия завершает основную работу + добавляет шаблон-блок выше.

### Fail-mode: pattern-mismatch (`totalReplacements: 0`)

Закрывает WARN W4 валидации P1 push-капчи (2026-05-02): без detection + recovery сессия молча отчитывалась «push сделан» при `totalReplacements: 0`, и push фактически терялся (silent gap).

**Что считается этим fail-mode (не сетевой класс):**
- REST endpoint ответил `200 OK`, но `totalReplacements: 0` — search-pattern не совпал с актуальной структурой Sync State.
- Возможные причины: структура `## Run History` сдвинулась (например, `## Run History` стал `## Run history`, дата-heading получил trailing space, формат `### YYYY-MM-DD` сменился); параллельная правка из другой сессии/пользователя пока готовился replace; неправильно прочитан `{LAST_EXISTING_DATE}` в Шаге 3; whitespace/невидимые символы в anchor'е.

Категория явно отделена от §«Fail-mode: REST endpoint недоступен» по 1 признаку — REST status: REST упал (timeout / ECONNREFUSED / 5xx) → сетевой класс. REST вернул 200 → этот fail-mode (даже если `totalReplacements: 0`). Не путать.

**Detection.** После каждого `replace_in_note` (Шаг 2 или Шаг 3) сессия обязана:
1. Прочитать `response.totalReplacements`. Если ≥ 1 → success path → сразу Verify (см. ниже).
2. Если 0 → pattern-mismatch → recovery R1.

**Verify (после успешной записи любым путём — Шаг 2, Шаг 3, R1, R2, R3).** Вызвать `obsidian_search_notes(mode: "text", query: "push-YYYY-MM-DD-THHMMSSZ", pathPrefix: "Claude Memory/System/Sync State")` с block-ID только что записанного push-блока. Если ровно 1 hit — write подтверждён (block-ID уникален в vault). Если 0 hits или другое количество — сильный сигнал silent failure (REST подтвердил запись, но блок физически не на месте) → R4. [Изначально планировался `obsidian_get_note(format: "section", section: {type: "block", target: "push-..."})` как primary метод, но на 2026-05-02 он возвращает tool-output schema mismatch для block-секций (см. §API). Text-search через block-ID унаследовал ту же гарантию: block-ID уникален, hit-в-Sync-State = блок на месте.]

**Recovery flow:**

- **R1 — диагностика структуры.** Перечитать `obsidian_get_note(format: "section", section: {type: "heading", target: "Run History"}, target)` и сравнить актуальную структуру с ожидаемой: для Шага 2 — есть ли `### {TODAY}` секция; для Шага 3 — что сейчас на месте `{LAST_EXISTING_DATE}`. Если структура изменилась (появилась/исчезла секция дня, дата сдвинулась, whitespace другой) → обновить anchor по свежему чтению и повторить тот же Шаг (2 или 3) **один раз**. Если success → Verify. Если структура совпадает с ожидаемой, но replace всё равно не нашёл match → R2 (whitespace/regex-проблема).
- **R2 — fallback на альтернативный pattern.** Если изначально был Шаг 2 (секция дня уже есть) — попробовать вариант Шага 3 (вдруг секция исчезла из-за гонки или была создана с другой формой даты). И наоборот: если изначально был Шаг 3 — попробовать Шаг 2 (вдруг параллельная сессия успела создать секцию дня после R1-чтения). Если success → Verify. Если снова `totalReplacements: 0` → R3.
- **R3 — failsafe append.** `obsidian_append_to_note(target, section: {type: "heading", target: "Run History"}, content: "<push-block-line>\n")` — добавляет push-блок в самый конец секции `## Run History` (после всех date-subsections). Daily-memory-sync regex (`^push-\d{4}-\d{2}-\d{2}-T\d{4,6}Z\b.*?\bsession:\s*([0-9a-f]{8})`, бэквард-компат по минутному/секундному разрешению block-ID) поймает блок независимо от позиции — реконсилиация и дедупликация не сломаются (см. SKILL.md `daily-memory-sync` §3a). Push-блок **обязательно** включает в `Anomalies:` явную метку: `pattern-mismatch fallback append, please move to correct date section manually`. Если success → Verify. Если append упал (например, `Run History` heading тоже не нашёлся) → R4.
- **R4 — surface anomaly.** Все R1-R3 не сработали (или Verify после R1/R2/R3 вернул пустой ответ). Поведение как у §«Fail-mode: REST endpoint недоступен»: in-memory anomaly с полями (тип `pattern-mismatch`, UTC timestamp попытки, session prefix, попытанные anchors, deliverables) + выделенный `[!warning]` блок в финальное сообщение пользователю (шаблон ниже) **отдельной секцией после deliverables**. **НЕ откатывать deliverables.**

**Шаблон сообщения пользователю** (отдельный `[!warning]` callout, отличается от W3 шаблона — упоминает причину «pattern-mismatch» вместо REST-недоступности):

```markdown
> [!warning] Push-блок не записан в Sync State (pattern-mismatch)
> Структура `## Run History` отличается от ожидаемой (`totalReplacements: 0` после fallback append).
> Что нужно сделать: вручную проверить [[Sync State]] — добавлен ли блок `^push-YYYY-MM-DD-THHMMSSZ` где-то в Run History? Если нет — скопируй ниже и вставь в правильное место (под сегодняшним `### YYYY-MM-DD`, первой строкой).
> Подготовленный блок: `<готовый push-block-line по §«Формат блока»>`
```

**Anti-patterns этого fail-mode:**
- Игнорировать `totalReplacements: 0` (push silently lost). Никогда. Каждый `replace_in_note` обязан проверять response.
- Принимать любой `200 OK` без Verify через `search_notes` по block-ID. Без Verify даже R3-append может пройти и потеряться — Verify это финальная страховка.

### Реконсилиация с `daily-memory-sync`

Утренний scheduled-прогон видит `^push-*` блоки за окно с прошлого Run → извлекает `session: <prefix>` (для retro-push'ей дополнительно `original_session: <prefix>`) → если любой из извлечённых префиксов уже в `sync_captured_recent` или среди только что просмотренных → **не дублирует** Run-записью. Retro-push (с `retro: true`) дедуплицирует сразу обе сессии — и новую (`session:`), и упавшую (`original_session:`) — обе попадают в `sync_captured_recent`. Push-блоки попадают в архив наравне с Run-блоками через `consolidate-memory`.

### Concurrency и known limitations

Закрывает WARN W6 валидации P1 push-капчи (2026-05-02): race condition между параллельными Cowork-сессиями + `### YYYY-MM-DD` заголовок описаны как known limitation, без обещаний атомарной транзакции через REST.

**Контекст модели согласованности.** Все push-блоки идут через единственный REST endpoint Local REST API plugin'а (`http://127.0.0.1:27123`). Plugin'ная атомарность операций не гарантируется Protocol'ом — обращаемся к нему как к best-effort layer. В one-user / single-machine setup вероятность реального race'а низкая (Cowork обычно не запускает substantive sub-task'и одновременно с substantive write'ом в основном dispatch'е), но не нулевая.

**Сценарий A (Шаг 2, секция дня уже есть, обе сессии).** REST plugin последовательно обрабатывает оба `replace_in_note`. Anchor `### {TODAY}\n\n- **` остаётся валидным после первой записи (новый bullet вставляется *перед* существующим первым bullet'ом — старый `- **` остаётся на месте как продолжение pattern'а). Вторая запись пройдёт штатно; оба push-блока окажутся в файле в порядке REST-обработки. **Поведение:** OK без специальной обработки.

**Сценарий B (Шаг 3, обе сессии — секции дня ещё нет).** Первая запись создаёт `### {TODAY}` секцию через якорь `### {LAST_EXISTING_DATE}`. Вторая, использующая тот же `{LAST_EXISTING_DATE}` anchor, получит `totalReplacements: 0` — anchor сместился (перед ним теперь стоит свежесозданная `### {TODAY}` секция с push-блоком первой сессии). Это сценарий §«Fail-mode: pattern-mismatch (`totalReplacements: 0`)» R1: re-read структуры `## Run History` → видно, что `### {TODAY}` теперь существует → переключиться на Шаг 2 → запись проходит. **Поведение:** один лишний REST call (R1 re-read + retry), без потери данных.

**Сценарий C (Шаг 3 одной сессии, Шаг 2 другой).** Первая сессия (Шаг 3) создаёт `### {TODAY}` и пишет первый push. Вторая попадает на сценарий «секция дня есть» и идёт по Шагу 2 — конфликта нет, оба push-блока на месте. **Поведение:** OK без специальной обработки (идемпотентность Шага 3 через Шаг 2).

**Сценарий D (одинаковые block-IDs).** Block-ID = `^push-YYYY-MM-DD-THHMMSSZ`, секундное разрешение (с 2026-05-02). Две сессии, завершившиеся в ту же секунду UTC — теоретически возможно, но в one-user setup пренебрежимо. **Mitigation:** перед Verify-step'ом сессия проверяет collision через `obsidian_search_notes(mode: "text", query: "push-YYYY-MM-DD-THHMMSSZ", pathPrefix: "Claude Memory/System/Sync State")` по полному block-ID. Если hit найден ДО записи (т.е. блок уже существует от другой сессии) — увеличить timestamp на 1 секунду, перестроить push-block-line, retry; до 3 попыток. После 3 неудач — surface anomaly через шаблон §«Fail-mode: pattern-mismatch» R4 (deliverables не откатывать). **Никогда** не перезаписывать существующий push-блок другой сессии (см. §Anti-patterns).

**Mitigation rules (что обязана делать сессия).**
- Перед записью в Шаге 2/3 явная блокировка не нужна — встроенный flow устойчив (Verify через `search_notes` после каждой записи; R1 при `totalReplacements: 0`).
- Перед записью обязательная collision-check на block-ID (Сценарий D mitigation): `search_notes` по полному block-ID; коллизия → timestamp + 1с, retry до 3 раз.
- При обнаруженной коллизии **не удалять** существующий push-блок другой сессии. Mitigation — сменить timestamp в собственном ID, не наоборот.

**От 2026-05-02:** разрешение block-ID мигрировано с минутного `THHMMZ` на секундное `THHMMSSZ`. `daily-memory-sync` regex (`T\d{4,6}Z`) принимает оба варианта для backward-compat со старыми блоками; `consolidate-memory` архивирует оба формата без миграции. Старые минутные push-блоки в Run History и архиве остаются валидными — переписывать не нужно.

**Не-цели.**
- Polished распределённая транзакция между сессиями (REST plugin не даёт таких гарантий).
- Гарантия порядка записи push-блоков (порядок в файле = порядок REST-обработки plugin'а, недетерминирован при близкой одновременности).

### Anti-patterns

- **Двойной push в одной сессии.** Перед записью — `obsidian_search_notes(mode: "text", query: "session: <prefix>", pathPrefix: "Claude Memory/System/Sync State")`. Если уже есть — обновить существующий блок через `replace_in_note`, не плодить новый.
- **Push с ретро-датой.** Block-ID timestamp = UTC сейчас, не задним числом. Это ломает append-only.
- **Push с frontmatter-update Sync State.** Только `daily-memory-sync` / `consolidate-memory`.
- **Push для read-only сессии.** Нечего пушить — шум в Run History.
- **Push-блок без следов решения в vault'е (для триггера 4).** Silent gap: push фиксирует факт встречи, но не контент решения — следующая сессия не сможет восстановить инвариант. Если триггер 4 сработал — записать само решение в vault (Profile / Backlog / decision-лог проекта) **перед** push-блоком (см. §«Когда пушить» Триггер 4).
- **Push в `^run-...` формате.** Block-ID разделяет источник (scheduled run vs session push).
- **`retro: true` без `original_session: <prefix>` (или наоборот).** Поля ходят парой; одно без другого — молчаливый дубль: `daily-memory-sync` reconciliation не свяжет retro-push с упавшей сессией, и она снова попадёт в CAPTURE на следующий run. Также не использовать retro-поля для обычных (не recovery) push'ей — это засоряет dedup-логику и вводит recon в заблуждение.

- **Запись push-блока без проверки `totalReplacements ≥ 1` и Verify через `get_note(block)`.** Silent gap. И detection (`totalReplacements` после каждого `replace_in_note`), и Verify (post-write `search_notes(mode: "text", query: "push-...", pathPrefix: "Claude Memory/System/Sync State")` — hit-count = 1) — обязательны, не «по желанию». Без Verify даже R3-append (failsafe) может пройти и потеряться — Verify это финальная страховка.

- **Удалять чужой push-блок при collision block-ID.** Теряет данные другой сессии — push-блок был записан раньше и содержит чужие deliverables. Mitigation: сменить timestamp в своём block-ID на следующую секунду, retry до 3 раз; после 3 попыток — surface anomaly через шаблон §«Fail-mode: pattern-mismatch» R4 (детали — §«Concurrency и known limitations» Сценарий D).

### Autostart требование

Push-капча всё ещё нужен живой Obsidian + REST API plugin **в момент записи**. Конфигурация Windows-автозапуска — shortcut `Obsidian.lnk` в `shell:startup` (см. §Связанные сущности). Без autostart первый push после reboot упадёт с REST timeout если Obsidian не запущен.

## Post-reboot verification

> [!check] Post-reboot verification
> После любого reboot ноута, **перед** запуском первой substantive Cowork-сессии:
> 1. Открыть Obsidian (должен открыться автоматически из `shell:startup\Obsidian.lnk`).
> 2. Проверить REST endpoint: `curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:27123/` → ожидается `401` (auth required) или `200`.
>     - Если `connection refused` / timeout → плагин «Local REST API» выключен. Settings → Community plugins → найти Local REST API → Enable (включая «Enable at startup» для следующего reboot'а).
>     - Если REST живой, но push-блоки не пишутся → уточнить токен в plugin settings, проверить что vault `brain` (или актуальное имя) — тот же, что в config'ах MCP-сервера.
> 3. После исправления — sanity-check: записать тестовый push-блок, увидеть его в Sync State.
> 4. Если первая Cowork-сессия после reboot напоролась на REST timeout — обязана **surface'ить пользователю** проблему в финальном сообщении, **не падать молча**. Полный fail-mode (1 retry → шаблон в финальное сообщение → recovery через retro-push; deliverables не откатываются) — см. §«Fail-mode: REST endpoint недоступен» в §Push captures выше.

Чек-лист закрывает WARN W1 валидации P1 push-капчи (2026-05-02): autostart REST API plugin'а программно не верифицируется до reboot'а, потому формализован как ручная процедура.

---

## Историческая справка

### Миграция v3.6 — anti-under-capture + token economy (2026-06-11)

Аудит по запросу пользователя («задача периодически гонит») выявил: (1) under-capture — прогоны 06-10 r1/r2 и 06-11 r1 на Haiku отчитались «zero-capture», пропустив 10 substantive-сессий (догнаны Run 2 2026-06-11); (2) фантомный отчёт 06-09 «SKILL.md v4.0 внесена» при фактическом v3.5 на диске; (3) сломанный порядок дат в Run History; (4) Sync State ~85 КБ — full read падает по token-limit; (5) FIFO-50 эвикция создаёт двусмысленность «не захвачена vs вытеснена».

Изменения: `daily-memory-sync` v3.5→v3.6 — zero-capture guard («0 new captures» только при пустом списке кандидатов, инвариант 14), pre-watermark skip rule (инвариант 16), lazy System reads на idle-днях (инвариант 17, экономия ~15–25K токенов/прогон), targeted Run History scan через search_notes вместо full-section read, Case B anchor = первый ### сверху, FIFO 50→100. `consolidate-memory` v1.2→v1.3 — архивация Run History 90→30 дней. Run History отсортирован одноразово (блок 06-09 перемещён, содержимое не менялось). Решение пользователя: модель daily-задач — Sonnet вместо Haiku (см. [[AI System Improvement Backlog]] §Model routing).

### Миграция API-слоя v3.5 (2026-06-07)

Obsidian-rest endpoint обновился: `manage_frontmatter`, `manage_tags`, `list_notes`, `delete_note` стали реально доступны. По итогам сбоя daily-memory-sync 2026-06-06 (schema validation на patch_note(frontmatter), run завершился на ~75%) проведена миграция: daily-memory-sync v3.4 → v3.5, consolidate-memory v1.1 → v1.2, синхронно обновлены [[Memory Sync Protocol]] и [[Obsidian Markdown Conventions]]. Frontmatter-записи — только через manage_frontmatter; листинг папок — list_notes; финальный ход scheduled-задач — text-only без tool-calls. Run History остаётся на replace_in_note (default v3.4 сохранён); block-section баг get_note перепроверен 2026-06-07 — всё ещё актуален, verify через search_notes.

### Эпоха StevenStavrakis/obsidian-mcp v1.0.6 (до 2026-04-26)

До перехода на cyanheads vault обслуживался через `obsidian-mcp` v1.0.6 с тремя блокирующими багами:

1. **`list-available-vaults` зависал на 60s**
2. **`edit-note` падал с discriminator error** — workaround: snapshot → `delete-note` → `create-note`
3. **`delete-note` каскадно strikethroughs incoming wikilinks** — whack-a-mole без выхода

Из-за багов появилась append-driven доктрина + обязательный `_Snapshots/`. 25 backup-файлов удалены 2026-04-26. Cascade discovery — [[Sync State#^run-2026-04-25-r3]].

**Маппинг старых тулов (отключены 2026-05-01):**
- `obsidian_read_note` → `obsidian_get_note`
- `obsidian_update_note(overwrite)` → `obsidian_write_note(overwrite: true)`
- `obsidian_update_note(append)` → `obsidian_append_to_note`
- `obsidian_search_replace` → `obsidian_replace_in_note`
- `obsidian_global_search` → `obsidian_search_notes(mode: "text")`
- `obsidian_manage_frontmatter` → `obsidian_patch_note(section: {type: "frontmatter"})`
- `obsidian_manage_tags` → `obsidian_patch_note(section: {type: "frontmatter", target: "tags"})`
- `obsidian_list_notes` → `obsidian_search_notes(mode: "dataview")`

### Эпоха `Sync Logs/<date>.md` (2026-04-25 → 2026-05-01)

С 2026-04-25 каждый прогон писал в `Sync Logs/<date>.md`. К 2026-05-01 накоплено 5 файлов. Миграция на v3 ([[Sync State#^run-2026-05-01-r5]]): уникальные дельты — в Changelog проектов; архитектурные milestones — в Историческую справку; run audit — compact one-liners в Sync State Run History. Папка упразднена.

### Открытые вопросы

> [!question] Scheduler serialization
> Гипотеза 2026-05-01 ([[Sync State#^run-2026-05-01-r4]]): Cowork scheduler не запускает scheduled-task пока есть active user-сессия. Если подтвердится — gap-дни 04-29/04-30 объяснимы. Архитектурного решения нет кроме «закрыть приложение» или ручного «Run now».

## Связанные сущности

- Scheduled tasks: `daily-memory-sync` (`0 9 * * *`), `consolidate-memory` (`0 9 1 * *`)
- MCP-сервер: [cyanheads/obsidian-mcp-server](https://github.com/cyanheads/obsidian-mcp-server) v2.0.7
- Bridge plugin: [Obsidian Local REST API](https://github.com/coddingtonbear/obsidian-local-rest-api) v3.6.1
- Endpoint: `http://127.0.0.1:27123`
- Конфиг: `%APPDATA%\Claude\claude_desktop_config.json` → entry `obsidian-rest`
- Autostart (Windows): shortcut `Obsidian.lnk` в `shell:startup` → запускает Obsidian + Local REST API plugin вместе со входом пользователя. REST API plugin держится в `enabled at startup` через Settings → Community plugins.
