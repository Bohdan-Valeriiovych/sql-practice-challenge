# Top 5 States With 5 Star Businesses

**Platform:** StrataScratch | **Difficulty:** Hard | **ID:** 10046

## Задача
Знайти топ-5 штатів за кількістю бізнесів з рейтингом 5 зірок.

**Tables:** yelp_business

## Розв'язок

```sql
SELECT
    state,
    n_businesses
FROM (
    SELECT
        state,
        COUNT(*) AS n_businesses,
        DENSE_RANK() OVER (ORDER BY COUNT(*) DESC) AS rank
    FROM yelp_business
    WHERE stars = 5
    GROUP BY state
) AS sub
WHERE rank <= 5
ORDER BY n_businesses DESC, state ASC;
```

## Підхід
Фільтрую тільки 5-зіркові бізнеси, групую по штатах і рахую кількість. `DENSE_RANK()` дозволяє коректно взяти "топ-5 місць" навіть якщо кілька штатів мають однакову кількість бізнесів на межі топ-5.

## Що вивчив
`DENSE_RANK() <= 5` — правильний спосіб взяти "топ-N груп за значенням", на відміну від `LIMIT 5`, який би довільно відрізав межові однакові значення.
