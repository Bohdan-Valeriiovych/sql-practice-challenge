# Bikes Last Used

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10176

## Задача
Знайти дату останнього використання кожного велосипеда.

**Tables:** dc_bikeshare_q1_2012

## Розв'язок

```sql
SELECT
    bike_number,
    MAX(end_time) AS last_used
FROM dc_bikeshare_q1_2012
GROUP BY bike_number
ORDER BY last_used DESC;
```

## Підхід
Проста агрегація MAX() по групі bike_number для знаходження останньої дати використання.

## Що вивчив
Базовий приклад, коли MAX()/MIN() з GROUP BY повністю замінює потребу у віконних функціях — вони потрібні тільки якщо треба зберегти деталізацію по рядках.
