# Лабораторная работа №1*

# Выполнил
Самсонов Станислав
К34202

## Цель работы

Создать два контейнера – в первом должно находиться приложение, которое принимает аргумент командной строки, во втором –
база данных, которая сохраняет эти аргументы.

## Задачи

* Написать приложение, которое будет сохранять записи в БД
* Написать Dockerfile, который соберет приложение в Docker-контейнер
* Написать docker-compose.yml, который будет ссылаться на уже написанный Dockerfile и создавать образ базы данных, в
  которой будут храниться записи из приложения.
* В docker-compose использовать volumes, чтобы записи не удалялись при перезапуске контейнера с БД

## Ход работы

В первую очередь было написано небольшое приложение на Go, подключающееся к БД, принимающее на вход аргумент командной
строки, и записывающее его в БД.

Далее был создан Dockerfile для контейнеризации приложения:

```
FROM golang:1.20-alpine as builder

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh build-base

COPY . /go/src/lab1

WORKDIR /go/src/lab1

ENV TZ=Europe/Moscow

RUN go mod download

RUN go build -o lab1

CMD ["./lab1", "Hello"]
```

В качестве сообщения используется строка "Hello". При отсутствии аргумента сохранится "Default message".

Далее был написан docker-compose, который запускает контейнер с приложением и создает контейнер с базой данных. В
качестве базы данных была выбрана PostgreSQL 13:

```
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      POSTGRES_USER: postgres
      POSTGRES_DB: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_HOST: db
      POSTGRES_PORT: 5432
    depends_on:
      - db
  db:
    image: postgres:13
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
volumes:
  db_data:
```

В docker-compose используются volumes, что позволяет хранить данные при перезапуске контейнера.

### Запуск

* Изменить сообщение в Dockerfile (аргумент командной строки)

* `docker-compose build`

* `docker-compose up`

### Скриншоты

#### Новый аргумент командной строки:

![1](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab1_*/images/1.png)

#### `docker-compose build`:

![2](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab1_*/images/2.png)

#### `docker-compose up`:

![3](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab1_*/images/3.png)

#### Записи в базе данных:

![4](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab1_*/images/4.png)
