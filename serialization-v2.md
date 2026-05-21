[Вопросы для собеседования](README.md)

# Сериализация

+ [Что такое сериализация?](#что-такое-сериализация)
+ [Опишите процесс сериализации/десериализации с использованием Serializable](#опишите-процесс-сериализациидесериализации-с-использованием-serializable)
+ [Как изменить стандартное поведение сериализации/десериализации?](#как-изменить-стандартное-поведение-сериализациидесериализации)
+ [Как исключить поля из сериализации?](#как-исключить-поля-из-сериализации)
+ [Что обозначает ключевое слово transient?](#что-обозначает-ключевое-слово-transient)
+ [Какое влияние оказывают на сериализуемость модификаторы полей static и final?](#какое-влияние-оказывают-на-сериализуемость-модификаторы-полей-static-и-final)
+ [Как не допустить сериализацию?](#как-не-допустить-сериализацию)
+ [Как создать собственный протокол сериализации?](#как-создать-собственный-протокол-сериализации)
+ [Какая роль поля serialVersionUID в сериализации?](#какая-роль-поля-serialversionuid-в-сериализации)
+ [Когда стоит изменять значение поля serialVersionUID?](#когда-стоит-изменять-значение-поля-serialversionuid)
+ [В чем проблема сериализации Singleton?](#в-чем-проблема-сериализации-singleton)
+ [Какие существуют способы контроля за значениями десериализованного объекта?](#какие-существуют-способы-контроля-за-значениями-десериализованного-объекта)
+ [Что такое Jackson и как он работает?](#что-такое-jackson-и-как-он-работает)
+ [Как кастомизировать сериализацию в Jackson?](#как-кастомизировать-сериализацию-в-jackson)
+ [Чем отличается Jackson от Gson?](#чем-отличается-jackson-от-gson)
+ [Как работать с JSON в Java без библиотек и с Jakarta JSON-P/JSON-B?](#как-работать-с-json-в-java-без-библиотек-и-с-jakarta-json-pjson-b)
+ [Java Serialization vs JSON: когда что использовать?](#java-serialization-vs-json-когда-что-использовать)

## Что такое сериализация?
<!-- grade: junior -->

Сериализация — это процесс преобразования структуры данных (объекта) в линейную последовательность байтов для передачи по сети, сохранения в файл или хранения в базе данных. Обратный процесс — десериализация — восстанавливает объект из этой последовательности.

> **Аналогия из жизни:** представьте, что вам нужно переслать собранный шкаф из IKEA другу. Вы не можете запихнуть его целиком в посылку, поэтому разбираете на плоские детали и прикладываете инструкцию по сборке. Друг получает посылку и собирает шкаф обратно. Разборка — сериализация, сборка — десериализация.

В Java существует два стандартных способа сериализации:

- `java.io.Serializable` — стандартная сериализация через рефлексию, минимум кода
- `java.io.Externalizable` — расширенная сериализация с полным контролем, разработчик сам описывает логику записи/чтения

### Совместимые изменения класса

Спецификация Java Object Serialization допускает ряд изменений без потери совместимости:

- добавление в класс новых полей
- изменение полей из статических в нестатические
- изменение полей из транзитных в нетранзитные

Обратные изменения (из нестатических в статические, из нетранзитных в транзитные) или удаление полей требуют дополнительной обработки в зависимости от того, какая степень обратной совместимости необходима.

### Частые ошибки

- Путать сериализацию с маршаллингом — маршаллинг включает передачу кода (codebase), сериализация только данные
- Считать, что сериализация сохраняет поведение — сохраняется только состояние (поля), не методы
- Забывать, что Java-сериализация привязана к платформе — десериализовать на Python/Go не получится

> **На собеседовании:** достаточно дать определение, назвать два интерфейса (`Serializable`, `Externalizable`) и упомянуть, что конструктор при стандартной десериализации не вызывается. Плюсом будет отметить, что Java-сериализация в новых проектах не рекомендуется в пользу JSON/Protobuf.

[к оглавлению](#Сериализация)

---

## Опишите процесс сериализации/десериализации с использованием Serializable
<!-- grade: junior -->

При использовании `Serializable` JVM применяет алгоритм сериализации, который через Reflection API автоматически записывает состояние объекта в поток байтов.

### Алгоритм сериализации

1. Запись метаданных о классе — имя класса, `serialVersionUID`, идентификаторы полей
2. Рекурсивная запись описания суперклассов вплоть до `java.lang.Object` (не включительно)
3. Запись примитивных значений полей, начиная с полей самого верхнего суперкласса
4. Рекурсивная запись объектов, являющихся полями сериализуемого объекта

Ранее сериализованные объекты повторно не записываются — это позволяет корректно обрабатывать циклические ссылки и граф объектов.

### Алгоритм десериализации

1. Под объект выделяется память
2. Поля заполняются значениями из потока
3. Конструктор сериализуемого класса **не вызывается**
4. Вызывается конструктор без параметров ближайшего **несериализуемого** родительского класса

Если у несериализуемого суперкласса нет конструктора без параметров, при десериализации будет выброшено `InvalidClassException`.

<details>
<summary>Пример сериализации/десериализации</summary>

```java
import java.io.*;

public class SerializationExample {
    public static void main(String[] args) throws Exception {
        User user = new User("Иван", 30);

        // Сериализация
        try (ObjectOutputStream oos = new ObjectOutputStream(
                new FileOutputStream("user.ser"))) {
            oos.writeObject(user);
        }

        // Десериализация
        try (ObjectInputStream ois = new ObjectInputStream(
                new FileInputStream("user.ser"))) {
            User restored = (User) ois.readObject();
            System.out.println(restored); // User{name='Иван', age=30}
        }
    }
}

class User implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }
    // getters, toString...
}
```

</details>

### Частые ошибки

- Ожидание вызова конструктора при десериализации — конструктор сериализуемого класса не вызывается, поэтому логика инициализации в нём не выполнится
- Отсутствие `Serializable` у вложенных объектов — если поле класса не реализует `Serializable` и не помечено `transient`, будет `NotSerializableException`
- Забыть про суперкласс — если родитель не `Serializable`, его поля не сохраняются, а при десериализации инициализируются через конструктор по умолчанию

> **На собеседовании:** ключевой момент — конструктор не вызывается при десериализации `Serializable`-объекта, но вызывается конструктор без параметров у несериализуемого суперкласса. Это частый вопрос-ловушка.

[к оглавлению](#Сериализация)

---

## Как изменить стандартное поведение сериализации/десериализации?
<!-- grade: middle -->

Существует два подхода: реализация `Externalizable` для полного контроля или определение специальных методов в `Serializable`-классе для точечной кастомизации.

### Подход 1: интерфейс Externalizable

Позволяет полностью описать логику сериализации и десериализации вручную. Во время десериализации **вызывается** конструктор без параметров, затем на созданном объекте вызывается `readExternal`.

```java
public class User implements Externalizable {
    private String name;
    private int age;

    public User() {} // Обязателен для Externalizable

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeInt(age);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException {
        this.name = in.readUTF();
        this.age = in.readInt();
    }
}
```

### Подход 2: специальные методы в Serializable-классе

Если у класса определены следующие методы, механизм сериализации использует их вместо поведения по умолчанию:

| Метод | Назначение |
|-------|-----------|
| `private void writeObject(ObjectOutputStream out)` | Запись объекта в поток |
| `private void readObject(ObjectInputStream in)` | Чтение объекта из потока |
| `Object writeReplace()` | Подмена объекта перед записью (возвращает замещающий объект) |
| `Object readResolve()` | Подмена объекта после чтения (возвращает замещающий объект) |

### Сравнение подходов

| Критерий | Serializable + методы | Externalizable |
|----------|----------------------|---------------|
| Конструктор при десериализации | Не вызывается | Вызывается (без параметров) |
| Контроль | Частичный (можно дополнить стандартный) | Полный |
| Объём кода | Меньше (по необходимости) | Больше (все поля вручную) |
| `final`-поля | Поддерживаются | Не поддерживаются (нельзя задать после конструктора) |

> **На собеседовании:** назовите оба подхода и подчеркните разницу в вызове конструктора. Упоминание `writeReplace`/`readResolve` — сильный плюс, особенно в контексте Singleton-паттерна.

[к оглавлению](#Сериализация)

---

## Как исключить поля из сериализации?
<!-- grade: junior -->

Для исключения полей из сериализации используется ключевое слово `transient`. Поле, помеченное `transient`, пропускается при стандартной сериализации и при десериализации получает значение по умолчанию для своего типа (`null`, `0`, `false`).

```java
public class User implements Serializable {
    private String name;           // сериализуется
    private transient String password; // НЕ сериализуется
    private transient int cachedHash; // НЕ сериализуется
}
```

### Другие способы исключения

| Способ | Применяется к |
|--------|--------------|
| `transient` | Стандартная Java-сериализация |
| `Externalizable` | Полный контроль — не записываете поле вручную |
| `@JsonIgnore` | Jackson (JSON-сериализация) |
| `@Expose` + `excludeFieldsWithoutExposeAnnotation()` | Gson |
| `@JsonbTransient` | Jakarta JSON-B |

> **На собеседовании:** достаточно назвать `transient` и объяснить, что после десериализации поле будет иметь значение по умолчанию. Если спрашивают про JSON — добавьте `@JsonIgnore`.

[к оглавлению](#Сериализация)

---

## Что обозначает ключевое слово transient?
<!-- grade: junior -->

Модификатор `transient` указывает JVM, что помеченное поле не является частью персистентного состояния объекта и не должно сериализоваться.

Типичные сценарии использования `transient`:

- **Вычисляемые значения** — кэшированный хэш-код, длина коллекции, производные поля, которые проще пересчитать
- **Ссылки на несериализуемые объекты** — подключения к БД, потоки ввода-вывода, логгеры
- **Чувствительные данные** — пароли, токены, ключи шифрования

```java
public class Session implements Serializable {
    private String userId;
    private transient Connection dbConnection;  // нельзя сериализовать
    private transient String authToken;          // не должен покидать JVM
    private transient int requestCount;          // вычисляемое значение
}
```

После десериализации `transient`-поля получают значения по умолчанию: `null` для объектов, `0` для числовых типов, `false` для `boolean`. Если нужно восстановить их в корректное состояние, используйте метод `readObject()`.

> **На собеседовании:** назовите назначение `transient`, приведите пример (пароль или соединение) и упомяните, что после десериализации поле будет `null`/`0`/`false`.

[к оглавлению](#Сериализация)

---

## Какое влияние оказывают на сериализуемость модификаторы полей static и final?
<!-- grade: middle -->

Модификаторы `static` и `final` ведут себя по-разному при стандартной сериализации (`Serializable`) и расширенной (`Externalizable`).

### static-поля

Поля с модификатором `static` **не сериализуются** при стандартной сериализации, так как принадлежат классу, а не экземпляру. После десериализации статическое поле будет содержать текущее значение из JVM, а не то, которое было в момент сериализации.

При использовании `Externalizable` статическое поле можно записать и прочитать вручную, но это **не рекомендуется** — это побочный эффект, влияющий на все экземпляры класса, что ведёт к трудноуловимым ошибкам.

### final-поля

При стандартной сериализации `final`-поля сериализуются и десериализуются как обычные поля — JVM обходит ограничение `final` через внутренние механизмы.

При использовании `Externalizable` `final`-поля **невозможно десериализовать**: конструктор по умолчанию инициализирует их начальным значением, а изменить в `readExternal()` уже нельзя.

### Сводная таблица

| Модификатор | Serializable | Externalizable |
|-------------|-------------|---------------|
| `static` | Не сериализуется | Можно, но не рекомендуется |
| `final` | Сериализуется нормально | Невозможно десериализовать |
| `static final` | Не сериализуется (константа класса) | Можно записать, но бессмысленно |

> **На собеседовании:** ключевое ограничение — `final`-поля нельзя восстановить через `Externalizable`, поэтому если в классе есть `final`-поля, нужно использовать `Serializable`. Это частый вопрос-ловушка.

[к оглавлению](#Сериализация)

---

## Как не допустить сериализацию?
<!-- grade: junior -->

Чтобы запретить сериализацию объекта, нужно переопределить методы `writeObject` и `readObject`, выбрасывающие `NotSerializableException`.

```java
private void writeObject(ObjectOutputStream out) throws IOException {
    throw new NotSerializableException();
}

private void readObject(ObjectInputStream in) throws IOException {
    throw new NotSerializableException();
}
```

Любая попытка сериализовать или десериализовать такой объект приведёт к исключению.

### Другие способы

- **Не реализовывать `Serializable`** — самый простой вариант, но не работает, если суперкласс уже `Serializable`
- **Переопределить `writeObject`/`readObject` с исключением** — работает даже если суперкласс сериализуем
- **Использовать `enum`** — enum-объекты сериализуются JVM особым образом (только имя константы), полный контроль невозможен, но и злонамеренная десериализация тоже

> **На собеседовании:** назовите способ с выбрасыванием `NotSerializableException` и объясните, зачем это нужно — например, класс содержит чувствительные данные или ресурсы, которые не имеет смысла сериализовать.

[к оглавлению](#Сериализация)

---

## Как создать собственный протокол сериализации?
<!-- grade: middle -->

Для создания собственного протокола сериализации нужно реализовать интерфейс `Externalizable`, который содержит два метода — `writeExternal` для записи и `readExternal` для чтения.

```java
public interface Externalizable extends Serializable {
    void writeExternal(ObjectOutput out) throws IOException;
    void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
}
```

Разработчик полностью контролирует, какие данные и в каком формате записываются в поток. Это позволяет оптимизировать размер данных, шифровать содержимое, менять порядок полей.

<details>
<summary>Пример собственного протокола</summary>

```java
public class CompactUser implements Externalizable {
    private String name;
    private int age;
    private List<String> roles;

    public CompactUser() {} // Обязателен

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeByte(age); // byte вместо int — экономия 3 байт
        out.writeShort(roles.size());
        for (String role : roles) {
            out.writeUTF(role);
        }
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException {
        this.name = in.readUTF();
        this.age = in.readByte() & 0xFF;
        int size = in.readShort();
        this.roles = new ArrayList<>(size);
        for (int i = 0; i < size; i++) {
            roles.add(in.readUTF());
        }
    }
}
```

</details>

### Важные особенности

- Порядок чтения в `readExternal` должен строго совпадать с порядком записи в `writeExternal`
- Конструктор без параметров **обязателен** — JVM вызывает его перед `readExternal`
- `final`-поля невозможно заполнить в `readExternal` (поле уже инициализировано в конструкторе)
- `Externalizable` наследует `Serializable`, поэтому объект может передаваться через `ObjectOutputStream`

> **На собеседовании:** упомяните `Externalizable`, покажите два метода и подчеркните, что порядок записи/чтения должен совпадать. Плюсом будет назвать преимущество — полный контроль над форматом и размером данных.

[к оглавлению](#Сериализация)

---

## Какая роль поля serialVersionUID в сериализации?
<!-- grade: junior -->

`serialVersionUID` — это уникальный идентификатор версии класса, который используется при десериализации для проверки совместимости сериализованных данных с текущей версией класса.

```java
private static final long serialVersionUID = 20161013L;
```

### Как работает

При сериализации JVM записывает `serialVersionUID` класса в поток. При десериализации сравнивает `serialVersionUID` из потока с `serialVersionUID` текущего класса. Если значения не совпадают, выбрасывается `InvalidClassException`.

### Почему нужно объявлять явно

Если `serialVersionUID` не объявлен, JVM генерирует его автоматически на основе метаданных класса: имени, полей, модификаторов, реализованных интерфейсов. Любое изменение класса (добавление метода, поля, изменение модификатора) приводит к изменению автоматически сгенерированного `serialVersionUID`, и ранее сериализованные данные становятся несовместимыми.

### Правила формирования

| Подход | Пример | Когда использовать |
|--------|--------|-------------------|
| Фиксированное число | `serialVersionUID = 1L` | Простые случаи, начало разработки |
| Дата/версия | `serialVersionUID = 20261013L` | Когда важна история изменений |
| Автогенерация IDE | `serialVersionUID = -812739472847L` | Когда IDE генерирует по структуре класса |

### Частые ошибки

- Не объявлять `serialVersionUID` явно — при любом рефакторинге класса старые данные перестанут читаться
- Менять `serialVersionUID` при совместимых изменениях — это сломает обратную совместимость без необходимости
- Одинаковый `serialVersionUID` у разных классов — не проблема, так как проверка учитывает и имя класса

> **На собеседовании:** объясните назначение поля, почему его нужно объявлять явно, и когда его следует менять. Достаточно одного предложения: «Без явного `serialVersionUID` JVM генерирует его автоматически, и любое изменение класса ломает обратную совместимость.»

[к оглавлению](#Сериализация)

---

## Когда стоит изменять значение поля serialVersionUID?
<!-- grade: middle -->

`serialVersionUID` следует изменять только при внесении **несовместимых** изменений в класс — таких, после которых старые сериализованные данные не могут корректно восстановиться.

### Несовместимые изменения (нужно менять serialVersionUID)

- Удаление поля
- Изменение типа поля (например, `int` -> `long`)
- Перемещение класса в другой пакет
- Изменение класса с `Serializable` на `Externalizable` или наоборот
- Удаление реализации `Serializable`

### Совместимые изменения (НЕ нужно менять serialVersionUID)

- Добавление нового поля (при десериализации старых данных оно получит значение по умолчанию)
- Добавление/удаление методов
- Изменение модификаторов доступа поля
- Изменение поля из `static` в нестатическое или из `transient` в нетранзитное

> **На собеседовании:** покажите, что понимаете разницу между совместимыми и несовместимыми изменениями. Главный критерий — если старые данные можно корректно загрузить в новую версию класса, `serialVersionUID` менять не нужно.

[к оглавлению](#Сериализация)

---

## В чем проблема сериализации Singleton?
<!-- grade: middle -->

При десериализации JVM создаёт **новый** экземпляр объекта, минуя приватный конструктор, что нарушает контракт Singleton — в системе оказывается два экземпляра вместо одного.

```java
Singleton original = Singleton.getInstance();

// Сериализация → десериализация
Singleton deserialized = (Singleton) ois.readObject();

System.out.println(original == deserialized); // false — два экземпляра!
```

### Решения

**1. Метод readResolve** — возвращает существующий экземпляр вместо десериализованного:

```java
public class Singleton implements Serializable {
    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() { return INSTANCE; }

    private Object readResolve() throws ObjectStreamException {
        return INSTANCE; // Вместо нового объекта возвращаем существующий
    }
}
```

**2. enum-Singleton** — рекомендуемый подход, JVM гарантирует единственность:

```java
public enum Singleton {
    INSTANCE;

    public void doSomething() { /* ... */ }
}
```

Enum-синглтон защищён от десериализации, рефлексии и клонирования «из коробки».

**3. Запрет сериализации** — если сериализация не нужна, выбросить `NotSerializableException`.

> **На собеседовании:** назовите проблему (десериализация создаёт новый экземпляр), опишите решение через `readResolve` и порекомендуйте `enum`-подход как самый надёжный. Это один из самых популярных вопросов на стыке паттернов и сериализации.

[к оглавлению](#Сериализация)

---

## Какие существуют способы контроля за значениями десериализованного объекта?
<!-- grade: middle -->

Контроль за значениями десериализованного объекта нужен для проверки инвариантов, валидации данных и защиты от подмены сериализованного потока.

### 1. Интерфейс ObjectInputValidation

Позволяет зарегистрировать валидацию, которая вызывается после полной десериализации объекта.

```java
public class Person implements Serializable, ObjectInputValidation {
    private String name;
    private int age;

    @Override
    public void validateObject() throws InvalidObjectException {
        if (age < 0 || age > 150) {
            throw new InvalidObjectException("Некорректный возраст: " + age);
        }
        if (name == null || name.isBlank()) {
            throw new InvalidObjectException("Имя не может быть пустым");
        }
    }

    private void readObject(ObjectInputStream in)
            throws IOException, ClassNotFoundException {
        in.defaultReadObject();
        in.registerValidation(this, 0); // регистрация валидации
    }
}
```

### 2. Валидация в readObject

Проверка значений прямо в методе `readObject` — проще, но вызывается до завершения десериализации всего графа объектов.

### 3. Шифрование и подпись

Для защиты от подмены данных можно обернуть объект в защитные обёртки:

| Класс | Назначение |
|-------|-----------|
| `javax.crypto.SealedObject` | Шифрование сериализованного объекта (нужен симметричный ключ) |
| `java.security.SignedObject` | Цифровая подпись для проверки целостности |

Оба класса сами реализуют `Serializable` — оборачивают исходный объект, создавая защищённую «упаковку».

### 4. ObjectInputFilter (Java 9+)

Фильтр десериализации, ограничивающий допустимые классы, глубину графа и размер данных:

```java
ObjectInputFilter filter = ObjectInputFilter.Config.createFilter(
    "com.myapp.**;!*"  // Разрешить только свои классы
);
ois.setObjectInputFilter(filter);
```

> **На собеседовании:** назовите `ObjectInputValidation` как основной механизм валидации и `ObjectInputFilter` (Java 9+) как защиту от атак десериализации. Упоминание `SealedObject`/`SignedObject` — дополнительный плюс.

[к оглавлению](#Сериализация)

---

## Что такое Jackson и как он работает?
<!-- grade: junior -->

Jackson — самая популярная библиотека для работы с JSON в Java-экосистеме. Центральный класс — `ObjectMapper`, который выполняет сериализацию (Java-объект -> JSON) и десериализацию (JSON -> Java-объект). Jackson используется по умолчанию в Spring Boot, Quarkus и Micronaut.

### ObjectMapper: базовое использование

<details>
<summary>Пример конфигурации и сериализации/десериализации</summary>

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

        // Сериализация: объект -> JSON
        User user = new User("Иван", "ivan@example.com", 30);
        String json = mapper.writeValueAsString(user);
        // {"name":"Иван","email":"ivan@example.com","age":30}

        // Красивый вывод
        String prettyJson = mapper.writerWithDefaultPrettyPrinter()
                                  .writeValueAsString(user);

        // Десериализация: JSON -> объект
        String inputJson = """
            {"name":"Мария","email":"maria@example.com","age":25}
            """;
        User restored = mapper.readValue(inputJson, User.class);

        // Десериализация коллекций (generic-типы)
        String arrayJson = """
            [{"name":"Иван","age":30},{"name":"Мария","age":25}]
            """;
        List<User> users = mapper.readValue(arrayJson,
            new TypeReference<List<User>>() {});
    }
}
```

</details>

### Основные аннотации

| Аннотация | Назначение | Пример |
|-----------|-----------|--------|
| `@JsonProperty("name")` | Явное имя JSON-поля | `@JsonProperty("full_name")` |
| `@JsonIgnore` | Исключение поля | На пароле, внутреннем коде |
| `@JsonFormat` | Формат даты/числа | `@JsonFormat(pattern = "dd.MM.yyyy")` |
| `@JsonInclude` | Условие включения | `Include.NON_NULL` — не писать null |
| `@JsonAlias` | Альтернативные имена при чтении | `@JsonAlias({"email_address"})` |
| `@JsonNaming` | Стратегия именования на уровне класса | `SnakeCaseStrategy.class` |

### Модуль JavaTimeModule

Обязателен для работы с `java.time.*` (`LocalDate`, `LocalDateTime` и др.). Без него Jackson выбросит `InvalidDefinitionException`.

```java
ObjectMapper mapper = new ObjectMapper();
mapper.registerModule(new JavaTimeModule());
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
```

Maven-зависимость:

```xml
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
</dependency>
```

### Ключевые правила

- `ObjectMapper` потокобезопасен после конфигурации — создавайте один экземпляр и переиспользуйте
- `FAIL_ON_UNKNOWN_PROPERTIES = false` — критически важно для обратной совместимости API
- `TypeReference` нужен для десериализации generic-типов, так как Java стирает generic-информацию в рантайме

### Частые ошибки

- Создание нового `ObjectMapper` на каждый запрос — потеря производительности
- Забытый `JavaTimeModule` при использовании `LocalDate`/`LocalDateTime`
- Отсутствие конструктора без аргументов — Jackson не сможет создать объект (если не использовать `@JsonCreator`)
- `FAIL_ON_UNKNOWN_PROPERTIES = true` (по умолчанию) — падение при добавлении новых полей в API

> **На собеседовании:** покажите знание `ObjectMapper`, назовите 3-4 основные аннотации и упомяните `JavaTimeModule`. Частая проверка — знаете ли вы, что `ObjectMapper` нужно переиспользовать, а не создавать каждый раз.

[к оглавлению](#Сериализация)

---

## Как кастомизировать сериализацию в Jackson?
<!-- grade: middle -->

Jackson предоставляет несколько механизмов кастомизации: пользовательские сериализаторы/десериализаторы, аннотации для управления созданием объектов и mix-ins для работы с классами, которые нельзя модифицировать.

### Custom Serializer / Deserializer

Наследуются от `StdSerializer` / `StdDeserializer` и применяются через аннотацию `@JsonSerialize` / `@JsonDeserialize`.

<details>
<summary>Пример кастомного сериализатора и десериализатора</summary>

```java
// Сериализатор: Money -> "100.50 RUB"
public class MoneySerializer extends StdSerializer<Money> {
    public MoneySerializer() { super(Money.class); }

    @Override
    public void serialize(Money value, JsonGenerator gen,
                          SerializerProvider provider) throws IOException {
        gen.writeString(value.getAmount().toPlainString() + " " + value.getCurrency());
    }
}

// Десериализатор: "100.50 RUB" -> Money
public class MoneyDeserializer extends StdDeserializer<Money> {
    public MoneyDeserializer() { super(Money.class); }

    @Override
    public Money deserialize(JsonParser p, DeserializationContext ctxt)
            throws IOException {
        String text = p.getText(); // "100.50 RUB"
        String[] parts = text.split(" ");
        return new Money(new BigDecimal(parts[0]), parts[1]);
    }
}

// Применение
public class Order {
    @JsonSerialize(using = MoneySerializer.class)
    @JsonDeserialize(using = MoneyDeserializer.class)
    private Money totalPrice;
}
```

</details>

### Аннотации для управления созданием объектов

| Аннотация | Назначение |
|-----------|-----------|
| `@JsonCreator` | Десериализация через конструктор/фабричный метод (для иммутабельных объектов) |
| `@JsonValue` | Объект сериализуется как одно значение (часто для enum) |
| `@JsonUnwrapped` | Развёртывание вложенного объекта на уровень выше |

<details>
<summary>Примеры: JsonCreator, JsonValue, JsonUnwrapped</summary>

```java
// @JsonCreator — иммутабельный объект без конструктора по умолчанию
public class Address {
    private final String city;
    private final String street;

    @JsonCreator
    public Address(
            @JsonProperty("city") String city,
            @JsonProperty("street") String street) {
        this.city = city;
        this.street = street;
    }
    // Только getters
}

// @JsonValue — enum сериализуется как строка
public enum Status {
    ACTIVE("active"), INACTIVE("inactive");
    private final String code;
    Status(String code) { this.code = code; }

    @JsonValue
    public String getCode() { return code; }

    @JsonCreator
    public static Status fromCode(String code) {
        return Arrays.stream(values())
                .filter(s -> s.code.equals(code))
                .findFirst().orElseThrow();
    }
}

// @JsonUnwrapped — развёртывание вложенного объекта
public class Person {
    private String name;

    @JsonUnwrapped
    private Address address;
    // Вместо {"name":"Иван","address":{"city":"Москва"}}
    // Получим: {"name":"Иван","city":"Москва"}
}
```

</details>

### Mix-ins для сторонних классов

Позволяют добавить аннотации Jackson к классам, исходный код которых нельзя изменить.

```java
// Mix-in — абстрактный класс с аннотациями
public abstract class ThirdPartyUserMixin {
    @JsonProperty("username")
    abstract String getName();

    @JsonIgnore
    abstract String getSecret();
}

// Регистрация
mapper.addMixIn(ThirdPartyUser.class, ThirdPartyUserMixin.class);
```

### Частые ошибки

- Забытая `@JsonProperty` в параметрах `@JsonCreator` — Jackson не знает, какой JSON-параметр передать в какой аргумент
- `@JsonUnwrapped` не работает с коллекциями и `Map` — только с POJO
- Конфликт `@JsonValue` с другими аннотациями — `@JsonValue` определяет единственное представление объекта

> **На собеседовании:** назовите `StdSerializer`/`StdDeserializer`, `@JsonCreator` для иммутабельных объектов и mix-ins для сторонних классов. Java `record`-классы (16+) поддерживаются Jackson из коробки, что делает `@JsonCreator` менее нужным для простых DTO.

[к оглавлению](#Сериализация)

---

## Чем отличается Jackson от Gson?
<!-- grade: middle -->

Jackson и Gson — две основные библиотеки для JSON-сериализации в Java. Обе решают одну задачу, но различаются производительностью, экосистемой и областью применения.

### Сравнительная таблица

| Критерий | Jackson | Gson |
|----------|---------|------|
| Разработчик | FasterXML (open-source) | Google |
| Производительность | Быстрее (особенно Streaming API) | Медленнее на больших объёмах |
| Размер библиотеки | Больше (модульная структура) | Компактная (~300 KB) |
| Streaming API | `JsonParser` / `JsonGenerator` | `JsonReader` / `JsonWriter` |
| Tree Model | `JsonNode` | `JsonElement` / `JsonObject` |
| Аннотации | Богатый набор (`@JsonProperty`, `@JsonCreator` и др.) | Минимальный (`@SerializedName`, `@Expose`) |
| Модульная система | Да (JavaTimeModule, Kotlin и др.) | Нет |
| Spring Boot | По умолчанию | Требует ручной настройки |
| record (Java 16+) | Полная поддержка | Поддержка с 2.10+ |
| Потокобезопасность | `ObjectMapper` переиспользуем | `Gson` переиспользуем |

### Когда использовать Jackson

- В любом Spring/Spring Boot проекте — он уже подключён
- При высоких требованиях к производительности на больших JSON
- Когда нужен богатый набор аннотаций и модулей
- В enterprise-проектах

### Когда использовать Gson

- В небольших утилитах, где важен минимальный размер зависимостей
- В Android-проектах (хотя Moshi и kotlinx.serialization вытесняют Gson)
- Когда нужна простая библиотека без сложной конфигурации

<details>
<summary>Пример: Jackson vs Gson</summary>

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

// === Gson: кастомизация ===
Gson customGson = new GsonBuilder()
    .setPrettyPrinting()
    .setDateFormat("dd.MM.yyyy")
    .serializeNulls()
    .excludeFieldsWithoutExposeAnnotation()
    .registerTypeAdapter(Money.class, new MoneyTypeAdapter())
    .create();
```

</details>

### Частые ошибки

- Смешивание аннотаций Jackson и Gson в одном проекте — они несовместимы
- Использование Gson в Spring Boot — конфликт с Jackson, который уже в classpath
- Предположение, что Gson быстрее из-за меньшего размера — Jackson быстрее в бенчмарках
- Ручное построение JSON через конкатенацию строк вместо использования библиотеки

> **На собеседовании:** кратко сравните по 3-4 критериям (производительность, экосистема, Spring Boot) и дайте рекомендацию: Jackson — стандарт для серверной Java, Gson — для простых случаев и legacy. В Kotlin-проектах всё чаще используется kotlinx.serialization.

[к оглавлению](#Сериализация)

---

## Как работать с JSON в Java без библиотек и с Jakarta JSON-P/JSON-B?
<!-- grade: middle -->

В Jakarta EE существуют два стандартных API: JSON-P (JSON Processing) для низкоуровневого парсинга и JSON-B (JSON Binding) для привязки к объектам. Оба входят в спецификацию Jakarta EE, но на практике Jackson используется значительно чаще.

### JSON-P (Jakarta JSON Processing)

Низкоуровневый API для парсинга, генерации и трансформации JSON. Работает на двух уровнях: потоковом (`JsonParser`/`JsonGenerator`) и объектном (`JsonObject`/`JsonArray`).

<details>
<summary>Пример JSON-P: построение и чтение JSON</summary>

```java
import jakarta.json.Json;
import jakarta.json.JsonObject;
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

</details>

### JSON-B (Jakarta JSON Binding)

Высокоуровневый API — аналог Jackson/Gson в мире Jakarta EE. Автоматическая привязка JSON к Java-объектам.

<details>
<summary>Пример JSON-B: аннотации и маппинг</summary>

```java
import jakarta.json.bind.Jsonb;
import jakarta.json.bind.JsonbBuilder;
import jakarta.json.bind.annotation.JsonbProperty;
import jakarta.json.bind.annotation.JsonbTransient;
import jakarta.json.bind.annotation.JsonbDateFormat;

public class Employee {
    @JsonbProperty("full_name")
    private String name;

    @JsonbTransient
    private String internalCode;

    @JsonbDateFormat("dd.MM.yyyy")
    private LocalDate hireDate;
}

// Использование
JsonbConfig config = new JsonbConfig()
        .withFormatting(true)
        .withNullValues(false);

try (Jsonb jsonb = JsonbBuilder.create(config)) {
    String json = jsonb.toJson(employee);          // Сериализация
    Employee restored = jsonb.fromJson(json, Employee.class); // Десериализация
}
```

</details>

### Сравнение подходов

| Критерий | JSON-P | JSON-B | Jackson |
|----------|--------|--------|---------|
| Уровень | Низкоуровневый (парсинг) | Высокоуровневый (binding) | Оба |
| Стандарт | Jakarta EE | Jakarta EE | Де-факто стандарт |
| Гибкость | Высокая | Средняя | Очень высокая |
| Экосистема | Ограниченная | Ограниченная | Огромная (модули) |
| Spring Boot | Не по умолчанию | Не по умолчанию | По умолчанию |
| Реализация | Eclipse Parsson | Eclipse Yasson | Встроена |

### Частые ошибки

- Путаница между `javax.json` (старый Java EE) и `jakarta.json` (новый Jakarta EE) — пакеты несовместимы
- Попытка использовать JSON-P для сложного маппинга — для этого нужен JSON-B или Jackson
- Отсутствие реализации в classpath — JSON-P/JSON-B это интерфейсы, нужна реализация (Parsson, Yasson)
- Смешивание аннотаций JSON-B и Jackson — они не взаимозаменяемы

> **На собеседовании:** достаточно знать, что JSON-P/JSON-B существуют как часть Jakarta EE, и объяснить разницу: JSON-P — низкоуровневый парсинг, JSON-B — object binding. На практике даже в Jakarta EE проектах многие используют Jackson.

[к оглавлению](#Сериализация)

---

## Java Serialization vs JSON: когда что использовать?
<!-- grade: middle -->

Стандартная Java-сериализация (`Serializable`) и JSON-сериализация (Jackson, Gson) решают схожую задачу — преобразование объектов в переносимый формат, — но кардинально различаются по безопасности, совместимости и области применения.

### Сравнительная таблица

| Критерий | Java Serialization | JSON (Jackson и др.) | Бинарные форматы (Protobuf, Avro) |
|----------|-------------------|---------------------|-----------------------------------|
| Читаемость | Нет (бинарный) | Да (текстовый) | Нет (бинарный) |
| Кросс-языковость | Только Java | Любой язык | Любой язык (кодогенерация) |
| Производительность | Средняя | Средняя | Высокая |
| Размер данных | Большой (метаданные) | Средний | Маленький |
| Эволюция схемы | Хрупкая (`serialVersionUID`) | Гибкая (новые поля игнорируются) | Встроенная поддержка |
| Безопасность | Опасная (атаки десериализации) | Безопасная (при правильной настройке) | Безопасная |
| REST API | Не применимо | Стандарт | Используется в gRPC |

### Почему Java Serialization опасна

Десериализация данных из непроверенного источника может привести к Remote Code Execution (RCE). Атака основана на «gadget chains» — цепочках вызовов в библиотеках (Apache Commons Collections, Spring и др.), которые при десериализации выполняют произвольный код.

```java
// ОПАСНО: десериализация из непроверенного источника
ObjectInputStream ois = new ObjectInputStream(untrustedInput);
Object obj = ois.readObject(); // Злоумышленник может выполнить произвольный код
```

С Java 9+ можно использовать `ObjectInputFilter` для ограничения допустимых классов, но это не решает проблему полностью:

```java
ObjectInputFilter filter = ObjectInputFilter.Config.createFilter(
    "com.myapp.**;!*"  // Разрешить только свои классы
);
ois.setObjectInputFilter(filter);
```

### Когда что выбрать

| Формат | Когда использовать |
|--------|-------------------|
| **JSON** | REST API, конфигурации, межсервисное взаимодействие по HTTP, NoSQL (MongoDB), логирование |
| **Protobuf / gRPC** | Высоконагруженное межсервисное взаимодействие, когда важна производительность и размер |
| **Avro** | Потоковая обработка (Kafka, Spark), event-driven архитектура с эволюцией схем |
| **Java Serialization** | Практически не используется в новых проектах. Встречается в legacy, RMI |

### Частые ошибки

- Использование Java Serialization для межсервисного обмена — невозможно прочитать на стороне, написанной не на Java
- Десериализация Java-объектов из непроверенных источников без фильтров — критическая уязвимость (CWE-502)
- Хранение сериализованных Java-объектов в БД — невозможно мигрировать при изменении классов
- Предположение, что JSON безопасен «автоматически» — полиморфная десериализация в Jackson (`@JsonTypeInfo`) тоже может быть вектором атаки

> **На собеседовании:** главный тезис — Java Serialization не рекомендуется для новых проектов (позиция Oracle и OWASP). JSON — стандарт для API. Protobuf/Avro — для высоконагруженных внутренних коммуникаций. Упомяните проблему безопасности (gadget chains) — это покажет глубокое понимание.

[к оглавлению](#Сериализация)

---

# Источники
+ [IBM developerWorks](https://www.ibm.com/developerworks/ru/library/j-5things1/)
+ [Java-online.ru](http://java-online.ru/blog-serialization.xhtml)
+ [Изучите секреты Java Serialization API](http://ccfit.nsu.ru/~deviv/courses/oop/java_ser_rus.html)
+ [JavaRush](http://bit.ly/1xwRA2D)
+ [Записки трезвого практика](http://www.skipy.ru/technics/serialization.html)

[Вопросы для собеседования](README.md)
