# Apple Product Counts

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10141

## Задача
Порахувати кількість користувачів Apple-пристроїв та загальну кількість користувачів по мовах.

**Tables:** playbook_events, playbook_users

## Розв'язок

```sql
SELECT
    language,
    COUNT(DISTINCT CASE WHEN e.device IN ('macbook pro', 'iphone 5s', 'ipad air') THEN u.user_id END) AS apple_users,
    COUNT(DISTINCT u.user_id) AS total_users
FROM playbook_events AS e
LEFT JOIN playbook_users AS u
    ON e.user_id = u.user_id
GROUP BY 1
ORDER BY COUNT(DISTINCT u.user_id) DESC;
```

## Підхід
Комбінація `COUNT(DISTINCT CASE WHEN ...)` дозволяє порахувати унікальних користувачів Apple-пристроїв окремою колонкою поряд із загальною кількістю унікальних користувачів в одному GROUP BY.

## Що вивчив
`COUNT(DISTINCT CASE WHEN condition THEN id END)` — потужний патерн для сегментованого підрахунку унікальних значень без розбиття запиту на кілька підзапитів чи JOIN.
