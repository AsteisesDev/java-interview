[Вопросы для собеседования](README.md)

# JDBC
+ [Что такое _JDBC_?](#Что-такое-jdbc)
+ [В чем заключаются преимущества использования JDBC?](#В-чем-заключаются-преимущества-использования-jdbc)
+ [Что из себя представляет JDBC URL?](#Что-из-себя-представляет-jdbc-url)
+ [Из каких частей стоит JDBC?](#Из-каких-частей-стоит-jdbc)
+ [Перечислите-основные-классы-и-интерфейсы-jdbc](#Перечислите-основные-классы-и-интерфейсы-jdbc)
+ [Опишите основные этапы работы с базой данных с использованием JDBC.](#Опишите-основные-этапы-работы-с-базой-данных-при-использовании-jdbc)
+ [Перечислите основные типы данных используемые в JDBC. Как они связаны с типами Java?](#Перечислите-основные-типы-данных-используемые-в-JDBC.-Как-они-связаны-с-типами-Java)
+ [Как зарегистрировать драйвер JDBC?](#Как-зарегистрировать-драйвер-jdbc)
+ [Как установить соединение с базой данных?](#Как-установить-соединение-с-базой-данных)
+ [Какие уровни изоляции транзакций поддерживаются в JDBC?](#Какие-уровни-изоляции-транзакций-поддерживаются-в-jdbc)
+ [При помощи чего формируются запросы к базе данных?](#При-помощи-чего-формируются-запросы-к-базе-данных)
+ [Чем отличается Statement от PreparedStatement?](#Чем-отличается-statement-от-preparedstatement)
+ [Как осуществляется запрос к базе данных и обработка результатов?](#Как-осуществляется-запрос-к-базе-данных-и-обработка-результатов)
+ [Как вызвать хранимую процедуру?](#Как-вызвать-хранимую-процедуру)
+ [Как закрыть соединение с базой данных?](#Как-закрыть-соединение-с-базой-данных)
+ [Что такое Connection Pool и зачем он нужен?](#Что-такое-connection-pool-и-зачем-он-нужен)
+ [Что такое HikariCP и как его настроить?](#Что-такое-hikaricp-и-как-его-настроить)
+ [Как выполнять batch-операции в JDBC?](#Как-выполнять-batch-операции-в-jdbc)
+ [Как управлять транзакциями в JDBC?](#Как-управлять-транзакциями-в-jdbc)
+ [Какие есть альтернативы чистому JDBC?](#Какие-есть-альтернативы-чистому-jdbc)

## Что такое _JDBC_?
__JDBC, Java DataBase Connectivity (соединение с базами данных на Java)__ — промышленный стандарт взаимодействия Java-приложений с различными СУБД. Реализован в виде пакета `java.sql`, входящего в состав Java SE.

JDBC основан на концепции драйверов, которые позволяют получать соединение с базой данных по специально описанному URL. При загрузке драйвер регистрирует себя в системе и в дальнейшем автоматически вызывается, когда программа требует URL, содержащий протокол, за который этот драйвер отвечает.

[к оглавлению](#jdbc)

## В чем заключаются преимущества использования JDBC?
Преимуществами JDBC считают:

+ Лёгкость разработки: разработчик может не знать специфики базы данных, с которой работает;
+ Код практически не меняется, если компания переходит на другую базу данных (количество изменений зависит исключительно от различий между диалектами SQL);
+ Не нужно дополнительно устанавливать клиентскую программу;
+ К любой базе данных можно подсоединиться через легко описываемый URL.

[к оглавлению](#jdbc)

## Что из себя представляет JDBC URL?
__JDBC URL__ состоит из:

+ `<protocol>:` (протокола) - всегда `jdbc:`.
+ `<subprotocol>:` (подпротокола) - это имя драйвера или имя механизма соединения с базой данных. Подпротокол может поддерживаться одним или несколькими драйверами. Лежащий на поверхности пример подпротокола - это "odbc", отведенный для URL, обозначающих имя источника данных ODBC. В случае необходимости использовать сервис имен (т.е. имя базы данных в JDBC URL не будет действительным именем базы данных), то подпротоколом может выступать сервис имен.
+ `<subname>` (подимени) - это идентификатор базы данных. Значение подимени может менятся в зависимости от подпротокола, и может также иметь под-подимя с синтаксисом, определяемым разработчиком драйвера. Назначение подимени - это предоставление всей информации, необходимой для поиска базы данных. Например, если база данных находится в Интернет, то в состав подимени JDBC URL должен быть включен сетевой адрес, подчиняющийся следующим соглашениям: `//<hostname>:<port>/<subsubname`.

Пример JDBC URL для подключения к MySQL базе данных «Test» расположенной по адресу localhost и ожидающей соединений по порту 3306: `jdbc:mysql://localhost:3306/Test`

[к оглавлению](#jdbc)

## Из каких частей стоит JDBC?
JDBC состоит из двух частей:

+ __JDBC API__, который содержит набор классов и интерфейсов, определяющих доступ к базам данных. Эти классы и методы объявлены в двух пакетах - `java.sql` и `javax.sql`;
+ __JDBC-драйвер__, компонент, специфичный для каждой базы данных. 

JDBC превращает вызовы уровня API в «родные» команды того или иного сервера баз данных.

[к оглавлению](#jdbc)

## Перечислите основные классы и интерфейсы JDBC.
+ `java.sql.DriverManager` - позволяет загрузить и зарегистрировать необходимый JDBC-драйвер, а затем получить соединение с базой данных.

+ `javax.sql.DataSource` - решает те же задачи, что и _DriverManager_, но более удобным и универсальным образом. Существуют также `javax.sql.ConnectionPoolDataSource` и `javax.sq1.XADataSource` задача которых - обеспечение поддержки пула соединений.

+ `java.sql.Connection`  - обеспечивает формирование запросов к источнику данных и управление транзакциями. Также предусмотрены интерфейсы `javax.sql.PooledConnection` и `javax.sql.XAConnection`.

+ `java.sql.Statement` , `java.sql.PreparedStatement` и `java.sql.CallableStatement`  - эти интерфейсы позволяют отправить запрос к источнику данных.

+ `java.sql.ResultSet`  - объявляет методы, которые позволяют перемещаться по набору данных и считывать значения отдельных полей в текущей записи.

+ `java.sql.ResultSetMetaData`  - позволяет получить информацию о структуре набора данных.

+ `java.sql.DatabaseMetaData` - позволяет получить информацию о структуре источника данных.

[к оглавлению](#jdbc)

## Перечислите основные типы данных используемые в JDBC. Как они связаны с типами Java?

| JDBC Type | Java Object Type |
|---------------:|---------------------------|
| __CHAR__ | `String` |
| __VARCHAR__ | `String` |
| __LONGVARCHAR__ | `String` |
| __NUMERIC__ | `java.math.BigDecimal` |
| __DECIMAL__ | `java.math.BigDecimal` |
| __BIT__ | `Boolean` |
| __TINYINT__ | `Integer` |
| __SMALLINT__ | `Integer` |
| __INTEGER__ | `Integer` |
| __BIGINT__ | `Long` |
| __REAL__ | `Float` |
| __FLOAT__ | `Double` |
| __DOUBLE__ | `Double` |
| __BINARY__ | `byte[]` |
| __VARBINARY__ | `byte[]` |
| __LONGVARBINARY__ | `byte[]` |
| __DATE__ | `java.sql.Date` |
| __TIME__ | `java.sql.Time` |
| __TIMESTAMP__ | `java.sql.Timestamp` |
| __CLOB__ | `Clob` |
| __BLOB__ | `Blob` |
| __ARRAY__ | `Array` |
| __STRUCT__ | `Struct`|
| __REF__ | `Ref` |
| __DISTINCT__ | сопоставление базового типа |
| __JAVA_OBJECT__ | базовый класс Java |

[к оглавлению](#jdbc)

## Опишите основные этапы работы с базой данных при использовании JDBC.
+ Регистрация драйверов;
+ Установление соединения с базой данных;
+ Создание запроса(ов) к базе данных;
+ Выполнение запроса(ов) к базе данных;
+ Обработка результата(ов);
+ Закрытие соединения с базой данных.

[к оглавлению](#jdbc)

## Как зарегистрировать драйвер JDBC?
Регистрацию драйвера можно осуществить несколькими способами:

+ `java.sql.DriverManager.registerDriver(%объект класса драйвера%)`.  

+ `Class.forName(«полное имя класса драйвера»).newInstance()`.

+ `Class.forName(«полное имя класса драйвера»)`;

[к оглавлению](#jdbc)

## Как установить соединение с базой данных?
Для установки соединения с базой данных используется статический вызов `java.sql.DriverManager.getConnection(...)` .


В качестве параметра может передаваться:

+ URL базы данных
```java
static Connection getConnection(String url)
```

+ URL базы данных и набор свойств для инициализации
```java
static Connection getConnection(String url, Properties info)
```

+ URL базы данных, имя пользователя и пароль
```java
static Connection getConnection(String url, String user, String password)
```
 
В результате вызова будет установлено соединение с базой данных и создан объект класса `java.sql.Connection` - своеобразная «сессия», внутри контекста которой и будет происходить дальнейшая работа с базой данных.

[к оглавлению](#jdbc)

## Какие уровни изоляции транзакций поддерживаются в JDBC?
__Уровень изолированности транзакций__ — значение, определяющее уровень, при котором в транзакции допускаются несогласованные данные, то есть степень изолированности одной транзакции от другой. Более высокий уровень изолированности повышает точность данных, но при этом может снижаться количество параллельно выполняемых транзакций. С другой стороны, более низкий уровень изолированности позволяет выполнять больше параллельных транзакций, но снижает точность данных.

Во время использования транзакций, для обеспечения целостности данных, СУБД использует блокировки, чтобы заблокировать доступ других обращений к данным, участвующим в транзакции. Такие блокировки необходимы, чтобы предотвратить:

+ _«грязное» чтение (dirty read)_ — чтение данных, добавленных или изменённых транзакцией, которая впоследствии не подтвердится (откатится);

+ _неповторяющееся чтение (non-repeatable read)_ — при повторном чтении в рамках одной транзакции ранее прочитанные данные оказываются изменёнными;

+ _фантомное чтение (phantom reads)_ — ситуация, когда при повторном чтении в рамках одной транзакции одна и та же выборка дает разные множества строк.

Уровни изоляции транзакций определены в виде констант интерфейса `java.sql.Connection`:

+ `TRANSACTION_NONE` – драйвер не поддерживает транзакции;

+ `TRANSACTION_READ_UNCOMMITTED` – позволяет транзакциям видеть несохраненные изменения данных: разрешает грязное, непроверяющееся и фантомное чтения;

+ `TRANSACTION_READ_COMMITTED` – любое изменение, сделанное в транзакции, не видно вне неё, пока она не сохранена: предотвращает грязное чтение, но разрешает непроверяющееся и фантомное;

+ `TRANSACTION_REPEATABLE_READ` – запрещает грязное и непроверяющееся, фантомное чтение разрешено;

+ `TRANSACTION_SERIALIZABLE` – грязное, непроверяющееся и фантомное чтения запрещены.

> __NB!__ Сервер базы данных может не поддерживать все уровни изоляции. Интерфейс `java.sql.DatabaseMetaData` предоставляет информацию об уровнях изолированности транзакций, которые поддерживаются данной СУБД.

Уровень изоляции транзакции используемый СУБД можно задать с помощью метода `setTransactionIsolation()` объекта `java.sql.Connection`. Получить информацию о применяемом уровне изоляции поможет метод `getTransactionIsolation()`.

[к оглавлению](#jdbc)

## При помощи чего формируются запросы к базе данных?

Для выполнения запросов к базе данных в Java используются три интерфейса:

+ `java.sql.Statement` - для операторов SQL без параметров;
+ `java.sql.PreparedStatement` - для операторов SQL с параметрами и часто выполняемых операторов;
+ `java.sql.CallableStatement` -  для исполнения хранимых в базе процедур.

Объекты-носители интерфейсов создаются при помощи методов объекта `java.sql.Connection`:

+ `java.sql.Connection.createStatement()` возвращает объект _Statement_;
+ `java.sql.Connection.prepareStatement()` возвращает объект _PreparedStatement_;
+ `java.sql.Connection.prepareCall()` возвращает объект _CallableStatement_;

[к оглавлению](#jdbc)

## Чем отличается Statement от PreparedStatement?
+ __Statement__: используется для простых случаев запроса без параметров.
+ __PreparedStatement__: предварительно компилирует запрос, который может содержать входные параметры и выполняться несколько раз с разным набором этих параметров.

Перед выполнением СУБД разбирает каждый запрос, оптимизирует его и создает «план» (query plan) его выполнения. Если один и тот же запрос выполняется несколько раз, то СУБД в состоянии кэшировать план его выполнения и не производить этапов разборки и оптимизации повторно. Благодаря этому запрос выполняется быстрее.

Суммируя: _PreparedStatement_ выгодно отличается от _Statement_ тем, что при повторном использовании с одним или несколькими наборами параметров позволяет получить преимущества заранее прекомпилированного и кэшированного запроса, помогая при этом избежать SQL Injection.

[к оглавлению](#jdbc)

## Как осуществляется запрос к базе данных и обработка результатов?
Выполнение запросов осуществляется при помощи вызова методов объекта, реализующего интерфейс `java.sql.Statement`:

+ __`executeQuery()`__ -  для запросов, результатом которых является один набор значений, например запросов `SELECT`. Результатом выполнения является объект класса `java.sql.ResultSet`;
 
+ __`executeUpdate()`__ - для выполнения операторов `INSERT`, `UPDATE` или `DELETE`, а также для операторов _DDL (Data Definition Language)_. Метод возвращает целое число, показывающее, сколько записей было модифицировано;

+ __`execute()`__ – исполняет SQL-команды, которые могут возвращать различные результаты. Например, может использоваться для операции `CREATE TABLE`. Возвращает `true`, если первый результат содержит _ResultSet_ и `false`, если первый результат - это количество модифицированных записей или результат отсутствует. Чтобы получить первый результат необходимо вызвать метод `getResultSet()` или `getUpdateCount()`. Остальные результаты доступны через вызов `getMoreResults()`, который при необходимости может быть произведён многократно.

Объект с интерфейсом `java.sql.ResultSet` хранит в себе результат запроса к базе данных - некий набор данных, внутри которого есть курсор, указывающий на один из элементов набора данных - текущую запись.

Используя курсор можно перемещаться по набору данных при помощи метода `next()`.

> __NB!__ Сразу после получения набора данных его курсор находится перед первой записью и чтобы сделать её текущей необходимо вызвать метод `next()`.

Содержание полей текущей записи доступно через вызовы методов `getInt()`, `getFloat()`, `getString()`, `getDate()` и им подобных.

[к оглавлению](#jdbc)

## Как вызвать хранимую процедуру?
__Хранимые процедуры__ – это именованный набор операторов SQL хранящийся на сервере. Такую процедуру можно вызвать из Java-класса с помощью вызова методов объекта реализующего интерфейс `java.sql.Statement`.

Выбор объекта зависит от характеристик хранимой процедуры:

+ без параметров → `Statement`
+ с входными параметрами → `PreparedStatement`
+ с входными и выходными параметрами → `CallableStatement`

> Если неизвестно, как была определена хранимая процедура, для получения информации о хранимой процедуре (например, имен и типов параметров) можно использовать методы `java.sql.DatabaseMetaData` позволяющие получить информацию о структуре источника данных.

Пример вызова хранимой процедуры с входными и выходными параметрами:

```java
public vois runStoredProcedure(final Connection connection) throws Exception {
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

[к оглавлению](#jdbc)

## Как закрыть соединение с базой данных?
Соединение с базой данной закрывается вызовом метода `close()` у соответствующего объекта `java.sql.Connection` или посредством использования механизма try-with-resources при создании такого объекта, появившегося в Java 7.

> __NB!__ Предварительно необходимо закрыть все запросы созданные этим соединением.

[к оглавлению](#jdbc)

## Что такое Connection Pool и зачем он нужен?

__Connection Pool (пул соединений)__ — это механизм управления набором заранее созданных соединений с базой данных, которые могут быть повторно использованы различными частями приложения.

### Проблема без пула соединений

Создание нового соединения с базой данных — дорогостоящая операция, занимающая от 50 до 100 мс. Каждое соединение требует:
+ Установление TCP-соединения с сервером БД;
+ Аутентификацию пользователя;
+ Выделение ресурсов на стороне СУБД;
+ Создание объекта `Connection` в JVM.

Кроме того, большинство СУБД имеют ограничение на максимальное количество одновременных соединений (например, PostgreSQL по умолчанию — 100).

### Принцип работы пула

Пул соединений работает по принципу «заимствование → использование → возврат»:

1. При запуске приложения пул создаёт заданное количество соединений;
2. Когда компоненту нужно соединение, он запрашивает его у пула;
3. Пул выдаёт свободное соединение;
4. После завершения работы соединение возвращается в пул (а не закрывается);
5. Если свободных соединений нет, запрос ожидает или получает исключение по таймауту.

### DataSource vs DriverManager

`DriverManager` создаёт новое соединение при каждом вызове `getConnection()`, тогда как `DataSource` может быть реализован с поддержкой пула соединений:

```java
// Без пула — каждый раз новое соединение (медленно)
Connection conn = DriverManager.getConnection(url, user, password);

// С пулом — получаем соединение из пула (быстро)
DataSource dataSource = createPooledDataSource();
Connection conn = dataSource.getConnection(); // ~1 мс вместо ~50-100 мс
```

### Пример с HikariCP

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
             PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?")) {

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

### Популярные реализации пулов соединений

| Пул | Описание |
|:---|:---|
| **HikariCP** | Самый быстрый, стандарт в Spring Boot |
| **Apache DBCP2** | Зрелый проект Apache |
| **c3p0** | Устаревший, но всё ещё встречается |
| **Tomcat JDBC Pool** | Встроен в Apache Tomcat |

### Важное
+ Всегда используйте `DataSource` вместо `DriverManager` в продуктивных приложениях;
+ Закрывайте соединение в `finally` или используйте try-with-resources — иначе соединение «утечёт» из пула;
+ Размер пула не должен быть слишком большим: для большинства приложений 10-20 соединений достаточно;
+ Формула оптимального размера пула: `connections = (core_count * 2) + effective_spindle_count`.

### Частые ошибки
+ Не закрывать `Connection` после использования — приводит к исчерпанию пула (connection leak);
+ Устанавливать слишком большой `maximumPoolSize` — создаёт избыточную нагрузку на СУБД;
+ Не устанавливать `connectionTimeout` — потоки могут зависнуть навсегда в ожидании соединения;
+ Использовать `DriverManager` напрямую в веб-приложениях — нет переиспользования соединений.

### Как используется в 2026
+ HikariCP является стандартным пулом соединений в Spring Boot 3.x;
+ В Kubernetes-средах размер пула часто настраивается через переменные окружения;
+ Для реактивных приложений используется R2DBC с собственным пулом `r2dbc-pool`;
+ Виртуальные потоки (Project Loom, Java 21+) позволяют эффективнее использовать пулы с большим количеством ожидающих потоков;
+ Мониторинг пулов через Micrometer + Grafana стал стандартной практикой.

[к оглавлению](#jdbc)

## Что такое HikariCP и как его настроить?

__HikariCP__ (от японского 光 — «свет») — это высокопроизводительная реализация пула JDBC-соединений. С версии Spring Boot 2.0 является пулом соединений по умолчанию.

### Почему HikariCP?

+ **Высокая производительность** — один из самых быстрых пулов соединений за счёт оптимизаций на уровне байткода;
+ **Малый размер** — библиотека занимает около 130 КБ;
+ **Надёжность** — строгое следование спецификации JDBC;
+ **Стандарт Spring Boot** — используется по умолчанию с версии 2.0.

### Ключевые параметры конфигурации

| Параметр | По умолчанию | Описание |
|:---|:---|:---|
| `maximumPoolSize` | 10 | Максимальное количество соединений в пуле (активных + ожидающих) |
| `minimumIdle` | = maximumPoolSize | Минимальное количество неактивных соединений в пуле |
| `connectionTimeout` | 30 000 мс | Максимальное время ожидания соединения из пула |
| `idleTimeout` | 600 000 мс | Максимальное время простоя соединения перед удалением |
| `maxLifetime` | 1 800 000 мс | Максимальное время жизни соединения в пуле |
| `poolName` | auto-generated | Имя пула (полезно для мониторинга и логирования) |
| `connectionTestQuery` | нет | SQL-запрос для проверки соединения (для драйверов без поддержки JDBC4) |
| `leakDetectionThreshold` | 0 (выкл.) | Время в мс, после которого соединение считается «утёкшим» |

### Программная настройка

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import javax.sql.DataSource;

public class HikariCPConfig {

    public static DataSource createDataSource() {
        HikariConfig config = new HikariConfig();

        // Основные параметры подключения
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
        config.setUsername("app_user");
        config.setPassword("secret");
        config.setDriverClassName("org.postgresql.Driver");

        // Параметры пула
        config.setMaximumPoolSize(15);
        config.setMinimumIdle(5);
        config.setConnectionTimeout(30_000);   // 30 секунд
        config.setIdleTimeout(600_000);        // 10 минут
        config.setMaxLifetime(1_800_000);      // 30 минут
        config.setPoolName("MyApp-Pool");

        // Обнаружение утечек соединений
        config.setLeakDetectionThreshold(60_000); // 60 секунд

        // Оптимизации для PostgreSQL
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");

        return new HikariDataSource(config);
    }
}
```

### Настройка в Spring Boot (application.yml)

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: app_user
    password: secret
    driver-class-name: org.postgresql.Driver
    hikari:
      maximum-pool-size: 15
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
      pool-name: MyApp-Pool
      leak-detection-threshold: 60000
      data-source-properties:
        cachePrepStmts: true
        prepStmtCacheSize: 250
        prepStmtCacheSqlLimit: 2048
```

### Рекомендации для продуктивной среды

```java
public class ProductionHikariConfig {

    public static DataSource createProductionDataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://db-host:5432/production");
        config.setUsername(System.getenv("DB_USERNAME"));
        config.setPassword(System.getenv("DB_PASSWORD"));

        // Размер пула: (CPU cores * 2) + effective_spindle_count
        // Для 4-ядерного сервера с SSD: (4 * 2) + 1 = 9, округляем до 10
        config.setMaximumPoolSize(10);
        config.setMinimumIdle(10); // В продакшене minimumIdle = maximumPoolSize

        // maxLifetime должен быть на несколько секунд меньше, чем
        // wait_timeout на стороне СУБД
        config.setMaxLifetime(1_740_000); // 29 минут (MySQL wait_timeout = 30 мин)

        config.setConnectionTimeout(10_000); // 10 секунд — fail fast
        config.setPoolName("Production-Pool");
        config.setLeakDetectionThreshold(30_000);

        // Регистрация MBean для мониторинга через JMX
        config.setRegisterMbeans(true);

        return new HikariDataSource(config);
    }
}
```

### Мониторинг через Micrometer (Spring Boot)

```java
import com.zaxxer.hikari.HikariDataSource;
import io.micrometer.core.instrument.MeterRegistry;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DataSourceMonitoringConfig {

    public DataSourceMonitoringConfig(HikariDataSource dataSource,
                                      MeterRegistry meterRegistry) {
        // HikariCP автоматически регистрирует метрики в Spring Boot
        // Доступные метрики:
        // hikaricp.connections.active   — активные соединения
        // hikaricp.connections.idle     — простаивающие соединения
        // hikaricp.connections.pending  — потоки, ожидающие соединение
        // hikaricp.connections.timeout  — количество таймаутов
        // hikaricp.connections.max      — максимальный размер пула
        dataSource.setMetricRegistry(meterRegistry);
    }
}
```

### Важное
+ `minimumIdle` в продуктивной среде рекомендуется устанавливать равным `maximumPoolSize` — это исключает задержки на создание новых соединений;
+ `maxLifetime` должен быть на несколько секунд **меньше**, чем таймаут на стороне СУБД (`wait_timeout` в MySQL, `idle_in_transaction_session_timeout` в PostgreSQL);
+ Имя пула (`poolName`) критически важно для мониторинга — особенно когда приложение использует несколько источников данных;
+ Включите `leakDetectionThreshold` в dev/stage-средах для обнаружения утечек соединений.

### Частые ошибки
+ Устанавливать `maximumPoolSize` = 50-100 — это перегружает СУБД; обычно 10-20 достаточно;
+ Забывать настроить `maxLifetime` — соединения могут быть разорваны со стороны СУБД;
+ Не закрывать `Connection` после использования (отсутствие try-with-resources) — приводит к утечке из пула;
+ Устанавливать `minimumIdle` < `maximumPoolSize` в продуктивной среде — вызывает задержки при масштабировании пула;
+ Не включать мониторинг — невозможно диагностировать проблемы с пулом в продакшене.

### Как используется в 2026
+ HikariCP 6.x — актуальная версия с поддержкой Java 21+ и виртуальных потоков;
+ В Spring Boot 3.x HikariCP является пулом по умолчанию и настраивается через `spring.datasource.hikari.*`;
+ Для cloud-native приложений параметры пула часто определяются через ConfigMap/Secrets в Kubernetes;
+ Интеграция с OpenTelemetry для распределённого трейсинга запросов к БД;
+ В GraalVM Native Image HikariCP поддерживается из коробки.

[к оглавлению](#jdbc)

## Как выполнять batch-операции в JDBC?

__Batch-операции (пакетные операции)__ позволяют отправить несколько SQL-команд на сервер за один вызов, значительно сокращая количество сетевых обращений и повышая производительность.

### Принцип работы

Вместо отправки каждого SQL-запроса отдельно, batch-операции группируют запросы и отправляют их одним пакетом:

+ `addBatch()` — добавляет SQL-команду в пакет;
+ `executeBatch()` — отправляет все накопленные команды на сервер;
+ `clearBatch()` — очищает накопленный пакет.

### Batch с Statement

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

        // results содержит количество затронутых строк для каждой команды
        for (int i = 0; i < results.length; i++) {
            System.out.println("Команда " + i + ": затронуто строк = " + results[i]);
        }
    } catch (BatchUpdateException e) {
        connection.rollback();
        System.err.println("Ошибка в batch: " + e.getMessage());
        int[] updateCounts = e.getUpdateCounts();
        for (int i = 0; i < updateCounts.length; i++) {
            if (updateCounts[i] == Statement.EXECUTE_FAILED) {
                System.err.println("Команда " + i + " завершилась с ошибкой");
            }
        }
    }
}
```

### Batch с PreparedStatement (рекомендуемый подход)

```java
public void batchInsertUsers(Connection connection, List<User> users) throws SQLException {
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

### Сравнение производительности (вставка 10 000 записей)

| Подход | Время | Сетевых обращений |
|:---|:---|:---|
| Одиночный `INSERT` | ~10 000 мс | 10 000 |
| Batch (размер 1 000) | ~500 мс | 10 |
| Batch + `rewriteBatchedStatements` | ~200 мс | 10 |

### Оптимизация для MySQL: rewriteBatchedStatements

Для MySQL параметр `rewriteBatchedStatements=true` заставляет драйвер преобразовывать отдельные `INSERT` в один мульти-`INSERT`:

```java
// URL с оптимизацией для MySQL
String url = "jdbc:mysql://localhost:3306/mydb?rewriteBatchedStatements=true";

// Драйвер преобразует:
// INSERT INTO t (a) VALUES (1); INSERT INTO t (a) VALUES (2); INSERT INTO t (a) VALUES (3);
// В:
// INSERT INTO t (a) VALUES (1), (2), (3);
```

### Batch-операции с Spring JdbcTemplate

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
            public void setValues(PreparedStatement ps, int i) throws SQLException {
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

    // Для больших объёмов — с разбиением на части
    public void batchInsertInChunks(List<User> users) {
        String sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";

        List<List<User>> chunks = partition(users, 1000);
        for (List<User> chunk : chunks) {
            jdbcTemplate.batchUpdate(sql, new BatchPreparedStatementSetter() {
                @Override
                public void setValues(PreparedStatement ps, int i) throws SQLException {
                    User user = chunk.get(i);
                    ps.setString(1, user.getName());
                    ps.setString(2, user.getEmail());
                    ps.setInt(3, user.getAge());
                }

                @Override
                public int getBatchSize() {
                    return chunk.size();
                }
            });
        }
    }

    private <T> List<List<T>> partition(List<T> list, int size) {
        List<List<T>> partitions = new java.util.ArrayList<>();
        for (int i = 0; i < list.size(); i += size) {
            partitions.add(list.subList(i, Math.min(i + size, list.size())));
        }
        return partitions;
    }
}
```

### Важное
+ Batch-операции эффективны при вставке/обновлении большого количества записей (от 100+);
+ Всегда оборачивайте batch-операции в транзакцию (`setAutoCommit(false)` + `commit()`);
+ Для больших объёмов данных разбивайте batch на части (обычно по 1000-5000 записей);
+ `PreparedStatement` batch безопаснее, чем `Statement` batch — защита от SQL Injection.

### Частые ошибки
+ Не отключать `autoCommit` — каждая команда в batch будет коммититься отдельно, теряя смысл пакетирования;
+ Накапливать слишком большой batch без промежуточного `executeBatch()` — может привести к `OutOfMemoryError`;
+ Не обрабатывать `BatchUpdateException` — невозможно определить, какие записи были успешно вставлены;
+ Забывать `clearBatch()` после `executeBatch()` при обработке данных частями.

### Как используется в 2026
+ Spring Data JDBC поддерживает batch-вставки через `saveAll()` с настройкой `spring.jdbc.template.batch-size`;
+ jOOQ предлагает типобезопасные batch-операции с автоматической оптимизацией;
+ Для массовой загрузки данных всё чаще используется `COPY` (PostgreSQL) или `LOAD DATA INFILE` (MySQL) вместо batch INSERT;
+ В реактивном стеке R2DBC поддерживает batch-операции через `Statement.add()`.

[к оглавлению](#jdbc)

## Как управлять транзакциями в JDBC?

__Транзакция__ — это набор операций с базой данных, которые выполняются как единое целое: либо все операции завершаются успешно (commit), либо все отменяются (rollback).

### Базовое управление транзакциями

По умолчанию JDBC работает в режиме `autoCommit = true`, то есть каждый SQL-запрос автоматически коммитится. Для ручного управления транзакциями необходимо отключить автокоммит:

```java
public void transferMoney(Connection connection, long fromAccountId,
                          long toAccountId, BigDecimal amount) throws SQLException {
    connection.setAutoCommit(false); // Начинаем транзакцию

    try {
        // Списание со счёта отправителя
        try (PreparedStatement debit = connection.prepareStatement(
                "UPDATE accounts SET balance = balance - ? WHERE id = ? AND balance >= ?")) {
            debit.setBigDecimal(1, amount);
            debit.setLong(2, fromAccountId);
            debit.setBigDecimal(3, amount);
            int updated = debit.executeUpdate();
            if (updated == 0) {
                throw new SQLException("Недостаточно средств на счёте " + fromAccountId);
            }
        }

        // Зачисление на счёт получателя
        try (PreparedStatement credit = connection.prepareStatement(
                "UPDATE accounts SET balance = balance + ? WHERE id = ?")) {
            credit.setBigDecimal(1, amount);
            credit.setLong(2, toAccountId);
            credit.executeUpdate();
        }

        connection.commit(); // Подтверждаем транзакцию

    } catch (SQLException e) {
        connection.rollback(); // Откатываем все изменения при ошибке
        throw e;
    } finally {
        connection.setAutoCommit(true); // Восстанавливаем режим по умолчанию
    }
}
```

### Паттерн try-with-resources для транзакций

```java
public void executeInTransaction(DataSource dataSource) {
    try (Connection conn = dataSource.getConnection()) {
        conn.setAutoCommit(false);
        try {
            // Выполняем операции
            try (PreparedStatement ps1 = conn.prepareStatement(
                    "INSERT INTO orders (user_id, total) VALUES (?, ?)")) {
                ps1.setLong(1, 1L);
                ps1.setBigDecimal(2, new BigDecimal("1500.00"));
                ps1.executeUpdate();
            }

            try (PreparedStatement ps2 = conn.prepareStatement(
                    "INSERT INTO order_items (order_id, product_id, qty) VALUES (?, ?, ?)")) {
                ps2.setLong(1, 1L);
                ps2.setLong(2, 42L);
                ps2.setInt(3, 2);
                ps2.executeUpdate();
            }

            conn.commit();
        } catch (SQLException e) {
            conn.rollback();
            throw new RuntimeException("Ошибка транзакции", e);
        }
    } catch (SQLException e) {
        throw new RuntimeException("Ошибка подключения", e);
    }
}
```

### Использование Savepoint

`Savepoint` позволяет создать промежуточную точку внутри транзакции, к которой можно откатиться без отката всей транзакции:

```java
public void transactionWithSavepoint(Connection connection) throws SQLException {
    connection.setAutoCommit(false);
    Savepoint savepoint = null;

    try {
        // Основная операция — должна выполниться обязательно
        try (PreparedStatement ps = connection.prepareStatement(
                "INSERT INTO audit_log (action, timestamp) VALUES (?, NOW())")) {
            ps.setString(1, "ORDER_CREATED");
            ps.executeUpdate();
        }

        // Создаём точку сохранения
        savepoint = connection.setSavepoint("before_notification");

        try {
            // Дополнительная операция — может завершиться неудачно
            try (PreparedStatement ps = connection.prepareStatement(
                    "INSERT INTO notifications (user_id, message) VALUES (?, ?)")) {
                ps.setLong(1, 1L);
                ps.setString(2, "Ваш заказ создан");
                ps.executeUpdate();
            }
        } catch (SQLException e) {
            // Откатываемся только до savepoint — audit_log сохраняется
            connection.rollback(savepoint);
            System.err.println("Уведомление не отправлено, но заказ создан: " + e.getMessage());
        }

        connection.commit();

    } catch (SQLException e) {
        connection.rollback(); // Полный откат при критической ошибке
        throw e;
    }
}
```

### Вспомогательный класс для управления транзакциями

```java
import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

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

// Использование:
// TransactionTemplate txTemplate = new TransactionTemplate(dataSource);
// txTemplate.executeInTransaction(conn -> {
//     // выполняем SQL-операции
//     return null;
// });
```

### Сравнение с Spring @Transactional

| Аспект | Чистый JDBC | Spring @Transactional |
|:---|:---|:---|
| Управление соединением | Вручную | Автоматически |
| Начало транзакции | `setAutoCommit(false)` | Декларативно через аннотацию |
| Коммит | `connection.commit()` | Автоматически при успешном завершении метода |
| Откат | `connection.rollback()` | Автоматически при RuntimeException |
| Уровень изоляции | `setTransactionIsolation()` | `@Transactional(isolation = ...)` |
| Propagation | Нет поддержки | Полная поддержка (REQUIRED, REQUIRES_NEW и т.д.) |

```java
// Spring @Transactional — декларативный подход
@Service
public class AccountService {

    @Transactional(isolation = Isolation.READ_COMMITTED,
                   rollbackFor = Exception.class)
    public void transferMoney(Long from, Long to, BigDecimal amount) {
        accountRepository.debit(from, amount);
        accountRepository.credit(to, amount);
        // commit происходит автоматически
        // rollback — при исключении
    }
}
```

### Важное
+ Всегда отключайте `autoCommit` перед началом транзакции;
+ Используйте try-catch-finally для гарантированного `rollback` при ошибке;
+ Закрывайте `Connection` после завершения работы с транзакцией;
+ Уровень изоляции устанавливается через `connection.setTransactionIsolation()` — выбирайте минимально необходимый.

### Частые ошибки
+ Забывать вызвать `rollback()` в блоке `catch` — данные могут остаться в неконсистентном состоянии;
+ Не восстанавливать `autoCommit` после транзакции — последующие операции через то же соединение не будут автоматически коммититься;
+ Держать транзакцию открытой слишком долго — блокирует записи в БД и снижает параллелизм;
+ Ловить `Exception` вместо `SQLException` при откате — может маскировать другие ошибки;
+ Не закрывать `PreparedStatement` внутри транзакции — утечка ресурсов.

### Как используется в 2026
+ В большинстве проектов транзакции управляются через Spring `@Transactional` или программный `TransactionTemplate`;
+ Чистый JDBC для управления транзакциями используется в микрофреймворках (Javalin, Spark) и при написании библиотек;
+ Saga-паттерн для распределённых транзакций между микросервисами вместо XA-транзакций;
+ Для реактивных приложений — `@Transactional` в Spring WebFlux с R2DBC;
+ Virtual threads (Java 21+) упрощают написание транзакционного кода без callback-ов.

[к оглавлению](#jdbc)

## Какие есть альтернативы чистому JDBC?

Чистый JDBC предоставляет низкоуровневый доступ к базам данных, но требует большого количества шаблонного кода. Существует множество инструментов, упрощающих работу с БД.

### Основные альтернативы

#### 1. Spring JdbcTemplate

Обёртка над JDBC, убирающая шаблонный код (управление соединениями, обработка исключений, закрытие ресурсов):

```java
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;

public class UserRepository {

    private final JdbcTemplate jdbcTemplate;

    public UserRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    // SELECT с маппингом
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

    // INSERT / UPDATE / DELETE
    public int updateEmail(Long userId, String newEmail) {
        return jdbcTemplate.update(
            "UPDATE users SET email = ? WHERE id = ?",
            newEmail, userId
        );
    }
}
```

#### 2. jOOQ (Java Object Oriented Querying)

Типобезопасный SQL-конструктор с генерацией кода из схемы БД:

```java
import org.jooq.DSLContext;
import static org.jooq.generated.Tables.USERS;

public class UserRepository {

    private final DSLContext dsl;

    public UserRepository(DSLContext dsl) {
        this.dsl = dsl;
    }

    // Типобезопасный SELECT — ошибки в SQL обнаруживаются на этапе компиляции
    public List<User> findByAge(int minAge) {
        return dsl.selectFrom(USERS)
            .where(USERS.AGE.ge(minAge))
            .orderBy(USERS.NAME)
            .fetchInto(User.class);
    }

    // Типобезопасный INSERT
    public void create(User user) {
        dsl.insertInto(USERS)
            .set(USERS.NAME, user.getName())
            .set(USERS.EMAIL, user.getEmail())
            .set(USERS.AGE, user.getAge())
            .execute();
    }

    // Сложные запросы с подзапросами
    public List<User> findActiveUsersWithOrders() {
        return dsl.selectFrom(USERS)
            .where(USERS.ID.in(
                dsl.select(ORDERS.USER_ID)
                   .from(ORDERS)
                   .where(ORDERS.STATUS.eq("ACTIVE"))
            ))
            .fetchInto(User.class);
    }
}
```

#### 3. JPA / Hibernate (ORM)

Объектно-реляционное отображение — работа с БД через Java-объекты:

```java
import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private int age;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders;

    // getters и setters
}

// Репозиторий с Spring Data JPA
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByAgeGreaterThanEqual(int minAge);

    @Query("SELECT u FROM User u WHERE u.email LIKE %:domain")
    List<User> findByEmailDomain(@Param("domain") String domain);
}
```

#### 4. MyBatis

SQL-маппинг: SQL пишется явно, но маппинг результатов автоматизирован:

```java
// Интерфейс маппера
@Mapper
public interface UserMapper {

    @Select("SELECT * FROM users WHERE age >= #{minAge}")
    List<User> findByAge(@Param("minAge") int minAge);

    @Insert("INSERT INTO users (name, email, age) VALUES (#{name}, #{email}, #{age})")
    @Options(useGeneratedKeys = true, keyProperty = "id")
    int insert(User user);
}
```

Или через XML:
```xml
<mapper namespace="com.example.mapper.UserMapper">
    <select id="findByAge" resultType="com.example.User">
        SELECT id, name, email, age
        FROM users
        WHERE age >= #{minAge}
        <if test="name != null">
            AND name LIKE #{name}
        </if>
        ORDER BY name
    </select>
</mapper>
```

#### 5. R2DBC (Reactive Relational Database Connectivity)

Реактивный доступ к реляционным БД (неблокирующий ввод-вывод):

```java
import org.springframework.data.r2dbc.repository.R2dbcRepository;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public interface UserRepository extends R2dbcRepository<User, Long> {

    Flux<User> findByAgeGreaterThanEqual(int minAge);

    @Query("SELECT * FROM users WHERE email LIKE :domain")
    Flux<User> findByEmailDomain(String domain);
}

// Использование
@Service
public class UserService {

    private final UserRepository userRepository;

    public Flux<User> getAdultUsers() {
        return userRepository.findByAgeGreaterThanEqual(18)
            .filter(user -> user.getEmail() != null)
            .doOnNext(user -> log.info("Найден пользователь: {}", user.getName()));
    }
}
```

### Сравнительная таблица

| Критерий | JDBC | JdbcTemplate | jOOQ | JPA/Hibernate | MyBatis | R2DBC |
|:---|:---|:---|:---|:---|:---|:---|
| **Уровень абстракции** | Низкий | Средний | Средний | Высокий | Средний | Низкий |
| **Контроль SQL** | Полный | Полный | Полный | Ограниченный | Полный | Полный |
| **Шаблонный код** | Много | Мало | Мало | Минимум | Мало | Мало |
| **Типобезопасность SQL** | Нет | Нет | Да | Нет (Criteria API — да) | Нет | Нет |
| **Кеширование** | Нет | Нет | Нет | Да (L1, L2) | Да (опционально) | Нет |
| **Реактивность** | Нет | Нет | Нет | Нет | Нет | Да |
| **Кривая обучения** | Средняя | Низкая | Средняя | Высокая | Средняя | Высокая |
| **Когда использовать** | Библиотеки, обучение | CRUD, простые запросы | Сложный SQL | Доменная модель | Сложный SQL, legacy БД | Реактивные приложения |

### Когда использовать каждый инструмент

+ **Чистый JDBC** — в библиотеках без внешних зависимостей, для обучения;
+ **JdbcTemplate** — простые CRUD-операции, когда JPA избыточен;
+ **jOOQ** — сложные SQL-запросы, необходимость типобезопасности, аналитические системы;
+ **JPA/Hibernate** — богатая доменная модель со связями, стандартные CRUD-приложения;
+ **MyBatis** — сложный SQL, работа с legacy-БД, необходимость полного контроля над SQL;
+ **R2DBC** — реактивные приложения, высоконагруженные системы с неблокирующим I/O.

### Важное
+ Выбор инструмента зависит от задачи — нет универсально лучшего решения;
+ В одном проекте можно комбинировать несколько подходов (например, JPA для CRUD + jOOQ для отчётов);
+ Знание JDBC необходимо в любом случае — все перечисленные инструменты (кроме R2DBC) работают поверх JDBC;
+ `JdbcTemplate` — оптимальный баланс между простотой и контролем для большинства задач.

### Частые ошибки
+ Использовать JPA для сложных аналитических запросов — генерируемый SQL может быть неоптимальным;
+ Игнорировать N+1 проблему в JPA/Hibernate — приводит к деградации производительности;
+ Выбирать инструмент «потому что модно», а не по требованиям проекта;
+ Смешивать JPA и чистый JDBC в одной транзакции без понимания кеша Hibernate — данные могут быть несогласованны;
+ Использовать R2DBC без реальной потребности в реактивности — добавляет сложность без выигрыша.

### Как используется в 2026
+ **Spring Data JDBC** набирает популярность как более простая альтернатива JPA для DDD (Domain-Driven Design);
+ **jOOQ** активно развивается: поддержка JSON-типов, оконных функций, CTE, и интеграция с виртуальными потоками;
+ **JPA 3.2** (Jakarta Persistence) — актуальная спецификация с поддержкой Java Records для проекций;
+ **R2DBC** используется в Spring WebFlux-проектах; пул `r2dbc-pool` базируется на Reactor;
+ Тренд на **multi-model access**: использование нескольких инструментов в одном проекте для разных задач (JPA + jOOQ, JdbcTemplate + R2DBC);
+ Генерация репозиториев и маппингов с помощью ИИ-инструментов становится распространённой практикой.

[к оглавлению](#jdbc)

# Источники
+ [Википедия - JDBC](https://ru.wikipedia.org/wiki/Java_Database_Connectivity)
+ [IBM developerWorks®](http://www.ibm.com/developerworks/ru/library/dm-1209storedprocedures/)
+ [Документация к пакету java.sql](https://docs.oracle.com/javase/7/docs/api/java/sql/package-summary.html)
+ [Википедия - Уровень изолированности транзакции](https://ru.wikipedia.org/wiki/Уровень_изолированности_транзакций)

[Вопросы для собеседования](README.md)
