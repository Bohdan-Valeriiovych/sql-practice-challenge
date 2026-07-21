# Number of Shipments Per Month

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 2056

## Задача
Порахувати кількість відправлень по місяцях, де унікальний ключ відправлення — комбінація shipment_id і sub_id.

**Tables:** amazon_shipment

## Розв'язок

```sql
SELECT 
    TO_CHAR(shipment_date, 'YYYY-MM') AS year_month,
    COUNT(DISTINCT shipment_id || '-' || sub_id) AS shipment_count
FROM amazon_shipment
GROUP BY 1
ORDER BY 1;
```

## Підхід
`TO_CHAR()` форматує дату до формату YYYY-MM для групування по місяцях. Оскільки унікальний ключ складений з двох колонок, конкатеную їх у один рядок через `||` перед COUNT(DISTINCT ...).

## Що вивчив
Коли унікальність визначається комбінацією кількох колонок, а не однією, конкатенація в один рядок перед COUNT(DISTINCT ...) — простий спосіб порахувати унікальні складені ключі без CONCAT-функцій чи ROW_NUMBER.
