# Лабораторная работа №2 "Kubernetes"

## Выполнил: 
Самсонов Станислав K34202

## Цель работы

Поднять kubernetes кластер локально, в нём развернуть свой сервис.

## Задачи

Использовать 2-3 ресурса kubernetes, поднять кластер minikube.

## Ход работы

Для поднятия локального Kubernetes кластера и развертывания своего сервиса были использованы Minikube и kubectl.

Код в файле hello-word-deployment.yaml представляет конфигурацию Kubernetes Deployment, который определяет, как развертывать и управлять подами.\

hello-word-deployment.yaml:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-world
          image: nginx:latest
          ports:
            - containerPort: 80

```

Сервис в Kubernetes предоставляет стабильный IP-адрес и DNS-имя для доступа к одному или нескольким подам.\

hello-world-service.yaml:

```yml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
spec:
  selector:
    app: hello-world
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

```bash
kubectl apply -f hello-world-deployment.yaml
kubectl apply -f hello-world-service.yaml
```

С помощью следующей команды был получен внешний IP и порт сервиса:

```bash
minikube ip
kubectl get service hello-world-service
```

Для проверки работы в браузере был открыт данный сайт:
![img5](./img/img5.png)

## Вывод

В результате работы был создан локальный Kubernetes кластер, развернут сервис и получен URL для доступа к сервису.
