---
type: error
status: active
owner_layer: SYSTEM
terminal_owner: meta-system
source_module: DEEP
sections:
  - audit
  - system
  - navigation
applies_to:
  - audit
  - RAG
priority_score: 7
audit_key: deep-system:public-route-to-service-page
closure_condition: resolved by terminal owner and lasting lesson moved to rule/knowledge/architecture
updated: 2026-06-27
---

# DEEP-SYSTEM - публичный маршрут ведет на служебную страницу

## Оценка опасности

Priority score: 7.

## Проблема

Публичный answer-path ссылается на service page как на смысловую карточку. Технически ссылка корректна, но роль страницы не подходит для публичного knowledge-owner.

## Затронутые файлы

- Вики/TiTan Bots - Метод и продвижение/Трафик/SolidarTrading - роботы продают сами себя.md -> Вики/TiTan Bots - Метод и продвижение/FAQ/SolidarTrading - карта вопросов.md
- Вики/TiTan Bots - Метод и продвижение/Трафик/SolidarTrading - страницы захвата как инструмент воронки.md -> Вики/TiTan Bots - Метод и продвижение/FAQ/SolidarTrading - карта вопросов.md
- Вики/TiTan Bots - Метод и продвижение/Трафик/SolidarTrading - AI-помощник как инструмент объяснения и конверсии.md -> Вики/TiTan Bots - Метод и продвижение/FAQ/SolidarTrading - карта вопросов.md
- Вики/TiTan Bots - Метод и продвижение/FAQ/01_Что такое SolidarTrading и модель участия.md -> Вики/TiTan Bots - Метод и продвижение/Партнерка/Партнерка - Индекс.md
- Вики/TiTan Bots - Метод и продвижение/FAQ/01_Что такое SolidarTrading и модель участия.md -> Вики/TiTan Bots - Метод и продвижение/FAQ/SolidarTrading - карта вопросов.md

## Что должен сделать DEEP

Не выбирать новый owner автоматически. Переназначить маршрут только если есть однозначная каноническая карточка; иначе оставить одну сгруппированную Ошибку.

## Условие закрытия

Закрыть только после выполнения действия выше и сохранения lasting lesson в правиле, знании или architecture-note, если такой урок нужен.
