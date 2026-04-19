[Вопросы для собеседования](README.md)

# Современная разработка WEB-приложений

Данное руководство представляет собой структурированный гайд по сквозной (end-to-end) разработке современных веб-приложений на Java-стеке в 2026 году. Материал охватывает полный цикл: от инициализации проекта до деплоя в production.

## Введение: архитектура современного веб-приложения

Современное веб-приложение на Java — это не монолитный WAR-файл на сервере приложений. Это контейнеризированный сервис с декларативной инфраструктурой, автоматизированным CI/CD и полным стеком наблюдаемости.

Типичный стек 2026 года выглядит так:

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
| GitOps | ArgoCD |

**Микросервисная архитектура vs модульный монолит** — один из ключевых вопросов при старте проекта.

**Модульный монолит** — правильный выбор, когда:
- Команда до 15-20 разработчиков
- Домен ещё не до конца понят (нужна быстрая итерация)
- Нет выделенной DevOps-команды
- Стартап или MVP-фаза

**Микросервисы** оправданы, когда:
- Несколько автономных команд, каждая владеет своим доменом
- Части системы имеют кардинально разные требования к масштабированию
- Необходим независимый деплой компонентов
- Организация готова к операционной сложности (service mesh, distributed tracing, eventual consistency)

**Cloud-native подход** означает, что приложение с самого начала проектируется для работы в облаке:
- Конфигурация через переменные окружения (12-Factor App)
- Stateless-сервисы с внешним хранением состояния
- Health checks для оркестратора
- Graceful shutdown
- Горизонтальное масштабирование

### Важное
- Не начинайте с микросервисов. Модульный монолит — это зрелое архитектурное решение, а не компромисс. Границы модулей потом можно превратить в границы сервисов.
- Cloud-native — это не про Kubernetes. Это про принципы проектирования. Приложение, следующее 12-Factor App, будет работать и в Docker Compose, и в K8s, и на bare metal.
- Архитектура — это не про диаграммы. Это про решения, которые дорого менять. Фиксируйте их в ADR (Architecture Decision Records).

### Частые ошибки
- Выбор микросервисов «потому что так делают в Netflix». У Netflix 2000+ инженеров и собственная инфраструктурная платформа.
- Отсутствие чётких границ модулей в монолите: получается «большой ком грязи» (Big Ball of Mud).
- Игнорирование операционной сложности микросервисов: каждый сервис — это отдельный CI/CD pipeline, мониторинг, алертинг, логирование.
- Преждевременная оптимизация архитектуры вместо фокуса на бизнес-логике.

### Как используется в 2026
В 2026 году отчётливо виден тренд на **модульный монолит как стартовую архитектуру** даже в крупных компаниях. Spring Modulith предоставляет инструменты для enforce'а модульных границ, генерации документации модулей и управления событиями между модулями. Микросервисы остаются стандартом для крупных платформ с десятками команд, но порог входа в микросервисную архитектуру осознанно повышен: теперь компании сначала доказывают необходимость декомпозиции, а не начинают с неё по умолчанию.

[к оглавлению](#Современная-разработка-web-приложений)

## Инициализация проекта

Java 21 LTS — основной выбор для production в 2026. Java 25 (сентябрь 2025) приносит новые возможности, но LTS-версия остаётся стандартом для enterprise.

Ключевые возможности Java 21+, которые используются повседневно:

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
String last = list.getLast();

// Structured Concurrency (Preview -> стабилизация в Java 24+)
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Subtask<User> userTask = scope.fork(() -> findUser(userId));
    Subtask<Order> orderTask = scope.fork(() -> findOrder(orderId));
    scope.join().throwIfFailed();
    return new UserOrder(userTask.get(), orderTask.get());
}
```

Spring Boot 3.x стартер с Gradle:

```kotlin
// build.gradle.kts
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

Структура проекта (Hexagonal/Clean Architecture). Гексагональная (порты и адаптеры) архитектура изолирует бизнес-логику от инфраструктуры:

```
order-service/
├── build.gradle.kts
├── src/main/java/com/example/order/
│   ├── OrderServiceApplication.java
│   │
│   ├── domain/                          # Ядро (нет зависимостей на фреймворки)
│   │   ├── model/
│   │   │   ├── Order.java               # Entity / Aggregate Root
│   │   │   ├── OrderItem.java
│   │   │   ├── OrderStatus.java         # Enum
│   │   │   └── Money.java               # Value Object
│   │   ├── port/
│   │   │   ├── in/                      # Входные порты (use cases)
│   │   │   │   ├── CreateOrderUseCase.java
│   │   │   │   └── GetOrderUseCase.java
│   │   │   └── out/                     # Выходные порты (SPI)
│   │   │       ├── OrderRepository.java
│   │   │       ├── PaymentGateway.java
│   │   │       └── OrderEventPublisher.java
│   │   ├── service/
│   │   │   └── OrderService.java        # Реализация use cases
│   │   └── exception/
│   │       ├── OrderNotFoundException.java
│   │       └── InsufficientStockException.java
│   │
│   ├── adapter/                         # Адаптеры (инфраструктура)
│   │   ├── in/
│   │   │   ├── web/                     # REST контроллеры
│   │   │   │   ├── OrderController.java
│   │   │   │   ├── dto/
│   │   │   │   │   ├── CreateOrderRequest.java
│   │   │   │   │   └── OrderResponse.java
│   │   │   │   └── mapper/
│   │   │   │       └── OrderDtoMapper.java
│   │   │   └── kafka/                   # Kafka consumers
│   │   │       └── PaymentEventConsumer.java
│   │   └── out/
│   │       ├── persistence/             # JPA реализация
│   │       │   ├── JpaOrderRepository.java
│   │       │   ├── OrderJpaEntity.java
│   │       │   └── OrderJpaMapper.java
│   │       ├── payment/                 # Внешний сервис
│   │       │   └── PaymentGatewayAdapter.java
│   │       └── messaging/
│   │           └── KafkaOrderEventPublisher.java
│   │
│   └── config/                          # Конфигурация Spring
│       ├── SecurityConfig.java
│       ├── KafkaConfig.java
│       └── OpenApiConfig.java
│
├── src/main/resources/
│   ├── application.yml
│   ├── application-local.yml
│   ├── application-prod.yml
│   └── db/migration/                    # Flyway миграции
│       ├── V1__create_orders_table.sql
│       └── V2__add_order_items_table.sql
│
└── src/test/java/com/example/order/
    ├── domain/service/
    │   └── OrderServiceTest.java        # Unit-тесты
    ├── adapter/in/web/
    │   └── OrderControllerTest.java     # Slice-тесты
    └── integration/
        └── OrderIntegrationTest.java    # Интеграционные тесты
```

**Gradle vs Maven в 2026.** **Gradle** стал основным инструментом сборки для новых проектов: инкрементальная компиляция и build cache значительно ускоряют сборку, Kotlin DSL (`build.gradle.kts`) обеспечивает типобезопасность и автодополнение в IDE, гибкость для сложных multi-module проектов, build scans для диагностики проблем сборки. **Maven** по-прежнему используется в legacy-проектах, в организациях со стандартизированным Maven-стеком, и для простых проектов (конвенция над конфигурацией).

**Spring Initializr и шаблоны.** Быстрый старт через CLI:

```bash
# Через curl
curl https://start.spring.io/starter.zip \
  -d type=gradle-project \
  -d language=java \
  -d bootVersion=3.4.1 \
  -d javaVersion=21 \
  -d groupId=com.example \
  -d artifactId=order-service \
  -d dependencies=web,data-jpa,postgresql,security,actuator,validation \
  -o order-service.zip

# Или через Spring Boot CLI
spring init --type=gradle-project --java-version=21 \
  --dependencies=web,data-jpa,postgresql,security,actuator \
  order-service
```

### Важное
- Всегда используйте Java 21 LTS для production. Java 25 — для экспериментов и подготовки к миграции.
- Гексагональная архитектура — не самоцель. Для простых CRUD-сервисов классическая трёхслойная архитектура (Controller -> Service -> Repository) по-прежнему уместна.
- Домен не должен зависеть от Spring. Аннотации вроде `@Entity`, `@Transactional` допустимы прагматично, но в идеале JPA-маппинг вынесен в адаптер.

### Частые ошибки
- Использование Java 8 или 11 в новых проектах — нет причин не использовать Java 21.
- Структурирование по техническим слоям (`controllers/`, `services/`, `repositories/`) вместо по доменным модулям — затрудняет навигацию и последующую декомпозицию.
- Смешивание DTO и доменных моделей: JPA-сущность не должна возвращаться напрямую из REST API.
- Отсутствие файлов профилей (`application-local.yml`, `application-prod.yml`) — вся конфигурация оказывается в одном файле.

### Как используется в 2026
Spring Boot 3.4.x — текущая стабильная линия. Spring Framework 6.2 полностью поддерживает Virtual Threads, что позволяет использовать привычную синхронную модель программирования с производительностью, сопоставимой с реактивной. Spring Modulith набирает популярность для модульных монолитов, предоставляя enforcement границ модулей через ArchUnit, поддержку событий между модулями и автогенерацию документации.

[к оглавлению](#Современная-разработка-web-приложений)

## API Design

Современный REST API строится по принципу «API-first»: сначала контракт (OpenAPI), потом реализация.

**OpenAPI 3.1 спецификация:**

```yaml
# openapi.yml
openapi: 3.1.0
info:
  title: Order Service API
  version: 1.0.0
  description: API управления заказами

servers:
  - url: http://localhost:8080/api/v1

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
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OrderResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '422':
          $ref: '#/components/responses/UnprocessableEntity'

  /orders/{orderId}:
    get:
      operationId: getOrder
      summary: Получить заказ по ID
      tags: [Orders]
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
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OrderResponse'
        '404':
          $ref: '#/components/responses/NotFound'

components:
  schemas:
    CreateOrderRequest:
      type: object
      required: [customerId, items]
      properties:
        customerId:
          type: string
          format: uuid
        items:
          type: array
          minItems: 1
          items:
            $ref: '#/components/schemas/OrderItemRequest'

    OrderItemRequest:
      type: object
      required: [productId, quantity]
      properties:
        productId:
          type: string
          format: uuid
        quantity:
          type: integer
          minimum: 1

    OrderResponse:
      type: object
      properties:
        id:
          type: string
          format: uuid
        customerId:
          type: string
          format: uuid
        status:
          type: string
          enum: [CREATED, CONFIRMED, SHIPPED, DELIVERED, CANCELLED]
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItemResponse'
        totalAmount:
          type: number
          format: decimal
        createdAt:
          type: string
          format: date-time

  responses:
    BadRequest:
      description: Некорректный запрос
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ProblemDetail'
    NotFound:
      description: Ресурс не найден
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ProblemDetail'
    UnprocessableEntity:
      description: Бизнес-ошибка
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ProblemDetail'

    ProblemDetail:
      type: object
      properties:
        type:
          type: string
          format: uri
        title:
          type: string
        status:
          type: integer
        detail:
          type: string
        instance:
          type: string
          format: uri
```

**Версионирование API** — через URL-путь (`/api/v1/orders`) или заголовок. URL-путь — наиболее простой и распространённый подход:

```java
@RestController
@RequestMapping("/api/v1/orders")
public class OrderController {

    private final CreateOrderUseCase createOrderUseCase;
    private final GetOrderUseCase getOrderUseCase;
    private final OrderDtoMapper mapper;

    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(
            @Valid @RequestBody CreateOrderRequest request) {

        Order order = createOrderUseCase.create(mapper.toDomain(request));
        OrderResponse response = mapper.toResponse(order);

        URI location = URI.create("/api/v1/orders/" + order.getId());
        return ResponseEntity.created(location).body(response);
    }

    @GetMapping("/{orderId}")
    public OrderResponse getOrder(@PathVariable UUID orderId) {
        Order order = getOrderUseCase.getById(orderId);
        return mapper.toResponse(order);
    }
}
```

**Problem Detail (RFC 9457)** — стандарт для ошибок. Spring Boot 3.x поддерживает его нативно:

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
        problem.setTitle("Некорректный запрос");

        Map<String, String> errors = ex.getBindingResult().getFieldErrors().stream()
            .collect(Collectors.toMap(
                FieldError::getField,
                fe -> fe.getDefaultMessage() != null ? fe.getDefaultMessage() : "invalid",
                (a, b) -> a
            ));
        problem.setProperty("fieldErrors", errors);
        return problem;
    }
}
```

**RestClient — современный HTTP-клиент (Spring 6.1+).** RestClient — это синхронный HTTP-клиент, пришедший на замену RestTemplate. Он сочетает в себе fluent API (как у WebClient) и синхронную модель выполнения:

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
                }
            )
            .build();
    }
}

@Component
public class PaymentGatewayAdapter implements PaymentGateway {

    private final RestClient restClient;

    public PaymentGatewayAdapter(RestClient paymentRestClient) {
        this.restClient = paymentRestClient;
    }

    @Override
    public PaymentResult charge(UUID orderId, Money amount) {
        ChargeRequest request = new ChargeRequest(orderId, amount.value(), amount.currency());

        return restClient.post()
            .uri("/api/v1/charges")
            .body(request)
            .retrieve()
            .body(PaymentResult.class);
    }

    @Override
    public Optional<PaymentStatus> getStatus(UUID paymentId) {
        return restClient.get()
            .uri("/api/v1/charges/{id}/status", paymentId)
            .retrieve()
            .onStatus(HttpStatusCode::is4xxClientError, (req, resp) -> {
                // 404 — это не ошибка, просто платёж не найден
            })
            .body(new ParameterizedTypeReference<>() {});
    }
}
```

**Эволюция HTTP-клиентов в Spring:**

| Клиент | Версия | Тип | Статус в 2026 |
|--------|--------|-----|---------------|
| `RestTemplate` | Spring 3.0 (2009) | Синхронный, шаблонный | Режим поддержки (maintenance) |
| `WebClient` | Spring 5.0 (2017) | Реактивный / блокирующий | Активно используется в реактивных приложениях |
| `RestClient` | Spring 6.1 (2023) | Синхронный, fluent API | **Рекомендован** для синхронных приложений |
| `HTTP Interface` | Spring 6.0 (2022) | Декларативный (как Feign) | Набирает популярность |

Декларативный HTTP Interface:

```java
// Объявляем интерфейс
public interface PaymentServiceClient {

    @PostExchange("/api/v1/charges")
    PaymentResult charge(@RequestBody ChargeRequest request);

    @GetExchange("/api/v1/charges/{id}/status")
    PaymentStatus getStatus(@PathVariable UUID id);
}

// Конфигурируем
@Configuration
public class HttpInterfaceConfig {

    @Bean
    public PaymentServiceClient paymentServiceClient(RestClient.Builder builder) {
        RestClient restClient = builder.baseUrl("https://payment-service.internal").build();
        RestClientAdapter adapter = RestClientAdapter.create(restClient);
        HttpServiceProxyFactory factory = HttpServiceProxyFactory.builderFor(adapter).build();
        return factory.createClient(PaymentServiceClient.class);
    }
}
```

**gRPC для межсервисного взаимодействия.** gRPC используется для синхронного взаимодействия между внутренними сервисами, где важна производительность и строгая типизация:

```protobuf
// proto/order_service.proto
syntax = "proto3";
package com.example.order;

option java_multiple_files = true;

service OrderService {
  rpc GetOrder (GetOrderRequest) returns (OrderResponse);
  rpc CreateOrder (CreateOrderRequest) returns (OrderResponse);
  rpc StreamOrderUpdates (GetOrderRequest) returns (stream OrderStatusUpdate);
}

message GetOrderRequest {
  string order_id = 1;
}

message OrderResponse {
  string id = 1;
  string customer_id = 2;
  OrderStatus status = 3;
  repeated OrderItem items = 4;
  string total_amount = 5;
}

enum OrderStatus {
  ORDER_STATUS_UNSPECIFIED = 0;
  ORDER_STATUS_CREATED = 1;
  ORDER_STATUS_CONFIRMED = 2;
  ORDER_STATUS_SHIPPED = 3;
}

message OrderItem {
  string product_id = 1;
  int32 quantity = 2;
  string price = 3;
}
```

**GraphQL — когда использовать.** GraphQL оправдан, когда: клиенты (BFF, мобильные приложения) нуждаются в гибких запросах к данным; множество связанных сущностей с разной глубиной вложенности; проблема over-fetching / under-fetching REST API критична.

```java
// Spring for GraphQL
@Controller
public class OrderGraphQlController {

    private final GetOrderUseCase getOrderUseCase;

    @QueryMapping
    public Order orderById(@Argument UUID id) {
        return getOrderUseCase.getById(id);
    }

    @SchemaMapping(typeName = "Order", field = "customer")
    public CompletableFuture<Customer> customer(
            Order order, DataLoader<UUID, Customer> customerLoader) {
        return customerLoader.load(order.getCustomerId());
    }
}
```

### Важное
- API-first подход: сначала контракт (OpenAPI-спецификация), потом реализация. Это позволяет фронтенд-команде и потребителям API начинать работу параллельно.
- Используйте Problem Detail (RFC 9457) для всех ошибок — это стандарт, который Spring Boot 3 поддерживает нативно.
- RestClient — рекомендованный HTTP-клиент для синхронных приложений в 2026.
- REST — для public API и простых CRUD-операций. gRPC — для внутренних высоконагруженных вызовов. GraphQL — для сложных клиентских запросов.

### Частые ошибки
- Использование RestTemplate в новых проектах — он находится в режиме maintenance.
- Возврат доменных сущностей напрямую из API вместо DTO — утечка внутренней структуры, проблемы с Lazy Loading.
- Отсутствие версионирования API — любое breaking change ломает клиентов.
- Использование HTTP 200 для всех ответов с кодом ошибки в теле — нарушение семантики HTTP.
- Отсутствие пагинации для коллекций — проблемы с производительностью на больших объёмах.

### Как используется в 2026
RestClient стал де-факто стандартом для HTTP-вызовов в синхронных Spring-приложениях. HTTP Interface (декларативный подход) набирает популярность как альтернатива OpenFeign, который уходит из экосистемы Spring Cloud. gRPC активно используется в микросервисных архитектурах для inter-service communication, особенно с поддержкой Spring Boot стартера `grpc-spring-boot-starter`. OpenAPI 3.1 (совместимый с JSON Schema) — стандарт для документирования REST API.

[к оглавлению](#Современная-разработка-web-приложений)

## Работа с данными

Spring Data JPA остаётся основным способом работы с реляционными данными. Hibernate 6 (включённый в Spring Boot 3.x) принёс значительные улучшения.

**Доменная модель (JPA-сущности):**

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

    // Бизнес-метод на уровне сущности
    public void addItem(OrderItemJpaEntity item) {
        items.add(item);
        item.setOrder(this);
        recalculateTotal();
    }

    private void recalculateTotal() {
        this.totalAmount = items.stream()
            .map(i -> i.getPrice().multiply(BigDecimal.valueOf(i.getQuantity())))
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
}
```

**Репозиторий:**

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

    // Native query для сложных случаев
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

**Настройка Hibernate для production:**

```yaml
# application-prod.yml
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
          batch_versioned_data: true
        order_inserts: true
        order_updates: true
        query:
          in_clause_parameter_padding: true
          fail_on_pagination_over_collection_fetch: true
        generate_statistics: false
```

**Flyway для миграций:**

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

```yaml
# application.yml
spring:
  flyway:
    enabled: true
    locations: classpath:db/migration
    baseline-on-migrate: true
    validate-on-migrate: true
```

**PostgreSQL — почему стандарт де-факто.** PostgreSQL доминирует в Java-экосистеме благодаря: полная поддержка ACID с отличной производительностью, расширенные типы данных (`JSONB`, `UUID`, `ARRAY`, `tstzrange`), полнотекстовый поиск без внешних систем, расширения (`pg_stat_statements`, `pg_trgm`, `PostGIS`), надёжные managed-сервисы (AWS RDS, GCP Cloud SQL, Neon, Supabase).

```sql
-- Использование JSONB для гибких атрибутов
ALTER TABLE orders ADD COLUMN metadata JSONB DEFAULT '{}';

-- GIN-индекс для поиска по JSONB
CREATE INDEX idx_orders_metadata ON orders USING GIN (metadata);

-- Запрос по JSONB-полю
SELECT * FROM orders
WHERE metadata->>'source' = 'mobile'
AND metadata->'tags' ? 'priority';
```

**Redis для кэширования:**

```java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public RedisCacheConfiguration cacheConfiguration() {
        return RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))
            .disableCachingNullValues()
            .serializeValuesWith(
                RedisSerializationContext.SerializationPair
                    .fromSerializer(new GenericJackson2JsonRedisSerializer()));
    }
}

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

```yaml
spring:
  data:
    redis:
      host: ${REDIS_HOST:localhost}
      port: ${REDIS_PORT:6379}
      timeout: 2000ms
      lettuce:
        pool:
          max-active: 16
          max-idle: 8
```

**R2DBC для реактивного доступа.** В 2026 году с Virtual Threads необходимость в реактивном стеке для I/O-bound задач значительно снизилась. R2DBC остаётся релевантным для изначально реактивных приложений.

```java
public interface OrderReactiveRepository extends ReactiveCrudRepository<Order, UUID> {

    Flux<Order> findByCustomerId(UUID customerId);

    @Query("SELECT * FROM orders WHERE status = :status LIMIT :limit")
    Flux<Order> findByStatus(String status, int limit);
}
```

### Важное
- `spring.jpa.open-in-view=false` — обязательная настройка. Open-in-view держит транзакцию открытой на протяжении всего HTTP-запроса, маскируя LazyInitializationException, но создавая проблемы с производительностью.
- Миграции БД — только через Flyway/Liquibase, никогда через `ddl-auto=update` в production.
- Optimistic locking (`@Version`) — предпочтительный подход для конкурентного доступа.
- Batch operations для массовых операций — настройка `batch_size` и `order_inserts/order_updates`.

### Частые ошибки
- N+1 проблема: загрузка связанных сущностей в цикле. Решение: `@EntityGraph`, `JOIN FETCH`, `default_batch_fetch_size`.
- Использование `ddl-auto=update` в production — потенциальная потеря данных и непредсказуемые изменения схемы.
- Кэширование без стратегии инвалидации — устаревшие данные хуже отсутствия кэша.
- Отсутствие индексов для часто используемых запросов — определяется через `EXPLAIN ANALYZE`.
- Игнорирование connection pool настроек — дефолтный HikariCP pool size (10) может быть недостаточен для нагруженных сервисов.

### Как используется в 2026
Spring Data JPA с Hibernate 6 остаётся стандартом для работы с реляционными данными. Virtual Threads (Java 21) снизили необходимость в R2DBC: теперь можно использовать блокирующий JDBC с производительностью, близкой к реактивному стеку. PostgreSQL занимает позицию «дефолтной» базы данных для Java-проектов. Flyway лидирует среди инструментов миграции, хотя Liquibase сохраняет позиции в enterprise-среде благодаря XML-формату и rollback-возможностям.

[к оглавлению](#Современная-разработка-web-приложений)

## Безопасность

Spring Security 6 полностью перешёл на компонентную модель с `SecurityFilterChain`, заменив устаревший `WebSecurityConfigurerAdapter`.

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
                .requestMatchers("/v3/api-docs/**", "/swagger-ui/**").permitAll()
                .requestMatchers(HttpMethod.POST, "/api/v1/orders").hasRole("USER")
                .requestMatchers(HttpMethod.GET, "/api/v1/orders/**").hasAnyRole("USER", "ADMIN")
                .requestMatchers("/api/v1/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthConverter()))
            )
            .exceptionHandling(ex -> ex
                .authenticationEntryPoint((request, response, authException) -> {
                    response.setStatus(HttpStatus.UNAUTHORIZED.value());
                    response.setContentType(MediaType.APPLICATION_PROBLEM_JSON_VALUE);
                    response.getWriter().write("""
                        {"type":"about:blank","title":"Unauthorized","status":401}
                        """);
                })
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

**OAuth 2.1 + OpenID Connect.** OAuth 2.1 — упрощённая и более безопасная версия OAuth 2.0: обязательный PKCE для всех клиентов (не только public), запрещён implicit flow, запрещён password grant, refresh tokens с привязкой к отправителю (sender-constrained).

```yaml
# application.yml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://keycloak.example.com/realms/my-app
          jwk-set-uri: https://keycloak.example.com/realms/my-app/protocol/openid-connect/certs
```

**JWT vs Opaque Tokens:**

| Критерий | JWT | Opaque Token |
|----------|-----|-------------|
| Валидация | Локально (по подписи) | Запрос к Authorization Server (introspection) |
| Размер | Большой (payload в токене) | Маленький (ссылка) |
| Отзыв | Сложно (только через blacklist/short TTL) | Легко (удалить на сервере) |
| Информация | Содержит claims | Только идентификатор |
| Подходит для | Микросервисов (нет доп. запросов) | Монолитов с высокими требованиями к безопасности |

Рекомендация: JWT для межсервисного взаимодействия, короткий TTL (5-15 минут), refresh token для обновления.

**Keycloak как Identity Provider:**

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
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: keycloak
```

**Rate Limiting (Bucket4j + Redis):**

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

### Важное
- Stateless API: `SessionCreationPolicy.STATELESS` + JWT/Opaque tokens. Никаких HTTP-сессий.
- CSRF отключается для stateless REST API. Для server-rendered HTML (Thymeleaf) CSRF обязателен.
- Method Security (`@PreAuthorize`, `@Secured`) — для fine-grained авторизации на уровне бизнес-логики.
- Secrets никогда не хранятся в коде или конфигурационных файлах. Используйте переменные окружения, Vault, или Kubernetes Secrets.

### Частые ошибки
- Хранение JWT в localStorage — уязвимость к XSS. Для браузерных приложений: HttpOnly cookie с SameSite=Strict.
- Длинный TTL для access token (часы, дни). Рекомендация: 5-15 минут.
- Отсутствие rate limiting — уязвимость к DDoS и brute-force атакам.
- Логирование токенов и паролей — критическая уязвимость. Настройте фильтрацию чувствительных данных.
- Использование `@Secured("ROLE_ADMIN")` без юнит-тестов на авторизацию.

### Как используется в 2026
Spring Security 6 с OAuth 2.1 — стандартная конфигурация для production-приложений. Keycloak остаётся лидером среди open-source Identity Provider, хотя набирают популярность альтернативы (Zitadel, Authentik). OAuth 2.1 формально стандартизирован, и большинство библиотек и IdP уже поддерживают его требования. Spring Authorization Server зрелый продукт для случаев, когда нужен собственный Authorization Server без Keycloak.

[к оглавлению](#Современная-разработка-web-приложений)

## Асинхронность и обмен сообщениями

Kafka — стандарт для event-driven взаимодействия между сервисами.

**Конфигурация:**

```yaml
spring:
  kafka:
    bootstrap-servers: ${KAFKA_BOOTSTRAP_SERVERS:localhost:9092}
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
      acks: all
      retries: 3
      properties:
        enable.idempotence: true
        max.in.flight.requests.per.connection: 5
    consumer:
      group-id: order-service
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      auto-offset-reset: earliest
      properties:
        spring.json.trusted.packages: com.example.*
    listener:
      ack-mode: manual
      concurrency: 3
```

**Producer:**

```java
@Component
@RequiredArgsConstructor
public class KafkaOrderEventPublisher implements OrderEventPublisher {

    private final KafkaTemplate<String, Object> kafkaTemplate;

    @Override
    public void publishOrderCreated(Order order) {
        OrderCreatedEvent event = new OrderCreatedEvent(
            order.getId(),
            order.getCustomerId(),
            order.getTotalAmount(),
            Instant.now()
        );

        kafkaTemplate.send("order.events", order.getId().toString(), event)
            .whenComplete((result, ex) -> {
                if (ex != null) {
                    log.error("Failed to publish OrderCreatedEvent for order {}",
                        order.getId(), ex);
                } else {
                    log.info("Published OrderCreatedEvent for order {} to partition {} offset {}",
                        order.getId(),
                        result.getRecordMetadata().partition(),
                        result.getRecordMetadata().offset());
                }
            });
    }
}
```

**Consumer с ручным подтверждением:**

```java
@Component
@RequiredArgsConstructor
@Slf4j
public class PaymentEventConsumer {

    private final OrderService orderService;

    @KafkaListener(
        topics = "payment.events",
        groupId = "order-service",
        containerFactory = "kafkaListenerContainerFactory"
    )
    public void handlePaymentEvent(
            @Payload PaymentCompletedEvent event,
            @Header(KafkaHeaders.RECEIVED_KEY) String key,
            @Header(KafkaHeaders.RECEIVED_PARTITION) int partition,
            @Header(KafkaHeaders.OFFSET) long offset,
            Acknowledgment acknowledgment) {

        log.info("Received PaymentCompletedEvent: orderId={}, partition={}, offset={}",
            event.orderId(), partition, offset);

        try {
            orderService.confirmOrder(event.orderId(), event.paymentId());
            acknowledgment.acknowledge();
        } catch (Exception e) {
            log.error("Failed to process PaymentCompletedEvent for order {}",
                event.orderId(), e);
            // Не подтверждаем — сообщение будет повторно обработано
            // Для poison pill: отправить в DLT (Dead Letter Topic)
        }
    }
}
```

**Transactional Outbox Pattern** — гарантированная доставка событий:

```java
// 1. Сохраняем событие в outbox-таблицу в той же транзакции, что и бизнес-данные
@Transactional
public Order createOrder(CreateOrderCommand command) {
    Order order = Order.create(command);
    orderRepository.save(order);

    OutboxEvent event = OutboxEvent.builder()
        .aggregateType("Order")
        .aggregateId(order.getId().toString())
        .eventType("OrderCreated")
        .payload(objectMapper.writeValueAsString(new OrderCreatedEvent(order)))
        .build();
    outboxRepository.save(event);

    return order;
}

// 2. Отдельный процесс (scheduler) читает из outbox и отправляет в Kafka
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

**Virtual Threads для I/O-bound задач.** Virtual Threads (Java 21) — революция в обработке конкурентных I/O-bound задач:

```yaml
# Включение Virtual Threads в Spring Boot 3.2+
spring:
  threads:
    virtual:
      enabled: true
```

Одна строка в конфигурации — и все HTTP-обработчики, Kafka listeners, scheduled tasks переключаются на виртуальные потоки. Каждый запрос получает свой виртуальный поток, при этом platform threads используются только во время CPU-bound работы.

```java
// До Virtual Threads: реактивный стек для масштабируемости
public Mono<Order> getOrder(UUID id) {
    return orderRepository.findById(id)
        .switchIfEmpty(Mono.error(new OrderNotFoundException(id)))
        .flatMap(order -> enrichWithCustomer(order));
}

// С Virtual Threads: простой синхронный код с той же масштабируемостью
public Order getOrder(UUID id) {
    Order order = orderRepository.findById(id)
        .orElseThrow(() -> new OrderNotFoundException(id));
    return enrichWithCustomer(order);
}
```

**Spring Events для внутренней коммуникации.** Для событий внутри одного приложения (модульный монолит):

```java
// Событие
public record OrderCreatedEvent(UUID orderId, UUID customerId, BigDecimal totalAmount) {}

// Публикация
@Service
@RequiredArgsConstructor
public class OrderService {

    private final ApplicationEventPublisher eventPublisher;

    @Transactional
    public Order createOrder(CreateOrderCommand command) {
        Order order = // ... создание заказа
        eventPublisher.publishEvent(new OrderCreatedEvent(
            order.getId(), order.getCustomerId(), order.getTotalAmount()));
        return order;
    }
}

// Обработка
@Component
@RequiredArgsConstructor
public class NotificationHandler {

    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    @Async
    public void onOrderCreated(OrderCreatedEvent event) {
        // Отправка уведомления (выполняется после коммита транзакции)
        notificationService.sendOrderConfirmation(event.orderId());
    }
}
```

**RabbitMQ vs Kafka — когда что:**

| Критерий | Apache Kafka | RabbitMQ |
|----------|-------------|----------|
| Модель | Лог (append-only, retention) | Очередь (сообщение удаляется после обработки) |
| Порядок | Гарантирован в пределах partition | Гарантирован в пределах очереди |
| Производительность | Миллионы msg/sec | Десятки тысяч msg/sec |
| Replay | Можно перечитать историю | Нет (только через DLQ) |
| Сложные маршруты | Простой topic routing | Exchanges, bindings, routing keys |
| Подходит для | Event Sourcing, Event Streaming, Log Aggregation | Task queues, RPC, сложная маршрутизация |

### Важное
- Kafka — для event streaming и event-driven архитектуры, когда важно хранение истории событий и replay.
- RabbitMQ — для task queues и request/reply паттернов с комплексной маршрутизацией.
- Virtual Threads устраняют основную мотивацию для реактивного стека в большинстве приложений.
- Transactional Outbox Pattern — единственный надёжный способ гарантировать атомарность бизнес-операции и публикации события.

### Частые ошибки
- Обработка Kafka-сообщений без idempotency: consumer может получить одно сообщение несколько раз (at-least-once). Всегда проектируйте идемпотентную обработку.
- Использование auto-commit в Kafka consumer: сообщение может быть потеряно при падении сервиса до фактической обработки.
- Блокирующие операции (synchronized) на виртуальных потоках: pinning виртуального потока к carrier thread. Используйте `ReentrantLock` или `Semaphore` вместо `synchronized`.
- Публикация события до коммита транзакции (без Outbox): при откате транзакции событие уже отправлено.

### Как используется в 2026
Virtual Threads стали стандартом для Spring Boot приложений — одна настройка `spring.threads.virtual.enabled=true` и всё работает. Apache Kafka остаётся лидером для event-driven архитектуры. Spring Modulith предоставляет встроенную поддержку Transactional Outbox через `@Externalized` аннотацию, упрощая публикацию доменных событий в Kafka/RabbitMQ. Structured Concurrency (Project Loom) стабилизируется и упрощает работу с параллельными задачами.

[к оглавлению](#Современная-разработка-web-приложений)

## Наблюдаемость (Observability)

Наблюдаемость — это три столпа: **метрики**, **логи**, **трейсы**. Все три должны быть связаны через correlation ID (trace ID).

OpenTelemetry (OTel) — вендор-нейтральный стандарт для сбора телеметрии. Micrometer — фасад для метрик в Spring-экосистеме.

```gradle
// build.gradle.kts
dependencies {
    implementation("org.springframework.boot:spring-boot-starter-actuator")
    implementation("io.micrometer:micrometer-registry-prometheus")
    implementation("io.micrometer:micrometer-tracing-bridge-otel")
    implementation("io.opentelemetry:opentelemetry-exporter-otlp")
}
```

```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: health, info, prometheus, metrics
  endpoint:
    health:
      show-details: when-authorized
      probes:
        enabled: true  # Kubernetes probes: /actuator/health/liveness, /readiness
  metrics:
    tags:
      application: order-service
      environment: ${ENVIRONMENT:local}
    distribution:
      percentiles-histogram:
        http.server.requests: true
  tracing:
    sampling:
      probability: 1.0  # 100% в dev, 10-50% в prod
  otlp:
    tracing:
      endpoint: http://tempo:4318/v1/traces
    metrics:
      endpoint: http://prometheus:9090/api/v1/otlp/v1/metrics
```

**Кастомные метрики:**

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

    public void recordActiveOrders(int count) {
        registry.gauge("orders.active", count);
    }
}
```

**Кастомные spans для трейсинга:**

```java
@Service
@RequiredArgsConstructor
public class OrderService {

    private final ObservationRegistry observationRegistry;

    public Order createOrder(CreateOrderCommand command) {
        return Observation.createNotStarted("order.create", observationRegistry)
            .lowCardinalityKeyValue("order.type", command.type().name())
            .observe(() -> {
                Order order = buildOrder(command);
                validateStock(order);
                Order saved = orderRepository.save(order);
                publishEvent(saved);
                return saved;
            });
    }
}
```

**Structured Logging (JSON logs, correlation ID):**

```xml
<!-- logback-spring.xml -->
<configuration>
    <!-- JSON-формат для production -->
    <springProfile name="prod">
        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <encoder class="net.logstash.logback.encoder.LogstashEncoder">
                <includeMdcKeyName>traceId</includeMdcKeyName>
                <includeMdcKeyName>spanId</includeMdcKeyName>
                <includeMdcKeyName>userId</includeMdcKeyName>
            </encoder>
        </appender>
    </springProfile>

    <!-- Человекочитаемый формат для локальной разработки -->
    <springProfile name="local">
        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>%d{HH:mm:ss.SSS} [%thread] [%X{traceId:-}] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
    </springProfile>

    <root level="INFO">
        <appender-ref ref="STDOUT"/>
    </root>

    <logger name="com.example" level="DEBUG"/>
    <logger name="org.springframework.security" level="WARN"/>
    <logger name="org.hibernate.SQL" level="DEBUG"/>
</configuration>
```

Пример JSON-лога в production:

```json
{
  "timestamp": "2026-04-19T10:30:45.123Z",
  "level": "INFO",
  "logger": "c.e.order.adapter.in.web.OrderController",
  "message": "Order created successfully",
  "traceId": "abc123def456",
  "spanId": "789ghi",
  "userId": "user-42",
  "orderId": "550e8400-e29b-41d4-a716-446655440000",
  "totalAmount": 150.00,
  "thread": "virtual-42"
}
```

**Grafana + Prometheus стек:**

```yaml
# docker-compose.yml (dev observability stack)
services:
  prometheus:
    image: prom/prometheus:v2.55.0
    ports:
      - "9090:9090"
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana:11.3.0
    ports:
      - "3000:3000"
    environment:
      GF_SECURITY_ADMIN_PASSWORD: admin
    volumes:
      - ./monitoring/grafana/dashboards:/var/lib/grafana/dashboards
      - ./monitoring/grafana/provisioning:/etc/grafana/provisioning

  tempo:
    image: grafana/tempo:2.6.0
    ports:
      - "4318:4318"   # OTLP HTTP
      - "3200:3200"   # Tempo API
    volumes:
      - ./monitoring/tempo.yml:/etc/tempo.yml
    command: ["-config.file=/etc/tempo.yml"]

  loki:
    image: grafana/loki:3.2.0
    ports:
      - "3100:3100"
```

```yaml
# monitoring/prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'order-service'
    metrics_path: /actuator/prometheus
    static_configs:
      - targets: ['order-service:8080']
```

**Spring Boot Actuator** — основные эндпоинты:

| Endpoint | Назначение |
|----------|-----------|
| `/actuator/health` | Health check (liveness + readiness) |
| `/actuator/health/liveness` | Kubernetes liveness probe |
| `/actuator/health/readiness` | Kubernetes readiness probe |
| `/actuator/prometheus` | Метрики в формате Prometheus |
| `/actuator/info` | Информация о приложении (версия, git commit) |
| `/actuator/env` | Переменные окружения (только для отладки) |

Кастомный Health Indicator:

```java
@Component
public class KafkaHealthIndicator implements HealthIndicator {

    private final KafkaAdmin kafkaAdmin;

    @Override
    public Health health() {
        try {
            kafkaAdmin.describeTopics("order.events");
            return Health.up()
                .withDetail("topic", "order.events")
                .build();
        } catch (Exception e) {
            return Health.down()
                .withDetail("error", e.getMessage())
                .build();
        }
    }
}
```

### Важное
- Три столпа наблюдаемости (метрики, логи, трейсы) связаны через trace ID. Без этой связи диагностика инцидентов крайне затруднена.
- JSON-логи в production — обязательны. Они парсятся Loki/Elasticsearch/CloudWatch. Человекочитаемый формат — только для локальной разработки.
- Alerting строится на метриках, а не на логах. Правила алертинга определяются через Prometheus Alertmanager или Grafana Alerting.
- Health checks разделяются на liveness (приложение живо) и readiness (приложение готово принимать трафик).

### Частые ошибки
- 100% sampling в production: огромный объём данных трейсинга. Используйте 10-50% или tail-based sampling.
- Логирование чувствительных данных: пароли, токены, персональные данные в логах.
- Отсутствие дашбордов «из коробки»: инструменты настроены, но никто не смотрит метрики до инцидента.
- Мониторинг только инфраструктурных метрик (CPU, RAM) без бизнес-метрик (количество заказов, ошибок оплаты).
- Алертинг на каждую ошибку в логе вместо агрегированных метрик (error rate).

### Как используется в 2026
OpenTelemetry стал де-факто стандартом для сбора телеметрии, заменив проприетарные агенты (Datadog, New Relic агенты по-прежнему используются, но через OTel SDK). Grafana-стек (Prometheus + Loki + Tempo + Grafana) — стандартный open-source стек наблюдаемости. Spring Boot Actuator + Micrometer обеспечивают глубокую интеграцию с OTel через `micrometer-tracing-bridge-otel`. Observation API (Micrometer) позволяет единообразно создавать метрики и spans из одного кода.

[к оглавлению](#Современная-разработка-web-приложений)

## Тестирование

Пирамида тестирования в 2026:

```
        /  E2E  \           <- Мало (Playwright/Selenium, API tests)
       /  Интегр. \         <- Средне (Testcontainers, @SpringBootTest)
      / Slice-тесты \       <- Средне (@WebMvcTest, @DataJpaTest)
     /   Unit-тесты   \    <- Много (JUnit 5 + Mockito, чистые функции)
    /  Архитектурные    \   <- Автоматически (ArchUnit)
   /   Contract-тесты   \  <- Между сервисами (Pact, Spring Cloud Contract)
```

**JUnit 5 + Mockito — Unit-тест бизнес-логики:**

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock
    OrderRepository orderRepository;

    @Mock
    PaymentGateway paymentGateway;

    @Mock
    OrderEventPublisher eventPublisher;

    @InjectMocks
    OrderService orderService;

    @Test
    void shouldCreateOrderWithCorrectTotal() {
        // given
        CreateOrderCommand command = new CreateOrderCommand(
            UUID.randomUUID(),
            List.of(
                new OrderItemCommand(UUID.randomUUID(), 2, new Money("29.99", "USD")),
                new OrderItemCommand(UUID.randomUUID(), 1, new Money("49.99", "USD"))
            )
        );

        when(orderRepository.save(any(Order.class)))
            .thenAnswer(invocation -> invocation.getArgument(0));

        // when
        Order result = orderService.createOrder(command);

        // then
        assertThat(result.getStatus()).isEqualTo(OrderStatus.CREATED);
        assertThat(result.getTotalAmount()).isEqualByComparingTo(new BigDecimal("109.97"));
        assertThat(result.getItems()).hasSize(2);

        verify(eventPublisher).publishOrderCreated(result);
        verify(paymentGateway, never()).charge(any(), any());
    }

    @Test
    void shouldThrowWhenOrderNotFound() {
        UUID orderId = UUID.randomUUID();
        when(orderRepository.findById(orderId)).thenReturn(Optional.empty());

        assertThatThrownBy(() -> orderService.getById(orderId))
            .isInstanceOf(OrderNotFoundException.class)
            .hasMessageContaining(orderId.toString());
    }

    @ParameterizedTest
    @CsvSource({
        "CREATED,    CONFIRMED, true",
        "CONFIRMED,  SHIPPED,   true",
        "SHIPPED,    DELIVERED, true",
        "DELIVERED,  CANCELLED, false",
        "CANCELLED,  CONFIRMED, false"
    })
    void shouldValidateStatusTransition(
            OrderStatus from, OrderStatus to, boolean allowed) {
        if (allowed) {
            assertThat(from.canTransitionTo(to)).isTrue();
        } else {
            assertThat(from.canTransitionTo(to)).isFalse();
        }
    }
}
```

**Testcontainers для интеграционных тестов.** Testcontainers запускает реальные зависимости (PostgreSQL, Kafka, Redis) в Docker-контейнерах:

```java
// Базовый класс для интеграционных тестов
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Testcontainers
abstract class IntegrationTestBase {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17-alpine")
        .withDatabaseName("testdb")
        .withUsername("test")
        .withPassword("test");

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

```java
class OrderIntegrationTest extends IntegrationTestBase {

    @Autowired
    TestRestTemplate restTemplate;

    @Autowired
    OrderRepository orderRepository;

    @Test
    void shouldCreateAndRetrieveOrder() {
        // given
        CreateOrderRequest request = new CreateOrderRequest(
            UUID.randomUUID(),
            List.of(new OrderItemRequest(UUID.randomUUID(), 2))
        );

        // when
        ResponseEntity<OrderResponse> createResponse = restTemplate.postForEntity(
            "/api/v1/orders", request, OrderResponse.class);

        // then
        assertThat(createResponse.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(createResponse.getHeaders().getLocation()).isNotNull();

        UUID orderId = createResponse.getBody().id();

        // verify persistence
        ResponseEntity<OrderResponse> getResponse = restTemplate.getForEntity(
            "/api/v1/orders/{id}", OrderResponse.class, orderId);

        assertThat(getResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(getResponse.getBody().id()).isEqualTo(orderId);
        assertThat(getResponse.getBody().status()).isEqualTo("CREATED");
    }

    @Test
    void shouldReturn404ForNonExistentOrder() {
        ResponseEntity<ProblemDetail> response = restTemplate.getForEntity(
            "/api/v1/orders/{id}", ProblemDetail.class, UUID.randomUUID());

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.NOT_FOUND);
        assertThat(response.getBody().getTitle()).isEqualTo("Заказ не найден");
    }
}
```

**Slice-тесты** для изолированного тестирования слоёв:

```java
// Тест контроллера (без запуска всего контекста)
@WebMvcTest(OrderController.class)
class OrderControllerTest {

    @Autowired
    MockMvc mockMvc;

    @MockBean
    CreateOrderUseCase createOrderUseCase;

    @MockBean
    GetOrderUseCase getOrderUseCase;

    @Test
    void shouldReturn201WhenOrderCreated() throws Exception {
        Order order = Order.builder()
            .id(UUID.randomUUID())
            .status(OrderStatus.CREATED)
            .build();
        when(createOrderUseCase.create(any())).thenReturn(order);

        mockMvc.perform(post("/api/v1/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("""
                    {
                        "customerId": "550e8400-e29b-41d4-a716-446655440000",
                        "items": [{"productId": "123e4567-e89b-12d3-a456-426614174000", "quantity": 2}]
                    }
                    """))
            .andExpect(status().isCreated())
            .andExpect(header().exists("Location"))
            .andExpect(jsonPath("$.status").value("CREATED"));
    }

    @Test
    void shouldReturn400WhenRequestInvalid() throws Exception {
        mockMvc.perform(post("/api/v1/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("""
                    {"customerId": null, "items": []}
                    """))
            .andExpect(status().isBadRequest())
            .andExpect(jsonPath("$.fieldErrors").exists());
    }
}

// Тест JPA-слоя
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
@Testcontainers
class OrderJpaRepositoryTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17-alpine");

    @DynamicPropertySource
    static void props(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    OrderJpaRepository repository;

    @Test
    void shouldFindOrdersByCustomerAndStatus() {
        UUID customerId = UUID.randomUUID();
        // given
        repository.saveAll(List.of(
            createOrder(customerId, OrderStatus.CREATED),
            createOrder(customerId, OrderStatus.SHIPPED),
            createOrder(UUID.randomUUID(), OrderStatus.CREATED)
        ));

        // when
        List<OrderJpaEntity> result = repository.findByCustomerIdAndStatus(
            customerId, OrderStatus.CREATED);

        // then
        assertThat(result).hasSize(1);
        assertThat(result.get(0).getCustomerId()).isEqualTo(customerId);
    }
}
```

**Contract Testing (Spring Cloud Contract / Pact).** Contract Testing гарантирует совместимость API между producer и consumer:

```java
// Spring Cloud Contract — на стороне producer
// contracts/shouldReturnOrder.groovy
Contract.make {
    description "should return order by ID"
    request {
        method GET()
        url "/api/v1/orders/550e8400-e29b-41d4-a716-446655440000"
    }
    response {
        status 200
        headers {
            contentType(applicationJson())
        }
        body(
            id: "550e8400-e29b-41d4-a716-446655440000",
            status: "CREATED",
            totalAmount: 99.99
        )
    }
}
```

**ArchUnit для архитектурных правил:**

```java
@AnalyzeClasses(packages = "com.example.order")
class ArchitectureTest {

    @ArchTest
    static final ArchRule domainShouldNotDependOnAdapters =
        noClasses()
            .that().resideInAPackage("..domain..")
            .should().dependOnClassesThat()
            .resideInAPackage("..adapter..");

    @ArchTest
    static final ArchRule domainShouldNotDependOnSpring =
        noClasses()
            .that().resideInAPackage("..domain..")
            .should().dependOnClassesThat()
            .resideInAPackage("org.springframework..");

    @ArchTest
    static final ArchRule controllersShouldNotAccessRepositories =
        noClasses()
            .that().resideInAPackage("..adapter.in.web..")
            .should().dependOnClassesThat()
            .resideInAPackage("..adapter.out.persistence..");

    @ArchTest
    static final ArchRule servicesShouldBeAnnotatedWithService =
        classes()
            .that().resideInAPackage("..domain.service..")
            .should().beAnnotatedWith(Service.class);
}
```

### Важное
- Unit-тесты — для бизнес-логики (быстрые, детерминированные). Интеграционные — для проверки взаимодействия компонентов.
- Testcontainers заменил H2 для тестов с БД: тестируете на реальном PostgreSQL, а не на in-memory базе с другим диалектом.
- ArchUnit автоматически проверяет архитектурные правила при каждом запуске тестов — нарушение архитектуры = упавший тест.
- Slice-тесты (`@WebMvcTest`, `@DataJpaTest`) быстрее, чем `@SpringBootTest`, потому что загружают только нужный срез контекста.

### Частые ошибки
- Тестирование на H2 вместо реальной БД: различия в SQL-диалектах, функциях, типах данных приводят к пропущенным багам.
- Отсутствие тестов на ошибочные сценарии: тестируется только happy path.
- Медленные тесты из-за запуска полного контекста (`@SpringBootTest`) для каждого теста. Используйте `@WebMvcTest`/`@DataJpaTest` где возможно.
- Mock всего подряд: unit-тест, который мокает всё, не тестирует ничего. Мокайте только внешние зависимости.
- Отсутствие contract-тестов между сервисами: интеграция ломается при деплое.

### Как используется в 2026
Testcontainers — стандарт для интеграционных тестов. Testcontainers Cloud позволяет запускать контейнеры в облаке для CI-среды без Docker daemon. ArchUnit повсеместно используется для enforcement'а архитектурных правил. JUnit 5 с параметризованными тестами и extension model — единственный актуальный тестовый фреймворк. WireMock и MockServer используются для мокирования внешних HTTP-сервисов в тестах.

[к оглавлению](#Современная-разработка-web-приложений)

## CI/CD

Типичный CI/CD pipeline с GitHub Actions:

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

permissions:
  contents: read
  packages: write

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
        options: >-
          --health-cmd="pg_isready"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=5

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
        env:
          SPRING_DATASOURCE_URL: jdbc:postgresql://localhost:5432/testdb
          SPRING_DATASOURCE_USERNAME: test
          SPRING_DATASOURCE_PASSWORD: test

      - name: Run checkstyle
        run: ./gradlew checkstyleMain checkstyleTest

      - name: Upload test results
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: test-results
          path: build/reports/tests/

  build-and-push:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: 'gradle'

      - name: Build JAR
        run: ./gradlew bootJar

      - name: Login to Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:${{ github.sha }}
            ghcr.io/${{ github.repository }}:latest

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production

    steps:
      - uses: actions/checkout@v4

      - name: Update Kubernetes manifest
        run: |
          cd k8s/overlays/production
          kustomize edit set image order-service=ghcr.io/${{ github.repository }}:${{ github.sha }}

      - name: Commit and push manifest changes
        run: |
          git config user.name "GitHub Actions"
          git config user.email "actions@github.com"
          git add k8s/
          git commit -m "deploy: update order-service to ${{ github.sha }}"
          git push
```

**Docker + Docker Compose для dev-окружения:**

```yaml
# docker-compose.yml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: local
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/orderdb
      SPRING_DATASOURCE_USERNAME: order
      SPRING_DATASOURCE_PASSWORD: order
      SPRING_KAFKA_BOOTSTRAP_SERVERS: kafka:9092
      SPRING_DATA_REDIS_HOST: redis
    depends_on:
      postgres:
        condition: service_healthy
      kafka:
        condition: service_healthy
      redis:
        condition: service_started

  postgres:
    image: postgres:17-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: orderdb
      POSTGRES_USER: order
      POSTGRES_PASSWORD: order
    volumes:
      - postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U order -d orderdb"]
      interval: 5s
      timeout: 5s
      retries: 5

  kafka:
    image: confluentinc/cp-kafka:7.7.0
    ports:
      - "9092:9092"
    environment:
      KAFKA_NODE_ID: 1
      KAFKA_PROCESS_ROLES: broker,controller
      KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092,CONTROLLER://0.0.0.0:9093
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092
      KAFKA_CONTROLLER_QUORUM_VOTERS: 1@kafka:9093
      KAFKA_CONTROLLER_LISTENER_NAMES: CONTROLLER
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
      CLUSTER_ID: "MkU3OEVBNTcwNTJENDM2Qk"
    healthcheck:
      test: kafka-broker-api-versions --bootstrap-server localhost:9092
      interval: 10s
      timeout: 10s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  prometheus:
    image: prom/prometheus:v2.55.0
    ports:
      - "9090:9090"
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana:11.3.0
    ports:
      - "3000:3000"
    environment:
      GF_SECURITY_ADMIN_PASSWORD: admin

volumes:
  postgres-data:
```

### Важное
- CI pipeline должен быть быстрым (< 10 минут). Используйте кэширование зависимостей, параллельные джобы, Gradle build cache.
- Тесты в CI должны быть детерминированными: никаких зависимостей от внешних сервисов, только Testcontainers или service containers.
- Docker Compose для локальной разработки должен полностью воспроизводить production-зависимости.
- GitOps: инфраструктура как код. Изменение манифестов в git-репозитории автоматически приводит к деплою.

### Частые ошибки
- CI pipeline длиннее 15-20 минут: разработчики перестают ждать и мёрджат без зелёного билда.
- Отсутствие кэширования зависимостей: каждый билд скачивает все jar-файлы заново.
- Один env для всего (dev = staging = prod): конфигурация расходится, баги появляются только в production.
- Ручной деплой: «Вася знает, как деплоить» — это single point of failure.
- Секреты в коде или в CI-файлах: используйте GitHub Secrets, Vault, Sealed Secrets.

### Как используется в 2026
GitHub Actions — лидер для open-source и стартапов. GitLab CI сохраняет позиции в enterprise. GitOps с ArgoCD стал стандартом для Kubernetes-деплоя: изменение манифеста в git приводит к автоматическому sync в кластере. Docker Compose v2 с profiles и healthchecks — стандарт для локальной разработки. Gradle build cache (локальный и remote через Develocity/Gradle Enterprise) радикально ускоряет сборку.

[к оглавлению](#Современная-разработка-web-приложений)

## Контейнеризация и деплой

**Dockerfile best practices (multi-stage build):**

```dockerfile
# Dockerfile — multi-stage build
# Stage 1: Build
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app

# Кэширование зависимостей (слой меняется редко)
COPY build.gradle.kts settings.gradle.kts gradlew ./
COPY gradle/ gradle/
RUN ./gradlew dependencies --no-daemon

# Копирование исходников и сборка
COPY src/ src/
RUN ./gradlew bootJar --no-daemon -x test

# Stage 2: Extract layers (Spring Boot layered JAR)
FROM eclipse-temurin:21-jdk-alpine AS extractor
WORKDIR /app
COPY --from=builder /app/build/libs/*.jar app.jar
RUN java -Djarmode=tools -jar app.jar extract --layers --launcher --destination extracted

# Stage 3: Runtime (minimal image)
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Создаём non-root пользователя
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Копируем слои (от наименее изменяемого к наиболее)
COPY --from=extractor /app/extracted/dependencies/ ./
COPY --from=extractor /app/extracted/spring-boot-loader/ ./
COPY --from=extractor /app/extracted/snapshot-dependencies/ ./
COPY --from=extractor /app/extracted/application/ ./

USER appuser

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --start-period=30s --retries=3 \
  CMD wget -qO- http://localhost:8080/actuator/health/liveness || exit 1

ENTRYPOINT ["java", \
  "-XX:+UseZGC", \
  "-XX:MaxRAMPercentage=75.0", \
  "-Djava.security.egd=file:/dev/./urandom", \
  "org.springframework.boot.loader.launch.JarLauncher"]
```

Оптимизации: Multi-stage build (финальный образ содержит только JRE + JAR), Layered JAR (Docker кэширует неизменённые слои), non-root пользователь, Alpine-based JRE (~200 MB vs ~600 MB для full JDK), ZGC (низкая latency для контейнеров).

**GraalVM Native Image — когда использовать.** Native Image компилирует Java-приложение в native binary: мгновенный старт (< 100ms), низкое потребление памяти.

```kotlin
// build.gradle.kts
plugins {
    id("org.graalvm.buildtools.native") version "0.10.4"
}

graalvmNative {
    binaries {
        named("main") {
            imageName.set("order-service")
            buildArgs.add("--enable-url-protocols=http,https")
            buildArgs.add("-H:+ReportExceptionStackTraces")
        }
    }
}
```

```bash
# Сборка native image
./gradlew nativeCompile

# Или через Spring Boot
./gradlew bootBuildImage
```

Когда использовать Native Image: Serverless / FaaS (AWS Lambda, Cloud Functions) — критичен cold start; CLI-утилиты; сервисы с жёсткими ограничениями по памяти.

Когда НЕ использовать: высоконагруженные long-running сервисы (JIT-компиляция JVM даёт лучший peak performance); активное использование Reflection/Dynamic Proxies без конфигурации reachability metadata; быстрый цикл разработки (компиляция Native Image занимает минуты).

**Kubernetes: основные ресурсы:**

```yaml
# k8s/base/deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
  labels:
    app: order-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
  template:
    metadata:
      labels:
        app: order-service
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "8080"
        prometheus.io/path: "/actuator/prometheus"
    spec:
      containers:
        - name: order-service
          image: ghcr.io/example/order-service:latest
          ports:
            - containerPort: 8080
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: prod
            - name: SPRING_DATASOURCE_URL
              valueFrom:
                secretKeyRef:
                  name: order-db-secret
                  key: url
            - name: SPRING_DATASOURCE_USERNAME
              valueFrom:
                secretKeyRef:
                  name: order-db-secret
                  key: username
            - name: SPRING_DATASOURCE_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: order-db-secret
                  key: password
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
            failureThreshold: 3
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 8080
            initialDelaySeconds: 15
            periodSeconds: 5
          startupProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            initialDelaySeconds: 10
            periodSeconds: 5
            failureThreshold: 30  # До 160 секунд на старт
          lifecycle:
            preStop:
              exec:
                command: ["sh", "-c", "sleep 10"]  # Graceful shutdown
      terminationGracePeriodSeconds: 60

---
# k8s/base/service.yml
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  selector:
    app: order-service
  ports:
    - port: 80
      targetPort: 8080
  type: ClusterIP

---
# k8s/base/configmap.yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: order-service-config
data:
  SPRING_KAFKA_BOOTSTRAP_SERVERS: "kafka.messaging.svc.cluster.local:9092"
  SPRING_DATA_REDIS_HOST: "redis.cache.svc.cluster.local"
  MANAGEMENT_OTLP_TRACING_ENDPOINT: "http://tempo.observability.svc.cluster.local:4318/v1/traces"

---
# k8s/base/hpa.yml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: order-service
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: order-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

**Health checks и graceful shutdown:**

```yaml
# application.yml
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s

server:
  shutdown: graceful  # Ждать завершения текущих запросов при остановке
```

Последовательность graceful shutdown:
1. Kubernetes отправляет SIGTERM
2. `preStop` hook: `sleep 10` — даёт время Ingress убрать pod из роутинга
3. Spring Boot переводит readiness probe в DOWN
4. Spring ждёт завершения текущих запросов (до `timeout-per-shutdown-phase`)
5. Контекст закрывается, ресурсы освобождаются

### Важное
- Multi-stage Docker build обязателен: финальный образ не содержит исходники, build tools, JDK.
- Layered JAR Spring Boot: зависимости кэшируются в отдельном Docker-слое, перебилдится только application layer.
- Kubernetes probes: startup -> liveness -> readiness. startup probe защищает от kill'а медленно стартующего приложения.
- Resource requests/limits обязательны: без них Kubernetes не может эффективно планировать поды.

### Частые ошибки
- Запуск контейнера от root: уязвимость. Всегда создавайте non-root пользователя.
- Отсутствие resource limits: pod «съедает» весь node и влияет на другие сервисы.
- `latest` тег для Docker-образов в production: неизвестно, какая версия запущена. Используйте SHA или семантическое версионирование.
- Отсутствие startup probe: liveness probe убивает pod, который ещё не успел стартовать.
- Graceful shutdown без `preStop` hook: запросы теряются в момент перемаршрутизации Ingress.
- MaxRAMPercentage: 100% — оставьте место для off-heap памяти. 75% — хорошее значение по умолчанию.

### Как используется в 2026
Kubernetes — стандартная платформа для production-деплоя. Managed Kubernetes (EKS, GKE, AKS) доминирует. Helm 3 используется для темплейтинга Kubernetes-манифестов. Kustomize (встроен в kubectl) — для простых overlay-конфигураций (dev/staging/prod). ArgoCD — стандарт для GitOps: git-репозиторий как source of truth для состояния кластера. Spring Boot 3.x Buildpacks (`bootBuildImage`) — альтернатива Dockerfile без написания Dockerfile вручную. GraalVM Native Image зрелый для Spring Boot приложений, но используется выборочно.

[к оглавлению](#Современная-разработка-web-приложений)

## Документация

**OpenAPI / Swagger UI (springdoc-openapi):**

```gradle
// build.gradle.kts
dependencies {
    implementation("org.springdoc:springdoc-openapi-starter-webmvc-ui:2.7.0")
}
```

```yaml
# application.yml
springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    path: /swagger-ui.html
    operations-sorter: method
    tags-sorter: alpha
  default-produces-media-type: application/json
```

```java
@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("Order Service API")
                .version("1.0.0")
                .description("API для управления заказами")
                .contact(new Contact()
                    .name("Platform Team")
                    .email("platform@example.com")))
            .addSecurityItem(new SecurityRequirement().addList("bearer-jwt"))
            .components(new Components()
                .addSecuritySchemes("bearer-jwt",
                    new SecurityScheme()
                        .type(SecurityScheme.Type.HTTP)
                        .scheme("bearer")
                        .bearerFormat("JWT")
                        .description("JWT Bearer Token")));
    }
}
```

Аннотации на контроллере:

```java
@RestController
@RequestMapping("/api/v1/orders")
@Tag(name = "Orders", description = "Управление заказами")
public class OrderController {

    @Operation(
        summary = "Создать заказ",
        description = "Создаёт новый заказ для указанного клиента"
    )
    @ApiResponses({
        @ApiResponse(responseCode = "201", description = "Заказ создан"),
        @ApiResponse(responseCode = "400", description = "Некорректный запрос",
            content = @Content(schema = @Schema(implementation = ProblemDetail.class))),
        @ApiResponse(responseCode = "422", description = "Бизнес-ошибка",
            content = @Content(schema = @Schema(implementation = ProblemDetail.class)))
    })
    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(
            @Valid @RequestBody CreateOrderRequest request) {
        // ...
    }
}
```

**Architecture Decision Records (ADR).** ADR — лёгкий формат для документирования архитектурных решений:

```
# ADR-001: Использование PostgreSQL в качестве основной БД

## Статус
Принято (2026-01-15)

## Контекст
Нужно выбрать основную реляционную СУБД для платформы.
Рассматривались: PostgreSQL, MySQL, Oracle.

## Решение
Используем PostgreSQL 17.

## Причины
- Лучшая поддержка JSONB для гибких атрибутов
- Развитая экосистема расширений
- Managed-сервисы во всех облаках
- Активное сообщество и развитие
- Лицензия (PostgreSQL License, аналог MIT)

## Последствия
- Команде необходимо знание PostgreSQL-специфичных возможностей
- Миграции через Flyway
- Мониторинг через pg_stat_statements
```

Структура ADR в проекте:

```
docs/
  adr/
    0001-use-postgresql.md
    0002-hexagonal-architecture.md
    0003-kafka-for-events.md
    0004-virtual-threads.md
    template.md
```

**README-driven development.** Перед написанием кода опишите в README: что делает сервис (бизнес-контекст), как запустить локально (docker-compose up), как запустить тесты, API-контракт (ссылка на Swagger UI), архитектурные решения (ссылки на ADR).

### Важное
- Документация API генерируется из кода (springdoc-openapi). Ручная документация API устаревает моментально.
- ADR — минимально необходимая архитектурная документация. Фиксирует «почему», а не «что».
- README — точка входа для нового разработчика. Если за 10 минут нельзя поднять проект локально, README плохой.

### Частые ошибки
- Ручное написание OpenAPI-спецификации, которая расходится с реальным API.
- Избыточная документация, которую никто не читает и не обновляет.
- Отсутствие ADR: через год никто не помнит, почему выбрали Kafka вместо RabbitMQ.
- Документация только в Confluence/Wiki: не версионируется вместе с кодом.

### Как используется в 2026
springdoc-openapi — стандартный инструмент для генерации OpenAPI из Spring Boot приложений (заменил SpringFox/Swagger). ADR становятся частью инженерной культуры — многие организации требуют ADR для значимых технических решений. Docs-as-code (документация в git, рядом с кодом) — предпочтительный подход.

[к оглавлению](#Современная-разработка-web-приложений)

## Сборка всего вместе: референсная архитектура

Полная диаграмма компонентов типового приложения:

```
+---------------------------------------------------------------------------+
|                        Kubernetes Cluster                                 |
|                                                                           |
|  +----------+     +----------------------------------------------------+ |
|  | Ingress  |---->|              Order Service                          | |
|  | (nginx)  |     |  +--------+  +---------+  +--------------+         | |
|  +----------+     |  |  REST  |->| Service  |->|  Repository  |         | |
|                   |  |  API   |  | (domain) |  |  (JPA)       |         | |
|                   |  +--------+  +----+-----+  +------+-------+         | |
|                   |                   |               |                  | |
|                   |            +------v------+  +-----v------+          | |
|                   |            | Kafka       |  | PostgreSQL |          | |
|                   |            | Producer    |  |            |          | |
|                   |            +------+------+  +------------+          | |
|                   +-------------------+-----------------------------+   | |
|                                       |                                 | |
|                                       v                                 | |
|                             +-----------------+                         | |
|                             |   Apache Kafka   |                        | |
|                             |  order.events    |                        | |
|                             +--------+--------+                         | |
|                                      |                                  | |
|                      +---------------+---------------+                  | |
|                      v               v               v                  | |
|              +--------------+ +------------+ +-------------+            | |
|              |  Payment     | | Inventory  | | Notification|            | |
|              |  Service     | | Service    | | Service     |            | |
|              +--------------+ +------------+ +-------------+            | |
|                                                                         | |
|  +------------------ Observability Stack --------------------+          | |
|  |  Prometheus --> Grafana                                   |          | |
|  |  Tempo (traces) --> Grafana                               |          | |
|  |  Loki (logs) --> Grafana                                  |          | |
|  +-----------------------------------------------------------+          | |
|                                                                         | |
|  +---- Security ----+                                                   | |
|  |  Keycloak (IdP)  |                                                   | |
|  +------------------+                                                   | |
+---------------------------------------------------------------------------+
```

**Поток запроса от клиента до БД и обратно:**

```
Клиент (Browser / Mobile)
    |
    v HTTP POST /api/v1/orders (JWT в Authorization header)
+----------+
| Ingress  |  TLS termination, rate limiting
+----+-----+
     v
+------------------------------------------+
|          Spring Security Filter Chain     |
|  1. CorsFilter                           |
|  2. BearerTokenAuthenticationFilter      |
|     -> JWT валидация (по JWK от Keycloak)|
|     -> Извлечение ролей из claims        |
|  3. AuthorizationFilter                  |
|     -> Проверка hasRole("USER")          |
+----+-------------------------------------+
     v
+------------------------------------------+
|          OrderController                  |
|  @PostMapping -> @Valid -> CreateOrderReq |
|  -> Mapping DTO -> Domain Command        |
+----+-------------------------------------+
     v
+------------------------------------------+
|          OrderService                     |
|  @Transactional                          |
|  1. Создание Order (domain model)        |
|  2. Валидация бизнес-правил              |
|  3. Сохранение через OrderRepository     |
|  4. Публикация OrderCreatedEvent         |
+----+----------------+-------------------+
     |                |
     v                v
+----------+   +--------------+
|PostgreSQL|   | Kafka Topic  |
|  INSERT  |   | order.events |
+----------+   +--------------+
     |
     v
+------------------------------------------+
|  Response: 201 Created                    |
|  Location: /api/v1/orders/{id}           |
|  Body: OrderResponse (JSON)              |
|  Headers: X-Trace-Id, X-Request-Id       |
+------------------------------------------+
```

**Структура multi-module проекта:**

```
order-platform/
├── build.gradle.kts              # Root build script
├── settings.gradle.kts
├── gradle/
│   └── libs.versions.toml        # Version catalog
│
├── order-service/                # Основной сервис
│   ├── build.gradle.kts
│   └── src/
│
├── order-domain/                 # Чистый домен (без Spring)
│   ├── build.gradle.kts
│   └── src/
│
├── order-api/                    # DTO, события, контракты
│   ├── build.gradle.kts
│   └── src/
│
├── common/                       # Общие утилиты
│   ├── build.gradle.kts
│   └── src/
│
├── docker-compose.yml
├── k8s/
│   ├── base/
│   │   ├── deployment.yml
│   │   ├── service.yml
│   │   └── kustomization.yml
│   └── overlays/
│       ├── dev/
│       ├── staging/
│       └── production/
│
├── monitoring/
│   ├── prometheus.yml
│   ├── grafana/
│   └── tempo.yml
│
└── docs/
    └── adr/
```

**Gradle Version Catalog** для централизованного управления версиями:

```toml
# gradle/libs.versions.toml
[versions]
spring-boot = "3.4.1"
spring-dependency-management = "1.1.7"
testcontainers = "1.20.4"
springdoc = "2.7.0"

[libraries]
spring-boot-starter-web = { module = "org.springframework.boot:spring-boot-starter-web" }
spring-boot-starter-data-jpa = { module = "org.springframework.boot:spring-boot-starter-data-jpa" }
spring-boot-starter-test = { module = "org.springframework.boot:spring-boot-starter-test" }
testcontainers-junit = { module = "org.testcontainers:junit-jupiter", version.ref = "testcontainers" }
testcontainers-postgresql = { module = "org.testcontainers:postgresql", version.ref = "testcontainers" }
springdoc-openapi = { module = "org.springdoc:springdoc-openapi-starter-webmvc-ui", version.ref = "springdoc" }

[plugins]
spring-boot = { id = "org.springframework.boot", version.ref = "spring-boot" }
spring-dependency-management = { id = "io.spring.dependency-management", version.ref = "spring-dependency-management" }
```

**Пример: микросервис управления заказами (Order Service).** Собираем всё вместе — минимальный, но production-ready сервис:

```java
@SpringBootApplication
public class OrderServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}
```

**application.yml:**

```yaml
spring:
  application:
    name: order-service

  datasource:
    url: jdbc:postgresql://${DB_HOST:localhost}:${DB_PORT:5432}/${DB_NAME:orderdb}
    username: ${DB_USERNAME:order}
    password: ${DB_PASSWORD:order}
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 5000

  jpa:
    open-in-view: false
    hibernate:
      ddl-auto: validate
    properties:
      hibernate:
        default_batch_fetch_size: 20

  flyway:
    enabled: true

  kafka:
    bootstrap-servers: ${KAFKA_BOOTSTRAP_SERVERS:localhost:9092}
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
      acks: all

  threads:
    virtual:
      enabled: true

  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: ${OAUTH2_ISSUER_URI:http://localhost:8180/realms/order-platform}

server:
  shutdown: graceful

management:
  endpoints:
    web:
      exposure:
        include: health, info, prometheus
  endpoint:
    health:
      probes:
        enabled: true
  tracing:
    sampling:
      probability: ${TRACING_SAMPLING:1.0}

springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    path: /swagger-ui.html
```

**Доменная модель:**

```java
// domain/model/Order.java
public class Order {
    private UUID id;
    private UUID customerId;
    private OrderStatus status;
    private List<OrderItem> items;
    private Money totalAmount;
    private Instant createdAt;

    public static Order create(UUID customerId, List<OrderItem> items) {
        Order order = new Order();
        order.id = UUID.randomUUID();
        order.customerId = customerId;
        order.status = OrderStatus.CREATED;
        order.items = List.copyOf(items);
        order.totalAmount = items.stream()
            .map(OrderItem::subtotal)
            .reduce(Money.ZERO, Money::add);
        order.createdAt = Instant.now();
        return order;
    }

    public void confirm() {
        if (status != OrderStatus.CREATED) {
            throw new IllegalOrderStateException(
                "Cannot confirm order in status " + status);
        }
        this.status = OrderStatus.CONFIRMED;
    }

    public void cancel() {
        if (status == OrderStatus.DELIVERED) {
            throw new IllegalOrderStateException(
                "Cannot cancel delivered order");
        }
        this.status = OrderStatus.CANCELLED;
    }
}

// domain/model/Money.java (Value Object)
public record Money(BigDecimal amount, String currency) {
    public static final Money ZERO = new Money(BigDecimal.ZERO, "RUB");

    public Money {
        if (amount.scale() > 2) {
            throw new IllegalArgumentException("Money scale must be <= 2");
        }
    }

    public Money add(Money other) {
        if (!currency.equals(other.currency)) {
            throw new CurrencyMismatchException(currency, other.currency);
        }
        return new Money(amount.add(other.amount), currency);
    }
}
```

**Use Case (порт):**

```java
// domain/port/in/CreateOrderUseCase.java
public interface CreateOrderUseCase {
    Order create(CreateOrderCommand command);
}

// domain/port/in/CreateOrderCommand.java
public record CreateOrderCommand(
    UUID customerId,
    List<OrderItemCommand> items
) {
    public record OrderItemCommand(UUID productId, int quantity, Money price) {}
}
```

**Сервис (реализация use case):**

```java
// domain/service/OrderService.java
@Service
@RequiredArgsConstructor
@Transactional
public class OrderService implements CreateOrderUseCase, GetOrderUseCase {

    private final OrderRepository orderRepository;
    private final OrderEventPublisher eventPublisher;

    @Override
    public Order create(CreateOrderCommand command) {
        List<OrderItem> items = command.items().stream()
            .map(i -> new OrderItem(i.productId(), i.quantity(), i.price()))
            .toList();

        Order order = Order.create(command.customerId(), items);
        Order saved = orderRepository.save(order);

        eventPublisher.publishOrderCreated(saved);

        return saved;
    }

    @Override
    @Transactional(readOnly = true)
    public Order getById(UUID orderId) {
        return orderRepository.findById(orderId)
            .orElseThrow(() -> new OrderNotFoundException(orderId));
    }
}
```

Этот пример демонстрирует все принципы, описанные в руководстве:
- Гексагональная архитектура с чётким разделением домена и инфраструктуры
- REST API с валидацией и стандартизированными ошибками
- Spring Security с JWT
- PostgreSQL + Flyway для данных
- Kafka для событий
- Virtual Threads для масштабируемости
- OpenTelemetry для наблюдаемости
- Docker + Kubernetes для деплоя

### Важное
- Референсная архитектура — это шаблон, а не догма. Адаптируйте под контекст проекта.
- Начинайте с модульного монолита и выделяйте сервисы только при реальной необходимости.
- Каждый компонент стека должен быть обоснован (ADR). Не добавляйте Kafka «на всякий случай».
- Production-ready сервис — это не только код. Это тесты, мониторинг, алертинг, документация, CI/CD.

### Частые ошибки
- Копирование референсной архитектуры без адаптации: не каждому проекту нужны Kafka, Redis и Kubernetes.
- Over-engineering на старте: лучше запустить MVP с минимальным стеком и усложнять по мере роста.
- Отсутствие единого стандарта в команде: каждый сервис построен по-своему, нет шаблонов.
- Пренебрежение operational excellence: сервис в production без мониторинга, алертинга и runbook'а.

### Как используется в 2026
Описанный стек (Spring Boot 3 + Java 21 + PostgreSQL + Kafka + Kubernetes + Grafana) — наиболее распространённая комбинация для production Java-приложений в 2026 году. Spring Modulith упрощает модульные монолиты. Virtual Threads устранили необходимость в реактивном стеке для большинства сценариев. Platform Engineering (внутренние платформы на базе Kubernetes) набирает обороты: команды получают готовые шаблоны сервисов с CI/CD, мониторингом и деплоем из коробки.

[к оглавлению](#Современная-разработка-web-приложений)
