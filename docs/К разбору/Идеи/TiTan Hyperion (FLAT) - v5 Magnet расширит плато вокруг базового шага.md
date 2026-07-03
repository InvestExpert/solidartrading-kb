---
type: idea
value_score: 6
source: 2026-05-20 - ChatGPT - FLAT adaptive grid step v5 Magnet.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - idea
  - adaptive step
  - SmartMode
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - v5 Magnet расширит плато вокруг базового шага

## Суть

Гипотеза: формула [TiTan Hyperion (FLAT) - adaptive step v5 Magnet](./TiTan%20Hyperion%20(FLAT)%20-%20adaptive%20step%20v5%20Magnet.md) расширит прибыльное плато вокруг базового шага, например `0.27`.

Формула должна не агрессивно сдвигать шаг, а удерживать его рядом с рабочей зоной.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- SmartMode
- adaptive grid step
- AbsVol

## Раздел

Гипотеза, торговая логика, оптимизация, риск.

## Источник

`2026-05-20 - ChatGPT - FLAT adaptive grid step v5 Magnet.md`

## Что ожидаем

Ожидаемые эффекты:

- меньше разлет шагов между месяцами;
- меньше рваность результатов;
- лучше устойчивость в зоне около базового шага;
- возможное снижение MaxDD;
- меньше плохих выходных веток V1/V2.

## Как проверить

- Прогнать шаги вокруг базы, например `0.25-0.31`.
- Сравнить v5 с v1-v4, не смешивая результаты.
- Проверить `SmartPivotValue = 1.20`.
- Проверить `SmartAbsVolCoef = 0.02` и `0.03`.
- Для каждого теста смотреть PnL, MaxDD, V1-V6 и ширину плато.

## Статус

Новая гипотеза. Требует backtest.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - adaptive step v5 Magnet](./TiTan%20Hyperion%20(FLAT)%20-%20adaptive%20step%20v5%20Magnet.md)
- [TiTan Hyperion (FLAT) - плато важнее пика PnL](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%BF%D0%BB%D0%B0%D1%82%D0%BE%20%D0%B2%D0%B0%D0%B6%D0%BD%D0%B5%D0%B5%20%D0%BF%D0%B8%D0%BA%D0%B0%20PnL.md)
- [TiTan Hyperion (FLAT) - ADX как второй фильтр режима рынка](./TiTan%20Hyperion%20(FLAT)%20-%20ADX%20%D0%BA%D0%B0%D0%BA%20%D0%B2%D1%82%D0%BE%D1%80%D0%BE%D0%B9%20%D1%84%D0%B8%D0%BB%D1%8C%D1%82%D1%80%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%D0%B0%20%D1%80%D1%8B%D0%BD%D0%BA%D0%B0.md)


## Что проверить

needs_review
