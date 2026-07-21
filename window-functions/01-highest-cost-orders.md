# Highest Cost Orders

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 9915

## Задача
Знайти клієнта(ів) з найвищою сумою денних замовлень за кожен день у період з 2019-02-01 по 2019-05-01.

**Tables:** customers, orders

## Розв'язок

```sql
WITH daily_summaries AS (
    SELECT
        c.first_name,
        o.order_date,
        SUM(o.total_order_cost) AS daily_total
    FROM customers AS c
    INNER JOIN orders AS o ON c.id = o.cust_id
    WHERE o.order_date BETWEEN '2019-02-01' AND '2019-05-01'
    GROUP BY c.first_name, o.order_date
),
ranked_data AS (
    SELECT
        *,
        DENSE_RANK() OVER (PARTITION BY order_date ORDER BY daily_total DESC) AS rnk
    FROM daily_summaries
)
SELECT
    first_name,
    daily_total,
    order_date
FROM ranked_data
WHERE rnk = 1;
```

## Підхід
Спочатку рахую сумарні витрати клієнта по днях (CTE `daily_summaries`), потім рангую клієнтів всередині кожного дня через `DENSE_RANK()`, щоб коректно обробити випадки, коли кілька клієнтів мають однакову максимальну суму.

## Що вивчив
`DENSE_RANK()` замість `RANK()` — щоб не втратити клієнтів з однаковою сумою при виборі топ-1 за день.
