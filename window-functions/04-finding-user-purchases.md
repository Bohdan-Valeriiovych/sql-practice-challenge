# Finding User Purchases

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10322

## Задача
Знайти користувачів, які здійснили другу покупку протягом 1-7 днів після першої покупки (без урахування покупок того ж дня).

**Tables:** amazon_transactions

## Розв'язок

```sql
SELECT DISTINCT user_id
FROM (
    SELECT
        user_id,
        created_at::DATE AS finish,
        MIN(created_at::DATE) OVER (PARTITION BY user_id) AS start
    FROM amazon_transactions
) AS sub
WHERE (finish - start) BETWEEN 1 AND 7;
```

## Підхід
Для кожної транзакції визначаю дату першої покупки користувача через `MIN() OVER (PARTITION BY user_id)`, а потім фільтрую рядки, де різниця між поточною датою і першою покупкою потрапляє в діапазон 1-7 днів.

## Що вивчив
`MIN() OVER (PARTITION BY ...)` дозволяє додати "найменше значення групи" до кожного рядка без згортання таблиці через GROUP BY — зручно, коли потрібно порівняти рядок із агрегатом його ж групи.
