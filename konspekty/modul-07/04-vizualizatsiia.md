# 7.9–7.10. Візуалізація в Python

> Модуль 7 · matplotlib / seaborn (типовий стек курсу)

---

## Головна ідея

Графік у Python = швидкий EDA і відтворюваний звіт у notebook (не заміна Power BI для стейкхолдерів).

Типи:
- line — тренд;
- bar — порівняння категорій;
- hist / box — розподіл і викиди;
- scatter — зв’язок двох метрик;
- heatmap — кореляції / матриці.

Мінімальний патерн:
```python
import matplotlib.pyplot as plt
ax = g.plot(kind="bar", x="country", y="amount")
plt.title("Revenue by country")
plt.tight_layout()
```

Правила ті самі, що в Sheets/PBI: підписи осей, одиниці, не перевантажувати серії.

---

## Підсумок модуля 7

Синтаксис Python → цикли/функції → pandas як «SQL у коді» → графіки для дослідження.

## Самоперевірка

1. Коли seaborn зручніший за «голий» matplotlib?  
2. Notebook vs Power BI — хто для кого?
