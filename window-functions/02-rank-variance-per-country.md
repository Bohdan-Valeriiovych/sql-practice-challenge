# Rank Variance Per Country

**Platform:** StrataScratch | **Difficulty:** Hard | **ID:** 2007

## Задача
Порівняти сумарну кількість коментарів по країнах у грудні 2019 та січні 2020, порангувати країни в кожному місяці, і повернути країни, чий ранг покращився (зменшився) від грудня до січня.

**Tables:** fb_comments_count, fb_active_users

## Розв'язок

```sql
WITH country_stats AS (
    SELECT
        b.country,
        EXTRACT(MONTH FROM c.created_at) AS month,
        SUM(c.number_of_comments) AS total_comments
    FROM fb_comments_count AS c
    INNER JOIN fb_active_users AS b ON c.user_id = b.user_id
    WHERE c.created_at BETWEEN '2019-12-01' AND '2020-01-31'
    GROUP BY b.country, EXTRACT(MONTH FROM c.created_at)
),
ranked_data AS (
    SELECT
        country,
        month,
        DENSE_RANK() OVER (PARTITION BY month ORDER BY total_comments DESC) AS rank_c
    FROM country_stats
)
SELECT jan_data.country
FROM ranked_data AS dec_data
INNER JOIN ranked_data AS jan_data
    ON dec_data.country = jan_data.country
WHERE
    dec_data.month = 12
    AND jan_data.month = 1
    AND jan_data.rank_c < dec_data.rank_c;
```

## Підхід
Спочатку рахую сумарну кількість коментарів по країнах окремо для кожного місяця, потім рангую країни всередині кожного місяця через `DENSE_RANK()`. Останній крок — self-join таблиці рангів самої на себе, щоб порівняти ранг однієї країни в грудні й січні напряму в одному рядку.

## Що вивчив
`DENSE_RANK()` не залишає "дірок" у нумерації при однакових сумах — важливо для точного порівняння позицій між періодами. Self-join працює, бо дані вже згруповані по (country, month) в одну таблицю.
