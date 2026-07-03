---
type: idea
priority_score: 1
source: MIGRATION-DEEP-v1
source_module: DEEP-SYSTEM
audit_key: process_idea:codex_large_task_preflight
created: 2026-06-09
applies_to:
  - Codex
  - TiTan Bots wiki
  - workflow
sections:
  - Codex
  - preflight
  - process
  - archive_processed
---

## Оценка ценности

Ценность: `1/10`.

Статус после разбора: `archive_processed / adopted_as_minimal_router`.

Почему ценная:

- может уменьшить расход токенов на архитектурные эксперименты;
- помогает заранее отделить миграцию, аудит, research и смысловые правки;
- снижает риск случайно расширить задачу.

Почему больше не остается активной идеей:

- отдельный skill `preflight` признан лишним;
- полезная часть принята как легкий TiTan-specific роутер внутри `titan-bots-wiki-import`;
- runtime-правило добавлено в `references/task-preflight.md`;
- системное объяснение добавлено в `Система/Правила обработки/Preflight крупных задач Codex.md`.

# Preflight для крупных задач Codex

## Суть идеи

Отдельно проверить, нужен ли preflight-процесс перед крупными задачами Codex.

## Что проверить

- нужен ли preflight внутри `titan-bots-wiki-import`;
- нужен ли отдельный reference-файл;
- нужен ли отдельный skill;
- как фиксировать запреты задачи до начала правок;
- как не тратить токены Codex на архитектурные эксперименты.

## Почему оставить в К разбору сейчас

Текущая задача не меняет `titan-bots-wiki-import`, `agent.md` и skills.

## Возможное решение

Сделать отдельный короткий preflight-чеклист для задач с архивом, аудитом, публичными FAQ и skill-изменениями.

## Решение после технического разбора

Отдельный skill не нужен. Существующие Superpowers уже закрывают общие режимы работы: brainstorming, planning, execution, debugging, TDD, verification and writing-skills. Но они не знают TiTan-specific границы между `К разбору`, `Вики`, `Система`, архивом, public FAQ/RAG, audit-stack, source cleanup и owner-approved queue decisions.

Принято минимальное решение:

- не создавать новый skill;
- добавить легкий reference `task-preflight.md` в `titan-bots-wiki-import`;
- описать правило в `Система/Правила обработки`;
- использовать preflight только для крупных или смешанных задач;
- не применять его как обязательную бюрократию для каждой обычной карточки.

Карточку закрыть как обработанную и перенести в архив.
