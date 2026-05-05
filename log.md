[Вопросы для собеседования](README.md)

# Журналирование
+ [Какие существуют типы логов?](#Какие-существуют-типы-логов)
+ [Какие уровни логирования существуют?](#Какие-уровни-логирования-существуют)
+ [Что такое SLF4J и зачем он нужен?](#Что-такое-SLF4J-и-зачем-он-нужен)
+ [Что такое Logback и чем он лучше Log4j?](#Что-такое-Logback-и-чем-он-лучше-Log4j)
+ [Как настроить логирование в Spring Boot?](#Как-настроить-логирование-в-Spring-Boot)
+ [Что такое MDC (Mapped Diagnostic Context)?](#Что-такое-MDC-Mapped-Diagnostic-Context)
+ [Что такое Structured Logging и зачем оно нужно?](#Что-такое-Structured-Logging-и-зачем-оно-нужно)
+ [Какие есть лучшие практики логирования?](#Какие-есть-лучшие-практики-логирования)
+ [Какие существуют фреймворки логирования в Java?](#Какие-существуют-фреймворки-логирования-в-Java)
+ [Как собирать и анализировать логи в production?](#Как-собирать-и-анализировать-логи-в-production)

## Какие существуют типы логов?

По назначению логи делятся на три основных типа:

- **Системные (System)** — события инфраструктуры: запуск/остановка сервисов, обращения между модулями, состояние подключений к БД, очередям, внешним API.
- **Безопасности (Security)** — аутентификация, авторизация, изменение прав, подозрительная активность. Критически важны для аудита.
- **Приложения (Application / Business)** — бизнес-события: создание заказа, изменение статуса, действия пользователя.

> Пример: пользователь входит в приложение — это Security-лог. Запускает модуль — Application-лог. Модуль обращается к другому сервису — System-лог.

### Важное
- Разделение типов логов помогает настроить разные правила хранения, доступа и алертинга
- Security-логи часто обязательны для compliance (PCI DSS, GDPR)
- В микросервисах system-логи критичны для отладки межсервисного взаимодействия

### Частые ошибки
- Смешивание бизнес-логов с debug-информацией — усложняет анализ
- Логирование sensitive данных (пароли, токены, PII) в security-логах
- Отсутствие бизнес-логов — когда что-то идёт не так, невозможно понять бизнес-контекст

### Как используется в 2026
- Security-логи интегрируются с SIEM-системами (Splunk, Elastic Security)
- Business-логи часто реализуются через event sourcing или audit trail
- Observability-платформы (Grafana, Datadog) объединяют все типы логов в единый интерфейс

[к оглавлению](#Журналирование)

## Какие уровни логирования существуют?

Стандартные уровни (SLF4J / Logback), от наименее до наиболее критичного:

| Уровень | Назначение | Пример |
|---------|-----------|--------|
| **TRACE** | Максимально детальная информация для трассировки | Входящие/исходящие параметры каждого метода |
| **DEBUG** | Диагностическая информация для разработки | SQL-запросы, состояние кэша, промежуточные вычисления |
| **INFO** | Значимые события нормальной работы | Запуск сервиса, подключение к БД, обработка запроса |
| **WARN** | Потенциальная проблема, не мешающая работе | Устаревший API, fallback на default, retry |
| **ERROR** | Ошибка, требующая внимания | Недоступен внешний сервис, ошибка бизнес-логики, неожиданное исключение |

**Иерархия фильтрации:** установка уровня INFO означает, что логируются INFO, WARN и ERROR; TRACE и DEBUG — отбрасываются.

```
TRACE < DEBUG < INFO < WARN < ERROR
```

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class OrderService {
    private static final Logger log = LoggerFactory.getLogger(OrderService.class);

    public void processOrder(Order order) {
        log.debug("Обработка заказа: {}", order.getId());
        try {
            // бизнес-логика
            log.info("Заказ {} успешно обработан", order.getId());
        } catch (Exception e) {
            log.error("Ошибка обработки заказа {}", order.getId(), e);
        }
    }
}
```

### Важное
- В production стандартный уровень — **INFO**; DEBUG/TRACE включаются временно для диагностики
- ERROR — для ситуаций, требующих реакции (алерт); WARN — для потенциальных проблем
- Третий аргумент в `log.error("msg", id, exception)` — исключение, Logback автоматически выводит стек-трейс

### Частые ошибки
- **Всё на уровне INFO** — теряется возможность фильтрации; DEBUG-информация засоряет production-логи
- **ERROR для бизнес-ошибок** — «пользователь ввёл неверный пароль» — это не ERROR, а INFO/WARN
- **WARN для нормальных ситуаций** — если retry всегда успешен, это не WARN

### Как используется в 2026
- Spring Boot позволяет менять уровни логирования на лету через Actuator: `POST /actuator/loggers/com.example`
- В Kubernetes уровни можно менять через ConfigMap без перезапуска пода

[к оглавлению](#Журналирование)

## Что такое SLF4J и зачем он нужен?

**SLF4J (Simple Logging Facade for Java)** — это фасад (абстракция) над фреймворками логирования. SLF4J предоставляет единый API для логирования, а конкретная реализация (Logback, Log4j2, JUL) подключается через classpath.

**Зачем нужен фасад:**
- Библиотека A использует Log4j, библиотека B — JUL, ваш код — Logback. SLF4J объединяет всё в одну систему.
- Можно заменить реализацию логирования без изменения кода приложения.

```java
// API SLF4J — единственное, что импортируется в коде
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class UserService {
    // Стандартный способ создания логгера
    private static final Logger log = LoggerFactory.getLogger(UserService.class);

    public User findById(Long id) {
        log.debug("Поиск пользователя по id={}", id);     // параметризованное сообщение
        User user = repository.findById(id).orElse(null);
        if (user == null) {
            log.warn("Пользователь с id={} не найден", id);
        }
        return user;
    }
}
```

**Параметризованные сообщения** — ключевое преимущество SLF4J:

```java
// ПРАВИЛЬНО — параметр подставляется только если уровень логирования активен
log.debug("Обработано {} записей за {} мс", count, duration);

// НЕПРАВИЛЬНО — конкатенация выполняется ВСЕГДА, даже если DEBUG отключен
log.debug("Обработано " + count + " записей за " + duration + " мс");
```

**Архитектура SLF4J:**

```
Ваш код → SLF4J API → SLF4J Binding → Реализация (Logback / Log4j2 / JUL)
                              ↑
                     Bridges (jcl-over-slf4j, jul-to-slf4j, log4j-over-slf4j)
                     перенаправляют логи из других фреймворков в SLF4J
```

### Важное
- SLF4J — только интерфейсы (API); без binding (реализации) на classpath — NOP logger (ничего не логируется)
- Параметризованные сообщения `{}` — не String.format(), а ленивая подстановка
- Bridges перенаправляют логи из legacy-фреймворков (Commons Logging, JUL, Log4j 1.x) в SLF4J
- В Spring Boot SLF4J + Logback подключены по умолчанию, bridges уже настроены

### Частые ошибки
- **Два binding на classpath** — SLF4J выведет предупреждение и выберет один случайно
- **Конкатенация строк вместо `{}`** — потеря производительности при отключенном уровне
- **Импорт конкретной реализации** — `import ch.qos.logback.classic.Logger` вместо `import org.slf4j.Logger`

### Как используется в 2026
- SLF4J 2.x — текущая версия, поддерживает fluent API: `log.atInfo().addKeyValue("orderId", id).log("Заказ создан")`
- Стандарт де-факто для всех Java-проектов; alternatives (JUL, Commons Logging) — legacy

[к оглавлению](#Журналирование)

## Что такое Logback и чем он лучше Log4j?

**Logback** — фреймворк логирования, созданный автором Log4j (Ceki Gülcü) как его преемник. Является нативной реализацией SLF4J и используется по умолчанию в Spring Boot.

**Структура Logback:**
- **logback-core** — базовый модуль
- **logback-classic** — реализация SLF4J API (аналог Log4j)
- **logback-access** — интеграция с Servlet-контейнерами для HTTP access логов

**Преимущества Logback над Log4j 1.x:**

| Критерий | Log4j 1.x | Logback |
|----------|-----------|---------|
| Производительность | Медленнее | В 10 раз быстрее на критических путях |
| SLF4J | Через адаптер | Нативная реализация |
| Автоматическая перезагрузка конфига | Нет | Да, без потери событий |
| Условная конфигурация (`<if>`) | Нет | Да |
| Фильтры | Базовые | Мощные (TurboFilter, EvaluatorFilter) |
| Graceful recovery | Нет | Автоматическое восстановление после ошибок I/O |
| Архивирование | Ограниченное | Автоматическое сжатие, удаление старых логов |

> **Примечание:** Log4j 1.x End-of-Life с 2015 года. Log4j 2.x — отдельный проект, современная альтернатива Logback. Оба варианта (Logback и Log4j 2.x) актуальны в 2026.

**Базовая конфигурация `logback.xml`:**

```xml
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/app.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>logs/app.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
            <maxFileSize>100MB</maxFileSize>
            <maxHistory>30</maxHistory>
            <totalSizeCap>3GB</totalSizeCap>
        </rollingPolicy>
        <encoder>
            <pattern>%d{ISO8601} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <logger name="com.example" level="DEBUG"/>
    <logger name="org.hibernate.SQL" level="DEBUG"/>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

### Важное
- Logback — нативная реализация SLF4J, Spring Boot по умолчанию
- Log4j 1.x — EOL, не использовать; Log4j 2.x — современная альтернатива
- Rolling policy — обязательна для production (ротация по размеру и времени, автоматическое сжатие)
- `%logger{36}` — имя логгера, обрезанное до 36 символов

### Частые ошибки
- **Логирование в файл без ротации** — диск заполняется, приложение падает
- **Не указать `totalSizeCap`** — старые логи никогда не удаляются
- **Использовать Log4j 1.x в новых проектах** — EOL, уязвимости не исправляются (не путать с Log4j2, где была Log4Shell)

### Как используется в 2026
- Logback — стандарт в Spring Boot экосистеме
- Log4j 2.x — альтернатива, особенно для проектов с async logging (LMAX Disruptor)
- В Kubernetes логирование в stdout предпочтительнее файлов — Logback ConsoleAppender

[к оглавлению](#Журналирование)

## Как настроить логирование в Spring Boot?

Spring Boot использует Logback по умолчанию и предоставляет удобную конфигурацию через `application.yml` и `logback-spring.xml`.

**Простая настройка через `application.yml`:**

```yaml
logging:
  level:
    root: INFO
    com.example: DEBUG
    org.hibernate.SQL: DEBUG
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE  # параметры SQL
  file:
    name: logs/application.log
  logback:
    rollingpolicy:
      max-file-size: 100MB
      max-history: 30
      total-size-cap: 3GB
  pattern:
    console: "%d{HH:mm:ss.SSS} %highlight(%-5level) [%thread] %cyan(%logger{36}) - %msg%n"
    file: "%d{ISO8601} %-5level [%thread] %logger{36} - %msg%n"
```

**Расширенная настройка через `logback-spring.xml`:**

```xml
<configuration>
    <!-- Поддержка Spring-профилей -->
    <springProfile name="dev">
        <root level="DEBUG">
            <appender-ref ref="CONSOLE"/>
        </root>
    </springProfile>

    <springProfile name="prod">
        <root level="INFO">
            <appender-ref ref="CONSOLE"/>
            <appender-ref ref="FILE"/>
        </root>
    </springProfile>
</configuration>
```

**Изменение уровней на лету через Actuator:**

```bash
# Посмотреть текущий уровень
curl http://localhost:8080/actuator/loggers/com.example

# Изменить уровень без перезапуска
curl -X POST http://localhost:8080/actuator/loggers/com.example \
  -H 'Content-Type: application/json' \
  -d '{"configuredLevel": "DEBUG"}'
```

### Важное
- `logback-spring.xml` (не `logback.xml`!) — поддерживает `<springProfile>` и `<springProperty>`
- `application.yml` — достаточен для простых сценариев; `logback-spring.xml` — для сложных (фильтры, несколько appenders)
- Spring Boot Actuator (`/actuator/loggers`) — изменение уровней в runtime без перезапуска

### Частые ошибки
- **`logback.xml` вместо `logback-spring.xml`** — теряется поддержка Spring-профилей и свойств
- **Включить `org.hibernate.SQL=DEBUG` в production** — огромный объём логов; только для диагностики
- **Не настроить Actuator security** — endpoint `/actuator/loggers` должен быть защищён

### Как используется в 2026
- `application.yml` + `logback-spring.xml` — стандартная комбинация
- В Kubernetes: логирование в stdout (ConsoleAppender), сбор через Promtail/Fluentd
- Actuator + Kubernetes probes — мониторинг и динамическая диагностика

[к оглавлению](#Журналирование)

## Что такое MDC (Mapped Diagnostic Context)?

**MDC (Mapped Diagnostic Context)** — механизм SLF4J/Logback для привязки контекстных данных к текущему потоку. Все лог-записи в рамках потока автоматически обогащаются данными из MDC.

**Основное применение — correlation ID (ID запроса):**

```java
@Component
public class RequestFilter implements Filter {

    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
            throws IOException, ServletException {
        String requestId = ((HttpServletRequest) req).getHeader("X-Request-ID");
        if (requestId == null) {
            requestId = UUID.randomUUID().toString();
        }
        MDC.put("requestId", requestId);
        try {
            chain.doFilter(req, resp);
        } finally {
            MDC.clear(); // ОБЯЗАТЕЛЬНО очистить!
        }
    }
}
```

**Конфигурация Logback для вывода MDC:**

```xml
<pattern>%d{ISO8601} [%thread] [%X{requestId}] %-5level %logger{36} - %msg%n</pattern>
<!-- %X{requestId} — подставляет значение из MDC -->
```

**Результат в логах:**

```
2026-04-22 10:15:32.456 [http-nio-8080-exec-1] [a1b2c3d4] INFO  OrderService - Создание заказа
2026-04-22 10:15:32.789 [http-nio-8080-exec-1] [a1b2c3d4] INFO  PaymentService - Оплата обработана
2026-04-22 10:15:33.012 [http-nio-8080-exec-1] [a1b2c3d4] INFO  NotificationService - Уведомление отправлено
```

Все три записи связаны одним `requestId` — можно отследить весь путь запроса.

### Важное
- MDC привязан к потоку (`ThreadLocal` внутри) — в асинхронном коде нужна ручная передача
- `MDC.clear()` в `finally` — обязательно, иначе данные «перетекут» в следующий запрос (thread reuse в пуле)
- `%X{key}` в паттерне Logback — вставляет значение MDC; если ключ отсутствует — пустая строка

### Частые ошибки
- **Забыть `MDC.clear()`** — утечка контекста между запросами в пуле потоков
- **MDC в `@Async` методах** — новый поток не наследует MDC; нужен TaskDecorator для копирования
- **MDC с Virtual Threads** — при миллионах потоков MDC (на основе ThreadLocal) потребляет память; рассмотрите ScopedValue

### Как используется в 2026
- MDC + correlation ID — стандартный паттерн для tracing в микросервисах
- Micrometer Tracing (Spring Boot 3.x) автоматически пробрасывает traceId/spanId в MDC
- В реактивном стеке (WebFlux) вместо MDC используется Reactor Context

[к оглавлению](#Журналирование)

## Что такое Structured Logging и зачем оно нужно?

**Structured Logging** — логирование в машиночитаемом формате (обычно JSON) вместо plain text. Позволяет эффективно искать, фильтровать и анализировать логи в системах агрегации.

**Plain text (традиционно):**

```
2026-04-22 10:15:32 INFO OrderService - Заказ создан: orderId=12345, userId=678, amount=99.99
```

**Structured (JSON):**

```json
{
  "timestamp": "2026-04-22T10:15:32.456Z",
  "level": "INFO",
  "logger": "com.example.OrderService",
  "message": "Заказ создан",
  "orderId": 12345,
  "userId": 678,
  "amount": 99.99,
  "requestId": "a1b2c3d4"
}
```

**Настройка JSON-логирования в Logback (Logstash encoder):**

```xml
<!-- pom.xml -->
<!-- <dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>7.4</version>
</dependency> -->

<!-- logback-spring.xml -->
<appender name="JSON" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder">
        <includeMdcKeyName>requestId</includeMdcKeyName>
        <includeMdcKeyName>userId</includeMdcKeyName>
    </encoder>
</appender>
```

**Добавление structured полей в код:**

```java
import static net.logstash.logback.argument.StructuredArguments.*;

log.info("Заказ создан", kv("orderId", orderId), kv("amount", amount));
// JSON: {"message":"Заказ создан","orderId":12345,"amount":99.99}

// SLF4J 2.x fluent API:
log.atInfo()
    .addKeyValue("orderId", orderId)
    .addKeyValue("amount", amount)
    .log("Заказ создан");
```

### Важное
- Structured logging — JSON формат, позволяющий фильтрацию по полям в ELK/Loki/Datadog
- Logstash encoder — стандартный инструмент для JSON-логирования в Logback
- MDC-данные автоматически включаются в JSON-вывод
- В dev-среде удобнее plain text, в production — JSON

### Частые ошибки
- **JSON-логи в dev-среде** — нечитаемо для человека; используйте профили (plain text для dev, JSON для prod)
- **Логирование огромных объектов** — `log.info("Request: {}", requestBody)` с большим JSON может замедлить систему
- **Не добавлять бизнес-контекст** — JSON без `orderId`, `userId` не полезнее plain text

### Как используется в 2026
- Structured logging — стандарт для production в Kubernetes/cloud
- Grafana Loki / Elasticsearch — основные системы для поиска по structured логам
- Spring Boot 3.x поддерживает structured logging из коробки через `logging.structured.format.console=logstash`

[к оглавлению](#Журналирование)

## Какие есть лучшие практики логирования?

**Что логировать:**
- Начало и завершение значимых бизнес-операций
- Ошибки и исключения (с полным стек-трейсом)
- Входящие запросы (метод, URL, ключевые параметры)
- Интеграционные вызовы (к другим сервисам, БД, очередям)
- Изменения состояния (статус заказа, роль пользователя)

**Что НЕ логировать:**
- Пароли, токены, API-ключи, секреты
- Персональные данные (PII): номер карты, СНИЛС, паспорт
- Тела запросов/ответов целиком (могут содержать sensitive данные и быть огромными)

**Как логировать правильно:**

```java
// ПРАВИЛЬНО — параметризованные сообщения
log.info("Пользователь {} авторизован, роль: {}", userId, role);

// НЕПРАВИЛЬНО — конкатенация (выполняется всегда, даже при отключенном уровне)
log.debug("Результат: " + expensiveOperation());

// ПРАВИЛЬНО — исключение как последний аргумент (стек-трейс автоматически)
log.error("Ошибка обработки заказа {}", orderId, exception);

// НЕПРАВИЛЬНО — стек-трейс потерян
log.error("Ошибка: " + exception.getMessage());

// ПРАВИЛЬНО — проверка уровня для дорогих операций
if (log.isDebugEnabled()) {
    log.debug("Детали: {}", computeExpensiveDebugInfo());
}
```

**Гайд по выбору уровня:**

| Уровень | Когда использовать | Пример |
|---------|-------------------|--------|
| ERROR | Требуется реакция (алерт) | Сервис недоступен, данные повреждены |
| WARN | Потенциальная проблема | Retry succeeded, deprecated API, fallback |
| INFO | Нормальные бизнес-события | Заказ создан, пользователь авторизован |
| DEBUG | Диагностика для разработчика | SQL, промежуточные данные, состояние кэша |
| TRACE | Максимальная детализация | Вход/выход из метода, значения переменных |

### Важное
- **Один запрос = одна корреляция**: всегда логируйте requestId/traceId для связывания записей
- Исключение — ВСЕГДА последним аргументом: `log.error("msg", arg1, exception)`
- В production: INFO + structured JSON + correlation ID
- Логи должны быть полезны через 3 месяца, когда контекст забыт

### Частые ошибки
- **`log.error(exception.getMessage())`** — теряется стек-трейс; используйте `log.error("msg", exception)`
- **Логирование в цикле** — 10 000 итераций × лог = 10 000 записей; агрегируйте
- **Catch + log + throw** — двойное логирование; логируйте на верхнем уровне
- **ERROR для всех исключений** — `NotFoundException` при GET /users/999 — это не ERROR

### Как используется в 2026
- Lombok `@Slf4j` — автоматическая генерация `private static final Logger log`
- IDE (IntelliJ) — live templates `logi`, `logd`, `loge` для быстрого логирования
- Spring Boot Actuator — динамическое изменение уровней через HTTP

[к оглавлению](#Журналирование)

## Какие существуют фреймворки логирования в Java?

**Эволюция логирования в Java:**

| Год | Фреймворк | Описание | Статус в 2026 |
|-----|-----------|----------|---------------|
| 2002 | **java.util.logging (JUL)** | Встроен в JDK | Legacy, используется в мелких утилитах |
| 2001 | **Log4j 1.x** | Первый популярный фреймворк | EOL с 2015, не использовать |
| 2004 | **Commons Logging (JCL)** | Фасад от Apache | Legacy, заменён SLF4J |
| 2005 | **SLF4J** | Современный фасад | Стандарт де-факто |
| 2006 | **Logback** | Нативная реализация SLF4J | Стандарт (Spring Boot default) |
| 2014 | **Log4j 2.x** | Полная переработка Log4j | Актуален, async через LMAX Disruptor |

**Что использовать в 2026:**

```
Рекомендуемый стек: SLF4J API + Logback (Spring Boot по умолчанию)
Альтернатива:       SLF4J API + Log4j 2.x (для высоконагруженных систем с async logging)
```

**Log4j 2.x vs Logback:**

| Критерий | Logback | Log4j 2.x |
|----------|---------|-----------|
| Async logging | AsyncAppender (на основе BlockingQueue) | LMAX Disruptor (lock-free, быстрее) |
| API | SLF4J | Свой + SLF4J bridge |
| Spring Boot | По умолчанию | Через starter-log4j2 |
| Lambda support | Нет | Да (`log.debug(() -> expensiveOp())`) |

### Важное
- **SLF4J — всегда API**, конкретный фреймворк — деталь реализации
- Log4j 1.x и Log4j 2.x — **разные проекты**; уязвимость Log4Shell (CVE-2021-44228) была в Log4j 2.x
- JUL — только если нельзя добавить зависимости (standalone утилиты)

### Частые ошибки
- **Путать Log4j 1.x и Log4j 2.x** — это разные проекты с разными API и зависимостями
- **Использовать Log4j 1.x** — EOL, без обновлений безопасности
- **Прямой импорт Logback API** — используйте SLF4J для переносимости

### Как используется в 2026
- 90%+ Spring Boot проектов — SLF4J + Logback (дефолт)
- Высоконагруженные системы — рассматривают Log4j 2.x с Async Loggers
- GraalVM Native Image — JUL или Log4j 2.x (Logback не полностью совместим с native image из коробки)

[к оглавлению](#Журналирование)

## Как собирать и анализировать логи в production?

**Принцип:** приложение пишет логи в stdout/файлы, инфраструктура собирает и доставляет их в систему агрегации.

**Популярные стеки:**

**1. ELK Stack (Elasticsearch + Logstash + Kibana):**

```
Приложение → stdout (JSON) → Filebeat/Logstash → Elasticsearch → Kibana (UI)
```

**2. Grafana Loki (лёгкая альтернатива):**

```
Приложение → stdout (JSON) → Promtail → Loki → Grafana (UI)
```

**3. Cloud-native:**

```
Приложение → stdout → CloudWatch (AWS) / Cloud Logging (GCP) / Azure Monitor
```

**Логирование в Kubernetes:**

```yaml
# Приложение пишет в stdout — стандартный подход в K8s
# Логи автоматически собираются kubelet и доступны через kubectl logs
# Для агрегации: DaemonSet с Promtail/Fluentd на каждой ноде

# Пример: Promtail DaemonSet читает /var/log/containers/*.log
# и отправляет в Loki
```

**Correlation ID для distributed tracing:**

```java
// Заголовок X-Request-ID проходит через все микросервисы
// Каждый сервис добавляет его в MDC → в логи → в систему агрегации
// В Kibana/Grafana: фильтр по requestId показывает путь запроса через все сервисы

// Spring Boot 3.x + Micrometer Tracing — автоматически:
// - генерирует traceId и spanId
// - пробрасывает через HTTP-заголовки (W3C Trace Context)
// - добавляет в MDC → в логи
```

### Важное
- В Kubernetes: **всегда stdout**, не файлы — kubelet + DaemonSet собирают автоматически
- JSON-формат обязателен для агрегации — plain text сложно парсить
- Correlation ID / traceId — связывает логи одного запроса через все микросервисы
- Retention policy — определите, сколько хранить логи (7 дней DEBUG, 90 дней INFO, 1 год ERROR)

### Частые ошибки
- **Логи в файлы внутри контейнера** — при рестарте пода файлы теряются; пишите в stdout
- **Отсутствие retention policy** — логи растут бесконечно, Elasticsearch потребляет терабайты
- **Нет алертинга** — логи собираются, но никто не настроил алерт на ERROR

### Как используется в 2026
- Grafana Loki вытесняет ELK для многих проектов (проще, дешевле)
- OpenTelemetry унифицирует логи, метрики и трейсы в единый стандарт
- Spring Boot 3.x + Micrometer — автоматическая интеграция с Grafana/Prometheus/Tempo

[к оглавлению](#Журналирование)

[Вопросы для собеседования](README.md)
