[Вопросы для собеседования](README.md)

# Liquibase

+ [Что такое Liquibase и зачем нужны миграции БД?](#что-такое-liquibase-и-зачем-нужны-миграции-бд)
+ [В чём отличия Liquibase от Flyway?](#в-чём-отличия-liquibase-от-flyway)
+ [Что такое changelog, changeset, author и id?](#что-такое-changelog-changeset-author-и-id)
+ [Какие форматы changelog поддерживает Liquibase?](#какие-форматы-changelog-поддерживает-liquibase)
+ [Какова структура changeset?](#какова-структура-changeset)
+ [Какие основные типы изменений (change types) существуют в Liquibase?](#какие-основные-типы-изменений-change-types-существуют-в-liquibase)
+ [Что такое preconditions (предусловия)?](#что-такое-preconditions-предусловия)
+ [Что такое contexts и labels? Когда их использовать?](#что-такое-contexts-и-labels-когда-их-использовать)
+ [Как работает rollback в Liquibase?](#как-работает-rollback-в-liquibase)
+ [Что такое DATABASECHANGELOG и DATABASECHANGELOGLOCK?](#что-такое-databasechangelog-и-databasechangeloglock)
+ [Что такое checksum и зачем он нужен?](#что-такое-checksum-и-зачем-он-нужен)
+ [Какие основные команды есть в Liquibase?](#какие-основные-команды-есть-в-liquibase)
+ [Как интегрировать Liquibase со Spring Boot?](#как-интегрировать-liquibase-со-spring-boot)
+ [Какие best practices существуют при работе с Liquibase?](#какие-best-practices-существуют-при-работе-с-liquibase)
+ [Как работать с данными в Liquibase (insert, loadData, loadUpdateData)?](#как-работать-с-данными-в-liquibase-insert-loaddata-loadupdatedata)
+ [Что такое tag и tagDatabase?](#что-такое-tag-и-tagdatabase)
+ [Как использовать параметры и подстановки (property substitution)?](#как-использовать-параметры-и-подстановки-property-substitution)
+ [Как организовать changelog-и с помощью include и includeAll?](#как-организовать-changelog-и-с-помощью-include-и-includeall)
+ [Как использовать Liquibase в CI/CD пайплайне?](#как-использовать-liquibase-в-cicd-пайплайне)
+ [Как Liquibase работает с несколькими экземплярами приложения (кластер)?](#как-liquibase-работает-с-несколькими-экземплярами-приложения-кластер)
+ [Что делать, если changeset уже применён, но в нём обнаружена ошибка?](#что-делать-если-changeset-уже-применён-но-в-нём-обнаружена-ошибка)
+ [Как использовать Liquibase для генерации changelog из существующей БД?](#как-использовать-liquibase-для-генерации-changelog-из-существующей-бд)

## Что такое Liquibase и зачем нужны миграции БД?

**Liquibase** — это open-source библиотека для управления и отслеживания изменений схемы базы данных (database migrations). Она позволяет версионировать структуру БД так же, как исходный код версионируется в системах контроля версий (Git).

**Зачем нужны миграции БД:**

+ **Версионирование схемы** — каждое изменение БД фиксируется в виде файла, который хранится в репозитории вместе с кодом приложения.
+ **Воспроизводимость** — любой разработчик может развернуть БД с нуля или обновить до актуальной версии, выполнив все миграции последовательно.
+ **Командная работа** — несколько разработчиков могут параллельно вносить изменения в схему, а система миграций обеспечит корректный порядок применения.
+ **Аудит изменений** — полная история всех изменений БД прозрачна и доступна для проверки.
+ **Автоматизация** — миграции легко встраиваются в CI/CD пайплайн, что исключает ручное выполнение SQL-скриптов на продакшене.

Без системы миграций возникают типичные проблемы: «у меня локально работает, а на стенде — нет», потеря изменений, конфликты структур БД между окружениями, невозможность откатить изменение.

[к оглавлению](#Liquibase)

## В чём отличия Liquibase от Flyway?

| Критерий | Liquibase | Flyway |
|---|---|---|
| Формат миграций | XML, YAML, JSON, SQL | SQL, Java |
| Абстракция от СУБД | Да — можно описать изменение в XML/YAML, и Liquibase сгенерирует SQL для конкретной СУБД | Нет — миграции пишутся на конкретном диалекте SQL |
| Rollback | Поддерживает автоматический и ручной rollback | Rollback только в платной версии (Flyway Teams) |
| Preconditions | Встроенная поддержка предусловий | Нет встроенных предусловий |
| Diff между БД | Есть команда diff и generateChangeLog | Нет (только в платной версии) |
| Именование миграций | По author + id (произвольные строки) | По версии в имени файла (V1__, V2__) |
| Контексты и метки | Contexts и Labels для гибкого управления | Нет аналогов |
| Порог входа | Чуть выше из-за обилия возможностей | Проще — «просто пиши SQL» |
| Сообщество | Большое, широко используется в enterprise | Большое, популярен в стартапах |

**Когда выбрать Liquibase:** проект должен работать с несколькими СУБД, нужен rollback, preconditions, сложная организация миграций — типичный случай для банковских систем.

**Когда выбрать Flyway:** простой проект с одной СУБД, команда предпочитает чистый SQL, не нужны расширенные возможности.

[к оглавлению](#Liquibase)

## Что такое changelog, changeset, author и id?

**Changelog** — корневой файл (или набор файлов), в котором описаны все изменения БД. Это «журнал» миграций. Changelog может включать другие changelog-файлы через `include` или `includeAll`.

**Changeset** — минимальная единица изменения БД. Каждый changeset содержит одно или несколько изменений, которые применяются как единое целое (в рамках одной транзакции, если СУБД поддерживает DDL-транзакции).

**Author** — строка, идентифицирующая автора changeset. Обычно указывается имя разработчика или его логин.

**Id** — уникальный идентификатор changeset в рамках данного changelog-файла. Вместе с `author` и именем файла образует уникальный ключ changeset.

Уникальность changeset определяется тройкой: **id + author + путь к файлу changelog**.

Пример в XML:

```xml
<changeSet id="1" author="ivanov">
    <createTable tableName="users">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="username" type="VARCHAR(255)">
            <constraints nullable="false" unique="true"/>
        </column>
    </createTable>
</changeSet>
```

Пример в YAML:

```yaml
databaseChangeLog:
  - changeSet:
      id: 1
      author: ivanov
      changes:
        - createTable:
            tableName: users
            columns:
              - column:
                  name: id
                  type: BIGINT
                  autoIncrement: true
                  constraints:
                    primaryKey: true
                    nullable: false
              - column:
                  name: username
                  type: VARCHAR(255)
                  constraints:
                    nullable: false
                    unique: true
```

[к оглавлению](#Liquibase)

## Какие форматы changelog поддерживает Liquibase?

Liquibase поддерживает четыре формата описания changelog:

### XML
Самый распространённый и документированный формат. Поддерживает XSD-валидацию.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <changeSet id="1" author="petrov">
        <createTable tableName="accounts">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true"/>
            </column>
            <column name="balance" type="DECIMAL(19,2)"/>
        </createTable>
    </changeSet>

</databaseChangeLog>
```

### YAML
Более компактный и читаемый формат.

```yaml
databaseChangeLog:
  - changeSet:
      id: 1
      author: petrov
      changes:
        - createTable:
            tableName: accounts
            columns:
              - column:
                  name: id
                  type: BIGINT
                  autoIncrement: true
                  constraints:
                    primaryKey: true
              - column:
                  name: balance
                  type: DECIMAL(19,2)
```

### JSON
Альтернативный формат, используется реже.

```json
{
  "databaseChangeLog": [
    {
      "changeSet": {
        "id": "1",
        "author": "petrov",
        "changes": [
          {
            "createTable": {
              "tableName": "accounts",
              "columns": [
                {
                  "column": {
                    "name": "id",
                    "type": "BIGINT",
                    "autoIncrement": true,
                    "constraints": { "primaryKey": true }
                  }
                }
              ]
            }
          }
        ]
      }
    }
  ]
}
```

### SQL
Для тех, кто предпочитает писать чистый SQL. Метаданные указываются в комментариях.

```sql
--liquibase formatted sql

--changeset petrov:1
CREATE TABLE accounts (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    balance DECIMAL(19,2)
);
--rollback DROP TABLE accounts;
```

На практике в банковских проектах чаще всего используют **XML** (как наиболее формализованный и поддающийся валидации) или **YAML** (за компактность).

[к оглавлению](#Liquibase)

## Какова структура changeset?

Changeset — это основной строительный блок миграции. Он имеет следующую структуру:

```xml
<changeSet id="2" author="sidorov"
           context="dev,test"
           labels="JIRA-123"
           runOnChange="false"
           runAlways="false"
           failOnError="true"
           dbms="postgresql">

    <!-- Предусловия (опционально) -->
    <preConditions onFail="MARK_RAN">
        <not>
            <tableExists tableName="transactions"/>
        </not>
    </preConditions>

    <!-- Комментарий (опционально) -->
    <comment>Создание таблицы транзакций</comment>

    <!-- Одно или несколько изменений -->
    <createTable tableName="transactions">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="account_id" type="BIGINT">
            <constraints nullable="false"
                         foreignKeyName="fk_transaction_account"
                         references="accounts(id)"/>
        </column>
        <column name="amount" type="DECIMAL(19,2)"/>
        <column name="created_at" type="TIMESTAMP" defaultValueComputed="CURRENT_TIMESTAMP"/>
    </createTable>

    <!-- Rollback (опционально) -->
    <rollback>
        <dropTable tableName="transactions"/>
    </rollback>

</changeSet>
```

**Основные атрибуты changeset:**

| Атрибут | Описание |
|---|---|
| `id` | Уникальный идентификатор (обязательный) |
| `author` | Автор изменения (обязательный) |
| `dbms` | СУБД, для которой применяется changeset (postgresql, oracle, mysql и т.д.) |
| `context` | Контекст выполнения (dev, test, prod) |
| `labels` | Метки для фильтрации |
| `runOnChange` | Перевыполнить, если changeset изменился (по умолчанию false) |
| `runAlways` | Выполнять при каждом запуске (по умолчанию false) |
| `failOnError` | Остановиться при ошибке (по умолчанию true) |
| `logicalFilePath` | Переопределить путь к файлу для расчёта уникальности |

[к оглавлению](#Liquibase)

## Какие основные типы изменений (change types) существуют в Liquibase?

Liquibase предоставляет множество встроенных типов изменений, которые абстрагируют SQL-операции. Основные из них:

### Работа с таблицами

| Change Type | Описание |
|---|---|
| `createTable` | Создание таблицы |
| `dropTable` | Удаление таблицы |
| `renameTable` | Переименование таблицы |
| `addColumn` | Добавление столбца |
| `dropColumn` | Удаление столбца |
| `renameColumn` | Переименование столбца |
| `modifyDataType` | Изменение типа данных столбца |
| `addNotNullConstraint` | Добавление ограничения NOT NULL |
| `dropNotNullConstraint` | Удаление ограничения NOT NULL |
| `addDefaultValue` | Установка значения по умолчанию |

### Ключи и ограничения

| Change Type | Описание |
|---|---|
| `addPrimaryKey` | Добавление первичного ключа |
| `dropPrimaryKey` | Удаление первичного ключа |
| `addForeignKeyConstraint` | Добавление внешнего ключа |
| `dropForeignKeyConstraint` | Удаление внешнего ключа |
| `addUniqueConstraint` | Добавление уникального ограничения |
| `dropUniqueConstraint` | Удаление уникального ограничения |

### Индексы

| Change Type | Описание |
|---|---|
| `createIndex` | Создание индекса |
| `dropIndex` | Удаление индекса |

### Представления и последовательности

| Change Type | Описание |
|---|---|
| `createView` | Создание представления (VIEW) |
| `dropView` | Удаление представления |
| `createSequence` | Создание последовательности |
| `dropSequence` | Удаление последовательности |
| `alterSequence` | Изменение последовательности |

### SQL напрямую

| Change Type | Описание |
|---|---|
| `sql` | Выполнение произвольного SQL |
| `sqlFile` | Выполнение SQL из файла |

Пример использования нескольких типов изменений в XML:

```xml
<changeSet id="3" author="kozlov">
    <addColumn tableName="users">
        <column name="email" type="VARCHAR(320)">
            <constraints nullable="false"/>
        </column>
    </addColumn>
</changeSet>

<changeSet id="4" author="kozlov">
    <createIndex indexName="idx_users_email" tableName="users">
        <column name="email"/>
    </createIndex>
</changeSet>

<changeSet id="5" author="kozlov">
    <addForeignKeyConstraint
        constraintName="fk_orders_user"
        baseTableName="orders"
        baseColumnNames="user_id"
        referencedTableName="users"
        referencedColumnNames="id"
        onDelete="CASCADE"
        onUpdate="NO ACTION"/>
</changeSet>
```

[к оглавлению](#Liquibase)

## Что такое preconditions (предусловия)?

**Preconditions** — это проверки, которые Liquibase выполняет перед применением changeset (или перед обработкой всего changelog). Если предусловие не выполнено, Liquibase может пропустить changeset, пометить его как выполненный, вызвать ошибку или остановиться.

**Основные предусловия:**

| Precondition | Описание |
|---|---|
| `tableExists` | Проверяет, существует ли таблица |
| `columnExists` | Проверяет, существует ли столбец |
| `indexExists` | Проверяет, существует ли индекс |
| `foreignKeyConstraintExists` | Проверяет, существует ли внешний ключ |
| `viewExists` | Проверяет, существует ли представление |
| `sequenceExists` | Проверяет, существует ли последовательность |
| `dbms` | Проверяет тип СУБД |
| `runningAs` | Проверяет имя пользователя БД |
| `sqlCheck` | Выполняет SQL-запрос и сверяет результат |
| `changeSetExecuted` | Проверяет, был ли выполнен другой changeset |

**Действия при невыполнении предусловия (`onFail`):**

+ `HALT` — прекратить выполнение (по умолчанию)
+ `CONTINUE` — пропустить changeset и продолжить
+ `MARK_RAN` — пометить changeset как выполненный и продолжить
+ `WARN` — вывести предупреждение и продолжить

Пример:

```xml
<changeSet id="6" author="fedorov">
    <preConditions onFail="MARK_RAN">
        <not>
            <columnExists tableName="users" columnName="phone"/>
        </not>
    </preConditions>

    <addColumn tableName="users">
        <column name="phone" type="VARCHAR(20)"/>
    </addColumn>
</changeSet>
```

В данном примере, если столбец `phone` уже существует, changeset будет помечен как выполненный (MARK_RAN) и пропущен. Это удобно при работе с уже существующими БД.

Предусловия можно комбинировать с помощью логических операторов `and`, `or`, `not`:

```xml
<preConditions onFail="HALT">
    <and>
        <dbms type="postgresql"/>
        <tableExists tableName="users"/>
        <not>
            <columnExists tableName="users" columnName="middle_name"/>
        </not>
    </and>
</preConditions>
```

[к оглавлению](#Liquibase)

## Что такое contexts и labels? Когда их использовать?

### Contexts

**Contexts** позволяют привязать changeset к определённому окружению (dev, test, staging, prod) и выполнять его только при совпадении контекста.

```xml
<changeSet id="7" author="ivanov" context="dev">
    <insert tableName="users">
        <column name="username" value="test_user"/>
        <column name="email" value="test@example.com"/>
    </insert>
</changeSet>
```

При запуске Liquibase указывается контекст:

```bash
liquibase --contexts=dev update
```

Или в Spring Boot:

```properties
spring.liquibase.contexts=dev
```

Changeset без указания context выполняется **всегда**, вне зависимости от переданного контекста.

### Labels

**Labels** — это более гибкий механизм фильтрации changeset. Labels поддерживают логические выражения (AND, OR, NOT, скобки).

```xml
<changeSet id="8" author="ivanov" labels="JIRA-456, release-2.0">
    <addColumn tableName="accounts">
        <column name="currency" type="VARCHAR(3)" defaultValue="RUB"/>
    </addColumn>
</changeSet>
```

При запуске:

```bash
liquibase --label-filter="JIRA-456" update
```

### Разница между contexts и labels

| Критерий | Contexts | Labels |
|---|---|---|
| Назначение | Разделение по окружениям | Разделение по задачам, релизам, фичам |
| Логика на changeset | Через запятую (OR) | Через запятую (OR) |
| Логика при запуске | Через запятую (OR) | Поддерживает AND, OR, NOT, скобки |
| Типичный пример | `context="dev,test"` | `labels="JIRA-123, release-1.5"` |

[к оглавлению](#Liquibase)

## Как работает rollback в Liquibase?

**Rollback** — это механизм отката изменений, внесённых changeset-ами. Liquibase поддерживает два вида rollback:

### Автоматический rollback

Для многих типов изменений Liquibase умеет генерировать обратные операции автоматически:

| Изменение | Автоматический rollback |
|---|---|
| `createTable` | `dropTable` |
| `addColumn` | `dropColumn` |
| `createIndex` | `dropIndex` |
| `addForeignKeyConstraint` | `dropForeignKeyConstraint` |
| `createView` | `dropView` |
| `createSequence` | `dropSequence` |
| `renameTable` | `renameTable` (обратное) |

### Ручной rollback

Для операций, которые Liquibase не может откатить автоматически (например, `dropTable`, `sql`, `insert`), необходимо указать rollback вручную:

```xml
<changeSet id="9" author="petrov">
    <dropColumn tableName="users" columnName="middle_name"/>

    <rollback>
        <addColumn tableName="users">
            <column name="middle_name" type="VARCHAR(100)"/>
        </addColumn>
    </rollback>
</changeSet>
```

Для SQL-формата:

```sql
--changeset petrov:10
ALTER TABLE users ADD COLUMN status VARCHAR(20) DEFAULT 'ACTIVE';
--rollback ALTER TABLE users DROP COLUMN status;
```

### Команды rollback

```bash
# Откат последних N changeset-ов
liquibase rollbackCount 1

# Откат до определённого тега
liquibase rollback release-1.0

# Откат до определённой даты
liquibase rollbackToDate 2025-01-15T10:00:00

# Генерация SQL для отката (без выполнения)
liquibase rollbackCountSQL 1
liquibase rollbackSQL release-1.0
```

Рекомендуется **всегда** явно указывать rollback для changeset-ов, даже если Liquibase может сгенерировать его автоматически — это делает миграции более предсказуемыми.

[к оглавлению](#Liquibase)

## Что такое DATABASECHANGELOG и DATABASECHANGELOGLOCK?

Это две служебные таблицы, которые Liquibase автоматически создаёт в БД при первом запуске.

### DATABASECHANGELOG

Хранит информацию о **каждом применённом changeset**. Основные столбцы:

| Столбец | Описание |
|---|---|
| `ID` | Идентификатор changeset |
| `AUTHOR` | Автор changeset |
| `FILENAME` | Путь к файлу changelog |
| `DATEEXECUTED` | Дата и время применения |
| `ORDEREXECUTED` | Порядковый номер выполнения |
| `EXECTYPE` | Тип выполнения (EXECUTED, FAILED, SKIPPED, RERAN, MARK_RAN) |
| `MD5SUM` | Контрольная сумма (checksum) changeset |
| `DESCRIPTION` | Описание типа изменения |
| `COMMENTS` | Комментарий из changeset |
| `TAG` | Тег (если был проставлен) |
| `LIQUIBASE` | Версия Liquibase |
| `CONTEXTS` | Контексты changeset |
| `LABELS` | Метки changeset |
| `DEPLOYMENT_ID` | Идентификатор развёртывания |

### DATABASECHANGELOGLOCK

Предотвращает **одновременное выполнение** миграций несколькими экземплярами приложения. Содержит столбцы:

| Столбец | Описание |
|---|---|
| `ID` | Идентификатор (обычно 1) |
| `LOCKED` | Флаг блокировки (true/false) |
| `LOCKGRANTED` | Время установки блокировки |
| `LOCKEDBY` | Хост, который установил блокировку |

Перед началом миграции Liquibase устанавливает `LOCKED = true`, а после завершения — `LOCKED = false`. Если приложение аварийно завершилось во время миграции, блокировка может «зависнуть». В этом случае можно вручную снять блокировку:

```bash
liquibase releaseLocks
```

Или SQL:

```sql
UPDATE DATABASECHANGELOGLOCK SET LOCKED = FALSE, LOCKGRANTED = NULL, LOCKEDBY = NULL WHERE ID = 1;
```

[к оглавлению](#Liquibase)

## Что такое checksum и зачем он нужен?

**Checksum (контрольная сумма)** — это хеш (MD5), который Liquibase вычисляет для каждого changeset на основе его содержимого. Checksum сохраняется в столбце `MD5SUM` таблицы `DATABASECHANGELOG`.

**Зачем нужен checksum:**

+ **Обнаружение изменений** — при каждом запуске Liquibase сверяет текущий checksum changeset с сохранённым в БД. Если они не совпадают, это значит, что уже применённый changeset был изменён.
+ **Защита целостности** — гарантирует, что миграции идемпотентны и выполняются одинаково на всех окружениях.

### Что делать при ошибке checksum?

Ошибка `Validation Failed: X changeset(s) had their checksum changed` возникает, когда содержимое уже применённого changeset было изменено.

**Способы решения:**

1. **Откатить изменение changeset** — вернуть его к исходному виду (предпочтительный вариант).

2. **Выполнить `clearCheckSums`** — сбрасывает все checksum, при следующем запуске они будут пересчитаны:
   ```bash
   liquibase clearCheckSums
   ```
   Это безопасно, так как Liquibase просто пересчитает checksum для всех changeset-ов и запишет новые значения.

3. **Установить `validCheckSum`** — разрешить конкретный changeset с определённым checksum:
   ```xml
   <changeSet id="1" author="ivanov">
       <validCheckSum>8:a]b234cf56</validCheckSum>
       <validCheckSum>ANY</validCheckSum>
       <!-- ... -->
   </changeSet>
   ```

**Важно:** Никогда не изменяйте уже применённые changeset-ы. Это одно из ключевых правил работы с Liquibase. Если нужно внести изменения — создайте новый changeset.

[к оглавлению](#Liquibase)

## Какие основные команды есть в Liquibase?

### Команды обновления

| Команда | Описание |
|---|---|
| `update` | Применить все невыполненные changeset-ы |
| `updateCount <N>` | Применить следующие N changeset-ов |
| `updateSQL` | Вывести SQL, который будет выполнен (dry-run), без применения |
| `updateTestingRollback` | Применить changeset, откатить, применить снова — для проверки rollback |

### Команды отката

| Команда | Описание |
|---|---|
| `rollback <tag>` | Откатить до указанного тега |
| `rollbackCount <N>` | Откатить последние N changeset-ов |
| `rollbackToDate <date>` | Откатить до указанной даты |
| `rollbackSQL <tag>` | Вывести SQL для отката (без выполнения) |

### Информационные команды

| Команда | Описание |
|---|---|
| `status` | Показать список невыполненных changeset-ов |
| `history` | Показать историю выполненных changeset-ов |
| `validate` | Проверить changelog на корректность (синтаксис, checksum) |

### Вспомогательные команды

| Команда | Описание |
|---|---|
| `diff` | Сравнить две БД и показать различия |
| `diffChangeLog` | Сравнить две БД и сгенерировать changelog с различиями |
| `generateChangeLog` | Сгенерировать changelog из существующей БД |
| `clearCheckSums` | Сбросить все контрольные суммы |
| `releaseLocks` | Снять блокировку в DATABASECHANGELOGLOCK |
| `tag <name>` | Пометить текущее состояние БД тегом |
| `dropAll` | Удалить все объекты из схемы (осторожно!) |

Пример типичного использования:

```bash
# Проверить, что changelog корректен
liquibase validate

# Посмотреть, что будет выполнено
liquibase status

# Посмотреть SQL без выполнения
liquibase updateSQL

# Применить миграции
liquibase update

# Пометить текущее состояние
liquibase tag release-1.0
```

[к оглавлению](#Liquibase)

## Как интегрировать Liquibase со Spring Boot?

### Подключение зависимости

Maven:

```xml
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
</dependency>
```

В Spring Boot Starter уже есть автоконфигурация — достаточно добавить зависимость, и Liquibase запустится автоматически при старте приложения.

### Основные свойства (application.properties / application.yml)

```properties
# Путь к master changelog (по умолчанию classpath:db/changelog/db.changelog-master.yaml)
spring.liquibase.change-log=classpath:db/changelog/db.changelog-master.xml

# Включить/отключить Liquibase (по умолчанию true)
spring.liquibase.enabled=true

# Контексты для выполнения
spring.liquibase.contexts=dev

# Метки для фильтрации
spring.liquibase.label-filter=release-1.0

# Схема по умолчанию
spring.liquibase.default-schema=public

# Схема для служебных таблиц Liquibase
spring.liquibase.liquibase-schema=liquibase

# Удалить все объекты перед миграцией (осторожно! только для тестов)
spring.liquibase.drop-first=false

# Отдельный источник данных для Liquibase (если нужен другой пользователь)
spring.liquibase.url=jdbc:postgresql://localhost:5432/mydb
spring.liquibase.user=liquibase_admin
spring.liquibase.password=secret

# Параметры для подстановки в changelog
spring.liquibase.parameters.env=dev
```

### Пример application.yml

```yaml
spring:
  liquibase:
    change-log: classpath:db/changelog/db.changelog-master.xml
    enabled: true
    contexts: ${LIQUIBASE_CONTEXTS:dev}
    default-schema: public
```

### Порядок работы

1. При старте приложения Spring Boot автоматически запускает `SpringLiquibase` bean.
2. Liquibase подключается к БД, используя `DataSource` приложения (или отдельный, если указан).
3. Создаёт служебные таблицы (если их нет).
4. Проверяет и применяет невыполненные changeset-ы.
5. Только после успешного завершения миграций приложение продолжает старт.

Если миграция завершилась ошибкой, приложение **не запустится** — это правильное поведение, так как работа с несогласованной схемой БД недопустима.

[к оглавлению](#Liquibase)

## Какие best practices существуют при работе с Liquibase?

### 1. Один changeset — одно изменение

Каждый changeset должен содержать одно логическое изменение. Это упрощает rollback и отладку.

```xml
<!-- Плохо: два изменения в одном changeset -->
<changeSet id="bad-1" author="dev">
    <createTable tableName="orders">...</createTable>
    <createTable tableName="order_items">...</createTable>
</changeSet>

<!-- Хорошо: каждое изменение в отдельном changeset -->
<changeSet id="1" author="dev">
    <createTable tableName="orders">...</createTable>
</changeSet>
<changeSet id="2" author="dev">
    <createTable tableName="order_items">...</createTable>
</changeSet>
```

### 2. Никогда не изменять уже применённые changeset-ы

После того как changeset выполнен на любом окружении — его содержимое нельзя менять. Это приведёт к ошибке checksum. Если нужно исправление — создавайте новый changeset.

### 3. Всегда указывать rollback

Даже если Liquibase может сгенерировать rollback автоматически, явное указание повышает предсказуемость.

### 4. Именование файлов

Использовать последовательную и понятную схему именования:

```
db/changelog/
├── db.changelog-master.xml
├── changes/
│   ├── 001-create-users-table.xml
│   ├── 002-create-accounts-table.xml
│   ├── 003-add-email-to-users.xml
│   └── 004-create-transactions-table.xml
```

### 5. Использовать осмысленные id

```xml
<!-- Плохо -->
<changeSet id="1" author="dev">

<!-- Хорошо -->
<changeSet id="2025-01-15-create-users-table" author="ivanov">
```

### 6. Не использовать `runAlways` и `runOnChange` без необходимости

Эти атрибуты допустимы для хранимых процедур или представлений, но не для DDL-операций.

### 7. Использовать `logicalFilePath`

При переносе файлов changelog в другую директорию указывайте `logicalFilePath`, чтобы не нарушить уникальность changeset:

```xml
<databaseChangeLog logicalFilePath="db/changelog/initial.xml">
```

### 8. Тестировать миграции

Использовать `updateTestingRollback` для проверки, что rollback работает корректно. Запускать миграции на чистой БД в CI.

### 9. Разделять структурные и data-миграции

Структурные изменения (DDL) и загрузку данных (DML) лучше разделять в разные changeset-ы.

### 10. Использовать `preconditions` для безопасности

Особенно полезно при работе с уже существующими БД или при деплое на разные окружения.

[к оглавлению](#Liquibase)

## Как работать с данными в Liquibase (insert, loadData, loadUpdateData)?

### insert

Вставка одной записи:

```xml
<changeSet id="10" author="ivanov">
    <insert tableName="currencies">
        <column name="code" value="RUB"/>
        <column name="name" value="Российский рубль"/>
        <column name="is_active" valueBoolean="true"/>
    </insert>
    <insert tableName="currencies">
        <column name="code" value="USD"/>
        <column name="name" value="Доллар США"/>
        <column name="is_active" valueBoolean="true"/>
    </insert>

    <rollback>
        <delete tableName="currencies">
            <where>code IN ('RUB', 'USD')</where>
        </delete>
    </rollback>
</changeSet>
```

### loadData

Загрузка данных из CSV-файла:

```xml
<changeSet id="11" author="ivanov">
    <loadData tableName="currencies"
              file="db/data/currencies.csv"
              separator=","
              encoding="UTF-8">
        <column name="code" type="STRING"/>
        <column name="name" type="STRING"/>
        <column name="is_active" type="BOOLEAN"/>
    </loadData>
</changeSet>
```

Файл `currencies.csv`:

```
code,name,is_active
RUB,Российский рубль,true
USD,Доллар США,true
EUR,Евро,true
```

### loadUpdateData

Комбинация INSERT и UPDATE — если запись существует (по первичному ключу или указанному столбцу), она обновляется; если не существует — вставляется:

```xml
<changeSet id="12" author="ivanov">
    <loadUpdateData tableName="currencies"
                    file="db/data/currencies.csv"
                    primaryKey="code"
                    separator=","
                    encoding="UTF-8">
        <column name="code" type="STRING"/>
        <column name="name" type="STRING"/>
        <column name="is_active" type="BOOLEAN"/>
    </loadUpdateData>
</changeSet>
```

Это особенно полезно для справочных данных, которые могут обновляться от релиза к релизу.

YAML-пример вставки данных:

```yaml
databaseChangeLog:
  - changeSet:
      id: 13
      author: ivanov
      changes:
        - insert:
            tableName: currencies
            columns:
              - column:
                  name: code
                  value: CNY
              - column:
                  name: name
                  value: Китайский юань
              - column:
                  name: is_active
                  valueBoolean: true
```

[к оглавлению](#Liquibase)

## Что такое tag и tagDatabase?

**Tag** — это именованная метка, которая ставится на определённое состояние БД. Теги используются для фиксации контрольных точек, к которым можно откатиться.

### Установка тега через командную строку

```bash
liquibase tag release-1.0
```

Эта команда добавляет запись в столбец `TAG` последнего выполненного changeset в таблице `DATABASECHANGELOG`.

### Установка тега через changeset

```xml
<changeSet id="tag-release-1.0" author="release-manager">
    <tagDatabase tag="release-1.0"/>
</changeSet>
```

YAML:

```yaml
databaseChangeLog:
  - changeSet:
      id: tag-release-1.0
      author: release-manager
      changes:
        - tagDatabase:
            tag: release-1.0
```

### Откат до тега

```bash
liquibase rollback release-1.0
```

Liquibase откатит все changeset-ы, применённые **после** указанного тега, в обратном порядке.

### Типичная схема использования тегов

```xml
<!-- Тег перед релизом -->
<changeSet id="tag-1.0" author="release">
    <tagDatabase tag="release-1.0"/>
</changeSet>

<!-- Миграции релиза 1.1 -->
<changeSet id="add-status-to-users" author="dev">
    <addColumn tableName="users">
        <column name="status" type="VARCHAR(20)" defaultValue="ACTIVE"/>
    </addColumn>
</changeSet>

<!-- Тег после релиза -->
<changeSet id="tag-1.1" author="release">
    <tagDatabase tag="release-1.1"/>
</changeSet>
```

Теги особенно важны в банковских проектах, где откат до известного стабильного состояния должен быть возможен в любой момент.

[к оглавлению](#Liquibase)

## Как использовать параметры и подстановки (property substitution)?

Liquibase позволяет использовать параметры (properties), которые подставляются в changelog во время выполнения. Это позволяет адаптировать миграции под разные окружения.

### Определение параметров в changelog

```xml
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <property name="table.prefix" value="app_"/>
    <property name="varchar.type" value="VARCHAR" dbms="postgresql"/>
    <property name="varchar.type" value="NVARCHAR" dbms="mssql"/>
    <property name="id.type" value="BIGINT"/>

    <changeSet id="14" author="dev">
        <createTable tableName="${table.prefix}users">
            <column name="id" type="${id.type}" autoIncrement="true">
                <constraints primaryKey="true"/>
            </column>
            <column name="name" type="${varchar.type}(255)"/>
        </createTable>
    </changeSet>

</databaseChangeLog>
```

### Передача параметров через командную строку

```bash
liquibase -Dtable.prefix=myapp_ update
```

### Передача параметров через Spring Boot

```properties
spring.liquibase.parameters.table.prefix=app_
spring.liquibase.parameters.schema.name=banking
```

### Передача через файл liquibase.properties

```properties
parameter.table.prefix=app_
parameter.schema.name=banking
```

### Приоритет параметров (от высшего к низшему)

1. Параметры из командной строки (`-D`)
2. Параметры из `liquibase.properties`
3. Параметры из Spring Boot (`spring.liquibase.parameters.*`)
4. Параметры, определённые в `<property>` внутри changelog

YAML-пример:

```yaml
databaseChangeLog:
  - property:
      name: table.prefix
      value: app_
  - property:
      name: varchar.type
      value: VARCHAR
      dbms: postgresql
  - changeSet:
      id: 15
      author: dev
      changes:
        - createTable:
            tableName: ${table.prefix}settings
            columns:
              - column:
                  name: key
                  type: ${varchar.type}(100)
              - column:
                  name: value
                  type: ${varchar.type}(500)
```

[к оглавлению](#Liquibase)

## Как организовать changelog-и с помощью include и includeAll?

В реальных проектах один файл changelog быстро становится неуправляемым. Liquibase предлагает два механизма для организации changelog-ов: `include` и `includeAll`.

### include

Включает конкретный файл changelog:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <include file="db/changelog/releases/release-1.0.xml"/>
    <include file="db/changelog/releases/release-1.1.xml"/>
    <include file="db/changelog/releases/release-2.0.xml"/>

</databaseChangeLog>
```

### includeAll

Включает все файлы из указанной директории (в алфавитном порядке):

```xml
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <includeAll path="db/changelog/changes/"/>

</databaseChangeLog>
```

### Типичная структура проекта

```
src/main/resources/
└── db/
    └── changelog/
        ├── db.changelog-master.xml          (корневой changelog)
        ├── changes/
        │   ├── 001-create-users.xml
        │   ├── 002-create-accounts.xml
        │   ├── 003-add-email-to-users.xml
        │   └── 004-create-transactions.xml
        └── data/
            ├── currencies.csv
            └── default-settings.csv
```

### Организация по релизам

```
src/main/resources/
└── db/
    └── changelog/
        ├── db.changelog-master.xml
        ├── release-1.0/
        │   ├── 001-create-users.xml
        │   └── 002-create-accounts.xml
        └── release-1.1/
            ├── 001-add-email-to-users.xml
            └── 002-create-transactions.xml
```

Master changelog:

```xml
<databaseChangeLog ...>
    <includeAll path="db/changelog/release-1.0/"/>
    <includeAll path="db/changelog/release-1.1/"/>
</databaseChangeLog>
```

YAML-пример master changelog:

```yaml
databaseChangeLog:
  - includeAll:
      path: db/changelog/changes/
```

**Важно:** при использовании `includeAll` файлы включаются в **алфавитном порядке**, поэтому нумерация в именах файлов обязательна (001-, 002- и т.д.).

[к оглавлению](#Liquibase)

## Как использовать Liquibase в CI/CD пайплайне?

Liquibase хорошо интегрируется в CI/CD процессы, обеспечивая автоматическое применение миграций при деплое.

### Основные стратегии

**1. Миграции при старте приложения (Spring Boot)**

Самый простой подход — Liquibase запускается автоматически при старте приложения. Подходит для большинства проектов.

```yaml
# application.yml
spring:
  liquibase:
    enabled: true
    change-log: classpath:db/changelog/db.changelog-master.xml
```

**2. Отдельный шаг в пайплайне**

Миграции выполняются отдельным шагом до деплоя приложения. Подходит для сложных сценариев и банковских систем.

```yaml
# Пример для GitLab CI
stages:
  - validate
  - migrate
  - deploy

validate-migrations:
  stage: validate
  script:
    - liquibase --changelog-file=db/changelog/db.changelog-master.xml validate
    - liquibase --changelog-file=db/changelog/db.changelog-master.xml status

apply-migrations:
  stage: migrate
  script:
    - liquibase --changelog-file=db/changelog/db.changelog-master.xml
                --url=$DB_URL
                --username=$DB_USER
                --password=$DB_PASSWORD
                update
  only:
    - main

deploy-app:
  stage: deploy
  script:
    - ./deploy.sh
  only:
    - main
```

### Maven-плагин

```xml
<plugin>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <configuration>
        <changeLogFile>src/main/resources/db/changelog/db.changelog-master.xml</changeLogFile>
        <url>${db.url}</url>
        <username>${db.username}</username>
        <password>${db.password}</password>
    </configuration>
</plugin>
```

Команды:

```bash
mvn liquibase:update
mvn liquibase:rollback -Dliquibase.rollbackCount=1
mvn liquibase:status
mvn liquibase:diff
```

### Gradle-плагин

```groovy
plugins {
    id 'org.liquibase.gradle' version '2.2.0'
}

liquibase {
    activities {
        main {
            changelogFile 'src/main/resources/db/changelog/db.changelog-master.xml'
            url project.ext.dbUrl
            username project.ext.dbUser
            password project.ext.dbPassword
        }
    }
}
```

### Рекомендации для CI/CD

+ **Всегда запускать `validate`** перед применением миграций — это поймает синтаксические ошибки и проблемы с checksum.
+ **Использовать `updateSQL`** для ревью SQL перед применением на production.
+ **Ставить теги** перед каждым релизом для возможности быстрого отката.
+ **Отделять миграцию от деплоя** — сначала накатить миграции, потом развернуть новую версию приложения.
+ **Использовать отдельного пользователя БД** для Liquibase с правами на DDL, а приложение подключать пользователем с ограниченными правами.
+ **Тестировать миграции на чистой БД** — в CI запускать полный цикл миграций на пустой базе.

[к оглавлению](#Liquibase)

## Как Liquibase работает с несколькими экземплярами приложения (кластер)?

В кластерном окружении (например, несколько экземпляров Spring Boot-приложения) возникает проблема: несколько экземпляров могут одновременно попытаться выполнить миграции.

Liquibase решает эту проблему с помощью таблицы **DATABASECHANGELOGLOCK**:

1. Перед началом миграции экземпляр пытается установить блокировку (`LOCKED = true`).
2. Если блокировка уже установлена другим экземпляром — текущий экземпляр **ждёт** (по умолчанию до 5 минут).
3. После завершения миграции блокировка снимается (`LOCKED = false`).
4. Остальные экземпляры обнаруживают, что все changeset-ы уже применены, и стартуют нормально.

### Возможные проблемы

**Зависшая блокировка** — если экземпляр аварийно завершился во время миграции, блокировка остаётся. Решение:

```bash
liquibase releaseLocks
```

Или в SQL:

```sql
UPDATE DATABASECHANGELOGLOCK SET LOCKED = FALSE WHERE ID = 1;
```

**Таймаут ожидания** — если блокировка держится слишком долго, другие экземпляры не смогут стартовать. Настройка таймаута:

```properties
# Spring Boot
spring.liquibase.lock-wait-time=300
```

### Рекомендации

В банковских системах предпочтительнее выполнять миграции **отдельным шагом до деплоя**, а не при старте приложения. Это позволяет:

+ Избежать проблем с блокировками в кластере.
+ Контролировать момент применения миграций.
+ Быстрее откатить изменения при проблемах.

[к оглавлению](#Liquibase)

## Что делать, если changeset уже применён, но в нём обнаружена ошибка?

Это распространённая ситуация, и есть несколько подходов к решению:

### 1. Создать новый changeset с исправлением (рекомендуемый подход)

```xml
<!-- Исходный changeset (уже применён, не трогаем!) -->
<changeSet id="20" author="dev">
    <createTable tableName="clients">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true"/>
        </column>
        <column name="name" type="VARCHAR(50)"/>  <!-- Ошибка: слишком маленький размер -->
    </createTable>
</changeSet>

<!-- Новый changeset с исправлением -->
<changeSet id="21" author="dev">
    <modifyDataType tableName="clients" columnName="name" newDataType="VARCHAR(255)"/>
</changeSet>
```

### 2. Откатить и применить заново (только если changeset ещё не на production)

```bash
liquibase rollbackCount 1
```

Затем исправить changeset и выполнить `update`. Этот подход допустим **только** на dev/test окружениях, где changeset ещё не был применён.

### 3. Использовать `runOnChange="true"` (для процедур и представлений)

```xml
<changeSet id="create-view-active-clients" author="dev" runOnChange="true">
    <createView viewName="active_clients">
        SELECT * FROM clients WHERE status = 'ACTIVE'
    </createView>
</changeSet>
```

При изменении такого changeset Liquibase перевыполнит его (вместо ошибки checksum).

### 4. Удалить запись из DATABASECHANGELOG (крайний случай)

```sql
DELETE FROM DATABASECHANGELOG
WHERE ID = '20' AND AUTHOR = 'dev' AND FILENAME = 'db/changelog/changes/020-create-clients.xml';
```

После этого можно исправить changeset и применить его заново. **Этот подход опасен** и допустим только на dev-окружении.

### Общее правило

Никогда не изменять уже применённые changeset-ы на production. Всегда создавать новый changeset с исправлением.

[к оглавлению](#Liquibase)

## Как использовать Liquibase для генерации changelog из существующей БД?

Команда `generateChangeLog` позволяет создать changelog на основе существующей структуры БД. Это полезно при внедрении Liquibase в уже существующий проект.

### Генерация через командную строку

```bash
liquibase --url=jdbc:postgresql://localhost:5432/mydb \
          --username=admin \
          --password=secret \
          generateChangeLog \
          --changelog-file=db/changelog/generated-changelog.xml
```

### Генерация через Maven

```bash
mvn liquibase:generateChangeLog
```

### Что генерируется

+ Таблицы и столбцы
+ Первичные ключи
+ Внешние ключи
+ Уникальные ограничения
+ Индексы
+ Представления (views)
+ Последовательности (sequences)

### Что НЕ генерируется

+ Хранимые процедуры и функции
+ Триггеры
+ Данные в таблицах (для этого используется отдельная опция `--diffTypes=data`)

### Команда diff

Сравнение двух БД:

```bash
liquibase --url=jdbc:postgresql://localhost:5432/dev_db \
          --username=admin \
          --password=secret \
          diff \
          --referenceUrl=jdbc:postgresql://localhost:5432/prod_db \
          --referenceUsername=admin \
          --referencePassword=secret
```

### diffChangeLog

Генерация changelog с различиями между двумя БД:

```bash
liquibase --url=jdbc:postgresql://localhost:5432/dev_db \
          --username=admin \
          --password=secret \
          diffChangeLog \
          --referenceUrl=jdbc:postgresql://localhost:5432/prod_db \
          --referenceUsername=admin \
          --referencePassword=secret \
          --changelog-file=diff-changelog.xml
```

### Типичный сценарий внедрения Liquibase в существующий проект

1. Сгенерировать changelog из production БД:
   ```bash
   liquibase generateChangeLog --changelog-file=initial-schema.xml
   ```

2. Проверить и отредактировать сгенерированный changelog.

3. Применить changelog с `changeLogSync` (отметить все changeset-ы как выполненные без реального применения):
   ```bash
   liquibase changeLogSync
   ```

4. С этого момента все новые изменения добавляются через обычные changeset-ы.

**Важно:** сгенерированный changelog следует рассматривать как отправную точку — его нужно просмотреть, подчистить и при необходимости переструктурировать.

[к оглавлению](#Liquibase)
