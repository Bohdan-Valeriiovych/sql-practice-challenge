# Salaries Differences

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10308

## Задача
Порахувати різницю між найвищими зарплатами в департаментах marketing та engineering. Вивести абсолютну різницю.

**Tables:** db_employee, db_dept

## Розв'язок

```sql
WITH highest_salary AS (
    SELECT
        d.department,
        MAX(de.salary) AS max_salary
    FROM db_employee de
    JOIN db_dept d ON de.department_id = d.id
    WHERE d.department IN ('engineering', 'marketing')
    GROUP BY d.department
)
SELECT ABS(
    (SELECT max_salary FROM highest_salary WHERE department = 'engineering') -
    (SELECT max_salary FROM highest_salary WHERE department = 'marketing')
) AS salary_difference;
```

## Підхід
Використав CTE (`highest_salary`), щоб спочатку згрупувати максимальну зарплату по департаментах окремо, а потім два скалярних підзапити для вибірки конкретних значень і обчислення різниці через ABS().

## Що вивчив
Можна було зробити через self-join замість CTE + subqueries, але CTE читається чистіше при двох конкретних категоріях.
