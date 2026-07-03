---
type: error
source: 2026-05-21 - ChatGPT - ATLAS live verify и ConnectionSupervisor.md
created: 2026-05-21
applies_to:
  - TiTan Atlas (BA)
  - Crypto Python
sections:
  - live
  - verify
  - recovery
priority_score: 8
owner_layer: TECH
terminal_owner: owner
closure_condition: owner/code review confirms suspicious verify invalidates, rebuilds, or fail-stops old buy_levels; otherwise code fix required
---


## Оценка опасности

8/10 - live recovery risk


# ATLAS live - старому buy_levels нельзя доверять после suspicious verify

## Симптом

После suspicious verify локальный хвост `buy_levels` может остаться старым, хотя last-state уже был сомнительным или синхронизировался заново.

## Применимо к

- [TiTan Atlas (BA)](../../../TiTan%20Atlas%20(BA).md)
- [TiTan Atlas (BA) - Crypto Python](../../../TiTan%20Atlas%20(BA)%20-%20Crypto%20Python.md)
- live recovery

## Раздел

- live verify;
- recovery;
- local state.

## Источник

`2026-05-21 - ChatGPT - ATLAS live verify и ConnectionSupervisor.md`

## Причина

`buy_levels` - вторичная техническая структура. Она не должна переживать подозрительный live-сценарий как будто ничего не произошло.

Особенно опасные причины:

- `R6_LAST_STATE_UNCONFIRMED`;
- `R7_LAST_STATE_MISMATCH`;
- `T2_ORDER_STATUS_SYNC`;
- `T3_LAST_STATE_MATCH_SYNC` после синхронизации.

## Решение

Выбранная модель: гибридная.

- обычный startup/recover может использовать мягкую sanity-проверку;
- после suspicious verify старому `buy_levels` не доверять;
- делать rebuild / full sync от подтвержденного якоря.

## Решение владельца

Решение от 2026-06-19: ошибка реальная и важная, но отложена владельцем до возврата Crypto Python / crypto live bot в активную разработку.

`deferred_by_owner: crypto bot not in active development`

Причина: Crypto Python / crypto live bot сейчас не находится в активной разработке. Код сейчас не чинить, ошибку не считать fixed и не считать false-positive.

Карточка остается в активной ручной очереди `К разбору/Ошибки`, потому что перед любым возвратом к crypto live нужно явно подтвердить, что отложенность все еще допустима и риск не стал критичным для текущего плана.

Lasting lesson уже вынесен в правило [ATLAS live - last order якорь сетки](../../../ATLAS%20live%20-%20last%20order%20%D1%8F%D0%BA%D0%BE%D1%80%D1%8C%20%D1%81%D0%B5%D1%82%D0%BA%D0%B8.md), но это не закрывает code/live риск. Условие закрытия остается прежним: owner/code review подтверждает, что suspicious verify инвалидирует старый `buy_levels`, выполняет rebuild от подтвержденного якоря или переводит систему в fail-stop/degraded state.

Аудитные слои не должны использовать эту карточку как вход для автоматической переработки базы. Это ручная ошибка, а не audit-source.

## Связи

- [ATLAS crypto - ConnectionSupervisor и live verify](../../../ATLAS%20crypto%20-%20ConnectionSupervisor%20%D0%B8%20live%20verify.md)
- [ATLAS live - last order якорь сетки](../../../ATLAS%20live%20-%20last%20order%20%D1%8F%D0%BA%D0%BE%D1%80%D1%8C%20%D1%81%D0%B5%D1%82%D0%BA%D0%B8.md)
- [ATLAS live - состояние сравнивать по side depth](../../../ATLAS%20live%20-%20%D1%81%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5%20%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B8%D0%B2%D0%B0%D1%82%D1%8C%20%D0%BF%D0%BE%20side%20depth.md)
