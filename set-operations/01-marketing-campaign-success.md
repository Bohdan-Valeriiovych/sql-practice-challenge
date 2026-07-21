# Marketing Campaign Success [Advanced]

**Platform:** StrataScratch | **Difficulty:** Hard | **ID:** 514

## Задача
Знайти користувачів, які в наступні дні після першої покупки придбали товари, яких не було серед покупок першого дня.

**Tables:** marketing_campaign

## Розв'язок

```sql
WITH basedata AS (
    SELECT
        user_id,
        product_id,
        created_at::DATE AS purchase_date,
        MIN(created_at::DATE) OVER (PARTITION BY user_id) AS first_day
    FROM marketing_campaign
),
newpurchases AS (
    SELECT
        user_id,
        product_id
    FROM basedata
    WHERE purchase_date > first_day
    EXCEPT
    SELECT
        user_id,
        product_id
    FROM basedata
    WHERE purchase_date = first_day
)
SELECT COUNT(DISTINCT user_id) AS successful_users
FROM newpurchases;
```

## Підхід
Визначаю дату першої покупки кожного користувача через MIN() OVER. Потім через EXCEPT віднімаю від "пар користувач-товар, куплених після першого дня" ті самі пари, що вже були куплені в перший день — залишаються тільки справді нові товари, придбані пізніше.

## Що вивчив
`EXCEPT` — елегантний спосіб знайти "нові" комбінації, яких не було в іншому наборі, без складних LEFT JOIN + IS NULL конструкцій. Працює як різниця множин на рівні пар колонок.
