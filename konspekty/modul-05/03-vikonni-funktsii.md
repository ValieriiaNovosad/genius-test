# 5.6–5.7. Віконні функції

> Модуль 5 · теорія + практика на `test3`  
> На співбесідах Middle+ — майже гарантовано

---

## Головна ідея

| | `GROUP BY` + агрегат | Window (`OVER`) |
|--|----------------------|-----------------|
| Рядки | Згортає в групи | **Зберігає** кожен рядок |
| Деталі | Втрачаєш trip/order id | Залишаються поруч із метрикою |

Паттерни: рейтинги, топи, дедуп, нумерація, running sum, rolling, `LAG`/`LEAD`.

```sql
SELECT
  customer_id,
  transaction_id,
  amount,
  ROW_NUMBER() OVER (
    PARTITION BY customer_id
    ORDER BY transaction_timestamp
  ) AS rn,
  SUM(amount) OVER (
    PARTITION BY customer_id
    ORDER BY transaction_timestamp
  ) AS running_sum,
  LAG(amount) OVER (
    PARTITION BY customer_id
    ORDER BY transaction_timestamp
  ) AS prev_amount
FROM `project.analytics.test3`;
```

`PARTITION BY` = вікно всередині групи (клієнт / країна).  
`ORDER BY` у вікні = хронологія подій.

---

## Функції з лекції / практики

| Функція | Кейс |
|---------|------|
| `ROW_NUMBER()` | унікальна нумерація; «перша покупка» (`rn = 1`); дедуп |
| `RANK()` / `DENSE_RANK()` | рейтинги з нічиїми |
| `LAG` / `LEAD` | попередня / наступна сума або дата |
| `SUM`/`AVG` `OVER` | накопичення, rolling |

Практика `test3`: merchants (retail, travel, restaurants, healthcare), currency, country, devices, channels.

Типовий патерн із CTE:

```sql
WITH ranked AS (
  SELECT
    *,
    ROW_NUMBER() OVER (
      PARTITION BY customer_id
      ORDER BY transaction_timestamp
    ) AS rn
  FROM `project.analytics.test3`
)
SELECT * FROM ranked WHERE rn = 1;  -- перша транзакція клієнта
```

---

## Підсумок модуля 5

Умієш: GCP/BigQuery, завантаження, типи (DATE/TIMESTAMP, ARRAY/STRUCT), вартість скану, partition/cluster, CTE/CASE/COALESCE, віконні функції для першої події, рейтингів і lag.

---

## Самоперевірка

1. Чому window не замінює повністю `GROUP BY`?  
2. `ROW_NUMBER` vs `RANK`?  
3. Як одним запитом знайти першу оплату кожного клієнта?
