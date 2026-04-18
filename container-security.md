[Вопросы для собеседования](README.md)

# Безопасность контейнеров
+ [Какие основные угрозы безопасности контейнеров существуют?](#Какие-основные-угрозы-безопасности-контейнеров-существуют)
+ [Что такое принцип наименьших привилегий и как он применяется в контейнерах?](#Что-такое-принцип-наименьших-привилегий-и-как-он-применяется-в-контейнерах)
+ [Как запустить контейнер не от пользователя root?](#Как-запустить-контейнер-не-от-пользователя-root)
+ [Зачем использовать минимальные базовые образы и какие варианты существуют?](#Зачем-использовать-минимальные-базовые-образы-и-какие-варианты-существуют)
+ [Как сканировать Docker-образы на уязвимости?](#Как-сканировать-Docker-образы-на-уязвимости)
+ [Что такое подпись и верификация образов?](#Что-такое-подпись-и-верификация-образов)
+ [Как правильно управлять секретами в контейнерах?](#Как-правильно-управлять-секретами-в-контейнерах)
+ [Как использовать multistage builds для повышения безопасности?](#Как-использовать-multistage-builds-для-повышения-безопасности)
+ [Зачем и как использовать read-only файловую систему в контейнерах?](#Зачем-и-как-использовать-read-only-файловую-систему-в-контейнерах)
+ [Что такое Network Policies в Kubernetes и как они защищают приложение?](#Что-такое-Network-Policies-в-Kubernetes-и-как-они-защищают-приложение)
+ [Что такое Pod Security Standards и Pod Security Admission?](#Что-такое-Pod-Security-Standards-и-Pod-Security-Admission)
+ [Что такое Security Context в Kubernetes?](#Что-такое-Security-Context-в-Kubernetes)
+ [Как работает RBAC в Kubernetes?](#Как-работает-RBAC-в-Kubernetes)
+ [Как ограничение ресурсов защищает от DoS-атак?](#Как-ограничение-ресурсов-защищает-от-DoS-атак)
+ [Что такое Supply Chain Security в контексте контейнеров?](#Что-такое-Supply-Chain-Security-в-контексте-контейнеров)
+ [Что такое Docker Bench for Security?](#Что-такое-Docker-Bench-for-Security)
+ [Как защитить Docker daemon?](#Как-защитить-Docker-daemon)
+ [Как работает изоляция контейнеров на уровне ядра Linux?](#Как-работает-изоляция-контейнеров-на-уровне-ядра-Linux)
+ [Что такое Runtime Security и как работает Falco?](#Что-такое-Runtime-Security-и-как-работает-Falco)
+ [Как организовать процесс обновления и патчинга базовых образов?](#Как-организовать-процесс-обновления-и-патчинга-базовых-образов)
+ [Как встроить проверки безопасности в CI/CD пайплайн?](#Как-встроить-проверки-безопасности-в-CICD-пайплайн)
+ [Что такое OWASP Docker Top 10?](#Что-такое-OWASP-Docker-Top-10)
+ [Как обеспечить безопасность Java-приложения в контейнере в банковской среде?](#Как-обеспечить-безопасность-Java-приложения-в-контейнере-в-банковской-среде)

## Какие основные угрозы безопасности контейнеров существуют?

Безопасность контейнеров — критически важная тема, особенно в банковской сфере, где обрабатываются персональные и финансовые данные. Основные угрозы можно разделить на несколько категорий:

**1. Угрозы на уровне образов:**
- **Уязвимости в базовых образах** — устаревшие пакеты и библиотеки с известными CVE (Common Vulnerabilities and Exposures).
- **Вредоносные образы** — использование непроверенных образов из публичных реестров (Docker Hub).
- **Утечка секретов** — пароли, ключи API, сертификаты, случайно включённые в слои образа.
- **Избыточные компоненты** — лишние утилиты и инструменты, увеличивающие поверхность атаки.

**2. Угрозы на уровне среды выполнения (runtime):**
- **Побег из контейнера (container escape)** — эксплуатация уязвимостей ядра Linux для получения доступа к хост-системе.
- **Запуск от root** — контейнер с правами root может получить доступ к ресурсам хоста.
- **Привилегированный режим** — флаг `--privileged` даёт контейнеру полный доступ к устройствам хоста.
- **Незащищённый Docker socket** — доступ к `/var/run/docker.sock` позволяет управлять всеми контейнерами.

**3. Угрозы на уровне оркестрации (Kubernetes):**
- **Неправильная настройка RBAC** — избыточные права для сервисных аккаунтов.
- **Отсутствие сетевых политик** — любой под может обращаться к любому другому поду.
- **Незащищённый API-сервер** — открытый доступ к Kubernetes API без аутентификации.
- **Компрометация etcd** — хранилище всех данных кластера, включая секреты.

**4. Угрозы на уровне цепочки поставок (supply chain):**
- **Подмена зависимостей** (dependency confusion) — вредоносные пакеты с именами, совпадающими с внутренними.
- **Компрометация CI/CD** — внедрение вредоносного кода через пайплайн.
- **Отсутствие проверки целостности** — использование образов без верификации подписи.

**5. Угрозы на уровне сети:**
- **Man-in-the-middle** — перехват трафика между контейнерами.
- **Незашифрованный трафик** — передача данных без TLS внутри кластера.
- **Эксфильтрация данных** — несанкционированная отправка данных наружу.

Для банковских систем особенно критичны: утечка персональных данных (ФЗ-152), утечка платёжных данных (PCI DSS), несанкционированный доступ к финансовым транзакциям.

[к оглавлению](#Безопасность-контейнеров)

## Что такое принцип наименьших привилегий и как он применяется в контейнерах?

**Принцип наименьших привилегий (Principle of Least Privilege, PoLP)** — фундаментальный принцип безопасности, согласно которому каждый субъект (пользователь, процесс, контейнер) должен иметь только те права и ресурсы, которые минимально необходимы для выполнения его функций.

**Применение в Docker:**

1. **Запуск процессов не от root** — указание непривилегированного пользователя через `USER` в Dockerfile.
2. **Удаление лишних capabilities** — Linux capabilities позволяют дробить права root на отдельные привилегии:

```dockerfile
# В Dockerfile
FROM eclipse-temurin:21-jre-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

```bash
# Запуск с удалением всех capabilities и добавлением только нужных
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE my-java-app
```

3. **Запрет привилегированного режима:**
```bash
# Никогда не использовать в production:
docker run --privileged my-app  # ОПАСНО!

# Правильно:
docker run --security-opt=no-new-privileges my-app
```

4. **Read-only файловая система:**
```bash
docker run --read-only --tmpfs /tmp my-java-app
```

**Применение в Kubernetes:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-java-app
spec:
  containers:
  - name: app
    image: my-registry/java-app:1.0.0
    securityContext:
      runAsNonRoot: true
      runAsUser: 1000
      runAsGroup: 1000
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop:
          - ALL
      seccompProfile:
        type: RuntimeDefault
    volumeMounts:
    - name: tmp
      mountPath: /tmp
  volumes:
  - name: tmp
    emptyDir:
      sizeLimit: 100Mi
  automountServiceAccountToken: false  # отключить монтирование токена SA
```

Ключевой параметр `allowPrivilegeEscalation: false` предотвращает получение дополнительных привилегий дочерними процессами (блокирует `setuid`-бинарники). Параметр `automountServiceAccountToken: false` отключает автоматическое монтирование токена сервисного аккаунта, если поду не нужен доступ к Kubernetes API.

**Почему это важно для банков:** Регуляторные требования (PCI DSS Requirement 7) прямо требуют ограничения доступа к данным по принципу need-to-know. Нарушение этого принципа может привести к штрафам и отзыву лицензии.

[к оглавлению](#Безопасность-контейнеров)

## Как запустить контейнер не от пользователя root?

По умолчанию процесс внутри контейнера запускается от имени root (UID 0). Это опасно, так как при побеге из контейнера атакующий получает root-права на хосте. Существует несколько уровней защиты.

**1. Инструкция USER в Dockerfile:**

```dockerfile
FROM eclipse-temurin:21-jre-alpine

# Создаём группу и пользователя
RUN addgroup -S javaapp && adduser -S javaapp -G javaapp

# Создаём директории и назначаем владельца
RUN mkdir -p /app/logs /app/config && \
    chown -R javaapp:javaapp /app

WORKDIR /app

# Копируем артефакт
COPY --chown=javaapp:javaapp target/banking-service.jar /app/app.jar

# Переключаемся на непривилегированного пользователя
USER javaapp

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

**2. Директива runAsNonRoot в Kubernetes:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: banking-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: banking-service
  template:
    metadata:
      labels:
        app: banking-service
    spec:
      securityContext:
        runAsNonRoot: true          # Запрет запуска от root на уровне пода
        fsGroup: 1000               # Группа для файловой системы томов
      containers:
      - name: app
        image: registry.bank.local/banking-service:1.0.0
        securityContext:
          runAsUser: 1000
          runAsGroup: 1000
          allowPrivilegeEscalation: false
```

Если в образе `USER` указан root, а в K8s выставлен `runAsNonRoot: true`, Kubernetes **откажется запустить** под и выдаст ошибку `CreateContainerConfigError`.

**3. User namespace remapping в Docker daemon:**

```json
// /etc/docker/daemon.json
{
  "userns-remap": "default"
}
```

Это сопоставляет UID 0 внутри контейнера с непривилегированным UID на хосте (например, 100000). Даже если процесс внутри контейнера работает как root, на хосте он будет обычным пользователем.

**4. Типичные проблемы при запуске не от root:**

| Проблема | Решение |
|----------|---------|
| Нет прав на запись в `/app/logs` | `RUN mkdir /app/logs && chown appuser:appgroup /app/logs` |
| Порт ниже 1024 (например, 80) | Использовать порт 8080+ или добавить `NET_BIND_SERVICE` capability |
| Java не может создать временные файлы | Примонтировать `tmpfs` или `emptyDir` в `/tmp` |
| Не удаётся подключиться к БД через unix-socket | Настроить правильные права на сокет или использовать TCP |

**Проверка текущего пользователя в работающем контейнере:**

```bash
docker exec my-container whoami
docker exec my-container id
# uid=1000(javaapp) gid=1000(javaapp) groups=1000(javaapp)
```

[к оглавлению](#Безопасность-контейнеров)

## Зачем использовать минимальные базовые образы и какие варианты существуют?

Чем меньше компонентов в образе, тем меньше **поверхность атаки** (attack surface). Каждая дополнительная утилита — потенциальная уязвимость.

**Сравнение базовых образов для Java:**

| Образ | Размер | Shell | Package Manager | Уязвимости* |
|-------|--------|-------|-----------------|-------------|
| `eclipse-temurin:21-jdk` | ~470 MB | Да | apt | Много |
| `eclipse-temurin:21-jre` | ~270 MB | Да | apt | Средне |
| `eclipse-temurin:21-jre-alpine` | ~90 MB | Да (ash) | apk | Мало |
| `gcr.io/distroless/java21-debian12` | ~230 MB | Нет | Нет | Минимум |

*\* — примерная оценка количества потенциальных уязвимостей*

**1. Alpine-based образы:**

```dockerfile
FROM eclipse-temurin:21-jre-alpine

RUN addgroup -S app && adduser -S app -G app
WORKDIR /app
COPY --chown=app:app target/service.jar /app/app.jar
USER app

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

Преимущества: маленький размер, musl libc, минимум пакетов. Недостатки: возможна несовместимость с некоторыми нативными библиотеками (glibc vs musl).

**2. Google Distroless:**

```dockerfile
# Этап сборки
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /build
COPY . .
RUN ./mvnw clean package -DskipTests

# Финальный образ
FROM gcr.io/distroless/java21-debian12

COPY --from=builder /build/target/service.jar /app/app.jar
WORKDIR /app

# Distroless уже работает от nonroot (UID 65532)
USER nonroot:nonroot

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

Distroless-образы не содержат shell, пакетного менеджера и прочих утилит. Это значит, что атакующий, даже проникнув в контейнер, **не сможет** выполнить `sh`, `bash`, `curl`, `wget` и другие команды. Для отладки можно использовать debug-вариант `gcr.io/distroless/java21-debian12:debug`, содержащий busybox shell.

**3. Slim-варианты:**

```dockerfile
FROM eclipse-temurin:21-jre-noble  # Ubuntu-based, полный
FROM eclipse-temurin:21-jre-alpine # Alpine-based, минимальный
```

**4. Собственный минимальный образ с jlink (Java 9+):**

```dockerfile
FROM eclipse-temurin:21-jdk-alpine AS builder

WORKDIR /build
COPY . .
RUN ./mvnw clean package -DskipTests

# Создаём кастомный JRE только с нужными модулями
RUN jlink \
    --add-modules java.base,java.sql,java.naming,java.management,java.logging \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=zip-9 \
    --output /custom-jre

FROM alpine:3.20
COPY --from=builder /custom-jre /opt/java
COPY --from=builder /build/target/service.jar /app/app.jar

RUN addgroup -S app && adduser -S app -G app
USER app

ENV JAVA_HOME=/opt/java
ENV PATH="${JAVA_HOME}/bin:${PATH}"

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

Результат: образ ~60 MB вместо 470 MB, содержит только нужные Java-модули.

**Рекомендации для банковских проектов:**
- Production: `distroless` или `alpine` с jlink.
- Staging/QA: `alpine`-варианты (есть shell для отладки).
- Никогда не использовать `latest` тег — всегда фиксировать версию с SHA256: `eclipse-temurin:21-jre-alpine@sha256:abc123...`.

[к оглавлению](#Безопасность-контейнеров)

## Как сканировать Docker-образы на уязвимости?

Сканирование образов — обязательный этап в безопасном конвейере доставки. Сканеры анализируют пакеты ОС и зависимости приложения, сверяя их с базами данных уязвимостей (NVD, GitHub Advisory, Alpine SecDB и др.).

**1. Trivy (Aqua Security) — один из самых популярных open-source сканеров:**

```bash
# Установка
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh

# Сканирование образа
trivy image my-registry/banking-service:1.0.0

# Сканирование только на CRITICAL и HIGH уязвимости
trivy image --severity CRITICAL,HIGH my-registry/banking-service:1.0.0

# Сканирование с учётом fix-only (только те, для которых есть исправление)
trivy image --ignore-unfixed my-registry/banking-service:1.0.0

# Сканирование файловой системы проекта (включая Maven/Gradle зависимости)
trivy fs --scanners vuln,secret,misconfig .

# Вывод в формате JSON для интеграции с CI/CD
trivy image --format json --output report.json my-registry/banking-service:1.0.0

# Завершить с ошибкой при наличии CRITICAL уязвимостей
trivy image --exit-code 1 --severity CRITICAL my-registry/banking-service:1.0.0
```

Пример вывода:

```
banking-service:1.0.0 (alpine 3.20)
====================================
Total: 3 (CRITICAL: 1, HIGH: 2)

┌───────────────┬──────────────────┬──────────┬─────────┬───────────────┐
│    Library    │  Vulnerability   │ Severity │ Version │ Fixed Version │
├───────────────┼──────────────────┼──────────┼─────────┼───────────────┤
│ libssl3       │ CVE-2024-XXXXX   │ CRITICAL │ 3.1.4   │ 3.1.5         │
│ spring-web    │ CVE-2024-YYYYY   │ HIGH     │ 6.1.2   │ 6.1.5         │
│ jackson-core  │ CVE-2024-ZZZZZ   │ HIGH     │ 2.16.0  │ 2.16.1        │
└───────────────┴──────────────────┴──────────┴─────────┴───────────────┘
```

**2. Snyk — коммерческий сканер с бесплатным tier-ом:**

```bash
# Сканирование Docker-образа
snyk container test my-registry/banking-service:1.0.0

# Мониторинг (постоянное отслеживание новых уязвимостей)
snyk container monitor my-registry/banking-service:1.0.0

# Сканирование зависимостей проекта
snyk test --all-projects
```

**3. Clair (Quay) — серверный сканер для интеграции с реестрами:**

Clair работает как сервис, анализирующий образы при их push в реестр. Используется в Harbor, Quay и других enterprise-реестрах.

**4. Grype (Anchore) — быстрый CLI-сканер:**

```bash
grype my-registry/banking-service:1.0.0
grype sbom:./sbom.spdx.json  # сканирование на основе SBOM
```

**Интеграция в Jenkinsfile:**

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
                sh 'docker build -t banking-service:${BUILD_NUMBER} .'
            }
        }
        stage('Security Scan') {
            steps {
                // Сканирование образа с Trivy
                sh '''
                    trivy image \
                        --exit-code 1 \
                        --severity CRITICAL,HIGH \
                        --ignore-unfixed \
                        --format table \
                        banking-service:${BUILD_NUMBER}
                '''
            }
        }
        stage('Push') {
            when {
                expression { currentBuild.result == null }
            }
            steps {
                sh 'docker push registry.bank.local/banking-service:${BUILD_NUMBER}'
            }
        }
    }
}
```

**Интеграция в GitHub Actions:**

```yaml
- name: Scan image with Trivy
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: 'my-registry/banking-service:${{ github.sha }}'
    format: 'sarif'
    output: 'trivy-results.sarif'
    severity: 'CRITICAL,HIGH'
    exit-code: '1'

- name: Upload Trivy scan results
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: 'trivy-results.sarif'
```

**Рекомендации для банков:** Сканировать образы при каждой сборке и периодически (ежедневно) пересканировать уже развёрнутые образы, так как новые CVE появляются постоянно.

[к оглавлению](#Безопасность-контейнеров)

## Что такое подпись и верификация образов?

Подпись образов — механизм, гарантирующий, что образ создан доверенным источником и не был изменён с момента сборки. Это критически важно для банковских систем, где необходимо обеспечить целостность артефактов.

**1. Docker Content Trust (DCT):**

DCT использует Notary для подписи тегов образов:

```bash
# Включить обязательную верификацию подписей
export DOCKER_CONTENT_TRUST=1

# При push образа Docker автоматически подпишет его
docker push registry.bank.local/banking-service:1.0.0

# При pull Docker откажется скачивать неподписанный образ
docker pull registry.bank.local/banking-service:1.0.0
```

При первом использовании DCT генерирует ключи: root key (хранить в оффлайн-хранилище) и repository key (используется для подписи).

**2. Cosign (Sigstore) — современный стандарт:**

```bash
# Генерация ключевой пары
cosign generate-key-pair

# Подпись образа
cosign sign --key cosign.key registry.bank.local/banking-service@sha256:abc123...

# Верификация подписи
cosign verify --key cosign.pub registry.bank.local/banking-service@sha256:abc123...

# Подпись с использованием keyless (через OIDC/Fulcio)
cosign sign registry.bank.local/banking-service@sha256:abc123...
```

**3. Верификация в Kubernetes с помощью Kyverno:**

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: verify-image-signature
spec:
  validationFailureAction: Enforce
  background: false
  rules:
  - name: check-cosign-signature
    match:
      any:
      - resources:
          kinds:
          - Pod
    verifyImages:
    - imageReferences:
      - "registry.bank.local/*"
      attestors:
      - count: 1
        entries:
        - keys:
            publicKeys: |-
              -----BEGIN PUBLIC KEY-----
              MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE...
              -----END PUBLIC KEY-----
```

Эта политика запретит запуск любого пода с образом из `registry.bank.local`, если образ не подписан соответствующим ключом.

**4. Верификация с помощью Connaisseur (admission controller):**

Connaisseur — альтернативный admission webhook, который проверяет подписи образов перед допуском в кластер.

**Интеграция подписи в CI/CD:**

```yaml
# GitHub Actions
- name: Sign image with Cosign
  run: |
    cosign sign \
      --key env://COSIGN_PRIVATE_KEY \
      --yes \
      registry.bank.local/banking-service@${{ steps.build.outputs.digest }}
  env:
    COSIGN_PRIVATE_KEY: ${{ secrets.COSIGN_KEY }}
    COSIGN_PASSWORD: ${{ secrets.COSIGN_PASSWORD }}
```

**Важно:** Всегда подписывать по digest (`@sha256:...`), а не по тегу. Тег можно переназначить на другой образ, а digest — неизменная ссылка на конкретный слой.

[к оглавлению](#Безопасность-контейнеров)

## Как правильно управлять секретами в контейнерах?

Управление секретами — одна из самых критичных задач безопасности в контейнеризированных приложениях. Неправильное обращение с секретами — частая причина утечек данных.

**Как НЕ надо хранить секреты:**

```dockerfile
# НЕПРАВИЛЬНО: секрет в Dockerfile — сохраняется во ВСЕХ слоях образа!
ENV DB_PASSWORD=SuperSecret123
RUN echo "password=SuperSecret123" > /app/config.properties

# НЕПРАВИЛЬНО: использование ARG — видно в docker history
ARG DB_PASSWORD
RUN echo $DB_PASSWORD > /app/config.properties
```

```bash
# НЕПРАВИЛЬНО: секрет в docker run через environment variable
docker run -e DB_PASSWORD=SuperSecret123 my-app

# Секрет виден через:
docker inspect <container_id>  # в разделе Env
cat /proc/1/environ            # внутри контейнера
```

```yaml
# НЕПРАВИЛЬНО: секрет в plain-text в Kubernetes манифесте
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: app
    env:
    - name: DB_PASSWORD
      value: "SuperSecret123"  # Это plain-text в Git!
```

**Как ПРАВИЛЬНО хранить секреты:**

**1. Kubernetes Secrets (базовый уровень):**

```yaml
# Создание секрета
apiVersion: v1
kind: Secret
metadata:
  name: banking-db-credentials
  namespace: banking
type: Opaque
data:
  username: YWRtaW4=          # base64 (НЕ шифрование!)
  password: UGFzc3dvcmQxMjM=  # base64

---
# Использование секрета как volume (рекомендуемый способ)
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: app
    image: registry.bank.local/banking-service:1.0.0
    volumeMounts:
    - name: db-creds
      mountPath: /etc/secrets/db
      readOnly: true
  volumes:
  - name: db-creds
    secret:
      secretName: banking-db-credentials
      defaultMode: 0400  # Только чтение для владельца
```

```java
// Чтение секрета из файла в Spring Boot
@Value("file:/etc/secrets/db/password")
private String dbPassword;

// Или через application.yml:
// spring.datasource.password: ${file:/etc/secrets/db/password}
```

Kubernetes Secrets кодирует данные в base64 — это **не шифрование**. Для реальной защиты необходимо включить **encryption at rest**:

```yaml
# /etc/kubernetes/encryption-config.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: <base64-encoded-32-byte-key>
  - identity: {}
```

**2. HashiCorp Vault (enterprise-уровень, рекомендуется для банков):**

```yaml
# Vault Agent Injector в Kubernetes
apiVersion: apps/v1
kind: Deployment
metadata:
  name: banking-service
spec:
  template:
    metadata:
      annotations:
        vault.hashicorp.com/agent-inject: "true"
        vault.hashicorp.com/role: "banking-service"
        vault.hashicorp.com/agent-inject-secret-db-password: "secret/data/banking/db"
        vault.hashicorp.com/agent-inject-template-db-password: |
          {{- with secret "secret/data/banking/db" -}}
          {{ .Data.data.password }}
          {{- end -}}
    spec:
      serviceAccountName: banking-service
      containers:
      - name: app
        image: registry.bank.local/banking-service:1.0.0
```

Vault Agent автоматически подставит секрет в файл `/vault/secrets/db-password` внутри пода.

**3. Docker Secrets (для Docker Swarm):**

```bash
# Создание секрета
echo "SuperSecret123" | docker secret create db_password -

# Использование в docker-compose (Swarm mode)
```
```yaml
version: '3.8'
services:
  banking-service:
    image: banking-service:1.0.0
    secrets:
      - db_password
secrets:
  db_password:
    external: true
```

Секрет будет доступен внутри контейнера по пути `/run/secrets/db_password` в tmpfs (не записывается на диск).

**4. Sealed Secrets (Bitnami) — для хранения секретов в Git:**

```bash
# Зашифровать секрет публичным ключом кластера
kubeseal --format yaml < secret.yaml > sealed-secret.yaml
```

```yaml
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  name: banking-db-credentials
spec:
  encryptedData:
    password: AgBy3i...encrypted...data...==
```

Sealed Secret можно безопасно хранить в Git — расшифровать его может только контроллер в кластере.

**5. External Secrets Operator — для интеграции с внешними хранилищами:**

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: banking-db-creds
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: vault-backend
    kind: ClusterSecretStore
  target:
    name: banking-db-credentials
  data:
  - secretKey: password
    remoteRef:
      key: secret/data/banking/db
      property: password
```

[к оглавлению](#Безопасность-контейнеров)

## Как использовать multistage builds для повышения безопасности?

**Multistage build** — техника, при которой Dockerfile содержит несколько стадий (этапов), и в финальный образ копируются только необходимые артефакты. Это критически важно для безопасности, так как из финального образа исключаются компилятор, исходный код, тесты, build-утилиты и промежуточные файлы.

**Проблема без multistage build:**

```dockerfile
# ПЛОХО: всё в одном образе
FROM eclipse-temurin:21-jdk
WORKDIR /app
COPY . .
RUN ./mvnw clean package
ENTRYPOINT ["java", "-jar", "target/app.jar"]
# Результат: образ ~800 MB, содержит JDK, Maven, исходники, тесты, Git-историю
```

Этот образ содержит: JDK с компилятором, Maven/Gradle wrapper, все исходные файлы, тестовые зависимости, `.git` директорию — всё это увеличивает поверхность атаки.

**Правильный multistage build для Java-приложения:**

```dockerfile
# ============ Этап 1: Сборка ============
FROM eclipse-temurin:21-jdk-alpine AS builder

WORKDIR /build

# Кэширование зависимостей Maven
COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .
RUN ./mvnw dependency:go-offline -B

# Копирование исходного кода и сборка
COPY src ./src
RUN ./mvnw clean package -DskipTests -B && \
    # Извлечение Spring Boot layered jar для лучшего кэширования
    java -Djarmode=layertools -jar target/*.jar extract --destination /extracted

# ============ Этап 2: Финальный образ ============
FROM eclipse-temurin:21-jre-alpine

RUN addgroup -S banking && adduser -S banking -G banking

WORKDIR /app

# Копирование только скомпилированного приложения (layered)
COPY --from=builder --chown=banking:banking /extracted/dependencies/ ./
COPY --from=builder --chown=banking:banking /extracted/spring-boot-loader/ ./
COPY --from=builder --chown=banking:banking /extracted/snapshot-dependencies/ ./
COPY --from=builder --chown=banking:banking /extracted/application/ ./

USER banking

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

**Multistage build с кастомным JRE (jlink):**

```dockerfile
# Этап 1: Сборка
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /build
COPY . .
RUN ./mvnw clean package -DskipTests

# Этап 2: Определение нужных Java-модулей
FROM eclipse-temurin:21-jdk-alpine AS jre-builder
COPY --from=builder /build/target/*.jar /app/app.jar
RUN jar xf /app/app.jar && \
    jdeps \
        --ignore-missing-deps \
        --print-module-deps \
        --multi-release 21 \
        --class-path 'BOOT-INF/lib/*' \
        /app/app.jar > /modules.txt && \
    jlink \
        --add-modules $(cat /modules.txt) \
        --strip-debug \
        --no-man-pages \
        --no-header-files \
        --compress=zip-9 \
        --output /custom-jre

# Этап 3: Минимальный финальный образ
FROM alpine:3.20
COPY --from=jre-builder /custom-jre /opt/java
COPY --from=builder /build/target/*.jar /app/app.jar

RUN addgroup -S app && adduser -S app -G app
USER app

ENV JAVA_HOME=/opt/java
ENV PATH="${JAVA_HOME}/bin:${PATH}"

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

**Что удаляется из финального образа:**

| Компонент | Угроза | Удалён? |
|-----------|--------|---------|
| JDK (javac, jdb) | Компиляция вредоносного кода на месте | Да |
| Maven/Gradle | Скачивание зависимостей извне | Да |
| Исходный код | Реверс-инжиниринг бизнес-логики | Да |
| Тестовый код и данные | Утечка тестовых данных / учётных данных | Да |
| Build-утилиты (git, curl) | Инструменты для атаки | Да |
| `.git` директория | История изменений, возможные секреты в коммитах | Да |

Также не забывайте о `.dockerignore`:

```
.git
.gitignore
*.md
docker-compose*.yml
.env
.idea
*.iml
target
!target/*.jar
```

[к оглавлению](#Безопасность-контейнеров)

## Зачем и как использовать read-only файловую систему в контейнерах?

Read-only файловая система предотвращает модификацию файлов контейнера во время выполнения. Это защищает от: внедрения вредоносного кода, модификации конфигурации, записи бэкдоров, изменения бинарных файлов приложения.

**В Docker:**

```bash
# Запуск с read-only ФС
docker run --read-only \
    --tmpfs /tmp:rw,noexec,nosuid,size=100m \
    --tmpfs /var/cache:rw,noexec,nosuid \
    my-java-app
```

Флаги tmpfs:
- `rw` — разрешить запись (только в tmpfs);
- `noexec` — запретить выполнение файлов из этой директории;
- `nosuid` — игнорировать setuid-бит;
- `size=100m` — ограничение размера.

**В Kubernetes:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: banking-service
spec:
  containers:
  - name: app
    image: registry.bank.local/banking-service:1.0.0
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    # Java нуждается в /tmp для временных файлов
    - name: tmp-volume
      mountPath: /tmp
    # Логи приложения
    - name: logs-volume
      mountPath: /app/logs
    # Spring Boot может создавать temporary файлы
    - name: spring-tmp
      mountPath: /app/BOOT-INF/tmp
  volumes:
  - name: tmp-volume
    emptyDir:
      medium: Memory    # Хранить в RAM (как tmpfs)
      sizeLimit: 100Mi
  - name: logs-volume
    emptyDir:
      sizeLimit: 500Mi
  - name: spring-tmp
    emptyDir:
      medium: Memory
      sizeLimit: 50Mi
```

**Типичные директории, требующие записи для Java-приложений:**

| Директория | Зачем нужна | Рекомендация |
|------------|-------------|--------------|
| `/tmp` | Временные файлы JVM, Tomcat | `emptyDir` с `medium: Memory` |
| `/app/logs` | Логи приложения | `emptyDir` или PVC |
| `/var/cache` | Кэш HTTP-клиентов | `emptyDir` |
| `/home/appuser/.java` | Preferences API | `emptyDir` |

**Dockerfile с учётом read-only ФС:**

```dockerfile
FROM eclipse-temurin:21-jre-alpine

RUN addgroup -S app && adduser -S app -G app && \
    mkdir -p /app /tmp && \
    chown -R app:app /app /tmp

WORKDIR /app
COPY --chown=app:app target/service.jar /app/app.jar

USER app

# Указать Java использовать /tmp для временных файлов
ENTRYPOINT ["java", "-Djava.io.tmpdir=/tmp", "-jar", "/app/app.jar"]
```

[к оглавлению](#Безопасность-контейнеров)

## Что такое Network Policies в Kubernetes и как они защищают приложение?

**Network Policies** — ресурс Kubernetes, определяющий правила сетевого взаимодействия между подами. По умолчанию в Kubernetes все поды могут свободно общаться друг с другом — это опасно, особенно в банковских системах.

Network Policies работают по принципу файрвола: можно ограничить как входящий (ingress), так и исходящий (egress) трафик.

**Важно:** Network Policies требуют CNI-плагин с поддержкой политик (Calico, Cilium, Weave Net). Стандартный flannel не поддерживает Network Policies.

**1. Запрет всего трафика по умолчанию (deny all):**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: banking
spec:
  podSelector: {}  # Применить ко всем подам в namespace
  policyTypes:
  - Ingress
  - Egress
```

Это рекомендуемая отправная точка — начинаем с полного запрета, затем открываем только необходимое.

**2. Разрешить входящий трафик от конкретного сервиса:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-api-to-payment-service
  namespace: banking
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: api-gateway
    ports:
    - protocol: TCP
      port: 8080
```

Эта политика разрешает подам с меткой `app: api-gateway` подключаться к `payment-service` на порт 8080. Все остальные подключения к `payment-service` запрещены.

**3. Разрешить исходящий трафик только к определённым сервисам:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: payment-service-egress
  namespace: banking
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes:
  - Egress
  egress:
  # Разрешить доступ к базе данных
  - to:
    - podSelector:
        matchLabels:
          app: postgres
    ports:
    - protocol: TCP
      port: 5432
  # Разрешить DNS-запросы
  - to:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          k8s-app: kube-dns
    ports:
    - protocol: UDP
      port: 53
    - protocol: TCP
      port: 53
  # Разрешить доступ к Kafka
  - to:
    - podSelector:
        matchLabels:
          app: kafka
    ports:
    - protocol: TCP
      port: 9092
```

**4. Изоляция namespace-ов (межсервисная изоляция):**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-from-other-namespaces
  namespace: banking
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  ingress:
  # Разрешить трафик только из своего namespace
  - from:
    - podSelector: {}
  # Разрешить трафик из namespace мониторинга
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: monitoring
```

**Типичная архитектура сетевых политик для банковского приложения:**

```
Internet → Ingress Controller → API Gateway → [Banking Services] → Database
                                     ↕                 ↕
                                  Auth Service      Kafka
                                     ↕
                                   Vault
```

Каждая стрелка — отдельная Network Policy. Всё, что не разрешено явно, запрещено.

[к оглавлению](#Безопасность-контейнеров)

## Что такое Pod Security Standards и Pod Security Admission?

**Pod Security Standards (PSS)** — это набор из трёх предопределённых уровней безопасности для подов в Kubernetes, заменивший устаревший PodSecurityPolicy (удалён в K8s 1.25).

**Pod Security Admission (PSA)** — встроенный admission controller, который применяет эти стандарты.

**Три уровня Pod Security Standards:**

| Уровень | Описание | Когда использовать |
|---------|----------|-------------------|
| **Privileged** | Без ограничений | Системные компоненты (CNI, мониторинг) |
| **Baseline** | Минимальные ограничения, предотвращающие известные повышения привилегий | Некритичные приложения |
| **Restricted** | Максимальные ограничения, следующие лучшим практикам | Банковские приложения, production |

**Что запрещает каждый уровень:**

**Baseline** запрещает:
- `hostNetwork`, `hostPID`, `hostIPC`
- Привилегированные контейнеры
- Определённые capabilities (NET_RAW и др.)
- Монтирование hostPath
- Определённые типы volume

**Restricted** дополнительно запрещает:
- Запуск от root
- Privilege escalation
- Все capabilities, кроме NET_BIND_SERVICE
- Требует seccomp-профиль
- Требует read-only root filesystem (рекомендуется)

**Настройка через labels на namespace:**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: banking
  labels:
    # Режимы: enforce, audit, warn
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/enforce-version: latest
    pod-security.kubernetes.io/audit: restricted
    pod-security.kubernetes.io/audit-version: latest
    pod-security.kubernetes.io/warn: restricted
    pod-security.kubernetes.io/warn-version: latest
```

- **enforce** — блокирует создание подов, нарушающих политику;
- **audit** — разрешает, но записывает нарушение в audit log;
- **warn** — разрешает, но показывает предупреждение пользователю.

**Пример пода, соответствующего уровню Restricted:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: compliant-banking-app
  namespace: banking
spec:
  securityContext:
    runAsNonRoot: true
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: registry.bank.local/banking-service:1.0.0
    securityContext:
      runAsUser: 1000
      runAsGroup: 1000
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
          - ALL
    resources:
      limits:
        memory: "512Mi"
        cpu: "500m"
      requests:
        memory: "256Mi"
        cpu: "250m"
    volumeMounts:
    - name: tmp
      mountPath: /tmp
  volumes:
  - name: tmp
    emptyDir: {}
  automountServiceAccountToken: false
```

**Стратегия миграции:**
1. Начать с `warn` и `audit` на уровне restricted — увидеть, какие поды не соответствуют.
2. Исправить манифесты.
3. Включить `enforce: restricted`.

Для банковских систем рекомендуется **restricted** уровень во всех production namespace-ах.

[к оглавлению](#Безопасность-контейнеров)

## Что такое Security Context в Kubernetes?

**Security Context** — набор параметров безопасности, определяющих привилегии и контроль доступа для пода или отдельного контейнера. Это основной механизм настройки безопасности на уровне рабочей нагрузки.

Security Context может задаваться на двух уровнях:
- **Pod-level** (`spec.securityContext`) — применяется ко всем контейнерам в поде.
- **Container-level** (`spec.containers[].securityContext`) — применяется к конкретному контейнеру и переопределяет pod-level.

**Полный пример Security Context:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-banking-pod
spec:
  # Pod-level security context
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    runAsGroup: 1000
    fsGroup: 1000
    fsGroupChangePolicy: OnRootMismatch
    seccompProfile:
      type: RuntimeDefault
    supplementalGroups: [2000]

  containers:
  - name: banking-app
    image: registry.bank.local/banking-service:1.0.0

    # Container-level security context
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      privileged: false
      capabilities:
        drop:
          - ALL
        # Добавить только если реально нужно:
        # add:
        #   - NET_BIND_SERVICE
      procMount: Default
```

**Ключевые параметры:**

**1. Linux Capabilities:**

Capabilities дробят root-привилегии на отдельные единицы. Вместо полного root вы даёте только необходимое:

```yaml
securityContext:
  capabilities:
    drop:
      - ALL           # Удалить ВСЕ capabilities
    add:
      - NET_BIND_SERVICE  # Привязка к портам < 1024 (если нужно)
```

Распространённые capabilities:

| Capability | Описание | Нужна ли Java-приложению? |
|-----------|----------|--------------------------|
| NET_BIND_SERVICE | Привязка к портам < 1024 | Редко (используйте порты > 1024) |
| NET_RAW | Raw sockets (ping) | Нет |
| SYS_ADMIN | Широкие admin-привилегии | Никогда |
| SYS_PTRACE | Трассировка процессов | Нет (только для отладки) |
| CHOWN | Изменение владельца файлов | Нет |

**2. Seccomp (Secure Computing):**

Seccomp ограничивает набор системных вызовов, доступных контейнеру:

```yaml
securityContext:
  seccompProfile:
    type: RuntimeDefault  # Используется профиль container runtime
    # Или кастомный профиль:
    # type: Localhost
    # localhostProfile: profiles/banking-app.json
```

Пример кастомного seccomp-профиля:

```json
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "names": [
        "read", "write", "open", "close", "stat", "fstat",
        "mmap", "mprotect", "munmap", "brk",
        "socket", "connect", "accept", "bind", "listen",
        "clone", "execve", "exit_group",
        "futex", "epoll_wait", "epoll_ctl"
      ],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
```

**3. AppArmor:**

AppArmor ограничивает доступ контейнера к файлам и операциям на уровне LSM (Linux Security Module):

```yaml
metadata:
  annotations:
    container.apparmor.security.beta.kubernetes.io/banking-app: runtime/default
    # Или кастомный профиль:
    # container.apparmor.security.beta.kubernetes.io/banking-app: localhost/banking-profile
```

**4. SELinux:**

```yaml
securityContext:
  seLinuxOptions:
    level: "s0:c123,c456"
    type: "container_t"
```

**Рекомендуемый минимальный Security Context для банковского Java-приложения:**

```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  runAsGroup: 1000
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  privileged: false
  capabilities:
    drop: ["ALL"]
  seccompProfile:
    type: RuntimeDefault
```

[к оглавлению](#Безопасность-контейнеров)

## Как работает RBAC в Kubernetes?

**RBAC (Role-Based Access Control)** — механизм авторизации в Kubernetes, определяющий, кто (субъект) может выполнять какие действия (глаголы) над какими ресурсами. RBAC включён по умолчанию в Kubernetes и является основным инструментом управления доступом.

**Основные объекты RBAC:**

| Объект | Scope | Описание |
|--------|-------|----------|
| **Role** | Namespace | Набор разрешений в пределах namespace |
| **ClusterRole** | Кластер | Набор разрешений на уровне кластера |
| **RoleBinding** | Namespace | Привязка Role/ClusterRole к субъекту в namespace |
| **ClusterRoleBinding** | Кластер | Привязка ClusterRole к субъекту на уровне кластера |

**Субъекты:**
- **User** — обычный пользователь (управляется внешней системой аутентификации).
- **Group** — группа пользователей.
- **ServiceAccount** — учётная запись для подов и внутренних сервисов.

**1. Создание ServiceAccount для Java-приложения:**

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: banking-service-sa
  namespace: banking
automountServiceAccountToken: false  # Не монтировать токен по умолчанию
```

**2. Role с минимальными правами:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: banking-service-role
  namespace: banking
rules:
# Чтение ConfigMaps (для конфигурации приложения)
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["get", "watch"]
  resourceNames: ["banking-service-config"]  # Только конкретный ConfigMap!

# Чтение секретов (только конкретных)
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get"]
  resourceNames: ["banking-db-credentials"]
```

**3. RoleBinding:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: banking-service-binding
  namespace: banking
subjects:
- kind: ServiceAccount
  name: banking-service-sa
  namespace: banking
roleRef:
  kind: Role
  name: banking-service-role
  apiGroup: rbac.authorization.k8s.io
```

**4. Использование ServiceAccount в Deployment:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: banking-service
  namespace: banking
spec:
  template:
    spec:
      serviceAccountName: banking-service-sa
      automountServiceAccountToken: true  # Включить, только если нужно
      containers:
      - name: app
        image: registry.bank.local/banking-service:1.0.0
```

**5. ClusterRole для кросс-namespace доступа (мониторинг):**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: monitoring-reader
rules:
- apiGroups: [""]
  resources: ["pods", "services", "endpoints"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["metrics.k8s.io"]
  resources: ["pods"]
  verbs: ["get", "list"]
```

**Проверка прав:**

```bash
# Проверить, может ли ServiceAccount получить секреты
kubectl auth can-i get secrets \
    --as=system:serviceaccount:banking:banking-service-sa \
    -n banking

# Проверить все права ServiceAccount
kubectl auth can-i --list \
    --as=system:serviceaccount:banking:banking-service-sa \
    -n banking
```

**Типичные ошибки RBAC:**
- Привязка `cluster-admin` к сервисному аккаунту приложения.
- Разрешение `*` на все ресурсы и глаголы.
- Использование default ServiceAccount (имеет автомонтируемый токен).
- Отсутствие `resourceNames` — доступ ко всем ресурсам данного типа.

**Рекомендации для банков:**
- Каждый микросервис — свой ServiceAccount с минимальными правами.
- Регулярный аудит RBAC (инструменты: `kubectl-who-can`, `rbac-lookup`, `rakkess`).
- Интеграция с корпоративным IdP (LDAP/AD) через OIDC.
- Запрет `automountServiceAccountToken` по умолчанию.

[к оглавлению](#Безопасность-контейнеров)

## Как ограничение ресурсов защищает от DoS-атак?

Ограничение ресурсов (resource limits) в Kubernetes и Docker предотвращает ситуацию, когда один контейнер потребляет все ресурсы хоста, лишая других контейнеров возможности работать. Это защита от **DoS (Denial of Service)** — как от злонамеренных атак, так и от ошибок в коде (утечки памяти, бесконечные циклы).

**В Kubernetes (requests и limits):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: banking-service
  namespace: banking
spec:
  template:
    spec:
      containers:
      - name: app
        image: registry.bank.local/banking-service:1.0.0
        resources:
          requests:
            memory: "256Mi"  # Гарантированные ресурсы для планировщика
            cpu: "250m"      # 0.25 CPU core
          limits:
            memory: "512Mi"  # Максимум — при превышении OOMKill
            cpu: "1000m"     # 1 CPU core — throttling при превышении
        env:
        - name: JAVA_OPTS
          value: >-
            -XX:MaxRAMPercentage=75.0
            -XX:+UseG1GC
            -XX:+ExitOnOutOfMemoryError
```

**Разница между requests и limits:**
- **requests** — минимальные гарантированные ресурсы. Используются планировщиком для размещения пода на ноде.
- **limits** — максимально допустимое потребление. При превышении memory — OOMKill. При превышении CPU — throttling.

**Настройка JVM с учётом limits:**

```dockerfile
ENTRYPOINT ["java", \
    "-XX:MaxRAMPercentage=75.0", \
    "-XX:InitialRAMPercentage=50.0", \
    "-XX:+UseContainerSupport", \
    "-XX:+ExitOnOutOfMemoryError", \
    "-jar", "/app/app.jar"]
```

`-XX:+UseContainerSupport` (включён по умолчанию с Java 10+) — JVM автоматически определяет limits контейнера и настраивает heap соответственно. `-XX:MaxRAMPercentage=75.0` — выделить 75% от доступной памяти контейнера под heap (25% остаётся для metaspace, стеков потоков, native memory).

**LimitRange — ограничения по умолчанию для namespace:**

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: banking-limits
  namespace: banking
spec:
  limits:
  - type: Container
    default:
      memory: "512Mi"
      cpu: "500m"
    defaultRequest:
      memory: "256Mi"
      cpu: "250m"
    max:
      memory: "2Gi"
      cpu: "2"
    min:
      memory: "64Mi"
      cpu: "50m"
  - type: Pod
    max:
      memory: "4Gi"
      cpu: "4"
```

**ResourceQuota — ограничение суммарных ресурсов namespace:**

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: banking-quota
  namespace: banking
spec:
  hard:
    requests.cpu: "10"
    requests.memory: "20Gi"
    limits.cpu: "20"
    limits.memory: "40Gi"
    pods: "50"
    services: "20"
    secrets: "30"
    configmaps: "30"
```

**В Docker:**

```bash
docker run \
    --memory=512m \
    --memory-swap=512m \    # Запретить swap
    --cpus=1.0 \
    --pids-limit=200 \      # Ограничение числа процессов (fork-bomb protection)
    --ulimit nofile=1024:2048 \
    my-java-app
```

**Сценарии атак, которые предотвращают limits:**

| Атака | Без limits | С limits |
|-------|-----------|----------|
| Утечка памяти | Падение всего хоста | OOMKill только этого пода |
| Бесконечный цикл (100% CPU) | Деградация всех сервисов | Throttling только этого пода |
| Fork bomb | Исчерпание PID-ов хоста | `pids-limit` ограничит число процессов |
| Заполнение диска логами | Диск полон для всех | `emptyDir.sizeLimit` + `ephemeral-storage` limits |

[к оглавлению](#Безопасность-контейнеров)

## Что такое Supply Chain Security в контексте контейнеров?

**Supply Chain Security (безопасность цепочки поставок)** — обеспечение целостности и безопасности всех компонентов, участвующих в создании и доставке программного обеспечения: от исходного кода и зависимостей до финального контейнерного образа.

Атаки на supply chain (как SolarWinds, Log4Shell, codecov) показали, что компрометация одного звена может затронуть тысячи организаций.

**Основные компоненты Supply Chain Security:**

**1. SBOM (Software Bill of Materials) — перечень всех компонентов:**

```bash
# Генерация SBOM с помощью Trivy (формат CycloneDX)
trivy image --format cyclonedx \
    --output sbom.cdx.json \
    registry.bank.local/banking-service:1.0.0

# Генерация SBOM с помощью Syft (Anchore)
syft registry.bank.local/banking-service:1.0.0 -o spdx-json > sbom.spdx.json

# Анализ SBOM на уязвимости
grype sbom:./sbom.cdx.json
```

SBOM содержит: список всех пакетов ОС, Java-зависимости (Maven/Gradle), их версии, лицензии, хеши.

**2. Проверка зависимостей:**

```xml
<!-- Maven: OWASP Dependency Check Plugin -->
<plugin>
    <groupId>org.owasp</groupId>
    <artifactId>dependency-check-maven</artifactId>
    <version>10.0.3</version>
    <configuration>
        <failBuildOnCVSS>7</failBuildOnCVSS>
        <suppressionFile>dependency-check-suppressions.xml</suppressionFile>
    </configuration>
</plugin>
```

```bash
# Запуск проверки
./mvnw dependency-check:check
```

```groovy
// Gradle
plugins {
    id 'org.owasp.dependencycheck' version '10.0.3'
}

dependencyCheck {
    failBuildOnCVSS = 7.0f
    analyzers {
        nodeEnabled = false
    }
}
```

**3. Фиксация версий зависимостей:**

```xml
<!-- Использовать конкретные версии, НЕ диапазоны -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>3.3.0</version> <!-- Конкретная версия -->
</dependency>

<!-- НЕ ИСПОЛЬЗОВАТЬ: -->
<version>[3.0,4.0)</version> <!-- Диапазон версий -->
<version>LATEST</version>    <!-- Последняя версия -->
```

**4. Верификация артефактов в Docker:**

```dockerfile
FROM eclipse-temurin:21-jre-alpine@sha256:a1b2c3d4...  # Pin по digest

# Проверка checksums скачиваемых файлов
RUN wget https://example.com/tool.tar.gz && \
    echo "expected_sha256  tool.tar.gz" | sha256sum -c - && \
    tar xzf tool.tar.gz
```

**5. SLSA (Supply-chain Levels for Software Artifacts):**

SLSA определяет четыре уровня зрелости безопасности цепочки поставок:

| Уровень | Требования |
|---------|-----------|
| SLSA 1 | Процесс сборки документирован |
| SLSA 2 | Сборка на CI/CD (не локально), подписанная provenance |
| SLSA 3 | Аудитируемая и защищённая от вмешательства CI/CD |
| SLSA 4 | Двусторонняя проверка, hermetic build |

**6. Provenance (происхождение артефакта):**

```bash
# Генерация attestation с помощью cosign
cosign attest --predicate provenance.json \
    --key cosign.key \
    registry.bank.local/banking-service@sha256:abc123...

# Верификация attestation
cosign verify-attestation \
    --key cosign.pub \
    --type slsaprovenance \
    registry.bank.local/banking-service@sha256:abc123...
```

**7. Полный пайплайн supply chain security:**

```yaml
# GitHub Actions пример
name: Secure Build Pipeline
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write  # Для keyless signing
    steps:
    - uses: actions/checkout@v4

    - name: Dependency Check
      run: ./mvnw dependency-check:check

    - name: Build
      run: ./mvnw clean package -DskipTests

    - name: Build & Push Image
      id: build
      run: |
        docker build -t registry.bank.local/banking-service:${{ github.sha }} .
        docker push registry.bank.local/banking-service:${{ github.sha }}

    - name: Generate SBOM
      uses: anchore/sbom-action@v0
      with:
        image: registry.bank.local/banking-service:${{ github.sha }}
        format: cyclonedx-json
        output-file: sbom.cdx.json

    - name: Scan for vulnerabilities
      run: trivy image --exit-code 1 --severity CRITICAL registry.bank.local/banking-service:${{ github.sha }}

    - name: Sign Image
      run: cosign sign --yes registry.bank.local/banking-service:${{ github.sha }}

    - name: Attach SBOM to Image
      run: cosign attach sbom --sbom sbom.cdx.json registry.bank.local/banking-service:${{ github.sha }}
```

[к оглавлению](#Безопасность-контейнеров)

## Что такое Docker Bench for Security?

**Docker Bench for Security** — автоматизированный инструмент аудита безопасности, проверяющий конфигурацию Docker-хоста и контейнеров на соответствие рекомендациям **CIS Docker Benchmark** (Center for Internet Security).

**Запуск:**

```bash
# Запуск Docker Bench for Security
docker run --rm --net host --pid host --userns host --cap-add audit_control \
    -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST \
    -v /var/lib:/var/lib:ro \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    -v /usr/lib/systemd:/usr/lib/systemd:ro \
    -v /etc:/etc:ro \
    docker/docker-bench-security
```

**Категории проверок:**

| Секция | Описание | Примеры проверок |
|--------|----------|-----------------|
| 1 | Host Configuration | Аудит-логирование, отдельный раздел для /var/lib/docker |
| 2 | Docker daemon configuration | TLS, usernamespace, live-restore |
| 3 | Docker daemon configuration files | Права на docker.sock, daemon.json |
| 4 | Container Images and Build Files | USER в Dockerfile, HEALTHCHECK, нет секретов |
| 5 | Container Runtime | Не privileged, не host network, read-only FS |
| 6 | Docker Security Operations | Регулярное обновление, мониторинг |
| 7 | Docker Swarm | Шифрование overlay-сети, ротация сертификатов |

**Пример вывода:**

```
[INFO] 1 - Host Configuration
[PASS] 1.1  - Ensure a separate partition for containers has been created
[WARN] 1.2  - Ensure the container host has been hardened
[PASS] 1.3  - Ensure Docker is up to date

[INFO] 4 - Container Images and Build File
[WARN] 4.1  - Ensure a user for the container has been created
[PASS] 4.2  - Ensure that containers use only trusted base images
[WARN] 4.6  - Ensure HEALTHCHECK instructions have been added to the container image
[WARN] 4.9  - Ensure that COPY is used instead of ADD in Dockerfiles

[INFO] 5 - Container Runtime
[WARN] 5.1  - Ensure AppArmor Profile is enabled
[PASS] 5.2  - Ensure SELinux security options are set
[WARN] 5.4  - Ensure privileged containers are not used
[WARN] 5.12 - Ensure the container's root filesystem is mounted as read only
[WARN] 5.25 - Ensure the container is restricted from acquiring additional privileges
```

**Как реагировать на результаты:**

Каждый WARN требует внимания. Для банковских систем цель — минимизировать количество WARN и устранить все FAIL.

**Автоматизация в CI/CD:**

```groovy
stage('Docker Bench Audit') {
    steps {
        sh '''
            docker run --rm \
                -v /var/run/docker.sock:/var/run/docker.sock:ro \
                -v /etc:/etc:ro \
                docker/docker-bench-security \
                -l /dev/stdout 2>&1 | tee bench-results.txt

            # Проверить наличие FAIL
            if grep -q "\\[FAIL\\]" bench-results.txt; then
                echo "Docker Bench found FAIL results!"
                exit 1
            fi
        '''
    }
}
```

Также существует **kube-bench** — аналог для Kubernetes, проверяющий CIS Kubernetes Benchmark:

```bash
# Запуск kube-bench
kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job.yaml
kubectl logs job/kube-bench
```

[к оглавлению](#Безопасность-контейнеров)

## Как защитить Docker daemon?

Docker daemon (`dockerd`) работает с правами root и управляет всеми контейнерами на хосте. Компрометация Docker daemon = полный контроль над хостом. Защита daemon — одна из приоритетных задач.

**1. Защита Docker socket:**

По умолчанию Docker слушает на Unix-сокете `/var/run/docker.sock`. Доступ к этому сокету равнозначен root-доступу к хосту.

```bash
# Проверить права на сокет
ls -la /var/run/docker.sock
# srw-rw---- 1 root docker 0 ... /var/run/docker.sock

# Убедиться, что только пользователи группы docker имеют доступ
# НЕ монтировать сокет в контейнеры без крайней необходимости!
```

**НИКОГДА** не делайте так в production:

```yaml
# ОПАСНО!
volumes:
  - /var/run/docker.sock:/var/run/docker.sock
```

Если всё же необходимо (например, для CI runner), используйте read-only монтирование и ограниченные авторизационные плагины.

**2. Включение TLS для удалённого доступа:**

```bash
# Генерация CA
openssl genrsa -aes256 -out ca-key.pem 4096
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem

# Генерация серверного ключа
openssl genrsa -out server-key.pem 4096
openssl req -subj "/CN=docker-host.bank.local" -sha256 \
    -new -key server-key.pem -out server.csr

# Подписание серверного сертификата
echo subjectAltName = DNS:docker-host.bank.local,IP:10.0.1.100 >> extfile.cnf
echo extendedKeyUsage = serverAuth >> extfile.cnf
openssl x509 -req -days 365 -sha256 \
    -in server.csr -CA ca.pem -CAkey ca-key.pem \
    -CAcreateserial -out server-cert.pem -extfile extfile.cnf

# Генерация клиентского ключа
openssl genrsa -out client-key.pem 4096
openssl req -subj '/CN=client' -new -key client-key.pem -out client.csr
echo extendedKeyUsage = clientAuth > extfile-client.cnf
openssl x509 -req -days 365 -sha256 \
    -in client.csr -CA ca.pem -CAkey ca-key.pem \
    -CAcreateserial -out client-cert.pem -extfile extfile-client.cnf
```

**Конфигурация daemon.json:**

```json
{
  "tls": true,
  "tlscacert": "/etc/docker/certs/ca.pem",
  "tlscert": "/etc/docker/certs/server-cert.pem",
  "tlskey": "/etc/docker/certs/server-key.pem",
  "tlsverify": true,
  "hosts": ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2376"],
  "icc": false,
  "userns-remap": "default",
  "no-new-privileges": true,
  "live-restore": true,
  "userland-proxy": false,
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 64000,
      "Soft": 64000
    }
  }
}
```

Ключевые параметры:
- `tlsverify: true` — обязательная проверка клиентского сертификата.
- `icc: false` — запрет межконтейнерного сетевого взаимодействия по умолчанию.
- `userns-remap: default` — маппинг UID (root в контейнере ≠ root на хосте).
- `no-new-privileges: true` — запрет повышения привилегий.
- `live-restore: true` — контейнеры продолжают работать при перезапуске daemon.

**3. Docker Authorization Plugin:**

```json
{
  "authorization-plugins": ["casbin-authz-plugin"]
}
```

Authorization plugin позволяет определять политики: кто может запускать какие контейнеры с какими параметрами. Например, запретить `--privileged` или монтирование определённых путей.

**4. Rootless Docker:**

```bash
# Установка rootless Docker
dockerd-rootless-setuptool.sh install

# Docker daemon работает от обычного пользователя
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```

Rootless Docker полностью исключает работу от root, но имеет ограничения: нельзя использовать некоторые storage drivers, нет привязки к портам < 1024 без дополнительных настроек.

**5. Ограничение логов (защита от заполнения диска):**

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

[к оглавлению](#Безопасность-контейнеров)

## Как работает изоляция контейнеров на уровне ядра Linux?

Контейнеры — это **не виртуальные машины**. Они используют одно ядро Linux с хостом, а изоляция обеспечивается механизмами ядра: **namespaces**, **cgroups** и **union filesystems**. Понимание этих механизмов важно для оценки реальной степени изоляции.

**1. Linux Namespaces (изоляция видимости):**

Namespaces создают иллюзию того, что у контейнера собственная операционная система:

| Namespace | Что изолирует | Флаг | Описание |
|-----------|--------------|------|----------|
| **PID** | Процессы | CLONE_NEWPID | Контейнер видит только свои процессы (PID 1 — entrypoint) |
| **NET** | Сеть | CLONE_NEWNET | Свой сетевой стек, IP-адрес, интерфейсы, порты |
| **MNT** | Файловая система | CLONE_NEWNS | Свои точки монтирования |
| **UTS** | Hostname | CLONE_NEWUTS | Свой hostname |
| **IPC** | Межпроцессное общение | CLONE_NEWIPC | Свои семафоры, очереди сообщений |
| **USER** | Пользователи | CLONE_NEWUSER | Свои UID/GID (root в контейнере ≠ root на хосте) |
| **Cgroup** | Cgroup view | CLONE_NEWCGROUP | Своя иерархия cgroups |

```bash
# Посмотреть namespaces контейнера
docker inspect --format '{{.State.Pid}}' my-container
# Например, PID = 12345

ls -la /proc/12345/ns/
# lrwxrwxrwx 1 root root 0 ... cgroup -> cgroup:[4026532234]
# lrwxrwxrwx 1 root root 0 ... ipc -> ipc:[4026532232]
# lrwxrwxrwx 1 root root 0 ... mnt -> mnt:[4026532230]
# lrwxrwxrwx 1 root root 0 ... net -> net:[4026532235]
# lrwxrwxrwx 1 root root 0 ... pid -> pid:[4026532233]
# lrwxrwxrwx 1 root root 0 ... user -> user:[4026531837]
# lrwxrwxrwx 1 root root 0 ... uts -> uts:[4026532231]
```

**2. Cgroups (Control Groups — ограничение ресурсов):**

Cgroups ограничивают, сколько ресурсов может потребить контейнер:

| Подсистема | Что ограничивает |
|------------|-----------------|
| **cpu** | Процессорное время (CPU shares, quotas) |
| **memory** | Оперативная память (limits, swap) |
| **blkio** | Disk I/O (пропускная способность, IOPS) |
| **pids** | Количество процессов |
| **cpuset** | Привязка к конкретным ядрам CPU |
| **devices** | Доступ к устройствам |

```bash
# Cgroup-лимиты контейнера
cat /sys/fs/cgroup/memory/docker/<container_id>/memory.limit_in_bytes
cat /sys/fs/cgroup/cpu/docker/<container_id>/cpu.cfs_quota_us
cat /sys/fs/cgroup/pids/docker/<container_id>/pids.max
```

Для Java важно: JVM с `-XX:+UseContainerSupport` (по умолчанию с Java 10+) корректно читает cgroup-лимиты и адаптирует размер heap.

**3. Seccomp (системные вызовы):**

Seccomp фильтрует системные вызовы, доступные контейнеру. Docker по умолчанию блокирует ~44 системных вызова из ~300+, включая: `reboot`, `mount`, `swapon`, `clock_settime`, `bpf` и другие опасные вызовы.

**4. Capabilities:**

Linux делит привилегии root на ~40 отдельных capabilities. Docker по умолчанию оставляет только необходимый минимум:

```bash
# Capabilities контейнера по умолчанию
docker run --rm alpine cat /proc/1/status | grep Cap
# CapPrm: 00000000a80425fb
# Включает: CHOWN, DAC_OVERRIDE, FSETID, FOWNER, NET_RAW, ...
```

**5. Ограничения изоляции контейнеров:**

Контейнеры разделяют ядро Linux с хостом. Это означает:
- Уязвимость ядра = потенциальный побег из контейнера (container escape).
- `/proc` и `/sys` частично доступны.
- `--privileged` полностью отключает изоляцию.
- Время (clock) общее для всех контейнеров.

**6. Усиление изоляции (для высокочувствительных систем):**

| Технология | Описание |
|-----------|----------|
| **gVisor** | Виртуальное ядро в user-space (перехватывает syscalls) |
| **Kata Containers** | Лёгкие VM с ядром и OCI-совместимостью |
| **Firecracker** | microVM (используется в AWS Lambda / Fargate) |

```yaml
# Использование gVisor в Kubernetes
apiVersion: v1
kind: Pod
spec:
  runtimeClassName: gvisor  # Указать runtime class
  containers:
  - name: banking-app
    image: registry.bank.local/banking-service:1.0.0
```

Для банковских систем с обработкой платёжных данных стоит рассмотреть Kata Containers или gVisor для дополнительного уровня изоляции.

[к оглавлению](#Безопасность-контейнеров)

## Что такое Runtime Security и как работает Falco?

**Runtime Security** — обнаружение и реагирование на аномальное поведение контейнеров в реальном времени. В отличие от сканирования образов (shift-left), runtime security работает на этапе выполнения и обнаруживает атаки, которые не могут быть выявлены статическим анализом.

**Falco (CNCF, graduated project)** — ведущий open-source инструмент runtime security для контейнеров и Kubernetes.

**Как работает Falco:**
1. Перехватывает системные вызовы через eBPF (или kernel module).
2. Анализирует их по набору правил.
3. Генерирует оповещения при обнаружении аномалий.

**Установка в Kubernetes:**

```bash
# Через Helm
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
helm install falco falcosecurity/falco \
    --namespace falco \
    --create-namespace \
    --set driver.kind=ebpf \
    --set falcosidekick.enabled=true \
    --set falcosidekick.config.slack.webhookurl="https://hooks.slack.com/..."
```

**Примеры правил Falco:**

```yaml
# Обнаружение запуска shell в контейнере
- rule: Terminal shell in container
  desc: A shell was used as the entrypoint/exec into a container
  condition: >
    spawned_process and container
    and shell_procs
    and proc.pname exists
    and not proc.pname in (shell_binaries)
  output: >
    Shell spawned in container
    (user=%user.name user_loginuid=%user.loginuid
    container_id=%container.id container_name=%container.name
    shell=%proc.name parent=%proc.pname cmdline=%proc.cmdline
    image=%container.image.repository)
  priority: WARNING
  tags: [container, shell, mitre_execution]

# Обнаружение чтения чувствительных файлов
- rule: Read sensitive file in container
  desc: Attempt to read sensitive files (e.g., /etc/shadow)
  condition: >
    sensitive_files and open_read
    and container
    and not proc.name in (allowed_sensitive_readers)
  output: >
    Sensitive file opened for reading in container
    (user=%user.name file=%fd.name container=%container.name
    image=%container.image.repository)
  priority: WARNING

# Обнаружение исходящих подключений к нестандартным портам
- rule: Unexpected outbound connection
  desc: Container making unexpected outbound network connection
  condition: >
    outbound and container
    and not fd.sport in (80, 443, 8080, 8443, 5432, 9092, 6379)
    and not k8s.ns.name = "kube-system"
  output: >
    Unexpected outbound connection from container
    (command=%proc.cmdline connection=%fd.name
    container=%container.name image=%container.image.repository
    namespace=%k8s.ns.name)
  priority: NOTICE

# Обнаружение записи в /etc
- rule: Write below etc
  desc: Attempt to write to /etc directory in container
  condition: >
    write_etc_common and container
  output: >
    File below /etc opened for writing in container
    (user=%user.name file=%fd.name container=%container.name
    image=%container.image.repository)
  priority: ERROR

# Обнаружение запуска криптомайнера
- rule: Detect crypto miners
  desc: Detect common crypto mining processes
  condition: >
    spawned_process and container
    and (proc.name in (xmrig, minergate, minerd, cpuminer)
    or proc.cmdline contains "stratum+tcp")
  output: >
    Crypto mining process detected in container
    (user=%user.name process=%proc.name container=%container.name
    image=%container.image.repository)
  priority: CRITICAL
```

**Falcosidekick — маршрутизация оповещений:**

Falcosidekick принимает оповещения от Falco и отправляет их в различные системы:

```yaml
# values.yaml для Falcosidekick
falcosidekick:
  config:
    slack:
      webhookurl: "https://hooks.slack.com/services/..."
      minimumpriority: "warning"
    elasticsearch:
      hostport: "http://elasticsearch:9200"
      index: "falco"
    prometheus:
      enabled: true
    # Автоматическая реакция: убить под при CRITICAL
    kubernetesPolicyReport:
      enabled: true
```

**Типичные инциденты, обнаруживаемые Falco в банковских системах:**

| Инцидент | Severity | Действие |
|----------|----------|---------|
| Запуск shell (`/bin/sh`) в production-контейнере | WARNING | Уведомление в SOC |
| Чтение `/etc/shadow` | ERROR | Уведомление + расследование |
| Запуск процесса не из entrypoint | NOTICE | Логирование |
| Исходящее подключение к неизвестному IP | WARNING | Уведомление |
| Модификация бинарных файлов | CRITICAL | Уведомление + автоматическое удаление пода |
| Криптомайнер | CRITICAL | Немедленная эскалация |

**Альтернативы Falco:** Sysdig Secure (коммерческий), Aqua Security, Prisma Cloud (Palo Alto).

[к оглавлению](#Безопасность-контейнеров)

## Как организовать процесс обновления и патчинга базовых образов?

Уязвимости обнаруживаются постоянно. Образ, безопасный сегодня, завтра может содержать критическую CVE. Регулярное обновление базовых образов — обязательный процесс для банковских систем.

**1. Стратегия pin-ования версий:**

```dockerfile
# ПЛОХО — неизвестно, что получим завтра
FROM eclipse-temurin:21-jre

# ЛУЧШЕ — фиксация минорной версии
FROM eclipse-temurin:21.0.4_7-jre-alpine

# ЛУЧШЕЕ — pin по digest (неизменная ссылка)
FROM eclipse-temurin@sha256:1a2b3c4d5e6f...
```

**2. Автоматическое обнаружение обновлений с Renovate/Dependabot:**

```json
// renovate.json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:base"],
  "docker": {
    "enabled": true,
    "pinDigests": true
  },
  "packageRules": [
    {
      "matchDatasources": ["docker"],
      "matchPackageNames": ["eclipse-temurin"],
      "automerge": false,
      "schedule": ["every weekday"],
      "groupName": "base-image-updates"
    }
  ],
  "vulnerabilityAlerts": {
    "enabled": true,
    "labels": ["security"]
  }
}
```

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "weekly"
    reviewers:
      - "security-team"
    labels:
      - "docker"
      - "dependencies"

  - package-ecosystem: "maven"
    directory: "/"
    schedule:
      interval: "daily"
    open-pull-requests-limit: 10
```

**3. Периодическое пересканирование развёрнутых образов:**

```yaml
# CronJob для ежедневного сканирования
apiVersion: batch/v1
kind: CronJob
metadata:
  name: image-scan
  namespace: security
spec:
  schedule: "0 6 * * *"  # Каждый день в 6:00
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: scanner
            image: aquasec/trivy:latest
            command:
            - /bin/sh
            - -c
            - |
              # Получить список всех образов в кластере
              for image in $(kubectl get pods -A -o jsonpath='{.items[*].spec.containers[*].image}' | tr ' ' '\n' | sort -u); do
                echo "Scanning: $image"
                trivy image --severity CRITICAL,HIGH --exit-code 0 "$image"
              done
          restartPolicy: OnFailure
          serviceAccountName: image-scanner-sa
```

**4. Процесс патчинга для банковского проекта:**

```
1. Обнаружение (автоматическое):
   ├── Renovate/Dependabot создаёт PR с обновлением
   ├── Trivy обнаруживает CVE в развёрнутом образе
   └── Подписка на security advisories (GitHub, Alpine SecDB)

2. Оценка:
   ├── CVSS score ≥ 9.0 (Critical) → патч в течение 24 часов
   ├── CVSS score 7.0-8.9 (High) → патч в течение 7 дней
   ├── CVSS score 4.0-6.9 (Medium) → патч в следующем релизе
   └── CVSS score < 4.0 (Low) → бэклог

3. Патчинг:
   ├── Обновить базовый образ в Dockerfile
   ├── Обновить зависимости (mvn/gradle)
   ├── Прогнать тесты
   ├── Сканировать новый образ
   └── Деплой через стандартный CI/CD пайплайн

4. Верификация:
   ├── Повторное сканирование после деплоя
   └── Мониторинг стабильности сервиса
```

**5. Пересборка образов по расписанию:**

```yaml
# GitHub Actions: еженедельная пересборка
name: Weekly Rebuild
on:
  schedule:
    - cron: '0 2 * * 1'  # Каждый понедельник в 2:00 UTC
  workflow_dispatch: {}

jobs:
  rebuild:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Build with latest base image
      run: docker build --pull --no-cache -t banking-service:weekly .
    - name: Scan
      run: trivy image --exit-code 1 --severity CRITICAL banking-service:weekly
    - name: Push if clean
      run: docker push registry.bank.local/banking-service:weekly
```

Флаг `--pull` гарантирует скачивание свежего базового образа, а `--no-cache` — полную пересборку.

[к оглавлению](#Безопасность-контейнеров)

## Как встроить проверки безопасности в CI/CD пайплайн?

**Security gates** — обязательные проверки безопасности в пайплайне, блокирующие продвижение артефакта при обнаружении проблем. Принцип **shift-left security** подразумевает выявление уязвимостей как можно раньше.

**Полный безопасный пайплайн (Jenkinsfile):**

```groovy
pipeline {
    agent any

    environment {
        REGISTRY = 'registry.bank.local'
        IMAGE_NAME = 'banking-service'
        IMAGE_TAG = "${BUILD_NUMBER}-${GIT_COMMIT.take(8)}"
    }

    stages {
        // ===== Stage 1: Статический анализ кода =====
        stage('SAST - Static Analysis') {
            parallel {
                stage('SonarQube') {
                    steps {
                        sh '''
                            ./mvnw sonar:sonar \
                                -Dsonar.host.url=${SONAR_URL} \
                                -Dsonar.token=${SONAR_TOKEN} \
                                -Dsonar.qualitygate.wait=true
                        '''
                    }
                }
                stage('SpotBugs + FindSecBugs') {
                    steps {
                        sh './mvnw spotbugs:check'
                    }
                }
                stage('Secret Detection') {
                    steps {
                        // Поиск секретов в коде
                        sh 'trivy fs --scanners secret --exit-code 1 .'
                    }
                }
            }
        }

        // ===== Stage 2: Проверка зависимостей =====
        stage('SCA - Dependency Check') {
            steps {
                sh '''
                    ./mvnw dependency-check:check \
                        -DfailBuildOnCVSS=7 \
                        -Dformat=JSON \
                        -DprettyPrint=true
                '''
            }
            post {
                always {
                    archiveArtifacts 'target/dependency-check-report.json'
                }
            }
        }

        // ===== Stage 3: Сборка и тесты =====
        stage('Build & Test') {
            steps {
                sh './mvnw clean verify'
            }
        }

        // ===== Stage 4: Сборка Docker-образа =====
        stage('Docker Build') {
            steps {
                sh """
                    docker build \
                        --no-cache \
                        --label "org.opencontainers.image.revision=${GIT_COMMIT}" \
                        --label "org.opencontainers.image.created=\$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
                        -t ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        // ===== Stage 5: Сканирование образа =====
        stage('Image Security Scan') {
            parallel {
                stage('Trivy Scan') {
                    steps {
                        sh """
                            trivy image \
                                --exit-code 1 \
                                --severity CRITICAL,HIGH \
                                --ignore-unfixed \
                                --format json \
                                --output trivy-report.json \
                                ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                        """
                    }
                }
                stage('Dockerfile Lint') {
                    steps {
                        sh 'hadolint Dockerfile --failure-threshold warning'
                    }
                }
                stage('SBOM Generation') {
                    steps {
                        sh """
                            trivy image \
                                --format cyclonedx \
                                --output sbom.cdx.json \
                                ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                        """
                    }
                }
            }
        }

        // ===== Stage 6: Проверка K8s-манифестов =====
        stage('IaC Security') {
            steps {
                sh '''
                    # Проверка Kubernetes манифестов
                    trivy config --exit-code 1 --severity CRITICAL,HIGH k8s/

                    # Или с помощью kubesec
                    kubesec scan k8s/deployment.yaml
                '''
            }
        }

        // ===== Stage 7: Push и подпись =====
        stage('Push & Sign') {
            steps {
                sh """
                    docker push ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}

                    # Подпись образа
                    cosign sign --key \${COSIGN_KEY} \
                        ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}

                    # Прикрепление SBOM к образу
                    cosign attach sbom \
                        --sbom sbom.cdx.json \
                        ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        // ===== Stage 8: DAST (опционально) =====
        stage('DAST - Dynamic Testing') {
            when {
                branch 'develop'
            }
            steps {
                sh '''
                    # Деплой в тестовую среду
                    kubectl apply -f k8s/ -n testing

                    # Запуск OWASP ZAP
                    docker run --rm owasp/zap2docker-stable zap-baseline.py \
                        -t http://banking-service.testing.svc:8080 \
                        -r zap-report.html
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '*.json, *.html', allowEmptyArchive: true
        }
        failure {
            // Уведомление команды безопасности
            slackSend channel: '#security-alerts',
                color: 'danger',
                message: "Security gate FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}
```

**Типы проверок безопасности:**

| Тип | Инструменты | Что проверяет | Когда |
|-----|------------|---------------|-------|
| **SAST** | SonarQube, SpotBugs, Semgrep | Исходный код | При коммите |
| **SCA** | OWASP Dep Check, Snyk | Зависимости | При сборке |
| **Secret Scan** | Trivy, GitLeaks, TruffleHog | Утечки секретов | При коммите |
| **Image Scan** | Trivy, Snyk, Grype | Docker-образ | После сборки |
| **IaC Scan** | Trivy, Checkov, kubesec | K8s манифесты, Dockerfile | При коммите |
| **DAST** | OWASP ZAP | Работающее приложение | После деплоя |
| **Lint** | hadolint, yamllint | Dockerfile, YAML | При коммите |

**Ключевой принцип:** пайплайн должен **блокировать** продвижение артефакта при обнаружении критических уязвимостей (`--exit-code 1`). Для банковских систем ни одна CRITICAL-уязвимость не должна попасть в production.

[к оглавлению](#Безопасность-контейнеров)

## Что такое OWASP Docker Top 10?

**OWASP Docker Top 10** — список наиболее критичных рисков безопасности при использовании Docker, подготовленный OWASP (Open Web Application Security Project). Этот список помогает расставить приоритеты в обеспечении безопасности контейнеров.

**D01 — Secure User Mapping (Безопасное сопоставление пользователей):**

Не запускать контейнеры от root. Использовать `USER` в Dockerfile и `runAsNonRoot` в Kubernetes.

```dockerfile
# Правильно
RUN addgroup -S app && adduser -S app -G app
USER app
```

**D02 — Patch Management Strategy (Стратегия управления патчами):**

Регулярное обновление базовых образов и зависимостей. Автоматическое сканирование, Renovate/Dependabot, пересборка образов по расписанию.

**D03 — Network Segmentation and Firewalling (Сегментация сети):**

Применение Network Policies в Kubernetes. Принцип deny-by-default. Изоляция чувствительных сервисов.

```yaml
# Deny all, then allow specific
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes: [Ingress, Egress]
```

**D04 — Secure Defaults and Hardening (Безопасные настройки по умолчанию):**

Hardening Docker daemon, отключение ICC, использование seccomp, AppArmor. Применение CIS Benchmark (Docker Bench).

**D05 — Maintain Security Contexts (Поддержание контекста безопасности):**

Настройка Security Context для каждого пода: drop ALL capabilities, read-only FS, no privilege escalation.

**D06 — Protect Secrets (Защита секретов):**

Никогда не хранить секреты в образах, переменных окружения или Git. Использовать Vault, Sealed Secrets, External Secrets Operator.

**D07 — Resource Protection (Защита ресурсов):**

Обязательное ограничение CPU, памяти, PID, disk I/O. Использование LimitRange и ResourceQuota.

```yaml
resources:
  limits:
    memory: "512Mi"
    cpu: "1"
  requests:
    memory: "256Mi"
    cpu: "250m"
```

**D08 — Container Image Integrity and Origin (Целостность и происхождение образов):**

Подпись образов (Cosign/DCT), верификация при деплое (Kyverno/OPA Gatekeeper), использование только проверенных реестров.

**D09 — Follow Immutable Paradigm (Неизменяемая парадигма):**

Контейнеры должны быть неизменяемыми (immutable). Read-only FS, запрет модификации файлов в runtime. Обновление — это замена контейнера, а не изменение существующего.

**D10 — Logging (Логирование):**

Централизованный сбор логов (ELK/EFK stack), аудит действий Docker daemon, мониторинг runtime событий (Falco).

**Чек-лист OWASP Docker Top 10 для банковского проекта:**

```
[  ] D01: Все контейнеры запускаются от non-root
[  ] D02: Настроено автоматическое сканирование и обновление
[  ] D03: Network Policies применены, deny-by-default
[  ] D04: Docker daemon hardened, CIS Benchmark пройден
[  ] D05: Security Context настроен для всех подов
[  ] D06: Секреты хранятся в Vault/Sealed Secrets
[  ] D07: Resource limits установлены для всех контейнеров
[  ] D08: Образы подписаны и верифицируются при деплое
[  ] D09: Read-only FS, immutable контейнеры
[  ] D10: Централизованное логирование, Falco для runtime security
```

[к оглавлению](#Безопасность-контейнеров)

## Как обеспечить безопасность Java-приложения в контейнере в банковской среде?

Это комплексный вопрос, объединяющий все аспекты контейнерной безопасности применительно к реальному банковскому Java-приложению.

**1. Безопасный Dockerfile для банковского Java-приложения:**

```dockerfile
# ============ Этап 1: Сборка ============
FROM eclipse-temurin:21-jdk-alpine@sha256:abc123... AS builder

WORKDIR /build

# Отдельные слои для зависимостей (лучшее кэширование)
COPY pom.xml mvnw ./
COPY .mvn .mvn
RUN ./mvnw dependency:go-offline -B

COPY src ./src
RUN ./mvnw clean package -DskipTests -B \
    && java -Djarmode=layertools -jar target/*.jar extract --destination /layers

# ============ Этап 2: Финальный образ ============
FROM eclipse-temurin:21-jre-alpine@sha256:def456...

# Метаданные OCI
LABEL org.opencontainers.image.title="banking-payment-service" \
      org.opencontainers.image.vendor="Bank JSC" \
      org.opencontainers.image.authors="platform-team@bank.local"

# Пользователь и группа
RUN addgroup -g 1000 -S banking && \
    adduser -u 1000 -S banking -G banking && \
    mkdir -p /app /tmp && \
    chown -R banking:banking /app /tmp

WORKDIR /app

# Копирование слоёв Spring Boot
COPY --from=builder --chown=banking:banking /layers/dependencies/ ./
COPY --from=builder --chown=banking:banking /layers/spring-boot-loader/ ./
COPY --from=builder --chown=banking:banking /layers/snapshot-dependencies/ ./
COPY --from=builder --chown=banking:banking /layers/application/ ./

# Переключение на непривилегированного пользователя
USER banking:banking

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --start-period=60s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["java", \
    "-XX:+UseContainerSupport", \
    "-XX:MaxRAMPercentage=75.0", \
    "-XX:+ExitOnOutOfMemoryError", \
    "-Djava.io.tmpdir=/tmp", \
    "-Djava.security.egd=file:/dev/./urandom", \
    "-Dspring.profiles.active=production", \
    "org.springframework.boot.loader.launch.JarLauncher"]
```

**2. Kubernetes Deployment:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: payment-service
  namespace: banking-production
  labels:
    app: payment-service
    tier: backend
    compliance: pci-dss
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: payment-service
  template:
    metadata:
      labels:
        app: payment-service
        tier: backend
      annotations:
        vault.hashicorp.com/agent-inject: "true"
        vault.hashicorp.com/role: "payment-service"
        vault.hashicorp.com/agent-inject-secret-db: "secret/data/banking/payment-db"
    spec:
      serviceAccountName: payment-service-sa
      automountServiceAccountToken: false

      securityContext:
        runAsNonRoot: true
        fsGroup: 1000
        seccompProfile:
          type: RuntimeDefault

      # Топология: распределение по разным нодам
      topologySpreadConstraints:
      - maxSkew: 1
        topologyKey: kubernetes.io/hostname
        whenUnsatisfiable: DoNotSchedule
        labelSelector:
          matchLabels:
            app: payment-service

      containers:
      - name: payment-service
        image: registry.bank.local/payment-service@sha256:789xyz...
        imagePullPolicy: Always

        securityContext:
          runAsUser: 1000
          runAsGroup: 1000
          allowPrivilegeEscalation: false
          readOnlyRootFilesystem: true
          privileged: false
          capabilities:
            drop: ["ALL"]

        ports:
        - containerPort: 8080
          name: http
          protocol: TCP

        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
            ephemeral-storage: "100Mi"
          limits:
            memory: "1Gi"
            cpu: "2"
            ephemeral-storage: "500Mi"

        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: http
          initialDelaySeconds: 60
          periodSeconds: 10
          failureThreshold: 3

        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: http
          initialDelaySeconds: 30
          periodSeconds: 5
          failureThreshold: 3

        volumeMounts:
        - name: tmp
          mountPath: /tmp
        - name: logs
          mountPath: /app/logs

        env:
        - name: JAVA_OPTS
          value: "-XX:MaxRAMPercentage=75.0"
        - name: SPRING_PROFILES_ACTIVE
          value: "production"

      volumes:
      - name: tmp
        emptyDir:
          medium: Memory
          sizeLimit: 100Mi
      - name: logs
        emptyDir:
          sizeLimit: 500Mi

      imagePullSecrets:
      - name: registry-credentials
```

**3. Network Policy для payment-service:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: payment-service-policy
  namespace: banking-production
spec:
  podSelector:
    matchLabels:
      app: payment-service
  policyTypes:
  - Ingress
  - Egress

  ingress:
  # Только от API Gateway
  - from:
    - podSelector:
        matchLabels:
          app: api-gateway
    ports:
    - protocol: TCP
      port: 8080

  egress:
  # К базе данных
  - to:
    - podSelector:
        matchLabels:
          app: payment-db
    ports:
    - protocol: TCP
      port: 5432
  # К Kafka
  - to:
    - podSelector:
        matchLabels:
          app: kafka
    ports:
    - protocol: TCP
      port: 9092
  # DNS
  - to:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          k8s-app: kube-dns
    ports:
    - protocol: UDP
      port: 53
    - protocol: TCP
      port: 53
  # К Vault
  - to:
    - namespaceSelector:
        matchLabels:
          name: vault
    ports:
    - protocol: TCP
      port: 8200
```

**4. Настройка namespace безопасности:**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: banking-production
  labels:
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/audit: restricted
    pod-security.kubernetes.io/warn: restricted
    compliance: pci-dss
```

**5. Spring Boot Security конфигурация:**

```yaml
# application-production.yml
server:
  port: 8080
  ssl:
    enabled: false  # TLS terminates at Ingress/Service Mesh

management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  endpoint:
    health:
      show-details: never  # Не показывать детали в production
      probes:
        enabled: true

spring:
  jackson:
    default-property-inclusion: non_null
    serialization:
      fail-on-empty-beans: false

# Logging: не логировать чувствительные данные
logging:
  level:
    org.springframework.security: WARN
    org.hibernate.SQL: WARN
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
```

**6. Итоговый чек-лист безопасности для банковского Java-приложения в контейнерах:**

```
ОБРАЗ:
  [  ] Multistage build (исходники не в финальном образе)
  [  ] Минимальный базовый образ (Alpine/Distroless)
  [  ] Pin по digest, не по тегу
  [  ] USER non-root
  [  ] HEALTHCHECK указан
  [  ] Нет секретов в слоях образа
  [  ] COPY вместо ADD
  [  ] .dockerignore настроен

KUBERNETES:
  [  ] runAsNonRoot: true
  [  ] readOnlyRootFilesystem: true
  [  ] allowPrivilegeEscalation: false
  [  ] capabilities: drop ALL
  [  ] seccompProfile: RuntimeDefault
  [  ] Resource limits установлены
  [  ] Network Policies применены
  [  ] ServiceAccount с минимальными правами
  [  ] automountServiceAccountToken: false
  [  ] Pod Security Standards: restricted

СЕКРЕТЫ:
  [  ] Vault / Sealed Secrets / External Secrets
  [  ] Encryption at rest для K8s Secrets
  [  ] Нет секретов в Git, env, Dockerfile

CI/CD:
  [  ] SAST (SonarQube, SpotBugs)
  [  ] SCA (Dependency Check, Snyk)
  [  ] Image scanning (Trivy)
  [  ] Secret detection
  [  ] Dockerfile linting (hadolint)
  [  ] K8s manifest scanning
  [  ] Подпись образов (Cosign)
  [  ] SBOM генерация

RUNTIME:
  [  ] Falco для мониторинга
  [  ] Централизованное логирование
  [  ] Мониторинг (Prometheus + Grafana)
  [  ] Регулярное обновление базовых образов
```

Безопасность контейнеров — не разовое действие, а непрерывный процесс. В банковской среде это требование регуляторов (ЦБ РФ, PCI DSS), и его несоблюдение влечёт серьёзные финансовые и репутационные риски.

[к оглавлению](#Безопасность-контейнеров)
