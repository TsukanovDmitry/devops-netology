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
