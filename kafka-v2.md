[Вопросы для собеседования](README.md)

# Apache Kafka

+ [Что такое Apache Kafka?](#что-такое-apache-kafka)
+ [Какие основные компоненты Kafka?](#какие-основные-компоненты-kafka)
+ [Как устроена архитектура топика?](#как-устроена-архитектура-топика)
+ [Какие настройки топика Kafka существуют?](#какие-настройки-топика-kafka-существуют)
+ [Как устроена архитектура брокера?](#как-устроена-архитектура-брокера)
+ [Какие настройки брокера Kafka существуют?](#какие-настройки-брокера-kafka-существуют)
+ [Как устроена архитектура продюсера?](#как-устроена-архитектура-продюсера)
+ [Какие настройки продюсера существуют?](#какие-настройки-продюсера-существуют)
+ [Как выглядит пример конфигурации Kafka Producer?](#как-выглядит-пример-конфигурации-kafka-producer)
+ [Как устроена архитектура консюмера?](#как-устроена-архитектура-консюмера)
+ [Какие настройки консюмера существуют?](#какие-настройки-консюмера-существуют)
+ [Как выглядит пример конфигурации Kafka Consumer?](#как-выглядит-пример-конфигурации-kafka-consumer)
+ [Какие основные API предоставляет Kafka?](#какие-основные-api-предоставляет-kafka)
+ [Какова роль Producer API?](#какова-роль-producer-api)
+ [Какова роль Consumer API?](#какова-роль-consumer-api)
+ [Какова роль Connector API?](#какова-роль-connector-api)
+ [Какова роль Streams API?](#какова-роль-streams-api)
+ [Какова роль Transactions API?](#какова-роль-transactions-api)
+ [Какова роль Quota API?](#какова-роль-quota-api)
+ [Какова роль AdminClient API?](#какова-роль-adminclient-api)
+ [Для чего нужен координатор группы?](#для-чего-нужен-координатор-группы)
+ [Для чего нужен Consumer heartbeat thread?](#для-чего-нужен-consumer-heartbeat-thread)
+ [Как Kafka обрабатывает сообщения?](#как-kafka-обрабатывает-сообщения)
+ [Как Kafka обрабатывает задержку консюмера?](#как-kafka-обрабатывает-задержку-консюмера)
+ [Для чего нужны методы subscribe() и poll()?](#для-чего-нужны-методы-subscribe-и-poll)
+ [Для чего нужен метод position()?](#для-чего-нужен-метод-position)
+ [Для чего нужны методы commitSync() и commitAsync()?](#для-чего-нужны-методы-commitsync-и-commitasync)
+ [Для чего нужен идемпотентный продюсер?](#для-чего-нужен-идемпотентный-продюсер)
+ [Для чего нужен интерфейс Partitioner?](#для-чего-нужен-интерфейс-partitioner)
+ [Для чего нужен Broker log cleaner thread?](#для-чего-нужен-broker-log-cleaner-thread)
+ [Для чего нужен Kafka Mirror Maker?](#для-чего-нужен-kafka-mirror-maker)
+ [Для чего нужна Schema Registry?](#для-чего-нужна-schema-registry)
+ [Для чего нужен Streams DSL?](#для-чего-нужен-streams-dsl)
+ [Как Kafka обеспечивает версионирование сообщений?](#как-kafka-обеспечивает-версионирование-сообщений)
+ [Как потребители получают сообщения от брокера?](#как-потребители-получают-сообщения-от-брокера)
+ [В чем разница между Kafka Consumer и Kafka Stream?](#в-чем-разница-между-kafka-consumer-и-kafka-stream)
+ [В чем разница между Kafka Streams и Apache Flink?](#в-чем-разница-между-kafka-streams-и-apache-flink)
+ [В чем разница между Kafka и Flume?](#в-чем-разница-между-kafka-и-flume)
+ [В чем разница между Kafka и RabbitMQ?](#в-чем-разница-между-kafka-и-rabbitmq)

## Что такое Apache Kafka?
<!-- grade: junior -->

Apache Kafka — это распределённая платформа потоковой передачи данных с открытым исходным кодом, разработанная для высокоскоростной обработки больших объёмов данных с минимальной задержкой.

> **Аналогия из жизни:** Kafka — это как конвейерная лента на заводе. Производители кладут детали на ленту, а потребители забирают их в нужном темпе. Лента не останавливается, если один потребитель отстал, и позволяет нескольким участникам работать параллельно.

### Преимущества

- **Персистентность данных** — сообщения хранятся на диске и доступны для повторного чтения
- **Высокая производительность** — миллионы сообщений в секунду с минимальной задержкой
- **Независимость пайплайнов обработки** — потребители работают независимо друг от друга
- **Возможность replay** — можно просмотреть историю записей заново
- **Гибкость** — подходит для разных сценариев: от логирования до event-driven архитектуры

### Когда использовать

- Lambda-архитектура или Kappa-архитектура
- Стриминг больших данных
- Много клиентов (producer и consumer)
- Требуется кратное масштабирование

### Чего в Kafka нет из коробки

- Это не классический брокер сообщений
- Нет отложенных сообщений
- Нет DLQ (Dead Letter Queue) как в RabbitMQ
- Нет поддержки AMQP / MQTT
- Нет TTL на отдельное сообщение
- Нет очередей с приоритетами

> **На собеседовании:** интервьюер ожидает не просто определение, а понимание позиционирования Kafka. Частая ошибка — называть Kafka «брокером сообщений». Kafka — это платформа потоковой передачи данных, которая принципиально отличается от классических брокеров: сообщения не удаляются после прочтения, потребители сами управляют offset, и данные хранятся на диске.

[к оглавлению](#apache-kafka)

---

## Какие основные компоненты Kafka?
<!-- grade: junior -->

Kafka состоит из нескольких ключевых компонентов, которые вместе обеспечивают распределённую потоковую обработку данных.

| Компонент | Описание |
|-----------|----------|
| **Producer** | Приложение, которое публикует сообщения в топики Kafka |
| **Consumer** | Приложение, которое подписывается на топики и читает сообщения |
| **Broker** | Сервер Kafka, который принимает, хранит и распределяет сообщения. В кластере может быть несколько брокеров |
| **Topic** | Логическое разделение, по которому организуются данные. Производители отправляют сообщения в топики, а потребители читают из них |
| **Partition** | Каждый топик разделён на партиции для параллельной обработки. Сообщения в партициях упорядочены |
| **Zookeeper / KRaft** | Сервис координации кластера. В новых версиях Kafka отказывается от Zookeeper в пользу KRaft (Kafka Raft) |

### KRaft (Kafka Raft)

KRaft — это новая внутренняя архитектура метаданных Kafka, которая устраняет зависимость от Zookeeper. Она основана на Raft-консенсусе, позволяя Kafka брокерам самостоятельно управлять метаданными и координировать взаимодействие между собой.

> **На собеседовании:** покажите, что знаете о переходе с Zookeeper на KRaft. Это актуальный тренд — в новых версиях Kafka Zookeeper помечен как deprecated, и KRaft стал production-ready.

[к оглавлению](#apache-kafka)

---

## Как устроена архитектура топика?
<!-- grade: middle -->

Топик — это логическая единица организации данных в Kafka, разбитая на партиции для параллельной обработки.

- **Топик разбит на партиции** — сообщения распределяются по партициям для эффективной параллельной обработки и хранения
- **Партиции хранятся на диске** — Kafka сохраняет данные на диск, что обеспечивает персистентность
- **Партиции делятся на сегменты** — сегмент представляет собой файл на диске; сегменты бывают пассивные и активный. Запись происходит в активный сегмент
- **Данные удаляются по времени или по размеру**. Удаление происходит посегментно, начиная с самого старого сегмента:
  - `retention.bytes` — по максимальному размеру
  - `retention.ms` — по времени
- **Сообщение можно быстро найти по его Offset** — каждому сообщению в партиции присваивается уникальный offset (смещение), по которому его можно мгновенно найти

> **На собеседовании:** важно объяснить связь «топик -> партиция -> сегмент -> файл на диске». Часто спрашивают, почему Kafka так быстра, несмотря на запись на диск — ответ в последовательной записи (sequential I/O) и использовании page cache ОС.

[к оглавлению](#apache-kafka)

---

## Какие настройки топика Kafka существуют?
<!-- grade: middle -->

Настройки топика определяют поведение хранения, репликации и очистки данных.

### Репликация

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `replication.factor` | Количество реплик для каждой партиции | `replication.factor=3` |
| `min.insync.replicas` | Минимальное количество синхронизированных реплик | `min.insync.replicas=2` |

### Хранение данных

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `retention.ms` | Время хранения сообщений в миллисекундах | `retention.ms=604800000` (7 дней) |
| `retention.bytes` | Максимальный объём данных, после чего старые сообщения удаляются | `retention.bytes=10737418240` (10 GB) |
| `segment.bytes` | Размер сегмента логов топика | `segment.bytes=1073741824` (1 GB) |

### Политики очистки

| Настройка | Описание | Значения |
|-----------|----------|----------|
| `cleanup.policy` | Как Kafka обрабатывает старые сообщения | `delete`, `compact` |

### Партиции

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `num.partitions` | Количество партиций в топике | `num.partitions=3` |

> **На собеседовании:** часто спрашивают про связку `replication.factor` + `min.insync.replicas` + `acks=all`. Это базовая конфигурация для надёжной доставки: например, при `replication.factor=3` и `min.insync.replicas=2` система переживёт падение одного брокера без потери данных.

[к оглавлению](#apache-kafka)

---

## Как устроена архитектура брокера?
<!-- grade: middle -->

Брокер — это сервер Kafka, который принимает, хранит и отдаёт сообщения. Кластер Kafka состоит из нескольких брокеров.

- **У каждой партиции свой лидер** — для каждой партиции назначается брокер-лидер, который отвечает за запись и чтение данных
- **Сообщения пишутся в лидера** — производители отправляют сообщения напрямую в брокер-лидер партиции
- **Данные реплицируются между брокерами** — для отказоустойчивости Kafka реплицирует данные партиций на другие брокеры (реплики)
- **Автоматический фейловер лидера** — при сбое брокера-лидера Kafka автоматически назначает нового лидера из числа синхронизированных реплик

> **На собеседовании:** ключевой вопрос — что происходит при падении лидера. Объясните механизм ISR (In-Sync Replicas): только брокеры из ISR-списка могут стать новым лидером, что гарантирует отсутствие потери данных.

[к оглавлению](#apache-kafka)

---

## Какие настройки брокера Kafka существуют?
<!-- grade: middle -->

Настройки брокера определяют поведение репликации, хранения, производительности и безопасности.

### Репликация и консистентность

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `min.insync.replicas` | Минимальное количество синхронизированных реплик для подтверждения записи | `min.insync.replicas=2` |
| `unclean.leader.election.enable` | Разрешает выбор лидера из неактуальных реплик, если нет синхронизированных | `false` |

### Логирование и хранение данных

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `log.dirs` | Директория для хранения логов партиций | `/var/lib/kafka/logs` |
| `log.retention.hours` | Максимальное время хранения данных в логах | `168` (7 дней) |
| `log.segment.bytes` | Максимальный размер сегмента лога | `1073741824` (1 GB) |

### Производительность и задержки

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `num.network.threads` | Количество потоков для обработки сетевых запросов | `3` |
| `num.io.threads` | Количество потоков для ввода-вывода | `8` |
| `socket.send.buffer.bytes` | Размер буфера для отправки данных по сети | `102400` |

### Управление сообщениями

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `message.max.bytes` | Максимальный размер сообщения | `1048576` (1 MB) |
| `replica.fetch.max.bytes` | Максимальный размер данных для запроса реплики | `1048576` (1 MB) |

### Безопасность

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `ssl.keystore.location` | Путь к хранилищу ключей SSL | `/var/private/ssl/kafka.keystore.jks` |
| `ssl.truststore.location` | Путь к хранилищу доверенных сертификатов | `/var/private/ssl/kafka.truststore.jks` |

> **На собеседовании:** обратите внимание на `unclean.leader.election.enable=false` — это критически важная настройка. Если включить её (`true`), то при потере всех ISR-реплик лидером может стать отставший брокер, что приведёт к потере данных.

[к оглавлению](#apache-kafka)

---

## Как устроена архитектура продюсера?
<!-- grade: middle -->

Продюсер — это клиент Kafka, который формирует, буферизирует и отправляет сообщения в топики.

### Этапы работы продюсера

1. **Создание сообщения (Record)** — продюсер формирует сообщение с ключом (необязательным), значением и метаданными (время отправки)
2. **Выбор партиции** — если ключ указан, Kafka использует его хеш для определения партиции (сообщения с одинаковым ключом попадают в одну партицию). Если ключа нет — round-robin или sticky partitioning
3. **Буферизация (Batching)** — продюсер группирует сообщения в пакеты перед отправкой, снижая сетевые задержки
4. **Сжатие (Compression)** — опциональное сжатие через GZIP, Snappy, LZ4 или ZSTD для уменьшения объёма данных
5. **Асинхронная отправка** — сообщения записываются в буфер памяти и отправляются брокеру без ожидания завершения предыдущих операций
6. **Подтверждения (Acknowledgments)** — настраиваемый уровень подтверждений от брокеров (acks=0, 1, all)
7. **Ретрай и идемпотентность** — повторная отправка при сбоях; идемпотентный режим предотвращает дублирование
8. **Error handling** — обработка ошибок через callback

### Резюме

- Продюсер выбирает партицию для сообщения
- Продюсер выбирает уровень гарантии доставки
- В продюсере можно тюнить производительность

> **На собеседовании:** часто спрашивают про связку batching + linger.ms. Объясните, что продюсер не отправляет каждое сообщение отдельно — он накапливает пакет и отправляет его через `linger.ms` миллисекунд или когда пакет достигает `batch.size`. Это ключ к высокой производительности Kafka.

[к оглавлению](#apache-kafka)

---

## Какие настройки продюсера существуют?
<!-- grade: middle -->

Настройки продюсера определяют сериализацию, буферизацию, сжатие, партицирование и гарантии доставки.

### Bootstrap-серверы

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `bootstrap.servers` | Адреса брокеров для подключения и получения метаданных кластера | `localhost:9092,localhost:9093` |

### Сериализация ключа и значения

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `key.serializer` | Сериализатор ключа | `StringSerializer` |
| `value.serializer` | Сериализатор значения | `StringSerializer` |

Варианты сериализаторов: `StringSerializer`, `ByteArraySerializer`, `LongSerializer`, а также пользовательские реализации.

### Отправка сообщений в буфер

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `batch.size` | Размер пакета в байтах перед отправкой брокеру | `16384` (16 KB) |
| `linger.ms` | Максимальное время ожидания перед отправкой пакета | `5` (5 мс) |
| `buffer.memory` | Общий объём памяти для буферизации сообщений | `33554432` (32 MB) |

### Сжатие

| Настройка | Описание | Значения |
|-----------|----------|----------|
| `compression.type` | Тип сжатия для сообщений | `none`, `gzip`, `snappy`, `lz4`, `zstd` |

### Партицирование

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `partitioner.class` | Логика выбора партиции для сообщения | `DefaultPartitioner`, `RoundRobinPartitioner`, `UniformStickyPartitioner` |

### Подтверждения (acks)

| Значение | Описание |
|----------|----------|
| `0` | Продюсер не ждёт подтверждений — максимальная скорость, высокий риск потери |
| `1` | Ждёт подтверждения от лидера партиции |
| `all` (`-1`) | Ждёт подтверждений от всех ISR-реплик — максимальная надёжность |

### Дополнительные настройки

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `retries` | Количество повторных попыток при неудаче | `3` |
| `enable.idempotence` | Включение идемпотентности для exactly-once | `true` |
| `max.request.size` | Максимальный размер сообщения | `1048576` (1 MB) |
| `request.timeout.ms` | Таймаут ожидания подтверждения от брокера | `30000` (30 сек) |

> **На собеседовании:** популярный вопрос — объяснить разницу между `acks=0`, `acks=1` и `acks=all`. Покажите, что понимаете компромисс: `acks=0` — fire-and-forget (потеря возможна), `acks=1` — лидер подтвердил (потеря при падении лидера до репликации), `acks=all` — все ISR подтвердили (потеря практически невозможна, но выше latency).

[к оглавлению](#apache-kafka)

---

## Как выглядит пример конфигурации Kafka Producer?
<!-- grade: middle -->

Рассмотрим три подхода к созданию продюсера: нативный Kafka API, Spring Kafka и Spring Cloud Stream.

### Нативный Kafka Producer

<details><summary>Пример кода</summary>

```java
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import java.util.Properties;

public class KafkaStringArrayProducer {

    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

        KafkaProducer<String, String[]> producer = new KafkaProducer<>(props);

        String key = "user123";
        String[] value = {"message1", "message2", "message3"};

        ProducerRecord<String, String> record = new ProducerRecord<>("my_topic", key, value);
        record.headers().add("traceId", "someTraceId");

        producer.send(record, (metadata, exception) -> {
            if (exception != null) {
                System.out.println("Ошибка при отправке: " + exception.getMessage());
            } else {
                System.out.println("Отправлено в " + metadata.topic() + " партиция " + metadata.partition());
            }
        });

        producer.close();
    }
}
```

```properties
acks=all
retries=3
compression.type=gzip
```

</details>

### С использованием Spring Kafka

<details><summary>Конфигурация и сервис</summary>

```java
@EnableKafka
@Configuration
public class KafkaProducerConfig {

    @Autowired
    private KafkaProperties kafkaProperties;

    @Bean
    public Map<String, Object> producerConfigs() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getServer());
        props.put(ProducerConfig.CLIENT_ID_CONFIG, kafkaProperties.getProducerId());
        props.put(
                ProducerConfig.INTERCEPTOR_CLASSES_CONFIG,
                "com.example.configuration.kafka.KafkaProducerLoggingInterceptor"
        );

        if ("SASL_SSL".equals(kafkaProperties.getSecurityProtocol())) {
            props.put("ssl.truststore.location", kafkaProperties.getSslTrustStoreLocation());
            props.put("ssl.truststore.password", kafkaProperties.getSslTrustStorePassword());
            props.put("ssl.truststore.type", kafkaProperties.getSslTrustStoreType());
            props.put("ssl.keystore.type", kafkaProperties.getSslKeyStoreType());

            props.put("sasl.mechanism", kafkaProperties.getSaslMechanism());
            props.put("security.protocol", kafkaProperties.getSecurityProtocol());
            props.put("sasl.jaas.config", kafkaProperties.getJaasConfigCompiled());
        }

        return props;
    }

    @Bean
    public ProducerFactory<String, String> producerFactory() {
        var stringSerializerKey = new StringSerializer();
        stringSerializerKey.configure(Map.of("key.serializer.encoding", "UTF-8"), true);
        stringSerializerKey.configure(Map.of("serializer.encoding", "UTF-8"), true);

        var stringSerializerValue = new StringSerializer();
        stringSerializerValue.configure(Map.of("value.serializer.encoding", "UTF-8"), false);
        stringSerializerValue.configure(Map.of("serializer.encoding", "UTF-8"), false);

        return new DefaultKafkaProducerFactory<>(producerConfigs(), stringSerializerKey, stringSerializerValue);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
}
```

```java
@Service
public class KafkaProducerService {

    private final KafkaTemplate<String, String> kafkaTemplate;

    public KafkaProducerService(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String message, String key, String topic) {
      try {
        log.info("Sending message {}", data);
        kafkaTemplate.send(topic, key, message);
        log.info("Successfully send message {}", data);
      } catch (Exception ex) {
        log.error("Failed send message to {} topic by key {}", key, topic);
        throw ex;
      }
    }
}
```

```java
@RestController
@RequestMapping("/kafka")
public class KafkaController {

    @Autowired
    private KafkaProducerService kafkaProducerService;

    @PostMapping("/send")
    public String sendMessage(@RequestParam String message, @RequestParam String key, @RequestParam String topic) {
        kafkaProducerService.sendMessage(message, key, topic);
        return "Message sent to Kafka!";
    }
}
```

</details>

### С использованием Spring Cloud Stream

<details><summary>Конфигурация и сервис</summary>

```yaml
spring:
  cloud:
    stream:
      bindings:
        output:
          destination: my_topic
      kafka:
        binder:
          brokers: localhost:9092
```

```java
@Service
@EnableBinding(Source.class)
public class KafkaStreamProducer {

    private final Source source;

    public KafkaStreamProducer(Source source) {
        this.source = source;
    }

    public void sendMessage(String message) {
        Message<String> msg = MessageBuilder.withPayload(message).build();
        source.output().send(msg);
    }
}
```

```java
@RestController
@RequestMapping("/kafka-stream")
public class KafkaStreamController {

    @Autowired
    private KafkaStreamProducer kafkaStreamProducer;

    @PostMapping("/send")
    public String sendMessage(@RequestParam String message) {
        kafkaStreamProducer.sendMessage(message);
        return "Message sent to Kafka via Spring Cloud Stream!";
    }
}
```

</details>

> **На собеседовании:** если вас спрашивают про конфигурацию продюсера, покажите, что знаете как минимум нативный подход и Spring Kafka. Spring Cloud Stream — бонус, который демонстрирует понимание абстракций.

[к оглавлению](#apache-kafka)

---

## Как устроена архитектура консюмера?
<!-- grade: middle -->

Консюмер — это клиент Kafka, который подписывается на топики и читает сообщения, используя Kafka Consumer API. Потребители могут быть объединены в группы (Consumer Groups).

### Ключевые характеристики

- **«Smart» консюмер** — консюмер сам управляет чтением и обработкой
- **Pull-модель** — консюмер опрашивает Kafka, а не получает push
- **Гарантия обработки** — консюмер отвечает за подтверждение обработки (commit offset)
- **Автоматический фейловер** в консюмер-группе
- **Независимая обработка** разными консюмер-группами

### Consumer Group

Kafka использует концепцию Consumer Groups для параллельной обработки данных из топиков. Каждый потребитель в группе обрабатывает только часть данных.

- Все сообщения из топика делятся между потребителями в группе
- Каждая партиция обрабатывается только одним потребителем в группе
- При выходе из строя потребителя его партиции автоматически перераспределяются (rebalancing)

### Offset (Смещение)

Потребитель отслеживает offset каждой партиции, чтобы знать, с какого сообщения продолжать чтение. Offset — это уникальный идентификатор сообщения в партиции. Потребители могут хранить offset в Kafka или вне её (в БД, файловой системе). При отключении потребитель возобновляет обработку с сохранённого offset.

### Poll (Опрос)

Потребители используют метод `poll()` для опроса Kafka на наличие новых сообщений. Потребитель указывает тайм-аут, после которого `poll()` вернёт пустой результат, если сообщений нет.

### Процесс работы

1. **Инициализация** — подключение к брокерам и присоединение к consumer group
2. **Подписка** — вызов `subscribe()` на нужные топики
3. **Опрос** — вызов `poll()` для получения новых сообщений
4. **Обработка** — извлечение и обработка полезной информации
5. **Подтверждение** — вызов `commit()` для обновления offset
6. **Обработка ошибок** — повторные попытки при необходимости
7. **Завершение** — выход из consumer group и закрытие соединения

> **На собеседовании:** ключевой вопрос — разница между auto-commit и manual commit. Auto-commit (`enable.auto.commit=true`) проще, но может привести к потере или дублированию сообщений. Manual commit даёт полный контроль: `commitSync()` — надёжно, но медленно; `commitAsync()` — быстро, но без гарантии.

[к оглавлению](#apache-kafka)

---

## Какие настройки консюмера существуют?
<!-- grade: middle -->

Настройки консюмера определяют подключение, смещения, группы и поведение при чтении.

| Настройка | Описание | Пример |
|-----------|----------|--------|
| `bootstrap.servers` | Список брокеров для подключения | `localhost:9092` |
| `group.id` | Идентификатор группы потребителей | `my-consumer-group` |
| `auto.offset.reset` | Поведение при отсутствии offset | `earliest` / `latest` |
| `enable.auto.commit` | Автоматический коммит offset | `true` / `false` |
| `auto.commit.interval.ms` | Интервал между авто-коммитами | `5000` |
| `max.poll.records` | Максимальное количество сообщений за один `poll()` | `500` |
| `session.timeout.ms` | Таймаут до признания потребителя недоступным | `10000` |
| `client.rack` | Метка серверной стойки или дата-центра | `rack-1` |

### Что такое Rack в контексте Kafka

**Rack** — это метка физического местоположения брокеров. Задаётся через `broker.rack` для управления репликацией данных между разными физическими машинами или дата-центрами.

Преимущества использования `client.rack`:

- **Снижение задержек** — Kafka предпочитает чтение из реплик в том же rack
- **Повышенная отказоустойчивость** — реплики в разных физических местах
- **Лучшее использование ресурсов** — равномерное распределение нагрузки

> **На собеседовании:** частый вопрос — что такое `auto.offset.reset`. `earliest` означает чтение с самого начала (если offset не найден), `latest` — чтение только новых сообщений. Это влияет только на первое подключение группы или при сбросе offset.

[к оглавлению](#apache-kafka)

---

## Как выглядит пример конфигурации Kafka Consumer?
<!-- grade: middle -->

Рассмотрим конфигурацию консюмера с разными гарантиями доставки и через разные фреймворки.

### Нативный Kafka Consumer

<details><summary>Пример кода</summary>

```java
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.common.serialization.StringDeserializer;

import java.time.Duration;
import java.util.HashMap;
import java.util.Map;
import java.util.Collections;

public class KafkaConsumerExample {

    public static void main(String[] args) {
        String bootstrapServers = "localhost:9092";
        String groupId = "my-consumer-group";
        String topic = "my-topic";

        Map<String, Object> consumerConfigs = new HashMap<>();
        consumerConfigs.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        consumerConfigs.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        consumerConfigs.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        consumerConfigs.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        consumerConfigs.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(consumerConfigs);
        consumer.subscribe(Collections.singletonList(topic));

        try {
            while (true) {
                var records = consumer.poll(Duration.ofSeconds(1));
                records.forEach(record -> System.out.println("Received: " + record.value()));
            }
        } finally {
            consumer.close();
        }
    }
}
```

</details>

### Гарантии доставки

| Гарантия | Принцип | Когда коммитить |
|----------|---------|-----------------|
| **At least once** | Сообщение обработано минимум один раз (возможны дубли) | После обработки (`commitAsync()`) |
| **At most once** | Сообщение обработано максимум один раз (возможна потеря) | До обработки или `enable.auto.commit=true` |
| **Mostly once** | Гибрид — обычно один раз, при сбоях возможны дубли | Ручной commit + дедупликация по messageId |

<details><summary>At least once — нативный API</summary>

```java
try {
    while (true) {
        var records = consumer.poll(Duration.ofSeconds(1));
        process(records);
        consumer.commitAsync(); // Commit после обработки
    }
} finally {
    consumer.close();
}
```

</details>

<details><summary>At most once — нативный API</summary>

```java
try {
    while (true) {
        var records = consumer.poll(Duration.ofSeconds(1));
        consumer.commitAsync(); // Commit перед обработкой
        process(records);
    }
} finally {
    consumer.close();
}
```

</details>

### С использованием Spring Kafka

<details><summary>Конфигурация и listener</summary>

```java
@EnableKafka
@Configuration
public class KafkaConsumerConfig {

    @Autowired
    private KafkaProperties kafkaProperties;

    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
        return new DefaultKafkaConsumerFactory<>(consumerConfigs());
    }

    @Bean
    public Map<String, Object> consumerConfigs() {
        Map<String, Object> configs = new HashMap<>();
        configs.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getServer());
        configs.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getConsumerGroupId());
        configs.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        configs.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        return configs;
    }

    @Bean
    public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, String>> kafkaListenerContainerFactory() {
        ConcurrentMessageListenerContainerFactory<String, String> factory = new ConcurrentMessageListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }
}
```

```java
@Service
public class KafkaConsumer {

    @KafkaListener(topics = "my_topic", groupId = "group_id")
    public void listen(@Payload String message,
                       @Header("traceId") String traceId,
                       @Header("correlationId") String correlationId) {
        System.out.println("Received message: " + message);
        System.out.println("Trace ID: " + traceId);
        System.out.println("Correlation ID: " + correlationId);
    }
}
```

</details>

<details><summary>At least once — Spring Kafka</summary>

```yaml
spring:
  kafka:
    consumer:
      enable-auto-commit: false
      auto-offset-reset: earliest
      group-id: my-consumer-group
      max-poll-records: 500
    listener:
      ack-mode: manual
```

```java
@EnableKafka
public class AtLeastOnceConsumer {

    @KafkaListener(topics = "my-topic", groupId = "my-consumer-group")
    public void listen(String message, Acknowledgment acknowledgment) {
        System.out.println("Received message: " + message);
        acknowledgment.acknowledge(); // Ручное подтверждение после обработки
    }
}
```

</details>

<details><summary>At most once — Spring Kafka</summary>

```yaml
spring:
  kafka:
    consumer:
      enable-auto-commit: true
      group-id: my-consumer-group
      auto-offset-reset: earliest
      max-poll-records: 100
```

```java
public class AtMostOnceConsumer {

    @KafkaListener(topics = "my-topic", groupId = "my-consumer-group")
    public void listen(String message) {
        System.out.println("Received message: " + message);
        // Смещение автоматически зафиксировано после получения
    }
}
```

</details>

### С использованием Spring Cloud Stream

<details><summary>Конфигурация и consumer</summary>

```yaml
spring:
  cloud:
    stream:
      bindings:
        input:
          destination: my-topic
          group: my-consumer-group
          content-type: application/json
      kafka:
        binder:
          brokers: localhost:9092
          auto-create-topics: false
```

```java
@Service
@EnableBinding(KafkaProcessor.class)
public class KafkaConsumerService {

    @StreamListener("input")
    public void handle(@Payload String message) {
        System.out.println("Received message: " + message);
    }
}
```

```java
public interface KafkaProcessor {

    @Input("input")
    SubscribableChannel input();
}
```

</details>

<details><summary>Mostly once — Spring Cloud Stream (дедупликация)</summary>

```yaml
spring:
  cloud:
    stream:
      bindings:
        input:
          destination: my-topic
          group: my-consumer-group
          content-type: application/json
          consumer:
            ackMode: manual
            maxAttempts: 3
```

```java
@Component
@EnableBinding(Sink.class)
public class MostlyOnceConsumer {

    private Set<String> processedMessageIds = new HashSet<>();

    @StreamListener(Sink.INPUT)
    public void handleMessage(Message<String> message, @Header("messageId") String messageId) {
        if (processedMessageIds.contains(messageId)) {
            System.out.println("Duplicate message: " + messageId);
            return;
        }
        System.out.println("Received message: " + message.getPayload());
        processedMessageIds.add(messageId);
    }
}
```

</details>

> **На собеседовании:** ключевой вопрос — объяснить разницу между at-least-once и at-most-once. Покажите на уровне кода: at-least-once — commit после обработки (при сбое сообщение будет обработано повторно); at-most-once — commit до обработки (при сбое сообщение потеряется).

[к оглавлению](#apache-kafka)

---

## Какие основные API предоставляет Kafka?
<!-- grade: junior -->

Kafka предоставляет четыре основных API для взаимодействия с платформой.

| API | Назначение |
|-----|-----------|
| **Producer API** | Публикация сообщений в топики |
| **Consumer API** | Чтение сообщений из топиков |
| **Streams API** | Потоковая обработка данных в реальном времени |
| **Connector API** | Интеграция внешних систем (БД, файлы) с Kafka через Kafka Connect |

> **На собеседовании:** достаточно назвать четыре основных API и кратко объяснить назначение каждого. Дополнительные API (Transactions, Quota, AdminClient) — бонус для middle+.

[к оглавлению](#apache-kafka)

---

## Какова роль Producer API?
<!-- grade: junior -->

Producer API используется для публикации потока сообщений в топики Kafka. Он управляет партицированием сообщений, сжатием и балансировкой нагрузки между несколькими брокерами. Продюсер также отвечает за повторные попытки при неудачной публикации и может быть настроен на различные уровни гарантий доставки.

> **На собеседовании:** упомяните, что Producer API — это низкоуровневый клиент. В Spring-приложениях обычно используется `KafkaTemplate`, который оборачивает Producer API и добавляет удобства.

[к оглавлению](#apache-kafka)

---

## Какова роль Consumer API?
<!-- grade: junior -->

Consumer API обеспечивает механизм для чтения сообщений из топиков. Он позволяет приложениям и микросервисам подписываться на топики, читать данные и обрабатывать их — для хранения, анализа или реактивной обработки.

> **На собеседовании:** подчеркните, что Consumer API реализует pull-модель — потребитель сам запрашивает данные, а не получает push от брокера. Это ключевое отличие от классических брокеров сообщений.

[к оглавлению](#apache-kafka)

---

## Какова роль Connector API?
<!-- grade: middle -->

Connector API является частью Kafka Connect — инфраструктуры для интеграции внешних систем с Kafka. Он упрощает подключение различных источников данных (Source Connectors) и систем-приёмников (Sink Connectors), обеспечивая автоматическое перемещение данных между ними без написания кода продюсеров и консюмеров.

> **На собеседовании:** приведите пример: JDBC Source Connector читает изменения из PostgreSQL и отправляет в Kafka, а Elasticsearch Sink Connector пишет данные из Kafka в Elasticsearch. Это CDC (Change Data Capture) без кода.

[к оглавлению](#apache-kafka)

---

## Какова роль Streams API?
<!-- grade: middle -->

Streams API — это компонент Kafka для создания приложений потоковой обработки данных в реальном времени. Он предоставляет высокоуровневый интерфейс для фильтрации, агрегации, объединения данных и вычисления оконных функций. Kafka Streams работает как библиотека внутри JVM-приложения, не требуя отдельного кластера.

> **На собеседовании:** ключевое отличие Streams API от обычного Consumer API — Streams может потреблять, обрабатывать и записывать данные обратно в Kafka в рамках одного приложения, поддерживая exactly-once семантику.

[к оглавлению](#apache-kafka)

---

## Какова роль Transactions API?
<!-- grade: senior -->

Transactions API позволяет выполнять атомарные обновления для нескольких топиков. Он обеспечивает exactly-once гарантию для приложений, которые читают данные из одного топика и пишут в другой. Это критически важно для потоковой обработки, где каждое входное событие должно повлиять на выходные данные ровно один раз, даже в случае сбоев.

> **На собеседовании:** Transactions API — это продвинутая тема. Упомяните, что для включения нужны `enable.idempotence=true` и `transactional.id` у продюсера, а консюмер должен использовать `isolation.level=read_committed`.

[к оглавлению](#apache-kafka)

---

## Какова роль Quota API?
<!-- grade: senior -->

Quota API позволяет настраивать квоты для каждого клиента, ограничивая скорость создания или потребления данных. Это предотвращает ситуацию, когда один клиент потребляет слишком много ресурсов брокера, обеспечивая справедливое распределение ресурсов и защиту от сценариев отказа в обслуживании.

> **На собеседовании:** Quota API — редкий вопрос, но демонстрирует глубокое знание Kafka. Упомяните, что квоты бывают на produce-скорость (bytes/sec), consume-скорость и request rate.

[к оглавлению](#apache-kafka)

---

## Какова роль AdminClient API?
<!-- grade: middle -->

AdminClient API предоставляет программный интерфейс для управления топиками, брокерами, конфигурацией и другими объектами Kafka. Его можно использовать для создания, удаления и описания топиков, управления ACL, получения информации о кластере и выполнения административных задач без CLI.

> **На собеседовании:** AdminClient API полезен для автоматизации. Например, в микросервисной архитектуре сервис может при старте проверить наличие нужного топика и создать его программно, если он отсутствует.

[к оглавлению](#apache-kafka)

---

## Для чего нужен координатор группы?
<!-- grade: middle -->

Координатор группы (Group Coordinator) — это компонент Kafka, который управляет жизненным циклом Consumer Group: членством, назначением партиций и фиксацией смещений.

Когда потребитель присоединяется к группе или покидает её, координатор запускает перебалансировку (rebalancing) для переназначения партиций среди оставшихся потребителей.

> **На собеседовании:** часто спрашивают, что происходит при rebalancing. Во время перебалансировки все потребители в группе приостанавливают обработку — это «stop-the-world» пауза. Поэтому важно минимизировать частоту rebalancing через правильную настройку `session.timeout.ms` и `heartbeat.interval.ms`.

[к оглавлению](#apache-kafka)

---

## Для чего нужен Consumer heartbeat thread?
<!-- grade: middle -->

Consumer Heartbeat Thread — это фоновый поток, который периодически отправляет сигналы (heartbeats) координатору группы, подтверждая, что потребитель жив и является частью группы.

Если потребитель не отправляет heartbeat в течение `session.timeout.ms`, координатор считает его мёртвым и инициирует перебалансировку для переназначения его партиций другим потребителям.

> **На собеседовании:** важно знать связку двух таймаутов: `session.timeout.ms` — сколько ждать без heartbeat до объявления потребителя мёртвым; `heartbeat.interval.ms` — как часто отправлять heartbeat (обычно 1/3 от session.timeout.ms).

[к оглавлению](#apache-kafka)

---

## Как Kafka обрабатывает сообщения?
<!-- grade: junior -->

Kafka поддерживает два основных способа обработки сообщений, определяемых конфигурацией Consumer Groups.

| Модель | Описание | Как достигается |
|--------|----------|-----------------|
| **Queue** | Каждое сообщение обрабатывается одним потребителем | Несколько потребителей в одной группе, каждый читает из отдельных партиций |
| **Publish-Subscribe** | Все сообщения обрабатываются всеми потребителями | Каждый потребитель в своей отдельной группе |

> **На собеседовании:** Kafka не имеет отдельных режимов Queue и Pub-Sub — обе модели реализуются через Consumer Groups. Это элегантное решение: одна группа = очередь, разные группы = pub-sub.

[к оглавлению](#apache-kafka)

---

## Как Kafka обрабатывает задержку консюмера?
<!-- grade: middle -->

Задержка (лаг) консюмера — это разница между offset последнего созданного сообщения и offset последнего прочитанного сообщения. Лаг показывает, насколько потребитель отстаёт от потока данных.

Kafka предоставляет инструменты для мониторинга лага:
- Инструмент командной строки `kafka-consumer-groups`
- AdminClient API
- JMX-метрики

Kafka не обрабатывает задержки автоматически, но предоставляет метрики для принятия решений о масштабировании или оптимизации.

> **На собеседовании:** высокий consumer lag — это красный флаг. Решения: добавить потребителей в группу (до количества партиций), увеличить `max.poll.records`, оптимизировать обработку сообщений, или добавить партиции в топик.

[к оглавлению](#apache-kafka)

---

## Для чего нужны методы subscribe() и poll()?
<!-- grade: junior -->

`subscribe()` — подписка потребителя на один или несколько топиков. Этот метод не извлекает данные, а только регистрирует интерес.

`poll()` — извлечение данных из подписанных топиков. Возвращает записи, опубликованные с момента последнего запроса. Обычно вызывается в бесконечном цикле для непрерывного получения данных.

> **На собеседовании:** `poll()` — это не только чтение данных. Внутри poll происходит: отправка heartbeat, участие в rebalancing, авто-коммит offset (если включён). Поэтому важно вызывать `poll()` регулярно — долгая пауза приведёт к тому, что координатор считает потребителя мёртвым.

[к оглавлению](#apache-kafka)

---

## Для чего нужен метод position()?
<!-- grade: middle -->

Метод `position()` возвращает offset следующей записи, которая будет извлечена для данной партиции. Это полезно для отслеживания прогресса чтения.

В сочетании с методом `committed()` можно определить, насколько потребитель отстал от своего последнего закоммиченного offset, что ценно для мониторинга производительности.

> **На собеседовании:** `position()` показывает текущую позицию чтения, а `committed()` — последний закоммиченный offset. Разница между ними — это «незакоммиченный прогресс», который будет потерян при рестарте потребителя.

[к оглавлению](#apache-kafka)

---

## Для чего нужны методы commitSync() и commitAsync()?
<!-- grade: middle -->

Эти методы используются для фиксации (commit) смещений потребителя.

| Метод | Поведение | Плюсы | Минусы |
|-------|-----------|-------|--------|
| `commitSync()` | Синхронная фиксация; повторяет попытки до успеха или неисправимой ошибки | Надёжный — offset точно зафиксирован | Блокирует поток, снижает throughput |
| `commitAsync()` | Асинхронная фиксация; не повторяет попытку при сбое | Быстрый, не блокирует | Менее надёжный — offset может не зафиксироваться |

> **На собеседовании:** распространённый паттерн — использовать `commitAsync()` в основном цикле (для скорости) и `commitSync()` в блоке `finally` перед закрытием потребителя (для надёжности последнего коммита).

[к оглавлению](#apache-kafka)

---

## Для чего нужен идемпотентный продюсер?
<!-- grade: middle -->

Идемпотентный продюсер гарантирует exactly-once доставку на уровне одной партиции, предотвращая дублирование записей при повторных попытках отправки. Включается через `enable.idempotence=true`.

Каждому продюсеру присваивается Producer ID (PID), а каждому сообщению — sequence number. Брокер отклоняет дубликаты с тем же PID и sequence number.

> **На собеседовании:** идемпотентный продюсер решает проблему дубликатов при ретраях, но только в пределах одной партиции и одной сессии продюсера. Для exactly-once между топиками нужен Transactions API.

[к оглавлению](#apache-kafka)

---

## Для чего нужен интерфейс Partitioner?
<!-- grade: middle -->

Интерфейс `Partitioner` в Producer API определяет, в какую партицию топика будет отправлено сообщение.

Partitioner по умолчанию (`DefaultPartitioner`) использует хеш ключа для выбора партиции, гарантируя, что сообщения с одинаковым ключом попадают в одну партицию. Можно реализовать пользовательский Partitioner для распределения сообщений на основе бизнес-логики.

> **На собеседовании:** типичный пример кастомного Partitioner — маршрутизация сообщений по регионам или по приоритету. Важно помнить: порядок сообщений гарантируется только в пределах одной партиции, поэтому выбор партиции напрямую влияет на гарантии порядка.

[к оглавлению](#apache-kafka)

---

## Для чего нужен Broker log cleaner thread?
<!-- grade: senior -->

Log Cleaner Thread — это фоновый поток в брокере, выполняющий сжатие журнала (log compaction). При сжатии Kafka удаляет устаревшие записи, сохраняя только последнее значение для каждого ключа.

Это полезно, когда нужно хранить только актуальное состояние: changelog, snapshot БД, конфигурации. Log Cleaner периодически запускается для сжатия партиций с `cleanup.policy=compact`.

> **На собеседовании:** log compaction — это не удаление по времени (retention), а сохранение последнего значения по ключу. Классический кейс — хранение последнего состояния сущности (например, последний адрес пользователя).

[к оглавлению](#apache-kafka)

---

## Для чего нужен Kafka Mirror Maker?
<!-- grade: senior -->

Kafka Mirror Maker — это инструмент для репликации данных между кластерами Kafka, потенциально расположенными в разных дата-центрах. Он работает как консюмер в исходном кластере и продюсер в целевом.

Сценарии использования:
- Создание резервной копии данных (disaster recovery)
- Объединение данных из нескольких дата-центров
- Миграция данных между кластерами

> **На собеседовании:** Mirror Maker 2 (MM2), основанный на Kafka Connect, значительно улучшил оригинальный Mirror Maker: поддержка exactly-once, автоматическая синхронизация конфигурации топиков и consumer group offsets.

[к оглавлению](#apache-kafka)

---

## Для чего нужна Schema Registry?
<!-- grade: middle -->

Schema Registry — это сервис (часть экосистемы Confluent), который предоставляет RESTful интерфейс для хранения и извлечения схем данных (Avro, Protobuf, JSON Schema). Он обеспечивает совместимость схем между производителями и потребителями.

Schema Registry позволяет эволюционировать модели данных со временем, сохраняя обратную и прямую совместимость, что критически важно для независимого развёртывания микросервисов.

> **На собеседовании:** без Schema Registry продюсер может изменить формат сообщения и сломать всех консюмеров. Registry проверяет совместимость при регистрации новой версии схемы — backward (новый код читает старые данные), forward (старый код читает новые данные) или full (оба направления).

[к оглавлению](#apache-kafka)

---

## Для чего нужен Streams DSL?
<!-- grade: middle -->

Kafka Streams DSL — это высокоуровневый API для операций потоковой обработки данных. Он позволяет описывать логику фильтрации, преобразования, агрегации и объединения потоков данных, абстрагируя низкоуровневые детали обработки.

DSL оперирует абстракциями `KStream` (поток событий) и `KTable` (changelog-таблица), что позволяет строить сложные топологии обработки декларативно.

> **На собеседовании:** покажите понимание разницы между `KStream` и `KTable`. KStream — это неограниченный поток событий (каждая запись — факт). KTable — это материализованное представление последнего значения по ключу (как таблица в БД).

[к оглавлению](#apache-kafka)

---

## Как Kafka обеспечивает версионирование сообщений?
<!-- grade: middle -->

Kafka не обеспечивает версионирование сообщений напрямую, но предоставляет механизмы для его реализации.

Основные подходы:
- **Поле версии в схеме** — включение номера версии в структуру сообщения
- **Schema Registry** — управление версиями схем с проверкой совместимости через Confluent Schema Registry
- **Заголовки сообщений** — передача метаинформации о версии через Kafka Headers

> **На собеседовании:** на практике Schema Registry — стандартный способ управления версиями. Без неё эволюция схемы превращается в координацию между командами, что ненадёжно.

[к оглавлению](#apache-kafka)

---

## Как потребители получают сообщения от брокера?
<!-- grade: junior -->

Kafka использует pull-модель для доставки сообщений. Потребители сами запрашивают данные у брокеров через `poll()`, а не получают push от брокера.

Потребитель отправляет запрос, указывая топик, партицию и начальное смещение. Брокер отвечает сообщениями с объёмом до указанного максимального предела в байтах.

> **На собеседовании:** pull-модель — это осознанный выбор архитектуры Kafka. Преимущества: потребитель контролирует скорость чтения, не перегружается при пиках, может перечитать данные. Недостаток: при отсутствии данных потребитель тратит ресурсы на пустые poll (решается через long polling с таймаутом).

[к оглавлению](#apache-kafka)

---

## В чем разница между Kafka Consumer и Kafka Stream?
<!-- grade: middle -->

Kafka Consumer и Kafka Streams — это два способа обработки данных из Kafka с разным уровнем абстракции.

| Критерий | Kafka Consumer | Kafka Streams |
|----------|---------------|---------------|
| **Уровень** | Низкоуровневый клиент | Высокоуровневая библиотека |
| **Назначение** | Чтение данных из топиков | Чтение, обработка и запись обратно в Kafka |
| **Операции** | Простое получение сообщений | Фильтрация, агрегация, join, windowing |
| **Запись обратно** | Требует отдельного продюсера | Встроенная поддержка |
| **Exactly-once** | Нужна ручная реализация | Встроенная поддержка |
| **Состояние** | Без состояния | State stores (RocksDB) |

> **На собеседовании:** если задача — просто прочитать сообщение и отправить в БД, достаточно Consumer API. Если нужно обрабатывать поток (агрегация, join, windowing) и писать результат обратно в Kafka — используйте Kafka Streams.

[к оглавлению](#apache-kafka)

---

## В чем разница между Kafka Streams и Apache Flink?
<!-- grade: senior -->

Kafka Streams и Apache Flink — это инструменты потоковой обработки данных с принципиально разной архитектурой.

| Критерий | Kafka Streams | Apache Flink |
|----------|--------------|--------------|
| **Архитектура** | Библиотека внутри JVM-приложения | Отдельный распределённый кластер |
| **Зависимость от Kafka** | Только Kafka | Любые источники (Kafka, HDFS, БД) |
| **Развёртывание** | Не требует отдельного кластера | Требует кластер Flink |
| **Обработка** | Только потоковая | Потоковая + пакетная (batch) |
| **Управление состоянием** | RocksDB, репликация | Развитая система с checkpointing |
| **Гарантия доставки** | At-least-once, exactly-once | At-least-once, at-most-once, exactly-once |
| **Масштабирование** | По партициям Kafka | По задачам (tasks) — гибче |
| **Ресурсоёмкость** | Низкая (внутри JVM) | Высокая (отдельный кластер) |

### Когда выбрать Kafka Streams

- Уже используете Kafka и нужна легковесная обработка
- Низкая задержка, данные приходят из Kafka
- Нужно встроить обработку в существующее Java-приложение

### Когда выбрать Apache Flink

- Источники данных не ограничены Kafka
- Сложные задачи: windowing, аналитика, восстановление после сбоев
- Нужна пакетная + потоковая обработка

> **На собеседовании:** не говорите, что один инструмент лучше другого. Покажите понимание компромиссов: Kafka Streams — проще, дешевле, но привязан к Kafka. Flink — мощнее, гибче, но сложнее в эксплуатации.

[к оглавлению](#apache-kafka)

---

## В чем разница между Kafka и Flume?
<!-- grade: middle -->

Apache Kafka и Apache Flume — это инструменты для передачи данных с разными целями и архитектурами.

| Критерий | Kafka | Flume |
|----------|-------|-------|
| **Назначение** | Платформа потоковой передачи данных | Сбор, агрегация и передача логов |
| **Архитектура** | Топики + партиции | Sources + Channels + Sinks |
| **Хранение** | Персистентное (на диске, до retention) | Транзитное (передаёт в HDFS/HBase) |
| **Производительность** | Миллионы сообщений в секунду | Ниже, ориентирован на логи |
| **Потребительская модель** | Много независимых потребителей, replay | Фиксированная схема доставки |
| **Гарантия доставки** | At-least-once, exactly-once | Базовые гарантии |
| **Экосистема** | Широкая интеграция | Hadoop-экосистема (HDFS, HBase) |
| **Кейсы** | Стриминг, event-driven архитектура, аналитика | Сбор логов, передача в HDFS |

> **На собеседовании:** Flume — это нишевый инструмент для Hadoop-экосистемы, в то время как Kafka — универсальная платформа. В современных архитектурах Kafka практически полностью заменила Flume, поскольку Kafka может делать всё то же самое и больше.

[к оглавлению](#apache-kafka)

---

## В чем разница между Kafka и RabbitMQ?
<!-- grade: middle -->

Kafka и RabbitMQ — это две фундаментально разные системы обмена сообщениями с разными моделями доставки.

| Критерий | Kafka | RabbitMQ |
|----------|-------|----------|
| **Модель** | Распределённый лог (append-only) | Классический брокер сообщений (очереди) |
| **Хранение** | Сообщения хранятся на диске до retention | Сообщения удаляются после обработки |
| **Повторное чтение** | Да (replay по offset) | Нет (сообщение удаляется) |
| **Потребители** | Pull-модель, независимые consumer groups | Push-модель, один потребитель на сообщение |
| **Производительность** | Миллионы msg/sec, оптимизирована для throughput | Десятки тысяч msg/sec, оптимизирована для latency |
| **Масштабирование** | Горизонтальное через партиции | Горизонтальное, но сложнее |
| **Гарантия доставки** | At-least-once, exactly-once | At-least-once, at-most-once |
| **Маршрутизация** | По ключу в партицию | Гибкая (exchange, routing key, bindings) |
| **Протоколы** | Собственный протокол | AMQP, MQTT, STOMP |

### Когда выбрать Kafka

- Обработка потоков данных в реальном времени
- Интеграция с Big Data
- Журналирование событий, event sourcing
- Нужен replay и долгосрочное хранение

### Когда выбрать RabbitMQ

- Классические задачи очередей задач
- Микросервисы с request-reply паттерном
- Нужна гибкая маршрутизация (routing, topic exchange)
- Нужны приоритетные очереди, TTL, DLQ

> **На собеседовании:** главное различие — Kafka хранит сообщения и позволяет перечитывать (лог), а RabbitMQ удаляет после обработки (очередь). Kafka оптимизирована для высокого throughput, RabbitMQ — для гибкой маршрутизации и низкой latency для отдельных сообщений.

[к оглавлению](#apache-kafka)
