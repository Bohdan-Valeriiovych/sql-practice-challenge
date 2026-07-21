# Order Details

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 9913

## Задача
Знайти деталі замовлень, зроблених клієнтами на ім'я Jill та Eva.

**Tables:** customers, orders

## Розв'язок

```sql
SELECT
    o.order_date,
    o.order_details,
    o.total_order_cost,
    c.first_name
FROM customers AS c
INNER JOIN orders AS o ON c.id = o.cust_id
WHERE c.first_name IN ('Jill', 'Eva')
ORDER BY c.id ASC;
```

## Підхід
INNER JOIN з фільтром за конкретними іменами через IN() замість кількох OR-умов.

## Що вивчив
`IN (...)` читається чистіше за ланцюжок `first_name = 'Jill' OR first_name = 'Eva'`, особливо коли список значень зростає.
