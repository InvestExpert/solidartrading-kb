# CE ZLSMA HA parity MT5 vs JS

Файл успешно подготовлен для импорта в Obsidian wiki TiTan Bots.

Основные темы:
- parity MT5 vs Pine;
- Heikin Ashi;
- CE;
- ZLSMA;
- Decision Gate;
- stopMode;
- EntryMode;
- online HA(0);
- closed HA(1);
- Persistent Signal;
- архитектура bar-only.

Главный вывод:
если useHeikinAshi=true, то ВСЯ стратегия должна использовать HA:
- CE;
- ZLSMA;
- flip;
- MA;
- сигналы;
- экстремумы;
- trailing.

Иначе parity TradingView ↔ MT5 ломается.
