---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: неизвестно
related_bot: CE / CE+ZLSMA
related_strategy: MT5 / CE + ZLSMA
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

# CE HA Fix v10 и архитектура Decision Gate

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: неизвестно  
Связанный бот: CE / CE+ZLSMA  
Связанная стратегия или платформа: MT5 / MQL5  
Что сохранено:
- анализ деградации прибыли между версиями v8 → v9.x → v10;
- влияние Heikin Ashi;
- изменение архитектуры Decision Gate;
- логика bar[1] vs bar[0];
- ATR / CE / ZLSMA взаимодействие;
- причины снижения частоты сделок;
- HA High/Low expansion effect;
- исправления версии v10;
- выводы по архитектуре;
- параметры тестов и baseline;
- различие bug vs architectural consequence.

Что пропущено:
- эмоциональные реплики;
- бытовые комментарии;
- нерелевантные фразы.

Риски приватности:
- минимальные;
- рекомендуется ручная проверка перед импортом.

Предложенное имя файла:
2026-05-20 - ChatGPT - CE HA Fix v10 and Decision Gate.md

## Obsidian links

- [CE Strategy](CE%20Strategy.md)
- [CE + ZLSMA](CE%20+%20ZLSMA.md)
- [MT5](MT5.md)
- [Heikin Ashi](Heikin%20Ashi.md)
- [Decision Gate](Decision%20Gate.md)
- [ATR Stop Logic](ATR%20Stop%20Logic.md)
- [Signal Architecture](Signal%20Architecture.md)

## Краткое резюме

Проведен анализ деградации прибыльности стратегии CE + ZLSMA после перехода от версии 8 к архитектуре 9.x. Выявлено, что снижение прибыли при включенном Heikin Ashi связано не с одиночным багом, а с архитектурными изменениями:
- использованием HA-extremes;
- разделением bar[1] и bar[0];
- новой логикой Decision Gate;
- изменением расстояния CE-stop;
- снижением частоты valid flips и re-entry.

В версии 10 внесены точечные исправления:
- CE stop refs переведены на реальные High/Low;
- signal compare переведен на реальный Close[0].

Исправления подтверждены проверкой кода.

## Точные факты

### Версии

- Version 8:
  - baseline profitability выше;
  - ATR=1;
  - Mult=2;
  - оба индикатора включены;
  - HA historically profitable.

- Version 9.x:
  - новая архитектура Decision Gate;
  - bar separation [1]/[0];
  - снижение trade frequency;
  - HA configurations начали проигрывать по прибыли.

- Version 10:
  - внедрен HA FIX;
  - CE stop refs используют real High/Low;
  - signal compare использует real Close[0].

### Параметры baseline

- ceAtrLength = 1
- ceAtrMult = 2.0
- zlsmaLength = 50–61
- zlsmaAdaptiveSpeed = 0

### Формулы HA

```text
haHigh = max(high, haOpen, haClose)
haLow  = min(low, haOpen, haClose)
```

### Формулы CE stop

```text
longStopRaw  = priceHighRef − ATR
shortStopRaw = priceLowRef + ATR
```

### Индексация

- CE/ATR/stops:
  - рассчитываются по закрытому бару [1].

- Signal compare:
  - рассчитывается по текущему бару [0].

### Decision Gate

В 9.x:
1. Сначала проверяются выходы.
2. Используются старые stop values.
3. Только потом обновляются HA/series.
4. После этого проверяются входы.

### Найденный эффект HA

При `useHeikinAshi=true`:
- HA High становится выше реального High;
- HA Low становится ниже реального Low;
- CE stop distance увеличивается;
- flips происходят реже;
- уменьшается количество re-entry;
- падает валовая прибыль.

### Изменения версии 10

Исправлено:
- CE stop refs:
  - теперь используют `iHigh(...,1)` и `iLow(...,1)`.

Исправлено:
- Signal compare:
  - теперь использует `iClose(...,0)` вместо online HA price.

### Остаточные зоны влияния HA

Даже после FIX:
- `_init_trail_stop()` все еще использует `MyLow/MyHigh`;
- ZLSMA feed при HA использует `ha_close`.

## Справочники и соответствия

### Версия → изменение

- v8
  - старое поведение;
  - высокая прибыльность HA.

- v9.x
  - новая архитектура Decision Gate;
  - разделение [1]/[0];
  - HA profitability degradation.

- v10
  - HA FIX;
  - real High/Low for CE stops;
  - real Close[0] for signal compare.

### Компонент → источник цены

- CE stops
  - real High/Low [1] (v10)

- ATR
  - real OHLC [1]

- Signal compare
  - real Close[0] (v10)

- ZLSMA feed
  - HA close (если HA enabled)

- Trail init
  - MyHigh/MyLow (HA-expanded)

## Идеи

### Идея 1
Использовать:
- HA только как smoothing layer;
- real Close как trigger source.

### Идея 2
Унифицировать:
- CE stops;
- trail init;
- signal compare.

Все должны использовать одинаковый тип price source.

### Идея 3
Добавить переключатель:
- ZLSMA source:
  - HA close;
  - real close.

## Гипотезы

### Гипотеза 1

Суть:
HA profitability degradation вызвана не багом, а комбинацией:
- HA extremes;
- Decision Gate;
- bar separation.

Относится:
- CE + ZLSMA.

Проверка:
- A/B test:
  - old HA logic;
  - real High/Low fix.

Статус:
- частично подтверждена.

### Гипотеза 2

Суть:
Главный источник потери прибыли:
- увеличение CE stop distance.

Проверка:
- сравнить trade count;
- compare flip frequency.

Статус:
- частично подтверждена.

### Гипотеза 3

Суть:
Trail initialization через HA extremes продолжает ухудшать profitability.

Проверка:
- заменить MyHigh/MyLow на iHigh/iLow.

Статус:
- не проверена.

### Гипотеза 4

Суть:
HA feed в ZLSMA продолжает сглаживать сигналы и уменьшает trade frequency.

Проверка:
- feed ZLSMA real Close.

Статус:
- не проверена.

## Выводы

### Вывод 1
Снижение прибыльности HA в 9.x:
- системное;
- воспроизводимое;
- не зависит от режимов входа/выхода.

### Вывод 2
Главная причина:
- HA расширяет High/Low;
- CE stop становится дальше;
- flips происходят реже.

### Вывод 3
Decision Gate 9.x:
- более консервативный;
- уменьшает aggressive re-entry.

### Вывод 4
Исправления v10:
- реализованы корректно;
- логических противоречий не обнаружено.

### Вывод 5
После FIX:
- разница между HA ON/OFF должна уменьшиться;
- trade count HA ON должен вырасти относительно 9.x.

## Ошибки и решения

### Ошибка / проблема 1

Симптом:
- HA configurations проигрывают по прибыли.

Причина:
- HA-expanded extremes.

Решение:
- использовать real High/Low для CE stops.

Где:
- CE 9.x.

### Ошибка / проблема 2

Симптом:
- reduced signal frequency.

Причина:
- signal compare через online HA price.

Решение:
- compare через real Close[0].

Где:
- Decision Gate.

### Ошибка / проблема 3

Симптом:
- trail remains conservative.

Причина:
- `_init_trail_stop()` использует MyHigh/MyLow.

Решение:
- заменить на iHigh/iLow.

Статус:
- еще не внедрено.

## Правила

### Правило 1
CE stops и signal compare должны использовать:
- одинаковую philosophy of price source.

### Правило 2
При анализе profitability degradation:
- отделять bug от architectural consequence.

### Правило 3
Для HA logic:
- нельзя смешивать:
  - HA-extremes;
  - real ATR;
  - online HA trigger;
без понимания cumulative effect.

### Правило 4
Все A/B tests:
- выполнять на одинаковом периоде;
- одинаковых режимах;
- одинаковом расписании.

## Архитектура

### Архитурное решение 1

В 9.x:
- ATR/stops:
  - bar[1].

- Signal:
  - bar[0].

### Архитектурное решение 2

Decision Gate:
1. exits;
2. old stop values;
3. update series;
4. entries.

### Архитектурное решение 3

Version 10:
- сохраняет architecture 9.x;
- но меняет price source для:
  - CE stops;
  - signal compare.

### Архитектурное решение 4

Гибридная архитектура:
- HA:
  - smoothing layer;
- real Close:
  - trigger layer.

## Боты и стратегии

### CE + ZLSMA

Техническое название:
- CE + ZLSMA.

Платформа:
- MT5 / MQL5.

Статус:
- актуальная разработка.

Особенности:
- ATR-based CE stops;
- ZLSMA trend filter;
- optional Heikin Ashi smoothing.

## Формулы и параметры

### HA formulas

```text
haHigh = max(high, haOpen, haClose)
haLow  = min(low, haOpen, haClose)
```

Смысл:
- построение HA extremes.

### CE stop formulas

```text
longStopRaw  = priceHighRef − ATR
shortStopRaw = priceLowRef + ATR
```

Смысл:
- расчет CE stop levels.

### Baseline settings

```text
ceAtrLength = 1
ceAtrMult = 2.0
zlsmaLength = 50–61
zlsmaAdaptiveSpeed = 0
```

### Logic separation

```text
Stops/ATR → bar[1]
Signal compare → bar[0]
```

## Что проверить дальше

- Compare:
  - v9.x vs v10 trade count.

- Replace:
  - MyHigh/MyLow → iHigh/iLow.

- Add:
  - ZLSMA source selector.

- Measure:
  - flip frequency;
  - stop distance;
  - re-entry count.

## Что НЕ переносить в wiki

- эмоциональные реплики;
- оценочные комментарии;
- бытовые фразы;
- нерелевантные сообщения.

## Цитаты или фрагменты

```text
HA High становится выше реального High,
HA Low становится ниже реального Low,
CE stop distance увеличивается.
```

```text
Decision Gate:
1. exits
2. old stop values
3. update series
4. entries
```

## Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые гипотезы выписаны: да
- Все новые факты выписаны: да
- Все ошибки и решения выписаны: да
- Все правила и архитектурные решения выписаны: да
- Если что-то не выписано, объясни почему: не требуется
