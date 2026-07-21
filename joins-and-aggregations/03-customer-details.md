# Customer Details

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 9891

## Задача
Вивести ім'я, прізвище, місто клієнта та деталі його замовлень.

**Tables:** customers, orders

## Розв'язок

```sql
SELECT
    c.first_name,
    c.last_name,
    c.city,
    o.order_details
FROM customers AS c
LEFT JOIN orders AS o ON c.id = o.cust_id
ORDER BY c.first_name ASC, o.order_details ASC;
```

## Підхід
LEFT JOIN замість INNER, щоб зберегти клієнтів навіть без жодного замовлення (тоді order_details буде NULL).

## Що вивчив
Вибір LEFT JOIN vs INNER JOIN залежить від того, чи потрібно зберігати "порожні" записи з лівої таблиці — тут умова не уточнює явно, але LEFT JOIN безпечніший за замовчуванням, якщо не сказано інакше.
