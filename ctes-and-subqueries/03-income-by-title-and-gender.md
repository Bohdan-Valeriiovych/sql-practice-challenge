# Income By Title and Gender

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10077

## Задача
Порахувати середню загальну компенсацію (зарплата + бонус) по посаді і статі, враховуючи тільки співробітників, які отримували бонус.

**Tables:** sf_employee, sf_bonus

## Розв'язок

```sql
WITH b AS (
    SELECT
        worker_ref_id,
        SUM(bonus) AS total_bonus
    FROM sf_bonus
    GROUP BY worker_ref_id
)
SELECT
    employee_title,
    sex,
    AVG(b.total_bonus + e.salary) AS avg_salary
FROM
    sf_employee AS e
INNER JOIN b ON e.id = b.worker_ref_id
GROUP BY 1, 2;
```

## Підхід
Спочатку сумую всі бонуси кожного співробітника окремо (CTE `b`), бо один співробітник міг отримати кілька бонусів. Потім INNER JOIN з таблицею співробітників автоматично відсіює тих, хто взагалі не отримував бонусу.

## Що вивчив
INNER JOIN — найпростіший спосіб реалізувати умову "тільки ті, хто має відповідний запис" без явного WHERE EXISTS чи фільтра на NULL.
