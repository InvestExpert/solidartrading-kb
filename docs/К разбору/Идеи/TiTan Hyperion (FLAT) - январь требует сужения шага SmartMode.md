---
type: idea
value_score: 6
source: flat_smartmode_v2_plan.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
section:
  - idea
  - adaptive step
  - monthly regimes
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - январь требует сужения шага SmartMode

## Суть

Январь для [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) выглядит как аномальный месяц, где сетке выгоден меньший шаг, чем большинству остальных месяцев.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- SmartMode
- adaptive grid step
- monthly backtests

## Источник

`flat_smartmode_v2_plan.md`

## Наблюдение

Для января оптимальный шаг:

```text
0.23-0.27
```

Для большинства остальных месяцев:

```text
около 0.33
```

Это означает:

- универсальный фиксированный шаг не работает;
- SmartMode должен уметь уменьшать шаг;
- режима "только расширение" недостаточно.

## Рабочая интерпретация

Январь не просто волатильный.

Он:

- часто дает локальные ускорения;
- но сетке выгоден маленький шаг;
- рынок чаще возвращается.

Это ближе к:

```text
mean reversion regime
```

а не к runaway trend.

## Что ожидаем

Если SmartMode v2 правильно определяет январский режим, он должен:

- сужать шаг в подходящих фазах;
- переводить часть V1 в V3/V4/V6;
- не ломать месяцы, где базовый шаг `0.33` уже стабилен.

## Как проверить

1. Зафиксировать базу `0.33`.
2. Сравнить январь на шагах `0.23`, `0.27`, `0.33`.
3. Проверить симметричный SmartMode с 3 ступенями вниз и 3 ступенями вверх.
4. Сначала менять только step.
5. Не смешивать step, buffer и trailing в одном тесте.
6. После января проверить февраль, март и multi-month.

## Статус

Нужна проверка на месячных и multi-month backtests.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - SmartMode v2 symmetric step](./TiTan%20Hyperion%20(FLAT)%20-%20SmartMode%20v2%20symmetric%20step.md)
- [TiTan Hyperion (FLAT) - SmartMode должен стабилизировать распределение V1-V6](../../../TiTan%20Hyperion%20(FLAT)%20-%20SmartMode%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D1%81%D1%82%D0%B0%D0%B1%D0%B8%D0%BB%D0%B8%D0%B7%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C%20%D1%80%D0%B0%D1%81%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5%20V1-V6.md)
- [TiTan Hyperion (FLAT) - тестировать adaptive step отдельно от буфера](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D1%82%D0%B5%D1%81%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C%20adaptive%20step%20%D0%BE%D1%82%D0%B4%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%20%D0%BE%D1%82%20%D0%B1%D1%83%D1%84%D0%B5%D1%80%D0%B0.md)
- [TiTan Hyperion (FLAT) - одной оси AbsVol недостаточно для всех месяцев](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B9%20%D0%BE%D1%81%D0%B8%20AbsVol%20%D0%BD%D0%B5%D0%B4%D0%BE%D1%81%D1%82%D0%B0%D1%82%D0%BE%D1%87%D0%BD%D0%BE%20%D0%B4%D0%BB%D1%8F%20%D0%B2%D1%81%D0%B5%D1%85%20%D0%BC%D0%B5%D1%81%D1%8F%D1%86%D0%B5%D0%B2.md)


## Что проверить

needs_review
