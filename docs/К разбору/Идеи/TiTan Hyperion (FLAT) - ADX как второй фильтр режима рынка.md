---
type: idea
value_score: 6
source: 2026-05-20 - ChatGPT - FlatDUAL SmartMode architecture and volatility logic.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
section:
  - idea
  - SmartMode
  - volatility filters
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Hyperion (FLAT) - ADX как второй фильтр режима рынка

## Суть

Для [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) одного `VI` может быть недостаточно, чтобы надежно отличать безопасные фазы рынка от трендовых месяцев.

Гипотеза: `ADX` нужен как второе измерение режима рынка рядом с `VI`.

Связанная причина: VI mapping помогает стабилизировать часть месяцев, но если эффект держится только на отдельных периодах, фильтр недостаточно универсален без второго измерения режима.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- SmartMode
- adaptive grid step
- volatility regime detection

## Раздел

Гипотеза, торговая логика, фильтры, риск.

## Источник

`2026-05-20 - ChatGPT - FlatDUAL SmartMode architecture and volatility logic.md`

## Что ожидаем

`VI` показывает изменение волатильности, но может плохо различать:

- волатильный боковик;
- устойчивый тренд;
- squeeze / breakout phase.

`ADX` может помочь отделить трендовые режимы, где сетке нужен более осторожный шаг, пауза или усиленная защита.

Практическая зона для проверки: `ADX > 28-30` вместе с повышенным VI. ADX сам по себе недостаточен без VI и структуры V1-V6.

## Как проверить

- Сравнить SmartMode только по `VI` против SmartMode `VI + ADX`.
- Отдельно проверить трендовые месяцы.
- Проверить, снижает ли `ADX` опасные входы без разрушения прибыльных боковиков.
- Логировать причину режима: `VI`, `ADX`, выбранный коэффициент, результат цикла.
- Проверять долю V6 и V1-V4, а не только итоговый PnL.

## Статус

Новая гипотеза. Требует тестов на наборе месяцев.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - FlatDUAL SmartMode](../../../TiTan%20Hyperion%20(FLAT)%20-%20FlatDUAL%20SmartMode.md)
- [TiTan Hyperion (FLAT) - VI SmartMode mapping](../../../TiTan%20Hyperion%20(FLAT)%20-%20VI%20SmartMode%20mapping.md)
- [TiTan Hyperion (FLAT) - ADX должен молчать в здоровом рынке](../../../TiTan%20Hyperion%20(FLAT)%20-%20ADX%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D1%82%D1%8C%20%D0%B2%20%D0%B7%D0%B4%D0%BE%D1%80%D0%BE%D0%B2%D0%BE%D0%BC%20%D1%80%D1%8B%D0%BD%D0%BA%D0%B5.md)
- TiTan Hyperion (FLAT) - ADX нельзя оценивать отдельно от V1-V6
- [SmartMode должен фиксировать режим на старте сессии](../../../SmartMode%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D1%84%D0%B8%D0%BA%D1%81%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%20%D0%BD%D0%B0%20%D1%81%D1%82%D0%B0%D1%80%D1%82%D0%B5%20%D1%81%D0%B5%D1%81%D1%81%D0%B8%D0%B8.md)


## Что проверить

needs_review
