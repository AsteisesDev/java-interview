[Вопросы для собеседования](README.md)

# Разработка для Jira

**Общие вопросы**

+ [Какие платформы разработки для Jira существуют?](#какие-платформы-разработки-для-jira-существуют)
+ [Какие существуют способы расширения Jira?](#какие-существуют-способы-расширения-jira)
+ [Что такое Jira REST API и как с ним работать?](#что-такое-jira-rest-api-и-как-с-ним-работать)
+ [Что такое JQL (Jira Query Language)?](#что-такое-jql-jira-query-language)
+ [Что такое Webhooks в Jira и как их использовать?](#что-такое-webhooks-в-jira-и-как-их-использовать)
+ [Как интегрировать внешнее Spring-приложение с Jira?](#как-интегрировать-внешнее-spring-приложение-с-jira)

**Jira Data Center (P2 плагины)**

+ [Что такое Atlassian SDK и как создать плагин?](#что-такое-atlassian-sdk-и-как-создать-плагин)
+ [Что такое atlassian-plugin.xml и какие модули плагина существуют?](#что-такое-atlassian-pluginxml-и-какие-модули-плагина-существуют)
+ [Как работает Spring в плагинах Jira Data Center?](#как-работает-spring-в-плагинах-jira-data-center)
+ [Что такое Active Objects и как с ними работать?](#что-такое-active-objects-и-как-с-ними-работать)
+ [Как создать REST-эндпоинт в плагине Jira DC?](#как-создать-rest-эндпоинт-в-плагине-jira-dc)
+ [Как создать Servlet Module в плагине?](#как-создать-servlet-module-в-плагине)
+ [Как создать Custom Field Type?](#как-создать-custom-field-type)
+ [Как создать Workflow Post-Function, Condition, Validator?](#как-создать-workflow-post-function-condition-validator)
+ [Как работать с Event Listener в плагине?](#как-работать-с-event-listener-в-плагине)
+ [Как работает кластеризация в Jira Data Center и как это влияет на плагины?](#как-работает-кластеризация-в-jira-data-center-и-как-это-влияет-на-плагины)
+ [Как тестировать плагины Jira DC?](#как-тестировать-плагины-jira-dc)
+ [Как отлаживать и профилировать плагины?](#как-отлаживать-и-профилировать-плагины)

**Jira Cloud (Forge и Connect)**

+ [Что такое Forge и как создать Forge-приложение?](#что-такое-forge-и-как-создать-forge-приложение)
+ [Что такое Connect и чем он отличается от Forge?](#что-такое-connect-и-чем-он-отличается-от-forge)
+ [Как работает аутентификация и авторизация в Jira Cloud приложениях?](#как-работает-аутентификация-и-авторизация-в-jira-cloud-приложениях)
+ [Что такое Forge Storage API и Custom UI?](#что-такое-forge-storage-api-и-custom-ui)
+ [Как обрабатывать события в Jira Cloud?](#как-обрабатывать-события-в-jira-cloud)
+ [Какие ограничения и лимиты существуют в Jira Cloud API?](#какие-ограничения-и-лимиты-существуют-в-jira-cloud-api)

**Архитектура и практики**

+ [Как проектировать приложение, совместимое с DC и Cloud?](#как-проектировать-приложение-совместимое-с-dc-и-cloud)
+ [Какие есть best practices при разработке для Jira?](#какие-есть-best-practices-при-разработке-для-jira)
+ [Что такое ScriptRunner и когда его использовать?](#что-такое-scriptrunner-и-когда-его-использовать)

## Какие платформы разработки для Jira существуют?
<!-- grade: junior -->

Платформы разработки для Jira — это варианты развёртывания Jira, каждый из которых определяет свой подход к созданию расширений и интеграций.

> **Аналогия из жизни:** платформы Jira можно сравнить с типами жилья. Data Center — это собственный дом (полный контроль, но обслуживание на вас). Cloud — это арендованная квартира в управляемом доме (удобно, но хозяин устанавливает правила). Server — снесённый дом, из которого пора переезжать.

### Jira Server (EOL февраль 2024)

- Однонодовая on-premise установка
- Плагины в формате P2 (OSGi-бандлы)
- Прекращена продажа в феврале 2021, полный End of Life — февраль 2024
- Все клиенты вынуждены мигрировать на Data Center или Cloud

### Jira Data Center

- Кластерная on-premise установка (несколько нод за балансировщиком)
- Общая БД и файловая система между нодами
- Те же P2-плагины, что и Server, но с требованиями к кластерной совместимости
- Поддерживает нулевой простой при обновлении (zero-downtime upgrades)
- Лицензируется по количеству пользователей (от 500)

### Jira Cloud

- SaaS-решение, управляемое Atlassian
- Два фреймворка для приложений: Forge (serverless) и Connect (self-hosted)
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

### Миграция

Миграция Server на DC относительно простая — тот же код плагинов, но нужно обеспечить кластерную совместимость (stateless-дизайн, распределённые кэши и блокировки).

Миграция Server на Cloud сложная — полная переработка плагинов на Forge/Connect, миграция данных через Jira Cloud Migration Assistant (JCMA), переработка интеграций.

### Частые ошибки

- Разработка нового плагина под Server API без учёта DC-специфики (кластеризация)
- Предположение, что плагин для DC автоматически работает в Cloud — это совершенно разные платформы
- Игнорирование сроков EOL при планировании — Server EOL уже наступил

### Как используется в 2026

- Data Center остаётся основной платформой для крупных enterprise-клиентов с on-premise требованиями
- Cloud доминирует по количеству инстансов и активно продвигается Atlassian
- Atlassian инвестирует преимущественно в Cloud — новые фичи появляются там первыми
- Forge стал рекомендуемым фреймворком для новых Cloud-приложений

> **На собеседовании:** интервьюер хочет услышать, что в 2026 году актуальны только Data Center и Cloud, а Server полностью прекращён. Покажите понимание ключевого отличия: DC — это свой контроль и P2-плагины, Cloud — это SaaS с Forge/Connect. Частая ошибка — не упомянуть, что плагины DC и Cloud абсолютно несовместимы.

[к оглавлению](#разработка-для-jira)

---

## Какие существуют способы расширения Jira?
<!-- grade: junior -->

Способы расширения Jira — это методы добавления функциональности, выбор которых определяется платформой (DC или Cloud) и сложностью задачи.

| Способ | Платформа | Язык | Хостинг | Сложность |
|---|---|---|---|---|
| P2 Plugin (Atlassian SDK) | Data Center | Java | Внутри Jira | Высокая |
| Forge App | Cloud | TypeScript/JS | Atlassian (serverless) | Средняя |
| Connect App | Cloud | Любой | Свой сервер | Средняя-Высокая |
| REST API Integration | DC + Cloud | Любой | Внешний | Низкая-Средняя |
| ScriptRunner | DC (+ Cloud limited) | Groovy / JS | Внутри Jira | Низкая |

### P2 плагины (Data Center)

- Полный доступ к внутреннему API Jira (Java)
- Могут добавлять UI-элементы, workflow-функции, custom fields, REST-эндпоинты
- Деплоятся как OSGi-бандлы прямо в Jira
- Максимальная гибкость, но и максимальная ответственность (могут уронить Jira)

### Forge (Cloud)

- Serverless-платформа Atlassian
- Код выполняется в изолированной среде Atlassian
- Ограниченный, но безопасный доступ к API
- UI через UI Kit (декларативный) или Custom UI (React)

### Connect (Cloud)

- Self-hosted приложение, интегрированное через iframe и REST API
- Полный контроль над стеком и инфраструктурой
- JWT-аутентификация между Jira и приложением
- Подходит для сложных приложений с собственной БД

### REST API Integration

- Внешнее приложение вызывает Jira REST API
- Не расширяет UI Jira напрямую
- Подходит для автоматизации, синхронизации данных, CI/CD

### ScriptRunner

- Готовый плагин для DC, позволяющий писать Groovy-скрипты
- Script Listeners, Script Fields, Behaviours, REST Endpoints
- Быстрое прототипирование без полноценной разработки плагина
- Ограниченная Cloud-версия (JavaScript вместо Groovy)

### Частые ошибки

- Написание P2-плагина для простой задачи, решаемой ScriptRunner-скриптом за 20 минут
- Выбор Connect вместо Forge без чёткого обоснования — Forge проще в поддержке
- Попытка использовать REST API для задач, требующих интеграции в UI Jira

### Как используется в 2026

- Forge стал зрелой платформой с поддержкой Custom UI, Storage API, async events
- Connect остаётся востребованным для сложных enterprise-приложений с собственной инфраструктурой
- P2-плагины для DC по-прежнему актуальны, но новые разработчики всё чаще начинают с Cloud
- Растёт тренд на dual-platform приложения — единая бизнес-логика, адаптеры для DC и Cloud

> **На собеседовании:** начните с таблицы сравнения, покажите, что выбор зависит от платформы и задачи. Упомяните ScriptRunner как быструю альтернативу полноценному плагину для DC. Не забудьте, что P2-плагины дают максимум возможностей, но несут риск повлиять на стабильность Jira.

[к оглавлению](#разработка-для-jira)

---

## Что такое Jira REST API и как с ним работать?
<!-- grade: junior -->

Jira REST API — основной программный интерфейс для взаимодействия с Jira извне, позволяющий выполнять CRUD-операции над задачами, проектами, пользователями, workflow и другими сущностями.

### REST API v2 vs v3

- v2 — стабильный API, доступен и в DC, и в Cloud. Базовый URL: `/rest/api/2/`
- v3 — Cloud-only, поддерживает ADF (Atlassian Document Format) для rich-text, улучшенная пагинация. Базовый URL: `/rest/api/3/`

### Аутентификация

| Метод | DC | Cloud | Описание |
|---|---|---|---|
| Basic Auth | Да | Нет (deprecated) | username:password в Base64 |
| PAT (Personal Access Token) | Да | Нет | Токен, привязанный к пользователю DC |
| API Token | Нет | Да | Токен Atlassian аккаунта + email |
| OAuth 2.0 (3LO) | Нет | Да | Полный OAuth-flow с авторизацией пользователя |
| OAuth 2.0 (2LO) | Нет | Да | App-level доступ без пользователя |

### Основные эндпоинты

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

### Пример: создание задачи (Java, Spring RestClient)

<details>
<summary>Код JiraIssueService</summary>

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

</details>

### Пример: поиск через JQL

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

### Пример: смена статуса (transition)

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

### Пагинация и Rate Limiting

- Параметры `startAt` и `maxResults`, ответ содержит `total`, `startAt`, `maxResults`
- Cloud API ограничивает `maxResults` до 100 для большинства эндпоинтов
- HTTP 429 (Too Many Requests) с заголовком `Retry-After`, рекомендуется экспоненциальный backoff

### Частые ошибки

- Использование Basic Auth с паролем в Cloud — поддерживается только API Token + email
- Игнорирование rate limiting — приложение получает 429 и ломается
- Жёсткое указание `maxResults=1000` — в Cloud реальный лимит 100, лишнее игнорируется
- Не обработка ошибок 404 при обращении к удалённым или перемещённым задачам

### Как используется в 2026

- REST API v3 — стандарт для Cloud-разработки
- Spring 6+ RestClient и WebClient — основные HTTP-клиенты для Java-интеграций
- Atlassian активно развивает GraphQL API (пока в beta), но REST остаётся основным
- Personal Access Tokens (PAT) стали стандартом аутентификации для DC-интеграций

> **На собеседовании:** покажите знание отличий между v2 (DC + Cloud) и v3 (Cloud-only). Обязательно упомяните пагинацию и rate limiting — это реальная повседневная проблема. Частая ошибка кандидатов — не упомянуть различия в аутентификации между DC (PAT) и Cloud (OAuth 2.0).

[к оглавлению](#разработка-для-jira)

---

## Что такое JQL (Jira Query Language)?
<!-- grade: junior -->

JQL (Jira Query Language) — структурированный язык запросов для поиска задач в Jira, используемый в UI (фильтры, дашборды), REST API (эндпоинт `/search`), плагинах и автоматизации.

> **Аналогия из жизни:** JQL — это как поисковая строка Google, но для задач Jira. Вместо свободного текста вы пишете структурированные условия: какой проект, какой статус, кто исполнитель — и получаете точный результат.

### Базовый синтаксис

```
field operator value [AND|OR] field operator value ORDER BY field [ASC|DESC]
```

### Операторы

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

### Встроенные функции

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

### Custom Fields в JQL

```
-- По имени (с кавычками)
"Story Points" > 5

-- По ID (надёжнее)
cf[10100] > 5
```

### Сложные запросы

```
project = PROJ
    AND status IN ("In Progress", "Code Review")
    AND assignee IN membersOf("backend-team")
    AND created >= startOfWeek()
    AND priority IN (Critical, Blocker)
    AND "Story Points" <= 8
ORDER BY priority DESC, created ASC
```

### Использование JQL в Java

<details>
<summary>Пример через REST API и в плагине Jira DC</summary>

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

</details>

### Производительность JQL

- Индексируемые поля (status, project, priority) работают быстро
- Текстовый поиск (`~`) использует Lucene, может быть медленным на больших инстансах
- `WAS` и `CHANGED` операторы сканируют историю — дорогие операции
- Сложные `OR` с подзапросами замедляют выполнение
- Для DC: кастомные поля без индексации значительно замедляют поиск

### Частые ошибки

- Использование `=` вместо `~` для текстового поиска: `summary = "error"` ищет точное совпадение
- Забыть кавычки для значений с пробелами: `status = In Progress` — ошибка, нужно `"In Progress"`
- Неэффективные запросы с `WAS`/`CHANGED` на инстансах с большой историей
- Использование `ORDER BY` с неиндексированными кастомными полями

### Как используется в 2026

- JQL остаётся основным языком запросов в Jira DC и Cloud
- В Cloud добавлены новые функции: `issuesWithRemoteLinksByGlobalId()`, улучшенный полнотекстовый поиск
- Atlassian экспериментирует с AI-ассистентом для генерации JQL из естественного языка
- JQL используется в Jira Automation rules для определения scope правил

> **На собеседовании:** знание JQL ожидается от любого Jira-разработчика. Покажите владение основными операторами, функциями (`currentUser()`, `membersOf()`, `startOfWeek()`) и умение составлять сложные запросы. Отметьте, что `cf[id]` надёжнее имени поля и что `~` поддерживает Lucene-синтаксис.

[к оглавлению](#разработка-для-jira)

---

## Что такое Webhooks в Jira и как их использовать?
<!-- grade: middle -->

Webhook — механизм уведомления внешних систем о событиях в Jira через HTTP POST-запросы. Когда в Jira происходит определённое событие, Jira отправляет JSON-payload на указанный URL.

> **Аналогия из жизни:** webhook — это как SMS-уведомление от банка. Вместо того чтобы каждые 5 минут проверять баланс (polling), банк сам отправляет вам сообщение при каждой операции (push).

### Доступные события

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

### Регистрация webhook через REST API

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

### Структура payload (issue_updated)

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

### Spring Boot webhook listener

<details>
<summary>Код JiraWebhookController</summary>

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

</details>

### Безопасность

- Shared Secret (DC): при регистрации webhook указывается secret, Jira подписывает payload HMAC-SHA256
- IP Whitelisting: ограничение входящих запросов по IP Jira-инстанса
- HTTPS: обязательно для production
- Идемпотентность: webhook может быть доставлен повторно — обработчик должен быть идемпотентным

### Частые ошибки

- Синхронная тяжёлая обработка в webhook-обработчике — нужно принять webhook, ответить 200 и обработать асинхронно
- Отсутствие валидации подписи — любой может отправить поддельный webhook
- Нет обработки дубликатов — Jira может повторно отправить webhook при timeout
- Регистрация webhook без JQL-фильтра — получение всех событий инстанса перегружает приложение

### Как используется в 2026

- В Cloud рекомендуется использовать Forge Triggers вместо прямой регистрации webhooks
- Jira Automation (встроенная) заменяет простые webhook-сценарии (if event, then action)
- Для DC webhooks остаются основным механизмом push-интеграции
- Тренд на event-driven архитектуру: Jira webhook -> Kafka -> микросервисы

> **На собеседовании:** подчеркните, что webhook — push-модель в отличие от polling. Обязательно упомяните идемпотентность и асинхронную обработку. Webhook должен ответить за 10 секунд (DC) / 30 секунд (Cloud). Для Cloud упомяните Forge Triggers как более надёжную альтернативу.

[к оглавлению](#разработка-для-jira)

---

## Как интегрировать внешнее Spring-приложение с Jira?
<!-- grade: middle -->

Интеграция внешнего Spring Boot приложения с Jira — частая задача enterprise-разработки, включающая синхронизацию задач, автоматизацию workflow и агрегацию данных.

### Архитектура интеграции

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

### Конфигурация (application.yml)

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

### Jira Client с retry и error handling

<details>
<summary>Код JiraClientConfig и JiraClient</summary>

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

</details>

### Синхронизация данных

<details>
<summary>Код JiraSyncService</summary>

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

</details>

### Частые ошибки

- Хранение токенов/паролей в application.yml — используйте переменные окружения или Vault
- Отсутствие retry-логики — Jira может временно возвращать 5xx
- Игнорирование rate limiting в Cloud — нужно обрабатывать HTTP 429 и ждать Retry-After
- Синхронные вызовы Jira API в обработчике HTTP-запроса — вызывает задержки для пользователя

### Как используется в 2026

- Spring Boot 3.x + RestClient — стандартный стек для интеграций
- JRJC (Jira REST Java Client) устарел и не обновляется — для новых проектов лучше собственный клиент
- Популярны интеграции через Spring Cloud Stream / Kafka: webhook -> Kafka topic -> обработчик
- Для Cloud-интеграций OAuth 2.0 (3LO) стал обязательным для user-context операций

> **На собеседовании:** покажите понимание двух моделей интеграции: polling (scheduled sync) и push (webhooks). Комбинируйте оба подхода: webhooks для real-time, polling для catch-up. Обязательно упомяните retry, пагинацию и безопасное хранение токенов.

[к оглавлению](#разработка-для-jira)

---

## Что такое Atlassian SDK и как создать плагин?
<!-- grade: middle -->

Atlassian SDK — набор инструментов для разработки плагинов (P2) для продуктов Atlassian, включая Jira Data Center, основанный на Maven и предоставляющий архетипы, команды сборки и локальное окружение для разработки.

### Создание проекта

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

### Ключевые команды SDK

```bash
atlas-run            # Запуск Jira с плагином (полная сборка + развёртывание)
atlas-debug          # Запуск с remote debug на порту 5005
atlas-package        # Сборка JAR/OBR для деплоя
atlas-mvn clean install  # Maven-сборка через обёртку SDK
atlas-unit-test      # Запуск юнит-тестов
atlas-integration-test   # Запуск интеграционных тестов
```

### pom.xml (ключевые части)

<details>
<summary>Конфигурация Maven</summary>

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

</details>

### OSGi основы

P2-плагины — это OSGi-бандлы. Каждый плагин работает в своём загрузчике классов (ClassLoader), изолированно от других плагинов.

```
┌─────────────────────────────────────────┐
│             Jira Application            │
│  ┌───────────┐  ┌───────────┐          │
│  │ Plugin A  │  │ Plugin B  │          │
│  │ (Bundle)  │  │ (Bundle)  │          │
│  │ Import:   │  │ Export:   │          │
│  │  pkg.b    │  │  pkg.b    │          │
│  └───────────┘  └───────────┘          │
│  OSGi Framework (Felix)                 │
└─────────────────────────────────────────┘
```

- Import-Package: пакеты, которые плагин использует из хоста или других плагинов
- Export-Package: пакеты, которые плагин предоставляет другим
- `@ExportAsService` — публикация компонента как OSGi-сервиса
- `@ComponentImport` — импорт сервиса из Jira или другого плагина

### Частые ошибки

- Добавление зависимостей без `provided` scope — плагин тащит конфликтующие библиотеки
- Использование библиотек, несовместимых с OSGi (например, некоторые версии Guava, Jackson)
- Забыть `atlas-package` после изменений — QuickReload подхватывает только скомпилированный JAR
- Разработка с версией Jira, отличной от production — API может отличаться

### Как используется в 2026

- Atlassian SDK продолжает поддерживаться для DC, но Atlassian не добавляет новых функций
- AMPS (Atlassian Maven Plugin Suite) версии 8.x — актуальная версия
- DC 9.x и 10.x — актуальные версии Jira для разработки
- Сообщество активно использует Docker для локальной разработки как альтернативу atlas-run

> **На собеседовании:** покажите знание основных команд SDK (atlas-run, atlas-debug, atlas-package) и понимание OSGi-модели. Ключевой момент: зависимости Jira API имеют scope provided, потому что они уже есть в рантайме. QuickReload ускоряет цикл разработки без перезапуска Jira.

[к оглавлению](#разработка-для-jira)

---

## Что такое atlassian-plugin.xml и какие модули плагина существуют?
<!-- grade: middle -->

atlassian-plugin.xml — центральный дескриптор P2-плагина, определяющий метаданные плагина и перечисляющий все модули (точки расширения), которые плагин предоставляет.

### Базовая структура

```xml
<atlassian-plugin key="com.example.my-plugin" name="My Jira Plugin"
                  plugins-version="2">
    <plugin-info>
        <description>Описание плагина</description>
        <version>1.0.0</version>
        <vendor name="Example Corp" url="https://example.com"/>
        <param name="plugin-icon">images/plugin-icon.png</param>
        <param name="atlassian-data-center-status">compatible</param>
        <param name="atlassian-data-center-compatible">true</param>
    </plugin-info>
</atlassian-plugin>
```

### Основные типы модулей

| Модуль | Назначение | Пример key |
|---|---|---|
| REST Module | REST-эндпоинты (JAX-RS) | `<rest>` |
| Servlet Module | HTTP-сервлеты, веб-страницы | `<servlet>` |
| Web Item | Ссылки в UI Jira | `<web-item>` |
| Web Section | Секция для группировки web-items | `<web-section>` |
| Web Panel | HTML-панели в страницах Jira | `<web-panel>` |
| Web Resource | CSS, JS ресурсы | `<web-resource>` |
| Component | Spring-компонент плагина | `<component>` |
| Component Import | Импорт компонента из Jira | `<component-import>` |
| Workflow Function | Расширения workflow | `<workflow-function>` |
| Custom Field Type | Кастомный тип поля | `<customfield-type>` |
| Listener | Обработчик событий | `<listener>` |
| Report | Отчёт | `<report>` |

<details>
<summary>Примеры объявления модулей в XML</summary>

```xml
<!-- REST Module -->
<rest key="my-rest" path="/myapi" version="1.0">
    <description>REST API плагина</description>
</rest>

<!-- Servlet Module -->
<servlet key="my-servlet" class="com.example.MyServlet">
    <url-pattern>/my-page</url-pattern>
</servlet>

<!-- Web Item -->
<web-item key="my-link" section="jira.issue.tools" weight="100">
    <label>Мой пункт меню</label>
    <link>/plugins/servlet/my-page?issueKey=${issue.key}</link>
    <condition class="com.atlassian.jira.plugin.webfragment.conditions.UserLoggedInCondition"/>
</web-item>

<!-- Web Panel -->
<web-panel key="my-panel" location="atl.jira.view.issue.right.context" weight="200">
    <label>Дополнительная информация</label>
    <resource name="view" type="velocity" location="templates/my-panel.vm"/>
    <context-provider class="com.example.MyPanelContextProvider"/>
</web-panel>

<!-- Web Resource -->
<web-resource key="my-resources" name="Plugin Resources">
    <dependency>com.atlassian.auiplugin:ajs</dependency>
    <resource type="download" name="app.js" location="js/app.js"/>
    <resource type="download" name="styles.css" location="css/styles.css"/>
    <context>atl.general</context>
</web-resource>

<!-- Workflow Function -->
<workflow-function key="my-postfunction"
                   class="com.example.MyPostFunctionFactory">
    <function-class>com.example.MyPostFunction</function-class>
    <orderable>true</orderable>
    <unique>false</unique>
    <resource name="view" type="velocity" location="templates/postfunction-view.vm"/>
</workflow-function>

<!-- Custom Field Type -->
<customfield-type key="my-field" class="com.example.MyCustomField">
    <name>Моё кастомное поле</name>
    <resource name="view" type="velocity" location="templates/field-view.vm"/>
    <resource name="edit" type="velocity" location="templates/field-edit.vm"/>
</customfield-type>
```

</details>

### Частые ошибки

- Дублирование key в модулях — key должен быть уникальным в рамках плагина
- Объявление component-import для сервисов, которые уже доступны через Spring Scanner `@ComponentImport`
- Неправильная секция (section/location) для web-item/web-panel — элемент просто не появляется в UI
- Отсутствие `<condition>` для web-item — пункт меню виден всем, включая анонимных пользователей

### Как используется в 2026

- XML-дескриптор остаётся обязательным для P2-плагинов, но содержимое минимизируется за счёт аннотаций
- Spring Scanner 2.x — стандарт, 1.x считается устаревшим
- Параметр `atlassian-data-center-compatible=true` обязателен для публикации на Marketplace
- Atlassian рекомендует минимизировать количество модулей для ускорения загрузки плагина

> **На собеседовании:** покажите знание основных типов модулей и их назначения. Отметьте, что `plugins-version="2"` означает формат P2 (OSGi). С Spring Scanner 2.x многие модули (component, component-import) можно объявлять через аннотации, но workflow-function и custom-field-type по-прежнему требуют XML.

[к оглавлению](#разработка-для-jira)

---

## Как работает Spring в плагинах Jira Data Center?
<!-- grade: middle -->

Плагины Jira DC используют Atlassian Spring Scanner — обёртку над Spring Framework, адаптированную для работы в OSGi-среде. Это не стандартный Spring Boot: здесь нет автоконфигурации, component-scan по конвенциям, и аннотации отличаются.

### Ключевые аннотации

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

### Пример компонента плагина

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

### Spring Scanner 2.x vs 1.x

| Аспект | Scanner 1.x | Scanner 2.x |
|---|---|---|
| Сканирование | Runtime (медленно) | Compile-time (быстро) |
| Конфигурация | XML + аннотации | Преимущественно аннотации |
| Производительность | Медленный старт плагина | Быстрый старт |
| component-import в XML | Обязательно | Не нужно (через `@ComponentImport`) |

### Отличия от стандартного Spring Boot

| Spring Boot | Jira Plugin (Spring Scanner) |
|---|---|
| `@Service`, `@Component` | `@Named` (javax.inject) |
| `@Autowired` | `@Inject` (javax.inject) |
| Component scan по пакетам | Spring Scanner — compile-time генерация |
| `@Configuration` + `@Bean` | Ограничено, чаще — XML или `@Named` |
| `@Value("${prop}")` | SAL PluginSettings или System.getProperty() |
| ApplicationContext | OSGi BundleContext |
| Spring Data JPA | Active Objects |

### Экспорт сервиса для других плагинов

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

// Другой плагин может импортировать этот сервис:
@Named
public class ConsumerComponent {

    @Inject
    public ConsumerComponent(
            @ComponentImport NotificationService notificationService) {
        // использование сервиса из другого плагина
    }
}
```

### Частые ошибки

- Использование `@Autowired` вместо `@Inject` — может не работать в OSGi-контексте
- Использование `@Service`/`@Component` вместо `@Named` — Spring Scanner их не обнаруживает
- Забыть `@ComponentImport` при инжекции Jira-сервиса — получите NoSuchBeanDefinitionException
- Циклические зависимости между компонентами — Spring Scanner не всегда корректно их разрешает
- Попытка использовать Spring Boot стартеры — они несовместимы с OSGi

### Как используется в 2026

- Spring Scanner 2.x — стандарт для всех новых плагинов
- Compile-time сканирование стало нормой — runtime-сканирование считается legacy
- Многие разработчики приходят из Spring Boot и допускают типичные ошибки — знание различий критично
- Atlassian не планирует переход на Spring Boot для DC-плагинов — OSGi-модель остаётся

> **На собеседовании:** главное — чётко показать отличия от стандартного Spring Boot. `@Named` вместо `@Service`, `@Inject` вместо `@Autowired`, `@ComponentImport` только для сервисов из Jira/других плагинов. Всегда используйте constructor injection. Это одна из тем, где ошибки допускают даже опытные Spring-разработчики.

[к оглавлению](#разработка-для-jira)

---

## Что такое Active Objects и как с ними работать?
<!-- grade: middle -->

Active Objects (AO) — ORM-фреймворк для плагинов Jira Data Center, позволяющий плагинам хранить данные в БД Jira без создания таблиц вручную. По концепции похож на JPA, но значительно легковеснее.

> **Аналогия из жизни:** Active Objects — это как встроенный шкаф в съёмной квартире. Вы не строите мебель сами (не создаёте таблицы SQL), а описываете, что хотите хранить (интерфейсы), и фреймворк сам организует пространство. При выезде (удалении плагина) шкаф убирается автоматически.

### Ключевые особенности

- Таблицы создаются автоматически из Java-интерфейсов
- Префикс таблиц — уникальный для каждого плагина (избежание конфликтов)
- Поддержка миграций (upgrade tasks)
- Работает с БД Jira (PostgreSQL, MySQL, Oracle, MS SQL)

### Определение сущности (entity)

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

### Регистрация AO в atlassian-plugin.xml

```xml
<ao key="ao-module">
    <description>Active Objects модуль</description>
    <entity>com.example.plugin.ao.TaskConfig</entity>
    <entity>com.example.plugin.ao.TaskExecution</entity>
</ao>
```

### CRUD-операции

<details>
<summary>Код TaskConfigService</summary>

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

</details>

### Миграция (Upgrade Task)

```java
public class UpgradeTask001 implements ActiveObjectsUpgradeTask {

    @Override
    public ModelVersion getModelVersion() {
        return ModelVersion.valueOf("1");
    }

    @Override
    public void upgrade(ModelVersion currentVersion, ActiveObjects ao) {
        ao.migrate(TaskConfig.class);
    }
}
```

```xml
<ao key="ao-module">
    <entity>com.example.plugin.ao.TaskConfig</entity>
    <entity>com.example.plugin.ao.TaskExecution</entity>
    <upgradeTask>com.example.plugin.upgrade.UpgradeTask001</upgradeTask>
</ao>
```

### Частые ошибки

- Забыть `@Preload` — N+1 проблема, каждый доступ к полю = отдельный SELECT
- Использование Java-имён полей в Query вместо SQL-имён: `"projectKey"` вместо `"PROJECT_KEY"`
- Не обернуть запись в транзакцию — данные могут быть в inconsistent-состоянии
- Отсутствие upgrade tasks — при первом деплое таблицы не создаются
- Хранение больших данных без `@StringLength(UNLIMITED)` — дефолтная длина строки 255 символов

### Как используется в 2026

- Active Objects остаётся стандартом для DC-плагинов
- Нет альтернатив (JPA/Hibernate недоступны из-за OSGi-изоляции)
- Для Cloud (Forge) аналог — Forge Storage API и Entity Storage
- При планировании миграции DC -> Cloud нужно продумать перенос данных из AO в Cloud storage

> **На собеседовании:** подчеркните, что AO — единственный поддерживаемый способ хранения данных плагина в БД Jira DC. Обязательно упомяните `@Preload` (без него — N+1), UPPERCASE-имена колонок в Query и `executeInTransaction()` для записи. Это практические знания, которые показывают реальный опыт.

[к оглавлению](#разработка-для-jira)

---

## Как создать REST-эндпоинт в плагине Jira DC?
<!-- grade: middle -->

REST-модуль плагина позволяет создавать API-эндпоинты, доступные по URL вида `/rest/myapi/1.0/...`, используя JAX-RS (Jersey).

### Объявление в atlassian-plugin.xml

```xml
<rest key="my-rest-api" path="/myapi" version="1.0">
    <description>REST API моего плагина</description>
</rest>
```

### REST-ресурс

<details>
<summary>Код ConfigResource</summary>

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

</details>

URL для вызова: `GET https://jira.company.com/rest/myapi/1.0/config/42`

### Частые ошибки

- Возвращение Entity (AO) напрямую вместо DTO — вызывает LazyLoading-ошибки и утечку данных
- Забыть проверку прав — REST-эндпоинт доступен любому аутентифицированному пользователю
- Использование `@Autowired` вместо `@Inject` в REST-ресурсе
- CORS: Jira не добавляет CORS-заголовки по умолчанию — для внешних клиентов нужен servlet-filter

### Как используется в 2026

- REST-эндпоинты в плагинах — основной способ API-интеграции для DC
- JAX-RS (Jersey) остаётся стандартом; переход на другие фреймворки невозможен из-за OSGi
- Для CORS рекомендуется использовать Atlassian CORS Plugin или собственный фильтр

> **На собеседовании:** покажите знание JAX-RS аннотаций (`@Path`, `@GET`, `@POST`, `@Consumes`, `@Produces`). Обязательно упомяните проверку аутентификации и авторизации в каждом эндпоинте — по умолчанию все эндпоинты требуют аутентификацию, `@AnonymousAllowed` — для открытых. Всегда возвращайте DTO, не AO-entity.

[к оглавлению](#разработка-для-jira)

---

## Как создать Servlet Module в плагине?
<!-- grade: middle -->

Servlet Module позволяет создавать веб-страницы внутри Jira с собственным UI, используется для административных панелей, конфигурационных страниц плагина, отчётов.

### Объявление и реализация

```xml
<servlet key="config-servlet" class="com.example.plugin.servlet.ConfigServlet">
    <url-pattern>/my-plugin/config</url-pattern>
</servlet>
```

<details>
<summary>Код ConfigServlet</summary>

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
        ApplicationUser user = authContext.getLoggedInUser();
        if (user == null) {
            resp.sendRedirect("/login.jsp");
            return;
        }

        if (!permissionManager.hasPermission(GlobalPermissionKey.ADMINISTER, user)) {
            resp.sendError(HttpServletResponse.SC_FORBIDDEN, "Admin access required");
            return;
        }

        Map<String, Object> context = new HashMap<>();
        context.put("configs", configService.findAll());
        context.put("currentUser", user.getDisplayName());

        resp.setContentType("text/html;charset=UTF-8");
        templateRenderer.render("templates/config-page.vm", context, resp.getWriter());
    }
}
```

</details>

### Velocity-шаблон

<details>
<summary>templates/config-page.vm</summary>

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

</details>

URL доступа: `https://jira.company.com/plugins/servlet/my-plugin/config`

### Частые ошибки

- Забыть декоратор `atl.admin` — страница отображается без навигации Jira
- XSS-уязвимости — не экранировать пользовательский ввод. Используйте `$!{htmlUtil.htmlEncode($value)}`
- Не проверять права доступа — сервлет доступен любому аутентифицированному пользователю
- Использование ComponentAccessor вместо DI — антипаттерн, затрудняет тестирование

### Как используется в 2026

- Servlet Module остаётся актуальным для административных страниц плагинов
- Тренд: минимум серверного рендеринга, максимум SPA (React/Vue) + REST API плагина
- AUI (Atlassian UI) — по-прежнему рекомендованная UI-библиотека для DC

> **На собеседовании:** покажите знание Velocity-шаблонов и декораторов (`atl.admin`). Обязательно упомяните `$webResourceManager.requireResource(...)` для подключения CSS/JS. Главная ошибка — не проверять права и не экранировать вывод.

[к оглавлению](#разработка-для-jira)

---

## Как создать Custom Field Type?
<!-- grade: senior -->

Custom Field Type позволяет создать собственный тип поля задачи с кастомной логикой ввода, отображения, валидации и поиска.

### Объявление и реализация

<details>
<summary>atlassian-plugin.xml и PriorityScoreField</summary>

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
        if (dbValue == null) return null;
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
        if (s == null || s.trim().isEmpty()) return null;
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

</details>

### Velocity-шаблоны

```html
<!-- templates/fields/priority-score-edit.vm -->
<input type="text" id="$customField.id" name="$customField.id"
       value="$!{value}" class="text short-field" placeholder="0-100"/>
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

### Частые ошибки

- Не реализовать Searcher — поле нельзя использовать в JQL и фильтрах
- Не обрабатывать null/пустые значения в `getSingularObjectFromString()` — NPE при создании задачи
- Тяжёлая логика в `getVelocityParameters()` — вызывается при каждом отображении задачи
- Изменение `getDatabaseType()` после деплоя — несовместимость с существующими данными

### Как используется в 2026

- Custom Field Type остаётся мощным инструментом для DC
- В Cloud кастомные поля создаются через Forge UI Kit или Connect (iframe)
- Тренд на использование JSON-полей (StringLength.UNLIMITED) для сложных структур данных

> **На собеседовании:** покажите понимание жизненного цикла кастомного поля: edit (ввод) -> validate -> persist -> view (отображение). Для простых типов наследуйтесь от AbstractSingleFieldType, для множественных значений — от AbstractMultiCFType. Searcher обязателен для доступа поля в JQL.

[к оглавлению](#разработка-для-jira)

---

## Как создать Workflow Post-Function, Condition, Validator?
<!-- grade: senior -->

Расширения workflow позволяют добавлять автоматическую логику при переходах между статусами в Jira: Post-Function выполняет действие после перехода, Condition определяет видимость перехода, Validator проверяет условия перед выполнением.

### Post-Function

<details>
<summary>Код AutoAssignFunction и Factory</summary>

```xml
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

    @Override
    public void execute(Map transientVars, Map args, PropertySet ps) {
        MutableIssue issue = (MutableIssue) transientVars.get("issue");
        String groupName = (String) args.get("groupName");

        if (groupName == null || groupName.isEmpty()) {
            log.warn("Имя группы не задано для AutoAssignFunction");
            return;
        }

        GroupManager groupManager = ComponentAccessor.getGroupManager();
        Collection<ApplicationUser> members = groupManager.getUsersInGroup(groupName);

        if (!members.isEmpty()) {
            ApplicationUser assignee = members.iterator().next();
            issue.setAssignee(assignee);
            log.info("Задача {} назначена на {}", issue.getKey(),
                    assignee.getDisplayName());
        }
    }
}
```

</details>

### Condition

```java
public class OnlyReporterCondition extends AbstractJiraCondition {

    @Override
    public boolean passesCondition(Map transientVars, Map args, PropertySet ps)
            throws WorkflowException {
        Issue issue = (Issue) transientVars.get("issue");
        ApplicationUser currentUser = getCallerUser(transientVars, args);

        if (issue == null || currentUser == null) return false;
        return currentUser.equals(issue.getReporter());
    }
}
```

### Validator

```java
public class CommentRequiredValidator implements Validator {

    @Override
    public void validate(Map transientVars, Map args, PropertySet ps)
            throws InvalidInputException, WorkflowException {

        int minLength = Integer.parseInt(
                (String) args.getOrDefault("minLength", "10"));

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

### Частые ошибки

- Не вызвать `issue.store()` в Post-Function — изменения не сохраняются
- Бросать RuntimeException в Condition — нужно возвращать false, а не падать
- Тяжёлая логика в Condition — вызывается при каждом отображении задачи для определения доступных переходов
- Порядок Post-Functions важен — встроенные функции Jira (Update Issue, Generate Events) должны идти после кастомных

### Как используется в 2026

- Workflow-расширения — одна из самых востребованных функций DC-плагинов
- В Cloud аналог — Forge jira:workflowCondition, jira:workflowValidator, jira:workflowPostFunction
- ScriptRunner покрывает около 80% типичных workflow-сценариев без написания Java-плагина

> **На собеседовании:** чётко разграничьте три типа: Condition (видимость перехода), Validator (проверка перед), Post-Function (действие после). Factory-класс обязателен — он отвечает за UI конфигурации в admin-панели. В Condition нельзя бросать исключения — только возвращать boolean.

[к оглавлению](#разработка-для-jira)

---

## Как работать с Event Listener в плагине?
<!-- grade: middle -->

Event Listener позволяет реагировать на события в Jira — создание, обновление, удаление задач, комментариев, проектов и других сущностей — через подписку с аннотацией `@EventListener`.

### Реализация через EventPublisher

<details>
<summary>Код IssueEventListener</summary>

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
}
```

</details>

### Кастомные события

```java
// Определение
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

// Публикация
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

// Подписка
@EventListener
public void onTaskProcessed(TaskProcessedEvent event) {
    log.info("Задача {} обработана: {}", event.getIssueKey(), event.getResult());
}
```

### Частые ошибки

- Не дерегистрировать слушатель в `destroy()` — при перезагрузке плагина старый слушатель остаётся активным и вызывается дважды
- Синхронная тяжёлая обработка — замедляет UI Jira для пользователя
- Изменение issue внутри слушателя без `IssueManager.updateIssue()` — изменения теряются или вызывают рекурсивные события
- Не обрабатывать исключения — необработанное исключение в listener может сломать операцию пользователя

### Как используется в 2026

- Event Listener — стандартный механизм реакции на события в DC-плагинах
- В Cloud аналог — Forge Triggers (`avi:jira:created:issue`)
- Для сложных event-driven сценариев применяют паттерн: Jira Event -> Plugin Queue -> Async Processor

> **На собеседовании:** обязательно упомяните register/unregister в afterPropertiesSet/destroy — иначе утечка памяти. Listener вызывается синхронно в том же потоке — тяжёлую логику выносите в отдельный поток. Сравните с Event Listener и не забудьте: `IssueEvent.getEventTypeId()` — числовой ID, сравнивайте с константами EventType.

[к оглавлению](#разработка-для-jira)

---

## Как работает кластеризация в Jira Data Center и как это влияет на плагины?
<!-- grade: senior -->

Jira Data Center — кластерное решение, где несколько нод Jira работают за балансировщиком нагрузки, разделяя общую БД и файловую систему, что предъявляет специфические требования к плагинам.

### Архитектура DC кластера

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
             └──────┬──────┘─────────────┘
                    │
              ┌─────┴─────┐
              │ Shared DB  │     ┌─────────────────┐
              │ (Postgres) │     │ Shared FS (NFS)  │
              └────────────┘     └─────────────────┘
```

### Влияние на плагины

```java
// НЕПРАВИЛЬНО — состояние в памяти ноды
@Named
public class BadService {
    // Этот кэш локальный — данные рассинхронизируются между нодами
    private final Map<String, Config> localCache = new ConcurrentHashMap<>();
}

// ПРАВИЛЬНО — данные в общей БД или распределённом кэше
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

### Распределённые блокировки (ClusterLockService)

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

### Чек-лист кластерной совместимости

| Требование | Проверка |
|---|---|
| Нет in-memory state | Все данные в БД или distributed cache |
| Нет локальных файлов | Используется shared filesystem или БД |
| Нет java.util.Timer | Используется SAL PluginScheduler или ClusterLockService |
| Нет статических кэшей | Используется CacheManager |
| Scheduled tasks идемпотентны | Безопасно выполнить на любой ноде |
| Plugin settings | Через SAL PluginSettingsFactory (хранятся в БД) |

### Частые ошибки

- Локальный ConcurrentHashMap как кэш — рассинхронизация между нодами
- java.util.Timer или ScheduledExecutorService — задача выполняется на каждой ноде одновременно
- Запись во временные локальные файлы — другая нода их не видит
- Не тестировать на кластере из 2+ нод перед релизом

### Как используется в 2026

- Кластерная совместимость — обязательное требование для публикации на Atlassian Marketplace
- Типичные кластеры: 2-4 ноды для средних компаний, 8+ нод для крупных enterprise
- Atlassian предоставляет DC Performance Toolkit для нагрузочного тестирования плагинов в кластере
- Zero-downtime upgrades предъявляют дополнительные требования к обратной совместимости плагинов

> **На собеседовании:** это senior-вопрос, показывающий понимание распределённых систем. Ключевое: stateless-дизайн, CacheManager вместо ConcurrentHashMap, ClusterLockService для эксклюзивных задач. Маркер `atlassian-data-center-compatible=true` обязателен. Hazelcast — движок распределённого кэша и messaging в Jira DC.

[к оглавлению](#разработка-для-jira)

---

## Как тестировать плагины Jira DC?
<!-- grade: middle -->

Тестирование плагинов Jira DC включает несколько уровней: юнит-тесты (Mockito), интеграционные тесты в контейнере Jira и UI-тесты.

### Юнит-тесты (Mockito)

```java
public class TaskConfigServiceTest {

    @Mock private ActiveObjects ao;
    @Mock private TaskConfig mockConfig;
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
    }
}
```

### Интеграционные тесты (@Wired)

```java
@RunWith(AtlassianPluginsTestRunner.class)
public class TaskConfigServiceWiredTest {

    @Inject private TaskConfigService configService;
    @Inject private ActiveObjects ao;

    @Test
    public void testCreateAndFindConfig() {
        TaskConfig created = configService.create("TEST", "{\"test\":true}");
        assertNotNull(created);

        TaskConfig[] found = configService.findByProject("TEST");
        assertEquals(1, found.length);
    }
}
```

### Команды запуска тестов

```bash
atlas-unit-test          # Юнит-тесты
atlas-integration-test   # Интеграционные тесты (запуск Jira + деплой плагина)
```

### Частые ошибки

- Тестирование только через UI (ручное) — не масштабируется, регрессии не ловятся
- Мокирование ComponentAccessor — лучше использовать DI и мокировать инжектированные сервисы
- Не тестировать на версии Jira, близкой к production — API может отличаться
- Не тестировать upgrade tasks — миграция данных ломается при обновлении плагина

### Как используется в 2026

- Atlassian DC Performance Toolkit — для нагрузочного тестирования в кластерном окружении
- Docker-based тестовые окружения стали стандартом (вместо atlas-integration-test для CI/CD)
- Рекомендуемое покрытие: 70%+ юнит-тестами, ключевые сценарии — интеграционными
- Тестирование обратной совместимости при rolling upgrades — требование для DC Marketplace

> **На собеседовании:** покажите знание всех уровней тестирования. Юнит-тесты — обязательный минимум, мокайте все Jira-сервисы через Mockito. Интеграционные тесты медленные (запуск Jira), но ловят проблемы OSGi-совместимости. Используйте atlas-debug + IDE breakpoints для отладки.

[к оглавлению](#разработка-для-jira)

---

## Как отлаживать и профилировать плагины?
<!-- grade: middle -->

Отладка и профилирование — критически важные навыки при разработке плагинов Jira DC, поскольку плагин работает внутри JVM Jira, что создаёт специфические вызовы.

### Remote Debug

```bash
# Запуск Jira с debug-портом
atlas-debug
# Jira стартует с: -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005

# В IDE: Run → Edit Configurations → Remote JVM Debug
# Host: localhost, Port: 5005
```

### Логирование (SLF4J)

```java
@Named
public class MyService {
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

### Типичные проблемы и диагностика

| Проблема | Симптом | Диагностика |
|---|---|---|
| Memory leak | OOM после нескольких дней | Heap dump -> MAT -> Dominator tree |
| Thread leak | Увеличение числа потоков | Thread dump -> ищем потоки плагина |
| Slow query | Медленные страницы | SQL logging -> анализ запросов AO |
| ClassNotFoundException | Ошибка загрузки плагина | Проверить Import-Package в MANIFEST.MF |
| Plugin не стартует | Статус Disabled в UPM | Логи: atlassian-jira.log -> ошибки OSGi |

### Частые ошибки

- Оставить уровень DEBUG в production — логи переполняют диск
- Использование System.out.println() вместо SLF4J — вывод теряется или идёт в неправильный файл
- Не делать heap dump при первом OOM — проблема воспроизводится с трудом
- Профилирование на dev-окружении с маленьким датасетом — не отражает production-нагрузку

### Как используется в 2026

- JFR (Java Flight Recorder) — встроенный профайлер JVM, zero-overhead в production
- Atlassian DC Performance Toolkit интегрирует JMeter + JFR для end-to-end профилирования
- Observability: Prometheus + Grafana для мониторинга метрик плагинов в production
- Distributed tracing (OpenTelemetry) начинает применяться для DC-кластеров

> **На собеседовании:** atlas-debug — основной инструмент повседневной разработки. Логи Jira: `<JIRA_HOME>/log/atlassian-jira.log`. Thread dump — первый инструмент при зависании Jira, Heap dump — при OOM. Покажите знание JFR для production-профилирования.

[к оглавлению](#разработка-для-jira)

---

## Что такое Forge и как создать Forge-приложение?
<!-- grade: middle -->

Forge — serverless-платформа Atlassian для разработки Cloud-приложений, где код выполняется в управляемой Atlassian среде (FaaS), что обеспечивает безопасность и простоту развёртывания.

> **Аналогия из жизни:** Forge — это как квартира-студия с мебелью от застройщика. Вы заезжаете и работаете (деплоите код), а управляющая компания (Atlassian) отвечает за отопление, электричество и уборку подъезда (инфраструктуру, безопасность, масштабирование).

### Ключевые характеристики

- Runtime: Node.js (TypeScript/JavaScript)
- Хостинг: Atlassian (не нужен свой сервер)
- Изоляция: каждое приложение работает в sandbox
- Ограничения: 25 секунд timeout, 512MB RAM на invocation

### Создание приложения

```bash
npm install -g @forge/cli
forge login
forge create
# Выбрать: Jira → jira-issue-panel (или другой шаблон)
```

### manifest.yml

<details>
<summary>Полный пример manifest.yml</summary>

```yaml
modules:
  jira:issuePanel:
    - key: my-issue-panel
      title: Дополнительная информация
      resource: main
      resolver:
        function: resolver

  jira:workflowPostFunction:
    - key: my-post-function
      name: Автоназначение
      function: workflowFunction

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

resources:
  - key: main
    path: src/frontend/index.jsx

permissions:
  scopes:
    - read:jira-work
    - write:jira-work
    - read:jira-user

app:
  runtime:
    name: nodejs18.x
  id: ari:cloud:ecosystem::app/xxxxxxxx
```

</details>

### Backend (Resolver)

```typescript
import Resolver from '@forge/resolver';
import api, { route } from '@forge/api';
import { storage } from '@forge/api';

const resolver = new Resolver();

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

export const handler = resolver.getDefinitions();
```

### Деплой и установка

```bash
forge deploy          # Деплой кода в Atlassian Cloud
forge install         # Установка на конкретный Jira Cloud site
forge tunnel          # Режим разработки (проксирование на локальный код)
forge logs            # Просмотр логов приложения
```

### Частые ошибки

- Превышение лимита 25 секунд на invocation — тяжёлую логику нужно разбивать на части
- Не указать необходимые scopes в manifest.yml — API-вызовы возвращают 403
- Использование console.log для отладки в production — логи ограничены 4 KB
- Не обработать ошибки в resolver — пользователь видит generic error message

### Как используется в 2026

- Forge стал зрелой платформой: Custom UI, Storage API, Entity Storage, Async Events, Scheduled Triggers
- Node.js 18+ — текущий runtime
- UI Kit 2 — актуальная версия с улучшенным набором компонентов
- Forge поддерживает multi-product apps (Jira + Confluence + Bitbucket)

> **На собеседовании:** подчеркните, что Forge — рекомендуемый Atlassian фреймворк для новых Cloud-приложений. Два варианта UI: UI Kit (декларативный, проще) и Custom UI (React, полная свобода). `api.asUser()` — от имени пользователя, `api.asApp()` — от имени приложения. `forge tunnel` — незаменим при разработке.

[к оглавлению](#разработка-для-jira)

---

## Что такое Connect и чем он отличается от Forge?
<!-- grade: middle -->

Atlassian Connect — фреймворк для разработки Cloud-приложений, где приложение хостится на собственном сервере разработчика и интегрируется с Jira через iframe и REST API.

### Архитектура Connect

```
┌──────────────────┐              ┌──────────────────┐
│   Jira Cloud     │   iframe     │  Connect App     │
│                  │ ←──────────→ │  (ваш сервер)    │
│  ┌────────────┐  │              │                  │
│  │ iframe     │  │   REST API   │  Spring Boot /   │
│  │ (app UI)   │  │ ←──────────→ │  Node.js / ...   │
│  └────────────┘  │              │                  │
│  JWT Auth        │   Webhooks   │  PostgreSQL /     │
└──────────────────┘ ────────────→│  MongoDB / ...    │
                                  └──────────────────┘
```

### Сравнение Forge vs Connect

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

### Lifecycle callbacks (Spring Boot)

```java
@RestController
@RequestMapping("/api/lifecycle")
public class LifecycleController {

    private final TenantService tenantService;

    @PostMapping("/installed")
    public ResponseEntity<Void> installed(@RequestBody InstallPayload payload) {
        tenantService.register(
                payload.getClientKey(),
                payload.getSharedSecret(),
                payload.getBaseUrl(),
                payload.getProductType()
        );
        return ResponseEntity.ok().build();
    }

    @PostMapping("/uninstalled")
    public ResponseEntity<Void> uninstalled(@RequestBody UninstallPayload payload) {
        tenantService.unregister(payload.getClientKey());
        return ResponseEntity.ok().build();
    }
}
```

### Частые ошибки

- Не реализовать lifecycle callbacks — приложение не знает о новых/удалённых инстансах
- Хранить shared secret в plain text — компрометация даёт полный доступ к Jira-инстансу
- Не обрабатывать мультитенантность — данные одного клиента утекают другому
- Не проверять JWT в каждом запросе — открытый API без аутентификации

### Как используется в 2026

- Connect остаётся востребованным для enterprise-приложений с собственной инфраструктурой
- Atlassian не deprecated Connect, но рекомендует Forge для новых приложений
- Популярный стек: Spring Boot + Connect + PostgreSQL + React (в iframe)
- Миграция Connect -> Forge возможна, но трудоёмка из-за различий в архитектуре

> **На собеседовании:** покажите понимание архитектурных различий. Connect — правильный выбор, когда нужен полный контроль над стеком, собственная БД, тяжёлые вычисления. Мультитенантность — ключевой вызов: Connect-приложение обслуживает множество Jira-инстансов. JWT shared secret критически важен — хранить в Vault/KMS.

[к оглавлению](#разработка-для-jira)

---

## Как работает аутентификация и авторизация в Jira Cloud приложениях?
<!-- grade: senior -->

Аутентификация и авторизация в Jira Cloud зависят от типа приложения (Forge, Connect) и сценария использования: OAuth 2.0 для user context, API Tokens для простых интеграций, JWT для Connect.

### OAuth 2.0 (3-legged) — для user context

```
User → Redirect → Atlassian Auth Server
     ← Login + Consent →
     ← Callback + code
     → Exchange code → token
     ← Access token + Refresh token
```

<details>
<summary>Spring Boot OAuth 2.0 конфигурация</summary>

```java
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

</details>

### Forge — автоматическая аутентификация

```typescript
import api, { route } from '@forge/api';

// От имени пользователя (scopes из manifest.yml)
const userResponse = await api.asUser().requestJira(route`/rest/api/3/myself`);

// От имени приложения
const appResponse = await api.asApp().requestJira(route`/rest/api/3/issue/PROJ-123`);
```

### Scopes (области доступа)

| Scope | Описание |
|---|---|
| `read:jira-work` | Чтение задач, проектов, досок |
| `write:jira-work` | Создание/обновление задач |
| `read:jira-user` | Чтение профилей пользователей |
| `manage:jira-project` | Управление проектами |
| `manage:jira-configuration` | Управление конфигурацией |

### Частые ошибки

- Использование API Token в серверном приложении для нескольких пользователей — все действия от одного пользователя
- Не обновлять refresh token — пользователю приходится переавторизовываться
- Запрос избыточных scopes — Atlassian может отклонить приложение при review
- Хранение client secret и shared secret в коде — используйте переменные окружения

### Как используется в 2026

- OAuth 2.0 — единственный рекомендуемый метод аутентификации для Cloud-приложений
- Basic Auth полностью deprecated для Cloud (работает только с API tokens)
- Atlassian вводит granular scopes для более точного контроля доступа
- PKCE рекомендуется для public-клиентов (SPA, мобильные)

> **На собеседовании:** разграничьте аутентификацию по типу приложения: в Forge она автоматическая (asUser/asApp), в Connect — JWT с shared secret, для внешних интеграций — OAuth 2.0. API Tokens подходят только для скриптов. Refresh tokens обновляйте до истечения срока (обычно 90 дней).

[к оглавлению](#разработка-для-jira)

---

## Что такое Forge Storage API и Custom UI?
<!-- grade: middle -->

Forge Storage API — встроенное хранилище данных для Forge-приложений, позволяющее сохранять данные без собственной БД. Custom UI — возможность использовать полноценное React-приложение вместо декларативного UI Kit.

### Типы хранилища

| Тип | Лимит | Назначение |
|---|---|---|
| Key-Value (storage) | 32 KB на ключ | Конфигурация, маленькие данные |
| Entity Storage | 32 KB на entity, queryable | Структурированные данные с поиском |
| Secrets | 4 KB | Токены, пароли |

### Key-Value Storage

```typescript
import { storage } from '@forge/api';

await storage.set('config:PROJ', { enabled: true, maxRetries: 3 });
const config = await storage.get('config:PROJ');
await storage.delete('config:PROJ');

// Список ключей (с пагинацией)
const result = await storage.query()
    .where('key', startsWith('config:'))
    .limit(20)
    .getMany();
```

### Entity Storage (структурированное хранилище)

```typescript
// Сохранение entity
await storage.entity('task-record').set('task-001', {
    projectKey: 'PROJ',
    status: 'active',
    score: 85,
    createdAt: Date.now()
});

// Поиск по атрибутам
const activeTasks = await storage.entity('task-record')
    .query()
    .index('projectKey')
    .where('projectKey', 'PROJ')
    .sort('createdAt', 'DESC')
    .limit(50)
    .getMany();
```

### Custom UI (React-приложение)

```typescript
// static/my-custom-ui/src/App.tsx
import React, { useEffect, useState } from 'react';
import { invoke, view } from '@forge/bridge';
import Button from '@atlaskit/button';

const App: React.FC = () => {
    const [details, setDetails] = useState(null);

    useEffect(() => {
        invoke('getIssueDetails').then(setDetails);
    }, []);

    return (
        <div style={{ padding: '16px' }}>
            <h3>{details?.summary}</h3>
            <p>Статус: {details?.status}</p>
            <Button appearance="primary" onClick={() => invoke('saveScore')}>
                Сохранить
            </Button>
        </div>
    );
};
```

### Bridge API (@forge/bridge)

```typescript
import { invoke, view, router } from '@forge/bridge';

const result = await invoke('functionName', { param1: 'value' });
const context = await view.getContext();
const issueKey = context.extension.issue.key;
await router.navigate('/browse/PROJ-123');
```

### Частые ошибки

- Хранение большого объёма данных в Storage (>32KB) — ошибка при записи
- Не использовать Entity Storage для данных, по которым нужен поиск — Key-Value не поддерживает query
- Не собирать Custom UI перед forge deploy — забыть `npm run build` в static/
- Прямые вызовы Jira REST API из Custom UI (frontend) — нужно через resolver (backend)

### Как используется в 2026

- Entity Storage стал основным способом хранения данных в Forge
- Custom UI + Atlaskit — стандарт для сложных UI
- Forge Remote Backend — позволяет вызывать внешние серверы из Forge
- Storage API получил улучшенные лимиты и возможности запросов

> **На собеседовании:** разграничьте Key-Value (простое, без поиска) и Entity Storage (с индексами и query). Custom UI даёт полную свободу дизайна через React + Atlaskit, UI Kit проще для стандартных сценариев. Все вызовы API из Custom UI идут через resolver (backend), а не напрямую.

[к оглавлению](#разработка-для-jira)

---

## Как обрабатывать события в Jira Cloud?
<!-- grade: middle -->

Обработка событий в Jira Cloud основана на Forge Triggers (рекомендуемый подход) и Connect Webhooks, в отличие от синхронных Event Listener в DC.

### Forge Triggers

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
    - key: scheduled-task
      function: scheduledHandler
      interval: hour  # minute, hour, day, week
```

<details>
<summary>Код обработчиков событий</summary>

```typescript
export async function onIssueCreated(event: any, context: any) {
    const { issue } = event;
    console.log(`Issue created: ${issue.key}`);

    if (issue.fields.issuetype.name === 'Bug'
        && issue.fields.priority.name === 'Critical') {

        await api.asApp().requestJira(
            route`/rest/api/3/issue/${issue.key}`,
            {
                method: 'PUT',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    update: { labels: [{ add: 'critical-auto' }] }
                })
            }
        );
    }
}

export async function onIssueUpdated(event: any, context: any) {
    const { issue, changelog } = event;
    if (!changelog?.items) return;

    for (const change of changelog.items) {
        if (change.field === 'status' && change.toString === 'Done') {
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

export async function onScheduled(event: any) {
    const response = await api.asApp().requestJira(route`/rest/api/3/search`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            jql: 'duedate < now() AND status != Done',
            maxResults: 50
        })
    });
    const data = await response.json();
    console.log(`Просроченных задач: ${data.total}`);
}
```

</details>

### Различия обработки событий DC vs Cloud

| Аспект | DC (Event Listener) | Cloud (Forge Trigger) | Cloud (Connect Webhook) |
|---|---|---|---|
| Вызов | Синхронно в JVM | Async (serverless) | HTTP POST |
| Доставка | Гарантированная | At-least-once | At-least-once |
| Порядок | Гарантированный | Не гарантированный | Не гарантированный |
| Timeout | Нет (в JVM) | 25 секунд | Зависит от сервера |
| Retry | Нет (свой) | Автоматический (3x) | Ограниченный |

### Частые ошибки

- Синхронная тяжёлая обработка в Forge trigger — превышение 25-секундного лимита
- Предположение о порядке событий — issue_updated может прийти раньше issue_created
- Не обрабатывать дубликаты — проверяйте, было ли событие уже обработано
- Неиспользование фильтров в Connect webhooks — получение всех событий перегружает сервер

### Как используется в 2026

- Forge Triggers стали основным механизмом для Cloud-приложений
- Atlassian улучшил гарантии доставки и добавил dead-letter queue для failed triggers
- Async Events в Forge — для цепочек обработки (trigger -> async function -> async function)
- Scheduled triggers ограничены: minute, hour, day, week — нет cron-выражений

> **На собеседовании:** сравните три подхода: DC Event Listener (синхронный, в JVM), Forge Trigger (async, serverless, 25s лимит), Connect Webhook (HTTP POST, свой сервер). Ключевое: idempotent-обработка обязательна — события at-least-once, порядок не гарантирован.

[к оглавлению](#разработка-для-jira)

---

## Какие ограничения и лимиты существуют в Jira Cloud API?
<!-- grade: middle -->

При работе с Jira Cloud API необходимо учитывать многочисленные ограничения, которые влияют на архитектуру приложения: rate limiting, пагинацию, Forge-лимиты.

### Rate Limiting

| Тип | Лимит | Описание |
|---|---|---|
| Per-user | ~100 requests/min | Зависит от эндпоинта и плана |
| Per-app (Connect) | ~500 requests/min | На один Jira site |
| Per-app (Forge) | ~100 requests/min | Product API calls |
| Concurrent | 10 | Одновременные запросы от приложения |

### Пагинация

| Эндпоинт | Max maxResults | Default |
|---|---|---|
| /search (JQL) | 100 | 50 |
| /project | 50 | 50 |
| /user/search | 1000 | 50 |
| /board/{id}/issue | 100 | 50 |

### Forge-специфичные лимиты

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

### Обработка rate limiting

```java
// Java (Connect/Integration)
@Component
public class RateLimitedJiraClient {

    private final RetryTemplate retryTemplate;

    public RateLimitedJiraClient(RestClient restClient) {
        this.retryTemplate = RetryTemplate.builder()
                .maxAttempts(5)
                .retryOn(RateLimitException.class)
                .exponentialBackoff(1000, 2.0, 30000)
                .build();
    }
}
```

### Частые ошибки

- Цикл из 1000 REST-запросов по одному — мгновенно упираемся в rate limit
- Игнорирование пагинации — получение первых 50 результатов как будто это все
- Хранение >32KB в одном ключе Storage — ошибка записи
- Синхронная обработка тяжёлой логики в 25-секундном окне Forge

### Как используется в 2026

- Atlassian постепенно увеличивает лимиты, но базовые ограничения остаются
- Forge Async Events позволяют обходить 25s лимит через цепочку invocations
- Atlassian рекомендует batch/bulk подход: минимум API-вызовов, максимум данных за вызов
- Мониторинг usage и rate limits через Atlassian Developer Console

> **На собеседовании:** покажите знание конкретных цифр: 100 requests/min per user, maxResults=100 для /search, 25s timeout Forge. Retry-After header — ваш друг. Архитектура Cloud-приложения должна учитывать лимиты с самого начала. Используйте bulk-операции вместо одиночных запросов в цикле.

[к оглавлению](#разработка-для-jira)

---

## Как проектировать приложение, совместимое с DC и Cloud?
<!-- grade: senior -->

Создание dual-platform приложения — сложная задача, требующая архитектурного разделения платформенно-зависимого и бизнес-кода через общие интерфейсы и отдельные адаптеры для каждой платформы.

### Архитектурный подход

```
┌──────────────────────────────────────────────┐
│              Shared Business Logic            │
│         (Java library / shared module)        │
│  ┌─────────────┐  ┌──────────┐  ┌──────────┐ │
│  │ Domain Model│  │ Business │  │  Utils   │ │
│  │  (POJOs)    │  │  Rules   │  │          │ │
│  └─────────────┘  └──────────┘  └──────────┘ │
└──────────┬──────────────────┬────────────────┘
           │                  │
    ┌──────┴──────┐    ┌──────┴──────┐
    │  DC Adapter │    │Cloud Adapter│
    │ P2 Plugin   │    │Forge / Conn.│
    │ Active Obj. │    │ Storage API │
    │ Jira Java   │    │ REST API v3 │
    │ Velocity UI │    │ UI Kit/React│
    └─────────────┘    └─────────────┘
```

### Пример: абстракция над Jira API

```java
// Общий интерфейс (shared module)
public interface JiraIssueAdapter {
    IssueDto getIssue(String key);
    String createIssue(CreateIssueRequest request);
    List<IssueDto> searchByJql(String jql, int maxResults);
}

public record IssueDto(
        String key, String summary, String status,
        String assignee, String projectKey,
        Map<String, Object> customFields
) {}

// DC реализация — Jira Java API напрямую
@Named
public class DcJiraIssueAdapter implements JiraIssueAdapter {
    @Inject
    public DcJiraIssueAdapter(@ComponentImport IssueManager issueManager,
                              @ComponentImport SearchService searchService) {
        // ...
    }
}

// Cloud реализация — REST API v3 через Forge
// (TypeScript, отдельный проект)
```

### Стратегия миграции данных

```
DC (Active Objects)          Cloud (Forge Storage)
┌──────────────────┐         ┌──────────────────┐
│  AO Table        │  export │  Entity Storage  │
│  - ID            │ ──────→ │  - key           │
│  - PROJECT_KEY   │         │  - projectKey    │
│  - CONFIG        │  import │  - config        │
└──────────────────┘         └──────────────────┘
```

### Частые ошибки

- Попытка создать единый codebase для DC и Cloud — языки и рантайм разные (Java vs TypeScript)
- Игнорирование различий API (v2 vs v3, user identification, ADF vs wiki markup)
- Не учитывать различия в модели данных — AO (реляционная) vs Storage (key-value)
- Копировать поведение DC один-в-один в Cloud без учёта Cloud-специфики (rate limits, timeouts)

### Как используется в 2026

- Dual-platform — маркетинговое преимущество на Atlassian Marketplace
- Atlassian предоставляет Migration API для помощи в переносе данных DC -> Cloud
- Крупные вендоры (Tempo, Xray, ScriptRunner) поддерживают обе платформы
- Тренд: DC как legacy support, Cloud как primary платформа для новых фич

> **На собеседовании:** покажите зрелое архитектурное мышление. Dual-platform = два отдельных проекта с общей бизнес-логикой, а не один универсальный. Java shared library для бизнес-правил, адаптеры для каждой платформы. Feature parity — осознанный выбор, не все функции воспроизводимы на обеих платформах.

[к оглавлению](#разработка-для-jira)

---

## Какие есть best practices при разработке для Jira?
<!-- grade: middle -->

Best practices при разработке для Jira — набор рекомендаций по производительности, безопасности, UX и обратной совместимости, применимых к обеим платформам.

### Производительность

```java
// ПЛОХО — загрузка всех задач проекта в память
Collection<Long> allIssueIds = issueManager.getIssueIdsForProject(project.getId());
// Может быть 100K+ задач!

// ХОРОШО — пагинация + ленивая загрузка
String jql = "project = " + projectKey + " ORDER BY created DESC";
Query query = jqlQueryParser.parseQuery(jql);
PagerFilter pager = new PagerFilter(page * pageSize, pageSize);
SearchResults results = searchService.search(user, query, pager);
```

### Безопасность

```java
// ПЛОХО — SQL injection в AO
ao.find(TaskConfig.class,
        Query.select().where("PROJECT_KEY = '" + projectKey + "'"));

// ХОРОШО — параметризованный запрос
ao.find(TaskConfig.class,
        Query.select().where("PROJECT_KEY = ?", projectKey));

// В Velocity шаблонах: $!{htmlUtil.htmlEncode($userInput)}
```

### Конфигурация плагина (DC)

```java
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

### Частые ошибки

- Пропуск валидации ввода — XSS, SQL injection, path traversal
- Отсутствие error handling — пользователь видит stack trace вместо понятного сообщения
- Блокирование потоков UI — тяжёлые операции должны быть асинхронными с progress bar
- Жёсткая привязка к конкретной версии Jira API — не работает после обновления

### Как используется в 2026

- Atlassian Design System (ADS) — единая система дизайна для DC и Cloud
- Observability (Prometheus, OpenTelemetry) — must-have для production плагинов
- Feature flags — безопасный rollout новых функций плагина
- Автоматизированное тестирование на матрице версий Jira (LTS versions)

> **На собеседовании:** структурируйте ответ по категориям: производительность (пагинация, кэширование), безопасность (параметризованные запросы, XSS), UX (AUI для DC, Atlaskit для Cloud), обратная совместимость (upgrade tasks). Никогда не загружайте все данные в память — используйте пагинацию.

[к оглавлению](#разработка-для-jira)

---

## Что такое ScriptRunner и когда его использовать?
<!-- grade: junior -->

ScriptRunner — один из самых популярных плагинов для Jira Data Center, позволяющий расширять Jira без написания полноценного Java-плагина через Groovy-скрипты с полным доступом к внутреннему API Jira.

> **Аналогия из жизни:** ScriptRunner — это как швейцарский нож для Jira-администратора. Вместо того чтобы заказывать специальный инструмент (писать Java-плагин), вы достаёте ножик и решаете задачу прямо на месте за минуты.

### Возможности ScriptRunner для DC

| Функция | Описание |
|---|---|
| Script Listeners | Реакция на события Jira |
| Script Fields | Вычисляемые кастомные поля |
| Behaviours | Динамическое управление формами |
| Script Post-Functions | Workflow post-functions на Groovy |
| Script Conditions | Workflow conditions |
| Script Validators | Workflow validators |
| Script Console | Выполнение Groovy-скриптов ad-hoc |
| REST Endpoints | Создание REST API на Groovy |
| Escalation Services | Периодические задания |

### Пример: Script Listener

```groovy
import com.atlassian.jira.component.ComponentAccessor
import com.atlassian.jira.event.issue.IssueEvent
import com.atlassian.jira.issue.MutableIssue

def event = event as IssueEvent
def issue = event.issue as MutableIssue

if (issue.issueType.name == 'Bug' && issue.priority.name == 'Critical') {
    def currentLabels = issue.labels
    currentLabels.add('critical-auto')
    issue.setLabels(currentLabels)
    issueManager.updateIssue(event.user, issue,
            com.atlassian.jira.event.type.EventDispatchOption.DO_NOT_DISPATCH, false)
}
```

### Пример: Script Field (вычисляемое поле)

```groovy
import java.time.LocalDate
import java.time.temporal.ChronoUnit

def created = issue.created.toInstant()
        .atZone(java.time.ZoneId.systemDefault()).toLocalDate()
return ChronoUnit.DAYS.between(created, LocalDate.now())
```

### Когда ScriptRunner достаточно vs когда нужен плагин

| Критерий | ScriptRunner | Кастомный плагин |
|---|---|---|
| Сложность логики | Простая-средняя | Высокая |
| UI | Минимальный | Полноценный |
| Модель данных | Поля Jira / PluginSettings | Active Objects (собственные таблицы) |
| Тестирование | Ручное / Script Console | JUnit, интеграционные тесты |
| CI/CD | Ограниченно | Полноценный |
| Время разработки | Минуты-часы | Дни-недели |

### Частые ошибки

- Groovy-скрипты без error handling — ошибка в listener может сломать операции пользователей
- Тяжёлая логика в Script Field — поле вычисляется при каждом отображении задачи
- Использование ComponentAccessor без проверки результата на null
- Скрипты, зависящие от имён полей/статусов — ломаются при переименовании (используйте ID)

### Как используется в 2026

- ScriptRunner DC — по-прежнему самый популярный способ быстрой кастомизации
- ScriptRunner Cloud значительно уступает DC-версии (JavaScript, нет Script Console, нет Behaviours)
- Тренд: ScriptRunner для быстрого прототипирования, миграция на плагин при росте сложности
- Для Cloud рекомендуют Jira Automation для простых сценариев вместо ScriptRunner Cloud

> **На собеседовании:** подчеркните, что ScriptRunner покрывает 70-80% задач кастомизации без написания Java-плагина. Это инструмент номер один на Atlassian Marketplace. Для Cloud-версии возможности значительно ограничены. Ключевое правило: если задача решается ScriptRunner-скриптом за 20 минут — не пишите плагин.

[к оглавлению](#разработка-для-jira)

---

**Источники:**
- [Atlassian Developer Documentation](https://developer.atlassian.com/)
- [Atlassian SDK Documentation](https://developer.atlassian.com/server/framework/atlassian-sdk/)
- [Forge Documentation](https://developer.atlassian.com/platform/forge/)
- [Connect Documentation](https://developer.atlassian.com/cloud/jira/platform/about-connect/)
- [ScriptRunner Documentation](https://docs.adaptavist.com/sr4js/latest)

[Вопросы для собеседования](README.md)
