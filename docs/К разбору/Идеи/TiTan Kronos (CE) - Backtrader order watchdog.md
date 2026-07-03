---
type: idea
source: 2026-05-21 - ChatGPT - CE watchdog verify Accepted.md
applies_to:
  - TiTan Kronos (CE)
  - Python Backtrader
  - live streams
section:
  - architecture
  - execution
  - reconnect
priority_score: 4
---

## Оценка ценности

4/10.

# TiTan Kronos (CE) - Backtrader order watchdog

## Суть

Для Python/Backtrader версии [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) нужен watchdog ордера: защитный слой, который обнаруживает ситуацию, когда ордер отправлен, но статус `Accepted` не пришел.

Главная задача watchdog - не менять торговую логику CE, а защищать live-работу от зависшего состояния broker/store streams.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- Python / Backtrader
- live streams
- `CE 7 4 light.py`
- Chandelier Exit / CE Adaptive

## Источник

`2026-05-21 - ChatGPT - CE watchdog verify Accepted.md`

## Архитектурное решение

Для CE контролируется не реальная позиция, а статус ордера.

Причина:

- в CE один активный ордер;
- нет виртуальной сетки;
- нет счетчика внутренних ордеров;
- нечего синхронизировать как в grid-боте.

Поэтому `sync_with_real_position()` в этой архитектуре не нужен.

## Компоненты

### pending_order

`self.pending_order` хранит ордер, по которому еще ожидается обработка.

Если `pending_order is None`, значит ордер уже обработан или очищен.

### need_verify

`self.need_verify` означает, что бот уже вошел в режим проверки из-за отсутствия подтверждения.

Этот флаг не должен включаться сразу при отправке ордера.

### verify_due

`self.verify_due` задает время следующей проверки.

Значение всегда должно быть объектом `datetime`.

Для сброса использовать:

```python
self.verify_due = datetime.min
```

Не использовать:

```python
self.verify_due = None
```

### verify_attempt

`self.verify_attempt` считает попытки reconnect / verify.

Back-off:

```python
seconds = 30 * self.verify_attempt
```

Ожидаемые паузы:

- попытка 1 - 30 секунд;
- попытка 2 - 60 секунд;
- попытка 3 - 90 секунд.

### _restart_streams

`self._restart_streams()` перезапускает broker/store streams.

Если рабочая функция уже есть, ее не нужно переписывать при переносе watchdog.

## Логика в next

1. Если есть `pending_order` или активен `need_verify`, бот входит в блок проверки.
2. Если `need_verify` активен и `datetime.utcnow() < verify_due`, нужно сделать `return`.
3. Если `pending_order is None`, статус пришел: сбросить `need_verify` и `verify_attempt`, затем продолжить торговую логику.
4. Если `pending_order is not None`, статус не пришел: вызвать `_restart_streams()`, увеличить `verify_attempt`, выставить следующий `verify_due`.
5. После 3 неудачных попыток выполнить force clear.

## Force clear

После 3 неудачных VERIFY-попыток:

```python
self.cancel(self.pending_order)
self.pending_order  = None
self.need_verify    = False
self.verify_attempt = 0
self.verify_due     = datetime.min
```

Более осторожный вариант при возможных гонках:

```python
o = self.pending_order
self.pending_order = None
if o is not None:
    self.cancel(o)
```

## Что проверить

- Где именно `notify_order()` очищает `pending_order`.
- Становится ли `pending_order` равным `None` при статусе `Accepted`.
- Не блокирует ли `return` в VERIFY-блоке важные служебные операции CE.
- Безопасен ли `cancel(self.pending_order)`, если брокер уже очистил ордер.
- Не конфликтует ли force clear с другими ссылками на `pending_order`.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - VERIFY включать только после отсутствия Accepted](../../../TiTan%20Kronos%20(CE)%20-%20VERIFY%20%D0%B2%D0%BA%D0%BB%D1%8E%D1%87%D0%B0%D1%82%D1%8C%20%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D0%BE%D1%82%D1%81%D1%83%D1%82%D1%81%D1%82%D0%B2%D0%B8%D1%8F%20Accepted.md)
- TiTan Kronos (CE) - ошибки переноса watchdog из Signal Bot
- [TiTan Kronos (CE) - Signal Bot watchdog можно перенести без изменения торговой логики](./TiTan%20Kronos%20(CE)%20-%20Signal%20Bot%20watchdog%20%D0%BC%D0%BE%D0%B6%D0%BD%D0%BE%20%D0%BF%D0%B5%D1%80%D0%B5%D0%BD%D0%B5%D1%81%D1%82%D0%B8%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F%20%D1%82%D0%BE%D1%80%D0%B3%D0%BE%D0%B2%D0%BE%D0%B9%20%D0%BB%D0%BE%D0%B3%D0%B8%D0%BA%D0%B8.md)

## RAG/Codex guard

- Non-current: карточка имеет status: needs_test.
- Это гипотеза до code-test/backtest.
- Нельзя выдавать как проверенный технический вывод, current formula или рабочее правило без результата теста.
