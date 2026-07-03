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
priority_score: 4
---

## Оценка ценности

4/10.

# News Bot - Правила Backtrader Bracket

Правила для прототипа News Bot / News Bracket Bot на Python/Backtrader.

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

## Entry-ордера

Для входа по пробою использовать `bt.Order.Stop`.

Для текущей логики новостной скобки не использовать `bt.Order.Limit` как entry:

- BUY entry выше текущей цены - это пробой вверх;
- SELL entry ниже текущей цены - это пробой вниз.

`Limit` подходит для отката, но не для этой схемы.

## Несколько entry

Если `order_count > 1`, однонаправленные entry с разными TP не являются ошибочными дублями.

Пример:

- 3 BUY Stop по одной цене;
- каждый объемом `0.01`;
- TP разные: `+1`, `+2`, `+3` шага.

После исполнения первого BUY не отменять остальные BUY-entry, если они специально поставлены для разных TP.

То же правило для SELL: после исполнения первого SELL не отменять остальные SELL-entry, если они заранее поставлены как части одной стороны с разными тейками.

## Отмена противоположной стороны

При исполнении BUY-entry:

- отменить все SELL-entry;
- свои BUY-entry не отменять.

При исполнении SELL-entry:

- отменить все BUY-entry;
- свои SELL-entry не отменять.

## Хранение ссылок

Минимально хранить только parent/entry:

- `self.buy_entries`;
- `self.sell_entries`.

Не создавать массивы всех TP/SL без необходимости.

Если используется `buy_bracket()` / `sell_bracket()`, Backtrader должен сам связывать entry с дочерними TP/SL.

## Инициализация состояния

В `__init__` обязательно создавать все поля, которые потом используются в:

- `nextstart()`;
- `next()`;
- `notify_order()`.

Пример:

```python
self.buy_entries = []
self.sell_entries = []
self.initialized = False
```

Или использовать один согласованный флаг, например `self.grid_done`.

## Округление цен

Все цены ордеров округлять до tick size инструмента.

В источнике для `XAUUSD.PRO` использовался tick size `0.01`, поэтому применялось:

```python
round(..., 2)
```

Это правило нельзя автоматически переносить на все рынки: для другого tick size нужно другое округление.

Важно: `round(..., 2)` - это не универсальное правило, а конкретное упрощение для инструмента с шагом цены `0.01`.

## Временной фильтр

Временной фильтр должен работать в торговой таймзоне, а не слепо по UTC.

В источнике использовалась Berlin timezone:

```python
hhmm = bar_berlin.hour * 60 + bar_berlin.minute

skip_weekend = self.p.weekend_pause and bar_berlin.weekday() >= 5
night_interval = hhmm >= 22*60 + 50 or hhmm <= 10
```

Ночная пауза:

- с `22:50` Berlin;
- до `00:10` Berlin.

## Логирование

Логи должны показывать:

- какой entry создан;
- side;
- entry price;
- TP;
- SL;
- ref entry;
- ref TP;
- ref SL;
- какой entry исполнился;
- какие противоположные entry отменяются.

Отдельно полезно логировать, что после отмены parent/entry были отменены связанные дочерние TP/SL. Это подтверждает, что bracket-связка работает штатно.

## Связи

- [News Bot](../../../News%20Bot.md)
- [News Bot - Bracket Architecture](./News%20Bot%20-%20Bracket%20Architecture.md)
- Backtrader Limit вместо Stop в новостной скобке

## RAG/Codex guard

- Non-current: карточка имеет status: draft.
- Использовать только как внутренний черновой context.
- Нельзя выдавать как актуальное техническое правило или current architecture.


## Что проверить

needs_review
