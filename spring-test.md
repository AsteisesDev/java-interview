[Вопросы для собеседования](README.md)

# Тестирование в Spring Boot
+ [Каковы основные отличия JUnit 5 от JUnit 4?](#Каковы-основные-отличия-JUnit-5-от-JUnit-4)
+ [Какие новые аннотации появились в JUnit 5?](#Какие-новые-аннотации-появились-в-JUnit-5)
+ [Какие методы утверждений (assertions) предоставляет JUnit 5?](#Какие-методы-утверждений-assertions-предоставляет-JUnit-5)
+ [Что такое `@SpringBootTest` и как он работает?](#Что-такое-SpringBootTest-и-как-он-работает)
+ [Какие режимы `webEnvironment` существуют в `@SpringBootTest`?](#Какие-режимы-webEnvironment-существуют-в-SpringBootTest)
+ [Как Spring кэширует контекст в тестах и что такое `@DirtiesContext`?](#Как-Spring-кэширует-контекст-в-тестах-и-что-такое-DirtiesContext)
+ [Что такое `@MockBean` и `@SpyBean`?](#Что-такое-MockBean-и-SpyBean)
+ [Каковы основы работы с Mockito?](#Каковы-основы-работы-с-Mockito)
+ [В чём разница между `doReturn/when` и `when/thenReturn` в Mockito?](#В-чём-разница-между-doReturnwhen-и-whenthenReturn-в-Mockito)
+ [Как тестировать контроллеры с помощью `MockMvc` и `@WebMvcTest`?](#Как-тестировать-контроллеры-с-помощью-MockMvc-и-WebMvcTest)
+ [Как выполнять запросы и проверять результаты в `MockMvc`?](#Как-выполнять-запросы-и-проверять-результаты-в-MockMvc)
+ [Как тестировать сервисный слой с помощью `MockitoExtension`?](#Как-тестировать-сервисный-слой-с-помощью-MockitoExtension)
+ [Как тестировать репозитории с помощью `@DataJpaTest`?](#Как-тестировать-репозитории-с-помощью-DataJpaTest)
+ [Что такое Testcontainers и зачем они нужны?](#Что-такое-Testcontainers-и-зачем-они-нужны)
+ [Как использовать `@Sql` и `@SqlGroup` для подготовки тестовых данных?](#Как-использовать-Sql-и-SqlGroup-для-подготовки-тестовых-данных)
+ [Как использовать `TestRestTemplate` для интеграционных тестов?](#Как-использовать-TestRestTemplate-для-интеграционных-тестов)
+ [Как использовать `WebTestClient` для тестирования реактивных и обычных приложений?](#Как-использовать-WebTestClient-для-тестирования-реактивных-и-обычных-приложений)
+ [Как настроить профили для тестов (`application-test.yml`, `@ActiveProfiles`)?](#Как-настроить-профили-для-тестов-application-testyml-ActiveProfiles)
+ [Как работает `@Transactional` в тестах и что такое автоматический rollback?](#Как-работает-Transactional-в-тестах-и-что-такое-автоматический-rollback)
+ [Что такое AssertJ и чем он лучше стандартных assertions?](#Что-такое-AssertJ-и-чем-он-лучше-стандартных-assertions)
+ [Что такое `@ParameterizedTest` и как он используется?](#Что-такое-ParameterizedTest-и-как-он-используется)
+ [Что такое `@Nested` тесты в JUnit 5?](#Что-такое-Nested-тесты-в-JUnit-5)
+ [Что такое TDD и BDD? Чем они отличаются?](#Что-такое-TDD-и-BDD-Чем-они-отличаются)
+ [Как тестировать Spring Security?](#Как-тестировать-Spring-Security)

## Каковы основные отличия JUnit 5 от JUnit 4?

JUnit 5 — это полностью переписанный фреймворк для тестирования, состоящий из трёх модулей:

- **JUnit Platform** — основа для запуска тестовых фреймворков на JVM.
- **JUnit Jupiter** — новая модель программирования и расширений для написания тестов.
- **JUnit Vintage** — обеспечивает обратную совместимость с JUnit 3 и JUnit 4.

Основные отличия:

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

Важно: в JUnit 5 тестовые классы и методы не обязаны быть `public`.

[к оглавлению](#Тестирование-в-Spring-Boot)

## Какие новые аннотации появились в JUnit 5?

JUnit 5 вводит ряд новых аннотаций, расширяющих возможности написания тестов:

- **`@DisplayName`** — задаёт человекочитаемое имя теста, которое отображается в отчётах.
- **`@Nested`** — позволяет создавать вложенные тестовые классы для логической группировки.
- **`@ParameterizedTest`** — параметризованный тест с различными источниками данных.
- **`@RepeatedTest`** — повторяет тест указанное число раз.
- **`@Tag`** — помечает тесты тегами для фильтрации при запуске.
- **`@Disabled`** — отключает тест или тестовый класс.
- **`@Timeout`** — задаёт максимальное время выполнения теста.
- **`@ExtendWith`** — регистрирует расширения (замена `@RunWith` и `@Rule`).
- **`@TempDir`** — инъекция временной директории для файловых тестов.

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Какие методы утверждений (assertions) предоставляет JUnit 5?

JUnit 5 значительно расширил набор утверждений по сравнению с JUnit 4:

**`assertAll`** — группирует несколько проверок и выполняет их все, даже если какая-то из них упадёт. Это позволяет увидеть все ошибки сразу, а не по одной:

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

**`assertThrows`** — проверяет, что выполнение кода бросает ожидаемое исключение (замена `@Test(expected = ...)` из JUnit 4):

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

**`assertTimeout`** — проверяет, что операция выполняется за отведённое время. В отличие от `assertTimeoutPreemptively`, не прерывает выполнение:

```java
@Test
void shouldCompleteInTime() {
    assertTimeout(Duration.ofSeconds(2), () -> {
        // операция, которая должна завершиться за 2 секунды
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

**Другие полезные assertions:**

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое `@SpringBootTest` и как он работает?

`@SpringBootTest` — это аннотация, которая создаёт полный контекст Spring-приложения для интеграционного тестирования. Она является заменой устаревших `@SpringApplicationConfiguration` и `@IntegrationTest`.

Что происходит при использовании `@SpringBootTest`:
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

Можно указать конкретные конфигурационные классы:

```java
@SpringBootTest(classes = {TestConfig.class, UserService.class})
class CustomContextTest {
    // ...
}
```

А также переопределить свойства:

```java
@SpringBootTest(properties = {
    "spring.datasource.url=jdbc:h2:mem:testdb",
    "app.feature.enabled=true"
})
class PropertyOverrideTest {
    // ...
}
```

[к оглавлению](#Тестирование-в-Spring-Boot)

## Какие режимы `webEnvironment` существуют в `@SpringBootTest`?

Параметр `webEnvironment` аннотации `@SpringBootTest` определяет, как будет запущено веб-окружение:

| Режим | Описание |
|---|---|
| `MOCK` (по умолчанию) | Создаёт mock-веб-окружение. Реальный сервер не запускается. Используется совместно с `MockMvc` или `MockWebServiceServer`. |
| `RANDOM_PORT` | Запускает реальный веб-сервер на случайном свободном порту. Идеален для интеграционных тестов с `TestRestTemplate`. |
| `DEFINED_PORT` | Запускает реальный веб-сервер на порту, указанном в конфигурации (`server.port`), по умолчанию 8080. |
| `NONE` | Загружает `ApplicationContext` без какого-либо веб-окружения. Подходит для тестирования сервисов и репозиториев. |

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как Spring кэширует контекст в тестах и что такое `@DirtiesContext`?

Spring TestContext Framework **кэширует** `ApplicationContext` между тестами для повышения производительности. Контекст создаётся один раз и переиспользуется всеми тестами с одинаковой конфигурацией. Ключ кэша формируется на основе:

- Набора конфигурационных классов / XML-файлов.
- Активных профилей (`@ActiveProfiles`).
- Свойств (`properties`).
- Наличия `@MockBean` / `@SpyBean`.
- Других параметров контекста.

Если два тестовых класса имеют одинаковый набор параметров, они разделят один и тот же контекст.

**`@DirtiesContext`** помечает тест, после которого контекст считается «грязным» и должен быть пересоздан. Это необходимо, когда тест изменяет состояние контекста (например, меняет бин, модифицирует синглтон, изменяет данные в кэше).

```java
@SpringBootTest
class ContextCachingTest {

    // Этот тест изменяет состояние бина в контексте
    @Test
    @DirtiesContext
    void testThatModifiesContext() {
        // модификация синглтона или состояния контекста
        cacheService.clearAll();
    }

    // После предыдущего теста контекст будет пересоздан
    @Test
    void testAfterDirtyContext() {
        // работает с чистым контекстом
    }
}
```

`@DirtiesContext` можно применять на уровне класса с различными режимами:

```java
// Пересоздавать контекст после каждого тестового метода
@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_EACH_TEST_METHOD)
@SpringBootTest
class AlwaysFreshContextTest {
    // ...
}

// Пересоздать контекст после всего тестового класса
@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_CLASS)
@SpringBootTest
class DirtyAfterClassTest {
    // ...
}

// Пересоздать контекст до класса (если предыдущий тест мог его загрязнить)
@DirtiesContext(classMode = DirtiesContext.ClassMode.BEFORE_CLASS)
@SpringBootTest
class NeedsFreshContextTest {
    // ...
}
```

> **Важно:** Злоупотребление `@DirtiesContext` значительно замедляет тесты, так как пересоздание контекста — дорогостоящая операция. Используйте его только при необходимости.

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое `@MockBean` и `@SpyBean`?

`@MockBean` и `@SpyBean` — аннотации Spring Boot Test, позволяющие подменять бины в контексте Spring.

**`@MockBean`** заменяет бин в контексте полностью мок-объектом Mockito. Все методы по умолчанию возвращают `null` (или значение по умолчанию для примитивов). Полезно, когда нужно изолировать тестируемый компонент от его зависимостей:

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

**`@SpyBean`** оборачивает реальный бин из контекста шпионом Mockito. Реальные методы продолжают работать, но их можно перехватить и переопределить при необходимости:

```java
@SpringBootTest
class NotificationServiceTest {

    @Autowired
    private NotificationService notificationService;

    @SpyBean
    private AuditService auditService;

    @Test
    void shouldLogAuditEvent() {
        // Реальный метод auditService.log() будет вызван
        notificationService.notifyUser(1L, "Сообщение");

        // Проверяем, что метод был вызван
        verify(auditService).log(eq(1L), contains("Сообщение"));
    }

    @Test
    void shouldHandleAuditFailure() {
        // Переопределяем поведение реального бина
        doThrow(new RuntimeException("Audit unavailable"))
            .when(auditService).log(anyLong(), anyString());

        // Проверяем, что сервис корректно обрабатывает ошибку аудита
        assertDoesNotThrow(
            () -> notificationService.notifyUser(1L, "Сообщение")
        );
    }
}
```

> **Примечание:** Начиная со Spring Boot 3.4 аннотации `@MockBean` и `@SpyBean` объявлены устаревшими (deprecated) в пользу нового механизма `@MockitoBean` и `@MockitoSpyBean`, а также поддержки через `BeanOverride`. Однако они по-прежнему широко используются.

> **Важно:** Каждая уникальная комбинация `@MockBean`/`@SpyBean` создаёт отдельный кэш контекста. Чрезмерное использование разных мок-бинов в разных тестах приведёт к множественным пересозданиям контекста.

[к оглавлению](#Тестирование-в-Spring-Boot)

## Каковы основы работы с Mockito?

Mockito — библиотека для создания мок-объектов в Java-тестах. Она позволяет подменять зависимости и задавать их поведение.

**Создание мока:**

```java
// Программно
UserRepository userRepository = Mockito.mock(UserRepository.class);

// С помощью аннотации (требуется @ExtendWith(MockitoExtension.class))
@Mock
private UserRepository userRepository;
```

**`when/thenReturn`** — задаёт возвращаемое значение при вызове метода:

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

**`when/thenThrow`** — задаёт выбрасываемое исключение:

```java
when(userRepository.findById(999L))
    .thenThrow(new EntityNotFoundException("Пользователь не найден"));
```

**`verify`** — проверяет, что метод был вызван:

```java
@Test
void shouldSaveUser() {
    User user = new User("Иван");
    userService.create(user);

    // Проверяем, что save() был вызван ровно 1 раз
    verify(userRepository, times(1)).save(user);

    // Проверяем, что delete() не вызывался
    verify(userRepository, never()).delete(any());

    // Проверяем, что findAll() вызывался хотя бы раз
    verify(userRepository, atLeastOnce()).findAll();
}
```

**`ArgumentCaptor`** — захватывает аргументы, переданные в мок-метод:

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

**`ArgumentMatchers`** — гибкое сопоставление аргументов:

```java
when(userRepository.findByName(anyString())).thenReturn(List.of(user));
when(userRepository.findById(eq(1L))).thenReturn(Optional.of(user));
when(userRepository.findByAgeGreaterThan(argThat(age -> age > 0)))
    .thenReturn(List.of(user));
```

[к оглавлению](#Тестирование-в-Spring-Boot)

## В чём разница между `doReturn/when` и `when/thenReturn` в Mockito?

Хотя обе конструкции задают поведение мока, между ними есть важные различия:

**`when/thenReturn`** — стандартный подход. Вызывает реальный метод при настройке мока (что может вызвать проблемы для spy-объектов):

```java
when(mock.someMethod()).thenReturn("результат");
```

**`doReturn/when`** — альтернативный подход. Не вызывает реальный метод при настройке:

```java
doReturn("результат").when(mock).someMethod();
```

Ключевые различия:

1. **Spy-объекты:** Для spy при использовании `when/thenReturn` реальный метод будет вызван один раз при настройке. `doReturn/when` этого не делает:

```java
List<String> spyList = spy(new ArrayList<>());

// Опасно! Реальный метод get(0) вызовется на пустом списке → IndexOutOfBoundsException
// when(spyList.get(0)).thenReturn("элемент");

// Безопасно — реальный метод не вызывается при настройке
doReturn("элемент").when(spyList).get(0);
```

2. **Void-методы:** `when/thenReturn` не работает с void-методами, нужно использовать семейство `do*`:

```java
// Не скомпилируется:
// when(mock.voidMethod()).thenThrow(new RuntimeException());

// Правильно:
doThrow(new RuntimeException("ошибка")).when(mock).voidMethod();
doNothing().when(mock).voidMethod();
```

3. **`doAnswer`** — позволяет задать произвольную логику обработки вызова:

```java
doAnswer(invocation -> {
    String arg = invocation.getArgument(0);
    return arg.toUpperCase();
}).when(mock).process(anyString());

// Или с when/thenAnswer:
when(mock.process(anyString())).thenAnswer(invocation -> {
    String arg = invocation.getArgument(0);
    return arg.toUpperCase();
});
```

4. **Цепочка вызовов** — разные результаты при последовательных вызовах:

```java
when(mock.nextValue())
    .thenReturn("первый")
    .thenReturn("второй")
    .thenThrow(new NoSuchElementException());

// Или:
doReturn("первый").doReturn("второй")
    .doThrow(new NoSuchElementException())
    .when(mock).nextValue();
```

| Метод | Когда использовать |
|---|---|
| `when/thenReturn` | Стандартный случай с моками |
| `doReturn/when` | Spy-объекты, void-методы |
| `doThrow/when` | Void-методы, которые должны бросить исключение |
| `doAnswer/when` | Сложная логика, зависящая от аргументов |
| `doNothing/when` | Подавление void-метода у spy |

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как тестировать контроллеры с помощью `MockMvc` и `@WebMvcTest`?

`@WebMvcTest` — это «срезовая» (slice) аннотация, которая поднимает только веб-слой Spring MVC. Она загружает в контекст только компоненты, относящиеся к MVC: контроллеры, `@ControllerAdvice`, `@JsonComponent`, фильтры, `WebMvcConfigurer` и т.д. Сервисы, репозитории и прочие бины **не загружаются** — их нужно мокать.

`MockMvc` — утилита для тестирования контроллеров без запуска реального HTTP-сервера. Запросы обрабатываются через `DispatcherServlet` в имитированном окружении.

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

Разница между `@WebMvcTest` и `@SpringBootTest` + `@AutoConfigureMockMvc`:

| Критерий | `@WebMvcTest` | `@SpringBootTest` + `@AutoConfigureMockMvc` |
|---|---|---|
| Контекст | Только MVC-слой | Полный контекст приложения |
| Скорость | Быстро | Медленнее |
| Зависимости | Нужно мокать через `@MockBean` | Реальные бины доступны |
| Применение | Unit-тесты контроллеров | Интеграционные тесты |

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как выполнять запросы и проверять результаты в `MockMvc`?

`MockMvc` предоставляет мощный DSL для выполнения HTTP-запросов и проверки ответов.

**Выполнение запросов (`perform`):**

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

**Проверка результатов (`andExpect`):**

```java
mockMvc.perform(get("/api/users/1"))
    // Статус ответа
    .andExpect(status().isOk())                 // 200
    .andExpect(status().isCreated())             // 201
    .andExpect(status().isBadRequest())          // 400
    .andExpect(status().isNotFound())            // 404
    .andExpect(status().is(200))                 // произвольный код

    // Заголовки
    .andExpect(header().string("Content-Type", "application/json"))
    .andExpect(header().exists("X-Custom-Header"))

    // Содержимое ответа как строка
    .andExpect(content().string("hello"))
    .andExpect(content().contentType(MediaType.APPLICATION_JSON));
```

**Работа с JSON через `jsonPath`:**

```java
mockMvc.perform(get("/api/users"))
    // Проверка значений
    .andExpect(jsonPath("$[0].name").value("Иван"))
    .andExpect(jsonPath("$[0].id").value(1))

    // Проверка размера массива
    .andExpect(jsonPath("$", hasSize(3)))

    // Проверка существования поля
    .andExpect(jsonPath("$[0].email").exists())
    .andExpect(jsonPath("$[0].password").doesNotExist())

    // Проверка типа
    .andExpect(jsonPath("$[0].id").isNumber())
    .andExpect(jsonPath("$[0].name").isString())
    .andExpect(jsonPath("$").isArray())

    // Hamcrest-матчеры
    .andExpect(jsonPath("$[0].name", containsString("Ив")))
    .andExpect(jsonPath("$[0].age", greaterThan(18)));
```

**Получение и обработка результата:**

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как тестировать сервисный слой с помощью `MockitoExtension`?

Для юнит-тестирования сервисного слоя не нужен контекст Spring. Вместо этого используется `@ExtendWith(MockitoExtension.class)` совместно с `@Mock` и `@InjectMocks`.

- **`@Mock`** — создаёт мок-объект для зависимости.
- **`@InjectMocks`** — создаёт экземпляр тестируемого класса и внедряет в него все моки.

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

Преимущества такого подхода:
- **Быстрота** — не поднимается контекст Spring.
- **Изоляция** — тестируется только логика сервиса.
- **Детерминированность** — нет зависимости от базы данных, сети и т.д.

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как тестировать репозитории с помощью `@DataJpaTest`?

`@DataJpaTest` — срезовая аннотация для тестирования JPA-репозиториев. Она:

- Загружает только компоненты, связанные с JPA (`@Entity`, `@Repository`, `EntityManager`).
- Автоматически настраивает встроенную базу данных (H2, HSQLDB или Derby).
- Применяет `@Transactional` с откатом после каждого теста.
- Не загружает `@Service`, `@Controller` и другие бины.

```java
@DataJpaTest
class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private TestEntityManager entityManager;

    @Test
    void shouldFindUserByEmail() {
        // Arrange — сохраняем через TestEntityManager
        User user = new User("Иван", "ivan@example.com");
        entityManager.persistAndFlush(user);

        // Act
        Optional<User> found = userRepository.findByEmail("ivan@example.com");

        // Assert
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

Для использования реальной базы данных вместо встроенной:

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class UserRepositoryRealDbTest {
    // Использует реальную БД из application.properties/yml
}
```

Добавление зависимости H2 для тестов (в `pom.xml`):

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope>
</dependency>
```

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое Testcontainers и зачем они нужны?

**Testcontainers** — библиотека, позволяющая запускать Docker-контейнеры прямо из тестов. Она решает проблему различия между тестовой (H2) и боевой (PostgreSQL, MySQL и др.) базами данных, обеспечивая тестирование на реальной СУБД.

Зачем нужны:
- H2 не поддерживает все возможности PostgreSQL (jsonb, оконные функции, специфичные типы).
- Тесты на реальной БД дают бОльшую уверенность в коде.
- Контейнеры автоматически создаются и уничтожаются.

**Подключение зависимостей:**

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

**Пример с PostgreSQL:**

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

**Использование с `@DataJpaTest`:**

```java
@DataJpaTest
@Testcontainers
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class UserRepositoryPostgresTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    private UserRepository userRepository;

    @Test
    void shouldUseJsonbColumn() {
        // Тестируем PostgreSQL-специфичную функциональность
        User user = new User("Иван", Map.of("role", "admin"));
        userRepository.save(user);

        List<User> admins = userRepository.findByMetadataRole("admin");
        assertEquals(1, admins.size());
    }
}
```

**Базовый абстрактный класс для переиспользования контейнера:**

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как использовать `@Sql` и `@SqlGroup` для подготовки тестовых данных?

`@Sql` — аннотация Spring Test, позволяющая выполнять SQL-скрипты до или после теста для подготовки и очистки тестовых данных.

```java
@SpringBootTest
class UserServiceSqlTest {

    @Autowired
    private UserService userService;

    // Выполняет скрипт перед тестом
    @Test
    @Sql("/test-data/users.sql")
    void shouldFindPreparedUsers() {
        List<User> users = userService.findAll();
        assertEquals(3, users.size());
    }

    // С настройками выполнения
    @Test
    @Sql(
        scripts = "/test-data/users.sql",
        config = @SqlConfig(
            encoding = "UTF-8",
            transactionMode = SqlConfig.TransactionMode.ISOLATED
        )
    )
    void shouldFindUsersWithConfig() {
        // ...
    }

    // Выполнение скрипта после теста (очистка)
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

**`@SqlGroup`** — группирует несколько `@Sql` аннотаций:

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

**Пример SQL-файла** (`src/test/resources/test-data/users.sql`):

```sql
INSERT INTO users (id, name, email) VALUES (1, 'Иван', 'ivan@example.com');
INSERT INTO users (id, name, email) VALUES (2, 'Мария', 'maria@example.com');
INSERT INTO users (id, name, email) VALUES (3, 'Пётр', 'petr@example.com');
```

**Инлайн SQL-выражения:**

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как использовать `TestRestTemplate` для интеграционных тестов?

`TestRestTemplate` — обёртка над `RestTemplate`, предназначенная для интеграционных тестов с реальным HTTP-сервером. Работает только с `webEnvironment = RANDOM_PORT` или `DEFINED_PORT`.

Отличия от обычного `RestTemplate`:
- Автоматически настраивает базовый URL (включая порт).
- Не бросает исключения при HTTP-ошибках (4xx, 5xx) — вместо этого возвращает `ResponseEntity`.
- Поддерживает Basic-аутентификацию из коробки.

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
        assertEquals("Иван", response.getBody().getName());
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
    void shouldUpdateUser() {
        UserDto updatedUser = new UserDto("Обновлённый", "updated@example.com");

        restTemplate.put("/api/users/1", updatedUser);

        ResponseEntity<UserDto> response = restTemplate.getForEntity(
            "/api/users/1",
            UserDto.class
        );

        assertEquals("Обновлённый", response.getBody().getName());
    }

    @Test
    void shouldDeleteUser() {
        restTemplate.delete("/api/users/1");

        ResponseEntity<String> response = restTemplate.getForEntity(
            "/api/users/1",
            String.class
        );

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как использовать `WebTestClient` для тестирования реактивных и обычных приложений?

`WebTestClient` — клиент для тестирования веб-приложений, поддерживающий как реактивные (WebFlux), так и классические (MVC) приложения. Он предоставляет fluent API для выполнения запросов и проверки ответов.

**С `@SpringBootTest` (реальный сервер):**

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

**С `MockMvc` (без реального сервера):**

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerMockMvcWebClientTest {

    @Autowired
    private WebTestClient webTestClient;

    // WebTestClient можно использовать вместе с MockMvc,
    // добавив зависимость spring-webflux в тестовый scope
}
```

**Работа с JSON-телом ответа:**

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

**С заголовками и аутентификацией:**

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как настроить профили для тестов (`application-test.yml`, `@ActiveProfiles`)?

Spring Boot позволяет использовать отдельные файлы конфигурации для тестового окружения. Это реализуется через механизм профилей.

**Создание тестовой конфигурации:**

Файл `src/test/resources/application-test.yml`:

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

**Активация профиля в тестах:**

```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceTest {

    @Autowired
    private UserService userService;

    @Value("${app.external-api.url}")
    private String apiUrl;

    @Test
    void shouldUseTestConfiguration() {
        // apiUrl == "http://localhost:8089/mock-api"
        assertEquals("http://localhost:8089/mock-api", apiUrl);
    }
}
```

**Несколько профилей:**

```java
@SpringBootTest
@ActiveProfiles({"test", "local"})
class MultiProfileTest {
    // Загружает application-test.yml и application-local.yml
}
```

**Файл `application.properties` в `src/test/resources`:**

Spring Boot сначала ищет файлы конфигурации в `src/test/resources`. Если там есть `application.properties` или `application.yml`, он используется **вместо** файла из `src/main/resources`:

```
# src/test/resources/application.properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.jpa.hibernate.ddl-auto=create-drop
```

**Переопределение отдельных свойств через аннотацию:**

```java
@SpringBootTest(properties = {
    "app.feature.notifications-enabled=false",
    "spring.jpa.show-sql=true"
})
class PropertyOverrideTest {
    // ...
}
```

**`@TestPropertySource`** — ещё один способ переопределения свойств:

```java
@SpringBootTest
@TestPropertySource(locations = "classpath:custom-test.properties")
class CustomPropertyTest {
    // ...
}

@SpringBootTest
@TestPropertySource(properties = {
    "app.timeout=5000",
    "app.retry-count=3"
})
class InlinePropertyTest {
    // ...
}
```

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как работает `@Transactional` в тестах и что такое автоматический rollback?

Когда тестовый метод или тестовый класс аннотирован `@Transactional`, Spring Test автоматически оборачивает каждый тест в транзакцию и **откатывает** её после завершения теста. Это гарантирует, что тестовые данные не «загрязняют» базу данных для последующих тестов.

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
        // Данные сохраняются в рамках транзакции
        userService.create(new UserDto("Иван", "ivan@example.com"));

        assertEquals(1, userRepository.count());
        // После теста транзакция откатится — данные не сохранятся в БД
    }

    @Test
    void shouldHaveEmptyDatabase() {
        // Благодаря rollback предыдущего теста, база чиста
        assertEquals(0, userRepository.count());
    }
}
```

**Отключение отката (если данные должны сохраниться):**

```java
@Test
@Commit  // или @Rollback(false)
void shouldPersistData() {
    userService.create(new UserDto("Иван", "ivan@example.com"));
    // Данные останутся в базе после теста
}
```

**Важные нюансы:**

1. `@DataJpaTest` автоматически применяет `@Transactional` — дополнительно аннотировать не нужно.

2. `@Transactional` в тестах **не работает** с `RANDOM_PORT` и `DEFINED_PORT`, потому что HTTP-запросы выполняются в отдельном потоке, а транзакция привязана к потоку теста:

```java
// ⚠️ Rollback НЕ сработает — запросы идут через реальный HTTP
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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое AssertJ и чем он лучше стандартных assertions?

**AssertJ** — библиотека fluent assertions для Java, предоставляющая читаемый и удобный API для написания проверок. Spring Boot включает AssertJ в стартер `spring-boot-starter-test` по умолчанию.

Главные преимущества:
- **Fluent API** — цепочки вызовов читаются как естественный язык.
- **IDE-автодополнение** — методы подсказываются в зависимости от типа объекта.
- **Информативные сообщения об ошибках** — при провале теста сразу видно, что пошло не так.

**Сравнение с JUnit assertions:**

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

**Работа со строками:**

```java
assertThat("Привет, мир!")
    .startsWith("Привет")
    .endsWith("мир!")
    .contains("вет, м")
    .hasSize(13)
    .doesNotContain("ошибка")
    .matches("Привет.*мир!");
```

**Работа с коллекциями:**

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

**Работа с исключениями:**

```java
assertThatThrownBy(() -> userService.findById(999L))
    .isInstanceOf(UserNotFoundException.class)
    .hasMessageContaining("999")
    .hasMessageStartingWith("Пользователь")
    .hasNoCause();

assertThatCode(() -> userService.findById(1L))
    .doesNotThrowAnyException();
```

**Работа с Optional:**

```java
assertThat(userRepository.findByEmail("ivan@example.com"))
    .isPresent()
    .hasValueSatisfying(user -> {
        assertThat(user.getName()).isEqualTo("Иван");
        assertThat(user.getAge()).isGreaterThan(18);
    });

assertThat(userRepository.findByEmail("unknown@example.com"))
    .isEmpty();
```

**Работа с числами и датами:**

```java
assertThat(user.getAge())
    .isPositive()
    .isGreaterThanOrEqualTo(18)
    .isLessThan(100)
    .isBetween(18, 65);

assertThat(user.getCreatedAt())
    .isNotNull()
    .isBefore(LocalDateTime.now())
    .isAfter(LocalDateTime.of(2020, 1, 1, 0, 0));
```

**Soft assertions** (аналог `assertAll` в JUnit 5):

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

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое `@ParameterizedTest` и как он используется?

`@ParameterizedTest` — аннотация JUnit 5, позволяющая запускать один и тот же тест с разными входными данными. Это избавляет от дублирования кода при тестировании одной и той же логики с различными параметрами.

**Источники данных:**

**`@ValueSource`** — простые литеральные значения:

```java
@ParameterizedTest
@ValueSource(strings = {"ivan@example.com", "test@mail.ru", "user@domain.org"})
void shouldAcceptValidEmails(String email) {
    assertTrue(validator.isValidEmail(email));
}

@ParameterizedTest
@ValueSource(ints = {1, 5, 10, 100})
void shouldBePositive(int number) {
    assertTrue(number > 0);
}
```

**`@NullSource`, `@EmptySource`, `@NullAndEmptySource`** — null и пустые значения:

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = {"  ", "\t", "\n"})
void shouldRejectBlankStrings(String input) {
    assertThrows(IllegalArgumentException.class,
        () -> userService.create(input));
}
```

**`@EnumSource`** — значения перечислений:

```java
@ParameterizedTest
@EnumSource(value = UserRole.class, names = {"ADMIN", "MODERATOR"})
void shouldHaveElevatedPermissions(UserRole role) {
    assertTrue(role.hasElevatedPermissions());
}
```

**`@CsvSource`** — данные в формате CSV:

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

**`@CsvFileSource`** — данные из CSV-файла:

```java
@ParameterizedTest
@CsvFileSource(resources = "/test-data/users.csv", numLinesToSkip = 1)
void shouldValidateUser(String name, String email, boolean expected) {
    assertEquals(expected, validator.isValid(name, email));
}
```

**`@MethodSource`** — данные из метода:

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

**Кастомное имя теста:**

```java
@ParameterizedTest(name = "{index}: {0} + {1} = {2}")
@CsvSource({"1,1,2", "2,3,5", "10,-5,5"})
void shouldAddWithDisplayName(int a, int b, int expected) {
    assertEquals(expected, calculator.add(a, b));
}
```

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое `@Nested` тесты в JUnit 5?

`@Nested` — аннотация JUnit 5, позволяющая создавать вложенные тестовые классы для логической группировки тестов. Это улучшает структуру и читаемость, особенно когда один класс содержит много тестов для разных сценариев.

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

    @Nested
    @DisplayName("Метод delete")
    class Delete {

        @Test
        @DisplayName("должен удалить существующего пользователя")
        void shouldDeleteExistingUser() {
            when(userRepository.existsById(1L)).thenReturn(true);

            userService.delete(1L);

            verify(userRepository).deleteById(1L);
        }

        @Test
        @DisplayName("должен бросить исключение при удалении несуществующего")
        void shouldThrowWhenDeletingNonExistent() {
            when(userRepository.existsById(999L)).thenReturn(false);

            assertThrows(UserNotFoundException.class,
                () -> userService.delete(999L));
        }
    }
}
```

В отчёте о тестах это будет выглядеть как дерево:

```
Тесты UserService
├── Метод findById
│   ├── ✓ должен вернуть пользователя по существующему id
│   └── ✓ должен бросить исключение при несуществующем id
├── Метод create
│   ├── ✓ должен создать пользователя с валидными данными
│   └── ✓ должен бросить исключение при дублировании email
└── Метод delete
    ├── ✓ должен удалить существующего пользователя
    └── ✓ должен бросить исключение при удалении несуществующего
```

Вложенные классы имеют доступ к полям и мокам внешнего класса. При этом `@BeforeEach` внешнего класса выполняется перед `@BeforeEach` вложенного.

[к оглавлению](#Тестирование-в-Spring-Boot)

## Что такое TDD и BDD? Чем они отличаются?

**TDD (Test-Driven Development)** — разработка через тестирование. Методология, в которой тесты пишутся **до** реализации кода.

Цикл TDD (Red-Green-Refactor):
1. **Red** — написать тест, который не проходит (реализации ещё нет).
2. **Green** — написать минимальный код, чтобы тест прошёл.
3. **Refactor** — улучшить код, не меняя поведение, убедившись, что тесты по-прежнему проходят.

```java
// Шаг 1 (Red): пишем тест
@Test
void shouldCalculateDiscount() {
    PriceCalculator calculator = new PriceCalculator();
    // Для заказа свыше 1000 — скидка 10%
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

**BDD (Behavior-Driven Development)** — разработка через поведение. Расширение TDD, фокусирующееся на описании **поведения** системы с точки зрения бизнеса. Тесты описываются в формате **Given-When-Then** (Дано-Когда-Тогда).

```java
// BDD-стиль именования тестов
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

**BDD-стиль с Mockito (BDDMockito):**

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

**Основные различия:**

| Критерий | TDD | BDD |
|---|---|---|
| Фокус | Корректность кода | Поведение системы |
| Формат | Test → Implement → Refactor | Given → When → Then |
| Терминология | Test, Assert | Specification, Should, Given/When/Then |
| Целевая аудитория | Разработчики | Разработчики, QA, бизнес |
| Инструменты | JUnit, Mockito | JUnit + BDDMockito, Cucumber, Spock |
| Уровень | Юнит-тесты | Юнит и приёмочные тесты |

[к оглавлению](#Тестирование-в-Spring-Boot)

## Как тестировать Spring Security?

Spring Security предоставляет модуль `spring-security-test` с инструментами для тестирования аутентификации и авторизации.

**Зависимость:**

```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```

**`@WithMockUser`** — эмулирует аутентифицированного пользователя:

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

**`SecurityMockMvcRequestPostProcessors`** — настройка безопасности в запросе:

```java
import static org.springframework.security.test.web.servlet.request.SecurityMockMvcRequestPostProcessors.*;

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

**Кастомная аннотация для повторяющихся сценариев:**

```java
@Retention(RetentionPolicy.RUNTIME)
@WithMockUser(username = "admin", roles = {"ADMIN"})
public @interface WithAdmin {
}

@Retention(RetentionPolicy.RUNTIME)
@WithMockUser(username = "user", roles = {"USER"})
public @interface WithRegularUser {
}

// Использование
@Test
@WithAdmin
void shouldAccessAsAdmin() throws Exception {
    mockMvc.perform(get("/api/admin/dashboard"))
        .andExpect(status().isOk());
}
```

**Тестирование с JWT (через `@WithMockUser` или кастомный SecurityContext):**

```java
@Test
@WithMockUser(username = "user@example.com", authorities = {"SCOPE_read", "SCOPE_write"})
void shouldAccessResourceWithScopes() throws Exception {
    mockMvc.perform(get("/api/resources"))
        .andExpect(status().isOk());
}
```

**Интеграционный тест с `TestRestTemplate`:**

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

[к оглавлению](#Тестирование-в-Spring-Boot)
