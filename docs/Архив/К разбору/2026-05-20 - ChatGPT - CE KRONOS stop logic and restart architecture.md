
---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: "CE / KRONOS / CE Adaptive Strategy"
related_bot: "TiTan KRONOS (CE)"
related_strategy: "MT5 / PineScript / CE + ZLSMA"
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

# CE KRONOS stop logic and restart architecture

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: CE / KRONOS / CE Adaptive Strategy  
Связанный бот: TiTan KRONOS (CE)  
Связанная стратегия или платформа: MT5 + PineScript + CE + ZLSMA  
Что сохранено:
- архитектура CE;
- adaptive ZLSMA;
- stopMode;
- EntryMode;
- Decision Gate;
- reload logic;
- stop/trail architecture;
- причины расхождения MT5 vs Pine;
- правила индексации баров;
- warm-up логика;
- HA logic;
- CE ATR logic;
- schedule reload;
- stopOffsetPercent;
- restart architecture;
- причины ложных стопов;
- выводы по баровой логике.

Что пропущено:
- бытовое общение;
- эмоциональные реплики;
- субъективные оценки.

Риски приватности:
- присутствует magic number;
- присутствует имя автора.

Предложенное имя файла:
2026-05-20 - ChatGPT - CE KRONOS stop logic and restart architecture.md

## Obsidian links

- [TiTan KRONOS](TiTan%20KRONOS.md)
- [CE Adaptive Strategy](CE%20Adaptive%20Strategy.md)
- [CE Stop Logic](CE%20Stop%20Logic.md)
- [Adaptive ZLSMA](Adaptive%20ZLSMA.md)
- [MT5 vs Pine Parity](MT5%20vs%20Pine%20Parity.md)
- [Decision Gate Architecture](Decision%20Gate%20Architecture.md)
- [Bar Mode vs Tick Mode](Bar%20Mode%20vs%20Tick%20Mode.md)

## Краткое резюме

Разобрана архитектура CE-бота KRONOS для MT5 и её отличие от PineScript-версии. Основной фокус — причины расхождения результатов между Pine и MT5. Вывод: основная проблема находится не в ZLSMA и не в adaptive ATR, а в архитектуре стопов, reload/reset логике и различиях между баровой и тиковой обработкой.

Также зафиксирована финальная архитектура:
- CE всегда работает барно;
- stop processing может быть интрабарным;
- введён Decision Gate;
- stopMode разделён на режимы;
- введена строгая индексация signal-bar / ce-bar / stop-bar;
- решена проблема warm-up ZLSMA;
- сохранена история сглаживания при reload;
- обсуждён hard restart vs warm restart.

## Точные факты

### Бот

- Название:
  - TiTan KRONOS
  - CE Adaptive Strategy

### Версия

- KRONOS CE v9.4
- PineScript reference:
  - CE v7.2 Light

### Базовые параметры CE

- ceAtrLength = 1
- ceAtrMult = 2.0

В документации указано:
- эти параметры являются обязательными;
- изменение нарушает синхронизацию CE и MA.

### Adaptive ZLSMA

Формула:

```text
delta = atr_fast / atr_slow - 1.0
adaptiveFactor = 1 + speed * delta * 0.1
```

Ограничения:

```text
adaptiveFactor ∈ [0.3 ; 5.0]
```

### Источники ATR

Используются отдельные ATR:
- ATR для CE stop;
- ATR fast для adaptive ZLSMA;
- ATR slow для adaptive ZLSMA;
- ATR для trail initialization/update.

### Warm-up логика

Пока данных меньше zlsmaLength:
- используется simplified ZLSMA;
- если данных очень мало:
  - возвращается текущая цена;
- если линрегрессия не рассчиталась:
  - используется последняя цена.

### Индексация баров

Разделение на 3 типа баров:

```text
signal-bar = 0
ce-bar     = 1
stop-bar   = 1
```

Где:
- signal-bar:
  - сигналы;
  - price vs ZLSMA;
  - online HA(0).
- ce-bar:
  - CE direction;
  - HA close;
  - flip detection.
- stop-bar:
  - ATR;
  - CE stop;
  - highs/lows.

### Decision Gate

Decision Gate расположен ДО обновления CE.

Проверяет:
1. CE-hit;
2. flip-on;
3. trail;
4. ZLSMA exit.

Использует:
- старые stop значения;
- старое направление;
- закрытый бар [1].

### stopMode

```text
0 = CE_THEN_FLIP
1 = FLIP_ONLY
2 = TRAIL
3 = ZLSMA_TRAIL
```

### EntryMode

```text
0 = Legacy
1 = Flip only
2 = Persistent only
3 = Hybrid
```

### CE stop

CE stop:
- хранится БЕЗ offset;
- offset применяется:
  - только в trail;
  - только в stop check;
  - НЕ внутри longStop_orig/shortStop_orig.

### HA правила

Для signal-bar:
- используется online HA(0).

Для CE:
- используется закрытый HA(1).

### Trail logic

Trail:
- инициализируется сразу после входа;
- обновляется только на новом баре;
- движется монотонно:
  - LONG → max(prev, candidate)
  - SHORT → min(prev, candidate)

### Schedule reload

При reload:
- CE state сбрасывается;
- trail state сбрасывается;
- pendingSignal сбрасывается;
- HA state сбрасывается.

Но:
- price_series НЕ очищается;
- negativeTradesCounter НЕ очищается.

### Reload behavior

Используются:
- reloadAtWeekend;
- reloadAtNight.

### Night window

```text
23:50 → 01:10
```

### Weekend logic

Weekend:
- Friday >= 19:00;
- Saturday;
- Sunday.

## Справочники и соответствия

### stopMode

| stopMode | Поведение |
|---|---|
| 0 | CE stop имеет приоритет над flip |
| 1 | Только flip |
| 2 | Только trail |
| 3 | Trail + ZLSMA exit |

### EntryMode

| EntryMode | Поведение |
|---|---|
| 0 | Legacy |
| 1 | Только flip |
| 2 | Persistent |
| 3 | Hybrid |

### Причины расхождения MT5 vs Pine

| Причина | Статус |
|---|---|
| 50 тиков | не главная |
| adaptive ZLSMA | не главная |
| reload/reset | критично |
| stop architecture | критично |
| spread | критично |
| tick stop behavior | критично |
| aggressive trail | критично |
| persistent weakened | влияет |

## Идеи

### Warm initialization после паузы

Идея:
- после reload не торговать сразу;
- сначала дождаться:
  - нового сигнала;
  - нового CE flip;
  - прогрева.

### Hard restart vs warm restart

Разделить:
- hard reset:
  - очистка истории;
- warm reset:
  - сохранение price_series.

### Stop softening

Идеи:
1. stopOffsetPercent;
2. отдельный стартовый stop;
3. delayed trailing activation;
4. более широкий первый stop;
5. ATR multiplier только для initial stop.

### ZLSMA warm-up fallback

Пока данных мало:
- использовать simplified ZLSMA;
- не ждать 50 тиков.

## Гипотезы

### Главная причина убыточности MT5 — stop architecture

Суть:
- Pine работает барно;
- MT5 закрывает слишком агрессивно по stop logic.

Статус:
- частично подтверждена.

Проверка:
- отключение aggressive stop;
- сравнение stop-only behavior.

### Reload усиливает расхождения

Суть:
- reload уничтожает состояние CE/HA/trail;
- Pine не имеет такого reset.

Статус:
- подтверждается логами.

### Spread критичен для M1 XAU

Суть:
- Pine работает без реального spread;
- MT5 использует Ask/Bid.

Статус:
- подтверждена.

## Выводы

### Главная проблема НЕ в adaptive ZLSMA

Adaptive ZLSMA:
- не является главным источником расхождения.

### 50 тиков — не корень проблемы

Warm-up:
- влияет только на старт;
- не объясняет общий минус стратегии.

### Стопы слишком агрессивны

Особенно:
- на M1 XAU;
- при интрабарной обработке.

### Reload после расписания создаёт новый режим рынка

После:
- weekend;
- night pause;

бот:
- стартует без актуального контекста;
- может сразу получить ложный stop.

### Pine и MT5 используют разную модель исполнения

Pine:
- чисто барная.

MT5:
- гибридная:
  - bar logic;
  - tick stop execution.

## Ошибки и решения

### Ошибка: reset history при reload

Симптом:
- потеря контекста после паузы.

Причина:
- очистка внутренних состояний.

Решение:
- сохранять price_series;
- использовать warm initialization.

### Ошибка: double stop aggressiveness

Симптом:
- слишком ранние stop exits.

Причина:
- tick execution + узкий CE.

Решение:
- stopOffsetPercent;
- delayed trail;
- separate initial stop.

### Ошибка: использование текущего HA не там где нужно

Симптом:
- repaint-like behavior.

Решение:
- online HA(0) только для signal-bar;
- closed HA(1) для CE.

## Правила

### CE всегда барный

Даже если stop работает по тикам:
- CE пересчитывается только по закрытому бару.

### ATR для stop всегда брать с закрытого бара

Используется:
- CopyBuffer(..., shift=1)

### CE stop хранится без offset

Offset:
- только для trail/check logic.

### signal-bar != stop-bar

Нельзя смешивать:
- signal calculations;
- stop calculations.

### Не торговать сразу после restart

Нужен:
- warm-up;
- новый сигнал;
- восстановление контекста.

## Архитектура

### Decision Gate architecture

Порядок:
1. CE-hit;
2. flip-on;
3. trail;
4. ZLSMA exit;
5. только потом пересчёт CE.

### Разделение логики

Разделены:
- signal;
- CE;
- stop;
- trail;
- execution.

### Hybrid execution

Система:
- барная логика;
- тиковые stop checks.

### Warm reload architecture

Сохраняются:
- price_series;
- сглаживание;
- часть статистики.

Сбрасываются:
- stop state;
- HA state;
- CE state.

## Боты и стратегии

### TiTan KRONOS

Тип:
- CE adaptive strategy.

Платформа:
- MT5.

Основа:
- CE;
- ZLSMA;
- HA.

### Pine reference strategy

Название:
- CE v7.2 Light.

Используется:
- как эталон логики.

## Формулы и параметры

### Adaptive factor

```text
adaptiveFactor = 1 + speed * delta * 0.1
```

### Delta ATR

```text
delta = atr_fast / atr_slow - 1.0
```

### Ограничения adaptiveFactor

```text
0.3 <= adaptiveFactor <= 5.0
```

### Trail update

LONG:
```text
trailLong = max(prev, candidate)
```

SHORT:
```text
trailShort = min(prev, candidate)
```

### CE flip

```text
close > prev_short → long
close < prev_long  → short
```

## Что проверить дальше

- сравнить:
  - pure bar stop;
  - hybrid stop;
  - tick stop.

- проверить:
  - delayed trail activation;
  - larger initial stop;
  - ATR multiplier for initial stop.

- проверить:
  - warm restart;
  - hard restart.

- проверить:
  - stopOffsetPercent.

## Что НЕ переносить в wiki

Не переносить:
- эмоции;
- споры;
- бытовые сообщения;
- субъективные оценки.

## Цитаты или фрагменты

```text
signal-bar = 0
ce-bar = 1
stop-bar = 1
```

```text
CE всегда барный
```

```text
Offset не должен модифицировать оригинальный CE stop
```

## Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые гипотезы выписаны: да
- Все новые факты выписаны: да
- Все ошибки и решения выписаны: да
- Все правила и архитектурные решения выписаны: да
