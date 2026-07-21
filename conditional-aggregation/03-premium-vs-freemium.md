# Premium vs Freemium

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10300

## Задача
Знайти дати, коли кількість завантажень безкоштовними користувачами перевищувала кількість завантажень платними користувачами.

**Tables:** ms_user_dimension, ms_acc_dimension, ms_download_facts

## Розв'язок

```sql
SELECT
    date,
    non_paying_downloads,
    paying_downloads
FROM (
    SELECT
        f.date,
        SUM(CASE WHEN ad.paying_customer = 'no' THEN f.downloads ELSE 0 END) AS non_paying_downloads,
        SUM(CASE WHEN ad.paying_customer = 'yes' THEN f.downloads ELSE 0 END) AS paying_downloads
    FROM ms_download_facts AS f
    INNER JOIN ms_user_dimension AS ud ON f.user_id = ud.user_id
    INNER JOIN ms_acc_dimension AS ad ON ud.acc_id = ad.acc_id
    GROUP BY f.date
) AS subquery
WHERE non_paying_downloads > paying_downloads
ORDER BY date ASC;
```

## Підхід
З'єдную три таблиці, щоб дістати статус платного клієнта для кожного завантаження. Через умовний SUM розкладаю завантаження по датах на дві окремі колонки (платні/безкоштовні), а потім фільтрую результат у зовнішньому запиті.

## Що вивчив
Фільтр за агрегованими (обчисленими) колонками (`non_paying_downloads > paying_downloads`) не можна поставити в HAVING напряму без повторення виразів — простіше винести розрахунок у підзапит і фільтрувати WHERE зовні.
