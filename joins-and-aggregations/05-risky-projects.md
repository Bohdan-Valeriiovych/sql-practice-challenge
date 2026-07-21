# Risky Projects

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10304

## Задача
Знайти проєкти, які перевищують бюджет, враховуючи пропорційну (прорейтовану) вартість зарплат співробітників за період їх роботи на проєкті.

**Tables:** linkedin_projects, linkedin_emp_projects, linkedin_employees

## Розв'язок

```sql
SELECT
    p.title,
    p.budget,
    CEIL(SUM((e.salary / 365.0) * (p.end_date - p.start_date))) AS total_expense
FROM linkedin_projects AS p
INNER JOIN linkedin_emp_projects AS ep ON p.id = ep.project_id
INNER JOIN linkedin_employees AS e ON ep.emp_id = e.id
GROUP BY p.title, p.budget
HAVING SUM((e.salary / 365.0) * (p.end_date - p.start_date)) > p.budget;
```

## Підхід
З'єдную три таблиці (проєкти, зв'язок співробітник-проєкт, співробітники). Пропорційну зарплату рахую як (річна зарплата / 365) помножену на кількість днів роботи над проєктом, підсумовую по проєкту й фільтрую через HAVING ті, де сума перевищує бюджет.

## Що вивчив
HAVING потрібен замість WHERE, коли умова фільтрації стосується результату агрегації (SUM), а не окремого рядка — WHERE виконується до GROUP BY, HAVING — після.
