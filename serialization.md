[Вопросы для собеседования](README.md)

# Сериализация
+ [Что такое _«сериализация»_?](#Что-такое-сериализация)
+ [Опишите процесс сериализации/десериализации с использованием `Serializable`.](#Опишите-процесс-сериализациидесериализации-с-использованием-serializable)
+ [Как изменить стандартное поведение сериализации/десериализации?](#Как-изменить-стандартное-поведение-сериализациидесериализации)
+ [Как исключить поля из сериализации?](#Как-исключить-поля-из-сериализации)
+ [Что обозначает ключевое слово `transient`?](#Что-обозначает-ключевое-слово-transient)
+ [Какое влияние оказывают на сериализуемость модификаторы полей `static` и `final`](#Какое-влияние-оказывают-на-сериализуемость-модификаторы-полей-static-и-final)
+ [Как не допустить сериализацию?](#Как-не-допустить-сериализацию)
+ [Как создать собственный протокол сериализации?](#Как-создать-собственный-протокол-сериализации)
+ [Какая роль поля `serialVersionUID` в сериализации?](#Какая-роль-поля-serialversionuid-в-сериализации)
+ [Когда стоит изменять значение поля `serialVersionUID`?](#Когда-стоит-изменять-значение-поля-serialversionuid)
+ [В чем проблема сериализации Singleton?](#В-чем-проблема-сериализации-singleton)
+ [Какие существуют способы контроля за значениями десериализованного объекта](#Какие-существуют-способы-контроля-за-значениями-десериализованного-объекта)
+ [Что такое Jackson и как он работает?](#Что-такое-jackson-и-как-он-работает)
+ [Как кастомизировать сериализацию в Jackson?](#Как-кастомизировать-сериализацию-в-jackson)
+ [Чем отличается Jackson от Gson?](#Чем-отличается-jackson-от-gson)
+ [Как работать с JSON в Java без библиотек и с Jakarta JSON-P/JSON-B?](#Как-работать-с-json-в-java-без-библиотек-и-с-jakarta-json-pjson-b)
+ [Java Serialization vs JSON: когда что использовать?](#Java-serialization-vs-json-когда-что-использовать)

## Что такое _«сериализация»_?
__Сериализация (Serialization)__ - процесс преобразования структуры данных в линейную последовательность байтов для дальнейшей передачи или сохранения. Сериализованные объекты можно затем восстановить (десериализовать).

В Java, согласно спецификации Java Object Serialization существует два стандартных способа сериализации: стандартная сериализация, через использование интерфейса `java.io.Serializable` и «расширенная» сериализация - `java.io.Externalizable`.

Сериализация позволяет в определенных пределах изменять класс. Вот наиболее важные изменения, с которыми спецификация Java Object Serialization может справляться автоматически:

+ добавление в класс новых полей;
+ изменение полей из статических в нестатические;
+ изменение полей из транзитных в нетранзитные.

Обратные изменения (из нестатических полей в статические и из нетранзитных в транзитные) или удаление полей требуют определенной дополнительной обработки в зависимости от того, какая степень обратной совместимости необходима.

[к оглавлению](#Сериализация)

## Опишите процесс сериализации/десериализации с использованием `Serializable`.
При использовании Serializable применяется алгоритм сериализации, который с помощью рефлексии (Reflection API) выполняет:

+ запись в поток метаданных о классе, ассоциированном с объектом (имя класса, идентификатор `SerialVersionUID`, идентификаторы полей класса);
+ рекурсивную запись в поток описания суперклассов до класса `java.lang.Object` (не включительно);
+ запись примитивных значений полей сериализуемого экземпляра, начиная с полей самого верхнего суперкласса;
+ рекурсивную запись объектов, которые являются полями сериализуемого объекта.

При этом ранее сериализованные объекты повторно не сериализуются, что позволяет алгоритму корректно работать с циклическими ссылками.

Для выполнения десериализации под объект выделяется память, после чего его поля заполняются значениями из потока. Конструктор объекта при этом не вызывается. Однако при десериализации будет вызван конструктор без параметров родительского несериализуемого класса, а его отсутствие повлечёт ошибку десериализации.

[к оглавлению](#Сериализация)

## Как изменить стандартное поведение сериализации/десериализации?
+ Реализовать интерфейс `java.io.Externalizable`, который позволяет применение пользовательской логики сериализации. Способ сериализации и десериализации описывается в методах `writeExternal()` и `readExternal()`. Во время десериализации вызывается конструктор без параметров, а потом уже на созданном объекте вызывается метод `readExternal`.
+ Если у сериализуемого объекта реализован один из следующих методов, то механизм сериализации будет использовать его, а не метод по умолчанию :
    + `writeObject()` - запись объекта в поток;
    + `readObject()` - чтение объекта из потока;
    + `writeReplace()` - позволяет заменить себя экземпляром другого класса перед записью;
    + `readResolve()` - позволяет заменить на себя другой объект после чтения.

[к оглавлению](#Сериализация)

## Как исключить поля из сериализации?
Для управления сериализацией при определении полей можно использовать ключевое слово `transient`, таким образом исключив поля из общего процесса сериализации.

[к оглавлению](#Сериализация)

## Что обозначает ключевое слово `transient`?
Поля класса, помеченные модификатором `transient`, не сериализуются.

Обычно в таких полях хранится промежуточное состояние объекта, которое, к примеру, проще вычислить. Другой пример такого поля - ссылка на экземпляр объекта, который не требует сериализации или не может быть сериализован.

[к оглавлению](#Сериализация)

## Какое влияние оказывают на сериализуемость модификаторы полей `static` и `final`
При стандартной сериализации поля, имеющие модификатор static, не сериализуются. Соответственно, после десериализации это поле значения не меняет. При использовании реализации `Externalizable` сериализовать и десериализовать статическое поле можно, но не рекомендуется этого делать, т.к. это может сопровождаться трудноуловимыми ошибками.

Поля с модификатором `final` сериализуются как и обычные. За одним исключением – их невозможно десериализовать при использовании `Externalizable`, поскольку `final` поля должны быть инициализированы в конструкторе, а после этого в `readExternal()` изменить значение этого поля будет невозможно. Соответственно, если необходимо сериализовать объект с `final` полем необходимо использовать только стандартную сериализацию.

[к оглавлению](#Сериализация)

## Как не допустить сериализацию?
Чтобы не допустить автоматическую сериализацию можно переопределить `private` методы для создания исключительной ситуации `NotSerializableException`.

```java
private void writeObject(ObjectOutputStream out) throws IOException {
    throw new NotSerializableException();
}

private void readObject(ObjectInputStream in) throws IOException {
    throw new NotSerializableException();
}
```

Любая попытка записать или прочитать этот объект теперь приведет к возникновению исключительной ситуации.

[к оглавлению](#Сериализация)

## Как создать собственный протокол сериализации?
Для создания собственного протокола сериализации достаточно реализовать интерфейс `Externalizable`, который содержит два метода:

```java
public void writeExternal(ObjectOutput out) throws IOException;
public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
```

[к оглавлению](#Сериализация)

## Какая роль поля `serialVersionUID` в сериализации?
`serialVersionUID` используется для указания версии сериализованных данных. 

Когда мы не объявляем `serialVersionUID` в нашем классе явно, среда выполнения Java делает это за нас, но этот процесс чувствителен ко многим метаданным класса включая количество полей, тип полей, модификаторы доступа полей, интерфейсов, которые реализованы в классе и пр. 

Рекомендуется явно объявлять `serialVersionUID` т.к. при добавлении, удалении атрибутов класса динамически сгенерированное значение может измениться и в момент выполнения будет выброшено исключение `InvalidClassException`.

```java
private static final long serialVersionUID = 20161013L;
```
[к оглавлению](#Сериализация)

## Когда стоит изменять значение поля `serialVersionUID`?
`serialVersionUID` нужно изменять при внесении в класс несовместимых изменений, например при удалении какого-либо его атрибута.

[к оглавлению](#Сериализация)

## В чем проблема сериализации Singleton?
Проблема в том что после десериализации мы получим другой объект. Таким образом, сериализация дает возможность создать Singleton еще раз, что недопустимо. Существует два способа избежать этого:

+ явный запрет сериализации.
+ определение метода с сигнатурой `(default/public/private/protected/) Object readResolve() throws ObjectStreamException`, назначением которого станет возврат замещающего объекта вместо объекта, на котором он вызван.

[к оглавлению](#Сериализация)

## Какие существуют способы контроля за значениями десериализованного объекта
Если есть необходимость выполнения контроля за значениями десериализованного объекта, то можно использовать интерфейс `ObjectInputValidation` с переопределением метода `validateObject()`.

```java
// Если вызвать метод validateObject() после десериализации объекта, то будет вызвано исключение InvalidObjectException при значении возраста за пределами 39...60.
public class Person implements java.io.Serializable,
                               java.io.ObjectInputValidation {
    ...
    @Override
    public void validateObject() throws InvalidObjectException {
        if ((age < 39) || (age > 60))
            throw new InvalidObjectException("Invalid age");
    }
}
```

Так же существуют способы подписывания и шифрования, позволяющие убедиться, что данные не были изменены:

+ с помощью описания логики в `writeObject()` и `readObject()`.

+ поместить в оберточный класс `javax.crypto.SealedObject` и/или `java.security.SignedObject`. Данные классы являются сериализуемыми, поэтому при оборачивании объекта в `SealedObject` создается подобие «подарочной упаковки» вокруг исходного объекта. Для шифрования необходимо создать симметричный ключ, управление которым должно осуществляться отдельно. Аналогично, для проверки данных можно использовать класс `SignedObject`, для работы с которым также нужен симметричный ключ, управляемый отдельно.

[к оглавлению](#Сериализация)

## Что такое Jackson и как он работает?
__Jackson__ — самая популярная библиотека для работы с JSON в Java-экосистеме. Она используется по умолчанию в Spring Boot, Quarkus и многих других фреймворках. Центральный класс — `ObjectMapper`, который выполняет сериализацию (Java-объект → JSON) и десериализацию (JSON → Java-объект).

### ObjectMapper: базовое использование

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;

public class JacksonBasicExample {
    public static void main(String[] args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();

        // Конфигурация
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
        mapper.registerModule(new JavaTimeModule());

        // Сериализация: объект → JSON
        User user = new User("Иван", "ivan@example.com", 30);
        String json = mapper.writeValueAsString(user);
        // {"name":"Иван","email":"ivan@example.com","age":30}

        // Красивый вывод
        String prettyJson = mapper.writerWithDefaultPrettyPrinter()
                                  .writeValueAsString(user);

        // Десериализация: JSON → объект
        String inputJson = """
            {"name":"Мария","email":"maria@example.com","age":25}
            """;
        User restored = mapper.readValue(inputJson, User.class);

        // Десериализация коллекций
        String arrayJson = """
            [{"name":"Иван","age":30},{"name":"Мария","age":25}]
            """;
        List<User> users = mapper.readValue(arrayJson,
            new TypeReference<List<User>>() {});
    }
}
```

### Основные аннотации

```java
import com.fasterxml.jackson.annotation.*;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;
import java.time.LocalDate;

public class Employee {

    // Явное задание имени JSON-поля
    @JsonProperty("full_name")
    private String name;

    // Исключение поля из сериализации/десериализации
    @JsonIgnore
    private String internalCode;

    // Форматирование даты
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd.MM.yyyy")
    private LocalDate hireDate;

    // Не включать поле, если значение null
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private String department;

    // Альтернативные имена при десериализации
    @JsonAlias({"email_address", "emailAddr"})
    @JsonProperty("email")
    private String email;

    // Конструктор для десериализации
    public Employee() {}

    // Getters/Setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getInternalCode() { return internalCode; }
    public void setInternalCode(String internalCode) { this.internalCode = internalCode; }
    public LocalDate getHireDate() { return hireDate; }
    public void setHireDate(LocalDate hireDate) { this.hireDate = hireDate; }
    public String getDepartment() { return department; }
    public void setDepartment(String department) { this.department = department; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

### Стратегия именования полей `@JsonNaming`

```java
import com.fasterxml.jackson.databind.PropertyNamingStrategies;
import com.fasterxml.jackson.databind.annotation.JsonNaming;

// Все поля будут в snake_case: firstName → first_name
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class ApiResponse {
    private String firstName;   // → "first_name"
    private String lastName;    // → "last_name"
    private int totalCount;     // → "total_count"

    // Конструктор, getters, setters...
}
```

### Модуль JavaTimeModule для LocalDate/LocalDateTime

```java
ObjectMapper mapper = new ObjectMapper();
mapper.registerModule(new JavaTimeModule());
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

// Без JavaTimeModule сериализация LocalDate вызовет ошибку.
// С модулем: {"date":"2026-04-22"}
```

Зависимость Maven:
```xml
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.17.0</version>
</dependency>
```

### Важное
+ `ObjectMapper` — потокобезопасный после конфигурации; создавайте один экземпляр и переиспользуйте.
+ `FAIL_ON_UNKNOWN_PROPERTIES = false` — критически важная настройка для обратной совместимости API.
+ `JavaTimeModule` обязателен для работы с `java.time.*`; без него Jackson не знает, как сериализовать `LocalDate`, `LocalDateTime` и т.д.
+ `TypeReference` нужен для десериализации generic-типов (`List<User>`, `Map<String, Object>`), так как Java стирает generic-информацию в рантайме.

### Частые ошибки
+ Создание нового `ObjectMapper` на каждый запрос — снижает производительность.
+ Забытый `JavaTimeModule` при использовании `LocalDate`/`LocalDateTime` — получите `InvalidDefinitionException`.
+ Отсутствие конструктора без аргументов — Jackson не сможет создать объект при десериализации (если не использовать `@JsonCreator`).
+ `FAIL_ON_UNKNOWN_PROPERTIES = true` (по умолчанию) — приводит к ошибкам при добавлении новых полей в API.

### Как используется в 2026
Jackson остаётся стандартом де-факто в Java-экосистеме. Spring Boot 3.x, Quarkus, Micronaut — все используют Jackson по умолчанию. Версия Jackson 2.17+ поддерживает `record`-классы Java, Virtual Threads (Project Loom) не требуют особых настроек. GraalVM Native Image полностью совместим с Jackson при правильной конфигурации рефлексии.

[к оглавлению](#Сериализация)

## Как кастомизировать сериализацию в Jackson?
Jackson предоставляет несколько механизмов для тонкой настройки процесса сериализации и десериализации: пользовательские сериализаторы/десериализаторы, аннотации для управления созданием объектов и mix-ins для работы с классами, которые нельзя модифицировать.

### Custom Serializer

```java
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.SerializerProvider;
import com.fasterxml.jackson.databind.ser.std.StdSerializer;

public class MoneySerializer extends StdSerializer<Money> {

    public MoneySerializer() {
        super(Money.class);
    }

    @Override
    public void serialize(Money value, JsonGenerator gen,
                          SerializerProvider provider) throws IOException {
        // Выводим деньги в формате "100.50 RUB"
        gen.writeString(value.getAmount().toPlainString() + " " + value.getCurrency());
    }
}
```

### Custom Deserializer

```java
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.deser.std.StdDeserializer;

public class MoneyDeserializer extends StdDeserializer<Money> {

    public MoneyDeserializer() {
        super(Money.class);
    }

    @Override
    public Money deserialize(JsonParser p, DeserializationContext ctxt)
            throws IOException {
        String text = p.getText(); // "100.50 RUB"
        String[] parts = text.split(" ");
        return new Money(new BigDecimal(parts[0]), parts[1]);
    }
}
```

### Применение через аннотации `@JsonSerialize` / `@JsonDeserialize`

```java
public class Order {
    private String id;

    @JsonSerialize(using = MoneySerializer.class)
    @JsonDeserialize(using = MoneyDeserializer.class)
    private Money totalPrice;

    // getters/setters...
}
```

### `@JsonCreator` для иммутабельных объектов

```java
public class Address {
    private final String city;
    private final String street;
    private final int zipCode;

    @JsonCreator
    public Address(
            @JsonProperty("city") String city,
            @JsonProperty("street") String street,
            @JsonProperty("zipCode") int zipCode) {
        this.city = city;
        this.street = street;
        this.zipCode = zipCode;
    }

    // Только getters, объект иммутабельный
    public String getCity() { return city; }
    public String getStreet() { return street; }
    public int getZipCode() { return zipCode; }
}
```

### `@JsonValue` — объект сериализуется как одно значение

```java
public enum Status {
    ACTIVE("active"),
    INACTIVE("inactive");

    private final String code;

    Status(String code) { this.code = code; }

    @JsonValue
    public String getCode() { return code; }

    @JsonCreator
    public static Status fromCode(String code) {
        return Arrays.stream(values())
                .filter(s -> s.code.equals(code))
                .findFirst()
                .orElseThrow();
    }
}
// Сериализация: "active" вместо "ACTIVE"
```

### `@JsonUnwrapped` — развёртывание вложенного объекта

```java
public class Person {
    private String name;

    @JsonUnwrapped
    private Address address;
    // Вместо {"name":"Иван","address":{"city":"Москва","street":"Тверская"}}
    // Получим: {"name":"Иван","city":"Москва","street":"Тверская"}
}
```

### Mix-ins для сторонних классов

Mix-ins позволяют добавить аннотации Jackson к классам, исходный код которых нельзя изменить (например, из внешних библиотек).

```java
// Сторонний класс, который нельзя модифицировать
// public class ThirdPartyUser {
//     private String name;
//     private String secret;
//     ...
// }

// Mix-in — абстрактный класс с аннотациями
public abstract class ThirdPartyUserMixin {
    @JsonProperty("username")
    abstract String getName();

    @JsonIgnore
    abstract String getSecret();
}

// Регистрация mix-in
ObjectMapper mapper = new ObjectMapper();
mapper.addMixIn(ThirdPartyUser.class, ThirdPartyUserMixin.class);
```

### Важное
+ `StdSerializer` / `StdDeserializer` — предпочтительнее прямой реализации `JsonSerializer` / `JsonDeserializer`, так как предоставляют удобные утилиты.
+ `@JsonCreator` — единственный способ десериализации иммутабельных объектов без конструктора по умолчанию (кроме Java `record`, которые Jackson поддерживает «из коробки»).
+ Mix-ins — ключевой механизм для интеграции Jackson с legacy-кодом или сторонними библиотеками.
+ Регистрация кастомных сериализаторов возможна также через `SimpleModule`, что удобнее при большом их количестве.

### Частые ошибки
+ Забытая аннотация `@JsonProperty` в параметрах `@JsonCreator` — без неё Jackson не знает, какой JSON-параметр передать в какой аргумент.
+ `@JsonUnwrapped` не работает с коллекциями и `Map` — только с обычными POJO.
+ Конфликт между `@JsonValue` и другими аннотациями — `@JsonValue` имеет приоритет и определяет единственное представление объекта.
+ Применение `@JsonSerialize` на уровне класса вместо поля — влияет на все случаи использования этого типа.

### Как используется в 2026
Java `record`-классы (Java 16+) автоматически поддерживаются Jackson 2.15+, что делает `@JsonCreator` менее нужным для простых DTO. В Spring Boot 3.x кастомизация Jackson чаще всего выполняется через `Jackson2ObjectMapperBuilderCustomizer` бин. Kotlin-проекты используют `jackson-module-kotlin` для поддержки data-классов и nullable-типов.

[к оглавлению](#Сериализация)

## Чем отличается Jackson от Gson?
Jackson и Gson — две основные библиотеки для работы с JSON в Java. Обе решают одну задачу, но различаются в архитектуре, производительности и экосистеме.

### Сравнительная таблица

| Критерий | Jackson | Gson |
|---|---|---|
| Разработчик | FasterXML (open-source) | Google |
| Производительность | Быстрее (особенно Streaming API) | Медленнее на больших объёмах |
| Размер библиотеки | Больше (модульная структура) | Компактная (~300 KB) |
| Streaming API | Да (`JsonParser` / `JsonGenerator`) | Да (`JsonReader` / `JsonWriter`) |
| Tree Model | `JsonNode` | `JsonElement` / `JsonObject` |
| Аннотации | Богатый набор (`@JsonProperty`, `@JsonCreator`, ...) | Минимальный (`@SerializedName`, `@Expose`) |
| Поддержка модулей | Да (JavaTimeModule, Kotlin, etc.) | Нет модульной системы |
| Spring Boot | По умолчанию | Требует ручной настройки |
| Null-обработка | Не пишет null по умолчанию (настраивается) | Не пишет null по умолчанию |
| record (Java 16+) | Полная поддержка | Поддержка с 2.10+ |

### Пример: Jackson vs Gson

```java
// === Jackson ===
ObjectMapper jackson = new ObjectMapper();
String jacksonJson = jackson.writeValueAsString(user);
User fromJackson = jackson.readValue(jacksonJson, User.class);

// Tree Model
JsonNode node = jackson.readTree(jacksonJson);
String name = node.get("name").asText();

// === Gson ===
Gson gson = new Gson();
String gsonJson = gson.toJson(user);
User fromGson = gson.fromJson(gsonJson, User.class);

// Tree Model
JsonElement element = JsonParser.parseString(gsonJson);
String name2 = element.getAsJsonObject().get("name").getAsString();
```

### Пример: кастомизация в Gson

```java
Gson gson = new GsonBuilder()
    .setPrettyPrinting()
    .setDateFormat("dd.MM.yyyy")
    .serializeNulls()
    .excludeFieldsWithoutExposeAnnotation()
    .registerTypeAdapter(Money.class, new MoneyTypeAdapter())
    .create();
```

### Когда использовать Jackson
+ В любом Spring/Spring Boot проекте — он уже подключён.
+ Когда нужна высокая производительность при обработке больших JSON.
+ Когда нужен богатый набор аннотаций и модулей.
+ В enterprise-проектах с развитой экосистемой.

### Когда использовать Gson
+ В Android-проектах (хотя Moshi и kotlinx.serialization вытесняют Gson).
+ В небольших утилитах, где важен минимальный размер зависимостей.
+ Когда нужна простая библиотека без сложной конфигурации.

### Важное
+ Jackson — стандарт в серверной Java-разработке; Gson — исторически популярен в Android.
+ Производительность Jackson заметно выше при работе с большими JSON-документами за счёт Streaming API.
+ `JsonNode` (Jackson) — мощнее и удобнее, чем `JsonElement` (Gson) для навигации по дереву.
+ Обе библиотеки потокобезопасны: `ObjectMapper` и `Gson` можно переиспользовать.

### Частые ошибки
+ Смешивание аннотаций Jackson и Gson в одном проекте — они не совместимы друг с другом.
+ Использование Gson в Spring Boot — приводит к конфликтам с Jackson, который уже в classpath.
+ Предположение, что Gson быстрее из-за меньшего размера — это не так, Jackson быстрее в бенчмарках.
+ Ручное построение JSON через конкатенацию строк вместо использования любой из библиотек.

### Как используется в 2026
Jackson доминирует в серверной Java. Gson остаётся в legacy-проектах и некоторых Android-приложениях, но уступает позиции kotlinx.serialization (Kotlin Multiplatform) и Moshi. В новых проектах на чистом Kotlin предпочтительнее kotlinx.serialization благодаря compile-time генерации кода и поддержке Kotlin-типов.

[к оглавлению](#Сериализация)

## Как работать с JSON в Java без библиотек и с Jakarta JSON-P/JSON-B?
В Java EE (и теперь Jakarta EE) существуют два стандартных API для работы с JSON: __JSON-P__ (JSON Processing) для низкоуровневого парсинга и __JSON-B__ (JSON Binding) для привязки к объектам. Оба входят в спецификацию Jakarta EE, но на практике Jackson используется значительно чаще.

### JSON-P (Jakarta JSON Processing)
JSON-P предоставляет API для парсинга, генерации и трансформации JSON. Работает на двух уровнях: потоковом (`JsonParser`/`JsonGenerator`) и объектном (`JsonObject`/`JsonArray`).

```java
import jakarta.json.Json;
import jakarta.json.JsonObject;
import jakarta.json.JsonArray;
import jakarta.json.JsonReader;
import jakarta.json.JsonWriter;
import java.io.StringReader;
import java.io.StringWriter;

public class JsonPExample {
    public static void main(String[] args) {
        // Построение JSON-объекта
        JsonObject json = Json.createObjectBuilder()
                .add("name", "Иван")
                .add("age", 30)
                .add("skills", Json.createArrayBuilder()
                        .add("Java")
                        .add("Spring")
                        .build())
                .build();

        // Запись в строку
        StringWriter sw = new StringWriter();
        try (JsonWriter writer = Json.createWriter(sw)) {
            writer.writeObject(json);
        }
        System.out.println(sw.toString());
        // {"name":"Иван","age":30,"skills":["Java","Spring"]}

        // Чтение из строки
        String input = """
            {"name":"Мария","age":25}
            """;
        try (JsonReader reader = Json.createReader(new StringReader(input))) {
            JsonObject obj = reader.readObject();
            String name = obj.getString("name");  // "Мария"
            int age = obj.getInt("age");           // 25
        }
    }
}
```

### JSON-B (Jakarta JSON Binding)
JSON-B — аналог Jackson/Gson в мире Jakarta EE. Выполняет автоматическую привязку JSON к Java-объектам.

```java
import jakarta.json.bind.Jsonb;
import jakarta.json.bind.JsonbBuilder;
import jakarta.json.bind.JsonbConfig;
import jakarta.json.bind.annotation.JsonbProperty;
import jakarta.json.bind.annotation.JsonbTransient;
import jakarta.json.bind.annotation.JsonbDateFormat;
import java.time.LocalDate;

public class Employee {
    @JsonbProperty("full_name")
    private String name;

    @JsonbTransient
    private String internalCode;

    @JsonbDateFormat("dd.MM.yyyy")
    private LocalDate hireDate;

    // getters/setters...
}

public class JsonBExample {
    public static void main(String[] args) {
        JsonbConfig config = new JsonbConfig()
                .withFormatting(true)
                .withNullValues(false);

        try (Jsonb jsonb = JsonbBuilder.create(config)) {
            Employee emp = new Employee();
            emp.setName("Иван");
            emp.setHireDate(LocalDate.of(2026, 1, 15));

            // Сериализация
            String json = jsonb.toJson(emp);

            // Десериализация
            Employee restored = jsonb.fromJson(json, Employee.class);
        }
    }
}
```

### Сравнение подходов

| Критерий | JSON-P | JSON-B | Jackson |
|---|---|---|---|
| Уровень | Низкоуровневый (парсинг) | Высокоуровневый (binding) | Оба |
| Стандарт | Jakarta EE | Jakarta EE | Де-факто стандарт |
| Гибкость | Высокая | Средняя | Очень высокая |
| Экосистема | Ограниченная | Ограниченная | Огромная (модули) |
| Spring Boot | Не по умолчанию | Не по умолчанию | По умолчанию |

### Важное
+ JSON-P и JSON-B — часть спецификации Jakarta EE; они гарантируют переносимость между серверами приложений (WildFly, Payara, Open Liberty).
+ JSON-P полезен, когда нужно работать с JSON без привязки к конкретным Java-классам (динамическая структура).
+ JSON-B имеет аннотации, аналогичные Jackson (`@JsonbProperty` ≈ `@JsonProperty`, `@JsonbTransient` ≈ `@JsonIgnore`).
+ В большинстве проектов вне Jakarta EE серверов применение JSON-P/JSON-B не оправдано — Jackson проще и мощнее.

### Частые ошибки
+ Путаница между `javax.json` (старый Java EE) и `jakarta.json` (новый Jakarta EE) — пакеты несовместимы.
+ Попытка использовать JSON-P для сложного маппинга — это низкоуровневый API, для маппинга нужен JSON-B или Jackson.
+ Отсутствие реализации в classpath — JSON-P/JSON-B это API (интерфейсы), нужна реализация (Eclipse Parsson для JSON-P, Eclipse Yasson для JSON-B).
+ Смешивание аннотаций JSON-B и Jackson в одном классе — они не взаимозаменяемы.

### Как используется в 2026
JSON-P и JSON-B остаются частью Jakarta EE 11. На серверах приложений (WildFly, Open Liberty) они доступны «из коробки». Однако даже в Jakarta EE проектах многие команды предпочитают Jackson из-за большей экосистемы и привычности. В микросервисных фреймворках (Spring Boot, Quarkus, Micronaut) Jackson по-прежнему стандартный выбор.

[к оглавлению](#Сериализация)

## Java Serialization vs JSON: когда что использовать?
Стандартная Java-сериализация (`Serializable`) и JSON-сериализация (Jackson, Gson) решают схожую задачу — преобразование объектов в переносимый формат, — но кардинально различаются по области применения, безопасности и совместимости.

### Сравнительная таблица

| Критерий | Java Serialization | JSON (Jackson и др.) | Бинарные форматы (Protobuf, Avro) |
|---|---|---|---|
| Читаемость | Нет (бинарный формат) | Да (текстовый) | Нет (бинарный) |
| Кросс-языковость | Только Java | Любой язык | Любой язык (кодогенерация) |
| Производительность | Средняя | Средняя | Высокая |
| Размер данных | Большой (метаданные классов) | Средний | Маленький |
| Эволюция схемы | Хрупкая (`serialVersionUID`) | Гибкая (новые поля игнорируются) | Встроенная поддержка |
| Безопасность | Опасная (атаки десериализации) | Безопасная (при правильной настройке) | Безопасная |
| REST API | Не применимо | Стандарт | Используется в gRPC |
| Типизация | Полная (включая классы) | Ограниченная (нужна явная) | Строгая (через схему) |

### Почему Java Serialization опасна

```java
// ОПАСНО: десериализация данных из непроверенного источника
// может привести к Remote Code Execution (RCE)!
ObjectInputStream ois = new ObjectInputStream(untrustedInput);
Object obj = ois.readObject(); // Злоумышленник может выполнить произвольный код

// Атака основана на "gadget chains" — цепочках вызовов в
// библиотеках (Apache Commons Collections, Spring и др.),
// которые при десериализации выполняют произвольный код.
```

С Java 9 появились фильтры десериализации (`ObjectInputFilter`), но они не решают проблему полностью:

```java
ObjectInputFilter filter = ObjectInputFilter.Config.createFilter(
    "com.myapp.**;!*"  // Разрешаем только свои классы
);
ObjectInputStream ois = new ObjectInputStream(input);
ois.setObjectInputFilter(filter);
```

### Когда использовать JSON

```java
// REST API — всегда JSON (или другой текстовый формат)
@RestController
public class UserController {

    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
        // Spring автоматически сериализует в JSON через Jackson
    }

    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // Spring автоматически десериализует JSON через Jackson
        return userService.save(user);
    }
}
```

### Когда использовать бинарные форматы (Protobuf, Avro)

```protobuf
// user.proto — схема Protocol Buffers
syntax = "proto3";

message User {
    int64 id = 1;
    string name = 2;
    string email = 3;
}
```

```java
// Использование Protobuf в Java (сгенерированный код)
User user = User.newBuilder()
    .setId(1L)
    .setName("Иван")
    .setEmail("ivan@example.com")
    .build();

byte[] bytes = user.toByteArray();           // Сериализация
User restored = User.parseFrom(bytes);       // Десериализация
```

### Когда что выбрать

+ __JSON__ — REST API, конфигурационные файлы, межсервисное взаимодействие по HTTP, хранение в NoSQL-базах (MongoDB, Elasticsearch), логирование.
+ __Protobuf / gRPC__ — высоконагруженное межсервисное взаимодействие, когда важна производительность и размер сообщений.
+ __Avro__ — потоковая обработка данных (Kafka, Spark), эволюция схем в event-driven архитектуре.
+ __Java Serialization__ — практически не используется в новых проектах. Может встретиться в legacy-коде, RMI, или глубокой сериализации сложных графов объектов.

### Важное
+ Java Serialization НЕ рекомендуется для новых проектов — это позиция Oracle, сообщества и OWASP.
+ JSON — универсальный формат для API и межсистемного обмена; читаемость и кросс-языковость перевешивают потерю в размере.
+ Бинарные форматы (Protobuf, Avro) выбирают при жёстких требованиях к производительности и размеру данных.
+ `ObjectInputFilter` (Java 9+) — обязательный минимум, если Java Serialization всё же используется.

### Частые ошибки
+ Использование Java Serialization для передачи данных между сервисами — невозможно прочитать на стороне, написанной не на Java.
+ Десериализация Java-объектов из непроверенных источников без фильтров — критическая уязвимость (CWE-502).
+ Хранение сериализованных Java-объектов в базе данных — невозможно мигрировать при изменении классов.
+ Выбор JSON для внутренних высоконагруженных коммуникаций, где Protobuf/Avro дали бы кратный прирост производительности.
+ Предположение, что JSON безопасен «автоматически» — полиморфная десериализация в Jackson (`@JsonTypeInfo`) тоже может быть вектором атаки при неправильной настройке.

### Как используется в 2026
Java Serialization фактически устарела. JEP по её deprecation обсуждается с Java 17+. В новых проектах JSON (через Jackson) — стандарт для REST и HTTP-взаимодействия. Для высокопроизводительных сценариев используют Protobuf/gRPC (межсервисная коммуникация) и Avro (Kafka, event streaming). Формат JSON уступает только в embedded/IoT-сценариях, где применяют CBOR или MessagePack (оба поддерживаются Jackson).

[к оглавлению](#Сериализация)

# Источники
+ [IBM developerWorks](https://www.ibm.com/developerworks/ru/library/j-5things1/)
+ [Java-online.ru](http://java-online.ru/blog-serialization.xhtml)
+ [Изучите секреты Java Serialization API](http://ccfit.nsu.ru/~deviv/courses/oop/java_ser_rus.html)
+ [JavaRush](http://bit.ly/1xwRA2D)
+ [Записки трезвого практика](http://www.skipy.ru/technics/serialization.html)

[Вопросы для собеседования](README.md)
