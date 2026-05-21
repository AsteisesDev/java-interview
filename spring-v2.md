[Вопросы для собеседования](README.md)

# Spring Framework

+ [Что такое Spring Framework и зачем он нужен?](#что-такое-spring-framework-и-зачем-он-нужен)
+ [Что такое IoC и DI? В чём разница между ними?](#что-такое-ioc-и-di-в-чём-разница-между-ними)
+ [Что такое Spring-контейнер? В чём разница между BeanFactory и ApplicationContext?](#что-такое-spring-контейнер-в-чём-разница-между-beanfactory-и-applicationcontext)
+ [Что такое Spring Bean?](#что-такое-spring-bean)
+ [Каков жизненный цикл Spring Bean?](#каков-жизненный-цикл-spring-bean)
+ [Какие существуют scope у Spring Bean?](#какие-существуют-scope-у-spring-bean)
+ [Для чего нужны аннотации @Component, @Service, @Repository и @Controller?](#для-чего-нужны-аннотации-component-service-repository-и-controller)
+ [Чем отличаются @Configuration и @Component? Что делает аннотация @Bean?](#чем-отличаются-configuration-и-component-что-делает-аннотация-bean)
+ [Как работает @Autowired? Какие существуют способы внедрения зависимостей?](#как-работает-autowired-какие-существуют-способы-внедрения-зависимостей)
+ [Для чего нужны @Qualifier и @Primary?](#для-чего-нужны-qualifier-и-primary)
+ [Как использовать @Value и @PropertySource?](#как-использовать-value-и-propertysource)
+ [Что такое профили в Spring? Как работает аннотация @Profile?](#что-такое-профили-в-spring-как-работает-аннотация-profile)
+ [Что такое Spring AOP? Какие основные понятия в AOP?](#что-такое-spring-aop-какие-основные-понятия-в-aop)
+ [Какие типы Advice существуют в Spring AOP?](#какие-типы-advice-существуют-в-spring-aop)
+ [Как работает @Transactional в Spring?](#как-работает-transactional-в-spring)
+ [Какие уровни propagation существуют?](#какие-уровни-propagation-существуют)
+ [Какие уровни isolation существуют?](#какие-уровни-isolation-существуют)
+ [Какие подводные камни есть у @Transactional?](#какие-подводные-камни-есть-у-transactional)
+ [Что такое Spring Boot и чем он отличается от Spring Framework?](#что-такое-spring-boot-и-чем-он-отличается-от-spring-framework)
+ [Как работает автоконфигурация в Spring Boot?](#как-работает-автоконфигурация-в-spring-boot)
+ [Что такое Spring Boot Starter-ы?](#что-такое-spring-boot-starter-ы)
+ [Как устроена конфигурация в application.properties / application.yml?](#как-устроена-конфигурация-в-applicationproperties--applicationyml)
+ [Что такое Spring Boot Actuator?](#что-такое-spring-boot-actuator)
+ [Как работает встроенный сервер в Spring Boot?](#как-работает-встроенный-сервер-в-spring-boot)
+ [Что такое Spring Boot DevTools?](#что-такое-spring-boot-devtools)
+ [Что такое Spring Data JPA?](#что-такое-spring-data-jpa)
+ [В чём разница между Repository, CrudRepository и JpaRepository?](#в-чём-разница-между-repository-crudrepository-и-jparepository)
+ [Что такое query methods в Spring Data?](#что-такое-query-methods-в-spring-data)
+ [Как использовать @Query, JPQL и native queries?](#как-использовать-query-jpql-и-native-queries)
+ [Как реализовать пагинацию и сортировку в Spring Data JPA?](#как-реализовать-пагинацию-и-сортировку-в-spring-data-jpa)
+ [Какие основные аннотации JPA для маппинга сущностей вы знаете?](#какие-основные-аннотации-jpa-для-маппинга-сущностей-вы-знаете)
+ [Как маппить связи между сущностями? В чём разница между LAZY и EAGER загрузкой?](#как-маппить-связи-между-сущностями-в-чём-разница-между-lazy-и-eager-загрузкой)
+ [Что такое проблема N+1 и как её решить?](#что-такое-проблема-n1-и-как-её-решить)
+ [Как работает @Transactional в контексте JPA?](#как-работает-transactional-в-контексте-jpa)
+ [Как устроена архитектура Spring MVC? Что такое DispatcherServlet?](#как-устроена-архитектура-spring-mvc-что-такое-dispatcherservlet)
+ [В чём разница между @Controller и @RestController?](#в-чём-разница-между-controller-и-restcontroller)
+ [Как работают аннотации маппинга запросов: @RequestMapping, @GetMapping, @PostMapping?](#как-работают-аннотации-маппинга-запросов-requestmapping-getmapping-postmapping)
+ [Для чего нужны @RequestParam, @PathVariable, @RequestBody и @ResponseBody?](#для-чего-нужны-requestparam-pathvariable-requestbody-и-responsebody)
+ [Что такое ResponseEntity и когда его использовать?](#что-такое-responseentity-и-когда-его-использовать)
+ [Как обрабатывать исключения в Spring MVC? Что такое @ExceptionHandler и @ControllerAdvice?](#как-обрабатывать-исключения-в-spring-mvc-что-такое-exceptionhandler-и-controlleradvice)
+ [Как работает валидация в Spring? Что такое @Valid и @Validated?](#как-работает-валидация-в-spring-что-такое-valid-и-validated)
+ [Что такое Spring Security?](#что-такое-spring-security)
+ [В чём разница между аутентификацией и авторизацией?](#в-чём-разница-между-аутентификацией-и-авторизацией)
+ [Что такое SecurityFilterChain и как он работает?](#что-такое-securityfilterchain-и-как-он-работает)
+ [Как работают аннотации @PreAuthorize и @Secured?](#как-работают-аннотации-preauthorize-и-secured)
+ [Как настроить аутентификацию на основе JWT в Spring Security?](#как-настроить-аутентификацию-на-основе-jwt-в-spring-security)
+ [Что такое Spring Event? Как создать и обработать событие?](#что-такое-spring-event-как-создать-и-обработать-событие)
+ [Что такое @Conditional и условная конфигурация в Spring Boot?](#что-такое-conditional-и-условная-конфигурация-в-spring-boot)
+ [Как тестировать Spring-приложения? Какие основные аннотации для тестирования?](#как-тестировать-spring-приложения-какие-основные-аннотации-для-тестирования)
+ [Что такое Spring Cache и как его использовать?](#что-такое-spring-cache-и-как-его-использовать)

## Что такое Spring Framework и зачем он нужен?
<!-- grade: junior -->

Spring Framework -- комплексный фреймворк для разработки Java-приложений, который берёт на себя управление инфраструктурой (создание объектов, транзакции, безопасность), позволяя разработчикам сосредоточиться на бизнес-логике.

> Аналогия из жизни: Spring -- это как администратор офиса. Вы не думаете о мебели, свете, интернете -- администратор всё организует, а вы занимаетесь своей работой.

### Основные причины использования Spring

1. Инверсия управления (IoC) -- фреймворк управляет созданием объектов и их зависимостями
2. Модульность -- Spring состоит из множества модулей (Core, MVC, Data, Security и др.), можно использовать только нужные
3. Упрощение работы с базами данных -- абстракции над JDBC, JPA, интеграция с Hibernate
4. Декларативное управление транзакциями -- аннотация `@Transactional` вместо ручного управления
5. AOP -- позволяет отделить сквозную функциональность (логирование, безопасность, транзакции)
6. Мощная экосистема -- Spring Boot, Spring Cloud, Spring Security, Spring Batch
7. Тестируемость -- IoC делает код легко тестируемым через подмену зависимостей

### Основные модули Spring Framework

| Модуль | Назначение |
|--------|------------|
| Spring Core | IoC-контейнер, DI |
| Spring AOP | Аспектно-ориентированное программирование |
| Spring MVC | Веб-фреймворк |
| Spring Data | Работа с базами данных |
| Spring Security | Безопасность |
| Spring TX | Управление транзакциями |

> **На собеседовании:** интервьюер хочет услышать не просто «это фреймворк», а понимание IoC/DI как ключевой идеи Spring. Частая ошибка -- перечислять модули, не объясняя, зачем Spring нужен и какие проблемы он решает.

[к оглавлению](#spring-framework)

---

## Что такое IoC и DI? В чём разница между ними?
<!-- grade: junior -->

IoC (Inversion of Control) -- принцип проектирования, при котором управление созданием объектов и их жизненным циклом передаётся от самого приложения к фреймворку. DI (Dependency Injection) -- конкретный паттерн реализации IoC, при котором зависимости «внедряются» в объект контейнером извне.

IoC -- это философия, DI -- способ её воплощения. DI -- наиболее распространённая форма IoC.

### Без IoC/DI (жёсткая связность)

```java
public class OrderService {
    // OrderService сам создаёт свою зависимость -- жёсткая связь
    private OrderRepository repository = new OrderRepositoryImpl();

    public void createOrder(Order order) {
        repository.save(order);
    }
}
```

### С IoC/DI (слабая связность)

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

### Преимущества DI

- Слабая связность -- классы зависят от интерфейсов, а не от конкретных реализаций
- Тестируемость -- в тестах легко подставить мок вместо реальной зависимости
- Гибкость -- можно менять реализации без изменения кода клиента
- Единая точка конфигурации -- зависимости описаны в одном месте

### Другие формы IoC (кроме DI)

- Service Locator -- объект сам запрашивает зависимости у специального реестра
- Template Method -- управление потоком передаётся родительскому классу
- Event-driven -- управление через события

> **На собеседовании:** ключевая мысль -- IoC это принцип, DI это реализация. Частая ошибка -- говорить, что IoC и DI это одно и то же.

[к оглавлению](#spring-framework)

---

## Что такое Spring-контейнер? В чём разница между BeanFactory и ApplicationContext?
<!-- grade: junior -->

Spring-контейнер (IoC-контейнер) -- ядро Spring Framework, отвечающее за создание бинов, их конфигурирование и управление жизненным циклом. Контейнер читает метаданные конфигурации (аннотации, XML, Java-конфигурация) и на их основе создаёт и связывает объекты.

| Аспект | BeanFactory | ApplicationContext |
|--------|-------------|--------------------|
| Инициализация | Ленивая (lazy) -- бины создаются при первом запросе | Жадная (eager) -- singleton-бины создаются при запуске |
| Интернационализация | Нет | Да (MessageSource) |
| Публикация событий | Нет | Да (ApplicationEventPublisher) |
| Загрузка ресурсов | Нет | Да (ResourceLoader) |
| Интеграция с AOP | Нет | Да |
| Поддержка профилей | Нет | Да |

```java
// BeanFactory -- базовый (редко используется напрямую)
BeanFactory factory = new XmlBeanFactory(new ClassPathResource("beans.xml"));
MyBean bean = factory.getBean(MyBean.class);

// ApplicationContext -- стандартный выбор
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

// Spring Boot создаёт ApplicationContext автоматически
SpringApplication.run(MyApplication.class, args);
```

### Основные реализации ApplicationContext

- `AnnotationConfigApplicationContext` -- для Java-конфигурации
- `ClassPathXmlApplicationContext` -- для XML-конфигурации из classpath
- `FileSystemXmlApplicationContext` -- для XML из файловой системы
- `WebApplicationContext` -- для веб-приложений

> **На собеседовании:** в современных приложениях всегда используется ApplicationContext. BeanFactory оправдан только в средах с крайне ограниченными ресурсами. Частая ошибка -- не знать, чем они отличаются, и сказать «это одно и то же».

[к оглавлению](#spring-framework)

---

## Что такое Spring Bean?
<!-- grade: junior -->

Spring Bean -- объект, который создаётся, управляется и конфигурируется IoC-контейнером Spring. По сути, это обычный Java-объект (POJO), жизненный цикл которого контролируется контейнером.

### Способы объявления бинов

```java
// 1. Аннотации на классе (Component Scanning)
@Component
public class MyBean { }

// 2. Java-конфигурация (@Bean)
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}

// 3. XML-конфигурация (устаревший подход)
// <bean id="myService" class="com.example.MyServiceImpl"/>
```

### Ключевые характеристики бина

- id/имя -- уникальный идентификатор бина в контейнере
- Класс -- конкретный класс объекта
- Scope -- область видимости (singleton, prototype и др.)
- Зависимости -- другие бины, которые ему нужны
- Callbacks -- методы инициализации и уничтожения

### Частая ошибка

Создание объекта вручную через `new` вместо получения из контейнера. Spring не будет управлять таким объектом, аннотации `@Autowired`, `@Transactional` и другие не будут работать:

```java
// НЕПРАВИЛЬНО -- Spring не управляет этим объектом
MyService service = new MyService();

// ПРАВИЛЬНО -- получаем бин из контейнера
@Autowired
private MyService service;
```

> **На собеседовании:** важно показать понимание того, что бин -- это не просто объект, а объект, управляемый контейнером. Частая ошибка -- забыть упомянуть, что создание объекта через `new` обходит контейнер и отключает все возможности Spring.

[к оглавлению](#spring-framework)

---

## Каков жизненный цикл Spring Bean?
<!-- grade: middle -->

Жизненный цикл Spring Bean -- последовательность этапов от создания до уничтожения объекта в контейнере.

### Полный жизненный цикл (для singleton-бина)

1. Инстанцирование -- контейнер создаёт экземпляр бина (через конструктор)
2. Установка свойств -- внедряются зависимости (через сеттеры, поля)
3. `BeanNameAware.setBeanName()` -- если бин реализует `BeanNameAware`
4. `BeanFactoryAware.setBeanFactory()` -- если реализует `BeanFactoryAware`
5. `ApplicationContextAware.setApplicationContext()` -- если реализует `ApplicationContextAware`
6. `BeanPostProcessor.postProcessBeforeInitialization()` -- для всех BeanPostProcessor
7. `@PostConstruct` -- вызов метода, помеченного аннотацией
8. `InitializingBean.afterPropertiesSet()` -- если реализует `InitializingBean`
9. `init-method` -- вызов пользовательского метода инициализации
10. `BeanPostProcessor.postProcessAfterInitialization()` -- здесь могут создаваться прокси (для `@Transactional`)
11. Бин готов к использованию
12. `@PreDestroy` -- при закрытии контейнера
13. `DisposableBean.destroy()` -- если реализует `DisposableBean`
14. `destroy-method` -- вызов пользовательского метода уничтожения

<details>
<summary>Пример бина с callback-методами</summary>

```java
@Component
public class MyBean implements InitializingBean, DisposableBean {

    @Autowired
    private SomeDependency dependency;

    @PostConstruct
    public void postConstruct() {
        System.out.println("3. @PostConstruct -- после внедрения зависимостей");
    }

    @Override
    public void afterPropertiesSet() {
        System.out.println("4. InitializingBean.afterPropertiesSet()");
    }

    @PreDestroy
    public void preDestroy() {
        System.out.println("6. @PreDestroy -- перед уничтожением");
    }

    @Override
    public void destroy() {
        System.out.println("7. DisposableBean.destroy()");
    }
}
```

</details>

### BeanPostProcessor

Мощный механизм, позволяющий вмешаться в процесс создания каждого бина. Именно через BeanPostProcessor работают многие аннотации Spring (`@Autowired`, `@Transactional`, `@Async`):

```java
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) {
        return bean; // вызывается перед @PostConstruct
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        return bean; // здесь можно обернуть бин в прокси
    }
}
```

> **На собеседовании:** важно назвать ключевые этапы: конструктор -> внедрение зависимостей -> BeanPostProcessor (before) -> @PostConstruct -> BeanPostProcessor (after, тут прокси). Частая ошибка -- использовать зависимости, внедрённые через поля, в конструкторе (они ещё не установлены). Для инициализации используйте `@PostConstruct` или конструкторное внедрение.

[к оглавлению](#spring-framework)

---

## Какие существуют scope у Spring Bean?
<!-- grade: junior -->

Scope определяет, сколько экземпляров бина будет создано и как они будут разделяться.

| Scope | Описание |
|-------|----------|
| singleton | (по умолчанию) Один экземпляр на весь контейнер |
| prototype | Новый экземпляр при каждом запросе бина |
| request | Один экземпляр на HTTP-запрос (только для веб) |
| session | Один экземпляр на HTTP-сессию (только для веб) |
| application | Один экземпляр на ServletContext (только для веб) |
| websocket | Один экземпляр на WebSocket-сессию |

```java
@Component
@Scope("prototype")
public class PrototypeBean { }

@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestScopedBean { }
```

### Проблема singleton-prototype

Если prototype-бин внедряется в singleton, он будет создан один раз и больше не изменится:

```java
@Component // singleton по умолчанию
public class SingletonService {
    @Autowired
    private PrototypeBean prototypeBean; // ПРОБЛЕМА: всегда один и тот же экземпляр!
}
```

### Решения

```java
// 1. Через ObjectProvider
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

> **На собеседовании:** ключевой вопрос -- проблема внедрения prototype в singleton и способы её решения. Частая ошибка -- не знать, что по умолчанию scope -- singleton, и не понимать последствия внедрения prototype в singleton.

[к оглавлению](#spring-framework)

---

## Для чего нужны аннотации @Component, @Service, @Repository и @Controller?
<!-- grade: junior -->

Все эти аннотации являются стереотипными (stereotype annotations) и помечают класс как Spring-бин для обнаружения при сканировании компонентов.

| Аннотация | Слой | Дополнительная функциональность |
|-----------|------|--------------------------------|
| @Component | Общий | Базовая аннотация, без дополнительного поведения |
| @Service | Бизнес-логика | Чисто семантическая |
| @Repository | Доступ к данным | Автоматическая трансляция исключений в DataAccessException |
| @Controller | Веб-контроллер | Включает обработку маппинга запросов |

```java
@Component // общий компонент
public class EmailValidator { }

@Service // бизнес-логика
public class UserService { }

@Repository // доступ к данным
public class UserRepository { }

@Controller // веб-контроллер
public class UserController { }
```

С точки зрения IoC-контейнера все четыре аннотации работают одинаково -- регистрируют бин. Различия: `@Repository` добавляет трансляцию исключений, `@Controller` включает обработку запросов, `@Service` -- чисто семантическая.

> **На собеседовании:** интервьюер хочет услышать, что это специализации `@Component`, и знание конкретных различий (особенно трансляция исключений в `@Repository`). Частая ошибка -- использовать `@Component` для всех классов без разбора.

[к оглавлению](#spring-framework)

---

## Чем отличаются @Configuration и @Component? Что делает аннотация @Bean?
<!-- grade: middle -->

`@Configuration` -- аннотация, указывающая, что класс содержит определения бинов (методы `@Bean`). `@Bean` -- аннотация, помечающая метод как фабричный: возвращаемый объект регистрируется как бин в контейнере.

Ключевое отличие: класс с `@Configuration` обрабатывается через CGLIB-прокси, гарантируя, что вызовы методов `@Bean` внутри класса возвращают один и тот же singleton-экземпляр.

```java
@Configuration
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        return new HikariDataSource();
    }

    @Bean
    public UserRepository userRepository() {
        // dataSource() НЕ создаст новый объект,
        // а вернёт тот же singleton-бин благодаря CGLIB-прокси
        return new UserRepository(dataSource());
    }
}
```

Если использовать `@Component` вместо `@Configuration`:

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

Это называется lite mode -- методы `@Bean` вызываются как обычные Java-методы, без перехвата CGLIB.

> **На собеседовании:** ключевое -- CGLIB-прокси в `@Configuration`, который обеспечивает singleton-семантику при вызове `@Bean`-методов друг из друга. Частая ошибка -- не знать про lite mode и разницу в поведении.

[к оглавлению](#spring-framework)

---

## Как работает @Autowired? Какие существуют способы внедрения зависимостей?
<!-- grade: junior -->

`@Autowired` -- аннотация Spring, которая автоматически внедряет зависимости по типу (by type). Если найден ровно один бин подходящего типа, он будет внедрён.

### Три способа внедрения зависимостей

| Способ | Рекомендация | Причина |
|--------|-------------|---------|
| Через конструктор | Рекомендуется | Неизменяемость (final), обязательность, тестируемость без рефлексии |
| Через сеттер | Допустимо | Для необязательных зависимостей |
| Через поле | Не рекомендуется | Скрывает зависимости, требует рефлексии для тестов |

```java
// 1. Через конструктор (рекомендуемый)
@Service
public class UserService {
    private final UserRepository userRepository;

    // С Spring 4.3 @Autowired можно не указывать при одном конструкторе
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// 2. Через сеттер
@Service
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// 3. Через поле (не рекомендуется)
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

### Порядок разрешения зависимостей

1. Ищет бин по типу
2. Если найдено несколько -- ищет по имени поля/параметра
3. Если не найден -- `NoSuchBeanDefinitionException`
4. Если найдено несколько и имя не совпадает -- `NoUniqueBeanDefinitionException`

> **На собеседовании:** интервьюер ждёт обоснование, почему конструкторное внедрение лучше: immutability, обязательность, тестируемость, видимость нарушений SRP. Частая ошибка -- не знать про циклические зависимости и способы их решения (`@Lazy`, рефакторинг).

[к оглавлению](#spring-framework)

---

## Для чего нужны @Qualifier и @Primary?
<!-- grade: junior -->

`@Qualifier` и `@Primary` решают проблему неоднозначности, когда в контейнере есть несколько бинов одного типа.

`@Qualifier` -- указывает конкретное имя бина для внедрения. `@Primary` -- помечает бин как «предпочтительный» по умолчанию. Приоритет: `@Qualifier` > `@Primary` > совпадение по имени > ошибка.

<details>
<summary>Пример использования @Qualifier и @Primary</summary>

```java
public interface NotificationService {
    void send(String message);
}

@Service("emailNotification")
@Primary // будет внедряться по умолчанию
public class EmailNotificationService implements NotificationService {
    public void send(String message) { /* отправка email */ }
}

@Service("smsNotification")
public class SmsNotificationService implements NotificationService {
    public void send(String message) { /* отправка SMS */ }
}

@Service
public class OrderService {
    // Внедрится EmailNotificationService, т.к. он @Primary
    public OrderService(NotificationService notificationService) { }
}

@Service
public class AlertService {
    // Явно выбираем SMS, несмотря на @Primary
    public AlertService(@Qualifier("smsNotification") NotificationService notificationService) { }
}
```

</details>

### Внедрение всех реализаций

```java
@Service
public class NotificationManager {
    private final List<NotificationService> services;

    // Spring внедрит ВСЕ бины типа NotificationService
    public NotificationManager(List<NotificationService> services) {
        this.services = services;
    }
}
```

### Собственный квалификатор (лучшая практика)

```java
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.PARAMETER, ElementType.TYPE})
public @interface Email { }

@Service
@Email
public class EmailNotificationService implements NotificationService { }

// Использование -- проверяется на этапе компиляции:
public OrderService(@Email NotificationService notificationService) { }
```

> **На собеседовании:** важно знать приоритет разрешения и когда использовать что. Частая ошибка -- использовать строковые значения в `@Qualifier` вместо собственных аннотаций-квалификаторов (строки не проверяются при компиляции).

[к оглавлению](#spring-framework)

---

## Как использовать @Value и @PropertySource?
<!-- grade: junior -->

`@Value` -- аннотация для внедрения значений из внешних источников (файлы свойств, переменные окружения, SpEL-выражения) в поля или параметры бинов. `@PropertySource` -- указывает файл свойств для загрузки.

```java
@Configuration
@PropertySource("classpath:application.properties")
@PropertySource("classpath:db.properties")
public class AppConfig { }
```

### Основные способы использования @Value

```java
@Component
public class AppSettings {

    @Value("${app.name}")
    private String appName;                    // из properties

    @Value("${app.description:Default}")
    private String description;                // со значением по умолчанию

    @Value("${server.port}")
    private int serverPort;                    // автоматическое преобразование типов

    @Value("${JAVA_HOME}")
    private String javaHome;                   // переменная окружения

    @Value("#{T(java.lang.Math).random()}")
    private double randomValue;                // SpEL-выражение

    @Value("${app.servers}")
    private List<String> servers;              // список (app.servers=a,b,c)
}
```

### Лучшая практика -- @ConfigurationProperties (Spring Boot)

Для группы связанных свойств лучше использовать `@ConfigurationProperties`:

```java
@Configuration
@ConfigurationProperties(prefix = "db")
public class DatabaseProperties {
    private String url;
    private int poolSize;
    // геттеры и сеттеры обязательны
}
```

> **На собеседовании:** интервьюер хочет услышать про значения по умолчанию, SpEL и `@ConfigurationProperties` как альтернативу `@Value`. Частая ошибка -- использовать `@Value` в статических полях (не работает) или забыть значение по умолчанию (приложение не запустится, если свойство не найдено).

[к оглавлению](#spring-framework)

---

## Что такое профили в Spring? Как работает аннотация @Profile?
<!-- grade: junior -->

Профили (Profiles) -- механизм Spring для условной регистрации бинов в зависимости от активного окружения (dev, test, prod). Позволяет иметь разные конфигурации без изменения кода.

```java
@Configuration
@Profile("dev")
public class DevDataSourceConfig {
    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2).build();
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
```

### Способы активации профиля

```properties
# 1. В application.properties
spring.profiles.active=dev

# 2. Аргумент командной строки
# java -jar app.jar --spring.profiles.active=prod

# 3. Переменная окружения
# SPRING_PROFILES_ACTIVE=prod
```

```java
// 4. В тестах
@ActiveProfiles("test")
@SpringBootTest
public class MyServiceTest { }
```

### Файлы свойств для профилей (Spring Boot)

```
application.properties            // общие свойства
application-dev.properties        // свойства для профиля dev
application-prod.properties       // свойства для профиля prod
```

Свойства из файла профиля перезаписывают значения из общего `application.properties`.

### Логические операторы в профилях (Spring 5.1+)

```java
@Profile("dev & local")    // И
@Profile("dev | staging")  // ИЛИ
@Profile("!prod")          // НЕ
```

> **На собеседовании:** важно показать знание способов активации и приоритета файлов свойств. Частая ошибка -- забыть активировать профиль: бины с `@Profile("dev")` не будут созданы, если профиль не активирован.

[к оглавлению](#spring-framework)

---

## Что такое Spring AOP? Какие основные понятия в AOP?
<!-- grade: middle -->

AOP (Aspect-Oriented Programming) -- парадигма программирования, позволяющая выделять сквозную функциональность (cross-cutting concerns) в отдельные модули. Сквозная функциональность -- это логика, пронизывающая множество модулей: логирование, транзакции, безопасность, кэширование.

### Основные понятия

| Понятие | Описание |
|---------|----------|
| Aspect (Аспект) | Модуль сквозной функциональности (класс с `@Aspect`) |
| Advice (Совет) | Действие, выполняемое аспектом в определённой точке |
| Join Point (Точка соединения) | Место в коде, где может быть применён advice (в Spring -- всегда вызов метода) |
| Pointcut (Срез) | Выражение, определяющее, к каким join point применяется advice |
| Target object | Объект, к которому применяется аспект |
| Proxy | Объект-обёртка для перехвата вызовов |
| Weaving | Процесс связывания аспектов с объектами (в Spring -- runtime через прокси) |

### Пример аспекта

```java
@Aspect
@Component
public class LoggingAspect {

    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {}

    @Before("serviceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        System.out.println("Вызов метода: " + methodName);
    }
}
```

### Синтаксис Pointcut

```java
@Pointcut("execution(* com.example.service.*.*(..))")  // все методы в пакете
@Pointcut("@annotation(com.example.Loggable)")          // с определённой аннотацией
@Pointcut("within(@org.springframework.stereotype.Service *)") // все @Service
@Pointcut("bean(userService)")                           // конкретный бин
```

### Как работает AOP в Spring

Spring AOP основан на прокси-объектах:
- JDK Dynamic Proxy -- если бин реализует интерфейс
- CGLIB Proxy -- если бин не реализует интерфейс (создаётся подкласс)

### Self-invocation -- главный подводный камень

```java
@Service
public class MyService {
    @Loggable
    public void methodA() { }

    public void methodB() {
        this.methodA(); // AOP НЕ сработает -- вызов идёт напрямую, минуя прокси
    }
}
```

> **На собеседовании:** ключевые моменты -- прокси-модель и проблема self-invocation. Частая ошибка -- не знать, что AOP в Spring работает только на уровне прокси и не перехватывает внутренние вызовы (`this.method()`).

[к оглавлению](#spring-framework)

---

## Какие типы Advice существуют в Spring AOP?
<!-- grade: middle -->

В Spring AOP существует пять типов advice, определяющих, когда выполняется дополнительная логика относительно целевого метода.

| Тип | Когда выполняется |
|-----|-------------------|
| @Before | До вызова метода |
| @AfterReturning | После успешного возврата метода |
| @AfterThrowing | После выброса исключения |
| @After | Всегда после метода (аналог finally) |
| @Around | Оборачивает вызов целиком (самый мощный) |

### Порядок выполнения

`@Around (начало)` -> `@Before` -> метод -> `@AfterReturning` / `@AfterThrowing` -> `@After` -> `@Around (конец)`

<details>
<summary>Примеры всех типов advice</summary>

```java
@Before("execution(* com.example.service.*.*(..))")
public void beforeAdvice(JoinPoint joinPoint) {
    System.out.println("До вызова: " + joinPoint.getSignature().getName());
}

@AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
public void afterReturningAdvice(JoinPoint joinPoint, Object result) {
    System.out.println("Метод вернул: " + result);
}

@AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "ex")
public void afterThrowingAdvice(JoinPoint joinPoint, Exception ex) {
    System.out.println("Исключение: " + ex.getMessage());
}

@After("execution(* com.example.service.*.*(..))")
public void afterAdvice(JoinPoint joinPoint) {
    System.out.println("Метод завершён (в любом случае)");
}

@Around("execution(* com.example.service.*.*(..))")
public Object aroundAdvice(ProceedingJoinPoint pjp) throws Throwable {
    long start = System.currentTimeMillis();
    try {
        Object result = pjp.proceed(); // вызов оригинального метода
        return result;
    } finally {
        long elapsed = System.currentTimeMillis() - start;
        System.out.println(pjp.getSignature().getName() + " выполнен за " + elapsed + " мс");
    }
}
```

</details>

### Практический пример -- замер времени через собственную аннотацию

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Timed { }

@Aspect
@Component
public class TimingAspect {
    @Around("@annotation(Timed)")
    public Object measureTime(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.nanoTime();
        Object result = pjp.proceed();
        long duration = (System.nanoTime() - start) / 1_000_000;
        log.info("{}.{} выполнен за {} мс",
                pjp.getTarget().getClass().getSimpleName(),
                pjp.getSignature().getName(), duration);
        return result;
    }
}
```

> **На собеседовании:** важно знать все 5 типов и порядок их выполнения. Частая ошибка в `@Around` -- забыть вызвать `pjp.proceed()` (метод не выполнится) или забыть вернуть результат `proceed()` (метод вернёт null).

[к оглавлению](#spring-framework)

---

## Как работает @Transactional в Spring?
<!-- grade: middle -->

`@Transactional` -- аннотация для декларативного управления транзакциями. Spring оборачивает вызов метода в транзакцию через прокси: начинает перед выполнением, коммитит при успехе, откатывает при RuntimeException/Error. При checked exception по умолчанию -- commit.

### Механизм работы

1. Spring создаёт прокси вокруг бина с `@Transactional`
2. При вызове метода прокси открывает транзакцию через `PlatformTransactionManager`
3. При успехе -- commit
4. При `RuntimeException` / `Error` -- rollback
5. При checked exception -- commit (по умолчанию!)

```java
@Service
public class TransferService {

    @Transactional
    public void transfer(Long fromId, Long toId, BigDecimal amount) {
        Account from = accountRepository.findById(fromId).orElseThrow();
        Account to = accountRepository.findById(toId).orElseThrow();
        from.setBalance(from.getBalance().subtract(amount));
        to.setBalance(to.getBalance().add(amount));
        accountRepository.save(from);
        accountRepository.save(to);
        // если здесь произойдёт RuntimeException -- все изменения откатятся
    }
}
```

### Основные параметры

```java
@Transactional(
    propagation = Propagation.REQUIRED,        // тип распространения
    isolation = Isolation.READ_COMMITTED,       // уровень изоляции
    timeout = 30,                               // таймаут в секундах
    readOnly = true,                            // только для чтения (оптимизация)
    rollbackFor = Exception.class,              // откат при checked exception
    noRollbackFor = BusinessException.class     // НЕ откатывать при этом исключении
)
```

### readOnly = true

Подсказка для оптимизации: Hibernate отключает dirty checking, база может использовать read-реплику:

```java
@Transactional(readOnly = true)
public List<User> findAllUsers() {
    return userRepository.findAll();
}
```

> **На собеседовании:** критически важно знать, что checked exception НЕ откатывает транзакцию по умолчанию. Для банковских операций всегда указывайте `rollbackFor = Exception.class`. Частая ошибка -- полагать, что любое исключение откатит транзакцию.

[к оглавлению](#spring-framework)

---

## Какие уровни propagation существуют?
<!-- grade: middle -->

Propagation определяет, как метод с `@Transactional` ведёт себя относительно уже существующей транзакции.

| Propagation | Описание |
|-------------|----------|
| REQUIRED (по умолчанию) | Если транзакция есть -- присоединиться. Если нет -- создать новую |
| REQUIRES_NEW | Всегда создавать новую транзакцию, текущую приостановить |
| SUPPORTS | Если транзакция есть -- присоединиться. Если нет -- без транзакции |
| NOT_SUPPORTED | Выполнять без транзакции, текущую приостановить |
| MANDATORY | Обязательно должна быть транзакция, иначе -- исключение |
| NEVER | Обязательно НЕ должно быть транзакции, иначе -- исключение |
| NESTED | Если есть транзакция -- создать вложенную (savepoint). Если нет -- как REQUIRED |

<details>
<summary>Пример: REQUIRED vs REQUIRES_NEW</summary>

```java
@Service
public class OrderService {

    @Transactional // REQUIRED по умолчанию
    public void createOrder(Order order) {
        orderRepository.save(order);
        paymentService.processPayment(order);  // присоединится к той же транзакции
        auditService.logAction("ORDER_CREATED"); // REQUIRES_NEW -- своя транзакция
    }
}

@Service
public class PaymentService {
    @Transactional(propagation = Propagation.REQUIRED)
    public void processPayment(Order order) {
        // работает в той же транзакции, что и createOrder()
        // если здесь ошибка -- откатится ВСЯ транзакция, включая order
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

</details>

### NESTED -- вложенные транзакции через savepoint

```java
@Transactional
public void processBatch(List<Item> items) {
    for (Item item : items) {
        try {
            itemService.processItem(item); // NESTED
        } catch (Exception e) {
            // откатится только обработка этого item (savepoint)
        }
    }
}
```

> **На собеседовании:** ключевое -- разница между REQUIRED и REQUIRES_NEW. При REQUIRED ошибка внутреннего метода откатит всю внешнюю транзакцию. При REQUIRES_NEW -- только внутреннюю. Частая ошибка -- путать эти два типа.

[к оглавлению](#spring-framework)

---

## Какие уровни isolation существуют?
<!-- grade: middle -->

Isolation определяет, насколько транзакция изолирована от изменений, вносимых другими параллельными транзакциями.

### Проблемы параллельного доступа

| Проблема | Описание |
|----------|----------|
| Dirty Read | Чтение данных, которые ещё не зафиксированы другой транзакцией |
| Non-Repeatable Read | Повторное чтение тех же данных даёт другой результат |
| Phantom Read | Повторный запрос возвращает другое количество строк |

### Уровни изоляции

| Isolation | Dirty Read | Non-Repeatable Read | Phantom Read |
|-----------|------------|---------------------|--------------|
| READ_UNCOMMITTED | Возможно | Возможно | Возможно |
| READ_COMMITTED | Нет | Возможно | Возможно |
| REPEATABLE_READ | Нет | Нет | Возможно |
| SERIALIZABLE | Нет | Нет | Нет |
| DEFAULT | Зависит от БД | | |

```java
@Transactional(isolation = Isolation.READ_COMMITTED)
public void updateBalance(Long accountId, BigDecimal amount) { }

@Transactional(isolation = Isolation.REPEATABLE_READ)
public void transfer(Long fromId, Long toId, BigDecimal amount) { }
```

> **На собеседовании:** важно знать проблемы параллельного доступа и какой уровень от чего защищает. Для большинства приложений подходит READ_COMMITTED (PostgreSQL по умолчанию). Частая ошибка -- использовать SERIALIZABLE повсеместно «для надёжности», что сильно снижает производительность.

[к оглавлению](#spring-framework)

---

## Какие подводные камни есть у @Transactional?
<!-- grade: senior -->

Это один из самых популярных вопросов на собеседованиях. Подводные камни `@Transactional` -- проверка глубины понимания Spring.

### 1. Self-invocation (вызов из того же класса)

`@Transactional` работает через прокси. Вызов метода внутри того же класса обходит прокси:

```java
@Service
public class UserService {
    @Transactional
    public void updateUser(User user) {
        userRepository.save(user);
    }

    public void processUser(Long userId) {
        User user = userRepository.findById(userId).orElseThrow();
        this.updateUser(user); // @Transactional НЕ сработает!
    }
}
```

Решения: вынести в другой сервис или использовать self-injection (`@Autowired private UserService self;`).

### 2. Checked exceptions не вызывают откат

```java
@Transactional // НЕ откатится при IOException!
public void saveFile(MultipartFile file) throws IOException {
    userRepository.save(user);
    fileStorage.store(file); // бросает IOException -- транзакция коммитится!
}

// Правильно:
@Transactional(rollbackFor = Exception.class)
public void saveFile(MultipartFile file) throws IOException { }
```

### 3. @Transactional на private методе не работает

Spring AOP не может создать прокси для private-метода.

### 4. Проглатывание исключения

```java
@Transactional
public void process() {
    try {
        riskyOperation();
    } catch (Exception e) {
        log.error("Error", e);
        // исключение проглочено -- транзакция закоммитится,
        // хотя данные могут быть в некорректном состоянии
    }
}
```

### 5. Долгие операции внутри транзакции

```java
@Transactional
public void processOrder(Order order) {
    orderRepository.save(order);
    emailService.sendEmail(order); // долгая операция -- блокирует соединение с БД!
}
```

Отправку уведомлений нужно выносить за пределы транзакции или делать асинхронно.

### 6. Не указан TransactionManager при нескольких DataSource

```java
@Transactional("secondaryTransactionManager")
public void saveToSecondaryDb(Data data) { }
```

> **На собеседовании:** это вопрос для senior-уровня. Интервьюер ждёт минимум 3-4 подводных камня с примерами. Главные: self-invocation, checked exceptions, private-методы. Частая ошибка -- не знать про self-invocation или думать, что любое исключение откатит транзакцию.

[к оглавлению](#spring-framework)

---

## Что такое Spring Boot и чем он отличается от Spring Framework?
<!-- grade: junior -->

Spring Boot -- надстройка над Spring Framework для максимального упрощения создания и запуска Spring-приложений. Принцип: Convention over Configuration (соглашение важнее конфигурации).

> Аналогия из жизни: Spring Framework -- это набор инструментов для строительства дома. Spring Boot -- это готовый каркасный дом, в который можно сразу заехать и достраивать по необходимости.

| Аспект | Spring Framework | Spring Boot |
|--------|-----------------|-------------|
| Конфигурация | Ручная настройка (XML, Java Config) | Автоконфигурация |
| Сервер | Внешний (Tomcat, WildFly) | Встроенный |
| Зависимости | Ручной подбор совместимых версий | Starter-ы с готовыми наборами |
| Запуск | WAR на сервере приложений | Исполняемый JAR (`java -jar`) |
| Мониторинг | Настраивать самостоятельно | Spring Boot Actuator |

### Минимальное Spring Boot приложение

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

`@SpringBootApplication` заменяет: `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`.

Spring Boot не заменяет Spring Framework -- он строится поверх него, убирая шаблонную конфигурацию.

> **На собеседовании:** важно подчеркнуть, что Spring Boot -- это НЕ отдельный фреймворк, а надстройка. Частая ошибка -- говорить, что Spring Boot заменяет Spring Framework.

[к оглавлению](#spring-framework)

---

## Как работает автоконфигурация в Spring Boot?
<!-- grade: middle -->

Автоконфигурация (Auto-configuration) -- механизм, при котором Spring Boot автоматически настраивает бины на основании библиотек в classpath и заданных свойств.

### Как это работает

1. `@EnableAutoConfiguration` (входит в `@SpringBootApplication`) активирует процесс
2. Spring Boot читает `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` для обнаружения классов автоконфигурации
3. Каждый класс содержит условия (`@Conditional`), при которых он применяется

### Основные условия (@Conditional)

| Аннотация | Условие |
|-----------|---------|
| `@ConditionalOnClass` | Класс есть в classpath |
| `@ConditionalOnMissingClass` | Класса нет в classpath |
| `@ConditionalOnBean` | Бин уже зарегистрирован |
| `@ConditionalOnMissingBean` | Бин ещё не зарегистрирован |
| `@ConditionalOnProperty` | Свойство имеет определённое значение |
| `@ConditionalOnWebApplication` | Приложение является веб-приложением |

### Принцип приоритета

Пользовательские бины всегда имеют приоритет над автоконфигурацией. Если вы определили свой `DataSource`, автосконфигурированный не будет создан (благодаря `@ConditionalOnMissingBean`).

### Отключение автоконфигурации

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class MyApplication { }
```

### Отладка

```properties
# Показывает, какие автоконфигурации применены, а какие нет
debug=true
```

> **На собеседовании:** ключевое -- понимание принципа `@ConditionalOnMissingBean` и приоритета пользовательских бинов. Частая ошибка -- добавить зависимость (например, `spring-boot-starter-data-jpa`) без настройки `spring.datasource.url` и получить ошибку запуска.

[к оглавлению](#spring-framework)

---

## Что такое Spring Boot Starter-ы?
<!-- grade: junior -->

Starter -- специальная Maven/Gradle-зависимость, которая подтягивает набор связанных библиотек для определённой функциональности. Устраняет необходимость вручную подбирать совместимые версии зависимостей.

| Starter | Что включает |
|---------|-------------|
| `spring-boot-starter` | Spring Core, автоконфигурация, логирование |
| `spring-boot-starter-web` | Spring MVC, встроенный Tomcat, Jackson |
| `spring-boot-starter-data-jpa` | Spring Data JPA, Hibernate, HikariCP |
| `spring-boot-starter-security` | Spring Security |
| `spring-boot-starter-test` | JUnit 5, Mockito, AssertJ, Spring Test |
| `spring-boot-starter-validation` | Hibernate Validator |
| `spring-boot-starter-actuator` | Мониторинг и метрики |
| `spring-boot-starter-cache` | Абстракция кэширования |

<details>
<summary>Пример подключения в Maven</summary>

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

</details>

Версии указывать не нужно -- `spring-boot-starter-parent` управляет совместимостью всех версий.

> **На собеседовании:** важно понимать, зачем нужны starter-ы (управление версиями + автоконфигурация). Частая ошибка -- добавлять зависимости напрямую (например, `hibernate-core`) вместо starter-а, что приводит к конфликтам версий.

[к оглавлению](#spring-framework)

---

## Как устроена конфигурация в application.properties / application.yml?
<!-- grade: junior -->

Spring Boot автоматически загружает файл `application.properties` (или `application.yml`) из каталога `src/main/resources`.

<details>
<summary>Пример application.properties</summary>

```properties
server.port=8080
spring.application.name=my-app

# Подключение к БД
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=admin
spring.datasource.password=${DB_PASSWORD}

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true
spring.jpa.open-in-view=false

# Логирование
logging.level.root=INFO
logging.level.com.example=DEBUG
```

</details>

<details>
<summary>Пример application.yml</summary>

```yaml
server:
  port: 8080

spring:
  application:
    name: my-app
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: admin
    password: ${DB_PASSWORD}
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: true
    open-in-view: false
```

</details>

### Приоритет загрузки свойств (от низшего к высшему)

1. Значения по умолчанию (`@Value("${prop:default}")`)
2. `application.properties` в JAR
3. `application-{profile}.properties` в JAR
4. `application.properties` вне JAR
5. `application-{profile}.properties` вне JAR
6. Аргументы командной строки (`--server.port=9090`)
7. Переменные окружения (`SERVER_PORT=9090`)

### Типизированная конфигурация с @ConfigurationProperties

```java
@Configuration
@ConfigurationProperties(prefix = "app.mail")
@Validated
public class MailProperties {
    @NotBlank
    private String host;
    private int port = 587;
    private String username;
    // геттеры и сеттеры
}
```

> **На собеседовании:** важно знать приоритет загрузки свойств и `@ConfigurationProperties`. Частая ошибка -- хранить пароли прямо в `application.properties`, которое попадает в Git. Используйте переменные окружения или Vault.

[к оглавлению](#spring-framework)

---

## Что такое Spring Boot Actuator?
<!-- grade: junior -->

Spring Boot Actuator -- модуль, предоставляющий набор HTTP-эндпоинтов для мониторинга и управления приложением в production.

### Основные эндпоинты

| Эндпоинт | Описание |
|----------|----------|
| `/actuator/health` | Состояние здоровья приложения |
| `/actuator/info` | Информация о приложении |
| `/actuator/metrics` | Метрики (память, CPU, HTTP-запросы) |
| `/actuator/env` | Свойства окружения |
| `/actuator/beans` | Все бины в контейнере |
| `/actuator/mappings` | Все URL-маппинги |
| `/actuator/loggers` | Уровни логирования (можно менять на лету) |
| `/actuator/prometheus` | Метрики в формате Prometheus |

### Настройка

```properties
# Включить все эндпоинты (по умолчанию открыт только health и info)
management.endpoints.web.exposure.include=health,info,metrics,prometheus

# Отдельный порт для actuator
management.server.port=9090

# Детальная информация о health
management.endpoint.health.show-details=always
```

### Создание собственного health-индикатора

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

> **На собеседовании:** покажите понимание безопасности actuator-эндпоинтов. Частая ошибка -- открыть все эндпоинты в production без защиты: `/actuator/env` может содержать пароли и секреты.

[к оглавлению](#spring-framework)

---

## Как работает встроенный сервер в Spring Boot?
<!-- grade: junior -->

Spring Boot включает встроенный (embedded) сервер приложений, позволяющий запускать приложение как обычный JAR-файл без развёртывания на внешнем сервере.

| Сервер | Примечание |
|--------|------------|
| Tomcat | По умолчанию в `spring-boot-starter-web` |
| Jetty | Легковесная альтернатива |
| Undertow | От JBoss, высокая производительность |
| Netty | Для реактивных приложений (`spring-boot-starter-webflux`) |

### Смена сервера

```xml
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

### Настройка

```properties
server.port=8080
server.servlet.context-path=/api
server.tomcat.threads.max=200
server.tomcat.threads.min-spare=10
server.tomcat.max-connections=8192
server.tomcat.connection-timeout=20000
```

### Запуск

```bash
mvn clean package
java -jar target/my-app-1.0.0.jar --server.port=9090 --spring.profiles.active=prod
```

### Преимущества

- Простота развёртывания -- один JAR-файл
- Идеально для микросервисов и Docker-контейнеров
- Единая конфигурация через `application.properties`
- Быстрый старт разработки

> **На собеседовании:** покажите знание поддерживаемых серверов и способов настройки. Частая ошибка -- не знать, как сменить сервер или настроить пул потоков Tomcat.

[к оглавлению](#spring-framework)

---

## Что такое Spring Boot DevTools?
<!-- grade: junior -->

Spring Boot DevTools -- модуль для ускорения разработки за счёт автоматической перезагрузки приложения, отключения кэширования и удобных настроек по умолчанию.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

### Ключевые возможности

1. Автоматический рестарт -- при изменении файлов приложение перезапускается. DevTools использует два ClassLoader-а: один для неизменяемых зависимостей, другой для классов проекта. Перезагружается только второй
2. LiveReload -- автоматическое обновление страницы в браузере
3. Отключение кэширования -- шаблонизаторы работают без кэша
4. Расширенное логирование

DevTools автоматически отключается при запуске в production (из JAR или при отсутствии на classpath). Атрибут `optional=true` гарантирует, что DevTools не попадёт в зависимости других модулей.

> **На собеседовании:** достаточно знать назначение и принцип работы двух ClassLoader-ов. Частая ошибка -- не упомянуть, что DevTools автоматически отключается в production.

[к оглавлению](#spring-framework)

---

## Что такое Spring Data JPA?
<!-- grade: junior -->

Spring Data JPA -- часть проекта Spring Data, которая значительно упрощает работу с JPA, устраняя необходимость в шаблонном коде для стандартных операций с базой данных.

> Аналогия из жизни: Spring Data JPA -- как калькулятор для бухгалтера. Вместо того чтобы вручную складывать столбцы цифр, вы нажимаете кнопки и получаете результат.

### Без Spring Data JPA

```java
@Repository
public class UserRepositoryImpl {
    @PersistenceContext
    private EntityManager em;

    public User findById(Long id) { return em.find(User.class, id); }
    public List<User> findAll() {
        return em.createQuery("SELECT u FROM User u", User.class).getResultList();
    }
    // ... ещё save(), delete() и т.д.
}
```

### С Spring Data JPA

```java
// Всё! Spring Data сгенерирует реализацию автоматически
public interface UserRepository extends JpaRepository<User, Long> {
}
```

### Основные возможности

- Автоматическая реализация репозиториев
- Query methods -- генерация запросов по имени метода
- Пагинация и сортировка из коробки
- `@Query` для пользовательских запросов (JPQL и native SQL)
- Auditing -- автоматическое заполнение дат создания/обновления
- Specifications -- динамическое построение запросов

> **На собеседовании:** покажите, что понимаете, как Spring Data JPA экономит время (автогенерация реализаций, query methods). Частая ошибка -- не знать, что за интерфейсом стоит автоматически сгенерированная реализация.

[к оглавлению](#spring-framework)

---

## В чём разница между Repository, CrudRepository и JpaRepository?
<!-- grade: junior -->

Эти интерфейсы образуют иерархию наследования, каждый следующий добавляет функциональность.

| Интерфейс | Что добавляет |
|-----------|---------------|
| Repository | Маркерный интерфейс (без методов) |
| CrudRepository | Базовые CRUD: save, findById, findAll, count, delete |
| ListCrudRepository (3.0+) | То же, но возвращает List вместо Iterable |
| PagingAndSortingRepository | findAll(Sort), findAll(Pageable) |
| JpaRepository | flush, saveAndFlush, deleteInBatch, getReferenceById |

### Ключевые различия CrudRepository vs JpaRepository

| Метод | CrudRepository | JpaRepository |
|-------|---------------|---------------|
| `findAll()` | `Iterable<T>` | `List<T>` |
| `flush()` | Нет | Есть |
| `saveAndFlush()` | Нет | Есть |
| `deleteInBatch()` | Нет | Есть (эффективнее) |
| `getReferenceById()` | Нет | Есть (lazy proxy) |

### Когда что использовать

```java
// Для большинства случаев -- JpaRepository
public interface UserRepository extends JpaRepository<User, Long> { }

// Если нужен минимум -- CrudRepository
public interface AuditLogRepository extends CrudRepository<AuditLog, Long> { }

// Если нужно ограничить набор методов
@NoRepositoryBean
public interface ReadOnlyRepository<T, ID> extends Repository<T, ID> {
    Optional<T> findById(ID id);
    List<T> findAll();
}
```

> **На собеседовании:** покажите знание иерархии и когда использовать какой интерфейс. Частая ошибка -- использовать `getReferenceById()` вместо `findById()` (getReferenceById возвращает прокси, и при обращении к полям вне транзакции будет `LazyInitializationException`).

[к оглавлению](#spring-framework)

---

## Что такое query methods в Spring Data?
<!-- grade: junior -->

Query methods (derived queries) -- механизм автоматической генерации SQL-запросов на основе имени метода в интерфейсе репозитория. Spring Data анализирует имя метода и создаёт соответствующий JPQL-запрос.

### Основные ключевые слова

```java
public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email);                  // WHERE email = ?
    List<User> findByFirstNameAndLastName(String fn, String ln); // AND
    List<User> findByAgeGreaterThan(int age);                  // WHERE age > ?
    List<User> findByAgeBetween(int from, int to);             // BETWEEN
    List<User> findByEmailContaining(String fragment);         // LIKE %?%
    List<User> findByStatusIn(Collection<String> statuses);    // IN
    List<User> findByDeletedFalse();                           // WHERE deleted = false
    List<User> findByEmailIsNull();                            // IS NULL
    List<User> findByStatusOrderByLastNameAsc(String status);  // ORDER BY
    Optional<User> findFirstByOrderByCreatedAtDesc();          // LIMIT 1
    List<User> findTop5ByStatusOrderByCreatedAtDesc(String s); // TOP 5
    long countByStatus(String status);                         // COUNT
    boolean existsByEmail(String email);                       // EXISTS
    void deleteByStatus(String status);                        // DELETE
}
```

### Вложенные свойства

```java
// Навигация по связям: user.address.city
List<User> findByAddressCity(String city);

// Разрешение неоднозначности через _
List<User> findByAddress_City(String city);
```

### Пагинация в query methods

```java
Page<User> findByStatus(String status, Pageable pageable);
Slice<User> findByStatus(String status, Pageable pageable);
List<User> findByStatus(String status, Sort sort);
```

> **На собеседовании:** покажите знание основных ключевых слов и навигации по связям. Частая ошибка -- создавать слишком длинные имена методов. Если имя становится нечитаемым, лучше использовать `@Query`.

[к оглавлению](#spring-framework)

---

## Как использовать @Query, JPQL и native queries?
<!-- grade: middle -->

Когда query methods недостаточно, Spring Data предоставляет аннотацию `@Query` для написания собственных запросов.

JPQL (Java Persistence Query Language) оперирует сущностями и их полями, а не таблицами и колонками. Native queries используют обычный SQL.

### JPQL

```java
@Query("SELECT u FROM User u WHERE u.email = :email")
Optional<User> findByEmailAddress(@Param("email") String email);

@Query("SELECT u FROM User u JOIN u.roles r WHERE r.name = :roleName")
List<User> findByRoleName(@Param("roleName") String roleName);

@Query("SELECT new com.example.dto.UserSummary(u.id, u.firstName, u.email) " +
       "FROM User u WHERE u.status = 'ACTIVE'")
List<UserSummary> findActiveUserSummaries(); // DTO-проекция
```

### Native queries

```java
@Query(value = "SELECT * FROM users WHERE email = :email", nativeQuery = true)
Optional<User> findByEmailNative(@Param("email") String email);

@Query(
    value = "SELECT * FROM users WHERE status = :status",
    countQuery = "SELECT COUNT(*) FROM users WHERE status = :status",
    nativeQuery = true
)
Page<User> findByStatusNative(@Param("status") String status, Pageable pageable);
```

### Модифицирующие запросы

```java
@Modifying(clearAutomatically = true)
@Transactional
@Query("UPDATE User u SET u.status = :status WHERE u.lastLoginAt < :date")
int deactivateInactiveUsers(@Param("status") String status,
                            @Param("date") LocalDateTime date);
```

### Когда что использовать

| Тип | Когда |
|-----|-------|
| Query methods | Простые запросы по 1-2 полям |
| JPQL | JOIN-ы, агрегация, DTO-проекции |
| Native SQL | Специфичные функции БД, оптимизация |

> **На собеседовании:** важно знать разницу между JPQL и native SQL и когда применять каждый. Частая ошибка -- забыть `@Modifying` для UPDATE/DELETE или забыть `@Transactional` для модифицирующих запросов. Ещё ошибка -- не указать `clearAutomatically = true`, из-за чего persistence context содержит устаревшие данные.

[к оглавлению](#spring-framework)

---

## Как реализовать пагинацию и сортировку в Spring Data JPA?
<!-- grade: junior -->

Spring Data предоставляет абстракции для пагинации и сортировки через интерфейсы `Pageable`, `Sort`, `Page` и `Slice`.

### Сортировка

```java
List<User> users = userRepository.findByStatus("ACTIVE",
    Sort.by(Sort.Direction.ASC, "lastName"));

Sort sort = Sort.by("lastName").ascending()
               .and(Sort.by("firstName").ascending());
```

### Пагинация

```java
Pageable pageable = PageRequest.of(0, 20, Sort.by("lastName").ascending());
Page<User> page = userRepository.findByStatus("ACTIVE", pageable);

page.getContent();          // List<User> -- данные текущей страницы
page.getTotalElements();    // общее количество элементов
page.getTotalPages();       // общее количество страниц
page.getNumber();           // номер текущей страницы (с 0)
page.hasNext();             // есть ли следующая страница
```

### Slice vs Page

`Page` выполняет дополнительный `COUNT(*)` для подсчёта общего количества. `Slice` этого не делает -- только знает, есть ли следующая страница. Slice быстрее для больших таблиц.

### Пагинация в контроллере

```java
@GetMapping
public Page<UserDto> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "20") int size,
        @RequestParam(defaultValue = "lastName,asc") String[] sort) {

    Pageable pageable = PageRequest.of(page, size, Sort.by(
        Sort.Order.by(sort[0]).with(Sort.Direction.fromString(sort[1]))));
    return userRepository.findByStatus("ACTIVE", pageable).map(this::toDto);
}
```

> **На собеседовании:** покажите понимание разницы Page vs Slice и когда что использовать. Частая ошибка -- загружать все данные в память и делать пагинацию на уровне Java. Также -- не ограничивать максимальный size, позволяя клиенту запросить `size=1000000`.

[к оглавлению](#spring-framework)

---

## Какие основные аннотации JPA для маппинга сущностей вы знаете?
<!-- grade: junior -->

JPA использует аннотации для маппинга Java-классов на таблицы БД. Минимальный набор: `@Entity`, `@Id`, `@GeneratedValue`.

<details>
<summary>Пример сущности со всеми основными аннотациями</summary>

```java
@Entity
@Table(name = "users", schema = "public",
       uniqueConstraints = @UniqueConstraint(columnNames = {"email"}))
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "first_name", nullable = false, length = 100)
    private String firstName;

    @Column(unique = true, nullable = false)
    private String email;

    @Column(precision = 19, scale = 2)
    private BigDecimal balance;

    @Enumerated(EnumType.STRING) // всегда STRING, не ORDINAL!
    @Column(nullable = false)
    private UserStatus status;

    private LocalDateTime updatedAt; // @Temporal не нужна для Java 8+ Date API

    @Lob // для больших объектов (TEXT, BLOB)
    private String description;

    @Transient // НЕ маппится на колонку
    private String temporaryField;

    @Embedded
    private Address address;

    protected User() { } // конструктор без аргументов обязателен для JPA
}
```

</details>

### Стратегии генерации ID

| Strategy | Описание |
|----------|----------|
| IDENTITY | Автоинкремент БД (PostgreSQL SERIAL, MySQL AUTO_INCREMENT) |
| SEQUENCE | Последовательность БД (рекомендуется для PostgreSQL) |
| TABLE | Специальная таблица для генерации ID (медленно) |
| AUTO | Провайдер выбирает стратегию сам |

```java
// Рекомендуемый вариант для PostgreSQL
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
@SequenceGenerator(name = "user_seq", sequenceName = "users_id_seq", allocationSize = 50)
private Long id;
```

### Частые ошибки

1. `@Enumerated(EnumType.ORDINAL)` (по умолчанию) -- при добавлении нового значения в enum порядок нарушится. Всегда используйте `EnumType.STRING`
2. Забыть конструктор без аргументов -- JPA не сможет создать экземпляр
3. `GenerationType.AUTO` с Hibernate может создать таблицу `hibernate_sequence` вместо нативной последовательности

> **На собеседовании:** ключевые аннотации -- `@Entity`, `@Id`, `@GeneratedValue`, `@Column`, `@Enumerated(STRING)`. Частая ошибка -- использовать EnumType.ORDINAL.

[к оглавлению](#spring-framework)

---

## Как маппить связи между сущностями? В чём разница между LAZY и EAGER загрузкой?
<!-- grade: middle -->

JPA поддерживает четыре типа связей: `@ManyToOne`, `@OneToMany`, `@ManyToMany`, `@OneToOne`.

### Значения по умолчанию (важно знать!)

| Аннотация | FetchType по умолчанию |
|-----------|------------------------|
| `@ManyToOne` | EAGER (это плохо!) |
| `@OneToOne` | EAGER (это плохо!) |
| `@OneToMany` | LAZY (это хорошо) |
| `@ManyToMany` | LAZY (это хорошо) |

Рекомендация: всегда явно указывайте `fetch = FetchType.LAZY` для всех связей.

### @ManyToOne / @OneToMany

```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}

@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders = new ArrayList<>();

    public void addOrder(Order order) {
        orders.add(order);
        order.setUser(this);
    }
}
```

### LAZY vs EAGER

| Тип | Когда загружается | Когда использовать |
|-----|-------------------|--------------------|
| LAZY | При первом обращении к коллекции/полю | По умолчанию для всех связей |
| EAGER | Сразу вместе с родительской сущностью | Почти никогда |

### CascadeType

| CascadeType | Описание |
|-------------|----------|
| PERSIST | Каскадное сохранение |
| MERGE | Каскадное обновление |
| REMOVE | Каскадное удаление |
| ALL | Все вышеперечисленные |

> **На собеседовании:** ключевое -- знать значения FetchType по умолчанию и всегда ставить LAZY. Частая ошибка -- оставлять EAGER по умолчанию для `@ManyToOne` (причина проблемы N+1) или использовать `CascadeType.ALL` без необходимости.

[к оглавлению](#spring-framework)

---

## Что такое проблема N+1 и как её решить?
<!-- grade: middle -->

Проблема N+1 -- ситуация, когда для получения данных выполняется 1 запрос для основной коллекции и N дополнительных запросов для загрузки связанных сущностей каждого элемента.

### Пример проблемы

```java
// 1 запрос: SELECT * FROM users
List<User> users = userRepository.findAll();

for (User user : users) {
    // N запросов: SELECT * FROM orders WHERE user_id = ?
    System.out.println(user.getOrders().size());
}
// Итого: 1 + N запросов. При 1000 пользователей -- 1001 запрос!
```

### Решения

### 1. JOIN FETCH (JPQL)

```java
@Query("SELECT u FROM User u JOIN FETCH u.orders WHERE u.status = :status")
List<User> findByStatusWithOrders(@Param("status") String status);
// 1 запрос с JOIN
```

### 2. @EntityGraph

```java
@EntityGraph(attributePaths = {"orders"})
List<User> findByStatus(String status);
```

### 3. Batch fetching

```properties
spring.jpa.properties.hibernate.default_batch_fetch_size=25
```

Вместо N запросов будет ceil(N/25) запросов с `WHERE user_id IN (?, ?, ..., ?)`.

### 4. DTO-проекция (лучшая производительность)

```java
@Query("SELECT new com.example.dto.UserOrderSummary(u.id, u.name, COUNT(o)) " +
       "FROM User u LEFT JOIN u.orders o GROUP BY u.id, u.name")
List<UserOrderSummary> findUserOrderSummaries();
```

### Как обнаружить

```properties
spring.jpa.show-sql=true
logging.level.org.hibernate.SQL=DEBUG
```

> **На собеседовании:** назовите проблему, покажите пример и минимум 2-3 решения. Частая ошибка -- использовать JOIN FETCH с пагинацией (Hibernate загрузит все данные в память). Решение -- `@BatchSize` или подзапрос.

[к оглавлению](#spring-framework)

---

## Как работает @Transactional в контексте JPA?
<!-- grade: middle -->

`@Transactional` в контексте JPA управляет не только транзакциями БД, но и жизненным циклом Persistence Context -- «рабочей области» JPA, которая хранит загруженные сущности и отслеживает их изменения (кэш первого уровня).

### Dirty Checking

```java
@Transactional
public void updateUserEmail(Long userId, String newEmail) {
    User user = userRepository.findById(userId).orElseThrow();
    // user в состоянии MANAGED
    user.setEmail(newEmail);
    // НЕ нужно вызывать save()!
    // Hibernate обнаружит изменение (dirty checking) и выполнит UPDATE при коммите
}
```

### Состояния сущности JPA

| Состояние | Описание |
|-----------|----------|
| NEW (Transient) | Новый объект, не связан с контекстом и БД |
| MANAGED | Управляется контекстом, изменения отслеживаются |
| DETACHED | Был управляемым, но контекст закрыт |
| REMOVED | Помечен для удаления |

### readOnly и оптимизация

```java
@Transactional(readOnly = true)
public User getUser(Long id) {
    // readOnly = true:
    // 1. Не делать dirty checking (экономия CPU)
    // 2. Не хранить snapshot-ы (экономия памяти)
    // 3. Потенциально использовать read-реплику
    return userRepository.findById(id).orElseThrow();
}
```

### Open Session in View (OSIV)

По умолчанию в Spring Boot включён `spring.jpa.open-in-view=true`, что держит Persistence Context открытым на протяжении всего HTTP-запроса. Рекомендуется отключить:

```properties
spring.jpa.open-in-view=false
```

> **На собеседовании:** ключевое -- dirty checking (автоматическое обнаружение изменений) и состояния сущностей. Частая ошибка -- изменять сущность внутри `@Transactional` метода, не осознавая, что Hibernate выполнит UPDATE даже без вызова `save()`.

[к оглавлению](#spring-framework)

---

## Как устроена архитектура Spring MVC? Что такое DispatcherServlet?
<!-- grade: middle -->

Spring MVC -- веб-фреймворк, построенный по паттерну Front Controller. Все HTTP-запросы проходят через единую точку входа -- DispatcherServlet.

### Архитектура обработки запроса

```
Клиент
   | HTTP запрос
DispatcherServlet
   |
HandlerMapping --> определяет контроллер
   |
HandlerAdapter --> вызывает метод контроллера
   |
Controller --> выполняет бизнес-логику
   |
HttpMessageConverter --> сериализует в JSON (для @RestController)
   или ViewResolver --> рендерит HTML (для @Controller)
   |
Клиент <-- HTTP ответ
```

### Фильтры и интерсепторы

```
HTTP запрос -> Filter1 -> Filter2 -> DispatcherServlet -> Interceptor.preHandle
                                                             -> Controller
                                                         <- Interceptor.postHandle
                                                         <- Interceptor.afterCompletion
              <- Filter2 <- Filter1 <- HTTP ответ
```

<details>
<summary>Пример интерсептора</summary>

```java
@Component
public class LoggingInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                             Object handler) {
        return true; // true = продолжить, false = прервать
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) { }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                 Object handler, Exception ex) { }
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

</details>

> **На собеседовании:** назовите основные компоненты (DispatcherServlet, HandlerMapping, HandlerAdapter, ViewResolver/HttpMessageConverter) и порядок обработки. Частая ошибка -- не знать разницу между Filter и Interceptor (Filter работает на уровне сервлета, Interceptor -- на уровне Spring MVC).

[к оглавлению](#spring-framework)

---

## В чём разница между @Controller и @RestController?
<!-- grade: junior -->

`@Controller` -- аннотация для контроллеров, возвращающих представления (HTML через шаблонизаторы). `@RestController` -- комбинация `@Controller` + `@ResponseBody`, где каждый метод автоматически сериализует объект в тело HTTP-ответа (JSON по умолчанию).

| Аспект | @Controller | @RestController |
|--------|-------------|-----------------|
| Возвращает | Имя view (String) | Объект (-> JSON) |
| @ResponseBody | Нужно добавить явно | Включено по умолчанию |
| Применение | SSR (Thymeleaf, JSP) | REST API |
| Content-Type | text/html | application/json |

```java
@Controller
public class PageController {
    @GetMapping("/users")
    public String usersPage(Model model) {
        model.addAttribute("users", userService.findAll());
        return "users"; // имя шаблона templates/users.html
    }
}

@RestController
@RequestMapping("/api/users")
public class UserRestController {
    @GetMapping
    public List<UserDto> getAllUsers() {
        return userService.findAll(); // автоматически в JSON
    }
}
```

> **На собеседовании:** `@RestController` = `@Controller` + `@ResponseBody` на каждом методе. Частая ошибка -- использовать `@RestController` и пытаться вернуть имя view (Spring вернёт строку как JSON, а не отрендерит шаблон).

[к оглавлению](#spring-framework)

---

## Как работают аннотации маппинга запросов: @RequestMapping, @GetMapping, @PostMapping?
<!-- grade: junior -->

Эти аннотации связывают HTTP-запросы с методами контроллера. `@GetMapping`, `@PostMapping` и другие -- сокращения для `@RequestMapping` с конкретным HTTP-методом.

```java
@RestController
@RequestMapping("/api/v1/users") // общий префикс
public class UserController {

    @GetMapping                    // GET /api/v1/users
    public List<UserDto> findAll() { }

    @GetMapping("/{id}")           // GET /api/v1/users/123
    public UserDto findById(@PathVariable Long id) { }

    @PostMapping                   // POST /api/v1/users
    public UserDto create(@RequestBody CreateUserRequest request) { }

    @PutMapping("/{id}")           // PUT /api/v1/users/123
    public UserDto update(@PathVariable Long id, @RequestBody UpdateUserRequest request) { }

    @DeleteMapping("/{id}")        // DELETE /api/v1/users/123
    public void delete(@PathVariable Long id) { }
}
```

### Расширенные параметры

```java
@GetMapping(value = "/users", headers = "X-API-VERSION=2")    // по заголовку
@PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)  // по Content-Type
@GetMapping(produces = MediaType.APPLICATION_XML_VALUE)       // по Accept
```

> **На собеседовании:** покажите знание shortcut-аннотаций и параметров маппинга. Частая ошибка -- маппинг нескольких методов на один URL + HTTP-метод (`IllegalStateException`).

[к оглавлению](#spring-framework)

---

## Для чего нужны @RequestParam, @PathVariable, @RequestBody и @ResponseBody?
<!-- grade: junior -->

Эти аннотации определяют, откуда Spring извлекает данные запроса и куда помещает данные ответа.

| Аннотация | Откуда берёт данные | Пример URL |
|-----------|---------------------|------------|
| @PathVariable | Из URL-пути | `/api/users/42` |
| @RequestParam | Из query-параметров | `/api/users?status=ACTIVE` |
| @RequestBody | Из тела запроса (JSON) | POST с JSON body |
| @ResponseBody | В тело ответа (JSON) | Автоматически в @RestController |

```java
@PutMapping("/api/departments/{deptId}/users/{userId}")
public ResponseEntity<UserDto> updateUser(
        @PathVariable Long deptId,
        @PathVariable Long userId,
        @RequestParam(required = false) boolean notify,
        @RequestBody @Valid UpdateUserRequest request) {
    // ...
}
```

```java
// GET /api/users?status=ACTIVE&page=0&size=20
@GetMapping("/api/users")
public Page<UserDto> findUsers(
        @RequestParam String status,
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "20") int size,
        @RequestParam(required = false) String search) { }
```

> **На собеседовании:** покажите умение комбинировать эти аннотации. Частая ошибка -- забыть `@RequestBody` для POST/PUT (параметр будет null) или отправлять form-data вместо JSON.

[к оглавлению](#spring-framework)

---

## Что такое ResponseEntity и когда его использовать?
<!-- grade: junior -->

ResponseEntity -- класс-обёртка для полного контроля HTTP-ответа: статус-код, заголовки и тело.

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
        return ResponseEntity.created(location).body(created); // 201 Created + Location
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build(); // 204 No Content
    }
}
```

### Распространённые статус-коды

```java
ResponseEntity.ok(body);                           // 200
ResponseEntity.created(uri).body(body);             // 201
ResponseEntity.noContent().build();                 // 204
ResponseEntity.badRequest().body(errorDto);         // 400
ResponseEntity.notFound().build();                  // 404
ResponseEntity.status(HttpStatus.CONFLICT).body(e); // 409
```

### Когда использовать

- Когда нужен статус-код, отличный от 200
- Когда нужно установить заголовки ответа
- Для POST-запросов (201 Created + Location header)
- Для условных ответов (200 или 404)

> **На собеседовании:** покажите знание REST best practices (правильные HTTP-статусы для каждой операции). Частая ошибка -- возвращать 200 OK с сообщением об ошибке в теле.

[к оглавлению](#spring-framework)

---

## Как обрабатывать исключения в Spring MVC? Что такое @ExceptionHandler и @ControllerAdvice?
<!-- grade: middle -->

Spring MVC предоставляет механизм обработки исключений на разных уровнях. `@ExceptionHandler` обрабатывает исключения в одном контроллере. `@ControllerAdvice` -- глобально для всех контроллеров.

<details>
<summary>Глобальный обработчик исключений</summary>

```java
@RestControllerAdvice
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
        return ResponseEntity.status(HttpStatus.FORBIDDEN)
                .body(new ErrorResponse("ACCESS_DENIED", "Доступ запрещён"));
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        log.error("Неожиданная ошибка", ex);
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(new ErrorResponse("INTERNAL_ERROR", "Внутренняя ошибка сервера"));
    }
}

public class ErrorResponse {
    private String code;
    private String message;
    private List<String> details;
    private LocalDateTime timestamp = LocalDateTime.now();
}
```

</details>

### Ограничение области действия

```java
@RestControllerAdvice(basePackages = "com.example.api")
public class ApiExceptionHandler { }
```

> **На собеседовании:** покажите умение создать полноценный GlobalExceptionHandler с обработкой валидации, not found и fallback для всех исключений. Частая ошибка -- не создавать обработчик `Exception.class` (Spring вернёт stack trace, раскрывая внутреннюю структуру приложения).

[к оглавлению](#spring-framework)

---

## Как работает валидация в Spring? Что такое @Valid и @Validated?
<!-- grade: middle -->

Spring интегрирует Bean Validation API (JSR 380) через Hibernate Validator для декларативной валидации данных. Аннотация `@Valid` перед `@RequestBody` активирует проверку.

### Основные аннотации валидации

```java
public class CreateUserRequest {
    @NotBlank(message = "Имя не может быть пустым")
    @Size(min = 2, max = 100)
    private String firstName;

    @NotBlank @Email
    private String email;

    @NotNull @Min(18) @Max(150)
    private Integer age;

    @Positive
    private BigDecimal balance;

    @Pattern(regexp = "^\\+7\\d{10}$")
    private String phone;

    @Past
    private LocalDate birthDate;

    @Valid @NotNull // каскадная валидация вложенного объекта
    private AddressDto address;
}
```

### Применение

```java
@PostMapping
public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request) {
    // Если валидация не пройдена -- MethodArgumentNotValidException (400)
    return ResponseEntity.status(HttpStatus.CREATED).body(userService.create(request));
}
```

### @Valid vs @Validated

| Аспект | @Valid (javax) | @Validated (Spring) |
|--------|---------------|---------------------|
| Группы валидации | Нет | Да |
| На уровне класса | Нет | Да |
| Каскадная валидация | Да | Да |

### Группы валидации

```java
public interface OnCreate {}
public interface OnUpdate {}

public class UserRequest {
    @Null(groups = OnCreate.class)
    @NotNull(groups = OnUpdate.class)
    private Long id;
}

@PostMapping
public UserDto create(@Validated(OnCreate.class) @RequestBody UserRequest request) { }

@PutMapping("/{id}")
public UserDto update(@Validated(OnUpdate.class) @RequestBody UserRequest request) { }
```

> **На собеседовании:** ключевое -- разница между `@Valid` и `@Validated` (группы валидации). Частая ошибка -- забыть `@Valid` перед `@RequestBody` (валидация не выполнится). Также -- не добавить `spring-boot-starter-validation` (в Spring Boot 3 он не входит в starter-web).

[к оглавлению](#spring-framework)

---

## Что такое Spring Security?
<!-- grade: middle -->

Spring Security -- фреймворк для обеспечения безопасности Java-приложений, отвечающий за аутентификацию (кто ты?), авторизацию (что тебе разрешено?) и защиту от распространённых атак (CSRF, Session Fixation).

### Что даёт «из коробки»

- Все эндпоинты защищены (требуют аутентификации)
- Форма логина `/login`
- Защита от CSRF
- Заголовки безопасности (X-Frame-Options, X-Content-Type-Options)

### Базовая конфигурация (Spring Security 6+)

<details>
<summary>Пример SecurityConfig</summary>

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // для REST API
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/users/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(Customizer.withDefaults());
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder().encode("password"))
                .roles("USER").build();
        return new InMemoryUserDetailsManager(user);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

</details>

### UserDetailsService для загрузки из БД

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        com.example.entity.User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new UsernameNotFoundException("Не найден: " + username));
        return org.springframework.security.core.userdetails.User.builder()
                .username(user.getUsername())
                .password(user.getPassword())
                .roles(user.getRoles().stream().map(Role::getName).toArray(String[]::new))
                .build();
    }
}
```

> **На собеседовании:** покажите понимание архитектуры (фильтры, UserDetailsService, PasswordEncoder). Частая ошибка -- хранить пароли в открытом виде или забыть отключить CSRF для REST API без сессий (403 при POST).

[к оглавлению](#spring-framework)

---

## В чём разница между аутентификацией и авторизацией?
<!-- grade: junior -->

Аутентификация (Authentication) -- проверка личности пользователя, ответ на вопрос «Кто ты?». Авторизация (Authorization) -- проверка прав доступа, ответ на вопрос «Что тебе разрешено?».

> Аналогия из жизни: паспортный контроль в аэропорту -- аутентификация (вы -- это вы). Проверка визы -- авторизация (имеете ли вы право въехать в страну).

### В Spring Security

```
HTTP запрос
     |
Аутентификация (AuthenticationFilter)
  "Кто ты?" -> проверка логина/пароля или токена
     |
Авторизация (AuthorizationFilter)
  "Что тебе можно?" -> проверка прав/ролей
     |
Контроллер
```

### Виды аутентификации

- По логину и паролю (Form Login, HTTP Basic)
- По токену (JWT, OAuth2)
- По сертификату (X.509)
- Многофакторная (пароль + SMS-код)

### Виды авторизации

- На основе ролей (ROLE_ADMIN, ROLE_USER)
- На основе прав/authorities (READ_USERS, WRITE_USERS)
- На основе выражений (SpEL)

### Ключевые интерфейсы

- `Authentication` -- представляет аутентифицированного пользователя
- `UserDetails` -- информация о пользователе (логин, пароль, роли)
- `UserDetailsService` -- загрузка пользователя по имени
- `GrantedAuthority` -- право/роль

> **На собеседовании:** ключевое -- чёткое разделение понятий и порядок (сначала аутентификация, потом авторизация). Частая ошибка -- путать `hasRole("ADMIN")` и `hasAuthority("ADMIN")`: `hasRole()` автоматически добавляет префикс `ROLE_`.

[к оглавлению](#spring-framework)

---

## Что такое SecurityFilterChain и как он работает?
<!-- grade: middle -->

SecurityFilterChain -- цепочка сервлетных фильтров, через которую проходит каждый HTTP-запрос в Spring Security. Каждый фильтр выполняет определённую задачу безопасности.

### Порядок основных фильтров

```
HTTP запрос
     |
SecurityContextPersistenceFilter -- загрузка SecurityContext
     |
CsrfFilter -- проверка CSRF-токена
     |
LogoutFilter -- обработка logout
     |
UsernamePasswordAuthenticationFilter -- логин/пароль
     |
BasicAuthenticationFilter -- HTTP Basic
     |
BearerTokenAuthenticationFilter -- JWT/OAuth2
     |
ExceptionTranslationFilter -- обработка ошибок безопасности
     |
AuthorizationFilter -- проверка прав доступа
     |
Контроллер
```

### Добавление собственного фильтра (JWT)

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .csrf(csrf -> csrf.disable())
        .sessionManagement(s -> s.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
        .authorizeHttpRequests(auth -> auth
            .requestMatchers(HttpMethod.POST, "/api/auth/**").permitAll()
            .anyRequest().authenticated()
        )
        .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
    return http.build();
}
```

### Несколько SecurityFilterChain

```java
@Bean @Order(1)
public SecurityFilterChain apiFilterChain(HttpSecurity http) throws Exception {
    http.securityMatcher("/api/**") // для API -- JWT, без сессий
        .csrf(csrf -> csrf.disable())
        .sessionManagement(s -> s.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
        .authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
    return http.build();
}

@Bean @Order(2)
public SecurityFilterChain webFilterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
        .formLogin(Customizer.withDefaults()); // для веб -- форма логина
    return http.build();
}
```

> **На собеседовании:** покажите знание порядка фильтров и умение добавить свой. Частая ошибка -- забыть `filterChain.doFilter(request, response)` в собственном фильтре (запрос «застрянет»). Также -- неправильный `@Order` при нескольких SecurityFilterChain.

[к оглавлению](#spring-framework)

---

## Как работают аннотации @PreAuthorize и @Secured?
<!-- grade: middle -->

`@PreAuthorize` и `@Secured` обеспечивают авторизацию на уровне методов (method-level security). Требуют `@EnableMethodSecurity` для активации.

| Аспект | @PreAuthorize | @Secured |
|--------|---------------|----------|
| SpEL | Да | Нет |
| Доступ к параметрам | Да (`#param`) | Нет |
| Комбинирование условий | Да (and, or) | Нет (только OR между ролями) |
| @PostAuthorize | Да | Нет |
| Гибкость | Высокая | Минимальная |

```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) { }

@PreAuthorize("hasRole('ADMIN') or #userId == authentication.principal.id")
public UserDto getUserProfile(Long userId) {
    // ADMIN видит любой профиль, пользователь -- только свой
}

@PreAuthorize("#request.amount <= 10000 or hasRole('ADMIN')")
public void transfer(TransferRequest request) { }

@Secured("ROLE_ADMIN") // проще, но без SpEL
public void deleteUser(Long id) { }
```

### Получение текущего пользователя

```java
@GetMapping("/me")
public UserDto getCurrentUser(@AuthenticationPrincipal UserDetails userDetails) {
    return userService.findByUsername(userDetails.getUsername());
}
```

> **На собеседовании:** ключевое -- `@PreAuthorize` гибче (SpEL, доступ к параметрам), `@Secured` проще. Частая ошибка -- забыть `@EnableMethodSecurity` (аннотации будут проигнорированы) или использовать на private-методах.

[к оглавлению](#spring-framework)

---

## Как настроить аутентификацию на основе JWT в Spring Security?
<!-- grade: senior -->

JWT (JSON Web Token) -- компактный самодостаточный формат токена для аутентификации в REST API. Структура: `HEADER.PAYLOAD.SIGNATURE`.

<details>
<summary>JwtTokenProvider -- генерация и валидация токенов</summary>

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
                .map(GrantedAuthority::getAuthority).collect(Collectors.toList()));
        return Jwts.builder()
                .claims(claims)
                .subject(userDetails.getUsername())
                .issuedAt(new Date())
                .expiration(new Date(System.currentTimeMillis() + expirationMs))
                .signWith(getSigningKey())
                .compact();
    }

    public String getUsernameFromToken(String token) {
        return Jwts.parser().verifyWith(getSigningKey()).build()
                .parseSignedClaims(token).getPayload().getSubject();
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

</details>

<details>
<summary>JwtAuthenticationFilter</summary>

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired private JwtTokenProvider tokenProvider;
    @Autowired private UserDetailsService userDetailsService;

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
        filterChain.doFilter(request, response);
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

</details>

### Контроллер аутентификации

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    @PostMapping("/login")
    public ResponseEntity<AuthResponse> login(@RequestBody @Valid LoginRequest request) {
        Authentication auth = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(request.getUsername(), request.getPassword()));
        String token = tokenProvider.generateToken((UserDetails) auth.getPrincipal());
        return ResponseEntity.ok(new AuthResponse(token));
    }
}
```

> **На собеседовании:** покажите понимание полного flow: логин -> генерация токена -> фильтр извлекает и проверяет токен -> установка SecurityContext. Частая ошибка -- хранить JWT-секрет в коде, делать слишком долгий TTL access-токена (рекомендуется 15-30 минут), не обрабатывать истечение токена (500 вместо 401).

[к оглавлению](#spring-framework)

---

## Что такое Spring Event? Как создать и обработать событие?
<!-- grade: middle -->

Spring Events -- механизм публикации и обработки событий, реализующий паттерн Observer (Наблюдатель). Позволяет ослабить связность между компонентами.

### Создание события (Spring 4.2+ -- просто POJO)

```java
public class OrderCreatedEvent {
    private final Long orderId;
    private final Long userId;
    private final BigDecimal amount;
    // конструктор, геттеры
}
```

### Публикация события

```java
@Service
public class OrderService {
    @Autowired
    private ApplicationEventPublisher eventPublisher;

    @Transactional
    public Order createOrder(CreateOrderRequest request) {
        Order order = orderRepository.save(new Order(request));
        eventPublisher.publishEvent(new OrderCreatedEvent(
                order.getId(), order.getUserId(), order.getAmount()));
        return order;
    }
}
```

### Обработка события

```java
@Component
public class OrderEventListener {

    // Синхронный (в той же транзакции!)
    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        log.info("Заказ {} создан", event.getOrderId());
    }

    // Асинхронный (в отдельном потоке)
    @Async
    @EventListener
    public void sendNotification(OrderCreatedEvent event) {
        emailService.sendOrderConfirmation(event.getUserId());
    }

    // После коммита транзакции
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void afterOrderCommitted(OrderCreatedEvent event) {
        // выполнится только если транзакция успешно закоммичена
    }

    // Условная обработка
    @EventListener(condition = "#event.amount > 10000")
    public void handleLargeOrder(OrderCreatedEvent event) {
        auditService.flagForReview(event.getOrderId());
    }
}
```

> **На собеседовании:** ключевое -- `@TransactionalEventListener` для гарантии, что обработчик выполнится только после коммита. Частая ошибка -- использовать синхронный `@EventListener` для долгих операций (отправка email) внутри транзакции. Используйте `@Async` или `@TransactionalEventListener(phase = AFTER_COMMIT)`.

[к оглавлению](#spring-framework)

---

## Что такое @Conditional и условная конфигурация в Spring Boot?
<!-- grade: middle -->

`@Conditional` -- базовая аннотация Spring для создания бинов только при выполнении определённых условий. Spring Boot расширяет её множеством специализированных аннотаций, которые лежат в основе механизма автоконфигурации.

```java
@Configuration
public class ConditionalConfig {

    @Bean
    @ConditionalOnClass(name = "com.redis.RedisClient")
    public CacheManager redisCacheManager() {
        return new RedisCacheManager();
    }

    @Bean
    @ConditionalOnMissingBean(CacheManager.class)
    public CacheManager defaultCacheManager() {
        return new ConcurrentMapCacheManager();
    }

    @Bean
    @ConditionalOnProperty(name = "feature.notifications.enabled", havingValue = "true")
    public NotificationService notificationService() {
        return new EmailNotificationService();
    }
}
```

### Feature toggles (частый кейс)

```properties
feature.new-payment-system.enabled=true
```

```java
@Bean
@ConditionalOnProperty(name = "feature.new-payment-system.enabled", havingValue = "true")
public PaymentService newPaymentService() { return new NewPaymentService(); }

@Bean
@ConditionalOnProperty(name = "feature.new-payment-system.enabled",
                       havingValue = "false", matchIfMissing = true)
public PaymentService legacyPaymentService() { return new LegacyPaymentService(); }
```

### Собственное условие

```java
public class OnLinuxCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        String os = System.getProperty("os.name");
        return os != null && os.toLowerCase().contains("linux");
    }
}

@Bean
@Conditional(OnLinuxCondition.class)
public FileWatcher linuxFileWatcher() { return new InotifyFileWatcher(); }
```

> **На собеседовании:** покажите понимание `@ConditionalOnMissingBean` как основы автоконфигурации. Частая ошибка -- чрезмерное использование @Conditional, превращающее конфигурацию в нечитаемую. Если логика сложная -- используйте профили.

[к оглавлению](#spring-framework)

---

## Как тестировать Spring-приложения? Какие основные аннотации для тестирования?
<!-- grade: middle -->

Spring Boot предоставляет инструментарий для тестирования на всех уровнях: от unit-тестов без контекста до полноценных интеграционных тестов.

### Основные аннотации

| Аннотация | Что поднимает | Скорость |
|-----------|---------------|----------|
| (без Spring) | Ничего, чистый Mockito | Быстро |
| `@WebMvcTest` | Только веб-слой (контроллеры) | Быстро |
| `@DataJpaTest` | Только JPA-слой (репозитории) | Средне |
| `@SpringBootTest` | Полный контекст приложения | Медленно |

### 1. Unit-тесты (без Spring-контекста)

```java
class UserServiceTest {
    @Mock private UserRepository userRepository;
    private UserService userService;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
        userService = new UserService(userRepository);
    }

    @Test
    void shouldFindUserById() {
        when(userRepository.findById(1L)).thenReturn(Optional.of(new User(1L, "Ivan")));
        UserDto result = userService.findById(1L);
        assertThat(result.getName()).isEqualTo("Ivan");
    }
}
```

### 2. @WebMvcTest -- тест контроллера

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired private MockMvc mockMvc;
    @MockBean private UserService userService; // Spring-мок в контексте

    @Test
    void shouldReturnUser() throws Exception {
        when(userService.findById(1L)).thenReturn(new UserDto(1L, "Ivan", "ivan@mail.ru"));
        mockMvc.perform(get("/api/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("Ivan"));
    }
}
```

### 3. @DataJpaTest -- тест репозитория

```java
@DataJpaTest
@ActiveProfiles("test")
class UserRepositoryTest {
    @Autowired private UserRepository userRepository;
    @Autowired private TestEntityManager entityManager;

    @Test
    void shouldFindByEmail() {
        entityManager.persistAndFlush(new User("Ivan", "ivan@mail.ru"));
        Optional<User> found = userRepository.findByEmail("ivan@mail.ru");
        assertThat(found).isPresent();
    }
}
```

### 4. @SpringBootTest -- интеграционный тест

```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceIntegrationTest {
    @Autowired private UserService userService;

    @Test @Transactional
    void shouldCreateUser() {
        UserDto result = userService.create(new CreateUserRequest("Ivan", "ivan@mail.ru"));
        assertThat(result.getId()).isNotNull();
    }
}
```

> **На собеседовании:** покажите знание «слайс-тестов» и когда что использовать. Частая ошибка -- использовать `@SpringBootTest` для всех тестов (медленно). Также -- путать `@Mock` (Mockito) и `@MockBean` (Spring): `@Mock` не подменяет бин в контексте.

[к оглавлению](#spring-framework)

---

## Что такое Spring Cache и как его использовать?
<!-- grade: middle -->

Spring Cache -- абстракция кэширования, позволяющая декларативно (через аннотации) кэшировать результаты методов без привязки к конкретной реализации кэша.

### Подключение

```java
@SpringBootApplication
@EnableCaching
public class MyApplication { }
```

### Основные аннотации

```java
@Service
public class UserService {

    // Кэширует результат. При повторном вызове с тем же ключом
    // метод НЕ выполняется, результат берётся из кэша
    @Cacheable(value = "users", key = "#id")
    public UserDto findById(Long id) {
        return userRepository.findById(id).map(this::toDto).orElseThrow();
    }

    // Условное кэширование
    @Cacheable(value = "users", key = "#id", unless = "#result == null")
    public UserDto findByIdConditional(Long id) { }

    // Всегда выполняет метод и обновляет кэш
    @CachePut(value = "users", key = "#result.id")
    public UserDto updateUser(UpdateUserRequest request) { }

    // Удаляет запись из кэша
    @CacheEvict(value = "users", key = "#id")
    public void deleteUser(Long id) { }

    // Очистка всего кэша
    @CacheEvict(value = "users", allEntries = true)
    public void clearUsersCache() { }
}
```

### Реализации кэша

По умолчанию Spring использует `ConcurrentMapCacheManager` (простой HashMap). Для production:

```properties
# Redis
spring.cache.type=redis
spring.cache.redis.time-to-live=600000

# Caffeine (in-memory, рекомендуется для одного экземпляра)
spring.cache.type=caffeine
spring.cache.caffeine.spec=maximumSize=500,expireAfterWrite=10m
```

> **На собеседовании:** ключевое -- знание аннотаций (`@Cacheable`, `@CachePut`, `@CacheEvict`) и их различий. Частая ошибка -- забыть инвалидировать кэш при обновлении данных. Также проблема self-invocation: вызов `@Cacheable`-метода из того же класса не кэшируется (та же проблема, что и с `@Transactional`).

[к оглавлению](#spring-framework)
