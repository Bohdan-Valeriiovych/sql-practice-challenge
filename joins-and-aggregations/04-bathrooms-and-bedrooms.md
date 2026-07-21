# Number Of Bathrooms And Bedrooms

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 9622

## Задача
Порахувати середню кількість ванних та спалень по містах і типах нерухомості.

**Tables:** airbnb_search_details

## Розв'язок

```sql
SELECT
    city,
    property_type,
    AVG(bathrooms) AS avg_bathrooms,
    AVG(bedrooms) AS avg_bedrooms
FROM airbnb_search_details
GROUP BY city, property_type;
```

## Підхід
Проста агрегація AVG() з групуванням по двох колонках — без JOIN, дані вже в одній таблиці.

## Що вивчив
Базовий приклад того, що не кожна задача потребує складних конструкцій — іноді просте GROUP BY з кількома AVG() і є правильним рішенням.
