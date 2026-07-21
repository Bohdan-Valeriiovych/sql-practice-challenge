# New Products

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10318

## Задача
Порахувати різницю в кількості запущених продуктів між 2020 та 2019 роками для кожної компанії.

**Tables:** car_launches

## Розв'язок

```sql
SELECT
    company_name,
    COUNT(CASE WHEN year = 2020 THEN product_name END)
    - COUNT(CASE WHEN year = 2019 THEN product_name END) AS net_difference
FROM car_launches
GROUP BY company_name;
```

## Підхід
Використовую умовний `COUNT(CASE WHEN ...)`, щоб порахувати кількість продуктів окремо для кожного року в одному проході по таблиці, а потім віднімаю один від одного.

## Що вивчив
`COUNT(CASE WHEN condition THEN column END)` рахує тільки не-NULL значення, що дозволяє розкласти один GROUP BY на кілька умовних підрахунків без додаткових JOIN чи підзапитів.
