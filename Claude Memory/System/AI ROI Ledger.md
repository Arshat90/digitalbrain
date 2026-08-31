---
type: system
created: 2026-05-02
updated: 2026-08-31
tags:
  - claude-memory
  - system
  - roi-ledger
  - metrics
ledger_version: 1
weekly_digest_day: понедельник
---

# AI ROI Ledger

Учёт работы, выполненной с Claude vs ручной альтернативой. Защищаемые цифры для Акционера, ровно как [[GAP Partnership Negotiation]] $133k-кейс. Закрывает **P0** из [[AI System Improvement Backlog]].

## Протокол заполнения

- **Когда:** еженедельно, в понедельник, retrospective за прошедшую неделю. Первый раз — retro-fill за апрель из [[Completed Deliverables]] (этот файл).
- **Что считать:** только artifacts, **доставленные внешнему/внутреннему получателю** (не итерации и не черновики, оставшиеся в чате).
- **Manual (ч):** честная оценка «сколько бы заняло без ИИ» — ориентир, не точная цифра. Округление до 0.5 ч.
- **Claude (ч):** реальное wall-clock с итерациями и проверкой. Округление до 0.25 ч.
- **Saved = Manual − Claude.** Может быть отрицательной (отметить ⚠️) — это сигнал, что ИИ не оправдан для данного типа задачи и стоит исключить из playbook'ов.

## Конвенции

### Типы leverage

- `analysis` — 4C, ROMI, бизнес-кейсы, бенчмарки
- `translation` — RU↔EN, RU↔KK
- `document` — договоры, отчёты, биографии, тендерные пакеты
- `visual` — HTML/CSS, презентации, видео, дизайн, AI-генерация (NB2/Veo)
- `content` — тексты, посты, нарратор, сценарии
- `negotiation` — письма контрагентам, draft с вариантами A/B

### Критичность

- 🔴 **critical** — Президент, Набсовет, регуляторы, M&A-контрагенты, бенефициары
- 🟡 **high** — внешние партнёры, важные ДЗО, тендеры
- 🟢 **medium** — внутренние материалы, регулярная отчётность

## Ledger

| Дата | Проект | Deliverable | Manual | Claude | Saved | Leverage | Крит. |
|---|---|---|--:|--:|--:|---|:--:|
| 2026-04-25 | [[Annual Shareholder Meeting Backdrop]] | LED-backdrop v2 (12 сцен HTML, ON AIR pulse, бегущая строка) | 20 | 3 | 17 | visual+content | 🔴 |
| 2026-04-25 | [[Annual Shareholder Meeting Backdrop]] | v2.1 finishing (base64-logo, clamp typography, 8×10s = 80s) | 5 | 1 | 4 | visual | 🔴 |
| 2026-04-25 | [[AMC - Almaty Mountain Cluster]] | Email Эрику Каллендеру (Ecosign), EN | 1.5 | 0.25 | 1.25 | negotiation | 🔴 |
| 2026-04-25 | [[ZILLI Marketing Services]] | Договор маркуслуг — заполнение реквизитов | 1.5 | 0.5 | 1 | document | 🟢 |
| 2026-04-25 | [[Ecole Ducasse Almaty Studio]] | Marketing & Sales Staff Plan (3 листа Excel + draft email) | 10 | 2 | 8 | analysis+visual | 🟡 |
| 2026-04-26 | [[I'M Restaurant Chain]] | Хасбулатов 70 video pre-prod (shot-list 5 sheets, README, 10 слайдов) | 14 | 4 | 10 | content+visual | 🟡 |
| 2026-04-27 | [[I'M Restaurant Chain]] | Хасбулатов 70 video production (14 NB2-портретов, Veo 3.1 промпты, Ken Burns) | 40 | 6 | 34 | visual | 🟡 |
| 2026-04-28 | [[AGS Brand Development]] | Draft ответа Дияру с вариантами A/B по КП брендбука | 5 | 1.5 | 3.5 | negotiation+analysis | 🟡 |
| 2026-05-01 | [[Ecole Ducasse Almaty Studio]] | SynApp-тендер (4 deliverables, MVP 17.4М₸ / полный 759.8М₸) | 7 | 1.5 | 5.5 | document+analysis | 🟡 |
| 2026-05-03 | [[1966 Plateau Deck]] | F&B тендер: рекомендация Simple Pleasures, письмо Акционеру + ответ Alléno | 4 | 1 | 3 | negotiation+analysis | 🔴 |
| 2026-05-03 | [[Completed Deliverables]] | Almaty Central Stadium — Blender-модель v9 (+80 ref-фото) | 10 | 4 | 6 | visual | 🟢 |
| 2026-05-04 | [[I'M Restaurant Chain]] | Paul 2GIS-аудит: дашборд v2 + CEO/CMO отчёты + bugfix | 8 | 2.5 | 5.5 | analysis+visual | 🟡 |
| 2026-05-04 | [[Completed Deliverables]] | AI-постер A1 | 3 | 0.5 | 2.5 | visual | 🟢 |
| 2026-05-04 | [[Ecole Ducasse Almaty Studio]] | IT-tender email поставщикам | 1 | 0.25 | 0.75 | negotiation | 🟡 |
| 2026-05-05 | [[Цех №1 — Завод минеральных вод]] | Строительный аудит (готовность 55–60%, 2 критич. риска, docx) | 6 | 1.5 | 4.5 | analysis+document | 🟡 |
| 2026-05-05 | [[Жеті Ата]] | Финансовый аудит — условное одобрение транша $250K (docx) | 6 | 1.5 | 4.5 | analysis+document | 🟡 |
| 2026-05-06 | [[ВНД Agent]] | VND Testing Report D04 (оценка 8.5/10) | 4 | 1 | 3 | document+analysis | 🟢 |
| 2026-05-06 | [[ВНД Agent]] | VND Testing Report D05 (9.2/10) + 4 фикса skills | 4 | 1 | 3 | document+analysis | 🟢 |
| 2026-05-06 | [[Completed Deliverables]] | Recommendation Letter — Arina Kozlova (EN, docx) | 1.5 | 0.5 | 1 | document | 🟢 |
| 2026-05-13 | [[1966 Plateau Deck]] | DD-вопросник ×50 к Simple Pleasures | 3 | 1 | 2 | analysis | 🟡 |
| 2026-05-13 | [[ВНД Agent]] | Ревью IT-архитектуры + методология передачи среды | 3 | 1 | 2 | document | 🟢 |
| 2026-05-13 | [[Ecole Ducasse Almaty Studio]] | ED signage: email Emilie Leducq + запрос брендбука | 1 | 0.25 | 0.75 | negotiation | 🟡 |
| 2026-05-13 | [[1966 Plateau Deck]] | protocol_1966_simple_pleasures.docx — протокол запуска | 2.5 | 0.5 | 2 | document | 🟡 |
| 2026-05-14 | [[Completed Deliverables]] | Статус-встреча: резюме 20 направлений + таблица поручений (.md) | 2 | 0.5 | 1.5 | analysis | 🟢 |
| 2026-05-14 | [[Цех №1 — Завод минеральных вод]] | Отчёт v2 с WhatsApp-данными (8 блоков, прогноз авг–сент) | 5 | 1.5 | 3.5 | document+analysis | 🟡 |
| 2026-05-15 | [[ZILLI Marketing Services]] | ZILLI_KZ_Analytics_Block1.pptx | 5 | 1.5 | 3.5 | analysis+visual | 🟡 |
| 2026-05-15 | [[Ecole Ducasse Almaty Studio]] | Анализ_кандидатов_ED_Almaty_2026.docx (11 канд., Marketing Director) | 4 | 1 | 3 | analysis+document | 🟡 |
| 2026-05-15 | [[ВНД Agent]] | Блок-схема MAR: 3 фикса + инструкция рецензентам | 2 | 0.75 | 1.25 | visual+document | 🟢 |
| 2026-05-15 | [[1966 Plateau Deck]] | Протокол встречи 14.05 + диаграмма Ганта | 3 | 1 | 2 | document | 🟡 |
| 2026-05-16 | [[Ecole Ducasse Almaty Studio]] | Tender_Comparison_v4.docx (Rocket Tech 5-й поставщик, SynApp рекомендован) | 3 | 0.75 | 2.25 | analysis+document | 🟡 |
| 2026-05-16 | [[AMC - Almaty Mountain Cluster]] | EY Kazakhstan: GAP-карта переговоров (13 переменных) | 4 | 1 | 3 | negotiation+analysis | 🟡 |
| 2026-05-16 | [[I'M Restaurant Chain]] | Оценка консультанта по пищевой безопасности | 1.5 | 0.5 | 1 | analysis | 🟢 |
| 2026-05-16 | [[ZILLI Marketing Services]] | Instagram-аналитика @zilli.kazakhstan (ER, 3 стратег. вектора) | 3 | 1 | 2 | analysis | 🟡 |
| 2026-05-16 | [[Ecole Ducasse Almaty Studio]] | BORK partnership: GAP-анализ + встречное предложение 15M KZT | 4 | 1 | 3 | negotiation+analysis | 🟡 |
| 2026-05-18 | [[ZILLI Marketing Services]] | Annual Report Block 1 — 14 слайдов LiveDune + 7 рекомендаций | 5 | 1.5 | 3.5 | analysis+visual | 🟡 |
| 2026-05-18 | [[Almaly Innovation Strategy]] | Go-NoGo 5 направлений инноваций — 36 слайдов + drone delivery КП | 10 | 2.5 | 7.5 | analysis+visual | 🟡 |
| 2026-05-19 | [[Claude Training — Marketing Team]] | Training Program: 45-slide PPTX + Handbook + 20 промптов + 30-day plan | 16 | 3 | 13 | visual+content | 🟢 |
| 2026-05-19 | [[ZILLI Marketing Services]] | Master Strategy v0.4 FINAL — 103 слайда, 6 блоков | 20 | 5 | 15 | analysis+visual | 🟡 |
| 2026-05-19 | [[AMC - Almaty Mountain Cluster]] | BIG + YYA: 2 meeting summary docx + 2 follow-up письма Alia | 4 | 1.5 | 2.5 | document+negotiation | 🟡 |
| 2026-05-19 | [[Completed Deliverables]] | KUA Almaly website mockup (index.html + Netlify-проект) | 8 | 2 | 6 | visual | 🟢 |
| 2026-05-19 | [[Жеті Ата]] | CLAUDE.md + 7-task план (заявка №3, $250K) | 3 | 1 | 2 | document+analysis | 🟡 |
| 2026-05-20 | [[ВНД Agent]] | Правовая экспертиза + flowchart generator v1.0 + APS-ORD-PRO-MAR-001 v2.2 | 8 | 2 | 6 | document+analysis | 🟢 |
| 2026-05-22 | [[1966 Plateau Deck]] | Тендеры БИО+RAK анализ + протокол встречи + PM-сессия | 6 | 2 | 4 | analysis+document | 🟡 |
| 2026-05-22 | [[Медео Парк Отель]] | Письмо Medeu Park Hotel | 1 | 0.25 | 0.75 | negotiation | 🟡 |
| 2026-05-25 | [[Completed Deliverables]] | Похвальный лист (дизайн) | 2 | 0.5 | 1.5 | visual | 🟢 |
| 2026-05-27 | [[Completed Deliverables]] | Билингвальное письмо RU/ZH партнёру (к встрече Акционер+Аким 05.06) | 2 | 0.5 | 1.5 | negotiation+translation | 🔴 |
| 2026-05-28 | [[ZILLI Marketing Services]] | ZILLI_Instagram_Audit_v2.docx — 16 стр., Quiet Power styling | 5 | 1.5 | 3.5 | document+analysis | 🟡 |
| 2026-05-28 | [[Drone Delivery Almaty]] | AEROLINK futuristic HTML-demo (Three.js+GSAP, Almaty coords) | 10 | 2 | 8 | visual | 🟡 |
| 2026-05-28 | [[AMC - Almaty Mountain Cluster]] | BIG KYC-форма (8 разделов) + EN-reply | 2 | 0.5 | 1.5 | document+negotiation | 🟡 |
| 2026-05-28 | [[AMC - Almaty Mountain Cluster]] | Butakovka topo survey RU→EN (bilingual, глоссарий 26 терминов) | 6 | 1 | 5 | translation | 🟡 |
| 2026-05-28 | [[AMC - Almaty Mountain Cluster]] | Engineering-geology survey RU→EN — 24 стр., полное зеркало | 8 | 1.5 | 6.5 | translation | 🟡 |
| 2026-05-29 | [[1966 Plateau Deck]] | HR org-структура fine dining (~25 позиций, 5 блоков) | 5 | 1.5 | 3.5 | analysis+document | 🟡 |
| 2026-05-29 | [[ZILLI Marketing Services]] | Strategy Landing v3.html 345КБ (вкл. откат v0.7 → пересборка) | 16 | 5 | 11 | visual | 🟡 |
| 2026-05-29 | [[Cinnabon Carvel Master Franchise]] | M&A-пакет: 4 deliverables + email-summary ($500K мастер-права KZ+KG) | 8 | 2 | 6 | analysis+negotiation | 🔴 |
| 2026-05-31 | [[Drone Delivery Almaty]] | Deck для Акима/Акционера: слайды 2/5/6/7/10 + регулятор fix + CAPEX 409 | 4 | 1.5 | 2.5 | visual+analysis | 🔴 |
| 2026-05-31 | [[ZILLI Marketing Services]] | 29 кадров Pixa NB2 + Landing v3.2 + локальный wordmark | 8 | 2.5 | 5.5 | visual | 🟡 |
| 2026-05-31 | [[AMC - Almaty Mountain Cluster]] | Yuji round 2: письмо (Pioneer 1,900 m Hotel + 1,700 m MFB) — отправлено | 1.5 | 0.5 | 1 | negotiation | 🟡 |
| 2026-06-01 | [[Drone Delivery Almaty]] | Deck: 3 ROI-сценария + CAPEX 409→392 | 3 | 1 | 2 | analysis | 🔴 |
| 2026-06-01 | [[AMC - Almaty Mountain Cluster]] | YYA SD proposal $176K — разбор | 2 | 0.75 | 1.25 | analysis | 🟡 |
| 2026-06-01 | [[AMC - Almaty Mountain Cluster]] | Basalt: masterplan-first reply | 1 | 0.25 | 0.75 | negotiation | 🟡 |
| 2026-06-07 | [[Completed Deliverables]] | Mortgage Report v6 (.docx 21 стр., 17 квартир, 5 маршрутов, сравнит. матрица, 10 чартов embedded) | 10 | 3 | 7 | document+analysis | 🟢 |
| 2026-06-09 | [[Drone Delivery Almaty]] | Деки Акционеру & Акиму: калькулятор встроен (Chart.js), фазы пересчитаны (18-мес.) | 8 | 2.5 | 5.5 | visual+analysis | 🔴 |
| 2026-06-09 | [[Autonomous Pharmacy Retail]] | Investor deck HTML (19 слайдов) + финанализ (×2.4 наценка Gemini) + DZIN промпт | 6 | 2 | 4 | visual+analysis | 🔴 |
| 2026-06-09 | [[AMC - Almaty Mountain Cluster]] | Email Hulda (Basalt Architects) — дедлайны fee proposal 12.06 & portfolio 9.06 | 0.5 | 0.25 | 0.25 | negotiation | 🟡 |
| 2026-06-09 | [[ZILLI Marketing Services]] | Taplink-мокап (HTML) к брендбуку + письмо Рысты (запрос обратной связи) | 4 | 1.5 | 2.5 | visual+negotiation | 🟡 |
| 2026-06-09 | [[Ecole Ducasse Almaty Studio]] | Follow-up Anne-France (3 вопроса по не-ED) + 2 WA-резюме Дияру/Дине | 1.5 | 0.5 | 1 | negotiation | 🟡 |
| 2026-06-10 | [[Drone Delivery Almaty]] | Финмодель v2: OPEX 300/CAPEX 520, калькулятор верифицирован, письмо Александру | 8 | 2.5 | 5.5 | analysis+visual | 🔴 |
| 2026-06-10 | [[Постаматы Almaty]] | Финмодель v2: дека v3 (22 слайда) + калькулятор v2.html + письмо Александру | 8 | 2.5 | 5.5 | analysis+visual | 🔴 |
| 2026-06-10 | [[AMC - Almaty Mountain Cluster]] | holding-ответ Yuji (EN) + письмо Ивану Вл. (RU) — сравнение 3 roadmap YYA | 3 | 1 | 2 | negotiation+analysis | 🔴 |
| 2026-06-10 | [[Completed Deliverables]] | Резюме статус-встречи 28.05 (.docx, 17 следующих шагов, ответственные, сроки) | 2 | 0.5 | 1.5 | document | 🟢 |
| 2026-06-10 | [[Completed Deliverables]] | Консолидация стратегической сессии: 11 топиков (.docx, дедупликация транскрипта) | 3 | 1 | 2 | document+analysis | 🟢 |
| 2026-06-11 | [[Autonomous Pharmacy Retail]] | Деки v2: Акиму (17 сл.) + Акционеру (21 сл.), офлайн-самодостаточные, финмодель CAPEX 144M ₸, 3 IRR-сценария (2.6/29.5/8.5%), рынок 667.7B ₸ | 8 | 2.5 | 5.5 | visual+analysis | 🔴 |
| 2026-06-12 | [[Claude Training — Marketing Team]] | Тренинг_AI_Маркетинг_Almaly_v1.html (54 сл., 10 блоков, 3 практикума, заметки докладчика) | 12 | 3 | 9 | visual+content | 🟢 |
| 2026-06-14 | [[Personal Brand @arshat]] | Instagram-аудит @arshat (11 секций, 6 критических gap'ов, 3 приоритета 48ч, 5 контентных столпов) | 2.5 | 0.5 | 2 | analysis | 🟢 |
| 2026-06-16 | [[Claude Training — Marketing Team]] | Training v2: 58 сл., обновлённый контент 10 блоков | 8 | 2 | 6 | visual+content | 🟢 |
| 2026-06-16 | [[ZILLI Marketing Services]] | SMM KPI — 3 варианта сценариев (targets by tier) | 2 | 0.5 | 1.5 | analysis | 🟡 |
| 2026-06-16 | [[AMC - Almaty Mountain Cluster]] | Basalt: ревью commercial proposal + email Hulde | 1.5 | 0.5 | 1 | negotiation+analysis | 🟡 |
| 2026-06-17 | [[Freedom Media — FK Kairat Documentary]] | FK Kairat deal 50M ₸ — анализ условий + структура сделки | 3 | 1 | 2 | analysis+negotiation | 🟡 |
| 2026-06-17 | [[Almaly Brand System & Skills]] | Web stack DOCX 9 стр. + 4-phase roadmap (инфраструктура бренд-активов) | 4 | 1 | 3 | document+analysis | 🟢 |
| 2026-06-18 | [[AMC - Almaty Mountain Cluster]] | Записка Президенту: AMC тендер закрыт (итоги + следующие шаги) | 4 | 1 | 3 | document | 🔴 |
| 2026-06-18 | [[ZILLI Marketing Services]] | Toplink-мокап + DM doc 4p (продуктовый пакет) | 3 | 0.75 | 2.25 | visual+document | 🟡 |
| 2026-06-19 | [[Drone Delivery Almaty]] | Дека Акиму v12 (обновление: фазы, финмодель) | 4 | 1.5 | 2.5 | visual+analysis | 🔴 |
| 2026-06-19 | [[Медео Парк Отель]] | Medeu Park Hotel — PPTX v2 14 сл. (стратегия открытия) | 5 | 1.5 | 3.5 | visual | 🟡 |
| 2026-06-19 | [[1966 Plateau Deck]] | BURO TABLETOP: аудит контракта (риски + рекомендации) | 3 | 1 | 2 | analysis+document | 🟡 |
| 2026-06-16 | [[Autonomous Pharmacy Retail]] | DZIN-промпт v2 + Pass A (4 слабые зоны) | 3 | 0.75 | 2.25 | analysis+content | 🔴 |
| 2026-06-17 | [[Almaly Brand System & Skills]] | Tool map HTML (18 кат. / 74 инструмента) | 3 | 0.5 | 2.5 | visual | 🟢 |
| 2026-06-19 | [[Autonomous Pharmacy Retail]] | Аптека: orange visual spec (новый цвет. слой) | 2 | 0.5 | 1.5 | visual | 🔴 |
| 2026-06-22 | [[1966 Plateau Deck]] | Мебель: сравнение 38 позиций Mars/VilleDoors/Китай (Excel, ≈25М ₸ экономии) | 5 | 1.5 | 3.5 | analysis | 🟡 |
| 2026-06-22 | [[Медео Парк Отель]] | EY event: шортлист 4 площадок | 2 | 0.5 | 1.5 | analysis | 🟡 |
| 2026-06-23 | [[1966 Plateau Deck]] | Dashboard переговорная эффективность 7,2М ₸ + протокол №05 | 5 | 1.5 | 3.5 | analysis+document | 🟡 |
| 2026-06-23 | [[Completed Deliverables]] | Протокол рабочей встречи (.docx) | 1.5 | 0.5 | 1 | document | 🟢 |
| 2026-06-23 | [[Completed Deliverables]] | Almaly Speech calendar 4K | 2 | 0.5 | 1.5 | visual | 🟢 |
| 2026-06-24 | [[Медео Парк Отель]] | Кресла MARS 701 171 ₸ + бокалы Spiegelau (условия 25%+25%) — аналитика закупки | 2 | 0.5 | 1.5 | analysis+negotiation | 🟡 |
| 2026-06-24 | [[1966 Plateau Deck]] | ALEDO КП DOCX 19,98М ₸ — ревью коммерческого предложения | 3 | 1 | 2 | analysis+document | 🟡 |
| 2026-06-24 | [[1966 Plateau Deck]] | Kaizen dashboard v2 | 3 | 1 | 2 | visual+analysis | 🟡 |
| 2026-06-27 | [[Food Packaging]] | Протокол стратегической встречи 26.06 (.docx, 11 разделов, 18 задач, воронка клиентов) | 3 | 0.5 | 2.5 | document | 🟢 |
| 2026-06-30 | [[1966 Plateau Deck]] | Анализ КП столярки 38 позиций: рекомендации по сплиту Mars/VilleDoors/Китай (потенциал экономии ≈25М ₸) | 3 | 1 | 2 | analysis | 🟡 |
| 2026-06-30 | [[1966 Plateau Deck]] | Строительная смета ред.2 (DOCX, 9 стр.): бетон/столярка/МДФ, диапазон 233,9–266,7 млн ₸ | 3 | 0.75 | 2.25 | analysis+document | 🟡 |
| 2026-06-30 | [[ВНД Agent]] | Compliance audit MAR-001/007: MAR-001 к согласованию (9/10) + MAR-007 доработка (4/10), DOCX-заключение | 3 | 1 | 2 | analysis+document | 🟢 |
| 2026-06-30 | [[ВНД Agent]] | Compliance audit MAR-006/008/010: 3 × FAIL, детализированные замечания по критическим нарушениям | 4 | 1.25 | 2.75 | analysis | 🟢 |
| 2026-06-30 | [[Drone Delivery Almaty]] | Collaboration DOCX (5 стр.): дроны + автономные магазины, восстановлен и пересобран | 2 | 0.5 | 1.5 | document | 🟡 |
| 2026-07-01 | [[1966 Plateau Deck]] | Маркетинговый бриф запуска (DOCX, 7 стр.): платформа «выше обыденного», тизеры A/B «1966» vs «Восхождение», таймлайн от 01.09 | 5 | 1 | 4 | document+content | 🟡 |
| 2026-07-01 | [[ВНД Agent]] | Compliance audit MAR-002/003/004/005/009: 5 документов (8.5/10 + 7/10 + 8.5/10 + FAIL 6крит. + FAIL 6крит.) | 6 | 1.5 | 4.5 | analysis | 🟢 |
| 2026-07-01 | [[Drone Delivery Almaty]] | Письмо Акционеру: итоги Акимат 26.06 по 4 направлениям (дроны → пауза; аптека → зел.свет + кит.финансирование; Цифровой двойник → эксклюзив) | 2.5 | 0.5 | 2 | negotiation+analysis | 🔴 |
| 2026-07-03 | [[1966 Plateau Deck]] | Музыкальная система: анализ 4 вариантов КП (Вариант 4 = 15 156 335 ₸, 2 gap: речевое оповещение + 5-зонность) | 2.5 | 0.5 | 2 | analysis | 🟡 |
| 2026-07-03 | [[1966 Plateau Deck]] | Строительная смета ред.3 (13 стр., раздел 8 Камины: GlammFire €8 125 + 3 поставщика портала 4.52–5.18 млн ₸) | 3 | 0.75 | 2.25 | analysis+document | 🟡 |
| 2026-07-04 | [[ZILLI Marketing Services]] | SMM RACI+SLA doc (2 стр. A4 Word): RACI-матрица + SLA ≤15 мин, 3 шага внедрения | 2.5 | 0.5 | 2 | document+analysis | 🟡 |
| 2026-07-04 | [[1966 Plateau Deck]] | Протокол встречи №07 (04.07, DOCX): финансирование 350–400 млн ₸, камин=спиртовой, ответственные Батырбеков+Толеубаев | 1.5 | 0.5 | 1 | document | 🟡 |
| 2026-07-04 | [[1966 Plateau Deck]] | Kaizen cost-out ≈77,6 млн ₸ + «Зоны развития» 12-я вкладка | 4 | 1 | 3 | analysis | 🟡 |
| 2026-07-09 | [[Ecole Ducasse Almaty Studio]] | Email Emilie Leducq: Paris PR tour (EN, детали партнёрства + запрос условий) | 1.5 | 0.25 | 1.25 | negotiation | 🟡 |
| 2026-07-09 | [[Almaty City Digital Twin]] | RFI + аналитическая записка в Акимат (цифровой двойник, основания для эксклюзива) | 6 | 1.5 | 4.5 | document+analysis | 🔴 |
| 2026-07-10 | [[Almaly Brand System & Skills]] | Marketing Book DOCX (10 механик маркетинга) | 4 | 1 | 3 | document+content | 🟢 |
| 2026-07-11 | [[Dorchester Academy]] | КП + ответное письмо партнёру (коммерческое предложение) | 3 | 1 | 2 | negotiation+document | 🟡 |
| 2026-07-11 | [[1966 Plateau Deck]] | Рекрутинг-DOCX v2: вакансии через Hurma+Rabotarestoran | 3 | 0.75 | 2.25 | document+analysis | 🟡 |
| 2026-07-11 | [[Медео Парк Отель]] | Протокол встречи 10.07 (посуда: Denis+Mars) | 1.5 | 0.5 | 1 | document | 🟢 |
| 2026-07-13 | [[1966 Plateau Deck]] | Мебель v2: Mars 65М₸ vs Китай 40.8М₸ (КП №1354, рекомендация сплита) | 4 | 1 | 3 | analysis | 🟡 |
| 2026-07-13 | [[1966 Plateau Deck]] | Финмодель ресторана аудит P&L: 32 критических вопроса команде Волкова/Мади | 3 | 1 | 2 | analysis | 🟡 |
| 2026-07-13 | [[1966 Plateau Deck]] | Повестка совещания №08 (9 блоков, протоколы №06+07, прототипы Mars 16-17.07) | 1 | 0.25 | 0.75 | document | 🟡 |
| 2026-07-13 | [[AMC - Almaty Mountain Cluster]] | Письма Alie Tolba (BIG) + Hrólfur (Basalt): статус мастер-плана, ориентир сен-окт 2026 | 1 | 0.25 | 0.75 | negotiation | 🟡 |
| 2026-07-14 | [[Медео Парк Отель]] | GAP-дашборд v2: БИО ДС1 −93 922 ₸ + Complex-Bar −15% + итого экономии 11,81 млн ₸ | 3 | 1 | 2 | analysis+visual | 🟡 |
| 2026-07-15 | [[1966 Plateau Deck]] | Рекрутинг Michelin-шефа: Word-свод (18 параметров, бюджеты A/B/C, рекомендация Hurma) | 5 | 1.5 | 3.5 | analysis+document | 🟡 |
| 2026-07-15 | [[1966 Plateau Deck]] | Реестр поставщиков Excel (112 файлов → 15 папок, ключевые суммы 255+ млн ₸) | 3 | 1 | 2 | analysis+document | 🟡 |
| 2026-07-15 | [[1966 Plateau Deck]] | Прототипы стульев Mars vs Китай: Word 4 стр. (конструктивная несравнимость, единое ТЗ) | 3 | 1 | 2 | analysis+document | 🟡 |
| 2026-07-15 | [[ВНД Agent]] | Compliance audit MAR-001 (9/10 PASS) + MAR-007 (8,5/10 PASS): DOCX-заключение | 4 | 1 | 3 | analysis | 🟢 |
| 2026-07-15 | [[Almaty City Digital Twin]] | Протокол встречи UTM/eVTOL 5 стр.: subscription/service модель подтверждена | 2 | 0.5 | 1.5 | document | 🔴 |
| 2026-07-16 | [[Медео Парк Отель]] | Протокол маркетинг-встречи 08.07 DOCX: правки Аршата (состав, план действий, сроки) | 2.5 | 0.5 | 2 | document | 🟡 |
| 2026-07-17 | [[1966 Plateau Deck]] | Бриф персонала DOCX 3 стр.: шеф/сомелье/бар-менеджер (вкусовые принципы + меню-вектора) | 3 | 0.75 | 2.25 | document+content | 🟡 |
| 2026-07-17 | [[1966 Plateau Deck]] | Адаптивное цифровое меню HTML (mobile breakpoints ≤900/≤440px, ready to deploy) | 5 | 1.5 | 3.5 | visual | 🟡 |
| 2026-07-17 | [[1966 Plateau Deck]] | Шорт-лист Michelin-шефов DOCX 3 стр.: лид Groupe Yannick Alléno (18★), параллельно Groupe Pic | 6 | 1.5 | 4.5 | analysis+document | 🔴 |
| 2026-07-17 | [[Медео Парк Отель]] | RACI-матрица объекта: 21 зона × 16 участников (протоколы №05-08) | 4 | 1 | 3 | document+analysis | 🟡 |
| 2026-07-18 | [[1966 Plateau Deck]] | ТЗ эргономики кресла Mars по ГОСТ 17524.1/2-93: высота 44-46, глубина 44-46, ширина ≥50 (передано поставщику) | 1.5 | 0.25 | 1.25 | document | 🟡 |
| 2026-07-18 | [[1966 Plateau Deck]] | Роадмап рекрутинга Michelin-шефа + письма Hurma/Rabotarestoran (дедлайн 24.07, 5 шагов) | 2.5 | 0.5 | 2 | negotiation+analysis | 🟡 |
| 2026-07-19 | [[Ecole Ducasse Almaty Studio]] | Paris PR тур: адженда согласована с Emilie Leducq (окно 2-7 дек, 10 чел, 3 дня, 5 кандидатов) | 2 | 0.5 | 1.5 | negotiation+document | 🟡 |
| 2026-07-22 | [[Almaty City Digital Twin]] | Письмо китайским партнёрам (RU+ZH): статус паузы + ориентир возобновления сен–окт 2026 | 1.5 | 0.25 | 1.25 | negotiation+translation | 🔴 |
| 2026-07-22 | [[1966 Plateau Deck]] | RACI-матрица проекта: 21 зона × 16 участников (разграничение ответственностей согласовано) | 4 | 1 | 3 | document+analysis | 🟡 |
| 2026-07-25 | [[Almaty City Digital Twin]] | HTML-дека Digital Twin v2: 5 AI-рендеров (аэрофото Алматы, 3D-слои инфраструктуры, LiDAR, ситуац. центр, тест. район) — для демонстрации Акимату | 12 | 3 | 9 | visual | 🔴 |
| 2026-07-27 | [[ZILLI Marketing Services]] | Taplink финализация: 3 бутика production-ready (Достык/Talan Gallery/Рихос Хадиша; телефоны зафиксированы) | 3 | 0.75 | 2.25 | visual+content | 🟡 |
| 2026-07-27 | [[Медео Парк Отель]] | Протокол встречи 23.07: сценарий открытия (художественная мистерия; тайминг 17:00–23:00; гости 100–150; бюджет 50↔20 млн₸; дата 24.09.2026) — DOCX | 4 | 1 | 3 | document | 🟡 |
| 2026-07-27 | [[Медео Парк Отель]] | Дашборд ревизия 20 правок (поставщики финал; посуда оплачена; леджер 168 114 858 ₸; открытых вопросов 11→9) | 2 | 0.5 | 1.5 | analysis+visual | 🟡 |
| 2026-07-30 | [[1966 Plateau Deck]] | Дашборд финализация перед Акционером: Китай убран; посуда RAK+MEPRA оплачена; платёжный леджер 168 114 858 ₸ | 2 | 0.5 | 1.5 | analysis+visual | 🟡 |
| 2026-07-30 | [[Медео Парк Отель]] | Протокол встречи 22.07 с Яндексом: форматы Ultima Еда/Гид; долгосрочные интеграции; KPI; канон имён — DOCX для рассылки партнёрам | 3.5 | 1 | 2.5 | document+negotiation | 🟡 |
| 2026-07-30 | [[Медео Парк Отель]] | Письмо Ducasse Conseil (Marie-Pia de Roquefeuil): hotel operational advisory; pre-opening SOPs — EN | 1 | 0.25 | 0.75 | negotiation | 🟡 |
| 2026-08-06 | [[Ecole Ducasse Almaty Studio]] | Paris PR тур: программа пересмотрена по 6 комментариям Дияра (Q1 2027 confirmed; Médон без ED-презентации; Paris Studio с Q&A) + письмо Emilie Leducq | 2 | 0.5 | 1.5 | document+negotiation | 🟡 |
| 2026-08-07 | [[1966 Plateau Deck]] | Hurma Michelin pitch analysis (Ana Roš 3★ / Maksut Ashkar 1★ / Francisco Araya 1★) + инициативное письмо Акционеру | 2 | 0.5 | 1.5 | negotiation+analysis | 🔴 |
| 2026-08-26 | [[Медео Парк Отель]] | Письмо George Togonidze (Marriott franchise, BATNA к DCA) — hotel operational advisory request EN; ответ получен, созвон подтверждён | 0.75 | 0.25 | 0.5 | negotiation | 🟡 |
| 2026-08-26 | [[Медео Парк Отель]] | Re-approach письмо Marie-Pia de Roquefeuil (Ducasse Conseil) — hotel service advisory, expanded scope beyond F&B EN | 0.75 | 0.25 | 0.5 | negotiation | 🟡 |

**Итого 25 апр → 31 авг:** **146 deliverables**, **491.75 ч сэкономлено**

## Q2 2026 Summary (25 апр — 27 июн, финальный черновик для 30.06)

### По типу leverage (часов сэкономлено)

| Категория | Saved | Доля |
|---|--:|--:|
| visual (вкл. visual+X) | 224.5 ч | 59.5% |
| analysis (вкл. analysis+X) | 189.25 ч | 50.1% |
| document (вкл. document+X) | 89.25 ч | 23.7% |
| content (вкл. content+X) | 57.25 ч | 15.2% |
| negotiation (вкл. negotiation+X) | 33.5 ч | 8.9% |
| translation | 13 ч | 3.4% |

> Категории пересекаются (один deliverable → два leverage'а), сумма >100% — норма.

### По критичности

| Уровень | Saved | N | Доля |
|---|--:|--:|--:|
| 🔴 critical | 74.5 ч | 18 | 19.8% |
| 🟡 high | 216 ч | 53 | 57.2% |
| 🟢 medium | 86.75 ч | 25 | 23.0% |

### Топ-3 ROI-задач Q2

1. **Хасбулатов 70 video production** — 34 ч (~5.7×). 14 AI-портретов NB2 + Veo 3.1 промпты + Ken Burns; без ИИ — внешняя продакшн-студия ($$$).
2. **Annual Shareholder Meeting LED Backdrop v2** — 17 ч (~6.7×). 12-сценовый Bloomberg-стиль HTML; без ИИ — frontend-dev 2–3 дня.
3. **ZILLI Master Strategy v0.4 FINAL** — 15 ч (~4.0×). 103 слайда, 6 блоков; без ИИ — 3–4 дня стратегической работы.

### Наблюдения

> [!success] Visual — главный ROI-драйвер (224.5 ч, 59.5%)
> Деки, HTML-артефакты, AI-генерация (NB2/Veo), презентации. Самый defensible слой — без ИИ требует внешних подрядчиков (дизайнер, frontend, motion).

> [!info] Analysis — второй по объёму (189.25 ч, 50.1%)
> Тендеры, финмодели, аудиты, GAP-анализы. Высокая частотность → кандидаты #1 на playbook'и (см. [[AI System Improvement Backlog]] §«Извлечь 3 промпта»).

> [!warning] Critical-level impact растёт (74.5 ч, 18 deliverables)
> Drone/Pharmacy деки Акиму/Акционеру, M&A Cinnabon, AMC записки Президенту. Максимальный риск галлюцинаций — подкрепляет P0 Verification Layer ([[AI System Improvement Backlog]]).

> [!note] Итог Q2: 96 deliverables / 377.25 ч / ~9.4 недели эквивалентного рабочего времени
> Средний ROI: ~3.9 ч saved per deliverable. Формат финальной сводки — [[Annual Report Framework]].

## Pending для следующих weekly digests

- [ ] **2026-05-09** (пн): захватить deliverables за неделю 02–08 мая
- [ ] **2026-05-16** (пн): первый «полноцикловый» digest, посчитать недельную средневзвешенную
- [ ] **2026-06-30**: Q2 close — одностраничная сводка для Акционера в формате [[Annual Report Framework]] (value-framed executive summary)

## Связанное

- [[AI System Improvement Backlog]] — этот файл закрывает P0 «AI ROI Ledger»
- [[Completed Deliverables]] — источник retro-fill
- [[GAP Partnership Negotiation]] — образец defensible-цифр для разговора с Акционером
- [[Annual Report Framework]] — целевой формат Q2-сводки
