---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: FlatDUAL / SmartMode / squeeze / статистика
related_bot: FLAT
related_strategy: MT5 / MQL5 / Grid Strategy
suggested_wiki_targets:
  - Вики/Идеи
  - Вики/Гипотезы
  - Вики/Выводы
  - Вики/Ошибки
  - Вики/Правила
  - Вики/Архитектура
  - Вики/Боты
created_for: TiTan Bots
---

# FlatDUAL SmartMode architecture and volatility logic

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  
Исходное название чата: FlatDUAL / SmartMode / squeeze / статистика  
Связанный бот: FLAT / FlatDUAL  
Связанная стратегия или платформа: MT5 / MQL5 / Grid Strategy  

Что сохранено:
- архитектура FlatDUAL;
- SmartMode;
- volatility regimes;
- adaptive grid step;
- squeeze protection;
- session statistics;
- restart logic;
- BUY/SELL capsule architecture;
- anti-flat logic;
- session logging;
- forced exit handling;
- guard stop system;
- V1–V6 classification;
- adaptive ATR/ADX logic.

Что пропущено:
- личные комментарии;
- эмоциональные реплики;
- косметические версии без технической сути;
- production-runtime детали без архитектурной ценности.

Риски приватности:
- присутствуют magic numbers;
- внутренние параметры стратегии;
- runtime-параметры MT5.

Предложенное имя файла:
2026-05-20 - ChatGPT - FlatDUAL SmartMode architecture and volatility logic.md

## Obsidian links

- [FLAT](FLAT.md)
- [FlatDUAL](FlatDUAL.md)
- [MT5](MT5.md)
- [MQL5](MQL5.md)
- [Grid Strategy](Grid%20Strategy.md)
- [Volatility Regime Detection](Volatility%20Regime%20Detection.md)
- [Squeeze Protection](Squeeze%20Protection.md)

## Краткое резюме

Чат содержит архитектуру торгового grid-бота FlatDUAL для MT5 с адаптивным SmartMode, системой volatility-regime detection, защитой от squeeze-движений, статистикой поведения торговых циклов и системой динамического изменения шага сетки.

Основной упор сделан на:
- адаптацию шага сетки под режим рынка;
- защиту депозита;
- фильтрацию опасных фаз рынка;
- классификацию торговых исходов;
- накопление статистики поведения сетки;
- разделение логики BUY/SELL сторон.

## Идеи

- Использование volatility regimes для динамической адаптации gridStep.
- Использование snapshot режима рынка только при старте сессии.
- Отделение логики SmartMode от базовой стратегии.
- Использование tanh/log dampening для сохранения прибыльных плато.
- Классификация поведения торговых циклов через V1–V6.

## Гипотезы

- Чистого VI недостаточно для универсального адаптивного шага.
- Для трендовых месяцев требуется дополнительный фильтр ADX.
- Формула tanh сохраняет прибыльное плато лучше степенных формул.
- Step является главным управляющим параметром стратегии.

## Выводы

- Формула v4/v5 для adaptive step показала лучшие результаты.
- Pivot-based step adaptation позволяет сохранить прибыльные месяцы.
- Простая степенная адаптация разрушает прибыльное плато.
- SmartMode должен изменять параметры только между сессиями.

## Ошибки и решения

### Ошибка: разрушение прибыльного плато

Решение:
- переход к tanh/log damped formulas.

### Ошибка: squeeze trigger вызывал лишние выходы

Решение:
- добавлен choppy-market filter.

## Правила

- SmartMode не изменяет параметры “на лету”.
- Regime snapshot фиксируется только при старте сессии.
- Adaptive step должен быть квантован.
- Forced exits и trailing exits разделяются.

## Архитектура

### SmartMode architecture

Компоненты:
- VI;
- ADX50;
- AbsVol;
- adaptive step coefficient.

### BUY/SELL capsule architecture

Для каждой стороны отдельно:
- forced boundary;
- trailing start;
- partial profit level;
- BE state;
- guard stops.

### Session statistics architecture

Классы:
- V1;
- V1.5;
- V2;
- V3;
- V4;
- V5;
- V6.

### Squeeze protection architecture

Компоненты:
- ATR ratio;
- squeeze thresholds;
- choppy market detector;
- forced exits.

## Боты и стратегии

### FlatDUAL

Grid-based dual-side strategy:
- BUY + SELL одновременно;
- adaptive step;
- adaptive protection;
- restart cycles;
- anti-flat logic.

## Что проверить дальше

- Проверить работу tanh-модели на полном наборе месяцев.
- Проверить необходимость ADX как второго измерения режима.
- Проверить устойчивость adaptive step на кризисных периодах.

## Что НЕ переносить в wiki

- Личные комментарии.
- Эмоциональные реплики.
- Косметические описания версий.
- Временные тестовые параметры.

## Цитаты или фрагменты

```cpp
gridStepPerc_smart =
    gridStepPerc *
    MathExp(absVolCoef * MathTanh(MathLog(g_snap.absVol / SmartPivotValue)));
```
