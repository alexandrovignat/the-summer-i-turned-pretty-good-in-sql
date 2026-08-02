# the-summer-i-turned-pretty-good-in-sql
hi! so i decided that i need to become smart and to find a really cool job in data engineering so now i studying mysql! watch me turn mega specialist and earn a dolla yay!
# Практика с индексами в MySQL

Этот проект создан для отработки навыков оптимизации SQL-запросов. Я провел эксперимент, чтобы наглядно увидеть, как индексы влияют на производительность.

## Задача
Сравнить скорость выполнения запроса `SELECT * FROM products WHERE category = 'Электроника' AND price BETWEEN 1000 AND 5000` с индексом и без него.

## Мои действия
1.  Создал тестовую таблицу `products`.
2.  Сгенерировал 100 000 случайных записей.
3.  Выполнил запрос и проанализировал его план через `EXPLAIN`.
4.  Создал составной индекс `CREATE INDEX idx_category_price ON products(category, price);`.
5.  Повторил запрос и снова посмотрел `EXPLAIN`.

## Результат
*   **Без индекса:** `rows = 100000`, `type = ALL`
*   **С индексом:** `rows = 12571`, `type = range`
*   **Вывод:** Использование индекса сократило количество просматриваемых строк в 8 раз, что значительно ускорило выполнение запроса.
