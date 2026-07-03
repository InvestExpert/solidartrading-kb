---
type: idea
value_score: 5
source: 2026-05-20 - ChatGPT - TiTan KRONOS CE stop architecture.md
created: 2026-05-20
bot: TiTan Kronos (CE)
applies_to:
  - TiTan Kronos (CE)
sections:
  - exit_logic
  - decision_gate
  - mt5
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Kronos (CE) - гипотезы CE Stop и Flip

Гипотезы по доминированию CE Stop и исчезновению CE Flip в TiTan Kronos (CE).

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- MT5 / MQL5

## Раздел

- exit logic;
- Decision Gate;
- MT5;
- CE Stop / CE Flip.

## Источник

`2026-05-20 - ChatGPT - TiTan KRONOS CE stop architecture.md`

## Гипотеза 1: CE-hit по High/Low убивает Flip

Суть:

- CE-stop проверяется по `High/Low[1]`;
- Flip проверяется по `Close[1]`;
- CE ловит хвост свечи раньше, чем close формирует смену направления CE.

Как проверить:

- сравнить режим `High/Low CE` против `Close-only CE`;
- включить `closeByConflict`;
- проверить долю `CE Flip`.

Статус:

- частично подтверждена логами: `CE Stop = 93.96%`, `CE Flip = 0%`.

## Гипотеза 2: новая логика стала слишком ранней по выходам

Суть:

- исправленная версия логически чище;
- но закрывает сделки раньше;
- поэтому прибыль могла упасть со старых около `1213$` до около `500$`.

Как проверить:

- сравнить среднюю длительность сделки старой и новой версии;
- сравнить распределение причин закрытия;
- сравнить PnL при одинаковых входах.

Статус:

- частично подтверждена наблюдениями пользователя.

## Гипотеза 3: CE offset ухудшает результат

Суть:

- дополнительный offset в CE-логике не улучшает фильтрацию;
- CE и так слишком часто перехватывает сделки.

Как проверить:

- прогнать `stopCeOffsetPercent` по сетке `0.00`, `0.02`, `0.05`, `0.10`;
- сравнить PnL, CE%, Flip%, среднюю длительность сделки.

Статус:

- частично подтверждена наблюдением пользователя, что offset на CE ухудшает результат.

## Гипотеза 4: Trail offset не влияет, потому что Trail не участвует

Суть:

- изменение `stopOffsetPercent` почти не влияет на результат, если `Trail Stop = 0%`.

Как проверить:

- сначала добиться ненулевого `Trail Stop`;
- затем тестировать `stopOffsetPercent`.

Статус:

- подтверждена текущими логами из источника: `Trail Stop = 0%`.

## Гипотеза 5: режимы 0 и 1 одинаковы из-за отсутствия flip_on

Суть:

- switch может работать;
- но режимы 0 и 1 совпадают, если `flip_on` почти не становится true или не доходит до исполнения.

Как проверить:

- логировать `stopMode`, `ce_hit`, `flip_on`;
- сравнить количество `flip_on=true` с количеством `CE FLIP`.

Статус:

- частично подтверждена.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - Decision Gate](../../../TiTan%20Kronos%20(CE)%20-%20Decision%20Gate.md)
- [TiTan Kronos (CE) - Правила CE Stop](../../../TiTan%20Kronos%20(CE)%20-%20%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%20CE%20Stop.md)
- [TiTan Kronos (CE) стал single-stop системой](../../../TiTan%20Kronos%20(CE)%20%D1%81%D1%82%D0%B0%D0%BB%20single-stop%20%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%BE%D0%B9.md)


## Что проверить

needs_review
