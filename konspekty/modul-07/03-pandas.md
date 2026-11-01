# 7.7–7.8. Pandas

> Модуль 7 · дві частини

---

## Головна ідея

**DataFrame** ≈ таблиця SQL; **Series** ≈ колонка.

Типовий pipeline:
```python
df = pd.read_csv("orders.csv")
df.head()
df.info()
df["amount"] = pd.to_numeric(df["amount"], errors="coerce")
df = df.dropna(subset=["customer_id"])
g = df.groupby("country", as_index=False)["amount"].sum()
```

### Must-know API

| Дія | Методи |
|-----|--------|
| Фільтр | булева маска `df[df.x > 0]` |
| Select колонок | `df[["a","b"]]` |
| Group by | `groupby` + `agg` |
| Join | `merge` (how=left/inner) |
| Sort / top | `sort_values`, `nlargest` |
| Дати | `to_datetime`, `.dt` |
| NULL | `isna`, `fillna` |
| Reshape | `pivot_table`, `melt` |

Частина 2: складніші агрегації, multi-key merge, дублікати (`drop_duplicates`), віконні аналоги через `groupby().rank()` / `transform`.

```mermaid
flowchart LR
  CSV[CSV/SQL] --> DF[DataFrame]
  DF --> G[groupby / merge]
  G --> Viz[plot / export]
```

---

## Самоперевірка

1. `merge` how=left — аналог якого JOIN?  
2. Чому `errors="coerce"` у `to_numeric`?
