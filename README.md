Домашнее задание к занятию "6.5. Elasticsearch"

Задача 1

В этом задании вы потренируетесь в:

установке elasticsearch
первоначальном конфигурировании elastcisearch
запуске elasticsearch в docker
Используя докер образ centos:7 как базовый и документацию по установке и запуску Elastcisearch:

составьте Dockerfile-манифест для elasticsearch
соберите docker-образ и сделайте push в ваш docker.io репозиторий
запустите контейнер из получившегося образа и выполните запрос пути / c хост-машины
Требования к elasticsearch.yml:

данные path должны сохраняться в /var/lib
имя ноды должно быть netology_test

В ответе приведите:

текст Dockerfile манифеста
```yml
FROM centos:7

EXPOSE 9200 9300

USER 0

RUN export ES_HOME="/var/lib/elasticsearch" && \
    yum -y install wget && \
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.4.2-linux-x86_64.tar.gz && \
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.4.2-linux-x86_64.tar.gz.sha512 && \
    sha512sum -c elasticsearch-8.4.2-linux-x86_64.tar.gz.sha512 && \
    tar -xzf elasticsearch-8.4.2-linux-x86_64.tar.gz && \
    rm -f elasticsearch-8.4.2-linux-x86_64.tar.gz* && \
    mv elasticsearch-8.4.2 ${ES_HOME} && \
    useradd -m -u 1000 elasticsearch && \
    chown elasticsearch:elasticsearch -R ${ES_HOME} && \
    yum -y remove wget && \
    yum clean all

COPY --chown=elasticsearch:elasticsearch config/* /var/lib/elasticsearch/config/

USER 1000

ENV ES_HOME="/var/lib/elasticsearch" \
    ES_PATH_CONF="/var/lib/elasticsearch/config"
WORKDIR ${ES_HOME}

CMD ["sh", "-c", "${ES_HOME}/bin/elasticsearch"]
```
ссылку на образ в репозитории dockerhub

https://hub.docker.com/repository/docker/tsukanovda/elasticsearch

ответ elasticsearch на запрос пути / в json виде

```json
{
  "name" : "netology_test",
  "cluster_name" : "devops_netology",
  "cluster_uuid" : "kpSjjkMmRb6oYdsASrIeNQ",
  "version" : {
    "number" : "8.4.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "89f8c6d8429db93b816403ee75e5c270b43a940a",
    "build_date" : "2022-09-14T16:26:04.382547801Z",
    "build_snapshot" : false,
    "lucene_version" : "9.3.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

Задача 2

В этом задании вы научитесь:

создавать и удалять индексы
изучать состояние кластера
обосновывать причину деградации доступности данных
Ознакомтесь с документацией и добавьте в elasticsearch 3 индекса, в соответствии со таблицей:


Получите список индексов и их статусов, используя API и приведите в ответе на задание.
```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ curl 'localhost:9200/_cat/indices?v'
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   ind-1 LMT7xSEBRjavHjVhx5cRnQ   1   0          0            0       225b           225b
yellow open   ind-3 m1uOPkXpQ1afq2eWc5Xqyw   4   2          0            0       900b           900b
yellow open   ind-2 swki13N7RNqzoe14QgspIw   2   1          0            0       450b           450b
```
Получите состояние кластера elasticsearch, используя API.
```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$  curl -X GET "localhost:9200/_cluster/health?pretty"
{
  "cluster_name" : "devops_netology",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 8,
  "active_shards" : 8,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 44.44444444444444
}
Tsukanovs-Air:elasticsearch tsukanovdmitry$ 
```
Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

Эластик не может разместить копию шарда, поскольку праймари шарда и реплика не могут находиться на одном узле

Задача 3

В данном задании вы научитесь:

создавать бэкапы данных
восстанавливать индексы из бэкапов
Создайте директорию {путь до корневой директории с elasticsearch в образе}/snapshots.

Используя API зарегистрируйте данную директорию как snapshot repository c именем netology_backup.
```bash
curl -X PUT "localhost:9200/_snapshot/netology_backup?pretty" -H 'Content-Type: application/json' -d'
{
  "type": "fs",
  "settings": {
    "location": "/var/lib/elasticsearch/snapshots",
    "compress": true
  }
}'
{"acknowledged":true}
```

Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.

```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ curl -X PUT "localhost:9200/_snapshot/netology_backup?pretty" -H 'Content-Type: application/json' -d'
> {
>   "type": "fs",
>   "settings": {
>     "location": "/var/lib/elasticsearch/snapshots",
>     "compress": true
>   }
> }'
{
  "acknowledged" : true
}


Tsukanovs-Air:elasticsearch tsukanovdmitry$ curl 'localhost:9200/_cat/indices?v'
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test  UejZ4q-0Tse5ZBayTdNfxQ   1   0          0            0       225b           225b
green  open   ind-1 8XQjgUVQRE2NtzM7-kCq-g   1   0          0            0       225b           225b
yellow open   ind-3 0r-ZNnX3QKatTJjRznt-og   4   2          0            0       900b           900b
yellow open   ind-2 W3GuwajRRjO_cHGsofKJXA   2   1          0            0       450b           450b
```

Создайте snapshot состояния кластера elasticsearch.
Приведите в ответе список файлов в директории со snapshotами.
```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ docker exec -it elasticsearch ls -l /var/lib/elasticsearch/snapshots/
total 36
-rw-r--r-- 1 elasticsearch elasticsearch  1681 Oct  1 17:34 index-0
-rw-r--r-- 1 elasticsearch elasticsearch     8 Oct  1 17:34 index.latest
drwxr-xr-x 7 elasticsearch elasticsearch  4096 Oct  1 17:34 indices
-rw-r--r-- 1 elasticsearch elasticsearch 18519 Oct  1 17:34 meta-PuAssj-USRuAYGsXvS8jWQ.dat
-rw-r--r-- 1 elasticsearch elasticsearch   392 Oct  1 17:34 snap-PuAssj-USRuAYGsXvS8jWQ.da
```
Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.

```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ curl 'localhost:9200/_cat/indices?pretty'
green  open test-2 AeBFbelwSXmdFN4FCDfYXA 1 0 0 0 225b 225b
green  open ind-1  8XQjgUVQRE2NtzM7-kCq-g 1 0 0 0 225b 225b
yellow open ind-3  0r-ZNnX3QKatTJjRznt-og 4 2 0 0 900b 900b
yellow open ind-2  W3GuwajRRjO_cHGsofKJXA 2 1 0 0 450b 450b
Tsukanovs-Air:elasticsearch tsukanovdmitry$ 
```
Приведите в ответе запрос к API восстановления и итоговый список индексов.

```bash
curl -X POST "localhost:9200/_snapshot/netology_backup/snapshot_1/_restore?pretty" -H 'Content-Type: application/json' -d'
{
  "indices": "*",
  "include_global_state": true
}
'

Tsukanovs-Air:elasticsearch tsukanovdmitry$ curl 'localhost:9200/_cat/indices?pretty'
green  open test-2 AeBFbelwSXmdFN4FCDfYXA 1 0 0 0 225b 225b
green  open test   YKIZNsHAQ7eplcXWNUjJgw 1 0 0 0 225b 225b
green  open ind-1  fMw-K5-YR3iwByWR6QaSSA 1 0 0 0 225b 225b
yellow open ind-3  wmSkYdjyTUiHoUrYvNx4mA 4 2 0 0 900b 900b
yellow open ind-2  lM6A5iw8QdqfBnwjR_Cb3g 2 1 0 0 450b 450b
Tsukanovs-Air:elasticsearch tsukanovdmitry$ 
```


