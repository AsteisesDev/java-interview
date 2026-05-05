[Вопросы для собеседования](README.md)

# Кэширование
+ [Что такое кэширование и зачем оно нужно?](#Что-такое-кэширование-и-зачем-оно-нужно)
+ [Какие паттерны кэширования существуют?](#Какие-паттерны-кэширования-существуют)
+ [Что такое Redis и когда его использовать?](#Что-такое-Redis-и-когда-его-использовать)
+ [Какие структуры данных поддерживает Redis?](#Какие-структуры-данных-поддерживает-Redis)
+ [Как настроить Redis в Spring Boot?](#Как-настроить-Redis-в-Spring-Boot)
+ [Какие стратегии инвалидации кэша существуют?](#Какие-стратегии-инвалидации-кэша-существуют)
+ [Что такое многоуровневое кэширование (L1 + L2)?](#Что-такое-многоуровневое-кэширование-L1--L2)
+ [Что такое Cache Stampede и как его предотвратить?](#Что-такое-Cache-Stampede-и-как-его-предотвратить)
+ [Как работает HTTP-кэширование?](#Как-работает-HTTP-кэширование)
+ [Что такое Caffeine Cache?](#Что-такое-Caffeine-Cache)
+ [Как обеспечить согласованность кэша в распределённой системе?](#Как-обеспечить-согласованность-кэша-в-распределённой-системе)
+ [Какие политики вытеснения (eviction) существуют?](#Какие-политики-вытеснения-eviction-существуют)

## Что такое кэширование и зачем оно нужно?

**Кэширование** — хранение результатов дорогих операций (запросы к БД, вычисления, внешние API) в быстром хранилище для повторного использования без повторного вычисления.

**Зачем:**
- Снижение latency — чтение из памяти (~1 мкс) вместо чтения из БД (~1-10 мс) или вызова API (~50-500 мс)
- Снижение нагрузки на БД и внешние сервисы
- Повышение throughput — один экземпляр приложения может обслуживать больше запросов

**Где кэшировать (уровни):**

```
Клиент (браузер)  →  CDN  →  API Gateway  →  Приложение (L1 in-memory)  →  Распределённый кэш (L2 Redis)  →  БД
```

| Уровень | Хранилище | Latency | Примеры |
|---------|-----------|---------|---------|
| Браузер | HTTP Cache | 0 мс (локально) | Cache-Control, ETag |
| CDN | Edge-серверы | 5-50 мс | CloudFront, Cloudflare |
| API Gateway | Встроенный кэш | 1-5 мс | Nginx, Spring Cloud Gateway |
| Приложение (L1) | Heap JVM | <1 мс | Caffeine, Guava, ConcurrentHashMap |
| Распределённый (L2) | Redis, Memcached | 1-5 мс | Spring Data Redis |
| БД | Query Cache | 1-10 мс | PostgreSQL, MySQL |

**Что кэшировать:**
- Результаты частых и одинаковых запросов (справочники, каталоги, профили)
- Результаты дорогих вычислений (агрегации, отчёты)
- Ответы внешних API (курсы валют, геолокация)
- Сессии пользователей, токены

**Что НЕ кэшировать:**
- Часто обновляемые данные (баланс счёта, инвентарь в реальном времени)
- Уникальные данные (каждый запрос уникален — кэш не помогает)
- Конфиденциальные данные (без шифрования)

### Важное
- Кэширование — это **компромисс**: ускорение за счёт возможной неактуальности данных
- Два главных вопроса: **что кэшировать** и **когда инвалидировать**
- «There are only two hard things in computer science: cache invalidation and naming things» — Phil Karlton

### Частые ошибки
- **Кэширование всего подряд** — кэш с hit rate < 50% потребляет память без пользы
- **Отсутствие TTL** — данные в кэше вечно устаревшие
- **Кэширование null-результатов без осознания** — повторные запросы несуществующих ресурсов не доходят до БД, но кэш растёт
- **Не мониторить hit/miss ratio** — без метрик невозможно оценить эффективность

### Как используется в 2026
- Кэширование — обязательная часть production-системы
- Redis — стандарт для распределённого кэша
- Caffeine — стандарт для in-process кэша
- Spring Cache абстракция — декларативный подход через аннотации (см. spring.md)

[к оглавлению](#Кэширование)

## Какие паттерны кэширования существуют?

### Cache-Aside (Lazy Loading)

Самый распространённый паттерн. Приложение управляет кэшем явно: сначала проверяет кэш, при промахе загружает из БД и кладёт в кэш.

```java
public User getUser(Long id) {
    // 1. Проверить кэш
    User cached = cache.get("user:" + id);
    if (cached != null) return cached;

    // 2. Cache miss — загрузить из БД
    User user = userRepository.findById(id).orElseThrow();

    // 3. Положить в кэш
    cache.put("user:" + id, user, Duration.ofMinutes(10));
    return user;
}
```

```java
// Spring Cache реализует Cache-Aside автоматически
@Cacheable(value = "users", key = "#id")
public User getUser(Long id) {
    return userRepository.findById(id).orElseThrow();
}
```

**Плюсы:** кэшируются только запрошенные данные; промах → кэш заполняется.
**Минусы:** первый запрос всегда медленный (cold start); возможна несогласованность при обновлении БД без инвалидации кэша.

### Read-Through

Кэш сам загружает данные из источника при промахе. Приложение всегда обращается к кэшу, не к БД напрямую.

```java
// Caffeine с CacheLoader — Read-Through
LoadingCache<Long, User> cache = Caffeine.newBuilder()
    .maximumSize(10_000)
    .expireAfterWrite(Duration.ofMinutes(10))
    .build(id -> userRepository.findById(id).orElseThrow()); // CacheLoader

// Использование — кэш сам вызовет loader при промахе
User user = cache.get(userId);
```

**Отличие от Cache-Aside:** логика загрузки инкапсулирована в кэше, а не в сервисе.

### Write-Through

При записи данные **одновременно** пишутся и в кэш, и в БД. Кэш всегда содержит актуальные данные.

```java
public User updateUser(Long id, UpdateRequest request) {
    User user = userRepository.save(toEntity(request)); // запись в БД
    cache.put("user:" + id, user);                      // запись в кэш
    return user;
}
```

```java
// Spring Cache — @CachePut реализует Write-Through
@CachePut(value = "users", key = "#result.id")
public User updateUser(UpdateRequest request) {
    return userRepository.save(toEntity(request));
}
```

**Плюсы:** кэш всегда актуален.
**Минусы:** каждая запись медленнее (двойная запись); кэшируются данные, которые могут никогда не быть прочитаны.

### Write-Behind (Write-Back)

Данные сначала пишутся в кэш, а запись в БД происходит **асинхронно** (с задержкой или батчами).

```
Приложение → Кэш (мгновенно) → [асинхронно, батчами] → БД
```

**Плюсы:** максимальная скорость записи; можно агрегировать множество записей в один batch.
**Минусы:** риск потери данных при сбое кэша до синхронизации с БД.
**Применение:** счётчики, метрики, аналитика, write-heavy нагрузка.

### Refresh-Ahead

Кэш проактивно обновляет данные **до истечения TTL** для «горячих» ключей.

```java
// Caffeine с refreshAfterWrite — автоматическое фоновое обновление
LoadingCache<Long, User> cache = Caffeine.newBuilder()
    .maximumSize(10_000)
    .expireAfterWrite(Duration.ofMinutes(30))     // жёсткое удаление через 30 мин
    .refreshAfterWrite(Duration.ofMinutes(10))    // фоновое обновление через 10 мин
    .build(id -> userRepository.findById(id).orElseThrow());
// Через 10 мин: возвращает старое значение + запускает фоновую загрузку нового
// Через 30 мин: ключ удаляется, следующий запрос — полный cache miss
```

**Плюсы:** нет холодных промахов для горячих данных.
**Минусы:** сложнее в реализации; нагрузка на БД для обновления даже незапрашиваемых данных.

### Сравнение паттернов

| Паттерн | Кто читает из БД | Кто пишет в БД | Consistency | Latency чтения | Latency записи |
|---------|-----------------|----------------|-------------|----------------|----------------|
| Cache-Aside | Приложение | Приложение | Eventual | Miss → медленно | Без кэша |
| Read-Through | Кэш | Приложение | Eventual | Miss → медленно | Без кэша |
| Write-Through | Кэш | Кэш + БД синхронно | Strong | Hit → быстро | Медленнее (2 записи) |
| Write-Behind | Кэш | Кэш → БД асинхронно | Eventual | Быстро | Быстро |
| Refresh-Ahead | Кэш (проактивно) | Приложение | Eventual | Всегда быстро | Без кэша |

### Важное
- **Cache-Aside + TTL** — самый распространённый и простой паттерн; Spring `@Cacheable` реализует его
- **Write-Through** — когда нужна согласованность кэша с БД; Spring `@CachePut`
- **Write-Behind** — для write-heavy нагрузок (счётчики, логи)
- **Refresh-Ahead** — для данных, которые должны быть всегда «тёплыми» (каталог товаров, курсы валют)

### Частые ошибки
- **Cache-Aside без инвалидации** — обновили БД, забыли обновить/удалить кэш → stale data
- **Write-Through для редко читаемых данных** — кэш заполняется данными, которые никто не читает
- **Write-Behind без обработки ошибок** — если асинхронная запись в БД упала, данные потеряны
- **Путать паттерны** — Cache-Aside + @CachePut = рабочая комбинация; Read-Through в Spring — через Caffeine `LoadingCache`

### Как используется в 2026
- Cache-Aside — 80%+ всех кэширований в Spring-приложениях
- Write-Behind — используется в Redis Streams и Apache Kafka для буферизации записей
- Caffeine `refreshAfterWrite` — стандарт для проактивного обновления справочных данных

[к оглавлению](#Кэширование)

## Что такое Redis и когда его использовать?

**Redis (Remote Dictionary Server)** — in-memory key-value хранилище данных, используемое как кэш, брокер сообщений и база данных. Хранит данные в оперативной памяти, обеспечивая latency <1 мс.

**Когда использовать Redis:**
- Распределённый кэш (несколько экземпляров приложения)
- Сессии пользователей (вместо sticky sessions)
- Rate limiting (ограничение запросов)
- Распределённые блокировки (distributed locks)
- Очереди задач (Redis Streams)
- Pub/Sub (уведомления в реальном времени)
- Leaderboards, счётчики (атомарные операции)

**Redis vs Memcached:**

| Критерий | Redis | Memcached |
|----------|-------|-----------|
| Структуры данных | String, List, Set, Hash, Sorted Set, Stream... | Только String |
| Персистентность | RDB снимки, AOF лог | Нет |
| Репликация | Master-Replica, Cluster | Нет |
| Pub/Sub | Да | Нет |
| Lua-скрипты | Да | Нет |
| Размер значения | 512 MB | 1 MB |
| Многопоточность | Один поток (I/O с 6.0) | Многопоточный |

**Конфигурация подключения в Spring Boot:**

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
      password: secret
      timeout: 2000ms
      lettuce:
        pool:
          max-active: 16
          max-idle: 8
          min-idle: 4
```

**Работа с Redis через RedisTemplate:**

```java
@Service
public class RedisService {
    private final StringRedisTemplate redisTemplate;

    // Простое key-value
    public void cacheUser(Long id, String json) {
        redisTemplate.opsForValue().set("user:" + id, json, Duration.ofMinutes(10));
    }

    public String getCachedUser(Long id) {
        return redisTemplate.opsForValue().get("user:" + id);
    }

    // Hash — отдельные поля объекта
    public void cacheUserFields(Long id, Map<String, String> fields) {
        redisTemplate.opsForHash().putAll("user:" + id, fields);
        redisTemplate.expire("user:" + id, Duration.ofMinutes(10));
    }

    // Sorted Set — leaderboard
    public void addScore(String player, double score) {
        redisTemplate.opsForZSet().add("leaderboard", player, score);
    }

    public Set<String> getTopPlayers(int count) {
        return redisTemplate.opsForZSet().reverseRange("leaderboard", 0, count - 1);
    }

    // Distributed lock (простой вариант)
    public boolean acquireLock(String key, Duration timeout) {
        return Boolean.TRUE.equals(
            redisTemplate.opsForValue().setIfAbsent("lock:" + key, "1", timeout));
    }

    public void releaseLock(String key) {
        redisTemplate.delete("lock:" + key);
    }
}
```

### Важное
- Redis однопоточный (основной цикл) — атомарность операций гарантирована без блокировок
- Вся БД в RAM — быстро, но ограничено объёмом памяти
- Redis Cluster — горизонтальное масштабирование (шардирование по ключам)
- Redis Sentinel — автоматический failover (master-replica)

### Частые ошибки
- **Хранить в Redis данные без TTL** — память исчерпается; всегда устанавливайте TTL
- **Большие значения (>100 KB)** — замедляют Redis; если нужно хранить большие объекты, используйте сжатие или ссылки
- **Использовать Redis как primary database** — Redis не заменяет PostgreSQL; данные должны быть восстановимы из основной БД
- **`KEYS *` в production** — блокирует Redis; используйте `SCAN` для итерации
- **Распределённая блокировка через `SETNX`** — упрощённая реализация ненадёжна; используйте Redisson или Redlock

### Как используется в 2026
- Redis 7.x — стабильная production-версия; Redis Functions (Lua замена), ACL, multi-threading I/O
- Spring Data Redis + Lettuce — стандартный клиент (вместо Jedis)
- Redis Stack — Redis + поиск (RediSearch), JSON (RedisJSON), Time Series
- Managed Redis: AWS ElastiCache, Azure Cache for Redis, GCP Memorystore

[к оглавлению](#Кэширование)

## Какие структуры данных поддерживает Redis?

| Структура | Команды | Применение |
|-----------|---------|------------|
| **String** | `SET`, `GET`, `INCR`, `DECR`, `MGET` | Кэш, счётчики, сессии |
| **Hash** | `HSET`, `HGET`, `HGETALL`, `HDEL` | Объекты (user:1 → {name, email, age}) |
| **List** | `LPUSH`, `RPUSH`, `LPOP`, `LRANGE` | Очереди, последние N элементов |
| **Set** | `SADD`, `SMEMBERS`, `SINTER`, `SUNION` | Теги, уникальные значения, пересечения |
| **Sorted Set** | `ZADD`, `ZRANGE`, `ZRANK`, `ZRANGEBYSCORE` | Рейтинги, таймлайны, приоритетные очереди |
| **Stream** | `XADD`, `XREAD`, `XREADGROUP` | Event streaming, лог событий, очереди задач |
| **HyperLogLog** | `PFADD`, `PFCOUNT`, `PFMERGE` | Подсчёт уникальных значений (approximate) |
| **Bitmap** | `SETBIT`, `GETBIT`, `BITCOUNT` | Флаги, онлайн-статус, daily active users |

**Практические примеры:**

```bash
# String — кэш с TTL
SET user:1 '{"name":"John","email":"john@mail.com"}' EX 600

# Hash — отдельные поля
HSET user:1 name "John" email "john@mail.com" age 30
HGET user:1 name  # → "John"

# Sorted Set — leaderboard
ZADD leaderboard 100 "player1" 200 "player2" 150 "player3"
ZREVRANGE leaderboard 0 2 WITHSCORES  # топ-3

# List — последние 10 уведомлений
LPUSH notifications:user:1 '{"text":"Новый заказ","time":"..."}'
LTRIM notifications:user:1 0 9  # оставить только последние 10

# HyperLogLog — уникальные посетители (приблизительно, погрешность <1%)
PFADD visitors:2026-04-22 "user1" "user2" "user3"
PFCOUNT visitors:2026-04-22  # → 3

# Stream — event log
XADD orders * orderId 123 status created
XREAD COUNT 10 STREAMS orders 0  # читать с начала
```

### Важное
- **Выбор структуры зависит от задачи:** кэш → String/Hash; рейтинг → Sorted Set; очередь → List/Stream; уникальные → Set/HyperLogLog
- Hash эффективнее String для объектов — можно читать/обновлять отдельные поля без десериализации всего объекта
- Sorted Set — O(log N) для add/remove/rank, идеален для рейтингов и таймлайнов
- Stream — аналог Kafka внутри Redis; consumer groups, acknowledgment

### Частые ошибки
- **String для объектов вместо Hash** — каждое обновление одного поля требует перезаписи всего JSON
- **List вместо Stream для очередей** — Stream поддерживает consumer groups и acknowledgment, List — нет
- **Set для больших коллекций** — `SMEMBERS` возвращает все элементы разом; для больших — `SSCAN`

### Как используется в 2026
- Redis Streams заменяют простые очереди на List
- RedisJSON — нативная работа с JSON без String serialization
- RediSearch — полнотекстовый поиск без Elasticsearch для простых случаев

[к оглавлению](#Кэширование)

## Как настроить Redis в Spring Boot?

**Зависимости:**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**Конфигурация `application.yml`:**

```yaml
spring:
  data:
    redis:
      host: ${REDIS_HOST:localhost}
      port: ${REDIS_PORT:6379}
      password: ${REDIS_PASSWORD:}
  cache:
    type: redis
    redis:
      time-to-live: 600000       # 10 минут (мс)
      cache-null-values: false   # не кэшировать null
      key-prefix: "myapp:"      # префикс для всех ключей
```

**Java-конфигурация RedisCacheManager (гибкая настройка TTL по кэшам):**

```java
@Configuration
@EnableCaching
public class RedisCacheConfig {

    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        // Конфигурация по умолчанию
        RedisCacheConfiguration defaultConfig = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))
            .disableCachingNullValues()
            .serializeKeysWith(SerializationPair.fromSerializer(new StringRedisSerializer()))
            .serializeValuesWith(SerializationPair.fromSerializer(
                new GenericJackson2JsonRedisSerializer()));

        // Разный TTL для разных кэшей
        Map<String, RedisCacheConfiguration> cacheConfigs = Map.of(
            "users",    defaultConfig.entryTtl(Duration.ofMinutes(30)),
            "products", defaultConfig.entryTtl(Duration.ofHours(1)),
            "sessions", defaultConfig.entryTtl(Duration.ofHours(24))
        );

        return RedisCacheManager.builder(connectionFactory)
            .cacheDefaults(defaultConfig)
            .withInitialCacheConfigurations(cacheConfigs)
            .build();
    }
}
```

**Использование через Spring Cache аннотации:**

```java
@Service
public class ProductService {

    @Cacheable(value = "products", key = "#id")
    public Product findById(Long id) {
        return productRepository.findById(id).orElseThrow();
    }

    @CachePut(value = "products", key = "#result.id")
    public Product update(Product product) {
        return productRepository.save(product);
    }

    @CacheEvict(value = "products", key = "#id")
    public void delete(Long id) {
        productRepository.deleteById(id);
    }
}
```

### Важное
- `GenericJackson2JsonRedisSerializer` — сериализует объекты в JSON (читаемо в Redis CLI)
- Разный TTL для разных кэшей — обязательно; справочники живут часами, пользовательские данные — минутами
- Spring Boot auto-configures `LettuceConnectionFactory` — Lettuce предпочтительнее Jedis (non-blocking, reactive support)

### Частые ошибки
- **JDK сериализация (по умолчанию)** — нечитаемые бинарные данные в Redis; используйте JSON-сериализатор
- **Один TTL для всех кэшей** — справочник товаров и токен авторизации требуют разных TTL
- **Не указать `disableCachingNullValues()`** — null кэшируется и возвращается при следующем запросе
- **Отсутствие key-prefix** — при нескольких приложениях на одном Redis ключи конфликтуют

### Как используется в 2026
- Spring Boot 3.x + Lettuce — стандартный стек
- Redis Sentinel для HA, Redis Cluster для шардирования
- Testcontainers с `GenericContainer<>("redis:7")` для интеграционных тестов

[к оглавлению](#Кэширование)

## Какие стратегии инвалидации кэша существуют?

Инвалидация кэша — процесс удаления или обновления устаревших данных. Это самая сложная часть кэширования.

### 1. TTL (Time-To-Live)

Самая простая стратегия — данные автоматически удаляются через заданное время.

```java
@Cacheable(value = "users", key = "#id") // TTL задаётся в конфигурации CacheManager
public User findById(Long id) { ... }
```

```bash
SET user:1 '{"name":"John"}' EX 600  # удалится через 10 минут
```

**Плюсы:** простота; гарантия, что данные не старше TTL.
**Минусы:** в пределах TTL данные могут быть устаревшими; после TTL — cache miss.

### 2. Явная инвалидация (Event-Driven)

При обновлении данных явно удаляем/обновляем кэш.

```java
@CacheEvict(value = "users", key = "#id")
public void updateUser(Long id, UpdateRequest request) {
    userRepository.save(toEntity(id, request));
}

// Или через события (в микросервисах)
@TransactionalEventListener
public void onUserUpdated(UserUpdatedEvent event) {
    cacheManager.getCache("users").evict(event.getUserId());
}
```

**В микросервисах:** сервис публикует событие в Kafka → потребители инвалидируют свои кэши.

```java
// Сервис A: обновил пользователя → опубликовал событие
@Transactional
public void updateUser(Long id, UpdateRequest request) {
    userRepository.save(toEntity(id, request));
    kafkaTemplate.send("user-events", new UserUpdatedEvent(id));
}

// Сервис B: получил событие → инвалидировал свой кэш
@KafkaListener(topics = "user-events")
public void onUserEvent(UserUpdatedEvent event) {
    cacheManager.getCache("users").evict(event.getUserId());
}
```

### 3. Write-Through / Write-Behind

Кэш обновляется одновременно с БД (Write-Through) или асинхронно (Write-Behind). См. раздел «Паттерны кэширования».

### 4. Version-based (ETag)

Каждая запись имеет версию. При чтении из кэша проверяется, не изменилась ли версия в БД.

```java
// Быстрая проверка версии (один лёгкий запрос к БД)
Long cachedVersion = cache.get("user:" + id + ":version");
Long currentVersion = userRepository.getVersion(id);
if (!currentVersion.equals(cachedVersion)) {
    // Версия изменилась — обновить кэш
    cache.put("user:" + id, userRepository.findById(id));
    cache.put("user:" + id + ":version", currentVersion);
}
```

### 5. Pub/Sub-инвалидация (для распределённых кэшей)

Redis Pub/Sub для уведомления всех экземпляров приложения.

```java
// При обновлении — публикуем в канал
redisTemplate.convertAndSend("cache-invalidation", "users:" + id);

// Все экземпляры слушают канал
@Component
public class CacheInvalidationListener implements MessageListener {
    @Override
    public void onMessage(Message message, byte[] pattern) {
        String key = new String(message.getBody());
        caffeineCache.invalidate(key); // инвалидируем L1 (in-memory)
    }
}
```

### Сравнение стратегий

| Стратегия | Согласованность | Сложность | Применение |
|-----------|----------------|-----------|------------|
| TTL | Eventual (в пределах TTL) | Низкая | Справочники, каталоги |
| Явная инвалидация | Высокая | Средняя | CRUD-операции |
| Event-driven (Kafka) | Eventual | Высокая | Микросервисы |
| Write-Through | Strong | Средняя | Критичные данные |
| Version-based | Strong | Средняя | Когда TTL неприемлем |

### Важное
- **TTL + явная инвалидация** — наиболее распространённая комбинация: TTL как «страховка», `@CacheEvict` при обновлении
- В микросервисах: событийная инвалидация через Kafka/Redis Pub/Sub
- Стратегия «cache aside + TTL + evict on update» покрывает 90% сценариев

### Частые ошибки
- **Обновить БД, забыть инвалидировать кэш** — пользователи видят старые данные
- **Инвалидировать кэш ДО записи в БД** — при ошибке записи кэш пуст, следующий запрос загрузит старые данные из БД
- **Порядок: сначала запись в БД, потом инвалидация кэша** — правильный порядок
- **TTL слишком большой** — 24 часа для профиля пользователя = показывать вчерашнее имя
- **TTL слишком маленький** — 1 секунда для справочника = нет смысла кэшировать

### Как используется в 2026
- TTL + `@CacheEvict` — стандарт в Spring-приложениях
- Kafka-based инвалидация — стандарт в микросервисах
- Redis keyspace notifications — автоматическое уведомление об истечении TTL

[к оглавлению](#Кэширование)

## Что такое многоуровневое кэширование (L1 + L2)?

**Многоуровневое (multi-level) кэширование** — использование нескольких уровней кэша с разной скоростью и ёмкостью. Типичная схема: L1 (in-process, Caffeine) + L2 (distributed, Redis).

```
Запрос → L1 (Caffeine, <1мс) → L2 (Redis, ~1мс) → БД (~5мс)
```

**Реализация в Spring:**

```java
@Configuration
@EnableCaching
public class MultiLevelCacheConfig {

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory redisFactory) {
        // L1 — Caffeine (in-process)
        CaffeineCacheManager caffeineManager = new CaffeineCacheManager();
        caffeineManager.setCaffeine(Caffeine.newBuilder()
            .maximumSize(1_000)
            .expireAfterWrite(Duration.ofMinutes(5))); // короткий TTL для L1

        // L2 — Redis (distributed)
        RedisCacheManager redisManager = RedisCacheManager.builder(redisFactory)
            .cacheDefaults(RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(30))) // длинный TTL для L2
            .build();

        // Композитный CacheManager
        return new CompositeCacheManager(caffeineManager, redisManager);
    }
}
```

**Или кастомная реализация с явным управлением:**

```java
@Service
public class UserService {
    private final LoadingCache<Long, User> l1Cache;  // Caffeine
    private final RedisTemplate<String, User> redis;  // Redis
    private final UserRepository repository;

    public User findById(Long id) {
        return l1Cache.get(id, key -> {
            // L1 miss → проверяем L2
            User fromRedis = redis.opsForValue().get("user:" + key);
            if (fromRedis != null) return fromRedis;

            // L2 miss → БД
            User fromDb = repository.findById(key).orElseThrow();
            redis.opsForValue().set("user:" + key, fromDb, Duration.ofMinutes(30));
            return fromDb;
        });
    }

    public void evict(Long id) {
        l1Cache.invalidate(id);               // инвалидировать L1
        redis.delete("user:" + id);           // инвалидировать L2
        // + Redis Pub/Sub для инвалидации L1 на других экземплярах
    }
}
```

### Важное
- L1 (Caffeine) — <1 мс, ограничен размером heap; L2 (Redis) — ~1 мс, разделяемый между экземплярами
- L1 TTL < L2 TTL — L1 обновляется чаще, чтобы не расходиться с L2
- Инвалидация L1 при обновлении данных — нужен Redis Pub/Sub или Kafka для уведомления всех экземпляров

### Частые ошибки
- **L1 без инвалидации между экземплярами** — экземпляр A обновил данные, экземпляр B отдаёт старые из L1
- **Одинаковый TTL для L1 и L2** — L1 должен быть короче, чтобы периодически «подтягивать» из L2
- **Слишком большой L1** — Caffeine в heap; при 100K объектов × 1KB = 100MB heap

### Как используется в 2026
- Caffeine (L1) + Redis (L2) — стандартная комбинация для high-traffic приложений
- Библиотеки: `caffeine-redis-cache`, `spring-boot-starter-cache` с кастомным `CacheManager`
- В Spring Boot нет multi-level из коробки — нужна кастомная конфигурация или сторонняя библиотека

[к оглавлению](#Кэширование)

## Что такое Cache Stampede и как его предотвратить?

**Cache Stampede (Thundering Herd)** — ситуация, когда TTL «горячего» ключа истекает, и множество одновременных запросов обращаются к БД, вызывая её перегрузку.

```
1. Ключ "popular-product" в кэше (10 000 запросов/сек)
2. TTL истёк → ключ удалён
3. 100 запросов одновременно видят cache miss
4. 100 одинаковых запросов к БД → перегрузка
5. БД медленно отвечает → ещё больше запросов → каскадный сбой
```

**Решения:**

### 1. Locking (единственный запрос обновляет кэш)

```java
public Product getProduct(Long id) {
    Product cached = cache.get("product:" + id);
    if (cached != null) return cached;

    // Только один поток обновляет кэш
    String lockKey = "lock:product:" + id;
    if (redisTemplate.opsForValue().setIfAbsent(lockKey, "1", Duration.ofSeconds(10))) {
        try {
            Product product = productRepository.findById(id).orElseThrow();
            cache.put("product:" + id, product, Duration.ofMinutes(10));
            return product;
        } finally {
            redisTemplate.delete(lockKey);
        }
    } else {
        // Другие потоки ждут или используют stale значение
        Thread.sleep(50);
        return getProduct(id); // retry
    }
}
```

### 2. Probabilistic Early Expiration (XFetch)

Каждый запрос с некоторой вероятностью обновляет кэш до истечения TTL. Чем ближе к истечению — тем выше вероятность.

### 3. Refresh-Ahead (Caffeine)

```java
LoadingCache<Long, Product> cache = Caffeine.newBuilder()
    .expireAfterWrite(Duration.ofMinutes(30))
    .refreshAfterWrite(Duration.ofMinutes(25))  // фоновое обновление за 5 мин до истечения
    .build(id -> productRepository.findById(id).orElseThrow());
```

### 4. Stale-While-Revalidate

Возвращать «протухшее» значение, пока фоновый поток обновляет кэш.

### Важное
- Cache Stampede опасен для «горячих» ключей с высоким RPS
- Locking — самый надёжный, но добавляет сложность
- Caffeine `refreshAfterWrite` — элегантное решение для in-process кэша
- Для Redis — locking через `SETNX` + TTL

### Частые ошибки
- **Игнорировать проблему** — «у нас не бывает» → бывает при распродажах, launch events
- **Lock без TTL** — если поток упал, lock навсегда → все ждут → deadlock
- **Retry без backoff** — 100 потоков спамят retry каждые 50 мс → та же нагрузка

### Как используется в 2026
- Caffeine `refreshAfterWrite` — стандарт для L1
- Redis locking через Redisson — для L2
- CDN (Cloudflare, CloudFront) решают stampede на уровне edge

[к оглавлению](#Кэширование)

## Как работает HTTP-кэширование?

HTTP-кэширование позволяет клиенту (браузер) и промежуточным серверам (CDN, proxy) хранить и переиспользовать ответы без обращения к серверу.

**Заголовки кэширования:**

```http
HTTP/1.1 200 OK
Cache-Control: max-age=3600, public
ETag: "abc123"
Last-Modified: Tue, 22 Apr 2026 10:00:00 GMT
```

| Заголовок | Назначение | Пример |
|-----------|-----------|--------|
| `Cache-Control` | Управление кэшированием | `max-age=3600, public` |
| `ETag` | Хэш содержимого для условных запросов | `"abc123"` |
| `Last-Modified` | Дата последнего изменения | `Tue, 22 Apr 2026 10:00:00 GMT` |
| `Vary` | По каким заголовкам различать кэш | `Vary: Accept-Encoding` |

**Директивы `Cache-Control`:**

| Директива | Значение |
|-----------|---------|
| `public` | Можно кэшировать где угодно (CDN, proxy) |
| `private` | Только в браузере (содержит персональные данные) |
| `no-cache` | Кэшировать, но всегда проверять актуальность (через ETag/Last-Modified) |
| `no-store` | Не кэшировать вообще (sensitive данные) |
| `max-age=N` | Свежесть в секундах |
| `must-revalidate` | После истечения max-age обязательно проверить |

**Условные запросы (conditional requests):**

```
1. Первый запрос:
   GET /api/products/1 → 200 OK, ETag: "abc123"

2. Повторный запрос (с ETag):
   GET /api/products/1
   If-None-Match: "abc123"

3. Если данные не изменились:
   304 Not Modified (тело НЕ передаётся — экономия трафика)

4. Если изменились:
   200 OK, ETag: "def456", новое тело
```

**Реализация в Spring:**

```java
@GetMapping("/products/{id}")
public ResponseEntity<Product> getProduct(@PathVariable Long id) {
    Product product = productService.findById(id);
    String etag = "\"" + product.getVersion() + "\"";

    return ResponseEntity.ok()
        .cacheControl(CacheControl.maxAge(Duration.ofMinutes(10)).cachePublic())
        .eTag(etag)
        .body(product);
}

// Или с Spring ShallowEtagHeaderFilter (автоматический ETag по содержимому)
@Bean
public FilterRegistrationBean<ShallowEtagHeaderFilter> etagFilter() {
    return new FilterRegistrationBean<>(new ShallowEtagHeaderFilter());
}
```

### Важное
- `Cache-Control: no-cache` ≠ «не кэшировать»; это «кэшировать, но проверять актуальность»
- `Cache-Control: no-store` — единственный способ полностью запретить кэширование
- ETag + `If-None-Match` → 304 Not Modified — экономия трафика без потери актуальности
- CDN кэширует только `public` ответы

### Частые ошибки
- **Не устанавливать `Cache-Control`** — браузер может кэшировать по эвристике, или не кэшировать вообще
- **`no-cache` вместо `no-store`** — путаница; `no-cache` всё ещё кэширует
- **Кэшировать персональные данные как `public`** — CDN может отдать чужие данные
- **Забыть `Vary: Authorization`** — ответы для разных пользователей смешиваются в кэше

### Как используется в 2026
- CDN (Cloudflare, CloudFront) — стандарт для статики и API
- `stale-while-revalidate` — директива Cache-Control для фонового обновления
- API Gateway (Spring Cloud Gateway, Nginx) — кэширование на уровне gateway

[к оглавлению](#Кэширование)

## Что такое Caffeine Cache?

**Caffeine** — высокопроизводительная in-process кэш-библиотека для Java (замена Guava Cache). Используется как L1-кэш в Spring-приложениях.

```java
// Создание кэша
Cache<Long, User> cache = Caffeine.newBuilder()
    .maximumSize(10_000)                          // макс. элементов
    .expireAfterWrite(Duration.ofMinutes(10))     // TTL после записи
    .expireAfterAccess(Duration.ofMinutes(5))     // TTL после последнего доступа
    .recordStats()                                 // включить метрики
    .build();

// Ручное использование
cache.put(1L, user);
User cached = cache.getIfPresent(1L);      // null если отсутствует
User loaded = cache.get(1L, id -> loadFromDb(id)); // загрузить при промахе

// LoadingCache — автоматическая загрузка
LoadingCache<Long, User> loadingCache = Caffeine.newBuilder()
    .maximumSize(10_000)
    .expireAfterWrite(Duration.ofMinutes(10))
    .refreshAfterWrite(Duration.ofMinutes(8))     // фоновое обновление
    .build(id -> userRepository.findById(id).orElseThrow());

User user = loadingCache.get(1L); // при промахе — автоматически загрузит из БД
```

**Caffeine как Spring CacheManager:**

```java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public CacheManager cacheManager() {
        CaffeineCacheManager manager = new CaffeineCacheManager();
        manager.setCaffeine(Caffeine.newBuilder()
            .maximumSize(5_000)
            .expireAfterWrite(Duration.ofMinutes(10))
            .recordStats());
        return manager;
    }
}
```

**Мониторинг:**

```java
CacheStats stats = cache.stats();
stats.hitRate();       // процент попаданий
stats.missRate();      // процент промахов
stats.evictionCount(); // количество вытесненных элементов
stats.loadCount();     // количество загрузок
```

### Важное
- Caffeine использует алгоритм **Window TinyLFU** — лучший hit rate среди in-process кэшей (лучше LRU)
- `recordStats()` — обязательно для production; без метрик невозможно оценить эффективность
- `refreshAfterWrite` — ключ к предотвращению cache stampede для горячих данных
- Caffeine — замена Guava Cache и `ConcurrentHashMap`-based кэшей

### Частые ошибки
- **Не ограничить `maximumSize`** — кэш растёт бесконечно → OOM
- **`expireAfterWrite` + `expireAfterAccess` одновременно** — данные удаляются по первому наступившему; обычно достаточно одного
- **Caffeine в микросервисах без L2** — каждый экземпляр имеет свой кэш; при обновлении данные рассинхронизируются

### Как используется в 2026
- Caffeine — стандартный in-process кэш в Spring экосистеме
- Spring Boot: `spring.cache.type=caffeine` — одна строка конфига
- Caffeine + Micrometer — автоматический экспорт метрик в Prometheus/Grafana

[к оглавлению](#Кэширование)

## Как обеспечить согласованность кэша в распределённой системе?

В распределённой системе (несколько экземпляров, микросервисы) согласованность кэша — сложная задача.

**Проблемы:**
- Экземпляр A обновил БД и инвалидировал свой L1-кэш, но экземпляр B отдаёт старые данные из своего L1
- Сервис A обновил пользователя, но сервис B кэширует старую версию
- Race condition: два запроса одновременно обновляют кэш

**Стратегии обеспечения согласованности:**

### 1. Только Redis (без L1)
Самый простой — все читают/пишут в один Redis. Согласованность гарантирована.
**Минус:** каждое чтение — сетевой вызов (~1 мс).

### 2. Redis Pub/Sub для инвалидации L1

```java
// При обновлении — publish в канал
public void updateUser(User user) {
    userRepository.save(user);
    cache.invalidate(user.getId());                    // свой L1
    redis.delete("user:" + user.getId());              // L2
    redis.convertAndSend("cache:invalidate", "user:" + user.getId()); // все L1
}

// Все экземпляры подписаны на канал
@Component
public class CacheInvalidationSubscriber implements MessageListener {
    private final Cache<Long, User> localCache;

    @Override
    public void onMessage(Message message, byte[] pattern) {
        String key = new String(message.getBody()); // "user:42"
        Long id = Long.parseLong(key.split(":")[1]);
        localCache.invalidate(id);
    }
}
```

### 3. Event-driven через Kafka

```java
// Сервис A → Kafka → Сервисы B, C, D
@TransactionalEventListener(phase = AFTER_COMMIT)
public void onUserUpdated(UserUpdatedEvent event) {
    kafkaTemplate.send("user-cache-invalidation", event.getUserId().toString());
}

// Каждый сервис-потребитель
@KafkaListener(topics = "user-cache-invalidation")
public void handleInvalidation(String userId) {
    cacheManager.getCache("users").evict(Long.parseLong(userId));
}
```

### 4. Короткий TTL для L1

Самый простой компромисс: L1 TTL = 30 секунд. Данные обновятся максимум через 30 секунд без дополнительной инфраструктуры.

### Важное
- **Строгая согласованность (strong consistency)** невозможна с кэшем — кэш по определению eventual consistent
- **Приемлемая задержка** — ключевой вопрос: 30 секунд устаревших данных — допустимо? Для каталога — да, для баланса — нет
- Для критичных данных (баланс, инвентарь) — не кэшировать или использовать только Redis с Write-Through

### Частые ошибки
- **Кэшировать мутабельные критичные данные** — баланс счёта, количество товара не должны кэшироваться
- **Redis Pub/Sub без обработки пропусков** — если экземпляр был недоступен, он пропустит сообщение; TTL как fallback обязателен
- **Полагаться только на TTL** — для важных данных TTL 10 минут = до 10 минут stale data

### Как используется в 2026
- Redis Pub/Sub для L1-инвалидации — простой и эффективный подход
- Kafka для межсервисной инвалидации — стандарт в микросервисах
- Короткий L1 TTL (30с-2мин) — pragmatic подход, когда Pub/Sub избыточен

[к оглавлению](#Кэширование)

## Какие политики вытеснения (eviction) существуют?

Когда кэш заполнен и нужно освободить место, алгоритм вытеснения решает, какой элемент удалить.

| Политика | Алгоритм | Плюсы | Минусы |
|----------|----------|-------|--------|
| **LRU** (Least Recently Used) | Удаляет давно не использованный | Простой, хороший hit rate | Не учитывает частоту |
| **LFU** (Least Frequently Used) | Удаляет редко используемый | Учитывает популярность | «Залипание» старых популярных |
| **FIFO** (First In First Out) | Удаляет самый старый | Очень простой | Не учитывает паттерн доступа |
| **Random** | Удаляет случайный | Простой, O(1) | Может удалить горячий элемент |
| **W-TinyLFU** (Caffeine) | LFU с окном + фильтром | Лучший hit rate | Сложная реализация |

**LRU (используется в Redis, LinkedHashMap):**

```java
// Java: LRU-кэш на LinkedHashMap
Map<String, String> lruCache = new LinkedHashMap<>(100, 0.75f, true) {
    @Override
    protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
        return size() > 100; // удалять при превышении 100 элементов
    }
};
```

**Redis eviction policies:**

```
# redis.conf
maxmemory 256mb
maxmemory-policy allkeys-lru   # LRU среди всех ключей
```

| Политика Redis | Описание |
|---------------|----------|
| `noeviction` | Ошибка при нехватке памяти (по умолчанию) |
| `allkeys-lru` | LRU среди всех ключей |
| `volatile-lru` | LRU среди ключей с TTL |
| `allkeys-lfu` | LFU среди всех ключей (Redis 4.0+) |
| `volatile-lfu` | LFU среди ключей с TTL |
| `allkeys-random` | Случайный среди всех |
| `volatile-ttl` | Удалять ключи с ближайшим истечением TTL |

**W-TinyLFU (Caffeine):**

Caffeine использует Window TinyLFU — гибрид LRU и LFU с Bloom-фильтром для отслеживания частоты. Обеспечивает near-optimal hit rate с минимальными затратами памяти.

```java
Caffeine.newBuilder()
    .maximumSize(10_000)  // при превышении — W-TinyLFU решает, что удалить
    .build();
```

### Важное
- **LRU** — хорош для большинства случаев; используется в Redis (`allkeys-lru`)
- **W-TinyLFU** (Caffeine) — лучший hit rate среди in-process кэшей; рекомендуется для L1
- Redis `allkeys-lru` — рекомендуемая политика для кэша; `noeviction` — для persistent storage
- `volatile-*` политики работают только с ключами, у которых установлен TTL

### Частые ошибки
- **Redis `noeviction` для кэша** — при заполнении памяти Redis начнёт отдавать ошибки вместо вытеснения
- **LFU без decay** — старые популярные ключи «залипают» навсегда; Redis LFU использует логарифмический decay
- **Не установить `maxmemory` в Redis** — Redis будет расти до OOM kill ОС

### Как используется в 2026
- Redis: `allkeys-lru` или `allkeys-lfu` — стандартная конфигурация для кэша
- Caffeine: W-TinyLFU по умолчанию — лучший in-process алгоритм
- Мониторинг eviction rate — если высокий, нужно увеличить `maximumSize` или `maxmemory`

[к оглавлению](#Кэширование)

[Вопросы для собеседования](README.md)
