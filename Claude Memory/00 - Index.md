---
type: index
created: 2026-04-23
updated: 2026-07-09
last_reviewed: 2026-05-29
tags:
  - claude-memory
  - index
---

# Claude Memory — Index

Карта vault. Это **карта**, не журнал — аудит прогонов в [[Sync State]]. Архитектура — **append-driven digital brain**: знание расширяется в карточках проектов, история сохраняется. Подробности — в [[Memory Sync Protocol]].

Последняя ревизия: **2026-05-22** · Состояние и история прогонов: [[Sync State]]

2026-05-22 (v3.4 closure): `daily-memory-sync` SKILL.md v3.3 → v3.4 — закрыты три класса recurring anomalies, пойманных в Run 1 сегодня: (A) orphan system-session rule в §3b (Daily memory sync / Consolidate memory titles в окне без origin в Run History → silent system class, не anomaly); (B) Run History writes default = `replace_in_note` (skip flaky `patch_note(heading)`); (C) Run History verify default = `search_notes(mode: "text")` (skip schema-mismatch `get_note(section: heading)` на новосозданных headings). 3 новых invariant (10/11/12), counter `orphan_system_count` в §5 one-liner, orphan-system поле в §9 REPORT. Push-блок: [[Sync State#^push-2026-05-22-T044924Z]].

2026-05-02 вечерние события: Pass A поймал первый recon gap в P1 push-капче (Memory Sync Protocol §Push captures описывал реконсилиацию, которой не было в `daily-memory-sync` prompt'е); `daily-memory-sync` v3.1 → v3.2 закрыл gap шагами 3a/3b/3c (PUSH SCAN + dedup); [[Verification Protocol#Mistakes Log]] активирован первой записью `^mistake-2026-05-02-recon-gap`; W1 closed: post-reboot REST check formalized в [[Memory Sync Protocol]] §Post-reboot verification; W2 closed: explicit S&R patterns в §Push captures workflow; W3 closed: REST timeout fail-mode задокументирован в §Push captures (1 retry → surface anomaly, не блокировать deliverables); W4 closed: pattern-mismatch detection + 4-stage recovery + Verify after write в §Push captures; §«Формат блока» расширен опциональными `retro: true` / `original_session:` для recovery-push'ей; `daily-memory-sync` v3.2 patched ретро-aware: PUSH SCAN распознаёт retro-поля, reconciliation дедуплицирует обе сессии; P3 added: obsidian-rest block-section bug tracked в [[AI System Improvement Backlog]]; W5 closed: substantive triggers заменены на 4 bright-line правила + Decision flow в §Push captures; W6 closed: concurrency сценарии A-D + mitigation rules в §Concurrency и known limitations; block-ID разрешение мигрировано на секундное `THHMMSSZ` (backward-compat regex `T\d{4,6}Z` в daily-memory-sync v3.2); S1 closed: consolidate-memory v1.0 → v1.1 — устаревшие API заменены (`list_notes` → `search_notes(dataview)`, `manage_frontmatter` → `patch_note(frontmatter)`, `append_to_note(heading:)` → `patch_note(heading)`), push backward-compat regex `T\d{4,6}Z` прописан в Шаг 1 + Important invariants; Deferred-tool-list misalignment задокументирован в §API + P3 трэкер в Backlog.

## Профиль

- [[Profile]] — Marketing Director, KUA Almaly; стиль, роль, форматы

## Активные проекты

- [[AMC - Almaty Mountain Cluster]] — горный курорт; диалог с Ecosign (Eric Callender), параллельный трек Almaty Superski с Акиматом
- [[1966 Plateau Deck]] — 16-слайдовый investor brief на английском для Almaly Holding
- [[ФК Кайрат — 1xBET]] — переговоры по долгу 2025 (422,5 млн ₸) и ДС № 6 на 2027–2028 по методологии GAP; открытое расхождение 200 млн ₸
- [[ФК Кайрат — Halyk Bank]] — привлечение спонсорства Halyk при действующем контракте с Freedom Bank; стадия сбора вводных по GAP
- [[I'M Restaurant Chain]] — ДЗО-сеть ресторанов, аудит ROMI + mystery shopper
- [[Медео Парк Отель]] — пре-опенинг (плаза 10 июня, отель 1 июля); коммуникационная стратегия
- [[n8n AI Agents]] — `arshat.app.n8n.cloud`; Telegram → n8n → Claude; KUA-документы и Almaty Business FM Bot
- [[Ecole Ducasse Almaty Studio]] — культурно-кулинарное партнёрство; Marketing & Sales штатка для Дины
- [[Annual Report Framework]] — переиспользуемый промпт-фреймворк; цикл 2025 завершён
- [[Annual Shareholder Meeting Backdrop]] — LED-backdrop v2.1 (8 сцен × 10 сек, 80-сек цикл, лого embedded, clamp-typography)
- [[ZILLI Marketing Services]] — договор и SMM-стратегия; подрядчик ТОО «Улар Трейд Хаус»
- [[AGS Brand Development]] — брендбук umbrella-бренда гастроцентра (ownable IP поверх партнёрских ED/SCA/EWA); КП отправлено, draft ответа Дияру с вариантами A/B готов
- [[Цех №1 — Завод минеральных вод]] — строительный аудит цеха розлива (клиент «Алекс»); готовность ≈55–60%, 2 критических риска
- [[Жеті Ата]] — финансовый аудит производства фильма; условное одобрение 3-го транша $250 000
- [[ВНД Agent]] — внутренний AI-агент для подготовки нормативных документов холдинга; D05 testing — оценка 9.2/10
- [[Almaly Innovation Strategy]] — стратегия инноваций холдинга 2026–2028; Go/No-Go 5 направлений по итогам китайской делегации; презентация акционеру 19–20 мая
- [[Claude Training — Marketing Team]] — 4-уровневая программа обучения маркетинг-команды холдинга (45-slide PPTX + Handbook + 20-prompt library + 30-day implementation plan, KPI −40% времени / +3× скорость драфта)
- [[VkusVill Acquisition KZ]] — M&A-возможность по ВкусВилл РК; противоречие письмо vs тизер ($6M запрос); fair value $2,5–4,0M; контр-оффер готов
- [[Drone Delivery Almaty]] — деки Акционеру/Акиму: 20+2 дрона / 5 хабов, CAPEX 520 млн ₸, OPEX 300 млн ₸/год; ask — грант 500 млн или субсидия 100 млн/год + BVLOS-разрешение КГА
- [[Autonomous Pharmacy Retail]] — RFI 55 вопросов RU/ZH под Galbot, YaoShiBang; одно из 5 направлений китайской делегации
- [[Постаматы Almaty]] — универсальный RFI 34 вопроса RU/ZH под OEM/операторы/JV; одно из 5 направлений китайской делегации
- [[Cinnabon Carvel Master Franchise]] — M&A-возможность мастер-прав Cinnabon + Carvel KZ + KG ($500K, флагман $60K/год, потенциал >$1M/год); предложение к рассмотрению вместо [[I'M Restaurant Chain]]
- [[Брендбук Алмалы]] — брендбук корпоративного бренда холдинга; опросники заполнены (контакт — Екатерина Тулякова), ядро «Основательность, которой доверяют», слоган «Строим на века»; финальный документ — конец июля 2026
- [[Personal Brand @arshat]] — аудит и оптимизация Instagram @arshat; 30–35% потенциала; 6 критических gap'ов; план действий 48ч + неделя
- [[Freedom Media — FK Kairat Documentary]] — сделка Freedom Media на документальный фильм о ФК «Кайрат»; 50 млн ₸, 4 серии, 3 года эксклюзива; письмо Акционеру готово
- [[Tselinny]] — Экспертный совет Целинного (25–27.06.2026); роль «внутренний внешний наблюдатель»; переход управления Джама→Гульнара Саттыбаева до 7 июля; встреча 25.06 11:00 Тимирязева 18А
- [[Almaly Brand System & Skills]] — дизайн-система и скиллы для HTML/web-deliverables; 4-фазный роадмап; Netlify MCP для лендингов ДЗО
- [[Food Packaging]] — новый клиент; экологичная упаковка B2B; 4 направления (стратегия, PR, ребрендинг, SMM); встреча на заводе 24.06
- [[Almaty City Digital Twin]] — цифровой двойник Алматы; 2 контура (гос. + коммерческий), КУА — оператор; RFI 38 вопросов (RU/ZH) для китайских партнёров; встреча Гуанчжоу 09.07+

- [[Маркетинг ЦКП 2026]] — KPI-презентация маркетинг-блока; защита 16.09.2026

## Фреймворки и методологии

- [[AI Workflow Guide — Arshat]] — роутинг задач по инструментам (Claude/ChatGPT/Perplexity/n8n), prompt-шаблоны, cross-tool workflows; создан 2026-05-02
- [[Hoshin Kanri X-matrix]] — стратегический каскад 3–5 лет → годовые цели
- [[GAP Partnership Negotiation]] — методология переговоров; $133 609 выигрыша в 2025
- [[4C Analysis]] — Company/Customers/Competitors/Context; деливери в .docx
- [[Kaizen]] — операционная философия, подкладка под Hoshin

## История и завершённое

- [[Completed Deliverables]] — Xiamen, AMC cheat-sheet, compliance-био, EDA CDP мокап, ROMI-аудит, 4C-кейсы, EDA staffing, backdrop v2 + v2.1, email Эрику, ZILLI-договор

## Система

- [[Memory Sync Protocol]] — append-driven доктрина v3, watermark + push captures (self-push at session end), активные тулы obsidian-rest, autostart через shell:startup, исторические справки
- [[Obsidian Markdown Conventions]] — синтаксис записи в vault (wikilinks, callouts, block-IDs, properties), адаптация obsidian-skills от kepano
- [[Sync State]] — watermark, pending captures и Run History, auto-managed `daily-memory-sync` v3
- [[AI System Improvement Backlog]] — приоритизированные рекомендации по личному AI-стеку (verification layer, push-капча, playbook'и, eval-набор, privacy-классификация)
- [[AI ROI Ledger]] — учёт работы Claude vs ручной альтернативы; weekly digest по понедельникам, Q2 close 30 июня для Акционера
- [[Verification Protocol]] — двухслойная проверка для 🔴/🟡 deliverables: Pass A (subagent fact-check) + Pass B (24h human review); Mistakes Log активирован 2026-05-02 первой записью (recon gap в P1 push-капче)

---

## Теги

- `#project/active` — проект в работе
- `#project/historical` — завершённый
- `#framework` — методология
- `#deliverable` — готовый артефакт (документ, презентация, пост)
- `#contact` — внешний контакт или контрагент
- `#system` — служебные заметки (протоколы, конфигурация)

## Правила поддержания (резюме)

- Даты — всегда абсолютные (`2026-04-26`)
- **Append, не rewrite.** Крупные overwrite — только по явной необходимости; история ведётся встроенными средствами Obsidian (.trash + File Recovery)
- Изменения проектов фиксируются в их секции `## Changelog` (внизу файла, обратная хронология); audit-trail всех прогонов — в [[Sync State]] §Run History
- Index — стабильная карта, обновляется только при структурных событиях (новый/завершённый проект, переименование, новый System-файл)
- Index держать **под 200 строк** — детали в topic-файлах
- Никогда не переписывать `Profile.md` без явного сигнала пользователя

Полный workflow и набор правил → [[Memory Sync Protocol]].
