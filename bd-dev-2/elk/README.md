# Домашнее задание к занятию 5. «Elasticsearch» - Логинов Даниил

### Задача 1

Используя Docker-образ centos:7 как базовый и документацию по установке и запуску Elastcisearch:

* составьте Dockerfile-манифест для Elasticsearch,
* соберите Docker-образ и сделайте push в ваш docker.io-репозиторий,
* запустите контейнер из получившегося образа и выполните запрос пути / c хост-машины.

Требования к elasticsearch.yml:

* данные path должны сохраняться в /var/lib,
* имя ноды должно быть netology_test.

**В ответе приведите:**

* текст Dockerfile-манифеста,
* ссылку на образ в репозитории dockerhub,
* ответ Elasticsearch на запрос пути / в json-виде.

### Ответ 1

```Dockerfile
FROM centos:7

RUN yum -y update && \
    yum -y install java-1.8.0-openjdk && \
    yum clean all && \
    useradd -m -d /elasticsearch -s /bin/bash elasticsearch

COPY elasticsearch-8.12.0-linux-x86_64.tar.gz /tmp/

RUN tar -xzf /tmp/elasticsearch-8.12.0-linux-x86_64.tar.gz -C /usr/share/ && \
    mv /usr/share/elasticsearch-8.12.0 /usr/share/elasticsearch && \
    rm /tmp/elasticsearch-8.12.0-linux-x86_64.tar.gz

COPY elasticsearch.yml usr/share/elasticsearch/config/

RUN mkdir /usr/share/elasticsearch/data && \
    chmod +x /usr/share/elasticsearch/bin/* && \
    chown -R elasticsearch:elasticsearch  /usr/share/elasticsearch && \
    chmod -R 777 /var/lib/

ENV PATH /usr/share/elasticsearch/bin:$PATH

USER elasticsearch
WORKDIR /elasticsearch

CMD ["elasticsearch", "-Enetwork.host=0.0.0.0"]

EXPOSE 9200
```

[Cсылка на образ в репозитории dockerhub](https://hub.docker.com/repository/docker/loginachka/elasticsearch/general)

[Ответ Elasticsearch на запрос пути / в json-виде](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_1.png)

### Задача 2

Ознакомьтесь с документацией и добавьте в Elasticsearch 3 индекса в соответствии с таблицей:

| Имя 	| Количество реплик  |	Количество шард |
| ----- | ------------------ | ---------------- |
| ind-1 |	      0 	     |        1         |
| ind-2 |   	  1 	     |        2         |
| ind-3 |	      2          |        4         |


Получите список индексов и их статусов, используя API, и приведите в ответе на задание.

Получите состояние кластера Elasticsearch, используя API.

Как вы думаете, почему часть индексов и кластер находятся в состоянии yellow?

Удалите все индексы.

**В ответе приведите:**

* Cписок индексов и их статусов, используя API
* Cостояние кластера Elasticsearch, используя API
* Ответ на вопрос: Как вы думаете, почему часть индексов и кластер находятся в состоянии yellow?

### Ответ 2

[Cписок индексов и их статусов + Cостояние кластера Elasticsearch](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_2.png)

Статус "yellow" в Elasticsearch означает, что все шарды индекса доступны для поиска, но не все реплики завершены. Это может быть вызвано неудачным созданием реплик из-за недоступности узлов, где они должны были бы разместиться.

### Задача 3

В этом задании вы научитесь:

* создавать бэкапы данных,
* восстанавливать индексы из бэкапов.

Создайте директорию {путь до корневой директории с Elasticsearch в образе}/snapshots.

Используя API, зарегистрируйте эту директорию как snapshot repository c именем netology_backup.

Приведите в ответе запрос API и результат вызова API для создания репозитория.

Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.

Создайте snapshot состояния кластера Elasticsearch.

Приведите в ответе список файлов в директории со snapshot.

Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.

Восстановите состояние кластера Elasticsearch из snapshot, созданного ранее.

Приведите в ответе запрос к API восстановления и итоговый список индексов.

**В ответе приведите:**

* Запрос API и результат вызова API для создания репозитория
* Cписок индексов (test)
* Cписок файлов в директории со snapshot
* Список индексов (test-2)
* Запрос к API восстановления и итоговый список индексов

### Ответ 3

[Запрос API и результат вызова API для создания репозитория](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_3.png)

[Список индексов](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_7.png)

[Список файлов в директории со snapshot](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_4.png)

[Список индексов после удаления](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_5.png)

[Запрос к API восстановления и итоговый список индексов](https://github.com/Loginochka/bd-dev/blob/main/bd-dev-2/elk/media/Screenshot_6.png)