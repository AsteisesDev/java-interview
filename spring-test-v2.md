[Вопросы для собеседования](README.md)

# Тестирование в Spring Boot

+ [Каковы основные отличия JUnit 5 от JUnit 4?](#каковы-основные-отличия-junit-5-от-junit-4)
+ [Какие новые аннотации появились в JUnit 5?](#какие-новые-аннотации-появились-в-junit-5)
+ [Какие методы утверждений (assertions) предоставляет JUnit 5?](#какие-методы-утверждений-assertions-предоставляет-junit-5)
+ [Что такое SpringBootTest и как он работает?](#что-такое-springboottest-и-как-он-работает)
+ [Какие режимы webEnvironment существуют в SpringBootTest?](#какие-режимы-webenvironment-существуют-в-springboottest)
+ [Как Spring кэширует контекст в тестах и что такое DirtiesContext?](#как-spring-кэширует-контекст-в-тестах-и-что-такое-dirtiescontext)
+ [Что такое MockBean и SpyBean?](#что-такое-mockbean-и-spybean)
+ [Каковы основы работы с Mockito?](#каковы-основы-работы-с-mockito)
+ [В чём разница между doReturn/when и when/thenReturn в Mockito?](#в-чём-разница-между-doreturnwhen-и-whenthenreturn-в-mockito)
+ [Как тестировать контроллеры с помощью MockMvc и WebMvcTest?](#как-тестировать-контроллеры-с-помощью-mockmvc-и-webmvctest)
+ [Как выполнять запросы и проверять результаты в MockMvc?](#как-выполнять-запросы-и-проверять-результаты-в-mockmvc)
+ [Как тестировать сервисный слой с помощью MockitoExtension?](#как-тестировать-сервисный-слой-с-помощью-mockitoextension)
+ [Как тестировать репозитории с помощью DataJpaTest?](#как-тестировать-репозитории-с-помощью-datajpatest)
+ [Что такое Testcontainers и зачем они нужны?](#что-такое-testcontainers-и-зачем-они-нужны)
+ [Как использовать Sql и SqlGroup для подготовки тестовых данных?](#как-использовать-sql-и-sqlgroup-для-подготовки-тестовых-данных)
+ [Как использовать TestRestTemplate для интеграционных тестов?](#как-использовать-testresttemplate-для-интеграционных-тестов)
+ [Как использовать WebTestClient для тестирования реактивных и обычных приложений?](#как-использовать-webtestclient-для-тестирования-реактивных-и-обычных-приложений)
+ [Как настроить профили для тестов?](#как-настроить-профили-для-тестов)
+ [Как работает Transactional в тестах и что такое автоматический rollback?](#как-работает-transactional-в-тестах-и-что-такое-автоматический-rollback)
+ [Что такое AssertJ и чем он лучше стандартных assertions?](#что-такое-assertj-и-чем-он-лучше-стандартных-assertions)
+ [Что такое ParameterizedTest и как он используется?](#что-такое-parameterizedtest-и-как-он-используется)
+ [Что такое Nested тесты в JUnit 5?](#что-такое-nested-тесты-в-junit-5)
+ [Что такое TDD и BDD? Чем они отличаются?](#что-такое-tdd-и-bdd-чем-они-отличаются)
+ [Как тестировать Spring Security?](#как-тестировать-spring-security)

## Каковы основные отличия JUnit 5 от JUnit 4?
<!-- grade: junior -->

JUnit 5 — полностью переписанный фреймворк для тестирования, состоящий из трёх модулей:

- **JUnit Platform** — основа для запуска тестовых фреймворков на JVM.
- **JUnit Jupiter** — новая модель программирования и расширений для написания тестов.
- **JUnit Vintage** — обеспечивает обратную совместимость с JUnit 3 и JUnit 4.

### Основные отличия

| Критерий | JUnit 4 | JUnit 5 |
|---|---|---|
| Пакет | `org.junit` | `org.junit.jupiter.api` |
| Аннотация теста | `@Test` (с параметрами `expected`, `timeout`) | `@Test` (без параметров) |
| Перед каждым тестом | `@Before` | `@BeforeEach` |
| После каждого теста | `@After` | `@AfterEach` |
| Перед всеми тестами | `@BeforeClass` | `@BeforeAll` |
| После всех тестов | `@AfterClass` | `@AfterAll` |
| Отключение теста | `@Ignore` | `@Disabled` |
| Расширения | `@RunWith`, `@Rule` | `@ExtendWith` |
| Минимальная версия Java | Java 5 | Java 8 |

<details><summary>Пример кода: JUnit 4 vs JUnit 5</summary>

```java
// JUnit 4
import org.junit.Test;
import org.junit.Before;
import static org.junit.Assert.assertEquals;

public class CalculatorTest {
    private Calculator calculator;

    @Before
    public void setUp() {
        calculator = new Calculator();
    }

    @Test
    public void testAdd() {
        assertEquals(5, calculator.add(2, 3));
    }
}

// JUnit 5
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.BeforeEach;
import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculatorTest {
    private Calculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }

    @Test
    void testAdd() {
        assertEquals(5, calculator.add(2, 3));
    }
}
```

</details>

В JUnit 5 тестовые классы и методы не обязаны быть `public`.

> **На собеседовании:** интервьюер ожидает знание модульной архитектуры JUnit 5 (Platform / Jupiter / Vintage) и конкретных отличий в аннотациях. Частая ошибка — забыть про `@ExtendWith` как замену `@RunWith` + `@Rule`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Какие новые аннотации появились в JUnit 5?
<!-- grade: junior -->

JUnit 5 вводит ряд новых аннотаций, расширяющих возможности написания тестов:

- `@DisplayName` — задаёт человекочитаемое имя теста, которое отображается в отчётах.
- `@Nested` — позволяет создавать вложенные тестовые классы для логической группировки.
- `@ParameterizedTest` — параметризованный тест с различными источниками данных.
- `@RepeatedTest` — повторяет тест указанное число раз.
- `@Tag` — помечает тесты тегами для фильтрации при запуске.
- `@Disabled` — отключает тест или тестовый класс.
- `@Timeout` — задаёт максимальное время выполнения теста.
- `@ExtendWith` — регистрирует расширения (замена `@RunWith` и `@Rule`).
- `@TempDir` — инъекция временной директории для файловых тестов.

<details><summary>Пример кода</summary>

```java
import org.junit.jupiter.api.*;

@DisplayName("Тесты калькулятора")
class CalculatorTest {

    @Test
    @DisplayName("Сложение двух положительных чисел")
    void shouldAddTwoPositiveNumbers() {
        assertEquals(5, calculator.add(2, 3));
    }

    @Test
    @Disabled("Деление ещё не реализовано")
    void shouldDivide() {
        // ...
    }

    @RepeatedTest(5)
    @DisplayName("Повторяющийся тест генерации случайных чисел")
    void randomTest(RepetitionInfo info) {
        System.out.println("Повторение: " + info.getCurrentRepetition());
    }

    @Test
    @Tag("slow")
    @Timeout(value = 5, unit = TimeUnit.SECONDS)
    void longRunningTest() {
        // долгий тест
    }
}
```

</details>

> **На собеседовании:** достаточно перечислить 5-6 ключевых аннотаций и описать их назначение. Частая ошибка — путать `@RepeatedTest` и `@ParameterizedTest`: первый повторяет один и тот же тест, второй запускает тест с разными данными.

[к оглавлению](#тестирование-в-spring-boot)

---

## Какие методы утверждений (assertions) предоставляет JUnit 5?
<!-- grade: junior -->

JUnit 5 значительно расширил набор утверждений по сравнению с JUnit 4, добавив `assertAll`, `assertThrows` и `assertTimeout`.

### assertAll — группировка проверок

Группирует несколько проверок и выполняет их все, даже если какая-то из них упадёт. Это позволяет увидеть все ошибки сразу:

```java
@Test
void testUserProperties() {
    User user = userService.findById(1L);

    assertAll("Проверка свойств пользователя",
        () -> assertEquals("Иван", user.getFirstName()),
        () -> assertEquals("Иванов", user.getLastName()),
        () -> assertEquals("ivan@example.com", user.getEmail()),
        () -> assertNotNull(user.getCreatedAt())
    );
}
```

### assertThrows — проверка исключений

Проверяет, что выполнение кода бросает ожидаемое исключение (замена `@Test(expected = ...)` из JUnit 4):

```java
@Test
void shouldThrowExceptionWhenDivideByZero() {
    Calculator calculator = new Calculator();

    ArithmeticException exception = assertThrows(
        ArithmeticException.class,
        () -> calculator.divide(10, 0)
    );

    assertEquals("/ by zero", exception.getMessage());
}
```

### assertTimeout — проверка времени выполнения

Проверяет, что операция выполняется за отведённое время. В отличие от `assertTimeoutPreemptively`, не прерывает выполнение:

```java
@Test
void shouldCompleteInTime() {
    assertTimeout(Duration.ofSeconds(2), () -> {
        service.processData();
    });
}

@Test
void shouldCompletePreemptively() {
    // прервёт выполнение, если превышен лимит
    assertTimeoutPreemptively(Duration.ofMillis(500), () -> {
        Thread.sleep(100);
        return "результат";
    });
}
```

### Другие полезные assertions

<details><summary>Пример кода</summary>

```java
@Test
void otherAssertions() {
    // Проверка на null
    assertNotNull(object);
    assertNull(nullObject);

    // Проверка истинности/ложности
    assertTrue(condition);
    assertFalse(condition);

    // Проверка ссылочного равенства
    assertSame(expected, actual);
    assertNotSame(expected, actual);

    // Проверка с итерируемыми коллекциями
    assertIterableEquals(List.of(1, 2, 3), actualList);

    // Проверка строк на совпадение строки
    assertLinesMatch(
        List.of("первая строка", "\\d+ элементов", "последняя строка"),
        actualLines
    );
}
```

</details>

> **На собеседовании:** ключевые методы для упоминания — `assertAll`, `assertThrows`, `assertTimeout`. Частая ошибка — не знать про `assertAll` и описывать только базовые `assertEquals`/`assertTrue`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое SpringBootTest и как он работает?
<!-- grade: junior -->

`@SpringBootTest` — аннотация, которая создаёт полный контекст Spring-приложения для интеграционного тестирования. Она является заменой устаревших `@SpringApplicationConfiguration` и `@IntegrationTest`.

### Что происходит при использовании SpringBootTest

1. Ищется класс с аннотацией `@SpringBootConfiguration` (обычно главный класс с `@SpringBootApplication`).
2. Создаётся полный `ApplicationContext` со всеми бинами.
3. Активируется автоконфигурация Spring Boot.
4. Регистрируется `TestRestTemplate` и/или `WebTestClient` (при необходимости).

```java
@SpringBootTest
class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @Test
    void shouldCreateUser() {
        User user = new User("Иван", "ivan@example.com");
        User saved = userService.create(user);

        assertNotNull(saved.getId());
        assertEquals("Иван", saved.getName());
    }
}
```

Можно указать конкретные конфигурационные классы или переопределить свойства:

```java
@SpringBootTest(classes = {TestConfig.class, UserService.class})
class CustomContextTest { /* ... */ }

@SpringBootTest(properties = {
    "spring.datasource.url=jdbc:h2:mem:testdb",
    "app.feature.enabled=true"
})
class PropertyOverrideTest { /* ... */ }
```

> **На собеседовании:** важно объяснить, что `@SpringBootTest` поднимает полный контекст и это дорогая операция. Частая ошибка — использовать `@SpringBootTest` для юнит-тестов сервисов, где достаточно `@ExtendWith(MockitoExtension.class)`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Какие режимы webEnvironment существуют в SpringBootTest?
<!-- grade: middle -->

Параметр `webEnvironment` аннотации `@SpringBootTest` определяет, как будет запущено веб-окружение.

| Режим | Описание |
|---|---|
| `MOCK` (по умолчанию) | Создаёт mock-веб-окружение. Реальный сервер не запускается. Используется совместно с `MockMvc`. |
| `RANDOM_PORT` | Запускает реальный веб-сервер на случайном свободном порту. Идеален для интеграционных тестов с `TestRestTemplate`. |
| `DEFINED_PORT` | Запускает реальный веб-сервер на порту из конфигурации (`server.port`), по умолчанию 8080. |
| `NONE` | Загружает `ApplicationContext` без какого-либо веб-окружения. Подходит для тестирования сервисов и репозиториев. |

<details><summary>Примеры использования</summary>

```java
// Мок-окружение (без реального сервера)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
class MockEnvTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void testWithMockMvc() throws Exception {
        mockMvc.perform(get("/api/users"))
               .andExpect(status().isOk());
    }
}

// Реальный сервер на случайном порту
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class RandomPortTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @LocalServerPort
    private int port;

    @Test
    void testWithRealServer() {
        ResponseEntity<String> response =
            restTemplate.getForEntity("/api/users", String.class);
        assertEquals(HttpStatus.OK, response.getStatusCode());
    }
}

// Без веб-окружения
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.NONE)
class NoWebTest {

    @Autowired
    private UserService userService;

    @Test
    void testServiceOnly() {
        assertNotNull(userService.findAll());
    }
}
```

</details>

> **На собеседовании:** интервьюер хочет услышать, когда какой режим применять. Частая ошибка — не знать разницу между `MOCK` и `RANDOM_PORT` и всегда использовать `RANDOM_PORT`, замедляя тесты.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как Spring кэширует контекст в тестах и что такое DirtiesContext?
<!-- grade: middle -->

Spring TestContext Framework кэширует `ApplicationContext` между тестами для повышения производительности. Контекст создаётся один раз и переиспользуется всеми тестами с одинаковой конфигурацией.

> **Аналогия из жизни:** кэширование контекста — как общий рабочий стол в офисе. Пока настройки одинаковые, все используют один стол. Но если кто-то разлил кофе (`@DirtiesContext`), стол нужно заменить.

### Ключ кэша формируется на основе

- Набора конфигурационных классов / XML-файлов
- Активных профилей (`@ActiveProfiles`)
- Свойств (`properties`)
- Наличия `@MockBean` / `@SpyBean`
- Других параметров контекста

Если два тестовых класса имеют одинаковый набор параметров, они разделят один и тот же контекст.

### DirtiesContext

`@DirtiesContext` помечает тест, после которого контекст считается «грязным» и должен быть пересоздан. Это необходимо, когда тест изменяет состояние контекста (меняет бин, модифицирует синглтон, изменяет данные в кэше).

```java
@SpringBootTest
class ContextCachingTest {

    @Test
    @DirtiesContext
    void testThatModifiesContext() {
        cacheService.clearAll();
    }

    // После предыдущего теста контекст будет пересоздан
    @Test
    void testAfterDirtyContext() {
        // работает с чистым контекстом
    }
}
```

### Режимы DirtiesContext на уровне класса

```java
// Пересоздавать контекст после каждого тестового метода
@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_EACH_TEST_METHOD)

// Пересоздать контекст после всего тестового класса
@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_CLASS)

// Пересоздать контекст до класса
@DirtiesContext(classMode = DirtiesContext.ClassMode.BEFORE_CLASS)
```

> **На собеседовании:** важно подчеркнуть, что злоупотребление `@DirtiesContext` значительно замедляет тесты, так как пересоздание контекста — дорогостоящая операция. Частая ошибка — ставить `@DirtiesContext` на каждый тест «для надёжности».

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое MockBean и SpyBean?
<!-- grade: middle -->

`@MockBean` и `@SpyBean` — аннотации Spring Boot Test, позволяющие подменять бины в контексте Spring моками и шпионами Mockito.

### MockBean

`@MockBean` заменяет бин в контексте полностью мок-объектом. Все методы по умолчанию возвращают `null` (или значение по умолчанию для примитивов). Полезно для изоляции тестируемого компонента от зависимостей:

<details><summary>Пример кода</summary>

```java
@SpringBootTest
class OrderServiceTest {

    @Autowired
    private OrderService orderService;

    @MockBean
    private PaymentService paymentService;

    @MockBean
    private EmailService emailService;

    @Test
    void shouldCreateOrder() {
        when(paymentService.processPayment(any()))
            .thenReturn(new PaymentResult(true, "txn-123"));

        Order order = orderService.createOrder(new OrderRequest("item-1", 100));

        assertNotNull(order);
        verify(paymentService).processPayment(any());
        verify(emailService).sendConfirmation(any());
    }
}
```

</details>

### SpyBean

`@SpyBean` оборачивает реальный бин из контекста шпионом Mockito. Реальные методы продолжают работать, но их можно перехватить и переопределить:

<details><summary>Пример кода</summary>

```java
@SpringBootTest
class NotificationServiceTest {

    @Autowired
    private NotificationService notificationService;

    @SpyBean
    private AuditService auditService;

    @Test
    void shouldLogAuditEvent() {
        notificationService.notifyUser(1L, "Сообщение");
        verify(auditService).log(eq(1L), contains("Сообщение"));
    }

    @Test
    void shouldHandleAuditFailure() {
        doThrow(new RuntimeException("Audit unavailable"))
            .when(auditService).log(anyLong(), anyString());

        assertDoesNotThrow(
            () -> notificationService.notifyUser(1L, "Сообщение")
        );
    }
}
```

</details>

### Важные нюансы

- Начиная со Spring Boot 3.4 аннотации `@MockBean` и `@SpyBean` объявлены устаревшими (deprecated) в пользу `@MockitoBean` и `@MockitoSpyBean`, а также поддержки через `BeanOverride`. Однако они по-прежнему широко используются.
- Каждая уникальная комбинация `@MockBean`/`@SpyBean` создаёт отдельный кэш контекста. Чрезмерное использование разных мок-бинов в разных тестах приведёт к множественным пересозданиям контекста.

> **На собеседовании:** интервьюер хочет услышать разницу между `@MockBean` (полная подмена) и `@SpyBean` (обёртка над реальным бином). Частая ошибка — не знать про влияние `@MockBean` на кэширование контекста.

[к оглавлению](#тестирование-в-spring-boot)

---

## Каковы основы работы с Mockito?
<!-- grade: junior -->

Mockito — библиотека для создания мок-объектов в Java-тестах. Она позволяет подменять зависимости и задавать их поведение.

### Создание мока

```java
// Программно
UserRepository userRepository = Mockito.mock(UserRepository.class);

// С помощью аннотации (требуется @ExtendWith(MockitoExtension.class))
@Mock
private UserRepository userRepository;
```

### when/thenReturn — задание возвращаемого значения

```java
@Test
void shouldReturnUser() {
    User user = new User(1L, "Иван");
    when(userRepository.findById(1L)).thenReturn(Optional.of(user));

    Optional<User> result = userRepository.findById(1L);

    assertTrue(result.isPresent());
    assertEquals("Иван", result.get().getName());
}
```

### when/thenThrow — задание выбрасываемого исключения

```java
when(userRepository.findById(999L))
    .thenThrow(new EntityNotFoundException("Пользователь не найден"));
```

### verify — проверка вызова метода

```java
@Test
void shouldSaveUser() {
    User user = new User("Иван");
    userService.create(user);

    verify(userRepository, times(1)).save(user);
    verify(userRepository, never()).delete(any());
    verify(userRepository, atLeastOnce()).findAll();
}
```

### ArgumentCaptor — захват аргументов

```java
@Test
void shouldCaptureArgument() {
    userService.create(new UserDto("Иван", "ivan@example.com"));

    ArgumentCaptor<User> captor = ArgumentCaptor.forClass(User.class);
    verify(userRepository).save(captor.capture());

    User savedUser = captor.getValue();
    assertEquals("Иван", savedUser.getName());
    assertEquals("ivan@example.com", savedUser.getEmail());
}
```

### ArgumentMatchers — гибкое сопоставление аргументов

```java
when(userRepository.findByName(anyString())).thenReturn(List.of(user));
when(userRepository.findById(eq(1L))).thenReturn(Optional.of(user));
when(userRepository.findByAgeGreaterThan(argThat(age -> age > 0)))
    .thenReturn(List.of(user));
```

> **На собеседовании:** минимальный набор, который нужно знать — `when/thenReturn`, `verify`, `@Mock`, `@InjectMocks`. Частая ошибка — путать `any()` и `eq()`: если хотя бы один аргумент использует матчер, все остальные тоже должны использовать матчеры.

[к оглавлению](#тестирование-в-spring-boot)

---

## В чём разница между doReturn/when и when/thenReturn в Mockito?
<!-- grade: middle -->

Обе конструкции задают поведение мока, но имеют критические различия при работе со spy-объектами и void-методами.

### when/thenReturn — стандартный подход

Вызывает реальный метод при настройке мока (что может вызвать проблемы для spy-объектов):

```java
when(mock.someMethod()).thenReturn("результат");
```

### doReturn/when — безопасный подход

Не вызывает реальный метод при настройке:

```java
doReturn("результат").when(mock).someMethod();
```

### Ключевые различия

**1. Spy-объекты:**

```java
List<String> spyList = spy(new ArrayList<>());

// Опасно! get(0) вызовется на пустом списке → IndexOutOfBoundsException
// when(spyList.get(0)).thenReturn("элемент");

// Безопасно — реальный метод не вызывается при настройке
doReturn("элемент").when(spyList).get(0);
```

**2. Void-методы:**

```java
// Не скомпилируется:
// when(mock.voidMethod()).thenThrow(new RuntimeException());

// Правильно:
doThrow(new RuntimeException("ошибка")).when(mock).voidMethod();
doNothing().when(mock).voidMethod();
```

**3. Цепочка вызовов:**

```java
when(mock.nextValue())
    .thenReturn("первый")
    .thenReturn("второй")
    .thenThrow(new NoSuchElementException());
```

### Сводная таблица

| Метод | Когда использовать |
|---|---|
| `when/thenReturn` | Стандартный случай с моками |
| `doReturn/when` | Spy-объекты, void-методы |
| `doThrow/when` | Void-методы, которые должны бросить исключение |
| `doAnswer/when` | Сложная логика, зависящая от аргументов |
| `doNothing/when` | Подавление void-метода у spy |

> **На собеседовании:** ключевой момент — объяснить, почему `when/thenReturn` опасен для spy (реальный метод вызывается при настройке). Частая ошибка — не знать, что для void-методов `when/thenReturn` не скомпилируется.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как тестировать контроллеры с помощью MockMvc и WebMvcTest?
<!-- grade: middle -->

`@WebMvcTest` — срезовая (slice) аннотация, которая поднимает только веб-слой Spring MVC: контроллеры, `@ControllerAdvice`, `@JsonComponent`, фильтры, `WebMvcConfigurer`. Сервисы, репозитории и прочие бины не загружаются — их нужно мокать.

`MockMvc` — утилита для тестирования контроллеров без запуска реального HTTP-сервера. Запросы обрабатываются через `DispatcherServlet` в имитированном окружении.

<details><summary>Пример теста контроллера</summary>

```java
@WebMvcTest(UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    void shouldReturnAllUsers() throws Exception {
        List<UserDto> users = List.of(
            new UserDto(1L, "Иван"),
            new UserDto(2L, "Мария")
        );
        when(userService.findAll()).thenReturn(users);

        mockMvc.perform(get("/api/users")
                .contentType(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$", hasSize(2)))
            .andExpect(jsonPath("$[0].name").value("Иван"))
            .andExpect(jsonPath("$[1].name").value("Мария"));
    }

    @Test
    void shouldCreateUser() throws Exception {
        UserDto newUser = new UserDto(null, "Пётр");
        UserDto savedUser = new UserDto(1L, "Пётр");
        when(userService.create(any(UserDto.class))).thenReturn(savedUser);

        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\": \"Пётр\"}"))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.id").value(1))
            .andExpect(jsonPath("$.name").value("Пётр"));
    }

    @Test
    void shouldReturn404WhenUserNotFound() throws Exception {
        when(userService.findById(999L))
            .thenThrow(new UserNotFoundException(999L));

        mockMvc.perform(get("/api/users/999"))
            .andExpect(status().isNotFound());
    }
}
```

</details>

### WebMvcTest vs SpringBootTest + AutoConfigureMockMvc

| Критерий | `@WebMvcTest` | `@SpringBootTest` + `@AutoConfigureMockMvc` |
|---|---|---|
| Контекст | Только MVC-слой | Полный контекст приложения |
| Скорость | Быстро | Медленнее |
| Зависимости | Нужно мокать через `@MockBean` | Реальные бины доступны |
| Применение | Unit-тесты контроллеров | Интеграционные тесты |

> **На собеседовании:** интервьюер ожидает понимание разницы между `@WebMvcTest` (только MVC-слой) и `@SpringBootTest` + `@AutoConfigureMockMvc` (полный контекст). Частая ошибка — не мокать сервисы при `@WebMvcTest`, из-за чего тест падает с `NoSuchBeanDefinitionException`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как выполнять запросы и проверять результаты в MockMvc?
<!-- grade: middle -->

`MockMvc` предоставляет DSL для выполнения HTTP-запросов и проверки ответов без запуска реального сервера.

### Выполнение запросов (perform)

<details><summary>Примеры запросов</summary>

```java
// GET-запрос с параметрами
mockMvc.perform(get("/api/users")
    .param("page", "0")
    .param("size", "10")
    .header("Authorization", "Bearer token123")
    .accept(MediaType.APPLICATION_JSON));

// POST-запрос с JSON-телом
mockMvc.perform(post("/api/users")
    .contentType(MediaType.APPLICATION_JSON)
    .content(objectMapper.writeValueAsString(userDto)));

// PUT-запрос
mockMvc.perform(put("/api/users/{id}", 1)
    .contentType(MediaType.APPLICATION_JSON)
    .content("{\"name\": \"Обновлённый\"}"));

// DELETE-запрос
mockMvc.perform(delete("/api/users/{id}", 1));

// Multipart (загрузка файлов)
mockMvc.perform(multipart("/api/upload")
    .file(new MockMultipartFile("file", "test.txt",
        "text/plain", "содержимое".getBytes())));
```

</details>

### Проверка результатов (andExpect)

```java
mockMvc.perform(get("/api/users/1"))
    // Статус ответа
    .andExpect(status().isOk())                 // 200
    .andExpect(status().isCreated())             // 201
    .andExpect(status().isBadRequest())          // 400
    .andExpect(status().isNotFound())            // 404

    // Заголовки
    .andExpect(header().string("Content-Type", "application/json"))
    .andExpect(header().exists("X-Custom-Header"))

    // Содержимое ответа
    .andExpect(content().string("hello"))
    .andExpect(content().contentType(MediaType.APPLICATION_JSON));
```

### Работа с JSON через jsonPath

```java
mockMvc.perform(get("/api/users"))
    .andExpect(jsonPath("$[0].name").value("Иван"))
    .andExpect(jsonPath("$", hasSize(3)))
    .andExpect(jsonPath("$[0].email").exists())
    .andExpect(jsonPath("$[0].password").doesNotExist())
    .andExpect(jsonPath("$[0].id").isNumber())
    .andExpect(jsonPath("$").isArray())
    .andExpect(jsonPath("$[0].name", containsString("Ив")))
    .andExpect(jsonPath("$[0].age", greaterThan(18)));
```

### Получение и обработка результата

```java
MvcResult result = mockMvc.perform(get("/api/users"))
    .andExpect(status().isOk())
    .andDo(print())  // выводит детали запроса/ответа в консоль
    .andReturn();

String json = result.getResponse().getContentAsString();
List<UserDto> users = objectMapper.readValue(json,
    new TypeReference<List<UserDto>>() {});
assertEquals(2, users.size());
```

> **На собеседовании:** покажите, что знаете три основных метода: `perform` (выполнение), `andExpect` (проверка), `andReturn` (получение результата). Частая ошибка — забыть про `jsonPath` и пытаться десериализовать ответ вручную для каждой проверки.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как тестировать сервисный слой с помощью MockitoExtension?
<!-- grade: junior -->

Для юнит-тестирования сервисного слоя не нужен контекст Spring. Используется `@ExtendWith(MockitoExtension.class)` совместно с `@Mock` и `@InjectMocks`.

- `@Mock` — создаёт мок-объект для зависимости.
- `@InjectMocks` — создаёт экземпляр тестируемого класса и внедряет в него все моки.

<details><summary>Полный пример теста сервиса</summary>

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @Mock
    private EmailService emailService;

    @Mock
    private PasswordEncoder passwordEncoder;

    @InjectMocks
    private UserServiceImpl userService;

    @Test
    void shouldCreateUserSuccessfully() {
        // Arrange
        UserDto dto = new UserDto("Иван", "ivan@example.com", "password");
        when(passwordEncoder.encode("password")).thenReturn("encodedPassword");
        when(userRepository.save(any(User.class))).thenAnswer(invocation -> {
            User user = invocation.getArgument(0);
            user.setId(1L);
            return user;
        });

        // Act
        User result = userService.create(dto);

        // Assert
        assertNotNull(result.getId());
        assertEquals("Иван", result.getName());
        assertEquals("encodedPassword", result.getPassword());

        verify(userRepository).save(any(User.class));
        verify(emailService).sendWelcomeEmail("ivan@example.com");
    }

    @Test
    void shouldThrowExceptionWhenUserNotFound() {
        when(userRepository.findById(999L)).thenReturn(Optional.empty());

        assertThrows(UserNotFoundException.class,
            () -> userService.findById(999L));

        verify(userRepository).findById(999L);
        verifyNoInteractions(emailService);
    }

    @Test
    void shouldUpdateUserName() {
        User existing = new User(1L, "Иван", "ivan@example.com");
        when(userRepository.findById(1L)).thenReturn(Optional.of(existing));
        when(userRepository.save(any(User.class))).thenReturn(existing);

        User result = userService.updateName(1L, "Пётр");

        assertEquals("Пётр", result.getName());

        ArgumentCaptor<User> captor = ArgumentCaptor.forClass(User.class);
        verify(userRepository).save(captor.capture());
        assertEquals("Пётр", captor.getValue().getName());
    }
}
```

</details>

### Преимущества подхода

- **Быстрота** — не поднимается контекст Spring
- **Изоляция** — тестируется только логика сервиса
- **Детерминированность** — нет зависимости от базы данных, сети и т.д.

> **На собеседовании:** интервьюер хочет убедиться, что вы понимаете разницу между `@MockBean` (Spring-контекст) и `@Mock` + `@InjectMocks` (чистый Mockito). Частая ошибка — использовать `@SpringBootTest` для юнит-тестов сервисов вместо `MockitoExtension`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как тестировать репозитории с помощью DataJpaTest?
<!-- grade: middle -->

`@DataJpaTest` — срезовая аннотация для тестирования JPA-репозиториев. Она загружает только компоненты, связанные с JPA (`@Entity`, `@Repository`, `EntityManager`), автоматически настраивает встроенную базу данных и применяет `@Transactional` с откатом после каждого теста.

### Что делает DataJpaTest

- Загружает только JPA-компоненты
- Автоматически настраивает встроенную БД (H2, HSQLDB или Derby)
- Применяет `@Transactional` с откатом
- Не загружает `@Service`, `@Controller` и другие бины

<details><summary>Пример теста репозитория</summary>

```java
@DataJpaTest
class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private TestEntityManager entityManager;

    @Test
    void shouldFindUserByEmail() {
        User user = new User("Иван", "ivan@example.com");
        entityManager.persistAndFlush(user);

        Optional<User> found = userRepository.findByEmail("ivan@example.com");

        assertTrue(found.isPresent());
        assertEquals("Иван", found.get().getName());
    }

    @Test
    void shouldReturnEmptyWhenEmailNotFound() {
        Optional<User> found = userRepository.findByEmail("unknown@example.com");
        assertTrue(found.isEmpty());
    }

    @Test
    void shouldFindUsersByNameContaining() {
        entityManager.persist(new User("Иванов Иван", "ivan@example.com"));
        entityManager.persist(new User("Иванова Мария", "maria@example.com"));
        entityManager.persist(new User("Петров Пётр", "petr@example.com"));
        entityManager.flush();

        List<User> result = userRepository.findByNameContaining("Иванов");

        assertEquals(2, result.size());
    }

    @Test
    void shouldSaveAndRetrieveUser() {
        User user = new User("Тест", "test@example.com");
        User saved = userRepository.save(user);

        entityManager.clear(); // очищаем кэш первого уровня

        User found = userRepository.findById(saved.getId()).orElseThrow();
        assertEquals("Тест", found.getName());
    }
}
```

</details>

### Использование реальной БД вместо встроенной

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class UserRepositoryRealDbTest {
    // Использует реальную БД из application.properties/yml
}
```

Зависимость H2 для тестов:

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope>
</dependency>
```

> **На собеседовании:** важно объяснить, что `@DataJpaTest` — это slice-тест, который поднимает только JPA-слой, а не весь контекст. Частая ошибка — не упомянуть автоматический rollback и `TestEntityManager`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое Testcontainers и зачем они нужны?
<!-- grade: middle -->

Testcontainers — библиотека, позволяющая запускать Docker-контейнеры прямо из тестов. Она решает проблему расхождения между тестовой (H2) и боевой (PostgreSQL, MySQL) базами данных, обеспечивая тестирование на реальной СУБД.

### Зачем нужны

- H2 не поддерживает все возможности PostgreSQL (jsonb, оконные функции, специфичные типы)
- Тесты на реальной БД дают большую уверенность в коде
- Контейнеры автоматически создаются и уничтожаются

### Подключение зависимостей

```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>junit-jupiter</artifactId>
    <scope>test</scope>
</dependency>
```

### Пример с PostgreSQL

<details><summary>Пример кода</summary>

```java
@SpringBootTest
@Testcontainers
class UserServiceIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15")
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
        User user = userService.create(new UserDto("Иван", "ivan@example.com"));

        assertNotNull(user.getId());
        assertEquals("Иван", userService.findById(user.getId()).getName());
    }
}
```

</details>

### Базовый абстрактный класс для переиспользования контейнера

<details><summary>Пример кода</summary>

```java
@Testcontainers
public abstract class AbstractPostgresTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15")
        .withDatabaseName("testdb")
        .withUsername("test")
        .withPassword("test");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
}

@SpringBootTest
class OrderServiceTest extends AbstractPostgresTest {
    // Переиспользует контейнер PostgreSQL
}
```

</details>

> **На собеседовании:** интервьюер хочет услышать, зачем Testcontainers нужны вместо H2 и как подключить контейнер через `@DynamicPropertySource`. Частая ошибка — не упомянуть абстрактный базовый класс для переиспользования контейнера между тестами.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как использовать Sql и SqlGroup для подготовки тестовых данных?
<!-- grade: middle -->

`@Sql` — аннотация Spring Test, позволяющая выполнять SQL-скрипты до или после теста для подготовки и очистки тестовых данных.

### Базовое использование

```java
@SpringBootTest
class UserServiceSqlTest {

    @Autowired
    private UserService userService;

    @Test
    @Sql("/test-data/users.sql")
    void shouldFindPreparedUsers() {
        List<User> users = userService.findAll();
        assertEquals(3, users.size());
    }

    @Test
    @Sql(
        scripts = "/test-data/cleanup.sql",
        executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD
    )
    void shouldCleanUpAfterTest() {
        // ...
    }
}
```

### SqlGroup — группировка скриптов

<details><summary>Пример кода</summary>

```java
@Test
@SqlGroup({
    @Sql(
        scripts = "/test-data/init-schema.sql",
        executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD
    ),
    @Sql(
        scripts = "/test-data/insert-users.sql",
        executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD
    ),
    @Sql(
        scripts = "/test-data/cleanup.sql",
        executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD
    )
})
void shouldWorkWithPreparedData() {
    // данные из обоих скриптов доступны
}
```

</details>

### Инлайн SQL-выражения

```java
@Test
@Sql(statements = {
    "INSERT INTO users (name, email) VALUES ('Тест', 'test@example.com')",
    "INSERT INTO users (name, email) VALUES ('Тест2', 'test2@example.com')"
})
void shouldWorkWithInlineSql() {
    assertEquals(2, userService.findAll().size());
}
```

`@Sql` можно применять на уровне класса — тогда скрипт будет выполняться перед каждым тестовым методом.

> **На собеседовании:** важно знать про `executionPhase` (BEFORE/AFTER) и возможность инлайн SQL через `statements`. Частая ошибка — забыть про очистку данных после теста при использовании `RANDOM_PORT` (где `@Transactional` rollback не работает).

[к оглавлению](#тестирование-в-spring-boot)

---

## Как использовать TestRestTemplate для интеграционных тестов?
<!-- grade: middle -->

`TestRestTemplate` — обёртка над `RestTemplate`, предназначенная для интеграционных тестов с реальным HTTP-сервером. Работает только с `webEnvironment = RANDOM_PORT` или `DEFINED_PORT`.

### Отличия от обычного RestTemplate

- Автоматически настраивает базовый URL (включая порт)
- Не бросает исключения при HTTP-ошибках (4xx, 5xx) — возвращает `ResponseEntity`
- Поддерживает Basic-аутентификацию из коробки

<details><summary>Полный пример интеграционного теста</summary>

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserControllerIntegrationTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @LocalServerPort
    private int port;

    @Test
    void shouldGetAllUsers() {
        ResponseEntity<List<UserDto>> response = restTemplate.exchange(
            "/api/users",
            HttpMethod.GET,
            null,
            new ParameterizedTypeReference<List<UserDto>>() {}
        );

        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertNotNull(response.getBody());
    }

    @Test
    void shouldCreateUser() {
        UserDto newUser = new UserDto("Иван", "ivan@example.com");

        ResponseEntity<UserDto> response = restTemplate.postForEntity(
            "/api/users",
            newUser,
            UserDto.class
        );

        assertEquals(HttpStatus.CREATED, response.getStatusCode());
        assertNotNull(response.getBody().getId());
    }

    @Test
    void shouldReturn404ForUnknownUser() {
        ResponseEntity<String> response = restTemplate.getForEntity(
            "/api/users/999",
            String.class
        );

        // TestRestTemplate не бросает исключение — возвращает ответ с кодом ошибки
        assertEquals(HttpStatus.NOT_FOUND, response.getStatusCode());
    }

    @Test
    void shouldAccessProtectedEndpointWithAuth() {
        TestRestTemplate authenticatedTemplate =
            restTemplate.withBasicAuth("admin", "password");

        ResponseEntity<String> response = authenticatedTemplate.getForEntity(
            "/api/admin/dashboard",
            String.class
        );

        assertEquals(HttpStatus.OK, response.getStatusCode());
    }
}
```

</details>

> **На собеседовании:** ключевой момент — `TestRestTemplate` не бросает исключения на 4xx/5xx, в отличие от `RestTemplate`. Частая ошибка — использовать `TestRestTemplate` с `webEnvironment = MOCK` (он не будет работать без реального сервера).

[к оглавлению](#тестирование-в-spring-boot)

---

## Как использовать WebTestClient для тестирования реактивных и обычных приложений?
<!-- grade: middle -->

`WebTestClient` — клиент для тестирования веб-приложений, поддерживающий как реактивные (WebFlux), так и классические (MVC) приложения. Предоставляет fluent API для выполнения запросов и проверки ответов.

### С SpringBootTest (реальный сервер)

<details><summary>Пример кода</summary>

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserControllerWebClientTest {

    @Autowired
    private WebTestClient webTestClient;

    @Test
    void shouldGetAllUsers() {
        webTestClient.get()
            .uri("/api/users")
            .accept(MediaType.APPLICATION_JSON)
            .exchange()
            .expectStatus().isOk()
            .expectBodyList(UserDto.class)
            .hasSize(3)
            .contains(new UserDto(1L, "Иван"));
    }

    @Test
    void shouldCreateUser() {
        UserDto newUser = new UserDto(null, "Пётр");

        webTestClient.post()
            .uri("/api/users")
            .contentType(MediaType.APPLICATION_JSON)
            .bodyValue(newUser)
            .exchange()
            .expectStatus().isCreated()
            .expectBody(UserDto.class)
            .value(user -> {
                assertNotNull(user.getId());
                assertEquals("Пётр", user.getName());
            });
    }

    @Test
    void shouldReturnNotFound() {
        webTestClient.get()
            .uri("/api/users/999")
            .exchange()
            .expectStatus().isNotFound();
    }
}
```

</details>

### Работа с JSON-телом ответа

```java
@Test
void shouldVerifyResponseBody() {
    webTestClient.get()
        .uri("/api/users/1")
        .exchange()
        .expectStatus().isOk()
        .expectBody()
        .jsonPath("$.name").isEqualTo("Иван")
        .jsonPath("$.email").isNotEmpty()
        .jsonPath("$.id").isNumber();
}
```

### С заголовками и аутентификацией

```java
@Test
void shouldSendHeaders() {
    webTestClient.get()
        .uri("/api/protected")
        .header("Authorization", "Bearer jwt-token-here")
        .exchange()
        .expectStatus().isOk()
        .expectHeader().contentType(MediaType.APPLICATION_JSON)
        .expectHeader().exists("X-Custom-Header");
}
```

> **На собеседовании:** интервьюер хочет услышать, что `WebTestClient` работает и с MVC, и с WebFlux, в отличие от `MockMvc` (только MVC) и `TestRestTemplate` (только реальный сервер). Частая ошибка — думать, что `WebTestClient` только для реактивных приложений.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как настроить профили для тестов?
<!-- grade: junior -->

Spring Boot позволяет использовать отдельные файлы конфигурации для тестового окружения через механизм профилей.

### Создание тестовой конфигурации

Файл `src/test/resources/application-test.yml`:

<details><summary>Пример конфигурации</summary>

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driver-class-name: org.h2.Driver
    username: sa
    password:
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: true
    properties:
      hibernate:
        format_sql: true
  mail:
    host: localhost
    port: 3025

app:
  external-api:
    url: http://localhost:8089/mock-api
  feature:
    notifications-enabled: false
```

</details>

### Активация профиля в тестах

```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceTest {

    @Value("${app.external-api.url}")
    private String apiUrl;

    @Test
    void shouldUseTestConfiguration() {
        assertEquals("http://localhost:8089/mock-api", apiUrl);
    }
}
```

### Несколько профилей

```java
@SpringBootTest
@ActiveProfiles({"test", "local"})
class MultiProfileTest {
    // Загружает application-test.yml и application-local.yml
}
```

### Переопределение отдельных свойств

```java
// Через @SpringBootTest
@SpringBootTest(properties = {
    "app.feature.notifications-enabled=false",
    "spring.jpa.show-sql=true"
})
class PropertyOverrideTest { /* ... */ }

// Через @TestPropertySource
@SpringBootTest
@TestPropertySource(locations = "classpath:custom-test.properties")
class CustomPropertyTest { /* ... */ }

@SpringBootTest
@TestPropertySource(properties = {
    "app.timeout=5000",
    "app.retry-count=3"
})
class InlinePropertyTest { /* ... */ }
```

Spring Boot сначала ищет файлы конфигурации в `src/test/resources`. Если там есть `application.properties` или `application.yml`, он используется вместо файла из `src/main/resources`.

> **На собеседовании:** важно знать приоритет: `@TestPropertySource` > `@SpringBootTest(properties)` > `application-test.yml` > `application.yml`. Частая ошибка — положить `application.properties` в `src/test/resources` и удивляться, что production-конфигурация полностью перекрыта.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как работает Transactional в тестах и что такое автоматический rollback?
<!-- grade: middle -->

Когда тестовый метод или класс аннотирован `@Transactional`, Spring Test автоматически оборачивает каждый тест в транзакцию и откатывает её после завершения. Это гарантирует, что тестовые данные не загрязняют базу для последующих тестов.

> **Аналогия из жизни:** это как черновик в текстовом редакторе — вы пишете, проверяете результат, а потом нажимаете «отменить» (Ctrl+Z), и документ возвращается в исходное состояние.

```java
@SpringBootTest
@Transactional
class UserServiceTransactionalTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @Test
    void shouldCreateUserAndRollback() {
        userService.create(new UserDto("Иван", "ivan@example.com"));
        assertEquals(1, userRepository.count());
        // После теста транзакция откатится — данные не сохранятся
    }

    @Test
    void shouldHaveEmptyDatabase() {
        // Благодаря rollback предыдущего теста, база чиста
        assertEquals(0, userRepository.count());
    }
}
```

### Отключение отката

```java
@Test
@Commit  // или @Rollback(false)
void shouldPersistData() {
    userService.create(new UserDto("Иван", "ivan@example.com"));
    // Данные останутся в базе после теста
}
```

### Важные нюансы

1. `@DataJpaTest` автоматически применяет `@Transactional` — дополнительно аннотировать не нужно.

2. `@Transactional` в тестах не работает с `RANDOM_PORT` и `DEFINED_PORT`, потому что HTTP-запросы выполняются в отдельном потоке, а транзакция привязана к потоку теста:

```java
// Rollback НЕ сработает — запросы идут через реальный HTTP
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Transactional
class WontRollbackTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void dataWillPersist() {
        restTemplate.postForEntity("/api/users",
            new UserDto("Иван"), UserDto.class);
        // Данные останутся в базе!
    }
}
```

3. Для очистки данных при интеграционных тестах с реальным сервером используйте `@Sql` с `AFTER_TEST_METHOD` или `@DirtiesContext`:

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class IntegrationTest {

    @Test
    @Sql(scripts = "/cleanup.sql",
         executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD)
    void integrationTest() {
        // ...
    }
}
```

> **На собеседовании:** ключевой момент — объяснить, почему rollback не работает с `RANDOM_PORT` (разные потоки). Частая ошибка — не знать про эту особенность и ожидать откат при использовании `TestRestTemplate`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое AssertJ и чем он лучше стандартных assertions?
<!-- grade: junior -->

AssertJ — библиотека fluent assertions для Java, предоставляющая читаемый и удобный API для написания проверок. Spring Boot включает AssertJ в стартер `spring-boot-starter-test` по умолчанию.

### Преимущества

- Fluent API — цепочки вызовов читаются как естественный язык
- IDE-автодополнение — методы подсказываются в зависимости от типа объекта
- Информативные сообщения об ошибках — при провале теста сразу видно, что пошло не так

### Сравнение с JUnit assertions

```java
// JUnit 5
assertEquals("Иван", user.getName());
assertTrue(list.contains("элемент"));
assertNotNull(user.getEmail());

// AssertJ — читается естественнее
assertThat(user.getName()).isEqualTo("Иван");
assertThat(list).contains("элемент");
assertThat(user.getEmail()).isNotNull();
```

### Работа со строками

```java
assertThat("Привет, мир!")
    .startsWith("Привет")
    .endsWith("мир!")
    .contains("вет, м")
    .hasSize(13)
    .doesNotContain("ошибка");
```

### Работа с коллекциями

```java
List<User> users = userService.findAll();

assertThat(users)
    .hasSize(3)
    .isNotEmpty()
    .extracting(User::getName)
    .containsExactly("Иван", "Мария", "Пётр")
    .doesNotContain("Неизвестный");

assertThat(users)
    .filteredOn(user -> user.getAge() > 25)
    .hasSize(2)
    .extracting(User::getName, User::getEmail)
    .containsExactly(
        tuple("Иван", "ivan@example.com"),
        tuple("Пётр", "petr@example.com")
    );
```

### Работа с исключениями

```java
assertThatThrownBy(() -> userService.findById(999L))
    .isInstanceOf(UserNotFoundException.class)
    .hasMessageContaining("999")
    .hasNoCause();

assertThatCode(() -> userService.findById(1L))
    .doesNotThrowAnyException();
```

### Работа с Optional

```java
assertThat(userRepository.findByEmail("ivan@example.com"))
    .isPresent()
    .hasValueSatisfying(user -> {
        assertThat(user.getName()).isEqualTo("Иван");
        assertThat(user.getAge()).isGreaterThan(18);
    });
```

### Soft assertions (аналог assertAll в JUnit 5)

```java
@Test
void shouldValidateUser() {
    User user = userService.findById(1L);

    SoftAssertions.assertSoftly(softly -> {
        softly.assertThat(user.getName()).isEqualTo("Иван");
        softly.assertThat(user.getEmail()).contains("@");
        softly.assertThat(user.getAge()).isGreaterThan(0);
        softly.assertThat(user.getRoles()).isNotEmpty();
    });
    // Все ошибки будут собраны и показаны разом
}
```

> **На собеседовании:** достаточно показать знание fluent API и пары фич: `extracting` для коллекций, `assertThatThrownBy` для исключений. Частая ошибка — путать порядок аргументов в JUnit (`assertEquals(expected, actual)`) и в AssertJ (`assertThat(actual).isEqualTo(expected)`).

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое ParameterizedTest и как он используется?
<!-- grade: junior -->

`@ParameterizedTest` — аннотация JUnit 5, позволяющая запускать один и тот же тест с разными входными данными. Это избавляет от дублирования кода при тестировании одной и той же логики с различными параметрами.

### Источники данных

**ValueSource** — простые литеральные значения:

```java
@ParameterizedTest
@ValueSource(strings = {"ivan@example.com", "test@mail.ru", "user@domain.org"})
void shouldAcceptValidEmails(String email) {
    assertTrue(validator.isValidEmail(email));
}
```

**NullSource, EmptySource, NullAndEmptySource** — null и пустые значения:

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = {"  ", "\t", "\n"})
void shouldRejectBlankStrings(String input) {
    assertThrows(IllegalArgumentException.class,
        () -> userService.create(input));
}
```

**EnumSource** — значения перечислений:

```java
@ParameterizedTest
@EnumSource(value = UserRole.class, names = {"ADMIN", "MODERATOR"})
void shouldHaveElevatedPermissions(UserRole role) {
    assertTrue(role.hasElevatedPermissions());
}
```

**CsvSource** — данные в формате CSV:

```java
@ParameterizedTest
@CsvSource({
    "1, 1, 2",
    "2, 3, 5",
    "10, -5, 5",
    "0, 0, 0"
})
void shouldAddNumbers(int a, int b, int expectedSum) {
    assertEquals(expectedSum, calculator.add(a, b));
}
```

**MethodSource** — данные из метода:

<details><summary>Пример кода</summary>

```java
@ParameterizedTest
@MethodSource("provideUsersForValidation")
void shouldValidateUsers(String name, String email, boolean expected) {
    assertEquals(expected, validator.isValid(name, email));
}

static Stream<Arguments> provideUsersForValidation() {
    return Stream.of(
        Arguments.of("Иван", "ivan@example.com", true),
        Arguments.of("", "ivan@example.com", false),
        Arguments.of("Иван", "not-an-email", false),
        Arguments.of(null, null, false)
    );
}
```

</details>

### Кастомное имя теста

```java
@ParameterizedTest(name = "{index}: {0} + {1} = {2}")
@CsvSource({"1,1,2", "2,3,5", "10,-5,5"})
void shouldAddWithDisplayName(int a, int b, int expected) {
    assertEquals(expected, calculator.add(a, b));
}
```

> **На собеседовании:** перечислите 3-4 источника данных (`@ValueSource`, `@CsvSource`, `@MethodSource`, `@EnumSource`) и объясните, когда какой использовать. Частая ошибка — не знать про `@MethodSource` для сложных объектов и пытаться впихнуть всё в `@CsvSource`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое Nested тесты в JUnit 5?
<!-- grade: junior -->

`@Nested` — аннотация JUnit 5, позволяющая создавать вложенные тестовые классы для логической группировки тестов. Это улучшает структуру и читаемость, особенно когда один класс содержит много тестов для разных сценариев.

<details><summary>Полный пример с Nested тестами</summary>

```java
@DisplayName("Тесты UserService")
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserServiceImpl userService;

    @Nested
    @DisplayName("Метод findById")
    class FindById {

        @Test
        @DisplayName("должен вернуть пользователя по существующему id")
        void shouldReturnUserWhenExists() {
            User user = new User(1L, "Иван");
            when(userRepository.findById(1L)).thenReturn(Optional.of(user));

            User result = userService.findById(1L);

            assertEquals("Иван", result.getName());
        }

        @Test
        @DisplayName("должен бросить исключение при несуществующем id")
        void shouldThrowWhenNotFound() {
            when(userRepository.findById(999L)).thenReturn(Optional.empty());

            assertThrows(UserNotFoundException.class,
                () -> userService.findById(999L));
        }
    }

    @Nested
    @DisplayName("Метод create")
    class Create {

        @Test
        @DisplayName("должен создать пользователя с валидными данными")
        void shouldCreateValidUser() {
            UserDto dto = new UserDto("Иван", "ivan@example.com");
            when(userRepository.save(any())).thenAnswer(inv -> {
                User u = inv.getArgument(0);
                u.setId(1L);
                return u;
            });

            User result = userService.create(dto);

            assertNotNull(result.getId());
            verify(userRepository).save(any());
        }

        @Test
        @DisplayName("должен бросить исключение при дублировании email")
        void shouldThrowOnDuplicateEmail() {
            UserDto dto = new UserDto("Иван", "existing@example.com");
            when(userRepository.existsByEmail("existing@example.com"))
                .thenReturn(true);

            assertThrows(DuplicateEmailException.class,
                () -> userService.create(dto));

            verify(userRepository, never()).save(any());
        }
    }
}
```

</details>

### Результат в отчёте

```
Тесты UserService
├── Метод findById
│   ├── должен вернуть пользователя по существующему id
│   └── должен бросить исключение при несуществующем id
├── Метод create
│   ├── должен создать пользователя с валидными данными
│   └── должен бросить исключение при дублировании email
└── Метод delete
    ├── должен удалить существующего пользователя
    └── должен бросить исключение при удалении несуществующего
```

Вложенные классы имеют доступ к полям и мокам внешнего класса. При этом `@BeforeEach` внешнего класса выполняется перед `@BeforeEach` вложенного.

> **На собеседовании:** покажите, что `@Nested` используется для организации тестов по тестируемым методам или сценариям. Частая ошибка — забыть, что вложенные классы не могут иметь `@BeforeAll`/`@AfterAll` без `@TestInstance(Lifecycle.PER_CLASS)`.

[к оглавлению](#тестирование-в-spring-boot)

---

## Что такое TDD и BDD? Чем они отличаются?
<!-- grade: middle -->

TDD (Test-Driven Development) — разработка через тестирование, методология, в которой тесты пишутся до реализации кода. BDD (Behavior-Driven Development) — расширение TDD, фокусирующееся на описании поведения системы с точки зрения бизнеса.

### Цикл TDD (Red-Green-Refactor)

1. **Red** — написать тест, который не проходит (реализации ещё нет)
2. **Green** — написать минимальный код, чтобы тест прошёл
3. **Refactor** — улучшить код, не меняя поведение, убедившись, что тесты проходят

<details><summary>Пример цикла TDD</summary>

```java
// Шаг 1 (Red): пишем тест
@Test
void shouldCalculateDiscount() {
    PriceCalculator calculator = new PriceCalculator();
    BigDecimal result = calculator.calculatePrice(new BigDecimal("1500"));
    assertEquals(new BigDecimal("1350.00"), result);
}

// Шаг 2 (Green): минимальная реализация
public class PriceCalculator {
    public BigDecimal calculatePrice(BigDecimal amount) {
        if (amount.compareTo(new BigDecimal("1000")) > 0) {
            return amount.multiply(new BigDecimal("0.90"))
                         .setScale(2, RoundingMode.HALF_UP);
        }
        return amount;
    }
}

// Шаг 3 (Refactor): улучшаем код
public class PriceCalculator {
    private static final BigDecimal DISCOUNT_THRESHOLD = new BigDecimal("1000");
    private static final BigDecimal DISCOUNT_RATE = new BigDecimal("0.10");

    public BigDecimal calculatePrice(BigDecimal amount) {
        if (amount.compareTo(DISCOUNT_THRESHOLD) > 0) {
            BigDecimal discount = amount.multiply(DISCOUNT_RATE);
            return amount.subtract(discount).setScale(2, RoundingMode.HALF_UP);
        }
        return amount;
    }
}
```

</details>

### BDD — формат Given-When-Then

```java
@Nested
@DisplayName("Оформление заказа")
class OrderPlacement {

    @Test
    @DisplayName("Учитывая корзину с товарами, когда пользователь оформляет заказ, тогда заказ создаётся")
    void givenCartWithItems_whenPlaceOrder_thenOrderIsCreated() {
        // Given (Дано)
        Cart cart = new Cart();
        cart.addItem(new Product("Книга", new BigDecimal("500")));
        cart.addItem(new Product("Ручка", new BigDecimal("50")));

        // When (Когда)
        Order order = orderService.placeOrder(cart, user);

        // Then (Тогда)
        assertThat(order).isNotNull();
        assertThat(order.getStatus()).isEqualTo(OrderStatus.CREATED);
        assertThat(order.getTotalAmount()).isEqualByComparingTo("550");
    }
}
```

### BDD-стиль с Mockito (BDDMockito)

```java
import static org.mockito.BDDMockito.*;

@Test
void shouldProcessPayment() {
    // Given
    given(paymentGateway.charge(any(PaymentRequest.class)))
        .willReturn(new PaymentResponse(true, "txn-123"));

    // When
    PaymentResult result = paymentService.processPayment(order);

    // Then
    then(paymentGateway).should().charge(any(PaymentRequest.class));
    assertThat(result.isSuccessful()).isTrue();
}
```

### Основные различия

| Критерий | TDD | BDD |
|---|---|---|
| Фокус | Корректность кода | Поведение системы |
| Формат | Test -> Implement -> Refactor | Given -> When -> Then |
| Терминология | Test, Assert | Specification, Should, Given/When/Then |
| Целевая аудитория | Разработчики | Разработчики, QA, бизнес |
| Инструменты | JUnit, Mockito | JUnit + BDDMockito, Cucumber, Spock |
| Уровень | Юнит-тесты | Юнит и приёмочные тесты |

> **На собеседовании:** важно объяснить цикл Red-Green-Refactor для TDD и формат Given-When-Then для BDD. Частая ошибка — считать, что BDD заменяет TDD. На самом деле BDD — это расширение TDD с фокусом на бизнес-поведение.

[к оглавлению](#тестирование-в-spring-boot)

---

## Как тестировать Spring Security?
<!-- grade: middle -->

Spring Security предоставляет модуль `spring-security-test` с инструментами для тестирования аутентификации и авторизации.

### Зависимость

```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```

### WithMockUser — эмуляция аутентифицированного пользователя

```java
@WebMvcTest(UserController.class)
class SecuredControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN"})
    void adminShouldAccessAdminEndpoint() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(username = "user", roles = {"USER"})
    void regularUserShouldNotAccessAdminEndpoint() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isForbidden());
    }

    @Test
    void anonymousUserShouldGetUnauthorized() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isUnauthorized());
    }
}
```

### SecurityMockMvcRequestPostProcessors — настройка безопасности в запросе

```java
import static org.springframework.security.test.web.servlet.request
    .SecurityMockMvcRequestPostProcessors.*;

@Test
void shouldAuthenticateWithCsrf() throws Exception {
    mockMvc.perform(post("/api/users")
            .with(csrf())
            .with(user("admin").roles("ADMIN"))
            .contentType(MediaType.APPLICATION_JSON)
            .content("{\"name\": \"Иван\"}"))
        .andExpect(status().isCreated());
}

@Test
void shouldAuthenticateWithHttpBasic() throws Exception {
    mockMvc.perform(get("/api/users")
            .with(httpBasic("user", "password")))
        .andExpect(status().isOk());
}
```

### Кастомная аннотация для повторяющихся сценариев

```java
@Retention(RetentionPolicy.RUNTIME)
@WithMockUser(username = "admin", roles = {"ADMIN"})
public @interface WithAdmin {}

@Retention(RetentionPolicy.RUNTIME)
@WithMockUser(username = "user", roles = {"USER"})
public @interface WithRegularUser {}

// Использование
@Test
@WithAdmin
void shouldAccessAsAdmin() throws Exception {
    mockMvc.perform(get("/api/admin/dashboard"))
        .andExpect(status().isOk());
}
```

### Интеграционный тест с TestRestTemplate

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class SecurityIntegrationTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void shouldReturnUnauthorizedWithoutCredentials() {
        ResponseEntity<String> response =
            restTemplate.getForEntity("/api/users", String.class);
        assertEquals(HttpStatus.UNAUTHORIZED, response.getStatusCode());
    }

    @Test
    void shouldReturnOkWithValidCredentials() {
        ResponseEntity<String> response = restTemplate
            .withBasicAuth("admin", "password")
            .getForEntity("/api/users", String.class);
        assertEquals(HttpStatus.OK, response.getStatusCode());
    }
}
```

> **На собеседовании:** минимум, который нужно знать — `@WithMockUser` и `csrf()`. Частая ошибка — не добавить `.with(csrf())` при тестировании POST/PUT/DELETE-запросов, из-за чего тест падает с 403 Forbidden.

[к оглавлению](#тестирование-в-spring-boot)
