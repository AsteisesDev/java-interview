[Вопросы для собеседования](README.md)

# Spring Framework
+ [Что такое Spring Framework и зачем он нужен?](#Что-такое-Spring-Framework-и-зачем-он-нужен)
+ [Что такое IoC и DI? В чём разница между ними?](#Что-такое-IoC-и-DI-В-чём-разница-между-ними)
+ [Что такое Spring-контейнер? В чём разница между BeanFactory и ApplicationContext?](#Что-такое-Spring-контейнер-В-чём-разница-между-BeanFactory-и-ApplicationContext)
+ [Что такое Spring Bean?](#Что-такое-Spring-Bean)
+ [Каков жизненный цикл Spring Bean?](#Каков-жизненный-цикл-Spring-Bean)
+ [Какие существуют scope (области видимости) у Spring Bean?](#Какие-существуют-scope-области-видимости-у-Spring-Bean)
+ [Для чего нужны аннотации @Component, @Service, @Repository и @Controller?](#Для-чего-нужны-аннотации-Component-Service-Repository-и-Controller)
+ [Чем отличаются @Configuration и @Component? Что делает аннотация @Bean?](#Чем-отличаются-Configuration-и-Component-Что-делает-аннотация-Bean)
+ [Как работает @Autowired? Какие существуют способы внедрения зависимостей?](#Как-работает-Autowired-Какие-существуют-способы-внедрения-зависимостей)
+ [Для чего нужны @Qualifier и @Primary?](#Для-чего-нужны-Qualifier-и-Primary)
+ [Как использовать @Value и @PropertySource?](#Как-использовать-Value-и-PropertySource)
+ [Что такое профили в Spring? Как работает аннотация @Profile?](#Что-такое-профили-в-Spring-Как-работает-аннотация-Profile)
+ [Что такое Spring AOP? Какие основные понятия в AOP?](#Что-такое-Spring-AOP-Какие-основные-понятия-в-AOP)
+ [Какие типы Advice существуют в Spring AOP?](#Какие-типы-Advice-существуют-в-Spring-AOP)
+ [Как работает @Transactional в Spring?](#Как-работает-Transactional-в-Spring)
+ [Какие уровни propagation (распространения транзакций) существуют?](#Какие-уровни-propagation-распространения-транзакций-существуют)
+ [Какие уровни isolation (изоляции транзакций) существуют?](#Какие-уровни-isolation-изоляции-транзакций-существуют)
+ [Какие подводные камни есть у @Transactional?](#Какие-подводные-камни-есть-у-Transactional)
+ [Что такое Spring Boot и чем он отличается от Spring Framework?](#Что-такое-Spring-Boot-и-чем-он-отличается-от-Spring-Framework)
+ [Как работает автоконфигурация в Spring Boot?](#Как-работает-автоконфигурация-в-Spring-Boot)
+ [Что такое Spring Boot Starter-ы?](#Что-такое-Spring-Boot-Starter-ы)
+ [Как устроена конфигурация в application.properties / application.yml?](#Как-устроена-конфигурация-в-applicationproperties--applicationyml)
+ [Что такое Spring Boot Actuator?](#Что-такое-Spring-Boot-Actuator)
+ [Как работает встроенный сервер в Spring Boot?](#Как-работает-встроенный-сервер-в-Spring-Boot)
+ [Что такое Spring Boot DevTools?](#Что-такое-Spring-Boot-DevTools)
+ [Что такое Spring Data JPA?](#Что-такое-Spring-Data-JPA)
+ [В чём разница между Repository, CrudRepository и JpaRepository?](#В-чём-разница-между-Repository-CrudRepository-и-JpaRepository)
+ [Что такое query methods (derived queries) в Spring Data?](#Что-такое-query-methods-derived-queries-в-Spring-Data)
+ [Как использовать @Query, JPQL и native queries?](#Как-использовать-Query-JPQL-и-native-queries)
+ [Как реализовать пагинацию и сортировку в Spring Data JPA?](#Как-реализовать-пагинацию-и-сортировку-в-Spring-Data-JPA)
+ [Какие основные аннотации JPA для маппинга сущностей вы знаете?](#Какие-основные-аннотации-JPA-для-маппинга-сущностей-вы-знаете)
+ [Как маппить связи между сущностями? В чём разница между LAZY и EAGER загрузкой?](#Как-маппить-связи-между-сущностями-В-чём-разница-между-LAZY-и-EAGER-загрузкой)
+ [Что такое проблема N+1 и как её решить?](#Что-такое-проблема-N1-и-как-её-решить)
+ [Как работает @Transactional в контексте JPA?](#Как-работает-Transactional-в-контексте-JPA)
+ [Как устроена архитектура Spring MVC? Что такое DispatcherServlet?](#Как-устроена-архитектура-Spring-MVC-Что-такое-DispatcherServlet)
+ [В чём разница между @Controller и @RestController?](#В-чём-разница-между-Controller-и-RestController)
+ [Как работают аннотации маппинга запросов: @RequestMapping, @GetMapping, @PostMapping?](#Как-работают-аннотации-маппинга-запросов-RequestMapping-GetMapping-PostMapping)
+ [Для чего нужны @RequestParam, @PathVariable, @RequestBody и @ResponseBody?](#Для-чего-нужны-RequestParam-PathVariable-RequestBody-и-ResponseBody)
+ [Что такое ResponseEntity и когда его использовать?](#Что-такое-ResponseEntity-и-когда-его-использовать)
+ [Как обрабатывать исключения в Spring MVC? Что такое @ExceptionHandler и @ControllerAdvice?](#Как-обрабатывать-исключения-в-Spring-MVC-Что-такое-ExceptionHandler-и-ControllerAdvice)
+ [Как работает валидация в Spring? Что такое @Valid и @Validated?](#Как-работает-валидация-в-Spring-Что-такое-Valid-и-Validated)
+ [Что такое Spring Security?](#Что-такое-Spring-Security)
+ [В чём разница между аутентификацией и авторизацией?](#В-чём-разница-между-аутентификацией-и-авторизацией)
+ [Что такое SecurityFilterChain и как он работает?](#Что-такое-SecurityFilterChain-и-как-он-работает)
+ [Как работают аннотации @PreAuthorize и @Secured?](#Как-работают-аннотации-PreAuthorize-и-Secured)
+ [Как настроить аутентификацию на основе JWT в Spring Security?](#Как-настроить-аутентификацию-на-основе-JWT-в-Spring-Security)
+ [Что такое Spring Event (события в Spring)? Как создать и обработать событие?](#Что-такое-Spring-Event-события-в-Spring-Как-создать-и-обработать-событие)
+ [Что такое @Conditional и условная конфигурация в Spring Boot?](#Что-такое-Conditional-и-условная-конфигурация-в-Spring-Boot)
+ [Как тестировать Spring-приложения? Какие основные аннотации для тестирования?](#Как-тестировать-Spring-приложения-Какие-основные-аннотации-для-тестирования)
+ [Что такое Spring Cache и как его использовать?](#Что-такое-Spring-Cache-и-как-его-использовать)

## Что такое Spring Framework и зачем он нужен?

**Spring Framework** — это комплексный фреймворк для разработки Java-приложений, который предоставляет инфраструктуру для упрощения создания enterprise-приложений. Spring берёт на себя управление «сантехникой» приложения, позволяя разработчикам сосредоточиться на бизнес-логике.

**Основные причины использования Spring:**

1. **Инверсия управления (IoC)** — фреймворк управляет созданием объектов и их зависимостями, избавляя разработчика от ручного управления.
2. **Модульность** — Spring состоит из множества модулей (Core, MVC, Data, Security и др.), можно использовать только нужные.
3. **Упрощение работы с базами данных** — абстракции над JDBC, JPA, интеграция с Hibernate.
4. **Декларативное управление транзакциями** — аннотация `@Transactional` вместо ручного управления.
5. **AOP (аспектно-ориентированное программирование)** — позволяет отделить сквозную функциональность (логирование, безопасность, транзакции).
6. **Мощная экосистема** — Spring Boot, Spring Cloud, Spring Security, Spring Batch и др.
7. **Тестируемость** — IoC делает код легко тестируемым через подмену зависимостей (моки).

**Основные модули Spring Framework:**

- **Spring Core** — IoC-контейнер, DI
- **Spring AOP** — аспектно-ориентированное программирование
- **Spring MVC** — веб-фреймворк
- **Spring Data** — работа с базами данных
- **Spring Security** — безопасность
- **Spring TX** — управление транзакциями

[к оглавлению](#Spring-Framework)

## Что такое IoC и DI? В чём разница между ними?

**IoC (Inversion of Control, инверсия управления)** — это принцип проектирования, при котором управление созданием объектов и их жизненным циклом передаётся от самого приложения к фреймворку (контейнеру). Вместо того чтобы объект сам создавал свои зависимости через `new`, он получает их извне.

**DI (Dependency Injection, внедрение зависимостей)** — это конкретный способ (паттерн) реализации принципа IoC. DI подразумевает, что зависимости объекта «внедряются» в него контейнером, а не создаются внутри самого объекта.

**Соотношение:** IoC — это принцип (философия), DI — это способ его реализации. DI — наиболее распространённая форма IoC.

**Без IoC/DI (жёсткая связность):**

```java
public class OrderService {
    // OrderService сам создаёт свою зависимость — жёсткая связь
    private OrderRepository repository = new OrderRepositoryImpl();

    public void createOrder(Order order) {
        repository.save(order);
    }
}
```

**С IoC/DI (слабая связность):**

```java
@Service
public class OrderService {
    private final OrderRepository repository;

    // Зависимость внедряется контейнером через конструктор
    @Autowired
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }

    public void createOrder(Order order) {
        repository.save(order);
    }
}
```

**Преимущества DI:**
- **Слабая связность** — классы зависят от интерфейсов, а не от конкретных реализаций.
- **Тестируемость** — в тестах легко подставить мок вместо реальной зависимости.
- **Гибкость** — можно менять реализации без изменения кода клиента.
- **Единая точка конфигурации** — зависимости описаны в одном месте.

**Другие формы IoC (кроме DI):**
- **Service Locator** — объект сам запрашивает зависимости у специального реестра.
- **Template Method** — управление потоком передаётся родительскому классу.
- **Event-driven** — управление через события.

[к оглавлению](#Spring-Framework)

## Что такое Spring-контейнер? В чём разница между BeanFactory и ApplicationContext?

**Spring-контейнер** (IoC-контейнер) — это ядро Spring Framework, которое отвечает за создание объектов (бинов), их конфигурирование, сборку и управление их жизненным циклом. Контейнер использует метаданные конфигурации (аннотации, XML, Java-конфигурация) для определения того, какие объекты создавать и как их связывать.

**BeanFactory** — это базовый интерфейс контейнера, обеспечивающий минимальную функциональность:
- Создание и управление бинами
- Поддержка DI
- **Ленивая (lazy) инициализация** — бины создаются только при первом запросе

**ApplicationContext** — расширенный интерфейс, наследующий `BeanFactory` и добавляющий:
- **Жадная (eager) инициализация** — все singleton-бины создаются при запуске контейнера
- **Интернационализация (i18n)** — поддержка `MessageSource`
- **Публикация событий** — `ApplicationEventPublisher`
- **Загрузка ресурсов** — `ResourceLoader`
- **Интеграция с AOP**
- **Поддержка профилей и Environment**

```java
// BeanFactory — базовый (редко используется напрямую)
BeanFactory factory = new XmlBeanFactory(new ClassPathResource("beans.xml"));
MyBean bean = factory.getBean(MyBean.class);

// ApplicationContext — стандартный выбор
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

// Для аннотаций:
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

// Spring Boot создаёт ApplicationContext автоматически:
SpringApplication.run(MyApplication.class, args);
```

**Основные реализации ApplicationContext:**
- `AnnotationConfigApplicationContext` — для Java-конфигурации
- `ClassPathXmlApplicationContext` — для XML-конфигурации из classpath
- `FileSystemXmlApplicationContext` — для XML из файловой системы
- `WebApplicationContext` — для веб-приложений

**Частая ошибка:** использование `BeanFactory` вместо `ApplicationContext`. В современных приложениях всегда следует использовать `ApplicationContext`, так как он предоставляет все необходимые функции. `BeanFactory` оправдан только в средах с крайне ограниченными ресурсами.

[к оглавлению](#Spring-Framework)

## Что такое Spring Bean?

**Spring Bean** — это объект, который создаётся, управляется и конфигурируется IoC-контейнером Spring. По сути, это обычный Java-объект (POJO), жизненный цикл которого контролируется контейнером.

**Способы объявления бинов:**

**1. Аннотации на классе (Component Scanning):**

```java
@Component
public class MyBean {
    // ...
}

// Необходимо включить сканирование компонентов:
@Configuration
@ComponentScan("com.example")
public class AppConfig {
}
```

**2. Java-конфигурация (@Bean):**

```java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }

    @Bean
    public MyRepository myRepository() {
        return new MyRepositoryImpl();
    }
}
```

**3. XML-конфигурация (устаревший подход):**

```xml
<bean id="myService" class="com.example.MyServiceImpl"/>
```

**Ключевые характеристики бина:**
- **id/имя** — уникальный идентификатор бина в контейнере
- **Класс** — конкретный класс объекта
- **Scope** — область видимости (singleton, prototype и др.)
- **Зависимости** — другие бины, которые ему нужны
- **Callbacks** — методы инициализации и уничтожения

**Частая ошибка:** создание объекта вручную через `new` вместо получения его из контейнера Spring. Если создать объект через `new`, Spring не будет управлять им, аннотации `@Autowired`, `@Transactional` и другие не будут работать.

```java
// НЕПРАВИЛЬНО — Spring не управляет этим объектом
MyService service = new MyService();

// ПРАВИЛЬНО — получаем бин из контейнера
@Autowired
private MyService service;
```

[к оглавлению](#Spring-Framework)

## Каков жизненный цикл Spring Bean?

Жизненный цикл Spring Bean — это последовательность этапов от создания до уничтожения объекта в контейнере.

**Полный жизненный цикл (для singleton-бина):**

1. **Инстанцирование** — контейнер создаёт экземпляр бина (через конструктор)
2. **Установка свойств** — внедряются зависимости (через сеттеры, поля)
3. **BeanNameAware.setBeanName()** — если бин реализует `BeanNameAware`
4. **BeanFactoryAware.setBeanFactory()** — если реализует `BeanFactoryAware`
5. **ApplicationContextAware.setApplicationContext()** — если реализует `ApplicationContextAware`
6. **BeanPostProcessor.postProcessBeforeInitialization()** — для всех `BeanPostProcessor`
7. **@PostConstruct** — вызов метода, помеченного аннотацией
8. **InitializingBean.afterPropertiesSet()** — если реализует `InitializingBean`
9. **init-method** — вызов пользовательского метода инициализации
10. **BeanPostProcessor.postProcessAfterInitialization()** — для всех `BeanPostProcessor` (здесь могут создаваться прокси, например, для `@Transactional`)
11. **Бин готов к использованию**
12. **@PreDestroy** — при закрытии контейнера
13. **DisposableBean.destroy()** — если реализует `DisposableBean`
14. **destroy-method** — вызов пользовательского метода уничтожения

```java
@Component
public class MyBean implements InitializingBean, DisposableBean {

    @Autowired
    private SomeDependency dependency;

    @PostConstruct
    public void postConstruct() {
        System.out.println("3. @PostConstruct — после внедрения зависимостей");
    }

    @Override
    public void afterPropertiesSet() {
        System.out.println("4. InitializingBean.afterPropertiesSet()");
    }

    @PreDestroy
    public void preDestroy() {
        System.out.println("6. @PreDestroy — перед уничтожением");
    }

    @Override
    public void destroy() {
        System.out.println("7. DisposableBean.destroy()");
    }
}
```

**BeanPostProcessor** — мощный механизм, позволяющий вмешаться в процесс создания каждого бина. Именно через `BeanPostProcessor` работают многие аннотации Spring (`@Autowired`, `@Transactional`, `@Async` и др.):

```java
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) {
        // вызывается перед @PostConstruct
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        // вызывается после инициализации
        // здесь можно обернуть бин в прокси
        return bean;
    }
}
```

**Частая ошибка:** использование `@Autowired`-зависимостей в конструкторе. На момент вызова конструктора зависимости, внедряемые через поля или сеттеры, ещё не установлены. Для инициализации, зависящей от внедрённых зависимостей, следует использовать `@PostConstruct` или внедрение через конструктор.

[к оглавлению](#Spring-Framework)

## Какие существуют scope (области видимости) у Spring Bean?

**Scope** определяет, сколько экземпляров бина будет создано и как они будут разделяться.

**Основные scope:**

| Scope | Описание |
|-------|----------|
| **singleton** | (по умолчанию) Один экземпляр на весь контейнер |
| **prototype** | Новый экземпляр при каждом запросе бина |
| **request** | Один экземпляр на HTTP-запрос (только для веб) |
| **session** | Один экземпляр на HTTP-сессию (только для веб) |
| **application** | Один экземпляр на `ServletContext` (только для веб) |
| **websocket** | Один экземпляр на WebSocket-сессию |

```java
@Component
@Scope("singleton") // можно не указывать — это значение по умолчанию
public class SingletonBean { }

@Component
@Scope("prototype")
public class PrototypeBean { }

@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestScopedBean { }
```

**Singleton vs Prototype:**

```java
// Singleton — каждый раз возвращается один и тот же объект
MyBean bean1 = context.getBean(MyBean.class);
MyBean bean2 = context.getBean(MyBean.class);
System.out.println(bean1 == bean2); // true

// Prototype — каждый раз создаётся новый объект
PrototypeBean proto1 = context.getBean(PrototypeBean.class);
PrototypeBean proto2 = context.getBean(PrototypeBean.class);
System.out.println(proto1 == proto2); // false
```

**Частая ошибка:** внедрение prototype-бина в singleton. Так как singleton создаётся один раз, зависимость (prototype-бин) тоже будет внедрена один раз и больше не изменится:

```java
@Component // singleton по умолчанию
public class SingletonService {

    @Autowired
    private PrototypeBean prototypeBean; // ПРОБЛЕМА: всегда один и тот же экземпляр!
}
```

**Решения проблемы singleton-prototype:**

```java
// 1. Через ObjectFactory / ObjectProvider
@Component
public class SingletonService {
    @Autowired
    private ObjectProvider<PrototypeBean> prototypeBeanProvider;

    public void doWork() {
        PrototypeBean bean = prototypeBeanProvider.getObject(); // каждый раз новый
    }
}

// 2. Через scoped proxy
@Component
@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class PrototypeBean { }

// 3. Через lookup method
@Component
public abstract class SingletonService {
    @Lookup
    public abstract PrototypeBean getPrototypeBean();
}
```

[к оглавлению](#Spring-Framework)

## Для чего нужны аннотации @Component, @Service, @Repository и @Controller?

Все эти аннотации являются **стереотипными аннотациями (stereotype annotations)** и помечают класс как Spring-бин, чтобы контейнер мог обнаружить его при сканировании компонентов (component scanning).

**@Component** — базовая аннотация, обозначающая класс как Spring-компонент. Остальные три — это специализации `@Component`:

**@Service** — для классов бизнес-логики (сервисный слой). Семантически обозначает, что класс содержит бизнес-логику.

**@Repository** — для классов работы с данными (слой доступа к данным). Помимо семантики, Spring добавляет **автоматическую трансляцию исключений**: специфичные исключения JDBC/JPA/Hibernate автоматически оборачиваются в Spring `DataAccessException`.

**@Controller** — для контроллеров Spring MVC (обработка HTTP-запросов).

```java
@Component // общий компонент
public class EmailValidator { }

@Service // бизнес-логика
public class UserService {
    @Autowired
    private UserRepository userRepository;
}

@Repository // доступ к данным
public class UserRepository {
    @PersistenceContext
    private EntityManager em;
}

@Controller // веб-контроллер
public class UserController {
    @Autowired
    private UserService userService;
}
```

**Важно:** с точки зрения IoC-контейнера все четыре аннотации работают одинаково — регистрируют бин. Различия:
- `@Repository` — добавляет трансляцию исключений
- `@Controller` — включает обработку маппинга запросов
- `@Service` — чисто семантическая, дополнительного поведения не добавляет

**Частая ошибка:** использование `@Component` для всех классов без разбора. Хотя технически это работает, правильная аннотация улучшает читаемость кода и может давать дополнительную функциональность (как трансляция исключений в `@Repository`).

[к оглавлению](#Spring-Framework)

## Чем отличаются @Configuration и @Component? Что делает аннотация @Bean?

**@Configuration** — аннотация, указывающая, что класс содержит определения бинов (методы `@Bean`). Она является специализацией `@Component`, но имеет принципиальное отличие.

**@Bean** — аннотация, которая помечает метод как фабричный. Возвращаемый объект метода регистрируется как бин в контейнере.

**Ключевое отличие @Configuration от @Component:**

Класс с `@Configuration` обрабатывается через **CGLIB-прокси**. Это означает, что вызовы методов `@Bean` внутри конфигурационного класса перехватываются, и контейнер гарантирует, что будет возвращён **тот же самый singleton-экземпляр**.

```java
@Configuration
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        return new HikariDataSource();
    }

    @Bean
    public UserRepository userRepository() {
        // Здесь dataSource() НЕ создаст новый объект,
        // а вернёт тот же singleton-бин благодаря CGLIB-прокси
        return new UserRepository(dataSource());
    }

    @Bean
    public OrderRepository orderRepository() {
        // И здесь тоже будет тот же DataSource
        return new OrderRepository(dataSource());
    }
}
```

**Если использовать @Component вместо @Configuration:**

```java
@Component // БЕЗ CGLIB-прокси!
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        return new HikariDataSource();
    }

    @Bean
    public UserRepository userRepository() {
        // ВНИМАНИЕ: dataSource() создаст НОВЫЙ экземпляр DataSource!
        return new UserRepository(dataSource());
    }
}
```

Это называется **lite mode** — методы `@Bean` вызываются как обычные Java-методы, без перехвата CGLIB.

**Параметры @Bean:**

```java
@Configuration
public class AppConfig {

    @Bean(name = "myService")           // имя бина
    @Bean(initMethod = "init")          // метод инициализации
    @Bean(destroyMethod = "cleanup")    // метод уничтожения
    @Scope("prototype")                 // scope бина
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

**Частая ошибка:** использование `@Component` вместо `@Configuration` для классов с несколькими `@Bean`-методами, которые ссылаются друг на друга. Это приводит к созданию множественных экземпляров вместо одного singleton.

[к оглавлению](#Spring-Framework)

## Как работает @Autowired? Какие существуют способы внедрения зависимостей?

**@Autowired** — аннотация Spring, которая автоматически внедряет зависимости по типу (by type). Если найден ровно один бин подходящего типа, он будет внедрён.

**Три способа внедрения зависимостей:**

### 1. Через конструктор (рекомендуемый)

```java
@Service
public class UserService {
    private final UserRepository userRepository;
    private final EmailService emailService;

    // Начиная с Spring 4.3, @Autowired можно не указывать,
    // если в классе один конструктор
    public UserService(UserRepository userRepository, EmailService emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
}
```

### 2. Через сеттер

```java
@Service
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

### 3. Через поле (не рекомендуется)

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

**Почему внедрение через конструктор предпочтительнее:**

1. **Неизменяемость (immutability)** — зависимости можно объявить `final`.
2. **Обязательность** — нельзя создать объект без зависимостей (гарантия корректного состояния).
3. **Тестируемость** — легко передать моки через конструктор без рефлексии.
4. **Видимость проблем** — если конструктор получает слишком много параметров, это сигнал о нарушении принципа единой ответственности (SRP).
5. **Нет рефлексии** — конструктор вызывается стандартным способом.

**Внедрение через поле нежелательно, потому что:**
- Зависимость не может быть `final`
- Для тестирования нужна рефлексия или Spring-контекст
- Скрывает зависимости класса

**Порядок разрешения зависимостей @Autowired:**
1. Ищет бин по типу
2. Если найдено несколько бинов — ищет по имени поля/параметра
3. Если не найден — бросает `NoSuchBeanDefinitionException`
4. Если найдено несколько и имя не совпадает — `NoUniqueBeanDefinitionException`

```java
// Параметр required = false позволяет необязательную зависимость:
@Autowired(required = false)
private Optional<CacheService> cacheService;

// Или через Optional (Spring 5+):
public UserService(Optional<CacheService> cacheService) {
    this.cacheService = cacheService.orElse(null);
}
```

**Частая ошибка:** циклические зависимости. Если BeanA зависит от BeanB, а BeanB от BeanA, при внедрении через конструктор Spring выбросит `BeanCurrentlyInCreationException`. Решения: рефакторинг, `@Lazy` на одной из зависимостей или переход на сеттер-внедрение.

[к оглавлению](#Spring-Framework)

## Для чего нужны @Qualifier и @Primary?

Когда в контейнере есть несколько бинов одного типа, Spring не может определить, какой из них внедрить. Аннотации `@Qualifier` и `@Primary` решают эту проблему неоднозначности.

**@Qualifier** — указывает конкретное имя бина, который нужно внедрить:

```java
public interface NotificationService {
    void send(String message);
}

@Service("emailNotification")
public class EmailNotificationService implements NotificationService {
    public void send(String message) { /* отправка email */ }
}

@Service("smsNotification")
public class SmsNotificationService implements NotificationService {
    public void send(String message) { /* отправка SMS */ }
}

@Service
public class OrderService {
    private final NotificationService notificationService;

    // Указываем конкретный бин
    public OrderService(@Qualifier("emailNotification") NotificationService notificationService) {
        this.notificationService = notificationService;
    }
}
```

**@Primary** — помечает бин как «предпочтительный» по умолчанию:

```java
@Service
@Primary // будет внедряться по умолчанию, если не указан @Qualifier
public class EmailNotificationService implements NotificationService {
    public void send(String message) { /* отправка email */ }
}

@Service
public class SmsNotificationService implements NotificationService {
    public void send(String message) { /* отправка SMS */ }
}

@Service
public class OrderService {
    // Внедрится EmailNotificationService, т.к. он @Primary
    public OrderService(NotificationService notificationService) {
        // ...
    }
}

@Service
public class AlertService {
    // А здесь явно выбираем SMS, несмотря на @Primary
    public AlertService(@Qualifier("smsNotificationService") NotificationService notificationService) {
        // ...
    }
}
```

**Приоритет:** `@Qualifier` > `@Primary` > совпадение по имени > ошибка.

**Внедрение всех реализаций:**

```java
@Service
public class NotificationManager {
    private final List<NotificationService> services;

    // Spring внедрит ВСЕ бины типа NotificationService
    public NotificationManager(List<NotificationService> services) {
        this.services = services;
    }

    public void notifyAll(String message) {
        services.forEach(s -> s.send(message));
    }
}
```

**Частая ошибка:** использование `@Qualifier` с жёстко прописанным строковым именем бина вместо создания собственных аннотаций-квалификаторов. Строковые значения не проверяются на этапе компиляции, что приводит к ошибкам во время выполнения. Лучше создать свой `@Qualifier`:

```java
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.PARAMETER, ElementType.TYPE})
public @interface Email { }

@Service
@Email
public class EmailNotificationService implements NotificationService { }

// Использование:
public OrderService(@Email NotificationService notificationService) { }
```

[к оглавлению](#Spring-Framework)

## Как использовать @Value и @PropertySource?

**@Value** — аннотация для внедрения значений из внешних источников (файлы свойств, переменные окружения, системные свойства) в поля, параметры методов или конструкторов бинов.

**@PropertySource** — указывает файл свойств, из которого Spring будет загружать значения.

```java
@Configuration
@PropertySource("classpath:application.properties")
@PropertySource("classpath:db.properties") // можно указать несколько
public class AppConfig {
}
```

Файл `application.properties`:
```properties
app.name=MyApp
app.version=1.0
server.port=8080
db.url=jdbc:postgresql://localhost:5432/mydb
db.pool.size=10
```

**Основные способы использования @Value:**

```java
@Component
public class AppSettings {

    // Простое значение из properties
    @Value("${app.name}")
    private String appName;

    // Значение по умолчанию (если свойство не найдено)
    @Value("${app.description:Default Description}")
    private String description;

    // Числовые значения (автоматическое преобразование типов)
    @Value("${server.port}")
    private int serverPort;

    // Системная переменная окружения
    @Value("${JAVA_HOME}")
    private String javaHome;

    // SpEL (Spring Expression Language)
    @Value("#{T(java.lang.Math).random() * 100}")
    private double randomValue;

    // SpEL — обращение к другому бину
    @Value("#{someBean.someProperty}")
    private String valueFromBean;

    // Литеральное значение
    @Value("Hello, World!")
    private String greeting;

    // Внедрение в параметр конструктора
    public AppSettings(@Value("${db.url}") String dbUrl) {
        // ...
    }
}
```

**Работа со списками:**

```properties
# application.properties
app.servers=server1,server2,server3
```

```java
@Value("${app.servers}")
private List<String> servers;

@Value("${app.servers}")
private String[] serversArray;
```

**Лучшая практика — @ConfigurationProperties (Spring Boot):**

Для группы связанных свойств лучше использовать `@ConfigurationProperties`:

```java
@Configuration
@ConfigurationProperties(prefix = "db")
public class DatabaseProperties {
    private String url;
    private int poolSize;

    // геттеры и сеттеры обязательны
    public String getUrl() { return url; }
    public void setUrl(String url) { this.url = url; }
    public int getPoolSize() { return poolSize; }
    public void setPoolSize(int poolSize) { this.poolSize = poolSize; }
}
```

**Частая ошибка:** использование `@Value` в статических полях — это не работает, так как Spring внедряет значения только в экземплярные (нестатические) поля. Также частая ошибка — забыть значение по умолчанию: если свойство не найдено и нет значения по умолчанию, приложение не запустится.

[к оглавлению](#Spring-Framework)

## Что такое профили в Spring? Как работает аннотация @Profile?

**Профили (Profiles)** — это механизм Spring, позволяющий регистрировать бины условно, в зависимости от активного окружения (dev, test, prod). Это позволяет иметь разные конфигурации для разных окружений без изменения кода.

**Объявление бина с профилем:**

```java
@Configuration
@Profile("dev")
public class DevDataSourceConfig {
    @Bean
    public DataSource dataSource() {
        // встроенная H2 для разработки
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
    }
}

@Configuration
@Profile("prod")
public class ProdDataSourceConfig {
    @Bean
    public DataSource dataSource() {
        HikariDataSource ds = new HikariDataSource();
        ds.setJdbcUrl("jdbc:postgresql://prod-server:5432/mydb");
        return ds;
    }
}

// Бин, который НЕ будет создан в prod
@Service
@Profile("!prod")
public class MockEmailService implements EmailService {
    // заглушка для разработки/тестов
}
```

**Способы активации профиля:**

```properties
# 1. В application.properties
spring.profiles.active=dev

# 2. Через аргумент командной строки
# java -jar app.jar --spring.profiles.active=prod

# 3. Через переменную окружения
# SPRING_PROFILES_ACTIVE=prod
```

```java
// 4. Программно
SpringApplication app = new SpringApplication(MyApp.class);
app.setAdditionalProfiles("dev");
app.run(args);

// 5. В тестах
@ActiveProfiles("test")
@SpringBootTest
public class MyServiceTest { }
```

**Файлы свойств для профилей (Spring Boot):**

```
application.properties            // общие свойства
application-dev.properties        // свойства для профиля dev
application-prod.properties       // свойства для профиля prod
application-test.properties       // свойства для профиля test
```

Свойства из файла профиля перезаписывают значения из общего `application.properties`.

**Логические операторы в профилях (Spring 5.1+):**

```java
@Profile("dev & local")    // И
@Profile("dev | staging")  // ИЛИ
@Profile("!prod")          // НЕ
```

**Частая ошибка:** забыть активировать профиль. Если профиль не активирован, бины, помеченные `@Profile("dev")`, не будут созданы. Бины без аннотации `@Profile` создаются всегда, независимо от активного профиля.

[к оглавлению](#Spring-Framework)

## Что такое Spring AOP? Какие основные понятия в AOP?

**AOP (Aspect-Oriented Programming, аспектно-ориентированное программирование)** — парадигма программирования, позволяющая выделять **сквозную функциональность** (cross-cutting concerns) в отдельные модули. Сквозная функциональность — это логика, которая пронизывает множество модулей: логирование, транзакции, безопасность, кэширование, обработка ошибок.

**Основные понятия AOP:**

| Понятие | Описание |
|---------|----------|
| **Aspect (Аспект)** | Модуль, инкапсулирующий сквозную функциональность (класс с `@Aspect`) |
| **Advice (Совет)** | Действие, выполняемое аспектом в определённой точке |
| **Join Point (Точка соединения)** | Место в коде, где может быть применён advice (в Spring — всегда вызов метода) |
| **Pointcut (Срез)** | Выражение, определяющее, к каким join point применяется advice |
| **Target object** | Объект, к которому применяется аспект |
| **Proxy** | Объект-обёртка, создаваемый Spring для перехвата вызовов |
| **Weaving** | Процесс связывания аспектов с целевыми объектами (в Spring — runtime weaving через прокси) |

**Пример аспекта:**

```java
@Aspect
@Component
public class LoggingAspect {

    // Pointcut — определяет, ГДЕ применяется
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {}

    // Advice — определяет, ЧТО и КОГДА выполняется
    @Before("serviceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        System.out.println("Вызов метода: " + methodName + ", аргументы: " + Arrays.toString(args));
    }
}
```

**Синтаксис Pointcut выражений:**

```java
// Все методы в пакете service
@Pointcut("execution(* com.example.service.*.*(..))")

// Все публичные методы
@Pointcut("execution(public * *(..))")

// Методы, возвращающие void
@Pointcut("execution(void com.example..*.*(..))")

// Методы с определённой аннотацией
@Pointcut("@annotation(com.example.Loggable)")

// Все методы бинов с аннотацией @Service
@Pointcut("within(@org.springframework.stereotype.Service *)")

// Конкретный бин
@Pointcut("bean(userService)")
```

**Как работает AOP в Spring:**

Spring AOP основан на **прокси-объектах**. При создании бина, к которому применяются аспекты, Spring создаёт прокси-обёртку:
- **JDK Dynamic Proxy** — если бин реализует интерфейс
- **CGLIB Proxy** — если бин не реализует интерфейс (создаётся подкласс)

**Частая ошибка:** ожидание, что AOP сработает при вызове метода внутри того же класса (self-invocation). Поскольку Spring AOP основан на прокси, вызов `this.someMethod()` обходит прокси, и аспект не срабатывает:

```java
@Service
public class MyService {
    @Loggable
    public void methodA() { }

    public void methodB() {
        this.methodA(); // AOP НЕ сработает — вызов идёт напрямую, минуя прокси
    }
}
```

[к оглавлению](#Spring-Framework)

## Какие типы Advice существуют в Spring AOP?

В Spring AOP существует пять типов advice (советов):

**1. @Before** — выполняется до вызова метода:

```java
@Before("execution(* com.example.service.*.*(..))")
public void beforeAdvice(JoinPoint joinPoint) {
    System.out.println("До вызова: " + joinPoint.getSignature().getName());
}
```

**2. @AfterReturning** — выполняется после успешного возврата метода:

```java
@AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
public void afterReturningAdvice(JoinPoint joinPoint, Object result) {
    System.out.println("Метод вернул: " + result);
}
```

**3. @AfterThrowing** — выполняется после выброса исключения:

```java
@AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "ex")
public void afterThrowingAdvice(JoinPoint joinPoint, Exception ex) {
    System.out.println("Исключение в " + joinPoint.getSignature().getName() + ": " + ex.getMessage());
}
```

**4. @After (finally)** — выполняется всегда, после метода (аналог `finally`):

```java
@After("execution(* com.example.service.*.*(..))")
public void afterAdvice(JoinPoint joinPoint) {
    System.out.println("Метод завершён (в любом случае): " + joinPoint.getSignature().getName());
}
```

**5. @Around** — самый мощный, оборачивает вызов метода целиком:

```java
@Around("execution(* com.example.service.*.*(..))")
public Object aroundAdvice(ProceedingJoinPoint pjp) throws Throwable {
    long start = System.currentTimeMillis();
    try {
        Object result = pjp.proceed(); // вызов оригинального метода
        return result;
    } catch (Exception e) {
        // обработка ошибки
        throw e;
    } finally {
        long elapsed = System.currentTimeMillis() - start;
        System.out.println(pjp.getSignature().getName() + " выполнен за " + elapsed + " мс");
    }
}
```

**Практический пример — замер времени выполнения через собственную аннотацию:**

```java
// Аннотация
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Timed { }

// Аспект
@Aspect
@Component
public class TimingAspect {

    private static final Logger log = LoggerFactory.getLogger(TimingAspect.class);

    @Around("@annotation(Timed)")
    public Object measureTime(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.nanoTime();
        Object result = pjp.proceed();
        long duration = (System.nanoTime() - start) / 1_000_000;
        log.info("{}.{} выполнен за {} мс",
                pjp.getTarget().getClass().getSimpleName(),
                pjp.getSignature().getName(),
                duration);
        return result;
    }
}

// Использование
@Service
public class UserService {
    @Timed
    public User findById(Long id) {
        return userRepository.findById(id).orElseThrow();
    }
}
```

**Порядок выполнения advice:** @Around (начало) -> @Before -> метод -> @AfterReturning/@AfterThrowing -> @After -> @Around (конец).

**Частая ошибка в @Around:** забыть вызвать `pjp.proceed()` — тогда оригинальный метод вообще не выполнится. Или забыть вернуть результат `proceed()` — метод вернёт `null`.

[к оглавлению](#Spring-Framework)

## Как работает @Transactional в Spring?

**@Transactional** — аннотация, которая декларативно управляет транзакциями. Spring оборачивает вызов метода в транзакцию: начинает перед выполнением и коммитит после успешного завершения (или откатывает при исключении).

**Механизм работы:**

1. Spring создаёт **прокси** вокруг бина с `@Transactional`
2. При вызове метода прокси:
   - Открывает транзакцию (через `PlatformTransactionManager`)
   - Вызывает оригинальный метод
   - При успехе — `commit`
   - При `RuntimeException` / `Error` — `rollback`
   - При checked exception — **commit** (по умолчанию!)

```java
@Service
public class TransferService {

    @Autowired
    private AccountRepository accountRepository;

    @Transactional
    public void transfer(Long fromId, Long toId, BigDecimal amount) {
        Account from = accountRepository.findById(fromId).orElseThrow();
        Account to = accountRepository.findById(toId).orElseThrow();

        from.setBalance(from.getBalance().subtract(amount));
        to.setBalance(to.getBalance().add(amount));

        accountRepository.save(from);
        accountRepository.save(to);
        // если здесь произойдёт исключение — все изменения откатятся
    }
}
```

**Основные параметры @Transactional:**

```java
@Transactional(
    propagation = Propagation.REQUIRED,        // тип распространения
    isolation = Isolation.READ_COMMITTED,       // уровень изоляции
    timeout = 30,                               // таймаут в секундах
    readOnly = true,                            // только для чтения (оптимизация)
    rollbackFor = Exception.class,              // откат при checked exception
    noRollbackFor = BusinessException.class     // НЕ откатывать при этом исключении
)
public void myMethod() { }
```

**readOnly = true:**

```java
@Transactional(readOnly = true)
public List<User> findAllUsers() {
    return userRepository.findAll();
}
```
Флаг `readOnly` — это подсказка, позволяющая оптимизировать работу: Hibernate отключает dirty checking, база может использовать read-реплику.

**rollbackFor:**

По умолчанию Spring откатывает транзакцию только при **unchecked** исключениях (`RuntimeException` и `Error`). Для checked исключений нужно явно указать:

```java
@Transactional(rollbackFor = Exception.class)
public void processPayment() throws PaymentException {
    // при PaymentException (checked) транзакция тоже откатится
}
```

**Частая ошибка:** полагать, что `@Transactional` откатит транзакцию при checked exception. По умолчанию checked exception вызывает commit! Для банковских операций это критически важно — всегда указывайте `rollbackFor = Exception.class`.

[к оглавлению](#Spring-Framework)

## Какие уровни propagation (распространения транзакций) существуют?

**Propagation** определяет, как метод с `@Transactional` ведёт себя относительно уже существующей транзакции.

| Propagation | Описание |
|-------------|----------|
| **REQUIRED** (по умолчанию) | Если транзакция есть — присоединиться. Если нет — создать новую |
| **REQUIRES_NEW** | Всегда создавать новую транзакцию, текущую приостановить |
| **SUPPORTS** | Если транзакция есть — присоединиться. Если нет — работать без транзакции |
| **NOT_SUPPORTED** | Выполнять без транзакции, текущую приостановить |
| **MANDATORY** | Обязательно должна быть транзакция, иначе — исключение |
| **NEVER** | Обязательно НЕ должно быть транзакции, иначе — исключение |
| **NESTED** | Если есть транзакция — создать вложенную (savepoint). Если нет — как REQUIRED |

**Примеры:**

```java
@Service
public class OrderService {

    @Autowired
    private PaymentService paymentService;
    @Autowired
    private AuditService auditService;

    @Transactional // REQUIRED по умолчанию
    public void createOrder(Order order) {
        orderRepository.save(order);
        paymentService.processPayment(order);  // присоединится к той же транзакции
        auditService.logAction("ORDER_CREATED"); // REQUIRES_NEW — своя транзакция
    }
}

@Service
public class PaymentService {
    @Transactional(propagation = Propagation.REQUIRED)
    public void processPayment(Order order) {
        // работает в той же транзакции, что и createOrder()
        // если здесь ошибка — откатится ВСЯ транзакция, включая order
    }
}

@Service
public class AuditService {
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logAction(String action) {
        // всегда в НОВОЙ транзакции
        // даже если основная транзакция откатится, лог сохранится
    }
}
```

**NESTED:**

```java
@Service
public class BatchService {
    @Transactional
    public void processBatch(List<Item> items) {
        for (Item item : items) {
            try {
                itemService.processItem(item); // NESTED
            } catch (Exception e) {
                // откатится только обработка этого item (savepoint)
                // остальные продолжат обработку
            }
        }
    }
}

@Service
public class ItemService {
    @Transactional(propagation = Propagation.NESTED)
    public void processItem(Item item) {
        // ...
    }
}
```

**Частая ошибка:** путать `REQUIRED` и `REQUIRES_NEW`. При `REQUIRED` — если внутренний метод выбросит исключение, откатится вся внешняя транзакция. При `REQUIRES_NEW` — откатится только внутренняя транзакция (внешняя может обработать исключение и продолжить).

[к оглавлению](#Spring-Framework)

## Какие уровни isolation (изоляции транзакций) существуют?

**Isolation** определяет, насколько транзакция изолирована от изменений, вносимых другими параллельными транзакциями.

**Проблемы параллельного доступа:**

| Проблема | Описание |
|----------|----------|
| **Dirty Read** | Чтение данных, которые ещё не зафиксированы другой транзакцией |
| **Non-Repeatable Read** | Повторное чтение тех же данных даёт другой результат (другая транзакция их изменила и зафиксировала) |
| **Phantom Read** | Повторный запрос возвращает другое количество строк (другая транзакция добавила/удалила строки) |

**Уровни изоляции:**

| Isolation | Dirty Read | Non-Repeatable Read | Phantom Read |
|-----------|------------|---------------------|--------------|
| **READ_UNCOMMITTED** | Возможно | Возможно | Возможно |
| **READ_COMMITTED** | Нет | Возможно | Возможно |
| **REPEATABLE_READ** | Нет | Нет | Возможно |
| **SERIALIZABLE** | Нет | Нет | Нет |
| **DEFAULT** | Зависит от БД | | |

```java
// READ_COMMITTED — стандарт для большинства приложений (PostgreSQL по умолчанию)
@Transactional(isolation = Isolation.READ_COMMITTED)
public void updateBalance(Long accountId, BigDecimal amount) {
    // видим только зафиксированные данные
}

// REPEATABLE_READ — для критически важных финансовых операций
@Transactional(isolation = Isolation.REPEATABLE_READ)
public void transfer(Long fromId, Long toId, BigDecimal amount) {
    // повторное чтение баланса внутри транзакции гарантирует тот же результат
}

// SERIALIZABLE — максимальная изоляция, минимальная производительность
@Transactional(isolation = Isolation.SERIALIZABLE)
public void criticalOperation() {
    // полная изоляция, как будто транзакции выполняются последовательно
}
```

**Для банковских приложений** обычно используется `READ_COMMITTED` (по умолчанию для PostgreSQL) или `REPEATABLE_READ` — баланс между производительностью и безопасностью.

**Частая ошибка:** использование `SERIALIZABLE` повсеместно «для надёжности» — это сильно снижает производительность из-за блокировок. Следует выбирать минимально достаточный уровень изоляции.

[к оглавлению](#Spring-Framework)

## Какие подводные камни есть у @Transactional?

Это один из самых популярных вопросов на собеседованиях, и здесь есть множество нюансов.

### 1. Self-invocation (вызов из того же класса)

Самая частая проблема. `@Transactional` работает через прокси. При вызове метода внутри того же класса (`this.method()`), прокси обходится, и транзакция не применяется:

```java
@Service
public class UserService {

    @Transactional
    public void updateUser(User user) {
        userRepository.save(user);
    }

    public void processUser(Long userId) {
        User user = userRepository.findById(userId).orElseThrow();
        user.setStatus("PROCESSED");
        this.updateUser(user); // @Transactional НЕ сработает!
    }
}
```

**Решения:**
```java
// 1. Вынести в другой сервис
@Service
public class UserProcessingService {
    @Autowired
    private UserService userService;

    public void processUser(Long userId) {
        User user = userRepository.findById(userId).orElseThrow();
        user.setStatus("PROCESSED");
        userService.updateUser(user); // теперь вызов через прокси
    }
}

// 2. Внедрить себя (self-injection)
@Service
public class UserService {
    @Autowired
    private UserService self; // Spring внедрит прокси

    public void processUser(Long userId) {
        User user = userRepository.findById(userId).orElseThrow();
        self.updateUser(user); // через прокси — @Transactional работает
    }
}
```

### 2. Checked exceptions не вызывают откат

```java
@Transactional // НЕ откатится при IOException!
public void saveFile(MultipartFile file) throws IOException {
    userRepository.save(user);
    fileStorage.store(file); // бросает IOException — транзакция всё равно коммитится!
}

// Правильно:
@Transactional(rollbackFor = Exception.class)
public void saveFile(MultipartFile file) throws IOException {
    // теперь откатится при любом исключении
}
```

### 3. @Transactional на private методе не работает

```java
@Service
public class UserService {
    @Transactional
    private void internalUpdate() { // НЕ работает — метод private
        // ...
    }
}
```

### 4. Проглатывание исключения внутри транзакции

```java
@Transactional
public void process() {
    try {
        riskyOperation();
    } catch (Exception e) {
        log.error("Error", e);
        // исключение проглочено — транзакция закоммитится,
        // хотя данные могут быть в некорректном состоянии
    }
}
```

### 5. Долгие операции внутри транзакции

```java
@Transactional
public void processOrder(Order order) {
    orderRepository.save(order);
    emailService.sendEmail(order); // долгая операция — блокирует соединение с БД!
    smsService.sendSms(order);
}

// Лучше:
@Transactional
public void processOrder(Order order) {
    orderRepository.save(order);
}
// Отправку уведомлений — вне транзакции или асинхронно
```

### 6. Не указан TransactionManager при нескольких DataSource

```java
@Transactional("secondaryTransactionManager") // явно указываем менеджер
public void saveToSecondaryDb(Data data) {
    secondaryRepository.save(data);
}
```

[к оглавлению](#Spring-Framework)

## Что такое Spring Boot и чем он отличается от Spring Framework?

**Spring Boot** — это надстройка над Spring Framework, созданная для максимального упрощения создания и запуска Spring-приложений. Девиз Spring Boot: **«Convention over Configuration»** (соглашение важнее конфигурации).

**Ключевые отличия:**

| Аспект | Spring Framework | Spring Boot |
|--------|-----------------|-------------|
| Конфигурация | Требует ручной настройки (XML, Java Config) | Автоконфигурация |
| Сервер | Требует внешний сервер (Tomcat, WildFly) | Встроенный сервер |
| Зависимости | Ручной подбор совместимых версий | Starter-ы с готовыми наборами |
| Запуск | WAR на сервере приложений | Исполняемый JAR (`java -jar`) |
| Мониторинг | Настраивать самостоятельно | Spring Boot Actuator |
| Свойства | Ручная загрузка properties | Автоматически `application.properties` |

**Минимальное Spring Boot приложение:**

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

Одна аннотация `@SpringBootApplication` заменяет:
- `@Configuration` — класс является конфигурацией
- `@EnableAutoConfiguration` — включает автоконфигурацию
- `@ComponentScan` — сканирует компоненты в текущем и дочерних пакетах

**Что делает Spring Boot «из коробки»:**
1. Автоматически настраивает `DataSource`, если в classpath есть JDBC-драйвер
2. Автоматически настраивает `EntityManagerFactory`, если есть Hibernate
3. Запускает встроенный Tomcat
4. Настраивает JSON-сериализацию через Jackson
5. Предоставляет `application.properties` для конфигурации
6. Настраивает логирование (Logback по умолчанию)

**Важно понимать:** Spring Boot **не заменяет** Spring Framework. Он строится поверх него, убирая необходимость в шаблонной конфигурации. Под капотом работает тот же Spring.

[к оглавлению](#Spring-Framework)

## Как работает автоконфигурация в Spring Boot?

**Автоконфигурация (Auto-configuration)** — это механизм, при котором Spring Boot автоматически настраивает бины на основании того, какие библиотеки присутствуют в classpath и какие свойства заданы.

**Как это работает:**

1. Аннотация `@EnableAutoConfiguration` (входит в `@SpringBootApplication`) активирует процесс.
2. Spring Boot использует `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` (или `spring.factories` в старых версиях) для обнаружения классов автоконфигурации.
3. Каждый класс автоконфигурации содержит **условия** (`@Conditional`), при которых он применяется.

**Пример — как Spring Boot автоматически настраивает DataSource:**

```java
// Упрощённый пример автоконфигурации (внутри Spring Boot)
@AutoConfiguration
@ConditionalOnClass(DataSource.class) // только если DataSource в classpath
@EnableConfigurationProperties(DataSourceProperties.class)
public class DataSourceAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean // только если пользователь НЕ определил свой DataSource
    public DataSource dataSource(DataSourceProperties properties) {
        return DataSourceBuilder.create()
                .url(properties.getUrl())
                .username(properties.getUsername())
                .password(properties.getPassword())
                .build();
    }
}
```

**Основные условия (@Conditional):**

| Аннотация | Условие |
|-----------|---------|
| `@ConditionalOnClass` | Класс есть в classpath |
| `@ConditionalOnMissingClass` | Класса нет в classpath |
| `@ConditionalOnBean` | Бин уже зарегистрирован |
| `@ConditionalOnMissingBean` | Бин ещё не зарегистрирован |
| `@ConditionalOnProperty` | Свойство имеет определённое значение |
| `@ConditionalOnWebApplication` | Приложение является веб-приложением |

**Принцип работы:** пользовательские бины всегда имеют приоритет над автоконфигурацией. Если вы определили свой `DataSource`, автосконфигурированный не будет создан (благодаря `@ConditionalOnMissingBean`).

**Отключение автоконфигурации:**

```java
// Отключение конкретной автоконфигурации
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class MyApplication { }

// Или через свойства
// spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

**Для отладки автоконфигурации:**

```properties
# Показывает, какие автоконфигурации были применены, а какие нет
debug=true
```

**Частая ошибка:** добавить зависимость (например, `spring-boot-starter-data-jpa`) в проект, но не настроить `spring.datasource.url`. Spring Boot попытается автосконфигурировать DataSource, но не сможет — приложение не запустится.

[к оглавлению](#Spring-Framework)

## Что такое Spring Boot Starter-ы?

**Starter** — это специальная Maven/Gradle-зависимость, которая подтягивает набор связанных библиотек, необходимых для определённой функциональности. Starter-ы устраняют необходимость вручную подбирать совместимые версии зависимостей.

**Основные starter-ы:**

| Starter | Что включает |
|---------|-------------|
| `spring-boot-starter` | Базовый: Spring Core, автоконфигурация, логирование |
| `spring-boot-starter-web` | Spring MVC, встроенный Tomcat, Jackson |
| `spring-boot-starter-data-jpa` | Spring Data JPA, Hibernate, HikariCP |
| `spring-boot-starter-security` | Spring Security |
| `spring-boot-starter-test` | JUnit 5, Mockito, AssertJ, Spring Test |
| `spring-boot-starter-validation` | Hibernate Validator, javax.validation |
| `spring-boot-starter-actuator` | Мониторинг и метрики |
| `spring-boot-starter-cache` | Абстракция кэширования |
| `spring-boot-starter-mail` | Отправка email |
| `spring-boot-starter-amqp` | RabbitMQ |

**Пример в Maven (pom.xml):**

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
</parent>

<dependencies>
    <!-- Веб-приложение -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Работа с БД -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Тестирование -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**Что конкретно подтягивает `spring-boot-starter-web`:**
- `spring-web`, `spring-webmvc` — Spring MVC
- `spring-boot-starter-tomcat` — встроенный Tomcat
- `spring-boot-starter-json` — Jackson для JSON
- `spring-boot-starter` — Spring Core, логирование

**Версии указывать не нужно** — `spring-boot-starter-parent` управляет совместимостью всех версий.

**Создание собственного starter-а** — это продвинутая тема. Собственный starter-а полезен для переиспользования конфигурации между микросервисами в компании.

**Частая ошибка:** добавление зависимостей напрямую (например, `hibernate-core`) вместо starter-а (`spring-boot-starter-data-jpa`). Это может привести к конфликтам версий и отсутствию автоконфигурации.

[к оглавлению](#Spring-Framework)

## Как устроена конфигурация в application.properties / application.yml?

Spring Boot автоматически загружает файл `application.properties` (или `application.yml`) из каталога `src/main/resources`.

**Формат properties:**

```properties
# application.properties
server.port=8080
spring.application.name=my-app

# Подключение к БД
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=admin
spring.datasource.password=secret
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.open-in-view=false

# Логирование
logging.level.root=INFO
logging.level.com.example=DEBUG
logging.level.org.hibernate.SQL=DEBUG
```

**Формат YAML (application.yml):**

```yaml
server:
  port: 8080

spring:
  application:
    name: my-app
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: admin
    password: secret
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: true
    open-in-view: false
```

**Профили конфигурации:**

```properties
# application.properties — общие свойства
spring.profiles.active=dev

# application-dev.properties — только для dev
server.port=8080
spring.datasource.url=jdbc:postgresql://localhost:5432/devdb

# application-prod.properties — только для prod
server.port=80
spring.datasource.url=jdbc:postgresql://prod-server:5432/proddb
```

**Приоритет загрузки свойств (от низшего к высшему):**

1. Значения по умолчанию (`@Value("${prop:default}")`)
2. `application.properties` в JAR
3. `application-{profile}.properties` в JAR
4. `application.properties` вне JAR
5. `application-{profile}.properties` вне JAR
6. Аргументы командной строки (`--server.port=9090`)
7. Переменные окружения (`SERVER_PORT=9090`)

**Типизированная конфигурация с @ConfigurationProperties:**

```java
@Configuration
@ConfigurationProperties(prefix = "app.mail")
@Validated
public class MailProperties {

    @NotBlank
    private String host;
    private int port = 587;
    private String username;
    private String password;

    // геттеры и сеттеры
}
```

```properties
app.mail.host=smtp.gmail.com
app.mail.port=587
app.mail.username=user@gmail.com
app.mail.password=secret
```

**Частая ошибка:** хранение паролей и секретов прямо в `application.properties`, которое попадает в Git. Используйте переменные окружения, Spring Cloud Config или HashiCorp Vault для секретов:

```properties
# Используйте переменные окружения
spring.datasource.password=${DB_PASSWORD}
```

[к оглавлению](#Spring-Framework)

## Что такое Spring Boot Actuator?

**Spring Boot Actuator** — модуль, который предоставляет набор готовых HTTP-эндпоинтов для мониторинга и управления приложением в production.

**Подключение:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**Основные эндпоинты:**

| Эндпоинт | Описание |
|----------|----------|
| `/actuator/health` | Состояние здоровья приложения |
| `/actuator/info` | Информация о приложении |
| `/actuator/metrics` | Метрики (память, CPU, HTTP-запросы) |
| `/actuator/env` | Свойства окружения |
| `/actuator/beans` | Все бины в контейнере |
| `/actuator/mappings` | Все URL-маппинги |
| `/actuator/loggers` | Уровни логирования (можно менять на лету) |
| `/actuator/threaddump` | Дамп потоков |
| `/actuator/heapdump` | Дамп кучи |
| `/actuator/prometheus` | Метрики в формате Prometheus |

**Настройка:**

```properties
# Включить все эндпоинты (по умолчанию открыт только health и info)
management.endpoints.web.exposure.include=*

# Или только конкретные
management.endpoints.web.exposure.include=health,info,metrics,prometheus

# Порт для actuator (отдельный от основного приложения)
management.server.port=9090

# Детальная информация о health
management.endpoint.health.show-details=always
```

**Пример ответа /actuator/health:**

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
        "diskSpace": {
            "status": "UP",
            "details": {
                "total": 499963174912,
                "free": 250000000000
            }
        }
    }
}
```

**Создание собственного health-индикатора:**

```java
@Component
public class ExternalApiHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        boolean isAvailable = checkExternalApi();
        if (isAvailable) {
            return Health.up().withDetail("api", "доступен").build();
        }
        return Health.down().withDetail("api", "недоступен").build();
    }
}
```

**Частая ошибка:** открытие всех эндпоинтов в production без защиты. Эндпоинты вроде `/actuator/env` могут содержать пароли и секреты. Обязательно защищайте Actuator через Spring Security или используйте отдельный порт.

[к оглавлению](#Spring-Framework)

## Как работает встроенный сервер в Spring Boot?

Spring Boot включает **встроенный (embedded) сервер** приложений, что позволяет запускать приложение как обычный JAR-файл без необходимости развёртывания на внешнем сервере.

**Поддерживаемые серверы:**
- **Tomcat** (по умолчанию) — `spring-boot-starter-web`
- **Jetty** — легковесная альтернатива
- **Undertow** — от JBoss, высокая производительность
- **Netty** — для реактивных приложений (`spring-boot-starter-webflux`)

**Смена сервера:**

```xml
<!-- Исключить Tomcat и добавить Jetty -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

**Настройка:**

```properties
# Порт сервера
server.port=8080

# Случайный порт (полезно для тестов)
server.port=0

# Контекстный путь
server.servlet.context-path=/api

# Настройки SSL
server.ssl.enabled=true
server.ssl.key-store=classpath:keystore.p12
server.ssl.key-store-password=secret

# Настройки пула потоков Tomcat
server.tomcat.threads.max=200
server.tomcat.threads.min-spare=10
server.tomcat.max-connections=8192
server.tomcat.accept-count=100

# Таймауты
server.tomcat.connection-timeout=20000
```

**Запуск:**

```bash
# Сборка JAR
mvn clean package

# Запуск
java -jar target/my-app-1.0.0.jar

# С переопределением свойств
java -jar my-app.jar --server.port=9090 --spring.profiles.active=prod
```

**Программная настройка сервера:**

```java
@Component
public class ServerConfig implements WebServerFactoryCustomizer<TomcatServletWebServerFactory> {

    @Override
    public void customize(TomcatServletWebServerFactory factory) {
        factory.setPort(8443);
        factory.addConnectorCustomizers(connector -> {
            connector.setMaxPostSize(10 * 1024 * 1024); // 10 MB
        });
    }
}
```

**Преимущества встроенного сервера:**
- Простота развёртывания — один JAR-файл
- Идеально для микросервисов и Docker-контейнеров
- Единая конфигурация через `application.properties`
- Быстрый старт разработки

[к оглавлению](#Spring-Framework)

## Что такое Spring Boot DevTools?

**Spring Boot DevTools** — модуль, ускоряющий разработку за счёт автоматической перезагрузки приложения, отключения кэширования и удобных настроек по умолчанию.

**Подключение:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

**Ключевые возможности:**

1. **Автоматический рестарт** — при изменении файлов в classpath приложение автоматически перезапускается. DevTools использует два ClassLoader-а: один для неизменяемых зависимостей (JAR-файлы), другой — для классов проекта. Перезагружается только второй, что значительно быстрее полного рестарта.

2. **LiveReload** — автоматическое обновление страницы в браузере при изменении ресурсов (HTML, CSS, JS).

3. **Отключение кэширования** — шаблонизаторы (Thymeleaf, FreeMarker) и другие компоненты работают без кэша.

4. **Расширенное логирование** — автоматическое логирование HTTP-запросов при `DEBUG` уровне.

**Настройки:**

```properties
# Перезагрузка
spring.devtools.restart.enabled=true

# Исключить директории из наблюдения
spring.devtools.restart.exclude=static/**,public/**

# Дополнительные пути для наблюдения
spring.devtools.restart.additional-paths=src/main/resources

# LiveReload
spring.devtools.livereload.enabled=true
```

**Важно:** DevTools **автоматически отключается** при запуске в production (из JAR или при отсутствии на classpath). Атрибут `optional=true` гарантирует, что DevTools не попадёт в зависимости других модулей.

**Частая ошибка:** забыть, что DevTools включён, и удивляться «медленным» перезапускам. Также DevTools может перехватывать горячие клавиши IDE для перезагрузки, что может конфликтовать с другими инструментами.

[к оглавлению](#Spring-Framework)

## Что такое Spring Data JPA?

**Spring Data JPA** — это часть проекта Spring Data, которая значительно упрощает работу с JPA (Java Persistence API), устраняя необходимость в написании шаблонного кода для стандартных операций с базой данных.

**Что решает Spring Data JPA:**

Без Spring Data JPA для простого CRUD нужно писать:

```java
// Без Spring Data JPA — много шаблонного кода
@Repository
public class UserRepositoryImpl {

    @PersistenceContext
    private EntityManager em;

    public User findById(Long id) {
        return em.find(User.class, id);
    }

    public List<User> findAll() {
        return em.createQuery("SELECT u FROM User u", User.class).getResultList();
    }

    public User save(User user) {
        if (user.getId() == null) {
            em.persist(user);
            return user;
        }
        return em.merge(user);
    }

    public void delete(User user) {
        em.remove(em.contains(user) ? user : em.merge(user));
    }
}
```

**С Spring Data JPA — достаточно интерфейса:**

```java
// Всё! Spring Data сгенерирует реализацию автоматически
public interface UserRepository extends JpaRepository<User, Long> {
}
```

**Основные возможности Spring Data JPA:**
1. **Автоматическая реализация репозиториев** — не нужно писать реализацию интерфейсов
2. **Query methods** — генерация запросов по имени метода
3. **Пагинация и сортировка** из коробки
4. **Поддержка @Query** для пользовательских запросов (JPQL и native SQL)
5. **Auditing** — автоматическое заполнение дат создания/обновления
6. **Спецификации (Specifications)** — динамическое построение запросов

**Подключение:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=admin
spring.datasource.password=secret
spring.jpa.hibernate.ddl-auto=validate
```

[к оглавлению](#Spring-Framework)

## В чём разница между Repository, CrudRepository и JpaRepository?

Эти интерфейсы образуют иерархию наследования, каждый следующий добавляет функциональность:

**Repository** — маркерный интерфейс (без методов), обозначающий, что класс является репозиторием.

**CrudRepository** — базовые CRUD-операции:

```java
public interface CrudRepository<T, ID> extends Repository<T, ID> {
    <S extends T> S save(S entity);
    <S extends T> Iterable<S> saveAll(Iterable<S> entities);
    Optional<T> findById(ID id);
    boolean existsById(ID id);
    Iterable<T> findAll();
    long count();
    void deleteById(ID id);
    void delete(T entity);
    void deleteAll();
}
```

**ListCrudRepository** (Spring Data 3.0+) — то же, что `CrudRepository`, но возвращает `List` вместо `Iterable`.

**PagingAndSortingRepository** — добавляет пагинацию и сортировку:

```java
public interface PagingAndSortingRepository<T, ID> extends Repository<T, ID> {
    Iterable<T> findAll(Sort sort);
    Page<T> findAll(Pageable pageable);
}
```

**JpaRepository** — расширяет `ListCrudRepository` и `ListPagingAndSortingRepository`, добавляя JPA-специфичные методы:

```java
public interface JpaRepository<T, ID> extends ListCrudRepository<T, ID>,
                                               ListPagingAndSortingRepository<T, ID> {
    void flush();
    <S extends T> S saveAndFlush(S entity);
    <S extends T> List<S> saveAllAndFlush(Iterable<S> entities);
    void deleteAllInBatch();
    void deleteAllByIdInBatch(Iterable<ID> ids);
    T getReferenceById(ID id);   // возвращает прокси (lazy)
    List<T> findAll();
    List<T> findAll(Sort sort);
    <S extends T> List<S> findAll(Example<S> example);
}
```

**Когда что использовать:**

```java
// Для большинства случаев — JpaRepository (максимум возможностей)
public interface UserRepository extends JpaRepository<User, Long> {
}

// Если нужен минимум — CrudRepository
public interface AuditLogRepository extends CrudRepository<AuditLog, Long> {
}

// Если нужно ограничить набор методов — Repository + свои методы
@NoRepositoryBean
public interface ReadOnlyRepository<T, ID> extends Repository<T, ID> {
    Optional<T> findById(ID id);
    List<T> findAll();
}
```

**Ключевые различия:**

| Метод | CrudRepository | JpaRepository |
|-------|---------------|---------------|
| `findAll()` | `Iterable<T>` | `List<T>` |
| `flush()` | Нет | Есть |
| `saveAndFlush()` | Нет | Есть |
| `deleteInBatch()` | Нет | Есть (эффективнее) |
| `getReferenceById()` | Нет | Есть (lazy proxy) |

**Частая ошибка:** использование `getReferenceById()` вместо `findById()`. Метод `getReferenceById()` возвращает прокси и не выполняет SQL-запрос сразу — при обращении к полям вне транзакции будет `LazyInitializationException`.

[к оглавлению](#Spring-Framework)

## Что такое query methods (derived queries) в Spring Data?

**Query methods (derived queries)** — это механизм автоматической генерации SQL-запросов на основе имени метода в интерфейсе репозитория. Spring Data анализирует имя метода и создаёт соответствующий JPQL-запрос.

**Основные ключевые слова:**

```java
public interface UserRepository extends JpaRepository<User, Long> {

    // SELECT * FROM users WHERE email = ?
    Optional<User> findByEmail(String email);

    // SELECT * FROM users WHERE first_name = ? AND last_name = ?
    List<User> findByFirstNameAndLastName(String firstName, String lastName);

    // SELECT * FROM users WHERE age > ?
    List<User> findByAgeGreaterThan(int age);

    // SELECT * FROM users WHERE age BETWEEN ? AND ?
    List<User> findByAgeBetween(int from, int to);

    // SELECT * FROM users WHERE email LIKE ?
    List<User> findByEmailContaining(String fragment);

    // SELECT * FROM users WHERE email LIKE 'prefix%'
    List<User> findByEmailStartingWith(String prefix);

    // SELECT * FROM users WHERE status IN (?, ?, ?)
    List<User> findByStatusIn(Collection<String> statuses);

    // SELECT * FROM users WHERE deleted = false
    List<User> findByDeletedFalse();

    // SELECT * FROM users WHERE email IS NULL
    List<User> findByEmailIsNull();

    // SELECT * FROM users WHERE email IS NOT NULL
    List<User> findByEmailIsNotNull();

    // С сортировкой: ORDER BY last_name ASC
    List<User> findByStatusOrderByLastNameAsc(String status);

    // Первый результат
    Optional<User> findFirstByOrderByCreatedAtDesc();

    // Топ-5
    List<User> findTop5ByStatusOrderByCreatedAtDesc(String status);

    // Подсчёт
    long countByStatus(String status);

    // Существование
    boolean existsByEmail(String email);

    // Удаление
    void deleteByStatus(String status);

    // Distinct
    List<User> findDistinctByLastName(String lastName);
}
```

**Поддержка вложенных свойств:**

```java
// Навигация по связям: user.address.city
List<User> findByAddressCity(String city);

// Если есть неоднозначность (есть поле addressCity),
// используйте _ для разделения
List<User> findByAddress_City(String city);
```

**Поддержка пагинации в query methods:**

```java
Page<User> findByStatus(String status, Pageable pageable);
Slice<User> findByStatus(String status, Pageable pageable);
List<User> findByStatus(String status, Sort sort);
```

**Частая ошибка:** создание очень длинных имён методов. Если имя метода становится нечитаемым (`findByStatusAndDeletedFalseAndCreatedAtAfterAndRoleIn`), лучше использовать `@Query`:

```java
// Плохо — нечитаемо
List<User> findByStatusAndDeletedFalseAndCreatedAtAfterAndAgeGreaterThan(
    String status, LocalDateTime date, int age);

// Лучше — @Query
@Query("SELECT u FROM User u WHERE u.status = :status AND u.deleted = false " +
       "AND u.createdAt > :date AND u.age > :age")
List<User> findActiveUsersAfterDate(
    @Param("status") String status,
    @Param("date") LocalDateTime date,
    @Param("age") int age);
```

[к оглавлению](#Spring-Framework)

## Как использовать @Query, JPQL и native queries?

Когда query methods недостаточно, Spring Data предоставляет аннотацию `@Query` для написания собственных запросов.

**JPQL (Java Persistence Query Language)** — объектно-ориентированный язык запросов, который оперирует сущностями и их полями, а не таблицами и колонками:

```java
public interface UserRepository extends JpaRepository<User, Long> {

    // JPQL — используем имена сущностей и полей, не таблиц
    @Query("SELECT u FROM User u WHERE u.email = :email")
    Optional<User> findByEmailAddress(@Param("email") String email);

    // Позиционные параметры
    @Query("SELECT u FROM User u WHERE u.firstName = ?1 AND u.lastName = ?2")
    List<User> findByFullName(String firstName, String lastName);

    // JOIN
    @Query("SELECT u FROM User u JOIN u.roles r WHERE r.name = :roleName")
    List<User> findByRoleName(@Param("roleName") String roleName);

    // Агрегация
    @Query("SELECT COUNT(u) FROM User u WHERE u.status = :status")
    long countByStatus(@Param("status") String status);

    // DTO-проекция
    @Query("SELECT new com.example.dto.UserSummary(u.id, u.firstName, u.email) " +
           "FROM User u WHERE u.status = 'ACTIVE'")
    List<UserSummary> findActiveUserSummaries();
}
```

**Native queries (нативный SQL):**

```java
public interface UserRepository extends JpaRepository<User, Long> {

    // Нативный SQL — используем имена таблиц и колонок
    @Query(value = "SELECT * FROM users WHERE email = :email", nativeQuery = true)
    Optional<User> findByEmailNative(@Param("email") String email);

    // Нативный запрос с пагинацией
    @Query(
        value = "SELECT * FROM users WHERE status = :status",
        countQuery = "SELECT COUNT(*) FROM users WHERE status = :status",
        nativeQuery = true
    )
    Page<User> findByStatusNative(@Param("status") String status, Pageable pageable);

    // Сложный нативный запрос
    @Query(value = """
        SELECT u.*, COUNT(o.id) as order_count
        FROM users u
        LEFT JOIN orders o ON o.user_id = u.id
        WHERE u.created_at > :since
        GROUP BY u.id
        HAVING COUNT(o.id) > :minOrders
        """, nativeQuery = true)
    List<User> findActiveCustomers(@Param("since") LocalDateTime since,
                                   @Param("minOrders") int minOrders);
}
```

**Модифицирующие запросы (@Modifying):**

```java
@Modifying
@Transactional
@Query("UPDATE User u SET u.status = :status WHERE u.lastLoginAt < :date")
int deactivateInactiveUsers(@Param("status") String status,
                            @Param("date") LocalDateTime date);

@Modifying
@Transactional
@Query("DELETE FROM User u WHERE u.status = 'DELETED' AND u.deletedAt < :date")
int purgeDeletedUsers(@Param("date") LocalDateTime date);
```

**Когда что использовать:**
- **Query methods** — простые запросы по 1-2 полям
- **JPQL** — запросы средней сложности, JOIN-ы, агрегация, DTO-проекции
- **Native SQL** — сложные запросы, специфичные функции БД, оптимизация производительности

**Частая ошибка:** забыть добавить `@Modifying` к UPDATE/DELETE запросам — Spring выбросит исключение. Также забывают `@Transactional` — модифицирующие запросы требуют транзакции. Ещё одна ошибка — использовать `@Modifying` без `clearAutomatically = true`, из-за чего persistence context содержит устаревшие данные:

```java
@Modifying(clearAutomatically = true) // очистить кэш первого уровня после выполнения
@Transactional
@Query("UPDATE User u SET u.status = 'BLOCKED' WHERE u.id = :id")
void blockUser(@Param("id") Long id);
```

[к оглавлению](#Spring-Framework)

## Как реализовать пагинацию и сортировку в Spring Data JPA?

Spring Data предоставляет удобные абстракции для пагинации и сортировки через интерфейсы `Pageable`, `Sort`, `Page` и `Slice`.

**Сортировка (Sort):**

```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByStatus(String status, Sort sort);
}

// Использование
List<User> users = userRepository.findByStatus("ACTIVE",
    Sort.by(Sort.Direction.ASC, "lastName"));

// Множественная сортировка
Sort sort = Sort.by("lastName").ascending()
               .and(Sort.by("firstName").ascending());
List<User> users = userRepository.findByStatus("ACTIVE", sort);
```

**Пагинация (Pageable, Page):**

```java
public interface UserRepository extends JpaRepository<User, Long> {
    Page<User> findByStatus(String status, Pageable pageable);
}

// Использование (страница 0, размер 20, сортировка по lastName)
Pageable pageable = PageRequest.of(0, 20, Sort.by("lastName").ascending());
Page<User> page = userRepository.findByStatus("ACTIVE", pageable);

// Информация о странице
page.getContent();          // List<User> — данные текущей страницы
page.getTotalElements();    // общее количество элементов
page.getTotalPages();       // общее количество страниц
page.getNumber();           // номер текущей страницы (начиная с 0)
page.getSize();             // размер страницы
page.hasNext();             // есть ли следующая страница
page.hasPrevious();         // есть ли предыдущая страница
```

**Slice vs Page:**

`Page` выполняет дополнительный запрос `COUNT(*)` для подсчёта общего количества. `Slice` этого не делает — он только знает, есть ли следующая страница:

```java
// Slice — быстрее, но без totalElements / totalPages
Slice<User> findByStatus(String status, Pageable pageable);
```

**Пагинация в контроллере:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserRepository userRepository;

    @GetMapping
    public Page<UserDto> getUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size,
            @RequestParam(defaultValue = "lastName,asc") String[] sort) {

        Pageable pageable = PageRequest.of(page, size, Sort.by(
            Sort.Order.by(sort[0]).with(Sort.Direction.fromString(sort[1]))
        ));

        return userRepository.findByStatus("ACTIVE", pageable)
                .map(this::toDto);
    }
}
```

**Пагинация с @Query:**

```java
@Query("SELECT u FROM User u WHERE u.department.name = :deptName")
Page<User> findByDepartmentName(@Param("deptName") String deptName, Pageable pageable);
```

**Частая ошибка:** загружать все данные в память и потом делать пагинацию на уровне Java — это полностью уничтожает смысл пагинации. Также частая ошибка — не ограничивать максимальный `size`, позволяя клиенту запросить `size=1000000`.

[к оглавлению](#Spring-Framework)

## Какие основные аннотации JPA для маппинга сущностей вы знаете?

JPA использует аннотации для маппинга Java-классов на таблицы БД.

```java
@Entity // обязательная — помечает класс как JPA-сущность
@Table(name = "users", schema = "public", // имя таблицы
       uniqueConstraints = @UniqueConstraint(columnNames = {"email"}))
public class User {

    @Id // обязательная — первичный ключ
    @GeneratedValue(strategy = GenerationType.IDENTITY) // автогенерация
    private Long id;

    @Column(name = "first_name", nullable = false, length = 100)
    private String firstName;

    @Column(name = "last_name", nullable = false, length = 100)
    private String lastName;

    @Column(unique = true, nullable = false)
    private String email;

    @Column(precision = 19, scale = 2) // для BigDecimal
    private BigDecimal balance;

    @Enumerated(EnumType.STRING) // сохранять как строку, а не ordinal
    @Column(nullable = false)
    private UserStatus status;

    @Temporal(TemporalType.TIMESTAMP) // для java.util.Date
    private Date createdAt;

    // Для Java 8+ Date API аннотация @Temporal не нужна
    private LocalDateTime updatedAt;

    @Lob // для больших объектов (TEXT, BLOB)
    private String description;

    @Transient // НЕ маппится на колонку в БД
    private String temporaryField;

    @Embedded // встроенный объект (разбивает на колонки)
    private Address address;

    // конструктор без аргументов обязателен для JPA
    protected User() { }

    // геттеры и сеттеры
}

@Embeddable
public class Address {
    private String city;
    private String street;
    @Column(name = "zip_code")
    private String zipCode;
}
```

**Стратегии генерации ID (@GeneratedValue):**

| Strategy | Описание |
|----------|----------|
| `IDENTITY` | Автоинкремент БД (PostgreSQL SERIAL, MySQL AUTO_INCREMENT) |
| `SEQUENCE` | Последовательность БД (рекомендуется для PostgreSQL) |
| `TABLE` | Специальная таблица для генерации ID (медленно, не рекомендуется) |
| `AUTO` | Провайдер выбирает стратегию сам (по умолчанию) |

```java
// Рекомендуемый вариант для PostgreSQL
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
@SequenceGenerator(name = "user_seq", sequenceName = "users_id_seq", allocationSize = 50)
private Long id;
```

**Частая ошибка:**
1. Использовать `@Enumerated(EnumType.ORDINAL)` (по умолчанию) — при добавлении нового значения в enum порядок нарушится, данные в БД станут некорректными. Всегда используйте `EnumType.STRING`.
2. Забыть конструктор без аргументов — JPA не сможет создать экземпляр сущности.
3. Использование `GenerationType.AUTO` с Hibernate, который может создать таблицу `hibernate_sequence` вместо использования нативной последовательности БД.

[к оглавлению](#Spring-Framework)

## Как маппить связи между сущностями? В чём разница между LAZY и EAGER загрузкой?

JPA поддерживает четыре типа связей, каждая задаётся соответствующей аннотацией.

**@ManyToOne / @OneToMany (самая распространённая связь):**

```java
@Entity
@Table(name = "orders")
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Много заказов → один пользователь
    @ManyToOne(fetch = FetchType.LAZY) // EAGER по умолчанию для @ManyToOne!
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Один пользователь → много заказов
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders = new ArrayList<>();

    // Вспомогательные методы для двунаправленной связи
    public void addOrder(Order order) {
        orders.add(order);
        order.setUser(this);
    }

    public void removeOrder(Order order) {
        orders.remove(order);
        order.setUser(null);
    }
}
```

**@ManyToMany:**

```java
@Entity
public class User {

    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(
        name = "user_roles",  // таблица-связка
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}

@Entity
public class Role {

    @ManyToMany(mappedBy = "roles")
    private Set<User> users = new HashSet<>();
}
```

**@OneToOne:**

```java
@Entity
public class User {

    @OneToOne(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JoinColumn(name = "profile_id")
    private UserProfile profile;
}
```

**LAZY vs EAGER загрузка:**

| Тип | Описание | Когда загружается |
|-----|----------|-------------------|
| **LAZY** | Ленивая загрузка | При первом обращении к коллекции/полю |
| **EAGER** | Жадная загрузка | Сразу вместе с родительской сущностью |

**Значения по умолчанию:**
- `@ManyToOne`, `@OneToOne` → **EAGER** (это плохо!)
- `@OneToMany`, `@ManyToMany` → **LAZY** (это хорошо)

**Рекомендация:** всегда явно указывайте `fetch = FetchType.LAZY` для всех связей:

```java
@ManyToOne(fetch = FetchType.LAZY) // явно, хотя по умолчанию EAGER
@JoinColumn(name = "user_id")
private User user;
```

**CascadeType:**

```java
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
private List<Order> orders;
```

| CascadeType | Описание |
|-------------|----------|
| `PERSIST` | Каскадное сохранение |
| `MERGE` | Каскадное обновление |
| `REMOVE` | Каскадное удаление |
| `REFRESH` | Каскадное обновление из БД |
| `DETACH` | Каскадное отсоединение |
| `ALL` | Все вышеперечисленные |

**Частая ошибка:** оставлять `FetchType.EAGER` по умолчанию для `@ManyToOne`. Это приводит к загрузке связанных сущностей при каждом запросе, даже когда они не нужны, и является одной из основных причин проблемы N+1. Также частая ошибка — использовать `CascadeType.ALL` без необходимости, что может привести к случайному каскадному удалению.

[к оглавлению](#Spring-Framework)

## Что такое проблема N+1 и как её решить?

**Проблема N+1** — это ситуация, когда для получения данных выполняется 1 запрос для основной коллекции и N дополнительных запросов для каждого элемента (для загрузки связанных сущностей).

**Пример проблемы:**

```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Order> orders;
}
```

```java
// 1 запрос: SELECT * FROM users
List<User> users = userRepository.findAll();

for (User user : users) {
    // N запросов: SELECT * FROM orders WHERE user_id = ?
    System.out.println(user.getOrders().size()); // для каждого пользователя — отдельный запрос!
}
// Итого: 1 + N запросов. При 1000 пользователей — 1001 запрос!
```

**Решения:**

### 1. JOIN FETCH (JPQL)

```java
public interface UserRepository extends JpaRepository<User, Long> {

    @Query("SELECT u FROM User u JOIN FETCH u.orders WHERE u.status = :status")
    List<User> findByStatusWithOrders(@Param("status") String status);
}
// Выполнится 1 запрос с JOIN
```

### 2. @EntityGraph

```java
public interface UserRepository extends JpaRepository<User, Long> {

    @EntityGraph(attributePaths = {"orders"})
    List<User> findByStatus(String status);

    // Именованный EntityGraph
    @EntityGraph(value = "User.withOrdersAndRoles")
    List<User> findAll();
}

@Entity
@NamedEntityGraph(name = "User.withOrdersAndRoles",
    attributeNodes = {
        @NamedAttributeNode("orders"),
        @NamedAttributeNode("roles")
    })
public class User {
    // ...
}
```

### 3. Batch fetching (настройка Hibernate)

```properties
# Загружать связанные сущности пачками по 25
spring.jpa.properties.hibernate.default_batch_fetch_size=25
```

Или на уровне сущности:

```java
@OneToMany(mappedBy = "user")
@BatchSize(size = 25)
private List<Order> orders;
```

При `@BatchSize(size = 25)` вместо N запросов будет ceil(N/25) запросов с `WHERE user_id IN (?, ?, ..., ?)`.

### 4. DTO-проекция (лучшая производительность)

```java
@Query("SELECT new com.example.dto.UserOrderSummary(u.id, u.name, COUNT(o)) " +
       "FROM User u LEFT JOIN u.orders o " +
       "GROUP BY u.id, u.name")
List<UserOrderSummary> findUserOrderSummaries();
```

**Как обнаружить проблему N+1:**

```properties
# Логирование SQL-запросов
spring.jpa.show-sql=true
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.orm.jdbc.bind=TRACE

# Или использовать spring.jpa.properties.hibernate.generate_statistics=true
```

**Частая ошибка:** использование `JOIN FETCH` с пагинацией — Hibernate загрузит все данные в память и выполнит пагинацию на уровне Java (с предупреждением «HHH90003004: firstResult/maxResults specified with collection fetch; applying in memory»). Решение — использовать подзапрос или `@BatchSize`.

[к оглавлению](#Spring-Framework)

## Как работает @Transactional в контексте JPA?

`@Transactional` в контексте JPA управляет не только транзакциями БД, но и жизненным циклом **Persistence Context** (контекста персистентности, также называемого кэшем первого уровня).

**Persistence Context** — это «рабочая область» JPA, которая хранит загруженные сущности и отслеживает их изменения.

**Основные принципы:**

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    @Transactional
    public void updateUserEmail(Long userId, String newEmail) {
        User user = userRepository.findById(userId).orElseThrow();
        // user сейчас в состоянии MANAGED (управляемый)

        user.setEmail(newEmail);
        // НЕ нужно вызывать save()!
        // Hibernate автоматически обнаружит изменение (dirty checking)
        // и выполнит UPDATE при коммите транзакции
    }
}
```

**Состояния сущности JPA:**

```
NEW (Transient) → persist() → MANAGED → detach()/close() → DETACHED
     ↑                           |                              |
     |                        remove()                       merge()
     |                           ↓                              |
     └─────────────────── REMOVED                               ↓
                                                            MANAGED
```

| Состояние | Описание |
|-----------|----------|
| **NEW (Transient)** | Новый объект, не связан с контекстом и БД |
| **MANAGED** | Управляется контекстом, изменения отслеживаются |
| **DETACHED** | Был управляемым, но контекст закрыт |
| **REMOVED** | Помечен для удаления |

**Dirty Checking:**

```java
@Transactional
public void processUser(Long id) {
    User user = userRepository.findById(id).orElseThrow(); // MANAGED
    user.setName("New Name"); // изменение отслеживается

    // При коммите транзакции Hibernate сравнит текущее состояние
    // с исходным снимком (snapshot) и сгенерирует UPDATE
    // Явный save() не нужен!
}
```

**readOnly и оптимизация:**

```java
@Transactional(readOnly = true)
public User getUser(Long id) {
    // readOnly = true подсказывает Hibernate:
    // 1. Не делать dirty checking (экономия CPU)
    // 2. Не хранить snapshot'ы сущностей (экономия памяти)
    // 3. Потенциально использовать read-реплику БД
    return userRepository.findById(id).orElseThrow();
}
```

**Open Session in View (OSIV):**

По умолчанию в Spring Boot включён паттерн **Open Session in View** (`spring.jpa.open-in-view=true`). Это означает, что Persistence Context открыт на протяжении всего HTTP-запроса (включая рендеринг view). Это позволяет лениво загружать связи в контроллере, но ведёт к проблемам:

```properties
# Рекомендуется отключить
spring.jpa.open-in-view=false
```

**Частая ошибка:** изменять сущность внутри `@Transactional`-метода, не осознавая, что изменения будут автоматически сохранены в БД. Hibernate выполнит `UPDATE` даже без явного вызова `save()`:

```java
@Transactional
public UserDto getUser(Long id) {
    User user = userRepository.findById(id).orElseThrow();
    user.setLastAccessedAt(LocalDateTime.now()); // ВНИМАНИЕ: это вызовет UPDATE!
    return toDto(user);
}
```

[к оглавлению](#Spring-Framework)

## Как устроена архитектура Spring MVC? Что такое DispatcherServlet?

**Spring MVC** — это веб-фреймворк, построенный по паттерну **Front Controller**. Все HTTP-запросы проходят через единую точку входа — `DispatcherServlet`.

**Архитектура обработки запроса:**

```
Клиент
   ↓ HTTP запрос
DispatcherServlet
   ↓
HandlerMapping ──→ определяет, какой контроллер обработает запрос
   ↓
HandlerAdapter ──→ вызывает метод контроллера
   ↓
Controller ──→ выполняет бизнес-логику, возвращает результат
   ↓
ViewResolver ──→ определяет view (для @Controller с шаблонами)
   или
HttpMessageConverter ──→ сериализует в JSON/XML (для @RestController)
   ↓
Клиент ← HTTP ответ
```

**Подробный порядок обработки:**

1. **DispatcherServlet** получает HTTP-запрос
2. **HandlerMapping** находит подходящий обработчик (контроллер) по URL и HTTP-методу
3. **HandlerAdapter** вызывает найденный метод контроллера
4. Контроллер выполняет бизнес-логику и возвращает результат
5. Для `@Controller` — **ViewResolver** находит шаблон и рендерит HTML
6. Для `@RestController` — **HttpMessageConverter** (Jackson) сериализует объект в JSON
7. Ответ отправляется клиенту

**DispatcherServlet** в Spring Boot настраивается автоматически и маппится на `/` (корневой путь). Вся настройка скрыта за автоконфигурацией.

**Регистрация (Spring Boot делает это автоматически):**

```java
// Не нужно в Spring Boot, но полезно знать:
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
        // DispatcherServlet создаётся и регистрируется автоматически
    }
}
```

**Фильтры и интерсепторы:**

```
HTTP запрос → Filter1 → Filter2 → DispatcherServlet → Interceptor.preHandle
                                                            → Controller
                                                       ← Interceptor.postHandle
                                                       ← Interceptor.afterCompletion
              ← Filter2 ← Filter1 ← HTTP ответ
```

```java
@Component
public class LoggingInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                             Object handler) {
        // перед вызовом контроллера
        return true; // true = продолжить, false = прервать
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) {
        // после контроллера, перед рендерингом
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                 Object handler, Exception ex) {
        // после полного завершения обработки
    }
}

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoggingInterceptor())
                .addPathPatterns("/api/**");
    }
}
```

[к оглавлению](#Spring-Framework)

## В чём разница между @Controller и @RestController?

**@Controller** — аннотация для веб-контроллеров Spring MVC, которые возвращают **представления** (HTML-страницы через шаблонизаторы — Thymeleaf, FreeMarker и др.).

**@RestController** — комбинация `@Controller` + `@ResponseBody`. Каждый метод автоматически сериализует возвращаемый объект в тело HTTP-ответа (JSON по умолчанию).

**@Controller (возврат HTML):**

```java
@Controller
public class PageController {

    @GetMapping("/users")
    public String usersPage(Model model) {
        model.addAttribute("users", userService.findAll());
        return "users"; // имя шаблона (templates/users.html)
    }

    // Чтобы вернуть JSON из @Controller, нужно явно добавить @ResponseBody
    @GetMapping("/api/users")
    @ResponseBody
    public List<User> getUsers() {
        return userService.findAll();
    }
}
```

**@RestController (возврат JSON/XML):**

```java
@RestController
@RequestMapping("/api/users")
public class UserRestController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<UserDto> getAllUsers() {
        // автоматически сериализуется в JSON (Jackson)
        return userService.findAll();
    }

    @GetMapping("/{id}")
    public UserDto getUser(@PathVariable Long id) {
        return userService.findById(id);
    }

    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request) {
        UserDto created = userService.create(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
}
```

**Сравнение:**

| Аспект | @Controller | @RestController |
|--------|-------------|-----------------|
| Возвращает | Имя view (String) | Объект (→ JSON) |
| @ResponseBody | Нужно добавить явно | Включено по умолчанию |
| Применение | SSR (Thymeleaf, JSP) | REST API |
| Content-Type | text/html | application/json |

**Для банковских приложений** практически всегда используется `@RestController` для создания REST API, с которым взаимодействует фронтенд (React, Angular) или мобильные приложения.

**Частая ошибка:** использовать `@RestController` и пытаться вернуть имя view (строку) — Spring вернёт эту строку как JSON (`"users"`), а не отрендерит шаблон.

[к оглавлению](#Spring-Framework)

## Как работают аннотации маппинга запросов: @RequestMapping, @GetMapping, @PostMapping?

Эти аннотации связывают HTTP-запросы с методами контроллера.

**@RequestMapping** — универсальная аннотация для маппинга запросов:

```java
@RestController
@RequestMapping("/api/v1/users") // общий префикс для всех методов контроллера
public class UserController {

    @RequestMapping(method = RequestMethod.GET)
    public List<UserDto> findAll() { ... }

    @RequestMapping(value = "/{id}", method = RequestMethod.GET)
    public UserDto findById(@PathVariable Long id) { ... }

    @RequestMapping(method = RequestMethod.POST,
                    consumes = MediaType.APPLICATION_JSON_VALUE,
                    produces = MediaType.APPLICATION_JSON_VALUE)
    public UserDto create(@RequestBody CreateUserRequest request) { ... }
}
```

**Shortcut-аннотации (предпочтительнее):**

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserController {

    @GetMapping                    // GET /api/v1/users
    public List<UserDto> findAll() { ... }

    @GetMapping("/{id}")           // GET /api/v1/users/123
    public UserDto findById(@PathVariable Long id) { ... }

    @PostMapping                   // POST /api/v1/users
    public UserDto create(@RequestBody CreateUserRequest request) { ... }

    @PutMapping("/{id}")           // PUT /api/v1/users/123
    public UserDto update(@PathVariable Long id, @RequestBody UpdateUserRequest request) { ... }

    @PatchMapping("/{id}")         // PATCH /api/v1/users/123
    public UserDto partialUpdate(@PathVariable Long id, @RequestBody Map<String, Object> updates) { ... }

    @DeleteMapping("/{id}")        // DELETE /api/v1/users/123
    public void delete(@PathVariable Long id) { ... }
}
```

**Расширенные параметры маппинга:**

```java
// Маппинг по заголовку
@GetMapping(value = "/users", headers = "X-API-VERSION=2")
public List<UserV2Dto> findAllV2() { ... }

// Маппинг по Content-Type
@PostMapping(value = "/upload", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
public void upload(@RequestParam("file") MultipartFile file) { ... }

// Маппинг по Accept
@GetMapping(value = "/users", produces = MediaType.APPLICATION_XML_VALUE)
public List<UserDto> findAllXml() { ... }

// Маппинг с параметрами запроса
@GetMapping(value = "/users", params = "status")
public List<UserDto> findByStatus(@RequestParam String status) { ... }
```

**Частая ошибка:** маппинг нескольких методов на один и тот же URL + HTTP-метод, что вызывает `IllegalStateException` при старте. Также — использование `@RequestMapping` на классе без указания метода (`method`), что маппит все HTTP-методы.

[к оглавлению](#Spring-Framework)

## Для чего нужны @RequestParam, @PathVariable, @RequestBody и @ResponseBody?

Эти аннотации определяют, откуда Spring извлекает данные запроса и куда помещает данные ответа.

**@PathVariable** — извлекает значение из URL-пути:

```java
// GET /api/users/42
@GetMapping("/api/users/{id}")
public UserDto getUser(@PathVariable Long id) {
    // id = 42
    return userService.findById(id);
}

// Несколько path-переменных
// GET /api/departments/5/users/42
@GetMapping("/api/departments/{deptId}/users/{userId}")
public UserDto getUser(@PathVariable Long deptId, @PathVariable Long userId) { ... }

// С явным указанием имени (если имя параметра метода отличается)
@GetMapping("/api/users/{user-id}")
public UserDto getUser(@PathVariable("user-id") Long userId) { ... }
```

**@RequestParam** — извлекает параметры запроса (query parameters):

```java
// GET /api/users?status=ACTIVE&page=0&size=20
@GetMapping("/api/users")
public Page<UserDto> findUsers(
        @RequestParam String status,                              // обязательный
        @RequestParam(defaultValue = "0") int page,               // с значением по умолчанию
        @RequestParam(defaultValue = "20") int size,
        @RequestParam(required = false) String search) {          // необязательный
    // ...
}
```

**@RequestBody** — десериализует тело запроса (JSON) в Java-объект:

```java
// POST /api/users с телом {"firstName": "Ivan", "email": "ivan@example.com"}
@PostMapping("/api/users")
public UserDto createUser(@RequestBody CreateUserRequest request) {
    // Jackson автоматически десериализует JSON в объект
    return userService.create(request);
}

public class CreateUserRequest {
    private String firstName;
    private String lastName;
    private String email;
    // геттеры и сеттеры
}
```

**@ResponseBody** — сериализует возвращаемый объект в тело ответа (JSON):

```java
@Controller
public class UserController {

    @GetMapping("/api/users")
    @ResponseBody // без этого Spring будет искать view
    public List<UserDto> getUsers() {
        return userService.findAll();
    }
}

// В @RestController @ResponseBody уже включён для всех методов
```

**Комбинирование:**

```java
@PutMapping("/api/departments/{deptId}/users/{userId}")
public ResponseEntity<UserDto> updateUser(
        @PathVariable Long deptId,
        @PathVariable Long userId,
        @RequestParam(required = false) boolean notify,
        @RequestBody @Valid UpdateUserRequest request) {

    UserDto updated = userService.update(deptId, userId, request);
    if (notify) {
        notificationService.notifyUpdate(updated);
    }
    return ResponseEntity.ok(updated);
}
```

**Частая ошибка:** забыть `@RequestBody` для POST/PUT — Spring не сможет десериализовать JSON, и параметр будет `null`. Также частая ошибка — отправлять данные как `form-data` вместо `JSON`, при этом на сервере ожидается `@RequestBody`.

[к оглавлению](#Spring-Framework)

## Что такое ResponseEntity и когда его использовать?

**ResponseEntity** — это класс-обёртка, позволяющая полностью контролировать HTTP-ответ: статус-код, заголовки и тело.

**Без ResponseEntity:**

```java
@GetMapping("/{id}")
public UserDto getUser(@PathVariable Long id) {
    return userService.findById(id); // всегда 200 OK
}
```

**С ResponseEntity:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        return userService.findById(id)
                .map(ResponseEntity::ok)                    // 200 OK
                .orElse(ResponseEntity.notFound().build());  // 404 Not Found
    }

    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request) {
        UserDto created = userService.create(request);
        URI location = URI.create("/api/users/" + created.getId());
        return ResponseEntity
                .created(location)    // 201 Created + заголовок Location
                .body(created);
    }

    @PutMapping("/{id}")
    public ResponseEntity<UserDto> updateUser(@PathVariable Long id,
                                               @Valid @RequestBody UpdateUserRequest request) {
        UserDto updated = userService.update(id, request);
        return ResponseEntity.ok(updated); // 200 OK
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build(); // 204 No Content
    }
}
```

**Установка заголовков:**

```java
@GetMapping("/export")
public ResponseEntity<byte[]> exportUsers() {
    byte[] data = userService.exportToCsv();
    return ResponseEntity.ok()
            .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=users.csv")
            .contentType(MediaType.parseMediaType("text/csv"))
            .contentLength(data.length)
            .body(data);
}
```

**Распространённые статус-коды:**

```java
ResponseEntity.ok(body);                           // 200
ResponseEntity.created(uri).body(body);             // 201
ResponseEntity.accepted().build();                  // 202
ResponseEntity.noContent().build();                 // 204
ResponseEntity.badRequest().body(errorDto);         // 400
ResponseEntity.status(HttpStatus.FORBIDDEN).build(); // 403
ResponseEntity.notFound().build();                  // 404
ResponseEntity.status(HttpStatus.CONFLICT).body(errorDto); // 409
ResponseEntity.internalServerError().body(errorDto); // 500
```

**Когда использовать ResponseEntity:**
- Когда нужно вернуть статус-код, отличный от 200
- Когда нужно установить заголовки ответа
- Для POST-запросов (201 Created + Location header)
- Для условных ответов (200 или 404)

**Когда можно обойтись без ResponseEntity:**
- Если всегда возвращается 200 OK
- Если ошибки обрабатываются через `@ControllerAdvice`

[к оглавлению](#Spring-Framework)

## Как обрабатывать исключения в Spring MVC? Что такое @ExceptionHandler и @ControllerAdvice?

Spring MVC предоставляет гибкий механизм обработки исключений на разных уровнях.

**@ExceptionHandler** — обработка исключений на уровне одного контроллера:

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public UserDto getUser(@PathVariable Long id) {
        return userService.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));
    }

    // Обработчик только для этого контроллера
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        ErrorResponse error = new ErrorResponse("USER_NOT_FOUND", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
}
```

**@ControllerAdvice** — глобальная обработка исключений для всех контроллеров:

```java
@RestControllerAdvice // = @ControllerAdvice + @ResponseBody
public class GlobalExceptionHandler {

    private static final Logger log = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(EntityNotFoundException ex) {
        ErrorResponse error = new ErrorResponse("NOT_FOUND", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException ex) {
        List<String> errors = ex.getBindingResult().getFieldErrors().stream()
                .map(e -> e.getField() + ": " + e.getDefaultMessage())
                .collect(Collectors.toList());

        ErrorResponse error = new ErrorResponse("VALIDATION_ERROR", "Ошибка валидации", errors);
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }

    @ExceptionHandler(AccessDeniedException.class)
    public ResponseEntity<ErrorResponse> handleAccessDenied(AccessDeniedException ex) {
        ErrorResponse error = new ErrorResponse("ACCESS_DENIED", "Доступ запрещён");
        return ResponseEntity.status(HttpStatus.FORBIDDEN).body(error);
    }

    @ExceptionHandler(DataIntegrityViolationException.class)
    public ResponseEntity<ErrorResponse> handleDataIntegrity(DataIntegrityViolationException ex) {
        ErrorResponse error = new ErrorResponse("CONFLICT", "Нарушение целостности данных");
        return ResponseEntity.status(HttpStatus.CONFLICT).body(error);
    }

    // Перехват всех остальных исключений
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        log.error("Неожиданная ошибка", ex);
        ErrorResponse error = new ErrorResponse("INTERNAL_ERROR", "Внутренняя ошибка сервера");
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}
```

**Стандартная модель ошибки:**

```java
public class ErrorResponse {
    private String code;
    private String message;
    private List<String> details;
    private LocalDateTime timestamp = LocalDateTime.now();

    // конструкторы, геттеры
}
```

**Пользовательское исключение:**

```java
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(Long id) {
        super("Пользователь с id=" + id + " не найден");
    }
}
```

**Ограничение области действия @ControllerAdvice:**

```java
// Только для контроллеров в определённом пакете
@RestControllerAdvice(basePackages = "com.example.api")
public class ApiExceptionHandler { ... }

// Только для контроллеров с определённой аннотацией
@RestControllerAdvice(annotations = RestController.class)
public class RestExceptionHandler { ... }
```

**Частая ошибка:** не создавать глобальный обработчик `Exception.class` — при необработанном исключении Spring вернёт стандартное сообщение с деталями (stack trace), которое может раскрыть внутреннюю структуру приложения. Также частая ошибка — возвращать `200 OK` вместе с сообщением об ошибке в теле; всегда используйте правильные HTTP-статусы.

[к оглавлению](#Spring-Framework)

## Как работает валидация в Spring? Что такое @Valid и @Validated?

Spring интегрирует **Bean Validation API (JSR 380)** через Hibernate Validator для декларативной валидации данных.

**Подключение:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

**Основные аннотации валидации:**

```java
public class CreateUserRequest {

    @NotBlank(message = "Имя не может быть пустым")
    @Size(min = 2, max = 100, message = "Имя должно быть от 2 до 100 символов")
    private String firstName;

    @NotBlank(message = "Email обязателен")
    @Email(message = "Некорректный формат email")
    private String email;

    @NotNull(message = "Возраст обязателен")
    @Min(value = 18, message = "Возраст должен быть не менее 18")
    @Max(value = 150, message = "Возраст должен быть не более 150")
    private Integer age;

    @NotNull
    @Positive(message = "Баланс должен быть положительным")
    private BigDecimal balance;

    @Pattern(regexp = "^\\+7\\d{10}$", message = "Формат телефона: +7XXXXXXXXXX")
    private String phone;

    @Past(message = "Дата рождения должна быть в прошлом")
    private LocalDate birthDate;

    @FutureOrPresent(message = "Дата должна быть в будущем или настоящем")
    private LocalDate expirationDate;

    @NotEmpty(message = "Список ролей не может быть пустым")
    private List<@NotBlank String> roles;

    @Valid // для каскадной валидации вложенного объекта
    @NotNull
    private AddressDto address;

    // геттеры и сеттеры
}
```

**Применение валидации в контроллере:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request) {
        // Если валидация не пройдена — MethodArgumentNotValidException (400 Bad Request)
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(userService.create(request));
    }

    // Валидация @PathVariable и @RequestParam
    @GetMapping("/{id}")
    public UserDto getUser(@PathVariable @Positive Long id) {
        return userService.findById(id);
    }
}
```

**Обработка ошибок валидации:**

```java
@RestControllerAdvice
public class ValidationExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, Object>> handleValidation(MethodArgumentNotValidException ex) {
        Map<String, String> fieldErrors = new LinkedHashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
            fieldErrors.put(error.getField(), error.getDefaultMessage()));

        Map<String, Object> body = Map.of(
            "code", "VALIDATION_ERROR",
            "errors", fieldErrors
        );
        return ResponseEntity.badRequest().body(body);
    }
}
```

**@Valid vs @Validated:**

| Аспект | @Valid (javax) | @Validated (Spring) |
|--------|---------------|---------------------|
| Пакет | `javax.validation` | `org.springframework.validation.annotation` |
| Каскадная валидация | Да | Да |
| Группы валидации | Нет | Да |
| На параметрах метода | Да | Да |
| На уровне класса | Нет | Да (для валидации параметров методов) |

**Группы валидации:**

```java
public interface OnCreate {}
public interface OnUpdate {}

public class UserRequest {
    @Null(groups = OnCreate.class, message = "ID не должен быть указан при создании")
    @NotNull(groups = OnUpdate.class, message = "ID обязателен при обновлении")
    private Long id;

    @NotBlank(groups = {OnCreate.class, OnUpdate.class})
    private String name;
}

@PostMapping
public UserDto create(@Validated(OnCreate.class) @RequestBody UserRequest request) { ... }

@PutMapping("/{id}")
public UserDto update(@Validated(OnUpdate.class) @RequestBody UserRequest request) { ... }
```

**Частая ошибка:** забыть `@Valid` перед `@RequestBody` — валидация просто не выполнится, и невалидные данные попадут в метод. Также частая ошибка — не добавить `spring-boot-starter-validation` в зависимости (в Spring Boot 3.x он не входит в `starter-web` по умолчанию).

[к оглавлению](#Spring-Framework)

## Что такое Spring Security?

**Spring Security** — это мощный и настраиваемый фреймворк для обеспечения безопасности Java-приложений. Он обеспечивает аутентификацию (кто ты?), авторизацию (что тебе разрешено?) и защиту от распространённых атак.

**Подключение:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

**Что Spring Security даёт «из коробки»:**
- Все эндпоинты становятся защищёнными (требуют аутентификации)
- Генерирует пароль в консоли при запуске
- Форма логина `/login`
- Защита от CSRF (Cross-Site Request Forgery)
- Защита от Session Fixation
- Заголовки безопасности (X-Frame-Options, X-Content-Type-Options и др.)
- Интеграция с HTTP Basic, Form Login, OAuth2

**Базовая конфигурация (Spring Security 6+, Spring Boot 3+):**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // для REST API обычно отключают CSRF
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()      // доступно всем
                .requestMatchers("/api/admin/**").hasRole("ADMIN")  // только ADMIN
                .requestMatchers("/api/users/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().authenticated()                       // остальное — только для авторизованных
            )
            .httpBasic(Customizer.withDefaults()); // HTTP Basic аутентификация

        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder().encode("password"))
                .roles("USER")
                .build();

        UserDetails admin = User.builder()
                .username("admin")
                .password(passwordEncoder().encode("admin"))
                .roles("ADMIN")
                .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**UserDetailsService для загрузки из БД:**

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        com.example.entity.User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("Пользователь не найден: " + username));

        return org.springframework.security.core.userdetails.User.builder()
                .username(user.getUsername())
                .password(user.getPassword()) // должен быть уже закодирован BCrypt
                .roles(user.getRoles().stream().map(Role::getName).toArray(String[]::new))
                .build();
    }
}
```

**Частая ошибка:** хранить пароли в открытом виде или использовать слабые алгоритмы хеширования (MD5, SHA-1). Всегда используйте `BCryptPasswordEncoder`. Также ошибка — забыть отключить CSRF для REST API без сессий, что приведёт к ошибке 403 при POST-запросах.

[к оглавлению](#Spring-Framework)

## В чём разница между аутентификацией и авторизацией?

Это два фундаментальных понятия информационной безопасности, которые часто путают.

**Аутентификация (Authentication)** — процесс проверки **личности** пользователя. Отвечает на вопрос: **«Кто ты?»**

**Авторизация (Authorization)** — процесс проверки **прав доступа** пользователя. Отвечает на вопрос: **«Что тебе разрешено?»**

**Аналогия:** паспортный контроль в аэропорту — это аутентификация (проверка, что вы — это вы). Проверка наличия визы — это авторизация (имеете ли вы право въехать в страну).

**В Spring Security:**

```
HTTP запрос
     ↓
Аутентификация (AuthenticationFilter)
  "Кто ты?" → проверка логина/пароля или токена
     ↓
Авторизация (AuthorizationFilter)
  "Что тебе можно?" → проверка прав/ролей
     ↓
Контроллер
```

**Аутентификация в Spring Security:**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            // Аутентификация: КАК проверяем пользователя
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/home")
            )
            // ИЛИ HTTP Basic
            .httpBasic(Customizer.withDefaults())

            // Авторизация: ЧТО разрешено
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            );

        return http.build();
    }
}
```

**Виды аутентификации:**
- **По логину и паролю** (Form Login, HTTP Basic)
- **По токену** (JWT, OAuth2)
- **По сертификату** (X.509)
- **Многофакторная** (пароль + SMS-код)

**Виды авторизации:**
- **На основе ролей** (ROLE_ADMIN, ROLE_USER)
- **На основе прав (authorities)** (READ_USERS, WRITE_USERS)
- **На основе выражений** (SpEL: `hasRole('ADMIN') and hasIpAddress('192.168.1.0/24')`)

**Ключевые интерфейсы Spring Security:**
- `Authentication` — представляет аутентифицированного пользователя
- `UserDetails` — информация о пользователе (логин, пароль, роли)
- `UserDetailsService` — загрузка пользователя по имени
- `GrantedAuthority` — право/роль пользователя

**Частая ошибка:** путать `hasRole("ADMIN")` и `hasAuthority("ADMIN")`. Метод `hasRole()` автоматически добавляет префикс `ROLE_`, то есть `hasRole("ADMIN")` эквивалентно `hasAuthority("ROLE_ADMIN")`.

[к оглавлению](#Spring-Framework)

## Что такое SecurityFilterChain и как он работает?

**SecurityFilterChain** — это цепочка фильтров (Servlet Filter), через которую проходит каждый HTTP-запрос в Spring Security. Каждый фильтр в цепочке выполняет определённую задачу безопасности.

**Порядок основных фильтров (упрощённо):**

```
HTTP запрос
     ↓
SecurityContextPersistenceFilter — загрузка SecurityContext (сессия)
     ↓
CsrfFilter — проверка CSRF-токена
     ↓
LogoutFilter — обработка logout
     ↓
UsernamePasswordAuthenticationFilter — аутентификация по логину/паролю
     ↓
BasicAuthenticationFilter — HTTP Basic аутентификация
     ↓
BearerTokenAuthenticationFilter — JWT/OAuth2 аутентификация
     ↓
ExceptionTranslationFilter — обработка ошибок безопасности
     ↓
AuthorizationFilter — проверка прав доступа
     ↓
Контроллер
```

**Конфигурация SecurityFilterChain:**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)) // для JWT
            .authorizeHttpRequests(auth -> auth
                .requestMatchers(HttpMethod.POST, "/api/auth/**").permitAll()
                .requestMatchers(HttpMethod.GET, "/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

**Добавление собственного фильтра:**

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JwtTokenProvider tokenProvider;

    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                     HttpServletResponse response,
                                     FilterChain filterChain) throws ServletException, IOException {

        String token = extractToken(request);
        if (token != null && tokenProvider.validateToken(token)) {
            String username = tokenProvider.getUsernameFromToken(token);
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);

            UsernamePasswordAuthenticationToken authentication =
                    new UsernamePasswordAuthenticationToken(
                            userDetails, null, userDetails.getAuthorities());

            SecurityContextHolder.getContext().setAuthentication(authentication);
        }

        filterChain.doFilter(request, response); // передаём дальше по цепочке
    }

    private String extractToken(HttpServletRequest request) {
        String header = request.getHeader("Authorization");
        if (header != null && header.startsWith("Bearer ")) {
            return header.substring(7);
        }
        return null;
    }
}
```

**Несколько SecurityFilterChain (для разных URL):**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    // Для API — JWT, без сессий
    @Bean
    @Order(1)
    public SecurityFilterChain apiFilterChain(HttpSecurity http) throws Exception {
        http
            .securityMatcher("/api/**")
            .csrf(csrf -> csrf.disable())
            .sessionManagement(s -> s.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
        return http.build();
    }

    // Для веб-страниц — форма логина, с сессиями
    @Bean
    @Order(2)
    public SecurityFilterChain webFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
            .formLogin(Customizer.withDefaults());
        return http.build();
    }
}
```

**Частая ошибка:** забыть вызвать `filterChain.doFilter(request, response)` в собственном фильтре — запрос «застрянет» и не дойдёт до контроллера. Также ошибка — неправильный порядок `@Order` при нескольких `SecurityFilterChain`.

[к оглавлению](#Spring-Framework)

## Как работают аннотации @PreAuthorize и @Secured?

Эти аннотации обеспечивают **авторизацию на уровне методов** (method-level security), позволяя защищать отдельные методы сервисов и контроллеров.

**Включение:**

```java
@Configuration
@EnableMethodSecurity // включает @PreAuthorize, @PostAuthorize, @Secured
public class SecurityConfig {
    // ...
}
```

**@PreAuthorize** — проверка до выполнения метода, поддерживает SpEL-выражения:

```java
@Service
public class UserService {

    // Только ADMIN
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }

    // ADMIN или USER
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')")
    public UserDto getUser(Long id) {
        return userRepository.findById(id).map(this::toDto).orElseThrow();
    }

    // Проверка по authority (без префикса ROLE_)
    @PreAuthorize("hasAuthority('USERS_WRITE')")
    public UserDto createUser(CreateUserRequest request) { ... }

    // Комбинированное выражение
    @PreAuthorize("hasRole('ADMIN') or #userId == authentication.principal.id")
    public UserDto getUserProfile(@Param("userId") Long userId) {
        // ADMIN может видеть любой профиль, обычный пользователь — только свой
        return userService.findById(userId);
    }

    // Проверка параметра метода
    @PreAuthorize("#request.amount <= 10000 or hasRole('ADMIN')")
    public void transfer(TransferRequest request) {
        // обычный пользователь может переводить до 10000, ADMIN — без ограничений
    }

    // Доступ к объекту аутентификации
    @PreAuthorize("authentication.principal.username == #username")
    public UserDto getUserByUsername(String username) { ... }
}
```

**@PostAuthorize** — проверка после выполнения метода (есть доступ к результату):

```java
@PostAuthorize("returnObject.username == authentication.principal.username or hasRole('ADMIN')")
public UserDto getUser(Long id) {
    return userRepository.findById(id).map(this::toDto).orElseThrow();
    // если условие не выполнится — AccessDeniedException
}
```

**@Secured** — более простая альтернатива, без поддержки SpEL:

```java
@Secured("ROLE_ADMIN") // только с префиксом ROLE_
public void deleteUser(Long id) { ... }

@Secured({"ROLE_ADMIN", "ROLE_USER"}) // любая из перечисленных ролей
public List<UserDto> getAllUsers() { ... }
```

**Сравнение @PreAuthorize и @Secured:**

| Аспект | @PreAuthorize | @Secured |
|--------|---------------|----------|
| SpEL | Да | Нет |
| Доступ к параметрам | Да (`#param`) | Нет |
| Комбинирование условий | Да (`and`, `or`) | Нет (только OR между ролями) |
| @PostAuthorize (доступ к результату) | Да | Нет |
| Гибкость | Высокая | Минимальная |

**Получение текущего пользователя:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/me")
    public UserDto getCurrentUser(@AuthenticationPrincipal UserDetails userDetails) {
        return userService.findByUsername(userDetails.getUsername());
    }

    // Или через SecurityContextHolder
    @GetMapping("/me-alt")
    public UserDto getCurrentUserAlt() {
        Authentication auth = SecurityContextHolder.getContext().getAuthentication();
        String username = auth.getName();
        return userService.findByUsername(username);
    }
}
```

**Частая ошибка:** забыть аннотацию `@EnableMethodSecurity` — аннотации `@PreAuthorize` и `@Secured` будут просто проигнорированы. Также частая ошибка — использовать `@PreAuthorize` на private-методах (не работает, так как AOP не может перехватить private-вызовы).

[к оглавлению](#Spring-Framework)

## Как настроить аутентификацию на основе JWT в Spring Security?

**JWT (JSON Web Token)** — компактный и самодостаточный формат токена для безопасной передачи информации. Широко используется для аутентификации в REST API, особенно в банковских приложениях.

**Структура JWT:** `HEADER.PAYLOAD.SIGNATURE`

```
eyJhbGciOiJIUzI1NiJ9.           ← Header (алгоритм)
eyJzdWIiOiJhZG1pbiIsInJvbGVz... ← Payload (данные)
SflKxwRJSMeKKF2QT4fwpMeJf36P... ← Signature (подпись)
```

**Полная реализация JWT-аутентификации:**

**1. Зависимости:**

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
```

**2. JwtTokenProvider — генерация и валидация токенов:**

```java
@Component
public class JwtTokenProvider {

    @Value("${jwt.secret}")
    private String secret;

    @Value("${jwt.expiration-ms}")
    private long expirationMs;

    private SecretKey getSigningKey() {
        return Keys.hmacShaKeyFor(Decoders.BASE64.decode(secret));
    }

    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        claims.put("roles", userDetails.getAuthorities().stream()
                .map(GrantedAuthority::getAuthority)
                .collect(Collectors.toList()));

        return Jwts.builder()
                .claims(claims)
                .subject(userDetails.getUsername())
                .issuedAt(new Date())
                .expiration(new Date(System.currentTimeMillis() + expirationMs))
                .signWith(getSigningKey())
                .compact();
    }

    public String getUsernameFromToken(String token) {
        return Jwts.parser()
                .verifyWith(getSigningKey())
                .build()
                .parseSignedClaims(token)
                .getPayload()
                .getSubject();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser().verifyWith(getSigningKey()).build().parseSignedClaims(token);
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            return false;
        }
    }
}
```

**3. Контроллер аутентификации:**

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    private JwtTokenProvider tokenProvider;

    @PostMapping("/login")
    public ResponseEntity<AuthResponse> login(@RequestBody @Valid LoginRequest request) {
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(request.getUsername(), request.getPassword())
        );
        SecurityContextHolder.getContext().setAuthentication(authentication);

        String token = tokenProvider.generateToken((UserDetails) authentication.getPrincipal());
        return ResponseEntity.ok(new AuthResponse(token));
    }
}
```

**4. Конфигурация Security:**

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {

    @Autowired
    private JwtAuthenticationFilter jwtAuthFilter;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**Частая ошибка:** хранить JWT-секрет в исходном коде. Секрет должен быть в переменных окружения. Также ошибка — не обрабатывать истечение токена (пользователь получает 500 вместо 401). И ещё — делать слишком долгий срок жизни access-токена (рекомендуется 15-30 минут, для обновления — refresh token).

[к оглавлению](#Spring-Framework)

## Что такое Spring Event (события в Spring)? Как создать и обработать событие?

**Spring Events** — это механизм публикации и обработки событий в контексте приложения, реализующий паттерн **Observer (Наблюдатель)**. Позволяет ослабить связность между компонентами.

**Создание и использование события:**

**1. Определение события:**

```java
// Наследование от ApplicationEvent (классический способ)
public class UserRegisteredEvent extends ApplicationEvent {
    private final String email;
    private final String username;

    public UserRegisteredEvent(Object source, String email, String username) {
        super(source);
        this.email = email;
        this.username = username;
    }

    public String getEmail() { return email; }
    public String getUsername() { return username; }
}

// Или просто POJO (Spring 4.2+, рекомендуется)
public class OrderCreatedEvent {
    private final Long orderId;
    private final Long userId;
    private final BigDecimal amount;

    public OrderCreatedEvent(Long orderId, Long userId, BigDecimal amount) {
        this.orderId = orderId;
        this.userId = userId;
        this.amount = amount;
    }
    // геттеры
}
```

**2. Публикация события:**

```java
@Service
public class OrderService {

    @Autowired
    private ApplicationEventPublisher eventPublisher;

    @Transactional
    public Order createOrder(CreateOrderRequest request) {
        Order order = orderRepository.save(new Order(request));

        // Публикация события
        eventPublisher.publishEvent(new OrderCreatedEvent(
                order.getId(), order.getUserId(), order.getAmount()));

        return order;
    }
}
```

**3. Обработка события:**

```java
@Component
public class OrderEventListener {

    // Синхронный обработчик (в той же транзакции!)
    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        log.info("Заказ {} создан на сумму {}", event.getOrderId(), event.getAmount());
    }

    // Асинхронный обработчик (в отдельном потоке)
    @Async
    @EventListener
    public void sendNotification(OrderCreatedEvent event) {
        emailService.sendOrderConfirmation(event.getUserId(), event.getOrderId());
    }

    // Обработчик после коммита транзакции
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void afterOrderCommitted(OrderCreatedEvent event) {
        // выполнится только если транзакция успешно закоммичена
        notificationService.notify(event);
    }

    // Условная обработка
    @EventListener(condition = "#event.amount > 10000")
    public void handleLargeOrder(OrderCreatedEvent event) {
        auditService.flagForReview(event.getOrderId());
    }
}
```

**@TransactionalEventListener** — особенно полезен в банковских приложениях:

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
public void afterCommit(OrderCreatedEvent event) {
    // отправлять уведомление только если заказ реально сохранён в БД
}

@TransactionalEventListener(phase = TransactionPhase.AFTER_ROLLBACK)
public void afterRollback(OrderCreatedEvent event) {
    // логирование неудачных операций
}
```

**Частая ошибка:** использование синхронного `@EventListener` внутри `@Transactional`-метода для долгих операций (отправка email). Обработчик выполнится синхронно, в той же транзакции, что замедлит основную операцию. Используйте `@Async` или `@TransactionalEventListener(phase = AFTER_COMMIT)`.

[к оглавлению](#Spring-Framework)

## Что такое @Conditional и условная конфигурация в Spring Boot?

**@Conditional** — базовая аннотация Spring, позволяющая создавать бины только при выполнении определённых условий. Spring Boot расширяет её множеством специализированных аннотаций.

**Основные условные аннотации Spring Boot:**

```java
@Configuration
public class ConditionalConfig {

    // Бин создаётся, только если класс есть в classpath
    @Bean
    @ConditionalOnClass(name = "com.redis.RedisClient")
    public CacheManager redisCacheManager() {
        return new RedisCacheManager();
    }

    // Бин создаётся, только если НЕТ другого бина этого типа
    @Bean
    @ConditionalOnMissingBean(CacheManager.class)
    public CacheManager defaultCacheManager() {
        return new ConcurrentMapCacheManager();
    }

    // Бин создаётся при определённом значении свойства
    @Bean
    @ConditionalOnProperty(name = "feature.notifications.enabled", havingValue = "true")
    public NotificationService notificationService() {
        return new EmailNotificationService();
    }

    // Бин создаётся, если свойство НЕ задано или равно "true"
    @Bean
    @ConditionalOnProperty(name = "cache.enabled", matchIfMissing = true)
    public CacheManager cacheManager() { ... }

    // Только для веб-приложений
    @Bean
    @ConditionalOnWebApplication
    public WebFilter loggingFilter() { ... }

    // Только если бин определённого типа уже существует
    @Bean
    @ConditionalOnBean(DataSource.class)
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

**Создание собственного условия:**

```java
// Условие
public class OnLinuxCondition implements Condition {

    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        String os = System.getProperty("os.name");
        return os != null && os.toLowerCase().contains("linux");
    }
}

// Использование
@Bean
@Conditional(OnLinuxCondition.class)
public FileWatcher linuxFileWatcher() {
    return new InotifyFileWatcher();
}
```

**Feature toggles (переключатели функциональности) — частый кейс в банках:**

```properties
# application.properties
feature.new-payment-system.enabled=true
feature.sms-notifications.enabled=false
```

```java
@Configuration
public class FeatureConfig {

    @Bean
    @ConditionalOnProperty(name = "feature.new-payment-system.enabled", havingValue = "true")
    public PaymentService newPaymentService() {
        return new NewPaymentService();
    }

    @Bean
    @ConditionalOnProperty(name = "feature.new-payment-system.enabled",
                           havingValue = "false", matchIfMissing = true)
    public PaymentService legacyPaymentService() {
        return new LegacyPaymentService();
    }
}
```

**Частая ошибка:** чрезмерное использование `@Conditional`, превращающее конфигурацию в нечитаемый набор условий. Если логика становится сложной, лучше использовать профили (`@Profile`) или вынести условия в отдельный `@Configuration`-класс.

[к оглавлению](#Spring-Framework)

## Как тестировать Spring-приложения? Какие основные аннотации для тестирования?

Spring Boot предоставляет богатый инструментарий для тестирования на всех уровнях.

**Зависимость:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

Включает: JUnit 5, Mockito, AssertJ, Spring Test, MockMvc, Hamcrest.

### 1. Unit-тесты (без Spring-контекста)

```java
class UserServiceTest {

    private UserService userService;

    @Mock
    private UserRepository userRepository;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
        userService = new UserService(userRepository);
    }

    @Test
    void shouldFindUserById() {
        User user = new User(1L, "Ivan", "ivan@mail.ru");
        when(userRepository.findById(1L)).thenReturn(Optional.of(user));

        UserDto result = userService.findById(1L);

        assertThat(result.getName()).isEqualTo("Ivan");
        verify(userRepository).findById(1L);
    }

    @Test
    void shouldThrowWhenUserNotFound() {
        when(userRepository.findById(99L)).thenReturn(Optional.empty());

        assertThrows(UserNotFoundException.class, () -> userService.findById(99L));
    }
}
```

### 2. @SpringBootTest — интеграционный тест с полным контекстом

```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @Test
    @Transactional // откатывает изменения после теста
    void shouldCreateUser() {
        CreateUserRequest request = new CreateUserRequest("Ivan", "ivan@mail.ru");
        UserDto result = userService.create(request);

        assertThat(result.getId()).isNotNull();
        assertThat(result.getEmail()).isEqualTo("ivan@mail.ru");
    }
}
```

### 3. @WebMvcTest — тест контроллера (без запуска сервера)

```java
@WebMvcTest(UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean // Spring-мок, внедряемый в контекст
    private UserService userService;

    @Test
    void shouldReturnUser() throws Exception {
        when(userService.findById(1L))
                .thenReturn(new UserDto(1L, "Ivan", "ivan@mail.ru"));

        mockMvc.perform(get("/api/users/1")
                        .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Ivan"))
                .andExpect(jsonPath("$.email").value("ivan@mail.ru"));
    }

    @Test
    void shouldReturn404WhenUserNotFound() throws Exception {
        when(userService.findById(99L)).thenThrow(new UserNotFoundException(99L));

        mockMvc.perform(get("/api/users/99"))
                .andExpect(status().isNotFound());
    }

    @Test
    void shouldValidateCreateRequest() throws Exception {
        String invalidJson = """
            {"firstName": "", "email": "invalid"}
            """;

        mockMvc.perform(post("/api/users")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(invalidJson))
                .andExpect(status().isBadRequest());
    }
}
```

### 4. @DataJpaTest — тест репозитория

```java
@DataJpaTest
@ActiveProfiles("test")
class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private TestEntityManager entityManager;

    @Test
    void shouldFindByEmail() {
        User user = new User("Ivan", "ivan@mail.ru");
        entityManager.persistAndFlush(user);

        Optional<User> found = userRepository.findByEmail("ivan@mail.ru");

        assertThat(found).isPresent();
        assertThat(found.get().getFirstName()).isEqualTo("Ivan");
    }
}
```

### Основные аннотации

| Аннотация | Описание |
|-----------|----------|
| `@SpringBootTest` | Полный контекст приложения |
| `@WebMvcTest` | Только веб-слой (контроллеры) |
| `@DataJpaTest` | Только JPA-слой (репозитории) |
| `@MockBean` | Мок бина в Spring-контексте |
| `@ActiveProfiles("test")` | Активация тестового профиля |
| `@Transactional` | Откат данных после каждого теста |
| `@Sql` | Выполнение SQL-скрипта перед тестом |

**Частая ошибка:** использование `@SpringBootTest` для всех тестов — это поднимает полный контекст и сильно замедляет выполнение. Используйте «слайс-тесты» (`@WebMvcTest`, `@DataJpaTest`) для тестирования отдельных слоёв. Также ошибка — путать `@Mock` (Mockito) и `@MockBean` (Spring): `@Mock` не внедряется в контекст, `@MockBean` — подменяет бин в контексте.

[к оглавлению](#Spring-Framework)

## Что такое Spring Cache и как его использовать?

**Spring Cache** — абстракция кэширования, позволяющая декларативно (через аннотации) добавлять кэширование к методам без привязки к конкретной реализации кэша.

**Подключение:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**Включение кэширования:**

```java
@SpringBootApplication
@EnableCaching
public class MyApplication { }
```

**Основные аннотации:**

```java
@Service
public class UserService {

    // @Cacheable — кэширует результат. При повторном вызове с тем же ключом
    // метод НЕ выполняется, результат берётся из кэша
    @Cacheable(value = "users", key = "#id")
    public UserDto findById(Long id) {
        log.info("Загрузка из БД: {}", id);
        return userRepository.findById(id)
                .map(this::toDto)
                .orElseThrow();
    }

    // Условное кэширование
    @Cacheable(value = "users", key = "#id", condition = "#id > 0", unless = "#result == null")
    public UserDto findByIdConditional(Long id) { ... }

    // @CachePut — всегда выполняет метод и обновляет кэш
    @CachePut(value = "users", key = "#result.id")
    public UserDto updateUser(UpdateUserRequest request) {
        User user = userRepository.save(toEntity(request));
        return toDto(user);
    }

    // @CacheEvict — удаляет запись из кэша
    @CacheEvict(value = "users", key = "#id")
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }

    // Очистка всего кэша
    @CacheEvict(value = "users", allEntries = true)
    public void clearUsersCache() { }

    // @Caching — комбинация нескольких операций
    @Caching(
        put = @CachePut(value = "users", key = "#result.id"),
        evict = @CacheEvict(value = "usersList", allEntries = true)
    )
    public UserDto createUser(CreateUserRequest request) { ... }
}
```

**Реализации кэша:**

По умолчанию Spring использует `ConcurrentMapCacheManager` (простой HashMap). Для production рекомендуются:

```properties
# Redis
spring.cache.type=redis
spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.cache.redis.time-to-live=600000  # TTL в мс (10 минут)

# Caffeine (in-memory, рекомендуется для одного экземпляра)
spring.cache.type=caffeine
spring.cache.caffeine.spec=maximumSize=500,expireAfterWrite=10m
```

```xml
<!-- Caffeine -->
<dependency>
    <groupId>com.github.ben-manes.caffeine</groupId>
    <artifactId>caffeine</artifactId>
</dependency>
```

**Конфигурация Caffeine:**

```java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public CacheManager cacheManager() {
        CaffeineCacheManager cacheManager = new CaffeineCacheManager();
        cacheManager.setCaffeine(Caffeine.newBuilder()
                .maximumSize(1000)
                .expireAfterWrite(Duration.ofMinutes(10))
                .recordStats()); // для мониторинга
        return cacheManager;
    }
}
```

**Частая ошибка:** забыть инвалидировать кэш при обновлении данных — пользователи будут видеть устаревшую информацию. Также проблема self-invocation: вызов `@Cacheable`-метода из того же класса не будет кэшироваться (та же проблема, что и с `@Transactional`). И ещё: нельзя кэшировать `null`-результаты без явного `unless = "#result == null"`, иначе при каждом запросе несуществующего объекта будет кэширован `null`.

[к оглавлению](#Spring-Framework)
