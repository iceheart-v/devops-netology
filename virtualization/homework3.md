Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"


Задача 1
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

https://hub.docker.com/layers/176149229/iceheartv/netology-devops/nginx-custom/images/sha256-c1e044c4d5c558767b119ce52e69074dfc329d8eed2abfa0b90aad230c7fe68c?context=repo


Задача 2
Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"
Детально опишите и обоснуйте свой выбор.

--

Сценарий:

Высоконагруженное монолитное java веб-приложение;
- Не стоит, так как java любит в overhead и падает от OOM частенько, лучше поддерживать монолитный сервис без лишних прослоек.

Nodejs веб-приложение;
- Да, особенно будет полезно, если его можно масштабировать, чтобы обрабатывать возросшую нагрузку

Мобильное приложение c версиями для Android и iOS
- Если имеется в виду разработка и тестирование, то для андроида можно собирать тестовые машины, но ios насколько мне известно не будет работать в докере. Релизы приложений, разумеется, в отличном от докера формате.

Шина данных на базе Apache Kafka;
- Можно, если осторожно. Так как под капотом та же java, то придется копаться с настройками jvm, плюс для отказоустойчивости нам нужно будет устраивать kubernetes или swarm, иначе все партиции будут на той же виртуальной машине, хоть и в одном и том же докере. 
Но если заморочиться, то будут свои плюсы в части простоты конфигурации и поднятия всей инфраструктуры кафки.

Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
- Да, прекрасно себя чувствуют в докере, можно просто несоклько контейнеров на одной машине, или если предполагается отказоустойчивость, то swarm

Мониторинг-стек на базе Prometheus и Grafana;
- Да, отличное решение, докер позволит ограничить влияние телеметрии, на бизнес сервисы.

MongoDB, как основное хранилище данных для java-приложения;
- Существует миф о том, что не стоит использовать docker для баз данных, но в текущей версии докера проблем с mongo не должно быть

Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.
- Для docker registry это даже рекомендует сам докер, для gitlab тоже можно, есть подробная инструкция на сайте гитлаба. 


Задача 3
Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;
Добавьте еще один файл в папку /data на хостовой машине;
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

    root@vagrant:/home/vagrant/docker# docker ps
    CONTAINER ID   IMAGE     COMMAND        CREATED              STATUS              PORTS     NAMES
    a259e1d4ef28   ubuntu    "sleep 1000"   About a minute ago   Up About a minute             docker_ubuntu_1
    64c9b8e9fc49   debian    "sleep 1000"   About a minute ago   Up About a minute             docker_debian_1
    root@vagrant:/home/vagrant/docker# docker exec -it docker_ubuntu_1 bash
    root@a259e1d4ef28:/# cd data
    root@a259e1d4ef28:/data# touch somefile txt
    root@a259e1d4ef28:/data# exit
    exit
    root@vagrant:/home/vagrant/docker# docker exec -it docker_debian_1 bash
    root@64c9b8e9fc49:/# ls -la /data
    total 8
    drwxrwxr-x 2 1000 1000 4096 Nov  7 12:02 .
    drwxr-xr-x 1 root root 4096 Nov  7 12:00 ..
    -rw-r--r-- 1 root root    0 Nov  7 12:02 somefile
    -rw-r--r-- 1 root root    0 Nov  7 12:02 txt
    root@64c9b8e9fc49:/# rm /data/txt
    root@64c9b8e9fc49:/# exit
    exit
    root@vagrant:/home/vagrant/docker# cd data
    root@vagrant:/home/vagrant/docker/data# ll
    total 8
    drwxrwxr-x 2 vagrant vagrant 4096 Nov  7 12:02 ./
    drwxrwxr-x 3 vagrant vagrant 4096 Nov  7 12:00 ../
    -rw-r--r-- 1 root    root       0 Nov  7 12:02 somefile


Задача 4 (*)
Воспроизвести практическую часть лекции самостоятельно.
Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.

https://hub.docker.com/layers/176149712/iceheartv/netology-devops/ansible/images/sha256-618997d01d4b269943a9231ce5b18b94a9153d84e089c5bfaf1ce869f0a8ad26?context=repo


