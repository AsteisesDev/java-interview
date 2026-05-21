[Вопросы для собеседования](README.md)

# REST API

+ [Что такое REST?](#что-такое-rest)
+ [Что такое RESTful?](#что-такое-restful)
+ [Какие ограничения (constraints) определяет REST?](#какие-ограничения-constraints-определяет-rest)
+ [Что такое ресурс в REST?](#что-такое-ресурс-в-rest)
+ [Какие HTTP-методы используются в REST и какова их семантика?](#какие-http-методы-используются-в-rest-и-какова-их-семантика)
+ [Что такое идемпотентность и безопасность HTTP-методов?](#что-такое-идемпотентность-и-безопасность-http-методов)
+ [В чём разница между PUT и POST?](#в-чём-разница-между-put-и-post)
+ [В чём разница между PUT и PATCH?](#в-чём-разница-между-put-и-patch)
+ [Какие существуют коды HTTP-ответов и когда их использовать?](#какие-существуют-коды-http-ответов-и-когда-их-использовать)
+ [Каковы лучшие практики проектирования URI?](#каковы-лучшие-практики-проектирования-uri)
+ [Как версионировать REST API?](#как-версионировать-rest-api)
+ [Что такое Content Negotiation?](#что-такое-content-negotiation)
+ [Что такое HATEOAS?](#что-такое-hateoas)
+ [В чём отличия REST от SOAP?](#в-чём-отличия-rest-от-soap)
+ [Что такое Richardson Maturity Model?](#что-такое-richardson-maturity-model)
+ [Как реализовать пагинацию, фильтрацию и сортировку в REST API?](#как-реализовать-пагинацию-фильтрацию-и-сортировку-в-rest-api)
+ [Как обрабатывать ошибки в REST API?](#как-обрабатывать-ошибки-в-rest-api)
+ [Какие способы аутентификации используются в REST API?](#какие-способы-аутентификации-используются-в-rest-api)
+ [Что такое OAuth 2.0 и как он работает?](#что-такое-oauth-20-и-как-он-работает)
+ [Как работает кэширование в REST?](#как-работает-кэширование-в-rest)
+ [Что такое CORS и как его настроить в Spring?](#что-такое-cors-и-как-его-настроить-в-spring)
+ [Как документировать REST API с помощью Swagger/OpenAPI?](#как-документировать-rest-api-с-помощью-swaggeropenapi)
+ [Как реализовать REST API в Spring?](#как-реализовать-rest-api-в-spring)
+ [Что такое ResponseEntity и зачем он нужен?](#что-такое-responseentity-и-зачем-он-нужен)
+ [Что такое Rate Limiting и как его реализовать?](#что-такое-rate-limiting-и-как-его-реализовать)
+ [Как обеспечить безопасность REST API?](#как-обеспечить-безопасность-rest-api)
+ [Что такое идемпотентный ключ (Idempotency Key)?](#что-такое-идемпотентный-ключ-idempotency-key)
+ [Как проектировать REST API для связанных ресурсов?](#как-проектировать-rest-api-для-связанных-ресурсов)
+ [Что такое подход API Design First?](#что-такое-подход-api-design-first)
+ [Чем отличаются REST, GraphQL и gRPC? Когда что использовать?](#чем-отличаются-rest-graphql-и-grpc-когда-что-использовать)
+ [Как обеспечить обратную совместимость REST API?](#как-обеспечить-обратную-совместимость-rest-api)
+ [Что такое асинхронные API и как их проектировать?](#что-такое-асинхронные-api-и-как-их-проектировать)
+ [Что такое BFF (Backend for Frontend) и API Gateway?](#что-такое-bff-backend-for-frontend-и-api-gateway)
+ [Как проектировать API для микросервисной архитектуры?](#как-проектировать-api-для-микросервисной-архитектуры)

## Что такое REST?
<!-- grade: junior -->

REST (Representational State Transfer) — архитектурный стиль взаимодействия компонентов распределённой системы, предложенный Роем Филдингом (Roy Fielding) в 2000 году в его докторской диссертации.

REST не является протоколом или стандартом — это набор архитектурных принципов (ограничений), которым должна соответствовать система. REST основан на использовании протокола HTTP и описывает способ взаимодействия клиента и сервера через обмен представлениями ресурсов.

### Ключевые идеи REST

- Всё является ресурсом (пользователь, заказ, товар и т.д.).
- Каждый ресурс имеет уникальный идентификатор (URI).
- Взаимодействие с ресурсами происходит через стандартные HTTP-методы (GET, POST, PUT, DELETE и др.).
- Ресурс может иметь различные представления (JSON, XML и др.).
- Взаимодействие не хранит состояния (stateless) — каждый запрос содержит всю необходимую информацию.

> **На собеседовании:** интервьюер хочет услышать, что REST — это не протокол, а архитектурный стиль, и что ключевые идеи — ресурсы, URI, HTTP-методы, stateless. Частая ошибка — путать REST с HTTP или считать его конкретной технологией.

[к оглавлению](#rest-api)

---

## Что такое RESTful?
<!-- grade: junior -->

RESTful — прилагательное, описывающее систему (обычно веб-сервис или API), которая полностью соответствует принципам и ограничениям архитектурного стиля REST.

| Термин | Значение |
|--------|----------|
| REST | Архитектурный стиль (набор принципов) |
| RESTful | Конкретная реализация, следующая этим принципам |

Говоря «RESTful API», мы подразумеваем API, которое:

- использует HTTP-методы по назначению;
- работает с ресурсами через URI;
- обменивается данными в стандартных форматах (JSON, XML);
- является stateless;
- использует стандартные коды HTTP-ответов.

На практике большинство так называемых «REST API» не являются полностью RESTful (например, не реализуют HATEOAS), а представляют собой «REST-like» или «HTTP API».

> **На собеседовании:** достаточно чётко разграничить REST (стиль) и RESTful (реализация). Бонус — упомянуть, что большинство реальных API — это «REST-like», а не полностью RESTful, потому что не реализуют HATEOAS.

[к оглавлению](#rest-api)

---

## Какие ограничения (constraints) определяет REST?
<!-- grade: junior -->

REST определяет шесть архитектурных ограничений, пять из которых обязательны и одно опциональное:

1. **Клиент-сервер (Client-Server)** — разделение ответственности: клиент отвечает за пользовательский интерфейс, сервер — за хранение и обработку данных. Это позволяет независимо развивать клиентскую и серверную части.

2. **Отсутствие состояния (Stateless)** — каждый запрос от клиента к серверу должен содержать всю информацию, необходимую для обработки. Сервер не хранит состояния клиента между запросами. Сессия хранится на стороне клиента (например, в виде токена).
   - упрощается масштабирование (любой сервер может обработать любой запрос);
   - повышается надёжность;
   - упрощается мониторинг.

3. **Кэширование (Cacheable)** — ответы сервера должны явно или неявно указывать, могут ли они быть кэшированы. Правильное кэширование снижает нагрузку на сервер и улучшает производительность клиента.

4. **Единообразие интерфейса (Uniform Interface)** — ключевое ограничение REST, включающее четыре подпринципа:
   - Идентификация ресурсов — каждый ресурс идентифицируется через URI.
   - Манипуляция ресурсами через представления — клиент работает с представлениями ресурсов (JSON, XML), а не с самими ресурсами напрямую.
   - Самоописательные сообщения — каждое сообщение содержит достаточно информации для его обработки (Content-Type, метод, заголовки).
   - HATEOAS (Hypermedia as the Engine of Application State) — клиент взаимодействует с приложением полностью через гипермедиа, предоставляемую сервером.

5. **Многоуровневая система (Layered System)** — архитектура может состоять из нескольких уровней (прокси, балансировщики, шлюзы). Клиент не знает, взаимодействует ли он напрямую с сервером или с промежуточным уровнем.

6. **Код по требованию (Code on Demand) — опционально** — сервер может расширять функциональность клиента, передавая исполняемый код (например, JavaScript). Это единственное опциональное ограничение.

> **На собеседовании:** нужно перечислить все шесть ограничений и отметить, что Code on Demand — опциональное. Частая ошибка — забыть про Uniform Interface или не раскрыть его четыре подпринципа.

[к оглавлению](#rest-api)

---

## Что такое ресурс в REST?
<!-- grade: junior -->

Ресурс — ключевая абстракция в REST. Ресурсом может быть любая именованная информация: документ, изображение, коллекция других ресурсов, временная сущность (например, текущая погода) и т.д.

Каждый ресурс идентифицируется одним или более URI (Uniform Resource Identifier):
```
/api/users          — коллекция пользователей
/api/users/42       — конкретный пользователь с id=42
/api/users/42/orders — заказы пользователя с id=42
```

### Типы ресурсов

| Тип | Описание | Пример URI |
|-----|----------|------------|
| Коллекция (Collection) | Набор ресурсов одного типа | `/api/users` |
| Элемент (Document) | Единичный ресурс | `/api/users/42` |
| Хранилище (Store) | Управляемое клиентом хранилище ресурсов | `/api/users/42/favorites` |
| Контроллер (Controller) | Процедурная концепция для действий вне CRUD | `/api/users/42/activate` |

Представление ресурса — это конкретное отображение состояния ресурса в определённом формате (JSON, XML, HTML). Один и тот же ресурс может иметь разные представления в зависимости от заголовка `Accept`.

```json
{
  "id": 42,
  "name": "Иван Петров",
  "email": "ivan@example.com"
}
```

> **На собеседовании:** важно объяснить разницу между ресурсом и его представлением. Ресурс — это абстракция (пользователь), а представление — конкретное отображение в формате JSON/XML. Частая ошибка — считать, что ресурс = запись в базе данных.

[к оглавлению](#rest-api)

---

## Какие HTTP-методы используются в REST и какова их семантика?
<!-- grade: junior -->

В REST используются стандартные HTTP-методы для операций над ресурсами:

| Метод | CRUD-операция | Описание | Идемпотентный | Безопасный |
|-------|--------------|----------|:-------------:|:----------:|
| GET | Read | Получение ресурса | Да | Да |
| POST | Create | Создание нового ресурса | Нет | Нет |
| PUT | Update/Replace | Полная замена ресурса | Да | Нет |
| PATCH | Update/Modify | Частичное обновление ресурса | Нет* | Нет |
| DELETE | Delete | Удаление ресурса | Да | Нет |
| OPTIONS | -- | Получение допустимых методов | Да | Да |
| HEAD | -- | Аналог GET, но без тела ответа | Да | Да |

*PATCH может быть идемпотентным в зависимости от реализации.

<details><summary>Примеры HTTP-запросов для каждого метода</summary>

GET — получает представление ресурса. Не должен изменять состояние сервера.
```
GET /api/users/42 HTTP/1.1
Accept: application/json
```

POST — создаёт новый ресурс. URI нового ресурса определяется сервером.
```
POST /api/users HTTP/1.1
Content-Type: application/json

{"name": "Иван", "email": "ivan@example.com"}
```

PUT — заменяет ресурс целиком. Если ресурс не существует, может создать его.
```
PUT /api/users/42 HTTP/1.1
Content-Type: application/json

{"name": "Иван Петров", "email": "ivan@example.com", "age": 30}
```

PATCH — частично обновляет ресурс. Отправляются только изменяемые поля.
```
PATCH /api/users/42 HTTP/1.1
Content-Type: application/json

{"email": "new-email@example.com"}
```

DELETE — удаляет ресурс.
```
DELETE /api/users/42 HTTP/1.1
```

OPTIONS — возвращает допустимые методы для ресурса. Используется в CORS-запросах (preflight).
```
OPTIONS /api/users HTTP/1.1
```

HEAD — аналогичен GET, но возвращает только заголовки без тела ответа. Используется для проверки существования ресурса или получения метаданных.
```
HEAD /api/users/42 HTTP/1.1
```

</details>

> **На собеседовании:** интервьюер ожидает знания всех основных методов с их свойствами идемпотентности и безопасности. Частая ошибка — не знать разницу между PUT и PATCH или забыть про OPTIONS и HEAD.

[к оглавлению](#rest-api)

---

## Что такое идемпотентность и безопасность HTTP-методов?
<!-- grade: junior -->

Безопасный метод (Safe method) — метод, который не изменяет состояние ресурса на сервере. Вызов безопасного метода не создаёт побочных эффектов. Безопасные методы: `GET`, `HEAD`, `OPTIONS`.

Идемпотентный метод (Idempotent method) — метод, при котором многократный одинаковый запрос приводит к тому же результату, что и однократный. Повторный вызов не изменяет состояние сервера сверх того, что произвёл первый вызов. Идемпотентные методы: `GET`, `HEAD`, `OPTIONS`, `PUT`, `DELETE`.

> **Аналогия из жизни:** кнопка лифта — нажатие на кнопку вызова лифта один раз или десять раз приведёт к одному результату (лифт приедет). Это идемпотентная операция. А вот добавление товара в корзину (POST) при каждом нажатии добавит новый экземпляр.

### Примеры

- `DELETE /api/users/42` — первый вызов удалит пользователя, второй вернёт 404, но состояние сервера не изменится дополнительно. Метод идемпотентный.
- `PUT /api/users/42` с одним и тем же телом — каждый вызов приведёт к одному и тому же состоянию ресурса. Метод идемпотентный.
- `POST /api/users` — каждый вызов может создать нового пользователя. Метод не идемпотентный.
- `PATCH /api/users/42` с `{"age": 31}` — идемпотентный, но `PATCH /api/users/42` с `{"age": "increment"}` — не идемпотентный. Зависит от семантики операции.

```
Безопасные ⊂ Идемпотентные ⊂ Все методы
GET, HEAD, OPTIONS ⊂ GET, HEAD, OPTIONS, PUT, DELETE ⊂ GET, HEAD, OPTIONS, PUT, DELETE, POST, PATCH
```

Понимание идемпотентности критично для построения надёжных систем: клиент может безопасно повторить идемпотентный запрос при сетевых ошибках, не опасаясь дублирования действий.

> **На собеседовании:** ключевое — объяснить разницу между безопасностью и идемпотентностью. Все безопасные методы идемпотентны, но не наоборот (DELETE идемпотентен, но не безопасен). Частая ошибка — считать PATCH всегда идемпотентным.

[к оглавлению](#rest-api)

---

## В чём разница между PUT и POST?
<!-- grade: junior -->

PUT и POST — два HTTP-метода для создания и обновления ресурсов, различающиеся семантикой, идемпотентностью и тем, кто определяет URI нового ресурса.

| Характеристика | POST | PUT |
|---------------|------|-----|
| Назначение | Создание нового ресурса | Создание или полная замена ресурса |
| URI | Указывает на коллекцию | Указывает на конкретный ресурс |
| Идемпотентность | Нет | Да |
| Кто определяет URI нового ресурса | Сервер | Клиент |
| Пример | `POST /api/users` | `PUT /api/users/42` |

POST — клиент говорит серверу: «создай ресурс в коллекции, ты сам определи его идентификатор»:
```
POST /api/users HTTP/1.1
Content-Type: application/json

{"name": "Иван"}

Ответ:
HTTP/1.1 201 Created
Location: /api/users/42
```

PUT — клиент говорит серверу: «помести (или замени) ресурс по данному URI»:
```
PUT /api/users/42 HTTP/1.1
Content-Type: application/json

{"name": "Иван", "email": "ivan@example.com", "age": 30}

Ответ:
HTTP/1.1 200 OK     (если ресурс обновлён)
HTTP/1.1 201 Created (если ресурс создан)
```

Главное правило: если клиент знает URI ресурса и может его задать — используйте PUT. Если URI определяет сервер — используйте POST.

> **На собеседовании:** ключевые отличия — идемпотентность и кто определяет URI. POST не идемпотентен, PUT идемпотентен. Частая ошибка — говорить, что POST только для создания, а PUT только для обновления (PUT может и создавать).

[к оглавлению](#rest-api)

---

## В чём разница между PUT и PATCH?
<!-- grade: junior -->

PUT выполняет полную замену ресурса, а PATCH — частичное обновление. При PUT все не переданные поля будут сброшены, при PATCH — останутся без изменений.

| Характеристика | PUT | PATCH |
|---------------|-----|-------|
| Тип обновления | Полная замена ресурса | Частичное обновление |
| Тело запроса | Полное представление ресурса | Только изменяемые поля |
| Идемпотентность | Да | Зависит от реализации |
| Поведение для отсутствующих полей | Сбрасывает в значения по умолчанию / null | Не затрагивает |

Пример. Исходный ресурс:
```json
{
  "id": 42,
  "name": "Иван",
  "email": "ivan@example.com",
  "age": 30
}
```

PUT — замена целиком. Если поле не указано, оно будет сброшено:
```
PUT /api/users/42
{"name": "Иван Петров", "email": "ivan@example.com", "age": 31}
```
Если отправить только `{"name": "Иван Петров"}`, поля `email` и `age` станут `null`.

PATCH — частичное обновление. Изменяются только переданные поля:
```
PATCH /api/users/42
{"age": 31}
```
Поля `name` и `email` останутся без изменений.

<details><summary>Пример реализации в Spring</summary>

```java
// PUT — полная замена
@PutMapping("/users/{id}")
public ResponseEntity<User> replaceUser(@PathVariable Long id,
                                         @RequestBody User user) {
    user.setId(id);
    User saved = userService.save(user);
    return ResponseEntity.ok(saved);
}

// PATCH — частичное обновление
@PatchMapping("/users/{id}")
public ResponseEntity<User> updateUser(@PathVariable Long id,
                                        @RequestBody Map<String, Object> updates) {
    User updated = userService.partialUpdate(id, updates);
    return ResponseEntity.ok(updated);
}
```

</details>

> **На собеседовании:** важно показать понимание через пример с отсутствующими полями. Частая ошибка — не упомянуть, что PATCH может быть как идемпотентным, так и нет, в зависимости от семантики (установка значения vs инкремент).

[к оглавлению](#rest-api)

---

## Какие существуют коды HTTP-ответов и когда их использовать?
<!-- grade: junior -->

HTTP-коды ответов делятся на 5 классов по первой цифре, определяющей категорию результата обработки запроса.

### 1xx — Информационные

| Код | Название | Описание |
|-----|----------|----------|
| 100 | Continue | Сервер получил заголовки и клиент может продолжать отправку тела |
| 101 | Switching Protocols | Сервер переключает протокол (например, на WebSocket) |

### 2xx — Успешные

| Код | Название | Когда использовать |
|-----|----------|--------------------|
| 200 | OK | Успешный GET, PUT, PATCH, DELETE |
| 201 | Created | Успешный POST, ресурс создан. Заголовок `Location` указывает URI нового ресурса |
| 202 | Accepted | Запрос принят, но обработка ещё не завершена (асинхронная операция) |
| 204 | No Content | Успешный запрос, тело ответа отсутствует (обычно DELETE или PUT) |

### 3xx — Перенаправления

| Код | Название | Когда использовать |
|-----|----------|--------------------|
| 301 | Moved Permanently | Ресурс перемещён на постоянной основе |
| 302 | Found | Временное перенаправление |
| 304 | Not Modified | Ресурс не изменился с момента последнего запроса (кэширование) |

### 4xx — Ошибки клиента

| Код | Название | Когда использовать |
|-----|----------|--------------------|
| 400 | Bad Request | Некорректный запрос (невалидный JSON, нарушение валидации) |
| 401 | Unauthorized | Не пройдена аутентификация (неверный или отсутствующий токен) |
| 403 | Forbidden | Аутентификация пройдена, но нет прав доступа (авторизация) |
| 404 | Not Found | Ресурс не найден |
| 405 | Method Not Allowed | HTTP-метод не поддерживается для данного URI |
| 409 | Conflict | Конфликт состояния (например, попытка создать дубликат) |
| 415 | Unsupported Media Type | Неподдерживаемый Content-Type |
| 422 | Unprocessable Entity | Запрос синтаксически верен, но семантически некорректен |
| 429 | Too Many Requests | Превышен лимит запросов (Rate Limiting) |

### 5xx — Ошибки сервера

| Код | Название | Когда использовать |
|-----|----------|--------------------|
| 500 | Internal Server Error | Непредвиденная ошибка сервера |
| 502 | Bad Gateway | Некорректный ответ от вышестоящего сервера |
| 503 | Service Unavailable | Сервер временно недоступен (обслуживание, перегрузка) |
| 504 | Gateway Timeout | Вышестоящий сервер не ответил вовремя |

### Типичное использование в REST API

```
POST   /api/users       → 201 Created
GET    /api/users       → 200 OK
GET    /api/users/42    → 200 OK или 404 Not Found
PUT    /api/users/42    → 200 OK или 204 No Content
PATCH  /api/users/42    → 200 OK
DELETE /api/users/42    → 204 No Content
```

> **На собеседовании:** достаточно знать ключевые коды: 200, 201, 204, 301, 304, 400, 401, 403, 404, 409, 429, 500, 503. Частая ошибка — путать 401 (аутентификация) и 403 (авторизация).

[к оглавлению](#rest-api)

---

## Каковы лучшие практики проектирования URI?
<!-- grade: junior -->

URI в REST API должны быть предсказуемыми, читаемыми и отражать структуру ресурсов, а не операции над ними.

1. **Используйте существительные, а не глаголы:**
```
GET  /api/users          (правильно)
GET  /api/getUsers       (неправильно)
POST /api/createUser     (неправильно)
```

2. **Используйте множественное число для коллекций:**
```
/api/users               (правильно)
/api/users/42             (правильно)
/api/user                 (неправильно)
```

3. **Используйте вложенность для связанных ресурсов** (не более 2-3 уровней):
```
GET /api/users/42/orders       — заказы пользователя 42
GET /api/users/42/orders/7     — заказ 7 пользователя 42

/api/users/42/orders/7/items/3/reviews   (неправильно — слишком глубоко)
/api/order-items/3/reviews               (правильно)
```

4. **Используйте дефисы для разделения слов:**
```
/api/order-items          (правильно)
/api/orderItems           (неправильно)
/api/order_items          (неправильно)
```

5. **Используйте строчные буквы.**

6. **Не используйте расширения файлов.** Формат определяется через заголовок `Accept`.

7. **Используйте query-параметры для фильтрации, сортировки и пагинации:**
```
GET /api/users?status=active&sort=name&page=2&size=20
```

8. **Для действий вне CRUD используйте подресурсы-глаголы:**
```
POST /api/users/42/activate
POST /api/orders/7/cancel
```

9. **Используйте префикс версии** (`/api/v1/users`).

10. **Всегда используйте префикс `/api`.**

> **На собеседовании:** интервьюер ожидает знания основных правил: существительные, множественное число, дефисы, строчные буквы. Частая ошибка — использовать глаголы в URI (POST /api/createUser) или camelCase (/api/orderItems).

[к оглавлению](#rest-api)

---

## Как версионировать REST API?
<!-- grade: middle -->

Версионирование позволяет развивать API без нарушения обратной совместимости для существующих клиентов.

| Подход | Пример | Плюсы | Минусы |
|--------|--------|-------|--------|
| Версия в URL | `GET /api/v1/users` | Простота, наглядность, легко кэшировать | Нарушает принцип REST (URI = ресурс, не версия) |
| Кастомный заголовок | `X-API-Version: 1` | URI остаётся чистым | Менее очевидно, сложнее тестировать |
| Accept-заголовок (Media Type) | `Accept: application/vnd.myapi.v1+json` | Наиболее RESTful | Сложнее в реализации и тестировании |
| Query-параметр | `GET /api/users?version=1` | Простота | Загрязняет URI, может конфликтовать с параметрами |

На практике версия в URL — самый распространённый и рекомендуемый подход для большинства проектов благодаря своей простоте и прозрачности.

<details><summary>Примеры реализации в Spring</summary>

```java
// 1. Версия в URL
@RestController
@RequestMapping("/api/v1/users")
public class UserControllerV1 {
    @GetMapping
    public List<UserV1Dto> getUsers() { ... }
}

@RestController
@RequestMapping("/api/v2/users")
public class UserControllerV2 {
    @GetMapping
    public List<UserV2Dto> getUsers() { ... }
}

// 2. Версия в заголовке
@GetMapping(value = "/users", headers = "X-API-Version=1")
public List<UserV1Dto> getUsersV1() { ... }

@GetMapping(value = "/users", headers = "X-API-Version=2")
public List<UserV2Dto> getUsersV2() { ... }

// 3. Версия через Accept-заголовок
@GetMapping(value = "/users", produces = "application/vnd.myapi.v1+json")
public List<UserV1Dto> getUsersV1() { ... }

@GetMapping(value = "/users", produces = "application/vnd.myapi.v2+json")
public List<UserV2Dto> getUsersV2() { ... }
```

</details>

> **На собеседовании:** нужно знать все четыре подхода с их плюсами и минусами. Частая ошибка — не упомянуть, что версия в URL — самый популярный на практике, хотя формально не самый RESTful.

[к оглавлению](#rest-api)

---

## Что такое Content Negotiation?
<!-- grade: middle -->

Content Negotiation (согласование содержимого) — механизм HTTP, позволяющий клиенту и серверу договориться о формате представления ресурса.

Клиент указывает желаемый формат через заголовки:
- `Accept` — желаемый формат ответа (`application/json`, `application/xml`).
- `Accept-Language` — предпочитаемый язык (`ru`, `en`).
- `Accept-Encoding` — предпочитаемое сжатие (`gzip`, `deflate`).
- `Content-Type` — формат отправляемых данных в теле запроса.

```
GET /api/users/42 HTTP/1.1
Accept: application/json
Accept-Language: ru

Ответ:
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Language: ru
```

Если сервер не может вернуть данные в запрошенном формате, он возвращает 406 Not Acceptable.

<details><summary>Реализация в Spring</summary>

```java
// Spring автоматически поддерживает JSON и XML
@RestController
@RequestMapping("/api/users")
public class UserController {

    // Поддерживает и JSON, и XML
    @GetMapping(value = "/{id}",
                produces = {MediaType.APPLICATION_JSON_VALUE,
                            MediaType.APPLICATION_XML_VALUE})
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }

    // Принимает JSON или XML
    @PostMapping(consumes = {MediaType.APPLICATION_JSON_VALUE,
                             MediaType.APPLICATION_XML_VALUE})
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User saved = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(saved);
    }
}
```

Для поддержки XML в Spring Boot необходимо добавить зависимость:
```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

</details>

> **На собеседовании:** достаточно объяснить механизм через заголовки Accept/Content-Type и упомянуть код 406. Частая ошибка — путать Accept (чего хочет клиент) и Content-Type (что реально отправлено).

[к оглавлению](#rest-api)

---

## Что такое HATEOAS?
<!-- grade: middle -->

HATEOAS (Hypermedia as the Engine of Application State) — принцип REST, согласно которому сервер в ответе предоставляет клиенту ссылки на доступные действия и связанные ресурсы.

> **Аналогия из жизни:** HATEOAS — как навигация в интернет-магазине. Вы открываете страницу товара, и на ней есть ссылки: «Добавить в корзину», «Похожие товары», «Отзывы». Вам не нужно знать URL-ы заранее — страница сама подсказывает, что можно сделать дальше.

Обычный ответ без HATEOAS:
```json
{
  "id": 42,
  "name": "Иван",
  "status": "active"
}
```

Ответ с HATEOAS:
```json
{
  "id": 42,
  "name": "Иван",
  "status": "active",
  "_links": {
    "self": {"href": "/api/users/42"},
    "orders": {"href": "/api/users/42/orders"},
    "deactivate": {"href": "/api/users/42/deactivate", "method": "POST"}
  }
}
```

### Преимущества

- Клиент не зависит от жёстко заданных URL.
- API становится самодокументируемым.
- Сервер может динамически управлять доступными действиями (например, скрывать ссылку `deactivate` для уже неактивного пользователя).

<details><summary>Реализация в Spring HATEOAS</summary>

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public EntityModel<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);

        EntityModel<User> model = EntityModel.of(user);
        model.add(linkTo(methodOn(UserController.class).getUser(id)).withSelfRel());
        model.add(linkTo(methodOn(OrderController.class).getOrdersByUser(id))
                  .withRel("orders"));

        if ("active".equals(user.getStatus())) {
            model.add(linkTo(methodOn(UserController.class).deactivateUser(id))
                      .withRel("deactivate"));
        }

        return model;
    }

    @GetMapping
    public CollectionModel<EntityModel<User>> getAllUsers() {
        List<EntityModel<User>> users = userService.findAll().stream()
            .map(user -> EntityModel.of(user,
                linkTo(methodOn(UserController.class).getUser(user.getId())).withSelfRel()))
            .toList();

        return CollectionModel.of(users,
            linkTo(methodOn(UserController.class).getAllUsers()).withSelfRel());
    }
}
```

Зависимость:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

</details>

На практике HATEOAS используется нечасто из-за сложности реализации и того, что большинство клиентов (SPA, мобильные приложения) проще строить с заранее известными URL.

> **На собеседовании:** нужно объяснить принцип и привести пример ответа с `_links`. Бонус — упомянуть, что HATEOAS — это уровень 3 Richardson Maturity Model и что на практике он редко реализуется полностью.

[к оглавлению](#rest-api)

---

## В чём отличия REST от SOAP?
<!-- grade: junior -->

REST — архитектурный стиль, работающий преимущественно через HTTP с любым форматом данных. SOAP — строгий протокол, использующий исключительно XML и поддерживающий множество транспортов.

| Характеристика | REST | SOAP |
|---------------|------|------|
| Тип | Архитектурный стиль | Протокол |
| Формат данных | JSON, XML, HTML, текст и др. | Только XML |
| Транспорт | Преимущественно HTTP | HTTP, SMTP, TCP, JMS и др. |
| Стандарт описания | OpenAPI/Swagger (опционально) | WSDL (обязательно) |
| Состояние | Stateless | Может быть stateful |
| Кэширование | Встроенная поддержка HTTP-кэширования | Нет нативной поддержки |
| Производительность | Выше (меньше overhead) | Ниже (XML-парсинг, SOAP-конверт) |
| Безопасность | HTTPS, OAuth, JWT | WS-Security (более мощная) |
| Транзакции | Нет встроенной поддержки | WS-AtomicTransaction |
| Надёжность | Нет встроенной поддержки | WS-ReliableMessaging |
| Стиль вызовов | Ориентирован на ресурсы | Ориентирован на операции (RPC) |

### Когда использовать REST

- Публичные API (простота интеграции).
- Мобильные приложения (экономия трафика).
- Микросервисные архитектуры.
- Когда важна скорость разработки и простота.

### Когда использовать SOAP

- Корпоративные системы с формальными контрактами.
- Когда нужна строгая типизация.
- Когда нужны продвинутые стандарты безопасности (WS-Security).
- Когда нужны распределённые транзакции (WS-AtomicTransaction).
- Финансовые и банковские системы.

> **На собеседовании:** достаточно назвать 5-6 ключевых различий из таблицы и привести примеры, когда подходит REST, а когда SOAP. Частая ошибка — говорить, что SOAP устарел. SOAP по-прежнему широко используется в банковских и корпоративных системах.

[к оглавлению](#rest-api)

---

## Что такое Richardson Maturity Model?
<!-- grade: middle -->

Richardson Maturity Model (Модель зрелости Ричардсона) — модель, описывающая четыре уровня зрелости REST API, предложенная Леонардом Ричардсоном.

| Уровень | Название | Описание |
|---------|----------|----------|
| 0 | «Болото POX» (The Swamp of POX) | Один URI, один HTTP-метод (обычно POST). Фактически RPC через HTTP |
| 1 | Ресурсы (Resources) | Каждый ресурс имеет собственный URI, но используется один метод |
| 2 | HTTP-глаголы (HTTP Verbs) | Используются правильные HTTP-методы и коды ответов |
| 3 | HATEOAS (Hypermedia Controls) | Ответы содержат гиперссылки на доступные действия и связанные ресурсы |

### Примеры каждого уровня

Уровень 0 — RPC через HTTP:
```
POST /api HTTP/1.1
{"action": "getUser", "userId": 42}
```

Уровень 1 — Ресурсы:
```
POST /api/users/42
{"action": "get"}
```

Уровень 2 — HTTP-глаголы (большинство современных API):
```
GET    /api/users/42    → 200 OK
POST   /api/users       → 201 Created
DELETE /api/users/42    → 204 No Content
```

Уровень 3 — HATEOAS:
```json
{
  "id": 42,
  "name": "Иван",
  "_links": {
    "self": {"href": "/api/users/42"},
    "orders": {"href": "/api/users/42/orders"},
    "delete": {"href": "/api/users/42", "method": "DELETE"}
  }
}
```

Согласно Рою Филдингу, только API уровня 3 по-настоящему является RESTful. Однако на практике большинство API реализуют уровень 2, что считается приемлемым.

> **На собеседовании:** нужно перечислить все четыре уровня с краткими примерами. Частая ошибка — не знать, что большинство реальных API находятся на уровне 2, или не упомянуть, что уровень 3 (HATEOAS) — теоретический идеал.

[к оглавлению](#rest-api)

---

## Как реализовать пагинацию, фильтрацию и сортировку в REST API?
<!-- grade: middle -->

Пагинация, фильтрация и сортировка — обязательные механизмы для работы с большими коллекциями ресурсов, реализуемые через query-параметры HTTP-запросов.

### Пагинация

| Подход | Пример | Когда использовать |
|--------|--------|--------------------|
| Offset-based | `GET /api/users?page=0&size=20` | Наиболее распространённый, подходит для большинства случаев |
| Cursor-based | `GET /api/users?cursor=eyJpZCI6NDJ9&size=20` | Для больших объёмов данных, real-time лент |

Пример ответа с метаданными пагинации:
```json
{
  "content": [...],
  "page": 0,
  "size": 20,
  "totalElements": 150,
  "totalPages": 8,
  "last": false,
  "_links": {
    "self": {"href": "/api/users?page=0&size=20"},
    "next": {"href": "/api/users?page=1&size=20"},
    "last": {"href": "/api/users?page=7&size=20"}
  }
}
```

### Фильтрация

```
GET /api/users?status=active
GET /api/users?status=active&role=admin
GET /api/users?age_gte=18&age_lte=65
GET /api/users?search=Иван
```

### Сортировка

```
GET /api/users?sort=name,asc
GET /api/users?sort=createdAt,desc&sort=name,asc
```

<details><summary>Реализация в Spring (Spring Data)</summary>

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserRepository userRepository;

    // Пагинация и сортировка через Pageable
    @GetMapping
    public Page<User> getUsers(
            @RequestParam(required = false) String status,
            @RequestParam(required = false) String name,
            @PageableDefault(size = 20, sort = "id") Pageable pageable) {

        if (status != null) {
            return userRepository.findByStatus(status, pageable);
        }
        if (name != null) {
            return userRepository.findByNameContainingIgnoreCase(name, pageable);
        }
        return userRepository.findAll(pageable);
    }
}
```

Запрос:
```
GET /api/users?status=active&page=0&size=10&sort=name,asc
```

Для сложной фильтрации можно использовать Spring Data Specifications:
```java
@GetMapping
public Page<User> getUsers(
        @RequestParam Map<String, String> filters,
        Pageable pageable) {

    Specification<User> spec = Specification.where(null);

    if (filters.containsKey("status")) {
        spec = spec.and((root, query, cb) ->
            cb.equal(root.get("status"), filters.get("status")));
    }
    if (filters.containsKey("name")) {
        spec = spec.and((root, query, cb) ->
            cb.like(cb.lower(root.get("name")),
                    "%" + filters.get("name").toLowerCase() + "%"));
    }

    return userRepository.findAll(spec, pageable);
}
```

</details>

> **На собеседовании:** нужно знать оба подхода к пагинации (offset и cursor) и понимать, когда использовать какой. Частая ошибка — забыть про метаданные пагинации (totalElements, totalPages) в ответе или не упомянуть cursor-based подход.

[к оглавлению](#rest-api)

---

## Как обрабатывать ошибки в REST API?
<!-- grade: middle -->

Правильная обработка ошибок строится на двух принципах: использование корректных HTTP-кодов (4xx/5xx) и возврат структурированного тела ответа с описанием ошибки.

Стандартный формат ответа об ошибке (RFC 7807 — Problem Details):
```json
{
  "type": "https://api.example.com/errors/validation-error",
  "title": "Ошибка валидации",
  "status": 400,
  "detail": "Поле 'email' содержит некорректное значение",
  "instance": "/api/users",
  "timestamp": "2025-01-15T10:30:00Z",
  "errors": [
    {
      "field": "email",
      "message": "Некорректный формат email",
      "rejectedValue": "not-an-email"
    }
  ]
}
```

<details><summary>Реализация в Spring с @ControllerAdvice</summary>

```java
// DTO для ответа об ошибке
public record ErrorResponse(
    String type,
    String title,
    int status,
    String detail,
    String instance,
    LocalDateTime timestamp,
    List<FieldError> errors
) {
    public record FieldError(String field, String message, Object rejectedValue) {}
}
```

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(
            EntityNotFoundException ex, HttpServletRequest request) {
        ErrorResponse error = new ErrorResponse(
            "https://api.example.com/errors/not-found",
            "Ресурс не найден",
            404,
            ex.getMessage(),
            request.getRequestURI(),
            LocalDateTime.now(),
            null
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(
            MethodArgumentNotValidException ex, HttpServletRequest request) {
        List<ErrorResponse.FieldError> fieldErrors = ex.getBindingResult()
            .getFieldErrors().stream()
            .map(fe -> new ErrorResponse.FieldError(
                fe.getField(), fe.getDefaultMessage(), fe.getRejectedValue()))
            .toList();

        ErrorResponse error = new ErrorResponse(
            "https://api.example.com/errors/validation-error",
            "Ошибка валидации",
            400,
            "Переданные данные не прошли валидацию",
            request.getRequestURI(),
            LocalDateTime.now(),
            fieldErrors
        );
        return ResponseEntity.badRequest().body(error);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(
            Exception ex, HttpServletRequest request) {
        ErrorResponse error = new ErrorResponse(
            "https://api.example.com/errors/internal-error",
            "Внутренняя ошибка сервера",
            500,
            "Произошла непредвиденная ошибка",
            request.getRequestURI(),
            LocalDateTime.now(),
            null
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}
```

</details>

Spring Boot 3 имеет встроенную поддержку RFC 7807 через `ProblemDetail`:
```java
@ExceptionHandler(EntityNotFoundException.class)
public ProblemDetail handleNotFound(EntityNotFoundException ex) {
    ProblemDetail problem = ProblemDetail.forStatusAndDetail(
        HttpStatus.NOT_FOUND, ex.getMessage());
    problem.setTitle("Ресурс не найден");
    problem.setType(URI.create("https://api.example.com/errors/not-found"));
    return problem;
}
```

> **На собеседовании:** ключевое — упомянуть RFC 7807 (Problem Details) и `@ControllerAdvice` как стандартные подходы. Частая ошибка — возвращать 200 OK с телом `{"success": false}` вместо правильных HTTP-кодов ошибок.

[к оглавлению](#rest-api)

---

## Какие способы аутентификации используются в REST API?
<!-- grade: middle -->

Аутентификация в REST API — процесс проверки личности клиента, выполняемый при каждом запросе в силу stateless-природы REST.

### 1. Basic Authentication

Логин и пароль передаются в заголовке `Authorization` в кодировке Base64.
```
GET /api/users HTTP/1.1
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```
- Простота реализации.
- Обязательно использовать только по HTTPS.
- Учётные данные передаются при каждом запросе.
- Не рекомендуется для публичных API.

### 2. Token-based (Bearer Token / JWT)

Клиент получает токен при аутентификации и использует его в последующих запросах.
```
POST /api/auth/login
{"username": "user", "password": "pass"}

Ответ:
{"accessToken": "eyJhbGciOiJIUzI1NiJ9...", "refreshToken": "..."}
```

```
GET /api/users HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

JWT (JSON Web Token) состоит из трёх частей: Header.Payload.Signature.
```
Header:    {"alg": "HS256", "typ": "JWT"}
Payload:   {"sub": "42", "name": "Иван", "role": "ADMIN", "exp": 1700000000}
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

Преимущества JWT: stateless (сервер не хранит сессии), содержит информацию о пользователе (claims), легко масштабируется.

### 3. API Key

Ключ передаётся в заголовке или query-параметре.
```
GET /api/users HTTP/1.1
X-API-Key: abcdef123456
```
Используется для межсервисного взаимодействия и публичных API с ограниченным доступом.

### 4. OAuth 2.0

Протокол авторизации, позволяющий приложению получить ограниченный доступ к ресурсам пользователя без передачи его учётных данных. Подробнее см. следующий вопрос.

<details><summary>Пример конфигурации Spring Security для JWT</summary>

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(csrf -> csrf.disable())
            .sessionManagement(session ->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated())
            .addFilterBefore(jwtAuthenticationFilter(),
                             UsernamePasswordAuthenticationFilter.class)
            .build();
    }
}
```

</details>

> **На собеседовании:** нужно знать все четыре способа и понимать, когда какой использовать. Частая ошибка — не упомянуть, что JWT — stateless, а Basic Auth обязателен только по HTTPS.

[к оглавлению](#rest-api)

---

## Что такое OAuth 2.0 и как он работает?
<!-- grade: middle -->

OAuth 2.0 — протокол (фреймворк) авторизации, который позволяет стороннему приложению получить ограниченный доступ к HTTP-сервису от имени владельца ресурса, не раскрывая его учётные данные.

> **Аналогия из жизни:** OAuth 2.0 — как выдача ключ-карты горничной в отеле. Вы (Resource Owner) не даёте горничной свой паспорт (пароль), а ресепшн (Authorization Server) выдаёт ей карту с ограниченным доступом (токен): только в вашу комнату и только до определённого времени.

### Основные роли

| Роль | Описание |
|------|----------|
| Resource Owner | Пользователь, владелец данных |
| Client | Приложение, запрашивающее доступ |
| Authorization Server | Сервер, выдающий токены (Keycloak, Auth0 и др.) |
| Resource Server | Сервер, хранящий защищённые ресурсы (наш API) |

### Основные потоки (Grant Types)

1. **Authorization Code** — наиболее безопасный, для серверных приложений:
```
1. Клиент → Redirect на Authorization Server
2. Пользователь аутентифицируется и даёт согласие
3. Authorization Server → Redirect на клиент с authorization code
4. Клиент → Authorization Server: обмен code на access_token
5. Клиент → Resource Server: запрос с access_token
```

2. **Authorization Code + PKCE** — для SPA и мобильных приложений (рекомендуемый). Дополняет Authorization Code проверкой code_verifier / code_challenge для защиты от перехвата authorization code.

3. **Client Credentials** — для межсервисного взаимодействия (machine-to-machine):
```
POST /oauth/token
grant_type=client_credentials&client_id=myapp&client_secret=secret&scope=read
```

4. **Resource Owner Password Credentials** — устаревший, не рекомендуется.

<details><summary>Настройка Spring Boot как Resource Server</summary>

```java
@Configuration
@EnableWebSecurity
public class ResourceServerConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .anyRequest().authenticated())
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthConverter())))
            .build();
    }

    @Bean
    public JwtAuthenticationConverter jwtAuthConverter() {
        JwtGrantedAuthoritiesConverter converter = new JwtGrantedAuthoritiesConverter();
        converter.setAuthorityPrefix("ROLE_");
        converter.setAuthoritiesClaimName("roles");

        JwtAuthenticationConverter jwtConverter = new JwtAuthenticationConverter();
        jwtConverter.setJwtGrantedAuthoritiesConverter(converter);
        return jwtConverter;
    }
}
```

В `application.yml`:
```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://auth-server.example.com/realms/myrealm
```

</details>

> **На собеседовании:** нужно назвать четыре роли и три основных потока (Authorization Code, PKCE, Client Credentials). Частая ошибка — путать аутентификацию и авторизацию: OAuth 2.0 — это протокол авторизации, а не аутентификации (для аутентификации — OpenID Connect поверх OAuth 2.0).

[к оглавлению](#rest-api)

---

## Как работает кэширование в REST?
<!-- grade: middle -->

Кэширование — одно из шести ограничений REST, реализуемое через HTTP-заголовки Cache-Control, ETag и Last-Modified для снижения нагрузки на сервер и ускорения ответов клиенту.

### 1. Cache-Control

Заголовок, управляющий политикой кэширования:
```
Cache-Control: max-age=3600          — кэшировать на 1 час
Cache-Control: no-cache              — проверять актуальность при каждом запросе
Cache-Control: no-store              — не кэшировать вообще
Cache-Control: public, max-age=86400 — кэшировать публично на 24 часа
Cache-Control: private, max-age=600  — кэшировать только на клиенте
```

### 2. ETag (Entity Tag)

Сервер возвращает хеш содержимого ресурса. Клиент при повторном запросе отправляет его для проверки:
```
Первый запрос:
GET /api/users/42 → 200 OK, ETag: "33a64df..."

Повторный запрос:
GET /api/users/42, If-None-Match: "33a64df..."
→ 304 Not Modified (если ресурс не изменился)
```

### 3. Last-Modified / If-Modified-Since

Аналогичен ETag, но использует дату последнего изменения:
```
Ответ: Last-Modified: Fri, 10 Jan 2025 12:00:00 GMT
Повторный запрос: If-Modified-Since: Fri, 10 Jan 2025 12:00:00 GMT
→ 304 Not Modified (если не изменился)
```

<details><summary>Реализация в Spring</summary>

Shallow ETag (Spring автоматически считает ETag от тела ответа):
```java
@Configuration
public class WebConfig {

    @Bean
    public FilterRegistrationBean<ShallowEtagHeaderFilter> shallowEtagHeaderFilter() {
        FilterRegistrationBean<ShallowEtagHeaderFilter> filter =
            new FilterRegistrationBean<>(new ShallowEtagHeaderFilter());
        filter.addUrlPatterns("/api/*");
        return filter;
    }
}
```

Ручное управление кэшированием:
```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.findById(id);

    return ResponseEntity.ok()
        .cacheControl(CacheControl.maxAge(Duration.ofMinutes(30)))
        .eTag(String.valueOf(user.getVersion()))
        .lastModified(user.getUpdatedAt().toInstant())
        .body(user);
}

// Условный запрос с ETag
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id,
                                     WebRequest request) {
    User user = userService.findById(id);
    String etag = String.valueOf(user.getVersion());

    if (request.checkNotModified(etag)) {
        return null; // Spring вернёт 304 автоматически
    }

    return ResponseEntity.ok()
        .eTag(etag)
        .body(user);
}
```

</details>

> **На собеседовании:** нужно знать три механизма (Cache-Control, ETag, Last-Modified) и код 304 Not Modified. Частая ошибка — путать no-cache (проверять актуальность) и no-store (не кэшировать вообще).

[к оглавлению](#rest-api)

---

## Что такое CORS и как его настроить в Spring?
<!-- grade: middle -->

CORS (Cross-Origin Resource Sharing) — механизм безопасности браузера, который контролирует, какие домены могут обращаться к ресурсам API. По умолчанию браузеры запрещают JavaScript-коду делать запросы к другому домену (Same-Origin Policy).

### Как работает CORS

Простые запросы (GET, HEAD, POST с простыми заголовками) — браузер отправляет запрос напрямую с заголовком `Origin`:
```
GET /api/users HTTP/1.1
Origin: https://frontend.example.com

Ответ:
Access-Control-Allow-Origin: https://frontend.example.com
```

Предварительные запросы (Preflight) — для «сложных» запросов (PUT, DELETE, кастомные заголовки) браузер сначала отправляет OPTIONS:
```
OPTIONS /api/users/42 HTTP/1.1
Origin: https://frontend.example.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: Content-Type, Authorization

Ответ:
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 3600
```

### Настройка в Spring

<details><summary>Три способа настройки CORS</summary>

1. Аннотация на контроллере:
```java
@RestController
@RequestMapping("/api/users")
@CrossOrigin(origins = "https://frontend.example.com")
public class UserController {

    @CrossOrigin(origins = "*", maxAge = 3600)
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) { ... }
}
```

2. Глобальная конфигурация через WebMvcConfigurer:
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
            .allowedOrigins("https://frontend.example.com")
            .allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE")
            .allowedHeaders("*")
            .allowCredentials(true)
            .maxAge(3600);
    }
}
```

3. Через Spring Security (если используется):
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http
        .cors(cors -> cors.configurationSource(corsConfigurationSource()))
        .csrf(csrf -> csrf.disable())
        .build();
}

@Bean
public CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration config = new CorsConfiguration();
    config.setAllowedOrigins(List.of("https://frontend.example.com"));
    config.setAllowedMethods(List.of("GET", "POST", "PUT", "PATCH", "DELETE"));
    config.setAllowedHeaders(List.of("*"));
    config.setAllowCredentials(true);

    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/api/**", config);
    return source;
}
```

</details>

Если используется Spring Security, настройку CORS нужно делать через `SecurityFilterChain`, так как Spring Security обрабатывает запросы раньше `WebMvcConfigurer`.

> **На собеседовании:** нужно объяснить, зачем нужен CORS (Same-Origin Policy), что такое preflight-запрос и как настроить в Spring. Частая ошибка — ставить `allowedOrigins("*")` в продакшне вместо конкретных доменов.

[к оглавлению](#rest-api)

---

## Как документировать REST API с помощью Swagger/OpenAPI?
<!-- grade: middle -->

OpenAPI Specification (ранее Swagger Specification) — стандарт описания REST API в формате JSON или YAML. Swagger — набор инструментов для работы с OpenAPI (Swagger UI, Swagger Editor, Swagger Codegen).

### Основные инструменты

- Swagger UI — интерактивная веб-документация с возможностью отправки запросов.
- OpenAPI Generator / Swagger Codegen — генерация клиентов и серверов из спецификации.
- Springdoc-openapi — библиотека для автоматической генерации OpenAPI-документации в Spring Boot.

### Настройка в Spring Boot (springdoc-openapi)

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.5.0</version>
</dependency>
```

После подключения доступны:
- `http://localhost:8080/swagger-ui.html` — Swagger UI.
- `http://localhost:8080/v3/api-docs` — OpenAPI-спецификация в JSON.

<details><summary>Аннотации для документирования</summary>

```java
@RestController
@RequestMapping("/api/users")
@Tag(name = "Users", description = "Управление пользователями")
public class UserController {

    @Operation(
        summary = "Получить пользователя по ID",
        description = "Возвращает полную информацию о пользователе"
    )
    @ApiResponses({
        @ApiResponse(responseCode = "200", description = "Пользователь найден",
            content = @Content(schema = @Schema(implementation = UserDto.class))),
        @ApiResponse(responseCode = "404", description = "Пользователь не найден",
            content = @Content(schema = @Schema(implementation = ErrorResponse.class)))
    })
    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(
            @Parameter(description = "ID пользователя", example = "42")
            @PathVariable Long id) {
        return ResponseEntity.ok(userService.findById(id));
    }

    @Operation(summary = "Создать нового пользователя")
    @ApiResponse(responseCode = "201", description = "Пользователь создан")
    @PostMapping
    public ResponseEntity<UserDto> createUser(
            @RequestBody @Valid CreateUserRequest request) {
        UserDto created = userService.create(request);
        URI location = URI.create("/api/users/" + created.id());
        return ResponseEntity.created(location).body(created);
    }
}
```

Документирование модели:
```java
@Schema(description = "Запрос на создание пользователя")
public record CreateUserRequest(
    @Schema(description = "Имя пользователя", example = "Иван Петров")
    @NotBlank String name,

    @Schema(description = "Email", example = "ivan@example.com")
    @Email String email,

    @Schema(description = "Возраст", minimum = "18", maximum = "120")
    @Min(18) Integer age
) {}
```

Глобальная конфигурация:
```java
@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("User Service API")
                .version("1.0.0")
                .description("API для управления пользователями"))
            .addSecurityItem(new SecurityRequirement().addList("Bearer"))
            .components(new Components()
                .addSecuritySchemes("Bearer",
                    new SecurityScheme()
                        .type(SecurityScheme.Type.HTTP)
                        .scheme("bearer")
                        .bearerFormat("JWT")));
    }
}
```

</details>

> **На собеседовании:** нужно знать разницу между OpenAPI (спецификация) и Swagger (инструменты), уметь назвать ключевые аннотации (@Operation, @Tag, @Schema). Частая ошибка — путать Swagger и OpenAPI или не знать про springdoc-openapi для Spring Boot 3.

[к оглавлению](#rest-api)

---

## Как реализовать REST API в Spring?
<!-- grade: junior -->

Spring предоставляет средства для создания REST API через Spring Web MVC и Spring WebFlux (реактивный подход). Ключевые аннотации: `@RestController` (= `@Controller` + `@ResponseBody`), `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@PatchMapping`, `@DeleteMapping`.

<details><summary>Полный пример CRUD-контроллера</summary>

```java
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
@Validated
public class UserController {

    private final UserService userService;

    @GetMapping
    public ResponseEntity<Page<UserDto>> getAllUsers(
            @RequestParam(required = false) String name,
            @PageableDefault(size = 20) Pageable pageable) {
        Page<UserDto> users = userService.findAll(name, pageable);
        return ResponseEntity.ok(users);
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        UserDto user = userService.findById(id);
        return ResponseEntity.ok(user);
    }

    @PostMapping
    public ResponseEntity<UserDto> createUser(
            @Valid @RequestBody CreateUserRequest request) {
        UserDto created = userService.create(request);
        URI location = ServletUriComponentsBuilder
            .fromCurrentRequest()
            .path("/{id}")
            .buildAndExpand(created.id())
            .toUri();
        return ResponseEntity.created(location).body(created);
    }

    @PutMapping("/{id}")
    public ResponseEntity<UserDto> replaceUser(
            @PathVariable Long id,
            @Valid @RequestBody UpdateUserRequest request) {
        UserDto updated = userService.replace(id, request);
        return ResponseEntity.ok(updated);
    }

    @PatchMapping("/{id}")
    public ResponseEntity<UserDto> updateUser(
            @PathVariable Long id,
            @RequestBody Map<String, Object> updates) {
        UserDto updated = userService.partialUpdate(id, updates);
        return ResponseEntity.ok(updated);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

</details>

### Основные аннотации для параметров

| Аннотация | Назначение | Пример |
|-----------|-----------|--------|
| `@PathVariable` | Извлечение из URI | `/users/{id}` |
| `@RequestParam` | Из query-строки | `/users?name=Иван` |
| `@RequestBody` | Десериализация тела запроса | JSON в объект |
| `@RequestHeader` | Извлечение HTTP-заголовка | `Authorization` |
| `@CookieValue` | Извлечение значения из cookie | |

<details><summary>DTO, валидация и сервисный слой</summary>

```java
public record CreateUserRequest(
    @NotBlank(message = "Имя не может быть пустым")
    String name,

    @Email(message = "Некорректный формат email")
    @NotBlank
    String email,

    @Min(value = 18, message = "Минимальный возраст — 18 лет")
    Integer age
) {}
```

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class UserService {

    private final UserRepository userRepository;
    private final UserMapper userMapper;

    public UserDto findById(Long id) {
        return userRepository.findById(id)
            .map(userMapper::toDto)
            .orElseThrow(() ->
                new EntityNotFoundException("Пользователь с id=" + id + " не найден"));
    }

    @Transactional
    public UserDto create(CreateUserRequest request) {
        User user = userMapper.toEntity(request);
        User saved = userRepository.save(user);
        return userMapper.toDto(saved);
    }
}
```

</details>

> **На собеседовании:** нужно показать знание основных аннотаций и структуру Controller → Service → Repository. Частая ошибка — не использовать DTO (работать с сущностями напрямую в контроллере) или забыть про `@Valid` для валидации.

[к оглавлению](#rest-api)

---

## Что такое ResponseEntity и зачем он нужен?
<!-- grade: junior -->

ResponseEntity — класс Spring, представляющий полный HTTP-ответ: тело, заголовки и код статуса. Позволяет гибко управлять всеми аспектами ответа контроллера.

### Зачем нужен

- Управление HTTP-кодом ответа.
- Добавление пользовательских заголовков.
- Контроль тела ответа.
- Возврат ответа без тела (204 No Content).
- Указание URI созданного ресурса (201 Created + Location).

### Примеры использования

```java
// 200 OK с телом
return ResponseEntity.ok(user);

// 201 Created с Location
URI location = URI.create("/api/users/" + user.getId());
return ResponseEntity.created(location).body(user);

// 204 No Content
return ResponseEntity.noContent().build();

// 404 Not Found
return ResponseEntity.notFound().build();

// С кэшированием
return ResponseEntity.ok()
    .cacheControl(CacheControl.maxAge(Duration.ofHours(1)))
    .eTag("v1")
    .body(user);
```

### Сравнение подходов

| Подход | Когда использовать |
|--------|-------------------|
| Без ResponseEntity (`return user`) | Простые случаи, всегда 200 OK, нет контроля над заголовками |
| С ResponseEntity | Когда нужен контроль статуса, заголовков или условный ответ |

```java
// Без ResponseEntity — всегда 200 OK
@GetMapping("/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}

// С ResponseEntity — полный контроль
@GetMapping("/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    return userService.findById(id)
        .map(user -> ResponseEntity.ok()
            .eTag(String.valueOf(user.getVersion()))
            .body(user))
        .orElse(ResponseEntity.notFound().build());
}
```

> **На собеседовании:** объясните, что ResponseEntity даёт контроль над статусом, заголовками и телом ответа. Частая ошибка — использовать ResponseEntity везде, даже где достаточно простого возврата объекта.

[к оглавлению](#rest-api)

---

## Что такое Rate Limiting и как его реализовать?
<!-- grade: middle -->

Rate Limiting (ограничение частоты запросов) — механизм защиты API от злоупотреблений и перегрузки путём ограничения количества запросов от клиента за определённый период времени.

### Основные алгоритмы

| Алгоритм | Описание |
|----------|----------|
| Fixed Window | Подсчёт запросов в фиксированных временных интервалах (100 запросов в минуту) |
| Sliding Window | Более точный, учитывает запросы за последние N секунд |
| Token Bucket | Жетоны добавляются с постоянной скоростью. Каждый запрос забирает жетон. Нет жетонов — отказ |
| Leaky Bucket | Запросы обрабатываются с постоянной скоростью, избыточные ставятся в очередь |

### HTTP-заголовки для Rate Limiting

```
HTTP/1.1 200 OK
X-RateLimit-Limit: 100           — максимум запросов
X-RateLimit-Remaining: 45        — осталось запросов
X-RateLimit-Reset: 1700000060    — время сброса (Unix timestamp)

HTTP/1.1 429 Too Many Requests
Retry-After: 30                  — через сколько секунд повторить
```

<details><summary>Реализация в Spring с помощью Bucket4j</summary>

```xml
<dependency>
    <groupId>com.bucket4j</groupId>
    <artifactId>bucket4j-core</artifactId>
    <version>8.7.0</version>
</dependency>
```

```java
@Component
public class RateLimitFilter extends OncePerRequestFilter {

    private final Map<String, Bucket> buckets = new ConcurrentHashMap<>();

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                     HttpServletResponse response,
                                     FilterChain filterChain)
            throws ServletException, IOException {

        String clientId = getClientId(request);
        Bucket bucket = buckets.computeIfAbsent(clientId, this::createBucket);

        ConsumptionProbe probe = bucket.tryConsumeAndReturnRemaining(1);
        response.setHeader("X-RateLimit-Limit", "100");
        response.setHeader("X-RateLimit-Remaining",
                           String.valueOf(probe.getRemainingTokens()));

        if (probe.isConsumed()) {
            filterChain.doFilter(request, response);
        } else {
            response.setStatus(HttpStatus.TOO_MANY_REQUESTS.value());
            response.setHeader("Retry-After",
                String.valueOf(probe.getNanosToWaitForRefill() / 1_000_000_000));
            response.getWriter().write("{\"error\": \"Превышен лимит запросов\"}");
        }
    }

    private Bucket createBucket(String clientId) {
        return Bucket.builder()
            .addLimit(Bandwidth.classic(100, Refill.greedy(100, Duration.ofMinutes(1))))
            .build();
    }

    private String getClientId(HttpServletRequest request) {
        String apiKey = request.getHeader("X-API-Key");
        return apiKey != null ? apiKey : request.getRemoteAddr();
    }
}
```

</details>

<details><summary>Реализация через Spring Cloud Gateway</summary>

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/api/users/**
          filters:
            - name: RequestRateLimiter
              args:
                redis-rate-limiter.replenishRate: 10
                redis-rate-limiter.burstCapacity: 20
                key-resolver: "#{@userKeyResolver}"
```

```java
@Bean
public KeyResolver userKeyResolver() {
    return exchange -> Mono.just(
        exchange.getRequest().getRemoteAddress().getAddress().getHostAddress());
}
```

</details>

> **На собеседовании:** нужно знать алгоритмы (Token Bucket самый популярный), HTTP-заголовки (429, Retry-After, X-RateLimit-*) и хотя бы один способ реализации. Частая ошибка — не упомянуть заголовок Retry-After для клиента.

[к оглавлению](#rest-api)

---

## Как обеспечить безопасность REST API?
<!-- grade: middle -->

Безопасность REST API — комплексная задача, включающая несколько уровней защиты: от транспортного шифрования до аудита действий.

### Чек-лист безопасности REST API

| Уровень | Меры |
|---------|------|
| Транспорт | HTTPS (TLS) для всех endpoint-ов |
| Аутентификация | JWT / OAuth 2.0 / API Key |
| Авторизация | Ролевая модель (@PreAuthorize) |
| Валидация | Все входные данные (@Valid) |
| Rate Limiting | Защита от DDoS и brute-force |
| CORS | Только доверенные домены |
| DTO | Вместо сущностей в API (защита от Mass Assignment) |
| Заголовки | X-Content-Type-Options, X-Frame-Options, HSTS |
| Информация | Нет утечки стек-трейсов, версий, внутренних ID |
| Аудит | Логирование аутентификации, авторизации, изменений |

### Защита от массового присваивания (Mass Assignment)

```java
// Опасно — клиент может передать {"role": "ADMIN"}
@PostMapping
public User create(@RequestBody User user) { ... }

// Безопасно — DTO содержит только допустимые поля
@PostMapping
public UserDto create(@RequestBody CreateUserRequest request) { ... }
```

<details><summary>Примеры конфигурации Spring Security</summary>

Авторизация на уровне методов:
```java
@PreAuthorize("hasRole('ADMIN')")
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) { ... }

@PreAuthorize("hasRole('USER') and #id == authentication.principal.id")
@GetMapping("/users/{id}/profile")
public ResponseEntity<Profile> getProfile(@PathVariable Long id) { ... }
```

Защита заголовков:
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http
        .headers(headers -> headers
            .contentTypeOptions(Customizer.withDefaults())       // X-Content-Type-Options: nosniff
            .frameOptions(frame -> frame.deny())                  // X-Frame-Options: DENY
            .httpStrictTransportSecurity(hsts -> hsts
                .maxAgeInSeconds(31536000)
                .includeSubDomains(true)))                        // Strict-Transport-Security
        .build();
}
```

Минимизация раскрытия информации:
```yaml
server:
  error:
    include-stacktrace: never
    include-message: never
```

Аудит:
```java
@Aspect
@Component
@Slf4j
public class AuditAspect {

    @AfterReturning("@annotation(auditable)")
    public void audit(JoinPoint joinPoint, Auditable auditable) {
        String user = SecurityContextHolder.getContext()
            .getAuthentication().getName();
        log.info("Audit: user={}, action={}, method={}",
                 user, auditable.action(), joinPoint.getSignature().getName());
    }
}
```

</details>

> **На собеседовании:** нужно перечислить минимум 5-6 уровней защиты из чек-листа. Частая ошибка — говорить только про аутентификацию и забыть про валидацию, CORS, Rate Limiting и DTO.

[к оглавлению](#rest-api)

---

## Что такое идемпотентный ключ (Idempotency Key)?
<!-- grade: middle -->

Идемпотентный ключ (Idempotency Key) — уникальный идентификатор, передаваемый клиентом в запросе, позволяющий серверу распознавать повторные запросы и не выполнять операцию дважды.

> **Аналогия из жизни:** идемпотентный ключ — как номер платёжного поручения в банке. Если вы случайно отправите одно и то же поручение дважды, банк увидит, что поручение с таким номером уже обработано, и не спишет деньги повторно.

### Зачем нужен

Метод POST не является идемпотентным. При сетевых ошибках (таймаут, разрыв соединения) клиент не знает, был ли запрос обработан. Повторная отправка может привести к дублированию (двойное списание денег, двойное создание заказа).

### Как работает

```
POST /api/payments HTTP/1.1
Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
Content-Type: application/json

{"amount": 1000, "currency": "RUB", "recipientId": 42}
```

1. Сервер проверяет, обрабатывался ли запрос с таким ключом.
2. Если нет — обрабатывает и сохраняет результат.
3. Если да — возвращает сохранённый результат без повторного выполнения.

<details><summary>Реализация в Spring</summary>

```java
@RestController
@RequestMapping("/api/payments")
@RequiredArgsConstructor
public class PaymentController {

    private final PaymentService paymentService;
    private final IdempotencyService idempotencyService;

    @PostMapping
    public ResponseEntity<PaymentResponse> createPayment(
            @RequestHeader("Idempotency-Key") String idempotencyKey,
            @Valid @RequestBody PaymentRequest request) {

        // Проверяем, есть ли уже результат
        return idempotencyService.findByKey(idempotencyKey)
            .map(cached -> ResponseEntity.ok(cached))
            .orElseGet(() -> {
                PaymentResponse response = paymentService.processPayment(request);
                idempotencyService.save(idempotencyKey, response);
                return ResponseEntity.status(HttpStatus.CREATED).body(response);
            });
    }
}

@Service
@RequiredArgsConstructor
public class IdempotencyService {

    private final IdempotencyRepository repository;

    public Optional<PaymentResponse> findByKey(String key) {
        return repository.findByKey(key)
            .map(entry -> deserialize(entry.getResponse()));
    }

    public void save(String key, PaymentResponse response) {
        IdempotencyEntry entry = new IdempotencyEntry();
        entry.setKey(key);
        entry.setResponse(serialize(response));
        entry.setCreatedAt(LocalDateTime.now());
        entry.setExpiresAt(LocalDateTime.now().plusHours(24));
        repository.save(entry);
    }
}
```

</details>

Ключи обычно имеют срок жизни (24 часа) и хранятся в БД или Redis. Паттерн широко используется в платёжных системах (Stripe, Tinkoff, YooMoney).

> **На собеседовании:** ключевое — объяснить, зачем нужен Idempotency Key (для POST-запросов при сетевых ошибках) и как он работает (проверка + сохранение результата). Частая ошибка — путать идемпотентность метода и Idempotency Key.

[к оглавлению](#rest-api)

---

## Как проектировать REST API для связанных ресурсов?
<!-- grade: middle -->

Проектирование API для связанных ресурсов строится на выборе между вложенными URI, ссылками через идентификаторы и развёрнутыми ресурсами в зависимости от типа связи и потребностей клиента.

### 1. Вложенные ресурсы (Sub-resources)

Для отношения «один-ко-многим»:
```
GET    /api/users/42/orders         — все заказы пользователя 42
POST   /api/users/42/orders         — создать заказ для пользователя 42
GET    /api/users/42/orders/7       — заказ 7 пользователя 42
DELETE /api/users/42/orders/7       — удалить заказ 7
```

### 2. Ссылки через идентификаторы

```json
{
  "id": 7,
  "product": "Ноутбук",
  "userId": 42,
  "categoryId": 5
}
```

### 3. Развёрнутые (embedded) ресурсы

Включайте связанные данные по запросу для сокращения количества запросов:
```
GET /api/orders/7?expand=user,items
```
```json
{
  "id": 7,
  "status": "DELIVERED",
  "user": {"id": 42, "name": "Иван"},
  "items": [{"id": 1, "product": "Ноутбук", "quantity": 1}]
}
```

### 4. Отношение многие-ко-многим

Используйте связующий ресурс:
```
GET    /api/users/42/roles          — роли пользователя
PUT    /api/users/42/roles/3        — назначить роль 3
DELETE /api/users/42/roles/3        — убрать роль 3
```

### 5. Правила глубины вложенности

- Один уровень — хорошо: `/users/42/orders`
- Два уровня — допустимо: `/users/42/orders/7`
- Три и более — избегайте. Вместо `/users/42/orders/7/items/3` используйте `/order-items/3`

<details><summary>Реализация в Spring</summary>

```java
@RestController
@RequestMapping("/api/users/{userId}/orders")
@RequiredArgsConstructor
public class UserOrderController {

    private final OrderService orderService;

    @GetMapping
    public ResponseEntity<List<OrderDto>> getUserOrders(
            @PathVariable Long userId,
            @RequestParam(required = false) String status) {
        List<OrderDto> orders = orderService.findByUserId(userId, status);
        return ResponseEntity.ok(orders);
    }

    @PostMapping
    public ResponseEntity<OrderDto> createOrder(
            @PathVariable Long userId,
            @Valid @RequestBody CreateOrderRequest request) {
        OrderDto order = orderService.create(userId, request);
        URI location = URI.create("/api/users/" + userId + "/orders/" + order.id());
        return ResponseEntity.created(location).body(order);
    }

    @GetMapping("/{orderId}")
    public ResponseEntity<OrderDto> getOrder(
            @PathVariable Long userId,
            @PathVariable Long orderId) {
        OrderDto order = orderService.findByIdAndUserId(orderId, userId);
        return ResponseEntity.ok(order);
    }
}
```

</details>

Принцип: если ресурс имеет смысл только в контексте родительского — используйте вложенные URI. Если ресурс самостоятелен — выделяйте его на верхний уровень.

> **На собеседовании:** нужно знать все подходы (вложенные URI, идентификаторы, expand) и правило глубины вложенности (не более 2-3 уровней). Частая ошибка — глубокая вложенность URI или неиспользование параметра expand для уменьшения количества запросов.

[к оглавлению](#rest-api)

---

## Что такое подход API Design First?
<!-- grade: senior -->

API Design First (API-first) — подход к разработке, при котором спецификация API (контракт) создаётся до написания кода. Из спецификации автоматически генерируются серверные интерфейсы, клиентские SDK, документация и тесты.

### API-first vs Code-first

| Характеристика | API-first (Design First) | Code-first |
|---------------|--------------------------|------------|
| Последовательность | Спецификация -> Код | Код -> Спецификация (генерируется) |
| Источник истины | OpenAPI-спецификация | Исходный код (аннотации) |
| Параллельная разработка | Да (frontend и backend одновременно) | Нет (frontend ждёт backend) |
| Согласованность | Контракт фиксирован заранее | Контракт может меняться неконтролируемо |
| Инструменты | OpenAPI Generator, Swagger Codegen | springdoc-openapi, Swagger annotations |

### Преимущества API-first

- Параллельная разработка — frontend и backend работают одновременно.
- Контракт как документация — спецификация всегда актуальна.
- Автоматизированное тестирование — тесты генерируются на основе спецификации.
- Генерация клиентских SDK для любого языка.
- Раннее обнаружение ошибок — несовместимые изменения обнаруживаются на этапе проектирования.

<details><summary>Пример OpenAPI-спецификации</summary>

```yaml
openapi: 3.0.3
info:
  title: User Service API
  version: 1.0.0
  description: API для управления пользователями

servers:
  - url: https://api.example.com/v1

paths:
  /users:
    get:
      operationId: getUsers
      summary: Получить список пользователей
      tags:
        - Users
      parameters:
        - name: status
          in: query
          required: false
          schema:
            type: string
            enum: [ACTIVE, INACTIVE, BLOCKED]
        - name: page
          in: query
          schema:
            type: integer
            default: 0
        - name: size
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Список пользователей
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserPage'
    post:
      operationId: createUser
      summary: Создать пользователя
      tags:
        - Users
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: Пользователь создан
          headers:
            Location:
              schema:
                type: string
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserDto'
        '400':
          description: Ошибка валидации

components:
  schemas:
    CreateUserRequest:
      type: object
      required: [name, email]
      properties:
        name:
          type: string
          minLength: 1
          maxLength: 100
        email:
          type: string
          format: email
        age:
          type: integer
          minimum: 18

    UserDto:
      type: object
      properties:
        id:
          type: integer
          format: int64
        name:
          type: string
        email:
          type: string
        age:
          type: integer
        createdAt:
          type: string
          format: date-time

    UserPage:
      type: object
      properties:
        content:
          type: array
          items:
            $ref: '#/components/schemas/UserDto'
        totalElements:
          type: integer
          format: int64
        totalPages:
          type: integer

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - BearerAuth: []
```

</details>

<details><summary>Настройка генерации кода в Spring Boot (Maven)</summary>

```xml
<plugin>
    <groupId>org.openapitools</groupId>
    <artifactId>openapi-generator-maven-plugin</artifactId>
    <version>7.5.0</version>
    <executions>
        <execution>
            <goals>
                <goal>generate</goal>
            </goals>
            <configuration>
                <inputSpec>${project.basedir}/src/main/resources/openapi/api.yaml</inputSpec>
                <generatorName>spring</generatorName>
                <apiPackage>com.example.api</apiPackage>
                <modelPackage>com.example.model</modelPackage>
                <configOptions>
                    <interfaceOnly>true</interfaceOnly>
                    <useSpringBoot3>true</useSpringBoot3>
                    <useTags>true</useTags>
                    <openApiNullable>false</openApiNullable>
                    <skipDefaultInterface>false</skipDefaultInterface>
                </configOptions>
            </configuration>
        </execution>
    </executions>
</plugin>
```

Сгенерированный интерфейс:
```java
@Tag(name = "Users", description = "Управление пользователями")
@Generated("org.openapitools.codegen")
public interface UsersApi {

    @Operation(summary = "Получить список пользователей")
    @GetMapping(value = "/users", produces = "application/json")
    ResponseEntity<UserPage> getUsers(
        @RequestParam(required = false) String status,
        @RequestParam(defaultValue = "0") Integer page,
        @RequestParam(defaultValue = "20") Integer size
    );

    @Operation(summary = "Создать пользователя")
    @PostMapping(value = "/users", consumes = "application/json", produces = "application/json")
    ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request);
}
```

Реализация:
```java
@RestController
@RequiredArgsConstructor
public class UsersApiController implements UsersApi {

    private final UserService userService;

    @Override
    public ResponseEntity<UserPage> getUsers(String status, Integer page, Integer size) {
        UserPage users = userService.findAll(status, page, size);
        return ResponseEntity.ok(users);
    }

    @Override
    public ResponseEntity<UserDto> createUser(CreateUserRequest request) {
        UserDto created = userService.create(request);
        URI location = URI.create("/users/" + created.getId());
        return ResponseEntity.created(location).body(created);
    }
}
```

</details>

### Частые ошибки

- Генерация полного сервера вместо `interfaceOnly` — приводит к нечитаемому коду.
- Редактирование сгенерированного кода вручную — изменения будут перезаписаны.
- Расхождение спецификации и реализации — нужна CI/CD проверка (contract testing).

### Как используется в 2026

- API-first стал стандартом де-факто в крупных компаниях и микросервисных архитектурах.
- OpenAPI Generator 7.x поддерживает Spring Boot 3.x, Jakarta EE, virtual threads.
- Spectral и Redocly CLI используются для линтинга спецификаций в CI/CD.
- AsyncAPI — аналог OpenAPI для асинхронных API (Kafka, RabbitMQ, WebSocket).

> **На собеседовании:** нужно объяснить разницу между API-first и Code-first, назвать преимущества API-first (параллельная разработка, контракт как единый источник истины) и упомянуть `interfaceOnly=true`. Частая ошибка — не знать про OpenAPI Generator или путать его со Swagger Codegen.

[к оглавлению](#rest-api)

---

## Чем отличаются REST, GraphQL и gRPC? Когда что использовать?
<!-- grade: senior -->

REST, GraphQL и gRPC — три подхода к проектированию API, решающие разные задачи: REST ориентирован на ресурсы, GraphQL — на гибкие запросы клиента, gRPC — на высокопроизводительный RPC.

### Сравнительная таблица

| Характеристика | REST | GraphQL | gRPC |
|---------------|------|---------|------|
| Протокол | HTTP/1.1, HTTP/2 | HTTP/1.1, HTTP/2 | HTTP/2 |
| Формат данных | JSON, XML | JSON | Protocol Buffers (бинарный) |
| Схема/контракт | OpenAPI (опционально) | GraphQL Schema (обязательно) | Protobuf (.proto, обязательно) |
| Стиль | Ресурсо-ориентированный | Запросо-ориентированный | RPC (вызов процедур) |
| Количество endpoint-ов | Множество (по ресурсу) | Один (`/graphql`) | Множество (по сервису/методу) |
| Over-fetching | Да | Нет (клиент выбирает поля) | Нет (строгий контракт) |
| Under-fetching | Да (нужны доп. запросы) | Нет (один запрос) | Да |
| Streaming | Нет (SSE, WebSocket отдельно) | Subscriptions (WebSocket) | Да (нативная поддержка) |
| Кэширование | Встроенное (HTTP-кэш) | Сложное (один endpoint) | Нет нативного |
| Производительность | Средняя | Средняя | Высокая |

### Когда что использовать

| Сценарий | Рекомендация |
|----------|-------------|
| Публичные API, CRUD | REST |
| Мобильные/браузерные клиенты | REST |
| Сложные UI с разнородными данными (дашборды) | GraphQL |
| Множество клиентов с разными потребностями | GraphQL |
| Межсервисное взаимодействие, высокая нагрузка | gRPC |
| Real-time, streaming | gRPC |
| Полиглотные архитектуры | gRPC |

### Комбинированный подход (наиболее распространённый)

```
                    ┌──────────────────┐
                    │   API Gateway    │
                    │  (REST / GraphQL)│
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
        ┌─────▼─────┐ ┌─────▼─────┐ ┌─────▼─────┐
        │  Service A │ │  Service B │ │  Service C │
        └─────┬─────┘ └─────┬─────┘ └─────┬─────┘
              │              │              │
              └──────────────┼──────────────┘
                        gRPC (внутри)
```

<details><summary>Примеры кода для каждого подхода</summary>

REST (Spring):
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.findById(id));
    }
}
```

GraphQL (Spring):
```java
@Controller
public class UserGraphQLController {

    @QueryMapping
    public User user(@Argument Long id) {
        return userService.findById(id);
    }

    // Resolver для вложенного поля — загружается только если клиент запросил
    @SchemaMapping(typeName = "User", field = "orders")
    public List<Order> orders(User user) {
        return orderService.findByUserId(user.getId());
    }
}
```

gRPC:
```java
@GrpcService
public class UserGrpcService extends UserServiceGrpc.UserServiceImplBase {

    @Override
    public void getUser(GetUserRequest request,
                        StreamObserver<UserResponse> responseObserver) {
        User user = userService.findById(request.getId());
        UserResponse response = UserResponse.newBuilder()
            .setId(user.getId())
            .setName(user.getName())
            .build();
        responseObserver.onNext(response);
        responseObserver.onCompleted();
    }
}
```

</details>

### Частые ошибки

- Выбор GraphQL для простого CRUD — избыточная сложность.
- Использование REST для межсервисного взаимодействия с высокой нагрузкой — gRPC значительно эффективнее.
- N+1 проблема в GraphQL — без DataLoader вложенные поля вызывают лавину запросов к БД.
- Использование gRPC для публичного API — браузеры не поддерживают gRPC напрямую.

### Как используется в 2026

- Большинство компаний используют гибридный подход: REST/GraphQL для внешних клиентов, gRPC для внутренней коммуникации.
- Spring Boot 3.x имеет встроенную поддержку GraphQL и gRPC.
- Connect RPC — новый протокол, совместимый с gRPC, но работающий через обычный HTTP/JSON.

> **На собеседовании:** нужно чётко разграничить три подхода по назначению и привести примеры, когда какой использовать. Частая ошибка — говорить, что GraphQL заменяет REST. Это инструменты для разных задач, и в реальных системах они часто сосуществуют.

[к оглавлению](#rest-api)

---

## Как обеспечить обратную совместимость REST API?
<!-- grade: senior -->

Обратная совместимость (backward compatibility) — свойство API, при котором обновлённая версия продолжает корректно работать с существующими клиентами без необходимости их изменения.

### Неломающие изменения (non-breaking)

- Добавление нового поля в ответ — клиенты игнорируют неизвестные поля (tolerant reader).
- Добавление нового endpoint-а.
- Добавление нового опционального параметра запроса.
- Добавление нового значения enum (в ответе).
- Расширение допустимого диапазона значений.

### Ломающие изменения (breaking)

- Удаление или переименование поля из ответа.
- Изменение типа поля (`"age": 30` -> `"age": "30"`).
- Удаление endpoint-а.
- Добавление обязательного параметра.
- Изменение кода ответа.

### Стратегии обеспечения совместимости

| Стратегия | Описание |
|-----------|----------|
| Версионирование (URL/Header) | `/api/v1/users` и `/api/v2/users` работают параллельно |
| Deprecation + Sunset (RFC 8594, 8977) | Заголовки уведомляют о плановом удалении |
| Tolerant Reader (Закон Постела) | Клиент игнорирует неизвестные поля, не зависит от порядка |
| API Evolution | Постепенное развитие: добавление новых полей, deprecated для старых |
| Feature Flags | Управление поведением через заголовки |

### Tolerant Reader Pattern

> «Будь консервативен в том, что отправляешь, и либерален в том, что принимаешь.»

```java
// Настройка Jackson для игнорирования неизвестных полей
@JsonIgnoreProperties(ignoreUnknown = true)
public record UserDto(Long id, String name, String email) {}

// Или глобально
ObjectMapper mapper = new ObjectMapper();
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
```

<details><summary>Примеры реализации</summary>

Заголовки Deprecation и Sunset:
```java
@GetMapping("/api/v1/users")
public ResponseEntity<List<UserV1Dto>> getUsersV1() {
    List<UserV1Dto> users = userService.findAllV1();
    return ResponseEntity.ok()
        .header("Deprecation", "true")
        .header("Sunset", "Sat, 01 Nov 2026 00:00:00 GMT")
        .header("Link", "</api/v2/users>; rel=\"successor-version\"")
        .body(users);
}
```

Паттерн эволюции API:
```java
// Шаг 1: Добавляем новое поле, старое оставляем
public record UserDto(
    Long id,
    String name,
    String email,
    @Deprecated String userName,    // старое поле (deprecated)
    String displayName              // новое поле
) {
    @JsonProperty("userName")
    public String getUserName() {
        return displayName != null ? displayName : name;
    }
}

// Шаг 2: Через несколько релизов удаляем старое поле
```

Автоматическое обнаружение несовместимых изменений:
```bash
openapi-diff --fail-on-incompatible \
  api-spec-v1.yaml \
  api-spec-v2.yaml
```

</details>

### Частые ошибки

- Удаление полей без предварительного deprecated-периода.
- Изменение типа поля без версионирования.
- Добавление обязательного параметра в существующий endpoint.
- Отсутствие автоматической проверки совместимости в CI/CD.
- Бесконечная поддержка всех версий — вовремя удаляйте старые версии.

### Как используется в 2026

- openapi-diff и Optic интегрируются в CI/CD для автоматической проверки совместимости.
- API lifecycle management — платформы (Backstage, Kong) управляют жизненным циклом API.
- Contract testing (Pact, Spring Cloud Contract) — стандарт для проверки совместимости.
- Тренд на API Evolution вместо строгого версионирования.

> **На собеседовании:** ключевое — разделить изменения на ломающие и неломающие, знать Tolerant Reader и заголовки Deprecation/Sunset. Частая ошибка — не упомянуть автоматическую проверку совместимости в CI/CD.

[к оглавлению](#rest-api)

---

## Что такое асинхронные API и как их проектировать?
<!-- grade: senior -->

Асинхронные API — подходы к взаимодействию клиента и сервера, при которых обработка запроса не блокирует клиента и результат доставляется позже (через callback, polling или push-уведомление).

### Когда нужны

- Долгие операции — генерация отчёта, обработка видео, массовый импорт.
- Real-time обновления — чат, биржевые котировки, уведомления.
- Событийные уведомления — платёж завершён, заказ отправлен.
- Интеграции — webhook-и от внешних систем (Stripe, GitHub).

### Сравнение подходов

| Характеристика | Polling | Webhooks | SSE | WebSocket |
|---------------|---------|----------|-----|-----------|
| Направление | Клиент -> Сервер | Сервер -> Клиент | Сервер -> Клиент | Двунаправленный |
| Протокол | HTTP | HTTP | HTTP | WS (поверх HTTP) |
| Задержка | Высокая (интервал) | Низкая | Низкая | Очень низкая |
| Нагрузка на сервер | Высокая | Низкая | Средняя | Средняя |
| Масштабирование | Простое | Простое | Сложнее | Сложнее |
| Подходит для | Долгие операции | Интеграции, события | Уведомления, ленты | Чат, real-time игры |

### Паттерн 1: Polling (202 Accepted + Location)

<details><summary>Реализация</summary>

```java
// Запуск долгой операции
@PostMapping("/api/reports")
public ResponseEntity<TaskStatus> createReport(
        @Valid @RequestBody ReportRequest request) {
    String taskId = reportService.startGeneration(request);

    URI statusUri = URI.create("/api/reports/tasks/" + taskId);
    TaskStatus status = new TaskStatus(taskId, "PENDING", null);

    return ResponseEntity.accepted()
        .location(statusUri)
        .header("Retry-After", "5")
        .body(status);
}

// Проверка статуса
@GetMapping("/api/reports/tasks/{taskId}")
public ResponseEntity<TaskStatus> getTaskStatus(@PathVariable String taskId) {
    TaskStatus status = reportService.getStatus(taskId);

    if ("COMPLETED".equals(status.status())) {
        return ResponseEntity.status(HttpStatus.SEE_OTHER)
            .location(URI.create(status.resultUrl()))
            .body(status);
    }
    if ("FAILED".equals(status.status())) {
        return ResponseEntity.ok(status);
    }
    return ResponseEntity.ok()
        .header("Retry-After", "5")
        .body(status);
}

public record TaskStatus(
    String taskId,
    String status,       // PENDING, PROCESSING, COMPLETED, FAILED
    String resultUrl,
    Integer progress,
    String errorMessage
) {
    public TaskStatus(String taskId, String status, String resultUrl) {
        this(taskId, status, resultUrl, null, null);
    }
}
```

</details>

### Паттерн 2: Webhooks (Callback URL)

<details><summary>Реализация</summary>

```java
@PostMapping("/api/webhooks")
public ResponseEntity<WebhookRegistration> registerWebhook(
        @Valid @RequestBody WebhookRequest request) {
    WebhookRegistration registration = webhookService.register(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(registration);
}

public record WebhookRequest(
    @NotBlank String url,           // URL для callback
    @NotEmpty Set<String> events,   // ["order.completed", "payment.failed"]
    String secret                   // секрет для подписи
) {}

@Service
@RequiredArgsConstructor
@Slf4j
public class WebhookSender {

    private final RestClient restClient;
    private final WebhookRepository webhookRepository;

    @Async
    @TransactionalEventListener
    public void onOrderCompleted(OrderCompletedEvent event) {
        List<WebhookRegistration> hooks = webhookRepository
            .findByEvent("order.completed");
        for (WebhookRegistration hook : hooks) {
            sendWebhook(hook, event);
        }
    }

    private void sendWebhook(WebhookRegistration hook, Object payload) {
        String body = objectMapper.writeValueAsString(payload);
        String signature = HmacUtils.hmacSha256Hex(hook.getSecret(), body);

        try {
            restClient.post()
                .uri(hook.getUrl())
                .header("X-Webhook-Signature", "sha256=" + signature)
                .header("X-Webhook-Event", "order.completed")
                .body(body)
                .retrieve()
                .toBodilessEntity();
        } catch (Exception e) {
            log.error("Webhook delivery failed: {}", hook.getUrl(), e);
            webhookRetryService.scheduleRetry(hook, payload);
        }
    }
}
```

</details>

### Паттерн 3: SSE (Server-Sent Events)

Сервер отправляет поток событий клиенту через постоянное HTTP-соединение. Однонаправленный.

<details><summary>Реализация</summary>

```java
@RestController
@RequestMapping("/api/events")
public class SseController {

    private final SseEmitterService sseService;

    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter streamEvents(@RequestParam Long userId) {
        SseEmitter emitter = new SseEmitter(0L);
        sseService.register(userId, emitter);

        emitter.onCompletion(() -> sseService.unregister(userId));
        emitter.onTimeout(() -> sseService.unregister(userId));

        return emitter;
    }
}

@Service
public class SseEmitterService {

    private final Map<Long, SseEmitter> emitters = new ConcurrentHashMap<>();

    public void sendEvent(Long userId, String eventType, Object data) {
        SseEmitter emitter = emitters.get(userId);
        if (emitter != null) {
            try {
                emitter.send(SseEmitter.event()
                    .name(eventType)
                    .data(data, MediaType.APPLICATION_JSON)
                    .id(UUID.randomUUID().toString())
                    .reconnectTime(3000));
            } catch (IOException e) {
                emitters.remove(userId);
            }
        }
    }
}
```

</details>

### Паттерн 4: WebSocket (двунаправленный)

<details><summary>Реализация</summary>

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/topic", "/queue");
        registry.setApplicationDestinationPrefixes("/app");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws")
            .setAllowedOriginPatterns("*")
            .withSockJS();
    }
}

@Controller
@RequiredArgsConstructor
public class ChatController {

    private final SimpMessagingTemplate messagingTemplate;

    @MessageMapping("/chat.send")
    public void sendMessage(@Payload ChatMessage message,
                            SimpMessageHeaderAccessor headerAccessor) {
        String userId = headerAccessor.getUser().getName();
        message.setSenderId(userId);
        message.setTimestamp(Instant.now());

        messagingTemplate.convertAndSend(
            "/topic/chat." + message.getRoomId(), message);
    }
}
```

</details>

### Частые ошибки

- Использование WebSocket там, где достаточно SSE.
- Отсутствие retry-механизма для webhook.
- Polling без `Retry-After` заголовка.
- Хранение состояния SSE/WebSocket в памяти без учёта масштабирования.

### Как используется в 2026

- Spring WebFlux + SSE — реактивный подход для потоковых данных.
- AsyncAPI 3.0 — стандарт описания асинхронных API.
- Virtual threads (Java 21+) упрощают обработку большого числа SSE/WebSocket-соединений.
- Standard Webhooks (standardwebhooks.com) — единый формат подписи и retry.

> **На собеседовании:** нужно знать все четыре паттерна (Polling, Webhooks, SSE, WebSocket) и когда использовать какой. Polling — самый простой для долгих операций, Webhooks — для интеграций, SSE — для уведомлений, WebSocket — только для полнодуплексной связи. Частая ошибка — предлагать WebSocket для всех async-задач.

[к оглавлению](#rest-api)

---

## Что такое BFF (Backend for Frontend) и API Gateway?
<!-- grade: senior -->

BFF (Backend for Frontend) и API Gateway — архитектурные паттерны, определяющие, как клиенты взаимодействуют с backend-сервисами в микросервисной архитектуре.

### API Gateway — единая точка входа

API Gateway — прокси-сервер, являющийся единственной точкой входа для всех клиентов. Маршрутизирует запросы и предоставляет сквозную функциональность.

```
    Web App     Mobile App     Partner API
       \            |            /
        ┌───────────▼───────────┐
        │      API Gateway      │
        │ (routing, auth, rate  │
        │  limiting, logging)   │
        └────┬────┬────┬────────┘
             │    │    │
        ┌────▼┐ ┌─▼──┐ ┌▼────┐
        │User ││Order││Stock│
        │Svc  ││ Svc ││ Svc │
        └─────┘ └────┘ └─────┘
```

Функции: маршрутизация, аутентификация/авторизация, Rate Limiting, логирование, трансформация запросов, кэширование, Circuit Breaker.

### BFF — отдельный API для каждого типа клиента

Для каждого типа клиента (web, mobile, admin) создаётся отдельный backend-сервис, оптимизированный под потребности этого клиента.

```
    Web App         Mobile App        Admin Panel
       │                │                  │
  ┌────▼─────┐    ┌─────▼──────┐    ┌─────▼──────┐
  │ Web BFF  │    │ Mobile BFF │    │ Admin BFF  │
  │(подробные│    │(компактные │    │(все данные)│
  │ данные)  │    │ данные)    │    │            │
  └──┬───┬───┘    └──┬───┬─────┘    └──┬───┬─────┘
     │   │           │   │             │   │
     └───┴───────────┴───┴─────────────┴───┘
              Микросервисы (User, Order, Stock)
```

### Сравнение подходов

| Характеристика | Прямые вызовы | API Gateway | BFF |
|---------------|---------------|-------------|-----|
| Сложность | Минимальная | Средняя | Высокая |
| Оптимизация под клиента | Нет | Частичная | Полная |
| Агрегация данных | На клиенте | Ограниченная | Полная |
| Over-fetching | Да | Да | Нет |
| Безопасность | На каждом сервисе | Централизованная | На каждом BFF |

<details><summary>Примеры реализации</summary>

API Gateway на Spring Cloud Gateway:
```java
@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
            .route("user-service", r -> r
                .path("/api/users/**")
                .filters(f -> f
                    .stripPrefix(1)
                    .circuitBreaker(cb -> cb
                        .setName("userServiceCB")
                        .setFallbackUri("forward:/fallback/users"))
                    .retry(retry -> retry.setRetries(3)))
                .uri("lb://user-service"))
            .route("order-service", r -> r
                .path("/api/orders/**")
                .filters(f -> f
                    .stripPrefix(1)
                    .requestRateLimiter(rl -> rl
                        .setRateLimiter(redisRateLimiter())
                        .setKeyResolver(userKeyResolver())))
                .uri("lb://order-service"))
            .build();
    }
}
```

Mobile BFF — один вызов для главного экрана:
```java
@RestController
@RequestMapping("/mobile/api")
@RequiredArgsConstructor
public class MobileBffController {

    private final UserServiceClient userClient;
    private final OrderServiceClient orderClient;
    private final NotificationServiceClient notificationClient;

    @GetMapping("/home")
    public ResponseEntity<MobileHomeResponse> getHomeScreen(
            @AuthenticationPrincipal UserPrincipal principal) {

        CompletableFuture<UserSummary> userFuture =
            CompletableFuture.supplyAsync(() -> userClient.getSummary(principal.getId()));
        CompletableFuture<List<OrderBrief>> ordersFuture =
            CompletableFuture.supplyAsync(() -> orderClient.getRecent(principal.getId(), 5));
        CompletableFuture<Integer> notifCountFuture =
            CompletableFuture.supplyAsync(() -> notificationClient.getUnreadCount(principal.getId()));

        CompletableFuture.allOf(userFuture, ordersFuture, notifCountFuture).join();

        MobileHomeResponse response = new MobileHomeResponse(
            userFuture.join(), ordersFuture.join(), notifCountFuture.join()
        );
        return ResponseEntity.ok(response);
    }
}
```

</details>

### Когда что использовать

- **API Gateway (без BFF):** все клиенты имеют одинаковые потребности, нужна централизованная маршрутизация.
- **BFF:** клиенты имеют существенно разные потребности, нужна агрегация данных из нескольких сервисов.
- **API Gateway + BFF:** Gateway для auth/rate limiting/logging, BFF для агрегации и адаптации данных.

### Частые ошибки

- Реализация бизнес-логики в API Gateway — шлюз должен быть «тонким».
- Единый BFF для всех клиентов — это просто API Gateway, а не BFF.
- Синхронные вызовы к микросервисам из BFF — используйте CompletableFuture или virtual threads.
- Отсутствие Circuit Breaker в BFF.

### Как используется в 2026

- Spring Cloud Gateway — стандарт для API Gateway в экосистеме Spring.
- GraphQL как альтернатива BFF для гибких запросов.
- BFF + Virtual Threads (Java 21+) — упрощают параллельные вызовы.
- API Gateway as a Service — Kong, AWS API Gateway, Azure APIM.

> **На собеседовании:** нужно чётко разграничить API Gateway (сквозная функциональность: auth, routing, rate limiting) и BFF (агрегация и адаптация данных под конкретный клиент). Частая ошибка — путать их или считать, что BFF заменяет API Gateway. Они дополняют друг друга.

[к оглавлению](#rest-api)

---

## Как проектировать API для микросервисной архитектуры?
<!-- grade: senior -->

Проектирование API для микросервисов учитывает распределённую природу системы: сетевую ненадёжность, независимое развёртывание, согласованность данных и отказоустойчивость.

### Синхронное взаимодействие (REST / gRPC)

Один сервис напрямую вызывает другой и ждёт ответа.

<details><summary>Пример вызова через RestClient</summary>

```java
@Service
@RequiredArgsConstructor
public class OrderService {

    private final RestClient userServiceClient;

    public OrderDto createOrder(Long userId, CreateOrderRequest request) {
        UserDto user = userServiceClient.get()
            .uri("/api/users/{id}", userId)
            .retrieve()
            .body(UserDto.class);

        if (user == null) {
            throw new UserNotFoundException("User not found: " + userId);
        }

        Order order = orderMapper.toEntity(request);
        order.setUserId(userId);
        order.setUserEmail(user.email());

        return orderMapper.toDto(orderRepository.save(order));
    }
}

@Configuration
public class RestClientConfig {

    @Bean
    public RestClient userServiceClient(RestClient.Builder builder) {
        return builder
            .baseUrl("http://user-service:8080")
            .defaultHeader("Content-Type", "application/json")
            .requestInterceptor(new TokenRelayInterceptor())
            .build();
    }
}
```

</details>

### Асинхронное взаимодействие (Events / Kafka)

Сервисы общаются через события — отправитель публикует событие, получатели подписаны на него.

<details><summary>Пример с Kafka</summary>

```java
// Публикация события
@Service
@RequiredArgsConstructor
public class OrderService {

    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;

    @Transactional
    public OrderDto createOrder(CreateOrderRequest request) {
        Order saved = orderRepository.save(orderMapper.toEntity(request));

        OrderEvent event = new OrderEvent(
            saved.getId(), "ORDER_CREATED",
            saved.getUserId(), saved.getTotal(), Instant.now()
        );
        kafkaTemplate.send("order-events", saved.getId().toString(), event);

        return orderMapper.toDto(saved);
    }
}

// Обработка события в другом сервисе
@Service
@Slf4j
public class OrderEventListener {

    @KafkaListener(topics = "order-events", groupId = "notification-service")
    public void handleOrderEvent(OrderEvent event) {
        if ("ORDER_CREATED".equals(event.eventType())) {
            notificationService.sendOrderConfirmation(event.userId(), event.orderId());
        }
    }
}
```

</details>

### API Composition — агрегация данных из нескольких сервисов

<details><summary>Пример с Virtual Threads (Java 21+)</summary>

```java
@RestController
@RequestMapping("/api/customer-view")
@RequiredArgsConstructor
public class CustomerViewController {

    private final RestClient userServiceClient;
    private final RestClient orderServiceClient;
    private final RestClient loyaltyServiceClient;

    @GetMapping("/{userId}")
    public ResponseEntity<CustomerView> getCustomerView(@PathVariable Long userId) {
        try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
            var userTask = scope.fork(() ->
                userServiceClient.get()
                    .uri("/api/users/{id}", userId)
                    .retrieve()
                    .body(UserDto.class));

            var ordersTask = scope.fork(() ->
                orderServiceClient.get()
                    .uri("/api/orders?userId={id}&limit=5", userId)
                    .retrieve()
                    .body(new ParameterizedTypeReference<List<OrderDto>>() {}));

            var loyaltyTask = scope.fork(() ->
                loyaltyServiceClient.get()
                    .uri("/api/loyalty/{id}", userId)
                    .retrieve()
                    .body(LoyaltyDto.class));

            scope.join().throwIfFailed();

            return ResponseEntity.ok(new CustomerView(
                userTask.get(), ordersTask.get(), loyaltyTask.get()
            ));
        }
    }
}
```

</details>

### Circuit Breaker — защита от каскадных сбоев

<details><summary>Пример с Resilience4j</summary>

```java
@Service
@RequiredArgsConstructor
public class UserServiceClient {

    private final RestClient restClient;

    @CircuitBreaker(name = "userService", fallbackMethod = "getUserFallback")
    @Retry(name = "userService")
    @TimeLimiter(name = "userService")
    public UserDto getUser(Long id) {
        return restClient.get()
            .uri("/api/users/{id}", id)
            .retrieve()
            .body(UserDto.class);
    }

    private UserDto getUserFallback(Long id, Throwable ex) {
        log.warn("User service unavailable, returning fallback for user {}", id, ex);
        return new UserDto(id, "Unknown User", null);
    }
}
```

```yaml
resilience4j:
  circuitbreaker:
    instances:
      userService:
        sliding-window-size: 10
        failure-rate-threshold: 50
        wait-duration-in-open-state: 10s
  retry:
    instances:
      userService:
        max-attempts: 3
        wait-duration: 500ms
        exponential-backoff-multiplier: 2
  timelimiter:
    instances:
      userService:
        timeout-duration: 3s
```

</details>

### Saga Pattern — распределённые транзакции

Saga — последовательность локальных транзакций, где каждый шаг имеет компенсирующее действие для отката.

| Характеристика | Хореография (события) | Оркестрация (координатор) |
|---------------|----------------------|--------------------------|
| Связанность | Слабая (через события) | Средняя (координатор знает все шаги) |
| Простота | Простые Saga | Сложные Saga с множеством шагов |
| Отслеживание | Сложно (события распределены) | Просто (всё в координаторе) |
| Единая точка отказа | Нет | Да (координатор) |

<details><summary>Примеры Saga</summary>

Хореографическая Saga (через события):
```java
@Service
@RequiredArgsConstructor
public class PaymentEventListener {

    private final PaymentService paymentService;
    private final KafkaTemplate<String, PaymentEvent> kafkaTemplate;

    @KafkaListener(topics = "order-events", groupId = "payment-service")
    public void handleOrderCreated(OrderEvent event) {
        if (!"ORDER_CREATED".equals(event.eventType())) return;

        try {
            paymentService.processPayment(event.orderId(), event.total());
            kafkaTemplate.send("payment-events",
                new PaymentEvent(event.orderId(), "PAYMENT_COMPLETED"));
        } catch (InsufficientFundsException e) {
            kafkaTemplate.send("payment-events",
                new PaymentEvent(event.orderId(), "PAYMENT_FAILED"));
        }
    }
}
```

Оркестрационная Saga:
```java
@Service
@RequiredArgsConstructor
@Slf4j
public class OrderSagaOrchestrator {

    private final PaymentServiceClient paymentClient;
    private final StockServiceClient stockClient;
    private final DeliveryServiceClient deliveryClient;

    @Transactional
    public OrderResult executeOrderSaga(Order order) {
        try {
            StockReservation reservation = stockClient.reserve(order.getItems());

            PaymentResult payment;
            try {
                payment = paymentClient.charge(order.getUserId(), order.getTotal());
            } catch (Exception e) {
                stockClient.cancelReservation(reservation.getId());
                throw e;
            }

            try {
                deliveryClient.schedule(order.getId(), order.getDeliveryAddress());
            } catch (Exception e) {
                paymentClient.refund(payment.getId());
                stockClient.cancelReservation(reservation.getId());
                throw e;
            }

            order.setStatus(OrderStatus.CONFIRMED);
            return OrderResult.success(order);

        } catch (Exception e) {
            log.error("Order saga failed for order {}", order.getId(), e);
            order.setStatus(OrderStatus.FAILED);
            return OrderResult.failure(order, e.getMessage());
        }
    }
}
```

</details>

### Ключевые принципы

- Каждый микросервис владеет своими данными (database per service).
- Синхронная связь (REST/gRPC) — для немедленного ответа. Асинхронная (Kafka) — для событий.
- Circuit Breaker обязателен для всех синхронных вызовов.
- Saga Pattern заменяет распределённые транзакции.
- Все обработчики событий должны быть идемпотентными.

### Частые ошибки

- Общая база данных для нескольких сервисов — нарушает автономность.
- Отсутствие Circuit Breaker — каскадный сбой всей системы.
- Синхронные цепочки вызовов (A -> B -> C -> D) — увеличивают латентность.
- Распределённые транзакции (2PC) — не масштабируются, используйте Saga.
- Слишком мелкие сервисы (nano-services) — overhead превышает пользу.

### Как используется в 2026

- Spring Boot 3.x + Virtual Threads — синхронный код с производительностью реактивного.
- Spring Modulith — модульный монолит как альтернатива для проектов среднего размера.
- Testcontainers — стандарт для интеграционного тестирования.
- OpenTelemetry — единый стандарт для distributed tracing, metrics и logging.
- Service Mesh (Istio, Linkerd) берёт на себя retry, circuit breaker и mTLS.

> **На собеседовании:** нужно показать знание обоих типов взаимодействия (синхронное и асинхронное), Circuit Breaker и Saga Pattern. Частая ошибка — не упомянуть принцип database per service или предлагать 2PC вместо Saga для распределённых транзакций.

[к оглавлению](#rest-api)
