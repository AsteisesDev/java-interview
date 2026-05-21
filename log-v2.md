[Вопросы для собеседования](README.md)

# Журналирование

+ [Какие существуют типы логов?](#какие-существуют-типы-логов)
+ [Какие уровни логирования существуют?](#какие-уровни-логирования-существуют)
+ [Что такое SLF4J и зачем он нужен?](#что-такое-slf4j-и-зачем-он-нужен)
+ [Что такое Logback и чем он лучше Log4j?](#что-такое-logback-и-чем-он-лучше-log4j)
+ [Как настроить логирование в Spring Boot?](#как-настроить-логирование-в-spring-boot)
+ [Что такое MDC (Mapped Diagnostic Context)?](#что-такое-mdc-mapped-diagnostic-context)
+ [Что такое Structured Logging и зачем оно нужно?](#что-такое-structured-logging-и-зачем-оно-нужно)
+ [Какие есть лучшие практики логирования?](#какие-есть-лучшие-практики-логирования)
+ [Какие существуют фреймворки логирования в Java?](#какие-существуют-фреймворки-логирования-в-java)
+ [Как собирать и анализировать логи в production?](#как-собирать-и-анализировать-логи-в-production)

## Какие существуют типы логов?
<!-- grade: junior -->

Логи делятся на три основных типа по назначению: системные, безопасности и приложения.

> **Аналогия из жизни:** в больнице ведут три журнала: журнал работы оборудования (системный), журнал посещений и допусков (безопасность) и историю болезни пациента (бизнес-логика). Каждый журнал читают разные люди и хранят по разным правилам.

### Типы логов

| Тип | Назначение | Примеры событий |
|-----|-----------|-----------------|
| Системные (System) | События инфраструктуры | Запуск/остановка сервисов, подключения к БД, очередям, внешним API |
| Безопасности (Security) | Аутентификация, авторизация, аудит | Вход/выход пользователя, изменение прав, подозрительная активность |
| Приложения (Application / Business) | Бизнес-события | Создание заказа, изменение статуса, действия пользователя |

Пример: пользователь входит в приложение — это Security-лог. Запускает модуль — Application-лог. Модуль обращается к другому сервису — System-лог.

### Зачем разделять типы

- Разные правила хранения, доступа и алертинга для каждого типа
- Security-логи часто обязательны для compliance (PCI DSS, GDPR)
- В микросервисах system-логи критичны для отладки межсервисного взаимодействия
- Business-логи позволяют понять бизнес-контекст инцидента

### Частые ошибки

- Смешивание бизнес-логов с debug-информацией — усложняет анализ
- Логирование sensitive данных (пароли, токены, PII) в security-логах
- Отсутствие бизнес-логов — невозможно восстановить бизнес-контекст инцидента

### Как используется в 2026

- Security-логи интегрируются с SIEM-системами (Splunk, Elastic Security)
- Business-логи реализуются через event sourcing или audit trail
- Observability-платформы (Grafana, Datadog) объединяют все типы логов в единый интерфейс

> **На собеседовании:** интервьюер хочет услышать не просто перечисление типов, а понимание того, зачем их разделять. Частая ошибка — забыть про security-логи и compliance-требования.

[к оглавлению](#Журналирование)

---

## Какие уровни логирования существуют?
<!-- grade: junior -->

Уровни логирования — это иерархия важности сообщений, позволяющая фильтровать вывод. Установка уровня INFO означает, что логируются INFO, WARN и ERROR, а TRACE и DEBUG отбрасываются.

### Стандартные уровни (SLF4J / Logback)

От наименее до наиболее критичного:

| Уровень | Назначение | Пример | Когда включать |
|---------|-----------|--------|---------------|
| TRACE | Максимально детальная трассировка | Входящие/исходящие параметры каждого метода | Только при глубокой отладке |
| DEBUG | Диагностическая информация | SQL-запросы, состояние кэша, промежуточные вычисления | Разработка, диагностика в production |
| INFO | Значимые события нормальной работы | Запуск сервиса, подключение к БД, обработка запроса | Всегда в production |
| WARN | Потенциальная проблема, не мешающая работе | Устаревший API, fallback на default, retry | Всегда |
| ERROR | Ошибка, требующая внимания | Недоступен внешний сервис, неожиданное исключение | Всегда |

### Иерархия фильтрации

```
TRACE < DEBUG < INFO < WARN < ERROR
```

В production стандартный уровень — INFO. DEBUG и TRACE включаются временно для диагностики конкретных проблем.

<details>
<summary>Пример использования уровней</summary>

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class OrderService {
    private static final Logger log = LoggerFactory.getLogger(OrderService.class);

    public void processOrder(Order order) {
        log.trace("Вход в processOrder, order={}", order);
        log.debug("Обработка заказа: {}", order.getId());
        try {
            // бизнес-логика
            log.info("Заказ {} успешно обработан", order.getId());
        } catch (TemporaryException e) {
            log.warn("Временная ошибка при обработке заказа {}, retry", order.getId(), e);
        } catch (Exception e) {
            log.error("Ошибка обработки заказа {}", order.getId(), e);
        }
    }
}
```

</details>

### Правила выбора уровня

- ERROR — ситуация требует реакции (алерт), что-то сломалось
- WARN — потенциальная проблема, система справилась сама (retry, fallback)
- INFO — нормальное бизнес-событие, полезное для понимания поведения системы
- DEBUG — информация для разработчика при отладке
- TRACE — пошаговая трассировка для глубокой диагностики

### Частые ошибки

- Все на уровне INFO — теряется возможность фильтрации; DEBUG-информация засоряет production-логи
- ERROR для бизнес-ошибок — "пользователь ввёл неверный пароль" — это INFO или WARN, но не ERROR
- WARN для нормальных ситуаций — если retry всегда успешен, это штатное поведение
- Третий аргумент `log.error("msg", id, exception)` — исключение, Logback автоматически выводит стек-трейс. Не используйте `exception.getMessage()`

### Как используется в 2026

- Spring Boot позволяет менять уровни логирования на лету через Actuator: `POST /actuator/loggers/com.example`
- В Kubernetes уровни можно менять через ConfigMap без перезапуска пода

> **На собеседовании:** покажите, что понимаете разницу между WARN и ERROR. ERROR — когда нужна реакция человека или алерт. WARN — система справилась сама, но ситуация нештатная. Частая ошибка — ставить ERROR на все исключения, включая NotFoundException.

[к оглавлению](#Журналирование)

---

## Что такое SLF4J и зачем он нужен?
<!-- grade: junior -->

SLF4J (Simple Logging Facade for Java) — фасад (абстракция) над фреймворками логирования, предоставляющий единый API, а конкретная реализация (Logback, Log4j2, JUL) подключается через classpath.

> **Аналогия из жизни:** SLF4J — это как стандартная розетка. Неважно, какая электростанция производит ток (Logback, Log4j2) — ваш прибор (код) просто втыкается в розетку (SLF4J API) и работает.

### Зачем нужен фасад

- Библиотека A использует Log4j, библиотека B — JUL, ваш код — Logback. SLF4J объединяет всё в одну систему
- Можно заменить реализацию логирования без изменения кода приложения
- Единый API для всей команды и всех зависимостей

### Архитектура SLF4J

```
Ваш код --> SLF4J API --> SLF4J Binding --> Реализация (Logback / Log4j2 / JUL)
                                ^
                       Bridges (jcl-over-slf4j, jul-to-slf4j, log4j-over-slf4j)
                       перенаправляют логи из других фреймворков в SLF4J
```

<details>
<summary>Пример использования SLF4J</summary>

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

</details>

### Параметризованные сообщения

Ключевое преимущество SLF4J — ленивая подстановка параметров через `{}`:

```java
// ПРАВИЛЬНО — параметр подставляется только если уровень логирования активен
log.debug("Обработано {} записей за {} мс", count, duration);

// НЕПРАВИЛЬНО — конкатенация выполняется ВСЕГДА, даже если DEBUG отключен
log.debug("Обработано " + count + " записей за " + duration + " мс");
```

Параметризованные сообщения `{}` — это не `String.format()`, а ленивая подстановка: строка формируется только если сообщение действительно будет залогировано.

### Ключевые компоненты

| Компонент | Роль | Пример |
|-----------|------|--------|
| SLF4J API | Интерфейсы для логирования | `org.slf4j.Logger`, `LoggerFactory` |
| Binding | Связь API с реализацией | `logback-classic`, `log4j-slf4j2-impl` |
| Bridge | Перенаправление legacy-фреймворков в SLF4J | `jcl-over-slf4j`, `jul-to-slf4j` |

### Частые ошибки

- Два binding на classpath — SLF4J выведет предупреждение и выберет один случайно
- Конкатенация строк вместо `{}` — потеря производительности при отключенном уровне
- Импорт конкретной реализации — `import ch.qos.logback.classic.Logger` вместо `import org.slf4j.Logger`
- Без binding на classpath — NOP logger (ничего не логируется, молчаливый провал)

### Как используется в 2026

- SLF4J 2.x — текущая версия, поддерживает fluent API: `log.atInfo().addKeyValue("orderId", id).log("Заказ создан")`
- Стандарт де-факто для всех Java-проектов; JUL, Commons Logging — legacy
- В Spring Boot SLF4J + Logback подключены по умолчанию, bridges уже настроены

> **На собеседовании:** интервьюер ждёт объяснения зачем фасад, а не только что это. Ответ: изоляция кода от конкретной реализации логирования + унификация логов из разных библиотек. Частая ошибка — не упомянуть параметризованные сообщения как ключевое преимущество.

[к оглавлению](#Журналирование)

---

## Что такое Logback и чем он лучше Log4j?
<!-- grade: middle -->

Logback — фреймворк логирования, созданный автором Log4j (Ceki Gulcu) как его преемник. Является нативной реализацией SLF4J и используется по умолчанию в Spring Boot.

### Структура Logback

| Модуль | Назначение |
|--------|-----------|
| logback-core | Базовый модуль |
| logback-classic | Реализация SLF4J API (основной модуль) |
| logback-access | Интеграция с Servlet-контейнерами для HTTP access логов |

### Logback vs Log4j 1.x

| Критерий | Log4j 1.x | Logback |
|----------|-----------|---------|
| Производительность | Медленнее | В 10 раз быстрее на критических путях |
| SLF4J | Через адаптер | Нативная реализация |
| Автоматическая перезагрузка конфига | Нет | Да, без потери событий |
| Условная конфигурация (`<if>`) | Нет | Да |
| Фильтры | Базовые | Мощные (TurboFilter, EvaluatorFilter) |
| Graceful recovery | Нет | Автоматическое восстановление после ошибок I/O |
| Архивирование | Ограниченное | Автоматическое сжатие, удаление старых логов |
| Статус в 2026 | EOL с 2015, не использовать | Стандарт (Spring Boot default) |

Log4j 1.x End-of-Life с 2015 года. Log4j 2.x — отдельный проект, современная альтернатива Logback. Оба варианта (Logback и Log4j 2.x) актуальны.

<details>
<summary>Базовая конфигурация logback.xml</summary>

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

</details>

### Ключевые элементы конфигурации

- `%logger{36}` — имя логгера, обрезанное до 36 символов
- Rolling policy — обязательна для production: ротация по размеру и времени, автоматическое сжатие
- `totalSizeCap` — общий лимит дискового пространства для всех архивов
- `maxHistory` — сколько дней хранить ротированные файлы

### Частые ошибки

- Логирование в файл без ротации — диск заполняется, приложение падает
- Не указать `totalSizeCap` — старые логи никогда не удаляются
- Использовать Log4j 1.x в новых проектах — EOL, уязвимости не исправляются

### Как используется в 2026

- Logback — стандарт в Spring Boot экосистеме
- Log4j 2.x — альтернатива, особенно для проектов с async logging (LMAX Disruptor)
- В Kubernetes логирование в stdout предпочтительнее файлов — Logback ConsoleAppender

> **На собеседовании:** важно не путать Log4j 1.x и Log4j 2.x — это разные проекты. Logback vs Log4j 2.x — оба актуальны; Logback выбирают за Spring Boot default, Log4j 2.x — за async logging на LMAX Disruptor.

[к оглавлению](#Журналирование)

---

## Как настроить логирование в Spring Boot?
<!-- grade: middle -->

Spring Boot использует Logback по умолчанию и предоставляет два уровня конфигурации: простой через `application.yml` и расширенный через `logback-spring.xml`.

### Простая настройка через application.yml

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

### Расширенная настройка через logback-spring.xml

Файл `logback-spring.xml` (не `logback.xml`) поддерживает Spring-профили и Spring-свойства:

```xml
<configuration>
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

### Изменение уровней на лету через Actuator

```bash
# Посмотреть текущий уровень
curl http://localhost:8080/actuator/loggers/com.example

# Изменить уровень без перезапуска
curl -X POST http://localhost:8080/actuator/loggers/com.example \
  -H 'Content-Type: application/json' \
  -d '{"configuredLevel": "DEBUG"}'
```

### Какой способ конфигурации выбрать

| Способ | Когда использовать |
|--------|-------------------|
| `application.yml` | Простые сценарии: уровни, файл, паттерн |
| `logback-spring.xml` | Сложные сценарии: фильтры, несколько appenders, условная конфигурация |
| Actuator | Динамическая диагностика в runtime без перезапуска |

### Частые ошибки

- `logback.xml` вместо `logback-spring.xml` — теряется поддержка `<springProfile>` и `<springProperty>`
- Включить `org.hibernate.SQL=DEBUG` в production — огромный объём логов; только для временной диагностики
- Не настроить Actuator security — endpoint `/actuator/loggers` должен быть защищён

### Как используется в 2026

- `application.yml` + `logback-spring.xml` — стандартная комбинация
- В Kubernetes: логирование в stdout (ConsoleAppender), сбор через Promtail/Fluentd
- Actuator + Kubernetes probes — мониторинг и динамическая диагностика

> **На собеседовании:** ключевой момент — знание разницы между `logback.xml` и `logback-spring.xml`. Второй поддерживает Spring-профили. Также упомяните Actuator для изменения уровней на лету — это показывает production-опыт.

[к оглавлению](#Журналирование)

---

## Что такое MDC (Mapped Diagnostic Context)?
<!-- grade: middle -->

MDC (Mapped Diagnostic Context) — механизм SLF4J/Logback для привязки контекстных данных к текущему потоку. Все лог-записи в рамках потока автоматически обогащаются данными из MDC.

> **Аналогия из жизни:** MDC — это как бейдж посетителя в бизнес-центре. Пока бейдж надет, каждая камера на каждом этаже автоматически идентифицирует человека. Не нужно представляться у каждой двери.

### Основное применение — correlation ID

Все лог-записи одного HTTP-запроса связываются единым идентификатором:

<details>
<summary>Фильтр для установки requestId в MDC</summary>

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

</details>

### Конфигурация Logback для вывода MDC

```xml
<pattern>%d{ISO8601} [%thread] [%X{requestId}] %-5level %logger{36} - %msg%n</pattern>
<!-- %X{requestId} — подставляет значение из MDC -->
```

### Результат в логах

```
2026-04-22 10:15:32.456 [http-nio-8080-exec-1] [a1b2c3d4] INFO  OrderService - Создание заказа
2026-04-22 10:15:32.789 [http-nio-8080-exec-1] [a1b2c3d4] INFO  PaymentService - Оплата обработана
2026-04-22 10:15:33.012 [http-nio-8080-exec-1] [a1b2c3d4] INFO  NotificationService - Уведомление отправлено
```

Все три записи связаны одним `requestId` — можно отследить весь путь запроса через систему.

### Как работает MDC внутри

MDC основан на `ThreadLocal` — каждый поток хранит свою копию контекстных данных. Это означает:

| Сценарий | Поведение MDC | Решение |
|----------|--------------|---------|
| Синхронный код | Работает автоматически | - |
| `@Async` методы | Новый поток не наследует MDC | `TaskDecorator` для копирования |
| Reactive (WebFlux) | `ThreadLocal` не работает | `Reactor Context` |
| Virtual Threads | Работает, но при миллионах потоков расходует память | Рассмотрите `ScopedValue` |

### Частые ошибки

- Забыть `MDC.clear()` в `finally` — утечка контекста между запросами в пуле потоков (thread reuse)
- MDC в `@Async` методах — новый поток не наследует MDC; нужен TaskDecorator для копирования
- MDC с Virtual Threads — при миллионах потоков MDC (на основе ThreadLocal) потребляет значительную память

### Как используется в 2026

- MDC + correlation ID — стандартный паттерн для tracing в микросервисах
- Micrometer Tracing (Spring Boot 3.x) автоматически пробрасывает traceId/spanId в MDC
- В реактивном стеке (WebFlux) вместо MDC используется Reactor Context

> **На собеседовании:** главное — объяснить связку MDC + ThreadLocal + обязательная очистка. Частая ошибка кандидатов — не упомянуть проблему с асинхронным кодом, где MDC не передаётся автоматически.

[к оглавлению](#Журналирование)

---

## Что такое Structured Logging и зачем оно нужно?
<!-- grade: middle -->

Structured Logging — логирование в машиночитаемом формате (обычно JSON) вместо plain text. Позволяет эффективно искать, фильтровать и анализировать логи в системах агрегации по отдельным полям.

### Plain text vs Structured (JSON)

| Аспект | Plain text | Structured (JSON) |
|--------|-----------|-------------------|
| Читаемость человеком | Высокая | Низкая без инструментов |
| Машинный парсинг | Сложный (regex) | Нативный (по полям) |
| Фильтрация | По подстроке | По конкретным полям (`orderId=123`) |
| Интеграция с ELK/Loki | Требует grok-паттернов | Из коробки |
| Использование | Dev-среда | Production |

Plain text:
```
2026-04-22 10:15:32 INFO OrderService - Заказ создан: orderId=12345, userId=678, amount=99.99
```

Structured (JSON):
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

### Настройка JSON-логирования в Logback

Используется Logstash encoder:

```xml
<!-- logback-spring.xml -->
<appender name="JSON" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder">
        <includeMdcKeyName>requestId</includeMdcKeyName>
        <includeMdcKeyName>userId</includeMdcKeyName>
    </encoder>
</appender>
```

Зависимость в `pom.xml`:
```xml
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>7.4</version>
</dependency>
```

### Добавление structured полей в код

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

### Частые ошибки

- JSON-логи в dev-среде — нечитаемо для человека; используйте профили (plain text для dev, JSON для prod)
- Логирование огромных объектов — `log.info("Request: {}", requestBody)` с большим JSON замедляет систему
- Не добавлять бизнес-контекст — JSON без `orderId`, `userId` не полезнее plain text

### Как используется в 2026

- Structured logging — стандарт для production в Kubernetes/cloud
- Grafana Loki / Elasticsearch — основные системы для поиска по structured логам
- Spring Boot 3.x поддерживает structured logging из коробки через `logging.structured.format.console=logstash`
- MDC-данные автоматически включаются в JSON-вывод

> **На собеседовании:** объясните, почему plain text недостаточен для production — невозможно эффективно фильтровать по полям. Частая ошибка — не упомянуть, что в dev-среде по-прежнему удобнее plain text (разные профили).

[к оглавлению](#Журналирование)

---

## Какие есть лучшие практики логирования?
<!-- grade: middle -->

Хорошее логирование — это баланс между достаточностью информации для диагностики и отсутствием шума. Логи должны быть полезны через 3 месяца, когда контекст забыт.

### Что логировать

- Начало и завершение значимых бизнес-операций
- Ошибки и исключения (с полным стек-трейсом)
- Входящие запросы (метод, URL, ключевые параметры)
- Интеграционные вызовы (к другим сервисам, БД, очередям)
- Изменения состояния (статус заказа, роль пользователя)

### Что НЕ логировать

- Пароли, токены, API-ключи, секреты
- Персональные данные (PII): номер карты, СНИЛС, паспорт
- Тела запросов/ответов целиком (могут содержать sensitive данные и быть огромными)

### Как логировать правильно

```java
// ПРАВИЛЬНО — параметризованные сообщения
log.info("Пользователь {} авторизован, роль: {}", userId, role);

// НЕПРАВИЛЬНО — конкатенация (выполняется всегда)
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

### Гайд по выбору уровня

| Уровень | Когда использовать | Пример |
|---------|-------------------|--------|
| ERROR | Требуется реакция (алерт) | Сервис недоступен, данные повреждены |
| WARN | Потенциальная проблема, система справилась | Retry succeeded, deprecated API, fallback |
| INFO | Нормальные бизнес-события | Заказ создан, пользователь авторизован |
| DEBUG | Диагностика для разработчика | SQL, промежуточные данные, состояние кэша |
| TRACE | Максимальная детализация | Вход/выход из метода, значения переменных |

### Золотые правила

- Один запрос = одна корреляция: всегда логируйте requestId/traceId для связывания записей
- Исключение — ВСЕГДА последним аргументом: `log.error("msg", arg1, exception)`
- В production: INFO + structured JSON + correlation ID
- Не логируйте в цикле — 10 000 итераций по одному логу = 10 000 записей; агрегируйте результат
- Не делайте catch + log + throw — двойное логирование; логируйте на верхнем уровне

### Частые ошибки

- `log.error(exception.getMessage())` — теряется стек-трейс; используйте `log.error("msg", exception)`
- Логирование в цикле без агрегации — замедляет систему и засоряет логи
- Catch + log + throw — одно исключение логируется дважды
- ERROR для всех исключений — `NotFoundException` при `GET /users/999` — это не ERROR, а INFO или WARN

### Как используется в 2026

- Lombok `@Slf4j` — автоматическая генерация `private static final Logger log`
- IDE (IntelliJ) — live templates `logi`, `logd`, `loge` для быстрого логирования
- Spring Boot Actuator — динамическое изменение уровней через HTTP

> **На собеседовании:** перечислите конкретные антипаттерны: потеря стек-трейса через `getMessage()`, логирование в цикле, catch-log-throw. Это показывает production-опыт. Частая ошибка — дать абстрактный ответ "логируйте всё важное" без конкретных примеров.

[к оглавлению](#Журналирование)

---

## Какие существуют фреймворки логирования в Java?
<!-- grade: junior -->

В экосистеме Java исторически сложилось несколько фреймворков логирования, которые делятся на фасады (API) и реализации.

### Эволюция логирования в Java

| Год | Фреймворк | Тип | Описание | Статус в 2026 |
|-----|-----------|-----|----------|---------------|
| 2001 | Log4j 1.x | Реализация | Первый популярный фреймворк | EOL с 2015, не использовать |
| 2002 | java.util.logging (JUL) | Реализация | Встроен в JDK | Legacy, мелкие утилиты |
| 2004 | Commons Logging (JCL) | Фасад | Фасад от Apache | Legacy, заменён SLF4J |
| 2005 | SLF4J | Фасад | Современный фасад | Стандарт де-факто |
| 2006 | Logback | Реализация | Нативная реализация SLF4J | Стандарт (Spring Boot default) |
| 2014 | Log4j 2.x | Реализация | Полная переработка Log4j | Актуален, async через LMAX Disruptor |

### Что использовать в 2026

```
Рекомендуемый стек: SLF4J API + Logback (Spring Boot по умолчанию)
Альтернатива:       SLF4J API + Log4j 2.x (для высоконагруженных систем с async logging)
```

SLF4J — всегда API, конкретный фреймворк — деталь реализации.

### Log4j 2.x vs Logback

| Критерий | Logback | Log4j 2.x |
|----------|---------|-----------|
| Async logging | AsyncAppender (BlockingQueue) | LMAX Disruptor (lock-free, быстрее) |
| API | SLF4J (нативно) | Свой + SLF4J bridge |
| Spring Boot | По умолчанию | Через starter-log4j2 |
| Lambda support | Нет | Да: `log.debug(() -> expensiveOp())` |
| GraalVM Native Image | Ограниченная совместимость | Поддерживается лучше |

### Частые ошибки

- Путать Log4j 1.x и Log4j 2.x — это разные проекты с разными API и зависимостями
- Использовать Log4j 1.x — EOL, без обновлений безопасности
- Прямой импорт Logback API — используйте SLF4J для переносимости
- Уязвимость Log4Shell (CVE-2021-44228) была в Log4j 2.x (исправлена в 2.17+), не в Logback

### Как используется в 2026

- 90%+ Spring Boot проектов — SLF4J + Logback (дефолт)
- Высоконагруженные системы — рассматривают Log4j 2.x с Async Loggers
- GraalVM Native Image — Log4j 2.x или JUL (Logback не полностью совместим из коробки)

> **На собеседовании:** покажите знание эволюции: Log4j 1.x (EOL) -> SLF4J + Logback (стандарт) -> Log4j 2.x (альтернатива для async). Частая ошибка — путать Log4j 1.x и Log4j 2.x или не знать про Log4Shell.

[к оглавлению](#Журналирование)

---

## Как собирать и анализировать логи в production?
<!-- grade: senior -->

Принцип production-логирования: приложение пишет логи в stdout (JSON), инфраструктура собирает и доставляет их в систему агрегации для поиска, визуализации и алертинга.

### Популярные стеки агрегации логов

| Стек | Компоненты | Особенности |
|------|-----------|-------------|
| ELK | Elasticsearch + Logstash + Kibana | Мощный полнотекстовый поиск, высокое потребление ресурсов |
| Grafana Loki | Promtail + Loki + Grafana | Легковесный, индексирует только метки, дешевле ELK |
| Cloud-native | CloudWatch (AWS) / Cloud Logging (GCP) / Azure Monitor | Минимальная настройка, vendor lock-in |

### Архитектура сбора логов

```
ELK:         Приложение -> stdout (JSON) -> Filebeat/Logstash -> Elasticsearch -> Kibana (UI)
Loki:        Приложение -> stdout (JSON) -> Promtail -> Loki -> Grafana (UI)
Cloud:       Приложение -> stdout -> CloudWatch / Cloud Logging / Azure Monitor
```

### Логирование в Kubernetes

В Kubernetes стандартный подход — вывод логов в stdout:

- Приложение пишет в stdout — kubelet собирает автоматически
- Доступ через `kubectl logs` для быстрой диагностики
- Для агрегации: DaemonSet с Promtail/Fluentd на каждой ноде читает `/var/log/containers/*.log` и отправляет в систему агрегации

Файловое логирование внутри контейнера — антипаттерн: при рестарте пода файлы теряются.

### Correlation ID для distributed tracing

В микросервисной архитектуре один запрос проходит через несколько сервисов. Для связывания логов используется correlation ID:

```java
// Заголовок X-Request-ID проходит через все микросервисы
// Каждый сервис добавляет его в MDC -> в логи -> в систему агрегации
// В Kibana/Grafana: фильтр по requestId показывает путь запроса через все сервисы
```

Spring Boot 3.x + Micrometer Tracing автоматизирует этот процесс:
- Генерирует traceId и spanId
- Пробрасывает через HTTP-заголовки (W3C Trace Context)
- Добавляет в MDC, откуда попадает в логи

### Retention policy

Определите сроки хранения для разных уровней:

| Уровень | Рекомендуемый срок хранения |
|---------|---------------------------|
| DEBUG/TRACE | 3-7 дней (если вообще включены) |
| INFO | 30-90 дней |
| WARN | 90 дней |
| ERROR | 1 год |

### Частые ошибки

- Логи в файлы внутри контейнера — при рестарте пода файлы теряются; пишите в stdout
- Отсутствие retention policy — логи растут бесконечно, Elasticsearch потребляет терабайты
- Нет алертинга на ERROR — логи собираются, но никто не настроил уведомления
- Отсутствие correlation ID — невозможно проследить путь запроса через микросервисы

### Как используется в 2026

- Grafana Loki вытесняет ELK для многих проектов (проще, дешевле)
- OpenTelemetry унифицирует логи, метрики и трейсы в единый стандарт
- Spring Boot 3.x + Micrometer — автоматическая интеграция с Grafana/Prometheus/Tempo

> **На собеседовании:** ключевые темы — stdout в Kubernetes (не файлы), structured JSON для машинного парсинга, correlation ID для distributed tracing. Частая ошибка — описать только ELK, не упомянув Loki и OpenTelemetry как современные альтернативы.

[к оглавлению](#Журналирование)

---

[Вопросы для собеседования](README.md)
