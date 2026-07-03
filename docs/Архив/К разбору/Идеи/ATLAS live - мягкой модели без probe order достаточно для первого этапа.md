---
type: idea
priority_score: 1
source: 2026-05-21 - ChatGPT - ATLAS live verify и ConnectionSupervisor.md; owner decision 2026-06-21
created: 2026-05-21
applies_to:
  - TiTan Atlas (BA)
  - Crypto Python
sections:
  - live
  - verify
  - recovery
  - resolved
  - archive_processed
---


## Оценка ценности

1/10.

Статус: `archive_processed / resolved`.

Карточка закрыта: первый live-этап ATLAS уже оформлен как мягкая модель без базового `probe order`. `Probe order` сохранен не как активная задача, а как резервный fallback при реальных скрытых или частых разрывах связи API.

## Решение после проверки

Смысл перенесен в активный owner-контекст:

- [ATLAS crypto - ConnectionSupervisor и live verify](../../../../ATLAS%20crypto%20-%20ConnectionSupervisor%20%D0%B8%20live%20verify.md) - главное правило первого live-этапа: `ConnectionSupervisor`, `LAST_DEAL_BT`, verify / reconcile / recovery, `side + depth`, без базового `probe order`.
- [ATLAS live - состояние сравнивать по side depth](../../../../ATLAS%20live%20-%20%D1%81%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5%20%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B8%D0%B2%D0%B0%D1%82%D1%8C%20%D0%BF%D0%BE%20side%20depth.md) - короткая граница: `probe order` не нужен на первом этапе, но остается fallback при тяжелых разрывах связи.
- [TiTan Hyperion (FLAT) - Правила восстановления связи TradeLocker](../../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%20%D0%B2%D0%BE%D1%81%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F%20%D1%81%D0%B2%D1%8F%D0%B7%D0%B8%20TradeLocker.md) - источник, где `probe order` остается рабочим проверенным механизмом для TradeLocker / FLAT.

Новую задачу на разработку не создавать, пока нет реальных ATLAS live-инцидентов такого же класса.


# ATLAS live - мягкой модели без probe order достаточно для первого этапа

## Гипотеза

Для первого live-этапа [TiTan Atlas (BA)](../../../../TiTan%20Atlas%20(BA).md) достаточно модели:

- `ConnectionSupervisor`;
- `LAST_DEAL_BT`;
- verify / reconcile / recovery;
- сравнение по `side + depth`;
- rebuild / sync после suspicious verify.

Полная FLAT probe-схема пока не нужна.

## Применимо к

- [TiTan Atlas (BA)](../../../../TiTan%20Atlas%20(BA).md)
- [TiTan Atlas (BA) - Crypto Python](../../../../TiTan%20Atlas%20(BA)%20-%20Crypto%20Python.md)
- live testing

## Раздел

- live verify;
- recovery;
- production hardening.

## Источник

`2026-05-21 - ChatGPT - ATLAS live verify и ConnectionSupervisor.md`

## Как проверить

- запустить первые live-наблюдения;
- собрать ambiguous execution кейсы;
- проверить рассинхрон last-state;
- смотреть, хватает ли `side + depth`;
- фиксировать, нужны ли stale-feed detector, action model и probe-order.

## Что будет следующим уровнем

Если мягкой модели не хватит, следующий шаг - перенос более жесткой FLAT-подобной probe-схемы и расширенного incident logging.

## Связи

- [ATLAS crypto - ConnectionSupervisor и live verify](../../../../ATLAS%20crypto%20-%20ConnectionSupervisor%20%D0%B8%20live%20verify.md)
- [ATLAS live - verify не вызывать каждый тик](../../../../ATLAS%20live%20-%20verify%20%D0%BD%D0%B5%20%D0%B2%D1%8B%D0%B7%D1%8B%D0%B2%D0%B0%D1%82%D1%8C%20%D0%BA%D0%B0%D0%B6%D0%B4%D1%8B%D0%B9%20%D1%82%D0%B8%D0%BA.md)
- [TiTan Hyperion (FLAT)](../../../../TiTan%20Hyperion%20(FLAT).md)



## Что проверить

needs_review
