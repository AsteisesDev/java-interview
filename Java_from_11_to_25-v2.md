[Вопросы для собеседования](README.md)

# Новое в Java: от 11 до 25

+ [Какие ключевые нововведения появились в Java 11?](#какие-ключевые-нововведения-появились-в-java-11)
+ [Что такое HttpClient API в Java 11?](#что-такое-httpclient-api-в-java-11)
+ [Новые методы String в Java 11](#новые-методы-string-в-java-11)
+ [Var в лямбда-выражениях (Java 11)](#var-в-лямбда-выражениях-java-11)
+ [Запуск .java файлов без компиляции](#запуск-java-файлов-без-компиляции)
+ [Какие ключевые нововведения появились в Java 17?](#какие-ключевые-нововведения-появились-в-java-17)
+ [Что такое Sealed Classes и Sealed Interfaces?](#что-такое-sealed-classes-и-sealed-interfaces)
+ [Pattern Matching для instanceof](#pattern-matching-для-instanceof)
+ [Records — что это и зачем?](#records--что-это-и-зачем)
+ [Text Blocks (многострочные строки)](#text-blocks-многострочные-строки)
+ [Switch Expressions](#switch-expressions)
+ [Удалённые и устаревшие API в Java 17](#удалённые-и-устаревшие-api-в-java-17)
+ [Какие ключевые нововведения появились в Java 21?](#какие-ключевые-нововведения-появились-в-java-21)
+ [Что такое Virtual Threads (Project Loom)?](#что-такое-virtual-threads-project-loom)
+ [Structured Concurrency — что это и зачем?](#structured-concurrency--что-это-и-зачем)
+ [Scoped Values — замена ThreadLocal?](#scoped-values--замена-threadlocal)
+ [Pattern Matching для switch](#pattern-matching-для-switch)
+ [Record Patterns (деконструкция записей)](#record-patterns-деконструкция-записей)
+ [Sequenced Collections](#sequenced-collections)
+ [Какие ключевые нововведения в Java 25?](#какие-ключевые-нововведения-в-java-25)
+ [Flexible Constructor Bodies](#flexible-constructor-bodies)
+ [Module Import Declarations и упрощённый импорт](#module-import-declarations-и-упрощённый-импорт)
+ [Primitive Types in Patterns и instanceof](#primitive-types-in-patterns-и-instanceof)
+ [Что нового в Stream API от Java 11 до Java 25?](#что-нового-в-stream-api-от-java-11-до-java-25)
+ [Какую версию Java выбрать для нового проекта в 2026 году?](#какую-версию-java-выбрать-для-нового-проекта-в-2026-году)
+ [Как мигрировать проект с Java 8/11 на Java 21/25?](#как-мигрировать-проект-с-java-811-на-java-2125)
+ [Эволюция null-safety в Java](#эволюция-null-safety-в-java)

---

# Java 11 (LTS, сентябрь 2018)

## Какие ключевые нововведения появились в Java 11?
<!-- grade: junior -->

Java 11 — первый LTS-релиз после Java 8 в новой модели релизов (каждые 6 месяцев). Многие проекты мигрировали напрямую с Java 8 на Java 11.

### Основные нововведения

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

### Новые методы коллекций и файлов

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

### Частые ошибки

- Миграция с Java 8 без проверки удалённых модулей — JAXB, JAX-WS больше не в JDK; нужно добавить зависимости
- Путать `strip()` и `trim()` — `strip()` корректно обрабатывает Unicode-пробелы, `trim()` — только ASCII
- Не обновлять библиотеки при миграции — многие библиотеки (Lombok, Mockito, ASM) требуют обновления для Java 11+

### Как используется в 2026

- Java 11 end-of-life для бесплатной поддержки у большинства вендоров
- Рекомендуется миграция на Java 21 или 25; новые проекты на Java 11 не стартуют
- Многие legacy-проекты всё ещё на Java 11, постепенно мигрируют

> **На собеседовании:** интервьюер ожидает, что вы не просто перечислите новые API, а знаете, какие модули были удалены и почему миграция с Java 8 на 11 нетривиальна. Частая ошибка — забыть упомянуть удаление Java EE модулей (JAXB, JAX-WS).

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Что такое HttpClient API в Java 11?
<!-- grade: junior -->

HttpClient — стандартный HTTP-клиент в JDK, поддерживающий HTTP/1.1 и HTTP/2, синхронные и асинхронные запросы. Заменяет устаревший `HttpURLConnection`.

> **Аналогия из жизни:** если старый `HttpURLConnection` — это отправка письма почтой (долго настраивать, неудобно), то `HttpClient` — мессенджер: отправил запрос, получил ответ, можно отправить несколько одновременно.

<details><summary>Пример: создание клиента и запросы</summary>

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

</details>

### Чтение ответа в разные типы

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

### Частые ошибки

- Создание нового `HttpClient` на каждый запрос — дорого; переиспользуйте экземпляр
- Не обрабатывать HTTP-статусы — `send()` не бросает исключение для 4xx/5xx; проверяйте `statusCode()`
- Блокирующий `join()` на `sendAsync()` — теряется смысл асинхронности; используйте `thenApply`/`thenAccept`

### Как используется в 2026

- В Spring-приложениях вместо HttpClient чаще используют `RestClient` (Spring 6.1+) или `WebClient`
- HttpClient из JDK полезен для standalone-приложений и библиотек без Spring-зависимостей
- В микросервисах HTTP-клиент обычно оборачивается в абстракцию с retry, timeout, circuit breaker

> **На собеседовании:** важно упомянуть потокобезопасность `HttpClient` (один экземпляр на приложение), нативную поддержку HTTP/2 и то, что `send()` не бросает исключение для 4xx/5xx — нужна ручная проверка `statusCode()`.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Новые методы String в Java 11
<!-- grade: junior -->

Java 11 добавила к классу `String` методы `isBlank()`, `strip()`, `lines()` и `repeat()`, покрывающие частые операции, для которых ранее требовались внешние библиотеки.

```java
// isBlank() — проверка на пустоту или только пробелы (включая Unicode)
"".isBlank();        // true
"   ".isBlank();     // true
" hello ".isBlank(); // false
// Отличие от isEmpty(): "   ".isEmpty() -> false, "   ".isBlank() -> true

// strip() — удаление пробелов в начале и конце (Unicode-aware)
"  hello  ".strip();        // "hello"
"  hello  ".stripLeading(); // "hello  "
"  hello  ".stripTrailing();// "  hello"

// lines() — разбиение на строки (возвращает Stream<String>)
"line1\nline2\nline3".lines().toList();  // ["line1", "line2", "line3"]

// repeat(int) — повторение строки
"abc".repeat(3);  // "abcabcabc"
"-".repeat(50);   // 50 дефисов
```

### strip() vs trim()

| Метод | Unicode-пробелы | Поведение |
|-------|-----------------|-----------|
| `trim()` | Только ASCII (<=\u0020) | Старый метод, не знает про Unicode |
| `strip()` | Все Unicode-пробелы (\u00A0, \u2000, \u3000...) | Рекомендуется в новом коде |

### Частые ошибки

- Использовать `trim()` вместо `strip()` — `trim()` не удаляет Unicode-пробелы (неразрывный пробел `\u00A0`)
- `repeat(0)` — возвращает пустую строку, не null; это корректное поведение

### Как используется в 2026

- Эти методы — повседневный инструмент; используются повсеместно
- `isBlank()` заменяет `StringUtils.isBlank()` из Apache Commons в большинстве случаев

> **На собеседовании:** ключевой момент — разница между `strip()` и `trim()` (Unicode), и между `isBlank()` и `isEmpty()` (пробельные строки). Покажите, что знаете, когда какой метод уместен.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Var в лямбда-выражениях (Java 11)
<!-- grade: junior -->

В Java 11 `var` разрешён в параметрах лямбда-выражений. Основная цель — возможность добавлять аннотации к лямбда-параметрам, что без `var` невозможно.

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
// (var x, y) -> ошибка компиляции
// (var x, var y) -> OK
```

### Частые ошибки

- Злоупотребление `var` — `var result = process()` непонятно; используйте `var` когда тип очевиден из правой части
- `var` для полей класса — не поддерживается; `var` только для локальных переменных и лямбда-параметров
- Смешивание `var` и явных типов в лямбде — `(var x, String y)` приводит к ошибке компиляции

### Как используется в 2026

- `var` широко используется для локальных переменных, особенно с дженериками
- Стандарты кодирования: `var` допустим, когда тип очевиден; явный тип — когда нет
- IntelliJ IDEA подсвечивает выведенный тип при наведении

> **На собеседовании:** важно сказать, что `var` — не ключевое слово, а зарезервированное имя типа. Главная мотивация для `var` в лямбдах — аннотации (`@NonNull`, `@Nullable`). Частая ошибка — считать, что `var` делает Java динамически типизированной.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Запуск .java файлов без компиляции
<!-- grade: junior -->

Java 11 позволяет запускать single-file Java-программы напрямую без явного вызова `javac` — файл компилируется в памяти и сразу выполняется.

```bash
# До Java 11 — два шага:
javac HelloWorld.java
java HelloWorld

# Java 11+ — один шаг:
java HelloWorld.java
```

<details><summary>Пример: Java как скрипт (Unix)</summary>

```bash
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

</details>

### Частые ошибки

- Использовать для многофайловых проектов — не работает; только один файл
- Передать имя без `.java` — `java HelloWorld` ищет `.class` файл; для исходника нужно `java HelloWorld.java`

### Как используется в 2026

- Удобно для quick scripts и прототипирования
- JEP 458 (Java 22+) расширил: поддержка нескольких классов в одном файле, implicit `main()`
- JBang — популярный инструмент для Java-скриптов с поддержкой зависимостей

> **На собеседовании:** достаточно упомянуть, что это работает только для single-file программ, компиляция происходит в памяти. Вопрос редко задают отдельно, чаще как часть обзора Java 11.

[к оглавлению](#новое-в-java-от-11-до-25)

---

# Java 17 (LTS, сентябрь 2021)

## Какие ключевые нововведения появились в Java 17?
<!-- grade: junior -->

Java 17 — второй LTS после Java 11, ставший минимальной версией для Spring Boot 3.x и Spring Framework 6.x. В этом релизе финализированы Records, Sealed Classes, Pattern Matching и Text Blocks.

### Нововведения по версиям (Java 12-17)

| Версия | Нововведение |
|--------|-------------|
| Java 12 | Switch Expressions (preview) |
| Java 13 | Text Blocks (preview) |
| Java 14 | Records (preview), Pattern Matching для instanceof (preview), NullPointerException с подробностями |
| Java 15 | Text Blocks (final), Sealed Classes (preview), ZGC (production) |
| Java 16 | Records (final), Pattern Matching для instanceof (final), Stream.toList() |
| Java 17 | Sealed Classes (final), удалён Security Manager, новый macOS rendering pipeline |

<details><summary>Пример: Helpful NullPointerException и Stream.toList()</summary>

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

</details>

### Частые ошибки

- `stream().toList()` vs `stream().collect(Collectors.toList())` — `toList()` возвращает неизменяемый список; если нужен мутабельный — `collect(Collectors.toList())`
- Не обновить зависимости при миграции на 17 — Lombok, Mockito, Byte Buddy, ASM требуют обновления

### Как используется в 2026

- Java 17 — актуальная LTS, широко используется в продакшене
- Большинство enterprise-проектов на Java 17 или мигрируют на Java 21
- Новые проекты стартуют на Java 21 или 25

> **На собеседовании:** главное — знать, что Java 17 обязательна для Spring Boot 3.x, и уметь перечислить ключевые финализированные фичи: Records, Sealed Classes, Pattern Matching для instanceof, Text Blocks. Частая ошибка — не знать разницу между `toList()` и `collect(Collectors.toList())`.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Что такое Sealed Classes и Sealed Interfaces?
<!-- grade: middle -->

Sealed Classes (запечатанные классы) — механизм ограничения набора классов, которые могут наследоваться от данного класса или реализовывать интерфейс. Это позволяет компилятору знать все возможные подтипы и проверять полноту switch-выражений.

> **Аналогия из жизни:** sealed class — как закрытый клуб с фейс-контролем: только перечисленные в списке (permits) могут войти, и охранник (компилятор) гарантирует, что никто посторонний не проникнет.

<details><summary>Пример: sealed interface и exhaustive switch</summary>

```java
// Запечатанный интерфейс — только перечисленные классы могут его реализовать
public sealed interface Shape
    permits Circle, Rectangle, Triangle {
}

public record Circle(double radius) implements Shape {}
public record Rectangle(double width, double height) implements Shape {}
public final class Triangle implements Shape {
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

</details>

### Модификаторы наследников

| Модификатор | Поведение |
|-------------|-----------|
| `final` | Нельзя наследоваться далее |
| `sealed` | Можно, но с ограничением (свой `permits`) |
| `non-sealed` | Снять ограничение, любой может наследоваться |

```java
public sealed class Animal permits Dog, Cat, Fish {}
public final class Dog extends Animal {}
public sealed class Cat extends Animal permits Persian, Siamese {}
public non-sealed class Fish extends Animal {}  // открыт для наследования
```

### Частые ошибки

- Забыть `permits` — если наследники в том же файле, `permits` необязателен (Java выведет автоматически)
- `non-sealed` нарушает гарантии — если один из подтипов `non-sealed`, exhaustive switch невозможен
- Sealed + Records — records implicitly final, поэтому идеальные кандидаты для sealed

### Как используется в 2026

- Sealed classes + Records + Pattern Matching = алгебраические типы данных (ADT) в Java
- Широко используется для моделирования доменных событий, состояний, результатов
- Пример: `sealed interface Result<T> permits Success<T>, Failure<T>` — type-safe обработка результатов

> **На собеседовании:** покажите связку Sealed + Records + Pattern Matching — это главная ценность sealed classes. Интервьюер ожидает, что вы знаете про exhaustive switch без default и три модификатора наследников (final, sealed, non-sealed).

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Pattern Matching для instanceof
<!-- grade: junior -->

Pattern Matching для instanceof (финализирован в Java 16) устраняет необходимость явного приведения типа после проверки `instanceof` — переменная привязывается к типу прямо в условии.

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
```

### Практическое применение — equals()

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

### Частые ошибки

- Pattern variable с `||` — `if (obj instanceof String s || obj instanceof Integer s)` приводит к ошибке компиляции
- Shadowing — если есть поле `s`, pattern variable `s` вызовет ошибку; используйте другое имя
- Использовать pattern matching в `catch` — не поддерживается

### Как используется в 2026

- Стандартная практика; старый стиль с явным cast считается устаревшим
- Комбинируется с Pattern Matching для switch (Java 21) для мощной деконструкции типов
- IDE автоматически предлагает рефакторинг старого кода

> **На собеседовании:** покажите рефакторинг `equals()` с pattern matching — это убедительный практический пример. Важно знать про flow scoping (переменная доступна только в scope, где instanceof гарантированно true) и ограничение с `||`.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Records — что это и зачем?
<!-- grade: junior -->

Record — компактный способ объявления классов-носителей неизменяемых данных (data carriers). Компилятор автоматически генерирует конструктор, accessor-методы, `equals()`, `hashCode()` и `toString()`.

> **Аналогия из жизни:** record — как бланк паспорта: поля фиксированы (имя, дата рождения, номер), нельзя добавить новые, нельзя изменить заполненные. Вы просто указываете, какие данные нужны, а формат генерируется автоматически.

<details><summary>Пример: Record vs обычный класс</summary>

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

</details>

### Использование

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
        value = value.toLowerCase().strip();
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
```

### Ограничения Records

| Ограничение | Причина |
|-------------|---------|
| Нельзя наследоваться от другого класса | Неявно наследует `java.lang.Record` |
| Нельзя объявлять instance-поля | Только компоненты записи |
| Все поля final и private | Неизменяемость по контракту |
| Record implicitly final | Нельзя наследоваться от record |
| Нельзя как JPA Entity | Entity требует no-arg constructor и мутабельность |

### Частые ошибки

- Мутабельные поля в Record — `record Data(List<String> items)` — список мутабельный; делайте defensive copy: `this.items = List.copyOf(items)`
- Record как JPA Entity — нельзя; Record можно как DTO или `@Embeddable`
- Ожидать `getX()` вместо `x()` — Jackson и другие библиотеки поддерживают record accessors, но legacy-код может ожидать JavaBean-стиль

### Как используется в 2026

- Records — стандарт для DTO, Value Objects, API responses/requests
- Широкая поддержка: Jackson, Spring, Hibernate (Embeddable), MapStruct
- В комбинации с Sealed Interfaces — алгебраические типы данных
- Record Patterns (Java 21) — деконструкция records в switch/instanceof

> **На собеседовании:** не забудьте упомянуть compact constructor (валидация без явного присвоения полей), accessor без get-префикса (`x()` вместо `getX()`), и невозможность использования как JPA Entity. Частая ошибка — не знать про defensive copy для мутабельных компонентов.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Text Blocks (многострочные строки)
<!-- grade: junior -->

Text Blocks (финализированы в Java 15) — многострочные строковые литералы, сохраняющие форматирование. Начинаются с `"""` и переноса строки, отступ определяется позицией закрывающих `"""`.

<details><summary>Пример: JSON, SQL, HTML</summary>

```java
// До Java 15 — конкатенация
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

// HTML с форматированием
String html = """
        <html>
            <body>
                <h1>Hello, %s!</h1>
            </body>
        </html>
        """.formatted("World");
```

</details>

### Управление отступами и escape-последовательности

```java
// Отступ определяется позицией закрывающих """
String s1 = """
        Hello"""; // нет завершающего \n

String s2 = """
        Hello
        """; // есть завершающий \n

// Trailing whitespace удаляется; для сохранения — \s
String s3 = """
        line with trailing space\s
        """;

// \: line continuation (Java 14+, без переноса строки)
String longLine = """
        This is a very long line that \
        continues on the same line""";
// -> "This is a very long line that continues on the same line"
```

### Частые ошибки

- Неожиданные отступы — если закрывающие `"""` не выровнены, результат содержит лишние пробелы
- Text Block в аннотациях — не поддерживается в `@Query` до Spring Data 3.x (нужна Java 17+)
- `\n` внутри Text Block — Text Block сам добавляет переносы; явный `\n` удвоит перенос

### Как используется в 2026

- Стандарт для встроенных SQL, JSON, HTML, XML, YAML в Java-коде
- Широко используется в `@Query` (Spring Data), тестах, конфигурации

> **На собеседовании:** покажите, что понимаете правило определения отступа (по позиции закрывающих `"""`), и знаете про `\s` для сохранения пробелов. Частая ошибка — путать поведение отступов.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Switch Expressions
<!-- grade: junior -->

Switch Expressions (финализированы в Java 14) превращают `switch` из statement в expression, возвращающее значение. Стрелочный синтаксис `->` исключает fall-through, `yield` возвращает значение из блока.

### Старый vs новый switch

| Свойство | Старый switch | Switch Expression |
|----------|--------------|-------------------|
| Тип | Statement | Expression (возвращает значение) |
| Fall-through | Да (нужен `break`) | Нет |
| Несколько значений | Каскад `case:` | `case A, B ->` |
| Возврат из блока | `break` | `yield` |
| Exhaustiveness | Не проверяется | Проверяется для enum/sealed |

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

// Блок с yield
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    default -> {
        String s = day.toString();
        yield s.length(); // yield вместо return
    }
};
```

### Частые ошибки

- Забыть `yield` в блоке — `return` не работает в switch expression; используйте `yield`
- Смешивание `->` и `:` — в одном switch нельзя комбинировать стрелочный и двоеточный стиль
- `default` для sealed/enum — скрывает ошибки при добавлении нового варианта; компилятор не предупредит

### Как используется в 2026

- Switch expression — стандартный стиль; старый `switch` с `break` — legacy
- Комбинируется с Pattern Matching (Java 21) для мощных деконструкций
- IDE автоматически предлагает рефакторинг старого `switch` в expression

> **На собеседовании:** ключевые моменты — отсутствие fall-through, `yield` вместо `return` в блоке, exhaustiveness check для enum/sealed. Частая ошибка — путать `yield` и `return`.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Удалённые и устаревшие API в Java 17
<!-- grade: junior -->

При миграции на Java 17 критически важно знать, какие API удалены и чем их заменить. Главное изменение для enterprise — переход с `javax.*` на `jakarta.*`.

| Удалено/Deprecated | Версия | Замена |
|--------------------|--------|--------|
| JAXB (`javax.xml.bind`) | Удалено в Java 11 | `jakarta.xml.bind:jakarta.xml.bind-api` + `org.glassfish.jaxb:jaxb-runtime` |
| JAX-WS (`javax.xml.ws`) | Удалено в Java 11 | `jakarta.xml.ws:jakarta.xml.ws-api` |
| CORBA | Удалено в Java 11 | Нет прямой замены |
| Nashorn JS Engine | Удалено в Java 15 | GraalJS |
| RMI Activation | Удалено в Java 17 | -- |
| Security Manager | Deprecated for removal в Java 17 | -- |
| Applet API | Deprecated for removal в Java 17 | -- |
| `finalize()` | Deprecated for removal в Java 18 | `Cleaner`, try-with-resources |
| `javax.*` пакеты | Переименованы (Jakarta EE 9+) | `jakarta.*` |

### Миграция javax -> jakarta

```java
// До (Java EE):
import javax.persistence.Entity;
import javax.servlet.http.HttpServlet;

// После (Jakarta EE):
import jakarta.persistence.Entity;
import jakarta.servlet.http.HttpServlet;
```

### Частые ошибки

- Не обновить зависимости при миграции — `javax.persistence` -> `jakarta.persistence` требует обновления Spring Boot, Hibernate, и всех Jakarta EE зависимостей
- Использовать `finalize()` — ненадёжный, медленный, deprecated; `AutoCloseable` + try-with-resources

### Как используется в 2026

- Все современные фреймворки используют `jakarta.*`
- Инструменты миграции: OpenRewrite recipes для автоматической замены `javax` -> `jakarta`

> **На собеседовании:** обязательно упомяните переход `javax` -> `jakarta` как основное изменение при миграции на Spring Boot 3.x. Интервьюер может спросить, как автоматизировать миграцию — ответ: OpenRewrite.

[к оглавлению](#новое-в-java-от-11-до-25)

---

# Java 21 (LTS, сентябрь 2023)

## Какие ключевые нововведения появились в Java 21?
<!-- grade: junior -->

Java 21 — третий LTS, самый значимый релиз со времён Java 8. Ключевые финализированные фичи: Virtual Threads, Pattern Matching для switch, Record Patterns, Sequenced Collections.

### Нововведения по версиям (Java 18-21)

| Версия | Нововведение |
|--------|-------------|
| Java 18 | UTF-8 по умолчанию, Simple Web Server |
| Java 19 | Virtual Threads (preview), Structured Concurrency (incubator) |
| Java 20 | Scoped Values (incubator), Record Patterns (preview) |
| Java 21 | Virtual Threads (final), Pattern Matching для switch (final), Record Patterns (final), Sequenced Collections, String Templates (preview) |

```java
// Простой HTTP-сервер (Java 18+, для разработки)
// Запуск: jwebserver --port 8000 --directory /path/to/site

// Unnamed patterns (Java 21)
if (obj instanceof Point(var x, _)) { // _ — неиспользуемый компонент
    System.out.println("x = " + x);
}
```

### Частые ошибки

- Путать Java 21 features и preview features — String Templates были preview в 21, удалены в 23
- Не использовать Virtual Threads сразу — Spring Boot 3.2+ поддерживает одной строкой конфига

### Как используется в 2026

- Java 21 — основная версия для новых проектов
- Spring Boot 3.2+ полностью поддерживает Virtual Threads
- Миграция с Java 17 на 21 обычно безболезненна

> **На собеседовании:** назовите тройку главных фич: Virtual Threads, Pattern Matching для switch, Sequenced Collections. Покажите, что понимаете: Virtual Threads — для I/O-bound, Pattern Matching — для type-safe обработки, Sequenced Collections — для единообразного API.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Что такое Virtual Threads (Project Loom)?
<!-- grade: middle -->

Virtual Threads — лёгкие потоки, управляемые JVM, а не операционной системой. Позволяют создавать миллионы потоков без значительных затрат памяти, решая проблему масштабирования блокирующего I/O.

> **Аналогия из жизни:** platform threads — это такси (каждая машина стоит дорого, их количество ограничено), virtual threads — это велосипеды (дешёвые, можно выдать каждому). Когда велосипедист останавливается на светофоре (I/O), велосипед не занимает дорогу — другие могут проехать.

### Platform Threads vs Virtual Threads

| Критерий | Platform Thread | Virtual Thread |
|----------|----------------|----------------|
| Стек | ~1 MB (фиксированный) | ~несколько KB (динамический) |
| Создание | Дорого (syscall) | Дёшево (объект JVM) |
| Количество | Тысячи | Миллионы |
| Планирование | ОС | JVM (ForkJoinPool) |
| Блокирующий I/O | Блокирует поток ОС | Отпускает carrier thread |
| CPU-bound | Эффективен | Не даёт преимущества |

<details><summary>Пример: создание и использование Virtual Threads</summary>

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

</details>

### Когда использовать

| Сценарий | Рекомендация |
|----------|-------------|
| I/O-bound (HTTP, БД, файлы) | Virtual Threads |
| Высокая конкурентность (тысячи соединений) | Virtual Threads |
| Замена реактивного стека | Virtual Threads (для 80% сценариев) |
| CPU-bound вычисления | Platform Threads / ForkJoinPool |
| Код с `synchronized` + I/O (pinning) | Заменить на `ReentrantLock` |

### Частые ошибки

- Пул Virtual Threads — `Executors.newFixedThreadPool()` с Virtual Threads бессмысленно; используйте `newVirtualThreadPerTaskExecutor()`
- ThreadLocal с Virtual Threads — при миллионе потоков ThreadLocal потребляет огромное количество памяти; используйте ScopedValues
- `synchronized` с I/O — pinning; замените на `ReentrantLock`
- Ожидать ускорение CPU-bound задач — Virtual Threads не добавляют ядра

### Как используется в 2026

- Spring Boot 3.2+ — `spring.threads.virtual.enabled=true` для MVC на Virtual Threads
- Tomcat, Jetty, Undertow поддерживают Virtual Threads
- JDBC-драйверы (PostgreSQL, MySQL) работают без pinning (заменили `synchronized` на `ReentrantLock`)
- Virtual Threads + блокирующий код = альтернатива WebFlux для 80% сценариев

> **На собеседовании:** объясните проблему platform threads (1 MB стек, лимит тысячи потоков), как virtual threads решают её (несколько KB, миллионы потоков), и обязательно упомяните pinning (synchronized + I/O блокирует carrier thread). Частая ошибка — сказать, что Virtual Threads ускоряют CPU-bound задачи.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Structured Concurrency — что это и зачем?
<!-- grade: middle -->

Structured Concurrency — API для управления группами связанных задач как единым целым. Гарантирует, что подзадачи завершатся (или будут отменены) до завершения родительской задачи, предотвращая утечки потоков.

> **Аналогия из жизни:** как руководитель проекта, который не уходит домой, пока все члены команды не сдали свои части работы. Если один провалил задачу — остальным сообщают, что проект отменён, и они тоже прекращают работу.

<details><summary>Пример: ShutdownOnFailure и ShutdownOnSuccess</summary>

```java
// Проблема: без Structured Concurrency
ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
Future<User> userFuture = executor.submit(() -> fetchUser(id));
Future<List<Order>> ordersFuture = executor.submit(() -> fetchOrders(id));
// Если fetchOrders бросит исключение — fetchUser продолжает выполняться!
// Если основной поток прервётся — подзадачи «утекают»

// Решение: Structured Concurrency
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Subtask<User> user = scope.fork(() -> fetchUser(id));
    Subtask<List<Order>> orders = scope.fork(() -> fetchOrders(id));

    scope.join();           // ждём завершения обеих задач
    scope.throwIfFailed();  // бросить исключение, если одна из задач упала

    return new UserWithOrders(user.get(), orders.get());
}
// При ошибке одной задачи — другая отменяется автоматически

// ShutdownOnSuccess — вернуть первый успешный результат
try (var scope = new StructuredTaskScope.ShutdownOnSuccess<String>()) {
    scope.fork(() -> fetchFromPrimary());
    scope.fork(() -> fetchFromFallback());
    scope.join();
    return scope.result(); // первый успешный
}
```

</details>

### Стратегии

| Стратегия | Поведение | Когда использовать |
|-----------|-----------|-------------------|
| `ShutdownOnFailure` | Отменить все при первой ошибке | Параллельные зависимые запросы |
| `ShutdownOnSuccess` | Вернуть первый успешный результат | Racing, fallback |

### Частые ошибки

- Не вызывать `join()` — без `join()` результаты недоступны и подзадачи могут утечь
- Использовать вне try-with-resources — scope должен быть закрыт для гарантии завершения
- Путать с `CompletableFuture.allOf()` — Structured Concurrency проще и безопаснее для fork-join паттерна

### Как используется в 2026

- Structured Concurrency финализирован и активно используется с Virtual Threads
- Заменяет ручное управление `Future` + `ExecutorService` для параллельных запросов
- В Spring-приложениях — для параллельного вызова нескольких микросервисов

> **На собеседовании:** объясните проблему (утечка подзадач при ошибке или отмене), решение (жизненный цикл подзадач привязан к scope), и две стратегии. Частая ошибка — не понимать, зачем нужен try-with-resources для scope.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Scoped Values — замена ThreadLocal?
<!-- grade: middle -->

Scoped Values — механизм для передачи иммутабельных данных через стек вызовов без явной передачи параметров. Альтернатива `ThreadLocal`, разработанная специально для Virtual Threads — не создаёт копию на каждый поток и автоматически ограничивает время жизни.

### ThreadLocal vs ScopedValue

| Критерий | ThreadLocal | ScopedValue |
|----------|------------|-------------|
| Мутабельность | Мутабельный (`set()`) | Immutable (только `where().run()`) |
| Время жизни | Пока не вызван `remove()` | Автоматически — ограничен `run()`/`call()` |
| Наследование | `InheritableThreadLocal` (копия) | Автоматическое наследование в child-scope |
| Память | Копия на каждый поток | Разделяемая ссылка |
| Virtual Threads | Проблема (миллион копий = OOM) | Оптимально |

```java
// ThreadLocal — проблемы с Virtual Threads:
private static final ThreadLocal<User> CURRENT_USER = new ThreadLocal<>();
CURRENT_USER.set(user);
// ... где-то в стеке вызовов:
User u = CURRENT_USER.get();
CURRENT_USER.remove(); // легко забыть!

// ScopedValue — решение:
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

### Частые ошибки

- Вызвать `get()` вне `run()` — `NoSuchElementException`; ScopedValue доступен только в scope
- Использовать ThreadLocal с миллионами Virtual Threads — OOM; мигрируйте на ScopedValue
- ScopedValue для мутабельного состояния — ScopedValue immutable; для мутабельного состояния нужен другой подход

### Как используется в 2026

- ScopedValue финализирован в Java 25
- Используется для propagation authentication context, trace ID, request metadata
- Spring Security и Micrometer начинают поддержку ScopedValue для Virtual Threads

> **На собеседовании:** объясните три проблемы ThreadLocal с Virtual Threads (мутабельность, утечки, память) и как ScopedValue решает каждую. Частая ошибка — не знать про автоматическое наследование в child-scope при Structured Concurrency.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Pattern Matching для switch
<!-- grade: middle -->

Pattern Matching для switch (финализирован в Java 21) позволяет использовать type patterns, guarded patterns и null-обработку в switch. В сочетании с sealed types обеспечивает exhaustive check без default.

<details><summary>Пример: type patterns, guard clauses, null</summary>

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

</details>

### Null-обработка в switch

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

### Частые ошибки

- Неправильный порядок patterns — `case String s` перед `case String s when s.length() > 10` делает guarded pattern недостижимым
- Забыть `null` case — если `null` не обработан и нет `default`, NPE как раньше
- `default` с sealed types — скрывает ошибки при добавлении нового подтипа

### Как используется в 2026

- Стандартная практика для обработки разных типов, состояний, событий
- Sealed + Records + Pattern Matching = визитор без Visitor pattern
- Активно используется в domain logic и event handling

> **На собеседовании:** покажите пример с sealed types и exhaustive switch — это самый мощный use case. Обязательно упомяните `when` для guard clauses и возможность обработки null. Частая ошибка — неправильный порядок patterns (более специфичные должны быть выше).

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Record Patterns (деконструкция записей)
<!-- grade: middle -->

Record Patterns (финализированы в Java 21) позволяют деконструировать (разложить) Record на компоненты прямо в pattern matching, включая вложенную деконструкцию.

<details><summary>Пример: деконструкция в instanceof и switch</summary>

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

</details>

### Частые ошибки

- Деконструировать не-Record — Record Patterns работают только с Records
- Забыть, что деконструкция создаёт копии — значения копируются, а не ссылаются на оригинал

### Как используется в 2026

- В связке с Sealed Interfaces + Records — элегантный pattern matching без instanceof-каскадов
- Заменяет Visitor pattern для обработки иерархий типов

> **На собеседовании:** продемонстрируйте вложенную деконструкцию (Record внутри Record) и unnamed patterns (`_`). Это показывает глубокое понимание, а не поверхностное знание синтаксиса.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Sequenced Collections
<!-- grade: junior -->

Sequenced Collections (Java 21) — новые интерфейсы, добавляющие единообразный доступ к первому/последнему элементу и реверсу для всех упорядоченных коллекций через методы `getFirst()`, `getLast()`, `reversed()`.

> **Аналогия из жизни:** до Java 21 у каждой очереди в магазине были разные правила: в одной спрашиваешь «кто первый?», в другой — «номер 1», в третьей — вообще не узнаешь. Sequenced Collections — единые правила для всех очередей.

```java
// Проблема до Java 21:
// List:          list.get(0), list.get(list.size()-1)
// Deque:         deque.getFirst(), deque.getLast()
// SortedSet:     set.first(), set.last()
// LinkedHashSet: ??? (только через итератор)

// Java 21 — единый API:
List<String> list = new ArrayList<>(List.of("A", "B", "C"));
list.getFirst();  // "A"
list.getLast();   // "C"
list.reversed().forEach(System.out::println); // C, B, A

// LinkedHashSet — теперь с getFirst/getLast
LinkedHashSet<String> set = new LinkedHashSet<>(List.of("X", "Y", "Z"));
set.getFirst(); // "X"
set.getLast();  // "Z"
```

### Иерархия интерфейсов

```
                  Collection
                      |
              SequencedCollection
               /            \
        List              SequencedSet
                           /       \
                     SortedSet   LinkedHashSet
```

### Частые ошибки

- `getFirst()` на пустой коллекции — `NoSuchElementException`; проверяйте `isEmpty()`
- Мутация через `reversed()` view — изменения в reversed view влияют на оригинальную коллекцию
- `addFirst()` / `addLast()` для unmodifiable — `UnsupportedOperationException`

### Как используется в 2026

- Sequenced Collections — часть стандартного API, используется повсеместно
- Заменяет нечитаемые конструкции вроде `list.get(list.size() - 1)`

> **На собеседовании:** покажите, что знаете проблему (разный API для первого/последнего элемента в List, Deque, SortedSet) и решение (единый интерфейс SequencedCollection). Упомяните, что `reversed()` возвращает view, а не копию.

[к оглавлению](#новое-в-java-от-11-до-25)

---

# Java 25 (LTS, сентябрь 2025)

## Какие ключевые нововведения в Java 25?
<!-- grade: junior -->

Java 25 — четвёртый LTS, в котором финализированы многие preview-фичи из Java 22-24: Flexible Constructor Bodies, Module Import Declarations, Primitive Patterns, Scoped Values, Structured Concurrency, Stream Gatherers.

### Нововведения по версиям (Java 22-25)

| Версия | Нововведение |
|--------|-------------|
| Java 22 | Unnamed Variables `_` (final), Stream Gatherers (preview), Statements before super() (preview) |
| Java 23 | Primitive Types in Patterns (preview), Module Import Declarations (preview), Markdown doc comments |
| Java 24 | Stream Gatherers (final), Compact Object Headers (experimental), Ahead-of-Time compilation |
| Java 25 | Flexible Constructor Bodies (final), Module Import Declarations (final), Primitive Patterns (final), Compact Object Headers, Scoped Values (final), Structured Concurrency (final) |

### Ключевые финализированные фичи

1. Flexible Constructor Bodies — код до `super()` / `this()`
2. Module Import Declarations — `import module java.base`
3. Primitive Types in Patterns — примитивы в switch/instanceof
4. Scoped Values — замена ThreadLocal для Virtual Threads
5. Structured Concurrency — управление параллельными задачами
6. Stream Gatherers — пользовательские промежуточные операции в Stream API
7. Compact Object Headers — уменьшение размера заголовка объекта с 12 до 8 байт

### Частые ошибки

- Мигрировать на Java 25 без проверки зависимостей — некоторые библиотеки могут не поддерживать; проверяйте Lombok, byte-code manipulation
- Использовать preview features из 22-24, которые могли измениться — preview API нестабильно

### Как используется в 2026

- Spring Boot 3.4+ поддерживает Java 25
- Ранние adopters уже используют Java 25 в продакшене
- Java 21 остаётся безопасным выбором для консервативных проектов

> **На собеседовании:** перечислите финализированные фичи и подчеркните, что Java 25 завершает экосистему Virtual Threads (Scoped Values + Structured Concurrency). Compact Object Headers — интересный ответ для senior-уровня (10-15% экономии памяти).

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Flexible Constructor Bodies
<!-- grade: middle -->

Flexible Constructor Bodies (финализировано в Java 25) позволяют выполнять код до вызова `super()` или `this()` в конструкторе — валидацию аргументов, подготовку параметров, логирование. Ранее `super()`/`this()` обязательно были первым оператором.

```java
// До Java 25 — super() обязан быть первым
public class PositiveInteger extends Number {
    public PositiveInteger(int value) {
        // if (value <= 0) throw ...; // НЕЛЬЗЯ до super()!
        super();
        if (value <= 0) throw new IllegalArgumentException("Must be positive");
        this.value = value;
    }
}

// Java 25 — код до super()
public class PositiveInteger extends Number {
    public PositiveInteger(int value) {
        if (value <= 0) {
            throw new IllegalArgumentException("Must be positive: " + value);
        }
        super(); // теперь может быть не первым
        this.value = value;
    }
}
```

### Практический пример — подготовка аргументов для super()

```java
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

### Ограничения кода до super()

| Можно | Нельзя |
|-------|--------|
| Валидировать аргументы | Обращаться к `this` |
| Подготовить параметры для super() | Вызывать instance-методы |
| Логировать | Присваивать instance-поля |
| Вычислять локальные переменные | Читать instance-поля |

### Частые ошибки

- Обращение к `this` до `super()` — ошибка компиляции; объект не инициализирован
- Инициализация полей до `super()` — невозможно; `this.field = value` до `super()` запрещено

### Как используется в 2026

- Упрощает конструкторы — не нужны static factory methods для валидации перед super()
- Особенно полезно для Records с валидацией и наследования

> **На собеседовании:** объясните проблему (раньше приходилось выносить валидацию в static-методы или делать её после super()), решение (код до super()) и ограничение (нет доступа к this). Вопрос показывает, что кандидат следит за новыми версиями Java.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Module Import Declarations и упрощённый импорт
<!-- grade: junior -->

Module Import Declarations (финализировано в Java 25) позволяют импортировать все пакеты модуля одной строкой `import module`, вместо множества отдельных import-ов.

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
```

<details><summary>Пример: программа с import module и implicit main</summary>

```java
// Программа с import module
import module java.base;

public class Demo {
    public static void main(String[] args) {
        var numbers = List.of(1, 2, 3, 4, 5);
        var sum = numbers.stream()
            .filter(n -> n % 2 == 0)
            .mapToInt(Integer::intValue)
            .sum();
        System.out.println("Sum of evens: " + sum);

        var today = LocalDate.now(); // тоже доступен
    }
}

// Без какого-либо main() boilerplate (Implicitly Declared Classes, Java 22+)
void main() {
    println("Hello, World!");
    // java.base импортирован автоматически
    // println() доступен без System.out
}
```

</details>

### Частые ошибки

- Конфликты имён — `import module java.base` и `import module java.desktop` оба содержат `List`; нужен явный `import java.util.List`
- В production-коде — `import module` может усложнить читаемость для других разработчиков

### Как используется в 2026

- Для скриптов, обучения, quick prototyping
- Production-код обычно использует явные импорты (IDE поддерживает автоматически)

> **На собеседовании:** скажите, что `import module` упрощает обучение и скрипты, но в production рекомендуются явные импорты. Упомяните проблему конфликтов имён и implicit main для простых программ.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Primitive Types in Patterns и instanceof
<!-- grade: middle -->

Primitive Types in Patterns (финализировано в Java 25) расширяют pattern matching на примитивные типы, проверяя безопасность преобразования (narrowing) без потери данных.

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

### Частые ошибки

- Ожидать автоматический narrowing cast — `long instanceof int` проверяет допустимость, а не выполняет приведение; значение должно быть в диапазоне int

### Как используется в 2026

- Упрощает обработку числовых типов в generic-коде
- Дополняет картину полного pattern matching для всех типов Java

> **На собеседовании:** ключевой момент — `instanceof` для примитивов проверяет, помещается ли значение без потери данных, а не просто приводит тип. Покажите пример с `long instanceof int` для 42L (true) и Long.MAX_VALUE (false).

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Что нового в Stream API от Java 11 до Java 25?
<!-- grade: middle -->

Stream API эволюционировал от добавления `toList()` (Java 16) и `mapMulti()` до полноценных расширяемых промежуточных операций через Stream Gatherers (Java 24/25).

### Эволюция Stream API

| Версия | Нововведение | Описание |
|--------|-------------|----------|
| Java 16 | `toList()` | Неизменяемый список, замена `collect(Collectors.toList())` |
| Java 16 | `mapMulti()` | Замена flatMap для простых случаев (0-N результатов) |
| Java 24/25 | Stream Gatherers | Пользовательские промежуточные операции |

<details><summary>Пример: toList(), mapMulti()</summary>

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
```

</details>

### Stream Gatherers (Java 24/25)

Gatherers позволяют создавать stateful промежуточные операции — то, что ранее было невозможно стандартными средствами Stream API.

```java
// Встроенные Gatherers:
// windowFixed — фиксированные окна
List<List<Integer>> windows = Stream.of(1, 2, 3, 4, 5, 6)
    .gather(Gatherers.windowFixed(3))
    .toList(); // [[1, 2, 3], [4, 5, 6]]

// windowSliding — скользящее окно
List<List<Integer>> sliding = Stream.of(1, 2, 3, 4, 5)
    .gather(Gatherers.windowSliding(3))
    .toList(); // [[1, 2, 3], [2, 3, 4], [3, 4, 5]]

// scan — промежуточные результаты свёртки
List<Integer> runningSum = Stream.of(1, 2, 3, 4, 5)
    .gather(Gatherers.scan(() -> 0, Integer::sum))
    .toList(); // [1, 3, 6, 10, 15]

// mapConcurrent — параллельная обработка с Virtual Threads
List<String> results = urls.stream()
    .gather(Gatherers.mapConcurrent(10, url -> fetchUrl(url)))
    .toList(); // до 10 параллельных запросов
```

<details><summary>Пример: custom Gatherer — distinctBy</summary>

```java
// Пример: distinctBy — unique по ключу
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

</details>

### Частые ошибки

- Мутация списка из `toList()` — `UnsupportedOperationException`; если нужен мутабельный — `collect(Collectors.toList())`
- `mapConcurrent` для CPU-bound — Virtual Threads не ускоряют CPU-bound; используйте `parallelStream()`
- Custom Gatherer без proper state management — Gatherer может быть parallel; state должен быть thread-safe или sequential

### Как используется в 2026

- `toList()` — повсеместно заменяет `collect(Collectors.toList())`
- Gatherers — новый инструмент для сложных потоковых обработок (окна, группировки, stateful)
- `mapConcurrent` — удобная параллелизация I/O-задач в stream

> **На собеседовании:** обязательно знайте разницу `toList()` vs `collect(Collectors.toList())` (иммутабельность). Для middle+ уровня — расскажите про Gatherers: windowFixed/windowSliding для оконных операций и mapConcurrent для параллельного I/O с Virtual Threads.

[к оглавлению](#новое-в-java-от-11-до-25)

---

# Кросс-версионные вопросы

## Какую версию Java выбрать для нового проекта в 2026 году?
<!-- grade: middle -->

Для нового проекта в 2026 году рекомендуется Java 25 (или Java 21 для консервативного подхода) — обе версии LTS с полной поддержкой Spring Boot 3.x и Virtual Threads.

### Рекомендации по выбору версии

| Сценарий | Рекомендуемая версия | Обоснование |
|----------|---------------------|-------------|
| Новый проект, greenfield | Java 25 (или 21) | Все современные фичи, LTS |
| Корпоративный, консервативный | Java 21 | Зрелая LTS, полная поддержка Spring Boot 3.x |
| Существующий на Java 17 | Java 21 (миграция) | Virtual Threads, Pattern Matching |
| Существующий на Java 11 | Java 21 (миграция) | Java 11 end-of-life |
| Существующий на Java 8 | Java 21 (миграция) | Критически важно мигрировать |

### Дистрибутивы JDK в 2026

| Дистрибутив | Вендор | Бесплатный | Поддержка |
|-------------|--------|-----------|-----------|
| Eclipse Temurin | Adoptium | Да | Community |
| Amazon Corretto | Amazon | Да | Amazon |
| Azul Zulu | Azul | Да (Community) | Azul |
| Oracle JDK | Oracle | Нет (NFTC license) | Oracle |
| GraalVM CE | Oracle | Да | Community |
| BellSoft Liberica | BellSoft | Да | BellSoft |

### Частые ошибки

- Стартовать новый проект на Java 11 или 17 — упускаете Virtual Threads, Pattern Matching, Records
- Использовать Oracle JDK без лицензии — с 2019 Oracle JDK требует подписку для коммерческого использования
- Не фиксировать версию JDK в проекте — используйте `.sdkmanrc`, `toolchains.xml` или `jenv`

### Как используется в 2026

- Java 21 — доминирующая версия в продакшене
- Java 25 — набирает обороты, ранние adopters уже используют
- Toolchains (Gradle/Maven) — позволяют собирать проект на конкретной версии JDK

> **На собеседовании:** ответ «Java 21 или 25, только LTS для продакшена» — базовый минимум. Добавьте факторы выбора: совместимость зависимостей, поддержка вендора, уровень команды. Упомяните, что Eclipse Temurin или Amazon Corretto — наиболее популярные бесплатные дистрибутивы.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Как мигрировать проект с Java 8/11 на Java 21/25?
<!-- grade: senior -->

Миграция с Java 8/11 на Java 21/25 выполняется поэтапно: сначала обновление JDK и зависимостей, затем переход javax -> jakarta (для Spring Boot 3.x), и наконец включение новых возможностей. OpenRewrite автоматизирует до 80% работы.

### Этап 1: Подготовка

```bash
# Проверить совместимость зависимостей
./mvnw versions:display-dependency-updates
./mvnw versions:display-plugin-updates

# Ключевые зависимости для обновления:
# Spring Boot 2.x -> 3.x (требует Java 17+)
# Hibernate 5.x -> 6.x
# Lombok -> последняя версия
# Mockito -> 5.x+
# Jackson -> 2.15+
```

### Этап 2: javax -> jakarta (при переходе на Spring Boot 3.x)

<details><summary>OpenRewrite конфигурация</summary>

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

</details>

### Этап 3: Основные изменения при миграции

| С версии | Изменение | Действие |
|----------|-----------|----------|
| Java 8 -> 11 | Удалены JAXB, JAX-WS | Добавить Jakarta зависимости |
| Java 8 -> 11 | Module system (JPMS) | Добавить `--add-opens` если нужен reflective access |
| Java 11 -> 17 | Сильная инкапсуляция internal API | `--add-opens` или заменить internal API |
| Java 17 -> 21 | javax -> jakarta | OpenRewrite |
| Any -> 21 | SecurityManager удалён | Убрать использование |

### Этап 4: Включение новых возможностей

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

### Частые ошибки

- Мигрировать всё сразу — мигрируйте поэтапно: JDK -> фреймворки -> рефакторинг кода
- Не обновить Lombok — Lombok сильно зависит от internal API JDK; каждая мажорная версия Java требует обновления
- Не проверить reflection-зависимости — библиотеки, использующие `sun.misc.Unsafe` или internal API, могут сломаться
- Игнорировать deprecation warnings — deprecated API могут быть удалены в следующей версии

### Как используется в 2026

- OpenRewrite — стандартный инструмент для автоматической миграции
- Большинство enterprise-проектов завершили миграцию на Java 17/21
- Миграция с Java 21 на 25 — минимальные изменения (в основном обновление зависимостей)

> **На собеседовании:** покажите, что знаете поэтапный подход (JDK -> зависимости -> javax/jakarta -> рефакторинг) и инструмент автоматизации (OpenRewrite). Частая ошибка — пытаться обновить всё одним коммитом. Для senior-уровня упомяните `--add-opens` как временное решение и стратегию отказа от него.

[к оглавлению](#новое-в-java-от-11-до-25)

---

## Эволюция null-safety в Java
<!-- grade: middle -->

Java не имеет встроенной null-safety на уровне типов (в отличие от Kotlin `?`), но постепенно добавляет инструменты: `Optional` (Java 8+), Helpful NPE (Java 14+), null в Pattern Matching (Java 21+), аннотации `@Nullable`/`@NonNull`.

### Optional — эволюция API по версиям

| Версия | Новый API | Описание |
|--------|-----------|----------|
| Java 8 | `Optional.of/ofNullable/empty` | Базовый Optional |
| Java 9 | `ifPresentOrElse()`, `or()`, `stream()` | Расширенный API |
| Java 10 | `orElseThrow()` без аргументов | Замена `get()` |
| Java 11 | `isEmpty()` | Инверсия `isPresent()` |
| Java 21 | Pattern Matching + null в switch | Альтернативный подход к null |

<details><summary>Пример: эволюция Optional и null handling</summary>

```java
// Java 8 — базовый Optional
Optional<String> name = Optional.ofNullable(user.getName());
String result = name.orElse("Unknown");

// Java 9 — ifPresentOrElse, or
optional.ifPresentOrElse(
    value -> process(value),
    () -> handleAbsence()
);
Optional<String> fallback = primary.or(() -> secondary);

// Java 10 — orElseThrow() без аргументов
String value = optional.orElseThrow(); // вместо optional.get()

// Java 11 — Optional.isEmpty()
if (optional.isEmpty()) { ... } // вместо !optional.isPresent()

// Java 21+ — Pattern Matching с null:
switch (value) {
    case null -> handleNull();
    case String s -> process(s);
    default -> other();
}
```

</details>

### Helpful NullPointerException (Java 14+)

```java
var street = user.getAddress().getCity().toUpperCase();
// До Java 14: NullPointerException (непонятно, что null)
// Java 14+:   Cannot invoke "City.toUpperCase()" because
//             the return value of "Address.getCity()" is null
```

### Аннотации @Nullable / @NonNull

```java
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

### Частые ошибки

- Optional как параметр метода — anti-pattern; используйте перегрузку методов или `@Nullable`
- Optional как поле класса — Optional не Serializable; используйте `@Nullable` для полей
- `optional.get()` без проверки — может бросить `NoSuchElementException`; используйте `orElseThrow()`, `orElse()`, `ifPresent()`
- Игнорировать `@Nullable` аннотации — IDE подсвечивает потенциальные NPE; не игнорируйте warnings

### Как используется в 2026

- `Optional` — стандарт для возвращаемых значений (особенно в repository/service слоях)
- `@Nullable`/`@NonNull` аннотации — best practice; Spring использует `@NonNullApi` для всего пакета
- JSpecify (`org.jspecify`) — новый стандарт null-safety аннотаций, набирает популярность
- Pattern Matching + null handling в switch — удобная обработка nullable значений

> **На собеседовании:** покажите, что знаете правила использования Optional (возвращаемые значения — да, параметры и поля — нет), Helpful NPE для отладки, и аннотации для статического анализа. Частая ошибка — вызывать `optional.get()` без проверки вместо `orElseThrow()`.

[к оглавлению](#новое-в-java-от-11-до-25)
