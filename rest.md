[Вопросы для собеседования](README.md)

# REST API
+ [Что такое REST?](#Что-такое-REST)
+ [Что такое RESTful?](#Что-такое-RESTful)
+ [Какие ограничения (constraints) определяет REST?](#Какие-ограничения-constraints-определяет-REST)
+ [Что такое ресурс в REST?](#Что-такое-ресурс-в-REST)
+ [Какие HTTP-методы используются в REST и какова их семантика?](#Какие-HTTP-методы-используются-в-REST-и-какова-их-семантика)
+ [Что такое идемпотентность и безопасность HTTP-методов?](#Что-такое-идемпотентность-и-безопасность-HTTP-методов)
+ [В чём разница между PUT и POST?](#В-чём-разница-между-PUT-и-POST)
+ [В чём разница между PUT и PATCH?](#В-чём-разница-между-PUT-и-PATCH)
+ [Какие существуют коды HTTP-ответов и когда их использовать?](#Какие-существуют-коды-HTTP-ответов-и-когда-их-использовать)
+ [Каковы лучшие практики проектирования URI?](#Каковы-лучшие-практики-проектирования-URI)
+ [Как версионировать REST API?](#Как-версионировать-REST-API)
+ [Что такое Content Negotiation?](#Что-такое-Content-Negotiation)
+ [Что такое HATEOAS?](#Что-такое-HATEOAS)
+ [В чём отличия REST от SOAP?](#В-чём-отличия-REST-от-SOAP)
+ [Что такое Richardson Maturity Model?](#Что-такое-Richardson-Maturity-Model)
+ [Как реализовать пагинацию, фильтрацию и сортировку в REST API?](#Как-реализовать-пагинацию-фильтрацию-и-сортировку-в-REST-API)
+ [Как обрабатывать ошибки в REST API?](#Как-обрабатывать-ошибки-в-REST-API)
+ [Какие способы аутентификации используются в REST API?](#Какие-способы-аутентификации-используются-в-REST-API)
+ [Что такое OAuth 2.0 и как он работает?](#Что-такое-OAuth-20-и-как-он-работает)
+ [Как работает кэширование в REST?](#Как-работает-кэширование-в-REST)
+ [Что такое CORS и как его настроить в Spring?](#Что-такое-CORS-и-как-его-настроить-в-Spring)
+ [Как документировать REST API с помощью Swagger/OpenAPI?](#Как-документировать-REST-API-с-помощью-SwaggerOpenAPI)
+ [Как реализовать REST API в Spring?](#Как-реализовать-REST-API-в-Spring)
+ [Что такое ResponseEntity и зачем он нужен?](#Что-такое-ResponseEntity-и-зачем-он-нужен)
+ [Что такое Rate Limiting и как его реализовать?](#Что-такое-Rate-Limiting-и-как-его-реализовать)
+ [Как обеспечить безопасность REST API?](#Как-обеспечить-безопасность-REST-API)
+ [Что такое идемпотентный ключ (Idempotency Key)?](#Что-такое-идемпотентный-ключ-Idempotency-Key)
+ [Как проектировать REST API для связанных ресурсов?](#Как-проектировать-REST-API-для-связанных-ресурсов)
+ [Что такое подход API Design First?](#Что-такое-подход-API-Design-First)
+ [Чем отличаются REST, GraphQL и gRPC? Когда что использовать?](#Чем-отличаются-REST-GraphQL-и-gRPC-Когда-что-использовать)
+ [Как обеспечить обратную совместимость REST API?](#Как-обеспечить-обратную-совместимость-REST-API)
+ [Что такое асинхронные API и как их проектировать?](#Что-такое-асинхронные-API-и-как-их-проектировать)
+ [Что такое BFF (Backend for Frontend) и API Gateway?](#Что-такое-BFF-Backend-for-Frontend-и-API-Gateway)
+ [Как проектировать API для микросервисной архитектуры?](#Как-проектировать-API-для-микросервисной-архитектуры)

## Что такое REST?
__REST (Representational State Transfer)__ — это архитектурный стиль взаимодействия компонентов распределённой системы, предложенный Роем Филдингом (Roy Fielding) в 2000 году в его докторской диссертации.

REST не является протоколом или стандартом — это набор архитектурных принципов (ограничений), которым должна соответствовать система. REST основан на использовании протокола HTTP и описывает способ взаимодействия клиента и сервера через обмен представлениями ресурсов.

Ключевые идеи REST:
+ Всё является **ресурсом** (пользователь, заказ, товар и т.д.).
+ Каждый ресурс имеет уникальный **идентификатор (URI)**.
+ Взаимодействие с ресурсами происходит через стандартные **HTTP-методы** (GET, POST, PUT, DELETE и др.).
+ Ресурс может иметь различные **представления** (JSON, XML и др.).
+ Взаимодействие не хранит состояния (**stateless**) — каждый запрос содержит всю необходимую информацию.

[к оглавлению](#REST-API)

## Что такое RESTful?
**RESTful** — это прилагательное, описывающее систему (обычно веб-сервис или API), которая полностью соответствует принципам и ограничениям архитектурного стиля REST.

Разница между REST и RESTful:
+ **REST** — архитектурный стиль (набор принципов).
+ **RESTful** — конкретная реализация, следующая этим принципам.

Говоря «RESTful API», мы подразумеваем API, которое:
+ использует HTTP-методы по назначению;
+ работает с ресурсами через URI;
+ обменивается данными в стандартных форматах (JSON, XML);
+ является stateless;
+ использует стандартные коды HTTP-ответов.

На практике большинство так называемых «REST API» не являются полностью RESTful (например, не реализуют HATEOAS), а представляют собой «REST-like» или «HTTP API».

[к оглавлению](#REST-API)

## Какие ограничения (constraints) определяет REST?
REST определяет шесть архитектурных ограничений, пять из которых обязательны и одно опциональное:

**1. Клиент-сервер (Client-Server)**
Разделение ответственности: клиент отвечает за пользовательский интерфейс, сервер — за хранение и обработку данных. Это позволяет независимо развивать клиентскую и серверную части.

**2. Отсутствие состояния (Stateless)**
Каждый запрос от клиента к серверу должен содержать всю информацию, необходимую для обработки. Сервер не хранит состояния клиента между запросами. Сессия хранится на стороне клиента (например, в виде токена).

Преимущества:
+ упрощается масштабирование (любой сервер может обработать любой запрос);
+ повышается надёжность;
+ упрощается мониторинг.

**3. Кэширование (Cacheable)**
Ответы сервера должны явно или неявно указывать, могут ли они быть кэшированы. Правильное кэширование снижает нагрузку на сервер и улучшает производительность клиента.

**4. Единообразие интерфейса (Uniform Interface)**
Это ключевое ограничение REST, включающее четыре подпринципа:
+ **Идентификация ресурсов** — каждый ресурс идентифицируется через URI.
+ **Манипуляция ресурсами через представления** — клиент работает с представлениями ресурсов (JSON, XML), а не с самими ресурсами напрямую.
+ **Самоописательные сообщения** — каждое сообщение содержит достаточно информации для его обработки (Content-Type, метод, заголовки).
+ **HATEOAS** (Hypermedia as the Engine of Application State) — клиент взаимодействует с приложением полностью через гипермедиа, предоставляемую сервером.

**5. Многоуровневая система (Layered System)**
Архитектура может состоять из нескольких уровней (прокси, балансировщики, шлюзы). Клиент не знает, взаимодействует ли он напрямую с сервером или с промежуточным уровнем.

**6. Код по требованию (Code on Demand) — опционально**
Сервер может расширять функциональность клиента, передавая исполняемый код (например, JavaScript). Это единственное опциональное ограничение.

[к оглавлению](#REST-API)

## Что такое ресурс в REST?
**Ресурс** — это ключевая абстракция в REST. Ресурсом может быть любая именованная информация: документ, изображение, коллекция других ресурсов, временная сущность (например, текущая погода) и т.д.

Каждый ресурс идентифицируется одним или более **URI (Uniform Resource Identifier)**:
```
/api/users          — коллекция пользователей
/api/users/42       — конкретный пользователь с id=42
/api/users/42/orders — заказы пользователя с id=42
```

Типы ресурсов:
+ **Коллекция (Collection)** — набор ресурсов одного типа (`/api/users`).
+ **Элемент (Document)** — единичный ресурс (`/api/users/42`).
+ **Хранилище (Store)** — управляемое клиентом хранилище ресурсов (`/api/users/42/favorites`).
+ **Контроллер (Controller)** — процедурная концепция, для действий, которые не укладываются в CRUD (`/api/users/42/activate`).

**Представление ресурса** — это конкретное отображение состояния ресурса в определённом формате (JSON, XML, HTML). Один и тот же ресурс может иметь разные представления в зависимости от заголовка `Accept`.

```json
{
  "id": 42,
  "name": "Иван Петров",
  "email": "ivan@example.com"
}
```

[к оглавлению](#REST-API)

## Какие HTTP-методы используются в REST и какова их семантика?
В REST используются стандартные HTTP-методы для операций над ресурсами:

| Метод | CRUD-операция | Описание | Идемпотентный | Безопасный |
|-------|--------------|----------|:-------------:|:----------:|
| **GET** | Read | Получение ресурса | Да | Да |
| **POST** | Create | Создание нового ресурса | Нет | Нет |
| **PUT** | Update/Replace | Полная замена ресурса | Да | Нет |
| **PATCH** | Update/Modify | Частичное обновление ресурса | Нет* | Нет |
| **DELETE** | Delete | Удаление ресурса | Да | Нет |
| **OPTIONS** | — | Получение допустимых методов | Да | Да |
| **HEAD** | — | Аналог GET, но без тела ответа | Да | Да |

*PATCH может быть идемпотентным в зависимости от реализации.

**GET** — получает представление ресурса. Не должен изменять состояние сервера.
```
GET /api/users/42 HTTP/1.1
Accept: application/json
```

**POST** — создаёт новый ресурс. URI нового ресурса определяется сервером.
```
POST /api/users HTTP/1.1
Content-Type: application/json

{"name": "Иван", "email": "ivan@example.com"}
```

**PUT** — заменяет ресурс целиком. Если ресурс не существует, может создать его.
```
PUT /api/users/42 HTTP/1.1
Content-Type: application/json

{"name": "Иван Петров", "email": "ivan@example.com", "age": 30}
```

**PATCH** — частично обновляет ресурс. Отправляются только изменяемые поля.
```
PATCH /api/users/42 HTTP/1.1
Content-Type: application/json

{"email": "new-email@example.com"}
```

**DELETE** — удаляет ресурс.
```
DELETE /api/users/42 HTTP/1.1
```

**OPTIONS** — возвращает допустимые методы для ресурса. Используется в CORS-запросах (preflight).
```
OPTIONS /api/users HTTP/1.1
```

**HEAD** — аналогичен GET, но возвращает только заголовки без тела ответа. Используется для проверки существования ресурса или получения метаданных.
```
HEAD /api/users/42 HTTP/1.1
```

[к оглавлению](#REST-API)

## Что такое идемпотентность и безопасность HTTP-методов?
**Безопасный метод (Safe method)** — метод, который не изменяет состояние ресурса на сервере. Вызов безопасного метода не создаёт побочных эффектов. Безопасные методы: `GET`, `HEAD`, `OPTIONS`.

**Идемпотентный метод (Idempotent method)** — метод, при котором многократный одинаковый запрос приводит к тому же результату, что и однократный. Иными словами, повторный вызов не изменяет состояние сервера сверх того, что произвёл первый вызов.

Идемпотентные методы: `GET`, `HEAD`, `OPTIONS`, `PUT`, `DELETE`.

Примеры:
+ `DELETE /api/users/42` — первый вызов удалит пользователя, второй вернёт 404, но состояние сервера не изменится дополнительно. Метод идемпотентный.
+ `PUT /api/users/42` с одним и тем же телом — каждый вызов приведёт к одному и тому же состоянию ресурса. Метод идемпотентный.
+ `POST /api/users` — каждый вызов может создать нового пользователя. Метод **не** идемпотентный.
+ `PATCH /api/users/42` с `{"age": 31}` — идемпотентный, но `PATCH /api/users/42` с `{"age": "increment"}` — не идемпотентный. Зависит от семантики операции.

Все безопасные методы являются идемпотентными, но не все идемпотентные методы безопасны:
```
Безопасные ⊂ Идемпотентные ⊂ Все методы
GET, HEAD, OPTIONS ⊂ GET, HEAD, OPTIONS, PUT, DELETE ⊂ GET, HEAD, OPTIONS, PUT, DELETE, POST, PATCH
```

Понимание идемпотентности критично для построения надёжных систем: клиент может безопасно повторить идемпотентный запрос при сетевых ошибках, не опасаясь дублирования действий.

[к оглавлению](#REST-API)

## В чём разница между PUT и POST?
| Характеристика | POST | PUT |
|---------------|------|-----|
| Назначение | Создание нового ресурса | Создание или полная замена ресурса |
| URI | Указывает на коллекцию | Указывает на конкретный ресурс |
| Идемпотентность | Нет | Да |
| Кто определяет URI нового ресурса | Сервер | Клиент |
| Пример | `POST /api/users` | `PUT /api/users/42` |

**POST** — клиент говорит серверу: «создай ресурс в коллекции, ты сам определи его идентификатор»:
```
POST /api/users HTTP/1.1
Content-Type: application/json

{"name": "Иван"}

Ответ:
HTTP/1.1 201 Created
Location: /api/users/42
```

**PUT** — клиент говорит серверу: «помести (или замени) ресурс по данному URI»:
```
PUT /api/users/42 HTTP/1.1
Content-Type: application/json

{"name": "Иван", "email": "ivan@example.com", "age": 30}

Ответ:
HTTP/1.1 200 OK     (если ресурс обновлён)
HTTP/1.1 201 Created (если ресурс создан)
```

Главное правило: если клиент знает URI ресурса и может его задать — используйте `PUT`. Если URI определяет сервер — используйте `POST`.

[к оглавлению](#REST-API)

## В чём разница между PUT и PATCH?
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

**PUT** — замена целиком. Если поле не указано, оно будет сброшено:
```
PUT /api/users/42
{"name": "Иван Петров", "email": "ivan@example.com", "age": 31}
```
Если отправить только `{"name": "Иван Петров"}`, поля `email` и `age` станут `null`.

**PATCH** — частичное обновление. Изменяются только переданные поля:
```
PATCH /api/users/42
{"age": 31}
```
Поля `name` и `email` останутся без изменений.

Пример в Spring:
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

[к оглавлению](#REST-API)

## Какие существуют коды HTTP-ответов и когда их использовать?
HTTP-коды ответов делятся на 5 классов:

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

Типичное использование в REST API:
```
POST   /api/users       → 201 Created
GET    /api/users       → 200 OK
GET    /api/users/42    → 200 OK или 404 Not Found
PUT    /api/users/42    → 200 OK или 204 No Content
PATCH  /api/users/42    → 200 OK
DELETE /api/users/42    → 204 No Content
```

[к оглавлению](#REST-API)

## Каковы лучшие практики проектирования URI?
Правила и рекомендации для проектирования URI в REST API:

**1. Используйте существительные, а не глаголы:**
```
✅ GET  /api/users
❌ GET  /api/getUsers
❌ POST /api/createUser
```

**2. Используйте множественное число для коллекций:**
```
✅ /api/users
✅ /api/users/42
❌ /api/user
❌ /api/user/42
```

**3. Используйте вложенность для связанных ресурсов:**
```
GET /api/users/42/orders       — заказы пользователя 42
GET /api/users/42/orders/7     — заказ 7 пользователя 42
```
Но избегайте глубокой вложенности (не более 2-3 уровней):
```
❌ /api/users/42/orders/7/items/3/reviews
✅ /api/order-items/3/reviews
```

**4. Используйте дефисы для разделения слов:**
```
✅ /api/order-items
❌ /api/orderItems
❌ /api/order_items
```

**5. Используйте строчные буквы:**
```
✅ /api/users
❌ /api/Users
```

**6. Не используйте расширения файлов:**
```
✅ /api/users
❌ /api/users.json
```
Формат определяется через заголовок `Accept`.

**7. Используйте query-параметры для фильтрации, сортировки и пагинации:**
```
GET /api/users?status=active&sort=name&page=2&size=20
```

**8. Для действий, не укладывающихся в CRUD, используйте подресурсы-глаголы:**
```
POST /api/users/42/activate
POST /api/orders/7/cancel
POST /api/emails/42/resend
```

**9. Используйте префикс версии:**
```
/api/v1/users
/api/v2/users
```

**10. Всегда используйте префикс `/api`:**
```
✅ /api/users
❌ /users
```

[к оглавлению](#REST-API)

## Как версионировать REST API?
Версионирование позволяет развивать API без нарушения обратной совместимости для существующих клиентов. Основные подходы:

**1. Версия в URL (наиболее распространённый подход):**
```
GET /api/v1/users
GET /api/v2/users
```
Плюсы: простота, наглядность, легко кэшировать.
Минусы: нарушает принцип REST (URI должен идентифицировать ресурс, а не его версию).

Реализация в Spring:
```java
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
```

**2. Версия в заголовке (кастомный заголовок):**
```
GET /api/users
X-API-Version: 1
```
Плюсы: URI остаётся чистым.
Минусы: менее очевидно, сложнее тестировать в браузере.

```java
@GetMapping(value = "/users", headers = "X-API-Version=1")
public List<UserV1Dto> getUsersV1() { ... }

@GetMapping(value = "/users", headers = "X-API-Version=2")
public List<UserV2Dto> getUsersV2() { ... }
```

**3. Версия через Accept-заголовок (Media Type Versioning / Content Negotiation):**
```
GET /api/users
Accept: application/vnd.myapi.v1+json
```
Плюсы: наиболее «RESTful» подход.
Минусы: сложнее в реализации и тестировании.

```java
@GetMapping(value = "/users", produces = "application/vnd.myapi.v1+json")
public List<UserV1Dto> getUsersV1() { ... }

@GetMapping(value = "/users", produces = "application/vnd.myapi.v2+json")
public List<UserV2Dto> getUsersV2() { ... }
```

**4. Версия в query-параметре:**
```
GET /api/users?version=1
```
Плюсы: простота.
Минусы: параметр может конфликтовать с другими параметрами, загрязняет URI.

На практике **версия в URL** — самый распространённый и рекомендуемый подход для большинства проектов благодаря своей простоте и прозрачности.

[к оглавлению](#REST-API)

## Что такое Content Negotiation?
**Content Negotiation (согласование содержимого)** — это механизм HTTP, позволяющий клиенту и серверу договориться о формате представления ресурса.

Клиент указывает желаемый формат через заголовки:
+ **Accept** — желаемый формат ответа (`application/json`, `application/xml`).
+ **Accept-Language** — предпочитаемый язык (`ru`, `en`).
+ **Accept-Encoding** — предпочитаемое сжатие (`gzip`, `deflate`).
+ **Content-Type** — формат отправляемых данных в теле запроса.

Пример запроса:
```
GET /api/users/42 HTTP/1.1
Accept: application/json
Accept-Language: ru
```

Сервер отвечает в запрошенном формате и указывает реальный формат в заголовке `Content-Type`:
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Language: ru
```

Если сервер не может вернуть данные в запрошенном формате, он возвращает **406 Not Acceptable**.

Реализация в Spring:
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

[к оглавлению](#REST-API)

## Что такое HATEOAS?
**HATEOAS (Hypermedia as the Engine of Application State)** — один из принципов REST, согласно которому сервер в ответе предоставляет клиенту ссылки на доступные действия и связанные ресурсы.

Идея: клиент не должен заранее знать структуру API. Он обнаруживает доступные переходы динамически через гиперссылки в ответах сервера (как пользователь в браузере переходит по ссылкам на веб-страницах).

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

Преимущества HATEOAS:
+ Клиент не зависит от жёстко заданных URL.
+ API становится самодокументируемым.
+ Сервер может динамически управлять доступными действиями (например, скрывать ссылку `deactivate` для уже неактивного пользователя).

Реализация в Spring с помощью **Spring HATEOAS**:
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

На практике HATEOAS используется нечасто из-за сложности реализации и того, что большинство клиентов (SPA, мобильные приложения) проще строить с заранее известными URL.

[к оглавлению](#REST-API)

## В чём отличия REST от SOAP?
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

**Когда использовать REST:**
+ Публичные API (простота интеграции).
+ Мобильные приложения (экономия трафика).
+ Микросервисные архитектуры.
+ Когда важна скорость разработки и простота.

**Когда использовать SOAP:**
+ Корпоративные системы с формальными контрактами.
+ Когда нужна строгая типизация.
+ Когда нужны продвинутые стандарты безопасности (WS-Security).
+ Когда нужны распределённые транзакции (WS-AtomicTransaction).
+ Финансовые и банковские системы.

Пример SOAP-запроса:
```xml
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
    <wsse:Security>...</wsse:Security>
  </soap:Header>
  <soap:Body>
    <getUser>
      <userId>42</userId>
    </getUser>
  </soap:Body>
</soap:Envelope>
```

Эквивалент в REST:
```
GET /api/users/42 HTTP/1.1
Accept: application/json
Authorization: Bearer eyJhbGciOiJI...
```

[к оглавлению](#REST-API)

## Что такое Richardson Maturity Model?
**Richardson Maturity Model (Модель зрелости Ричардсона)** — это модель, предложенная Леонардом Ричардсоном, описывающая четыре уровня зрелости REST API.

### Уровень 0 — «Болото POX» (The Swamp of POX)
Один URI, один HTTP-метод (обычно POST). Фактически это RPC через HTTP.
```
POST /api HTTP/1.1

{"action": "getUser", "userId": 42}
{"action": "createUser", "name": "Иван"}
{"action": "deleteUser", "userId": 42}
```

### Уровень 1 — Ресурсы (Resources)
Каждый ресурс имеет собственный URI, но используется один метод (обычно POST).
```
POST /api/users/42
{"action": "get"}

POST /api/users
{"action": "create", "name": "Иван"}
```

### Уровень 2 — HTTP-глаголы (HTTP Verbs)
Используются правильные HTTP-методы и коды ответов. **Это уровень, на котором находится большинство современных API.**
```
GET    /api/users/42    → 200 OK
POST   /api/users       → 201 Created
PUT    /api/users/42    → 200 OK
DELETE /api/users/42    → 204 No Content
```

### Уровень 3 — HATEOAS (Hypermedia Controls)
Ответы содержат гиперссылки на доступные действия и связанные ресурсы.
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

```
Уровень 0:  POST /api            (RPC через HTTP)
Уровень 1:  POST /api/users/42   (Ресурсы)
Уровень 2:  GET  /api/users/42   (HTTP-глаголы + коды ответов)
Уровень 3:  GET  /api/users/42   (+ HATEOAS ссылки в ответе)
```

[к оглавлению](#REST-API)

## Как реализовать пагинацию, фильтрацию и сортировку в REST API?
### Пагинация
Пагинация необходима для работы с большими коллекциями. Основные подходы:

**Offset-based (наиболее распространённый):**
```
GET /api/users?page=0&size=20
GET /api/users?offset=40&limit=20
```

**Cursor-based (для больших объёмов данных):**
```
GET /api/users?cursor=eyJpZCI6NDJ9&size=20
```

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

### Реализация в Spring (Spring Data):
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

Для сложной фильтрации можно использовать **Spring Data Specifications**:
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

[к оглавлению](#REST-API)

## Как обрабатывать ошибки в REST API?
Правильная обработка ошибок важна для удобства использования API. Рекомендации:

**1. Используйте правильные HTTP-коды ответов** (4xx для ошибок клиента, 5xx для ошибок сервера).

**2. Возвращайте структурированное тело ответа с описанием ошибки.**

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
    },
    {
      "field": "name",
      "message": "Не может быть пустым",
      "rejectedValue": null
    }
  ]
}
```

**Реализация в Spring с использованием `@ControllerAdvice`:**
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

[к оглавлению](#REST-API)

## Какие способы аутентификации используются в REST API?
Основные способы аутентификации:

### 1. Basic Authentication
Логин и пароль передаются в заголовке `Authorization` в кодировке Base64.
```
GET /api/users HTTP/1.1
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```
+ Простота реализации.
+ **Обязательно** использовать только по HTTPS.
+ Учётные данные передаются при каждом запросе.
+ Не рекомендуется для публичных API.

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

**JWT (JSON Web Token)** состоит из трёх частей: Header.Payload.Signature.
```
Header:    {"alg": "HS256", "typ": "JWT"}
Payload:   {"sub": "42", "name": "Иван", "role": "ADMIN", "exp": 1700000000}
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

Преимущества JWT:
+ Stateless — сервер не хранит сессии.
+ Содержит информацию о пользователе (claims).
+ Легко масштабируется.

### 3. API Key
Ключ передаётся в заголовке или query-параметре.
```
GET /api/users HTTP/1.1
X-API-Key: abcdef123456
```
Используется для межсервисного взаимодействия и публичных API с ограниченным доступом.

### 4. OAuth 2.0
Протокол авторизации, позволяющий приложению получить ограниченный доступ к ресурсам пользователя без передачи его учётных данных. Подробнее см. следующий вопрос.

Пример конфигурации Spring Security для JWT:
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

[к оглавлению](#REST-API)

## Что такое OAuth 2.0 и как он работает?
**OAuth 2.0** — это протокол (фреймворк) авторизации, который позволяет стороннему приложению получить ограниченный доступ к HTTP-сервису от имени владельца ресурса.

Основные роли:
+ **Resource Owner** — пользователь, владелец данных.
+ **Client** — приложение, запрашивающее доступ.
+ **Authorization Server** — сервер, выдающий токены (Keycloak, Auth0 и др.).
+ **Resource Server** — сервер, хранящий защищённые ресурсы (наш API).

### Основные потоки (Grant Types):

**1. Authorization Code** — наиболее безопасный, для серверных приложений:
```
1. Клиент → Redirect на Authorization Server
2. Пользователь аутентифицируется и даёт согласие
3. Authorization Server → Redirect на клиент с authorization code
4. Клиент → Authorization Server: обмен code на access_token
5. Клиент → Resource Server: запрос с access_token
```

**2. Authorization Code + PKCE** — для SPA и мобильных приложений (рекомендуемый):
```
Дополняет Authorization Code проверкой code_verifier / code_challenge
для защиты от перехвата authorization code.
```

**3. Client Credentials** — для межсервисного взаимодействия (machine-to-machine):
```
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=myapp&client_secret=secret&scope=read
```

**4. Resource Owner Password Credentials** (устаревший, не рекомендуется):
```
POST /oauth/token
grant_type=password&username=user&password=pass&client_id=myapp
```

Настройка Spring Boot как Resource Server:
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

[к оглавлению](#REST-API)

## Как работает кэширование в REST?
Кэширование — одно из ограничений REST, позволяющее снизить нагрузку на сервер и ускорить ответы клиенту. Основные механизмы HTTP-кэширования:

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
GET /api/users/42 HTTP/1.1

Ответ:
HTTP/1.1 200 OK
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
Content-Type: application/json
{"id": 42, "name": "Иван"}

Повторный запрос:
GET /api/users/42 HTTP/1.1
If-None-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"

Ответ (если ресурс не изменился):
HTTP/1.1 304 Not Modified
```

### 3. Last-Modified / If-Modified-Since
Аналогичен ETag, но использует дату последнего изменения:
```
Ответ:
HTTP/1.1 200 OK
Last-Modified: Fri, 10 Jan 2025 12:00:00 GMT

Повторный запрос:
GET /api/users/42 HTTP/1.1
If-Modified-Since: Fri, 10 Jan 2025 12:00:00 GMT

Ответ (если не изменился):
HTTP/1.1 304 Not Modified
```

### Реализация в Spring:
**Shallow ETag (Spring автоматически считает ETag от тела ответа):**
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

**Ручное управление кэшированием:**
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

[к оглавлению](#REST-API)

## Что такое CORS и как его настроить в Spring?
**CORS (Cross-Origin Resource Sharing)** — это механизм безопасности браузера, который контролирует, какие домены могут обращаться к ресурсам вашего API.

По умолчанию браузеры запрещают JavaScript-коду делать запросы к другому домену (Same-Origin Policy). CORS позволяет серверу указать, с каких доменов разрешены запросы.

### Как работает CORS:

**Простые запросы (Simple Requests):** GET, HEAD, POST с простыми заголовками — браузер отправляет запрос напрямую с заголовком `Origin`:
```
GET /api/users HTTP/1.1
Origin: https://frontend.example.com

Ответ:
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://frontend.example.com
```

**Предварительные запросы (Preflight):** для «сложных» запросов (PUT, DELETE, кастомные заголовки) браузер сначала отправляет OPTIONS:
```
OPTIONS /api/users/42 HTTP/1.1
Origin: https://frontend.example.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: Content-Type, Authorization

Ответ:
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 3600
```

### Настройка в Spring:

**1. Аннотация на контроллере:**
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

**2. Глобальная конфигурация через WebMvcConfigurer:**
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

**3. Через Spring Security (если используется):**
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

> **Важно:** если используется Spring Security, настройку CORS нужно делать через `SecurityFilterChain`, так как Spring Security обрабатывает запросы раньше `WebMvcConfigurer`.

[к оглавлению](#REST-API)

## Как документировать REST API с помощью Swagger/OpenAPI?
**OpenAPI Specification (ранее Swagger Specification)** — это стандарт описания REST API в формате JSON или YAML. **Swagger** — это набор инструментов для работы с OpenAPI (Swagger UI, Swagger Editor, Swagger Codegen).

### Основные инструменты:
+ **Swagger UI** — интерактивная веб-документация с возможностью отправки запросов.
+ **OpenAPI Generator / Swagger Codegen** — генерация клиентов и серверов из спецификации.
+ **Springdoc-openapi** — библиотека для автоматической генерации OpenAPI-документации в Spring Boot.

### Настройка в Spring Boot (springdoc-openapi):

Зависимость:
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.5.0</version>
</dependency>
```

После подключения доступны:
+ `http://localhost:8080/swagger-ui.html` — Swagger UI.
+ `http://localhost:8080/v3/api-docs` — OpenAPI-спецификация в JSON.

### Аннотации для документирования:
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

### Документирование модели:
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

### Глобальная конфигурация:
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

[к оглавлению](#REST-API)

## Как реализовать REST API в Spring?
Spring предоставляет мощные средства для создания REST API через **Spring Web MVC** и **Spring WebFlux** (реактивный подход).

### Ключевые аннотации:

**`@RestController`** — комбинация `@Controller` + `@ResponseBody`. Все методы автоматически сериализуют возвращаемые объекты в JSON/XML.

**`@RequestMapping`** — базовый маппинг URI на уровне класса или метода.

**Специализированные аннотации:** `@GetMapping`, `@PostMapping`, `@PutMapping`, `@PatchMapping`, `@DeleteMapping`.

### Полный пример CRUD-контроллера:
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

### Основные аннотации для параметров:
```java
@PathVariable    — извлечение из URI: /users/{id}
@RequestParam    — из query-строки: /users?name=Иван
@RequestBody     — десериализация тела запроса
@RequestHeader   — извлечение HTTP-заголовка
@CookieValue     — извлечение значения из cookie
```

### DTO и валидация:
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

### Сервисный слой:
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

[к оглавлению](#REST-API)

## Что такое ResponseEntity и зачем он нужен?
**`ResponseEntity<T>`** — это класс Spring, представляющий полный HTTP-ответ: тело, заголовки и код статуса. Он позволяет гибко управлять ответом контроллера.

### Зачем нужен ResponseEntity:
+ Управление HTTP-кодом ответа.
+ Добавление пользовательских заголовков.
+ Контроль тела ответа.
+ Возврат ответа без тела (204 No Content).
+ Указание URI созданного ресурса (201 Created + Location).

### Примеры использования:
```java
// 200 OK с телом
return ResponseEntity.ok(user);

// 200 OK с заголовками
return ResponseEntity.ok()
    .header("X-Custom-Header", "value")
    .body(user);

// 201 Created с Location
URI location = URI.create("/api/users/" + user.getId());
return ResponseEntity.created(location).body(user);

// 204 No Content
return ResponseEntity.noContent().build();

// 404 Not Found
return ResponseEntity.notFound().build();

// Произвольный статус
return ResponseEntity.status(HttpStatus.I_AM_A_TEAPOT).body("Я чайник");

// С кэшированием
return ResponseEntity.ok()
    .cacheControl(CacheControl.maxAge(Duration.ofHours(1)))
    .eTag("v1")
    .body(user);

// Условный ответ
return ResponseEntity.status(HttpStatus.ACCEPTED)
    .header("X-Request-Id", requestId)
    .header("Retry-After", "30")
    .body(new AsyncResponse("Запрос принят к обработке"));
```

### Сравнение подходов:
```java
// Без ResponseEntity — всегда 200 OK, нет контроля над заголовками
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

`ResponseEntity` рекомендуется использовать, когда необходимо управлять статусом ответа или заголовками. Для простых случаев с кодом 200 достаточно возвращать объект напрямую.

[к оглавлению](#REST-API)

## Что такое Rate Limiting и как его реализовать?
**Rate Limiting (ограничение частоты запросов)** — это механизм защиты API от злоупотреблений и перегрузки путём ограничения количества запросов от клиента за определённый период времени.

### Основные алгоритмы:

**1. Fixed Window (Фиксированное окно):**
Подсчёт запросов в фиксированных временных интервалах (например, 100 запросов в минуту).

**2. Sliding Window (Скользящее окно):**
Более точный подход, учитывающий запросы за последние N секунд.

**3. Token Bucket (Корзина с жетонами):**
Жетоны добавляются с постоянной скоростью. Каждый запрос забирает один жетон. Если жетонов нет — запрос отклоняется.

**4. Leaky Bucket (Дырявое ведро):**
Запросы обрабатываются с постоянной скоростью, избыточные ставятся в очередь.

### HTTP-заголовки для Rate Limiting:
```
HTTP/1.1 200 OK
X-RateLimit-Limit: 100           — максимум запросов
X-RateLimit-Remaining: 45        — осталось запросов
X-RateLimit-Reset: 1700000060    — время сброса (Unix timestamp)

HTTP/1.1 429 Too Many Requests
Retry-After: 30                  — через сколько секунд повторить
```

### Реализация в Spring с помощью Bucket4j:
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

### Реализация через Spring Cloud Gateway (для микросервисов):
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

[к оглавлению](#REST-API)

## Как обеспечить безопасность REST API?
Безопасность REST API — это комплексная задача, включающая несколько уровней защиты:

### 1. Транспортная безопасность
Всегда используйте **HTTPS (TLS)**. Никогда не передавайте чувствительные данные по HTTP.
```yaml
server:
  ssl:
    key-store: classpath:keystore.p12
    key-store-password: ${SSL_PASSWORD}
    key-store-type: PKCS12
```

### 2. Аутентификация и авторизация
Используйте JWT, OAuth 2.0 или API Keys. Реализуйте ролевую модель доступа:
```java
@PreAuthorize("hasRole('ADMIN')")
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) { ... }

@PreAuthorize("hasRole('USER') and #id == authentication.principal.id")
@GetMapping("/users/{id}/profile")
public ResponseEntity<Profile> getProfile(@PathVariable Long id) { ... }
```

### 3. Защита от CSRF
Для stateless REST API (JWT-based) защита от CSRF не требуется, так как токен не хранится в cookie автоматически. Если используете cookie-based аутентификацию — CSRF-защита обязательна.

### 4. Валидация входных данных
Всегда валидируйте все входные данные для защиты от SQL injection, XSS и других атак:
```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody CreateUserRequest request) { ... }
```

### 5. Rate Limiting
Защита от DDoS и brute-force атак (подробнее в предыдущем вопросе).

### 6. Защита заголовков
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

### 7. Минимизация раскрытия информации
Не возвращайте:
+ стек-трейсы в продакшне;
+ детали внутренней реализации;
+ версии серверов и фреймворков;
+ лишние поля (пароли, внутренние ID).

```yaml
# application.yml
server:
  error:
    include-stacktrace: never
    include-message: never
```

### 8. Аудит и логирование
Логируйте все важные действия: аутентификацию, авторизацию, изменения данных, ошибки:
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

### 9. Защита от массового присваивания (Mass Assignment)
Используйте DTO вместо сущностей в контроллерах, чтобы клиент не мог установить произвольные поля (например, `role` или `isAdmin`):
```java
// ❌ Опасно — клиент может передать {"role": "ADMIN"}
@PostMapping
public User create(@RequestBody User user) { ... }

// ✅ Безопасно — DTO содержит только допустимые поля
@PostMapping
public UserDto create(@RequestBody CreateUserRequest request) { ... }
```

### 10. CORS
Настройте разрешённые источники (подробнее в вопросе о CORS).

### Чек-лист безопасности REST API:
+ ✅ HTTPS для всех endpoint-ов
+ ✅ Аутентификация (JWT / OAuth 2.0)
+ ✅ Авторизация (ролевая модель)
+ ✅ Валидация всех входных данных
+ ✅ Rate Limiting
+ ✅ CORS только для доверенных доменов
+ ✅ DTO вместо сущностей в API
+ ✅ Нет утечки внутренней информации
+ ✅ Аудит и логирование
+ ✅ Защита заголовков

[к оглавлению](#REST-API)

## Что такое идемпотентный ключ (Idempotency Key)?
**Идемпотентный ключ (Idempotency Key)** — это уникальный идентификатор, передаваемый клиентом в запросе, позволяющий серверу распознавать повторные запросы и не выполнять операцию дважды.

### Зачем нужен:
Метод `POST` не является идемпотентным. При сетевых ошибках (таймаут, разрыв соединения) клиент не знает, был ли запрос обработан сервером. Повторная отправка может привести к дублированию (например, двойное списание денег или двойное создание заказа).

### Как работает:
```
POST /api/payments HTTP/1.1
Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
Content-Type: application/json

{"amount": 1000, "currency": "RUB", "recipientId": 42}
```

1. Сервер проверяет, обрабатывался ли запрос с таким ключом.
2. Если нет — обрабатывает и сохраняет результат.
3. Если да — возвращает сохранённый результат без повторного выполнения.

### Реализация в Spring:
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

Ключи обычно имеют срок жизни (24 часа) и хранятся в БД или Redis.

Данный паттерн широко используется в платёжных системах (Stripe, Tinkoff, YooMoney).

[к оглавлению](#REST-API)

## Как проектировать REST API для связанных ресурсов?
При проектировании REST API часто возникает вопрос: как работать с ресурсами, которые связаны между собой (один-ко-многим, многие-ко-многим)?

### 1. Вложенные ресурсы (Sub-resources)
Для отношения «один-ко-многим» используйте вложенные URI:
```
GET    /api/users/42/orders         — все заказы пользователя 42
POST   /api/users/42/orders         — создать заказ для пользователя 42
GET    /api/users/42/orders/7       — заказ 7 пользователя 42
DELETE /api/users/42/orders/7       — удалить заказ 7
```

### 2. Ссылки через идентификаторы
Включайте идентификаторы связанных ресурсов:
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
  "user": {
    "id": 42,
    "name": "Иван"
  },
  "items": [
    {"id": 1, "product": "Ноутбук", "quantity": 1}
  ]
}
```

### 4. Отношение многие-ко-многим
Используйте связующий ресурс:
```
GET    /api/users/42/roles          — роли пользователя
PUT    /api/users/42/roles/3        — назначить роль 3 пользователю
DELETE /api/users/42/roles/3        — убрать роль 3 у пользователя
```

### 5. Правила выбора глубины вложенности
+ Один уровень вложенности — хорошо: `/users/42/orders`
+ Два уровня — допустимо: `/users/42/orders/7`
+ Три и более — избегайте. Вместо `/users/42/orders/7/items/3` используйте `/order-items/3`

### Реализация в Spring:
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

При проектировании стоит руководствоваться принципом: если ресурс имеет смысл только в контексте родительского — используйте вложенные URI. Если ресурс самостоятелен — выделяйте его на верхний уровень.

[к оглавлению](#REST-API)

## Что такое подход API Design First?
**API Design First (API-first)** — это подход к разработке, при котором спецификация API (контракт) создаётся **до** написания кода. Спецификация описывается в формате OpenAPI (YAML/JSON), после чего из неё автоматически генерируются серверные интерфейсы, клиентские SDK, документация и тесты.

### API-first vs Code-first

| Характеристика | API-first (Design First) | Code-first |
|---------------|--------------------------|------------|
| Последовательность | Спецификация → Код | Код → Спецификация (генерируется) |
| Источник истины | OpenAPI-спецификация | Исходный код (аннотации) |
| Параллельная разработка | Да (frontend и backend одновременно) | Нет (frontend ждёт backend) |
| Согласованность | Контракт фиксирован заранее | Контракт может меняться неконтролируемо |
| Инструменты | OpenAPI Generator, Swagger Codegen | springdoc-openapi, Swagger annotations |

### Преимущества API-first:
+ **Параллельная разработка** — frontend и backend могут работать одновременно, используя контракт как общую точку отсчёта.
+ **Контракт как документация** — спецификация всегда актуальна, так как код генерируется из неё.
+ **Автоматизированное тестирование** — тесты генерируются на основе спецификации и проверяют соответствие реализации контракту.
+ **Генерация клиентских SDK** — для любого языка (Java, TypeScript, Python, Kotlin и др.).
+ **Раннее обнаружение ошибок** — несовместимые изменения обнаруживаются на этапе проектирования, а не в рантайме.

### Пример OpenAPI-спецификации:
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
          required: false
          schema:
            type: integer
            default: 0
        - name: size
          in: query
          required: false
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
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ProblemDetail'

  /users/{id}:
    get:
      operationId: getUserById
      summary: Получить пользователя по ID
      tags:
        - Users
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
            format: int64
      responses:
        '200':
          description: Пользователь найден
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserDto'
        '404':
          description: Пользователь не найден

components:
  schemas:
    CreateUserRequest:
      type: object
      required:
        - name
        - email
      properties:
        name:
          type: string
          minLength: 1
          maxLength: 100
          example: "Иван Петров"
        email:
          type: string
          format: email
          example: "ivan@example.com"
        age:
          type: integer
          minimum: 18
          example: 30

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
        page:
          type: integer
        size:
          type: integer

    ProblemDetail:
      type: object
      properties:
        type:
          type: string
        title:
          type: string
        status:
          type: integer
        detail:
          type: string

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - BearerAuth: []
```

### Настройка генерации кода в Spring Boot (Maven):
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

### Сгенерированный интерфейс (результат генерации):
```java
// Автоматически сгенерированный интерфейс из OpenAPI-спецификации
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

    @Operation(summary = "Получить пользователя по ID")
    @GetMapping(value = "/users/{id}", produces = "application/json")
    ResponseEntity<UserDto> getUserById(@PathVariable Long id);
}
```

### Реализация сгенерированного интерфейса:
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

    @Override
    public ResponseEntity<UserDto> getUserById(Long id) {
        UserDto user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
}
```

### Генерация клиентского SDK:
```bash
# Генерация TypeScript-клиента для frontend
openapi-generator-cli generate \
  -i src/main/resources/openapi/api.yaml \
  -g typescript-axios \
  -o generated/ts-client

# Генерация Java-клиента для другого сервиса
openapi-generator-cli generate \
  -i src/main/resources/openapi/api.yaml \
  -g java \
  --library webclient \
  -o generated/java-client
```

### Валидация реализации против спецификации (тесты):
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class ApiContractTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void shouldReturnUserById() {
        ResponseEntity<UserDto> response = restTemplate.getForEntity(
            "/users/1", UserDto.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).isNotNull();
        assertThat(response.getBody().getName()).isNotBlank();
    }

    @Test
    void shouldReturn400ForInvalidRequest() {
        CreateUserRequest invalid = new CreateUserRequest("", "not-email", -1);

        ResponseEntity<ProblemDetail> response = restTemplate.postForEntity(
            "/users", invalid, ProblemDetail.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.BAD_REQUEST);
    }
}
```

### Важное
+ API-first означает, что спецификация — единственный источник истины (single source of truth). Код **должен** соответствовать ей, а не наоборот.
+ `interfaceOnly=true` в конфигурации генератора создаёт только интерфейсы — реализацию пишете вы сами.
+ Спецификацию удобно хранить в Git рядом с кодом (`src/main/resources/openapi/api.yaml`).
+ OpenAPI Generator поддерживает более 50 целевых языков и фреймворков.

### Частые ошибки
+ Генерация полного сервера (с реализацией) вместо `interfaceOnly` — приводит к нечитаемому коду и сложности поддержки.
+ Редактирование сгенерированного кода вручную — изменения будут перезаписаны при следующей генерации.
+ Расхождение спецификации и реализации — нужно настроить CI/CD проверку (contract testing).
+ Слишком детализированная спецификация на раннем этапе — начинайте с основных endpoint-ов, детализируйте итеративно.
+ Отсутствие версионирования спецификации — всегда указывайте `version` и ведите историю изменений.

### Как используется в 2026
+ API-first стал стандартом де-факто в крупных компаниях и микросервисных архитектурах.
+ **OpenAPI Generator 7.x** поддерживает Spring Boot 3.x, Jakarta EE, virtual threads.
+ **Spectral** и **Redocly CLI** используются для линтинга OpenAPI-спецификаций в CI/CD.
+ **Backstage** (платформа для developer portal) интегрируется с OpenAPI для автоматического каталога сервисов.
+ Тренд на **AI-assisted API design** — LLM-модели помогают генерировать спецификации из описания на естественном языке.
+ **AsyncAPI** — аналог OpenAPI для асинхронных API (Kafka, RabbitMQ, WebSocket).

[к оглавлению](#REST-API)

## Чем отличаются REST, GraphQL и gRPC? Когда что использовать?
При проектировании API важно выбрать правильный протокол взаимодействия. Три наиболее популярных подхода — **REST**, **GraphQL** и **gRPC** — решают разные задачи и имеют свои области применения.

### Сравнительная таблица

| Характеристика | REST | GraphQL | gRPC |
|---------------|------|---------|------|
| Протокол | HTTP/1.1, HTTP/2 | HTTP/1.1, HTTP/2 | HTTP/2 |
| Формат данных | JSON, XML | JSON | Protocol Buffers (бинарный) |
| Схема/контракт | OpenAPI (опционально) | GraphQL Schema (обязательно) | Protobuf (.proto, обязательно) |
| Стиль | Ресурсо-ориентированный | Запросо-ориентированный | RPC (вызов процедур) |
| Количество endpoint-ов | Множество (по ресурсу) | Один (`/graphql`) | Множество (по сервису/методу) |
| Over-fetching | Да (фиксированная структура) | Нет (клиент выбирает поля) | Нет (строгий контракт) |
| Under-fetching | Да (нужны доп. запросы) | Нет (один запрос) | Да (нужны доп. вызовы) |
| Streaming | Нет (SSE, WebSocket отдельно) | Subscriptions (WebSocket) | Да (нативная поддержка) |
| Кэширование | Встроенное (HTTP-кэш) | Сложное (один endpoint) | Нет нативного |
| Производительность | Средняя | Средняя | Высокая |
| Типизация | Слабая (зависит от реализации) | Строгая (GraphQL Schema) | Строгая (Protobuf) |
| Tooling | Swagger UI, Postman | GraphiQL, Apollo Studio | gRPC UI, Evans |

### REST — ресурсо-ориентированный подход
Лучше всего подходит для простых CRUD-операций с чётко определёнными ресурсами.

```java
// Spring REST Controller
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.findById(id));
    }

    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request) {
        UserDto user = userService.create(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }
}
```

```bash
# Пример вызова
curl -X GET https://api.example.com/api/users/42 \
  -H "Accept: application/json"
```

### GraphQL — язык запросов для API
Клиент сам определяет, какие данные ему нужны. Решает проблемы over-fetching и under-fetching.

```graphql
# Схема GraphQL
type Query {
    user(id: ID!): User
    users(status: String, page: Int, size: Int): UserPage!
}

type Mutation {
    createUser(input: CreateUserInput!): User!
    updateUser(id: ID!, input: UpdateUserInput!): User!
}

type User {
    id: ID!
    name: String!
    email: String!
    age: Int
    orders: [Order!]!
}

type Order {
    id: ID!
    status: String!
    total: Float!
}

input CreateUserInput {
    name: String!
    email: String!
    age: Int
}
```

```java
// Spring GraphQL Controller
@Controller
public class UserGraphQLController {

    @QueryMapping
    public User user(@Argument Long id) {
        return userService.findById(id);
    }

    @QueryMapping
    public UserPage users(@Argument String status,
                          @Argument Integer page,
                          @Argument Integer size) {
        return userService.findAll(status, page, size);
    }

    @MutationMapping
    public User createUser(@Argument CreateUserInput input) {
        return userService.create(input);
    }

    // Resolver для вложенного поля — загружается только если клиент запросил
    @SchemaMapping(typeName = "User", field = "orders")
    public List<Order> orders(User user) {
        return orderService.findByUserId(user.getId());
    }
}
```

```bash
# Клиент запрашивает только нужные поля
curl -X POST https://api.example.com/graphql \
  -H "Content-Type: application/json" \
  -d '{
    "query": "query { user(id: 42) { name email orders { id status } } }"
  }'
```

Ответ содержит **только запрошенные поля**:
```json
{
  "data": {
    "user": {
      "name": "Иван Петров",
      "email": "ivan@example.com",
      "orders": [
        {"id": "1", "status": "DELIVERED"},
        {"id": "2", "status": "PENDING"}
      ]
    }
  }
}
```

### gRPC — высокопроизводительный RPC
Использует Protocol Buffers для сериализации и HTTP/2 для транспорта. Идеален для межсервисного взаимодействия.

```protobuf
// user_service.proto
syntax = "proto3";

package com.example.user;

option java_multiple_files = true;
option java_package = "com.example.grpc.user";

service UserService {
    rpc GetUser (GetUserRequest) returns (UserResponse);
    rpc CreateUser (CreateUserRequest) returns (UserResponse);
    rpc ListUsers (ListUsersRequest) returns (ListUsersResponse);
    // Server streaming — сервер отправляет поток данных
    rpc WatchUserUpdates (WatchRequest) returns (stream UserEvent);
}

message GetUserRequest {
    int64 id = 1;
}

message CreateUserRequest {
    string name = 1;
    string email = 2;
    int32 age = 3;
}

message UserResponse {
    int64 id = 1;
    string name = 2;
    string email = 3;
    int32 age = 4;
}

message ListUsersRequest {
    string status = 1;
    int32 page = 2;
    int32 size = 3;
}

message ListUsersResponse {
    repeated UserResponse users = 1;
    int64 total_elements = 2;
}

message WatchRequest {
    int64 user_id = 1;
}

message UserEvent {
    string event_type = 1;
    UserResponse user = 2;
}
```

```java
// Реализация gRPC-сервиса на Spring Boot (grpc-spring-boot-starter)
@GrpcService
public class UserGrpcService extends UserServiceGrpc.UserServiceImplBase {

    private final UserService userService;

    @Override
    public void getUser(GetUserRequest request,
                        StreamObserver<UserResponse> responseObserver) {
        User user = userService.findById(request.getId());
        UserResponse response = UserResponse.newBuilder()
            .setId(user.getId())
            .setName(user.getName())
            .setEmail(user.getEmail())
            .setAge(user.getAge())
            .build();
        responseObserver.onNext(response);
        responseObserver.onCompleted();
    }

    @Override
    public void watchUserUpdates(WatchRequest request,
                                 StreamObserver<UserEvent> responseObserver) {
        // Server streaming — отправляем обновления по мере их появления
        userService.subscribe(request.getUserId(), event -> {
            responseObserver.onNext(UserEvent.newBuilder()
                .setEventType(event.getType())
                .setUser(toProto(event.getUser()))
                .build());
        });
    }
}
```

### Когда что использовать

**REST подходит для:**
+ Публичных API (простота, стандартизированность, HTTP-кэширование).
+ CRUD-операций над ресурсами.
+ Когда нужна максимальная совместимость (любой HTTP-клиент).
+ Браузерных приложений и мобильных клиентов.

**GraphQL подходит для:**
+ Сложных UI с разнородными данными (дашборды, социальные сети).
+ Множества клиентов с разными потребностями (mobile vs web vs admin).
+ Избежания over-fetching / under-fetching.
+ Быстро меняющихся требований к данным.

**gRPC подходит для:**
+ Межсервисного взаимодействия в микросервисах.
+ Высоконагруженных систем (до 10x быстрее REST/JSON).
+ Real-time коммуникации (streaming).
+ Полиглотных архитектур (одна .proto → клиенты на любом языке).

### Комбинированный подход (наиболее распространённый):
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

### Важное
+ REST, GraphQL и gRPC — не конкуренты, а инструменты для разных задач. В реальных системах они часто используются вместе.
+ REST остаётся лучшим выбором для публичных API благодаря простоте и повсеместной поддержке.
+ GraphQL не заменяет REST — он решает конкретную проблему гибких запросов со стороны клиента.
+ gRPC требует HTTP/2 и не работает напрямую из браузера (нужен grpc-web прокси).

### Частые ошибки
+ Выбор GraphQL для простого CRUD — избыточная сложность, когда REST справится лучше.
+ Использование REST для межсервисного взаимодействия с высокой нагрузкой — gRPC значительно эффективнее.
+ N+1 проблема в GraphQL — без DataLoader вложенные поля вызывают лавину запросов к БД.
+ Игнорирование HTTP-кэширования REST при переходе на GraphQL — нужно реализовывать кэширование на уровне приложения.
+ Использование gRPC для публичного API без прокси — браузеры не поддерживают gRPC напрямую.

### Как используется в 2026
+ **GraphQL Federation v2** — стандарт для объединения нескольких GraphQL-сервисов в единый граф (Apollo Federation, Netflix DGS).
+ **gRPC + Kotlin coroutines** — нативная поддержка корутин для асинхронных gRPC-вызовов в Spring Boot.
+ **Connect RPC** — новый протокол, совместимый с gRPC, но работающий через обычный HTTP/JSON (удобнее для отладки).
+ Большинство компаний используют **гибридный подход**: REST/GraphQL для внешних клиентов, gRPC для внутренней коммуникации.
+ **Spring Boot 3.x** имеет встроенную поддержку GraphQL (`spring-boot-starter-graphql`) и gRPC через `grpc-spring-boot-starter`.

[к оглавлению](#REST-API)

## Как обеспечить обратную совместимость REST API?
**Обратная совместимость (backward compatibility)** — это свойство API, при котором обновлённая версия продолжает корректно работать с существующими клиентами без необходимости их изменения.

### Неломающие изменения (non-breaking changes)
Эти изменения можно вносить безопасно, не нарушая работу клиентов:

+ **Добавление нового поля в ответ** — клиенты, не знающие о поле, просто его игнорируют (tolerant reader).
+ **Добавление нового endpoint-а** — не затрагивает существующие.
+ **Добавление нового опционального параметра запроса** — старые запросы работают без него.
+ **Добавление нового значения enum (в ответе)** — клиент должен быть готов к неизвестным значениям.
+ **Расширение допустимого диапазона значений** — например, увеличение максимальной длины строки.

```java
// Было: UserDto v1
public record UserDto(Long id, String name, String email) {}

// Стало: UserDto v1 — добавили поле (non-breaking)
public record UserDto(Long id, String name, String email, String phone) {}
// Старые клиенты игнорируют поле "phone"
```

### Ломающие изменения (breaking changes)
Эти изменения **нарушают** работу существующих клиентов:

+ **Удаление поля из ответа** — клиент может зависеть от этого поля.
+ **Переименование поля** — `userName` → `name` — клиент не найдёт старое поле.
+ **Изменение типа поля** — `"age": 30` → `"age": "30"`.
+ **Удаление endpoint-а** — клиент получит 404.
+ **Изменение семантики endpoint-а** — `POST /orders` теперь возвращает другую структуру.
+ **Добавление обязательного параметра** — старые запросы без него сломаются.
+ **Изменение кода ответа** — `200 OK` → `201 Created` может сломать клиентскую логику.

### Стратегии обеспечения обратной совместимости

**1. Версионирование API (URL / Header)**
```
GET /api/v1/users      — старая версия
GET /api/v2/users      — новая версия
```

Обе версии работают параллельно, давая клиентам время на миграцию.

**2. Заголовки Deprecation и Sunset (RFC 8594, RFC 8977)**
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

```bash
# Ответ сервера при вызове устаревшего endpoint-а
HTTP/1.1 200 OK
Deprecation: true
Sunset: Sat, 01 Nov 2026 00:00:00 GMT
Link: </api/v2/users>; rel="successor-version"
```

**3. Tolerant Reader Pattern (Закон Постела)**
> «Будь консервативен в том, что отправляешь, и либерален в том, что принимаешь.»

Клиенты должны:
+ Игнорировать неизвестные поля в ответе.
+ Не зависеть от порядка полей в JSON.
+ Обрабатывать отсутствие необязательных полей.

```java
// Настройка Jackson для игнорирования неизвестных полей (клиентская сторона)
@JsonIgnoreProperties(ignoreUnknown = true)
public record UserDto(Long id, String name, String email) {}

// Или глобально
ObjectMapper mapper = new ObjectMapper();
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
```

**4. Паттерн эволюции (API Evolution)**
Вместо версионирования — постепенное развитие API:

```java
// Шаг 1: Добавляем новое поле, старое оставляем
public record UserDto(
    Long id,
    String name,
    String email,
    @Deprecated String userName,    // старое поле (deprecated)
    String displayName              // новое поле
) {
    // Сериализация: оба поля содержат одно значение
    @JsonProperty("userName")
    public String getUserName() {
        return displayName != null ? displayName : name;
    }
}

// Шаг 2: Через несколько релизов удаляем старое поле
// (после того как все клиенты мигрировали)
```

**5. Feature Flags для управления поведением API**
```java
@GetMapping("/api/users/{id}")
public ResponseEntity<Object> getUser(@PathVariable Long id,
                                       @RequestHeader(value = "X-API-Features",
                                                      required = false) String features) {
    User user = userService.findById(id);

    if (features != null && features.contains("extended-profile")) {
        return ResponseEntity.ok(userMapper.toExtendedDto(user));
    }
    return ResponseEntity.ok(userMapper.toDto(user));
}
```

### Реализация поддержки нескольких версий в Spring:
```java
// Подход с отдельными контроллерами
@RestController
@RequestMapping("/api/v1/users")
public class UserControllerV1 {

    @GetMapping("/{id}")
    public ResponseEntity<UserV1Dto> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        // V1: name — одна строка
        return ResponseEntity.ok(new UserV1Dto(user.getId(), user.getFullName(), user.getEmail()));
    }
}

@RestController
@RequestMapping("/api/v2/users")
public class UserControllerV2 {

    @GetMapping("/{id}")
    public ResponseEntity<UserV2Dto> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        // V2: name разделено на firstName и lastName
        return ResponseEntity.ok(new UserV2Dto(
            user.getId(), user.getFirstName(), user.getLastName(), user.getEmail()));
    }
}
```

### Автоматическое обнаружение несовместимых изменений (CI/CD):
```bash
# Использование openapi-diff для проверки совместимости
openapi-diff --fail-on-incompatible \
  api-spec-v1.yaml \
  api-spec-v2.yaml

# Результат:
# - What's New: GET /users/{id}/preferences
# - What's Deleted: (none)
# - What's Changed:
#   - GET /users: added optional query parameter "status"
# Result: compatible ✅
```

### Важное
+ Обратная совместимость — это не только техническая, но и организационная задача. Нужна чёткая политика версионирования.
+ Закон Постела (tolerant reader) — ключевой принцип для построения устойчивых интеграций.
+ Заголовки `Deprecation` и `Sunset` — стандартный способ уведомить клиентов о планируемых изменениях.
+ Поддержка старых версий API — это затраты. Планируйте lifecycle заранее (обычно N и N-1 версии).

### Частые ошибки
+ Удаление полей без предварительного deprecated-периода — ломает клиентов внезапно.
+ Изменение типа поля (число → строка) без версионирования — клиенты падают при десериализации.
+ Добавление обязательного параметра в существующий endpoint — все старые запросы перестанут работать.
+ Отсутствие автоматической проверки совместимости в CI/CD — несовместимые изменения попадают в продакшн.
+ Бесконечная поддержка всех версий — вовремя удаляйте старые версии с достаточным периодом предупреждения.

### Как используется в 2026
+ **openapi-diff** и **Optic** интегрируются в CI/CD для автоматической проверки совместимости при каждом pull request.
+ **API lifecycle management** — платформы (Backstage, Kong) управляют жизненным циклом API (draft → published → deprecated → retired).
+ Стандарт **RFC 8594 (Sunset Header)** и **RFC 8977 (Deprecation Header)** широко поддерживаются клиентскими библиотеками.
+ Тренд на **API Evolution** вместо строгого версионирования — добавление полей, deprecated-аннотации, tolerant reader.
+ **Contract testing** (Pact, Spring Cloud Contract) — стандарт для проверки совместимости между producer и consumer.

[к оглавлению](#REST-API)

## Что такое асинхронные API и как их проектировать?
**Асинхронные API** — это подходы к взаимодействию клиента и сервера, при которых обработка запроса не блокирует клиента и результат доставляется позже (через callback, polling или push-уведомление).

### Когда нужны асинхронные API:
+ **Долгие операции** — генерация отчёта, обработка видео, массовый импорт данных.
+ **Real-time обновления** — чат, биржевые котировки, уведомления.
+ **Событийные уведомления** — платёж завершён, заказ отправлен, статус изменился.
+ **Интеграции** — webhook-и от внешних систем (Stripe, GitHub, Telegram).

### Паттерн 1: Polling (202 Accepted + Location)
Клиент отправляет запрос, получает `202 Accepted` и периодически проверяет статус операции.

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
        // Операция завершена — перенаправляем на результат
        return ResponseEntity.status(HttpStatus.SEE_OTHER)
            .location(URI.create(status.resultUrl()))
            .body(status);
    }
    if ("FAILED".equals(status.status())) {
        return ResponseEntity.ok(status);
    }
    // Ещё обрабатывается
    return ResponseEntity.ok()
        .header("Retry-After", "5")
        .body(status);
}

// DTO
public record TaskStatus(
    String taskId,
    String status,       // PENDING, PROCESSING, COMPLETED, FAILED
    String resultUrl,
    Integer progress,    // 0-100
    String errorMessage
) {
    public TaskStatus(String taskId, String status, String resultUrl) {
        this(taskId, status, resultUrl, null, null);
    }
}
```

```bash
# 1. Запускаем генерацию отчёта
curl -X POST https://api.example.com/api/reports \
  -H "Content-Type: application/json" \
  -d '{"type": "SALES", "dateFrom": "2026-01-01", "dateTo": "2026-03-31"}'

# Ответ:
# HTTP/1.1 202 Accepted
# Location: /api/reports/tasks/abc-123
# Retry-After: 5
# {"taskId": "abc-123", "status": "PENDING"}

# 2. Проверяем статус
curl https://api.example.com/api/reports/tasks/abc-123

# Ответ (в процессе):
# HTTP/1.1 200 OK
# Retry-After: 5
# {"taskId": "abc-123", "status": "PROCESSING", "progress": 65}

# Ответ (завершено):
# HTTP/1.1 303 See Other
# Location: /api/reports/files/report-abc-123.pdf
# {"taskId": "abc-123", "status": "COMPLETED", "resultUrl": "/api/reports/files/report-abc-123.pdf"}
```

### Паттерн 2: Webhooks (Callback URL)
Сервер отправляет HTTP-запрос на указанный URL клиента при наступлении события.

```java
// Регистрация webhook
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

// Отправка webhook при наступлении события
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
                .header("Content-Type", "application/json")
                .header("X-Webhook-Signature", "sha256=" + signature)
                .header("X-Webhook-Event", "order.completed")
                .header("X-Webhook-Id", UUID.randomUUID().toString())
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

```bash
# Клиент регистрирует webhook
curl -X POST https://api.example.com/api/webhooks \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://my-app.com/callbacks/orders",
    "events": ["order.completed", "order.cancelled"],
    "secret": "whsec_abc123"
  }'

# Когда заказ завершён, сервер отправит:
# POST https://my-app.com/callbacks/orders
# X-Webhook-Signature: sha256=5d3a...
# X-Webhook-Event: order.completed
# {"orderId": 42, "status": "COMPLETED", "total": 1500.00}
```

### Паттерн 3: SSE (Server-Sent Events)
Сервер отправляет поток событий клиенту через постоянное HTTP-соединение. Однонаправленный (сервер → клиент).

```java
@RestController
@RequestMapping("/api/events")
public class SseController {

    private final SseEmitterService sseService;

    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter streamEvents(@RequestParam Long userId) {
        SseEmitter emitter = new SseEmitter(0L); // без таймаута
        sseService.register(userId, emitter);

        emitter.onCompletion(() -> sseService.unregister(userId));
        emitter.onTimeout(() -> sseService.unregister(userId));

        return emitter;
    }
}

@Service
public class SseEmitterService {

    private final Map<Long, SseEmitter> emitters = new ConcurrentHashMap<>();

    public void register(Long userId, SseEmitter emitter) {
        emitters.put(userId, emitter);
    }

    public void unregister(Long userId) {
        emitters.remove(userId);
    }

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
                unregister(userId);
            }
        }
    }
}
```

```bash
# Клиент подключается к потоку событий
curl -N https://api.example.com/api/events/stream?userId=42

# Сервер отправляет:
# event: notification
# id: 550e8400-e29b-41d4-a716-446655440000
# retry: 3000
# data: {"type": "ORDER_SHIPPED", "orderId": 7, "message": "Ваш заказ отправлен"}
#
# event: notification
# id: 660e8400-e29b-41d4-a716-446655440001
# data: {"type": "PAYMENT_RECEIVED", "amount": 1500.00}
```

### Паттерн 4: WebSocket (двунаправленный)
Полнодуплексная связь — и клиент, и сервер могут отправлять сообщения.

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

    // Клиент отправляет сообщение на /app/chat.send
    @MessageMapping("/chat.send")
    public void sendMessage(@Payload ChatMessage message,
                            SimpMessageHeaderAccessor headerAccessor) {
        String userId = headerAccessor.getUser().getName();
        message.setSenderId(userId);
        message.setTimestamp(Instant.now());

        // Отправляем всем подписчикам комнаты
        messagingTemplate.convertAndSend(
            "/topic/chat." + message.getRoomId(), message);
    }

    // Отправка персонального уведомления
    public void sendNotification(String userId, Object payload) {
        messagingTemplate.convertAndSendToUser(
            userId, "/queue/notifications", payload);
    }
}
```

### Сравнение подходов

| Характеристика | Polling | Webhooks | SSE | WebSocket |
|---------------|---------|----------|-----|-----------|
| Направление | Клиент → Сервер | Сервер → Клиент | Сервер → Клиент | Двунаправленный |
| Протокол | HTTP | HTTP | HTTP | WS (поверх HTTP) |
| Задержка | Высокая (интервал) | Низкая | Низкая | Очень низкая |
| Нагрузка на сервер | Высокая (частые запросы) | Низкая | Средняя (открытые соединения) | Средняя |
| Надёжность | Высокая | Средняя (нужны retry) | Средняя (auto-reconnect) | Средняя |
| Масштабирование | Простое | Простое | Сложнее | Сложнее |
| Подходит для | Долгие операции | Интеграции, события | Уведомления, ленты | Чат, real-time игры |

### Важное
+ **Polling (202 Accepted)** — самый простой и надёжный подход для долгих операций. Начинайте с него.
+ **Webhooks** — стандарт для интеграций. Обязательно подписывайте payload (HMAC) и реализуйте retry с exponential backoff.
+ **SSE** — простая альтернатива WebSocket для однонаправленных обновлений. Работает через обычный HTTP, поддерживает auto-reconnect.
+ **WebSocket** — используйте только когда нужна полнодуплексная связь (чат, real-time совместное редактирование).

### Частые ошибки
+ Использование WebSocket там, где достаточно SSE — избыточная сложность.
+ Отсутствие retry-механизма для webhook — первая доставка может не пройти.
+ Polling без `Retry-After` заголовка — клиент может перегружать сервер слишком частыми запросами.
+ Отсутствие аутентификации в WebSocket-соединении — необходимо проверять токен при handshake.
+ Хранение состояния SSE/WebSocket в памяти без учёта масштабирования — при нескольких инстансах нужен Redis или брокер сообщений.

### Как используется в 2026
+ **Spring WebFlux + SSE** — реактивный подход для потоковых данных (`Flux<ServerSentEvent>`).
+ **HTTP/2 Server Push** устарел и удалён из Chrome; SSE остаётся основным push-механизмом.
+ **AsyncAPI 3.0** — стандарт описания асинхронных API (WebSocket, Kafka, MQTT), аналог OpenAPI для event-driven систем.
+ **Webhooks стандартизируются** — проект Standard Webhooks (standardwebhooks.com) определяет единый формат подписи и retry.
+ **Virtual threads (Project Loom)** в Java 21+ упрощают обработку большого числа одновременных SSE/WebSocket-соединений.

[к оглавлению](#REST-API)

## Что такое BFF (Backend for Frontend) и API Gateway?
**BFF (Backend for Frontend)** и **API Gateway** — это архитектурные паттерны, определяющие, как клиенты взаимодействуют с backend-сервисами. Оба решают проблему организации доступа к множеству микросервисов.

### API Gateway — единая точка входа

**API Gateway** — это прокси-сервер, который является единственной точкой входа для всех клиентов. Он маршрутизирует запросы к соответствующим микросервисам и предоставляет сквозную функциональность.

```
    Web App     Mobile App     Partner API
       \            |            /
        \           |           /
         ┌──────────▼──────────┐
         │     API Gateway     │
         │  (routing, auth,    │
         │   rate limiting,    │
         │   logging)          │
         └────┬────┬────┬──────┘
              │    │    │
         ┌────▼┐ ┌─▼──┐ ┌▼────┐
         │User ││Order││Stock│
         │Svc  ││ Svc ││ Svc │
         └─────┘ └────┘ └─────┘
```

Функции API Gateway:
+ **Маршрутизация** — перенаправление запросов к нужному сервису.
+ **Аутентификация и авторизация** — единая проверка токенов.
+ **Rate Limiting** — ограничение частоты запросов.
+ **Логирование и мониторинг** — централизованный сбор метрик.
+ **Трансформация запросов/ответов** — изменение формата данных.
+ **Кэширование** — кэширование ответов на уровне шлюза.
+ **Circuit Breaker** — защита от каскадных сбоев.

### Реализация API Gateway на Spring Cloud Gateway:
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
                    .addRequestHeader("X-Gateway", "true")
                    .circuitBreaker(cb -> cb
                        .setName("userServiceCB")
                        .setFallbackUri("forward:/fallback/users"))
                    .retry(retry -> retry.setRetries(3))
                )
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

```yaml
# application.yml для Spring Cloud Gateway
spring:
  cloud:
    gateway:
      globalcors:
        corsConfigurations:
          '[/**]':
            allowedOrigins: "https://frontend.example.com"
            allowedMethods: "*"
            allowedHeaders: "*"
      default-filters:
        - DedupeResponseHeader=Access-Control-Allow-Origin
        - TokenRelay
```

### BFF (Backend for Frontend) — отдельный API для каждого типа клиента

**BFF** — это паттерн, при котором для каждого типа клиента (web, mobile, admin, partner) создаётся отдельный backend-сервис, оптимизированный под потребности этого клиента.

```
    Web App         Mobile App        Admin Panel
       │                │                  │
  ┌────▼─────┐    ┌─────▼──────┐    ┌─────▼──────┐
  │ Web BFF  │    │ Mobile BFF │    │ Admin BFF  │
  │(подробные│    │(компактные │    │(все данные,│
  │ данные,  │    │ данные,    │    │ управление)│
  │ пагинация)│   │ агрегация) │    │            │
  └──┬───┬───┘    └──┬───┬─────┘    └──┬───┬─────┘
     │   │           │   │             │   │
  ┌──▼┐ ┌▼──┐    ┌──▼┐ ┌▼──┐      ┌──▼┐ ┌▼──┐
  │User│ │Ord│    │User│ │Ord│      │User│ │Ord│
  │Svc │ │Svc│    │Svc │ │Svc│      │Svc │ │Svc│
  └────┘ └───┘    └────┘ └───┘      └────┘ └───┘
```

```java
// Mobile BFF — компактные данные, агрегация в один запрос
@RestController
@RequestMapping("/mobile/api")
@RequiredArgsConstructor
public class MobileBffController {

    private final UserServiceClient userClient;
    private final OrderServiceClient orderClient;
    private final NotificationServiceClient notificationClient;

    // Один вызов — все данные для главного экрана
    @GetMapping("/home")
    public ResponseEntity<MobileHomeResponse> getHomeScreen(
            @AuthenticationPrincipal UserPrincipal principal) {

        // Параллельные вызовы к микросервисам
        CompletableFuture<UserSummary> userFuture =
            CompletableFuture.supplyAsync(() -> userClient.getSummary(principal.getId()));
        CompletableFuture<List<OrderBrief>> ordersFuture =
            CompletableFuture.supplyAsync(() -> orderClient.getRecent(principal.getId(), 5));
        CompletableFuture<Integer> notifCountFuture =
            CompletableFuture.supplyAsync(() -> notificationClient.getUnreadCount(principal.getId()));

        CompletableFuture.allOf(userFuture, ordersFuture, notifCountFuture).join();

        MobileHomeResponse response = new MobileHomeResponse(
            userFuture.join(),
            ordersFuture.join(),
            notifCountFuture.join()
        );
        return ResponseEntity.ok(response);
    }
}

// Web BFF — подробные данные с пагинацией
@RestController
@RequestMapping("/web/api")
@RequiredArgsConstructor
public class WebBffController {

    private final UserServiceClient userClient;
    private final OrderServiceClient orderClient;

    @GetMapping("/users/{id}/dashboard")
    public ResponseEntity<WebDashboardResponse> getDashboard(
            @PathVariable Long id,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {

        UserProfile profile = userClient.getFullProfile(id);
        Page<OrderDetail> orders = orderClient.getByUserId(id, page, size);
        OrderStatistics stats = orderClient.getStatistics(id);

        return ResponseEntity.ok(new WebDashboardResponse(profile, orders, stats));
    }
}
```

### Сравнение подходов

| Характеристика | Прямые вызовы | API Gateway | BFF |
|---------------|---------------|-------------|-----|
| Сложность | Минимальная | Средняя | Высокая |
| Оптимизация под клиента | Нет | Частичная | Полная |
| Количество сервисов | N микросервисов | 1 gateway + N | M BFF + N |
| Агрегация данных | На клиенте | Ограниченная | Полная |
| Команда | Одна | Одна (gateway) | По BFF на команду |
| Over-fetching | Да | Да | Нет |
| Безопасность | На каждом сервисе | Централизованная | На каждом BFF |

### Когда что использовать

**API Gateway (без BFF):**
+ Все клиенты имеют одинаковые потребности.
+ Нужна централизованная маршрутизация, auth и rate limiting.
+ Команда небольшая и не хочет поддерживать дополнительные сервисы.

**BFF:**
+ Клиенты имеют **существенно разные** потребности (mobile vs web vs admin).
+ Нужна агрегация данных из нескольких микросервисов в один вызов.
+ Разные команды отвечают за разные клиенты.
+ Нужна оптимизация ответов под конкретный клиент (размер данных, формат).

**API Gateway + BFF (комбинированный):**
+ API Gateway — для сквозной функциональности (auth, rate limiting, logging).
+ BFF — для агрегации и адаптации данных под конкретный клиент.

```
    Web App         Mobile App
       │                │
       └────────┬───────┘
         ┌──────▼──────┐
         │ API Gateway │ ← auth, rate limiting, logging
         └──┬───────┬──┘
       ┌────▼──┐ ┌──▼─────┐
       │Web BFF│ │Mobile  │ ← агрегация, адаптация
       │       │ │BFF     │
       └──┬──┬─┘ └──┬──┬──┘
          │  │      │  │
       ┌──▼┐ ┌▼──┐  │  │
       │Svc││Svc │──┘  │
       │ A ││ B  │─────┘
       └───┘ └───┘
```

### Важное
+ API Gateway — обязательный компонент микросервисной архитектуры. Он решает сквозные задачи (auth, rate limiting, logging) в одном месте.
+ BFF не заменяет API Gateway — они дополняют друг друга.
+ Каждый BFF должен принадлежать той же команде, что и клиент, который он обслуживает.
+ BFF не содержит бизнес-логику — только агрегацию, трансформацию и адаптацию данных.

### Частые ошибки
+ Реализация бизнес-логики в API Gateway — шлюз должен быть «тонким», только маршрутизация и сквозные задачи.
+ Единый BFF для всех клиентов — это просто API Gateway, а не BFF. Смысл BFF — в разных API для разных клиентов.
+ Слишком много BFF — каждый BFF требует поддержки. Создавайте BFF только если потребности клиентов действительно различаются.
+ Отсутствие circuit breaker в BFF — если один микросервис недоступен, не должен падать весь BFF.
+ Синхронные вызовы к микросервисам из BFF — используйте `CompletableFuture`, WebClient или virtual threads для параллельных вызовов.

### Как используется в 2026
+ **Spring Cloud Gateway** — стандарт для API Gateway в экосистеме Spring (поддержка reactive и MVC).
+ **GraphQL как альтернатива BFF** — GraphQL решает ту же проблему гибкости запросов, но без дополнительного сервиса.
+ **Netflix Zuul** устарел, замещён Spring Cloud Gateway и Envoy.
+ **Envoy Proxy + Istio** — service mesh как альтернатива API Gateway для внутренней коммуникации.
+ **BFF + Virtual Threads (Java 21+)** — упрощают параллельные вызовы к микросервисам без реактивного стека.
+ **API Gateway as a Service** — Kong Gateway, AWS API Gateway, Azure APIM позволяют не писать свой шлюз.

[к оглавлению](#REST-API)

## Как проектировать API для микросервисной архитектуры?
Проектирование API для микросервисов требует учёта распределённой природы системы: сетевая ненадёжность, независимое развёртывание сервисов, согласованность данных и отказоустойчивость.

### Синхронное взаимодействие (REST / gRPC)

Один сервис напрямую вызывает другой и ждёт ответа.

```java
// Вызов другого сервиса через RestClient (Spring Boot 3.2+)
@Service
@RequiredArgsConstructor
public class OrderService {

    private final RestClient userServiceClient;

    public OrderDto createOrder(Long userId, CreateOrderRequest request) {
        // Синхронный вызов к User Service
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

// Конфигурация RestClient
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

### Асинхронное взаимодействие (Events / Kafka)

Сервисы общаются через события — отправитель публикует событие, получатели подписаны на него.

```java
// Публикация события при создании заказа
@Service
@RequiredArgsConstructor
public class OrderService {

    private final OrderRepository orderRepository;
    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;

    @Transactional
    public OrderDto createOrder(CreateOrderRequest request) {
        Order order = orderMapper.toEntity(request);
        order.setStatus(OrderStatus.CREATED);
        Order saved = orderRepository.save(order);

        // Публикуем событие
        OrderEvent event = new OrderEvent(
            saved.getId(), "ORDER_CREATED",
            saved.getUserId(), saved.getTotal(), Instant.now()
        );
        kafkaTemplate.send("order-events", saved.getId().toString(), event);

        return orderMapper.toDto(saved);
    }
}

// Обработка события в другом сервисе (Notification Service)
@Service
@Slf4j
public class OrderEventListener {

    @KafkaListener(topics = "order-events", groupId = "notification-service")
    public void handleOrderEvent(OrderEvent event) {
        log.info("Received order event: {}", event);

        if ("ORDER_CREATED".equals(event.eventType())) {
            notificationService.sendOrderConfirmation(event.userId(), event.orderId());
        }
    }
}

// Событие
public record OrderEvent(
    Long orderId,
    String eventType,
    Long userId,
    BigDecimal total,
    Instant timestamp
) {}
```

### API Composition — агрегация данных из нескольких сервисов

```java
// API Composer — агрегирует данные из нескольких сервисов
@RestController
@RequestMapping("/api/customer-view")
@RequiredArgsConstructor
public class CustomerViewController {

    private final RestClient userServiceClient;
    private final RestClient orderServiceClient;
    private final RestClient loyaltyServiceClient;

    @GetMapping("/{userId}")
    public ResponseEntity<CustomerView> getCustomerView(@PathVariable Long userId) {
        // Параллельные вызовы с virtual threads (Java 21+)
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

            CustomerView view = new CustomerView(
                userTask.get(), ordersTask.get(), loyaltyTask.get()
            );
            return ResponseEntity.ok(view);
        }
    }
}
```

### Circuit Breaker — защита от каскадных сбоев

Если вызываемый сервис недоступен, Circuit Breaker «размыкает» цепь и возвращает fallback-ответ.

```java
@Service
@RequiredArgsConstructor
public class UserServiceClient {

    private final RestClient restClient;
    private final CircuitBreakerRegistry circuitBreakerRegistry;

    @CircuitBreaker(name = "userService", fallbackMethod = "getUserFallback")
    @Retry(name = "userService")
    @TimeLimiter(name = "userService")
    public UserDto getUser(Long id) {
        return restClient.get()
            .uri("/api/users/{id}", id)
            .retrieve()
            .body(UserDto.class);
    }

    // Fallback — вызывается при сбое
    private UserDto getUserFallback(Long id, Throwable ex) {
        log.warn("User service unavailable, returning fallback for user {}", id, ex);
        return new UserDto(id, "Unknown User", null);
    }
}
```

```yaml
# application.yml — настройка Resilience4j
resilience4j:
  circuitbreaker:
    instances:
      userService:
        sliding-window-size: 10
        failure-rate-threshold: 50
        wait-duration-in-open-state: 10s
        permitted-number-of-calls-in-half-open-state: 3
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

### Saga Pattern — распределённые транзакции

Saga — это последовательность локальных транзакций, где каждый шаг имеет компенсирующее действие для отката.

```java
// Хореографическая Saga (через события)
//
// 1. Order Service: создать заказ → опубликовать ORDER_CREATED
// 2. Payment Service: обработать платёж → опубликовать PAYMENT_COMPLETED
// 3. Stock Service: зарезервировать товар → опубликовать STOCK_RESERVED
// 4. Delivery Service: создать доставку → опубликовать DELIVERY_SCHEDULED
//
// Если на шаге N произошла ошибка, публикуется событие-компенсация:
// PAYMENT_FAILED → Order Service откатывает заказ (ORDER_CANCELLED)

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
            // Компенсация — уведомляем об ошибке
            kafkaTemplate.send("payment-events",
                new PaymentEvent(event.orderId(), "PAYMENT_FAILED"));
        }
    }
}

// Оркестрационная Saga (централизованный координатор)
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
            // Шаг 1: Резервируем товар
            StockReservation reservation = stockClient.reserve(order.getItems());

            // Шаг 2: Обрабатываем платёж
            PaymentResult payment;
            try {
                payment = paymentClient.charge(order.getUserId(), order.getTotal());
            } catch (Exception e) {
                // Компенсация шага 1
                stockClient.cancelReservation(reservation.getId());
                throw e;
            }

            // Шаг 3: Создаём доставку
            try {
                deliveryClient.schedule(order.getId(), order.getDeliveryAddress());
            } catch (Exception e) {
                // Компенсация шагов 1 и 2
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

### Choreography vs Orchestration

| Характеристика | Хореография (события) | Оркестрация (координатор) |
|---------------|----------------------|--------------------------|
| Связанность | Слабая (через события) | Средняя (координатор знает все шаги) |
| Простота | Простые Saga | Сложные Saga с множеством шагов |
| Отслеживание | Сложно (события распределены) | Просто (всё в координаторе) |
| Единая точка отказа | Нет | Да (координатор) |
| Масштабируемость | Высокая | Средняя |

### Практический пример: полная архитектура

```yaml
# docker-compose.yml — пример микросервисной архитектуры
services:
  api-gateway:
    image: api-gateway:latest
    ports: ["8080:8080"]
    environment:
      USER_SERVICE_URL: http://user-service:8081
      ORDER_SERVICE_URL: http://order-service:8082

  user-service:
    image: user-service:latest
    ports: ["8081:8081"]
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://user-db:5432/users
      KAFKA_BOOTSTRAP_SERVERS: kafka:9092

  order-service:
    image: order-service:latest
    ports: ["8082:8082"]
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://order-db:5432/orders
      KAFKA_BOOTSTRAP_SERVERS: kafka:9092
      USER_SERVICE_URL: http://user-service:8081

  notification-service:
    image: notification-service:latest
    environment:
      KAFKA_BOOTSTRAP_SERVERS: kafka:9092

  kafka:
    image: confluentinc/cp-kafka:7.6.0
    ports: ["9092:9092"]

  user-db:
    image: postgres:16
  order-db:
    image: postgres:16
```

```
                    ┌────────────┐
                    │API Gateway │ ← auth, routing, rate limiting
                    │(port 8080) │
                    └────┬───┬───┘
                         │   │
              ┌──────────┘   └──────────┐
              │                         │
        ┌─────▼──────┐           ┌──────▼─────┐
        │User Service│           │Order Service│ ← REST (синхронно)
        │(port 8081) │◄──────── │(port 8082)  │
        └─────┬──────┘           └──────┬──────┘
              │                         │
              ▼                         ▼
        ┌──────────┐              ┌──────────┐
        │ user-db  │              │ order-db  │
        └──────────┘              └──────────┘
              │                         │
              └──────────┬──────────────┘
                         ▼
                  ┌─────────────┐
                  │    Kafka    │ ← события (асинхронно)
                  └──────┬──────┘
                         │
                  ┌──────▼───────┐
                  │ Notification │
                  │   Service    │
                  └──────────────┘
```

### Важное
+ **Каждый микросервис владеет своими данными** (database per service). Нет общей БД.
+ **Синхронная связь (REST/gRPC)** — для запросов, которые требуют немедленного ответа. **Асинхронная (Kafka/RabbitMQ)** — для событий и уведомлений.
+ **Circuit Breaker** обязателен для всех синхронных вызовов между сервисами.
+ **Saga Pattern** заменяет распределённые транзакции. Каждый шаг должен иметь компенсирующее действие.
+ **Idempotency** — все обработчики событий должны быть идемпотентными (событие может быть доставлено повторно).

### Частые ошибки
+ **Общая база данных** для нескольких сервисов — нарушает принцип автономности, создаёт tight coupling.
+ **Отсутствие Circuit Breaker** — сбой одного сервиса каскадно «роняет» всю систему.
+ **Синхронные цепочки вызовов** (A → B → C → D) — увеличивают латентность и снижают надёжность. Используйте асинхронные события.
+ **Распределённые транзакции (2PC)** — не масштабируются. Используйте Saga.
+ **Отсутствие idempotency в consumer-ах** — повторная обработка события создаёт дубликаты.
+ **Слишком мелкие сервисы (nano-services)** — overhead на коммуникацию превышает пользу от разделения. Сервис должен покрывать целый bounded context.

### Как используется в 2026
+ **Spring Boot 3.x + Virtual Threads** — виртуальные потоки (Project Loom) позволяют писать синхронный код с производительностью реактивного.
+ **Spring Modulith** — альтернатива микросервисам для проектов среднего размера. Модульный монолит с чёткими границами модулей и возможностью выделения в отдельные сервисы.
+ **Testcontainers** — стандарт для интеграционного тестирования микросервисов (Kafka, PostgreSQL, Redis в Docker).
+ **OpenTelemetry** — единый стандарт для distributed tracing, metrics и logging.
+ **Dapr (Distributed Application Runtime)** — runtime для микросервисов с абстракциями для pub/sub, state management и service invocation.
+ **Service Mesh (Istio, Linkerd)** берёт на себя retry, circuit breaker и mTLS, освобождая код приложения от инфраструктурных задач.

[к оглавлению](#REST-API)
