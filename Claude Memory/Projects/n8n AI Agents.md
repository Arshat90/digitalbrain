---
type: project
status: active
created: 2026-04-23
updated: 2026-07-02
tags: [claude-memory, project/active, automation, ai, no-code]
---

# n8n AI Agents

## Суть

Построение **AI-агентной архитектуры** на базе **n8n Cloud**. Фокус — no-code/low-code деплой.

- **Инстанс:** `arshat.app.n8n.cloud`
- **Основной стек:** Telegram → n8n → Claude
- **Интеграция:** Claude через Anthropic API внутри n8n-workflow

## Use-cases

### Автоматизация внутренних регуляторных документов
Генерация документов в соответствии с **процедурами документооборота KUA Almaly**. Основной use-case, на который архитектура проектировалась.

### Almaty Business FM — Telegram Bot
Workflow для радиостанции Almaly Business FM (ДЗО холдинга). В работе:
- `Almaly Business FM Telegram Bot.patched.json`
- `Almaly Business FM Telegram Bot.v2.json`
- `Review-Almaly-Bot.md`, `Prompt-for-Claude-Code.md`

Артефакты лежат в рабочей папке проекта; v2 параллельна с patched-версией.

## Связанные навыки

- Промпт-инжиниринг для генеративных задач
- Интеграция с корпоративными источниками (Google Drive, Outlook)

## Ссылки

- [[Annual Report Framework]] — похожий паттерн: промпт → источник данных → форматированный выход

## Changelog

### 2026-07-02 — Sonnet 5 вышел: обновить роутинг n8n до 31.08 (intro-цена) ^cl-2026-07-02-n8n-sonnet5

- **Sonnet 5 вышел ~30.06.2026**, intro-цена $2/$10 за 1M токенов (input/output) действует ==до 31.08.2026==. Необходимо обновить default-роутинг n8n-workflow с текущей модели на `claude-sonnet-5`.
- **Новый модельный ряд Anthropic:** `claude-fable-5` (1M контекст, 128K output, always-on adaptive thinking — для стратегии/юридики/крупных транскриптов); `claude-opus-4-8`; `claude-sonnet-5`; `claude-haiku-4-5-20251001`.
- **Выявленный пробел:** Marketing data layer оценён в 2.0/10 — нет подключённого MCP для ad analytics (Google/Meta Ads), web analytics (GA4) или CRM (HubSpot). Рекомендованы Supermetrics / Windsor.ai / HubSpot connector.
- **Нативная мультиагентность** (subagents, agent teams, dynamic workflows) доступна в Claude Agent SDK — приоритет для Pass A флота (3–5 параллельных субагентов). Источник: `local_a100808b`.
