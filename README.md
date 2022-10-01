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

