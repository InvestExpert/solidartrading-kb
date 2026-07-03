---
type: idea
priority_score: 7
created: 2026-06-24
source: Graphify YouTube video https://youtu.be/94YEb8_qnRc?si=NO8tIDcePOTt81FH; prior Codex preflight 2026-06-17
applies_to:
  - Codex workflow
  - Obsidian
  - TiTan Bots wiki
  - RAG
  - token optimization
sections:
  - AI infrastructure
  - knowledge graph
  - workflow
---

# Graphify - проверить граф знаний для Codex и Obsidian

## Оценка ценности

7/10.

Идея потенциально ценная, потому что TiTan Bots база быстро растет, а Codex/RAG/аудитам нужно точнее находить связанные знания без лишнего чтения всего подряд. Но Graphify пока является внешним инструментом с непроверенными claims, поэтому карточка остается в `К разбору/Идеи`.

## Lifecycle

Эта карточка живет в `К разбору/Идеи`.

Это не active/current знание и не ответ для RAG. Идея становится active wiki-карточкой только после проверки, решения владельца и отдельного переноса в нужный слой.

## Идея

Проверить, может ли Graphify или похожий knowledge-graph слой помочь Codex работать с TiTan Bots wiki: строить граф связей, быстрее находить связанные карточки, снижать context waste и лучше видеть смысловые зависимости между Obsidian-карточками, skills, правилами, аудитами и проектными материалами.

## Почему может быть ценно

- Может помочь находить связанные знания не только через `rg`, но и через граф отношений.
- Может быть полезно для больших задач, где Codex должен видеть связи между идеями, активной wiki, skills, audit-stack и archive.
- Может снизить риск пропуска старых идей, дублей и связанных process-rules.
- Может стать внешним аналогом или дополнением к текущим подходам: Obsidian graph, `context-mode`, audit-stack, RAG/FAQ routes, source registry.

## К чему относится

- TiTan Bots wiki.
- Codex workflow.
- Obsidian graph hygiene.
- RAG / Telegram bot retrieval.
- Token optimization and context rot prevention.
- Future smart audit / synthesis layer.

## Что проверить

- Что Graphify реально делает локально: индекс, граф, поиск, визуализация, retrieval или все вместе.
- Можно ли безопасно подключать его к TiTan Bots workspace без утечки ключей, приватных файлов, архивов, сырья и broker/account данных.
- Умеет ли он нормально работать с Obsidian wiki-links, Markdown frontmatter, кириллицей и большим числом файлов.
- Дает ли он практическую пользу сверх `rg`, Obsidian graph, `context-mode`, текущих METHOD/DEEP/SYNTHESIS проверок и ручных индексов.
- Можно ли использовать его не как новую бюрократию, а как точечный инструмент перед большими задачами: поиск дублей, связанных owner-карточек, route gaps, stale process rules.
- Есть ли более подходящий Codex-native аналог: MCP, local index, context-mode, graph script, custom audit layer.

## Как понять, что идея сработала

- На реальной задаче Graphify находит связанные карточки, которые обычный `rg` или текущий audit-stack легко пропускают.
- Время на preflight больших задач сокращается без потери качества.
- Снижается число пропущенных дублей, старых хвостов и неверных placement decisions.
- Инструмент не требует постоянного ручного обслуживания и не создает отдельную шумную базу.

## Риски или сомнения

- Claims из видео не считать подтвержденными.
- Может оказаться рекламным или слишком общим инструментом без пользы для нашей структуры.
- Может дублировать `context-mode`, Obsidian graph или будущий SYNTHESIS/META слой.
- Может создать риск приватности, если индексирует лишние файлы.
- Может усложнить workflow, если его придется запускать вручную перед каждой мелкой задачей.

## Источник

- Graphify video: https://youtu.be/94YEb8_qnRc?si=NO8tIDcePOTt81FH
- Previous Codex preflight: идея была разобрана 2026-06-17, но файл не был создан из-за остановки на approval/preflight.

## Почему карточка пропала

Проверка 2026-06-24 показала: Graphify не был частью другой идеи и не был закрыт архивом. Предыдущий разбор остановился на preflight/approval до записи в wiki, поэтому смысл остался только в Codex memory / attachment context. Обычный vault-аудит и архивный просмотр не могли найти карточку, потому что файла в `К разбору/Идеи`, `Вики` или `Архив/К разбору` тогда не существовало.

Связанные карточки про RLM, Hermes Desktop, Codex workflow, AI-инфраструктуру и оптимизацию токенов являются соседними темами, но не закрывают Graphify как отдельную идею проверки knowledge-graph слоя.

## Связанные материалы

- [RLM для рекурсивного аудита и защиты от context rot](./RLM%20%D0%B4%D0%BB%D1%8F%20%D1%80%D0%B5%D0%BA%D1%83%D1%80%D1%81%D0%B8%D0%B2%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%B0%D1%83%D0%B4%D0%B8%D1%82%D0%B0%20%D0%B8%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D1%8B%20%D0%BE%D1%82%20context%20rot.md)
- [Правила оптимизации токенов Codex](../../../%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%20%D0%BE%D0%BF%D1%82%D0%B8%D0%BC%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D0%B8%20%D1%82%D0%BE%D0%BA%D0%B5%D0%BD%D0%BE%D0%B2%20Codex.md)
- [Preflight крупных задач Codex](../../../Preflight%20%D0%BA%D1%80%D1%83%D0%BF%D0%BD%D1%8B%D1%85%20%D0%B7%D0%B0%D0%B4%D0%B0%D1%87%20Codex.md)
- [Skills в работе TiTan Bots](../../../Skills%20%D0%B2%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B5%20TiTan%20Bots.md)
