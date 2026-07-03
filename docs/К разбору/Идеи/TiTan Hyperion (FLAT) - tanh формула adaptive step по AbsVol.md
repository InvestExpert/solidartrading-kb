---
type: idea
value_score: 5
source: 2026-05-20 - ChatGPT - Формула адаптации шага сетки по AbsVol.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - idea
  - adaptive step
  - AbsVol
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - tanh формула adaptive step по AbsVol

## Суть

Для [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) `tanh`-формула может дать компромисс между двумя задачами:

- сдвигать рабочий шаг между месяцами;
- не ломать прибыльное плато внутри хороших месяцев.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- SmartMode
- adaptive grid step
- AbsVol
- optimization

## Раздел

Гипотеза, SmartMode, adaptive step, volatility formula.

## Источник

`2026-05-20 - ChatGPT - Формула адаптации шага сетки по AbsVol.md`

## Формула-кандидат

```mql5
g_snap.gridStepPerc_smart =
    gridStepPerc * MathExp(
        g_snap.absVolCoef * MathTanh(MathLog(g_snap.absVol / SmartPivotValue))
    );
```

## Почему это может сработать

Рядом с `SmartPivotValue` формула ведет себя похоже на степенную.

Далеко от pivot реакция насыщается через `tanh`, поэтому шаг не должен слишком резко убегать и разрывать прибыльное плато.

## Рабочие ориентиры

Из обсуждения:

```text
SmartPivotValue около 1.50
gridStepPerc около 0.33
SmartAbsVolCoef: 0.30 / 0.33 / 0.35 / 0.37 / 0.40
```

Эти числа не являются production-настройками. Это диапазон для теста.

## Что ожидаем

- Апрель и май не теряют широкое прибыльное плато.
- Январь, февраль и август получают заметный сдвиг шага.
- Март и июль показывают, хватает ли одной оси `AbsVol` или нужна вторая ось вроде `ADX`.

## Как проверить

Для каждого месяца собрать:

```text
AbsVol
итоговый gridStepPerc_smart
PnL
MaxDD
число сессий
V1-V6
```

Сравнивать не коэффициенты, а итоговую карту:

```text
AbsVol -> gridStepPerc_smart -> результат теста
```

## Статус

`needs_test`.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [AbsVol как ось адаптации шага сетки](../../../AbsVol%20%D0%BA%D0%B0%D0%BA%20%D0%BE%D1%81%D1%8C%20%D0%B0%D0%B4%D0%B0%D0%BF%D1%82%D0%B0%D1%86%D0%B8%D0%B8%20%D1%88%D0%B0%D0%B3%D0%B0%20%D1%81%D0%B5%D1%82%D0%BA%D0%B8.md)
- [TiTan Hyperion (FLAT) - adaptive step сохраняет прибыльное плато только с dampening](../../../TiTan%20Hyperion%20(FLAT)%20-%20adaptive%20step%20%D1%81%D0%BE%D1%85%D1%80%D0%B0%D0%BD%D1%8F%D0%B5%D1%82%20%D0%BF%D1%80%D0%B8%D0%B1%D1%8B%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5%20%D0%BF%D0%BB%D0%B0%D1%82%D0%BE%20%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE%20%D1%81%20dampening.md)
- [TiTan Hyperion (FLAT) - одной оси AbsVol недостаточно для всех месяцев](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%BE%D0%B4%D0%BD%D0%BE%D0%B9%20%D0%BE%D1%81%D0%B8%20AbsVol%20%D0%BD%D0%B5%D0%B4%D0%BE%D1%81%D1%82%D0%B0%D1%82%D0%BE%D1%87%D0%BD%D0%BE%20%D0%B4%D0%BB%D1%8F%20%D0%B2%D1%81%D0%B5%D1%85%20%D0%BC%D0%B5%D1%81%D1%8F%D1%86%D0%B5%D0%B2.md)
- [TiTan Hyperion (FLAT) - тестировать adaptive step отдельно от буфера](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D1%82%D0%B5%D1%81%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C%20adaptive%20step%20%D0%BE%D1%82%D0%B4%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%20%D0%BE%D1%82%20%D0%B1%D1%83%D1%84%D0%B5%D1%80%D0%B0.md)


## Что проверить

needs_review
