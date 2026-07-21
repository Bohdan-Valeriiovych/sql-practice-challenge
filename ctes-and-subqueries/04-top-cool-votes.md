# Top Cool Votes

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10060

## Задача
Знайти текст відгуку з найбільшою кількістю "cool" голосів.

**Tables:** yelp_reviews

## Розв'язок

```sql
SELECT
    business_name,
    review_text
FROM
    yelp_reviews
WHERE
    cool = (SELECT MAX(cool) FROM yelp_reviews);
```

## Підхід
Скалярний підзапит знаходить максимальне значення `cool` по всій таблиці, а зовнішній запит фільтрує рядки, що відповідають цьому максимуму.

## Що вивчив
Простий і читабельний спосіб знайти рядок(и) з екстремальним значенням без window functions — доречний, коли не потрібне ранжування, а тільки сам максимум.
