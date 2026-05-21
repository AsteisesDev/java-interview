[Вопросы для собеседования](README.md)

# Docker

+ [Что такое Docker?](#что-такое-docker)
+ [Что такое контейнеризация и чем она отличается от виртуализации?](#что-такое-контейнеризация-и-чем-она-отличается-от-виртуализации)
+ [Какие основные понятия существуют в Docker?](#какие-основные-понятия-существуют-в-docker)
+ [Опишите архитектуру Docker](#опишите-архитектуру-docker)
+ [Что такое Dockerfile?](#что-такое-dockerfile)
+ [Какие основные инструкции используются в Dockerfile?](#какие-основные-инструкции-используются-в-dockerfile)
+ [В чём разница между CMD и ENTRYPOINT?](#в-чём-разница-между-cmd-и-entrypoint)
+ [В чём разница между COPY и ADD?](#в-чём-разница-между-copy-и-add)
+ [Что такое multi-stage build и зачем он нужен?](#что-такое-multi-stage-build-и-зачем-он-нужен)
+ [Что такое слои образа (image layers)?](#что-такое-слои-образа-image-layers)
+ [Как оптимизировать Dockerfile?](#как-оптимизировать-dockerfile)
+ [Что такое .dockerignore?](#что-такое-dockerignore)
+ [Перечислите основные команды Docker](#перечислите-основные-команды-docker)
+ [Что такое Docker Compose?](#что-такое-docker-compose)
+ [Какие основные директивы используются в docker-compose.yml?](#какие-основные-директивы-используются-в-docker-composeyml)
+ [Что такое тома (volumes) и bind mounts?](#что-такое-тома-volumes-и-bind-mounts)
+ [Какие типы сетей существуют в Docker?](#какие-типы-сетей-существуют-в-docker)
+ [Как написать Dockerfile для Spring Boot приложения?](#как-написать-dockerfile-для-spring-boot-приложения)
+ [Что такое layered JAR в Spring Boot и как его использовать с Docker?](#что-такое-layered-jar-в-spring-boot-и-как-его-использовать-с-docker)
+ [Как передавать переменные окружения и конфигурацию в Docker-контейнер?](#как-передавать-переменные-окружения-и-конфигурацию-в-docker-контейнер)
+ [Что такое health check в Docker?](#что-такое-health-check-в-docker)
+ [Как устроено логирование в Docker?](#как-устроено-логирование-в-docker)
+ [Как Docker используется в CI/CD?](#как-docker-используется-в-cicd)
+ [Какие практики безопасности следует соблюдать при работе с Docker?](#какие-практики-безопасности-следует-соблюдать-при-работе-с-docker)
+ [Как Docker взаимодействует с JVM? Какие есть нюансы?](#как-docker-взаимодействует-с-jvm-какие-есть-нюансы)
+ [Как организовать docker-compose для Java-приложения с базой данных?](#как-организовать-docker-compose-для-java-приложения-с-базой-данных)
+ [Что такое Docker Registry и Docker Hub?](#что-такое-docker-registry-и-docker-hub)
+ [Чем отличается контейнер от образа?](#чем-отличается-контейнер-от-образа)

## Что такое Docker?
<!-- grade: junior -->

Docker — это платформа с открытым исходным кодом для автоматизации разработки, доставки и запуска приложений в изолированных средах, называемых контейнерами.

> Аналогия из жизни: Docker — это как морской контейнер для перевозки грузов. Не важно, что внутри — мебель, электроника или продукты — контейнер стандартизирован, его можно погрузить на любой корабль, поезд или грузовик. Так и Docker-контейнер: внутри может быть любое приложение, но он запустится на любой машине с установленным Docker.

Docker позволяет упаковать приложение вместе со всем его окружением и зависимостями в стандартизированный блок (контейнер), который можно запустить на любой машине с установленным Docker. Это решает классическую проблему "у меня на машине работает, а на сервере нет".

### Зачем нужен Docker

- Единообразие окружения — разработка, тестирование и production используют одинаковое окружение
- Изоляция — каждое приложение работает в своём контейнере, не мешая другим
- Быстрое развёртывание — контейнеры запускаются за секунды, в отличие от виртуальных машин
- Масштабирование — легко запустить несколько экземпляров приложения
- Воспроизводимость — Dockerfile описывает среду декларативно, что гарантирует одинаковый результат при сборке
- Упрощение CI/CD — контейнеры прекрасно вписываются в пайплайны непрерывной интеграции и доставки

> **На собеседовании:** достаточно дать определение и перечислить 3-4 преимущества. Покажите, что понимаете главную идею: Docker решает проблему "works on my machine" за счёт упаковки приложения вместе с окружением. Часто спрашивают следом про отличие от виртуализации.

[к оглавлению](#docker)

---

## Что такое контейнеризация и чем она отличается от виртуализации?
<!-- grade: junior -->

Контейнеризация — это метод виртуализации на уровне операционной системы, при котором ядро ОС позволяет запускать несколько изолированных экземпляров пользовательского пространства (контейнеров) вместо одного.

### Ключевые отличия от классической виртуализации

| Характеристика | Виртуализация (VM) | Контейнеризация |
|---|---|---|
| Уровень изоляции | Полная ОС с собственным ядром | Общее ядро хостовой ОС |
| Гипервизор | Требуется (VMware, VirtualBox, Hyper-V) | Не требуется |
| Размер | Гигабайты (полная ОС) | Мегабайты (только приложение и зависимости) |
| Время запуска | Минуты | Секунды |
| Потребление ресурсов | Высокое (каждая VM имеет свою ОС) | Низкое (контейнеры разделяют ядро) |
| Портативность | Ограничена форматом VM | Высокая (стандарт OCI) |
| Плотность | Десятки VM на хосте | Сотни/тысячи контейнеров на хосте |

Виртуальная машина эмулирует полный аппаратный сервер, включая процессор, память, диск, сеть, и на ней работает полноценная гостевая ОС. Контейнер же использует ядро хостовой ОС и изолирует только процессы, файловую систему и сеть с помощью механизмов Linux: namespaces (изоляция) и cgroups (ограничение ресурсов).

> **На собеседовании:** ключевое отличие — контейнеры разделяют ядро хостовой ОС, а VM содержат полноценную гостевую ОС. Из-за этого контейнеры легче, быстрее стартуют, но дают меньшую изоляцию. Упомяните namespaces и cgroups — это покажет понимание внутренних механизмов.

[к оглавлению](#docker)

---

## Какие основные понятия существуют в Docker?
<!-- grade: junior -->

Docker оперирует набором ключевых абстракций, каждая из которых решает свою задачу.

- Образ (Image) — неизменяемый шаблон, содержащий файловую систему и метаданные для создания контейнера. Образ состоит из набора слоёв, каждый из которых представляет собой результат выполнения одной инструкции Dockerfile. Образы хранятся в реестрах и могут быть версионированы с помощью тегов (например, `openjdk:17-slim`).

- Контейнер (Container) — запущенный экземпляр образа. Контейнер добавляет поверх слоёв образа тонкий записываемый слой (writable layer), в котором хранятся все изменения, сделанные во время работы. Контейнер можно запустить, остановить, удалить, а также подключиться к нему.

- Реестр (Registry) — хранилище образов Docker. Реестр может быть публичным или приватным. Примеры: Docker Hub, GitHub Container Registry, Amazon ECR, Google Container Registry, GitLab Container Registry.

- Docker Hub — самый популярный публичный реестр Docker-образов. Содержит официальные образы (OpenJDK, PostgreSQL, Nginx и др.) и пользовательские образы. Бесплатный аккаунт позволяет хранить один приватный репозиторий.

- Тег (Tag) — метка версии образа. Например, `eclipse-temurin:17-jre-alpine` — образ Eclipse Temurin JRE 17 на базе Alpine Linux. Тег `latest` используется по умолчанию, если тег не указан явно.

- Том (Volume) — механизм для сохранения данных, созданных контейнером, вне его жизненного цикла. Данные в томах не удаляются при удалении контейнера.

> **На собеседовании:** перечислите минимум четыре понятия: образ, контейнер, реестр, том. Покажите связь между ними: из образа создаётся контейнер, образы хранятся в реестре, данные контейнера сохраняются в томах.

[к оглавлению](#docker)

---

## Опишите архитектуру Docker
<!-- grade: middle -->

Docker использует клиент-серверную архитектуру, состоящую из нескольких компонентов.

### Docker Daemon (dockerd)

Серверный процесс, который управляет Docker-объектами: образами, контейнерами, сетями и томами. Daemon слушает запросы через Docker API (REST API по Unix-сокету или TCP). Он отвечает за сборку образов, запуск контейнеров, управление сетями и хранением.

### Docker Client (docker)

Основной интерфейс для взаимодействия пользователя с Docker. Когда пользователь вводит команду вроде `docker run`, клиент отправляет соответствующий запрос к Docker Daemon через Docker API. Клиент может работать с удалённым daemon.

### Docker Engine

Общее название для комбинации Docker Daemon, REST API и CLI-клиента. Docker Engine бывает двух редакций:
- Docker Engine Community (CE) — бесплатная версия.
- Docker Engine Enterprise (EE) — коммерческая версия с дополнительными функциями.

### Containerd

Среда выполнения контейнеров (container runtime), которую Docker использует внутри для управления жизненным циклом контейнеров. Containerd отвечает за загрузку образов, запуск и остановку контейнеров.

### Схема взаимодействия

```
Docker Client (CLI)
    │
    │  REST API
    ▼
Docker Daemon (dockerd)
    │
    ▼
Containerd → runc (создание контейнеров)
    │
    ▼
Контейнеры, Образы, Тома, Сети
```

> **На собеседовании:** важно показать, что Docker — это не монолит, а клиент-серверная архитектура. CLI-клиент общается с daemon через REST API, а daemon делегирует запуск контейнеров containerd/runc. Знание этой цепочки показывает понимание того, как работает Docker на уровне процессов.

[к оглавлению](#docker)

---

## Что такое Dockerfile?
<!-- grade: junior -->

Dockerfile — это текстовый файл с набором инструкций для автоматической сборки Docker-образа. Каждая инструкция в Dockerfile создаёт новый слой в образе.

### Пример для Java-приложения

```dockerfile
# Базовый образ с JRE
FROM eclipse-temurin:17-jre-alpine

# Рабочая директория внутри контейнера
WORKDIR /app

# Копируем JAR-файл в контейнер
COPY target/myapp.jar app.jar

# Порт, который слушает приложение
EXPOSE 8080

# Команда запуска
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Сборка образа

Для сборки образа из Dockerfile используется команда:

```bash
docker build -t myapp:1.0 .
```

Здесь `-t` задаёт имя и тег образа, а `.` указывает на контекст сборки (директорию, из которой Docker берёт файлы).

### Правила именования

Файл должен называться `Dockerfile` (без расширения). Можно использовать другое имя, передав флаг `-f`:

```bash
docker build -f Dockerfile.prod -t myapp:prod .
```

> **На собеседовании:** определите Dockerfile как декларативное описание образа. Упомяните, что каждая инструкция создаёт слой, и что порядок инструкций влияет на кэширование при пересборке. Будьте готовы написать простой Dockerfile для Spring Boot приложения.

[к оглавлению](#docker)

---

## Какие основные инструкции используются в Dockerfile?
<!-- grade: junior -->

Инструкции Dockerfile — это команды, определяющие содержимое и поведение Docker-образа.

| Инструкция | Описание |
|---|---|
| `FROM` | Задаёт базовый образ. Каждый Dockerfile должен начинаться с `FROM`. Пример: `FROM eclipse-temurin:17-jre-alpine` |
| `RUN` | Выполняет команду в процессе сборки образа и создаёт новый слой. Пример: `RUN apt-get update && apt-get install -y curl` |
| `COPY` | Копирует файлы и директории из контекста сборки в файловую систему образа. Пример: `COPY target/app.jar /app/app.jar` |
| `ADD` | Аналог `COPY`, но дополнительно поддерживает URL и автоматическую распаковку архивов (tar, gzip и т.д.) |
| `CMD` | Определяет команду по умолчанию, которая выполнится при запуске контейнера. Может быть переопределена аргументами `docker run` |
| `ENTRYPOINT` | Определяет исполняемый файл, который запустится при старте контейнера. В отличие от `CMD`, не так легко переопределяется |
| `EXPOSE` | Документирует, какой порт слушает приложение внутри контейнера. Не публикует порт автоматически — для этого нужен `-p` при `docker run` |
| `ENV` | Устанавливает переменную окружения, которая будет доступна и при сборке, и в работающем контейнере. Пример: `ENV JAVA_OPTS="-Xmx512m"` |
| `ARG` | Определяет переменную, доступную только во время сборки. Пример: `ARG JAR_FILE=target/*.jar` |
| `WORKDIR` | Устанавливает рабочую директорию для последующих инструкций `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD`. Пример: `WORKDIR /app` |
| `VOLUME` | Создаёт точку монтирования для тома. Данные в этой директории будут сохраняться вне контейнера. Пример: `VOLUME /data` |
| `USER` | Задаёт пользователя, от имени которого выполняются последующие инструкции и запускается контейнер. Пример: `USER appuser` |
| `LABEL` | Добавляет метаданные к образу. Пример: `LABEL maintainer="dev@example.com"` |
| `HEALTHCHECK` | Определяет команду для проверки состояния контейнера |

> **На собеседовании:** не нужно перечислять все инструкции наизусть. Важно знать ключевые: FROM, RUN, COPY, CMD, ENTRYPOINT, EXPOSE, ENV, WORKDIR. Часто задают уточняющие вопросы про разницу CMD/ENTRYPOINT и COPY/ADD — это отдельные вопросы собеседования.

[к оглавлению](#docker)

---

## В чём разница между CMD и ENTRYPOINT?
<!-- grade: middle -->

CMD и ENTRYPOINT — это инструкции Dockerfile, определяющие, что будет выполняться при запуске контейнера, но различающиеся поведением при переопределении аргументов.

### CMD — команда по умолчанию

CMD задаёт команду, которая легко переопределяется аргументами `docker run`:

```dockerfile
FROM eclipse-temurin:17-jre-alpine
CMD ["java", "-jar", "app.jar"]
```

```bash
# Выполнит java -jar app.jar
docker run myapp

# CMD полностью заменяется: выполнит /bin/sh
docker run myapp /bin/sh
```

### ENTRYPOINT — основная команда

ENTRYPOINT задаёт основную команду, а аргументы `docker run` добавляются к ней:

```dockerfile
FROM eclipse-temurin:17-jre-alpine
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```bash
# Выполнит java -jar app.jar
docker run myapp

# Аргументы добавятся: java -jar app.jar --spring.profiles.active=prod
docker run myapp --spring.profiles.active=prod
```

### Комбинация ENTRYPOINT + CMD

Идиоматический подход: ENTRYPOINT задаёт исполняемый файл, а CMD — аргументы по умолчанию:

```dockerfile
FROM eclipse-temurin:17-jre-alpine
ENTRYPOINT ["java", "-jar", "app.jar"]
CMD ["--spring.profiles.active=dev"]
```

```bash
# Выполнит java -jar app.jar --spring.profiles.active=dev
docker run myapp

# CMD заменяется: java -jar app.jar --spring.profiles.active=prod
docker run myapp --spring.profiles.active=prod
```

### Формы записи

| Форма | Синтаксис | PID 1 | Сигналы ОС |
|---|---|---|---|
| Exec-форма (рекомендуется) | `ENTRYPOINT ["java", "-jar", "app.jar"]` | Процесс Java | Доходят напрямую |
| Shell-форма | `ENTRYPOINT java -jar app.jar` | `/bin/sh -c` | Не доходят до Java |

Shell-форма оборачивает команду в `/bin/sh -c`, из-за чего Java-процесс не получает PID 1 и сигналы ОС (SIGTERM) не доходят до приложения напрямую. Это приводит к проблемам с graceful shutdown.

> **На собеседовании:** ключевое различие: CMD легко переопределяется целиком, а к ENTRYPOINT аргументы добавляются. Идиоматический подход — комбинация ENTRYPOINT + CMD. Обязательно упомяните exec-форму и проблему PID 1 при shell-форме — это показывает production-опыт.

[к оглавлению](#docker)

---

## В чём разница между COPY и ADD?
<!-- grade: junior -->

COPY и ADD — это инструкции Dockerfile для копирования файлов в образ, но ADD обладает дополнительными возможностями, которые в большинстве случаев не нужны.

### COPY — простое копирование

```dockerfile
COPY target/app.jar /app/app.jar
COPY config/ /app/config/
```

### ADD — расширенное копирование

```dockerfile
# Автоматически распаковывает tar-архив
ADD archive.tar.gz /app/

# Может загружать файл по URL (не рекомендуется)
ADD https://example.com/file.txt /app/
```

### Рекомендация

Всегда используйте COPY, если вам не нужна автоматическая распаковка архивов. Это делает Dockerfile более прозрачным и предсказуемым. ADD с URL считается анти-паттерном — лучше использовать `RUN curl` или `RUN wget`, чтобы можно было контролировать процесс и удалить загруженный файл в том же слое:

```dockerfile
# Плохо
ADD https://example.com/tool.tar.gz /opt/

# Хорошо
RUN curl -fsSL https://example.com/tool.tar.gz | tar -xz -C /opt/
```

> **На собеседовании:** короткий ответ: используйте COPY всегда, кроме случаев, когда нужна автоматическая распаковка tar-архива. ADD с URL — анти-паттерн. Этот вопрос проверяет, знаете ли вы best practices написания Dockerfile.

[к оглавлению](#docker)

---

## Что такое multi-stage build и зачем он нужен?
<!-- grade: middle -->

Multi-stage build (многоступенчатая сборка) — это подход, при котором в одном Dockerfile используется несколько инструкций FROM, где каждая начинает новую стадию сборки, а в финальный образ попадает только результат последней стадии.

### Зачем нужен

- Разделение среды сборки и среды выполнения
- Уменьшение размера финального образа — в него не попадают инструменты сборки (Maven, Gradle, JDK)
- Один Dockerfile для всего процесса — от исходного кода до готового образа

### Пример для Spring Boot приложения с Maven

```dockerfile
# --- Стадия 1: сборка ---
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
# Загружаем зависимости отдельным слоем (кэширование)
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn package -DskipTests -B

# --- Стадия 2: запуск ---
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
# Копируем только JAR из стадии сборки
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Результат: финальный образ содержит только JRE и JAR-файл, без Maven, JDK, исходного кода и промежуточных артефактов. Размер образа может уменьшиться с ~800 МБ до ~200 МБ.

<details><summary>Пример с Gradle</summary>

```dockerfile
FROM gradle:8.5-jdk17 AS build
WORKDIR /app
COPY build.gradle settings.gradle ./
COPY src ./src
RUN gradle bootJar --no-daemon

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

</details>

> **На собеседовании:** объясните главную идею: multi-stage build позволяет собирать приложение в одном контейнере (с JDK и Maven), а запускать в другом (только JRE). Это уменьшает размер образа и поверхность атаки. Покажите, что знаете синтаксис `COPY --from=build`.

[к оглавлению](#docker)

---

## Что такое слои образа (image layers)?
<!-- grade: middle -->

Слои образа — это неизменяемые (immutable) read-only блоки файловой системы, из которых состоит Docker-образ. Каждая инструкция в Dockerfile (FROM, RUN, COPY, ADD) создаёт новый слой.

### Как работают слои

```
┌──────────────────────────┐
│   Writable Layer         │  ← Контейнер (записываемый слой)
├──────────────────────────┤
│   ENTRYPOINT java -jar   │  ← Только метаданные (не создаёт слой)
├──────────────────────────┤
│   COPY app.jar           │  ← Слой 3
├──────────────────────────┤
│   RUN apt-get install    │  ← Слой 2
├──────────────────────────┤
│   FROM ubuntu:22.04      │  ← Слой 1 (базовый образ)
└──────────────────────────┘
```

### Кэширование слоёв

Кэширование слоёв — одна из ключевых оптимизаций Docker. При повторной сборке Docker проверяет, изменилась ли инструкция и её контекст. Если нет — используется кэшированный слой. Но если один слой инвалидируется, все последующие слои тоже пересобираются.

### Оптимальный порядок инструкций

```dockerfile
# Плохо: любое изменение в исходном коде инвалидирует кэш зависимостей
COPY . /app
RUN mvn package

# Хорошо: зависимости кэшируются отдельно
COPY pom.xml /app/
RUN mvn dependency:go-offline
COPY src /app/src
RUN mvn package
```

### Разделение слоёв между образами

Слои разделяются между образами: если два образа используют одну базу (`FROM eclipse-temurin:17-jre-alpine`), базовый слой хранится на диске только один раз.

> **На собеседовании:** покажите понимание двух вещей: (1) слои неизменяемы и кэшируются, (2) инвалидация одного слоя каскадно пересобирает все последующие. Отсюда правило: инструкции, которые меняются реже (зависимости), должны идти перед теми, которые меняются чаще (исходный код).

[к оглавлению](#docker)

---

## Как оптимизировать Dockerfile?
<!-- grade: middle -->

Оптимизация Dockerfile направлена на уменьшение размера образа и ускорение сборки за счёт правильной организации инструкций и выбора базовых образов.

### 1. Правильный порядок инструкций (кэширование)

Инструкции, которые меняются реже, должны быть в начале:

```dockerfile
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package
```

### 2. Минимизация количества слоёв

Объединяйте команды RUN в одну инструкцию:

```dockerfile
# Плохо: 3 слоя
RUN apt-get update
RUN apt-get install -y curl
RUN rm -rf /var/lib/apt/lists/*

# Хорошо: 1 слой
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*
```

### 3. Используйте минимальные базовые образы

- `eclipse-temurin:17-jre-alpine` (~80 МБ) вместо `eclipse-temurin:17-jdk` (~400 МБ)
- Alpine-образы значительно меньше Debian/Ubuntu-основанных
- Для production нужен JRE, а не JDK

### 4. Удаляйте ненужное в том же слое

```dockerfile
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    curl -o tool.tar.gz https://example.com/tool.tar.gz && \
    tar -xzf tool.tar.gz && \
    rm tool.tar.gz && \
    apt-get purge -y curl && \
    rm -rf /var/lib/apt/lists/*
```

### 5. Дополнительные практики

- Используйте `.dockerignore` для исключения ненужных файлов из контекста сборки
- Используйте multi-stage builds для разделения среды сборки и запуска
- Не запускайте контейнер от root
- Используйте конкретные теги образов вместо `latest`:

```dockerfile
# Плохо: непредсказуемый результат
FROM eclipse-temurin:latest

# Хорошо: воспроизводимая сборка
FROM eclipse-temurin:17.0.9_9-jre-alpine
```

> **На собеседовании:** назовите минимум 4-5 практик. Самые важные: порядок инструкций для кэширования, минимальные базовые образы, multi-stage build, объединение RUN-команд, конкретные теги. Если можете объяснить почему каждая практика работает (через понимание слоёв) — это уровень middle+.

[к оглавлению](#docker)

---

## Что такое .dockerignore?
<!-- grade: junior -->

.dockerignore — это файл, определяющий, какие файлы и директории нужно исключить из контекста сборки Docker. Он работает аналогично `.gitignore`.

При выполнении `docker build .` Docker отправляет весь контекст сборки (содержимое указанной директории) демону Docker. Если в проекте есть большие или ненужные файлы, это замедляет сборку и может привести к попаданию конфиденциальных данных в образ.

### Пример .dockerignore для Java-проекта

```
# Результаты сборки
target/
build/
*.jar
*.war

# IDE
.idea/
*.iml
.vscode/
.settings/
.project
.classpath

# Git
.git
.gitignore

# Docker
Dockerfile
docker-compose.yml
.dockerignore

# Документация
*.md
docs/

# Логи
*.log

# Переменные окружения
.env
.env.*
```

### Зачем нужен .dockerignore

- Ускоряет сборку (меньше файлов отправляется демону)
- Предотвращает попадание конфиденциальных файлов (`.env`, ключи) в образ
- Исключает ненужные файлы, которые могут инвалидировать кэш слоёв

> **На собеседовании:** объясните, что .dockerignore исключает файлы из контекста сборки (а не из образа напрямую). Это ускоряет сборку и предотвращает утечку секретов. Приведите пример: без .dockerignore директория `.git` попадёт в контекст, что и замедлит сборку, и может попасть в образ через `COPY . .`.

[к оглавлению](#docker)

---

## Перечислите основные команды Docker
<!-- grade: junior -->

Команды Docker разделяются на группы по работе с образами, контейнерами и инфраструктурой.

### Работа с образами

| Команда | Описание |
|---|---|
| `docker build -t name:tag .` | Собрать образ из Dockerfile |
| `docker images` / `docker image ls` | Список локальных образов |
| `docker pull image:tag` | Скачать образ из реестра |
| `docker push image:tag` | Отправить образ в реестр |
| `docker rmi image:tag` | Удалить образ |
| `docker image prune` | Удалить неиспользуемые образы |

### Работа с контейнерами

| Команда | Описание |
|---|---|
| `docker run image` | Создать и запустить контейнер |
| `docker run -d -p 8080:8080 --name myapp image` | Запустить в фоне с маппингом портов и именем |
| `docker ps` | Список запущенных контейнеров |
| `docker ps -a` | Список всех контейнеров (включая остановленные) |
| `docker stop container` | Остановить контейнер (SIGTERM, затем SIGKILL) |
| `docker start container` | Запустить остановленный контейнер |
| `docker restart container` | Перезапустить контейнер |
| `docker rm container` | Удалить контейнер |
| `docker logs container` | Посмотреть логи контейнера |
| `docker logs -f container` | Следить за логами в реальном времени |
| `docker exec -it container /bin/sh` | Войти в работающий контейнер |
| `docker inspect container` | Подробная информация о контейнере (JSON) |

<details><summary>Примеры для Java-разработчика</summary>

```bash
# Собрать образ Spring Boot приложения
docker build -t my-spring-app:1.0 .

# Запустить с переменными окружения и маппингом порта
docker run -d -p 8080:8080 \
  -e SPRING_PROFILES_ACTIVE=prod \
  -e DB_URL=jdbc:postgresql://db:5432/mydb \
  --name my-spring-app \
  my-spring-app:1.0

# Проверить логи
docker logs -f my-spring-app

# Войти в контейнер для отладки
docker exec -it my-spring-app /bin/sh
```

</details>

> **На собеседовании:** не нужно перечислять все команды. Покажите, что знаете рабочий цикл: `docker build` (сборка) -> `docker run` (запуск) -> `docker logs` (отладка) -> `docker stop`/`docker rm` (остановка/удаление). Упомяните `docker exec` для входа в контейнер.

[к оглавлению](#docker)

---

## Что такое Docker Compose?
<!-- grade: junior -->

Docker Compose — это инструмент для определения и запуска многоконтейнерных Docker-приложений. Конфигурация описывается в файле `docker-compose.yml` (YAML), и с помощью одной команды можно поднять всю инфраструктуру.

### Зачем нужен

- Локальная разработка — запуск приложения вместе с базой данных, кэшем, очередью сообщений одной командой
- Тестовые окружения
- Описание зависимостей между сервисами

### Основные команды

| Команда | Описание |
|---|---|
| `docker compose up` | Создать и запустить все сервисы |
| `docker compose up -d` | Запустить в фоновом режиме |
| `docker compose down` | Остановить и удалить контейнеры, сети |
| `docker compose down -v` | Остановить и удалить вместе с томами |
| `docker compose build` | Пересобрать образы |
| `docker compose logs -f` | Следить за логами всех сервисов |
| `docker compose ps` | Список запущенных сервисов |
| `docker compose exec service cmd` | Выполнить команду в контейнере сервиса |

### Пример docker-compose.yml

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=dev
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

Начиная с Docker Compose V2, команда `docker-compose` (через дефис) заменена на `docker compose` (через пробел). Ключ `version` в `docker-compose.yml` теперь является необязательным.

> **На собеседовании:** Docker Compose — стандартный инструмент для локальной разработки Java-приложений. Покажите, что умеете описать типичный стек: приложение + БД + кэш. Упомяните, что `depends_on` гарантирует только порядок запуска контейнеров, но не готовность сервиса внутри.

[к оглавлению](#docker)

---

## Какие основные директивы используются в docker-compose.yml?
<!-- grade: middle -->

Директивы docker-compose.yml — это ключевые слова YAML-конфигурации, определяющие сервисы, их образы, сети, тома и поведение при запуске.

### services

Определяет контейнеры (сервисы), которые будут запущены:

```yaml
services:
  app:
    image: my-spring-app:1.0
  db:
    image: postgres:16-alpine
```

### image / build

Указывает, какой образ использовать или как его собрать:

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    # или
    image: my-spring-app:1.0
```

### ports

Маппинг портов `хост:контейнер`:

```yaml
ports:
  - "8080:8080"
  - "5005:5005"  # debug порт
```

### environment / env_file

Переменные окружения:

```yaml
environment:
  SPRING_PROFILES_ACTIVE: prod
  JAVA_OPTS: "-Xmx512m -Xms256m"
# или
environment:
  - SPRING_PROFILES_ACTIVE=prod

# загрузка из файла
env_file:
  - .env
```

### volumes

Монтирование томов:

```yaml
volumes:
  - postgres-data:/var/lib/postgresql/data   # именованный том
  - ./config:/app/config                      # bind mount
```

### networks

Определение сетей:

```yaml
services:
  app:
    networks:
      - backend
  db:
    networks:
      - backend
networks:
  backend:
    driver: bridge
```

### depends_on

Порядок запуска сервисов:

```yaml
depends_on:
  - db
  - redis
# С проверкой условия (Compose V2):
depends_on:
  db:
    condition: service_healthy
```

`depends_on` гарантирует только порядок запуска контейнеров, но не то, что сервис внутри контейнера готов принимать соединения. Для этого нужно использовать `condition: service_healthy` с health check или механизм retry на уровне приложения.

### restart

Политика перезапуска:

```yaml
restart: unless-stopped
# Другие варианты: no, always, on-failure
```

### healthcheck

Проверка состояния сервиса:

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8080/actuator/health"]
  interval: 30s
  timeout: 10s
  retries: 3
  start_period: 40s
```

> **На собеседовании:** назовите ключевые директивы: services, image/build, ports, environment, volumes, depends_on, healthcheck. Частая ловушка — вопрос про depends_on: он не ждёт готовности сервиса, а только запускает контейнеры в нужном порядке. Для ожидания готовности нужен `condition: service_healthy`.

[к оглавлению](#docker)

---

## Что такое тома (volumes) и bind mounts?
<!-- grade: middle -->

Тома и bind mounts — это механизмы Docker для сохранения данных вне жизненного цикла контейнера, поскольку контейнеры по своей природе эфемерны и при удалении все данные внутри теряются.

### Тома (Volumes)

Управляемые Docker хранилища данных, расположенные в файловой системе хоста (`/var/lib/docker/volumes/`):

```bash
# Создать и использовать именованный том
docker run -v postgres-data:/var/lib/postgresql/data postgres:16

# Создать том отдельно
docker volume create my-volume
docker volume ls
docker volume inspect my-volume
docker volume rm my-volume
```

### Bind mounts

Монтирование конкретной директории или файла хоста в контейнер:

```bash
# Монтирование директории хоста
docker run -v /path/on/host:/path/in/container myapp

# Монтирование файла конфигурации
docker run -v ./application.yml:/app/config/application.yml myapp
```

### Сравнение

| Характеристика | Volumes | Bind mounts |
|---|---|---|
| Управление | Docker управляет расположением | Пользователь указывает точный путь |
| Расположение | `/var/lib/docker/volumes/` | Любая директория хоста |
| Портативность | Высокая (не зависит от файловой системы хоста) | Низкая (привязка к структуре хоста) |
| Использование | Данные БД, постоянное хранение | Конфиги, исходный код при разработке |
| Backup | Проще через Docker CLI | Обычными средствами ОС |
| Права доступа | Docker контролирует | Зависят от прав на хосте |

### Типичные сценарии для Java-разработчика

- Volume — данные PostgreSQL, данные Elasticsearch, кэш Maven/Gradle
- Bind mount — `application.yml` при разработке, исходный код для hot-reload

### tmpfs

Третий вид монтирования. Данные хранятся только в оперативной памяти хоста и не записываются на диск. Подходит для временных конфиденциальных данных:

```bash
docker run --tmpfs /app/tmp myapp
```

> **На собеседовании:** объясните разницу: volumes управляются Docker и подходят для данных БД, bind mounts — для монтирования конфигов и исходного кода при разработке. Упомяните tmpfs как третий вариант для конфиденциальных временных данных. Частая ошибка — путать volumes и bind mounts.

[к оглавлению](#docker)

---

## Какие типы сетей существуют в Docker?
<!-- grade: middle -->

Docker предоставляет несколько сетевых драйверов для организации связи между контейнерами, каждый из которых решает свой сценарий использования.

### bridge (по умолчанию)

Изолированная сеть на хосте. Контейнеры в одной bridge-сети могут общаться друг с другом по имени контейнера (DNS-резолвинг):

```bash
# Создать пользовательскую bridge-сеть
docker network create my-network

# Запустить контейнеры в одной сети
docker run -d --name db --network my-network postgres:16
docker run -d --name app --network my-network my-spring-app
# Приложение может подключиться к БД по имени "db"
```

В стандартной bridge-сети (`docker0`) DNS по имени контейнера не работает. Используйте пользовательские bridge-сети.

### host

Контейнер использует сетевой стек хоста напрямую. Нет изоляции сети, порты контейнера сразу доступны на хосте:

```bash
docker run --network host my-spring-app
# Приложение на порту 8080 сразу доступно на хосте
```

Используется для максимальной производительности сети. На macOS и Windows работает иначе из-за виртуализации.

### none

У контейнера нет сетевого интерфейса (кроме loopback). Полная сетевая изоляция:

```bash
docker run --network none myapp
```

### overlay

Сеть, объединяющая контейнеры на разных Docker-хостах. Используется в Docker Swarm и Kubernetes для кластерных развёртываний.

### Полезные команды

```bash
docker network ls                  # Список сетей
docker network inspect my-network  # Информация о сети
docker network connect my-network container  # Подключить контейнер к сети
docker network disconnect my-network container
```

> **На собеседовании:** назовите четыре типа: bridge, host, none, overlay. Самый важный — bridge (используется по умолчанию). Покажите, что знаете разницу между дефолтной bridge-сетью и пользовательской: в пользовательской работает DNS по имени контейнера, а в дефолтной — нет. Это частая ловушка.

[к оглавлению](#docker)

---

## Как написать Dockerfile для Spring Boot приложения?
<!-- grade: middle -->

Dockerfile для Spring Boot приложения варьируется от простого (JAR уже собран) до production-ready (multi-stage build с health check и непривилегированным пользователем).

### Вариант 1: простой Dockerfile (JAR уже собран)

```dockerfile
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY target/myapp-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Вариант 2: production-ready multi-stage build с Maven

<details><summary>Полный Dockerfile</summary>

```dockerfile
# Стадия сборки
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app

# Кэширование зависимостей
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Сборка
COPY src ./src
RUN mvn package -DskipTests -B

# Стадия запуска
FROM eclipse-temurin:17-jre-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app

COPY --from=build /app/target/*.jar app.jar
RUN chown -R appuser:appgroup /app
USER appuser

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=40s \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["java", \
  "-XX:+UseContainerSupport", \
  "-XX:MaxRAMPercentage=75.0", \
  "-Djava.security.egd=file:/dev/./urandom", \
  "-jar", "app.jar"]
```

</details>

### Ключевые моменты

- Используйте `eclipse-temurin` (бывший AdoptOpenJDK) — это стабильные production-ready образы
- Для production берите `jre`, а не `jdk`
- Образы на базе `alpine` значительно компактнее
- Флаг `-XX:+UseContainerSupport` (включён по умолчанию с JDK 10+) позволяет JVM корректно определять лимиты CPU и памяти, установленные контейнером
- `-XX:MaxRAMPercentage=75.0` — JVM будет использовать не более 75% доступной памяти контейнера под heap
- `-Djava.security.egd=file:/dev/./urandom` — ускоряет запуск за счёт использования неблокирующего источника энтропии

> **На собеседовании:** будьте готовы написать Dockerfile для Spring Boot с нуля. Минимальный вариант — 5 строк (FROM, WORKDIR, COPY, EXPOSE, ENTRYPOINT). Для production добавьте: multi-stage build, непривилегированного пользователя, health check, JVM-флаги для контейнеров. Знание `-XX:MaxRAMPercentage` и `UseContainerSupport` показывает реальный опыт.

[к оглавлению](#docker)

---

## Что такое layered JAR в Spring Boot и как его использовать с Docker?
<!-- grade: senior -->

Layered JAR — это механизм Spring Boot (начиная с версии 2.3), разделяющий JAR-файл на логические слои для оптимизации кэширования Docker-образов при пересборке.

Обычный JAR при каждом изменении кода полностью пересобирается, и Docker не может использовать кэш для зависимостей. Layered JAR разделяет содержимое на слои:

1. `dependencies` — внешние зависимости (меняются редко)
2. `spring-boot-loader` — загрузчик Spring Boot (почти никогда не меняется)
3. `snapshot-dependencies` — SNAPSHOT-зависимости (меняются чаще)
4. `application` — код приложения (меняется при каждой сборке)

### Dockerfile с layered JAR

<details><summary>Пример кода</summary>

```dockerfile
# Стадия сборки
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn package -DskipTests -B

# Стадия извлечения слоёв
FROM eclipse-temurin:17-jre-alpine AS layers
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract

# Финальная стадия
FROM eclipse-temurin:17-jre-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app

# Копируем слои по отдельности (от редко меняющихся к часто меняющимся)
COPY --from=layers /app/dependencies/ ./
COPY --from=layers /app/spring-boot-loader/ ./
COPY --from=layers /app/snapshot-dependencies/ ./
COPY --from=layers /app/application/ ./

RUN chown -R appuser:appgroup /app
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

</details>

### Преимущество

При изменении только кода приложения Docker пересобирает лишь последний слой (`application`), а остальные три берутся из кэша. Это значительно ускоряет пересборку образа.

### Включение в pom.xml

Для Spring Boot 2.3+ (в Spring Boot 3.x включён по умолчанию):

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <layers>
            <enabled>true</enabled>
        </layers>
    </configuration>
</plugin>
```

> **На собеседовании:** layered JAR — это продвинутая оптимизация Docker-сборки для Spring Boot. Объясните проблему: обычный COPY jar пересобирает один большой слой при каждом изменении кода, включая неизменившиеся зависимости. Layered JAR решает это, разделяя JAR на 4 слоя по частоте изменений. Знание этого механизма показывает глубокое понимание Docker + Spring Boot.

[к оглавлению](#docker)

---

## Как передавать переменные окружения и конфигурацию в Docker-контейнер?
<!-- grade: junior -->

Переменные окружения — это основной механизм конфигурирования Docker-контейнеров, позволяющий менять поведение приложения без пересборки образа.

### 1. Через флаг -e при запуске

```bash
docker run -e SPRING_PROFILES_ACTIVE=prod \
           -e DB_PASSWORD=secret \
           my-spring-app
```

### 2. Через файл с переменными окружения

```bash
# .env файл
SPRING_PROFILES_ACTIVE=prod
DB_HOST=postgres
DB_PORT=5432
DB_PASSWORD=secret

docker run --env-file .env my-spring-app
```

### 3. Через ENV в Dockerfile (значения по умолчанию)

```dockerfile
ENV SPRING_PROFILES_ACTIVE=dev
ENV JAVA_OPTS="-Xmx512m"
```

### 4. Через ARG + ENV (параметризация при сборке)

```dockerfile
ARG APP_VERSION=1.0
ENV APP_VERSION=${APP_VERSION}
```

```bash
docker build --build-arg APP_VERSION=2.0 -t myapp .
```

### 5. Через Docker Compose

```yaml
services:
  app:
    environment:
      SPRING_PROFILES_ACTIVE: prod
    env_file:
      - .env
```

### 6. Монтирование файлов конфигурации

```bash
docker run -v ./application-prod.yml:/app/config/application.yml my-spring-app
```

### Как Spring Boot использует переменные окружения

Spring Boot автоматически маппит переменные окружения в свойства конфигурации через relaxed binding:
- `SPRING_DATASOURCE_URL` -> `spring.datasource.url`
- `SERVER_PORT` -> `server.port`

```bash
docker run -e SERVER_PORT=9090 \
           -e SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydb \
           -e SPRING_DATASOURCE_USERNAME=admin \
           -e SPRING_DATASOURCE_PASSWORD=secret \
           -p 9090:9090 \
           my-spring-app
```

### Безопасность секретов

Никогда не храните секреты (пароли, токены, ключи) в Dockerfile через ENV или ARG — они будут видны в истории образа (`docker history`). Используйте переменные окружения при запуске, Docker secrets или внешние системы управления секретами (Vault, AWS Secrets Manager).

> **На собеседовании:** перечислите способы от простого к сложному: -e флаг, --env-file, ENV в Dockerfile, Docker Compose environment. Обязательно упомяните relaxed binding в Spring Boot и проблему безопасности — секреты нельзя зашивать в Dockerfile.

[к оглавлению](#docker)

---

## Что такое health check в Docker?
<!-- grade: middle -->

Health check — это механизм Docker, позволяющий определять, работает ли приложение внутри контейнера корректно. По результатам проверки контейнер получает один из статусов: `healthy`, `unhealthy` или `starting`.

### Определение в Dockerfile

```dockerfile
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=60s \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/actuator/health || exit 1
```

### Параметры

| Параметр | Описание | По умолчанию |
|---|---|---|
| `--interval` | Как часто выполнять проверку | 30 с |
| `--timeout` | Максимальное время ожидания ответа | 30 с |
| `--retries` | Сколько неудачных проверок подряд для статуса unhealthy | 3 |
| `--start-period` | Начальный период, в течение которого неудачные проверки не учитываются | 0 с |

### Определение в docker-compose.yml

```yaml
services:
  app:
    build: .
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider",
             "http://localhost:8080/actuator/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 60s
```

### Использование с depends_on (Compose V2)

```yaml
services:
  app:
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16-alpine
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U admin -d mydb"]
      interval: 10s
      timeout: 5s
      retries: 5
```

### Spring Boot Actuator

Для Spring Boot рекомендуется использовать Spring Boot Actuator (`/actuator/health`):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: health
  endpoint:
    health:
      show-details: never
```

В Alpine-образах `curl` отсутствует по умолчанию, поэтому используйте `wget` (он есть в Alpine).

> **На собеседовании:** health check нужен для двух целей: (1) Docker/оркестратор знает, жив ли контейнер, (2) `depends_on` с `condition: service_healthy` позволяет ждать реальной готовности сервиса, а не просто запуска контейнера. Для Spring Boot — используйте Actuator endpoint. Не забудьте про start_period — без него health check будет считать контейнер unhealthy во время запуска приложения.

[к оглавлению](#docker)

---

## Как устроено логирование в Docker?
<!-- grade: middle -->

Docker перехватывает потоки stdout и stderr процесса внутри контейнера и направляет их в систему логирования через настраиваемый драйвер.

### Просмотр логов

```bash
docker logs myapp              # Все логи
docker logs -f myapp           # Следить в реальном времени (follow)
docker logs --tail 100 myapp   # Последние 100 строк
docker logs --since 2h myapp   # Логи за последние 2 часа
docker logs --timestamps myapp # С временными метками
```

### Драйверы логирования (logging drivers)

| Драйвер | Описание |
|---|---|
| `json-file` | По умолчанию. Хранит логи в JSON-файлах на хосте |
| `local` | Оптимизированный формат хранения |
| `syslog` | Отправка в syslog |
| `journald` | Отправка в systemd journal |
| `fluentd` | Отправка в Fluentd |
| `awslogs` | Отправка в Amazon CloudWatch |
| `gelf` | Отправка в Graylog (GELF формат) |

```bash
# Указать драйвер при запуске
docker run --log-driver=json-file \
           --log-opt max-size=10m \
           --log-opt max-file=3 \
           my-spring-app
```

### Настройка в docker-compose.yml

```yaml
services:
  app:
    image: my-spring-app
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
```

### Рекомендации для Java-приложений

- Направляйте логи в stdout/stderr вместо файлов. В Spring Boot для этого достаточно не настраивать file appender
- Используйте структурированное логирование (JSON) для удобства парсинга системами агрегации (ELK, Loki)
- Настройте ограничение размера логов (`max-size`, `max-file`), чтобы не переполнить диск
- В production используйте централизованные системы логирования (ELK Stack, Grafana Loki, Graylog)

<details><summary>Пример JSON-логирования в Spring Boot (logback-spring.xml)</summary>

```xml
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
    </appender>
    <root level="INFO">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

</details>

> **На собеседовании:** ключевой принцип: приложение в контейнере должно писать логи в stdout/stderr, а не в файлы. Docker сам собирает эти потоки и направляет в настроенный драйвер. Обязательно упомяните `max-size`/`max-file` — без них логи могут заполнить диск хоста. Знание ELK/Loki как production-решения — плюс.

[к оглавлению](#docker)

---

## Как Docker используется в CI/CD?
<!-- grade: middle -->

Docker в CI/CD обеспечивает воспроизводимость сборки, единообразие окружения и стандартизацию доставки артефактов через Docker-образы.

### 1. Единообразная среда сборки

Сборка проекта выполняется внутри контейнера, что гарантирует одинаковый результат на машине разработчика и CI-сервере:

```bash
docker run --rm -v $(pwd):/app -w /app maven:3.9-eclipse-temurin-17 mvn package
```

### 2. Сборка и публикация Docker-образа

```bash
# Сборка с тегированием
docker build -t registry.example.com/myapp:${GIT_COMMIT_SHA} .
docker build -t registry.example.com/myapp:latest .

# Отправка в реестр
docker push registry.example.com/myapp:${GIT_COMMIT_SHA}
docker push registry.example.com/myapp:latest
```

### 3. Пример GitLab CI/CD

<details><summary>Пример .gitlab-ci.yml</summary>

```yaml
stages:
  - test
  - build
  - deploy

test:
  stage: test
  image: maven:3.9-eclipse-temurin-17
  script:
    - mvn test
  cache:
    paths:
      - .m2/repository

build:
  stage: build
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

deploy:
  stage: deploy
  script:
    - docker pull $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker stop myapp || true
    - docker rm myapp || true
    - docker run -d --name myapp -p 8080:8080 $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```

</details>

### 4. Пример GitHub Actions

<details><summary>Пример workflow</summary>

```yaml
name: Build and Push
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: myuser/myapp:${{ github.sha }}
```

</details>

### 5. Тестирование с Testcontainers

Библиотека Testcontainers позволяет запускать Docker-контейнеры в интеграционных тестах:

<details><summary>Пример кода</summary>

```java
@SpringBootTest
@Testcontainers
class UserRepositoryTest {

    @Container
    static PostgreSQLContainer<?> postgres =
        new PostgreSQLContainer<>("postgres:16-alpine")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Test
    void shouldSaveUser() {
        // тест с реальной базой данных
    }
}
```

</details>

> **На собеседовании:** покажите, что Docker используется на всех этапах CI/CD: сборка в контейнере (единообразие), упаковка в образ (артефакт), тестирование с Testcontainers (интеграционные тесты с реальными БД), деплой образа. Знание Testcontainers — большой плюс для Java-разработчика.

[к оглавлению](#docker)

---

## Какие практики безопасности следует соблюдать при работе с Docker?
<!-- grade: senior -->

Безопасность Docker — это комплекс мер, направленных на минимизацию поверхности атаки, предотвращение утечки секретов и ограничение привилегий контейнеров.

### 1. Не запускайте контейнер от root

```dockerfile
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

### 2. Используйте минимальные базовые образы

- `alpine` — минимальный Linux (~5 МБ)
- `eclipse-temurin:17-jre-alpine` — вместо полного JDK
- `distroless` (от Google) — образ без shell и утилит, только среда выполнения:

```dockerfile
FROM gcr.io/distroless/java17-debian12
COPY app.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### 3. Не храните секреты в образе

```dockerfile
# ПЛОХО — секрет останется в истории слоёв
ENV DB_PASSWORD=supersecret
COPY .env /app/.env

# ХОРОШО — передавайте при запуске
# docker run -e DB_PASSWORD=supersecret myapp
```

### 4. Используйте конкретные теги образов

```dockerfile
# ПЛОХО — непредсказуемо
FROM eclipse-temurin:latest

# ХОРОШО — воспроизводимо
FROM eclipse-temurin:17.0.9_9-jre-alpine
```

### 5. Сканируйте образы на уязвимости

```bash
docker scout cves myapp:1.0
# Или с помощью Trivy
trivy image myapp:1.0
```

### 6. Используйте read-only файловую систему

```bash
docker run --read-only --tmpfs /tmp myapp
```

### 7. Ограничивайте ресурсы

```bash
docker run --memory=512m --cpus=1.0 myapp
```

### 8. Не устанавливайте лишнее в образ

```dockerfile
RUN apt-get update && \
    apt-get install -y --no-install-recommends необходимый-пакет && \
    rm -rf /var/lib/apt/lists/*
```

### 9. Дополнительные меры

- Используйте `.dockerignore` для исключения конфиденциальных файлов
- Обновляйте базовые образы регулярно, чтобы получать исправления безопасности
- Используйте multi-stage builds для исключения инструментов сборки из финального образа

> **На собеседовании:** назовите минимум 5 практик: непривилегированный пользователь, минимальные базовые образы, no secrets in image, конкретные теги, сканирование уязвимостей. Для senior-уровня упомяните distroless-образы, read-only filesystem, ограничение ресурсов и инструменты сканирования (Trivy, Docker Scout).

[к оглавлению](#docker)

---

## Как Docker взаимодействует с JVM? Какие есть нюансы?
<!-- grade: senior -->

Взаимодействие Docker с JVM — это набор нюансов, связанных с тем, как JVM определяет доступные ресурсы (память, CPU) в условиях ограничений cgroups контейнера.

### 1. Осведомлённость JVM о лимитах контейнера

До Java 10 JVM не знала о cgroups и определяла доступную память и CPU по хосту, а не по контейнеру. Это приводило к OOM-kill, когда JVM пыталась использовать больше памяти, чем разрешено контейнеру.

Начиная с Java 10+ (и бэкпорт в 8u191), флаг `-XX:+UseContainerSupport` включён по умолчанию, и JVM корректно определяет лимиты контейнера.

### 2. Настройка памяти

```dockerfile
# Рекомендуемый подход — процент от памяти контейнера
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75.0", "-jar", "app.jar"]
```

Почему 75%, а не 100%? JVM использует память не только для heap:
- Heap (основная память для объектов)
- Metaspace (метаданные классов)
- Thread stacks (стек каждого потока)
- Direct memory (NIO буферы)
- Code cache (JIT-скомпилированный код)
- GC overhead

### 3. Настройка CPU

JVM определяет количество доступных процессоров из cgroups. Это влияет на:
- Количество потоков GC
- Размер пула `ForkJoinPool.commonPool()`
- Количество потоков в `parallelStream()`

```bash
# Ограничение CPU
docker run --cpus=2.0 myapp
```

### 4. Graceful shutdown

Для корректного завершения Spring Boot приложения важно, чтобы Java-процесс получал сигнал SIGTERM:

```dockerfile
# ПРАВИЛЬНО: exec-форма — java получает PID 1
ENTRYPOINT ["java", "-jar", "app.jar"]

# НЕПРАВИЛЬНО: shell-форма — SIGTERM получает shell, не java
ENTRYPOINT java -jar app.jar
```

В Spring Boot включите graceful shutdown:

```yaml
# application.yml
server:
  shutdown: graceful
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
```

### 5. Время запуска

Для ускорения запуска в контейнере:
- Используйте `-Djava.security.egd=file:/dev/./urandom`
- Рассмотрите CDS (Class Data Sharing) или Spring AOT
- Используйте GraalVM Native Image для сокращения времени старта до миллисекунд

> **На собеседовании:** это вопрос senior-уровня. Три ключевых нюанса: (1) UseContainerSupport — JVM должна знать о лимитах контейнера (до Java 10 не знала), (2) MaxRAMPercentage=75% — heap это не вся память JVM, (3) exec-форма ENTRYPOINT для получения SIGTERM. Знание этих деталей показывает реальный production-опыт запуска Java в Docker.

[к оглавлению](#docker)

---

## Как организовать docker-compose для Java-приложения с базой данных?
<!-- grade: middle -->

Docker Compose для Java-приложения — это YAML-конфигурация, описывающая все сервисы (приложение, БД, кэш, reverse proxy), их зависимости, health check и ресурсные ограничения.

### Типичная конфигурация: Spring Boot + PostgreSQL + Redis + Nginx

<details><summary>Полный docker-compose.yml</summary>

```yaml
version: '3.8'

services:
  # Nginx как reverse proxy
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      app:
        condition: service_healthy
    restart: unless-stopped

  # Spring Boot приложение
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
      - "5005:5005"  # debug порт (только для разработки)
    environment:
      SPRING_PROFILES_ACTIVE: docker
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/mydb
      SPRING_DATASOURCE_USERNAME: admin
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD:-secret}
      SPRING_DATA_REDIS_HOST: redis
      JAVA_OPTS: "-Xmx512m -XX:MaxRAMPercentage=75.0"
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider",
             "http://localhost:8080/actuator/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 60s
    restart: unless-stopped
    deploy:
      resources:
        limits:
          memory: 768M
          cpus: '1.0'

  # PostgreSQL
  db:
    image: postgres:16-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: ${DB_PASSWORD:-secret}
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./sql/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U admin -d mydb"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  # Redis для кэширования
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    command: redis-server --maxmemory 128mb --maxmemory-policy allkeys-lru
    volumes:
      - redis-data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

volumes:
  postgres-data:
  redis-data:
```

</details>

### Файл .env (не коммитится в Git)

```
DB_PASSWORD=my_secure_password
```

### Команды для работы

```bash
# Запуск всей инфраструктуры
docker compose up -d

# Пересборка только приложения
docker compose up -d --build app

# Просмотр логов приложения
docker compose logs -f app

# Остановка с удалением данных
docker compose down -v
```

> **На собеседовании:** покажите, что умеете описать реалистичный стек. Ключевые моменты: health check для всех сервисов, depends_on с condition: service_healthy, вынос секретов в .env файл, именованные тома для данных БД, ограничение ресурсов для JVM-приложения. Это типовая задача на техническом собеседовании для middle/senior.

[к оглавлению](#docker)

---

## Что такое Docker Registry и Docker Hub?
<!-- grade: junior -->

Docker Registry — это сервер для хранения и распространения Docker-образов, реализующий Docker Registry HTTP API для загрузки (push) и скачивания (pull) образов.

### Типы реестров

- Публичные — доступны всем. Главный пример — Docker Hub
- Приватные — доступны только внутри организации. Примеры: GitLab Container Registry, GitHub Container Registry, Amazon ECR, Google Artifact Registry, Azure Container Registry
- Self-hosted — собственный реестр:

```bash
docker run -d -p 5000:5000 --name registry registry:2
docker push localhost:5000/myapp:1.0
```

### Docker Hub

Крупнейший публичный реестр Docker-образов (hub.docker.com). Содержит:
- Официальные образы — проверенные и поддерживаемые командой Docker или авторами проектов: `openjdk`, `eclipse-temurin`, `postgres`, `redis`, `nginx`, `maven`, `gradle`
- Верифицированные образы издателей — от проверенных компаний (Bitnami, Confluent и др.)
- Пользовательские образы — загруженные сообществом

### Работа с Docker Hub

```bash
# Авторизация
docker login

# Скачать образ
docker pull eclipse-temurin:17-jre-alpine

# Тегировать для отправки
docker tag myapp:1.0 myuser/myapp:1.0

# Отправить
docker push myuser/myapp:1.0
```

### Именование образов

```
[registry/]namespace/repository:tag

docker.io/library/postgres:16-alpine    # Полное имя официального образа
postgres:16-alpine                       # Сокращённая форма
mycompany.com/backend/myapp:1.0         # Приватный реестр
ghcr.io/myorg/myapp:latest              # GitHub Container Registry
```

> **На собеседовании:** Registry — это хранилище образов (аналог Maven Central для JAR). Docker Hub — главный публичный реестр. Знайте формат именования: `registry/namespace/repo:tag`. Упомяните, что в production обычно используют приватные реестры (GitLab CR, Amazon ECR) для контроля доступа и безопасности.

[к оглавлению](#docker)

---

## Чем отличается контейнер от образа?
<!-- grade: junior -->

Образ — это неизменяемый шаблон (набор read-only слоёв), а контейнер — запущенный экземпляр этого образа с добавленным записываемым слоем и собственным состоянием.

> Аналогия для Java-разработчиков: образ — это класс, контейнер — это объект (экземпляр класса). Из одного класса можно создать множество объектов, из одного образа — множество контейнеров.

| Характеристика | Образ (Image) | Контейнер (Container) |
|---|---|---|
| Аналогия в Java | Класс | Объект (экземпляр класса) |
| Изменяемость | Неизменяемый (immutable) | Изменяемый (имеет writable layer) |
| Хранение | На диске в реестре или локально | Запущенный или остановленный процесс |
| Создание | `docker build` / `docker pull` | `docker run` / `docker create` |
| Количество | Один образ | Множество контейнеров из одного образа |
| Состояние | Нет (шаблон) | Есть (running, stopped, paused) |

Образ — это набор неизменяемых слоёв (read-only layers), представляющих файловую систему, метаданные и инструкции для запуска. Образ не потребляет CPU и не имеет состояния.

Контейнер — это запущенный экземпляр образа. При создании контейнера Docker добавляет тонкий записываемый слой (writable/container layer) поверх слоёв образа. Все изменения в файловой системе (создание, изменение, удаление файлов) записываются в этот слой.

```
Контейнер = Образ (read-only) + Writable Layer + Конфигурация (сети, тома, переменные)
```

Из одного образа можно запустить множество контейнеров, и каждый будет иметь свой независимый writable layer:

```bash
docker run -d --name app1 my-spring-app
docker run -d --name app2 my-spring-app
docker run -d --name app3 my-spring-app
# Три независимых контейнера из одного образа
```

При удалении контейнера (`docker rm`) writable layer удаляется. Данные, которые нужно сохранить, следует хранить в томах (volumes).

> **На собеседовании:** используйте аналогию класс/объект — она понятна любому Java-разработчику. Ключевое: образ неизменяемый (read-only слои), контейнер добавляет writable layer. При удалении контейнера writable layer теряется, поэтому для персистентных данных нужны volumes.

[к оглавлению](#docker)
