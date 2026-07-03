---
type: idea
value_score: 6
source: 2026-05-21 - ChatGPT - CE watchdog verify Accepted.md
applies_to:
  - TiTan Kronos (CE)
  - Python Backtrader
section:
  - execution
  - reconnect
  - live testing
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Kronos (CE) - Signal Bot watchdog можно перенести без изменения торговой логики

## Суть

Watchdog-защиту из Signal Bot можно добавить в Python/Backtrader версию [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) как внешний защитный слой, не меняя торговую логику Chandelier Exit.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- `CE 7 4 light.py`
- Python / Backtrader
- live streams

## Источник

`2026-05-21 - ChatGPT - CE watchdog verify Accepted.md`

## Что ожидаем

Если статус `Accepted` не пришел:

- watchdog обнаружит зависшее состояние;
- `_restart_streams()` будет вызван;
- `verify_attempt` увеличится;
- `verify_due` выставит паузу 30 / 60 / 90 секунд;
- после 3 неудач произойдет force clear.

При этом торговые условия CE не должны измениться.

## Как проверить

1. Запустить live или simulated-live сценарий.
2. Имитировать отсутствие статуса `Accepted`.
3. Проверить вызов `_restart_streams()`.
4. Проверить back-off 30 / 60 / 90 секунд.
5. Проверить, что после восстановления статуса `pending_order` очищается.
6. Проверить, что после `VERIFY OK` стратегия продолжает обычную торговую логику.
7. Проверить, что входы, выходы, CE stop и CE flip не изменились из-за watchdog.

## Риск

Если `return` в VERIFY-блоке стоит слишком рано, он может пропустить важные служебные операции CE.

Если `cancel(self.pending_order)` вызывается после того, как брокер уже очистил ордер, возможна гонка состояния.

## Статус

Нужен тест на коде и live/simulated-live сценарии.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - Backtrader order watchdog](./TiTan%20Kronos%20(CE)%20-%20Backtrader%20order%20watchdog.md)
- [TiTan Kronos (CE) - VERIFY включать только после отсутствия Accepted](../../../TiTan%20Kronos%20(CE)%20-%20VERIFY%20%D0%B2%D0%BA%D0%BB%D1%8E%D1%87%D0%B0%D1%82%D1%8C%20%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D0%BE%D1%82%D1%81%D1%83%D1%82%D1%81%D1%82%D0%B2%D0%B8%D1%8F%20Accepted.md)


## Что проверить

needs_review
