[Вопросы для собеседования](README.md)

# Микросервисы
+ [Что такое микросервисная архитектура?](#Что-такое-микросервисная-архитектура)
+ [Чем микросервисная архитектура отличается от монолитной?](#Чем-микросервисная-архитектура-отличается-от-монолитной)
+ [Какие преимущества у микросервисной архитектуры?](#Какие-преимущества-у-микросервисной-архитектуры)
+ [Какие недостатки у микросервисной архитектуры?](#Какие-недостатки-у-микросервисной-архитектуры)
+ [Когда стоит использовать микросервисы, а когда нет?](#Когда-стоит-использовать-микросервисы-а-когда-нет)
+ [Какие принципы декомпозиции на микросервисы существуют?](#Какие-принципы-декомпозиции-на-микросервисы-существуют)
+ [Что такое Bounded Context и как он связан с микросервисами?](#Что-такое-Bounded-Context-и-как-он-связан-с-микросервисами)
+ [Какие способы взаимодействия микросервисов существуют?](#Какие-способы-взаимодействия-микросервисов-существуют)
+ [Как организовать синхронное взаимодействие микросервисов через REST?](#Как-организовать-синхронное-взаимодействие-микросервисов-через-REST)
+ [Что такое gRPC и когда его стоит использовать вместо REST?](#Что-такое-gRPC-и-когда-его-стоит-использовать-вместо-REST)
+ [Как организовать асинхронное взаимодействие через брокеры сообщений?](#Как-организовать-асинхронное-взаимодействие-через-брокеры-сообщений)
+ [Что такое API Gateway и зачем он нужен?](#Что-такое-API-Gateway-и-зачем-он-нужен)
+ [Что такое Service Discovery и как оно работает?](#Что-такое-Service-Discovery-и-как-оно-работает)
+ [Как организовать конфигурацию микросервисов?](#Как-организовать-конфигурацию-микросервисов)
+ [Что такое паттерн Circuit Breaker?](#Что-такое-паттерн-Circuit-Breaker)
+ [Что такое паттерн Saga и как он решает проблему распределённых транзакций?](#Что-такое-паттерн-Saga-и-как-он-решает-проблему-распределённых-транзакций)
+ [Что такое Event Sourcing?](#Что-такое-Event-Sourcing)
+ [Что такое CQRS?](#Что-такое-CQRS)
+ [Что такое паттерн Outbox?](#Что-такое-паттерн-Outbox)
+ [Что такое паттерн Strangler Fig?](#Что-такое-паттерн-Strangler-Fig)
+ [Как организовать распределённую трассировку в микросервисах?](#Как-организовать-распределённую-трассировку-в-микросервисах)
+ [Как организовать централизованное логирование?](#Как-организовать-централизованное-логирование)
+ [Как реализовать health checks и мониторинг микросервисов?](#Как-реализовать-health-checks-и-мониторинг-микросервисов)
+ [Что такое идемпотентность и почему она важна в микросервисах?](#Что-такое-идемпотентность-и-почему-она-важна-в-микросервисах)
+ [Как версионировать API микросервисов?](#Как-версионировать-API-микросервисов)
+ [Как тестировать микросервисы?](#Как-тестировать-микросервисы)
+ [Что такое 12-Factor App?](#Что-такое-12-Factor-App)
+ [Что такое паттерн Sidecar?](#Что-такое-паттерн-Sidecar)
+ [Что такое паттерн BFF (Backend for Frontend)?](#Что-такое-паттерн-BFF-Backend-for-Frontend)
+ [Database per Service или Shared Database — какой подход выбрать?](#Database-per-Service-или-Shared-Database--какой-подход-выбрать)
+ [Что такое паттерны Retry и Timeout?](#Что-такое-паттерны-Retry-и-Timeout)
+ [Что такое паттерн Bulkhead?](#Что-такое-паттерн-Bulkhead)
+ [Как обеспечить безопасность межсервисного взаимодействия?](#Как-обеспечить-безопасность-межсервисного-взаимодействия)
+ [Что такое Service Mesh?](#Что-такое-Service-Mesh)
+ [Как организовать деплой микросервисов?](#Как-организовать-деплой-микросервисов)

## Что такое микросервисная архитектура?
**Микросервисная архитектура** — это подход к разработке программного обеспечения, при котором приложение строится как набор небольших, автономных сервисов, каждый из которых работает в собственном процессе, развёртывается независимо и взаимодействует с другими сервисами через чётко определённые API (обычно HTTP/REST, gRPC или через брокеры сообщений).

Каждый микросервис:
+ **Отвечает за одну бизнес-функцию** — например, сервис платежей, сервис пользователей, сервис уведомлений.
+ **Имеет собственное хранилище данных** — каждый сервис владеет своими данными и не обращается напрямую к базе другого сервиса.
+ **Развёртывается независимо** — можно обновить один сервис, не затрагивая остальные.
+ **Может быть написан на любом стеке технологий** — хотя в рамках одной организации обычно используют ограниченный набор (например, Java/Spring Boot).

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│  Сервис  │    │  Сервис  │    │  Сервис  │
│ платежей │◄──►│  заказов │◄──►│ клиентов │
│          │    │          │    │          │
│  ┌────┐  │    │  ┌────┐  │    │  ┌────┐  │
│  │ DB │  │    │  │ DB │  │    │  │ DB │  │
│  └────┘  │    │  └────┘  │    │  └────┘  │
└──────────┘    └──────────┘    └──────────┘
```

В контексте банковской системы с 5000+ системами микросервисная архитектура позволяет разным командам параллельно разрабатывать, тестировать и развёртывать свои сервисы, что критически важно при таком масштабе.

[к оглавлению](#Микросервисы)

## Чем микросервисная архитектура отличается от монолитной?

| Критерий | Монолит | Микросервисы |
|---|---|---|
| **Структура** | Единое приложение, один деплой | Набор независимых сервисов |
| **База данных** | Одна общая БД | Отдельная БД у каждого сервиса |
| **Развёртывание** | Всё приложение целиком | Каждый сервис независимо |
| **Масштабирование** | Только целиком (vertical/horizontal) | Каждый сервис отдельно |
| **Технологический стек** | Единый для всего приложения | Может различаться |
| **Отказоустойчивость** | Ошибка может положить всё | Ошибка изолирована в одном сервисе |
| **Размер команды** | Вся команда работает с одной кодовой базой | Маленькие команды владеют отдельными сервисами |
| **Сложность** | Простая начальная разработка | Сложная инфраструктура с самого начала |
| **Транзакции** | Локальные ACID-транзакции | Распределённые (Saga, eventual consistency) |
| **Тестирование** | Простое интеграционное | Требует contract testing, E2E |

**Монолит** — это единое приложение, в котором все компоненты (UI, бизнес-логика, доступ к данным) развёртываются как один артефакт:

```java
// Монолит: всё в одном приложении
@SpringBootApplication
public class BankingMonolithApplication {
    // Модуль платежей, модуль клиентов, модуль отчётов —
    // всё в одном JAR/WAR
}
```

**Микросервисы** — это набор отдельных приложений:

```java
// Отдельное приложение — сервис платежей
@SpringBootApplication
public class PaymentServiceApplication { }

// Отдельное приложение — сервис клиентов
@SpringBootApplication
public class CustomerServiceApplication { }
```

Важно понимать, что микросервисы — это не «серебряная пуля». Монолит прекрасно работает для многих задач, и переход на микросервисы оправдан только при наличии реальных проблем масштабирования команды или системы.

[к оглавлению](#Микросервисы)

## Какие преимущества у микросервисной архитектуры?

1. **Независимое развёртывание** — можно обновлять каждый сервис отдельно, без перезапуска всей системы. В банковской среде с 5000+ системами это критически важно: обновление одного сервиса не требует регрессионного тестирования всей платформы.

2. **Масштабирование отдельных компонентов** — если сервис обработки платежей испытывает высокую нагрузку, можно масштабировать только его, не тратя ресурсы на масштабирование сервиса отчётов.

3. **Технологическая гибкость** — каждый сервис может использовать наиболее подходящий стек. Например, сервис аналитики может использовать Python, а основные сервисы — Java/Spring Boot.

4. **Отказоустойчивость** — сбой одного сервиса не приводит к падению всей системы (при правильной реализации паттернов устойчивости, таких как Circuit Breaker).

5. **Параллельная разработка** — разные команды могут работать над разными сервисами независимо, что ускоряет delivery.

6. **Простота понимания** — каждый сервис относительно небольшой и сфокусирован на конкретной бизнес-функции, что упрощает онбординг новых разработчиков.

7. **Гибкость выбора хранилища данных (Polyglot Persistence)** — для каждого сервиса можно выбрать оптимальное хранилище: PostgreSQL для транзакционных данных, MongoDB для документов, Redis для кеширования.

8. **Лучшее соответствие организационной структуре** (Закон Конвея) — структура системы отражает структуру организации.

[к оглавлению](#Микросервисы)

## Какие недостатки у микросервисной архитектуры?

1. **Сложность распределённой системы** — приходится решать проблемы сетевых задержек, отказов сети, согласованности данных, что гораздо сложнее, чем вызов метода в монолите.

2. **Сложность управления данными** — невозможно использовать обычные ACID-транзакции между сервисами. Приходится применять паттерн Saga, eventual consistency, что значительно усложняет логику.

3. **Операционная сложность** — необходимо организовать мониторинг, логирование, трассировку для десятков или сотен сервисов. Нужна зрелая DevOps-культура и CI/CD.

4. **Сетевые задержки** — вызов между сервисами по сети значительно медленнее вызова метода в рамках одного процесса.

5. **Сложность тестирования** — интеграционное и E2E-тестирование становятся гораздо сложнее. Необходимо использовать contract testing (Pact), testcontainers, моки внешних сервисов.

6. **Дублирование кода** — некоторые общие функции (например, валидация, маппинг DTO) приходится дублировать в разных сервисах или выносить в shared-библиотеки, что создаёт связность.

7. **Сложность отладки** — один бизнес-процесс может проходить через 5-10 сервисов, и для отладки нужны инструменты распределённой трассировки.

8. **Overhead на межсервисное взаимодействие** — сериализация/десериализация данных, HTTP/gRPC-вызовы, брокеры сообщений добавляют накладные расходы.

9. **Проблема «распределённого монолита»** — при неправильной декомпозиции можно получить систему с недостатками и монолита, и микросервисов одновременно.

[к оглавлению](#Микросервисы)

## Когда стоит использовать микросервисы, а когда нет?

**Стоит использовать, когда:**
+ Система большая и сложная, разрабатывается несколькими командами (как в банке с 5000+ системами).
+ Требуется независимое масштабирование отдельных компонентов.
+ Нужны разные циклы релизов для разных частей системы.
+ Организация достаточно зрелая: есть CI/CD, мониторинг, опыт работы с распределёнными системами.
+ Разные части системы имеют разные требования к доступности, нагрузке или технологиям.
+ Команда разработки достаточно большая (обычно больше 20-30 человек).

**НЕ стоит использовать, когда:**
+ Проект маленький или находится на ранней стадии — лучше начать с монолита (или «модульного монолита»).
+ Команда маленькая (3-5 разработчиков) — накладные расходы на инфраструктуру съедят все преимущества.
+ Домен недостаточно изучен — невозможно правильно определить границы сервисов.
+ Нет DevOps-культуры и инфраструктуры — без CI/CD, мониторинга, оркестрации контейнеров микросервисы будут кошмаром.
+ Жёсткие требования к транзакционной согласованности — если все данные должны быть строго консистентны, eventual consistency создаст серьёзные проблемы.

> **Совет Мартина Фаулера:** «Не начинайте с микросервисов. Начните с монолита, правильно структурируйте его по модулям, и когда почувствуете боль масштабирования — выделяйте микросервисы.»

Хорошей промежуточной стратегией является **модульный монолит** — единое приложение, но с чёткими границами модулей, которые потом можно выделить в отдельные сервисы:

```java
// Модульный монолит — чёткие границы модулей
// module-payment/src/main/java/com/bank/payment/PaymentModule.java
// module-customer/src/main/java/com/bank/customer/CustomerModule.java
// Модули взаимодействуют только через определённые интерфейсы
```

[к оглавлению](#Микросервисы)

## Какие принципы декомпозиции на микросервисы существуют?

Правильная декомпозиция — ключ к успешной микросервисной архитектуре. Основные принципы:

**1. Single Responsibility Principle (SRP)** — каждый сервис отвечает за одну бизнес-способность (business capability). Например: сервис платежей, сервис уведомлений, сервис аутентификации.

**2. Domain-Driven Design (DDD)** — декомпозиция по ограниченным контекстам (Bounded Contexts). Это самый эффективный подход:
+ Проведите Event Storming с бизнес-экспертами
+ Определите домены и поддомены
+ Выделите Bounded Contexts
+ Каждый Bounded Context — кандидат на микросервис

**3. Высокая связность (High Cohesion)** — всё, что относится к одной бизнес-функции, должно быть в одном сервисе.

**4. Слабая связанность (Loose Coupling)** — изменение одного сервиса не должно требовать изменения других. Сервисы взаимодействуют только через API.

**5. Размер команды (правило «двух пицц» Безоса)** — один сервис должен поддерживаться командой, которую можно накормить двумя пиццами (5-9 человек).

**6. Декомпозиция по бизнес-подобластям (Subdomains):**
+ **Core domain** — конкурентное преимущество (например, скоринговая модель)
+ **Supporting subdomain** — поддерживающие функции (управление клиентами)
+ **Generic subdomain** — типовые функции (уведомления, аутентификация)

**Антипаттерны декомпозиции:**
+ Декомпозиция по техническим слоям (сервис для DAO, сервис для бизнес-логики) — это плохо.
+ Слишком мелкие сервисы (nano-сервисы) — чрезмерный overhead.
+ Распределённый монолит — сервисы жёстко связаны и деплоятся вместе.

[к оглавлению](#Микросервисы)

## Что такое Bounded Context и как он связан с микросервисами?

**Bounded Context** (ограниченный контекст) — это центральная концепция Domain-Driven Design (DDD), обозначающая границу, внутри которой определённая модель предметной области является согласованной и единообразной.

В разных контекстах одно и то же понятие может иметь разное значение. Например, в банковской системе «Клиент»:
+ В контексте **продаж** — это лид с контактными данными и историей взаимодействий.
+ В контексте **кредитования** — это заёмщик с кредитной историей и скоринговым баллом.
+ В контексте **платежей** — это владелец счёта с реквизитами.

Каждый контекст имеет свою модель «Клиента», и это нормально. Попытка создать единую модель для всех контекстов приводит к «Большому комку грязи» (Big Ball of Mud).

**Связь с микросервисами:** один Bounded Context, как правило, соответствует одному микросервису (или группе тесно связанных сервисов).

```java
// Контекст "Платежи" — своя модель клиента
package com.bank.payment.domain;

public class PaymentCustomer {
    private Long id;
    private String accountNumber;
    private BigDecimal balance;
    // Здесь нет данных о кредитной истории — они не нужны
}

// Контекст "Кредитование" — своя модель клиента
package com.bank.credit.domain;

public class CreditCustomer {
    private Long id;
    private CreditScore creditScore;
    private List<CreditHistory> history;
    // Здесь нет данных о балансе счёта
}
```

**Context Map** — диаграмма, показывающая взаимосвязи между контекстами:
+ **Shared Kernel** — общий код между контекстами (минимизировать!).
+ **Customer-Supplier** — один контекст является потребителем данных другого.
+ **Anti-Corruption Layer (ACL)** — слой адаптации между контекстами, предотвращающий загрязнение модели чужими концепциями.
+ **Published Language** — формальный язык обмена данными (например, события в Kafka).

[к оглавлению](#Микросервисы)

## Какие способы взаимодействия микросервисов существуют?

Взаимодействие микросервисов делится на два основных типа:

**1. Синхронное взаимодействие (Request-Response):**
+ **REST (HTTP)** — наиболее распространённый способ. Простой, основан на стандартах HTTP.
+ **gRPC** — бинарный протокол на основе Protocol Buffers. Быстрее REST, поддерживает стриминг.
+ **GraphQL** — гибкие запросы, клиент сам определяет структуру ответа.

**2. Асинхронное взаимодействие (Event-Driven):**
+ **Message Brokers** (Kafka, RabbitMQ) — сервис публикует события, подписчики обрабатывают.
+ **Event Streaming** (Apache Kafka) — поток событий с возможностью повторного чтения.

| Критерий | Синхронное | Асинхронное |
|---|---|---|
| Связность | Временная связность (оба сервиса должны быть доступны) | Слабая связность |
| Latency | Суммируется по всей цепочке | Не блокирует вызывающую сторону |
| Сложность | Простая реализация | Сложнее (idempotency, ordering) |
| Отладка | Проще (request-response) | Сложнее (асинхронные цепочки) |
| Надёжность | Ниже (cascade failures) | Выше (буферизация в брокере) |

**Рекомендации:**
+ Для операций, требующих немедленного ответа (запрос баланса) — синхронное взаимодействие.
+ Для операций, допускающих задержку (отправка уведомления после платежа) — асинхронное.
+ В банковских системах предпочтительно асинхронное взаимодействие, так как оно обеспечивает лучшую отказоустойчивость.

```
Синхронное:          Асинхронное:
A ──REST──► B        A ──event──► Kafka ──event──► B
A ◄─resp.── B                                     (A не ждёт B)
```

[к оглавлению](#Микросервисы)

## Как организовать синхронное взаимодействие микросервисов через REST?

В Spring Boot для синхронного взаимодействия между микросервисами используются HTTP-клиенты. Рассмотрим основные варианты:

**1. RestTemplate (устаревший, но ещё встречается):**

```java
@Service
public class PaymentService {
    private final RestTemplate restTemplate;

    public PaymentService(RestTemplateBuilder builder) {
        this.restTemplate = builder
            .rootUri("http://customer-service")
            .setConnectTimeout(Duration.ofSeconds(3))
            .setReadTimeout(Duration.ofSeconds(5))
            .build();
    }

    public CustomerDto getCustomer(Long customerId) {
        return restTemplate.getForObject(
            "/api/customers/{id}", CustomerDto.class, customerId
        );
    }
}
```

**2. WebClient (реактивный, рекомендуется):**

```java
@Service
public class PaymentService {
    private final WebClient webClient;

    public PaymentService(WebClient.Builder builder) {
        this.webClient = builder
            .baseUrl("http://customer-service")
            .build();
    }

    public Mono<CustomerDto> getCustomer(Long customerId) {
        return webClient.get()
            .uri("/api/customers/{id}", customerId)
            .retrieve()
            .bodyToMono(CustomerDto.class);
    }
}
```

**3. Declarative HTTP Client (Spring 6+ / Spring Boot 3+):**

```java
// Декларативный клиент — аналог Feign, но нативный Spring
@HttpExchange("/api/customers")
public interface CustomerClient {

    @GetExchange("/{id}")
    CustomerDto getCustomer(@PathVariable Long id);

    @PostExchange
    CustomerDto createCustomer(@RequestBody CreateCustomerRequest request);
}

@Configuration
public class ClientConfig {
    @Bean
    public CustomerClient customerClient(WebClient.Builder builder) {
        WebClient webClient = builder.baseUrl("http://customer-service").build();
        HttpServiceProxyFactory factory = HttpServiceProxyFactory
            .builderFor(WebClientAdapter.create(webClient))
            .build();
        return factory.createClient(CustomerClient.class);
    }
}
```

**4. OpenFeign (Spring Cloud):**

```java
@FeignClient(name = "customer-service", url = "${customer-service.url}")
public interface CustomerClient {

    @GetMapping("/api/customers/{id}")
    CustomerDto getCustomer(@PathVariable("id") Long id);
}
```

**Важные моменты:**
+ Всегда устанавливайте таймауты (connect timeout, read timeout).
+ Используйте Circuit Breaker для защиты от каскадных сбоев.
+ Применяйте retry с exponential backoff для transient errors.
+ При использовании Service Discovery имена сервисов резолвятся автоматически.

[к оглавлению](#Микросервисы)

## Что такое gRPC и когда его стоит использовать вместо REST?

**gRPC** (gRPC Remote Procedure Call) — это высокопроизводительный фреймворк для удалённого вызова процедур, разработанный Google. Он использует **Protocol Buffers** (protobuf) для сериализации данных и **HTTP/2** как транспортный протокол.

**Ключевые особенности gRPC:**
+ **Бинарный формат** — protobuf компактнее JSON, что снижает нагрузку на сеть.
+ **HTTP/2** — мультиплексирование, сжатие заголовков, двунаправленный стриминг.
+ **Контракт через .proto файл** — строгая типизация, автоматическая генерация кода.
+ **Четыре типа взаимодействия:** Unary, Server Streaming, Client Streaming, Bidirectional Streaming.

**Пример .proto файла:**

```protobuf
syntax = "proto3";
package com.bank.customer;

service CustomerService {
    rpc GetCustomer (GetCustomerRequest) returns (CustomerResponse);
    rpc ListCustomers (ListCustomersRequest) returns (stream CustomerResponse);
}

message GetCustomerRequest {
    int64 customer_id = 1;
}

message CustomerResponse {
    int64 id = 1;
    string name = 2;
    string account_number = 3;
}
```

**Реализация сервера на Spring Boot (с grpc-spring-boot-starter):**

```java
@GrpcService
public class CustomerGrpcService extends CustomerServiceGrpc.CustomerServiceImplBase {

    private final CustomerRepository customerRepository;

    @Override
    public void getCustomer(GetCustomerRequest request,
                            StreamObserver<CustomerResponse> responseObserver) {
        Customer customer = customerRepository.findById(request.getCustomerId())
            .orElseThrow(() -> new StatusRuntimeException(Status.NOT_FOUND));

        responseObserver.onNext(CustomerResponse.newBuilder()
            .setId(customer.getId())
            .setName(customer.getName())
            .setAccountNumber(customer.getAccountNumber())
            .build());
        responseObserver.onCompleted();
    }
}
```

**Когда использовать gRPC вместо REST:**
+ Высоконагруженное межсервисное взаимодействие внутри кластера.
+ Требуется стриминг данных.
+ Критична производительность и размер пакетов.
+ Строгая типизация контрактов важнее «человекочитаемости».

**Когда оставить REST:**
+ Публичное API для внешних потребителей.
+ Простые CRUD-операции.
+ Нужна простая отладка (JSON читаем, protobuf — нет).
+ Браузерные клиенты (gRPC-Web существует, но сложнее).

[к оглавлению](#Микросервисы)

## Как организовать асинхронное взаимодействие через брокеры сообщений?

Асинхронное взаимодействие позволяет сервисам обмениваться данными без ожидания немедленного ответа. Основные брокеры: **Apache Kafka** и **RabbitMQ**.

**Apache Kafka — распределённый лог событий:**

Kafka идеально подходит для event-driven архитектуры в банковских системах, где важно сохранять историю всех событий.

```java
// Продюсер (сервис платежей)
@Service
@RequiredArgsConstructor
public class PaymentEventProducer {
    private final KafkaTemplate<String, PaymentEvent> kafkaTemplate;

    public void publishPaymentCompleted(Payment payment) {
        PaymentEvent event = new PaymentEvent(
            payment.getId(),
            payment.getAmount(),
            payment.getCustomerId(),
            EventType.PAYMENT_COMPLETED,
            Instant.now()
        );
        kafkaTemplate.send("payment-events",
            payment.getId().toString(), event);
    }
}

// Конфигурация продюсера
@Configuration
public class KafkaProducerConfig {
    @Bean
    public ProducerFactory<String, PaymentEvent> producerFactory() {
        Map<String, Object> config = Map.of(
            ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "kafka:9092",
            ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class,
            ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class,
            ProducerConfig.ACKS_CONFIG, "all",  // Важно для банковских систем!
            ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true
        );
        return new DefaultKafkaProducerFactory<>(config);
    }
}

// Консьюмер (сервис уведомлений)
@Service
@Slf4j
public class NotificationEventConsumer {

    @KafkaListener(
        topics = "payment-events",
        groupId = "notification-service",
        containerFactory = "kafkaListenerContainerFactory"
    )
    public void handlePaymentEvent(PaymentEvent event) {
        log.info("Получено событие платежа: {}", event.getPaymentId());
        // Отправить уведомление клиенту
        notificationService.sendPaymentNotification(event);
    }
}
```

**RabbitMQ — классический брокер сообщений:**

RabbitMQ лучше подходит для task queues, routing, когда нужна гибкая маршрутизация сообщений.

```java
// Продюсер
@Service
@RequiredArgsConstructor
public class NotificationProducer {
    private final RabbitTemplate rabbitTemplate;

    public void sendNotification(NotificationMessage message) {
        rabbitTemplate.convertAndSend(
            "notifications-exchange",
            "notification.email",
            message
        );
    }
}

// Консьюмер
@Service
public class EmailNotificationConsumer {

    @RabbitListener(queues = "email-notifications-queue")
    public void handleEmailNotification(NotificationMessage message) {
        emailService.send(message);
    }
}

// Конфигурация
@Configuration
public class RabbitConfig {
    @Bean
    public TopicExchange exchange() {
        return new TopicExchange("notifications-exchange");
    }

    @Bean
    public Queue emailQueue() {
        return QueueBuilder.durable("email-notifications-queue")
            .withArgument("x-dead-letter-exchange", "dlx-exchange")
            .build();
    }

    @Bean
    public Binding binding(Queue emailQueue, TopicExchange exchange) {
        return BindingBuilder.bind(emailQueue)
            .to(exchange).with("notification.email");
    }
}
```

**Kafka vs RabbitMQ:**

| Критерий | Kafka | RabbitMQ |
|---|---|---|
| Модель | Распределённый лог | Очередь сообщений |
| Хранение | Сообщения хранятся после прочтения | Удаляются после подтверждения |
| Пропускная способность | Очень высокая (миллионы msg/sec) | Высокая (десятки тысяч msg/sec) |
| Порядок | Гарантирован в рамках партиции | Гарантирован в рамках очереди |
| Маршрутизация | По топикам и партициям | Гибкая (direct, topic, fanout, headers) |
| Применение | Event streaming, event sourcing | Task queues, RPC, маршрутизация |

[к оглавлению](#Микросервисы)

## Что такое API Gateway и зачем он нужен?

**API Gateway** — это единая точка входа для всех клиентских запросов к микросервисам. Он выступает обратным прокси, который маршрутизирует запросы к соответствующим сервисам и выполняет ряд сквозных функций.

**Функции API Gateway:**
+ **Маршрутизация запросов** — направление запросов к нужному сервису по URL-пути.
+ **Аутентификация и авторизация** — проверка токенов (JWT, OAuth2) на входе.
+ **Rate limiting** — ограничение количества запросов.
+ **Load balancing** — балансировка нагрузки между экземплярами сервиса.
+ **Агрегация запросов** — объединение ответов от нескольких сервисов.
+ **Кеширование** — кеширование частых запросов.
+ **Трансформация запросов/ответов** — изменение формата данных.
+ **SSL termination** — обработка HTTPS.
+ **Логирование и мониторинг** — централизованный сбор метрик.

**Spring Cloud Gateway:**

```java
@SpringBootApplication
public class ApiGatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```

```yaml
# application.yml
spring:
  cloud:
    gateway:
      routes:
        - id: payment-service
          uri: lb://payment-service
          predicates:
            - Path=/api/payments/**
          filters:
            - StripPrefix=1
            - name: CircuitBreaker
              args:
                name: paymentCircuitBreaker
                fallbackUri: forward:/fallback/payments
            - name: RequestRateLimiter
              args:
                redis-rate-limiter.replenishRate: 100
                redis-rate-limiter.burstCapacity: 200

        - id: customer-service
          uri: lb://customer-service
          predicates:
            - Path=/api/customers/**
          filters:
            - StripPrefix=1
            - AddRequestHeader=X-Request-Source, api-gateway
```

**Программная конфигурация маршрутов:**

```java
@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
            .route("payment-service", r -> r
                .path("/api/payments/**")
                .filters(f -> f
                    .stripPrefix(1)
                    .addRequestHeader("X-Gateway", "true")
                    .retry(config -> config
                        .setRetries(3)
                        .setStatuses(HttpStatus.SERVICE_UNAVAILABLE)))
                .uri("lb://payment-service"))
            .build();
    }
}
```

**Альтернативные решения:**
+ **Kong** — высокопроизводительный API Gateway на базе Nginx, с плагинами.
+ **NGINX** — может использоваться как простой API Gateway.
+ **AWS API Gateway** — управляемый сервис в облаке AWS.
+ **Envoy** — часто используется в связке с Service Mesh (Istio).

В банковской среде API Gateway особенно важен для централизованного управления безопасностью и аудита всех запросов.

[к оглавлению](#Микросервисы)

## Что такое Service Discovery и как оно работает?

**Service Discovery** — это механизм, позволяющий микросервисам автоматически находить друг друга в сети без жёсткого указания IP-адресов и портов. Это критически важно, поскольку в динамической среде (контейнеры, автомасштабирование) адреса сервисов постоянно меняются.

**Два подхода к Service Discovery:**

**1. Client-Side Discovery** — клиент сам запрашивает реестр сервисов и выбирает экземпляр:
```
Клиент ──► Service Registry ──► [Список экземпляров]
Клиент ──► Выбранный экземпляр (load balancing на стороне клиента)
```

**2. Server-Side Discovery** — запрос идёт через балансировщик:
```
Клиент ──► Load Balancer ──► Service Registry
                           ──► Выбранный экземпляр
```

**Netflix Eureka (Client-Side Discovery):**

```java
// Eureka Server
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

```yaml
# Eureka Server — application.yml
server:
  port: 8761
eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

```java
// Микросервис-клиент (payment-service)
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(PaymentServiceApplication.class, args);
    }
}
```

```yaml
# Клиент — application.yml
spring:
  application:
    name: payment-service
eureka:
  client:
    service-url:
      defaultZone: http://eureka-server:8761/eureka/
  instance:
    prefer-ip-address: true
```

**Consul** — Service Discovery + Key-Value Store + Health Checks. Более зрелое решение для production:

```yaml
spring:
  cloud:
    consul:
      host: consul-server
      port: 8500
      discovery:
        service-name: payment-service
        health-check-interval: 10s
```

**Kubernetes DNS** — если микросервисы работают в Kubernetes, Service Discovery встроено:

```yaml
# Kubernetes Service
apiVersion: v1
kind: Service
metadata:
  name: payment-service
spec:
  selector:
    app: payment-service
  ports:
    - port: 8080
# Другие сервисы обращаются по имени: http://payment-service:8080
```

В Kubernetes-среде часто отказываются от Eureka/Consul в пользу встроенного DNS, что упрощает архитектуру.

[к оглавлению](#Микросервисы)

## Как организовать конфигурацию микросервисов?

В микросервисной архитектуре необходимо централизованное управление конфигурацией, чтобы не хранить настройки в каждом сервисе отдельно и иметь возможность менять их без переразвёртывания.

**1. Spring Cloud Config Server:**

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

```yaml
# Config Server — application.yml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://git.bank.ru/config-repo
          default-label: main
          search-paths: '{application}'
```

```yaml
# Клиент (payment-service) — application.yml
spring:
  config:
    import: configserver:http://config-server:8888
  application:
    name: payment-service
```

Конфигурационные файлы хранятся в Git-репозитории:
```
config-repo/
├── application.yml           # Общие настройки
├── payment-service.yml       # Настройки payment-service
├── payment-service-prod.yml  # Настройки для production
└── customer-service.yml      # Настройки customer-service
```

**2. Consul KV Store:**

```yaml
spring:
  cloud:
    consul:
      config:
        enabled: true
        format: YAML
        prefix: config
        default-context: application
```

**3. HashiCorp Vault — для секретов:**

Vault предназначен для безопасного хранения секретов (пароли БД, API-ключи, сертификаты). В банковской среде это must-have.

```yaml
spring:
  cloud:
    vault:
      uri: https://vault.bank.ru:8200
      authentication: APPROLE
      app-role:
        role-id: ${VAULT_ROLE_ID}
        secret-id: ${VAULT_SECRET_ID}
      kv:
        enabled: true
        backend: secret
        default-context: payment-service
```

```java
// Секреты автоматически инжектируются в Environment
@Value("${database.password}")
private String dbPassword; // Получен из Vault
```

**4. Kubernetes ConfigMaps и Secrets:**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: payment-service-config
data:
  application.yml: |
    payment:
      max-amount: 1000000
      currency: RUB
```

**Динамическое обновление конфигурации (без перезапуска):**

```java
@RefreshScope
@RestController
public class PaymentController {

    @Value("${payment.max-amount}")
    private BigDecimal maxAmount; // Обновится при POST /actuator/refresh
}
```

Для массового обновления используется Spring Cloud Bus с Kafka/RabbitMQ — один POST-запрос обновляет конфигурацию всех экземпляров сервиса.

[к оглавлению](#Микросервисы)

## Что такое паттерн Circuit Breaker?

**Circuit Breaker** (автоматический выключатель) — это паттерн отказоустойчивости, который предотвращает каскадные сбои в распределённой системе. Он работает аналогично электрическому автомату: при обнаружении проблем с вызываемым сервисом «размыкает цепь», чтобы не допустить дальнейшие бесполезные вызовы.

**Три состояния Circuit Breaker:**

```
   ┌─────────┐   Порог ошибок превышен   ┌──────────┐
   │ CLOSED  │ ──────────────────────────► │   OPEN   │
   │(нормаль)│                             │(отказ)   │
   └────▲────┘                             └────┬─────┘
        │                                       │
        │ Пробный вызов успешен    Таймаут      │
        │                         истёк         │
   ┌────┴────────┐◄─────────────────────────────┘
   │ HALF-OPEN   │
   │(проверка)   │ ── Пробный вызов неуспешен ──► OPEN
   └─────────────┘
```

+ **CLOSED** — запросы проходят нормально. Счётчик ошибок ведётся.
+ **OPEN** — все запросы мгновенно возвращают fallback-ответ, не дожидаясь таймаута.
+ **HALF-OPEN** — пропускается ограниченное число пробных запросов. Если они успешны — CLOSED, иначе — OPEN.

**Реализация с Resilience4j (замена Hystrix):**

```java
// Зависимость: resilience4j-spring-boot3

@Service
public class PaymentService {

    private final CustomerClient customerClient;

    @CircuitBreaker(name = "customerService", fallbackMethod = "getCustomerFallback")
    public CustomerDto getCustomer(Long customerId) {
        return customerClient.getCustomer(customerId);
    }

    // Fallback-метод — вызывается при открытом circuit breaker
    private CustomerDto getCustomerFallback(Long customerId, Throwable t) {
        log.warn("Circuit breaker активирован для customer-service: {}", t.getMessage());
        // Можно вернуть кешированные данные или значение по умолчанию
        return CustomerDto.builder()
            .id(customerId)
            .name("Неизвестный клиент (сервис недоступен)")
            .build();
    }
}
```

```yaml
# application.yml — конфигурация Resilience4j
resilience4j:
  circuitbreaker:
    instances:
      customerService:
        sliding-window-type: COUNT_BASED
        sliding-window-size: 10
        failure-rate-threshold: 50       # Открыть при 50% ошибок
        wait-duration-in-open-state: 30s # Ждать 30 сек перед HALF-OPEN
        permitted-number-of-calls-in-half-open-state: 3
        record-exceptions:
          - java.io.IOException
          - java.net.SocketTimeoutException
          - org.springframework.web.client.HttpServerErrorException
```

Circuit Breaker критически важен в банковской среде: если сервис скоринга недоступен, не нужно бесконечно ждать ответа — лучше сразу вернуть клиенту сообщение «попробуйте позже» или использовать кешированный результат.

[к оглавлению](#Микросервисы)

## Что такое паттерн Saga и как он решает проблему распределённых транзакций?

**Saga** — это паттерн управления распределёнными транзакциями, при котором длинная бизнес-транзакция разбивается на последовательность локальных транзакций в разных сервисах. Каждая локальная транзакция обновляет данные в своём сервисе и публикует событие, запускающее следующий шаг. При сбое выполняются **компенсирующие транзакции** для отмены уже выполненных шагов.

**Проблема:** в микросервисах нельзя использовать распределённые ACID-транзакции (2PC — Two-Phase Commit), так как они плохо масштабируются и создают сильную связность.

**Пример: оформление кредита в банке:**
1. Сервис заявок → создать заявку
2. Сервис скоринга → проверить кредитоспособность
3. Сервис счетов → открыть кредитный счёт
4. Сервис платежей → перевести деньги
5. Сервис уведомлений → уведомить клиента

Если шаг 3 не удался — нужно компенсировать шаги 2 и 1.

**Два подхода к реализации Saga:**

**1. Хореография (Choreography)** — каждый сервис сам знает, какое событие опубликовать, и реагирует на события других сервисов. Нет центрального координатора.

```java
// Сервис заявок — публикует событие
@Service
public class ApplicationService {
    @Transactional
    public void createApplication(CreditApplication app) {
        applicationRepository.save(app);
        eventPublisher.publish(new ApplicationCreatedEvent(app.getId(), app.getCustomerId()));
    }

    // Компенсация
    @KafkaListener(topics = "scoring-failed-events")
    public void handleScoringFailed(ScoringFailedEvent event) {
        applicationRepository.updateStatus(event.getApplicationId(), Status.REJECTED);
    }
}

// Сервис скоринга — слушает событие и обрабатывает
@Service
public class ScoringService {
    @KafkaListener(topics = "application-created-events")
    public void handleApplicationCreated(ApplicationCreatedEvent event) {
        ScoringResult result = performScoring(event.getCustomerId());
        if (result.isApproved()) {
            eventPublisher.publish(new ScoringApprovedEvent(event.getApplicationId()));
        } else {
            eventPublisher.publish(new ScoringFailedEvent(event.getApplicationId()));
        }
    }
}
```

**2. Оркестрация (Orchestration)** — центральный оркестратор (Saga Coordinator) управляет последовательностью шагов и компенсациями.

```java
// Saga-оркестратор
@Service
@RequiredArgsConstructor
public class CreditSagaOrchestrator {
    private final ApplicationClient applicationClient;
    private final ScoringClient scoringClient;
    private final AccountClient accountClient;

    public void executeCreditSaga(CreditRequest request) {
        SagaState state = new SagaState(request);
        try {
            // Шаг 1: Создать заявку
            state.setApplicationId(applicationClient.create(request));

            // Шаг 2: Скоринг
            ScoringResult scoring = scoringClient.evaluate(request.getCustomerId());
            if (!scoring.isApproved()) {
                throw new ScoringRejectedException(scoring.getReason());
            }

            // Шаг 3: Открыть счёт
            state.setAccountId(accountClient.openCreditAccount(request));

            // Шаг 4: Перевести средства
            paymentClient.transfer(state.getAccountId(), request.getAmount());

        } catch (Exception e) {
            compensate(state); // Откатить выполненные шаги
            throw new SagaFailedException("Кредитная saga провалена", e);
        }
    }

    private void compensate(SagaState state) {
        if (state.getAccountId() != null) {
            accountClient.closeAccount(state.getAccountId());
        }
        if (state.getApplicationId() != null) {
            applicationClient.reject(state.getApplicationId());
        }
    }
}
```

**Хореография vs Оркестрация:**

| Критерий | Хореография | Оркестрация |
|---|---|---|
| Связность | Сервисы слабо связаны | Оркестратор знает обо всех шагах |
| Сложность | Растёт с числом шагов | Линейная, проще отслеживать |
| Единая точка отказа | Нет | Оркестратор |
| Видимость процесса | Сложно отследить | Легко |
| Рекомендации | 3-4 шага | 5+ шагов |

[к оглавлению](#Микросервисы)

## Что такое Event Sourcing?

**Event Sourcing** — это паттерн хранения данных, при котором состояние объекта (агрегата) определяется не текущим снимком (snapshot), а последовательностью доменных событий, которые произошли с этим объектом.

Вместо хранения «баланс счёта = 1000 руб.» хранится:
```
1. AccountOpened(accountId=123, initialBalance=0)
2. MoneyDeposited(accountId=123, amount=5000)
3. MoneyWithdrawn(accountId=123, amount=2000)
4. MoneyDeposited(accountId=123, amount=500)
5. MoneyWithdrawn(accountId=123, amount=2500)
→ Текущий баланс = 0 + 5000 - 2000 + 500 - 2500 = 1000
```

**Преимущества:**
+ **Полный аудит** — каждое изменение зафиксировано. Критически важно для банков.
+ **Возможность воспроизвести состояние на любой момент времени** — «какой был баланс вчера в 15:00?»
+ **Отладка** — можно воспроизвести проблему, повторив события.
+ **Event-driven архитектура** — события естественно интегрируются с другими сервисами.
+ **Нет конфликтов при записи** — запись только в append-only лог.

**Недостатки:**
+ **Сложность** — непривычная модель, кривая обучения.
+ **Восстановление состояния** — при большом количестве событий нужны снапшоты.
+ **Eventual consistency** — проекции могут отставать.
+ **Миграция событий** — изменение формата события требует стратегии миграции.

**Пример реализации:**

```java
// Доменное событие
public sealed interface AccountEvent {
    UUID accountId();
    Instant occurredAt();

    record AccountOpened(UUID accountId, BigDecimal initialBalance,
                         Instant occurredAt) implements AccountEvent {}
    record MoneyDeposited(UUID accountId, BigDecimal amount,
                          Instant occurredAt) implements AccountEvent {}
    record MoneyWithdrawn(UUID accountId, BigDecimal amount,
                          Instant occurredAt) implements AccountEvent {}
}

// Агрегат, восстанавливаемый из событий
public class Account {
    private UUID id;
    private BigDecimal balance;
    private List<AccountEvent> uncommittedEvents = new ArrayList<>();

    // Восстановление состояния из событий
    public static Account reconstitute(List<AccountEvent> events) {
        Account account = new Account();
        events.forEach(account::apply);
        return account;
    }

    public void deposit(BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Сумма должна быть положительной");
        }
        var event = new MoneyDeposited(id, amount, Instant.now());
        apply(event);
        uncommittedEvents.add(event);
    }

    public void withdraw(BigDecimal amount) {
        if (balance.compareTo(amount) < 0) {
            throw new InsufficientFundsException("Недостаточно средств");
        }
        var event = new MoneyWithdrawn(id, amount, Instant.now());
        apply(event);
        uncommittedEvents.add(event);
    }

    private void apply(AccountEvent event) {
        switch (event) {
            case AccountOpened e -> {
                this.id = e.accountId();
                this.balance = e.initialBalance();
            }
            case MoneyDeposited e -> this.balance = this.balance.add(e.amount());
            case MoneyWithdrawn e -> this.balance = this.balance.subtract(e.amount());
        }
    }
}

// Хранилище событий
@Repository
public class EventStore {
    private final JdbcTemplate jdbc;

    public void save(UUID aggregateId, List<AccountEvent> events, long expectedVersion) {
        for (AccountEvent event : events) {
            jdbc.update("""
                INSERT INTO events (aggregate_id, event_type, event_data, version, occurred_at)
                VALUES (?, ?, ?::jsonb, ?, ?)
                """,
                aggregateId, event.getClass().getSimpleName(),
                objectMapper.writeValueAsString(event),
                ++expectedVersion, event.occurredAt()
            );
        }
    }
}
```

Для оптимизации производительности при большом количестве событий используются **снапшоты** — периодическое сохранение текущего состояния, чтобы не проигрывать все события с начала.

[к оглавлению](#Микросервисы)

## Что такое CQRS?

**CQRS (Command Query Responsibility Segregation)** — это паттерн, разделяющий модели чтения и записи данных. Команды (Commands) изменяют состояние системы, а запросы (Queries) читают данные. Для каждой из этих задач используется своя оптимизированная модель.

```
                    ┌─────────────────┐
                    │    API Gateway   │
                    └───────┬─────────┘
                     ┌──────┴──────┐
                     │             │
              ┌──────▼──┐   ┌─────▼─────┐
              │ Command  │   │   Query   │
              │  Model   │   │   Model   │
              └──────┬───┘   └─────▲─────┘
                     │             │
              ┌──────▼───┐   ┌────┴──────┐
              │ Write DB  │──►│ Read DB   │
              │(PostgreSQL)│  │(Elasticsearch,│
              └───────────┘  │ Redis, etc.)  │
                             └──────────────┘
```

**Зачем нужен CQRS:**
+ **Разная оптимизация** — модель записи нормализована (3NF), модель чтения денормализована для быстрых запросов.
+ **Масштабирование** — чтений обычно гораздо больше, чем записей (соотношение 100:1). Можно масштабировать read-модель отдельно.
+ **Разные хранилища** — запись в PostgreSQL, чтение из Elasticsearch или Redis.
+ **Естественно сочетается с Event Sourcing** — события из write-модели проецируются в read-модель.

**Пример реализации:**

```java
// Command — изменение данных
public record TransferMoneyCommand(
    UUID fromAccountId,
    UUID toAccountId,
    BigDecimal amount
) {}

@Service
@RequiredArgsConstructor
public class AccountCommandService {
    private final AccountRepository accountRepository;
    private final EventPublisher eventPublisher;

    @Transactional
    public void handle(TransferMoneyCommand cmd) {
        Account from = accountRepository.findById(cmd.fromAccountId());
        Account to = accountRepository.findById(cmd.toAccountId());

        from.withdraw(cmd.amount());
        to.deposit(cmd.amount());

        accountRepository.save(from);
        accountRepository.save(to);

        eventPublisher.publish(new MoneyTransferredEvent(
            cmd.fromAccountId(), cmd.toAccountId(), cmd.amount()));
    }
}

// Query — чтение данных из оптимизированной read-модели
public record AccountBalanceQuery(UUID accountId) {}

@Service
@RequiredArgsConstructor
public class AccountQueryService {
    private final AccountReadRepository readRepository; // Может быть Redis, ES

    public AccountBalanceView handle(AccountBalanceQuery query) {
        return readRepository.findById(query.accountId())
            .orElseThrow(() -> new AccountNotFoundException(query.accountId()));
    }
}

// Read-модель (денормализованный view)
@Document(indexName = "account-balances") // Elasticsearch
public class AccountBalanceView {
    private UUID accountId;
    private String customerName;
    private BigDecimal balance;
    private String currency;
    private LocalDateTime lastTransactionDate;
    private int totalTransactions;
}

// Проекция — обновление read-модели при получении событий
@Service
public class AccountBalanceProjection {

    @KafkaListener(topics = "account-events")
    public void project(AccountEvent event) {
        switch (event) {
            case MoneyDeposited e -> readRepository.updateBalance(
                e.accountId(), e.amount());
            case MoneyWithdrawn e -> readRepository.updateBalance(
                e.accountId(), e.amount().negate());
            // ...
        }
    }
}
```

**Важно:** CQRS добавляет сложность. Используйте его только когда есть реальная необходимость:
+ Существенная разница в нагрузке чтение/запись
+ Разные требования к моделям чтения и записи
+ Необходимость поддержки сложных запросов (полнотекстовый поиск, агрегации)

[к оглавлению](#Микросервисы)

## Что такое паттерн Outbox?

**Outbox** (Transactional Outbox) — это паттерн, решающий проблему **атомарности** при необходимости одновременно обновить базу данных и опубликовать событие в брокер сообщений.

**Проблема:** нельзя атомарно выполнить запись в БД и отправку в Kafka. Возможны ситуации:
+ Запись в БД прошла, отправка в Kafka — нет (потеря события).
+ Отправка в Kafka прошла, запись в БД — нет (фантомное событие).

**Решение:** событие записывается в специальную таблицу `outbox` в той же транзакции, что и бизнес-данные. Отдельный процесс читает эту таблицу и публикует события в брокер.

```
┌─── Одна транзакция ───────────────┐
│  1. UPDATE accounts SET balance... │
│  2. INSERT INTO outbox (event...) │
└───────────────────────────────────┘
         │
         ▼
┌─── Отдельный процесс ────────────┐
│  Читает outbox → Публикует в     │
│  Kafka → Помечает как отправлено │
└───────────────────────────────────┘
```

**Реализация:**

```java
// Таблица outbox
// CREATE TABLE outbox (
//   id UUID PRIMARY KEY,
//   aggregate_type VARCHAR(255),
//   aggregate_id VARCHAR(255),
//   event_type VARCHAR(255),
//   payload JSONB,
//   created_at TIMESTAMP DEFAULT now(),
//   sent BOOLEAN DEFAULT false
// );

@Entity
@Table(name = "outbox")
public class OutboxEvent {
    @Id
    private UUID id;
    private String aggregateType;
    private String aggregateId;
    private String eventType;
    @Column(columnDefinition = "jsonb")
    private String payload;
    private Instant createdAt;
    private boolean sent;
}

// Сервис, который атомарно сохраняет данные и событие
@Service
@RequiredArgsConstructor
public class PaymentService {
    private final PaymentRepository paymentRepository;
    private final OutboxRepository outboxRepository;

    @Transactional // Одна транзакция!
    public void processPayment(PaymentRequest request) {
        // 1. Бизнес-логика
        Payment payment = new Payment(request);
        payment.setStatus(PaymentStatus.COMPLETED);
        paymentRepository.save(payment);

        // 2. Сохраняем событие в outbox
        OutboxEvent event = new OutboxEvent();
        event.setId(UUID.randomUUID());
        event.setAggregateType("Payment");
        event.setAggregateId(payment.getId().toString());
        event.setEventType("PaymentCompleted");
        event.setPayload(objectMapper.writeValueAsString(
            new PaymentCompletedEvent(payment.getId(), payment.getAmount())));
        event.setCreatedAt(Instant.now());
        outboxRepository.save(event);
    }
}

// Polling Publisher — читает outbox и отправляет в Kafka
@Scheduled(fixedDelay = 1000)
@Transactional
public void publishOutboxEvents() {
    List<OutboxEvent> events = outboxRepository.findBySentFalseOrderByCreatedAt();
    for (OutboxEvent event : events) {
        kafkaTemplate.send("payment-events", event.getAggregateId(), event.getPayload());
        event.setSent(true);
        outboxRepository.save(event);
    }
}
```

**Альтернативный подход — Debezium CDC (Change Data Capture):**

Вместо polling используется Debezium, который отслеживает WAL (Write-Ahead Log) PostgreSQL и автоматически публикует записи из таблицы outbox в Kafka. Это более надёжно и эффективно:

```json
{
  "name": "outbox-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "database.hostname": "postgres",
    "database.dbname": "payments",
    "table.include.list": "public.outbox",
    "transforms": "outbox",
    "transforms.outbox.type": "io.debezium.transforms.outbox.EventRouter"
  }
}
```

Паттерн Outbox является стандартом де-факто для надёжной доставки событий в банковских системах, где потеря события (например, о проведённом платеже) недопустима.

[к оглавлению](#Микросервисы)

## Что такое паттерн Strangler Fig?

**Strangler Fig** (душитель) — это паттерн постепенной миграции от монолита к микросервисам. Название происходит от тропического дерева-паразита (фикус-душитель), которое обвивает другое дерево и постепенно замещает его.

**Идея:** вместо рискованного «Big Bang» переписывания монолита, новая функциональность реализуется в микросервисах, а существующая функциональность постепенно перемещается из монолита. Монолит «усыхает» по мере миграции.

**Этапы миграции:**

```
Этап 1: Монолит обрабатывает все запросы
┌──────────┐
│ Клиенты  │──► Монолит
└──────────┘

Этап 2: Фасад (API Gateway) перенаправляет часть запросов
┌──────────┐    ┌─────────┐    ┌──────────────┐
│ Клиенты  │──► │ Gateway │──► │   Монолит    │ (старые функции)
└──────────┘    └────┬────┘    └──────────────┘
                     │
                     └──────► ┌──────────────┐
                              │ Микросервис A │ (новые/мигрированные)
                              └──────────────┘

Этап 3: Большинство функций мигрировано
┌──────────┐    ┌─────────┐    ┌──────────────┐
│ Клиенты  │──► │ Gateway │──► │   Монолит    │ (минимум функций)
└──────────┘    └────┬────┘    └──────────────┘
                     ├──────► Микросервис A
                     ├──────► Микросервис B
                     └──────► Микросервис C

Этап 4: Монолит полностью заменён
┌──────────┐    ┌─────────┐
│ Клиенты  │──► │ Gateway │──► Микросервисы
└──────────┘    └─────────┘
```

**Реализация через Spring Cloud Gateway:**

```yaml
spring:
  cloud:
    gateway:
      routes:
        # Новый функционал — микросервис
        - id: new-payments
          uri: lb://payment-service
          predicates:
            - Path=/api/v2/payments/**

        # Старый функционал — пока в монолите
        - id: legacy-reports
          uri: http://monolith:8080
          predicates:
            - Path=/api/reports/**

        # Постепенная миграция: сначала 10% трафика на новый сервис
        - id: canary-customers
          uri: lb://customer-service
          predicates:
            - Path=/api/customers/**
            - Weight=new, 10

        - id: legacy-customers
          uri: http://monolith:8080
          predicates:
            - Path=/api/customers/**
            - Weight=legacy, 90
```

**Стратегии выделения микросервисов из монолита:**
1. **Новая функциональность** — всё новое пишем как микросервис.
2. **По бизнес-домену** — выделяем наиболее изолированные модули.
3. **По частоте изменений** — выделяем то, что часто меняется.
4. **По требованиям к масштабированию** — выделяем «горячие» модули.

**Anti-Corruption Layer** — важнейший элемент при миграции. Это слой-адаптер между микросервисом и монолитом, который транслирует модели и предотвращает загрязнение новой чистой модели старыми структурами.

```java
// Anti-Corruption Layer в новом микросервисе
@Service
public class LegacyCustomerAdapter implements CustomerProvider {
    private final LegacyMonolithClient monolithClient;

    @Override
    public Customer getCustomer(Long id) {
        // Получаем данные из монолита в его формате
        LegacyCustomerDto legacy = monolithClient.fetchCustomer(id);
        // Преобразуем в нашу чистую доменную модель
        return Customer.builder()
            .id(legacy.getCustId())           // custId → id
            .fullName(legacy.getFio())         // fio → fullName
            .email(legacy.getElectronicMail()) // electronicMail → email
            .build();
    }
}
```

[к оглавлению](#Микросервисы)

## Как организовать распределённую трассировку в микросервисах?

**Distributed Tracing** (распределённая трассировка) — это метод отслеживания прохождения запроса через цепочку микросервисов. Позволяет понять, какие сервисы были вызваны, сколько времени занял каждый вызов и где произошла ошибка.

**Основные понятия:**
+ **Trace** — полный путь запроса через все сервисы (от входа до ответа).
+ **Span** — одна операция в рамках trace (например, вызов одного сервиса или запрос к БД).
+ **Trace ID** — уникальный идентификатор всего запроса (передаётся между сервисами).
+ **Span ID** — идентификатор конкретной операции.
+ **Parent Span ID** — ссылка на родительский span (для построения дерева вызовов).

```
Trace ID: abc123
├── Span 1: API Gateway (50ms)
│   ├── Span 2: Payment Service (30ms)
│   │   ├── Span 3: DB Query (5ms)
│   │   └── Span 4: Customer Service call (20ms)
│   │       └── Span 5: DB Query (3ms)
│   └── Span 6: Notification Service (10ms)
```

**Micrometer Tracing + Zipkin (Spring Boot 3+):**

```xml
<!-- pom.xml -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-tracing-bridge-brave</artifactId>
</dependency>
<dependency>
    <groupId>io.zipkin.reporter2</groupId>
    <artifactId>zipkin-reporter-brave</artifactId>
</dependency>
```

```yaml
# application.yml
management:
  tracing:
    sampling:
      probability: 1.0  # 100% запросов трассируются (в prod обычно 0.1)
  zipkin:
    tracing:
      endpoint: http://zipkin:9411/api/v2/spans

logging:
  pattern:
    level: "%5p [${spring.application.name},%X{traceId},%X{spanId}]"
```

Trace ID и Span ID автоматически прокидываются в логи:
```
INFO [payment-service,abc123def456,span789012] Processing payment #42
INFO [customer-service,abc123def456,span345678] Fetching customer data
```

**Создание кастомных span:**

```java
@Service
@RequiredArgsConstructor
public class PaymentService {
    private final ObservationRegistry observationRegistry;

    public void processPayment(PaymentRequest request) {
        Observation.createNotStarted("payment.processing", observationRegistry)
            .lowCardinalityKeyValue("payment.type", request.getType().name())
            .observe(() -> {
                // Этот блок будет отдельным span
                validatePayment(request);
                executePayment(request);
            });
    }
}
```

**Инструменты визуализации:**
+ **Zipkin** — простой и легковесный, хорошо интегрируется со Spring.
+ **Jaeger** — более мощный, поддерживает сэмплирование, создан Uber.
+ **Grafana Tempo** — интеграция с Grafana, поддержка больших объёмов.

Распределённая трассировка — обязательный инструмент в банковской среде для отслеживания прохождения транзакций через десятки сервисов и быстрого выявления проблем.

[к оглавлению](#Микросервисы)

## Как организовать централизованное логирование?

В микросервисной архитектуре логи каждого сервиса разбросаны по разным серверам и контейнерам. **Централизованное логирование** собирает все логи в одном месте для удобного поиска и анализа.

**ELK Stack (Elasticsearch + Logstash + Kibana):**

```
Микросервисы → Logstash (или Filebeat) → Elasticsearch → Kibana
```

**Настройка структурированного логирования в Spring Boot (Logback + JSON):**

```xml
<!-- logback-spring.xml -->
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="net.logstash.logback.encoder.LogstashEncoder">
            <includeMdcKeyName>traceId</includeMdcKeyName>
            <includeMdcKeyName>spanId</includeMdcKeyName>
            <customFields>
                {"service":"payment-service","environment":"${ENV}"}
            </customFields>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```

Результат — структурированный JSON-лог:
```json
{
  "@timestamp": "2026-04-18T10:30:00.123Z",
  "level": "INFO",
  "service": "payment-service",
  "traceId": "abc123def456",
  "spanId": "span789012",
  "message": "Платёж обработан успешно",
  "paymentId": "PAY-001",
  "amount": 50000,
  "environment": "production"
}
```

**Grafana Loki — более лёгкая альтернатива ELK:**

Loki не индексирует содержимое логов, а только метки (labels), что делает его значительно дешевле и проще в эксплуатации.

```yaml
# docker-compose.yml для Loki + Grafana
services:
  loki:
    image: grafana/loki:latest
    ports:
      - "3100:3100"

  promtail:
    image: grafana/promtail:latest
    volumes:
      - /var/log:/var/log
    command: -config.file=/etc/promtail/config.yml

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
```

**Рекомендации по логированию в микросервисах:**
+ **Всегда включайте traceId** — для связывания логов одного запроса.
+ **Используйте структурированные логи** (JSON) — для удобного поиска и фильтрации.
+ **Логируйте бизнес-события** — не только ошибки, но и важные бизнес-действия (платёж проведён, клиент создан).
+ **Не логируйте чувствительные данные** — номера карт, пароли, персональные данные (требования PCI DSS в банке!).
+ **Используйте уровни логирования правильно:** ERROR — сбои, WARN — нештатные ситуации, INFO — бизнес-события, DEBUG — для отладки (отключено в prod).

```java
@Slf4j
@Service
public class PaymentService {

    public void processPayment(PaymentRequest request) {
        log.info("Начало обработки платежа: paymentId={}, amount={}, currency={}",
            request.getPaymentId(), request.getAmount(), request.getCurrency());
        try {
            // ... бизнес-логика
            log.info("Платёж успешно обработан: paymentId={}", request.getPaymentId());
        } catch (InsufficientFundsException e) {
            log.warn("Недостаточно средств: paymentId={}, accountId={}",
                request.getPaymentId(), request.getAccountId());
            throw e;
        } catch (Exception e) {
            log.error("Ошибка обработки платежа: paymentId={}",
                request.getPaymentId(), e);
            throw e;
        }
    }
}
```

[к оглавлению](#Микросервисы)

## Как реализовать health checks и мониторинг микросервисов?

**Health checks** и мониторинг — это критически важные аспекты production-ready микросервисов, позволяющие вовремя обнаруживать проблемы.

**Spring Boot Actuator:**

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
        include: health, info, metrics, prometheus
  endpoint:
    health:
      show-details: when-authorized
      show-components: always
  health:
    db:
      enabled: true
    diskspace:
      enabled: true
    kafka:
      enabled: true
```

**Кастомный Health Indicator:**

```java
@Component
public class PaymentGatewayHealthIndicator implements HealthIndicator {

    private final PaymentGatewayClient gatewayClient;

    @Override
    public Health health() {
        try {
            boolean isAvailable = gatewayClient.ping();
            if (isAvailable) {
                return Health.up()
                    .withDetail("gateway", "Payment Gateway доступен")
                    .withDetail("responseTime", "45ms")
                    .build();
            } else {
                return Health.down()
                    .withDetail("gateway", "Payment Gateway не отвечает")
                    .build();
            }
        } catch (Exception e) {
            return Health.down(e)
                .withDetail("error", e.getMessage())
                .build();
        }
    }
}
```

**Liveness и Readiness Probes (для Kubernetes):**

```yaml
management:
  endpoint:
    health:
      probes:
        enabled: true
  health:
    livenessstate:
      enabled: true
    readinessstate:
      enabled: true
```

+ `/actuator/health/liveness` — жив ли сервис (если нет — Kubernetes перезапустит контейнер).
+ `/actuator/health/readiness` — готов ли сервис принимать трафик (если нет — Kubernetes перестанет направлять трафик).

```yaml
# Kubernetes deployment
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
  initialDelaySeconds: 20
  periodSeconds: 5
```

**Prometheus + Grafana — мониторинг метрик:**

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

```java
// Кастомные метрики
@Service
public class PaymentService {
    private final Counter paymentCounter;
    private final Timer paymentTimer;

    public PaymentService(MeterRegistry registry) {
        this.paymentCounter = Counter.builder("payments.processed")
            .description("Количество обработанных платежей")
            .tag("service", "payment-service")
            .register(registry);

        this.paymentTimer = Timer.builder("payments.duration")
            .description("Время обработки платежа")
            .register(registry);
    }

    public void processPayment(PaymentRequest request) {
        paymentTimer.record(() -> {
            // Бизнес-логика
            doProcess(request);
            paymentCounter.increment();
        });
    }
}
```

Endpoint `/actuator/prometheus` предоставляет метрики в формате Prometheus:
```
# HELP payments_processed_total Количество обработанных платежей
payments_processed_total{service="payment-service"} 1542.0
# HELP payments_duration_seconds Время обработки платежа
payments_duration_seconds_sum{service="payment-service"} 125.4
```

**Ключевые метрики для мониторинга (RED-метрики):**
+ **Rate** — количество запросов в секунду.
+ **Errors** — процент ошибок.
+ **Duration** — время ответа (p50, p95, p99).

[к оглавлению](#Микросервисы)

## Что такое идемпотентность и почему она важна в микросервисах?

**Идемпотентность** — это свойство операции, при котором многократное выполнение даёт тот же результат, что и однократное. В контексте микросервисов это означает, что повторная отправка одного и того же запроса не приводит к дублированию побочных эффектов.

**Почему это критично:**
+ Сетевые сбои: клиент отправил запрос, не получил ответ (таймаут) и повторяет.
+ Retry-механизмы: автоматический retry при ошибке может отправить запрос повторно.
+ Kafka: при ребалансировке потребитель может обработать сообщение дважды (at-least-once delivery).
+ В банковской системе списание денег дважды — это катастрофа.

**Идемпотентность HTTP-методов:**
+ **GET, PUT, DELETE** — идемпотентны по спецификации.
+ **POST** — НЕ идемпотентен (создание ресурса), требует дополнительных мер.

**Реализация через Idempotency Key:**

```java
// Клиент передаёт уникальный ключ идемпотентности
@PostMapping("/api/payments")
public ResponseEntity<PaymentResponse> createPayment(
        @RequestHeader("Idempotency-Key") String idempotencyKey,
        @RequestBody PaymentRequest request) {
    return paymentService.processPayment(idempotencyKey, request);
}

@Service
@RequiredArgsConstructor
public class PaymentService {
    private final PaymentRepository paymentRepository;
    private final IdempotencyStore idempotencyStore; // Redis

    @Transactional
    public ResponseEntity<PaymentResponse> processPayment(
            String idempotencyKey, PaymentRequest request) {

        // 1. Проверяем, не обрабатывался ли уже этот запрос
        Optional<PaymentResponse> existing = idempotencyStore.get(idempotencyKey);
        if (existing.isPresent()) {
            return ResponseEntity.ok(existing.get()); // Возвращаем сохранённый ответ
        }

        // 2. Обрабатываем платёж
        Payment payment = executePayment(request);
        PaymentResponse response = toResponse(payment);

        // 3. Сохраняем результат с TTL
        idempotencyStore.save(idempotencyKey, response, Duration.ofHours(24));

        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
}

// Redis-хранилище идемпотентных ключей
@Repository
@RequiredArgsConstructor
public class RedisIdempotencyStore implements IdempotencyStore {
    private final RedisTemplate<String, PaymentResponse> redisTemplate;

    public void save(String key, PaymentResponse response, Duration ttl) {
        redisTemplate.opsForValue().set("idempotency:" + key, response, ttl);
    }

    public Optional<PaymentResponse> get(String key) {
        return Optional.ofNullable(
            redisTemplate.opsForValue().get("idempotency:" + key));
    }
}
```

**Идемпотентность Kafka-консьюмера:**

```java
@KafkaListener(topics = "payment-events")
public void handlePaymentEvent(PaymentEvent event) {
    // Проверяем, обрабатывали ли уже это событие
    if (processedEventRepository.existsByEventId(event.getEventId())) {
        log.info("Событие {} уже обработано, пропускаем", event.getEventId());
        return;
    }

    // Обрабатываем и помечаем как обработанное в одной транзакции
    processEvent(event);
    processedEventRepository.save(new ProcessedEvent(event.getEventId()));
}
```

[к оглавлению](#Микросервисы)

## Как версионировать API микросервисов?

Версионирование API необходимо для обратной совместимости — чтобы обновление одного сервиса не ломало потребителей его API.

**Основные подходы к версионированию:**

**1. Версия в URL (наиболее распространённый):**

```java
@RestController
@RequestMapping("/api/v1/payments")
public class PaymentControllerV1 {
    @GetMapping("/{id}")
    public PaymentResponseV1 getPayment(@PathVariable Long id) {
        return paymentService.getPaymentV1(id);
    }
}

@RestController
@RequestMapping("/api/v2/payments")
public class PaymentControllerV2 {
    @GetMapping("/{id}")
    public PaymentResponseV2 getPayment(@PathVariable Long id) {
        // Новая версия с дополнительными полями
        return paymentService.getPaymentV2(id);
    }
}
```

**2. Версия в заголовке:**

```java
@GetMapping(value = "/api/payments/{id}", headers = "X-API-Version=1")
public PaymentResponseV1 getPaymentV1(@PathVariable Long id) { ... }

@GetMapping(value = "/api/payments/{id}", headers = "X-API-Version=2")
public PaymentResponseV2 getPaymentV2(@PathVariable Long id) { ... }
```

**3. Версия через Accept header (Content Negotiation):**

```java
@GetMapping(value = "/api/payments/{id}",
            produces = "application/vnd.bank.payment.v1+json")
public PaymentResponseV1 getPaymentV1(@PathVariable Long id) { ... }

@GetMapping(value = "/api/payments/{id}",
            produces = "application/vnd.bank.payment.v2+json")
public PaymentResponseV2 getPaymentV2(@PathVariable Long id) { ... }
```

**Сравнение подходов:**

| Подход | Плюсы | Минусы |
|---|---|---|
| URL | Простой, наглядный, кешируемый | «Загрязняет» URL |
| Header | Чистый URL | Сложнее тестировать и документировать |
| Content-Type | Следует HTTP-стандартам | Самый сложный |

**Правила эволюции API (обратная совместимость):**
+ **Добавление** нового поля в ответ — безопасно (клиенты игнорируют неизвестные поля).
+ **Удаление** обязательного поля — НЕ безопасно, требует новой версии.
+ **Изменение** типа поля — НЕ безопасно.
+ **Добавление** нового опционального параметра запроса — безопасно.
+ Используйте `@JsonIgnoreProperties(ignoreUnknown = true)` на DTO-клиентах для толерантности к новым полям.

**Стратегия deprecation:**

```java
@Deprecated(since = "2025-01-01", forRemoval = true)
@GetMapping("/api/v1/payments/{id}")
public PaymentResponseV1 getPaymentV1(@PathVariable Long id) {
    response.addHeader("Sunset", "Sat, 01 Jul 2025 00:00:00 GMT");
    response.addHeader("Deprecation", "true");
    response.addHeader("Link", "</api/v2/payments>; rel=\"successor-version\"");
    return paymentService.getPaymentV1(id);
}
```

[к оглавлению](#Микросервисы)

## Как тестировать микросервисы?

Тестирование микросервисов строится на основе **пирамиды тестирования**, адаптированной для распределённой архитектуры:

```
         ╱  E2E тесты  ╲         — мало (дорогие, хрупкие)
        ╱ Integration    ╲        — средне
       ╱ Contract tests   ╲       — много (между сервисами)
      ╱ Component tests    ╲      — средне (один сервис целиком)
     ╱ Unit tests           ╲     — очень много (быстрые)
```

**1. Unit-тесты — тестирование бизнес-логики:**

```java
@ExtendWith(MockitoExtension.class)
class PaymentServiceTest {
    @Mock private PaymentRepository paymentRepository;
    @Mock private CustomerClient customerClient;
    @InjectMocks private PaymentService paymentService;

    @Test
    void shouldRejectPaymentWhenInsufficientFunds() {
        when(paymentRepository.findById(1L))
            .thenReturn(Optional.of(new Payment(1L, new BigDecimal("100"))));

        assertThrows(InsufficientFundsException.class, () ->
            paymentService.withdraw(1L, new BigDecimal("200")));
    }
}
```

**2. Component-тесты — тест одного сервиса с поднятым контекстом и тестовой БД:**

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Testcontainers
class PaymentServiceComponentTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void shouldCreatePayment() {
        PaymentRequest request = new PaymentRequest(BigDecimal.valueOf(1000), "RUB");

        ResponseEntity<PaymentResponse> response = restTemplate
            .postForEntity("/api/payments", request, PaymentResponse.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(response.getBody().getStatus()).isEqualTo("CREATED");
    }
}
```

**3. Contract Testing с Pact — проверка совместимости API между сервисами:**

Contract testing гарантирует, что API-контракт между consumer (потребителем) и provider (поставщиком) не нарушен.

```java
// Consumer-сторона (сервис, который вызывает payment-service)
@ExtendWith(PactConsumerTestExt.class)
@PactTestFor(providerName = "payment-service")
class PaymentClientContractTest {

    @Pact(consumer = "order-service")
    public V4Pact createPact(PactDslWithProvider builder) {
        return builder
            .given("Платёж с ID 1 существует")
            .uponReceiving("Запрос платежа по ID")
            .path("/api/payments/1")
            .method("GET")
            .willRespondWith()
            .status(200)
            .body(newJsonBody(body -> {
                body.numberType("id", 1L);
                body.stringType("status", "COMPLETED");
                body.decimalType("amount", 1000.00);
            }).build())
            .toPact(V4Pact.class);
    }

    @Test
    @PactTestFor(pactMethod = "createPact")
    void shouldGetPayment(MockServer mockServer) {
        PaymentClient client = new PaymentClient(mockServer.getUrl());
        PaymentResponse payment = client.getPayment(1L);

        assertThat(payment.getStatus()).isEqualTo("COMPLETED");
    }
}

// Provider-сторона (payment-service проверяет, что соответствует контракту)
@Provider("payment-service")
@PactBroker(url = "http://pact-broker:9292")
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class PaymentProviderContractTest {

    @TestTarget
    public final Target target = new SpringBootHttpTarget();

    @State("Платёж с ID 1 существует")
    void setupPaymentExists() {
        paymentRepository.save(new Payment(1L, BigDecimal.valueOf(1000), "COMPLETED"));
    }
}
```

**4. Integration-тесты с WireMock (мокирование внешних сервисов):**

```java
@SpringBootTest
@AutoConfigureWireMock(port = 0)
class PaymentServiceIntegrationTest {

    @Test
    void shouldCallCustomerServiceAndProcessPayment() {
        // Мокируем внешний сервис
        stubFor(get(urlEqualTo("/api/customers/1"))
            .willReturn(aResponse()
                .withHeader("Content-Type", "application/json")
                .withBody("""
                    {"id": 1, "name": "Иванов", "status": "ACTIVE"}
                    """)));

        // Выполняем тест
        PaymentResponse result = paymentService.processPayment(
            new PaymentRequest(1L, BigDecimal.valueOf(5000)));

        assertThat(result.getStatus()).isEqualTo("COMPLETED");
        verify(getRequestedFor(urlEqualTo("/api/customers/1")));
    }
}
```

[к оглавлению](#Микросервисы)

## Что такое 12-Factor App?

**12-Factor App** — это методология разработки SaaS-приложений, описанная инженерами Heroku. Она особенно актуальна для микросервисов, так как определяет лучшие практики создания облачных приложений.

**12 факторов:**

**1. Codebase (Кодовая база)** — одна кодовая база в системе контроля версий, много деплоев (dev, staging, prod).
```
Git repo → dev deploy
         → staging deploy
         → production deploy
```

**2. Dependencies (Зависимости)** — все зависимости явно объявлены и изолированы. В Java это `pom.xml` или `build.gradle`. Никогда не полагайтесь на системные библиотеки.

**3. Config (Конфигурация)** — конфигурация хранится в переменных окружения, а не в коде.
```java
// Правильно:
@Value("${DATABASE_URL}")
private String databaseUrl;

// Неправильно:
private String databaseUrl = "jdbc:postgresql://prod-db:5432/payments";
```

**4. Backing Services (Сторонние сервисы)** — БД, брокеры, кеши — это подключаемые ресурсы, заменяемые без изменения кода.
```yaml
# Локальная БД или облачная — только через конфиг
spring:
  datasource:
    url: ${DATABASE_URL}
```

**5. Build, Release, Run (Сборка, релиз, выполнение)** — строгое разделение этапов. Сборка (Maven/Gradle) → Релиз (сборка + конфиг) → Выполнение (запуск контейнера).

**6. Processes (Процессы)** — приложение выполняется как один или несколько stateless-процессов. Состояние хранится в backing services (БД, Redis).

**7. Port Binding (Привязка портов)** — приложение самодостаточно и экспортирует HTTP через привязку к порту. Spring Boot со встроенным Tomcat/Netty — идеальный пример.

**8. Concurrency (Конкурентность)** — масштабирование через запуск дополнительных процессов (горизонтальное масштабирование), а не увеличение мощности одного процесса.

**9. Disposability (Утилизируемость)** — быстрый запуск и корректное завершение (graceful shutdown).
```yaml
server:
  shutdown: graceful
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
```

**10. Dev/Prod Parity (Паритет окружений)** — разработка, staging и production должны быть максимально похожи. Docker и Testcontainers помогают с этим.

**11. Logs (Логи)** — приложение не управляет лог-файлами, а пишет логи в stdout. Инфраструктура (Docker, Kubernetes) собирает и маршрутизирует логи.

**12. Admin Processes (Административные процессы)** — одноразовые задачи (миграции БД, скрипты) запускаются в том же окружении, что и основное приложение.
```bash
# Flyway-миграции запускаются при старте приложения
# или как отдельный Job в Kubernetes
```

12-Factor App — это фундамент cloud-native микросервисов. Следование этим принципам обеспечивает переносимость, масштабируемость и предсказуемость поведения.

[к оглавлению](#Микросервисы)

## Что такое паттерн Sidecar?

**Sidecar** — это паттерн развёртывания, при котором вспомогательный компонент (sidecar-контейнер) работает рядом с основным сервисом в одном pod'е (в Kubernetes) и берёт на себя сквозные задачи (cross-cutting concerns).

```
┌─── Pod ─────────────────────────────┐
│                                     │
│  ┌────────────────┐  ┌───────────┐  │
│  │  Основной      │  │  Sidecar  │  │
│  │  сервис        │◄─►│           │  │
│  │  (Payment App) │  │  (Envoy)  │  │
│  └────────────────┘  └───────────┘  │
│                                     │
│  Общий localhost, общие volumes     │
└─────────────────────────────────────┘
```

**Что может делать Sidecar:**
+ **Проксирование трафика** — mTLS, retry, circuit breaker (Envoy в Istio).
+ **Логирование** — сбор и отправка логов (Fluentd sidecar).
+ **Мониторинг** — экспорт метрик.
+ **Безопасность** — аутентификация, авторизация, шифрование трафика.
+ **Конфигурация** — подтягивание секретов из Vault.

**Пример в Kubernetes:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: payment-service
spec:
  containers:
    # Основной контейнер
    - name: payment-app
      image: bank/payment-service:1.0
      ports:
        - containerPort: 8080

    # Sidecar для логирования
    - name: log-collector
      image: fluent/fluentd:latest
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/app

    # Sidecar для экспорта метрик
    - name: metrics-exporter
      image: prom/node-exporter:latest
      ports:
        - containerPort: 9100

  volumes:
    - name: shared-logs
      emptyDir: {}
```

**Преимущества:**
+ Основной сервис не содержит инфраструктурного кода — разработчик фокусируется на бизнес-логике.
+ Sidecar может быть на другом языке (Envoy на C++, сервис на Java).
+ Единообразие — одна конфигурация sidecar для всех сервисов.

**Недостатки:**
+ Дополнительное потребление ресурсов (CPU, RAM).
+ Увеличенная задержка (дополнительный hop через sidecar).
+ Сложность отладки.

Sidecar — основа Service Mesh (Istio, Linkerd), который активно используется в крупных банковских системах для управления сетевым взаимодействием.

[к оглавлению](#Микросервисы)

## Что такое паттерн BFF (Backend for Frontend)?

**BFF (Backend for Frontend)** — это паттерн, при котором для каждого типа клиента (мобильное приложение, веб-приложение, партнёрское API) создаётся отдельный backend-сервис, оптимизированный под потребности этого клиента.

```
┌──────────┐   ┌──────────┐   ┌──────────────┐
│ Мобильное│   │ Веб-     │   │ Партнёрское  │
│ прилож.  │   │ прилож.  │   │ API          │
└────┬─────┘   └────┬─────┘   └─────┬────────┘
     │              │               │
┌────▼─────┐   ┌────▼─────┐   ┌────▼────────┐
│ Mobile   │   │ Web      │   │ Partner     │
│ BFF      │   │ BFF      │   │ BFF         │
└────┬─────┘   └────┬─────┘   └─────┬───────┘
     │              │               │
     └──────────────┼───────────────┘
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   Payment     Customer    Notification
   Service     Service     Service
```

**Зачем нужен BFF:**

1. **Разные потребности клиентов.** Мобильному приложению нужен минимум данных (экономия трафика), а веб-приложению — полный набор полей.

2. **Агрегация данных.** BFF может объединить данные из нескольких микросервисов в один ответ, чтобы клиент не делал множество вызовов.

3. **Разная частота изменений.** Мобильное приложение обновляется реже, и его BFF может поддерживать старые форматы дольше.

**Пример:**

```java
// Mobile BFF — минимум данных, оптимизация для мобильных
@RestController
@RequestMapping("/mobile/api")
public class MobileAccountController {

    private final AccountService accountService;
    private final TransactionService transactionService;

    @GetMapping("/dashboard")
    public MobileDashboardResponse getDashboard(@AuthenticationPrincipal User user) {
        // Один вызов — все данные для главного экрана мобилки
        Account account = accountService.getMainAccount(user.getId());
        List<Transaction> recent = transactionService.getRecent(user.getId(), 5);

        return MobileDashboardResponse.builder()
            .balance(account.getBalance())
            .currency(account.getCurrency())
            .recentTransactions(recent.stream()
                .map(t -> new MobileTransactionDto(t.getAmount(), t.getDescription()))
                .toList())
            .build(); // Компактный ответ
    }
}

// Web BFF — больше данных, сложные представления
@RestController
@RequestMapping("/web/api")
public class WebAccountController {

    @GetMapping("/dashboard")
    public WebDashboardResponse getDashboard(@AuthenticationPrincipal User user) {
        // Полноценный dashboard для веба
        List<Account> accounts = accountService.getAllAccounts(user.getId());
        List<Transaction> transactions = transactionService.getRecent(user.getId(), 50);
        AnalyticsData analytics = analyticsService.getMonthlyAnalytics(user.getId());
        List<Notification> notifications = notificationService.getUnread(user.getId());

        return WebDashboardResponse.builder()
            .accounts(accounts)
            .transactions(transactions)
            .analytics(analytics)
            .notifications(notifications)
            .build(); // Полный набор данных
    }
}
```

**Когда использовать BFF:**
+ Есть несколько типов клиентов с разными потребностями.
+ Клиентам нужны агрегированные данные из нескольких сервисов.
+ Разные команды отвечают за разные клиентские приложения.

**Когда НЕ нужен BFF:**
+ Один тип клиента — достаточно API Gateway.
+ Все клиенты потребляют одинаковые данные.

[к оглавлению](#Микросервисы)

## Database per Service или Shared Database — какой подход выбрать?

Выбор стратегии управления данными — один из ключевых архитектурных решений в микросервисах.

**Database per Service (отдельная БД для каждого сервиса):**

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ Payment      │  │ Customer     │  │ Notification │
│ Service      │  │ Service      │  │ Service      │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │
┌──────▼───────┐  ┌──────▼───────┐  ┌──────▼───────┐
│ PostgreSQL   │  │ PostgreSQL   │  │  MongoDB     │
│ (payments)   │  │ (customers)  │  │ (notif.)     │
└──────────────┘  └──────────────┘  └──────────────┘
```

**Преимущества:**
+ Слабая связность — сервисы полностью независимы.
+ Каждый сервис выбирает оптимальное хранилище (polyglot persistence).
+ Независимое масштабирование БД.
+ Изменение схемы не затрагивает другие сервисы.
+ Сбой одной БД не влияет на другие сервисы.

**Недостатки:**
+ Сложность запросов к данным из нескольких сервисов (нет JOIN между базами).
+ Распределённые транзакции (Saga вместо ACID).
+ Дублирование данных (денормализация между сервисами).
+ Больше ресурсов на инфраструктуру.

**Shared Database (общая БД для нескольких сервисов):**

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ Payment      │  │ Customer     │  │ Report       │
│ Service      │  │ Service      │  │ Service      │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │
       └────────────┬────┴─────────────────┘
                    │
             ┌──────▼───────┐
             │  PostgreSQL  │
             │  (shared)    │
             └──────────────┘
```

**Преимущества:**
+ Простые JOIN-запросы между данными.
+ ACID-транзакции.
+ Нет дублирования данных.
+ Проще в начале разработки.

**Недостатки:**
+ Сильная связность — изменение схемы одной таблицы может сломать другие сервисы.
+ Один сервис может заблокировать таблицу, нужную другому.
+ Невозможно независимо масштабировать хранилище.
+ БД становится единой точкой отказа.

**Компромисс — Schema per Service:**

```sql
-- Каждый сервис имеет свою схему в одной БД
CREATE SCHEMA payment_service;
CREATE SCHEMA customer_service;

-- Сервисы обращаются ТОЛЬКО к своей схеме
-- payment_service имеет доступ ТОЛЬКО к payment_service.*
GRANT USAGE ON SCHEMA payment_service TO payment_svc_user;
REVOKE ALL ON SCHEMA customer_service FROM payment_svc_user;
```

**Рекомендации:**
+ **Начинайте с Database per Service** — это правильный подход для микросервисов.
+ Если нужны данные другого сервиса — запрашивайте через API, а не через прямой доступ к БД.
+ Используйте **Shared Database** только как временное решение при миграции с монолита (паттерн Strangler Fig).
+ Для отчётов и аналитики — используйте **CQRS** с отдельной read-моделью, агрегирующей данные из нескольких сервисов.

[к оглавлению](#Микросервисы)

## Что такое паттерны Retry и Timeout?

**Retry** и **Timeout** — это базовые паттерны обеспечения отказоустойчивости при межсервисном взаимодействии.

**Timeout** — ограничение времени ожидания ответа. Без таймаута один зависший сервис может «повесить» всю цепочку вызовов.

```java
// Таймаут через WebClient
WebClient webClient = WebClient.builder()
    .baseUrl("http://customer-service")
    .clientConnector(new ReactorClientHttpConnector(
        HttpClient.create()
            .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 3000)
            .responseTimeout(Duration.ofSeconds(5))
    ))
    .build();
```

**Retry** — автоматический повтор запроса при сбое. Важно повторять только **идемпотентные** операции и только при **transient** (временных) ошибках.

**Resilience4j Retry:**

```java
@Service
public class CustomerService {

    @Retry(name = "customerService", fallbackMethod = "getCustomerFallback")
    public CustomerDto getCustomer(Long id) {
        return customerClient.getCustomer(id);
    }

    private CustomerDto getCustomerFallback(Long id, Exception e) {
        log.warn("Все попытки вызова customer-service исчерпаны: {}", e.getMessage());
        return cachedCustomerRepository.findById(id)
            .orElseThrow(() -> new ServiceUnavailableException("Customer service недоступен"));
    }
}
```

```yaml
resilience4j:
  retry:
    instances:
      customerService:
        max-attempts: 3
        wait-duration: 500ms
        exponential-backoff-multiplier: 2  # 500ms, 1s, 2s
        retry-exceptions:
          - java.io.IOException
          - java.net.SocketTimeoutException
          - org.springframework.web.client.HttpServerErrorException
        ignore-exceptions:
          - org.springframework.web.client.HttpClientErrorException  # 4xx — не повторять!
```

**Exponential Backoff с Jitter — избежание «грозового стада»:**

```
Попытка 1: ошибка → ждём 500ms + random(0-200ms)
Попытка 2: ошибка → ждём 1000ms + random(0-400ms)
Попытка 3: ошибка → ждём 2000ms + random(0-800ms)
Попытка 4: ошибка → fallback
```

Без jitter все клиенты, получившие ошибку одновременно, будут повторять запросы в одно и то же время, создавая пиковую нагрузку.

**Программная реализация retry с Spring Retry:**

```java
@Configuration
@EnableRetry
public class RetryConfig {}

@Service
public class PaymentGatewayService {

    @Retryable(
        retryFor = {SocketTimeoutException.class, HttpServerErrorException.class},
        noRetryFor = {HttpClientErrorException.class},
        maxAttempts = 3,
        backoff = @Backoff(delay = 1000, multiplier = 2, maxDelay = 10000)
    )
    public PaymentResult sendPayment(PaymentRequest request) {
        return paymentGateway.execute(request);
    }

    @Recover
    public PaymentResult recoverSendPayment(Exception e, PaymentRequest request) {
        log.error("Платёжный шлюз недоступен после всех попыток: {}", e.getMessage());
        return PaymentResult.deferred(request.getId()); // Отложить обработку
    }
}
```

**Комбинация Retry + Timeout + Circuit Breaker:**

```yaml
# Порядок: Retry → CircuitBreaker → Timeout
resilience4j:
  retry:
    instances:
      paymentGateway:
        max-attempts: 3
        wait-duration: 500ms
  circuitbreaker:
    instances:
      paymentGateway:
        failure-rate-threshold: 50
        wait-duration-in-open-state: 30s
  timelimiter:
    instances:
      paymentGateway:
        timeout-duration: 5s
```

[к оглавлению](#Микросервисы)

## Что такое паттерн Bulkhead?

**Bulkhead** (переборка) — это паттерн отказоустойчивости, заимствованный из судостроения. Корабль разделён на отсеки (bulkheads): если один отсек затоплен, другие остаются сухими. Аналогично, в микросервисах ресурсы изолируются, чтобы сбой одного компонента не исчерпал ресурсы для других.

**Проблема без Bulkhead:**

Если один внешний сервис «завис» и отвечает медленно, все потоки (threads) пула будут заняты ожиданием ответа от него, и сервис не сможет обрабатывать запросы к другим, нормально работающим сервисам.

```
Без Bulkhead:
┌──────────────────────────────────┐
│ Thread Pool (20 потоков)         │
│                                  │
│ [customer] [customer] [customer] │ ← все потоки заняты зависшим
│ [customer] [customer] [customer] │   customer-service
│ [customer] ... нет потоков для   │
│ payment или notification!        │
└──────────────────────────────────┘

С Bulkhead:
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ Customer Pool│ │ Payment Pool │ │ Notif. Pool  │
│ (5 потоков)  │ │ (10 потоков) │ │ (5 потоков)  │
│ [зависли]    │ │ [работают]   │ │ [работают]   │
└──────────────┘ └──────────────┘ └──────────────┘
```

**Два типа Bulkhead:**

**1. Thread Pool Bulkhead — отдельный пул потоков для каждого внешнего вызова:**

```java
@Service
public class PaymentService {

    @Bulkhead(name = "customerService", type = Bulkhead.Type.THREADPOOL,
              fallbackMethod = "getCustomerFallback")
    public CompletableFuture<CustomerDto> getCustomer(Long id) {
        return CompletableFuture.supplyAsync(() -> customerClient.getCustomer(id));
    }

    private CompletableFuture<CustomerDto> getCustomerFallback(Long id, Throwable t) {
        log.warn("Bulkhead для customer-service полон: {}", t.getMessage());
        return CompletableFuture.completedFuture(
            CustomerDto.unknown(id));
    }
}
```

```yaml
resilience4j:
  thread-pool-bulkhead:
    instances:
      customerService:
        maxThreadPoolSize: 5
        coreThreadPoolSize: 3
        queueCapacity: 10
        keepAliveDuration: 60s
      paymentGateway:
        maxThreadPoolSize: 10
        coreThreadPoolSize: 5
        queueCapacity: 20
```

**2. Semaphore Bulkhead — ограничение количества одновременных вызовов (без отдельного пула):**

```java
@Bulkhead(name = "customerService", type = Bulkhead.Type.SEMAPHORE)
public CustomerDto getCustomer(Long id) {
    return customerClient.getCustomer(id);
}
```

```yaml
resilience4j:
  bulkhead:
    instances:
      customerService:
        maxConcurrentCalls: 5        # Максимум 5 одновременных вызовов
        maxWaitDuration: 500ms       # Время ожидания разрешения
```

**Сравнение:**

| Критерий | Thread Pool | Semaphore |
|---|---|---|
| Изоляция | Полная (отдельные потоки) | Частичная (общий пул) |
| Overhead | Выше (переключение контекста) | Ниже |
| Таймаут | Можно прервать поток | Нет контроля |
| Подходит для | Медленные вызовы | Быстрые вызовы |

**Комбинация паттернов отказоустойчивости (порядок имеет значение!):**
```
Запрос → Bulkhead → CircuitBreaker → Retry → Timeout → Вызов сервиса
```

Bulkhead обеспечивает, что даже при сбое одного из множества интегрированных банковских сервисов (а их может быть сотни) основные функции вашего сервиса продолжают работать.

[к оглавлению](#Микросервисы)

## Как обеспечить безопасность межсервисного взаимодействия?

Безопасность в микросервисной архитектуре — комплексная задача, особенно в банковской среде, где требования к защите данных максимально строгие.

**1. Аутентификация и авторизация через OAuth2/JWT:**

```
Клиент → API Gateway → Проверка JWT → Микросервис
                            │
                    ┌───────▼───────┐
                    │ Keycloak /    │
                    │ Spring Auth   │
                    │ Server        │
                    └───────────────┘
```

```java
// Конфигурация Resource Server (каждый микросервис)
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/actuator/health/**").permitAll()
                .requestMatchers("/api/payments/**").hasAuthority("SCOPE_payments")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthConverter()))
            )
            .build();
    }

    private JwtAuthenticationConverter jwtAuthConverter() {
        JwtAuthenticationConverter converter = new JwtAuthenticationConverter();
        converter.setJwtGrantedAuthoritiesConverter(jwt -> {
            // Извлекаем роли из JWT (Keycloak формат)
            Map<String, Object> realmAccess = jwt.getClaim("realm_access");
            List<String> roles = (List<String>) realmAccess.get("roles");
            return roles.stream()
                .map(role -> new SimpleGrantedAuthority("ROLE_" + role))
                .collect(Collectors.toList());
        });
        return converter;
    }
}
```

**2. mTLS (Mutual TLS) — взаимная аутентификация сервисов:**

При mTLS оба участника соединения предъявляют сертификаты, подтверждая свою идентичность. Обычно реализуется через Service Mesh (Istio):

```yaml
# Istio PeerAuthentication — требовать mTLS
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: banking
spec:
  mtls:
    mode: STRICT  # Только mTLS
```

**3. Передача контекста пользователя между сервисами:**

```java
// Перехватчик для прокидывания JWT между сервисами
@Component
public class AuthTokenInterceptor implements ClientHttpRequestInterceptor {

    @Override
    public ClientHttpResponse intercept(HttpRequest request, byte[] body,
            ClientHttpRequestExecution execution) throws IOException {

        String token = SecurityContextHolder.getContext().getAuthentication()
            .getCredentials().toString();
        request.getHeaders().setBearerAuth(token);

        return execution.execute(request, body);
    }
}
```

**4. Защита секретов:**
+ Хранение в HashiCorp Vault (не в переменных окружения и не в конфигурационных файлах).
+ Ротация секретов (Vault поддерживает автоматическую ротацию паролей БД).
+ Шифрование чувствительных данных в Kafka-сообщениях.

**5. Сетевые политики (Network Policies в Kubernetes):**

```yaml
# Разрешить трафик к payment-service только от api-gateway и order-service
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: payment-service-policy
spec:
  podSelector:
    matchLabels:
      app: payment-service
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: api-gateway
        - podSelector:
            matchLabels:
              app: order-service
      ports:
        - port: 8080
```

**6. Валидация входных данных** — каждый сервис должен сам валидировать входные данные, не полагаясь на то, что вызывающий сервис уже провёл валидацию.

[к оглавлению](#Микросервисы)

## Что такое Service Mesh?

**Service Mesh** — это инфраструктурный слой, который управляет межсервисным взаимодействием. Он реализуется через sidecar-прокси, развёрнутые рядом с каждым сервисом, и берёт на себя сетевые задачи без изменения кода приложения.

```
┌─── Data Plane ──────────────────────────────────────────┐
│                                                         │
│  ┌──────┐ ┌───────┐       ┌──────┐ ┌───────┐          │
│  │ App A│─│Proxy A│──────►│Proxy B│─│ App B │          │
│  └──────┘ └───┬───┘       └───┬───┘ └──────┘          │
│               │               │                        │
└───────────────┼───────────────┼────────────────────────┘
                │               │
┌─── Control Plane ─────────────┼───────────────────────┐
│               │               │                        │
│  ┌────────────▼───────────────▼──────────────┐        │
│  │ Istio Control Plane (istiod)              │        │
│  │ - Маршрутизация   - mTLS-сертификаты      │        │
│  │ - Политики        - Конфигурация прокси   │        │
│  └───────────────────────────────────────────┘        │
└───────────────────────────────────────────────────────┘
```

**Что предоставляет Service Mesh:**
+ **Наблюдаемость (Observability)** — метрики, трассировка, логирование — автоматически, без изменения кода.
+ **Безопасность** — mTLS между всеми сервисами, RBAC-политики.
+ **Управление трафиком** — canary deployments, A/B тестирование, fault injection.
+ **Отказоустойчивость** — retry, timeout, circuit breaker — на уровне прокси.

**Популярные решения:**
+ **Istio** — наиболее функциональный, на основе Envoy. Стандарт де-факто.
+ **Linkerd** — легковесный, проще в эксплуатации.
+ **Consul Connect** — от HashiCorp, интегрирован с Consul.

**Пример Istio: canary deployment:**

```yaml
# Направить 90% трафика на v1 и 10% на v2
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: payment-service
spec:
  hosts:
    - payment-service
  http:
    - route:
        - destination:
            host: payment-service
            subset: v1
          weight: 90
        - destination:
            host: payment-service
            subset: v2
          weight: 10
---
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: payment-service
spec:
  host: payment-service
  subsets:
    - name: v1
      labels:
        version: v1
    - name: v2
      labels:
        version: v2
```

**Пример Istio: fault injection для тестирования устойчивости:**

```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: customer-service
spec:
  hosts:
    - customer-service
  http:
    - fault:
        delay:
          percentage:
            value: 10  # 10% запросов получат задержку
          fixedDelay: 5s
        abort:
          percentage:
            value: 5   # 5% запросов получат ошибку 500
          httpStatus: 500
      route:
        - destination:
            host: customer-service
```

**Когда нужен Service Mesh:**
+ Много микросервисов (50+) — ручное управление сетевыми политиками невозможно.
+ Строгие требования к безопасности (mTLS везде).
+ Нужна единообразная наблюдаемость без изменения кода сервисов.

**Когда НЕ нужен:**
+ Мало сервисов (менее 10-15).
+ Нет Kubernetes.
+ Команда не готова к дополнительной операционной сложности.

[к оглавлению](#Микросервисы)

## Как организовать деплой микросервисов?

Развёртывание микросервисов требует зрелых CI/CD практик и стратегий, минимизирующих риски. В банковской среде с 5000+ системами это особенно важно.

**Стратегии развёртывания:**

**1. Blue-Green Deployment:**

```
┌───────────┐
│ Load      │
│ Balancer  │──── 100% ────► Blue (v1.0) — текущая версия
│           │                Green (v2.0) — новая версия (тестируется)
└───────────┘

После проверки:
┌───────────┐
│ Load      │
│ Balancer  │──── 100% ────► Green (v2.0) — теперь активная
│           │                Blue (v1.0) — rollback при проблемах
└───────────┘
```

+ Мгновенное переключение и мгновенный откат.
+ Требует двойных ресурсов.

**2. Canary Deployment (канареечное развёртывание):**

```
Этап 1: 5% трафика на новую версию
Этап 2: 25% (если метрики в норме)
Этап 3: 50%
Этап 4: 100%
```

Позволяет выявить проблемы на малом проценте пользователей.

**3. Rolling Update (стандарт в Kubernetes):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: payment-service
spec:
  replicas: 4
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1        # Создать 1 новый pod перед удалением старого
      maxUnavailable: 0   # Не допускать недоступности (важно для банка!)
  template:
    spec:
      containers:
        - name: payment-service
          image: bank-registry/payment-service:2.0
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 8080
            initialDelaySeconds: 20
            periodSeconds: 5
```

**CI/CD Pipeline для микросервиса:**

```yaml
# Jenkinsfile / GitLab CI
stages:
  - build       # Maven build + unit tests
  - test        # Integration tests + Contract tests
  - scan        # SonarQube + SAST/DAST + License compliance
  - package     # Docker build + push to registry
  - deploy-dev  # Deploy to dev
  - deploy-stage # Deploy to staging + E2E tests
  - deploy-prod # Canary → Rolling update to production
```

**Graceful Shutdown — корректное завершение при обновлении:**

```yaml
# application.yml
server:
  shutdown: graceful
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s

# Kubernetes
spec:
  terminationGracePeriodSeconds: 60
  containers:
    - lifecycle:
        preStop:
          exec:
            command: ["sh", "-c", "sleep 10"]  # Даём время LB убрать pod из ротации
```

```java
// Обработка graceful shutdown для Kafka consumer
@PreDestroy
public void onShutdown() {
    log.info("Получен сигнал завершения. Останавливаем обработку сообщений...");
    kafkaListenerEndpointRegistry.stop();
    // Дожидаемся завершения текущих операций
}
```

**Рекомендации для банковского окружения:**
+ Каждый сервис деплоится независимо, со своим пайплайном.
+ Обязательные этапы: сканирование безопасности (SAST/DAST), проверка лицензий.
+ Feature flags (Unleash, LaunchDarkly) для безопасного включения новых функций.
+ Автоматический rollback при деградации метрик.
+ Чёткая процедура согласования релизов (change management).

[к оглавлению](#Микросервисы)
