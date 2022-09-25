Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

```yml
version: '3.6'
services:
  mysql:
    image: mysql:8.0
    container_name: mysql
    ports:
      - "0.0.0.0:3306:3306"
    volumes:
      - /Users/tsukanovdmitry/docker/mysql/mysql:/var/lib/mysql
      - /Users/tsukanovdmitry/docker/mysql/backup:/media/mysql/backup
      - /Users/tsukanovdmitry/docker/mysql/conf.d:/etc/mysql/conf.d
    environment:
      MYSQL_DATABASE: "netology"
      MYSQL_ROOT_PASSWORD: "devops"
    restart: always
``` 
Изучите бэкап БД и восстановитесь из него.

```bash
bash-4.4# mysql -u root -p netology < /media/mysql/backup/test_dump.sql
Enter password: 
bash-4.4# 
```
