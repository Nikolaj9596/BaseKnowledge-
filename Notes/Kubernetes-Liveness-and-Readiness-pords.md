# The example yaml deployment with liveness and readiness pods

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - image: nginx:1.12
          name: nginx
        ports:
          - containerPort: 80
        # Проверка на способность обрабатывать трафик
        readinessProbe:

          # Доступно кол-во не удачных пробоб
          failureThreshold: 3
          httpGet:
            path: /
            port: 80
          # Как часто будет выполняться
          periodSeconds: 10
          # Обновление счетчика проб
          successThreshold: 1
          # Timeout пробы на выпольнение
          timeoutSeconds: 1
          # Через сколько запуск 
          initialDelaySeconds: 10

        # Проверка работает ли приложение
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /
            port: 80
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
```
