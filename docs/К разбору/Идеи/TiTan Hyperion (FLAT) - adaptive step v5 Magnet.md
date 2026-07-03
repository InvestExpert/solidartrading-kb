---
type: idea
source: 2026-05-20 - ChatGPT - FLAT adaptive grid step v5 Magnet.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - bot
  - adaptive step
  - SmartMode
priority_score: 4
---

## Оценка ценности

4/10.

# TiTan Hyperion (FLAT) - adaptive step v5 Magnet

## Суть

`v5 Magnet` - отдельная формула adaptive step для [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md).

Цель формулы - не искать максимальный PnL в одной точке, а расширить устойчивое прибыльное плато вокруг базового `gridStepPerc`, например `0.27`.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- SmartMode
- adaptive grid step
- AbsVol
- оптимизация

## Раздел

Бот, формула, SmartMode, adaptive step.

## Источник

`2026-05-20 - ChatGPT - FLAT adaptive grid step v5 Magnet.md`

## Формула

```mql5
g_snap.gridStepPerc_smart =
    gridStepPerc
    + g_snap.absVolCoef
    * (MathLog(g_snap.absVol / SmartPivotValue)
    / (1.0 + MathAbs(MathLog(g_snap.absVol / SmartPivotValue))));
```

## Параметры

- `gridStepPerc` - базовый шаг сетки.
- `g_snap.absVol` - фактический индекс волатильности для этой формулы.
- `SmartPivotValue` - pivot, около которого поправка близка к нулю.
- `g_snap.absVolCoef` / `SmartAbsVolCoef` - максимальная сила смещения.
- `gridStepPerc_smart` - итоговый шаг сетки.

Важно: в комментариях удобно писать `ln(index/pivot)`, но в текущей формуле используется именно `g_snap.absVol / SmartPivotValue`, а не `VI / pivot`.

## Идея Magnet

Формула использует насыщение:

```text
x / (1 + |x|)
```

Это ограничивает крайние отклонения и не дает шагу агрессивно убегать от базового значения.

## Когда пересчитывать

Пересчет `gridStepPerc_smart` допускается:

- при старте новой сетки;
- однократно после стопа одной стороны, если выходная ветка должна работать с актуальной волатильностью.

Нельзя пересчитывать шаг бесконечно внутри выходной ветки, иначе геометрия сетки начнет дрожать.

## Что проверить

- Базу `0.27` и соседние шаги вокруг нее.
- `SmartPivotValue = 1.20`.
- `SmartAbsVolCoef = 0.02` и `0.03`.
- PnL, MaxDD и распределение V1-V6.
- Ширину плато из 3+ соседних значений.
- Месяцы, которые остаются красными после v5.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - FlatDUAL SmartMode](../../../TiTan%20Hyperion%20(FLAT)%20-%20FlatDUAL%20SmartMode.md)
- [TiTan Hyperion (FLAT) - плато важнее пика PnL](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%BF%D0%BB%D0%B0%D1%82%D0%BE%20%D0%B2%D0%B0%D0%B6%D0%BD%D0%B5%D0%B5%20%D0%BF%D0%B8%D0%BA%D0%B0%20PnL.md)
- [TiTan Hyperion (FLAT) - v5 Magnet расширит плато вокруг базового шага](./TiTan%20Hyperion%20(FLAT)%20-%20v5%20Magnet%20%D1%80%D0%B0%D1%81%D1%88%D0%B8%D1%80%D0%B8%D1%82%20%D0%BF%D0%BB%D0%B0%D1%82%D0%BE%20%D0%B2%D0%BE%D0%BA%D1%80%D1%83%D0%B3%20%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D0%BE%D0%B3%D0%BE%20%D1%88%D0%B0%D0%B3%D0%B0.md)
- [TiTan Hyperion (FLAT) - adaptive step сохраняет прибыльное плато только с dampening](../../../TiTan%20Hyperion%20(FLAT)%20-%20adaptive%20step%20%D1%81%D0%BE%D1%85%D1%80%D0%B0%D0%BD%D1%8F%D0%B5%D1%82%20%D0%BF%D1%80%D0%B8%D0%B1%D1%8B%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5%20%D0%BF%D0%BB%D0%B0%D1%82%D0%BE%20%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE%20%D1%81%20dampening.md)
- TiTan Hyperion (FLAT) - риск двойной квантизации шага
- TiTan Hyperion (FLAT) - случайное умножение на coef при квантизации

## RAG/Codex guard

- Non-current: карточка имеет status: needs_test.
- Это гипотеза до code-test/backtest.
- Нельзя выдавать как проверенный технический вывод, current formula или рабочее правило без результата теста.
