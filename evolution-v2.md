[Вопросы для собеседования](README.md)

# Современная разработка WEB-приложений

+ [Какую архитектуру выбрать для нового Java-проекта?](#какую-архитектуру-выбрать-для-нового-java-проекта)
+ [Как инициализировать современный Java-проект?](#как-инициализировать-современный-java-проект)
+ [Как проектировать REST API в Spring Boot?](#как-проектировать-rest-api-в-spring-boot)
+ [Какие HTTP-клиенты используются в Spring в 2026 году?](#какие-http-клиенты-используются-в-spring-в-2026-году)
+ [Как организовать работу с данными в Spring Boot?](#как-организовать-работу-с-данными-в-spring-boot)
+ [Как управлять миграциями базы данных?](#как-управлять-миграциями-базы-данных)
+ [Как настроить безопасность REST API с помощью Spring Security?](#как-настроить-безопасность-rest-api-с-помощью-spring-security)
+ [Как работает OAuth 2.1 и JWT в микросервисной архитектуре?](#как-работает-oauth-21-и-jwt-в-микросервисной-архитектуре)
+ [Как организовать асинхронное взаимодействие через Kafka?](#как-организовать-асинхронное-взаимодействие-через-kafka)
+ [Что такое Virtual Threads и как они меняют разработку?](#что-такое-virtual-threads-и-как-они-меняют-разработку)
+ [Как обеспечить наблюдаемость приложения?](#как-обеспечить-наблюдаемость-приложения)
+ [Как организовать тестирование в современном Java-проекте?](#как-организовать-тестирование-в-современном-java-проекте)
+ [Как устроен CI/CD pipeline для Java-приложения?](#как-устроен-cicd-pipeline-для-java-приложения)
+ [Как контейнеризировать и деплоить Java-приложение?](#как-контейнеризировать-и-деплоить-java-приложение)
+ [Как документировать API и архитектурные решения?](#как-документировать-api-и-архитектурные-решения)

## Какую архитектуру выбрать для нового Java-проекта?
<!-- grade: middle -->

Архитектурный выбор для нового проекта сводится к трём основным вариантам: модульный монолит, микросервисы и cloud-native подход к проектированию.

> Аналогия из жизни: архитектура проекта похожа на планировку дома. Можно жить в большом доме с комнатами (модульный монолит) или в коттеджном посёлке из отдельных домиков (микросервисы) — второй вариант даёт независимость, но требует дороги, коммуникации и управляющую компанию.

### Типичный стек 2026 года

| Слой | Технология |
|------|-----------|
| Язык | Java 21 LTS / Java 25 |
| Фреймворк | Spring Boot 3.x (Spring Framework 6.x) |
| API | REST (OpenAPI 3.1), gRPC, GraphQL |
| Данные | PostgreSQL, Redis, MongoDB |
| Миграции | Liquibase / Flyway |
| Безопасность | Spring Security 6, OAuth 2.1, Keycloak |
| Обмен сообщениями | Apache Kafka, RabbitMQ |
| Наблюдаемость | OpenTelemetry, Micrometer, Grafana, Prometheus |
| Тестирование | JUnit 5, Testcontainers, Mockito, ArchUnit |
| Сборка | Gradle 8.x / Maven 3.9+ |
| CI/CD | GitHub Actions / GitLab CI |
| Контейнеризация | Docker, Kubernetes, Helm |

### Модульный монолит vs микросервисы

| Критерий | Модульный монолит | Микросервисы |
|----------|-------------------|-------------|
| Команда | До 15-20 разработчиков | Несколько автономных команд |
| Домен | Ещё не до конца понят | Чёткие доменные границы |
| Масштабирование | Единое | Независимое по компонентам |
| DevOps | Нет выделенной команды | Готовность к операционной сложности |
| Деплой | Единый артефакт | Независимый деплой компонентов |

### Cloud-native подход

Cloud-native означает, что приложение с самого начала проектируется для работы в облаке:
- Конфигурация через переменные окружения (12-Factor App)
- Stateless-сервисы с внешним хранением состояния
- Health checks для оркестратора
- Graceful shutdown
- Горизонтальное масштабирование

### Частые ошибки

- Выбор микросервисов "потому что так делают в Netflix" — у Netflix 2000+ инженеров и собственная инфраструктурная платформа
- Отсутствие чётких границ модулей в монолите — получается Big Ball of Mud
- Игнорирование операционной сложности микросервисов: каждый сервис это отдельный CI/CD pipeline, мониторинг, алертинг
- Преждевременная оптимизация архитектуры вместо фокуса на бизнес-логике

### Как используется в 2026

В 2026 году отчётливо виден тренд на модульный монолит как стартовую архитектуру даже в крупных компаниях. Spring Modulith предоставляет инструменты для enforce границ модулей, генерации документации и управления событиями между модулями.

> **На собеседовании:** интервьюер ожидает не просто перечисление "монолит vs микросервисы", а обоснованный выбор для конкретного контекста. Сильный ответ: "Начинаем с модульного монолита, потому что домен ещё не понят, и выделяем сервисы при реальной необходимости". Упоминание cloud-native принципов и 12-Factor App добавляет балл.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как инициализировать современный Java-проект?
<!-- grade: junior -->

Инициализация современного Java-проекта начинается с выбора Java 21 LTS, Spring Boot 3.x и системы сборки Gradle с Kotlin DSL.

### Ключевые возможности Java 21+

<details>
<summary>Примеры возможностей Java 21</summary>

```java
// Virtual Threads (Project Loom) — Java 21
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    List<Future<String>> futures = urls.stream()
        .map(url -> executor.submit(() -> fetchData(url)))
        .toList();
}

// Pattern Matching for switch — Java 21
String describe(Object obj) {
    return switch (obj) {
        case Integer i when i > 0 -> "positive: " + i;
        case String s -> "string: " + s;
        case null -> "null value";
        default -> "unknown: " + obj;
    };
}

// Record Patterns — Java 21
record Point(int x, int y) {}
record Line(Point start, Point end) {}

void printLength(Object obj) {
    if (obj instanceof Line(Point(var x1, var y1), Point(var x2, var y2))) {
        double length = Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
        System.out.println("Length: " + length);
    }
}

// Sequenced Collections — Java 21
SequencedCollection<String> list = new ArrayList<>();
list.addFirst("first");
list.addLast("last");
String first = list.getFirst();
```

</details>

### Spring Boot 3.x стартер с Gradle

<details>
<summary>build.gradle.kts</summary>

```kotlin
plugins {
    java
    id("org.springframework.boot") version "3.4.1"
    id("io.spring.dependency-management") version "1.1.7"
}

group = "com.example"
version = "0.0.1-SNAPSHOT"

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    implementation("org.springframework.boot:spring-boot-starter-validation")
    implementation("org.springframework.boot:spring-boot-starter-actuator")
    implementation("org.springframework.boot:spring-boot-starter-security")

    runtimeOnly("org.postgresql:postgresql")
    runtimeOnly("io.micrometer:micrometer-registry-prometheus")

    compileOnly("org.projectlombok:lombok")
    annotationProcessor("org.projectlombok:lombok")

    testImplementation("org.springframework.boot:spring-boot-starter-test")
    testImplementation("org.springframework.security:spring-security-test")
    testImplementation("org.testcontainers:junit-jupiter")
    testImplementation("org.testcontainers:postgresql")
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

</details>

### Структура проекта (гексагональная архитектура)

Гексагональная (порты и адаптеры) архитектура изолирует бизнес-логику от инфраструктуры:

```
order-service/
├── src/main/java/com/example/order/
│   ├── domain/                    # Ядро (нет зависимостей на фреймворки)
│   │   ├── model/                 # Entity, Value Object, Enum
│   │   ├── port/in/               # Входные порты (use cases)
│   │   ├── port/out/              # Выходные порты (SPI)
│   │   ├── service/               # Реализация use cases
│   │   └── exception/
│   ├── adapter/                   # Адаптеры (инфраструктура)
│   │   ├── in/web/                # REST контроллеры, DTO, mapper
│   │   ├── in/kafka/              # Kafka consumers
│   │   └── out/                   # JPA, внешние сервисы, messaging
│   └── config/                    # Конфигурация Spring
├── src/main/resources/
│   ├── application.yml
│   ├── application-local.yml
│   └── db/migration/              # Flyway миграции
```

### Gradle vs Maven в 2026

| Критерий | Gradle | Maven |
|----------|--------|-------|
| Build cache | Инкрементальная компиляция, build cache | Нет нативного build cache |
| DSL | Kotlin DSL с типобезопасностью | XML |
| Multi-module | Гибкость для сложных проектов | Конвенция над конфигурацией |
| Популярность | Основной для новых проектов | Legacy и enterprise со стандартизацией |

### Spring Initializr

```bash
curl https://start.spring.io/starter.zip \
  -d type=gradle-project \
  -d language=java \
  -d bootVersion=3.4.1 \
  -d javaVersion=21 \
  -d groupId=com.example \
  -d artifactId=order-service \
  -d dependencies=web,data-jpa,postgresql,security,actuator,validation \
  -o order-service.zip
```

### Частые ошибки

- Использование Java 8 или 11 в новых проектах — нет причин не использовать Java 21
- Структурирование по техническим слоям (controllers/, services/) вместо доменных модулей
- Смешивание DTO и доменных моделей: JPA-сущность не должна возвращаться напрямую из REST API
- Отсутствие файлов профилей (application-local.yml, application-prod.yml)

> **На собеседовании:** покажите, что знаете актуальные версии (Java 21, Spring Boot 3.x) и умеете обосновать структуру проекта. Частый вопрос: "Зачем гексагональная архитектура, а не трёхслойная?" Ответ: для простых CRUD трёхслойная подходит, для сложного домена гексагональная изолирует бизнес-логику от инфраструктуры.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как проектировать REST API в Spring Boot?
<!-- grade: middle -->

Проектирование REST API в 2026 году строится по принципу API-first: сначала контракт (OpenAPI 3.1), потом реализация.

### OpenAPI 3.1 спецификация

<details>
<summary>Пример OpenAPI-спецификации</summary>

```yaml
openapi: 3.1.0
info:
  title: Order Service API
  version: 1.0.0

paths:
  /orders:
    post:
      operationId: createOrder
      summary: Создать новый заказ
      tags: [Orders]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
      responses:
        '201':
          description: Заказ создан
          headers:
            Location:
              schema:
                type: string
                format: uri
        '400':
          $ref: '#/components/responses/BadRequest'

  /orders/{orderId}:
    get:
      operationId: getOrder
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Успешный ответ
        '404':
          $ref: '#/components/responses/NotFound'
```

</details>

### Контроллер с версионированием API

Версионирование через URL-путь (/api/v1/orders) — наиболее простой и распространённый подход:

```java
@RestController
@RequestMapping("/api/v1/orders")
public class OrderController {

    private final CreateOrderUseCase createOrderUseCase;
    private final OrderDtoMapper mapper;

    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(
            @Valid @RequestBody CreateOrderRequest request) {
        Order order = createOrderUseCase.create(mapper.toDomain(request));
        URI location = URI.create("/api/v1/orders/" + order.getId());
        return ResponseEntity.created(location).body(mapper.toResponse(order));
    }

    @GetMapping("/{orderId}")
    public OrderResponse getOrder(@PathVariable UUID orderId) {
        return mapper.toResponse(getOrderUseCase.getById(orderId));
    }
}
```

### Problem Detail (RFC 9457) для ошибок

Spring Boot 3.x нативно поддерживает стандарт Problem Detail:

<details>
<summary>GlobalExceptionHandler</summary>

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(OrderNotFoundException.class)
    public ProblemDetail handleOrderNotFound(OrderNotFoundException ex) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
            HttpStatus.NOT_FOUND, ex.getMessage());
        problem.setTitle("Заказ не найден");
        problem.setType(URI.create("https://api.example.com/errors/order-not-found"));
        problem.setProperty("orderId", ex.getOrderId());
        return problem;
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ProblemDetail handleValidation(MethodArgumentNotValidException ex) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
            HttpStatus.BAD_REQUEST, "Ошибка валидации");
        Map<String, String> errors = ex.getBindingResult().getFieldErrors().stream()
            .collect(Collectors.toMap(
                FieldError::getField,
                fe -> fe.getDefaultMessage() != null ? fe.getDefaultMessage() : "invalid",
                (a, b) -> a));
        problem.setProperty("fieldErrors", errors);
        return problem;
    }
}
```

</details>

### REST vs gRPC vs GraphQL

| Критерий | REST | gRPC | GraphQL |
|----------|------|------|---------|
| Формат | JSON | Protobuf (бинарный) | JSON |
| Контракт | OpenAPI 3.1 | .proto файлы | Schema |
| Производительность | Средняя | Высокая | Средняя |
| Подходит для | Public API, CRUD | Inter-service, высоконагруженные | Гибкие клиентские запросы |
| Streaming | Нет (SSE/WS) | Да (native) | Subscriptions |

### Частые ошибки

- Возврат доменных сущностей напрямую из API вместо DTO — утечка внутренней структуры
- Отсутствие версионирования API — любое breaking change ломает клиентов
- Использование HTTP 200 для всех ответов с кодом ошибки в теле — нарушение семантики HTTP
- Отсутствие пагинации для коллекций

> **На собеседовании:** интервьюер часто спрашивает про обработку ошибок в API. Упомяните Problem Detail (RFC 9457) и покажите, что знаете разницу между 400 (валидация) и 422 (бизнес-ошибка). Вопрос "REST или gRPC?" — ответ зависит от контекста: REST для public API, gRPC для inter-service.

[к оглавлению](#современная-разработка-web-приложений)

---

## Какие HTTP-клиенты используются в Spring в 2026 году?
<!-- grade: middle -->

RestClient — рекомендованный HTTP-клиент для синхронных Spring-приложений, пришедший на замену RestTemplate. Он сочетает fluent API и синхронную модель выполнения.

### Эволюция HTTP-клиентов

| Клиент | Версия | Тип | Статус в 2026 |
|--------|--------|-----|---------------|
| RestTemplate | Spring 3.0 (2009) | Синхронный, шаблонный | Maintenance |
| WebClient | Spring 5.0 (2017) | Реактивный / блокирующий | Для реактивных приложений |
| RestClient | Spring 6.1 (2023) | Синхронный, fluent API | Рекомендован |
| HTTP Interface | Spring 6.0 (2022) | Декларативный (как Feign) | Набирает популярность |

### RestClient — конфигурация и использование

<details>
<summary>Пример RestClient</summary>

```java
@Configuration
public class HttpClientConfig {

    @Bean
    public RestClient paymentRestClient(RestClient.Builder builder) {
        return builder
            .baseUrl("https://payment-service.internal")
            .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
            .requestInterceptor((request, body, execution) -> {
                request.getHeaders().set("X-Trace-Id", MDC.get("traceId"));
                return execution.execute(request, body);
            })
            .defaultStatusHandler(
                HttpStatusCode::is5xxServerError,
                (request, response) -> {
                    throw new PaymentServiceUnavailableException(
                        "Payment service returned " + response.getStatusCode());
                })
            .build();
    }
}

@Component
public class PaymentGatewayAdapter implements PaymentGateway {

    private final RestClient restClient;

    public PaymentResult charge(UUID orderId, Money amount) {
        return restClient.post()
            .uri("/api/v1/charges")
            .body(new ChargeRequest(orderId, amount.value(), amount.currency()))
            .retrieve()
            .body(PaymentResult.class);
    }
}
```

</details>

### Декларативный HTTP Interface

HTTP Interface работает аналогично Feign, но на базе RestClient:

```java
public interface PaymentServiceClient {

    @PostExchange("/api/v1/charges")
    PaymentResult charge(@RequestBody ChargeRequest request);

    @GetExchange("/api/v1/charges/{id}/status")
    PaymentStatus getStatus(@PathVariable UUID id);
}
```

### Частые ошибки

- Использование RestTemplate в новых проектах — он в режиме maintenance
- Отсутствие обработки ошибок и таймаутов в HTTP-клиенте
- Отсутствие передачи trace ID между сервисами

> **На собеседовании:** ключевой вопрос: "Чем RestClient отличается от WebClient?" RestClient — синхронный fluent API, WebClient — реактивный. С Virtual Threads RestClient даёт ту же масштабируемость, что и WebClient, но с простым синхронным кодом. Упоминание HTTP Interface как декларативной альтернативы Feign покажет, что вы следите за экосистемой.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как организовать работу с данными в Spring Boot?
<!-- grade: middle -->

Spring Data JPA с Hibernate 6 остаётся основным способом работы с реляционными данными. PostgreSQL занимает позицию дефолтной базы данных для Java-проектов.

### JPA-сущность

<details>
<summary>Пример JPA-сущности с optimistic locking</summary>

```java
@Entity
@Table(name = "orders")
public class OrderJpaEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    @Column(nullable = false)
    private UUID customerId;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private OrderStatus status;

    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<OrderItemJpaEntity> items = new ArrayList<>();

    @Column(nullable = false, precision = 19, scale = 2)
    private BigDecimal totalAmount;

    @CreationTimestamp
    @Column(updatable = false)
    private Instant createdAt;

    @UpdateTimestamp
    private Instant updatedAt;

    @Version
    private Long version; // Optimistic locking

    public void addItem(OrderItemJpaEntity item) {
        items.add(item);
        item.setOrder(this);
        recalculateTotal();
    }
}
```

</details>

### Репозиторий с различными типами запросов

<details>
<summary>Пример репозитория</summary>

```java
public interface OrderJpaRepository extends JpaRepository<OrderJpaEntity, UUID> {

    // Derived query
    List<OrderJpaEntity> findByCustomerIdAndStatus(UUID customerId, OrderStatus status);

    // JPQL с проекцией
    @Query("""
        SELECT new com.example.order.adapter.out.persistence.OrderSummary(
            o.id, o.status, o.totalAmount, o.createdAt
        )
        FROM OrderJpaEntity o
        WHERE o.customerId = :customerId
        ORDER BY o.createdAt DESC
        """)
    Page<OrderSummary> findOrderSummaries(
        @Param("customerId") UUID customerId, Pageable pageable);

    // Native query
    @Query(value = """
        SELECT o.* FROM orders o
        WHERE o.status = 'CREATED'
        AND o.created_at < NOW() - INTERVAL '30 minutes'
        FOR UPDATE SKIP LOCKED
        LIMIT :limit
        """, nativeQuery = true)
    List<OrderJpaEntity> findStaleOrders(@Param("limit") int limit);

    // Batch update
    @Modifying
    @Query("UPDATE OrderJpaEntity o SET o.status = :status WHERE o.id IN :ids")
    int updateStatusBatch(@Param("ids") List<UUID> ids, @Param("status") OrderStatus status);
}
```

</details>

### Настройка Hibernate для production

```yaml
spring:
  jpa:
    open-in-view: false  # Отключить ОБЯЗАТЕЛЬНО
    hibernate:
      ddl-auto: validate  # Только валидация, миграции через Flyway
    properties:
      hibernate:
        default_batch_fetch_size: 20
        jdbc:
          batch_size: 50
        order_inserts: true
        order_updates: true
```

### Redis для кэширования

```java
@Service
public class ProductService {

    @Cacheable(value = "products", key = "#productId")
    public Product getProduct(UUID productId) {
        return productRepository.findById(productId)
            .orElseThrow(() -> new ProductNotFoundException(productId));
    }

    @CacheEvict(value = "products", key = "#product.id")
    public Product updateProduct(Product product) {
        return productRepository.save(product);
    }
}
```

### Частые ошибки

- N+1 проблема: загрузка связанных сущностей в цикле. Решение: @EntityGraph, JOIN FETCH, default_batch_fetch_size
- Использование ddl-auto=update в production — потенциальная потеря данных
- Кэширование без стратегии инвалидации
- Игнорирование connection pool настроек — дефолтный HikariCP pool size (10) может быть недостаточен
- open-in-view=true (значение по умолчанию) — держит транзакцию открытой на весь HTTP-запрос

> **На собеседовании:** три обязательных пункта: 1) open-in-view=false, 2) ddl-auto=validate в production, 3) знание N+1 проблемы и способов решения. Без этих знаний разговор о JPA на middle-уровне не пройдёт. Бонус: упомянуть, что Virtual Threads снизили необходимость в R2DBC.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как управлять миграциями базы данных?
<!-- grade: junior -->

Миграции базы данных — это версионированные изменения схемы БД, которые применяются автоматически при старте приложения через Flyway или Liquibase.

> Аналогия из жизни: миграции БД похожи на историю коммитов в Git, но для структуры базы данных. Каждая миграция — это "коммит", который невозможно изменить после применения.

### Пример Flyway-миграции

<details>
<summary>SQL-миграции</summary>

```sql
-- V1__create_orders_table.sql
CREATE TABLE orders (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    customer_id UUID         NOT NULL,
    status      VARCHAR(20)  NOT NULL DEFAULT 'CREATED',
    total_amount DECIMAL(19,2) NOT NULL DEFAULT 0,
    created_at  TIMESTAMPTZ  NOT NULL DEFAULT NOW(),
    updated_at  TIMESTAMPTZ  NOT NULL DEFAULT NOW(),
    version     BIGINT       NOT NULL DEFAULT 0,
    CONSTRAINT chk_status CHECK (status IN ('CREATED','CONFIRMED','SHIPPED','DELIVERED','CANCELLED'))
);

CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at);

-- V2__create_order_items_table.sql
CREATE TABLE order_items (
    id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id   UUID NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
    product_id UUID NOT NULL,
    quantity   INTEGER NOT NULL CHECK (quantity > 0),
    price      DECIMAL(19,2) NOT NULL CHECK (price >= 0),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_order_items_order_id ON order_items(order_id);
```

</details>

### Конфигурация

```yaml
spring:
  flyway:
    enabled: true
    locations: classpath:db/migration
    baseline-on-migrate: true
    validate-on-migrate: true
```

### Flyway vs Liquibase

| Критерий | Flyway | Liquibase |
|----------|--------|-----------|
| Формат | SQL-файлы | XML / YAML / SQL |
| Rollback | Ручной (отдельная миграция) | Автоматический |
| Сложность | Простой | Больше возможностей |
| Популярность | Лидер для Java-проектов | Enterprise с XML-стеком |

### PostgreSQL — почему стандарт де-факто

PostgreSQL доминирует в Java-экосистеме благодаря: полная поддержка ACID, расширенные типы данных (JSONB, UUID, ARRAY), полнотекстовый поиск без внешних систем, надёжные managed-сервисы (AWS RDS, GCP Cloud SQL, Neon).

```sql
-- Использование JSONB для гибких атрибутов
ALTER TABLE orders ADD COLUMN metadata JSONB DEFAULT '{}';
CREATE INDEX idx_orders_metadata ON orders USING GIN (metadata);
```

### Частые ошибки

- ddl-auto=update в production — непредсказуемые изменения схемы
- Отсутствие индексов для часто используемых запросов
- Изменение уже применённых миграций — нарушает контрольную сумму

> **На собеседовании:** ключевое: "Миграции только через Flyway/Liquibase, никогда через ddl-auto=update в production". Если спросят про выбор между Flyway и Liquibase — Flyway проще и популярнее, Liquibase даёт автоматический rollback и XML-формат для enterprise.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как настроить безопасность REST API с помощью Spring Security?
<!-- grade: middle -->

Spring Security 6 полностью перешёл на компонентную модель с SecurityFilterChain, заменив устаревший WebSecurityConfigurerAdapter.

<details>
<summary>Конфигурация SecurityFilterChain</summary>

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(csrf -> csrf.disable()) // Для stateless API
            .cors(cors -> cors.configurationSource(corsConfigurationSource()))
            .sessionManagement(session ->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/v1/public/**").permitAll()
                .requestMatchers("/actuator/health", "/actuator/info").permitAll()
                .requestMatchers(HttpMethod.POST, "/api/v1/orders").hasRole("USER")
                .requestMatchers("/api/v1/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthConverter()))
            )
            .build();
    }

    @Bean
    public JwtAuthenticationConverter jwtAuthConverter() {
        JwtGrantedAuthoritiesConverter grantedAuthoritiesConverter =
            new JwtGrantedAuthoritiesConverter();
        grantedAuthoritiesConverter.setAuthoritiesClaimName("roles");
        grantedAuthoritiesConverter.setAuthorityPrefix("ROLE_");

        JwtAuthenticationConverter converter = new JwtAuthenticationConverter();
        converter.setJwtGrantedAuthoritiesConverter(grantedAuthoritiesConverter);
        return converter;
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowedOrigins(List.of("https://app.example.com"));
        config.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE"));
        config.setAllowedHeaders(List.of("Authorization", "Content-Type"));
        config.setMaxAge(3600L);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/api/**", config);
        return source;
    }
}
```

</details>

### Rate Limiting (Bucket4j + Redis)

<details>
<summary>RateLimitInterceptor</summary>

```java
@Component
public class RateLimitInterceptor implements HandlerInterceptor {

    private final ProxyManager<String> proxyManager;

    @Override
    public boolean preHandle(HttpServletRequest request,
            HttpServletResponse response, Object handler) {
        String clientId = extractClientId(request);

        Bucket bucket = proxyManager.builder()
            .build(clientId, () -> BucketConfiguration.builder()
                .addLimit(Bandwidth.builder()
                    .capacity(100)
                    .refillGreedy(100, Duration.ofMinutes(1))
                    .build())
                .build());

        if (bucket.tryConsume(1)) {
            return true;
        }
        response.setStatus(429);
        return false;
    }
}
```

</details>

### Ключевые принципы

- Stateless API: SessionCreationPolicy.STATELESS + JWT. Никаких HTTP-сессий
- CSRF отключается для stateless REST API. Для Thymeleaf CSRF обязателен
- Method Security (@PreAuthorize, @Secured) — для fine-grained авторизации
- Secrets никогда не хранятся в коде — переменные окружения, Vault, Kubernetes Secrets

### Частые ошибки

- Хранение JWT в localStorage — уязвимость к XSS. Для браузеров: HttpOnly cookie с SameSite=Strict
- Отсутствие rate limiting — уязвимость к DDoS и brute-force
- Логирование токенов и паролей — критическая уязвимость
- Использование @Secured("ROLE_ADMIN") без юнит-тестов на авторизацию

> **На собеседовании:** покажите, что знаете новый API (SecurityFilterChain вместо WebSecurityConfigurerAdapter). Обязательные пункты: STATELESS для API, CSRF отключён для REST но включён для HTML, Method Security для бизнес-правил. Вопрос про хранение JWT в браузере — ответ: HttpOnly cookie, никогда localStorage.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как работает OAuth 2.1 и JWT в микросервисной архитектуре?
<!-- grade: senior -->

OAuth 2.1 — упрощённая и более безопасная версия OAuth 2.0, в которой обязателен PKCE для всех клиентов, запрещены implicit flow и password grant.

### JWT vs Opaque Tokens

| Критерий | JWT | Opaque Token |
|----------|-----|-------------|
| Валидация | Локально (по подписи) | Запрос к Authorization Server |
| Размер | Большой (payload в токене) | Маленький (ссылка) |
| Отзыв | Сложно (blacklist / short TTL) | Легко (удалить на сервере) |
| Информация | Содержит claims | Только идентификатор |
| Подходит для | Микросервисов (нет доп. запросов) | Монолитов с требованиями к безопасности |

Рекомендация: JWT для межсервисного взаимодействия, короткий TTL (5-15 минут), refresh token для обновления.

### Keycloak как Identity Provider

```yaml
# docker-compose.yml (dev-окружение)
services:
  keycloak:
    image: quay.io/keycloak/keycloak:26.0
    command: start-dev
    ports:
      - "8180:8080"
    environment:
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres:5432/keycloak
```

### Конфигурация Resource Server

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://keycloak.example.com/realms/my-app
          jwk-set-uri: https://keycloak.example.com/realms/my-app/protocol/openid-connect/certs
```

### Частые ошибки

- Длинный TTL для access token (часы, дни). Рекомендация: 5-15 минут
- Отсутствие PKCE для public клиентов
- Хранение секретов в коде или конфигурационных файлах

### Как используется в 2026

Keycloak остаётся лидером среди open-source Identity Provider. OAuth 2.1 формально стандартизирован. Spring Authorization Server зрелый продукт для случаев, когда нужен собственный Authorization Server.

> **На собеседовании:** ключевой вопрос senior-уровня: "Как отзывать JWT?" Правильный ответ: JWT по природе не отзываемый, поэтому используют короткий TTL (5-15 мин) + refresh token. Для критичных систем — blacklist в Redis. Упоминание OAuth 2.1 (vs 2.0) и PKCE показывает актуальные знания.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как организовать асинхронное взаимодействие через Kafka?
<!-- grade: middle -->

Apache Kafka — стандарт для event-driven взаимодействия между сервисами, обеспечивающий надёжную доставку сообщений с гарантией порядка в пределах partition.

> Аналогия из жизни: Kafka — это как журнал бухгалтерских проводок. Каждая запись добавляется в конец, никогда не удаляется (до истечения срока хранения), и любой аудитор (consumer) может прочитать историю с любого места.

### Kafka vs RabbitMQ

| Критерий | Apache Kafka | RabbitMQ |
|----------|-------------|----------|
| Модель | Лог (append-only, retention) | Очередь (удаляется после обработки) |
| Порядок | В пределах partition | В пределах очереди |
| Производительность | Миллионы msg/sec | Десятки тысяч msg/sec |
| Replay | Можно перечитать историю | Нет |
| Подходит для | Event Sourcing, Event Streaming | Task queues, RPC, сложная маршрутизация |

### Producer и Consumer

<details>
<summary>Пример Producer</summary>

```java
@Component
@RequiredArgsConstructor
public class KafkaOrderEventPublisher implements OrderEventPublisher {

    private final KafkaTemplate<String, Object> kafkaTemplate;

    @Override
    public void publishOrderCreated(Order order) {
        OrderCreatedEvent event = new OrderCreatedEvent(
            order.getId(), order.getCustomerId(),
            order.getTotalAmount(), Instant.now());

        kafkaTemplate.send("order.events", order.getId().toString(), event)
            .whenComplete((result, ex) -> {
                if (ex != null) {
                    log.error("Failed to publish event for order {}", order.getId(), ex);
                } else {
                    log.info("Published event for order {} to partition {} offset {}",
                        order.getId(),
                        result.getRecordMetadata().partition(),
                        result.getRecordMetadata().offset());
                }
            });
    }
}
```

</details>

<details>
<summary>Пример Consumer с ручным подтверждением</summary>

```java
@Component
@RequiredArgsConstructor
@Slf4j
public class PaymentEventConsumer {

    private final OrderService orderService;

    @KafkaListener(topics = "payment.events", groupId = "order-service")
    public void handlePaymentEvent(
            @Payload PaymentCompletedEvent event,
            @Header(KafkaHeaders.RECEIVED_PARTITION) int partition,
            @Header(KafkaHeaders.OFFSET) long offset,
            Acknowledgment acknowledgment) {

        log.info("Received PaymentCompletedEvent: orderId={}, partition={}, offset={}",
            event.orderId(), partition, offset);

        try {
            orderService.confirmOrder(event.orderId(), event.paymentId());
            acknowledgment.acknowledge();
        } catch (Exception e) {
            log.error("Failed to process event for order {}", event.orderId(), e);
            // Не подтверждаем — сообщение будет повторно обработано
        }
    }
}
```

</details>

### Transactional Outbox Pattern

Единственный надёжный способ гарантировать атомарность бизнес-операции и публикации события:

```java
@Transactional
public Order createOrder(CreateOrderCommand command) {
    Order order = Order.create(command);
    orderRepository.save(order);

    // Событие сохраняется в outbox-таблицу в той же транзакции
    OutboxEvent event = OutboxEvent.builder()
        .aggregateType("Order")
        .aggregateId(order.getId().toString())
        .eventType("OrderCreated")
        .payload(objectMapper.writeValueAsString(new OrderCreatedEvent(order)))
        .build();
    outboxRepository.save(event);
    return order;
}

// Отдельный scheduler читает из outbox и отправляет в Kafka
@Scheduled(fixedDelay = 1000)
@Transactional
public void publishOutboxEvents() {
    List<OutboxEvent> events = outboxRepository.findUnpublished(100);
    for (OutboxEvent event : events) {
        kafkaTemplate.send(event.getTopic(), event.getAggregateId(), event.getPayload());
        event.markPublished();
    }
}
```

### Spring Events для внутренней коммуникации

Для событий внутри одного приложения (модульный монолит):

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
@Async
public void onOrderCreated(OrderCreatedEvent event) {
    notificationService.sendOrderConfirmation(event.orderId());
}
```

### Частые ошибки

- Обработка без idempotency: consumer может получить одно сообщение несколько раз (at-least-once)
- Использование auto-commit: сообщение может быть потеряно при падении до обработки
- Публикация события до коммита транзакции (без Outbox): при откате событие уже отправлено

> **На собеседовании:** три ключевых пункта: 1) Kafka vs RabbitMQ — разные модели (лог vs очередь), 2) Transactional Outbox Pattern для гарантированной доставки, 3) идемпотентность consumer. Если спросят "Как гарантировать exactly-once?" — ответ: exactly-once семантика в Kafka достигается через idempotent producer + transactional consumer, но на практике проще проектировать idempotent обработку.

[к оглавлению](#современная-разработка-web-приложений)

---

## Что такое Virtual Threads и как они меняют разработку?
<!-- grade: middle -->

Virtual Threads (Project Loom, Java 21) — легковесные потоки, управляемые JVM, которые позволяют использовать простой синхронный код с масштабируемостью реактивного стека.

> Аналогия из жизни: обычные (platform) потоки — это дорогие сотрудники, которых можно нанять ограниченное количество. Virtual Threads — это волонтёры: их может быть миллион, и они "спят", пока ждут ответа, не потребляя ресурсов.

### Включение в Spring Boot

```yaml
spring:
  threads:
    virtual:
      enabled: true
```

Одна строка — и все HTTP-обработчики, Kafka listeners, scheduled tasks переключаются на виртуальные потоки.

### До и после Virtual Threads

```java
// До: реактивный стек для масштабируемости
public Mono<Order> getOrder(UUID id) {
    return orderRepository.findById(id)
        .switchIfEmpty(Mono.error(new OrderNotFoundException(id)))
        .flatMap(order -> enrichWithCustomer(order));
}

// После: простой синхронный код с той же масштабируемостью
public Order getOrder(UUID id) {
    Order order = orderRepository.findById(id)
        .orElseThrow(() -> new OrderNotFoundException(id));
    return enrichWithCustomer(order);
}
```

### Structured Concurrency (Java 24+)

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Subtask<User> userTask = scope.fork(() -> findUser(userId));
    Subtask<Order> orderTask = scope.fork(() -> findOrder(orderId));
    scope.join().throwIfFailed();
    return new UserOrder(userTask.get(), orderTask.get());
}
```

### Частые ошибки

- Блокирующие операции (synchronized) на виртуальных потоках: pinning виртуального потока к carrier thread. Используйте ReentrantLock вместо synchronized
- Попытка пулить виртуальные потоки — они создаются по одному на задачу, пул не нужен
- Предположение, что Virtual Threads ускоряют CPU-bound задачи — они помогают только для I/O-bound

### Как используется в 2026

Virtual Threads стали стандартом для Spring Boot приложений. Реактивный стек (WebFlux, R2DBC) остаётся релевантным для изначально реактивных систем, но для типичного enterprise-приложения Virtual Threads устраняют основную мотивацию использования реактивного подхода.

> **На собеседовании:** главное — объяснить разницу между platform threads и virtual threads: platform thread = обёртка над OS thread (ограничен тысячами), virtual thread = управляется JVM (миллионы). Упомяните pinning как главную проблему и ReentrantLock как решение. Бонус: "Virtual Threads не заменяют реактивный стек полностью — для backpressure и streaming WebFlux остаётся актуальным".

[к оглавлению](#современная-разработка-web-приложений)

---

## Как обеспечить наблюдаемость приложения?
<!-- grade: middle -->

Наблюдаемость (observability) — это способность понять внутреннее состояние системы по её внешним сигналам, которая строится на трёх столпах: метрики, логи и трейсы, связанных через trace ID.

> Аналогия из жизни: наблюдаемость — это как приборная панель автомобиля. Спидометр (метрики) показывает текущую скорость, бортовой журнал (логи) фиксирует события, а навигатор (трейсы) показывает маршрут запроса от начала до конца.

### Стек наблюдаемости

| Компонент | Инструмент | Назначение |
|-----------|-----------|-----------|
| Метрики | Prometheus + Micrometer | Числовые показатели (latency, throughput, errors) |
| Логи | Loki + Logback | Структурированные события |
| Трейсы | Tempo + OpenTelemetry | Путь запроса через сервисы |
| Визуализация | Grafana | Дашборды, алертинг |
| SDK | OpenTelemetry (OTel) | Вендор-нейтральный сбор телеметрии |

### Конфигурация

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, info, prometheus, metrics
  endpoint:
    health:
      probes:
        enabled: true  # Kubernetes: /actuator/health/liveness, /readiness
  metrics:
    tags:
      application: order-service
    distribution:
      percentiles-histogram:
        http.server.requests: true
  tracing:
    sampling:
      probability: 1.0  # 100% в dev, 10-50% в prod
```

### Кастомные метрики

```java
@Component
@RequiredArgsConstructor
public class OrderMetrics {

    private final MeterRegistry registry;

    public void recordOrderCreated(String status) {
        registry.counter("orders.created", "status", status).increment();
    }

    public void recordOrderProcessingTime(Duration duration) {
        registry.timer("orders.processing.time").record(duration);
    }
}
```

### Structured Logging (JSON)

В production логи должны быть в JSON-формате для парсинга Loki/Elasticsearch:

```json
{
  "timestamp": "2026-04-19T10:30:45.123Z",
  "level": "INFO",
  "logger": "c.e.order.adapter.in.web.OrderController",
  "message": "Order created successfully",
  "traceId": "abc123def456",
  "spanId": "789ghi",
  "orderId": "550e8400-e29b-41d4-a716-446655440000"
}
```

### Spring Boot Actuator — ключевые эндпоинты

| Endpoint | Назначение |
|----------|-----------|
| /actuator/health | Health check (liveness + readiness) |
| /actuator/health/liveness | Kubernetes liveness probe |
| /actuator/health/readiness | Kubernetes readiness probe |
| /actuator/prometheus | Метрики в формате Prometheus |
| /actuator/info | Информация о приложении |

### Частые ошибки

- 100% sampling в production: огромный объём данных трейсинга. Используйте 10-50%
- Логирование чувствительных данных: пароли, токены, персональные данные
- Мониторинг только инфраструктурных метрик (CPU, RAM) без бизнес-метрик
- Алертинг на каждую ошибку в логе вместо агрегированного error rate

> **На собеседовании:** назовите три столпа (метрики, логи, трейсы) и объясните, что они связаны через trace ID. Частый вопрос: "Чем отличается liveness от readiness probe?" Liveness — приложение живо (если нет — перезапуск), readiness — готово принимать трафик (если нет — убрать из балансировки). Упоминание OpenTelemetry как вендор-нейтрального стандарта добавляет очков.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как организовать тестирование в современном Java-проекте?
<!-- grade: middle -->

Стратегия тестирования в 2026 году строится вокруг пирамиды: unit-тесты (много, быстрые), slice-тесты и интеграционные (средне), E2E и contract-тесты (мало), плюс автоматические архитектурные проверки через ArchUnit.

### Пирамида тестирования

```
        /  E2E  \           <- Мало (Playwright, API tests)
       /  Интегр. \         <- Средне (Testcontainers, @SpringBootTest)
      / Slice-тесты \       <- Средне (@WebMvcTest, @DataJpaTest)
     /   Unit-тесты   \    <- Много (JUnit 5 + Mockito)
    /  Архитектурные    \   <- Автоматически (ArchUnit)
```

### Unit-тест бизнес-логики

<details>
<summary>OrderServiceTest</summary>

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock OrderRepository orderRepository;
    @Mock PaymentGateway paymentGateway;
    @Mock OrderEventPublisher eventPublisher;
    @InjectMocks OrderService orderService;

    @Test
    void shouldCreateOrderWithCorrectTotal() {
        CreateOrderCommand command = new CreateOrderCommand(
            UUID.randomUUID(),
            List.of(
                new OrderItemCommand(UUID.randomUUID(), 2, new Money("29.99", "USD")),
                new OrderItemCommand(UUID.randomUUID(), 1, new Money("49.99", "USD"))
            ));
        when(orderRepository.save(any(Order.class)))
            .thenAnswer(invocation -> invocation.getArgument(0));

        Order result = orderService.createOrder(command);

        assertThat(result.getStatus()).isEqualTo(OrderStatus.CREATED);
        assertThat(result.getTotalAmount()).isEqualByComparingTo(new BigDecimal("109.97"));
        verify(eventPublisher).publishOrderCreated(result);
        verify(paymentGateway, never()).charge(any(), any());
    }

    @ParameterizedTest
    @CsvSource({
        "CREATED,    CONFIRMED, true",
        "CONFIRMED,  SHIPPED,   true",
        "DELIVERED,  CANCELLED, false"
    })
    void shouldValidateStatusTransition(OrderStatus from, OrderStatus to, boolean allowed) {
        assertThat(from.canTransitionTo(to)).isEqualTo(allowed);
    }
}
```

</details>

### Testcontainers для интеграционных тестов

Testcontainers запускает реальные зависимости (PostgreSQL, Kafka, Redis) в Docker-контейнерах:

<details>
<summary>IntegrationTestBase</summary>

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Testcontainers
abstract class IntegrationTestBase {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17-alpine");

    @Container
    static KafkaContainer kafka = new KafkaContainer(
        DockerImageName.parse("confluentinc/cp-kafka:7.7.0"));

    @Container
    static GenericContainer<?> redis = new GenericContainer<>("redis:7-alpine")
        .withExposedPorts(6379);

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
        registry.add("spring.kafka.bootstrap-servers", kafka::getBootstrapServers);
        registry.add("spring.data.redis.host", redis::getHost);
        registry.add("spring.data.redis.port", () -> redis.getMappedPort(6379));
    }
}
```

</details>

### Slice-тесты

```java
// Тест контроллера (без запуска всего контекста)
@WebMvcTest(OrderController.class)
class OrderControllerTest {

    @Autowired MockMvc mockMvc;
    @MockBean CreateOrderUseCase createOrderUseCase;

    @Test
    void shouldReturn201WhenOrderCreated() throws Exception {
        when(createOrderUseCase.create(any())).thenReturn(order);

        mockMvc.perform(post("/api/v1/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{...}"))
            .andExpect(status().isCreated())
            .andExpect(header().exists("Location"));
    }
}
```

### ArchUnit для архитектурных правил

```java
@AnalyzeClasses(packages = "com.example.order")
class ArchitectureTest {

    @ArchTest
    static final ArchRule domainShouldNotDependOnAdapters =
        noClasses().that().resideInAPackage("..domain..")
            .should().dependOnClassesThat().resideInAPackage("..adapter..");

    @ArchTest
    static final ArchRule domainShouldNotDependOnSpring =
        noClasses().that().resideInAPackage("..domain..")
            .should().dependOnClassesThat().resideInAPackage("org.springframework..");
}
```

### Частые ошибки

- Тестирование на H2 вместо реальной БД: различия в SQL-диалектах приводят к пропущенным багам
- Отсутствие тестов на ошибочные сценарии: тестируется только happy path
- Mock всего подряд: unit-тест, который мокает всё, не тестирует ничего
- Запуск @SpringBootTest для каждого теста — используйте @WebMvcTest/@DataJpaTest где возможно

> **На собеседовании:** покажите знание пирамиды тестирования и объясните, когда какой тип используется. Ключевая фраза: "Testcontainers заменил H2 — тестируем на реальном PostgreSQL". Упоминание ArchUnit для автоматической проверки архитектурных правил показывает зрелость. Частый вопрос: "Чем @WebMvcTest отличается от @SpringBootTest?" — первый загружает только web-слой, второй поднимает весь контекст.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как устроен CI/CD pipeline для Java-приложения?
<!-- grade: middle -->

CI/CD pipeline — это автоматизированная цепочка сборки, тестирования и деплоя, которая для Java-приложения типично включает этапы: test, build, push image, deploy.

<details>
<summary>GitHub Actions CI/CD pipeline</summary>

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:17-alpine
        env:
          POSTGRES_DB: testdb
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432

    steps:
      - uses: actions/checkout@v4
      - name: Set up JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: 'gradle'
      - name: Run tests
        run: ./gradlew test
      - name: Run checkstyle
        run: ./gradlew checkstyleMain checkstyleTest

  build-and-push:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Build JAR
        run: ./gradlew bootJar
      - name: Build and push Docker image
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}:${{ github.sha }}

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Update Kubernetes manifest
        run: |
          cd k8s/overlays/production
          kustomize edit set image order-service=ghcr.io/${{ github.repository }}:${{ github.sha }}
```

</details>

### Docker Compose для dev-окружения

<details>
<summary>docker-compose.yml</summary>

```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: local
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/orderdb
      SPRING_KAFKA_BOOTSTRAP_SERVERS: kafka:9092
    depends_on:
      postgres:
        condition: service_healthy

  postgres:
    image: postgres:17-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: orderdb
      POSTGRES_USER: order
      POSTGRES_PASSWORD: order
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U order -d orderdb"]

  kafka:
    image: confluentinc/cp-kafka:7.7.0
    ports:
      - "9092:9092"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  prometheus:
    image: prom/prometheus:v2.55.0
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana:11.3.0
    ports:
      - "3000:3000"
```

</details>

### Ключевые принципы

- CI pipeline должен быть быстрым (менее 10 минут): кэширование зависимостей, параллельные джобы
- Тесты в CI детерминированные: только Testcontainers или service containers
- Docker Compose для локальной разработки полностью воспроизводит production-зависимости
- GitOps: изменение манифестов в git автоматически приводит к деплою

### Частые ошибки

- CI pipeline длиннее 15-20 минут: разработчики мёрджат без зелёного билда
- Отсутствие кэширования зависимостей: каждый билд скачивает все jar-файлы
- Ручной деплой: "Вася знает, как деплоить" — single point of failure
- Секреты в CI-файлах вместо GitHub Secrets / Vault

> **На собеседовании:** опишите этапы pipeline: checkout, test, build JAR, build Docker image, push to registry, deploy to K8s. Упоминание GitOps (ArgoCD) и кэширования Gradle показывает production-опыт. Вопрос "Как ускорить CI?" — ответ: Gradle build cache, параллельные джобы, тонкие Docker-слои.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как контейнеризировать и деплоить Java-приложение?
<!-- grade: middle -->

Контейнеризация Java-приложения строится на multi-stage Docker build с layered JAR, а деплой в production выполняется через Kubernetes с Helm/Kustomize.

### Multi-stage Dockerfile

<details>
<summary>Dockerfile</summary>

```dockerfile
# Stage 1: Build
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app
COPY build.gradle.kts settings.gradle.kts gradlew ./
COPY gradle/ gradle/
RUN ./gradlew dependencies --no-daemon

COPY src/ src/
RUN ./gradlew bootJar --no-daemon -x test

# Stage 2: Extract layers
FROM eclipse-temurin:21-jdk-alpine AS extractor
WORKDIR /app
COPY --from=builder /app/build/libs/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --launcher --destination extracted

# Stage 3: Runtime
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=extractor /app/extracted/dependencies/ ./
COPY --from=extractor /app/extracted/spring-boot-loader/ ./
COPY --from=extractor /app/extracted/snapshot-dependencies/ ./
COPY --from=extractor /app/extracted/application/ ./

USER appuser
EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --start-period=30s --retries=3 \
  CMD wget -qO- http://localhost:8080/actuator/health/liveness || exit 1

ENTRYPOINT ["java", "-XX:+UseZGC", "-XX:MaxRAMPercentage=75.0", \
  "org.springframework.boot.loader.launch.JarLauncher"]
```

</details>

### Kubernetes Deployment

<details>
<summary>deployment.yml</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: order-service
          image: ghcr.io/example/order-service:latest
          ports:
            - containerPort: 8080
          resources:
            requests:
              cpu: 250m
              memory: 512Mi
            limits:
              cpu: 1000m
              memory: 1Gi
          livenessProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 8080
            initialDelaySeconds: 15
          startupProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            failureThreshold: 30
          lifecycle:
            preStop:
              exec:
                command: ["sh", "-c", "sleep 10"]
      terminationGracePeriodSeconds: 60
```

</details>

### Graceful shutdown

```yaml
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
server:
  shutdown: graceful
```

Последовательность: SIGTERM -> preStop (sleep 10) -> readiness DOWN -> ожидание завершения запросов -> закрытие контекста.

### GraalVM Native Image

| Критерий | JVM | Native Image |
|----------|-----|-------------|
| Старт | 2-10 секунд | Менее 100 мс |
| Память | Средняя-высокая | Низкая |
| Peak performance | Выше (JIT) | Ниже (AOT) |
| Подходит для | Long-running сервисы | Serverless, CLI, FaaS |

### Частые ошибки

- Запуск контейнера от root — уязвимость
- Отсутствие resource limits: pod занимает весь node
- latest тег в production — неизвестно, какая версия запущена
- Отсутствие startup probe: liveness probe убивает pod, который ещё не стартовал
- MaxRAMPercentage=100% — не остаётся места для off-heap памяти, используйте 75%

> **На собеседовании:** обязательные знания: multi-stage build (зачем три этапа), layered JAR (кэширование Docker-слоёв), non-root user, три типа probe (startup, liveness, readiness) и их назначение. Вопрос "Почему не latest?" — ответ: невозможно откатить и понять, что запущено. Используйте SHA или semver.

[к оглавлению](#современная-разработка-web-приложений)

---

## Как документировать API и архитектурные решения?
<!-- grade: junior -->

Документация API генерируется автоматически из кода через springdoc-openapi, а архитектурные решения фиксируются в ADR (Architecture Decision Records) рядом с кодом.

> Аналогия из жизни: ADR — это как протокол заседания совета директоров. Через год никто не помнит, почему выбрали Kafka, а не RabbitMQ — но если есть протокол с аргументами, ответ очевиден.

### springdoc-openapi

```java
@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("Order Service API")
                .version("1.0.0")
                .description("API для управления заказами"))
            .addSecurityItem(new SecurityRequirement().addList("bearer-jwt"))
            .components(new Components()
                .addSecuritySchemes("bearer-jwt",
                    new SecurityScheme()
                        .type(SecurityScheme.Type.HTTP)
                        .scheme("bearer")
                        .bearerFormat("JWT")));
    }
}
```

### Аннотации на контроллере

```java
@RestController
@RequestMapping("/api/v1/orders")
@Tag(name = "Orders", description = "Управление заказами")
public class OrderController {

    @Operation(summary = "Создать заказ")
    @ApiResponses({
        @ApiResponse(responseCode = "201", description = "Заказ создан"),
        @ApiResponse(responseCode = "400", description = "Некорректный запрос",
            content = @Content(schema = @Schema(implementation = ProblemDetail.class)))
    })
    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(
            @Valid @RequestBody CreateOrderRequest request) { ... }
}
```

### Architecture Decision Records (ADR)

```markdown
# ADR-001: Использование PostgreSQL в качестве основной БД

Статус: Принято (2026-01-15)

Контекст: Нужно выбрать основную реляционную СУБД.

Решение: Используем PostgreSQL 17.

Причины:
- Лучшая поддержка JSONB для гибких атрибутов
- Managed-сервисы во всех облаках
- Лицензия (PostgreSQL License, аналог MIT)

Последствия:
- Миграции через Flyway
- Мониторинг через pg_stat_statements
```

### Структура документации в проекте

```
docs/
  adr/
    0001-use-postgresql.md
    0002-hexagonal-architecture.md
    0003-kafka-for-events.md
    template.md
```

### Ключевые принципы

- Документация API генерируется из кода (springdoc-openapi). Ручная документация устаревает
- ADR — минимально необходимая архитектурная документация. Фиксирует "почему", а не "что"
- README — точка входа. Если за 10 минут нельзя поднять проект, README плохой
- Docs-as-code: документация в git, рядом с кодом, версионируется вместе с ним

### Частые ошибки

- Ручное написание OpenAPI-спецификации, которая расходится с API
- Избыточная документация, которую никто не обновляет
- Отсутствие ADR: через год никто не помнит причины решений
- Документация только в Confluence: не версионируется с кодом

> **На собеседовании:** два ключевых пункта: 1) API-документация генерируется из кода (springdoc-openapi), 2) архитектурные решения фиксируются в ADR. Если спросят "Зачем ADR?" — ответ: фиксирует контекст и причины решений, которые дорого менять. Новый разработчик читает ADR и понимает "почему Kafka, а не RabbitMQ" без созвона с архитектором.

[к оглавлению](#современная-разработка-web-приложений)
