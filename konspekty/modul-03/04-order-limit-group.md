# 3.8–3.9. ORDER BY, LIMIT і агрегації

> Модуль 3

---

## 3.8. Сортування та обмеження

```sql
SELECT *
FROM employees.departments
ORDER BY dept_name ASC;  -- або DESC
```

`LIMIT n` — перші n рядків після сортування.  
`OFFSET` — пропустити n (пагінація).

Порядок виконання (розширений):  
`FROM → WHERE → SELECT → ORDER BY → LIMIT/OFFSET`

Без `ORDER BY` «топ-10» **недетермінований**.

---

## 3.9. Агрегації та GROUP BY

Агрегація = багато рядків → один показник.

| Функція | Сенс |
|---------|------|
| `COUNT(*)` / `COUNT(col)` | Кількість |
| `SUM` | Сума |
| `AVG` | Середнє |
| `MIN` / `MAX` | Екстремуми |

```sql
SELECT dept_no, AVG(salary) AS avg_salary
FROM employees.salaries
GROUP BY dept_no;
```

Правило: у `SELECT` лише агрегати або поля з `GROUP BY`.  
Фільтр **після** агрегації — `HAVING` (не `WHERE`).

Аналітику рідко потрібні «сирі тисячі рядків» — потрібні *скільки / середнє / максимум*.

---

## Самоперевірка

1. Чому LIMIT без ORDER BY небезпечний для звіту «топ»?  
2. WHERE vs HAVING?  
3. Що не можна писати в SELECT разом із GROUP BY?
