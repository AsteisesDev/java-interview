[Вопросы для собеседования](README.md)

# Архитектура приложений
+ [Что такое архитектура ПО и какова роль архитектора?](#Что-такое-архитектура-ПО-и-какова-роль-архитектора)
+ [Что такое монолитная архитектура? Каковы её плюсы и минусы?](#Что-такое-монолитная-архитектура-Каковы-её-плюсы-и-минусы)
+ [Что такое многослойная (layered) архитектура?](#Что-такое-многослойная-layered-архитектура)
+ [Что такое гексагональная архитектура (Ports and Adapters)?](#Что-такое-гексагональная-архитектура-Ports-and-Adapters)
+ [Что такое Clean Architecture (чистая архитектура)?](#Что-такое-Clean-Architecture-чистая-архитектура)
+ [Что такое Onion Architecture (луковая архитектура)?](#Что-такое-Onion-Architecture-луковая-архитектура)
+ [Как принципы SOLID применяются в контексте архитектуры?](#Как-принципы-SOLID-применяются-в-контексте-архитектуры)
+ [Что означают принципы DRY, KISS и YAGNI?](#Что-означают-принципы-DRY-KISS-и-YAGNI)
+ [Что такое Coupling и Cohesion (связанность и сцепленность)?](#Что-такое-Coupling-и-Cohesion-связанность-и-сцепленность)
+ [Что такое DDD (Domain-Driven Design)? Назовите основные понятия.](#Что-такое-DDD-Domain-Driven-Design-Назовите-основные-понятия)
+ [Какие архитектурные стили существуют? Сравните монолит, SOA, микросервисы и serverless.](#Какие-архитектурные-стили-существуют-Сравните-монолит-SOA-микросервисы-и-serverless)
+ [Что такое Event-Driven Architecture (EDA)?](#Что-такое-Event-Driven-Architecture-EDA)
+ [Что такое CQRS (Command Query Responsibility Segregation)?](#Что-такое-CQRS-Command-Query-Responsibility-Segregation)
+ [В чём различия паттернов MVC, MVP и MVVM?](#В-чём-различия-паттернов-MVC-MVP-и-MVVM)
+ [Что такое CAP-теорема?](#Что-такое-CAP-теорема)
+ [В чём разница между BASE и ACID?](#В-чём-разница-между-BASE-и-ACID)
+ [Чем отличается горизонтальное масштабирование от вертикального?](#Чем-отличается-горизонтальное-масштабирование-от-вертикального)
+ [Что такое балансировка нагрузки (load balancing)?](#Что-такое-балансировка-нагрузки-load-balancing)
+ [Какие стратегии кэширования существуют?](#Какие-стратегии-кэширования-существуют)
+ [Зачем нужен Message Broker? В чём разница между очередью и топиком?](#Зачем-нужен-Message-Broker-В-чём-разница-между-очередью-и-топиком)
+ [Что такое идемпотентность операций?](#Что-такое-идемпотентность-операций)
+ [Что такое API-first подход?](#Что-такое-API-first-подход)
+ [В чём разница между Stateless и Stateful приложениями?](#В-чём-разница-между-Stateless-и-Stateful-приложениями)
+ [Что такое Feature Flags (флаги функциональности)?](#Что-такое-Feature-Flags-флаги-функциональности)
+ [Что такое ADR (Architecture Decision Records)?](#Что-такое-ADR-Architecture-Decision-Records)
+ [Что такое нефункциональные требования (NFR)?](#Что-такое-нефункциональные-требования-NFR)
+ [Что такое паттерн Repository?](#Что-такое-паттерн-Repository)
+ [Что такое паттерн Unit of Work?](#Что-такое-паттерн-Unit-of-Work)
+ [Как Dependency Injection работает как архитектурный принцип?](#Как-Dependency-Injection-работает-как-архитектурный-принцип)
+ [Как спроектировать модуль с нуля? Пошаговый подход.](#Как-спроектировать-модуль-с-нуля-Пошаговый-подход)
+ [Что такое Saga-паттерн в распределённых системах?](#Что-такое-Saga-паттерн-в-распределённых-системах)

## Что такое архитектура ПО и какова роль архитектора?

__Архитектура программного обеспечения__ — это совокупность ключевых решений об организации программной системы: выбор структурных элементов и их интерфейсов, поведение системы как результат взаимодействия этих элементов, объединение элементов в подсистемы, а также архитектурный стиль, определяющий всю организацию.

Проще говоря, архитектура отвечает на вопрос: __«Из каких крупных частей состоит система, как они взаимодействуют и почему именно так?»__

Архитектура определяет:
+ Структуру компонентов системы и их взаимосвязи;
+ Распределение ответственности между модулями;
+ Принципы расширяемости и модифицируемости;
+ Подходы к обеспечению качественных атрибутов (производительность, безопасность, масштабируемость).

__Роль архитектора__ в проекте:
+ __Принятие ключевых технических решений__ — выбор технологий, фреймворков, баз данных, протоколов взаимодействия;
+ __Формирование архитектурных требований__ — перевод бизнес-требований в технические ограничения;
+ __Проектирование модулей__ — определение границ модулей, их API и контрактов;
+ __Контроль технического долга__ — отслеживание компромиссов и планирование рефакторинга;
+ __Менторство команды__ — передача знаний, проведение архитектурных review;
+ __Документирование решений__ — ведение ADR, диаграмм, описание принципов.

В банковских проектах архитектор также отвечает за соответствие регуляторным требованиям (ЦБ РФ, PCI DSS), обеспечение отказоустойчивости финансовых операций и безопасность данных.

[к оглавлению](#Архитектура-приложений)

## Что такое монолитная архитектура? Каковы её плюсы и минусы?

__Монолитная архитектура__ — это архитектурный стиль, при котором всё приложение разрабатывается, развёртывается и масштабируется как единое целое. Весь код находится в одном развёртываемом артефакте (например, один WAR/JAR-файл).

```
┌─────────────────────────────────────────┐
│              Монолит (WAR/JAR)          │
│  ┌──────────┐ ┌──────────┐ ┌────────┐  │
│  │    UI    │ │ Бизнес-  │ │ Доступ │  │
│  │  слой    │ │ логика   │ │ к БД   │  │
│  └──────────┘ └──────────┘ └────────┘  │
│  ┌──────────┐ ┌──────────┐ ┌────────┐  │
│  │ Модуль   │ │ Модуль   │ │ Модуль │  │
│  │ платежей │ │ клиентов │ │ отчётов│  │
│  └──────────┘ └──────────┘ └────────┘  │
└─────────────────────────────────────────┘
                    │
              ┌─────┴─────┐
              │    БД     │
              └───────────┘
```

__Плюсы:__
+ __Простота разработки__ — единая кодовая база, один процесс сборки;
+ __Простота развёртывания__ — один артефакт для деплоя;
+ __Простота отладки__ — можно пройти весь поток выполнения в одном процессе;
+ __Нет накладных расходов на сетевое взаимодействие__ — все вызовы локальные (in-process);
+ __Транзакционная целостность__ — легко обеспечить ACID в рамках одной БД;
+ __Низкий порог входа__ — не нужна экспертиза в распределённых системах.

__Минусы:__
+ __Сложность масштабирования__ — нельзя масштабировать отдельный модуль, только всё приложение целиком;
+ __Долгий цикл развёртывания__ — изменение одной строки требует пересборки и передеплоя всего приложения;
+ __Связанность (coupling)__ — изменения в одном модуле могут сломать другой;
+ __Технологическая привязка__ — весь код использует один стек технологий;
+ __Сложность для больших команд__ — конфликты при слиянии кода, сложная координация;
+ __Рост кодовой базы__ — со временем становится трудно понять и поддерживать.

__Когда монолит оправдан:__
+ На старте проекта, когда границы домена ещё не ясны;
+ Для небольших команд (до 5-7 разработчиков);
+ Когда важна транзакционная целостность и простота;
+ В MVP и прототипах.

[к оглавлению](#Архитектура-приложений)

## Что такое многослойная (layered) архитектура?

__Многослойная (layered) архитектура__ — один из наиболее распространённых архитектурных паттернов, при котором система разделяется на горизонтальные слои, каждый из которых выполняет определённую роль. Каждый слой зависит только от нижележащего.

Классическая трёхслойная архитектура в Spring-приложении:

```
        HTTP-запрос
             │
             ▼
┌─────────────────────────┐
│   Controller (Web слой) │  ← Принимает запросы, валидирует входные данные,
│   @RestController       │     возвращает ответы (DTO)
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│   Service (Бизнес-слой) │  ← Содержит бизнес-логику,
│   @Service              │     оркестрирует вызовы
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│  Repository (Слой       │  ← Работает с БД,
│  доступа к данным)      │     выполняет CRUD-операции
│  @Repository            │
└────────────┬────────────┘
             │
             ▼
        ┌─────────┐
        │   БД    │
        └─────────┘
```

Пример на Java (Spring Boot):

```java
// Controller — Web-слой
@RestController
@RequestMapping("/api/accounts")
public class AccountController {

    private final AccountService accountService;

    public AccountController(AccountService accountService) {
        this.accountService = accountService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<AccountDto> getAccount(@PathVariable Long id) {
        AccountDto account = accountService.findById(id);
        return ResponseEntity.ok(account);
    }

    @PostMapping("/transfer")
    public ResponseEntity<Void> transfer(@RequestBody @Valid TransferRequest request) {
        accountService.transfer(request);
        return ResponseEntity.ok().build();
    }
}

// Service — бизнес-слой
@Service
@Transactional
public class AccountService {

    private final AccountRepository accountRepository;

    public AccountService(AccountRepository accountRepository) {
        this.accountRepository = accountRepository;
    }

    public AccountDto findById(Long id) {
        Account account = accountRepository.findById(id)
            .orElseThrow(() -> new AccountNotFoundException(id));
        return AccountMapper.toDto(account);
    }

    public void transfer(TransferRequest request) {
        Account from = accountRepository.findById(request.getFromId())
            .orElseThrow(() -> new AccountNotFoundException(request.getFromId()));
        Account to = accountRepository.findById(request.getToId())
            .orElseThrow(() -> new AccountNotFoundException(request.getToId()));

        from.withdraw(request.getAmount());
        to.deposit(request.getAmount());

        accountRepository.save(from);
        accountRepository.save(to);
    }
}

// Repository — слой доступа к данным
@Repository
public interface AccountRepository extends JpaRepository<Account, Long> {
    List<Account> findByClientId(Long clientId);
}
```

__Правила многослойной архитектуры:__
+ Каждый слой зависит только от слоя непосредственно ниже;
+ Слой не должен знать о вышестоящем слое;
+ Слой может быть заменён без влияния на остальные (при соблюдении контракта);
+ Бизнес-логика сосредоточена в Service-слое.

__Плюсы:__
+ Простота понимания и низкий порог входа;
+ Разделение ответственности;
+ Возможность независимого тестирования каждого слоя.

__Минусы:__
+ Зависимость бизнес-логики от инфраструктуры (Service знает про Repository);
+ Склонность к «анемичной» модели данных (логика в сервисах, сущности — просто контейнеры данных);
+ При большом количестве модулей слои «размываются».

[к оглавлению](#Архитектура-приложений)

## Что такое гексагональная архитектура (Ports and Adapters)?

__Гексагональная архитектура__ (Hexagonal Architecture), также известная как __Ports and Adapters__, была предложена Алистером Кокберном. Основная идея — изолировать ядро приложения (бизнес-логику) от внешнего мира с помощью портов (интерфейсов) и адаптеров (реализаций).

```
                    ┌──── Адаптеры (Driving / Входящие) ────┐
                    │                                        │
              ┌───────────┐                          ┌───────────┐
              │  REST API │                          │   CLI     │
              │ Controller│                          │  Adapter  │
              └─────┬─────┘                          └─────┬─────┘
                    │                                      │
                    ▼                                      ▼
              ┌─────────────────────────────────────────────────┐
              │              Входящие порты                     │
              │         (интерфейсы Use Case)                   │
              │  ┌─────────────────────────────────────────┐    │
              │  │                                         │    │
              │  │         Ядро приложения                  │    │
              │  │     (Domain Model + Use Cases)          │    │
              │  │                                         │    │
              │  └─────────────────────────────────────────┘    │
              │              Исходящие порты                    │
              │         (интерфейсы репозиториев,               │
              │          внешних сервисов)                      │
              └──────────────┬──────────────┬───────────────────┘
                             │              │
                             ▼              ▼
                       ┌──────────┐  ┌────────────┐
                       │ JPA      │  │  Kafka     │
                       │ Adapter  │  │  Adapter   │
                       └──────────┘  └────────────┘
                    └──── Адаптеры (Driven / Исходящие) ────┘
```

__Порт__ — это интерфейс, определяющий контракт взаимодействия:
+ __Входящий (driving) порт__ — интерфейс, через который внешний мир обращается к приложению (Use Case);
+ __Исходящий (driven) порт__ — интерфейс, через который приложение обращается к внешнему миру (БД, очередь сообщений, внешний API).

__Адаптер__ — конкретная реализация порта:
+ __Входящий адаптер__ — REST-контроллер, gRPC-сервис, обработчик очереди;
+ __Исходящий адаптер__ — JPA-репозиторий, HTTP-клиент, Kafka-продюсер.

Пример на Java:

```java
// Входящий порт (Use Case)
public interface TransferMoneyUseCase {
    void transfer(TransferCommand command);
}

// Доменная модель
public class Account {
    private AccountId id;
    private Money balance;

    public void withdraw(Money amount) {
        if (balance.isLessThan(amount)) {
            throw new InsufficientFundsException(id, amount);
        }
        balance = balance.minus(amount);
    }

    public void deposit(Money amount) {
        balance = balance.plus(amount);
    }
}

// Исходящий порт
public interface AccountRepository {
    Account findById(AccountId id);
    void save(Account account);
}

// Реализация Use Case (Application Service)
@Service
public class TransferMoneyService implements TransferMoneyUseCase {

    private final AccountRepository accountRepository;

    public TransferMoneyService(AccountRepository accountRepository) {
        this.accountRepository = accountRepository;
    }

    @Override
    @Transactional
    public void transfer(TransferCommand command) {
        Account from = accountRepository.findById(command.getSourceId());
        Account to = accountRepository.findById(command.getTargetId());
        from.withdraw(command.getAmount());
        to.deposit(command.getAmount());
        accountRepository.save(from);
        accountRepository.save(to);
    }
}

// Входящий адаптер (REST)
@RestController
public class AccountController {

    private final TransferMoneyUseCase transferMoney;

    @PostMapping("/transfer")
    public ResponseEntity<Void> transfer(@RequestBody TransferRequest request) {
        transferMoney.transfer(new TransferCommand(
            new AccountId(request.getFromId()),
            new AccountId(request.getToId()),
            Money.of(request.getAmount())
        ));
        return ResponseEntity.ok().build();
    }
}

// Исходящий адаптер (JPA)
@Component
public class JpaAccountRepository implements AccountRepository {

    private final AccountJpaRepository jpaRepository;

    @Override
    public Account findById(AccountId id) {
        AccountEntity entity = jpaRepository.findById(id.getValue())
            .orElseThrow(() -> new AccountNotFoundException(id));
        return AccountMapper.toDomain(entity);
    }

    @Override
    public void save(Account account) {
        jpaRepository.save(AccountMapper.toEntity(account));
    }
}
```

__Плюсы:__
+ Бизнес-логика полностью изолирована от инфраструктуры;
+ Легко подменять адаптеры (например, заменить PostgreSQL на MongoDB);
+ Отличная тестируемость — можно тестировать ядро без БД и сети;
+ Соответствует принципу инверсии зависимостей (DIP из SOLID).

__Минусы:__
+ Больше кода (интерфейсы, маппинг между слоями);
+ Сложнее для небольших CRUD-приложений;
+ Требует дисциплины от команды.

[к оглавлению](#Архитектура-приложений)

## Что такое Clean Architecture (чистая архитектура)?

__Clean Architecture (чистая архитектура)__ — концепция, предложенная Робертом Мартином (Uncle Bob). Основной принцип: __зависимости направлены только внутрь__ — внешние слои знают о внутренних, но внутренние не знают о внешних.

```
┌──────────────────────────────────────────────────────────────┐
│                  Frameworks & Drivers                        │
│   (Spring, JPA, Kafka, REST, UI)                            │
│  ┌────────────────────────────────────────────────────────┐  │
│  │               Interface Adapters                       │  │
│  │   (Controllers, Gateways, Presenters)                  │  │
│  │  ┌──────────────────────────────────────────────────┐  │  │
│  │  │            Application Business Rules            │  │  │
│  │  │               (Use Cases)                        │  │  │
│  │  │  ┌────────────────────────────────────────────┐  │  │  │
│  │  │  │       Enterprise Business Rules            │  │  │  │
│  │  │  │          (Entities / Domain)               │  │  │  │
│  │  │  └────────────────────────────────────────────┘  │  │  │
│  │  └──────────────────────────────────────────────────┘  │  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘

Зависимости → всегда направлены внутрь (к центру)
```

__Слои:__
+ __Entities (Сущности)__ — бизнес-объекты и правила, не зависящие ни от чего. Могут использоваться в любом приложении;
+ __Use Cases (Сценарии использования)__ — специфическая бизнес-логика приложения. Оркестрируют сущности;
+ __Interface Adapters (Адаптеры интерфейсов)__ — преобразование данных между форматом Use Case и внешним форматом (контроллеры, презентеры, маппинг);
+ __Frameworks & Drivers__ — фреймворки, БД, веб-серверы и прочие внешние инструменты.

__Правило зависимостей (Dependency Rule):__ ничто во внутреннем слое не должно знать о чём-либо во внешнем слое. Имена, типы, классы — ничто, объявленное во внешнем слое, не должно упоминаться во внутреннем.

Типичная структура пакетов в Java-проекте:

```
com.bank.payment
├── domain/                     # Entities
│   ├── model/
│   │   ├── Payment.java
│   │   └── Money.java
│   └── port/
│       ├── in/
│       │   └── CreatePaymentUseCase.java
│       └── out/
│           └── PaymentRepository.java
├── application/                # Use Cases
│   └── CreatePaymentService.java
├── adapter/                    # Interface Adapters
│   ├── in/
│   │   └── web/
│   │       └── PaymentController.java
│   └── out/
│       └── persistence/
│           ├── PaymentJpaRepository.java
│           └── PaymentEntity.java
└── config/                     # Frameworks & Drivers
    └── PaymentBeanConfig.java
```

__Отличие от гексагональной архитектуры:__ концептуально Clean Architecture и Hexagonal Architecture очень похожи. Главное отличие — Clean Architecture явно выделяет слой Use Cases и описывает чёткую иерархию из четырёх колец, тогда как гексагональная архитектура фокусируется на портах и адаптерах без строгой внутренней стратификации ядра.

[к оглавлению](#Архитектура-приложений)

## Что такое Onion Architecture (луковая архитектура)?

__Onion Architecture (луковая архитектура)__ — архитектурный стиль, предложенный Джеффри Палермо. Как и Clean Architecture, строится на принципе: __зависимости направлены только к центру__.

```
┌────────────────────────────────────────────────────┐
│               Infrastructure                       │
│  (Spring, JPA, REST, Kafka, файловая система)      │
│  ┌──────────────────────────────────────────────┐  │
│  │          Application Services                │  │
│  │    (Use Cases, оркестрация, DTO)             │  │
│  │  ┌────────────────────────────────────────┐  │  │
│  │  │        Domain Services                 │  │  │
│  │  │  (доменные сервисы, правила)           │  │  │
│  │  │  ┌──────────────────────────────────┐  │  │  │
│  │  │  │        Domain Model              │  │  │  │
│  │  │  │  (Entities, Value Objects)       │  │  │  │
│  │  │  └──────────────────────────────────┘  │  │  │
│  │  └────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────┘
```

__Слои (от центра к периферии):__
+ __Domain Model__ — сущности, Value Objects, доменные события. Не зависит ни от чего;
+ __Domain Services__ — доменная логика, которая не принадлежит конкретной сущности;
+ __Application Services__ — координация сценариев, управление транзакциями;
+ __Infrastructure__ — реализации репозиториев, внешние сервисы, фреймворки.

__Ключевые правила:__
+ Интерфейсы репозиториев определяются в доменном слое;
+ Реализации репозиториев находятся в инфраструктурном слое;
+ Доменная модель не зависит от фреймворков.

__Сравнение Clean, Hexagonal и Onion Architecture:__

Все три архитектуры преследуют одну цель — изоляция бизнес-логики от инфраструктуры. Различия заключаются в терминологии и акцентах:

| Аспект | Hexagonal | Clean | Onion |
|--------|-----------|-------|-------|
| Автор | Алистер Кокберн | Роберт Мартин | Джеффри Палермо |
| Акцент | Порты и адаптеры | Правило зависимостей, 4 кольца | Слои вокруг доменной модели |
| Центр | Domain + Application | Entities | Domain Model |
| Кол-во слоёв | 2-3 (ядро, порты, адаптеры) | 4 (Entities, Use Cases, Adapters, Frameworks) | 4 (Domain, Domain Services, App Services, Infra) |

[к оглавлению](#Архитектура-приложений)

## Как принципы SOLID применяются в контексте архитектуры?

__SOLID__ — пять принципов объектно-ориентированного проектирования, которые применимы не только на уровне классов, но и на уровне модулей и компонентов системы.

__S — Single Responsibility Principle (Принцип единственной ответственности)__

На уровне архитектуры: каждый модуль / сервис отвечает за одну бизнес-область.

```
// Плохо: один сервис делает всё
@Service
public class MegaService {
    public void createPayment(...) { }
    public void generateReport(...) { }
    public void sendNotification(...) { }
}

// Хорошо: разделение ответственности
@Service public class PaymentService { ... }
@Service public class ReportService { ... }
@Service public class NotificationService { ... }
```

В микросервисной архитектуре SRP определяет границы сервисов: сервис платежей, сервис уведомлений, сервис отчётов.

__O — Open/Closed Principle (Принцип открытости/закрытости)__

На уровне архитектуры: система открыта для расширения, но закрыта для модификации. Новые возможности добавляются через новые модули, а не через изменение существующих.

```java
// Стратегия расчёта комиссии — расширяем через новую реализацию
public interface CommissionStrategy {
    Money calculate(Payment payment);
}

@Component public class StandardCommission implements CommissionStrategy { ... }
@Component public class VipCommission implements CommissionStrategy { ... }
// Новый тип — просто добавляем класс, не меняя существующий код
@Component public class CorporateCommission implements CommissionStrategy { ... }
```

__L — Liskov Substitution Principle (Принцип подстановки Барбары Лисков)__

На уровне архитектуры: любой компонент, реализующий интерфейс (порт), может быть заменён другой реализацией без нарушения работы системы. Например, замена PostgreSQL-репозитория на MongoDB-репозиторий не должна ломать бизнес-логику.

__I — Interface Segregation Principle (Принцип разделения интерфейсов)__

На уровне архитектуры: API модулей и сервисов должны быть узкоспециализированными. Клиент не должен зависеть от методов, которые он не использует.

```java
// Плохо: один «жирный» интерфейс
public interface AccountOperations {
    Account findById(Long id);
    void transfer(Long from, Long to, BigDecimal amount);
    Report generateMonthlyReport(Long accountId);
    void blockAccount(Long id);
}

// Хорошо: разделённые интерфейсы
public interface AccountQueryPort { Account findById(Long id); }
public interface TransferPort { void transfer(TransferCommand cmd); }
public interface AccountAdminPort { void blockAccount(Long id); }
```

__D — Dependency Inversion Principle (Принцип инверсии зависимостей)__

На уровне архитектуры: это фундамент гексагональной и чистой архитектуры. Модули высокого уровня (бизнес-логика) не зависят от модулей низкого уровня (инфраструктура). Оба зависят от абстракций.

```
Без инверсии:               С инверсией:
Service → JpaRepository      Service → Repository (интерфейс)
                                              ↑
                              JpaRepository реализует Repository
```

[к оглавлению](#Архитектура-приложений)

## Что означают принципы DRY, KISS и YAGNI?

__DRY (Don't Repeat Yourself)__ — «Не повторяйся».

Каждый фрагмент знания должен иметь единственное, непротиворечивое, авторитетное представление в системе.

+ Дублирование кода ведёт к рассинхронизации при изменениях;
+ Относится не только к коду, но и к конфигурации, схемам БД, документации;
+ Внимание: DRY — это не «любой одинаковый код надо выносить». Если два фрагмента кода __случайно__ совпадают, но описывают __разные__ концепции, объединять их не нужно. Иначе при изменении одной концепции сломается другая.

```java
// Плохо: дублирование логики валидации
public class PaymentService {
    public void createPayment(PaymentRequest req) {
        if (req.getAmount().compareTo(BigDecimal.ZERO) <= 0)
            throw new IllegalArgumentException("Amount must be positive");
        // ...
    }

    public void refundPayment(RefundRequest req) {
        if (req.getAmount().compareTo(BigDecimal.ZERO) <= 0) // дубль!
            throw new IllegalArgumentException("Amount must be positive");
        // ...
    }
}

// Хорошо: единое место для правила
public class Money {
    public Money(BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0)
            throw new IllegalArgumentException("Amount must be positive");
        this.amount = amount;
    }
}
```

__KISS (Keep It Simple, Stupid)__ — «Делай проще».

+ Простое решение лучше сложного, если оно решает задачу;
+ Не стоит внедрять микросервисы, CQRS и Event Sourcing для CRUD-приложения с 3 экранами;
+ Предпочитайте понятный код «умному» коду;
+ Архитектурная сложность должна быть оправдана бизнес-требованиями.

__YAGNI (You Aren't Gonna Need It)__ — «Вам это не понадобится».

+ Не реализуйте функциональность «на будущее»;
+ Проектируйте расширяемую архитектуру, но реализуйте только то, что нужно сейчас;
+ Преждевременная оптимизация и преждевременное усложнение — враги хорошей архитектуры.

```java
// YAGNI нарушение: написали поддержку 5 баз данных, хотя нужна только PostgreSQL
public interface DatabaseAdapter { }
public class PostgresAdapter implements DatabaseAdapter { }
public class MongoAdapter implements DatabaseAdapter { }      // не нужен
public class CassandraAdapter implements DatabaseAdapter { }  // не нужен
public class RedisAdapter implements DatabaseAdapter { }      // не нужен
public class ElasticAdapter implements DatabaseAdapter { }    // не нужен
```

[к оглавлению](#Архитектура-приложений)

## Что такое Coupling и Cohesion (связанность и сцепленность)?

__Coupling (связанность / зацепление)__ — степень взаимозависимости между модулями. Чем ниже coupling, тем лучше: модули можно изменять и развивать независимо.

__Cohesion (сцепленность / связность)__ — степень, в которой элементы внутри одного модуля принадлежат друг другу и работают над одной задачей. Чем выше cohesion, тем лучше: модуль сфокусирован и понятен.

__Идеал: Low Coupling + High Cohesion.__

```
Плохо: High Coupling + Low Cohesion

┌──────────────────┐     ┌──────────────────┐
│  Модуль A        │────▶│  Модуль B        │
│  - платежи       │◀────│  - клиенты       │
│  - уведомления   │────▶│  - отчёты        │
│  - часть отчётов │◀────│  - часть платежей│
└──────────────────┘     └──────────────────┘
  (всё в куче)             (всё в куче)

Хорошо: Low Coupling + High Cohesion

┌──────────────┐   event   ┌──────────────┐
│ Модуль       │──────────▶│ Модуль       │
│ Платежей     │           │ Уведомлений  │
│ (всё про     │           │ (всё про     │
│  платежи)    │           │  уведомления)│
└──────────────┘           └──────────────┘
```

__Типы связанности (от худшей к лучшей):__
+ __Content coupling__ — один модуль напрямую изменяет данные другого;
+ __Common coupling__ — модули разделяют глобальные данные;
+ __Control coupling__ — один модуль управляет логикой другого через флаги;
+ __Stamp coupling__ — модули обмениваются сложными структурами данных, используя лишь часть;
+ __Data coupling__ — модули обмениваются только необходимыми примитивными данными;
+ __Message coupling__ — модули общаются только через сообщения/события (наилучший вариант).

__Способы снижения coupling:__
+ Использовать интерфейсы и абстракции вместо конкретных реализаций;
+ Общаться через события (event-driven);
+ Применять Dependency Injection;
+ Определять чёткие контракты (API) между модулями;
+ В микросервисах — использовать API Gateway и асинхронное взаимодействие.

[к оглавлению](#Архитектура-приложений)

## Что такое DDD (Domain-Driven Design)? Назовите основные понятия.

__Domain-Driven Design (DDD)__ — подход к разработке программного обеспечения, при котором в центре проектирования находится предметная область (домен) бизнеса. Предложен Эриком Эвансом в книге «Domain-Driven Design: Tackling Complexity in the Heart of Software» (2003).

DDD особенно полезен в банковских приложениях, где бизнес-логика сложна и является главной ценностью системы.

__Стратегические паттерны:__

__Ubiquitous Language (Единый язык)__ — общий язык, разделяемый разработчиками и бизнес-экспертами. Термины из этого языка используются в коде, документации и разговорах. Например, в банковском контексте: «Платёжное поручение», «Корреспондентский счёт», «Операционный день».

```java
// Единый язык в коде: не "processItem()", а "executePaymentOrder()"
public class PaymentOrder {          // Платёжное поручение
    private Money amount;            // Сумма
    private Account payerAccount;    // Счёт плательщика
    private Account payeeAccount;    // Счёт получателя

    public void execute() { ... }    // Исполнить
    public void reject(String reason) { ... }  // Отклонить
}
```

__Bounded Context (Ограниченный контекст)__ — явная граница, внутри которой определённая модель однозначна и согласована. Один и тот же термин может означать разные вещи в разных контекстах.

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   Контекст       │     │   Контекст       │     │   Контекст       │
│   «Платежи»      │     │   «Клиенты»      │     │   «Риски»        │
│                  │     │                  │     │                  │
│  Account =       │     │  Account =       │     │  Account =       │
│  расчётный счёт  │     │  учётная запись   │     │  объект скоринга │
│  с балансом      │     │  клиента в ЛК     │     │                  │
└──────────────────┘     └──────────────────┘     └──────────────────┘
```

__Тактические паттерны:__

__Entity (Сущность)__ — объект, обладающий уникальной идентичностью, которая сохраняется на протяжении всего жизненного цикла. Две сущности с одинаковыми полями, но разными ID — это разные сущности.

```java
public class Account {
    private final AccountId id;  // идентичность
    private Money balance;
    private AccountStatus status;

    // Бизнес-поведение внутри сущности (Rich Domain Model)
    public void withdraw(Money amount) {
        if (status != AccountStatus.ACTIVE)
            throw new AccountNotActiveException(id);
        if (balance.isLessThan(amount))
            throw new InsufficientFundsException(id, amount);
        balance = balance.minus(amount);
    }
}
```

__Value Object (Объект-значение)__ — объект, определяемый своими атрибутами, а не идентичностью. Два Value Object с одинаковыми полями — это один и тот же объект. Неизменяем (immutable).

```java
public final class Money {
    private final BigDecimal amount;
    private final Currency currency;

    public Money(BigDecimal amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }

    public Money plus(Money other) {
        if (!currency.equals(other.currency))
            throw new CurrencyMismatchException();
        return new Money(amount.add(other.amount), currency);
    }

    // equals() и hashCode() по всем полям
}
```

__Aggregate (Агрегат)__ — кластер связанных сущностей и Value Objects, объединённых под одним корнем (Aggregate Root). Внешний мир взаимодействует с агрегатом только через корень. Агрегат гарантирует согласованность инвариантов.

```java
// Aggregate Root
public class Order {
    private OrderId id;
    private List<OrderItem> items;  // часть агрегата
    private Money totalAmount;

    public void addItem(Product product, int quantity) {
        items.add(new OrderItem(product, quantity));
        recalculateTotal();
    }

    private void recalculateTotal() {
        totalAmount = items.stream()
            .map(OrderItem::getSubtotal)
            .reduce(Money.ZERO, Money::plus);
    }
}
```

__Repository (Репозиторий)__ — абстракция для доступа к агрегатам. Создаёт иллюзию коллекции в памяти. Интерфейс определяется в домене, реализация — в инфраструктуре.

__Domain Event (Доменное событие)__ — событие, значимое для бизнеса, которое произошло в домене: `PaymentExecuted`, `AccountBlocked`, `LimitExceeded`.

__Domain Service (Доменный сервис)__ — бизнес-логика, которая не принадлежит ни одной конкретной сущности (например, конвертация валют между двумя счетами).

[к оглавлению](#Архитектура-приложений)

## Какие архитектурные стили существуют? Сравните монолит, SOA, микросервисы и serverless.

__1. Монолит__ — единое приложение, все компоненты в одном процессе.

__2. SOA (Service-Oriented Architecture)__ — приложение разделено на крупные сервисы, взаимодействующие через Enterprise Service Bus (ESB). Типично используется SOAP, WSDL.

__3. Микросервисы__ — приложение состоит из множества мелких независимых сервисов, каждый из которых отвечает за свой Bounded Context. Общение через REST/gRPC/Message Broker.

__4. Serverless__ — код исполняется в виде функций (FaaS), управляемых облачной платформой. Нет необходимости управлять серверами.

```
Монолит:
┌──────────────────────────┐
│    Всё в одном процессе  │
└──────────────────────────┘

SOA:
┌────────┐   ESB   ┌────────┐
│Service │◄──────►│Service │
│   A    │         │   B    │
└────────┘         └────────┘
    (крупные сервисы, общая шина)

Микросервисы:
┌───┐  ┌───┐  ┌───┐  ┌───┐
│ S1│  │ S2│  │ S3│  │ S4│
└─┬─┘  └─┬─┘  └─┬─┘  └─┬─┘
  │       │      │      │
  └───────┴──────┴──────┘
     (REST / MQ / gRPC)

Serverless:
  Event → λ Function → Result
  (управляет облачная платформа)
```

| Критерий | Монолит | SOA | Микросервисы | Serverless |
|----------|---------|-----|-------------|------------|
| Размер сервисов | Один | Крупные | Мелкие | Функции |
| Развёртывание | Единый артефакт | По сервисам | Независимое | Автоматическое |
| Масштабирование | Вертикальное | По сервисам | По сервисам | Автоматическое |
| Технологический стек | Единый | Может различаться | Независимый | Ограничен платформой |
| Связь между компонентами | Прямые вызовы | ESB (централизованная) | API / очереди (децентрализованная) | События |
| Сложность инфраструктуры | Низкая | Средняя | Высокая | Средняя |
| Транзакции | ACID (просто) | Распределённые | Eventual consistency | Eventual consistency |
| Подходит для | Малые/средние проекты | Enterprise legacy | Крупные проекты, большие команды | Event-driven задачи, нестабильная нагрузка |

__Для банковских приложений__ часто используется комбинация: монолитное ядро для критичных транзакций (ACID) и микросервисы для сопутствующих функций (уведомления, отчёты, аналитика).

[к оглавлению](#Архитектура-приложений)

## Что такое Event-Driven Architecture (EDA)?

__Event-Driven Architecture (EDA)__ — архитектурный стиль, в котором компоненты системы взаимодействуют посредством событий. Событие — факт, который произошёл в системе (например, «Платёж совершён», «Клиент зарегистрирован»).

```
┌─────────────┐   PaymentCreated   ┌──────────────┐
│  Сервис     │───────────────────▶│  Брокер      │
│  Платежей   │                    │  сообщений   │
└─────────────┘                    │  (Kafka)     │
                                   └──────┬───────┘
                                          │
                         ┌────────────────┼────────────────┐
                         ▼                ▼                ▼
                  ┌────────────┐  ┌────────────┐  ┌────────────┐
                  │  Сервис    │  │  Сервис    │  │  Сервис    │
                  │ Уведомлений│  │  Аналитики │  │  Комплаенс │
                  └────────────┘  └────────────┘  └────────────┘
```

__Основные топологии EDA:__

__1. Broker Topology (через брокер)__ — все события публикуются в брокер (Kafka, RabbitMQ), подписчики сами забирают нужные. Нет центрального оркестратора.

__2. Mediator Topology (через посредник)__ — центральный компонент (медиатор) координирует обработку события, рассылая команды обработчикам.

__Типы событий:__
+ __Event Notification__ — уведомление о факте. Содержит минимум данных. Подписчик сам запрашивает детали при необходимости;
+ __Event-Carried State Transfer__ — событие несёт в себе все необходимые данные, чтобы подписчик мог обработать его без дополнительных запросов;
+ __Domain Event__ — событие, имеющее смысл в домене бизнеса (`PaymentExecuted`, `AccountBlocked`).

Пример на Spring с Kafka:

```java
// Публикация события
@Service
public class PaymentService {

    private final KafkaTemplate<String, PaymentEvent> kafkaTemplate;

    @Transactional
    public void executePayment(PaymentCommand cmd) {
        Payment payment = createAndSavePayment(cmd);

        kafkaTemplate.send("payment-events",
            new PaymentExecutedEvent(payment.getId(), payment.getAmount(),
                LocalDateTime.now()));
    }
}

// Подписчик
@Component
public class NotificationEventHandler {

    @KafkaListener(topics = "payment-events", groupId = "notification-service")
    public void onPaymentExecuted(PaymentExecutedEvent event) {
        notificationService.sendPaymentConfirmation(event);
    }
}
```

__Плюсы EDA:__
+ Слабая связанность (low coupling) — продюсер не знает о подписчиках;
+ Масштабируемость — можно добавлять подписчиков без изменения продюсера;
+ Отказоустойчивость — сбой одного подписчика не влияет на остальных;
+ Асинхронность — высокая пропускная способность.

__Минусы EDA:__
+ Сложность отладки и трассировки;
+ Eventual consistency вместо немедленной согласованности;
+ Необходимость обрабатывать дублирование событий (идемпотентность);
+ Сложность гарантии порядка обработки.

[к оглавлению](#Архитектура-приложений)

## Что такое CQRS (Command Query Responsibility Segregation)?

__CQRS__ — паттерн, разделяющий операции чтения (Query) и записи (Command) на отдельные модели.

В классической архитектуре одна и та же модель используется и для записи, и для чтения. CQRS разделяет их, позволяя оптимизировать каждую сторону независимо.

```
              ┌──────────────────────────────────┐
              │            API Gateway           │
              └────────┬────────────┬────────────┘
                       │            │
                Command│            │Query
                       ▼            ▼
              ┌──────────┐  ┌──────────────┐
              │ Command  │  │   Query      │
              │ Model    │  │   Model      │
              │ (Write)  │  │   (Read)     │
              └────┬─────┘  └──────┬───────┘
                   │               │
                   ▼               ▼
              ┌──────────┐  ┌──────────────┐
              │  Write   │  │   Read DB    │
              │  DB      │──▶  (денормал.) │
              │(PostgreSQL)│ │  (Redis /   │
              └──────────┘  │  Elasticsearch)│
                  синхронизация └────────────┘
                  через события
```

```java
// Command — изменяет состояние, ничего не возвращает
public interface CreatePaymentCommandHandler {
    void handle(CreatePaymentCommand command);
}

@Service
public class CreatePaymentCommandHandlerImpl implements CreatePaymentCommandHandler {

    private final PaymentWriteRepository repository;
    private final EventPublisher eventPublisher;

    @Override
    @Transactional
    public void handle(CreatePaymentCommand command) {
        Payment payment = Payment.create(command);
        repository.save(payment);
        eventPublisher.publish(new PaymentCreatedEvent(payment));
    }
}

// Query — только читает, не изменяет состояние
public interface PaymentQueryService {
    PaymentView findById(Long id);
    List<PaymentView> findByClientId(Long clientId);
}

@Service
public class PaymentQueryServiceImpl implements PaymentQueryService {

    private final PaymentReadRepository readRepository;  // оптимизирован для чтения

    @Override
    public PaymentView findById(Long id) {
        return readRepository.findById(id);
    }
}
```

__Когда применять CQRS:__
+ Нагрузка на чтение значительно превышает нагрузку на запись (типично для банковских систем: просмотр выписок vs. совершение платежей);
+ Модели для чтения и записи сильно отличаются;
+ Нужна независимая масштабируемость чтения и записи;
+ Система использует Event Sourcing.

__Когда не стоит применять:__
+ Простые CRUD-приложения;
+ Когда eventual consistency неприемлема;
+ Маленькие проекты с одной командой.

__CQRS часто комбинируют с Event Sourcing__ — вместо хранения текущего состояния хранится полная последовательность событий, из которых состояние можно восстановить.

[к оглавлению](#Архитектура-приложений)

## В чём различия паттернов MVC, MVP и MVVM?

Все три паттерна разделяют представление (UI) и логику приложения, но по-разному организуют взаимодействие.

__MVC (Model-View-Controller)__

```
         Пользователь
              │
              ▼
        ┌────────────┐
        │ Controller │ ← Обрабатывает запрос, управляет потоком
        └──┬─────┬───┘
           │     │
     update│     │select view
           ▼     ▼
     ┌───────┐ ┌──────┐
     │ Model │ │ View │ ← View может читать из Model напрямую
     └───────┘ └──────┘
```

+ __Model__ — данные и бизнес-логика;
+ __View__ — отображение данных пользователю;
+ __Controller__ — принимает входные данные от пользователя, управляет Model и выбирает View.

В Spring MVC: `@Controller` принимает HTTP-запрос, вызывает сервис (Model), возвращает имя представления (View).

__MVP (Model-View-Presenter)__

```
     ┌──────┐        ┌───────────┐        ┌───────┐
     │ View │◄──────▶│ Presenter │◄──────▶│ Model │
     └──────┘        └───────────┘        └───────┘
```

+ __View__ — пассивно отображает данные. Только делегирует действия Presenter-у;
+ __Presenter__ — содержит логику представления, обновляет View через интерфейс;
+ View и Model не знают друг о друге. Presenter — единственный посредник.

__MVVM (Model-View-ViewModel)__

```
     ┌──────┐  data binding  ┌───────────┐        ┌───────┐
     │ View │◄══════════════▶│ ViewModel │◄──────▶│ Model │
     └──────┘                └───────────┘        └───────┘
```

+ __ViewModel__ — абстракция View, предоставляющая данные и команды;
+ Связь View и ViewModel осуществляется через __data binding__ (двусторонняя привязка данных);
+ Используется в JavaFX, Angular, WPF.

| Аспект | MVC | MVP | MVVM |
|--------|-----|-----|------|
| Связь View-Model | View может читать Model | Через Presenter | Через data binding |
| Тестируемость | Средняя | Высокая | Высокая |
| Сложность | Низкая | Средняя | Высокая |
| Типичное применение | Web (Spring MVC) | Desktop, Android | JavaFX, SPA-фреймворки |

В банковских Java-приложениях чаще всего используется __MVC__ (Spring MVC для серверной части) в сочетании с REST API и отдельным фронтенд-приложением.

[к оглавлению](#Архитектура-приложений)

## Что такое CAP-теорема?

__CAP-теорема__ (теорема Брюера) утверждает, что в распределённой системе невозможно одновременно обеспечить все три свойства:

+ __C (Consistency)__ — согласованность: все узлы видят одинаковые данные в одно время. Любое чтение возвращает результат последней успешной записи;
+ __A (Availability)__ — доступность: каждый запрос получает ответ (успех или ошибка), даже если часть узлов недоступна;
+ __P (Partition Tolerance)__ — устойчивость к разделению: система продолжает работать при потере связи между узлами сети.

```
                    Consistency (C)
                         ╱╲
                        ╱  ╲
                       ╱    ╲
                 CP   ╱  CAP ╲   CA
                     ╱ (невоз-╲
                    ╱  можно)  ╲
                   ╱────────────╲
     Partition    ╱      AP      ╲  Availability (A)
     Tolerance (P)
```

Поскольку в реальных распределённых системах сетевые разделения (P) неизбежны, фактический выбор — между __CP__ и __AP__:

+ __CP-системы__ — жертвуют доступностью ради согласованности. Пример: HBase, ZooKeeper, MongoDB (в конфигурации с ожиданием подтверждения от большинства). Подходит для финансовых операций, где важна точность данных;
+ __AP-системы__ — жертвуют немедленной согласованностью ради доступности. Пример: Cassandra, DynamoDB, CouchDB. Данные могут быть временно рассогласованы (eventual consistency);
+ __CA-системы__ — возможны только без сетевых разделений (по сути, одиночный узел). Пример: традиционная СУБД на одном сервере (PostgreSQL, MySQL без репликации).

__В банковском контексте:__
+ Для основных финансовых операций (переводы, платежи) выбирают __CP__ — лучше вернуть ошибку, чем провести некорректную транзакцию;
+ Для сервисов аналитики и отображения истории можно использовать __AP__ — допустима задержка в несколько секунд.

[к оглавлению](#Архитектура-приложений)

## В чём разница между BASE и ACID?

__ACID__ и __BASE__ — два подхода к управлению данными в системах.

__ACID__ (свойства классических реляционных транзакций):

+ __Atomicity (Атомарность)__ — транзакция либо выполняется полностью, либо не выполняется вовсе;
+ __Consistency (Согласованность)__ — транзакция переводит БД из одного согласованного состояния в другое;
+ __Isolation (Изолированность)__ — параллельные транзакции не влияют друг на друга;
+ __Durability (Долговечность)__ — после фиксации (commit) данные сохраняются даже при сбое.

__BASE__ (подход распределённых NoSQL-систем):

+ __Basically Available (Базовая доступность)__ — система доступна, даже если часть узлов отказала;
+ __Soft state (Мягкое состояние)__ — состояние системы может изменяться со временем даже без новых входных данных (из-за синхронизации);
+ __Eventually consistent (Согласованность в конечном счёте)__ — система придёт в согласованное состояние через некоторое время.

| Аспект | ACID | BASE |
|--------|------|------|
| Согласованность | Немедленная (strong) | В конечном счёте (eventual) |
| Доступность | Может быть снижена | Приоритет |
| Подход | Пессимистичный (блокировки) | Оптимистичный |
| Масштабирование | Вертикальное | Горизонтальное |
| Применение | Финансовые транзакции, банковские операции | Социальные сети, каталоги, аналитика |
| Примеры БД | PostgreSQL, Oracle, MySQL (InnoDB) | Cassandra, DynamoDB, MongoDB |

__В банковских приложениях:__
+ Основные операции (переводы, зачисления) — __ACID__ через PostgreSQL/Oracle;
+ Вспомогательные данные (история просмотров, аналитика, рекомендации) — могут использовать __BASE__-подход;
+ Микросервисная архитектура часто вынуждена использовать __BASE__ между сервисами (Saga-паттерн), сохраняя __ACID__ внутри каждого сервиса.

[к оглавлению](#Архитектура-приложений)

## Чем отличается горизонтальное масштабирование от вертикального?

__Вертикальное масштабирование (Scale Up)__ — увеличение мощности одного сервера: добавление CPU, RAM, дисков.

__Горизонтальное масштабирование (Scale Out)__ — добавление новых серверов (узлов) в кластер.

```
Вертикальное:                    Горизонтальное:

┌──────────────────┐            ┌────────┐ ┌────────┐ ┌────────┐
│                  │            │ Server │ │ Server │ │ Server │
│   Мощный сервер  │            │   1    │ │   2    │ │   3    │
│   (много CPU,    │            └───┬────┘ └───┬────┘ └───┬────┘
│    много RAM)    │                │          │          │
│                  │                └──────────┴──────────┘
└──────────────────┘                          │
                                     ┌────────┴────────┐
                                     │  Load Balancer  │
                                     └─────────────────┘
```

| Аспект | Вертикальное | Горизонтальное |
|--------|-------------|---------------|
| Способ | Больше ресурсов одному серверу | Больше серверов |
| Предел | Ограничен оборудованием | Теоретически безграничен |
| Downtime | Обычно нужна остановка | Без остановки |
| Стоимость | Экспоненциально растёт | Линейно растёт |
| Сложность | Простое | Требует балансировки, распределённой работы |
| Данные | Одна БД | Нужна репликация / шардирование |
| Отказоустойчивость | Единая точка отказа | Высокая (при сбое одного узла работают другие) |

__Стратегии горизонтального масштабирования:__
+ __Stateless-сервисы__ — приложение не хранит состояние в памяти, любой запрос может обработать любой узел;
+ __Репликация БД__ — master-slave для распределения нагрузки на чтение;
+ __Шардирование БД__ — разделение данных по узлам (например, по диапазону ID клиентов);
+ __Кэширование__ — Redis / Memcached для снижения нагрузки на БД.

__Типичный подход для банковского приложения:__
+ Вертикальное масштабирование для БД (мощный сервер + репликация);
+ Горизонтальное масштабирование для stateless-сервисов (Spring Boot) за балансировщиком нагрузки.

[к оглавлению](#Архитектура-приложений)

## Что такое балансировка нагрузки (load balancing)?

__Балансировка нагрузки (Load Balancing)__ — распределение входящих запросов между несколькими серверами для обеспечения высокой доступности и равномерного распределения ресурсов.

```
         Клиенты
      ┌────┬────┬────┐
      │    │    │    │
      ▼    ▼    ▼    ▼
  ┌─────────────────────┐
  │   Load Balancer     │
  │   (Nginx / HAProxy  │
  │    / AWS ALB)       │
  └──┬──────┬──────┬────┘
     │      │      │
     ▼      ▼      ▼
  ┌──────┐┌──────┐┌──────┐
  │App 1 ││App 2 ││App 3 │
  └──────┘└──────┘└──────┘
```

__Алгоритмы балансировки:__

+ __Round Robin__ — запросы распределяются по очереди. Простой, но не учитывает нагрузку серверов;
+ __Weighted Round Robin__ — учитывает веса (мощность) серверов. Более мощный сервер получает больше запросов;
+ __Least Connections__ — запрос направляется к серверу с наименьшим числом активных соединений;
+ __Least Response Time__ — к серверу с минимальным временем отклика;
+ __IP Hash__ — определение сервера по хешу IP-адреса клиента. Гарантирует, что один клиент всегда попадает на один сервер (sticky sessions);
+ __Random__ — случайный выбор сервера.

__L4 vs L7 балансировка:__

| Аспект | L4 (транспортный уровень) | L7 (уровень приложения) |
|--------|--------------------------|------------------------|
| Уровень OSI | 4 (TCP/UDP) | 7 (HTTP/HTTPS) |
| Что видит | IP-адреса, порты | URL, заголовки, cookies, тело запроса |
| Скорость | Быстрее (не разбирает содержимое) | Медленнее (анализирует HTTP) |
| Возможности | Простая маршрутизация | Content-based routing, A/B-тестирование, SSL termination |
| Примеры | HAProxy (L4 mode), AWS NLB | Nginx, HAProxy (L7 mode), AWS ALB |
| Применение | TCP-сервисы, БД, высокопроизводительная маршрутизация | Веб-приложения, REST API, микросервисы |

```
L7 балансировка — маршрутизация по содержимому:

/api/payments  →  Payment Service (3 инстанса)
/api/clients   →  Client Service (2 инстанса)
/api/reports   →  Report Service (1 инстанс)
```

__Health Checks:__ балансировщик периодически проверяет доступность серверов. Неработающие серверы исключаются из пула до восстановления. В Spring Boot для этого используется Spring Actuator (`/actuator/health`).

[к оглавлению](#Архитектура-приложений)

## Какие стратегии кэширования существуют?

__Кэширование__ — хранение часто запрашиваемых данных в быстром хранилище для снижения нагрузки на основной источник данных.

__Основные стратегии:__

__1. Cache-Aside (Lazy Loading)__ — приложение само управляет кэшем. При промахе читает из БД и кладёт в кэш.

```
    Запрос → Есть в кэше?
              ├── Да  → Вернуть из кэша
              └── Нет → Прочитать из БД → Записать в кэш → Вернуть
```

```java
@Service
public class AccountService {

    private final AccountRepository repository;
    private final RedisTemplate<String, Account> redisTemplate;

    public Account findById(Long id) {
        String key = "account:" + id;
        Account cached = redisTemplate.opsForValue().get(key);
        if (cached != null) return cached;

        Account account = repository.findById(id)
            .orElseThrow(() -> new NotFoundException(id));
        redisTemplate.opsForValue().set(key, account, Duration.ofMinutes(30));
        return account;
    }
}
```

+ Плюс: кэшируются только реально запрашиваемые данные;
+ Минус: первый запрос всегда медленный (cache miss); данные могут устаревать.

__2. Write-Through__ — при записи данные сохраняются одновременно в кэш и в БД.

```
    Запись → Обновить кэш → Обновить БД → Ответ
```

+ Плюс: кэш всегда актуален;
+ Минус: увеличенная задержка записи; кэшируются данные, которые могут никогда не читаться.

__3. Write-Behind (Write-Back)__ — данные записываются в кэш немедленно, а в БД — асинхронно (с задержкой).

```
    Запись → Обновить кэш → Ответ
                  │
                  └── Асинхронно (через N мс) → Обновить БД
```

+ Плюс: очень быстрая запись;
+ Минус: риск потери данных при сбое кэша до синхронизации с БД.

__4. Read-Through__ — кэш сам обращается к БД при промахе (в отличие от Cache-Aside, где это делает приложение).

__5. Refresh-Ahead__ — кэш обновляет данные до истечения TTL, если они часто запрашиваются.

__Инструменты кэширования в Java:__

| Инструмент | Тип | Применение |
|------------|-----|-----------|
| __Redis__ | Распределённый | Кэш между микросервисами, сессии, лимиты |
| __EhCache__ | Локальный (in-process) | Кэш внутри одного приложения |
| __Caffeine__ | Локальный (in-process) | Быстрый кэш в памяти JVM |
| __Hazelcast__ | Распределённый | Distributed cache, in-memory data grid |

Пример с Spring Cache (абстракция):

```java
@Service
public class ClientService {

    @Cacheable(value = "clients", key = "#id")
    public ClientDto findById(Long id) {
        return clientRepository.findById(id)
            .map(ClientMapper::toDto)
            .orElseThrow();
    }

    @CacheEvict(value = "clients", key = "#id")
    public void updateClient(Long id, UpdateClientRequest request) {
        // обновление клиента в БД
    }

    @CacheEvict(value = "clients", allEntries = true)
    public void clearCache() { }
}
```

__Проблемы кэширования:__
+ __Cache Stampede__ — множество одновременных запросов при промахе кэша. Решение: блокировка (mutex) при подгрузке;
+ __Stale data__ — устаревшие данные. Решение: TTL, активная инвалидация при обновлении;
+ __Cache Invalidation__ — одна из двух самых сложных проблем в computer science (Phil Karlton).

[к оглавлению](#Архитектура-приложений)

## Зачем нужен Message Broker? В чём разница между очередью и топиком?

__Message Broker (брокер сообщений)__ — промежуточное ПО, которое принимает, хранит и доставляет сообщения между компонентами системы.

__Зачем нужен:__
+ __Слабая связанность__ — отправитель не знает получателя;
+ __Асинхронность__ — отправитель не ждёт обработки;
+ __Буферизация__ — сглаживание пиков нагрузки;
+ __Гарантия доставки__ — сообщение не потеряется при сбое получателя;
+ __Масштабируемость__ — можно добавлять потребителей динамически.

__Очередь (Queue) vs Топик (Topic):__

```
Очередь (Queue) — Point-to-Point:
Каждое сообщение обрабатывается ОДНИМ потребителем.

Producer → [  msg3 | msg2 | msg1  ] → Consumer A  (получит msg1)
                                    → Consumer B  (получит msg2)
                                    → Consumer C  (получит msg3)

Топик (Topic) — Publish/Subscribe:
Каждое сообщение получают ВСЕ подписчики.

                              ┌→ Consumer Group A (получит ВСЕ сообщения)
Producer → [ Topic ] ─────────┼→ Consumer Group B (получит ВСЕ сообщения)
                              └→ Consumer Group C (получит ВСЕ сообщения)
```

| Аспект | Очередь (Queue) | Топик (Topic) |
|--------|-----------------|---------------|
| Модель | Point-to-Point | Pub/Sub |
| Получатели | Один на сообщение | Все подписчики |
| Сценарий | Обработка задач (worker pool) | Уведомление нескольких систем |
| Пример | Обработка платежей (каждый платёж обрабатывается одним воркером) | Событие «Платёж совершён» — отправить уведомление, обновить аналитику, проверить фрод |

__Популярные брокеры:__

| Брокер | Модель | Особенности |
|--------|--------|------------|
| __Apache Kafka__ | Topic (лог) | Высокая пропускная способность, хранение истории, replay |
| __RabbitMQ__ | Queue / Topic (exchange) | Гибкая маршрутизация, поддержка AMQP |
| __ActiveMQ__ | Queue / Topic | JMS-совместимый, зрелый |
| __Redis Streams__ | Topic (лог) | Простой, быстрый, встроен в Redis |

В Kafka каждый __Consumer Group__ — это логический подписчик. Внутри группы сообщения распределяются между потребителями (как очередь), но между группами — доставляются всем (как топик).

[к оглавлению](#Архитектура-приложений)

## Что такое идемпотентность операций?

__Идемпотентность__ — свойство операции, при котором многократное выполнение с одними и теми же входными данными даёт тот же результат, что и однократное выполнение.

```
Идемпотентная операция:
  f(x) = f(f(x))

  Перевести статус заказа в "ОПЛАЧЕН" — можно вызвать 100 раз, результат один.
  GET /api/accounts/123 — сколько ни вызывай, данные не изменятся.

НЕ идемпотентная операция:
  "Пополнить счёт на 1000 руб." — при повторном вызове сумма удвоится!
```

__HTTP-методы и идемпотентность:__

| Метод | Идемпотентный | Безопасный |
|-------|:------------:|:---------:|
| GET | Да | Да |
| HEAD | Да | Да |
| PUT | Да | Нет |
| DELETE | Да | Нет |
| POST | Нет | Нет |
| PATCH | Нет | Нет |

__Почему это критично:__
+ Сетевые сбои — клиент не получил ответ и повторяет запрос;
+ Retry-политики в микросервисах;
+ Дублирование сообщений в очередях (at-least-once delivery в Kafka);
+ В банковских системах повторный вызов может привести к двойному списанию.

__Способы обеспечения идемпотентности:__

__1. Idempotency Key__ — клиент передаёт уникальный ключ. Сервер проверяет, обрабатывалась ли операция с таким ключом.

```java
@PostMapping("/payments")
public ResponseEntity<PaymentResult> createPayment(
        @RequestHeader("Idempotency-Key") String idempotencyKey,
        @RequestBody CreatePaymentRequest request) {

    // Проверяем, не обрабатывали ли мы уже этот запрос
    Optional<PaymentResult> existing = idempotencyStore.find(idempotencyKey);
    if (existing.isPresent()) {
        return ResponseEntity.ok(existing.get());  // возвращаем прежний результат
    }

    PaymentResult result = paymentService.create(request);
    idempotencyStore.save(idempotencyKey, result);
    return ResponseEntity.status(HttpStatus.CREATED).body(result);
}
```

__2. Условные операции__ — использование версий, ETag, условных обновлений в БД:

```sql
-- Идемпотентное обновление: статус изменится только один раз
UPDATE payments SET status = 'COMPLETED'
WHERE id = :id AND status = 'PENDING';
```

__3. Естественная идемпотентность__ — проектирование операций как установка состояния, а не как приращение:

```java
// Не идемпотентно:
account.addBalance(1000);  // при повторе удвоится

// Идемпотентно:
account.setBalance(5000);  // при повторе результат тот же
```

[к оглавлению](#Архитектура-приложений)

## Что такое API-first подход?

__API-first__ — подход к разработке, при котором API проектируется и описывается __до__ написания кода реализации. API становится контрактом между командами (backend, frontend, мобильные приложения, внешние системы).

__Порядок работы:__

```
1. Проектирование API (OpenAPI / Swagger спецификация)
        │
        ▼
2. Обсуждение и согласование контракта между командами
        │
        ▼
3. Генерация кода (контроллеры, клиенты, DTO)
        │
        ▼
4. Параллельная разработка:
   Backend реализует API    |    Frontend использует mock API
        │                              │
        ▼                              ▼
5. Интеграция и тестирование
```

Пример OpenAPI-спецификации (YAML):

```yaml
openapi: 3.0.3
info:
  title: Payment API
  version: 1.0.0
paths:
  /api/v1/payments:
    post:
      summary: Создать платёж
      operationId: createPayment
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreatePaymentRequest'
      responses:
        '201':
          description: Платёж создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PaymentResponse'
        '400':
          description: Некорректный запрос
        '409':
          description: Дублирование (идемпотентность)
components:
  schemas:
    CreatePaymentRequest:
      type: object
      required: [payerAccountId, payeeAccountId, amount, currency]
      properties:
        payerAccountId:
          type: integer
          format: int64
        payeeAccountId:
          type: integer
          format: int64
        amount:
          type: number
          format: decimal
        currency:
          type: string
          enum: [RUB, USD, EUR]
```

__Инструменты:__
+ __OpenAPI Generator__ — генерация серверного и клиентского кода из спецификации;
+ __Swagger UI__ — интерактивная документация;
+ __Spring Cloud Contract__ — contract testing между микросервисами;
+ __Prism__ — mock-сервер по спецификации.

__Плюсы:__
+ Параллельная разработка frontend и backend;
+ Единый источник правды для API;
+ Автогенерация кода, документации и тестов;
+ Раннее выявление несовместимостей.

__Минусы:__
+ Дополнительные усилия на проектирование API до начала реализации;
+ Необходимость поддерживать спецификацию в актуальном состоянии.

В банковских проектах API-first особенно ценен, так как множество систем интегрируются друг с другом и чёткий контракт критически важен.

[к оглавлению](#Архитектура-приложений)

## В чём разница между Stateless и Stateful приложениями?

__Stateless (без состояния)__ — приложение не хранит состояние клиента между запросами. Каждый запрос содержит всю необходимую информацию для обработки.

__Stateful (с состоянием)__ — приложение хранит состояние клиента (сессию) в памяти между запросами.

```
Stateless:
  Запрос 1 (token + данные) → Сервер A → Ответ
  Запрос 2 (token + данные) → Сервер B → Ответ   (любой сервер)
  Запрос 3 (token + данные) → Сервер C → Ответ

Stateful:
  Запрос 1 → Сервер A (создаёт сессию) → Ответ (session_id)
  Запрос 2 (session_id) → Сервер A → Ответ   (ТОЛЬКО Сервер A!)
  Запрос 3 (session_id) → Сервер A → Ответ
```

| Аспект | Stateless | Stateful |
|--------|-----------|----------|
| Состояние | Не хранится на сервере | Хранится в памяти сервера |
| Масштабирование | Простое (любой узел) | Сложное (sticky sessions / репликация) |
| Отказоустойчивость | Высокая | Потеря сессии при падении узла |
| Производительность | Каждый запрос содержит полный контекст | Меньше передаваемых данных (сессия на сервере) |
| Пример | REST API + JWT | Servlet с HttpSession |

__Как сделать приложение Stateless:__
+ Аутентификация через __JWT-токены__ (вся информация о пользователе внутри токена);
+ Хранение сессий во __внешнем хранилище__ (Redis) — Spring Session;
+ Хранение состояния на стороне клиента (cookie, localStorage).

```java
// Stateless: JWT содержит всю информацию
@GetMapping("/api/profile")
public ResponseEntity<ProfileDto> getProfile(
        @AuthenticationPrincipal JwtUser user) {
    // user уже извлечён из токена, не нужна серверная сессия
    return ResponseEntity.ok(profileService.getProfile(user.getId()));
}
```

__В банковских приложениях__ предпочитают Stateless-подход для backend-сервисов (REST API на Spring Boot + JWT/OAuth2 + Redis для сессий), что упрощает горизонтальное масштабирование и развёртывание в Kubernetes.

[к оглавлению](#Архитектура-приложений)

## Что такое Feature Flags (флаги функциональности)?

__Feature Flags (Feature Toggles)__ — техника, позволяющая включать и выключать функциональность без повторного развёртывания приложения. Код новой функции присутствует в production, но активируется по условию.

```java
@Service
public class PaymentService {

    private final FeatureFlagService featureFlags;

    public PaymentResult processPayment(PaymentRequest request) {
        if (featureFlags.isEnabled("new-fraud-check")) {
            // Новая логика проверки на мошенничество
            fraudCheckV2.check(request);
        } else {
            // Старая логика
            fraudCheckV1.check(request);
        }
        return executePayment(request);
    }
}
```

__Типы Feature Flags:__

+ __Release Toggle__ — скрытие незавершённой функциональности. Позволяет trunk-based development (код в main, но фича выключена);
+ __Experiment Toggle__ — A/B-тестирование. Часть пользователей видит вариант A, часть — вариант B;
+ __Ops Toggle__ — оперативное управление. Отключение тяжёлой функции при деградации производительности;
+ __Permission Toggle__ — функциональность только для определённых пользователей (beta-тестеры, VIP-клиенты).

__Инструменты:__
+ __Togglz__ — Java-библиотека для Feature Flags;
+ __Unleash__ — open-source сервер Feature Flags;
+ __LaunchDarkly__ — облачный SaaS;
+ Самописное решение на Spring + БД/Redis.

__Лучшие практики:__
+ Удалять флаги после полного включения функции (иначе код засоряется);
+ Логировать состояние флагов для отладки;
+ Тестировать обе ветки (включено и выключено).

__В банковских проектах__ Feature Flags критичны: можно постепенно раскатывать новые тарифы, алгоритмы скоринга, интеграции с внешними системами, не рискуя стабильностью всей системы. Если что-то пошло не так — мгновенный откат через выключение флага.

[к оглавлению](#Архитектура-приложений)

## Что такое ADR (Architecture Decision Records)?

__ADR (Architecture Decision Records)__ — документ, фиксирующий принятое архитектурное решение, его контекст, причины и последствия.

ADR решает проблему «почему мы это сделали?» — через полгода никто не помнит, почему выбрали Kafka вместо RabbitMQ или PostgreSQL вместо MongoDB.

__Типичная структура ADR:__

```
# ADR-001: Использовать Kafka для межсервисного взаимодействия

## Статус
Принято (Accepted) — 2025-01-15

## Контекст
Система платёжного шлюза состоит из 8 микросервисов. Необходимо обеспечить
надёжное асинхронное взаимодействие между сервисами с гарантией доставки
и возможностью повторного чтения сообщений.

## Решение
Использовать Apache Kafka как основной Message Broker.

## Обоснование
- Kafka обеспечивает гарантию at-least-once delivery;
- Хранение истории сообщений (retention) позволяет replay при ошибках;
- Высокая пропускная способность (до 1M msg/sec);
- В команде есть экспертиза по Kafka;
- RabbitMQ рассматривался, но не подходит из-за отсутствия
  хранения истории и lower throughput для наших объёмов.

## Последствия
- Необходимо развернуть и поддерживать Kafka-кластер (3+ брокера);
- Команда должна обеспечить идемпотентность консьюмеров;
- Мониторинг consumer lag через Kafka Lag Exporter + Grafana;
- Усложняется локальная разработка (нужен Docker Compose с Kafka).
```

__Принципы ведения ADR:__
+ Каждое решение — отдельный файл (ADR-001.md, ADR-002.md, ...);
+ Хранить в репозитории рядом с кодом (docs/adr/);
+ Статусы: Proposed → Accepted / Rejected / Superseded;
+ Не удалять старые ADR — помечать как Superseded и ссылаться на новый;
+ Писать кратко — ADR не должен быть на 10 страниц.

[к оглавлению](#Архитектура-приложений)

## Что такое нефункциональные требования (NFR)?

__Нефункциональные требования (Non-Functional Requirements, NFR)__ — требования, описывающие __как__ система должна работать (качество), а не __что__ она должна делать (функции).

__Основные категории NFR:__

__1. Производительность (Performance)__
+ Время отклика (Response Time): «API-метод должен отвечать за < 200 мс (P95)»;
+ Пропускная способность (Throughput): «Система должна обрабатывать 1000 платежей в секунду»;
+ Задержка (Latency): «End-to-end задержка < 500 мс».

__2. Доступность (Availability)__
+ SLA: «99.95% uptime = не более 4.38 часов простоя в год»;
+ RTO (Recovery Time Objective): максимальное время восстановления после сбоя;
+ RPO (Recovery Point Objective): максимально допустимый объём потерянных данных.

__3. Масштабируемость (Scalability)__
+ «Система должна выдерживать 10-кратный рост нагрузки за 6 месяцев»;
+ Горизонтальное масштабирование без архитектурных изменений.

__4. Безопасность (Security)__
+ Аутентификация и авторизация (OAuth2, JWT);
+ Шифрование данных at rest и in transit (TLS);
+ Соответствие PCI DSS для обработки карточных данных;
+ Журналирование всех критичных операций (audit log).

__5. Надёжность (Reliability)__
+ Отказоустойчивость — работа при сбое отдельных компонентов;
+ Консистентность данных — отсутствие потерь транзакций.

__6. Наблюдаемость (Observability)__
+ Логирование (ELK stack);
+ Метрики (Prometheus + Grafana);
+ Трассировка (Jaeger, Zipkin).

__7. Сопровождаемость (Maintainability)__
+ Читаемость кода, покрытие тестами;
+ Документация API и архитектурных решений.

__Как фиксировать NFR:__

```
Требование: Время отклика API создания платежа
Метрика: P95 < 300 мс при нагрузке 500 RPS
Измерение: JMeter / Gatling нагрузочные тесты
Мониторинг: Prometheus histogram + Grafana dashboard
```

В банковских проектах NFR часто определяются регуляторными требованиями ЦБ РФ, стандартами PCI DSS и внутренними SLA.

[к оглавлению](#Архитектура-приложений)

## Что такое паттерн Repository?

__Repository (Репозиторий)__ — паттерн, предоставляющий абстракцию для доступа к данным. Репозиторий создаёт иллюзию работы с коллекцией объектов в памяти, скрывая детали хранения (SQL-запросы, ORM, внешние API).

```
Бизнес-логика  ──────▶  Repository (интерфейс)
                               ▲
                               │ реализует
                               │
                    ┌──────────┴──────────┐
                    │   JPA Repository     │
                    │   (инфраструктура)   │
                    └──────────┬──────────┘
                               │
                          ┌────┴────┐
                          │   БД    │
                          └─────────┘
```

__Зачем нужен:__
+ Изолирует бизнес-логику от способа хранения данных;
+ Упрощает тестирование — можно подставить in-memory реализацию;
+ Центральное место для запросов к определённой сущности;
+ Соответствует принципу DIP из SOLID.

__Пример:__

```java
// Интерфейс в доменном слое
public interface PaymentRepository {
    Payment findById(PaymentId id);
    List<Payment> findByStatus(PaymentStatus status);
    void save(Payment payment);
    void delete(PaymentId id);
}

// Реализация в инфраструктурном слое
@Repository
public class JpaPaymentRepository implements PaymentRepository {

    private final PaymentJpaRepo jpaRepo;
    private final PaymentMapper mapper;

    @Override
    public Payment findById(PaymentId id) {
        PaymentEntity entity = jpaRepo.findById(id.getValue())
            .orElseThrow(() -> new PaymentNotFoundException(id));
        return mapper.toDomain(entity);
    }

    @Override
    public void save(Payment payment) {
        PaymentEntity entity = mapper.toEntity(payment);
        jpaRepo.save(entity);
    }

    // ...
}

// Spring Data JPA — упрощённая реализация
public interface PaymentJpaRepo extends JpaRepository<PaymentEntity, Long> {
    List<PaymentEntity> findByStatus(String status);
}
```

__Repository vs DAO:__
+ __DAO (Data Access Object)__ — ориентирован на таблицу БД, работает с записями;
+ __Repository__ — ориентирован на доменный объект (агрегат), работает с бизнес-сущностями.

В DDD репозиторий создаётся __для каждого Aggregate Root__, а не для каждой таблицы.

[к оглавлению](#Архитектура-приложений)

## Что такое паттерн Unit of Work?

__Unit of Work (Единица работы)__ — паттерн, который отслеживает все изменения доменных объектов в рамках одной бизнес-операции и выполняет их в БД как единую атомарную транзакцию.

```
    ┌────────────────────────────────────────────┐
    │             Unit of Work                   │
    │                                            │
    │  Новые:     [Payment A]                    │
    │  Изменённые: [Account X, Account Y]        │
    │  Удалённые:  []                            │
    │                                            │
    │  commit() → INSERT Payment A               │
    │             UPDATE Account X               │
    │             UPDATE Account Y               │
    │             COMMIT                         │
    └────────────────────────────────────────────┘
```

__Зачем нужен:__
+ Группировка нескольких операций в одну транзакцию;
+ Минимизация обращений к БД (batch insert/update);
+ Гарантия целостности данных;
+ Отслеживание изменений (dirty checking).

__В Java/Spring реализуется неявно__ через JPA/Hibernate:

```java
@Service
public class TransferService {

    @Transactional  // Spring начинает Unit of Work
    public void transfer(Long fromId, Long toId, BigDecimal amount) {
        Account from = accountRepository.findById(fromId).orElseThrow();
        Account to = accountRepository.findById(toId).orElseThrow();

        from.withdraw(amount);   // Hibernate отслеживает изменение (dirty)
        to.deposit(amount);      // Hibernate отслеживает изменение (dirty)

        // НЕ нужно вызывать save() — Hibernate при commit()
        // автоматически выполнит UPDATE для всех «грязных» сущностей
    }
    // По завершении метода Spring коммитит транзакцию
    // Hibernate выполняет flush() — записывает все изменения в БД
}
```

__Как работает в Hibernate:__
1. `@Transactional` начинает транзакцию и создаёт Persistence Context;
2. Загруженные сущности отслеживаются (managed state);
3. Изменения сущностей запоминаются в Persistence Context;
4. При коммите (`flush()`) Hibernate сравнивает текущее состояние с исходным (dirty checking) и генерирует SQL;
5. Все SQL выполняются в рамках одной транзакции.

__Persistence Context в JPA — это и есть реализация Unit of Work.__

[к оглавлению](#Архитектура-приложений)

## Как Dependency Injection работает как архитектурный принцип?

__Dependency Injection (DI, внедрение зависимостей)__ — принцип, при котором объект не создаёт свои зависимости сам, а получает их извне (через конструктор, сеттер или поле). На архитектурном уровне DI обеспечивает __инверсию зависимостей__ — модули высокого уровня не зависят от модулей низкого уровня.

```
Без DI (жёсткая связь):              С DI (слабая связь):

PaymentService                        PaymentService
  └── new JpaPaymentRepo()              └── PaymentRepository (интерфейс)
  └── new SmtpNotifier()                          ↑ внедряется извне
                                        JpaPaymentRepo implements PaymentRepository
Сервис ЗНАЕТ о реализациях.           Сервис знает только интерфейс.
Нельзя подменить для теста.           Легко подменить на мок.
```

__Три способа внедрения в Spring:__

```java
// 1. Через конструктор (РЕКОМЕНДУЕТСЯ)
@Service
public class PaymentService {
    private final PaymentRepository repository;
    private final NotificationPort notifier;

    // Spring внедрит реализации автоматически
    public PaymentService(PaymentRepository repository, NotificationPort notifier) {
        this.repository = repository;
        this.notifier = notifier;
    }
}

// 2. Через сеттер
@Service
public class PaymentService {
    private PaymentRepository repository;

    @Autowired
    public void setRepository(PaymentRepository repository) {
        this.repository = repository;
    }
}

// 3. Через поле (НЕ рекомендуется)
@Service
public class PaymentService {
    @Autowired
    private PaymentRepository repository;
}
```

__Почему конструктор предпочтителен:__
+ Зависимости явно видны;
+ Объект неизменяем (final-поля);
+ Не может быть создан без зависимостей (нет NullPointerException);
+ Легко тестировать без Spring-контекста.

__DI на архитектурном уровне:__

DI — это механизм реализации __Dependency Inversion Principle (DIP)__. На уровне архитектуры это означает:

+ __Модуль бизнес-логики__ определяет интерфейсы (порты), которые ему нужны;
+ __Модуль инфраструктуры__ предоставляет реализации (адаптеры);
+ __Конфигурация (Composition Root)__ связывает всё вместе.

```
┌──────────────────────────────┐
│   domain (ядро)              │
│   ├── PaymentService         │
│   ├── PaymentRepository      │  ← интерфейс
│   └── NotificationPort       │  ← интерфейс
└──────────────────────────────┘

┌──────────────────────────────┐
│   infrastructure             │
│   ├── JpaPaymentRepository   │  → implements PaymentRepository
│   └── KafkaNotificationAdapter│ → implements NotificationPort
└──────────────────────────────┘

┌──────────────────────────────┐
│   config (Composition Root)  │
│   └── @Configuration класс   │  ← связывает интерфейсы с реализациями
└──────────────────────────────┘
```

__Преимущества DI как архитектурного принципа:__
+ Модули можно разрабатывать и тестировать независимо;
+ Легко подменять реализации (PostgreSQL → MongoDB, SMTP → Kafka);
+ Обеспечивает тестируемость через мок-объекты;
+ Снижает coupling между модулями;
+ В гексагональной архитектуре DI — ключевой механизм связи портов с адаптерами.

[к оглавлению](#Архитектура-приложений)

## Как спроектировать модуль с нуля? Пошаговый подход.

Этот вопрос часто задают на собеседованиях уровня Middle, особенно в контексте «проектирования будущих модулей». Вот системный подход:

__Шаг 1. Понять бизнес-требования__
+ Какую проблему решает модуль?
+ Кто пользователь (другой сервис, оператор, клиент)?
+ Какие бизнес-правила и ограничения?

__Шаг 2. Определить границы модуля (Bounded Context)__
+ Какие сущности входят?
+ Какие данные принадлежат модулю, а какие — соседнему?
+ Ubiquitous Language — согласовать термины с бизнесом.

__Шаг 3. Определить API (контракт)__
+ Входящие API (REST, gRPC, Message Listener);
+ Исходящие интеграции (другие модули, внешние системы);
+ Формат данных (DTO, Events).

__Шаг 4. Выбрать архитектуру модуля__

```
Простой CRUD-модуль:          Сложная бизнес-логика:
  Controller                    Controller (Adapter In)
  → Service                     → Use Case (Port In)
  → Repository                  → Domain Model
  → Entity                      → Repository (Port Out)
                                → Adapter Out (JPA, Kafka)
```

__Шаг 5. Спроектировать доменную модель__
+ Выделить Entity и Value Objects;
+ Определить Aggregates и их границы;
+ Определить доменные события.

__Шаг 6. Определить нефункциональные требования__
+ Производительность (RPS, latency);
+ Доступность (SLA);
+ Безопасность (авторизация, аудит).

__Шаг 7. Спроектировать хранение данных__
+ Выбор БД (PostgreSQL, MongoDB, Redis);
+ Схема данных, индексы;
+ Кэширование.

__Шаг 8. Спроектировать обработку ошибок__
+ Retry-стратегия;
+ Fallback-поведение;
+ Dead Letter Queue для необработанных сообщений.

__Пример: модуль «Лимиты операций» в банке:__

```
Bounded Context: Лимиты
Entities: OperationLimit, LimitRule
Value Objects: Money, Period

API:
  POST /api/limits          — создать лимит
  GET  /api/limits/{id}     — получить лимит
  POST /api/limits/check    — проверить операцию на лимит

Events:
  LimitExceededEvent → уведомление клиенту
  LimitUpdatedEvent  → обновление кэша

Хранение: PostgreSQL (лимиты) + Redis (кэш текущих остатков)

NFR: latency < 50ms (проверка лимита на hot-path платежа)
```

[к оглавлению](#Архитектура-приложений)

## Что такое Saga-паттерн в распределённых системах?

__Saga__ — паттерн управления распределёнными транзакциями, при котором длинная бизнес-транзакция разбивается на последовательность локальных транзакций. Каждая локальная транзакция обновляет данные в своём сервисе и публикует событие, запускающее следующий шаг. При ошибке выполняются __компенсирующие транзакции__ для отмены уже выполненных шагов.

__Почему нужна Saga:__ в микросервисной архитектуре каждый сервис имеет свою БД. Обычная ACID-транзакция через несколько БД невозможна (или крайне дорогая — 2PC). Saga обеспечивает согласованность в конечном счёте (eventual consistency).

__Пример: перевод между счетами в разных сервисах__

```
Успешный сценарий:

  Сервис Платежей           Сервис Счетов         Сервис Уведомлений
       │                         │                        │
  1. Создать платёж (PENDING)    │                        │
       │────── DebitAccount ────▶│                        │
       │                    2. Списать со счёта           │
       │◀──── AccountDebited ────│                        │
  3. Подтвердить платёж          │                        │
       │─── PaymentCompleted ───▶│───── Notify ──────────▶│
       │                         │               4. Уведомить клиента

Ошибка на шаге 2 (недостаточно средств):

  Сервис Платежей           Сервис Счетов
       │                         │
  1. Создать платёж (PENDING)    │
       │────── DebitAccount ────▶│
       │                    2. ОШИБКА: недостаточно средств
       │◀──── DebitFailed ───────│
  3. Компенсация: отменить платёж (CANCELLED)
```

__Два подхода к реализации:__

__1. Оркестрация (Orchestration)__ — центральный Saga Orchestrator управляет последовательностью шагов:

```java
@Service
public class TransferSagaOrchestrator {

    public void execute(TransferCommand cmd) {
        try {
            // Шаг 1: списание
            accountService.debit(cmd.getFromAccountId(), cmd.getAmount());
            // Шаг 2: зачисление
            accountService.credit(cmd.getToAccountId(), cmd.getAmount());
            // Шаг 3: уведомление
            notificationService.notify(cmd);
        } catch (CreditFailedException e) {
            // Компенсация шага 1
            accountService.reverseDebit(cmd.getFromAccountId(), cmd.getAmount());
            throw new TransferFailedException(e);
        }
    }
}
```

__2. Хореография (Choreography)__ — каждый сервис слушает события и сам решает, что делать:

```
PaymentCreated → Сервис Счетов слушает → списывает → AccountDebited
AccountDebited → Сервис Платежей слушает → подтверждает → PaymentCompleted
PaymentCompleted → Сервис Уведомлений слушает → уведомляет
```

| Аспект | Оркестрация | Хореография |
|--------|-------------|-------------|
| Управление | Центральный координатор | Децентрализованное |
| Сложность | Логика в одном месте | Размазана по сервисам |
| Связанность | Orchestrator знает все шаги | Сервисы знают только свои события |
| Отладка | Проще (один поток) | Сложнее (нужна трассировка) |
| Масштабирование | Orchestrator — узкое место | Лучше масштабируется |

__В банковских системах__ Saga — обязательный паттерн для распределённых платёжных операций, где нужна компенсация при сбоях.

[к оглавлению](#Архитектура-приложений)
