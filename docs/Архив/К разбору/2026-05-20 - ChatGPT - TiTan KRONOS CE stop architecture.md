---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: средняя
needs_review: true
source_chat_title: "TiTan KRONOS CE stop architecture: MT5 vs Pine"
related_bot: другой
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

# TiTan KRONOS CE stop architecture

## Статус обработки

Релевантность: высокая  
Уверенность: средняя  
Нужна ручная проверка: да  
Исходное название чата: неизвестно; рабочее название — "TiTan KRONOS CE stop architecture: MT5 vs Pine"  
Связанный бот: TiTan KRONOS (CE Adaptive Strategy)  
Связанная стратегия или платформа: CE + ZLSMA + Heikin-Ashi; MT5 / MQL5; источник сравнения — Pine Script / TradingView `CE v7.2 Light`  
Что сохранено:
- перенос CE + ZLSMA стратегии из Pine/JS-логики в MT5;
- архитектура `Decision Gate`;
- различия `CE Stop`, `CE Flip`, `Trail Stop`, `ZLSMA Exit`;
- проблема исчезновения `CE Flip`;
- режимы стопов `stopMode`;
- режимы входа `EntryMode`;
- правила индексации баров `[0]` и `[1]`;
- точные параметры, формулы и тестовые наблюдения;
- найденные баги и решения;
- гипотезы для дальнейших тестов.

Что пропущено:
- эмоциональные реплики;
- бытовые темы;
- личные данные и контакты;
- copyright/правовые блоки из шапки кода;
- маркетинговые формулировки, кроме технического названия и версии.

Риски приватности: низкие, но есть авторские/контактные данные в шапке кода. Их не переносить в wiki.  
Предложенное имя файла: `2026-05-20 - ChatGPT - TiTan KRONOS CE stop architecture.md`

## Obsidian links

- [TiTan KRONOS](TiTan%20KRONOS.md)
- [CE Strategy](CE%20Strategy.md)
- [ZLSMA](ZLSMA.md)
- [Heikin-Ashi](Heikin-Ashi.md)
- [MT5](MT5.md)
- [Pine Script](Pine%20Script.md)
- [Decision Gate](Decision%20Gate.md)

## Краткое резюме

В чате разбиралась стратегия TiTan KRONOS, основанная на CE-логике, ZLSMA и Heikin-Ashi, при портировании из Pine/TradingView в MT5/MQL5.

Ключевой вывод: после выравнивания логики под Pine и переноса проверок в `Decision Gate` стратегия фактически превратилась в single-stop систему: почти все выходы стали происходить по `CE Stop`, а `CE Flip`, `Trail Stop` и `ZLSMA Exit` практически перестали участвовать. Это объясняет падение прибыли по сравнению со старыми, технически неправильными, но более “дышащими” версиями.

Главная техническая проблема: CE-выход проверяется по `High/Low[1]`, а Flip — по `Close[1]`. Если CE-stop стоит раньше Flip и делает `return`, то хвост свечи почти всегда перехватывает сделку раньше, чем close сформирует смену направления CE.

## Точные факты

### Бот / стратегия

- Техническое название в коде: `TiTan KRONOS (CE Adaptive Strategy)`.
- Версия файла: `9.4`.
- Платформа: MT5 / MQL5.
- Источник сравнения: Pine Script `CE v7.2 Light`.
- Торговая идея: CE + ZLSMA + Heikin-Ashi.
- Актив в технической шапке: XAU, 1 minute.
- Magic number в версии 9.4: `111815141519`.
- Comment tag: `KRONOS_CE`.

### Pine / JS источник

В Pine-версии `CE v7.2 Light`:

```pinescript
strategy("CE v7.2 Light", process_orders_on_close=true, calc_on_every_tick=false, pyramiding=1)
```

Базовые CE-параметры:

```pinescript
int atrLength = 1
float atrMult = 2.0
```

Важное поведение Pine:

- `process_orders_on_close = true`;
- `calc_on_every_tick = false`;
- CE пересчитывается по закрытию бара;
- `strategy.exit(..., stop=longStop_orig/shortStop_orig)` выставляет стоп;
- `strategy.close(...)` закрывает по смене направления CE;
- отдельного независимого трейлинга в Pine-версии нет.

### Формулы CE из Pine

Long stop:

```pinescript
longStopRaw = ta.highest(priceHighRef, atrLength) - atr_orig
prevLongStop = nz(longStop_orig[1], longStopRaw)
longStop = prevClose > prevLongStop ? math.max(longStopRaw, prevLongStop) : longStopRaw
```

Short stop:

```pinescript
shortStopRaw = ta.lowest(priceLowRef, atrLength) + atr_orig
prevShortStop = nz(shortStop_orig[1], shortStopRaw)
shortStop = prevClose < prevShortStop ? math.min(shortStopRaw, prevShortStop) : shortStopRaw
```

Направление CE:

```pinescript
tmpPrevLongStop  = nz(longStop_orig[1], longStop_orig)
tmpPrevShortStop = nz(shortStop_orig[1], shortStop_orig)
ce_dir_orig := (close > tmpPrevShortStop) ? 1 : (close < tmpPrevLongStop ? -1 : ce_dir_orig)
```

Сигналы:

```pinescript
buySignal  = ce_dir_orig == 1 and ce_dir_orig[1] == -1
sellSignal = ce_dir_orig == -1 and ce_dir_orig[1] == 1
```

### Формула ZLSMA

```pinescript
ZLSMA = 2 * linreg(price, len, 0) - linreg(price, len, 1)
```

### Адаптивная ZLSMA

В Pine-версии использовались:

```pinescript
atrFast = ta.atr(max(1, baseZlsmaLength / 5))
atrSlow = ta.atr(baseZlsmaLength)
deltaATRpct = atrFast / atrSlow - 1
adaptiveFactor = 1 + speed * deltaATRpct * 0.1
adaptiveFactor := min(adaptiveFactor, 5.0)
adaptiveFactor := max(adaptiveFactor, 0.3)
targetLen = baseZlsmaLength * adaptiveFactor
```

Смысл:
- `speed > 0` удлиняет ZLSMA при росте волатильности;
- `speed < 0` укорачивает ZLSMA при росте волатильности;
- влияние ограничено диапазоном `0.3...5.0`.

### Основные параметры MT5 версии 9.4

```cpp
input long   magic_number       = 111815141519;
input string commentTag         = "KRONOS_CE";
input double lot_size           = 0.01;
input int    zlsmaLength        = 61;
input int    zlsmaAdaptiveSpeed = 0;
input int    stopMode           = 0;
input double stopOffsetPercent  = 0.0;
input bool   useHeikinAshi      = true;
input bool   original_useClose  = false;
input int    EntryMode          = 1;
input int    ceAtrLength        = 1;
input double ceAtrMult          = 2.0;
```

### Тестовые наблюдения из логов

#### Режим stopMode=0

Лог:

- `Total Trades: 1308`;
- `Profitable: 446`;
- `Loss: 853`;
- `Win Rate: 34.10%`;
- `Trail Stop: 0 (0.00%)`;
- `CE Stop: 1229 (93.96%)`;
- `CE Flip: 0 (0.00%)`;
- `ZLSMA Exit: 0 (0.00%)`;
- `Schedule: 79 (6.04%)`.

#### Режим stopMode=1

Лог дал те же значения:

- `Total Trades: 1308`;
- `Profitable: 446`;
- `Loss: 853`;
- `Win Rate: 34.10%`;
- `Trail Stop: 0 (0.00%)`;
- `CE Stop: 1229 (93.96%)`;
- `CE Flip: 0 (0.00%)`;
- `ZLSMA Exit: 0 (0.00%)`;
- `Schedule: 79 (6.04%)`.

Вывод: режимы 0 и 1 в текущей архитектуре фактически дают одинаковый результат, потому что Flip не участвует в торговле.

#### Более ранний лог при активном CE

- `Total Trades: 3345`;
- `Win Rate: 39.94%`;
- `CE Stop: 3252 (97.22%)`;
- `CE Flip: 0`;
- `Trail Stop: 0`;
- `Schedule: 93 (2.78%)`.

#### Лог при другой паре ATR-параметров

Пользователь уточнил:
- первый лог был `ATR length = 4`, `mult = 2`;
- второй лог был `ATR length = 2`, `mult = 5`.

Во втором логе:
- `Total Trades: 312`;
- `Win Rate: 38.78%`;
- `CE Stop: 252 (80.77%)`;
- `CE Flip: 0`;
- `Trail Stop: 0`;
- `Schedule: 60 (19.23%)`.

Вывод: изменение ATR-параметров меняет частоту CE-выходов и долю расписания, но не оживляет Flip.

### Старые версии

Пользователь отметил:
- на старых неправильных версиях удавалось получать около `1213$` за год;
- после исправлений прибыль упала примерно до `500$`.

Вывод: старые версии могли быть технически менее корректны, но давали сделкам больше “дышать”. Новая логика стала строже и раньше закрывает позиции.

## Справочники и соответствия

### stopMode в версии 9.4

| stopMode | Название в коде | Смысл |
|---:|---|---|
| 0 | `CE_THEN_FLIP` | Сначала CE Stop, затем CE Flip |
| 1 | `FLIP_ONLY` | Только Flip в Decision Gate |
| 2 | `TRAIL` | CE/Flip в Decision Gate не закрывают; проверяется Trail |
| 3 | `ZLSMA_TRAIL` | Проверяются Trail и ZLSMA Exit |
| default | `CE_THEN_FLIP` | Неизвестный режим ведёт себя как режим 0 |

Важно: в более раннем обсуждении предлагалась более широкая матрица режимов:
- `PINE_CE_THEN_FLIP`;
- `FLIP_THEN_CE`;
- `CE_ONLY`;
- `FLIP_ONLY`;
- `TICK_TRAIL_FIRST`;
- `CE_HYSTERESIS`;
- `NO_CE_NO_FLIP`.

Но в финальном коде 9.4 фактическая таблица другая и короче.

### EntryMode

| EntryMode | Название | Смысл |
|---:|---|---|
| 0 | `Legacy` | Вход разрешён всегда по базовым условиям |
| 1 | `Flip only` | Вход только при смене CE относительно последнего flat-направления |
| 2 | `Persistent only` | Используется `pendingSignal` для восстановления входа |
| 3 | `Hybrid` | После стопа можно восстановиться через `pendingSignal`, иначе вход только при смене CE |

### Причины закрытия

| Причина | Счётчик |
|---|---|
| `TRAIL_STOP` | `closeByTrailStop` |
| `CE STOP` | `closeByCeStop` |
| `CE FLIP` | `closeByCeFlip` |
| `ZLSMA_EXIT` / `ZLSMA EXIT` | `closeByZlsma` |
| `SCHEDULE` | `closeBySchedule` |

### Баровая индексация в версии 9.4

- `barIdxSignal = 0`: сигнальная цена для сравнения с ZLSMA использует текущий формирующийся бар `[0]` / online HA(0).
- `barIdxCE = 1`: CE-направление рассчитывается по закрытому бару `[1]`.
- `barIdxStop = 1`: стопы рассчитываются по закрытому бару `[1]`.

В коде это явно разделено на три группы:
- signal-bar;
- ce/ha-bar;
- stop-bar.

## Идеи

### Идея 1. Универсальный переключатель режимов стопов

Создать один параметр `stopMode`, который позволяет тестировать режимы выходов без переписывания логики.

Ранее предложенные режимы:

```cpp
STOPMODE_PINE_CE_THEN_FLIP = 0
STOPMODE_FLIP_THEN_CE      = 1
STOPMODE_CE_ONLY           = 2
STOPMODE_FLIP_ONLY         = 3
STOPMODE_TICK_TRAIL_FIRST  = 4
STOPMODE_CE_HYSTERESIS     = 5
STOPMODE_NO_CE_NO_FLIP     = 6
```

Статус: идея частично реализована, но в версии 9.4 фактические значения `stopMode` отличаются.

### Идея 2. Режим close-only CE

Добавить режим CE-стопа, где выход происходит не по хвосту свечи, а по закрытию:

```cpp
LONG:  Close[1] <= ce_stop_price
SHORT: Close[1] >= ce_stop_price
```

Вместо текущего:

```cpp
LONG:  Low[1] <= ce_stop_price
SHORT: High[1] >= ce_stop_price
```

Смысл: проверить, вернётся ли `CE Flip` и увеличится ли средняя длительность сделки.

Статус: идея для теста, не подтверждена.

### Идея 3. Лог конфликтов CE/Flip

Добавить счётчик ситуаций, где одновременно:
- `ce_hit = true`;
- `flip_on = true`.

В версии 9.4 уже есть:

```cpp
int closeByConflict = 0;
```

И в Decision Gate:

```cpp
if(ce_hit && flip_on) {
    closeByConflict++;
}
```

Смысл: показать, сколько Flip-сценариев было “задушено” CE-приоритетом.

Статус: реализовано как диагностика.

### Идея 4. Сравнение duration старой и новой версии

Сравнить:
- среднюю длительность сделки;
- максимальную длительность сделки;
- bars in trade.

Гипотеза: новая версия режет сделки значительно раньше, чем старая.

Статус: идея для проверки.

### Идея 5. CE hysteresis

Добавить к CE-условию дополнительный фильтр прокола:

```cpp
price <= ce_stop_price - k * ATR(1)
price >= ce_stop_price + k * ATR(1)
```

Рекомендуемые тестовые значения:
- `k = 0.10`;
- `k = 0.20`.

Смысл: отфильтровать случайные касания хвостом и дать Flip/ZLSMA шанс участвовать.

Статус: идея для теста.

## Гипотезы

### Гипотеза 1. CE-hit по High/Low убивает Flip

Суть: если CE-stop проверяется по `High/Low[1]`, а Flip по `Close[1]`, то CE почти всегда срабатывает раньше и не даёт Flip дойти до исполнения.

Относится к: TiTan KRONOS / CE + ZLSMA / MT5.

Как проверить:
- сравнить режим `High/Low CE` против `Close-only CE`;
- включить `closeByConflict`;
- проверить долю `CE Flip`.

Статус: частично подтверждена логами. В режимах 0 и 1 `CE Stop = 93.96%`, `CE Flip = 0%`.

### Гипотеза 2. Новая логика стала слишком ранней по выходам

Суть: исправленная версия стала логически чище, но закрывает сделки раньше, поэтому прибыль упала с около `1213$` до около `500$`.

Относится к: TiTan KRONOS / MT5.

Как проверить:
- сравнить среднюю длительность сделки старой и новой версии;
- сравнить распределение причин закрытия;
- сравнить PnL при одинаковых входах.

Статус: частично подтверждена наблюдениями пользователя.

### Гипотеза 3. Offset CE ухудшает результат

Суть: дополнительный offset в CE-логике не улучшает фильтрацию, а ухудшает результат, потому что CE и так слишком часто перехватывает сделки.

Относится к: TiTan KRONOS / CE Stop.

Как проверить:
- прогнать `stopCeOffsetPercent` по сетке `0.00`, `0.02`, `0.05`, `0.10`;
- сравнить PnL, CE%, Flip%, среднюю длительность сделки.

Статус: частично подтверждена наблюдением пользователя: “оффсет на CE ухудшает результат”.

### Гипотеза 4. Trail offset не влияет, потому что Trail не участвует

Суть: изменение `stopOffsetPercent` почти не влияет на результат, если `Trail Stop = 0%`.

Относится к: TiTan KRONOS / Trail Stop.

Как проверить:
- сначала добиться ненулевого `Trail Stop`;
- затем тестировать `stopOffsetPercent`.

Статус: подтверждена текущими логами: `Trail Stop = 0%`.

### Гипотеза 5. Режимы 0 и 1 одинаковы не из-за switch, а из-за отсутствия flip_on

Суть: switch работает, потому что режимы 3 и 6 ранее давали другой результат. Но режимы 0 и 1 совпадают, потому что `flip_on` практически не становится true или не доходит до исполнения.

Относится к: TiTan KRONOS / Decision Gate.

Как проверить:
- логировать `stopMode`, `ce_hit`, `flip_on`;
- сравнить количество `flip_on=true` с количеством `CE FLIP`.

Статус: частично подтверждена.

## Выводы

1. `CE Stop`, `CE Flip` и `Trail Stop` — разные механизмы:
   - `CE Stop`: уровеньный выход;
   - `CE Flip`: смена направления CE по закрытию;
   - `Trail Stop`: независимый трейлинг-модуль, не существующий отдельно в исходной Pine-версии.

2. В текущей MT5 версии почти все выходы происходят по `CE Stop`.

3. `CE Flip` не участвует в торговле при текущей архитектуре и текущих параметрах.

4. `Trail Stop` не участвует в торговле, если статистика показывает `Trail Stop = 0%`.

5. Если режимы 0 и 1 дают одинаковую статистику, это не обязательно означает, что `stopMode` не работает. Это может означать, что `flip_on` не возникает или не доходит до исполнения.

6. Проверка CE-стопа по `High/Low[1]` более агрессивна, чем Flip по `Close[1]`.

7. `stopCeOffsetPercent` нельзя встраивать в сами уровни CE. Offset должен применяться только в проверке, иначе получается двойное смещение.

8. Стратегия после исправлений стала более логичной, но менее прибыльной, потому что сделки закрываются раньше.

9. Текущая архитектура фактически превращает многослойную стратегию в single-stop систему.

10. 216 сделок в год на M5 для активной CE-стратегии по XAU выглядит слишком мало и указывает либо на чрезмерно редкие выходы, либо на слишком сильное подавление торговой логики.

## Ошибки и решения

### Ошибка 1. CE Stop проверялся после пересчёта новых уровней

Симптом:
- `CE Stop = 0%` в логах;
- даже при большом трейлинге CE Stop не работал.

Причина:
- сначала пересчитывались новые CE-уровни;
- потом проверялся пробой по уже обновлённому стопу;
- старое касание исчезало.

Решение:
- перенести проверку CE-стопа вверх, до пересчёта новых уровней;
- проверять старый стоп по закрытому бару `[1]`.

Где проявляется:
- MT5 версия CE 9.x;
- bar-mode / попытка имитировать Pine.

### Ошибка 2. Избыточное решение с новыми глобальными переменными

Симптом:
- было предложено добавлять `ce_stop_active`, `ce_active_side`, `ce_stop_active_time`.

Причина:
- попытка явно хранить “активный стоп следующего бара”.

Решение:
- не добавлять новые глобальные переменные;
- использовать существующие `ce_stop_price`, `longStop_orig`, `shortStop_orig`;
- просто перенести CE-check перед пересчётом.

Статус:
- отвергнуто как лишнее усложнение.

### Ошибка 3. Смешение `stopOffsetPercent` и `stopCeOffsetPercent`

Симптом:
- CE-логика использовала общий трейлинговый offset;
- в другом месте CE использовал отдельный `stopCeOffsetPercent`.

Причина:
- разные параметры offset применялись к разным участкам CE.

Решение:
- для CE использовать только `stopCeOffsetPercent`;
- для независимого трейлинга использовать `stopOffsetPercent`.

Статус:
- в версии 9.4 CE-offset фактически убран из входных параметров; остался `stopOffsetPercent` для трейла.

### Ошибка 4. Двойной CE-offset

Симптом:
- CE Flip почти невозможен;
- CE-стопы сдвигались сильнее ожидаемого.

Причина:
- offset добавлялся в расчёт CE-уровней;
- потом offset применялся ещё раз при проверке.

Решение:
- CE-уровни считать чисто, без offset;
- offset применять только в проверке.

Правило:
```cpp
longStopRaw = priceHighRef - atr_orig;
shortStopRaw = priceLowRef + atr_orig;
```

Без offset в этих строках.

### Ошибка 5. Несоответствие строк `"CE_FLIP"` и `"CE FLIP"`

Симптом:
- `pendingSignal` мог не сбрасываться после закрытия по Flip.

Причина:
- в сбросе искалась строка `"CE_FLIP"`, а реальная причина закрытия была `"CE FLIP"`.

Решение:
- добавить оба варианта:
```cpp
StringFind(reason, "CE_FLIP") >= 0 ||
StringFind(reason, "CE FLIP") >= 0
```

Статус:
- в версии 9.4 исправлено.

### Ошибка 6. Legacy `CheckStopLoss()`

Симптом:
- старая функция могла конфликтовать с новой централизованной логикой.

Причина:
- после переноса логики в `Decision Gate` часть старой логики стала дублирующей.

Решение:
- не использовать legacy `CheckStopLoss()`;
- все выходы централизовать через `Decision Gate`.

### Ошибка 7. Трейл не влияет, потому что не участвует

Симптом:
- изменение `stopOffsetPercent` не меняет результат;
- статистика `Trail Stop = 0%`.

Причина:
- Trail не становится причиной закрытия;
- CE закрывает раньше.

Решение:
- тестировать режимы, где CE отключён или Trail проверяется отдельно;
- добиться ненулевой доли `Trail Stop`, затем оптимизировать offset.

## Правила

1. Сначала проверяются выходы на старых данных, затем пересчитываются новые индикаторы и стопы.

2. CE-уровни должны считаться без offset.

3. Offset, если используется, применяется только в проверке стопа, а не в расчёте самого CE-уровня.

4. Для CE и независимого Trail должны использоваться разные параметры offset.

5. Если выход по CE проверяется по `High/Low`, он будет агрессивнее, чем Flip по `Close`.

6. Если нужно проверить вклад отдельного механизма выхода, остальные механизмы должны быть отключены или изолированы через `stopMode`.

7. Если причина выхода имеет строковый формат, статистика и сервисная логика должны искать тот же формат.

8. Если `Trail Stop = 0%`, оптимизация параметров трейла бессмысленна.

9. Если `CE Flip = 0%`, нужно сначала логировать `flip_on`, а не крутить ATR-параметры вслепую.

10. Для сравнения Pine и MT5 нужно фиксировать:
    - какой бар используется для сигналов;
    - какой бар используется для CE;
    - какой бар используется для stop-check;
    - используется ли `High/Low` или `Close`.

11. В MT5-портировании нельзя молча смешивать барную и интрабарную механику: это меняет смысл стратегии.

12. При анализе падения прибыли нужно сравнивать не только PnL, но и:
    - количество сделок;
    - причины закрытия;
    - среднюю длительность сделки;
    - распределение `CE Stop / CE Flip / Trail / Schedule`.

## Архитектура

### Decision Gate

`Decision Gate` — единая точка принятия решений о выходе до пересчёта новых значений.

Текущая схема версии 9.4:

1. Получить старые значения:
   - `prev_long = longStop_orig`;
   - `prev_short = shortStop_orig`;
   - `prev_dir_old = ce_dir_orig`.

2. Получить данные закрытого бара `[1]`:
   - `price_close_ce_old`;
   - `checkHigh`;
   - `checkLow`.

3. Рассчитать:
   - `ce_hit`;
   - `flip_on`;
   - `conflict`.

4. Применить `stopMode`.

5. Если выхода нет:
   - обновить HA и price_series;
   - рассчитать CE-стопы;
   - рассчитать ZLSMA;
   - обновить трейлинг;
   - проверить входы.

### Фактический Decision Gate версии 9.4

CE-hit:

```cpp
bool ce_hit_long  = buyCount  > 0 && ce_stop_price > 0.0 && checkLow  <= ce_stop_price;
bool ce_hit_short = sellCount > 0 && ce_stop_price > 0.0 && checkHigh >= ce_stop_price;
bool ce_hit = ce_hit_long || ce_hit_short;
```

Flip-on:

```cpp
bool flip_on = false;
if(prev_long > 0.0 && prev_short > 0.0) {
    int new_dir = prev_dir_old;
    if(price_close_ce_old > prev_short) new_dir = 1;
    else if(price_close_ce_old < prev_long) new_dir = -1;

    if(buyCount > 0 && new_dir == -1 && prev_dir_old == 1) flip_on = true;
    if(sellCount > 0 && new_dir == 1 && prev_dir_old == -1) flip_on = true;
}
```

Conflict:

```cpp
if(ce_hit && flip_on) {
    closeByConflict++;
}
```

### Текущая проблема архитектуры

CE-hit использует `High/Low[1]`, а Flip использует `Close[1]`.

Поэтому:
- CE ловит хвост;
- Flip ждёт закрытие;
- CE почти всегда выигрывает;
- Flip исчезает из статистики.

### Изоляция входов и выходов

После Decision Gate:

- если позиция открыта, входы блокируются;
- входы выполняются только если `buyCount == 0 && sellCount == 0`.

### Режимы входа

EntryMode влияет на то, разрешён ли повторный вход:
- `Legacy`: всё разрешено;
- `Flip only`: вход только при смене CE относительно `g_lastFlatCeDir`;
- `Persistent`: используется `pendingSignal`;
- `Hybrid`: после стопа разрешает восстановление, иначе требует смену CE.

## Боты и стратегии

### TiTan KRONOS

- Техническое название: `TiTan KRONOS (CE Adaptive Strategy)`.
- Человекочитаемое название: TiTan KRONOS.
- Имя титана: KRONOS.
- Платформа: MT5 / MQL5.
- Источник сравнения: Pine Script / TradingView.
- Стратегия: CE + ZLSMA + Heikin-Ashi.
- Статус: актуальная исследуемая версия.
- Версии в чате:
  - `CE 9.1`;
  - `CE 9.2`;
  - `CE 9.4`.
- Основная проблема версии 9.4: доминирование CE Stop и исчезновение CE Flip.

### CE v7.2 Light

- Платформа: Pine Script / TradingView.
- Роль: эталонная логика для переноса.
- Ключевые параметры:
  - `atrLength = 1`;
  - `atrMult = 2.0`;
  - `process_orders_on_close = true`;
  - `calc_on_every_tick = false`;
  - `pyramiding = 1`.
- Отдельного независимого трейлинга нет.

## Формулы и параметры

### CE ATR

```cpp
atr_orig = ceAtrMult * ATR(ceAtrLength)
```

Смысл: дистанция CE-стопа от экстремума.

### Long CE raw

```cpp
longStopRaw = priceHighRef - atr_orig
```

### Short CE raw

```cpp
shortStopRaw = priceLowRef + atr_orig
```

### CE direction

```cpp
if(price_close_ce > prev_short) ce_dir_orig = 1;
else if(price_close_ce < prev_long) ce_dir_orig = -1;
```

### CE-hit текущей версии

```cpp
LONG:  checkLow  <= ce_stop_price
SHORT: checkHigh >= ce_stop_price
```

Где:
- `checkLow = Low[1]` или HA Low[1];
- `checkHigh = High[1]` или HA High[1].

### Предлагаемый close-only CE-hit

```cpp
LONG:  Close[1] <= ce_stop_price
SHORT: Close[1] >= ce_stop_price
```

Смысл: проверить, оживит ли это `CE Flip`.

### Flip-on

```cpp
LONG position:  new_dir == -1 && prev_dir_old == 1
SHORT position: new_dir == 1 && prev_dir_old == -1
```

### ZLSMA

```cpp
ZLSMA = 2 * linreg(price, len, 0) - linreg(price, len, 1)
```

### Adaptive ZLSMA

```cpp
delta = atrFast / atrSlow - 1.0
adaptiveFactor = 1.0 + zlsmaAdaptiveSpeed * delta * 0.1
adaptiveFactor = clamp(adaptiveFactor, 0.3, 5.0)
target_len = round(zlsmaLength * adaptiveFactor)
```

### Trail

Long trail init:

```cpp
trailLong = MyLow(barShift) - atr_value - offset
```

Short trail init:

```cpp
trailShort = MyHigh(barShift) + atr_value + offset
```

Long trail update:

```cpp
candidate = price_close - atr_value - offset
trailLong = max(trailLong, candidate)
```

Short trail update:

```cpp
candidate = price_close + atr_value + offset
trailShort = min(trailShort, candidate)
```

### Прогрессивный лот

Типы:

```cpp
ProgLot_Type = 0 // OFF
ProgLot_Type = 1 // LINEAR
ProgLot_Type = 2 // MARTINGALE
```

Linear:

```cpp
lot = lot_size + negativeTradesCounter * lot_size
```

Martingale:

```cpp
lot = lot_size * pow(2.0, negativeTradesCounter)
```

Ограничение:

```cpp
if ProgLot_MaxIncreases > 0:
    effectiveCounter = min(negativeTradesCounter, ProgLot_MaxIncreases)
```

### Расписание

Weekend:

```cpp
Friday >= 19:00 => pause
Saturday => pause
Sunday => pause
```

Night:

```cpp
23:50–01:10 => pause
```

Если `reloadAtWeekend` или `reloadAtNight` включены, позиция закрывается по `SCHEDULE`.

## Что проверить дальше

1. Сделать режим `CE close-only`:
   - заменить `Low[1]/High[1]` на `Close[1]` в CE-hit;
   - сравнить `CE Stop`, `CE Flip`, PnL и duration.

2. Логировать `ce_hit`, `flip_on`, `ce_hit && flip_on`:
   - сколько раз Flip был, но проиграл CE;
   - сколько раз Flip вообще появляется.

3. Сравнить старую и новую версии по:
   - средняя длительность сделки;
   - максимальная длительность сделки;
   - bars in trade;
   - средний PnL на сделку.

4. Прогнать stopMode:
   - текущий `0`;
   - текущий `1`;
   - текущий `2`;
   - текущий `3`;
   - экспериментальный `NO_CE_NO_FLIP`;
   - экспериментальный `CE_HYSTERESIS`.

5. Проверить влияние `ceAtrLength` и `ceAtrMult` отдельно:
   - `ceAtrLength = 1, 2, 4`;
   - `ceAtrMult = 2.0, 2.5, 3.0, 5.0`.

6. Проверить `stopOffsetPercent` только после того, как `Trail Stop > 0%`.

7. Проверить, не запаздывает ли HA:
   - `MyClose(1)`;
   - online HA(0);
   - `_calc_ha_bar1`.

8. Разделить тесты:
   - bar-only;
   - hybrid with intrabar stop;
   - close-only stop.

## Что НЕ переносить в wiki

- Личные данные и контакты из авторского блока.
- Copyright/правовые предупреждения.
- Эмоциональные оценки и ругань.
- Бытовые темы.
- Непроверенные утверждения без связи с кодом.
- Сырые полные логи, кроме ключевых чисел.
- Рекламные описания стратегии.

## Цитаты или фрагменты

Ключевой фрагмент, объясняющий доминирование CE:

```cpp
bool ce_hit_long  = buyCount  > 0 && ce_stop_price > 0.0 && checkLow  <= ce_stop_price;
bool ce_hit_short = sellCount > 0 && ce_stop_price > 0.0 && checkHigh >= ce_stop_price;
```

Ключевой фрагмент Flip:

```cpp
if(price_close_ce_old > prev_short) new_dir = 1;
else if(price_close_ce_old < prev_long) new_dir = -1;
```

Ключевой вывод:

```text
CE-hit проверяется по High/Low[1], а Flip — по Close[1].
Поэтому CE ловит хвост свечи раньше, чем close формирует смену направления.
```

## Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да.
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да.
- Нет общих фраз вместо точных данных: да.
- Все новые гипотезы выписаны: да.
- Все новые факты выписаны: да.
- Все ошибки и решения выписаны: да.
- Все правила и архитектурные решения выписаны: да.
- Если что-то не выписано, объясни почему: не выписаны личные контакты, полные copyright-блоки и эмоциональные реплики, потому что они не нужны для инженерной wiki.
