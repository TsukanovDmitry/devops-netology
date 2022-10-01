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

