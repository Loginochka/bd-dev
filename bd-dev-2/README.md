# Домашнее задание к занятию 2. «SQL» - Логинов Даниил

### Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
Приведите получившуюся команду или docker-compose-манифест.


### Ответ 1

[Ссылка на compose sql_compose.yml](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/media/sql_compose.yml)

### Задача 2

В БД из задачи 1:

    создайте пользователя test-admin-user и БД test_db;
    в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);
    предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;
    создайте пользователя test-simple-user;
    предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Таблица orders:

    id (serial primary key);
    наименование (string);
    цена (integer).

Таблица clients:

    id (serial primary key);
    фамилия (string);
    страна проживания (string, index);
    заказ (foreign key orders).

Приведите:

    итоговый список БД после выполнения пунктов выше;
    описание таблиц (describe);
    SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;
    список пользователей с правами над таблицами test_db.

### Ответ 2

[Ссылка на Ответ 2](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/media/job_2.png)

### Задача 3

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:

Таблица 		orders

| Наименование | цена |
| :----------- | ---: |
| Шоколад 	   | 10   |
| Принтер 	   | 3000 |
| Книга 	   | 500  |
| Монитор 	   | 7000 |
| Гитара 	   | 4000 |

Таблица clients

| ФИО 				   | Страна проживания |
| :------------------- | ----------------: |
| Иванов Иван Иванович | USA         	   |
| Петров Петр Петрович | Canada			   |
| Иоганн Себастьян Бах | Japan             |
| Ронни Джеймс Дио 	   | Russia            |
| Ritchie Blackmore    | Russia            |

Используя SQL-синтаксис:
* вычислите количество записей для каждой таблицы.

Приведите в ответе:
- запросы,
- результаты их выполнения.

### Ответ 3

[Ссылка на Ответ 3](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/media/job_3.png)

### Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys, свяжите записи из таблиц, согласно таблице:

| ФИО 				    | Заказ    |
| :-------------------- | -------: | 
| Иванов Иван Иванович  | Книга    |
| Петров Петр Петрович  | Монитор  |
| Иоганн Себастьян Бах  | Гитара   |

Приведите SQL-запросы для выполнения этих операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.

Подсказка: используйте директиву UPDATE.

### Ответ 4

[Ссылка на Ответ 4](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/media/job_4.png)

### Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения.

### Ответ 4

Seq Scan on clients: Это означает, что PostgreSQL использует последовательное сканирование (по порядку) таблицы clients.
cost=0.00..10.70 rows=70 width=520: Это представляет оценочные затраты выполнения запроса. Более низкие значения обычно предпочтительны.
Filter: (заказ IS NOT NULL): Это фильтр, который применяется к таблице. В данном случае, выбираются только те строки, где значение в столбце заказ не является NULL.

### Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

Остановите контейнер с PostgreSQL, но не удаляйте volumes.

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления.

### Ответ 4

pg_dump -U test-admin-user -d test_db > /backups/test_db_backup.sql
docker stop postgresql
docker-compose.exe -f '.\sql_compose bkp.yml' up -d 
docker exec -it postgresql_bkp /bin/bash
psql -U test-admin-user -d test_db < /backups/test_db_backup.sql

[Ссылка на sql_compose bkp.yml](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/media/sql_compose%20bkp.yml)