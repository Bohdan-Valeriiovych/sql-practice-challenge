# Number Of Units Per Nationality

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10156

## Задача
Знайти кількість унікальних апартаментів, якими володіють господарі молодші 30 років, згруповано по національності.

**Tables:** airbnb_hosts, airbnb_units

## Розв'язок

```sql
SELECT
    h.nationality,
    COUNT(DISTINCT u.unit_id) AS apartment_count
FROM airbnb_hosts AS h
INNER JOIN airbnb_units AS u ON h.host_id = u.host_id
WHERE
    h.age < 30
    AND u.unit_type = 'Apartment'
GROUP BY h.nationality
ORDER BY apartment_count DESC;
```

## Підхід
INNER JOIN господарів і об'єктів нерухомості з фільтром за віком і типом нерухомості, підрахунок унікальних unit_id по національностях.

## Що вивчив
COUNT(DISTINCT ...) важливий тут, бо один host_id теоретично міг би з'явитись кілька разів при з'єднанні — явний DISTINCT гарантує коректний підрахунок унікальних об'єктів.
