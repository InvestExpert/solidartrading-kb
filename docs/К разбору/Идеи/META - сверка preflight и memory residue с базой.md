---
type: idea
priority_score: 9
created: 2026-06-24
source: Graphify miss root-cause analysis 2026-06-24
applies_to:
  - audit-stack
  - Codex workflow
  - wiki import
  - archive lifecycle
sections:
  - META
  - SYSTEM
  - audit
---

# META - сверка preflight и memory residue с базой

## Оценка ценности

9/10.

Идея важная, потому что Graphify показал реальный сбой процесса: полезная идея была разобрана Codex, но не попала ни в `К разбору/Идеи`, ни в активную wiki, ни в архив. Она осталась только в Codex memory / attachment context и всплыла случайно.

## Lifecycle

Эта карточка живет в `К разбору/Идеи`.

Это не active/current знание и не ответ для RAG. Идея становится active system/audit rule только после отдельного решения владельца и внедрения в правильный слой.

## Идея

Добавить в audit-stack или рядом с ним отдельный META/SYSTEM-контроль, который проверяет: нет ли полезных preflight-разборов, attachment-source, memory residue или partial outcomes, которые были разобраны Codex, но не получили явный lifecycle outcome в базе.

Цель не в том, чтобы читать весь архив смыслом каждую неделю. Цель - ловить именно процессный провал: `идея/ошибка/правило были найдены, но остались только в чате, памяти или attachment`.

## Почему может быть ценно

- Снижает риск, что важная идея потеряется из-за остановки на approval/preflight.
- Ловит случаи, где Codex сделал анализ, но не создал карточку, ошибку, правило, active knowledge или archive note.
- Защищает принцип: ни один полезный остаток не остается только в чате.
- Помогает проверять завершенность больших imports, видео-разборов, skill/process обсуждений и системных задач.

## К чему относится

- META / SYSTEM audit.
- `titan-bots-wiki-import`.
- YouTube / transcript / attachment imports.
- Preflight крупных задач.
- Codex memory and rollout summaries.
- Archive lifecycle.

## Что проверить

- Где лучше жить контролю: отдельный META-check, пункт в `audit-deep.ps1`, отдельный on-demand script или правило import-closeout.
- Нужно ли сканировать только Codex memories / rollout summaries / `.codex/attachments`, или еще `К разбору/Сырье` и архив обработанных источников.
- Какие сигналы искать детерминированно:
  - `no wiki files were edited`;
  - `stopped before edits`;
  - `Outcome: partial`;
  - `target page name`;
  - `asked for confirmation before editing`;
  - `добавь в идеи`;
  - `не упустить`;
  - `полезный остаток только в чате`.
- Как сверять кандидата с реальной базой:
  - есть карточка в `К разбору/Идеи`;
  - есть ошибка в `К разбору/Ошибки`;
  - смысл внедрен в active wiki;
  - источник закрыт в archive;
  - идея отклонена в `Архив/К разбору/Отклонено`.
- Как оформлять найденный residue: grouped error, grouped idea, или отчет с точным списком.

## Как понять, что идея сработала

- Повторная проверка Graphify больше не показывает missing outcome.
- Preflight-only источники не остаются только в Codex memory.
- Если Codex остановился до записи файлов, следующая проверка явно показывает pending outcome.
- Проверка не создает массового шума и не превращает каждую старую фразу в новую карточку.

## Риски или сомнения

- Нельзя превращать это в тяжелый семантический аудит всего архива.
- Нельзя автоматически создавать active knowledge из старых memory snippets без проверки.
- Нельзя считать `Outcome: partial` ошибкой всегда: иногда partial означает нормальную остановку задачи.
- Нужны строгие правила, чтобы контроль находил реальные пропуски, а не плодил шум.

## Предварительное решение

Не встраивать сразу как новый обязательный слой без отдельного утверждения владельца.

Рекомендуемый формат для обсуждения: `META / intake-outcome reconciliation` - узкая проверка завершенности входящих источников и preflight residue. Она должна отвечать на один вопрос: получил ли каждый полезный найденный остаток явный исход в базе.

## Источник

- Случай Graphify: идея была разобрана 2026-06-17, но файл не был создан из-за остановки на preflight/approval.
- Повторная проверка 2026-06-24: карточка отсутствовала в vault и была восстановлена вручную.

## Связанные материалы

- [Graphify - проверить граф знаний для Codex и Obsidian](./Graphify%20-%20%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%B8%D1%82%D1%8C%20%D0%B3%D1%80%D0%B0%D1%84%20%D0%B7%D0%BD%D0%B0%D0%BD%D0%B8%D0%B9%20%D0%B4%D0%BB%D1%8F%20Codex%20%D0%B8%20Obsidian.md)
- [Preflight крупных задач Codex](../../../Preflight%20%D0%BA%D1%80%D1%83%D0%BF%D0%BD%D1%8B%D1%85%20%D0%B7%D0%B0%D0%B4%D0%B0%D1%87%20Codex.md)
- [Skills в работе TiTan Bots](../../../Skills%20%D0%B2%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B5%20TiTan%20Bots.md)
