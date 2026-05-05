[Вопросы для собеседования](README.md)

# Тестирование
+ [Что такое модульное тестирование?](#Что-такое-модульное-тестирование)
+ [Что такое интеграционное тестирование?](#Что-такое-интеграционное-тестирование)
+ [Что такое пирамида тестирования?](#Что-такое-пирамида-тестирования)
+ [Какие существуют виды тестовых объектов?](#Какие-существуют-виды-тестовых-объектов)
+ [Чем stub отличается от mock?](#Чем-stub-отличается-от-mock)
+ [Какие основные аннотации JUnit 5 вы знаете?](#Какие-основные-аннотации-JUnit-5-вы-знаете)
+ [Что такое Mockito и как его использовать?](#Что-такое-Mockito-и-как-его-использовать)
+ [Что такое AssertJ?](#Что-такое-AssertJ)
+ [Что такое TDD и BDD?](#Что-такое-TDD-и-BDD)
+ [Что такое Testcontainers и зачем они нужны?](#Что-такое-Testcontainers-и-зачем-они-нужны)
+ [Как тестировать исключения в JUnit 5?](#Как-тестировать-исключения-в-JUnit-5)
+ [Что такое Code Coverage?](#Что-такое-Code-Coverage)

## Что такое модульное тестирование?

**Модульное (unit) тестирование** — проверка корректности отдельных модулей (классов, методов) в изоляции от зависимостей. Зависимости заменяются моками или стабами.

```java
class CalculatorTest {

    @Test
    @DisplayName("Сложение двух положительных чисел")
    void shouldAddTwoNumbers() {
        Calculator calc = new Calculator();
        assertEquals(5, calc.add(2, 3));
    }

    @Test
    @DisplayName("Деление на ноль бросает исключение")
    void shouldThrowOnDivisionByZero() {
        Calculator calc = new Calculator();
        assertThrows(ArithmeticException.class, () -> calc.divide(10, 0));
    }
}
```

**Характеристики хороших unit-тестов (F.I.R.S.T.):**
- **Fast** — выполняются за миллисекунды
- **Isolated** — не зависят от других тестов, БД, сети
- **Repeatable** — одинаковый результат при каждом запуске
- **Self-validating** — pass/fail без ручной проверки
- **Timely** — написаны вовремя (в идеале — до или одновременно с кодом)

### Важное
- Unit-тесты проверяют **логику**, не инфраструктуру
- Зависимости заменяются через Mockito/stub — тест должен быть изолирован
- Один тест = один сценарий; имя теста описывает ожидание

### Частые ошибки
- **Тестирование приватных методов** — тестируйте публичный API, не реализацию
- **Тесты, зависящие от порядка выполнения** — каждый тест должен быть независим
- **Один assert на весь класс** — один тест проверяет одно поведение

### Как используется в 2026
- JUnit 5 + Mockito + AssertJ — стандартный стек для unit-тестов
- Минимальное время выполнения — сотни unit-тестов за секунды

[к оглавлению](#Тестирование)

## Что такое интеграционное тестирование?

**Интеграционное тестирование** — проверка взаимодействия нескольких компонентов системы: приложение + БД, приложение + внешний API, несколько сервисов вместе.

```java
@SpringBootTest
@Testcontainers
class OrderRepositoryIT {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    private OrderRepository orderRepository;

    @Test
    void shouldSaveAndFindOrder() {
        Order order = new Order("product-1", BigDecimal.valueOf(99.99));
        orderRepository.save(order);

        Optional<Order> found = orderRepository.findById(order.getId());
        assertThat(found).isPresent();
        assertThat(found.get().getProductId()).isEqualTo("product-1");
    }
}
```

**Отличие от unit-тестов:**

| Критерий | Unit-тест | Интеграционный тест |
|----------|----------|-------------------|
| Зависимости | Замокированы | Реальные (БД, API) |
| Скорость | Миллисекунды | Секунды |
| Что проверяет | Логику метода | Взаимодействие компонентов |
| Инфраструктура | Не нужна | Нужна (БД, контейнеры) |

### Важное
- Интеграционные тесты дороже unit-тестов — запускайте их реже (CI pipeline)
- Testcontainers — стандарт для интеграционных тестов с БД, Kafka, Redis
- `@SpringBootTest` поднимает полный Spring Context — медленно; используйте слайсы (`@DataJpaTest`, `@WebMvcTest`)

### Частые ошибки
- **H2 вместо реальной БД** — синтаксис и поведение отличаются; тесты проходят, production падает
- **Общая БД между тестами** — данные одного теста влияют на другой; используйте `@Transactional` или очистку
- **Слишком много интеграционных тестов** — медленный CI; придерживайтесь пирамиды тестирования

### Как используется в 2026
- Testcontainers + реальная БД — стандарт, H2 для интеграционных тестов считается anti-pattern
- Spring Boot слайсы (`@DataJpaTest`, `@WebMvcTest`) — быстрее полного `@SpringBootTest`

[к оглавлению](#Тестирование)

## Что такое пирамида тестирования?

**Пирамида тестирования** — модель, описывающая оптимальное соотношение типов тестов:

```
         /\
        /  \         E2E / UI тесты (мало, медленные, хрупкие)
       /    \
      /------\       Интеграционные тесты (среднее количество)
     /        \
    /----------\     Unit-тесты (много, быстрые, стабильные)
   /____________\
```

| Уровень | Количество | Скорость | Стоимость поддержки | Что проверяет |
|---------|-----------|----------|---------------------|---------------|
| **Unit** | Много (70-80%) | Миллисекунды | Низкая | Логика методов |
| **Integration** | Среднее (15-20%) | Секунды | Средняя | Связки компонентов |
| **E2E** | Мало (5-10%) | Минуты | Высокая | Весь путь пользователя |

**Анти-паттерн — «рожок мороженого»:** много E2E, мало unit-тестов → медленный CI, хрупкие тесты, долгая обратная связь.

### Важное
- Большинство багов ловится unit-тестами — они дешёвые и быстрые
- Интеграционные — для проверки «склейки» компонентов (SQL, HTTP, messaging)
- E2E — только critical path (логин, оплата, основной flow)

### Частые ошибки
- **Только E2E тесты** — медленный CI, хрупкие тесты, сложная отладка
- **0% интеграционных** — unit-тесты проходят, но приложение не работает с реальной БД
- **Гонка за 100% coverage unit-тестами** — тестируются геттеры/сеттеры, реальная логика не покрыта

### Как используется в 2026
- Пирамида — ориентир, не догма; в микросервисах доля интеграционных тестов выше
- Contract testing (Spring Cloud Contract, Pact) — дополнение к пирамиде для межсервисных контрактов

[к оглавлению](#Тестирование)

## Какие существуют виды тестовых объектов?

**Dummy** — объект-заглушка, передаётся как параметр, но не используется:

```java
// null или пустой объект — нужен только для компиляции
var service = new OrderService(new DummyLogger());
```

**Fake** — упрощённая, но рабочая реализация (например, in-memory БД):

```java
class FakeUserRepository implements UserRepository {
    private final Map<Long, User> store = new HashMap<>();

    @Override
    public User save(User user) {
        store.put(user.getId(), user);
        return user;
    }

    @Override
    public Optional<User> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }
}
```

**Stub** — возвращает заранее заданные ответы:

```java
when(userRepository.findById(1L)).thenReturn(Optional.of(testUser));
```

**Spy** — оборачивает реальный объект, записывая вызовы:

```java
List<String> spyList = spy(new ArrayList<>());
spyList.add("item");
verify(spyList).add("item"); // проверяем, что add был вызван
assertEquals(1, spyList.size()); // реальная логика работает
```

**Mock** — объект с запрограммированным поведением и проверкой вызовов:

```java
UserRepository mock = mock(UserRepository.class);
when(mock.findById(1L)).thenReturn(Optional.of(testUser));
// ... использование
verify(mock, times(1)).findById(1L); // проверка вызова
```

### Важное
- **Stub** — подменяет данные (state verification); **Mock** — проверяет взаимодействие (behavior verification)
- В Mockito: `mock()` создаёт mock, `spy()` оборачивает реальный объект
- Предпочитайте state verification (stub + assert result) над behavior verification (mock + verify)

### Частые ошибки
- **Mock всего** — если можно протестировать с реальным объектом, mock не нужен
- **Verify без необходимости** — `verify` нужен только когда возвращаемое значение не говорит о корректности
- **Spy с final-классами** — Mockito не может spy final-классы без mockito-inline

### Как используется в 2026
- Mockito 5.x — поддержка final-классов по умолчанию (mockmaker-inline)
- Тренд: меньше моков, больше интеграционных тестов с Testcontainers

[к оглавлению](#Тестирование)

## Чем stub отличается от mock?

| Критерий | Stub | Mock |
|----------|------|------|
| Цель | Подменить данные | Проверить взаимодействие |
| Проверка | Состояние результата (assertEquals) | Факт/количество вызовов (verify) |
| Пример | `when(...).thenReturn(...)` + `assertEquals` | `verify(mock).method()` |

```java
// STUB — проверяем результат
@Test
void stubExample() {
    when(priceService.getPrice("AAPL")).thenReturn(150.0); // stub
    double total = portfolio.calculateTotal(); // используем stub
    assertEquals(1500.0, total); // проверяем СОСТОЯНИЕ
}

// MOCK — проверяем взаимодействие
@Test
void mockExample() {
    orderService.placeOrder(order);
    verify(emailService).sendConfirmation(order.getEmail()); // проверяем ВЫЗОВ
}
```

### Важное
- Martin Fowler: «Stubs provide canned answers, mocks verify expectations»
- Предпочитайте stub (state testing) — тесты менее хрупкие при рефакторинге
- Mock оправдан для side effects (отправка email, запись в лог, вызов внешнего API)

### Частые ошибки
- **Verify + when на одном объекте** — stub достаточно, verify избыточен
- **Mock на каждый метод** — тест тестирует реализацию, а не поведение; рефакторинг ломает тест

### Как используется в 2026
- Mockito BDD: `given(...).willReturn(...)` + `then(mock).should().method()` — читаемый стиль

[к оглавлению](#Тестирование)

## Какие основные аннотации JUnit 5 вы знаете?

**Жизненный цикл теста:**

```java
class LifecycleTest {

    @BeforeAll  // один раз перед всеми тестами (static)
    static void initAll() { }

    @BeforeEach // перед каждым тестом
    void init() { }

    @Test
    void testMethod() { }

    @AfterEach  // после каждого теста
    void tearDown() { }

    @AfterAll   // один раз после всех тестов (static)
    static void tearDownAll() { }
}
```

**Основные аннотации:**

```java
@Test                          // маркирует тестовый метод
@DisplayName("Описание теста") // человекочитаемое имя
@Disabled("Причина")          // пропустить тест (аналог @Ignore в JUnit 4)
@Tag("integration")           // тегирование для фильтрации
@Timeout(5)                   // таймаут в секундах

// Вложенные тесты — группировка
@Nested
@DisplayName("Когда пользователь авторизован")
class WhenAuthenticated {
    @Test void shouldAccessProfile() { }
    @Test void shouldAccessSettings() { }
}
```

**Параметризованные тесты:**

```java
@ParameterizedTest
@ValueSource(strings = {"racecar", "radar", "level"})
void shouldDetectPalindrome(String word) {
    assertTrue(StringUtils.isPalindrome(word));
}

@ParameterizedTest
@CsvSource({"1, 1, 2", "2, 3, 5", "10, 20, 30"})
void shouldAdd(int a, int b, int expected) {
    assertEquals(expected, calculator.add(a, b));
}

@ParameterizedTest
@MethodSource("orderProvider")
void shouldCalculateTotal(Order order, BigDecimal expected) {
    assertEquals(expected, order.getTotal());
}

static Stream<Arguments> orderProvider() {
    return Stream.of(
        Arguments.of(new Order(2, 10.0), BigDecimal.valueOf(20.0)),
        Arguments.of(new Order(0, 10.0), BigDecimal.ZERO)
    );
}
```

**Assertions (проверки):**

```java
assertEquals(expected, actual);
assertNotNull(object);
assertTrue(condition);
assertThrows(Exception.class, () -> riskyMethod());
assertDoesNotThrow(() -> safeMethod());
assertTimeout(Duration.ofSeconds(2), () -> longMethod());

// Группировка — все assertions выполняются, даже если первый упал
assertAll(
    () -> assertEquals("John", user.getName()),
    () -> assertEquals("john@mail.com", user.getEmail()),
    () -> assertNotNull(user.getId())
);
```

### Важное
- JUnit 5 = JUnit Platform + JUnit Jupiter (новый API) + JUnit Vintage (совместимость с JUnit 4)
- `@BeforeAll`/`@AfterAll` — `static` по умолчанию; для нестатических: `@TestInstance(Lifecycle.PER_CLASS)`
- `@ParameterizedTest` — один тест с множеством входных данных; заменяет дублирование
- `assertAll` — проверяет все условия, показывая все ошибки сразу

### Частые ошибки
- **JUnit 4 аннотации в JUnit 5** — `@Before` → `@BeforeEach`, `@BeforeClass` → `@BeforeAll`, `@Ignore` → `@Disabled`
- **`@ParameterizedTest` без источника данных** — нужен `@ValueSource`, `@CsvSource` или `@MethodSource`
- **Забыть `@Test`** — метод без аннотации не запустится как тест

### Как используется в 2026
- JUnit 5 — единственная актуальная версия; JUnit 4 — legacy
- IntelliJ IDEA полностью поддерживает `@DisplayName`, `@Nested`, `@ParameterizedTest`
- `@Tag` + Maven/Gradle profiles — раздельный запуск unit и integration тестов

[к оглавлению](#Тестирование)

## Что такое Mockito и как его использовать?

**Mockito** — фреймворк для создания mock-объектов в unit-тестах. Позволяет подменить зависимости и проверить взаимодействие.

```java
@ExtendWith(MockitoExtension.class) // подключение Mockito к JUnit 5
class OrderServiceTest {

    @Mock // создаёт mock
    private OrderRepository orderRepository;

    @Mock
    private PaymentService paymentService;

    @InjectMocks // создаёт объект и внедряет моки
    private OrderService orderService;

    @Test
    void shouldCreateOrder() {
        // given (подготовка)
        Order order = new Order("product-1", BigDecimal.TEN);
        when(orderRepository.save(any(Order.class))).thenReturn(order);
        when(paymentService.charge(any())).thenReturn(true);

        // when (действие)
        Order result = orderService.createOrder("product-1", BigDecimal.TEN);

        // then (проверка)
        assertThat(result.getProductId()).isEqualTo("product-1");
        verify(orderRepository).save(any(Order.class));
        verify(paymentService).charge(any());
    }

    @Test
    void shouldThrowWhenPaymentFails() {
        when(paymentService.charge(any())).thenReturn(false);

        assertThrows(PaymentException.class,
            () -> orderService.createOrder("product-1", BigDecimal.TEN));

        verify(orderRepository, never()).save(any()); // save НЕ вызван
    }
}
```

**ArgumentCaptor — захват аргументов:**

```java
@Test
void shouldSendCorrectEmail() {
    orderService.createOrder("product-1", BigDecimal.TEN);

    ArgumentCaptor<Email> captor = ArgumentCaptor.forClass(Email.class);
    verify(emailService).send(captor.capture());

    Email sentEmail = captor.getValue();
    assertThat(sentEmail.getSubject()).contains("Заказ создан");
    assertThat(sentEmail.getTo()).isEqualTo("customer@mail.com");
}
```

**Основные методы:**

| Метод | Назначение |
|-------|-----------|
| `when(...).thenReturn(...)` | Задать возвращаемое значение |
| `when(...).thenThrow(...)` | Бросить исключение |
| `doReturn(...).when(spy)` | Для spy-объектов |
| `verify(mock).method()` | Проверить, что метод вызван |
| `verify(mock, times(2))` | Проверить количество вызовов |
| `verify(mock, never())` | Проверить, что метод НЕ вызван |
| `any()`, `eq()`, `argThat()` | Матчеры аргументов |

### Важное
- `@ExtendWith(MockitoExtension.class)` — обязательно для JUnit 5
- `@InjectMocks` — создаёт объект и внедряет `@Mock`-поля через конструктор/сеттеры
- BDD-стиль: `given(...).willReturn(...)` + `then(mock).should().method()`
- Mockito 5.x — final-классы мокируются по умолчанию

### Частые ошибки
- **Mock вместо реального объекта** — если класс простой и без зависимостей, mock не нужен
- **`when()` на spy** — используйте `doReturn().when(spy)`, иначе реальный метод выполнится
- **Mixing matchers**: `verify(mock).method(eq("a"), any())` — если один аргумент matcher, все должны быть matchers

### Как используется в 2026
- Mockito 5.x — стандарт; mockmaker-inline по умолчанию (final, static mocking)
- `@MockitoBean` (Spring Boot 3.4+) — замена `@MockBean` с лучшей производительностью

[к оглавлению](#Тестирование)

## Что такое AssertJ?

**AssertJ** — библиотека fluent assertions для Java, предоставляющая читаемый chainable API вместо стандартных `assertEquals`/`assertTrue`.

```java
// JUnit 5 стандартные assertions
assertEquals("John", user.getName());
assertTrue(list.contains("item"));
assertNotNull(result);

// AssertJ — fluent и более выразительный
assertThat(user.getName()).isEqualTo("John");
assertThat(list).contains("item").hasSize(3).doesNotContain("bad");
assertThat(result).isNotNull().extracting(User::getEmail).isEqualTo("john@mail.com");
```

**Проверка коллекций:**

```java
assertThat(users)
    .hasSize(3)
    .extracting(User::getName)
    .containsExactly("Alice", "Bob", "Charlie");

assertThat(users)
    .filteredOn(User::isActive)
    .extracting(User::getEmail)
    .allMatch(email -> email.contains("@"));
```

**Проверка исключений:**

```java
assertThatThrownBy(() -> service.findById(-1L))
    .isInstanceOf(NotFoundException.class)
    .hasMessageContaining("not found")
    .hasFieldOrPropertyWithValue("id", -1L);

assertThatCode(() -> service.findById(1L))
    .doesNotThrowAnyException();
```

### Важное
- `assertThat(actual)` — точка входа; IDE автоматически подсказывает доступные проверки
- `extracting()` — извлечение полей из объектов/коллекций для цепочки проверок
- AssertJ поддерживает soft assertions (`SoftAssertions`) — все проверки выполняются, ошибки собираются

### Частые ошибки
- **Путать порядок аргументов** — `assertThat(actual)`, не `assertThat(expected)`
- **Импортировать JUnit assertEquals вместе с AssertJ** — конфликт; выберите один стиль

### Как используется в 2026
- AssertJ — стандарт де-факто; Spring Boot включает в `spring-boot-starter-test`
- Лучшие сообщения об ошибках: `expected: "John" but was: "Jane"` вместо JUnit `expected <John> but was <Jane>`

[к оглавлению](#Тестирование)

## Что такое TDD и BDD?

**TDD (Test-Driven Development)** — методология, где тесты пишутся **до** кода. Цикл:

```
1. RED    — написать failing test
2. GREEN  — написать минимальный код, чтобы тест прошёл
3. REFACTOR — улучшить код, не ломая тесты
```

```java
// Шаг 1: RED — тест для несуществующего метода
@Test
void shouldCalculateDiscount() {
    PricingService service = new PricingService();
    assertEquals(90.0, service.applyDiscount(100.0, 10)); // метода ещё нет
}

// Шаг 2: GREEN — минимальная реализация
public double applyDiscount(double price, int percent) {
    return price * (100 - percent) / 100;
}

// Шаг 3: REFACTOR — улучшаем, тест остаётся зелёным
```

**BDD (Behavior-Driven Development)** — расширение TDD с фокусом на поведение, описанное бизнес-языком. Формат **Given-When-Then**:

```java
@Test
@DisplayName("Когда применяется скидка 10%, цена уменьшается на 10%")
void discountScenario() {
    // Given — начальное состояние
    var pricing = new PricingService();
    double originalPrice = 100.0;

    // When — действие
    double discountedPrice = pricing.applyDiscount(originalPrice, 10);

    // Then — ожидаемый результат
    assertThat(discountedPrice).isEqualTo(90.0);
}
```

### Важное
- TDD заставляет думать о дизайне API до реализации — код получается тестируемым
- BDD Given-When-Then — универсальная структура любого теста, даже без формального BDD
- Mockito BDD: `given(mock.method()).willReturn(value)` + `then(mock).should().method()`

### Частые ошибки
- **TDD для всего** — для тривиального CRUD overhead превышает пользу
- **Слишком детальные тесты** — тест привязан к реализации, рефакторинг ломает тесты
- **BDD без участия бизнеса** — Given-When-Then полезен как формат, но полноценный BDD подразумевает Cucumber + бизнес-аналитиков

### Как используется в 2026
- Given-When-Then — стандартная структура тестов даже без формального BDD
- TDD применяется точечно для сложной бизнес-логики, алгоритмов, edge cases
- Cucumber/Gherkin — для acceptance-тестов с участием non-technical stakeholders

[к оглавлению](#Тестирование)

## Что такое Testcontainers и зачем они нужны?

**Testcontainers** — Java-библиотека для запуска Docker-контейнеров в тестах. Позволяет тестировать с реальными БД, брокерами сообщений, кэшами вместо in-memory заглушек.

```java
@SpringBootTest
@Testcontainers
class UserServiceIT {

    @Container
    static PostgreSQLContainer<?> postgres =
        new PostgreSQLContainer<>("postgres:16-alpine")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    private UserService userService;

    @Test
    void shouldSaveAndRetrieveUser() {
        User user = userService.create("John", "john@mail.com");
        assertThat(userService.findById(user.getId()))
            .isPresent()
            .hasValueSatisfying(u -> assertThat(u.getName()).isEqualTo("John"));
    }
}
```

**Доступные контейнеры:**

```java
PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");
MongoDBContainer mongo = new MongoDBContainer("mongo:7");
KafkaContainer kafka = new KafkaContainer(DockerImageName.parse("confluentinc/cp-kafka:7.6.0"));
GenericContainer<?> redis = new GenericContainer<>("redis:7").withExposedPorts(6379);
LocalStackContainer aws = new LocalStackContainer(DockerImageName.parse("localstack/localstack:3"))
    .withServices(LocalStackContainer.Service.S3, LocalStackContainer.Service.SQS);
```

### Важное
- **Testcontainers > H2** — тестируете на реальной БД; нет расхождений синтаксиса и поведения
- `@Container` + `@Testcontainers` — автоматический lifecycle (start/stop)
- `@DynamicPropertySource` — внедрение динамических свойств (порт, URL) в Spring context
- Контейнеры переиспользуются между тестами через `static` поля

### Частые ошибки
- **Docker не установлен** — Testcontainers требует Docker; в CI нужен Docker-in-Docker или Testcontainers Cloud
- **Не `static` контейнер** — без `static` контейнер перезапускается для каждого теста (медленно)
- **Не указать версию образа** — `new PostgreSQLContainer<>("postgres")` → непредсказуемая версия

### Как используется в 2026
- Testcontainers — стандарт для интеграционных тестов в Java-экосистеме
- Spring Boot 3.1+ — нативная поддержка через `@ServiceConnection` (без `@DynamicPropertySource`)
- Testcontainers Desktop / Cloud — запуск без локального Docker

[к оглавлению](#Тестирование)

## Как тестировать исключения в JUnit 5?

```java
// assertThrows — проверить, что исключение выброшено
@Test
void shouldThrowWhenUserNotFound() {
    Exception ex = assertThrows(NotFoundException.class,
        () -> userService.findById(-1L));

    assertThat(ex.getMessage()).contains("not found");
}

// assertDoesNotThrow — проверить, что исключение НЕ выброшено
@Test
void shouldNotThrowForValidInput() {
    assertDoesNotThrow(() -> userService.findById(1L));
}

// AssertJ — более выразительный синтаксис
@Test
void shouldThrowWithDetails() {
    assertThatThrownBy(() -> userService.findById(-1L))
        .isInstanceOf(NotFoundException.class)
        .hasMessage("User not found: -1")
        .hasNoCause();
}

// Проверка вложенного исключения (cause)
@Test
void shouldWrapDatabaseException() {
    assertThatThrownBy(() -> userService.findById(999L))
        .isInstanceOf(ServiceException.class)
        .hasCauseInstanceOf(DataAccessException.class);
}
```

### Важное
- `assertThrows` возвращает исключение — можно проверить message, cause, поля
- `assertThatThrownBy` (AssertJ) — fluent API с цепочкой проверок
- В JUnit 4 использовались `@Test(expected=...)` и `ExpectedException` rule — устарело

### Частые ошибки
- **`@Test(expected = Exception.class)`** — это JUnit 4; в JUnit 5 используйте `assertThrows`
- **Слишком широкий тип** — `assertThrows(Exception.class, ...)` поймает любое исключение; используйте конкретный тип
- **Не проверять message** — два разных `IllegalArgumentException` могут означать разные ошибки

### Как используется в 2026
- `assertThrows` (JUnit 5) + `assertThatThrownBy` (AssertJ) — два стандартных подхода

[к оглавлению](#Тестирование)

## Что такое Code Coverage?

**Code Coverage (покрытие кода)** — метрика, показывающая, какой процент кода выполняется при запуске тестов.

**Типы покрытия:**

| Тип | Что измеряет | Пример |
|-----|-------------|--------|
| **Line coverage** | % строк, выполненных тестами | 80 из 100 строк = 80% |
| **Branch coverage** | % ветвлений (if/else, switch) | Протестирована ли каждая ветка if? |
| **Method coverage** | % методов, вызванных тестами | Все ли публичные методы вызваны? |
| **Instruction coverage** | % байткод-инструкций | Наиболее точная метрика |

**JaCoCo — стандартный инструмент:**

```xml
<!-- pom.xml -->
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.12</version>
    <executions>
        <execution>
            <goals><goal>prepare-agent</goal></goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals><goal>report</goal></goals>
        </execution>
        <execution>
            <id>check</id>
            <goals><goal>check</goal></goals>
            <configuration>
                <rules>
                    <rule>
                        <limits>
                            <limit>
                                <counter>LINE</counter>
                                <value>COVEREDRATIO</value>
                                <minimum>0.80</minimum> <!-- 80% минимум -->
                            </limit>
                        </limits>
                    </rule>
                </rules>
            </configuration>
        </execution>
    </executions>
</plugin>
```

### Важное
- **80% line coverage** — разумный минимум для большинства проектов
- **100% — не цель**: геттеры, конфигурация, DTO не нуждаются в тестах
- Branch coverage важнее line coverage — показывает, протестированы ли edge cases
- JaCoCo интегрируется с SonarQube для визуализации и трендов

### Частые ошибки
- **Гонка за 100%** — тестирование `toString()` и конструкторов ради метрики бессмысленно
- **Coverage без assertions** — тест вызывает метод, но не проверяет результат; coverage растёт, качество — нет
- **Exclusion вместо исправления** — исключение пакетов из отчёта вместо написания тестов

### Как используется в 2026
- JaCoCo + SonarQube — стандартная связка в CI/CD pipeline
- Quality Gate: «coverage не должен падать» (delta check) важнее абсолютной цифры
- Mutation testing (PIT) — более продвинутая альтернатива: проверяет, ловят ли тесты реальные мутации в коде

[к оглавлению](#Тестирование)

[Вопросы для собеседования](README.md)
