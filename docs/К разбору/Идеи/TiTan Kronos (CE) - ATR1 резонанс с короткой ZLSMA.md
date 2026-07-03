---
type: idea
value_score: 4
source: 2026-05-20 - ChatGPT - CE ZLSMA architecture and findings.md
applies_to:
  - TiTan Kronos (CE)
  - MT5
section:
  - idea
  - ATR
  - ZLSMA
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Kronos (CE) - ATR1 резонанс с короткой ZLSMA

## Суть

В [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) прибыльность в зоне `ZLSMA 5-7` может быть резонансом между `ATR(1)` и короткой ZLSMA, а не устойчивой закономерностью.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- MT5 / MQL5
- CE + ZLSMA
- M1 / XAUUSD

## Раздел

Гипотеза, ATR, ZLSMA, optimization, robustness.

## Источник

`2026-05-20 - ChatGPT - CE ZLSMA architecture and findings.md`

## Что ожидаем

Если это резонанс, то:

- прибыль будет узкой по параметрам;
- соседние ZLSMA lengths быстро ухудшатся;
- результат будет чувствителен к spread;
- переход на M5/M15 изменит или разрушит эффект.

## Как проверить

Построить heatmap:

```text
ATR length: 1..5
ZLSMA length: широкий диапазон вокруг 5-7
```

Дополнительно сравнить:

- месяцы;
- M1/M5/M15;
- fixed ZLSMA vs adaptive ZLSMA;
- profit per trade;
- total trade count.

## Статус

`needs_test`.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- TiTan Kronos (CE) - узкое плато ZLSMA 5-7 опасный симптом
- [TiTan Kronos (CE) - CE как directional volatility filter](../../../TiTan%20Kronos%20(CE)%20-%20CE%20%D0%BA%D0%B0%D0%BA%20directional%20volatility%20filter.md)


## Что проверить

needs_review
