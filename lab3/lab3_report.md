# Лабораторная работа №3 "Работа с CI/CD"

## Выполнил: 
Самсонов Станислав К34202

## Цель работы:
Сделать, чтобы после пуша в репозиторий автоматически собирался докер образ и результат его сборки сохранялся в Docker Hub.

## Задачи:
* Настроить работу CI/CD в рабочем репозитории. 
* Протестировать работу настроенной CI/CD. 

## Ход работы

### Настройка CI/CD

Для настройки CI/CD был выбран Github Action.

Был создан файл с разрешением .yml в папке .github/workflows/.

Содержимое созданного файла lab3.yml выглядят следующим образом:

```
name: lab3 

on:
  push:
    branches: ["main"]
    paths:
      - "lab3/**"

jobs:
  push-to-docker-hub:
    runs-on: ubuntu-22.04

    defaults:
      run:
        working-directory: "/lab3"

    steps:
      - name: checkout repository
        uses: actions/checkout@v4

      - name: dockerhub login
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_PASSWORD }}

      - name: build docker image and push to docker hub
        uses: docker/build-push-action@v5
        with:
          context: "./lab3/"
          push: true
          tags: slabhide/hello:latest
```

### Тестирование

Был выполнен пуш изменения в папку lab3 и выполнена проверка, что сценарий выполнился корректно:

1. Выполнение сценария:

![1]()

2. Проверим, запушился ли образ в Docker Hub:

![2]()



Все сработало успешно, а значит цель работы достигнута.

## Вывод:
В процессе выполнения лабораторной работы была установлена непрерывная интеграция и развертывание (CI/CD). Благодаря этому после отправки изменений в рабочий репозиторий автоматически создавался Docker-образ и сохранялся в Docker Hub.
