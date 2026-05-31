---
type: question_map
status: active
created: 2026-05-28
source: Диалог Codex 2026-05-28
applies_to:
  - SolidarTrading
  - Telegram bot
  - RAG
  - external AI gateways
sections:
  - FAQ
  - question map
  - routing
---

# SolidarTrading - карта вопросов

## Назначение

Эта страница - не база ответов, а маршрутизатор вопросов.

Она помогает ИИ, Telegram-боту и RAG быстро понять:

- что спрашивает человек;
- какими словами он может это спросить;
- есть ли в базе нормальный ответ;
- к каким каноническим знаниям идти;
- что нельзя спутать.

Готовые ответы должны храниться в смысловых страницах: `Метод`, `Оффер`, `Партнерка`, `Возражения`, `Кейсы и доказательства`, `Риски и ограничения`.

## Статусы

- `answered` - есть каноническое знание, можно отвечать через него.
- `partial` - ответ есть, но неполный или слабый.
- `missing` - вопрос важный, но ответа в базе пока нет.
- `needs_update` - ответ есть, но требует обновления после новых данных.
- `do_not_answer` - не отвечать автоматически; нужен человек или отдельная проверка.

## Правило для бота

- Если `status = answered`, отвечать через связанные знания.
- Если `status = partial`, отвечать осторожно и указывать, что вопрос требует уточнения.
- Если `status = missing`, не выдумывать ответ; зафиксировать вопрос как пробел базы.
- Если `status = needs_update`, не использовать как финальный ответ без проверки.
- Если `status = do_not_answer`, передать человеку.

## Что такое SolidarTrading?

status: `answered`

intent: метод / первое объяснение / onboarding

Варианты вопроса:

- что такое SolidarTrading?
- что это за проект?
- это робот или метод?
- как это работает простыми словами?
- в чем суть?

Связанные знания:

- [SolidarTrading - долгосрочный метод заработка](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D0%BB%D0%B3%D0%BE%D1%81%D1%80%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B7%D0%B0%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B0.md)
- [SolidarTrading - базовый оффер](undefinedhidden/SolidarTrading%20-%20%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B9%20%D0%BE%D1%84%D1%84%D0%B5%D1%80.md)
- [Почему SolidarTrading не пирамида и не ДУ](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D0%B8%D1%80%D0%B0%D0%BC%D0%B8%D0%B4%D0%B0%20%D0%B8%20%D0%BD%D0%B5%20%D0%94%D0%A3.md)

Не путать:

- метод;
- проект с выплатами;
- доверительное управление;
- продажу робота;
- безрисковый кошелек.

## Как начать?

status: `answered`

intent: старт / подключение / инструкция / RoboForex

Варианты вопроса:

- как начать?
- как подключиться?
- что делать сначала?
- где зарегистрироваться?
- как открыть счет?
- как подключить копирование?
- какой код партнера?

Связанные знания:

- [RoboForex - запуск и бонусы SolidarTrading](undefinedhidden/RoboForex%20-%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%B8%20%D0%B1%D0%BE%D0%BD%D1%83%D1%81%D1%8B%20SolidarTrading.md)

Не путать:

- регистрацию;
- KYC;
- открытие счета;
- пополнение;
- бонус;
- подключение копирования;
- партнерский код.

## Какая комиссия?

status: `answered`

intent: commission / copy trading fee / current terms

Варианты вопроса:

- какая комиссия?
- сколько стоит подключение?
- сколько берет SolidarTrading?
- почему раньше было 5 процентов?
- комиссия может вырасти?
- сколько берут другие трейдеры?

Связанные знания:

- [SolidarTrading - базовый оффер](undefinedhidden/SolidarTrading%20-%20%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B9%20%D0%BE%D1%84%D1%84%D0%B5%D1%80.md)
- [RoboForex - запуск и бонусы SolidarTrading](undefinedhidden/RoboForex%20-%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%B8%20%D0%B1%D0%BE%D0%BD%D1%83%D1%81%D1%8B%20SolidarTrading.md)

Не путать:

- старую комиссию `5%`;
- текущую комиссию `10%`;
- комиссию других трейдеров;
- брокерские расходы;
- комиссию с прибыли;
- гарантию результата.

## Сколько можно заработать?

status: `answered`

intent: доходность / ожидания / доказательства

Варианты вопроса:

- сколько можно заработать?
- какая доходность?
- сколько в месяц?
- реально ли 10%?
- реально ли 30%?
- реально ли 120%?
- сколько сейчас показывает робот?

Связанные знания:

- [TiTan ATLAS v18 - доказательства и результаты](undefinedhidden/TiTan%20ATLAS%20v18%20-%20%D0%B4%D0%BE%D0%BA%D0%B0%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D1%81%D1%82%D0%B2%D0%B0%20%D0%B8%20%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D1%8B.md)
- [SolidarTrading - 3 неделя обновленного метода - первая передача](undefinedhidden/SolidarTrading%20-%203%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%20%D0%BF%D0%B5%D1%80%D0%B2%D0%B0%D1%8F%20%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0.md)
- [SolidarTrading - 4 неделя обновленного метода - 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)
- [SolidarTrading - 12-118 процентов в месяц и гарантия дохода](undefinedhidden/SolidarTrading%20-%2012-118%20%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%BD%D1%82%D0%BE%D0%B2%20%D0%B2%20%D0%BC%D0%B5%D1%81%D1%8F%D1%86%20%D0%B8%20%D0%B3%D0%B0%D1%80%D0%B0%D0%BD%D1%82%D0%B8%D1%8F%20%D0%B4%D0%BE%D1%85%D0%BE%D0%B4%D0%B0.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)

Не путать:

- backtest;
- реальный результат;
- прогноз;
- рекламный крючок;
- старые проценты первой версии;
- текущий оффер.

## Где доказательства?

status: `answered`

intent: proof / trust / Myfxbook / результаты

Варианты вопроса:

- где доказательства?
- чем подтверждается результат?
- можно посмотреть статистику?
- есть Myfxbook?
- почему я должен верить?
- где реальные цифры?

Связанные знания:

- [TiTan ATLAS v18 - доказательства и результаты](undefinedhidden/TiTan%20ATLAS%20v18%20-%20%D0%B4%D0%BE%D0%BA%D0%B0%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D1%81%D1%82%D0%B2%D0%B0%20%D0%B8%20%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D1%8B.md)
- [SolidarTrading - первая версия - первые отчеты](undefinedhidden/SolidarTrading%20-%20%D0%BF%D0%B5%D1%80%D0%B2%D0%B0%D1%8F%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20-%20%D0%BF%D0%B5%D1%80%D0%B2%D1%8B%D0%B5%20%D0%BE%D1%82%D1%87%D0%B5%D1%82%D1%8B.md)
- [SolidarTrading - 3 неделя обновленного метода - первая передача](undefinedhidden/SolidarTrading%20-%203%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%20%D0%BF%D0%B5%D1%80%D0%B2%D0%B0%D1%8F%20%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0.md)
- [SolidarTrading - 4 неделя обновленного метода - 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)

Не путать:

- внешнюю статистику;
- внутренний backtest;
- видеоотчет;
- скрин;
- короткий период наблюдения;
- текущую версию и раннюю первую версию.

## Какие риски?

status: `answered`

intent: риск / безопасность / просадка / гарантии

Варианты вопроса:

- какие риски?
- можно потерять деньги?
- что будет при просадке?
- это безопасно?
- есть ли гарантия?
- что если робот ошибется?

Связанные знания:

- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)
- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)
- [SolidarTrading - 12-118 процентов в месяц и гарантия дохода](undefinedhidden/SolidarTrading%20-%2012-118%20%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%BD%D1%82%D0%BE%D0%B2%20%D0%B2%20%D0%BC%D0%B5%D1%81%D1%8F%D1%86%20%D0%B8%20%D0%B3%D0%B0%D1%80%D0%B0%D0%BD%D1%82%D0%B8%D1%8F%20%D0%B4%D0%BE%D1%85%D0%BE%D0%B4%D0%B0.md)

Не путать:

- риск передачи денег;
- торговый риск;
- брокерский риск;
- технический риск;
- текущую просадку;
- максимальный будущий риск.

## Какие есть брокерские риски?

status: `answered`

intent: broker risk / broker choice / platform / jurisdiction

Варианты вопроса:

- какие риски у брокера?
- брокер может повлиять на результат?
- почему важно юрлицо брокера?
- что с лицензией брокера?
- почему нельзя выбрать любого брокера?
- что если у брокера закрытая платформа?

Связанные знания:

- [TiTan Bots - брокерские риски](undefinedhidden/TiTan%20Bots%20-%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D1%81%D0%BA%D0%B8%D0%B5%20%D1%80%D0%B8%D1%81%D0%BA%D0%B8.md)
- [TiTan Bots - как выбирать брокера для торговых роботов](undefinedhidden/TiTan%20Bots%20-%20%D0%BA%D0%B0%D0%BA%20%D0%B2%D1%8B%D0%B1%D0%B8%D1%80%D0%B0%D1%82%D1%8C%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D0%B0%20%D0%B4%D0%BB%D1%8F%20%D1%82%D0%BE%D1%80%D0%B3%D0%BE%D0%B2%D1%8B%D1%85%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%BE%D0%B2.md)
- [Почему партнерка брокера не доказывает надежность](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20%D0%BF%D0%B0%D1%80%D1%82%D0%BD%D0%B5%D1%80%D0%BA%D0%B0%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D0%B0%20%D0%BD%D0%B5%20%D0%B4%D0%BE%D0%BA%D0%B0%D0%B7%D1%8B%D0%B2%D0%B0%D0%B5%D1%82%20%D0%BD%D0%B0%D0%B4%D0%B5%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C.md)

Не путать:

- брокерский риск;
- торговый риск робота;
- партнерскую выгоду;
- официальный список условий;
- гарантию прибыли;
- проверку конкретного брокера на текущую дату.

## Как выбрать брокера для робота?

status: `answered`

intent: broker choice / trading robot / MT4 / MT5 / cTrader

Варианты вопроса:

- какой брокер подходит для робота?
- почему не любой брокер?
- что важно для советника?
- нужна MT4 или MT5?
- почему закрытая платформа плоха для бота?
- чем брокер для робота отличается от брокера для ручной торговли?

Связанные знания:

- [TiTan Bots - как выбирать брокера для торговых роботов](undefinedhidden/TiTan%20Bots%20-%20%D0%BA%D0%B0%D0%BA%20%D0%B2%D1%8B%D0%B1%D0%B8%D1%80%D0%B0%D1%82%D1%8C%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D0%B0%20%D0%B4%D0%BB%D1%8F%20%D1%82%D0%BE%D1%80%D0%B3%D0%BE%D0%B2%D1%8B%D1%85%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%BE%D0%B2.md)
- [TiTan Bots - брокерские риски](undefinedhidden/TiTan%20Bots%20-%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D1%81%D0%BA%D0%B8%D0%B5%20%D1%80%D0%B8%D1%81%D0%BA%D0%B8.md)
- [TiTan Bots - проверить и актуализировать список брокеров](undefinedTiTan%20Bots%20-%20%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%B8%D1%82%D1%8C%20%D0%B8%20%D0%B0%D0%BA%D1%82%D1%83%D0%B0%D0%BB%D0%B8%D0%B7%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C%20%D1%81%D0%BF%D0%B8%D1%81%D0%BE%D0%BA%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D0%BE%D0%B2.md)

Не путать:

- удобное приложение;
- платформу для автоматизации;
- партнерскую программу;
- надежность брокера;
- торговые условия;
- непроверенный рейтинг брокеров.

## Какие роботы есть в TiTan Bots?

status: `answered`

intent: robots / TiTan Bots / ATLAS / product line

Варианты вопроса:

- какие у вас роботы?
- это один робот или линейка?
- что такое TiTan Bots?
- почему сейчас ATLAS?
- можно купить робота?
- какие еще роботы будут?

Связанные знания:

- [TiTan Bots - линейка торговых роботов для SolidarTrading](undefinedhidden/TiTan%20Bots%20-%20%D0%BB%D0%B8%D0%BD%D0%B5%D0%B9%D0%BA%D0%B0%20%D1%82%D0%BE%D1%80%D0%B3%D0%BE%D0%B2%D1%8B%D1%85%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%BE%D0%B2%20%D0%B4%D0%BB%D1%8F%20SolidarTrading.md)
- [TiTan ATLAS v18 - доказательства и результаты](undefinedhidden/TiTan%20ATLAS%20v18%20-%20%D0%B4%D0%BE%D0%BA%D0%B0%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D1%81%D1%82%D0%B2%D0%B0%20%D0%B8%20%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D1%8B.md)

Не путать:

- текущий публичный робот ATLAS;
- внутреннюю техническую линейку;
- продажу робота как программы;
- подключение к системе;
- будущие роботы без публичных условий.

## Чем TiTan Bots отличается от одного торгового робота?

status: `answered`

intent: product / TiTan Bots / positioning / offer

Варианты вопроса:

- TiTan Bots это просто робот?
- чем это отличается от советника?
- что входит в продукт?
- это программа или система?
- почему это продукт, а не файл?

Связанные знания:

- [TiTan Bots - продуктовая упаковка и варианты участия](undefinedhidden/TiTan%20Bots%20-%20%D0%BF%D1%80%D0%BE%D0%B4%D1%83%D0%BA%D1%82%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%83%D0%BF%D0%B0%D0%BA%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%B8%D1%8F.md)
- [TiTan Bots - линейка торговых роботов для SolidarTrading](undefinedhidden/TiTan%20Bots%20-%20%D0%BB%D0%B8%D0%BD%D0%B5%D0%B9%D0%BA%D0%B0%20%D1%82%D0%BE%D1%80%D0%B3%D0%BE%D0%B2%D1%8B%D1%85%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%BE%D0%B2%20%D0%B4%D0%BB%D1%8F%20SolidarTrading.md)
- [Почему TiTan Bots сложно скопировать](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20TiTan%20Bots%20%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%20%D1%81%D0%BA%D0%BE%D0%BF%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C.md)

Не путать:

- файл советника;
- публичный способ подключения;
- внутреннюю разработку;
- сопровождение;
- гарантию результата.

## Почему TiTan Bots сложно скопировать?

status: `answered`

intent: trust / moat / copy / objections

Варианты вопроса:

- почему вас не скопируют?
- почему не сделать такого же бота?
- что мешает конкурентам повторить?
- в чем уникальность TiTan Bots?
- почему ценность не только в коде?

Связанные знания:

- [Почему TiTan Bots сложно скопировать](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20TiTan%20Bots%20%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%20%D1%81%D0%BA%D0%BE%D0%BF%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C.md)
- [TiTan Bots - продуктовая упаковка и варианты участия](undefinedhidden/TiTan%20Bots%20-%20%D0%BF%D1%80%D0%BE%D0%B4%D1%83%D0%BA%D1%82%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%83%D0%BF%D0%B0%D0%BA%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%B8%D1%8F.md)
- [TiTan ATLAS v18 - доказательства и результаты](undefinedhidden/TiTan%20ATLAS%20v18%20-%20%D0%B4%D0%BE%D0%BA%D0%B0%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D1%81%D1%82%D0%B2%D0%B0%20%D0%B8%20%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D1%8B.md)

Не путать:

- сложность повторения;
- невозможность копирования;
- технический код;
- путь разработки;
- доказательство доходности.

## Можно ли White Label или под своим брендом?

status: `partial`

intent: White Label / business / partner / own brand

Варианты вопроса:

- можно под своим брендом?
- есть White Label?
- можно запустить для своей команды?
- можно компании использовать TiTan Bots?
- можно купить продукт для своего проекта?

Связанные знания:

- [TiTan Bots - продуктовая упаковка и варианты участия](undefinedhidden/TiTan%20Bots%20-%20%D0%BF%D1%80%D0%BE%D0%B4%D1%83%D0%BA%D1%82%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%83%D0%BF%D0%B0%D0%BA%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%B8%D1%8F.md)
- [SolidarTrading - три роли участия](undefinedhidden/SolidarTrading%20-%20%D1%82%D1%80%D0%B8%20%D1%80%D0%BE%D0%BB%D0%B8%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%B8%D1%8F.md)
- [TiTan Bots - уточнить условия White Label](undefinedTiTan%20Bots%20-%20%D1%83%D1%82%D0%BE%D1%87%D0%BD%D0%B8%D1%82%D1%8C%20%D1%83%D1%81%D0%BB%D0%BE%D0%B2%D0%B8%D1%8F%20White%20Label.md)

Не путать:

- идею White Label;
- готовый публичный регламент;
- продажу файла робота;
- управляемый формат;
- полный контроль партнера;
- гарантию запуска за конкретный срок.

## Можно ли купить робота или продукт?

status: `partial`

intent: buy bot / product purchase / direct sale / managed format

Варианты вопроса:

- можно купить робота?
- можно купить TiTan Bots?
- можно поставить у себя?
- можно без копитрейдинга?
- можно получить настройки?

Связанные знания:

- [Почему SolidarTrading не продает робота напрямую](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D0%B5%D1%82%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%B0%20%D0%BD%D0%B0%D0%BF%D1%80%D1%8F%D0%BC%D1%83%D1%8E.md)
- [TiTan Bots - продуктовая упаковка и варианты участия](undefinedhidden/TiTan%20Bots%20-%20%D0%BF%D1%80%D0%BE%D0%B4%D1%83%D0%BA%D1%82%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%83%D0%BF%D0%B0%D0%BA%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%B8%D1%8F.md)
- [TiTan Bots - линейка торговых роботов для SolidarTrading](undefinedhidden/TiTan%20Bots%20-%20%D0%BB%D0%B8%D0%BD%D0%B5%D0%B9%D0%BA%D0%B0%20%D1%82%D0%BE%D1%80%D0%B3%D0%BE%D0%B2%D1%8B%D1%85%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%BE%D0%B2%20%D0%B4%D0%BB%D1%8F%20SolidarTrading.md)

Не путать:

- текущий публичный формат подключения;
- покупку продукта;
- самостоятельную установку;
- управляемое сопровождение;
- технический файл;
- торговый риск.

## Кто стоит за SolidarTrading?

status: `partial`

intent: founder / trust / person / credibility

Варианты вопроса:

- кто основатель?
- кто такой Алексей Титов?
- кто отвечает за проект?
- кому писать по подключению?
- почему можно доверять?

Связанные знания:

- [SolidarTrading - основатель и доверие к проекту](undefinedhidden/SolidarTrading%20-%20%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%20%D0%B8%20%D0%B4%D0%BE%D0%B2%D0%B5%D1%80%D0%B8%D0%B5%20%D0%BA%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D1%83.md)
- [Почему SolidarTrading не пирамида и не ДУ](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D0%B8%D1%80%D0%B0%D0%BC%D0%B8%D0%B4%D0%B0%20%D0%B8%20%D0%BD%D0%B5%20%D0%94%D0%A3.md)

Не путать:

- роль основателя;
- доверительное управление;
- доказательства доходности;
- неподтвержденную биографию;
- гарантию результата.

## Это скальпинг?

status: `answered`

intent: strategy / Atlas / grid / scalping

Варианты вопроса:

- это скальпинг?
- Atlas скальпер?
- почему так много сделок?
- это сетка?
- что за стратегия у робота?

Связанные знания:

- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)

Не путать:

- частые сделки;
- ручной скальпинг;
- сеточную систему;
- гарантию безопасности;
- защиту депозита.

## Почему бывают минусовые сделки?

status: `answered`

intent: losing trades / protection / drawdown

Варианты вопроса:

- почему есть минусовые сделки?
- робот ошибся?
- почему сделка закрылась в минус?
- защита депозита съела прибыль?
- минусовые сделки это плохо?

Связанные знания:

- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)

Не путать:

- минусовую сделку;
- поломку робота;
- защитное закрытие;
- гарантию восстановления;
- текущий результат.

## Плечо 1 к 2000 это опасно?

status: `answered`

intent: leverage / Forex / robot advantage / account capacity

Варианты вопроса:

- плечо 1 к 2000 это опасно?
- зачем такое большое плечо?
- большое плечо увеличивает риск?
- почему RoboForex дает 1:2000?
- чем плечо полезно роботу?

Связанные знания:

- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)
- [RoboForex - запуск и бонусы SolidarTrading](undefinedhidden/RoboForex%20-%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%B8%20%D0%B1%D0%BE%D0%BD%D1%83%D1%81%D1%8B%20SolidarTrading.md)

Не путать:

- ручной азартный трейдинг;
- роботизированную торговлю по системе;
- возможности счета;
- обещание прибыли;
- условия бонуса RoboForex, где может требоваться плечо не выше `1:1000`.

## Что если сервер отключится?

status: `answered`

intent: technical risk / VPS / recovery / operations

Варианты вопроса:

- что если сервер отключится?
- что если терминал зависнет?
- что если Windows Update остановит робота?
- робот сможет продолжить?
- что будет с открытыми сделками?

Связанные знания:

- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)

Не путать:

- восстановление после остановки;
- гарантию отсутствия последствий;
- серверный риск;
- брокерский риск;
- открытые сделки.

## Что если автор недоступен?

status: `partial`

intent: operations / team / key person risk

Варианты вопроса:

- что если Алексей недоступен?
- кто следит за роботом?
- есть команда?
- что если главный разработчик заболел?
- кто меняет настройки?

Связанные знания:

- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)

Не путать:

- текущий ранний риск;
- будущую команду;
- регламенты;
- автоматизацию торговли;
- полное отсутствие человеческого фактора.

## Деньги правда в безопасности?

status: `answered`

intent: safety / money control / drawdown / bank comparison

Варианты вопроса:

- деньги правда в безопасности?
- когда деньги в безопасности?
- это как банковский вклад?
- почему защита может закрыть сделки в минус?
- почему защита уменьшает прибыль?
- можно ли гарантировать, что счет не сольется?

Связанные знания:

- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - базовый оффер](undefinedhidden/SolidarTrading%20-%20%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B9%20%D0%BE%D1%84%D1%84%D0%B5%D1%80.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)

Не путать:

- деньги на счете пользователя;
- отсутствие доверительного управления;
- банковский вклад;
- торговый риск;
- защиту депозита;
- гарантию сохранности.

## Почему флет полезен сеточному роботу?

status: `answered`

intent: method / grid trading / market conditions / drawdown

Варианты вопроса:

- что такое флет?
- почему сетке выгоден боковик?
- почему роботу хорошо, когда рынок стоит?
- флет снижает риск?
- что будет, если флет закончится?

Связанные знания:

- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)

Не путать:

- флет как удобную среду;
- гарантию прибыли;
- отсутствие просадки;
- риск выхода из диапазона;
- сезонный крючок;
- текущий оффер.

## Это пирамида или доверительное управление?

status: `answered`

intent: trust / scam objection / money control

Варианты вопроса:

- это пирамида?
- это ДУ?
- я отдаю деньги вам?
- деньги у кого?
- это хайп?
- откуда берутся выплаты?

Связанные знания:

- [Почему SolidarTrading не пирамида и не ДУ](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D0%B8%D1%80%D0%B0%D0%BC%D0%B8%D0%B4%D0%B0%20%D0%B8%20%D0%BD%D0%B5%20%D0%94%D0%A3.md)
- [SolidarTrading - доход от оборота](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D1%85%D0%BE%D0%B4%20%D0%BE%D1%82%20%D0%BE%D0%B1%D0%BE%D1%80%D0%BE%D1%82%D0%B0.md)
- [SolidarTrading - долгосрочный метод заработка](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D0%BB%D0%B3%D0%BE%D1%81%D1%80%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B7%D0%B0%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B0.md)
- [SolidarTrading - 4 неделя 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)

Не путать:

- личный счет клиента;
- копирование сделок;
- партнерку с оборота;
- выплаты из депозита;
- отсутствие ДУ;
- отсутствие риска.

## Где будут мои деньги?

status: `answered`

intent: money location / account control / withdrawal

Варианты вопроса:

- где мои деньги?
- я перевожу деньги вам?
- можно вывести деньги?
- кто контролирует счет?
- деньги на кошельке?
- это обычный кошелек?

Связанные знания:

- [RoboForex - запуск и бонусы SolidarTrading](undefinedhidden/RoboForex%20-%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%B8%20%D0%B1%D0%BE%D0%BD%D1%83%D1%81%D1%8B%20SolidarTrading.md)
- [Почему SolidarTrading не пирамида и не ДУ](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D0%B8%D1%80%D0%B0%D0%BC%D0%B8%D0%B4%D0%B0%20%D0%B8%20%D0%BD%D0%B5%20%D0%94%D0%A3.md)
- [SolidarTrading - базовый оффер](undefinedhidden/SolidarTrading%20-%20%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B9%20%D0%BE%D1%84%D1%84%D0%B5%D1%80.md)

Не путать:

- личный счет;
- брокерский счет;
- кошелек;
- доверительное управление;
- право вывода;
- торговый риск.

## Почему робот не продается напрямую?

status: `answered`

intent: product model / copy trading / technical simplicity

Варианты вопроса:

- почему нельзя купить робота?
- продайте мне советника;
- я хочу поставить робота сам;
- почему через копирование?
- почему вы не даете настройки?

Связанные знания:

- [Почему SolidarTrading не продает робота напрямую](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D0%B5%D1%82%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%B0%20%D0%BD%D0%B0%D0%BF%D1%80%D1%8F%D0%BC%D1%83%D1%8E.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)

Не путать:

- продажу файла;
- копитрейдинг;
- самостоятельную настройку;
- централизованное сопровождение;
- отсутствие торгового риска.

## Чем копитрейдинг отличается от сигналов?

status: `answered`

intent: copy trading / signals / automation / execution

Варианты вопроса:

- почему не сигналы?
- чем копитрейдинг лучше сигналов?
- можно просто давать сделки?
- почему я не могу открывать руками?
- что лучше: сигналы или копирование?
- почему winrate не показатель?

Связанные знания:

- [Почему копитрейдинг лучше сигналов](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20%D0%BA%D0%BE%D0%BF%D0%B8%D1%82%D1%80%D0%B5%D0%B9%D0%B4%D0%B8%D0%BD%D0%B3%20%D0%BB%D1%83%D1%87%D1%88%D0%B5%20%D1%81%D0%B8%D0%B3%D0%BD%D0%B0%D0%BB%D0%BE%D0%B2.md)
- [SolidarTrading - идеальный трейдинг автоматизация копитрейдинг и статистика](undefinedhidden/SolidarTrading%20-%20%D0%B8%D0%B4%D0%B5%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9%20%D1%82%D1%80%D0%B5%D0%B9%D0%B4%D0%B8%D0%BD%D0%B3%20%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F%20%D0%BA%D0%BE%D0%BF%D0%B8%D1%82%D1%80%D0%B5%D0%B9%D0%B4%D0%B8%D0%BD%D0%B3%20%D0%B8%20%D1%81%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0.md)
- [Почему SolidarTrading не продает робота напрямую](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D0%B5%D1%82%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%D0%B0%20%D0%BD%D0%B0%D0%BF%D1%80%D1%8F%D0%BC%D1%83%D1%8E.md)

Не путать:

- сигнал;
- автоматическое копирование;
- ручное исполнение;
- задержку;
- проскальзывание;
- winrate;
- просадку;
- гарантию прибыли.

## Что такое партнерка с оборота?

status: `answered`

intent: partner model / turnover / broker commissions

Варианты вопроса:

- что такое партнерка с оборота?
- откуда партнерские деньги?
- партнерка идет с депозита?
- почему это не пирамида?
- чем 10 уровней отличаются от стандартной партнерки?
- что значит 40 и 5 процентов?

Связанные знания:

- [SolidarTrading - доход от оборота](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D1%85%D0%BE%D0%B4%20%D0%BE%D1%82%20%D0%BE%D0%B1%D0%BE%D1%80%D0%BE%D1%82%D0%B0.md)
- [Почему SolidarTrading не пирамида и не ДУ](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B5%20%D0%BF%D0%B8%D1%80%D0%B0%D0%BC%D0%B8%D0%B4%D0%B0%20%D0%B8%20%D0%BD%D0%B5%20%D0%94%D0%A3.md)

Не путать:

- депозит;
- доход клиента;
- торговый оборот;
- комиссия/спред брокера;
- партнерский доход;
- гарантию дохода.

## Если у брокера есть партнерка, значит он надежный?

status: `answered`

intent: broker partner program / trust / reliability / objection

Варианты вопроса:

- партнерка значит брокер хороший?
- если брокер платит партнерам, ему можно доверять?
- почему вы рекомендуете брокера с партнеркой?
- партнерская программа доказывает надежность?
- не конфликт ли это интересов?

Связанные знания:

- [Почему партнерка брокера не доказывает надежность](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20%D0%BF%D0%B0%D1%80%D1%82%D0%BD%D0%B5%D1%80%D0%BA%D0%B0%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D0%B0%20%D0%BD%D0%B5%20%D0%B4%D0%BE%D0%BA%D0%B0%D0%B7%D1%8B%D0%B2%D0%B0%D0%B5%D1%82%20%D0%BD%D0%B0%D0%B4%D0%B5%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C.md)
- [TiTan Bots - брокерские риски](undefinedhidden/TiTan%20Bots%20-%20%D0%B1%D1%80%D0%BE%D0%BA%D0%B5%D1%80%D1%81%D0%BA%D0%B8%D0%B5%20%D1%80%D0%B8%D1%81%D0%BA%D0%B8.md)
- [SolidarTrading - доход от оборота](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D1%85%D0%BE%D0%B4%20%D0%BE%D1%82%20%D0%BE%D0%B1%D0%BE%D1%80%D0%BE%D1%82%D0%B0.md)

Не путать:

- коммерческую партнерку;
- надежность брокера;
- торговые условия;
- вывод средств;
- гарантию дохода;
- независимую проверку брокера.

## Что такое сложный процент и запас прочности счета?

status: `answered`

intent: compound interest / risk / account strength

Варианты вопроса:

- что такое сложный процент?
- почему он снижает риск?
- как прибыль усиливает счет?
- что значит запас прочности?
- почему большему счету легче выдержать просадку?
- автодолив это безопасно?

Связанные знания:

- [SolidarTrading - сложный процент и автодолив](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D1%8B%D0%B9%20%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%BD%D1%82%20%D0%B8%20%D0%B0%D0%B2%D1%82%D0%BE%D0%B4%D0%BE%D0%BB%D0%B8%D0%B2.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)

Не путать:

- рост доходности;
- рост запаса прочности;
- отсутствие риска;
- долив собственных денег;
- прибыль, оставленную в работе;
- старые агрессивные расчеты.

## Почему был рестарт SolidarTrading?

status: `answered`

intent: restart / early failure / copy trading / safe mode

Варианты вопроса:

- почему был рестарт?
- что случилось в первом запуске?
- почему люди потеряли деньги?
- бот слил?
- что изменили после сбоя?
- что такое безопасный режим?

Связанные знания:

- [SolidarTrading - рестарт после сбоя и безопасный режим](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B5%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D1%81%D0%B1%D0%BE%D1%8F%20%D0%B8%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D1%8B%D0%B9%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC.md)
- [SolidarTrading - ранний тестовый этап и переход к обновленному методу](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B0%D0%BD%D0%BD%D0%B8%D0%B9%20%D1%82%D0%B5%D1%81%D1%82%D0%BE%D0%B2%D1%8B%D0%B9%20%D1%8D%D1%82%D0%B0%D0%BF%20%D0%B8%20%D0%BF%D0%B5%D1%80%D0%B5%D1%85%D0%BE%D0%B4%20%D0%BA%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%BC%D1%83%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D1%83.md)
- [SolidarTrading - неудобные вопросы и честные ответы](undefinedhidden/SolidarTrading%20-%20%D0%BD%D0%B5%D1%83%D0%B4%D0%BE%D0%B1%D0%BD%D1%8B%D0%B5%20%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B8%20%D1%87%D0%B5%D1%81%D1%82%D0%BD%D1%8B%D0%B5%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%8B.md)

Не путать:

- ранний тестовый этап;
- текущий оффер;
- версию метода;
- версию кода бота;
- проблему копитрейдинга;
- полное устранение риска.

## Что такое безопасный режим?

status: `answered`

intent: safe mode / risk control / drawdown / restart

Варианты вопроса:

- что такое безопасный режим?
- почему доходность ниже?
- почему запущено мало пар?
- что значит сначала риск потом доходность?
- почему мастер-счет стал 200?
- что такое суперзащита?
- что такое защита от сквизов?

Связанные знания:

- [SolidarTrading - рестарт после сбоя и безопасный режим](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B5%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D1%81%D0%B1%D0%BE%D1%8F%20%D0%B8%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D1%8B%D0%B9%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC.md)
- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - сеточный робот и просадка простым языком](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%B5%D1%82%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D1%80%D0%BE%D0%B1%D0%BE%D1%82%20%D0%B8%20%D0%BF%D1%80%D0%BE%D1%81%D0%B0%D0%B4%D0%BA%D0%B0%20%D0%BF%D1%80%D0%BE%D1%81%D1%82%D1%8B%D0%BC%20%D1%8F%D0%B7%D1%8B%D0%BA%D0%BE%D0%BC.md)

Не путать:

- осторожный режим;
- гарантию сохранности;
- текущую просадку;
- будущий максимум риска;
- расчетные проценты;
- рекламную фразу `абсолютно безопасно`.

## Что значит первая передача и 4-ступенчатая защита?

status: `answered`

intent: safe mode / protection levels / v13 v14 / sales analogy

Варианты вопроса:

- что значит первая передача?
- почему робот едет на первой передаче?
- что такое 4-ступенчатая защита?
- зачем нужна коробка передач в боте?
- почему v14 должна быть лучше?
- когда включат следующие режимы?

Связанные знания:

- [SolidarTrading - 3 неделя обновленного метода - первая передача](undefinedhidden/SolidarTrading%20-%203%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%20%D0%BF%D0%B5%D1%80%D0%B2%D0%B0%D1%8F%20%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0.md)
- [SolidarTrading - 4 неделя обновленного метода - 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)
- [SolidarTrading - рестарт после сбоя и безопасный режим](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B5%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D1%81%D0%B1%D0%BE%D1%8F%20%D0%B8%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D1%8B%D0%B9%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC.md)
- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - 3 неделя мы едем на первой передаче](undefinedhidden/SolidarTrading%20-%203%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BC%D1%8B%20%D0%B5%D0%B4%D0%B5%D0%BC%20%D0%BD%D0%B0%20%D0%BF%D0%B5%D1%80%D0%B2%D0%BE%D0%B9%20%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B5.md)
- [SolidarTrading - 4 неделя 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)

Не путать:

- аналогию;
- доказанный режим;
- будущий план `v14`;
- обещание большей доходности;
- защиту как снижение риска;
- гарантию отсутствия убытка.

## Почему 4-я неделя дала мало?

status: `answered`

intent: report / low return / safe mode / drawdown

Варианты вопроса:

- почему за 4 неделю всего 0.3%?
- почему робот заработал мало?
- безопасный режим режет прибыль?
- зачем жертвовать прибылью?
- что значит 14 версия уже в деле?

Связанные знания:

- [SolidarTrading - 4 неделя обновленного метода - 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B0%20-%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)
- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - рестарт после сбоя и безопасный режим](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B5%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D1%81%D0%B1%D0%BE%D1%8F%20%D0%B8%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D1%8B%D0%B9%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC.md)
- [SolidarTrading - 4 неделя 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)

Не путать:

- слабую неделю;
- провал системы;
- безопасный режим;
- потерю части потенциальной прибыли;
- доказанную доходность `v14`;
- обещание будущих `10-15%`.

## Может ли робот работать на падении рынка?

status: `answered`

intent: market stress / falling market / safe mode / proof

Варианты вопроса:

- что будет, если рынок падает?
- робот зарабатывает на падении?
- как бот ведет себя в кризисный день?
- почему в ролике важна просадка 0.18%?
- падение рынка это хорошо или плохо для робота?

Связанные знания:

- [SolidarTrading - рестарт после сбоя и безопасный режим](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B5%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D1%81%D0%B1%D0%BE%D1%8F%20%D0%B8%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D1%8B%D0%B9%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC.md)
- [SolidarTrading - защита депозита и стоимость безопасности](undefinedhidden/SolidarTrading%20-%20%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0%20%D0%B4%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%B0%20%D0%B8%20%D1%81%D1%82%D0%BE%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D1%8C%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D0%B8.md)
- [SolidarTrading - старт бота в безопасном режиме](undefinedhidden/SolidarTrading%20-%20%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%B1%D0%BE%D1%82%D0%B0%20%D0%B2%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D0%BC%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%D0%B5.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)

Не путать:

- работу в стрессовом рынке;
- гарантию заработка на любом падении;
- один день демонстрации;
- независимое доказательство;
- расчетные проценты;
- текущий оффер.

## Почему старт с Forex, а не с крипты?

status: `answered`

intent: market choice / Forex / crypto future

Варианты вопроса:

- почему Forex?
- почему не крипта?
- будет ли криптоверсия?
- крипта же популярнее;
- Forex надежнее?

Связанные знания:

- [Почему SolidarTrading начал с Forex а не с крипты](undefinedhidden/%D0%9F%D0%BE%D1%87%D0%B5%D0%BC%D1%83%20SolidarTrading%20%D0%BD%D0%B0%D1%87%D0%B0%D0%BB%20%D1%81%20Forex%20%D0%B0%20%D0%BD%D0%B5%20%D1%81%20%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D1%8B.md)
- [SolidarTrading - долгосрочный метод заработка](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D0%BB%D0%B3%D0%BE%D1%81%D1%80%D0%BE%D1%87%D0%BD%D1%8B%D0%B9%20%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B7%D0%B0%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B0.md)
- [SolidarTrading - старт бота в безопасном режиме](undefinedhidden/SolidarTrading%20-%20%D1%81%D1%82%D0%B0%D1%80%D1%82%20%D0%B1%D0%BE%D1%82%D0%B0%20%D0%B2%20%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D0%BC%20%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%D0%B5.md)
- [SolidarTrading - 3 неделя мы едем на первой передаче](undefinedhidden/SolidarTrading%20-%203%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%20%D0%BC%D1%8B%20%D0%B5%D0%B4%D0%B5%D0%BC%20%D0%BD%D0%B0%20%D0%BF%D0%B5%D1%80%D0%B2%D0%BE%D0%B9%20%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B5.md)
- [SolidarTrading - 4 неделя 14 версия уже в деле](undefinedhidden/SolidarTrading%20-%204%20%D0%BD%D0%B5%D0%B4%D0%B5%D0%BB%D1%8F%2014%20%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%83%D0%B6%D0%B5%20%D0%B2%20%D0%B4%D0%B5%D0%BB%D0%B5.md)
- [Крипто-версия с отдельной логикой риска](undefined%D0%9A%D1%80%D0%B8%D0%BF%D1%82%D0%BE-%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D1%81%20%D0%BE%D1%82%D0%B4%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%D0%B9%20%D0%BB%D0%BE%D0%B3%D0%B8%D0%BA%D0%BE%D0%B9%20%D1%80%D0%B8%D1%81%D0%BA%D0%B0.md)

Не путать:

- зрелость инфраструктуры;
- безопасность;
- ликвидность;
- готовый продукт;
- будущее направление.

## Почему ATLAS на споте лучше обычного ходлинга?

status: `partial`

intent: crypto spot / ATLAS / holding / product explanation

Варианты вопроса:

- чем ATLAS Spot лучше холда?
- почему не просто купить BTC?
- что дает робот на споте?
- если рынок падает, что делает робот?
- чем сетка лучше одной покупки?

Связанные знания:

- [ATLAS SPOT BUY - риск меняется с margin call на холд монеты](undefinedATLAS%20SPOT%20BUY%20-%20%D1%80%D0%B8%D1%81%D0%BA%20%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D1%82%D1%81%D1%8F%20%D1%81%20margin%20call%20%D0%BD%D0%B0%20%D1%85%D0%BE%D0%BB%D0%B4%20%D0%BC%D0%BE%D0%BD%D0%B5%D1%82%D1%8B.md)
- [ATLAS SPOT BUY - архитектура спотовой BUY-сетки](undefinedATLAS%20SPOT%20BUY%20-%20%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0%20%D1%81%D0%BF%D0%BE%D1%82%D0%BE%D0%B2%D0%BE%D0%B9%20BUY-%D1%81%D0%B5%D1%82%D0%BA%D0%B8.md)
- [ATLAS SPOT BUY - сравнить с ходлингом на BTC ETH и топовых монетах](undefinedATLAS%20SPOT%20BUY%20-%20%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B8%D1%82%D1%8C%20%D1%81%20%D1%85%D0%BE%D0%B4%D0%BB%D0%B8%D0%BD%D0%B3%D0%BE%D0%BC%20%D0%BD%D0%B0%20BTC%20ETH%20%D0%B8%20%D1%82%D0%BE%D0%BF%D0%BE%D0%B2%D1%8B%D1%85%20%D0%BC%D0%BE%D0%BD%D0%B5%D1%82%D0%B0%D1%85.md)

Не путать:

- ходлинг;
- сеточный набор позиции;
- фиксацию откатов;
- среднюю цену входа;
- обесценивание самого актива;
- тесты параметров и монет.

## Если на споте нет ликвидации, значит риска нет?

status: `answered`

intent: crypto spot / liquidation / risk / objection

Варианты вопроса:

- на споте нет риска?
- что значит нет ликвидации?
- можно ли слить депозит на споте?
- что будет при просадке 100 процентов?
- чем спот безопаснее Forex?

Связанные знания:

- [ATLAS SPOT BUY - риск меняется с margin call на холд монеты](undefinedATLAS%20SPOT%20BUY%20-%20%D1%80%D0%B8%D1%81%D0%BA%20%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D1%82%D1%81%D1%8F%20%D1%81%20margin%20call%20%D0%BD%D0%B0%20%D1%85%D0%BE%D0%BB%D0%B4%20%D0%BC%D0%BE%D0%BD%D0%B5%D1%82%D1%8B.md)
- [ATLAS SPOT BUY - архитектура спотовой BUY-сетки](undefinedATLAS%20SPOT%20BUY%20-%20%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0%20%D1%81%D0%BF%D0%BE%D1%82%D0%BE%D0%B2%D0%BE%D0%B9%20BUY-%D1%81%D0%B5%D1%82%D0%BA%D0%B8.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)

Не путать:

- маржинальную ликвидацию;
- холд монеты;
- обесценивание самого актива;
- слабые альты;
- топовые ликвидные монеты;
- гарантию роста монеты.

## Что такое солидарные десятки?

status: `partial`

intent: partner structure / team mechanics / future rules

Варианты вопроса:

- что такое солидарная десятка?
- будут ли гарантированные партнеры?
- можно ли зарабатывать без приглашений?
- как работает нижний аккаунт?
- что такое партнерский R-робот?

Связанные знания:

- [SolidarTrading - солидарные десятки](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%BE%D0%BB%D0%B8%D0%B4%D0%B0%D1%80%D0%BD%D1%8B%D0%B5%20%D0%B4%D0%B5%D1%81%D1%8F%D1%82%D0%BA%D0%B8.md)
- [SolidarTrading - партнерский R-робот](undefinedhidden/SolidarTrading%20-%20%D0%BF%D0%B0%D1%80%D1%82%D0%BD%D0%B5%D1%80%D1%81%D0%BA%D0%B8%D0%B9%20R-%D1%80%D0%BE%D0%B1%D0%BE%D1%82.md)
- [SolidarTrading - доход от оборота](undefinedhidden/SolidarTrading%20-%20%D0%B4%D0%BE%D1%85%D0%BE%D0%B4%20%D0%BE%D1%82%20%D0%BE%D0%B1%D0%BE%D1%80%D0%BE%D1%82%D0%B0.md)

Не путать:

- концепцию;
- готовый регламент;
- гарантию рефералов;
- гарантию партнерского дохода;
- торговый оборот;
- торговую прибыль.

## Для кого подходит SolidarTrading?

status: `partial`

intent: audience / fit / roles

Варианты вопроса:

- кому это подходит?
- для кого этот метод?
- я новичок, мне можно?
- это для инвестора или партнера?
- можно ли без аудитории?
- можно ли как лидер команды?

Связанные знания:

- [SolidarTrading - три роли участия](undefinedhidden/SolidarTrading%20-%20%D1%82%D1%80%D0%B8%20%D1%80%D0%BE%D0%BB%D0%B8%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%B8%D1%8F.md)
- [Целевая аудитория - Индекс](undefinedhidden/%D0%A6%D0%B5%D0%BB%D0%B5%D0%B2%D0%B0%D1%8F%20%D0%B0%D1%83%D0%B4%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D1%8F%20-%20%D0%98%D0%BD%D0%B4%D0%B5%D0%BA%D1%81.md)
- [SolidarTrading - базовый оффер](undefinedhidden/SolidarTrading%20-%20%D0%B1%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B9%20%D0%BE%D1%84%D1%84%D0%B5%D1%80.md)

Не путать:

- клиента;
- партнера;
- лидера команды;
- новичка без опыта;
- человека с аудиторией;
- человека, которому нельзя брать торговый риск.

## Можно ли подключить юридическое лицо?

status: `missing`

intent: legal entity / company account / compliance

Варианты вопроса:

- можно от компании?
- можно на юридическое лицо?
- можно для бизнеса?
- как подключить счет компании?
- есть ли документы для юрлица?

Связанные знания:

- нет.

Не путать:

- личный счет;
- корпоративный счет;
- налоги;
- брокерские правила;
- юридическую консультацию.

## Какие налоги?

status: `missing`

intent: taxes / legal / jurisdiction

Варианты вопроса:

- какие налоги платить?
- кто платит налоги?
- нужно ли декларировать прибыль?
- как платить налоги с партнерки?
- как платить налоги с трейдинга?

Связанные знания:

- нет.

Не путать:

- торговую прибыль;
- партнерское вознаграждение;
- страну налогового резидентства;
- юридическую консультацию.

## Можно ли подключить большой депозит?

status: `partial`

intent: deposit size / risk mode / onboarding

Варианты вопроса:

- можно завести большую сумму?
- с какой суммы лучше начать?
- можно ли сразу 10 тысяч?
- чем больше счет, тем безопаснее?
- как выбрать сумму?

Связанные знания:

- [SolidarTrading - сложный процент и автодолив](undefinedhidden/SolidarTrading%20-%20%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D1%8B%D0%B9%20%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%BD%D1%82%20%D0%B8%20%D0%B0%D0%B2%D1%82%D0%BE%D0%B4%D0%BE%D0%BB%D0%B8%D0%B2.md)
- [SolidarTrading - рискованные обещания в презентации](undefinedhidden/SolidarTrading%20-%20%D1%80%D0%B8%D1%81%D0%BA%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5%20%D0%BE%D0%B1%D0%B5%D1%89%D0%B0%D0%BD%D0%B8%D1%8F%20%D0%B2%20%D0%BF%D1%80%D0%B5%D0%B7%D0%B5%D0%BD%D1%82%D0%B0%D1%86%D0%B8%D0%B8.md)
- [RoboForex - запуск и бонусы SolidarTrading](undefinedhidden/RoboForex%20-%20%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%20%D0%B8%20%D0%B1%D0%BE%D0%BD%D1%83%D1%81%D1%8B%20SolidarTrading.md)

Не путать:

- стартовую сумму;
- комфортный риск;
- запас прочности;
- гарантию безопасности;
- старые расчеты на больших депозитах.
