[Вопросы для собеседования](README.md)

# Тестирование

+ [Что такое модульное тестирование?](#что-такое-модульное-тестирование)
+ [Что такое интеграционное тестирование?](#что-такое-интеграционное-тестирование)
+ [Что такое пирамида тестирования?](#что-такое-пирамида-тестирования)
+ [Какие существуют виды тестовых объектов?](#какие-существуют-виды-тестовых-объектов)
+ [Чем stub отличается от mock?](#чем-stub-отличается-от-mock)
+ [Какие основные аннотации JUnit 5 вы знаете?](#какие-основные-аннотации-junit-5-вы-знаете)
+ [Что такое Mockito и как его использовать?](#что-такое-mockito-и-как-его-использовать)
+ [Что такое AssertJ?](#что-такое-assertj)
+ [Что такое TDD и BDD?](#что-такое-tdd-и-bdd)
+ [Что такое Testcontainers и зачем они нужны?](#что-такое-testcontainers-и-зачем-они-нужны)
+ [Как тестировать исключения в JUnit 5?](#как-тестировать-исключения-в-junit-5)
+ [Что такое Code Coverage?](#что-такое-code-coverage)

## Что такое модульное тестирование?
<!-- grade: junior -->

Модульное (unit) тестирование -- проверка корректности отдельных модулей (классов, методов) в изоляции от внешних зависимостей, которые заменяются моками или стабами.

> **Аналогия из жизни:** проверка отдельной детали на заводе перед сборкой. Каждый болт тестируется отдельно от всего механизма -- если он бракованный, это выясняется до того, как машина будет собрана.

### Характеристики хороших unit-тестов (F.I.R.S.T.)

| Принцип | Значение |
|---------|----------|
| Fast | Выполняются за миллисекунды |
| Isolated | Не зависят от других тестов, БД, сети |
| Repeatable | Одинаковый результат при каждом запуске |
| Self-validating | Pass/fail без ручной проверки |
| Timely | Написаны вовремя (в идеале -- до или одновременно с кодом) |

### Пример

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

### Ключевые принципы

- Unit-тесты проверяют логику, а не инфраструктуру
- Зависимости заменяются через Mockito/stub -- тест должен быть изолирован
- Один тест = один сценарий; имя теста описывает ожидание
- Тестируйте публичный API, не приватные методы

### Частые ошибки

- Тестирование приватных методов -- тестируйте публичный API, не реализацию
- Тесты, зависящие от порядка выполнения -- каждый тест должен быть независим
- Один assert на весь класс -- один тест проверяет одно поведение

### Как используется в 2026

- JUnit 5 + Mockito + AssertJ -- стандартный стек для unit-тестов
- Минимальное время выполнения -- сотни unit-тестов за секунды

> **На собеседовании:** интервьюер ожидает не просто определение, а понимание принципа F.I.R.S.T. и умение объяснить, почему unit-тест должен быть изолирован. Частая ошибка -- путать unit-тесты с интеграционными или говорить, что unit-тест может обращаться к БД.

[к оглавлению](#Тестирование)

---

## Что такое интеграционное тестирование?
<!-- grade: middle -->

Интеграционное тестирование -- проверка взаимодействия нескольких компонентов системы: приложение + БД, приложение + внешний API, несколько сервисов вместе.

> **Аналогия из жизни:** после того как каждая деталь проверена отдельно (unit), их собирают вместе и проверяют, что механизм работает в сборке -- передачи переключаются, двигатель крутит колёса.

### Отличие от unit-тестов

| Критерий | Unit-тест | Интеграционный тест |
|----------|----------|-------------------|
| Зависимости | Замокированы | Реальные (БД, API) |
| Скорость | Миллисекунды | Секунды |
| Что проверяет | Логику метода | Взаимодействие компонентов |
| Инфраструктура | Не нужна | Нужна (БД, контейнеры) |

### Пример с Testcontainers

<details>
<summary>Пример кода</summary>

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

</details>

### Ключевые принципы

- Интеграционные тесты дороже unit-тестов -- запускайте их реже (CI pipeline)
- Testcontainers -- стандарт для интеграционных тестов с БД, Kafka, Redis
- `@SpringBootTest` поднимает полный Spring Context -- медленно; используйте слайсы (`@DataJpaTest`, `@WebMvcTest`)

### Частые ошибки

- H2 вместо реальной БД -- синтаксис и поведение отличаются; тесты проходят, production падает
- Общая БД между тестами -- данные одного теста влияют на другой; используйте `@Transactional` или очистку
- Слишком много интеграционных тестов -- медленный CI; придерживайтесь пирамиды тестирования

### Как используется в 2026

- Testcontainers + реальная БД -- стандарт, H2 для интеграционных тестов считается anti-pattern
- Spring Boot слайсы (`@DataJpaTest`, `@WebMvcTest`) -- быстрее полного `@SpringBootTest`

> **На собеседовании:** интервьюер хочет услышать, чем интеграционный тест отличается от unit-теста, и когда что применять. Частая ошибка -- не упомянуть Testcontainers и сказать, что для интеграционных тестов используется H2.

[к оглавлению](#Тестирование)

---

## Что такое пирамида тестирования?
<!-- grade: junior -->

Пирамида тестирования -- модель, описывающая оптимальное соотношение типов тестов: много быстрых unit-тестов внизу, среднее количество интеграционных в середине, и мало медленных E2E-тестов наверху.

> **Аналогия из жизни:** как проверка здоровья. Ежедневная самодиагностика (unit) дёшева и быстра, ежегодная диспансеризация (интеграционная) требует времени, а полное обследование в стационаре (E2E) -- дорого и редко.

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
| Unit | Много (70-80%) | Миллисекунды | Низкая | Логика методов |
| Integration | Среднее (15-20%) | Секунды | Средняя | Связки компонентов |
| E2E | Мало (5-10%) | Минуты | Высокая | Весь путь пользователя |

### Анти-паттерн -- "рожок мороженого"

Перевёрнутая пирамида: много E2E, мало unit-тестов. Результат -- медленный CI, хрупкие тесты, долгая обратная связь. Встречается, когда тесты пишутся "сверху вниз" без стратегии.

### Ключевые принципы

- Большинство багов ловится unit-тестами -- они дешёвые и быстрые
- Интеграционные -- для проверки "склейки" компонентов (SQL, HTTP, messaging)
- E2E -- только critical path (логин, оплата, основной flow)

### Частые ошибки

- Только E2E тесты -- медленный CI, хрупкие тесты, сложная отладка
- 0% интеграционных -- unit-тесты проходят, но приложение не работает с реальной БД
- Гонка за 100% coverage unit-тестами -- тестируются геттеры/сеттеры, реальная логика не покрыта

### Как используется в 2026

- Пирамида -- ориентир, не догма; в микросервисах доля интеграционных тестов выше
- Contract testing (Spring Cloud Contract, Pact) -- дополнение к пирамиде для межсервисных контрактов

> **На собеседовании:** интервьюер ожидает конкретные цифры соотношения (70/20/10) и понимание, почему пирамида, а не "рожок мороженого". Частая ошибка -- не упомянуть стоимость поддержки каждого уровня.

[к оглавлению](#Тестирование)

---

## Какие существуют виды тестовых объектов?
<!-- grade: middle -->

Тестовые объекты (test doubles) -- подменные объекты, которые заменяют реальные зависимости в тестах. Классификация по Gerard Meszaros: Dummy, Fake, Stub, Spy, Mock.

| Тип | Назначение | Реальная логика | Проверка вызовов |
|-----|-----------|-----------------|-----------------|
| Dummy | Заглушка для компиляции | Нет | Нет |
| Fake | Упрощённая рабочая реализация | Да (упрощённая) | Нет |
| Stub | Возвращает заданные ответы | Нет | Нет |
| Spy | Оборачивает реальный объект | Да | Да |
| Mock | Программируемое поведение + проверка | Нет | Да |

### Dummy

Объект-заглушка, передаётся как параметр, но не используется:

```java
// null или пустой объект -- нужен только для компиляции
var service = new OrderService(new DummyLogger());
```

### Fake

Упрощённая, но рабочая реализация (например, in-memory хранилище вместо реальной БД):

<details>
<summary>Пример Fake-репозитория</summary>

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

</details>

### Stub

Возвращает заранее заданные ответы:

```java
when(userRepository.findById(1L)).thenReturn(Optional.of(testUser));
```

### Spy

Оборачивает реальный объект, записывая вызовы:

```java
List<String> spyList = spy(new ArrayList<>());
spyList.add("item");
verify(spyList).add("item"); // проверяем, что add был вызван
assertEquals(1, spyList.size()); // реальная логика работает
```

### Mock

Объект с запрограммированным поведением и проверкой вызовов:

```java
UserRepository mock = mock(UserRepository.class);
when(mock.findById(1L)).thenReturn(Optional.of(testUser));
// ... использование
verify(mock, times(1)).findById(1L); // проверка вызова
```

### Ключевые принципы

- Stub -- подменяет данные (state verification); Mock -- проверяет взаимодействие (behavior verification)
- В Mockito: `mock()` создаёт mock, `spy()` оборачивает реальный объект
- Предпочитайте state verification (stub + assert result) над behavior verification (mock + verify)

### Частые ошибки

- Mock всего -- если можно протестировать с реальным объектом, mock не нужен
- Verify без необходимости -- `verify` нужен только когда возвращаемое значение не говорит о корректности
- Spy с final-классами -- Mockito не может spy final-классы без mockito-inline

### Как используется в 2026

- Mockito 5.x -- поддержка final-классов по умолчанию (mockmaker-inline)
- Тренд: меньше моков, больше интеграционных тестов с Testcontainers

> **На собеседовании:** интервьюер проверяет, знаете ли вы разницу между всеми пятью видами, а не только между stub и mock. Частая ошибка -- путать Fake и Stub или не знать, что такое Dummy.

[к оглавлению](#Тестирование)

---

## Чем stub отличается от mock?
<!-- grade: junior -->

Stub подменяет данные и проверяет состояние результата, а Mock проверяет факт и порядок взаимодействия с зависимостью. Martin Fowler: "Stubs provide canned answers, mocks verify expectations".

| Критерий | Stub | Mock |
|----------|------|------|
| Цель | Подменить данные | Проверить взаимодействие |
| Проверка | Состояние результата (assertEquals) | Факт/количество вызовов (verify) |
| Хрупкость | Низкая -- рефакторинг не ломает | Высокая -- зависит от реализации |
| Когда использовать | Метод возвращает результат | Метод выполняет side effect |

### Пример

```java
// STUB -- проверяем результат
@Test
void stubExample() {
    when(priceService.getPrice("AAPL")).thenReturn(150.0); // stub
    double total = portfolio.calculateTotal(); // используем stub
    assertEquals(1500.0, total); // проверяем СОСТОЯНИЕ
}

// MOCK -- проверяем взаимодействие
@Test
void mockExample() {
    orderService.placeOrder(order);
    verify(emailService).sendConfirmation(order.getEmail()); // проверяем ВЫЗОВ
}
```

### Когда что выбирать

- Stub (state testing) -- когда метод возвращает значение и результат можно проверить через assert. Тесты менее хрупкие при рефакторинге
- Mock (behavior testing) -- когда метод выполняет побочный эффект (отправка email, запись в лог, вызов внешнего API) и единственный способ проверить -- убедиться, что вызов произошёл

### Частые ошибки

- Verify + when на одном объекте -- stub достаточно, verify избыточен
- Mock на каждый метод -- тест тестирует реализацию, а не поведение; рефакторинг ломает тест

### Как используется в 2026

- Mockito BDD: `given(...).willReturn(...)` + `then(mock).should().method()` -- читаемый стиль
- Предпочитайте stub (state testing) -- тесты менее хрупкие при рефакторинге

> **На собеседовании:** это один из самых частых вопросов по тестированию. Интервьюер хочет услышать конкретную разницу: stub проверяет состояние, mock проверяет поведение. Частая ошибка -- сказать, что stub и mock это одно и то же, просто разные названия.

[к оглавлению](#Тестирование)

---

## Какие основные аннотации JUnit 5 вы знаете?
<!-- grade: junior -->

JUnit 5 предоставляет аннотации для управления жизненным циклом тестов, параметризации, группировки и фильтрации. Ядро: `@Test`, `@BeforeEach`, `@AfterEach`, `@BeforeAll`, `@AfterAll`, `@DisplayName`, `@Disabled`, `@Nested`, `@ParameterizedTest`.

### Жизненный цикл теста

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

### Основные аннотации

| Аннотация | Назначение |
|-----------|-----------|
| `@Test` | Маркирует тестовый метод |
| `@DisplayName` | Человекочитаемое имя теста |
| `@Disabled` | Пропустить тест (аналог `@Ignore` в JUnit 4) |
| `@Tag` | Тегирование для фильтрации (unit, integration) |
| `@Timeout` | Таймаут в секундах |
| `@Nested` | Вложенные тесты для группировки сценариев |
| `@RepeatedTest` | Повторение теста N раз |
| `@ParameterizedTest` | Параметризованный тест с несколькими наборами данных |

### Вложенные тесты

```java
@Nested
@DisplayName("Когда пользователь авторизован")
class WhenAuthenticated {
    @Test void shouldAccessProfile() { }
    @Test void shouldAccessSettings() { }
}
```

### Параметризованные тесты

<details>
<summary>Примеры параметризованных тестов</summary>

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

</details>

### Assertions (проверки)

```java
assertEquals(expected, actual);
assertNotNull(object);
assertTrue(condition);
assertThrows(Exception.class, () -> riskyMethod());
assertDoesNotThrow(() -> safeMethod());
assertTimeout(Duration.ofSeconds(2), () -> longMethod());

// Группировка -- все assertions выполняются, даже если первый упал
assertAll(
    () -> assertEquals("John", user.getName()),
    () -> assertEquals("john@mail.com", user.getEmail()),
    () -> assertNotNull(user.getId())
);
```

### Миграция с JUnit 4 на JUnit 5

| JUnit 4 | JUnit 5 |
|---------|---------|
| `@Before` | `@BeforeEach` |
| `@After` | `@AfterEach` |
| `@BeforeClass` | `@BeforeAll` |
| `@AfterClass` | `@AfterAll` |
| `@Ignore` | `@Disabled` |
| `@Test(expected=...)` | `assertThrows(...)` |
| `@RunWith` | `@ExtendWith` |

### Ключевые принципы

- JUnit 5 = JUnit Platform + JUnit Jupiter (новый API) + JUnit Vintage (совместимость с JUnit 4)
- `@BeforeAll`/`@AfterAll` -- `static` по умолчанию; для нестатических: `@TestInstance(Lifecycle.PER_CLASS)`
- `@ParameterizedTest` -- один тест с множеством входных данных; заменяет дублирование
- `assertAll` -- проверяет все условия, показывая все ошибки сразу

### Частые ошибки

- JUnit 4 аннотации в JUnit 5 -- `@Before` вместо `@BeforeEach`, `@Ignore` вместо `@Disabled`
- `@ParameterizedTest` без источника данных -- нужен `@ValueSource`, `@CsvSource` или `@MethodSource`
- Забыть `@Test` -- метод без аннотации не запустится как тест

### Как используется в 2026

- JUnit 5 -- единственная актуальная версия; JUnit 4 -- legacy
- `@Tag` + Maven/Gradle profiles -- раздельный запуск unit и integration тестов
- IntelliJ IDEA полностью поддерживает `@DisplayName`, `@Nested`, `@ParameterizedTest`

> **На собеседовании:** интервьюер часто просит перечислить аннотации жизненного цикла и объяснить разницу с JUnit 4. Частая ошибка -- не знать про `@ParameterizedTest` и `@Nested`, которые показывают уровень владения фреймворком.

[к оглавлению](#Тестирование)

---

## Что такое Mockito и как его использовать?
<!-- grade: middle -->

Mockito -- фреймворк для создания mock-объектов в unit-тестах. Позволяет подменить зависимости тестируемого класса и проверить взаимодействие с ними.

### Базовая настройка

Для работы с JUnit 5 требуется `@ExtendWith(MockitoExtension.class)`. Аннотация `@Mock` создаёт mock-объект, `@InjectMocks` создаёт тестируемый объект и внедряет в него моки через конструктор или сеттеры.

### Пример

<details>
<summary>Полный пример с Mockito</summary>

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @Mock
    private PaymentService paymentService;

    @InjectMocks
    private OrderService orderService;

    @Test
    void shouldCreateOrder() {
        // given
        Order order = new Order("product-1", BigDecimal.TEN);
        when(orderRepository.save(any(Order.class))).thenReturn(order);
        when(paymentService.charge(any())).thenReturn(true);

        // when
        Order result = orderService.createOrder("product-1", BigDecimal.TEN);

        // then
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

</details>

### ArgumentCaptor -- захват аргументов

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

### Основные методы

| Метод | Назначение |
|-------|-----------|
| `when(...).thenReturn(...)` | Задать возвращаемое значение |
| `when(...).thenThrow(...)` | Бросить исключение |
| `doReturn(...).when(spy)` | Для spy-объектов (без вызова реального метода) |
| `verify(mock).method()` | Проверить, что метод вызван |
| `verify(mock, times(2))` | Проверить количество вызовов |
| `verify(mock, never())` | Проверить, что метод НЕ вызван |
| `any()`, `eq()`, `argThat()` | Матчеры аргументов |

### Ключевые принципы

- `@ExtendWith(MockitoExtension.class)` -- обязательно для JUnit 5
- `@InjectMocks` -- создаёт объект и внедряет `@Mock`-поля через конструктор/сеттеры
- BDD-стиль: `given(...).willReturn(...)` + `then(mock).should().method()`
- Mockito 5.x -- final-классы мокируются по умолчанию

### Частые ошибки

- Mock вместо реального объекта -- если класс простой и без зависимостей, mock не нужен
- `when()` на spy -- используйте `doReturn().when(spy)`, иначе реальный метод выполнится
- Смешивание matchers: `verify(mock).method(eq("a"), any())` -- если один аргумент matcher, все должны быть matchers

### Как используется в 2026

- Mockito 5.x -- стандарт; mockmaker-inline по умолчанию (final, static mocking)
- `@MockitoBean` (Spring Boot 3.4+) -- замена `@MockBean` с лучшей производительностью

> **На собеседовании:** интервьюер часто просит написать тест с Mockito на доске. Важно показать структуру given-when-then, использование `@Mock`/`@InjectMocks` и знание разницы между `when().thenReturn()` и `doReturn().when()`. Частая ошибка -- не знать про matchers и `ArgumentCaptor`.

[к оглавлению](#Тестирование)

---

## Что такое AssertJ?
<!-- grade: junior -->

AssertJ -- библиотека fluent assertions для Java, предоставляющая читаемый chainable API вместо стандартных `assertEquals`/`assertTrue`. Входит в `spring-boot-starter-test`.

### Сравнение с JUnit assertions

| JUnit 5 | AssertJ |
|---------|---------|
| `assertEquals("John", name)` | `assertThat(name).isEqualTo("John")` |
| `assertTrue(list.contains("item"))` | `assertThat(list).contains("item")` |
| `assertNotNull(result)` | `assertThat(result).isNotNull()` |

Преимущество AssertJ -- IDE автоподсказки после `assertThat()`, цепочки проверок, лучшие сообщения об ошибках.

### Проверка коллекций

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

### Проверка исключений

```java
assertThatThrownBy(() -> service.findById(-1L))
    .isInstanceOf(NotFoundException.class)
    .hasMessageContaining("not found")
    .hasFieldOrPropertyWithValue("id", -1L);

assertThatCode(() -> service.findById(1L))
    .doesNotThrowAnyException();
```

### Soft Assertions

Все проверки выполняются, ошибки собираются и показываются вместе:

```java
SoftAssertions.assertSoftly(softly -> {
    softly.assertThat(user.getName()).isEqualTo("John");
    softly.assertThat(user.getEmail()).isEqualTo("john@mail.com");
    softly.assertThat(user.getId()).isNotNull();
});
```

### Ключевые принципы

- `assertThat(actual)` -- точка входа; IDE автоматически подсказывает доступные проверки
- `extracting()` -- извлечение полей из объектов/коллекций для цепочки проверок
- `SoftAssertions` -- аналог `assertAll` из JUnit, но с fluent API

### Частые ошибки

- Путать порядок аргументов -- `assertThat(actual)`, не `assertThat(expected)`
- Импортировать JUnit assertEquals вместе с AssertJ -- конфликт; выберите один стиль

### Как используется в 2026

- AssertJ -- стандарт де-факто; Spring Boot включает в `spring-boot-starter-test`
- Лучшие сообщения об ошибках: `expected: "John" but was: "Jane"` вместо JUnit `expected <John> but was <Jane>`

> **На собеседовании:** интервьюер может попросить переписать JUnit assert на AssertJ. Важно показать знание `assertThat()`, `extracting()`, `assertThatThrownBy()`. Частая ошибка -- не знать про `SoftAssertions` и писать `assertThat(expected)` вместо `assertThat(actual)`.

[к оглавлению](#Тестирование)

---

## Что такое TDD и BDD?
<!-- grade: middle -->

TDD (Test-Driven Development) -- методология разработки, при которой тесты пишутся до кода. BDD (Behavior-Driven Development) -- расширение TDD с фокусом на поведение системы, описанное бизнес-языком в формате Given-When-Then.

### Цикл TDD: Red-Green-Refactor

```
1. RED      -- написать failing test
2. GREEN    -- написать минимальный код, чтобы тест прошёл
3. REFACTOR -- улучшить код, не ломая тесты
```

### Пример TDD

```java
// Шаг 1: RED -- тест для несуществующего метода
@Test
void shouldCalculateDiscount() {
    PricingService service = new PricingService();
    assertEquals(90.0, service.applyDiscount(100.0, 10)); // метода ещё нет
}

// Шаг 2: GREEN -- минимальная реализация
public double applyDiscount(double price, int percent) {
    return price * (100 - percent) / 100;
}

// Шаг 3: REFACTOR -- улучшаем, тест остаётся зелёным
```

### BDD: Given-When-Then

```java
@Test
@DisplayName("Когда применяется скидка 10%, цена уменьшается на 10%")
void discountScenario() {
    // Given -- начальное состояние
    var pricing = new PricingService();
    double originalPrice = 100.0;

    // When -- действие
    double discountedPrice = pricing.applyDiscount(originalPrice, 10);

    // Then -- ожидаемый результат
    assertThat(discountedPrice).isEqualTo(90.0);
}
```

### Сравнение TDD и BDD

| Критерий | TDD | BDD |
|----------|-----|-----|
| Фокус | Корректность кода | Поведение системы |
| Язык | Технический | Бизнес-язык |
| Формат | Тест -> Код -> Рефакторинг | Given-When-Then |
| Участники | Разработчики | Разработчики + бизнес |
| Инструменты | JUnit, TestNG | Cucumber, JBehave |

### Ключевые принципы

- TDD заставляет думать о дизайне API до реализации -- код получается тестируемым
- BDD Given-When-Then -- универсальная структура любого теста, даже без формального BDD
- Mockito BDD: `given(mock.method()).willReturn(value)` + `then(mock).should().method()`

### Частые ошибки

- TDD для всего -- для тривиального CRUD overhead превышает пользу
- Слишком детальные тесты -- тест привязан к реализации, рефакторинг ломает тесты
- BDD без участия бизнеса -- Given-When-Then полезен как формат, но полноценный BDD подразумевает Cucumber + бизнес-аналитиков

### Как используется в 2026

- Given-When-Then -- стандартная структура тестов даже без формального BDD
- TDD применяется точечно для сложной бизнес-логики, алгоритмов, edge cases
- Cucumber/Gherkin -- для acceptance-тестов с участием non-technical stakeholders

> **На собеседовании:** интервьюер хочет услышать цикл Red-Green-Refactor для TDD и формат Given-When-Then для BDD. Частая ошибка -- сказать, что TDD это "просто писать тесты" без упоминания цикла, или спутать BDD с обычным тестированием.

[к оглавлению](#Тестирование)

---

## Что такое Testcontainers и зачем они нужны?
<!-- grade: middle -->

Testcontainers -- Java-библиотека для запуска Docker-контейнеров в тестах. Позволяет тестировать с реальными БД, брокерами сообщений, кэшами вместо in-memory заглушек.

> **Аналогия из жизни:** вместо того чтобы репетировать речь перед зеркалом (H2 in-memory), вы выступаете перед небольшой реальной аудиторией (настоящая PostgreSQL в контейнере). Обратная связь гораздо ближе к реальности.

### Пример

<details>
<summary>Полный пример с PostgreSQL</summary>

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

</details>

### Доступные контейнеры

| Контейнер | Класс | Образ |
|-----------|-------|-------|
| PostgreSQL | `PostgreSQLContainer` | `postgres:16` |
| MongoDB | `MongoDBContainer` | `mongo:7` |
| Kafka | `KafkaContainer` | `confluentinc/cp-kafka:7.6.0` |
| Redis | `GenericContainer` | `redis:7` |
| LocalStack (AWS) | `LocalStackContainer` | `localstack/localstack:3` |

### Ключевые принципы

- Testcontainers > H2 -- тестируете на реальной БД; нет расхождений синтаксиса и поведения
- `@Container` + `@Testcontainers` -- автоматический lifecycle (start/stop)
- `@DynamicPropertySource` -- внедрение динамических свойств (порт, URL) в Spring context
- Контейнеры переиспользуются между тестами через `static` поля

### Частые ошибки

- Docker не установлен -- Testcontainers требует Docker; в CI нужен Docker-in-Docker или Testcontainers Cloud
- Не `static` контейнер -- без `static` контейнер перезапускается для каждого теста (медленно)
- Не указать версию образа -- `new PostgreSQLContainer<>("postgres")` приводит к непредсказуемой версии

### Как используется в 2026

- Testcontainers -- стандарт для интеграционных тестов в Java-экосистеме
- Spring Boot 3.1+ -- нативная поддержка через `@ServiceConnection` (без `@DynamicPropertySource`)
- Testcontainers Desktop / Cloud -- запуск без локального Docker

> **На собеседовании:** интервьюер хочет услышать, почему Testcontainers лучше H2, и как настраивается интеграция со Spring Boot. Частая ошибка -- не знать про `@DynamicPropertySource` и `@ServiceConnection`.

[к оглавлению](#Тестирование)

---

## Как тестировать исключения в JUnit 5?
<!-- grade: junior -->

В JUnit 5 исключения тестируются через `assertThrows()`, который возвращает пойманное исключение для дополнительных проверок. В AssertJ используется `assertThatThrownBy()` с fluent API.

### assertThrows (JUnit 5)

```java
@Test
void shouldThrowWhenUserNotFound() {
    Exception ex = assertThrows(NotFoundException.class,
        () -> userService.findById(-1L));

    assertThat(ex.getMessage()).contains("not found");
}

@Test
void shouldNotThrowForValidInput() {
    assertDoesNotThrow(() -> userService.findById(1L));
}
```

### assertThatThrownBy (AssertJ)

```java
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

### Сравнение подходов

| Подход | Библиотека | Стиль | Цепочки проверок |
|--------|-----------|-------|-----------------|
| `assertThrows()` | JUnit 5 | Императивный | Нет (отдельные assert) |
| `assertThatThrownBy()` | AssertJ | Fluent | Да |
| `@Test(expected=...)` | JUnit 4 | Аннотация | Нет (устарело) |

### Ключевые принципы

- `assertThrows` возвращает исключение -- можно проверить message, cause, поля
- `assertThatThrownBy` (AssertJ) -- fluent API с цепочкой проверок
- В JUnit 4 использовались `@Test(expected=...)` и `ExpectedException` rule -- устарело

### Частые ошибки

- `@Test(expected = Exception.class)` -- это JUnit 4; в JUnit 5 используйте `assertThrows`
- Слишком широкий тип -- `assertThrows(Exception.class, ...)` поймает любое исключение; используйте конкретный тип
- Не проверять message -- два разных `IllegalArgumentException` могут означать разные ошибки

### Как используется в 2026

- `assertThrows` (JUnit 5) + `assertThatThrownBy` (AssertJ) -- два стандартных подхода

> **На собеседовании:** интервьюер может попросить написать тест на исключение. Важно показать, что вы проверяете не только тип, но и сообщение, и cause. Частая ошибка -- использовать `@Test(expected=...)` из JUnit 4 или ловить слишком широкий тип исключения.

[к оглавлению](#Тестирование)

---

## Что такое Code Coverage?
<!-- grade: junior -->

Code Coverage (покрытие кода) -- метрика, показывающая, какой процент кода выполняется при запуске тестов. Стандартный инструмент в Java-экосистеме -- JaCoCo.

### Типы покрытия

| Тип | Что измеряет | Пример |
|-----|-------------|--------|
| Line coverage | % строк, выполненных тестами | 80 из 100 строк = 80% |
| Branch coverage | % ветвлений (if/else, switch) | Протестирована ли каждая ветка if? |
| Method coverage | % методов, вызванных тестами | Все ли публичные методы вызваны? |
| Instruction coverage | % байткод-инструкций | Наиболее точная метрика |

### JaCoCo -- настройка в Maven

<details>
<summary>Конфигурация JaCoCo в pom.xml</summary>

```xml
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
                                <minimum>0.80</minimum>
                            </limit>
                        </limits>
                    </rule>
                </rules>
            </configuration>
        </execution>
    </executions>
</plugin>
```

</details>

### Разумные пороги покрытия

| Метрика | Минимум | Комментарий |
|---------|---------|------------|
| Line coverage | 80% | Разумный минимум для большинства проектов |
| Branch coverage | 70% | Важнее line coverage -- показывает edge cases |
| 100% | Не цель | Геттеры, DTO, конфигурация не нуждаются в тестах |

### Mutation Testing -- следующий уровень

Code coverage показывает, что код выполняется, но не показывает, что тесты действительно ловят ошибки. Mutation testing (PIT) вносит мутации в код (меняет `>` на `<`, удаляет строки) и проверяет, падают ли тесты. Если тест не замечает мутацию -- он бесполезен.

### Ключевые принципы

- 80% line coverage -- разумный минимум для большинства проектов
- 100% -- не цель: геттеры, конфигурация, DTO не нуждаются в тестах
- Branch coverage важнее line coverage -- показывает, протестированы ли edge cases
- JaCoCo интегрируется с SonarQube для визуализации и трендов

### Частые ошибки

- Гонка за 100% -- тестирование `toString()` и конструкторов ради метрики бессмысленно
- Coverage без assertions -- тест вызывает метод, но не проверяет результат; coverage растёт, качество -- нет
- Exclusion вместо исправления -- исключение пакетов из отчёта вместо написания тестов

### Как используется в 2026

- JaCoCo + SonarQube -- стандартная связка в CI/CD pipeline
- Quality Gate: "coverage не должен падать" (delta check) важнее абсолютной цифры
- Mutation testing (PIT) -- более продвинутая альтернатива: проверяет, ловят ли тесты реальные мутации в коде

> **На собеседовании:** интервьюер хочет услышать не только "JaCoCo измеряет покрытие", но и понимание, что высокий coverage не гарантирует качество тестов (coverage без assertions). Частая ошибка -- назвать 100% как цель или не знать про branch coverage.

[к оглавлению](#Тестирование)

---

[Вопросы для собеседования](README.md)
