# Activity Rank

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10351

## Задача
Порангувати відправників за кількістю надісланих листів.

**Tables:** google_gmail_emails

## Розв'язок

```sql
SELECT
    from_user,
    c AS total_cnt,
    row_numb AS rank_1
FROM (
    SELECT
        from_user,
        COUNT(*) AS c,
        ROW_NUMBER() OVER (ORDER BY COUNT(*) DESC, from_user ASC) AS row_numb
    FROM google_gmail_emails
    GROUP BY from_user
) AS e_u
ORDER BY c DESC, from_user ASC;
```

## Підхід
Рахую кількість листів кожного відправника через GROUP BY, потім рангую через `ROW_NUMBER()` з подвійним сортуванням (за кількістю, потім за іменем) для стабільного і однозначного результату при однаковій кількості листів.

## Що вивчив
Другий критерій сортування (`from_user ASC`) всередині `ORDER BY` вікна робить ранжування детермінованим — без нього порядок рядків з однаковим count був би непередбачуваним.
