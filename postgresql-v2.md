[Вопросы для собеседования](README.md)

# PostgreSQL

+ [Что такое PostgreSQL и чем он отличается от других СУБД?](#что-такое-postgresql-и-чем-он-отличается-от-других-субд)
+ [Какие основные типы данных поддерживает PostgreSQL?](#какие-основные-типы-данных-поддерживает-postgresql)
+ [Что такое схемы (schemas) в PostgreSQL?](#что-такое-схемы-schemas-в-postgresql)
+ [Что такое последовательности (sequences) и как они работают?](#что-такое-последовательности-sequences-и-как-они-работают)
+ [Чем отличаются типы serial и bigserial от явного использования последовательностей?](#чем-отличаются-типы-serial-и-bigserial-от-явного-использования-последовательностей)
+ [Что такое JSONB в PostgreSQL и чем он отличается от JSON?](#что-такое-jsonb-в-postgresql-и-чем-он-отличается-от-json)
+ [Какие операторы существуют для работы с JSONB?](#какие-операторы-существуют-для-работы-с-jsonb)
+ [Как индексировать JSONB-поля?](#как-индексировать-jsonb-поля)
+ [Как работать с массивами в PostgreSQL?](#как-работать-с-массивами-в-postgresql)
+ [Какие типы индексов поддерживает PostgreSQL?](#какие-типы-индексов-поддерживает-postgresql)
+ [Когда какой тип индекса использовать?](#когда-какой-тип-индекса-использовать)
+ [Что такое составные индексы и как они работают?](#что-такое-составные-индексы-и-как-они-работают)
+ [Что такое частичные индексы и индексы на выражениях?](#что-такое-частичные-индексы-и-индексы-на-выражениях)
+ [Как использовать EXPLAIN и EXPLAIN ANALYZE для анализа запросов?](#как-использовать-explain-и-explain-analyze-для-анализа-запросов)
+ [Как читать план выполнения запроса?](#как-читать-план-выполнения-запроса)
+ [Какие основные приёмы оптимизации запросов в PostgreSQL?](#какие-основные-приёмы-оптимизации-запросов-в-postgresql)
+ [Как работают транзакции в PostgreSQL?](#как-работают-транзакции-в-postgresql)
+ [Какие уровни изоляции транзакций поддерживает PostgreSQL?](#какие-уровни-изоляции-транзакций-поддерживает-postgresql)
+ [Что такое MVCC и как он реализован в PostgreSQL?](#что-такое-mvcc-и-как-он-реализован-в-postgresql)
+ [Какие виды блокировок существуют в PostgreSQL?](#какие-виды-блокировок-существуют-в-postgresql)
+ [Что такое Advisory Locks и когда их использовать?](#что-такое-advisory-locks-и-когда-их-использовать)
+ [Что такое deadlock и как его избежать?](#что-такое-deadlock-и-как-его-избежать)
+ [Что такое представления (views) и материализованные представления?](#что-такое-представления-views-и-материализованные-представления)
+ [Что такое CTE (Common Table Expressions)?](#что-такое-cte-common-table-expressions)
+ [Как работают оконные функции (window functions)?](#как-работают-оконные-функции-window-functions)
+ [Какие основные оконные функции существуют в PostgreSQL?](#какие-основные-оконные-функции-существуют-в-postgresql)
+ [Что такое триггеры в PostgreSQL?](#что-такое-триггеры-в-postgresql)
+ [Чем отличаются функции от процедур в PostgreSQL?](#чем-отличаются-функции-от-процедур-в-postgresql)
+ [Что такое партиционирование (partitioning) таблиц?](#что-такое-партиционирование-partitioning-таблиц)
+ [Какие виды репликации поддерживает PostgreSQL?](#какие-виды-репликации-поддерживает-postgresql)
+ [Зачем нужны VACUUM и AUTOVACUUM?](#зачем-нужны-vacuum-и-autovacuum)
+ [Как мониторить производительность PostgreSQL?](#как-мониторить-производительность-postgresql)
+ [Как подключить PostgreSQL к Java/Spring приложению?](#как-подключить-postgresql-к-javaspring-приложению)
+ [Как настроить пул соединений HikariCP для PostgreSQL?](#как-настроить-пул-соединений-hikaricp-для-postgresql)

## Что такое PostgreSQL и чем он отличается от других СУБД?
<!-- grade: junior -->

PostgreSQL — это объектно-реляционная система управления базами данных (ОРСУБД) с открытым исходным кодом, одна из наиболее зрелых и функциональных СУБД, активно развивающаяся с 1986 года (проект POSTGRES в Калифорнийском университете Беркли).

> **Аналогия из жизни:** если MySQL — это надёжный семейный автомобиль, то PostgreSQL — это полноприводный внедорожник с прицепом. Он справляется и с простыми поездками, и с бездорожьем (сложные запросы, JSON, геоданные), но требует чуть больше внимания при настройке.

### Ключевые особенности

- Полная поддержка ACID
- Поддержка MVCC (Multi-Version Concurrency Control)
- Расширяемая система типов данных (JSON/JSONB, массивы, hstore, геоданные и др.)
- Мощный оптимизатор запросов
- Поддержка CTE, оконных функций, полнотекстового поиска
- Механизм расширений (extensions): PostGIS, pg_trgm, pgcrypto и др.
- Строгое соответствие стандарту SQL

### Сравнение с MySQL

| Критерий | PostgreSQL | MySQL |
|---|---|---|
| Тип | Объектно-реляционная | Реляционная |
| MVCC | Нативный, на уровне ядра | Зависит от движка (InnoDB) |
| JSON | Полноценный JSONB с индексами | Поддержка JSON (менее мощная) |
| Полнотекстовый поиск | Встроенный | Базовый (FULLTEXT) |
| Репликация | Потоковая и логическая | Встроенная (master-slave) |
| Расширяемость | Высокая (extensions, custom types) | Ограниченная |
| Производительность чтения | Отлично, особенно сложные запросы | Быстрые простые SELECT |

### Сравнение с Oracle

| Критерий | PostgreSQL | Oracle |
|---|---|---|
| Лицензия | Открытая (PostgreSQL License) | Коммерческая (дорогая) |
| Функциональность | Сопоставима для большинства задач | Немного шире (RAC, Data Guard) |
| PL/pgSQL vs PL/SQL | Похожий синтаксис | Более зрелый |
| Партиционирование | Декларативное (с v10) | Развитое |
| Стоимость | Бесплатная | Высокая стоимость лицензий |

В банковской сфере PostgreSQL часто выбирают за надёжность, строгое соответствие ACID и отсутствие лицензионных платежей.

> **На собеседовании:** интервьюер ожидает не перечисление фич, а понимание позиционирования: PostgreSQL строже следует стандарту SQL, лучше работает со сложными запросами и имеет нативный MVCC, тогда как MySQL быстрее на простых SELECT. Упомяните расширяемость (extensions) как уникальную черту PostgreSQL.

[к оглавлению](#postgresql)

---

## Какие основные типы данных поддерживает PostgreSQL?
<!-- grade: junior -->

PostgreSQL обладает одной из самых богатых систем типов данных среди всех СУБД, включая числовые, строковые, временные, а также специализированные типы вроде UUID, JSONB и массивов.

### Числовые типы

| Тип | Размер | Диапазон |
|---|---|---|
| `smallint` | 2 байта | -32768 до +32767 |
| `integer` | 4 байта | -2147483648 до +2147483647 |
| `bigint` | 8 байт | -9223372036854775808 до +9223372036854775807 |
| `decimal/numeric` | переменный | до 131072 цифр до точки, до 16383 после |
| `real` | 4 байта | 6 значащих цифр |
| `double precision` | 8 байт | 15 значащих цифр |

Для финансовых расчётов всегда используют `numeric` — он обеспечивает точные вычисления без ошибок округления, в отличие от `real` и `double precision`.

### Символьные типы

- `char(n)` — строка фиксированной длины, дополняется пробелами
- `varchar(n)` — строка переменной длины с ограничением
- `text` — строка произвольной длины (без ограничения)

В PostgreSQL `varchar` без указания длины и `text` абсолютно идентичны по производительности. Рекомендуется использовать `text`, а ограничения длины делать через `CHECK`-constraints.

### Дата и время

- `date` — только дата (4 байта)
- `time` — только время (8 байт)
- `timestamp` — дата и время без часового пояса
- `timestamptz` (`timestamp with time zone`) — дата и время с часовым поясом
- `interval` — интервал времени

```sql
-- Рекомендуется всегда использовать timestamptz
CREATE TABLE transactions (
    id bigserial PRIMARY KEY,
    amount numeric(15, 2) NOT NULL,
    created_at timestamptz DEFAULT now()
);
```

### Прочие типы

- `boolean` — `true`, `false`, `null`
- `uuid` — 128-битный универсальный уникальный идентификатор; для генерации используется `gen_random_uuid()` (с PostgreSQL 13) или расширение `uuid-ossp`
- `json` / `jsonb` — хранение JSON-данных (текстовое и бинарное представление)
- Массивы — любой тип данных может быть массивом: `integer[]`, `text[]`
- Serial типы — `serial`, `bigserial`, `smallserial` (автоинкрементные псевдотипы)

Начиная с PostgreSQL 10 рекомендуется использовать стандартный `GENERATED ALWAYS AS IDENTITY` вместо `serial`.

```sql
CREATE TABLE clients (
    id bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name text NOT NULL
);
```

> **На собеседовании:** ключевые моменты — `numeric` для денег (не `double`), `timestamptz` вместо `timestamp`, `text` вместо `varchar`, и `GENERATED ALWAYS AS IDENTITY` вместо `serial`. Это показывает знание лучших практик, а не просто перечисление типов.

[к оглавлению](#postgresql)

---

## Что такое схемы (schemas) в PostgreSQL?
<!-- grade: junior -->

Схема (schema) — это логическое пространство имён внутри базы данных, позволяющее группировать и изолировать объекты (таблицы, представления, функции, типы).

### Ключевые особенности

- Каждая база данных содержит как минимум схему `public`, которая используется по умолчанию
- Разные схемы могут содержать объекты с одинаковыми именами без конфликтов
- Схемы используются для логического разделения модулей приложения, управления доступом и мультитенантности

<details><summary>Примеры работы со схемами</summary>

```sql
-- Создание схемы
CREATE SCHEMA payments;

-- Создание таблицы в схеме
CREATE TABLE payments.orders (
    id bigserial PRIMARY KEY,
    amount numeric(15, 2),
    status text DEFAULT 'NEW'
);

-- Обращение к таблице
SELECT * FROM payments.orders;

-- Установка пути поиска схем
SET search_path TO payments, public;
-- Теперь можно без указания схемы:
SELECT * FROM orders;
```

</details>

Параметр `search_path` определяет порядок, в котором PostgreSQL ищет объекты по неквалифицированному имени. По умолчанию: `"$user", public`.

### Применение для разделения модулей

```sql
CREATE SCHEMA core;       -- основные сущности (клиенты, счета)
CREATE SCHEMA payments;   -- платежи и переводы
CREATE SCHEMA audit;      -- аудит и журналирование
CREATE SCHEMA reports;    -- отчётные представления

-- Можно ограничить права:
GRANT USAGE ON SCHEMA audit TO audit_role;
REVOKE ALL ON SCHEMA core FROM public;
```

> **На собеседовании:** схемы часто путают с отдельными базами данных. Важно пояснить, что схема — это пространство имён внутри одной БД, и между схемами можно делать JOIN, а между разными базами данных — нельзя (без расширения `dblink`/`postgres_fdw`).

[к оглавлению](#postgresql)

---

## Что такое последовательности (sequences) и как они работают?
<!-- grade: junior -->

Последовательность (sequence) — это специальный объект базы данных, генерирующий уникальные числовые значения в возрастающем (или убывающем) порядке. Последовательности часто используются для генерации первичных ключей.

<details><summary>Примеры работы с последовательностями</summary>

```sql
-- Создание последовательности
CREATE SEQUENCE order_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 20;

-- Получение следующего значения
SELECT nextval('order_id_seq');  -- 1
SELECT nextval('order_id_seq');  -- 2

-- Текущее значение (в текущей сессии, после вызова nextval)
SELECT currval('order_id_seq');  -- 2

-- Последнее присвоенное значение (глобально)
SELECT last_value FROM order_id_seq;

-- Использование в таблице
CREATE TABLE orders (
    id bigint DEFAULT nextval('order_id_seq') PRIMARY KEY,
    description text
);
```

</details>

### Важные особенности

- Последовательности работают **вне транзакций** — если транзакция откатывается, значение последовательности **не возвращается**. Поэтому в значениях могут быть пропуски (gaps), и это нормальное поведение
- Параметр `CACHE` позволяет каждой сессии заранее резервировать блок значений, что повышает производительность при высоком конкурентном доступе
- Последовательности гарантируют уникальность, но не гарантируют непрерывность

```sql
-- Сброс последовательности
ALTER SEQUENCE order_id_seq RESTART WITH 1;

-- Привязка последовательности к столбцу
ALTER SEQUENCE order_id_seq OWNED BY orders.id;
```

> **На собеседовании:** типичный вопрос-ловушка — «почему в таблице пропуски в id?». Ответ: последовательности работают вне транзакций, откат транзакции не возвращает значение. Это by design, а не баг.

[к оглавлению](#postgresql)

---

## Чем отличаются типы serial и bigserial от явного использования последовательностей?
<!-- grade: junior -->

`serial` и `bigserial` — это псевдотипы (не настоящие типы данных), которые являются сокращённой записью для создания столбца с автоинкрементом. За кулисами PostgreSQL создаёт последовательность и привязывает её к столбцу.

Запись:
```sql
CREATE TABLE users (
    id serial PRIMARY KEY
);
```

Эквивалентна:
```sql
CREATE SEQUENCE users_id_seq;
CREATE TABLE users (
    id integer NOT NULL DEFAULT nextval('users_id_seq')
);
ALTER SEQUENCE users_id_seq OWNED BY users.id;
```

### Различия serial, bigserial, smallserial

| Псевдотип | Реальный тип | Размер | Макс. значение |
|---|---|---|---|
| `smallserial` | `smallint` | 2 байта | 32 767 |
| `serial` | `integer` | 4 байта | 2 147 483 647 |
| `bigserial` | `bigint` | 8 байт | 9 223 372 036 854 775 807 |

### Рекомендация (PostgreSQL 10+)

Использовать `GENERATED ALWAYS AS IDENTITY` вместо `serial`, потому что:
- это стандарт SQL
- запрещает явную вставку значения без `OVERRIDING SYSTEM VALUE`
- последовательность автоматически привязана к столбцу

```sql
-- Современный подход
CREATE TABLE users (
    id bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    email text NOT NULL
);

-- Если нужно вставить конкретное значение (миграция данных):
INSERT INTO users (id, email)
OVERRIDING SYSTEM VALUE
VALUES (999, 'admin@bank.ru');
```

> **На собеседовании:** покажите знание эволюции подходов: `serial` -> `GENERATED ALWAYS AS IDENTITY`. Объясните, что `serial` — это PostgreSQL-специфичный псевдотип, а `IDENTITY` — стандарт SQL, который вдобавок защищает от случайной вставки произвольного значения.

[к оглавлению](#postgresql)

---

## Что такое JSONB в PostgreSQL и чем он отличается от JSON?
<!-- grade: middle -->

PostgreSQL поддерживает два типа для хранения JSON-данных: `json` (текстовое хранение «как есть») и `jsonb` (бинарное декомпозированное хранение с индексированием). В подавляющем большинстве случаев следует использовать `jsonb`.

### JSON

- Хранит данные как текстовую строку
- Сохраняет форматирование, порядок ключей, дубликаты ключей
- При каждом обращении данные парсятся заново
- Нельзя индексировать

### JSONB

- Хранит данные в бинарном декомпозированном формате
- Не сохраняет порядок ключей и пробелы, убирает дубликаты ключей
- Не требует повторного парсинга — быстрее чтение
- Поддерживает индексирование (GIN)
- Поддерживает операторы проверки вхождения (`@>`, `<@`, `?`, `?|`, `?&`)

```sql
CREATE TABLE client_settings (
    client_id bigint PRIMARY KEY,
    settings jsonb NOT NULL DEFAULT '{}'
);

INSERT INTO client_settings VALUES
(1, '{"theme": "dark", "notifications": {"email": true, "sms": false}, "limits": [1000, 5000, 10000]}');
```

### Когда использовать JSONB

- Для хранения полуструктурированных данных с изменяемой схемой
- Для данных, приходящих из внешних API (ответы от платёжных систем, настройки)
- Для хранения расширяемых атрибутов (EAV-паттерн)
- Для метаданных и конфигураций

### Когда НЕ использовать JSONB

- Если структура данных стабильна и известна — лучше обычные столбцы
- Если нужны JOIN, FOREIGN KEY, агрегации по полям — реляционная модель эффективнее
- Если данные регулярно обновляются по отдельным ключам — UPDATE всего JSONB-поля дороже, чем обновление столбца

> **На собеседовании:** частая ошибка — сказать «JSONB быстрее JSON». Уточните: JSONB быстрее при чтении и поиске, но медленнее при записи (из-за парсинга в бинарный формат). Главное преимущество — возможность индексирования (GIN) и операторов проверки вхождения.

[к оглавлению](#postgresql)

---

## Какие операторы существуют для работы с JSONB?
<!-- grade: middle -->

PostgreSQL предоставляет три группы операторов для JSONB: извлечение данных, проверка вхождения и модификация. Это позволяет работать с JSON-данными без извлечения их в приложение.

### Операторы извлечения данных

| Оператор | Описание | Пример | Результат |
|---|---|---|---|
| `->` | Получить элемент по ключу (JSON) | `'{"a":1}'::jsonb -> 'a'` | `1` (jsonb) |
| `->>` | Получить элемент по ключу (текст) | `'{"a":1}'::jsonb ->> 'a'` | `1` (text) |
| `#>` | Получить по пути (JSON) | `'{"a":{"b":2}}'::jsonb #> '{a,b}'` | `2` (jsonb) |
| `#>>` | Получить по пути (текст) | `'{"a":{"b":2}}'::jsonb #>> '{a,b}'` | `2` (text) |

### Операторы проверки

| Оператор | Описание | Пример |
|---|---|---|
| `@>` | Левое содержит правое | `'{"a":1,"b":2}'::jsonb @> '{"a":1}'` |
| `<@` | Левое содержится в правом | `'{"a":1}'::jsonb <@ '{"a":1,"b":2}'` |
| `?` | Содержит ключ | `'{"a":1}'::jsonb ? 'a'` |
| `?|` | Содержит хотя бы один ключ | `'{"a":1}'::jsonb ?| array['a','b']` |
| `?&` | Содержит все ключи | `'{"a":1,"b":2}'::jsonb ?& array['a','b']` |

### Операторы модификации

| Оператор | Описание | Пример |
|---|---|---|
| `||` | Конкатенация | `'{"a":1}'::jsonb || '{"b":2}'::jsonb` |
| `-` | Удаление ключа | `'{"a":1,"b":2}'::jsonb - 'a'` |
| `#-` | Удаление по пути | `'{"a":{"b":1}}'::jsonb #- '{a,b}'` |

<details><summary>Практические примеры</summary>

```sql
-- Найти клиентов с определённой настройкой
SELECT client_id
FROM client_settings
WHERE settings @> '{"theme": "dark"}';

-- Получить значение вложенного ключа
SELECT settings -> 'notifications' ->> 'email' AS email_enabled
FROM client_settings
WHERE client_id = 1;

-- Обновить отдельный ключ через jsonb_set
UPDATE client_settings
SET settings = jsonb_set(settings, '{theme}', '"light"')
WHERE client_id = 1;

-- Добавить новый ключ
UPDATE client_settings
SET settings = settings || '{"language": "ru"}'::jsonb
WHERE client_id = 1;

-- Полезные функции
SELECT jsonb_pretty(settings) FROM client_settings;     -- красивый вывод
SELECT jsonb_each(settings) FROM client_settings;       -- разбор на key-value
SELECT jsonb_object_keys(settings) FROM client_settings; -- только ключи
SELECT jsonb_array_elements('[1,2,3]'::jsonb);           -- разбор массива
```

</details>

> **На собеседовании:** ключевое различие `->` и `->>`: первый возвращает jsonb (для дальнейшей навигации), второй — text (для сравнений и вывода). Для индексированного поиска используйте `@>` с GIN-индексом, а не `->>`с B-tree.

[к оглавлению](#postgresql)

---

## Как индексировать JSONB-поля?
<!-- grade: middle -->

Для эффективного поиска по JSONB существует три основных подхода: GIN-индекс на всё поле, GIN с классом `jsonb_path_ops` и B-tree на конкретное выражение. Выбор зависит от паттерна запросов.

### GIN-индекс на всё поле (jsonb_ops)

```sql
CREATE INDEX idx_settings_gin ON client_settings USING GIN (settings);
```

Поддерживает операторы: `@>`, `?`, `?|`, `?&`. Это наиболее универсальный вариант.

### GIN-индекс с классом jsonb_path_ops

```sql
CREATE INDEX idx_settings_path ON client_settings USING GIN (settings jsonb_path_ops);
```

Поддерживает **только** оператор `@>`, но индекс компактнее и быстрее, чем `jsonb_ops`.

### B-tree индекс на конкретное выражение

```sql
-- Если часто ищем по конкретному ключу
CREATE INDEX idx_settings_theme ON client_settings ((settings ->> 'theme'));

-- Использование:
SELECT * FROM client_settings WHERE settings ->> 'theme' = 'dark';
```

### Сравнение подходов

| Подход | Размер | Операторы | Когда использовать |
|---|---|---|---|
| GIN `jsonb_ops` | Большой | `@>`, `?`, `?|`, `?&` | Поиск по произвольным ключам |
| GIN `jsonb_path_ops` | Меньше | Только `@>` | Поиск по вхождению |
| B-tree на выражении | Маленький | `=`, `<`, `>`, `LIKE` | Поиск по конкретному ключу |

> **На собеседовании:** покажите понимание компромиссов. GIN `jsonb_path_ops` — лучший выбор, если нужен только `@>` (самый частый случай). B-tree на выражении — если поиск всегда по одному и тому же ключу. Не забудьте упомянуть, что `->>`-выражение в WHERE должно совпадать с выражением в индексе.

[к оглавлению](#postgresql)

---

## Как работать с массивами в PostgreSQL?
<!-- grade: middle -->

PostgreSQL позволяет хранить массивы значений любого типа в одном поле. Массивы удобны для небольших списков тегов и меток, которые читаются целиком, но для сложных связей лучше использовать отдельную таблицу.

```sql
CREATE TABLE products (
    id serial PRIMARY KEY,
    name text,
    tags text[],
    prices numeric[]
);

INSERT INTO products (name, tags, prices) VALUES
('Кредит наличными', ARRAY['кредит', 'физлица', 'наличные'], ARRAY[100000, 500000, 1000000]),
('Вклад "Надёжный"', '{вклад,физлица,депозит}', '{50000,100000}');
```

<details><summary>Операции с массивами</summary>

```sql
-- Доступ к элементу (индексация с 1!)
SELECT tags[1] FROM products;  -- 'кредит'

-- Срез массива
SELECT tags[1:2] FROM products;  -- {'кредит','физлица'}

-- Проверка вхождения элемента
SELECT * FROM products WHERE 'кредит' = ANY(tags);
SELECT * FROM products WHERE tags @> ARRAY['кредит'];

-- Проверка пересечения массивов
SELECT * FROM products WHERE tags && ARRAY['вклад', 'кредит'];  -- overlap

-- Конкатенация
SELECT tags || ARRAY['новый_тег'] FROM products;
SELECT array_append(tags, 'новый_тег') FROM products;

-- Удаление элемента
SELECT array_remove(tags, 'физлица') FROM products;

-- Длина массива
SELECT array_length(tags, 1) FROM products;

-- Разворачивание массива в строки
SELECT unnest(tags) AS tag FROM products;

-- Агрегация в массив
SELECT array_agg(name) FROM products WHERE 'физлица' = ANY(tags);
```

</details>

### Индексирование массивов

```sql
-- GIN-индекс для поиска по массивам
CREATE INDEX idx_products_tags ON products USING GIN (tags);

-- После создания индекса эффективно работают: @>, &&, <@
SELECT * FROM products WHERE tags @> ARRAY['кредит', 'физлица'];
```

### Когда массивы, а когда отдельная таблица

- **Массивы** — для небольших списков тегов/меток, которые в основном читаются целиком
- **Отдельная таблица (many-to-many)** — если нужны JOIN, агрегации, FOREIGN KEY, или если массивы большие

> **На собеседовании:** индексация массивов в PostgreSQL начинается с 1, а не с 0 — классический вопрос-ловушка. Также стоит упомянуть, что для поиска по массивам нужен GIN-индекс, а не B-tree.

[к оглавлению](#postgresql)

---

## Какие типы индексов поддерживает PostgreSQL?
<!-- grade: middle -->

PostgreSQL поддерживает шесть типов индексов, каждый из которых оптимизирован для определённых сценариев. B-tree используется по умолчанию и покрывает большинство задач, но для JSON, массивов, геоданных и временных рядов нужны специализированные типы.

### B-tree (по умолчанию)

Сбалансированное дерево поиска. Используется для операций сравнения: `=`, `<`, `>`, `<=`, `>=`, `BETWEEN`, `IN`, `IS NULL`, а также для `LIKE 'prefix%'`.

```sql
CREATE INDEX idx_users_email ON users (email);
```

### Hash

Хеш-таблица. Оптимизирован только для операции равенства (`=`). Занимает меньше места, чем B-tree. С PostgreSQL 10 хеш-индексы WAL-безопасны (реплицируются).

```sql
CREATE INDEX idx_users_uuid ON users USING hash (external_id);
```

### GIN (Generalized Inverted Index)

Обратный (инвертированный) индекс. Используется для составных значений: массивов, JSONB, полнотекстового поиска (`tsvector`), `hstore`.

```sql
CREATE INDEX idx_doc_content ON documents USING GIN (to_tsvector('russian', content));
CREATE INDEX idx_settings ON client_settings USING GIN (settings);
```

### GiST (Generalized Search Tree)

Обобщённое дерево поиска. Используется для геоданных (PostGIS), диапазонных типов (`int4range`, `tsrange`), полнотекстового поиска, нечёткого поиска (`pg_trgm`).

```sql
CREATE INDEX idx_location ON branches USING GiST (coordinates);
CREATE INDEX idx_period ON contracts USING GiST (valid_period);
```

### BRIN (Block Range Index)

Хранит мин/макс значения для диапазонов физических блоков. Очень компактный. Эффективен для больших таблиц, где данные физически коррелируют с порядком индексируемого столбца (например, временные метки при последовательной вставке).

```sql
-- Идеально для таблицы логов, где данные вставляются хронологически
CREATE INDEX idx_logs_created ON audit_logs USING BRIN (created_at);
```

### SP-GiST (Space-Partitioned GiST)

Для данных с естественным кластерным разбиением: IP-адреса, телефонные номера, геоданные.

```sql
CREATE INDEX idx_ip ON connections USING SPGIST (ip_address inet_ops);
```

> **На собеседовании:** не нужно знать все шесть наизусть — достаточно B-tree, GIN и BRIN. Покажите понимание: B-tree для сравнений, GIN для «содержит» (массивы, JSON, полнотекст), BRIN для огромных таблиц с хронологическими данными. Остальные назовите по сценарию применения.

[к оглавлению](#postgresql)

---

## Когда какой тип индекса использовать?
<!-- grade: middle -->

Выбор типа индекса зависит от паттерна запросов: какой оператор используется в WHERE и какова природа данных.

| Задача | Рекомендуемый индекс |
|---|---|
| Поиск по равенству, диапазону, сортировка | **B-tree** |
| Только поиск по равенству | **Hash** |
| Поиск по массивам, JSONB, полнотекстовый поиск | **GIN** |
| Геоданные, диапазонные типы, нечёткий поиск | **GiST** |
| Большие таблицы с коррелированными данными (логи, временные ряды) | **BRIN** |
| IP-адреса, иерархические данные | **SP-GiST** |

<details><summary>Примеры для типового приложения</summary>

```sql
-- B-tree: поиск транзакций по номеру счёта
CREATE INDEX idx_tx_account ON transactions (account_id);

-- B-tree: сортировка по дате
CREATE INDEX idx_tx_date ON transactions (created_at DESC);

-- GIN: поиск по JSONB-метаданным платежа
CREATE INDEX idx_tx_meta ON transactions USING GIN (metadata jsonb_path_ops);

-- BRIN: огромная таблица аудит-логов (миллиарды записей)
CREATE INDEX idx_audit_ts ON audit_log USING BRIN (event_timestamp)
    WITH (pages_per_range = 32);

-- GiST: поиск банкоматов по геолокации
CREATE INDEX idx_atm_location ON atms USING GiST (location);

-- GIN + pg_trgm: нечёткий поиск по имени клиента
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE INDEX idx_client_name_trgm ON clients USING GIN (full_name gin_trgm_ops);
SELECT * FROM clients WHERE full_name ILIKE '%иванов%';
```

</details>

> **На собеседовании:** покажите практический подход — не абстрактное знание типов, а привязку к реальным задачам. BRIN для логов экономит место в сотни раз по сравнению с B-tree, GIN + pg_trgm решает задачу поиска «содержит подстроку» (ILIKE '%...%'), которую B-tree не может.

[к оглавлению](#postgresql)

---

## Что такое составные индексы и как они работают?
<!-- grade: middle -->

Составной (композитный) индекс — индекс, построенный по нескольким столбцам. Он эффективен, когда запросы фильтруют по комбинации столбцов, но работает по правилу левого префикса.

```sql
CREATE INDEX idx_tx_account_date ON transactions (account_id, created_at);
```

### Правило левого префикса

Составной B-tree индекс эффективно используется, если в условии запроса участвуют столбцы, начиная с первого (левого) столбца индекса.

```sql
-- Индекс: (account_id, created_at)

-- Использует индекс (оба столбца):
SELECT * FROM transactions
WHERE account_id = 100 AND created_at > '2024-01-01';

-- Использует индекс (только первый столбец):
SELECT * FROM transactions
WHERE account_id = 100;

-- НЕ использует индекс (нет первого столбца):
SELECT * FROM transactions
WHERE created_at > '2024-01-01';
```

### Порядок столбцов

- Столбцы с высокой селективностью (больше уникальных значений) ставятся первыми
- Столбцы для фильтрации по равенству — перед столбцами для диапазонного поиска

```sql
-- Оптимальный порядок для запроса:
-- WHERE status = 'ACTIVE' AND created_at BETWEEN ... AND ...
CREATE INDEX idx_tx_status_date ON transactions (status, created_at);
```

### Покрывающий индекс (Index Only Scan)

```sql
-- Если запрос выбирает только столбцы из индекса,
-- PostgreSQL может ответить, не обращаясь к таблице
CREATE INDEX idx_covering ON transactions (account_id, created_at, amount);

-- Index Only Scan:
SELECT created_at, amount FROM transactions WHERE account_id = 100;
```

Начиная с PostgreSQL 11 можно использовать `INCLUDE` для добавления столбцов в индекс без влияния на поисковые ключи:

```sql
CREATE INDEX idx_tx_cover ON transactions (account_id) INCLUDE (amount, status);
```

> **На собеседовании:** правило левого префикса — обязательный вопрос. Объясните его через аналогию с телефонной книгой: можно найти «Иванов, Пётр», можно найти всех «Ивановых», но нельзя найти всех «Петров» — для этого нужен отдельный индекс.

[к оглавлению](#postgresql)

---

## Что такое частичные индексы и индексы на выражениях?
<!-- grade: middle -->

Частичный индекс (partial index) строится только для части строк таблицы, а индекс на выражении — на результате функции. Оба подхода позволяют создавать компактные и целевые индексы вместо тяжёлых полных.

### Частичный индекс

```sql
-- Индекс только по активным транзакциям
CREATE INDEX idx_tx_active ON transactions (account_id)
WHERE status = 'PENDING';

-- Значительно меньше обычного индекса, быстрее обновляется
-- Используется только если в запросе совпадает условие:
SELECT * FROM transactions
WHERE account_id = 100 AND status = 'PENDING';  -- использует idx_tx_active
```

<details><summary>Типичные применения частичных индексов</summary>

```sql
-- Индекс только по необработанным записям
CREATE INDEX idx_queue_unprocessed ON task_queue (created_at)
WHERE processed = false;

-- Уникальность только для активных записей
CREATE UNIQUE INDEX idx_unique_active_email ON users (email)
WHERE is_deleted = false;

-- Индекс исключая NULL
CREATE INDEX idx_tx_reference ON transactions (reference_number)
WHERE reference_number IS NOT NULL;
```

</details>

### Индекс на выражении (expression index)

Индекс, построенный на результате функции или выражения, а не на значении столбца напрямую.

```sql
-- Индекс для регистронезависимого поиска
CREATE INDEX idx_users_email_lower ON users (lower(email));
SELECT * FROM users WHERE lower(email) = 'user@bank.ru';

-- Индекс на дату из timestamp
CREATE INDEX idx_tx_date ON transactions (date(created_at));
SELECT * FROM transactions WHERE date(created_at) = '2024-06-15';

-- Индекс на извлечённое значение JSONB
CREATE INDEX idx_meta_type ON transactions ((metadata ->> 'type'));
SELECT * FROM transactions WHERE metadata ->> 'type' = 'transfer';
```

Запрос должен содержать **точно такое же выражение**, которое использовалось при создании индекса, иначе оптимизатор не сможет его применить.

> **На собеседовании:** частичные индексы — это «серебряная пуля» для таблиц-очередей, где 99% строк обработаны и только 1% актуальны. Индекс на 1% строк вместо 100% — разница в размере и скорости обновления на порядки.

[к оглавлению](#postgresql)

---

## Как использовать EXPLAIN и EXPLAIN ANALYZE для анализа запросов?
<!-- grade: middle -->

`EXPLAIN` показывает план выполнения запроса — как PostgreSQL собирается его выполнять. `EXPLAIN ANALYZE` фактически выполняет запрос и показывает реальное время и количество строк. Это главный инструмент диагностики медленных запросов.

```sql
-- Только план (запрос НЕ выполняется)
EXPLAIN
SELECT * FROM transactions WHERE account_id = 100;

-- План + реальное выполнение
EXPLAIN ANALYZE
SELECT * FROM transactions WHERE account_id = 100;

-- Расширенный вывод с буферами
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM transactions WHERE account_id = 100;
```

### Пример вывода EXPLAIN ANALYZE

```
Index Scan using idx_tx_account on transactions  (cost=0.43..8.45 rows=1 width=64) (actual time=0.021..0.023 rows=1 loops=1)
  Index Cond: (account_id = 100)
  Buffers: shared hit=3
Planning Time: 0.085 ms
Execution Time: 0.045 ms
```

### Ключевые метрики

- `cost=0.43..8.45` — оценка стоимости (начало..итого) в условных единицах
- `rows=1` — ожидаемое количество строк
- `actual time=0.021..0.023` — реальное время (мс) первой строки..последней строки
- `rows=1` (actual) — реальное количество строк
- `loops=1` — сколько раз узел выполнялся
- `Buffers: shared hit=3` — прочитано 3 страницы из кеша

### Осторожно с модифицирующими запросами

`EXPLAIN ANALYZE` для `UPDATE`, `DELETE`, `INSERT` **выполняет** операцию! Оборачивайте в транзакцию:

```sql
BEGIN;
EXPLAIN ANALYZE UPDATE transactions SET status = 'DONE' WHERE id = 1;
ROLLBACK;
```

> **На собеседовании:** обязательно упомяните, что EXPLAIN ANALYZE реально выполняет запрос. Для DML-запросов — всегда BEGIN/ROLLBACK. Расхождение между estimated и actual rows — признак устаревшей статистики, решается через `ANALYZE table_name`.

[к оглавлению](#postgresql)

---

## Как читать план выполнения запроса?
<!-- grade: middle -->

План выполнения — это дерево узлов (nodes). Выполнение идёт от внутренних узлов к внешним (снизу вверх). Чтение плана — ключевой навык для оптимизации SQL.

### Основные типы узлов сканирования

| Узел | Описание |
|---|---|
| `Seq Scan` | Последовательное чтение всей таблицы |
| `Index Scan` | Чтение через индекс, затем обращение к таблице за данными |
| `Index Only Scan` | Все нужные данные уже есть в индексе |
| `Bitmap Index Scan` + `Bitmap Heap Scan` | Двухфазное: сначала собирается битовая карта по индексу, затем читаются страницы |

### Типы соединений (JOIN)

| Узел | Описание |
|---|---|
| `Nested Loop` | Для каждой строки внешнего набора ищем во внутреннем. Хорош при малом внешнем наборе |
| `Hash Join` | Строится хеш-таблица по одной стороне. Эффективен для больших наборов |
| `Merge Join` | Оба набора сортируются, затем сливаются. Эффективен, если данные уже отсортированы |

<details><summary>Пример чтения сложного плана</summary>

```sql
EXPLAIN ANALYZE
SELECT t.id, t.amount, c.name
FROM transactions t
JOIN clients c ON t.client_id = c.id
WHERE t.created_at > '2024-01-01'
  AND t.amount > 50000;
```

```
Hash Join  (cost=30.00..150.00 rows=100 width=48) (actual time=0.5..2.1 rows=87 loops=1)
  Hash Cond: (t.client_id = c.id)
  ->  Bitmap Heap Scan on transactions t  (cost=10.00..120.00 rows=100 width=24) (actual time=0.3..1.5 rows=87 loops=1)
        Recheck Cond: (created_at > '2024-01-01')
        Filter: (amount > 50000)
        Rows Removed by Filter: 213
        ->  Bitmap Index Scan on idx_tx_date  (cost=0.00..9.97 rows=300 width=0) (actual time=0.2..0.2 rows=300 loops=1)
              Index Cond: (created_at > '2024-01-01')
  ->  Hash  (cost=15.00..15.00 rows=500 width=32) (actual time=0.1..0.1 rows=500 loops=1)
        Buckets: 1024  Batches: 1  Memory Usage: 40kB
        ->  Seq Scan on clients c  (cost=0.00..15.00 rows=500 width=32) (actual time=0.01..0.05 rows=500 loops=1)
Planning Time: 0.2 ms
Execution Time: 2.3 ms
```

</details>

### На что обращать внимание

- **Большое расхождение между estimated и actual rows** — устаревшая статистика, нужен `ANALYZE`
- **Seq Scan на большой таблице с фильтром** — возможно, не хватает индекса
- **Rows Removed by Filter** — строки прочитаны, но отброшены; признак неоптимального индекса
- **Buffers: shared read** — чтение с диска (медленно), **shared hit** — чтение из кеша

> **На собеседовании:** не нужно помнить все типы узлов. Покажите, что умеете читать план: снизу вверх, сравнивать estimated vs actual rows, искать Seq Scan на больших таблицах и «Rows Removed by Filter». Это три красных флага, которые решают 80% проблем производительности.

[к оглавлению](#postgresql)

---

## Какие основные приёмы оптимизации запросов в PostgreSQL?
<!-- grade: middle -->

Оптимизация запросов — это системный процесс: сначала EXPLAIN ANALYZE для диагностики, затем точечные изменения. Восемь основных приёмов покрывают подавляющее большинство ситуаций.

### 1. Создание подходящих индексов

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 42 AND status = 'active';
-- Если Seq Scan — создать индекс:
CREATE INDEX idx_orders_cust_status ON orders (customer_id, status);
```

### 2. Актуальность статистики

```sql
ANALYZE transactions;
-- Увеличить детализацию статистики для столбца
ALTER TABLE transactions ALTER COLUMN status SET STATISTICS 1000;
ANALYZE transactions;
```

### 3. Выбирать только нужные столбцы

```sql
-- Плохо:
SELECT * FROM transactions WHERE account_id = 100;
-- Хорошо (может использовать Index Only Scan):
SELECT id, amount, created_at FROM transactions WHERE account_id = 100;
```

### 4. Keyset pagination вместо OFFSET

```sql
SELECT * FROM transactions
WHERE account_id = 100 AND id > 1000
ORDER BY id
LIMIT 20;
```

### 5. Избегать функций на индексированных столбцах

```sql
-- Плохо (индекс не используется):
SELECT * FROM users WHERE UPPER(email) = 'USER@BANK.RU';
-- Хорошо (создать expression index):
CREATE INDEX idx_email_upper ON users (UPPER(email));
```

### 6. EXISTS вместо IN для подзапросов

```sql
-- Менее эффективно для больших подзапросов:
SELECT * FROM clients WHERE id IN (SELECT client_id FROM transactions WHERE amount > 1000000);
-- Более эффективно:
SELECT * FROM clients c WHERE EXISTS (
    SELECT 1 FROM transactions t WHERE t.client_id = c.id AND t.amount > 1000000
);
```

### 7. Параллельное выполнение запросов

```sql
SET max_parallel_workers_per_gather = 4;
-- В EXPLAIN видно: Gather -> Parallel Seq Scan
```

### 8. Материализованные представления для аналитики

```sql
CREATE MATERIALIZED VIEW mv_daily_totals AS
SELECT date(created_at) AS day, sum(amount) AS total
FROM transactions GROUP BY date(created_at);

REFRESH MATERIALIZED VIEW CONCURRENTLY mv_daily_totals;
```

> **На собеседовании:** не перечисляйте все приёмы списком — расскажите алгоритм: «сначала EXPLAIN ANALYZE, смотрю Seq Scan и Rows Removed, затем добавляю индекс или переписываю запрос». Это показывает системный подход, а не заученный список.

[к оглавлению](#postgresql)

---

## Как работают транзакции в PostgreSQL?
<!-- grade: junior -->

Транзакция — это последовательность операций, выполняемых как единое целое и удовлетворяющих свойствам ACID. В PostgreSQL каждый отдельный оператор — это неявная транзакция (autocommit), а для объединения нескольких операций используется `BEGIN ... COMMIT/ROLLBACK`.

### Свойства ACID

- **Atomicity (атомарность)** — транзакция либо выполняется полностью, либо не выполняется совсем
- **Consistency (согласованность)** — транзакция переводит БД из одного согласованного состояния в другое
- **Isolation (изолированность)** — параллельные транзакции не влияют друг на друга
- **Durability (долговечность)** — результат зафиксированной транзакции сохраняется даже при сбое

<details><summary>Примеры работы с транзакциями</summary>

```sql
-- Явная транзакция
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT;

-- Откат
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
-- Что-то пошло не так
ROLLBACK;

-- Точки сохранения (savepoints)
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
SAVEPOINT sp1;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
-- Ошибка — откатываем до savepoint
ROLLBACK TO sp1;
-- Продолжаем с другой логикой
UPDATE accounts SET balance = balance + 1000 WHERE id = 3;
COMMIT;
```

</details>

### Использование из Java/Spring

```java
@Transactional
public void transfer(Long fromId, Long toId, BigDecimal amount) {
    accountRepository.debit(fromId, amount);
    accountRepository.credit(toId, amount);
    // при исключении — автоматический ROLLBACK
}
```

> **На собеседовании:** не забудьте упомянуть autocommit: каждый отдельный SELECT/INSERT — это неявная транзакция. SAVEPOINT — продвинутый ответ, показывающий глубину знаний: позволяет откатить часть транзакции без полного ROLLBACK.

[к оглавлению](#postgresql)

---

## Какие уровни изоляции транзакций поддерживает PostgreSQL?
<!-- grade: middle -->

PostgreSQL поддерживает четыре уровня изоляции в соответствии со стандартом SQL, но реализует их через MVCC — без блокировок для чтения. Фактически уникальных поведений три, поскольку READ UNCOMMITTED работает идентично READ COMMITTED.

| Уровень | Грязное чтение | Неповторяемое чтение | Фантомное чтение | Аномалия сериализации |
|---|---|---|---|---|
| `READ UNCOMMITTED` | Невозможно* | Возможно | Возможно | Возможно |
| `READ COMMITTED` | Невозможно | Возможно | Возможно | Возможно |
| `REPEATABLE READ` | Невозможно | Невозможно | Невозможно** | Возможно |
| `SERIALIZABLE` | Невозможно | Невозможно | Невозможно | Невозможно |

(*) В PostgreSQL `READ UNCOMMITTED` работает идентично `READ COMMITTED` — грязное чтение невозможно благодаря MVCC.

(**) В PostgreSQL на уровне `REPEATABLE READ` фантомное чтение также невозможно (в отличие от стандарта SQL, где оно допустимо).

### READ COMMITTED (по умолчанию)

Каждый оператор видит данные, зафиксированные до начала этого оператора. Два одинаковых SELECT в одной транзакции могут вернуть разные результаты.

### REPEATABLE READ

Транзакция видит данные, зафиксированные до начала транзакции. При попытке обновить строку, изменённую другой транзакцией, выбрасывается ошибка сериализации — нужно повторить транзакцию.

### SERIALIZABLE

Гарантирует, что результат параллельного выполнения транзакций эквивалентен некоторому последовательному выполнению. Использует механизм Serializable Snapshot Isolation (SSI). При конфликте одна из транзакций откатывается — приложение должно быть готово к повторной попытке.

```java
// Spring — повторная попытка при ошибке сериализации
@Transactional(isolation = Isolation.SERIALIZABLE)
@Retryable(value = CannotSerializeTransactionException.class, maxAttempts = 3)
public void criticalOperation() {
    // ...
}
```

> **На собеседовании:** ключевой ответ про PostgreSQL — грязное чтение невозможно даже на READ UNCOMMITTED (благодаря MVCC), а фантомное чтение невозможно уже на REPEATABLE READ (строже стандарта SQL). Обязательно упомяните, что при SERIALIZABLE приложение должно уметь повторять транзакции.

[к оглавлению](#postgresql)

---

## Что такое MVCC и как он реализован в PostgreSQL?
<!-- grade: middle -->

MVCC (Multi-Version Concurrency Control) — механизм управления конкурентным доступом, при котором каждая транзакция видит свой «снимок» (snapshot) данных. Главный принцип: читатели не блокируют писателей, а писатели не блокируют читателей.

> **Аналогия из жизни:** MVCC — это как Google Docs с историей версий. Каждый пользователь видит свою версию документа на момент открытия, а изменения других пользователей появляются только после «обновления страницы» (нового оператора или транзакции).

### Реализация в PostgreSQL

Каждая строка (tuple) содержит скрытые системные столбцы:

- `xmin` — ID транзакции, которая создала эту версию строки
- `xmax` — ID транзакции, которая удалила (или обновила) эту версию (0, если строка «живая»)
- `ctid` — физический адрес строки на странице

### Что происходит при UPDATE

1. Текущая версия строки **не изменяется** — у неё устанавливается `xmax` равным ID текущей транзакции
2. Создаётся **новая версия** строки с изменёнными данными, `xmin` = ID текущей транзакции
3. Обе версии физически присутствуют в таблице

```sql
-- Наглядно: смотрим системные столбцы
SELECT xmin, xmax, ctid, * FROM accounts WHERE id = 1;
-- Результат: xmin=100, xmax=0, ctid=(0,1), id=1, balance=5000

-- После UPDATE в другой транзакции (txid=200):
-- Старая версия: xmin=100, xmax=200, ctid=(0,1)
-- Новая версия:  xmin=200, xmax=0,   ctid=(0,5)
```

### Видимость строк

Транзакция видит строку, если:
- `xmin` зафиксирована (committed) и < ID текущей транзакции (или это она сама)
- `xmax` не установлен (0) или не зафиксирован, или > ID текущей транзакции

### Последствия MVCC

- При обновлении и удалении создаются «мёртвые» (dead) версии, которые занимают место
- Необходим процесс VACUUM для очистки мёртвых версий
- Индексы также содержат ссылки на все версии строк
- Таблицы могут «раздуваться» (table bloat) без регулярного VACUUM

### Преимущества MVCC

- Читатели не блокируют писателей — высокая производительность при конкурентном доступе
- Консистентные snapshot-чтения без блокировок
- Это фундамент всех уровней изоляции в PostgreSQL

> **На собеседовании:** MVCC — один из самых частых вопросов по PostgreSQL. Обязательно упомяните xmin/xmax и то, что UPDATE создаёт новую версию строки (а не изменяет существующую). Связка MVCC -> dead tuples -> VACUUM показывает системное понимание.

[к оглавлению](#postgresql)

---

## Какие виды блокировок существуют в PostgreSQL?
<!-- grade: middle -->

PostgreSQL использует многоуровневую систему блокировок: строковые (row-level) и табличные (table-level). Строковые блокировки не используют память менеджера блокировок — информация хранится в самих строках через `xmax`.

### Строковые блокировки (row-level locks)

```sql
-- Явная блокировка строк
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;          -- эксклюзивная блокировка
SELECT * FROM accounts WHERE id = 1 FOR NO KEY UPDATE;   -- без блокировки FK
SELECT * FROM accounts WHERE id = 1 FOR SHARE;           -- разделяемая (для чтения)
SELECT * FROM accounts WHERE id = 1 FOR KEY SHARE;       -- мягкая разделяемая
```

Полезные модификаторы:

```sql
-- SKIP LOCKED — пропустить заблокированные строки (очередь задач)
SELECT * FROM task_queue
WHERE status = 'new'
ORDER BY created_at
LIMIT 1
FOR UPDATE SKIP LOCKED;

-- NOWAIT — не ждать, сразу ошибка
SELECT * FROM accounts WHERE id = 1 FOR UPDATE NOWAIT;
```

### Табличные блокировки (table-level locks)

| Режим | Конфликтует с |
|---|---|
| `ACCESS SHARE` | `ACCESS EXCLUSIVE` |
| `ROW SHARE` | `EXCLUSIVE`, `ACCESS EXCLUSIVE` |
| `ROW EXCLUSIVE` | `SHARE`, `SHARE ROW EXCLUSIVE`, `EXCLUSIVE`, `ACCESS EXCLUSIVE` |
| `SHARE` | `ROW EXCLUSIVE`, `SHARE ROW EXCLUSIVE`, `EXCLUSIVE`, `ACCESS EXCLUSIVE` |
| `ACCESS EXCLUSIVE` | Все режимы |

Обычные операции автоматически берут блокировки: SELECT -> `ACCESS SHARE`, INSERT/UPDATE/DELETE -> `ROW EXCLUSIVE`, CREATE INDEX -> `SHARE`, ALTER TABLE / DROP TABLE -> `ACCESS EXCLUSIVE`.

```sql
-- CREATE INDEX CONCURRENTLY не блокирует запись
CREATE INDEX CONCURRENTLY idx_tx_amount ON transactions (amount);
```

<details><summary>Мониторинг блокировок</summary>

```sql
-- Посмотреть текущие блокировки
SELECT pid, relation::regclass, mode, granted
FROM pg_locks
WHERE NOT granted;

-- Найти блокирующие и ожидающие запросы
SELECT blocked.pid AS blocked_pid,
       blocked.query AS blocked_query,
       blocking.pid AS blocking_pid,
       blocking.query AS blocking_query
FROM pg_stat_activity blocked
JOIN pg_locks bl ON bl.pid = blocked.pid AND NOT bl.granted
JOIN pg_locks bk ON bk.relation = bl.relation AND bk.granted
JOIN pg_stat_activity blocking ON blocking.pid = bk.pid
WHERE blocked.pid != blocking.pid;
```

</details>

> **На собеседовании:** ключевые практические знания — `FOR UPDATE SKIP LOCKED` для реализации очереди задач и `CREATE INDEX CONCURRENTLY` для создания индексов без блокировки записи на production. Это показывает опыт работы с реальными системами.

[к оглавлению](#postgresql)

---

## Что такое Advisory Locks и когда их использовать?
<!-- grade: senior -->

Advisory Locks (рекомендательные блокировки) — это механизм блокировок на уровне приложения. PostgreSQL предоставляет инфраструктуру, а семантику определяет приложение. Они не связаны ни с какими объектами базы данных.

```sql
-- Блокировка на уровне сессии (удерживается до явного освобождения или отключения)
SELECT pg_advisory_lock(12345);
-- ... выполняем критическую секцию ...
SELECT pg_advisory_unlock(12345);

-- Блокировка на уровне транзакции (освобождается при COMMIT/ROLLBACK)
BEGIN;
SELECT pg_advisory_xact_lock(12345);
-- ... работаем ...
COMMIT;  -- блокировка автоматически освобождена

-- Неблокирующие варианты (возвращают true/false)
SELECT pg_try_advisory_lock(12345);

-- Разделяемые (shared) advisory locks
SELECT pg_advisory_lock_shared(12345);
```

### Типичные применения

```sql
-- 1. Предотвращение параллельного выполнения batch-процесса
SELECT pg_try_advisory_lock(hashtext('daily_report_generation'));
-- Если true — выполняем, иначе — уже запущен

-- 2. Блокировка обработки конкретной сущности
SELECT pg_advisory_xact_lock(client_id) FROM clients WHERE id = 42;

-- 3. Двухпараметровая блокировка (пространство имён)
SELECT pg_advisory_lock(1, 42);  -- (тип операции, ID сущности)
```

### Преимущества перед SELECT FOR UPDATE

- Не привязаны к строкам таблицы — можно блокировать абстрактные ресурсы
- Быстрее для сценариев, не требующих блокировки конкретных строк
- Полезны для координации между различными процессами приложения

Advisory locks потребляют разделяемую память, ограничение задаётся параметром `max_locks_per_transaction`.

> **На собеседовании:** advisory locks — это продвинутая тема. Расскажите конкретный сценарий: «предотвращение дублирования batch-задач» или «блокировка обработки клиента без FOR UPDATE». Упомяните разницу между сессионными и транзакционными advisory locks — забытая сессионная блокировка приведёт к утечке.

[к оглавлению](#postgresql)

---

## Что такое deadlock и как его избежать?
<!-- grade: middle -->

Deadlock (взаимная блокировка) — ситуация, когда две или более транзакции блокируют друг друга, каждая ожидая ресурс, удерживаемый другой. PostgreSQL автоматически обнаруживает deadlock и откатывает одну из транзакций.

### Пример

```
Транзакция 1:                        Транзакция 2:
BEGIN;                               BEGIN;
UPDATE accounts SET balance = 900    UPDATE accounts SET balance = 1100
  WHERE id = 1;  -- блокирует id=1    WHERE id = 2;  -- блокирует id=2
UPDATE accounts SET balance = 1100   UPDATE accounts SET balance = 900
  WHERE id = 2;  -- ЖДЁТ id=2         WHERE id = 1;  -- ЖДЁТ id=1
-- DEADLOCK!
```

PostgreSQL обнаруживает deadlock через детектор с периодом `deadlock_timeout` (по умолчанию 1 секунда) и откатывает одну из транзакций с ошибкой `ERROR: deadlock detected`.

### Способы избежать deadlock

**1. Единый порядок обращения к ресурсам:**
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = LEAST(1, 2);
UPDATE accounts SET balance = balance + 100 WHERE id = GREATEST(1, 2);
COMMIT;
```

**2. SELECT FOR UPDATE с сортировкой:**
```sql
BEGIN;
SELECT * FROM accounts WHERE id IN (1, 2) ORDER BY id FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

**3. Минимизировать время транзакций** — не выполнять долгие вычисления или вызовы внешних сервисов внутри транзакции.

**4. NOWAIT или SKIP LOCKED:**
```sql
SELECT * FROM accounts WHERE id = 1 FOR UPDATE NOWAIT;
SELECT * FROM task_queue WHERE status = 'pending'
ORDER BY id LIMIT 1 FOR UPDATE SKIP LOCKED;
```

**5. Уменьшить уровень изоляции** — `READ COMMITTED` менее склонен к deadlock, чем `SERIALIZABLE`.

> **На собеседовании:** главное правило предотвращения deadlock — единый порядок обращения к ресурсам. Если все транзакции блокируют строки в порядке возрастания ID, deadlock невозможен. Упомяните, что PostgreSQL обнаруживает deadlock автоматически, поэтому приложение должно уметь повторять откаченную транзакцию.

[к оглавлению](#postgresql)

---

## Что такое представления (views) и материализованные представления?
<!-- grade: middle -->

Представление (view) — именованный сохранённый запрос, выполняемый каждый раз при обращении. Материализованное представление (materialized view) — представление, результат которого физически сохраняется на диск. Выбор между ними — компромисс между актуальностью данных и скоростью чтения.

### Обычное представление (VIEW)

```sql
CREATE VIEW active_clients AS
SELECT c.id, c.name, c.email, a.balance
FROM clients c
JOIN accounts a ON a.client_id = c.id
WHERE c.status = 'active' AND a.balance > 0;

-- Использование как обычной таблицы
SELECT * FROM active_clients WHERE balance > 100000;
```

Простые представления (одна таблица, без агрегаций) могут быть обновляемыми — допускают INSERT/UPDATE/DELETE.

### Материализованное представление (MATERIALIZED VIEW)

<details><summary>Пример создания и использования</summary>

```sql
CREATE MATERIALIZED VIEW mv_monthly_stats AS
SELECT
    date_trunc('month', created_at) AS month,
    account_id,
    count(*) AS tx_count,
    sum(amount) AS total_amount,
    avg(amount) AS avg_amount
FROM transactions
GROUP BY date_trunc('month', created_at), account_id;

-- Создание индекса на материализованном представлении
CREATE UNIQUE INDEX idx_mv_stats ON mv_monthly_stats (month, account_id);

-- Запрос — быстро, читаем из предвычисленных данных
SELECT * FROM mv_monthly_stats WHERE account_id = 100;

-- Обновление данных (полная перестройка)
REFRESH MATERIALIZED VIEW mv_monthly_stats;

-- Обновление без блокировки чтения (требует UNIQUE INDEX)
REFRESH MATERIALIZED VIEW CONCURRENTLY mv_monthly_stats;
```

</details>

### Сравнение

| Свойство | VIEW | MATERIALIZED VIEW |
|---|---|---|
| Хранение данных | Нет (запрос каждый раз) | Да (физически на диске) |
| Актуальность | Всегда актуальные | Данные на момент REFRESH |
| Скорость чтения | Зависит от сложности запроса | Быстро |
| Индексы | Нет (используются индексы базовых таблиц) | Можно создавать свои |
| INSERT/UPDATE/DELETE | Возможно (для простых) | Нет |

### Когда использовать materialized view

- Тяжёлые аналитические запросы (отчёты, дашборды)
- Данные обновляются редко или допустима небольшая задержка
- Запрос читается часто, а базовые данные меняются нечасто

> **На собеседовании:** `REFRESH MATERIALIZED VIEW CONCURRENTLY` — это ключевое слово, которое показывает знание production-практик: без CONCURRENTLY обновление блокирует чтение. Для CONCURRENTLY обязателен UNIQUE INDEX на materialized view.

[к оглавлению](#postgresql)

---

## Что такое CTE (Common Table Expressions)?
<!-- grade: middle -->

CTE (Common Table Expressions) — именованные подзапросы, определяемые в блоке `WITH`, которые улучшают читаемость сложных запросов и позволяют ссылаться на один и тот же подзапрос несколько раз.

<details><summary>Простой и составной CTE</summary>

```sql
-- Простой CTE
WITH large_accounts AS (
    SELECT id, client_id, balance
    FROM accounts
    WHERE balance > 1000000
)
SELECT c.name, la.balance
FROM large_accounts la
JOIN clients c ON c.id = la.client_id;

-- Несколько CTE
WITH
debits AS (
    SELECT account_id, sum(amount) AS total_debit
    FROM transactions
    WHERE type = 'DEBIT' AND created_at > '2024-01-01'
    GROUP BY account_id
),
credits AS (
    SELECT account_id, sum(amount) AS total_credit
    FROM transactions
    WHERE type = 'CREDIT' AND created_at > '2024-01-01'
    GROUP BY account_id
)
SELECT
    a.id,
    COALESCE(d.total_debit, 0) AS total_debit,
    COALESCE(c.total_credit, 0) AS total_credit,
    COALESCE(c.total_credit, 0) - COALESCE(d.total_debit, 0) AS net_flow
FROM accounts a
LEFT JOIN debits d ON d.account_id = a.id
LEFT JOIN credits c ON c.account_id = a.id;
```

</details>

### Рекурсивный CTE

```sql
-- Иерархия подразделений
WITH RECURSIVE department_tree AS (
    -- Якорный запрос (базовый случай)
    SELECT id, name, parent_id, 1 AS level
    FROM departments
    WHERE parent_id IS NULL

    UNION ALL

    -- Рекурсивный запрос
    SELECT d.id, d.name, d.parent_id, dt.level + 1
    FROM departments d
    JOIN department_tree dt ON d.parent_id = dt.id
)
SELECT * FROM department_tree ORDER BY level, name;
```

### CTE с модификацией данных

```sql
-- Удалить устаревшие записи и вернуть количество
WITH deleted AS (
    DELETE FROM sessions
    WHERE expires_at < now()
    RETURNING *
)
SELECT count(*) AS deleted_count FROM deleted;
```

### Важно о производительности

- До PostgreSQL 12 CTE были «барьером оптимизации» — планировщик не мог протолкнуть условия из внешнего запроса внутрь CTE
- Начиная с PostgreSQL 12 CTE по умолчанию инлайнятся (встраиваются) в основной запрос, если используются один раз и не имеют побочных эффектов
- Принудительная материализация: `WITH cte AS MATERIALIZED (...)`
- Принудительный инлайн: `WITH cte AS NOT MATERIALIZED (...)`

> **На собеседовании:** два ключевых момента: (1) рекурсивный CTE для обхода деревьев — WHERE RECURSIVE и UNION ALL, (2) изменение поведения в PostgreSQL 12 — CTE теперь инлайнятся, что может как улучшить, так и ухудшить производительность по сравнению с ожидаемым.

[к оглавлению](#postgresql)

---

## Как работают оконные функции (window functions)?
<!-- grade: middle -->

Оконные функции выполняют вычисления над набором строк, связанных с текущей строкой, без группировки — в отличие от GROUP BY строки не схлопываются. Это мощный инструмент для аналитических запросов.

### Синтаксис

```sql
функция() OVER (
    [PARTITION BY столбец1, столбец2, ...]   -- разбивка на группы (окна)
    [ORDER BY столбец3 [ASC|DESC], ...]      -- сортировка внутри окна
    [ROWS|RANGE BETWEEN ... AND ...]         -- рамка окна
)
```

### Пример: нумерация и нарастающий итог

```sql
SELECT
    id, account_id, amount, created_at,
    ROW_NUMBER() OVER (PARTITION BY account_id ORDER BY created_at) AS tx_number,
    SUM(amount) OVER (PARTITION BY account_id ORDER BY created_at) AS running_total,
    SUM(amount) OVER (PARTITION BY account_id) AS account_total
FROM transactions;
```

### Рамка окна (frame)

```sql
-- Скользящее среднее за 3 записи
AVG(amount) OVER (
    ORDER BY created_at
    ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
) AS moving_avg
```

При наличии ORDER BY рамка по умолчанию: `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` (нарастающий итог).

### Именованное окно (WINDOW)

```sql
SELECT
    id, amount,
    ROW_NUMBER() OVER w AS rn,
    SUM(amount) OVER w AS running_total,
    AVG(amount) OVER w AS running_avg
FROM transactions
WINDOW w AS (PARTITION BY account_id ORDER BY created_at);
```

> **На собеседовании:** главное отличие от GROUP BY — строки не схлопываются. PARTITION BY разбивает данные на окна (как GROUP BY), но каждая строка остаётся в результате. Упомяните именованное окно (WINDOW) — это показывает умение писать чистый SQL.

[к оглавлению](#postgresql)

---

## Какие основные оконные функции существуют в PostgreSQL?
<!-- grade: middle -->

PostgreSQL предоставляет ранжирующие функции (ROW_NUMBER, RANK, DENSE_RANK, NTILE) и функции смещения (LAG, LEAD, FIRST_VALUE, LAST_VALUE). Вместе с агрегатными функциями (SUM, AVG, COUNT) в оконном контексте они покрывают большинство аналитических задач.

### Ранжирующие функции

```sql
SELECT
    name, department, salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,
    RANK()       OVER (ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank,
    NTILE(4)     OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

| Функция | Описание | Одинаковые значения |
|---|---|---|
| `ROW_NUMBER()` | Уникальный номер строки | Разные номера |
| `RANK()` | Ранг с пропусками | Одинаковый ранг, следующий с пропуском |
| `DENSE_RANK()` | Ранг без пропусков | Одинаковый ранг, следующий без пропуска |
| `NTILE(n)` | Разбивка на n групп | Равномерное распределение |

### Функции смещения

```sql
SELECT
    id, amount, created_at,
    LAG(amount, 1) OVER (ORDER BY created_at) AS prev_amount,
    LEAD(amount, 1) OVER (ORDER BY created_at) AS next_amount,
    FIRST_VALUE(amount) OVER (PARTITION BY account_id ORDER BY created_at) AS first_tx,
    LAST_VALUE(amount) OVER (
        PARTITION BY account_id ORDER BY created_at
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS last_tx
FROM transactions;
```

<details><summary>Практические задачи</summary>

```sql
-- Топ-3 транзакции по каждому счёту
WITH ranked AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY account_id ORDER BY amount DESC) AS rn
    FROM transactions
)
SELECT * FROM ranked WHERE rn <= 3;

-- Разница с предыдущей транзакцией
SELECT
    id, amount,
    amount - LAG(amount) OVER (ORDER BY created_at) AS diff_with_prev,
    created_at - LAG(created_at) OVER (ORDER BY created_at) AS time_gap
FROM transactions
WHERE account_id = 100;

-- Процент от общей суммы
SELECT
    account_id, amount,
    ROUND(amount * 100.0 / SUM(amount) OVER (), 2) AS pct_of_total,
    ROUND(amount * 100.0 / SUM(amount) OVER (PARTITION BY account_id), 2) AS pct_of_account
FROM transactions;

-- Нарастающий итог с ежедневной группировкой
SELECT
    date(created_at) AS day,
    SUM(amount) AS daily_total,
    SUM(SUM(amount)) OVER (ORDER BY date(created_at)) AS cumulative_total
FROM transactions
GROUP BY date(created_at);
```

</details>

> **На собеседовании:** обязательный вопрос — отличие ROW_NUMBER, RANK, DENSE_RANK на одинаковых значениях. Классическая задача: «топ-N записей в каждой группе» — решается через ROW_NUMBER + PARTITION BY + CTE с фильтром.

[к оглавлению](#postgresql)

---

## Что такое триггеры в PostgreSQL?
<!-- grade: middle -->

Триггер (trigger) — именованный объект базы данных, который автоматически выполняет функцию при наступлении определённого события (INSERT, UPDATE, DELETE, TRUNCATE) на таблице. Триггеры позволяют реализовать бизнес-логику на уровне БД.

### Виды триггеров

- `BEFORE` — выполняется до операции (может изменить данные или отменить операцию)
- `AFTER` — выполняется после операции (данные уже изменены)
- `INSTEAD OF` — заменяет операцию (только для представлений)
- `FOR EACH ROW` — для каждой строки
- `FOR EACH STATEMENT` — один раз на весь оператор

### Пример: автообновление timestamp

```sql
CREATE OR REPLACE FUNCTION update_modified_timestamp()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = now();
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_accounts_updated
    BEFORE UPDATE ON accounts
    FOR EACH ROW
    EXECUTE FUNCTION update_modified_timestamp();
```

<details><summary>Пример: аудит изменений</summary>

```sql
CREATE TABLE audit_log (
    id bigserial PRIMARY KEY,
    table_name text NOT NULL,
    operation text NOT NULL,
    old_data jsonb,
    new_data jsonb,
    changed_by text DEFAULT current_user,
    changed_at timestamptz DEFAULT now()
);

CREATE OR REPLACE FUNCTION audit_trigger_func()
RETURNS TRIGGER AS $$
BEGIN
    IF TG_OP = 'DELETE' THEN
        INSERT INTO audit_log (table_name, operation, old_data)
        VALUES (TG_TABLE_NAME, 'DELETE', to_jsonb(OLD));
        RETURN OLD;
    ELSIF TG_OP = 'UPDATE' THEN
        INSERT INTO audit_log (table_name, operation, old_data, new_data)
        VALUES (TG_TABLE_NAME, 'UPDATE', to_jsonb(OLD), to_jsonb(NEW));
        RETURN NEW;
    ELSIF TG_OP = 'INSERT' THEN
        INSERT INTO audit_log (table_name, operation, new_data)
        VALUES (TG_TABLE_NAME, 'INSERT', to_jsonb(NEW));
        RETURN NEW;
    END IF;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_accounts_audit
    AFTER INSERT OR UPDATE OR DELETE ON accounts
    FOR EACH ROW
    EXECUTE FUNCTION audit_trigger_func();
```

</details>

### Специальные переменные в триггерных функциях

- `NEW` — новая строка (INSERT, UPDATE)
- `OLD` — старая строка (UPDATE, DELETE)
- `TG_OP` — тип операции ('INSERT', 'UPDATE', 'DELETE', 'TRUNCATE')
- `TG_TABLE_NAME` — имя таблицы
- `TG_WHEN` — 'BEFORE' или 'AFTER'

### Условный триггер (PostgreSQL 9.0+)

```sql
CREATE TRIGGER trg_large_transactions
    AFTER INSERT ON transactions
    FOR EACH ROW
    WHEN (NEW.amount > 1000000)
    EXECUTE FUNCTION notify_compliance_department();
```

> **На собеседовании:** важно разделить BEFORE и AFTER: BEFORE может модифицировать NEW и даже отменить операцию (RETURN NULL), AFTER — нет. Условный триггер (WHEN) — продвинутый ответ, показывающий, что вы не используете IF внутри триггерной функции для простых условий.

[к оглавлению](#postgresql)

---

## Чем отличаются функции от процедур в PostgreSQL?
<!-- grade: middle -->

Начиная с PostgreSQL 11 поддерживаются как функции (FUNCTION), так и процедуры (PROCEDURE). Ключевое различие: процедуры могут управлять транзакциями (COMMIT/ROLLBACK внутри тела), а функции — нет.

### Функции (FUNCTION)

```sql
CREATE OR REPLACE FUNCTION get_account_balance(p_account_id bigint)
RETURNS numeric AS $$
DECLARE
    v_balance numeric;
BEGIN
    SELECT balance INTO v_balance
    FROM accounts WHERE id = p_account_id;

    IF NOT FOUND THEN
        RAISE EXCEPTION 'Счёт % не найден', p_account_id;
    END IF;

    RETURN v_balance;
END;
$$ LANGUAGE plpgsql;

SELECT get_account_balance(1);
```

### Процедуры (PROCEDURE)

<details><summary>Пример процедуры перевода средств</summary>

```sql
CREATE OR REPLACE PROCEDURE transfer_funds(
    p_from_account bigint,
    p_to_account bigint,
    p_amount numeric
)
LANGUAGE plpgsql AS $$
BEGIN
    IF p_amount <= 0 THEN
        RAISE EXCEPTION 'Сумма перевода должна быть положительной';
    END IF;

    UPDATE accounts SET balance = balance - p_amount WHERE id = p_from_account;
    IF NOT FOUND THEN
        RAISE EXCEPTION 'Счёт-отправитель % не найден', p_from_account;
    END IF;

    UPDATE accounts SET balance = balance + p_amount WHERE id = p_to_account;
    IF NOT FOUND THEN
        RAISE EXCEPTION 'Счёт-получатель % не найден', p_to_account;
    END IF;

    -- Процедура может управлять транзакциями!
    COMMIT;
END;
$$;

CALL transfer_funds(1, 2, 50000);
```

</details>

### Ключевые отличия

| Свойство | FUNCTION | PROCEDURE |
|---|---|---|
| Возвращает значение | Да (RETURNS) | Нет |
| Управление транзакциями | Нет (COMMIT/ROLLBACK запрещены) | Да (COMMIT/ROLLBACK разрешены) |
| Использование в SELECT | `SELECT func()` | Нет |
| Вызов | `SELECT func()` или в выражениях | `CALL proc()` |
| Используется в триггерах | Да | Нет |

Процедуры полезны для длительных пакетных операций, где нужно промежуточно фиксировать результаты (например, миграция данных большими батчами).

> **На собеседовании:** единственный критический факт — процедуры могут делать COMMIT/ROLLBACK, функции — нет. Это определяет выбор: если нужно промежуточно фиксировать результаты в длительной операции — процедура, если нужно вернуть значение или использовать в SELECT — функция.

[к оглавлению](#postgresql)

---

## Что такое партиционирование (partitioning) таблиц?
<!-- grade: middle -->

Партиционирование — это разбиение большой таблицы на несколько физических частей (партиций), при этом логически они остаются единой таблицей. Повышает производительность запросов (partition pruning) и упрощает обслуживание. С PostgreSQL 10 поддерживается декларативное партиционирование.

### RANGE-партиционирование (по диапазону)

```sql
CREATE TABLE transactions (
    id bigserial,
    account_id bigint NOT NULL,
    amount numeric(15, 2) NOT NULL,
    created_at timestamptz NOT NULL,
    PRIMARY KEY (id, created_at)
) PARTITION BY RANGE (created_at);

CREATE TABLE transactions_2024_01 PARTITION OF transactions
    FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');
CREATE TABLE transactions_2024_02 PARTITION OF transactions
    FOR VALUES FROM ('2024-02-01') TO ('2024-03-01');
CREATE TABLE transactions_default PARTITION OF transactions DEFAULT;
```

### LIST-партиционирование (по списку значений)

```sql
CREATE TABLE accounts (
    id bigserial,
    client_id bigint,
    currency char(3) NOT NULL,
    balance numeric(15, 2),
    PRIMARY KEY (id, currency)
) PARTITION BY LIST (currency);

CREATE TABLE accounts_rub PARTITION OF accounts FOR VALUES IN ('RUB');
CREATE TABLE accounts_usd PARTITION OF accounts FOR VALUES IN ('USD');
CREATE TABLE accounts_eur PARTITION OF accounts FOR VALUES IN ('EUR');
CREATE TABLE accounts_other PARTITION OF accounts DEFAULT;
```

### HASH-партиционирование (по хешу)

```sql
CREATE TABLE session_data (
    session_id uuid NOT NULL,
    data jsonb,
    PRIMARY KEY (session_id)
) PARTITION BY HASH (session_id);

CREATE TABLE session_data_0 PARTITION OF session_data
    FOR VALUES WITH (MODULUS 4, REMAINDER 0);
-- ... и так далее для REMAINDER 1, 2, 3
```

### Преимущества

- **Partition pruning** — PostgreSQL сканирует только нужные партиции
- Быстрое удаление старых данных: `DROP TABLE transactions_2023_01` вместо `DELETE`
- Индексы меньше — быстрее обслуживаются
- VACUUM работает на отдельных партициях

### Рекомендации

- Партиционирование имеет смысл для таблиц от нескольких миллионов строк
- Ключ партиционирования должен входить в PRIMARY KEY
- Индексы создаются на каждой партиции отдельно (или автоматически при создании на родительской таблице)

> **На собеседовании:** три типа (RANGE, LIST, HASH) — назовите с примером. RANGE для временных рядов (логи, транзакции), LIST для дискретных значений (валюта, регион), HASH для равномерного распределения. Упомяните partition pruning и быстрое удаление партиций через DROP вместо DELETE.

[к оглавлению](#postgresql)

---

## Какие виды репликации поддерживает PostgreSQL?
<!-- grade: senior -->

PostgreSQL поддерживает два основных вида репликации: потоковую (streaming/физическую) и логическую (logical). Потоковая реплицирует всю БД побайтово через WAL-поток, логическая — отдельные таблицы на уровне SQL-операций.

### Потоковая (физическая) репликация

```
Мастер (Primary) ---> WAL stream ---> Реплика (Standby)
```

- Реплицируется **вся** база данных целиком (нельзя выбрать отдельные таблицы)
- Реплика может быть «горячей» (hot standby) — принимает запросы на чтение
- Синхронная или асинхронная репликация
- Используется для отказоустойчивости (failover) и масштабирования чтения

### Синхронная vs асинхронная

- **Асинхронная** (по умолчанию) — мастер не ждёт подтверждения от реплики. Возможна потеря данных при падении мастера
- **Синхронная** — мастер ждёт подтверждения от реплики перед COMMIT. Нулевая потеря данных, но выше задержка записи

### Логическая репликация (PostgreSQL 10+)

```sql
-- На мастере (publisher)
CREATE PUBLICATION payments_pub FOR TABLE transactions, accounts;

-- На подписчике (subscriber)
CREATE SUBSCRIPTION payments_sub
    CONNECTION 'host=master port=5432 dbname=bank user=replicator'
    PUBLICATION payments_pub;
```

- Можно реплицировать **отдельные таблицы**
- Можно реплицировать между **разными версиями** PostgreSQL
- Подписчик может иметь собственные индексы, триггеры
- Подписчик может быть **доступен для записи**

### Сравнение

| Свойство | Потоковая | Логическая |
|---|---|---|
| Единица репликации | Вся БД | Таблицы |
| Формат | Физический (WAL-байты) | Логический (SQL-операции) |
| Запись на реплике | Нет | Да |
| Разные версии PG | Нет | Да |
| DDL-репликация | Да | Нет |
| Применение | Failover, чтение | Миграция, частичная репликация |

> **На собеседовании:** покажите понимание сценариев: потоковая — для failover и read-replicas, логическая — для миграции между версиями и частичной репликации. Упомяните, что логическая не реплицирует DDL (ALTER TABLE) — это частый подводный камень при миграции.

[к оглавлению](#postgresql)

---

## Зачем нужны VACUUM и AUTOVACUUM?
<!-- grade: middle -->

Из-за MVCC при UPDATE и DELETE старые версии строк не удаляются физически — они помечаются как «мёртвые» (dead tuples). VACUUM — это процесс очистки мёртвых версий строк и возврата освобождённого пространства для повторного использования.

### Что делает VACUUM

1. Помечает пространство от мёртвых строк как доступное для повторного использования (но **не возвращает** его ОС)
2. Обновляет карту видимости (visibility map) — ускоряет Index Only Scan
3. Обновляет карту свободного пространства (free space map)
4. Предотвращает переполнение счётчика транзакций (transaction ID wraparound)

```sql
VACUUM transactions;                -- ручной VACUUM
VACUUM VERBOSE transactions;        -- с подробностями
VACUUM ANALYZE transactions;        -- VACUUM + обновление статистики
VACUUM FULL transactions;           -- полная перестройка, возвращает место ОС
-- ВНИМАНИЕ: VACUUM FULL блокирует таблицу (ACCESS EXCLUSIVE)!
```

### AUTOVACUUM

AUTOVACUUM — фоновый процесс, который автоматически выполняет VACUUM и ANALYZE.

Формула запуска: `dead_tuples > threshold + scale_factor * n_live_tup`

<details><summary>Настройка и мониторинг AUTOVACUUM</summary>

```sql
-- Мониторинг
SELECT
    relname, n_dead_tup, n_live_tup,
    last_vacuum, last_autovacuum,
    last_analyze, last_autoanalyze
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC;

-- Настройка для конкретной таблицы (горячие таблицы)
ALTER TABLE transactions SET (
    autovacuum_vacuum_scale_factor = 0.01,     -- 1% вместо 20%
    autovacuum_vacuum_threshold = 1000,
    autovacuum_analyze_scale_factor = 0.005
);
```

</details>

### Transaction ID Wraparound

PostgreSQL использует 32-битный счётчик транзакций (~4 млрд). VACUUM обязан выполняться периодически, чтобы «заморозить» (freeze) старые транзакции и предотвратить переполнение. Если автовакуум не справляется, PostgreSQL может **остановить** приём новых транзакций для защиты данных.

```sql
-- Проверить возраст самой старой незамороженной транзакции
SELECT datname, age(datfrozenxid) FROM pg_database ORDER BY age DESC;
-- Если age приближается к 2 млрд — критическая ситуация!
```

**Никогда не отключайте AUTOVACUUM!** Вместо этого настройте его параметры для конкретных таблиц.

> **На собеседовании:** связка MVCC -> dead tuples -> VACUUM -> wraparound показывает системное понимание PostgreSQL. Ключевые факты: VACUUM не возвращает место ОС (это делает только VACUUM FULL с блокировкой), AUTOVACUUM нельзя отключать из-за риска transaction ID wraparound.

[к оглавлению](#postgresql)

---

## Как мониторить производительность PostgreSQL?
<!-- grade: senior -->

PostgreSQL предоставляет богатый набор системных представлений (pg_stat_*) и расширений для мониторинга. Шесть ключевых источников покрывают большинство задач диагностики.

### 1. pg_stat_statements — статистика запросов

<details><summary>Примеры запросов к pg_stat_statements</summary>

```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;

-- Топ-10 самых тяжёлых запросов по общему времени
SELECT
    queryid,
    substr(query, 1, 100) AS query,
    calls,
    round(total_exec_time::numeric, 2) AS total_time_ms,
    round(mean_exec_time::numeric, 2) AS avg_time_ms,
    rows
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 10;

-- Запросы с наибольшим количеством чтений с диска
SELECT
    substr(query, 1, 80) AS query,
    calls, shared_blks_hit, shared_blks_read,
    round(100.0 * shared_blks_hit / NULLIF(shared_blks_hit + shared_blks_read, 0), 2) AS cache_hit_pct
FROM pg_stat_statements
ORDER BY shared_blks_read DESC
LIMIT 10;
```

</details>

### 2. pg_stat_user_tables — статистика по таблицам

```sql
SELECT
    relname AS table_name,
    seq_scan,            -- много Seq Scan — нужен индекс?
    idx_scan,            -- количество Index Scan
    n_tup_ins AS inserts, n_tup_upd AS updates, n_tup_del AS deletes,
    n_dead_tup AS dead_tuples,
    last_autovacuum
FROM pg_stat_user_tables
ORDER BY seq_scan DESC;
```

### 3. pg_stat_user_indexes — неиспользуемые индексы

```sql
SELECT
    schemaname, tablename, indexrelname,
    idx_scan,          -- 0 = не используется
    pg_size_pretty(pg_relation_size(indexrelid)) AS index_size
FROM pg_stat_user_indexes
WHERE idx_scan = 0
ORDER BY pg_relation_size(indexrelid) DESC;
```

### 4. pg_stat_activity — текущие сессии и запросы

```sql
SELECT
    pid, usename, state,
    now() - query_start AS duration,
    wait_event_type, wait_event,
    substr(query, 1, 100) AS query
FROM pg_stat_activity
WHERE state = 'active' AND pid != pg_backend_pid()
ORDER BY duration DESC;

-- Завершить зависший запрос
SELECT pg_cancel_backend(pid);     -- мягкое (SIGINT)
SELECT pg_terminate_backend(pid);  -- жёсткое (SIGTERM)
```

### 5. Размер таблиц и индексов

```sql
SELECT
    relname AS table_name,
    pg_size_pretty(pg_total_relation_size(relid)) AS total_size,
    pg_size_pretty(pg_relation_size(relid)) AS table_size,
    pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) AS index_size
FROM pg_stat_user_tables
ORDER BY pg_total_relation_size(relid) DESC
LIMIT 10;
```

### 6. Cache hit ratio

```sql
SELECT
    sum(blks_hit) * 100.0 / NULLIF(sum(blks_hit) + sum(blks_read), 0) AS cache_hit_ratio
FROM pg_stat_database;
-- Хорошее значение: > 99%
```

> **На собеседовании:** назовите три ключевых представления — pg_stat_statements (какие запросы тормозят), pg_stat_user_tables (много ли Seq Scan), pg_stat_activity (что выполняется сейчас). Cache hit ratio > 99% — признак здоровой системы, < 95% — нужно увеличить shared_buffers.

[к оглавлению](#postgresql)

---

## Как подключить PostgreSQL к Java/Spring приложению?
<!-- grade: junior -->

Подключение PostgreSQL к Java-приложению выполняется через JDBC-драйвер. Spring Boot автоматизирует конфигурацию, достаточно указать URL, пользователя и пароль в application.yml.

### Подключение через JDBC

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.3</version>
</dependency>
```

<details><summary>Прямое подключение через JDBC</summary>

```java
String url = "jdbc:postgresql://localhost:5432/bank_db";
String user = "app_user";
String password = "secret";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement ps = conn.prepareStatement(
         "SELECT id, balance FROM accounts WHERE client_id = ?")) {

    ps.setLong(1, clientId);
    try (ResultSet rs = ps.executeQuery()) {
        while (rs.next()) {
            long id = rs.getLong("id");
            BigDecimal balance = rs.getBigDecimal("balance");
        }
    }
}
```

</details>

### Подключение через Spring Boot

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/bank_db
    username: app_user
    password: ${DB_PASSWORD}
    driver-class-name: org.postgresql.Driver
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    hibernate:
      ddl-auto: validate
    properties:
      hibernate:
        default_schema: core
```

### Работа с типами PostgreSQL через JDBC

```java
// JSONB
PGobject jsonObject = new PGobject();
jsonObject.setType("jsonb");
jsonObject.setValue("{\"key\": \"value\"}");
ps.setObject(1, jsonObject);

// UUID
ps.setObject(1, UUID.randomUUID());

// Array
Array sqlArray = conn.createArrayOf("text", new String[]{"tag1", "tag2"});
ps.setArray(1, sqlArray);
```

### Использование с JPA/Hibernate

<details><summary>Пример Entity с типами PostgreSQL</summary>

```java
@Entity
@Table(name = "accounts", schema = "core")
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "client_id", nullable = false)
    private Long clientId;

    @Column(precision = 15, scale = 2)
    private BigDecimal balance;

    @Column(name = "created_at", columnDefinition = "timestamptz")
    private OffsetDateTime createdAt;

    // Для JSONB необходим пользовательский тип или библиотека
    @Type(JsonBinaryType.class)
    @Column(columnDefinition = "jsonb")
    private Map<String, Object> metadata;
}
```

```xml
<!-- Для работы с JSONB через Hibernate -->
<dependency>
    <groupId>io.hypersistence</groupId>
    <artifactId>hypersistence-utils-hibernate-63</artifactId>
    <version>3.7.3</version>
</dependency>
```

</details>

> **На собеседовании:** покажите знание нюансов: `ddl-auto: validate` (не create!) для production, `PGobject` для JSONB через JDBC, `hypersistence-utils` для JSONB через Hibernate. Упомяните, что `OffsetDateTime` маппится на `timestamptz`, а `LocalDateTime` — на `timestamp` (без зоны).

[к оглавлению](#postgresql)

---

## Как настроить пул соединений HikariCP для PostgreSQL?
<!-- grade: middle -->

HikariCP — высокопроизводительный пул соединений для JDBC, используемый по умолчанию в Spring Boot. Пул переиспользует готовые соединения, снижая задержки (создание TCP-соединения ~5-10 мс) и нагрузку на PostgreSQL (~10 МБ RAM на каждое соединение).

### Настройка в application.yml

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000       # таймаут ожидания соединения из пула (мс)
      idle-timeout: 600000            # время жизни неактивного соединения (мс)
      max-lifetime: 1800000           # максимальное время жизни соединения (мс)
      pool-name: BankAppPool
      leak-detection-threshold: 60000 # предупреждение при удержании > 60 сек
      data-source-properties:
        prepareThreshold: 5
        preparedStatementCacheQueries: 256
        preparedStatementCacheSizeMiB: 5
```

### Формула размера пула

Оптимальный размер пула обычно значительно меньше, чем ожидают. Формула от авторов HikariCP:

```
pool_size = количество_ядер_CPU * 2 + количество_дисков
```

Для сервера с 4 ядрами и 1 SSD: `4 * 2 + 1 = 9` соединений. Этого достаточно для тысяч запросов в секунду.

### Мониторинг HikariCP

<details><summary>Конфигурация метрик</summary>

```java
@Configuration
public class HikariMetricsConfig {

    @Bean
    public HikariDataSource dataSource(DataSourceProperties properties,
                                        MeterRegistry meterRegistry) {
        HikariDataSource ds = properties.initializeDataSourceBuilder()
                .type(HikariDataSource.class).build();
        ds.setMetricRegistry(meterRegistry);
        return ds;
    }
}
```

</details>

### Ключевые метрики

- `hikaricp.connections.active` — активные соединения
- `hikaricp.connections.idle` — простаивающие соединения
- `hikaricp.connections.pending` — потоки, ожидающие соединение (если > 0, пул мал)
- `hikaricp.connections.timeout.total` — количество таймаутов получения соединения

### Типичные проблемы

- **connection-timeout срабатывает** — пул исчерпан: увеличить пул или оптимизировать время транзакций
- **max-lifetime** должен быть меньше `idle_in_transaction_session_timeout` на PostgreSQL и меньше таймаута файервола/балансировщика
- **Утечка соединений** — включить `leak-detection-threshold` для обнаружения

> **На собеседовании:** формула размера пула — обязательный ответ. Покажите, что 10 соединений хватает для тысяч rps, а 100 соединений — это антипаттерн, который замедлит PostgreSQL из-за контекстных переключений. Метрика `pending > 0` — сигнал к действию.

[к оглавлению](#postgresql)
