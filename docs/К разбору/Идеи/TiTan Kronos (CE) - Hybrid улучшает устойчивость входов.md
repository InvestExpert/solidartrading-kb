---
type: idea
value_score: 5
source: 2026-05-20 - ChatGPT - CE EntryMode and Hybrid Logic.md
applies_to:
  - TiTan Kronos (CE)
  - MT5
section:
  - idea
  - EntryMode
  - robustness
priority_score: 4
---


## Оценка ценности

4/10.


# TiTan Kronos (CE) - Hybrid улучшает устойчивость входов

## Суть

В [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md) EntryMode `Hybrid` может быть наиболее устойчивым режимом входа.

Он уменьшает ложные повторные входы через flip-memory, но сохраняет recovery после stop через `pendingSignal`.

## Применимо к

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- MT5 / MQL5
- CE + ZLSMA
- EntryMode

## Раздел

Гипотеза, EntryMode, robustness, recovery.

## Источник

`2026-05-20 - ChatGPT - CE EntryMode and Hybrid Logic.md`

## Что ожидаем

- Меньше overtrading.
- Лучше recovery after stop.
- Более стабильная работа на M5/M15.
- Потенциально хуже на режимах, где нужен частый re-entry.

## Как проверить

Сравнить:

- Legacy;
- Flip only;
- Persistent only;
- Hybrid.

Метрики:

- PnL;
- DD;
- trade count;
- close reasons;
- recovery entries;
- robustness по M1/M5/M15.

## Статус

`needs_test`.

## Связи

- [TiTan Kronos (CE)](../../../TiTan%20Kronos%20(CE).md)
- [TiTan Kronos (CE) - EntryMode и Hybrid Logic](../../../TiTan%20Kronos%20(CE)%20-%20EntryMode%20%D0%B8%20Hybrid%20Logic.md)
- [TiTan Kronos (CE) - pendingSignal survives stop](../../../TiTan%20Kronos%20(CE)%20-%20pendingSignal%20survives%20stop.md)


## Что проверить

needs_review
