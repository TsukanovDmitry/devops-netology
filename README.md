Домашнее задание к занятию "6.2. SQL"

Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
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
