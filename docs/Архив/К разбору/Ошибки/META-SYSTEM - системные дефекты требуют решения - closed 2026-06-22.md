---
type: error
owner_layer: META
terminal_owner: META-SYSTEM
priority_score: 7
created: 2026-06-22
source: meta-system-v1.ps1
applies_to:
  - TiTan Bots system
  - audit-stack
  - skills
  - scripts
  - templates
sections:
  - audit
  - meta
  - system
audit_key: meta_system_unresolved_defects
---

# META-SYSTEM - системные дефекты требуют решения

## Оценка опасности

`7/10`

## Lifecycle

Эта карточка живет в `К разбору/Ошибки`.

Это не active/current знание и не источник для RAG. Ошибка является отдельным ручным входом после audit-stack. После исправления нужно вынести lasting lesson в правило / знание / архитектурное уточнение, а саму ошибку убрать из active очереди в `Архив/К разбору` или другой закрытый архив входов.

`К разбору/Ошибки` и `К разбору/Идеи` не являются active wiki. Status/queue/warning/guard не является финальным закрытием без terminal owner или lifecycle destination.

## Суть

META-SYSTEM нашел системные дефекты, которые не помечены как safe auto-fix и требуют отдельного решения владельца или META-прохода.

## Где проявилось

- Система/Шаблоны/Шаблон - SYNTHESIS report.md | META_TEMPLATE_STATUS_CONTAMINATION | owner=META-SYSTEM | template contains status field that can be inherited by new cards. | next=remove service lifecycle status from template unless it is intentional card content

## Причина

Автоматический META-проход не должен оставлять unresolved findings только в generated report. Если дефект нельзя безопасно исправить в рамках audit-run, он должен быть вынесен в grouped manual input.

## Что исправить

- Разобрать findings из текущего META-SYSTEM report.
- Для каждого пункта выбрать: исправить правило/скрипт/шаблон/skill, признать valid exception или закрыть как stale.
- После исправления повторно запустить META-SYSTEM.

## Как проверить

- Запустить `Система/Скрипты/meta-system-v1.ps1`.
- Убедиться, что `meta_findings` по этим дефектам стал `0` или остались только новые независимые findings.
- Запустить FAST.

## Что вынести в базу после исправления

- правило;
- process note;
- script rule;
- ничего, если finding признан stale/noise.

## Условие закрытия

Карточку можно убрать из активной папки `К разбору/Ошибки`, когда META-SYSTEM больше не возвращает перечисленные unresolved findings или когда владелец явно признал их valid exception с зафиксированным правилом.

## Итог закрытия

2026-06-22: finding признан ложным срабатыванием META-чека. `Шаблон - SYNTHESIS report.md` является шаблоном generated audit report, поэтому `type: audit_report` + `status: generated` допустимы. Исправлен сам META-чек и правило META: service `status` запрещен для active/manual card templates, но разрешен для generated audit report templates.
