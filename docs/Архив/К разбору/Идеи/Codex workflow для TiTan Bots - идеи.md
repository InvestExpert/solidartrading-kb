---
type: idea
priority_score: 1
primary_source_title: Самый полный гайд по Codex от OpenAI (бесплатный курс на 1.5 часа)
primary_source_type: YouTube video
primary_source_url: https://www.youtube.com/watch?v=f05jEOo4hhA
processing_source: codex-workflow-incubator.md; video_raw_codex_full_guide_2026.md
created: 2026-05-21
closed: 2026-06-22
applies_to:
  - TiTan Bots wiki
  - Codex
sections:
  - AI workflow
  - skills
  - agents
  - rules
  - archive_processed
  - absorbed
---

# Codex workflow для TiTan Bots - идеи

## Оценка ценности

`1/10`.

Статус: `archive_processed / absorbed`.

Сборная карточка закрыта. Она была полезна как источник для разбора Codex workflow, но больше не нужна как активная идея: внутри смешаны уже принятые правила, доступные optional-инструменты и спорные будущие темы без единого next action.

## Итоговое решение

Не держать эту карточку в активной очереди.

Полезный смысл уже разнесен по рабочим владельцам:

- `Система/agent.md` - схема работы Codex с vault и слоями базы;
- `Система/Скрипты/update-agent-files.ps1` - обновление локальных `agent.md` в корне и папках разработки;
- `Система/Правила обработки/Preflight крупных задач Codex.md` - легкий router для крупных и смешанных задач;
- `Система/Skills/titan-bots-wiki-import/references/task-preflight.md` - runtime-правило preflight внутри skill;
- `Система/Правила обработки/Skills в работе TiTan Bots.md` - какие skills реально используются, а какие доступны только по задаче;
- `Система/Правила обработки/Регламент регулярных автоматизаций базы.md` - weekly audit-stack и границы автоматизации;
- `Система/Правила обработки/Правила Git-снимков перед изменениями.md` - Git checkpoint через FAST;
- `Система/Скрипты/audit-fast.ps1` - post-FAST checkpoint при значимых изменениях.

## Что уже покрыто

- `agent.md` вместо отдельного `AGENTS.md`: принято и синхронизируется через `update-agent-files.ps1`.
- Preflight крупных задач: принят как легкий router, отдельный skill не нужен.
- "Сначала план, потом правки": покрыто `task-preflight.md`, `idea-import`, `error-triage`, `promotion-import` и текущими правилами approval/scope guard.
- Git checkpoint: встроен в FAST и не является отдельной регулярной задачей.
- Review/verification: для wiki покрыто FAST; для кода используются профильные проверки и Superpowers по задаче.
- Automations: утвержден только weekly full audit-stack по субботам в 15:00 Europe/Berlin.
- Hooks/MCP/background security scans: не включены как постоянный процесс; разрешены только под конкретную задачу или ручной запуск.

## Что осталось как future-направление, но не в этой карточке

- MCP / Context7 / Playwright / внешние tools - рассматривать только под конкретную внешнюю систему и действие. Общий принцип уже закрыт карточкой `AI-инфраструктура TiTan Bots - минимальная сложность`.
- Subagents / RLM / context decomposition - остаются в более точных активных карточках про RLM и Claude Code skills analogs.
- Worktrees - доступны через Superpowers, но не становятся обязательным процессом TiTan Bots.
- `plans.md` - не создавать отдельным обязательным файлом, пока хватает preflight + плана в чате.
- Публичная подача "AI-разработка как trust angle" - не использовать без отдельного owner-решения и copywriter/promotion process.

## Что не выделять в новые карточки сейчас

- `AGENTS.md для TiTan Bots` - дубль текущей модели `agent.md`.
- `plans.md для сложных правок` - риск лишней бюрократии, пока нет повторяемой боли.
- `Git checkpoint перед каждой AI-сессией` - уже решено через FAST threshold.
- `Hooks для всего workflow` - преждевременно и рискованно без конкретного сценария.
- `MCP вообще` - слишком широко; нужен только конкретный MCP под конкретную задачу.

## Вопросы владельцу на будущее

Отдельного решения владельца требуют только эти темы, если они снова станут актуальны:

- использовать ли тему AI-разработки публично в маркетинге;
- нужен ли отдельный worktree-процесс для реальных крупных code experiments;
- нужен ли конкретный MCP под конкретный внешний источник/API;
- нужен ли отдельный subagent-процесс для повторяемого аудита кода или логов.

Пока эти вопросы не требуют активной карточки: они уже представлены в более точных future-идеях или должны появляться только от конкретного кейса.

## Источник

- Видео: `Самый полный гайд по Codex от OpenAI (бесплатный курс на 1.5 часа)`.
- URL: `https://www.youtube.com/watch?v=f05jEOo4hhA`.
- Дополнительное видео: `Codex: ПОЛНЫЙ ГАЙД 2026 (1,5 часовой курс)`.
- URL: `https://www.youtube.com/watch?v=h4b5FwztscA`.
- ChatGPT-выжимки: `codex-workflow-incubator.md`, `video_raw_codex_full_guide_2026.md`.

## Что проверить

Ничего не проверять в этой сборной карточке. Новые действия по Codex workflow оформлять только как конкретные задачи с владельцем, критерием внедрения и проверкой.
