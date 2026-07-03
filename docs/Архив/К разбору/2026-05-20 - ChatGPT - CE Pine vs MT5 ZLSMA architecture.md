---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: неизвестно
related_bot: CE / CE Light
related_strategy: MT5 / Pine(JS) / ZLSMA
suggested_wiki_targets:
  - Вики/Идеи
  - Вики/Гипотезы
  - Вики/Выводы
  - Вики/Правила
  - Вики/Архитектура
  - Вики/Боты
created_for: TiTan Bots
---

# CE Pine(JS) vs MT5 — ZLSMA и архитектурное соответствие

## Статус обработки

Релевантность: высокая
Уверенность: высокая
Нужна ручная проверка: да
Исходное название чата: неизвестно
Связанный бот: CE / CE Light
Связанная стратегия или платформа: MT5 / Pine(JS) / ZLSMA

## Obsidian links

- [CE Strategy](CE%20Strategy.md)
- [MT5](MT5.md)
- [Pine Script](Pine%20Script.md)
- [ZLSMA](ZLSMA.md)
- [Heikin Ashi](Heikin%20Ashi.md)
- [Moving Averages](Moving%20Averages.md)
- [Cross-platform parity](Cross-platform%20parity.md)

## Краткое резюме

Проведено подробное сравнение логики стратегии CE между Pine(JS) и MT5.

Установлено:
- ядро CE совпадает;
- логика смены направления совпадает;
- логика ZLSMA совпадает;
- adaptive ZLSMA совпадает;
- режимы persistent signal совпадают по смыслу;
- MT5 содержит дополнительные расширения поверх базовой Pine-логики.

Сделан вывод:
- ни одна стандартная MA MT5 не повторяет поведение ZLSMA;
- HMA визуально близка к ZLSMA, но не является стандартной MA MT5;
- LSMA является частью формулы ZLSMA, но не заменяет её;
- переход с ZLSMA на HMA/LSMA не даёт архитектурных преимуществ.

## Точные факты

### Формула ZLSMA

ZLSMA:
2 * linreg(src, len, 0) - linreg(src, len, 1)

### Формула CE stop

longStopRaw:
highest(priceHighRef,1) - atr

shortStopRaw:
lowest(priceLowRef,1) + atr

### Формула направления CE

ce_dir_orig:
- 1 если close > prev_short
- -1 если close < prev_long
- иначе сохраняется предыдущее направление

### Pine(JS) CE параметры

- atrLength = 1
- atrMult = 2.0

### MT5 CE параметры

- ceAtrLength
- ceAtrMult

Для полного соответствия Pine:
- ceAtrLength = 1
- ceAtrMult = 2.0

### Adaptive ZLSMA

Используется:
- atrFast / atrSlow - 1

Клампы:
- 0.3x
- 5x

### Persistent signal

В Pine:
- usePersistentSignal

В MT5:
- EntryMode:
  - 1 = Flip only
  - 2 = Persistent
  - 3 = Hybrid

### stopMode в MT5

- 0 = CE_THEN_FLIP
- 1 = Flip only
- 2 = Trail only
- 3 = ZLSMA + Trail

## Справочники и соответствия

### Стандартные MA MT5

Стандартные:
- SMA
- EMA
- SMMA
- LWMA

Не являются стандартными:
- HMA
- LSMA
- ZLSMA

### Поведение MA относительно ZLSMA

SMA:
- самый большой лаг

SMMA:
- ещё более сглаженная
- ещё больший лаг

EMA:
- быстрее SMA

LWMA:
- быстрее EMA
- но всё ещё запаздывает

ZLSMA:
- минимальный лаг
- реагирует быстрее всех стандартных MA

### HMA

- визуально очень близка к ZLSMA
- требует кастомной реализации
- не даёт преимуществ относительно ZLSMA

### LSMA

- linear regression MA
- часть формулы ZLSMA
- более медленная, чем ZLSMA
- не даёт преимуществ

## Идеи

### Временный режим A/B теста

Флаг:
- 0 = ZLSMA
- 1 = HMA
- 2 = LSMA

Цель:
- сравнить PnL;
- сравнить DD;
- сравнить количество сделок;
- сравнить отличающиеся входы.

## Выводы

### Главный вывод

Базовая логика Pine(JS) и MT5 совпадает.

### По ZLSMA

Ни одна стандартная MT5 MA не повторяет поведение ZLSMA.

### По HMA

HMA:
- визуально близка к ZLSMA;
- не даёт преимуществ;
- требует кастомной реализации так же, как ZLSMA.

### По LSMA

LSMA:
- является только частью ZLSMA;
- не заменяет её полностью.

### Архитектурное решение

Оставить:
- ZLSMA как канон;
- одну реализацию;
- единое поведение между Pine и MT5.

## Правила

### Правило соответствия платформ

При сравнении Pine(JS) и MT5:
- использовать одинаковые CE параметры;
- использовать одинаковый источник цены;
- использовать одинаковую барную логику;
- отключать дополнительные MT5 функции.

### Правило production архитектуры

Не добавлять multiple MA режимы без доказанной пользы.

## Архитектура

### Архитектура CE

Состав:
- CE direction;
- CE stop;
- ZLSMA filter;
- optional persistent signal.

### Архитектура MT5

MT5 содержит:
- базовую Pine совместимую логику;
- дополнительные production расширения.

### Дополнительные MT5 блоки

- independent trailing;
- ZLSMA exit;
- stop priority;
- pause windows;
- progressive lot;
- logging.

### Архитектурное решение

Production:
- одна MA реализация;
- ZLSMA как единый стандарт.

## Что проверить дальше

- сравнить реальные сигналы HMA vs ZLSMA на XAUUSD;
- проверить процент отличающихся входов;
- проверить влияние online HA;
- проверить чувствительность к длине ZLSMA.

## Самопроверка полноты

- Все пункты из "Что сохранено" реально раскрыты ниже: да
- Все упомянутые списки, соответствия, формулы и параметры выписаны явно: да
- Нет общих фраз вместо точных данных: да
- Все новые факты выписаны: да
- Все правила и архитектурные решения выписаны: да
