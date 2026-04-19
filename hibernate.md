[Вопросы для собеседования](README.md)

# Hibernate
+ [Что такое Hibernate и зачем он нужен?](#Что-такое-Hibernate-и-зачем-он-нужен)
+ [Что такое ORM? Как Hibernate реализует ORM?](#Что-такое-ORM-Как-Hibernate-реализует-ORM)
+ [Что такое JPA и как Hibernate связан с JPA?](#Что-такое-JPA-и-как-Hibernate-связан-с-JPA)
+ [Архитектура Hibernate: SessionFactory, Session, Transaction](#Архитектура-Hibernate-SessionFactory-Session-Transaction)
+ [Жизненный цикл Entity в Hibernate](#Жизненный-цикл-Entity-в-Hibernate)
+ [Основные аннотации маппинга сущностей](#Основные-аннотации-маппинга-сущностей)
+ [Стратегии генерации идентификаторов](#Стратегии-генерации-идентификаторов)
+ [Маппинг связей между сущностями](#Маппинг-связей-между-сущностями)
+ [Что такое FetchType.LAZY и FetchType.EAGER?](#Что-такое-FetchType-LAZY-и-FetchType-EAGER)
+ [Проблема N+1 запросов и способы решения](#Проблема-N1-запросов-и-способы-решения)
+ [Что такое кэш первого уровня (L1 Cache)?](#Что-такое-кэш-первого-уровня-L1-Cache)
+ [Что такое кэш второго уровня (L2 Cache)?](#Что-такое-кэш-второго-уровня-L2-Cache)
+ [Что такое Query Cache?](#Что-такое-Query-Cache)
+ [HQL и JPQL — что это и чем отличаются от SQL?](#HQL-и-JPQL--что-это-и-чем-отличаются-от-SQL)
+ [Criteria API — что это и когда использовать?](#Criteria-API--что-это-и-когда-использовать)
+ [Оптимистичная и пессимистичная блокировки](#Оптимистичная-и-пессимистичная-блокировки)
+ [Стратегии наследования в Hibernate](#Стратегии-наследования-в-Hibernate)
+ [Что такое Dirty Checking и Flush?](#Что-такое-Dirty-Checking-и-Flush)
+ [Что такое EntityManager и чем он отличается от Session?](#Что-такое-EntityManager-и-чем-он-отличается-от-Session)
+ [Как работает каскадирование (CascadeType)?](#Как-работает-каскадирование-CascadeType)
+ [Что такое LazyInitializationException и как его избежать?](#Что-такое-LazyInitializationException-и-как-его-избежать)
+ [Что такое проекции и DTO-маппинг?](#Что-такое-проекции-и-DTO-маппинг)
+ [Batch-операции в Hibernate](#Batch-операции-в-Hibernate)

## Что такое Hibernate и зачем он нужен?

**Hibernate** — это фреймворк объектно-реляционного маппинга (ORM) для Java, который автоматизирует преобразование данных между объектной моделью Java и реляционными таблицами базы данных. Hibernate является наиболее популярной реализацией спецификации JPA.

**Проблема, которую решает Hibernate:**

Реляционные БД оперируют таблицами, строками и столбцами, а Java — объектами, полями и коллекциями. Это несоответствие называется **impedance mismatch**. Без ORM разработчик вручную пишет SQL-запросы, маппит `ResultSet` в объекты и обратно:

```java
// Без Hibernate — ручной JDBC
public User findById(Long id) throws SQLException {
    String sql = "SELECT id, name, email FROM users WHERE id = ?";
    try (PreparedStatement ps = connection.prepareStatement(sql)) {
        ps.setLong(1, id);
        ResultSet rs = ps.executeQuery();
        if (rs.next()) {
            User user = new User();
            user.setId(rs.getLong("id"));
            user.setName(rs.getString("name"));
            user.setEmail(rs.getString("email"));
            return user;
        }
    }
    return null;
}

// С Hibernate — одна строка
public User findById(Long id) {
    return session.find(User.class, id);
}
```

**Основные возможности Hibernate:**
- Автоматический маппинг объектов в таблицы и обратно
- Генерация SQL-запросов (INSERT, UPDATE, DELETE, SELECT)
- Управление транзакциями
- Кэширование (L1, L2, Query Cache)
- Ленивая загрузка связанных сущностей
- HQL/JPQL — объектно-ориентированные языки запросов
- Автоматическое отслеживание изменений (Dirty Checking)
- Управление схемой БД (hbm2ddl)

### Важное
- Hibernate — не замена SQL, а абстракция над JDBC, которая генерирует SQL автоматически
- Hibernate реализует спецификацию JPA, но имеет и собственный расширенный API
- Знание SQL по-прежнему необходимо — для сложных запросов, оптимизации, отладки
- Hibernate управляет жизненным циклом объектов через Persistence Context

### Частые ошибки
- **«Hibernate = медленный»** — медленным его делает неправильное использование (N+1, отсутствие индексов, EAGER для коллекций)
- **Игнорирование сгенерированного SQL** — всегда нужно проверять, какие запросы генерирует Hibernate (`spring.jpa.show-sql=true` или logging)
- **Использование `hbm2ddl.auto=update` в продакшене** — в продакшене схему БД должен контролировать инструмент миграций (Liquibase/Flyway)
- **Избыточный маппинг** — не каждая таблица нуждается в сущности; для read-only и отчётов лучше использовать JDBC/DTO-проекции

### Как используется в 2026
- Hibernate 6.x (текущая мажорная версия) — значительные улучшения производительности, поддержка Java records для embeddable
- Spring Data JPA + Hibernate — стандартный стек для работы с реляционными БД
- Для read-heavy сценариев тренд на использование JDBC/jOOQ рядом с Hibernate
- Hibernate Reactive существует, но на практике для реактивных приложений чаще выбирают R2DBC

[к оглавлению](#Hibernate)

## Что такое ORM? Как Hibernate реализует ORM?

**ORM (Object-Relational Mapping)** — технология программирования, связывающая объектную модель приложения с реляционной моделью базы данных. ORM автоматически конвертирует данные между двумя несовместимыми системами типов.

**Маппинг понятий:**

| Объектная модель (Java) | Реляционная модель (БД) |
|------------------------|------------------------|
| Класс | Таблица |
| Объект (экземпляр) | Строка (row) |
| Поле | Столбец (column) |
| Ссылка на объект | Внешний ключ (FK) |
| Коллекция | Join-таблица или FK |
| Наследование | Нет прямого аналога |

**Как Hibernate реализует ORM:**

```java
// Java-класс маппится на таблицу
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;                    // → столбец id (PK)

    @Column(name = "user_name", nullable = false)
    private String name;                // → столбец user_name

    @Column(unique = true)
    private String email;               // → столбец email

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "department_id")
    private Department department;      // → столбец department_id (FK)

    @OneToMany(mappedBy = "user")
    private List<Order> orders;         // → связь через FK в таблице orders
}
```

**Hibernate автоматически выполняет:**
1. **Чтение (SELECT)** — загружает строку из БД и создаёт Java-объект с заполненными полями
2. **Запись (INSERT)** — берёт Java-объект и формирует INSERT-запрос
3. **Обновление (UPDATE)** — отслеживает изменения полей и формирует UPDATE только для изменённых столбцов
4. **Удаление (DELETE)** — удаляет строку по объекту

### Важное
- ORM — мост между объектным и реляционным мирами
- Hibernate использует аннотации (или XML) для описания маппинга
- Маппинг наследования — уникальная задача ORM, имеющая несколько стратегий (SINGLE_TABLE, JOINED, TABLE_PER_CLASS)
- ORM не заменяет понимание реляционной модели — без него невозможно эффективно проектировать сущности

### Частые ошибки
- **Проектировать объектную модель без учёта реляционной** — ORM не магия, плохо спроектированные связи ведут к плохим запросам
- **Маппить все поля и связи** — не каждый столбец должен быть в сущности; используйте `@Transient` или DTO
- **Не учитывать impedance mismatch при наследовании** — наследование в Java элегантно, но в БД может порождать сложные JOIN-ы

### Как используется в 2026
- ORM остаётся доминирующим подходом для CRUD-операций в enterprise Java
- Для сложных отчётов и аналитики — тренд на совмещение ORM (write) и jOOQ/JDBC (read)
- Hibernate 6.x улучшил маппинг — поддержка Java records как `@Embeddable`, улучшенный SQL

[к оглавлению](#Hibernate)

## Что такое JPA и как Hibernate связан с JPA?

**JPA (Jakarta Persistence API, ранее Java Persistence API)** — спецификация Java, определяющая стандартный API для объектно-реляционного маппинга и управления персистентными данными. JPA — это набор интерфейсов и аннотаций, а не реализация.

**Hibernate — наиболее популярная реализация JPA.** Помимо Hibernate, существуют другие реализации: EclipseLink, OpenJPA.

**Соотношение JPA и Hibernate:**

```
JPA (спецификация)     →  Hibernate (реализация)
EntityManager          →  Session (extends EntityManager)
EntityManagerFactory   →  SessionFactory (extends EntityManagerFactory)
JPQL                   →  HQL (расширение JPQL)
@Entity, @Table, @Id   →  аннотации JPA + собственные (@NaturalId, @BatchSize...)
persistence.xml        →  hibernate.cfg.xml (или Spring Boot auto-config)
```

**JPA API (стандартный, переносимый):**

```java
@PersistenceContext
private EntityManager entityManager;

public User findById(Long id) {
    return entityManager.find(User.class, id);
}

public List<User> findByName(String name) {
    return entityManager.createQuery(
            "SELECT u FROM User u WHERE u.name = :name", User.class)
        .setParameter("name", name)
        .getResultList();
}
```

**Hibernate API (расширенный):**

```java
Session session = entityManager.unwrap(Session.class);

// NaturalId — Hibernate-специфичная функция
User user = session.byNaturalId(User.class)
    .using("email", "john@example.com")
    .load();

// Hibernate-специфичные аннотации
@Entity
public class User {
    @NaturalId
    private String email;       // натуральный ключ — только Hibernate

    @BatchSize(size = 25)       // оптимизация загрузки — только Hibernate
    @OneToMany(mappedBy = "user")
    private List<Order> orders;
}
```

### Важное
- JPA — спецификация (интерфейсы), Hibernate — реализация
- Используя только JPA API, можно заменить Hibernate на другую реализацию без изменения кода
- Hibernate расширяет JPA собственными аннотациями и API (`@NaturalId`, `@BatchSize`, `@Formula`, `Session`)
- В Spring Boot по умолчанию используется Hibernate как JPA-провайдер

### Частые ошибки
- **Путать JPA и Hibernate** — JPA не умеет ничего делать сам, это только набор интерфейсов
- **Использовать Hibernate-специфичный API без необходимости** — теряется переносимость; начинайте с JPA API
- **Не знать, что происходит «под капотом»** — Spring Data JPA → JPA → Hibernate → JDBC → SQL; понимание всех уровней необходимо для отладки

### Как используется в 2026
- JPA 3.1+ (Jakarta) — актуальная версия спецификации, входит в Jakarta EE 10
- Hibernate 6.x — реализация JPA 3.1, значительные внутренние изменения по сравнению с 5.x
- Spring Data JPA абстрагирует от JPA/Hibernate ещё сильнее, но знание JPA необходимо для оптимизации
- Миграция с `javax.persistence` на `jakarta.persistence` — обязательна при переходе на Spring Boot 3.x

[к оглавлению](#Hibernate)

## Архитектура Hibernate: SessionFactory, Session, Transaction

**Архитектура Hibernate** строится вокруг трёх ключевых компонентов: `SessionFactory`, `Session` и `Transaction`.

**SessionFactory:**
- Тяжёлый, потокобезопасный объект, создаётся **один раз** при запуске приложения
- Содержит маппинг-метаданные, конфигурацию, пул соединений
- Создание `SessionFactory` — дорогая операция (парсинг аннотаций, валидация маппинга)
- В JPA-терминах — `EntityManagerFactory`

**Session:**
- Лёгкий, **не потокобезопасный** объект, создаётся для каждой единицы работы
- Представляет собой **Persistence Context** — кэш первого уровня
- Отслеживает состояние загруженных сущностей (Dirty Checking)
- В JPA-терминах — `EntityManager`

**Transaction:**
- Атомарная единица работы с БД
- Все операции с БД должны выполняться в рамках транзакции

```java
// Программное управление (без Spring)
SessionFactory sessionFactory = new Configuration()
    .configure()
    .buildSessionFactory();

try (Session session = sessionFactory.openSession()) {
    Transaction tx = session.beginTransaction();
    try {
        User user = new User("John", "john@example.com");
        session.persist(user);

        User found = session.find(User.class, 1L);
        found.setName("Jane");
        // UPDATE сгенерируется автоматически при commit (Dirty Checking)

        tx.commit();
    } catch (Exception e) {
        tx.rollback();
        throw e;
    }
}

// С Spring — всё управляется автоматически через @Transactional
@Service
public class UserService {
    @Autowired
    private EntityManager entityManager;

    @Transactional
    public void createUser(String name, String email) {
        User user = new User(name, email);
        entityManager.persist(user);
        // commit произойдёт автоматически при выходе из метода
    }
}
```

**Взаимосвязь компонентов:**

```
Application
    └── SessionFactory (1 на приложение, потокобезопасный)
            └── Session (1 на запрос/транзакцию, НЕ потокобезопасный)
                    ├── Persistence Context (L1 Cache)
                    ├── Transaction
                    └── JDBC Connection (из пула)
```

### Важное
- `SessionFactory` — один на приложение, создаётся при старте; `Session` — один на единицу работы
- `Session` = Persistence Context = кэш первого уровня
- `Session` не потокобезопасна — никогда не делите её между потоками
- В Spring `Session` привязана к `@Transactional` — открывается при входе, закрывается при выходе

### Частые ошибки
- **Создавать `SessionFactory` повторно** — это крайне дорогая операция; один раз при запуске
- **Использовать `Session` из разных потоков** — приводит к непредсказуемому поведению и data corruption
- **Работать без транзакции** — чтение без транзакции допустимо, но запись без транзакции приведёт к ошибке или потере данных
- **Длинные транзакции** — держать `Session` открытой долго (например, на время HTTP-запроса + рендеринга) — anti-pattern Open Session in View

### Как используется в 2026
- Spring Boot полностью управляет жизненным циклом `SessionFactory` и `Session`
- `EntityManager` (JPA) предпочтительнее `Session` (Hibernate) в коде — стандартный API
- HikariCP — стандартный пул соединений, интегрированный с Hibernate через Spring Boot
- Open Session in View по умолчанию включён в Spring Boot, но рекомендуется отключать (`spring.jpa.open-in-view=false`)

[к оглавлению](#Hibernate)

## Жизненный цикл Entity в Hibernate

Сущность (Entity) в Hibernate проходит через четыре состояния в рамках Persistence Context:

**1. Transient (Новый)** — объект создан через `new`, не связан с Persistence Context и не имеет представления в БД:

```java
User user = new User("John", "john@example.com"); // Transient
// Hibernate ничего не знает об этом объекте
```

**2. Persistent (Управляемый)** — объект связан с Persistence Context и имеет представление в БД. Все изменения автоматически синхронизируются с БД при flush/commit:

```java
session.persist(user);              // Transient → Persistent
User found = session.find(User.class, 1L); // Persistent (загружен из БД)

found.setName("Jane");              // изменение автоматически попадёт в БД (Dirty Checking)
```

**3. Detached (Отсоединённый)** — объект имеет представление в БД, но не связан с текущим Persistence Context:

```java
session.close();                    // все сущности → Detached
session.detach(user);               // конкретная сущность → Detached
session.evict(user);                // Hibernate API — то же самое

// Detached объект можно вернуть в Persistent:
session.merge(user);                // Detached → Persistent (копирование состояния)
```

**4. Removed (Удалённый)** — объект помечен для удаления, будет удалён из БД при flush/commit:

```java
session.remove(user);               // Persistent → Removed
// DELETE будет выполнен при flush/commit
```

**Диаграмма переходов:**

```
         new()          persist()
[Нет] ────────→ [Transient] ────────→ [Persistent] ←──── find()/load()/query
                     │                    │   ↑
                     │              detach()/  merge()
                     │              close()    │
                     │                    ↓   │
                     │              [Detached]
                     │                    │
                     │                remove()
                     │                    ↓
                     └──────────→ [Removed] ────→ DELETE в БД
```

### Важное
- **Persistent** — единственное состояние, в котором Hibernate отслеживает изменения
- `persist()` — Transient → Persistent, `merge()` — Detached → Persistent (копия), `remove()` — Persistent → Removed
- Dirty Checking работает только для Persistent-объектов: изменение поля автоматически генерирует UPDATE
- Persistence Context гарантирует identity: `session.find(User.class, 1L) == session.find(User.class, 1L)` → `true`

### Частые ошибки
- **Путать `persist()` и `merge()`** — `persist` делает объект managed, `merge` создаёт managed **копию** (оригинал остаётся detached)
- **Изменять Detached-объект и ожидать UPDATE** — изменения detached-объекта не отслеживаются; нужен `merge()`
- **Вызывать `persist()` для объекта с заданным `@Id`** — если `@GeneratedValue` не используется, нужен `merge()`
- **Не понимать flush** — `persist()` не выполняет INSERT немедленно; SQL выполняется при flush (commit, query, `session.flush()`)

### Как используется в 2026
- Spring Data JPA скрывает управление состояниями за `save()`, `findById()`, `delete()`
- `save()` в Spring Data вызывает `persist()` для нового объекта или `merge()` для существующего
- Понимание жизненного цикла необходимо для отладки — без него невозможно объяснить, почему UPDATE не выполнился или выполнился лишний раз

[к оглавлению](#Hibernate)

## Основные аннотации маппинга сущностей

**Аннотации JPA** определяют, как Java-классы и поля маппятся на таблицы и столбцы.

```java
@Entity                                    // класс — JPA-сущность
@Table(name = "users",                     // таблица в БД
       uniqueConstraints = @UniqueConstraint(columnNames = "email"),
       indexes = @Index(columnList = "name"))
public class User {

    @Id                                    // первичный ключ
    @GeneratedValue(strategy = GenerationType.IDENTITY) // автоинкремент
    private Long id;

    @Column(name = "user_name",            // имя столбца
            nullable = false,              // NOT NULL
            length = 100)                  // VARCHAR(100)
    private String name;

    @Column(unique = true, nullable = false)
    private String email;

    @Enumerated(EnumType.STRING)           // enum как строка (не ordinal!)
    @Column(nullable = false)
    private UserStatus status;

    @Temporal(TemporalType.TIMESTAMP)      // для java.util.Date (не нужен для LocalDateTime)
    private Date createdAt;

    @Lob                                   // BLOB/CLOB — для больших объектов
    private String description;

    @Transient                             // не маппится в БД
    private String temporaryField;

    @Embedded                              // встраиваемый объект
    private Address address;

    @Version                               // оптимистичная блокировка
    private Long version;
}

@Embeddable                                // встраиваемый компонент (без своей таблицы)
public class Address {
    private String city;
    private String street;

    @Column(name = "zip_code")
    private String zipCode;
}
```

**Краткая таблица аннотаций:**

| Аннотация | Назначение |
|-----------|-----------|
| `@Entity` | Маркирует класс как JPA-сущность |
| `@Table` | Настройка таблицы (имя, уникальные ограничения, индексы) |
| `@Id` | Первичный ключ |
| `@GeneratedValue` | Стратегия генерации ID |
| `@Column` | Настройка столбца (имя, nullable, unique, length) |
| `@Enumerated` | Маппинг enum (STRING или ORDINAL) |
| `@Temporal` | Тип для `java.util.Date` |
| `@Lob` | Большие объекты (BLOB, CLOB) |
| `@Transient` | Поле не маппится в БД |
| `@Embedded` / `@Embeddable` | Встраиваемый объект без собственной таблицы |
| `@Version` | Поле для оптимистичной блокировки |

### Важное
- `@Entity` обязательна; класс должен иметь конструктор без аргументов и `@Id`
- `@Column` необязательна — без неё поле маппится по имени
- `@Enumerated(EnumType.STRING)` — **всегда используйте STRING**, не ORDINAL (добавление значения в enum сломает данные)
- `@Embedded` / `@Embeddable` — способ декомпозиции сущности без создания отдельной таблицы

### Частые ошибки
- **`@Enumerated(EnumType.ORDINAL)`** — при изменении порядка элементов enum данные в БД станут некорректными
- **Отсутствие `@Version`** — без оптимистичной блокировки потерянные обновления (lost updates) неизбежны при конкурентном доступе
- **`@Column(nullable = false)` без NOT NULL в БД** — аннотация влияет только на DDL-генерацию; если БД уже создана через миграции, ограничение нужно добавлять в миграции
- **Маппинг `boolean` → `@Column`** — по умолчанию маппится в BIT/BOOLEAN, но в Oracle нет BOOLEAN; нужен `@Column(columnDefinition = "NUMBER(1)")`

### Как используется в 2026
- Hibernate 6.x поддерживает Java Records как `@Embeddable` — удобнее для Value Objects
- `@JdbcTypeCode(SqlTypes.JSON)` — нативная поддержка JSON-столбцов в Hibernate 6
- Temporal API (`LocalDate`, `LocalDateTime`) маппится без `@Temporal` начиная с JPA 2.2
- `@SoftDelete` (Hibernate 6.4) — встроенная поддержка мягкого удаления

[к оглавлению](#Hibernate)

## Стратегии генерации идентификаторов

Стратегия генерации ID определяет, как будет назначаться первичный ключ новой сущности.

**Основные стратегии JPA:**

**1. IDENTITY** — автоинкремент на стороне БД:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
// SQL: id BIGINT GENERATED ALWAYS AS IDENTITY
```
- БД генерирует ID при INSERT
- **Не поддерживает batch insert** — Hibernate выполняет INSERT немедленно для получения ID
- Хорошо работает с MySQL, PostgreSQL

**2. SEQUENCE** — последовательность (sequence) в БД:

```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
@SequenceGenerator(name = "user_seq", sequenceName = "user_sequence",
                   allocationSize = 50)
private Long id;
```
- Hibernate заранее запрашивает блок ID из sequence (`allocationSize`)
- **Поддерживает batch insert** — ID известен до выполнения INSERT
- Рекомендуемая стратегия для PostgreSQL и Oracle

**3. TABLE** — отдельная таблица для хранения счётчиков:

```java
@Id
@GeneratedValue(strategy = GenerationType.TABLE, generator = "user_gen")
@TableGenerator(name = "user_gen", table = "id_generator",
                pkColumnName = "gen_key", valueColumnName = "gen_value",
                allocationSize = 50)
private Long id;
```
- Переносима между БД, но **самая медленная** — требует блокировки строки в таблице
- На практике используется крайне редко

**4. AUTO** — Hibernate сам выбирает стратегию в зависимости от БД:

```java
@Id
@GeneratedValue(strategy = GenerationType.AUTO)
private Long id;
```
- В Hibernate 6 по умолчанию выбирает SEQUENCE (если БД поддерживает) или TABLE
- Не рекомендуется в продакшене — поведение может отличаться между БД

**5. UUID — генерация уникальных идентификаторов:**

```java
@Id
@GeneratedValue(strategy = GenerationType.UUID)
private UUID id;
```
- Hibernate 6+ поддерживает `GenerationType.UUID`
- Подходит для распределённых систем (не зависит от БД)
- Занимает 16 байт вместо 8 (Long), хуже индексируется в B-Tree

### Важное
- **SEQUENCE** — рекомендуемая стратегия для PostgreSQL/Oracle (поддерживает batch insert)
- **IDENTITY** — проще, но блокирует batch insert (Hibernate выполняет INSERT сразу для получения ID)
- `allocationSize` у SEQUENCE — Hibernate запрашивает блок ID одним запросом, снижая обращения к БД
- UUID — для распределённых систем, где централизованная генерация ID невозможна

### Частые ошибки
- **IDENTITY + batch insert** — batch insert невозможен с IDENTITY; если нужны пакетные вставки — SEQUENCE
- **`allocationSize=1` у SEQUENCE** — Hibernate будет вызывать `nextval` на каждый INSERT; увеличьте до 50
- **Несовпадение `allocationSize` и INCREMENT в БД** — `allocationSize` в аннотации и INCREMENT BY в DDL sequence должны совпадать
- **AUTO в Hibernate 6** — по умолчанию выбирает TABLE (не SEQUENCE!) для некоторых БД, что медленнее

### Как используется в 2026
- SEQUENCE с `allocationSize=50` — стандарт для PostgreSQL-приложений
- UUID v7 (time-ordered) — набирает популярность для распределённых систем (лучше индексируется чем UUID v4)
- Hibernate 6.x: `@UuidGenerator(style = TIME)` — генерация time-based UUID
- TSID (Time-Sorted ID) — альтернатива UUID, компактнее (Long), используется в некоторых проектах

[к оглавлению](#Hibernate)

## Маппинг связей между сущностями

Hibernate поддерживает четыре типа связей, соответствующих отношениям в реляционной модели.

**@ManyToOne / @OneToMany — самая частая связь:**

```java
// Сторона "многие" — владелец связи (содержит FK)
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}

// Сторона "один" — обратная сторона
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders = new ArrayList<>();

    // Вспомогательные методы для синхронизации обеих сторон
    public void addOrder(Order order) {
        orders.add(order);
        order.setUser(this);
    }

    public void removeOrder(Order order) {
        orders.remove(order);
        order.setUser(null);
    }
}
```

**@OneToOne:**

```java
@Entity
public class User {
    @OneToOne(mappedBy = "user", cascade = CascadeType.ALL,
              fetch = FetchType.LAZY, optional = false)
    private UserProfile profile;
}

@Entity
public class UserProfile {
    @Id
    private Long id; // общий PK с User

    @OneToOne(fetch = FetchType.LAZY)
    @MapsId // PK UserProfile = FK на User
    @JoinColumn(name = "id")
    private User user;
}
```

**@ManyToMany:**

```java
@Entity
public class Student {
    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id"))
    private Set<Course> courses = new HashSet<>();
}

@Entity
public class Course {
    @ManyToMany(mappedBy = "courses")
    private Set<Student> students = new HashSet<>();
}
```

### Важное
- `mappedBy` — указывает обратную (не владеющую) сторону связи; FK хранится на стороне без `mappedBy`
- **Всегда используйте `FetchType.LAZY`** для `@OneToMany` и `@ManyToMany`
- `@ManyToOne` — LAZY, EAGER по умолчанию; явно указывайте LAZY
- Для `@ManyToMany` предпочитайте `Set` вместо `List` — производительнее при операциях удаления
- Синхронизируйте обе стороны связи (helper-методы `addOrder`, `removeOrder`)

### Частые ошибки
- **Не указывать `mappedBy`** — Hibernate создаст промежуточную таблицу вместо использования FK
- **Использовать `List` в `@ManyToMany`** — при удалении Hibernate удалит все записи из join-таблицы и вставит заново; `Set` удаляет точечно
- **EAGER для коллекций** — загрузка всех связанных сущностей сразу ведёт к N+1 и OutOfMemoryError
- **Двунаправленная связь без синхронизации** — если не обновить обе стороны, in-memory модель не совпадает с БД
- **`@OneToOne` с LAZY** — LAZY для `@OneToOne` на стороне без FK не работает без bytecode enhancement (Hibernate должен знать, null ли связь, что требует запроса)

### Как используется в 2026
- `@ManyToOne(fetch = LAZY)` + JOIN FETCH — золотой стандарт
- `@ManyToMany` часто заменяется промежуточной сущностью (`@Entity StudentCourse`) для добавления атрибутов связи
- Hibernate 6 улучшил работу с `@OneToOne` LAZY через bytecode enhancement

[к оглавлению](#Hibernate)

## Что такое FetchType.LAZY и FetchType.EAGER?

**FetchType** определяет, когда Hibernate загружает связанные сущности из БД.

**EAGER** — связанная сущность загружается **сразу** вместе с основной (в том же SQL-запросе или отдельным):

```java
@ManyToOne(fetch = FetchType.EAGER)
private Department department;
// При загрузке User → сразу загружается Department
```

**LAZY** — связанная сущность загружается **только при первом обращении** к ней (по требованию):

```java
@ManyToOne(fetch = FetchType.LAZY)
private Department department;
// При загрузке User → department НЕ загружается
// department загрузится при вызове user.getDepartment().getName()
```

**Значения по умолчанию:**

| Тип связи | FetchType по умолчанию |
|-----------|----------------------|
| `@ManyToOne` | **EAGER** |
| `@OneToOne` | **EAGER** |
| `@OneToMany` | **LAZY** |
| `@ManyToMany` | **LAZY** |

**Как LAZY работает технически:**

Hibernate подставляет вместо реальной сущности **прокси-объект** (наследник сущности, сгенерированный через Byte Buddy). При первом вызове метода прокси выполняет SQL-запрос:

```java
User user = session.find(User.class, 1L);
// user.department — это прокси (не настоящий Department)

String deptName = user.getDepartment().getName();
// ↑ Здесь прокси выполняет SQL: SELECT * FROM departments WHERE id = ?
```

### Важное
- **Всегда явно указывайте `FetchType.LAZY`** для `@ManyToOne` и `@OneToOne` (по умолчанию EAGER!)
- LAZY — это **подсказка**, не требование; Hibernate может загрузить eager при необходимости
- LAZY работает через прокси-объекты — вызов метода на прокси инициирует SQL-запрос
- LAZY требует открытой `Session` — обращение к прокси после закрытия Session → `LazyInitializationException`

### Частые ошибки
- **Оставлять `@ManyToOne` без явного `fetch = LAZY`** — по умолчанию EAGER, загрузка всех связей каскадно
- **EAGER для коллекций** — загрузка `List<Order>` при каждом чтении `User` — катастрофа для производительности
- **Множественные EAGER-коллекции** — Hibernate не может выполнить один запрос для двух EAGER-коллекций; получается Cartesian product или `MultipleBagFetchException`
- **Проверка `== null` для LAZY-прокси** — прокси никогда не null (даже если записи в БД нет); используйте `Hibernate.isInitialized()`

### Как используется в 2026
- Правило: **всё LAZY, загружать через JOIN FETCH или EntityGraph** по необходимости
- Spring Boot `spring.jpa.open-in-view=false` — отключение OSIV заставляет явно загружать данные в сервисном слое
- Hibernate 6 bytecode enhancement — позволяет LAZY для `@Basic` полей (не загружать тяжёлые BLOB/CLOB до обращения)

[к оглавлению](#Hibernate)

## Проблема N+1 запросов и способы решения

**Проблема N+1** — ситуация, когда загрузка списка из N сущностей приводит к 1 основному запросу + N дополнительных запросов для загрузки связанных сущностей.

**Пример проблемы:**

```java
// 1 запрос для загрузки 100 пользователей
List<User> users = userRepository.findAll();
// SELECT * FROM users                    -- 1 запрос

// + 100 запросов для загрузки Department каждого пользователя
for (User user : users) {
    System.out.println(user.getDepartment().getName());
    // SELECT * FROM departments WHERE id = ?  -- N запросов
}
// Итого: 1 + 100 = 101 запрос!
```

**Решения:**

**1. JOIN FETCH (JPQL):**

```java
@Query("SELECT u FROM User u JOIN FETCH u.department")
List<User> findAllWithDepartment();
// Один запрос: SELECT u.*, d.* FROM users u JOIN departments d ON u.department_id = d.id
```

**2. @EntityGraph:**

```java
@EntityGraph(attributePaths = {"department", "orders"})
@Query("SELECT u FROM User u")
List<User> findAllWithGraph();

// Или через именованный EntityGraph:
@Entity
@NamedEntityGraph(name = "User.withDepartment",
    attributeNodes = @NamedAttributeNode("department"))
public class User { ... }

@EntityGraph("User.withDepartment")
List<User> findAll();
```

**3. @BatchSize (Hibernate):**

```java
@Entity
public class User {
    @BatchSize(size = 25)
    @ManyToOne(fetch = FetchType.LAZY)
    private Department department;
}
// Вместо 100 запросов — 4 запроса (по 25 ID):
// SELECT * FROM departments WHERE id IN (?, ?, ..., ?)  — 4 раза
```

**Глобальный `@BatchSize`** — можно задать для всех сущностей:

```properties
spring.jpa.properties.hibernate.default_batch_fetch_size=25
```

**4. DTO-проекция (без загрузки сущностей):**

```java
@Query("SELECT new com.example.UserDto(u.name, d.name) " +
       "FROM User u JOIN u.department d")
List<UserDto> findUserDtos();
// Один запрос, только нужные поля, без сущностей и прокси
```

**Сравнение подходов:**

| Подход | Плюсы | Минусы |
|--------|-------|--------|
| JOIN FETCH | Один запрос | Нельзя пагинировать коллекции; Cartesian product |
| EntityGraph | Декларативный, гибкий | Менее предсказуемый SQL |
| @BatchSize | Простой, не меняет запрос | Всё ещё несколько запросов |
| DTO-проекция | Максимальная эффективность | Нет управляемых сущностей |

### Важное
- N+1 — самая частая проблема производительности Hibernate
- **Всегда проверяйте SQL-логи** при работе с коллекциями (`show-sql` или `logging.level.org.hibernate.SQL=DEBUG`)
- JOIN FETCH — основное решение для `@ManyToOne`; @BatchSize — для `@OneToMany`
- Нельзя JOIN FETCH две коллекции одновременно → используйте `@BatchSize` для второй

### Частые ошибки
- **JOIN FETCH + пагинация** — `findAll(Pageable)` с JOIN FETCH выполнит пагинацию в памяти (загрузит все строки!); Hibernate выдаст предупреждение
- **JOIN FETCH для двух `List`-коллекций** — `MultipleBagFetchException`; замените одну на `Set`
- **Не замечать N+1** — приложение работает «нормально» на тестовых данных, падает в продакшене с тысячами записей
- **Overfix** — JOIN FETCH для всех связей приводит к огромным Cartesian product

### Как используется в 2026
- Стандартный подход: `default_batch_fetch_size=25` глобально + `JOIN FETCH` для критичных запросов
- Spring Data JPA `@EntityGraph` — удобный декларативный подход
- Для read-heavy сценариев — DTO-проекции через Spring Data `interface-based projections`
- Hibernate 6 `@FetchProfile` — более гибкое управление стратегией загрузки

[к оглавлению](#Hibernate)

## Что такое кэш первого уровня (L1 Cache)?

**Кэш первого уровня (L1 Cache)** — это **Persistence Context**, привязанный к `Session` (или `EntityManager`). Это обязательный кэш, который нельзя отключить.

**Как работает:**

```java
@Transactional
public void example() {
    // Запрос к БД — объект помещается в L1 Cache
    User user1 = entityManager.find(User.class, 1L);  // SQL SELECT

    // Повторный запрос — объект берётся из L1 Cache, без SQL
    User user2 = entityManager.find(User.class, 1L);  // НЕТ SQL

    // Гарантия идентичности
    assert user1 == user2;  // true — один и тот же объект

    // Изменение объекта — Dirty Checking отслеживает
    user1.setName("New Name");
    // При commit/flush → UPDATE users SET name = 'New Name' WHERE id = 1
}
// L1 Cache уничтожается при закрытии Session/транзакции
```

**Свойства L1 Cache:**
- Привязан к `Session`, живёт в рамках одной транзакции
- Хранит управляемые (Persistent) сущности
- Гарантирует **identity** — один ID → один объект в рамках Session
- Обеспечивает **Dirty Checking** — автоматическое обнаружение изменений
- **Write-behind** — SQL-запросы откладываются до flush

**Очистка L1 Cache:**

```java
entityManager.clear();          // очистить весь L1 Cache
entityManager.detach(user);     // убрать конкретную сущность из кэша
```

### Важное
- L1 Cache = Persistence Context = Session — это одно и то же
- Нельзя отключить — это фундаментальная часть Hibernate
- Живёт в рамках одной транзакции (`@Transactional`)
- Гарантирует, что два вызова `find(User.class, 1L)` вернут один и тот же Java-объект

### Частые ошибки
- **Загрузка слишком большого количества сущностей** — все загруженные сущности хранятся в L1 Cache; 100K сущностей → OutOfMemoryError
- **Не использовать `clear()` при batch-обработке** — при вставке миллиона записей L1 Cache переполняется
- **Ожидать L1 Cache между транзакциями** — каждая транзакция имеет свой Persistence Context; кэш не переживает транзакцию

### Как используется в 2026
- L1 Cache работает прозрачно, разработчик взаимодействует с ним косвенно
- Для batch-операций: `clear()` каждые N записей — обязательный паттерн
- Spring `@Transactional(readOnly = true)` оптимизирует L1 Cache — отключает Dirty Checking

[к оглавлению](#Hibernate)

## Что такое кэш второго уровня (L2 Cache)?

**Кэш второго уровня (L2 Cache)** — опциональный кэш, разделяемый между всеми `Session` в рамках одного `SessionFactory`. В отличие от L1, хранит данные между транзакциями.

**Архитектура кэширования:**

```
Session 1 → L1 Cache ─┐
Session 2 → L1 Cache ──┤──→ L2 Cache ──→ БД
Session 3 → L1 Cache ─┘
```

**Поиск сущности:** L1 Cache → L2 Cache → БД

**Настройка L2 Cache с Ehcache:**

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-jcache</artifactId>
</dependency>
<dependency>
    <groupId>org.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

```properties
# application.properties
spring.jpa.properties.hibernate.cache.use_second_level_cache=true
spring.jpa.properties.hibernate.cache.region.factory_class=jcache
spring.jpa.properties.javax.cache.provider=org.ehcache.jsr107.EhcacheCachingProvider
```

```java
@Entity
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Department {
    @Id
    private Long id;
    private String name;
}
```

**Стратегии конкурентного доступа:**

| Стратегия | Описание | Применение |
|-----------|----------|------------|
| `READ_ONLY` | Только чтение, данные не меняются | Справочники, словари |
| `NONSTRICT_READ_WRITE` | Чтение/запись без строгой согласованности | Редко обновляемые данные |
| `READ_WRITE` | Чтение/запись с мягкими блокировками | Часто читаемые, редко обновляемые |
| `TRANSACTIONAL` | Полная транзакционная согласованность (JTA) | Критичные данные |

### Важное
- L2 Cache — опциональный, разделяемый между всеми Session
- Кэшируются **данные** (не объекты) — при чтении из L2 создаётся новый Java-объект
- L2 Cache эффективен для часто читаемых, редко обновляемых данных (справочники, каталоги)
- Провайдеры: Ehcache, Caffeine (через JCache), Infinispan, Hazelcast

### Частые ошибки
- **Кэшировать часто обновляемые данные** — L2 Cache добавит overhead на инвалидацию без выигрыша
- **Не кэшировать коллекции** — даже если сущность в кэше, её коллекции нужно кэшировать отдельно (`@Cache` на коллекции)
- **Забыть про инвалидацию при прямом SQL** — native queries и bulk operations обходят L2 Cache
- **L2 Cache в кластере без распределённого кэша** — каждый узел имеет свою копию, данные расходятся

### Как используется в 2026
- L2 Cache используется для справочных данных (страны, категории, настройки)
- Для distributed кэширования — Redis/Hazelcast вместо локального Ehcache
- Тренд: кэширование на уровне сервиса (Spring Cache + Redis) вместо L2 Cache Hibernate
- L2 Cache остаётся полезным для монолитов; в микросервисах — Spring Cache предпочтительнее

[к оглавлению](#Hibernate)

## Что такое Query Cache?

**Query Cache** — кэш результатов JPQL/HQL-запросов. Хранит не данные сущностей, а **список ID**, возвращённых запросом с определёнными параметрами.

**Как работает:**

```
Ключ кэша: запрос + параметры
Значение: список ID сущностей

При чтении: Query Cache → получить ID → L2 Cache → получить данные по ID → (если нет в L2 → БД)
```

**Настройка:**

```properties
spring.jpa.properties.hibernate.cache.use_query_cache=true
```

```java
// JPQL с кэшированием
List<User> users = entityManager.createQuery(
        "SELECT u FROM User u WHERE u.status = :status", User.class)
    .setParameter("status", UserStatus.ACTIVE)
    .setHint("org.hibernate.cacheable", true)
    .getResultList();

// Spring Data JPA
@QueryHints(@QueryHint(name = "org.hibernate.cacheable", value = "true"))
@Query("SELECT u FROM User u WHERE u.status = :status")
List<User> findByStatus(@Param("status") UserStatus status);
```

### Важное
- Query Cache хранит **список ID**, а не данные — работает в связке с L2 Cache
- Query Cache инвалидируется при **любом изменении** таблицы, участвующей в запросе
- Эффективен только для запросов, которые выполняются часто с одинаковыми параметрами
- Без L2 Cache — Query Cache бесполезен (ID есть, но данные придётся загружать из БД)

### Частые ошибки
- **Включать Query Cache без L2 Cache** — получив список ID из Query Cache, Hibernate пойдёт за каждой сущностью в БД (хуже, чем без кэша)
- **Кэшировать запросы к часто обновляемым таблицам** — любой UPDATE/INSERT/DELETE инвалидирует весь Query Cache для этой таблицы
- **Кэшировать все запросы подряд** — Query Cache полезен только для стабильных данных с повторяющимися параметрами

### Как используется в 2026
- Query Cache — нишевый инструмент, используется реже L2 Cache
- Для большинства задач кэширования на уровне запросов — Spring Cache + Redis
- Query Cache оправдан для тяжёлых аналитических запросов к неизменяемым данным

[к оглавлению](#Hibernate)

## HQL и JPQL — что это и чем отличаются от SQL?

**JPQL (Jakarta Persistence Query Language)** — объектно-ориентированный язык запросов, определённый спецификацией JPA. Работает с **сущностями и их полями**, а не с таблицами и столбцами.

**HQL (Hibernate Query Language)** — расширение JPQL от Hibernate с дополнительными возможностями.

**JPQL vs SQL:**

```sql
-- SQL: работает с таблицами
SELECT u.user_name, d.dept_name
FROM users u
JOIN departments d ON u.department_id = d.id
WHERE u.status = 'ACTIVE';

-- JPQL: работает с сущностями
SELECT u.name, u.department.name
FROM User u
WHERE u.status = com.example.UserStatus.ACTIVE
```

**Основные возможности JPQL:**

```java
// SELECT
List<User> users = em.createQuery(
    "SELECT u FROM User u WHERE u.name LIKE :name", User.class)
    .setParameter("name", "%John%")
    .getResultList();

// JOIN
List<User> users = em.createQuery(
    "SELECT u FROM User u JOIN u.department d WHERE d.name = :deptName", User.class)
    .setParameter("deptName", "Engineering")
    .getResultList();

// Агрегация
Long count = em.createQuery(
    "SELECT COUNT(u) FROM User u WHERE u.status = :status", Long.class)
    .setParameter("status", UserStatus.ACTIVE)
    .getSingleResult();

// DTO-проекция (конструктор)
List<UserDto> dtos = em.createQuery(
    "SELECT new com.example.UserDto(u.name, u.email) FROM User u", UserDto.class)
    .getResultList();

// UPDATE (bulk)
int updated = em.createQuery(
    "UPDATE User u SET u.status = :newStatus WHERE u.lastLogin < :date")
    .setParameter("newStatus", UserStatus.INACTIVE)
    .setParameter("date", cutoffDate)
    .executeUpdate();

// DELETE (bulk)
int deleted = em.createQuery(
    "DELETE FROM User u WHERE u.status = :status")
    .setParameter("status", UserStatus.DELETED)
    .executeUpdate();
```

**HQL-расширения (Hibernate-специфичные):**

```java
// WITH CLAUSE (Hibernate)
List<User> users = session.createQuery(
    "SELECT u FROM User u LEFT JOIN u.orders o WITH o.status = 'PAID'", User.class)
    .getResultList();
```

### Важное
- JPQL — стандарт JPA, HQL — расширение Hibernate; JPQL-код переносим между реализациями
- JPQL работает с **сущностями**, не таблицами: `FROM User u`, не `FROM users u`
- Навигация по связям через точку: `u.department.name` → Hibernate сгенерирует JOIN
- Bulk UPDATE/DELETE обходят Persistence Context и L1/L2 Cache — после них нужен `em.clear()`

### Частые ошибки
- **Конкатенация параметров в запросе** — SQL Injection! Всегда используйте `setParameter()`
- **Bulk UPDATE без `clear()`** — L1 Cache не знает об изменениях, данные в памяти устарели
- **Навигация по LAZY-связям в SELECT** — `SELECT u.department.name FROM User u` генерирует JOIN, но если department LAZY и не загружен, может привести к N+1 в других сценариях
- **Забывать про fetch** — `SELECT u FROM User u` не загружает LAZY-связи; для них нужен `JOIN FETCH`

### Как используется в 2026
- JPQL/HQL — основной инструмент для нетривиальных запросов в JPA-приложениях
- Spring Data `@Query` использует JPQL по умолчанию, native SQL — через `nativeQuery = true`
- Для type-safe запросов — Criteria API или QueryDSL
- HQL в Hibernate 6 получил новые функции: `ARRAY`, `JSON`, улучшенный синтаксис

[к оглавлению](#Hibernate)

## Criteria API — что это и когда использовать?

**Criteria API** — программный (type-safe) способ построения запросов в JPA. В отличие от JPQL (строковые запросы), Criteria API использует Java-объекты для построения запроса, что обеспечивает проверку на этапе компиляции.

```java
CriteriaBuilder cb = entityManager.getCriteriaBuilder();
CriteriaQuery<User> query = cb.createQuery(User.class);
Root<User> user = query.from(User.class);

// SELECT u FROM User u WHERE u.status = 'ACTIVE' AND u.name LIKE '%John%'
query.select(user)
     .where(
         cb.and(
             cb.equal(user.get("status"), UserStatus.ACTIVE),
             cb.like(user.get("name"), "%John%")
         )
     )
     .orderBy(cb.asc(user.get("name")));

List<User> result = entityManager.createQuery(query).getResultList();
```

**Type-safe Criteria с Metamodel (JPA Static Metamodel Generator):**

```java
// Hibernate генерирует User_ класс автоматически
CriteriaBuilder cb = entityManager.getCriteriaBuilder();
CriteriaQuery<User> query = cb.createQuery(User.class);
Root<User> user = query.from(User.class);

query.where(
    cb.equal(user.get(User_.status), UserStatus.ACTIVE) // User_.status — проверяется компилятором
);
```

**Динамические запросы — основное преимущество:**

```java
public List<User> search(String name, UserStatus status, String email) {
    CriteriaBuilder cb = entityManager.getCriteriaBuilder();
    CriteriaQuery<User> query = cb.createQuery(User.class);
    Root<User> user = query.from(User.class);

    List<Predicate> predicates = new ArrayList<>();

    if (name != null) {
        predicates.add(cb.like(cb.lower(user.get("name")), "%" + name.toLowerCase() + "%"));
    }
    if (status != null) {
        predicates.add(cb.equal(user.get("status"), status));
    }
    if (email != null) {
        predicates.add(cb.equal(user.get("email"), email));
    }

    query.where(predicates.toArray(new Predicate[0]));
    return entityManager.createQuery(query).getResultList();
}
```

**Spring Data JPA Specification (обёртка над Criteria API):**

```java
public class UserSpecifications {
    public static Specification<User> hasName(String name) {
        return (root, query, cb) -> cb.like(cb.lower(root.get("name")), "%" + name.toLowerCase() + "%");
    }

    public static Specification<User> hasStatus(UserStatus status) {
        return (root, query, cb) -> cb.equal(root.get("status"), status);
    }
}

// Использование
public interface UserRepository extends JpaRepository<User, Long>, JpaSpecificationExecutor<User> {}

List<User> users = userRepository.findAll(
    Specification.where(UserSpecifications.hasName("John"))
                 .and(UserSpecifications.hasStatus(UserStatus.ACTIVE))
);
```

### Важное
- Criteria API — type-safe построение запросов через Java-объекты
- Основное преимущество — **динамические запросы** с переменным набором условий
- Для статических запросов JPQL проще и читаемее
- Spring Data Specification — удобная обёртка над Criteria API

### Частые ошибки
- **Использовать Criteria для простых запросов** — JPQL намного читаемее для статических запросов
- **Строки вместо Metamodel** — `user.get("name")` не проверяется компилятором; опечатка обнаружится только в runtime
- **Не подключить Metamodel Generator** — без `User_` теряется основное преимущество type-safety

### Как используется в 2026
- Spring Data `Specification` — стандарт для динамических фильтров в REST API
- QueryDSL — популярная альтернатива Criteria API с более читаемым синтаксом (но проблемы с поддержкой Jakarta)
- Для сложных запросов — jOOQ набирает популярность как type-safe альтернатива

[к оглавлению](#Hibernate)

## Оптимистичная и пессимистичная блокировки

Блокировки решают проблему **потерянных обновлений (lost updates)**, когда два потока одновременно читают и обновляют одну и ту же запись.

**Оптимистичная блокировка (@Version):**

Предполагает, что конфликты редки. Проверяет версию записи при обновлении. Если версия изменилась — бросает исключение.

```java
@Entity
public class Product {
    @Id
    private Long id;

    private String name;
    private BigDecimal price;

    @Version  // Hibernate автоматически управляет этим полем
    private Long version;
}
```

Сгенерированный SQL:

```sql
-- При UPDATE Hibernate проверяет version:
UPDATE products SET name = ?, price = ?, version = 2
WHERE id = 1 AND version = 1;
-- Если version изменилась → 0 строк обновлено → OptimisticLockException
```

**Обработка конфликта:**

```java
@Service
public class ProductService {

    @Transactional
    public void updatePrice(Long id, BigDecimal newPrice) {
        try {
            Product product = productRepository.findById(id).orElseThrow();
            product.setPrice(newPrice);
        } catch (OptimisticLockException e) {
            // Стратегии: повторить, уведомить пользователя, merge
            throw new ConflictException("Продукт был изменён другим пользователем");
        }
    }
}
```

**Пессимистичная блокировка (LockModeType):**

Блокирует строку в БД (SELECT ... FOR UPDATE), предотвращая конкурентный доступ.

```java
// JPA
Product product = entityManager.find(Product.class, id,
    LockModeType.PESSIMISTIC_WRITE);
// SQL: SELECT * FROM products WHERE id = 1 FOR UPDATE

// Spring Data JPA
@Lock(LockModeType.PESSIMISTIC_WRITE)
@Query("SELECT p FROM Product p WHERE p.id = :id")
Optional<Product> findByIdForUpdate(@Param("id") Long id);
```

**Типы LockModeType:**

| Тип | SQL | Назначение |
|-----|-----|------------|
| `PESSIMISTIC_READ` | `FOR SHARE` | Разрешает concurrent reads, блокирует writes |
| `PESSIMISTIC_WRITE` | `FOR UPDATE` | Блокирует и reads, и writes |
| `PESSIMISTIC_FORCE_INCREMENT` | `FOR UPDATE` + version++ | Для update с инкрементом версии |

**Сравнение:**

| Критерий | Оптимистичная | Пессимистичная |
|----------|--------------|----------------|
| Механизм | Проверка версии при UPDATE | Блокировка строки в БД |
| Конфликт | Обнаружение (exception) | Предотвращение (ожидание) |
| Производительность | Выше при редких конфликтах | Выше при частых конфликтах |
| Deadlock | Нет | Возможен |

### Важное
- Оптимистичная — `@Version`, проверка при UPDATE, исключение при конфликте; подходит для большинства сценариев
- Пессимистичная — `FOR UPDATE`, блокировка строки; для критичных операций (списание средств, резервирование)
- `@Version` может быть `Long`, `Integer`, `Timestamp` (Long предпочтительнее)
- Оптимистичная блокировка работает и между HTTP-запросами (stateless)

### Частые ошибки
- **Не использовать блокировки вообще** — lost updates в конкурентном приложении неизбежны
- **Пессимистичная блокировка без таймаута** — бесконечное ожидание при deadlock; задайте `javax.persistence.lock.timeout`
- **`@Version` на поле, которое обновляется вручную** — Hibernate должен управлять version автоматически
- **Оптимистичная блокировка для частых конфликтов** — постоянные `OptimisticLockException` ухудшают UX; используйте пессимистичную

### Как используется в 2026
- Оптимистичная блокировка (`@Version`) — стандарт по умолчанию для большинства сущностей
- Пессимистичная — для финансовых операций, инвентаря, резервирования
- Для распределённых систем — distributed locks (Redis, Zookeeper) вместо БД-блокировок

[к оглавлению](#Hibernate)

## Стратегии наследования в Hibernate

JPA определяет три стратегии маппинга иерархии наследования в реляционные таблицы.

**1. SINGLE_TABLE (одна таблица):**

```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "type", discriminatorType = DiscriminatorType.STRING)
public abstract class Payment {
    @Id @GeneratedValue
    private Long id;
    private BigDecimal amount;
}

@Entity
@DiscriminatorValue("CARD")
public class CardPayment extends Payment {
    private String cardNumber;  // null для других типов
}

@Entity
@DiscriminatorValue("CASH")
public class CashPayment extends Payment {
    private String currency;
}
```

Таблица: `payment(id, type, amount, card_number, currency)` — все поля в одной таблице, неиспользуемые = NULL.

**2. JOINED (таблица на класс):**

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Payment {
    @Id @GeneratedValue
    private Long id;
    private BigDecimal amount;
}

@Entity
public class CardPayment extends Payment {
    private String cardNumber;
}
```

Таблицы: `payment(id, amount)` + `card_payment(id, card_number)` — данные разнесены, связаны по PK/FK.

**3. TABLE_PER_CLASS (таблица на конкретный класс):**

```java
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class Payment {
    @Id @GeneratedValue(strategy = GenerationType.TABLE)
    private Long id;
    private BigDecimal amount;
}
```

Таблицы: `card_payment(id, amount, card_number)`, `cash_payment(id, amount, currency)` — дублирование общих полей.

**Сравнение:**

| Критерий | SINGLE_TABLE | JOINED | TABLE_PER_CLASS |
|----------|-------------|--------|-----------------|
| Производительность полиморфных запросов | Лучшая (одна таблица) | Средняя (JOIN) | Худшая (UNION) |
| Нормализация | Низкая (NULL-столбцы) | Высокая | Средняя |
| NOT NULL ограничения | Невозможны для подклассов | Возможны | Возможны |
| Добавление подкласса | Простое (новый столбец) | Новая таблица + FK | Новая таблица |

### Важное
- **SINGLE_TABLE** — по умолчанию и самая производительная; идеальна когда подклассов немного
- **JOINED** — когда важна нормализация и NOT NULL ограничения; ценой производительности на JOIN
- **TABLE_PER_CLASS** — используется редко; полиморфные запросы требуют UNION ALL
- `@DiscriminatorColumn` — указывает столбец-дискриминатор для SINGLE_TABLE и JOINED

### Частые ошибки
- **TABLE_PER_CLASS с полиморфными запросами** — `findAll()` для базового класса генерирует UNION ALL по всем таблицам
- **SINGLE_TABLE с NOT NULL** — поля подклассов не могут быть NOT NULL (они null для других типов)
- **Глубокая иерархия с JOINED** — каждый уровень наследования = дополнительный JOIN

### Как используется в 2026
- SINGLE_TABLE — стандартный выбор для большинства случаев
- Альтернатива наследованию: композиция через `@Embedded` или паттерн «тип + JSON-данные»
- В DDD: наследование Entity используется для Value Objects и Aggregate Roots с ограниченной иерархией

[к оглавлению](#Hibernate)

## Что такое Dirty Checking и Flush?

**Dirty Checking** — механизм Hibernate, автоматически отслеживающий изменения Persistent-сущностей. При flush Hibernate сравнивает текущее состояние объекта с его снимком (snapshot), сделанным при загрузке, и генерирует UPDATE для изменённых полей.

```java
@Transactional
public void updateUser(Long id) {
    User user = entityManager.find(User.class, id);  // загрузка + snapshot
    user.setName("New Name");  // изменение в памяти
    // НЕ нужен вызов save() или update()!
    // При commit Hibernate обнаружит изменение и выполнит UPDATE
}
```

**Flush** — процесс синхронизации Persistence Context с базой данных. При flush все накопленные изменения (INSERT, UPDATE, DELETE) отправляются в БД.

**Когда происходит flush:**

1. При `commit()` транзакции (автоматически)
2. Перед выполнением JPQL/HQL-запроса (чтобы запрос видел актуальные данные)
3. При явном вызове `entityManager.flush()`

**FlushMode:**

```java
// AUTO (по умолчанию) — flush перед запросами и при commit
entityManager.setFlushMode(FlushModeType.AUTO);

// COMMIT — flush только при commit (не перед запросами)
entityManager.setFlushMode(FlushModeType.COMMIT);
```

**Порядок операций при flush:**

1. INSERT (новые сущности — `persist()`)
2. UPDATE (изменённые сущности — Dirty Checking)
3. DELETE коллекций
4. INSERT/UPDATE коллекций
5. DELETE (удалённые сущности — `remove()`)

### Важное
- Dirty Checking автоматический — изменили поле Persistent-объекта → UPDATE при flush
- Flush ≠ commit; flush отправляет SQL, commit фиксирует транзакцию
- `@DynamicUpdate` — генерирует UPDATE только для изменённых столбцов (по умолчанию — все столбцы)
- `@Transactional(readOnly = true)` — подсказка Hibernate отключить Dirty Checking (оптимизация)

### Частые ошибки
- **Вызов `save()` для уже managed-сущности** — Spring Data `save()` вызовет `merge()`, создав лишнюю копию; для managed-сущностей достаточно изменить поля
- **Неожиданный UPDATE** — изменили поле managed-объекта случайно → Hibernate выполнит UPDATE при flush
- **Порядок flush** — DELETE выполняется после INSERT/UPDATE; это может нарушить ограничения уникальности
- **Flush в цикле** — `entityManager.flush()` в цикле может привести к деградации производительности

### Как используется в 2026
- Dirty Checking — фундаментальный механизм, работает прозрачно
- `@DynamicUpdate` рекомендуется для таблиц с большим количеством столбцов
- `readOnly = true` — обязательный паттерн для read-only транзакций (экономит память и CPU)

[к оглавлению](#Hibernate)

## Что такое EntityManager и чем он отличается от Session?

**EntityManager** — JPA-интерфейс для управления персистентными сущностями. **Session** — Hibernate-интерфейс, расширяющий `EntityManager` дополнительными возможностями.

```java
// EntityManager (JPA — стандартный)
@PersistenceContext
private EntityManager entityManager;

entityManager.persist(entity);      // сохранить
entityManager.find(User.class, id); // найти
entityManager.merge(entity);        // слить Detached
entityManager.remove(entity);       // удалить
entityManager.createQuery("...");   // JPQL-запрос

// Session (Hibernate — расширенный)
Session session = entityManager.unwrap(Session.class);

session.byNaturalId(User.class).using("email", email).load(); // поиск по натуральному ключу
session.byMultipleIds(User.class).multiLoad(1L, 2L, 3L);      // batch load
session.setDefaultReadOnly(true);                               // read-only для всей сессии
session.enableFilter("activeOnly");                             // динамические фильтры
```

**Основные различия:**

| Возможность | EntityManager (JPA) | Session (Hibernate) |
|-------------|-------------------|-------------------|
| `persist()` | Да | Да |
| `find()` | Да | Да |
| Natural ID | Нет | `byNaturalId()` |
| Multi-load | Нет | `byMultipleIds()` |
| Фильтры | Нет | `enableFilter()` |
| Read-only mode | Нет | `setDefaultReadOnly()` |

### Важное
- `EntityManager` — стандартный JPA API, переносимый между реализациями
- `Session` — Hibernate-специфичный, с дополнительными возможностями
- В Spring приложении используйте `EntityManager`, переключаясь на `Session` через `unwrap()` только при необходимости
- Spring Data JPA полностью абстрагирует от EntityManager/Session

### Частые ошибки
- **Использовать Session без необходимости** — теряется переносимость кода
- **Создавать EntityManager вручную** — в Spring используйте `@PersistenceContext` или Spring Data
- **Путать `persist()` и `merge()`** — `persist` для новых (transient), `merge` для detached; `persist` бросит исключение для detached

### Как используется в 2026
- Spring Data JPA — основной API; прямой доступ к EntityManager — для кастомных запросов
- Session используется для Hibernate-специфичных оптимизаций (batch load, natural ID, filters)

[к оглавлению](#Hibernate)

## Как работает каскадирование (CascadeType)?

**Каскадирование** определяет, какие операции с родительской сущностью автоматически распространяются на связанные (дочерние) сущности.

```java
@Entity
public class Order {
    @Id @GeneratedValue
    private Long id;

    @OneToMany(mappedBy = "order",
               cascade = CascadeType.ALL,     // каскад всех операций
               orphanRemoval = true)           // удалять «осиротевших» детей
    private List<OrderItem> items = new ArrayList<>();
}
```

**Типы каскадирования:**

| CascadeType | Операция | Описание |
|-------------|----------|----------|
| `PERSIST` | persist/save | Новые дочерние сущности сохраняются вместе с родителем |
| `MERGE` | merge | Detached дочерние сущности сливаются вместе с родителем |
| `REMOVE` | remove/delete | Дочерние сущности удаляются при удалении родителя |
| `REFRESH` | refresh | Дочерние сущности перезагружаются из БД |
| `DETACH` | detach | Дочерние сущности отсоединяются от Persistence Context |
| `ALL` | все вышеперечисленные | Все операции каскадируются |

**`orphanRemoval = true`** — удаляет дочернюю сущность при удалении из коллекции родителя:

```java
@Transactional
public void removeItem(Long orderId, Long itemId) {
    Order order = orderRepository.findById(orderId).orElseThrow();
    order.getItems().removeIf(item -> item.getId().equals(itemId));
    // orphanRemoval=true → Hibernate выполнит DELETE FROM order_items WHERE id = ?
}
```

### Важное
- `CascadeType.ALL` = PERSIST + MERGE + REMOVE + REFRESH + DETACH
- `orphanRemoval` — удаляет дочернюю сущность, когда она убирается из коллекции родителя
- Каскад распространяется **только от родителя к ребёнку**, не наоборот
- Используйте каскад только для связей «родитель владеет ребёнком» (Order → OrderItem), не для ссылочных связей (Order → User)

### Частые ошибки
- **`CascadeType.ALL` для `@ManyToOne`** — удаление Order не должно удалять User; каскад здесь опасен
- **`CascadeType.REMOVE` без `orphanRemoval`** — REMOVE срабатывает при `remove(order)`, но не при `order.getItems().remove(item)`
- **Каскад на ассоциациях с общими сущностями** — если Department каскадно удаляет Users, удаление отдела удалит всех пользователей
- **`orphanRemoval` на `@ManyToMany`** — концептуально бессмысленно; удаление из связи не должно удалять сущность

### Как используется в 2026
- `CascadeType.ALL + orphanRemoval` — стандарт для parent-child отношений (Order-OrderItem, Post-Comment)
- Для слабых связей (User-Department) — каскад не используется
- В DDD: каскад только внутри Aggregate Root → его Entity

[к оглавлению](#Hibernate)

## Что такое LazyInitializationException и как его избежать?

**LazyInitializationException** — исключение, возникающее при обращении к LAZY-связи после закрытия Session (Persistence Context).

```java
@Transactional
public User getUser(Long id) {
    return userRepository.findById(id).orElseThrow();
    // Session закрывается при выходе из @Transactional
}

// В контроллере:
User user = userService.getUser(1L);
user.getOrders().size(); // LazyInitializationException!
// Session уже закрыта, прокси не может загрузить данные
```

**Способы решения:**

**1. JOIN FETCH — загрузить связь в запросе:**

```java
@Query("SELECT u FROM User u JOIN FETCH u.orders WHERE u.id = :id")
Optional<User> findByIdWithOrders(@Param("id") Long id);
```

**2. @EntityGraph — декларативная загрузка:**

```java
@EntityGraph(attributePaths = {"orders"})
Optional<User> findById(Long id);
```

**3. DTO-проекция — вернуть только нужные данные:**

```java
@Transactional(readOnly = true)
public UserDto getUser(Long id) {
    User user = userRepository.findByIdWithOrders(id).orElseThrow();
    return new UserDto(user.getName(), user.getOrders().stream()
        .map(OrderDto::from).toList());
}
```

**4. Инициализация внутри транзакции:**

```java
@Transactional(readOnly = true)
public User getUser(Long id) {
    User user = userRepository.findById(id).orElseThrow();
    Hibernate.initialize(user.getOrders()); // принудительная загрузка
    return user;
}
```

**5. Open Session in View (OSIV) — anti-pattern:**

```properties
spring.jpa.open-in-view=true  # по умолчанию в Spring Boot — Session открыта до рендеринга View
```

### Важное
- LazyInitializationException = обращение к LAZY-прокси после закрытия Session
- **Лучшее решение: JOIN FETCH или @EntityGraph** — загрузка нужных данных в сервисном слое
- OSIV (`spring.jpa.open-in-view=true`) решает проблему, но создаёт новые: долгие транзакции, непредсказуемые запросы
- Рекомендация: `open-in-view=false` + явная загрузка данных в сервисах

### Частые ошибки
- **Полагаться на OSIV** — маскирует проблему; запросы к БД выполняются из контроллера/view, усложняя отладку
- **`Hibernate.initialize()` для больших коллекций** — загружает все элементы в память; для 10K записей лучше пагинация
- **Возвращать Entity из контроллера** — при сериализации в JSON Jackson обращается к LAZY-полям → исключение; используйте DTO

### Как используется в 2026
- Стандартный подход: `open-in-view=false` + DTO-проекции + JOIN FETCH
- Spring Boot предупреждает о включённом OSIV в логах при старте
- В REST API — всегда DTO, никогда Entity в ответе; это решает и LazyInitializationException, и проблемы безопасности

[к оглавлению](#Hibernate)

## Что такое проекции и DTO-маппинг?

**Проекции** позволяют загружать только нужные поля из БД, а не полные сущности. Это оптимизирует производительность и решает проблемы с LAZY-загрузкой и безопасностью (не отдавать лишние данные).

**1. DTO через JPQL-конструктор:**

```java
public record UserDto(String name, String email) {}

@Query("SELECT new com.example.UserDto(u.name, u.email) FROM User u WHERE u.id = :id")
Optional<UserDto> findUserDtoById(@Param("id") Long id);
// SQL: SELECT u.name, u.email FROM users u WHERE u.id = ?
```

**2. Interface-based проекция (Spring Data):**

```java
public interface UserSummary {
    String getName();
    String getEmail();

    @Value("#{target.name + ' <' + target.email + '>'}")
    String getDisplayName(); // вычисляемое поле (SpEL)
}

public interface UserRepository extends JpaRepository<User, Long> {
    List<UserSummary> findByStatus(UserStatus status);
    // Spring Data автоматически загружает только name и email
}
```

**3. Class-based проекция (DTO класс):**

```java
public record UserDto(String name, String email) {}

List<UserDto> findByStatus(UserStatus status);
// Spring Data маппит результат в DTO автоматически (по именам полей)
```

**4. Tuple (кортеж) — для ad-hoc запросов:**

```java
List<Tuple> results = entityManager.createQuery(
    "SELECT u.name AS name, COUNT(o) AS orderCount " +
    "FROM User u LEFT JOIN u.orders o GROUP BY u.name", Tuple.class)
    .getResultList();

results.forEach(t -> System.out.println(
    t.get("name") + ": " + t.get("orderCount")));
```

### Важное
- Проекции загружают только нужные столбцы → меньше трафика, памяти, нет LAZY-проблем
- Interface-based проекции — удобны, но создают прокси (чуть медленнее)
- Record DTO — оптимальный по производительности вариант
- Проекции не отслеживаются Persistence Context → нет Dirty Checking

### Частые ошибки
- **Загружать полную Entity для read-only отображения** — лишний overhead: Dirty Checking, snapshot, L1 Cache
- **Смешивать проекции и Entity** — проекция возвращает DTO, не managed Entity; изменения в DTO не попадут в БД
- **Сложные SpEL в interface-проекциях** — сложнее для отладки и тестирования

### Как используется в 2026
- DTO-проекции — стандарт для REST API (разделение read/write модели — CQRS-lite)
- Java Records идеально подходят для DTO — иммутабельные, компактные
- Spring Data автоматически оптимизирует SELECT при использовании interface/class проекций

[к оглавлению](#Hibernate)

## Batch-операции в Hibernate

**Batch-операции** позволяют выполнять массовые INSERT/UPDATE/DELETE эффективно, группируя SQL-запросы в пакеты вместо отправки по одному.

**Настройка batch insert/update:**

```properties
# application.properties
spring.jpa.properties.hibernate.jdbc.batch_size=50
spring.jpa.properties.hibernate.order_inserts=true
spring.jpa.properties.hibernate.order_updates=true
spring.jpa.properties.hibernate.jdbc.batch_versioned_data=true
```

**Batch INSERT:**

```java
@Transactional
public void batchInsert(List<User> users) {
    for (int i = 0; i < users.size(); i++) {
        entityManager.persist(users.get(i));

        if (i % 50 == 0) { // batch_size = 50
            entityManager.flush();  // отправить SQL в БД
            entityManager.clear();  // очистить L1 Cache (освободить память)
        }
    }
}
```

**IDENTITY vs SEQUENCE для batch:**

```java
// IDENTITY — batch insert НЕВОЗМОЖЕН!
// Hibernate выполняет INSERT сразу для получения ID
@GeneratedValue(strategy = GenerationType.IDENTITY) // ❌ для batch

// SEQUENCE — batch insert РАБОТАЕТ
// Hibernate заранее получает блок ID из sequence
@GeneratedValue(strategy = GenerationType.SEQUENCE,
                generator = "user_seq")
@SequenceGenerator(name = "user_seq", allocationSize = 50) // ✅ для batch
```

**Bulk UPDATE/DELETE (JPQL — обходит Persistence Context):**

```java
@Modifying
@Query("UPDATE User u SET u.status = :status WHERE u.lastLogin < :date")
int deactivateInactiveUsers(@Param("status") UserStatus status,
                            @Param("date") LocalDateTime date);
// Один SQL UPDATE, без загрузки сущностей
// ВАЖНО: @Modifying(clearAutomatically = true) — очистить L1 Cache после
```

**Spring Data `saveAll()` с batch:**

```java
// saveAll() использует batch, если настроен batch_size
userRepository.saveAll(users); // batch INSERT/UPDATE
```

### Важное
- `batch_size` — количество SQL-операций, группируемых в один batch
- `order_inserts/order_updates` — **обязательно** для эффективного batch (группировка по таблицам)
- `flush() + clear()` каждые N записей — предотвращение OutOfMemoryError при больших batch
- SEQUENCE стратегия генерации ID — обязательна для batch INSERT (IDENTITY не поддерживает batch)

### Частые ошибки
- **IDENTITY + batch** — Hibernate выполняет INSERT по одному; batch не работает
- **Не вызывать `clear()`** — L1 Cache растёт с каждой persist(), 1M сущностей → OOM
- **Bulk UPDATE без `clearAutomatically`** — L1 Cache содержит устаревшие данные, следующий `find()` вернёт старое значение
- **Не включить `order_inserts`** — без сортировки Hibernate перемешивает INSERT разных таблиц, batch не формируется

### Как используется в 2026
- Для массовых операций (импорт данных, миграции) — JDBC batch напрямую (через `JdbcTemplate`) или jOOQ
- Hibernate batch — для умеренных объёмов (тысячи, не миллионы)
- Spring Batch — для ETL-задач с миллионами записей
- `@Modifying` JPQL — для bulk UPDATE/DELETE; самый простой способ массового обновления

[к оглавлению](#Hibernate)
