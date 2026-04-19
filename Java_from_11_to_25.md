[Вопросы для собеседования](README.md)

# Новое в Java: от 11 до 25
+ [Какие ключевые нововведения появились в Java 11?](#Какие-ключевые-нововведения-появились-в-Java-11)
+ [Что такое HttpClient API в Java 11?](#Что-такое-HttpClient-API-в-Java-11)
+ [Новые методы String в Java 11](#Новые-методы-String-в-Java-11)
+ [Var в лямбда-выражениях (Java 11)](#Var-в-лямбда-выражениях-Java-11)
+ [Запуск .java файлов без компиляции](#Запуск-java-файлов-без-компиляции)
+ [Какие ключевые нововведения появились в Java 17?](#Какие-ключевые-нововведения-появились-в-Java-17)
+ [Что такое Sealed Classes и Sealed Interfaces?](#Что-такое-Sealed-Classes-и-Sealed-Interfaces)
+ [Pattern Matching для instanceof](#Pattern-Matching-для-instanceof)
+ [Records — что это и зачем?](#Records--что-это-и-зачем)
+ [Text Blocks (многострочные строки)](#Text-Blocks-многострочные-строки)
+ [Switch Expressions](#Switch-Expressions)
+ [Удалённые и устаревшие API в Java 17](#Удалённые-и-устаревшие-API-в-Java-17)
+ [Какие ключевые нововведения появились в Java 21?](#Какие-ключевые-нововведения-появились-в-Java-21)
+ [Что такое Virtual Threads (Project Loom)?](#Что-такое-Virtual-Threads-Project-Loom)
+ [Structured Concurrency — что это и зачем?](#Structured-Concurrency--что-это-и-зачем)
+ [Scoped Values — замена ThreadLocal?](#Scoped-Values--замена-ThreadLocal)
+ [Pattern Matching для switch](#Pattern-Matching-для-switch)
+ [Record Patterns (деконструкция записей)](#Record-Patterns-деконструкция-записей)
+ [Sequenced Collections](#Sequenced-Collections)
+ [Какие ключевые нововведения в Java 25?](#Какие-ключевые-нововведения-в-Java-25)
+ [Flexible Constructor Bodies](#Flexible-Constructor-Bodies)
+ [Module Import Declarations и упрощённый импорт](#Module-Import-Declarations-и-упрощённый-импорт)
+ [Primitive Types in Patterns и instanceof](#Primitive-Types-in-Patterns-и-instanceof)
+ [Что нового в Stream API от Java 11 до Java 25?](#Что-нового-в-Stream-API-от-Java-11-до-Java-25)
+ [Какую версию Java выбрать для нового проекта в 2026 году?](#Какую-версию-Java-выбрать-для-нового-проекта-в-2026-году)
+ [Как мигрировать проект с Java 8/11 на Java 21/25?](#Как-мигрировать-проект-с-Java-811-на-Java-2125)
+ [Эволюция null-safety в Java](#Эволюция-null-safety-в-Java)

---

# Java 11 (LTS, сентябрь 2018)

## Какие ключевые нововведения появились в Java 11?

Java 11 — первый LTS-релиз после Java 8 в новой модели релизов (каждые 6 месяцев). Многие проекты мигрировали напрямую с Java 8 на Java 11.

**Основные нововведения:**

| Категория | Нововведение |
|-----------|-------------|
| API | HttpClient API (стандартный HTTP/2 клиент) |
| Язык | `var` в лямбда-параметрах |
| API | Новые методы `String`: `isBlank()`, `lines()`, `strip()`, `repeat()` |
| API | `Optional.isEmpty()` |
| API | `Files.readString()`, `Files.writeString()` |
| Запуск | Запуск `.java` файлов без явной компиляции |
| Удалено | Java EE модули (JAXB, JAX-WS, CORBA) |
| Удалено | JavaFX (вынесен в отдельный проект) |
| GC | Epsilon GC (no-op сборщик), ZGC (экспериментальный) |
| Удалено | Nashorn JavaScript Engine (deprecated) |

**Новые методы коллекций и файлов:**

```java
// Collection.toArray() с IntFunction
List<String> list = List.of("a", "b", "c");
String[] array = list.toArray(String[]::new); // вместо list.toArray(new String[0])

// Files — чтение/запись строк
String content = Files.readString(Path.of("file.txt"));
Files.writeString(Path.of("output.txt"), "Hello Java 11");

// Optional.isEmpty() — инверсия isPresent()
Optional<String> opt = Optional.empty();
if (opt.isEmpty()) { ... } // вместо !opt.isPresent()

// Predicate.not() — инверсия предиката
List<String> nonBlank = lines.stream()
    .filter(Predicate.not(String::isBlank))
    .toList();
```

### Важное
- Java 11 — LTS, стала де-факто минимальной версией для enterprise-проектов
- Удалены Java EE модули — нужны отдельные зависимости (jakarta.xml.bind, jakarta.xml.ws)
- `var` появился в Java 10, в Java 11 расширен на лямбда-параметры
- HttpClient API — полноценная замена `HttpURLConnection`

### Частые ошибки
- **Миграция с Java 8 без проверки удалённых модулей** — JAXB, JAX-WS больше не в JDK; нужно добавить зависимости
- **Путать `strip()` и `trim()`** — `strip()` корректно обрабатывает Unicode-пробелы, `trim()` — только ASCII
- **Не обновлять библиотеки при миграции** — многие библиотеки (Lombok, Mockito, ASM) требуют обновления для Java 11+

### Как используется в 2026
- Java 11 end-of-life для бесплатной поддержки у большинства вендоров
- Рекомендуется миграция на Java 21 или 25; новые проекты на Java 11 не стартуют
- Многие legacy-проекты всё ещё на Java 11, постепенно мигрируют

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Что такое HttpClient API в Java 11?

**HttpClient** — стандартный HTTP-клиент в JDK, поддерживающий HTTP/1.1 и HTTP/2, синхронные и асинхронные запросы. Заменяет устаревший `HttpURLConnection`.

```java
// Создание клиента (рекомендуется переиспользовать)
HttpClient client = HttpClient.newBuilder()
    .version(HttpClient.Version.HTTP_2)
    .connectTimeout(Duration.ofSeconds(10))
    .followRedirects(HttpClient.Redirect.NORMAL)
    .build();

// GET-запрос (синхронный)
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/users/1"))
    .header("Accept", "application/json")
    .GET()
    .build();

HttpResponse<String> response = client.send(request,
    HttpResponse.BodyHandlers.ofString());

System.out.println(response.statusCode());  // 200
System.out.println(response.body());        // JSON

// POST-запрос
HttpRequest postRequest = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/users"))
    .header("Content-Type", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(
        """
        {"name": "John", "email": "john@example.com"}
        """))
    .build();

// Асинхронный запрос (возвращает CompletableFuture)
CompletableFuture<HttpResponse<String>> future =
    client.sendAsync(request, HttpResponse.BodyHandlers.ofString());

future.thenApply(HttpResponse::body)
      .thenAccept(System.out::println)
      .join();
```

**Чтение ответа в разные типы:**

```java
// В строку
HttpResponse<String> str = client.send(req, BodyHandlers.ofString());

// В файл
HttpResponse<Path> file = client.send(req, BodyHandlers.ofFile(Path.of("out.json")));

// В InputStream
HttpResponse<InputStream> stream = client.send(req, BodyHandlers.ofInputStream());

// Отбросить тело
HttpResponse<Void> discarded = client.send(req, BodyHandlers.discarding());
```

### Важное
- `HttpClient` потокобезопасный — создавайте один экземпляр и переиспользуйте
- Нативная поддержка HTTP/2 с автоматическим fallback на HTTP/1.1
- Поддерживает синхронный (`send`) и асинхронный (`sendAsync`) режимы
- `BodyHandlers` / `BodyPublishers` — типизированное чтение/запись тела запроса

### Частые ошибки
- **Создание нового `HttpClient` на каждый запрос** — дорого; переиспользуйте экземпляр
- **Не обрабатывать HTTP-статусы** — `send()` не бросает исключение для 4xx/5xx; проверяйте `statusCode()`
- **Блокирующий `join()` на `sendAsync()`** — теряется смысл асинхронности; используйте `thenApply`/`thenAccept`

### Как используется в 2026
- В Spring-приложениях вместо HttpClient чаще используют `RestClient` (Spring 6.1+) или `WebClient`
- HttpClient из JDK полезен для standalone-приложений и библиотек без Spring-зависимостей
- В микросервисах HTTP-клиент обычно оборачивается в абстракцию с retry, timeout, circuit breaker

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Новые методы String в Java 11

Java 11 добавила несколько полезных методов к классу `String`:

```java
// isBlank() — проверка на пустоту или только пробелы (включая Unicode-пробелы)
"".isBlank();        // true
"   ".isBlank();     // true
" hello ".isBlank(); // false
// Отличие от isEmpty(): "   ".isEmpty() → false, "   ".isBlank() → true

// strip() — удаление пробелов в начале и конце (Unicode-aware)
"  hello  ".strip();        // "hello"
"  hello  ".stripLeading(); // "hello  "
"  hello  ".stripTrailing();// "  hello"
// Отличие от trim(): strip() корректно удаляет Unicode-пробелы (\u2000, \u3000...)

// lines() — разбиение на строки (возвращает Stream<String>)
"line1\nline2\nline3".lines().toList();  // ["line1", "line2", "line3"]
// Распознаёт \n, \r, \r\n

// repeat(int) — повторение строки
"abc".repeat(3);  // "abcabcabc"
"-".repeat(50);   // "---...---" (50 дефисов)
```

### Важное
- `isBlank()` ≠ `isEmpty()`: пустая строка `""` — и blank, и empty; пробельная `"  "` — blank, но не empty
- `strip()` — Unicode-aware версия `trim()`; в новом коде используйте `strip()`
- `lines()` — ленивый Stream, эффективен для многострочных текстов

### Частые ошибки
- **Использовать `trim()` вместо `strip()`** — `trim()` не удаляет Unicode-пробелы (неразрывный пробел `\u00A0`)
- **`repeat(0)`** — возвращает пустую строку, не null; это корректное поведение

### Как используется в 2026
- Эти методы — повседневный инструмент; используются повсеместно
- `isBlank()` заменяет `StringUtils.isBlank()` из Apache Commons в большинстве случаев

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Var в лямбда-выражениях (Java 11)

`var` (введён в Java 10 для локальных переменных) в Java 11 разрешён в параметрах лямбда-выражений. Основная цель — возможность добавлять аннотации к лямбда-параметрам.

```java
// Java 10 — var для локальных переменных
var list = new ArrayList<String>(); // тип выведен: ArrayList<String>
var stream = list.stream();         // тип выведен: Stream<String>

// Java 11 — var в лямбда-параметрах
// Без var (и так работает):
list.stream()
    .filter(s -> s.length() > 3)
    .toList();

// С var — позволяет добавлять аннотации:
list.stream()
    .filter((@NotNull var s) -> s.length() > 3)
    .toList();

// Все параметры должны быть или с var, или без:
// (var x, y) → ошибка компиляции
// (var x, var y) → OK
```

### Важное
- `var` — не ключевое слово, а зарезервированное имя типа (можно иметь переменную `var var = 1`)
- `var` не меняет тип — тип выводится компилятором, рантайм не затронут
- Основная мотивация для `var` в лямбдах — аннотации (`@NonNull`, `@Nullable`)

### Частые ошибки
- **Злоупотребление `var`** — `var result = process()` — непонятно, что вернёт; используйте `var` когда тип очевиден из правой части
- **`var` для полей класса** — не поддерживается; `var` только для локальных переменных и лямбда-параметров
- **Смешивание `var` и явных типов в лямбде** — `(var x, String y)` → ошибка компиляции

### Как используется в 2026
- `var` широко используется для локальных переменных, особенно с дженериками
- Стандарты кодирования: `var` допустим, когда тип очевиден; явный тип — когда нет
- IntelliJ IDEA подсвечивает выведенный тип при наведении

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Запуск .java файлов без компиляции

Java 11 позволяет запускать single-file Java-программы напрямую без явного вызова `javac`:

```bash
# До Java 11 — два шага:
javac HelloWorld.java
java HelloWorld

# Java 11+ — один шаг:
java HelloWorld.java

# Или как скрипт (Unix):
#!/usr/bin/java --source 17
public class Script {
    public static void main(String[] args) {
        System.out.println("Hello from Java script!");
    }
}
```

```bash
chmod +x script.java
./script.java
```

### Важное
- Работает только для **одного** .java файла (single-file source-code program)
- Файл компилируется в памяти и сразу выполняется
- Полезно для скриптов, быстрых экспериментов, обучения

### Частые ошибки
- **Использовать для многофайловых проектов** — не работает; только один файл
- **Передать имя без `.java`** — `java HelloWorld` ищет `.class` файл; для исходника нужно `java HelloWorld.java`

### Как используется в 2026
- Удобно для quick scripts и прототипирования
- JEP 458 (Java 22+) расширил: поддержка нескольких классов в одном файле, implicit `main()`
- JBang — популярный инструмент для Java-скриптов с поддержкой зависимостей

[к оглавлению](#Новое-в-Java-от-11-до-25)

---

# Java 17 (LTS, сентябрь 2021)

## Какие ключевые нововведения появились в Java 17?

Java 17 — второй LTS после Java 11, ставший минимальной версией для Spring Boot 3.x и Spring Framework 6.x.

**Основные нововведения (Java 12–17):**

| Версия | Нововведение |
|--------|-------------|
| Java 12 | Switch Expressions (preview) |
| Java 13 | Text Blocks (preview) |
| Java 14 | Records (preview), Pattern Matching для instanceof (preview), NullPointerException с подробностями |
| Java 15 | Text Blocks (final), Sealed Classes (preview), ZGC (production) |
| Java 16 | Records (final), Pattern Matching для instanceof (final), Stream.toList() |
| Java 17 | Sealed Classes (final), удалён Security Manager, новый macOS rendering pipeline |

```java
// Helpful NullPointerException (Java 14)
var user = getUser();
var street = user.getAddress().getStreet().toUpperCase();
// До Java 14: NullPointerException
// Java 14+: Cannot invoke "Address.getStreet()" because the return value of
//           "User.getAddress()" is null

// Stream.toList() (Java 16) — unmodifiable list
List<String> names = users.stream()
    .map(User::getName)
    .toList(); // вместо .collect(Collectors.toList())
```

### Важное
- Java 17 LTS — минимальная версия для Spring Boot 3.x, Spring 6, Jakarta EE 10
- Records, Sealed Classes, Pattern Matching, Text Blocks, Switch Expressions — все финализированы
- Helpful NullPointerException — показывает, что именно было null в цепочке вызовов
- ZGC стал production-ready — низколатентный GC для больших heap

### Частые ошибки
- **`stream().toList()` vs `stream().collect(Collectors.toList())`** — `toList()` возвращает неизменяемый список; если нужен мутабельный — `collect(Collectors.toList())`
- **Не обновить зависимости при миграции на 17** — Lombok, Mockito, Byte Buddy, ASM требуют обновления

### Как используется в 2026
- Java 17 — актуальная LTS, широко используется в продакшене
- Большинство enterprise-проектов на Java 17 или мигрируют на Java 21
- Новые проекты стартуют на Java 21 или 25

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Что такое Sealed Classes и Sealed Interfaces?

**Sealed Classes** (запечатанные классы) ограничивают набор классов, которые могут наследоваться от данного класса или реализовывать интерфейс. Это позволяет компилятору знать **все** возможные подтипы.

```java
// Запечатанный интерфейс — только перечисленные классы могут его реализовать
public sealed interface Shape
    permits Circle, Rectangle, Triangle {
}

public record Circle(double radius) implements Shape {}
public record Rectangle(double width, double height) implements Shape {}
public final class Triangle implements Shape {
    // final — нельзя наследоваться дальше
    private final double a, b, c;
    // ...
}

// Exhaustive switch (компилятор проверяет полноту)
public double area(Shape shape) {
    return switch (shape) {
        case Circle c    -> Math.PI * c.radius() * c.radius();
        case Rectangle r -> r.width() * r.height();
        case Triangle t  -> calculateTriangleArea(t);
        // default не нужен — компилятор знает все подтипы!
    };
}
```

**Модификаторы наследников:**
- `final` — нельзя наследоваться далее
- `sealed` — можно, но с ограничением (свой `permits`)
- `non-sealed` — снять ограничение, любой может наследоваться

```java
public sealed class Animal permits Dog, Cat, Fish {}
public final class Dog extends Animal {}              // никто не наследует
public sealed class Cat extends Animal permits Persian, Siamese {} // ограниченно
public non-sealed class Fish extends Animal {}         // открыт для наследования
```

### Важное
- Sealed classes ограничивают иерархию наследования — компилятор знает все подтипы
- В `switch` с sealed типом не нужен `default` — компилятор проверяет exhaustiveness
- Наследники должны быть `final`, `sealed` или `non-sealed`
- Все наследники должны быть в том же модуле (или пакете, если не в модуле)

### Частые ошибки
- **Забыть `permits`** — если наследники в том же файле, `permits` необязателен (Java выведет автоматически)
- **`non-sealed` нарушает гарантии** — если один из подтипов `non-sealed`, exhaustive switch невозможен
- **Sealed + Records** — records implicitly final, поэтому идеальные кандидаты для sealed

### Как используется в 2026
- Sealed classes + Records + Pattern Matching = **алгебраические типы данных** (ADT) в Java
- Широко используется для моделирования доменных событий, состояний, результатов
- Пример: `sealed interface Result<T> permits Success<T>, Failure<T>` — type-safe обработка результатов

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Pattern Matching для instanceof

**Pattern Matching для instanceof** (финализирован в Java 16) устраняет необходимость явного приведения типа после проверки `instanceof`.

```java
// До Java 16 — проверка + приведение:
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toUpperCase());
}

// Java 16+ — pattern variable:
if (obj instanceof String s) {
    System.out.println(s.toUpperCase()); // s уже String
}

// С условием (guarded pattern):
if (obj instanceof String s && s.length() > 5) {
    System.out.println("Long string: " + s);
}

// В отрицании:
if (!(obj instanceof String s)) {
    return; // s недоступна здесь
}
// s доступна здесь (flow scoping)
System.out.println(s.toUpperCase());
```

**Практическое применение — equals():**

```java
// До Java 16:
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Point)) return false;
    Point other = (Point) o;
    return x == other.x && y == other.y;
}

// Java 16+:
@Override
public boolean equals(Object o) {
    return (o instanceof Point other)
        && x == other.x && y == other.y;
}
```

### Важное
- Pattern variable (`s` в `instanceof String s`) доступна в scope, где `instanceof` гарантированно true
- Поддерживает **flow scoping** — переменная доступна после `if`, если поток управления гарантирует истинность
- Работает с `&&` (и pattern variable доступна), но не с `||`
- Компилятор предотвращает shadowing — pattern variable не может скрывать поле класса

### Частые ошибки
- **Pattern variable с `||`** — `if (obj instanceof String s || obj instanceof Integer s)` → ошибка компиляции
- **Shadowing** — если есть поле `s`, pattern variable `s` вызовет ошибку; используйте другое имя
- **Использовать pattern matching в `catch`** — не поддерживается

### Как используется в 2026
- Стандартная практика; старый стиль с явным cast считается устаревшим
- Комбинируется с Pattern Matching для switch (Java 21) для мощной деконструкции типов
- IDE автоматически предлагает рефакторинг старого кода

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Records — что это и зачем?

**Record** — компактный способ объявления классов-носителей неизменяемых данных (data carriers). Компилятор автоматически генерирует конструктор, геттеры, `equals()`, `hashCode()` и `toString()`.

```java
// Record — одна строка вместо ~50 строк boilerplate
public record Point(int x, int y) {}

// Эквивалентный обычный класс:
public final class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() { return x; }     // не getX()!
    public int y() { return y; }

    @Override
    public boolean equals(Object o) { ... }

    @Override
    public int hashCode() { ... }

    @Override
    public String toString() { return "Point[x=" + x + ", y=" + y + "]"; }
}
```

**Использование:**

```java
var p = new Point(10, 20);
int x = p.x();           // accessor (не getX!)
System.out.println(p);   // Point[x=10, y=20]

// Compact constructor — валидация
public record Email(String value) {
    public Email {   // compact canonical constructor
        if (value == null || !value.contains("@")) {
            throw new IllegalArgumentException("Invalid email: " + value);
        }
        value = value.toLowerCase().strip(); // можно переназначить параметр
    }
}

// Record с дополнительными методами
public record Range(int from, int to) {
    public Range {
        if (from > to) throw new IllegalArgumentException();
    }

    public int length() { return to - from; }

    public boolean contains(int value) { return value >= from && value <= to; }
}

// Record может реализовывать интерфейсы
public record UserDto(String name, String email) implements Serializable {}
```

**Ограничения Records:**
- Нельзя наследоваться от другого класса (неявно наследует `java.lang.Record`)
- Нельзя объявлять instance-поля (только компоненты записи)
- Все поля — final и private (неизменяемые)
- Record implicitly final — нельзя наследоваться от record

### Важное
- Record — **immutable data carrier**, не полноценный domain object
- Accessor-методы без `get` префикса: `point.x()`, не `point.getX()`
- Compact constructor — для валидации; присвоение полей происходит автоматически после конструктора
- Records можно использовать как `@Embeddable` в Hibernate 6

### Частые ошибки
- **Мутабельные поля в Record** — `record Data(List<String> items)` — список мутабельный; делайте defensive copy: `this.items = List.copyOf(items)`
- **Record как JPA Entity** — нельзя, Entity требует no-arg constructor и мутабельность; Record можно как DTO или `@Embeddable`
- **Ожидать `getX()` вместо `x()`** — Jackson и другие библиотеки поддерживают record accessors, но legacy-код может ожидать JavaBean-стиль

### Как используется в 2026
- Records — стандарт для DTO, Value Objects, API responses/requests
- Широкая поддержка: Jackson, Spring, Hibernate (Embeddable), MapStruct
- В комбинации с Sealed Interfaces — алгебраические типы данных
- Record Patterns (Java 21) — деконструкция records в switch/instanceof

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Text Blocks (многострочные строки)

**Text Blocks** (финализированы в Java 15) — многострочные строковые литералы, сохраняющие форматирование.

```java
// До Java 15 — конкатенация или StringBuilder
String json = "{\n" +
    "  \"name\": \"John\",\n" +
    "  \"age\": 30\n" +
    "}";

// Java 15+ — Text Block
String json = """
        {
          "name": "John",
          "age": 30
        }
        """;

// SQL
String sql = """
        SELECT u.name, u.email
        FROM users u
        WHERE u.status = 'ACTIVE'
        ORDER BY u.name
        """;

// HTML
String html = """
        <html>
            <body>
                <h1>Hello, %s!</h1>
            </body>
        </html>
        """.formatted("World");
```

**Управление отступами:**

```java
// Отступ определяется позицией закрывающих """
String s1 = """
        Hello"""; // нет завершающего \n

String s2 = """
        Hello
        """; // есть завершающий \n

// Trailing whitespace удаляется; для сохранения используйте \s
String s3 = """
        line with trailing space\s
        """;
```

**Escape-последовательности (Java 14+):**

```java
// \: line continuation (без переноса строки)
String longLine = """
        This is a very long line that \
        continues on the same line""";
// → "This is a very long line that continues on the same line"
```

### Важное
- Text Blocks начинаются с `"""` + перенос строки; на одной строке с `"""` не может быть текста
- Отступ определяется наименее отступлённой строкой или позицией закрывающих `"""`
- Trailing whitespace удаляется автоматически; используйте `\s` для его сохранения
- `formatted()` — замена `String.format()` с удобным синтаксисом

### Частые ошибки
- **Неожиданные отступы** — если закрывающие `"""` не выровнены, результат содержит лишние пробелы
- **Text Block в аннотациях** — не поддерживается в `@Query` до Spring Data 3.x (нужна Java 17+)
- **`\n` внутри Text Block** — Text Block сам добавляет переносы; явный `\n` удвоит перенос

### Как используется в 2026
- Стандарт для встроенных SQL, JSON, HTML, XML, YAML в Java-коде
- Широко используется в `@Query` (Spring Data), тестах, конфигурации
- String Templates (preview в Java 21-22, удалены в 23-24, переработаны) — дальнейшее развитие форматирования строк

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Switch Expressions

**Switch Expressions** (финализированы в Java 14) превращают `switch` из statement в expression (выражение, возвращающее значение).

```java
// Старый switch (statement, fall-through)
String result;
switch (day) {
    case MONDAY:
    case TUESDAY:
        result = "Начало недели";
        break;
    case FRIDAY:
        result = "Пятница!";
        break;
    default:
        result = "Обычный день";
        break;
}

// Новый switch (expression, no fall-through)
String result = switch (day) {
    case MONDAY, TUESDAY -> "Начало недели";
    case FRIDAY          -> "Пятница!";
    default              -> "Обычный день";
};

// Блок с yield (когда нужна логика)
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    default -> {
        String s = day.toString();
        yield s.length(); // yield вместо return в switch expression
    }
};
```

**Exhaustiveness (полнота):**

```java
// Для enum — компилятор проверяет все значения
enum Color { RED, GREEN, BLUE }

String hex = switch (color) {
    case RED   -> "#FF0000";
    case GREEN -> "#00FF00";
    case BLUE  -> "#0000FF";
    // default не нужен — все варианты покрыты
};
```

### Важное
- Switch expression **возвращает значение** и заканчивается `;`
- Стрелка `->` — нет fall-through; запятая для нескольких значений: `case A, B ->`
- `yield` — для возврата значения из блока `{}`
- Для enum и sealed types — exhaustiveness check без `default`

### Частые ошибки
- **Забыть `yield` в блоке** — `return` не работает в switch expression; используйте `yield`
- **Смешивание `->` и `:`** — в одном switch нельзя комбинировать стрелочный и двоеточный стиль
- **`default` для sealed/enum** — скрывает ошибки при добавлении нового варианта; компилятор не предупредит

### Как используется в 2026
- Switch expression — стандартный стиль; старый `switch` с `break` — legacy
- Комбинируется с Pattern Matching (Java 21) для мощных деконструкций
- IDE автоматически предлагает рефакторинг старого `switch` в expression

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Удалённые и устаревшие API в Java 17

| Удалено/Deprecated | Версия | Замена |
|--------------------|--------|--------|
| JAXB (`javax.xml.bind`) | Удалено в Java 11 | `jakarta.xml.bind:jakarta.xml.bind-api` + `org.glassfish.jaxb:jaxb-runtime` |
| JAX-WS (`javax.xml.ws`) | Удалено в Java 11 | `jakarta.xml.ws:jakarta.xml.ws-api` |
| CORBA | Удалено в Java 11 | Нет прямой замены |
| Nashorn JS Engine | Удалено в Java 15 | GraalJS |
| RMI Activation | Удалено в Java 17 | — |
| Security Manager | Deprecated for removal в Java 17 | — |
| Applet API | Deprecated for removal в Java 17 | — |
| `Finalization` (finalize()) | Deprecated for removal в Java 18 | `Cleaner`, try-with-resources |
| `javax.*` пакеты | Переименованы (Jakarta EE 9+) | `jakarta.*` |

**Миграция `javax` → `jakarta`:**

```java
// До (Java EE):
import javax.persistence.Entity;
import javax.servlet.http.HttpServlet;

// После (Jakarta EE):
import jakarta.persistence.Entity;
import jakarta.servlet.http.HttpServlet;
```

### Важное
- Переход `javax.*` → `jakarta.*` — обязательный при миграции на Spring Boot 3.x
- Security Manager удалён — для sandbox-изоляции используйте контейнеры
- `finalize()` deprecated — используйте `try-with-resources` и `Cleaner`

### Частые ошибки
- **Не обновить зависимости при миграции** — `javax.persistence` → `jakarta.persistence` требует обновления Spring Boot, Hibernate, и всех Jakarta EE зависимостей
- **Использовать `finalize()`** — ненадёжный, медленный, deprecated; `AutoCloseable` + try-with-resources

### Как используется в 2026
- Все современные фреймворки используют `jakarta.*`
- Инструменты миграции: OpenRewrite recipes для автоматической замены `javax` → `jakarta`

[к оглавлению](#Новое-в-Java-от-11-до-25)

---

# Java 21 (LTS, сентябрь 2023)

## Какие ключевые нововведения появились в Java 21?

Java 21 — третий LTS, самый значимый релиз со времён Java 8. Ключевая функциональность: Virtual Threads, Pattern Matching для switch, Record Patterns.

| Версия | Нововведение |
|--------|-------------|
| Java 18 | UTF-8 по умолчанию, Simple Web Server |
| Java 19 | Virtual Threads (preview), Structured Concurrency (incubator) |
| Java 20 | Scoped Values (incubator), Record Patterns (preview) |
| Java 21 | **Virtual Threads (final)**, Pattern Matching для switch (final), Record Patterns (final), Sequenced Collections, String Templates (preview) |

```java
// Простой HTTP-сервер (Java 18+, для разработки)
// Запуск: jwebserver --port 8000 --directory /path/to/site

// UTF-8 по умолчанию (Java 18)
// Больше не нужен -Dfile.encoding=UTF-8

// Unnamed patterns (Java 21)
if (obj instanceof Point(var x, _)) { // _ — неиспользуемый компонент
    System.out.println("x = " + x);
}
```

### Важное
- Virtual Threads — революция для серверных приложений; тысячи виртуальных потоков без overhead
- Pattern Matching + Sealed Types — полноценные алгебраические типы данных
- Sequenced Collections — наконец-то `getFirst()`/`getLast()` для List, Set, Map
- Java 21 — рекомендуемая LTS для новых проектов в 2026

### Частые ошибки
- **Путать Java 21 features и preview features** — String Templates были preview в 21, удалены в 23
- **Не использовать Virtual Threads сразу** — Spring Boot 3.2+ поддерживает одной строкой конфига

### Как используется в 2026
- Java 21 — основная версия для новых проектов
- Spring Boot 3.2+ полностью поддерживает Virtual Threads
- Миграция с Java 17 на 21 обычно безболезненна

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Что такое Virtual Threads (Project Loom)?

**Virtual Threads** — лёгкие потоки, управляемые JVM, а не операционной системой. Позволяют создавать миллионы потоков без значительных затрат памяти.

**Проблема Platform Threads:**
- Каждый platform thread = поток ОС ≈ 1 MB stack
- 10 000 потоков ≈ 10 GB памяти только на стеки
- При I/O-блокировке поток простаивает, но занимает ресурсы

**Virtual Threads решают это:**
- Виртуальный поток ≈ несколько KB (вместо 1 MB)
- Миллион виртуальных потоков — нормально
- При I/O-блокировке JVM освобождает carrier thread для другой работы

```java
// Создание Virtual Thread
Thread vThread = Thread.ofVirtual().start(() -> {
    System.out.println("Running in: " + Thread.currentThread());
});

// С именем
Thread.ofVirtual().name("my-vthread").start(() -> {
    // ...
});

// ExecutorService с Virtual Threads
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 100_000; i++) {
        executor.submit(() -> {
            // Каждая задача в своём виртуальном потоке
            Thread.sleep(Duration.ofSeconds(1));
            return fetchData();
        });
    }
} // executor.close() ждёт завершения всех задач

// Spring Boot — одна строка конфигурации
// application.yml:
// spring:
//   threads:
//     virtual:
//       enabled: true
```

**Сравнение:**

| Критерий | Platform Thread | Virtual Thread |
|----------|----------------|----------------|
| Стек | ~1 MB (фиксированный) | ~несколько KB (динамический) |
| Создание | Дорого (syscall) | Дёшево (объект JVM) |
| Количество | Тысячи | Миллионы |
| Планирование | ОС | JVM (ForkJoinPool) |
| Блокирующий I/O | Блокирует поток ОС | Отпускает carrier thread |
| CPU-bound | Эффективен | Не даёт преимущества |

**Когда Virtual Threads полезны:**
- I/O-bound задачи: HTTP-запросы, обращения к БД, файловые операции
- Высокая конкурентность: тысячи одновременных соединений
- Замена реактивного стека для большинства сценариев

**Когда НЕ использовать:**
- CPU-bound вычисления — количество потоков ≤ количество ядер
- Код с `synchronized` блоками, удерживающими блокировку при I/O (pinning)

### Важное
- Virtual Threads — НЕ замена parallelism, а инструмент для scalable concurrency
- Стиль программирования: **thread-per-request** (один поток на задачу) — простой и масштабируемый
- Не используйте пулы для Virtual Threads — создавайте новый на каждую задачу
- **Pinning** — если виртуальный поток блокируется внутри `synchronized`, он «закрепляется» за carrier thread; используйте `ReentrantLock`

### Частые ошибки
- **Пул Virtual Threads** — `Executors.newFixedThreadPool()` с Virtual Threads бессмысленно; используйте `newVirtualThreadPerTaskExecutor()`
- **ThreadLocal с Virtual Threads** — при миллионе потоков ThreadLocal потребляет огромное количество памяти; используйте ScopedValues
- **`synchronized` с I/O** — pinning; замените на `ReentrantLock`
- **Ожидать ускорение CPU-bound задач** — Virtual Threads не добавляют ядра; для CPU используйте `ForkJoinPool`

### Как используется в 2026
- Spring Boot 3.2+ — `spring.threads.virtual.enabled=true` для MVC на Virtual Threads
- Tomcat, Jetty, Undertow поддерживают Virtual Threads
- JDBC-драйверы (PostgreSQL, MySQL) работают с Virtual Threads без pinning (заменили `synchronized` на `ReentrantLock`)
- Virtual Threads + блокирующий код = альтернатива WebFlux для 80% сценариев

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Structured Concurrency — что это и зачем?

**Structured Concurrency** — API для управления группами связанных задач как единым целым. Гарантирует, что подзадачи завершатся (или будут отменены) до завершения родительской задачи.

```java
// Проблема: без Structured Concurrency
ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
Future<User> userFuture = executor.submit(() -> fetchUser(id));
Future<List<Order>> ordersFuture = executor.submit(() -> fetchOrders(id));

// Если fetchOrders бросит исключение — fetchUser продолжает выполняться!
// Если основной поток прервётся — подзадачи «утекают»

// Решение: Structured Concurrency (Java 21, preview → Java 24+)
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Subtask<User> user = scope.fork(() -> fetchUser(id));
    Subtask<List<Order>> orders = scope.fork(() -> fetchOrders(id));

    scope.join();           // ждём завершения обеих задач
    scope.throwIfFailed();  // бросить исключение, если одна из задач упала

    return new UserWithOrders(user.get(), orders.get());
}
// Если одна задача упадёт — другая будет отменена автоматически
// При выходе из try-with-resources все подзадачи гарантированно завершены
```

**Стратегии:**

```java
// ShutdownOnFailure — отменить все при первой ошибке
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    var a = scope.fork(() -> callServiceA());
    var b = scope.fork(() -> callServiceB());
    scope.join().throwIfFailed();
    return combine(a.get(), b.get());
}

// ShutdownOnSuccess — вернуть первый успешный результат
try (var scope = new StructuredTaskScope.ShutdownOnSuccess<String>()) {
    scope.fork(() -> fetchFromPrimary());
    scope.fork(() -> fetchFromFallback());
    scope.join();
    return scope.result(); // первый успешный
}
```

### Важное
- Structured Concurrency связывает жизненный цикл подзадач с родительской задачей
- `ShutdownOnFailure` — отмена всех при первой ошибке (параллельные зависимые запросы)
- `ShutdownOnSuccess` — вернуть первый успешный результат (racing, fallback)
- Прекрасно работает с Virtual Threads — каждая подзадача в своём виртуальном потоке

### Частые ошибки
- **Не вызывать `join()`** — без `join()` результаты недоступны и подзадачи могут утечь
- **Использовать вне try-with-resources** — scope должен быть закрыт для гарантии завершения
- **Путать с `CompletableFuture.allOf()`** — Structured Concurrency проще и безопаснее для fork-join паттерна

### Как используется в 2026
- Structured Concurrency финализирован и активно используется с Virtual Threads
- Заменяет ручное управление `Future` + `ExecutorService` для параллельных запросов
- В Spring-приложениях — для параллельного вызова нескольких микросервисов

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Scoped Values — замена ThreadLocal?

**Scoped Values** — механизм для передачи данных между методами без передачи параметров, альтернатива `ThreadLocal` для Virtual Threads.

```java
// ThreadLocal — проблемы с Virtual Threads:
// 1. Мутабельный — любой может перезаписать
// 2. Неограниченное время жизни — утечки памяти
// 3. При миллионе Virtual Threads — миллион копий

private static final ThreadLocal<User> CURRENT_USER = new ThreadLocal<>();
CURRENT_USER.set(user);
// ... где-то в стеке вызовов:
User u = CURRENT_USER.get();
CURRENT_USER.remove(); // легко забыть!

// ScopedValue — решение для Virtual Threads:
private static final ScopedValue<User> CURRENT_USER = ScopedValue.newInstance();

ScopedValue.where(CURRENT_USER, authenticatedUser)
    .run(() -> {
        // CURRENT_USER доступен во всём этом scope
        processRequest();
    });

// В вызываемом коде:
void processRequest() {
    User user = CURRENT_USER.get(); // доступно
    // user автоматически недоступен после выхода из run()
}
```

**Сравнение:**

| Критерий | ThreadLocal | ScopedValue |
|----------|------------|-------------|
| Мутабельность | Мутабельный (`set()`) | Immutable (только `where().run()`) |
| Время жизни | Пока не вызван `remove()` | Автоматически — ограничен `run()`/`call()` |
| Наследование | `InheritableThreadLocal` (копия) | Автоматическое наследование в child-scope |
| Память | Копия на каждый поток | Разделяемая ссылка |

### Важное
- ScopedValue immutable — нельзя `set()`, только `where().run()`
- Время жизни ограничено лексическим scope — нет утечек
- Идеально для Virtual Threads — не создаёт копию на каждый поток
- Дочерние scope (fork в Structured Concurrency) автоматически наследуют ScopedValue

### Частые ошибки
- **Вызвать `get()` вне `run()`** — `NoSuchElementException`; ScopedValue доступен только в scope
- **Использовать ThreadLocal с миллионами Virtual Threads** — OOM; мигрируйте на ScopedValue
- **ScopedValue для мутабельного состояния** — ScopedValue immutable; для мутабельного состояния нужен другой подход

### Как используется в 2026
- ScopedValue финализирован (preview → Java 25)
- Используется для propagation authentication context, trace ID, request metadata
- Spring Security и Micrometer начинают поддержку ScopedValue для Virtual Threads

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Pattern Matching для switch

**Pattern Matching для switch** (финализирован в Java 21) позволяет использовать patterns (включая type patterns, guarded patterns, null) в switch.

```java
// Type patterns в switch
static String format(Object obj) {
    return switch (obj) {
        case Integer i  -> "int: " + i;
        case Long l     -> "long: " + l;
        case Double d   -> "double: " + d;
        case String s   -> "string: " + s;
        case null       -> "null";       // обработка null!
        default         -> "unknown: " + obj;
    };
}

// Guarded patterns (when)
static String classify(Shape shape) {
    return switch (shape) {
        case Circle c when c.radius() > 100 -> "Большой круг";
        case Circle c                       -> "Круг r=" + c.radius();
        case Rectangle r when r.width() == r.height() -> "Квадрат";
        case Rectangle r                    -> "Прямоугольник";
        case Triangle t                     -> "Треугольник";
    };
}

// Sealed types — exhaustive без default
sealed interface Result<T> permits Success, Failure {}
record Success<T>(T value) implements Result<T> {}
record Failure<T>(String error) implements Result<T> {}

static <T> String handle(Result<T> result) {
    return switch (result) {
        case Success<T> s -> "OK: " + s.value();
        case Failure<T> f -> "Error: " + f.error();
        // default не нужен — компилятор знает все подтипы
    };
}
```

**Null-обработка:**

```java
// До Java 21: switch бросал NPE для null
// Java 21: можно обрабатывать null
static String process(String s) {
    return switch (s) {
        case null          -> "null value";
        case "hello"       -> "greeting";
        case String str when str.length() > 10 -> "long: " + str;
        case String str    -> "other: " + str;
    };
}
```

### Важное
- Pattern Matching + switch = мощная деконструкция типов (как в Scala/Kotlin)
- `when` — guard clause для дополнительных условий
- `null` можно обрабатывать в switch (ранее — NPE)
- С sealed types — exhaustive check без `default`
- Порядок case важен — более специфичные паттерны должны быть выше

### Частые ошибки
- **Неправильный порядок patterns** — `case String s` перед `case String s when s.length() > 10` → guarded pattern недостижим
- **Забыть `null` case** — если `null` не обработан и нет `default`, NPE как раньше
- **`default` с sealed types** — скрывает ошибки при добавлении нового подтипа

### Как используется в 2026
- Стандартная практика для обработки разных типов, состояний, событий
- Sealed + Records + Pattern Matching = визитор без Visitor pattern
- Активно используется в domain logic и event handling

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Record Patterns (деконструкция записей)

**Record Patterns** (финализированы в Java 21) позволяют деконструировать (разложить) Record на его компоненты прямо в pattern matching.

```java
record Point(int x, int y) {}
record Circle(Point center, double radius) {}

// instanceof с деконструкцией
if (obj instanceof Point(int x, int y)) {
    System.out.println("x=" + x + ", y=" + y);
}

// Вложенная деконструкция
if (shape instanceof Circle(Point(var x, var y), var r)) {
    System.out.println("Circle at (" + x + ", " + y + ") with radius " + r);
}

// В switch
static String describe(Shape shape) {
    return switch (shape) {
        case Circle(Point(var x, var y), var r) when r > 10 ->
            "Большой круг в (%d, %d)".formatted(x, y);
        case Circle(Point(var x, var y), var r) ->
            "Круг в (%d, %d) r=%.1f".formatted(x, y, r);
        case Rectangle(Point(var x, var y), var w, var h) ->
            "Прямоугольник в (%d, %d) %dx%d".formatted(x, y, w, h);
    };
}

// Unnamed patterns (Java 21) — игнорирование ненужных компонентов
if (obj instanceof Point(var x, _)) {
    // Нужен только x, y игнорируется
}
```

### Важное
- Record Patterns деконструируют Record на компоненты в одном выражении
- Работают в `instanceof` и `switch`
- Поддерживают вложенность — деконструкция Record внутри Record
- `_` (unnamed pattern) — для игнорирования ненужных компонентов

### Частые ошибки
- **Деконструировать не-Record** — Record Patterns работают только с Records
- **Забыть, что деконструкция создаёт копии** — значения копируются, а не ссылаются на оригинал

### Как используется в 2026
- В связке с Sealed Interfaces + Records — элегантный pattern matching без instanceof-каскадов
- Заменяет Visitor pattern для обработки иерархий типов

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Sequenced Collections

**Sequenced Collections** (Java 21) — новые интерфейсы, добавляющие единообразный доступ к первому/последнему элементу и реверсу для упорядоченных коллекций.

```java
// Проблема до Java 21:
// List: list.get(0), list.get(list.size()-1)
// Deque: deque.getFirst(), deque.getLast()
// SortedSet: set.first(), set.last()
// LinkedHashSet: ??? (итератор)

// Java 21 — единый API:
interface SequencedCollection<E> extends Collection<E> {
    E getFirst();
    E getLast();
    void addFirst(E e);
    void addLast(E e);
    E removeFirst();
    E removeLast();
    SequencedCollection<E> reversed();
}

// Примеры использования:
List<String> list = new ArrayList<>(List.of("A", "B", "C"));
list.getFirst();  // "A"
list.getLast();   // "C"
list.reversed().forEach(System.out::println); // C, B, A

// LinkedHashSet — теперь с getFirst/getLast
LinkedHashSet<String> set = new LinkedHashSet<>(List.of("X", "Y", "Z"));
set.getFirst(); // "X"
set.getLast();  // "Z"

// SequencedMap
interface SequencedMap<K, V> extends Map<K, V> {
    Map.Entry<K, V> firstEntry();
    Map.Entry<K, V> lastEntry();
    Map.Entry<K, V> pollFirstEntry();
    Map.Entry<K, V> pollLastEntry();
    SequencedMap<K, V> reversed();
}

LinkedHashMap<String, Integer> map = new LinkedHashMap<>();
map.put("a", 1);
map.put("b", 2);
map.firstEntry(); // a=1
map.lastEntry();  // b=2
```

**Иерархия:**

```
                  Collection
                      |
              SequencedCollection
               /            \
        List              SequencedSet
                           /       \
                     SortedSet   LinkedHashSet
```

### Важное
- `getFirst()` / `getLast()` — единый API для List, Deque, LinkedHashSet, SortedSet
- `reversed()` — возвращает view (не копию), изменения отражаются на оригинале
- `Collections.unmodifiableSequencedCollection()` — для иммутабельных sequenced коллекций
- Retrofit — существующие классы (ArrayList, LinkedHashSet, TreeSet) получили эти методы

### Частые ошибки
- **`getFirst()` на пустой коллекции** — `NoSuchElementException`; проверяйте `isEmpty()`
- **Мутация через `reversed()` view** — изменения в reversed view влияют на оригинальную коллекцию
- **`addFirst()` / `addLast()` для unmodifiable** — `UnsupportedOperationException`

### Как используется в 2026
- Sequenced Collections — часть стандартного API, используется повсеместно
- Заменяет нечитаемые конструкции вроде `list.get(list.size() - 1)`

[к оглавлению](#Новое-в-Java-от-11-до-25)

---

# Java 25 (LTS, сентябрь 2025)

## Какие ключевые нововведения в Java 25?

Java 25 — четвёртый LTS, включающий финализацию многих preview-фич из Java 22–24 и новые возможности.

| Версия | Нововведение |
|--------|-------------|
| Java 22 | Unnamed Variables `_` (final), Stream Gatherers (preview), Statements before super() (preview) |
| Java 23 | Primitive Types in Patterns (preview), Module Import Declarations (preview), Markdown doc comments |
| Java 24 | Stream Gatherers (final), Compact Object Headers (experimental), Ahead-of-Time compilation |
| Java 25 | Flexible Constructor Bodies (final), Module Import Declarations (final), Primitive Patterns (final), Compact Object Headers, Scoped Values (final), Structured Concurrency (final) |

**Ключевые финализированные фичи в Java 25:**

1. **Flexible Constructor Bodies** — код до `super()` / `this()`
2. **Module Import Declarations** — `import module java.base`
3. **Primitive Types in Patterns** — примитивы в switch/instanceof
4. **Scoped Values** — замена ThreadLocal для Virtual Threads
5. **Structured Concurrency** — управление параллельными задачами
6. **Stream Gatherers** — пользовательские промежуточные операции в Stream API
7. **Compact Object Headers** — уменьшение размера заголовка объекта с 12 до 8 байт

### Важное
- Java 25 LTS — рекомендуемая версия для новых проектов, стартующих во второй половине 2025+
- Scoped Values и Structured Concurrency финализированы — Virtual Threads экосистема полная
- Stream Gatherers — расширяемость Stream API пользовательскими операциями
- Compact Object Headers — 10-15% снижение потребления памяти для object-heavy приложений

### Частые ошибки
- **Мигрировать на Java 25 без проверки зависимостей** — некоторые библиотеки могут не поддерживать; проверяйте Lombok, byte-code manipulation
- **Использовать preview features из 22-24, которые могли измениться** — preview API нестабильно

### Как используется в 2026
- Spring Boot 3.4+ поддерживает Java 25
- Ранние adopters уже используют Java 25 в продакшене
- Java 21 остаётся безопасным выбором для консервативных проектов

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Flexible Constructor Bodies

**Flexible Constructor Bodies** (финализировано в Java 25) позволяют выполнять код **до вызова** `super()` или `this()` в конструкторе. Ранее `super()`/`this()` обязательно были первым оператором.

```java
// До Java 25 — super() обязан быть первым
public class PositiveInteger extends Number {
    private final int value;

    public PositiveInteger(int value) {
        // if (value <= 0) throw ...; // НЕЛЬЗЯ до super()!
        super();
        if (value <= 0) throw new IllegalArgumentException("Must be positive");
        this.value = value;
    }
}

// Java 25 — код до super()
public class PositiveInteger extends Number {
    private final int value;

    public PositiveInteger(int value) {
        if (value <= 0) {
            throw new IllegalArgumentException("Must be positive: " + value);
        }
        super(); // теперь может быть не первым
        this.value = value;
    }
}

// Практический пример — подготовка аргументов для super()
public class NamedLogger extends AbstractLogger {
    public NamedLogger(String rawName) {
        var cleanName = rawName.strip().toLowerCase();
        if (cleanName.isEmpty()) {
            throw new IllegalArgumentException("Name cannot be blank");
        }
        super(cleanName); // передаём обработанное значение
    }
}
```

**Ограничения:** до `super()` нельзя обращаться к `this` полям или методам экземпляра (объект ещё не инициализирован).

### Важное
- Код до `super()`/`this()` может: валидировать аргументы, подготовить параметры, логировать
- Код до `super()` НЕ может: обращаться к `this`, вызывать instance-методы, присваивать instance-поля
- Решает давнюю проблему необходимости static helper-методов для подготовки аргументов super()

### Частые ошибки
- **Обращение к `this` до `super()`** — ошибка компиляции; объект не инициализирован
- **Инициализация полей до `super()`** — невозможно; `this.field = value` до `super()` запрещено

### Как используется в 2026
- Упрощает конструкторы — не нужны static factory methods для валидации перед super()
- Особенно полезно для Records с валидацией и наследования

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Module Import Declarations и упрощённый импорт

**Module Import Declarations** (финализировано в Java 25) позволяют импортировать все пакеты модуля одной строкой.

```java
// Импорт всего модуля
import module java.base;    // все пакеты java.base (java.util, java.io, java.time, ...)
import module java.sql;     // java.sql, javax.sql

// Вместо:
import java.util.*;
import java.util.stream.*;
import java.io.*;
import java.time.*;
import java.math.*;
// ... и т.д.

// Пример программы
import module java.base;

public class Demo {
    public static void main(String[] args) {
        // List, Map, Stream — всё доступно без отдельных импортов
        var numbers = List.of(1, 2, 3, 4, 5);
        var sum = numbers.stream()
            .filter(n -> n % 2 == 0)
            .mapToInt(Integer::intValue)
            .sum();
        System.out.println("Sum of evens: " + sum);

        // LocalDate — тоже доступен
        var today = LocalDate.now();
    }
}
```

**Implicit imports для простых программ (Java 22+):**

```java
// Без какого-либо main() boilerplate (Implicitly Declared Classes)
void main() {
    println("Hello, World!");
    // java.base импортирован автоматически
    // println() доступен без System.out
}
```

### Важное
- `import module` импортирует все экспортируемые пакеты модуля
- Конфликты имён разрешаются обычным `import` конкретного класса
- Упрощает обучение и написание простых программ
- Для production-кода рекомендуется явный импорт — IDE генерирует его автоматически

### Частые ошибки
- **Конфликты имён** — `import module java.base` и `import module java.desktop` оба содержат `List`; нужен явный `import java.util.List`
- **В production-коде** — `import module` может усложнить читаемость для других разработчиков

### Как используется в 2026
- Для скриптов, обучения, quick prototyping
- Production-код обычно использует явные импорты (IDE поддерживает автоматически)

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Primitive Types in Patterns и instanceof

**Primitive Types in Patterns** (финализировано в Java 25) расширяют pattern matching на примитивные типы. Ранее patterns работали только с reference types.

```java
// Примитивы в instanceof
Object obj = 42;
if (obj instanceof int i) {
    System.out.println("int value: " + i);
}

// Примитивы в switch
static String classify(Object obj) {
    return switch (obj) {
        case int i when i > 0    -> "positive int: " + i;
        case int i               -> "non-positive int: " + i;
        case long l              -> "long: " + l;
        case double d            -> "double: " + d;
        case String s            -> "string: " + s;
        default                  -> "other";
    };
}

// Безопасные преобразования (narrowing) без потери данных
long value = 42L;
if (value instanceof int i) { // true — 42 помещается в int
    System.out.println("fits in int: " + i);
}

long big = Long.MAX_VALUE;
if (big instanceof int i) { // false — не помещается
    // не выполнится
}
```

### Важное
- Примитивные паттерны проверяют, можно ли безопасно привести значение к целевому типу
- `long instanceof int` — true только если значение помещается без потери данных
- Работает для boxing/unboxing: `Object(42) instanceof int` → true
- Объединяет обработку примитивов и ссылочных типов в одном switch

### Частые ошибки
- **Ожидать автоматический narrowing cast** — `long instanceof int` проверяет допустимость, а не выполняет приведение; значение должно быть в диапазоне int

### Как используется в 2026
- Упрощает обработку числовых типов в generic-коде
- Дополняет картину полного pattern matching для всех типов Java

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Что нового в Stream API от Java 11 до Java 25?

**Эволюция Stream API:**

```java
// Java 16: Stream.toList() — неизменяемый список
List<String> names = users.stream()
    .map(User::getName)
    .toList(); // вместо .collect(Collectors.toList())

// Java 16: Stream.mapMulti() — замена flatMap для простых случаев
Stream.of(1, 2, 3, 4)
    .<Integer>mapMulti((n, consumer) -> {
        if (n % 2 == 0) {
            consumer.accept(n);
            consumer.accept(n * 10);
        }
    })
    .toList(); // [2, 20, 4, 40]

// Java 22/25: Stream Gatherers — пользовательские промежуточные операции
// Gatherer позволяет создавать stateful промежуточные операции

// Встроенные Gatherers:
// windowFixed — фиксированные окна
List<List<Integer>> windows = Stream.of(1, 2, 3, 4, 5, 6)
    .gather(Gatherers.windowFixed(3))
    .toList(); // [[1, 2, 3], [4, 5, 6]]

// windowSliding — скользящее окно
List<List<Integer>> sliding = Stream.of(1, 2, 3, 4, 5)
    .gather(Gatherers.windowSliding(3))
    .toList(); // [[1, 2, 3], [2, 3, 4], [3, 4, 5]]

// fold — свёртка с начальным значением
Optional<String> joined = Stream.of("a", "b", "c")
    .gather(Gatherers.fold(() -> "", (acc, el) -> acc + el))
    .findFirst(); // "abc"

// scan — промежуточные результаты свёртки
List<Integer> runningSum = Stream.of(1, 2, 3, 4, 5)
    .gather(Gatherers.scan(() -> 0, Integer::sum))
    .toList(); // [1, 3, 6, 10, 15]

// mapConcurrent — параллельная обработка с Virtual Threads
List<String> results = urls.stream()
    .gather(Gatherers.mapConcurrent(10, url -> fetchUrl(url)))
    .toList(); // до 10 параллельных запросов
```

**Создание custom Gatherer:**

```java
// Пример: distinctBy — unique по ключу (без стандартной реализации)
static <T, K> Gatherer<T, ?, T> distinctBy(Function<T, K> keyExtractor) {
    return Gatherer.ofSequential(
        HashSet<K>::new,
        (state, element, downstream) -> {
            K key = keyExtractor.apply(element);
            if (state.add(key)) {
                return downstream.push(element);
            }
            return true;
        }
    );
}

// Использование:
users.stream()
    .gather(distinctBy(User::getEmail))
    .toList();
```

### Важное
- `toList()` — возвращает **неизменяемый** список (в отличие от `Collectors.toList()`)
- `mapMulti` — эффективная замена `flatMap` для case, когда элемент маппится в 0-N результатов
- `Gatherers` — расширяемые промежуточные операции; можно создавать свои
- `mapConcurrent` — виртуальные потоки прямо в Stream pipeline

### Частые ошибки
- **Мутация списка из `toList()`** — `UnsupportedOperationException`; если нужен мутабельный — `collect(Collectors.toList())`
- **`mapConcurrent` для CPU-bound** — Virtual Threads не ускоряют CPU-bound; используйте `parallelStream()`
- **Custom Gatherer без proper state management** — Gatherer может быть parallel; state должен быть thread-safe или sequential

### Как используется в 2026
- `toList()` — повсеместно заменяет `collect(Collectors.toList())`
- Gatherers — новый инструмент для сложных потоковых обработок (окна, группировки, stateful)
- `mapConcurrent` — удобная параллелизация I/O-задач в stream

[к оглавлению](#Новое-в-Java-от-11-до-25)

---

# Кросс-версионные вопросы

## Какую версию Java выбрать для нового проекта в 2026 году?

**Рекомендации:**

| Сценарий | Рекомендуемая версия | Обоснование |
|----------|---------------------|-------------|
| Новый проект, greenfield | **Java 25** (или 21) | Все современные фичи, LTS |
| Корпоративный проект, консервативный подход | **Java 21** | Зрелая LTS, полная поддержка Spring Boot 3.x |
| Существующий проект на Java 17 | **Java 21** (миграция) | Virtual Threads, Pattern Matching |
| Существующий проект на Java 11 | **Java 21** (миграция) | Java 11 end-of-life |
| Существующий проект на Java 8 | **Java 21** (миграция) | Критически важно мигрировать |

**Факторы выбора:**
1. **LTS vs non-LTS** — только LTS для продакшена (11, 17, 21, 25)
2. **Совместимость зависимостей** — Spring Boot, Hibernate, Lombok, Kotlin
3. **Поддержка вендора** — Oracle, Amazon Corretto, Eclipse Adoptium, Azul
4. **Требования команды** — уровень знакомства с новыми фичами

**Дистрибутивы JDK в 2026:**

| Дистрибутив | Вендор | Бесплатный | Поддержка |
|-------------|--------|-----------|-----------|
| Eclipse Temurin | Adoptium | Да | Community |
| Amazon Corretto | Amazon | Да | Amazon |
| Azul Zulu | Azul | Да (Community) | Azul |
| Oracle JDK | Oracle | Нет (NFTC license) | Oracle |
| GraalVM CE | Oracle | Да | Community |
| BellSoft Liberica | BellSoft | Да | BellSoft |

### Важное
- **Java 21 или 25** — единственный правильный выбор для нового проекта в 2026
- Virtual Threads — основная мотивация для миграции на 21+
- Eclipse Temurin или Amazon Corretto — наиболее популярные бесплатные дистрибутивы
- Модель релизов: LTS каждые 2 года (17→21→25), non-LTS каждые 6 месяцев

### Частые ошибки
- **Стартовать новый проект на Java 11 или 17** — упускаете Virtual Threads, Pattern Matching, Records
- **Использовать Oracle JDK без лицензии** — с 2019 Oracle JDK требует подписку для коммерческого использования
- **Не фиксировать версию JDK в проекте** — используйте `.sdkmanrc`, `toolchains.xml` или `jenv`

### Как используется в 2026
- Java 21 — доминирующая версия в продакшене
- Java 25 — набирает обороты, ранние adopters уже используют
- Toolchains (Gradle/Maven) — позволяют собирать проект на конкретной версии JDK

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Как мигрировать проект с Java 8/11 на Java 21/25?

**Пошаговый план миграции:**

**Этап 1: Подготовка**

```bash
# Проверить совместимость зависимостей
./mvnw versions:display-dependency-updates
./mvnw versions:display-plugin-updates

# Ключевые зависимости для обновления:
# Spring Boot 2.x → 3.x (требует Java 17+)
# Hibernate 5.x → 6.x
# Lombok → последняя версия
# Mockito → 5.x+
# Jackson → 2.15+
```

**Этап 2: javax → jakarta (при переходе на Spring Boot 3.x)**

```bash
# OpenRewrite — автоматическая миграция
./mvnw org.openrewrite.maven:rewrite-maven-plugin:run \
  -Drewrite.recipeArtifactCoordinates=org.openrewrite.recipe:rewrite-spring:LATEST \
  -Drewrite.activeRecipes=org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_0
```

```xml
<!-- Или добавить в pom.xml -->
<plugin>
    <groupId>org.openrewrite.maven</groupId>
    <artifactId>rewrite-maven-plugin</artifactId>
    <version>5.x</version>
    <configuration>
        <activeRecipes>
            <recipe>org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_0</recipe>
        </activeRecipes>
    </configuration>
</plugin>
```

**Этап 3: Основные изменения при миграции**

| С версии | Изменение | Действие |
|----------|-----------|----------|
| Java 8 → 11 | Удалены JAXB, JAX-WS | Добавить Jakarta зависимости |
| Java 8 → 11 | Module system (JPMS) | Добавить `--add-opens` если нужен reflective access |
| Java 11 → 17 | Сильная инкапсуляция internal API | `--add-opens` или заменить internal API |
| Java 17 → 21 | javax → jakarta | OpenRewrite |
| Any → 21 | `SecurityManager` удалён | Убрать использование |

**Этап 4: Включение новых возможностей**

```properties
# Virtual Threads (Java 21+)
spring.threads.virtual.enabled=true

# Полезные JVM-флаги
-XX:+UseZGC                    # ZGC для низкой латентности
-XX:+ZGenerational             # Generational ZGC (Java 21+)
```

```java
// Постепенно обновлять код:
// 1. var для локальных переменных (Java 10+)
// 2. Records для DTO (Java 16+)
// 3. Text Blocks для SQL, JSON (Java 15+)
// 4. Switch Expressions (Java 14+)
// 5. Pattern Matching (Java 21+)
// 6. Sealed Interfaces для domain types
```

### Важное
- **OpenRewrite** — автоматизирует 80% миграции (javax→jakarta, deprecated API, Spring Boot upgrade)
- Миграция поэтапная: сначала версия JDK, потом Spring Boot, потом рефакторинг
- `--add-opens` — временное решение для reflective access; цель — убрать его
- Тесты — первый приоритет: убедитесь, что все тесты проходят на новой версии

### Частые ошибки
- **Мигрировать всё сразу** — мигрируйте поэтапно: JDK → фреймворки → рефакторинг кода
- **Не обновить Lombok** — Lombok сильно зависит от internal API JDK; каждая мажорная версия Java требует обновления
- **Не проверить reflection-зависимости** — библиотеки, использующие `sun.misc.Unsafe` или internal API, могут сломаться
- **Игнорировать deprecation warnings** — deprecated API могут быть удалены в следующей версии

### Как используется в 2026
- OpenRewrite — стандартный инструмент для автоматической миграции
- Большинство enterprise-проектов завершили миграцию на Java 17/21
- Миграция с Java 21 на 25 — минимальные изменения (в основном обновление зависимостей)

[к оглавлению](#Новое-в-Java-от-11-до-25)

## Эволюция null-safety в Java

Java не имеет встроенной null-safety (в отличие от Kotlin), но постепенно добавляет инструменты для работы с null.

**Optional (Java 8+) — эволюция API:**

```java
// Java 8 — базовый Optional
Optional<String> name = Optional.ofNullable(user.getName());
String result = name.orElse("Unknown");

// Java 9 — ifPresentOrElse, or, stream
optional.ifPresentOrElse(
    value -> process(value),
    () -> handleAbsence()
);

Optional<String> fallback = primary.or(() -> secondary);

// Java 10 — orElseThrow() без аргументов
String value = optional.orElseThrow(); // вместо optional.get()

// Java 11 — Optional.isEmpty()
if (optional.isEmpty()) { ... } // вместо !optional.isPresent()

// Java 16 — mapMulti (не для Optional, но связано)
// Java 21+ — Pattern Matching работает с null:
switch (value) {
    case null -> handleNull();
    case String s -> process(s);
    default -> other();
}
```

**Helpful NullPointerException (Java 14+):**

```java
var street = user.getAddress().getCity().toUpperCase();
// До Java 14: NullPointerException (непонятно, что null)
// Java 14+:   Cannot invoke "City.toUpperCase()" because
//             the return value of "Address.getCity()" is null
```

**Аннотации `@Nullable`/`@NonNull`:**

```java
// JetBrains annotations, Jakarta annotations, или Spring annotations
import org.springframework.lang.NonNull;
import org.springframework.lang.Nullable;

@NonNull
public User findById(@NonNull Long id) {
    return userRepository.findById(id)
        .orElseThrow(() -> new NotFoundException("User not found: " + id));
}

@Nullable
public User findByEmail(@NonNull String email) {
    return userRepository.findByEmail(email).orElse(null);
}
```

**NullAway / Checker Framework — статический анализ:**

```xml
<!-- NullAway — error-prone plugin -->
<dependency>
    <groupId>com.uber.nullaway</groupId>
    <artifactId>nullaway</artifactId>
</dependency>
```

### Важное
- Java **не имеет** встроенной null-safety на уровне типов (в отличие от Kotlin `?`)
- `Optional` — для возвращаемых значений, НЕ для параметров методов или полей
- `@Nullable`/`@NonNull` — документация + статический анализ (IDE, SpotBugs, NullAway)
- Helpful NPE (Java 14+) значительно упрощает отладку NullPointerException

### Частые ошибки
- **Optional как параметр метода** — anti-pattern; используйте перегрузку методов или `@Nullable`
- **Optional как поле класса** — Optional не Serializable; используйте `@Nullable` для полей
- **`optional.get()` без проверки** — может бросить `NoSuchElementException`; используйте `orElseThrow()`, `orElse()`, `ifPresent()`
- **Игнорировать `@Nullable` аннотации** — IDE подсвечивает потенциальные NPE; не игнорируйте warnings

### Как используется в 2026
- `Optional` — стандарт для возвращаемых значений (особенно в repository/service слоях)
- `@Nullable`/`@NonNull` аннотации — best practice; Spring использует `@NonNullApi` для всего пакета
- JSpecify (`org.jspecify`) — новый стандарт null-safety аннотаций, набирает популярность
- Pattern Matching + null handling в switch — удобная обработка nullable значений
- Valhalla (будущее) — потенциально улучшит null-safety через Value Types

[к оглавлению](#Новое-в-Java-от-11-до-25)
