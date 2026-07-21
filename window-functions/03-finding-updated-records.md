# Finding Updated Records

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10299

## Задача
Знайти актуальний запис зарплати кожного співробітника, вважаючи, що зарплата не спадає з часом (тобто найбільше значення — найактуальніше).

**Tables:** ms_employee_salary

## Розв'язок

```sql
WITH employees_salarys AS (
    SELECT
        *,
        DENSE_RANK() OVER (PARTITION BY id ORDER BY salary DESC) AS employe_salary
    FROM ms_employee_salary
)
SELECT
    id,
    first_name,
    last_name,
    department_id,
    salary
FROM employees_salarys
WHERE employe_salary = 1
ORDER BY id ASC;
```

## Підхід
Оскільки немає timestamp, а зарплата за умовою тільки зростає — найбільше значення зарплати для кожного `id` і є актуальним записом. Рангую записи всередині кожного співробітника за спаданням зарплати й беру перший.

## Що вивчив
Коли немає явного часового поля, але є монотонна властивість (тут — незменшувана зарплата), можна використати цю властивість як заміну сортуванню за часом.
