---
type: idea
source: 2026-05-20 - ChatGPT - News Bracket Bot Python.md
created: 2026-05-20
bot: News Bot
platforms:
  - Python
  - Backtrader
applies_to:
  - News Bot
sections:
  - trading_logic
  - execution
  - backtrader
  - future
  - later
  - news_bot_backlog
priority_score: 4
---


## Оценка ценности

4/10.

Статус: `future / later / News Bot backlog`.

Карточку не закрывать как мусор и не переносить в архив. Новостная скобка - официальный будущий метод для News Bot, но сейчас News Bot не находится в активной разработке. Вернуться к реализации после возврата к News Bot.

## Решение владельца

News Bot должен работать на прорыве в определенное время. Гипотезы по bracket-скобке сохранить как будущий технический backlog.

Подтверждено:

- entry для новостного пробоя должен быть `Stop`, а не `Limit`;
- несколько entry на одной цене с разными TP являются допустимой архитектурной схемой, а не ошибочным дублем;
- при отмене противоположного parent/entry в простом BUY-first тесте Backtrader отменил связанные дочерние TP/SL.

Оставить на будущую проверку:

- сравнить несколько entry с разными TP против одного entry с частичными тейками;
- проверить отмену противоположных entry и связанных bracket-ордеров на gap;
- проверить частичные исполнения;
- обязательно проверить зеркальный SELL-first сценарий;
- проверить поведение на тиках / меньшем таймфрейме и на демо/реальном брокере.


# News Bot - гипотезы новостной скобки

Гипотезы по прототипу News Bot / News Bracket Bot.

## Применимо к

- [News Bot](../../../News%20Bot.md)
- Python / Backtrader

## Раздел

- торговая логика;
- execution;
- Backtrader;
- order types.

## Источник

`2026-05-20 - ChatGPT - News Bracket Bot Python.md`

## Гипотеза 1: несколько entry с разными TP удобнее одного entry с частичными тейками

Суть:

- поставить несколько BUY Stop на одной цене;
- каждому назначить свой TP;
- аналогично для SELL Stop.

Пример:

- `order_count = 3`;
- общий long при срабатывании BUY: `0.03 lot`;
- каждый entry имеет объем `0.01`;
- TP: `+1`, `+2`, `+3` шага.

Чем проверить:

- прогнать бэктест на нескольких новостных периодах;
- сравнить с вариантом одного entry объемом `order_count * lot_size` и частичными тейками.

Статус:

- частично проверена на одном тесте `XAUUSD.PRO` за `2025-07-15`;
- требует дальнейшей проверки.

## Гипотеза 2: для новостного пробоя entry должен быть Stop, а не Limit

Суть:

- BUY выше текущей цены - пробой вверх;
- SELL ниже текущей цены - пробой вниз;
- значит entry должен быть `bt.Order.Stop`.

Чем проверить:

- сравнить логи Limit-entry и Stop-entry на 1m данных;
- проверить отсутствие мгновенных ложных исполнений внутри OHLC-бара.

Статус:

- подтверждена на текущих логах и вынесена в активное правило [Backtrader пробойный entry должен быть Stop](../../../Backtrader%20%D0%BF%D1%80%D0%BE%D0%B1%D0%BE%D0%B9%D0%BD%D1%8B%D0%B9%20entry%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%B1%D1%8B%D1%82%D1%8C%20Stop.md);
- Limit-entry давал искажение;
- Stop-entry стал исполняться только при достижении уровня.

## Гипотеза 3: достаточно отменять только противоположные entry

Суть:

- если используется `buy_bracket()` / `sell_bracket()`, отмена parent/entry должна отменить связанные дочерние TP/SL.

Чем проверить:

- по логам убедиться, что после отмены entry дочерние TP/SL получают статус canceled;
- проверить, что дочерние ордера не исполняются позже;
- отдельно проверить gap и частичные исполнения.
- обязательно проверить зеркальный сценарий, где первой срабатывает SELL-сторона и отменяются BUY-entry.

Статус:

- частично проверена;
- в логе после отмены SELL-entry были отменены связанные TP/SL;
- требуется отдельный тест на сложных сценариях.
- нужен отдельный зеркальный SELL-first тест.

## Связи

- [News Bot](../../../News%20Bot.md)
- [News Bot - Bracket Architecture](./News%20Bot%20-%20Bracket%20Architecture.md)
- [News Bot - Правила Backtrader Bracket](./News%20Bot%20-%20%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%20Backtrader%20Bracket.md)
- [Backtrader пробойный entry должен быть Stop](../../../Backtrader%20%D0%BF%D1%80%D0%BE%D0%B1%D0%BE%D0%B9%D0%BD%D1%8B%D0%B9%20entry%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%B1%D1%8B%D1%82%D1%8C%20Stop.md)
- Backtrader Limit вместо Stop в новостной скобке


## Что проверить

- future/later: multi-entry vs partial TP;
- future/later: gap scenarios;
- future/later: partial fills;
- future/later: SELL-first mirror scenario;
- future/later: broker/demo behavior.
