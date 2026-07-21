# Monthly Percentage Difference

**Platform:** StrataScratch | **Difficulty:** Hard | **ID:** 10319

## Задача
Порахувати відсоткову зміну виручки місяць-до-місяця (MoM), з округленням до 2 знаків після коми, відсортовано по місяцях.

**Tables:** sf_transactions

## Розв'язок

```sql
WITH monthly_sales AS (
    SELECT
        TO_CHAR(created_at, 'YYYY-MM') AS month_date,
        SUM(value) AS current_revenue
    FROM sf_transactions
    GROUP BY 1
)
SELECT
    month_date,
    ROUND(
        CAST((current_revenue - LAG(current_revenue) OVER (ORDER BY month_date)) AS NUMERIC)
        / NULLIF(LAG(current_revenue) OVER (ORDER BY month_date), 0) * 100,
        2
    ) AS revenue_diff_pct
FROM monthly_sales
ORDER BY month_date ASC;
```

## Підхід
Спочатку групую транзакції по місяцях і рахую сумарну виручку. Потім через `LAG()` дістаю виручку попереднього місяця і рахую відсоткову зміну за стандартною формулою.

## Що вивчив
`NULLIF(x, 0)` захищає від ділення на нуль, якщо виручка попереднього місяця дорівнює нулю — без цього запит впав би з помилкою на такому рядку.
