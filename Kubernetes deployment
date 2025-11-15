# Kubernetes Deployment Project

## О проекте

*"Kubernetes упрощает развертывание и управление контейнеризированными приложениями в масштабе."*

Этот проект демонстрирует процесс деплоя веб-приложения в кластер Kubernetes. Приложение представляет собой простой веб-сервер, который отображает информацию о версии и хосте.

Назначение деплоя

1. Автоматизация развертывания - обеспечение повторяемости процесса
2. Масштабируемость - возможность легко увеличивать/уменьшать количество реплик
3. Отказоустойчивость - автоматическое восстановление при сбоях
4. Управление версиями - контроль за версиями развертываемого приложения

## Параметры Deployment

| Параметр                      | Значение по умолчанию | Описание |
|------------------------------|------------------------|----------|
| `replicas`                   | `3`                   | Количество реплик пода |
| `image`                      | `registry.example.com/user-service:v1.2.0` | Образ контейнера |
| `containerPort`              | `8080`                | Порт приложения внутри пода |
| `resources.limits.cpu`       | `500m`                | Максимальный CPU на под |
| `resources.limits.memory`    | `512Mi`               | Максимальная память на под |
| `resources.requests.cpu`     | `200m`                | Запрашиваемый CPU |
| `resources.requests.memory`  | `256Mi`               | Запрашиваемая память |
| `livenessProbe.initialDelaySeconds` | `30`          | Задержка перед первым лiveness-чеком |
| `readinessProbe.periodSeconds` | `10`              | Интервал проверки readiness |
| `strategy.type`              | `RollingUpdate`       | Стратегия обновления |
| `strategy.rollingUpdate.maxSurge` | `25%`         | Макс. избыточных подов при обновлении |
| `strategy.rollingUpdate.maxUnavailable` | `25%`   | Макс. недоступных подов при обновлении |

> **Примечание**: Все параметры можно переопределить через `values.yaml` при использовании Helm или напрямую в манифесте.

## 📄 Пример манифеста `deployment.yaml`

`yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: user-service
  namespace: production
  labels:
    app: user-service
    tier: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: user-service
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
  template:
    metadata:
      labels:
        app: user-service
        tier: backend
    spec:
      containers:
      - name: user-service
        image: registry.example.com/user-service:v1.2.0
        ports:
        - containerPort: 8080
          name: http
        env:
        - name: NODE_ENV
          value: "production"
        - name: DB_HOST
          valueFrom:
            secretKeyRef:
              name: db-creds
              key: host
        resources:
          limits:
            cpu: 500m
            memory: 512Mi
          requests:
            cpu: 200m
            memory: 256Mi
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 15
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 10
      imagePullSecrets:
      - name: regcred`

# Команды для применения и проверки

### Основная информация
kubectl get deployment user-service -n production

### Подробности (включая условия и события)
kubectl describe deployment user-service -n production

### Проверить поды
kubectl get pods -n production -l app=user-service

### Просмотр логов одного из подов
kubectl logs -n production -l app=user-service --tail=50

Подробнее с Kubernetes Deployment Project можно познакомиться по [ссылке](https://habr.com/ru/companies/gazprombank/articles/789404/)
