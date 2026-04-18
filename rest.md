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
