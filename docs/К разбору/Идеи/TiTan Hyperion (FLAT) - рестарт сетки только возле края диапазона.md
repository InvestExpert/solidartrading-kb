---
type: idea
value_score: 5
source: 2026-05-20 - ChatGPT - FLAT SmartMode и adaptive grid architecture.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - idea
  - restart logic
  - grid logic
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - рестарт сетки только возле края диапазона

## Суть

В [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) рестарт сетки может быть безопаснее, если разрешать его только возле краев рабочего диапазона.

Идея: не разрушать прибыльную центральную фазу сетки и перезапускать цикл только после достаточного смещения цены.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- grid restart
- dual-side grid
- SmartMode

## Раздел

Гипотеза, торговая логика, restart logic, риск.

## Источник

`2026-05-20 - ChatGPT - FLAT SmartMode и adaptive grid architecture.md`

## Что ожидаем

Restart-by-edge должен:

- снизить лишние рестарты в центре диапазона;
- сохранить прибыльную фазу сетки;
- уменьшить повторные stop/restart события;
- сделать статистику циклов стабильнее.

## Как проверить

- Сравнить обычный restart и restart-by-edge на тех же месяцах.
- Отдельно посчитать частоту рестартов в центре диапазона и возле края.
- Проверить влияние на V1-V6 классификацию торговых циклов.
- Проверить BUY и SELL стороны отдельно.

## Статус

Новая гипотеза. Требует backtest.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - FlatDUAL SmartMode](../../../TiTan%20Hyperion%20(FLAT)%20-%20FlatDUAL%20SmartMode.md)
- [TiTan Hyperion (FLAT) - BUY SELL капсулы](../../../TiTan%20Hyperion%20(FLAT)%20-%20BUY%20SELL%20%D0%BA%D0%B0%D0%BF%D1%81%D1%83%D0%BB%D1%8B.md)
- [TiTan Hyperion (FLAT) - один шаг сетки за тик](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%BE%D0%B4%D0%B8%D0%BD%20%D1%88%D0%B0%D0%B3%20%D1%81%D0%B5%D1%82%D0%BA%D0%B8%20%D0%B7%D0%B0%20%D1%82%D0%B8%D0%BA.md)


## Что проверить

needs_review
