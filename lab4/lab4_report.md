## Лабораторная работа №4 

### Цель работы
Настроить мониторинг сервиса, запущенного в Kubernetes, с использованием Prometheus и Grafana, для обеспечения эффективного функционирования и своевременного обнаружения и устранения возможных проблем.

### Задачи
1. Установить Prometheus.
2. Установить Grafana.
3. Выбрать два рабочих графика, отражающих состояние системы.

### Ход работы

#### 1. Установка Prometheus
Через консоль был добавлен репозиторий Prometheus. 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Далее был установлен и запущен сервис.

```
helm install prometheus prometheus-community/prometheus
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np
```

Был открыт веб-интерфейс Prometheus.

```
minikube service prometheus-server-np
```

![1](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab4/image/1.png)

#### 2. Установка Grafana
Добавлен репозиторий Grafana. 

```
helm repo add grafana https://grafana.github.io/helm-charts
```

Установлен и запущен сервис.

```
helm install grafana grafana/grafana
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-np
```

Был открыт веб-интерфейс Grafana.

```
minikube service grafana-np
```

![2](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab4/image/2.png)

#### 3. Настройка
Далее произведена связка Grafana с Prometheus через Connections > Datasource.
Результат.

![3](https://github.com/Slabhide/itmo-cloud-systems/blob/main/lab4/image/3.png)

## Вывод
В ходе выполнения лабораторной работы были установлены и настроены сервисы Prometheos и Grafana, прилинкованы друг к другу для отображения данных кластера и создан на основе получаемых данных Dashboard. Во время выполнения работы проблем не возникло.
