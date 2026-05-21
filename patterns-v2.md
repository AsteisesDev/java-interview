[Вопросы для собеседования](README.md)

# Шаблоны проектирования

+ [Что такое шаблон проектирования?](#что-такое-шаблон-проектирования)
+ [Типы шаблонов проектирования](#типы-шаблонов-проектирования)
+ [Что такое принципы SOLID?](#что-такое-принципы-solid)
+ [Что такое паттерн Singleton?](#что-такое-паттерн-singleton)
+ [Что такое паттерн Builder?](#что-такое-паттерн-builder)
+ [Что такое паттерн Factory Method и Abstract Factory?](#что-такое-паттерн-factory-method-и-abstract-factory)
+ [Что такое паттерн Strategy?](#что-такое-паттерн-strategy)
+ [Что такое паттерн Observer?](#что-такое-паттерн-observer)
+ [Что такое паттерн Decorator?](#что-такое-паттерн-decorator)
+ [Что такое паттерн Adapter?](#что-такое-паттерн-adapter)
+ [Что такое паттерн Proxy?](#что-такое-паттерн-proxy)
+ [Что такое паттерн Template Method?](#что-такое-паттерн-template-method)
+ [Что такое паттерн Chain of Responsibility?](#что-такое-паттерн-chain-of-responsibility)
+ [Что такое антипаттерн? Какие антипаттерны вы знаете?](#что-такое-антипаттерн-какие-антипаттерны-вы-знаете)
+ [Что такое Dependency Injection?](#что-такое-dependency-injection)

## Что такое шаблон проектирования?
<!-- grade: junior -->

Шаблон (паттерн) проектирования -- проверенное, типовое решение часто встречающейся проблемы проектирования. Не конкретный код, а концептуальный подход, адаптируемый под конкретную ситуацию.

> Аналогия из жизни: паттерн -- это как рецепт блюда. Рецепт не привязан к конкретной кухне или продуктам, но описывает последовательность шагов, которые дают предсказуемый результат.

### Плюсы

- Готовые решения для типовых проблем -- не нужно изобретать велосипед
- Общий словарь между разработчиками: "используем Strategy" понятнее, чем описание реализации
- Проверены временем и тысячами проектов

### Минусы

- Слепое применение усложняет код (паттерн ради паттерна)
- Некоторые паттерны -- следствие ограничений языка (в языках с first-class functions Strategy не нужен как отдельный класс)

> **На собеседовании:** интервьюер хочет услышать определение и понимание того, что паттерн -- это не готовый код, а концептуальная схема. Частая ошибка -- перечислять паттерны вместо объяснения сути понятия.

[к оглавлению](#шаблоны-проектирования)

---

## Типы шаблонов проектирования
<!-- grade: junior -->

Шаблоны проектирования делятся на три группы по назначению: порождающие, структурные и поведенческие.

| Тип | Назначение | Примеры |
|-----|-----------|---------|
| Порождающие (Creational) | Абстрагируют процесс создания объектов | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| Структурные (Structural) | Определяют способы композиции классов и объектов | Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy |
| Поведенческие (Behavioral) | Определяют взаимодействие между объектами | Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor |

> **На собеседовании:** достаточно назвать три группы и привести по 2-3 примера из каждой. Частая ошибка -- путать структурные и поведенческие паттерны (например, относить Decorator к поведенческим).

[к оглавлению](#шаблоны-проектирования)

---

## Что такое принципы SOLID?
<!-- grade: middle -->

SOLID -- пять принципов объектно-ориентированного проектирования, сформулированных Робертом Мартином. Они помогают создавать код, который легко поддерживать, расширять и тестировать.

### S -- Single Responsibility Principle (Принцип единственной ответственности)

Класс должен иметь только одну причину для изменения.

<details>
<summary>Пример кода</summary>

```java
// НАРУШЕНИЕ -- класс и обрабатывает заказ, и отправляет email, и сохраняет в БД
class OrderService {
    void processOrder(Order order) {
        validate(order);
        save(order);                    // ответственность: persistence
        sendEmail(order.getEmail());    // ответственность: уведомления
        generatePdf(order);             // ответственность: отчёты
    }
}

// ПРАВИЛЬНО -- каждый класс отвечает за одно
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

</details>

### O -- Open/Closed Principle (Принцип открытости/закрытости)

Класс открыт для расширения, закрыт для модификации.

<details>
<summary>Пример кода</summary>

```java
// НАРУШЕНИЕ -- добавление нового типа скидки требует изменения метода
double calculateDiscount(Order order) {
    if (order.getType() == REGULAR) return 0;
    if (order.getType() == VIP) return order.getTotal() * 0.1;
    if (order.getType() == PREMIUM) return order.getTotal() * 0.2; // добавили -- модифицировали
}

// ПРАВИЛЬНО -- расширение через новый класс, без модификации существующего
interface DiscountStrategy {
    double calculate(Order order);
}
class VipDiscount implements DiscountStrategy { ... }
class PremiumDiscount implements DiscountStrategy { ... }
// Добавление нового типа = новый класс, существующий код не трогается
```

</details>

### L -- Liskov Substitution Principle (Принцип подстановки Барбары Лисков)

Объекты подклассов должны быть заменяемы объектами суперкласса без нарушения корректности программы.

<details>
<summary>Пример кода</summary>

```java
// НАРУШЕНИЕ -- Square нарушает контракт Rectangle
class Rectangle {
    void setWidth(int w) { this.width = w; }
    void setHeight(int h) { this.height = h; }
}
class Square extends Rectangle {
    @Override void setWidth(int w) { this.width = w; this.height = w; } // сюрприз!
}

// ПРАВИЛЬНО -- отдельные типы или общий интерфейс Shape
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

</details>

### I -- Interface Segregation Principle (Принцип разделения интерфейса)

Клиент не должен зависеть от методов, которые он не использует.

<details>
<summary>Пример кода</summary>

```java
// НАРУШЕНИЕ -- один "жирный" интерфейс
interface Worker {
    void work();
    void eat();     // роботу не нужно
    void sleep();   // роботу не нужно
}

// ПРАВИЛЬНО -- разделённые интерфейсы
interface Workable { void work(); }
interface Feedable { void eat(); }
class Human implements Workable, Feedable { ... }
class Robot implements Workable { ... } // только нужные методы
```

</details>

### D -- Dependency Inversion Principle (Принцип инверсии зависимостей)

Модули верхнего уровня не должны зависеть от модулей нижнего уровня. Оба должны зависеть от абстракций.

<details>
<summary>Пример кода</summary>

```java
// НАРУШЕНИЕ -- сервис зависит от конкретной реализации
class OrderService {
    private MySqlOrderRepository repo = new MySqlOrderRepository(); // жёсткая связь
}

// ПРАВИЛЬНО -- зависимость от абстракции
class OrderService {
    private final OrderRepository repo; // интерфейс
    OrderService(OrderRepository repo) { this.repo = repo; } // внедрение
}
```

</details>

### Важное

- SOLID -- ориентиры, не догмы; слепое следование может привести к over-engineering
- SRP -- самый важный и часто нарушаемый принцип
- DIP -- фундамент для Dependency Injection (Spring IoC)
- LSP -- ключ к корректному наследованию; нарушение -- частый источник багов

### Частые ошибки

- Один класс = один метод -- это не SRP; SRP = одна причина для изменения
- Абстракция ради абстракции -- интерфейс с одной реализацией не нужен, если не планируется подмена
- Наследование вместо композиции -- LSP нарушается чаще при глубокой иерархии

### Как используется в 2026

- SOLID -- стандартный вопрос на собеседовании Middle+
- Spring Framework построен на DIP (IoC-контейнер)
- OCP реализуется через Strategy, Plugin-архитектуру, Spring Profiles

> **На собеседовании:** от Middle ожидают не заученные определения, а примеры из собственного опыта: "я нарушал SRP, когда...", "мы применили OCP через...". Частая ошибка -- путать SRP с "один метод в классе" и не уметь объяснить DIP своими словами.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Singleton?
<!-- grade: junior -->

Singleton -- паттерн, гарантирующий, что у класса есть только один экземпляр, и предоставляющий глобальную точку доступа к нему.

> Аналогия из жизни: Singleton -- как президент страны. В один момент времени может быть только один, и все обращаются к одному и тому же человеку.

### Способы реализации

| Способ | Ленивость | Потокобезопасность | Защита от reflection | Простота |
|--------|-----------|-------------------|----------------------|----------|
| Enum Singleton | Нет | Да | Да | Максимальная |
| Static Holder (Bill Pugh) | Да | Да | Нет | Высокая |
| Double-Checked Locking | Да | Да (volatile) | Нет | Средняя |

<details>
<summary>Примеры реализации</summary>

```java
// 1. Enum Singleton -- рекомендуемый способ (Effective Java, Joshua Bloch)
public enum AppConfig {
    INSTANCE;

    private final Map<String, String> properties = new HashMap<>();

    public String get(String key) { return properties.get(key); }
    public void set(String key, String value) { properties.put(key, value); }
}
// Использование: AppConfig.INSTANCE.get("db.url")

// 2. Static Holder -- ленивая инициализация, потокобезопасная
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

</details>

### Частые ошибки

- Double-checked locking без volatile -- объект может быть частично инициализирован
- Singleton как глобальная переменная -- если нужен глобальный доступ, возможно, нужен DI
- Singleton в многоклассовом загрузчике -- каждый ClassLoader создаёт свой экземпляр

### Как используется в 2026

- Ручной Singleton редок -- Spring управляет жизненным циклом бинов
- В Spring все бины -- singleton по умолчанию (управляемый контейнером)
- Enum Singleton -- для утилитных объектов вне Spring-контекста

> **На собеседовании:** интервьюер часто просит реализовать Singleton и объяснить потокобезопасность. Назовите Enum-вариант первым -- это покажет знание Effective Java. Частая ошибка -- забыть volatile в DCL или не упомянуть, что Spring уже решает эту задачу.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Builder?
<!-- grade: junior -->

Builder -- паттерн, отделяющий конструирование сложного объекта от его представления, позволяя создавать объекты пошагово.

> Аналогия из жизни: Builder -- как конструктор бургера в приложении доставки. Вы выбираете булочку, котлету, соус и добавки шаг за шагом, а в конце нажимаете "Заказать" -- и получаете готовый бургер.

<details>
<summary>Ручной Builder</summary>

```java
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

// Использование -- читаемо, невозможно забыть обязательные параметры
HttpRequest request = new HttpRequest.Builder("https://api.example.com")
    .method("POST")
    .header("Content-Type", "application/json")
    .body("{\"name\": \"John\"}")
    .build();
```

</details>

```java
// Lombok @Builder -- генерирует Builder автоматически
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

- Builder решает проблему "телескопического конструктора" (много параметров, часть optional)
- Объект, созданный Builder, обычно immutable
- Lombok `@Builder` -- стандарт в Spring-проектах для DTO

### Частые ошибки

- Builder для объекта с 2 полями -- over-engineering; достаточно конструктора или record
- Мутабельный объект после build() -- Builder должен создавать immutable объект
- Повторное использование Builder -- после build() Builder не должен использоваться повторно

### Как используется в 2026

- Lombok `@Builder` -- повсеместно в Spring-проектах
- Java Records -- для простых DTO без Builder; Builder -- для объектов с optional полями
- Fluent API в JDK: `HttpClient.newBuilder()`, `HttpRequest.newBuilder()`, `ProcessBuilder`

> **На собеседовании:** покажите, что понимаете, когда Builder нужен (много optional-параметров, immutable-объект), а когда избыточен (2-3 поля -- достаточно record). Частая ошибка -- не упомянуть Lombok и писать Builder вручную в 2026 году.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Factory Method и Abstract Factory?
<!-- grade: middle -->

Factory Method -- паттерн, определяющий интерфейс для создания объекта, но позволяющий подклассам решать, какой класс создавать. Abstract Factory -- интерфейс для создания семейств связанных объектов без указания конкретных классов.

| Критерий | Factory Method | Abstract Factory |
|----------|---------------|-----------------|
| Что создаёт | Один продукт | Семейство связанных продуктов |
| Суть | Выбор реализации | Согласованность семейства |
| Расширение | Новый подкласс фабрики | Новая фабрика для семейства |
| Пример в JDK | `Calendar.getInstance()` | `UIManager` (Swing Look&Feel) |

<details>
<summary>Factory Method -- пример</summary>

```java
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

</details>

<details>
<summary>Abstract Factory -- пример</summary>

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

</details>

### Важное

- В Spring: `BeanFactory` -- реализация Factory; `FactoryBean<T>` -- кастомная фабрика бинов
- `switch` с sealed interface -- современная реализация Factory в Java 21+

### Частые ошибки

- Factory для одной реализации -- если нет вариативности, фабрика не нужна
- God Factory -- фабрика, знающая о 50 типах; лучше разбить на несколько

### Как используется в 2026

- Spring создаёт бины через фабрику (ApplicationContext)
- `@ConditionalOnProperty` -- условное создание бинов, фабрика на уровне конфигурации

> **На собеседовании:** ключевое -- чётко разграничить Factory Method (один продукт, выбор реализации) и Abstract Factory (семейство продуктов, согласованность). Частая ошибка -- смешивать их или не привести пример из Spring.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Strategy?
<!-- grade: middle -->

Strategy -- паттерн, определяющий семейство алгоритмов, инкапсулирующий каждый и делающий их взаимозаменяемыми. Клиент выбирает алгоритм в runtime.

> Аналогия из жизни: Strategy -- как навигатор с выбором маршрута. Вы можете ехать "быстрее", "короче" или "без платных дорог" -- алгоритм маршрутизации меняется, а интерфейс навигатора остаётся прежним.

<details>
<summary>Strategy через интерфейс</summary>

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

</details>

С лямбдами (Java 8+) Strategy можно реализовать без отдельных классов:

```java
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

- Strategy + if/else для выбора -- выбор стратегии через DI/Map, а не через if/else
- Strategy для двух строк кода -- лямбда проще отдельного класса

### Как используется в 2026

- `Comparator`, `Predicate`, `Function` -- встроенные Strategy в JDK
- Spring: `List<PaymentStrategy>` -- Spring автоматически собирает все реализации

> **На собеседовании:** покажите связь Strategy с OCP и объясните, когда достаточно лямбды вместо полноценного класса. Частая ошибка -- реализовать Strategy, а выбор стратегии сделать через if/else, что сводит весь паттерн на нет.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Observer?
<!-- grade: middle -->

Observer -- паттерн, определяющий зависимость "один ко многим" между объектами: при изменении состояния одного все зависимые уведомляются автоматически.

> Аналогия из жизни: Observer -- как подписка на Telegram-канал. Автор публикует пост, и все подписчики получают уведомление. Подписчики не опрашивают канал -- уведомление приходит само.

<details>
<summary>Пример реализации</summary>

```java
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

</details>

### Важное

- Observer -- основа event-driven архитектуры
- Spring Events (`ApplicationEventPublisher`) -- Observer из коробки
- `CopyOnWriteArrayList` -- потокобезопасный список подписчиков

### Частые ошибки

- Утечки памяти -- подписчик не отписывается; в Java нет weak listeners по умолчанию
- Синхронный Observer для тяжёлых операций -- блокирует publisher; используйте `@Async` или очереди

### Как используется в 2026

- Spring `@EventListener` / `@TransactionalEventListener` -- стандартная реализация
- Kafka, RabbitMQ -- Observer на уровне распределённой системы

> **На собеседовании:** интервьюер ждёт связь Observer с event-driven архитектурой и Spring Events. Частая ошибка -- не упомянуть проблему утечек памяти при забытой отписке подписчиков.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Decorator?
<!-- grade: middle -->

Decorator -- паттерн, динамически добавляющий объекту новую функциональность, оборачивая его. Альтернатива наследованию через композицию.

> Аналогия из жизни: Decorator -- как топпинги к кофе. Базовый эспрессо можно "обернуть" молоком (латте), сиропом, взбитыми сливками -- каждый слой добавляет что-то новое, не меняя сам кофе.

```java
// Классический пример -- java.io
InputStream raw = new FileInputStream("data.txt");
InputStream buffered = new BufferedInputStream(raw);           // декоратор: буферизация
InputStream gzip = new GZIPInputStream(buffered);              // декоратор: распаковка
Reader reader = new InputStreamReader(gzip, StandardCharsets.UTF_8); // декоратор: символьный поток
```

<details>
<summary>Собственный Decorator</summary>

```java
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

</details>

### Decorator vs Proxy vs Adapter

| Критерий | Decorator | Proxy | Adapter |
|----------|-----------|-------|---------|
| Цель | Добавить функциональность | Контролировать доступ | Преобразовать интерфейс |
| Интерфейс | Тот же, что у обёртки | Тот же, что у обёртки | Другой (целевой) |
| Пример в JDK | `BufferedInputStream` | `Proxy.newProxyInstance()` | `Arrays.asList()` |

### Частые ошибки

- Слишком глубокая вложенность -- 5+ декораторов усложняют отладку (длинный стек-трейс)
- Путать Decorator и Proxy -- Decorator добавляет функциональность, Proxy контролирует доступ

### Как используется в 2026

- `java.io` (streams), `Collections.unmodifiable*()`, `Collections.synchronized*()`
- Spring: `HandlerInterceptor`, `Filter` -- по сути декораторы HTTP-обработки

> **На собеседовании:** назовите java.io как каноничный пример Decorator в JDK -- это покажет понимание стандартной библиотеки. Частая ошибка -- не уметь объяснить разницу между Decorator и Proxy.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Adapter?
<!-- grade: middle -->

Adapter -- паттерн, преобразующий интерфейс класса в другой интерфейс, ожидаемый клиентом. Позволяет работать вместе классам с несовместимыми интерфейсами.

> Аналогия из жизни: Adapter -- как переходник для розетки. Европейская вилка не подходит к американской розетке, но адаптер решает проблему, не меняя ни вилку, ни розетку.

<details>
<summary>Пример кода</summary>

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

</details>

### Примеры Adapter в JDK

- `Arrays.asList(array)` -- адаптирует массив к интерфейсу `List`
- `InputStreamReader` -- адаптирует `InputStream` (байты) к `Reader` (символы)
- `Collections.enumeration(collection)` -- адаптирует `Collection` к `Enumeration`

### Adapter vs Facade

| Критерий | Adapter | Facade |
|----------|---------|--------|
| Цель | Адаптировать один интерфейс к другому | Упростить сложную подсистему |
| Количество классов | Работает с одним классом | Объединяет несколько классов |
| Интерфейс | Преобразует существующий | Создаёт новый упрощённый |

### Частые ошибки

- Adapter с бизнес-логикой -- адаптер только преобразует интерфейс, не добавляет логику
- Путать Adapter и Facade -- Adapter адаптирует один интерфейс к другому, Facade упрощает подсистему

### Как используется в 2026

- Spring: `HandlerAdapter` адаптирует разные типы контроллеров к единому `DispatcherServlet`
- Интеграция legacy-систем -- адаптеры для старых API

> **На собеседовании:** приведите пример из JDK (Arrays.asList, InputStreamReader) и объясните два типа адаптеров: Object Adapter (композиция, предпочтительнее) и Class Adapter (наследование). Частая ошибка -- путать Adapter с Facade.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Proxy?
<!-- grade: middle -->

Proxy -- паттерн, предоставляющий объект-заместитель, который контролирует доступ к другому объекту.

> Аналогия из жизни: Proxy -- как секретарь руководителя. Все звонки идут через секретаря, который решает: передать ли звонок, записать сообщение, отказать или добавить информацию к запросу.

<details>
<summary>Статический Proxy -- пример</summary>

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

</details>

<details>
<summary>JDK Dynamic Proxy</summary>

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

</details>

### Виды Proxy

| Вид | Назначение | Пример |
|-----|-----------|--------|
| Virtual Proxy | Ленивая загрузка | Hibernate Lazy Loading |
| Protection Proxy | Контроль доступа | Spring Security `@PreAuthorize` |
| Caching Proxy | Кэширование | Spring `@Cacheable` |
| Logging Proxy | Логирование | Spring AOP |

### Важное

- Spring AOP построен на Proxy: `@Transactional`, `@Cacheable`, `@Async` работают через прокси
- JDK Dynamic Proxy -- для интерфейсов; CGLIB -- для классов (наследование)

### Частые ошибки

- Вызов метода внутри того же класса -- Spring Proxy не перехватывает self-invocation: `this.method()` обходит прокси
- Proxy на final-класс -- CGLIB не может создать прокси для final-класса

### Как используется в 2026

- Spring AOP: `@Transactional`, `@Cacheable`, `@Async`, `@PreAuthorize` -- всё через Proxy
- Hibernate Lazy Loading -- Proxy для отложенной загрузки сущностей

> **На собеседовании:** ключевой вопрос -- self-invocation в Spring. Объясните, почему `this.method()` обходит прокси и как это решить (через `AopContext` или вынос в другой бин). Частая ошибка -- не знать разницу между JDK Dynamic Proxy и CGLIB.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Template Method?
<!-- grade: middle -->

Template Method -- паттерн, определяющий скелет алгоритма в базовом классе, позволяя подклассам переопределять отдельные шаги без изменения общей структуры.

> Аналогия из жизни: Template Method -- как рецепт теста для пиццы. Шаги всегда одинаковы (замесить, раскатать, выложить начинку, запечь), но начинка у каждой пиццы своя.

<details>
<summary>Пример кода</summary>

```java
abstract class ReportGenerator {
    // Template Method -- определяет порядок шагов
    final void generateReport() {
        fetchData();
        processData();
        formatReport();
        deliver();
    }

    abstract void fetchData();
    abstract void processData();

    void formatReport() { /* формат по умолчанию -- PDF */ }
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

</details>

### Примеры Template Method в JDK и Spring

- `AbstractList.get()` -- подклассы определяют доступ к элементам
- `HttpServlet.doGet()` / `doPost()` -- скелет обработки запроса
- Spring `JdbcTemplate.query()` -- скелет работы с JDBC (открытие/закрытие соединений)

### Template Method vs Strategy

| Критерий | Template Method | Strategy |
|----------|----------------|----------|
| Механизм | Наследование | Композиция |
| Что меняется | Отдельные шаги алгоритма | Весь алгоритм целиком |
| Связность | Сильная (подкласс привязан к базовому) | Слабая (стратегия независима) |
| Тренд 2026 | Используется в framework-коде | Предпочтительнее для бизнес-логики |

### Частые ошибки

- Слишком много абстрактных шагов -- класс становится невозможно реализовать
- Глубокая иерархия -- Template Method провоцирует наследование; предпочитайте Strategy (композиция)

### Как используется в 2026

- Spring `*Template` классы -- JdbcTemplate, RestTemplate, KafkaTemplate
- "Голливудский принцип": не звоните нам, мы сами вам позвоним (framework вызывает ваш код)
- Тренд: Strategy (композиция) предпочтительнее Template Method (наследование) для новых проектов

> **На собеседовании:** назовите JdbcTemplate как пример и объясните "Голливудский принцип". Частая ошибка -- не упомянуть, что template method должен быть final, чтобы подклассы не могли изменить порядок шагов.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое паттерн Chain of Responsibility?
<!-- grade: middle -->

Chain of Responsibility -- паттерн, передающий запрос по цепочке обработчиков. Каждый обработчик решает, обработать запрос самостоятельно или передать следующему.

> Аналогия из жизни: Chain of Responsibility -- как обработка обращения в техподдержку. Сначала отвечает бот, если не справился -- оператор первой линии, затем специалист, затем инженер. Каждый уровень либо решает проблему, либо передаёт дальше.

<details>
<summary>Пример кода</summary>

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

</details>

### Примеры в Java-экосистеме

- Servlet Filter -- `FilterChain.doFilter()` -- каждый фильтр вызывает следующий
- Spring Security FilterChain -- цепочка security-фильтров
- Spring Interceptors -- `HandlerInterceptor.preHandle()` / `postHandle()`
- `java.util.logging Handler` -- обработчики логов

### Важное

- Каждый обработчик отвечает за одну задачу (SRP)
- Порядок обработчиков важен (auth -> rate limit -> validation -> business logic)
- Обработчик может прервать цепочку или передать управление дальше

### Частые ошибки

- Запрос не обрабатывается никем -- нужен default handler в конце цепочки
- Слишком длинная цепочка -- усложняет отладку; логируйте прохождение через каждый handler

### Как используется в 2026

- Servlet Filters, Spring Security -- фундаментально построены на Chain of Responsibility
- Middleware-паттерн в HTTP-фреймворках (аналогично)

> **На собеседовании:** назовите Servlet Filter и Spring Security как примеры Chain of Responsibility -- это покажет понимание реальных фреймворков. Частая ошибка -- описать паттерн теоретически, но не связать с FilterChain из Spring.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое антипаттерн? Какие антипаттерны вы знаете?
<!-- grade: middle -->

Антипаттерн -- распространённый подход к решению проблемы, который является неэффективным или контрпродуктивным. В отличие от паттерна, антипаттерн описывает, как делать не надо.

| Антипаттерн | Описание |
|-------------|----------|
| God Object | Класс, знающий и делающий слишком много (нарушение SRP) |
| Spaghetti Code | Запутанный код без структуры, сложно читать и поддерживать |
| Golden Hammer | Использование одного инструмента для всех задач ("у меня есть молоток...") |
| Copy-Paste Programming | Дублирование кода вместо выделения общей логики |
| Premature Optimization | Оптимизация до измерения производительности |
| Magic Numbers/Strings | Литералы без имён: `if (status == 3)` вместо `if (status == Status.APPROVED)` |
| Cargo Cult Programming | Использование паттернов/технологий без понимания зачем |
| Lava Flow | Мёртвый код, который боятся удалить ("вдруг понадобится") |
| Big Ball of Mud | Система без различимой архитектуры |
| Poltergeist | Классы с единственной ролью -- передача вызова другому классу |

### Важное

- Антипаттерны часто появляются из-за спешки, недостатка опыта или давления требований
- God Object -- самый частый; бороться через декомпозицию (SRP)
- "Лучший код -- удалённый код" -- не бойтесь удалять Lava Flow

### Частые ошибки

- Рефакторинг ради рефакторинга -- антипаттерн стоит исправлять, если он мешает, а не потому что "некрасиво"
- Замена одного антипаттерна другим -- God Object -> 50 микроклассов (Ravioli Code)

### Как используется в 2026

- Code review -- основной инструмент выявления антипаттернов
- Static analysis (SonarQube, SpotBugs) -- автоматическое обнаружение God Object, дублирования
- ArchUnit -- архитектурные тесты для предотвращения нарушений

> **На собеседовании:** назовите 3-4 антипаттерна с примерами из своего опыта. Интервьюер хочет видеть, что вы умеете распознавать проблемы в коде. Частая ошибка -- перечислить антипаттерны списком, но не объяснить, как вы с ними боролись.

[к оглавлению](#шаблоны-проектирования)

---

## Что такое Dependency Injection?
<!-- grade: junior -->

Dependency Injection (DI) -- паттерн, при котором объект получает свои зависимости извне, а не создаёт их сам. Реализует принцип Dependency Inversion (D из SOLID).

> Аналогия из жизни: DI -- как заказ еды в ресторане. Вы не идёте на кухню сами (не создаёте зависимости), а официант (контейнер) приносит вам блюдо (зависимость) за столик.

```java
// БЕЗ DI -- жёсткая связность
class OrderService {
    private final OrderRepository repo = new PostgresOrderRepository(); // жёсткая связь
    private final EmailService email = new SmtpEmailService();          // жёсткая связь
}

// С DI -- слабая связность
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

### Три способа внедрения в Spring

| Способ | Рекомендуется | Поля final | Тестируемость | Пример |
|--------|:------------:|:----------:|:-------------:|--------|
| Конструктор | Да | Да | Легко (`new Service(mock)`) | `OrderService(OrderRepository repo)` |
| Сеттер | Иногда | Нет | Средне | `@Autowired void setRepo(...)` |
| Поле | Нет | Нет | Только через reflection | `@Autowired private OrderRepository repo` |

<details>
<summary>Примеры трёх способов</summary>

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

// 3. Через поле (НЕ РЕКОМЕНДУЕТСЯ -- затрудняет тестирование)
@Service
class OrderService {
    @Autowired
    private OrderRepository repo;
}
```

</details>

### Важное

- DI = реализация Dependency Inversion Principle (D из SOLID)
- Конструктор -- лучший способ: поля final, объект полностью инициализирован, легко тестировать
- IoC-контейнер (Spring) -- автоматизирует DI, управляя созданием и связыванием объектов

### Частые ошибки

- Field injection -- невозможно создать объект без рефлексии; `new OrderService()` не скомпилируется
- Циклические зависимости -- A зависит от B, B от A; признак плохого дизайна
- `new` внутри сервиса для зависимостей -- нарушает DI; используйте фабрику или Provider

### Как используется в 2026

- Spring IoC -- стандартный DI-контейнер в Java-экосистеме
- Constructor injection -- единственный рекомендуемый способ

> **На собеседовании:** объясните разницу между DI и IoC (DI -- конкретный паттерн, IoC -- общий принцип, DI -- один из способов реализации IoC). Частая ошибка -- использовать field injection в примерах и не знать, почему constructor injection лучше.

[к оглавлению](#шаблоны-проектирования)

[Вопросы для собеседования](README.md)
