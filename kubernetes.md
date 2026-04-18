[Вопросы для собеседования](README.md)

# Kubernetes
+ [Что такое Kubernetes и зачем он нужен?](#что-такое-kubernetes-и-зачем-он-нужен)
+ [Чем Kubernetes отличается от Docker Compose и Docker Swarm?](#чем-kubernetes-отличается-от-docker-compose-и-docker-swarm)
+ [Какова архитектура кластера Kubernetes?](#какова-архитектура-кластера-kubernetes)
+ [Что такое Pod?](#что-такое-pod)
+ [Каков жизненный цикл Pod?](#каков-жизненный-цикл-pod)
+ [Что такое Init-контейнеры и Sidecar-контейнеры?](#что-такое-init-контейнеры-и-sidecar-контейнеры)
+ [Что такое ReplicaSet?](#что-такое-replicaset)
+ [Что такое Deployment и какие стратегии обновления существуют?](#что-такое-deployment-и-какие-стратегии-обновления-существуют)
+ [Что такое Service и какие типы существуют?](#что-такое-service-и-какие-типы-существуют)
+ [Что такое Ingress и Ingress Controller?](#что-такое-ingress-и-ingress-controller)
+ [Что такое Namespace и зачем он нужен?](#что-такое-namespace-и-зачем-он-нужен)
+ [Что такое ConfigMap и Secret?](#что-такое-configmap-и-secret)
+ [Как передать конфигурацию в Java/Spring Boot приложение через ConfigMap и Secret?](#как-передать-конфигурацию-в-javaspring-boot-приложение-через-configmap-и-secret)
+ [Какие типы Volumes существуют в Kubernetes?](#какие-типы-volumes-существуют-в-kubernetes)
+ [Что такое PersistentVolume и PersistentVolumeClaim?](#что-такое-persistentvolume-и-persistentvolumeclaim)
+ [Что такое Liveness, Readiness и Startup Probes?](#что-такое-liveness-readiness-и-startup-probes)
+ [Что такое Requests и Limits для ресурсов?](#что-такое-requests-и-limits-для-ресурсов)
+ [Что такое Horizontal Pod Autoscaler (HPA)?](#что-такое-horizontal-pod-autoscaler-hpa)
+ [Какие основные команды kubectl нужно знать?](#какие-основные-команды-kubectl-нужно-знать)
+ [Как выглядит YAML-манифест для деплоя Java/Spring Boot приложения?](#как-выглядит-yaml-манифест-для-деплоя-javaspring-boot-приложения)
+ [Что такое Helm?](#что-такое-helm)
+ [Как задеплоить Java-приложение в Kubernetes?](#как-задеплоить-java-приложение-в-kubernetes)
+ [Как организовать мониторинг и логирование в Kubernetes?](#как-организовать-мониторинг-и-логирование-в-kubernetes)
+ [Какие особенности запуска JVM-приложений в Kubernetes нужно учитывать?](#какие-особенности-запуска-jvm-приложений-в-kubernetes-нужно-учитывать)

## Что такое Kubernetes и зачем он нужен?

**Kubernetes** (K8s) — это платформа с открытым исходным кодом для автоматизации развёртывания, масштабирования и управления контейнеризированными приложениями. Разработана компанией Google и передана в Cloud Native Computing Foundation (CNCF).

### Основные задачи, которые решает Kubernetes:

+ **Оркестрация контейнеров** — управление сотнями и тысячами контейнеров, распределённых по множеству серверов.
+ **Автоматическое масштабирование** — увеличение или уменьшение количества экземпляров приложения в зависимости от нагрузки.
+ **Самовосстановление** — если контейнер упал, Kubernetes автоматически перезапустит его или создаст новый.
+ **Балансировка нагрузки** — распределение трафика между экземплярами приложения.
+ **Управление конфигурацией и секретами** — безопасная передача настроек и паролей в приложения.
+ **Обновление без простоя** (Rolling Update) — плавное обновление приложения без остановки сервиса.
+ **Service Discovery** — автоматическое обнаружение сервисов внутри кластера.

### Зачем Java-разработчику знать Kubernetes?

В современной микросервисной архитектуре Java/Spring Boot приложения часто деплоятся как Docker-контейнеры, а Kubernetes управляет их жизненным циклом в продакшн-среде. Понимание Kubernetes позволяет:
+ правильно конфигурировать приложение для работы в кластере;
+ настраивать health-check'и (liveness/readiness probes);
+ управлять ресурсами (CPU, память);
+ диагностировать проблемы на проде.

[к оглавлению](#kubernetes)

## Чем Kubernetes отличается от Docker Compose и Docker Swarm?

| Характеристика | Docker Compose | Docker Swarm | Kubernetes |
|---|---|---|---|
| **Назначение** | Запуск нескольких контейнеров на одной машине (разработка) | Простая оркестрация в кластере | Полноценная оркестрация для продакшена |
| **Масштабирование** | Ручное, ограниченное | Базовое | Автоматическое (HPA, VPA) |
| **Самовосстановление** | Нет (только `restart: always`) | Базовое | Продвинутое (health checks, rolling updates) |
| **Service Discovery** | DNS внутри docker-compose сети | Встроенный | Встроенный, гибкий (Service, Ingress) |
| **Балансировка нагрузки** | Нет (нужен внешний балансировщик) | Встроенная | Встроенная, с множеством вариантов |
| **Сложность** | Низкая | Средняя | Высокая |
| **Подходит для** | Локальной разработки | Небольших кластеров | Продакшена любого масштаба |
| **Экосистема** | Минимальная | Ограниченная | Огромная (Helm, Istio, Prometheus и т.д.) |

### Когда что использовать:

+ **Docker Compose** — для локальной разработки: поднять БД, Kafka, Redis и ваше приложение одной командой.
+ **Docker Swarm** — для небольших проектов, где нужен простой кластер без сложной конфигурации.
+ **Kubernetes** — для продакшена, когда требуется надёжность, масштабируемость, автоматизация и богатая экосистема.

[к оглавлению](#kubernetes)

## Какова архитектура кластера Kubernetes?

Кластер Kubernetes состоит из **Master Node** (управляющий узел, Control Plane) и **Worker Nodes** (рабочие узлы).

### Master Node (Control Plane)

Управляющий узел отвечает за состояние всего кластера и принятие решений:

+ **API Server (`kube-apiserver`)** — центральный компонент, точка входа для всех REST-запросов. Через него работает `kubectl`, другие компоненты кластера и внешние системы. Все операции (создание Pod, Service и т.д.) проходят через API Server.

+ **etcd** — распределённое хранилище типа «ключ-значение». Хранит всё состояние кластера: информацию о нодах, подах, конфигурации, секретах. Это «источник истины» для кластера.

+ **Scheduler (`kube-scheduler`)** — отвечает за назначение Pod'ов на Worker Node. При создании нового Pod'а Scheduler анализирует доступные ресурсы на нодах, affinity/anti-affinity правила, taints/tolerations и выбирает оптимальный узел.

+ **Controller Manager (`kube-controller-manager`)** — запускает контроллеры, которые следят за состоянием кластера и приводят его к желаемому состоянию. Примеры контроллеров:
  - **ReplicaSet Controller** — поддерживает нужное количество реплик Pod'а.
  - **Deployment Controller** — управляет обновлениями приложений.
  - **Node Controller** — следит за состоянием нод.
  - **Job Controller** — управляет одноразовыми задачами.

### Worker Node

Рабочий узел, на котором выполняются контейнеры с приложениями:

+ **kubelet** — агент, работающий на каждой Worker Node. Получает от API Server описание Pod'ов, которые должны запускаться на данной ноде, и обеспечивает их запуск и работу. Отправляет статусы обратно в API Server.

+ **kube-proxy** — сетевой прокси на каждой ноде. Обеспечивает сетевое взаимодействие между Pod'ами и Service'ами. Управляет правилами iptables/IPVS для маршрутизации трафика.

+ **Container Runtime** — среда выполнения контейнеров. Kubernetes поддерживает несколько рантаймов через стандарт CRI (Container Runtime Interface): `containerd`, `CRI-O`. Docker как рантайм был исключён начиная с Kubernetes 1.24, но Docker-образы по-прежнему полностью совместимы.

### Схема взаимодействия

```
                   +-------------------+
                   |    Master Node    |
                   |  (Control Plane)  |
                   |                   |
  kubectl -------->|  API Server       |
                   |  Scheduler        |
                   |  Controller Mgr   |
                   |  etcd             |
                   +--------+----------+
                            |
              +-------------+-------------+
              |                           |
     +--------v--------+        +--------v--------+
     |  Worker Node 1  |        |  Worker Node 2  |
     |                  |        |                  |
     |  kubelet         |        |  kubelet         |
     |  kube-proxy      |        |  kube-proxy      |
     |  Container RT    |        |  Container RT    |
     |                  |        |                  |
     |  [Pod] [Pod]     |        |  [Pod] [Pod]     |
     +-----------------+        +-----------------+
```

[к оглавлению](#kubernetes)

## Что такое Pod?

**Pod** — это минимальная единица развёртывания в Kubernetes. Pod представляет собой группу из одного или нескольких контейнеров, которые:

+ разделяют общее сетевое пространство (один IP-адрес);
+ могут разделять общие тома (volumes);
+ запускаются и управляются как единое целое.

### Ключевые особенности:

+ Pod получает **один IP-адрес** внутри кластера. Все контейнеры внутри Pod'а общаются друг с другом через `localhost`.
+ Pod — это **эфемерная сущность**. Он может быть уничтожен и пересоздан в любой момент. Поэтому нельзя полагаться на постоянный IP-адрес Pod'а.
+ Обычно в Pod'е запускается **один основной контейнер**. Несколько контейнеров используются для вспомогательных задач (sidecar pattern).

### Пример манифеста Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-java-app
  labels:
    app: my-java-app
spec:
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      ports:
        - containerPort: 8080
      resources:
        requests:
          memory: "256Mi"
          cpu: "250m"
        limits:
          memory: "512Mi"
          cpu: "500m"
```

> На практике Pod'ы редко создаются напрямую. Обычно используют **Deployment**, который управляет Pod'ами через ReplicaSet.

[к оглавлению](#kubernetes)

## Каков жизненный цикл Pod?

Pod проходит через несколько фаз (phases) за время своей жизни:

| Фаза | Описание |
|---|---|
| **Pending** | Pod принят кластером, но один или несколько контейнеров ещё не готовы к запуску. Pod ждёт назначения на ноду, скачивания образов и т.д. |
| **Running** | Pod назначен на ноду, все контейнеры созданы. Как минимум один контейнер выполняется или находится в процессе запуска/перезапуска. |
| **Succeeded** | Все контейнеры в Pod'е успешно завершились и не будут перезапущены. Типично для Job'ов. |
| **Failed** | Все контейнеры завершились, и хотя бы один завершился с ошибкой (ненулевой exit code). |
| **Unknown** | Состояние Pod'а не удалось определить, обычно из-за проблем с связью с нодой. |

### Состояния контейнера внутри Pod:

+ **Waiting** — контейнер ожидает (скачивание образа, ожидание зависимостей).
+ **Running** — контейнер запущен и работает.
+ **Terminated** — контейнер завершил выполнение (успешно или с ошибкой).

### Политики перезапуска (restartPolicy):

+ **Always** (по умолчанию) — всегда перезапускать контейнер.
+ **OnFailure** — перезапускать только при ошибке (ненулевой exit code).
+ **Never** — никогда не перезапускать.

### Graceful Shutdown:

При удалении Pod'а Kubernetes:
1. Отправляет сигнал `SIGTERM` контейнеру.
2. Ждёт **terminationGracePeriodSeconds** (по умолчанию 30 секунд).
3. Если контейнер не остановился — отправляет `SIGKILL`.

Для Spring Boot приложений важно корректно обрабатывать `SIGTERM`, чтобы завершить текущие запросы. Spring Boot 2.3+ поддерживает graceful shutdown через настройку `server.shutdown=graceful`.

[к оглавлению](#kubernetes)

## Что такое Init-контейнеры и Sidecar-контейнеры?

### Init-контейнеры

**Init-контейнеры** — это специальные контейнеры, которые запускаются **до** основных контейнеров Pod'а. Они выполняются последовательно, каждый следующий стартует только после успешного завершения предыдущего.

**Типичные сценарии использования:**
+ Ожидание готовности базы данных или другого сервиса перед стартом приложения.
+ Загрузка конфигурации или секретов из внешнего хранилища.
+ Выполнение миграций базы данных (Liquibase, Flyway).
+ Настройка файловой системы или прав доступа.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  initContainers:
    - name: wait-for-db
      image: busybox:1.36
      command: ['sh', '-c', 'until nc -z postgres-service 5432; do echo "Waiting for DB..."; sleep 2; done']
    - name: run-migrations
      image: my-registry/my-app-migrations:1.0.0
      command: ['java', '-jar', 'migrations.jar']
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      ports:
        - containerPort: 8080
```

### Sidecar-контейнеры

**Sidecar-контейнеры** — это дополнительные контейнеры, которые работают **параллельно** с основным контейнером в одном Pod'е. Они расширяют функциональность основного контейнера.

**Типичные сценарии использования:**
+ **Логирование** — сбор и отправка логов (Fluentd, Filebeat).
+ **Прокси** — Envoy/Istio sidecar для service mesh.
+ **Мониторинг** — экспорт метрик.
+ **Синхронизация данных** — загрузка конфигурации из внешнего источника.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-with-sidecar
spec:
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      ports:
        - containerPort: 8080
      volumeMounts:
        - name: logs
          mountPath: /var/log/app
    - name: log-collector
      image: fluent/fluent-bit:latest
      volumeMounts:
        - name: logs
          mountPath: /var/log/app
          readOnly: true
  volumes:
    - name: logs
      emptyDir: {}
```

В этом примере основной контейнер пишет логи в общий том, а sidecar-контейнер `fluent-bit` читает их и отправляет в систему логирования.

[к оглавлению](#kubernetes)

## Что такое ReplicaSet?

**ReplicaSet** — это контроллер, который гарантирует, что указанное количество одинаковых Pod'ов (реплик) запущено в любой момент времени.

### Основные функции:

+ Поддерживает заданное количество реплик Pod'а (поле `replicas`).
+ Если Pod упал или был удалён — автоматически создаёт новый.
+ Если Pod'ов больше, чем нужно — удаляет лишние.
+ Использует **label selector** для определения, какие Pod'ы принадлежат данному ReplicaSet.

### Пример:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-java-app
  template:
    metadata:
      labels:
        app: my-java-app
    spec:
      containers:
        - name: app
          image: my-registry/my-java-app:1.0.0
          ports:
            - containerPort: 8080
```

> **Важно:** На практике ReplicaSet не создают напрямую. Вместо этого используют **Deployment**, который управляет ReplicaSet'ами автоматически и предоставляет возможности обновления и отката.

[к оглавлению](#kubernetes)

## Что такое Deployment и какие стратегии обновления существуют?

**Deployment** — это основной объект для управления развёртыванием stateless-приложений в Kubernetes. Deployment управляет ReplicaSet'ами, которые, в свою очередь, управляют Pod'ами.

### Возможности Deployment:

+ Декларативное обновление приложений.
+ Откат (rollback) к предыдущей версии.
+ Масштабирование (изменение количества реплик).
+ Пауза и возобновление развёртывания.
+ Хранение истории ревизий.

### Стратегии обновления:

#### 1. RollingUpdate (по умолчанию)

Плавное обновление: новые Pod'ы создаются, старые удаляются постепенно. Приложение остаётся доступным во время обновления.

Параметры:
+ **maxSurge** — сколько дополнительных Pod'ов можно создать сверх желаемого количества (абсолютное число или процент). По умолчанию 25%.
+ **maxUnavailable** — сколько Pod'ов может быть недоступно одновременно. По умолчанию 25%.

#### 2. Recreate

Все старые Pod'ы удаляются, затем создаются новые. Приводит к кратковременному простою. Используется, когда одновременная работа двух версий невозможна (например, из-за миграций БД).

### Пример Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-java-app
  labels:
    app: my-java-app
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-java-app
  template:
    metadata:
      labels:
        app: my-java-app
    spec:
      containers:
        - name: app
          image: my-registry/my-java-app:1.0.0
          ports:
            - containerPort: 8080
```

### Откат (Rollback):

```bash
# Посмотреть историю ревизий
kubectl rollout history deployment/my-java-app

# Откатить к предыдущей версии
kubectl rollout undo deployment/my-java-app

# Откатить к конкретной ревизии
kubectl rollout undo deployment/my-java-app --to-revision=2

# Проверить статус обновления
kubectl rollout status deployment/my-java-app
```

[к оглавлению](#kubernetes)

## Что такое Service и какие типы существуют?

**Service** — это абстракция, которая определяет логическую группу Pod'ов и политику доступа к ним. Service обеспечивает стабильный сетевой эндпоинт (IP-адрес и DNS-имя) для набора Pod'ов, даже если сами Pod'ы пересоздаются и их IP-адреса меняются.

### Зачем нужен Service?

Pod'ы — эфемерны. Они могут быть уничтожены и созданы заново с другим IP. Service решает эту проблему, предоставляя постоянный адрес и балансировку нагрузки.

### Типы Service:

#### 1. ClusterIP (по умолчанию)

Создаёт виртуальный IP-адрес, доступный **только внутри кластера**. Используется для взаимодействия между сервисами внутри кластера.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-java-app-service
spec:
  type: ClusterIP
  selector:
    app: my-java-app
  ports:
    - port: 80
      targetPort: 8080
```

Другие сервисы в кластере могут обращаться по адресу `http://my-java-app-service:80` или `http://my-java-app-service.namespace.svc.cluster.local:80`.

#### 2. NodePort

Открывает фиксированный порт (30000-32767) на **каждой ноде** кластера. Трафик, поступающий на `<NodeIP>:<NodePort>`, перенаправляется на Service.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-java-app-nodeport
spec:
  type: NodePort
  selector:
    app: my-java-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30080
```

#### 3. LoadBalancer

Создаёт **внешний балансировщик нагрузки** (в облачных провайдерах — AWS ELB, GCP Load Balancer и т.д.). Это основной способ выставить сервис в интернет в облаке.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-java-app-lb
spec:
  type: LoadBalancer
  selector:
    app: my-java-app
  ports:
    - port: 80
      targetPort: 8080
```

#### 4. ExternalName

Маппит Service на внешнее DNS-имя. Не создаёт прокси, а возвращает CNAME-запись. Используется для обращения к внешним сервисам по внутреннему имени.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-db
spec:
  type: ExternalName
  externalName: database.example.com
```

### Сравнение типов:

| Тип | Доступность | Использование |
|---|---|---|
| ClusterIP | Только внутри кластера | Взаимодействие микросервисов |
| NodePort | Извне через IP ноды | Тестирование, dev-окружения |
| LoadBalancer | Извне через балансировщик | Продакшен в облаке |
| ExternalName | DNS-алиас | Доступ к внешним сервисам |

[к оглавлению](#kubernetes)

## Что такое Ingress и Ingress Controller?

### Ingress

**Ingress** — это объект Kubernetes, который управляет внешним HTTP/HTTPS-доступом к сервисам внутри кластера. Ingress позволяет определять правила маршрутизации на основе хоста и пути запроса.

### Зачем нужен Ingress, если есть Service LoadBalancer?

+ **Service LoadBalancer** создаёт отдельный балансировщик для каждого сервиса — это дорого.
+ **Ingress** позволяет использовать один балансировщик для множества сервисов, маршрутизируя трафик по правилам.
+ Ingress поддерживает SSL/TLS-терминацию, перенаправление, URL rewriting.

### Пример Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  tls:
    - hosts:
        - myapp.example.com
      secretName: myapp-tls-secret
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: backend-service
                port:
                  number: 80
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend-service
                port:
                  number: 80
```

### Ingress Controller

Сам объект Ingress — это просто набор правил. Для их реализации необходим **Ingress Controller** — отдельный компонент, который считывает правила Ingress и настраивает реальный прокси-сервер.

Популярные Ingress Controller'ы:
+ **NGINX Ingress Controller** — самый популярный, на базе NGINX.
+ **Traefik** — лёгкий, с автоматическим обнаружением сервисов.
+ **HAProxy Ingress** — на базе HAProxy.
+ **AWS ALB Ingress Controller** — для AWS Application Load Balancer.
+ **Istio Ingress Gateway** — часть service mesh Istio.

> Ingress Controller необходимо устанавливать отдельно — он не входит в стандартную поставку Kubernetes.

[к оглавлению](#kubernetes)

## Что такое Namespace и зачем он нужен?

**Namespace** — это механизм виртуального разделения кластера Kubernetes на логически изолированные пространства.

### Зачем нужны Namespace:

+ **Разделение окружений** — `dev`, `staging`, `production` в одном кластере.
+ **Разделение команд** — каждая команда работает в своём Namespace.
+ **Управление ресурсами** — можно задать квоты (ResourceQuota) на CPU, память, количество объектов для каждого Namespace.
+ **Контроль доступа** — RBAC-политики привязываются к Namespace.
+ **Изоляция имён** — объекты в разных Namespace могут иметь одинаковые имена.

### Стандартные Namespace:

+ **default** — используется по умолчанию, если Namespace не указан.
+ **kube-system** — системные компоненты Kubernetes (DNS, Controller Manager и т.д.).
+ **kube-public** — доступен всем пользователям, обычно используется для общей информации.
+ **kube-node-lease** — для heartbeat'ов нод.

### Основные команды:

```bash
# Создать Namespace
kubectl create namespace dev

# Посмотреть все Namespace
kubectl get namespaces

# Работать в конкретном Namespace
kubectl get pods -n dev
kubectl apply -f deployment.yaml -n dev

# Установить Namespace по умолчанию для текущего контекста
kubectl config set-context --current --namespace=dev
```

### Пример YAML:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
  labels:
    environment: development
```

### Пример ResourceQuota:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    pods: "20"
    requests.cpu: "4"
    requests.memory: "8Gi"
    limits.cpu: "8"
    limits.memory: "16Gi"
```

> **Обращение к сервису в другом Namespace:** `<service-name>.<namespace>.svc.cluster.local`. Например: `postgres-service.database.svc.cluster.local`.

[к оглавлению](#kubernetes)

## Что такое ConfigMap и Secret?

### ConfigMap

**ConfigMap** — это объект Kubernetes для хранения конфигурационных данных в виде пар «ключ-значение». Позволяет отделить конфигурацию от образа контейнера.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APPLICATION_PORT: "8080"
  DATABASE_HOST: "postgres-service"
  DATABASE_PORT: "5432"
  LOG_LEVEL: "INFO"
  application.yml: |
    spring:
      datasource:
        url: jdbc:postgresql://postgres-service:5432/mydb
      jpa:
        hibernate:
          ddl-auto: validate
```

### Secret

**Secret** — аналог ConfigMap, но предназначен для хранения конфиденциальных данных (пароли, токены, ключи). Значения хранятся в формате base64.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DATABASE_USERNAME: cG9zdGdyZXM=        # postgres (base64)
  DATABASE_PASSWORD: c2VjcmV0MTIz        # secret123 (base64)
  JWT_SECRET: bXktand0LXNlY3JldA==       # my-jwt-secret (base64)
```

Создание Secret через kubectl:
```bash
kubectl create secret generic app-secret \
  --from-literal=DATABASE_USERNAME=postgres \
  --from-literal=DATABASE_PASSWORD=secret123
```

### Способы передачи в контейнер:

#### 1. Через переменные окружения:

```yaml
spec:
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DATABASE_HOST
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DATABASE_PASSWORD
      envFrom:
        - configMapRef:
            name: app-config
        - secretRef:
            name: app-secret
```

#### 2. Через монтирование как Volume:

```yaml
spec:
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      volumeMounts:
        - name: config-volume
          mountPath: /app/config
          readOnly: true
  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

> **Важно:** Secret в Kubernetes по умолчанию хранятся только в base64, а не зашифрованы. Для настоящего шифрования нужно включить encryption at rest или использовать внешние решения (HashiCorp Vault, Sealed Secrets).

[к оглавлению](#kubernetes)

## Как передать конфигурацию в Java/Spring Boot приложение через ConfigMap и Secret?

Spring Boot приложения гибко работают с конфигурацией из Kubernetes. Есть несколько подходов:

### 1. Переменные окружения

Spring Boot автоматически подхватывает переменные окружения. Переменная `SPRING_DATASOURCE_URL` маппится на свойство `spring.datasource.url`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: spring-config
data:
  SPRING_DATASOURCE_URL: "jdbc:postgresql://postgres-service:5432/mydb"
  SPRING_JPA_HIBERNATE_DDL_AUTO: "validate"
  SERVER_PORT: "8080"
---
apiVersion: v1
kind: Secret
metadata:
  name: spring-secret
type: Opaque
stringData:
  SPRING_DATASOURCE_USERNAME: "postgres"
  SPRING_DATASOURCE_PASSWORD: "secret123"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-spring-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-spring-app
  template:
    metadata:
      labels:
        app: my-spring-app
    spec:
      containers:
        - name: app
          image: my-registry/my-spring-app:1.0.0
          ports:
            - containerPort: 8080
          envFrom:
            - configMapRef:
                name: spring-config
            - secretRef:
                name: spring-secret
```

### 2. Монтирование application.yml как файл

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: spring-app-config
data:
  application.yml: |
    server:
      port: 8080
      shutdown: graceful
    spring:
      datasource:
        url: jdbc:postgresql://postgres-service:5432/mydb
        hikari:
          maximum-pool-size: 10
      jpa:
        hibernate:
          ddl-auto: validate
    management:
      endpoints:
        web:
          exposure:
            include: health,info,prometheus
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-spring-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-spring-app
  template:
    metadata:
      labels:
        app: my-spring-app
    spec:
      containers:
        - name: app
          image: my-registry/my-spring-app:1.0.0
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: config
              mountPath: /app/config
              readOnly: true
          env:
            - name: SPRING_CONFIG_LOCATION
              value: "file:/app/config/application.yml"
      volumes:
        - name: config
          configMap:
            name: spring-app-config
```

### 3. Spring Cloud Kubernetes

Библиотека **Spring Cloud Kubernetes** позволяет напрямую читать ConfigMap и Secret как PropertySource. При изменении ConfigMap приложение может автоматически перезагрузить конфигурацию без перезапуска Pod'а.

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-kubernetes-client-config</artifactId>
</dependency>
```

```yaml
# bootstrap.yml
spring:
  cloud:
    kubernetes:
      config:
        enabled: true
        name: my-app-config
      secrets:
        enabled: true
        name: my-app-secret
      reload:
        enabled: true
        strategy: refresh
```

[к оглавлению](#kubernetes)

## Какие типы Volumes существуют в Kubernetes?

**Volume** — это механизм хранения данных для Pod'ов. В отличие от файловой системы контейнера, данные в Volume переживают перезапуск контейнера (но не обязательно удаление Pod'а).

### Основные типы:

#### 1. emptyDir

Создаётся при назначении Pod'а на ноду, существует пока Pod запущен. Удаляется при удалении Pod'а. Все контейнеры в Pod'е могут читать и писать в этот том.

**Применение:** обмен данными между контейнерами одного Pod'а (например, основное приложение и sidecar).

```yaml
spec:
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      volumeMounts:
        - name: shared-data
          mountPath: /app/data
    - name: sidecar
      image: busybox
      volumeMounts:
        - name: shared-data
          mountPath: /data
  volumes:
    - name: shared-data
      emptyDir: {}
```

#### 2. hostPath

Монтирует файл или директорию с файловой системы ноды в Pod. Данные привязаны к конкретной ноде.

**Применение:** доступ к системным файлам ноды, логам Docker. Используется редко, так как привязывает Pod к конкретной ноде.

```yaml
volumes:
  - name: host-logs
    hostPath:
      path: /var/log
      type: Directory
```

#### 3. configMap и secret

Монтирование ConfigMap или Secret как файловой системы (описано в предыдущем вопросе).

#### 4. nfs

Монтирование сетевой файловой системы NFS. Данные доступны с любой ноды.

```yaml
volumes:
  - name: nfs-volume
    nfs:
      server: nfs-server.example.com
      path: /exports/data
```

[к оглавлению](#kubernetes)

## Что такое PersistentVolume и PersistentVolumeClaim?

Для хранения данных, которые должны пережить удаление Pod'а, в Kubernetes существует подсистема **Persistent Volumes**.

### PersistentVolume (PV)

**PersistentVolume** — это ресурс хранилища в кластере, предоставленный администратором или динамически через StorageClass. PV существует независимо от Pod'ов.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: postgres-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: /data/postgres
```

### PersistentVolumeClaim (PVC)

**PersistentVolumeClaim** — это запрос на хранилище от пользователя. Pod запрашивает PVC, а Kubernetes связывает его с подходящим PV.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard
```

### Использование в Pod:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:15
          ports:
            - containerPort: 5432
          volumeMounts:
            - name: postgres-storage
              mountPath: /var/lib/postgresql/data
          env:
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: postgres-secret
                  key: password
      volumes:
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: postgres-pvc
```

### Access Modes:

| Режим | Описание |
|---|---|
| **ReadWriteOnce (RWO)** | Чтение и запись одной нодой |
| **ReadOnlyMany (ROX)** | Только чтение множеством нод |
| **ReadWriteMany (RWX)** | Чтение и запись множеством нод |

### Reclaim Policy (политика освобождения):

+ **Retain** — PV сохраняется после удаления PVC (данные остаются, нужна ручная очистка).
+ **Delete** — PV и данные удаляются вместе с PVC.
+ **Recycle** (устарел) — очистка данных и повторное использование.

### StorageClass

**StorageClass** позволяет динамически создавать PV по запросу PVC. Администратору не нужно создавать PV вручную.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp3
  fsType: ext4
reclaimPolicy: Delete
allowVolumeExpansion: true
```

[к оглавлению](#kubernetes)

## Что такое Liveness, Readiness и Startup Probes?

**Probes (пробы)** — это механизм проверки состояния контейнера в Kubernetes. Они определяют, жив ли контейнер, готов ли принимать трафик и успешно ли запустился.

### Liveness Probe

Проверяет, **жив ли контейнер**. Если проверка не проходит — Kubernetes перезапускает контейнер.

**Когда использовать:** для обнаружения deadlock'ов, зависаний приложения, утечек памяти, когда приложение «висит», но процесс работает.

### Readiness Probe

Проверяет, **готов ли контейнер принимать трафик**. Если проверка не проходит — Pod убирается из списка эндпоинтов Service (трафик не поступает), но контейнер **не перезапускается**.

**Когда использовать:** при длительном прогреве приложения (загрузка кэша, подключение к БД), временных перегрузках.

### Startup Probe

Проверяет, **завершился ли запуск контейнера**. Пока Startup Probe не пройдёт — Liveness и Readiness Probes не запускаются.

**Когда использовать:** для приложений с долгим стартом (большие Spring Boot приложения могут стартовать минуту и более).

### Типы проверок:

| Тип | Описание |
|---|---|
| **httpGet** | HTTP GET-запрос к эндпоинту. Успех: код 200-399 |
| **tcpSocket** | Проверка TCP-соединения к порту |
| **exec** | Выполнение команды внутри контейнера. Успех: exit code 0 |
| **grpc** | gRPC Health Check |

### Пример для Spring Boot приложения:

Spring Boot Actuator предоставляет готовые эндпоинты для проб:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-spring-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-spring-app
  template:
    metadata:
      labels:
        app: my-spring-app
    spec:
      containers:
        - name: app
          image: my-registry/my-spring-app:1.0.0
          ports:
            - containerPort: 8080
          startupProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            initialDelaySeconds: 10
            periodSeconds: 5
            failureThreshold: 30       # 10 + 30*5 = до 160 сек на старт
          livenessProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            periodSeconds: 10
            failureThreshold: 3
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 8080
            periodSeconds: 5
            failureThreshold: 3
```

Настройка Spring Boot `application.yml`:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  endpoint:
    health:
      probes:
        enabled: true
      group:
        liveness:
          include: livenessState
        readiness:
          include: readinessState,db
```

### Параметры проб:

| Параметр | Описание | По умолчанию |
|---|---|---|
| **initialDelaySeconds** | Задержка перед первой проверкой | 0 |
| **periodSeconds** | Интервал между проверками | 10 |
| **timeoutSeconds** | Тайм-аут одной проверки | 1 |
| **failureThreshold** | Кол-во неудачных проверок подряд для признания провала | 3 |
| **successThreshold** | Кол-во успешных проверок для признания успеха | 1 |

[к оглавлению](#kubernetes)

## Что такое Requests и Limits для ресурсов?

**Requests** и **Limits** — это механизм управления вычислительными ресурсами (CPU и память), которые контейнер может использовать.

### Requests (запросы)

**Requests** — минимальное количество ресурсов, **гарантированное** контейнеру. Scheduler использует Requests для выбора ноды, на которой хватит ресурсов для Pod'а.

### Limits (лимиты)

**Limits** — максимальное количество ресурсов, которое контейнер может потребить.

+ **CPU Limit** — при превышении контейнер будет **троттлиться** (throttling), но не убит.
+ **Memory Limit** — при превышении контейнер будет **убит** (OOMKilled) и перезапущен.

### Единицы измерения:

| Ресурс | Единицы | Примеры |
|---|---|---|
| **CPU** | Ядра или millicores | `1` = 1 ядро, `500m` = 0.5 ядра, `250m` = 0.25 ядра |
| **Memory** | Байты с суффиксами | `128Mi` = 128 мебибайт, `1Gi` = 1 гибибайт |

### Пример:

```yaml
spec:
  containers:
    - name: app
      image: my-registry/my-java-app:1.0.0
      resources:
        requests:
          memory: "512Mi"
          cpu: "250m"
        limits:
          memory: "1Gi"
          cpu: "1000m"
```

### QoS-классы (Quality of Service):

Kubernetes назначает Pod'у QoS-класс на основе Requests и Limits:

| QoS-класс | Условие | Приоритет при вытеснении |
|---|---|---|
| **Guaranteed** | Requests == Limits для всех контейнеров | Последний (самый защищённый) |
| **Burstable** | Requests < Limits хотя бы у одного контейнера | Средний |
| **BestEffort** | Нет ни Requests, ни Limits | Первый (вытесняется первым) |

### Рекомендации для Java-приложений:

+ Всегда устанавливайте и Requests, и Limits для памяти.
+ Memory Limit должен учитывать: heap (`-Xmx`) + metaspace + thread stacks + native memory + буфер (~20-30%).
+ Пример: если `-Xmx512m`, то Memory Limit рекомендуется ставить `768Mi`–`1Gi`.
+ Используйте JVM-флаги для корректной работы в контейнере:

```yaml
env:
  - name: JAVA_OPTS
    value: >-
      -XX:MaxRAMPercentage=75.0
      -XX:+UseContainerSupport
```

Флаг `-XX:+UseContainerSupport` (включён по умолчанию с JDK 10+) позволяет JVM корректно определять память и CPU, доступные контейнеру, а не всей ноде.

[к оглавлению](#kubernetes)

## Что такое Horizontal Pod Autoscaler (HPA)?

**HPA (Horizontal Pod Autoscaler)** — это контроллер, который автоматически масштабирует количество Pod'ов в Deployment (или ReplicaSet) в зависимости от наблюдаемых метрик.

### Принцип работы:

1. HPA периодически (по умолчанию каждые 15 секунд) проверяет текущие метрики.
2. Сравнивает текущее значение с целевым.
3. Вычисляет необходимое количество реплик по формуле:
   `desiredReplicas = ceil(currentReplicas * (currentMetricValue / desiredMetricValue))`
4. Масштабирует Deployment.

### Типы метрик:

+ **Resource metrics** — использование CPU, памяти (через Metrics Server).
+ **Custom metrics** — пользовательские метрики из Prometheus (количество запросов в секунду, размер очереди и т.д.).
+ **External metrics** — метрики из внешних систем (длина очереди в RabbitMQ/Kafka и т.д.).

### Пример HPA:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-java-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-java-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
        - type: Percent
          value: 10
          periodSeconds: 60
    scaleUp:
      stabilizationWindowSeconds: 60
      policies:
        - type: Percent
          value: 50
          periodSeconds: 60
```

### Команды для работы с HPA:

```bash
# Создать HPA через kubectl (простой вариант)
kubectl autoscale deployment my-java-app --cpu-percent=70 --min=2 --max=10

# Посмотреть состояние HPA
kubectl get hpa

# Подробная информация
kubectl describe hpa my-java-app-hpa
```

### Важные моменты:

+ Для работы HPA необходим **Metrics Server** в кластере.
+ Для Pod'ов обязательно должны быть заданы **Requests** по CPU/памяти — иначе HPA не сможет рассчитать процент утилизации.
+ **Stabilization window** предотвращает слишком частое масштабирование (flapping).
+ HPA масштабирует горизонтально (количество Pod'ов). Для вертикального масштабирования (ресурсы Pod'а) существует **VPA (Vertical Pod Autoscaler)**.

[к оглавлению](#kubernetes)

## Какие основные команды kubectl нужно знать?

**kubectl** — это CLI-инструмент для управления кластером Kubernetes. Все команды работают с API Server.

### Получение информации:

```bash
# Список Pod'ов (в текущем namespace)
kubectl get pods

# Список Pod'ов во всех namespace
kubectl get pods --all-namespaces
kubectl get pods -A

# Список Pod'ов с подробностями (IP, нода)
kubectl get pods -o wide

# Список всех ресурсов
kubectl get all

# Список по типу
kubectl get deployments
kubectl get services
kubectl get configmaps
kubectl get secrets
kubectl get ingress
kubectl get nodes
kubectl get namespaces
kubectl get pv
kubectl get pvc

# Вывод в YAML или JSON формате
kubectl get deployment my-app -o yaml
kubectl get pod my-pod -o json
```

### Подробная информация:

```bash
# Детальное описание ресурса (включая Events!)
kubectl describe pod my-pod
kubectl describe deployment my-app
kubectl describe node worker-1
kubectl describe service my-service
```

> `kubectl describe` — одна из самых полезных команд для диагностики. Секция **Events** в выводе показывает, почему Pod не запускается.

### Логи:

```bash
# Логи Pod'а
kubectl logs my-pod

# Логи конкретного контейнера в Pod'е
kubectl logs my-pod -c app

# Стримить логи (аналог tail -f)
kubectl logs -f my-pod

# Последние 100 строк
kubectl logs --tail=100 my-pod

# Логи за последний час
kubectl logs --since=1h my-pod

# Логи предыдущего (упавшего) контейнера
kubectl logs my-pod --previous
```

### Выполнение команд внутри контейнера:

```bash
# Выполнить команду
kubectl exec my-pod -- ls /app

# Открыть интерактивный терминал
kubectl exec -it my-pod -- /bin/sh
kubectl exec -it my-pod -- /bin/bash

# Для Pod'а с несколькими контейнерами
kubectl exec -it my-pod -c app -- /bin/bash
```

### Создание и удаление ресурсов:

```bash
# Применить манифест (создать или обновить)
kubectl apply -f deployment.yaml

# Применить все манифесты из директории
kubectl apply -f ./k8s/

# Удалить ресурс
kubectl delete pod my-pod
kubectl delete deployment my-app
kubectl delete -f deployment.yaml

# Создать ресурс из манифеста (только создание, без обновления)
kubectl create -f deployment.yaml
```

### Масштабирование:

```bash
# Изменить количество реплик
kubectl scale deployment my-app --replicas=5
```

### Переадресация портов (для отладки):

```bash
# Пробросить порт Pod'а на локальную машину
kubectl port-forward pod/my-pod 8080:8080

# Пробросить порт Service
kubectl port-forward service/my-service 8080:80
```

### Полезные возможности:

```bash
# Посмотреть потребление ресурсов
kubectl top pods
kubectl top nodes

# Проверить манифест без применения (dry run)
kubectl apply -f deployment.yaml --dry-run=client

# Посмотреть отличия до применения
kubectl diff -f deployment.yaml
```

[к оглавлению](#kubernetes)

## Как выглядит YAML-манифест для деплоя Java/Spring Boot приложения?

Ниже приведён полный набор манифестов для развёртывания типичного Spring Boot приложения с базой данных PostgreSQL.

### Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-spring-app
  namespace: production
  labels:
    app: my-spring-app
    version: "1.0.0"
spec:
  replicas: 3
  revisionHistoryLimit: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-spring-app
  template:
    metadata:
      labels:
        app: my-spring-app
        version: "1.0.0"
    spec:
      terminationGracePeriodSeconds: 60
      containers:
        - name: app
          image: my-registry/my-spring-app:1.0.0
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
          env:
            - name: JAVA_OPTS
              value: >-
                -XX:MaxRAMPercentage=75.0
                -XX:+UseContainerSupport
                -Djava.security.egd=file:/dev/./urandom
            - name: SPRING_PROFILES_ACTIVE
              value: "kubernetes,production"
          envFrom:
            - configMapRef:
                name: spring-config
            - secretRef:
                name: spring-secret
          resources:
            requests:
              memory: "512Mi"
              cpu: "250m"
            limits:
              memory: "1Gi"
              cpu: "1000m"
          startupProbe:
            httpGet:
              path: /actuator/health/liveness
              port: http
            initialDelaySeconds: 15
            periodSeconds: 5
            failureThreshold: 30
          livenessProbe:
            httpGet:
              path: /actuator/health/liveness
              port: http
            periodSeconds: 10
            failureThreshold: 3
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: http
            periodSeconds: 5
            failureThreshold: 3
      imagePullSecrets:
        - name: registry-credentials
```

### Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-spring-app-service
  namespace: production
  labels:
    app: my-spring-app
spec:
  type: ClusterIP
  selector:
    app: my-spring-app
  ports:
    - name: http
      port: 80
      targetPort: http
      protocol: TCP
```

### Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-spring-app-ingress
  namespace: production
  annotations:
    nginx.ingress.kubernetes.io/proxy-body-size: "10m"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "60"
spec:
  ingressClassName: nginx
  tls:
    - hosts:
        - api.example.com
      secretName: api-tls-secret
  rules:
    - host: api.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-spring-app-service
                port:
                  number: 80
```

### ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: spring-config
  namespace: production
data:
  SPRING_DATASOURCE_URL: "jdbc:postgresql://postgres-service:5432/mydb"
  SPRING_JPA_HIBERNATE_DDL_AUTO: "validate"
  SPRING_JPA_SHOW_SQL: "false"
  SERVER_SHUTDOWN: "graceful"
  SPRING_LIFECYCLE_TIMEOUT_PER_SHUTDOWN_PHASE: "30s"
```

### Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: spring-secret
  namespace: production
type: Opaque
stringData:
  SPRING_DATASOURCE_USERNAME: "app_user"
  SPRING_DATASOURCE_PASSWORD: "secure-password-here"
```

### HPA:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-spring-app-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-spring-app
  minReplicas: 3
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

> Все эти манифесты можно поместить в отдельные файлы в директории `k8s/` и применить одной командой: `kubectl apply -f ./k8s/ -n production`.

[к оглавлению](#kubernetes)

## Что такое Helm?

**Helm** — это пакетный менеджер для Kubernetes, аналог apt/yum для Linux или Maven для Java. Helm позволяет описывать, устанавливать и обновлять приложения в Kubernetes как единый пакет.

### Основные понятия:

+ **Chart** — пакет Helm. Набор файлов, описывающих связанные ресурсы Kubernetes (Deployment, Service, Ingress и т.д.) в виде шаблонов.
+ **Values** — файл с параметрами (`values.yaml`), которые подставляются в шаблоны Chart'а. Позволяет конфигурировать одно и то же приложение для разных окружений.
+ **Release** — конкретная установка Chart'а в кластер с определёнными values. Один Chart можно установить несколько раз с разными параметрами.
+ **Repository** — хранилище Chart'ов (аналог Maven Repository).

### Структура Chart'а:

```
my-spring-app/
  Chart.yaml          # Метаданные Chart'а (имя, версия, описание)
  values.yaml         # Значения по умолчанию
  templates/          # Шаблоны Kubernetes-ресурсов
    deployment.yaml
    service.yaml
    ingress.yaml
    configmap.yaml
    hpa.yaml
    _helpers.tpl      # Вспомогательные шаблонные функции
  charts/             # Зависимости (sub-charts)
```

### Пример templates/deployment.yaml:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "my-app.fullname" . }}
  labels:
    {{- include "my-app.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      {{- include "my-app.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      labels:
        {{- include "my-app.selectorLabels" . | nindent 8 }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: {{ .Values.service.targetPort }}
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

### Пример values.yaml:

```yaml
replicaCount: 3

image:
  repository: my-registry/my-spring-app
  tag: "1.0.0"
  pullPolicy: Always

service:
  type: ClusterIP
  port: 80
  targetPort: 8080

ingress:
  enabled: true
  host: api.example.com

resources:
  requests:
    memory: "512Mi"
    cpu: "250m"
  limits:
    memory: "1Gi"
    cpu: "1000m"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilization: 70
```

### Основные команды Helm:

```bash
# Добавить репозиторий
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# Установить Chart
helm install my-release my-spring-app/ -f values-prod.yaml -n production

# Обновить Release
helm upgrade my-release my-spring-app/ -f values-prod.yaml -n production

# Установить или обновить
helm upgrade --install my-release my-spring-app/ -f values-prod.yaml

# Откатить к предыдущей версии
helm rollback my-release 1

# Список установленных Release
helm list -n production

# Удалить Release
helm uninstall my-release -n production

# Посмотреть сгенерированные манифесты без установки
helm template my-release my-spring-app/ -f values-prod.yaml

# Установить популярный Chart (например, PostgreSQL)
helm install my-postgres bitnami/postgresql -f postgres-values.yaml
```

### Зачем Helm Java-разработчику?

+ Один Chart для всех окружений (dev, staging, prod) — разные `values.yaml`.
+ Управление зависимостями (PostgreSQL, Redis, Kafka — готовые Chart'ы из публичных репозиториев).
+ Версионирование и откат деплоев.
+ Стандартизация инфраструктуры в команде.

[к оглавлению](#kubernetes)

## Как задеплоить Java-приложение в Kubernetes?

Процесс деплоя Java-приложения в Kubernetes состоит из нескольких этапов:

### 1. Создание Docker-образа

```dockerfile
# Dockerfile (multi-stage build)
FROM eclipse-temurin:17-jdk-alpine AS builder
WORKDIR /app
COPY . .
RUN ./mvnw clean package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --from=builder /app/target/*.jar app.jar
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Сборка и публикация:
```bash
# Сборка образа
docker build -t my-registry/my-spring-app:1.0.0 .

# Публикация в Container Registry
docker push my-registry/my-spring-app:1.0.0
```

### 2. Создание Kubernetes-манифестов

Создать файлы Deployment, Service, ConfigMap, Secret, Ingress (примеры приведены в предыдущих вопросах).

### 3. Применение манифестов

```bash
# Создать namespace
kubectl create namespace production

# Создать секрет для доступа к registry (если приватный)
kubectl create secret docker-registry registry-credentials \
  --docker-server=my-registry \
  --docker-username=user \
  --docker-password=password \
  -n production

# Применить все манифесты
kubectl apply -f k8s/ -n production

# Проверить статус
kubectl get pods -n production
kubectl get services -n production
kubectl describe deployment my-spring-app -n production
```

### 4. Типичный CI/CD Pipeline (GitLab CI / GitHub Actions)

```yaml
# Пример GitHub Actions workflow
name: Deploy
on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn clean package -DskipTests

      - name: Build Docker image
        run: docker build -t my-registry/my-app:${{ github.sha }} .

      - name: Push Docker image
        run: docker push my-registry/my-app:${{ github.sha }}

      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/my-spring-app \
            app=my-registry/my-app:${{ github.sha }} \
            -n production
```

### Чек-лист перед деплоем:

+ Docker-образ оптимизирован (multi-stage build, JRE вместо JDK, non-root user).
+ Настроены Liveness, Readiness и Startup Probes.
+ Указаны Requests и Limits по CPU и памяти.
+ JVM настроена для работы в контейнере (`-XX:MaxRAMPercentage`, `-XX:+UseContainerSupport`).
+ Настроен graceful shutdown (`server.shutdown=graceful`).
+ Конфигурация вынесена в ConfigMap/Secret.
+ Логи пишутся в stdout/stderr (не в файл).
+ Приложение stateless (состояние хранится во внешних хранилищах).

[к оглавлению](#kubernetes)

## Как организовать мониторинг и логирование в Kubernetes?

### Мониторинг

#### Prometheus + Grafana (стандарт de facto)

**Prometheus** — система мониторинга и алертинга. Собирает метрики с приложений и компонентов кластера по pull-модели (периодически опрашивает HTTP-эндпоинты `/metrics`).

**Grafana** — платформа визуализации. Строит дашборды и графики на основе данных из Prometheus.

Для Spring Boot приложений:

1. Подключить зависимости:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

2. Настроить `application.yml`:
```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  metrics:
    export:
      prometheus:
        enabled: true
    tags:
      application: my-spring-app
```

3. Добавить аннотации для автоматического обнаружения Prometheus:
```yaml
# В Deployment -> template -> metadata
metadata:
  labels:
    app: my-spring-app
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "8080"
    prometheus.io/path: "/actuator/prometheus"
```

#### Какие метрики доступны:

+ **JVM-метрики** — heap memory, GC, threads, class loading.
+ **HTTP-метрики** — количество запросов, время ответа, коды ошибок.
+ **Connection pool** — активные соединения HikariCP, ожидающие запросы.
+ **Кастомные метрики** — бизнес-метрики через Micrometer API.

### Логирование

#### ELK Stack (Elasticsearch + Logstash + Kibana)

Стандартный подход к централизованному логированию:

+ **Elasticsearch** — хранение и поиск логов.
+ **Logstash** (или **Fluentd/Fluent Bit**) — сбор, парсинг и отправка логов.
+ **Kibana** — визуализация и поиск по логам.

#### Как это работает в Kubernetes:

1. Приложения пишут логи в **stdout/stderr** (стандартная практика для контейнеров).
2. Container Runtime сохраняет логи на ноде.
3. **DaemonSet** с Fluent Bit / Fluentd на каждой ноде собирает логи и отправляет в Elasticsearch.
4. Разработчики ищут и анализируют логи через Kibana.

#### Рекомендации для Java-приложений:

+ Писать логи в **stdout/stderr**, а не в файлы.
+ Использовать **JSON-формат** для логов — это упрощает парсинг:

```yaml
# logback-spring.xml
<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
</appender>
```

+ Добавлять в логи **идентификаторы**: traceId, spanId, podName, namespace.
+ Настроить уровни логирования через ConfigMap, чтобы менять их без пересборки.

### Встроенные средства kubectl для логов:

```bash
# Логи Pod'а
kubectl logs my-pod -n production

# Стримить логи нескольких Pod'ов (с помощью label selector)
kubectl logs -l app=my-spring-app -f -n production

# Использовать stern (удобная утилита для мультиподовых логов)
stern my-spring-app -n production
```

### Общая архитектура мониторинга:

```
Spring Boot App ──metrics──> Prometheus ──> Grafana (дашборды)
      │                          │
      │                    Alertmanager (оповещения)
      │
      └──logs (stdout)──> Fluent Bit ──> Elasticsearch ──> Kibana
```

[к оглавлению](#kubernetes)

## Какие особенности запуска JVM-приложений в Kubernetes нужно учитывать?

Запуск Java-приложений в Kubernetes имеет ряд нюансов, связанных с особенностями JVM и контейнерной среды.

### 1. Настройка памяти JVM

JVM должна корректно определять доступную память контейнера, а не всей ноды.

```yaml
env:
  - name: JAVA_OPTS
    value: >-
      -XX:+UseContainerSupport
      -XX:MaxRAMPercentage=75.0
      -XX:InitialRAMPercentage=50.0
```

+ **`-XX:+UseContainerSupport`** — включён по умолчанию с JDK 10+. JVM видит лимиты контейнера, а не ресурсы ноды.
+ **`-XX:MaxRAMPercentage=75.0`** — heap займёт не более 75% доступной контейнеру памяти. Остальные 25% — для metaspace, thread stacks, native memory, off-heap буферов.
+ Не используйте фиксированные `-Xmx`/`-Xms`, если хотите гибкости при изменении Limits.

### 2. Определение CPU

Kubernetes ограничивает CPU через CFS-квоты Linux. JVM с `UseContainerSupport` определяет количество «процессоров» на основе CPU Limits.

Это влияет на:
+ Количество потоков в пулах (ForkJoinPool, GC threads).
+ Параллелизм по умолчанию.

Если CPU Limit = `500m` (0.5 ядра), JVM увидит 1 процессор. При `2000m` — 2 процессора.

### 3. Graceful Shutdown

Spring Boot должен корректно завершать работу при удалении Pod'а:

```yaml
# application.yml
server:
  shutdown: graceful
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
```

В Deployment:
```yaml
spec:
  terminationGracePeriodSeconds: 60
```

Значение `terminationGracePeriodSeconds` должно быть **больше**, чем `timeout-per-shutdown-phase`, чтобы Spring Boot успел завершить текущие запросы до принудительного убийства процесса.

### 4. Долгий старт

Spring Boot приложения могут стартовать долго (30-120 секунд), особенно с большим количеством бинов, миграциями БД. Используйте **Startup Probe**, чтобы Liveness Probe не убила контейнер во время запуска.

### 5. Логирование

+ Пишите логи в **stdout/stderr** — Kubernetes автоматически их подхватывает.
+ Не пишите в файлы внутри контейнера — это затрудняет сбор логов и расходует дисковое пространство.
+ Используйте JSON-формат для структурированного логирования.

### 6. Оптимизация Docker-образа

```dockerfile
# Плохо: используем JDK, root-пользователь, толстый образ
FROM openjdk:17
COPY target/app.jar /app.jar
CMD ["java", "-jar", "/app.jar"]

# Хорошо: multi-stage, JRE, non-root, alpine
FROM eclipse-temurin:17-jdk-alpine AS builder
WORKDIR /app
COPY . .
RUN ./mvnw clean package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
RUN addgroup -S spring && adduser -S spring -G spring
COPY --from=builder /app/target/*.jar app.jar
USER spring
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 7. Stateless-архитектура

Приложение в Kubernetes должно быть **stateless**:
+ Сессии — во внешнем хранилище (Redis, DB).
+ Файлы — в S3 / MinIO / PersistentVolume.
+ Кэш — Redis / Hazelcast.
+ Конфигурация — ConfigMap / Secret.

Это позволяет свободно масштабировать и перемещать Pod'ы между нодами.

### 8. DNS и Service Discovery

В Spring Boot для обращения к другим сервисам используйте DNS-имена Service'ов Kubernetes:

```yaml
# application.yml
app:
  user-service-url: http://user-service.production.svc.cluster.local:80
  # Или короткая форма (в пределах одного namespace):
  order-service-url: http://order-service:80
```

[к оглавлению](#kubernetes)
