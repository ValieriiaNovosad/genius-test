# 5.4–5.5. Serverless DWH, партиції, CTE / CASE / COALESCE

> Модуль 5 · теорія архітектури + практика на транзакціях

---

## 5.4. Архітектура BigQuery

BigQuery = **Data Warehouse** без власних серверів: Google тримає storage і compute; ти платиш за обробку/зберігання.

| Принцип | Суть |
|---------|------|
| Storage layer | дані в хмарі Google, не на ноутбуці |
| Compute (Dremel) | запит дробиться на тисячі частин і йде паралельно |
| Columnar storage | скануються **обрані колонки** → дешевше й швидше |

Правило вартості: **не `SELECT *`**, а потрібні поля + фільтри по партиціях.

### Dataset / table SQL

```sql
CREATE SCHEMA IF NOT EXISTS analytics
OPTIONS (
  location = 'EU',
  description = 'production analytics',
  default_table_expiration_days = 365
);

-- DROP SCHEMA видаляє і всі таблиці всередині
```

Створення таблиць: порожня схема / `AS SELECT` / CSV / **Google Sheets** як джерело.

### Partition + cluster

- **Partition** (часто по даті) — фізичні блоки; `WHERE` по партиції зменшує ГБ скану (приклад лекції: −75%).  
- **Cluster** — сортування всередині партиції (напр. `user_id`) для швидшого пошуку.  
- `partition_expiration_days` — авто-прибирання старих партицій (напр. 90 днів).

```sql
CREATE TABLE analytics.events (
  event_id INT64,
  event_type STRING,
  user_id INT64,
  occurred_at TIMESTAMP
)
PARTITION BY DATE(occurred_at)
CLUSTER BY user_id
OPTIONS (partition_expiration_days = 90);
```

### Порядок виконання SQL

Пишемо: `SELECT → FROM/JOIN → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT`  
Логіка движка: спочатку джерела й JOIN → WHERE → GROUP → потім проєкція SELECT → ORDER/LIMIT.

### Синтаксичні must-have

| Конструкція | Навіщо |
|-------------|--------|
| `DISTINCT` | унікальні рядки до/без зайвих дублів |
| `HAVING` | фільтр **після** агрегації (`COUNT(*) > 10`) |
| `CASE WHEN` | сегменти (small/medium/large), умовні `COUNT` |
| `WITH` / CTE | читабельні кроки замість вкладених підзапитів (живуть лише в цьому job) |
| `CURRENT_DATE` / `DATETIME` / `TIMESTAMP` / `TIME` | динамічні вікна «останні N днів», звіти в TZ |
| `COALESCE(a, b, …)` | перше ненульове (NULL після JOIN, дефолт ціни, fallback дати) |

```sql
WITH user_2025 AS (
  SELECT * FROM `proj.ds.users`
  WHERE created_at >= '2025-01-01'
)
SELECT country, COUNT(*) AS user_count
FROM user_2025
GROUP BY country
HAVING COUNT(*) > 10;
```

```sql
CASE
  WHEN total_amount < 100 THEN 'small'
  WHEN total_amount < 500 THEN 'medium'
  WHEN total_amount < 2000 THEN 'large'
  ELSE 'enterprise'
END AS segment
```

---

## 5.5. Практика

Ієрархія в консолі: `project` → dataset `analytics` → таблиці `test2` / `test3` (транзакції: customer_id, amount, currency, country, city, card type, device, channel, **fraud flag**).

Задачі з уроку:
- розподіл типів карток / каналів;
- `CASE` по сумі;
- `COALESCE(fraud_flag, 0)` щоб NULL не ламали логіку;
- CTE для покрокової очистки й агрегації.

Перед запитом завжди дивись **схему** (типи STRING vs NUMERIC).

---

## Самоперевірка

1. Чому columnar + не `SELECT *` економить гроші?  
2. Partition vs cluster?  
3. Чому `HAVING`, а не `WHERE`, для `SUM(amount) > 5000`?
