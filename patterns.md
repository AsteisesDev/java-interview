[Вопросы для собеседования](README.md)

# Шаблоны проектирования
+ [Что такое шаблон проектирования?](#Что-такое-шаблон-проектирования)
+ [Типы шаблонов проектирования](#Типы-шаблонов-проектирования)
+ [Что такое принципы SOLID?](#Что-такое-принципы-SOLID)
+ [Что такое паттерн Singleton?](#Что-такое-паттерн-Singleton)
+ [Что такое паттерн Builder?](#Что-такое-паттерн-Builder)
+ [Что такое паттерн Factory Method и Abstract Factory?](#Что-такое-паттерн-Factory-Method-и-Abstract-Factory)
+ [Что такое паттерн Strategy?](#Что-такое-паттерн-Strategy)
+ [Что такое паттерн Observer?](#Что-такое-паттерн-Observer)
+ [Что такое паттерн Decorator?](#Что-такое-паттерн-Decorator)
+ [Что такое паттерн Adapter?](#Что-такое-паттерн-Adapter)
+ [Что такое паттерн Proxy?](#Что-такое-паттерн-Proxy)
+ [Что такое паттерн Template Method?](#Что-такое-паттерн-Template-Method)
+ [Что такое паттерн Chain of Responsibility?](#Что-такое-паттерн-Chain-of-Responsibility)
+ [Что такое антипаттерн? Какие антипаттерны вы знаете?](#Что-такое-антипаттерн-Какие-антипаттерны-вы-знаете)
+ [Что такое Dependency Injection?](#Что-такое-Dependency-Injection)

## Что такое шаблон проектирования?

**Шаблон (паттерн) проектирования** — проверенное, типовое решение часто встречающейся проблемы проектирования. Не конкретный код, а концептуальный подход, адаптируемый под конкретную ситуацию.

**Плюсы:**
- Готовые решения для типовых проблем — не нужно изобретать велосипед
- Общий словарь между разработчиками: «используем Strategy» понятнее, чем описание реализации
- Проверены временем и тысячами проектов

**Минусы:**
- Слепое применение усложняет код (паттерн ради паттерна)
- Некоторые паттерны — следствие ограничений языка (в языках с first-class functions Strategy не нужен как отдельный класс)

[к оглавлению](#Шаблоны-проектирования)

## Типы шаблонов проектирования

**Порождающие (Creational)** — абстрагируют процесс создания объектов:
- Singleton, Factory Method, Abstract Factory, Builder, Prototype

**Структурные (Structural)** — определяют способы композиции классов и объектов:
- Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy

**Поведенческие (Behavioral)** — определяют взаимодействие между объектами:
- Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor

[к оглавлению](#Шаблоны-проектирования)

## Что такое принципы SOLID?

SOLID — пять принципов объектно-ориентированного проектирования, сформулированных Робертом Мартином.

### S — Single Responsibility Principle (Принцип единственной ответственности)

Класс должен иметь только одну причину для изменения.

```java
// НАРУШЕНИЕ — класс и обрабатывает заказ, и отправляет email, и сохраняет в БД
class OrderService {
    void processOrder(Order order) {
        validate(order);
        save(order);                    // ответственность: persistence
        sendEmail(order.getEmail());    // ответственность: уведомления
        generatePdf(order);             // ответственность: отчёты
    }
}

// ПРАВИЛЬНО — каждый класс отвечает за одно
class OrderService {
    private final OrderRepository repository;
    private final NotificationService notifications;

    void processOrder(Order order) {
        validate(order);
        repository.save(order);
        notifications.orderCreated(order);
    }
}
```

### O — Open/Closed Principle (Принцип открытости/закрытости)

Класс открыт для расширения, закрыт для модификации.

```java
// НАРУШЕНИЕ — добавление нового типа скидки требует изменения метода
double calculateDiscount(Order order) {
    if (order.getType() == REGULAR) return 0;
    if (order.getType() == VIP) return order.getTotal() * 0.1;
    if (order.getType() == PREMIUM) return order.getTotal() * 0.2; // добавили — модифицировали
}

// ПРАВИЛЬНО — расширение через новый класс, без модификации существующего
interface DiscountStrategy {
    double calculate(Order order);
}
class VipDiscount implements DiscountStrategy { ... }
class PremiumDiscount implements DiscountStrategy { ... }
// Добавление нового типа = новый класс, существующий код не трогается
```

### L — Liskov Substitution Principle (Принцип подстановки Барбары Лисков)

Объекты подклассов должны быть заменяемы объектами суперкласса без нарушения корректности программы.

```java
// НАРУШЕНИЕ — Square нарушает контракт Rectangle
class Rectangle {
    void setWidth(int w) { this.width = w; }
    void setHeight(int h) { this.height = h; }
}
class Square extends Rectangle {
    @Override void setWidth(int w) { this.width = w; this.height = w; } // сюрприз!
}

// ПРАВИЛЬНО — отдельные типы или общий интерфейс Shape
interface Shape {
    double area();
}
record Rectangle(double width, double height) implements Shape {
    public double area() { return width * height; }
}
record Square(double side) implements Shape {
    public double area() { return side * side; }
}
```

### I — Interface Segregation Principle (Принцип разделения интерфейса)

Клиент не должен зависеть от методов, которые он не использует.

```java
// НАРУШЕНИЕ — один "жирный" интерфейс
interface Worker {
    void work();
    void eat();     // роботу не нужно
    void sleep();   // роботу не нужно
}

// ПРАВИЛЬНО — разделённые интерфейсы
interface Workable { void work(); }
interface Feedable { void eat(); }
class Human implements Workable, Feedable { ... }
class Robot implements Workable { ... } // только нужные методы
```

### D — Dependency Inversion Principle (Принцип инверсии зависимостей)

Модули верхнего уровня не должны зависеть от модулей нижнего уровня. Оба должны зависеть от абстракций.

```java
// НАРУШЕНИЕ — сервис зависит от конкретной реализации
class OrderService {
    private MySqlOrderRepository repo = new MySqlOrderRepository(); // жёсткая связь
}

// ПРАВИЛЬНО — зависимость от абстракции
class OrderService {
    private final OrderRepository repo; // интерфейс
    OrderService(OrderRepository repo) { this.repo = repo; } // внедрение
}
```

### Важное
- SOLID — ориентиры, не догмы; слепое следование может привести к over-engineering
- SRP — самый важный и часто нарушаемый принцип
- DIP — фундамент для Dependency Injection (Spring IoC)
- LSP — ключ к корректному наследованию; нарушение — частый источник багов

### Частые ошибки
- **Один класс = один метод** — это не SRP; SRP = одна причина для изменения
- **Абстракция ради абстракции** — интерфейс с одной реализацией не нужен, если не планируется подмена
- **Наследование вместо композиции** — LSP нарушается чаще при глубокой иерархии

### Как используется в 2026
- SOLID — стандартный вопрос на собеседовании Middle+
- Spring Framework построен на DIP (IoC-контейнер)
- OCP реализуется через Strategy, Plugin-архитектуру, Spring Profiles

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Singleton?

**Singleton** гарантирует, что у класса есть только один экземпляр, и предоставляет глобальную точку доступа к нему.

```java
// 1. Enum Singleton — рекомендуемый способ (Effective Java, Joshua Bloch)
public enum AppConfig {
    INSTANCE;

    private final Map<String, String> properties = new HashMap<>();

    public String get(String key) { return properties.get(key); }
    public void set(String key, String value) { properties.put(key, value); }
}
// Использование: AppConfig.INSTANCE.get("db.url")

// 2. Static Holder — ленивая инициализация, потокобезопасная
public class Singleton {
    private Singleton() {}

    private static class Holder {
        static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}

// 3. Double-Checked Locking (если нужна параметризованная инициализация)
public class Singleton {
    private static volatile Singleton instance;

    public static Singleton getInstance() {
        Singleton local = instance;
        if (local == null) {
            synchronized (Singleton.class) {
                local = instance;
                if (local == null) {
                    instance = local = new Singleton();
                }
            }
        }
        return local;
    }
}
```

### Важное
- Enum Singleton — самый простой, потокобезопасный, защищён от reflection и сериализации
- В Spring все бины — singleton по умолчанию (управляемый контейнером)
- Singleton затрудняет тестирование — нельзя подменить; в Spring решается через DI

### Частые ошибки
- **Double-checked locking без `volatile`** — объект может быть частично инициализирован
- **Singleton как глобальная переменная** — если нужен глобальный доступ, возможно, нужен DI
- **Singleton в многоклассовом загрузчике** — каждый ClassLoader создаёт свой экземпляр

### Как используется в 2026
- Ручной Singleton редок — Spring управляет жизненным циклом бинов
- Enum Singleton — для утилитных объектов вне Spring-контекста

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Builder?

**Builder** отделяет конструирование сложного объекта от его представления, позволяя создавать объекты пошагово.

```java
// Ручной Builder
public class HttpRequest {
    private final String url;
    private final String method;
    private final Map<String, String> headers;
    private final String body;

    private HttpRequest(Builder builder) {
        this.url = builder.url;
        this.method = builder.method;
        this.headers = Map.copyOf(builder.headers);
        this.body = builder.body;
    }

    public static class Builder {
        private final String url;       // обязательный
        private String method = "GET";  // по умолчанию
        private final Map<String, String> headers = new HashMap<>();
        private String body;

        public Builder(String url) { this.url = url; }

        public Builder method(String method) { this.method = method; return this; }
        public Builder header(String key, String value) { headers.put(key, value); return this; }
        public Builder body(String body) { this.body = body; return this; }

        public HttpRequest build() {
            if (url == null) throw new IllegalStateException("URL is required");
            return new HttpRequest(this);
        }
    }
}

// Использование — читаемо, невозможно забыть обязательные параметры
HttpRequest request = new HttpRequest.Builder("https://api.example.com")
    .method("POST")
    .header("Content-Type", "application/json")
    .body("{\"name\": \"John\"}")
    .build();
```

```java
// Lombok @Builder — генерирует Builder автоматически
@Builder
@Value // immutable
public class UserDto {
    String name;
    String email;
    int age;
}

UserDto user = UserDto.builder()
    .name("John")
    .email("john@mail.com")
    .age(30)
    .build();
```

### Важное
- Builder решает проблему «телескопического конструктора» (много параметров, часть optional)
- Объект, созданный Builder, обычно immutable
- Lombok `@Builder` — стандарт в Spring-проектах для DTO

### Частые ошибки
- **Builder для объекта с 2 полями** — over-engineering; достаточно конструктора или record
- **Мутабельный объект после build()** — Builder должен создавать immutable объект
- **Повторное использование Builder** — после `build()` Builder не должен использоваться повторно

### Как используется в 2026
- Lombok `@Builder` — повсеместно в Spring-проектах
- Java Records — для простых DTO без Builder; Builder — для объектов с optional полями
- Fluent API в JDK: `HttpClient.newBuilder()`, `HttpRequest.newBuilder()`, `ProcessBuilder`

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Factory Method и Abstract Factory?

**Factory Method** — определяет интерфейс для создания объекта, но позволяет подклассам решать, какой класс создавать.

```java
// Factory Method
interface Notification {
    void send(String message);
}

class EmailNotification implements Notification {
    public void send(String message) { /* отправка email */ }
}
class SmsNotification implements Notification {
    public void send(String message) { /* отправка SMS */ }
}
class PushNotification implements Notification {
    public void send(String message) { /* push-уведомление */ }
}

// Фабричный метод
class NotificationFactory {
    static Notification create(String type) {
        return switch (type) {
            case "email" -> new EmailNotification();
            case "sms"   -> new SmsNotification();
            case "push"  -> new PushNotification();
            default -> throw new IllegalArgumentException("Unknown type: " + type);
        };
    }
}

// Использование
Notification notification = NotificationFactory.create("email");
notification.send("Привет!");
```

**Abstract Factory** — предоставляет интерфейс для создания семейств связанных объектов без указания конкретных классов.

```java
interface UIFactory {
    Button createButton();
    TextField createTextField();
}

class MaterialUIFactory implements UIFactory {
    public Button createButton() { return new MaterialButton(); }
    public TextField createTextField() { return new MaterialTextField(); }
}
class IOSUIFactory implements UIFactory {
    public Button createButton() { return new IOSButton(); }
    public TextField createTextField() { return new IOSTextField(); }
}
```

### Важное
- Factory Method — один продукт, выбор реализации; Abstract Factory — семейство связанных продуктов
- В Spring: `BeanFactory` — реализация Factory; `FactoryBean<T>` — кастомная фабрика бинов
- `switch` с sealed interface — современная реализация Factory в Java 21+

### Частые ошибки
- **Factory для одной реализации** — если нет вариативности, фабрика не нужна
- **God Factory** — фабрика, знающая о 50 типах; лучше разбить на несколько

### Как используется в 2026
- Spring создаёт бины через фабрику (ApplicationContext)
- `@ConditionalOnProperty` — условное создание бинов, фабрика на уровне конфигурации

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Strategy?

**Strategy** — определяет семейство алгоритмов, инкапсулирует каждый и делает их взаимозаменяемыми. Клиент выбирает алгоритм в runtime.

```java
// Интерфейс стратегии
interface PricingStrategy {
    BigDecimal calculatePrice(BigDecimal basePrice, int quantity);
}

// Конкретные стратегии
class RegularPricing implements PricingStrategy {
    public BigDecimal calculatePrice(BigDecimal basePrice, int quantity) {
        return basePrice.multiply(BigDecimal.valueOf(quantity));
    }
}

class BulkPricing implements PricingStrategy {
    public BigDecimal calculatePrice(BigDecimal basePrice, int quantity) {
        BigDecimal discount = quantity > 100 ? new BigDecimal("0.8") : new BigDecimal("0.9");
        return basePrice.multiply(BigDecimal.valueOf(quantity)).multiply(discount);
    }
}

// Контекст, использующий стратегию
class OrderService {
    private final PricingStrategy pricingStrategy;

    OrderService(PricingStrategy pricingStrategy) { // внедряется через DI
        this.pricingStrategy = pricingStrategy;
    }

    BigDecimal calculateTotal(BigDecimal price, int qty) {
        return pricingStrategy.calculatePrice(price, qty);
    }
}
```

**С лямбдами (Java 8+) — Strategy без отдельных классов:**

```java
// Стратегия как Function
Map<String, BiFunction<BigDecimal, Integer, BigDecimal>> strategies = Map.of(
    "regular", (price, qty) -> price.multiply(BigDecimal.valueOf(qty)),
    "bulk",    (price, qty) -> price.multiply(BigDecimal.valueOf(qty))
                                    .multiply(new BigDecimal("0.85"))
);

BigDecimal total = strategies.get("bulk").apply(price, quantity);
```

### Важное
- Strategy = OCP (Open/Closed Principle) на практике
- В Java 8+ простые Strategy заменяются лямбдами (`Comparator`, `Function`)
- Spring: стратегии как бины, выбор через `@Qualifier` или `Map<String, Strategy>`

### Частые ошибки
- **Strategy + if/else для выбора** — выбор стратегии через DI/Map, а не через if/else
- **Strategy для двух строк кода** — лямбда проще отдельного класса

### Как используется в 2026
- `Comparator`, `Predicate`, `Function` — встроенные Strategy в JDK
- Spring: `List<PaymentStrategy>` → Spring автоматически собирает все реализации

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Observer?

**Observer** — определяет зависимость «один ко многим» между объектами: при изменении состояния одного все зависимые уведомляются автоматически.

```java
// Java встроенная поддержка — через функциональные интерфейсы
interface EventListener<T> {
    void onEvent(T event);
}

class EventBus<T> {
    private final List<EventListener<T>> listeners = new CopyOnWriteArrayList<>();

    void subscribe(EventListener<T> listener) { listeners.add(listener); }
    void unsubscribe(EventListener<T> listener) { listeners.remove(listener); }

    void publish(T event) {
        listeners.forEach(listener -> listener.onEvent(event));
    }
}

// Использование
EventBus<OrderCreatedEvent> bus = new EventBus<>();
bus.subscribe(event -> emailService.sendConfirmation(event));
bus.subscribe(event -> analyticsService.track(event));
bus.publish(new OrderCreatedEvent(orderId));
```

### Важное
- Observer = основа event-driven архитектуры
- Spring Events (`ApplicationEventPublisher`) — Observer из коробки
- `CopyOnWriteArrayList` — потокобезопасный список подписчиков

### Частые ошибки
- **Утечки памяти** — подписчик не отписывается; в Java нет weak listeners по умолчанию
- **Синхронный Observer для тяжёлых операций** — блокирует publisher; используйте `@Async` или очереди

### Как используется в 2026
- Spring `@EventListener` / `@TransactionalEventListener` — стандартная реализация
- Kafka, RabbitMQ — Observer на уровне распределённой системы

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Decorator?

**Decorator** — динамически добавляет объекту новую функциональность, оборачивая его. Альтернатива наследованию.

```java
// Классический пример — java.io
InputStream raw = new FileInputStream("data.txt");
InputStream buffered = new BufferedInputStream(raw);           // декоратор: буферизация
InputStream gzip = new GZIPInputStream(buffered);              // декоратор: распаковка
Reader reader = new InputStreamReader(gzip, StandardCharsets.UTF_8); // декоратор: символьный поток

// Собственный Decorator
interface Logger {
    void log(String message);
}

class ConsoleLogger implements Logger {
    public void log(String message) { System.out.println(message); }
}

class TimestampLogger implements Logger {  // декоратор
    private final Logger wrapped;
    TimestampLogger(Logger wrapped) { this.wrapped = wrapped; }

    public void log(String message) {
        wrapped.log("[" + Instant.now() + "] " + message);
    }
}

class UpperCaseLogger implements Logger {  // декоратор
    private final Logger wrapped;
    UpperCaseLogger(Logger wrapped) { this.wrapped = wrapped; }

    public void log(String message) { wrapped.log(message.toUpperCase()); }
}

// Комбинирование декораторов
Logger logger = new TimestampLogger(new UpperCaseLogger(new ConsoleLogger()));
logger.log("hello"); // [2026-04-22T10:00:00Z] HELLO
```

### Важное
- Decorator реализует тот же интерфейс, что и оборачиваемый объект
- `java.io` — классический пример Decorator в JDK
- Альтернатива множественному наследованию — композиция декораторов

### Частые ошибки
- **Слишком глубокая вложенность** — 5+ декораторов усложняют отладку (стек-трейс длинный)
- **Decorator vs Proxy** — Decorator добавляет функциональность, Proxy контролирует доступ

### Как используется в 2026
- `java.io` (streams), `Collections.unmodifiable*()`, `Collections.synchronized*()`
- Spring: `HandlerInterceptor`, `Filter` — по сути декораторы HTTP-обработки

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Adapter?

**Adapter** — преобразует интерфейс класса в другой интерфейс, ожидаемый клиентом. Позволяет работать вместе классам с несовместимыми интерфейсами.

```java
// Существующий интерфейс
interface MediaPlayer {
    void play(String filename);
}

// Несовместимый интерфейс стороннего API
class AdvancedPlayer {
    void playMp4(String file) { /* ... */ }
    void playMkv(String file) { /* ... */ }
}

// Адаптер
class MediaAdapter implements MediaPlayer {
    private final AdvancedPlayer advancedPlayer = new AdvancedPlayer();

    @Override
    public void play(String filename) {
        if (filename.endsWith(".mp4")) advancedPlayer.playMp4(filename);
        else if (filename.endsWith(".mkv")) advancedPlayer.playMkv(filename);
    }
}
```

**Примеры Adapter в JDK:**
- `Arrays.asList(array)` — адаптирует массив к интерфейсу `List`
- `InputStreamReader` — адаптирует `InputStream` (байты) к `Reader` (символы)
- `Collections.enumeration(collection)` — адаптирует `Collection` к `Enumeration`

### Важное
- Adapter — «переводчик» между несовместимыми интерфейсами
- Два типа: Object Adapter (композиция) и Class Adapter (наследование); в Java предпочтительна композиция

### Частые ошибки
- **Adapter с бизнес-логикой** — адаптер только преобразует интерфейс, не добавляет логику
- **Путать Adapter и Facade** — Adapter адаптирует один интерфейс к другому, Facade упрощает сложную подсистему

### Как используется в 2026
- Spring: `HandlerAdapter` адаптирует разные типы контроллеров к единому `DispatcherServlet`
- Интеграция legacy-систем — адаптеры для старых API

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Proxy?

**Proxy** — предоставляет объект-заместитель, контролирующий доступ к другому объекту.

```java
interface UserService {
    User findById(Long id);
}

class UserServiceImpl implements UserService {
    public User findById(Long id) {
        return database.query("SELECT * FROM users WHERE id = ?", id);
    }
}

// Proxy: кэширование
class CachingUserServiceProxy implements UserService {
    private final UserService delegate;
    private final Map<Long, User> cache = new ConcurrentHashMap<>();

    CachingUserServiceProxy(UserService delegate) { this.delegate = delegate; }

    public User findById(Long id) {
        return cache.computeIfAbsent(id, delegate::findById);
    }
}

// Proxy: логирование
class LoggingUserServiceProxy implements UserService {
    private final UserService delegate;

    public User findById(Long id) {
        log.info("findById called with id={}", id);
        User result = delegate.findById(id);
        log.info("findById returned: {}", result);
        return result;
    }
}
```

**JDK Dynamic Proxy — создание прокси в runtime:**

```java
UserService proxy = (UserService) Proxy.newProxyInstance(
    UserService.class.getClassLoader(),
    new Class[]{UserService.class},
    (proxyObj, method, args) -> {
        log.info("Вызов: {}", method.getName());
        long start = System.nanoTime();
        Object result = method.invoke(realService, args);
        log.info("Время: {} мс", (System.nanoTime() - start) / 1_000_000);
        return result;
    }
);
```

### Важное
- **Spring AOP** построен на Proxy: `@Transactional`, `@Cacheable`, `@Async` работают через прокси
- JDK Dynamic Proxy — для интерфейсов; CGLIB — для классов (наследование)
- Виды: Virtual Proxy (ленивая загрузка), Protection Proxy (контроль доступа), Caching Proxy, Logging Proxy

### Частые ошибки
- **Вызов метода внутри того же класса** — Spring Proxy не перехватывает self-invocation: `this.method()` обходит прокси
- **Proxy на final-класс** — CGLIB не может создать прокси для final-класса

### Как используется в 2026
- Spring AOP: `@Transactional`, `@Cacheable`, `@Async`, `@PreAuthorize` — всё через Proxy
- Hibernate Lazy Loading — Proxy для отложенной загрузки сущностей

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Template Method?

**Template Method** — определяет скелет алгоритма в базовом классе, позволяя подклассам переопределять отдельные шаги без изменения структуры.

```java
abstract class ReportGenerator {
    // Template Method — определяет порядок шагов
    final void generateReport() {
        fetchData();
        processData();
        formatReport();
        deliver();
    }

    abstract void fetchData();
    abstract void processData();

    void formatReport() { /* формат по умолчанию — PDF */ }
    void deliver() { /* email по умолчанию */ }
}

class SalesReport extends ReportGenerator {
    void fetchData() { /* запрос к БД продаж */ }
    void processData() { /* агрегация по регионам */ }
    // formatReport() и deliver() используют реализацию по умолчанию
}

class InventoryReport extends ReportGenerator {
    void fetchData() { /* запрос к складской БД */ }
    void processData() { /* подсчёт остатков */ }
    @Override void deliver() { /* выгрузка в S3, а не email */ }
}
```

**Примеры Template Method в JDK и Spring:**
- `AbstractList.get()` — подклассы определяют доступ к элементам
- `HttpServlet.doGet()` / `doPost()` — скелет обработки запроса
- Spring `JdbcTemplate.query()` — скелет работы с JDBC (открытие/закрытие соединений)
- Spring `AbstractController` — скелет обработки HTTP-запроса

### Важное
- `final` на template method — запрещает подклассам менять порядок шагов
- «Голливудский принцип»: не звоните нам, мы сами вам позвоним (framework вызывает ваш код)
- Spring Framework активно использует Template Method: `JdbcTemplate`, `RestTemplate`, `TransactionTemplate`

### Частые ошибки
- **Слишком много абстрактных шагов** — класс становится невозможно реализовать
- **Глубокая иерархия** — Template Method провоцирует наследование; предпочитайте Strategy (композиция)

### Как используется в 2026
- Spring `*Template` классы — JdbcTemplate, RestTemplate, KafkaTemplate
- Тренд: Strategy (композиция) предпочтительнее Template Method (наследование) для новых проектов

[к оглавлению](#Шаблоны-проектирования)

## Что такое паттерн Chain of Responsibility?

**Chain of Responsibility** — передаёт запрос по цепочке обработчиков. Каждый обработчик решает, обработать запрос или передать следующему.

```java
abstract class Handler {
    private Handler next;

    Handler setNext(Handler next) {
        this.next = next;
        return next;
    }

    void handle(Request request) {
        if (canHandle(request)) {
            process(request);
        } else if (next != null) {
            next.handle(request);
        }
    }

    abstract boolean canHandle(Request request);
    abstract void process(Request request);
}

class AuthenticationHandler extends Handler {
    boolean canHandle(Request req) { return true; } // всегда проверяет
    void process(Request req) {
        if (!req.hasValidToken()) throw new UnauthorizedException();
        // передать дальше
    }
}

class RateLimitHandler extends Handler {
    boolean canHandle(Request req) { return true; }
    void process(Request req) {
        if (rateLimiter.isExceeded(req.getIp())) throw new TooManyRequestsException();
    }
}

// Построение цепочки
Handler chain = new AuthenticationHandler();
chain.setNext(new RateLimitHandler())
     .setNext(new BusinessLogicHandler());

chain.handle(request);
```

**Примеры в Java-экосистеме:**
- **Servlet Filter** — `FilterChain.doFilter()` — каждый фильтр вызывает следующий
- **Spring Security FilterChain** — цепочка security-фильтров
- **Spring Interceptors** — `HandlerInterceptor.preHandle()` / `postHandle()`
- **java.util.logging Handler** — обработчики логов

### Важное
- Каждый обработчик отвечает за одну задачу (SRP)
- Порядок обработчиков важен (auth → rate limit → validation → business logic)
- Обработчик может прервать цепочку или передать управление дальше

### Частые ошибки
- **Запрос не обрабатывается никем** — нужен default handler в конце цепочки
- **Слишком длинная цепочка** — усложняет отладку; логируйте прохождение через каждый handler

### Как используется в 2026
- Servlet Filters, Spring Security — фундаментально построены на Chain of Responsibility
- Middleware-паттерн в HTTP-фреймворках (аналогично)

[к оглавлению](#Шаблоны-проектирования)

## Что такое антипаттерн? Какие антипаттерны вы знаете?

**Антипаттерн** — распространённый подход к решению проблемы, который является неэффективным или контрпродуктивным.

| Антипаттерн | Описание |
|-------------|----------|
| **God Object** | Класс, знающий и делающий слишком много (нарушение SRP) |
| **Spaghetti Code** | Запутанный код без структуры, сложно читать и поддерживать |
| **Golden Hammer** | Использование одного инструмента для всех задач («у меня есть молоток...») |
| **Copy-Paste Programming** | Дублирование кода вместо выделения общей логики |
| **Premature Optimization** | Оптимизация до измерения производительности |
| **Magic Numbers/Strings** | Литералы без имён: `if (status == 3)` вместо `if (status == Status.APPROVED)` |
| **Cargo Cult Programming** | Использование паттернов/технологий без понимания зачем |
| **Lava Flow** | Мёртвый код, который боятся удалить («вдруг понадобится») |
| **Big Ball of Mud** | Система без различимой архитектуры |
| **Poltergeist** | Классы с единственной ролью — передача вызова другому классу |

### Важное
- Антипаттерны часто появляются из-за спешки, недостатка опыта или требований
- God Object — самый частый; бороться через декомпозицию (SRP)
- «Лучший код — удалённый код» — не бойтесь удалять Lava Flow

### Частые ошибки
- **Рефакторинг ради рефакторинга** — антипаттерн стоит исправлять, если он мешает, а не потому что «некрасиво»
- **Замена одного антипаттерна другим** — God Object → 50 микроклассов (Ravioli Code)

### Как используется в 2026
- Code review — основной инструмент выявления антипаттернов
- Static analysis (SonarQube, SpotBugs) — автоматическое обнаружение God Object, дублирования
- ArchUnit — архитектурные тесты для предотвращения нарушений

[к оглавлению](#Шаблоны-проектирования)

## Что такое Dependency Injection?

**Dependency Injection (DI)** — паттерн, при котором объект получает свои зависимости извне, а не создаёт их сам. Реализует принцип Dependency Inversion (SOLID).

```java
// БЕЗ DI — жёсткая связность
class OrderService {
    private final OrderRepository repo = new PostgresOrderRepository(); // жёсткая связь
    private final EmailService email = new SmtpEmailService();          // жёсткая связь
}

// С DI — слабая связность
class OrderService {
    private final OrderRepository repo;
    private final EmailService email;

    // Зависимости внедряются через конструктор (рекомендуемый способ)
    OrderService(OrderRepository repo, EmailService email) {
        this.repo = repo;
        this.email = email;
    }
}
```

**Три способа внедрения в Spring:**

```java
// 1. Через конструктор (РЕКОМЕНДУЕТСЯ)
@Service
class OrderService {
    private final OrderRepository repo;
    OrderService(OrderRepository repo) { this.repo = repo; }
}

// 2. Через сеттер
@Service
class OrderService {
    private OrderRepository repo;
    @Autowired
    void setRepo(OrderRepository repo) { this.repo = repo; }
}

// 3. Через поле (НЕ РЕКОМЕНДУЕТСЯ — затрудняет тестирование)
@Service
class OrderService {
    @Autowired
    private OrderRepository repo;
}
```

### Важное
- DI = реализация Dependency Inversion Principle (D из SOLID)
- Конструктор — лучший способ: поля final, объект полностью инициализирован, легко тестировать
- IoC-контейнер (Spring) — автоматизирует DI, управляя созданием и связыванием объектов

### Частые ошибки
- **Field injection** — невозможно создать объект без рефлексии; `new OrderService()` не скомпилируется
- **Циклические зависимости** — A зависит от B, B от A; признак плохого дизайна
- **`new` внутри сервиса для зависимостей** — нарушает DI; используйте фабрику или Provider

### Как используется в 2026
- Spring IoC — стандартный DI-контейнер в Java-экосистеме
- Constructor injection — единственный рекомендуемый способ

[к оглавлению](#Шаблоны-проектирования)

[Вопросы для собеседования](README.md)
