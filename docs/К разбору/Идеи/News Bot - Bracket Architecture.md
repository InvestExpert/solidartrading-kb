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
  - architecture
  - backtrader
priority_score: 4
---

## Оценка ценности

4/10.

# News Bot - Bracket Architecture

Архитектура описывает прототип News Bot / News Bracket Bot: ручную новостную скобку на Python/Backtrader.

## Применимо к

- [News Bot](../../../News%20Bot.md)
- Python / Backtrader

## Раздел

- торговая логика;
- архитектура;
- Backtrader.

## Источник

`2026-05-20 - ChatGPT - News Bracket Bot Python.md`

## Смысл стратегии

Бот не анализирует новости и не выбирает направление. Пользователь запускает его вручную перед новостным движением.

После запуска бот:

- берет текущую цену;
- ставит BUY Stop выше цены;
- ставит SELL Stop ниже цены;
- к каждому entry привязывает stop-loss и take-profit через bracket-механизм;
- после исполнения одной стороны отменяет противоположные entry.

## Основные параметры

```python
("step_pct", 0.20)
("lot_size", 0.01)
("order_count", 3)
("weekend_pause", True)
```

Смысл:

- `step_pct` - размер шага в процентах от текущей цены.
- `lot_size` - фиксированный объем одного ордера.
- `order_count` - количество entry в каждую сторону.
- `weekend_pause` - запрет торговли в субботу и воскресенье.

## Формула шага

```python
step = round(price * step_pct / 100.0, 2)
```

В тесте из источника:

- инструмент: `XAUUSD.PRO`;
- `symbolId = 781`;
- `leverage = 50.0`;
- `margin = 80000.0`;
- рабочий файл в чате: `News 0 1.py`;
- шаг: `step = 6.69`;
- tick size: `0.01`, поэтому применялось `round(..., 2)`.

## BUY-уровни

```python
buy_entry = round(price + 2 * step, 2)
buy_tp_i = round(buy_entry + (i + 1) * step, 2)
buy_sl = round(buy_entry - step, 2)
```

Пример из теста:

- BUY entry: `3358.46`;
- BUY stop-loss: `3351.77`;
- BUY take-profit 1: `3365.15`;
- BUY take-profit 2: `3371.84`;
- BUY take-profit 3: `3378.53`.

## SELL-уровни

```python
sell_entry = round(price - 2 * step, 2)
sell_tp_i = round(sell_entry - (i + 1) * step, 2)
sell_sl = round(sell_entry + step, 2)
```

Пример из теста:

- SELL entry: `3331.70`;
- SELL stop-loss: `3338.39`;
- SELL take-profit 1: `3325.01`;
- SELL take-profit 2: `3318.32`;
- SELL take-profit 3: `3311.63`.

## Хранение ордеров

Выбранный вариант:

- `self.buy_entries`;
- `self.sell_entries`.

Хранить отдельные массивы всех TP/SL не нужно, если используется штатный bracket-механизм Backtrader.

Причина:

- стратегия управляет противоположными entry;
- дочерние TP/SL должны сниматься через bracket-связь после отмены parent/entry.

## Компоненты

- `params` - параметры шага, лота, количества ордеров и фильтра выходных.
- `__init__` - создает состояние стратегии.
- `nextstart` - один раз рассчитывает уровни и выставляет bracket-ордера.
- `notify_order` - реагирует на исполнение entry и отменяет противоположную сторону.
- `_cancel_entries` - отменяет список entry и пишет лог.
- `next` - применяет фильтры времени.
- `bar_to_berlin` - переводит UTC-время бара в Berlin.
- `_last_sunday` - helper для DST.

## Финальная логика cancel

```python
if order in self.buy_entries:
    self.log(f"BUY EXECUTED @{order.executed.price:.2f} (ref={order.ref})")
    self._cancel_entries(self.sell_entries, label="SELL")
    self.sell_entries.clear()

elif order in self.sell_entries:
    self.log(f"SELL EXECUTED @{order.executed.price:.2f} (ref={order.ref})")
    self._cancel_entries(self.buy_entries, label="BUY")
    self.buy_entries.clear()
```

## Тест из источника

Параметры финального теста:

- `symbolName: XAUUSD.PRO`;
- `symbolId = 781`;
- `resolution = 1m`;
- `commission = 0.0`;
- `startDate = 2025-07-15 00:00 UTC`;
- `endDate = 2025-07-16 00:00 UTC`;
- `leverage = 50.0`;
- `margin = 80000.0`;
- sizer: `TLLotSizer`.

Сценарий:

- в `02:00` выставлено `3 BUY + 3 SELL brackets`;
- в `06:00` сработали все 3 BUY Stop;
- SELL-entry были отменены;
- в `06:01` лог показал отмену SELL-entry и связанных дочерних TP/SL;
- одна BUY-часть закрылась по TP;
- две BUY-части закрылись по SL;
- противоположная SELL-ветка не открыла шорт.

В логе после отмены SELL-entry были отменены:

- `ref=4`, `ref=5`, `ref=6`;
- `ref=10`, `ref=11`, `ref=12`;
- `ref=16`, `ref=17`, `ref=18`.

Это подтверждает, что при отмене parent/entry Backtrader снял связанные дочерние TP/SL.

## Что проверить дальше

- Зеркальный сценарий, где первой срабатывает SELL-сторона.
- Поведение на тиковых данных или меньшем таймфрейме.
- Поведение bracket-связок при gap и частичном исполнении.
- Поведение на демо/реальном брокере.
- Нужен ли режим ручного направления: only BUY, only SELL, both.
- Нужен ли timeout: если entry не сработали за N минут, отменить все.

## Связи

- [News Bot](../../../News%20Bot.md)
- [News Bot - Правила Backtrader Bracket](./News%20Bot%20-%20%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0%20Backtrader%20Bracket.md)
- [News Bot - гипотезы новостной скобки](./News%20Bot%20-%20%D0%B3%D0%B8%D0%BF%D0%BE%D1%82%D0%B5%D0%B7%D1%8B%20%D0%BD%D0%BE%D0%B2%D0%BE%D1%81%D1%82%D0%BD%D0%BE%D0%B9%20%D1%81%D0%BA%D0%BE%D0%B1%D0%BA%D0%B8.md)
- Backtrader Limit вместо Stop в новостной скобке

## RAG/Codex guard

- Non-current: карточка имеет status: draft.
- Использовать только как внутренний черновой context.
- Нельзя выдавать как актуальное техническое правило или current architecture.


## Что проверить

needs_review
