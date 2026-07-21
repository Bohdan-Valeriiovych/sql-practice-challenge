# Acceptance Rate By Date

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10285

## Задача
Порахувати відсоток прийнятих запитів у друзі для кожної дати відправлення (тільки для дат, де хоча б один запит був прийнятий).

**Tables:** fb_friend_requests

## Розв'язок

```sql
WITH a AS (
    SELECT * FROM fb_friend_requests
    WHERE action = 'accepted'
)
SELECT
    s.date,
    COUNT(a.user_id_receiver)::FLOAT / COUNT(s.user_id_sender) AS acceptance_rate
FROM (
    SELECT * FROM fb_friend_requests
    WHERE action = 'sent'
) AS s
LEFT JOIN a
    ON
        s.user_id_sender = a.user_id_sender
        AND s.user_id_receiver = a.user_id_receiver
GROUP BY s.date
HAVING COUNT(a.user_id_receiver) > 0;
```

## Підхід
Розділяю таблицю на дві частини — надіслані (`s`) і прийняті (`a`) запити, з'єдную через LEFT JOIN по парі відправник-отримувач, і рахую співвідношення прийнятих до надісланих по датах.

## Що вивчив
LEFT JOIN тут ключовий: він зберігає всі надіслані запити, навіть неприйняті (з NULL у частині `a`), що дозволяє коректно порахувати знаменник (усі надіслані) і чисельник (тільки прийняті) в одному запиті.
