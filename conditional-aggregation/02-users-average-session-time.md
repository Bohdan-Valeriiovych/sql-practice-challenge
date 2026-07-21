# Users By Average Session Time

**Platform:** StrataScratch | **Difficulty:** Medium | **ID:** 10352

## Задача
Порахувати середній час сесії користувача (різниця між page_load і page_exit одного дня, з урахуванням найпізнішого load і найранішого exit).

**Tables:** facebook_web_log

## Розв'язок

```sql
WITH session_data AS (
    SELECT
        user_id,
        timestamp::DATE AS day,
        MAX(CASE WHEN action = 'page_load' THEN timestamp END) AS start,
        MIN(CASE WHEN action = 'page_exit' THEN timestamp END) AS finish
    FROM facebook_web_log
    GROUP BY user_id, timestamp::DATE
)
SELECT
    user_id,
    AVG(EXTRACT(EPOCH FROM (finish - start))) AS duration
FROM session_data
WHERE finish IS NOT NULL AND start IS NOT NULL AND start < finish
GROUP BY user_id;
```

## Підхід
Групую події по користувачу й дню, використовуючи умовний MAX/MIN, щоб знайти найпізніший page_load і найраніший page_exit за день. Потім рахую різницю в секундах через EXTRACT(EPOCH FROM ...) і усереднюю по користувачах.

## Що вивчив
Комбінація умовної агрегації (`MAX(CASE WHEN ...)`) із групуванням по (user, day) — компактний спосіб "розгорнути" вертикальні події в горизонтальні пари start/finish без самоджойну.
