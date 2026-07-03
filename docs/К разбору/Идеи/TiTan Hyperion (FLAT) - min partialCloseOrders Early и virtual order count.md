---
type: idea
value_score: 4
source: 2026-05-20 - ChatGPT - FLAT partialClosePercent dead logic.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - идея
  - partial close
  - execution
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - min partialCloseOrders Early и virtual order count

## Суть

В [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) условие раннего partial close может быть слишком жестким, если требует:

```mql5
virtual_order_count >= partialCloseOrders_Early
```

Гипотеза: вместо полного отказа от partial close можно закрывать доступное количество через `min(partialCloseOrders_Early, virtual_order_count)`.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- partial close
- execution
- риск-логика

## Раздел

Гипотеза, торговая логика, execution, риск.

## Источник

`2026-05-20 - ChatGPT - FLAT partialClosePercent dead logic.md`

## Что ожидаем

Если `virtual_order_count` меньше расчетного `partialCloseOrders_Early`, стратегия не должна полностью терять возможность частичного закрытия.

Ожидаемый вариант поведения:

```mql5
toClose = MathMin(partialCloseOrders_Early, virtual_order_count);
```

## Как проверить

- Прогнать backtest до и после изменения условия.
- Отдельно логировать `virtual_order_count`, `partialCloseOrders_Early`, фактический `toClose`.
- Проверить сценарии, где виртуальных ордеров меньше расчетного partial close.
- Сравнить влияние на прибыль, просадку, частоту защитных выходов.

## Статус

Новая гипотеза. Требует теста.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- TiTan Hyperion (FLAT) - partialClosePercent был мертвым параметром
- [TiTan Hyperion (FLAT) - параметры надо проверять через исполняемые ветки](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B%20%D0%BD%D0%B0%D0%B4%D0%BE%20%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D1%8F%D1%82%D1%8C%20%D1%87%D0%B5%D1%80%D0%B5%D0%B7%20%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D0%B5%20%D0%B2%D0%B5%D1%82%D0%BA%D0%B8.md)


## Что проверить

needs_review
