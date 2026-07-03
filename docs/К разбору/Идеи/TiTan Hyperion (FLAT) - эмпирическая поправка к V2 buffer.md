---
type: idea
value_score: 4
source: 2026-05-20 - ChatGPT - FLAT V2 buffer formula and protection logic.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - idea
  - buffer formula
  - execution costs
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - эмпирическая поправка к V2 buffer

## Суть

Теоретический V2-buffer для [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) может требовать эмпирической поправки под реальные торговые издержки.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- buffer formula
- execution costs
- V2/V3/V4

## Раздел

Гипотеза, риск-менеджмент, execution, тестирование.

## Источник

`2026-05-20 - ChatGPT - FLAT V2 buffer formula and protection logic.md`

## Что ожидаем

Реальная точка безубытка выше теоретической из-за:

- spread;
- commission;
- slippage;
- rounding;
- broker execution.

В источнике предложен ориентир поправки:

```text
Delta k примерно 0.2-0.25 шага
```

## Как проверить

- Проверить V2-buffer на разных инструментах: XAU, Forex, Crypto.
- Сравнить теоретический buffer и фактический ExitPNL.
- Отдельно смотреть spread spikes и Digits/Point rounding.
- Проверить влияние trailing speed и `partialClosePercent > 0`.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - теоретический buffer не равен production buffer](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D1%82%D0%B5%D0%BE%D1%80%D0%B5%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9%20buffer%20%D0%BD%D0%B5%20%D1%80%D0%B0%D0%B2%D0%B5%D0%BD%20production%20buffer.md)
- [TiTan Hyperion (FLAT) - V2 buffer считается без reserveOrders](../../../TiTan%20Hyperion%20(FLAT)%20-%20V2%20buffer%20%D1%81%D1%87%D0%B8%D1%82%D0%B0%D0%B5%D1%82%D1%81%D1%8F%20%D0%B1%D0%B5%D0%B7%20reserveOrders.md)


## Что проверить

needs_review
