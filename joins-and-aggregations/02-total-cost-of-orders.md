# Total Cost Of Orders

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10183

## Задача
Порахувати загальну вартість замовлень кожного клієнта, відсортовано за іменем.

**Tables:** customers, orders

## Розв'язок

```sql
SELECT
    c.id,
    c.first_name,
    SUM(o.total_order_cost) AS cost
FROM customers AS c
INNER JOIN orders AS o
    ON c.id = o.cust_id
GROUP BY 1, 2
ORDER BY c.first_name ASC;
```

## Підхід
Базовий INNER JOIN клієнтів і замовлень з групуванням по клієнту й сумуванням вартості замовлень.

## Що вивчив
Групування через номери колонок (`GROUP BY 1, 2`) — швидший спосіб писати запит, коли колонки для групування збігаються з колонками SELECT у тому ж порядку.
