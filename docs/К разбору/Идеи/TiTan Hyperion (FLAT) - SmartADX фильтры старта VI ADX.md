---
type: idea
source: 2026-05-20 - ChatGPT - FLAT SmartMode фильтры старта VI ADX.md
applies_to:
  - TiTan Hyperion (FLAT)
  - MT5
  - MQL5
section:
  - bot
  - SmartMode
  - start filters
priority_score: 4
---

## Оценка ценности

4/10.

# TiTan Hyperion (FLAT) - SmartADX фильтры старта VI ADX

## Суть

SmartADX в [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md) - режим фильтрации старта сетки по `VI` и `ADX`.

Фильтр не знает будущий исход V1-V6. Он только запрещает старт в некоторых рыночных режимах и статистически меняет распределение будущих сессий.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- MT5 / MQL5
- SmartMode
- VI / ADX
- start filters

## Раздел

Бот, SmartMode, фильтры старта, session statistics.

## Источник

`2026-05-20 - ChatGPT - FLAT SmartMode фильтры старта VI ADX.md`

## Режимы SmartADX

Из источника:

```mql5
input int SmartADX = 1; // Smart: фильтрация ADX: 0=выкл, 1-4 блоки, 5=все, 6=2+4, 7=3+4
```

Смысл:

- `0` - без защиты старта;
- `1` - блок 1;
- `2` - блок 2;
- `3` - блок 3;
- `4` - блок 4;
- `5` - все блоки;
- `6` - блоки 2+4;
- `7` - блоки 3+4.

## Кандидаты из текущих тестов

Для дальнейшей проверки оставить:

- блок 4;
- комбинация 2+4;
- комбинация 3+4.

По текущему источнику эти варианты улучшали февраль и апрель, но почти не решали март.

## Что смотреть

Фильтр старта оценивать не по одному PnL, а по матрице:

- totalPnL;
- ExitPNL;
- худший месяц;
- число улучшенных месяцев;
- число ухудшенных месяцев;
- количество сессий;
- распределение V1-V6;
- влияние на долю V6.

## Март как отдельная диагностика

Если март почти не меняется от стартовых фильтров, проблема может быть не во входе.

Проверять глубже:

- шаг сетки;
- ширину сетки;
- buffer;
- trailing;
- логику выхода;
- рыночный режим после старта.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Hyperion (FLAT) - фильтр старта оценивать по распределению исходов](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D1%84%D0%B8%D0%BB%D1%8C%D1%82%D1%80%20%D1%81%D1%82%D0%B0%D1%80%D1%82%D0%B0%20%D0%BE%D1%86%D0%B5%D0%BD%D0%B8%D0%B2%D0%B0%D1%82%D1%8C%20%D0%BF%D0%BE%20%D1%80%D0%B0%D1%81%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D1%8E%20%D0%B8%D1%81%D1%85%D0%BE%D0%B4%D0%BE%D0%B2.md)
- [TiTan Hyperion (FLAT) - ADX как второй фильтр режима рынка](./TiTan%20Hyperion%20(FLAT)%20-%20ADX%20%D0%BA%D0%B0%D0%BA%20%D0%B2%D1%82%D0%BE%D1%80%D0%BE%D0%B9%20%D1%84%D0%B8%D0%BB%D1%8C%D1%82%D1%80%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%D0%B0%20%D1%80%D1%8B%D0%BD%D0%BA%D0%B0.md)
- [TiTan Hyperion (FLAT) - ADX должен молчать в здоровом рынке](../../../TiTan%20Hyperion%20(FLAT)%20-%20ADX%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%BC%D0%BE%D0%BB%D1%87%D0%B0%D1%82%D1%8C%20%D0%B2%20%D0%B7%D0%B4%D0%BE%D1%80%D0%BE%D0%B2%D0%BE%D0%BC%20%D1%80%D1%8B%D0%BD%D0%BA%D0%B5.md)
- TiTan Hyperion (FLAT) - ADX нельзя оценивать отдельно от V1-V6
- [TiTan Hyperion (FLAT) - здоровый месяц сетки это высокий V6 и низкие V1-V4](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D0%B7%D0%B4%D0%BE%D1%80%D0%BE%D0%B2%D1%8B%D0%B9%20%D0%BC%D0%B5%D1%81%D1%8F%D1%86%20%D1%81%D0%B5%D1%82%D0%BA%D0%B8%20%D1%8D%D1%82%D0%BE%20%D0%B2%D1%8B%D1%81%D0%BE%D0%BA%D0%B8%D0%B9%20V6%20%D0%B8%20%D0%BD%D0%B8%D0%B7%D0%BA%D0%B8%D0%B5%20V1-V4.md)

## RAG/Codex guard

- Non-current: карточка имеет status: needs_test.
- Это гипотеза до code-test/backtest.
- Нельзя выдавать как проверенный технический вывод, current formula или рабочее правило без результата теста.


## Что проверить

needs_review
