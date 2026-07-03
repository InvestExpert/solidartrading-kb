---
type: idea
source: 2026-05-20 - ChatGPT - CE offset и ATR на M1-M5.md
applies_to:
  - TiTan Kronos (CE)
  - MT5
section:
  - architecture
  - CE
  - noise protection
priority_score: 4
---

## Оценка ценности

4/10.

# TiTan Kronos (CE) - TiTan Kronos (CE) - CE offset как anti-noise layer

## Суть

В [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) offset на M1-M5 нужно рассматривать не как косметику и не как скрытую часть CE core, а как отдельный anti-noise слой.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- MT5 / MQL5
- CE + ZLSMA
- M1-M5

## Раздел

Архитектура, CE, low timeframe noise, offset.

## Источник

`2026-05-20 - ChatGPT - CE offset и ATR на M1-M5.md`

## Почему

На M1-M5 CE без offset может слишком активно реагировать на:

- spread;
- tick jitter;
- HA noise;
- микротени свечей;
- ложные CE flip.

`ATR(1)` на M1 фактически равен диапазону одной свечи, поэтому CE может стать шумовым индикатором.

## Слои

Рекомендуемая трактовка:

1. CE core:
   - ATR;
   - экстремумы;
   - направление.
2. Noise protection:
   - explicit offset;
   - adaptive ATR;
   - adaptive filters.
3. Confirmation:
   - ZLSMA;
   - HA;
   - persistent signal.

## Профили для проверки

| Режим | ATR | Mult | Назначение |
|---|---:|---:|---|
| Fast | 2 | 1.8 | быстрый CE, низкий spread |
| Balanced | 2 | 2.0 | универсальный M1-M5 |
| Slow | 3 | 2.5 | шумный XAUUSD, новости |

## Идеи для теста

Adaptive offset:

```cpp
double stopOffsetPercent = (Period() <= PERIOD_M5) ? 0.10 : 0.0;
```

ATR-dependent offset:

```cpp
offset = MathMax(0.0002, 0.1 * curr_atr);
```

Adaptive ATR length:

```text
ATRlength = clamp(1 + round(VI * 2), 1, 3)
```

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - CE без hidden offset](../../../TiTan%20Kronos%20(CE)%20-%20CE%20%D0%B1%D0%B5%D0%B7%20hidden%20offset.md)
- [TiTan Kronos (CE) - ATR1 допустим только при контроле шума](../../../TiTan%20Kronos%20(CE)%20-%20ATR1%20%D0%B4%D0%BE%D0%BF%D1%83%D1%81%D1%82%D0%B8%D0%BC%20%D1%82%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE%20%D0%BF%D1%80%D0%B8%20%D0%BA%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D0%B5%20%D1%88%D1%83%D0%BC%D0%B0.md)
- [TiTan Kronos (CE) - ATR1 резонанс с короткой ZLSMA](./TiTan%20Kronos%20(CE)%20-%20ATR1%20%D1%80%D0%B5%D0%B7%D0%BE%D0%BD%D0%B0%D0%BD%D1%81%20%D1%81%20%D0%BA%D0%BE%D1%80%D0%BE%D1%82%D0%BA%D0%BE%D0%B9%20ZLSMA.md)

## RAG/Codex guard

- Non-current: карточка имеет status: needs_test.
- Это гипотеза до code-test/backtest.
- Нельзя выдавать как проверенный технический вывод, current formula или рабочее правило без результата теста.


## Что проверить

needs_review
