[Вопросы для собеседования](README.md)

# Разработка для Jira

**Общие вопросы**

+ [Какие платформы разработки для Jira существуют?](#Какие-платформы-разработки-для-Jira-существуют)
+ [Какие существуют способы расширения Jira?](#Какие-существуют-способы-расширения-Jira)
+ [Что такое Jira REST API и как с ним работать?](#Что-такое-Jira-REST-API-и-как-с-ним-работать)
+ [Что такое JQL (Jira Query Language)?](#Что-такое-JQL-Jira-Query-Language)
+ [Что такое Webhooks в Jira и как их использовать?](#Что-такое-Webhooks-в-Jira-и-как-их-использовать)
+ [Как интегрировать внешнее Spring-приложение с Jira?](#Как-интегрировать-внешнее-Spring-приложение-с-Jira)

**Jira Data Center (P2 плагины)**

+ [Что такое Atlassian SDK и как создать плагин?](#Что-такое-Atlassian-SDK-и-как-создать-плагин)
+ [Что такое atlassian-plugin.xml и какие модули плагина существуют?](#Что-такое-atlassian-pluginxml-и-какие-модули-плагина-существуют)
+ [Как работает Spring в плагинах Jira Data Center?](#Как-работает-Spring-в-плагинах-Jira-Data-Center)
+ [Что такое Active Objects и как с ними работать?](#Что-такое-Active-Objects-и-как-с-ними-работать)
+ [Как создать REST-эндпоинт в плагине Jira DC?](#Как-создать-REST-эндпоинт-в-плагине-Jira-DC)
+ [Как создать Servlet Module в плагине?](#Как-создать-Servlet-Module-в-плагине)
+ [Как создать Custom Field Type?](#Как-создать-Custom-Field-Type)
+ [Как создать Workflow Post-Function, Condition, Validator?](#Как-создать-Workflow-Post-Function-Condition-Validator)
+ [Как работать с Event Listener в плагине?](#Как-работать-с-Event-Listener-в-плагине)
+ [Как работает кластеризация в Jira Data Center и как это влияет на плагины?](#Как-работает-кластеризация-в-Jira-Data-Center-и-как-это-влияет-на-плагины)
+ [Как тестировать плагины Jira DC?](#Как-тестировать-плагины-Jira-DC)
+ [Как отлаживать и профилировать плагины?](#Как-отлаживать-и-профилировать-плагины)

**Jira Cloud (Forge и Connect)**

+ [Что такое Forge и как создать Forge-приложение?](#Что-такое-Forge-и-как-создать-Forge-приложение)
+ [Что такое Connect и чем он отличается от Forge?](#Что-такое-Connect-и-чем-он-отличается-от-Forge)
+ [Как работает аутентификация и авторизация в Jira Cloud приложениях?](#Как-работает-аутентификация-и-авторизация-в-Jira-Cloud-приложениях)
+ [Что такое Forge Storage API и Custom UI?](#Что-такое-Forge-Storage-API-и-Custom-UI)
+ [Как обрабатывать события в Jira Cloud?](#Как-обрабатывать-события-в-Jira-Cloud)
+ [Какие ограничения и лимиты существуют в Jira Cloud API?](#Какие-ограничения-и-лимиты-существуют-в-Jira-Cloud-API)

**Архитектура и практики**

+ [Как проектировать приложение, совместимое с DC и Cloud?](#Как-проектировать-приложение-совместимое-с-DC-и-Cloud)
+ [Какие есть best practices при разработке для Jira?](#Какие-есть-best-practices-при-разработке-для-Jira)
+ [Что такое ScriptRunner и когда его использовать?](#Что-такое-ScriptRunner-и-когда-его-использовать)

---

## Какие платформы разработки для Jira существуют?

Jira развивалась как продукт и прошла через несколько платформ развёртывания, каждая из которых определяет свой подход к разработке расширений.

**Jira Server (EOL февраль 2024)**
- Однонодовая on-premise установка
- Плагины в формате P2 (OSGi-бандлы)
- **Прекращена продажа в феврале 2021, полный End of Life — февраль 2024**
- Все клиенты вынуждены мигрировать на Data Center или Cloud

**Jira Data Center**
- Кластерная on-premise установка (несколько нод за балансировщиком)
- Общая БД и файловая система между нодами
- Те же P2-плагины, что и Server, но с требованиями к кластерной совместимости
- Поддерживает «нулевой простой» при обновлении (zero-downtime upgrades)
- Лицензируется по количеству пользователей (от 500)

**Jira Cloud**
- SaaS-решение, управляемое Atlassian
- Два фреймворка для приложений: **Forge** (serverless) и **Connect** (self-hosted)
- Автоматические обновления, мультитенантность
- REST API v3, GraphQL (в development)

| Характеристика | Data Center | Cloud |
|---|---|---|
| Развёртывание | On-premise / IaaS | SaaS |
| Кастомизация | Полная (P2 плагины) | Через Forge/Connect |
| Контроль данных | Полный | Ограниченный |
| Обновления | Ручные | Автоматические |
| Масштабирование | Горизонтальное (кластер) | Автоматическое |
| API | REST v2 | REST v2/v3 |

**Миграция Server → DC:** относительно простая — тот же код плагинов, но нужно обеспечить кластерную совместимость (stateless-дизайн, распределённые кэши и блокировки).

**Миграция Server → Cloud:** сложная — полная переработка плагинов на Forge/Connect, миграция данных через Jira Cloud Migration Assistant (JCMA), переработка интеграций.

### Важное
- В 2026 году актуальны **только Data Center и Cloud**. Jira Server полностью прекращён.
- При выборе платформы ключевой фактор — требования к хранению данных (compliance, residency).
- DC подходит для организаций с жёсткими требованиями к контролю данных и инфраструктуры.

### Частые ошибки
- Разработка нового плагина под Server API без учёта DC-специфики (кластеризация).
- Предположение, что плагин для DC автоматически работает в Cloud — это совершенно разные платформы.
- Игнорирование сроков EOL при планировании — Server EOL уже наступил.

### Как используется в 2026
- Data Center остаётся основной платформой для крупных enterprise-клиентов с on-premise требованиями.
- Cloud доминирует по количеству инстансов и активно продвигается Atlassian.
- Atlassian инвестирует преимущественно в Cloud — новые фичи появляются там первыми.
- Forge стал рекомендуемым фреймворком для новых Cloud-приложений.

[к оглавлению](#Разработка-для-Jira)

---

## Какие существуют способы расширения Jira?

| Способ | Платформа | Язык | Хостинг | Сложность |
|---|---|---|---|---|
| P2 Plugin (Atlassian SDK) | Data Center | Java | Внутри Jira | Высокая |
| Forge App | Cloud | TypeScript/JS | Atlassian (serverless) | Средняя |
| Connect App | Cloud | Любой | Свой сервер | Средняя-Высокая |
| REST API Integration | DC + Cloud | Любой | Внешний | Низкая-Средняя |
| ScriptRunner | DC (+ Cloud limited) | Groovy / JS | Внутри Jira | Низкая |

**P2 плагины (Data Center)**
- Полный доступ к внутреннему API Jira (Java)
- Могут добавлять UI-элементы, workflow-функции, custom fields, REST-эндпоинты
- Деплоятся как OSGi-бандлы прямо в Jira
- Максимальная гибкость, но и максимальная ответственность (могут уронить Jira)

**Forge (Cloud)**
- Serverless-платформа Atlassian
- Код выполняется в изолированной среде Atlassian
- Ограниченный, но безопасный доступ к API
- UI через UI Kit (декларативный) или Custom UI (React)

**Connect (Cloud)**
- Self-hosted приложение, интегрированное через iframe и REST API
- Полный контроль над стеком и инфраструктурой
- JWT-аутентификация между Jira и приложением
- Подходит для сложных приложений с собственной БД

**REST API Integration**
- Внешнее приложение вызывает Jira REST API
- Не расширяет UI Jira напрямую
- Подходит для автоматизации, синхронизации данных, CI/CD

**ScriptRunner**
- Готовый плагин для DC, позволяющий писать Groovy-скрипты
- Script Listeners, Script Fields, Behaviours, REST Endpoints
- Быстрое прототипирование без полноценной разработки плагина
- Ограниченная Cloud-версия (JavaScript вместо Groovy)

### Важное
- Выбор способа расширения определяется платформой (DC vs Cloud) и сложностью задачи.
- P2-плагины дают максимальную мощь, но требуют глубокого понимания Jira internals.
- Для Cloud Atlassian рекомендует Forge как основной фреймворк, Connect — для случаев, когда нужен полный контроль.

### Частые ошибки
- Написание P2-плагина для простой задачи, решаемой ScriptRunner-скриптом за 20 минут.
- Выбор Connect вместо Forge без чёткого обоснования — Forge проще в поддержке.
- Попытка использовать REST API для задач, требующих интеграции в UI Jira.

### Как используется в 2026
- Forge стал зрелой платформой с поддержкой Custom UI, Storage API, async events.
- Connect остаётся востребованным для сложных enterprise-приложений с собственной инфраструктурой.
- P2-плагины для DC по-прежнему актуальны, но новые разработчики всё чаще начинают с Cloud.
- Растёт тренд на «dual-platform» приложения — единая бизнес-логика, адаптеры для DC и Cloud.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Jira REST API и как с ним работать?

Jira REST API — основной программный интерфейс для взаимодействия с Jira извне. Позволяет выполнять CRUD-операции над задачами, проектами, пользователями, workflow и другими сущностями.

**REST API v2 vs v3:**
- **v2** — стабильный API, доступен и в DC, и в Cloud. Базовый URL: `/rest/api/2/`
- **v3** — Cloud-only, поддерживает ADF (Atlassian Document Format) для rich-text, улучшенная пагинация. Базовый URL: `/rest/api/3/`

**Аутентификация:**

| Метод | DC | Cloud | Описание |
|---|---|---|---|
| Basic Auth | Да | Нет (deprecated) | `username:password` в Base64 |
| PAT (Personal Access Token) | Да | Нет | Токен, привязанный к пользователю DC |
| API Token | Нет | Да | Токен Atlassian аккаунта + email |
| OAuth 2.0 (3LO) | Нет | Да | Полный OAuth-flow с авторизацией пользователя |
| OAuth 2.0 (2LO) | Нет | Да | App-level доступ без пользователя |

**Основные эндпоинты:**

```
GET    /rest/api/2/issue/{issueKey}          — получить задачу
POST   /rest/api/2/issue                      — создать задачу
PUT    /rest/api/2/issue/{issueKey}           — обновить задачу
DELETE /rest/api/2/issue/{issueKey}           — удалить задачу
POST   /rest/api/2/issue/{issueKey}/transitions — сменить статус
POST   /rest/api/2/search                     — поиск через JQL
GET    /rest/api/2/project                    — список проектов
GET    /rest/api/2/user?accountId=...         — информация о пользователе
```

**Пример: создание задачи (Java, Spring RestClient):**

```java
@Service
public class JiraIssueService {

    private final RestClient restClient;

    public JiraIssueService(@Value("${jira.base-url}") String baseUrl,
                            @Value("${jira.pat}") String pat) {
        this.restClient = RestClient.builder()
                .baseUrl(baseUrl + "/rest/api/2")
                .defaultHeader("Authorization", "Bearer " + pat)
                .defaultHeader("Content-Type", "application/json")
                .build();
    }

    public String createIssue(String projectKey, String summary, String description) {
        String body = """
                {
                    "fields": {
                        "project": {"key": "%s"},
                        "summary": "%s",
                        "description": "%s",
                        "issuetype": {"name": "Task"}
                    }
                }
                """.formatted(projectKey, summary, description);

        Map<String, Object> response = restClient.post()
                .uri("/issue")
                .body(body)
                .retrieve()
                .body(new ParameterizedTypeReference<>() {});

        return (String) response.get("key");
    }
}
```

**Пример: поиск через JQL:**

```java
public List<Map<String, Object>> searchIssues(String jql, int maxResults) {
    String body = """
            {
                "jql": "%s",
                "maxResults": %d,
                "fields": ["summary", "status", "assignee"]
            }
            """.formatted(jql, maxResults);

    Map<String, Object> response = restClient.post()
            .uri("/search")
            .body(body)
            .retrieve()
            .body(new ParameterizedTypeReference<>() {});

    return (List<Map<String, Object>>) response.get("issues");
}
```

**Пример: смена статуса (transition):**

```java
public void transitionIssue(String issueKey, String transitionId) {
    String body = """
            {
                "transition": {"id": "%s"}
            }
            """.formatted(transitionId);

    restClient.post()
            .uri("/issue/{issueKey}/transitions", issueKey)
            .body(body)
            .retrieve()
            .toBodilessEntity();
}
```

**Пагинация:**
- Параметры `startAt` и `maxResults`
- Ответ содержит `total`, `startAt`, `maxResults`
- Cloud API ограничивает `maxResults` до 100 для большинства эндпоинтов

**Rate Limiting (Cloud):**
- Лимиты на уровне пользователя и приложения
- HTTP 429 (Too Many Requests) с заголовком `Retry-After`
- Рекомендуется экспоненциальный backoff

### Важное
- Всегда используйте пагинацию для списковых запросов — не пытайтесь получить все данные за один вызов.
- В Cloud используйте `accountId` вместо `username` для идентификации пользователей (GDPR).
- REST API v3 в Cloud возвращает описание в формате ADF, а не в wiki-разметке.

### Частые ошибки
- Использование Basic Auth с паролем в Cloud — поддерживается только API Token + email.
- Игнорирование rate limiting — приложение получает 429 и «ломается».
- Жёсткое указание `maxResults=1000` — в Cloud реальный лимит 100, лишнее игнорируется.
- Не обработка ошибок 404 при обращении к удалённым или перемещённым задачам.

### Как используется в 2026
- REST API v3 — стандарт для Cloud-разработки.
- Spring 6+ `RestClient` и `WebClient` — основные HTTP-клиенты для Java-интеграций.
- Atlassian активно развивает GraphQL API (пока в beta), но REST остаётся основным.
- Personal Access Tokens (PAT) стали стандартом аутентификации для DC-интеграций.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое JQL (Jira Query Language)?

**JQL (Jira Query Language)** — структурированный язык запросов для поиска задач в Jira. Используется в UI (фильтры, дашборды), REST API (эндпоинт `/search`), плагинах и автоматизации.

**Базовый синтаксис:**
```
field operator value [AND|OR] field operator value ORDER BY field [ASC|DESC]
```

**Операторы:**

| Оператор | Описание | Пример |
|---|---|---|
| `=` | Равно | `project = "PROJ"` |
| `!=` | Не равно | `status != Done` |
| `>`, `<`, `>=`, `<=` | Сравнение | `created >= -7d` |
| `~` | Содержит (текстовый поиск) | `summary ~ "критическая ошибка"` |
| `!~` | Не содержит | `description !~ "test"` |
| `IN` | Одно из значений | `status IN (Open, "In Progress")` |
| `NOT IN` | Не одно из значений | `priority NOT IN (Low, Lowest)` |
| `IS EMPTY` | Пусто | `assignee IS EMPTY` |
| `IS NOT EMPTY` | Не пусто | `fixVersion IS NOT EMPTY` |
| `WAS` | Было (история) | `status WAS "In Progress"` |
| `CHANGED` | Изменилось | `assignee CHANGED` |

**Встроенные функции:**

```
-- Текущий пользователь
assignee = currentUser()

-- Дата: начало/конец дня, недели, месяца
created >= startOfDay()
created >= startOfWeek(-1)
duedate <= endOfMonth()

-- Члены группы
assignee IN membersOf("developers")

-- Связанные задачи
issue IN linkedIssues("PROJ-123")

-- Подзадачи
issue IN subtasksOf("PROJ-100")

-- Задачи из эпика
"Epic Link" = "PROJ-50"

-- Текстовый поиск по всем полям
text ~ "deploy*"
```

**Custom Fields в JQL:**
```
-- По имени (с кавычками)
"Story Points" > 5

-- По ID (надёжнее)
cf[10100] > 5
```

**Сложные запросы:**
```
project = PROJ
    AND status IN ("In Progress", "Code Review")
    AND assignee IN membersOf("backend-team")
    AND created >= startOfWeek()
    AND priority IN (Critical, Blocker)
    AND "Story Points" <= 8
ORDER BY priority DESC, created ASC
```

**Использование JQL в Java:**

```java
// Через REST API
public List<Issue> findCriticalBugs(String projectKey) {
    String jql = "project = %s AND type = Bug AND priority = Critical AND status != Done"
            .formatted(projectKey);

    String body = """
            {
                "jql": "%s",
                "maxResults": 50,
                "fields": ["summary", "status", "assignee", "priority"]
            }
            """.formatted(jql);

    return restClient.post()
            .uri("/search")
            .body(body)
            .retrieve()
            .body(SearchResult.class)
            .getIssues();
}

// В плагине Jira DC через SearchService
@Named
public class IssueSearchHelper {

    private final SearchService searchService;
    private final JqlQueryParser jqlQueryParser;

    @Inject
    public IssueSearchHelper(@ComponentImport SearchService searchService,
                             @ComponentImport JqlQueryParser jqlQueryParser) {
        this.searchService = searchService;
        this.jqlQueryParser = jqlQueryParser;
    }

    public SearchResults search(ApplicationUser user, String jqlString)
            throws SearchException, ParseException {
        Query query = jqlQueryParser.parseQuery(jqlString);
        return searchService.search(user, query, PagerFilter.getUnlimitedFilter());
    }
}
```

**Производительность JQL:**
- Индексируемые поля (`status`, `project`, `priority`) работают быстро
- Текстовый поиск (`~`) использует Lucene, может быть медленным на больших инстансах
- `WAS` и `CHANGED` операторы сканируют историю — дорогие операции
- Сложные `OR` с подзапросами замедляют выполнение
- Для DC: кастомные поля без индексации значительно замедляют поиск

### Важное
- JQL — основной инструмент поиска задач. Знание JQL ожидается от любого Jira-разработчика.
- Используйте `cf[id]` вместо имени поля — имена могут совпадать или меняться.
- Оператор `~` поддерживает Lucene-синтаксис: wildcards (`*`), fuzzy (`~`), exact phrase (`"..."`).

### Частые ошибки
- Использование `=` вместо `~` для текстового поиска: `summary = "error"` ищет точное совпадение.
- Забыть кавычки для значений с пробелами: `status = In Progress` — ошибка, нужно `"In Progress"`.
- Неэффективные запросы с `WAS`/`CHANGED` на инстансах с большой историей.
- Использование `ORDER BY` с неиндексированными кастомными полями.

### Как используется в 2026
- JQL остаётся основным языком запросов в Jira DC и Cloud.
- В Cloud добавлены новые функции: `issuesWithRemoteLinksByGlobalId()`, улучшенный полнотекстовый поиск.
- Atlassian экспериментирует с AI-ассистентом для генерации JQL из естественного языка.
- JQL используется в Jira Automation rules для определения scope правил.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Webhooks в Jira и как их использовать?

**Webhook** — механизм уведомления внешних систем о событиях в Jira через HTTP POST-запросы. Когда в Jira происходит определённое событие (создание задачи, смена статуса и т.д.), Jira отправляет JSON-payload на указанный URL.

**Доступные события:**

| Событие | Описание |
|---|---|
| `jira:issue_created` | Создание задачи |
| `jira:issue_updated` | Обновление задачи |
| `jira:issue_deleted` | Удаление задачи |
| `jira:worklog_updated` | Обновление worklog |
| `sprint_created` | Создание спринта |
| `sprint_started` | Старт спринта |
| `sprint_closed` | Закрытие спринта |
| `board_created` | Создание доски |
| `project_created` | Создание проекта |
| `comment_created` | Добавление комментария |
| `issuelink_created` | Создание связи |

**Регистрация webhook через REST API:**

```java
public void registerWebhook(String jiraBaseUrl, String token) {
    String body = """
            {
                "name": "My Integration Webhook",
                "url": "https://my-app.example.com/api/jira/webhook",
                "events": [
                    "jira:issue_created",
                    "jira:issue_updated"
                ],
                "filters": {
                    "issue-related-events-section": "project = PROJ AND type = Bug"
                },
                "excludeBody": false
            }
            """;

    restClient.post()
            .uri(jiraBaseUrl + "/rest/webhooks/1.0/webhook")
            .header("Authorization", "Bearer " + token)
            .body(body)
            .retrieve()
            .toBodilessEntity();
}
```

**Структура payload (issue_updated):**

```json
{
    "timestamp": 1700000000000,
    "webhookEvent": "jira:issue_updated",
    "issue_event_type_name": "issue_generic",
    "user": {
        "accountId": "5a1234567890",
        "displayName": "Ivan Petrov"
    },
    "issue": {
        "key": "PROJ-123",
        "fields": {
            "summary": "Исправить баг авторизации",
            "status": {"name": "In Progress"},
            "assignee": {"displayName": "Ivan Petrov"},
            "priority": {"name": "Critical"}
        }
    },
    "changelog": {
        "items": [
            {
                "field": "status",
                "fromString": "Open",
                "toString": "In Progress"
            }
        ]
    }
}
```

**Spring Boot webhook listener:**

```java
@RestController
@RequestMapping("/api/jira/webhook")
public class JiraWebhookController {

    private static final Logger log = LoggerFactory.getLogger(JiraWebhookController.class);

    @PostMapping
    public ResponseEntity<Void> handleWebhook(
            @RequestBody Map<String, Object> payload,
            @RequestHeader(value = "X-Atlassian-Webhook-Identifier",
                           required = false) String webhookId) {

        String event = (String) payload.get("webhookEvent");
        log.info("Получен webhook: event={}, id={}", event, webhookId);

        switch (event) {
            case "jira:issue_created" -> handleIssueCreated(payload);
            case "jira:issue_updated" -> handleIssueUpdated(payload);
            default -> log.warn("Неизвестное событие: {}", event);
        }

        return ResponseEntity.ok().build();
    }

    private void handleIssueCreated(Map<String, Object> payload) {
        Map<String, Object> issue = (Map<String, Object>) payload.get("issue");
        String key = (String) issue.get("key");
        Map<String, Object> fields = (Map<String, Object>) issue.get("fields");
        String summary = (String) fields.get("summary");
        log.info("Создана задача: {} - {}", key, summary);
        // Бизнес-логика: создать запись в системе, уведомить команду и т.д.
    }

    private void handleIssueUpdated(Map<String, Object> payload) {
        Map<String, Object> changelog = (Map<String, Object>) payload.get("changelog");
        if (changelog != null) {
            List<Map<String, Object>> items =
                    (List<Map<String, Object>>) changelog.get("items");
            for (Map<String, Object> item : items) {
                if ("status".equals(item.get("field"))) {
                    log.info("Статус изменён: {} → {}",
                            item.get("fromString"), item.get("toString"));
                }
            }
        }
    }
}
```

**Безопасность:**
- **Shared Secret** (DC): при регистрации webhook указывается secret, Jira подписывает payload HMAC-SHA256
- **IP Whitelisting**: ограничение входящих запросов по IP Jira-инстанса
- **HTTPS**: обязательно для production
- **Идемпотентность**: webhook может быть доставлен повторно — обработчик должен быть идемпотентным

### Важное
- Webhooks — это push-модель интеграции. В отличие от polling REST API, они обеспечивают near-real-time уведомления.
- Webhook должен ответить за 10 секунд (DC) / 30 секунд (Cloud), иначе считается failed.
- В Cloud используйте Forge Triggers вместо raw webhooks для более надёжной обработки событий.

### Частые ошибки
- Синхронная тяжёлая обработка в webhook-обработчике — нужно принять webhook, ответить 200 и обработать асинхронно.
- Отсутствие валидации подписи — любой может отправить поддельный webhook.
- Нет обработки дубликатов — Jira может повторно отправить webhook при timeout.
- Регистрация webhook без JQL-фильтра — получение всех событий инстанса перегружает приложение.

### Как используется в 2026
- В Cloud рекомендуется использовать Forge Triggers вместо прямой регистрации webhooks.
- Jira Automation (встроенная) заменяет простые webhook-сценарии (if event → then action).
- Для DC webhooks остаются основным механизмом push-интеграции.
- Тренд на event-driven архитектуру: Jira webhook → Kafka → микросервисы.

[к оглавлению](#Разработка-для-Jira)

---

## Как интегрировать внешнее Spring-приложение с Jira?

Интеграция внешнего Spring Boot приложения с Jira — частая задача enterprise-разработки. Основные сценарии: синхронизация задач, автоматизация workflow, агрегация данных из Jira.

**Архитектура интеграции:**

```
┌─────────────────┐         REST API          ┌──────────┐
│  Spring Boot    │ ──────────────────────────→│   Jira   │
│  Application    │ ←─────── Webhooks ─────── │  DC/Cloud│
│                 │                            │          │
│  - JiraClient   │         Events             │          │
│  - WebhookCtrl  │ ←──── (push model) ─────  │          │
│  - SyncService  │                            │          │
└─────────────────┘                            └──────────┘
```

**Конфигурация (application.yml):**

```yaml
jira:
  base-url: https://jira.company.com
  api-version: 2
  auth:
    type: pat  # pat | api-token | oauth2
    token: ${JIRA_PAT}
  connection:
    connect-timeout: 5s
    read-timeout: 10s
    max-connections: 20
  retry:
    max-attempts: 3
    backoff: 1s
```

**Jira Client с retry и error handling:**

```java
@Configuration
public class JiraClientConfig {

    @Bean
    public RestClient jiraRestClient(JiraProperties props) {
        return RestClient.builder()
                .baseUrl(props.getBaseUrl() + "/rest/api/" + props.getApiVersion())
                .defaultHeader("Authorization", "Bearer " + props.getAuth().getToken())
                .defaultHeader("Content-Type", "application/json")
                .defaultHeader("Accept", "application/json")
                .build();
    }
}

@Service
public class JiraClient {

    private static final Logger log = LoggerFactory.getLogger(JiraClient.class);
    private final RestClient restClient;
    private final RetryTemplate retryTemplate;

    public JiraClient(RestClient jiraRestClient) {
        this.restClient = jiraRestClient;
        this.retryTemplate = RetryTemplate.builder()
                .maxAttempts(3)
                .exponentialBackoff(1000, 2.0, 10000)
                .retryOn(RestClientException.class)
                .build();
    }

    public JiraIssue getIssue(String issueKey) {
        return retryTemplate.execute(ctx -> {
            log.debug("Запрос задачи {}, попытка {}", issueKey, ctx.getRetryCount() + 1);
            return restClient.get()
                    .uri("/issue/{key}", issueKey)
                    .retrieve()
                    .body(JiraIssue.class);
        });
    }

    public List<JiraIssue> searchByJql(String jql) {
        List<JiraIssue> allIssues = new ArrayList<>();
        int startAt = 0;
        int maxResults = 50;
        int total;

        do {
            SearchRequest request = new SearchRequest(jql, startAt, maxResults,
                    List.of("summary", "status", "assignee"));

            SearchResult result = retryTemplate.execute(ctx ->
                    restClient.post()
                            .uri("/search")
                            .body(request)
                            .retrieve()
                            .body(SearchResult.class));

            allIssues.addAll(result.getIssues());
            total = result.getTotal();
            startAt += maxResults;
        } while (startAt < total);

        return allIssues;
    }
}
```

**Использование jira-rest-java-client (JRJC) для DC:**

```xml
<dependency>
    <groupId>com.atlassian.jira</groupId>
    <artifactId>jira-rest-java-client-core</artifactId>
    <version>5.2.7</version>
</dependency>
```

```java
@Service
public class JrjcService {

    private final JiraRestClient jiraClient;

    public JrjcService(@Value("${jira.base-url}") String baseUrl,
                       @Value("${jira.username}") String user,
                       @Value("${jira.password}") String password) {
        JiraRestClientFactory factory = new AsynchronousJiraRestClientFactory();
        this.jiraClient = factory.createWithBasicHttpAuthentication(
                URI.create(baseUrl), user, password);
    }

    public Issue getIssue(String key) {
        return jiraClient.getIssueClient().getIssue(key).claim();
    }

    public void addComment(String issueKey, String text) {
        Issue issue = getIssue(issueKey);
        jiraClient.getIssueClient()
                .addComment(issue.getCommentsUri(), Comment.valueOf(text))
                .claim();
    }
}
```

**Синхронизация данных:**

```java
@Service
public class JiraSyncService {

    private final JiraClient jiraClient;
    private final TaskRepository taskRepository;

    @Scheduled(fixedDelay = 300_000) // каждые 5 минут
    public void syncRecentlyUpdated() {
        String jql = "project = PROJ AND updated >= -10m ORDER BY updated DESC";
        List<JiraIssue> issues = jiraClient.searchByJql(jql);

        for (JiraIssue issue : issues) {
            taskRepository.upsert(mapToTask(issue));
        }
    }

    private Task mapToTask(JiraIssue issue) {
        return Task.builder()
                .jiraKey(issue.getKey())
                .summary(issue.getFields().getSummary())
                .status(issue.getFields().getStatus().getName())
                .assignee(issue.getFields().getAssignee() != null
                        ? issue.getFields().getAssignee().getDisplayName() : null)
                .lastSynced(Instant.now())
                .build();
    }
}
```

### Важное
- Комбинируйте polling (scheduled sync) и push (webhooks) для надёжной синхронизации — webhooks для real-time, polling для catch-up.
- Всегда используйте пагинацию при поиске через JQL.
- Храните дату последней синхронизации и используйте `updated >= ...` для инкрементального обновления.

### Частые ошибки
- Хранение токенов/паролей в `application.yml` — используйте переменные окружения или Vault.
- Отсутствие retry-логики — Jira может временно возвращать 5xx.
- Игнорирование rate limiting в Cloud — нужно обрабатывать HTTP 429 и ждать `Retry-After`.
- Синхронные вызовы Jira API в обработчике HTTP-запроса — вызывает задержки для пользователя.

### Как используется в 2026
- Spring Boot 3.x + `RestClient` — стандартный стек для интеграций.
- JRJC (Jira REST Java Client) устарел и не обновляется — для новых проектов лучше собственный клиент.
- Популярны интеграции через Spring Cloud Stream / Kafka: webhook → Kafka topic → обработчик.
- Для Cloud-интеграций OAuth 2.0 (3LO) стал обязательным для user-context операций.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Atlassian SDK и как создать плагин?

**Atlassian SDK** — набор инструментов для разработки плагинов (P2) для продуктов Atlassian, включая Jira Data Center. Основан на Maven и предоставляет архетипы, команды сборки и локальное окружение для разработки.

**Установка и создание проекта:**

```bash
# Создание нового плагина из архетипа
atlas-create-jira-plugin
# Интерактивно: groupId, artifactId, version, package

# Структура созданного проекта:
my-jira-plugin/
├── pom.xml                          # Maven POM с Atlassian-зависимостями
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/plugin/  # Java-код плагина
│   │   └── resources/
│   │       ├── atlassian-plugin.xml # Дескриптор плагина
│   │       ├── css/
│   │       ├── js/
│   │       └── templates/           # Velocity-шаблоны
│   └── test/
│       ├── java/                    # Юнит-тесты
│       └── resources/
└── LICENSE
```

**Ключевые команды SDK:**

```bash
atlas-run            # Запуск Jira с плагином (полная сборка + развёртывание)
atlas-debug          # Запуск с remote debug на порту 5005
atlas-package        # Сборка JAR/OBR для деплоя
atlas-mvn clean install  # Maven-сборка через обёртку SDK
atlas-unit-test      # Запуск юнит-тестов
atlas-integration-test   # Запуск интеграционных тестов

# QuickReload: автоматическая перезагрузка плагина при изменениях
atlas-run --product jira --version 9.12.0
# Затем: atlas-mvn package (в другом терминале) → плагин перезагрузится
```

**pom.xml (ключевые части):**

```xml
<project>
    <parent>
        <groupId>com.atlassian.pom</groupId>
        <artifactId>public-pom</artifactId>
        <version>6.0.3</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>com.atlassian.jira</groupId>
            <artifactId>jira-api</artifactId>
            <version>${jira.version}</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.atlassian.plugin</groupId>
            <artifactId>atlassian-spring-scanner-annotation</artifactId>
            <version>${spring.scanner.version}</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>com.atlassian.maven.plugins</groupId>
                <artifactId>jira-maven-plugin</artifactId>
                <version>${amps.version}</version>
                <configuration>
                    <productVersion>${jira.version}</productVersion>
                    <productDataVersion>${jira.version}</productDataVersion>
                    <enableQuickReload>true</enableQuickReload>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**OSGi основы:**
P2-плагины — это OSGi-бандлы. Каждый плагин работает в своём загрузчике классов (ClassLoader), изолированно от других плагинов.

```
┌─────────────────────────────────────────┐
│             Jira Application            │
│  ┌───────────┐  ┌───────────┐          │
│  │ Plugin A  │  │ Plugin B  │          │
│  │ (Bundle)  │  │ (Bundle)  │          │
│  │           │  │           │          │
│  │ Import:   │  │ Export:   │          │
│  │  pkg.b    │  │  pkg.b    │          │
│  └───────────┘  └───────────┘          │
│                                         │
│  OSGi Framework (Felix)                 │
└─────────────────────────────────────────┘
```

- **Import-Package**: пакеты, которые плагин использует из хоста или других плагинов
- **Export-Package**: пакеты, которые плагин предоставляет другим
- `@ExportAsService` — публикация компонента как OSGi-сервиса
- `@ComponentImport` — импорт сервиса из Jira или другого плагина

### Важное
- Atlassian SDK основан на Maven — все стандартные знания Maven применимы.
- Зависимости Jira API имеют scope `provided` — они уже есть в рантайме Jira.
- QuickReload значительно ускоряет цикл разработки — не нужно перезапускать Jira при каждом изменении.
- atlas-run скачивает и запускает полноценный инстанс Jira — первый запуск занимает значительное время.

### Частые ошибки
- Добавление зависимостей без `provided` scope — плагин тащит конфликтующие библиотеки.
- Использование библиотек, несовместимых с OSGi (например, некоторые версии Guava, Jackson).
- Забыть `atlas-package` после изменений — QuickReload подхватывает только скомпилированный JAR.
- Разработка с версией Jira, отличной от production — API может отличаться.

### Как используется в 2026
- Atlassian SDK продолжает поддерживаться для DC, но Atlassian не добавляет новых функций.
- AMPS (Atlassian Maven Plugin Suite) версии 8.x — актуальная версия.
- DC 9.x и 10.x — актуальные версии Jira для разработки.
- Сообщество активно использует Docker для локальной разработки как альтернативу atlas-run.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое atlassian-plugin.xml и какие модули плагина существуют?

**atlassian-plugin.xml** — центральный дескриптор P2-плагина. Определяет метаданные плагина и перечисляет все модули (точки расширения), которые плагин предоставляет.

**Базовая структура:**

```xml
<atlassian-plugin key="com.example.my-plugin" name="My Jira Plugin"
                  plugins-version="2">

    <plugin-info>
        <description>Описание плагина</description>
        <version>1.0.0</version>
        <vendor name="Example Corp" url="https://example.com"/>
        <param name="plugin-icon">images/plugin-icon.png</param>
        <param name="plugin-logo">images/plugin-logo.png</param>
        <param name="atlassian-data-center-status">compatible</param>
        <param name="atlassian-data-center-compatible">true</param>
    </plugin-info>

    <!-- Модули плагина описываются ниже -->
</atlassian-plugin>
```

**Основные типы модулей:**

**1. REST Module** — REST-эндпоинты:
```xml
<rest key="my-rest" path="/myapi" version="1.0">
    <description>REST API плагина</description>
</rest>
```

**2. Servlet Module** — HTTP-сервлеты:
```xml
<servlet key="my-servlet" class="com.example.MyServlet">
    <url-pattern>/my-page</url-pattern>
</servlet>
<servlet-filter key="my-filter" class="com.example.MyFilter"
                location="before-dispatch">
    <url-pattern>/secure/*</url-pattern>
</servlet-filter>
```

**3. Web Item** — ссылки в UI Jira:
```xml
<web-item key="my-link" section="jira.issue.tools" weight="100">
    <label>Мой пункт меню</label>
    <link>/plugins/servlet/my-page?issueKey=${issue.key}</link>
    <condition class="com.atlassian.jira.plugin.webfragment.conditions.UserLoggedInCondition"/>
</web-item>
```

**4. Web Section** — секция для группировки web-items:
```xml
<web-section key="my-section" location="admin_plugins_menu" weight="100">
    <label>Мой раздел</label>
</web-section>
```

**5. Web Panel** — HTML-панели в страницах Jira:
```xml
<web-panel key="my-panel" location="atl.jira.view.issue.right.context" weight="200">
    <label>Дополнительная информация</label>
    <resource name="view" type="velocity" location="templates/my-panel.vm"/>
    <context-provider class="com.example.MyPanelContextProvider"/>
</web-panel>
```

**6. Web Resource** — CSS, JS ресурсы:
```xml
<web-resource key="my-resources" name="Plugin Resources">
    <dependency>com.atlassian.auiplugin:ajs</dependency>
    <resource type="download" name="app.js" location="js/app.js"/>
    <resource type="download" name="styles.css" location="css/styles.css"/>
    <context>atl.general</context>
</web-resource>
```

**7. Component** — Spring-компонент плагина:
```xml
<component key="myService" class="com.example.MyServiceImpl">
    <interface>com.example.MyService</interface>
</component>
```

**8. Component Import** — импорт компонента из Jira или другого плагина:
```xml
<component-import key="applicationProperties"
                  interface="com.atlassian.sal.api.ApplicationProperties"/>
```

**9. Workflow Function** — расширения workflow:
```xml
<workflow-function key="my-postfunction"
                   class="com.example.MyPostFunctionFactory">
    <function-class>com.example.MyPostFunction</function-class>
    <orderable>true</orderable>
    <unique>false</unique>
    <resource name="view" type="velocity" location="templates/postfunction-view.vm"/>
    <resource name="input-parameters" type="velocity"
              location="templates/postfunction-input.vm"/>
    <resource name="edit-parameters" type="velocity"
              location="templates/postfunction-edit.vm"/>
</workflow-function>
```

**10. Custom Field Type:**
```xml
<customfield-type key="my-field" class="com.example.MyCustomField">
    <name>Моё кастомное поле</name>
    <description>Описание поля</description>
    <resource name="view" type="velocity" location="templates/field-view.vm"/>
    <resource name="edit" type="velocity" location="templates/field-edit.vm"/>
</customfield-type>
```

**11. Listener** — обработчик событий:
```xml
<listener key="my-listener" class="com.example.MyListener">
    <description>Обработчик событий задач</description>
</listener>
```

**12. Report:**
```xml
<report key="my-report" class="com.example.MyReport">
    <label>Мой отчёт</label>
    <resource name="view" type="velocity" location="templates/report.vm"/>
</report>
```

### Важное
- С появлением Spring Scanner 2.x многие модули (component, component-import) можно объявлять через аннотации `@Named`, `@ComponentImport` вместо XML.
- Однако workflow-function, custom-field-type, web-panel, web-item по-прежнему требуют XML-объявления.
- `plugins-version="2"` — обязательный атрибут, означающий формат P2 (OSGi).

### Частые ошибки
- Дублирование key в модулях — key должен быть уникальным в рамках плагина.
- Объявление component-import для сервисов, которые уже доступны через Spring Scanner `@ComponentImport`.
- Неправильная секция (`section`/`location`) для web-item/web-panel — элемент просто не появляется в UI.
- Отсутствие `<condition>` для web-item — пункт меню виден всем, включая анонимных пользователей.

### Как используется в 2026
- XML-дескриптор остаётся обязательным для P2-плагинов, но содержимое минимизируется за счёт аннотаций.
- Spring Scanner 2.x — стандарт, 1.x считается устаревшим.
- Параметр `atlassian-data-center-compatible=true` обязателен для публикации на Marketplace.
- Atlassian рекомендует минимизировать количество модулей для ускорения загрузки плагина.

[к оглавлению](#Разработка-для-Jira)

---

## Как работает Spring в плагинах Jira Data Center?

Плагины Jira DC используют **Atlassian Spring Scanner** — обёртку над Spring Framework, адаптированную для работы в OSGi-среде. Это не стандартный Spring Boot — здесь нет автоконфигурации, component-scan по конвенциям, и аннотации отличаются.

**Ключевые аннотации:**

```java
import javax.inject.Inject;
import javax.inject.Named;
import com.atlassian.plugin.spring.scanner.annotation.imports.ComponentImport;
import com.atlassian.plugin.spring.scanner.annotation.export.ExportAsService;

// @Named — аналог @Component/@Service в Spring Boot
// @Inject — аналог @Autowired
// @ComponentImport — импорт сервиса из Jira (host application)
// @ExportAsService — публикация компонента как OSGi-сервиса
```

**Пример компонента плагина:**

```java
@Named
public class IssueHelperService {

    private final IssueManager issueManager;
    private final UserManager userManager;
    private final JiraAuthenticationContext authContext;

    @Inject
    public IssueHelperService(
            @ComponentImport IssueManager issueManager,
            @ComponentImport UserManager userManager,
            @ComponentImport JiraAuthenticationContext authContext) {
        this.issueManager = issueManager;
        this.userManager = userManager;
        this.authContext = authContext;
    }

    public MutableIssue getIssue(String key) {
        ApplicationUser user = authContext.getLoggedInUser();
        return issueManager.getIssueByCurrentKey(key);
    }
}
```

**Spring Scanner 2.x vs 1.x:**

| Аспект | Scanner 1.x | Scanner 2.x |
|---|---|---|
| Сканирование | Runtime (медленно) | Compile-time (быстро) |
| Конфигурация | XML + аннотации | Преимущественно аннотации |
| pom.xml | `atlassian-spring-scanner-annotation` | + `atlassian-spring-scanner-runtime` |
| Производительность | Медленный старт плагина | Быстрый старт |
| component-import в XML | Обязательно | Не нужно (через `@ComponentImport`) |

**Конфигурация Spring Scanner 2.x в pom.xml:**

```xml
<dependency>
    <groupId>com.atlassian.plugin</groupId>
    <artifactId>atlassian-spring-scanner-annotation</artifactId>
    <version>2.2.4</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.atlassian.plugin</groupId>
    <artifactId>atlassian-spring-scanner-runtime</artifactId>
    <version>2.2.4</version>
    <scope>runtime</scope>
</dependency>
```

И в `jira-maven-plugin`:
```xml
<configuration>
    <instructions>
        <Spring-Context>*</Spring-Context>
    </instructions>
</configuration>
```

**Отличия от стандартного Spring Boot:**

| Spring Boot | Jira Plugin (Spring Scanner) |
|---|---|
| `@Service`, `@Component` | `@Named` (javax.inject) |
| `@Autowired` | `@Inject` (javax.inject) |
| Component scan по пакетам | Spring Scanner — compile-time генерация |
| `@Configuration` + `@Bean` | Ограничено, чаще — XML или `@Named` |
| `@Value("${prop}")` | `SAL PluginSettings` или `System.getProperty()` |
| `ApplicationContext` | OSGi BundleContext |
| Spring Data JPA | Active Objects |

**Экспорт сервиса для других плагинов:**

```java
@ExportAsService({NotificationService.class})
@Named
public class NotificationServiceImpl implements NotificationService {

    @Inject
    public NotificationServiceImpl(@ComponentImport MailServerManager mailManager) {
        // ...
    }

    @Override
    public void notify(String userKey, String message) {
        // реализация
    }
}
```

**Другой плагин может импортировать этот сервис:**

```java
@Named
public class ConsumerComponent {

    @Inject
    public ConsumerComponent(
            @ComponentImport NotificationService notificationService) {
        // использование сервиса из другого плагина
    }
}
```

### Важное
- Всегда используйте constructor injection — field injection через `@Inject` на поле работает, но не рекомендуется.
- `@ComponentImport` нужен **только** для сервисов из Jira (host) или других плагинов, не для своих компонентов.
- Свои компоненты плагина инжектируются без `@ComponentImport`.

### Частые ошибки
- Использование `@Autowired` вместо `@Inject` — может не работать в OSGi-контексте.
- Использование `@Service`/`@Component` вместо `@Named` — Spring Scanner их не обнаруживает.
- Забыть `@ComponentImport` при инжекции Jira-сервиса — получите NoSuchBeanDefinitionException.
- Циклические зависимости между компонентами — Spring Scanner не всегда корректно их разрешает.
- Попытка использовать Spring Boot стартеры — они несовместимы с OSGi.

### Как используется в 2026
- Spring Scanner 2.x — стандарт для всех новых плагинов.
- Compile-time сканирование стало нормой — runtime-сканирование считается legacy.
- Многие разработчики приходят из Spring Boot и допускают типичные ошибки — знание различий критично.
- Atlassian не планирует переход на Spring Boot для DC-плагинов — OSGi-модель остаётся.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Active Objects и как с ними работать?

**Active Objects (AO)** — ORM-фреймворк для плагинов Jira Data Center. Позволяет плагинам хранить данные в БД Jira, не создавая таблицы вручную. По концепции похож на JPA, но значительно легковеснее.

**Ключевые особенности:**
- Таблицы создаются автоматически из Java-интерфейсов
- Префикс таблиц — уникальный для каждого плагина (избежание конфликтов)
- Поддержка миграций (upgrade tasks)
- Работает с БД Jira (PostgreSQL, MySQL, Oracle, MS SQL)

**Определение сущности (entity):**

```java
@Table("TASK_CONFIG")
@Preload  // загружать все поля при выборке (оптимизация)
public interface TaskConfig extends Entity {
    // Entity предоставляет int getID() автоматически

    @StringLength(255)
    @NotNull
    String getProjectKey();
    void setProjectKey(String projectKey);

    @StringLength(StringLength.UNLIMITED)
    String getConfiguration();
    void setConfiguration(String configuration);

    boolean isEnabled();
    void setEnabled(boolean enabled);

    @Default("0")
    int getPriority();
    void setPriority(int priority);

    // Связь один-ко-многим
    @OneToMany(reverse = "getTaskConfig")
    TaskExecution[] getExecutions();
}

@Table("TASK_EXEC")
public interface TaskExecution extends Entity {

    @NotNull
    TaskConfig getTaskConfig();
    void setTaskConfig(TaskConfig config);

    @StringLength(50)
    String getStatus();
    void setStatus(String status);

    long getStartedAt();
    void setStartedAt(long startedAt);

    long getFinishedAt();
    void setFinishedAt(long finishedAt);

    @StringLength(StringLength.UNLIMITED)
    String getErrorMessage();
    void setErrorMessage(String message);
}
```

**Регистрация AO в atlassian-plugin.xml:**

```xml
<ao key="ao-module">
    <description>Active Objects модуль</description>
    <entity>com.example.plugin.ao.TaskConfig</entity>
    <entity>com.example.plugin.ao.TaskExecution</entity>
</ao>
```

**Зависимость в pom.xml:**

```xml
<dependency>
    <groupId>com.atlassian.activeobjects</groupId>
    <artifactId>activeobjects-plugin</artifactId>
    <version>${ao.version}</version>
    <scope>provided</scope>
</dependency>
```

**CRUD-операции:**

```java
@Named
public class TaskConfigService {

    private final ActiveObjects ao;

    @Inject
    public TaskConfigService(@ComponentImport ActiveObjects ao) {
        this.ao = ao;
    }

    // CREATE
    public TaskConfig create(String projectKey, String config) {
        return ao.executeInTransaction(() -> {
            TaskConfig entity = ao.create(TaskConfig.class);
            entity.setProjectKey(projectKey);
            entity.setConfiguration(config);
            entity.setEnabled(true);
            entity.setPriority(0);
            entity.save();
            return entity;
        });
    }

    // READ — по ID
    public TaskConfig getById(int id) {
        return ao.get(TaskConfig.class, id);
    }

    // READ — поиск по условию
    public TaskConfig[] findByProject(String projectKey) {
        return ao.find(TaskConfig.class,
                Query.select()
                        .where("PROJECT_KEY = ?", projectKey)
                        .order("PRIORITY DESC")
                        .limit(100));
    }

    // READ — все записи
    public TaskConfig[] findAll() {
        return ao.find(TaskConfig.class);
    }

    // UPDATE
    public void update(int id, String newConfig) {
        ao.executeInTransaction(() -> {
            TaskConfig entity = ao.get(TaskConfig.class, id);
            if (entity != null) {
                entity.setConfiguration(newConfig);
                entity.save();
            }
            return null;
        });
    }

    // DELETE
    public void delete(int id) {
        ao.executeInTransaction(() -> {
            TaskConfig entity = ao.get(TaskConfig.class, id);
            if (entity != null) {
                // Сначала удалить дочерние записи
                for (TaskExecution exec : entity.getExecutions()) {
                    ao.delete(exec);
                }
                ao.delete(entity);
            }
            return null;
        });
    }
}
```

**Миграция (Upgrade Task):**

```java
public class UpgradeTask001 implements ActiveObjectsUpgradeTask {

    @Override
    public ModelVersion getModelVersion() {
        return ModelVersion.valueOf("1");
    }

    @Override
    public void upgrade(ModelVersion currentVersion, ActiveObjects ao) {
        ao.migrate(TaskConfig.class); // создаёт таблицу, если не существует
    }
}

public class UpgradeTask002 implements ActiveObjectsUpgradeTask {

    @Override
    public ModelVersion getModelVersion() {
        return ModelVersion.valueOf("2");
    }

    @Override
    public void upgrade(ModelVersion currentVersion, ActiveObjects ao) {
        ao.migrate(TaskConfig.class, TaskExecution.class);
        // Добавляет новую таблицу TaskExecution и новые колонки в TaskConfig
    }
}
```

```xml
<ao key="ao-module">
    <entity>com.example.plugin.ao.TaskConfig</entity>
    <entity>com.example.plugin.ao.TaskExecution</entity>
    <upgradeTask>com.example.plugin.upgrade.UpgradeTask001</upgradeTask>
    <upgradeTask>com.example.plugin.upgrade.UpgradeTask002</upgradeTask>
</ao>
```

### Важное
- AO — единственный поддерживаемый способ хранения данных плагина в БД Jira DC.
- Имена колонок в SQL-запросах пишутся UPPERCASE с underscore: `projectKey` → `PROJECT_KEY`.
- `@Preload` критически важен для производительности — без него каждое поле загружается отдельным SQL-запросом.
- Все операции записи должны выполняться в `executeInTransaction()`.

### Частые ошибки
- Забыть `@Preload` — N+1 проблема, каждый доступ к полю = отдельный SELECT.
- Использование Java-имён полей в Query вместо SQL-имён: `"projectKey"` вместо `"PROJECT_KEY"`.
- Не обернуть запись в транзакцию — данные могут быть в inconsistent-состоянии.
- Отсутствие upgrade tasks — при первом деплое таблицы не создаются.
- Хранение больших данных без `@StringLength(UNLIMITED)` — дефолтная длина строки 255 символов.

### Как используется в 2026
- Active Objects остаётся стандартом для DC-плагинов.
- Нет альтернатив (JPA/Hibernate недоступны из-за OSGi-изоляции).
- Для Cloud (Forge) аналог — Forge Storage API и Entity Storage.
- При планировании миграции DC → Cloud нужно продумать перенос данных из AO в Cloud storage.

[к оглавлению](#Разработка-для-Jira)

---

## Как создать REST-эндпоинт в плагине Jira DC?

REST-модуль плагина позволяет создавать API-эндпоинты, доступные по URL вида `/rest/myapi/1.0/...`. Используется JAX-RS (Jersey).

**Объявление в atlassian-plugin.xml:**

```xml
<rest key="my-rest-api" path="/myapi" version="1.0">
    <description>REST API моего плагина</description>
</rest>
```

**REST-ресурс:**

```java
@Path("/config")
@Consumes({MediaType.APPLICATION_JSON})
@Produces({MediaType.APPLICATION_JSON})
@Named
public class ConfigResource {

    private final TaskConfigService configService;
    private final JiraAuthenticationContext authContext;
    private final PermissionManager permissionManager;

    @Inject
    public ConfigResource(TaskConfigService configService,
                          @ComponentImport JiraAuthenticationContext authContext,
                          @ComponentImport PermissionManager permissionManager) {
        this.configService = configService;
        this.authContext = authContext;
        this.permissionManager = permissionManager;
    }

    @GET
    @Path("/{id}")
    public Response getConfig(@PathParam("id") int id) {
        // Проверка аутентификации
        ApplicationUser user = authContext.getLoggedInUser();
        if (user == null) {
            return Response.status(Response.Status.UNAUTHORIZED).build();
        }

        TaskConfig config = configService.getById(id);
        if (config == null) {
            return Response.status(Response.Status.NOT_FOUND)
                    .entity(new ErrorResponse("Config not found"))
                    .build();
        }

        return Response.ok(ConfigDto.from(config)).build();
    }

    @POST
    public Response createConfig(CreateConfigRequest request) {
        ApplicationUser user = authContext.getLoggedInUser();
        if (user == null) {
            return Response.status(Response.Status.UNAUTHORIZED).build();
        }

        // Проверка прав — только Jira Admin
        if (!permissionManager.hasPermission(GlobalPermissionKey.ADMINISTER, user)) {
            return Response.status(Response.Status.FORBIDDEN)
                    .entity(new ErrorResponse("Admin permission required"))
                    .build();
        }

        TaskConfig created = configService.create(
                request.getProjectKey(), request.getConfiguration());

        return Response.status(Response.Status.CREATED)
                .entity(ConfigDto.from(created))
                .build();
    }

    @PUT
    @Path("/{id}")
    public Response updateConfig(@PathParam("id") int id, UpdateConfigRequest request) {
        ApplicationUser user = authContext.getLoggedInUser();
        if (user == null) {
            return Response.status(Response.Status.UNAUTHORIZED).build();
        }

        configService.update(id, request.getConfiguration());
        return Response.ok().build();
    }

    @DELETE
    @Path("/{id}")
    public Response deleteConfig(@PathParam("id") int id) {
        ApplicationUser user = authContext.getLoggedInUser();
        if (!permissionManager.hasPermission(GlobalPermissionKey.ADMINISTER, user)) {
            return Response.status(Response.Status.FORBIDDEN).build();
        }

        configService.delete(id);
        return Response.noContent().build();
    }
}
```

**DTO:**

```java
@XmlRootElement
public class ConfigDto {
    @XmlElement private int id;
    @XmlElement private String projectKey;
    @XmlElement private String configuration;
    @XmlElement private boolean enabled;

    public static ConfigDto from(TaskConfig entity) {
        ConfigDto dto = new ConfigDto();
        dto.id = entity.getID();
        dto.projectKey = entity.getProjectKey();
        dto.configuration = entity.getConfiguration();
        dto.enabled = entity.isEnabled();
        return dto;
    }
}
```

**Анонимный доступ:**

```java
@GET
@Path("/public/health")
@AnonymousAllowed  // Доступен без аутентификации
public Response healthCheck() {
    return Response.ok(Map.of("status", "UP")).build();
}
```

URL для вызова: `GET https://jira.company.com/rest/myapi/1.0/config/42`

### Важное
- REST-ресурсы сканируются автоматически в пакетах, указанных в `<rest>` модуле.
- По умолчанию все эндпоинты требуют аутентификацию — `@AnonymousAllowed` для открытых.
- Используйте `@XmlRootElement` или Jackson-аннотации для сериализации DTO.

### Частые ошибки
- Возвращение `Entity` (AO) напрямую вместо DTO — вызывает LazyLoading-ошибки и утечку данных.
- Забыть проверку прав — REST-эндпоинт доступен любому аутентифицированному пользователю.
- Использование `@Autowired` вместо `@Inject` в REST-ресурсе.
- CORS: Jira не добавляет CORS-заголовки по умолчанию — для внешних клиентов нужен servlet-filter.

### Как используется в 2026
- REST-эндпоинты в плагинах — основной способ API-интеграции для DC.
- JAX-RS (Jersey) остаётся стандартом; переход на другие фреймворки невозможен из-за OSGi.
- Для CORS рекомендуется использовать Atlassian CORS Plugin или собственный фильтр.

[к оглавлению](#Разработка-для-Jira)

---

## Как создать Servlet Module в плагине?

Servlet Module позволяет создавать веб-страницы внутри Jira с собственным UI. Используется для административных панелей, конфигурационных страниц плагина, отчётов.

**Объявление в atlassian-plugin.xml:**

```xml
<servlet key="config-servlet" class="com.example.plugin.servlet.ConfigServlet">
    <url-pattern>/my-plugin/config</url-pattern>
</servlet>
```

**Реализация сервлета:**

```java
@Named
public class ConfigServlet extends HttpServlet {

    private final TemplateRenderer templateRenderer;
    private final JiraAuthenticationContext authContext;
    private final PermissionManager permissionManager;
    private final TaskConfigService configService;

    @Inject
    public ConfigServlet(@ComponentImport TemplateRenderer templateRenderer,
                         @ComponentImport JiraAuthenticationContext authContext,
                         @ComponentImport PermissionManager permissionManager,
                         TaskConfigService configService) {
        this.templateRenderer = templateRenderer;
        this.authContext = authContext;
        this.permissionManager = permissionManager;
        this.configService = configService;
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        // Проверка аутентификации
        ApplicationUser user = authContext.getLoggedInUser();
        if (user == null) {
            resp.sendRedirect("/login.jsp");
            return;
        }

        // Проверка прав
        if (!permissionManager.hasPermission(GlobalPermissionKey.ADMINISTER, user)) {
            resp.sendError(HttpServletResponse.SC_FORBIDDEN, "Admin access required");
            return;
        }

        // Подготовка данных для шаблона
        Map<String, Object> context = new HashMap<>();
        context.put("configs", configService.findAll());
        context.put("currentUser", user.getDisplayName());
        context.put("baseUrl", ComponentAccessor.getApplicationProperties()
                .getString(APKeys.JIRA_BASEURL));

        // Рендеринг Velocity-шаблона
        resp.setContentType("text/html;charset=UTF-8");
        templateRenderer.render("templates/config-page.vm", context, resp.getWriter());
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        ApplicationUser user = authContext.getLoggedInUser();
        if (user == null || !permissionManager.hasPermission(
                GlobalPermissionKey.ADMINISTER, user)) {
            resp.sendError(HttpServletResponse.SC_FORBIDDEN);
            return;
        }

        String projectKey = req.getParameter("projectKey");
        String configuration = req.getParameter("configuration");
        configService.create(projectKey, configuration);

        resp.sendRedirect(req.getContextPath() + "/plugins/servlet/my-plugin/config");
    }
}
```

**Velocity-шаблон (templates/config-page.vm):**

```html
<html>
<head>
    <title>Конфигурация плагина</title>
    <meta name="decorator" content="atl.admin"/>
    $webResourceManager.requireResource("com.example.my-plugin:my-resources")
</head>
<body>
    <h2>Настройки плагина</h2>

    <form method="post" class="aui">
        <div class="field-group">
            <label for="projectKey">Проект:</label>
            <input type="text" id="projectKey" name="projectKey" class="text"/>
        </div>
        <div class="field-group">
            <label for="configuration">Конфигурация:</label>
            <textarea id="configuration" name="configuration" class="textarea"></textarea>
        </div>
        <div class="buttons-container">
            <input type="submit" value="Сохранить" class="aui-button aui-button-primary"/>
        </div>
    </form>

    <h3>Существующие конфигурации</h3>
    <table class="aui">
        <thead>
            <tr><th>ID</th><th>Проект</th><th>Статус</th></tr>
        </thead>
        <tbody>
            #foreach($config in $configs)
            <tr>
                <td>$config.getID()</td>
                <td>$config.getProjectKey()</td>
                <td>#if($config.isEnabled()) Активна #else Отключена #end</td>
            </tr>
            #end
        </tbody>
    </table>
</body>
</html>
```

URL доступа: `https://jira.company.com/plugins/servlet/my-plugin/config`

### Важное
- Декоратор `atl.admin` оборачивает страницу в стандартный layout Jira с навигацией.
- `$webResourceManager.requireResource(...)` подключает CSS/JS ресурсы плагина.
- Velocity — стандартный шаблонизатор в Jira; Freemarker и другие не поддерживаются.

### Частые ошибки
- Забыть декоратор — страница отображается без навигации Jira.
- XSS-уязвимости — не экранировать пользовательский ввод в шаблоне. Используйте `$!{htmlUtil.htmlEncode($value)}`.
- Не проверять права доступа — сервлет доступен любому аутентифицированному пользователю.
- Использование `ComponentAccessor` вместо DI — антипаттерн, затрудняет тестирование.

### Как используется в 2026
- Servlet Module остаётся актуальным для административных страниц плагинов.
- Тренд: минимум серверного рендеринга, максимум SPA (React/Vue) + REST API плагина.
- AUI (Atlassian UI) — по-прежнему рекомендованная UI-библиотека для DC.

[к оглавлению](#Разработка-для-Jira)

---

## Как создать Custom Field Type?

Custom Field Type позволяет создать собственный тип поля задачи с кастомной логикой ввода, отображения, валидации и поиска.

**Объявление в atlassian-plugin.xml:**

```xml
<customfield-type key="priority-score-field"
                  class="com.example.plugin.field.PriorityScoreField">
    <name>Priority Score</name>
    <description>Числовой приоритет с расчётом на основе severity и impact</description>
    <resource name="view" type="velocity"
              location="templates/fields/priority-score-view.vm"/>
    <resource name="edit" type="velocity"
              location="templates/fields/priority-score-edit.vm"/>
    <resource name="xml" type="velocity"
              location="templates/fields/priority-score-xml.vm"/>
</customfield-type>
```

**Реализация кастомного поля:**

```java
@Named
public class PriorityScoreField extends AbstractSingleFieldType<Double> {

    private static final Logger log = LoggerFactory.getLogger(PriorityScoreField.class);

    @Inject
    public PriorityScoreField(
            @ComponentImport CustomFieldValuePersister persister,
            @ComponentImport GenericConfigManager configManager) {
        super(persister, configManager);
    }

    @Override
    protected PersistenceFieldType getDatabaseType() {
        return PersistenceFieldType.TYPE_DECIMAL;
    }

    @Override
    protected Object getDbValueFromObject(Double value) {
        return value;
    }

    @Override
    protected Double getObjectValue(String dbValue) {
        if (dbValue == null) {
            return null;
        }
        try {
            return Double.parseDouble(dbValue);
        } catch (NumberFormatException e) {
            log.warn("Невозможно распарсить значение: {}", dbValue);
            return null;
        }
    }

    @Override
    public String getStringFromSingularObject(Double value) {
        return value != null ? value.toString() : "";
    }

    @Override
    public Double getSingularObjectFromString(String s)
            throws FieldValidationException {
        if (s == null || s.trim().isEmpty()) {
            return null;
        }
        try {
            double value = Double.parseDouble(s);
            if (value < 0 || value > 100) {
                throw new FieldValidationException(
                        "Priority Score должен быть от 0 до 100");
            }
            return value;
        } catch (NumberFormatException e) {
            throw new FieldValidationException("Некорректное числовое значение: " + s);
        }
    }

    @Override
    public Map<String, Object> getVelocityParameters(Issue issue,
            CustomField field, FieldLayoutItem fieldLayoutItem) {
        Map<String, Object> params = super.getVelocityParameters(
                issue, field, fieldLayoutItem);

        // Добавляем дополнительные данные для шаблона
        Double value = (Double) issue.getCustomFieldValue(field);
        if (value != null) {
            String cssClass;
            if (value >= 80) cssClass = "aui-lozenge-error";
            else if (value >= 50) cssClass = "aui-lozenge-current";
            else cssClass = "aui-lozenge-success";
            params.put("cssClass", cssClass);
            params.put("formattedValue", String.format("%.1f", value));
        }

        return params;
    }
}
```

**Velocity-шаблоны:**

```html
<!-- templates/fields/priority-score-edit.vm -->
<input type="text"
       id="$customField.id"
       name="$customField.id"
       value="$!{value}"
       class="text short-field"
       placeholder="0-100"/>
<div class="description">Введите числовое значение от 0 до 100</div>
```

```html
<!-- templates/fields/priority-score-view.vm -->
#if($formattedValue)
    <span class="aui-lozenge $cssClass">$formattedValue</span>
#else
    <span class="aui-lozenge">Не задано</span>
#end
```

**Поддержка поиска (Searcher):**

```java
public class PriorityScoreSearcher extends AbstractInitializationCustomFieldSearcher
        implements CustomFieldSearcher {

    @Override
    public SearcherGroupType getSearcherGroupType() {
        return SearcherGroupType.CUSTOM;
    }
    // ... реализация для возможности поиска по кастомному полю через JQL
}
```

```xml
<customfield-searcherkey="priority-score-searcher"
                       class="com.example.plugin.field.PriorityScoreSearcher">
    <name>Priority Score Searcher</name>
    <valid-customfield-type
            package="com.example.my-plugin" key="priority-score-field"/>
</customfield-searcher>
```

### Важное
- Для простых типов (текст, число) наследуйтесь от `AbstractSingleFieldType`.
- Для множественных значений — от `AbstractMultiCFType`.
- Searcher обязателен, если поле должно быть доступно в JQL.
- Шаблоны `view`, `edit`, `xml` — обязательные; `column-view` — опциональный (для отображения в списке задач).

### Частые ошибки
- Не реализовать Searcher — поле нельзя использовать в JQL и фильтрах.
- Не обрабатывать `null`/пустые значения в `getSingularObjectFromString()` — NPE при создании задачи.
- Тяжёлая логика в `getVelocityParameters()` — вызывается при каждом отображении задачи.
- Изменение `getDatabaseType()` после деплоя — несовместимость с существующими данными.

### Как используется в 2026
- Custom Field Type остаётся мощным инструментом для DC.
- В Cloud кастомные поля создаются через Forge UI Kit или Connect (iframe).
- Тренд на использование JSON-полей (`StringLength.UNLIMITED`) для сложных структур данных вместо множества простых полей.

[к оглавлению](#Разработка-для-Jira)

---

## Как создать Workflow Post-Function, Condition, Validator?

Расширения workflow позволяют добавлять автоматическую логику при переходах между статусами в Jira.

**Три типа расширений:**
- **Post-Function** — действие, выполняемое после перехода (назначить исполнителя, обновить поле, отправить уведомление)
- **Condition** — условие, определяющее видимость перехода (показывать кнопку только определённым пользователям)
- **Validator** — проверка перед выполнением перехода (комментарий обязателен, поле заполнено)

**Post-Function:**

```xml
<!-- atlassian-plugin.xml -->
<workflow-function key="auto-assign-function"
                   class="com.example.plugin.workflow.AutoAssignFunctionFactory">
    <function-class>com.example.plugin.workflow.AutoAssignFunction</function-class>
    <orderable>true</orderable>
    <unique>false</unique>
    <resource name="view" type="velocity"
              location="templates/workflow/auto-assign-view.vm"/>
    <resource name="input-parameters" type="velocity"
              location="templates/workflow/auto-assign-input.vm"/>
</workflow-function>
```

```java
public class AutoAssignFunction implements FunctionProvider {

    private static final Logger log = LoggerFactory.getLogger(AutoAssignFunction.class);
    private static final String FIELD_GROUP_NAME = "groupName";

    @Override
    public void execute(Map transientVars, Map args, PropertySet ps) {
        MutableIssue issue = (MutableIssue) transientVars.get("issue");
        String groupName = (String) args.get(FIELD_GROUP_NAME);

        if (groupName == null || groupName.isEmpty()) {
            log.warn("Имя группы не задано для AutoAssignFunction");
            return;
        }

        GroupManager groupManager = ComponentAccessor.getGroupManager();
        Collection<ApplicationUser> members = groupManager.getUsersInGroup(groupName);

        if (!members.isEmpty()) {
            // Простая стратегия: назначить первого доступного
            ApplicationUser assignee = members.iterator().next();
            issue.setAssignee(assignee);
            log.info("Задача {} назначена на {}", issue.getKey(),
                    assignee.getDisplayName());
        }
    }
}

public class AutoAssignFunctionFactory extends AbstractWorkflowPluginFactory
        implements WorkflowPluginFunctionFactory {

    @Override
    protected void getVelocityParamsForInput(Map<String, Object> velocityParams) {
        velocityParams.put("groupName", "");
    }

    @Override
    protected void getVelocityParamsForEdit(Map<String, Object> velocityParams,
            AbstractDescriptor descriptor) {
        FunctionDescriptor funcDesc = (FunctionDescriptor) descriptor;
        velocityParams.put("groupName", funcDesc.getArgs().get("groupName"));
    }

    @Override
    protected void getVelocityParamsForView(Map<String, Object> velocityParams,
            AbstractDescriptor descriptor) {
        FunctionDescriptor funcDesc = (FunctionDescriptor) descriptor;
        velocityParams.put("groupName", funcDesc.getArgs().get("groupName"));
    }

    @Override
    public Map<String, ?> getDescriptorParams(Map<String, Object> formParams) {
        return Map.of("groupName", extractSingleParam(formParams, "groupName"));
    }
}
```

**Condition:**

```java
public class OnlyReporterCondition extends AbstractJiraCondition {

    @Override
    public boolean passesCondition(Map transientVars, Map args, PropertySet ps)
            throws WorkflowException {
        Issue issue = (Issue) transientVars.get("issue");
        ApplicationUser currentUser = getCallerUser(transientVars, args);

        if (issue == null || currentUser == null) {
            return false;
        }

        return currentUser.equals(issue.getReporter());
    }
}
```

```xml
<workflow-condition key="only-reporter-condition"
                    class="com.example.plugin.workflow.OnlyReporterConditionFactory">
    <condition-class>com.example.plugin.workflow.OnlyReporterCondition</condition-class>
    <resource name="view" type="velocity"
              location="templates/workflow/reporter-condition-view.vm"/>
</workflow-condition>
```

**Validator:**

```java
public class CommentRequiredValidator implements Validator {

    private static final String MIN_LENGTH_PARAM = "minLength";

    @Override
    public void validate(Map transientVars, Map args, PropertySet ps)
            throws InvalidInputException, WorkflowException {

        int minLength = Integer.parseInt(
                (String) args.getOrDefault(MIN_LENGTH_PARAM, "10"));

        IssueInputParameters inputParams =
                (IssueInputParameters) transientVars.get("issueInputParameters");

        String comment = inputParams != null ? inputParams.getComment() : null;

        if (comment == null || comment.trim().length() < minLength) {
            throw new InvalidInputException(
                    "Комментарий обязателен (минимум " + minLength + " символов)");
        }
    }
}
```

```xml
<workflow-validator key="comment-required-validator"
                    class="com.example.plugin.workflow.CommentRequiredValidatorFactory">
    <validator-class>com.example.plugin.workflow.CommentRequiredValidator</validator-class>
    <resource name="view" type="velocity"
              location="templates/workflow/comment-validator-view.vm"/>
    <resource name="input-parameters" type="velocity"
              location="templates/workflow/comment-validator-input.vm"/>
</workflow-validator>
```

### Важное
- Post-Function выполняется **после** перехода — если нужно изменить поля issue, обязательно вызвать `issue.store()` или использовать `IssueManager.updateIssue()`.
- Factory-класс обязателен — он отвечает за UI конфигурации workflow-функции в admin-панели.
- Порядок Post-Functions важен — встроенные функции Jira (Update Issue, Generate Events) должны идти после кастомных.

### Частые ошибки
- Не вызвать `issue.store()` в Post-Function — изменения не сохраняются.
- Бросать RuntimeException в Condition — нужно возвращать `false`, а не падать.
- Тяжёлая логика в Condition — вызывается при каждом отображении задачи для определения доступных переходов.
- Использование `ComponentAccessor` вместо DI — усложняет тестирование (но в workflow-функциях DI ограничен).

### Как используется в 2026
- Workflow-расширения — одна из самых востребованных функций DC-плагинов.
- В Cloud аналог — Forge `jira:workflowCondition`, `jira:workflowValidator`, `jira:workflowPostFunction`.
- ScriptRunner покрывает ~80% типичных workflow-сценариев без написания Java-плагина.

[к оглавлению](#Разработка-для-Jira)

---

## Как работать с Event Listener в плагине?

Event Listener позволяет реагировать на события в Jira: создание/обновление/удаление задач, комментариев, проектов и других сущностей.

**Реализация через аннотацию `@EventListener`:**

```java
@Named
public class IssueEventListener implements InitializingBean, DisposableBean {

    private static final Logger log = LoggerFactory.getLogger(IssueEventListener.class);

    private final EventPublisher eventPublisher;
    private final MailService mailService;

    @Inject
    public IssueEventListener(@ComponentImport EventPublisher eventPublisher,
                              MailService mailService) {
        this.eventPublisher = eventPublisher;
        this.mailService = mailService;
    }

    // Регистрация/дерегистрация слушателя
    @Override
    public void afterPropertiesSet() {
        eventPublisher.register(this);
    }

    @Override
    public void destroy() {
        eventPublisher.unregister(this);
    }

    @EventListener
    public void onIssueCreated(IssueEvent event) {
        if (event.getEventTypeId().equals(EventType.ISSUE_CREATED_ID)) {
            Issue issue = event.getIssue();
            log.info("Создана задача: {} в проекте {}",
                    issue.getKey(), issue.getProjectObject().getKey());

            // Бизнес-логика: уведомление, создание связанных задач и т.д.
            if ("Bug".equals(issue.getIssueType().getName())
                    && "Critical".equals(issue.getPriority().getName())) {
                mailService.notifyTeamLead(issue);
            }
        }
    }

    @EventListener
    public void onIssueUpdated(IssueEvent event) {
        if (event.getEventTypeId().equals(EventType.ISSUE_UPDATED_ID)) {
            Issue issue = event.getIssue();
            ChangeLog changeLog = event.getChangeLog();

            if (changeLog != null) {
                for (ChangeItemBean change : changeLog.getChangeItemBeans()) {
                    if ("status".equals(change.getField())) {
                        log.info("Задача {} сменила статус: {} → {}",
                                issue.getKey(), change.getFromString(),
                                change.getToString());
                    }
                }
            }
        }
    }

    // Типизированные события (Jira 8.x+)
    @EventListener
    public void onIssueAssigned(IssueEvent event) {
        if (event.getEventTypeId().equals(EventType.ISSUE_ASSIGNED_ID)) {
            log.info("Задача {} назначена на {}",
                    event.getIssue().getKey(),
                    event.getIssue().getAssignee().getDisplayName());
        }
    }
}
```

**Кастомные события:**

```java
// Определение кастомного события
public class TaskProcessedEvent {
    private final String issueKey;
    private final String result;
    private final Instant processedAt;

    public TaskProcessedEvent(String issueKey, String result) {
        this.issueKey = issueKey;
        this.result = result;
        this.processedAt = Instant.now();
    }

    // getters
}

// Публикация события
@Named
public class TaskProcessor {

    private final EventPublisher eventPublisher;

    @Inject
    public TaskProcessor(@ComponentImport EventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }

    public void process(String issueKey) {
        // ... обработка ...
        eventPublisher.publish(new TaskProcessedEvent(issueKey, "SUCCESS"));
    }
}

// Подписка на кастомное событие
@EventListener
public void onTaskProcessed(TaskProcessedEvent event) {
    log.info("Задача {} обработана с результатом: {}",
            event.getIssueKey(), event.getResult());
}
```

**Асинхронная обработка:**

```java
@Named
public class AsyncIssueListener implements InitializingBean, DisposableBean {

    private final EventPublisher eventPublisher;
    private final ExecutorService executor;

    @Inject
    public AsyncIssueListener(@ComponentImport EventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
        this.executor = Executors.newFixedThreadPool(4);
    }

    @EventListener
    public void onIssueCreated(IssueEvent event) {
        if (event.getEventTypeId().equals(EventType.ISSUE_CREATED_ID)) {
            // Асинхронная обработка — не блокировать поток Jira
            executor.submit(() -> {
                try {
                    heavyProcessing(event.getIssue());
                } catch (Exception e) {
                    log.error("Ошибка обработки события для {}",
                            event.getIssue().getKey(), e);
                }
            });
        }
    }

    @Override
    public void destroy() {
        eventPublisher.unregister(this);
        executor.shutdown();
    }
}
```

### Важное
- Обязательно вызывайте `eventPublisher.register(this)` при инициализации и `unregister(this)` при уничтожении — иначе утечка памяти.
- Event Listener вызывается **синхронно** в том же потоке, что и операция — тяжёлую логику выносите в отдельный поток.
- `IssueEvent.getEventTypeId()` — числовой ID типа события, сравнивайте с константами из `EventType`.

### Частые ошибки
- Не дерегистрировать слушатель в `destroy()` — при перезагрузке плагина старый слушатель остаётся активным и вызывается дважды.
- Синхронная тяжёлая обработка — замедляет UI Jira для пользователя.
- Изменение issue внутри слушателя без `IssueManager.updateIssue()` — изменения теряются или вызывают рекурсивные события.
- Не обрабатывать исключения — необработанное исключение в listener может сломать операцию пользователя.

### Как используется в 2026
- Event Listener — стандартный механизм реакции на события в DC-плагинах.
- В Cloud аналог — Forge Triggers (`avi:jira:created:issue`).
- Для сложных event-driven сценариев применяют паттерн: Jira Event → Plugin Queue → Async Processor.

[к оглавлению](#Разработка-для-Jira)

---

## Как работает кластеризация в Jira Data Center и как это влияет на плагины?

Jira Data Center — кластерное решение, где несколько нод Jira работают за балансировщиком нагрузки, разделяя общую БД и файловую систему.

**Архитектура DC кластера:**

```
                    ┌──────────────────┐
                    │  Load Balancer   │
                    └───────┬──────────┘
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
         ┌────────┐    ┌────────┐    ┌────────┐
         │ Node 1 │    │ Node 2 │    │ Node 3 │
         │ Jira   │    │ Jira   │    │ Jira   │
         │+Plugin │    │+Plugin │    │+Plugin │
         └───┬────┘    └───┬────┘    └───┬────┘
             │   Hazelcast │             │
             │   Cluster   │             │
             └──────┬──────┘─────────────┘
                    │
              ┌─────┴─────┐
              │ Shared DB  │     ┌─────────────────┐
              │ (Postgres) │     │ Shared FS (NFS)  │
              └────────────┘     │ attachments/     │
                                 │ plugins/         │
                                 └─────────────────┘
```

**Влияние на плагины:**

**1. Stateless-дизайн:**
```java
// НЕПРАВИЛЬНО — состояние в памяти ноды
@Named
public class BadService {
    // Этот кэш локальный для ноды — данные рассинхронизируются!
    private final Map<String, Config> localCache = new ConcurrentHashMap<>();
}

// ПРАВИЛЬНО — данные в общей БД (Active Objects) или распределённом кэше
@Named
public class GoodService {
    private final ActiveObjects ao;
    private final CacheManager cacheManager;

    @Inject
    public GoodService(@ComponentImport ActiveObjects ao,
                       @ComponentImport CacheManager cacheManager) {
        this.ao = ao;
        this.cacheManager = cacheManager;
    }
}
```

**2. Распределённые блокировки (ClusterLockService):**
```java
@Named
public class ScheduledTaskService {

    private final ClusterLockService clusterLockService;

    @Inject
    public ScheduledTaskService(
            @ComponentImport ClusterLockService clusterLockService) {
        this.clusterLockService = clusterLockService;
    }

    public void runExclusiveTask() {
        ClusterLock lock = clusterLockService.getLockForName("my-plugin-daily-task");

        if (lock.tryLock()) {
            try {
                // Этот код выполнится только на одной ноде
                performDailyCleanup();
            } finally {
                lock.unlock();
            }
        } else {
            log.debug("Задача уже выполняется на другой ноде");
        }
    }
}
```

**3. Межнодовые сообщения (ClusterMessageService):**
```java
@Named
public class CacheInvalidationService implements ClusterMessageConsumer,
        InitializingBean, DisposableBean {

    private static final String CHANNEL = "my-plugin-cache-invalidation";
    private final ClusterMessagingService messagingService;
    private final Map<String, Object> localCache = new ConcurrentHashMap<>();

    @Inject
    public CacheInvalidationService(
            @ComponentImport ClusterMessagingService messagingService) {
        this.messagingService = messagingService;
    }

    @Override
    public void afterPropertiesSet() {
        messagingService.registerListener(CHANNEL, this);
    }

    @Override
    public void destroy() {
        messagingService.unregisterListener(CHANNEL, this);
    }

    // Вызывается на других нодах при получении сообщения
    @Override
    public void receive(String channel, String message, String senderId) {
        log.debug("Получено сообщение инвалидации кэша: key={}", message);
        localCache.remove(message);
    }

    // При изменении данных — уведомить все ноды
    public void invalidateCache(String key) {
        localCache.remove(key);
        messagingService.sendRemote(CHANNEL, key);
    }
}
```

**4. Кластерный кэш (через CacheManager):**
```java
@Named
public class ConfigCacheService {

    private final Cache<String, String> configCache;

    @Inject
    public ConfigCacheService(@ComponentImport CacheManager cacheManager) {
        this.configCache = cacheManager.getCache(
                "com.example.plugin.config-cache",
                new ConfigCacheLoader(),
                new CacheSettingsBuilder()
                        .expireAfterWrite(10, TimeUnit.MINUTES)
                        .maxEntries(1000)
                        .build());
    }

    public String getConfig(String key) {
        return configCache.get(key);
    }
}
```

**Чек-лист кластерной совместимости плагина:**

| Требование | Проверка |
|---|---|
| Нет in-memory state | Все данные в БД или distributed cache |
| Нет локальных файлов | Используется shared filesystem или БД |
| Нет java.util.Timer | Используется `SAL PluginScheduler` или `ClusterLockService` |
| Нет статических кэшей | Используется `CacheManager` |
| Scheduled tasks идемпотентны | Безопасно выполнить на любой ноде |
| Plugin settings | Через `SAL PluginSettingsFactory` (хранятся в БД) |

### Важное
- Плагин устанавливается на **все ноды** автоматически через shared filesystem.
- Hazelcast — движок распределённого кэша и messaging в Jira DC.
- `atlassian-data-center-compatible=true` в `plugin-info` — маркер кластерной совместимости.
- Тесты на одной ноде не гарантируют корректную работу в кластере.

### Частые ошибки
- Локальный `ConcurrentHashMap` как кэш — рассинхронизация между нодами.
- `java.util.Timer` или `ScheduledExecutorService` — задача выполняется на каждой ноде одновременно.
- Запись во временные локальные файлы — другая нода их не видит.
- Не тестировать на кластере из 2+ нод перед релизом.

### Как используется в 2026
- Кластерная совместимость — обязательное требование для публикации на Atlassian Marketplace.
- Типичные кластеры: 2-4 ноды для средних компаний, 8+ нод для крупных enterprise.
- Atlassian предоставляет DC Performance Toolkit для нагрузочного тестирования плагинов в кластере.
- Zero-downtime upgrades (rolling upgrades) предъявляют дополнительные требования к обратной совместимости плагинов.

[к оглавлению](#Разработка-для-Jira)

---

## Как тестировать плагины Jira DC?

Тестирование плагинов Jira DC включает несколько уровней: юнит-тесты, интеграционные тесты в контейнере Jira, UI-тесты.

**Юнит-тесты (Mockito):**

```java
public class TaskConfigServiceTest {

    @Mock
    private ActiveObjects ao;

    @Mock
    private TaskConfig mockConfig;

    private TaskConfigService service;

    @Before
    public void setUp() {
        MockitoAnnotations.openMocks(this);
        service = new TaskConfigService(ao);
    }

    @Test
    public void testFindByProject() {
        when(mockConfig.getProjectKey()).thenReturn("PROJ");
        when(mockConfig.isEnabled()).thenReturn(true);
        when(ao.find(eq(TaskConfig.class), any(Query.class)))
                .thenReturn(new TaskConfig[]{mockConfig});

        TaskConfig[] results = service.findByProject("PROJ");

        assertEquals(1, results.length);
        assertEquals("PROJ", results[0].getProjectKey());
        assertTrue(results[0].isEnabled());
    }

    @Test
    public void testCreateConfig() {
        when(ao.create(TaskConfig.class)).thenReturn(mockConfig);
        when(ao.executeInTransaction(any())).thenAnswer(invocation -> {
            TransactionCallback<?> callback = invocation.getArgument(0);
            return callback.doInTransaction();
        });

        service.create("PROJ", "{\"key\":\"value\"}");

        verify(mockConfig).setProjectKey("PROJ");
        verify(mockConfig).setConfiguration("{\"key\":\"value\"}");
        verify(mockConfig).save();
    }
}
```

**Запуск юнит-тестов:**
```bash
atlas-unit-test
# или стандартный Maven
atlas-mvn test
```

**Интеграционные тесты (@Wired):**

```java
@RunWith(AtlassianPluginsTestRunner.class)
public class TaskConfigServiceWiredTest {

    @Inject
    private TaskConfigService configService;

    @Inject
    private ActiveObjects ao;

    @Before
    public void setUp() {
        // AO будет использовать тестовую БД Jira
    }

    @Test
    public void testCreateAndFindConfig() {
        TaskConfig created = configService.create("TEST", "{\"test\":true}");
        assertNotNull(created);

        TaskConfig[] found = configService.findByProject("TEST");
        assertEquals(1, found.length);
        assertEquals("TEST", found[0].getProjectKey());
    }
}
```

**Запуск интеграционных тестов:**
```bash
atlas-integration-test
# Запускает реальный инстанс Jira, деплоит плагин, выполняет тесты
```

**TestKit для REST API тестирования:**

```java
public class ConfigResourceFuncTest {

    private static final String BASE_URL = "http://localhost:2990/jira";
    private static final String REST_PATH = "/rest/myapi/1.0/config";

    @Test
    public void testGetConfig() {
        Response response = RestAssured.given()
                .auth().preemptive().basic("admin", "admin")
                .contentType(ContentType.JSON)
                .get(BASE_URL + REST_PATH + "/1");

        assertEquals(200, response.getStatusCode());
        assertNotNull(response.jsonPath().getString("projectKey"));
    }

    @Test
    public void testCreateConfigRequiresAdmin() {
        Response response = RestAssured.given()
                .auth().preemptive().basic("user", "user")
                .contentType(ContentType.JSON)
                .body("{\"projectKey\":\"PROJ\",\"configuration\":\"{}\"}")
                .post(BASE_URL + REST_PATH);

        assertEquals(403, response.getStatusCode());
    }
}
```

**UI тесты (Page Objects):**

```java
public class ConfigPageTest extends AbstractJiraFuncTest {

    @Test
    public void testConfigPageLoads() {
        jira.gotoLoginPage().loginAsSysAdmin(SysAdminPage.class);

        ConfigPage configPage = jira.goTo(ConfigPage.class);
        assertTrue(configPage.isLoaded());
        assertEquals("Настройки плагина", configPage.getTitle());
    }
}

public class ConfigPage extends AbstractJiraPage {

    @ElementBy(id = "config-form")
    private PageElement configForm;

    @Override
    public String getUrl() {
        return "/plugins/servlet/my-plugin/config";
    }

    @Override
    public TimedCondition isAt() {
        return configForm.timed().isPresent();
    }

    public String getTitle() {
        return find(By.tagName("h2")).getText();
    }
}
```

### Важное
- Юнит-тесты — обязательный минимум. Mock-айте все Jira-сервисы через Mockito.
- Интеграционные тесты медленные (запуск Jira), но ловят проблемы OSGi-совместимости.
- Используйте `atlas-debug` + IDE breakpoints для отладки интеграционных тестов.

### Частые ошибки
- Тестирование только через UI (ручное) — не масштабируется, регрессии не ловятся.
- Мокирование `ComponentAccessor` — лучше использовать DI и мокировать инжектированные сервисы.
- Не тестировать на версии Jira, близкой к production — API может отличаться.
- Не тестировать upgrade tasks — миграция данных ломается при обновлении плагина.

### Как используется в 2026
- Atlassian DC Performance Toolkit — для нагрузочного тестирования в кластерном окружении.
- Docker-based тестовые окружения стали стандартом (вместо atlas-integration-test для CI/CD).
- Рекомендуемое покрытие: 70%+ юнит-тестами, ключевые сценарии — интеграционными.
- Тестирование обратной совместимости при rolling upgrades — требование для DC Marketplace.

[к оглавлению](#Разработка-для-Jira)

---

## Как отлаживать и профилировать плагины?

Отладка и профилирование — критически важные навыки при разработке плагинов Jira DC. Плагин работает внутри JVM Jira, что создаёт специфические вызовы.

**Remote Debug:**

```bash
# Запуск Jira с включённым debug-портом
atlas-debug
# Jira стартует с: -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005

# В IDE (IntelliJ IDEA):
# Run → Edit Configurations → Remote JVM Debug
# Host: localhost, Port: 5005
# Подключиться после старта Jira
```

**Логирование (SLF4J):**

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Named
public class MyService {
    // Используйте SLF4J — он предоставляется Jira
    private static final Logger log = LoggerFactory.getLogger(MyService.class);

    public void process(String key) {
        log.debug("Начало обработки: {}", key);
        try {
            // ... логика ...
            log.info("Обработка завершена: {}", key);
        } catch (Exception e) {
            log.error("Ошибка обработки {}: {}", key, e.getMessage(), e);
        }
    }
}
```

**Настройка уровня логирования в runtime:**
```
# Через UI: Administration → System → Logging and Profiling
# Через REST API:
PUT /rest/api/2/admin/log/com.example.plugin
Content-Type: application/json
{"level": "DEBUG"}
```

**Управление плагинами через REST:**

```bash
# Список всех плагинов
GET /rest/plugins/1.0/

# Информация о конкретном плагине
GET /rest/plugins/1.0/com.example.my-plugin-key

# Отключить плагин
PUT /rest/plugins/1.0/com.example.my-plugin-key
Content-Type: application/json
{"enabled": false}

# Удалить плагин
DELETE /rest/plugins/1.0/com.example.my-plugin-key-installed

# Установить плагин (загрузить JAR)
POST /rest/plugins/1.0/?token=...
Content-Type: application/octet-stream
[binary JAR content]
```

**Профилирование и диагностика:**

```bash
# JVM flags для диагностики (в setenv.sh / setenv.bat)
CATALINA_OPTS="-Xmx4g -Xms2g"
CATALINA_OPTS="$CATALINA_OPTS -XX:+HeapDumpOnOutOfMemoryError"
CATALINA_OPTS="$CATALINA_OPTS -XX:HeapDumpPath=/var/atlassian/jira/heapdumps"
CATALINA_OPTS="$CATALINA_OPTS -verbose:gc -XX:+PrintGCDetails"

# Thread dump (для анализа deadlocks, thread starvation)
kill -3 <jira_pid>  # На Linux — вывод в stdout
jstack <jira_pid> > thread_dump.txt

# Heap dump
jmap -dump:live,format=b,file=heap.hprof <jira_pid>
# Анализ: Eclipse MAT, VisualVM, IntelliJ Profiler
```

**Developer Toolbox (плагин Atlassian):**
- Установите `atlassian-developer-toolbox` для дополнительных диагностических возможностей
- SQL-запросы к БД Jira
- Просмотр OSGi-бандлов и сервисов
- Анализ зависимостей между плагинами

**Типичные проблемы и диагностика:**

| Проблема | Симптом | Диагностика |
|---|---|---|
| Memory leak | OOM после нескольких дней работы | Heap dump → MAT → Dominator tree |
| Thread leak | Увеличение числа потоков, зависание | Thread dump → ищем потоки плагина |
| Slow query | Медленные страницы | SQL logging → анализ запросов AO |
| ClassNotFoundException | Ошибка при загрузке плагина | Проверить Import-Package в MANIFEST.MF |
| Plugin не стартует | Статус "Disabled" в UPM | Логи: `atlassian-jira.log` → ошибки OSGi |

**Анализ производительности AO-запросов:**

```java
// Включить SQL-логирование для Active Objects
// log4j.logger.net.java.ao.sql=DEBUG

// Или программно:
@Named
public class DiagnosticService {

    public void diagnoseSlowQuery() {
        long start = System.nanoTime();
        TaskConfig[] results = ao.find(TaskConfig.class,
                Query.select().where("PROJECT_KEY = ?", "PROJ"));
        long duration = TimeUnit.NANOSECONDS.toMillis(System.nanoTime() - start);

        if (duration > 100) {
            log.warn("Медленный AO-запрос: {}ms, результатов: {}",
                    duration, results.length);
        }
    }
}
```

### Важное
- `atlas-debug` — основной инструмент повседневной разработки. Настройте IDE для автоматического подключения.
- Логи Jira: `<JIRA_HOME>/log/atlassian-jira.log` — основной лог, `atlassian-jira-security.log` — события безопасности.
- Thread dump — первый инструмент при зависании Jira; Heap dump — при OOM.

### Частые ошибки
- Оставить уровень DEBUG в production — логи переполняют диск.
- Использование `System.out.println()` вместо SLF4J — вывод теряется или идёт в неправильный файл.
- Не делать heap dump при первом OOM — проблема воспроизводится с трудом.
- Профилирование на dev-окружении с маленьким датасетом — не отражает production-нагрузку.

### Как используется в 2026
- JFR (Java Flight Recorder) — встроенный профайлер JVM, zero-overhead в production.
- Atlassian DC Performance Toolkit интегрирует JMeter + JFR для end-to-end профилирования.
- Observability: Prometheus + Grafana для мониторинга метрик плагинов в production.
- Distributed tracing (OpenTelemetry) начинает применяться для DC-кластеров.

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Forge и как создать Forge-приложение?

**Forge** — serverless-платформа Atlassian для разработки Cloud-приложений. Код выполняется в управляемой Atlassian среде (FaaS), что обеспечивает безопасность и простоту развёртывания.

**Ключевые характеристики:**
- Runtime: Node.js (TypeScript/JavaScript)
- Хостинг: Atlassian (не нужен свой сервер)
- Изоляция: каждое приложение работает в sandbox
- Ограничения: 25 секунд timeout, 512MB RAM на invocation

**Создание Forge-приложения:**

```bash
# Установка Forge CLI
npm install -g @forge/cli

# Авторизация
forge login

# Создание приложения из шаблона
forge create
# Выбрать: Jira → jira-issue-panel (или другой шаблон)

# Структура проекта:
my-forge-app/
├── manifest.yml          # Дескриптор приложения
├── src/
│   ├── index.tsx         # Точка входа (UI Kit)
│   └── resolvers/
│       └── index.ts      # Backend-функции (resolvers)
├── package.json
├── tsconfig.json
└── static/               # Для Custom UI (React-приложение)
    └── hello-world/
        ├── src/
        │   └── App.tsx
        └── package.json
```

**manifest.yml:**

```yaml
modules:
  jira:issuePanel:
    - key: my-issue-panel
      title: Дополнительная информация
      resource: main
      resolver:
        function: resolver
      icon: https://example.com/icon.svg

  jira:globalPage:
    - key: my-global-page
      title: Моя страница
      resource: globalPage
      resolver:
        function: resolver

  jira:workflowPostFunction:
    - key: my-post-function
      name: Автоназначение
      function: workflowFunction
      description: Автоматически назначает задачу

  function:
    - key: resolver
      handler: index.handler
    - key: workflowFunction
      handler: workflow.handler

  trigger:
    - key: issue-created-trigger
      function: onIssueCreated
      events:
        - avi:jira:created:issue
      filter:
        project: PROJ

resources:
  - key: main
    path: src/frontend/index.jsx
  - key: globalPage
    path: static/hello-world/build

permissions:
  scopes:
    - read:jira-work
    - write:jira-work
    - read:jira-user
  external:
    fetch:
      backend:
        - https://api.example.com

app:
  runtime:
    name: nodejs18.x
  id: ari:cloud:ecosystem::app/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

**Backend (Resolver):**

```typescript
// src/resolvers/index.ts
import Resolver from '@forge/resolver';
import api, { route, fetch } from '@forge/api';
import { storage } from '@forge/api';

const resolver = new Resolver();

// Получение данных задачи
resolver.define('getIssueDetails', async ({ payload, context }) => {
    const issueKey = context.extension.issue.key;

    const response = await api.asUser().requestJira(
        route`/rest/api/3/issue/${issueKey}`,
        { headers: { 'Accept': 'application/json' } }
    );

    const data = await response.json();
    return {
        summary: data.fields.summary,
        status: data.fields.status.name,
        assignee: data.fields.assignee?.displayName || 'Не назначен'
    };
});

// Сохранение данных в Storage
resolver.define('saveConfig', async ({ payload }) => {
    const { key, value } = payload;
    await storage.set(key, value);
    return { success: true };
});

// Получение данных из Storage
resolver.define('getConfig', async ({ payload }) => {
    const { key } = payload;
    const value = await storage.get(key);
    return { value };
});

export const handler = resolver.getDefinitions();
```

**Frontend (UI Kit):**

```typescript
// src/frontend/index.tsx
import ForgeUI, {
    render,
    IssuePanel,
    Fragment,
    Text,
    Button,
    useState,
    useEffect,
} from '@forge/ui';
import { invoke } from '@forge/bridge';

const Panel = () => {
    const [details, setDetails] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(async () => {
        const data = await invoke('getIssueDetails');
        setDetails(data);
        setLoading(false);
    }, []);

    if (loading) return <Text>Загрузка...</Text>;

    return (
        <Fragment>
            <Text>Статус: {details.status}</Text>
            <Text>Исполнитель: {details.assignee}</Text>
            <Button
                text="Обновить"
                onClick={async () => {
                    setLoading(true);
                    const data = await invoke('getIssueDetails');
                    setDetails(data);
                    setLoading(false);
                }}
            />
        </Fragment>
    );
};

export const run = render(
    <IssuePanel>
        <Panel />
    </IssuePanel>
);
```

**Trigger (обработка событий):**

```typescript
// src/triggers/index.ts
import api, { route } from '@forge/api';

export async function handler(event: any, context: any) {
    const { issue } = event;
    console.log(`Создана задача: ${issue.key}`);

    // Добавить комментарий к задаче
    await api.asApp().requestJira(
        route`/rest/api/3/issue/${issue.key}/comment`,
        {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                body: {
                    type: 'doc',
                    version: 1,
                    content: [{
                        type: 'paragraph',
                        content: [{
                            type: 'text',
                            text: 'Задача зарегистрирована автоматически.'
                        }]
                    }]
                }
            })
        }
    );
}
```

**Деплой и установка:**

```bash
forge deploy          # Деплой кода в Atlassian Cloud
forge install         # Установка на конкретный Jira Cloud site
forge install --upgrade  # Обновление установки после изменения scopes
forge tunnel          # Режим разработки (проксирование на локальный код)
forge logs            # Просмотр логов приложения
```

### Важное
- Forge — рекомендуемый Atlassian фреймворк для новых Cloud-приложений.
- Два варианта UI: **UI Kit** (декларативный, ограниченный) и **Custom UI** (React, полная свобода).
- `api.asUser()` — выполнить запрос от имени текущего пользователя, `api.asApp()` — от имени приложения.
- `forge tunnel` — незаменим при разработке, позволяет тестировать локальный код в реальном Jira Cloud.

### Частые ошибки
- Превышение лимита 25 секунд на invocation — тяжёлую логику нужно разбивать на части или использовать async events.
- Не указать необходимые scopes в `manifest.yml` — API-вызовы будут возвращать 403.
- Использование `console.log` для отладки в production — логи доступны через `forge logs`, но ограничены.
- Не обработать ошибки в resolver — пользователь видит generic error message.

### Как используется в 2026
- Forge стал зрелой платформой: Custom UI, Storage API, Entity Storage, Async Events, Scheduled Triggers.
- Node.js 18+ — текущий runtime.
- UI Kit 2 — актуальная версия с улучшенным набором компонентов.
- Forge поддерживает multi-product apps (Jira + Confluence + Bitbucket).

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Connect и чем он отличается от Forge?

**Atlassian Connect** — фреймворк для разработки Cloud-приложений, где приложение хостится на собственном сервере разработчика и интегрируется с Jira через iframe и REST API.

**Архитектура Connect:**

```
┌──────────────────┐              ┌──────────────────┐
│   Jira Cloud     │   iframe     │  Connect App     │
│                  │ ←──────────→ │  (ваш сервер)    │
│  ┌────────────┐  │              │                  │
│  │ iframe     │  │   REST API   │  Spring Boot /   │
│  │ (app UI)   │  │ ←──────────→ │  Node.js / ...   │
│  └────────────┘  │              │                  │
│                  │   Webhooks   │  PostgreSQL /     │
│  JWT Auth        │ ────────────→│  MongoDB / ...    │
└──────────────────┘              └──────────────────┘
```

**atlassian-connect.json (дескриптор):**

```json
{
    "key": "my-connect-app",
    "name": "My Connect Application",
    "description": "Описание приложения",
    "vendor": {
        "name": "Example Corp",
        "url": "https://example.com"
    },
    "baseUrl": "https://my-app.example.com",
    "authentication": {
        "type": "jwt"
    },
    "lifecycle": {
        "installed": "/api/lifecycle/installed",
        "uninstalled": "/api/lifecycle/uninstalled",
        "enabled": "/api/lifecycle/enabled",
        "disabled": "/api/lifecycle/disabled"
    },
    "scopes": ["READ", "WRITE"],
    "modules": {
        "generalPages": [
            {
                "key": "config-page",
                "name": {"value": "Настройки приложения"},
                "url": "/pages/config?projectKey={project.key}",
                "location": "system.top.navigation.bar",
                "conditions": [
                    {"condition": "user_is_admin"}
                ]
            }
        ],
        "jiraIssueTabPanels": [
            {
                "key": "extra-info-tab",
                "name": {"value": "Доп. информация"},
                "url": "/panels/issue-tab?issueKey={issue.key}"
            }
        ],
        "webhooks": [
            {
                "event": "jira:issue_created",
                "url": "/api/webhooks/issue-created",
                "filter": "project = PROJ"
            }
        ],
        "webPanels": [
            {
                "key": "right-panel",
                "name": {"value": "Статистика"},
                "url": "/panels/right-panel?issueKey={issue.key}",
                "location": "atl.jira.view.issue.right.context"
            }
        ]
    }
}
```

**Lifecycle callbacks (Spring Boot):**

```java
@RestController
@RequestMapping("/api/lifecycle")
public class LifecycleController {

    private final TenantService tenantService;

    @PostMapping("/installed")
    public ResponseEntity<Void> installed(@RequestBody InstallPayload payload) {
        // Сохранить информацию о tenant (Jira site)
        tenantService.register(
                payload.getClientKey(),
                payload.getSharedSecret(),
                payload.getBaseUrl(),
                payload.getProductType()
        );
        log.info("Приложение установлено на: {}", payload.getBaseUrl());
        return ResponseEntity.ok().build();
    }

    @PostMapping("/uninstalled")
    public ResponseEntity<Void> uninstalled(@RequestBody UninstallPayload payload) {
        tenantService.unregister(payload.getClientKey());
        return ResponseEntity.ok().build();
    }
}

@Data
public class InstallPayload {
    private String key;
    private String clientKey;      // Уникальный ID инстанса Jira
    private String sharedSecret;   // Для JWT-аутентификации
    private String baseUrl;        // URL инстанса Jira
    private String productType;
}
```

**JWT-аутентификация:**

```java
@Component
public class JwtAuthFilter extends OncePerRequestFilter {

    private final TenantService tenantService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {

        String jwt = extractJwt(request);
        if (jwt == null) {
            response.sendError(401, "Missing JWT");
            return;
        }

        try {
            DecodedJWT decoded = JWT.decode(jwt);
            String clientKey = decoded.getIssuer();
            Tenant tenant = tenantService.getByClientKey(clientKey);

            // Верификация JWT с shared secret
            Algorithm algorithm = Algorithm.HMAC256(tenant.getSharedSecret());
            JWTVerifier verifier = JWT.require(algorithm)
                    .withIssuer(clientKey)
                    .build();
            verifier.verify(jwt);

            // Установить tenant в контекст запроса
            request.setAttribute("tenant", tenant);
            chain.doFilter(request, response);
        } catch (JWTVerificationException e) {
            response.sendError(401, "Invalid JWT: " + e.getMessage());
        }
    }

    private String extractJwt(HttpServletRequest request) {
        // JWT может быть в query parameter или Authorization header
        String jwtParam = request.getParameter("jwt");
        if (jwtParam != null) return jwtParam;

        String authHeader = request.getHeader("Authorization");
        if (authHeader != null && authHeader.startsWith("JWT ")) {
            return authHeader.substring(4);
        }
        return null;
    }
}
```

**Сравнение Forge vs Connect:**

| Аспект | Forge | Connect |
|---|---|---|
| Хостинг | Atlassian (serverless) | Свой сервер |
| Язык | TypeScript/JavaScript | Любой |
| БД | Forge Storage (KV, 32KB) | Любая (PostgreSQL, etc.) |
| UI | UI Kit / Custom UI (React) | iframe (любой фреймворк) |
| Аутентификация | Автоматическая | JWT (ручная реализация) |
| Timeout | 25 секунд | Нет (свой сервер) |
| Сложность деплоя | Минимальная (forge deploy) | Высокая (свой CI/CD) |
| Контроль | Ограниченный | Полный |
| Стоимость инфраструктуры | Бесплатно (до лимитов) | Свои серверы |
| Доступ к данным | Через API | Через API |
| Поддержка | Atlassian | Собственная |

### Важное
- Connect — правильный выбор, когда нужен полный контроль над стеком, собственная БД, тяжёлые вычисления или интеграция с внутренними системами.
- Мультитенантность — Connect-приложение обслуживает множество Jira-инстансов, нужно правильно изолировать данные по `clientKey`.
- JWT shared secret — критически важный секрет, хранить в Vault/KMS.

### Частые ошибки
- Не реализовать lifecycle callbacks — приложение не знает о новых/удалённых инстансах.
- Хранить shared secret в plain text — компрометация даёт полный доступ к Jira-инстансу.
- Не обрабатывать мультитенантность — данные одного клиента утекают другому.
- Не проверять JWT в каждом запросе — открытый API без аутентификации.
- iframe не адаптивный — плохой UX на разных размерах экрана.

### Как используется в 2026
- Connect остаётся востребованным для enterprise-приложений с собственной инфраструктурой.
- Atlassian не deprecated Connect, но рекомендует Forge для новых приложений.
- Популярный стек: Spring Boot + Connect + PostgreSQL + React (в iframe).
- Миграция Connect → Forge возможна, но трудоёмка из-за различий в архитектуре.

[к оглавлению](#Разработка-для-Jira)

---

## Как работает аутентификация и авторизация в Jira Cloud приложениях?

Аутентификация и авторизация в Jira Cloud зависят от типа приложения (Forge, Connect) и сценария использования.

**OAuth 2.0 (3-legged) — для user context:**

```
┌──────┐     1. Redirect        ┌──────────┐
│ User │ ──────────────────────→ │ Atlassian│
│      │                         │ Auth     │
│      │     2. Login + Consent  │ Server   │
│      │ ←──────────────────── → │          │
│      │                         └────┬─────┘
│      │                              │
│      │     3. Callback + code       │
│      │ ←────────────────────────────┘
│      │
│      │     4. Exchange code → token
│      │ ──────────────────────────────→ Atlassian
│      │     5. Access token + Refresh
│      │ ←──────────────────────────────
└──────┘
```

```java
// Spring Boot OAuth 2.0 клиент для Jira Cloud
@Configuration
public class JiraOAuth2Config {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
                .oauth2Client(Customizer.withDefaults())
                .build();
    }

    @Bean
    public ClientRegistration jiraClientRegistration(
            @Value("${jira.oauth2.client-id}") String clientId,
            @Value("${jira.oauth2.client-secret}") String clientSecret) {
        return ClientRegistration.withRegistrationId("jira")
                .clientId(clientId)
                .clientSecret(clientSecret)
                .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
                .redirectUri("{baseUrl}/callback")
                .authorizationUri("https://auth.atlassian.com/authorize")
                .tokenUri("https://auth.atlassian.com/oauth/token")
                .scope("read:jira-work", "write:jira-work", "read:jira-user")
                .build();
    }
}
```

**OAuth 2.0 (2-legged) — для app context (без пользователя):**

```java
// Получение токена приложения
public String getAppAccessToken(String clientId, String clientSecret) {
    RestClient authClient = RestClient.create();

    Map<String, String> body = Map.of(
            "grant_type", "client_credentials",
            "client_id", clientId,
            "client_secret", clientSecret
    );

    Map<String, Object> response = authClient.post()
            .uri("https://auth.atlassian.com/oauth/token")
            .contentType(MediaType.APPLICATION_JSON)
            .body(body)
            .retrieve()
            .body(new ParameterizedTypeReference<>() {});

    return (String) response.get("access_token");
}
```

**API Tokens (для простых интеграций):**

```java
// Аутентификация через API Token (email + token)
RestClient restClient = RestClient.builder()
        .baseUrl("https://your-domain.atlassian.net/rest/api/3")
        .defaultHeader("Authorization", "Basic " +
                Base64.getEncoder().encodeToString(
                        (email + ":" + apiToken).getBytes()))
        .build();
```

**Forge — автоматическая аутентификация:**

```typescript
import api, { route } from '@forge/api';

// Запрос от имени пользователя (используются scopes из manifest.yml)
const userResponse = await api.asUser().requestJira(
    route`/rest/api/3/myself`
);

// Запрос от имени приложения
const appResponse = await api.asApp().requestJira(
    route`/rest/api/3/issue/PROJ-123`
);
```

**Connect — JWT аутентификация:**

```java
// Создание JWT для вызова Jira API из Connect-приложения
public String createJwtToken(String method, String path,
                              String clientKey, String sharedSecret) {
    long now = System.currentTimeMillis() / 1000;

    String canonicalRequest = method.toUpperCase() + "&" + path + "&";
    String qsh = Hashing.sha256()
            .hashString(canonicalRequest, StandardCharsets.UTF_8)
            .toString();

    return JWT.create()
            .withIssuer(clientKey)
            .withIssuedAt(new Date(now * 1000))
            .withExpiresAt(new Date((now + 180) * 1000))
            .withClaim("qsh", qsh)
            .sign(Algorithm.HMAC256(sharedSecret));
}

// Использование
String jwt = createJwtToken("GET", "/rest/api/3/issue/PROJ-123",
        tenant.getClientKey(), tenant.getSharedSecret());

restClient.get()
        .uri(tenant.getBaseUrl() + "/rest/api/3/issue/PROJ-123")
        .header("Authorization", "JWT " + jwt)
        .retrieve()
        .body(Map.class);
```

**Scopes (области доступа):**

| Scope | Описание |
|---|---|
| `read:jira-work` | Чтение задач, проектов, досок |
| `write:jira-work` | Создание/обновление задач |
| `read:jira-user` | Чтение профилей пользователей |
| `manage:jira-project` | Управление проектами |
| `manage:jira-configuration` | Управление конфигурацией |
| `manage:jira-data-provider` | Управление провайдерами данных |

### Важное
- OAuth 2.0 (3LO) — обязательный для операций от имени пользователя в Cloud.
- API Tokens подходят для скриптов и простых интеграций, но не для production-приложений.
- В Forge аутентификация абстрагирована — разработчик только указывает scopes.
- Refresh tokens нужно обновлять до истечения срока (обычно 90 дней).

### Частые ошибки
- Использование API Token в серверном приложении для нескольких пользователей — все действия выполняются от одного пользователя.
- Не обновлять refresh token — пользователю приходится переавторизовываться.
- Запрос избыточных scopes — Atlassian может отклонить приложение при review.
- Хранение client secret и shared secret в коде — используйте переменные окружения.

### Как используется в 2026
- OAuth 2.0 — единственный рекомендуемый метод аутентификации для Cloud-приложений.
- Basic Auth полностью deprecated для Cloud (работает только с API tokens).
- Atlassian вводит granular scopes (fine-grained permissions) для более точного контроля доступа.
- PKCE (Proof Key for Code Exchange) рекомендуется для public-клиентов (SPA, мобильные).

[к оглавлению](#Разработка-для-Jira)

---

## Что такое Forge Storage API и Custom UI?

**Forge Storage API** — встроенное хранилище данных для Forge-приложений. Позволяет сохранять данные без собственной БД.

**Типы хранилища:**

| Тип | Лимит | Назначение |
|---|---|---|
| Key-Value (storage) | 32 KB на ключ | Конфигурация, маленькие данные |
| Entity Storage | 32 KB на entity, but queryable | Структурированные данные с поиском |
| Secrets | 4 KB | Токены, пароли |

**Key-Value Storage:**

```typescript
import { storage } from '@forge/api';

// Сохранить
await storage.set('config:PROJ', {
    enabled: true,
    notifyChannel: '#dev-team',
    maxRetries: 3
});

// Получить
const config = await storage.get('config:PROJ');
console.log(config.enabled); // true

// Удалить
await storage.delete('config:PROJ');

// Список ключей (с пагинацией)
const result = await storage.query()
    .where('key', startsWith('config:'))
    .limit(20)
    .getMany();

for (const { key, value } of result.results) {
    console.log(`${key}: ${JSON.stringify(value)}`);
}
```

**Entity Storage (структурированное хранилище):**

```typescript
import { storage } from '@forge/api';

// Определение "индексов" для поиска
// В manifest.yml:
// storage:
//   entities:
//     - name: task-record
//       attributes:
//         projectKey:
//           type: string
//         status:
//           type: string
//         score:
//           type: integer
//         createdAt:
//           type: integer

// Сохранение entity
await storage.entity('task-record').set('task-001', {
    projectKey: 'PROJ',
    status: 'active',
    score: 85,
    createdAt: Date.now(),
    details: { assignee: 'user-123', tags: ['backend', 'critical'] }
});

// Поиск по атрибутам
const activeTasks = await storage.entity('task-record')
    .query()
    .index('projectKey')
    .where('projectKey', 'PROJ')
    .sort('createdAt', 'DESC')
    .limit(50)
    .getMany();

// Фильтрация по нескольким условиям
const highScoreTasks = await storage.entity('task-record')
    .query()
    .index('status')
    .where('status', 'active')
    .filter('score', greaterThan(80))
    .getMany();
```

**Secrets Storage:**

```typescript
import { storage } from '@forge/api';

// Сохранить секрет (зашифрован at rest)
await storage.setSecret('external-api-key', 'sk-xxxxxxxxxxxx');

// Получить секрет
const apiKey = await storage.getSecret('external-api-key');
```

---

**Custom UI** — возможность использовать полноценное React-приложение вместо декларативного UI Kit.

**Структура Custom UI:**

```
my-forge-app/
├── manifest.yml
├── src/
│   └── resolvers/
│       └── index.ts          # Backend (resolvers)
└── static/
    └── my-custom-ui/
        ├── package.json
        ├── src/
        │   ├── App.tsx       # React-приложение
        │   ├── index.tsx
        │   └── components/
        │       ├── IssuePanel.tsx
        │       └── ConfigForm.tsx
        └── build/            # Собранный React-app
```

**manifest.yml для Custom UI:**

```yaml
modules:
  jira:issuePanel:
    - key: custom-panel
      title: Custom Panel
      resource: customUI
      resolver:
        function: resolver

  function:
    - key: resolver
      handler: resolvers/index.handler

resources:
  - key: customUI
    path: static/my-custom-ui/build
```

**React-компонент (Custom UI):**

```typescript
// static/my-custom-ui/src/App.tsx
import React, { useEffect, useState } from 'react';
import { invoke, view } from '@forge/bridge';
import Button from '@atlaskit/button';
import Spinner from '@atlaskit/spinner';
import SectionMessage from '@atlaskit/section-message';

interface IssueDetails {
    summary: string;
    status: string;
    score: number;
}

const App: React.FC = () => {
    const [details, setDetails] = useState<IssueDetails | null>(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        loadDetails();
    }, []);

    const loadDetails = async () => {
        try {
            setLoading(true);
            // Вызов resolver (backend)
            const data = await invoke<IssueDetails>('getIssueDetails');
            setDetails(data);
        } catch (e) {
            setError('Не удалось загрузить данные');
        } finally {
            setLoading(false);
        }
    };

    const handleSave = async () => {
        if (!details) return;
        try {
            await invoke('saveScore', { score: details.score + 1 });
            await loadDetails();
        } catch (e) {
            setError('Ошибка сохранения');
        }
    };

    if (loading) return <Spinner size="large" />;
    if (error) return (
        <SectionMessage appearance="error">{error}</SectionMessage>
    );

    return (
        <div style={{ padding: '16px' }}>
            <h3>{details!.summary}</h3>
            <p>Статус: {details!.status}</p>
            <p>Score: {details!.score}</p>
            <Button appearance="primary" onClick={handleSave}>
                Увеличить Score
            </Button>
        </div>
    );
};

export default App;
```

**Bridge API (@forge/bridge):**

```typescript
import { invoke, view, router, Modal } from '@forge/bridge';

// Вызов backend resolver
const result = await invoke('functionName', { param1: 'value' });

// Получение контекста (issueKey, projectKey и т.д.)
const context = await view.getContext();
const issueKey = context.extension.issue.key;

// Навигация
await router.navigate('/browse/PROJ-123');
await router.open('https://external-site.com');

// Модальное окно
const modal = new Modal({
    resource: 'modal-resource',
    onClose: (payload) => console.log('Modal closed', payload),
    size: 'large'
});
modal.open();
```

### Важное
- Custom UI даёт полную свободу дизайна, но увеличивает размер бандла и сложность разработки.
- UI Kit — проще и быстрее для стандартных сценариев (таблицы, формы, панели).
- Storage API ограничен 32KB на ключ/entity — для больших данных используйте внешнюю БД через `fetch`.
- `@atlaskit/*` — библиотека UI-компонентов Atlassian, рекомендуется для единообразного дизайна.

### Частые ошибки
- Хранение большого объёма данных в Storage (>32KB) — ошибка при записи.
- Не использовать Entity Storage для данных, по которым нужен поиск — Key-Value не поддерживает query.
- Не собирать Custom UI перед `forge deploy` — забыть `npm run build` в static/.
- Прямые вызовы Jira REST API из Custom UI (frontend) — нужно через resolver (backend).

### Как используется в 2026
- Entity Storage стал основным способом хранения данных в Forge.
- Custom UI + Atlaskit — стандарт для сложных UI.
- Forge Remote Backend — новая функция, позволяющая вызывать внешние серверы из Forge.
- Storage API получил улучшенные лимиты и возможности запросов.

[к оглавлению](#Разработка-для-Jira)

---

## Как обрабатывать события в Jira Cloud?

Обработка событий в Jira Cloud отличается от DC. Основные механизмы: **Forge Triggers** и **Connect Webhooks**.

**Forge Triggers:**

```yaml
# manifest.yml
modules:
  trigger:
    - key: on-issue-created
      function: issueCreatedHandler
      events:
        - avi:jira:created:issue

    - key: on-issue-updated
      function: issueUpdatedHandler
      events:
        - avi:jira:updated:issue

    - key: on-comment-created
      function: commentHandler
      events:
        - avi:jira:commented:issue

    - key: scheduled-task
      function: scheduledHandler
      interval: hour  # minute, hour, day, week

  function:
    - key: issueCreatedHandler
      handler: triggers.onIssueCreated
    - key: issueUpdatedHandler
      handler: triggers.onIssueUpdated
    - key: commentHandler
      handler: triggers.onCommentCreated
    - key: scheduledHandler
      handler: triggers.onScheduled
```

```typescript
// src/triggers.ts
import api, { route } from '@forge/api';
import { storage } from '@forge/api';

export async function onIssueCreated(event: any, context: any) {
    const { issue } = event;
    console.log(`Issue created: ${issue.key}`);
    console.log(`Project: ${issue.fields.project.key}`);
    console.log(`Type: ${issue.fields.issuetype.name}`);
    console.log(`Reporter: ${issue.fields.reporter.accountId}`);

    // Пример: автоматически добавить метку для критических багов
    if (issue.fields.issuetype.name === 'Bug'
        && issue.fields.priority.name === 'Critical') {

        await api.asApp().requestJira(
            route`/rest/api/3/issue/${issue.key}`,
            {
                method: 'PUT',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    update: {
                        labels: [{ add: 'critical-auto' }]
                    }
                })
            }
        );

        // Сохранить статистику
        const stats = await storage.get('critical-bug-stats') || { count: 0 };
        stats.count += 1;
        await storage.set('critical-bug-stats', stats);
    }
}

export async function onIssueUpdated(event: any, context: any) {
    const { issue, changelog } = event;

    if (!changelog || !changelog.items) return;

    for (const change of changelog.items) {
        if (change.field === 'status') {
            console.log(`${issue.key}: ${change.fromString} → ${change.toString}`);

            if (change.toString === 'Done') {
                // Задача завершена — уведомить внешнюю систему
                await fetch('https://api.example.com/tasks/completed', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        issueKey: issue.key,
                        completedAt: new Date().toISOString()
                    })
                });
            }
        }
    }
}

export async function onScheduled(event: any) {
    console.log('Scheduled task executed at:', new Date().toISOString());

    // Пример: ежечасная проверка просроченных задач
    const response = await api.asApp().requestJira(
        route`/rest/api/3/search`,
        {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                jql: 'duedate < now() AND status != Done',
                maxResults: 50,
                fields: ['summary', 'assignee', 'duedate']
            })
        }
    );

    const data = await response.json();
    console.log(`Найдено просроченных задач: ${data.total}`);
}
```

**Connect Webhooks (для сравнения):**

```json
// atlassian-connect.json
{
    "modules": {
        "webhooks": [
            {
                "event": "jira:issue_created",
                "url": "/api/webhooks/issue-created"
            },
            {
                "event": "jira:issue_updated",
                "url": "/api/webhooks/issue-updated",
                "filter": "project = PROJ AND type = Bug"
            }
        ]
    }
}
```

```java
// Spring Boot — Connect Webhook handler
@RestController
@RequestMapping("/api/webhooks")
public class ConnectWebhookController {

    @PostMapping("/issue-created")
    public ResponseEntity<Void> onIssueCreated(
            @RequestBody Map<String, Object> payload,
            HttpServletRequest request) {
        // JWT уже проверен фильтром
        Tenant tenant = (Tenant) request.getAttribute("tenant");

        Map<String, Object> issue = (Map<String, Object>) payload.get("issue");
        String issueKey = (String) issue.get("key");

        log.info("[{}] Issue created: {}", tenant.getBaseUrl(), issueKey);

        // Обработка события
        eventProcessor.processAsync(tenant, issueKey, EventType.CREATED);

        return ResponseEntity.ok().build();
    }
}
```

**Различия обработки событий DC vs Cloud:**

| Аспект | DC (Event Listener) | Cloud (Forge Trigger) | Cloud (Connect Webhook) |
|---|---|---|---|
| Вызов | Синхронно в JVM | Async (serverless) | HTTP POST |
| Доставка | Гарантированная | At-least-once | At-least-once |
| Порядок | Гарантированный | Не гарантированный | Не гарантированный |
| Timeout | Нет (в JVM) | 25 секунд | Зависит от сервера |
| Retry | Нет (свой) | Автоматический (3x) | Ограниченный |

### Важное
- Forge Triggers — рекомендуемый подход для Cloud. Автоматический retry, управление через manifest.
- Событийная модель Cloud — eventual consistency. Событие может прийти с задержкой.
- Scheduled triggers в Forge ограничены: `minute`, `hour`, `day`, `week` — нет cron-выражений.
- Idempotent обработка обязательна — события могут дублироваться.

### Частые ошибки
- Синхронная тяжёлая обработка в Forge trigger — превышение 25-секундного лимита.
- Предположение о порядке событий — `issue_updated` может прийти раньше `issue_created`.
- Не обрабатывать дубликаты — проверяйте, было ли событие уже обработано (по ID или timestamp).
- Неиспользование фильтров в Connect webhooks — получение всех событий инстанса перегружает сервер.

### Как используется в 2026
- Forge Triggers стали основным механизмом для Cloud-приложений.
- Atlassian улучшил гарантии доставки и добавил dead-letter queue для failed triggers.
- Async Events в Forge — для цепочек обработки (trigger → async function → async function).
- Event-driven архитектура: Forge Trigger → External API → обратный вызов Jira API.

[к оглавлению](#Разработка-для-Jira)

---

## Какие ограничения и лимиты существуют в Jira Cloud API?

При работе с Jira Cloud API необходимо учитывать многочисленные ограничения, которые влияют на архитектуру приложения.

**Rate Limiting:**

| Тип | Лимит | Описание |
|---|---|---|
| Per-user | ~100 requests/min | Зависит от эндпоинта и плана |
| Per-app (Connect) | ~500 requests/min | На один Jira site |
| Per-app (Forge) | ~100 requests/min | Product API calls |
| Concurrent | 10 | Одновременные запросы от приложения |

```typescript
// Обработка rate limiting в Forge
async function requestWithRetry(path: string, maxRetries = 3): Promise<any> {
    for (let attempt = 0; attempt <= maxRetries; attempt++) {
        const response = await api.asApp().requestJira(route`${path}`);

        if (response.status === 429) {
            const retryAfter = parseInt(
                response.headers.get('Retry-After') || '5', 10);
            console.warn(`Rate limited. Retry after ${retryAfter}s. ` +
                         `Attempt ${attempt + 1}/${maxRetries}`);

            await new Promise(resolve =>
                setTimeout(resolve, retryAfter * 1000));
            continue;
        }

        return response.json();
    }
    throw new Error('Max retries exceeded due to rate limiting');
}
```

```java
// Обработка rate limiting в Java (Connect/Integration)
@Component
public class RateLimitedJiraClient {

    private final RestClient restClient;
    private final RetryTemplate retryTemplate;

    public RateLimitedJiraClient(RestClient restClient) {
        this.restClient = restClient;
        this.retryTemplate = RetryTemplate.builder()
                .maxAttempts(5)
                .retryOn(RateLimitException.class)
                .exponentialBackoff(1000, 2.0, 30000)
                .build();
    }

    public <T> T get(String path, Class<T> responseType) {
        return retryTemplate.execute(context -> {
            try {
                return restClient.get()
                        .uri(path)
                        .retrieve()
                        .body(responseType);
            } catch (HttpClientErrorException.TooManyRequests e) {
                String retryAfter = e.getResponseHeaders()
                        .getFirst("Retry-After");
                throw new RateLimitException(
                        Integer.parseInt(retryAfter != null ? retryAfter : "5"));
            }
        });
    }
}
```

**Пагинация:**

| Эндпоинт | Max `maxResults` | Default |
|---|---|---|
| `/search` (JQL) | 100 | 50 |
| `/project` | 50 | 50 |
| `/user/search` | 1000 | 50 |
| `/board/{id}/issue` | 100 | 50 |

**Forge-специфичные лимиты:**

| Ресурс | Лимит |
|---|---|
| Invocation timeout | 25 секунд (sync), 55 секунд (async) |
| Memory | 512 MB |
| Payload size | 6 MB (request/response) |
| Storage key-value | 32 KB per key |
| Storage total | 50 GB per app per site |
| Entity Storage queries | 50 per invocation |
| External fetch | 20 секунд timeout |
| Console log | 4 KB per invocation |
| Deploy size | 250 MB |

**JQL лимиты:**

```
// Максимальная сложность JQL-запроса
- Длина строки: 4000 символов
- Количество clauses: ~20 (рекомендуемый максимум)
- IN оператор: до 1000 значений
- Результаты: maxResults до 100 за запрос
```

**Другие лимиты:**

| Ресурс | Лимит |
|---|---|
| Attachment size | 256 MB (default, настраиваемый) |
| Custom fields | 1000 per project (рекомендуемый) |
| Projects | Нет жёсткого лимита |
| Webhooks per app | 100 |
| Connect modules | 100 per app |
| Bulk operations | 50 issues per request |

### Важное
- Rate limiting — реальность Cloud-разработки. Архитектура должна учитывать лимиты с самого начала.
- Используйте bulk-операции вместо одиночных запросов в цикле.
- `Retry-After` header — ваш друг. Всегда читайте и соблюдайте.
- Forge invocation timeout (25s) — жёсткое ограничение, которое нельзя обойти.

### Частые ошибки
- Цикл из 1000 REST-запросов по одному — мгновенно упираемся в rate limit.
- Игнорирование пагинации — получение первых 50 результатов и предположение, что это все.
- Хранение >32KB в одном ключе Storage — ошибка записи.
- Синхронная обработка тяжёлой логики в 25-секундном окне Forge.

### Как используется в 2026
- Atlassian постепенно увеличивает лимиты, но базовые ограничения остаются.
- Forge Async Events позволяют обходить 25s лимит через цепочку invocations.
- Atlassian рекомендует batch/bulk подход: минимум API-вызовов, максимум данных за вызов.
- Мониторинг usage и rate limits через Atlassian Developer Console.

[к оглавлению](#Разработка-для-Jira)

---

## Как проектировать приложение, совместимое с DC и Cloud?

Создание приложения, работающего на обеих платформах, — сложная, но востребованная задача. Ключ — архитектурное разделение платформенно-зависимого и бизнес-кода.

**Архитектурный подход:**

```
┌──────────────────────────────────────────────┐
│              Shared Business Logic            │
│         (Java library / shared module)        │
│                                               │
│  ┌─────────────┐  ┌──────────┐  ┌──────────┐ │
│  │ Domain Model│  │ Business │  │  Utils   │ │
│  │  (POJOs)    │  │  Rules   │  │          │ │
│  └─────────────┘  └──────────┘  └──────────┘ │
└──────────┬──────────────────┬────────────────┘
           │                  │
    ┌──────┴──────┐    ┌──────┴──────┐
    │  DC Adapter │    │Cloud Adapter│
    │             │    │             │
    │ P2 Plugin   │    │Forge / Conn.│
    │ Active Obj. │    │ Storage API │
    │ Jira Java   │    │ REST API v3 │
    │   API       │    │ UI Kit/React│
    │ Velocity UI │    │             │
    └─────────────┘    └─────────────┘
```

**Пример: абстракция над Jira API:**

```java
// Общий интерфейс (shared module)
public interface JiraIssueAdapter {
    IssueDto getIssue(String key);
    String createIssue(CreateIssueRequest request);
    void transitionIssue(String key, String transitionId);
    List<IssueDto> searchByJql(String jql, int maxResults);
}

// DTO (shared module)
public record IssueDto(
        String key,
        String summary,
        String status,
        String assignee,
        String projectKey,
        Map<String, Object> customFields
) {}

// DC реализация
@Named
public class DcJiraIssueAdapter implements JiraIssueAdapter {

    private final IssueManager issueManager;
    private final SearchService searchService;

    @Inject
    public DcJiraIssueAdapter(@ComponentImport IssueManager issueManager,
                              @ComponentImport SearchService searchService) {
        this.issueManager = issueManager;
        this.searchService = searchService;
    }

    @Override
    public IssueDto getIssue(String key) {
        MutableIssue issue = issueManager.getIssueByCurrentKey(key);
        return mapToDto(issue);
    }

    @Override
    public List<IssueDto> searchByJql(String jql, int maxResults) {
        // Использование Jira Java API напрямую
        Query query = jqlQueryParser.parseQuery(jql);
        SearchResults results = searchService.search(user, query,
                PagerFilter.newPageAlignedFilter(0, maxResults));
        return results.getResults().stream()
                .map(this::mapToDto)
                .collect(Collectors.toList());
    }
}
```

```typescript
// Cloud реализация (Forge / TypeScript)
import api, { route } from '@forge/api';

export class CloudJiraIssueAdapter {

    async getIssue(key: string): Promise<IssueDto> {
        const response = await api.asApp().requestJira(
            route`/rest/api/3/issue/${key}`,
            { headers: { 'Accept': 'application/json' } }
        );
        const data = await response.json();
        return this.mapToDto(data);
    }

    async searchByJql(jql: string, maxResults: number): Promise<IssueDto[]> {
        const response = await api.asApp().requestJira(
            route`/rest/api/3/search`,
            {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ jql, maxResults,
                    fields: ['summary', 'status', 'assignee'] })
            }
        );
        const data = await response.json();
        return data.issues.map(this.mapToDto);
    }
}
```

**Стратегия миграции данных:**

```
DC (Active Objects)          Cloud (Forge Storage)
┌──────────────────┐         ┌──────────────────┐
│  AO Table        │  ─────→ │  Entity Storage  │
│  - ID            │  export │  - key           │
│  - PROJECT_KEY   │  ──────→│  - projectKey    │
│  - CONFIG        │  import │  - config        │
│  - ENABLED       │         │  - enabled       │
└──────────────────┘         └──────────────────┘

// Экспорт из DC (REST-эндпоинт плагина)
GET /rest/myapi/1.0/export/configs → JSON array

// Импорт в Cloud (Forge admin page)
POST resolver: importConfigs(data)
```

**Общая бизнес-логика (Java library):**

```java
// shared-logic/src/main/java/com/example/shared/ScoreCalculator.java
public class ScoreCalculator {

    public double calculatePriorityScore(String priority, int daysOpen,
                                          boolean hasBlocker) {
        double base = switch (priority) {
            case "Critical" -> 90;
            case "High" -> 70;
            case "Medium" -> 50;
            case "Low" -> 30;
            default -> 10;
        };

        // Увеличение score за каждый день просрочки
        double ageFactor = Math.min(daysOpen * 0.5, 10);

        // Множитель за наличие блокирующей задачи
        double blockerFactor = hasBlocker ? 1.2 : 1.0;

        return Math.min(base + ageFactor, 100) * blockerFactor;
    }
}
```

### Важное
- Dual-platform = два отдельных проекта с общей бизнес-логикой, а не один «универсальный» проект.
- Java-код (shared) можно использовать в DC напрямую, для Cloud — переписать адаптеры на TypeScript.
- Feature parity между платформами — осознанный выбор. Не все функции DC воспроизводимы в Cloud (и наоборот).

### Частые ошибки
- Попытка создать единый codebase для DC и Cloud — языки и рантайм разные (Java vs TypeScript).
- Игнорирование различий API (v2 vs v3, user identification, ADF vs wiki markup).
- Не учитывать различия в модели данных — AO (реляционная) vs Storage (key-value).
- Копировать поведение DC один-в-один в Cloud без учёта Cloud-специфики (rate limits, timeouts).

### Как используется в 2026
- «Dual-platform» — маркетинговое преимущество на Atlassian Marketplace.
- Atlassian предоставляет Migration API для помощи в переносе данных DC → Cloud.
- Крупные вендоры (Tempo, Xray, ScriptRunner) поддерживают обе платформы.
- Тренд: DC как «legacy support», Cloud как primary платформа для новых фич.

[к оглавлению](#Разработка-для-Jira)

---

## Какие есть best practices при разработке для Jira?

Набор рекомендаций, применимых при разработке плагинов и приложений для обеих платформ Jira.

**Производительность:**

```java
// DC: Lazy loading — не загружать данные, пока не нужны
@Named
public class LazyIssueService {

    // ПЛОХО — загрузка всех задач проекта в память
    public void badApproach(Project project) {
        Collection<Long> allIssueIds = issueManager.getIssueIdsForProject(
                project.getId());
        // Может быть 100K+ задач!
    }

    // ХОРОШО — пагинация + ленивая загрузка
    public void goodApproach(String projectKey, int page, int pageSize) {
        String jql = "project = " + projectKey + " ORDER BY created DESC";
        Query query = jqlQueryParser.parseQuery(jql);
        PagerFilter pager = new PagerFilter(page * pageSize, pageSize);
        SearchResults results = searchService.search(user, query, pager);
    }
}

// DC: Кэширование с CacheManager
@Named
public class CachedConfigService {

    private final Cache<String, ProjectConfig> cache;
    private final ActiveObjects ao;

    @Inject
    public CachedConfigService(@ComponentImport CacheManager cacheManager,
                               @ComponentImport ActiveObjects ao) {
        this.ao = ao;
        this.cache = cacheManager.getCache(
                "com.example.plugin.project-config",
                this::loadFromDb,
                new CacheSettingsBuilder()
                        .expireAfterWrite(5, TimeUnit.MINUTES)
                        .maxEntries(500)
                        .build());
    }

    public ProjectConfig getConfig(String projectKey) {
        return cache.get(projectKey);
    }

    private ProjectConfig loadFromDb(String projectKey) {
        TaskConfig[] configs = ao.find(TaskConfig.class,
                Query.select().where("PROJECT_KEY = ?", projectKey).limit(1));
        return configs.length > 0 ? mapToProjectConfig(configs[0]) : null;
    }
}
```

**Безопасность:**

```java
// DC: XSS-предотвращение
@Named
public class SafeOutputService {

    private final TextUtils textUtils;

    // ПЛОХО — пользовательский ввод напрямую в HTML
    // <div>$userInput</div>

    // ХОРОШО — экранирование
    // <div>$htmlUtil.htmlEncode($userInput)</div>

    // ПЛОХО — SQL injection в AO
    public TaskConfig[] badSearch(String projectKey) {
        return ao.find(TaskConfig.class,
                Query.select().where("PROJECT_KEY = '" + projectKey + "'"));
    }

    // ХОРОШО — параметризованный запрос
    public TaskConfig[] goodSearch(String projectKey) {
        return ao.find(TaskConfig.class,
                Query.select().where("PROJECT_KEY = ?", projectKey));
    }
}

// DC: CSRF-защита
@Named
public class SecureServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) {
        // Проверка XSRF-токена (Atlassian предоставляет)
        String token = req.getParameter("atl_token");
        if (!xsrfTokenGenerator.validateToken(req, token)) {
            resp.sendError(403, "Invalid XSRF token");
            return;
        }
        // ... обработка
    }
}
```

**UX (Atlassian Design System):**

```html
<!-- DC: Использование AUI (Atlassian User Interface) -->
<link rel="stylesheet" href="//aui-cdn.atlassian.com/aui-adg/6.0.0/css/aui.min.css"/>

<!-- Корректные компоненты -->
<div class="aui-message aui-message-success">
    <p>Операция выполнена успешно</p>
</div>

<table class="aui aui-table-sortable">
    <thead><tr><th>Колонка</th></tr></thead>
    <tbody><tr><td>Значение</td></tr></tbody>
</table>

<button class="aui-button aui-button-primary">Сохранить</button>
```

```typescript
// Cloud: Использование Atlaskit
import Button from '@atlaskit/button';
import Flag from '@atlaskit/flag';
import DynamicTable from '@atlaskit/dynamic-table';

// Следуем Atlassian Design Guidelines
<Button appearance="primary">Сохранить</Button>
<Flag title="Успех" description="Операция выполнена" appearance="success" />
```

**Конфигурация плагина (DC):**

```java
// Хранение настроек через SAL PluginSettings
@Named
public class PluginConfigService {

    private static final String PLUGIN_KEY = "com.example.my-plugin";
    private final PluginSettingsFactory settingsFactory;

    @Inject
    public PluginConfigService(
            @ComponentImport PluginSettingsFactory settingsFactory) {
        this.settingsFactory = settingsFactory;
    }

    public void setSetting(String key, String value) {
        PluginSettings settings = settingsFactory.createGlobalSettings();
        settings.put(PLUGIN_KEY + "." + key, value);
    }

    public String getSetting(String key) {
        PluginSettings settings = settingsFactory.createGlobalSettings();
        return (String) settings.get(PLUGIN_KEY + "." + key);
    }
}
```

**Upgrade Tasks (обратная совместимость):**

```java
// Безопасное обновление плагина
public class PluginUpgradeManager implements LifecycleAware {

    @Override
    public void onStart() {
        String currentVersion = pluginAccessor
                .getPlugin("com.example.my-plugin").getPluginInformation()
                .getVersion();
        String lastVersion = configService.getSetting("installed-version");

        if (!currentVersion.equals(lastVersion)) {
            performUpgrade(lastVersion, currentVersion);
            configService.setSetting("installed-version", currentVersion);
        }
    }

    private void performUpgrade(String from, String to) {
        log.info("Upgrading plugin from {} to {}", from, to);
        // Миграция данных, обновление схемы AO и т.д.
    }
}
```

### Важное
- Производительность: никогда не загружайте все данные в память. Используйте пагинацию и кэширование.
- Безопасность: параметризованные запросы в AO, экранирование вывода, XSRF-токены для форм.
- UX: следуйте Atlassian Design Guidelines (AUI для DC, Atlaskit для Cloud).
- Обратная совместимость: upgrade tasks для безопасного обновления плагина без потери данных.

### Частые ошибки
- Пропуск валидации ввода — XSS, SQL injection, path traversal.
- Отсутствие error handling — пользователь видит stack trace вместо понятного сообщения.
- Блокирование потоков UI — тяжёлые операции должны быть асинхронными с progress bar.
- Жёсткая привязка к конкретной версии Jira API — не работает после обновления.

### Как используется в 2026
- Atlassian Design System (ADS) — единая система дизайна для DC и Cloud.
- Observability (Prometheus, OpenTelemetry) — must-have для production плагинов.
- Feature flags — безопасный rollout новых функций плагина.
- Автоматизированное тестирование на матрице версий Jira (LTS versions).

[к оглавлению](#Разработка-для-Jira)

---

## Что такое ScriptRunner и когда его использовать?

**ScriptRunner** — один из самых популярных плагинов для Jira Data Center, позволяющий расширять Jira без написания полноценного Java-плагина. Основан на Groovy-скриптинге.

**Возможности ScriptRunner для DC:**

| Функция | Описание |
|---|---|
| Script Listeners | Реакция на события Jira (аналог Event Listener) |
| Script Fields | Вычисляемые кастомные поля |
| Behaviours | Динамическое управление формами (скрытие/показ полей, валидация) |
| Script Post-Functions | Workflow post-functions на Groovy |
| Script Conditions | Workflow conditions |
| Script Validators | Workflow validators |
| Script Console | Выполнение Groovy-скриптов ad-hoc |
| REST Endpoints | Создание REST API на Groovy |
| Escalation Services | Периодические задания |
| Script Fragments | Добавление UI-элементов |

**Пример: Script Listener (реакция на создание задачи):**

```groovy
import com.atlassian.jira.component.ComponentAccessor
import com.atlassian.jira.event.issue.IssueEvent
import com.atlassian.jira.issue.MutableIssue

def event = event as IssueEvent
def issue = event.issue as MutableIssue
def customFieldManager = ComponentAccessor.customFieldManager
def issueManager = ComponentAccessor.issueManager

// Если создан Critical Bug — автоматически установить label
if (issue.issueType.name == 'Bug' && issue.priority.name == 'Critical') {
    def currentLabels = issue.labels
    currentLabels.add('critical-auto')
    issue.setLabels(currentLabels)
    issueManager.updateIssue(event.user, issue,
            com.atlassian.jira.event.type.EventDispatchOption.DO_NOT_DISPATCH, false)

    log.info("Added critical-auto label to ${issue.key}")
}
```

**Пример: Script Field (вычисляемое поле):**

```groovy
// Подсчёт дней с момента создания задачи
import java.time.LocalDate
import java.time.temporal.ChronoUnit

def created = issue.created.toInstant()
        .atZone(java.time.ZoneId.systemDefault()).toLocalDate()
def today = LocalDate.now()

return ChronoUnit.DAYS.between(created, today)
```

**Пример: Behaviour (динамическая форма):**

```groovy
// Показать поле "Environment" только для типа "Bug"
def issueType = getFieldById("issuetype")
def environment = getFieldByName("Environment")

if (issueType.value?.name == "Bug") {
    environment.setHidden(false)
    environment.setRequired(true)
} else {
    environment.setHidden(true)
    environment.setRequired(false)
}
```

**Пример: REST Endpoint:**

```groovy
import com.onresolve.scriptrunner.runner.rest.common.CustomEndpointDelegate
import groovy.json.JsonOutput
import groovy.transform.BaseScript
import javax.ws.rs.core.MediaType
import javax.ws.rs.core.Response

@BaseScript CustomEndpointDelegate delegate

getProjectStats(httpMethod: "GET", groups: ["jira-administrators"]) {
    MultivaluedMap queryParams, String body ->

    def projectKey = queryParams.getFirst("project") as String
    def searchService = ComponentAccessor.getComponent(SearchService)
    // ... логика подсчёта

    def stats = [
        project: projectKey,
        openBugs: 42,
        avgResolutionDays: 3.5
    ]

    Response.ok(JsonOutput.toJson(stats))
            .type(MediaType.APPLICATION_JSON)
            .build()
}
// URL: GET /rest/scriptrunner/latest/custom/getProjectStats?project=PROJ
```

**Когда ScriptRunner достаточно vs когда нужен плагин:**

| Критерий | ScriptRunner | Кастомный плагин |
|---|---|---|
| Сложность логики | Простая-средняя | Высокая |
| UI | Минимальный | Полноценный |
| Модель данных | Поля Jira / PluginSettings | Active Objects (собственные таблицы) |
| Тестирование | Ручное / Script Console | JUnit, интеграционные тесты |
| CI/CD | Ограниченно | Полноценный |
| Производительность | Достаточная для скриптов | Оптимизируемая |
| Поддержка | Скрипты в файлах/БД | Полноценный проект с VCS |
| Время разработки | Минуты-часы | Дни-недели |

**ScriptRunner Cloud:**
- Значительно ограниченнее DC-версии
- JavaScript вместо Groovy
- Ограниченный набор доступных Jira API
- Нет Script Console
- Нет Behaviours
- Подходит для простых listener и post-function сценариев

### Важное
- ScriptRunner — №1 плагин на Atlassian Marketplace по количеству установок.
- Для 70-80% задач кастомизации ScriptRunner достаточно — не нужно писать Java-плагин.
- Groovy-скрипты имеют полный доступ к внутреннему Java API Jira (в DC).
- ScriptRunner скрипты можно хранить в VCS и деплоить через shared filesystem.

### Частые ошибки
- Groovy-скрипты без error handling — ошибка в listener может сломать операции пользователей.
- Тяжёлая логика в Script Field — поле вычисляется при каждом отображении задачи.
- Использование `ComponentAccessor` без проверки результата на `null`.
- Скрипты, зависящие от имён полей/статусов — ломаются при переименовании (используйте ID).
- Не тестировать скрипты на staging перед production.

### Как используется в 2026
- ScriptRunner DC — по-прежнему самый популярный способ быстрой кастомизации Jira DC.
- ScriptRunner Cloud развивается, но значительно уступает DC-версии по возможностям.
- Тренд: ScriptRunner для быстрого прототипирования, миграция на плагин при росте сложности.
- Adaptavist (разработчик ScriptRunner) предоставляет библиотеку готовых скриптов.
- Для Cloud рекомендуют Jira Automation (встроенный) для простых сценариев вместо ScriptRunner Cloud.

[к оглавлению](#Разработка-для-Jira)

---

[Вопросы для собеседования](README.md)
