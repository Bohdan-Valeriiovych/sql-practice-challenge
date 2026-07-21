# Popularity of Hack

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10061

## Задача
Порахувати середню популярність опитування по локаціях працівників.

**Tables:** facebook_employees, facebook_hack_survey

## Розв'язок

```sql
SELECT
    location,
    AVG(popularity)
FROM facebook_employees AS e
INNER JOIN facebook_hack_survey AS h
    ON e.id = h.employee_id
GROUP BY 1;
```

## Підхід
Простий INNER JOIN двох таблиць з групуванням по локації та усередненням популярності.

## Що вивчив
Базовий приклад join + aggregation — хороша розминка перед складнішими задачами з умовною агрегацією чи віконними функціями.
