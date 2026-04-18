[Вопросы для собеседования](README.md)

# PostgreSQL
+ [Что такое PostgreSQL и чем он отличается от других СУБД?](#Что-такое-PostgreSQL-и-чем-он-отличается-от-других-СУБД)
+ [Какие основные типы данных поддерживает PostgreSQL?](#Какие-основные-типы-данных-поддерживает-PostgreSQL)
+ [Что такое схемы (schemas) в PostgreSQL?](#Что-такое-схемы-schemas-в-PostgreSQL)
+ [Что такое последовательности (sequences) и как они работают?](#Что-такое-последовательности-sequences-и-как-они-работают)
+ [Чем отличаются типы serial и bigserial от явного использования последовательностей?](#Чем-отличаются-типы-serial-и-bigserial-от-явного-использования-последовательностей)
+ [Что такое JSONB в PostgreSQL и чем он отличается от JSON?](#Что-такое-JSONB-в-PostgreSQL-и-чем-он-отличается-от-JSON)
+ [Какие операторы существуют для работы с JSONB?](#Какие-операторы-существуют-для-работы-с-JSONB)
+ [Как индексировать JSONB-поля?](#Как-индексировать-JSONB-поля)
+ [Как работать с массивами в PostgreSQL?](#Как-работать-с-массивами-в-PostgreSQL)
+ [Какие типы индексов поддерживает PostgreSQL?](#Какие-типы-индексов-поддерживает-PostgreSQL)
+ [Когда какой тип индекса использовать?](#Когда-какой-тип-индекса-использовать)
+ [Что такое составные индексы и как они работают?](#Что-такое-составные-индексы-и-как-они-работают)
+ [Что такое частичные индексы и индексы на выражениях?](#Что-такое-частичные-индексы-и-индексы-на-выражениях)
+ [Как использовать EXPLAIN и EXPLAIN ANALYZE для анализа запросов?](#Как-использовать-EXPLAIN-и-EXPLAIN-ANALYZE-для-анализа-запросов)
+ [Как читать план выполнения запроса?](#Как-читать-план-выполнения-запроса)
+ [Какие основные приёмы оптимизации запросов в PostgreSQL?](#Какие-основные-приёмы-оптимизации-запросов-в-PostgreSQL)
+ [Как работают транзакции в PostgreSQL?](#Как-работают-транзакции-в-PostgreSQL)
+ [Какие уровни изоляции транзакций поддерживает PostgreSQL?](#Какие-уровни-изоляции-транзакций-поддерживает-PostgreSQL)
+ [Что такое MVCC и как он реализован в PostgreSQL?](#Что-такое-MVCC-и-как-он-реализован-в-PostgreSQL)
+ [Какие виды блокировок существуют в PostgreSQL?](#Какие-виды-блокировок-существуют-в-PostgreSQL)
+ [Что такое Advisory Locks и когда их использовать?](#Что-такое-Advisory-Locks-и-когда-их-использовать)
+ [Что такое deadlock и как его избежать?](#Что-такое-deadlock-и-как-его-избежать)
+ [Что такое представления (views) и материализованные представления?](#Что-такое-представления-views-и-материализованные-представления)
+ [Что такое CTE (Common Table Expressions)?](#Что-такое-CTE-Common-Table-Expressions)
+ [Как работают оконные функции (window functions)?](#Как-работают-оконные-функции-window-functions)
+ [Какие основные оконные функции существуют в PostgreSQL?](#Какие-основные-оконные-функции-существуют-в-PostgreSQL)
+ [Что такое триггеры в PostgreSQL?](#Что-такое-триггеры-в-PostgreSQL)
+ [Чем отличаются функции от процедур в PostgreSQL?](#Чем-отличаются-функции-от-процедур-в-PostgreSQL)
+ [Что такое партиционирование (partitioning) таблиц?](#Что-такое-партиционирование-partitioning-таблиц)
+ [Какие виды репликации поддерживает PostgreSQL?](#Какие-виды-репликации-поддерживает-PostgreSQL)
+ [Зачем нужны VACUUM и AUTOVACUUM?](#Зачем-нужны-VACUUM-и-AUTOVACUUM)
+ [Как мониторить производительность PostgreSQL?](#Как-мониторить-производительность-PostgreSQL)
+ [Как подключить PostgreSQL к Java/Spring приложению?](#Как-подключить-PostgreSQL-к-JavaSpring-приложению)
+ [Как настроить пул соединений HikariCP для PostgreSQL?](#Как-настроить-пул-соединений-HikariCP-для-PostgreSQL)

## Что такое PostgreSQL и чем он отличается от других СУБД?

__PostgreSQL__ — это объектно-реляционная система управления базами данных (ОРСУБД) с открытым исходным кодом. Это одна из наиболее зрелых и функциональных СУБД, активно развивающаяся с 1986 года (проект POSTGRES в Калифорнийском университете Беркли).

__Ключевые особенности PostgreSQL:__
+ полная поддержка ACID;
+ поддержка MVCC (Multi-Version Concurrency Control);
+ расширяемая система типов данных (JSON/JSONB, массивы, hstore, геоданные и др.);
+ мощный оптимизатор запросов;
+ поддержка CTE, оконных функций, полнотекстового поиска;
+ механизм расширений (extensions): PostGIS, pg_trgm, pgcrypto и др.;
+ строгое соответствие стандарту SQL.

__Сравнение с MySQL:__

| Критерий | PostgreSQL | MySQL |
|---|---|---|
| Тип | Объектно-реляционная | Реляционная |
| MVCC | Нативный, на уровне ядра | Зависит от движка (InnoDB) |
| JSON | Полноценный JSONB с индексами | Поддержка JSON (менее мощная) |
| Полнотекстовый поиск | Встроенный | Базовый (FULLTEXT) |
| Репликация | Потоковая и логическая | Встроенная (master-slave) |
| Расширяемость | Высокая (extensions, custom types) | Ограниченная |
| Производительность чтения | Отлично, особенно сложные запросы | Быстрые простые SELECT |

__Сравнение с Oracle:__

| Критерий | PostgreSQL | Oracle |
|---|---|---|
| Лицензия | Открытая (PostgreSQL License) | Коммерческая (дорогая) |
| Функциональность | Сопоставима для большинства задач | Немного шире (RAC, Data Guard) |
| PL/pgSQL vs PL/SQL | Похожий синтаксис | Более зрелый |
| Партиционирование | Декларативное (с v10) | Развитое |
| Стоимость | Бесплатная | Высокая стоимость лицензий |

В банковской сфере PostgreSQL часто выбирают за надёжность, строгое соответствие ACID и отсутствие лицензионных платежей.

[к оглавлению](#PostgreSQL)

## Какие основные типы данных поддерживает PostgreSQL?

PostgreSQL обладает одной из самых богатых систем типов данных среди всех СУБД.

__Числовые типы:__

| Тип | Размер | Диапазон |
|---|---|---|
| `smallint` | 2 байта | -32768 до +32767 |
| `integer` | 4 байта | -2147483648 до +2147483647 |
| `bigint` | 8 байт | -9223372036854775808 до +9223372036854775807 |
| `decimal/numeric` | переменный | до 131072 цифр до точки, до 16383 после |
| `real` | 4 байта | 6 значащих цифр |
| `double precision` | 8 байт | 15 значащих цифр |

Для финансовых расчётов в банковских приложениях всегда используют `numeric` — он обеспечивает точные вычисления без ошибок округления, в отличие от `real` и `double precision`.

__Символьные типы:__
+ `char(n)` — строка фиксированной длины, дополняется пробелами;
+ `varchar(n)` — строка переменной длины с ограничением;
+ `text` — строка произвольной длины (без ограничения).

В PostgreSQL `varchar` без указания длины и `text` абсолютно идентичны по производительности. Рекомендуется использовать `text`, а ограничения длины делать через `CHECK`-constraints.

__Дата и время:__
+ `date` — только дата (4 байта);
+ `time` — только время (8 байт);
+ `timestamp` — дата и время без часового пояса;
+ `timestamptz` (`timestamp with time zone`) — дата и время с часовым поясом;
+ `interval` — интервал времени.

```sql
-- Рекомендуется всегда использовать timestamptz
CREATE TABLE transactions (
    id bigserial PRIMARY KEY,
    amount numeric(15, 2) NOT NULL,
    created_at timestamptz DEFAULT now()
);
```

__Логический тип:__
+ `boolean` — `true`, `false`, `null`.

__UUID:__
+ `uuid` — 128-битный универсальный уникальный идентификатор. Для генерации используется расширение `gen_random_uuid()` (с PostgreSQL 13) или `uuid-ossp`.

```sql
CREATE TABLE accounts (
    id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
    owner_name text NOT NULL
);
```

__JSON и JSONB:__
+ `json` — хранит текст JSON «как есть»;
+ `jsonb` — хранит JSON в бинарном декомпозированном формате, поддерживает индексирование.

__Массивы:__
+ Любой тип данных может быть массивом: `integer[]`, `text[]` и т.д.

__Serial типы:__
+ `serial` — автоинкрементный `integer` (4 байта);
+ `bigserial` — автоинкрементный `bigint` (8 байт);
+ `smallserial` — автоинкрементный `smallint` (2 байта).

Начиная с PostgreSQL 10 рекомендуется использовать стандартный `GENERATED ALWAYS AS IDENTITY` вместо `serial`.

```sql
CREATE TABLE clients (
    id bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name text NOT NULL
);
```

[к оглавлению](#PostgreSQL)

## Что такое схемы (schemas) в PostgreSQL?

__Схема (schema)__ — это логическое пространство имён внутри базы данных, позволяющее группировать и изолировать объекты (таблицы, представления, функции, типы и т.д.).

__Ключевые особенности:__
+ Каждая база данных содержит как минимум схему `public`, которая используется по умолчанию.
+ Разные схемы могут содержать объекты с одинаковыми именами, и они не будут конфликтовать.
+ Схемы используются для логического разделения модулей приложения, управления доступом и мультитенантности.

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

__Параметр `search_path`__ определяет порядок, в котором PostgreSQL ищет объекты по неквалифицированному имени. По умолчанию: `"$user", public`.

__Применение в банковском приложении:__
```sql
CREATE SCHEMA core;       -- основные сущности (клиенты, счета)
CREATE SCHEMA payments;   -- платежи и переводы
CREATE SCHEMA audit;      -- аудит и журналирование
CREATE SCHEMA reports;    -- отчётные представления

-- Можно ограничить права:
GRANT USAGE ON SCHEMA audit TO audit_role;
REVOKE ALL ON SCHEMA core FROM public;
```

[к оглавлению](#PostgreSQL)

## Что такое последовательности (sequences) и как они работают?

__Последовательность (sequence)__ — это специальный объект базы данных, генерирующий уникальные числовые значения в возрастающем (или убывающем) порядке. Последовательности часто используются для генерации первичных ключей.

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

__Важные особенности:__
+ Последовательности работают **вне транзакций** — если транзакция откатывается, значение последовательности **не возвращается**. Поэтому в значениях могут быть пропуски (gaps), и это нормальное поведение.
+ Параметр `CACHE` позволяет каждой сессии заранее резервировать блок значений, что повышает производительность при высоком конкурентном доступе.
+ Последовательности гарантируют уникальность, но не гарантируют непрерывность.

```sql
-- Сброс последовательности
ALTER SEQUENCE order_id_seq RESTART WITH 1;

-- Привязка последовательности к столбцу
ALTER SEQUENCE order_id_seq OWNED BY orders.id;
```

[к оглавлению](#PostgreSQL)

## Чем отличаются типы serial и bigserial от явного использования последовательностей?

`serial` и `bigserial` — это **псевдотипы** (не настоящие типы данных), которые являются сокращённой записью для создания столбца с автоинкрементом.

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

__Различия serial, bigserial, smallserial:__

| Псевдотип | Реальный тип | Размер | Макс. значение |
|---|---|---|---|
| `smallserial` | `smallint` | 2 байта | 32 767 |
| `serial` | `integer` | 4 байта | 2 147 483 647 |
| `bigserial` | `bigint` | 8 байт | 9 223 372 036 854 775 807 |

__Рекомендация (PostgreSQL 10+):__ использовать `GENERATED ALWAYS AS IDENTITY` вместо `serial`, потому что:
+ это стандарт SQL;
+ запрещает явную вставку значения без `OVERRIDING SYSTEM VALUE`;
+ последовательность автоматически привязана к столбцу.

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

[к оглавлению](#PostgreSQL)

## Что такое JSONB в PostgreSQL и чем он отличается от JSON?

PostgreSQL поддерживает два типа для хранения JSON-данных:

__JSON:__
+ хранит данные как текстовую строку;
+ сохраняет форматирование, порядок ключей, дубликаты ключей;
+ при каждом обращении данные парсятся заново;
+ нельзя индексировать.

__JSONB:__
+ хранит данные в бинарном декомпозированном формате;
+ не сохраняет порядок ключей и пробелы, убирает дубликаты ключей;
+ не требует повторного парсинга — быстрее чтение;
+ поддерживает индексирование (GIN);
+ поддерживает операторы проверки вхождения (`@>`, `<@`, `?`, `?|`, `?&`).

```sql
CREATE TABLE client_settings (
    client_id bigint PRIMARY KEY,
    settings jsonb NOT NULL DEFAULT '{}'
);

INSERT INTO client_settings VALUES
(1, '{"theme": "dark", "notifications": {"email": true, "sms": false}, "limits": [1000, 5000, 10000]}');
```

__Когда использовать JSONB:__
+ для хранения полуструктурированных данных с изменяемой схемой;
+ для данных, приходящих из внешних API (ответы от платёжных систем, настройки);
+ для хранения расширяемых атрибутов (EAV-паттерн);
+ для метаданных и конфигураций.

__Когда НЕ использовать JSONB:__
+ если структура данных стабильна и известна — лучше обычные столбцы;
+ если нужны `JOIN`, `FOREIGN KEY`, агрегации по полям — реляционная модель эффективнее;
+ если данные регулярно обновляются по отдельным ключам — `UPDATE` всего JSONB-поля дороже, чем обновление столбца.

[к оглавлению](#PostgreSQL)

## Какие операторы существуют для работы с JSONB?

__Операторы извлечения данных:__

| Оператор | Описание | Пример | Результат |
|---|---|---|---|
| `->` | Получить элемент по ключу (JSON) | `'{"a":1}'::jsonb -> 'a'` | `1` (jsonb) |
| `->>` | Получить элемент по ключу (текст) | `'{"a":1}'::jsonb ->> 'a'` | `1` (text) |
| `#>` | Получить по пути (JSON) | `'{"a":{"b":2}}'::jsonb #> '{a,b}'` | `2` (jsonb) |
| `#>>` | Получить по пути (текст) | `'{"a":{"b":2}}'::jsonb #>> '{a,b}'` | `2` (text) |

__Операторы проверки:__

| Оператор | Описание | Пример |
|---|---|---|
| `@>` | Левое содержит правое | `'{"a":1,"b":2}'::jsonb @> '{"a":1}'` |
| `<@` | Левое содержится в правом | `'{"a":1}'::jsonb <@ '{"a":1,"b":2}'` |
| `?` | Содержит ключ | `'{"a":1}'::jsonb ? 'a'` |
| `?|` | Содержит хотя бы один ключ | `'{"a":1}'::jsonb ?| array['a','b']` |
| `?&` | Содержит все ключи | `'{"a":1,"b":2}'::jsonb ?& array['a','b']` |

__Операторы модификации:__

| Оператор | Описание | Пример |
|---|---|---|
| `||` | Конкатенация | `'{"a":1}'::jsonb || '{"b":2}'::jsonb` |
| `-` | Удаление ключа | `'{"a":1,"b":2}'::jsonb - 'a'` |
| `#-` | Удаление по пути | `'{"a":{"b":1}}'::jsonb #- '{a,b}'` |

__Практические примеры:__
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

[к оглавлению](#PostgreSQL)

## Как индексировать JSONB-поля?

Для эффективного поиска по JSONB существует несколько подходов.

__1. GIN-индекс на всё поле (jsonb_ops):__
```sql
CREATE INDEX idx_settings_gin ON client_settings USING GIN (settings);
```
Поддерживает операторы: `@>`, `?`, `?|`, `?&`. Это наиболее универсальный вариант.

__2. GIN-индекс с классом jsonb_path_ops:__
```sql
CREATE INDEX idx_settings_path ON client_settings USING GIN (settings jsonb_path_ops);
```
Поддерживает **только** оператор `@>`, но индекс компактнее и быстрее, чем `jsonb_ops`.

__3. B-tree индекс на конкретное выражение:__
```sql
-- Если часто ищем по конкретному ключу
CREATE INDEX idx_settings_theme ON client_settings ((settings ->> 'theme'));

-- Использование:
SELECT * FROM client_settings WHERE settings ->> 'theme' = 'dark';
```

__Сравнение подходов:__

| Подход | Размер | Операторы | Когда использовать |
|---|---|---|---|
| GIN `jsonb_ops` | Большой | `@>`, `?`, `?|`, `?&` | Поиск по произвольным ключам |
| GIN `jsonb_path_ops` | Меньше | Только `@>` | Поиск по вхождению |
| B-tree на выражении | Маленький | `=`, `<`, `>`, `LIKE` | Поиск по конкретному ключу |

```sql
-- Пример: индекс на вложенный ключ для поиска по лимитам
CREATE INDEX idx_notifications_email
ON client_settings ((settings #>> '{notifications,email}'));
```

[к оглавлению](#PostgreSQL)

## Как работать с массивами в PostgreSQL?

PostgreSQL позволяет хранить массивы значений любого типа в одном поле.

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

__Операции с массивами:__
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

__Индексирование массивов:__
```sql
-- GIN-индекс для поиска по массивам
CREATE INDEX idx_products_tags ON products USING GIN (tags);

-- После создания индекса эффективно работают: @>, &&, <@
SELECT * FROM products WHERE tags @> ARRAY['кредит', 'физлица'];
```

__Когда использовать массивы, а когда отдельную таблицу:__
+ Массивы — для небольших списков тегов/меток, которые в основном читаются целиком.
+ Отдельная таблица (many-to-many) — если нужны JOIN, агрегации, FOREIGN KEY, или если массивы большие.

[к оглавлению](#PostgreSQL)

## Какие типы индексов поддерживает PostgreSQL?

PostgreSQL поддерживает несколько типов индексов, каждый из которых оптимизирован для определённых сценариев.

__1. B-tree (по умолчанию):__
Сбалансированное дерево поиска. Используется для операций сравнения: `=`, `<`, `>`, `<=`, `>=`, `BETWEEN`, `IN`, `IS NULL`, а также для `LIKE 'prefix%'`.

```sql
CREATE INDEX idx_users_email ON users (email);
-- эквивалентно:
CREATE INDEX idx_users_email ON users USING btree (email);
```

__2. Hash:__
Хеш-таблица. Оптимизирован только для операции равенства (`=`). Занимает меньше места, чем B-tree. С PostgreSQL 10 хеш-индексы WAL-безопасны (реплицируются).

```sql
CREATE INDEX idx_users_uuid ON users USING hash (external_id);
```

__3. GIN (Generalized Inverted Index):__
Обратный (инвертированный) индекс. Используется для составных значений: массивов, JSONB, полнотекстового поиска (`tsvector`), `hstore`.

```sql
CREATE INDEX idx_doc_content ON documents USING GIN (to_tsvector('russian', content));
CREATE INDEX idx_settings ON client_settings USING GIN (settings);
```

__4. GiST (Generalized Search Tree):__
Обобщённое дерево поиска. Используется для геоданных (PostGIS), диапазонных типов (`int4range`, `tsrange`), полнотекстового поиска, нечёткого поиска (`pg_trgm`).

```sql
CREATE INDEX idx_location ON branches USING GiST (coordinates);
CREATE INDEX idx_period ON contracts USING GiST (valid_period);
```

__5. BRIN (Block Range Index):__
Хранит мин/макс значения для диапазонов физических блоков. Очень компактный. Эффективен для больших таблиц, где данные физически коррелируют с порядком индексируемого столбца (например, временные метки при последовательной вставке).

```sql
-- Идеально для таблицы логов, где данные вставляются хронологически
CREATE INDEX idx_logs_created ON audit_logs USING BRIN (created_at);
```

__6. SP-GiST (Space-Partitioned GiST):__
Для данных с естественным кластерным разбиением: IP-адреса, телефонные номера, геоданные.

```sql
CREATE INDEX idx_ip ON connections USING SPGIST (ip_address inet_ops);
```

[к оглавлению](#PostgreSQL)

## Когда какой тип индекса использовать?

| Задача | Рекомендуемый индекс |
|---|---|
| Поиск по равенству, диапазону, сортировка | **B-tree** |
| Только поиск по равенству | **Hash** |
| Поиск по массивам, JSONB, полнотекстовый поиск | **GIN** |
| Геоданные, диапазонные типы, нечёткий поиск | **GiST** |
| Большие таблицы с коррелированными данными (логи, временные ряды) | **BRIN** |
| IP-адреса, иерархические данные | **SP-GiST** |

__Примеры для банковского приложения:__

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

[к оглавлению](#PostgreSQL)

## Что такое составные индексы и как они работают?

__Составной (композитный) индекс__ — индекс, построенный по нескольким столбцам.

```sql
CREATE INDEX idx_tx_account_date ON transactions (account_id, created_at);
```

__Правило левого префикса:__ составной B-tree индекс эффективно используется, если в условии запроса участвуют столбцы, начиная с **первого** (левого) столбца индекса.

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

__Порядок столбцов имеет значение:__
+ Столбцы с высокой селективностью (больше уникальных значений) ставятся первыми.
+ Столбцы для фильтрации по равенству — перед столбцами для диапазонного поиска.

```sql
-- Оптимальный порядок для запроса:
-- WHERE status = 'ACTIVE' AND created_at BETWEEN ... AND ...
CREATE INDEX idx_tx_status_date ON transactions (status, created_at);
```

__Составной индекс для покрывающего запроса (Index Only Scan):__
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

[к оглавлению](#PostgreSQL)

## Что такое частичные индексы и индексы на выражениях?

__Частичный индекс (partial index)__ — индекс, который строится только для части строк таблицы, удовлетворяющих условию `WHERE`.

```sql
-- Индекс только по активным транзакциям
CREATE INDEX idx_tx_active ON transactions (account_id)
WHERE status = 'PENDING';

-- Значительно меньше обычного индекса, быстрее обновляется
-- Используется только если в запросе совпадает условие:
SELECT * FROM transactions
WHERE account_id = 100 AND status = 'PENDING';  -- использует idx_tx_active
```

__Типичные применения частичных индексов:__
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

__Индекс на выражении (expression index)__ — индекс, построенный на результате функции или выражения, а не на значении столбца напрямую.

```sql
-- Индекс для регистронезависимого поиска
CREATE INDEX idx_users_email_lower ON users (lower(email));

-- Используется при:
SELECT * FROM users WHERE lower(email) = 'user@bank.ru';

-- Индекс на дату из timestamp
CREATE INDEX idx_tx_date ON transactions (date(created_at));

-- Используется при:
SELECT * FROM transactions WHERE date(created_at) = '2024-06-15';

-- Индекс на извлечённое значение JSONB
CREATE INDEX idx_meta_type ON transactions ((metadata ->> 'type'));

SELECT * FROM transactions WHERE metadata ->> 'type' = 'transfer';
```

__Важно:__ запрос должен содержать **точно такое же выражение**, которое использовалось при создании индекса, иначе оптимизатор не сможет его применить.

[к оглавлению](#PostgreSQL)

## Как использовать EXPLAIN и EXPLAIN ANALYZE для анализа запросов?

`EXPLAIN` показывает **план выполнения** запроса — как PostgreSQL собирается его выполнять. `EXPLAIN ANALYZE` **фактически выполняет** запрос и показывает реальное время и количество строк.

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

-- JSON-формат (удобен для визуализаторов)
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON)
SELECT * FROM transactions WHERE account_id = 100;
```

__Пример вывода EXPLAIN ANALYZE:__
```
Index Scan using idx_tx_account on transactions  (cost=0.43..8.45 rows=1 width=64) (actual time=0.021..0.023 rows=1 loops=1)
  Index Cond: (account_id = 100)
  Buffers: shared hit=3
Planning Time: 0.085 ms
Execution Time: 0.045 ms
```

__Ключевые метрики:__
+ `cost=0.43..8.45` — оценка стоимости (начало..итого) в условных единицах;
+ `rows=1` — ожидаемое количество строк;
+ `actual time=0.021..0.023` — реальное время (мс) первой строки..последней строки;
+ `rows=1` (actual) — реальное количество строк;
+ `loops=1` — сколько раз узел выполнялся;
+ `Buffers: shared hit=3` — прочитано 3 страницы из кеша.

__ВНИМАНИЕ:__ `EXPLAIN ANALYZE` для `UPDATE`, `DELETE`, `INSERT` **выполняет** операцию! Оборачивайте в транзакцию:

```sql
BEGIN;
EXPLAIN ANALYZE UPDATE transactions SET status = 'DONE' WHERE id = 1;
ROLLBACK;
```

[к оглавлению](#PostgreSQL)

## Как читать план выполнения запроса?

План выполнения — это дерево узлов (nodes). Выполнение идёт от внутренних узлов к внешним (снизу вверх).

__Основные типы узлов сканирования:__

| Узел | Описание |
|---|---|
| `Seq Scan` | Последовательное чтение всей таблицы |
| `Index Scan` | Чтение через индекс, затем обращение к таблице за данными |
| `Index Only Scan` | Все нужные данные уже есть в индексе |
| `Bitmap Index Scan` + `Bitmap Heap Scan` | Двухфазное: сначала собирается битовая карта по индексу, затем читаются страницы |

__Типы соединений (JOIN):__

| Узел | Описание |
|---|---|
| `Nested Loop` | Для каждой строки внешнего набора ищем во внутреннем. Хорош при малом внешнем наборе |
| `Hash Join` | Строится хеш-таблица по одной стороне. Эффективен для больших наборов |
| `Merge Join` | Оба набора сортируются, затем сливаются. Эффективен, если данные уже отсортированы |

__Пример чтения сложного плана:__
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

__На что обращать внимание:__
+ Большое расхождение между `rows` (estimated) и `rows` (actual) — устаревшая статистика, нужен `ANALYZE`.
+ `Seq Scan` на большой таблице с фильтром — возможно, не хватает индекса.
+ `Rows Removed by Filter` — строки прочитаны, но отброшены. Признак неоптимального индекса.
+ `Buffers: shared read` — чтение с диска (медленно), `shared hit` — чтение из кеша.

[к оглавлению](#PostgreSQL)

## Какие основные приёмы оптимизации запросов в PostgreSQL?

__1. Создание подходящих индексов:__
```sql
-- Анализ медленного запроса
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 42 AND status = 'active';
-- Если Seq Scan — создать индекс:
CREATE INDEX idx_orders_cust_status ON orders (customer_id, status);
```

__2. Актуальность статистики:__
```sql
-- Обновить статистику по таблице
ANALYZE transactions;
-- Увеличить детализацию статистики для столбца
ALTER TABLE transactions ALTER COLUMN status SET STATISTICS 1000;
ANALYZE transactions;
```

__3. Оптимизация SELECT — выбирать только нужные столбцы:__
```sql
-- Плохо:
SELECT * FROM transactions WHERE account_id = 100;
-- Хорошо (может использовать Index Only Scan):
SELECT id, amount, created_at FROM transactions WHERE account_id = 100;
```

__4. Использование LIMIT и пагинации:__
```sql
-- Keyset pagination (эффективнее OFFSET):
SELECT * FROM transactions
WHERE account_id = 100 AND id > 1000
ORDER BY id
LIMIT 20;
```

__5. Избегать функций на индексированных столбцах:__
```sql
-- Плохо (индекс не используется):
SELECT * FROM users WHERE UPPER(email) = 'USER@BANK.RU';
-- Хорошо (создать expression index или хранить в нормализованном виде):
CREATE INDEX idx_email_upper ON users (UPPER(email));
```

__6. EXISTS вместо IN для подзапросов:__
```sql
-- Менее эффективно для больших подзапросов:
SELECT * FROM clients WHERE id IN (SELECT client_id FROM transactions WHERE amount > 1000000);
-- Более эффективно:
SELECT * FROM clients c WHERE EXISTS (
    SELECT 1 FROM transactions t WHERE t.client_id = c.id AND t.amount > 1000000
);
```

__7. Параллельное выполнение запросов:__
```sql
-- PostgreSQL может выполнять запрос параллельно
-- Настройки:
SET max_parallel_workers_per_gather = 4;
-- В EXPLAIN видно: Gather -> Parallel Seq Scan
```

__8. Материализованные представления для тяжёлых аналитических запросов:__
```sql
CREATE MATERIALIZED VIEW mv_daily_totals AS
SELECT date(created_at) AS day, sum(amount) AS total
FROM transactions GROUP BY date(created_at);

REFRESH MATERIALIZED VIEW CONCURRENTLY mv_daily_totals;
```

[к оглавлению](#PostgreSQL)

## Как работают транзакции в PostgreSQL?

__Транзакция__ — это последовательность операций, выполняемых как единое целое и удовлетворяющих свойствам ACID:
+ **Atomicity (атомарность)** — транзакция либо выполняется полностью, либо не выполняется совсем;
+ **Consistency (согласованность)** — транзакция переводит БД из одного согласованного состояния в другое;
+ **Isolation (изолированность)** — параллельные транзакции не влияют друг на друга;
+ **Durability (долговечность)** — результат зафиксированной транзакции сохраняется даже при сбое.

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

__В PostgreSQL каждый отдельный оператор — это неявная транзакция__ (autocommit). Если нужно объединить несколько операций, используется `BEGIN ... COMMIT/ROLLBACK`.

__Из Java/Spring:__
```java
@Transactional
public void transfer(Long fromId, Long toId, BigDecimal amount) {
    accountRepository.debit(fromId, amount);
    accountRepository.credit(toId, amount);
    // при исключении — автоматический ROLLBACK
}
```

[к оглавлению](#PostgreSQL)

## Какие уровни изоляции транзакций поддерживает PostgreSQL?

PostgreSQL поддерживает четыре уровня изоляции в соответствии со стандартом SQL, но реализует их через MVCC (без блокировок для чтения).

| Уровень | Грязное чтение | Неповторяемое чтение | Фантомное чтение | Аномалия сериализации |
|---|---|---|---|---|
| `READ UNCOMMITTED` | Невозможно* | Возможно | Возможно | Возможно |
| `READ COMMITTED` | Невозможно | Возможно | Возможно | Возможно |
| `REPEATABLE READ` | Невозможно | Невозможно | Невозможно** | Возможно |
| `SERIALIZABLE` | Невозможно | Невозможно | Невозможно | Невозможно |

(*) В PostgreSQL `READ UNCOMMITTED` работает идентично `READ COMMITTED` — грязное чтение невозможно благодаря MVCC.

(**) В PostgreSQL на уровне `REPEATABLE READ` фантомное чтение также невозможно (в отличие от стандарта SQL, где оно допустимо).

```sql
-- Установка уровня изоляции для транзакции
BEGIN ISOLATION LEVEL REPEATABLE READ;
SELECT * FROM accounts WHERE id = 1;  -- видим snapshot на момент BEGIN
-- Другая транзакция меняет данные и коммитит
SELECT * FROM accounts WHERE id = 1;  -- видим те же данные (snapshot)
COMMIT;

-- Установка уровня по умолчанию для сессии
SET default_transaction_isolation = 'read committed';
```

__READ COMMITTED (по умолчанию):__
+ Каждый оператор видит данные, зафиксированные до **начала этого оператора**.
+ Два одинаковых SELECT в одной транзакции могут вернуть разные результаты.

__REPEATABLE READ:__
+ Транзакция видит данные, зафиксированные до **начала транзакции**.
+ При попытке обновить строку, изменённую другой транзакцией, выбрасывается ошибка сериализации — нужно повторить транзакцию.

__SERIALIZABLE:__
+ Гарантирует, что результат параллельного выполнения транзакций эквивалентен некоторому последовательному выполнению.
+ Использует механизм Serializable Snapshot Isolation (SSI).
+ При конфликте одна из транзакций откатывается с ошибкой — приложение должно быть готово к повторной попытке.

```java
// Spring — повторная попытка при ошибке сериализации
@Transactional(isolation = Isolation.SERIALIZABLE)
@Retryable(value = CannotSerializeTransactionException.class, maxAttempts = 3)
public void criticalOperation() {
    // ...
}
```

[к оглавлению](#PostgreSQL)

## Что такое MVCC и как он реализован в PostgreSQL?

__MVCC (Multi-Version Concurrency Control)__ — механизм управления конкурентным доступом, при котором каждая транзакция видит свой «снимок» (snapshot) данных. Читатели не блокируют писателей, а писатели не блокируют читателей.

__Как PostgreSQL реализует MVCC:__

Каждая строка (tuple) в таблице содержит скрытые системные столбцы:
+ `xmin` — ID транзакции, которая **создала** эту версию строки;
+ `xmax` — ID транзакции, которая **удалила** (или обновила) эту версию (0, если строка «живая»);
+ `ctid` — физический адрес строки на странице.

__Что происходит при UPDATE:__
1. Текущая версия строки **не изменяется** — у неё устанавливается `xmax` равным ID текущей транзакции.
2. Создаётся **новая версия** строки с изменёнными данными, `xmin` = ID текущей транзакции.
3. Обе версии физически присутствуют в таблице.

```sql
-- Наглядно: смотрим системные столбцы
SELECT xmin, xmax, ctid, * FROM accounts WHERE id = 1;
-- Результат: xmin=100, xmax=0, ctid=(0,1), id=1, balance=5000

-- После UPDATE в другой транзакции (txid=200):
-- Старая версия: xmin=100, xmax=200, ctid=(0,1)
-- Новая версия:  xmin=200, xmax=0,   ctid=(0,5)
```

__Видимость строк:__
Транзакция видит строку, если:
+ `xmin` зафиксирована (committed) и < ID текущей транзакции (или это она сама);
+ `xmax` не установлен (0) или не зафиксирован, или > ID текущей транзакции.

__Последствия MVCC:__
+ При обновлении и удалении строк создаются «мёртвые» (dead) версии, которые занимают место.
+ Необходим процесс `VACUUM` для очистки мёртвых версий.
+ Индексы также содержат ссылки на все версии строк.
+ Таблицы могут «раздуваться» (table bloat) без регулярного VACUUM.

__Преимущества MVCC:__
+ Читатели не блокируют писателей — высокая производительность при конкурентном доступе.
+ Консистентные snapshot-чтения без блокировок.
+ Это фундамент всех уровней изоляции в PostgreSQL.

[к оглавлению](#PostgreSQL)

## Какие виды блокировок существуют в PostgreSQL?

PostgreSQL использует систему блокировок для координации конкурентного доступа. Блокировки делятся на несколько уровней.

__1. Строковые блокировки (row-level locks):__
Не используют память менеджера блокировок — информация хранится в самих строках (через `xmax`).

```sql
-- Явная блокировка строк
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;          -- эксклюзивная блокировка
SELECT * FROM accounts WHERE id = 1 FOR NO KEY UPDATE;   -- без блокировки FK
SELECT * FROM accounts WHERE id = 1 FOR SHARE;           -- разделяемая (для чтения)
SELECT * FROM accounts WHERE id = 1 FOR KEY SHARE;       -- мягкая разделяемая

-- FOR UPDATE SKIP LOCKED — пропустить заблокированные строки (очередь задач)
SELECT * FROM task_queue
WHERE status = 'new'
ORDER BY created_at
LIMIT 1
FOR UPDATE SKIP LOCKED;

-- FOR UPDATE NOWAIT — не ждать, сразу ошибка
SELECT * FROM accounts WHERE id = 1 FOR UPDATE NOWAIT;
```

__2. Табличные блокировки (table-level locks):__

| Режим | Конфликтует с |
|---|---|
| `ACCESS SHARE` | `ACCESS EXCLUSIVE` |
| `ROW SHARE` | `EXCLUSIVE`, `ACCESS EXCLUSIVE` |
| `ROW EXCLUSIVE` | `SHARE`, `SHARE ROW EXCLUSIVE`, `EXCLUSIVE`, `ACCESS EXCLUSIVE` |
| `SHARE` | `ROW EXCLUSIVE`, `SHARE ROW EXCLUSIVE`, `EXCLUSIVE`, `ACCESS EXCLUSIVE` |
| `ACCESS EXCLUSIVE` | Все режимы |

```sql
-- Обычные операции автоматически берут блокировки:
-- SELECT        -> ACCESS SHARE
-- INSERT/UPDATE/DELETE -> ROW EXCLUSIVE
-- CREATE INDEX  -> SHARE
-- ALTER TABLE   -> ACCESS EXCLUSIVE
-- DROP TABLE    -> ACCESS EXCLUSIVE

-- Явная блокировка таблицы
LOCK TABLE accounts IN SHARE MODE;  -- запрещает запись, разрешает чтение

-- CREATE INDEX CONCURRENTLY не блокирует запись
CREATE INDEX CONCURRENTLY idx_tx_amount ON transactions (amount);
```

__3. Мониторинг блокировок:__
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

[к оглавлению](#PostgreSQL)

## Что такое Advisory Locks и когда их использовать?

__Advisory Locks (рекомендательные блокировки)__ — это механизм блокировок на уровне приложения. PostgreSQL просто предоставляет инфраструктуру блокировок, а семантику определяет приложение. Они не связаны ни с какими объектами базы данных.

```sql
-- Блокировка на уровне сессии (удерживается до явного освобождения или отключения)
SELECT pg_advisory_lock(12345);        -- получить эксклюзивную блокировку
-- ... выполняем критическую секцию ...
SELECT pg_advisory_unlock(12345);      -- освободить

-- Блокировка на уровне транзакции (освобождается при COMMIT/ROLLBACK)
BEGIN;
SELECT pg_advisory_xact_lock(12345);
-- ... работаем ...
COMMIT;  -- блокировка автоматически освобождена

-- Неблокирующие варианты (возвращают true/false)
SELECT pg_try_advisory_lock(12345);    -- попробовать, не ждать

-- Разделяемые (shared) advisory locks
SELECT pg_advisory_lock_shared(12345);
```

__Типичные применения:__

```sql
-- 1. Предотвращение параллельного выполнения одного и того же batch-процесса
SELECT pg_try_advisory_lock(hashtext('daily_report_generation'));
-- Если вернулось true — выполняем, иначе — уже запущен

-- 2. Блокировка обработки конкретной сущности
-- Используем ID клиента как ключ блокировки
SELECT pg_advisory_xact_lock(client_id) FROM clients WHERE id = 42;
-- Теперь другая транзакция, обрабатывающая того же клиента, будет ждать

-- 3. Двухпараметровая блокировка (пространство имён)
SELECT pg_advisory_lock(1, 42);  -- (тип операции, ID сущности)
```

__Преимущества перед SELECT FOR UPDATE:__
+ Не привязаны к строкам таблицы — можно блокировать абстрактные ресурсы.
+ Быстрее для сценариев, не требующих блокировки конкретных строк.
+ Полезны для координации между различными процессами приложения.

__Важно:__ advisory locks потребляют разделяемую память, ограничение задаётся параметром `max_locks_per_transaction`.

[к оглавлению](#PostgreSQL)

## Что такое deadlock и как его избежать?

__Deadlock (взаимная блокировка)__ — ситуация, когда две или более транзакции блокируют друг друга, каждая ожидая ресурс, удерживаемый другой.

__Пример:__
```
Транзакция 1:                        Транзакция 2:
BEGIN;                               BEGIN;
UPDATE accounts SET balance = 900    UPDATE accounts SET balance = 1100
  WHERE id = 1;  -- блокирует id=1    WHERE id = 2;  -- блокирует id=2
UPDATE accounts SET balance = 1100   UPDATE accounts SET balance = 900
  WHERE id = 2;  -- ЖДЁТ id=2         WHERE id = 1;  -- ЖДЁТ id=1
-- DEADLOCK!
```

PostgreSQL **автоматически обнаруживает** deadlock (через детектор с периодом `deadlock_timeout`, по умолчанию 1 секунда) и **откатывает одну из транзакций** с ошибкой:

```
ERROR: deadlock detected
DETAIL: Process 1234 waits for ShareLock on transaction 5678;
blocked by process 9012.
```

__Способы избежать deadlock:__

__1. Единый порядок обращения к ресурсам:__
```sql
-- Всегда блокировать строки в порядке возрастания ID
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = LEAST(1, 2);
UPDATE accounts SET balance = balance + 100 WHERE id = GREATEST(1, 2);
COMMIT;
```

__2. Использовать SELECT ... FOR UPDATE с сортировкой:__
```sql
BEGIN;
-- Блокируем все нужные строки сразу в определённом порядке
SELECT * FROM accounts WHERE id IN (1, 2) ORDER BY id FOR UPDATE;
-- Теперь безопасно обновляем
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

__3. Минимизировать время транзакций:__
+ Не выполнять долгие вычисления или вызовы внешних сервисов внутри транзакции.
+ Быстро получить блокировку, выполнить операцию, зафиксировать.

__4. Использовать NOWAIT или SKIP LOCKED:__
```sql
-- Не ждать блокировку — сразу ошибка
SELECT * FROM accounts WHERE id = 1 FOR UPDATE NOWAIT;

-- Пропустить заблокированные строки
SELECT * FROM task_queue WHERE status = 'pending'
ORDER BY id LIMIT 1 FOR UPDATE SKIP LOCKED;
```

__5. Уменьшить уровень изоляции:__ `READ COMMITTED` менее склонен к deadlock, чем `SERIALIZABLE`.

__6. Настройка `deadlock_timeout`:__
```sql
-- По умолчанию 1 секунда. Уменьшение быстрее обнаруживает deadlock,
-- но увеличивает нагрузку на детектор.
SET deadlock_timeout = '500ms';
```

[к оглавлению](#PostgreSQL)

## Что такое представления (views) и материализованные представления?

__Представление (view)__ — именованный сохранённый запрос. При обращении к представлению запрос выполняется каждый раз заново.

```sql
-- Создание представления
CREATE VIEW active_clients AS
SELECT c.id, c.name, c.email, a.balance
FROM clients c
JOIN accounts a ON a.client_id = c.id
WHERE c.status = 'active' AND a.balance > 0;

-- Использование как обычной таблицы
SELECT * FROM active_clients WHERE balance > 100000;

-- Обновляемые представления (simple views)
CREATE VIEW pending_orders AS
SELECT * FROM orders WHERE status = 'pending';

-- Можно выполнять INSERT/UPDATE/DELETE (если view простой — одна таблица, без агрегаций)
UPDATE pending_orders SET status = 'processing' WHERE id = 1;

-- WITH CHECK OPTION запрещает вставку строк, не попадающих в условие view
CREATE VIEW pending_orders AS
SELECT * FROM orders WHERE status = 'pending'
WITH CHECK OPTION;
```

__Материализованное представление (materialized view)__ — это представление, результат которого физически сохраняется на диск. Запрос не выполняется при каждом обращении.

```sql
-- Создание материализованного представления
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

__Сравнение:__

| Свойство | VIEW | MATERIALIZED VIEW |
|---|---|---|
| Хранение данных | Нет (запрос каждый раз) | Да (физически на диске) |
| Актуальность | Всегда актуальные | Данные на момент REFRESH |
| Скорость чтения | Зависит от сложности запроса | Быстро |
| Индексы | Нет (используются индексы базовых таблиц) | Можно создавать свои |
| INSERT/UPDATE/DELETE | Возможно (для простых) | Нет |

__Когда использовать materialized view:__
+ Тяжёлые аналитические запросы (отчёты, дашборды);
+ Данные обновляются редко или допустима небольшая задержка;
+ Запрос читается часто, а базовые данные меняются нечасто.

[к оглавлению](#PostgreSQL)

## Что такое CTE (Common Table Expressions)?

__CTE (Common Table Expressions)__ — именованные подзапросы, определяемые в блоке `WITH`, которые можно использовать в основном запросе. Улучшают читаемость сложных запросов.

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

__Рекурсивный CTE:__
```sql
-- Иерархия подразделений банка
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

__CTE с модификацией данных:__
```sql
-- Удалить устаревшие записи и вернуть количество
WITH deleted AS (
    DELETE FROM sessions
    WHERE expires_at < now()
    RETURNING *
)
SELECT count(*) AS deleted_count FROM deleted;

-- INSERT с возвратом и дальнейшим использованием
WITH new_account AS (
    INSERT INTO accounts (client_id, balance, currency)
    VALUES (42, 0, 'RUB')
    RETURNING id, client_id
)
INSERT INTO account_history (account_id, event, created_at)
SELECT id, 'CREATED', now() FROM new_account;
```

__Важно о производительности:__
+ До PostgreSQL 12 CTE были «барьером оптимизации» — планировщик не мог протолкнуть условия из внешнего запроса внутрь CTE.
+ Начиная с PostgreSQL 12 CTE по умолчанию инлайнятся (встраиваются) в основной запрос, если используются один раз и не имеют побочных эффектов.
+ Чтобы принудительно материализовать CTE: `WITH cte AS MATERIALIZED (...)`.
+ Чтобы принудительно инлайнить: `WITH cte AS NOT MATERIALIZED (...)`.

[к оглавлению](#PostgreSQL)

## Как работают оконные функции (window functions)?

__Оконные функции__ выполняют вычисления над набором строк, связанных с текущей строкой, без группировки (в отличие от `GROUP BY` строки не схлопываются).

__Синтаксис:__
```sql
функция() OVER (
    [PARTITION BY столбец1, столбец2, ...]   -- разбивка на группы (окна)
    [ORDER BY столбец3 [ASC|DESC], ...]      -- сортировка внутри окна
    [ROWS|RANGE BETWEEN ... AND ...]         -- рамка окна
)
```

__Пример — нумерация и сумма:__
```sql
SELECT
    id,
    account_id,
    amount,
    created_at,
    -- Номер транзакции внутри каждого счёта
    ROW_NUMBER() OVER (PARTITION BY account_id ORDER BY created_at) AS tx_number,
    -- Нарастающий итог по счёту
    SUM(amount) OVER (PARTITION BY account_id ORDER BY created_at) AS running_total,
    -- Общая сумма по счёту
    SUM(amount) OVER (PARTITION BY account_id) AS account_total
FROM transactions;
```

__Рамка окна (frame):__
```sql
-- Скользящее среднее за 3 записи
SELECT
    id,
    amount,
    AVG(amount) OVER (
        ORDER BY created_at
        ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
    ) AS moving_avg
FROM transactions;

-- Нарастающий итог (по умолчанию при ORDER BY)
SUM(amount) OVER (ORDER BY created_at)
-- Эквивалентно:
SUM(amount) OVER (ORDER BY created_at ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
```

__Именованное окно (WINDOW):__
```sql
SELECT
    id,
    amount,
    ROW_NUMBER() OVER w AS rn,
    SUM(amount) OVER w AS running_total,
    AVG(amount) OVER w AS running_avg
FROM transactions
WINDOW w AS (PARTITION BY account_id ORDER BY created_at);
```

[к оглавлению](#PostgreSQL)

## Какие основные оконные функции существуют в PostgreSQL?

__Ранжирующие функции:__

```sql
SELECT
    name, department, salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,     -- 1,2,3,4,5
    RANK()       OVER (ORDER BY salary DESC) AS rank,         -- 1,2,2,4,5  (пропуск)
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank,   -- 1,2,2,3,4  (без пропуска)
    NTILE(4)     OVER (ORDER BY salary DESC) AS quartile      -- разбивка на 4 группы
FROM employees;
```

| Функция | Описание | Одинаковые значения |
|---|---|---|
| `ROW_NUMBER()` | Уникальный номер строки | Разные номера |
| `RANK()` | Ранг с пропусками | Одинаковый ранг, следующий с пропуском |
| `DENSE_RANK()` | Ранг без пропусков | Одинаковый ранг, следующий без пропуска |
| `NTILE(n)` | Разбивка на n групп | Равномерное распределение |

__Функции смещения:__

```sql
SELECT
    id,
    amount,
    created_at,
    -- Предыдущая транзакция
    LAG(amount, 1) OVER (ORDER BY created_at) AS prev_amount,
    -- Следующая транзакция
    LEAD(amount, 1) OVER (ORDER BY created_at) AS next_amount,
    -- Первая в окне
    FIRST_VALUE(amount) OVER (PARTITION BY account_id ORDER BY created_at) AS first_tx,
    -- Последняя в окне
    LAST_VALUE(amount) OVER (
        PARTITION BY account_id ORDER BY created_at
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS last_tx
FROM transactions;
```

__Практические задачи:__

```sql
-- Топ-3 транзакции по каждому счёту
WITH ranked AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY account_id ORDER BY amount DESC) AS rn
    FROM transactions
)
SELECT * FROM ranked WHERE rn <= 3;

-- Разница с предыдущей транзакцией
SELECT
    id,
    amount,
    amount - LAG(amount) OVER (ORDER BY created_at) AS diff_with_prev,
    created_at - LAG(created_at) OVER (ORDER BY created_at) AS time_gap
FROM transactions
WHERE account_id = 100;

-- Процент от общей суммы
SELECT
    account_id,
    amount,
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

[к оглавлению](#PostgreSQL)

## Что такое триггеры в PostgreSQL?

__Триггер (trigger)__ — это именованный объект базы данных, который автоматически выполняет функцию при наступлении определённого события (INSERT, UPDATE, DELETE, TRUNCATE) на таблице.

__Виды триггеров:__
+ `BEFORE` — выполняется до операции (может изменить данные или отменить операцию);
+ `AFTER` — выполняется после операции (данные уже изменены);
+ `INSTEAD OF` — заменяет операцию (только для представлений);
+ `FOR EACH ROW` — для каждой строки;
+ `FOR EACH STATEMENT` — один раз на весь оператор.

```sql
-- 1. Создаём триггерную функцию
CREATE OR REPLACE FUNCTION update_modified_timestamp()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = now();
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- 2. Привязываем триггер к таблице
CREATE TRIGGER trg_accounts_updated
    BEFORE UPDATE ON accounts
    FOR EACH ROW
    EXECUTE FUNCTION update_modified_timestamp();
```

__Пример: аудит изменений:__
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

__Специальные переменные в триггерных функциях:__
+ `NEW` — новая строка (INSERT, UPDATE);
+ `OLD` — старая строка (UPDATE, DELETE);
+ `TG_OP` — тип операции ('INSERT', 'UPDATE', 'DELETE', 'TRUNCATE');
+ `TG_TABLE_NAME` — имя таблицы;
+ `TG_WHEN` — 'BEFORE' или 'AFTER'.

__Условный триггер (PostgreSQL 9.0+):__
```sql
CREATE TRIGGER trg_large_transactions
    AFTER INSERT ON transactions
    FOR EACH ROW
    WHEN (NEW.amount > 1000000)
    EXECUTE FUNCTION notify_compliance_department();
```

[к оглавлению](#PostgreSQL)

## Чем отличаются функции от процедур в PostgreSQL?

Начиная с PostgreSQL 11 поддерживаются как функции (`FUNCTION`), так и процедуры (`PROCEDURE`).

__Функции (FUNCTION):__
```sql
CREATE OR REPLACE FUNCTION get_account_balance(p_account_id bigint)
RETURNS numeric AS $$
DECLARE
    v_balance numeric;
BEGIN
    SELECT balance INTO v_balance
    FROM accounts
    WHERE id = p_account_id;

    IF NOT FOUND THEN
        RAISE EXCEPTION 'Счёт % не найден', p_account_id;
    END IF;

    RETURN v_balance;
END;
$$ LANGUAGE plpgsql;

-- Вызов
SELECT get_account_balance(1);

-- Функция, возвращающая набор строк
CREATE OR REPLACE FUNCTION get_client_transactions(
    p_client_id bigint,
    p_limit int DEFAULT 10
)
RETURNS TABLE(tx_id bigint, amount numeric, created_at timestamptz) AS $$
BEGIN
    RETURN QUERY
    SELECT t.id, t.amount, t.created_at
    FROM transactions t
    JOIN accounts a ON a.id = t.account_id
    WHERE a.client_id = p_client_id
    ORDER BY t.created_at DESC
    LIMIT p_limit;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM get_client_transactions(42, 5);
```

__Процедуры (PROCEDURE):__
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

-- Вызов процедуры
CALL transfer_funds(1, 2, 50000);
```

__Ключевые отличия:__

| Свойство | FUNCTION | PROCEDURE |
|---|---|---|
| Возвращает значение | Да (RETURNS) | Нет |
| Управление транзакциями | Нет (COMMIT/ROLLBACK запрещены) | Да (COMMIT/ROLLBACK разрешены) |
| Использование в SELECT | `SELECT func()` | Нет |
| Вызов | `SELECT func()` или в выражениях | `CALL proc()` |
| Используется в триггерах | Да | Нет |

Процедуры полезны для длительных пакетных операций, где нужно промежуточно фиксировать результаты (например, миграция данных большими батчами).

[к оглавлению](#PostgreSQL)

## Что такое партиционирование (partitioning) таблиц?

__Партиционирование__ — разбиение большой таблицы на несколько физических частей (партиций), при этом логически они остаются единой таблицей. Повышает производительность запросов и упрощает обслуживание.

С PostgreSQL 10 поддерживается __декларативное партиционирование__.

__1. RANGE-партиционирование (по диапазону):__
```sql
-- Партиционирование транзакций по месяцам
CREATE TABLE transactions (
    id bigserial,
    account_id bigint NOT NULL,
    amount numeric(15, 2) NOT NULL,
    created_at timestamptz NOT NULL,
    PRIMARY KEY (id, created_at)
) PARTITION BY RANGE (created_at);

-- Создание партиций
CREATE TABLE transactions_2024_01 PARTITION OF transactions
    FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');
CREATE TABLE transactions_2024_02 PARTITION OF transactions
    FOR VALUES FROM ('2024-02-01') TO ('2024-03-01');
CREATE TABLE transactions_2024_03 PARTITION OF transactions
    FOR VALUES FROM ('2024-03-01') TO ('2024-04-01');

-- Партиция по умолчанию (для строк, не попавших ни в одну партицию)
CREATE TABLE transactions_default PARTITION OF transactions DEFAULT;
```

__2. LIST-партиционирование (по списку значений):__
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

__3. HASH-партиционирование (по хешу):__
```sql
CREATE TABLE session_data (
    session_id uuid NOT NULL,
    data jsonb,
    PRIMARY KEY (session_id)
) PARTITION BY HASH (session_id);

CREATE TABLE session_data_0 PARTITION OF session_data
    FOR VALUES WITH (MODULUS 4, REMAINDER 0);
CREATE TABLE session_data_1 PARTITION OF session_data
    FOR VALUES WITH (MODULUS 4, REMAINDER 1);
CREATE TABLE session_data_2 PARTITION OF session_data
    FOR VALUES WITH (MODULUS 4, REMAINDER 2);
CREATE TABLE session_data_3 PARTITION OF session_data
    FOR VALUES WITH (MODULUS 4, REMAINDER 3);
```

__Преимущества партиционирования:__
+ __Partition pruning__ — PostgreSQL сканирует только нужные партиции.
+ Быстрое удаление старых данных: `DROP TABLE transactions_2023_01` вместо `DELETE`.
+ Индексы меньше — быстрее обслуживаются.
+ `VACUUM` работает на отдельных партициях.

```sql
-- PostgreSQL автоматически выбирает нужную партицию
EXPLAIN SELECT * FROM transactions WHERE created_at = '2024-02-15';
-- Покажет сканирование только transactions_2024_02

-- Отсоединение партиции (для архивации)
ALTER TABLE transactions DETACH PARTITION transactions_2024_01;

-- Присоединение существующей таблицы как партиции
ALTER TABLE transactions ATTACH PARTITION transactions_2024_04
    FOR VALUES FROM ('2024-04-01') TO ('2024-05-01');
```

__Рекомендации:__
+ Партиционирование имеет смысл для таблиц от нескольких миллионов строк.
+ Ключ партиционирования должен входить в PRIMARY KEY.
+ Индексы создаются на каждой партиции отдельно (или автоматически, если создать на родительской таблице).

[к оглавлению](#PostgreSQL)

## Какие виды репликации поддерживает PostgreSQL?

PostgreSQL поддерживает два основных вида репликации: __потоковую (streaming)__ и __логическую (logical)__.

__1. Потоковая (физическая) репликация:__

Реплика получает поток WAL-записей (Write-Ahead Log) от мастера и применяет их побайтово. Реплика — полная копия мастера.

```
Мастер (Primary) ---> WAL stream ---> Реплика (Standby)
```

__Характеристики:__
+ Реплицируется **вся** база данных целиком (нельзя выбрать отдельные таблицы);
+ Реплика может быть «горячей» (hot standby) — принимает запросы на чтение;
+ Синхронная или асинхронная репликация;
+ Поддержка каскадной репликации (replica -> replica);
+ Используется для отказоустойчивости (failover) и масштабирования чтения.

```ini
# postgresql.conf на мастере
wal_level = replica
max_wal_senders = 5
synchronous_standby_names = 'replica1'

# postgresql.conf на реплике
hot_standby = on
primary_conninfo = 'host=master port=5432 user=replicator'
```

__Синхронная vs асинхронная:__
+ **Асинхронная** (по умолчанию) — мастер не ждёт подтверждения от реплики. Возможна потеря данных при падении мастера.
+ **Синхронная** — мастер ждёт подтверждения от реплики перед COMMIT. Нулевая потеря данных, но выше задержка записи.

__2. Логическая репликация (PostgreSQL 10+):__

Реплицируются логические изменения (INSERT, UPDATE, DELETE) на уровне отдельных таблиц через механизм публикаций/подписок.

```sql
-- На мастере (publisher)
CREATE PUBLICATION payments_pub FOR TABLE transactions, accounts;

-- На подписчике (subscriber)
CREATE SUBSCRIPTION payments_sub
    CONNECTION 'host=master port=5432 dbname=bank user=replicator'
    PUBLICATION payments_pub;
```

__Характеристики логической репликации:__
+ Можно реплицировать **отдельные таблицы**;
+ Можно реплицировать между **разными версиями** PostgreSQL;
+ Подписчик может иметь собственные индексы, триггеры;
+ Подписчик может быть **доступен для записи**;
+ Используется для миграций, агрегации данных из нескольких источников, обновления версии PostgreSQL с минимальным простоем.

__Сравнение:__

| Свойство | Потоковая | Логическая |
|---|---|---|
| Единица репликации | Вся БД | Таблицы |
| Формат | Физический (WAL-байты) | Логический (SQL-операции) |
| Запись на реплике | Нет | Да |
| Разные версии PG | Нет | Да |
| DDL-репликация | Да | Нет |
| Применение | Failover, чтение | Миграция, частичная репликация |

[к оглавлению](#PostgreSQL)

## Зачем нужны VACUUM и AUTOVACUUM?

Из-за MVCC при UPDATE и DELETE старые версии строк не удаляются физически — они помечаются как «мёртвые» (dead tuples). __VACUUM__ — это процесс очистки мёртвых версий строк и возврата освобождённого пространства.

__Что делает VACUUM:__
1. Помечает пространство от мёртвых строк как доступное для повторного использования (но **не возвращает** его ОС).
2. Обновляет карту видимости (visibility map) — ускоряет Index Only Scan.
3. Обновляет карту свободного пространства (free space map).
4. Предотвращает переполнение счётчика транзакций (transaction ID wraparound).

```sql
-- Ручной VACUUM
VACUUM transactions;

-- VACUUM с подробностями
VACUUM VERBOSE transactions;

-- VACUUM + ANALYZE (обновить статистику)
VACUUM ANALYZE transactions;

-- VACUUM FULL — перестраивает таблицу полностью, возвращает место ОС
-- ВНИМАНИЕ: блокирует таблицу на всё время выполнения (ACCESS EXCLUSIVE)!
VACUUM FULL transactions;
```

__AUTOVACUUM__ — фоновый процесс, который автоматически выполняет VACUUM и ANALYZE.

```sql
-- Мониторинг: когда последний раз выполнялся автовакуум
SELECT
    relname,
    n_dead_tup,
    n_live_tup,
    last_vacuum,
    last_autovacuum,
    last_analyze,
    last_autoanalyze
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC;
```

__Настройка AUTOVACUUM:__
```sql
-- Глобальные параметры (postgresql.conf)
-- autovacuum = on                          -- включён по умолчанию
-- autovacuum_vacuum_threshold = 50         -- мин. кол-во мёртвых строк
-- autovacuum_vacuum_scale_factor = 0.2     -- 20% от размера таблицы
-- autovacuum_naptime = 60                  -- интервал проверки (секунды)

-- Формула запуска: dead_tuples > threshold + scale_factor * n_live_tup

-- Настройка для конкретной таблицы (горячие таблицы)
ALTER TABLE transactions SET (
    autovacuum_vacuum_scale_factor = 0.01,     -- 1% вместо 20%
    autovacuum_vacuum_threshold = 1000,
    autovacuum_analyze_scale_factor = 0.005
);
```

__Transaction ID Wraparound:__
PostgreSQL использует 32-битный счётчик транзакций (~4 млрд). VACUUM **обязан** выполняться периодически, чтобы «заморозить» (freeze) старые транзакции и предотвратить переполнение. Если автовакуум не справляется, PostgreSQL может **остановить** приём новых транзакций для защиты данных.

```sql
-- Проверить возраст самой старой незамороженной транзакции
SELECT datname, age(datfrozenxid) FROM pg_database ORDER BY age DESC;
-- Если age приближается к 2 млрд — критическая ситуация!
```

__Никогда не отключайте AUTOVACUUM!__ Вместо этого настройте его параметры для конкретных таблиц.

[к оглавлению](#PostgreSQL)

## Как мониторить производительность PostgreSQL?

__1. pg_stat_statements — статистика выполнения запросов:__

```sql
-- Включение расширения
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
-- В postgresql.conf: shared_preload_libraries = 'pg_stat_statements'

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
    calls,
    shared_blks_hit,
    shared_blks_read,
    round(100.0 * shared_blks_hit / NULLIF(shared_blks_hit + shared_blks_read, 0), 2) AS cache_hit_pct
FROM pg_stat_statements
ORDER BY shared_blks_read DESC
LIMIT 10;

-- Сброс статистики
SELECT pg_stat_statements_reset();
```

__2. pg_stat_user_tables — статистика по таблицам:__
```sql
SELECT
    relname AS table_name,
    seq_scan,            -- количество Seq Scan (много — нужен индекс?)
    idx_scan,            -- количество Index Scan
    n_tup_ins AS inserts,
    n_tup_upd AS updates,
    n_tup_del AS deletes,
    n_dead_tup AS dead_tuples,
    last_autovacuum
FROM pg_stat_user_tables
ORDER BY seq_scan DESC;
```

__3. pg_stat_user_indexes — использование индексов:__
```sql
-- Неиспользуемые индексы (кандидаты на удаление)
SELECT
    schemaname, tablename, indexrelname,
    idx_scan,          -- 0 = не используется
    pg_size_pretty(pg_relation_size(indexrelid)) AS index_size
FROM pg_stat_user_indexes
WHERE idx_scan = 0
ORDER BY pg_relation_size(indexrelid) DESC;
```

__4. pg_stat_activity — текущие сессии и запросы:__
```sql
-- Активные запросы прямо сейчас
SELECT
    pid, usename, state,
    now() - query_start AS duration,
    wait_event_type, wait_event,
    substr(query, 1, 100) AS query
FROM pg_stat_activity
WHERE state = 'active' AND pid != pg_backend_pid()
ORDER BY duration DESC;

-- Завершить зависший запрос
SELECT pg_cancel_backend(pid);     -- мягкое завершение (SIGINT)
SELECT pg_terminate_backend(pid);  -- жёсткое завершение (SIGTERM)
```

__5. Мониторинг размера таблиц и индексов:__
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

__6. Cache hit ratio (эффективность кеширования):__
```sql
SELECT
    sum(blks_hit) * 100.0 / NULLIF(sum(blks_hit) + sum(blks_read), 0) AS cache_hit_ratio
FROM pg_stat_database;
-- Хорошее значение: > 99%
```

[к оглавлению](#PostgreSQL)

## Как подключить PostgreSQL к Java/Spring приложению?

__1. Подключение через JDBC:__

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.3</version>
</dependency>
```

```java
// Прямое подключение через JDBC
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

__2. Подключение через Spring Boot:__

```yaml
# application.yml
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

__3. Работа с типами PostgreSQL через JDBC:__

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

// Timestamp with time zone
ps.setTimestamp(1, Timestamp.from(Instant.now()));
```

__4. Использование с JPA/Hibernate:__

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
    // (например, vladmihalcea hibernate-types)
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

[к оглавлению](#PostgreSQL)

## Как настроить пул соединений HikariCP для PostgreSQL?

__HikariCP__ — высокопроизводительный пул соединений для JDBC, используемый по умолчанию в Spring Boot.

__Зачем нужен пул соединений:__
+ Создание TCP-соединения с PostgreSQL дорого (~5-10 мс).
+ Каждое соединение занимает ~10 МБ RAM на стороне PostgreSQL (work_mem, temp_buffers и пр.).
+ Пул переиспользует готовые соединения, снижая задержки и нагрузку.

```yaml
# application.yml — настройки HikariCP
spring:
  datasource:
    hikari:
      # Максимальный размер пула
      maximum-pool-size: 20
      # Минимальное количество неактивных соединений
      minimum-idle: 5
      # Таймаут ожидания соединения из пула (мс)
      connection-timeout: 30000
      # Время жизни неактивного соединения (мс)
      idle-timeout: 600000
      # Максимальное время жизни соединения (мс)
      max-lifetime: 1800000
      # Имя пула (для мониторинга)
      pool-name: BankAppPool
      # Тестовый запрос (для PostgreSQL необязателен, JDBC4 isValid())
      # connection-test-query: SELECT 1
      # Свойства PostgreSQL JDBC-драйвера
      data-source-properties:
        prepareThreshold: 5
        preparedStatementCacheQueries: 256
        preparedStatementCacheSizeMiB: 5
```

__Формула размера пула:__

Оптимальный размер пула обычно значительно меньше, чем ожидают. Формула от авторов HikariCP:

```
pool_size = количество_ядер_CPU * 2 + количество_дисков
```

Для сервера с 4 ядрами и 1 SSD: `4 * 2 + 1 = 9` соединений. Этого достаточно для тысяч запросов в секунду.

__Мониторинг HikariCP:__

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

Ключевые метрики для мониторинга:
+ `hikaricp.connections.active` — активные соединения;
+ `hikaricp.connections.idle` — простаивающие соединения;
+ `hikaricp.connections.pending` — потоки, ожидающие соединение (если > 0, пул мал);
+ `hikaricp.connections.timeout.total` — количество таймаутов получения соединения.

__Типичные проблемы:__
+ `connection-timeout` срабатывает — пул исчерпан: либо увеличить пул, либо оптимизировать время транзакций.
+ `max-lifetime` должен быть **меньше** `idle_in_transaction_session_timeout` на PostgreSQL и меньше таймаута файервола/балансировщика.
+ Утечка соединений (connection leak): включить `leak-detection-threshold: 60000` для обнаружения.

```yaml
spring:
  datasource:
    hikari:
      leak-detection-threshold: 60000  # предупреждение, если соединение удерживается > 60 сек
```

[к оглавлению](#PostgreSQL)
