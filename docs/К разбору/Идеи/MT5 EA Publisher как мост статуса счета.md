---
type: idea
priority_score: 8
created: 2026-06-23
source:
  - "Диалог Codex 2026-06-23: идея пользователя про схему Myfxbook EA/EX5"
  - https://www.myfxbook.com/help/knowledge-base/metatrader-5-connection-methods/
applies_to:
  - TradingObserver
  - MT5 account monitoring
  - automation scripts
sections:
  - technique
  - monitoring
  - architecture
---

# MT5 EA Publisher как мост статуса счета

## Оценка ценности

`8/10`

## Lifecycle

Эта карточка живет в `К разбору/Идеи`.

Это не active/current знание и не ответ для RAG. Идея становится active wiki-карточкой только после проверки, решения владельца и отдельного переноса в нужный слой. Если реальная идея отклонена, перенести в `Архив/К разбору/Отклонено` с причиной и условием возврата.

Если карточка оказалась не настоящей идеей, а старым пробелом, остатком импорта, дублем задачи формирования базы или закрытым owner-gap, после закрытия перенести ее в обычный `Архив/К разбору`, не в `Отклонено`.

`К разбору/Идеи` и `К разбору/Ошибки` являются отдельными ручными входами после audit-stack. Status/queue/warning/guard не является финальным закрытием без terminal owner или lifecycle destination.

## Идея

Проверить альтернативную архитектуру TradingObserver: вместо прямого Python-подключения к локальному MT5 через библиотеку `MetaTrader5` поставить на график небольшой read-only MQL5 Expert Advisor, который будет читать состояние счета в терминале и передавать данные наружу в файл, локальный HTTP endpoint, named pipe или другой простой канал.

Аналог для изучения: Myfxbook MT5 `EA Publisher`. В официальной справке Myfxbook описано, что для варианта EA Publisher пользователь устанавливает EA в терминал, размещает его на любом графике, разрешает нужные зависимости, а обновления счета идут примерно каждые 5 минут. Для ручной установки Myfxbook указывает `.ex5` в `MQL5/experts` и `.dll` в `MQL5/libraries`.

## Почему может быть ценно

- Может обойти проблему зависания Python `mt5.initialize()` на конкретном терминале.
- Может быть универсальнее для нескольких терминалов: каждый терминал публикует свой heartbeat/account snapshot.
- MQL5-скрипт находится внутри терминала и видит счет тем же способом, что обычный советник.
- Python-сервис может стать простым сборщиком/публикатором в Telegram без прямого управления терминалом.

## К чему относится

- `Automation_Scripts/TradingObserver`
- мониторинг MT5-счетов;
- будущая Telegram-публикация статуса;
- архитектура read-only наблюдения за счетом.

## Что проверить

- Можно ли в MQL5 безопасно читать все нужные поля счета: `login`, `server`, `name`, `balance`, `equity`, `profit`, `margin`, `margin_free`, `margin_level`, `positions_total`.
- Какой канал передачи проще и надежнее: файл в `Common Files`, локальный HTTP, socket, named pipe или WebRequest.
- Не требует ли схема DLL, повышенных прав или включения небезопасных настроек терминала.
- Как отличать несколько терминалов и счетов: `login + server + terminal path + heartbeat`.
- Как Python должен обнаруживать потерю связи: timestamp, heartbeat age, stale snapshot.
- Может ли прямой Python `MetaTrader5` остаться первым вариантом, а EA Publisher-схема быть fallback для терминалов, где `initialize()` зависает.

## Как понять, что идея сработала

- EA без торговых команд пишет актуальный account snapshot и heartbeat.
- Python читает snapshot без зависаний и публикует те же поля, что текущий этап TradingObserver.
- При закрытии терминала Python видит устаревший heartbeat и может сформировать тревогу.
- Решение не требует логина к кабинету брокера, браузерной автоматизации, торговых команд или управления счетом.

## Риски или сомнения

- MQL5 EA внутри терминала сложнее сопровождать, чем чистый Python-скрипт.
- Файловый обмен может дать stale data, если не контролировать timestamp.
- DLL/WebRequest-настройки могут быть нежелательны для первой версии.
- Нельзя копировать модель Myfxbook буквально: нужна своя read-only схема без передачи паролей и без публикации данных во внешний сервис без явного решения.

## Источник

- Идея пользователя: использовать подход, похожий на Myfxbook, где на терминал ставится небольшой EA/EX5 для публикации данных счета.
- Официальная справка Myfxbook по MT5 connection methods: https://www.myfxbook.com/help/knowledge-base/metatrader-5-connection-methods/

## Связанные материалы

- `Automation_Scripts/TradingObserver`
- `titan-bots-code-review / references/code-router.md`
- `titan-bots-code-review / references/python-script-standards.md`
