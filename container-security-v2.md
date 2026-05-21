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

---

## Какие основные угрозы безопасности контейнеров существуют?
<!-- grade: junior -->

Угрозы безопасности контейнеров -- это совокупность рисков, возникающих на каждом уровне жизненного цикла контейнеризированного приложения: от сборки образа до работы в продакшене.

Представьте контейнер как квартиру в многоэтажном доме. Угрозы могут исходить от соседей (другие контейнеры на том же хосте), от поставщиков мебели (зависимости и базовые образы), от незапертых дверей (неправильная конфигурация) и от самого жильца (запущенный процесс).

### Угрозы на уровне образов

- **Уязвимости в базовых образах** -- устаревшие пакеты и библиотеки с известными CVE (Common Vulnerabilities and Exposures). Каждый непропатченный пакет -- потенциальная точка входа для атакующего.
- **Вредоносные образы** -- использование непроверенных образов из публичных реестров (Docker Hub) может привести к внедрению троянов или бэкдоров.
- **Утечка секретов** -- пароли, ключи API, сертификаты, случайно включённые в слои образа. Даже если секрет удалён в последующем слое, он остаётся доступен через `docker history`.
- **Избыточные компоненты** -- лишние утилиты и инструменты (curl, wget, bash) увеличивают поверхность атаки, предоставляя атакующему инструментарий для дальнейшего проникновения.

### Угрозы на уровне среды выполнения (runtime)

- **Побег из контейнера (container escape)** -- эксплуатация уязвимостей ядра Linux для получения доступа к хост-системе. Контейнеры разделяют ядро с хостом, что делает этот вектор особенно опасным.
- **Запуск от root** -- контейнер с правами root (UID 0) при побеге даёт атакующему root-доступ на хосте.
- **Привилегированный режим** -- флаг `--privileged` даёт контейнеру полный доступ к устройствам хоста, фактически снимая изоляцию.
- **Незащищённый Docker socket** -- доступ к `/var/run/docker.sock` позволяет управлять всеми контейнерами на хосте, включая создание новых привилегированных контейнеров.

### Угрозы на уровне оркестрации (Kubernetes)

- **Неправильная настройка RBAC** -- избыточные права для сервисных аккаунтов позволяют скомпрометированному поду получить контроль над кластером.
- **Отсутствие сетевых политик** -- по умолчанию любой под может обращаться к любому другому поду в кластере, что облегчает боковое перемещение (lateral movement).
- **Незащищённый API-сервер** -- открытый доступ к Kubernetes API без аутентификации позволяет любому управлять кластером.
- **Компрометация etcd** -- хранилище всех данных кластера, включая секреты в открытом виде (если не настроено шифрование at rest).

### Угрозы на уровне цепочки поставок (supply chain)

- **Подмена зависимостей** (dependency confusion) -- вредоносные пакеты с именами, совпадающими с внутренними библиотеками организации.
- **Компрометация CI/CD** -- внедрение вредоносного кода через пайплайн сборки (например, подмена шагов в Jenkinsfile или GitHub Actions).
- **Отсутствие проверки целостности** -- использование образов без верификации подписи не гарантирует, что образ не был подменён.

### Угрозы на уровне сети

- **Man-in-the-middle** -- перехват трафика между контейнерами при отсутствии mTLS.
- **Незашифрованный трафик** -- передача данных без TLS внутри кластера позволяет перехватить чувствительную информацию.
- **Эксфильтрация данных** -- несанкционированная отправка данных наружу через незаблокированный egress-трафик.

### Вывод

Для банковских систем особенно критичны: утечка персональных данных (ФЗ-152), утечка платёжных данных (PCI DSS), несанкционированный доступ к финансовым транзакциям. Эшелонированная защита (defense in depth) предполагает применение мер на каждом уровне одновременно.

> **На собеседовании:** обычно ожидают, что кандидат перечислит хотя бы 3-4 категории угроз и приведёт конкретные примеры. Упомяните container escape, запуск от root, утечку секретов в образе и отсутствие Network Policies -- это покажет понимание разных уровней атаки.

[к оглавлению](#Безопасность-контейнеров)

---

## Что такое принцип наименьших привилегий и как он применяется в контейнерах?
<!-- grade: junior -->

Принцип наименьших привилегий (Principle of Least Privilege, PoLP) -- это фундаментальный принцип информационной безопасности, согласно которому каждый субъект (пользователь, процесс, контейнер) должен иметь только те права и ресурсы, которые минимально необходимы для выполнения его задачи.

Представьте кассира в банке: ему дают ключ только от своей кассы, а не от хранилища. Контейнеру точно так же нужно давать только те привилегии, без которых он не может работать.

### Применение в Docker

**Запуск процессов не от root** -- указание непривилегированного пользователя через инструкцию `USER` в Dockerfile:

```dockerfile
FROM eclipse-temurin:21-jre-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

**Удаление лишних capabilities** -- Linux capabilities позволяют дробить монолитные root-привилегии на отдельные единицы:

```bash
# Запуск с удалением всех capabilities и добавлением только нужных
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE my-java-app
```

**Запрет привилегированного режима и эскалации привилегий:**

```bash
# Никогда не использовать в production:
docker run --privileged my-app  # ОПАСНО!

# Правильно:
docker run --security-opt=no-new-privileges my-app
```

**Read-only файловая система:**

```bash
docker run --read-only --tmpfs /tmp my-java-app
```

### Применение в Kubernetes

<details>
<summary>Полный манифест пода с минимальными привилегиями</summary>

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
  automountServiceAccountToken: false
```

</details>

### Ключевые параметры

| Параметр | Назначение |
|----------|------------|
| `runAsNonRoot: true` | Запрещает запуск от root |
| `allowPrivilegeEscalation: false` | Блокирует `setuid`-бинарники и получение дополнительных привилегий дочерними процессами |
| `capabilities.drop: [ALL]` | Удаляет все Linux capabilities |
| `readOnlyRootFilesystem: true` | Запрещает запись в файловую систему контейнера |
| `automountServiceAccountToken: false` | Отключает монтирование токена сервисного аккаунта, если поду не нужен доступ к Kubernetes API |
| `seccompProfile.type: RuntimeDefault` | Ограничивает набор доступных системных вызовов |

### Вывод

Принцип наименьших привилегий -- это не одна настройка, а комплекс мер: отказ от root, удаление capabilities, read-only ФС, запрет эскалации и отключение ненужных токенов. Регуляторные требования (PCI DSS Requirement 7) прямо требуют ограничения доступа к данным по принципу need-to-know.

> **На собеседовании:** покажите, что знаете не только определение, но и конкретные механизмы реализации. Перечислите `runAsNonRoot`, `cap-drop=ALL`, `readOnlyRootFilesystem`, `allowPrivilegeEscalation: false` -- это набор, который ожидают услышать.

[к оглавлению](#Безопасность-контейнеров)

---

## Как запустить контейнер не от пользователя root?
<!-- grade: junior -->

Запуск контейнера не от пользователя root -- это практика создания и использования непривилегированного пользователя (с UID отличным от 0) для выполнения процесса внутри контейнера, что предотвращает получение атакующим root-прав на хосте при побеге из контейнера.

### Инструкция USER в Dockerfile

Основной способ -- создать непривилегированного пользователя и переключиться на него:

<details>
<summary>Пример Dockerfile с непривилегированным пользователем</summary>

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

</details>

### Директива runAsNonRoot в Kubernetes

Kubernetes может принудительно запретить запуск контейнеров от root:

<details>
<summary>Пример Deployment с runAsNonRoot</summary>

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
        runAsNonRoot: true
        fsGroup: 1000
      containers:
      - name: app
        image: registry.bank.local/banking-service:1.0.0
        securityContext:
          runAsUser: 1000
          runAsGroup: 1000
          allowPrivilegeEscalation: false
```

</details>

Если в образе `USER` указан root, а в Kubernetes выставлен `runAsNonRoot: true`, кластер **откажется запустить** под и выдаст ошибку `CreateContainerConfigError`.

### User namespace remapping в Docker daemon

Этот механизм сопоставляет UID 0 внутри контейнера с непривилегированным UID на хосте:

```json
// /etc/docker/daemon.json
{
  "userns-remap": "default"
}
```

Даже если процесс внутри контейнера работает как root (UID 0), на хосте он будет обычным пользователем (например, UID 100000). Это обеспечивает дополнительный уровень защиты при побеге из контейнера.

### Типичные проблемы при запуске не от root

| Проблема | Решение |
|----------|---------|
| Нет прав на запись в `/app/logs` | `RUN mkdir /app/logs && chown appuser:appgroup /app/logs` |
| Порт ниже 1024 (например, 80) | Использовать порт 8080+ или добавить `NET_BIND_SERVICE` capability |
| Java не может создать временные файлы | Примонтировать `tmpfs` или `emptyDir` в `/tmp` |
| Не удаётся подключиться к БД через unix-socket | Настроить права на сокет или использовать TCP |

### Проверка текущего пользователя

```bash
docker exec my-container whoami
docker exec my-container id
# uid=1000(javaapp) gid=1000(javaapp) groups=1000(javaapp)
```

### Вывод

Запуск от непривилегированного пользователя -- первая и простейшая мера защиты контейнера. Комбинация инструкции `USER` в Dockerfile и `runAsNonRoot: true` в Kubernetes гарантирует, что процесс никогда не запустится от root, даже если кто-то случайно соберёт образ без `USER`.

> **На собеседовании:** достаточно показать знание трёх уровней защиты: `USER` в Dockerfile, `runAsNonRoot` в Kubernetes, и user namespace remapping как дополнительный барьер. Также упомяните типичные проблемы -- это демонстрирует практический опыт.

[к оглавлению](#Безопасность-контейнеров)

---

## Зачем использовать минимальные базовые образы и какие варианты существуют?
<!-- grade: junior -->

Минимальный базовый образ -- это образ контейнера, содержащий только компоненты, необходимые для запуска приложения, что сокращает поверхность атаки (attack surface), уменьшает размер и количество потенциальных уязвимостей.

Аналогия: если вы переезжаете в новую квартиру, вы берёте только нужные вещи. Оставлять в образе компилятор, отладчик и пакетный менеджер -- всё равно что привезти с собой набор отмычек и оставить его на видном месте.

### Сравнение базовых образов для Java

| Образ | Размер | Shell | Package Manager | Уязвимости* |
|-------|--------|-------|-----------------|-------------|
| `eclipse-temurin:21-jdk` | ~470 MB | Да | apt | Много |
| `eclipse-temurin:21-jre` | ~270 MB | Да | apt | Средне |
| `eclipse-temurin:21-jre-alpine` | ~90 MB | Да (ash) | apk | Мало |
| `gcr.io/distroless/java21-debian12` | ~230 MB | Нет | Нет | Минимум |

*\* -- примерная оценка количества потенциальных уязвимостей*

### Alpine-based образы

```dockerfile
FROM eclipse-temurin:21-jre-alpine

RUN addgroup -S app && adduser -S app -G app
WORKDIR /app
COPY --chown=app:app target/service.jar /app/app.jar
USER app

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

Преимущества: маленький размер (~90 MB), musl libc, минимум пакетов. Недостатки: возможна несовместимость с некоторыми нативными библиотеками, зависящими от glibc (например, некоторые JDBC-драйверы или библиотеки с JNI).

### Google Distroless

<details>
<summary>Пример multistage build с Distroless</summary>

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

</details>

Distroless-образы не содержат shell, пакетного менеджера и прочих утилит. Атакующий, даже проникнув в контейнер, **не сможет** выполнить `sh`, `bash`, `curl`, `wget`. Для отладки существует debug-вариант `gcr.io/distroless/java21-debian12:debug` с busybox shell.

### Собственный минимальный образ с jlink (Java 9+)

<details>
<summary>Пример Dockerfile с кастомным JRE через jlink</summary>

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

</details>

Результат: образ ~60 MB вместо 470 MB, содержит только нужные Java-модули.

### Рекомендации для production

- **Production:** `distroless` или `alpine` + jlink.
- **Staging/QA:** `alpine`-варианты (есть shell для отладки).
- **Фиксация версии:** никогда не использовать тег `latest` -- всегда указывать версию с SHA256-дайджестом: `eclipse-temurin:21-jre-alpine@sha256:abc123...`

### Вывод

Выбор минимального базового образа -- один из самых эффективных способов уменьшить количество уязвимостей. Distroless-образы дают наилучшую защиту для production, а jlink позволяет создать ещё более компактный образ с кастомным JRE.

> **На собеседовании:** назовите 3-4 варианта образов (jdk, jre, alpine, distroless), объясните разницу в размере и безопасности. Упомяните jlink как продвинутый приём -- это выделит вас среди кандидатов.

[к оглавлению](#Безопасность-контейнеров)

---

## Как сканировать Docker-образы на уязвимости?
<!-- grade: middle -->

Сканирование Docker-образов на уязвимости -- это процесс автоматического анализа пакетов ОС и зависимостей приложения внутри образа с последующей сверкой с базами данных известных уязвимостей (NVD, GitHub Advisory, Alpine SecDB и др.).

### Trivy (Aqua Security)

Trivy -- один из самых популярных open-source сканеров. Поддерживает сканирование образов, файловых систем, Git-репозиториев, конфигураций Kubernetes и IaC.

```bash
# Сканирование образа
trivy image my-registry/banking-service:1.0.0

# Только CRITICAL и HIGH уязвимости
trivy image --severity CRITICAL,HIGH my-registry/banking-service:1.0.0

# Только уязвимости с доступным исправлением
trivy image --ignore-unfixed my-registry/banking-service:1.0.0

# Комплексное сканирование проекта (уязвимости + секреты + misconfig)
trivy fs --scanners vuln,secret,misconfig .

# Вывод в JSON для интеграции с CI/CD
trivy image --format json --output report.json my-registry/banking-service:1.0.0

# Завершить с ошибкой при наличии CRITICAL уязвимостей (для CI/CD gate)
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

### Другие инструменты сканирования

| Инструмент | Тип | Особенности |
|-----------|-----|-------------|
| **Trivy** | Open-source CLI | Универсальный, быстрый, поддерживает SBOM |
| **Snyk** | Коммерческий (есть бесплатный tier) | Мониторинг новых уязвимостей, интеграция с IDE |
| **Clair** (Quay) | Серверный | Интеграция с реестрами (Harbor, Quay) при push |
| **Grype** (Anchore) | Open-source CLI | Быстрый, поддерживает сканирование SBOM |

### Snyk

```bash
# Сканирование Docker-образа
snyk container test my-registry/banking-service:1.0.0

# Постоянный мониторинг (уведомления о новых CVE)
snyk container monitor my-registry/banking-service:1.0.0

# Сканирование зависимостей проекта
snyk test --all-projects
```

### Grype

```bash
grype my-registry/banking-service:1.0.0
grype sbom:./sbom.spdx.json  # сканирование на основе SBOM
```

### Интеграция в CI/CD

<details>
<summary>Пример Jenkinsfile со сканированием</summary>

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

</details>

<details>
<summary>Пример GitHub Actions со сканированием</summary>

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

</details>

### Вывод

Сканирование образов должно быть обязательным этапом CI/CD-пайплайна. Для банковских систем рекомендуется сканировать образы при каждой сборке и периодически (ежедневно) пересканировать уже развёрнутые образы, так как новые CVE появляются постоянно. Блокировка деплоя при наличии CRITICAL-уязвимостей -- обязательная практика.

> **На собеседовании:** назовите Trivy как основной инструмент, покажите знание ключевых флагов (`--exit-code 1`, `--severity`, `--ignore-unfixed`). Упомяните необходимость регулярного пересканирования уже задеплоенных образов -- это показывает зрелое понимание процесса.

[к оглавлению](#Безопасность-контейнеров)

---

## Что такое подпись и верификация образов?
<!-- grade: middle -->

Подпись образов -- это криптографический механизм, гарантирующий, что контейнерный образ создан доверенным источником и не был изменён (не был подменён или скомпрометирован) с момента сборки.

Аналогия: подпись образа -- как сургучная печать на письме. Она подтверждает отправителя и гарантирует, что содержимое не вскрывали в пути.

### Docker Content Trust (DCT)

DCT использует Notary для подписи тегов образов:

```bash
# Включить обязательную верификацию подписей
export DOCKER_CONTENT_TRUST=1

# При push Docker автоматически подпишет образ
docker push registry.bank.local/banking-service:1.0.0

# При pull Docker откажется скачивать неподписанный образ
docker pull registry.bank.local/banking-service:1.0.0
```

При первом использовании DCT генерируются два ключа: **root key** (хранить в офлайн-хранилище, например HSM) и **repository key** (используется для подписи конкретного репозитория).

### Cosign (Sigstore) -- современный стандарт

Cosign -- более современный инструмент, активно вытесняющий DCT благодаря простоте и поддержке keyless-подписи через OIDC.

```bash
# Генерация ключевой пары
cosign generate-key-pair

# Подпись образа (всегда по digest, не по тегу!)
cosign sign --key cosign.key registry.bank.local/banking-service@sha256:abc123...

# Верификация подписи
cosign verify --key cosign.pub registry.bank.local/banking-service@sha256:abc123...

# Keyless-подпись через OIDC/Fulcio (без локальных ключей)
cosign sign registry.bank.local/banking-service@sha256:abc123...
```

### Верификация в Kubernetes с помощью Kyverno

Kyverno -- policy engine, который может блокировать запуск подов с неподписанными образами:

<details>
<summary>Пример ClusterPolicy для верификации подписей</summary>

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

</details>

Эта политика запретит запуск любого пода с образом из `registry.bank.local`, если образ не подписан соответствующим ключом. Альтернативный admission controller -- **Connaisseur**.

### Интеграция подписи в CI/CD

<details>
<summary>Пример GitHub Actions с Cosign</summary>

```yaml
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

</details>

### Подпись по digest vs по тегу

| Способ ссылки | Пример | Безопасность |
|---------------|--------|-------------|
| По тегу | `registry/app:1.0.0` | Тег можно переназначить на другой образ |
| По digest | `registry/app@sha256:abc123...` | Неизменная ссылка на конкретный контент |

Всегда подписывать по **digest** (`@sha256:...`), а не по тегу.

### Вывод

Подпись образов -- обязательная практика для production-окружений. Cosign (Sigstore) стал де-факто стандартом. В связке с Kyverno или Connaisseur в Kubernetes можно гарантировать, что ни один неподписанный образ не попадёт в кластер.

> **На собеседовании:** объясните разницу между DCT и Cosign, упомяните keyless-подпись через OIDC. Обязательно скажите, что подписывать нужно по digest, а не по тегу -- это частая ошибка, которую интервьюер проверяет.

[к оглавлению](#Безопасность-контейнеров)

---

## Как правильно управлять секретами в контейнерах?
<!-- grade: middle -->

Управление секретами в контейнерах -- это набор практик безопасного хранения, доставки и ротации конфиденциальных данных (паролей, ключей API, сертификатов), исключающий их попадание в образ, исходный код или переменные окружения в открытом виде.

### Как НЕ надо хранить секреты

```dockerfile
# НЕПРАВИЛЬНО: секрет в Dockerfile -- сохраняется во ВСЕХ слоях образа!
ENV DB_PASSWORD=SuperSecret123
RUN echo "password=SuperSecret123" > /app/config.properties

# НЕПРАВИЛЬНО: использование ARG -- видно через docker history
ARG DB_PASSWORD
RUN echo $DB_PASSWORD > /app/config.properties
```

```bash
# НЕПРАВИЛЬНО: секрет в docker run через переменную окружения
docker run -e DB_PASSWORD=SuperSecret123 my-app

# Секрет виден через:
docker inspect <container_id>  # в разделе Env
cat /proc/1/environ            # внутри контейнера
```

```yaml
# НЕПРАВИЛЬНО: секрет в plain-text в Kubernetes манифесте (попадёт в Git!)
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: app
    env:
    - name: DB_PASSWORD
      value: "SuperSecret123"
```

### Kubernetes Secrets (базовый уровень)

Kubernetes Secrets кодирует данные в base64 -- это **не шифрование**, а лишь кодирование. Для реальной защиты необходимо включить **encryption at rest**.

<details>
<summary>Пример создания и использования Kubernetes Secret</summary>

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

</details>

Чтение секрета в Spring Boot:

```java
@Value("file:/etc/secrets/db/password")
private String dbPassword;
```

<details>
<summary>Настройка encryption at rest для секретов в etcd</summary>

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

</details>

### HashiCorp Vault (enterprise-уровень)

Vault -- рекомендуемое решение для банковских систем. Vault Agent Injector автоматически подставляет секреты в файлы внутри пода.

<details>
<summary>Пример интеграции Vault с Kubernetes</summary>

```yaml
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

</details>

Vault Agent автоматически подставит секрет в файл `/vault/secrets/db-password` внутри пода.

### Docker Secrets (для Docker Swarm)

```bash
# Создание секрета
echo "SuperSecret123" | docker secret create db_password -
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

Секрет доступен внутри контейнера по пути `/run/secrets/db_password` в tmpfs (не записывается на диск).

### Sealed Secrets (Bitnami) -- для хранения секретов в Git

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

Sealed Secret можно безопасно хранить в Git -- расшифровать его может только контроллер в кластере.

### External Secrets Operator -- интеграция с внешними хранилищами

<details>
<summary>Пример ExternalSecret для интеграции с Vault</summary>

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

</details>

### Сравнение подходов

| Подход | Шифрование | Ротация | GitOps-совместимость | Рекомендация |
|--------|-----------|---------|---------------------|-------------|
| Kubernetes Secrets | base64 (нет) | Ручная | Нет (plain-text) | Минимум + encryption at rest |
| Sealed Secrets | RSA | Ручная | Да | Для небольших команд |
| HashiCorp Vault | AES-256 | Автоматическая | Через ESO | Для банков и enterprise |
| External Secrets Operator | Зависит от бэкенда | Автоматическая | Да | Универсальный выбор |

### Вывод

Секреты никогда не должны попадать в образ, исходный код или переменные окружения в открытом виде. Для банковских систем рекомендуется HashiCorp Vault с Vault Agent Injector или External Secrets Operator. Минимальное требование -- Kubernetes Secrets с encryption at rest и монтированием через volume (не через env).

> **На собеседовании:** начните с антипаттернов (секрет в ENV, в Dockerfile, в plain-text манифесте), затем покажите правильные подходы. Обязательно подчеркните, что base64 -- это не шифрование. Упомяните Vault и Sealed Secrets для полноты.

[к оглавлению](#Безопасность-контейнеров)

---

## Как использовать multistage builds для повышения безопасности?
<!-- grade: junior -->

Multistage build -- это техника построения Docker-образов, при которой Dockerfile содержит несколько стадий сборки, и в финальный образ копируются только необходимые артефакты, исключая компилятор, исходный код, тесты и build-утилиты.

Аналогия: multistage build -- как кухня ресторана. Гостю (production) подают только готовое блюдо, а все ножи, разделочные доски и отходы (JDK, Maven, исходники) остаются на кухне и не попадают в зал.

### Проблема без multistage build

```dockerfile
# ПЛОХО: всё в одном образе
FROM eclipse-temurin:21-jdk
WORKDIR /app
COPY . .
RUN ./mvnw clean package
ENTRYPOINT ["java", "-jar", "target/app.jar"]
# Результат: образ ~800 MB, содержит JDK, Maven, исходники, тесты, .git
```

Этот образ содержит JDK с компилятором, Maven wrapper, все исходные файлы, тестовые зависимости, `.git` директорию -- всё это увеличивает поверхность атаки и даёт атакующему инструменты для дальнейшего проникновения.

### Правильный multistage build для Java

<details>
<summary>Multistage build с layered Spring Boot jar</summary>

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

</details>

### Multistage build с кастомным JRE (jlink)

<details>
<summary>Трёхстадийная сборка с jlink</summary>

```dockerfile
# Этап 1: Сборка приложения
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /build
COPY . .
RUN ./mvnw clean package -DskipTests

# Этап 2: Определение нужных Java-модулей и создание кастомного JRE
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

</details>

### Что удаляется из финального образа

| Компонент | Угроза | Удалён в multistage? |
|-----------|--------|---------------------|
| JDK (javac, jdb) | Компиляция вредоносного кода на месте | Да |
| Maven/Gradle | Скачивание вредоносных зависимостей извне | Да |
| Исходный код | Реверс-инжиниринг бизнес-логики | Да |
| Тестовый код и данные | Утечка тестовых учётных данных | Да |
| Build-утилиты (git, curl) | Инструменты для развития атаки | Да |
| `.git` директория | История изменений, возможные секреты в коммитах | Да |

### Файл .dockerignore

Не забывайте о `.dockerignore` -- он предотвращает копирование ненужных файлов в контекст сборки:

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

### Вывод

Multistage build -- обязательная практика для production-образов. Она уменьшает размер образа в 5-10 раз, удаляет из финального образа все build-инструменты и исходный код, что радикально сокращает поверхность атаки.

> **На собеседовании:** покажите понимание двух- или трёхстадийной сборки, объясните, почему каждый удалённый компонент -- это закрытая дверь для атакующего. Бонус -- упомяните layered jars Spring Boot и jlink для создания кастомного JRE.

[к оглавлению](#Безопасность-контейнеров)

---

## Зачем и как использовать read-only файловую систему в контейнерах?
<!-- grade: middle -->

Read-only файловая система контейнера -- это режим, при котором корневая файловая система монтируется только для чтения, запрещая любую запись в неё во время выполнения. Это защищает от внедрения вредоносного кода, модификации конфигурации, записи бэкдоров и изменения бинарных файлов приложения.

Аналогия: read-only ФС -- как стеклянная витрина в музее. Экспонаты (файлы приложения) можно смотреть, но нельзя трогать. Если атакующий проникнет в контейнер, он не сможет ничего изменить или подложить.

### В Docker

```bash
# Запуск с read-only ФС
docker run --read-only \
    --tmpfs /tmp:rw,noexec,nosuid,size=100m \
    --tmpfs /var/cache:rw,noexec,nosuid \
    my-java-app
```

Флаги tmpfs:

| Флаг | Назначение |
|------|-----------|
| `rw` | Разрешить запись (только в tmpfs) |
| `noexec` | Запретить выполнение файлов из этой директории |
| `nosuid` | Игнорировать setuid-бит |
| `size=100m` | Ограничение размера |

### В Kubernetes

<details>
<summary>Полный манифест с read-only ФС и необходимыми volume</summary>

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

</details>

### Типичные директории, требующие записи для Java-приложений

| Директория | Зачем нужна | Рекомендация |
|------------|-------------|--------------|
| `/tmp` | Временные файлы JVM, Tomcat | `emptyDir` с `medium: Memory` |
| `/app/logs` | Логи приложения | `emptyDir` или PVC |
| `/var/cache` | Кэш HTTP-клиентов | `emptyDir` |
| `/home/appuser/.java` | Preferences API | `emptyDir` |

### Dockerfile с учётом read-only ФС

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

### Вывод

Read-only файловая система -- простая, но мощная мера защиты. Она гарантирует иммутабельность контейнера: то, что было собрано на этапе build, нельзя изменить в runtime. Для Java-приложений необходимо явно монтировать writable-директории (`/tmp`, `/app/logs`) через `emptyDir` или `tmpfs`.

> **На собеседовании:** упомяните `readOnlyRootFilesystem: true` и объясните, что Java-приложению нужен writable `/tmp` (для JVM и Tomcat). Покажите знание флагов `noexec` и `nosuid` для tmpfs -- это демонстрирует глубокое понимание.

[к оглавлению](#Безопасность-контейнеров)

---

## Что такое Network Policies в Kubernetes и как они защищают приложение?
<!-- grade: middle -->

Network Policy -- это ресурс Kubernetes, определяющий правила сетевого взаимодействия между подами, работающий по принципу файрвола: можно ограничить как входящий (ingress), так и исходящий (egress) трафик.

По умолчанию в Kubernetes все поды могут свободно общаться друг с другом -- это опасно, так как скомпрометированный под может обратиться к любому сервису в кластере (lateral movement).

Аналогия: Network Policy -- как система пропусков в бизнес-центре. Без них любой может зайти на любой этаж. С ними -- только туда, куда выписан пропуск.

### Требования

Network Policies требуют CNI-плагин с поддержкой политик: **Calico**, **Cilium**, **Weave Net**. Стандартный **flannel** не поддерживает Network Policies -- политики будут создаваться без ошибок, но не будут применяться.

### Запрет всего трафика по умолчанию (deny all)

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

Это рекомендуемая отправная точка -- начинаем с полного запрета, затем открываем только необходимое (whitelist-подход).

### Разрешение входящего трафика от конкретного сервиса

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

### Разрешение исходящего трафика только к определённым сервисам

<details>
<summary>Пример egress-политики для payment-service</summary>

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
  # Разрешить DNS-запросы (обязательно!)
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

</details>

### Изоляция namespace-ов

<details>
<summary>Пример политики изоляции namespace</summary>

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

</details>

### Типичная архитектура сетевых политик

```
Internet -> Ingress Controller -> API Gateway -> [Banking Services] -> Database
                                       |                 |
                                  Auth Service         Kafka
                                       |
                                     Vault
```

Каждая стрелка -- отдельная Network Policy. Всё, что не разрешено явно, запрещено.

### Вывод

Network Policies -- обязательный элемент безопасности в Kubernetes. Подход deny-all + whitelist гарантирует, что каждый сервис может общаться только с теми компонентами, которые ему действительно нужны. Не забывайте разрешать DNS (порт 53), иначе под не сможет резолвить имена сервисов.

> **На собеседовании:** покажите знание подхода deny-all + whitelist, объясните разницу между ingress и egress. Обязательно упомяните необходимость CNI-плагина с поддержкой политик (Calico, Cilium) и частую ошибку -- забытый DNS в egress.

[к оглавлению](#Безопасность-контейнеров)

---

## Что такое Pod Security Standards и Pod Security Admission?
<!-- grade: senior -->

Pod Security Standards (PSS) -- это набор из трёх предопределённых уровней безопасности для подов в Kubernetes, а Pod Security Admission (PSA) -- встроенный admission controller, который применяет эти стандарты. PSS/PSA заменили устаревший PodSecurityPolicy, удалённый в Kubernetes 1.25.

### Три уровня Pod Security Standards

| Уровень | Описание | Когда использовать |
|---------|----------|-------------------|
| **Privileged** | Без ограничений | Системные компоненты (CNI, мониторинг, логирование) |
| **Baseline** | Минимальные ограничения, предотвращающие известные повышения привилегий | Некритичные приложения, staging |
| **Restricted** | Максимальные ограничения, следующие лучшим практикам | Банковские приложения, production |

### Что запрещает каждый уровень

**Baseline** запрещает:
- `hostNetwork`, `hostPID`, `hostIPC`
- Привилегированные контейнеры (`privileged: true`)
- Опасные capabilities (NET_RAW и др.)
- Монтирование `hostPath`
- Определённые типы volume

**Restricted** дополнительно запрещает:
- Запуск от root (`runAsNonRoot` обязателен)
- Privilege escalation (`allowPrivilegeEscalation: false` обязателен)
- Все capabilities, кроме `NET_BIND_SERVICE`
- Требует seccomp-профиль (`RuntimeDefault` или `Localhost`)
- Рекомендует read-only root filesystem

### Настройка через labels на namespace

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

| Режим | Поведение |
|-------|----------|
| **enforce** | Блокирует создание подов, нарушающих политику |
| **audit** | Разрешает, но записывает нарушение в audit log |
| **warn** | Разрешает, но показывает предупреждение пользователю |

### Пример пода, соответствующего уровню Restricted

<details>
<summary>Полный манифест compliant-пода</summary>

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

</details>

### Стратегия миграции

1. Начать с `warn` и `audit` на уровне **restricted** -- увидеть, какие поды не соответствуют.
2. Исправить манифесты подов, не прошедших проверку.
3. Включить `enforce: restricted`.

### Вывод

PSS/PSA -- встроенный механизм Kubernetes для обеспечения минимальных стандартов безопасности подов на уровне namespace. Для банковских систем рекомендуется уровень **restricted** во всех production namespace-ах. Для более гибких политик (например, проверка подписей образов, проверка реестров) используйте Kyverno или OPA Gatekeeper.

> **На собеседовании:** назовите три уровня (Privileged, Baseline, Restricted), объясните три режима применения (enforce, audit, warn). Упомяните, что PSA заменил PodSecurityPolicy с версии 1.25, и опишите стратегию постепенной миграции.

[к оглавлению](#Безопасность-контейнеров)

---

## Что такое Security Context в Kubernetes?
<!-- grade: middle -->

Security Context -- это набор параметров безопасности в манифесте Kubernetes, определяющих привилегии и контроль доступа для пода или отдельного контейнера. Это основной механизм настройки безопасности на уровне рабочей нагрузки.

Security Context задаётся на двух уровнях:
- **Pod-level** (`spec.securityContext`) -- применяется ко всем контейнерам в поде.
- **Container-level** (`spec.containers[].securityContext`) -- применяется к конкретному контейнеру и **переопределяет** pod-level настройки при конфликте.

### Полный пример Security Context

<details>
<summary>Манифест с полным Security Context на обоих уровнях</summary>

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
      procMount: Default
```

</details>

### Linux Capabilities

Capabilities дробят монолитные root-привилегии на отдельные единицы. Вместо полного root контейнер получает только необходимое:

```yaml
securityContext:
  capabilities:
    drop:
      - ALL           # Удалить ВСЕ capabilities
    add:
      - NET_BIND_SERVICE  # Привязка к портам < 1024 (если нужно)
```

| Capability | Описание | Нужна ли типичному Java-приложению? |
|-----------|----------|-------------------------------------|
| `NET_BIND_SERVICE` | Привязка к портам < 1024 | Редко (используйте порты > 1024) |
| `NET_RAW` | Raw sockets (ping) | Нет |
| `SYS_ADMIN` | Широкие admin-привилегии | Никогда |
| `SYS_PTRACE` | Трассировка процессов | Нет (только для отладки) |
| `CHOWN` | Изменение владельца файлов | Нет |

### Seccomp (Secure Computing)

Seccomp ограничивает набор системных вызовов (syscalls), доступных процессу в контейнере:

```yaml
securityContext:
  seccompProfile:
    type: RuntimeDefault  # Профиль container runtime (containerd/CRI-O)
    # Или кастомный профиль:
    # type: Localhost
    # localhostProfile: profiles/banking-app.json
```

<details>
<summary>Пример кастомного seccomp-профиля</summary>

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

</details>

### AppArmor

AppArmor ограничивает доступ контейнера к файлам и операциям на уровне LSM (Linux Security Module):

```yaml
metadata:
  annotations:
    container.apparmor.security.beta.kubernetes.io/banking-app: runtime/default
```

### SELinux

```yaml
securityContext:
  seLinuxOptions:
    level: "s0:c123,c456"
    type: "container_t"
```

### Рекомендуемый минимальный Security Context для production

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

### Вывод

Security Context -- центральный механизм безопасности в Kubernetes. Минимальный набор для production: `runAsNonRoot`, `allowPrivilegeEscalation: false`, `readOnlyRootFilesystem: true`, `capabilities.drop: ALL`, `seccompProfile: RuntimeDefault`. Дополнительные уровни защиты (AppArmor, SELinux, кастомный seccomp) применяются для высокочувствительных сервисов.

> **На собеседовании:** перечислите ключевые параметры Security Context, объясните разницу между pod-level и container-level. Упомяните capabilities (drop ALL -- золотое правило) и seccomp. Если знаете AppArmor/SELinux -- это бонус для senior-уровня.

[к оглавлению](#Безопасность-контейнеров)
## Как работает RBAC в Kubernetes?
<!-- grade: -->

**RBAC (Role-Based Access Control)** — механизм авторизации в Kubernetes, который определяет, какой субъект (пользователь, группа, сервисный аккаунт) может выполнять какие действия (глаголы) над какими ресурсами. RBAC включён по умолчанию и является основным инструментом управления доступом в кластере.

**Основные объекты RBAC:**

| Объект | Область действия | Описание |
|--------|-----------------|----------|
| **Role** | Namespace | Набор разрешений в пределах одного namespace |
| **ClusterRole** | Кластер | Набор разрешений на уровне всего кластера |
| **RoleBinding** | Namespace | Привязывает Role или ClusterRole к субъекту в рамках namespace |
| **ClusterRoleBinding** | Кластер | Привязывает ClusterRole к субъекту на уровне всего кластера |

**Субъекты RBAC:**

- **User** — обычный пользователь, управляемый внешней системой аутентификации (LDAP, OIDC и т.д.). Kubernetes не хранит учётные записи пользователей самостоятельно.
- **Group** — группа пользователей для упрощения управления правами.
- **ServiceAccount** — учётная запись для подов и внутренних сервисов кластера. В отличие от User, ServiceAccount — это полноценный ресурс Kubernetes.

**1. Создание ServiceAccount для Java-приложения:**

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: banking-service-sa
  namespace: banking
automountServiceAccountToken: false  # Не монтировать токен по умолчанию
```

Параметр `automountServiceAccountToken: false` означает, что токен сервисного аккаунта не будет автоматически смонтирован в файловую систему пода. Это важно, потому что смонтированный токен может быть использован злоумышленником для доступа к Kubernetes API при компрометации контейнера.

**2. Role с минимальными правами (принцип наименьших привилегий):**

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
  resourceNames: ["banking-service-config"]  # Доступ только к конкретному ConfigMap

# Чтение секретов (только конкретных)
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get"]
  resourceNames: ["banking-db-credentials"]
```

Поле `resourceNames` ограничивает доступ конкретными именованными ресурсами — без него Role даёт доступ ко **всем** ресурсам указанного типа в namespace, что нарушает принцип наименьших привилегий.

**3. RoleBinding — привязка роли к субъекту:**

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

RoleBinding связывает субъект (ServiceAccount) с набором разрешений (Role). После создания этого ресурса сервисный аккаунт `banking-service-sa` получит только те права, которые определены в `banking-service-role`.

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
      automountServiceAccountToken: true  # Включить только если приложению нужен доступ к K8s API
      containers:
      - name: app
        image: registry.bank.local/banking-service:1.0.0
```

Значение `automountServiceAccountToken: true` на уровне Pod spec переопределяет настройку ServiceAccount. Устанавливайте его в `true` только в том случае, если приложению действительно необходим программный доступ к Kubernetes API (например, для service discovery или работы с ConfigMap).

**5. ClusterRole для кросс-namespace доступа (пример для мониторинга):**

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

ClusterRole отличается от Role тем, что работает на уровне всего кластера. Это необходимо, например, для систем мониторинга (Prometheus), которые должны собирать метрики из подов во всех namespace-ах.

**Проверка прав (диагностика):**

```bash
# Проверить, может ли ServiceAccount получить секреты
kubectl auth can-i get secrets \
    --as=system:serviceaccount:banking:banking-service-sa \
    -n banking

# Вывести все права ServiceAccount в данном namespace
kubectl auth can-i --list \
    --as=system:serviceaccount:banking:banking-service-sa \
    -n banking
```

Команда `kubectl auth can-i` позволяет проверить, имеет ли конкретный субъект право на выполнение определённого действия. Это полезно для аудита и отладки проблем с доступом.

**Типичные ошибки при настройке RBAC:**

| Ошибка | Почему это опасно | Как правильно |
|--------|-------------------|---------------|
| Привязка `cluster-admin` к сервисному аккаунту приложения | Даёт полные права на всё в кластере — любая компрометация контейнера ведёт к компрометации всего кластера | Создавать отдельную Role с минимально необходимыми правами |
| Разрешение `*` (wildcard) на все ресурсы и глаголы | Фактически эквивалент admin-доступа | Указывать конкретные ресурсы и глаголы |
| Использование default ServiceAccount | Имеет автоматически монтируемый токен и может иметь унаследованные права | Создавать именованный ServiceAccount для каждого сервиса |
| Отсутствие `resourceNames` в Role | Даёт доступ ко **всем** ресурсам данного типа, а не к конкретным | Указывать `resourceNames` везде, где это возможно |

**Рекомендации для банковских систем:**

- **Один микросервис — один ServiceAccount** с минимально необходимыми правами. Это обеспечивает изоляцию: компрометация одного сервиса не даёт доступа к ресурсам другого.
- **Регулярный аудит RBAC** с помощью специализированных инструментов: `kubectl-who-can` (кто имеет доступ к ресурсу), `rbac-lookup` (какие права у субъекта), `rakkess` (обзор прав в namespace).
- **Интеграция с корпоративным IdP** (LDAP/Active Directory) через OIDC для аутентификации пользователей-администраторов.
- **Запрет `automountServiceAccountToken`** по умолчанию — включать только для сервисов, которым действительно нужен доступ к Kubernetes API.

[к оглавлению](#Безопасность-контейнеров)

## Как ограничение ресурсов защищает от DoS-атак?
<!-- grade: -->

Ограничение ресурсов (resource limits) в Kubernetes и Docker предотвращает ситуацию, когда один контейнер потребляет все ресурсы хоста, лишая остальные контейнеры возможности нормально функционировать. Это защита от **DoS (Denial of Service)** — как от целенаправленных атак, так и от ошибок в коде (утечки памяти, бесконечные циклы, неконтролируемый рост числа потоков).

**Requests и Limits в Kubernetes:**

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
            memory: "512Mi"  # Максимум — при превышении контейнер получит OOMKill
            cpu: "1000m"     # 1 CPU core — при превышении контейнер будет троттлиться
        env:
        - name: JAVA_OPTS
          value: >-
            -XX:MaxRAMPercentage=75.0
            -XX:+UseG1GC
            -XX:+ExitOnOutOfMemoryError
```

**Разница между requests и limits:**

- **requests** — минимальный гарантированный объём ресурсов. Планировщик Kubernetes использует requests при выборе ноды для размещения пода: под будет размещён только на той ноде, где имеется достаточно свободных ресурсов для покрытия requests всех контейнеров.
- **limits** — максимально допустимое потребление. При превышении memory limit ядро Linux немедленно завершает процесс через OOMKill. При превышении CPU limit контейнер подвергается троттлингу (throttling) — ему выделяется меньше процессорного времени, но процесс продолжает работать.

**Настройка JVM с учётом container limits:**

```dockerfile
ENTRYPOINT ["java", \
    "-XX:MaxRAMPercentage=75.0", \
    "-XX:InitialRAMPercentage=50.0", \
    "-XX:+UseContainerSupport", \
    "-XX:+ExitOnOutOfMemoryError", \
    "-jar", "/app/app.jar"]
```

Флаг `-XX:+UseContainerSupport` (включён по умолчанию с Java 10+) позволяет JVM корректно определять memory и CPU limits контейнера через cgroups и соответствующим образом настраивать размер heap и количество потоков GC. Параметр `-XX:MaxRAMPercentage=75.0` выделяет 75% от доступной памяти контейнера под Java heap, оставляя 25% для metaspace, стеков потоков, native memory (NIO-буферы, JNI) и самой операционной системы.

Флаг `-XX:+ExitOnOutOfMemoryError` приказывает JVM немедленно завершить процесс при OutOfMemoryError, чтобы Kubernetes мог перезапустить под через restartPolicy. Без этого флага JVM может оказаться в «зомби»-состоянии: процесс жив, но не обрабатывает запросы.

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
    default:          # Значения limits по умолчанию (если не указаны в Pod spec)
      memory: "512Mi"
      cpu: "500m"
    defaultRequest:   # Значения requests по умолчанию
      memory: "256Mi"
      cpu: "250m"
    max:              # Максимально допустимые limits
      memory: "2Gi"
      cpu: "2"
    min:              # Минимально допустимые requests
      memory: "64Mi"
      cpu: "50m"
  - type: Pod
    max:
      memory: "4Gi"
      cpu: "4"
```

LimitRange выполняет две функции: автоматически назначает limits/requests контейнерам, для которых они не указаны явно, и валидирует, что указанные значения попадают в допустимый диапазон. Это предотвращает ситуацию, когда разработчик забывает указать limits или устанавливает неоправданно высокие значения.

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

ResourceQuota ограничивает **суммарное** потребление ресурсов всеми подами в namespace. Это защита от ситуации, когда одна команда создаёт слишком много подов или потребляет непропорционально много ресурсов кластера. Поле `pods: "50"` также ограничивает количество подов, что защищает от атаки через массовое создание ресурсов.

**Ограничение ресурсов в Docker:**

```bash
docker run \
    --memory=512m \
    --memory-swap=512m \    # Запретить использование swap
    --cpus=1.0 \
    --pids-limit=200 \      # Ограничение числа процессов (защита от fork bomb)
    --ulimit nofile=1024:2048 \
    my-java-app
```

Параметр `--memory-swap=512m`, равный `--memory`, полностью запрещает использование swap. Swap может скрывать проблемы с потреблением памяти и ухудшать производительность, поэтому в production-среде его рекомендуется отключать. Параметр `--pids-limit=200` ограничивает количество процессов внутри контейнера, что защищает от fork bomb (атака, при которой процесс рекурсивно создаёт копии самого себя, исчерпывая PID-ы системы).

**Сценарии атак, которые предотвращают limits:**

| Атака | Без ограничений | С ограничениями |
|-------|----------------|-----------------|
| Утечка памяти (memory leak) | Падение всего хоста из-за исчерпания RAM | OOMKill только конкретного пода, остальные продолжают работать |
| Бесконечный цикл (100% CPU) | Деградация производительности всех сервисов на ноде | Throttling только этого пода, другие получают своё процессорное время |
| Fork bomb | Исчерпание PID-ов хоста, невозможность запустить новые процессы | `pids-limit` ограничит число процессов внутри контейнера |
| Заполнение диска логами | Диск заполнен, все сервисы на ноде перестают писать логи и данные | `emptyDir.sizeLimit` + `ephemeral-storage` limits ограничат потребление дискового пространства |

[к оглавлению](#Безопасность-контейнеров)

## Что такое Supply Chain Security в контексте контейнеров?
<!-- grade: -->

**Supply Chain Security (безопасность цепочки поставок)** — обеспечение целостности и безопасности всех компонентов, участвующих в создании и доставке программного обеспечения: от исходного кода и зависимостей до финального контейнерного образа, развёрнутого в production.

Атаки на цепочку поставок (SolarWinds, Log4Shell, codecov) продемонстрировали, что компрометация одного звена цепочки может затронуть тысячи организаций-потребителей. В контексте контейнеров цепочка поставок включает: базовый образ, пакеты операционной системы, зависимости приложения (Maven/Gradle), инструменты сборки, CI/CD-платформу и реестр образов.

**Основные компоненты Supply Chain Security:**

**1. SBOM (Software Bill of Materials) — перечень всех компонентов:**

SBOM — это машинно-читаемый документ, содержащий полный список компонентов программного обеспечения: пакеты ОС, Java-зависимости (Maven/Gradle), их точные версии, лицензии и криптографические хеши. Два основных формата: CycloneDX и SPDX.

```bash
# Генерация SBOM с помощью Trivy (формат CycloneDX)
trivy image --format cyclonedx \
    --output sbom.cdx.json \
    registry.bank.local/banking-service:1.0.0

# Генерация SBOM с помощью Syft (Anchore)
syft registry.bank.local/banking-service:1.0.0 -o spdx-json > sbom.spdx.json

# Сканирование SBOM на уязвимости (отдельно от генерации)
grype sbom:./sbom.cdx.json
```

SBOM позволяет быстро определить, содержит ли ваш образ уязвимый компонент. Например, при обнаружении новой CVE в Log4j можно за секунды проверить все SBOM и определить, какие именно сервисы затронуты, вместо того чтобы пересканировать каждый образ.

**2. Проверка зависимостей на уязвимости:**

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
# Запуск проверки зависимостей
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

Параметр `failBuildOnCVSS=7` означает, что сборка будет автоматически провалена при обнаружении зависимости с уязвимостью, имеющей CVSS-балл 7.0 и выше (High и Critical). Файл `suppressionFile` позволяет задокументировать исключения для ложных срабатываний или принятых рисков.

**3. Фиксация версий зависимостей:**

```xml
<!-- Использовать конкретные версии, НЕ диапазоны -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>3.3.0</version> <!-- Конкретная версия -->
</dependency>

<!-- ЗАПРЕЩЕНО в банковских проектах: -->
<version>[3.0,4.0)</version> <!-- Диапазон версий — может подтянуть непроверенное обновление -->
<version>LATEST</version>    <!-- Последняя версия — непредсказуемый результат сборки -->
```

Диапазоны версий и метатеги (`LATEST`, `RELEASE`) создают **нереспроизводимые сборки**: один и тот же коммит может дать разный результат в зависимости от момента сборки. Это как минимум нарушает аудируемость, а в худшем случае позволяет злоумышленнику внедрить вредоносный пакет через подмену (dependency confusion attack).

**4. Верификация артефактов в Docker:**

```dockerfile
FROM eclipse-temurin:21-jre-alpine@sha256:a1b2c3d4...  # Pin по digest

# Проверка контрольной суммы скачиваемых файлов
RUN wget https://example.com/tool.tar.gz && \
    echo "expected_sha256  tool.tar.gz" | sha256sum -c - && \
    tar xzf tool.tar.gz
```

Pin по digest (`@sha256:...`) гарантирует, что будет использован ровно тот образ, который был протестирован. В отличие от тега, digest нельзя переназначить на другой образ. Проверка контрольной суммы скачиваемых файлов предотвращает использование подменённых артефактов.

**5. SLSA (Supply-chain Levels for Software Artifacts):**

SLSA (произносится «salsa») — это фреймворк, определяющий уровни зрелости безопасности цепочки поставок:

| Уровень | Требования | Что это даёт |
|---------|-----------|--------------|
| SLSA 1 | Процесс сборки документирован | Базовая прозрачность |
| SLSA 2 | Сборка выполняется на CI/CD (не локально), подписанная provenance | Защита от ручного вмешательства |
| SLSA 3 | CI/CD-система аудитируемая и защищённая от вмешательства | Защита от компрометации pipeline |
| SLSA 4 | Двусторонняя проверка изменений, hermetic build (изолированная сборка без доступа к сети) | Полная верифицируемость |

Для банковских систем рекомендуется стремиться к SLSA 3 как минимум.

**6. Provenance (происхождение артефакта):**

Provenance — это подписанный документ (attestation), фиксирующий, кто собрал образ, из какого исходного кода, на какой CI/CD-системе и с какими параметрами. Это позволяет проследить полную цепочку от коммита до деплоя.

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

**7. Полный пайплайн supply chain security (GitHub Actions):**

```yaml
name: Secure Build Pipeline
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write  # Для keyless signing через Fulcio/Sigstore
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

Этот пайплайн последовательно выполняет все этапы supply chain security: проверку зависимостей, сборку, генерацию SBOM, сканирование на уязвимости, подпись образа и прикрепление SBOM к образу в реестре. Каждый этап — это «ворота безопасности» (security gate), при провале которого пайплайн останавливается.

[к оглавлению](#Безопасность-контейнеров)

## Что такое Docker Bench for Security?
<!-- grade: -->

**Docker Bench for Security** — автоматизированный инструмент аудита безопасности, проверяющий конфигурацию Docker-хоста и контейнеров на соответствие рекомендациям **CIS Docker Benchmark** (Center for Internet Security). CIS Benchmark — это общепризнанный стандарт, используемый в том числе при аудитах PCI DSS.

**Запуск Docker Bench for Security:**

```bash
docker run --rm --net host --pid host --userns host --cap-add audit_control \
    -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST \
    -v /var/lib:/var/lib:ro \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    -v /usr/lib/systemd:/usr/lib/systemd:ro \
    -v /etc:/etc:ro \
    docker/docker-bench-security
```

Инструмент запускается как контейнер с расширенными правами, потому что ему необходим доступ к конфигурации хоста, Docker daemon и работающим контейнерам для их анализа. Все тома монтируются в режиме read-only (`:ro`).

**Категории проверок (секции CIS Docker Benchmark):**

| Секция | Описание | Примеры проверок |
|--------|----------|-----------------|
| 1 — Host Configuration | Конфигурация хоста | Аудит-логирование, отдельный раздел для `/var/lib/docker`, обновление ядра |
| 2 — Docker daemon configuration | Настройки Docker daemon | TLS для удалённого доступа, user namespace remapping, live-restore |
| 3 — Docker daemon configuration files | Права доступа к файлам | Права на `docker.sock`, `daemon.json`, TLS-сертификаты |
| 4 — Container Images and Build Files | Образы и Dockerfile | Наличие `USER` в Dockerfile, `HEALTHCHECK`, отсутствие секретов в слоях |
| 5 — Container Runtime | Параметры запуска контейнеров | Запрет privileged, запрет host network, read-only FS, ограничение capabilities |
| 6 — Docker Security Operations | Операционная безопасность | Регулярное обновление Docker Engine, мониторинг, бэкапы |
| 7 — Docker Swarm | Безопасность Swarm | Шифрование overlay-сети, ротация сертификатов, autolock |

**Пример вывода Docker Bench:**

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

Результаты бывают трёх типов: **PASS** (проверка пройдена), **WARN** (предупреждение — требует внимания), **FAIL** (грубое нарушение). Для банковских систем цель — устранить все FAIL и минимизировать WARN. Каждый WARN должен быть либо устранён, либо задокументирован как принятый риск (с обоснованием).

**Автоматизация Docker Bench в CI/CD (Jenkinsfile):**

```groovy
stage('Docker Bench Audit') {
    steps {
        sh '''
            docker run --rm \
                -v /var/run/docker.sock:/var/run/docker.sock:ro \
                -v /etc:/etc:ro \
                docker/docker-bench-security \
                -l /dev/stdout 2>&1 | tee bench-results.txt

            # Проверить наличие FAIL — если есть, прервать пайплайн
            if grep -q "\\[FAIL\\]" bench-results.txt; then
                echo "Docker Bench found FAIL results!"
                exit 1
            fi
        '''
    }
}
```

**kube-bench — аналог для Kubernetes:**

kube-bench выполняет проверку кластера Kubernetes на соответствие **CIS Kubernetes Benchmark**. Проверяет конфигурацию API-сервера, etcd, kubelet, controller-manager и scheduler.

```bash
# Запуск kube-bench как Job в кластере
kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job.yaml
kubectl logs job/kube-bench
```

kube-bench проверяет, в частности: включено ли шифрование etcd, правильно ли настроен RBAC, используется ли TLS для всех компонентов control plane, включён ли audit logging.

[к оглавлению](#Безопасность-контейнеров)

## Как защитить Docker daemon?
<!-- grade: -->

Docker daemon (`dockerd`) работает с правами root и управляет всеми контейнерами на хосте. Компрометация Docker daemon эквивалентна полному контролю над хостом: злоумышленник может запускать произвольные контейнеры, читать данные из любых томов и выполнять команды на хосте. Поэтому защита daemon — одна из приоритетных задач безопасности.

**1. Защита Docker socket:**

По умолчанию Docker daemon слушает на Unix-сокете `/var/run/docker.sock`. Доступ к этому сокету равнозначен root-доступу к хосту, потому что через него можно запустить привилегированный контейнер с монтированием корневой файловой системы хоста.

```bash
# Проверить права на сокет
ls -la /var/run/docker.sock
# srw-rw---- 1 root docker 0 ... /var/run/docker.sock

# Только пользователи группы docker должны иметь доступ
```

**Категорически запрещено** монтировать Docker socket в production-контейнеры:

```yaml
# ОПАСНО! Даёт контейнеру полный контроль над Docker daemon
volumes:
  - /var/run/docker.sock:/var/run/docker.sock
```

Если монтирование сокета необходимо (например, для CI runner), используйте read-only монтирование и авторизационные плагины для ограничения доступных операций.

**2. Включение TLS для удалённого доступа:**

Если Docker daemon должен быть доступен по сети (например, для удалённого управления), обязательна настройка mutual TLS (mTLS) — проверка и серверного, и клиентского сертификатов.

```bash
# Генерация CA (Certificate Authority)
openssl genrsa -aes256 -out ca-key.pem 4096
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem

# Генерация серверного ключа и сертификата
openssl genrsa -out server-key.pem 4096
openssl req -subj "/CN=docker-host.bank.local" -sha256 \
    -new -key server-key.pem -out server.csr

echo subjectAltName = DNS:docker-host.bank.local,IP:10.0.1.100 >> extfile.cnf
echo extendedKeyUsage = serverAuth >> extfile.cnf
openssl x509 -req -days 365 -sha256 \
    -in server.csr -CA ca.pem -CAkey ca-key.pem \
    -CAcreateserial -out server-cert.pem -extfile extfile.cnf

# Генерация клиентского ключа и сертификата
openssl genrsa -out client-key.pem 4096
openssl req -subj '/CN=client' -new -key client-key.pem -out client.csr
echo extendedKeyUsage = clientAuth > extfile-client.cnf
openssl x509 -req -days 365 -sha256 \
    -in client.csr -CA ca.pem -CAkey ca-key.pem \
    -CAcreateserial -out client-cert.pem -extfile extfile-client.cnf
```

**3. Безопасная конфигурация daemon.json:**

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

Назначение ключевых параметров:

| Параметр | Значение | Описание |
|----------|----------|----------|
| `tlsverify: true` | Обязательная mTLS | Клиент должен предоставить сертификат, подписанный доверенным CA. Без этого параметра любой может подключиться к daemon |
| `icc: false` | Запрет Inter-Container Communication | Контейнеры в одной bridge-сети не смогут общаться друг с другом напрямую — только через явно опубликованные порты (--link или docker network) |
| `userns-remap: default` | Маппинг user namespace | UID 0 (root) внутри контейнера маппится на непривилегированный UID на хосте (например, 100000). Даже при побеге из контейнера атакующий не получит root на хосте |
| `no-new-privileges: true` | Запрет повышения привилегий | Блокирует setuid/setgid-бинарники и другие механизмы повышения привилегий |
| `live-restore: true` | Сохранение контейнеров при перезапуске daemon | Контейнеры продолжают работать, даже если Docker daemon перезапускается (важно для обновлений без простоя) |
| `userland-proxy: false` | Отключение userland-proxy | Использование iptables вместо docker-proxy для маршрутизации портов — эффективнее и безопаснее |

**4. Docker Authorization Plugin:**

```json
{
  "authorization-plugins": ["casbin-authz-plugin"]
}
```

Authorization plugin — это webhook, который перехватывает каждый запрос к Docker daemon и применяет политики авторизации. Позволяет определять гранулярные правила: кто может запускать контейнеры, с какими параметрами (например, запретить `--privileged` или монтирование `/` хоста). Это особенно важно в средах с несколькими пользователями Docker.

**5. Rootless Docker:**

```bash
# Установка rootless Docker
dockerd-rootless-setuptool.sh install

# Docker daemon работает от обычного пользователя
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```

Rootless Docker полностью исключает работу daemon от root — и сам daemon, и все контейнеры работают в user namespace непривилегированного пользователя. Это радикально снижает риск при компрометации daemon, но имеет ограничения: нельзя использовать некоторые storage drivers (overlay2 требует fuse-overlayfs), нет привязки к портам ниже 1024 без дополнительных настроек, невозможно использовать cgroup v1.

**6. Ограничение логов (защита от заполнения диска):**

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

Без ограничений логи контейнера могут заполнить весь диск хоста, что приведёт к отказу в обслуживании всех контейнеров. Параметр `max-size` ограничивает размер одного файла лога, а `max-file` — количество ротируемых файлов. В данном примере максимальный объём логов на контейнер — 30 MB (3 файла по 10 MB).

[к оглавлению](#Безопасность-контейнеров)

## Как работает изоляция контейнеров на уровне ядра Linux?
<!-- grade: -->

Контейнеры — это **не виртуальные машины**. Они не содержат собственного ядра операционной системы. Все контейнеры на хосте разделяют одно ядро Linux, а изоляция обеспечивается тремя основными механизмами ядра: **namespaces** (изоляция видимости), **cgroups** (ограничение ресурсов) и **union filesystems** (послойная файловая система). Понимание этих механизмов необходимо для оценки реальной степени изоляции и осознанного выбора дополнительных мер защиты.

**1. Linux Namespaces (изоляция видимости ресурсов):**

Namespaces создают для процесса внутри контейнера иллюзию собственной изолированной операционной системы. Каждый namespace изолирует определённый тип системных ресурсов:

| Namespace | Что изолирует | Флаг clone() | Описание |
|-----------|--------------|--------------|----------|
| **PID** | Процессы | CLONE_NEWPID | Контейнер видит только свои процессы. Процесс entrypoint получает PID 1 внутри контейнера, хотя на хосте у него другой PID |
| **NET** | Сеть | CLONE_NEWNET | Собственный сетевой стек: IP-адрес, интерфейсы, таблица маршрутизации, порты |
| **MNT** | Файловая система | CLONE_NEWNS | Собственные точки монтирования, изолированные от хоста |
| **UTS** | Hostname | CLONE_NEWUTS | Собственное имя хоста (hostname), независимое от реального хоста |
| **IPC** | Межпроцессное взаимодействие | CLONE_NEWIPC | Собственные семафоры, очереди сообщений, разделяемая память |
| **USER** | Пользователи | CLONE_NEWUSER | Собственная таблица UID/GID. Root (UID 0) внутри контейнера может маппиться на непривилегированного пользователя на хосте |
| **Cgroup** | Cgroup-иерархия | CLONE_NEWCGROUP | Контейнер видит собственную иерархию cgroups, а не всю иерархию хоста |

```bash
# Посмотреть namespaces контейнера по PID процесса
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

Каждый namespace идентифицируется числовым inode (числа в квадратных скобках). Контейнеры с одинаковыми inode делят один namespace. Например, если USER namespace совпадает с хостовым — значит, user namespace remapping не включён.

**2. Cgroups (Control Groups — ограничение ресурсов):**

Если namespaces отвечают за то, что контейнер **видит**, то cgroups отвечают за то, сколько ресурсов он может **потребить**:

| Подсистема | Что ограничивает |
|------------|-----------------|
| **cpu** | Процессорное время (CPU shares, quotas) |
| **memory** | Оперативная память (limits, swap) |
| **blkio** | Disk I/O (пропускная способность, IOPS) |
| **pids** | Количество процессов (защита от fork bomb) |
| **cpuset** | Привязка к конкретным ядрам CPU (CPU pinning) |
| **devices** | Доступ к устройствам хоста (/dev/*) |

```bash
# Просмотр cgroup-лимитов контейнера
cat /sys/fs/cgroup/memory/docker/<container_id>/memory.limit_in_bytes
cat /sys/fs/cgroup/cpu/docker/<container_id>/cpu.cfs_quota_us
cat /sys/fs/cgroup/pids/docker/<container_id>/pids.max
```

Для Java особенно важно: начиная с Java 10, JVM по умолчанию включает `-XX:+UseContainerSupport` и корректно читает cgroup-лимиты для автоматической настройки размера heap и количества потоков GC. Без этой поддержки JVM может попытаться использовать всю RAM хоста, что приведёт к OOMKill.

**3. Seccomp (Secure Computing — фильтрация системных вызовов):**

Seccomp позволяет фильтровать системные вызовы (syscalls), доступные процессу. Docker по умолчанию применяет профиль, блокирующий примерно 44 системных вызова из более чем 300, включая: `reboot` (перезагрузка хоста), `mount` (монтирование файловых систем), `swapon` (управление swap), `clock_settime` (изменение системного времени), `bpf` (загрузка BPF-программ) и другие потенциально опасные вызовы.

**4. Linux Capabilities:**

Традиционная модель привилегий Linux бинарна: процесс либо root (всемогущий), либо обычный пользователь. Capabilities дробят привилегии root на примерно 40 отдельных единиц, позволяя дать процессу только необходимые:

```bash
# Capabilities контейнера по умолчанию
docker run --rm alpine cat /proc/1/status | grep Cap
# CapPrm: 00000000a80425fb
# Включает: CHOWN, DAC_OVERRIDE, FSETID, FOWNER, NET_RAW и др.
```

Docker по умолчанию оставляет минимальный набор capabilities, но для максимальной безопасности рекомендуется `--cap-drop=ALL` с добавлением только действительно нужных.

**5. Ограничения изоляции контейнеров:**

Поскольку контейнеры разделяют ядро Linux с хостом, существуют принципиальные ограничения изоляции:

- **Уязвимость ядра = потенциальный побег из контейнера** (container escape). Эксплоит уязвимости ядра может дать злоумышленнику доступ к хостовой системе.
- **`/proc` и `/sys` частично доступны** — некоторые файлы в этих виртуальных файловых системах содержат информацию о хосте.
- **Флаг `--privileged` полностью отключает изоляцию** — контейнер получает доступ ко всем устройствам хоста и capabilities root.
- **Время (clock) является общим** для всех контейнеров и хоста — нет способа изолировать системные часы.

**6. Усиление изоляции (для высокочувствительных систем):**

Когда стандартной контейнерной изоляции недостаточно, существуют технологии, добавляющие дополнительный уровень:

| Технология | Описание | Накладные расходы |
|-----------|----------|-------------------|
| **gVisor** (Google) | Виртуальное ядро в user-space, перехватывающее все syscalls контейнера | Средние (замедление syscall-heavy нагрузок) |
| **Kata Containers** | Лёгкие виртуальные машины с собственным ядром, OCI-совместимые | Низкие (быстрый старт, но больше RAM) |
| **Firecracker** (AWS) | microVM с минимальным VMM, используется в AWS Lambda и Fargate | Минимальные (старт за ~125 мс) |

```yaml
# Использование gVisor как runtime class в Kubernetes
apiVersion: v1
kind: Pod
spec:
  runtimeClassName: gvisor  # Указать runtime class
  containers:
  - name: banking-app
    image: registry.bank.local/banking-service:1.0.0
```

gVisor перехватывает все системные вызовы контейнера и выполняет их в собственном ядре (Sentry), работающем в user-space. Даже при наличии уязвимости в «ядре» gVisor атакующий не получит доступ к реальному ядру хоста. Для банковских систем с обработкой платёжных данных стоит рассмотреть Kata Containers или gVisor как дополнительный уровень изоляции.

[к оглавлению](#Безопасность-контейнеров)

## Что такое Runtime Security и как работает Falco?
<!-- grade: -->

**Runtime Security** — обнаружение и реагирование на аномальное поведение контейнеров в реальном времени, то есть непосредственно во время их работы. В отличие от сканирования образов (подход shift-left, который выявляет уязвимости до деплоя), runtime security работает на этапе выполнения и обнаруживает атаки, которые невозможно выявить статическим анализом: запуск неожиданных процессов, доступ к конфиденциальным файлам, аномальные сетевые подключения.

**Falco (CNCF graduated project)** — ведущий open-source инструмент runtime security для контейнеров и Kubernetes, разработанный компанией Sysdig.

**Принцип работы Falco:**

1. **Перехват системных вызовов** — Falco подключается к ядру Linux через eBPF (рекомендуемый способ) или загружаемый kernel module. Каждый системный вызов (open, execve, connect, write и др.) из каждого контейнера проходит через Falco.
2. **Анализ по правилам** — каждый перехваченный вызов сопоставляется с набором правил, написанных на языке условий Falco. Правила описывают нормальное и аномальное поведение.
3. **Генерация оповещений** — при совпадении с правилом Falco создаёт событие с указанием severity, деталей процесса, контейнера и namespace.

**Установка Falco в Kubernetes через Helm:**

```bash
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
helm install falco falcosecurity/falco \
    --namespace falco \
    --create-namespace \
    --set driver.kind=ebpf \
    --set falcosidekick.enabled=true \
    --set falcosidekick.config.slack.webhookurl="https://hooks.slack.com/..."
```

Параметр `driver.kind=ebpf` указывает использовать eBPF вместо kernel module. eBPF безопаснее и не требует установки дополнительных модулей ядра, но требует ядро Linux 5.8+.

**Примеры правил Falco:**

```yaml
# Обнаружение запуска shell в контейнере
# (в production-контейнере не должно запускаться интерактивных оболочек)
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

# Обнаружение записи в директорию /etc
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

Каждое правило содержит: `condition` (условие срабатывания на языке фильтров Falco), `output` (шаблон сообщения с макросами для подстановки контекста), `priority` (уровень критичности от DEBUG до CRITICAL) и `tags` (метки для группировки и фильтрации).

**Falcosidekick — маршрутизация оповещений:**

Falcosidekick принимает оповещения от Falco и маршрутизирует их в различные целевые системы: мессенджеры, SIEM, системы мониторинга, хранилища логов:

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
    # Автоматическая реакция: создание Policy Report при инцидентах
    kubernetesPolicyReport:
      enabled: true
```

Falcosidekick поддерживает более 50 целевых систем, включая Slack, PagerDuty, Elasticsearch, Kafka, AWS SNS, Prometheus и другие.

**Типичные инциденты, обнаруживаемые Falco в банковских системах:**

| Инцидент | Severity | Рекомендуемое действие |
|----------|----------|----------------------|
| Запуск shell (`/bin/sh`, `/bin/bash`) в production-контейнере | WARNING | Уведомление в SOC (Security Operations Center) |
| Чтение `/etc/shadow` или других чувствительных файлов | ERROR | Уведомление + немедленное расследование инцидента |
| Запуск процесса, не являющегося entrypoint | NOTICE | Логирование и анализ |
| Исходящее подключение к неизвестному IP/порту | WARNING | Уведомление + проверка на эксфильтрацию данных |
| Модификация бинарных файлов приложения | CRITICAL | Уведомление + автоматическое удаление пода |
| Обнаружение криптомайнера | CRITICAL | Немедленная эскалация, удаление пода, расследование вектора компрометации |

**Альтернативы Falco:** Sysdig Secure (коммерческая версия от тех же разработчиков), Aqua Security, Prisma Cloud (Palo Alto Networks).

[к оглавлению](#Безопасность-контейнеров)

## Как организовать процесс обновления и патчинга базовых образов?
<!-- grade: -->

Уязвимости обнаруживаются постоянно — образ, безопасный сегодня, завтра может содержать критическую CVE. Регулярное обновление базовых образов и зависимостей является обязательным процессом для банковских систем и требованием стандартов безопасности (PCI DSS Requirement 6).

**1. Стратегия фиксации (pin) версий:**

```dockerfile
# ПЛОХО — неизвестно, что получим при следующей сборке
FROM eclipse-temurin:21-jre

# ЛУЧШЕ — фиксация конкретной минорной версии
FROM eclipse-temurin:21.0.4_7-jre-alpine

# ЛУЧШЕЕ — pin по digest (неизменная ссылка на конкретный образ)
FROM eclipse-temurin@sha256:1a2b3c4d5e6f...
```

Тег (например, `21-jre`) может указывать на разные образы в разное время — разработчики базового образа обновляют теги при выпуске патчей. Digest (`@sha256:...`) — это криптографический хеш, однозначно идентифицирующий конкретную версию образа. Pin по digest гарантирует воспроизводимость сборки, но требует дисциплины при обновлениях.

**2. Автоматическое обнаружение обновлений с Renovate/Dependabot:**

Renovate и Dependabot — инструменты, автоматически создающие Pull Request при появлении новых версий зависимостей. Они поддерживают Dockerfile, docker-compose, Maven, Gradle и другие экосистемы.

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

Параметр `pinDigests: true` автоматически добавляет digest к тегам при создании PR. Параметр `automerge: false` означает, что PR с обновлением базового образа требует ручного review — для банковских систем автоматический merge обновлений обычно запрещён политиками безопасности.

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

Новые CVE появляются ежедневно. Образ, не имевший известных уязвимостей при деплое, может стать уязвимым через неделю. Поэтому необходимо регулярно пересканировать уже развёрнутые образы:

```yaml
# CronJob для ежедневного сканирования всех образов в кластере
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
              # Получить список уникальных образов во всех namespace-ах
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
   +-- Renovate/Dependabot создаёт PR с обновлением зависимости
   +-- Trivy обнаруживает CVE при ежедневном пересканировании
   +-- Подписка на security advisories (GitHub Advisory, Alpine SecDB, Spring Security)

2. Оценка (определение приоритета):
   +-- CVSS >= 9.0 (Critical)  -> патч в течение 24 часов
   +-- CVSS 7.0-8.9 (High)    -> патч в течение 7 дней
   +-- CVSS 4.0-6.9 (Medium)  -> патч в следующем плановом релизе
   +-- CVSS < 4.0 (Low)       -> в бэклог

3. Патчинг:
   +-- Обновить базовый образ в Dockerfile
   +-- Обновить зависимости (Maven/Gradle)
   +-- Прогнать полный набор тестов (unit, integration, e2e)
   +-- Сканировать новый образ на уязвимости
   +-- Деплой через стандартный CI/CD пайплайн

4. Верификация:
   +-- Повторное сканирование после деплоя (подтверждение устранения CVE)
   +-- Мониторинг стабильности сервиса в течение контрольного периода
```

SLA по патчингу (24 часа для Critical, 7 дней для High) — типичное требование PCI DSS и внутренних политик безопасности банков. Каждое исключение из SLA должно быть задокументировано с обоснованием и планом устранения.

**5. Пересборка образов по расписанию:**

```yaml
# GitHub Actions: еженедельная пересборка для актуализации базового образа
name: Weekly Rebuild
on:
  schedule:
    - cron: '0 2 * * 1'  # Каждый понедельник в 2:00 UTC
  workflow_dispatch: {}   # Возможность ручного запуска

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

Флаг `--pull` гарантирует, что Docker скачает свежую версию базового образа, а не использует локальный кэш. Флаг `--no-cache` отключает кэш слоёв, обеспечивая полную пересборку. В результате все пакеты ОС и зависимости будут обновлены до последних доступных версий.

[к оглавлению](#Безопасность-контейнеров)

## Как встроить проверки безопасности в CI/CD пайплайн?
<!-- grade: -->

**Security gates** — обязательные проверки безопасности в пайплайне, которые блокируют продвижение артефакта при обнаружении проблем. Принцип **shift-left security** подразумевает выявление уязвимостей как можно раньше — на этапе написания кода и сборки, а не после деплоя в production.

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
        // ===== Stage 1: Статический анализ кода (SAST) =====
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
                        // Поиск случайно добавленных секретов в исходном коде
                        sh 'trivy fs --scanners secret --exit-code 1 .'
                    }
                }
            }
        }

        // ===== Stage 2: Проверка зависимостей (SCA) =====
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

        // ===== Stage 6: Проверка Kubernetes-манифестов =====
        stage('IaC Security') {
            steps {
                sh '''
                    # Проверка Kubernetes манифестов на мисконфигурации
                    trivy config --exit-code 1 --severity CRITICAL,HIGH k8s/

                    # Дополнительная проверка с помощью kubesec
                    kubesec scan k8s/deployment.yaml
                '''
            }
        }

        // ===== Stage 7: Push и подпись образа =====
        stage('Push & Sign') {
            steps {
                sh """
                    docker push ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}

                    # Подпись образа с помощью Cosign
                    cosign sign --key \${COSIGN_KEY} \
                        ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}

                    # Прикрепление SBOM к образу в реестре
                    cosign attach sbom \
                        --sbom sbom.cdx.json \
                        ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        // ===== Stage 8: Динамическое тестирование (DAST) =====
        stage('DAST - Dynamic Testing') {
            when {
                branch 'develop'
            }
            steps {
                sh '''
                    # Деплой в тестовую среду
                    kubectl apply -f k8s/ -n testing

                    # Запуск OWASP ZAP для динамического сканирования
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
            // Уведомление команды безопасности о провале security gate
            slackSend channel: '#security-alerts',
                color: 'danger',
                message: "Security gate FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}
```

Пайплайн выстроен так, что каждый последующий этап выполняется только при успехе предыдущего. Если на любом этапе обнаружена критическая проблема (`--exit-code 1`), пайплайн останавливается и образ не попадает в реестр. Параллельные этапы (`parallel`) ускоряют выполнение, сохраняя полноту проверок.

**Сводная таблица типов проверок безопасности:**

| Тип | Инструменты | Что проверяет | Когда запускать |
|-----|------------|---------------|-----------------|
| **SAST** (Static Application Security Testing) | SonarQube, SpotBugs + FindSecBugs, Semgrep | Уязвимости в исходном коде: SQL injection, XSS, небезопасная десериализация | При каждом коммите |
| **SCA** (Software Composition Analysis) | OWASP Dependency Check, Snyk | Уязвимости в сторонних зависимостях (библиотеках) | При сборке |
| **Secret Scan** | Trivy, GitLeaks, TruffleHog | Случайно добавленные секреты: пароли, ключи API, токены в коде и Git-истории | При каждом коммите |
| **Image Scan** | Trivy, Snyk, Grype | Уязвимости в Docker-образе: пакеты ОС и зависимости приложения | После сборки образа |
| **IaC Scan** | Trivy config, Checkov, kubesec | Мисконфигурации: Kubernetes-манифесты, Dockerfile, Terraform | При каждом коммите |
| **DAST** (Dynamic Application Security Testing) | OWASP ZAP | Уязвимости работающего приложения: проверка HTTP-эндпоинтов, заголовков, аутентификации | После деплоя в тестовую среду |
| **Lint** | hadolint, yamllint | Стилистические и безопасностные проблемы в Dockerfile и YAML | При каждом коммите |

**Ключевой принцип:** пайплайн должен **блокировать** продвижение артефакта при обнаружении критических уязвимостей. Для банковских систем ни одна CRITICAL-уязвимость не должна попасть в production. Параметр `--exit-code 1` в сканерах обеспечивает это: при обнаружении уязвимости указанного severity процесс завершается с ненулевым кодом, и CI/CD-система считает этап провалившимся.

[к оглавлению](#Безопасность-контейнеров)

## Что такое OWASP Docker Top 10?
<!-- grade: -->

**OWASP Docker Top 10** — список десяти наиболее критичных рисков безопасности при использовании Docker, подготовленный OWASP (Open Web Application Security Project). Аналогично знаменитому OWASP Top 10 для веб-приложений, этот документ помогает расставить приоритеты при обеспечении безопасности контейнеров.

**D01 — Secure User Mapping (Безопасное сопоставление пользователей):**

Контейнеры не должны запускаться от root. Необходимо использовать `USER` в Dockerfile для указания непривилегированного пользователя и `runAsNonRoot: true` в Kubernetes для принудительной проверки.

```dockerfile
RUN addgroup -S app && adduser -S app -G app
USER app
```

Если процесс в контейнере работает от root и атакующий эксплуатирует уязвимость контейнерной изоляции (container escape), он получает root-доступ к хосту.

**D02 — Patch Management Strategy (Стратегия управления патчами):**

Регулярное обновление базовых образов и зависимостей приложения. Включает: автоматическое сканирование образов (Trivy), автоматические PR с обновлениями (Renovate/Dependabot), пересборку образов по расписанию и SLA по времени устранения уязвимостей в зависимости от CVSS-балла.

**D03 — Network Segmentation and Firewalling (Сегментация сети):**

Применение Network Policies в Kubernetes для ограничения сетевого взаимодействия между подами. Принцип deny-by-default: начинать с полного запрета, затем явно разрешать только необходимые коммуникации.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes: [Ingress, Egress]
```

Без Network Policies любой под в кластере может обратиться к любому другому поду — компрометация одного сервиса даёт доступ ко всем остальным.

**D04 — Secure Defaults and Hardening (Безопасные настройки по умолчанию):**

Hardening Docker daemon: отключение ICC (`icc: false`), включение user namespace remapping, применение seccomp и AppArmor профилей. Регулярная проверка с помощью CIS Benchmark (Docker Bench for Security).

**D05 — Maintain Security Contexts (Поддержание контекста безопасности):**

Настройка Security Context для каждого пода в Kubernetes: `capabilities.drop: ALL` (удалить все привилегии), `readOnlyRootFilesystem: true` (запретить запись в файловую систему), `allowPrivilegeEscalation: false` (запретить повышение привилегий), `seccompProfile: RuntimeDefault` (ограничить системные вызовы).

**D06 — Protect Secrets (Защита секретов):**

Секреты никогда не должны находиться в образах (Dockerfile), переменных окружения (env) или репозитории Git. Правильные решения: HashiCorp Vault (enterprise-уровень), Sealed Secrets (для хранения зашифрованных секретов в Git), External Secrets Operator (для интеграции с внешними хранилищами).

**D07 — Resource Protection (Защита ресурсов):**

Обязательное ограничение CPU, памяти, PID и disk I/O для каждого контейнера. На уровне namespace: LimitRange (значения по умолчанию) и ResourceQuota (суммарные ограничения).

```yaml
resources:
  limits:
    memory: "512Mi"
    cpu: "1"
  requests:
    memory: "256Mi"
    cpu: "250m"
```

Без ограничений один контейнер с утечкой памяти или бесконечным циклом может вывести из строя весь хост.

**D08 — Container Image Integrity and Origin (Целостность и происхождение образов):**

Подпись образов с помощью Cosign или Docker Content Trust, верификация подписей при деплое через admission controller (Kyverno, OPA Gatekeeper). Использование только проверенных реестров (private registry), запрет на скачивание образов из публичных источников без проверки.

**D09 — Follow Immutable Paradigm (Неизменяемая парадигма):**

Контейнеры должны быть неизменяемыми (immutable) после запуска. Read-only файловая система, запрет модификации файлов в runtime. Обновление — это всегда замена контейнера новой версией, а не изменение существующего (не `docker exec` + `apt install`).

**D10 — Logging (Логирование):**

Централизованный сбор и анализ логов с помощью ELK/EFK stack (Elasticsearch + Logstash/Fluentd + Kibana), аудит действий Docker daemon и Kubernetes API, мониторинг runtime-событий с помощью Falco.

**Чек-лист OWASP Docker Top 10 для банковского проекта:**

```
[  ] D01: Все контейнеры запускаются от non-root пользователя
[  ] D02: Настроено автоматическое сканирование и обновление образов/зависимостей
[  ] D03: Network Policies применены ко всем namespace-ам, deny-by-default
[  ] D04: Docker daemon hardened, CIS Benchmark пройден без FAIL
[  ] D05: Security Context настроен для всех подов (drop ALL, read-only FS)
[  ] D06: Секреты хранятся в Vault/Sealed Secrets, не в Git и не в env
[  ] D07: Resource limits установлены для всех контейнеров
[  ] D08: Образы подписаны Cosign и верифицируются при деплое
[  ] D09: Read-only FS, контейнеры immutable, без ручных изменений
[  ] D10: Централизованное логирование, Falco для runtime security
```

[к оглавлению](#Безопасность-контейнеров)

## Как обеспечить безопасность Java-приложения в контейнере в банковской среде?
<!-- grade: -->

Это комплексный вопрос, объединяющий все аспекты контейнерной безопасности применительно к реальному банковскому Java-приложению. Ответ демонстрирует целостную картину: от Dockerfile до Kubernetes-манифестов, сетевых политик и чек-листа аудита.

**1. Безопасный Dockerfile для банковского Java-приложения:**

```dockerfile
# ============ Этап 1: Сборка ============
FROM eclipse-temurin:21-jdk-alpine@sha256:abc123... AS builder

WORKDIR /build

# Отдельные слои для зависимостей (лучшее кэширование Docker-слоёв)
COPY pom.xml mvnw ./
COPY .mvn .mvn
RUN ./mvnw dependency:go-offline -B

COPY src ./src
RUN ./mvnw clean package -DskipTests -B \
    && java -Djarmode=layertools -jar target/*.jar extract --destination /layers

# ============ Этап 2: Финальный образ ============
FROM eclipse-temurin:21-jre-alpine@sha256:def456...

# Метаданные OCI (для аудита и трассировки)
LABEL org.opencontainers.image.title="banking-payment-service" \
      org.opencontainers.image.vendor="Bank JSC" \
      org.opencontainers.image.authors="platform-team@bank.local"

# Создание непривилегированного пользователя с фиксированным UID/GID
RUN addgroup -g 1000 -S banking && \
    adduser -u 1000 -S banking -G banking && \
    mkdir -p /app /tmp && \
    chown -R banking:banking /app /tmp

WORKDIR /app

# Копирование слоёв Spring Boot (послойное копирование для лучшего кэширования)
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

Ключевые аспекты данного Dockerfile: multistage build (исходный код и build-инструменты не попадают в финальный образ), pin по digest (воспроизводимость), фиксированный UID/GID 1000 (согласованность с Kubernetes Security Context), послойное копирование Spring Boot (оптимизация кэша), HEALTHCHECK (мониторинг здоровья). Параметр `-Djava.security.egd=file:/dev/./urandom` ускоряет генерацию случайных чисел, что критично для TLS и криптографических операций.

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
      maxUnavailable: 0  # Нулевое время простоя при обновлении
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

      # Pod-level Security Context
      securityContext:
        runAsNonRoot: true
        fsGroup: 1000
        seccompProfile:
          type: RuntimeDefault

      # Распределение по разным нодам для отказоустойчивости
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

        # Container-level Security Context
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
          medium: Memory     # tmpfs — хранение в RAM, не на диске
          sizeLimit: 100Mi
      - name: logs
        emptyDir:
          sizeLimit: 500Mi

      imagePullSecrets:
      - name: registry-credentials
```

`maxUnavailable: 0` в стратегии RollingUpdate гарантирует, что во время обновления всегда доступно минимальное количество реплик — нулевое время простоя. `topologySpreadConstraints` распределяет реплики по разным физическим нодам, обеспечивая отказоустойчивость при выходе ноды из строя. Аннотации `vault.hashicorp.com/*` настраивают Vault Agent Injector для автоматической подстановки секретов. `ephemeral-storage` ограничивает использование временного дискового пространства контейнером.

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
  # Принимать трафик только от API Gateway
  - from:
    - podSelector:
        matchLabels:
          app: api-gateway
    ports:
    - protocol: TCP
      port: 8080

  egress:
  # К базе данных PostgreSQL
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
  # DNS (обязателен для резолвинга имён сервисов)
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
  # К Vault (для получения секретов)
  - to:
    - namespaceSelector:
        matchLabels:
          name: vault
    ports:
    - protocol: TCP
      port: 8200
```

Эта Network Policy реализует принцип наименьших привилегий на сетевом уровне: payment-service может принимать входящий трафик только от api-gateway и инициировать исходящие подключения только к базе данных, Kafka, DNS и Vault. Любые другие сетевые подключения (например, попытка обратиться к другому микросервису или к внешнему IP) будут заблокированы.

**4. Настройка namespace с принудительной безопасностью:**

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

Уровень `restricted` в Pod Security Standards запрещает создание подов, не соответствующих строгим требованиям безопасности: запуск от root, privileged, без seccomp-профиля и т.д. Это гарантирует, что даже при ошибке в манифесте небезопасный под не будет запущен.

**5. Spring Boot Security конфигурация (application-production.yml):**

```yaml
server:
  port: 8080
  ssl:
    enabled: false  # TLS терминируется на Ingress Controller или Service Mesh

management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus  # Минимальный набор эндпоинтов
  endpoint:
    health:
      show-details: never  # Не раскрывать детали подключений к БД и т.д. в production
      probes:
        enabled: true      # Включить /health/liveness и /health/readiness

spring:
  jackson:
    default-property-inclusion: non_null
    serialization:
      fail-on-empty-beans: false

# Логирование: не логировать чувствительные данные
logging:
  level:
    org.springframework.security: WARN
    org.hibernate.SQL: WARN
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
```

Ключевые решения: TLS не на уровне приложения (терминируется на Ingress или Service Mesh — централизованное управление сертификатами), минимальный набор Actuator-эндпоинтов (не раскрывать `/env`, `/beans`, `/configprops`), `show-details: never` (не показывать информацию о внутренней инфраструктуре), уровень логирования для security-модулей — WARN (не логировать попытки аутентификации на уровне DEBUG, чтобы избежать утечки токенов и паролей).

**6. Итоговый чек-лист безопасности для банковского Java-приложения в контейнерах:**

```
ОБРАЗ:
  [  ] Multistage build (исходники, build-инструменты не в финальном образе)
  [  ] Минимальный базовый образ (Alpine или Distroless)
  [  ] Pin по digest (@sha256:...), не по тегу
  [  ] USER — непривилегированный с фиксированным UID/GID
  [  ] HEALTHCHECK указан в Dockerfile
  [  ] Нет секретов в слоях образа (проверено через docker history и secret scan)
  [  ] COPY вместо ADD (ADD может распаковывать архивы и скачивать URL)
  [  ] .dockerignore настроен (исключены .git, .env, .idea, target)

KUBERNETES:
  [  ] runAsNonRoot: true
  [  ] readOnlyRootFilesystem: true
  [  ] allowPrivilegeEscalation: false
  [  ] capabilities: drop ALL
  [  ] seccompProfile: RuntimeDefault
  [  ] Resource limits (memory, cpu, ephemeral-storage) установлены
  [  ] Network Policies применены (deny-by-default + явные разрешения)
  [  ] ServiceAccount с минимальными правами (RBAC)
  [  ] automountServiceAccountToken: false
  [  ] Pod Security Standards: restricted на namespace

СЕКРЕТЫ:
  [  ] Vault / Sealed Secrets / External Secrets Operator
  [  ] Encryption at rest включён для Kubernetes Secrets
  [  ] Нет секретов в Git, переменных окружения, Dockerfile

CI/CD:
  [  ] SAST (SonarQube, SpotBugs + FindSecBugs)
  [  ] SCA (OWASP Dependency Check или Snyk)
  [  ] Image scanning (Trivy) с блокировкой при CRITICAL
  [  ] Secret detection (Trivy/GitLeaks)
  [  ] Dockerfile linting (hadolint)
  [  ] K8s manifest scanning (trivy config / kubesec)
  [  ] Подпись образов (Cosign)
  [  ] SBOM генерация и прикрепление к образу

RUNTIME:
  [  ] Falco для мониторинга аномалий
  [  ] Централизованное логирование (ELK/EFK)
  [  ] Мониторинг метрик (Prometheus + Grafana)
  [  ] Регулярное обновление базовых образов с SLA по CVSS
```

Безопасность контейнеров — это не разовое действие, а непрерывный процесс. В банковской среде это требование регуляторов (ЦБ РФ, PCI DSS), и его несоблюдение влечёт серьёзные финансовые и репутационные последствия. Каждый пункт данного чек-листа должен быть реализован и регулярно проверяться в рамках аудитов безопасности.

[к оглавлению](#Безопасность-контейнеров)
