1. **CI/CD** — это процесс автоматизации сборки, тестирования и развертывания кода: CI (Continuous Integration) — частая интеграция кода, CD (Continuous Delivery/Deployment) — автоматизированное развертывание.

2. **Systemd демон**:
```ini
[Unit]
Description=Simple Service
After=network.target

[Service]
ExecStart=/usr/bin/your_process --option
WorkingDirectory=/app
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```
Сохранить в `/etc/systemd/system/your_service.service`, затем `systemctl enable your_service` и `systemctl start your_service`.

3. **Inode в Linux** — структура данных, хранящая метаданные файла (размер, права, расположение), но не имя или содержимое.

4. **Blue/Green в Kubernetes**:
- Создайте два `Deployment` (blue и green) с разными метками (`app: blue`, `app: green`).
- Настройте `Service` с селектором `app: blue` (или green для переключения).
- `Ingress` направляет трафик на `Service`.
- **Переключение**: Измените селектор `Service` на `app: green` (или обратно) с помощью `kubectl patch service <name> -p '{"spec":{"selector":{"app":"green"}}}'`.

5. **Политика AWS S3**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::your-bucket/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": ["203.0.113.0/24", "198.51.100.0/24"]
        }
      }
    }
  ]
}
```

6. **IaaS/PaaS/SaaS на примере пиццы**:
- **IaaS**: Аренда кухни (инфраструктура: плита, духовка), вы готовите пиццу сами.
- **PaaS**: Кухня с готовым тестом и ингредиентами, вы только собираете и печете.
- **SaaS**: Заказ пиццы с доставкой, ничего не готовите.

7. **Исправленный Dockerfile**:
```dockerfile
FROM node:18-slim
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY ./src ./
EXPOSE 3000
ENTRYPOINT ["npm"]
CMD ["run", "prod"]
```
Использует легкий образ, кэширует зависимости, устанавливает только продакшен-зависимости.

8. **Ограничение сетевого взаимодействия в Kubernetes**:
- Используйте `NetworkPolicy`.
- Пример:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: restrict-pod-access
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Ingress
  ingress:
  - from:
    - ipBlock:
        cidr: 192.168.0.0/16
    ports:
    - protocol: TCP
      port: 80
```
- **Включение**: Требуется сетевой плагин (например, Calico), поддерживающий `NetworkPolicy`. Включать отдельно не нужно, если плагин уже настроен.

9. **POSIX** — стандарт для обеспечения совместимости между Unix-подобными ОС, определяет API, утилиты и поведение системы.

10. **Типы DNS записей**:
- **A**: Сопоставляет домен с IPv4-адресом.
- **AAAA**: Сопоставляет домен с IPv6-адресом.
- **CNAME**: Псевдоним, перенаправляет на другой домен.
- **MX**: Указывает почтовые серверы домена.
- **TXT**: Хранит текстовую информацию (например, SPF).
- **NS**: Указывает серверы DNS для домена.
- **SRV**: Определяет хост и порт для сервисов.
- **PTR**: Обратное сопоставление IP с доменом.
