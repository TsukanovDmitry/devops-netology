Домашнее задание к занятию "6.2. SQL"

Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.

docker-compose.yaml
```yml
version: '3.6'
services:
  postgres:
    image: postgres:12
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

Задача 2
итоговый список БД после выполнения пунктов выше
```
test_db=# \l+
                                                                          List of databases
      Name       |  Owner   | Encoding |  Collate   |   Ctype    |       Access privileges        |  Size   | Tablesp
ace |                Description                 
-----------------+----------+----------+------------+------------+--------------------------------+---------+--------
----+--------------------------------------------
 netology-devops | postgres | UTF8     | en_US.utf8 | en_US.utf8 |                                | 7825 kB | pg_defa
ult | 
 postgres        | postgres | UTF8     | en_US.utf8 | en_US.utf8 |                                | 7969 kB | pg_defa
ult | default administrative connection database
 template0       | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres                   +| 7825 kB | pg_defa
ult | unmodifiable empty database
                 |          |          |            |            | postgres=CTc/postgres          |         |        
    | 
 template1       | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres                   +| 7825 kB | pg_defa
ult | default template for new databases
                 |          |          |            |            | postgres=CTc/postgres          |         |        
    | 
 test_db         | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =Tc/postgres                  +| 8121 kB | pg_defa
ult | 
                 |          |          |            |            | postgres=CTc/postgres         +|         |        
    | 
                 |          |          |            |            | "test-admin-user"=CTc/postgres+|         |        
    | 
                 |          |          |            |            | "test-simple-user"=c/postgres  |         |        
    | 
(5 rows)
```

описание таблиц (describe)
```
test_db=# \d+ clients
                                                        Table "public.clients"
   Column    |       Type        | Collation | Nullable |               Default               | Storage  | Stats targ
et | Description 
-------------+-------------------+-----------+----------+-------------------------------------+----------+-----------
---+-------------
 id          | integer           |           | not null | nextval('clients_id_seq'::regclass) | plain    |           
   | 
 second_name | character varying |           |          |                                     | extended |           
   | 
 country     | character varying |           |          |                                     | extended |           
   | 
 order       | integer           |           |          |                                     | plain    |           
   | 
Indexes:
    "clients_pkey" PRIMARY KEY, btree (id)
    "clients_country_idx" btree (country)
Foreign-key constraints:
    "fk_order" FOREIGN KEY ("order") REFERENCES orders(id)
Access method: heap
```
```
test_db=# \d+ orders 
                                                          Table "public.orders"
      Column      |       Type        | Collation | Nullable |              Default               | Storage  | Stats 
target | Description 
------------------+-------------------+-----------+----------+------------------------------------+----------+-------
-------+-------------
 id               | integer           |           | not null | nextval('orders_id_seq'::regclass) | plain    |       
       | 
 name | character varying |           |          |                                    | extended |              | 
 price        | integer           |           |          |                                    | plain    |           
   | 
Indexes:
    "orders_pkey" PRIMARY KEY, btree (id)
Referenced by:
    TABLE "clients" CONSTRAINT "fk_order" FOREIGN KEY ("order") REFERENCES orders(id)
Access method: heap
```

SQL-запрос для выдачи списка пользователей с правами над таблицами test_db

```
SELECT 
    grantee, table_name, privilege_type 
FROM 
    information_schema.table_privileges 
WHERE 
    grantee in ('test-admin-user','test-simple-user')
    and table_name in ('clients','orders')
order by 
    1,2,3;
 ```
 список пользователей с правами над таблицами test_db
 ```
     grantee      | table_name | privilege_type
------------------+------------+----------------
 test-admin-user  | clients    | DELETE
 test-admin-user  | clients    | INSERT
 test-admin-user  | clients    | REFERENCES
 test-admin-user  | clients    | SELECT
 test-admin-user  | clients    | TRIGGER
 test-admin-user  | clients    | TRUNCATE
 test-admin-user  | clients    | UPDATE
 test-admin-user  | orders     | DELETE
 test-admin-user  | orders     | INSERT
 test-admin-user  | orders     | REFERENCES
 test-admin-user  | orders     | SELECT
 test-admin-user  | orders     | TRIGGER
 test-admin-user  | orders     | TRUNCATE
 test-admin-user  | orders     | UPDATE
 test-simple-user | clients    | DELETE
 test-simple-user | clients    | INSERT
 test-simple-user | clients    | SELECT
 test-simple-user | clients    | UPDATE
 test-simple-user | orders     | DELETE
 test-simple-user | orders     | INSERT
 test-simple-user | orders     | SELECT
 test-simple-user | orders     | UPDATE
(22 rows)
```

Задача 3

INSERT INTO orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);
INSERT INTO clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');

select count(1) from public.clients
select count(1) from public.orders

```
test_db=# select count(1) from public.clients;
 count 
-------
     5
(1 row)
```
```
test_db=# select count(1) from public.orders; 
 count 
-------
     5
(1 row)
```

Задача 4

UPDATE clients SET "order" = (SELECT id FROM orders WHERE "name"='Книга') WHERE second_name ='Иванов Иван Иванович';
UPDATE clients SET "order" = (SELECT id FROM orders WHERE "name"='Монитор') WHERE "second_name"='Петров Петр Петрович';
UPDATE clients SET "order" = (SELECT id FROM orders WHERE "name"='Гитара') WHERE "second_name"='Иоганн Себастьян Бах';

SELECT c.* FROM clients c JOIN orders o ON c.order = o.id;

test_db=# SELECT c.* FROM clients c JOIN orders o ON c.order = o.id;
 id |              second_name               | country | order 
----+----------------------------------------+---------+-------
  1 | Иванов Иван Иванович | USA     |     3
  2 | Петров Петр Петрович | Canada  |     4
  3 | Иоганн Себастьян Бах | Japan   |     5
(3 rows)

Задача 5

test_db=# EXPLAIN SELECT c.* FROM clients c JOIN orders o ON c.order = o.id;
                               QUERY PLAN                               
------------------------------------------------------------------------
 Hash Join  (cost=37.00..57.24 rows=810 width=72)
   Hash Cond: (c."order" = o.id) - Проверка соответствия в кэше orders. В случае совпадения, строка попадет в выборку
   ->  Seq Scan on clients c  (cost=0.00..18.10 rows=810 width=72) - Считана таблица clients
   ->  Hash  (cost=22.00..22.00 rows=1200 width=4) - Создан кеш по полю id для таблицы orders
         ->  Seq Scan on orders o  (cost=0.00..22.00 rows=1200 width=4) -  Полностью считана таблица orders
(5 rows)

Задача 6

```bash
root@a540c2d91831:/# export PGPASSWORD=postgres && pg_dumpall -h localhost -U test-admin-user > /media/postgresql/backup/test_db.sql
root@a540c2d91831:/# ls /media/postgresql/backup/
test_db.sql
Tsukanovs-Air:~ tsukanovdmitry$ docker-compose stop
Stopping psql ... done
Tsukanovs-Air:~ tsukanovdmitry$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                     PORTS     NAMES
a540c2d91831   postgres:12   "docker-entrypoint.s…"   30 minutes ago   Exited (0) 9 seconds ago             psql
Tsukanovs-Air:~ tsukanovdmitry$ docker run --rm -d -e POSTGRES_USER=test-admin-user -e POSTGRES_PASSWORD=netology -e POSTGRES_DB=test_db -v /Users/tsukanovdmitry/docker/postgresql/pg_backup:/media/postgresql/backup --name psql2 postgres:12
Tsukanovs-Air:~ tsukanovdmitry$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                          PORTS      NAMES
d82a1de7ffc4   postgres:12   "docker-entrypoint.s…"   8 seconds ago    Up 7 seconds                    5432/tcp   psql2
a540c2d91831   postgres:12   "docker-entrypoint.s…"   31 minutes ago   Exited (0) About a minute ago              psql
Tsukanovs-Air:~ tsukanovdmitry$ docker exec -it psql2  bash
root@d82a1de7ffc4:/# ls /media/postgresql/backup/
test_db.sql
root@d82a1de7ffc4:/# export PGPASSWORD=postgres
root@d82a1de7ffc4:/# psql -h localhost -U test-admin-user -f /media/postgresql/backup/test_db.sql test_db
root@d82a1de7ffc4:/# psql -h localhost -U test-admin-user test_db
psql (12.10 (Debian 12.10-1.pgdg110+1))
Type "help" for help.


