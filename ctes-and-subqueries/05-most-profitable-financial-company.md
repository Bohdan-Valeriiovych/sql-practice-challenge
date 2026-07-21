# Most Profitable Financial Company

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 9663

## Задача
Знайти найбільш прибуткову компанію фінансового сектору.

**Tables:** forbes_global_2010_2014

## Розв'язок

```sql
SELECT 
    company,
    continent
FROM forbes_global_2010_2014
WHERE profits = (
    SELECT MAX(profits) 
    FROM forbes_global_2010_2014 
    WHERE sector = 'Financials'
);
```

## Підхід
Підзапит знаходить максимальний прибуток серед компаній сектору Financials, зовнішній запит фільтрує за цим значенням і одразу за тим самим сектором.

## Що вивчив
Важливо продублювати умову `sector = 'Financials'` і в підзапиті, і мати на увазі, що фільтр застосовується до всієї таблиці окремо від зовнішнього запиту.
