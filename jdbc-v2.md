[Вопросы для собеседования](README.md)

# JDBC

+ [Что такое JDBC](#что-такое-jdbc)
+ [В чем заключаются преимущества использования JDBC](#в-чем-заключаются-преимущества-использования-jdbc)
+ [Что из себя представляет JDBC URL](#что-из-себя-представляет-jdbc-url)
+ [Из каких частей состоит JDBC](#из-каких-частей-состоит-jdbc)
+ [Перечислите основные классы и интерфейсы JDBC](#перечислите-основные-классы-и-интерфейсы-jdbc)
+ [Опишите основные этапы работы с базой данных при использовании JDBC](#опишите-основные-этапы-работы-с-базой-данных-при-использовании-jdbc)
+ [Перечислите основные типы данных используемые в JDBC и как они связаны с типами Java](#перечислите-основные-типы-данных-используемые-в-jdbc-и-как-они-связаны-с-типами-java)
+ [Как зарегистрировать драйвер JDBC](#как-зарегистрировать-драйвер-jdbc)
+ [Как установить соединение с базой данных](#как-установить-соединение-с-базой-данных)
+ [Какие уровни изоляции транзакций поддерживаются в JDBC](#какие-уровни-изоляции-транзакций-поддерживаются-в-jdbc)
+ [При помощи чего формируются запросы к базе данных](#при-помощи-чего-формируются-запросы-к-базе-данных)
+ [Чем отличается Statement от PreparedStatement](#чем-отличается-statement-от-preparedstatement)
+ [Как осуществляется запрос к базе данных и обработка результатов](#как-осуществляется-запрос-к-базе-данных-и-обработка-результатов)
+ [Как вызвать хранимую процедуру](#как-вызвать-хранимую-процедуру)
+ [Как закрыть соединение с базой данных](#как-закрыть-соединение-с-базой-данных)
+ [Что такое Connection Pool и зачем он нужен](#что-такое-connection-pool-и-зачем-он-нужен)
+ [Что такое HikariCP и как его настроить](#что-такое-hikaricp-и-как-его-настроить)
+ [Как выполнять batch-операции в JDBC](#как-выполнять-batch-операции-в-jdbc)
+ [Как управлять транзакциями в JDBC](#как-управлять-транзакциями-в-jdbc)
+ [Какие есть альтернативы чистому JDBC](#какие-есть-альтернативы-чистому-jdbc)

## Что такое JDBC
<!-- grade: junior -->

JDBC (Java DataBase Connectivity) — это промышленный стандарт взаимодействия Java-приложений с различными СУБД, реализованный в виде пакетов `java.sql` и `javax.sql`, входящих в состав Java SE.

> Аналогия из жизни: JDBC — это как универсальная розетка-переходник для путешественника. Вы не думаете о том, какой стандарт розетки в конкретной стране (MySQL, PostgreSQL, Oracle) — переходник (JDBC-драйвер) обеспечивает совместимость вашего устройства (Java-приложения) с любой сетью.

JDBC основан на концепции драйверов, которые позволяют получать соединение с базой данных по специально описанному URL. При загрузке драйвер регистрирует себя в системе и в дальнейшем автоматически вызывается, когда программа требует URL, содержащий протокол, за который этот драйвер отвечает.

> **На собеседовании:** интервьюер ожидает услышать, что JDBC — это стандартный API для работы с реляционными БД из Java, основанный на концепции драйверов. Ключевое преимущество — абстракция: код не зависит от конкретной СУБД. Частая ошибка — путать JDBC с ORM (Hibernate) или с конкретным драйвером.

[к оглавлению](#jdbc)

---

## В чем заключаются преимущества использования JDBC
<!-- grade: junior -->

Преимущества JDBC — это набор свойств, которые делают этот API удобным для работы с базами данных из Java.

- Легкость разработки: разработчик может не знать специфики базы данных, с которой работает
- Код практически не меняется при переходе на другую базу данных (количество изменений зависит исключительно от различий между диалектами SQL)
- Не нужно дополнительно устанавливать клиентскую программу
- К любой базе данных можно подсоединиться через легко описываемый URL

> **На собеседовании:** достаточно назвать 2-3 преимущества. Главное — подчеркнуть абстракцию от конкретной СУБД и унифицированный интерфейс. Это вопрос-разминка, на нем не задерживаются.

[к оглавлению](#jdbc)

---

## Что из себя представляет JDBC URL
<!-- grade: junior -->

JDBC URL — это строка подключения к базе данных, состоящая из трех частей: протокола, подпротокола и подимени.

- `<protocol>:` (протокол) — всегда `jdbc:`
- `<subprotocol>:` (подпротокол) — имя драйвера или механизма соединения с базой данных. Например, `mysql`, `postgresql`, `oracle`
- `<subname>` (подимя) — идентификатор базы данных. Обычно содержит сетевой адрес в формате `//<hostname>:<port>/<database>`

Пример JDBC URL для подключения к MySQL базе данных «Test» на localhost:

```
jdbc:mysql://localhost:3306/Test
```

Примеры для разных СУБД:

| СУБД | JDBC URL |
|------|----------|
| MySQL | `jdbc:mysql://localhost:3306/mydb` |
| PostgreSQL | `jdbc:postgresql://localhost:5432/mydb` |
| Oracle | `jdbc:oracle:thin:@localhost:1521:mydb` |
| H2 (in-memory) | `jdbc:h2:mem:testdb` |

> **На собеседовании:** важно знать структуру URL (jdbc:подпротокол://хост:порт/бд) и уметь привести пример для конкретной СУБД. Иногда просят написать URL прямо на доске.

[к оглавлению](#jdbc)

---

## Из каких частей состоит JDBC
<!-- grade: junior -->

JDBC состоит из двух частей, которые работают вместе для обеспечения доступа к базам данных из Java.

- JDBC API — набор классов и интерфейсов, определяющих доступ к базам данных, объявленных в пакетах `java.sql` и `javax.sql`
- JDBC-драйвер — компонент, специфичный для каждой базы данных

JDBC превращает вызовы уровня API в «родные» команды того или иного сервера баз данных.

> **На собеседовании:** достаточно назвать две части — API (стандартные интерфейсы) и драйвер (реализация для конкретной СУБД). Это демонстрирует понимание паттерна Bridge — разделение абстракции и реализации.

[к оглавлению](#jdbc)

---

## Перечислите основные классы и интерфейсы JDBC
<!-- grade: junior -->

Основные классы и интерфейсы JDBC — это набор типов из пакетов `java.sql` и `javax.sql`, образующих API для работы с базой данных.

| Класс/Интерфейс | Назначение |
|-----------------|------------|
| `java.sql.DriverManager` | Загрузка и регистрация JDBC-драйвера, получение соединения |
| `javax.sql.DataSource` | Альтернатива DriverManager с поддержкой пула соединений |
| `java.sql.Connection` | Формирование запросов и управление транзакциями |
| `java.sql.Statement` | Отправка простых SQL-запросов без параметров |
| `java.sql.PreparedStatement` | Отправка параметризованных SQL-запросов |
| `java.sql.CallableStatement` | Вызов хранимых процедур |
| `java.sql.ResultSet` | Перемещение по набору данных и чтение полей |
| `java.sql.ResultSetMetaData` | Информация о структуре набора данных |
| `java.sql.DatabaseMetaData` | Информация о структуре источника данных |

Также существуют расширения: `javax.sql.ConnectionPoolDataSource` и `javax.sql.XADataSource` для поддержки пула соединений, `javax.sql.PooledConnection` и `javax.sql.XAConnection` для управления соединениями в пуле.

> **На собеседовании:** ключевые интерфейсы, которые нужно знать: Connection, Statement, PreparedStatement, ResultSet и DataSource. Интервьюер часто спрашивает цепочку вызовов: DriverManager/DataSource -> Connection -> PreparedStatement -> ResultSet. Покажите, что понимаете роль каждого звена.

[к оглавлению](#jdbc)

---

## Опишите основные этапы работы с базой данных при использовании JDBC
<!-- grade: junior -->

Этапы работы с базой данных при использовании JDBC — это последовательность шагов, которую проходит каждый JDBC-запрос от подключения до освобождения ресурсов.

1. Регистрация драйверов
2. Установление соединения с базой данных
3. Создание запроса(ов) к базе данных
4. Выполнение запроса(ов) к базе данных
5. Обработка результата(ов)
6. Закрытие соединения с базой данных

> **На собеседовании:** перечислите шаги в правильном порядке. Частая ошибка — забыть про закрытие ресурсов (шаг 6). Отдельный плюс — упомянуть try-with-resources как способ гарантировать закрытие.

[к оглавлению](#jdbc)

---

## Перечислите основные типы данных используемые в JDBC и как они связаны с типами Java
<!-- grade: junior -->

Типы данных JDBC — это набор констант из класса `java.sql.Types`, определяющих маппинг между SQL-типами и Java-типами.

| JDBC Type | Java Object Type |
|-----------:|---------------------------|
| CHAR | `String` |
| VARCHAR | `String` |
| LONGVARCHAR | `String` |
| NUMERIC | `java.math.BigDecimal` |
| DECIMAL | `java.math.BigDecimal` |
| BIT | `Boolean` |
| TINYINT | `Integer` |
| SMALLINT | `Integer` |
| INTEGER | `Integer` |
| BIGINT | `Long` |
| REAL | `Float` |
| FLOAT | `Double` |
| DOUBLE | `Double` |
| BINARY | `byte[]` |
| VARBINARY | `byte[]` |
| LONGVARBINARY | `byte[]` |
| DATE | `java.sql.Date` |
| TIME | `java.sql.Time` |
| TIMESTAMP | `java.sql.Timestamp` |
| CLOB | `Clob` |
| BLOB | `Blob` |
| ARRAY | `Array` |
| STRUCT | `Struct` |
| REF | `Ref` |
| DISTINCT | сопоставление базового типа |
| JAVA_OBJECT | базовый класс Java |

> **На собеседовании:** не нужно заучивать всю таблицу. Важно знать основные маппинги: VARCHAR -> String, INTEGER -> Integer, BIGINT -> Long, TIMESTAMP -> java.sql.Timestamp, NUMERIC/DECIMAL -> BigDecimal. Частый вопрос-ловушка: FLOAT в JDBC маппится на Double, а не на Float.

[к оглавлению](#jdbc)

---

## Как зарегистрировать драйвер JDBC
<!-- grade: junior -->

Регистрация драйвера JDBC — это процесс загрузки класса драйвера в JVM и его добавления в реестр DriverManager.

Регистрацию можно осуществить несколькими способами:

- `java.sql.DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver())` — явная регистрация объекта драйвера
- `Class.forName("com.mysql.cj.jdbc.Driver")` — загрузка класса драйвера, который регистрирует себя в статическом блоке инициализации
- `Class.forName("com.mysql.cj.jdbc.Driver").newInstance()` — загрузка и создание экземпляра (устаревший подход)

Начиная с JDBC 4.0 (Java 6+), драйвер регистрируется автоматически через механизм SPI (Service Provider Interface) — достаточно добавить JAR-файл драйвера в classpath.

> **На собеседовании:** скажите, что с JDBC 4.0 ручная регистрация не нужна — драйвер подгружается автоматически через SPI. Но если спросят о ручном способе, назовите `Class.forName(...)`. Знание механизма SPI показывает более глубокое понимание.

[к оглавлению](#jdbc)

---

## Как установить соединение с базой данных
<!-- grade: junior -->

Установление соединения с базой данных — это процесс создания объекта `java.sql.Connection`, представляющего сессию работы с СУБД.

Для установки соединения используется статический вызов `java.sql.DriverManager.getConnection(...)` с одним из вариантов параметров:

```java
// Только URL
Connection conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/mydb");

// URL + свойства
Properties props = new Properties();
props.setProperty("user", "admin");
props.setProperty("password", "secret");
Connection conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/mydb", props);

// URL + логин + пароль
Connection conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/mydb", "admin", "secret");
```

В результате вызова будет установлено соединение с базой данных и создан объект `Connection` — своеобразная «сессия», внутри контекста которой происходит дальнейшая работа с базой данных.

В продуктивном коде вместо DriverManager используется DataSource (с поддержкой пула соединений):

```java
DataSource dataSource = ...; // HikariCP, Tomcat Pool и т.д.
Connection conn = dataSource.getConnection();
```

> **На собеседовании:** назовите три перегрузки getConnection. Обязательно упомяните, что в реальных проектах используется DataSource, а не DriverManager, потому что DataSource поддерживает пул соединений. Это показывает практический опыт.

[к оглавлению](#jdbc)

---

## Какие уровни изоляции транзакций поддерживаются в JDBC
<!-- grade: middle -->

Уровень изолированности транзакций — это значение, определяющее степень, при которой одна транзакция изолирована от изменений, произведённых другими параллельными транзакциями.

Более высокий уровень изолированности повышает точность данных, но может снижать количество параллельно выполняемых транзакций. Более низкий уровень позволяет выполнять больше параллельных транзакций, но снижает точность данных.

### Аномалии при параллельных транзакциях

- Грязное чтение (dirty read) — чтение данных, изменённых транзакцией, которая впоследствии откатится
- Неповторяющееся чтение (non-repeatable read) — при повторном чтении ранее прочитанные данные оказываются изменёнными
- Фантомное чтение (phantom reads) — при повторном чтении одна и та же выборка дает разные множества строк

### Уровни изоляции

Уровни определены как константы интерфейса `java.sql.Connection`:

| Уровень | Dirty Read | Non-Repeatable Read | Phantom Read |
|---------|:----------:|:-------------------:|:------------:|
| `TRANSACTION_READ_UNCOMMITTED` | Да | Да | Да |
| `TRANSACTION_READ_COMMITTED` | Нет | Да | Да |
| `TRANSACTION_REPEATABLE_READ` | Нет | Нет | Да |
| `TRANSACTION_SERIALIZABLE` | Нет | Нет | Нет |

Также существует `TRANSACTION_NONE` — драйвер не поддерживает транзакции.

Уровень изоляции можно задать методом `setTransactionIsolation()` и получить через `getTransactionIsolation()` объекта `Connection`.

Не все СУБД поддерживают все уровни изоляции. Информацию о поддерживаемых уровнях можно получить через `java.sql.DatabaseMetaData`.

> **На собеседовании:** нарисуйте или расскажите таблицу: какой уровень какие аномалии допускает. Самый частый вопрос — разница между READ_COMMITTED и REPEATABLE_READ. По умолчанию большинство СУБД используют READ_COMMITTED (PostgreSQL, Oracle) или REPEATABLE_READ (MySQL InnoDB).

[к оглавлению](#jdbc)

---

## При помощи чего формируются запросы к базе данных
<!-- grade: junior -->

Запросы к базе данных в JDBC формируются при помощи трёх интерфейсов, каждый из которых предназначен для определённого типа SQL-операций.

| Интерфейс | Назначение | Метод создания |
|-----------|-----------|----------------|
| `Statement` | Простые SQL-запросы без параметров | `connection.createStatement()` |
| `PreparedStatement` | Параметризованные и часто выполняемые запросы | `connection.prepareStatement(sql)` |
| `CallableStatement` | Вызов хранимых процедур | `connection.prepareCall(sql)` |

> **На собеседовании:** назовите три интерфейса и способ получения каждого через Connection. Обычно следом идёт вопрос о разнице между Statement и PreparedStatement — будьте готовы к нему.

[к оглавлению](#jdbc)

---

## Чем отличается Statement от PreparedStatement
<!-- grade: junior -->

Statement и PreparedStatement — это два интерфейса для выполнения SQL-запросов, различающиеся по способу компиляции запроса и уровню безопасности.

| Критерий | Statement | PreparedStatement |
|----------|-----------|-------------------|
| Параметры | Нет (значения вставляются в строку SQL) | Да (параметры подставляются через `?`) |
| Компиляция | Каждый раз заново | Однократно, план кэшируется СУБД |
| SQL Injection | Уязвим | Защищён |
| Повторное использование | Неэффективно | Эффективно при повторных вызовах |
| Когда использовать | DDL, одноразовые запросы | DML, параметризованные запросы |

Перед выполнением СУБД разбирает каждый запрос, оптимизирует его и создает план (query plan) выполнения. Если один и тот же запрос выполняется несколько раз, СУБД может кэшировать план и не производить этапов разборки и оптимизации повторно.

PreparedStatement выгодно отличается от Statement тем, что при повторном использовании с одним или несколькими наборами параметров позволяет получить преимущества заранее прекомпилированного и кэшированного запроса, помогая при этом избежать SQL Injection.

> **На собеседовании:** два ключевых отличия: (1) PreparedStatement защищает от SQL Injection, (2) запрос компилируется один раз и переиспользуется. Правило: всегда используйте PreparedStatement, кроме случаев DDL без параметров.

[к оглавлению](#jdbc)

---

## Как осуществляется запрос к базе данных и обработка результатов
<!-- grade: junior -->

Выполнение запросов и обработка результатов — это процесс отправки SQL-команд через Statement и навигации по ResultSet для получения данных.

Выполнение запросов осуществляется при помощи методов объекта Statement:

| Метод | Назначение | Возвращает |
|-------|-----------|------------|
| `executeQuery()` | SELECT-запросы | `ResultSet` |
| `executeUpdate()` | INSERT, UPDATE, DELETE, DDL | `int` (количество затронутых строк) |
| `execute()` | Любые SQL-команды | `boolean` (true если ResultSet, false если update count) |

ResultSet хранит результат запроса и содержит курсор, указывающий на текущую запись. Сразу после получения набора данных курсор находится перед первой записью — необходимо вызвать `next()` для перехода к первой строке.

Содержание полей текущей записи доступно через вызовы методов `getInt()`, `getFloat()`, `getString()`, `getDate()` и им подобных.

> **На собеседовании:** назовите три метода execute и объясните разницу. Частая ошибка — забыть, что `next()` нужно вызвать до первого чтения из ResultSet. Если спрашивают про `execute()`, объясните, что результат определяется через `getResultSet()` / `getUpdateCount()`.

[к оглавлению](#jdbc)

---

## Как вызвать хранимую процедуру
<!-- grade: middle -->

Хранимая процедура — это именованный набор SQL-операторов, хранящийся на сервере базы данных и вызываемый из Java через интерфейсы Statement.

Выбор интерфейса зависит от характеристик хранимой процедуры:

- без параметров — `Statement`
- с входными параметрами — `PreparedStatement`
- с входными и выходными параметрами — `CallableStatement`

Для получения информации о хранимой процедуре (имена и типы параметров) можно использовать методы `java.sql.DatabaseMetaData`.

<details>
<summary>Пример вызова хранимой процедуры с входными и выходными параметрами</summary>

```java
public void runStoredProcedure(final Connection connection) throws Exception {
    // описываем хранимую процедуру
    String procedure = "{ call procedureExample(?, ?, ?) }";

    // подготавливаем запрос
    CallableStatement cs = connection.prepareCall(procedure);

    // устанавливаем входные параметры
    cs.setString(1, "abcd");
    cs.setBoolean(2, true);
    cs.setInt(3, 10);

    // описываем выходные параметры
    cs.registerOutParameter(1, java.sql.Types.VARCHAR);
    cs.registerOutParameter(2, java.sql.Types.INTEGER);

    // запускаем выполнение хранимой процедуры
    cs.execute();

    // получаем результаты
    String parameter1 = cs.getString(1);
    int parameter2 = cs.getInt(2);

    // заканчиваем работу с запросом
    cs.close();
}
```

</details>

> **На собеседовании:** назовите CallableStatement и синтаксис вызова `{ call procedureName(?, ?) }`. Упомяните `registerOutParameter()` для выходных параметров. На практике хранимые процедуры в Java-проектах встречаются редко — чаще используются в legacy-системах или при интеграции с Oracle.

[к оглавлению](#jdbc)

---

## Как закрыть соединение с базой данных
<!-- grade: junior -->

Закрытие соединения с базой данных — это процесс освобождения ресурсов, связанных с объектом Connection, который необходимо выполнять после завершения работы.

Соединение закрывается вызовом метода `close()` у объекта `java.sql.Connection` или автоматически при использовании try-with-resources (начиная с Java 7):

```java
// Ручное закрытие
Connection conn = dataSource.getConnection();
try {
    // работа с БД
} finally {
    conn.close();
}

// Try-with-resources (рекомендуемый подход)
try (Connection conn = dataSource.getConnection();
     PreparedStatement ps = conn.prepareStatement(sql);
     ResultSet rs = ps.executeQuery()) {
    // работа с результатами
}
```

Предварительно необходимо закрыть все запросы (Statement, PreparedStatement), созданные этим соединением.

> **На собеседовании:** скажите, что правильный способ — try-with-resources. Обязательно упомяните порядок закрытия: ResultSet -> Statement -> Connection. При использовании пула соединений вызов `close()` не уничтожает соединение, а возвращает его в пул.

[к оглавлению](#jdbc)

---

## Что такое Connection Pool и зачем он нужен
<!-- grade: middle -->

Connection Pool (пул соединений) — это механизм управления набором заранее созданных соединений с базой данных, которые могут быть повторно использованы различными частями приложения.

> Аналогия из жизни: пул соединений — это как парк такси на стоянке. Вместо того чтобы каждый раз вызывать и ждать машину (создавать новое соединение ~50-100 мс), вы берёте свободную с ближайшей стоянки (~1 мс), используете и возвращаете обратно.

### Проблема без пула соединений

Создание нового соединения с базой данных — дорогостоящая операция (50-100 мс), включающая:
- установление TCP-соединения с сервером БД
- аутентификацию пользователя
- выделение ресурсов на стороне СУБД
- создание объекта Connection в JVM

Большинство СУБД имеют ограничение на максимальное количество одновременных соединений (PostgreSQL по умолчанию — 100).

### Принцип работы

Пул работает по принципу «заимствование -> использование -> возврат»:

1. При запуске приложения пул создаёт заданное количество соединений
2. Компонент запрашивает соединение у пула
3. Пул выдаёт свободное соединение
4. После завершения работы соединение возвращается в пул (а не закрывается)
5. Если свободных соединений нет — запрос ожидает или получает исключение по таймауту

### DataSource vs DriverManager

| Аспект | DriverManager | DataSource |
|--------|--------------|------------|
| Создание соединения | Новое каждый раз (~50-100 мс) | Из пула (~1 мс) |
| Повторное использование | Нет | Да |
| Пул соединений | Не поддерживает | Поддерживает |
| Применение | Обучение, прототипы | Production |

### Популярные реализации

| Пул | Описание |
|:----|:---------|
| HikariCP | Самый быстрый, стандарт в Spring Boot |
| Apache DBCP2 | Зрелый проект Apache |
| c3p0 | Устаревший, но всё ещё встречается |
| Tomcat JDBC Pool | Встроен в Apache Tomcat |

<details>
<summary>Пример использования с HikariCP</summary>

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class ConnectionPoolExample {

    private static HikariDataSource dataSource;

    public static void init() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
        config.setUsername("user");
        config.setPassword("password");
        config.setMaximumPoolSize(10);
        config.setMinimumIdle(5);
        config.setConnectionTimeout(30_000); // 30 секунд
        config.setIdleTimeout(600_000);      // 10 минут
        config.setMaxLifetime(1_800_000);    // 30 минут

        dataSource = new HikariDataSource(config);
    }

    public static void executeQuery() {
        // Соединение автоматически возвращается в пул при закрытии
        try (Connection conn = dataSource.getConnection();
             PreparedStatement ps = conn.prepareStatement(
                 "SELECT * FROM users WHERE id = ?")) {

            ps.setLong(1, 42L);
            try (ResultSet rs = ps.executeQuery()) {
                while (rs.next()) {
                    System.out.println(rs.getString("name"));
                }
            }
        } catch (SQLException e) {
            throw new RuntimeException("Ошибка при выполнении запроса", e);
        }
    }

    public static void shutdown() {
        if (dataSource != null) {
            dataSource.close(); // Закрывает все соединения в пуле
        }
    }
}
```

</details>

### Важное

- Всегда используйте DataSource вместо DriverManager в продуктивных приложениях
- Закрывайте соединение в finally или используйте try-with-resources — иначе соединение «утечёт» из пула
- Размер пула не должен быть слишком большим: для большинства приложений 10-20 соединений достаточно
- Формула оптимального размера пула: `connections = (core_count * 2) + effective_spindle_count`

### Частые ошибки

- Не закрывать Connection после использования — приводит к исчерпанию пула (connection leak)
- Устанавливать слишком большой maximumPoolSize — создаёт избыточную нагрузку на СУБД
- Не устанавливать connectionTimeout — потоки могут зависнуть навсегда в ожидании соединения
- Использовать DriverManager напрямую в веб-приложениях — нет переиспользования соединений

### Как используется в 2026

- HikariCP является стандартным пулом соединений в Spring Boot 3.x
- В Kubernetes-средах размер пула настраивается через переменные окружения
- Для реактивных приложений используется R2DBC с собственным пулом r2dbc-pool
- Виртуальные потоки (Java 21+) позволяют эффективнее использовать пулы с большим количеством ожидающих потоков
- Мониторинг пулов через Micrometer + Grafana стал стандартной практикой

> **На собеседовании:** объясните принцип «заимствование -> использование -> возврат». Назовите HikariCP как стандарт Spring Boot. Ключевая мысль: пул экономит время на создании соединений и ограничивает нагрузку на СУБД. Частый follow-up: как определить оптимальный размер пула.

[к оглавлению](#jdbc)

---

## Что такое HikariCP и как его настроить
<!-- grade: middle -->

HikariCP (от японского «свет») — это высокопроизводительная реализация пула JDBC-соединений, являющаяся стандартом в Spring Boot начиная с версии 2.0.

### Почему HikariCP

- Высокая производительность — один из самых быстрых пулов за счёт оптимизаций на уровне байткода
- Малый размер — библиотека около 130 КБ
- Надёжность — строгое следование спецификации JDBC
- Стандарт Spring Boot — используется по умолчанию с версии 2.0

### Ключевые параметры конфигурации

| Параметр | По умолчанию | Описание |
|:---------|:-------------|:---------|
| `maximumPoolSize` | 10 | Максимальное количество соединений в пуле |
| `minimumIdle` | = maximumPoolSize | Минимальное количество неактивных соединений |
| `connectionTimeout` | 30 000 мс | Максимальное время ожидания соединения из пула |
| `idleTimeout` | 600 000 мс | Максимальное время простоя соединения |
| `maxLifetime` | 1 800 000 мс | Максимальное время жизни соединения |
| `poolName` | auto-generated | Имя пула (для мониторинга) |
| `leakDetectionThreshold` | 0 (выкл.) | Время в мс, после которого соединение считается «утёкшим» |

### Настройка в Spring Boot (application.yml)

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: app_user
    password: secret
    hikari:
      maximum-pool-size: 15
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
      pool-name: MyApp-Pool
      leak-detection-threshold: 60000
```

<details>
<summary>Программная настройка HikariCP</summary>

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class HikariCPConfig {

    public static DataSource createDataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
        config.setUsername("app_user");
        config.setPassword("secret");
        config.setDriverClassName("org.postgresql.Driver");

        config.setMaximumPoolSize(15);
        config.setMinimumIdle(5);
        config.setConnectionTimeout(30_000);
        config.setIdleTimeout(600_000);
        config.setMaxLifetime(1_800_000);
        config.setPoolName("MyApp-Pool");
        config.setLeakDetectionThreshold(60_000);

        // Оптимизации для PostgreSQL
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");

        return new HikariDataSource(config);
    }
}
```

</details>

<details>
<summary>Рекомендации для продуктивной среды</summary>

```java
public class ProductionHikariConfig {

    public static DataSource createProductionDataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://db-host:5432/production");
        config.setUsername(System.getenv("DB_USERNAME"));
        config.setPassword(System.getenv("DB_PASSWORD"));

        // Размер пула: (CPU cores * 2) + effective_spindle_count
        config.setMaximumPoolSize(10);
        config.setMinimumIdle(10); // В продакшене minimumIdle = maximumPoolSize

        // maxLifetime должен быть меньше wait_timeout на стороне СУБД
        config.setMaxLifetime(1_740_000); // 29 минут
        config.setConnectionTimeout(10_000); // fail fast
        config.setPoolName("Production-Pool");
        config.setLeakDetectionThreshold(30_000);
        config.setRegisterMbeans(true); // JMX мониторинг

        return new HikariDataSource(config);
    }
}
```

</details>

### Важное

- В продуктивной среде minimumIdle рекомендуется устанавливать равным maximumPoolSize
- maxLifetime должен быть на несколько секунд меньше, чем таймаут на стороне СУБД
- poolName критически важен для мониторинга — особенно при нескольких источниках данных
- Включите leakDetectionThreshold в dev/stage-средах для обнаружения утечек

### Частые ошибки

- Устанавливать maximumPoolSize = 50-100 — перегружает СУБД; обычно 10-20 достаточно
- Забывать настроить maxLifetime — соединения могут быть разорваны со стороны СУБД
- Не закрывать Connection (отсутствие try-with-resources) — утечка из пула
- Не включать мониторинг — невозможно диагностировать проблемы с пулом в продакшене

### Как используется в 2026

- HikariCP 6.x — актуальная версия с поддержкой Java 21+ и виртуальных потоков
- В Spring Boot 3.x настраивается через `spring.datasource.hikari.*`
- Для cloud-native приложений параметры пула определяются через ConfigMap/Secrets в Kubernetes
- Интеграция с OpenTelemetry для распределённого трейсинга запросов к БД

> **На собеседовании:** назовите 3-4 ключевых параметра (maximumPoolSize, connectionTimeout, maxLifetime, leakDetectionThreshold) и их типичные значения. Покажите, что знаете формулу размера пула. Частый вопрос: почему minimumIdle = maximumPoolSize в продакшене — ответ: исключает задержки на создание новых соединений при пиковой нагрузке.

[к оглавлению](#jdbc)

---

## Как выполнять batch-операции в JDBC
<!-- grade: middle -->

Batch-операции (пакетные операции) — это механизм отправки нескольких SQL-команд на сервер за один сетевой вызов, значительно сокращающий количество обращений к СУБД и повышающий производительность.

Вместо отправки каждого SQL-запроса отдельно, batch-операции группируют запросы и отправляют их одним пакетом:

- `addBatch()` — добавляет SQL-команду в пакет
- `executeBatch()` — отправляет все накопленные команды на сервер
- `clearBatch()` — очищает накопленный пакет

### Сравнение производительности (вставка 10 000 записей)

| Подход | Время | Сетевых обращений |
|:-------|:------|:-----------------|
| Одиночный INSERT | ~10 000 мс | 10 000 |
| Batch (размер 1 000) | ~500 мс | 10 |
| Batch + rewriteBatchedStatements | ~200 мс | 10 |

<details>
<summary>Batch с Statement</summary>

```java
public void batchWithStatement(Connection connection) throws SQLException {
    try (Statement stmt = connection.createStatement()) {
        connection.setAutoCommit(false);

        stmt.addBatch("INSERT INTO products (name, price) VALUES ('Телефон', 29999)");
        stmt.addBatch("INSERT INTO products (name, price) VALUES ('Ноутбук', 74999)");
        stmt.addBatch("INSERT INTO products (name, price) VALUES ('Планшет', 19999)");
        stmt.addBatch("UPDATE products SET price = price * 0.9 WHERE price > 50000");

        int[] results = stmt.executeBatch();
        connection.commit();

        for (int i = 0; i < results.length; i++) {
            System.out.println("Команда " + i + ": затронуто строк = " + results[i]);
        }
    } catch (BatchUpdateException e) {
        connection.rollback();
        int[] updateCounts = e.getUpdateCounts();
        for (int i = 0; i < updateCounts.length; i++) {
            if (updateCounts[i] == Statement.EXECUTE_FAILED) {
                System.err.println("Команда " + i + " завершилась с ошибкой");
            }
        }
    }
}
```

</details>

<details>
<summary>Batch с PreparedStatement (рекомендуемый подход)</summary>

```java
public void batchInsertUsers(Connection connection, List<User> users)
        throws SQLException {
    String sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";

    try (PreparedStatement ps = connection.prepareStatement(sql)) {
        connection.setAutoCommit(false);
        int batchSize = 1000;
        int count = 0;

        for (User user : users) {
            ps.setString(1, user.getName());
            ps.setString(2, user.getEmail());
            ps.setInt(3, user.getAge());
            ps.addBatch();
            count++;

            // Выполняем промежуточный batch каждые 1000 записей
            if (count % batchSize == 0) {
                ps.executeBatch();
                ps.clearBatch();
            }
        }

        // Выполняем оставшиеся записи
        ps.executeBatch();
        connection.commit();
    } catch (BatchUpdateException e) {
        connection.rollback();
        throw new RuntimeException("Ошибка batch-вставки", e);
    }
}
```

</details>

<details>
<summary>Batch-операции с Spring JdbcTemplate</summary>

```java
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.BatchPreparedStatementSetter;

public class UserRepository {

    private final JdbcTemplate jdbcTemplate;

    public UserRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void batchInsert(List<User> users) {
        String sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";

        jdbcTemplate.batchUpdate(sql, new BatchPreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement ps, int i)
                    throws SQLException {
                User user = users.get(i);
                ps.setString(1, user.getName());
                ps.setString(2, user.getEmail());
                ps.setInt(3, user.getAge());
            }

            @Override
            public int getBatchSize() {
                return users.size();
            }
        });
    }
}
```

</details>

### Важное

- Batch-операции эффективны при вставке/обновлении от 100+ записей
- Всегда оборачивайте batch в транзакцию (setAutoCommit(false) + commit())
- Для больших объёмов разбивайте batch на части (обычно по 1000-5000 записей)
- PreparedStatement batch безопаснее — защита от SQL Injection

### Частые ошибки

- Не отключать autoCommit — каждая команда коммитится отдельно, теряя смысл пакетирования
- Накапливать слишком большой batch без промежуточного executeBatch() — может привести к OutOfMemoryError
- Не обрабатывать BatchUpdateException — невозможно определить, какие записи вставлены успешно
- Забывать clearBatch() после executeBatch() при обработке данных частями

### Как используется в 2026

- Spring Data JDBC поддерживает batch-вставки через saveAll() с настройкой spring.jdbc.template.batch-size
- jOOQ предлагает типобезопасные batch-операции с автоматической оптимизацией
- Для массовой загрузки всё чаще используется COPY (PostgreSQL) или LOAD DATA INFILE (MySQL)
- В реактивном стеке R2DBC поддерживает batch через Statement.add()

> **На собеседовании:** объясните зачем нужен batch (сокращение сетевых вызовов), назовите три метода (addBatch, executeBatch, clearBatch) и скажите про разбиение на части при большом объёме данных. Частый follow-up: что вернёт executeBatch() — массив int с количеством затронутых строк для каждой команды.

[к оглавлению](#jdbc)

---

## Как управлять транзакциями в JDBC
<!-- grade: middle -->

Транзакция в JDBC — это набор операций с базой данных, выполняемых как единое целое: либо все операции завершаются успешно (commit), либо все отменяются (rollback).

По умолчанию JDBC работает в режиме autoCommit = true, то есть каждый SQL-запрос автоматически коммитится. Для ручного управления транзакциями необходимо отключить автокоммит.

### Базовое управление

```java
connection.setAutoCommit(false); // начинаем транзакцию
try {
    // выполняем SQL-операции
    connection.commit();         // подтверждаем
} catch (SQLException e) {
    connection.rollback();       // откатываем при ошибке
    throw e;
} finally {
    connection.setAutoCommit(true); // восстанавливаем режим
}
```

### Savepoint

Savepoint позволяет создать промежуточную точку внутри транзакции, к которой можно откатиться без отката всей транзакции:

```java
Savepoint sp = connection.setSavepoint("before_notification");
try {
    // дополнительная операция
} catch (SQLException e) {
    connection.rollback(sp); // откат только до savepoint
}
connection.commit(); // основная операция сохраняется
```

### Сравнение с Spring @Transactional

| Аспект | Чистый JDBC | Spring @Transactional |
|:-------|:------------|:----------------------|
| Управление соединением | Вручную | Автоматически |
| Начало транзакции | setAutoCommit(false) | Декларативно через аннотацию |
| Коммит | connection.commit() | Автоматически при успешном завершении |
| Откат | connection.rollback() | Автоматически при RuntimeException |
| Уровень изоляции | setTransactionIsolation() | @Transactional(isolation = ...) |
| Propagation | Нет поддержки | Полная поддержка (REQUIRED, REQUIRES_NEW и т.д.) |

<details>
<summary>Полный пример: перевод денег между счетами</summary>

```java
public void transferMoney(Connection connection, long fromAccountId,
                          long toAccountId, BigDecimal amount)
        throws SQLException {
    connection.setAutoCommit(false);

    try {
        // Списание со счёта отправителя
        try (PreparedStatement debit = connection.prepareStatement(
                "UPDATE accounts SET balance = balance - ? "
                + "WHERE id = ? AND balance >= ?")) {
            debit.setBigDecimal(1, amount);
            debit.setLong(2, fromAccountId);
            debit.setBigDecimal(3, amount);
            int updated = debit.executeUpdate();
            if (updated == 0) {
                throw new SQLException(
                    "Недостаточно средств на счёте " + fromAccountId);
            }
        }

        // Зачисление на счёт получателя
        try (PreparedStatement credit = connection.prepareStatement(
                "UPDATE accounts SET balance = balance + ? WHERE id = ?")) {
            credit.setBigDecimal(1, amount);
            credit.setLong(2, toAccountId);
            credit.executeUpdate();
        }

        connection.commit();
    } catch (SQLException e) {
        connection.rollback();
        throw e;
    } finally {
        connection.setAutoCommit(true);
    }
}
```

</details>

<details>
<summary>Вспомогательный класс TransactionTemplate</summary>

```java
public class TransactionTemplate {

    private final DataSource dataSource;

    public TransactionTemplate(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @FunctionalInterface
    public interface TransactionCallback<T> {
        T execute(Connection connection) throws SQLException;
    }

    public <T> T executeInTransaction(TransactionCallback<T> callback) {
        try (Connection conn = dataSource.getConnection()) {
            conn.setAutoCommit(false);
            try {
                T result = callback.execute(conn);
                conn.commit();
                return result;
            } catch (Exception e) {
                conn.rollback();
                throw new RuntimeException("Ошибка в транзакции", e);
            }
        } catch (SQLException e) {
            throw new RuntimeException("Ошибка подключения", e);
        }
    }
}
```

</details>

### Важное

- Всегда отключайте autoCommit перед началом транзакции
- Используйте try-catch-finally для гарантированного rollback при ошибке
- Закрывайте Connection после завершения работы с транзакцией
- Уровень изоляции устанавливается через setTransactionIsolation() — выбирайте минимально необходимый

### Частые ошибки

- Забывать вызвать rollback() в блоке catch — данные могут остаться в неконсистентном состоянии
- Не восстанавливать autoCommit после транзакции — последующие операции не будут коммититься
- Держать транзакцию открытой слишком долго — блокирует записи в БД
- Не закрывать PreparedStatement внутри транзакции — утечка ресурсов

### Как используется в 2026

- В большинстве проектов транзакции управляются через Spring @Transactional
- Чистый JDBC для транзакций используется в микрофреймворках (Javalin, Spark) и библиотеках
- Saga-паттерн для распределённых транзакций между микросервисами вместо XA
- Virtual threads (Java 21+) упрощают написание транзакционного кода без callback-ов

> **На собеседовании:** покажите паттерн setAutoCommit(false) -> try { ... commit() } catch { rollback() }. Упомяните Savepoint для частичного отката. Обязательно скажите, что в реальных проектах используется Spring @Transactional, а чистый JDBC — для понимания того, что происходит «под капотом». Частый follow-up: что такое propagation и почему его нет в чистом JDBC.

[к оглавлению](#jdbc)

---

## Какие есть альтернативы чистому JDBC
<!-- grade: middle -->

Альтернативы чистому JDBC — это набор фреймворков и библиотек, упрощающих работу с базами данных за счёт снижения количества шаблонного кода и повышения уровня абстракции.

### Сравнительная таблица

| Критерий | JDBC | JdbcTemplate | jOOQ | JPA/Hibernate | MyBatis | R2DBC |
|:---------|:-----|:-------------|:-----|:--------------|:--------|:------|
| Уровень абстракции | Низкий | Средний | Средний | Высокий | Средний | Низкий |
| Контроль SQL | Полный | Полный | Полный | Ограниченный | Полный | Полный |
| Шаблонный код | Много | Мало | Мало | Минимум | Мало | Мало |
| Типобезопасность SQL | Нет | Нет | Да | Нет (Criteria API — да) | Нет | Нет |
| Кеширование | Нет | Нет | Нет | Да (L1, L2) | Опционально | Нет |
| Реактивность | Нет | Нет | Нет | Нет | Нет | Да |

### Когда использовать каждый инструмент

- Чистый JDBC — в библиотеках без внешних зависимостей, для обучения
- JdbcTemplate — простые CRUD-операции, когда JPA избыточен
- jOOQ — сложные SQL-запросы, типобезопасность, аналитические системы
- JPA/Hibernate — богатая доменная модель со связями, стандартные CRUD
- MyBatis — сложный SQL, legacy-БД, полный контроль над запросами
- R2DBC — реактивные приложения, неблокирующий I/O

<details>
<summary>Пример: Spring JdbcTemplate</summary>

```java
public class UserRepository {

    private final JdbcTemplate jdbcTemplate;

    public UserRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public List<User> findByAge(int minAge) {
        return jdbcTemplate.query(
            "SELECT id, name, email, age FROM users WHERE age >= ?",
            (rs, rowNum) -> new User(
                rs.getLong("id"),
                rs.getString("name"),
                rs.getString("email"),
                rs.getInt("age")
            ),
            minAge
        );
    }

    public int updateEmail(Long userId, String newEmail) {
        return jdbcTemplate.update(
            "UPDATE users SET email = ? WHERE id = ?",
            newEmail, userId
        );
    }
}
```

</details>

<details>
<summary>Пример: jOOQ</summary>

```java
public class UserRepository {

    private final DSLContext dsl;

    public UserRepository(DSLContext dsl) {
        this.dsl = dsl;
    }

    public List<User> findByAge(int minAge) {
        return dsl.selectFrom(USERS)
            .where(USERS.AGE.ge(minAge))
            .orderBy(USERS.NAME)
            .fetchInto(User.class);
    }

    public void create(User user) {
        dsl.insertInto(USERS)
            .set(USERS.NAME, user.getName())
            .set(USERS.EMAIL, user.getEmail())
            .set(USERS.AGE, user.getAge())
            .execute();
    }
}
```

</details>

<details>
<summary>Пример: JPA / Hibernate</summary>

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private int age;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL,
               fetch = FetchType.LAZY)
    private List<Order> orders;
}

public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByAgeGreaterThanEqual(int minAge);

    @Query("SELECT u FROM User u WHERE u.email LIKE %:domain")
    List<User> findByEmailDomain(@Param("domain") String domain);
}
```

</details>

<details>
<summary>Пример: MyBatis</summary>

```java
@Mapper
public interface UserMapper {

    @Select("SELECT * FROM users WHERE age >= #{minAge}")
    List<User> findByAge(@Param("minAge") int minAge);

    @Insert("INSERT INTO users (name, email, age) "
            + "VALUES (#{name}, #{email}, #{age})")
    @Options(useGeneratedKeys = true, keyProperty = "id")
    int insert(User user);
}
```

</details>

<details>
<summary>Пример: R2DBC</summary>

```java
public interface UserRepository extends R2dbcRepository<User, Long> {

    Flux<User> findByAgeGreaterThanEqual(int minAge);

    @Query("SELECT * FROM users WHERE email LIKE :domain")
    Flux<User> findByEmailDomain(String domain);
}
```

</details>

### Важное

- Выбор инструмента зависит от задачи — нет универсально лучшего решения
- В одном проекте можно комбинировать подходы (JPA для CRUD + jOOQ для отчётов)
- Знание JDBC необходимо в любом случае — все инструменты (кроме R2DBC) работают поверх JDBC
- JdbcTemplate — оптимальный баланс между простотой и контролем для большинства задач

### Частые ошибки

- Использовать JPA для сложных аналитических запросов — генерируемый SQL может быть неоптимальным
- Игнорировать N+1 проблему в JPA/Hibernate — деградация производительности
- Выбирать инструмент «потому что модно», а не по требованиям проекта
- Смешивать JPA и чистый JDBC в одной транзакции без понимания кеша Hibernate
- Использовать R2DBC без реальной потребности в реактивности — добавляет сложность без выигрыша

### Как используется в 2026

- Spring Data JDBC набирает популярность как простая альтернатива JPA для DDD
- jOOQ активно развивается: поддержка JSON-типов, оконных функций, CTE и виртуальных потоков
- JPA 3.2 (Jakarta Persistence) — актуальная спецификация с поддержкой Java Records
- Тренд на multi-model access: использование нескольких инструментов в одном проекте

> **На собеседовании:** назовите 4-5 альтернатив и скажите, когда каждая уместна. Ключевое: все они (кроме R2DBC) работают поверх JDBC. Покажите, что понимаете trade-off между уровнем абстракции и контролем над SQL. Частый follow-up: почему в вашем проекте выбрали именно этот инструмент.

[к оглавлению](#jdbc)

# Источники
+ [Википедия - JDBC](https://ru.wikipedia.org/wiki/Java_Database_Connectivity)
+ [IBM developerWorks](http://www.ibm.com/developerworks/ru/library/dm-1209storedprocedures/)
+ [Документация к пакету java.sql](https://docs.oracle.com/javase/7/docs/api/java/sql/package-summary.html)
+ [Википедия - Уровень изолированности транзакции](https://ru.wikipedia.org/wiki/Уровень_изолированности_транзакций)

[Вопросы для собеседования](README.md)
