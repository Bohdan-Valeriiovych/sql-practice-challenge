# Average Salaries

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 9917

## Задача
Порівняти зарплату кожного співробітника із середньою зарплатою по його відділу.

**Tables:** employee

## Розв'язок

```sql
SELECT
    department,
    first_name,
    salary,
    AVG(salary) OVER (PARTITION BY department) AS avg_dept_salary
FROM employee;
```

## Підхід
`AVG() OVER (PARTITION BY department)` додає середнє значення відділу до кожного рядка без згортання таблиці — на відміну від `GROUP BY`, тут зберігається деталізація по кожному співробітнику.

## Що вивчив
Віконні функції — ідеальний інструмент, коли потрібно одночасно показати деталізований рядок і агрегат його групи в одному виводі.
