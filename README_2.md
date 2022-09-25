Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

```yml
version: '3.6'
services:
  postgres:
    image: postgres:13
    container_name: psql
    ports:
      - "0.0.0.0:5432:5432"
    volumes:
      - /Users/tsukanovdmitry/docker/postgresql/pg_data:/var/lib/postgresql/data
      - /Users/tsukanovdmitry/docker/postgresql/pg_backup:/media/postgresql/backup
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
      POSTGRES_DB: "netology-devops"
    restart: always
```

```bash
root@7347969bd15c:/# psql -U postgres -h 127.0.0.1
psql (13.8 (Debian 13.8-1.pgdg110+1))
Type "help" for help.

postgres=# 
```
Найдите и приведите управляющие команды для:

вывода списка БД
```bash
postgres=# \l
                                    List of databases
      Name       |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------------+----------+----------+------------+------------+-----------------------
 netology-devops | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres        | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0       | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
                 |          |          |            |            | postgres=CTc/postgres
 template1       | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
                 |          |          |            |            | postgres=CTc/postgres
(4 rows)


```
подключения к БД
```bash
postgres=# \c netology-devops
You are now connected to database "netology-devops" as user "postgres".
netology-devops=# 
```
вывода списка таблиц
```bash
netology-devops=# \dtS
                    List of relations
   Schema   |          Name           | Type  |  Owner   
------------+-------------------------+-------+----------
 pg_catalog | pg_aggregate            | table | postgres
 pg_catalog | pg_am                   | table | postgres
 pg_catalog | pg_amop                 | table | postgres
 pg_catalog | pg_amproc               | table | postgres
 pg_catalog | pg_attrdef              | table | postgres
```
вывода описания содержимого таблиц
```bash
netology-devops=# \dS+ pg_type
                                       Table "pg_catalog.pg_type"
     Column     |     Type     | Collation | Nullable | Default | Storage  | Stats target | Description 
----------------+--------------+-----------+----------+---------+----------+--------------+-------------
 oid            | oid          |           | not null |         | plain    |              | 
 typname        | name         |           | not null |         | plain    |              | 
 typnamespace   | oid          |           | not null |         | plain    |              | 
```
выхода из psql
```bash
netology-devops-# \q
root@7347969bd15c:/# 
```

Задача 2
Используя psql создайте БД test_database.
Изучите бэкап БД.
Восстановите бэкап БД в test_database.
Перейдите в управляющую консоль psql внутри контейнера.
Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.
Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.
```bash
postgres=# create database test_database;
CREATE DATABASE

root@7347969bd15c:/# psql -U postgres -d test_database < /media/postgresql/backup/test_dump.sql
SET
SET
SET
SET
SET
 set_config 
------------
 
(1 row)

SET
SET
SET
SET
SET
SET
CREATE TABLE
ALTER TABLE
CREATE SEQUENCE
ALTER TABLE
ALTER SEQUENCE
ALTER TABLE
COPY 8
 setval 
--------
      8
(1 row)

ALTER TABLE
root@7347969bd15c:/# 



postgres=# \c test_database
You are now connected to database "test_database" as user "postgres".
test_database=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner   
--------+--------+-------+----------
 public | orders | table | postgres
(1 row)

test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
test_database=# 


test_database=# select avg_width from pg_stats where tablename='orders';
 avg_width 
-----------
         4
        16
         4
(3 rows)

test_database=#
```

Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

```bash
test_database=# alter table orders rename to orders_backup;
ALTER TABLE
test_database=# create table orders (id integer, title varchar(80), price integer) partition by range(price);
CREATE TABLE
test_database=# create table orders_less499 partition of orders for values from (0) to (499);
CREATE TABLE
test_database=# create table orders_more499 partition of orders for values from (499) to (999999999);
CREATE TABLE
test_database=# insert into orders (id, title, price) select * from orders_backup;
INSERT 0 8
```

Можно было избежать ручного разбиения, сделав таблицу шардированной на этапе проектирования. Это позволило бы не переносить данные вручную из старой таблицы

Задача 4
Используя утилиту pg_dump создайте бекап БД test_database.
Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

root@7347969bd15c:/# pg_dump -U postgres -d test_database > /media/postgresql/backup/test_database.sql
root@7347969bd15c:/# ls -la /media/postgresql/backup
total 12
drwxr-xr-x 4 root root  128 Sep 25 14:00 .
drwxr-xr-x 3 root root 4096 Sep 25 13:28 ..
-rw-r--r-- 1 root root 3569 Sep 25 14:00 test_database.sql
-rw-r--r-- 1 root root 2081 Sep 25 13:37 test_dump.sql
root@7347969bd15c:/# 

Для уникальности можно создать индекс по полю title
CREATE INDEX ON orders ((lower(title)));
