---
type: idea
source: 2026-05-20 - ChatGPT - minimum-protection-buffer-formula.md
applies_to:
  - TiTan Hyperion (FLAT)
  - TiTan Atlas (BA)
  - hedge grid bots
section:
  - architecture
  - risk management
  - adaptive buffer
priority_score: needs_scoring
---

## Оценка ценности

needs_review

# Минимальный защитный buffer для hedge-сетки

## Суть

Для hedge-сетки минимальный защитный buffer можно считать как математический baseline, а не подбирать только optimizer-ом.

Идея: убыток одной стороны сетки компенсируется частичным закрытием прибыльной противоположной корзины.

## Применимо к

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Atlas (BA)](../../../TiTan%20Atlas%20(BA).md)
- hedge grid bots
- adaptive protection
- RiskGuard / Force Exit logic

## Раздел

Архитектура, риск-менеджмент, hedge compensation, adaptive buffer.

## Источник

`2026-05-20 - ChatGPT - minimum-protection-buffer-formula.md`

## Базовая формула

```text
b_min = g * k / (2 * alpha)
```

Где:

- `g` - шаг сетки;
- `k` - глубина;
- `alpha` - доля hedge-компенсации.

## Главный вывод

Buffer имеет математическую нижнюю границу.

Optimizer может находить прибыльные варианты ниже этой границы на отдельных участках, но такие варианты могут быть нестабильными вне тестового периода.

## Теоретический и практический buffer

Нужно разделять:

- theoretical minimum buffer;
- practical safe buffer.

Практический safe buffer должен учитывать:

- комиссии;
- spread;
- slippage;
- progressive lot;
- режим рынка;
- кризисные участки.

## Где применять

Как универсальный baseline для:

- adaptive protection;
- anti-crisis logic;
- RiskGuard;
- force exit;
- настройки hedge-сеток.

## Связи

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [TiTan Atlas (BA)](../../../TiTan%20Atlas%20(BA).md)
- [Антикризисный протокол сеточных ботов](../../../%D0%90%D0%BD%D1%82%D0%B8%D0%BA%D1%80%D0%B8%D0%B7%D0%B8%D1%81%D0%BD%D1%8B%D0%B9%20%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D1%85%20%D0%B1%D0%BE%D1%82%D0%BE%D0%B2.md)
- [Сначала защита потом масштабирование](../../../%D0%A1%D0%BD%D0%B0%D1%87%D0%B0%D0%BB%D0%B0%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%BF%D0%BE%D1%82%D0%BE%D0%BC%20%D0%BC%D0%B0%D1%81%D1%88%D1%82%D0%B0%D0%B1%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5.md)
- [TiTan Hyperion (FLAT) - теоретический buffer не равен production buffer](../../../TiTan%20Hyperion%20(FLAT)%20-%20%D1%82%D0%B5%D0%BE%D1%80%D0%B5%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9%20buffer%20%D0%BD%D0%B5%20%D1%80%D0%B0%D0%B2%D0%B5%D0%BD%20production%20buffer.md)
- [TiTan Hyperion (FLAT) - buffer сначала baseline потом optimizer](../../../TiTan%20Hyperion%20(FLAT)%20-%20buffer%20%D1%81%D0%BD%D0%B0%D1%87%D0%B0%D0%BB%D0%B0%20baseline%20%D0%BF%D0%BE%D1%82%D0%BE%D0%BC%20optimizer.md)

## RAG/Codex guard

- Non-current: карточка имеет status: needs_test.
- Это гипотеза до code-test/backtest.
- Нельзя выдавать как проверенный технический вывод, current formula или рабочее правило без результата теста.


## Что проверить

needs_review
