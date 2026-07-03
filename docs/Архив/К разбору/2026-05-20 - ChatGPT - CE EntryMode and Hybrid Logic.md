---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: CE / CE Adaptive / EntryMode / Persistent vs Flip
related_bot: TiTan KRONOS
related_strategy: MT5 / Pine / CE + ZLSMA
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

# CE EntryMode and Hybrid Logic

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: CE / CE Adaptive / EntryMode / Persistent vs Flip  
Связанный бот: TiTan KRONOS  
Связанная стратегия или платформа: MT5 / Pine / CE + ZLSMA  

Что сохранено:
- архитектура EntryMode;
- конфликт usePersistentSignal и EnterOnlyOnFlip;
- переход от двух bool-флагов к enum-режиму;
- Hybrid режим;
- поведение режимов на M1/M5/M15;
- Pine vs MT5 различия;
- adaptive ZLSMA;
- Decision Gate;
- stopMode;
- pendingSignal;
- online HA(0);
- trail architecture;
- close reasons.

Что пропущено:
- эмоциональные реплики;
- бытовые комментарии;
- промежуточные реакции без инженерной ценности.

Риски приватности:
- низкие.

Предложенное имя файла:
2026-05-20 - ChatGPT - CE EntryMode and Hybrid Logic.md

## Obsidian links

- [TiTan KRONOS](TiTan%20KRONOS.md)
- [CE + ZLSMA](CE%20+%20ZLSMA.md)
- [EntryMode Architecture](EntryMode%20Architecture.md)
- [Persistent Signal](Persistent%20Signal.md)
- [Decision Gate](Decision%20Gate.md)
- [Bar Mode Execution](Bar%20Mode%20Execution.md)
- [Adaptive ZLSMA](Adaptive%20ZLSMA.md)

## Краткое резюме

В чате сформирована полноценная архитектура режимов входа CE-стратегии.

Главный инженерный вывод:
два независимых bool-флага:
- usePersistentSignal
- EnterOnlyOnFlip

создавали конфликтующие состояния и плохо масштабировались.

Было принято решение перейти на enum-архитектуру EntryMode:

- 0 = Legacy
- 1 = Flip only
- 2 = Persistent only
- 3 = Hybrid

Также были:
- формализованы правила pendingSignal;
- зафиксирована логика Hybrid;
- внедрен Decision Gate;
- разделены signal-bar / ce-bar / stop-bar;
- исправлены проблемы HA indexing;
- описаны Pine vs MT5 divergence;
- зафиксирована баровая архитектура CE.

## Точные факты

### Финальная архитектура EntryMode

```cpp
EntryMode:
0 = Legacy
1 = Flip only
2 = Persistent only
3 = Hybrid
```

Смысл режимов:

#### 0 = Legacy
- старое поведение;
- входы всегда разрешены;
- без защиты от повторного входа.

#### 1 = Flip only
- вход разрешен только если CE направление сменилось относительно последнего flat-состояния;
- защита от повторного входа в том же направлении;
- pendingSignal не используется.

#### 2 = Persistent only
- CE flip создает pendingSignal;
- вход выполняется позже после подтверждения ZLSMA;
- pendingSignal используется для восстановления после стопа.

#### 3 = Hybrid
- комбинация Flip-only и Persistent;
- flip блокирует повторный вход;
- но stop может разрешить восстановительный вход через pendingSignal.

---

### stopMode architecture

```cpp
stopMode:
0 = CE_THEN_FLIP
1 = FLIP_ONLY
2 = TRAIL
3 = ZLSMA_TRAIL
```

---

### Adaptive ZLSMA

Формула:

```cpp
deltaATRpct = atrFast / atrSlow - 1
adaptiveFactor = 1 + speed * deltaATRpct * 0.1
adaptiveFactor clamp 0.3 ... 5.0
targetLen = baseLen * adaptiveFactor
```

---

### CE defaults

```cpp
ceAtrLength = 1
ceAtrMult = 2.0
```

Документационно зафиксировано:
- менять нельзя без нарушения синхронизации CE.

---

### Разделение баров

Сформировано строгое разделение:

```cpp
signal-bar = 0
ce-bar     = 1
stop-bar   = 1
```

Где:
- signal-bar используется для signal/ZLSMA;
- ce-bar для CE direction;
- stop-bar для CE stop calculations.

---

### Decision Gate

Decision Gate выполняется ДО пересчетов нового бара.

Проверяет:
- CE-hit;
- Flip-on;
- TRAIL;
- ZLSMA_EXIT.

Приоритет:

```cpp
CE STOP -> FLIP -> TRAIL -> ZLSMA
```

---

### pendingSignal

```cpp
pendingSignal:
0 = none
1 = long
-1 = short
```

Persistent signal:
- создается при CE flip;
- не сбрасывается после обычного стопа;
- сбрасывается после CE_FLIP;
- сбрасывается после TRAIL_STOP;
- сбрасывается после ZLSMA_EXIT.

---

### online HA(0)

Зафиксировано важное правило:

Для signal-price используется:
- online HA(0)

Для CE logic:
- closed HA(1)

Причина:
иначе HA lag на 1 бар ломает сигналы.

---

### Pine vs MT5 divergence

Сформирована гипотеза:

M1 в Pine может выглядеть прибыльным:
- из-за отсутствия реальных комиссий;
- из-за отсутствия спреда;
- из-за intrabar execution differences.

Особенно критично для:
- high-frequency CE;
- M1 scalping;
- small ATR stop distances.

---

### Статистика close reasons

Введена детальная классификация:

```cpp
closeByTrailStop
closeByCeStop
closeByCeFlip
closeByZlsma
closeBySchedule
closeByConflict
```

---

### Конфликт старых флагов

Старые флаги:

```cpp
usePersistentSignal
InpCE_EnterOnlyOnFlip
```

создавали:
- пересечение логики;
- неоднозначные состояния;
- плохо читаемый код;
- сложность оптимизации.

Финальное решение:
замена на единый EntryMode enum.

## Справочники и соответствия

### EntryMode

| Значение | Режим |
|---|---|
| 0 | Legacy |
| 1 | Flip only |
| 2 | Persistent only |
| 3 | Hybrid |

---

### stopMode

| Значение | Режим |
|---|---|
| 0 | CE_THEN_FLIP |
| 1 | FLIP_ONLY |
| 2 | TRAIL |
| 3 | ZLSMA_TRAIL |

---

### pendingSignal

| Значение | Смысл |
|---|---|
| 0 | нет |
| 1 | long |
| -1 | short |

## Идеи

### Unified EntryMode

Идея:
заменить несколько bool-флагов единым enum-режимом.

Причина:
- bool-флаги плохо масштабируются;
- режимы начинают конфликтовать;
- оптимизация становится хаотичной.

---

### Hybrid recovery

Идея:
после стопа разрешать восстановительный вход,
даже если flip-only обычно запрещает повторный вход.

## Гипотезы

### Pine profitability illusion on M1

Суть:
Pine показывает прибыль на M1 из-за отсутствия:
- спреда;
- комиссии;
- реального intrabar исполнения.

Относится:
- CE;
- CE scalping;
- Pine vs MT5.

Как проверить:
- включить комиссии;
- включить спред;
- сравнить MT5.

Статус:
частично подтверждена.

---

### Hybrid improves robustness

Суть:
Hybrid может быть лучшим универсальным режимом:
- уменьшает ложные перезаходы;
- сохраняет recovery после стопов.

Проверка:
M1/M5/M15 optimization.

Статус:
частично подтверждена.

## Выводы

### EntryMode enum лучше bool-флагов

Вывод:
единый enum:
- проще;
- масштабируемее;
- безопаснее;
- лучше оптимизируется.

---

### Persistent сильно влияет на M15

На M15:
- Persistent dramatically changes results;
- уменьшает шум;
- улучшает RF на части конфигураций.

---

### M1 крайне чувствителен к комиссиям

На M1:
- тысячи трейдов;
- маленькое expectancy;
- комиссия может полностью уничтожать edge.

---

### Flip-only уменьшает overtrading

Flip-only:
- уменьшает повторные входы;
- режет trade count;
- улучшает стабильность.

---

### Hybrid оказался наиболее инженерно логичным

Hybrid:
- сохраняет защиту flip-only;
- но не ломает recovery после stop.

## Ошибки и решения

### Ошибка: конфликт двух bool-флагов

Симптом:
- режимы пересекались;
- логика становилась неочевидной.

Причина:
независимые bool-комбинации.

Решение:
единый EntryMode enum.

---

### Ошибка: HA lag

Симптом:
сигналы отставали на 1 бар.

Причина:
использование cached HA вместо online HA(0).

Решение:
- signals -> HA(0)
- CE -> HA(1)

---

### Ошибка: pendingSignal reset

Симптом:
терялись recovery entries.

Причина:
pendingSignal сбрасывался слишком агрессивно.

Решение:
не сбрасывать после обычного стопа.

---

### Ошибка: смешивание signal/stop bars

Симптом:
несогласованность сигналов.

Причина:
одинаковая индексация для разных задач.

Решение:
разделение:
- signal-bar;
- ce-bar;
- stop-bar.

## Правила

### Rule: CE работает только барно

CE:
- всегда bar-based;
- не tick-based.

---

### Rule: ATR defaults фиксированы

```cpp
ceAtrLength = 1
ceAtrMult = 2.0
```

Не менять без полной ревалидации.

---

### Rule: signal-bar != stop-bar

Нельзя смешивать:
- signal calculations;
- stop calculations.

---

### Rule: Decision Gate выполняется первым

Выходы должны проверяться:
- ДО пересчетов;
- ДО новых сигналов.

---

### Rule: pendingSignal survives stop

После обычного stop:
- pendingSignal сохраняется.

## Архитектура

### Decision Gate architecture

Decision Gate:
- единая точка выхода;
- проверяет все stop conditions;
- выполняется первым.

---

### Entry architecture

Система разделена:
- EntryMode;
- stopMode;
- pendingSignal;
- Decision Gate.

---

### Hybrid architecture

Hybrid:
- использует flip memory;
- использует pendingSignal;
- разрешает recovery after stop.

---

### HA architecture

Разделение:
- online HA(0) для сигналов;
- closed HA(1) для CE.

## Боты и стратегии

### TiTan KRONOS

Тип:
- CE adaptive strategy.

Платформа:
- MT5.

Версия:
- 9.4.

Особенности:
- CE;
- adaptive ZLSMA;
- Heikin Ashi;
- EntryMode;
- stopMode;
- Decision Gate.

---

### CE v7.2 Light

Платформа:
- Pine Script.

Особенности:
- adaptive ZLSMA;
- Persistent Signal;
- HA;
- CE logic.

## Формулы и параметры

### AdaptiveFactor

```cpp
adaptiveFactor = 1 + speed * delta * 0.1
```

---

### Delta ATR

```cpp
delta = atrFast / atrSlow - 1
```

---

### Clamp

```cpp
adaptiveFactor:
0.3 ... 5.0
```

---

### ATR settings

```cpp
ceAtrLength = 1
ceAtrMult = 2.0
```

---

### EntryMode

```cpp
0 = Legacy
1 = Flip only
2 = Persistent only
3 = Hybrid
```

---

### stopMode

```cpp
0 = CE_THEN_FLIP
1 = FLIP_ONLY
2 = TRAIL
3 = ZLSMA_TRAIL
```

## Что проверить дальше

- сравнение M1 с реальными комиссиями;
- проверить Hybrid на крипте;
- проверить EntryMode на разных spread profiles;
- сравнить Hybrid vs Flip-only на трендовых рынках;
- проверить M15 robustness;
- проверить sensitivity к ATR length > 1.

## Что НЕ переносить в wiki

Пропущены:
- эмоциональные комментарии;
- личные реплики;
- бытовые фразы;
- промежуточные реакции без технической ценности.

## Цитаты или фрагменты

```cpp
(0=Legacy, 1=Flip only, 2=Persistent only, 3=Hybrid)
```

```cpp
signal-bar = 0
ce-bar     = 1
stop-bar   = 1
```

## Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые гипотезы выписаны: да
- Все новые факты выписаны: да
- Все ошибки и решения выписаны: да
- Все правила и архитектурные решения выписаны: да
