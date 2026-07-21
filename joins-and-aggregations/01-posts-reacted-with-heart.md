# Find all posts which were reacted to with a heart

**Platform:** StrataScratch | **Difficulty:** Easy | **ID:** 10087

## Задача
Знайти всі пости, на які реагували "серцем", і вивести всі колонки з таблиці постів.

**Tables:** facebook_reactions, facebook_posts

## Розв'язок

```sql
SELECT DISTINCT
    p.*
FROM facebook_reactions r
JOIN facebook_posts p
    ON r.post_id = p.post_id 
WHERE r.reaction = 'heart';
```

## Підхід
Простий INNER JOIN двох таблиць за post_id з фільтром за типом реакції. DISTINCT потрібен, бо один пост може мати кілька реакцій-сердець від різних користувачів, і без нього пост дублювався б у виводі.

## Що вивчив
DISTINCT після JOIN "один-до-багатьох" — важливо пам'ятати, коли з одного боку зв'язку може бути кілька рядків (тут: кілька реакцій на один пост).
