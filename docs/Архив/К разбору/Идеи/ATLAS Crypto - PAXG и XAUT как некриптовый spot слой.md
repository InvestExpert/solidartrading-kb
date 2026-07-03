---
type: idea
source: 2026-05-21 - ChatGPT - Хедж к BTC и BingX Spot.md
created: 2026-05-21
applies_to:
  - ATLAS Crypto
  - ATLAS Spot BUY
  - Crypto Python
sections:
  - crypto
  - spot
  - portfolio
  - BingX Spot
  - tokenized_assets
  - archive_processed
  - merged
  - absorbed
priority_score: 1
---


## Оценка ценности

1/10.

Статус: `archive_processed / merged / absorbed`.

Карточка закрыта как отдельная активная идея: смысл PAXG/XAUT сохранен в зонтичной research-карточке [ATLAS Crypto - в поисках хеджа и снижения синхронности просадок](../../../%D0%9A%20%D1%80%D0%B0%D0%B7%D0%B1%D0%BE%D1%80%D1%83/%D0%98%D0%B4%D0%B5%D0%B8/ATLAS%20Crypto%20-%20%D0%B2%20%D0%BF%D0%BE%D0%B8%D1%81%D0%BA%D0%B0%D1%85%20%D1%85%D0%B5%D0%B4%D0%B6%D0%B0%20%D0%B8%20%D1%81%D0%BD%D0%B8%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F%20%D1%81%D0%B8%D0%BD%D1%85%D1%80%D0%BE%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BE%D0%BA.md).

Отдельно держать эту карточку в `К разбору/Идеи` больше не нужно.

# ATLAS Crypto - PAXG и XAUT как некриптовый spot слой

## Гипотеза

`PAXG/USDT` и `XAUT/USDT` могут быть первыми некриптовыми spot-кандидатами для ATLAS Crypto / ATLAS Spot BUY на BingX Spot.

Идея не в том, что золото гарантированно лучше BTC, а в том, что у него другая рыночная логика. Это может снизить совпадение просадок внутри портфеля.

## Применимо к

- [TiTan Atlas (BA)](../../../../TiTan%20Atlas%20(BA).md)
- [TiTan Atlas (BA) - Crypto Python](../../../../TiTan%20Atlas%20(BA)%20-%20Crypto%20Python.md)
- [ATLAS Crypto - криптовалютная адаптация TiTan ATLAS](../../../%D0%92%D0%B8%D0%BA%D0%B8/TiTan%20Bots%20-%20%D0%9C%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B8%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B2%D0%B8%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4/ATLAS%20Crypto%20-%20%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B2%D0%B0%D0%BB%D1%8E%D1%82%D0%BD%D0%B0%D1%8F%20%D0%B0%D0%B4%D0%B0%D0%BF%D1%82%D0%B0%D1%86%D0%B8%D1%8F%20TiTan%20ATLAS.md)
- BingX Spot

## Раздел

- portfolio;
- crypto spot;
- risk diversification;
- symbol selection.

## Источник

`2026-05-21 - ChatGPT - Хедж к BTC и BingX Spot.md`

## Почему это важно

Если все пары в портфеле падают вместе с BTC, то это не настоящий hedge. Золото может дать другой профиль движения и другую просадку.

## Как проверить

- проверить `PAXG/USDT` и `XAUT/USDT` через BingX Spot API;
- проверить объем, спред, стакан, минимальный ордер и комиссии;
- прогнать backtest;
- сравнить PnL, DD, Force и RiskGuard с `BTC/USDT` и `ETH/USDT`;
- смотреть не только прибыль пары, а влияние на весь портфель.

## Статус

`future / later / research / internal`.

Пары считать кандидатами, а не подтвержденными рабочими инструментами. Не использовать в публичных промо-материалах как обещание hedge, защиты от BTC или снижения риска без тестов.

## Ограничение

Даже если `PAXG` или `XAUT` дадут другой профиль движения, это не доказывает, что они подходят для робота. Перед тестами нужно проверить доступность через API, ликвидность, спред, комиссии, минимальный ордер, качество данных и влияние на портфельную equity.

## Связи

- [TiTan Atlas (BA)](../../../../TiTan%20Atlas%20(BA).md)
- [ATLAS Crypto - криптовалютная адаптация TiTan ATLAS](../../../%D0%92%D0%B8%D0%BA%D0%B8/TiTan%20Bots%20-%20%D0%9C%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B8%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B2%D0%B8%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4/ATLAS%20Crypto%20-%20%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B2%D0%B0%D0%BB%D1%8E%D1%82%D0%BD%D0%B0%D1%8F%20%D0%B0%D0%B4%D0%B0%D0%BF%D1%82%D0%B0%D1%86%D0%B8%D1%8F%20TiTan%20ATLAS.md)
- [BingX Spot - сначала API проверка потом тест](../../../../BingX%20Spot%20-%20%D1%81%D0%BD%D0%B0%D1%87%D0%B0%D0%BB%D0%B0%20API%20%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B0%20%D0%BF%D0%BE%D1%82%D0%BE%D0%BC%20%D1%82%D0%B5%D1%81%D1%82.md)
- [Crypto spot - полезность инструмента считать по портфелю](../../../../Crypto%20spot%20-%20%D0%BF%D0%BE%D0%BB%D0%B5%D0%B7%D0%BD%D0%BE%D1%81%D1%82%D1%8C%20%D0%B8%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D0%B0%20%D1%81%D1%87%D0%B8%D1%82%D0%B0%D1%82%D1%8C%20%D0%BF%D0%BE%20%D0%BF%D0%BE%D1%80%D1%82%D1%84%D0%B5%D0%BB%D1%8E.md)



## Что проверить

- Доступны ли `PAXG/USDT` и `XAUT/USDT` через BingX Spot API.
- Достаточны ли объем, стакан, спред, минимальный ордер и комиссии для сеточного бота.
- Дают ли эти пары иной профиль просадки по сравнению с `BTC/USDT` и `ETH/USDT`.
- Улучшают ли они портфельную equity, Force/RiskGuard и синхронность просадок.
- Не создает ли tokenized-слой отдельные риски эмитента, ликвидности или ограничений.
