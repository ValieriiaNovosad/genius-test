# 6.9–6.12. DAX, фінальний дашборд, БД

> Модуль 6 · за програмою + опис LMS

---

## 6.9–6.10. DAX

**DAX (Data Analysis Expressions)** — мова обчислень у Power BI.  
**Міри (measures)** — розрахунки в контексті фільтрів візуала (не плутати з calculated column по рядку).

```dax
Total Sales = SUM( FactSales[Amount] )
Orders = DISTINCTCOUNT( FactSales[OrderId] )
AOV = DIVIDE( [Total Sales], [Orders] )
```

Частина 2: робота з табличними даними / «пісочниця» для практики мір і фільтр-контексту (`CALCULATE`, time intel з Date table).

## 6.11. Фінальний дашборд

Підсумок модуля: зібрати власний дашборд з імпорту → Query → моделі → візуалів → мір.

Чеклист: чисті імена, KPI зверху, узгоджені slicers, звірка цифр із джерелом.

## 6.12. Підключення до БД

Підключити Power BI до бази (у курсі — та сама, що в модулі SQL): Import vs DirectQuery, отримати дані для аналізу без лише CSV.

---

## Підсумок модуля 6

BI-логіка → Desktop/інтерфейс → Power Query → модель → візуали/drill → DAX → фінальний звіт → SQL-джерело.

> Повні конспекти з відео: **6.1–6.2**. Уроки 6.3–6.12 на платформі без субтитрових доріжок — каркас з описів LMS + стандартний пайплайн PBI.

## Самоперевірка

1. Measure vs calculated column?  
2. Навіщо Date table для YoY?  
3. Import vs DirectQuery — коли що?
