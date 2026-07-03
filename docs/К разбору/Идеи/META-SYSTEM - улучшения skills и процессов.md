---
type: idea
priority_score: 6
created: 2026-07-02
source: meta-system-v1.ps1
applies_to:
  - TiTan Bots system
  - Codex skills
  - audit-stack
sections:
  - skills
  - meta
  - process
audit_key: meta_system_skill_process_improvements
---

# META-SYSTEM - улучшения skills и процессов

## Оценка ценности

`6/10`

## Lifecycle

Эта карточка живет в `К разбору/Идеи`.

Это не active/current знание и не ответ для RAG. Идея становится active wiki-карточкой только после проверки, решения владельца и отдельного переноса в нужный слой.

`К разбору/Идеи` и `К разбору/Ошибки` являются отдельными ручными входами после audit-stack. Status/queue/warning/guard не является финальным закрытием без terminal owner или lifecycle destination.

## Идея

META-SYSTEM нашел не блокирующие улучшения skills или процессов, которые не нужно внедрять автоматически, но нельзя оставлять только в generated report.

## Почему может быть ценно

- .agents/skills/copywriter/references/examples-before-after.md | META_SKILL_REFERENCE_OVERSIZED_ADVISORY | owner=META-IDEA | reference has 642 lines; very large references should usually split into narrower routed parts. | next=review whether this reference should be split into smaller referenced parts
- .agents/skills/titan-bots-wiki-import/references/promotion-import.md | META_SKILL_REFERENCE_OVERSIZED_ADVISORY | owner=META-IDEA | reference has 797 lines; very large references should usually split into narrower routed parts. | next=review whether this reference should be split into smaller referenced parts
- .agents/skills/titan-bots-wiki-import/references/shared-rules.md | META_SKILL_REFERENCE_OVERSIZED_ADVISORY | owner=META-IDEA | reference has 572 lines; very large references should usually split into narrower routed parts. | next=review whether this reference should be split into smaller referenced parts

## К чему относится

- Codex skills;
- skill routing;
- preflight;
- audit-stack;
- process hygiene.

## Что проверить

- Есть ли повторяющийся реальный кейс, который оправдывает улучшение.
- Не дублирует ли улучшение уже существующий skill или rule.
- Нужно ли обновить skill, router, `Skills в работе TiTan Bots` или backlog ideas.

## Как понять, что идея сработала

- Улучшение принято владельцем.
- Skill/rule/router обновлен.
- Повторный META-SYSTEM не возвращает этот advisory как unresolved.

## Риски или сомнения

- Не создавать новые skills без реального повторяющегося кейса.
- Не превращать advisory в blocker.
- Не плодить отдельные панели или registry.

## Источник

- `meta-system-v1.ps1`
- `META-SKILL-HEALTH`

## Связанные материалы

- `Skills в работе TiTan Bots`
- `Skills - идеи развития TiTan Bots`