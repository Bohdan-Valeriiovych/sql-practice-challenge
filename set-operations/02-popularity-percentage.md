# Popularity Percentage

**Platform:** StrataScratch | **Difficulty:** Hard | **ID:** 10284

## Задача
Порахувати для кожного користувача відсоток його друзів відносно загальної кількості унікальних користувачів у системі.

**Tables:** facebook_friends

## Розв'язок

```sql
WITH all_connections AS (
    SELECT
        user1 AS member,
        user2 AS friend
    FROM facebook_friends
    UNION ALL
    SELECT
        user2,
        user1
    FROM facebook_friends
),
total_users_count AS (
    SELECT COUNT(DISTINCT member) AS total FROM all_connections
)
SELECT
    member,
    COUNT(friend) / (SELECT total FROM total_users_count)::FLOAT * 100 AS popularity_percent
FROM all_connections
GROUP BY 1
ORDER BY 1;
```

## Підхід
Таблиця дружби зберігає кожну пару лише один раз (user1-user2), тому через UNION ALL "дзеркалю" пари в обидва боки, щоб кожен користувач з'явився і як member, і як friend. Далі рахую загальну кількість унікальних людей і ділю кількість друзів кожного на це число.

## Що вивчив
UNION ALL для "дзеркалення" неорієнтованих зв'язків (де A-B і B-A логічно однакові, але фізично в таблиці лише один рядок) — типовий патерн для аналізу графів дружби/зв'язків у SQL.
