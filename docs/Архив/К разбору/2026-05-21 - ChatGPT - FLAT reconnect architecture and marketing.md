---
type: chatgpt_manual_extract
source: ChatGPT
privacy_checked: true
relevance: высокая
confidence: высокая
needs_review: true
source_chat_title: неизвестно
related_bot: TiTan Hyperion (FLAT)
related_strategy: FLAT / Python / Backtrader
created_for: TiTan Bots
---

# FLAT reconnect architecture and marketing

## Статус обработки

Релевантность: высокая  
Уверенность: высокая  
Нужна ручная проверка: да  

Связанный бот:
- FLAT

Связанная стратегия:
- Python
- Backtrader
- reconnect architecture

## Obsidian links

- [TiTan Hyperion (FLAT)](../../../TiTan%20Hyperion%20(FLAT).md)
- [FLAT - Архитектура восстановления связи](FLAT%20-%20%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0%20%D0%B2%D0%BE%D1%81%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F%20%D1%81%D0%B2%D1%8F%D0%B7%D0%B8.md)
- [FLAT - VERIFY Gate](FLAT%20-%20VERIFY%20Gate.md)
- [FLAT - PROBE ROTATE](FLAT%20-%20PROBE%20ROTATE.md)
- [FLAT - Параметры стратегии](FLAT%20-%20%D0%9F%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B%20%D1%81%D1%82%D1%80%D0%B0%D1%82%D0%B5%D0%B3%D0%B8%D0%B8.md)
- [FLAT - Продажные формулировки](../../%D0%92%D0%B8%D0%BA%D0%B8/TiTan%20Bots%20-%20%D0%9C%D0%B5%D1%82%D0%BE%D0%B4%20%D0%B8%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B2%D0%B8%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5/%D0%9E%D1%84%D1%84%D0%B5%D1%80/FLAT%20-%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D0%B6%D0%BD%D1%8B%D0%B5%20%D1%84%D0%BE%D1%80%D0%BC%D1%83%D0%BB%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B8.md)

## Краткое резюме

В чате зафиксирована новая reconnect-архитектура FLAT v7/v9 для Python/Backtrader.

Главная идея:
- отказаться от websocket/store restart;
- использовать OMS heartbeat через probe-order;
- считать соединение восстановленным только через order status > Submitted.

Также сохранены:
- VERIFY-state machine;
- reconnect-логика;
- pause window;
- описание стратегии;
- продажные формулировки;
- офферы;
- параметры S/k/m/U/L/R/P;
- сценарии V1–V7.

---

# Точные факты

## Версии

- FLAT BUY v9.1
- FLAT SELL v9.0

## Платформа

- Python
- Backtrader

## Архитектурные ограничения

Запрещено:
- HTTP/REST;
- внешние библиотеки;
- обновление токенов;
- параллельные процессы;
- broker.start();
- broker.stop();
- websocket restart;
- store reconnect.

## Единственные reconnect-действия

### A1

- только логирование;
- pseudo RESUBSCRIBE;
- без реальных действий.

### A2

PROBE-ROTATE:
- CANCEL старого ping-order;
- PLACE нового ping-order.

## Trigger R

### R1
BROKER_CLOSE

### R2
DATA.DISCONNECTED

### R3
WS_CLOSED  
Только для совместимости.

### R4
pending_order существует на следующем next()

## Trigger T

### T2
Любой order status > Submitted.

Единственный доверенный trigger восстановления.

### T1
DATA.LIVE / BROKER_OPEN.

Только restore=DEFER.

## VERIFY параметры

- verify_max_attempts = 5
- backoff:
  - 30
  - 60
  - 90
  - 120
  - 150 sec

## Pause logic

### Summer
20:50 → 00:10 UTC

### Winter
21:50 → 00:10 UTC

## PROBE order

### BUY version
BUY LIMIT сильно ниже рынка:
price = close * 0.10

### SELL version
SELL LIMIT сильно выше рынка:
price = close * 10

---

# Справочники и соответствия

## Reconnect actions

- A1 → LOG / pseudo RESUBSCRIBE
- A2 → PROBE ROTATE

## Restore trust model

- T2_ORDER_STATUS → trusted restore
- T1_DATA_LIVE → defer only
- T1_BROKER_OPEN → defer only

## Нерабочие методы

Не использовать:
- broker.stop()
- broker.start()
- store.reconnect()
- store.restart_ws()
- store.connect()
- store.disconnect()
- start_stream()
- stop_stream()

Причины:
- freeze;
- dead threads;
- RuntimeError threads can only be started once.

---

# Идеи

## OMS-based reconnect

Соединение считается восстановленным только после реального статуса ордера.

## Probe-order heartbeat

Heartbeat через безопасный лимитный ордер вне рынка.

## VERIFY Gate

Единая reconnect state machine.

---

# Гипотезы

## OMS status надежнее LIVE feed

Статус:
- подтверждается архитектурой.

Проверка:
- disconnect/reconnect tests.

## Websocket restart бесполезен

Статус:
- частично подтверждено.

Причина:
- restart вызывал зависания и ошибки потоков.

---

# Выводы

- websocket/store restart нестабилен;
- OMS heartbeat оказался надежнее;
- T2_ORDER_STATUS — лучший reconnect signal;
- T1 LIVE недостаточен;
- FLAT позиционируется как market monetization system, а не direction bot.

---

# Ошибки и решения

## broker.start()

Ошибка:
RuntimeError threads can only be started once.

Решение:
- полностью отказаться.

## broker.stop()

Ошибка:
прайсы перестают оживать.

Решение:
- не использовать.

## Pending order зависает

Решение:
- trigger R4_PENDING_OBJECT.

---

# Правила

- Restore только через T2_ORDER_STATUS.
- Пока need_verify=True — торговля запрещена.
- VERIFY работает только через next().
- Не использовать websocket restart.
- Не доверять reconnect только по LIVE feed.

---

# Архитектура

## VERIFY state machine

Состояния:
- DETECT
- WAIT
- FAIL
- RESTORED

## Поток

1. DETECT R1–R4
2. need_verify=True
3. WAIT with backoff
4. A1 → A2 escalation
5. T2_ORDER_STATUS
6. RESTORED
7. sync_with_real_position()

## PROBE ROTATE

1. cancel old probe
2. place new probe
3. wait order status
4. restore by T2

---

# Боты и стратегии

## FLAT BUY

- Python
- Backtrader
- v9.1
- актуальный

## FLAT SELL

- Python
- Backtrader
- v9.0
- актуальный

---

# Формулы и параметры

## Step

step = price * (gridStepPerc / 100)

## PartialClose

P = U − L − R

## Главные параметры

### S = GridStep

- S↓:
  - больше сделок;
  - чаще тейки;
  - выше шанс выхода.

- S↑:
  - меньше сделок;
  - дольше удержание;
  - тейки реже.

### k = Buffer

- k↓:
  - защита чаще;
  - больше чистых тейков.

- k↑:
  - защита глубже;
  - включается реже.

### m = Trailing

- m↓:
  - быстрый BE;
  - больше ранних стопов.

- m↑:
  - позже подтягивание;
  - выше шанс V2–V5.

### U = UpperOrders

- U↑:
  - выше комиссии;
  - лучше защита;
  - лучше V2–V5.

### L = LowOrders

- L↑:
  - больше работы во флете;
  - хуже защита при выходе.

### R = ReserveOrders

- R↑:
  - лучше потенциал V4–V5.

---

# V-сценарии

## V1
Убыток по защите.

## V2
Защита + стоп.

## V3
Защита + прибыль.

## V4
Расширенный профит.

## V5
Максимальный профит.

## V6
Форс-мажор:
- FullStop;
- reconnect;
- разрыв связи.

## V7
Целевая прибыль.

---

# Метод и объяснение продукта

FLAT:
- не угадывает направление;
- зарабатывает во флете и импульсе;
- сочетает сетку и hedge/fork logic;
- позиционируется как новый класс стратегии.

---

# Офферы и продажные формулировки

## Формулировки

### “FLAT — новый класс стратегии”

Использование:
- презентация
- лендинг
- видео

---

### “FLAT не гадает направление — FLAT монетизирует рынок”

Использование:
- лендинг
- видео
- презентация

---

### “Берём деньги там, где другим скучно, и там, где другим больно”

Использование:
- branding
- посты
- видео

---

### “FLAT — бот, за который платят, чтобы им просто владеть”

Использование:
- FOMO-маркетинг
- Telegram
- лендинг

---

### “$275 только до 01.09. Потом? Готовь $1000+”

Использование:
- оффер
- запуск
- Telegram

Риск:
- завышенные ожидания.

---

### “Проходит челенджи и зарабатывает”

Использование:
- prop audience
- кейсы
- прогрев

---

# Контент и презентации

## Позиционирование

- FLAT как anti-guessing system.
- FLAT как hybrid grid + hedge.
- FLAT как prop challenge bot.
- FLAT как market monetization engine.

---

# Возражения и ответы

## “Это обычная сетка?”

Ответ:
- нет;
- это hybrid grid + smart hedge system.

## “Он угадывает рынок?”

Ответ:
- нет;
- FLAT монетизирует движение и диапазон.

---

# Кейсы и доказательства

Упоминалось:
- пользователи проходят challenge;
- пользователи оценивают бота выше $1000.

Подтверждений в чате:
- нет.

---

# Риски и ограничения

- reconnect architecture зависит от OMS;
- websocket restart признан нестабильным;
- нельзя обещать гарантированную прибыль;
- challenge claims требуют подтверждений;
- hype-formulations желательно сопровождать дисклеймерами.

---

# Что проверить дальше

- reconnect under disconnect;
- false restore cases;
- pending order recovery;
- OMS edge cases;
- latency impact.

---

# Что НЕ переносить в wiki

- генерацию изображений;
- личные данные;
- Telegram handle;
- бытовые темы;
- UI ChatGPT.

---

# Самопроверка полноты

- Все новые факты выписаны: да
- Все архитектурные решения выписаны: да
- Все reconnect-правила выписаны: да
- Все офферы и продажные формулировки выписаны: да
- Все параметры и V-сценарии выписаны: да
- Все ограничения выписаны: да