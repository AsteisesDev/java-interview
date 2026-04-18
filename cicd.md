[Вопросы для собеседования](README.md)

# CI/CD
+ [Что такое CI (Continuous Integration)?](#Что-такое-CI-Continuous-Integration)
+ [Что такое CD (Continuous Delivery и Continuous Deployment)?](#Что-такое-CD-Continuous-Delivery-и-Continuous-Deployment)
+ [Зачем нужен CI/CD?](#Зачем-нужен-CICD)
+ [Каковы основные этапы CI/CD пайплайна?](#Каковы-основные-этапы-CICD-пайплайна)
+ [Какие best practices CI/CD вы знаете?](#Какие-best-practices-CICD-вы-знаете)
+ [Какие существуют стратегии деплоя?](#Какие-существуют-стратегии-деплоя)
+ [Что такое Jenkins?](#Что-такое-Jenkins)
+ [Какова архитектура Jenkins (master/agent)?](#Какова-архитектура-Jenkins-masteragent)
+ [Что такое Jenkinsfile? Чем отличается Declarative Pipeline от Scripted Pipeline?](#Что-такое-Jenkinsfile-Чем-отличается-Declarative-Pipeline-от-Scripted-Pipeline)
+ [Какие основные секции Jenkinsfile вы знаете?](#Какие-основные-секции-Jenkinsfile-вы-знаете)
+ [Как использовать параметры, условия и параллельные стадии в Jenkins Pipeline?](#Как-использовать-параметры-условия-и-параллельные-стадии-в-Jenkins-Pipeline)
+ [Какие плагины Jenkins наиболее часто используются?](#Какие-плагины-Jenkins-наиболее-часто-используются)
+ [Что такое Shared Libraries в Jenkins?](#Что-такое-Shared-Libraries-в-Jenkins)
+ [Как управлять Credentials в Jenkins?](#Как-управлять-Credentials-в-Jenkins)
+ [Какие триггеры сборки существуют в Jenkins?](#Какие-триггеры-сборки-существуют-в-Jenkins)
+ [Как Jenkins взаимодействует с Docker?](#Как-Jenkins-взаимодействует-с-Docker)
+ [Что такое Multibranch Pipeline?](#Что-такое-Multibranch-Pipeline)
+ [Что такое Nexus Repository Manager?](#Что-такое-Nexus-Repository-Manager)
+ [Какие типы репозиториев существуют в Nexus?](#Какие-типы-репозиториев-существуют-в-Nexus)
+ [Зачем использовать Nexus в проекте?](#Зачем-использовать-Nexus-в-проекте)
+ [Как настроить Maven для работы с Nexus?](#Как-настроить-Maven-для-работы-с-Nexus)
+ [Как настроить Gradle для работы с Nexus?](#Как-настроить-Gradle-для-работы-с-Nexus)
+ [Как использовать Nexus в качестве Docker Registry?](#Как-использовать-Nexus-в-качестве-Docker-Registry)
+ [Как организовать деплой артефактов из Jenkins в Nexus?](#Как-организовать-деплой-артефактов-из-Jenkins-в-Nexus)
+ [Что такое Git Flow, GitHub Flow и Trunk-based development?](#Что-такое-Git-Flow-GitHub-Flow-и-Trunk-based-development)
+ [Merge vs Rebase в контексте CI — что предпочтительнее?](#Merge-vs-Rebase-в-контексте-CI--что-предпочтительнее)
+ [Как организовать CI/CD для Java-микросервисов в банковской среде?](#Как-организовать-CICD-для-Java-микросервисов-в-банковской-среде)
+ [Как обеспечить безопасность CI/CD пайплайна?](#Как-обеспечить-безопасность-CICD-пайплайна)

## Что такое CI (Continuous Integration)?

**Continuous Integration (непрерывная интеграция)** — это практика разработки программного обеспечения, при которой разработчики регулярно (несколько раз в день) интегрируют свои изменения кода в общий репозиторий, после чего автоматически выполняются сборка и тестирование.

Основные принципы CI:
- **Единый репозиторий исходного кода** — весь код хранится в системе контроля версий (Git).
- **Автоматизированная сборка** — каждый коммит запускает процесс сборки проекта.
- **Автоматизированное тестирование** — после сборки автоматически выполняются unit-тесты, интеграционные тесты и т.д.
- **Быстрая обратная связь** — разработчик узнает о проблемах в течение нескольких минут после коммита.
- **Частая интеграция** — чем чаще разработчики интегрируют код, тем меньше конфликтов и проблем возникает.

Типичный CI-цикл:
1. Разработчик делает `git push` в удаленный репозиторий.
2. CI-сервер (например, Jenkins) обнаруживает изменения.
3. Запускается сборка проекта (`mvn clean compile` или `gradle build`).
4. Выполняются автоматические тесты.
5. Генерируются отчеты о результатах.
6. Разработчик получает уведомление об успехе или ошибке.

[к оглавлению](#CICD)

## Что такое CD (Continuous Delivery и Continuous Deployment)?

Аббревиатура CD имеет два значения, которые важно различать:

**Continuous Delivery (непрерывная доставка)** — практика, при которой код всегда находится в состоянии, готовом к развертыванию в продуктивную среду. Развертывание на production выполняется вручную по решению команды (нажатие кнопки).

**Continuous Deployment (непрерывное развертывание)** — расширение Continuous Delivery, при котором каждое изменение, прошедшее все этапы пайплайна (сборка, тесты, проверки), автоматически разворачивается на production без ручного вмешательства.

Ключевые различия:

| Критерий | Continuous Delivery | Continuous Deployment |
|---|---|---|
| Деплой на production | Ручной (по кнопке) | Автоматический |
| Требования к тестам | Высокие | Очень высокие |
| Применимость | Банки, enterprise | Стартапы, SaaS |
| Контроль | Больше контроля | Максимальная скорость |

В банковской среде, как правило, используется **Continuous Delivery**, так как развертывание на production требует согласования, проверки compliance и ручного подтверждения.

[к оглавлению](#CICD)

## Зачем нужен CI/CD?

CI/CD решает ряд критических проблем в разработке ПО:

**Проблемы без CI/CD:**
- «Интеграционный ад» — конфликты при слиянии больших веток кода.
- «На моей машине работает» — отсутствие единой среды сборки.
- Ручные ошибки при сборке и развертывании.
- Долгий цикл обратной связи — баги обнаруживаются поздно.
- Непредсказуемые и рискованные релизы.

**Преимущества CI/CD:**
- **Раннее обнаружение ошибок** — баги находятся в течение минут после коммита, а не через недели.
- **Уменьшение рисков** — маленькие и частые изменения менее рискованны, чем большие и редкие.
- **Ускорение доставки** — новая функциональность попадает к пользователям быстрее.
- **Повторяемость** — автоматизированный процесс всегда выполняется одинаково.
- **Прозрачность** — каждый член команды видит состояние сборки и тестов.
- **Единый источник правды** — пайплайн описан как код (Jenkinsfile), хранится в Git.
- **Снижение стоимости исправлений** — чем раньше найдена ошибка, тем дешевле её исправить.

[к оглавлению](#CICD)

## Каковы основные этапы CI/CD пайплайна?

Типичный CI/CD пайплайн для Java-проекта состоит из следующих этапов:

**1. Checkout (получение кода)**
- Клонирование репозитория или получение изменений из VCS.

**2. Build (сборка)**
- Компиляция исходного кода (`mvn compile`, `gradle compileJava`).
- Разрешение зависимостей (скачивание из Nexus/Maven Central).

**3. Test (тестирование)**
- Unit-тесты (`mvn test`).
- Интеграционные тесты (`mvn verify`).
- Иногда — тесты производительности, нагрузочные тесты.

**4. Code Quality (проверка качества кода)**
- Статический анализ кода (SonarQube, Checkstyle, SpotBugs).
- Проверка покрытия тестами (JaCoCo).
- Проверка безопасности зависимостей (OWASP Dependency Check).

**5. Package (упаковка)**
- Создание артефакта: JAR, WAR, Docker-образ.
- Версионирование артефакта.

**6. Publish (публикация)**
- Загрузка артефакта в Nexus Repository.
- Публикация Docker-образа в Docker Registry.

**7. Deploy (развертывание)**
- Развертывание на DEV/TEST/STAGING окружение.
- Для production — ручное подтверждение (в Continuous Delivery).

**8. Smoke/Acceptance Tests (приемочные тесты)**
- Проверка работоспособности приложения после деплоя.
- Автоматические end-to-end тесты.

Пример схематичного Jenkinsfile с этими этапами:

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout')  { steps { checkout scm } }
        stage('Build')     { steps { sh 'mvn clean compile' } }
        stage('Test')      { steps { sh 'mvn test' } }
        stage('Quality')   { steps { sh 'mvn sonar:sonar' } }
        stage('Package')   { steps { sh 'mvn package -DskipTests' } }
        stage('Publish')   { steps { sh 'mvn deploy -DskipTests' } }
        stage('Deploy')    { steps { sh './deploy.sh staging' } }
    }
}
```

[к оглавлению](#CICD)

## Какие best practices CI/CD вы знаете?

**Практики для CI:**
1. **Коммитьте часто** — маленькие коммиты проще ревьюить и отлаживать.
2. **Не коммитьте сломанный код** — перед push запускайте тесты локально.
3. **Чините сборку немедленно** — сломанная сборка блокирует всю команду.
4. **Пишите автотесты** — без тестов CI теряет смысл.
5. **Быстрая сборка** — CI-пайплайн должен выполняться быстро (до 10-15 минут).
6. **Сборка один раз** — собранный артефакт переиспользуется на всех окружениях, не пересобирается.

**Практики для CD:**
1. **Infrastructure as Code** — инфраструктура описана в коде (Terraform, Ansible).
2. **Pipeline as Code** — пайплайн описан в Jenkinsfile и хранится в репозитории проекта.
3. **Идентичные окружения** — DEV, STAGING и PRODUCTION максимально похожи.
4. **Feature flags** — новая функциональность скрыта за переключателями.
5. **Мониторинг и алертинг** — автоматическое обнаружение проблем после деплоя.
6. **Rollback** — возможность быстро откатиться на предыдущую версию.
7. **Секреты не в коде** — пароли и ключи хранятся в Jenkins Credentials, HashiCorp Vault и т.д.
8. **Версионирование артефактов** — каждая сборка создает уникальную версию артефакта.

[к оглавлению](#CICD)

## Какие существуют стратегии деплоя?

Основные стратегии развертывания:

**1. Recreate (пересоздание)**
- Старая версия останавливается, затем запускается новая.
- Плюсы: простота.
- Минусы: даунтайм во время переключения.

**2. Rolling Update (поэтапное обновление)**
- Экземпляры приложения обновляются по одному (или группами).
- В каждый момент времени часть инстансов работает на старой версии, часть — на новой.
- Плюсы: нет даунтайма, экономия ресурсов.
- Минусы: временная несовместимость версий, сложный откат.

**3. Blue-Green Deployment**
- Существуют два идентичных окружения: Blue (текущая production-версия) и Green (новая версия).
- Трафик переключается с Blue на Green после проверки.
- Плюсы: мгновенный откат (переключение обратно на Blue), нет даунтайма.
- Минусы: требуется вдвое больше ресурсов.

**4. Canary Deployment (канареечный деплой)**
- Новая версия развертывается на небольшую часть серверов (например, 5% трафика).
- Мониторятся метрики (ошибки, латенси). Если все хорошо — трафик постепенно переводится на новую версию.
- Плюсы: минимальный риск, раннее обнаружение проблем.
- Минусы: сложность реализации, необходимость продвинутого мониторинга.

**5. A/B Testing**
- Разные версии для разных групп пользователей.
- Используется для проверки бизнес-гипотез.

В банковской среде чаще всего используются **Blue-Green** и **Canary** стратегии, так как они обеспечивают минимальный риск и возможность быстрого отката.

[к оглавлению](#CICD)

## Что такое Jenkins?

**Jenkins** — это открытый (open-source) сервер автоматизации, написанный на Java. Является одним из наиболее популярных инструментов для реализации CI/CD.

Основные характеристики Jenkins:
- **Бесплатный и open-source** — лицензия MIT.
- **Кроссплатформенный** — работает на Windows, Linux, macOS (написан на Java).
- **Расширяемый** — более 1800 плагинов для интеграции с различными инструментами.
- **Pipeline as Code** — пайплайны описываются в Jenkinsfile.
- **Распределенная сборка** — архитектура master/agent позволяет масштабировать нагрузку.
- **Веб-интерфейс** — управление и мониторинг через браузер.
- **REST API** — программный доступ к функциям Jenkins.

Jenkins широко используется в банковском секторе благодаря:
- Возможности работы в закрытых контурах (on-premise) без доступа в интернет.
- Гибкой системе прав доступа (RBAC через плагины).
- Интеграции с корпоративными LDAP/Active Directory.
- Поддержке audit trail — журналирование всех действий.

[к оглавлению](#CICD)

## Какова архитектура Jenkins (master/agent)?

Jenkins использует распределенную архитектуру **master/agent** (ранее называвшуюся master/slave):

**Jenkins Master (Controller):**
- Основной сервер Jenkins.
- Управляет конфигурацией, планированием и распределением задач.
- Хранит настройки, историю сборок, результаты.
- Предоставляет веб-интерфейс и REST API.
- Может выполнять сборки самостоятельно (но не рекомендуется в production).

**Jenkins Agent (Node):**
- Рабочая машина, на которой выполняются сборки.
- Подключается к master и получает задания.
- Может иметь специфичные инструменты (JDK, Maven, Docker и т.д.).
- Помечается метками (labels) для маршрутизации задач.

**Способы подключения агентов:**
- **SSH** — master подключается к агенту по SSH.
- **JNLP (Java Web Start)** — агент сам подключается к master.
- **Docker** — агент запускается как Docker-контейнер на время сборки.
- **Kubernetes** — агенты создаются как Pod'ы в кластере Kubernetes.

**Пример конфигурации агента в Jenkinsfile:**

```groovy
pipeline {
    agent {
        label 'java17-maven'  // Сборка выполняется на агенте с данной меткой
    }
    stages {
        stage('Build') {
            steps {
                sh 'java -version'
                sh 'mvn --version'
            }
        }
    }
}
```

**Пример с Docker-агентом:**

```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.9-eclipse-temurin-17'
            args '-v /root/.m2:/root/.m2'  // Кэширование зависимостей Maven
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

[к оглавлению](#CICD)

## Что такое Jenkinsfile? Чем отличается Declarative Pipeline от Scripted Pipeline?

**Jenkinsfile** — это текстовый файл, содержащий описание CI/CD пайплайна на Groovy DSL. Хранится в корне репозитория проекта вместе с исходным кодом, что реализует подход **Pipeline as Code**.

Существуют два синтаксиса:

**1. Declarative Pipeline** (рекомендуемый):

```groovy
pipeline {
    agent any

    environment {
        MAVEN_OPTS = '-Xmx1024m'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            mail to: 'team@company.com',
                 subject: "FAILED: ${env.JOB_NAME}",
                 body: "Build ${env.BUILD_NUMBER} failed."
        }
    }
}
```

**2. Scripted Pipeline** (полноценный Groovy):

```groovy
node('java17-maven') {
    try {
        stage('Build') {
            checkout scm
            sh 'mvn clean compile'
        }
        stage('Test') {
            sh 'mvn test'
        }
        stage('Package') {
            sh 'mvn package -DskipTests'
        }
    } catch (e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        junit '**/target/surefire-reports/*.xml'
    }
}
```

**Сравнение:**

| Критерий | Declarative | Scripted |
|---|---|---|
| Синтаксис | Структурированный, строгий | Свободный Groovy |
| Простота | Проще для новичков | Сложнее |
| Гибкость | Ограниченная (но есть `script {}`) | Полная |
| Валидация | Валидируется до запуска | Ошибки в runtime |
| Секция `post` | Встроенная | Через `try/catch/finally` |
| Рекомендация | Рекомендуется Jenkins | Для сложных случаев |

В банковской практике обычно используется **Declarative Pipeline** как более читаемый и поддерживаемый вариант.

[к оглавлению](#CICD)

## Какие основные секции Jenkinsfile вы знаете?

Declarative Pipeline состоит из следующих секций:

**`pipeline {}`** — корневой блок, обязательный.

**`agent`** — определяет, где выполняется пайплайн:
```groovy
agent any                          // Любой доступный агент
agent none                         // Агент указывается на уровне stage
agent { label 'linux' }            // Агент с меткой
agent { docker { image 'maven:3.9' } }  // Docker-контейнер
```

**`stages {}`** — содержит упорядоченный список стадий:
```groovy
stages {
    stage('Build')   { steps { sh 'mvn compile' } }
    stage('Test')    { steps { sh 'mvn test' } }
    stage('Deploy')  { steps { sh './deploy.sh' } }
}
```

**`steps {}`** — конкретные шаги внутри стадии:
```groovy
steps {
    sh 'mvn clean package'           // Выполнение shell-команды
    echo 'Hello!'                     // Вывод сообщения
    archiveArtifacts '**/*.jar'       // Архивация артефактов
    junit '**/surefire-reports/*.xml'  // Публикация результатов тестов
}
```

**`environment {}`** — переменные окружения:
```groovy
environment {
    APP_VERSION = '1.0.0'
    NEXUS_URL = credentials('nexus-url')  // Из Jenkins Credentials
}
```

**`tools {}`** — инструменты сборки (должны быть настроены в Jenkins):
```groovy
tools {
    maven 'Maven-3.9'
    jdk 'JDK-17'
}
```

**`options {}`** — параметры пайплайна:
```groovy
options {
    timeout(time: 30, unit: 'MINUTES')
    timestamps()
    disableConcurrentBuilds()
    buildDiscarder(logRotator(numToKeepStr: '10'))
}
```

**`triggers {}`** — триггеры автоматического запуска:
```groovy
triggers {
    cron('H/15 * * * *')         // Каждые 15 минут
    pollSCM('H/5 * * * *')       // Проверка VCS каждые 5 минут
}
```

**`post {}`** — действия после выполнения пайплайна:
```groovy
post {
    always  { junit '**/surefire-reports/*.xml' }
    success { echo 'Build succeeded' }
    failure { mail to: 'dev@company.com', subject: 'Build failed', body: '...' }
    cleanup { cleanWs() }
}
```

[к оглавлению](#CICD)

## Как использовать параметры, условия и параллельные стадии в Jenkins Pipeline?

**Параметры (`parameters`)** позволяют задавать входные данные при запуске сборки:

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Target environment')
        booleanParam(name: 'SKIP_TESTS', defaultValue: false, description: 'Skip tests')
        password(name: 'DEPLOY_TOKEN', description: 'Deployment token')
    }
    stages {
        stage('Build') {
            steps {
                echo "Building branch: ${params.BRANCH}"
                sh "mvn clean package ${params.SKIP_TESTS ? '-DskipTests' : ''}"
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to: ${params.ENVIRONMENT}"
            }
        }
    }
}
```

**Условия (`when`)** определяют, при каких условиях выполняется стадия:

```groovy
stages {
    stage('Deploy to Staging') {
        when {
            branch 'develop'  // Только для ветки develop
        }
        steps { sh './deploy.sh staging' }
    }
    stage('Deploy to Production') {
        when {
            allOf {
                branch 'main'                         // Ветка main
                expression { params.ENVIRONMENT == 'production' }  // Параметр
            }
        }
        steps {
            input message: 'Deploy to production?', ok: 'Deploy'  // Ручное подтверждение
            sh './deploy.sh production'
        }
    }
    stage('SonarQube') {
        when {
            not { changeRequest() }  // Не для pull request
        }
        steps { sh 'mvn sonar:sonar' }
    }
}
```

**Параллельные стадии (`parallel`)** ускоряют выполнение пайплайна:

```groovy
stage('Tests') {
    parallel {
        stage('Unit Tests') {
            steps { sh 'mvn test -pl unit-tests' }
        }
        stage('Integration Tests') {
            agent { label 'docker' }
            steps { sh 'mvn verify -pl integration-tests' }
        }
        stage('Security Scan') {
            steps { sh 'mvn dependency-check:check' }
        }
    }
}
```

[к оглавлению](#CICD)

## Какие плагины Jenkins наиболее часто используются?

Наиболее востребованные плагины для Java-разработки:

**Сборка и управление:**
- **Pipeline** — поддержка декларативных и скриптовых пайплайнов.
- **Pipeline: Stage View** — визуализация стадий пайплайна.
- **Blue Ocean** — современный UI для визуализации и редактирования пайплайнов.
- **Multibranch Pipeline** — автоматическое создание пайплайнов для каждой ветки.
- **Job DSL** — описание джобов как кода на Groovy DSL.

**Контроль версий:**
- **Git** — интеграция с Git.
- **GitHub / GitLab / Bitbucket** — интеграция с платформами хостинга кода.
- **Generic Webhook Trigger** — обработка вебхуков от различных систем.

**Сборка Java:**
- **Maven Integration** — поддержка Maven.
- **Gradle** — поддержка Gradle.
- **Config File Provider** — управление конфигурационными файлами (например, `settings.xml`).

**Тестирование и качество:**
- **JUnit** — публикация результатов JUnit-тестов.
- **JaCoCo** — отчеты о покрытии кода.
- **SonarQube Scanner** — интеграция со статическим анализом.

**Артефакты и деплой:**
- **Nexus Artifact Uploader** — загрузка артефактов в Nexus.
- **Docker Pipeline** — работа с Docker из пайплайна.
- **SSH Agent** — работа с SSH-ключами.
- **Publish Over SSH** — деплой файлов на удаленные серверы по SSH.

**Безопасность и управление:**
- **Credentials Binding** — использование секретов в пайплайнах.
- **Role-based Authorization Strategy** — управление правами на основе ролей.
- **LDAP** — аутентификация через корпоративный LDAP.
- **Audit Trail** — журналирование действий пользователей.

**Уведомления:**
- **Email Extension** — расширенные email-уведомления.
- **Slack Notification** — уведомления в Slack.
- **Telegram Bot** — уведомления в Telegram.

[к оглавлению](#CICD)

## Что такое Shared Libraries в Jenkins?

**Shared Libraries** — это механизм Jenkins, позволяющий выносить общую логику пайплайнов в отдельный Git-репозиторий и переиспользовать её в нескольких проектах.

**Структура Shared Library:**

```
shared-library-repo/
├── vars/                     # Глобальные переменные и функции
│   ├── buildJavaApp.groovy   # Вызывается как buildJavaApp() в Jenkinsfile
│   ├── deployToNexus.groovy
│   └── notifyTeam.groovy
├── src/                      # Groovy-классы (полноценный ООП)
│   └── com/
│       └── company/
│           └── Pipeline.groovy
└── resources/                # Ресурсы (конфиги, шаблоны)
    └── settings.xml
```

**Пример файла `vars/buildJavaApp.groovy`:**

```groovy
def call(Map config = [:]) {
    def mavenVersion = config.mavenVersion ?: 'Maven-3.9'
    def jdkVersion = config.jdkVersion ?: 'JDK-17'

    pipeline {
        agent { label 'java' }
        tools {
            maven mavenVersion
            jdk jdkVersion
        }
        stages {
            stage('Build') {
                steps { sh 'mvn clean compile' }
            }
            stage('Test') {
                steps { sh 'mvn test' }
            }
            stage('Package') {
                steps { sh 'mvn package -DskipTests' }
            }
        }
    }
}
```

**Использование в Jenkinsfile проекта:**

```groovy
@Library('my-shared-library') _

buildJavaApp(mavenVersion: 'Maven-3.9', jdkVersion: 'JDK-17')
```

**Или отдельные функции:**

```groovy
@Library('my-shared-library') _

pipeline {
    agent any
    stages {
        stage('Build') {
            steps { sh 'mvn clean package' }
        }
        stage('Publish') {
            steps { deployToNexus(artifactId: 'my-app', version: '1.0.0') }
        }
    }
    post {
        failure { notifyTeam(channel: '#builds', status: 'FAILED') }
    }
}
```

Shared Libraries особенно полезны в банковской среде, где десятки микросервисов используют одинаковый CI/CD процесс — можно стандартизировать пайплайны и централизованно управлять ими.

[к оглавлению](#CICD)

## Как управлять Credentials в Jenkins?

**Credentials (учетные данные)** — механизм безопасного хранения секретов в Jenkins (пароли, SSH-ключи, токены, сертификаты). Секреты хранятся в зашифрованном виде и никогда не выводятся в логах (маскируются как `****`).

**Типы Credentials:**
- **Username with password** — логин и пароль (например, для Nexus).
- **Secret text** — одиночный секретный текст (например, API-токен).
- **Secret file** — секретный файл (например, сертификат, `settings.xml`).
- **SSH Username with private key** — SSH-ключ для подключения к серверам.
- **Certificate** — PKCS#12 сертификат.

**Области видимости (Scope):**
- **Global** — доступен во всех проектах Jenkins.
- **System** — доступен только на уровне системы Jenkins (для подключения агентов и т.д.).
- **Folder** — доступен внутри определенной папки проектов.

**Использование в Declarative Pipeline:**

```groovy
pipeline {
    agent any
    environment {
        // Username with password — создает две переменные: NEXUS_CREDS_USR и NEXUS_CREDS_PSW
        NEXUS_CREDS = credentials('nexus-credentials')
        // Secret text
        SONAR_TOKEN = credentials('sonarqube-token')
    }
    stages {
        stage('Deploy to Nexus') {
            steps {
                sh """
                    mvn deploy \
                        -DaltDeploymentRepository=nexus::default::https://nexus.company.com/repository/maven-releases \
                        -Dusername=${NEXUS_CREDS_USR} \
                        -Dpassword=${NEXUS_CREDS_PSW}
                """
            }
        }
    }
}
```

**Использование через `withCredentials`:**

```groovy
stage('Deploy') {
    steps {
        withCredentials([
            usernamePassword(credentialsId: 'nexus-credentials',
                           usernameVariable: 'NEXUS_USER',
                           passwordVariable: 'NEXUS_PASS'),
            sshUserPrivateKey(credentialsId: 'deploy-ssh-key',
                            keyFileVariable: 'SSH_KEY')
        ]) {
            sh 'curl -u ${NEXUS_USER}:${NEXUS_PASS} --upload-file app.jar ${NEXUS_URL}'
            sh 'ssh -i ${SSH_KEY} user@server "systemctl restart app"'
        }
    }
}
```

**Использование `configFileProvider` для `settings.xml`:**

```groovy
stage('Build') {
    steps {
        configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
            sh 'mvn -s $MAVEN_SETTINGS clean deploy'
        }
    }
}
```

[к оглавлению](#CICD)

## Какие триггеры сборки существуют в Jenkins?

Jenkins поддерживает несколько механизмов автоматического запуска сборок:

**1. Webhook (рекомендуемый способ)**
- Git-сервер (GitHub, GitLab, Bitbucket) отправляет HTTP-запрос в Jenkins при push/merge.
- Мгновенная реакция на изменения.
- Не создает нагрузку на Jenkins.

Настройка в Jenkinsfile:
```groovy
// Для GitHub:
triggers {
    githubPush()
}
// Для GitLab:
triggers {
    gitlab(triggerOnPush: true, triggerOnMergeRequest: true)
}
```

**2. SCM Polling (опрос системы контроля версий)**
- Jenkins периодически проверяет репозиторий на наличие изменений.
- Используется, когда webhook невозможен (закрытый контур, ограничения сети).

```groovy
triggers {
    pollSCM('H/5 * * * *')  // Проверка каждые 5 минут
}
```

**3. Cron (по расписанию)**
- Сборка запускается по расписанию независимо от изменений в коде.
- Используется для ночных сборок, регулярных проверок безопасности.

```groovy
triggers {
    cron('H 2 * * 1-5')  // В 2 часа ночи по будням
}
```

Синтаксис: `MINUTE HOUR DOM MONTH DOW` (как в Unix cron). Символ `H` — хеш, распределяющий нагрузку.

**4. Upstream (по завершению другой сборки)**
- Сборка запускается после успешного завершения другого проекта.

```groovy
triggers {
    upstream(upstreamProjects: 'core-library/main', threshold: hudson.model.Result.SUCCESS)
}
```

**5. Ручной запуск**
- Через веб-интерфейс Jenkins (кнопка «Build Now» / «Build with Parameters»).
- Через REST API: `curl -X POST https://jenkins.company.com/job/my-job/build`.

В банковской практике чаще всего используется комбинация: **webhook** для веток разработки и **ручной запуск с параметрами** для деплоя на production.

[к оглавлению](#CICD)

## Как Jenkins взаимодействует с Docker?

Jenkins можно интегрировать с Docker на нескольких уровнях:

**1. Docker как агент сборки** — каждая сборка выполняется в чистом контейнере:

```groovy
pipeline {
    agent {
        docker {
            image 'maven:3.9-eclipse-temurin-17'
            args '-v $HOME/.m2:/root/.m2 --network=host'
        }
    }
    stages {
        stage('Build') {
            steps { sh 'mvn clean package' }
        }
    }
}
```

**2. Разные Docker-образы для разных стадий:**

```groovy
pipeline {
    agent none
    stages {
        stage('Build') {
            agent { docker { image 'maven:3.9-eclipse-temurin-17' } }
            steps { sh 'mvn clean package -DskipTests' }
        }
        stage('Test') {
            agent { docker { image 'maven:3.9-eclipse-temurin-17' } }
            steps { sh 'mvn test' }
        }
        stage('Build Docker Image') {
            agent { label 'docker' }
            steps {
                script {
                    def app = docker.build("company/my-app:${env.BUILD_NUMBER}")
                    docker.withRegistry('https://nexus.company.com:8083', 'nexus-credentials') {
                        app.push()
                        app.push('latest')
                    }
                }
            }
        }
    }
}
```

**3. Docker Compose для интеграционных тестов:**

```groovy
stage('Integration Tests') {
    steps {
        sh 'docker-compose -f docker-compose.test.yml up -d'
        sh 'mvn verify -Pintegration-tests'
    }
    post {
        always {
            sh 'docker-compose -f docker-compose.test.yml down -v'
        }
    }
}
```

**4. Dockerfile как агент** — сборка Docker-образа из Dockerfile в репозитории:

```groovy
agent {
    dockerfile {
        filename 'Dockerfile.build'
        dir 'ci'
        additionalBuildArgs '--build-arg MAVEN_VERSION=3.9'
    }
}
```

[к оглавлению](#CICD)

## Что такое Multibranch Pipeline?

**Multibranch Pipeline** — тип проекта в Jenkins, который автоматически создает отдельный пайплайн для каждой ветки в репозитории, содержащей `Jenkinsfile`.

**Как это работает:**
1. Jenkins сканирует репозиторий и находит все ветки с `Jenkinsfile`.
2. Для каждой ветки автоматически создается Job.
3. При появлении новой ветки — создается новый Job; при удалении ветки — Job удаляется.
4. Pull Request / Merge Request автоматически получают свой пайплайн.

**Преимущества:**
- Автоматизация — не нужно вручную создавать Job для каждой ветки.
- Каждая ветка может иметь свой `Jenkinsfile` (разные пайплайны для разных веток).
- Отображение статуса сборки для Pull Request в Git-платформе.
- Удобная навигация по веткам в Jenkins UI.

**Пример Jenkinsfile для Multibranch Pipeline:**

```groovy
pipeline {
    agent { label 'java17' }

    stages {
        stage('Build') {
            steps { sh 'mvn clean compile' }
        }
        stage('Test') {
            steps { sh 'mvn test' }
        }
        stage('SonarQube') {
            when { anyOf { branch 'main'; branch 'develop' } }
            steps { sh 'mvn sonar:sonar' }
        }
        stage('Deploy to Nexus') {
            when { branch 'main' }
            steps { sh 'mvn deploy -DskipTests' }
        }
        stage('Deploy to Dev') {
            when { branch 'develop' }
            steps { sh './deploy.sh dev' }
        }
        stage('Deploy to Production') {
            when { branch 'main' }
            steps {
                input message: 'Deploy to production?'
                sh './deploy.sh production'
            }
        }
    }

    post {
        always { junit '**/surefire-reports/*.xml' }
    }
}
```

Переменная `env.BRANCH_NAME` содержит имя текущей ветки, а `env.CHANGE_ID` — номер Pull Request (если сборка запущена для PR).

[к оглавлению](#CICD)

## Что такое Nexus Repository Manager?

**Sonatype Nexus Repository Manager** — это менеджер репозиториев артефактов, который служит единой точкой хранения и распространения программных компонентов (JAR, WAR, Docker-образы, npm-пакеты и т.д.).

**Основные функции:**
- **Проксирование** внешних репозиториев (Maven Central, Docker Hub и др.) — кэширование зависимостей внутри организации.
- **Хранение** приватных артефактов (собственные библиотеки, приложения).
- **Управление** жизненным циклом артефактов (snapshot, release, удаление старых версий).
- **Контроль доступа** — кто может читать и публиковать артефакты.

**Версии Nexus:**
- **Nexus Repository OSS** — бесплатная версия (open-source).
- **Nexus Repository Pro** — платная с расширенными возможностями (staging, SAML, HA clustering).

**Поддерживаемые форматы:**
- **Maven (Java)** — JAR, WAR, POM.
- **npm** — JavaScript пакеты.
- **Docker** — Docker-образы.
- **PyPI** — Python пакеты.
- **NuGet** — .NET пакеты.
- **Helm** — Kubernetes charts.
- **Raw** — произвольные файлы.
- И многие другие.

В банковской среде Nexus необходим, так как:
- Обеспечивает работу в закрытом контуре (без прямого доступа в интернет).
- Позволяет контролировать, какие внешние зависимости используются (compliance, лицензии).
- Гарантирует воспроизводимость сборки — зависимости не пропадут из публичных репозиториев.
- Обеспечивает единое хранилище артефактов для всех команд.

[к оглавлению](#CICD)

## Какие типы репозиториев существуют в Nexus?

В Nexus существуют три типа репозиториев:

**1. Hosted (размещённый)**
- Хранит артефакты, загруженные непосредственно в Nexus.
- Используется для приватных артефактов вашей организации.
- Примеры:
  - `maven-releases` — релизные версии (1.0.0, 2.1.3).
  - `maven-snapshots` — snapshot-версии (1.0.0-SNAPSHOT).
- Важное различие: **releases** репозиторий обычно не позволяет перезаписывать уже загруженный артефакт (обеспечивает неизменяемость релизов), а **snapshots** — позволяет.

**2. Proxy (прокси)**
- Проксирует и кэширует артефакты из удалённых репозиториев.
- При первом запросе скачивает артефакт из внешнего репозитория и сохраняет в локальный кэш.
- Последующие запросы отдаются из кэша.
- Примеры:
  - `maven-central` — прокси для Maven Central.
  - `docker-hub` — прокси для Docker Hub.
  - `npm-registry` — прокси для npmjs.com.

**3. Group (групповой)**
- Объединяет несколько hosted и proxy репозиториев под одним URL.
- Клиент (Maven, Gradle) настраивается на один URL, а Nexus автоматически ищет артефакт во всех входящих репозиториях.
- Пример: `maven-public` объединяет `maven-releases`, `maven-snapshots` и `maven-central`.

**Схема взаимодействия:**

```
Maven/Gradle  →  maven-public (group)
                    ├── maven-releases (hosted)   ← Ваши релизы
                    ├── maven-snapshots (hosted)   ← Ваши snapshot'ы
                    └── maven-central (proxy)       ← Кэш Maven Central
```

Разработчик настраивает один URL (`maven-public`) и получает доступ ко всем артефактам: и своим, и публичным.

[к оглавлению](#CICD)

## Зачем использовать Nexus в проекте?

Основные причины внедрения Nexus:

**1. Кэширование зависимостей**
- Внешние зависимости скачиваются из интернета один раз и кэшируются в Nexus.
- Последующие сборки скачивают зависимости из Nexus по локальной сети — значительно быстрее.
- Сборка не сломается, если Maven Central или другой внешний ресурс временно недоступен.

**2. Хранение приватных артефактов**
- Собственные библиотеки и модули, используемые несколькими проектами.
- Закрытый исходный код, который нельзя публиковать в открытых репозиториях.

**3. Контроль и безопасность**
- Контроль того, какие внешние зависимости попадают в организацию.
- Nexus IQ Server (в Pro-версии) проверяет зависимости на известные уязвимости (CVE).
- Проверка лицензий — важно для банковского сектора.
- Аудит загрузок — кто и когда скачал/загрузил артефакт.

**4. Воспроизводимость сборки**
- Зависимости не исчезнут из публичного репозитория — они сохранены в Nexus.
- Гарантия, что сборка через год воспроизведётся с теми же зависимостями.

**5. Работа в закрытом контуре**
- В банках и enterprise-среде сборочные серверы часто не имеют прямого доступа в интернет.
- Nexus является единственным каналом получения внешних зависимостей (через proxy-репозиторий в DMZ).

**6. Единая точка распространения**
- Все команды используют один Nexus.
- Стандартизация конфигурации: один URL для Maven, один для Docker и т.д.

[к оглавлению](#CICD)

## Как настроить Maven для работы с Nexus?

Для работы Maven с Nexus необходимо настроить два файла:

**1. `settings.xml`** (обычно в `~/.m2/settings.xml` или предоставляется Jenkins через Config File Provider):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0
          https://maven.apache.org/xsd/settings-1.2.0.xsd">

    <!-- Учетные данные для Nexus -->
    <servers>
        <server>
            <id>nexus-releases</id>
            <username>${env.NEXUS_USER}</username>
            <password>${env.NEXUS_PASS}</password>
        </server>
        <server>
            <id>nexus-snapshots</id>
            <username>${env.NEXUS_USER}</username>
            <password>${env.NEXUS_PASS}</password>
        </server>
    </servers>

    <!-- Зеркало: все запросы идут через Nexus -->
    <mirrors>
        <mirror>
            <id>nexus</id>
            <mirrorOf>*</mirrorOf>
            <url>https://nexus.company.com/repository/maven-public/</url>
        </mirror>
    </mirrors>

    <profiles>
        <profile>
            <id>nexus</id>
            <repositories>
                <repository>
                    <id>nexus-public</id>
                    <url>https://nexus.company.com/repository/maven-public/</url>
                    <releases><enabled>true</enabled></releases>
                    <snapshots><enabled>true</enabled></snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>nexus-public</id>
                    <url>https://nexus.company.com/repository/maven-public/</url>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>

    <activeProfiles>
        <activeProfile>nexus</activeProfile>
    </activeProfiles>
</settings>
```

**2. `pom.xml`** проекта (секция `distributionManagement` — куда публиковать артефакты):

```xml
<distributionManagement>
    <repository>
        <id>nexus-releases</id>
        <name>Nexus Releases</name>
        <url>https://nexus.company.com/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <name>Nexus Snapshots</name>
        <url>https://nexus.company.com/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

Значения `<id>` в `distributionManagement` должны совпадать с `<id>` в `<servers>` файла `settings.xml` — так Maven понимает, какие учетные данные использовать.

Команда для публикации артефакта:
```bash
mvn deploy -s /path/to/settings.xml
```

[к оглавлению](#CICD)

## Как настроить Gradle для работы с Nexus?

Для работы Gradle с Nexus настраивается файл `build.gradle` (или `build.gradle.kts` для Kotlin DSL):

**Скачивание зависимостей из Nexus (`build.gradle`):**

```groovy
repositories {
    // Все зависимости скачиваются из Nexus
    maven {
        url 'https://nexus.company.com/repository/maven-public/'
        credentials {
            username = project.findProperty('nexusUser') ?: System.getenv('NEXUS_USER')
            password = project.findProperty('nexusPass') ?: System.getenv('NEXUS_PASS')
        }
        allowInsecureProtocol = false  // Требовать HTTPS
    }
}
```

**Публикация артефактов в Nexus (`build.gradle`):**

```groovy
plugins {
    id 'java'
    id 'maven-publish'
}

publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
            groupId = 'com.company'
            artifactId = 'my-service'
            version = '1.0.0'
        }
    }
    repositories {
        maven {
            name = 'nexus'
            def releasesUrl = 'https://nexus.company.com/repository/maven-releases/'
            def snapshotsUrl = 'https://nexus.company.com/repository/maven-snapshots/'
            url = version.endsWith('SNAPSHOT') ? snapshotsUrl : releasesUrl
            credentials {
                username = project.findProperty('nexusUser') ?: System.getenv('NEXUS_USER')
                password = project.findProperty('nexusPass') ?: System.getenv('NEXUS_PASS')
            }
        }
    }
}
```

**Файл `gradle.properties`** (для локальной разработки; не коммитить в Git!):

```properties
nexusUser=developer
nexusPass=secret
```

**Kotlin DSL (`build.gradle.kts`):**

```kotlin
repositories {
    maven {
        url = uri("https://nexus.company.com/repository/maven-public/")
        credentials {
            username = findProperty("nexusUser")?.toString() ?: System.getenv("NEXUS_USER")
            password = findProperty("nexusPass")?.toString() ?: System.getenv("NEXUS_PASS")
        }
    }
}
```

Команда для публикации:
```bash
gradle publish -PnexusUser=$NEXUS_USER -PnexusPass=$NEXUS_PASS
```

[к оглавлению](#CICD)

## Как использовать Nexus в качестве Docker Registry?

Nexus может выступать в роли приватного Docker Registry, что позволяет хранить Docker-образы внутри организации.

**Настройка в Nexus:**

1. Создать **hosted** Docker-репозиторий:
   - Имя: `docker-hosted`
   - HTTP-порт: `8083` (отдельный порт для Docker API)
   - Разрешить Docker v2 API.

2. Создать **proxy** Docker-репозиторий:
   - Имя: `docker-hub-proxy`
   - Remote URL: `https://registry-1.docker.io`
   - HTTP-порт: `8084`

3. Создать **group** Docker-репозиторий:
   - Имя: `docker-group`
   - Объединяет `docker-hosted` и `docker-hub-proxy`.
   - HTTP-порт: `8085`

**Использование Docker с Nexus:**

```bash
# Авторизация в Nexus Docker Registry
docker login nexus.company.com:8083 -u admin -p admin123

# Тегирование образа для Nexus
docker tag my-app:1.0.0 nexus.company.com:8083/my-app:1.0.0

# Загрузка образа в Nexus
docker push nexus.company.com:8083/my-app:1.0.0

# Скачивание образа из Nexus
docker pull nexus.company.com:8085/my-app:1.0.0
```

**Использование в Jenkinsfile:**

```groovy
stage('Build & Push Docker Image') {
    steps {
        script {
            def image = docker.build("nexus.company.com:8083/my-app:${env.BUILD_NUMBER}")
            docker.withRegistry('https://nexus.company.com:8083', 'nexus-docker-credentials') {
                image.push()
                image.push('latest')
            }
        }
    }
}
```

**Настройка Docker daemon** (`/etc/docker/daemon.json`) при использовании HTTP (без TLS):

```json
{
    "insecure-registries": ["nexus.company.com:8083", "nexus.company.com:8085"]
}
```

В production рекомендуется использовать HTTPS с валидным сертификатом.

[к оглавлению](#CICD)

## Как организовать деплой артефактов из Jenkins в Nexus?

Существует несколько способов публикации артефактов из Jenkins в Nexus:

**Способ 1: Maven Deploy (рекомендуемый для Maven-проектов)**

```groovy
pipeline {
    agent { label 'java17-maven' }
    stages {
        stage('Build & Test') {
            steps { sh 'mvn clean verify' }
        }
        stage('Deploy to Nexus') {
            when { branch 'main' }
            steps {
                configFileProvider([configFile(fileId: 'maven-settings-nexus', variable: 'MVN_SETTINGS')]) {
                    sh 'mvn deploy -DskipTests -s $MVN_SETTINGS'
                }
            }
        }
    }
}
```

**Способ 2: Nexus Artifact Uploader Plugin**

```groovy
stage('Upload to Nexus') {
    steps {
        nexusArtifactUploader(
            nexusVersion: 'nexus3',
            protocol: 'https',
            nexusUrl: 'nexus.company.com',
            groupId: 'com.company',
            version: '1.0.0',
            repository: 'maven-releases',
            credentialsId: 'nexus-credentials',
            artifacts: [
                [artifactId: 'my-app',
                 classifier: '',
                 file: 'target/my-app-1.0.0.jar',
                 type: 'jar'],
                [artifactId: 'my-app',
                 classifier: '',
                 file: 'pom.xml',
                 type: 'pom']
            ]
        )
    }
}
```

**Способ 3: Nexus REST API (curl)**

```groovy
stage('Upload via REST API') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'nexus-credentials',
                                         usernameVariable: 'USER',
                                         passwordVariable: 'PASS')]) {
            sh """
                curl -v -u ${USER}:${PASS} \\
                    --upload-file target/my-app-1.0.0.jar \\
                    'https://nexus.company.com/repository/maven-releases/com/company/my-app/1.0.0/my-app-1.0.0.jar'
            """
        }
    }
}
```

**Полный пайплайн с Nexus:**

```groovy
pipeline {
    agent { label 'java17-maven' }

    environment {
        VERSION = "${env.BUILD_NUMBER}"
        NEXUS_URL = 'https://nexus.company.com'
    }

    stages {
        stage('Build')   { steps { sh 'mvn clean compile' } }
        stage('Test')    { steps { sh 'mvn test' } }
        stage('Quality') { steps { sh 'mvn sonar:sonar' } }
        stage('Package') { steps { sh "mvn package -DskipTests -Drevision=${VERSION}" } }

        stage('Publish JAR to Nexus') {
            when { branch 'main' }
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'SETTINGS')]) {
                    sh "mvn deploy -DskipTests -s \$SETTINGS -Drevision=${VERSION}"
                }
            }
        }

        stage('Publish Docker to Nexus') {
            when { branch 'main' }
            steps {
                script {
                    def img = docker.build("nexus.company.com:8083/my-app:${VERSION}")
                    docker.withRegistry("${NEXUS_URL}:8083", 'nexus-docker-creds') {
                        img.push()
                        img.push('latest')
                    }
                }
            }
        }
    }
}
```

[к оглавлению](#CICD)

## Что такое Git Flow, GitHub Flow и Trunk-based development?

Это стратегии ветвления (branching strategies), определяющие, как команда работает с ветками Git в контексте CI/CD.

**1. Git Flow**

Классическая модель с долгоживущими ветками:
- `main` (master) — production-код.
- `develop` — интеграционная ветка.
- `feature/*` — ветки для новой функциональности (от `develop`).
- `release/*` — подготовка релиза (от `develop` в `main`).
- `hotfix/*` — срочные исправления production (от `main`).

Плюсы: строгая организация, подходит для планового релизного цикла.
Минусы: сложность, долгоживущие ветки приводят к конфликтам, медленная интеграция.

**2. GitHub Flow**

Упрощенная модель:
- `main` — всегда deployable.
- `feature/*` — короткоживущие ветки от `main`.
- Pull Request -> Code Review -> Merge в `main` -> Deploy.

Плюсы: простота, быстрая интеграция, хорошо сочетается с CI/CD.
Минусы: нужна хорошая автоматизация тестов и деплоя.

**3. Trunk-based Development**

Вся работа ведётся в одной ветке (`main`/`trunk`):
- Разработчики коммитят напрямую в `main` (или через очень короткоживущие ветки — не более 1-2 дней).
- Используются feature flags для скрытия незавершенной функциональности.
- Релиз-ветки создаются от `main` при необходимости.

Плюсы: максимально частая интеграция, минимум конфликтов, лучше всего для CI.
Минусы: требует высокой дисциплины, мощного набора тестов и feature flags.

**Рекомендации для CI/CD:**

| Стратегия | CI/CD совместимость | Применимость |
|---|---|---|
| Git Flow | Низкая (долгие ветки) | Плановые релизы, legacy |
| GitHub Flow | Высокая | Большинство проектов |
| Trunk-based | Максимальная | Зрелые команды, микросервисы |

В банковской среде часто используется **GitHub Flow** или **Git Flow** (из-за регуляторных требований к релизам).

[к оглавлению](#CICD)

## Merge vs Rebase в контексте CI — что предпочтительнее?

**Merge** и **Rebase** — два способа интеграции изменений из одной ветки в другую. Каждый имеет свои плюсы и минусы в контексте CI.

**Merge (слияние):**
```bash
git checkout main
git merge feature/my-feature
```
- Создает merge-коммит, сохраняя полную историю ветвления.
- История нелинейная — видно, когда ветка была создана и влита.
- Не изменяет существующие коммиты.

**Rebase (перебазирование):**
```bash
git checkout feature/my-feature
git rebase main
git checkout main
git merge feature/my-feature  # fast-forward
```
- Переносит коммиты ветки на вершину целевой ветки.
- Создает линейную историю.
- Изменяет хеши коммитов (переписывает историю).

**Влияние на CI:**

| Аспект | Merge | Rebase |
|---|---|---|
| CI-статус | Merge-коммит проверен CI | Каждый коммит должен собираться |
| Конфликты | Разрешаются один раз в merge-коммите | Разрешаются для каждого коммита при rebase |
| Повторный запуск CI | Только для merge-коммита | Для всех перебазированных коммитов |
| Bisect (поиск бага) | Сложнее из-за нелинейной истории | Проще — линейная история |
| Force push | Не нужен | Нужен в feature-ветку после rebase |

**Рекомендации:**
- **Для feature-веток**: rebase на `main` перед merge — обеспечивает актуальность ветки и линейную историю.
- **Для интеграции в `main`**: merge (или squash merge) — сохраняет точку интеграции.
- **Никогда не rebase публичные ветки** (`main`, `develop`) — это ломает историю для всех.

**Типичный workflow с CI:**
1. Разработчик работает в `feature/ABC-123`.
2. Перед merge в `main`: `git rebase main` (актуализация).
3. Push и создание Pull Request.
4. CI проверяет ветку.
5. Code review.
6. Merge (или squash merge) в `main`.
7. CI проверяет `main`.

В банковской среде часто используют **squash merge** — все коммиты feature-ветки объединяются в один коммит при merge, что упрощает историю и аудит.

[к оглавлению](#CICD)

## Как организовать CI/CD для Java-микросервисов в банковской среде?

Типичная архитектура CI/CD для банковских Java-микросервисов включает:

**Инфраструктура:**
- **Jenkins** — сервер автоматизации (on-premise, в закрытом контуре).
- **Nexus** — хранение артефактов (JAR, Docker-образы).
- **SonarQube** — статический анализ кода и проверка качества.
- **GitLab / Bitbucket** — система контроля версий (on-premise).
- **Kubernetes / OpenShift** — оркестрация контейнеров.
- **HashiCorp Vault** — управление секретами.

**Типичный пайплайн (полный Jenkinsfile):**

```groovy
@Library('bank-shared-library') _

pipeline {
    agent { label 'java17-maven-docker' }

    environment {
        APP_NAME    = 'payment-service'
        VERSION     = "${env.BUILD_NUMBER}"
        NEXUS_URL   = 'https://nexus.bank.internal'
        SONAR_URL   = 'https://sonar.bank.internal'
        DOCKER_REG  = 'nexus.bank.internal:8083'
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '20'))
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MVN_SETTINGS')]) {
                    sh "mvn clean compile -s \$MVN_SETTINGS"
                }
            }
        }

        stage('Tests') {
            parallel {
                stage('Unit Tests') {
                    steps { sh 'mvn test' }
                    post {
                        always {
                            junit '**/surefire-reports/*.xml'
                            jacoco(execPattern: '**/target/jacoco.exec')
                        }
                    }
                }
                stage('Security Check') {
                    steps { sh 'mvn dependency-check:check' }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.host.url=${SONAR_URL}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Package & Publish JAR') {
            when { anyOf { branch 'main'; branch 'develop' } }
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MVN_SETTINGS')]) {
                    sh "mvn deploy -DskipTests -s \$MVN_SETTINGS -Drevision=${VERSION}"
                }
            }
        }

        stage('Build & Push Docker') {
            when { anyOf { branch 'main'; branch 'develop' } }
            steps {
                script {
                    def img = docker.build("${DOCKER_REG}/${APP_NAME}:${VERSION}")
                    docker.withRegistry("https://${DOCKER_REG}", 'nexus-docker-creds') {
                        img.push()
                        img.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Dev') {
            when { branch 'develop' }
            steps { sh "./deploy.sh dev ${VERSION}" }
        }

        stage('Deploy to Staging') {
            when { branch 'main' }
            steps { sh "./deploy.sh staging ${VERSION}" }
        }

        stage('Deploy to Production') {
            when { branch 'main' }
            steps {
                input message: "Deploy ${APP_NAME}:${VERSION} to production?",
                      submitter: 'release-managers'
                sh "./deploy.sh production ${VERSION}"
            }
        }
    }

    post {
        success { notifyTeam(status: 'SUCCESS') }
        failure { notifyTeam(status: 'FAILURE') }
        cleanup { cleanWs() }
    }
}
```

**Ключевые особенности банковского CI/CD:**
- Обязательный Quality Gate (SonarQube) — код не проходит дальше при несоответствии стандартам.
- Проверка безопасности зависимостей (OWASP).
- Ручное подтверждение деплоя на production (`input` с ограничением по роли).
- Аудит всех действий.
- Работа в закрытом контуре — все инструменты on-premise.

[к оглавлению](#CICD)

## Как обеспечить безопасность CI/CD пайплайна?

Безопасность CI/CD особенно критична в банковском секторе. Основные практики:

**1. Управление секретами:**
- Никогда не хранить пароли, токены, ключи в коде или Jenkinsfile.
- Использовать Jenkins Credentials Store или внешние хранилища (HashiCorp Vault).
- Ротация секретов по расписанию.

```groovy
// Правильно: секреты из Credentials
withCredentials([string(credentialsId: 'db-password', variable: 'DB_PASS')]) {
    sh 'flyway -password=$DB_PASS migrate'
}

// Неправильно: секреты в коде
// sh 'flyway -password=MySecret123 migrate'  // НИКОГДА!
```

**2. Контроль доступа:**
- Role-Based Access Control (RBAC) — разделение прав по ролям.
- Принцип наименьших привилегий — агенты и процессы имеют только необходимые права.
- Ограничение, кто может редактировать Jenkinsfile и конфигурацию пайплайнов.
- LDAP/AD интеграция для единой аутентификации.

**3. Безопасность агентов:**
- Агенты не должны иметь доступ к master-данным Jenkins.
- Использование эфемерных (временных) агентов — контейнеры уничтожаются после сборки.
- Изоляция сборок разных проектов.

**4. Проверка зависимостей:**
- OWASP Dependency Check — поиск известных уязвимостей (CVE) в зависимостях.
- Nexus IQ — анализ лицензий и уязвимостей.
- Запрет использования зависимостей с критическими уязвимостями.

```groovy
stage('Security Scan') {
    steps {
        sh 'mvn dependency-check:check -DfailBuildOnCVSS=7'
        // Сборка упадет при обнаружении уязвимости с CVSS >= 7
    }
}
```

**5. Подпись и верификация артефактов:**
- Подпись Docker-образов (Docker Content Trust / Cosign).
- Подпись JAR-файлов.
- Проверка целостности артефактов перед деплоем.

**6. Аудит и мониторинг:**
- Jenkins Audit Trail Plugin — журнал всех действий.
- Мониторинг аномальных активностей (неожиданные сборки, деплой в нерабочее время).
- Хранение логов сборок длительное время.

**7. Защита пайплайна:**
- Jenkinsfile хранится в Git и проходит code review (изменение пайплайна = изменение кода).
- Запрет выполнения произвольных Groovy-скриптов (Script Security Plugin).
- Использование Shared Libraries, проверенных командой DevOps.

[к оглавлению](#CICD)
