[Вопросы для собеседования](README.md)

# Мониторинг и Observability

+ [Что такое Observability и чем она отличается от мониторинга?](#что-такое-observability-и-чем-она-отличается-от-мониторинга)
+ [Что такое метрики и какие типы метрик существуют?](#что-такое-метрики-и-какие-типы-метрик-существуют)
+ [Что такое Micrometer и как его использовать в Spring Boot?](#что-такое-micrometer-и-как-его-использовать-в-spring-boot)
+ [Что такое Prometheus и как он собирает метрики?](#что-такое-prometheus-и-как-он-собирает-метрики)
+ [Что такое Grafana и как визуализировать метрики?](#что-такое-grafana-и-как-визуализировать-метрики)
+ [Что такое distributed tracing?](#что-такое-distributed-tracing)
+ [Что такое OpenTelemetry?](#что-такое-opentelemetry)
+ [Как настроить distributed tracing в Spring Boot?](#как-настроить-distributed-tracing-в-spring-boot)
+ [Что такое Spring Boot Actuator?](#что-такое-spring-boot-actuator)
+ [Что такое алертинг и как его правильно настроить?](#что-такое-алертинг-и-как-его-правильно-настроить)
+ [Что такое SLO, SLI и SLA?](#что-такое-slo-sli-и-sla)
+ [Как организовать centralized logging в микросервисах?](#как-организовать-centralized-logging-в-микросервисах)

## Что такое Observability и чем она отличается от мониторинга?
<!-- grade: junior -->

Observability (наблюдаемость) — свойство системы, позволяющее понять её внутреннее состояние по внешним выходным данным. В отличие от мониторинга, который отвечает на заранее известные вопросы ("работает ли сервис?", "какова загрузка CPU?"), Observability позволяет исследовать неизвестные заранее проблемы ("почему запросы из конкретного региона стали медленнее?").

> **Аналогия из жизни:** мониторинг — это приборная панель автомобиля (температура, скорость, уровень топлива). Observability — это возможность подключить диагностический сканер и понять, почему двигатель стучит, даже если на панели все показатели в норме.

| Аспект | Мониторинг | Observability |
|--------|-----------|---------------|
| Подход | Реактивный — «что сломалось?» | Проактивный — «почему это происходит?» |
| Вопросы | Known-unknowns (известные неизвестные) | Unknown-unknowns (неизвестные неизвестные) |
| Данные | Предопределённые метрики и дашборды | Произвольные запросы по любым измерениям |
| Фокус | Состояние инфраструктуры | Поведение системы глазами пользователя |

### Три столпа Observability

1. **Метрики (Metrics)** — числовые измерения, агрегированные за период времени. Примеры: количество запросов в секунду, время ответа p99, процент ошибок. Метрики дёшевы в хранении и идеальны для алертинга.

2. **Логи (Logs)** — дискретные события с текстовым описанием. Содержат контекст: timestamp, уровень, сообщение, stack trace. Дорогие в хранении, но незаменимы для диагностики конкретной проблемы.

3. **Трейсы (Traces)** — запись прохождения запроса через все сервисы. Показывают причинно-следственные связи и позволяют найти узкое место в цепочке вызовов.

```
┌─────────────────────────────────────────────────────────────┐
│                    Observability                             │
│                                                              │
│   ┌──────────┐    ┌──────────┐    ┌──────────┐              │
│   │ Metrics  │    │   Logs   │    │  Traces  │              │
│   │          │    │          │    │          │              │
│   │ Counter  │    │ JSON     │    │ Span A   │              │
│   │ Gauge    │    │ Struct   │    │  └Span B │              │
│   │ Histogram│    │ Events   │    │   └Span C│              │
│   └──────────┘    └──────────┘    └──────────┘              │
│                                                              │
│   «Что происходит?» «Почему?»    «Где именно?»             │
└─────────────────────────────────────────────────────────────┘
```

### Почему Observability критична для микросервисов

В монолите один запрос обрабатывается внутри одного процесса — достаточно логов и стек-трейса. В микросервисной архитектуре один пользовательский запрос может пройти через 10-20 сервисов, и без Observability невозможно:
- Понять, какой из сервисов в цепочке тормозит
- Найти причину каскадных сбоев
- Оценить влияние деплоя одного сервиса на всю систему

### Важное
- Observability — это **свойство системы**, а не набор инструментов. Инструменты (Prometheus, Grafana, Jaeger) лишь помогают реализовать это свойство.
- Мониторинг является **подмножеством** Observability: хороший мониторинг необходим, но недостаточен.
- Без структурированных логов, метрик с правильными лейблами и distributed tracing система остаётся «чёрным ящиком».

### Частые ошибки
- **Путать инструменты с Observability**: установка Grafana не делает систему наблюдаемой, если логи неструктурированные и нет трейсов.
- **Мониторить только инфраструктуру**: CPU и RAM важны, но бизнес-метрики (конверсия, время обработки заказа) важнее.
- **Игнорировать корреляцию**: метрики, логи и трейсы должны быть связаны через общий traceId/correlationId.
- **Избыточный сбор данных**: собирать всё подряд без анализа — дорого и бесполезно. Нужно осознанно выбирать, что мониторить.

### Как используется в 2026
- **OpenTelemetry** стал де-факто стандартом для инструментирования. Большинство фреймворков (Spring Boot 3.x, Quarkus, Micronaut) имеют встроенную поддержку OTel.
- **Grafana Stack (Mimir + Loki + Tempo)** — популярная open-source альтернатива коммерческим APM-решениям.
- **eBPF-based observability** (Cilium, Pixie) позволяет собирать метрики и трейсы без изменения кода приложения.
- **AI/ML для анализа**: AIOps-инструменты автоматически обнаруживают аномалии и коррелируют события из разных источников.
- **Continuous Profiling** (Pyroscope, Grafana Pyroscope) добавляется как четвёртый столп Observability.

> **На собеседовании:** интервьюер ожидает, что вы не просто перечислите три столпа, а покажете понимание разницы между мониторингом и Observability. Частая ошибка — свести ответ к перечислению инструментов (Prometheus, Grafana). Сильный ответ: Observability — это свойство системы, а не набор тулов; мониторинг — подмножество; ключевое отличие — возможность исследовать неизвестные заранее проблемы.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое метрики и какие типы метрик существуют?
<!-- grade: junior -->

Метрика — числовое значение, измеряемое в определённый момент времени, которое агрегируется и хранится как временной ряд (time series). Каждый временной ряд идентифицируется именем и набором лейблов (меток).

```
http_server_requests_seconds_count{method="GET", uri="/api/orders", status="200"} 1523
│                                  │                                                │
│ имя метрики                      │ лейблы (dimensions)                            │ значение
```

### Основные типы метрик

| Тип | Описание | Примеры | Функции PromQL |
|-----|----------|---------|----------------|
| Counter | Монотонно возрастающее значение (сбрасывается в 0 при перезапуске) | Количество HTTP-запросов, число ошибок | `rate()`, `increase()` |
| Gauge | Значение, которое может увеличиваться и уменьшаться | Активные подключения, размер очереди, heap usage | `avg()`, `min()`, `max()` |
| Histogram | Распределение значений по бакетам | Время ответа, размер запроса | `histogram_quantile()` |
| Timer | Counter + Histogram для измерения длительности | Время обработки заказа | `rate()`, `histogram_quantile()` |

<details><summary>Примеры кода для каждого типа</summary>

```java
// Counter
Counter orderCounter = Counter.builder("orders.created")
    .description("Total number of created orders")
    .tag("region", "eu")
    .register(meterRegistry);
orderCounter.increment();

// Gauge
Gauge.builder("queue.size", queue, Queue::size)
    .description("Current queue size")
    .register(meterRegistry);

// Histogram (DistributionSummary)
DistributionSummary summary = DistributionSummary.builder("order.amount")
    .description("Order amounts distribution")
    .baseUnit("rubles")
    .publishPercentiles(0.5, 0.95, 0.99)
    .publishPercentileHistogram()
    .register(meterRegistry);
summary.record(orderAmount);

// Timer
Timer timer = Timer.builder("order.processing.time")
    .description("Time to process an order")
    .publishPercentiles(0.5, 0.95, 0.99)
    .register(meterRegistry);
timer.record(() -> processOrder(order));
```

</details>

### Методологии выбора метрик

**RED-метод** — для сервисов (запрос-ответ):
- **R**ate — количество запросов в секунду
- **E**rrors — количество неудачных запросов в секунду
- **D**uration — распределение времени ответа (гистограмма)

**USE-метод** — для ресурсов (CPU, память, диск, сеть):
- **U**tilization — процент использования ресурса
- **S**aturation — степень перегрузки (очередь ожидания)
- **E**rrors — количество ошибок ресурса

### Бизнес-метрики vs Технические метрики

| Бизнес-метрики | Технические метрики |
|---------------|-------------------|
| Количество заказов в минуту | CPU utilization |
| Конверсия корзины | JVM heap usage |
| Среднее время доставки | HTTP response time |
| Выручка в реальном времени | Connection pool utilization |
| Количество активных пользователей | GC pause duration |

<details><summary>Пример бизнес-метрики</summary>

```java
@Service
public class PaymentService {
    private final Counter successPayments;
    private final Counter failedPayments;
    private final DistributionSummary paymentAmount;

    public PaymentService(MeterRegistry registry) {
        this.successPayments = Counter.builder("payments.success")
            .tag("provider", "stripe")
            .register(registry);
        this.failedPayments = Counter.builder("payments.failed")
            .tag("provider", "stripe")
            .register(registry);
        this.paymentAmount = DistributionSummary.builder("payments.amount")
            .baseUnit("kopecks")
            .publishPercentiles(0.5, 0.95)
            .register(registry);
    }

    public PaymentResult pay(PaymentRequest request) {
        try {
            PaymentResult result = processPayment(request);
            successPayments.increment();
            paymentAmount.record(request.getAmount());
            return result;
        } catch (PaymentException e) {
            failedPayments.increment();
            throw e;
        }
    }
}
```

</details>

### Важное
- **Counter** нельзя использовать для значений, которые уменьшаются (например, количество активных сессий — это Gauge).
- При работе с Counter в Prometheus всегда используйте функцию `rate()` или `increase()`, а не абсолютное значение.
- **Перцентили** нельзя агрегировать между инстансами: p99 от 5 инстансов — это **не** p99 системы. Для агрегации используйте гистограммы.
- Лейблы с высокой кардинальностью (userId, requestId) **убивают** производительность Prometheus. Используйте лейблы с ограниченным набором значений.

### Частые ошибки
- **Использование Gauge вместо Counter** для подсчёта событий: при перезапуске теряется контекст, а `rate()` не работает корректно.
- **Высокая кардинальность лейблов**: добавление `userId` или `orderId` как лейбла создаёт миллионы time series и перегружает хранилище.
- **Отсутствие бизнес-метрик**: техническая команда мониторит только инфраструктуру, но не понимает, влияет ли деградация на бизнес.
- **Неправильный выбор бакетов гистограммы**: дефолтные бакеты могут не подходить для конкретного SLA. Настраивайте `serviceLevelObjectives()`.

### Как используется в 2026
- **Micrometer 1.13+/2.x** — стандарт для метрик в Spring-экосистеме, с нативной поддержкой OpenTelemetry.
- **Exemplars** — связь метрик с трейсами. Prometheus поддерживает exemplars, позволяя «кликнуть» на spike в графике и перейти к конкретному трейсу.
- **Prometheus Native Histograms** — новый формат гистограмм с автоматическим выбором бакетов и экономией места.
- **PromQL** остаётся основным языком запросов; Grafana поддерживает визуальный query builder.

> **На собеседовании:** важно не просто перечислить Counter/Gauge/Histogram, а объяснить, когда какой тип применять. Покажите знание RED/USE-методологий и упомяните, что бизнес-метрики часто важнее технических. Классическая ловушка — забыть про ограничение кардинальности лейблов.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое Micrometer и как его использовать в Spring Boot?
<!-- grade: middle -->

Micrometer — библиотека-фасад для сбора метрик в JVM-приложениях. По аналогии с SLF4J для логирования, Micrometer предоставляет единый API для работы с метриками, а конкретная реализация (backend) подключается отдельно.

> **Аналогия из жизни:** Micrometer — это как универсальная розетка-переходник. Вы подключаете прибор (приложение) через один стандартный разъём, а на выходе можете подключить любую сеть — Prometheus, Datadog, CloudWatch.

### Поддерживаемые бэкенды

Prometheus, Datadog, New Relic, CloudWatch, InfluxDB, Graphite, StatsD, Dynatrace, Elastic, Wavefront, OpenTelemetry.

### Подключение к Spring Boot

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<!-- Для экспорта в Prometheus -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: health, info, metrics, prometheus
  metrics:
    tags:
      application: ${spring.application.name}
    distribution:
      percentiles-histogram:
        http.server.requests: true
      sla:
        http.server.requests: 100ms, 500ms, 1s
```

### MeterRegistry — центральный компонент

`MeterRegistry` — интерфейс, через который регистрируются все метрики. Spring Boot автоматически создаёт и настраивает нужную реализацию.

<details><summary>Пример OrderMetrics с Counter, Timer, Gauge</summary>

```java
@Component
public class OrderMetrics {
    private final Counter createdOrders;
    private final Timer orderProcessingTimer;
    private final AtomicInteger pendingOrders = new AtomicInteger(0);

    public OrderMetrics(MeterRegistry registry) {
        this.createdOrders = Counter.builder("orders.created.total")
            .description("Total number of created orders")
            .register(registry);

        this.orderProcessingTimer = Timer.builder("orders.processing.duration")
            .description("Order processing duration")
            .publishPercentiles(0.5, 0.95, 0.99)
            .publishPercentileHistogram()
            .register(registry);

        Gauge.builder("orders.pending", pendingOrders, AtomicInteger::get)
            .description("Number of pending orders")
            .register(registry);
    }

    public void onOrderCreated() {
        createdOrders.increment();
        pendingOrders.incrementAndGet();
    }

    public void onOrderProcessed() {
        pendingOrders.decrementAndGet();
    }

    public Timer getProcessingTimer() {
        return orderProcessingTimer;
    }
}
```

</details>

### Аннотация @Timed

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {

    @Timed(value = "orders.get.time",
           description = "Time to get orders",
           percentiles = {0.5, 0.95, 0.99})
    @GetMapping
    public List<Order> getOrders() {
        return orderService.findAll();
    }
}
```

Для работы `@Timed` необходимо зарегистрировать `TimedAspect`:

```java
@Configuration
public class MetricsConfig {
    @Bean
    public TimedAspect timedAspect(MeterRegistry registry) {
        return new TimedAspect(registry);
    }
}
```

### Тэги (Dimensions) — ключевая концепция

Тэги позволяют разбивать метрики по измерениям (dimensions). Одна метрика с разными тэгами даёт множество временных рядов.

```java
Counter.builder("http.requests")
    .tag("method", "GET")
    .tag("uri", "/api/orders")
    .tag("status", "200")
    .register(registry);
```

```java
// Общие тэги для всех метрик приложения
@Bean
public MeterRegistryCustomizer<MeterRegistry> commonTags() {
    return registry -> registry.config()
        .commonTags("service", "order-service")
        .commonTags("env", "production")
        .commonTags("region", "eu-west-1");
}
```

### Автоматические метрики Spring Boot

| Группа | Метрики | Описание |
|--------|---------|----------|
| JVM | `jvm.memory.used`, `jvm.memory.max`, `jvm.gc.pause` | Память, GC, потоки |
| HTTP | `http.server.requests` | Все HTTP-запросы (метод, URI, статус, время) |
| HikariCP | `hikaricp.connections.active`, `hikaricp.connections.idle` | Пул соединений к БД |
| Kafka | `kafka.consumer.records.consumed.total` | Метрики Kafka consumer/producer |
| Cache | `cache.gets`, `cache.puts`, `cache.evictions` | Метрики кэша |
| Logback | `logback.events` | Количество лог-событий по уровням |
| System | `system.cpu.usage`, `process.cpu.usage` | CPU, открытые файлы |

<details><summary>Кастомный MeterBinder</summary>

```java
@Component
public class BusinessMetricsBinder implements MeterBinder {
    private final OrderRepository orderRepository;

    public BusinessMetricsBinder(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    @Override
    public void bindTo(MeterRegistry registry) {
        Gauge.builder("orders.total.in.db", orderRepository, OrderRepository::count)
            .description("Total orders in database")
            .register(registry);
    }
}
```

</details>

<details><summary>Пример Prometheus-выхода (/actuator/prometheus)</summary>

```
# HELP http_server_requests_seconds Duration of HTTP server request handling
# TYPE http_server_requests_seconds histogram
http_server_requests_seconds_bucket{method="GET",status="200",uri="/api/orders",le="0.1"} 450
http_server_requests_seconds_bucket{method="GET",status="200",uri="/api/orders",le="0.5"} 498
http_server_requests_seconds_bucket{method="GET",status="200",uri="/api/orders",le="+Inf"} 500
http_server_requests_seconds_count{method="GET",status="200",uri="/api/orders"} 500
http_server_requests_seconds_sum{method="GET",status="200",uri="/api/orders"} 12.35

# HELP jvm_memory_used_bytes The amount of used memory
# TYPE jvm_memory_used_bytes gauge
jvm_memory_used_bytes{area="heap",id="G1 Eden Space"} 1.2345678E7
```

</details>

### Важное
- Micrometer — **фасад**, а не реализация. Без подключённого registry (например, `micrometer-registry-prometheus`) метрики собираются, но никуда не экспортируются.
- **Dimensional metrics** (тэги) — основа модели Micrometer. В отличие от hierarchical (Graphite-стиль `orders.eu.success`), тэги позволяют гибко фильтровать и агрегировать.
- Spring Boot 3.x использует **Micrometer Observation API** — единый механизм для метрик и трейсов одновременно.

### Частые ошибки
- **Создание метрики при каждом вызове**: `Counter.builder(...).register(registry)` внутри метода — метрика должна создаваться один раз (в конструкторе или `@PostConstruct`).
- **Динамические тэги с высокой кардинальностью**: `tag("userId", userId)` создаёт отдельный time series для каждого пользователя.
- **Забыть подключить TimedAspect**: без него аннотация `@Timed` на кастомных методах не работает (на контроллерах работает через WebMvcMetricsFilter).
- **Не настроить exposure endpoints**: по умолчанию endpoint `/actuator/prometheus` не доступен; нужно явно добавить в `management.endpoints.web.exposure.include`.

### Как используется в 2026
- **Micrometer Observation API** — унифицированный API, одно «наблюдение» автоматически создаёт и метрику, и span трейса.
- **Micrometer Context Propagation** — автоматическая передача контекста (traceId, MDC) между потоками, включая реактивные цепочки и виртуальные потоки.
- **Micrometer 2.x** с нативной поддержкой OpenTelemetry Protocol (OTLP) для экспорта метрик напрямую в OTel Collector.
- Большинство Spring Boot-стартеров (Data, Security, Kafka, RabbitMQ) поставляются с автоматическими Observation-хуками.

> **На собеседовании:** ключевой тезис — Micrometer является фасадом (как SLF4J для логов). Покажите, что умеете выбирать тип метрики (Counter vs Gauge vs Timer), знаете про ограничение кардинальности тэгов и понимаете разницу между `@Timed` и Observation API в Spring Boot 3.x.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое Prometheus и как он собирает метрики?
<!-- grade: middle -->

Prometheus — open-source система мониторинга и алертинга (CNCF), основанная на модели временных рядов (time series database). Главная особенность — pull-модель: Prometheus сам периодически опрашивает (scrape) endpoints приложений по HTTP.

```
┌──────────────┐    GET /actuator/prometheus     ┌──────────────┐
│  Prometheus  │ ─────────────────────────────►  │  Spring Boot │
│   Server     │ ◄─────────────────────────────  │  Application │
│              │    text/plain (metrics)          │              │
└──────────────┘                                  └──────────────┘
     │  scrape каждые 15s
     ▼
┌──────────────┐
│   TSDB       │  Хранение временных рядов
│  (storage)   │
└──────────────┘
```

**Преимущества pull-модели:**
- Prometheus сам контролирует частоту и таймауты
- Легко обнаружить «упавший» сервис (scrape failed)
- Не нужно настраивать push-агенты на каждом сервисе
- Проще отлаживать: можно зайти на endpoint вручную

<details><summary>Конфигурация prometheus.yml</summary>

```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "alert_rules.yml"
  - "recording_rules.yml"

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'order-service'
    metrics_path: '/actuator/prometheus'
    scrape_interval: 10s
    static_configs:
      - targets: ['order-service:8080']
        labels:
          environment: 'production'

  # Service Discovery в Kubernetes
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        action: replace
        target_label: __metrics_path__
        regex: (.+)
```

</details>

### Типы метрик в Prometheus

| Тип | Описание | Функции PromQL |
|-----|----------|---------------|
| counter | Монотонно растущее значение | `rate()`, `increase()`, `irate()` |
| gauge | Значение, меняющееся в обе стороны | `avg()`, `min()`, `max()`, `delta()` |
| histogram | Распределение значений по бакетам | `histogram_quantile()`, `rate()` по бакетам |
| summary | Клиентские перцентили (вычисляются на стороне приложения) | Прямое чтение перцентилей |

### PromQL — язык запросов

```promql
# Скорость роста (requests per second) за последние 5 минут
rate(http_server_requests_seconds_count{job="order-service"}[5m])

# Процент ошибок (5xx)
sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m]))
/
sum(rate(http_server_requests_seconds_count[5m]))
* 100

# 99-й перцентиль времени ответа
histogram_quantile(0.99,
  sum(rate(http_server_requests_seconds_bucket{job="order-service"}[5m])) by (le)
)

# Память JVM heap
jvm_memory_used_bytes{area="heap"} / jvm_memory_max_bytes{area="heap"} * 100
```

<details><summary>Recording Rules и Alerting Rules</summary>

```yaml
# recording_rules.yml
groups:
  - name: http_rules
    interval: 30s
    rules:
      - record: job:http_requests:rate5m
        expr: sum(rate(http_server_requests_seconds_count[5m])) by (job)

      - record: job:http_errors:rate5m
        expr: sum(rate(http_server_requests_seconds_count{status=~"5.."}[5m])) by (job)

      - record: job:http_error_ratio:rate5m
        expr: job:http_errors:rate5m / job:http_requests:rate5m
```

```yaml
# alert_rules.yml
groups:
  - name: application_alerts
    rules:
      - alert: HighErrorRate
        expr: job:http_error_ratio:rate5m > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate on {{ $labels.job }}"
          description: "Error rate is {{ $value | humanizePercentage }} for 5 minutes"

      - alert: HighLatency
        expr: histogram_quantile(0.99, sum(rate(http_server_requests_seconds_bucket[5m])) by (le, job)) > 1
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "High p99 latency on {{ $labels.job }}"
          description: "p99 latency is {{ $value }}s"
```

</details>

### Важное
- Prometheus хранит данные **локально** на диске. Для долгосрочного хранения используйте remote storage (Thanos, Cortex, Grafana Mimir).
- **Pull-модель** не подходит для короткоживущих задач (batch jobs). Для них используйте **Pushgateway**.
- **Лейблы** — мощный инструмент, но каждая уникальная комбинация лейблов создаёт отдельный time series. Следите за кардинальностью.
- Prometheus **не гарантирует** 100% точность данных: при перезапуске возможны пробелы, scrape может не совпадать с моментом события.

### Частые ошибки
- **Не использовать `rate()` для counter**: абсолютное значение counter бесполезно, нужна скорость изменения.
- **Слишком короткий `[range]` в `rate()`**: `rate(...[1m])` при scrape_interval=15s даст всего 4 точки — шумно и неточно. Рекомендуется `[5m]`.
- **Хранить метрики вечно**: Prometheus не предназначен для долгосрочного хранения. Настройте retention (по умолчанию 15 дней).
- **Злоупотребление Pushgateway**: он предназначен только для batch jobs, не для замены pull-модели.

### Как используется в 2026
- **Prometheus 3.x** с нативными гистограммами (Native Histograms) и улучшенной производительностью TSDB.
- **Grafana Mimir** — горизонтально масштабируемое долгосрочное хранилище, совместимое с Prometheus API.
- **Prometheus Agent Mode** — режим, в котором Prometheus только скрейпит и отправляет метрики через remote write, не храня данных локально.
- **Service Discovery** — автоматическое обнаружение целей через Kubernetes SD, Consul SD, DNS SD.
- **OpenMetrics** — стандартизированный формат метрик (эволюция Prometheus exposition format), поддерживаемый OTel.

> **На собеседовании:** акцентируйте внимание на pull-модели — это главная архитектурная особенность Prometheus. Покажите знание PromQL хотя бы на уровне `rate()` и `histogram_quantile()`. Частая ошибка — не упомянуть ограничения (локальное хранение, проблема кардинальности лейблов).

[к оглавлению](#мониторинг-и-observability)

---

## Что такое Grafana и как визуализировать метрики?
<!-- grade: middle -->

Grafana — open-source платформа для визуализации, мониторинга и анализа данных из множества источников. Grafana не хранит данные — только визуализирует их из подключённых data sources.

> **Аналогия из жизни:** Grafana — это телевизор с множеством входов (HDMI, USB, антенна). Он не создаёт контент, но показывает видео из любого подключённого источника в удобном формате.

### Ключевые компоненты

| Компонент | Описание |
|-----------|----------|
| Data Sources | Подключения к хранилищам: Prometheus (PromQL), Loki (LogQL), Tempo, Elasticsearch, PostgreSQL |
| Dashboards | Набор панелей для визуализации данных |
| Panels | Отдельные визуализации: Time series, Stat, Gauge, Table, Heatmap, Logs, Traces |

<details><summary>Настройка data sources (provisioning)</summary>

```yaml
# grafana/provisioning/datasources/datasources.yml
apiVersion: 1
datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
    isDefault: true
    jsonData:
      exemplarTraceIdDestinations:
        - name: traceID
          datasourceUid: tempo

  - name: Loki
    type: loki
    access: proxy
    url: http://loki:3100
    jsonData:
      derivedFields:
        - name: TraceID
          matcherRegex: "traceId=(\\w+)"
          url: "$${__value.raw}"
          datasourceUid: tempo

  - name: Tempo
    type: tempo
    access: proxy
    url: http://tempo:3200
    jsonData:
      tracesToLogsV2:
        datasourceUid: loki
        filterByTraceID: true
      tracesToMetrics:
        datasourceUid: prometheus
```

</details>

### Стандартные дашборды для Spring Boot

| ID | Название | Что показывает |
|----|---------|---------------|
| 4701 | JVM (Micrometer) | Heap, non-heap, GC, threads, classloading |
| 11378 | Spring Boot Statistics | HTTP requests, response times, error rates |
| 6756 | HikariCP | Connections (active, idle, pending), timeouts |
| 7362 | Kafka Consumer | Lag, records consumed, fetch latency |

### Пример PromQL-запросов для дашборда

```promql
# HTTP Requests Rate (по статусу)
sum by (status) (rate(http_server_requests_seconds_count{application="$application"}[5m]))

# HTTP Response Time (p99, p95, p50)
histogram_quantile(0.99, sum(rate(http_server_requests_seconds_bucket{application="$application"}[5m])) by (le))

# JVM Heap Used
jvm_memory_used_bytes{application="$application", area="heap"}

# HikariCP Active Connections
hikaricp_connections_active{application="$application"}
```

### Переменные (Variables) в дашбордах

Переменные позволяют создавать параметризованные дашборды:

```
# Variable: application
Type: Query
Query: label_values(jvm_memory_used_bytes, application)

# Variable: instance
Type: Query
Query: label_values(jvm_memory_used_bytes{application="$application"}, instance)
```

Используются в запросах как `$application`, `$instance`.

<details><summary>Docker Compose для локального observability-стека</summary>

```yaml
# docker-compose-observability.yml
version: '3.8'
services:
  prometheus:
    image: prom/prometheus:v2.53.0
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus/alert_rules.yml:/etc/prometheus/alert_rules.yml
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.retention.time=7d'
      - '--web.enable-lifecycle'

  grafana:
    image: grafana/grafana:11.1.0
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
      - GF_INSTALL_PLUGINS=grafana-clock-panel
    volumes:
      - ./grafana/provisioning:/etc/grafana/provisioning
      - grafana-data:/var/lib/grafana

  loki:
    image: grafana/loki:3.1.0
    ports:
      - "3100:3100"
    command: -config.file=/etc/loki/local-config.yaml

  tempo:
    image: grafana/tempo:2.5.0
    ports:
      - "3200:3200"
      - "4317:4317"    # OTLP gRPC
      - "4318:4318"    # OTLP HTTP
    command: ["-config.file=/etc/tempo/tempo.yaml"]
    volumes:
      - ./tempo/tempo.yaml:/etc/tempo/tempo.yaml

  promtail:
    image: grafana/promtail:3.1.0
    volumes:
      - /var/log:/var/log
      - ./promtail/config.yml:/etc/promtail/config.yml
    command: -config.file=/etc/promtail/config.yml

volumes:
  grafana-data:
```

</details>

### Важное
- Grafana **не хранит данные** — только визуализирует. Данные остаются в Prometheus, Loki, Tempo и т.д.
- **Dashboard as Code**: дашборды можно хранить как JSON и версионировать в Git (provisioning).
- **Корреляция сигналов**: настройте связи между Prometheus -> Tempo (exemplars), Loki -> Tempo (derived fields), Tempo -> Loki/Prometheus (trace-to-logs/metrics).
- Grafana поддерживает **Explore** режим для ad-hoc запросов (отладки), в отличие от дашбордов, которые предназначены для постоянного мониторинга.

### Частые ошибки
- **Перегруженные дашборды**: 30 панелей на одном дашборде — нечитаемо. Разделяйте по уровням: Overview -> Service -> Detail.
- **Хардкод значений**: используйте переменные (`$application`, `$instance`) вместо конкретных имён в запросах.
- **Отсутствие документации на дашборде**: добавляйте Text-панели с описанием, что мониторится и какие пороговые значения нормальны.
- **Не настроенные корреляции**: клик из графика метрик должен вести к трейсу, из трейса — к логам.

### Как используется в 2026
- **Grafana 11+** с нативной поддержкой AI-ассистента (Grafana LLM) для генерации PromQL/LogQL запросов.
- **Grafana Scenes** — новый фреймворк для программного создания дашбордов на TypeScript.
- **Grafana OnCall** — встроенный инструмент для управления дежурствами и эскалациями.
- **Unified Alerting** — единая система алертинга для всех data sources (Prometheus, Loki, SQL и др.).
- **Grafana Cloud** с бесплатным уровнем — популярный выбор для стартапов.

> **На собеседовании:** подчеркните, что Grafana — инструмент визуализации, а не хранения данных. Упомяните корреляцию сигналов (metrics -> traces -> logs) — это показывает зрелое понимание observability-стека. Знание конкретных dashboard ID (4701, 11378) производит хорошее впечатление.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое distributed tracing?
<!-- grade: middle -->

Distributed tracing (распределённая трассировка) — метод отслеживания прохождения запроса через множество сервисов в распределённой системе. Каждый запрос получает уникальный идентификатор (traceId), который передаётся между сервисами, позволяя восстановить полную картину обработки.

### Зачем нужен distributed tracing

В микросервисной архитектуре один пользовательский запрос может вызвать цепочку вызовов:

```
Пользователь → API Gateway → Order Service → Payment Service → Notification Service
                                  │                                    │
                                  └── Inventory Service                └── Email Service
                                         │
                                         └── Warehouse Service
```

Без distributed tracing невозможно:
- Понять, какой сервис в цепочке вызвал задержку
- Отследить, где произошла ошибка
- Увидеть зависимости между сервисами
- Оценить влияние одного сервиса на остальные

### Основные концепции

| Концепция | Описание |
|-----------|----------|
| Trace | Полная запись обработки одного запроса через все сервисы (уникальный `traceId`) |
| Span | Единица работы внутри trace: spanId, parentSpanId, operationName, startTime, duration, tags, status |
| Context Propagation | Механизм передачи traceId/spanId между сервисами через HTTP-заголовки, Kafka headers, gRPC metadata |

### W3C Trace Context (стандарт)

```http
GET /api/orders HTTP/1.1
Host: order-service
traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01

# traceparent формат:
# {version}-{trace-id}-{parent-id}-{trace-flags}
# 00       - 4bf92f...  - 00f067...  - 01 (sampled)
```

### Waterfall-диаграмма (визуализация трейса)

```
TraceID: abc123def456

├── API Gateway: GET /api/orders [250ms] ────────────────────────────────────
│   ├── Order Service: getOrders [200ms] ────────────────────────────────
│   │   ├── PostgreSQL: SELECT * FROM orders [15ms] ──────
│   │   ├── Redis: GET cache:orders [2ms] ──
│   │   ├── Inventory Service: checkStock [120ms] ─────────────────
│   │   │   └── Warehouse DB: SELECT stock [10ms] ────
│   │   └── Payment Service: validatePayments [50ms] ──────────
│   │       └── Stripe API: GET /charges [40ms] ────────
│   └── Auth: validateToken [5ms] ──
```

По этой диаграмме видно:
- Общее время запроса — 250ms
- Самая долгая операция — checkStock (120ms) — кандидат на оптимизацию
- Запросы к Redis и Auth выполняются быстро
- Order Service ждёт последовательно Inventory и Payment — можно параллелить

### Стратегии семплирования (Sampling)

Трейсить 100% запросов в production дорого. Стратегии:

| Стратегия | Описание | Когда использовать |
|-----------|----------|-------------------|
| Head-based | Решение о семплировании принимается в начале трейса | Простая, но может пропустить интересные трейсы |
| Tail-based | Решение принимается после завершения трейса | Позволяет сохранять трейсы с ошибками или высокой задержкой |
| Rate limiting | Фиксированное количество трейсов в секунду | Контроль нагрузки |
| Probabilistic | Фиксированный процент (1%, 10%) | Простейший вариант |

<details><summary>Пример tail-based sampling в OTel Collector</summary>

```yaml
processors:
  tail_sampling:
    decision_wait: 10s
    policies:
      - name: errors
        type: status_code
        status_code: {status_codes: [ERROR]}
      - name: slow-traces
        type: latency
        latency: {threshold_ms: 1000}
      - name: probabilistic
        type: probabilistic
        probabilistic: {sampling_percentage: 10}
```

</details>

### Важное
- **Context propagation** — критически важен. Если хотя бы один сервис в цепочке не передаёт контекст, трейс обрывается.
- **Span — не лог**. Span описывает единицу работы с началом и концом. Не создавайте span для каждой строки кода.
- **Instrumentation библиотеки** (JDBC, HTTP-клиенты, Kafka, gRPC) автоматически создают spans — не нужно оборачивать каждый вызов вручную.
- **Sampling** обязателен в production: без него overhead на трейсинг может составлять 5-15% ресурсов.

### Частые ошибки
- **Потеря контекста при асинхронных вызовах**: при использовании `@Async`, `CompletableFuture`, реактивных цепочек контекст нужно передавать явно.
- **Слишком детальные спаны**: создание span на каждый вызов метода перегружает систему. Спаны нужны для I/O-операций и бизнес-логики.
- **Игнорирование семплирования**: 100% трейсов в production — это огромный объём данных и нагрузка на хранилище.
- **Несогласованные имена операций**: `getOrder`, `GET_ORDER`, `order.get` — разные имена для одной операции усложняют анализ.

### Как используется в 2026
- **W3C Trace Context** — стандарт де-факто для propagation.
- **OpenTelemetry** — единый API и SDK для трейсинга, заменивший Jaeger client, Zipkin Brave и OpenTracing.
- **Grafana Tempo** и **Jaeger v2** — популярные бэкенды для хранения трейсов.
- **Trace-based testing** (Tracetest) — использование трейсов для написания интеграционных тестов.
- **Continuous profiling + tracing** — связка профилирования с трейсами (Grafana Pyroscope).

> **На собеседовании:** объясните концепции Trace/Span/Context Propagation и покажите, что умеете читать waterfall-диаграмму. Упомяните необходимость семплирования в production и W3C Trace Context как стандарт. Частая ошибка — забыть про потерю контекста в асинхронных вызовах.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое OpenTelemetry?
<!-- grade: middle -->

OpenTelemetry (OTel) — vendor-neutral open-source стандарт и набор инструментов для генерации, сбора и экспорта телеметрии (traces, metrics, logs). Проект CNCF, объединивший OpenTracing и OpenCensus.

```
┌─────────────────────────────────────────────────────────────────┐
│                     Ваше приложение                              │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │  OTel SDK    │  │ Auto-instrum │  │ Manual spans │           │
│  │  (Traces,    │  │ (JDBC, HTTP, │  │ (ваш код)   │           │
│  │   Metrics,   │  │  Kafka, gRPC)│  │              │           │
│  │   Logs)      │  │              │  │              │           │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘           │
│         └──────────────────┼─────────────────┘                   │
│                            ▼                                     │
│                  ┌──────────────────┐                            │
│                  │  OTLP Exporter   │                            │
│                  └────────┬─────────┘                            │
└───────────────────────────┼──────────────────────────────────────┘
                            │ OTLP (gRPC/HTTP)
                            ▼
                  ┌──────────────────┐
                  │  OTel Collector  │
                  │                  │
                  │ Receivers → Processors → Exporters             │
                  └──┬──────────┬──────────┬───────────────────────┘
                     │          │          │
                     ▼          ▼          ▼
               ┌─────────┐ ┌────────┐ ┌────────┐
               │  Jaeger  │ │Promethe│ │  Loki  │
               │  Tempo   │ │  us    │ │        │
               └─────────┘ └────────┘ └────────┘
```

### Три сигнала OpenTelemetry

1. **Traces** — распределённые трейсы (stable)
2. **Metrics** — метрики с поддержкой Counter, Gauge, Histogram (stable)
3. **Logs** — логи с корреляцией трейсами (stable с 2024)

### Auto-instrumentation vs Manual

**Auto-instrumentation** — Java-агент, автоматически инструментирующий популярные библиотеки:

```bash
java -javaagent:opentelemetry-javaagent.jar \
  -Dotel.service.name=order-service \
  -Dotel.exporter.otlp.endpoint=http://otel-collector:4317 \
  -Dotel.traces.sampler=parentbased_traceidratio \
  -Dotel.traces.sampler.arg=0.1 \
  -jar order-service.jar
```

Поддерживаемые библиотеки (автоматически): Spring MVC/WebFlux, JDBC, Hibernate, Kafka, RabbitMQ, gRPC, OkHttp, RestTemplate, WebClient, Jedis, Lettuce, Logback, Log4j2.

<details><summary>Manual instrumentation — явное создание спанов</summary>

```java
import io.opentelemetry.api.OpenTelemetry;
import io.opentelemetry.api.trace.Span;
import io.opentelemetry.api.trace.Tracer;
import io.opentelemetry.context.Scope;

@Service
public class OrderService {
    private final Tracer tracer;

    public OrderService(OpenTelemetry openTelemetry) {
        this.tracer = openTelemetry.getTracer("order-service");
    }

    public Order processOrder(OrderRequest request) {
        Span span = tracer.spanBuilder("processOrder")
            .setAttribute("order.type", request.getType())
            .setAttribute("order.items.count", request.getItems().size())
            .startSpan();

        try (Scope scope = span.makeCurrent()) {
            Order order = createOrder(request);
            span.setAttribute("order.id", order.getId());
            span.addEvent("Order validated");

            processPayment(order);
            span.addEvent("Payment processed");

            return order;
        } catch (Exception e) {
            span.setStatus(StatusCode.ERROR, e.getMessage());
            span.recordException(e);
            throw e;
        } finally {
            span.end();
        }
    }
}
```

</details>

<details><summary>Конфигурация OTel Collector</summary>

```yaml
# otel-collector-config.yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318

processors:
  batch:
    timeout: 5s
    send_batch_size: 1024
  memory_limiter:
    check_interval: 1s
    limit_mib: 512
  attributes:
    actions:
      - key: environment
        value: production
        action: upsert
  tail_sampling:
    decision_wait: 10s
    policies:
      - name: errors-policy
        type: status_code
        status_code:
          status_codes: [ERROR]
      - name: slow-traces-policy
        type: latency
        latency:
          threshold_ms: 500
      - name: probabilistic-policy
        type: probabilistic
        probabilistic:
          sampling_percentage: 5

exporters:
  otlp/tempo:
    endpoint: tempo:4317
    tls:
      insecure: true
  prometheus:
    endpoint: 0.0.0.0:8889
  loki:
    endpoint: http://loki:3100/loki/api/v1/push

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [memory_limiter, batch, tail_sampling]
      exporters: [otlp/tempo]
    metrics:
      receivers: [otlp]
      processors: [memory_limiter, batch]
      exporters: [prometheus]
    logs:
      receivers: [otlp]
      processors: [memory_limiter, batch, attributes]
      exporters: [loki]
```

</details>

### OTel vs Vendor-specific решения

| Аспект | OpenTelemetry | Jaeger / Zipkin |
|--------|--------------|----------------|
| Стандарт | Vendor-neutral (CNCF) | Специфичный формат |
| Сигналы | Traces + Metrics + Logs | Только traces |
| Переключение бэкенда | Изменение конфига exporter'а | Переписывание интеграций |
| Auto-instrumentation | Единый Java-агент | Отдельные библиотеки |
| Будущее | Активно развивается, стандарт индустрии | Jaeger v2 использует OTel SDK |

### Важное
- OpenTelemetry — это **стандарт и SDK**, а не бэкенд для хранения. Данные экспортируются в Jaeger, Tempo, Datadog и т.д.
- **OTel Collector** — рекомендуемый промежуточный слой между приложениями и бэкендами. Позволяет менять бэкенд без изменения приложений.
- **Auto-instrumentation** покрывает 80-90% потребностей. Manual instrumentation нужен только для кастомной бизнес-логики.
- **OTLP (OpenTelemetry Protocol)** — единый протокол для всех сигналов (gRPC и HTTP).

### Частые ошибки
- **Смешивание OTel с Jaeger/Zipkin SDK**: используйте только OTel SDK, а Jaeger/Tempo — только как бэкенд.
- **Отправка напрямую в бэкенд** без Collector: теряется возможность батчинга, семплирования, обогащения данных.
- **Не настроен memory_limiter в Collector**: при высокой нагрузке Collector может съесть всю память.
- **Забыть про context propagation**: OTel автоматически передаёт контекст для HTTP и gRPC, но для Kafka/RabbitMQ нужна дополнительная настройка.

### Как используется в 2026
- **OpenTelemetry** — стандарт индустрии. Все крупные вендоры (Datadog, New Relic, Dynatrace, Splunk) поддерживают OTLP.
- **OTel Java Agent 2.x** — с поддержкой виртуальных потоков (Project Loom), нативных образов (GraalVM).
- **OTel Logs** — stable API, заменяет прямую интеграцию с Logstash/Fluentd.
- **OTel Profiling** — экспериментальный сигнал для continuous profiling.
- **Spring Boot 3.x** — нативная интеграция через Micrometer Observation API -> OTel bridge.

> **На собеседовании:** ключевое — OTel является vendor-neutral стандартом, не бэкендом. Объясните роль Collector как промежуточного слоя. Покажите понимание разницы между auto-instrumentation (Java-агент) и manual instrumentation. Сильный ответ включает упоминание OTLP как единого протокола и трёх сигналов (traces, metrics, logs).

[к оглавлению](#мониторинг-и-observability)

---

## Как настроить distributed tracing в Spring Boot?
<!-- grade: middle -->

Начиная с Spring Boot 3.x, для distributed tracing используется Micrometer Tracing (замена Spring Cloud Sleuth). Micrometer Tracing предоставляет фасад, а конкретная реализация (OTel или Brave/Zipkin) подключается как bridge.

<details><summary>Подключение зависимостей (pom.xml)</summary>

```xml
<dependencies>
    <!-- Actuator (обязательно) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

    <!-- Micrometer Tracing — фасад -->
    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-tracing</artifactId>
    </dependency>

    <!-- Bridge к OpenTelemetry -->
    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-tracing-bridge-otel</artifactId>
    </dependency>

    <!-- OTLP exporter для отправки трейсов -->
    <dependency>
        <groupId>io.opentelemetry</groupId>
        <artifactId>opentelemetry-exporter-otlp</artifactId>
    </dependency>

    <!-- Для Prometheus метрик с exemplars -->
    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-registry-prometheus</artifactId>
    </dependency>
</dependencies>
```

</details>

### Конфигурация application.yml

```yaml
spring:
  application:
    name: order-service

management:
  tracing:
    sampling:
      probability: 1.0          # 1.0 = 100% для dev, 0.1 = 10% для prod
    propagation:
      type: w3c                  # W3C Trace Context (по умолчанию)
  otlp:
    tracing:
      endpoint: http://otel-collector:4318/v1/traces
  metrics:
    distribution:
      percentiles-histogram:
        http.server.requests: true
    tags:
      application: ${spring.application.name}

logging:
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} [%thread] [%X{traceId}/%X{spanId}] %-5level %logger{36} - %msg%n"
```

### Автоматическое распространение контекста

Spring Boot автоматически передаёт trace context через:
- **RestTemplate** / **WebClient** — HTTP-заголовки `traceparent`
- **Spring Kafka** — Kafka Record Headers
- **Spring AMQP** — RabbitMQ message headers
- **Spring gRPC** — gRPC metadata

```java
@Service
public class OrderService {
    private final RestClient restClient;

    public OrderService(RestClient.Builder builder) {
        this.restClient = builder
            .baseUrl("http://inventory-service:8080")
            .build();
    }

    public InventoryResponse checkStock(String productId) {
        // traceId/spanId автоматически передаются в заголовках
        return restClient.get()
            .uri("/api/inventory/{id}", productId)
            .retrieve()
            .body(InventoryResponse.class);
    }
}
```

### Кастомные спаны с Observation API

```java
@Service
public class OrderService {
    private final ObservationRegistry observationRegistry;

    public OrderService(ObservationRegistry observationRegistry) {
        this.observationRegistry = observationRegistry;
    }

    public Order processOrder(OrderRequest request) {
        return Observation.createNotStarted("order.processing", observationRegistry)
            .lowCardinalityKeyValue("order.type", request.getType())
            .highCardinalityKeyValue("order.id", request.getId())
            .observe(() -> {
                // Автоматически создаёт:
                // 1. Span с именем "order.processing"
                // 2. Timer-метрику "order.processing"
                Order order = createOrder(request);
                validateOrder(order);
                return order;
            });
    }
}
```

### Аннотация @Observed

```java
@Configuration
public class ObservationConfig {
    @Bean
    public ObservedAspect observedAspect(ObservationRegistry registry) {
        return new ObservedAspect(registry);
    }
}

@Service
public class PaymentService {
    @Observed(name = "payment.process",
              contextualName = "processing-payment",
              lowCardinalityKeyValues = {"payment.provider", "stripe"})
    public PaymentResult processPayment(PaymentRequest request) {
        // Автоматически создаётся span + метрика
        return callPaymentProvider(request);
    }
}
```

### Логирование с traceId

Micrometer Tracing автоматически добавляет `traceId` и `spanId` в MDC (Mapped Diagnostic Context):

```java
@Service
public class OrderService {
    private static final Logger log = LoggerFactory.getLogger(OrderService.class);

    public Order processOrder(OrderRequest request) {
        // traceId/spanId автоматически добавляются через MDC
        log.info("Processing order for customer: {}", request.getCustomerId());
        // Лог: 2026-04-22 10:30:00 [main] [abc123/def456] INFO  OrderService - Processing order...
        return doProcess(request);
    }
}
```

<details><summary>Конфигурация logback для JSON-формата</summary>

```xml
<!-- logback-spring.xml -->
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="net.logstash.logback.encoder.LogstashEncoder">
            <includeMdcKeyName>traceId</includeMdcKeyName>
            <includeMdcKeyName>spanId</includeMdcKeyName>
        </encoder>
    </appender>
    <root level="INFO">
        <appender-ref ref="CONSOLE" />
    </root>
</configuration>
```

Результат:
```json
{
  "@timestamp": "2026-04-22T10:30:00.123Z",
  "level": "INFO",
  "logger_name": "com.example.OrderService",
  "message": "Processing order for customer: 42",
  "traceId": "abc123def456789",
  "spanId": "def456789abc",
  "service": "order-service"
}
```

</details>

### Важное
- **Micrometer Tracing** — фасад, аналогичный SLF4J. Нужен bridge (`micrometer-tracing-bridge-otel`) для конкретной реализации.
- **Observation API** — единый механизм, создающий одновременно метрику и span. Используйте `@Observed` вместо отдельных `@Timed` + ручных спанов.
- **sampling.probability** = 1.0 в dev/staging, 0.01-0.1 в production. В production используйте tail-based sampling на стороне OTel Collector.
- Spring Boot 3.x автоматически пропагирует контекст через стандартные HTTP-клиенты, Kafka, RabbitMQ.

### Частые ошибки
- **Забыть подключить bridge**: без `micrometer-tracing-bridge-otel` трейсы не генерируются.
- **sampling.probability = 1.0 в production**: 100% семплирование на высоконагруженном сервисе создаёт огромный overhead.
- **Потеря контекста в `@Async`**: стандартный `TaskExecutor` не передаёт trace context. Используйте `ContextPropagatingTaskDecorator`.
- **Не добавить traceId в логи**: без `%X{traceId}` в log pattern невозможно связать логи с трейсами.
- **Путать Spring Cloud Sleuth и Micrometer Tracing**: Sleuth устарел и не поддерживается в Spring Boot 3.x.

<details><summary>Правильная передача контекста в @Async</summary>

```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {

    private final ObservationRegistry observationRegistry;

    public AsyncConfig(ObservationRegistry observationRegistry) {
        this.observationRegistry = observationRegistry;
    }

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setTaskDecorator(new ContextPropagatingTaskDecorator());
        executor.initialize();
        return executor;
    }
}
```

</details>

### Как используется в 2026
- **Micrometer Observation API** — стандартный способ инструментирования в Spring-экосистеме.
- **OTLP экспорт** — нативная поддержка в Spring Boot через `management.otlp.tracing` и `management.otlp.metrics`.
- **Виртуальные потоки (Project Loom)** — Micrometer Context Propagation корректно работает с virtual threads.
- **Spring Boot 3.4+** — улучшенная поддержка OTel Logs, автоматическая корреляция логов с трейсами через OTLP.
- **Testcontainers + OTel** — интеграционные тесты с реальным OTel Collector для проверки трейсинга.

> **На собеседовании:** обязательно упомяните переход от Spring Cloud Sleuth к Micrometer Tracing в Spring Boot 3.x. Покажите знание Observation API как единого механизма для метрик и трейсов. Ключевая ошибка на собеседовании — не знать про необходимость bridge и `ContextPropagatingTaskDecorator` для `@Async`.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое Spring Boot Actuator?
<!-- grade: junior -->

Spring Boot Actuator — модуль Spring Boot, предоставляющий production-ready функции для мониторинга и управления приложением через HTTP-эндпоинты или JMX.

### Подключение

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### Основные endpoints

| Endpoint | Описание | По умолчанию |
|----------|----------|-------------|
| `/actuator/health` | Состояние приложения и зависимостей | Включён |
| `/actuator/info` | Информация о приложении | Включён |
| `/actuator/metrics` | Список всех метрик | Выключен |
| `/actuator/prometheus` | Метрики в формате Prometheus | Выключен |
| `/actuator/env` | Переменные окружения | Выключен |
| `/actuator/beans` | Все Spring-бины | Выключен |
| `/actuator/loggers` | Управление уровнями логирования | Выключен |
| `/actuator/threaddump` | Дамп потоков | Выключен |
| `/actuator/heapdump` | Дамп кучи (скачиваемый файл) | Выключен |
| `/actuator/mappings` | Все `@RequestMapping` | Выключен |

### Конфигурация

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, info, metrics, prometheus, loggers, env
      base-path: /actuator
  endpoint:
    health:
      show-details: when-authorized  # never | when-authorized | always
      show-components: always
    env:
      show-values: when-authorized

  # Отдельный порт для actuator (рекомендуется для production)
  server:
    port: 9090

  info:
    env:
      enabled: true
    git:
      mode: full
```

### Health Indicators

Spring Boot автоматически регистрирует health indicators для подключённых зависимостей:

| Indicator | Проверяет |
|-----------|----------|
| `DataSourceHealthIndicator` | Подключение к БД |
| `RedisHealthIndicator` | Подключение к Redis |
| `KafkaHealthIndicator` | Подключение к Kafka broker |
| `ElasticsearchHealthIndicator` | Кластер Elasticsearch |
| `RabbitHealthIndicator` | Подключение к RabbitMQ |
| `DiskSpaceHealthIndicator` | Свободное место на диске |

<details><summary>Пример ответа /actuator/health</summary>

```json
{
  "status": "UP",
  "components": {
    "db": {
      "status": "UP",
      "details": {
        "database": "PostgreSQL",
        "validationQuery": "isValid()"
      }
    },
    "redis": {
      "status": "UP",
      "details": {
        "version": "7.2.4"
      }
    },
    "kafka": {
      "status": "UP",
      "details": {
        "brokerId": "1"
      }
    },
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 107374182400,
        "free": 53687091200,
        "threshold": 10485760
      }
    }
  }
}
```

</details>

<details><summary>Кастомный Health Indicator</summary>

```java
@Component
public class PaymentGatewayHealthIndicator implements HealthIndicator {

    private final PaymentGatewayClient client;

    public PaymentGatewayHealthIndicator(PaymentGatewayClient client) {
        this.client = client;
    }

    @Override
    public Health health() {
        try {
            boolean isAvailable = client.ping();
            if (isAvailable) {
                return Health.up()
                    .withDetail("provider", "stripe")
                    .withDetail("responseTime", "45ms")
                    .build();
            } else {
                return Health.down()
                    .withDetail("provider", "stripe")
                    .withDetail("reason", "Ping failed")
                    .build();
            }
        } catch (Exception e) {
            return Health.down(e)
                .withDetail("provider", "stripe")
                .build();
        }
    }
}
```

</details>

### Kubernetes Probes

Spring Boot Actuator автоматически поддерживает Kubernetes health probes:

| Probe | Назначение | Что происходит при fail |
|-------|-----------|----------------------|
| Startup | Приложение запустилось? | Контейнер перезапускается |
| Liveness | Приложение работает? | Контейнер перезапускается |
| Readiness | Приложение готово принимать трафик? | Убирается из Service (перестаёт получать трафик) |

<details><summary>Настройка probes в application.yml и Kubernetes deployment</summary>

```yaml
# application.yml
management:
  endpoint:
    health:
      probes:
        enabled: true
      group:
        liveness:
          include: livenessState
        readiness:
          include: readinessState, db, redis
        startup:
          include: livenessState
```

```yaml
# Kubernetes deployment.yml
spec:
  containers:
    - name: order-service
      livenessProbe:
        httpGet:
          path: /actuator/health/liveness
          port: 9090
        initialDelaySeconds: 30
        periodSeconds: 10
        failureThreshold: 3
      readinessProbe:
        httpGet:
          path: /actuator/health/readiness
          port: 9090
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 3
      startupProbe:
        httpGet:
          path: /actuator/health/startup
          port: 9090
        initialDelaySeconds: 5
        periodSeconds: 5
        failureThreshold: 30
```

</details>

### Управление уровнями логирования в runtime

```bash
# Получить текущий уровень логгера
curl http://localhost:9090/actuator/loggers/com.example.service

# Изменить уровень без перезапуска
curl -X POST http://localhost:9090/actuator/loggers/com.example.service \
  -H 'Content-Type: application/json' \
  -d '{"configuredLevel": "DEBUG"}'
```

<details><summary>Безопасность Actuator</summary>

```java
@Configuration
@EnableWebSecurity
public class ActuatorSecurityConfig {

    @Bean
    public SecurityFilterChain actuatorFilterChain(HttpSecurity http) throws Exception {
        http
            .securityMatcher("/actuator/**")
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/actuator/health/**").permitAll()
                .requestMatchers("/actuator/info").permitAll()
                .requestMatchers("/actuator/prometheus").permitAll()
                .requestMatchers("/actuator/**").hasRole("ADMIN")
            )
            .httpBasic(Customizer.withDefaults());
        return http.build();
    }
}
```

</details>

### Важное
- **Отдельный порт** для Actuator (`management.server.port`) — рекомендуемая практика. Позволяет ограничить доступ на уровне сети.
- **health/liveness** не должен зависеть от внешних систем (БД, Redis). Если БД недоступна, приложение не нужно перезапускать — нужно убрать из балансировки (readiness).
- **health/readiness** должен включать проверки критических зависимостей (БД, кэш), без которых сервис не может обрабатывать запросы.
- Endpoint `/actuator/prometheus` должен быть доступен для Prometheus, но **не для внешних пользователей**.

### Частые ошибки
- **Включение всех endpoints в production** (`include: "*"`): опасно — `heapdump` и `env` могут содержать чувствительные данные.
- **Liveness probe зависит от БД**: при недоступности БД Kubernetes перезапустит все поды, хотя проблема не в приложении.
- **Нет отдельного порта для Actuator**: actuator endpoints доступны на том же порту, что и бизнес-API.
- **Забыть настроить show-details**: по умолчанию `/health` возвращает только `{"status":"UP"}` без деталей.
- **Тяжёлые health checks**: healthcheck, делающий полный SELECT из БД каждые 5 секунд, создаёт нагрузку. Используйте лёгкие проверки (ping, `isValid()`).

### Как используется в 2026
- **Spring Boot 3.4+** — Actuator endpoints с поддержкой structured logging (JSON) из коробки.
- **Actuator + Observation API** — все endpoints автоматически инструментируются (метрики + трейсы).
- **Graceful shutdown** — Actuator интегрируется с lifecycle: при shutdown сервис сначала снимается с readiness, затем дожидает текущие запросы.
- **Container-aware health** — Actuator понимает, что работает в контейнере, и автоматически адаптирует health groups для Kubernetes.
- **Info endpoint** — автоматически подтягивает Git commit info, Java version, OS info, Spring Boot version.

> **На собеседовании:** обязательно упомяните разницу между liveness и readiness probes — это очень частый вопрос. Liveness проверяет "живо ли приложение" (не включать внешние зависимости), readiness — "готово ли принимать трафик" (включать БД, Redis). Также покажите знание про отдельный порт для Actuator в production и безопасность endpoints.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое алертинг и как его правильно настроить?
<!-- grade: senior -->

Алертинг — процесс автоматического оповещения команды о проблемах в системе на основании заданных правил. Правильно настроенный алертинг позволяет быстро реагировать на инциденты, неправильный — приводит к alert fatigue (усталости от алертов), когда все уведомления игнорируются.

### Принципы правильного алертинга

1. **Алерты на симптомы, а не на причины**: алерт «высокая latency для пользователей» лучше, чем «высокий CPU», потому что CPU может быть высоким и при нормальной работе.

2. **Каждый алерт должен требовать действия (actionable)**: если при получении алерта дежурный не может ничего сделать — алерт не нужен.

3. **Минимум шума**: лучше пропустить некритичный инцидент, чем завалить команду ложными срабатываниями.

### Уровни severity

| Severity | Описание | Действие | Время реакции |
|----------|----------|---------|--------------|
| Critical (P1) | Сервис недоступен, потеря данных | Немедленная эскалация, звонок | < 5 минут |
| Warning (P2) | Деградация сервиса, SLO под угрозой | Уведомление в рабочее время | < 1 час |
| Info (P3) | Аномалия, требует внимания | Тикет в бэклог | Следующий рабочий день |

<details><summary>Конфигурация Prometheus Alertmanager</summary>

```yaml
# alertmanager.yml
global:
  resolve_timeout: 5m
  slack_api_url: 'https://hooks.slack.com/services/T.../B.../xxxx'

route:
  receiver: 'default-slack'
  group_by: ['alertname', 'job']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h

  routes:
    - match:
        severity: critical
      receiver: 'pagerduty-critical'
      continue: true
    - match:
        severity: critical
      receiver: 'slack-critical'
    - match:
        severity: warning
      receiver: 'slack-warning'

receivers:
  - name: 'default-slack'
    slack_configs:
      - channel: '#alerts'
        title: '{{ .GroupLabels.alertname }}'
        text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'

  - name: 'slack-critical'
    slack_configs:
      - channel: '#alerts-critical'
        title: 'CRITICAL: {{ .GroupLabels.alertname }}'
        color: 'danger'

  - name: 'slack-warning'
    slack_configs:
      - channel: '#alerts-warning'
        title: 'WARNING: {{ .GroupLabels.alertname }}'
        color: 'warning'

  - name: 'pagerduty-critical'
    pagerduty_configs:
      - service_key: '<pagerduty-service-key>'
        severity: critical

inhibit_rules:
  - source_match:
      alertname: 'ServiceDown'
    target_match:
      severity: 'warning'
    equal: ['job']
```

</details>

<details><summary>Примеры alert rules</summary>

```yaml
# alert_rules.yml
groups:
  - name: application
    rules:
      # Сервис не отвечает
      - alert: ServiceDown
        expr: up{job="order-service"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Service {{ $labels.job }} is down"
          description: "{{ $labels.instance }} has been down for more than 1 minute"

      # Высокий процент ошибок (> 5%)
      - alert: HighErrorRate
        expr: |
          sum(rate(http_server_requests_seconds_count{status=~"5..", job="order-service"}[5m]))
          /
          sum(rate(http_server_requests_seconds_count{job="order-service"}[5m]))
          > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate on {{ $labels.job }}"
          description: "Error rate is {{ $value | humanizePercentage }}"

      # Высокая latency (p99 > 2s)
      - alert: HighLatency
        expr: |
          histogram_quantile(0.99,
            sum(rate(http_server_requests_seconds_bucket{job="order-service"}[5m])) by (le)
          ) > 2
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "High p99 latency on {{ $labels.job }}"

      # JVM Heap > 90%
      - alert: HighHeapUsage
        expr: |
          jvm_memory_used_bytes{area="heap"}
          /
          jvm_memory_max_bytes{area="heap"}
          > 0.9
        for: 5m
        labels:
          severity: warning

      # HikariCP — нет свободных соединений
      - alert: NoFreeDbConnections
        expr: hikaricp_connections_idle{job="order-service"} == 0
        for: 2m
        labels:
          severity: critical

      # Error budget burn rate (SLO-based)
      - alert: ErrorBudgetBurnRate
        expr: |
          (
            1 - (
              sum(rate(http_server_requests_seconds_count{status!~"5..", job="order-service"}[1h]))
              /
              sum(rate(http_server_requests_seconds_count{job="order-service"}[1h]))
            )
          ) > (1 - 0.999) * 14.4
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Error budget burning too fast on {{ $labels.job }}"
```

</details>

### Важное
- **Alert fatigue** — главная проблема алертинга. Если команда получает 100 алертов в день, все они будут игнорироваться.
- **Алерт = действие**: каждый алерт должен иметь runbook (документ с инструкцией по решению проблемы).
- **`for` clause** обязателен: без него алерт сработает на короткий spike. `for: 5m` означает «проблема должна длиться 5 минут подряд».
- **group_by** предотвращает спам: если упало 10 инстансов одного сервиса, приходит один алерт, а не десять.

### Частые ошибки
- **Алерт на каждую метрику**: «CPU > 80%» не означает проблему. Алертить нужно на влияние на пользователя (latency, errors).
- **Слишком низкие пороги**: `error_rate > 0.01` при 100 RPS будет срабатывать постоянно из-за единичных 500-ок.
- **Нет `for` clause**: алерт срабатывает на каждый кратковременный spike, создавая шум.
- **Нет inhibit_rules**: при падении всего кластера приходят сотни алертов, хотя нужен один.
- **Не настроен resolve**: команда не знает, когда проблема решена, и продолжает тратить время.

### Как используется в 2026
- **SLO-based alerting** — вместо статических порогов используется burn rate error budget.
- **Grafana Unified Alerting** — единый алертинг для метрик, логов и трейсов.
- **AIOps** — ML-модели автоматически определяют аномалии и корреляции между алертами.
- **Grafana OnCall / PagerDuty** — интеграция с системами управления дежурствами.
- **ChatOps** — алерты в Slack/Teams с возможностью acknowledge, silence и escalate прямо из чата.

> **На собеседовании:** ключевые принципы — алерты на симптомы (не причины), каждый алерт actionable, обязательный `for` clause. Покажите знание уровней severity и их маппинг на действия. Частая ошибка — не упомянуть alert fatigue и inhibit_rules. Сильный ответ включает SLO-based alerting с burn rate.

[к оглавлению](#мониторинг-и-observability)

---

## Что такое SLO, SLI и SLA?
<!-- grade: senior -->

SLA, SLO, SLI — три уровня определения и измерения надёжности сервиса, введённые Google в рамках практик Site Reliability Engineering (SRE).

- **SLA (Service Level Agreement)** — юридическое соглашение с клиентом, определяющее минимальный уровень качества и компенсации при нарушении.
- **SLO (Service Level Objective)** — внутренняя цель команды по надёжности, строже SLA, без юридических последствий.
- **SLI (Service Level Indicator)** — конкретная метрика, показывающая текущий уровень качества.

```
SLI (что измеряем) → SLO (какую цель ставим) → SLA (что обещаем клиенту)

SLI: availability = 99.97%
SLO: availability >= 99.95%   -- SLO выполняется
SLA: availability >= 99.9%    -- SLA выполняется
```

### Error Budget (бюджет ошибок)

Error budget = 100% - SLO. Это «допустимый» объём ошибок или простоя.

| SLO | Error Budget/месяц | Допустимый downtime/месяц |
|-----|-------------------|--------------------------|
| 99% | 1% | 7 часов 18 минут |
| 99.9% | 0.1% | 43 минуты 50 секунд |
| 99.95% | 0.05% | 21 минута 55 секунд |
| 99.99% | 0.01% | 4 минуты 23 секунды |
| 99.999% | 0.001% | 26 секунд |

**Как использовать error budget:**
- Пока error budget > 0: можно деплоить новые фичи, проводить эксперименты
- Error budget исчерпан: замораживаем фичи, фокусируемся на надёжности
- Error budget сгорает слишком быстро: алерт (burn rate alert)

### Типичные SLI

| SLI | Формула | Для чего |
|-----|---------|----------|
| Availability | `success_requests / total_requests` | Доступность сервиса |
| Latency | `requests_within_threshold / total_requests` | Скорость ответа |
| Throughput | `successful_operations / time_period` | Пропускная способность |
| Error Rate | `error_requests / total_requests` | Частота ошибок |
| Freshness | `data_age < threshold` | Актуальность данных |
| Correctness | `correct_responses / total_responses` | Корректность ответов |

<details><summary>Реализация SLI/SLO с Prometheus (recording + alert rules)</summary>

```yaml
# recording_rules.yml — предвычисление SLI
groups:
  - name: sli_rules
    rules:
      # SLI: Availability (доля успешных запросов)
      - record: sli:availability:ratio_rate5m
        expr: |
          sum(rate(http_server_requests_seconds_count{status!~"5..", job="order-service"}[5m]))
          /
          sum(rate(http_server_requests_seconds_count{job="order-service"}[5m]))

      # SLI: Latency (доля запросов быстрее 500ms)
      - record: sli:latency:ratio_rate5m
        expr: |
          sum(rate(http_server_requests_seconds_bucket{le="0.5", job="order-service"}[5m]))
          /
          sum(rate(http_server_requests_seconds_count{job="order-service"}[5m]))

      # Error budget consumed (за последние 30 дней)
      - record: slo:error_budget:consumed_ratio_30d
        expr: |
          1 - (
            (1 - sli:availability:ratio_rate5m)
            /
            (1 - 0.999)
          )
```

```yaml
# alert_rules.yml — алерты на burn rate
groups:
  - name: slo_alerts
    rules:
      # Быстрое сгорание error budget (бюджет закончится за ~2 дня)
      - alert: ErrorBudgetFastBurn
        expr: |
          (
            1 - (
              sum(rate(http_server_requests_seconds_count{status!~"5..", job="order-service"}[1h]))
              /
              sum(rate(http_server_requests_seconds_count{job="order-service"}[1h]))
            )
          )
          /
          (1 - 0.999) > 14.4
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Error budget burning fast — exhaustion in ~2 hours"

      # Медленное сгорание (бюджет закончится за ~5 дней)
      - alert: ErrorBudgetSlowBurn
        expr: |
          (
            1 - (
              sum(rate(http_server_requests_seconds_count{status!~"5..", job="order-service"}[6h]))
              /
              sum(rate(http_server_requests_seconds_count{job="order-service"}[6h]))
            )
          )
          /
          (1 - 0.999) > 6
        for: 30m
        labels:
          severity: warning
        annotations:
          summary: "Error budget burning slowly — exhaustion in ~5 days"
```

</details>

### Grafana Dashboard для SLO

```promql
# Текущая availability (SLI)
sli:availability:ratio_rate5m{job="order-service"}

# Error budget оставшийся (%)
(1 - (1 - sli:availability:ratio_rate5m) / (1 - 0.999)) * 100
```

### Пример SLO-документа для сервиса

```
Сервис: Order Service
Владелец: Team Orders

SLO 1: Availability
  SLI: доля HTTP-запросов с кодом != 5xx
  Target: 99.9% за 30-дневное окно
  Error budget: 0.1% (~43 мин downtime)

SLO 2: Latency
  SLI: доля HTTP-запросов с latency < 500ms
  Target: 99% за 30-дневное окно
  Error budget: 1% запросов могут быть медленнее

Действия при исчерпании error budget:
  1. Заморозка нестабильных деплоев
  2. Приоритет задачам надёжности
  3. Post-mortem для крупных инцидентов
```

### Важное
- **SLO должен быть не 100%**: 100% SLO невозможен и блокирует любые изменения. «Достаточно надёжный» — правильный подход.
- **SLO определяет SLA**: SLO всегда строже SLA. Если SLO нарушен, команда реагирует **до** нарушения SLA.
- **Error budget — инструмент баланса**: между скоростью разработки (новые фичи) и надёжностью.
- **SLI должен отражать пользовательский опыт**: не «CPU < 80%», а «99% запросов отвечают за < 500ms».

### Частые ошибки
- **SLO = SLA**: если SLO равен SLA, нет запаса — любая деградация сразу нарушает соглашение.
- **Слишком высокий SLO**: 99.999% означает 26 секунд downtime в месяц — стоимость поддержания огромна.
- **SLI не привязан к пользовательскому опыту**: мониторить внутренние метрики вместо user-facing.
- **Нет error budget policy**: error budget определён, но нет процесса, что делать при его исчерпании.
- **Один SLO на весь сервис**: разные endpoint'ы имеют разную важность. `/api/payments` и `/api/health` — разные SLO.

### Как используется в 2026
- **SLO-based alerting** — стандартная практика в Google, рекомендуется в SRE Book. Prometheus + burn rate alerts.
- **Grafana SLO feature** — встроенная поддержка SLO в Grafana Enterprise и Cloud.
- **OpenSLO** — open-source стандарт для описания SLO в формате YAML.
- **Sloth** — инструмент для генерации Prometheus recording/alerting rules из OpenSLO-описания.
- **Platform Engineering** — SLO как контракт между Platform team и Product teams.

> **На собеседовании:** чётко разграничьте SLA (юридическое), SLO (внутренняя цель), SLI (метрика). Обязательно объясните Error Budget и его роль как баланса между фичами и надёжностью. Сильный ответ включает конкретный пример: "SLI availability = 99.97%, SLO >= 99.95%, SLA >= 99.9%". Частая ошибка — не упомянуть, что SLO не должен быть 100%.

[к оглавлению](#мониторинг-и-observability)

---

## Как организовать centralized logging в микросервисах?
<!-- grade: middle -->

Centralized logging — подход, при котором логи всех сервисов собираются в единое хранилище для поиска, анализа и корреляции. В микросервисной архитектуре без централизованного логирования невозможно расследовать проблемы, затрагивающие несколько сервисов.

### Основные стеки

| Аспект | ELK (Elastic Stack) | Grafana Loki |
|--------|-----|-------------|
| Путь данных | Приложение -> Logstash/Filebeat -> Elasticsearch -> Kibana | Приложение -> Promtail/Alloy -> Loki -> Grafana |
| Индексирование | Полнотекстовый индекс (каждое слово) | Индекс только по лейблам |
| Стоимость хранения | Высокая (индекс занимает много места) | Низкая (сжатые chunks) |
| Поиск | Быстрый по любому полю | Быстрый по лейблам, медленнее по тексту |
| Масштабирование | Сложное (кластер Elasticsearch) | Простое (object storage S3/GCS) |
| Экосистема | Зрелая, множество интеграций | Тесная интеграция с Grafana, Tempo, Prometheus |

### Structured logging (структурированные логи)

Вместо текстовых логов используйте JSON — легче парсить, фильтровать и анализировать:

```java
// НЕ делайте так:
log.info("Order " + orderId + " created for user " + userId + " with amount " + amount);
// "Order 12345 created for user 42 with amount 9999" — парсить сложно

// Делайте так (structured):
log.info("Order created",
    kv("orderId", orderId),
    kv("userId", userId),
    kv("amount", amount));
// {"message":"Order created","orderId":"12345","userId":"42","amount":9999}
```

<details><summary>Настройка structured logging (Spring Boot + Logback)</summary>

```yaml
# application.yml (Spring Boot 3.4+)
logging:
  structured:
    format:
      console: ecs       # Elastic Common Schema
```

```xml
<!-- logback-spring.xml -->
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="net.logstash.logback.encoder.LogstashEncoder">
            <includeMdcKeyName>traceId</includeMdcKeyName>
            <includeMdcKeyName>spanId</includeMdcKeyName>
            <customFields>{"service":"order-service","env":"production"}</customFields>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
    </root>
</configuration>
```

Результат:
```json
{
  "@timestamp": "2026-04-22T10:30:00.123Z",
  "@version": "1",
  "message": "Order created",
  "logger_name": "com.example.OrderService",
  "thread_name": "http-nio-8080-exec-1",
  "level": "INFO",
  "traceId": "abc123def456",
  "spanId": "789xyz",
  "service": "order-service",
  "env": "production",
  "orderId": "12345",
  "userId": "42"
}
```

</details>

### Correlation ID / Trace ID

Для связывания логов из разных сервисов используется единый идентификатор:

```java
// Micrometer Tracing автоматически добавляет traceId в MDC
// В Loki (LogQL):
// {service="order-service"} | json | traceId="abc123def456"
```

<details><summary>Кастомный Correlation ID фильтр (если нет трейсинга)</summary>

```java
@Component
public class CorrelationIdFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                     HttpServletResponse response,
                                     FilterChain chain) throws ServletException, IOException {
        String correlationId = request.getHeader("X-Correlation-ID");
        if (correlationId == null) {
            correlationId = UUID.randomUUID().toString();
        }
        MDC.put("correlationId", correlationId);
        response.setHeader("X-Correlation-ID", correlationId);
        try {
            chain.doFilter(request, response);
        } finally {
            MDC.remove("correlationId");
        }
    }
}
```

</details>

### Сбор логов в Kubernetes

В Kubernetes стандартный подход — логирование в stdout, а сборщик (DaemonSet) читает логи контейнеров:

```
┌──────────────────────────────────────────────┐
│              Kubernetes Node                  │
│                                               │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐ │
│  │ Pod A     │  │ Pod B     │  │ Pod C     │ │
│  │ stdout→   │  │ stdout→   │  │ stdout→   │ │
│  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘ │
│        │              │              │        │
│        ▼              ▼              ▼        │
│  ┌─────────────────────────────────────────┐  │
│  │  /var/log/containers/*.log               │  │
│  └──────────────┬──────────────────────────┘  │
│                 │                              │
│  ┌──────────────▼──────────────┐              │
│  │  Promtail / Fluentd / Alloy │ (DaemonSet) │
│  └──────────────┬──────────────┘              │
└─────────────────┼────────────────────────────┘
                  │
                  ▼
          ┌───────────────┐
          │  Loki / ELK   │
          └───────────────┘
```

### LogQL — язык запросов Loki

```logql
# Все логи сервиса
{service="order-service"}

# Логи с ошибками
{service="order-service"} |= "ERROR"
{service="order-service"} | json | level="ERROR"

# Конкретный traceId (поиск по всем сервисам)
{service=~".+"} | json | traceId="abc123def456"

# Количество ошибок в секунду
sum(rate({service="order-service"} | json | level="ERROR" [5m]))
```

### Управление уровнями логирования через Actuator

```bash
# Временно включить DEBUG для конкретного пакета (без перезапуска)
curl -X POST http://localhost:9090/actuator/loggers/com.example.service.OrderService \
  -H 'Content-Type: application/json' \
  -d '{"configuredLevel": "DEBUG"}'

# Вернуть обратно
curl -X POST http://localhost:9090/actuator/loggers/com.example.service.OrderService \
  -H 'Content-Type: application/json' \
  -d '{"configuredLevel": null}'
```

### Важное
- **Логи в stdout** — стандарт для Kubernetes. Не пишите в файлы внутри контейнера.
- **Structured JSON logging** — обязательно в микросервисах. Текстовые логи невозможно эффективно парсить.
- **traceId в каждой записи** — позволяет найти все логи конкретного запроса по всем сервисам.
- **Log levels**: DEBUG — только для разработки, INFO — нормальная работа, WARN — потенциальная проблема, ERROR — ошибка требует внимания.
- **Retention policy**: логи нельзя хранить вечно. Настройте автоматическое удаление (7 дней для DEBUG, 30 для ERROR, 90 для audit).

### Частые ошибки
- **Логирование чувствительных данных**: пароли, токены, номера карт в логах — нарушение PCI DSS и GDPR. Используйте маскирование.
- **Отсутствие structured logging**: `log.info("Something happened with " + obj)` — невозможно фильтровать.
- **Логирование в файл внутри контейнера**: файл теряется при перезапуске пода, если не смонтирован volume.
- **Слишком много DEBUG-логов в production**: создаёт огромный объём данных и увеличивает стоимость хранения.
- **Нет корреляции**: логи есть, но без traceId невозможно связать события из разных сервисов.

### Как используется в 2026
- **Grafana Loki** — основной выбор для новых проектов благодаря низкой стоимости и интеграции с Grafana stack.
- **Grafana Alloy** (замена Promtail + Grafana Agent) — единый агент для сбора метрик, логов и трейсов.
- **OpenTelemetry Logs** — стандартный способ отправки логов через OTel Collector, с автоматической корреляцией с трейсами.
- **Spring Boot 3.4+ Structured Logging** — нативная поддержка JSON/ECS формата логов без дополнительных библиотек.
- **Log-based metrics** — Loki и Elasticsearch позволяют создавать метрики из логов.
- **AI-powered log analysis** — автоматическое обнаружение паттернов ошибок и группировка похожих событий.

> **На собеседовании:** начните с обоснования необходимости centralized logging в микросервисах (невозможно SSH на каждый под). Обязательно упомяните structured logging (JSON) и корреляцию через traceId. Покажите знание хотя бы одного стека (ELK или Loki). Частая ошибка — забыть про retention policy и маскирование чувствительных данных.

[к оглавлению](#мониторинг-и-observability)

[Вопросы для собеседования](README.md)
