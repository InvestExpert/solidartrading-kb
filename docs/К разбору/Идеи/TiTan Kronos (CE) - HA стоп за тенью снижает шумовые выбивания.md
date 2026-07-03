---
type: idea
value_score: 5
source: 2026-05-20 - ChatGPT - CE HA stop logic and M1 behavior.md
applies_to:
  - TiTan Kronos (CE)
  - MT5
section:
  - idea
  - Heikin Ashi
  - CE Stop
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Kronos (CE) - HA стоп за тенью снижает шумовые выбивания

## Суть

В [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) на M1 stop-level внутри HA-свечи может приводить к шумовым выбиваниям.

Гипотеза: перенос CE stop за HA-тень или hybrid stop увеличит удержание позиции и снизит ложные выходы.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- MT5 / MQL5
- CE + ZLSMA
- Heikin Ashi
- M1 / low timeframe

## Раздел

Гипотеза, HA, CE Stop, M1 behavior.

## Источник

`2026-05-20 - ChatGPT - CE HA stop logic and M1 behavior.md`

## Проблема

Классическая формула:

```text
longStopRaw  = priceHighRef - ATR
shortStopRaw = priceLowRef + ATR
```

При HA High/Low может ставить stop внутри HA-свечи или слишком близко к шуму.

## Кандидаты

HA stop:

```text
long stop  = HA low - ATR
short stop = HA high + ATR
```

Hybrid stop:

```text
long stop  = min(realLow, haLow) - ATR
short stop = max(realHigh, haHigh) + ATR
```

## Что ожидаем

- Меньше шумовых stop-out.
- Больше удержание позиции.
- Выше средний profit/trade.
- Возможное снижение trade frequency.

## Как проверить

Сравнить:

- old stop logic;
- HA stop logic;
- hybrid stop logic.

Метрики:

- количество стопов;
- средняя длина удержания;
- winrate;
- profit factor;
- DD;
- trade count.

## Статус

`needs_test`.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - HA Fix v10](../../../TiTan%20Kronos%20(CE)%20-%20HA%20Fix%20v10.md)
- [TiTan Kronos (CE) - HA расширяет CE stop distance](../../../TiTan%20Kronos%20(CE)%20-%20HA%20%D1%80%D0%B0%D1%81%D1%88%D0%B8%D1%80%D1%8F%D0%B5%D1%82%20CE%20stop%20distance.md)
- [TiTan Kronos (CE) - CE offset как anti-noise layer](./TiTan%20Kronos%20(CE)%20-%20CE%20offset%20%D0%BA%D0%B0%D0%BA%20anti-noise%20layer.md)
- [TiTan Kronos (CE) - ATR1 допустим только при контроле шума](../../../TiTan%20Kronos%20(CE)%20-%20ATR1%20%D0%B4%D0%BE%D0%BF%D1%83%D1%81%D1%82%D0%B8%D0%BC%20%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE%20%D0%BF%D1%80%D0%B8%20%D0%BA%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D0%B5%20%D1%88%D1%83%D0%BC%D0%B0.md)


## Что проверить

needs_review
