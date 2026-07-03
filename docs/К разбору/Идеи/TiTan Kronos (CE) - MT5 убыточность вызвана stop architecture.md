---
type: idea
value_score: 5
source: 2026-05-20 - ChatGPT - CE KRONOS stop logic and restart architecture.md
applies_to:
  - TiTan Kronos (CE)
  - MT5
section:
  - idea
  - stop architecture
  - parity
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Kronos (CE) - MT5 убыточность вызвана stop architecture

## Суть

В [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) расхождение MT5 и Pine может быть вызвано не Adaptive ZLSMA, а stop architecture.

MT5 использует гибрид:

- bar logic;
- tick stop execution.

Pine reference ближе к чисто барной модели.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- MT5 / MQL5
- Pine Script reference
- CE + ZLSMA

## Раздел

Гипотеза, stop architecture, MT5/Pine parity.

## Источник

`2026-05-20 - ChatGPT - CE KRONOS stop logic and restart architecture.md`

## Что ожидаем

Если гипотеза верна:

- pure bar stop приблизит MT5 к Pine;
- aggressive tick stop ухудшит M1 XAU;
- delayed trail / larger initial stop снизит ложные выходы;
- adaptive ZLSMA окажется вторичным фактором.

## Как проверить

Сравнить:

- pure bar stop;
- hybrid stop;
- tick stop.

Дополнительно:

- delayed trail activation;
- larger initial stop;
- ATR multiplier только для initial stop;
- stopOffsetPercent.

## Статус

`needs_test`.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - warm restart после паузы](../../../TiTan%20Kronos%20(CE)%20-%20warm%20restart%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D0%BF%D0%B0%D1%83%D0%B7%D1%8B.md)
- [TiTan Kronos (CE) - M1 stop distance должен быть больше шума](../../../TiTan%20Kronos%20(CE)%20-%20M1%20stop%20distance%20%D0%B4%D0%BE%D0%BB%D0%B6%D0%B5%D0%BD%20%D0%B1%D1%8B%D1%82%D1%8C%20%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B5%20%D1%88%D1%83%D0%BC%D0%B0.md)
- [TiTan Kronos (CE) - Adaptive ZLSMA warmup](../../../TiTan%20Kronos%20(CE)%20-%20Adaptive%20ZLSMA%20warmup.md)


## Что проверить

needs_review
