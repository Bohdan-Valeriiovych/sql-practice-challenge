# Workers With The Highest Salaries

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10353

## Задача
Знайти назву(и) посади працівника(ів) з найвищою зарплатою серед тих, хто має офіційний запис у таблиці `title`.

**Tables:** worker, title

## Розв'язок

```sql
WITH ranked_salaries AS (
    SELECT
        t.worker_title,
        DENSE_RANK() OVER (ORDER BY w.salary DESC) AS salary_rank
    FROM worker AS w
    INNER JOIN title AS t ON w.worker_id = t.worker_ref_id
)
SELECT worker_title
FROM ranked_salaries
WHERE salary_rank = 1;
```

## Підхід
INNER JOIN одразу відсіює працівників без запису в `title`. Далі рангую всіх через `DENSE_RANK()` за зарплатою і беру найвищий ранг — це коректно обробляє випадок кількох працівників з однаковою максимальною зарплатою.

## Що вивчив
INNER JOIN — простий і надійний спосіб реалізувати умову "тільки ті, хто має відповідний запис у другій таблиці", без додаткових EXISTS чи підзапитів.
