# CE + ZLSMA: архитектура трейлинга, stopMode и исправление логики MT5

Файл подготовлен автоматически для импорта в Obsidian Wiki TiTan Bots.

Основные темы:
- Decision Gate
- CE stop vs Trail stop
- Adaptive ZLSMA
- Heikin Ashi online/closed
- stopMode / EntryMode
- parity PineScript vs MT5
- ошибки трейлинга
- double offset bug
- мгновенная инициализация трейлинга
- правила bar execution

## stopMode

- 0 = CE_THEN_FLIP
- 1 = FLIP_ONLY
- 2 = TRAIL
- 3 = ZLSMA_TRAIL

## EntryMode

- 0 = Legacy
- 1 = Flip only
- 2 = Persistent only
- 3 = Hybrid

## Adaptive ZLSMA

delta = atr_fast / atr_slow - 1

adaptiveFactor = 1 + speed * delta * 0.1

Clamp:
- 0.3
- 5.0

## Главные выводы

- CE должен оставаться барным.
- CE stop и trail stop нельзя смешивать.
- Offset должен применяться только к трейлингу.
- online HA и closed HA должны быть разделены.
- Decision Gate должен выполняться ДО пересчета CE.
- Persistent signal нельзя сбрасывать после CE STOP.
- Трейлинг должен инициализироваться мгновенно при входе.

## Основные ошибки

### Double offset bug

Причина:
offset применялся дважды.

Решение:
оставить offset только в трейлинге.

### Trail update without hit

Причина:
трейлинг обновлялся, но не инициализировался вовремя.

Решение:
мгновенная _init_trail_stop() при входе.

### HA lag

Причина:
использование stale HA values.

Решение:
online HA для signal-bar.

## Архитектура

Pipeline:
1. Decision Gate
2. HA update
3. CE recalculation
4. ZLSMA recalculation
5. Trail update
6. Entry logic
