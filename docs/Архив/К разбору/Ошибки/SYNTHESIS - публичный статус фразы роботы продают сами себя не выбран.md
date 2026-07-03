---
type: error
owner_layer: METHOD
terminal_owner: owner
priority_score: 7
created: 2026-06-19
source: SYNTHESIS
applies_to:
  - SolidarTrading
  - SYNTHESIS
  - marketing
sections:
  - synthesis
  - method
  - traffic
audit_key: synthesis_public_status_robots_sell_themselves
---

# SYNTHESIS - публичный статус фразы роботы продают сами себя не выбран

## Оценка опасности

7/10

## Lifecycle

Эта карточка была создана в К разбору/Ошибки как ручной вход после audit-stack.

Это не active/current знание и не источник для RAG. Ошибка является отдельным ручным входом после audit-stack. После исправления нужно вынести lasting lesson в правило / знание / архитектурное уточнение, а саму ошибку убрать из active очереди в Архив/К разбору или другой закрытый архив входов.

## Суть

SYNTHESIS может использовать концепцию как стратегический мост, но публичный статус самой фразы не зафиксирован достаточно жестко для автогенерации внешних marketing assets.

## Где проявилось

Маркетинговый SYNTHESIS вокруг traffic pages, AI-помощника и концепции роботы продают сами себя.

## Причина

Концепция current, но граница между внутренним стратегическим названием и публичной формулировкой требует owner-решения.

## Что исправить

- Решить: фраза публичная, внутренняя, или публичная только в мягкой адаптированной версии.

## Как проверить

- После решения SYNTHESIS-MARKETING не должен создавать public-facing идеи с непроверенным названием.

## Что вынести в базу после исправления

- Правило public/internal для стратегических marketing concepts.

## Условие закрытия

Закрыто 2026-06-19.

Публичный статус выбран: `internal strategic phrase`.

Дословно публично не использовать: `роботы продают сами себя`.

Мягкие публичные адаптации и границы зафиксированы в [SolidarTrading - роботы продают сами себя](../../../%D0%92%D0%B8%D0%BA%D0%B8/TiTan%20Bots%20-%20%D0%9C%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B8%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B2%D0%B8%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5/%D0%A2%D1%80%D0%B0%D1%84%D0%B8%D0%BA/SolidarTrading%20-%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D1%8B%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D1%8E%D1%82%20%D1%81%D0%B0%D0%BC%D0%B8%20%D1%81%D0%B5%D0%B1%D1%8F.md).

Lasting lesson для SYNTHESIS/METHOD зафиксирован в [Правила продающих материалов](../../../%D0%92%D0%B8%D0%BA%D0%B8/TiTan%20Bots%20-%20%D0%9C%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B8%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B2%D0%B8%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4/%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D1%8E%D1%89%D0%B8%D1%85%20%D0%BC%D0%B0%D1%82%D0%B5%D1%80%D0%B8%D0%B0%D0%BB%D0%BE%D0%B2.md): стратегические marketing concepts должны иметь public/internal статус; если статус не выбран, SYNTHESIS не должен генерировать public-facing assets с дословной формулировкой.

## Source basis

- `Вики\TiTan Bots - Метод и продвижение\Трафик\SolidarTrading - роботы продают сами себя.md`
- `Вики\TiTan Bots - Метод и продвижение\Трафик\SolidarTrading - страницы захвата как инструмент воронки.md`
- `Вики\TiTan Bots - Метод и продвижение\Трафик\SolidarTrading - AI-помощник как инструмент объяснения и конверсии.md`
