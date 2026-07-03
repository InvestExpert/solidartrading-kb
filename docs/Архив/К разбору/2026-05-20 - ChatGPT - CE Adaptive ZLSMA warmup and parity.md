---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: CE Adaptive ZLSMA / KRONOS
related_bot: TiTan KRONOS
related_strategy: CE / ZLSMA / MT5 / Pine
suggested_wiki_targets:
  - Вики/Идеи
  - Вики/Гипотезы
  - Вики/Выводы
  - Вики/Ошибки
  - Вики/Правила
  - Вики/Архитектура
  - Вики/Боты
created_for: TiTan Bots
---

# CE Adaptive ZLSMA warmup and parity

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: CE Adaptive ZLSMA / KRONOS  
Связанный бот: TiTan KRONOS  
Связанная стратегия или платформа: CE / ZLSMA / MT5 / Pine  

Что сохранено:
- архитектура CE + ZLSMA;
- adaptive ZLSMA;
- warmup / прогрев;
- parity MT5 ↔ Pine;
- shift 0 vs shift 1;
- HA логика;
- Decision Gate;
- stopMode / EntryMode;
- persistent signal;
- bar-based execution;
- причины расхождений результатов;
- структура CE v9.4.

Что пропущено:
- бытовые и личные темы;
- эмоциональные реплики;
- интерфейсные обсуждения;
- всё вне разработки ботов.

Риски приватности:
- низкие, но нужна ручная проверка комментариев и авторских блоков.

Предложенное имя файла:
2026-05-20 - ChatGPT - CE Adaptive ZLSMA warmup and parity.md

---

## Obsidian links

- [TiTan KRONOS](TiTan%20KRONOS.md)
- [CE + ZLSMA](CE%20+%20ZLSMA.md)
- [Adaptive ZLSMA](Adaptive%20ZLSMA.md)
- [MT5 vs Pine parity](MT5%20vs%20Pine%20parity.md)
- [Heikin Ashi architecture](Heikin%20Ashi%20architecture.md)
- [Decision Gate](Decision%20Gate.md)
- [Bar-based execution](Bar-based%20execution.md)

---

## Краткое резюме

В чате разбиралась проблема расхождения adaptive ZLSMA между Pine и MT5.

Главный вывод:
проблема была не в формуле adaptiveFactor, а в различии:
- прогрева;
- ATR shift;
- жизненного цикла adaptive/non-adaptive;
- HA источников цены;
- порядка барных расчетов.

После введения общего warmup и унификации ATR shift adaptive режим при speed=0 начал практически совпадать с выключенной адаптивностью.

Также в чате была подробно зафиксирована архитектура KRONOS CE v9.4:
- bar execution;
- Decision Gate;
- разделение signal/ce/stop bar;
- stop system;
- persistent entry;
- CE flip;
- HA synchronization.

---

# Точные факты

## Pine базовая конфигурация

- strategy:
  - process_orders_on_close=true
  - calc_on_every_tick=false
  - pyramiding=1

## CE параметры

- atrLength = 1
- atrMult = 2.0

Указано как обязательная конфигурация CE.

## Adaptive ZLSMA

Базовые параметры:
- baseZlsmaLength = 50 (Pine)
- zlsmaLength = 61 (MT5 KRONOS)
- speed range = -20..20

Формула:
```text
deltaATRpct = atrFast / atrSlow - 1
adaptiveFactor = 1 + speed * deltaATRpct * 0.1
```

Clamp:
```text
adaptiveFactor ∈ [0.3 ; 5.0]
```

targetLen:
```text
targetLen = baseLength * adaptiveFactor
```

adaptive ATR:
- fast ATR = baseLen / 5
- slow ATR = baseLen

## Главный вывод по adaptive

speed=0 должен давать тот же результат, что и adaptive OFF.

Изначально это нарушалось.

Причины:
- разный warmup;
- ATR adaptive читался с текущего бара;
- разная инициализация веток.

После исправлений:
- OFF ≈ speed=0.

## Warmup

Принято решение:
- warmup должен быть ОБЩИМ для adaptive и non-adaptive.
- warmup зависит от zlsmaLength.
- фиксированный warmup 50 признан неправильным.

Правильный подход:
```text
warmup = zlsmaLength
```

Логика:
- пока данных меньше zlsmaLength:
  - adaptive не включается;
  - используется обычная ZLSMA.

## ATR shift

Найдена ошибка:

Было:
```text
CopyBuffer(... shift=0 ...)
```

Стало:
```text
CopyBuffer(... shift=1 ...)
```

Причина:
- adaptive ATR использовал текущий формирующийся бар;
- CE использовал закрытый бар;
- возникала несовместимость Pine ↔ MT5.

Также добавлен:
```text
IndicatorRelease(handle)
```

для ATR handles.

## Индексация баров

Зафиксировано разделение:

### signal-bar
```text
barIdxSignal = 0
```

Используется:
- signal price;
- ZLSMA compare;
- entry compare.

### ce-bar
```text
barIdxCE = 1
```

Используется:
- CE direction;
- HA high/low.

### stop-bar
```text
barIdxStop = 1
```

Используется:
- ATR;
- stop calculations;
- CE stop.

---

# Справочники и соответствия

## stopMode

| stopMode | Режим |
|---|---|
| 0 | CE_THEN_FLIP |
| 1 | FLIP_ONLY |
| 2 | TRAIL |
| 3 | ZLSMA_TRAIL |

## EntryMode

| EntryMode | Режим |
|---|---|
| 0 | Legacy |
| 1 | Flip only |
| 2 | Persistent only |
| 3 | Hybrid |

## Adaptive ZLSMA

| Элемент | Значение |
|---|---|
| ATR fast | baseLen / 5 |
| ATR slow | baseLen |
| Clamp min | 0.3 |
| Clamp max | 5.0 |
| Speed range | -20..20 |

---

# Идеи

## Общий warmup

Идея:
- adaptive и non-adaptive должны проходить одинаковый стартовый режим.

Статус:
- реализовано.

## Native MA режим

Предложена идея:
- добавить флаг переключения:
  - custom ZLSMA;
  - native MA.

Цель:
- проверить, даёт ли самописная линрег реальное преимущество.

Статус:
- не реализовано.

## Unified parity mode

Идея:
- полностью синхронизировать MT5 с Pine через:
  - bar execution;
  - одинаковые shift;
  - одинаковые HA;
  - одинаковые stop rules.

Статус:
- частично реализовано.

---

# Гипотезы

## Причина расхождения OFF vs speed=0

Суть:
- проблема вызвана разным прогревом adaptive/non-adaptive.

Проверка:
- сделать общий warmup.

Статус:
- подтверждена.

## Причина расхождения MT5 ↔ Pine

Суть:
- расхождения вызываются:
  - ATR shift;
  - HA источником;
  - stop execution;
  - different lifecycle.

Проверка:
- выровнять bar execution.

Статус:
- частично подтверждена.

## Adaptive formula не виновата

Суть:
- проблема не в adaptiveFactor.

Проверка:
- отключение adaptive logic.

Статус:
- подтверждена.

---

# Выводы

## Главный технический вывод

Различие результатов adaptive OFF и speed=0 вызывалось:
- разным warmup;
- разными ATR bars;
- разной инициализацией.

## Warmup должен быть единым

Нельзя:
- adaptive прогревать отдельно;
- non-adaptive отдельно.

Нужен:
- единый стартовый режим.

## ATR adaptive должен использовать закрытый бар

Использование:
```text
shift=0
```

признано ошибкой.

## CE должен быть bar-based

Для parity с Pine:
- CE;
- ATR;
- direction;
- stop logic

должны работать только по закрытому бару.

## Decision Gate должен быть ДО пересчета

Иначе:
- используются уже обновленные стопы;
- возникает смещение логики.

---

# Ошибки и решения

## Ошибка: adaptive ATR использует текущий бар

Симптом:
- speed=0 ≠ adaptive OFF.

Причина:
- ATR adaptive читал shift=0.

Решение:
```text
CopyBuffer(... shift=1 ...)
```

---

## Ошибка: разные warmup веток

Симптом:
- adaptive и OFF расходились даже при speed=0.

Причина:
- adaptive/non-adaptive имели разную инициализацию.

Решение:
- общий warmup;
- adaptive disabled during warmup.

---

## Ошибка: утечка indicator handles

Причина:
- ATR handles создавались постоянно.

Решение:
```text
IndicatorRelease(handle)
```

---

## Ошибка: смешение HA bar0 и HA bar1

Симптом:
- нестабильный parity.

Причина:
- разные HA источники.

Решение:
- online HA(0) только для signal compare;
- HA(1) для CE/stop.

---

# Правила

## Rule: speed=0

speed=0 обязан быть эквивалентом adaptive OFF.

## Rule: adaptive warmup

Adaptive ZLSMA не должна активироваться до завершения warmup.

## Rule: ATR parity

Adaptive ATR должен использовать закрытый бар.

## Rule: CE parity

CE direction и CE stop должны использовать только bar[1].

## Rule: Decision Gate order

Decision Gate выполняется до пересчета новых stop values.

## Rule: signal/ce/stop separation

Signal logic, CE logic и stop logic используют разные bar groups.

---

# Архитектура

## Decision Gate

Decision Gate расположен:
- ДО пересчета новых stop values.

Проверяет:
- CE stop;
- flip;
- trail;
- zlsma exit.

## CE architecture

CE:
- bar-based;
- работает по закрытому бару;
- использует ATR length=1;
- ATR mult=2.

## HA architecture

HA:
- HA(1) используется для CE;
- online HA(0) используется для signal compare.

## Adaptive architecture

Adaptive:
- использует ATR fast/slow;
- speed управляет length scaling;
- warmup общий.

---

# Боты и стратегии

## TiTan KRONOS

Техническое название:
```text
TiTan KRONOS (CE Adaptive Strategy)
```

Платформа:
- MT5

Версия:
- 9.4

Статус:
- актуальный.

Основа:
- CE;
- Adaptive ZLSMA;
- HA;
- bar execution.

---

# Формулы и параметры

## AdaptiveFactor

```text
adaptiveFactor = 1 + speed * deltaATRpct * 0.1
```

## deltaATRpct

```text
deltaATRpct = atrFast / atrSlow - 1
```

## Clamp

```text
adaptiveFactor:
0.3 <= value <= 5.0
```

## Warmup

```text
warmup = zlsmaLength
```

## PRICE_SERIES_MAX

```text
PRICE_SERIES_MAX = 200
```

---

# Что проверить дальше

- parity MT5 ↔ Pine;
- влияние HA source;
- влияние stopOffsetPercent;
- native MA vs custom ZLSMA;
- влияние persistent signal;
- parity при zlsmaLength=50 vs 61;
- влияние hybrid stop execution.

---

# Что НЕ переносить в wiki

Пропущено:
- бытовое общение;
- эмоциональные реплики;
- обсуждение интерфейсов;
- личные комментарии;
- всё вне разработки CE/KRONOS.

---

# Цитаты или фрагменты

```text
speed=0 должен давать тот же результат, что и adaptive OFF
```

```text
warmup должен быть ОБЩИМ для adaptive и non-adaptive
```

```text
Decision Gate должен выполняться ДО пересчета stop values
```

---

# Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые гипотезы выписаны: да
- Все новые факты выписаны: да
- Все ошибки и решения выписаны: да
- Все правила и архитектурные решения выписаны: да
