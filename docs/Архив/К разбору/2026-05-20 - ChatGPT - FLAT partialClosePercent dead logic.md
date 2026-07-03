---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: анализ partialClosePercent и partial close логики FLAT
related_bot: FLAT
related_strategy: FLAT / MT5 / MQL5
suggested_wiki_targets:
  - Вики/Ошибки
  - Вики/Выводы
  - Вики/Гипотезы
  - Вики/Архитектура
  - Вики/Правила
  - Вики/Боты
created_for: TiTan Bots
---

# Диагностика partialClosePercent в FLAT

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: неизвестно  
Связанный бот: FLAT  
Связанная стратегия или платформа: FLAT / MT5 / MQL5  

Что сохранено:
- анализ partial close логики;
- диагностика параметра `partialClosePercent`;
- поиск причины одинаковых результатов оптимизации;
- анализ `StopAndReset_B/S`;
- анализ `TryPartial`;
- анализ `totalAvailable`;
- обнаружение "мертвого" параметра;
- логические выводы по нормализации `lowerOrders`.

Что пропущено:
- эмоциональные реплики;
- не технические обсуждения;
- личные и бытовые фрагменты.

Риски приватности:
- низкие;
- в чате отсутствуют чувствительные персональные данные.

Предложенное имя файла:
2026-05-20 - ChatGPT - FLAT partialClosePercent dead logic.md

## Obsidian links

- [FLAT](FLAT.md)
- [MT5](MT5.md)
- [MQL5](MQL5.md)
- [Partial Close](Partial%20Close.md)
- [Grid Strategy](Grid%20Strategy.md)
- [Backtest Debug](Backtest%20Debug.md)
- [Dead Parameters](Dead%20Parameters.md)

## Краткое резюме

В чате проводилась диагностика параметра `partialClosePercent` в FLAT-боте.  
Оптимизация показывала полностью одинаковые результаты при разных значениях параметра.

В ходе анализа выяснилось, что проблема не в самом параметре `partialClosePercent`, а в вычислении:

```mql5
totalAvailable = upperOrders - lowerOrders - reserveOrders;
```

и в ограничении:

```mql5
lowerOrders = MathMin(lowerOrdersInput, upperOrders - reserveOrders);
```

При некоторых конфигурациях это приводило к:

```text
available = 0
```

Из-за этого:

- `partialCloseOrders_Early = 0`
- `partialCloseOrders_Stop = 0`

и вся логика partial close полностью отключалась.

## Идеи

- Проверять "живость" параметров стратегии через логи и реальные ветки исполнения.
- Добавлять диагностические логи для вычисляемых параметров (`available`, `Early`, `Stop`).
- Исключать из оптимизации параметры, которые могут математически отключаться.

## Гипотезы

### Гипотеза 1

Параметр `partialClosePercent` не влиял на результат, потому что:

```mql5
available = 0
```

и код partial close никогда не исполнялся.

Статус: подтверждено логами.

---

### Гипотеза 2

Условие:

```mql5
virtual_order_count >= partialCloseOrders_Early
```

может быть слишком жёстким.

Обсуждалась замена на:

```mql5
min(partialCloseOrders_Early, virtual_order_count)
```

чтобы закрывать хотя бы доступное количество ордеров.

Статус: требует дополнительной проверки.

## Выводы

### Вывод 1

Параметр может выглядеть "нерабочим", хотя проблема находится в предварительной нормализации параметров.

---

### Вывод 2

Оптимизация параметров без проверки промежуточных вычислений может давать ложные выводы.

---

### Вывод 3

Необходимо логировать вычисляемые внутренние параметры стратегии, особенно если они участвуют в условных ветках исполнения.

## Ошибки и решения

### Ошибка

Ограничение:

```mql5
lowerOrders = MathMin(lowerOrdersInput, upperOrders - reserveOrders);
```

приводило к ситуации:

```text
upperOrders - lowerOrders - reserveOrders = 0
```

что полностью отключало partial close.

---

### Решение

Обсуждалась замена на:

```mql5
lowerOrders = MathMin(lowerOrdersInput, upperOrders);
```

с отдельным вычислением:

```mql5
available = upperOrders - lowerOrders - reserveOrders;
if(available < 0)
    available = 0;
```

## Правила

- Перед оптимизацией параметров проверять, что они реально участвуют в исполняемых ветках.
- Логировать вычисляемые значения внутренних коэффициентов и лимитов.
- Проверять параметры на возможность математического отключения.
- Не делать вывод о "нерабочем" параметре без анализа логов исполнения.

## Архитектура

### Связанные элементы логики

- `partialClosePercent`
- `partialCloseOrders_Early`
- `partialCloseOrders_Stop`
- `TryPartial`
- `StopAndReset_B`
- `StopAndReset_S`
- `totalAvailable`

### Связанный поток логики

```text
upperOrders
 -> lowerOrders
 -> reserveOrders
 -> totalAvailable
 -> partialCloseOrders_Early / Stop
 -> partial close execution
```

## Боты и стратегии

### FLAT

Диагностика partial close механизма в MT5-версии FLAT.

Используемые механизмы:
- сеточная логика;
- частичные закрытия;
- Early/Stop распределение;
- forced reset;
- partial profit levels.

## Что проверить дальше

- Проверить влияние новой формулы `lowerOrders`.
- Проверить работу `partialClosePercent` после устранения `available=0`.
- Проверить гипотезу с `min(partialCloseOrders_Early, virtual_order_count)`.
- Добавить диагностические логи для partial close веток.
- Проверить, насколько часто реально исполняются `TryPartial` и `StopAndReset_*`.

## Что НЕ переносить в wiki

- эмоциональные реплики;
- не технические комментарии;
- спорные оценочные фразы;
- бытовые и личные сообщения.

## Цитаты или фрагменты

### Диагностический лог

```text
Params: step=0.09%, upper=40, available=0 (Early=0/0% + Stop=0), lower=33, rest=7
```

### Формула, вызвавшая проблему

```mql5
lowerOrders = MathMin(lowerOrdersInput, upperOrders - reserveOrders);
```

### Partial close вычисления

```mql5
totalAvailable = upperOrders - lowerOrders - reserveOrders;
```
