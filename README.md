Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"

Задача 1

1. https://hub.docker.com/r/tsukanovda/nginx_web

Задача 2

Высоконагруженное монолитное java веб-приложение;

В случае монолита, я бы отдал предпочтение физ серверам, поскольку не тратятся лишние ресурсы на виртуализацию

Nodejs веб-приложение;
Docker вполне подойдет, поскольку разработчик будет иметь возможность сложить все зависимости в один проект.

Мобильное приложение c версиями для Android и iOS;
Я бы отдал предпочтение в сторону виртуальных машин, поскольку это упростит установку и дальнейшее сопровождение приложения. В случае с докером придется придумывать какую-то интеграцию


Шина данных на базе Apache Kafka;

Да, но на тестовых средах. На боевых использовал бы ВМ и физ сервера в зависимости от нагрузки.


Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
Docker подойдёт лучше, поскольку проще выстроить кластеризацию.

Мониторинг-стек на базе Prometheus и Grafana;
Прекрасно подойдет, правда возникнет вопрос с экспортерами, поскольку например node_exporter требует доступ к ядру.

MongoDB, как основное хранилище данных для java-приложения;
Да, даже официальный образ есть.

Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.
Удобнее будет с ВМ. Не требуется много настройки, кластеризации и пр. Зато будет проше работать с файловой системой, например создавать бекапы

Задача 3

---bash---
Tsukanovs-Air:~ tsukanovdmitry$ docker run -it --rm -d --name centos -v $(pwd)/data:/data centos:latest
916f8257d1c1906abba4ad53b93417394198fbc23ac36317c09f670f42292218

Tsukanovs-Air:~ tsukanovdmitry$ docker run -it --rm -d --name debian -v $(pwd)/data:/data centos:latest
636f75f828e31b8618d4d2c2a8d9284802ca6cccceecc211d4759a263e642874

Tsukanovs-Air:~ tsukanovdmitry$ docker exec -it centos bash

[root@916f8257d1c1 /]# echo 'I am devops engineer!' > /data/text_file_1

[root@916f8257d1c1 /]# exit

exit

Tsukanovs-Air:~ tsukanovdmitry$ echo 'This is text file from host' > ./data/text_file_2
Tsukanovs-Air:~ tsukanovdmitry$ docker exec -it debian bash
[root@636f75f828e3 /]# cd /data
[root@636f75f828e3 data]# ls -la
total 12
drwxr-xr-x 4 root root  128 Aug 10 14:30 .
drwxr-xr-x 1 root root 4096 Aug 10 14:26 ..
-rw-r--r-- 1 root root   22 Aug 10 14:27 text_file_1
-rw-r--r-- 1 root root   28 Aug 10 14:30 text_file_2

