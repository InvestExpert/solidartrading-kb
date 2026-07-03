---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: CE + ZLSMA / Execution Modes / Bar Architecture
related_bot: TiTan KRONOS
related_strategy: CE + ZLSMA / MT5 / Pine
suggested_wiki_targets:
  - Вики/Архитектура
  - Вики/Правила
  - Вики/Стратегии
  - Вики/Идеи
  - Вики/Выводы
created_for: TiTan Bots
---

# CE + ZLSMA: bar architecture, execution modes и persistent signals

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: неизвестно  
Связанный бот: TiTan KRONOS / CE v7.2 Light  
Связанная стратегия или платформа: CE + ZLSMA / Pine / MT5  
Что сохранено:
- архитектура CE + ZLSMA;
- execution modes;
- hybrid / candle режим;
- usePersistentSignal;
- bar-based CE;
- HA логика;
- Decision Gate;
- stopMode / EntryMode;
- адаптивная ZLSMA;
- правила воспроизводимости между Pine и MT5;
- идеи расширений стратегии.
Что пропущено:
- разговорные комментарии;
- нерелевантные бытовые темы.
Риски приватности: низкие  
Предложенное имя файла:
2026-05-20 - ChatGPT - CE ZLSMA execution modes and bar architecture.md

## Obsidian links

- [TiTan KRONOS](TiTan%20KRONOS.md)
- [CE Strategy](CE%20Strategy.md)
- [ZLSMA Adaptive](ZLSMA%20Adaptive.md)
- [Execution Modes](Execution%20Modes.md)
- [Bar Mode Architecture](Bar%20Mode%20Architecture.md)
- [Persistent Signal](Persistent%20Signal.md)
- [Heikin Ashi Processing](Heikin%20Ashi%20Processing.md)

## Краткое резюме

В чате сформирована и зафиксирована архитектура стратегии CE + ZLSMA для Pine и MT5. Главный итог — переход на строгую барную архитектуру для CE, разделение execution modes, формализация hybrid режима, описание usePersistentSignal и синхронизация поведения между Pine Script и MT5.

Также зафиксированы:
- фиксированные параметры CE;
- структура signal-bar / ce-bar / stop-bar;
- правила работы Heikin Ashi;
- правила обработки stop / flip / trail;
- архитектура Decision Gate;
- набор расширений для будущих тестов.

## Точные факты

### Pine Script CE v7.2 Light
Источник: CE v7.2 Light.js

Ключевые параметры:
- `process_orders_on_close = true`
- `calc_on_every_tick = false`
- `pyramiding = 1`

Фиксированные параметры CE:
- `atrLength = 1`
- `atrMult = 2.0`

Основные параметры:
- `baseZlsmaLength`
- `adaptZLSMA`
- `speed`
- `useHeikinAshi`
- `original_useClose`
- `usePersistentSignal`

Persistent signal:
- `pendingSignal = 0`
- при смене направления CE pendingSignal получает:
  - `1` = LONG
  - `-1` = SHORT
- вход выполняется только после подтверждения:
  - LONG: `priceClose > zlsmaValue`
  - SHORT: `priceClose < zlsmaValue`
- после входа pendingSignal сбрасывается.

### Формула adaptive ZLSMA

Используется:
- `atrFast = ATR(baseLen / 5)`
- `atrSlow = ATR(baseLen)`

Формула:
`deltaATRpct = atrFast / atrSlow - 1`

`adaptiveFactor = 1 + speed * deltaATRpct * 0.1`

Ограничения:
- min = 0.3
- max = 5.0

Финальная длина:
`targetLen = baseZlsmaLength * adaptiveFactor`

### Правила Heikin Ashi

Если:
`useHeikinAshi = true`

то:
- все сигналы;
- CE;
- MA;
- входы;
- выходы

используют HA-данные.

HA формулы:
- `haClose = (open + high + low + close) / 4`
- `haOpen = (prev_haOpen + prev_haClose) / 2`

### Execution Modes

Итоговое решение:
оставить только 2 режима.

#### Режим 0 — Hybrid
- CE пересчитывается по закрытию бара;
- сигналы формируются по закрытию;
- стопы могут срабатывать внутри бара;
- рекомендован для реальной торговли.

#### Режим 1 — Candle
- всё работает только по закрытию бара;
- используется для тестов и воспроизводимости.

### Важный вывод по CE

CE не должен работать в tick-mode.

Причины:
- CE использует экстремумы бара;
- high/low ещё не сформированы на тиках;
- при tick-mode CE начинает "мигать";
- появляются ложные flip-сигналы;
- ломается persistent logic;
- результаты перестают воспроизводиться.

Итог:
CE остаётся bar-based даже в hybrid режиме.

## Справочники и соответствия

### EntryMode (MT5 KRONOS 9.4)

- `0 = Legacy`
- `1 = Flip only`
- `2 = Persistent only`
- `3 = Hybrid`

### stopMode

- `0 = CE_THEN_FLIP`
- `1 = FLIP`
- `2 = TRAIL`
- `3 = ZLSMA_TRAIL`

### Execution Modes

- `0 = Hybrid`
- `1 = Candle`

### CE параметры

- `ceAtrLength = 1`
- `ceAtrMult = 2.0`

### usePersistentSignal

- `false` → немедленный вход;
- `true` → deferred entry через pendingSignal.

## Идеи

### 1. Hybrid execution mode
Сигналы по барам, стопы интрабарно.

Причина:
- сохранить стабильность CE;
- ускорить защиту.

### 2. Decision Gate
Создать единую точку принятия решений:
- CE-hit;
- flip;
- trail;
- zlsma-exit.

### 3. Разделение индексации баров

Архитектура:
- signal-bar;
- ce-bar;
- stop-bar.

Назначение:
устранить хаос индексации.

### 4. Deferred signal
Persistent signal как буфер между flip CE и входом.

## Гипотезы

### CE + MA уменьшает ложные flip-сигналы
Статус: частично подтверждена.

Проверка:
- сравнение pure CE vs CE+MA.

### Hybrid режим лучше pure candle для live
Статус: не проверена.

Проверка:
- live-forward тесты.

### Persistent signal улучшает качество входов
Статус: частично подтверждена.

Проверка:
- сравнение количества ложных входов.

## Выводы

### 1. CE должен быть bar-based
Тиковый CE признан архитектурно неправильным.

### 2. Hybrid режим — основной
Для live:
- сигналы по барам;
- стопы интрабарно.

### 3. Candle режим нужен для reproducibility
Используется:
- Pine;
- MT5;
- Python parity.

### 4. Persistent signal полезен на волатильных рынках
Особенно:
- XAUUSD;
- crypto;
- forex intraday.

## Ошибки и решения

### Ошибка: CE в tick-mode

Симптом:
- мигающие сигналы;
- flip noise.

Причина:
- high/low бара не сформированы.

Решение:
- CE только bar-based.

### Ошибка: рассинхрон Pine ↔ MT5

Причина:
- разные execution модели.

Решение:
- process_orders_on_close;
- bar architecture;
- strict candle processing.

### Ошибка: ложные flip-сигналы

Причина:
- мгновенный вход после CE flip.

Решение:
- usePersistentSignal.

## Правила

### 1. CE всегда работает по барам
Даже в hybrid execution.

### 2. HA должен использоваться везде одинаково
CE и MA обязаны использовать один источник цены.

### 3. ATR параметры CE фиксированы
- length = 1
- mult = 2.0

### 4. Decision logic должна быть централизована
Через Decision Gate.

### 5. Stop logic отделяется от signal logic
Стопы могут быть интрабарными.

## Архитектура

### Decision Gate

Порядок проверки:
1. CE-hit
2. Flip
3. Trail
4. ZLSMA exit

### Bar architecture

Разделение:
- signal-bar;
- ce-bar;
- stop-bar.

### Persistent Architecture

Схема:
1. CE flip;
2. pendingSignal;
3. MA confirmation;
4. entry;
5. pending reset.

### Hybrid execution

- signal calc = bar close;
- stop execution = realtime.

## Боты и стратегии

### TiTan KRONOS

Техническое название:
`TiTan KRONOS (CE Adaptive Strategy)`

Платформа:
MT5

Версия:
9.4

Особенности:
- CE;
- adaptive ZLSMA;
- HA;
- hybrid stop architecture;
- persistent entries.

### CE v7.2 Light

Платформа:
Pine Script

Особенности:
- bar execution;
- adaptive ZLSMA;
- persistent signals;
- HA processing.

## Формулы и параметры

### Adaptive ZLSMA

`delta = atrFast / atrSlow - 1`

`adaptiveFactor = 1 + speed * delta * 0.1`

Clamp:
- 0.3
- 5.0

### Persistent Signal

`pendingSignal`
- `1 = long`
- `-1 = short`
- `0 = none`

### Execution

Hybrid:
- signals = bar;
- stops = realtime.

Candle:
- all = bar close.

## Что проверить дальше

- pure CE vs CE+MA;
- hybrid vs candle;
- persistent vs instant;
- EMA vs ZLSMA;
- strict MA filter;
- MA200 trend filter;
- ATR volatility filter;
- re-entry logic;
- pyramiding.

## Что НЕ переносить в wiki

Не переносить:
- разговорные комментарии;
- эмоциональные оценки;
- UI/оформление;
- общие обсуждения без технической ценности.

## Цитаты или фрагменты

Ключевой вывод:
> CE остаётся bar-based даже в hybrid режиме.

Ключевая архитектура:
> signal-bar / ce-bar / stop-bar.

## Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые гипотезы выписаны: да
- Все новые факты выписаны: да
- Все ошибки и решения выписаны: да
- Все правила и архитектурные решения выписаны: да
