# Aleksgav_microservices
Aleksgav microservices repository

## Homework 20

  Ссылка на dockerhub: https://hub.docker.com/u/abirvalg/
  

## Homework 19

 - Создал правила файервола для пумы и прометеус
 - Создал инстанс docker-host в GCE
 - Запустил прометеус
 - Ознакомился с веб интерфейсом прометеуса
 - Создал Dockerfile для прометеуса
 - Сконфигурировал прометеус
 - Собрал images микросервисов
 - Добавил в docker-compouse сервис прометеуса
 - Запустил микросервисы с прометеусом
 - Сэмулировал проблему с микросервисом и ознакомился с выдачей в прометеусе
 - Добавил node-exporter

 Ссылка на dockerhub: https://hub.docker.com/u/abirvalg/

#### Задание со *
> Добавьте в Prometheus мониторинг MongoDB с использованием необходимого экспортера.

Добавил мониторинг mongo-db: monitoring/mongo_db_exporter

#### Задание со *
> Добавьте в Prometheus мониторинг сервисов comment, post, ui с помощью blackbox экспортера.

Добавил мониторинг сервисов с помощью blackbox экспортера: monitoring/blackbox_exporter

#### Задание со *
> Задание: Напишите Makefile

Написал makefile который билдит все образы и отправлет их в докерхаб


## Homework 18

 - Создал новый проект
 - Разрешил запуск раннеров в этом проекте
 - Описал окружения: staging, production
 - Добавил ограничение для staging и production - на них выкатывать только с тегом
 - Создал динамическое окружение

#### Задание со *
  При пуше новой ветки создается сервер для окружения.
  Сервер можно удалить кнопкой.


## HOMEWORK 17

 - Написал терраформ шаблон для развертывания окружения и развернул его
 - Написал энсибл плейбук для установки докера, настройки и установки gitlab и установил его
 - Зарегистрировался в созданном gitlab
 - Создал группу проектов
 - Создал проект
 - Добавил .gitlab-ci.yml
 - Запушил репозиторий в созданный гитлаб
 - Создал, запустил и зарегистрировал раннер
 - Добавил тестирование приложения reddit в pipeline
 - Изменил настройки в .gitlab-ci.yml для приложения reddit
 - Добавил тест приложения reddit
 - Отправил код в репозиторий и убедился что раннеры отработали

#### Задание со *

Продумайте автоматизацию развертывания и регистрации Gitlab CI Runner.

Для этого нужно перейти в папку gitlab-ci/ansible
Dыплнить команду:
```
ansible-playbook playbooks/gitlab_runners.yml --extra-vars "registration_token=XXXX"
```

#### Задание со *

Настройте интеграцию вашего Pipeline с тестовым Slack-чатом, который вы использовали ранее.

Оповещения настроены в канал: #aleksandr_gavrishchuk


Для запуска проекта нужно выполнить:
из папки gitlab-ci/terraform/prod
```
terraform apply
```
из папки gitlab-ci/ansible
```
ansible-playbook playbooks/docker_host.yml
```


## HOMEWORK 16

 - Запустил контейнер с none драйвером сети
 - Посмотрел конфигурацию сетевых интерфейсов
 - Запустил контейнер в пространстве докер хоста
 - Сравнил конфигурации сетевых интерфейсов хоста и контейнера
 - Посмотрел как меняются net неймспейсы при запуске при запуске сети используя none и хост драйвера
 - Создал бридж сеть reddit
 - Запустил проект в созданной бридж сети
 - Запустил проект в 2х бридж сетях
 - Посмотрел сетевой стек в созданной сети, iptables
 - Запустил приложение еспользуя docker-compose
 - Изменил конфигурацию под кейс с множестовм сетей и сетевых алиасов
 - Параметризовал конфигурацию
 - Создал override конфиг с запуском пумы в дебаг режиме с 2мя воркерами, и волюмами чтоб не пересобирать каждый раз контейнеры при изменении кода приложений

#### Задание со *

Узнайте как образуется базовое имя проекта. Можно ли его задать? Если можно то как?

Префикс образуется из имени папки в котором находится проект - так сделано для избежания конфликтов.
Задать его можно двумя путями (кроме переименования папки ))):
  - С помощью переменной окружения COMPOSE_PROJECT_NAME
  - С помощью флага -p, --project-name NAME


## HOMEWORK 15

 - Подключился к ранее созданному хосту
 - Распаковал микросервисное приложение
 - Создал докер файлы для:
  * post
  * comment
  * ui
 - Собрал образа с сервисами
 - Создал бридж сеть
 - Запустил контейнеры
 - Проверил - все ок
 - Оптимизировал образа
 - Создал docker volume и подключил его к mongo db
 - Перезапустил приложение - все посты остались


#### Задание со *

Остановил все контейнеры и запустил с другими сетевыми алиасами, проверил - все работает.

```
docker run -d --network=reddit \
  --network-alias=post_db_test --network-alias=comment_db_test mongo:latest
docker run -d --network=reddit \
  --network-alias=post_test -e "POST_DATABASE_HOST=post_db_test" abirvalg/post:1.0
docker run -d --network=reddit \
  --network-alias=comment_test -e "COMMENT_DATABASE_HOST=comment_db_test" abirvalg/comment:1.0
docker run -d --network=reddit \
  -p 9292:9292 -e "POST_SERVICE_HOST=post_test" -e "COMMENT_SERVICE_HOST=comment_test" abirvalg/ui:1.0
```

#### Задание со *

Пересобрал образа на основе alpine linux

#### Задание со **

Еще сильнее уменьшил размеры образов.
Главным счетом за счет использования чистого alpine linux, и удаления ненужных пакетов после сборки проекта.
Пример в репозитории.


## HOMEWORK 14
 - Создал новый проект в GCE
 - Поднял зост с докером
 - Поковырялся с PID namespace
 - Сравнил выводы: docker run --rm -ti tehbilly/htop || docker run --rm --pid host -ti tehbilly/htop
 - Создал докер фаил для контейнера с reddit monolith
 - Собрал образ на базе контейнера reddit-monolith
 - Запустил собранный контейнер
 - Отправил имэдж в докерхаб
 - Проверил что он доступен и работоспособен

#### Задание со *

 - Создал плейбук энсибл для установки докера - для пакер шаблона
 - Создал пакер шаблон, который делает образ с установленным Docker
 - Создал терраформ шаблон, поднимающий инстансы в зависимости от переменной. Используется образ с докером, ранее запеченный пакером.
 - Создал плейбук для деплоя докер контейнера, с динамическим инвентори, в поднятую инфраструктуру


## HOMEWORK 13
 - Установил Docker
 - Запустил контейнер hello world
 - Изучил команды:
    * docker info
    * docker version
    * docker ps
    * docker images
    * docker run
    * docker start
    * docker attach
    * docker create
    * docker exec
    * docker commit
    * docker inspect
    * docker kill
    * docker stop
    * docker system df
    * docker rm
    * docker rmi
