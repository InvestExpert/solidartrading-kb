---
type: idea
priority_score: 1
source: 2026-02-25 - ChatGPT - Binance BTCUSDT в MT5 для тестов ATLAS.md; owner decision 2026-06-21
created: 2026-05-21
applies_to:
  - TiTan Atlas (BA)
  - MT5
sections:
  - data_quality
  - TickVolume
  - backtest
  - resolved
  - archive_processed
---


## Оценка ценности

1/10.

Статус: `archive_processed / resolved`.

Карточка закрыта как старый технический хвост импорта Binance/crypto-данных в MT5 custom symbol. Отдельного решения владельца или новой проверки не требуется.

## Решение после проверки

Активное правило уже есть: [MT5 custom symbol - TickVolume должен быть больше нуля](../../../../MT5%20custom%20symbol%20-%20TickVolume%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%B1%D1%8B%D1%82%D1%8C%20%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B5%20%D0%BD%D1%83%D0%BB%D1%8F.md).

Смысл правила:

- для MT5 custom symbol `TickVolume` должен быть больше нуля;
- минимально допустимо `TickVolume = 1`;
- это допустимо только если стратегия не использует `Volume`, `tick_volume`, `iVolume`, OBV, MFI, VWAP или volume-фильтры;
- если стратегия начнет использовать объем, нужен реальный proxy-volume, например Binance `number_of_trades`.

Связанный technical context: [ATLAS MT5 - Binance Spot данные через custom symbol](../../../../ATLAS%20MT5%20-%20Binance%20Spot%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D1%87%D0%B5%D1%80%D0%B5%D0%B7%20custom%20symbol.md).

Новую задачу не создавать, пока стратегия не использует объемные признаки.


# ATLAS MT5 - TickVolume 1 не влияет если стратегия не использует объем

## Гипотеза

Если [TiTan Atlas (BA)](../../../../TiTan%20Atlas%20(BA).md) в MT5 не использует объемные индикаторы и фильтры по объему, то `TickVolume = 1` нужен только как техническое условие импорта и не влияет на сделки.

## Применимо к

- [TiTan Atlas (BA)](../../../../TiTan%20Atlas%20(BA).md)
- [TiTan Atlas (BA) - MT5](../../../../TiTan%20Atlas%20(BA)%20-%20MT5.md)
- MT5 custom symbol

## Раздел

- data quality;
- backtest;
- TickVolume.

## Источник

`2026-02-25 - ChatGPT - Binance BTCUSDT в MT5 для тестов ATLAS.md`

## Как проверить

Прогнать одинаковый тест:

- с `TickVolume = 1`;
- с `TickVolume = number_of_trades` из Binance CSV.

Сравнить:

- количество сделок;
- PnL;
- DD;
- точки входа и выхода.

## Риск

Если в коде есть логика по объему, эта гипотеза становится неверной.

## Связи

- [MT5 custom symbol - TickVolume должен быть больше нуля](../../../../MT5%20custom%20symbol%20-%20TickVolume%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%B1%D1%8B%D1%82%D1%8C%20%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B5%20%D0%BD%D1%83%D0%BB%D1%8F.md)
- [ATLAS MT5 - Binance Spot данные через custom symbol](../../../../ATLAS%20MT5%20-%20Binance%20Spot%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D1%87%D0%B5%D1%80%D0%B5%D0%B7%20custom%20symbol.md)



## Что проверить

needs_review
