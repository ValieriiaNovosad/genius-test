# 6.9–6.12. DAX, фінальний дашборд, підключення до БД

> Модуль 6 · міри + публікація

---

## 6.9–6.10. Основи DAX

**Міра (measure)** рахується в контексті фільтрів візуала; **calculated column** — по рядку таблиці (дорожче в моделі).

База:
```dax
Total Sales = SUM( FactSales[Amount] )
Orders = DISTINCTCOUNT( FactSales[OrderId] )
AOV = DIVIDE( [Total Sales], [Orders] )
```

Контекст: `CALCULATE`, фільтри, `ALL` / `REMOVEFILTERS` для «% від усього».  
Time intel (з Date table): `DATEADD`, `SAMEPERIODLASTYEAR`, YTD.

Правило: бізнес-KPI = міри; сирі поля тягни в візуал лише для деталізації.

## 6.11. Фінальний дашборд

Чеклист:
- [ ] чиста модель і імена;
- [ ] KPI зверху, деталі знизу;
- [ ] slicers узгоджені;
- [ ] перевірені цифри vs джерело;
- [ ] зрозумілий title + одиниці.

## 6.12. Підключення до БД

Import vs DirectQuery: швидкість моделі vs live дані. Типові джерела: SQL Server, PostgreSQL, BigQuery/хмарні коннектори, Excel/CSV для прототипу.

---

## Підсумок модуля 6

Імпорт → Power Query → star schema → візуали → DAX-міри → дашборд → (опційно) live БД.

## Самоперевірка

1. Measure vs calculated column?  
2. Навіщо окрема Date table для YoY?
