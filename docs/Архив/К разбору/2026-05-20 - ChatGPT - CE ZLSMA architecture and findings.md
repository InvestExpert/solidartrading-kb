---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: неизвестно
related_bot: TiTan KRONOS / CE Adaptive Strategy
related_strategy: MT5 / Pine Script / CE + ZLSMA
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

# CE + ZLSMA architecture and findings

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: неизвестно  
Связанный бот: TiTan KRONOS / CE Adaptive Strategy  
Связанная стратегия или платформа: MT5 / Pine Script / CE + ZLSMA  
Что сохранено:
- оригинальная логика CE;
- Pine и MT5 реализации;
- Adaptive ZLSMA;
- EntryMode / stopMode;
- CE как filter mode;
- CE как flip mode;
- HA архитектура;
- баровая логика;
- выводы по M1/XAUUSD;
- влияние offset;
- narrow stability issue;
- trailing logic;
- Decision Gate architecture.

Что пропущено:
- бытовое обсуждение;
- личные темы;
- эмоциональные оценки.

Риски приватности: низкие

Предложенное имя файла:
2026-05-20 - ChatGPT - CE ZLSMA architecture and findings.md

---

## Obsidian links

- [TiTan KRONOS](TiTan%20KRONOS.md)
- [CE Adaptive Strategy](CE%20Adaptive%20Strategy.md)
- [CE + ZLSMA](CE%20+%20ZLSMA.md)
- [Chandelier Exit](Chandelier%20Exit.md)
- [Adaptive ZLSMA](Adaptive%20ZLSMA.md)
- [MT5 Bar Mode Architecture](MT5%20Bar%20Mode%20Architecture.md)
- [Decision Gate Architecture](Decision%20Gate%20Architecture.md)

---

# Краткое резюме

Обсуждалась архитектура стратегии CE + ZLSMA:
- оригинальная Pine-логика;
- перенос в MT5;
- баровая синхронизация;
- CE stop architecture;
- HA indexing;
- adaptive ZLSMA;
- stop modes;
- CE как directional filter.

Ключевой практический вывод:
лучшие результаты на M1/XAUUSD были получены НЕ в классическом режиме CE flip, а в режиме, где CE работает как широкий directional volatility filter.

---

# Точные факты

## Базовые параметры CE

Фиксированные параметры:
- atrLength = 1
- atrMult = 2.0

Причина:
- обеспечивают согласование CE и ZLSMA;
- изменение нарушает синхронизацию сигналов.

---

## Формула CE

Long:
```text
longStopRaw = highest(high, atrLength) - ATR * atrMult
```

Short:
```text
shortStopRaw = lowest(low, atrLength) + ATR * atrMult
```

При atrLength = 1:
```text
highest(high,1) = current high
lowest(low,1) = current low
```

---

## Логика CE direction

```text
ce_dir =
  1  если close > prevShortStop
 -1  если close < prevLongStop
```

---

## Оригинальный трейлинг CE

Long:
```text
if close > prevLongStop:
    longStop = max(longStopRaw, prevLongStop)
else:
    longStop = longStopRaw
```

Short:
```text
if close < prevShortStop:
    shortStop = min(shortStopRaw, prevShortStop)
else:
    shortStop = shortStopRaw
```

---

## ZLSMA formula

```text
ls_current = linreg(price, len, 0)
ls_prev    = linreg(price, len, 1)

ZLSMA = 2 * ls_current - ls_prev
```

---

## Adaptive ZLSMA

Формула:

```text
atrFast  = ATR(baseLen/5)
atrSlow  = ATR(baseLen)

deltaATR = atrFast / atrSlow - 1

adaptiveFactor =
    1 + speed * deltaATR * 0.1
```

Ограничения:
```text
adaptiveFactor ∈ [0.3 ; 5.0]
```

Финальная длина:
```text
targetLen = baseLen * adaptiveFactor
```

---

# EntryMode

## 0 = Legacy
Свободные входы.

## 1 = Flip only
Вход только после смены CE относительно последнего FLAT.

## 2 = Persistent only
Используется pendingSignal.

## 3 = Hybrid
Стоп важнее flip.

---

# stopMode

## 0 = CE_THEN_FLIP
CE stop имеет приоритет.

## 1 = FLIP_ONLY
Только flip.

## 2 = TRAIL
Используется отдельный trailing.

## 3 = ZLSMA_TRAIL
Trailing + выход по ZLSMA.

---

# Decision Gate Architecture

Важная архитектурная схема.

Порядок:
1. CE-hit
2. Flip-on
3. Trail
4. ZLSMA exit
5. Entry logic

---

# Bar indexing architecture

Разделение баров:

## signal-bar
Используется:
- текущий бар [0]

Для:
- signals;
- ZLSMA comparison.

---

## ce-bar
Используется:
- закрытый бар [1]

Для:
- CE direction;
- HA high/low.

---

## stop-bar
Используется:
- закрытый бар [1]

Для:
- CE stop calculation.

---

# Heikin Ashi architecture

При useHeikinAshi=true:
- все сигналы CE;
- ZLSMA;
- входы;
- выходы

должны использовать один и тот же источник цены.

Иначе появляются ложные flips.

---

# Trailing architecture

Инициализация:
```text
trailLong = low - ATR - offset
trailShort = high + ATR + offset
```

Обновление:
```text
trailLong = max(prevTrail, candidate)
trailShort = min(prevTrail, candidate)
```

---

# Выводы

## Главный практический вывод

Классический CE flip mode оказался хуже режима:
```text
ExitOnFlip = false
```

На практике лучший результат был получен, когда:
- CE НЕ закрывает позицию;
- CE работает как directional filter;
- позиция удерживается дольше.

---

## Вывод по M1/XAUUSD

Лучший результат:
- около 1290$
- на M1
- при ATR≈1
- с очень узким диапазоном рабочих ZLSMA.

---

## Вывод по narrow stability

Положительный результат был только для:
```text
ZLSMA = 5,6,7
```

Большинство других длин:
- отрицательные.

Это признано опасным симптомом.

---

## Интерпретация narrow stability

Гипотеза:
система попала в резонанс между:
- шумом рынка;
- ATR(1);
- частотой ZLSMA.

Система слишком чувствительна к микроструктуре.

---

## Вывод по offset

Offset действует как:
- anti-noise buffer;
- hysteresis layer.

Без offset:
- слишком много false flips.

Слишком большой offset:
- система становится "ленивой".

---

## Вывод по CE filter mode

Практически найден режим:
```text
CE = directional volatility filter
```

А НЕ:
```text
CE = stop system
```

Это стало главным архитектурным выводом чата.

---

# Гипотезы

## Гипотеза 1
CE filter mode может быть устойчивее классического CE flip mode.

Статус:
частично подтверждена тестами.

---

## Гипотеза 2
ATR(1) создаёт частотный резонанс с короткой ZLSMA.

Проверка:
- тест ATR 1..5;
- heatmap ATR vs ZLSMA.

Статус:
не проверена полностью.

---

## Гипотеза 3
Adaptive ZLSMA может расширить narrow profitable range.

Статус:
не проверена полностью.

---

# Ошибки и проблемы

## Проблема: слишком мало сделок

Симптом:
- около 236 сделок в год на M1.

Вывод:
для M1 это слишком мало.

Причины:
- CE слишком инертный;
- слишком большой offset;
- delayed flip.

---

## Проблема: слишком много сделок

Симптом:
- много flips;
- equity drift вниз.

Причина:
- спред;
- комиссия;
- noise around CE line.

---

## Проблема: узкий profitable range

Симптом:
- работают только ZLSMA 5-7.

Причина:
возможный resonance effect.

---

# Правила

## Правило 1
CE direction всегда определяется:
```text
close vs previous CE stops
```

---

## Правило 2
CE stop calculations всегда баровые.

Даже если execution intrabar.

---

## Правило 3
HA calculations должны использовать единый source.

---

## Правило 4
Decision Gate должен проверять:
- CE stop;
- flip;
- trail;
- ZLSMA exit

в фиксированном порядке.

---

# Архитектура

## CE как filter architecture

Вместо:
```text
CE = immediate stop
```

используется:
```text
CE = directional filter
```

---

## Hybrid architecture

Смысл:
- stop важнее flip;
- persistent signal позволяет re-entry.

---

## PendingSignal architecture

Используется для:
- delayed entry;
- re-entry after stop.

---

# Боты и стратегии

## TiTan KRONOS

Тип:
- CE Adaptive Strategy

Платформа:
- MT5

Особенности:
- adaptive ZLSMA;
- HA support;
- CE architecture;
- stopMode;
- EntryMode.

---

## CE v7.2 Light

Платформа:
- Pine Script / TradingView

Особенности:
- original CE implementation;
- adaptive ZLSMA;
- persistent signal.

---

# Что проверить дальше

- heatmap ATR vs ZLSMA;
- stability across months;
- M5/M15 validation;
- CE filter mode vs CE flip mode;
- adaptive ZLSMA robustness;
- spread sensitivity;
- profitability per trade.

---

# Что НЕ переносить в wiki

- эмоциональные комментарии;
- бытовые темы;
- субъективные оценки без технической ценности.

---

# Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые гипотезы выписаны: да
- Все новые факты выписаны: да
- Все ошибки и решения выписаны: да
- Все правила и архитектурные решения выписаны: да
