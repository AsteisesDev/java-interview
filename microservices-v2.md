[Вопросы для собеседования](README.md)

# Микросервисы

+ [Что такое микросервисная архитектура?](#что-такое-микросервисная-архитектура)
+ [Чем микросервисная архитектура отличается от монолитной?](#чем-микросервисная-архитектура-отличается-от-монолитной)
+ [Какие преимущества у микросервисной архитектуры?](#какие-преимущества-у-микросервисной-архитектуры)
+ [Какие недостатки у микросервисной архитектуры?](#какие-недостатки-у-микросервисной-архитектуры)
+ [Когда стоит использовать микросервисы, а когда нет?](#когда-стоит-использовать-микросервисы-а-когда-нет)
+ [Какие принципы декомпозиции на микросервисы существуют?](#какие-принципы-декомпозиции-на-микросервисы-существуют)
+ [Что такое Bounded Context и как он связан с микросервисами?](#что-такое-bounded-context-и-как-он-связан-с-микросервисами)
+ [Какие способы взаимодействия микросервисов существуют?](#какие-способы-взаимодействия-микросервисов-существуют)
+ [Как организовать синхронное взаимодействие микросервисов через REST?](#как-организовать-синхронное-взаимодействие-микросервисов-через-rest)
+ [Что такое gRPC и когда его стоит использовать вместо REST?](#что-такое-grpc-и-когда-его-стоит-использовать-вместо-rest)
+ [Как организовать асинхронное взаимодействие через брокеры сообщений?](#как-организовать-асинхронное-взаимодействие-через-брокеры-сообщений)
+ [Что такое API Gateway и зачем он нужен?](#что-такое-api-gateway-и-зачем-он-нужен)
+ [Что такое Service Discovery и как оно работает?](#что-такое-service-discovery-и-как-оно-работает)
+ [Как организовать конфигурацию микросервисов?](#как-организовать-конфигурацию-микросервисов)
+ [Что такое паттерн Circuit Breaker?](#что-такое-паттерн-circuit-breaker)
+ [Что такое паттерн Saga и как он решает проблему распределённых транзакций?](#что-такое-паттерн-saga-и-как-он-решает-проблему-распределённых-транзакций)
+ [Что такое Event Sourcing?](#что-такое-event-sourcing)
+ [Что такое CQRS?](#что-такое-cqrs)
+ [Что такое паттерн Outbox?](#что-такое-паттерн-outbox)
+ [Что такое паттерн Strangler Fig?](#что-такое-паттерн-strangler-fig)
+ [Как организовать распределённую трассировку в микросервисах?](#как-организовать-распределённую-трассировку-в-микросервисах)
+ [Как организовать централизованное логирование?](#как-организовать-централизованное-логирование)
+ [Как реализовать health checks и мониторинг микросервисов?](#как-реализовать-health-checks-и-мониторинг-микросервисов)
+ [Что такое идемпотентность и почему она важна в микросервисах?](#что-такое-идемпотентность-и-почему-она-важна-в-микросервисах)
+ [Как версионировать API микросервисов?](#как-версионировать-api-микросервисов)
+ [Как тестировать микросервисы?](#как-тестировать-микросервисы)
+ [Что такое 12-Factor App?](#что-такое-12-factor-app)
+ [Что такое паттерн Sidecar?](#что-такое-паттерн-sidecar)
+ [Что такое паттерн BFF (Backend for Frontend)?](#что-такое-паттерн-bff-backend-for-frontend)
+ [Database per Service или Shared Database -- какой подход выбрать?](#database-per-service-или-shared-database--какой-подход-выбрать)
+ [Что такое паттерны Retry и Timeout?](#что-такое-паттерны-retry-и-timeout)
+ [Что такое паттерн Bulkhead?](#что-такое-паттерн-bulkhead)
+ [Как обеспечить безопасность межсервисного взаимодействия?](#как-обеспечить-безопасность-межсервисного-взаимодействия)
+ [Что такое Service Mesh?](#что-такое-service-mesh)
+ [Как организовать деплой микросервисов?](#как-организовать-деплой-микросервисов)

## Что такое микросервисная архитектура?
<!-- grade: junior -->

Микросервисная архитектура — это подход к разработке программного обеспечения, при котором приложение строится как набор небольших, автономных сервисов, каждый из которых работает в собственном процессе, развёртывается независимо и взаимодействует с другими сервисами через чётко определённые API (обычно HTTP/REST, gRPC или через брокеры сообщений).

> Аналогия из жизни: микросервисы — это как отделы в банке. Кассовый зал, кредитный отдел и IT-отдел работают автономно, у каждого свои процессы и данные, но они обмениваются информацией через чётко определённые регламенты.

Каждый микросервис:
- Отвечает за одну бизнес-функцию — например, сервис платежей, сервис пользователей, сервис уведомлений.
- Имеет собственное хранилище данных — каждый сервис владеет своими данными и не обращается напрямую к базе другого сервиса.
- Развёртывается независимо — можно обновить один сервис, не затрагивая остальные.
- Может быть написан на любом стеке технологий — хотя в рамках одной организации обычно используют ограниченный набор (например, Java/Spring Boot).

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

> **На собеседовании:** интервьюер ожидает не только определение, но и понимание ключевых характеристик: автономность, собственное хранилище, независимый деплой. Частая ошибка — описывать микросервисы просто как «маленькие сервисы», не упомянув про владение данными и независимость развёртывания.

[к оглавлению](#микросервисы)

---

## Чем микросервисная архитектура отличается от монолитной?
<!-- grade: junior -->

Монолит — это единое приложение, в котором все компоненты (UI, бизнес-логика, доступ к данным) развёртываются как один артефакт. Микросервисы — набор отдельных приложений, каждое со своей кодовой базой и хранилищем.

| Критерий | Монолит | Микросервисы |
|---|---|---|
| Структура | Единое приложение, один деплой | Набор независимых сервисов |
| База данных | Одна общая БД | Отдельная БД у каждого сервиса |
| Развёртывание | Всё приложение целиком | Каждый сервис независимо |
| Масштабирование | Только целиком (vertical/horizontal) | Каждый сервис отдельно |
| Технологический стек | Единый для всего приложения | Может различаться |
| Отказоустойчивость | Ошибка может положить всё | Ошибка изолирована в одном сервисе |
| Размер команды | Вся команда работает с одной кодовой базой | Маленькие команды владеют отдельными сервисами |
| Сложность | Простая начальная разработка | Сложная инфраструктура с самого начала |
| Транзакции | Локальные ACID-транзакции | Распределённые (Saga, eventual consistency) |
| Тестирование | Простое интеграционное | Требует contract testing, E2E |

<details><summary>Пример кода: монолит vs микросервисы</summary>

```java
// Монолит: всё в одном приложении
@SpringBootApplication
public class BankingMonolithApplication {
    // Модуль платежей, модуль клиентов, модуль отчётов —
    // всё в одном JAR/WAR
}
```

```java
// Микросервисы: набор отдельных приложений
@SpringBootApplication
public class PaymentServiceApplication { }

@SpringBootApplication
public class CustomerServiceApplication { }
```

</details>

Микросервисы — это не «серебряная пуля». Монолит прекрасно работает для многих задач, и переход на микросервисы оправдан только при наличии реальных проблем масштабирования команды или системы.

> **На собеседовании:** покажите, что понимаете trade-offs обоих подходов. Частая ошибка — представлять монолит как устаревший антипаттерн. Хороший ответ: «монолит подходит для старта, микросервисы — когда появляется боль масштабирования».

[к оглавлению](#микросервисы)

---

## Какие преимущества у микросервисной архитектуры?
<!-- grade: junior -->

Основные преимущества микросервисов связаны с независимостью компонентов: независимое развёртывание, масштабирование, разработка и выбор технологий.

1. Независимое развёртывание — можно обновлять каждый сервис отдельно, без перезапуска всей системы.

2. Масштабирование отдельных компонентов — если сервис обработки платежей испытывает высокую нагрузку, можно масштабировать только его, не тратя ресурсы на масштабирование сервиса отчётов.

3. Технологическая гибкость — каждый сервис может использовать наиболее подходящий стек. Например, сервис аналитики может использовать Python, а основные сервисы — Java/Spring Boot.

4. Отказоустойчивость — сбой одного сервиса не приводит к падению всей системы (при правильной реализации паттернов устойчивости, таких как Circuit Breaker).

5. Параллельная разработка — разные команды могут работать над разными сервисами независимо, что ускоряет delivery.

6. Простота понимания — каждый сервис относительно небольшой и сфокусирован на конкретной бизнес-функции, что упрощает онбординг новых разработчиков.

7. Гибкость выбора хранилища данных (Polyglot Persistence) — для каждого сервиса можно выбрать оптимальное хранилище: PostgreSQL для транзакционных данных, MongoDB для документов, Redis для кеширования.

8. Лучшее соответствие организационной структуре (Закон Конвея) — структура системы отражает структуру организации.

> **На собеседовании:** недостаточно просто перечислить преимущества — важно привязать их к реальным бизнес-проблемам. Вместо «независимое масштабирование» лучше: «можем масштабировать сервис платежей в Black Friday, не тратя ресурсы на сервис отчётов».

[к оглавлению](#микросервисы)

---

## Какие недостатки у микросервисной архитектуры?
<!-- grade: junior -->

Главные недостатки микросервисов — это сложность распределённой системы: сетевые задержки, согласованность данных, операционная нагрузка и сложность отладки.

1. Сложность распределённой системы — приходится решать проблемы сетевых задержек, отказов сети, согласованности данных, что гораздо сложнее, чем вызов метода в монолите.

2. Сложность управления данными — невозможно использовать обычные ACID-транзакции между сервисами. Приходится применять паттерн Saga, eventual consistency, что значительно усложняет логику.

3. Операционная сложность — необходимо организовать мониторинг, логирование, трассировку для десятков или сотен сервисов. Нужна зрелая DevOps-культура и CI/CD.

4. Сетевые задержки — вызов между сервисами по сети значительно медленнее вызова метода в рамках одного процесса.

5. Сложность тестирования — интеграционное и E2E-тестирование становятся гораздо сложнее. Необходимо использовать contract testing (Pact), testcontainers, моки внешних сервисов.

6. Дублирование кода — некоторые общие функции (например, валидация, маппинг DTO) приходится дублировать в разных сервисах или выносить в shared-библиотеки, что создаёт связность.

7. Сложность отладки — один бизнес-процесс может проходить через 5-10 сервисов, и для отладки нужны инструменты распределённой трассировки.

8. Overhead на межсервисное взаимодействие — сериализация/десериализация данных, HTTP/gRPC-вызовы, брокеры сообщений добавляют накладные расходы.

9. Проблема «распределённого монолита» — при неправильной декомпозиции можно получить систему с недостатками и монолита, и микросервисов одновременно.

> **На собеседовании:** знание недостатков ценится даже выше, чем знание преимуществ — оно показывает реальный опыт. Частая ошибка — не упомянуть distributed monolith и проблему eventual consistency.

[к оглавлению](#микросервисы)

---

## Когда стоит использовать микросервисы, а когда нет?
<!-- grade: middle -->

Решение о переходе на микросервисы определяется масштабом системы, размером команды и зрелостью DevOps-культуры. Начинать новый проект с микросервисов почти всегда ошибочно.

### Стоит использовать, когда

- Система большая и сложная, разрабатывается несколькими командами.
- Требуется независимое масштабирование отдельных компонентов.
- Нужны разные циклы релизов для разных частей системы.
- Организация достаточно зрелая: есть CI/CD, мониторинг, опыт работы с распределёнными системами.
- Разные части системы имеют разные требования к доступности, нагрузке или технологиям.
- Команда разработки достаточно большая (обычно больше 20-30 человек).

### НЕ стоит использовать, когда

- Проект маленький или находится на ранней стадии — лучше начать с монолита (или «модульного монолита»).
- Команда маленькая (3-5 разработчиков) — накладные расходы на инфраструктуру съедят все преимущества.
- Домен недостаточно изучен — невозможно правильно определить границы сервисов.
- Нет DevOps-культуры и инфраструктуры — без CI/CD, мониторинга, оркестрации контейнеров микросервисы будут кошмаром.
- Жёсткие требования к транзакционной согласованности — если все данные должны быть строго консистентны, eventual consistency создаст серьёзные проблемы.

> Совет Мартина Фаулера: «Не начинайте с микросервисов. Начните с монолита, правильно структурируйте его по модулям, и когда почувствуете боль масштабирования — выделяйте микросервисы.»

Хорошей промежуточной стратегией является модульный монолит — единое приложение, но с чёткими границами модулей, которые потом можно выделить в отдельные сервисы:

```java
// Модульный монолит — чёткие границы модулей
// module-payment/src/main/java/com/bank/payment/PaymentModule.java
// module-customer/src/main/java/com/bank/customer/CustomerModule.java
// Модули взаимодействуют только через определённые интерфейсы
```

> **На собеседовании:** ключевой посыл — «it depends». Интервьюер хочет увидеть, что вы не фанатик микросервисов и понимаете, когда монолит лучше. Упомяните модульный монолит как промежуточный вариант.

[к оглавлению](#микросервисы)

---

## Какие принципы декомпозиции на микросервисы существуют?
<!-- grade: middle -->

Правильная декомпозиция — ключ к успешной микросервисной архитектуре. Она определяет границы сервисов так, чтобы минимизировать связность и максимизировать автономность каждого сервиса.

### 1. Single Responsibility Principle (SRP)

Каждый сервис отвечает за одну бизнес-способность (business capability). Например: сервис платежей, сервис уведомлений, сервис аутентификации.

### 2. Domain-Driven Design (DDD)

Декомпозиция по ограниченным контекстам (Bounded Contexts). Это самый эффективный подход:
- Проведите Event Storming с бизнес-экспертами
- Определите домены и поддомены
- Выделите Bounded Contexts
- Каждый Bounded Context — кандидат на микросервис

### 3. Высокая связность (High Cohesion)

Всё, что относится к одной бизнес-функции, должно быть в одном сервисе.

### 4. Слабая связанность (Loose Coupling)

Изменение одного сервиса не должно требовать изменения других. Сервисы взаимодействуют только через API.

### 5. Размер команды (правило «двух пицц» Безоса)

Один сервис должен поддерживаться командой, которую можно накормить двумя пиццами (5-9 человек).

### 6. Декомпозиция по бизнес-подобластям (Subdomains)

- Core domain — конкурентное преимущество (например, скоринговая модель)
- Supporting subdomain — поддерживающие функции (управление клиентами)
- Generic subdomain — типовые функции (уведомления, аутентификация)

### Антипаттерны декомпозиции

- Декомпозиция по техническим слоям (сервис для DAO, сервис для бизнес-логики) — это плохо.
- Слишком мелкие сервисы (nano-сервисы) — чрезмерный overhead.
- Распределённый монолит — сервисы жёстко связаны и деплоятся вместе.

> **На собеседовании:** самый ценный ответ — DDD и Bounded Context как основа декомпозиции. Обязательно упомяните антипаттерны, особенно «распределённый монолит» — это показывает реальный опыт.

[к оглавлению](#микросервисы)

---

## Что такое Bounded Context и как он связан с микросервисами?
<!-- grade: middle -->

Bounded Context (ограниченный контекст) — это центральная концепция Domain-Driven Design, обозначающая границу, внутри которой определённая модель предметной области является согласованной и единообразной.

> Аналогия из жизни: в разных отделах банка слово «клиент» означает разное. Для отдела продаж — это лид с контактами, для кредитного — заёмщик со скоринговым баллом, для платёжного — владелец счёта. Каждый отдел — это свой Bounded Context.

В разных контекстах одно и то же понятие может иметь разное значение. Например, «Клиент» в банковской системе:
- В контексте продаж — это лид с контактными данными и историей взаимодействий.
- В контексте кредитования — это заёмщик с кредитной историей и скоринговым баллом.
- В контексте платежей — это владелец счёта с реквизитами.

Каждый контекст имеет свою модель «Клиента», и это нормально. Попытка создать единую модель для всех контекстов приводит к «Большому комку грязи» (Big Ball of Mud).

Связь с микросервисами: один Bounded Context, как правило, соответствует одному микросервису (или группе тесно связанных сервисов).

<details><summary>Пример кода: разные модели клиента в разных контекстах</summary>

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

</details>

### Context Map

Context Map — диаграмма, показывающая взаимосвязи между контекстами:
- Shared Kernel — общий код между контекстами (минимизировать!).
- Customer-Supplier — один контекст является потребителем данных другого.
- Anti-Corruption Layer (ACL) — слой адаптации между контекстами, предотвращающий загрязнение модели чужими концепциями.
- Published Language — формальный язык обмена данными (например, события в Kafka).

> **На собеседовании:** связка «Bounded Context = микросервис» — это ключевой инсайт. Также упомяните Anti-Corruption Layer — он показывает, что вы понимаете, как защитить свою модель от чужих абстракций.

[к оглавлению](#микросервисы)

---

## Какие способы взаимодействия микросервисов существуют?
<!-- grade: middle -->

Взаимодействие микросервисов делится на два основных типа: синхронное (Request-Response) и асинхронное (Event-Driven).

### Синхронное взаимодействие (Request-Response)

- REST (HTTP) — наиболее распространённый способ. Простой, основан на стандартах HTTP.
- gRPC — бинарный протокол на основе Protocol Buffers. Быстрее REST, поддерживает стриминг.
- GraphQL — гибкие запросы, клиент сам определяет структуру ответа.

### Асинхронное взаимодействие (Event-Driven)

- Message Brokers (Kafka, RabbitMQ) — сервис публикует события, подписчики обрабатывают.
- Event Streaming (Apache Kafka) — поток событий с возможностью повторного чтения.

| Критерий | Синхронное | Асинхронное |
|---|---|---|
| Связность | Временная связность (оба сервиса должны быть доступны) | Слабая связность |
| Latency | Суммируется по всей цепочке | Не блокирует вызывающую сторону |
| Сложность | Простая реализация | Сложнее (idempotency, ordering) |
| Отладка | Проще (request-response) | Сложнее (асинхронные цепочки) |
| Надёжность | Ниже (cascade failures) | Выше (буферизация в брокере) |

### Рекомендации

- Для операций, требующих немедленного ответа (запрос баланса) — синхронное взаимодействие.
- Для операций, допускающих задержку (отправка уведомления после платежа) — асинхронное.
- В банковских системах предпочтительно асинхронное взаимодействие, так как оно обеспечивает лучшую отказоустойчивость.

```
Синхронное:          Асинхронное:
A ──REST──► B        A ──event──► Kafka ──event──► B
A ◄─resp.── B                                     (A не ждёт B)
```

> **На собеседовании:** покажите, что понимаете, когда какой способ применять. Ключевой критерий — нужен ли немедленный ответ. Частая ошибка — забыть упомянуть temporal coupling в синхронном взаимодействии.

[к оглавлению](#микросервисы)

---

## Как организовать синхронное взаимодействие микросервисов через REST?
<!-- grade: middle -->

В Spring Boot для синхронного взаимодействия между микросервисами используются HTTP-клиенты: RestTemplate (устаревший), WebClient (рекомендуемый), Declarative HTTP Client (Spring 6+) и OpenFeign.

<details><summary>1. RestTemplate (устаревший, но ещё встречается)</summary>

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

</details>

<details><summary>2. WebClient (реактивный, рекомендуется)</summary>

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

</details>

<details><summary>3. Declarative HTTP Client (Spring 6+ / Spring Boot 3+)</summary>

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

</details>

<details><summary>4. OpenFeign (Spring Cloud)</summary>

```java
@FeignClient(name = "customer-service", url = "${customer-service.url}")
public interface CustomerClient {

    @GetMapping("/api/customers/{id}")
    CustomerDto getCustomer(@PathVariable("id") Long id);
}
```

</details>

### Важные моменты

- Всегда устанавливайте таймауты (connect timeout, read timeout).
- Используйте Circuit Breaker для защиты от каскадных сбоев.
- Применяйте retry с exponential backoff для transient errors.
- При использовании Service Discovery имена сервисов резолвятся автоматически.

> **На собеседовании:** знайте эволюцию: RestTemplate -> WebClient -> Declarative HTTP Client. В Spring Boot 3+ рекомендуется Declarative HTTP Client или WebClient. Feign остаётся в Spring Cloud-экосистеме.

[к оглавлению](#микросервисы)

---

## Что такое gRPC и когда его стоит использовать вместо REST?
<!-- grade: middle -->

gRPC (gRPC Remote Procedure Call) — это высокопроизводительный фреймворк для удалённого вызова процедур, использующий Protocol Buffers для сериализации и HTTP/2 как транспортный протокол.

### Ключевые особенности

- Бинарный формат — protobuf компактнее JSON, что снижает нагрузку на сеть.
- HTTP/2 — мультиплексирование, сжатие заголовков, двунаправленный стриминг.
- Контракт через .proto файл — строгая типизация, автоматическая генерация кода.
- Четыре типа взаимодействия: Unary, Server Streaming, Client Streaming, Bidirectional Streaming.

<details><summary>Пример .proto файла и реализации сервера</summary>

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

</details>

| Критерий | gRPC | REST |
|---|---|---|
| Формат данных | Protobuf (бинарный) | JSON (текстовый) |
| Протокол | HTTP/2 | HTTP/1.1 или HTTP/2 |
| Контракт | .proto файл (обязательный) | OpenAPI/Swagger (опционально) |
| Стриминг | Нативный (4 типа) | Ограниченный (SSE, WebSocket) |
| Читаемость | Нет (бинарный) | Да (JSON) |
| Кодогенерация | Автоматическая | Опциональная |
| Браузер | gRPC-Web (ограниченно) | Нативная поддержка |

### Когда использовать gRPC

- Высоконагруженное межсервисное взаимодействие внутри кластера.
- Требуется стриминг данных.
- Критична производительность и размер пакетов.
- Строгая типизация контрактов важнее «человекочитаемости».

### Когда оставить REST

- Публичное API для внешних потребителей.
- Простые CRUD-операции.
- Нужна простая отладка (JSON читаем, protobuf — нет).
- Браузерные клиенты.

> **На собеседовании:** ключевой вопрос — «когда что выбрать». REST для внешних API и простоты, gRPC для внутреннего межсервисного взаимодействия с высокой нагрузкой. Упомяните HTTP/2 и стриминг как основные преимущества gRPC.

[к оглавлению](#микросервисы)

---

## Как организовать асинхронное взаимодействие через брокеры сообщений?
<!-- grade: middle -->

Асинхронное взаимодействие позволяет сервисам обмениваться данными без ожидания немедленного ответа. Два основных брокера: Apache Kafka (распределённый лог событий) и RabbitMQ (классическая очередь сообщений).

<details><summary>Apache Kafka: продюсер и консьюмер</summary>

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
            ProducerConfig.ACKS_CONFIG, "all",
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
        notificationService.sendPaymentNotification(event);
    }
}
```

</details>

<details><summary>RabbitMQ: продюсер, консьюмер и конфигурация</summary>

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

</details>

### Kafka vs RabbitMQ

| Критерий | Kafka | RabbitMQ |
|---|---|---|
| Модель | Распределённый лог | Очередь сообщений |
| Хранение | Сообщения хранятся после прочтения | Удаляются после подтверждения |
| Пропускная способность | Очень высокая (миллионы msg/sec) | Высокая (десятки тысяч msg/sec) |
| Порядок | Гарантирован в рамках партиции | Гарантирован в рамках очереди |
| Маршрутизация | По топикам и партициям | Гибкая (direct, topic, fanout, headers) |
| Применение | Event streaming, event sourcing | Task queues, RPC, маршрутизация |

> **На собеседовании:** ключевое различие — Kafka хранит сообщения после прочтения (лог), а RabbitMQ удаляет после подтверждения (очередь). Kafka — для event-driven архитектуры и event sourcing, RabbitMQ — для task queues и гибкой маршрутизации.

[к оглавлению](#микросервисы)

---

## Что такое API Gateway и зачем он нужен?
<!-- grade: middle -->

API Gateway — это единая точка входа для всех клиентских запросов к микросервисам. Он выступает обратным прокси, маршрутизирует запросы к соответствующим сервисам и выполняет сквозные функции: аутентификацию, rate limiting, логирование.

### Функции API Gateway

- Маршрутизация запросов — направление запросов к нужному сервису по URL-пути.
- Аутентификация и авторизация — проверка токенов (JWT, OAuth2) на входе.
- Rate limiting — ограничение количества запросов.
- Load balancing — балансировка нагрузки между экземплярами сервиса.
- Агрегация запросов — объединение ответов от нескольких сервисов.
- Кеширование — кеширование частых запросов.
- Трансформация запросов/ответов — изменение формата данных.
- SSL termination — обработка HTTPS.
- Логирование и мониторинг — централизованный сбор метрик.

<details><summary>Spring Cloud Gateway: YAML-конфигурация</summary>

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

</details>

<details><summary>Spring Cloud Gateway: программная конфигурация</summary>

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

</details>

### Альтернативные решения

- Kong — высокопроизводительный API Gateway на базе Nginx, с плагинами.
- NGINX — может использоваться как простой API Gateway.
- AWS API Gateway — управляемый сервис в облаке AWS.
- Envoy — часто используется в связке с Service Mesh (Istio).

> **На собеседовании:** API Gateway — это не просто «прокси». Покажите, что понимаете его роль в безопасности (JWT-валидация на входе), устойчивости (circuit breaker, rate limiting) и наблюдаемости (централизованное логирование). Частая ошибка — не упомянуть, что Gateway может стать единой точкой отказа.

[к оглавлению](#микросервисы)

---

## Что такое Service Discovery и как оно работает?
<!-- grade: middle -->

Service Discovery — это механизм, позволяющий микросервисам автоматически находить друг друга в сети без жёсткого указания IP-адресов и портов.

> Аналогия из жизни: Service Discovery — это как телефонный справочник. Вместо того чтобы помнить номер каждого отдела, вы звоните на ресепшен (реестр), называете имя отдела и получаете актуальный номер.

### Два подхода

| Подход | Как работает | Пример |
|---|---|---|
| Client-Side Discovery | Клиент запрашивает реестр и сам выбирает экземпляр | Netflix Eureka |
| Server-Side Discovery | Запрос идёт через балансировщик, который обращается к реестру | Kubernetes DNS, AWS ELB |

<details><summary>Netflix Eureka (Client-Side Discovery)</summary>

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

</details>

### Consul

Consul — Service Discovery + Key-Value Store + Health Checks. Более зрелое решение для production:

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

### Kubernetes DNS

Если микросервисы работают в Kubernetes, Service Discovery встроено:

```yaml
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

> **На собеседовании:** покажите, что знаете оба подхода (client-side и server-side), и что в Kubernetes Service Discovery встроено через DNS. Частая ошибка — рассказывать только про Eureka, не упомянув Kubernetes.

[к оглавлению](#микросервисы)

---

## Как организовать конфигурацию микросервисов?
<!-- grade: middle -->

Централизованная конфигурация позволяет управлять настройками всех микросервисов из одного места и менять их без переразвёртывания. Основные инструменты: Spring Cloud Config, Consul KV, HashiCorp Vault, Kubernetes ConfigMaps.

<details><summary>Spring Cloud Config Server</summary>

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

</details>

### Consul KV Store

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

### HashiCorp Vault -- для секретов

Vault предназначен для безопасного хранения секретов (пароли БД, API-ключи, сертификаты).

<details><summary>Конфигурация Vault</summary>

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

</details>

### Kubernetes ConfigMaps и Secrets

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

### Динамическое обновление конфигурации (без перезапуска)

```java
@RefreshScope
@RestController
public class PaymentController {

    @Value("${payment.max-amount}")
    private BigDecimal maxAmount; // Обновится при POST /actuator/refresh
}
```

Для массового обновления используется Spring Cloud Bus с Kafka/RabbitMQ — один POST-запрос обновляет конфигурацию всех экземпляров сервиса.

> **На собеседовании:** упомяните три уровня: конфигурация (Config Server), секреты (Vault), динамическое обновление (@RefreshScope). Частая ошибка — хранить секреты в Git-репозитории конфигурации вместо Vault.

[к оглавлению](#микросервисы)

---

## Что такое паттерн Circuit Breaker?
<!-- grade: middle -->

Circuit Breaker (автоматический выключатель) — это паттерн отказоустойчивости, который предотвращает каскадные сбои в распределённой системе, «размыкая цепь» при обнаружении проблем с вызываемым сервисом.

> Аналогия из жизни: работает как электрический автомат в квартире. При коротком замыкании (сбое сервиса) автомат срабатывает и отключает линию, чтобы не сгорела вся проводка (остальные сервисы).

### Три состояния

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

- CLOSED — запросы проходят нормально. Счётчик ошибок ведётся.
- OPEN — все запросы мгновенно возвращают fallback-ответ, не дожидаясь таймаута.
- HALF-OPEN — пропускается ограниченное число пробных запросов. Если они успешны — CLOSED, иначе — OPEN.

<details><summary>Реализация с Resilience4j</summary>

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

</details>

> **На собеседовании:** обязательно назовите три состояния (CLOSED, OPEN, HALF-OPEN) и объясните переходы между ними. Упомяните Resilience4j как замену устаревшего Hystrix. Частая ошибка — забыть про fallback-стратегию.

[к оглавлению](#микросервисы)

---

## Что такое паттерн Saga и как он решает проблему распределённых транзакций?
<!-- grade: senior -->

Saga — это паттерн управления распределёнными транзакциями, при котором длинная бизнес-транзакция разбивается на последовательность локальных транзакций в разных сервисах. При сбое выполняются компенсирующие транзакции для отмены уже выполненных шагов.

> Аналогия из жизни: бронирование путешествия. Вы бронируете авиабилет, затем отель, затем экскурсию. Если экскурсию забронировать не удалось — вы отменяете отель и авиабилет (компенсирующие действия).

### Проблема

В микросервисах нельзя использовать распределённые ACID-транзакции (2PC — Two-Phase Commit), так как они плохо масштабируются и создают сильную связность.

### Два подхода к реализации

| Критерий | Хореография | Оркестрация |
|---|---|---|
| Координация | Каждый сервис сам публикует/слушает события | Центральный оркестратор управляет шагами |
| Связность | Сервисы слабо связаны | Оркестратор знает обо всех шагах |
| Единая точка отказа | Нет | Оркестратор |
| Видимость процесса | Сложно отследить | Легко |
| Рекомендации | 3-4 шага | 5+ шагов |

<details><summary>Хореография (Choreography)</summary>

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

</details>

<details><summary>Оркестрация (Orchestration)</summary>

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
            compensate(state);
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

</details>

> **На собеседовании:** знайте оба подхода и когда какой выбрать. Обязательно упомяните компенсирующие транзакции — это ядро паттерна. Частая ошибка — путать Saga с 2PC (двухфазным коммитом).

[к оглавлению](#микросервисы)

---

## Что такое Event Sourcing?
<!-- grade: senior -->

Event Sourcing — это паттерн хранения данных, при котором состояние объекта определяется не текущим снимком (snapshot), а последовательностью доменных событий, которые произошли с этим объектом.

> Аналогия из жизни: банковская выписка. Вместо хранения только текущего баланса хранится вся история операций. Баланс в любой момент можно вычислить, «проиграв» все транзакции с начала.

Вместо хранения «баланс счёта = 1000 руб.» хранится:
```
1. AccountOpened(accountId=123, initialBalance=0)
2. MoneyDeposited(accountId=123, amount=5000)
3. MoneyWithdrawn(accountId=123, amount=2000)
4. MoneyDeposited(accountId=123, amount=500)
5. MoneyWithdrawn(accountId=123, amount=2500)
→ Текущий баланс = 0 + 5000 - 2000 + 500 - 2500 = 1000
```

### Преимущества

- Полный аудит — каждое изменение зафиксировано. Критически важно для банков.
- Возможность воспроизвести состояние на любой момент времени.
- Отладка — можно воспроизвести проблему, повторив события.
- Event-driven архитектура — события естественно интегрируются с другими сервисами.
- Нет конфликтов при записи — запись только в append-only лог.

### Недостатки

- Сложность — непривычная модель, кривая обучения.
- Восстановление состояния — при большом количестве событий нужны снапшоты.
- Eventual consistency — проекции могут отставать.
- Миграция событий — изменение формата события требует стратегии миграции.

<details><summary>Пример реализации: агрегат и хранилище событий</summary>

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

</details>

Для оптимизации производительности при большом количестве событий используются снапшоты — периодическое сохранение текущего состояния, чтобы не проигрывать все события с начала.

> **На собеседовании:** подчеркните, что Event Sourcing — это не только «хранить события», а ещё и восстановление состояния из событий, снапшоты для производительности и natural fit с CQRS. Частая ошибка — забыть про проблему миграции формата событий.

[к оглавлению](#микросервисы)

---

## Что такое CQRS?
<!-- grade: senior -->

CQRS (Command Query Responsibility Segregation) — это паттерн, разделяющий модели чтения и записи данных. Команды (Commands) изменяют состояние, запросы (Queries) читают данные. Для каждой задачи используется своя оптимизированная модель.

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

### Зачем нужен CQRS

- Разная оптимизация — модель записи нормализована (3NF), модель чтения денормализована для быстрых запросов.
- Масштабирование — чтений обычно гораздо больше, чем записей (соотношение 100:1). Можно масштабировать read-модель отдельно.
- Разные хранилища — запись в PostgreSQL, чтение из Elasticsearch или Redis.
- Естественно сочетается с Event Sourcing — события из write-модели проецируются в read-модель.

<details><summary>Пример реализации CQRS</summary>

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
        }
    }
}
```

</details>

CQRS добавляет сложность. Используйте его только когда есть реальная необходимость:
- Существенная разница в нагрузке чтение/запись
- Разные требования к моделям чтения и записи
- Необходимость поддержки сложных запросов (полнотекстовый поиск, агрегации)

> **На собеседовании:** объясните связку CQRS + Event Sourcing: события из write-модели проецируются в read-модель. Обязательно упомяните eventual consistency как следствие разделения моделей. Частая ошибка — предлагать CQRS для простых CRUD-приложений.

[к оглавлению](#микросервисы)

---

## Что такое паттерн Outbox?
<!-- grade: senior -->

Outbox (Transactional Outbox) — это паттерн, решающий проблему атомарности при необходимости одновременно обновить базу данных и опубликовать событие в брокер сообщений.

### Проблема

Нельзя атомарно выполнить запись в БД и отправку в Kafka. Возможны ситуации:
- Запись в БД прошла, отправка в Kafka — нет (потеря события).
- Отправка в Kafka прошла, запись в БД — нет (фантомное событие).

### Решение

Событие записывается в специальную таблицу `outbox` в той же транзакции, что и бизнес-данные. Отдельный процесс читает эту таблицу и публикует события в брокер.

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

<details><summary>Реализация Outbox паттерна</summary>

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

</details>

### Альтернативный подход -- Debezium CDC (Change Data Capture)

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

> **На собеседовании:** объясните проблему dual write (БД + брокер) и как Outbox её решает через единую транзакцию. Упомяните два способа публикации: polling и CDC (Debezium). Частая ошибка — не знать про Debezium как production-ready альтернативу polling.

[к оглавлению](#микросервисы)

---

## Что такое паттерн Strangler Fig?
<!-- grade: middle -->

Strangler Fig (душитель) — это паттерн постепенной миграции от монолита к микросервисам. Вместо рискованного «Big Bang» переписывания, новая функциональность реализуется в микросервисах, а существующая постепенно перемещается из монолита.

> Аналогия из жизни: название происходит от тропического фикуса-душителя, который обвивает другое дерево и постепенно замещает его. Монолит — это старое дерево, микросервисы — новые побеги.

### Этапы миграции

```
Этап 1: Монолит обрабатывает все запросы
┌──────────┐
│ Клиенты  │──► Монолит
└──────────┘

Этап 2: Фасад перенаправляет часть запросов
┌──────────┐    ┌─────────┐    ┌──────────────┐
│ Клиенты  │──► │ Gateway │──► │   Монолит    │ (старые функции)
└──────────┘    └────┬────┘    └──────────────┘
                     └──────► Микросервис A      (мигрированные)

Этап 3: Большинство функций мигрировано
┌──────────┐    ┌─────────┐    ┌──────────────┐
│ Клиенты  │──► │ Gateway │──► │   Монолит    │ (минимум)
└──────────┘    └────┬────┘    └──────────────┘
                     ├──────► Микросервис A
                     ├──────► Микросервис B
                     └──────► Микросервис C

Этап 4: Монолит полностью заменён
```

<details><summary>Реализация через Spring Cloud Gateway</summary>

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

</details>

### Стратегии выделения микросервисов из монолита

1. Новая функциональность — всё новое пишем как микросервис.
2. По бизнес-домену — выделяем наиболее изолированные модули.
3. По частоте изменений — выделяем то, что часто меняется.
4. По требованиям к масштабированию — выделяем «горячие» модули.

### Anti-Corruption Layer

Anti-Corruption Layer — слой-адаптер между микросервисом и монолитом, который транслирует модели и предотвращает загрязнение новой чистой модели старыми структурами.

<details><summary>Пример Anti-Corruption Layer</summary>

```java
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

</details>

> **На собеседовании:** ключевой посыл — постепенность и безопасность миграции. Упомяните API Gateway как фасад и Anti-Corruption Layer для защиты модели. Частая ошибка — забыть про ACL и допустить «утечку» старых абстракций в новые сервисы.

[к оглавлению](#микросервисы)

---

## Как организовать распределённую трассировку в микросервисах?
<!-- grade: middle -->

Distributed Tracing (распределённая трассировка) — это метод отслеживания прохождения запроса через цепочку микросервисов, позволяющий понять, какие сервисы были вызваны, сколько времени занял каждый вызов и где произошла ошибка.

### Основные понятия

- Trace — полный путь запроса через все сервисы (от входа до ответа).
- Span — одна операция в рамках trace (например, вызов одного сервиса или запрос к БД).
- Trace ID — уникальный идентификатор всего запроса (передаётся между сервисами).
- Span ID — идентификатор конкретной операции.
- Parent Span ID — ссылка на родительский span (для построения дерева вызовов).

```
Trace ID: abc123
├── Span 1: API Gateway (50ms)
│   ├── Span 2: Payment Service (30ms)
│   │   ├── Span 3: DB Query (5ms)
│   │   └── Span 4: Customer Service call (20ms)
│   │       └── Span 5: DB Query (3ms)
│   └── Span 6: Notification Service (10ms)
```

<details><summary>Micrometer Tracing + Zipkin (Spring Boot 3+)</summary>

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

</details>

<details><summary>Создание кастомных span</summary>

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

</details>

### Инструменты визуализации

- Zipkin — простой и легковесный, хорошо интегрируется со Spring.
- Jaeger — более мощный, поддерживает сэмплирование, создан Uber.
- Grafana Tempo — интеграция с Grafana, поддержка больших объёмов.

> **На собеседовании:** объясните понятия Trace и Span, покажите, что знаете Micrometer Tracing как замену Spring Cloud Sleuth (устарел в Spring Boot 3). Частая ошибка — не упомянуть sampling в production (100% трассировка создаёт огромную нагрузку).

[к оглавлению](#микросервисы)

---

## Как организовать централизованное логирование?
<!-- grade: middle -->

Централизованное логирование — это сбор логов со всех микросервисов в одном месте для удобного поиска и анализа. Основные стеки: ELK (Elasticsearch + Logstash + Kibana) и Grafana Loki.

```
Микросервисы → Logstash (или Filebeat) → Elasticsearch → Kibana
```

<details><summary>Настройка структурированного логирования (Logback + JSON)</summary>

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

</details>

### Grafana Loki -- альтернатива ELK

Loki не индексирует содержимое логов, а только метки (labels), что делает его значительно дешевле и проще в эксплуатации.

### Рекомендации по логированию в микросервисах

- Всегда включайте traceId — для связывания логов одного запроса.
- Используйте структурированные логи (JSON) — для удобного поиска и фильтрации.
- Логируйте бизнес-события — не только ошибки, но и важные бизнес-действия.
- Не логируйте чувствительные данные — номера карт, пароли, персональные данные (PCI DSS!).
- Используйте уровни логирования правильно: ERROR — сбои, WARN — нештатные ситуации, INFO — бизнес-события, DEBUG — для отладки (отключено в prod).

<details><summary>Пример правильного логирования</summary>

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

</details>

> **На собеседовании:** покажите, что знаете стек (ELK или Loki), и что логи должны быть структурированными (JSON) с traceId. Частая ошибка — забыть про маскирование чувствительных данных в логах.

[к оглавлению](#микросервисы)

---

## Как реализовать health checks и мониторинг микросервисов?
<!-- grade: middle -->

Health checks и мониторинг — критические аспекты production-ready микросервисов. Spring Boot Actuator предоставляет готовые эндпоинты для проверки здоровья, а Prometheus + Grafana — для сбора и визуализации метрик.

<details><summary>Spring Boot Actuator: конфигурация и кастомный HealthIndicator</summary>

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

</details>

### Liveness и Readiness Probes (для Kubernetes)

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

- `/actuator/health/liveness` — жив ли сервис (если нет — Kubernetes перезапустит контейнер).
- `/actuator/health/readiness` — готов ли сервис принимать трафик (если нет — Kubernetes перестанет направлять трафик).

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

<details><summary>Prometheus + Grafana: кастомные метрики</summary>

```java
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

</details>

### Ключевые метрики для мониторинга (RED-метрики)

| Метрика | Описание |
|---|---|
| Rate | Количество запросов в секунду |
| Errors | Процент ошибок |
| Duration | Время ответа (p50, p95, p99) |

> **На собеседовании:** различайте liveness и readiness probes — это частый вопрос. Liveness: «жив ли процесс» (перезапуск при false), readiness: «готов ли принимать трафик» (отключение от балансировщика при false). Упомяните RED-метрики как стандарт мониторинга.

[к оглавлению](#микросервисы)

---

## Что такое идемпотентность и почему она важна в микросервисах?
<!-- grade: middle -->

Идемпотентность — это свойство операции, при котором многократное выполнение даёт тот же результат, что и однократное. В микросервисах это гарантирует, что повторная отправка запроса не приводит к дублированию побочных эффектов.

> Аналогия из жизни: нажатие кнопки «Вызвать лифт» — сколько бы раз вы ни нажали, лифт приедет один раз. Повторное нажатие не вызывает второй лифт.

### Почему это критично

- Сетевые сбои: клиент отправил запрос, не получил ответ (таймаут) и повторяет.
- Retry-механизмы: автоматический retry при ошибке может отправить запрос повторно.
- Kafka: при ребалансировке потребитель может обработать сообщение дважды (at-least-once delivery).

### Идемпотентность HTTP-методов

| Метод | Идемпотентный? | Пояснение |
|---|---|---|
| GET | Да | Чтение не меняет состояние |
| PUT | Да | Полная замена ресурса |
| DELETE | Да | Повторное удаление = нет эффекта |
| POST | Нет | Создание ресурса — требует дополнительных мер |

<details><summary>Реализация через Idempotency Key</summary>

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
            return ResponseEntity.ok(existing.get());
        }

        // 2. Обрабатываем платёж
        Payment payment = executePayment(request);
        PaymentResponse response = toResponse(payment);

        // 3. Сохраняем результат с TTL
        idempotencyStore.save(idempotencyKey, response, Duration.ofHours(24));

        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
}
```

</details>

<details><summary>Идемпотентность Kafka-консьюмера</summary>

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

</details>

> **На собеседовании:** объясните через конкретный пример: «если POST /payments выполняется дважды — деньги не должны списаться дважды». Покажите решение через Idempotency Key + Redis. Частая ошибка — забыть про идемпотентность Kafka-консьюмеров.

[к оглавлению](#микросервисы)

---

## Как версионировать API микросервисов?
<!-- grade: middle -->

Версионирование API необходимо для обратной совместимости — чтобы обновление одного сервиса не ломало потребителей его API. Три основных подхода: версия в URL, в заголовке и через Content-Type.

| Подход | Пример | Плюсы | Минусы |
|---|---|---|---|
| URL | `/api/v1/payments` | Простой, наглядный, кешируемый | «Загрязняет» URL |
| Header | `X-API-Version: 1` | Чистый URL | Сложнее тестировать и документировать |
| Content-Type | `Accept: application/vnd.bank.v1+json` | Следует HTTP-стандартам | Самый сложный |

<details><summary>Примеры реализации всех подходов</summary>

```java
// 1. Версия в URL (наиболее распространённый)
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
        return paymentService.getPaymentV2(id);
    }
}

// 2. Версия в заголовке
@GetMapping(value = "/api/payments/{id}", headers = "X-API-Version=1")
public PaymentResponseV1 getPaymentV1(@PathVariable Long id) { ... }

@GetMapping(value = "/api/payments/{id}", headers = "X-API-Version=2")
public PaymentResponseV2 getPaymentV2(@PathVariable Long id) { ... }

// 3. Версия через Accept header (Content Negotiation)
@GetMapping(value = "/api/payments/{id}",
            produces = "application/vnd.bank.payment.v1+json")
public PaymentResponseV1 getPaymentV1(@PathVariable Long id) { ... }
```

</details>

### Правила эволюции API (обратная совместимость)

- Добавление нового поля в ответ — безопасно (клиенты игнорируют неизвестные поля).
- Удаление обязательного поля — НЕ безопасно, требует новой версии.
- Изменение типа поля — НЕ безопасно.
- Добавление нового опционального параметра запроса — безопасно.
- Используйте `@JsonIgnoreProperties(ignoreUnknown = true)` на DTO-клиентах для толерантности к новым полям.

### Стратегия deprecation

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

> **На собеседовании:** рекомендуйте версионирование через URL как самый простой и распространённый подход. Обязательно упомяните правила обратной совместимости и Sunset-заголовок для deprecated API.

[к оглавлению](#микросервисы)

---

## Как тестировать микросервисы?
<!-- grade: middle -->

Тестирование микросервисов строится на основе пирамиды тестирования, адаптированной для распределённой архитектуры: unit-тесты (много), component-тесты, contract-тесты (между сервисами), integration-тесты и E2E-тесты (мало).

```
         ╱  E2E тесты  ╲         — мало (дорогие, хрупкие)
        ╱ Integration    ╲        — средне
       ╱ Contract tests   ╲       — много (между сервисами)
      ╱ Component tests    ╲      — средне (один сервис целиком)
     ╱ Unit tests           ╲     — очень много (быстрые)
```

<details><summary>1. Unit-тесты: тестирование бизнес-логики</summary>

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

</details>

<details><summary>2. Component-тесты: один сервис с поднятым контекстом и тестовой БД</summary>

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

</details>

<details><summary>3. Contract Testing с Pact</summary>

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

</details>

<details><summary>4. Integration-тесты с WireMock</summary>

```java
@SpringBootTest
@AutoConfigureWireMock(port = 0)
class PaymentServiceIntegrationTest {

    @Test
    void shouldCallCustomerServiceAndProcessPayment() {
        stubFor(get(urlEqualTo("/api/customers/1"))
            .willReturn(aResponse()
                .withHeader("Content-Type", "application/json")
                .withBody("""
                    {"id": 1, "name": "Иванов", "status": "ACTIVE"}
                    """)));

        PaymentResponse result = paymentService.processPayment(
            new PaymentRequest(1L, BigDecimal.valueOf(5000)));

        assertThat(result.getStatus()).isEqualTo("COMPLETED");
        verify(getRequestedFor(urlEqualTo("/api/customers/1")));
    }
}
```

</details>

> **На собеседовании:** ключевое отличие от монолита — contract testing. Именно оно гарантирует, что API между сервисами не рассинхронизируется. Упомяните Pact и пирамиду тестов. Частая ошибка — пытаться покрыть всё E2E-тестами вместо contract-тестов.

[к оглавлению](#микросервисы)

---

## Что такое 12-Factor App?
<!-- grade: middle -->

12-Factor App — это методология разработки SaaS-приложений, описанная инженерами Heroku, определяющая 12 принципов создания облачных приложений. Особенно актуальна для микросервисов.

| Фактор | Принцип | Пример в Spring Boot |
|---|---|---|
| 1. Codebase | Один репозиторий, много деплоев | Git repo -> dev, staging, prod |
| 2. Dependencies | Явное объявление зависимостей | pom.xml, build.gradle |
| 3. Config | Конфигурация в переменных окружения | `@Value("${DATABASE_URL}")` |
| 4. Backing Services | БД, брокеры — подключаемые ресурсы | Смена БД через конфиг |
| 5. Build, Release, Run | Разделение этапов | Maven build -> Docker image -> Run |
| 6. Processes | Stateless-процессы | Состояние в Redis/БД |
| 7. Port Binding | Самодостаточность через порт | Встроенный Tomcat/Netty |
| 8. Concurrency | Горизонтальное масштабирование | Запуск дополнительных реплик |
| 9. Disposability | Быстрый запуск, graceful shutdown | `server.shutdown: graceful` |
| 10. Dev/Prod Parity | Минимальные различия окружений | Docker, Testcontainers |
| 11. Logs | Логи в stdout | Сбор через Docker/K8s |
| 12. Admin Processes | Одноразовые задачи в том же окружении | Flyway-миграции |

### Ключевые примеры

```java
// Фактор 3: Config — конфигурация через переменные окружения
@Value("${DATABASE_URL}")
private String databaseUrl;

// НЕ правильно:
// private String databaseUrl = "jdbc:postgresql://prod-db:5432/payments";
```

```yaml
# Фактор 9: Disposability — graceful shutdown
server:
  shutdown: graceful
spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
```

> **На собеседовании:** не нужно перечислять все 12 факторов. Выделите 3-4 самых важных (Config, Processes/Statelessness, Disposability, Logs) и объясните их на примерах. Покажите, что Spring Boot «из коробки» соответствует большинству факторов.

[к оглавлению](#микросервисы)

---

## Что такое паттерн Sidecar?
<!-- grade: middle -->

Sidecar — это паттерн развёртывания, при котором вспомогательный контейнер работает рядом с основным сервисом в одном pod'е (в Kubernetes) и берёт на себя сквозные задачи: проксирование трафика, логирование, мониторинг, безопасность.

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

### Что может делать Sidecar

- Проксирование трафика — mTLS, retry, circuit breaker (Envoy в Istio).
- Логирование — сбор и отправка логов (Fluentd sidecar).
- Мониторинг — экспорт метрик.
- Безопасность — аутентификация, авторизация, шифрование трафика.
- Конфигурация — подтягивание секретов из Vault.

<details><summary>Пример в Kubernetes</summary>

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

</details>

| Аспект | Преимущества | Недостатки |
|---|---|---|
| Разделение ответственности | Основной сервис не содержит инфраструктурного кода | Дополнительное потребление CPU, RAM |
| Технологическая гибкость | Sidecar может быть на другом языке | Увеличенная задержка (hop через sidecar) |
| Единообразие | Одна конфигурация sidecar для всех сервисов | Сложность отладки |

Sidecar — основа Service Mesh (Istio, Linkerd), который активно используется в крупных системах для управления сетевым взаимодействием.

> **На собеседовании:** свяжите Sidecar с Service Mesh — это показывает системное понимание. Ключевой плюс: разработчик пишет только бизнес-логику, а сетевые задачи (mTLS, retry, трассировка) берёт на себя sidecar.

[к оглавлению](#микросервисы)

---

## Что такое паттерн BFF (Backend for Frontend)?
<!-- grade: middle -->

BFF (Backend for Frontend) — это паттерн, при котором для каждого типа клиента (мобильное приложение, веб-приложение, партнёрское API) создаётся отдельный backend-сервис, оптимизированный под потребности этого клиента.

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
     └──────────────┼───────────────┘
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   Payment     Customer    Notification
   Service     Service     Service
```

### Зачем нужен BFF

1. Разные потребности клиентов — мобильному приложению нужен минимум данных (экономия трафика), веб-приложению — полный набор полей.
2. Агрегация данных — BFF объединяет данные из нескольких микросервисов в один ответ.
3. Разная частота изменений — мобильное приложение обновляется реже, его BFF может поддерживать старые форматы дольше.

<details><summary>Пример: Mobile BFF vs Web BFF</summary>

```java
// Mobile BFF — минимум данных, оптимизация для мобильных
@RestController
@RequestMapping("/mobile/api")
public class MobileAccountController {

    @GetMapping("/dashboard")
    public MobileDashboardResponse getDashboard(@AuthenticationPrincipal User user) {
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

</details>

### Когда использовать BFF

- Есть несколько типов клиентов с разными потребностями.
- Клиентам нужны агрегированные данные из нескольких сервисов.
- Разные команды отвечают за разные клиентские приложения.

### Когда НЕ нужен BFF

- Один тип клиента — достаточно API Gateway.
- Все клиенты потребляют одинаковые данные.

> **На собеседовании:** объясните отличие BFF от API Gateway. Gateway — общий для всех клиентов, BFF — специализированный для каждого типа клиента. Частая ошибка — путать эти два паттерна.

[к оглавлению](#микросервисы)

---

## Database per Service или Shared Database -- какой подход выбрать?
<!-- grade: middle -->

Database per Service — это принцип, при котором каждый микросервис владеет собственным хранилищем данных. Shared Database — общая БД для нескольких сервисов. Правильный подход для микросервисов — Database per Service, Shared Database допустим только как временное решение при миграции.

### Database per Service

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

| Критерий | Database per Service | Shared Database |
|---|---|---|
| Связность | Слабая — сервисы независимы | Сильная — общая схема |
| Хранилище | Оптимальное для каждого сервиса | Единое для всех |
| JOIN-запросы | Невозможны между базами | Простые JOIN |
| Транзакции | Saga (eventual consistency) | ACID |
| Масштабирование БД | Независимое | Общее |
| Отказоустойчивость | Сбой одной БД не влияет на другие | БД — единая точка отказа |
| Дублирование данных | Неизбежно | Нет |

### Компромисс -- Schema per Service

```sql
-- Каждый сервис имеет свою схему в одной БД
CREATE SCHEMA payment_service;
CREATE SCHEMA customer_service;

-- Сервисы обращаются ТОЛЬКО к своей схеме
GRANT USAGE ON SCHEMA payment_service TO payment_svc_user;
REVOKE ALL ON SCHEMA customer_service FROM payment_svc_user;
```

### Рекомендации

- Начинайте с Database per Service — это правильный подход для микросервисов.
- Если нужны данные другого сервиса — запрашивайте через API, а не через прямой доступ к БД.
- Shared Database — только как временное решение при миграции с монолита (паттерн Strangler Fig).
- Для отчётов и аналитики — используйте CQRS с отдельной read-моделью.

> **На собеседовании:** рекомендуйте Database per Service как целевой подход и объясните, почему: слабая связность и независимость. Но покажите, что понимаете цену: Saga вместо ACID и невозможность JOIN. Упомяните Schema per Service как промежуточный шаг.

[к оглавлению](#микросервисы)

---

## Что такое паттерны Retry и Timeout?
<!-- grade: middle -->

Retry и Timeout — базовые паттерны отказоустойчивости при межсервисном взаимодействии. Timeout ограничивает время ожидания ответа, Retry автоматически повторяет запрос при transient-ошибках.

### Timeout

Без таймаута один зависший сервис может «повесить» всю цепочку вызовов.

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

### Retry

Автоматический повтор запроса при сбое. Повторять только идемпотентные операции и только при transient (временных) ошибках.

<details><summary>Resilience4j Retry</summary>

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

</details>

### Exponential Backoff с Jitter

```
Попытка 1: ошибка → ждём 500ms + random(0-200ms)
Попытка 2: ошибка → ждём 1000ms + random(0-400ms)
Попытка 3: ошибка → ждём 2000ms + random(0-800ms)
Попытка 4: ошибка → fallback
```

Без jitter все клиенты, получившие ошибку одновременно, будут повторять запросы в одно и то же время, создавая пиковую нагрузку («грозовое стадо»).

<details><summary>Spring Retry</summary>

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
        return PaymentResult.deferred(request.getId());
    }
}
```

</details>

### Комбинация паттернов (порядок имеет значение!)

```
Запрос → Retry → CircuitBreaker → Timeout → Вызов сервиса
```

```yaml
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

> **На собеседовании:** обязательно упомяните exponential backoff с jitter — это показывает понимание реальных проблем. Ключевое правило: retry только для идемпотентных операций и только для 5xx, не для 4xx. Частая ошибка — ретраить бизнес-ошибки (400 Bad Request).

[к оглавлению](#микросервисы)

---

## Что такое паттерн Bulkhead?
<!-- grade: senior -->

Bulkhead (переборка) — это паттерн отказоустойчивости, изолирующий ресурсы для разных внешних вызовов, чтобы сбой одного компонента не исчерпал ресурсы для других.

> Аналогия из жизни: корабль разделён на водонепроницаемые отсеки (bulkheads). Если один отсек затоплен, другие остаются сухими и корабль не тонет.

### Проблема без Bulkhead

```
Без Bulkhead:
┌──────────────────────────────────┐
│ Thread Pool (20 потоков)         │
│ [customer] [customer] [customer] │ ← все потоки заняты зависшим
│ [customer] ... нет потоков для   │   customer-service
│ payment или notification!        │
└──────────────────────────────────┘

С Bulkhead:
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ Customer Pool│ │ Payment Pool │ │ Notif. Pool  │
│ (5 потоков)  │ │ (10 потоков) │ │ (5 потоков)  │
│ [зависли]    │ │ [работают]   │ │ [работают]   │
└──────────────┘ └──────────────┘ └──────────────┘
```

### Два типа Bulkhead

| Критерий | Thread Pool | Semaphore |
|---|---|---|
| Изоляция | Полная (отдельные потоки) | Частичная (общий пул) |
| Overhead | Выше (переключение контекста) | Ниже |
| Таймаут | Можно прервать поток | Нет контроля |
| Подходит для | Медленные вызовы | Быстрые вызовы |

<details><summary>Thread Pool Bulkhead</summary>

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
        return CompletableFuture.completedFuture(CustomerDto.unknown(id));
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

</details>

<details><summary>Semaphore Bulkhead</summary>

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
        maxConcurrentCalls: 5
        maxWaitDuration: 500ms
```

</details>

### Комбинация паттернов отказоустойчивости (порядок имеет значение!)

```
Запрос → Bulkhead → CircuitBreaker → Retry → Timeout → Вызов сервиса
```

> **На собеседовании:** объясните через аналогию с кораблём и покажите, что понимаете разницу между Thread Pool и Semaphore Bulkhead. Ключевой вопрос: почему Bulkhead стоит перед Circuit Breaker в цепочке — потому что он ограничивает количество одновременных вызовов ещё до проверки circuit breaker.

[к оглавлению](#микросервисы)

---

## Как обеспечить безопасность межсервисного взаимодействия?
<!-- grade: senior -->

Безопасность в микросервисной архитектуре — комплексная задача, включающая аутентификацию (OAuth2/JWT), шифрование (mTLS), управление секретами (Vault) и сетевые политики (Network Policies).

```
Клиент → API Gateway → Проверка JWT → Микросервис
                            │
                    ┌───────▼───────┐
                    │ Keycloak /    │
                    │ Spring Auth   │
                    │ Server        │
                    └───────────────┘
```

<details><summary>1. OAuth2/JWT: конфигурация Resource Server</summary>

```java
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

</details>

### 2. mTLS (Mutual TLS) -- взаимная аутентификация сервисов

При mTLS оба участника соединения предъявляют сертификаты. Обычно реализуется через Service Mesh (Istio):

```yaml
# Istio PeerAuthentication — требовать mTLS
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: banking
spec:
  mtls:
    mode: STRICT
```

### 3. Передача контекста пользователя между сервисами

```java
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

### 4. Защита секретов

- Хранение в HashiCorp Vault (не в переменных окружения и не в конфигурационных файлах).
- Ротация секретов (Vault поддерживает автоматическую ротацию паролей БД).
- Шифрование чувствительных данных в Kafka-сообщениях.

### 5. Сетевые политики (Network Policies в Kubernetes)

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

### 6. Валидация входных данных

Каждый сервис должен сам валидировать входные данные, не полагаясь на то, что вызывающий сервис уже провёл валидацию.

> **На собеседовании:** покажите многослойную защиту: JWT на Gateway, mTLS между сервисами, Vault для секретов, Network Policies для сетевой изоляции. Частая ошибка — думать, что JWT-валидации на Gateway достаточно и внутренние сервисы не нужно защищать.

[к оглавлению](#микросервисы)

---

## Что такое Service Mesh?
<!-- grade: senior -->

Service Mesh — это инфраструктурный слой, управляющий межсервисным взаимодействием через sidecar-прокси, развёрнутые рядом с каждым сервисом. Он берёт на себя сетевые задачи (mTLS, retry, трассировка) без изменения кода приложения.

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
│  ┌────────────▼───────────────▼──────────────┐        │
│  │ Istio Control Plane (istiod)              │        │
│  │ - Маршрутизация   - mTLS-сертификаты      │        │
│  │ - Политики        - Конфигурация прокси   │        │
│  └───────────────────────────────────────────┘        │
└───────────────────────────────────────────────────────┘
```

### Что предоставляет Service Mesh

- Наблюдаемость (Observability) — метрики, трассировка, логирование автоматически, без изменения кода.
- Безопасность — mTLS между всеми сервисами, RBAC-политики.
- Управление трафиком — canary deployments, A/B тестирование, fault injection.
- Отказоустойчивость — retry, timeout, circuit breaker на уровне прокси.

### Популярные решения

| Решение | Характеристика |
|---|---|
| Istio | Наиболее функциональный, на основе Envoy. Стандарт де-факто |
| Linkerd | Легковесный, проще в эксплуатации |
| Consul Connect | От HashiCorp, интегрирован с Consul |

<details><summary>Пример Istio: canary deployment и fault injection</summary>

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

```yaml
# Fault injection для тестирования устойчивости
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

</details>

### Когда нужен Service Mesh

- Много микросервисов (50+) — ручное управление сетевыми политиками невозможно.
- Строгие требования к безопасности (mTLS везде).
- Нужна единообразная наблюдаемость без изменения кода сервисов.

### Когда НЕ нужен

- Мало сервисов (менее 10-15).
- Нет Kubernetes.
- Команда не готова к дополнительной операционной сложности.

> **На собеседовании:** объясните архитектуру: Data Plane (sidecar-прокси) + Control Plane (управление). Ключевое преимущество: разработчик пишет только бизнес-логику, всё сетевое (mTLS, retry, трассировка) обеспечивается mesh'ем. Частая ошибка — предлагать Service Mesh для 5 сервисов.

[к оглавлению](#микросервисы)

---

## Как организовать деплой микросервисов?
<!-- grade: middle -->

Развёртывание микросервисов требует зрелых CI/CD практик и стратегий, минимизирующих риски. Основные стратегии: Blue-Green, Canary и Rolling Update.

### Стратегии развёртывания

| Стратегия | Принцип | Плюсы | Минусы |
|---|---|---|---|
| Blue-Green | 100% переключение между двумя окружениями | Мгновенный откат | Двойные ресурсы |
| Canary | Постепенное увеличение трафика (5% → 25% → 100%) | Раннее обнаружение проблем | Сложнее настроить |
| Rolling Update | Постепенная замена pod'ов | Стандарт в Kubernetes | Медленный откат |

<details><summary>Rolling Update в Kubernetes</summary>

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
      maxUnavailable: 0   # Не допускать недоступности
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

</details>

### CI/CD Pipeline для микросервиса

```yaml
stages:
  - build       # Maven build + unit tests
  - test        # Integration tests + Contract tests
  - scan        # SonarQube + SAST/DAST + License compliance
  - package     # Docker build + push to registry
  - deploy-dev  # Deploy to dev
  - deploy-stage # Deploy to staging + E2E tests
  - deploy-prod # Canary → Rolling update to production
```

### Graceful Shutdown -- корректное завершение при обновлении

<details><summary>Конфигурация graceful shutdown</summary>

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

</details>

### Рекомендации

- Каждый сервис деплоится независимо, со своим пайплайном.
- Обязательные этапы: сканирование безопасности (SAST/DAST), проверка лицензий.
- Feature flags (Unleash, LaunchDarkly) для безопасного включения новых функций.
- Автоматический rollback при деградации метрик.
- Чёткая процедура согласования релизов (change management).

> **На собеседовании:** знайте три стратегии деплоя и когда какую использовать. Обязательно упомяните graceful shutdown и readiness probes — без них rolling update будет терять запросы. Частая ошибка — забыть про preStop hook в Kubernetes.

[к оглавлению](#микросервисы)
